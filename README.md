<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Compras da Família</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 20px; background-color: #f4f4f9; color: #333; }
        .container { max-width: 500px; margin: 0 auto; background: white; padding: 20px; border-radius: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        h2 { text-align: center; color: #2c3e50; margin-top: 0; }
        .total-box { background: #2ecc71; color: white; text-align: center; padding: 15px; border-radius: 10px; font-size: 24px; font-weight: bold; margin-bottom: 20px; }
        .input-row { display: flex; gap: 10px; margin-bottom: 10px; flex-wrap: wrap; }
        input { padding: 12px; border: 1px solid #ccc; border-radius: 8px; font-size: 16px; flex: 1; min-width: 100px; }
        .btn { padding: 12px; border: none; border-radius: 8px; font-size: 16px; cursor: pointer; font-weight: bold; }
        .btn-add { background: #3498db; color: white; flex: 1; min-width: 120px; }
        .btn-voice { background: #e67e22; color: white; }
        .lista { list-style: none; padding: 0; margin-top: 20px; }
        .item-loja { display: flex; justify-content: space-between; align-items: center; padding: 12px; border-bottom: 1px solid #eee; font-size: 18px; }
        .item-loja.comprado { text-decoration: line-through; color: #95a5a6; background: #f9f9f9; }
        .btn-del { background: #e74c3c; color: white; padding: 6px 12px; font-size: 14px; }
        .checkbox { width: 22px; height: 22px; margin-right: 10px; cursor: pointer; }
        .item-info { display: flex; align-items: center; flex: 1; }
    </style>
</head>
<body>

<div class="container">
    <h2>🛒 Lista de Compras</h2>
    <div class="total-box">Total: R$ <span id="valorTotal">0,00</span></div>
    
    <div class="input-row">
        <input type="text" id="produto" placeholder="Produto (Ex: Arroz)">
        <button class="btn btn-voice" id="btnVoz" onclick="ouvirVoz()">🎤 Voz</button>
    </div>
    <div class="input-row">
        <input type="number" id="qtd" placeholder="Qtd" value="1">
        <input type="number" id="preco" placeholder="Preço R$" step="0.01">
        <button class="btn btn-add" onclick="adicionarItem()">+ Adicionar</button>
    </div>

    <ul class="lista" id="listaProdutos"></ul>
</div>

<script>
    let itens = [];

    function adicionarItem() {
        let nome = document.getElementById('produto').value.trim();
        let qtd = parseInt(document.getElementById('qtd').value) || 1;
        let preco = parseFloat(document.getElementById('preco').value) || 0;

        if (nome === "") { alert("Por favor, digite o nome do produto!"); return; }

        let item = { id: Date.now(), nome: nome, qtd: qtd, preco: preco, comprado: false };
        itens.push(item);
        atualizarTela();

        document.getElementById('produto').value = "";
        document.getElementById('preco').value = "";
        document.getElementById('qtd').value = "1";
    </script>
</body>
</html>
