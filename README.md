<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cadastro de Roteirização</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f4f4f4;
        }
        .form-container {
            margin-bottom: 20px;
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        .form-container input {
            padding: 10px;
            margin: 10px 0;
            width: 100%;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        .btn {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            margin-top: 10px;
            border-radius: 4px;
        }
        .btn:hover {
            background-color: #45a049;
        }
        .btn:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
    </style>
</head>
<body>

    <h1>Cadastro de Roteirização</h1>

    <div class="form-container">
        <label for="transportadora-origem">Transportadora Origem:</label>
        <input type="text" id="transportadora-origem" required>
        <label for="destinatario">Destinatário:</label>
        <input type="text" id="destinatario" required>
        <label for="placa-carreta">Placa Carreta:</label>
        <input type="text" id="placa-carreta" required>
        <label for="remetente-uf">Remetente UF:</label>
        <input type="text" id="remetente-uf" required>
        <label for="destino-uf">Destino UF:</label>
        <input type="text" id="destino-uf" required>
        <label for="nome-motorista">Nome Motorista:</label>
        <input type="text" id="nome-motorista" required>
        <button class="btn" onclick="cadastrar()">Cadastrar</button>
    </div>

    <div id="gerar-pdf-container" style="display:none;">
        <button class="btn" onclick="gerarPDF()">Gerar PDF</button>
    </div>

    <div id="gerar-novo-cadastro-container" style="display:none;">
        <p>Gerar Novo Cadastro?</p>
        <button class="btn" onclick="novoCadastro('sim')">Sim</button>
        <button class="btn" onclick="novoCadastro('nao')">Não</button>
    </div>

    <script>
        function cadastrar() {
            const transportadoraOrigem = document.getElementById('transportadora-origem').value;
            const destinatario = document.getElementById('destinatario').value;
            const placaCarreta = document.getElementById('placa-carreta').value;
            const remetenteUf = document.getElementById('remetente-uf').value;
            const destinoUf = document.getElementById('destino-uf').value;
            const nomeMotorista = document.getElementById('nome-motorista').value;

            // Verificar se os campos estão preenchidos
            if (!transportadoraOrigem || !destinatario || !placaCarreta || !remetenteUf || !destinoUf || !nomeMotorista) {
                alert('Por favor, preencha todos os campos!');
                return;
            }

            // Exibir botão "Gerar PDF"
            document.getElementById('gerar-pdf-container').style.display = 'block';

            // Exibir opção para gerar novo cadastro
            document.getElementById('gerar-novo-cadastro-container').style.display = 'block';

            // Armazenar dados em variáveis globais
            window.dadosCadastro = {
                transportadoraOrigem,
                destinatario,
                placaCarreta,
                remetenteUf,
                destinoUf,
                nomeMotorista
            };
        }

        function gerarPDF() {
            const { transportadoraOrigem, destinatario, placaCarreta, remetenteUf, destinoUf, nomeMotorista } = window.dadosCadastro;

            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            // Tamanho da página (formato A4)
            const pageWidth = doc.internal.pageSize.width;
            const pageHeight = doc.internal.pageSize.height;

            // Adiciona borda ao redor de todo o conteúdo
            const borderMargin = 15;
            const borderX = 20;
            const borderY = 20;
            const borderWidth = pageWidth - 40; // 20px de cada lado
            const borderHeight = 150; // Altura do conteúdo

            doc.setDrawColor(0, 0, 0); // Cor preta para borda
            doc.setLineWidth(2);
            doc.rect(borderX, borderY, borderWidth, borderHeight);

            // Título: "ROTEIRIZAÇÃO"
            doc.setFontSize(26);
            doc.setTextColor(0, 0, 255); // Azul
            const title = 'ROTEIRIZAÇÃO';
            const titleWidth = doc.getStringUnitWidth(title) * doc.internal.getFontSize() / doc.internal.scaleFactor;
            const titleX = (pageWidth - titleWidth) / 2; // Centraliza horizontalmente
            doc.text(title, titleX, 40);

            // Subtítulo: "MARKETCARGO"
            doc.setFontSize(18);
            doc.setTextColor(255, 165, 0); // Laranja
            const subtitle = 'MARKETCARGO';
            const subtitleWidth = doc.getStringUnitWidth(subtitle) * doc.internal.getFontSize() / doc.internal.scaleFactor;
            const subtitleX = (pageWidth - subtitleWidth) / 2; // Centraliza horizontalmente
            doc.text(subtitle, subtitleX, 55);

            // Dados do formulário
            doc.setFontSize(14);
            doc.setTextColor(0, 0, 0); // Preto
            const textY = 75;

            doc.text(`Transportadora Origem: ${transportadoraOrigem}`, borderX + 10, textY);
            doc.text(`Destinatário: ${destinatario}`, borderX + 10, textY + 10);
            doc.text(`Placa Carreta: ${placaCarreta}`, borderX + 10, textY + 20);
            doc.text(`Remetente UF: ${remetenteUf}`, borderX + 10, textY + 30);
            doc.text(`Destino UF: ${destinoUf}`, borderX + 10, textY + 40);
            doc.text(`Nome Motorista: ${nomeMotorista}`, borderX + 10, textY + 50);

            // Gerar o PDF e abrir para download
            doc.save('roteirizacao.pdf');
        }

        function novoCadastro(opcao) {
            if (opcao === 'sim') {
                // Limpar os campos de cadastro
                document.getElementById('transportadora-origem').value = '';
                document.getElementById('destinatario').value = '';
                document.getElementById('placa-carreta').value = '';
                document.getElementById('remetente-uf').value = '';
                document.getElementById('destino-uf').value = '';
                document.getElementById('nome-motorista').value = '';
            }

            // Ocultar a opção de novo cadastro
            document.getElementById('gerar-novo-cadastro-container').style.display = 'none';
        }
    </script>

</body>
</html>
