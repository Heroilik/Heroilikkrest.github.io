<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎵 Doodle Jump - МУЗЫКА + НОВЫЕ СКИНЫ</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #0f0c1f 0%, #1a1a2e 50%, #16213e 100%);
            font-family: 'Arial', sans-serif;
            position: relative;
            overflow: hidden;
        }
        
        /* Космический фон */
        .stars, .twinkling, .nebula {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }
        
        .stars {
            background: #000 url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIzMDAiIGhlaWdodD0iMzAwIj48Y2lyY2xlIGN4PSI2IiBjeT0iMTQiIHI9IjEiIGZpbGw9IndoaXRlIiAvPjxjaXJjbGUgY3g9IjE2MCIgY3k9IjYwIiByPSIxLjUiIGZpbGw9IndoaXRlIiAvPjxjaXJjbGUgY3g9IjQwIiBjeT0iMjAwIiByPSIxLjIiIGZpbGw9IndoaXRlIiAvPjxjaXJjbGUgY3g9IjI4MCIgY3k9IjE4MCIgcj0iMSIgZmlsbD0id2hpdGUiIC8+PGNpcmNsZSBjeD0iMjAiIGN5PSI4MCIgcj0iMSIgZmlsbD0id2hpdGUiIC8+PGNpcmNsZSBjeD0iMjUwIiBjeT0iMjUwIiByPSIxLjgiIGZpbGw9IndoaXRlIiAvPjxjaXJjbGUgY3g9IjcwIiBjeT0iMTUwIiByPSIxLjMiIGZpbGw9IndoaXRlIiAvPjxjaXJjbGUgY3g9IjE5MCIgY3k9IjkwIiByPSIxIiBmaWxsPSJ3aGl0ZSIgLz48L3N2Zz4=') repeat;
            animation: starsMove 200s linear infinite;
            opacity: 0.8;
        }
        
        .twinkling {
            background: transparent url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MDAiIGhlaWdodD0iNDAwIj48Y2lyY2xlIGN4PSI1MCIgY3k9IjUwIiByPSIzIiBmaWxsPSJ3aGl0ZSIgb3BhY2l0eT0iMC44IiAvPjxjaXJjbGUgY3g9IjIwMCIgY3k9IjE1MCIgcj0iNCIgZmlsbD0iY3lhbiIgb3BhY2l0eT0iMC42IiAvPjxjaXJjbGUgY3g9IjMwMCIgY3k9IjI1MCIgcj0iMiIgZmlsbD0ibWFnZW50YSIgb3BhY2l0eT0iMC43IiAvPjxjaXJjbGUgY3g9IjE1MCIgY3k9IjMwMCIgcj0iMyIgZmlsbD0iYmx1ZSIgb3BhY2l0eT0iMC41IiAvPjwvc3ZnPg==') repeat;
            animation: twinkling 3s ease-in-out infinite;
        }
        
        .nebula {
            background: radial-gradient(circle at 20% 30%, rgba(100, 0, 255, 0.1) 0%, transparent 30%),
                        radial-gradient(circle at 80% 70%, rgba(255, 0, 200, 0.1) 0%, transparent 40%),
                        radial-gradient(circle at 40% 80%, rgba(0, 200, 255, 0.1) 0%, transparent 50%);
            animation: nebulaMove 30s ease-in-out infinite alternate;
        }
        
        @keyframes starsMove {
            from { transform: translateY(0); }
            to { transform: translateY(-1000px); }
        }
        
        @keyframes twinkling {
            0% { opacity: 0.3; }
            50% { opacity: 1; }
            100% { opacity: 0.3; }
        }
        
        @keyframes nebulaMove {
            0% { transform: scale(1) rotate(0deg); }
            100% { transform: scale(1.2) rotate(5deg); }
        }
        
        .main-container {
            display: flex;
            gap: 20px;
            z-index: 10;
            position: relative;
        }
        
        #gameContainer {
            text-align: center;
            background: rgba(20, 20, 40, 0.3);
            backdrop-filter: blur(15px);
            padding: 20px;
            border-radius: 30px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.8), 0 0 30px rgba(100, 0, 255, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        /* Магазин */
        .shop-panel {
            width: 300px;
            background: rgba(20, 20, 40, 0.4);
            backdrop-filter: blur(15px);
            border-radius: 30px;
            padding: 20px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.8);
            color: white;
            max-height: 700px;
            overflow-y: auto;
        }
        
        .shop-header {
            text-align: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .shop-header h2 {
            font-size: 28px;
            background: linear-gradient(45deg, gold, orange);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 10px;
        }
        
        .player-coins {
            font-size: 24px;
            color: gold;
            text-shadow: 0 0 20px gold;
            background: rgba(0, 0, 0, 0.3);
            padding: 10px;
            border-radius: 50px;
            border: 1px solid rgba(255, 215, 0, 0.3);
        }
        
        .shop-section {
            margin-bottom: 25px;
        }
        
        .shop-section h3 {
            color: cyan;
            margin-bottom: 15px;
            font-size: 20px;
            text-shadow: 0 0 10px cyan;
        }
        
        .shop-item {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            padding: 12px;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 12px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s;
            cursor: pointer;
        }
        
        .shop-item:hover {
            transform: translateX(5px);
            background: rgba(255, 255, 255, 0.1);
            border-color: cyan;
        }
        
        .shop-item.owned {
            border-color: gold;
            background: rgba(255, 215, 0, 0.1);
            cursor: default;
        }
        
        .shop-item.owned:hover {
            transform: none;
        }
        
        .shop-item.selected {
            border-color: cyan;
            background: rgba(0, 255, 255, 0.1);
            box-shadow: 0 0 20px cyan;
        }
        
        .item-preview {
            width: 50px;
            height: 50px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            background: rgba(255, 255, 255, 0.1);
        }
        
        .item-info {
            flex: 1;
            text-align: left;
        }
        
        .item-name {
            font-weight: bold;
            margin-bottom: 3px;
        }
        
        .item-price {
            color: gold;
            font-size: 14px;
        }
        
        .item-price span {
            color: white;
            opacity: 0.7;
        }
        
        .buy-button {
            background: linear-gradient(45deg, gold, orange);
            border: none;
            color: black;
            font-weight: bold;
            padding: 5px 15px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s;
        }
        
        .buy-button:hover {
            transform: scale(1.1);
            box-shadow: 0 0 20px gold;
        }
        
        .buy-button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        canvas {
            border: 2px solid rgba(255, 255, 255, 0.2);
            background: transparent;
            display: block;
            margin: 0 auto;
            border-radius: 20px;
            box-shadow: 0 0 50px rgba(0, 255, 255, 0.2);
        }
        
        .stats-panel {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 10px 0;
            color: white;
            font-size: 18px;
            text-shadow: 0 0 15px cyan;
            gap: 20px;
        }
        
        #score, #highScore, #coins {
            padding: 8px 20px;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 30px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(5px);
            font-weight: bold;
            letter-spacing: 1px;
        }
        
        #coins {
            color: gold;
            text-shadow: 0 0 15px gold;
        }
        
        .music-control {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 5px 15px;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 30px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .music-control:hover {
            background: rgba(255, 255, 255, 0.1);
            border-color: cyan;
        }
        
        .music-icon {
            font-size: 20px;
        }
        
        .jetpack-timer {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 5px 15px;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 30px;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .jetpack-icon {
            font-size: 24px;
            filter: drop-shadow(0 0 10px orange);
        }
        
        #jetpackTimeFill {
            width: 100px;
            height: 10px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 5px;
            overflow: hidden;
        }
        
        #jetpackProgress {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, #ffaa00, #ff5500);
            border-radius: 5px;
            box-shadow: 0 0 15px #ffaa00;
            transition: width 0.1s;
        }
        
        #gameOver {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(10, 10, 20, 0.95);
            backdrop-filter: blur(20px);
            padding: 50px;
            border-radius: 40px;
            border: 2px solid rgba(255, 255, 255, 0.2);
            text-align: center;
            z-index: 1000;
            color: white;
            box-shadow: 0 0 100px rgba(255, 0, 255, 0.5);
            animation: gameOverGlow 2s ease-in-out infinite;
        }
        
        @keyframes gameOverGlow {
            0% { box-shadow: 0 0 50px cyan; }
            50% { box-shadow: 0 0 100px magenta; }
            100% { box-shadow: 0 0 50px cyan; }
        }
        
        #gameOver h2 {
            font-size: 60px;
            margin-bottom: 20px;
            background: linear-gradient(45deg, cyan, magenta, yellow);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: none;
        }
        
        #restartBtn {
            background: linear-gradient(45deg, cyan, magenta);
            color: white;
            border: none;
            padding: 15px 50px;
            font-size: 24px;
            border-radius: 60px;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 30px;
            font-weight: bold;
            letter-spacing: 2px;
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        
        #restartBtn:hover {
            transform: scale(1.1);
            box-shadow: 0 0 50px magenta;
        }
        
        .instructions {
            margin-top: 15px;
            color: rgba(255, 255, 255, 0.8);
            font-size: 14px;
            background: rgba(0, 0, 0, 0.4);
            padding: 12px;
            border-radius: 20px;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .instructions kbd {
            background: rgba(255, 255, 255, 0.15);
            border: 1px solid rgba(255, 255, 255, 0.3);
            border-radius: 8px;
            padding: 3px 10px;
            margin: 0 3px;
            color: cyan;
            font-weight: bold;
        }
        
        .powerup-indicator {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 48px;
            font-weight: bold;
            animation: powerupPop 1s ease-out forwards;
            pointer-events: none;
            z-index: 1000;
            text-shadow: 0 0 30px currentColor;
            white-space: nowrap;
        }
        
        @keyframes powerupPop {
            0% { transform: translate(-50%, -50%) scale(0); opacity: 0; }
            50% { transform: translate(-50%, -50%) scale(1.5); opacity: 1; }
            100% { transform: translate(-50%, -50%) scale(1); opacity: 0; }
        }
        
        .crack-effect {
            position: absolute;
            pointer-events: none;
            font-size: 30px;
            animation: crackFly 0.5s ease-out forwards;
            z-index: 100;
        }
        
        @keyframes crackFly {
            0% { transform: scale(1); opacity: 1; }
            100% { transform: scale(2) translateY(-50px); opacity: 0; }
        }
        
        #newRecord {
            color: gold;
            font-size: 28px;
            margin: 20px 0;
            animation: newRecordPulse 0.5s ease-in-out infinite;
            text-shadow: 0 0 30px gold;
        }
        
        @keyframes newRecordPulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .shop-panel::-webkit-scrollbar {
            width: 8px;
        }
        
        .shop-panel::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
        }
        
        .shop-panel::-webkit-scrollbar-thumb {
            background: linear-gradient(cyan, magenta);
            border-radius: 10px;
        }
    </style>
</head>
<body>
    <div class="stars"></div>
    <div class="twinkling"></div>
    <div class="nebula"></div>
    
    <div class="main-container">
        <div id="gameContainer">
            <div class="stats-panel">
                <div id="score">🎯 Счет: 0</div>
                <div id="coins">🪙 Монеты: 0</div>
                <div class="music-control" id="musicToggle">
                    <span class="music-icon" id="musicIcon">🔊</span>
                    <span>Музыка</span>
                </div>
                <div class="jetpack-timer">
                    <span class="jetpack-icon">🚀</span>
                    <div id="jetpackTimeFill">
                        <div id="jetpackProgress"></div>
                    </div>
                </div>
                <div id="highScore">🏆 Рекорд: 0</div>
            </div>
            
            <canvas id="gameCanvas" width="400" height="600"></canvas>
            
            <div class="instructions">
                <kbd>W</kbd> <kbd>A</kbd> <kbd>S</kbd> <kbd>D</kbd> или <kbd>← ↑ → ↓</kbd> - движение | 
                <kbd>ПРОБЕЛ</kbd> - активировать ранец | 
                <kbd>R</kbd> - рестарт
            </div>
        </div>

        <!-- Магазин -->
        <div class="shop-panel">
            <div class="shop-header">
                <h2>🛒 МАГАЗИН</h2>
                <div class="player-coins" id="shopCoins">🪙 0</div>
            </div>
            
            <!-- Скины шарика -->
            <div class="shop-section">
                <h3>🎨 СКИНЫ ШАРИКА</h3>
                <div id="ballSkins"></div>
            </div>
            
            <!-- Скины фона -->
            <div class="shop-section">
                <h3>🌌 СКИНЫ ФОНА</h3>
                <div id="backgroundSkins"></div>
            </div>
            
            <!-- Бонусы -->
            <div class="shop-section">
                <h3>⚡ БОНУСЫ</h3>
                <div id="bonuses"></div>
            </div>
        </div>
    </div>
    
    <div id="gameOver">
        <h2>GAME OVER</h2>
        <p style="font-size: 32px; margin: 20px 0;">Счет: <span id="finalScore">0</span></p>
        <p style="font-size: 24px;">Монеты: <span id="finalCoins">0</span></p>
        <p style="font-size: 24px;">Рекорд: <span id="finalHighScore">0</span></p>
        <div id="newRecord" style="display: none;">🌟 НОВЫЙ РЕКОРД! 🌟</div>
        <button id="restartBtn">ИГРАТЬ СНОВА</button>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const scoreElement = document.getElementById('score');
        const coinsElement = document.getElementById('coins');
        const shopCoinsElement = document.getElementById('shopCoins');
        const highScoreElement = document.getElementById('highScore');
        const jetpackProgress = document.getElementById('jetpackProgress');
        const gameOverElement = document.getElementById('gameOver');
        const finalScoreElement = document.getElementById('finalScore');
        const finalCoinsElement = document.getElementById('finalCoins');
        const finalHighScoreElement = document.getElementById('finalHighScore');
        const newRecordElement = document.getElementById('newRecord');
        const restartBtn = document.getElementById('restartBtn');
        const musicToggle = document.getElementById('musicToggle');
        const musicIcon = document.getElementById('musicIcon');

        // ========== МУЗЫКА ==========
        let audioContext;
        let isMusicPlaying = false;
        let musicNodes = [];

        function initAudio() {
            try {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
                
                // Создаем спокойную мелодию
                createBackgroundMusic();
                
                // Автоматически запускаем после первого взаимодействия
                document.addEventListener('click', function initAudioOnClick() {
                    if (audioContext.state === 'suspended') {
                        audioContext.resume();
                    }
                    document.removeEventListener('click', initAudioOnClick);
                }, { once: true });
                
            } catch (e) {
                console.log('Web Audio API не поддерживается');
            }
        }

        function createBackgroundMusic() {
            if (!audioContext) return;
            
            // Останавливаем старую музыку
            musicNodes.forEach(node => {
                if (node.stop) node.stop();
            });
            musicNodes = [];
            
            // Создаем спокойную фоновую мелодию
            const now = audioContext.currentTime;
            
            // Основной бас (простая последовательность)
            const bassNotes = [55, 55, 57, 59, 55, 55, 57, 62]; // Ноты для баса
            bassNotes.forEach((note, i) => {
                const osc = audioContext.createOscillator();
                const gain = audioContext.createGain();
                
                osc.type = 'sine';
                osc.frequency.value = note;
                
                gain.gain.setValueAtTime(0, now + i * 0.5);
                gain.gain.linearRampToValueAtTime(0.1, now + i * 0.5 + 0.1);
                gain.gain.linearRampToValueAtTime(0, now + i * 0.5 + 0.4);
                
                osc.connect(gain);
                gain.connect(audioContext.destination);
                
                osc.start(now + i * 0.5);
                osc.stop(now + i * 0.5 + 0.4);
                
                musicNodes.push(osc);
            });
            
            // Мелодия (арпеджио)
            const melodyNotes = [69, 72, 76, 72, 69, 76, 72, 69];
            melodyNotes.forEach((note, i) => {
                const osc = audioContext.createOscillator();
                const gain = audioContext.createGain();
                
                osc.type = 'triangle';
                osc.frequency.value = note;
                
                gain.gain.setValueAtTime(0, now + i * 0.25 + 2);
                gain.gain.linearRampToValueAtTime(0.05, now + i * 0.25 + 2.1);
                gain.gain.linearRampToValueAtTime(0, now + i * 0.25 + 2.3);
                
                osc.connect(gain);
                gain.connect(audioContext.destination);
                
                osc.start(now + i * 0.25 + 2);
                osc.stop(now + i * 0.25 + 2.3);
                
                musicNodes.push(osc);
            });
            
            // Аккорды (подложка)
            [0, 4, 8].forEach((time, i) => {
                const chord = [60, 64, 67]; // До мажор
                chord.forEach(note => {
                    const osc = audioContext.createOscillator();
                    const gain = audioContext.createGain();
                    
                    osc.type = 'sine';
                    osc.frequency.value = note;
                    
                    gain.gain.setValueAtTime(0, now + time);
                    gain.gain.linearRampToValueAtTime(0.03, now + time + 0.5);
                    gain.gain.linearRampToValueAtTime(0, now + time + 3.5);
                    
                    osc.connect(gain);
                    gain.connect(audioContext.destination);
                    
                    osc.start(now + time);
                    osc.stop(now + time + 3.5);
                    
                    musicNodes.push(osc);
                });
            });
            
            // Зацикливаем
            setTimeout(() => {
                if (isMusicPlaying) {
                    createBackgroundMusic();
                }
            }, 8000);
        }

        function toggleMusic() {
            if (!audioContext) {
                initAudio();
            }
            
            if (isMusicPlaying) {
                // Выключаем
                musicNodes.forEach(node => {
                    if (node.stop) node.stop();
                });
                musicNodes = [];
                musicIcon.textContent = '🔇';
                isMusicPlaying = false;
            } else {
                // Включаем
                if (audioContext.state === 'suspended') {
                    audioContext.resume();
                }
                createBackgroundMusic();
                musicIcon.textContent = '🔊';
                isMusicPlaying = true;
            }
        }

        musicToggle.addEventListener('click', toggleMusic);

        // Инициализируем аудио при загрузке
        window.addEventListener('load', () => {
            initAudio();
            // Автоматически включаем музыку через небольшое время
            setTimeout(() => {
                if (!isMusicPlaying) {
                    toggleMusic();
                }
            }, 1000);
        });

        // ========== ИГРОВАЯ ЛОГИКА ==========
        
        // Загружаем данные из localStorage
        let highScore = localStorage.getItem('doodleHighScore') ? parseInt(localStorage.getItem('doodleHighScore')) : 0;
        let coins = localStorage.getItem('doodleCoins') ? parseInt(localStorage.getItem('doodleCoins')) : 0;
        
        // Скины и предметы
        let ownedItems = localStorage.getItem('doodleOwned') ? JSON.parse(localStorage.getItem('doodleOwned')) : {
            balls: ['default'],
            backgrounds: ['default'],
            bonuses: []
        };
        
        let selectedSkin = localStorage.getItem('doodleSelectedSkin') || 'default';
        let selectedBackground = localStorage.getItem('doodleSelectedBackground') || 'default';
        let activeBonuses = localStorage.getItem('doodleBonuses') ? JSON.parse(localStorage.getItem('doodleBonuses')) : {};

        // Магазин предметов - НОВЫЕ СКИНЫ!
        const shopItems = {
            balls: [
                { id: 'default', name: 'Классический', price: 0, emoji: '😊', color: '#FFE66D', pattern: 'solid' },
                { id: 'panda', name: 'Панда', price: 150, emoji: '🐼', color: '#FFFFFF', pattern: 'panda' },
                { id: 'cow', name: 'Коровка', price: 200, emoji: '🐮', color: '#F5F5DC', pattern: 'cow' },
                { id: 'penguin', name: 'Пингвин', price: 250, emoji: '🐧', color: '#1E3A5F', pattern: 'penguin' },
                { id: 'bee', name: 'Пчелка', price: 300, emoji: '🐝', color: '#FFD700', pattern: 'bee' },
                { id: 'ladybug', name: 'Божья коровка', price: 350, emoji: '🐞', color: '#FF4444', pattern: 'ladybug' },
                { id: 'octopus', name: 'Осьминог', price: 400, emoji: '🐙', color: '#FF69B4', pattern: 'octopus' },
                { id: 'dragon', name: 'Дракон', price: 500, emoji: '🐲', color: '#50C878', pattern: 'dragon' },
                { id: 'unicorn', name: 'Единорог', price: 600, emoji: '🦄', color: '#C8A2C8', pattern: 'unicorn' },
                { id: 'rainbow', name: 'Радужный', price: 800, emoji: '🌈', color: 'rainbow', pattern: 'rainbow' }
            ],
            backgrounds: [
                { id: 'default', name: 'Космос', price: 0, emoji: '🌌', style: 'cosmic' },
                { id: 'sunset', name: 'Закат', price: 200, emoji: '🌅', style: 'sunset' },
                { id: 'forest', name: 'Лес', price: 300, emoji: '🌳', style: 'forest' },
                { id: 'ocean', name: 'Океан', price: 400, emoji: '🌊', style: 'ocean' },
                { id: 'cyber', name: 'Киберпанк', price: 600, emoji: '🤖', style: 'cyber' },
                { id: 'magic', name: 'Магия', price: 800, emoji: '✨', style: 'magic' }
            ],
            bonuses: [
                { id: 'doubleCoins', name: 'Двойные монеты', price: 500, emoji: '🪙', description: 'Монет ×2', duration: 'постоянно' },
                { id: 'longJetpack', name: 'Долгий ранец', price: 300, emoji: '🚀', description: '+3 сек к ранцу', duration: 'постоянно' },
                { id: 'shield', name: 'Щит', price: 400, emoji: '🛡️', description: '1 доп. жизнь', duration: '1 раз' },
                { id: 'magnet', name: 'Магнит', price: 600, emoji: '🧲', description: 'Притягивает монеты', duration: 'постоянно' },
                { id: 'slowMotion', name: 'Замедление', price: 450, emoji: '⏱️', description: 'Медленное падение', duration: 'постоянно' },
                { id: 'tripleJump', name: 'Тройной прыжок', price: 700, emoji: '🦘', description: '3 прыжка в воздухе', duration: 'постоянно' }
            ]
        };

        // Обновление отображения монет
        function updateCoinsDisplay() {
            coinsElement.textContent = `🪙 Монеты: ${coins}`;
            shopCoinsElement.textContent = `🪙 ${coins}`;
            localStorage.setItem('doodleCoins', coins);
        }

        // Отрисовка магазина
        function renderShop() {
            // Скины шарика
            const ballSkinsDiv = document.getElementById('ballSkins');
            ballSkinsDiv.innerHTML = '';
            shopItems.balls.forEach(item => {
                const owned = ownedItems.balls.includes(item.id);
                const selected = selectedSkin === item.id;
                
                const div = document.createElement('div');
                div.className = `shop-item ${owned ? 'owned' : ''} ${selected ? 'selected' : ''}`;
                
                div.innerHTML = `
                    <div class="item-preview">${item.emoji}</div>
                    <div class="item-info">
                        <div class="item-name">${item.name}</div>
                        <div class="item-price">${item.price === 0 ? 'Бесплатно' : `💰 ${item.price} <span>монет</span>`}</div>
                    </div>
                    ${!owned ? `<button class="buy-button" onclick="window.buyItem('ball', '${item.id}', ${item.price})">Купить</button>` : 
                      !selected ? `<button class="buy-button" onclick="window.selectItem('ball', '${item.id}')">Выбрать</button>` :
                      '<button class="buy-button" disabled>Выбрано</button>'}
                `;
                
                ballSkinsDiv.appendChild(div);
            });

            // Скины фона
            const bgSkinsDiv = document.getElementById('backgroundSkins');
            bgSkinsDiv.innerHTML = '';
            shopItems.backgrounds.forEach(item => {
                const owned = ownedItems.backgrounds.includes(item.id);
                const selected = selectedBackground === item.id;
                
                const div = document.createElement('div');
                div.className = `shop-item ${owned ? 'owned' : ''} ${selected ? 'selected' : ''}`;
                
                div.innerHTML = `
                    <div class="item-preview">${item.emoji}</div>
                    <div class="item-info">
                        <div class="item-name">${item.name}</div>
                        <div class="item-price">${item.price === 0 ? 'Бесплатно' : `💰 ${item.price} <span>монет</span>`}</div>
                    </div>
                    ${!owned ? `<button class="buy-button" onclick="window.buyItem('background', '${item.id}', ${item.price})">Купить</button>` : 
                      !selected ? `<button class="buy-button" onclick="window.selectItem('background', '${item.id}')">Выбрать</button>` :
                      '<button class="buy-button" disabled>Выбрано</button>'}
                `;
                
                bgSkinsDiv.appendChild(div);
            });

            // Бонусы
            const bonusesDiv = document.getElementById('bonuses');
            bonusesDiv.innerHTML = '';
            shopItems.bonuses.forEach(item => {
                const owned = ownedItems.bonuses.includes(item.id);
                
                const div = document.createElement('div');
                div.className = `shop-item ${owned ? 'owned' : ''}`;
                
                div.innerHTML = `
                    <div class="item-preview">${item.emoji}</div>
                    <div class="item-info">
                        <div class="item-name">${item.name}</div>
                        <div class="item-price">💰 ${item.price} монет</div>
                        <div style="font-size: 12px; opacity: 0.7;">${item.description}</div>
                    </div>
                    ${!owned ? `<button class="buy-button" onclick="window.buyItem('bonus', '${item.id}', ${item.price})">Купить</button>` : 
                      '<button class="buy-button" disabled>Куплено</button>'}
                `;
                
                bonusesDiv.appendChild(div);
            });
        }

        // Покупка предмета
        window.buyItem = function(type, id, price) {
            if (coins >= price) {
                coins -= price;
                ownedItems[type + 's'].push(id);
                
                localStorage.setItem('doodleOwned', JSON.stringify(ownedItems));
                updateCoinsDisplay();
                
                if (type === 'bonus') {
                    activeBonuses[id] = true;
                    localStorage.setItem('doodleBonuses', JSON.stringify(activeBonuses));
                    showPowerup(`✅ Куплен бонус!`, 'gold');
                }
                
                renderShop();
                showPowerup(`🎉 Покупка успешна!`, 'green');
            } else {
                showPowerup(`❌ Не хватает монет!`, 'red');
            }
        };

        // Выбор предмета
        window.selectItem = function(type, id) {
            if (type === 'ball') {
                selectedSkin = id;
                localStorage.setItem('doodleSelectedSkin', id);
            } else if (type === 'background') {
                selectedBackground = id;
                localStorage.setItem('doodleSelectedBackground', id);
            }
            renderShop();
            showPowerup(`✨ Скин выбран!`, 'cyan');
        };

        // Показать сообщение
        function showPowerup(text, color = 'cyan') {
            const indicator = document.createElement('div');
            indicator.className = 'powerup-indicator';
            indicator.textContent = text;
            indicator.style.color = color;
            document.body.appendChild(indicator);
            setTimeout(() => indicator.remove(), 1000);
        }

        // Создание эффекта треска
        function createCrackEffect(x, y) {
            const effect = document.createElement('div');
            effect.className = 'crack-effect';
            effect.textContent = '💥';
            effect.style.left = x + 'px';
            effect.style.top = y + 'px';
            effect.style.color = '#FF4444';
            effect.style.position = 'fixed';
            document.body.appendChild(effect);
            setTimeout(() => effect.remove(), 500);
        }

        // Игровые переменные
        let score = 0;
        let gameRunning = true;
        
        // Реактивный ранец
        let hasJetpack = false;
        let jetpackTime = 0;
        const maxJetpackTime = activeBonuses.longJetpack ? 480 : 300;
        
        // Щит
        let hasShield = activeBonuses.shield ? 1 : 0;
        
        // Платформы
        const platforms = [];
        const platformCount = 10;
        const platformWidth = 70;
        const platformHeight = 15;
        
        // Игрок
        const player = {
            x: canvas.width / 2 - 15,
            y: canvas.height - 100,
            width: 30,
            height: 30,
            velocityY: 0,
            velocityX: 0,
            speed: activeBonuses.slowMotion ? 4 : 5,
            jumpPower: -10,
            jetpackPower: activeBonuses.slowMotion ? -5 : -6,
            color: '#FFE66D',
            jumpsLeft: activeBonuses.tripleJump ? 3 : 1,
            airJumps: 0
        };

        // Управление - ИСПРАВЛЕНО!
        const keys = {
            'w': false, 'a': false, 's': false, 'd': false,
            'arrowup': false, 'arrowdown': false, 'arrowleft': false, 'arrowright': false,
            ' ': false
        };
        
        document.addEventListener('keydown', (e) => {
            const key = e.key.toLowerCase();
            
            // WASD
            if (key === 'w') keys['w'] = true;
            if (key === 'a') keys['a'] = true;
            if (key === 's') keys['s'] = true;
            if (key === 'd') keys['d'] = true;
            
            // Стрелки
            if (key === 'arrowup') keys['arrowup'] = true;
            if (key === 'arrowdown') keys['arrowdown'] = true;
            if (key === 'arrowleft') keys['arrowleft'] = true;
            if (key === 'arrowright') keys['arrowright'] = true;
            
            // Пробел
            if (key === ' ') keys[' '] = true;
            
            // Рестарт
            if (key === 'r') {
                restartGame();
            }
            
            // Предотвращаем скролл для всех клавиш управления
            if (['w', 'a', 's', 'd', 'arrowup', 'arrowdown', 'arrowleft', 'arrowright', ' '].includes(key)) {
                e.preventDefault();
            }
        });
        
        document.addEventListener('keyup', (e) => {
            const key = e.key.toLowerCase();
            
            // WASD
            if (key === 'w') keys['w'] = false;
            if (key === 'a') keys['a'] = false;
            if (key === 's') keys['s'] = false;
            if (key === 'd') keys['d'] = false;
            
            // Стрелки
            if (key === 'arrowup') keys['arrowup'] = false;
            if (key === 'arrowdown') keys['arrowdown'] = false;
            if (key === 'arrowleft') keys['arrowleft'] = false;
            if (key === 'arrowright') keys['arrowright'] = false;
            
            // Пробел
            if (key === ' ') keys[' '] = false;
        });

        // Инициализация платформ
        function initPlatforms() {
            platforms.length = 0;
            
            // Стартовая платформа
            platforms.push({
                x: canvas.width / 2 - platformWidth / 2,
                y: canvas.height - 50,
                width: platformWidth,
                height: platformHeight,
                type: 'normal',
                color: '#4ECDC4',
                jumpsLeft: Infinity
            });
            
            // Генерация остальных платформ
            for (let i = 1; i < platformCount; i++) {
                createPlatform(i * (canvas.height / platformCount));
            }
        }

        function createPlatform(y) {
            let type = 'normal';
            let color = '#4ECDC4';
            let jumpsLeft = Infinity;
            let special = null;
            
            const rand = Math.random();
            
            if (rand < 0.15) {
                type = 'moving';
                color = '#FF6B6B';
            } else if (rand < 0.3) {
                type = 'crack';
                color = '#8B4513';
                jumpsLeft = 2;
                special = 'crack';
            } else if (rand < 0.4) {
                type = 'jetpack';
                color = '#FFA500';
                special = 'jetpack';
            } else if (rand < 0.45) {
                type = 'bouncy';
                color = '#FFE66D';
                special = 'bouncy';
            } else if (rand < 0.5 && activeBonuses.doubleCoins) {
                type = 'coin';
                color = '#FFD700';
                special = 'coin';
            }
            
            const platform = {
                x: Math.random() * (canvas.width - platformWidth),
                y: y,
                width: platformWidth,
                height: platformHeight,
                type: type,
                color: color,
                special: special,
                jumpsLeft: jumpsLeft,
                direction: type === 'moving' ? (Math.random() < 0.5 ? 1 : -1) : 0,
                crackLevel: 0
            };
            
            platforms.push(platform);
        }

        // Обновление игры
        function update() {
            if (!gameRunning) return;
            
            // Управление - ИСПРАВЛЕНО! WASD и стрелки работают одновременно
            let moveX = 0;
            
            // WASD
            if (keys['a'] || keys['arrowleft']) moveX -= 1;
            if (keys['d'] || keys['arrowright']) moveX += 1;
            
            player.velocityX = moveX * player.speed;
            
            // Реактивный ранец (пробел)
            const jetpackActive = hasJetpack && jetpackTime > 0 && keys[' '];
            
            if (jetpackActive) {
                player.velocityY = player.jetpackPower;
                jetpackTime--;
                createJetpackParticles();
                jetpackProgress.style.width = `${(jetpackTime / maxJetpackTime) * 100}%`;
                
                if (jetpackTime <= 0) {
                    hasJetpack = false;
                }
            } else {
                // Гравитация
                player.velocityY += 0.5;
                
                // Тройной прыжок
                if (activeBonuses.tripleJump && keys[' '] && player.airJumps < player.jumpsLeft && player.velocityY > 0) {
                    player.velocityY = player.jumpPower;
                    player.airJumps++;
                    showPowerup(`🦘 Прыжок ${player.airJumps + 1}/3`, 'yellow');
                }
            }
            
            player.y += player.velocityY;
            player.x += player.velocityX;
            
            // Границы
            if (player.x < 0) player.x = 0;
            if (player.x > canvas.width - player.width) player.x = canvas.width - player.width;
            
            // Проверка столкновений с платформами
            for (let i = platforms.length - 1; i >= 0; i--) {
                const platform = platforms[i];
                
                if (player.velocityY > 0 && 
                    player.y + player.height > platform.y &&
                    player.y + player.height < platform.y + platform.height + 15 &&
                    player.x + player.width > platform.x &&
                    player.x < platform.x + platform.width) {
                    
                    player.airJumps = 0;
                    
                    let jumpPower = player.jumpPower;
                    
                    if (platform.special === 'jetpack') {
                        hasJetpack = true;
                        jetpackTime = maxJetpackTime;
                        jetpackProgress.style.width = '100%';
                        showPowerup('🚀 РЕАКТИВНЫЙ РАНЕЦ!', 'orange');
                        platforms.splice(i, 1);
                        continue;
                    }
                    else if (platform.special === 'bouncy') {
                        jumpPower = player.jumpPower * 1.5;
                        showPowerup('✨ СУПЕР ПРЫЖОК!', 'yellow');
                    }
                    else if (platform.special === 'coin') {
                        coins += activeBonuses.doubleCoins ? 20 : 10;
                        updateCoinsDisplay();
                        showPowerup('🪙 +' + (activeBonuses.doubleCoins ? '20' : '10'), 'gold');
                        platforms.splice(i, 1);
                        continue;
                    }
                    else if (platform.special === 'crack') {
                        platform.crackLevel++;
                        
                        createCrackEffect(
                            canvas.getBoundingClientRect().left + platform.x + platform.width/2,
                            canvas.getBoundingClientRect().top + platform.y
                        );
                        
                        if (platform.crackLevel >= 2) {
                            showPowerup('💥 БЛОК РАЗРУШЕН!', '#ff4444');
                            platforms.splice(i, 1);
                            continue;
                        } else {
                            showPowerup('💢 ТРЕЩИНА!', '#ff8888');
                            platform.color = '#A0522D';
                        }
                    }
                    
                    player.velocityY = jumpPower;
                    player.y = platform.y - player.height;
                    
                    score += 10;
                    coins += activeBonuses.doubleCoins ? 2 : 1;
                    
                    scoreElement.textContent = `🎯 Счет: ${score}`;
                    updateCoinsDisplay();
                    
                    if (score > highScore) {
                        highScore = score;
                        localStorage.setItem('doodleHighScore', highScore);
                        highScoreElement.textContent = `🏆 Рекорд: ${highScore}`;
                    }
                }
            }
            
            // Движение платформ
            for (let platform of platforms) {
                if (platform.type === 'moving') {
                    platform.x += platform.direction * 2;
                    
                    if (platform.x <= 0 || platform.x >= canvas.width - platform.width) {
                        platform.direction *= -1;
                    }
                }
            }
            
            // Камера
            if (player.y < canvas.height / 3) {
                const diff = canvas.height / 3 - player.y;
                player.y += diff;
                
                for (let platform of platforms) {
                    platform.y += diff;
                }
                
                for (let i = platforms.length - 1; i >= 0; i--) {
                    if (platforms[i].y > canvas.height) {
                        platforms.splice(i, 1);
                        createPlatform(-platformHeight);
                    }
                }
            }
            
            // Проверка проигрыша
            if (player.y > canvas.height) {
                if (hasShield > 0) {
                    hasShield--;
                    player.y = canvas.height - 200;
                    player.velocityY = player.jumpPower;
                    showPowerup('🛡️ ЩИТ АКТИВИРОВАН!', 'cyan');
                } else {
                    gameOver();
                }
            }
        }

        // Частицы для ранца
        const particles = [];
        
        function createJetpackParticles() {
            for (let i = 0; i < 5; i++) {
                particles.push({
                    x: player.x + player.width / 2,
                    y: player.y + player.height,
                    vx: (Math.random() - 0.5) * 3,
                    vy: Math.random() * 5 + 3,
                    life: 30,
                    color: `hsl(${Math.random() * 60 + 20}, 100%, 60%)`
                });
            }
        }

        // Рисование игрока с разными скинами
        function drawPlayer() {
            const skin = shopItems.balls.find(s => s.id === selectedSkin) || shopItems.balls[0];
            
            ctx.shadowBlur = 20;
            ctx.shadowColor = skin.color === 'rainbow' ? 'white' : skin.color;
            
            // Базовая форма
            ctx.beginPath();
            ctx.ellipse(player.x + player.width/2, player.y + player.height/2, 
                       player.width/2, player.height/2, 0, 0, Math.PI * 2);
            
            // Заливка в зависимости от скина
            if (skin.pattern === 'rainbow') {
                // Радужный градиент
                const gradient = ctx.createLinearGradient(player.x, player.y, player.x + player.width, player.y + player.height);
                gradient.addColorStop(0, 'red');
                gradient.addColorStop(0.2, 'orange');
                gradient.addColorStop(0.4, 'yellow');
                gradient.addColorStop(0.6, 'green');
                gradient.addColorStop(0.8, 'blue');
                gradient.addColorStop(1, 'purple');
                ctx.fillStyle = gradient;
                ctx.fill();
            } else {
                ctx.fillStyle = skin.color;
                ctx.fill();
            }
            
            // Рисуем узоры для разных скинов
            ctx.shadowBlur = 0;
            ctx.fillStyle = 'rgba(0,0,0,0.3)';
            
            switch(skin.pattern) {
                case 'panda':
                    // Черные пятна панды
                    ctx.beginPath();
                    ctx.arc(player.x + player.width/2 - 8, player.y + player.height/2 - 5, 5, 0, Math.PI * 2);
                    ctx.arc(player.x + player.width/2 + 8, player.y + player.height/2 - 5, 5, 0, Math.PI * 2);
                    ctx.fill();
                    break;
                    
                case 'cow':
                    // Пятна коровы
                    for (let i = 0; i < 5; i++) {
                        ctx.beginPath();
                        ctx.arc(player.x + 5 + i * 5, player.y + 10, 3, 0, Math.PI * 2);
                        ctx.fill();
                    }
                    break;
                    
                case 'penguin':
                    // Белый животик пингвина
                    ctx.fillStyle = 'white';
                    ctx.beginPath();
                    ctx.ellipse(player.x + player.width/2, player.y + player.height/2 + 2, 8, 10, 0, 0, Math.PI * 2);
                    ctx.fill();
                    break;
                    
                case 'bee':
                    // Полоски пчелы
                    ctx.fillStyle = 'black';
                    for (let i = 0; i < 3; i++) {
                        ctx.fillRect(player.x + 5, player.y + 5 + i * 7, player.width - 10, 3);
                    }
                    break;
                    
                case 'ladybug':
                    // Точки божьей коровки
                    ctx.fillStyle = 'black';
                    ctx.beginPath();
                    ctx.arc(player.x + 10, player.y + 12, 3, 0, Math.PI * 2);
                    ctx.arc(player.x + 20, player.y + 18, 3, 0, Math.PI * 2);
                    ctx.arc(player.x + 15, player.y + 22, 3, 0, Math.PI * 2);
                    ctx.fill();
                    break;
                    
                case 'octopus':
                    // Щупальца осьминога
                    ctx.strokeStyle = skin.color;
                    ctx.lineWidth = 3;
                    for (let i = 0; i < 4; i++) {
                        ctx.beginPath();
                        ctx.moveTo(player.x + 5 + i * 7, player.y + player.height - 5);
                        ctx.lineTo(player.x + i * 7, player.y + player.height + 5);
                        ctx.stroke();
                    }
                    break;
                    
                case 'dragon':
                    // Крылья дракона
                    ctx.fillStyle = '#FFA500';
                    ctx.beginPath();
                    ctx.moveTo(player.x, player.y + 10);
                    ctx.lineTo(player.x - 8, player.y);
                    ctx.lineTo(player.x, player.y - 5);
                    ctx.fill();
                    
                    ctx.beginPath();
                    ctx.moveTo(player.x + player.width, player.y + 10);
                    ctx.lineTo(player.x + player.width + 8, player.y);
                    ctx.lineTo(player.x + player.width, player.y - 5);
                    ctx.fill();
                    break;
                    
                case 'unicorn':
                    // Рог единорога
                    ctx.fillStyle = 'gold';
                    ctx.beginPath();
                    ctx.moveTo(player.x + player.width/2, player.y - 5);
                    ctx.lineTo(player.x + player.width/2 - 3, player.y - 12);
                    ctx.lineTo(player.x + player.width/2 + 3, player.y - 12);
                    ctx.fill();
                    break;
            }
            
            // Глаза (общие для всех)
            ctx.fillStyle = '#FFFFFF';
            ctx.beginPath();
            ctx.arc(player.x + player.width/2 - 5, player.y + player.height/2 - 5, 5, 0, Math.PI * 2);
            ctx.arc(player.x + player.width/2 + 5, player.y + player.height/2 - 5, 5, 0, Math.PI * 2);
            ctx.fill();
            
            // Зрачки
            ctx.fillStyle = '#000000';
            ctx.beginPath();
            ctx.arc(player.x + player.width/2 - 5 + (player.velocityX * 0.3), player.y + player.height/2 - 6, 2, 0, Math.PI * 2);
            ctx.arc(player.x + player.width/2 + 5 + (player.velocityX * 0.3), player.y + player.height/2 - 6, 2, 0, Math.PI * 2);
            ctx.fill();
            
            // Ранец
            if (hasJetpack) {
                ctx.fillStyle = '#FFA500';
                ctx.shadowColor = 'orange';
                ctx.fillRect(player.x - 5, player.y + 5, 8, 20);
                ctx.fillRect(player.x + player.width - 3, player.y + 5, 8, 20);
                
                if (keys[' '] && jetpackTime > 0) {
                    const gradient = ctx.createLinearGradient(player.x + player.width/2, player.y + player.height,
                                                            player.x + player.width/2, player.y + player.height + 30);
                    gradient.addColorStop(0, 'yellow');
                    gradient.addColorStop(0.5, 'orange');
                    gradient.addColorStop(1, 'red');
                    
                    ctx.fillStyle = gradient;
                    ctx.beginPath();
                    ctx.moveTo(player.x + player.width/2 - 8, player.y + player.height);
                    ctx.lineTo(player.x + player.width/2, player.y + player.height + 25);
                    ctx.lineTo(player.x + player.width/2 + 8, player.y + player.height);
                    ctx.fill();
                }
            }
            
            // Щит
            if (hasShield > 0) {
                ctx.strokeStyle = 'cyan';
                ctx.lineWidth = 3;
                ctx.setLineDash([10, 5]);
                ctx.beginPath();
                ctx.ellipse(player.x + player.width/2, player.y + player.height/2, 
                           player.width, player.height, 0, 0, Math.PI * 2);
                ctx.stroke();
                ctx.setLineDash([]);
            }
            
            // Магнит
            if (activeBonuses.magnet) {
                ctx.strokeStyle = 'gold';
                ctx.lineWidth = 2;
                ctx.setLineDash([5, 5]);
                ctx.beginPath();
                ctx.ellipse(player.x + player.width/2, player.y + player.height/2, 
                           player.width + 5, player.height + 5, 0, 0, Math.PI * 2);
                ctx.stroke();
                ctx.setLineDash([]);
            }
        }

        // Отрисовка фона
        function drawBackground() {
            switch(selectedBackground) {
                case 'sunset':
                    const gradient1 = ctx.createLinearGradient(0, 0, 0, canvas.height);
                    gradient1.addColorStop(0, '#ff7e5f');
                    gradient1.addColorStop(0.5, '#feb47b');
                    gradient1.addColorStop(1, '#ff6b6b');
                    ctx.fillStyle = gradient1;
                    ctx.fillRect(0, 0, canvas.width, canvas.height);
                    break;
                    
                case 'forest':
                    ctx.fillStyle = '#1a4d2e';
                    ctx.fillRect(0, 0, canvas.width, canvas.height);
                    for (let i = 0; i < 5; i++) {
                        ctx.fillStyle = '#2d6a4f';
                        ctx.beginPath();
                        ctx.moveTo(i * 100, canvas.height);
                        ctx.lineTo(i * 100 + 30, canvas.height - 100);
                        ctx.lineTo(i * 100 - 30, canvas.height - 100);
                        ctx.fill();
                    }
                    break;
                    
                case 'ocean':
                    const gradient2 = ctx.createLinearGradient(0, 0, 0, canvas.height);
                    gradient2.addColorStop(0, '#00b4d8');
                    gradient2.addColorStop(0.5, '#0077b6');
                    gradient2.addColorStop(1, '#023e8a');
                    ctx.fillStyle = gradient2;
                    ctx.fillRect(0, 0, canvas.width, canvas.height);
                    
                    for (let i = 0; i < 10; i++) {
                        ctx.fillStyle = 'rgba(255, 255, 255, 0.1)';
                        ctx.beginPath();
                        ctx.arc(50 + i * 40, 400 + Math.sin(Date.now() * 0.001 + i) * 20, 20, 0, Math.PI * 2);
                        ctx.fill();
                    }
                    break;
                    
                case 'cyber':
                    ctx.fillStyle = '#0d0d0d';
                    ctx.fillRect(0, 0, canvas.width, canvas.height);
                    ctx.strokeStyle = '#00ff00';
                    ctx.lineWidth = 1;
                    for (let i = 0; i < canvas.width; i += 30) {
                        ctx.beginPath();
                        ctx.moveTo(i, 0);
                        ctx.lineTo(i + 20, canvas.height);
                        ctx.strokeStyle = `rgba(0, 255, 0, 0.1)`;
                        ctx.stroke();
                    }
                    break;
                    
                case 'magic':
                    for (let i = 0; i < 10; i++) {
                        ctx.fillStyle = `hsla(${Date.now() / 20 + i * 36}, 70%, 50%, 0.1)`;
                        ctx.beginPath();
                        ctx.arc(200 + Math.sin(Date.now() * 0.001 + i) * 100, 
                               300 + Math.cos(Date.now() * 0.001 + i) * 100, 100, 0, Math.PI * 2);
                        ctx.fill();
                    }
                    break;
                    
                default: // cosmic
                    ctx.fillStyle = 'rgba(0, 0, 0, 0.3)';
                    ctx.fillRect(0, 0, canvas.width, canvas.height);
                    
                    for (let i = 0; i < 20; i++) {
                        ctx.fillStyle = `rgba(255, 255, 255, ${Math.random() * 0.5})`;
                        ctx.beginPath();
                        ctx.arc(Math.random() * canvas.width, Math.random() * canvas.height, Math.random() * 2, 0, Math.PI * 2);
                        ctx.fill();
                    }
                    break;
            }
        }

        // Отрисовка
        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Фон
            drawBackground();
            
            // Частицы
            for (let i = particles.length - 1; i >= 0; i--) {
                const p = particles[i];
                ctx.globalAlpha = p.life / 30;
                ctx.fillStyle = p.color;
                ctx.beginPath();
                ctx.arc(p.x, p.y, 4, 0, Math.PI * 2);
                ctx.fill();
                
                p.x += p.vx;
                p.y += p.vy;
                p.life--;
                
                if (p.life <= 0) {
                    particles.splice(i, 1);
                }
            }
            ctx.globalAlpha = 1;
            
            // Платформы
            for (let platform of platforms) {
                ctx.shadowColor = 'rgba(0,0,0,0.5)';
                ctx.shadowBlur = 10;
                ctx.shadowOffsetY = 5;
                
                if (platform.special === 'crack' && platform.crackLevel === 1) {
                    ctx.fillStyle = platform.color;
                    ctx.fillRect(platform.x, platform.y, platform.width, platform.height);
                    
                    ctx.fillStyle = '#000000';
                    ctx.beginPath();
                    ctx.moveTo(platform.x + 10, platform.y + 5);
                    ctx.lineTo(platform.x + 30, platform.y + 10);
                    ctx.lineTo(platform.x + 20, platform.y + 12);
                    ctx.fill();
                    
                    ctx.beginPath();
                    ctx.moveTo(platform.x + 50, platform.y + 8);
                    ctx.lineTo(platform.x + 60, platform.y + 12);
                    ctx.lineTo(platform.x + 55, platform.y + 14);
                    ctx.fill();
                } else {
                    const gradient = ctx.createLinearGradient(platform.x, platform.y, platform.x, platform.y + platform.height);
                    gradient.addColorStop(0, platform.color);
                    gradient.addColorStop(1, platform.color);
                    ctx.fillStyle = gradient;
                    ctx.fillRect(platform.x, platform.y, platform.width, platform.height);
                }
                
                // Блик
                ctx.shadowBlur = 0;
                ctx.fillStyle = 'rgba(255, 255, 255, 0.3)';
                ctx.fillRect(platform.x, platform.y, platform.width, 3);
                
                // Индикаторы
                if (platform.special === 'jetpack') {
                    ctx.font = '20px Arial';
                    ctx.fillStyle = 'white';
                    ctx.shadowBlur = 10;
                    ctx.shadowColor = 'orange';
                    ctx.fillText('🚀', platform.x + platform.width/2 - 10, platform.y - 10);
                }
                
                if (platform.special === 'coin') {
                    ctx.font = '20px Arial';
                    ctx.fillStyle = 'gold';
                    ctx.shadowColor = 'gold';
                    ctx.fillText('🪙', platform.x + platform.width/2 - 10, platform.y - 10);
                }
            }
            
            // Игрок
            drawPlayer();
            
            ctx.shadowBlur = 0;
        }

        function gameOver() {
            gameRunning = false;
            finalScoreElement.textContent = score;
            finalCoinsElement.textContent = coins;
            finalHighScoreElement.textContent = highScore;
            
            newRecordElement.style.display = score >= highScore ? 'block' : 'none';
            gameOverElement.style.display = 'block';
        }

        function restartGame() {
            gameRunning = true;
            score = 0;
            hasJetpack = false;
            jetpackTime = 0;
            jetpackProgress.style.width = '0%';
            
            scoreElement.textContent = `🎯 Счет: 0`;
            player.y = canvas.height - 100;
            player.velocityY = 0;
            player.x = canvas.width / 2 - 15;
            gameOverElement.style.display = 'none';
            particles.length = 0;
            
            initPlatforms();
        }

        // Запуск
        highScoreElement.textContent = `🏆 Рекорд: ${highScore}`;
        updateCoinsDisplay();
        renderShop();
        initPlatforms();
        gameLoop();

        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        restartBtn.addEventListener('click', restartGame);
    </script>
</body>
</html>
