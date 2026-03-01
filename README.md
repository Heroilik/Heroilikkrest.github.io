<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D МАЙНИНГ ФЕРМА - Танцующие клиенты</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden;
            background: #0a0a1a;
        }

        #ui-container {
            position: absolute;
            top: 20px;
            left: 20px;
            right: 20px;
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            z-index: 1000;
            pointer-events: none;
        }

        #stats-panel {
            background: rgba(10, 20, 30, 0.95);
            backdrop-filter: blur(10px);
            border: 2px solid #00ffaa;
            border-radius: 20px;
            padding: 20px;
            color: white;
            min-width: 320px;
            box-shadow: 0 0 30px rgba(0, 255, 170, 0.3);
            pointer-events: auto;
        }

        #balance {
            font-size: 48px;
            font-weight: bold;
            color: #00ffaa;
            text-shadow: 0 0 20px #00ffaa;
            line-height: 1.2;
        }

        #income {
            font-size: 24px;
            color: #ffaa00;
            margin-top: 10px;
        }

        #mining-power {
            font-size: 18px;
            color: #888;
            margin-top: 5px;
        }

        #controls-panel {
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 50px;
            padding: 12px 25px;
            color: white;
            font-size: 14px;
            pointer-events: auto;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .key {
            background: rgba(255, 255, 255, 0.2);
            padding: 4px 10px;
            border-radius: 8px;
            margin: 0 4px;
            border: 1px solid rgba(255, 255, 255, 0.3);
        }

        #music-toggle {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid #ffaa00;
            color: #ffaa00;
            padding: 8px 15px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s;
            margin-left: 10px;
        }

        #music-toggle:hover {
            background: #ffaa00;
            color: black;
        }

        #shop-panel {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            max-width: 1400px;
            background: rgba(20, 30, 45, 0.95);
            backdrop-filter: blur(10px);
            border: 2px solid #ffaa00;
            border-radius: 25px;
            padding: 15px;
            color: white;
            box-shadow: 0 0 40px rgba(255, 170, 0, 0.3);
            max-height: 350px;
            z-index: 1000;
            pointer-events: auto;
            transition: transform 0.3s ease, bottom 0.3s ease;
        }

        #shop-panel.hidden {
            bottom: -370px;
        }

        #toggle-shop {
            position: absolute;
            bottom: 370px;
            left: 50%;
            transform: translateX(-50%);
            background: #ffaa00;
            color: black;
            border: none;
            border-radius: 20px 20px 0 0;
            padding: 10px 30px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            z-index: 1001;
            transition: bottom 0.3s ease;
            box-shadow: 0 -5px 15px rgba(255, 170, 0, 0.3);
        }

        #toggle-shop.hidden {
            bottom: 20px;
        }

        #shop-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            padding-bottom: 10px;
            border-bottom: 2px solid rgba(255, 170, 0, 0.3);
        }

        #shop-header h2 {
            color: #ffaa00;
            font-size: 20px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        #shop-categories {
            display: flex;
            gap: 8px;
            margin-bottom: 10px;
            flex-wrap: wrap;
        }

        .category-btn {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 170, 0, 0.3);
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 13px;
            transition: all 0.3s;
        }

        .category-btn:hover {
            background: rgba(255, 170, 0, 0.2);
            border-color: #ffaa00;
        }

        .category-btn.active {
            background: #ffaa00;
            color: black;
            border-color: #ffaa00;
        }

        #shop-items {
            display: flex;
            gap: 10px;
            overflow-x: auto;
            padding: 5px 0 15px 0;
            scroll-behavior: smooth;
        }

        #shop-items::-webkit-scrollbar {
            height: 6px;
        }

        #shop-items::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 3px;
        }

        #shop-items::-webkit-scrollbar-thumb {
            background: #ffaa00;
            border-radius: 3px;
        }

        .shop-item {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 12px;
            padding: 12px;
            cursor: pointer;
            transition: all 0.3s;
            border-left: 4px solid #ffaa00;
            min-width: 200px;
            flex: 0 0 auto;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .shop-item:hover {
            transform: translateY(-5px);
            background: rgba(255, 170, 0, 0.15);
            box-shadow: 0 5px 20px rgba(255, 170, 0, 0.3);
        }

        .shop-item.owned {
            border-left-color: #00ffaa;
            opacity: 0.9;
        }

        .shop-item.owned::after {
            content: "✓";
            position: absolute;
            top: 5px;
            right: 5px;
            color: #00ffaa;
            font-size: 20px;
            font-weight: bold;
        }

        .item-icon {
            width: 45px;
            height: 45px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
        }

        .item-info {
            flex: 1;
        }

        .item-name {
            font-size: 14px;
            font-weight: bold;
            color: #ffaa00;
            margin-bottom: 3px;
        }

        .item-price {
            color: #00ffaa;
            font-size: 12px;
            margin-bottom: 2px;
        }

        .item-income {
            color: #ffaa00;
            font-size: 11px;
        }

        .item-specs {
            font-size: 10px;
            color: #888;
            margin-top: 3px;
        }

        #action-buttons {
            position: absolute;
            bottom: 420px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 20px;
            z-index: 1000;
            pointer-events: auto;
        }

        .action-btn {
            padding: 15px 30px;
            border: none;
            border-radius: 60px;
            font-size: 20px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
            letter-spacing: 2px;
            box-shadow: 0 0 30px;
            min-width: 200px;
        }

        #mining-button {
            background: linear-gradient(135deg, #00ffaa, #00cc88);
            color: black;
            box-shadow: 0 0 40px #00ffaa;
            animation: pulse 2s infinite;
        }

        #boost-button {
            background: linear-gradient(135deg, #ffaa00, #ff8800);
            color: black;
            box-shadow: 0 0 40px #ffaa00;
            position: relative;
            overflow: hidden;
        }

        #boost-button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            animation: none;
            filter: grayscale(0.5);
        }

        #boost-cooldown {
            position: absolute;
            top: 0;
            left: 0;
            height: 100%;
            background: rgba(0, 0, 0, 0.3);
            width: 0%;
            transition: width 1s linear;
            pointer-events: none;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .boost-active {
            animation: boostPulse 0.5s infinite !important;
        }

        @keyframes boostPulse {
            0% { box-shadow: 0 0 40px #ffaa00; }
            50% { box-shadow: 0 0 80px #ffaa00, 0 0 120px #ff5500; }
            100% { box-shadow: 0 0 40px #ffaa00; }
        }

        #progress-container {
            position: absolute;
            bottom: 490px;
            left: 50%;
            transform: translateX(-50%);
            width: 500px;
            background: rgba(0, 0, 0, 0.7);
            border-radius: 15px;
            padding: 5px;
            border: 2px solid #00ffaa;
            z-index: 1000;
            cursor: pointer;
        }

        #progress-bar {
            height: 25px;
            background: linear-gradient(90deg, #00ffaa, #00cc88);
            border-radius: 10px;
            width: 0%;
            transition: width 0.3s;
            position: relative;
            overflow: hidden;
        }

        #progress-bar::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
            animation: shine 2s infinite;
        }

        #progress-text {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: black;
            font-weight: bold;
            font-size: 14px;
            z-index: 1;
        }

        #computers-panel {
            position: absolute;
            top: 50%;
            left: 20px;
            transform: translateY(-50%);
            background: rgba(0, 0, 0, 0.8);
            border: 2px solid #00ffaa;
            border-radius: 15px;
            padding: 15px;
            color: white;
            z-index: 1000;
            pointer-events: auto;
            width: 150px;
        }

        #computers-header {
            text-align: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }

        #computers-header h3 {
            color: #00ffaa;
            font-size: 16px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        #computers-grid {
            display: flex;
            flex-direction: column;
            gap: 8px;
            max-height: 300px;
            overflow-y: auto;
            padding-right: 5px;
        }

        #computers-grid::-webkit-scrollbar {
            width: 4px;
        }

        #computers-grid::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
        }

        #computers-grid::-webkit-scrollbar-thumb {
            background: #00ffaa;
            border-radius: 2px;
        }

        .computer-slot {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid #ffaa00;
            border-radius: 8px;
            padding: 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            width: 100%;
        }

        .computer-slot:hover {
            background: rgba(255, 170, 0, 0.2);
            transform: translateX(5px);
        }

        .computer-slot.active {
            border: 2px solid #00ffaa;
            background: rgba(0, 255, 170, 0.1);
        }

        .computer-slot.empty {
            border-color: #666;
            opacity: 0.5;
            cursor: default;
        }

        .computer-slot.empty:hover {
            transform: none;
            background: rgba(255, 255, 255, 0.1);
        }

        .computer-slot .name {
            font-weight: bold;
            color: #ffaa00;
            font-size: 14px;
            margin-bottom: 3px;
        }

        .computer-slot .power {
            color: #00ffaa;
            font-size: 11px;
        }

        .computer-slot .count {
            font-size: 10px;
            color: #888;
        }

        #new-computer-btn {
            background: linear-gradient(135deg, #00ffaa, #00cc88);
            color: black;
            border: none;
            border-radius: 8px;
            padding: 10px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 13px;
            box-shadow: 0 0 20px #00ffaa;
            margin-top: 15px;
            width: 100%;
        }

        #new-computer-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 0 30px #00ffaa;
        }

        #new-computer-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        #client-panel {
            position: absolute;
            bottom: 120px;
            right: 20px;
            background: rgba(0, 0, 0, 0.9);
            border: 2px solid #ffaa00;
            border-radius: 15px;
            padding: 15px;
            color: white;
            z-index: 1000;
            pointer-events: auto;
            width: 300px;
            transition: transform 0.3s ease, opacity 0.3s ease;
        }

        #client-panel.hidden {
            transform: translateX(320px);
            opacity: 0;
            pointer-events: none;
        }

        #client-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid rgba(255, 170, 0, 0.3);
        }

        #client-header h3 {
            color: #ffaa00;
            font-size: 18px;
        }

        .client-header-buttons {
            display: flex;
            gap: 5px;
        }

        .client-header-btn {
            background: none;
            border: none;
            color: #ffaa00;
            font-size: 18px;
            cursor: pointer;
            padding: 0 8px;
            transition: all 0.3s;
            border-radius: 5px;
        }

        .client-header-btn:hover {
            background: rgba(255, 170, 0, 0.2);
        }

        #close-client {
            color: #ff5555;
        }

        #close-client:hover {
            background: rgba(255, 85, 85, 0.2);
        }

        #client-avatar {
            width: 60px;
            height: 60px;
            background: linear-gradient(135deg, #ffaa00, #ff8800);
            border-radius: 50%;
            margin: 0 auto 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            border: 2px solid #00ffaa;
        }

        #client-name {
            text-align: center;
            color: #ffaa00;
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        #client-message {
            text-align: center;
            color: #888;
            font-size: 14px;
            margin-bottom: 15px;
            font-style: italic;
        }

        #order-requirements {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 15px;
        }

        .requirement-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 13px;
        }

        .requirement-item .label {
            color: #888;
        }

        .requirement-item .value {
            color: #ffaa00;
            font-weight: bold;
        }

        .requirement-item .status {
            color: #00ffaa;
        }

        .requirement-item .status.missing {
            color: #ff5555;
        }

        #sell-button {
            background: linear-gradient(135deg, #00ffaa, #00cc88);
            color: black;
            border: none;
            border-radius: 10px;
            padding: 12px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            width: 100%;
            font-size: 16px;
        }

        #sell-button:hover {
            transform: scale(1.05);
            box-shadow: 0 0 20px #00ffaa;
        }

        #sell-button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        #order-reward {
            text-align: center;
            margin-top: 10px;
            color: #00ffaa;
            font-size: 14px;
        }

        #notification {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: linear-gradient(135deg, #00ffaa, #00cc88);
            color: black;
            padding: 25px 60px;
            border-radius: 60px;
            font-size: 28px;
            font-weight: bold;
            z-index: 2000;
            display: none;
            box-shadow: 0 0 60px #00ffaa;
            animation: notificationFade 2s ease-out;
            white-space: nowrap;
        }

        @keyframes notificationFade {
            0% { opacity: 0; transform: translate(-50%, -30%); }
            20% { opacity: 1; transform: translate(-50%, -50%); }
            80% { opacity: 1; transform: translate(-50%, -50%); }
            100% { opacity: 0; transform: translate(-50%, -70%); }
        }

        .floating-particle {
            position: absolute;
            color: #00ffaa;
            font-weight: bold;
            font-size: 20px;
            pointer-events: none;
            animation: floatUp 1.5s ease-out forwards;
            z-index: 2000;
        }

        @keyframes floatUp {
            0% { opacity: 1; transform: translateY(0); }
            100% { opacity: 0; transform: translateY(-200px); }
        }

        #boost-timer {
            position: absolute;
            top: -40px;
            left: 50%;
            transform: translateX(-50%);
            background: #ffaa00;
            color: black;
            padding: 5px 15px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 18px;
            white-space: nowrap;
        }

        ::-webkit-scrollbar {
            width: 8px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
        }

        ::-webkit-scrollbar-thumb {
            background: #ffaa00;
            border-radius: 4px;
        }

        #open-client-btn {
            position: absolute;
            bottom: 120px;
            right: 20px;
            background: #ffaa00;
            color: black;
            border: none;
            border-radius: 50px;
            padding: 10px 20px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            z-index: 1000;
            box-shadow: 0 0 20px #ffaa00;
            display: none;
        }

        #open-client-btn.visible {
            display: block;
        }
    </style>
</head>
<body>
    <div id="ui-container">
        <div id="stats-panel">
            <div style="font-size: 14px; color: #888; margin-bottom: 5px;">БАЛАНС</div>
            <div id="balance">200</div>
            <div style="margin-top: 15px; font-size: 14px; color: #888;">ДОХОД В СЕКУНДУ</div>
            <div id="income">0</div>
            <div id="mining-power">Мощность фермы: 0 GH/s</div>
            
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 20px;">
                <div style="background: rgba(255,255,255,0.05); border-radius: 10px; padding: 10px; text-align: center;">
                    <div style="font-size: 24px; font-weight: bold; color: #ffaa00;" id="computers-count">1</div>
                    <div style="font-size: 12px; color: #888;">Компьютеров</div>
                </div>
                <div style="background: rgba(255,255,255,0.05); border-radius: 10px; padding: 10px; text-align: center;">
                    <div style="font-size: 24px; font-weight: bold; color: #00ffaa;" id="total-parts">0</div>
                    <div style="font-size: 12px; color: #888;">Компонентов</div>
                </div>
            </div>
        </div>

        <div id="controls-panel">
            <span class="key">🖱 ЛКМ</span> вращать
            <span class="key">🖱 ПКМ</span> двигать
            <span class="key">R</span> сброс камеры
            <button id="music-toggle">🔊 Музыка вкл</button>
        </div>
    </div>

    <button id="toggle-shop">▲ Скрыть магазин</button>

    <div id="shop-panel">
        <div id="shop-header">
            <h2>🏪 МАГАЗИН КОМПЛЕКТУЮЩИХ</h2>
        </div>
        
        <div id="shop-categories">
            <button class="category-btn active" onclick="filterCategory('all')">Все</button>
            <button class="category-btn" onclick="filterCategory('cpu')">Процессоры</button>
            <button class="category-btn" onclick="filterCategory('gpu')">Видеокарты</button>
            <button class="category-btn" onclick="filterCategory('ram')">ОЗУ</button>
            <button class="category-btn" onclick="filterCategory('storage')">Накопители</button>
            <button class="category-btn" onclick="filterCategory('cooling')">Охлаждение</button>
            <button class="category-btn" onclick="filterCategory('psu')">БП</button>
        </div>

        <div id="shop-items"></div>
    </div>

    <div id="client-panel">
        <div id="client-header">
            <h3>👤 КЛИЕНТ</h3>
            <div class="client-header-buttons">
                <button class="client-header-btn" id="minimize-client">🗕</button>
                <button class="client-header-btn" id="close-client">✖</button>
            </div>
        </div>
        <div id="client-avatar">👨‍💼</div>
        <div id="client-name">Алексей</div>
        <div id="client-message">"Соберите ПК для моих задач"</div>
        <div id="order-requirements">
            <div class="requirement-item">
                <span class="label">Требуемый процессор:</span>
                <span class="value" id="req-cpu">Intel Core i7</span>
                <span class="status" id="status-cpu">❌</span>
            </div>
            <div class="requirement-item">
                <span class="label">Требуемая видеокарта:</span>
                <span class="value" id="req-gpu">RTX 3060</span>
                <span class="status" id="status-gpu">❌</span>
            </div>
            <div class="requirement-item">
                <span class="label">Минимум ОЗУ:</span>
                <span class="value" id="req-ram">16GB</span>
                <span class="status" id="status-ram">❌</span>
            </div>
            <div class="requirement-item">
                <span class="label">Минимум накопитель:</span>
                <span class="value" id="req-storage">500GB</span>
                <span class="status" id="status-storage">❌</span>
            </div>
        </div>
        <button id="sell-button" disabled>💰 Продать ПК (награда: 500💰)</button>
        <div id="order-reward">+500 монет за выполнение заказа</div>
    </div>

    <button id="open-client-btn" class="visible">👤 Открыть клиента</button>

    <div id="computers-panel">
        <div id="computers-header">
            <h3>🖥 МОИ ПК</h3>
        </div>
        <div id="computers-grid"></div>
        <button id="new-computer-btn">➕ Купить ПК (2000💰)</button>
    </div>

    <div id="action-buttons">
        <button id="mining-button" class="action-btn">⛏ МАЙНИНГ +5</button>
        <button id="boost-button" class="action-btn">
            ⚡ X2 БУСТ
            <div id="boost-cooldown"></div>
            <div id="boost-timer"></div>
        </button>
    </div>

    <div id="progress-container">
        <div id="progress-bar">
            <div id="progress-text">0%</div>
        </div>
    </div>

    <div id="notification">+20 монет!</div>

    <script type="importmap">
        {
            "imports": {
                "three": "https://unpkg.com/three@0.128.0/build/three.module.js"
            }
        }
    </script>

    <script type="module">
        import * as THREE from 'three';
        import { OrbitControls } from 'https://unpkg.com/three@0.128.0/examples/jsm/controls/OrbitControls.js';

        // ============ МУЗЫКА ============
        let audioContext = null;
        let musicEnabled = false;
        let oscillator = null;
        let gainNode = null;

        function initAudio() {
            if (audioContext) return;
            
            audioContext = new (window.AudioContext || window.webkitAudioContext)();
            gainNode = audioContext.createGain();
            gainNode.gain.value = 0.1;
            gainNode.connect(audioContext.destination);
            
            createMusic();
        }

        function createMusic() {
            if (!audioContext) return;
            
            oscillator = audioContext.createOscillator();
            oscillator.type = 'sine';
            oscillator.frequency.value = 440;
            
            const bassOsc = audioContext.createOscillator();
            bassOsc.type = 'triangle';
            bassOsc.frequency.value = 110;
            
            const filter = audioContext.createBiquadFilter();
            filter.type = 'lowpass';
            filter.frequency.value = 1000;
            
            oscillator.connect(filter);
            bassOsc.connect(filter);
            filter.connect(gainNode);
            
            oscillator.start();
            bassOsc.start();
            
            setInterval(() => {
                if (musicEnabled && audioContext) {
                    const now = audioContext.currentTime;
                    gainNode.gain.exponentialRampToValueAtTime(0.15, now + 0.1);
                    gainNode.gain.exponentialRampToValueAtTime(0.08, now + 0.3);
                }
            }, 500);
            
            let note = 0;
            setInterval(() => {
                if (musicEnabled && audioContext) {
                    const notes = [440, 554, 659, 880];
                    oscillator.frequency.exponentialRampToValueAtTime(
                        notes[note % 4], 
                        audioContext.currentTime + 0.1
                    );
                    note++;
                }
            }, 300);
        }

        function toggleMusic() {
            const btn = document.getElementById('music-toggle');
            
            if (!musicEnabled) {
                initAudio();
                if (audioContext) {
                    if (audioContext.state === 'suspended') {
                        audioContext.resume();
                    }
                    musicEnabled = true;
                    btn.textContent = '🔊 Музыка выкл';
                    btn.style.background = '#ffaa00';
                    btn.style.color = 'black';
                }
            } else {
                if (audioContext) {
                    audioContext.suspend();
                    musicEnabled = false;
                    btn.textContent = '🔈 Музыка вкл';
                    btn.style.background = 'rgba(255,255,255,0.1)';
                    btn.style.color = '#ffaa00';
                }
            }
        }

        document.getElementById('music-toggle').addEventListener('click', toggleMusic);

        // ============ LOCALSTORAGE ============
        function saveGame() {
            const gameState = {
                computers: computers.map(comp => ({
                    ...comp,
                    components: [...comp.components]
                })),
                balance: balance,
                totalIncome: totalIncome,
                totalPower: totalPower,
                currentComputer: currentComputer,
                currentOrder: currentOrder
            };
            localStorage.setItem('miningFarmSave', JSON.stringify(gameState));
            console.log('Игра сохранена');
        }

        function loadGame() {
            const saved = localStorage.getItem('miningFarmSave');
            if (saved) {
                try {
                    const gameState = JSON.parse(saved);
                    
                    computers = gameState.computers.map(comp => ({
                        ...comp,
                        components: [...comp.components]
                    }));
                    
                    balance = gameState.balance;
                    totalIncome = gameState.totalIncome;
                    totalPower = gameState.totalPower;
                    currentComputer = gameState.currentComputer;
                    if (gameState.currentOrder) {
                        currentOrder = gameState.currentOrder;
                    }
                    
                    console.log('Игра загружена');
                    
                    for (let i = 1; i < computers.length; i++) {
                        if (i < computerPositions.length) {
                            const pos = computerPositions[i];
                            const newComputer = createComputerCase(pos[0], pos[1], i);
                            scene.add(newComputer);
                            computerModels.push(newComputer);
                        }
                    }
                    
                    return true;
                } catch (e) {
                    console.error('Ошибка загрузки:', e);
                }
            }
            return false;
        }

        function resetGame() {
            if (confirm('Начать новую игру? Весь прогресс будет потерян!')) {
                localStorage.removeItem('miningFarmSave');
                location.reload();
            }
        }

        const saveBtn = document.createElement('button');
        saveBtn.textContent = '💾 Сохранить';
        saveBtn.style.background = '#00ffaa';
        saveBtn.style.color = 'black';
        saveBtn.style.border = 'none';
        saveBtn.style.padding = '8px 15px';
        saveBtn.style.borderRadius = '30px';
        saveBtn.style.cursor = 'pointer';
        saveBtn.style.marginLeft = '10px';
        saveBtn.style.fontWeight = 'bold';
        
        const loadBtn = document.createElement('button');
        loadBtn.textContent = '📂 Загрузить';
        loadBtn.style.background = '#ffaa00';
        loadBtn.style.color = 'black';
        loadBtn.style.border = 'none';
        loadBtn.style.padding = '8px 15px';
        loadBtn.style.borderRadius = '30px';
        loadBtn.style.cursor = 'pointer';
        loadBtn.style.marginLeft = '10px';
        loadBtn.style.fontWeight = 'bold';
        
        const resetBtn = document.createElement('button');
        resetBtn.textContent = '🔄 Новая игра';
        resetBtn.style.background = '#ff5555';
        resetBtn.style.color = 'white';
        resetBtn.style.border = 'none';
        resetBtn.style.padding = '8px 15px';
        resetBtn.style.borderRadius = '30px';
        resetBtn.style.cursor = 'pointer';
        resetBtn.style.marginLeft = '10px';
        resetBtn.style.fontWeight = 'bold';
        
        document.getElementById('controls-panel').appendChild(saveBtn);
        document.getElementById('controls-panel').appendChild(loadBtn);
        document.getElementById('controls-panel').appendChild(resetBtn);
        
        saveBtn.addEventListener('click', saveGame);
        loadBtn.addEventListener('click', () => {
            if (loadGame()) {
                updateUI();
                showNotification('Игра загружена!');
            } else {
                showNotification('Нет сохранения!');
            }
        });
        resetBtn.addEventListener('click', resetGame);

        setInterval(saveGame, 30000);

        // ============ РАСШИРЕННЫЙ СПИСОК КОМПЛЕКТУЮЩИХ ============
        const componentsData = [
            // ПРОЦЕССОРЫ (УВЕЛИЧЕНО)
            { id: 'cpu_i3', name: 'Intel Core i3-12100', category: 'cpu', price: 150, income: 0.5, power: 4, tier: 1, icon: '🔹', specs: '4 ядра, 8 потоков' },
            { id: 'cpu_i5', name: 'Intel Core i5-13600K', category: 'cpu', price: 350, income: 1.2, power: 14, tier: 2, icon: '🔶', specs: '14 ядер, 20 потоков' },
            { id: 'cpu_i7', name: 'Intel Core i7-13700K', category: 'cpu', price: 500, income: 2, power: 20, tier: 3, icon: '💠', specs: '16 ядер, 24 потока' },
            { id: 'cpu_i9', name: 'Intel Core i9-13900KS', category: 'cpu', price: 800, income: 3, power: 30, tier: 4, icon: '💎', specs: '24 ядра, 32 потока' },
            { id: 'cpu_i9_14900k', name: 'Intel Core i9-14900K', category: 'cpu', price: 1200, income: 4.5, power: 40, tier: 5, icon: '👑', specs: '24 ядра, 32 потока' },
            { id: 'cpu_ryzen5', name: 'AMD Ryzen 5 7600X', category: 'cpu', price: 300, income: 1, power: 12, tier: 2, icon: '🔸', specs: '6 ядер, 12 потоков' },
            { id: 'cpu_ryzen7', name: 'AMD Ryzen 7 7800X3D', category: 'cpu', price: 550, income: 2.2, power: 22, tier: 3, icon: '🔷', specs: '8 ядер, 16 потоков' },
            { id: 'cpu_ryzen9', name: 'AMD Ryzen 9 7950X', category: 'cpu', price: 900, income: 3.5, power: 35, tier: 4, icon: '⚡', specs: '16 ядер, 32 потока' },
            { id: 'cpu_ryzen9_7950x3d', name: 'AMD Ryzen 9 7950X3D', category: 'cpu', price: 1300, income: 5, power: 45, tier: 5, icon: '🔥', specs: '16 ядер, 32 потока' },
            { id: 'cpu_threadripper', name: 'AMD Threadripper 7980X', category: 'cpu', price: 2500, income: 8, power: 80, tier: 6, icon: '💢', specs: '64 ядра, 128 потоков' },
            { id: 'cpu_epyc', name: 'AMD EPYC 9654', category: 'cpu', price: 5000, income: 15, power: 150, tier: 7, icon: '⚙️', specs: '96 ядра, 192 потока' },
            { id: 'cpu_xeon', name: 'Intel Xeon Platinum 8480+', category: 'cpu', price: 6000, income: 18, power: 180, tier: 7, icon: '🏭', specs: '56 ядер, 112 потоков' },

            // ВИДЕОКАРТЫ (СИЛЬНО УВЕЛИЧЕНО)
            { id: 'gpu_1650', name: 'NVIDIA GTX 1650', category: 'gpu', price: 200, income: 0.6, power: 6, tier: 1, icon: '🎮', specs: '4GB GDDR6' },
            { id: 'gpu_1660', name: 'NVIDIA GTX 1660 Super', category: 'gpu', price: 300, income: 1, power: 10, tier: 2, icon: '🎯', specs: '6GB GDDR6' },
            { id: 'gpu_2060', name: 'NVIDIA RTX 2060', category: 'gpu', price: 400, income: 1.5, power: 15, tier: 2, icon: '✨', specs: '6GB GDDR6' },
            { id: 'gpu_2070', name: 'NVIDIA RTX 2070 Super', category: 'gpu', price: 550, income: 2, power: 20, tier: 3, icon: '💫', specs: '8GB GDDR6' },
            { id: 'gpu_2080', name: 'NVIDIA RTX 2080 Ti', category: 'gpu', price: 800, income: 3, power: 30, tier: 3, icon: '🚀', specs: '11GB GDDR6' },
            { id: 'gpu_3060', name: 'NVIDIA RTX 3060 Ti', category: 'gpu', price: 600, income: 2.5, power: 25, tier: 3, icon: '⚡', specs: '8GB GDDR6' },
            { id: 'gpu_3070', name: 'NVIDIA RTX 3070', category: 'gpu', price: 800, income: 3.5, power: 35, tier: 3, icon: '💫', specs: '8GB GDDR6' },
            { id: 'gpu_3080', name: 'NVIDIA RTX 3080', category: 'gpu', price: 1200, income: 5, power: 50, tier: 4, icon: '🚀', specs: '10GB GDDR6X' },
            { id: 'gpu_3080ti', name: 'NVIDIA RTX 3080 Ti', category: 'gpu', price: 1600, income: 6.5, power: 65, tier: 4, icon: '💥', specs: '12GB GDDR6X' },
            { id: 'gpu_3090', name: 'NVIDIA RTX 3090 Ti', category: 'gpu', price: 2000, income: 8, power: 75, tier: 5, icon: '💥', specs: '24GB GDDR6X' },
            { id: 'gpu_3090ti', name: 'NVIDIA RTX 3090 Ti', category: 'gpu', price: 2500, income: 10, power: 90, tier: 5, icon: '💢', specs: '24GB GDDR6X' },
            { id: 'gpu_4070', name: 'NVIDIA RTX 4070', category: 'gpu', price: 1800, income: 7, power: 70, tier: 5, icon: '✨', specs: '12GB GDDR6X' },
            { id: 'gpu_4070ti', name: 'NVIDIA RTX 4070 Ti', category: 'gpu', price: 2200, income: 9, power: 85, tier: 5, icon: '💫', specs: '12GB GDDR6X' },
            { id: 'gpu_4080', name: 'NVIDIA RTX 4080', category: 'gpu', price: 2800, income: 12, power: 110, tier: 6, icon: '🚀', specs: '16GB GDDR6X' },
            { id: 'gpu_4090', name: 'NVIDIA RTX 4090', category: 'gpu', price: 3500, income: 16, power: 140, tier: 6, icon: '👑', specs: '24GB GDDR6X' },
            { id: 'gpu_4090ti', name: 'NVIDIA RTX 4090 Ti', category: 'gpu', price: 4500, income: 20, power: 180, tier: 7, icon: '🔥', specs: '48GB GDDR6X' },
            { id: 'gpu_titan', name: 'NVIDIA Titan RTX', category: 'gpu', price: 3000, income: 13, power: 120, tier: 6, icon: '💎', specs: '24GB GDDR6' },
            { id: 'gpu_titan_ada', name: 'NVIDIA Titan Ada', category: 'gpu', price: 5000, income: 22, power: 200, tier: 7, icon: '⚜️', specs: '48GB GDDR6X' },
            { id: 'gpu_rx6600', name: 'AMD RX 6600 XT', category: 'gpu', price: 450, income: 1.8, power: 18, tier: 3, icon: '🔴', specs: '8GB GDDR6' },
            { id: 'gpu_rx6700', name: 'AMD RX 6700 XT', category: 'gpu', price: 650, income: 2.8, power: 28, tier: 3, icon: '❤️', specs: '12GB GDDR6' },
            { id: 'gpu_rx6800', name: 'AMD RX 6800 XT', category: 'gpu', price: 900, income: 4, power: 45, tier: 4, icon: '🔥', specs: '16GB GDDR6' },
            { id: 'gpu_rx6900', name: 'AMD RX 6950 XT', category: 'gpu', price: 1500, income: 6, power: 65, tier: 5, icon: '💢', specs: '16GB GDDR6' },
            { id: 'gpu_rx7900', name: 'AMD RX 7900 XTX', category: 'gpu', price: 2500, income: 11, power: 100, tier: 6, icon: '⭐', specs: '24GB GDDR6' },
            { id: 'gpu_rx7950', name: 'AMD RX 7950 XTX', category: 'gpu', price: 3500, income: 15, power: 140, tier: 6, icon: '🌟', specs: '32GB GDDR6' },
            { id: 'gpu_arc_a770', name: 'Intel Arc A770', category: 'gpu', price: 400, income: 1.6, power: 16, tier: 3, icon: '🔷', specs: '16GB GDDR6' },

            // ОПЕРАТИВНАЯ ПАМЯТЬ (УВЕЛИЧЕНО)
            { id: 'ram_4gb', name: 'DDR3 4GB 1600MHz', category: 'ram', price: 30, income: 0.1, power: 1, tier: 1, icon: '💾', specs: '4GB, CL11' },
            { id: 'ram_8gb', name: 'DDR4 8GB 3200MHz', category: 'ram', price: 50, income: 0.2, power: 2, tier: 1, icon: '🧠', specs: '8GB, CL16' },
            { id: 'ram_16gb', name: 'DDR4 16GB 3600MHz', category: 'ram', price: 90, income: 0.3, power: 4, tier: 2, icon: '🧮', specs: '16GB, CL18' },
            { id: 'ram_32gb', name: 'DDR4 32GB 3600MHz', category: 'ram', price: 180, income: 0.5, power: 8, tier: 3, icon: '💿', specs: '32GB, CL18' },
            { id: 'ram_64gb', name: 'DDR4 64GB 3200MHz', category: 'ram', price: 350, income: 1, power: 15, tier: 4, icon: '📀', specs: '64GB, CL16' },
            { id: 'ram_128gb', name: 'DDR4 128GB 3200MHz', category: 'ram', price: 700, income: 2, power: 28, tier: 5, icon: '💿', specs: '128GB, CL18' },
            { id: 'ram_ddr5_8', name: 'DDR5 8GB 5600MHz', category: 'ram', price: 80, income: 0.25, power: 3, tier: 2, icon: '⚡', specs: '8GB, CL36' },
            { id: 'ram_ddr5_16', name: 'DDR5 16GB 6000MHz', category: 'ram', price: 150, income: 0.4, power: 6, tier: 3, icon: '⚡', specs: '16GB, CL36' },
            { id: 'ram_ddr5_32', name: 'DDR5 32GB 6000MHz', category: 'ram', price: 280, income: 0.8, power: 12, tier: 4, icon: '💨', specs: '32GB, CL36' },
            { id: 'ram_ddr5_64', name: 'DDR5 64GB 6400MHz', category: 'ram', price: 550, income: 1.5, power: 24, tier: 5, icon: '🚀', specs: '64GB, CL32' },
            { id: 'ram_ddr5_96', name: 'DDR5 96GB 6400MHz', category: 'ram', price: 850, income: 2.2, power: 32, tier: 5, icon: '🚀', specs: '96GB, CL32' },
            { id: 'ram_ddr5_128', name: 'DDR5 128GB 6800MHz', category: 'ram', price: 1200, income: 3, power: 40, tier: 6, icon: '💎', specs: '128GB, CL34' },
            { id: 'ram_ddr5_192', name: 'DDR5 192GB 6000MHz', category: 'ram', price: 1800, income: 4.5, power: 55, tier: 6, icon: '💎', specs: '192GB, CL36' },
            { id: 'ram_ddr5_256', name: 'DDR5 256GB 5600MHz', category: 'ram', price: 2500, income: 6, power: 70, tier: 7, icon: '👑', specs: '256GB, CL40' },
            { id: 'ram_ecc', name: 'DDR4 ECC 64GB Server', category: 'ram', price: 800, income: 2.2, power: 28, tier: 5, icon: '🔒', specs: '64GB, ECC' },
            { id: 'ram_ecc_128', name: 'DDR4 ECC 128GB Server', category: 'ram', price: 1500, income: 4, power: 50, tier: 6, icon: '🔒', specs: '128GB, ECC' },

            // НАКОПИТЕЛИ (УВЕЛИЧЕНО)
            { id: 'ssd_120', name: 'SSD 120GB SATA', category: 'storage', price: 30, income: 0.1, power: 1, tier: 1, icon: '💿', specs: '120GB, 450MB/s' },
            { id: 'ssd_240', name: 'SSD 240GB SATA', category: 'storage', price: 45, income: 0.15, power: 1.5, tier: 1, icon: '💿', specs: '240GB, 500MB/s' },
            { id: 'ssd_480', name: 'SSD 480GB SATA', category: 'storage', price: 60, income: 0.2, power: 2, tier: 2, icon: '💿', specs: '480GB, 550MB/s' },
            { id: 'ssd_500', name: 'SSD 500GB NVMe', category: 'storage', price: 80, income: 0.25, power: 3, tier: 2, icon: '⚡', specs: '500GB, 3500MB/s' },
            { id: 'ssd_1tb', name: 'SSD 1TB NVMe', category: 'storage', price: 150, income: 0.4, power: 4, tier: 3, icon: '💨', specs: '1TB, 5000MB/s' },
            { id: 'ssd_2tb', name: 'SSD 2TB NVMe', category: 'storage', price: 280, income: 0.7, power: 7, tier: 4, icon: '🚀', specs: '2TB, 7000MB/s' },
            { id: 'ssd_4tb', name: 'SSD 4TB NVMe', category: 'storage', price: 550, income: 1.2, power: 13, tier: 5, icon: '💫', specs: '4TB, 7000MB/s' },
            { id: 'ssd_8tb', name: 'SSD 8TB NVMe', category: 'storage', price: 1200, income: 2.5, power: 25, tier: 6, icon: '🌟', specs: '8TB, 7000MB/s' },
            { id: 'ssd_16tb', name: 'SSD 16TB NVMe', category: 'storage', price: 2500, income: 5, power: 45, tier: 7, icon: '💫', specs: '16TB, 7000MB/s' },
            { id: 'ssd_30tb', name: 'SSD 30TB NVMe', category: 'storage', price: 4500, income: 9, power: 70, tier: 8, icon: '👑', specs: '30TB, 7000MB/s' },
            { id: 'hdd_500', name: 'HDD 500GB', category: 'storage', price: 25, income: 0.05, power: 0.5, tier: 1, icon: '💽', specs: '500GB, 150MB/s' },
            { id: 'hdd_1tb', name: 'HDD 1TB 7200rpm', category: 'storage', price: 50, income: 0.1, power: 1, tier: 1, icon: '💽', specs: '1TB, 160MB/s' },
            { id: 'hdd_2tb', name: 'HDD 2TB 7200rpm', category: 'storage', price: 80, income: 0.15, power: 1.5, tier: 2, icon: '💽', specs: '2TB, 180MB/s' },
            { id: 'hdd_4tb', name: 'HDD 4TB 7200rpm', category: 'storage', price: 120, income: 0.2, power: 2, tier: 2, icon: '📀', specs: '4TB, 180MB/s' },
            { id: 'hdd_8tb', name: 'HDD 8TB 7200rpm', category: 'storage', price: 200, income: 0.3, power: 3, tier: 3, icon: '📀', specs: '8TB, 200MB/s' },
            { id: 'hdd_10tb', name: 'HDD 10TB 7200rpm', category: 'storage', price: 300, income: 0.4, power: 4, tier: 3, icon: '💿', specs: '10TB, 210MB/s' },
            { id: 'hdd_12tb', name: 'HDD 12TB 7200rpm', category: 'storage', price: 380, income: 0.5, power: 5, tier: 4, icon: '💿', specs: '12TB, 220MB/s' },
            { id: 'hdd_14tb', name: 'HDD 14TB 7200rpm', category: 'storage', price: 450, income: 0.6, power: 6, tier: 4, icon: '💿', specs: '14TB, 220MB/s' },
            { id: 'hdd_16tb', name: 'HDD 16TB 7200rpm', category: 'storage', price: 550, income: 0.7, power: 7, tier: 5, icon: '📀', specs: '16TB, 230MB/s' },
            { id: 'hdd_18tb', name: 'HDD 18TB 7200rpm', category: 'storage', price: 650, income: 0.8, power: 8, tier: 5, icon: '📀', specs: '18TB, 240MB/s' },
            { id: 'hdd_20tb', name: 'HDD 20TB 7200rpm', category: 'storage', price: 800, income: 1, power: 10, tier: 6, icon: '📀', specs: '20TB, 250MB/s' },
            { id: 'hdd_22tb', name: 'HDD 22TB 7200rpm', category: 'storage', price: 1000, income: 1.3, power: 12, tier: 6, icon: '📀', specs: '22TB, 260MB/s' },
            { id: 'hdd_24tb', name: 'HDD 24TB 7200rpm', category: 'storage', price: 1300, income: 1.6, power: 15, tier: 7, icon: '📀', specs: '24TB, 270MB/s' },

            // ОХЛАЖДЕНИЕ (СИЛЬНО УВЕЛИЧЕНО)
            { id: 'cooler_air', name: 'Воздушный кулер', category: 'cooling', price: 50, income: 0.1, power: 2, tier: 1, icon: '🌀', specs: 'TDP 150W' },
            { id: 'cooler_air_pro', name: 'Башенный кулер', category: 'cooling', price: 120, income: 0.2, power: 4, tier: 2, icon: '💨', specs: 'TDP 220W' },
            { id: 'cooler_air_dual', name: 'Двухбашенный кулер', category: 'cooling', price: 200, income: 0.3, power: 6, tier: 3, icon: '🌪️', specs: 'TDP 280W' },
            { id: 'cooler_air_mega', name: 'Мега кулер', category: 'cooling', price: 300, income: 0.5, power: 9, tier: 4, icon: '🌀', specs: 'TDP 350W' },
            { id: 'aio_120', name: 'Жидкостное охлаждение 120мм', category: 'cooling', price: 150, income: 0.3, power: 5, tier: 3, icon: '💧', specs: 'TDP 200W' },
            { id: 'aio_140', name: 'Жидкостное охлаждение 140мм', category: 'cooling', price: 200, income: 0.4, power: 6, tier: 3, icon: '💧', specs: 'TDP 220W' },
            { id: 'aio_240', name: 'Жидкостное охлаждение 240мм', category: 'cooling', price: 250, income: 0.5, power: 8, tier: 4, icon: '🌊', specs: 'TDP 280W' },
            { id: 'aio_280', name: 'Жидкостное охлаждение 280мм', category: 'cooling', price: 320, income: 0.7, power: 10, tier: 4, icon: '💧', specs: 'TDP 320W' },
            { id: 'aio_360', name: 'Жидкостное охлаждение 360мм', category: 'cooling', price: 400, income: 0.9, power: 12, tier: 5, icon: '💦', specs: 'TDP 350W' },
            { id: 'aio_420', name: 'Жидкостное охлаждение 420мм', category: 'cooling', price: 550, income: 1.2, power: 16, tier: 6, icon: '🌊', specs: 'TDP 400W' },
            { id: 'aio_480', name: 'Жидкостное охлаждение 480мм', category: 'cooling', price: 800, income: 1.8, power: 22, tier: 6, icon: '💦', specs: 'TDP 500W' },
            { id: 'aio_560', name: 'Жидкостное охлаждение 560мм', category: 'cooling', price: 1200, income: 2.5, power: 30, tier: 7, icon: '🌊', specs: 'TDP 600W' },
            { id: 'custom_loop', name: 'Кастомная СЖО', category: 'cooling', price: 800, income: 1.8, power: 20, tier: 6, icon: '🔮', specs: 'TDP 500W+' },
            { id: 'custom_loop_pro', name: 'Кастомная СЖО Pro', category: 'cooling', price: 1500, income: 3.2, power: 35, tier: 7, icon: '🔮', specs: 'TDP 800W+' },
            { id: 'phase_change', name: 'Фазовый переход', category: 'cooling', price: 2000, income: 4, power: 45, tier: 8, icon: '❄️', specs: 'TDP 1000W' },
            { id: 'liquid_nitrogen', name: 'Жидкий азот', category: 'cooling', price: 3000, income: 6, power: 60, tier: 9, icon: '🧊', specs: '-196°C' },
            { id: 'thermoelectric', name: 'Термоэлектрическое', category: 'cooling', price: 1200, income: 2.8, power: 28, tier: 7, icon: '❄️', specs: 'TEC, 300W' },
            { id: 'immersion', name: 'Иммерсионное охлаждение', category: 'cooling', price: 5000, income: 10, power: 100, tier: 10, icon: '💧', specs: 'Масляная ванна' },

            // БЛОКИ ПИТАНИЯ (СИЛЬНО УВЕЛИЧЕНО)
            { id: 'psu_400w', name: 'БП 400W Bronze', category: 'psu', price: 60, income: 0.1, power: 2, tier: 1, icon: '🔌', specs: '400W, 80+ Bronze' },
            { id: 'psu_450w', name: 'БП 450W Bronze', category: 'psu', price: 70, income: 0.12, power: 2.5, tier: 1, icon: '🔌', specs: '450W, 80+ Bronze' },
            { id: 'psu_500w', name: 'БП 500W Bronze', category: 'psu', price: 80, income: 0.15, power: 3, tier: 1, icon: '🔌', specs: '500W, 80+ Bronze' },
            { id: 'psu_550w', name: 'БП 550W Gold', category: 'psu', price: 120, income: 0.2, power: 4, tier: 2, icon: '⚡', specs: '550W, 80+ Gold' },
            { id: 'psu_600w', name: 'БП 600W Gold', category: 'psu', price: 140, income: 0.25, power: 4.5, tier: 2, icon: '⚡', specs: '600W, 80+ Gold' },
            { id: 'psu_650w', name: 'БП 650W Gold', category: 'psu', price: 160, income: 0.3, power: 5, tier: 2, icon: '⚡', specs: '650W, 80+ Gold' },
            { id: 'psu_700w', name: 'БП 700W Gold', category: 'psu', price: 180, income: 0.35, power: 6, tier: 3, icon: '⚡', specs: '700W, 80+ Gold' },
            { id: 'psu_750w', name: 'БП 750W Gold', category: 'psu', price: 200, income: 0.4, power: 7, tier: 3, icon: '💡', specs: '750W, 80+ Gold' },
            { id: 'psu_800w', name: 'БП 800W Gold', category: 'psu', price: 240, income: 0.5, power: 8, tier: 3, icon: '💡', specs: '800W, 80+ Gold' },
            { id: 'psu_850w', name: 'БП 850W Platinum', category: 'psu', price: 300, income: 0.6, power: 10, tier: 4, icon: '🔋', specs: '850W, 80+ Platinum' },
            { id: 'psu_900w', name: 'БП 900W Platinum', category: 'psu', price: 350, income: 0.7, power: 12, tier: 4, icon: '🔋', specs: '900W, 80+ Platinum' },
            { id: 'psu_1000w', name: 'БП 1000W Platinum', category: 'psu', price: 450, income: 1, power: 14, tier: 5, icon: '⚡', specs: '1000W, 80+ Platinum' },
            { id: 'psu_1100w', name: 'БП 1100W Platinum', category: 'psu', price: 550, income: 1.2, power: 16, tier: 5, icon: '⚡', specs: '1100W, 80+ Platinum' },
            { id: 'psu_1200w', name: 'БП 1200W Titanium', category: 'psu', price: 700, income: 1.6, power: 20, tier: 6, icon: '💎', specs: '1200W, 80+ Titanium' },
            { id: 'psu_1300w', name: 'БП 1300W Titanium', category: 'psu', price: 850, income: 2, power: 23, tier: 6, icon: '💎', specs: '1300W, 80+ Titanium' },
            { id: 'psu_1500w', name: 'БП 1500W Titanium', category: 'psu', price: 1000, income: 2.2, power: 26, tier: 6, icon: '💎', specs: '1500W, 80+ Titanium' },
            { id: 'psu_1600w', name: 'БП 1600W Titanium', category: 'psu', price: 1300, income: 2.8, power: 32, tier: 7, icon: '👑', specs: '1600W, 80+ Titanium' },
            { id: 'psu_1800w', name: 'БП 1800W Titanium', category: 'psu', price: 1600, income: 3.5, power: 38, tier: 7, icon: '👑', specs: '1800W, 80+ Titanium' },
            { id: 'psu_2000w', name: 'БП 2000W Server', category: 'psu', price: 2000, income: 4.5, power: 45, tier: 8, icon: '🏭', specs: '2000W, Redundant' },
            { id: 'psu_2200w', name: 'БП 2200W Server', category: 'psu', price: 2500, income: 5.5, power: 52, tier: 8, icon: '🏭', specs: '2200W, Redundant' },
            { id: 'psu_2400w', name: 'БП 2400W Server', category: 'psu', price: 3200, income: 7, power: 65, tier: 9, icon: '⚙️', specs: '2400W, Redundant' },
            { id: 'psu_3000w', name: 'БП 3000W Server', category: 'psu', price: 4500, income: 10, power: 85, tier: 10, icon: '⚙️', specs: '3000W, Redundant' }
        ];

        // Система компьютеров
        let computers = [
            {
                id: 0,
                name: 'ПК #1',
                components: new Array(componentsData.length).fill(false),
                totalIncome: 0,
                totalPower: 0,
                position: 0
            }
        ];
        
        let currentComputer = 0;
        let balance = 200;
        let totalIncome = 0;
        let totalPower = 0;
        let miningProgress = 0;
        let currentCategory = 'all';

        // Система заказов
        const clients = [
            { name: 'Алексей', avatar: '👨‍💼', message: 'Нужен мощный ПК для работы' },
            { name: 'Елена', avatar: '👩‍💻', message: 'Для видеомонтажа и игр' },
            { name: 'Дмитрий', avatar: '👨‍🎮', message: 'Хочу играть в новые игры' },
            { name: 'Ольга', avatar: '👩‍🎨', message: 'Для графического дизайна' },
            { name: 'Михаил', avatar: '👨‍🔬', message: 'Для научных расчетов' },
            { name: 'Анна', avatar: '👩‍🏫', message: 'Для учебы и программирования' },
            { name: 'Сергей', avatar: '👨‍🚀', message: 'Для 3D моделирования' },
            { name: 'Мария', avatar: '👩‍🎤', message: 'Для музыки и звукозаписи' }
        ];

        let currentOrder = {
            client: clients[0],
            requirements: {
                cpu: 'Intel Core i7',
                gpu: 'RTX 3060',
                ram: 16,
                storage: 500
            },
            reward: 500,
            completed: false
        };

        let clientPanelHidden = false;
        let clientPanelClosed = false;

        // Boost система
        let boostActive = false;
        let boostEndTime = 0;
        let boostCooldown = false;
        let boostCooldownEnd = 0;
        const BOOST_DURATION = 10;
        const BOOST_COOLDOWN = 300;

        // Состояние магазина
        let shopHidden = false;

        // Пытаемся загрузить сохранение
        loadGame();

        // ============ 3D СЦЕНА ============
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x0a0a1a);
        scene.fog = new THREE.Fog(0x0a0a1a, 20, 60);

        const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.set(8, 5, 12);

        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.shadowMap.enabled = true;
        renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        renderer.setPixelRatio(window.devicePixelRatio);
        document.body.appendChild(renderer.domElement);

        const controls = new OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;
        controls.autoRotate = true;
        controls.autoRotateSpeed = 0.5;
        controls.enableZoom = true;
        controls.maxPolarAngle = Math.PI / 2;
        controls.minDistance = 5;
        controls.maxDistance = 25;
        controls.target.set(0, 2, 0);

        window.addEventListener('keydown', (e) => {
            if (e.key === 'r' || e.key === 'R') {
                camera.position.set(8, 5, 12);
                controls.target.set(0, 2, 0);
            }
        });

        // Освещение
        const ambientLight = new THREE.AmbientLight(0x404060);
        scene.add(ambientLight);

        const dirLight = new THREE.DirectionalLight(0xffffff, 1.2);
        dirLight.position.set(5, 10, 7);
        dirLight.castShadow = true;
        dirLight.receiveShadow = true;
        dirLight.shadow.mapSize.width = 2048;
        dirLight.shadow.mapSize.height = 2048;
        scene.add(dirLight);

        // Пол
        const floorGeometry = new THREE.CircleGeometry(30, 32);
        const floorMaterial = new THREE.MeshStandardMaterial({ color: 0x1a1a2e, roughness: 0.3 });
        const floor = new THREE.Mesh(floorGeometry, floorMaterial);
        floor.rotation.x = -Math.PI / 2;
        floor.position.y = -0.1;
        floor.receiveShadow = true;
        scene.add(floor);

        const gridHelper = new THREE.GridHelper(30, 30, 0x00ffaa, 0x3366ff);
        gridHelper.position.y = 0;
        scene.add(gridHelper);

        // ============ СОЗДАНИЕ ТАНЦУЮЩИХ 3D ЧЕЛОВЕЧКОВ ============
        function createHuman(gender = 'male', accessories = {}) {
            const group = new THREE.Group();
            
            // Цвета для разных персонажей
            const colors = {
                male: { body: 0x3366ff, legs: 0x2244aa },
                female: { body: 0xff66aa, legs: 0xcc4488 }
            };
            
            const colorScheme = colors[gender] || colors.male;
            
            // Тело
            const bodyGeo = new THREE.CylinderGeometry(0.4, 0.4, 1.2, 8);
            const bodyMat = new THREE.MeshStandardMaterial({ color: colorScheme.body, emissive: new THREE.Color(0x112244) });
            const body = new THREE.Mesh(bodyGeo, bodyMat);
            body.position.y = 0.6;
            body.castShadow = true;
            body.receiveShadow = true;
            group.add(body);
            
            // Голова
            const headGeo = new THREE.SphereGeometry(0.25, 16);
            const headMat = new THREE.MeshStandardMaterial({ color: 0xffccaa });
            const head = new THREE.Mesh(headGeo, headMat);
            head.position.y = 1.4;
            head.castShadow = true;
            head.receiveShadow = true;
            group.add(head);
            
            // Глаза
            const eyeGeo = new THREE.SphereGeometry(0.05, 8);
            const eyeMat = new THREE.MeshStandardMaterial({ color: 0x000000 });
            
            const eyeL = new THREE.Mesh(eyeGeo, eyeMat);
            eyeL.position.set(-0.1, 1.45, 0.2);
            group.add(eyeL);
            
            const eyeR = new THREE.Mesh(eyeGeo, eyeMat);
            eyeR.position.set(0.1, 1.45, 0.2);
            group.add(eyeR);
            
            // Усы (если есть)
            if (accessories.mustache) {
                const mustacheGeo = new THREE.CylinderGeometry(0.08, 0.08, 0.2, 6);
                const mustacheMat = new THREE.MeshStandardMaterial({ color: 0x8b4513 });
                const mustache = new THREE.Mesh(mustacheGeo, mustacheMat);
                mustache.rotation.z = 0.2;
                mustache.rotation.x = 0.3;
                mustache.position.set(0, 1.3, 0.25);
                group.add(mustache);
                
                const mustache2 = new THREE.Mesh(mustacheGeo, mustacheMat);
                mustache2.rotation.z = -0.2;
                mustache2.rotation.x = 0.3;
                mustache2.position.set(0, 1.3, 0.25);
                group.add(mustache2);
            }
            
            // Борода
            if (accessories.beard) {
                const beardGeo = new THREE.ConeGeometry(0.15, 0.2, 6);
                const beardMat = new THREE.MeshStandardMaterial({ color: 0x8b4513 });
                const beard = new THREE.Mesh(beardGeo, beardMat);
                beard.position.set(0, 1.25, 0.2);
                beard.rotation.x = 0.1;
                group.add(beard);
            }
            
            // Очки
            if (accessories.glasses) {
                const glassGeo = new THREE.TorusGeometry(0.1, 0.02, 8, 16, Math.PI);
                const glassMat = new THREE.MeshStandardMaterial({ color: 0x888888 });
                
                const glassL = new THREE.Mesh(glassGeo, glassMat);
                glassL.position.set(-0.15, 1.45, 0.25);
                glassL.rotation.y = 0.2;
                group.add(glassL);
                
                const glassR = new THREE.Mesh(glassGeo, glassMat);
                glassR.position.set(0.15, 1.45, 0.25);
                glassR.rotation.y = -0.2;
                group.add(glassR);
                
                const bridgeGeo = new THREE.BoxGeometry(0.1, 0.02, 0.02);
                const bridgeMat = new THREE.MeshStandardMaterial({ color: 0x888888 });
                const bridge = new THREE.Mesh(bridgeGeo, bridgeMat);
                bridge.position.set(0, 1.45, 0.3);
                group.add(bridge);
            }
            
            // Шляпа
            if (accessories.hat) {
                const hatBaseGeo = new THREE.CylinderGeometry(0.25, 0.3, 0.1, 8);
                const hatBaseMat = new THREE.MeshStandardMaterial({ color: 0x442200 });
                const hatBase = new THREE.Mesh(hatBaseGeo, hatBaseMat);
                hatBase.position.set(0, 1.6, 0);
                group.add(hatBase);
                
                const hatTopGeo = new THREE.ConeGeometry(0.15, 0.2, 8);
                const hatTopMat = new THREE.MeshStandardMaterial({ color: 0x442200 });
                const hatTop = new THREE.Mesh(hatTopGeo, hatTopMat);
                hatTop.position.set(0, 1.75, 0);
                group.add(hatTop);
            }
            
            // Волосы для девочек
            if (gender === 'female') {
                const hairGeo = new THREE.SphereGeometry(0.15, 8);
                const hairMat = new THREE.MeshStandardMaterial({ color: 0x8b4513 });
                const hair = new THREE.Mesh(hairGeo, hairMat);
                hair.position.set(0, 1.5, 0.1);
                hair.scale.set(1.2, 0.5, 1);
                group.add(hair);
            }
            
            // Руки
            const armGeo = new THREE.CylinderGeometry(0.1, 0.1, 0.8, 6);
            const armMat = new THREE.MeshStandardMaterial({ color: colorScheme.body });
            
            const armL = new THREE.Mesh(armGeo, armMat);
            armL.position.set(-0.5, 1.0, 0);
            armL.castShadow = true;
            group.add(armL);
            
            const armR = new THREE.Mesh(armGeo, armMat);
            armR.position.set(0.5, 1.0, 0);
            armR.castShadow = true;
            group.add(armR);
            
            // Ноги
            const legGeo = new THREE.CylinderGeometry(0.15, 0.15, 0.8, 6);
            const legMat = new THREE.MeshStandardMaterial({ color: colorScheme.legs });
            
            const legL = new THREE.Mesh(legGeo, legMat);
            legL.position.set(-0.2, 0.0, 0);
            legL.castShadow = true;
            group.add(legL);
            
            const legR = new THREE.Mesh(legGeo, legMat);
            legR.position.set(0.2, 0.0, 0);
            legR.castShadow = true;
            group.add(legR);
            
            return group;
        }

        // Создаем разных персонажей
        const humans = [];
        
        // Мужчина с усами
        const human1 = createHuman('male', { mustache: true, glasses: false, hat: false });
        human1.position.set(3, 0, 3);
        human1.scale.set(0.8, 0.8, 0.8);
        scene.add(human1);
        humans.push(human1);
        
        // Девушка
        const human2 = createHuman('female', { mustache: false, glasses: true, hat: false });
        human2.position.set(-3, 0, 3);
        human2.scale.set(0.8, 0.8, 0.8);
        scene.add(human2);
        humans.push(human2);
        
        // Мужчина в шляпе с очками
        const human3 = createHuman('male', { mustache: true, glasses: true, hat: true });
        human3.position.set(0, 0, -3);
        human3.scale.set(0.8, 0.8, 0.8);
        scene.add(human3);
        humans.push(human3);
        
        // Девушка с очками
        const human4 = createHuman('female', { mustache: false, glasses: true, hat: false });
        human4.position.set(4, 0, -2);
        human4.scale.set(0.8, 0.8, 0.8);
        scene.add(human4);
        humans.push(human4);
        
        // Мужчина с бородой
        const human5 = createHuman('male', { mustache: false, beard: true, glasses: false, hat: false });
        human5.position.set(-4, 0, -2);
        human5.scale.set(0.8, 0.8, 0.8);
        scene.add(human5);
        humans.push(human5);

        // ============ ФУНКЦИЯ СОЗДАНИЯ КОРПУСА ============
        function createComputerCase(posX, posZ, index) {
            const group = new THREE.Group();
            
            const caseMaterial = new THREE.MeshStandardMaterial({ color: 0x222233, roughness: 0.4, metalness: 0.6 });
            const glassMaterial = new THREE.MeshStandardMaterial({ color: 0x88aaff, transparent: true, opacity: 0.25 });
            
            const w = 3, h = 4, d = 3;
            
            // Стекло спереди
            const glass = new THREE.Mesh(new THREE.BoxGeometry(w - 0.4, h - 0.4, 0.2), glassMaterial);
            glass.position.set(0, h/2, d/2 - 0.1);
            glass.castShadow = true;
            glass.receiveShadow = true;
            group.add(glass);
            
            // Корпус
            const back = new THREE.Mesh(new THREE.BoxGeometry(w, h, 0.3), caseMaterial);
            back.position.set(0, h/2, -d/2);
            back.castShadow = true;
            back.receiveShadow = true;
            group.add(back);
            
            const left = new THREE.Mesh(new THREE.BoxGeometry(0.3, h, d), caseMaterial);
            left.position.set(-w/2, h/2, 0);
            left.castShadow = true;
            left.receiveShadow = true;
            group.add(left);
            
            const right = new THREE.Mesh(new THREE.BoxGeometry(0.3, h, d), caseMaterial);
            right.position.set(w/2, h/2, 0);
            right.castShadow = true;
            right.receiveShadow = true;
            group.add(right);
            
            const top = new THREE.Mesh(new THREE.BoxGeometry(w, 0.3, d), caseMaterial);
            top.position.set(0, h, 0);
            top.castShadow = true;
            top.receiveShadow = true;
            group.add(top);
            
            const bottom = new THREE.Mesh(new THREE.BoxGeometry(w, 0.3, d), caseMaterial);
            bottom.position.set(0, 0, 0);
            bottom.castShadow = true;
            bottom.receiveShadow = true;
            group.add(bottom);
            
            // RGB подсветка
            const rgb = new THREE.Mesh(new THREE.BoxGeometry(w - 0.5, 0.1, d - 0.5), 
                new THREE.MeshStandardMaterial({ color: 0xffffff, emissive: new THREE.Color(0x00ffaa) }));
            rgb.position.set(0, 0.2, 0);
            rgb.castShadow = true;
            group.add(rgb);
            
            // Номер компьютера
            const canvas = document.createElement('canvas');
            canvas.width = 128;
            canvas.height = 128;
            const ctx = canvas.getContext('2d');
            ctx.fillStyle = '#00ffaa';
            ctx.font = 'bold 60px Arial';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText('#' + (index + 1), 64, 64);
            
            const texture = new THREE.CanvasTexture(canvas);
            const numberMat = new THREE.SpriteMaterial({ map: texture });
            const numberSprite = new THREE.Sprite(numberMat);
            numberSprite.scale.set(0.5, 0.5, 0.5);
            numberSprite.position.set(0, 3.5, 1.5);
            group.add(numberSprite);
            
            group.position.set(posX, 0, posZ);
            return group;
        }

        // Создаем первый компьютер в центре
        const computerModels = [];
        const firstComputer = createComputerCase(0, 0, 0);
        scene.add(firstComputer);
        computerModels.push(firstComputer);

        // Позиции для будущих компьютеров
        const computerPositions = [
            [0, 0],      // центр
            [5, 0],      // справа
            [-5, 0],     // слева
            [0, 5],      // сверху
            [0, -5],     // снизу
            [5, 5]       // диагональ
        ];

        // Создаем компьютеры из загрузки
        for (let i = 1; i < computers.length; i++) {
            if (i < computerPositions.length) {
                const pos = computerPositions[i];
                const computer = createComputerCase(pos[0], pos[1], i);
                scene.add(computer);
                computerModels.push(computer);
            }
        }

        // Частицы
        const particleCount = 500;
        const particleGeo = new THREE.BufferGeometry();
        const particlePositions = new Float32Array(particleCount * 3);
        const particleColors = new Float32Array(particleCount * 3);
        
        for (let i = 0; i < particleCount; i++) {
            const r = 12 + Math.random() * 12;
            const theta = Math.random() * Math.PI * 2;
            const phi = Math.random() * Math.PI - Math.PI/2;
            
            particlePositions[i*3] = Math.cos(theta) * Math.cos(phi) * r;
            particlePositions[i*3+1] = Math.sin(phi) * r + 3;
            particlePositions[i*3+2] = Math.sin(theta) * Math.cos(phi) * r;
            
            const color = new THREE.Color().setHSL(Math.random(), 0.8, 0.6);
            particleColors[i*3] = color.r;
            particleColors[i*3+1] = color.g;
            particleColors[i*3+2] = color.b;
        }
        
        particleGeo.setAttribute('position', new THREE.BufferAttribute(particlePositions, 3));
        particleGeo.setAttribute('color', new THREE.BufferAttribute(particleColors, 3));
        
        const particleMat = new THREE.PointsMaterial({ 
            size: 0.15,
            vertexColors: true,
            transparent: true,
            opacity: 0.6,
            blending: THREE.AdditiveBlending
        });
        
        const particles = new THREE.Points(particleGeo, particleMat);
        scene.add(particles);

        // ============ ИГРОВАЯ ЛОГИКА ============
        
        function checkOrderCompletion() {
            const currentComp = computers[currentComputer];
            if (!currentComp) return false;
            
            // Проверяем наличие требуемых компонентов
            let hasCPU = false;
            let hasGPU = false;
            let hasRAM = false;
            let hasStorage = false;
            
            // Проверяем процессор
            if (currentOrder.requirements.cpu.includes('i7')) {
                hasCPU = currentComp.components[componentsData.findIndex(c => c.id === 'cpu_i7')] ||
                         currentComp.components[componentsData.findIndex(c => c.id === 'cpu_i9')] ||
                         currentComp.components[componentsData.findIndex(c => c.id === 'cpu_i9_14900k')];
            } else if (currentOrder.requirements.cpu.includes('i5')) {
                hasCPU = currentComp.components[componentsData.findIndex(c => c.id === 'cpu_i5')] ||
                         currentComp.components[componentsData.findIndex(c => c.id === 'cpu_i7')] ||
                         currentComp.components[componentsData.findIndex(c => c.id === 'cpu_i9')] ||
                         currentComp.components[componentsData.findIndex(c => c.id === 'cpu_i9_14900k')];
            }
            
            // Проверяем видеокарту
            if (currentOrder.requirements.gpu.includes('3060')) {
                hasGPU = currentComp.components[componentsData.findIndex(c => c.id === 'gpu_3060')] ||
                         currentComp.components[componentsData.findIndex(c => c.id === 'gpu_3070')] ||
                         currentComp.components[componentsData.findIndex(c => c.id === 'gpu_3080')] ||
                         currentComp.components[componentsData.findIndex(c => c.id === 'gpu_3090')] ||
                         currentComp.components[componentsData.findIndex(c => c.id === 'gpu_4090')];
            }
            
            // Проверяем RAM
            const ramIndices = [
                componentsData.findIndex(c => c.id === 'ram_4gb'),
                componentsData.findIndex(c => c.id === 'ram_8gb'),
                componentsData.findIndex(c => c.id === 'ram_16gb'),
                componentsData.findIndex(c => c.id === 'ram_32gb'),
                componentsData.findIndex(c => c.id === 'ram_64gb'),
                componentsData.findIndex(c => c.id === 'ram_128gb'),
                componentsData.findIndex(c => c.id === 'ram_ddr5_8'),
                componentsData.findIndex(c => c.id === 'ram_ddr5_16'),
                componentsData.findIndex(c => c.id === 'ram_ddr5_32'),
                componentsData.findIndex(c => c.id === 'ram_ddr5_64'),
                componentsData.findIndex(c => c.id === 'ram_ddr5_96'),
                componentsData.findIndex(c => c.id === 'ram_ddr5_128'),
                componentsData.findIndex(c => c.id === 'ram_ddr5_192'),
                componentsData.findIndex(c => c.id === 'ram_ddr5_256')
            ];
            
            let totalRAM = 0;
            ramIndices.forEach(idx => {
                if (currentComp.components[idx]) {
                    if (idx === componentsData.findIndex(c => c.id === 'ram_4gb')) totalRAM += 4;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_8gb')) totalRAM += 8;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_16gb')) totalRAM += 16;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_32gb')) totalRAM += 32;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_64gb')) totalRAM += 64;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_128gb')) totalRAM += 128;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_ddr5_8')) totalRAM += 8;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_ddr5_16')) totalRAM += 16;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_ddr5_32')) totalRAM += 32;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_ddr5_64')) totalRAM += 64;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_ddr5_96')) totalRAM += 96;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_ddr5_128')) totalRAM += 128;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_ddr5_192')) totalRAM += 192;
                    if (idx === componentsData.findIndex(c => c.id === 'ram_ddr5_256')) totalRAM += 256;
                }
            });
            hasRAM = totalRAM >= currentOrder.requirements.ram;
            
            // Проверяем накопители
            const storageIndices = [
                componentsData.findIndex(c => c.id === 'ssd_120'),
                componentsData.findIndex(c => c.id === 'ssd_240'),
                componentsData.findIndex(c => c.id === 'ssd_480'),
                componentsData.findIndex(c => c.id === 'ssd_500'),
                componentsData.findIndex(c => c.id === 'ssd_1tb'),
                componentsData.findIndex(c => c.id === 'ssd_2tb'),
                componentsData.findIndex(c => c.id === 'ssd_4tb'),
                componentsData.findIndex(c => c.id === 'ssd_8tb'),
                componentsData.findIndex(c => c.id === 'ssd_16tb'),
                componentsData.findIndex(c => c.id === 'ssd_30tb'),
                componentsData.findIndex(c => c.id === 'hdd_500'),
                componentsData.findIndex(c => c.id === 'hdd_1tb'),
                componentsData.findIndex(c => c.id === 'hdd_2tb'),
                componentsData.findIndex(c => c.id === 'hdd_4tb'),
                componentsData.findIndex(c => c.id === 'hdd_8tb'),
                componentsData.findIndex(c => c.id === 'hdd_10tb'),
                componentsData.findIndex(c => c.id === 'hdd_12tb'),
                componentsData.findIndex(c => c.id === 'hdd_14tb'),
                componentsData.findIndex(c => c.id === 'hdd_16tb'),
                componentsData.findIndex(c => c.id === 'hdd_18tb'),
                componentsData.findIndex(c => c.id === 'hdd_20tb'),
                componentsData.findIndex(c => c.id === 'hdd_22tb'),
                componentsData.findIndex(c => c.id === 'hdd_24tb')
            ];
            
            let totalStorage = 0;
            storageIndices.forEach(idx => {
                if (currentComp.components[idx]) {
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_120')) totalStorage += 120;
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_240')) totalStorage += 240;
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_480')) totalStorage += 480;
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_500')) totalStorage += 500;
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_1tb')) totalStorage += 1000;
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_2tb')) totalStorage += 2000;
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_4tb')) totalStorage += 4000;
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_8tb')) totalStorage += 8000;
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_16tb')) totalStorage += 16000;
                    if (idx === componentsData.findIndex(c => c.id === 'ssd_30tb')) totalStorage += 30000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_500')) totalStorage += 500;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_1tb')) totalStorage += 1000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_2tb')) totalStorage += 2000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_4tb')) totalStorage += 4000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_8tb')) totalStorage += 8000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_10tb')) totalStorage += 10000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_12tb')) totalStorage += 12000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_14tb')) totalStorage += 14000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_16tb')) totalStorage += 16000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_18tb')) totalStorage += 18000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_20tb')) totalStorage += 20000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_22tb')) totalStorage += 22000;
                    if (idx === componentsData.findIndex(c => c.id === 'hdd_24tb')) totalStorage += 24000;
                }
            });
            hasStorage = totalStorage >= currentOrder.requirements.storage;
            
            // Обновляем статусы в интерфейсе
            document.getElementById('status-cpu').textContent = hasCPU ? '✅' : '❌';
            document.getElementById('status-gpu').textContent = hasGPU ? '✅' : '❌';
            document.getElementById('status-ram').textContent = hasRAM ? '✅' : '❌';
            document.getElementById('status-storage').textContent = hasStorage ? '✅' : '❌';
            
            const canSell = hasCPU && hasGPU && hasRAM && hasStorage;
            document.getElementById('sell-button').disabled = !canSell;
            
            return canSell;
        }

        function updateUI() {
            document.getElementById('balance').textContent = Math.floor(balance);
            
            const displayIncome = boostActive ? totalIncome * 2 : totalIncome;
            document.getElementById('income').textContent = displayIncome.toFixed(1);
            document.getElementById('mining-power').textContent = `Мощность фермы: ${totalPower} GH/s ${boostActive ? '(x2)' : ''}`;
            
            document.getElementById('computers-count').textContent = computers.length;
            document.getElementById('total-parts').textContent = computers.reduce((sum, comp) => 
                sum + comp.components.filter(c => c).length, 0);
            
            // Обновление вертикальной сетки компьютеров
            let computersHTML = computers.map((comp, index) => `
                <div class="computer-slot ${index === currentComputer ? 'active' : ''}" 
                     onclick="switchComputer(${index})">
                    <div class="name">${comp.name}</div>
                    <div class="power">⚡ ${comp.totalPower} GH/s</div>
                    <div class="count">${comp.components.filter(c => c).length} комп.</div>
                </div>
            `).join('');
            
            // Добавляем пустые слоты вертикально
            const emptySlots = 6 - computers.length;
            for (let i = 0; i < emptySlots; i++) {
                computersHTML += `
                    <div class="computer-slot empty">
                        <div class="name">🔒 Пусто</div>
                        <div class="count">Купите ПК</div>
                    </div>
                `;
            }
            
            document.getElementById('computers-grid').innerHTML = computersHTML;
            
            // Кнопка нового компьютера
            const newBtn = document.getElementById('new-computer-btn');
            if (computers.length >= 6) {
                newBtn.disabled = true;
                newBtn.textContent = '➕ Максимум ПК (6/6)';
            } else {
                newBtn.disabled = balance < 2000;
                newBtn.textContent = `➕ Купить ПК (2000💰) ${computers.length}/6`;
            }
            
            // Обновление магазина
            const filtered = currentCategory === 'all' 
                ? componentsData 
                : componentsData.filter(c => c.category === currentCategory);
            
            const currentComp = computers[currentComputer];
            const shopHTML = filtered.map((comp) => {
                const index = componentsData.findIndex(c => c.id === comp.id);
                const owned = currentComp.components[index];
                const canBuy = balance >= comp.price && !owned;
                
                return `
                    <div class="shop-item ${owned ? 'owned' : ''}" onclick="buyComponent('${comp.id}')" style="${!canBuy && !owned ? 'opacity:0.5;' : ''}">
                        <div class="item-icon">${comp.icon}</div>
                        <div class="item-info">
                            <div class="item-name">${comp.name}</div>
                            <div class="item-price">💰 ${comp.price} монет</div>
                            <div class="item-income">⛏ +${comp.income}/сек</div>
                            <div class="item-specs">${comp.specs}</div>
                        </div>
                    </div>
                `;
            }).join('');
            
            document.getElementById('shop-items').innerHTML = shopHTML;
            
            // Проверяем выполнение заказа, если панель открыта
            if (!clientPanelClosed) {
                checkOrderCompletion();
            }
        }

        window.buyComponent = (id) => {
            const index = componentsData.findIndex(c => c.id === id);
            if (index === -1) return;
            
            const comp = componentsData[index];
            const currentComp = computers[currentComputer];
            
            if (!currentComp.components[index] && balance >= comp.price) {
                balance -= comp.price;
                currentComp.components[index] = true;
                currentComp.totalIncome += comp.income;
                currentComp.totalPower += comp.power;
                
                // Обновляем общую статистику
                totalIncome = computers.reduce((sum, c) => sum + c.totalIncome, 0);
                totalPower = computers.reduce((sum, c) => sum + c.totalPower, 0);
                
                // Визуальный эффект
                if (computerModels[currentComputer]) {
                    computerModels[currentComputer].children.forEach(child => {
                        if (child.material && child.material.emissive) {
                            child.material.emissive.setHex(0x224400);
                            setTimeout(() => {
                                child.material.emissive.setHex(0x000000);
                            }, 500);
                        }
                    });
                }
                
                showNotification(`Куплено: ${comp.name} в ${currentComp.name}!`);
                updateUI();
            }
        };

        window.switchComputer = (index) => {
            if (index >= 0 && index < computers.length) {
                currentComputer = index;
                updateUI();
                
                // Подсветить выбранный компьютер
                computerModels.forEach((model, i) => {
                    model.children.forEach(child => {
                        if (child.material && child.material.emissive) {
                            child.material.emissive.setHex(i === index ? 0x224400 : 0x000000);
                        }
                    });
                });
            }
        };

        // Новый компьютер
        document.getElementById('new-computer-btn').addEventListener('click', () => {
            if (balance >= 2000 && computers.length < 6) {
                balance -= 2000;
                
                const newId = computers.length;
                
                // Создаем новый 3D корпус на свободной позиции
                const pos = computerPositions[computers.length];
                const newComputer = createComputerCase(pos[0], pos[1], newId);
                scene.add(newComputer);
                computerModels.push(newComputer);
                
                // Добавляем в массив данных
                computers.push({
                    id: newId,
                    name: `ПК #${newId + 1}`,
                    components: new Array(componentsData.length).fill(false),
                    totalIncome: 0,
                    totalPower: 0
                });
                
                showNotification(`Новый компьютер #${newId + 1} построен!`);
                updateUI();
            }
        });

        // Продажа ПК клиенту
        document.getElementById('sell-button').addEventListener('click', () => {
            const canSell = checkOrderCompletion();
            if (canSell) {
                balance += currentOrder.reward;
                
                // Анимация радости у человечков
                humans.forEach(human => {
                    human.position.y = 0.2;
                    setTimeout(() => {
                        human.position.y = 0;
                    }, 500);
                });
                
                // Новый заказ
                const newClient = clients[Math.floor(Math.random() * clients.length)];
                const newRequirements = {
                    cpu: ['Intel Core i5', 'Intel Core i7', 'Intel Core i9'][Math.floor(Math.random() * 3)],
                    gpu: ['RTX 3060', 'RTX 3070', 'RTX 3080', 'RTX 4090'][Math.floor(Math.random() * 4)],
                    ram: [8, 16, 32, 64, 128][Math.floor(Math.random() * 5)],
                    storage: [500, 1000, 2000, 4000, 8000][Math.floor(Math.random() * 5)]
                };
                const newReward = 300 + (newRequirements.ram * 5) + (newRequirements.storage / 10);
                
                currentOrder = {
                    client: newClient,
                    requirements: newRequirements,
                    reward: Math.floor(newReward)
                };
                
                document.getElementById('client-name').textContent = newClient.name;
                document.getElementById('client-avatar').textContent = newClient.avatar;
                document.getElementById('client-message').textContent = `"${newClient.message}"`;
                document.getElementById('req-cpu').textContent = newRequirements.cpu;
                document.getElementById('req-gpu').textContent = newRequirements.gpu;
                document.getElementById('req-ram').textContent = newRequirements.ram + 'GB';
                document.getElementById('req-storage').textContent = newRequirements.storage + 'GB';
                document.getElementById('order-reward').textContent = `+${currentOrder.reward} монет за выполнение заказа`;
                
                showNotification(`Заказ выполнен! +${currentOrder.reward} монет!`);
                updateUI();
            }
        });

        // Скрытие/показ магазина
        document.getElementById('toggle-shop').addEventListener('click', () => {
            const shop = document.getElementById('shop-panel');
            const toggle = document.getElementById('toggle-shop');
            
            shopHidden = !shopHidden;
            
            if (shopHidden) {
                shop.classList.add('hidden');
                toggle.classList.add('hidden');
                toggle.textContent = '▼ Показать магазин';
            } else {
                shop.classList.remove('hidden');
                toggle.classList.remove('hidden');
                toggle.textContent = '▲ Скрыть магазин';
            }
        });

        // Управление панелью клиента
        document.getElementById('minimize-client').addEventListener('click', () => {
            const client = document.getElementById('client-panel');
            const openBtn = document.getElementById('open-client-btn');
            
            clientPanelHidden = !clientPanelHidden;
            
            if (clientPanelHidden) {
                client.classList.add('hidden');
                openBtn.classList.add('visible');
            } else {
                client.classList.remove('hidden');
                openBtn.classList.remove('visible');
            }
        });

        document.getElementById('close-client').addEventListener('click', () => {
            const client = document.getElementById('client-panel');
            const openBtn = document.getElementById('open-client-btn');
            
            clientPanelClosed = !clientPanelClosed;
            
            if (clientPanelClosed) {
                client.classList.add('hidden');
                openBtn.classList.add('visible');
            }
        });

        document.getElementById('open-client-btn').addEventListener('click', () => {
            const client = document.getElementById('client-panel');
            const openBtn = document.getElementById('open-client-btn');
            
            clientPanelClosed = false;
            clientPanelHidden = false;
            client.classList.remove('hidden');
            openBtn.classList.remove('visible');
            
            // Обновляем статусы заказа
            checkOrderCompletion();
        });

        window.filterCategory = (category) => {
            currentCategory = category;
            document.querySelectorAll('.category-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            updateUI();
        };

        function showNotification(text) {
            const notif = document.getElementById('notification');
            notif.textContent = text;
            notif.style.display = 'block';
            setTimeout(() => {
                notif.style.display = 'none';
            }, 2000);
        }

        function createFloatingParticle(text, x, y) {
            const particle = document.createElement('div');
            particle.className = 'floating-particle';
            particle.textContent = text;
            particle.style.left = x + 'px';
            particle.style.top = y + 'px';
            document.body.appendChild(particle);
            setTimeout(() => particle.remove(), 1500);
        }

        // Буст
        function updateBoostUI() {
            const boostBtn = document.getElementById('boost-button');
            const cooldownBar = document.getElementById('boost-cooldown');
            const boostTimer = document.getElementById('boost-timer');
            
            const now = Math.floor(Date.now() / 1000);
            
            if (boostActive) {
                const timeLeft = boostEndTime - now;
                if (timeLeft > 0) {
                    boostTimer.textContent = `${timeLeft}с`;
                    boostTimer.style.display = 'block';
                } else {
                    boostActive = false;
                    boostTimer.style.display = 'none';
                    boostBtn.classList.remove('boost-active');
                }
            }
            
            if (boostCooldown) {
                const cooldownLeft = boostCooldownEnd - now;
                if (cooldownLeft > 0) {
                    const percent = (cooldownLeft / BOOST_COOLDOWN) * 100;
                    cooldownBar.style.width = percent + '%';
                    boostBtn.disabled = true;
                } else {
                    boostCooldown = false;
                    cooldownBar.style.width = '0%';
                    boostBtn.disabled = false;
                }
            }
        }

        document.getElementById('boost-button').addEventListener('click', () => {
            const now = Math.floor(Date.now() / 1000);
            
            if (!boostActive && !boostCooldown) {
                boostActive = true;
                boostEndTime = now + BOOST_DURATION;
                boostCooldown = true;
                boostCooldownEnd = now + BOOST_COOLDOWN;
                
                const boostBtn = document.getElementById('boost-button');
                boostBtn.classList.add('boost-active');
                
                showNotification('⚡ БУСТ АКТИВИРОВАН! x2 ДОХОД НА 10 СЕКУНД!');
                
                computerModels.forEach(model => {
                    model.children.forEach(child => {
                        if (child.material && child.material.emissive) {
                            child.material.emissive.setHex(0xaa5500);
                        }
                    });
                });
                
                setTimeout(() => {
                    computerModels.forEach(model => {
                        model.children.forEach(child => {
                            if (child.material && child.material.emissive) {
                                child.material.emissive.setHex(0x000000);
                            }
                        });
                    });
                }, BOOST_DURATION * 1000);
            }
        });

        setInterval(updateBoostUI, 100);

        // Майнинг кнопка - ИСПРАВЛЕНО!
        document.getElementById('mining-button').addEventListener('click', (e) => {
            const baseReward = 5;
            const powerBonus = Math.floor(totalPower / 10);
            const boostMultiplier = boostActive ? 2 : 1;
            const reward = (baseReward + powerBonus) * boostMultiplier;
            
            balance += reward;
            
            showNotification(`+${reward} монет! ${boostActive ? '(x2 BOOST)' : ''}`);
            
            for (let i = 0; i < 5; i++) {
                setTimeout(() => {
                    createFloatingParticle(`+${Math.floor(reward/3)}`, 
                        e.clientX + (Math.random() - 0.5) * 150, 
                        e.clientY + (Math.random() - 0.5) * 100);
                }, i * 100);
            }
            
            computerModels.forEach(model => {
                model.children.forEach(child => {
                    if (child.material && child.material.emissive) {
                        child.material.emissive.setHex(boostActive ? 0xaa5500 : 0x224400);
                        setTimeout(() => {
                            if (!boostActive) {
                                child.material.emissive.setHex(0x000000);
                            }
                        }, 200);
                    }
                });
            });
            
            updateUI();
        });

        // Прогресс бар для сбора 20 монет
        setInterval(() => {
            if (miningProgress < 100) {
                const speed = 0.5 + totalPower / 50;
                miningProgress += speed;
                const progress = Math.min(miningProgress, 100);
                document.getElementById('progress-bar').style.width = progress + '%';
                document.getElementById('progress-text').textContent = Math.floor(progress) + '%';
            }
        }, 100);

        // Клик на прогресс бар для получения 20 монет
        document.getElementById('progress-container').addEventListener('click', (e) => {
            if (miningProgress >= 100) {
                const baseReward = 20;
                const powerBonus = Math.floor(totalPower / 5);
                const boostMultiplier = boostActive ? 2 : 1;
                const reward = (baseReward + powerBonus) * boostMultiplier;
                
                balance += reward;
                miningProgress = 0;
                document.getElementById('progress-bar').style.width = '0%';
                document.getElementById('progress-text').textContent = '0%';
                
                showNotification(`+${reward} монет с плашки! ${boostActive ? '(x2 BOOST)' : ''}`);
                
                for (let i = 0; i < 5; i++) {
                    setTimeout(() => {
                        createFloatingParticle(`+${Math.floor(reward/3)}`, 
                            e.clientX + (Math.random() - 0.5) * 150, 
                            e.clientY + (Math.random() - 0.5) * 100);
                    }, i * 100);
                }
                
                updateUI();
            }
        });

        // Автоматический доход
        setInterval(() => {
            const multiplier = boostActive ? 2 : 1;
            balance += totalIncome * multiplier;
            updateUI();
        }, 1000);

        // Анимация танцующих человечков
        let danceTime = 0;
        setInterval(() => {
            danceTime += 0.1;
            
            humans.forEach((human, index) => {
                // Разные танцы для разных персонажей
                if (index % 3 === 0) {
                    // Танец с подпрыгиванием
                    human.position.y = Math.abs(Math.sin(danceTime * 2 + index)) * 0.1;
                    human.rotation.y = Math.sin(danceTime + index) * 0.5;
                } else if (index % 3 === 1) {
                    // Танец с вращением
                    human.rotation.y += 0.02;
                    human.position.y = Math.sin(danceTime * 3 + index) * 0.05;
                } else {
                    // Танец с наклонами
                    human.rotation.z = Math.sin(danceTime * 2 + index) * 0.1;
                    human.rotation.x = Math.sin(danceTime * 2 + index + 1) * 0.1;
                    human.position.y = Math.abs(Math.sin(danceTime * 2 + index)) * 0.08;
                }
                
                // Анимация рук
                human.children.forEach(child => {
                    if (child.geometry && child.geometry.type === 'CylinderGeometry') {
                        if (child.position.x < -0.3) { // Левая рука
                            child.rotation.z = Math.sin(danceTime * 3 + index) * 0.3;
                        }
                        if (child.position.x > 0.3) { // Правая рука
                            child.rotation.z = Math.sin(danceTime * 3 + index + 2) * 0.3;
                        }
                    }
                });
            });
        }, 50);

        // Анимация
        function animate() {
            requestAnimationFrame(animate);

            particles.rotation.y += 0.001;
            
            computerModels.forEach((model, i) => {
                const comp = computers[i];
                if (comp && comp.components.filter(c => c).length > 0) {
                    const time = Date.now() * 0.001;
                    model.children.forEach(child => {
                        if (child.material && child.material.emissive && child.material.emissive.getHex() !== 0xaa5500) {
                            const intensity = 0.1 + Math.sin(time * 2 + i) * 0.1;
                            child.material.emissive.setHSL(0.3, 1, intensity);
                        }
                    });
                }
            });

            controls.update();
            renderer.render(scene, camera);
        }

        animate();

        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });

        // Инициализация
        updateUI();
    </script>
</body>
</html>
