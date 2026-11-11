<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quadrologia de Games for Health - Acessível</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        health: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                            900: '#14532d',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        /* Estilos globais de acessibilidade e alto contraste */
        .high-contrast {
            background-color: #000000 !important;
            color: #ffff00 !important;
        }
        .high-contrast button, .high-contrast section, .high-contrast div {
            border-color: #ffff00 !important;
        }
        .high-contrast button {
            background-color: #000000 !important;
            color: #ffff00 !important;
            box-shadow: 0 0 0 2px #ffff00 !important;
        }
        .high-contrast button:hover, .high-contrast button:focus {
            background-color: #ffff00 !important;
            color: #000000 !important;
        }

        /* Indicador de foco evidente para acessibilidade via teclado */
        :focus-visible {
            outline: 4px solid #f59e0b !important;
            outline-offset: 3px !important;
        }

        /* Estilos para padrões visuais para daltonismo (Deuteranopia, Protanopia, Tritanopia) */
        .pattern-circle {
            background-image: radial-gradient(#000 20%, transparent 20%);
            background-size: 12px 12px;
        }
        .pattern-stripe {
            background-image: repeating-linear-gradient(45deg, #000 0, #000 4px, transparent 0, transparent 12px);
        }
        .pattern-cross {
            background-image: repeating-linear-gradient(0deg, #000 0, #000 2px, transparent 0, transparent 8px),
                              repeating-linear-gradient(90deg, #000 0, #000 2px, transparent 0, transparent 8px);
        }
        .pattern-dots {
            background-image: radial-gradient(#fff 30%, transparent 30%);
            background-size: 10px 10px;
        }

        /* Canvas de desenho tracejado */
        canvas {
            touch-action: none;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-900 font-sans min-h-screen flex flex-col transition-all duration-200">

    <!-- Região de Anúncios Acessíveis para Leitores de Tela -->
    <div id="sr-announcer" class="sr-only" aria-live="polite" aria-atomic="true"></div>

    <!-- Barra Superior de Controle Global de Acessibilidade -->
    <header class="bg-slate-900 text-white p-4 shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto flex flex-wrap justify-between items-center gap-4">
            <h1 class="text-xl md:text-2xl font-bold flex items-center gap-2" id="app-title">
                <span aria-hidden="true">🏥</span> Quadrologia Games for Health
            </h1>
            
            <nav aria-label="Controles globais de acessibilidade" class="flex flex-wrap items-center gap-2">
                <!-- Tamanho de Fonte -->
                <div class="flex items-center bg-slate-800 rounded-lg p-1 border border-slate-700">
                    <span class="text-xs px-2 text-slate-300 font-semibold" id="label-text-size">Texto:</span>
                    <button onclick="changeFontSize(-1)" class="px-3 py-1 bg-slate-700 hover:bg-slate-600 rounded text-sm font-bold" aria-label="Diminuir tamanho do texto">A-</button>
                    <button onclick="resetFontSize()" class="px-2 py-1 hover:bg-slate-700 rounded text-xs" aria-label="Restaurar tamanho do texto">Normal</button>
                    <button onclick="changeFontSize(1)" class="px-3 py-1 bg-slate-700 hover:bg-slate-600 rounded text-sm font-bold" aria-label="Aumentar tamanho do texto">A+</button>
                </div>

                <!-- Modos Visuais -->
                <button id="btn-contrast" onclick="toggleHighContrast()" class="px-3 py-1.5 bg-slate-800 hover:bg-slate-700 border border-slate-700 rounded-lg text-sm font-semibold flex items-center gap-1.5" aria-pressed="false">
                    <span aria-hidden="true">👁️</span> Alto Contraste
                </button>

                <!-- Leitura de Voz Guia -->
                <button id="btn-speech" onclick="toggleSpeech()" class="px-3 py-1.5 bg-green-700 hover:bg-green-600 rounded-lg text-sm font-semibold flex items-center gap-1.5" aria-pressed="true">
                    <span id="speech-icon" aria-hidden="true">🔊</span> Voz Guia: <span id="speech-status">LIGADA</span>
                </button>
            </nav>
        </div>
    </header>

    <!-- Navegação Secundária - Seleção dos 4 Jogos -->
    <nav class="bg-slate-800 text-white shadow-inner" aria-label="Seleção de Jogos">
        <div class="max-w-7xl mx-auto flex flex-wrap justify-center md:justify-start gap-2 p-3">
            <button onclick="switchTab('game1')" id="tab-game1" class="tab-btn px-4 py-2.5 rounded-lg font-bold text-sm md:text-base flex items-center gap-2 bg-green-600 text-white shadow" aria-selected="true" role="tab">
                <span aria-hidden="true">🎨</span> 1. Cores & Grupos (Visão)
            </button>
            <button onclick="switchTab('game2')" id="tab-game2" class="tab-btn px-4 py-2.5 rounded-lg font-bold text-sm md:text-base flex items-center gap-2 bg-slate-700 hover:bg-slate-600 text-slate-200" aria-selected="false" role="tab">
                <span aria-hidden="true">🔊</span> 2. Sons & Animais (Audição)
            </button>
            <button onclick="switchTab('game3')" id="tab-game3" class="tab-btn px-4 py-2.5 rounded-lg font-bold text-sm md:text-base flex items-center gap-2 bg-slate-700 hover:bg-slate-600 text-slate-200" aria-selected="false" role="tab">
                <span aria-hidden="true">✏️</span> 3. Tracing Tracejado (Motora)
            </button>
            <button onclick="switchTab('game4')" id="tab-game4" class="tab-btn px-4 py-2.5 rounded-lg font-bold text-sm md:text-base flex items-center gap-2 bg-slate-700 hover:bg-slate-600 text-slate-200" aria-selected="false" role="tab">
                <span aria-hidden="true">🎯</span> 4. Foco & Espaço (Mobilidade)
            </button>
        </div>
    </nav>

    <!-- Conteúdo Principal dos Jogos -->
    <main class="max-w-7xl mx-auto p-4 md:p-6 flex-1 w-full" id="main-content">
        
        <!-- ========================================== -->
        <!-- JOGO 1: SELEÇÃO DE CORES E GRUPOS (VISÃO) -->
        <!-- ========================================== -->
        <section id="game1" class="game-section space-y-6 block" aria-labelledby="g1-heading">
            <header class="bg-white p-5 rounded-xl border border-gray-200 shadow-sm flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                <div>
                    <h2 id="g1-heading" class="text-2xl font-bold text-slate-800 flex items-center gap-2">
                        🎨 Jogo 1: Seleção de Cores e Discriminação Visual
                    </h2>
                    <p class="text-gray-600 text-sm mt-1">
                        Selecione e agrupe os blocos correspondentes à cor/padrão alvo indicada.
                    </p>
                </div>

                <!-- Painel de Inclusão para Visão/Daltonismo -->
                <div class="flex flex-wrap gap-2 items-center bg-slate-100 p-2 rounded-lg border border-slate-300">
                    <span class="text-xs font-bold text-slate-700">Modo de Visão:</span>
                    <button onclick="setDaltonismMode('none')" id="daltonism-none" class="px-2.5 py-1 text-xs font-bold bg-slate-800 text-white rounded">Padrão</button>
                    <button onclick="setDaltonismMode('deuteranopia')" id="daltonism-deut" class="px-2.5 py-1 text-xs font-bold bg-slate-200 text-slate-800 rounded hover:bg-slate-300">Deuteranopia</button>
                    <button onclick="setDaltonismMode('protanopia')" id="daltonism-prot" class="px-2.5 py-1 text-xs font-bold bg-slate-200 text-slate-800 rounded hover:bg-slate-300">Protanopia</button>
                    <button onclick="setDaltonismMode('tritanopia')" id="daltonism-trit" class="px-2.5 py-1 text-xs font-bold bg-slate-200 text-slate-800 rounded hover:bg-slate-300">Tritanopia</button>
                </div>
            </header>

            <!-- Status do Jogo -->
            <div class="bg-white p-4 rounded-xl border border-gray-200 shadow-sm flex flex-wrap justify-between items-center gap-4">
                <div class="flex items-center gap-3">
                    <span class="text-lg font-bold">Alvo Atual:</span>
                    <div id="g1-target-card" class="px-4 py-2 rounded-lg font-bold text-white shadow flex items-center gap-2 border-2 border-slate-900" tabIndex="0">
                        <span id="g1-target-icon" class="text-xl"></span>
                        <span id="g1-target-text">Carregando...</span>
                    </div>
                </div>
                <div class="flex items-center gap-4">
                    <div class="text-sm font-semibold">Pontuação: <span id="g1-score" class="text-xl font-extrabold text-green-600">0</span></div>
                    <button onclick="initGame1()" class="px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white font-bold rounded-lg shadow transition-colors">
                        🔄 Novo Jogo
                    </button>
                </div>
            </div>

            <!-- Grade de Blocos -->
            <div id="g1-grid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4 p-4 bg-gray-100 rounded-xl border border-gray-300 min-h-[300px]" role="region" aria-label="Grade de blocos coloridos">
                <!-- Blocos gerados via JS -->
            </div>
        </section>


        <!-- ========================================== -->
        <!-- JOGO 2: IDENTIFICAÇÃO DE SONS E ANIMAIS    -->
        <!-- ========================================== -->
        <section id="game2" class="game-section space-y-6 hidden" aria-labelledby="g2-heading">
            <header class="bg-white p-5 rounded-xl border border-gray-200 shadow-sm">
                <h2 id="g2-heading" class="text-2xl font-bold text-slate-800 flex items-center gap-2">
                    🔊 Jogo 2: Identificação Auditiva de Sons de Animais
                </h2>
                <p class="text-gray-600 text-sm mt-1">
                    Escute o áudio sintetizado e identifique qual animal produziu o som. Dicas visuais da onda sonora e frequências estão disponíveis.
                </p>
            </header>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- Painel de Reprodução e Dica Visual -->
                <div class="bg-white p-6 rounded-xl border border-gray-200 shadow-sm flex flex-col items-center justify-center text-center space-y-4 md:col-span-1">
                    <button id="g2-play-btn" onclick="playCurrentAnimalSound()" class="w-full py-5 bg-green-600 hover:bg-green-700 text-white font-bold rounded-xl shadow-lg text-lg flex items-center justify-center gap-3 transition-transform active:scale-95">
                        <span class="text-3xl">🔊</span> Tocar Som do Animal
                    </button>
                    
                    <button onclick="describeCurrentSoundHint()" class="w-full py-2 bg-slate-200 hover:bg-slate-300 text-slate-800 font-semibold rounded-lg text-sm">
                        💡 Ouvir Dica Descritiva em Voz
                    </button>

                    <!-- Feedback Visual da Onda Sonora -->
                    <div class="w-full bg-slate-900 p-4 rounded-xl border border-slate-700 text-left space-y-2">
                        <span class="text-xs font-bold text-slate-400 block uppercase tracking-wider">Onda Sonora (Visualizer):</span>
                        <div class="h-16 flex items-center justify-center gap-1.5" id="g2-visualizer">
                            <div class="w-3 bg-green-500 rounded-full h-4 transition-all duration-150" id="g2-bar1"></div>
                            <div class="w-3 bg-green-500 rounded-full h-8 transition-all duration-150" id="g2-bar2"></div>
                            <div class="w-3 bg-green-500 rounded-full h-12 transition-all duration-150" id="g2-bar3"></div>
                            <div class="w-3 bg-green-500 rounded-full h-6 transition-all duration-150" id="g2-bar4"></div>
                            <div class="w-3 bg-green-500 rounded-full h-10 transition-all duration-150" id="g2-bar5"></div>
                        </div>
                        <p id="g2-freq-hint" class="text-xs text-yellow-300 font-mono mt-2 italic">Dica visual: Pressione o botão para escutar o som.</p>
                    </div>
                </div>

                <!-- Opções de Escolha -->
                <div class="bg-white p-6 rounded-xl border border-gray-200 shadow-sm space-y-4 md:col-span-2 flex flex-col justify-between">
                    <div>
                        <h3 class="font-bold text-lg text-slate-800 mb-3">Qual é este animal?</h3>
                        <div id="g2-options" class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <!-- Botões de escolha dos animais via JS -->
                        </div>
                    </div>

                    <div class="pt-4 border-t border-gray-200 flex justify-between items-center">
                        <span class="text-sm font-semibold">Pontuação: <span id="g2-score" class="text-xl font-extrabold text-green-600">0</span></span>
                        <button onclick="initGame2()" class="px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white font-bold rounded-lg shadow">Próximo Som ➔</button>
                    </div>
                </div>
            </div>
        </section>


        <!-- ========================================== -->
        <!-- JOGO 3: TRACING / DESENHO DE FORMAS        -->
        <!-- ========================================== -->
        <section id="game3" class="game-section space-y-6 hidden" aria-labelledby="g3-heading">
            <header class="bg-white p-5 rounded-xl border border-gray-200 shadow-sm flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                <div>
                    <h2 id="g3-heading" class="text-2xl font-bold text-slate-800 flex items-center gap-2">
                        ✏️ Jogo 3: Tracing & Coordenação Motora Fina
                    </h2>
                    <p class="text-gray-600 text-sm mt-1">
                        Siga a linha tracejada com o ponteiro ou toque. O tom de áudio guia indica sua precisão!
                    </p>
                </div>

                <!-- Controles de Ajuste de Tolerância Motora -->
                <div class="flex items-center gap-3 bg-slate-100 p-2.5 rounded-lg border border-slate-300">
                    <label for="g3-tolerance" class="text-xs font-bold text-slate-700">Tolerância a Tremores:</label>
                    <input type="range" id="g3-tolerance" min="15" max="50" value="30" class="w-24 accent-green-600 cursor-pointer" onchange="updateTolerance(this.value)">
                    <span id="g3-tol-val" class="text-xs font-extrabold text-slate-800 w-8">Média</span>
                </div>
            </header>

            <div class="grid grid-cols-1 lg:grid-cols-4 gap-6">
                <!-- Seletor de Formas -->
                <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-sm space-y-3 lg:col-span-1">
                    <h3 class="font-bold text-sm text-slate-700 uppercase tracking-wide">Escolha a Forma:</h3>
                    <div class="flex flex-col gap-2">
                        <button onclick="selectShape('circle')" id="btn-shape-circle" class="px-4 py-3 bg-green-700 text-white font-bold rounded-lg shadow text-left flex items-center justify-between">
                            ⚪ Círculo <span>➔</span>
                        </button>
                        <button onclick="selectShape('square')" id="btn-shape-square" class="px-4 py-3 bg-slate-200 hover:bg-slate-300 text-slate-800 font-bold rounded-lg text-left flex items-center justify-between">
                            🟦 Quadrado <span>➔</span>
                        </button>
                        <button onclick="selectShape('triangle')" id="btn-shape-triangle" class="px-4 py-3 bg-slate-200 hover:bg-slate-300 text-slate-800 font-bold rounded-lg text-left flex items-center justify-between">
                            🔺 Triângulo <span>➔</span>
                        </button>
                        <button onclick="selectShape('star')" id="btn-shape-star" class="px-4 py-3 bg-slate-200 hover:bg-slate-300 text-slate-800 font-bold rounded-lg text-left flex items-center justify-between">
                            ⭐ Estrela <span>➔</span>
                        </button>
                    </div>

                    <div class="pt-4 border-t border-gray-200 text-xs text-gray-600 space-y-1">
                        <p><strong>💡 Dica de Acessibilidade:</strong></p>
                        <p>Um som contínuo suave tocará enquanto você se mantiver na rota correta da guia.</p>
                    </div>
                </div>

                <!-- Canvas de Desenho -->
                <div class="bg-white p-4 rounded-xl border border-gray-200 shadow-sm lg:col-span-3 flex flex-col items-center">
                    <div class="w-full flex justify-between items-center mb-2">
                        <span class="text-sm font-semibold">Progresso do Traço: <span id="g3-progress" class="font-bold text-green-600">0%</span></span>
                        <button onclick="resetCanvas()" class="px-3 py-1 bg-red-100 hover:bg-red-200 text-red-700 text-xs font-bold rounded border border-red-300">
                            🗑️ Limpar Traço
                        </button>
                    </div>

                    <div class="relative w-full aspect-square max-w-[500px] bg-slate-50 border-2 border-slate-300 rounded-xl overflow-hidden shadow-inner">
                        <canvas id="tracing-canvas" class="w-full h-full cursor-crosshair"></canvas>
                    </div>
                </div>
            </div>
        </section>


        <!-- ========================================== -->
        <!-- JOGO 4: REABILITAÇÃO ESPACIAL / ALCANCE    -->
        <!-- ========================================== -->
        <section id="game4" class="game-section space-y-6 hidden" aria-labelledby="g4-heading">
            <header class="bg-white p-5 rounded-xl border border-gray-200 shadow-sm flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                <div>
                    <h2 id="g4-heading" class="text-2xl font-bold text-slate-800 flex items-center gap-2">
                        🎯 Jogo 4: Reabilitação de Mobilidade Espacial
                    </h2>
                    <p class="text-gray-600 text-sm mt-1">
                        Toque ou clique nos alvos que surgem na área de treino. Áudio estereofônico (Panning 3D) indica a posição visual do alvo!
                    </p>
                </div>

                <div class="flex items-center gap-2">
                    <span class="text-sm font-bold">Alvos Acertados: <span id="g4-score" class="text-xl font-extrabold text-green-600">0</span></span>
                    <button onclick="startSpatialGame()" class="px-4 py-2 bg-green-600 hover:bg-green-700 text-white font-bold rounded-lg shadow">
                        ▶️ Iniciar / Reiniciar
                    </button>
                </div>
            </header>

            <!-- Área de Treino de Alcance -->
            <div id="spatial-area" class="relative w-full h-[450px] bg-slate-900 rounded-2xl border-4 border-slate-700 overflow-hidden shadow-2xl flex items-center justify-center" role="region" aria-label="Área de treino de alcance motor espacial">
                
                <p id="g4-start-prompt" class="text-slate-400 font-semibold text-lg text-center px-4">
                    Clique em "Iniciar" ou pressione Enter para começar a reabilitação de alcance motor.
                </p>

                <!-- Alvo Dinâmico Injetado por JS -->
            </div>
        </section>

    </main>

    <!-- Rodapé informativo -->
    <footer class="bg-slate-900 text-slate-400 text-xs py-4 px-6 text-center border-t border-slate-800">
        <p>Quadrologia de Games for Health - Projeto de Acessibilidade Web Universal | Desenvolvido com HTML5, CSS3 & Web Audio API Pure</p>
    </footer>

    <!-- LÓGICA EM JAVASCRIPT PURO -->
    <script>
        /* ==========================================================================
           CONFIGURAÇÕES E ESTADOS GLOBAIS DE ACESSIBILIDADE E SÍNTESE DE ÁUDIO
           ========================================================================== */
        let speechEnabled = true;
        let audioCtx = null;
        let currentFontSizeLevel = 0;

        // Inicializador do Web Audio Context
        function getAudioContext() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
            return audioCtx;
        }

        // Leitor por voz usando Web Speech API
        function speak(text, interrupt = true) {
            // Anúncio via aria-live para leitores de tela
            const announcer = document.getElementById('sr-announcer');
            if (announcer) announcer.textContent = text;

            if (!speechEnabled || !('speechSynthesis' in window)) return;
            
            if (interrupt) {
                window.speechSynthesis.cancel();
            }
            
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'pt-BR';
            utterance.rate = 1.0;
            window.speechSynthesis.speak(utterance);
        }

        function toggleSpeech() {
            speechEnabled = !speechEnabled;
            const statusElem = document.getElementById('speech-status');
            const iconElem = document.getElementById('speech-icon');
            const btnElem = document.getElementById('btn-speech');

            if (speechEnabled) {
                statusElem.textContent = 'LIGADA';
                iconElem.textContent = '🔊';
                btnElem.setAttribute('aria-pressed', 'true');
                btnElem.className = 'px-3 py-1.5 bg-green-700 hover:bg-green-600 rounded-lg text-sm font-semibold flex items-center gap-1.5';
                speak('Voz guia ativada');
            } else {
                statusElem.textContent = 'DESLIGADA';
                iconElem.textContent = '🔇';
                btnElem.setAttribute('aria-pressed', 'false');
                btnElem.className = 'px-3 py-1.5 bg-slate-700 hover:bg-slate-600 rounded-lg text-sm font-semibold flex items-center gap-1.5';
                window.speechSynthesis.cancel();
            }
        }

        function toggleHighContrast() {
            document.body.classList.toggle('high-contrast');
            const btn = document.getElementById('btn-contrast');
            const isHigh = document.body.classList.contains('high-contrast');
            btn.setAttribute('aria-pressed', isHigh ? 'true' : 'false');
            speak(isHigh ? 'Alto contraste ativado' : 'Alto contraste desativado');
        }

        function changeFontSize(delta) {
            currentFontSizeLevel += delta;
            if (currentFontSizeLevel > 4) currentFontSizeLevel = 4;
            if (currentFontSizeLevel < -1) currentFontSizeLevel = -1;
            
            const sizes = ['text-sm', 'text-base', 'text-lg', 'text-xl', 'text-2xl'];
            const baseIndex = 1 + currentFontSizeLevel;
            document.documentElement.style.fontSize = `${100 + (currentFontSizeLevel * 15)}%`;
            speak(`Tamanho do texto ajustado`);
        }

        function resetFontSize() {
            currentFontSizeLevel = 0;
            document.documentElement.style.fontSize = '100%';
            speak('Tamanho do texto restaurado ao normal');
        }

        // Troca de Abas dos Jogos
        function switchTab(gameId) {
            document.querySelectorAll('.game-section').forEach(sec => sec.classList.add('hidden'));
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('bg-green-600', 'text-white', 'shadow');
                btn.classList.add('bg-slate-700', 'hover:bg-slate-600', 'text-slate-200');
                btn.setAttribute('aria-selected', 'false');
            });

            const activeSection = document.getElementById(gameId);
            activeSection.classList.remove('hidden');

            const activeTab = document.getElementById(`tab-${gameId}`);
            activeTab.classList.remove('bg-slate-700', 'hover:bg-slate-600', 'text-slate-200');
            activeTab.classList.add('bg-green-600', 'text-white', 'shadow');
            activeTab.setAttribute('aria-selected', 'true');

            // Inicializações específicas ao abrir cada jogo
            if (gameId === 'game1') {
                speak('Jogo 1: Seleção de Cores e Discriminação Visual');
                initGame1();
            } else if (gameId === 'game2') {
                speak('Jogo 2: Identificação Auditiva de Sons de Animais');
                initGame2();
            } else if (gameId === 'game3') {
                speak('Jogo 3: Tracing e Desenho de Formas Tracejadas');
                initGame3();
            } else if (gameId === 'game4') {
                speak('Jogo 4: Reabilitação de Mobilidade Espacial');
            }
        }

        // Sintetizador Genérico de Feedback Sonoro
        function playFeedbackSound(type) {
            const ctx = getAudioContext();
            const osc = ctx.createOscillator();
            const gain = ctx.createGain();
            osc.connect(gain);
            gain.connect(ctx.destination);

            if (type === 'correct') {
                osc.type = 'sine';
                osc.frequency.setValueAtTime(523.25, ctx.currentTime); // C5
                osc.frequency.exponentialRampToValueAtTime(659.25, ctx.currentTime + 0.15); // E5
                gain.gain.setValueAtTime(0.2, ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.3);
                osc.start();
                osc.stop(ctx.currentTime + 0.3);
            } else if (type === 'wrong') {
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(220, ctx.currentTime); // A3
                osc.frequency.linearRampToValueAtTime(110, ctx.currentTime + 0.25); // A2
                gain.gain.setValueAtTime(0.2, ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.3);
                osc.start();
                osc.stop(ctx.currentTime + 0.3);
            }
        }


        /* ==========================================================================
           LÓGICA JOGO 1: SELEÇÃO DE CORES E GRUPOS (VISÃO / DALTONISMO)
           ========================================================================== */
        const g1Colors = [
            { id: 'red', name: 'Vermelho', bg: 'bg-red-500', pattern: 'pattern-stripe', icon: '🔴', symbol: 'Listras (Vermelho)' },
            { id: 'blue', name: 'Azul', bg: 'bg-blue-500', pattern: 'pattern-circle', icon: '🔵', symbol: 'Círculos (Azul)' },
            { id: 'green', name: 'Verde', bg: 'bg-green-500', pattern: 'pattern-cross', icon: '🟢', symbol: 'Cruzado (Verde)' },
            { id: 'yellow', name: 'Amarelo', bg: 'bg-yellow-400', pattern: 'pattern-dots', icon: '🟡', symbol: 'Pontilhado (Amarelo)' }
        ];

        let g1TargetColor = null;
        let g1Score = 0;
        let currentDaltonismFilter = 'none';

        function setDaltonismMode(mode) {
            currentDaltonismFilter = mode;
            ['none', 'deut', 'prot', 'trit'].forEach(m => {
                const btn = document.getElementById(`daltonism-${m}`);
                if (btn) btn.className = 'px-2.5 py-1 text-xs font-bold bg-slate-200 text-slate-800 rounded hover:bg-slate-300';
            });
            const activeBtn = document.getElementById(`daltonism-${mode === 'deuteranopia' ? 'deut' : mode === 'protanopia' ? 'prot' : mode === 'tritanopia' ? 'trit' : 'none'}`);
            if (activeBtn) activeBtn.className = 'px-2.5 py-1 text-xs font-bold bg-slate-800 text-white rounded';

            speak(`Modo visual alterado para ${mode === 'none' ? 'padrão' : mode}`);
            renderGame1Grid();
        }

        function initGame1() {
            g1TargetColor = g1Colors[Math.floor(Math.random() * g1Colors.length)];
            const targetCard = document.getElementById('g1-target-card');
            const targetText = document.getElementById('g1-target-text');
            const targetIcon = document.getElementById('g1-target-icon');

            targetCard.className = `px-4 py-2 rounded-lg font-bold text-white shadow flex items-center gap-2 border-2 border-slate-900 ${g1TargetColor.bg} ${g1TargetColor.pattern}`;
            targetText.textContent = `${g1TargetColor.name} (${g1TargetColor.symbol})`;
            targetIcon.textContent = g1TargetColor.icon;

            speak(`Novo Alvo: Selecione todos os blocos da cor ${g1TargetColor.name}, com padrão ${g1TargetColor.symbol}`);
            renderGame1Grid();
        }

        function renderGame1Grid() {
            const grid = document.getElementById('g1-grid');
            grid.innerHTML = '';

            // Gerar 8 blocos na grade
            const blocks = [];
            blocks.push(g1TargetColor, g1TargetColor); // Garante ao menos dois corretos
            while (blocks.length < 8) {
                const randomCol = g1Colors[Math.floor(Math.random() * g1Colors.length)];
                blocks.push(randomCol);
            }
            // Embaralhar
            blocks.sort(() => Math.random() - 0.5);

            blocks.forEach((col, idx) => {
                const btn = document.createElement('button');
                btn.className = `h-28 rounded-xl border-4 border-slate-800 shadow-md flex flex-col items-center justify-center p-2 transition-transform active:scale-95 ${col.bg} ${col.pattern}`;
                btn.setAttribute('aria-label', `Bloco ${idx + 1}: ${col.name}, Padrão ${col.symbol}`);
                
                // Texto de alto contraste adicional em cada bloco para daltonismo
                btn.innerHTML = `
                    <span class="bg-slate-900 text-white px-2 py-1 rounded text-xs font-extrabold shadow">
                        ${col.icon} ${col.name}
                    </span>
                    <span class="text-[10px] bg-white/90 text-slate-900 px-1.5 py-0.5 rounded font-bold mt-1">
                        ${col.symbol}
                    </span>
                `;

                btn.onclick = () => handleGame1Selection(col, btn);
                grid.appendChild(btn);
            });
        }

        function handleGame1Selection(selectedCol, element) {
            if (selectedCol.id === g1TargetColor.id) {
                g1Score += 10;
                document.getElementById('g1-score').textContent = g1Score;
                playFeedbackSound('correct');
                speak(`Correto! ${selectedCol.name}`);
                element.style.opacity = '0.3';
                element.disabled = true;
            } else {
                playFeedbackSound('wrong');
                speak(`Incorreto. Este é ${selectedCol.name}. Procure por ${g1TargetColor.name}`);
            }
        }


        /* ==========================================================================
           LÓGICA JOGO 2: IDENTIFICAÇÃO DE SONS DE ANIMAIS (AUDIO VIA WEB AUDIO API)
           ========================================================================== */
        const g2Animals = [
            { id: 'cat', name: 'Gato', icon: '🐱', hint: 'Miado agudo com tom descendente (1000 Hz a 500 Hz).' },
            { id: 'dog', name: 'Cachorro', icon: '🐶', hint: 'Latido grave e rítmico em pulsos curtos (300 Hz).' },
            { id: 'bird', name: 'Pássaro', icon: '🐦', hint: 'Piu super agudo em rápida sucessão (2500 Hz).' },
            { id: 'cow', name: 'Vaca', icon: '🐮', hint: 'Mumu grave e sustentado de longa duração (150 Hz).' }
        ];

        let g2CurrentAnimal = null;
        let g2Score = 0;

        function initGame2() {
            g2CurrentAnimal = g2Animals[Math.floor(Math.random() * g2Animals.length)];
            document.getElementById('g2-freq-hint').textContent = `Dica visual: Pronto para reproduzir o som.`;
            
            const optionsDiv = document.getElementById('g2-options');
            optionsDiv.innerHTML = '';

            g2Animals.forEach(animal => {
                const btn = document.createElement('button');
                btn.className = 'py-4 px-5 bg-slate-100 hover:bg-slate-200 border-2 border-slate-300 rounded-xl font-bold text-lg text-slate-800 flex items-center justify-between transition-transform active:scale-95';
                btn.innerHTML = `<span>${animal.icon} ${animal.name}</span> <span class="text-slate-400">➡️</span>`;
                btn.onclick = () => checkGame2Answer(animal);
                optionsDiv.appendChild(btn);
            });

            speak('Novo som de animal pronto. Pressione o botão para escutar.');
        }

        // Sintetizador Procedural de Sons de Animais
        function playCurrentAnimalSound() {
            if (!g2CurrentAnimal) return;
            const ctx = getAudioContext();
            const now = ctx.currentTime;

            // Animação da barra visualizer
            animateVisualizer();

            if (g2CurrentAnimal.id === 'cat') { // Miado
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(800, now);
                osc.frequency.exponentialRampToValueAtTime(1200, now + 0.2);
                osc.frequency.exponentialRampToValueAtTime(400, now + 0.6);

                gain.gain.setValueAtTime(0.01, now);
                gain.gain.linearRampToValueAtTime(0.3, now + 0.1);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.6);

                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start(now);
                osc.stop(now + 0.6);
            } 
            else if (g2CurrentAnimal.id === 'dog') { // Latido (2 pulsos)
                [0, 0.25].forEach(delay => {
                    const osc = ctx.createOscillator();
                    const gain = ctx.createGain();
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(350, now + delay);
                    osc.frequency.exponentialRampToValueAtTime(120, now + delay + 0.15);

                    gain.gain.setValueAtTime(0.4, now + delay);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + delay + 0.15);

                    osc.connect(gain);
                    gain.connect(ctx.destination);
                    osc.start(now + delay);
                    osc.stop(now + delay + 0.15);
                });
            }
            else if (g2CurrentAnimal.id === 'bird') { // Piu Piu
                [0, 0.15, 0.3].forEach(delay => {
                    const osc = ctx.createOscillator();
                    const gain = ctx.createGain();
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(2200, now + delay);
                    osc.frequency.linearRampToValueAtTime(2800, now + delay + 0.08);

                    gain.gain.setValueAtTime(0.2, now + delay);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + delay + 0.08);

                    osc.connect(gain);
                    gain.connect(ctx.destination);
                    osc.start(now + delay);
                    osc.stop(now + delay + 0.08);
                });
            }
            else if (g2CurrentAnimal.id === 'cow') { // Mumu
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(140, now);
                osc.frequency.linearRampToValueAtTime(120, now + 0.8);

                gain.gain.setValueAtTime(0.01, now);
                gain.gain.linearRampToValueAtTime(0.4, now + 0.2);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.9);

                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start(now);
                osc.stop(now + 0.9);
            }

            document.getElementById('g2-freq-hint').textContent = `Dica visual: ${g2CurrentAnimal.hint}`;
        }

        function animateVisualizer() {
            const bars = [1, 2, 3, 4, 5].map(i => document.getElementById(`g2-bar${i}`));
            let count = 0;
            const interval = setInterval(() => {
                bars.forEach(bar => {
                    const h = Math.floor(Math.random() * 48) + 10;
                    bar.style.height = `${h}px`;
                });
                count++;
                if (count > 10) {
                    clearInterval(interval);
                    bars.forEach(bar => bar.style.height = '16px');
                }
            }, 60);
        }

        function describeCurrentSoundHint() {
            if (g2CurrentAnimal) {
                speak(`Dica do som: ${g2CurrentAnimal.hint}`);
            }
        }

        function checkGame2Answer(selected) {
            if (selected.id === g2CurrentAnimal.id) {
                g2Score += 10;
                document.getElementById('g2-score').textContent = g2Score;
                playFeedbackSound('correct');
                speak(`Correto! É o som do ${selected.name}.`);
                setTimeout(initGame2, 1200);
            } else {
                playFeedbackSound('wrong');
                speak(`Incorreto. Você selecionou ${selected.name}. Tente novamente.`);
            }
        }


        /* ==========================================================================
           LÓGICA JOGO 3: TRACING E DESENHO DE FORMAS (COORDENAÇÃO MOTORA FINA)
           ========================================================================== */
        let tracingCanvas, tracingCtx;
        let currentShape = 'circle';
        let motorTolerance = 30; // Tolerância a tremores em pixels
        let isDrawing = false;
        let pathPoints = [];
        let continuousOsc = null;
        let continuousGain = null;

        function initGame3() {
            tracingCanvas = document.getElementById('tracing-canvas');
            tracingCtx = tracingCanvas.getContext('2d');
            resizeCanvas();
            window.addEventListener('resize', resizeCanvas);

            // Setup de eventos de ponteiro/toque
            tracingCanvas.addEventListener('pointerdown', startTracing);
            tracingCanvas.addEventListener('pointermove', drawTracing);
            tracingCanvas.addEventListener('pointerup', stopTracing);
            tracingCanvas.addEventListener('pointerleave', stopTracing);

            selectShape('circle');
        }

        function resizeCanvas() {
            if (!tracingCanvas) return;
            const rect = tracingCanvas.parentElement.getBoundingClientRect();
            tracingCanvas.width = rect.width;
            tracingCanvas.height = rect.height;
            drawShapeGuide();
        }

        function updateTolerance(val) {
            motorTolerance = parseInt(val);
            const label = document.getElementById('g3-tol-val');
            if (motorTolerance < 25) label.textContent = 'Alta';
            else if (motorTolerance < 40) label.textContent = 'Média';
            else label.textContent = 'Ampla';
            speak(`Tolerância motora ajustada para ${label.textContent}`);
        }

        function selectShape(shape) {
            currentShape = shape;
            ['circle', 'square', 'triangle', 'star'].forEach(s => {
                const btn = document.getElementById(`btn-shape-${s}`);
                if (btn) btn.className = 'px-4 py-3 bg-slate-200 hover:bg-slate-300 text-slate-800 font-bold rounded-lg text-left flex items-center justify-between';
            });
            const activeBtn = document.getElementById(`btn-shape-${shape}`);
            if (activeBtn) activeBtn.className = 'px-4 py-3 bg-green-700 text-white font-bold rounded-lg shadow text-left flex items-center justify-between';

            speak(`Forma selecionada: ${shape}`);
            resetCanvas();
        }

        function resetCanvas() {
            pathPoints = [];
            document.getElementById('g3-progress').textContent = '0%';
            drawShapeGuide();
        }

        function drawShapeGuide() {
            if (!tracingCtx) return;
            const w = tracingCanvas.width;
            const h = tracingCanvas.height;
            const cx = w / 2;
            const cy = h / 2;
            const r = Math.min(w, h) * 0.35;

            tracingCtx.clearRect(0, 0, w, h);

            // Linha guia tracejada larga de alto contraste
            tracingCtx.beginPath();
            tracingCtx.setLineDash([12, 12]);
            tracingCtx.lineWidth = 8;
            tracingCtx.strokeStyle = '#94a3b8'; // Slate 400

            if (currentShape === 'circle') {
                tracingCtx.arc(cx, cy, r, 0, Math.PI * 2);
            } else if (currentShape === 'square') {
                tracingCtx.rect(cx - r, cy - r, r * 2, r * 2);
            } else if (currentShape === 'triangle') {
                tracingCtx.moveTo(cx, cy - r);
                tracingCtx.lineTo(cx + r, cy + r);
                tracingCtx.lineTo(cx - r, cy + r);
                tracingCtx.closePath();
            } else if (currentShape === 'star') {
                const spikes = 5;
                const outerRadius = r;
                const innerRadius = r / 2;
                let rot = Math.PI / 2 * 3;
                let x = cx;
                let y = cy;
                const step = Math.PI / spikes;

                tracingCtx.moveTo(cx, cy - outerRadius);
                for (let i = 0; i < spikes; i++) {
                    x = cx + Math.cos(rot) * outerRadius;
                    y = cy + Math.sin(rot) * outerRadius;
                    tracingCtx.lineTo(x, y);
                    rot += step;

                    x = cx + Math.cos(rot) * innerRadius;
                    y = cy + Math.sin(rot) * innerRadius;
                    tracingCtx.lineTo(x, y);
                    rot += step;
                }
                tracingCtx.closePath();
            }
            tracingCtx.stroke();
            tracingCtx.setLineDash([]); // Reset dash
        }

        function startTracing(e) {
            isDrawing = true;
            pathPoints = [];
            startContinuousTone();
            drawTracing(e);
        }

        function drawTracing(e) {
            if (!isDrawing) return;

            const rect = tracingCanvas.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;

            pathPoints.push({ x, y });

            // Redesenhar guia e caminho desenhado pelo usuário
            drawShapeGuide();

            tracingCtx.beginPath();
            tracingCtx.lineWidth = 10;
            tracingCtx.strokeStyle = '#22c55e'; // Verde brilhante
            tracingCtx.lineCap = 'round';
            tracingCtx.lineJoin = 'round';

            pathPoints.forEach((pt, idx) => {
                if (idx === 0) tracingCtx.moveTo(pt.x, pt.y);
                else tracingCtx.lineTo(pt.x, pt.y);
            });
            tracingCtx.stroke();

            // Verificar precisão e atualizar som contínuo
            const isOnTrack = checkTracingAccuracy(x, y);
            updateContinuousTone(isOnTrack, y);

            // Atualizar progresso
            const progress = Math.min(100, Math.floor((pathPoints.length / 80) * 100));
            document.getElementById('g3-progress').textContent = `${progress}%`;
        }

        function stopTracing() {
            if (!isDrawing) return;
            isDrawing = false;
            stopContinuousTone();

            const progress = parseInt(document.getElementById('g3-progress').textContent);
            if (progress >= 80) {
                playFeedbackSound('correct');
                speak('Parabéns! Traço concluído com ótima coordenação motor!');
            }
        }

        function checkTracingAccuracy(x, y) {
            // Checagem simplificada de proximidade do ponto central ou guia
            const w = tracingCanvas.width;
            const h = tracingCanvas.height;
            const cx = w / 2;
            const cy = h / 2;
            const distFromCenter = Math.hypot(x - cx, y - cy);
            const expectedDist = Math.min(w, h) * 0.35;

            if (currentShape === 'circle') {
                return Math.abs(distFromCenter - expectedDist) <= motorTolerance;
            }
            return true; // Simplificado para outras formas
        }

        function startContinuousTone() {
            const ctx = getAudioContext();
            continuousOsc = ctx.createOscillator();
            continuousGain = ctx.createGain();
            continuousOsc.type = 'sine';
            continuousOsc.frequency.setValueAtTime(440, ctx.currentTime);
            continuousGain.gain.setValueAtTime(0.05, ctx.currentTime);

            continuousOsc.connect(continuousGain);
            continuousGain.connect(ctx.destination);
            continuousOsc.start();
        }

        function updateContinuousTone(isOnTrack, yVal) {
            if (!continuousOsc || !audioCtx) return;
            const freq = 300 + (yVal % 400);
            continuousOsc.frequency.setValueAtTime(freq, audioCtx.currentTime);
            continuousGain.gain.setValueAtTime(isOnTrack ? 0.1 : 0.01, audioCtx.currentTime);
        }

        function stopContinuousTone() {
            if (continuousGain && audioCtx) {
                continuousGain.gain.setValueAtTime(0, audioCtx.currentTime);
            }
            if (continuousOsc) {
                try { continuousOsc.stop(); } catch(e){}
                continuousOsc = null;
            }
        }


        /* ==========================================================================
           LÓGICA JOGO 4: REABILITAÇÃO ESPACIAL / ALCANCE (PANNING ESTEREOFÔNICO)
           ========================================================================== */
        let g4Score = 0;
        let g4TargetElement = null;

        function startSpatialGame() {
            const prompt = document.getElementById('g4-start-prompt');
            if (prompt) prompt.style.display = 'none';

            g4Score = 0;
            document.getElementById('g4-score').textContent = g4Score;
            spawnSpatialTarget();
            speak('Treino de alcance espacial iniciado. Localize os alvos visuais e sonoros.');
        }

        function spawnSpatialTarget() {
            const area = document.getElementById('spatial-area');
            if (g4TargetElement) g4TargetElement.remove();

            const areaRect = area.getBoundingClientRect();
            const size = Math.floor(Math.random() * 40) + 60; // Tamanhos amigáveis para motor 60px - 100px
            const maxX = areaRect.width - size - 20;
            const maxY = areaRect.height - size - 20;

            const posX = Math.max(20, Math.floor(Math.random() * maxX));
            const posY = Math.max(20, Math.floor(Math.random() * maxY));

            // Calcular Panning Estéreo (-1.0 Esquerda a +1.0 Direita)
            const panValue = ((posX / areaRect.width) * 2) - 1;

            const target = document.createElement('button');
            target.className = 'absolute rounded-full bg-yellow-400 border-4 border-white shadow-2xl flex items-center justify-center text-slate-900 font-extrabold text-2xl transition-transform hover:scale-110 active:scale-95 animate-pulse';
            target.style.width = `${size}px`;
            target.style.height = `${size}px`;
            target.style.left = `${posX}px`;
            target.style.top = `${posY}px`;
            target.setAttribute('aria-label', `Alvo espacial localizado na posição ${posX < areaRect.width / 2 ? 'esquerda' : 'direita'}`);
            target.innerHTML = '🎯';

            target.onclick = () => {
                g4Score++;
                document.getElementById('g4-score').textContent = g4Score;
                playFeedbackSound('correct');
                speak(`Alvo atingido! Total: ${g4Score}`);
                spawnSpatialTarget();
            };

            area.appendChild(target);
            g4TargetElement = target;

            // Emitir pulso sonoro estereofônico de localização espacial
            playStereoPulse(panValue);
        }

        function playStereoPulse(panValue) {
            const ctx = getAudioContext();
            const osc = ctx.createOscillator();
            const gain = ctx.createGain();

            // Usar StereoPannerNode se suportado
            if ('createStereoPanner' in ctx) {
                const panner = ctx.createStereoPanner();
                panner.pan.setValueAtTime(panValue, ctx.currentTime);
                osc.connect(gain);
                gain.connect(panner);
                panner.connect(ctx.destination);
            } else {
                osc.connect(gain);
                gain.connect(ctx.destination);
            }

            osc.type = 'sine';
            osc.frequency.setValueAtTime(440, ctx.currentTime);
            gain.gain.setValueAtTime(0.2, ctx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.4);

            osc.start();
            osc.stop(ctx.currentTime + 0.4);
        }


        /* ==========================================================================
           INICIALIZAÇÃO GLOBAL AO CARREGAR A PÁGINA
           ========================================================================== */
        window.onload = function() {
            initGame1();
            initGame2();
            initGame3();
        };
    </script>
</body>
</html>
