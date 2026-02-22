<!DOCTYPE html>
<html>
<head>
    <title>❌ Крестики-нолики по QR</title>
    <script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background: #f0f0f0;
        }
        .game {
            text-align: center;
            background: white;
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        .board {
            display: grid;
            grid-template-columns: repeat(3, 80px);
            gap: 5px;
            margin: 20px auto;
            justify-content: center;
        }
        .cell {
            width: 80px;
            height: 80px;
            border: 2px solid #333;
            font-size: 2.5em;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            background: #fff;
        }
        #qrcode {
            margin: 20px auto;
            padding: 20px;
            background: white;
            display: inline-block;
        }
        .game-link {
            margin: 20px 0;
            word-break: break-all;
        }
    </style>
</head>
<body>
    <div class="game">
        <h2>❌ Крестики-нолики по QR ⭕</h2>
        
        <div class="board" id="board"></div>
        <div id="status">Ваша очередь: X</div>
        
        <div id="qrcode"></div>
        <div class="game-link" id="gameLink"></div>
        <button onclick="copyLink()">Копировать ссылку</button>
        <button onclick="newGame()">Новая игра</button>
    </div>

    <script>
        let board = ['', '', '', '', '', '', '', '', ''];
        let currentPlayer = 'X';
        
        function createBoard() {
            const boardElement = document.getElementById('board');
            boardElement.innerHTML = '';
            for (let i = 0; i < 9; i++) {
                const cell = document.createElement('div');
                cell.className = 'cell';
                cell.onclick = () => makeMove(i);
                boardElement.appendChild(cell);
            }
        }
        
        function updateBoard() {
            const cells = document.querySelectorAll('.cell');
            cells.forEach((cell, index) => {
                cell.textContent = board[index];
            });
            document.getElementById('status').textContent = `Ходит: ${currentPlayer}`;
        }
        
        function makeMove(index) {
            if (board[index] !== '') return;
            board[index] = currentPlayer;
            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            updateBoard();
            saveToURL();
        }
        
        function saveToURL() {
            const gameData = btoa(JSON.stringify({
                board: board,
                player: currentPlayer
            }));
            const url = `${window.location.href.split('?')[0]}?game=${gameData}`;
            
            // Обновляем QR код
            document.getElementById('qrcode').innerHTML = '';
            new QRCode(document.getElementById('qrcode'), url);
            document.getElementById('gameLink').innerHTML = `<a href="${url}" target="_blank">Ссылка на игру</a>`;
        }
        
        function loadFromURL() {
            const params = new URLSearchParams(window.location.search);
            const gameData = params.get('game');
            if (gameData) {
                try {
                    const data = JSON.parse(atob(gameData));
                    board = data.board;
                    currentPlayer = data.player;
                    updateBoard();
                } catch (e) {
                    console.log('Не удалось загрузить игру');
                }
            }
        }
        
        function copyLink() {
            navigator.clipboard.writeText(window.location.href);
            alert('Ссылка скопирована!');
        }
        
        function newGame() {
            board = ['', '', '', '', '', '', '', '', ''];
            currentPlayer = 'X';
            updateBoard();
            saveToURL();
        }
        
        createBoard();
        loadFromURL();
        saveToURL();
    </script>
</body>
</html>
