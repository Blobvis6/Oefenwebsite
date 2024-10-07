<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spellen Pagina</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background-color: #00ff00;
        }
        canvas {
            border: 2px solid black;
            margin-bottom: 20px;
        }
        table {
            border-collapse: collapse;
        }
        td {
            width: 60px;
            height: 60px;
            text-align: center;
            vertical-align: middle;
            font-size: 24px;
            border: 1px solid black;
        }
        button {
            background-color: blue;
            color: white;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h2>Welkom bij het Auto Spel!</h2>
    <p>Gebruik de pijltjestoetsen om de auto te besturen en vermijd obstakels.</p>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <button id="resetButton" style="background-color: blue; color: white;">Reset Spel</button>

    <div id="ticTacToe">
        <h2>Tic Tac Toe</h2>
        <table>
            <tr>
                <td onclick="makeMove(this)"></td>
                <td onclick="makeMove(this)"></td>
                <td onclick="makeMove(this)"></td>
            </tr>
            <tr>
                <td onclick="makeMove(this)"></td>
                <td onclick="makeMove(this)"></td>
                <td onclick="makeMove(this)"></td>
            </tr>
            <tr>
                <td onclick="makeMove(this)"></td>
                <td onclick="makeMove(this)"></td>
                <td onclick="makeMove(this)"></td>
            </tr>
        </table>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        let car = { x: 180, y: 350, width: 40, height: 80 };
        let obstacles = [];
        let speed = 2;
        let gameOver = false;

        document.getElementById('resetButton').addEventListener('click', resetGame);
        document.addEventListener('keydown', changeDirection);

        function changeDirection(event) {
            if (event.key === 'ArrowLeft' && car.x > 0) {
                car.x -= 20;
            } else if (event.key === 'ArrowRight' && car.x < canvas.width - car.width) {
                car.x += 20;
            }
        }

        function drawCar() {
            ctx.fillStyle = 'blue';
            ctx.fillRect(car.x, car.y, car.width, car.height);
            const carImg = new Image();
            carImg.src = 'https://example.com/blue_car.png';
            carImg.onload = function() {
                ctx.drawImage(carImg, car.x, car.y, car.width, car.height);
            };
        }

        function drawObstacles() {
            ctx.fillStyle = 'red';
            obstacles.forEach(obstacle => {
                ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
                obstacle.y += speed;
            });
        }

        function updateObstacles() {
            if (Math.random() < 0.02) {
                let width = Math.floor(Math.random() * 100) + 50;
                let x = Math.floor(Math.random() * (canvas.width - width));
                obstacles.push({ x: x, y: 0, width: width, height: 20 });
            }

            obstacles = obstacles.filter(obstacle => obstacle.y < canvas.height);
        }

        function checkCollision() {
            for (let obstacle of obstacles) {
                if (
                    car.x < obstacle.x + obstacle.width &&
                    car.x + car.width > obstacle.x &&
                    car.y < obstacle.y + obstacle.height &&
                    car.y + car.height > obstacle.y
                ) {
                    gameOver = true;
                    alert('Game Over');
                    clearInterval(game);
                }
            }
        }

        function draw() {
            if (gameOver) return;
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawCar();
            drawObstacles();
            updateObstacles();
            checkCollision();
        }

        const game = setInterval(draw, 100);

        function resetGame() {
            car = { x: 180, y: 350, width: 40, height: 80 };
            obstacles = [];
            gameOver = false;
            clearInterval(game);
            setInterval(draw, 100);
        }

        let currentPlayer = 'X';

        function makeMove(cell) {
            if (cell.innerHTML === '') {
                cell.innerHTML = currentPlayer;
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            }
        }
    </script>
</body>
</html>
