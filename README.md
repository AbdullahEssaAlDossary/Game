<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة X-O حماسية</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            color: #fff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }

        h1 {
            margin-bottom: 10px;
            font-size: 2.5rem;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .status {
            font-size: 1.2rem;
            margin-bottom: 20px;
            background: rgba(255, 255, 255, 0.1);
            padding: 10px 20px;
            border-radius: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 10px;
            background: rgba(255, 255, 255, 0.05);
            padding: 10px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
        }

        .cell {
            background-color: rgba(255, 255, 255, 0.9);
            border: none;
            border-radius: 10px;
            font-size: 2.5rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s ease-in-out;
            color: #333;
        }

        .cell:hover {
            background-color: #fff;
            transform: scale(1.05);
        }

        .cell.X { color: #e74c3c; }
        .cell.O { color: #3498db; }

        .btn-reset {
            margin-top: 25px;
            padding: 12px 30px;
            font-size: 1rem;
            font-weight: bold;
            background-color: #2ecc71;
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(46, 204, 113, 0.4);
            transition: background 0.2s;
        }

        .btn-reset:hover {
            background-color: #27ae60;
        }
    </style>
</head>
<body>

    <h1>لعبة X - O</h1>
    <div class="status" id="statusText">دور اللاعب: X</div>

    <div class="board" id="board">
        <button class="cell" data-index="0"></button>
        <button class="cell" data-index="1"></button>
        <button class="cell" data-index="2"></button>
        <button class="cell" data-index="3"></button>
        <button class="cell" data-index="4"></button>
        <button class="cell" data-index="5"></button>
        <button class="cell" data-index="6"></button>
        <button class="cell" data-index="7"></button>
        <button class="cell" data-index="8"></button>
    </div>

    <button class="btn-reset" onclick="resetGame()">إعادة اللعب</button>

    <script>
        const board = document.getElementById('board');
        const cells = document.querySelectorAll('.cell');
        const statusText = document.getElementById('statusText');
        
        let currentPlayer = "X";
        let gameState = ["", "", "", "", "", "", "", "", ""];
        let isGameActive = true;

        // احتمالات الفوز
        const winningConditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // أفقي
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // عمودي
            [0, 4, 8], [2, 4, 6]             // قطري
        ];

        cells.forEach(cell => cell.addEventListener('click', handleCellClick));

        function handleCellClick(e) {
            const clickedCell = e.target;
            const clickedCellIndex = parseInt(clickedCell.getAttribute('data-index'));

            if (gameState[clickedCellIndex] !== "" || !isGameActive) {
                return;
            }

            updateCell(clickedCell, clickedCellIndex);
            checkResult();
        }

        function updateCell(cell, index) {
            gameState[index] = currentPlayer;
            cell.textContent = currentPlayer;
            cell.classList.add(currentPlayer);
        }

        function checkResult() {
            let roundWon = false;

            for (let i = 0; i < winningConditions.length; i++) {
                const winCondition = winningConditions[i];
                let a = gameState[winCondition[0]];
                let b = gameState[winCondition[1]];
                let c = gameState[winCondition[2]];

                if (a === '' || b === '' || c === '') {
                    continue;
                }
                if (a === b && b === c) {
                    roundWon = true;
                    break;
                }
            }

            if (roundWon) {
                statusText.textContent = `🎉 مبروك! اللاعب ${currentPlayer} هو الفائز!`;
                isGameActive = false;
                return;
            }

            // تحقق من التعادل
            let roundDraw = !gameState.includes("");
            if (roundDraw) {
                statusText.textContent = "تعادل! لعب رائع من الطرفين.";
                isGameActive = false;
                return;
            }

            // تبديل الأدوار
            currentPlayer = currentPlayer === "X" ? "O" : "X";
            statusText.textContent = `دور اللاعب: ${currentPlayer}`;
        }

        function resetGame() {
            currentPlayer = "X";
            gameState = ["", "", "", "", "", "", "", "", ""];
            isGameActive = true;
            statusText.textContent = `دور اللاعب: ${currentPlayer}`;
            cells.forEach(cell => {
                cell.textContent = "";
                cell.classList.remove('X', 'O');
            });
        }
    </script>
</body>
</html>
