<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Compras Família</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 15px; background-color: #f4f7f6; }
        .container { max-width: 500px; margin: 0 auto; background: white; padding: 20px; border-radius: 15px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        h2 { text-align: center; color: #333; margin-top: 0; }
        .total-box { background: #2ecc71; color: white; text-align: center; padding: 15px; border-radius: 10px; font-size: 24px; font-weight: bold; margin-bottom: 20px; }
        .input-row { display: flex; gap: 8px; margin-bottom: 10px; }
        input { padding: 12px; border: 1px solid #ddd; border-radius: 8px; font-size: 16px; flex: 1; min-width: 0; }
        .btn { padding: 12px; border: none; border-radius: 8px; font-size: 16px; cursor: pointer; font-weight: bold; }
        .btn-add { background: #3498db; color: white; }
        .btn-voice { background: #e67e22; color: white; width: 50px; padding: 0; display: flex; align-items: center; justify-content: center; font-size: 20px; }
        .lista { list-style: none; padding: 0; margin-top: 20px; }
        .item-loja { display: flex; justify-content: space-between; align-items: center; padding: 12px; border-bottom: 1px solid #eee; }
        .item-loja.comprado { text-decoration: line-through; color: #95a5a6; }
        .btn-del { background: #e74c3c; color: white; padding: 6px 10px; border-radius: 5px; font-size: 14px; }
        .item-info { display: flex; align-items: center; gap: 10px; font-size: 18px; }
        .checkbox { width: 20px; height: 20px; }
    </style>
</head>
<body>

<div class="container">
    <h2>🛒 Lista de Compras</h2>
    <div class="total-box">Total: R$ <span id="valorTotal">0,00</span></div>
    
    <div class="input-row">
        <input type="text" id="produto" placeholder="Produto (ex: Arroz)">
        <button class="btn btn-voice" onclick="ouvirVoz()">🎤</button>
    </div>
    <div class="input-row">
        <input type="number" id="qtd" placeholder="Qtd" value="1" style="max-width: 60px;">
        <input type="number" id="preco" placeholder="Preço R$" step="0.01">
        <button class="btn btn-add" onclick="adicionarItem()">+ Add</button>
    </div>

    <ul class="lista" id="listaProdutos"></ul>
</div>

<script>
    let itens = [];

    function adicionarItem() {
        let nome = document.getElementById('produto').value.trim();
        let qtd = parseInt(document.getElementById('qtd').value) || 1;
        let preco = parseFloat(document.getElementById('preco').value) || 0;

        if (nome === "") return alert("Digite o nome do produto!");

        itens.push({ id: Date.now(), nome, qtd, preco, comprado: false });
        atualizarTela();

        document.getElementById('produto').value = "";
        document.getElementById('preco').value = "";
        document.getElementById('qtd').value = "1";
    }

    function alternarComprado(id) {
        let item = itens.find(i => i.id === id);
        if (item) item.comprado = !item.comprado;
        atualizarTela();
    }

    function deletarItem(id) {
        itens = itens.filter(i => i.id !== id);
        atualizarTela();
    }

    function atualizarTela() {
        let lista = document.getElementById('listaProdutos');
        lista.innerHTML = "";
        let total = 0;

        itens.forEach(item => {
            let subtotal = item.qtd * item.preco;
            total += subtotal;

            let li = document.createElement('li');
            li.className = `item-loja ${item.comprado ? 'comprado' : ''}`;
            li.innerHTML = `
                <div class="item-info">
                    <input type="checkbox" class="checkbox" ${item.comprado ? 'checked' : ''} onclick="alternarComprado(${item.id})">
                    <span>${item.nome} (${item.qtd}x R$ ${item.preco.toFixed(2)})</span>
                </div>
                <button class="btn btn-del" onclick="deletarItem(${item.id})">X</button>
            `;
            lista.appendChild(li);
        });

        document.getElementById('valorTotal').innerText = total.toFixed(2).replace('.', ',');
    }

    function ouvirVoz() {
        window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        if (!window.SpeechRecognition) return alert("Seu celular não suporta comando de voz no navegador.");
        
        let recognition = new SpeechRecognition();
        recognition.lang = 'pt-BR';
        recognition.start();

        recognition.onresult = function(event) {
            let texto = event.results.transcript;
            document.getElementById('produto').value = texto;
        };
    }
</script>

</body>
</html> 
