<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gargamelpass - Simulador de Recompensas Brawl Stars</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .text-shadow {
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
        .card-enter {
            animation: card-enter 0.7s cubic-bezier(0.25, 0.46, 0.45, 0.94) both;
        }
        @keyframes card-enter {
            0% {
                transform: scale(0.5);
                opacity: 0;
            }
            100% {
                transform: scale(1);
                opacity: 1;
            }
        }
        /* Mapeamento de Raridade para Estilos */
        .rarity-comum { background-color: #6a737b; border-color: #a2acb4; }
        .rarity-raro { background-color: #4CAF50; border-color: #81C784; }
        .rarity-super-raro { background-color: #2196F3; border-color: #64B5F6; }
        .rarity-epico { background-color: #9C27B0; border-color: #CE93D8; }
        .rarity-mitico { background-color: #F44336; border-color: #EF9A9A; }
        .rarity-lendario { background-color: #FFEB3B; border-color: #FFF59D; color: #333; }
        .rarity-cromatico { 
            background: linear-gradient(45deg, #FFEB3B, #F44336, #9C27B0, #2196F3);
            border-color: #fff;
        }
        .btn-generate {
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        .btn-generate:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }
        .btn-generate:active {
            transform: translateY(0);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
    </style>
</head>
<body class="bg-gray-900 text-white">

    <div class="container mx-auto p-4 md:p-8">

        <!-- Cabeçalho -->
        <header class="text-center mb-8">
            <h1 class="text-5xl md:text-7xl font-black uppercase text-shadow bg-clip-text text-transparent bg-gradient-to-r from-yellow-400 via-red-500 to-purple-600">
                Gargamelpass
            </h1>
            <p class="text-gray-300 mt-2 text-lg">Recompensas com descontos e muito mais no Brawstars!!!</p>
        </header>

        <!-- Aviso Legal Importante -->
        <div class="bg-red-800 border-l-4 border-red-500 text-red-100 p-4 rounded-lg mb-8" role="alert">
            <p class="font-bold">Aviso Importante!</p>
            <p>Este é um site de <strong class="underline">SIMULAÇÃO</strong> criado por um fã. As recompensas geradas aqui são fictícias e <strong class="underline">NÃO</strong> têm valor ou efeito no jogo Brawl Stars. Nunca pediremos sua senha ou dados pessoais.</p>
        </div>

        <!-- Botão Gerador -->
        <div class="text-center mb-8">
            <button id="generate-button" class="btn-generate bg-gradient-to-br from-yellow-500 to-orange-600 hover:from-yellow-600 hover:to-orange-700 text-white font-bold py-4 px-10 rounded-full text-2xl uppercase shadow-lg transform transition hover:scale-105">
                Gerar Chave Aleatória!
            </button>
        </div>

        <!-- Área de Resultados -->
        <div id="result-area" class="text-center min-h-[400px] flex items-center justify-center">
             <div class="text-gray-500 text-2xl">Clique no botão para testar sua sorte!</div>
        </div>
        
    </div>

    <footer class="text-center p-4 mt-8 border-t border-gray-700">
        <p class="text-gray-500">&copy; 2024 Gargamelpass. Todos os direitos reservados (para a simulação, claro!).</p>
        <p class="text-gray-500 text-sm mt-1">Brawl Stars e todos os seus elementos são propriedade da Supercell.</p>
    </footer>

    <script>
        const generateButton = document.getElementById('generate-button');
        const resultArea = document.getElementById('result-area');

        // --- INÍCIO DA LÓGICA DO BANCO DE DADOS ---

        let database = null; // Variável para armazenar nosso banco de dados

        // Função para carregar o banco de dados. O JSON foi incorporado para evitar erros de 'fetch'.
        function setupDatabase() {
            database = {
              "info": {
                "nome_banco": "Gargamelpass DB",
                "versao": "1.0",
                "descricao": "Banco de dados fictício com recompensas para o simulador de Brawl Stars.",
                "autor": "Gargamel & Gemini"
              },
              "brawlers": [
                { "id": 101, "nome": "Shelly", "raridade": "Comum", "classe": "Destruidora" },
                { "id": 102, "nome": "Nita", "raridade": "Raro", "classe": "Lutadora" },
                { "id": 103, "nome": "Colt", "raridade": "Raro", "classe": "Atirador" },
                { "id": 104, "nome": "Bull", "raridade": "Raro", "classe": "Tanque" },
                { "id": 105, "nome": "El Primo", "raridade": "Raro", "classe": "Tanque" },
                { "id": 106, "nome": "Jessie", "raridade": "Super-Raro", "classe": "Lutadora" },
                { "id": 107, "nome": "Dynamike", "raridade": "Super-Raro", "classe": "Lançador" },
                { "id": 108, "nome": "Tick", "raridade": "Super-Raro", "classe": "Lançador" },
                { "id": 109, "nome": "Piper", "raridade": "Épico", "classe": "Atiradora" },
                { "id": 110, "nome": "Frank", "raridade": "Épico", "classe": "Tanque" },
                { "id": 111, "nome": "Bibi", "raridade": "Épico", "classe": "Algoz" },
                { "id": 112, "nome": "Edgar", "raridade": "Épico", "classe": "Algoz" },
                { "id": 113, "nome": "Mortis", "raridade": "Mítico", "classe": "Algoz" },
                { "id": 114, "nome": "Tara", "raridade": "Mítico", "classe": "Lutadora" },
                { "id": 115, "nome": "Squeak", "raridade": "Mítico", "classe": "Lutador" },
                { "id": 116, "nome": "Alli", "raridade": "Mítico", "classe": "Assassina" },
                { "id": 117, "nome": "Spike", "raridade": "Lendário", "classe": "Atirador" },
                { "id": 118, "nome": "Crow", "raridade": "Lendário", "classe": "Algoz" },
                { "id": 119, "nome": "Leon", "raridade": "Lendário", "classe": "Algoz" },
                { "id": 120, "nome": "Sandy", "raridade": "Lendário", "classe": "Suporte" },
                { "id": 121, "nome": "Gale", "raridade": "Cromático", "classe": "Suporte" },
                { "id": 122, "nome": "Surge", "raridade": "Cromático", "classe": "Lutador" },
                { "id": 123, "nome": "Colette", "raridade": "Cromático", "classe": "Lutadora" }
              ],
              "roupas": [
                { "id": 201, "nome": "Shelly Bandida", "raridade": "Raro", "brawler_associado": "Shelly" },
                { "id": 202, "nome": "Colt Roqueiro", "raridade": "Raro", "brawler_associado": "Colt" },
                { "id": 203, "nome": "Brock Praiano", "raridade": "Super-Raro", "brawler_associado": "Brock" },
                { "id": 204, "nome": "Ricochet", "raridade": "Super-Raro", "brawler_associado": "Rico" },
                { "id": 205, "nome": "Spike Sakura", "raridade": "Épico", "brawler_associado": "Spike" },
                { "id": 206, "nome": "Leon Tubarão", "raridade": "Épico", "brawler_associado": "Leon" },
                { "id": 207, "nome": "Mortis Robô", "raridade": "Mítico", "brawler_associado": "Mortis" },
                { "id": 208, "nome": "Corvo Fênix", "raridade": "Mítico", "brawler_associado": "Crow" },
                { "id": 209, "nome": "Robô-Spike", "raridade": "Lendário", "brawler_associado": "Spike" },
                { "id": 210, "nome": "Mecha-Corvo", "raridade": "Lendário", "brawler_associado": "Crow" },
                { "id": 211, "nome": "Mortis de Hades", "raridade": "Cromático", "brawler_associado": "Mortis" }
              ],
              "recursos": {
                "pontos_de_evolucao": [
                  { "id": 301, "nome": "100 Pontos de Poder", "valor": 100, "raridade": "Comum" },
                  { "id": 302, "nome": "250 Pontos de Poder", "valor": 250, "raridade": "Raro" },
                  { "id": 303, "nome": "500 Pontos de Poder", "valor": 500, "raridade": "Épico" }
                ],
                "moedas": [
                  { "id": 401, "nome": "200 Moedas", "valor": 200, "raridade": "Comum" },
                  { "id": 402, "nome": "500 Moedas", "valor": 500, "raridade": "Raro" },
                  { "id": 403, "nome": "1000 Moedas", "valor": 1000, "raridade": "Épico" }
                ],
                "gemas": [
                  { "id": 501, "nome": "10 Gemas", "valor": 10, "raridade": "Épico" },
                  { "id": 502, "nome": "30 Gemas", "valor": 30, "raridade": "Mítico" },
                  { "id": 503, "nome": "80 Gemas", "valor": 80, "raridade": "Lendário" }
                ]
              }
            };
            console.log("Banco de dados carregado com sucesso!", database.info);
        }

        // Função para converter o nome da raridade (ex: "Super-Raro") para um slug de CSS (ex: "super-raro")
        function getRaritySlug(rarityName) {
            return rarityName.toLowerCase().replace('-', ' ').replace(/\s+/g, '-');
        }

        function getRandomReward() {
            // Criamos uma grande lista com todas as recompensas possíveis
            const allRewards = [
                ...database.brawlers.map(item => ({ ...item, type: 'Brawler' })),
                ...database.roupas.map(item => ({ ...item, type: 'Roupa' })),
                ...database.recursos.pontos_de_evolucao.map(item => ({ ...item, type: 'Pontos' })),
                ...database.recursos.moedas.map(item => ({ ...item, type: 'Moedas' })),
                ...database.recursos.gemas.map(item => ({ ...item, type: 'Gemas' }))
            ];

            // Sorteia um item aleatório da lista geral
            const randomReward = allRewards[Math.floor(Math.random() * allRewards.length)];
            return randomReward;
        }

        function displayReward(reward) {
            const raritySlug = getRaritySlug(reward.raridade);
            const isLegendary = raritySlug === 'lendario';

            let title = 'Recompensa Desbloqueada!';
            let subtext = '';
            
            // Ajusta o título com base no tipo de recompensa
            switch(reward.type) {
                case 'Brawler':
                    title = 'Novo Brawler!';
                    subtext = `Classe: ${reward.classe}`;
                    break;
                case 'Roupa':
                    title = 'Nova Roupa!';
                    subtext = `Para o brawler: ${reward.brawler_associado}`;
                    break;
                case 'Pontos':
                    title = 'Pontos de Evolução!';
                    break;
                case 'Moedas':
                    title = 'Moedas!';
                    break;
                case 'Gemas':
                    title = 'Gemas!';
                    break;
            }

            resultArea.innerHTML = `
                <div class="card-enter bg-gray-800 rounded-2xl p-6 border-4 rarity-${raritySlug} w-full max-w-sm shadow-2xl">
                    <div class="text-sm font-bold uppercase tracking-widest ${isLegendary ? 'text-gray-800' : 'text-white'}">${reward.raridade}</div>
                    <h2 class="text-2xl font-bold mt-2">${title}</h2>
                    <div class="my-4">
                        <img src="https://placehold.co/200x200/${isLegendary ? '333333' : 'FFFFFF'}/${isLegendary ? 'FFEB3B' : '000000'}?text=${encodeURIComponent(reward.type)}" class="mx-auto rounded-lg border-2 border-gray-600" alt="Imagem da recompensa">
                    </div>
                    <p class="text-4xl font-black my-2">${reward.nome}</p>
                    <p class="text-gray-400">${subtext}</p>
                </div>
            `;
        }
        
        // --- EVENTO DO BOTÃO ---
        generateButton.addEventListener('click', () => {
            if (!database) {
                alert("O banco de dados ainda não foi carregado. Tente novamente em um instante.");
                return;
            }

            generateButton.disabled = true;
            generateButton.textContent = 'Gerando...';
            resultArea.innerHTML = `<div class="text-gray-500 text-2xl animate-pulse">Sorteando uma recompensa...</div>`;

            setTimeout(() => {
                const reward = getRandomReward();
                displayReward(reward);
                generateButton.disabled = false;
                generateButton.textContent = 'Gerar Chave Novamente!';
            }, 1500);
        });
        
        // Inicializa o banco de dados quando a página carrega
        setupDatabase();

    </script>
</body>
</html>
