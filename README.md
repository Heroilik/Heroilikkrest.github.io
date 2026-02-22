<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>❌ Крестики-нолики P2P (WebRTC) ⭕</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
        }

        body {
            background: linear-gradient(145deg, #1e2b3a 0%, #0f1a24 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0;
            padding: 16px;
        }

        .game-card {
            max-width: 700px;
            width: 100%;
            background: rgba(25, 35, 48, 0.95);
            backdrop-filter: blur(8px);
            border-radius: 48px;
            padding: 32px;
            box-shadow: 0 30px 40px -15px black, 0 0 0 1px #2f4d73 inset;
            border: 1px solid #3a607f;
        }

        h1 {
            color: #e3f0ff;
            font-weight: 400;
            font-size: 2.2rem;
            margin: 0 0 10px 0;
            display: flex;
            align-items: center;
            gap: 15px;
            border-bottom: 2px solid #2b4b6f;
            padding-bottom: 20px;
        }

        .debug-section {
            background: #0e1a24;
            border-radius: 20px;
            padding: 15px;
            margin: 15px 0;
            font-family: monospace;
            color: #a0c0e0;
            border: 1px solid #2a4b6e;
            max-height: 120px;
            overflow-y: auto;
            font-size: 0.9rem;
        }

        .debug-log {
            margin: 3px 0;
            border-bottom: 1px dotted #2e4a66;
            padding: 3px 0;
        }

        .peer-section {
            background: #1e3142;
            border-radius: 50px;
            padding: 25px;
            margin: 20px 0;
            border: 1px solid #3f6792;
        }

        .input-group {
            display: flex;
            gap: 12px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .input-field {
            flex: 1;
            background: #0d1a29;
            border: 2px solid #2f5377;
            border-radius: 60px;
            padding: 16px 24px;
            font-size: 1.1rem;
            color: white;
            outline: none;
            transition: 0.15s;
        }

        .input-field:focus {
            border-color: #60a5ff;
            box-shadow: 0 0 0 3px #60a5ff40;
        }

        .btn {
            background: #2b4f77;
            border: none;
            border-radius: 60px;
            padding: 16px 32px;
            font-size: 1.1rem;
            font-weight: 600;
            color: white;
            cursor: pointer;
            border-bottom: 4px solid #12304a;
            transition: 0.08s;
            box-shadow: 0 7px 0 #0f1f30;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn:active {
            transform: translateY(5px);
            border-bottom-width: 2px;
            box-shadow: 0 2px 0 #0f1f30;
        }

        .btn.secondary {
            background: #3d4f6b;
            border-bottom-color: #1d2c3f;
        }

        .btn.success {
            background: #2b6b4f;
            border-bottom-color: #1a4a35;
        }

        .status-box {
            background: #0e1e2d;
            border-radius: 40px;
            padding: 16px 24px;
            color: #c6defa;
            font-size: 1.1rem;
            border: 1px solid #315f86;
            margin: 20px 0;
            display: flex;
            align-items: center;
            gap: 12px;
            flex-wrap: wrap;
        }

        .connection-indicator {
            width: 14px;
            height: 14px;
            border-radius: 50%;
            background: #777;
            display: inline-block;
            transition: 0.2s;
        }

        .connection-indicator.connected {
            background: #42f56c;
            box-shadow: 0 0 15px #3eff9e;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            background: #1b2e44;
            padding: 20px;
            border-radius: 40px;
            margin: 30px 0;
            border: 2px solid #2a5985;
        }

        .cell {
            aspect-ratio: 1;
            background: #132334;
            border-radius: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 5rem;
            font-weight: 700;
            color: white;
            cursor: pointer;
            box-shadow: 0 12px 0 #0a131f, 0 0 0 2px #4080c0;
            transition: 0.06s linear;
            user-select: none;
        }

        .cell:active {
            transform: translateY(6px);
            box-shadow: 0 6px 0 #0a131f, 0 0 0 2px #60a0e0;
        }

        .cell.x-color {
            color: #6bc2ff;
            text-shadow: 0 0 15px #0077ff;
        }

        .cell.o-color {
            color: #ffb36b;
            text-shadow: 0 0 15px #ff8800;
        }

        .cell.disabled {
            pointer-events: none;
            opacity: 0.8;
        }

        .player-badge {
            background: #1a3148;
            border-radius: 30px;
            padding: 8px 20px;
            color: #bedcff;
            border: 1px solid #2e6b9b;
        }

        .footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 25px;
            color: #7e9fc7;
            flex-wrap: wrap;
            gap: 15px;
        }

        .id-display {
            background: #0e1e2d;
            padding: 10px 24px;
            border-radius: 40px;
            font-family: monospace;
            font-size: 1.3rem;
            border: 1px solid #3f6792;
            color: #b5d6ff;
        }

        .offline-test {
            margin-top: 20px;
            background: #1b3a4a;
            border-radius: 30px;
            padding: 15px;
            text-align: center;
        }
    </style>
    
    <!-- Подключаем PeerJS (WebRTC библиотека) -->
    <script src="https://unpkg.com/peerjs@1.5.2/dist/peerjs.min.js"></script>
</head>
<body>
    <div class="game-card">
        <h1>
            <span>❌⭕</span> P2P Tic-Tac-Toe
            <span style="font-size: 0.9rem; background: #203a50; padding: 5px 18px; border-radius: 30px;">WebRTC v2</span>
        </h1>

        <!-- Отладочное окно -->
        <div class="debug-section" id="debugLog">
            <div class="debug-log">🔍 Система отладки активна...</div>
        </div>

        <!-- Секция подключения -->
        <div class="peer-section">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px; flex-wrap: wrap; gap: 10px;">
                <span class="player-badge" id="myIdDisplay">🆔 ID: генерируется...</span>
                <div style="display: flex; gap: 10px;">
                    <button class="btn secondary" id="copyIdBtn" style="padding: 10px 20px;">📋 Копировать</button>
                    <button class="btn secondary" id="newIdBtn" style="padding: 10px 20px;">🔄 Новый ID</button>
                </div>
            </div>

            <div class="input-group">
                <input type="text" class="input-field" id="peerIdInput" placeholder="Введите ID соперника...">
                <button class="btn" id="connectBtn">🔗 Подключиться</button>
            </div>
            
            <div class="status-box" id="connectionStatus">
                <span class="connection-indicator" id="indicator"></span>
                <span id="statusText">Ожидание подключения...</span>
            </div>

            <!-- Кнопка оффлайн теста -->
            <div class="offline-test">
                <button class="btn success" id="offlineTestBtn" style="width: 100%;">🧪 Оффлайн тест (два игрока в одной вкладке)</button>
            </div>
        </div>

        <!-- Игровая доска -->
        <div class="board" id="board">
            <!-- Ячейки генерируются через JS -->
        </div>

        <!-- Информация о ходе -->
        <div class="footer">
            <div id="turnIndicator">❌ Ожидание игрока...</div>
            <button class="btn secondary" id="resetGameBtn" style="padding: 12px 28px;">🔄 Заново</button>
            <div id="opponentIdDisplay">🤝 нет оппонента</div>
        </div>
    </div>

    <script>
        (function() {
            // Логгер
            function addLog(message) {
                const debugDiv = document.getElementById('debugLog');
                const logEntry = document.createElement('div');
                logEntry.className = 'debug-log';
                logEntry.textContent = `[${new Date().toLocaleTimeString()}] ${message}`;
                debugDiv.appendChild(logEntry);
                debugDiv.scrollTop = debugDiv.scrollHeight;
                if (debugDiv.children.length > 6) {
                    debugDiv.removeChild(debugDiv.children[0]);
                }
                console.log(message);
            }

            // Состояние игры
            let board = ['', '', '', '', '', '', '', '', ''];
            let gameActive = false;
            let mySymbol = null;
            let opponentSymbol = null;
            let myTurn = false;

            // PeerJS переменные
            let peer = null;
            let conn = null;
            let myPeerId = null;
            let connectedPeerId = null;

            // DOM элементы
            const boardElement = document.getElementById('board');
            const myIdDisplay = document.getElementById('myIdDisplay');
            const opponentIdDisplay = document.getElementById('opponentIdDisplay');
            const connectionStatus = document.getElementById('statusText');
            const indicator = document.getElementById('indicator');
            const turnIndicator = document.getElementById('turnIndicator');
            const peerIdInput = document.getElementById('peerIdInput');
            const connectBtn = document.getElementById('connectBtn');
            const copyIdBtn = document.getElementById('copyIdBtn');
            const newIdBtn = document.getElementById('newIdBtn');
            const resetBtn = document.getElementById('resetGameBtn');
            const offlineTestBtn = document.getElementById('offlineTestBtn');

            // Инициализация Peer с улучшенными настройками
            function initPeer(customId = null) {
                if (peer) {
                    peer.destroy();
                }

                addLog('Инициализация PeerJS...');
                
                // Генерируем ID
                const peerId = customId || 'player_' + Math.random().toString(36).substring(2, 10) + '_' + Date.now().toString(36);
                
                // Улучшенная конфигурация ICE серверов
                peer = new Peer(peerId, {
                    host: '0.peerjs.com',
                    port: 443,
                    path: '/',
                    secure: true,
                    config: {
                        'iceServers': [
                            { urls: 'stun:stun.l.google.com:19302' },
                            { urls: 'stun:stun1.l.google.com:19302' },
                            { urls: 'stun:stun2.l.google.com:19302' },
                            { urls: 'stun:stun3.l.google.com:19302' },
                            { urls: 'stun:stun4.l.google.com:19302' },
                            // TURN сервер на случай строгих NAT (публичный, но медленный)
                            {
                                urls: 'turn:turn.bistri.com:80',
                                credential: 'homeo',
                                username: 'homeo'
                            },
                            {
                                urls: 'turn:turn.anyfirewall.com:443?transport=tcp',
                                credential: 'webrtc',
                                username: 'webrtc'
                            }
                        ]
                    },
                    debug: 2 // Включить подробное логирование PeerJS
                });

                peer.on('open', (id) => {
                    myPeerId = id;
                    myIdDisplay.innerText = `🆔 Ваш ID: ${id}`;
                    addLog(`✅ PeerJS открыт, ID: ${id}`);
                    connectionStatus.innerText = 'Сервер готов. Введите ID соперника.';
                });

                peer.on('connection', (incomingConn) => {
                    addLog(`📞 Входящее соединение от: ${incomingConn.peer}`);
                    
                    if (conn && conn.open) {
                        addLog('⚠️ Уже есть активное соединение, отклоняем');
                        incomingConn.close();
                        return;
                    }

                    conn = incomingConn;
                    connectedPeerId = conn.peer;
                    opponentIdDisplay.innerText = `🤝 соперник: ${connectedPeerId.substring(0,10)}...`;
                    
                    setupConnection();

                    // Мы принимающая сторона => мы O
                    mySymbol = 'O';
                    opponentSymbol = 'X';
                    myTurn = false;
                    
                    gameActive = true;
                    updateTurnDisplay();
                    updateUI();
                    addLog('🎮 Принято соединение. Вы играете за O (нолики)');
                });

                peer.on('disconnected', () => {
                    addLog('⚠️ PeerJS отключён от сервера');
                    connectionStatus.innerText = 'Потеря связи с сервером. Переподключаемся...';
                    peer.reconnect();
                });

                peer.on('error', (err) => {
                    addLog(`❌ Ошибка PeerJS: ${err.type} - ${err.message}`);
                    
                    if (err.type === 'unavailable-id') {
                        // ID занят, генерируем новый
                        addLog('🔄 ID занят, генерируем новый...');
                        initPeer();
                    } else if (err.type === 'peer-unavailable') {
                        connectionStatus.innerText = '❌ Игрок с таким ID не найден';
                    } else {
                        connectionStatus.innerText = `Ошибка: ${err.type}`;
                    }
                });

                peer.on('close', () => {
                    addLog('🔒 PeerJS закрыт');
                });
            }

            // Настройка соединения
            function setupConnection() {
                conn.on('open', () => {
                    addLog('🔗 Соединение установлено!');
                    indicator.classList.add('connected');
                    connectionStatus.innerText = '✅ Подключено к сопернику!';
                    
                    // Если символ ещё не определён (инициатор), то мы X
                    if (mySymbol === null) {
                        mySymbol = 'X';
                        opponentSymbol = 'O';
                        myTurn = true;
                        addLog('🎮 Вы инициатор, играете за X (крестики)');
                    }
                    
                    gameActive = true;
                    updateTurnDisplay();
                    updateUI();
                });

                conn.on('data', (data) => {
                    addLog(`📨 Получено: ${data.type}`);
                    
                    if (data.type === 'move') {
                        const index = data.index;
                        const symbol = data.symbol;

                        if (board[index] === '' && symbol === opponentSymbol) {
                            board[index] = symbol;
                            myTurn = true;
                            updateUI();
                            checkGameStatus();
                            addLog(`🎯 Ход соперника в клетку ${index}`);
                        }
                    } else if (data.type === 'reset') {
                        addLog('🔄 Соперник сбросил игру');
                        resetGameLocal(false);
                    }
                });

                conn.on('close', () => {
                    addLog('🔌 Соединение разорвано');
                    indicator.classList.remove('connected');
                    connectionStatus.innerText = 'Соединение потеряно';
                    gameActive = false;
                    connectedPeerId = null;
                    opponentIdDisplay.innerText = '🤝 нет оппонента';
                    mySymbol = null;
                    opponentSymbol = null;
                    myTurn = false;
                    updateUI();
                    updateTurnDisplay();
                });

                conn.on('error', (err) => {
                    addLog(`❌ Ошибка соединения: ${err}`);
                });
            }

            // Подключение к удалённому peer
            function connectToPeer(remoteId) {
                remoteId = remoteId.trim();
                if (!remoteId) {
                    alert('Введите ID соперника');
                    return;
                }

                if (remoteId === myPeerId) {
                    alert('Нельзя подключиться к самому себе! Используйте оффлайн тест для игры с собой.');
                    return;
                }

                if (conn && conn.open) {
                    conn.close();
                }

                addLog(`🔌 Попытка подключения к: ${remoteId}`);
                connectionStatus.innerText = `Подключаемся к ${remoteId}...`;
                
                conn = peer.connect(remoteId, {
                    reliable: true,
                    serialization: 'json'
                });

                connectedPeerId = remoteId;
                opponentIdDisplay.innerText = `🤝 соперник: ${remoteId.substring(0,10)}...`;

                // Мы инициатор => символ определится позже (в conn.on('open'))
                // Пока не назначаем символ

                setupConnection();
                
                // Таймаут подключения
                setTimeout(() => {
                    if (!conn || !conn.open) {
                        addLog('⏰ Таймаут подключения');
                        connectionStatus.innerText = '⏰ Таймаут. Проверьте ID.';
                    }
                }, 10000);
            }

            // Отрисовка доски
            function renderBoard() {
                let html = '';
                board.forEach((value, index) => {
                    const symbolClass = value === 'X' ? 'x-color' : (value === 'O' ? 'o-color' : '');
                    html += `<div class="cell ${symbolClass}" data-index="${index}">${value}</div>`;
                });
                boardElement.innerHTML = html;

                document.querySelectorAll('.cell').forEach(cell => {
                    cell.addEventListener('click', cellClickHandler);
                });
            }

            // Клик по клетке
            function cellClickHandler(e) {
                const index = e.currentTarget.dataset.index;
                if (!gameActive || !myTurn || board[index] !== '' || !conn || !conn.open) {
                    if (!conn || !conn.open) addLog('⚠️ Нет соединения');
                    else if (!gameActive) addLog('⚠️ Игра не активна');
                    else if (!myTurn) addLog('⚠️ Сейчас не ваш ход');
                    return;
                }

                // Делаем ход
                board[index] = mySymbol;
                myTurn = false;

                conn.send({
                    type: 'move',
                    index: parseInt(index),
                    symbol: mySymbol
                });

                addLog(`📤 Отправлен ход в клетку ${index}`);

                updateUI();
                checkGameStatus();
            }

            // Проверка победы
            function checkGameStatus() {
                const winPatterns = [
                    [0,1,2], [3,4,5], [6,7,8],
                    [0,3,6], [1,4,7], [2,5,8],
                    [0,4,8], [2,4,6]
                ];

                for (let pattern of winPatterns) {
                    const [a,b,c] = pattern;
                    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
                        gameActive = false;
                        const winner = board[a];
                        if (winner === mySymbol) {
                            turnIndicator.innerText = '🎉 Вы победили!';
                            addLog('🏆 Победа!');
                        } else {
                            turnIndicator.innerText = '😵 Вы проиграли...';
                            addLog('😵 Поражение');
                        }
                        updateUI();
                        return;
                    }
                }

                if (board.every(cell => cell !== '')) {
                    gameActive = false;
                    turnIndicator.innerText = '🤝 Ничья!';
                    addLog('🤝 Ничья');
                } else {
                    updateTurnDisplay();
                }
                
                updateUI();
            }

            // Обновление интерфейса
            function updateUI() {
                renderBoard();
                
                if (!gameActive || !myTurn || !conn || !conn.open) {
                    document.querySelectorAll('.cell').forEach(cell => {
                        if (cell.innerText === '') {
                            cell.style.pointerEvents = 'none';
                            cell.style.opacity = '0.6';
                        }
                    });
                } else {
                    document.querySelectorAll('.cell').forEach(cell => {
                        if (cell.innerText === '') {
                            cell.style.pointerEvents = 'auto';
                            cell.style.opacity = '1';
                        } else {
                            cell.style.pointerEvents = 'none';
                        }
                    });
                }
            }

            // Обновление индикатора хода
            function updateTurnDisplay() {
                if (!gameActive) {
                    if (mySymbol) turnIndicator.innerText = '⏸️ Игра завершена';
                    else turnIndicator.innerText = '⏳ Ожидание игрока...';
                    return;
                }
                
                if (myTurn) {
                    turnIndicator.innerText = mySymbol === 'X' ? '❌ Ваш ход (крестики)' : '⭕ Ваш ход (нолики)';
                } else {
                    turnIndicator.innerText = opponentSymbol === 'X' ? '❌ Ход соперника' : '⭕ Ход соперника';
                }
            }

            // Локальный сброс игры
            function resetGameLocal(sendSignal = true) {
                board = ['', '', '', '', '', '', '', '', ''];
                gameActive = true;
                
                if (mySymbol === 'X') {
                    myTurn = true;
                } else if (mySymbol === 'O') {
                    myTurn = false;
                }
                
                updateUI();
                updateTurnDisplay();

                if (sendSignal && conn && conn.open) {
                    conn.send({ type: 'reset' });
                    addLog('📤 Отправлен сигнал сброса');
                }
            }

            // Оффлайн тест (два игрока в одной вкладке)
            function startOfflineTest() {
                addLog('🧪 Запуск оффлайн теста');
                
                // Создаём второе окно для теста
                const testWindow = window.open('', '_blank');
                if (!testWindow) {
                    alert('Разрешите всплывающие окна для теста');
                    return;
                }
                
                // Генерируем тестовый HTML для второго игрока
                testWindow.document.write(`
                    <html>
                    <head><title>Тестовый игрок 2 (O)</title>
                    <style>
                        body { background: #1a2634; color: white; font-family: monospace; padding: 20px; }
                        .info { background: #2a3f5a; padding: 20px; border-radius: 20px; }
                    </style>
                    </head>
                    <body>
                        <div class="info">
                            <h2>🎮 Тестовый игрок 2 (нолики)</h2>
                            <p>Ваш ID: <b>test_player_2</b></p>
                            <p>Это окно для теста. Играйте здесь вторым игроком.</p>
                            <p>Вернитесь в основное окно и подключитесь к ID: <b>test_player_2</b></p>
                            <button onclick="location.reload()">Обновить</button>
                        </div>
                        <script>
                            // Загружаем PeerJS
                            const peer = new Peer('test_player_2', {
                                host: '0.peerjs.com',
                                port: 443,
                                path: '/',
                                config: { iceServers: [{ urls: 'stun:stun.l.google.com:19302' }] }
                            });
                            
                            peer.on('open', (id) => {
                                document.body.innerHTML += '<p>✅ Peer готов, ждём подключения...</p>';
                            });
                            
                            peer.on('connection', (conn) => {
                                document.body.innerHTML += '<p>🔗 Подключено!</p>';
                                window.conn = conn;
                                
                                // Упрощённая логика для теста
                                conn.on('data', (data) => {
                                    document.body.innerHTML += '<p>Получено: ' + JSON.stringify(data) + '</p>';
                                });
                            });
                        <\/script>
                    </body>
                    </html>
                `);
                
                alert('Оффлайн тест запущен! Во втором окне игрок O. Подключитесь к ID: test_player_2');
            }

            // Инициализация
            initPeer();

            // Обработчики
            connectBtn.addEventListener('click', () => {
                connectToPeer(peerIdInput.value);
            });

            copyIdBtn.addEventListener('click', () => {
                if (myPeerId) {
                    navigator.clipboard.writeText(myPeerId);
                    addLog('📋 ID скопирован: ' + myPeerId);
                    alert('ID скопирован: ' + myPeerId);
                }
            });

            newIdBtn.addEventListener('click', () => {
                initPeer();
            });

            resetBtn.addEventListener('click', () => {
                if (!conn || !conn.open) {
                    alert('Нет соединения');
                    return;
                }
                resetGameLocal(true);
            });

            offlineTestBtn.addEventListener('click', startOfflineTest);

            // Первоначальная отрисовка
            renderBoard();
            updateTurnDisplay();

            // Обработка enter в поле ввода
            peerIdInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    connectBtn.click();
                }
            });
        })();
    </script>
</body>
</html>
