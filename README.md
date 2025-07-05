<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Financeiro Familiar</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- 
      Palette Name: Vibrant and Fresh
      Colors:
      - Teal: #00A9A5
      - Dark Blue: #022E5A
      - Light Blue: #4D648D
      - Off-white: #E0E3E8
      - Yellow-Orange: #F0A202
    -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #E0E3E8;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
        .flow-arrow {
            position: relative;
            width: 100%;
            height: 40px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .flow-arrow::after {
            content: '↓';
            font-size: 2rem;
            color: #4D648D;
            font-weight: bold;
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.4);
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #fefefe;
            margin: auto;
            padding: 20px;
            border-radius: 8px;
            width: 90%;
            max-width: 700px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            position: relative;
            max-height: 80vh;
            overflow-y: auto;
        }
        .close-button {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }
        .close-button:hover,
        .close-button:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }
        .loading-spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #00A9A5;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
            display: inline-block;
            vertical-align: middle;
            margin-right: 8px;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="bg-gray-100 text-gray-800">

    <div class="container mx-auto p-4 md:p-8">
        
        <header class="text-center mb-10">
            <h1 class="text-4xl md:text-5xl font-bold text-[#022E5A]">Dashboard Financeiro Familiar</h1>
            <p class="text-lg text-[#4D648D] mt-2">Um plano visual para nosso sucesso financeiro.</p>
        </header>

        <main class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">

            <!-- Card: Visão Geral -->
            <div class="bg-white rounded-xl shadow-lg p-6 md:col-span-2 lg:col-span-1">
                <h2 class="text-2xl font-bold text-[#022E5A] mb-4">Visão Geral da Renda</h2>
                <div class="flex flex-col items-center justify-center space-y-2">
                    <p class="text-lg text-gray-600">Renda Bruta Mensal</p>
                    <p class="text-5xl font-extrabold text-[#00A9A5]">R$ 3.600,00</p>
                    <p class="text-sm text-gray-500">(R$ 3.000,00 em dinheiro + R$ 600,00 de vale alimentação)</p>
                </div>
                <div class="chart-container mt-6">
                    <canvas id="incomeDonutChart"></canvas>
                </div>
                <p class="text-center text-sm text-gray-500 mt-4">Esta é a divisão inicial do nosso orçamento. A maior parte está disponível para nossas despesas variáveis e metas.</p>
            </div>

            <!-- Card: Detalhamento dos Gastos -->
            <div class="bg-white rounded-xl shadow-lg p-6 md:col-span-2">
                <h2 class="text-2xl font-bold text-[#022E5A] mb-4">Onde Nosso Dinheiro Vai?</h2>
                <div class="chart-container h-[400px] max-h-[500px]">
                    <canvas id="spendingBarChart"></canvas>
                </div>
                <p class="text-center text-sm text-gray-500 mt-4">Este gráfico compara nossas principais categorias de gastos. A alimentação é um item crucial, agora com o apoio do vale alimentação.</p>
            </div>

            <!-- Card: Alocação do Dinheiro Flexível -->
            <div class="bg-white rounded-xl shadow-lg p-6 md:col-span-2 lg:col-span-1">
                <h2 class="text-2xl font-bold text-[#022E5A] mb-4">Plano para o Dinheiro Flexível</h2>
                <div class="flex flex-col items-center justify-center space-y-2">
                    <p class="text-lg text-gray-600">Valor Total Disponível (em dinheiro)</p>
                    <p class="text-5xl font-extrabold text-[#F0A202]">R$ 2.340,00</p>
                </div>
                <div class="chart-container mt-6">
                    <canvas id="flexibleSpendingPieChart"></canvas>
                </div>
                <div class="mt-6 text-sm text-gray-700 space-y-2">
                    <p><span class="font-semibold text-[#00A9A5]">Reserva de Emergência (R$ 400):</span> Essencial para imprevistos como despesas médicas não cobertas ou reparos urgentes. Priorize a construção dessa reserva.</p>
                    <p><span class="font-semibold text-[#00A9A5]">Saúde (R$ 250):</span> Inclui medicamentos de rotina, consultas pontuais (se não houver plano de saúde) e itens de primeiros socorros.</p>
                    <p><span class="font-semibold text-[#00A9A5]">Educação da Criança (R$ 200):</span> Para materiais escolares, livros infantis, pequenas atividades educativas ou passeios culturais.</p>
                    <p><span class="font-semibold text-[#00A9A5]">Lazer e Pessoais (R$ 500):</span> Destine para momentos de diversão em família (parques, picnics, filmes em casa), pequenos mimos pessoais e vestuário essencial, sempre com consciência.</p>
                    <p><span class="font-semibold text-[#00A9A5]">Flexível / Variáveis (R$ 990):</span> Esta é a categoria "coringa" para despesas variáveis não previstas, como manutenção da casa, transporte extra, ou para reforçar outras categorias se necessário. Use com sabedoria e acompanhamento!</p>
                </div>
            </div>

            <!-- Card: Foco na Alimentação -->
            <div class="bg-white rounded-xl shadow-lg p-6 md:col-span-1 lg:col-span-1 flex flex-col justify-center items-center text-center">
                 <div class="text-6xl mb-4">🛒</div>
                <h2 class="text-2xl font-bold text-[#022E5A] mb-2">Meta de Alimentação</h2>
                <p class="text-4xl font-extrabold text-[#00A9A5] mb-2">R$ 900,00</p>
                <p class="text-lg text-gray-600 mb-4">por mês</p>
                <div class="bg-blue-100 text-[#022E5A] font-semibold py-2 px-4 rounded-full">
                    R$ 600,00 (Vale Alimentação) + R$ 300,00 (Dinheiro)
                </div>
                <p class="text-sm text-gray-500 mt-4">O vale alimentação cobre a maior parte, mas ainda precisamos planejar os R$ 300,00 em dinheiro e os R$ 225,00 semanais para o total.</p>
                <button id="generateRecipesBtn" class="mt-4 bg-[#F0A202] hover:bg-yellow-600 text-white font-bold py-2 px-4 rounded-full shadow-md transition duration-300 ease-in-out transform hover:scale-105">
                    ✨ Gerar Ideias de Refeições
                </button>
            </div>

            <!-- Card: Dica Financeira Personalizada -->
            <div class="bg-white rounded-xl shadow-lg p-6 md:col-span-1 lg:col-span-1 flex flex-col justify-center items-center text-center">
                <div class="text-6xl mb-4">💡</div>
                <h2 class="text-2xl font-bold text-[#022E5A] mb-2">Dica Financeira ✨</h2>
                <p class="text-lg text-gray-600 mb-4">Receba uma dica personalizada para otimizar suas finanças.</p>
                <button id="generateTipBtn" class="mt-4 bg-[#00A9A5] hover:bg-teal-600 text-white font-bold py-2 px-4 rounded-full shadow-md transition duration-300 ease-in-out transform hover:scale-105">
                    Obter Dica
                </button>
                <div id="financialTipOutput" class="mt-4 text-sm text-gray-700 italic"></div>
            </div>

            <!-- Card: Lista de Compras Detalhada -->
            <div class="bg-white rounded-xl shadow-lg p-6 md:col-span-2 lg:col-span-3">
                <h2 class="text-2xl font-bold text-[#022E5A] mb-4 text-center">Lista de Compras Essenciais (Sugestão Mensal)</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div>
                        <h3 class="text-xl font-semibold text-[#4D648D] mb-3">Alimentação</h3>
                        <ul class="list-disc list-inside text-gray-700 text-sm space-y-1">
                            <li class="font-bold text-[#022E5A]">Cesta Básica / Mercearia:</li>
                            <li>Arroz (5kg) - 1 a 2 pacotes</li>
                            <li>Feijão (1kg) - 2 a 3 pacotes</li>
                            <li>Macarrão (espaguete, parafuso) - 3 a 4 pacotes</li>
                            <li>Farinha de trigo / de mandioca / Fubá</li>
                            <li>Óleo de soja / Azeite (para cozinhar)</li>
                            <li>Sal / Açúcar / Café</li>
                            <li>Leite (líquido ou em pó, para o café e a criança) - Grande quantidade</li>
                            <li>Pães (de forma e/ou francês fresco - comprar semanalmente)</li>
                            <li>Biscoitos (simples, tipo água e sal, maisena)</li>
                            <li>Granola / Aveia (para o café da manhã)</li>
                            <li>Ovos (1 ou 2 cartelas)</li>
                            <li>Temperos secos (orégano, pimenta do reino, colorau, cominho)</li>
                            <li>Molho de tomate / Extrato de tomate</li>
                            <li>Milho / Ervilha (em lata/sachê)</li>
                            <li>Sardinha / Atum (em lata, para refeições rápidas)</li>
                            <li class="font-bold text-[#022E5A] mt-3">Açougue / Peixaria:</li>
                            <li>Cortes de frango (peito, coxa/sobrecoxa - grandes quantidades, congelar)</li>
                            <li>Carne bovina (patinho, acém, músculo - cortes para ensopados ou moer)</li>
                            <li>Carne suína (pernil, paleta - cortes econômicos)</li>
                            <li>Peixe (sugestão: tilápia, sardinha - opções mais acessíveis)</li>
                            <li class="font-bold text-[#022E5A] mt-3">Hortifrúti (comprar semanalmente ou a cada 15 dias):</li>
                            <li>Verduras: Alface, couve, espinafre, cheiro-verde, brócolis, couve-flor.</li>
                            <li>Legumes: Batata, cebola, alho, cenoura, abobrinha, chuchu, tomate, beterraba, pepino.</li>
                            <li>Frutas: Banana, maçã, laranja, mamão (opções mais baratas e que a criança gosta).</li>
                            <li class="font-bold text-[#022E5A] mt-3">Laticínios e Frios (comprar semanalmente):</li>
                            <li>Queijo (muçarela ou prato - em peça é mais barato)</li>
                            <li>Presunto</li>
                            <li>Iogurte (especialmente para a criança)</li>
                            <li>Manteiga ou margarina</li>
                        </ul>
                    </div>
                    <div>
                        <h3 class="text-xl font-semibold text-[#4D648D] mb-3">Higiene Pessoal</h3>
                        <ul class="list-disc list-inside text-gray-700 text-sm space-y-1">
                            <li>Sabonete (em barra ou líquido)</li>
                            <li>Shampoo e Condicionador</li>
                            <li>Creme Dental</li>
                            <li>Escovas de dente (repor a cada 3 meses)</li>
                            <li>Papel Higiênico</li>
                            <li>Desodorante</li>
                            <li>Absorventes (se aplicável)</li>
                            <li>Creme para bebê / Pomada para assadura (se usa)</li>
                            <li>Fio dental</li>
                            <li>Cotonetes</li>
                            <li>Álcool em gel (para as mãos)</li>
                        </ul>
                    </div>
                    <div>
                        <h3 class="text-xl font-semibold text-[#4D648D] mb-3">Limpeza da Casa</h3>
                        <ul class="list-disc list-inside text-gray-700 text-sm space-y-1">
                            <li>Detergente de louça</li>
                            <li>Sabão em pó / Líquido para roupas</li>
                            <li>Amaciante de roupas</li>
                            <li>Desinfetante</li>
                            <li>Água sanitária / Cloro</li>
                            <li>Limpa-vidros</li>
                            <li>Pano de chão / Buchas / Esponjas</li>
                            <li>Sacos de lixo (tamanhos variados)</li>
                            <li>Desinfetante para banheiro</li>
                            <li>Limpa-superfícies multiuso</li>
                            <li>Vassoura / Rodo (se precisar repor)</li>
                        </ul>
                    </div>
                </div>
                <p class="text-center text-sm text-gray-500 mt-6">Esta lista é uma sugestão e pode ser adaptada às preferências e necessidades específicas da sua família. Priorize sempre promoções e compras em maior volume para itens não perecíveis.</p>
            </div>

            <!-- Card: Fluxo do Dinheiro -->
            <div class="bg-white rounded-xl shadow-lg p-6 md:col-span-2">
                <h2 class="text-2xl font-bold text-[#022E5A] mb-6 text-center">Nosso Fluxo de Dinheiro Simplificado</h2>
                <div class="flex flex-col items-center">
                    <!-- Renda -->
                    <div class="bg-[#022E5A] text-white p-4 rounded-lg shadow-md w-full max-w-xs text-center">
                        <p class="font-bold text-lg">Renda Bruta</p>
                        <p class="text-2xl font-bold">R$ 3.600,00</p>
                    </div>
                    
                    <div class="flow-arrow"></div>

                    <!-- Divisão -->
                    <div class="flex flex-col md:flex-row gap-4 w-full justify-center">
                        <div class="bg-[#4D648D] text-white p-4 rounded-lg shadow-md flex-1 text-center">
                            <p class="font-bold text-lg">Gastos Fixos</p>
                            <p class="text-2xl font-bold">R$ 710,00</p>
                        </div>
                        <div class="bg-[#00A9A5] text-white p-4 rounded-lg shadow-md flex-1 text-center">
                            <p class="font-bold text-lg">Dinheiro Disponível (em dinheiro)</p>
                            <p class="text-2xl font-bold">R$ 2.890,00</p>
                        </div>
                    </div>

                    <div class="flow-arrow md:hidden"></div>
                    <div class="w-px bg-[#4D648D] h-10 hidden md:block mx-4"></div>


                    <!-- Destinos -->
                     <div class="bg-gray-100 p-4 rounded-lg w-full max-w-2xl text-center mt-4 md:mt-0">
                        <p class="font-bold text-lg text-gray-700">Destino do Dinheiro Disponível</p>
                         <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 mt-2 text-sm">
                            <div class="bg-white p-2 rounded shadow">Alimentação (Vale): R$ 600</div>
                            <div class="bg-white p-2 rounded shadow">Alimentação (Dinheiro): R$ 300</div>
                            <div class="bg-white p-2 rounded shadow">Higiene: R$ 150</div>
                            <div class="bg-white p-2 rounded shadow">Limpeza: R$ 100</div>
                            <div class="bg-white p-2 rounded shadow">Outros: R$ 2.340</div>
                        </div>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <!-- Modal para Ideias de Refeições -->
    <div id="recipeModal" class="modal">
        <div class="modal-content">
            <span class="close-button" onclick="closeModal('recipeModal')">&times;</span>
            <h3 class="text-xl font-bold text-[#022E5A] mb-4">Ideias de Refeições para o Orçamento</h3>
            <div id="recipeOutput" class="text-gray-700 text-base leading-relaxed">
                <div class="flex items-center justify-center text-gray-500" id="recipeLoading">
                    <div class="loading-spinner"></div>
                    Carregando ideias...
                </div>
            </div>
        </div>
    </div>

    <script>
        const FONT_COLOR = '#4D648D';
        const GRID_COLOR = '#E0E3E8';
        const PALETTE = {
            teal: '#00A9A5',
            darkBlue: '#022E5A',
            lightBlue: '#4D648D',
            offWhite: '#E0E3E8',
            yellowOrange: '#F0A202'
        };

        const tooltipTitleCallback = (tooltipItems) => {
            const item = tooltipItems[0];
            let label = item.chart.data.labels[item.dataIndex];
            if (Array.isArray(label)) {
                return label.join(' ');
            }
            return label;
        };

        const chartDefaultOptions = {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    position: 'bottom',
                    labels: {
                        color: FONT_COLOR,
                        font: {
                            family: "'Inter', sans-serif"
                        }
                    }
                },
                tooltip: {
                    callbacks: {
                        title: tooltipTitleCallback
                    }
                }
            }
        };

        // Gráfico 1: Visão Geral da Renda (Donut)
        const incomeCtx = document.getElementById('incomeDonutChart').getContext('2d');
        new Chart(incomeCtx, {
            type: 'doughnut',
            data: {
                labels: ['Gastos Fixos', 'Dinheiro Disponível (Dinheiro)', 'Vale Alimentação'],
                datasets: [{
                    label: 'Distribuição da Renda',
                    data: [710, 2290, 600], // 3000 - 710 = 2290 cash available
                    backgroundColor: [PALETTE.darkBlue, PALETTE.teal, PALETTE.yellowOrange],
                    borderColor: '#FFFFFF',
                    borderWidth: 4
                }]
            },
            options: { ...chartDefaultOptions }
        });

        // Gráfico 2: Detalhamento dos Gastos (Barras)
        const spendingCtx = document.getElementById('spendingBarChart').getContext('2d');
        new Chart(spendingCtx, {
            type: 'bar',
            data: {
                labels: ['Gastos Fixos', 'Alimentação (Vale)', 'Alimentação (Dinheiro)', 'Higiene e Limpeza', 'Outras Despesas'],
                datasets: [{
                    label: 'Valor (R$)',
                    data: [710, 600, 300, 250, 2340],
                    backgroundColor: [PALETTE.darkBlue, PALETTE.yellowOrange, PALETTE.teal, PALETTE.lightBlue, '#AAB8C2'],
                    borderRadius: 5
                }]
            },
            options: {
                ...chartDefaultOptions,
                plugins: {
                    ...chartDefaultOptions.plugins,
                    legend: {
                        display: false
                    }
                },
                scales: {
                    y: {
                        beginAtZero: true,
                        grid: {
                            color: GRID_COLOR
                        },
                        ticks: {
                            color: FONT_COLOR
                        }
                    },
                    x: {
                        grid: {
                            display: false
                        },
                        ticks: {
                            color: FONT_COLOR
                        }
                    }
                }
            }
        });

        // Gráfico 3: Alocação Flexível (Torta)
        const flexibleCtx = document.getElementById('flexibleSpendingPieChart').getContext('2d');
        new Chart(flexibleCtx, {
            type: 'pie',
            data: {
                labels: ['Reserva de Emergência', 'Saúde', 'Educação da Criança', 'Lazer e Pessoais', 'Flexível/Variáveis'],
                datasets: [{
                    label: 'Distribuição Sugerida',
                    data: [400, 250, 200, 500, 990],
                    backgroundColor: [
                        PALETTE.darkBlue,
                        PALETTE.teal,
                        PALETTE.yellowOrange,
                        PALETTE.lightBlue,
                        '#AAB8C2'
                    ],
                    borderColor: '#FFFFFF',
                    borderWidth: 4,
                }]
            },
            options: { ...chartDefaultOptions }
        });

        // Gemini API Integration
        const generateRecipesBtn = document.getElementById('generateRecipesBtn');
        const recipeModal = document.getElementById('recipeModal');
        const recipeOutput = document.getElementById('recipeOutput');
        const recipeLoading = document.getElementById('recipeLoading');

        const generateTipBtn = document.getElementById('generateTipBtn');
        const financialTipOutput = document.getElementById('financialTipOutput');

        function showModal(modalId) {
            document.getElementById(modalId).style.display = 'flex';
        }

        function closeModal(modalId) {
            document.getElementById(modalId).style.display = 'none';
        }

        generateRecipesBtn.addEventListener('click', async () => {
            showModal('recipeModal');
            recipeOutput.innerHTML = ''; // Clear previous content
            recipeLoading.style.display = 'flex'; // Show loading spinner

            const prompt = `Crie um plano de refeições semanal ou algumas ideias de receitas para uma família de 3 pessoas (2 adultos e 1 criança de 3 anos) com um orçamento total de R$ 900,00 por mês para alimentação, sendo R$ 600,00 de vale alimentação e R$ 300,00 em dinheiro. Foco em refeições econômicas, saudáveis e fáceis de preparar, considerando o uso do vale alimentação para compras maiores e o dinheiro para perecíveis. Formate a resposta de forma clara com títulos e listas.`;

            let chatHistory = [];
            chatHistory.push({ role: "user", parts: [{ text: prompt }] });
            const payload = { contents: chatHistory };
            const apiKey = ""; 
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    recipeOutput.innerHTML = text.replace(/\n/g, '<br>'); 
                } else {
                    recipeOutput.innerHTML = 'Não foi possível gerar ideias de refeições no momento. Tente novamente.';
                }
            } catch (error) {
                console.error('Erro ao chamar a API Gemini para receitas:', error);
                recipeOutput.innerHTML = 'Ocorreu um erro ao gerar as ideias de refeições. Verifique sua conexão ou tente novamente mais tarde.';
            } finally {
                recipeLoading.style.display = 'none'; // Hide loading spinner
            }
        });

        generateTipBtn.addEventListener('click', async () => {
            financialTipOutput.innerHTML = '<div class="loading-spinner"></div> Carregando dica...';

            const prompt = `Com base em uma renda bruta mensal de R$ 3.600,00 (sendo R$ 3.000,00 em dinheiro e R$ 600,00 de vale alimentação) e gastos fixos de R$ 710,00 (internet, água, energia, combustível), forneça uma dica financeira prática e acionável para uma família de 2 adultos e 1 criança de 3 anos que busca otimizar seu orçamento. A dica deve ser concisa e focada em economia ou planejamento, considerando a existência do vale alimentação.`;

            let chatHistory = [];
            chatHistory.push({ role: "user", parts: [{ text: prompt }] });
            const payload = { contents: chatHistory };
            const apiKey = ""; 
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    financialTipOutput.innerHTML = text;
                } else {
                    financialTipOutput.innerHTML = 'Não foi possível gerar uma dica no momento.';
                }
            } catch (error) {
                console.error('Erro ao chamar a API Gemini para dica:', error);
                financialTipOutput.innerHTML = 'Ocorreu um erro ao gerar a dica. Tente novamente.';
            }
        });
    </script>
</body>
</html>
