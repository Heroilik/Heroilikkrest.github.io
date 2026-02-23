<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ПК Сборщик: Реалистичная сборка с перетаскиванием</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }

        body {
            background: linear-gradient(145deg, #1a2f3f, #0d1c2a);
            min-height: 100vh;
            font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #e0f0ff;
            overflow-x: hidden;
        }

        /* Музыкальный плеер */
        .music-player {
            position: fixed;
            top: 20px;
            left: 20px;
            background: rgba(10, 25, 40, 0.9);
            backdrop-filter: blur(10px);
            border-radius: 60px;
            padding: 12px 25px;
            border: 2px solid #5f9fc0;
            box-shadow: 0 8px 0 #1f4057, 0 15px 25px rgba(0,0,0,0.5);
            display: flex;
            align-items: center;
            gap: 20px;
            z-index: 1000;
        }

        .music-btn {
            background: #2f6580;
            border: none;
            color: white;
            font-size: 24px;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            cursor: pointer;
            border-bottom: 4px solid #12384f;
            transition: 0.05s linear;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .music-btn:active {
            border-bottom-width: 1px;
            transform: translateY(3px);
        }

        .music-status {
            font-size: 18px;
            color: #b5e4ff;
        }

        .volume-slider {
            width: 100px;
            height: 6px;
            background: #1f4057;
            border-radius: 10px;
            overflow: hidden;
        }

        .volume-fill {
            height: 100%;
            width: 70%;
            background: #7fd4ff;
            border-radius: 10px;
        }

        .game-container {
            max-width: 1800px;
            width: 100%;
            background: rgba(8, 22, 35, 0.85);
            backdrop-filter: blur(10px);
            border-radius: 60px;
            padding: 30px;
            box-shadow: 0 30px 50px rgba(0, 0, 0, 0.8), inset 0 2px 5px rgba(255, 255, 255, 0.1);
            border: 1px solid #4f8fc0;
            margin-top: 80px;
        }

        .stats-panel {
            background: linear-gradient(145deg, #0f273b, #07212f);
            border-radius: 50px;
            padding: 20px 35px;
            margin-bottom: 30px;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            border: 1px solid #5fa3d9;
            box-shadow: inset 0 3px 8px #00000088, 0 15px 20px #00000055;
            gap: 20px;
        }

        .money {
            font-size: 48px;
            font-weight: 800;
            background: linear-gradient(135deg, #ffe68f, #ffb347);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 15px #ffaa33;
        }

        .money::before {
            content: "💰 ";
            font-size: 50px;
            -webkit-text-fill-color: initial;
            color: #ffd966;
        }

        .level-info {
            display: flex;
            align-items: center;
            gap: 20px;
            background: #1d4057;
            padding: 10px 25px;
            border-radius: 60px;
            border: 1px solid #7bb3d9;
        }

        .level-badge {
            font-size: 28px;
            font-weight: 700;
            color: #ffd966;
        }

        .exp-bar {
            width: 200px;
            height: 20px;
            background: #0f2a38;
            border-radius: 30px;
            overflow: hidden;
            border: 2px solid #4f8fb2;
        }

        .exp-fill {
            height: 100%;
            background: linear-gradient(90deg, #ffd966, #ffaa33);
            width: 0%;
            transition: width 0.3s;
        }

        /* Основной макет */
        .main-layout {
            display: flex;
            gap: 30px;
            flex-wrap: wrap;
        }

        .workshop {
            flex: 2.5;
            min-width: 700px;
        }

        .store-column {
            flex: 1.8;
            min-width: 450px;
        }

        /* РЕАЛИСТИЧНЫЙ 3D КОМПЬЮТЕР */
        .computer-3d-model {
            background: #0c2a3c;
            border-radius: 50px;
            padding: 30px;
            margin-bottom: 30px;
            border: 1px solid #4e9fd1;
            box-shadow: 0 18px 0 #063044, inset 0 -5px 15px #2a6480;
        }

        .model-title {
            font-size: 28px;
            font-weight: 700;
            color: #ffd78c;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        /* Реалистичный корпус */
        .realistic-case {
            background: #1b2f3f;
            border-radius: 30px 30px 20px 20px;
            padding: 20px;
            border: 3px solid #6a8fa8;
            box-shadow: 
                inset 0 0 0 2px #a5c9e5,
                0 20px 0 #0d3145,
                0 30px 30px rgba(0,0,0,0.7),
                inset -10px -10px 20px rgba(0,0,0,0.5),
                inset 10px 10px 20px rgba(255,255,255,0.1);
            position: relative;
            transform: perspective(1500px) rotateX(3deg);
        }

        /* Стеклянная боковая панель */
        .glass-panel {
            position: absolute;
            top: 25px;
            right: 25px;
            bottom: 25px;
            left: 25px;
            background: linear-gradient(135deg, rgba(100, 180, 255, 0.15), rgba(50, 100, 150, 0.1));
            border: 2px solid #7fb3d9;
            border-radius: 20px;
            backdrop-filter: blur(2px);
            pointer-events: none;
            box-shadow: inset 0 0 30px rgba(0, 160, 255, 0.3);
            z-index: 2;
        }

        /* Вентиляционные отверстия */
        .vents {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 60px;
            height: 150px;
            background: repeating-linear-gradient(90deg, 
                transparent 0px, 
                transparent 8px, 
                #3a5f7a 8px, 
                #3a5f7a 12px);
            border-radius: 5px;
            opacity: 0.7;
            z-index: 3;
        }

        /* Материнская плата внутри */
        .motherboard-tray {
            background: #1f4a5e;
            border-radius: 20px;
            padding: 20px;
            border: 2px solid #7fa9c9;
            box-shadow: inset 0 0 20px #00000055;
            position: relative;
            z-index: 5;
            min-height: 450px;
        }

        /* Слоты для компонентов */
        .parts-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-bottom: 20px;
        }

        .part-slot-real {
            background: #153c51;
            border-radius: 20px;
            padding: 15px;
            border: 2px solid #5f9fc0;
            box-shadow: 
                inset 0 -5px 0 #0a2535,
                0 8px 0 #0a2535,
                0 0 0 2px #7fb3d9 inset;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            cursor: pointer;
            transition: 0.1s;
            position: relative;
            min-height: 160px;
        }

        .part-slot-real:hover {
            background: #1f5a77;
            transform: translateY(-3px);
            box-shadow: 
                inset 0 -5px 0 #0a2535,
                0 11px 0 #0a2535,
                0 0 0 2px #b3e0ff inset;
        }

        /* Реалистичные 3D детали */
        .real-part {
            width: 100px;
            height: 100px;
            background: #2a5a7a;
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            border: 3px solid #a3d0f0;
            box-shadow: 
                0 10px 0 #0d3145,
                0 15px 20px rgba(0,0,0,0.4),
                inset 0 -5px 10px #c0e1ff;
            transform: rotate(0deg) scale(0.95);
            transition: 0.1s;
            position: relative;
        }

        /* Детали с уникальным дизайном */
        .real-part.cpu {
            background: #4f658d;
            border-color: #ffd966;
            border-radius: 25px 25px 10px 10px;
        }
        .real-part.cpu::after {
            content: "";
            position: absolute;
            top: 5px;
            left: 5px;
            right: 5px;
            bottom: 5px;
            border: 2px dashed #ffd966;
            border-radius: 15px;
        }

        .real-part.motherboard {
            background: #3f7890;
            border-color: #9fdf9f;
            border-radius: 30px 10px 30px 10px;
        }
        .real-part.motherboard::after {
            content: "🔌";
            font-size: 20px;
            position: absolute;
            bottom: 5px;
            right: 5px;
            opacity: 0.7;
        }

        .real-part.ram {
            background: #607d8b;
            border-color: #f4c542;
            border-radius: 5px 20px 5px 20px;
            height: 80px;
        }
        .real-part.ram::after {
            content: "";
            position: absolute;
            top: 10px;
            bottom: 10px;
            left: 15px;
            right: 15px;
            background: repeating-linear-gradient(90deg, #ffd966, #ffd966 5px, transparent 5px, transparent 10px);
            border-radius: 3px;
        }

        .real-part.gpu {
            background: #6a4e7e;
            border-color: #d8a1d8;
            border-radius: 40px 10px 40px 10px;
            width: 120px;
        }
        .real-part.gpu::after {
            content: "🎮";
            font-size: 30px;
            position: absolute;
            bottom: 5px;
            right: 5px;
            opacity: 0.5;
        }

        .real-part.storage {
            background: #3f7e6b;
            border-color: #98f0b0;
            border-radius: 5px 20px 5px 20px;
        }

        .real-part.power {
            background: #8b6b46;
            border-color: #f7b773;
            border-radius: 20px 5px 20px 5px;
        }
        .real-part.power::after {
            content: "⚡";
            font-size: 30px;
            position: absolute;
            bottom: 5px;
            right: 5px;
            opacity: 0.7;
        }

        .real-part.case {
            background: #5f7c9a;
            border-color: #b0e0e6;
            border-radius: 20px;
        }

        /* Провода и кабели */
        .cable {
            position: absolute;
            background: #2a2a2a;
            height: 3px;
            width: 30px;
            transform: rotate(45deg);
            z-index: 10;
        }

        .cable:nth-child(1) { top: 20px; left: 30px; width: 50px; background: #444; transform: rotate(30deg); }
        .cable:nth-child(2) { bottom: 30px; right: 40px; width: 70px; background: #555; transform: rotate(-20deg); }
        .cable:nth-child(3) { top: 50px; right: 60px; width: 40px; background: #666; transform: rotate(60deg); }

        /* Кулеры */
        .fan {
            width: 40px;
            height: 40px;
            background: #1a1a1a;
            border-radius: 50%;
            border: 3px solid #aaa;
            position: relative;
            animation: spin 3s linear infinite;
            box-shadow: 0 0 10px #4da6ff;
        }

        .fan::after {
            content: "";
            position: absolute;
            top: 5px;
            left: 5px;
            right: 5px;
            bottom: 5px;
            background: conic-gradient(from 0deg, #333, #666, #333, #666, #333);
            border-radius: 50%;
        }

        @keyframes spin {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        .part-name-real {
            font-weight: 600;
            font-size: 15px;
            color: #fff0cc;
            text-align: center;
            background: #1f3d55;
            padding: 5px 10px;
            border-radius: 30px;
            width: 100%;
        }

        .empty-slot-real {
            color: #9abed9;
            font-style: italic;
            font-size: 18px;
            padding: 20px 0;
        }

        .case-footer-real {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
            padding-top: 15px;
            border-top: 2px solid #4f8fb2;
            color: #ceeaff;
            font-size: 20px;
        }

        /* СБОРОЧНЫЙ СТОЛ */
        .workbench {
            background: #3a2a1e;
            background-image: radial-gradient(circle, #5a4a3a 1px, transparent 1px);
            background-size: 30px 30px;
            border-radius: 40px;
            padding: 20px;
            margin: 20px 0;
            border: 4px solid #8b6e4b;
            box-shadow: 0 15px 0 #4a3a2a, inset 0 -5px 20px #6b5a4a;
            min-height: 150px;
        }

        .bench-title {
            font-size: 24px;
            color: #ffd78c;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .bench-parts {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            min-height: 120px;
            padding: 15px;
            background: #2a1e12;
            border-radius: 30px;
            border: 2px dashed #c0a070;
        }

        .bench-part {
            background: #1d4b68;
            border-radius: 25px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            align-items: center;
            border: 2px solid #62b3e6;
            box-shadow: 0 8px 0 #0b2b3f;
            cursor: grab;
            transition: 0.1s;
            min-width: 120px;
        }

        .bench-part:active {
            cursor: grabbing;
            transform: scale(0.98);
        }

        .bench-part.dragging {
            opacity: 0.5;
        }

        /* МАГАЗИН */
        .store-panel {
            background: #0b3147;
            border-radius: 55px;
            padding: 28px;
            border: 1px solid #68a9d1;
            box-shadow: 0 15px 0 #073144;
        }

        .store-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }

        .store-header h2 {
            font-size: 36px;
            color: #f5faff;
        }

        .tab-container {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 25px;
            background: #0c2638;
            padding: 12px;
            border-radius: 60px;
            border: 1px solid #3f7d9f;
        }

        .tab-btn {
            background: transparent;
            border: none;
            color: #c2dfff;
            font-size: 18px;
            font-weight: 600;
            padding: 14px 22px;
            border-radius: 50px;
            cursor: pointer;
            transition: 0.15s;
            text-transform: uppercase;
        }

        .tab-btn.active {
            background: #1e6c99;
            color: white;
            box-shadow: 0 5px 0 #0b3f5c;
            border: 1px solid #9dd0ff;
        }

        .component-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 16px;
            max-height: 500px;
            overflow-y: auto;
            padding: 10px;
        }

        .component-item {
            background: #1d4b68;
            border-radius: 40px;
            padding: 20px 15px;
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            border: 2px solid #62b3e6;
            cursor: grab;
            transition: 0.1s;
            box-shadow: 0 8px 0 #0b2b3f;
        }

        .component-item:active {
            cursor: grabbing;
            transform: scale(0.98);
        }

        .component-item:hover {
            background: #2f6488;
            transform: translateY(-3px);
            box-shadow: 0 11px 0 #0b2b3f;
        }

        .shop-icon {
            width: 70px;
            height: 70px;
            background: #20577a;
            border-radius: 25px;
            border: 3px solid #a3d0f0;
            box-shadow: 0 5px 0 #0a2b3b;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 36px;
            margin-bottom: 10px;
        }

        .comp-price {
            font-weight: 800;
            font-size: 24px;
            color: #b9ffb9;
            margin-top: 10px;
        }

        .comp-price::before {
            content: "$";
            font-size: 18px;
        }

        .action-button {
            background: #2f77a1;
            border: none;
            color: white;
            font-size: 22px;
            font-weight: 700;
            padding: 15px 25px;
            border-radius: 60px;
            width: 100%;
            cursor: pointer;
            border-bottom: 5px solid #144258;
            transition: 0.08s;
            text-transform: uppercase;
            margin-top: 10px;
        }

        .action-button:active {
            border-bottom-width: 1px;
            transform: translateY(4px);
        }

        .action-button:disabled {
            opacity: 0.4;
            pointer-events: none;
        }

        .save-buttons {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }

        /* Уведомления */
        .notification-container {
            position: fixed;
            top: 30px;
            right: 30px;
            z-index: 9999;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .notification {
            background: linear-gradient(135deg, #1e4b6e, #0d2f47);
            border-left: 8px solid #ffd966;
            border-radius: 25px;
            padding: 20px 30px;
            color: white;
            font-size: 20px;
            font-weight: 600;
            box-shadow: 0 15px 25px rgba(0,0,0,0.5);
            animation: slideIn 0.3s ease;
            max-width: 400px;
            backdrop-filter: blur(10px);
            display: flex;
            align-items: center;
            gap: 15px;
        }

        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
    </style>
</head>
<body>
    <!-- Музыкальный плеер -->
    <div class="music-player">
        <button class="music-btn" id="playMusicBtn">▶️</button>
        <button class="music-btn" id="pauseMusicBtn">⏸️</button>
        <div class="music-status" id="musicStatus">🎵 Lo-fi играет</div>
        <div class="volume-slider">
            <div class="volume-fill" id="volumeFill" style="width: 70%"></div>
        </div>
    </div>

    <!-- Аудиоэлемент (спокойная lo-fi музыка) -->
    <audio id="bgMusic" loop>
        <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_c8b2a5a6a6.mp3?filename=lofi-study-112191.mp3" type="audio/mpeg">
    </audio>

    <!-- Контейнер для уведомлений -->
    <div class="notification-container" id="notificationContainer"></div>

    <div class="game-container">
        <!-- Статистика -->
        <div class="stats-panel">
            <span class="money" id="balanceDisplay">5000</span>
            
            <div class="level-info">
                <span class="level-badge" id="levelDisplay">УРОВЕНЬ 1</span>
                <div class="exp-bar">
                    <div class="exp-fill" id="expBar" style="width: 0%"></div>
                </div>
            </div>
        </div>

        <div class="main-layout">
            <!-- ЛЕВАЯ ЧАСТЬ: РЕАЛИСТИЧНЫЙ ПК И СТОЛ -->
            <div class="workshop">
                <!-- РЕАЛИСТИЧНЫЙ 3D КОМПЬЮТЕР -->
                <div class="computer-3d-model">
                    <div class="model-title">
                        <span>🖥️ РЕАЛИСТИЧНЫЙ ПК</span>
                        <span style="font-size: 18px;">(перетащи детали на слоты)</span>
                    </div>
                    
                    <div class="realistic-case">
                        <div class="glass-panel"></div>
                        <div class="vents"></div>
                        
                        <!-- Материнская плата с компонентами -->
                        <div class="motherboard-tray">
                            <div class="parts-grid" id="realPcSlots">
                                <!-- Слоты будут заполнены через JS -->
                            </div>
                            
                            <!-- Кулеры и провода для реализма -->
                            <div class="cable"></div>
                            <div class="cable"></div>
                            <div class="cable"></div>
                            
                            <div style="display: flex; gap: 10px; justify-content: center; margin-top: 15px;">
                                <div class="fan"></div>
                                <div class="fan"></div>
                                <div class="fan"></div>
                            </div>
                        </div>
                        
                        <div class="case-footer-real">
                            <span>🔧 Системный блок</span>
                            <span id="realPartsCount">0/7 деталей</span>
                        </div>
                    </div>
                </div>

                <!-- СБОРОЧНЫЙ СТОЛ (сюда перетаскиваются детали из магазина) -->
                <div class="workbench" id="workbench">
                    <div class="bench-title">
                        <span>🔨 СБОРОЧНЫЙ СТОЛ</span>
                        <span style="font-size: 16px;">(перетащи детали в ПК)</span>
                    </div>
                    <div class="bench-parts" id="benchParts">
                        <!-- Сюда будут добавляться купленные детали -->
                    </div>
                </div>

                <!-- ЗАКАЗ КЛИЕНТА -->
                <div class="order-card" style="background: #0a2c40; border-radius: 50px; padding: 28px; border: 2px solid #f5c77e; margin-top: 20px;">
                    <div class="order-title" style="font-size: 30px; color: #ffeac2;">📋 ЗАКАЗ КЛИЕНТА</div>
                    <div id="orderRequirements" style="display: grid; grid-template-columns: repeat(2,1fr); gap: 15px; background: #184d68; border-radius: 30px; padding: 20px; margin: 15px 0;"></div>
                    <button class="action-button" id="completeOrderBtn" style="background:#37996b;">✅ ВЫПОЛНИТЬ</button>
                    <button class="action-button" id="newOrderBtn" style="background:#8b6e48; margin-top: 10px;">🔄 Новый клиент</button>
                </div>
            </div>

            <!-- ПРАВАЯ ЧАСТЬ: МАГАЗИН -->
            <div class="store-column">
                <div class="store-panel">
                    <div class="store-header">
                        <h2>🛒 МАГАЗИН</h2>
                    </div>

                    <!-- Вкладки -->
                    <div class="tab-container">
                        <button class="tab-btn active" data-type="all">Все</button>
                        <button class="tab-btn" data-type="cpu">Процессоры</button>
                        <button class="tab-btn" data-type="motherboard">Материнки</button>
                        <button class="tab-btn" data-type="ram">ОЗУ</button>
                        <button class="tab-btn" data-type="gpu">Видеокарты</button>
                        <button class="tab-btn" data-type="storage">Накопители</button>
                        <button class="tab-btn" data-type="power">БП</button>
                        <button class="tab-btn" data-type="case">Корпуса</button>
                    </div>

                    <!-- Список товаров -->
                    <div class="component-list" id="storeList"></div>
                    
                    <!-- Кнопки -->
                    <div class="save-buttons">
                        <button class="action-button" id="saveGameBtn">💾 Сохранить</button>
                        <button class="action-button" id="loadGameBtn">📂 Загрузить</button>
                    </div>
                    
                    <button class="action-button" id="resetGameBtn">⟲ Перезапустить</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        (function() {
            // ---------- АССОРТИМЕНТ ----------
            const shopComponents = [
                // CPU
                { id: 'cpu_i5', name: 'Intel i5-12400', brand: 'Intel', type: 'cpu', price: 180, details: '6 ядер', emoji: '⚡' },
                { id: 'cpu_i7', name: 'Intel i7-12700', brand: 'Intel', type: 'cpu', price: 320, details: '12 ядер', emoji: '🔥' },
                { id: 'cpu_i9', name: 'Intel i9-12900K', brand: 'Intel', type: 'cpu', price: 520, details: '16 ядер', emoji: '💎' },
                { id: 'cpu_r5', name: 'Ryzen 5 5600X', brand: 'AMD', type: 'cpu', price: 200, details: '6 ядер', emoji: '⚡' },
                { id: 'cpu_r7', name: 'Ryzen 7 5800X', brand: 'AMD', type: 'cpu', price: 300, details: '8 ядер', emoji: '🔥' },
                { id: 'cpu_r9', name: 'Ryzen 9 5900X', brand: 'AMD', type: 'cpu', price: 400, details: '12 ядер', emoji: '💎' },
                
                // Материнки
                { id: 'mb_b660', name: 'MSI B660', brand: 'MSI', type: 'motherboard', price: 150, details: 'DDR4', emoji: '🔌' },
                { id: 'mb_z690', name: 'ASUS Z690', brand: 'ASUS', type: 'motherboard', price: 280, details: 'DDR5', emoji: '🔌' },
                { id: 'mb_b550', name: 'MSI B550', brand: 'MSI', type: 'motherboard', price: 140, details: 'AM4', emoji: '🔌' },
                { id: 'mb_x570', name: 'Gigabyte X570', brand: 'Gigabyte', type: 'motherboard', price: 250, details: 'AM4', emoji: '🔌' },
                
                // RAM
                { id: 'ram_16', name: '16GB DDR4', brand: 'Corsair', type: 'ram', price: 70, details: '3600MHz', emoji: '🧠' },
                { id: 'ram_32', name: '32GB DDR4', brand: 'G.Skill', type: 'ram', price: 140, details: '3600MHz', emoji: '🧠' },
                { id: 'ram_32_ddr5', name: '32GB DDR5', brand: 'Corsair', type: 'ram', price: 180, details: '5600MHz', emoji: '💡' },
                
                // GPU
                { id: 'gpu_3060', name: 'RTX 3060', brand: 'NVIDIA', type: 'gpu', price: 320, details: '12GB', emoji: '🎮' },
                { id: 'gpu_3070', name: 'RTX 3070', brand: 'NVIDIA', type: 'gpu', price: 500, details: '8GB', emoji: '🔥' },
                { id: 'gpu_3080', name: 'RTX 3080', brand: 'NVIDIA', type: 'gpu', price: 700, details: '10GB', emoji: '💎' },
                { id: 'gpu_6700xt', name: 'RX 6700 XT', brand: 'AMD', type: 'gpu', price: 350, details: '12GB', emoji: '🎮' },
                
                // Storage
                { id: 'ssd_500', name: '500GB NVMe', brand: 'Samsung', type: 'storage', price: 60, details: 'NVMe', emoji: '💾' },
                { id: 'ssd_1tb', name: '1TB NVMe', brand: 'WD', type: 'storage', price: 90, details: 'Gen4', emoji: '💾' },
                { id: 'hdd_2tb', name: '2TB HDD', brand: 'Seagate', type: 'storage', price: 50, details: '7200rpm', emoji: '💾' },
                
                // Power
                { id: 'psu_650', name: '650W Gold', brand: 'Corsair', type: 'power', price: 90, details: 'Gold', emoji: '⚡' },
                { id: 'psu_750', name: '750W Gold', brand: 'Seasonic', type: 'power', price: 120, details: 'Gold', emoji: '⚡' },
                { id: 'psu_850', name: '850W Platinum', brand: 'be quiet!', type: 'power', price: 180, details: 'Platinum', emoji: '💎' },
                
                // Cases
                { id: 'case_4000d', name: 'Corsair 4000D', brand: 'Corsair', type: 'case', price: 90, details: 'Airflow', emoji: '🖳' },
                { id: 'case_h510', name: 'NZXT H510', brand: 'NZXT', type: 'case', price: 80, details: 'Black', emoji: '🖳' },
                { id: 'case_hyte', name: 'Hyte Y60', brand: 'Hyte', type: 'case', price: 160, details: 'Panoramic', emoji: '🖳' },
            ];

            // Начальные значения
            let balance = 5000;
            let level = 1;
            let exp = 0;
            let expToNextLevel = 1000;
            
            // Сборка (детали в ПК)
            const build = { cpu: null, motherboard: null, ram: null, gpu: null, storage: null, power: null, case: null };
            
            // Детали на столе (купленные, но ещё не установленные)
            let benchParts = [];
            
            let currentOrder = generateRandomOrder();
            let currentTab = 'all';

            // Аудио
            const bgMusic = document.getElementById('bgMusic');
            let musicPlaying = false;

            // Управление музыкой
            document.getElementById('playMusicBtn').addEventListener('click', () => {
                bgMusic.volume = 0.3;
                bgMusic.play();
                musicPlaying = true;
                document.getElementById('musicStatus').innerText = '🎵 Lo-fi играет';
            });

            document.getElementById('pauseMusicBtn').addEventListener('click', () => {
                bgMusic.pause();
                musicPlaying = false;
                document.getElementById('musicStatus').innerText = '🎵 Музыка остановлена';
            });

            // Громкость
            let volume = 0.7;
            document.querySelector('.volume-slider').addEventListener('click', (e) => {
                const rect = e.target.getBoundingClientRect();
                const x = e.clientX - rect.left;
                volume = Math.max(0, Math.min(1, x / rect.width));
                bgMusic.volume = volume;
                document.getElementById('volumeFill').style.width = volume * 100 + '%';
            });

            // Генерация заказа
            function generateRandomOrder() {
                const byType = (type) => shopComponents.filter(c => c.type === type);
                return {
                    cpu: byType('cpu')[Math.floor(Math.random() * byType('cpu').length)],
                    motherboard: byType('motherboard')[Math.floor(Math.random() * byType('motherboard').length)],
                    ram: byType('ram')[Math.floor(Math.random() * byType('ram').length)],
                    gpu: byType('gpu')[Math.floor(Math.random() * byType('gpu').length)],
                    storage: byType('storage')[Math.floor(Math.random() * byType('storage').length)],
                    power: byType('power')[Math.floor(Math.random() * byType('power').length)],
                    case: byType('case')[Math.floor(Math.random() * byType('case').length)]
                };
            }

            // Уведомления
            function showNotification(message, type = 'info') {
                const container = document.getElementById('notificationContainer');
                const notification = document.createElement('div');
                notification.className = `notification ${type}`;
                notification.innerHTML = `<span>${message}</span>`;
                container.appendChild(notification);
                setTimeout(() => notification.remove(), 3000);
            }

            // Добавление опыта
            function addExp(amount) {
                exp += amount;
                while (exp >= expToNextLevel) {
                    level++;
                    exp -= expToNextLevel;
                    expToNextLevel = Math.floor(expToNextLevel * 1.5);
                    showNotification(`🎉 Уровень ${level}!`, 'success');
                }
                document.getElementById('levelDisplay').innerText = `УРОВЕНЬ ${level}`;
                document.getElementById('expBar').style.width = `${(exp / expToNextLevel) * 100}%`;
            }

            // Рендер реалистичного ПК
            function renderRealPC() {
                const grid = document.getElementById('realPcSlots');
                const slots = [
                    { type: 'cpu', label: 'CPU' },
                    { type: 'motherboard', label: 'MOTHERBOARD' },
                    { type: 'ram', label: 'RAM' },
                    { type: 'gpu', label: 'GPU' },
                    { type: 'storage', label: 'STORAGE' },
                    { type: 'power', label: 'PSU' },
                    { type: 'case', label: 'CASE' }
                ];
                
                let html = '';
                slots.forEach(slot => {
                    const comp = build[slot.type];
                    html += `<div class="part-slot-real" data-slot="${slot.type}" draggable="false">
                        <div class="slot-label-small">${slot.label}</div>
                        ${comp ? 
                            `<div class="real-part ${slot.type}">${comp.emoji}</div>
                             <div class="part-name-real">${comp.name}</div>` : 
                            `<div class="empty-slot-real">🔲 пусто</div>`
                        }
                    </div>`;
                });
                grid.innerHTML = html;

                // Подсчет деталей
                const count = Object.values(build).filter(v => v !== null).length;
                document.getElementById('realPartsCount').innerText = `${count}/7 деталей`;

                // Обработчики для слотов (принимают перетаскиваемые детали)
                document.querySelectorAll('.part-slot-real').forEach(slot => {
                    slot.addEventListener('dragover', (e) => e.preventDefault());
                    
                    slot.addEventListener('drop', (e) => {
                        e.preventDefault();
                        const slotType = slot.dataset.slot;
                        const partId = e.dataTransfer.getData('text/plain');
                        const part = benchParts.find(p => p.id === partId);
                        
                        if (!part) return;
                        
                        // Проверяем тип
                        if (part.type !== slotType) {
                            showNotification(`❌ Это не подходит для слота ${slotType}!`, 'error');
                            return;
                        }
                        
                        // Устанавливаем деталь
                        build[slotType] = part;
                        
                        // Удаляем со стола
                        benchParts = benchParts.filter(p => p.id !== partId);
                        
                        renderAll();
                        showNotification(`✅ ${part.name} установлен!`, 'success');
                    });
                });
            }

            // Рендер стола
            function renderBench() {
                const bench = document.getElementById('benchParts');
                
                if (benchParts.length === 0) {
                    bench.innerHTML = '<div style="color: #9abed9; padding: 20px;">Перетащи детали из магазина сюда</div>';
                    return;
                }
                
                let html = '';
                benchParts.forEach(part => {
                    html += `<div class="bench-part" draggable="true" data-part-id="${part.id}">
                        <div class="shop-icon" style="width: 50px; height: 50px; font-size: 28px;">${part.emoji}</div>
                        <div class="part-name-real">${part.name}</div>
                    </div>`;
                });
                bench.innerHTML = html;

                // Добавляем draggable
                document.querySelectorAll('.bench-part').forEach(part => {
                    part.addEventListener('dragstart', (e) => {
                        e.dataTransfer.setData('text/plain', part.dataset.partId);
                    });
                });
            }

            // Рендер магазина
            function renderShop() {
                const listDiv = document.getElementById('storeList');
                let filtered = currentTab === 'all' ? shopComponents : shopComponents.filter(c => c.type === currentTab);
                
                let html = '';
                filtered.forEach(comp => {
                    html += `<div class="component-item" draggable="true" data-comp-id="${comp.id}">
                        <div class="shop-icon">${comp.emoji}</div>
                        <div class="comp-name">${comp.name}</div>
                        <div class="comp-desc">${comp.brand} • ${comp.details}</div>
                        <div class="comp-price">${comp.price}</div>
                    </div>`;
                });
                listDiv.innerHTML = html;

                // Добавляем перетаскивание из магазина
                document.querySelectorAll('.component-item').forEach(item => {
                    item.addEventListener('dragstart', (e) => {
                        const compId = item.dataset.compId;
                        const component = shopComponents.find(c => c.id === compId);
                        
                        if (balance < component.price) {
                            e.preventDefault();
                            showNotification('❌ Не хватает денег!', 'error');
                            return;
                        }
                        
                        // Покупаем и кладём на стол
                        balance -= component.price;
                        benchParts.push({ ...component });
                        
                        showNotification(`✅ Куплен: ${component.name}`, 'success');
                        renderAll();
                        
                        e.dataTransfer.setData('text/plain', component.id);
                    });
                });
            }

            // Рендер заказа
            function renderOrder() {
                const o = currentOrder;
                document.getElementById('orderRequirements').innerHTML = `
                    <div>CPU: <b>${o.cpu.name}</b></div>
                    <div>Мать: <b>${o.motherboard.name}</b></div>
                    <div>RAM: <b>${o.ram.name}</b></div>
                    <div>GPU: <b>${o.gpu.name}</b></div>
                    <div>Диск: <b>${o.storage.name}</b></div>
                    <div>БП: <b>${o.power.name}</b></div>
                    <div>Корпус: <b>${o.case.name}</b></div>
                `;
            }

            // Проверка соответствия
            function isBuildMatchesOrder() {
                return (
                    build.cpu?.id === currentOrder.cpu.id &&
                    build.motherboard?.id === currentOrder.motherboard.id &&
                    build.ram?.id === currentOrder.ram.id &&
                    build.gpu?.id === currentOrder.gpu.id &&
                    build.storage?.id === currentOrder.storage.id &&
                    build.power?.id === currentOrder.power.id &&
                    build.case?.id === currentOrder.case.id
                );
            }

            // Выполнение заказа
            function completeOrder() {
                if (!isBuildMatchesOrder()) {
                    showNotification('❌ Сборка не соответствует заказу!', 'error');
                    return;
                }
                
                const parts = [currentOrder.cpu, currentOrder.motherboard, currentOrder.ram, currentOrder.gpu, currentOrder.storage, currentOrder.power, currentOrder.case];
                const totalCost = parts.reduce((sum, p) => sum + p.price, 0);
                const reward = Math.floor(totalCost * 1.7);
                
                balance += reward;
                addExp(totalCost);
                
                showNotification(`✅ Заказ выполнен! +$${reward}`, 'success');
                
                // Очищаем сборку
                for (let key in build) build[key] = null;
                currentOrder = generateRandomOrder();
                
                renderAll();
            }

            // Новый заказ
            function newOrder() {
                currentOrder = generateRandomOrder();
                showNotification('🔄 Новый клиент!', 'info');
                renderAll();
            }

            // Сброс
            function resetGame() {
                if (confirm('Перезапустить игру?')) {
                    balance = 5000;
                    level = 1;
                    exp = 0;
                    for (let key in build) build[key] = null;
                    benchParts = [];
                    currentOrder = generateRandomOrder();
                    renderAll();
                }
            }

            // Сохранение
            function saveGame() {
                const gameState = {
                    balance, level, exp, expToNextLevel,
                    build, benchParts, currentOrder
                };
                localStorage.setItem('pcBuilderSave', JSON.stringify(gameState));
                showNotification('Игра сохранена!', 'success');
            }

            // Загрузка
            function loadGame() {
                const saved = localStorage.getItem('pcBuilderSave');
                if (!saved) {
                    showNotification('Нет сохранения!', 'warning');
                    return;
                }
                const gameState = JSON.parse(saved);
                balance = gameState.balance;
                level = gameState.level;
                exp = gameState.exp;
                expToNextLevel = gameState.expToNextLevel;
                Object.assign(build, gameState.build);
                benchParts = gameState.benchParts || [];
                currentOrder = gameState.currentOrder;
                renderAll();
                showNotification('Игра загружена!', 'success');
            }

            // Вкладки
            function setActiveTab(type) {
                currentTab = type;
                document.querySelectorAll('.tab-btn').forEach(btn => {
                    if (btn.dataset.type === type) btn.classList.add('active');
                    else btn.classList.remove('active');
                });
                renderShop();
            }

            // Полный рендер
            function renderAll() {
                document.getElementById('balanceDisplay').innerText = balance;
                renderRealPC();
                renderBench();
                renderOrder();
                renderShop();
                document.getElementById('completeOrderBtn').disabled = !isBuildMatchesOrder();
            }

            // Инициализация
            window.addEventListener('load', () => {
                renderAll();

                document.getElementById('newOrderBtn').addEventListener('click', newOrder);
                document.getElementById('completeOrderBtn').addEventListener('click', completeOrder);
                document.getElementById('resetGameBtn').addEventListener('click', resetGame);
                document.getElementById('saveGameBtn').addEventListener('click', saveGame);
                document.getElementById('loadGameBtn').addEventListener('click', loadGame);

                document.querySelectorAll('.tab-btn').forEach(btn => {
                    btn.addEventListener('click', (e) => setActiveTab(e.target.dataset.type));
                });
            });
        })();
    </script>
</body>
</html>
