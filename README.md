<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D МАЙНИНГ ФЕРМА - Строй свою империю</title>
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
        }

        .key {
            background: rgba(255, 255, 255, 0.2);
            padding: 4px 10px;
            border-radius: 8px;
            margin: 0 4px;
            border: 1px solid rgba(255, 255, 255, 0.3);
        }

        #shop-panel {
            position: absolute;
            top: 20px;
            right: 20px;
            width: 400px;
            background: rgba(20, 30, 45, 0.98);
            backdrop-filter: blur(10px);
            border: 2px solid #ffaa00;
            border-radius: 25px;
            padding: 20px;
            color: white;
            box-shadow: 0 0 40px rgba(255, 170, 0, 0.3);
            max-height: calc(100vh - 100px);
            overflow-y: auto;
            z-index: 1000;
            pointer-events: auto;
            transition: transform 0.3s ease;
        }

        #shop-panel.hidden {
            transform: translateX(420px);
        }

        #toggle-shop {
            position: absolute;
            top: 20px;
            right: 420px;
            background: #ffaa00;
            color: black;
            border: none;
            border-radius: 50px 0 0 50px;
            padding: 15px 20px;
            font-size: 20px;
            font-weight: bold;
            cursor: pointer;
            z-index: 1001;
            transition: right 0.3s ease;
            box-shadow: -5px 0 15px rgba(255, 170, 0, 0.3);
        }

        #toggle-shop.hidden {
            right: 20px;
            border-radius: 50px;
        }

        #shop-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid rgba(255, 170, 0, 0.3);
        }

        #shop-header h2 {
            color: #ffaa00;
            font-size: 24px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        #shop-categories {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .category-btn {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 170, 0, 0.3);
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 14px;
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

        .shop-item {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 15px;
            margin-bottom: 15px;
            cursor: pointer;
            transition: all 0.3s;
            border-left: 5px solid #ffaa00;
            position: relative;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .shop-item:hover {
            transform: translateX(-5px);
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
            top: 10px;
            right: 10px;
            color: #00ffaa;
            font-size: 24px;
            font-weight: bold;
        }

        .item-icon {
            width: 50px;
            height: 50px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
        }

        .item-info {
            flex: 1;
        }

        .item-name {
            font-size: 16px;
            font-weight: bold;
            color: #ffaa00;
            margin-bottom: 5px;
        }

        .item-price {
            color: #00ffaa;
            font-size: 14px;
            margin-bottom: 3px;
        }

        .item-income {
            color: #ffaa00;
            font-size: 13px;
        }

        .item-specs {
            font-size: 11px;
            color: #888;
            margin-top: 5px;
        }

        #action-buttons {
            position: absolute;
            bottom: 50px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 20px;
            z-index: 1000;
            pointer-events: auto;
        }

        .action-btn {
            padding: 20px 40px;
            border: none;
            border-radius: 60px;
            font-size: 24px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
            letter-spacing: 2px;
            box-shadow: 0 0 30px;
            min-width: 250px;
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
            bottom: 150px;
            left: 50%;
            transform: translateX(-50%);
            width: 600px;
            background: rgba(0, 0, 0, 0.7);
            border-radius: 15px;
            padding: 5px;
            border: 2px solid #00ffaa;
            z-index: 1000;
        }

        #progress-bar {
            height: 30px;
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
            font-size: 16px;
            z-index: 1;
        }

        #computers-panel {
            position: absolute;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.8);
            border: 2px solid #00ffaa;
            border-radius: 15px;
            padding: 15px;
            color: white;
            z-index: 1000;
            pointer-events: auto;
            display: flex;
            gap: 15px;
            min-width: 500px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .computer-slot {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid #ffaa00;
            border-radius: 10px;
            padding: 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            min-width: 100px;
        }

        .computer-slot:hover {
            background: rgba(255, 170, 0, 0.2);
            transform: translateY(-5px);
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

        .computer-slot .power {
            color: #00ffaa;
            font-size: 12px;
        }

        #new-computer-btn {
            background: linear-gradient(135deg, #00ffaa, #00cc88);
            color: black;
            border: none;
            border-radius: 10px;
            padding: 10px 20px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 16px;
            box-shadow: 0 0 20px #00ffaa;
        }

        #new-computer-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 0 30px #00ffaa;
        }

        #new-computer-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
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
            font-size: 24px;
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
    </style>
</head>
<body>
    <div id="ui-container">
        <div id="stats-panel">
            <div style="font-size: 14px; color: #888; margin-bottom: 5px;">БАЛАНС</div>
            <div id="balance">1000</div>
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
        </div>
    </div>

    <button id="toggle-shop">◀ Скрыть магазин</button>

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

    <div id="computers-panel">
        <div id="computers-list"></div>
        <button id="new-computer-btn">➕ Купить новый ПК (2000💰) 1/6</button>
    </div>

    <div id="action-buttons">
        <button id="mining-button" class="action-btn">⛏ МАЙНИНГ</button>
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

    <div id="notification">+100 монет!</div>

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

        // ============ РАСШИРЕННЫЙ СПИСОК КОМПЛЕКТУЮЩИХ ============
        const componentsData = [
            // Процессоры
            { id: 'cpu_i3', name: 'Intel Core i3-12100', category: 'cpu', price: 150, income: 3, power: 4, tier: 1, icon: '🔹', specs: '4 ядра, 8 потоков' },
            { id: 'cpu_i5', name: 'Intel Core i5-13600K', category: 'cpu', price: 350, income: 8, power: 14, tier: 2, icon: '🔶', specs: '14 ядер, 20 потоков' },
            { id: 'cpu_i7', name: 'Intel Core i7-13700K', category: 'cpu', price: 500, income: 15, power: 20, tier: 3, icon: '💠', specs: '16 ядер, 24 потока' },
            { id: 'cpu_i9', name: 'Intel Core i9-13900KS', category: 'cpu', price: 800, income: 25, power: 30, tier: 4, icon: '💎', specs: '24 ядра, 32 потока' },
            { id: 'cpu_ryzen5', name: 'AMD Ryzen 5 7600X', category: 'cpu', price: 300, income: 7, power: 12, tier: 2, icon: '🔸', specs: '6 ядер, 12 потоков' },
            { id: 'cpu_ryzen7', name: 'AMD Ryzen 7 7800X3D', category: 'cpu', price: 550, income: 18, power: 22, tier: 3, icon: '🔷', specs: '8 ядер, 16 потоков' },
            { id: 'cpu_ryzen9', name: 'AMD Ryzen 9 7950X', category: 'cpu', price: 900, income: 30, power: 35, tier: 4, icon: '⚡', specs: '16 ядер, 32 потока' },
            { id: 'cpu_threadripper', name: 'AMD Threadripper 7980X', category: 'cpu', price: 2500, income: 80, power: 80, tier: 5, icon: '👑', specs: '64 ядра, 128 потоков' },

            // Видеокарты
            { id: 'gpu_1650', name: 'NVIDIA GTX 1650', category: 'gpu', price: 200, income: 5, power: 6, tier: 1, icon: '🎮', specs: '4GB GDDR6' },
            { id: 'gpu_1660', name: 'NVIDIA GTX 1660 Super', category: 'gpu', price: 300, income: 8, power: 10, tier: 2, icon: '🎯', specs: '6GB GDDR6' },
            { id: 'gpu_2060', name: 'NVIDIA RTX 2060', category: 'gpu', price: 400, income: 12, power: 15, tier: 2, icon: '✨', specs: '6GB GDDR6' },
            { id: 'gpu_3060', name: 'NVIDIA RTX 3060 Ti', category: 'gpu', price: 600, income: 20, power: 25, tier: 3, icon: '⚡', specs: '8GB GDDR6' },
            { id: 'gpu_3070', name: 'NVIDIA RTX 3070', category: 'gpu', price: 800, income: 30, power: 35, tier: 3, icon: '💫', specs: '8GB GDDR6' },
            { id: 'gpu_3080', name: 'NVIDIA RTX 3080', category: 'gpu', price: 1200, income: 45, power: 50, tier: 4, icon: '🚀', specs: '10GB GDDR6X' },
            { id: 'gpu_3090', name: 'NVIDIA RTX 3090 Ti', category: 'gpu', price: 2000, income: 70, power: 75, tier: 5, icon: '💥', specs: '24GB GDDR6X' },
            { id: 'gpu_4090', name: 'NVIDIA RTX 4090', category: 'gpu', price: 3000, income: 120, power: 120, tier: 6, icon: '👑', specs: '24GB GDDR6X' },
            { id: 'gpu_rx6600', name: 'AMD RX 6600 XT', category: 'gpu', price: 450, income: 15, power: 18, tier: 2, icon: '🔴', specs: '8GB GDDR6' },
            { id: 'gpu_rx6700', name: 'AMD RX 6700 XT', category: 'gpu', price: 650, income: 25, power: 28, tier: 3, icon: '❤️', specs: '12GB GDDR6' },
            { id: 'gpu_rx6800', name: 'AMD RX 6800 XT', category: 'gpu', price: 900, income: 40, power: 45, tier: 4, icon: '🔥', specs: '16GB GDDR6' },
            { id: 'gpu_rx7900', name: 'AMD RX 7900 XTX', category: 'gpu', price: 2500, income: 100, power: 100, tier: 6, icon: '⭐', specs: '24GB GDDR6' },

            // ОПЕРАТИВНАЯ ПАМЯТЬ (УВЕЛИЧЕНО)
            { id: 'ram_4gb', name: 'DDR3 4GB 1600MHz', category: 'ram', price: 30, income: 0.5, power: 1, tier: 1, icon: '💾', specs: '4GB, CL11' },
            { id: 'ram_8gb', name: 'DDR4 8GB 3200MHz', category: 'ram', price: 50, income: 1, power: 2, tier: 1, icon: '🧠', specs: '8GB, CL16' },
            { id: 'ram_16gb', name: 'DDR4 16GB 3600MHz', category: 'ram', price: 90, income: 2, power: 4, tier: 2, icon: '🧮', specs: '16GB, CL18' },
            { id: 'ram_32gb', name: 'DDR4 32GB 3600MHz', category: 'ram', price: 180, income: 4, power: 8, tier: 3, icon: '💿', specs: '32GB, CL18' },
            { id: 'ram_64gb', name: 'DDR4 64GB 3200MHz', category: 'ram', price: 350, income: 8, power: 15, tier: 4, icon: '📀', specs: '64GB, CL16' },
            { id: 'ram_ddr5_16', name: 'DDR5 16GB 6000MHz', category: 'ram', price: 150, income: 3, power: 6, tier: 3, icon: '⚡', specs: '16GB, CL36' },
            { id: 'ram_ddr5_32', name: 'DDR5 32GB 6000MHz', category: 'ram', price: 280, income: 6, power: 12, tier: 4, icon: '💨', specs: '32GB, CL36' },
            { id: 'ram_ddr5_64', name: 'DDR5 64GB 6400MHz', category: 'ram', price: 550, income: 12, power: 24, tier: 5, icon: '🚀', specs: '64GB, CL32' },
            { id: 'ram_ddr5_128', name: 'DDR5 128GB 5600MHz', category: 'ram', price: 1200, income: 25, power: 45, tier: 6, icon: '💎', specs: '128GB, CL40' },
            { id: 'ram_ecc', name: 'DDR4 ECC 64GB Server', category: 'ram', price: 800, income: 18, power: 30, tier: 5, icon: '🔒', specs: '64GB, ECC' },

            // НАКОПИТЕЛИ (УВЕЛИЧЕНО)
            { id: 'ssd_120', name: 'SSD 120GB SATA', category: 'storage', price: 30, income: 0.5, power: 1, tier: 1, icon: '💿', specs: '120GB, 450MB/s' },
            { id: 'ssd_240', name: 'SSD 240GB SATA', category: 'storage', price: 45, income: 0.8, power: 1.5, tier: 1, icon: '💿', specs: '240GB, 500MB/s' },
            { id: 'ssd_480', name: 'SSD 480GB SATA', category: 'storage', price: 60, income: 1.2, power: 2, tier: 2, icon: '💿', specs: '480GB, 550MB/s' },
            { id: 'ssd_500', name: 'SSD 500GB NVMe', category: 'storage', price: 80, income: 2, power: 3, tier: 2, icon: '⚡', specs: '500GB, 3500MB/s' },
            { id: 'ssd_1tb', name: 'SSD 1TB NVMe', category: 'storage', price: 150, income: 3, power: 4, tier: 3, icon: '💨', specs: '1TB, 5000MB/s' },
            { id: 'ssd_2tb', name: 'SSD 2TB NVMe', category: 'storage', price: 280, income: 5, power: 7, tier: 4, icon: '🚀', specs: '2TB, 7000MB/s' },
            { id: 'ssd_4tb', name: 'SSD 4TB NVMe', category: 'storage', price: 550, income: 10, power: 13, tier: 5, icon: '💫', specs: '4TB, 7000MB/s' },
            { id: 'ssd_8tb', name: 'SSD 8TB NVMe', category: 'storage', price: 1200, income: 20, power: 25, tier: 6, icon: '🌟', specs: '8TB, 7000MB/s' },
            { id: 'hdd_500', name: 'HDD 500GB', category: 'storage', price: 25, income: 0.3, power: 0.5, tier: 1, icon: '💽', specs: '500GB, 150MB/s' },
            { id: 'hdd_1tb', name: 'HDD 1TB 7200rpm', category: 'storage', price: 50, income: 0.6, power: 1, tier: 1, icon: '💽', specs: '1TB, 160MB/s' },
            { id: 'hdd_2tb', name: 'HDD 2TB 7200rpm', category: 'storage', price: 80, income: 1, power: 1.5, tier: 2, icon: '💽', specs: '2TB, 180MB/s' },
            { id: 'hdd_4tb', name: 'HDD 4TB 7200rpm', category: 'storage', price: 120, income: 1.5, power: 2, tier: 2, icon: '📀', specs: '4TB, 180MB/s' },
            { id: 'hdd_8tb', name: 'HDD 8TB 7200rpm', category: 'storage', price: 200, income: 2.5, power: 3, tier: 3, icon: '📀', specs: '8TB, 200MB/s' },
            { id: 'hdd_10tb', name: 'HDD 10TB 7200rpm', category: 'storage', price: 300, income: 3.5, power: 4, tier: 3, icon: '💿', specs: '10TB, 210MB/s' },
            { id: 'hdd_14tb', name: 'HDD 14TB 7200rpm', category: 'storage', price: 450, income: 5, power: 6, tier: 4, icon: '💿', specs: '14TB, 220MB/s' },
            { id: 'hdd_18tb', name: 'HDD 18TB 7200rpm', category: 'storage', price: 600, income: 7, power: 8, tier: 5, icon: '📀', specs: '18TB, 240MB/s' },
            { id: 'hdd_20tb', name: 'HDD 20TB 7200rpm', category: 'storage', price: 800, income: 9, power: 10, tier: 5, icon: '📀', specs: '20TB, 250MB/s' },

            // ОХЛАЖДЕНИЕ (УВЕЛИЧЕНО)
            { id: 'cooler_air', name: 'Воздушный кулер', category: 'cooling', price: 50, income: 1, power: 2, tier: 1, icon: '🌀', specs: 'TDP 150W' },
            { id: 'cooler_air_pro', name: 'Башенный кулер', category: 'cooling', price: 120, income: 2, power: 4, tier: 2, icon: '💨', specs: 'TDP 220W' },
            { id: 'cooler_air_dual', name: 'Двухбашенный кулер', category: 'cooling', price: 200, income: 3, power: 6, tier: 3, icon: '🌪️', specs: 'TDP 280W' },
            { id: 'aio_120', name: 'Жидкостное охлаждение 120мм', category: 'cooling', price: 150, income: 3, power: 5, tier: 3, icon: '💧', specs: 'TDP 200W' },
            { id: 'aio_240', name: 'Жидкостное охлаждение 240мм', category: 'cooling', price: 250, income: 5, power: 8, tier: 4, icon: '🌊', specs: 'TDP 280W' },
            { id: 'aio_280', name: 'Жидкостное охлаждение 280мм', category: 'cooling', price: 320, income: 6.5, power: 10, tier: 4, icon: '💧', specs: 'TDP 320W' },
            { id: 'aio_360', name: 'Жидкостное охлаждение 360мм', category: 'cooling', price: 400, income: 8, power: 12, tier: 5, icon: '💦', specs: 'TDP 350W' },
            { id: 'aio_420', name: 'Жидкостное охлаждение 420мм', category: 'cooling', price: 550, income: 11, power: 16, tier: 6, icon: '🌊', specs: 'TDP 400W' },
            { id: 'custom_loop', name: 'Кастомная СЖО', category: 'cooling', price: 800, income: 15, power: 20, tier: 6, icon: '🔮', specs: 'TDP 500W+' },
            { id: 'phase_change', name: 'Фазовый переход', category: 'cooling', price: 1500, income: 25, power: 35, tier: 7, icon: '❄️', specs: 'Экстремальное' },
            { id: 'liquid_nitrogen', name: 'Жидкий азот', category: 'cooling', price: 2500, income: 40, power: 50, tier: 8, icon: '🧊', specs: '-196°C' },

            // БЛОКИ ПИТАНИЯ (УВЕЛИЧЕНО)
            { id: 'psu_400w', name: 'БП 400W Bronze', category: 'psu', price: 60, income: 1, power: 2, tier: 1, icon: '🔌', specs: '400W, 80+ Bronze' },
            { id: 'psu_500w', name: 'БП 500W Bronze', category: 'psu', price: 80, income: 1.5, power: 3, tier: 1, icon: '🔌', specs: '500W, 80+ Bronze' },
            { id: 'psu_550w', name: 'БП 550W Gold', category: 'psu', price: 120, income: 2, power: 4, tier: 2, icon: '⚡', specs: '550W, 80+ Gold' },
            { id: 'psu_650w', name: 'БП 650W Gold', category: 'psu', price: 160, income: 3, power: 5, tier: 2, icon: '⚡', specs: '650W, 80+ Gold' },
            { id: 'psu_750w', name: 'БП 750W Gold', category: 'psu', price: 200, income: 4, power: 7, tier: 3, icon: '💡', specs: '750W, 80+ Gold' },
            { id: 'psu_850w', name: 'БП 850W Platinum', category: 'psu', price: 300, income: 6, power: 10, tier: 4, icon: '🔋', specs: '850W, 80+ Platinum' },
            { id: 'psu_1000w', name: 'БП 1000W Platinum', category: 'psu', price: 450, income: 9, power: 14, tier: 5, icon: '⚡', specs: '1000W, 80+ Platinum' },
            { id: 'psu_1200w', name: 'БП 1200W Titanium', category: 'psu', price: 700, income: 14, power: 20, tier: 6, icon: '💎', specs: '1200W, 80+ Titanium' },
            { id: 'psu_1600w', name: 'БП 1600W Titanium', category: 'psu', price: 1200, income: 22, power: 30, tier: 7, icon: '👑', specs: '1600W, 80+ Titanium' },
            { id: 'psu_2000w', name: 'БП 2000W Server', category: 'psu', price: 2000, income: 35, power: 45, tier: 8, icon: '🏭', specs: '2000W, Redundant' },
            { id: 'psu_2400w', name: 'БП 2400W Server', category: 'psu', price: 3000, income: 50, power: 60, tier: 9, icon: '⚙️', specs: '2400W, Redundant' }
        ];

        // Система компьютеров - начинаем с 1 ПК
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
        let balance = 1000;
        let totalIncome = 0;
        let totalPower = 0;
        let miningProgress = 0;
        let currentCategory = 'all';

        // Boost система
        let boostActive = false;
        let boostEndTime = 0;
        let boostCooldown = false;
        let boostCooldownEnd = 0;
        const BOOST_DURATION = 10;
        const BOOST_COOLDOWN = 300;

        // Состояние магазина
        let shopHidden = false;

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

        // Позиции для будущих компьютеров (вокруг центра)
        const computerPositions = [
            [0, 0],      // центр
            [5, 0],      // справа
            [-5, 0],     // слева
            [0, 5],      // сверху
            [0, -5],     // снизу
            [5, 5]       // диагональ
        ];

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
        
        function updateUI() {
            document.getElementById('balance').textContent = Math.floor(balance);
            
            const displayIncome = boostActive ? totalIncome * 2 : totalIncome;
            document.getElementById('income').textContent = displayIncome.toFixed(1);
            document.getElementById('mining-power').textContent = `Мощность фермы: ${totalPower} GH/s ${boostActive ? '(x2)' : ''}`;
            
            document.getElementById('computers-count').textContent = computers.length;
            document.getElementById('total-parts').textContent = computers.reduce((sum, comp) => 
                sum + comp.components.filter(c => c).length, 0);
            
            // Обновление списка компьютеров
            let computersHTML = computers.map((comp, index) => `
                <div class="computer-slot ${index === currentComputer ? 'active' : ''}" 
                     onclick="switchComputer(${index})">
                    <div>${comp.name}</div>
                    <div class="power">⚡ ${comp.totalPower} GH/s</div>
                    <div style="font-size: 10px; color: #888;">${comp.components.filter(c => c).length} компонентов</div>
                </div>
            `).join('');
            
            // Добавляем пустые слоты
            const emptySlots = 6 - computers.length;
            for (let i = 0; i < emptySlots; i++) {
                computersHTML += `
                    <div class="computer-slot empty">
                        <div>🔒 Пустой слот</div>
                        <div style="font-size: 10px; color: #888;">Купите новый ПК</div>
                    </div>
                `;
            }
            
            document.getElementById('computers-list').innerHTML = computersHTML;
            
            // Кнопка нового компьютера
            const newBtn = document.getElementById('new-computer-btn');
            if (computers.length >= 6) {
                newBtn.disabled = true;
                newBtn.textContent = '➕ Максимум ПК (6/6)';
            } else {
                newBtn.disabled = balance < 2000;
                newBtn.textContent = `➕ Купить новый ПК (2000💰) ${computers.length}/6`;
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

        // Скрытие/показ магазина
        document.getElementById('toggle-shop').addEventListener('click', () => {
            const shop = document.getElementById('shop-panel');
            const toggle = document.getElementById('toggle-shop');
            
            shopHidden = !shopHidden;
            
            if (shopHidden) {
                shop.classList.add('hidden');
                toggle.classList.add('hidden');
                toggle.textContent = '▶ Показать магазин';
            } else {
                shop.classList.remove('hidden');
                toggle.classList.remove('hidden');
                toggle.textContent = '◀ Скрыть магазин';
            }
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

        // Майнинг
        document.getElementById('mining-button').addEventListener('click', (e) => {
            const baseReward = 100;
            const powerBonus = Math.floor(totalPower / 5);
            const boostMultiplier = boostActive ? 2 : 1;
            const reward = (baseReward + powerBonus) * boostMultiplier;
            
            balance += reward;
            miningProgress = 0;
            document.getElementById('progress-bar').style.width = '0%';
            document.getElementById('progress-text').textContent = '0%';
            
            showNotification(`+${reward} монет! ${boostActive ? '(x2 BOOST)' : ''}`);
            
            for (let i = 0; i < 8; i++) {
                setTimeout(() => {
                    createFloatingParticle(`+${Math.floor(reward/5)}`, 
                        e.clientX + (Math.random() - 0.5) * 150, 
                        e.clientY + (Math.random() - 0.5) * 100);
                }, i * 80);
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

        // Прогресс
        setInterval(() => {
            if (miningProgress < 100) {
                const speed = 1 + totalPower / 30;
                miningProgress += speed;
                const progress = Math.min(miningProgress, 100);
                document.getElementById('progress-bar').style.width = progress + '%';
                document.getElementById('progress-text').textContent = Math.floor(progress) + '%';
            }
        }, 100);

        // Автоматический доход
        setInterval(() => {
            const multiplier = boostActive ? 2 : 1;
            balance += totalIncome * multiplier;
            updateUI();
        }, 1000);

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
