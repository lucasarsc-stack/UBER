<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Painel de Metas - Uber</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: #ffffff;
            color: #000000;
            max-width: 480px;
            margin: 0 auto;
            padding: 20px;
            box-sizing: border-box;
        }

        .uber-header {
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 25px;
            border-bottom: 2px solid #000000;
            padding-bottom: 15px;
        }

        /* Estilização inspirada no logotipo original da Uber: Minimalista, caixa alta e espaçada */
        .uber-logo {
            font-size: 32px;
            font-weight: 900;
            letter-spacing: 8px;
            text-transform: uppercase;
            color: #000000;
        }

        .meta-container {
            background-color: #f4f4f5;
            border: 2px solid #000000;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
        }

        .meta-container label {
            font-size: 14px;
            font-weight: bold;
            display: block;
            margin-bottom: 5px;
        }

        .meta-container input {
            width: 100%;
            padding: 10px;
            font-size: 16px;
            border: 1px solid #000000;
            border-radius: 4px;
            box-sizing: border-box;
            background-color: #ffffff;
            color: #000000;
        }

        .dias-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 10px;
            margin-bottom: 20px;
        }

        .dia-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: #fafafa;
            border: 1px solid #d1d5db;
            padding: 10px 12px;
            border-radius: 6px;
        }

        .dia-item label {
            font-weight: 600;
            font-size: 14px;
            flex: 1;
        }

        .dia-item input {
            width: 120px;
            padding: 8px;
            font-size: 15px;
            border: 1px solid #000000;
            border-radius: 4px;
            text-align: right;
            background-color: #ffffff;
            color: #000000;
        }

        .resumo-card {
            border: 2px solid #000000;
            padding: 15px;
            border-radius: 8px;
            background-color: #ffffff;
            margin-top: 15px;
        }

        .resumo-card h3 {
            margin: 0 0 10px 0;
            font-size: 16px;
            text-transform: uppercase;
            border-bottom: 1px solid #e5e7eb;
            padding-bottom: 5px;
        }

        .resumo-linha {
            display: flex;
            justify-content: space-between;
            margin: 8px 0;
            font-size: 15px;
        }

        .status-positivo {
            color: #16a34a;
            font-weight: bold;
        }

        .status-negativo {
            color: #dc2626;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div class="uber-header">
        <div class="uber-logo">UBER</div>
    </div>

    <div class="meta-container">
        <label for="metaSemanal">Meta Semanal (R$):</label>
        <input type="number" id="metaSemanal" value="400" oninput="calcular()">
    </div>

    <div class="dias-grid">
        <div class="dia-item">
            <label>Segunda-feira:</label>
            <input type="number" class="dia-input" placeholder="0,00" oninput="calcular()">
        </div>
        <div class="dia-item">
            <label>Terça-feira:</label>
            <input type="number" class="dia-input" placeholder="0,00" oninput="calcular()">
        </div>
        <div class="dia-item">
            <label>Quarta-feira:</label>
            <input type="number" class="dia-input" placeholder="0,00" oninput="calcular()">
        </div>
        <div class="dia-item">
            <label>Quinta-feira:</label>
            <input type="number" class="dia-input" placeholder="0,00" oninput="calcular()">
        </div>
        <div class="dia-item">
            <label>Sexta-feira:</label>
            <input type="number" class="dia-input" placeholder="0,00" oninput="calcular()">
        </div>
        <div class="dia-item">
            <label>Sábado:</label>
            <input type="number" class="dia-input" placeholder="0,00" oninput="calcular()">
        </div>
        <div class="dia-item">
            <label>Domingo:</label>
            <input type="number" class="dia-input" placeholder="0,00" oninput="calcular()">
        </div>
    </div>

    <div class="resumo-card">
        <h3>Balanço da Semana</h3>
        <div class="resumo-linha">
            <span>Total Feito:</span>
            <strong id="totalFeito">R$ 0,00</strong>
        </div>
        <div class="resumo-linha">
            <span>Falta para a Meta:</span>
            <strong id="faltaMeta">R$ 400,00</strong>
        </div>
        <div class="resumo-linha">
            <span>Status:</span>
            <span id="statusMeta" class="status-negativo">Abaixo da meta</span>
        </div>
    </div>

    <div class="resumo-card" style="margin-top: 15px;">
        <h3>Projeção Mensal (Aprox. 4 Semanas)</h3>
        <div class="resumo-linha">
            <span>Total Estimado no Mês:</span>
            <strong id="totalMes">R$ 0,00</strong>
        </div>
    </div>

    <script>
        function calcular() {
            let meta = parseFloat(document.getElementById('metaSemanal').value) || 0;
            let inputs = document.getElementsByClassName('dia-input');
            let total = 0;

            for (let i = 0; i < inputs.length; i++) {
                total += parseFloat(inputs[i].value) || 0;
            }

            let falta = meta - total;
            let totalMes = total * 4; // Média de 4 semanas

            document.getElementById('totalFeito').innerText = 'R$ ' + total.toFixed(2);
            
            let elementoFalta = document.getElementById('faltaMeta');
            let elementoStatus = document.getElementById('statusMeta');

            if (falta <= 0) {
                elementoFalta.innerText = 'R$ 0,00 (Meta Batida!)';
                elementoStatus.innerText = 'POSITIVO 🚀';
                elementoStatus.className = 'status-positivo';
            } else {
                elementoFalta.innerText = 'R$ ' + falta.toFixed(2);
                elementoStatus.innerText = 'Em andamento...';
                elementoStatus.className = 'status-negativo';
            }

            document.getElementById('totalMes').innerText = 'R$ ' + totalMes.toFixed(2);
        }

        // Executa ao carregar para alinhar os valores iniciais
        calcular();
    </script>

</body>
</html>
