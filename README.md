<!DOCTYPE html>
<html>
<head>
    <title>Pong Game</title>
    <style>
        canvas {
            display: block;
            margin: auto;
            background: #000;
        }
    </style>
</head>
<body>
    <canvas id="pongCanvas" width="800" height="400"></canvas>
    <script>
        const canvas = document.getElementById('pongCanvas');
        const ctx = canvas.getContext('2d');

        const paddleWidth = 10, paddleHeight = 100;
        const ballRadius = 10;
        let paddle1Y = canvas.height / 2 - paddleHeight / 2;
        let paddle2Y = canvas.height / 2 - paddleHeight / 2;
        let ballX = canvas.width / 2, ballY = canvas.height / 2;
        let ballDX = 2, ballDY = -2;
        const paddleSpeed = 4;

        function drawPaddle(x, y) {
            ctx.fillStyle = "#FFF";
            ctx.fillRect(x, y, paddleWidth, paddleHeight);
        }

        function drawBall() {
            ctx.beginPath();
            ctx.arc(ballX, ballY, ballRadius, 0, Math.PI*2);
            ctx.fillStyle = "#FFF";
            ctx.fill();
            ctx.closePath();
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawPaddle(0, paddle1Y);
            drawPaddle(canvas.width - paddleWidth, paddle2Y);
            drawBall();

            ballX += ballDX;
            ballY += ballDY;

            if(ballY + ballDY > canvas.height - ballRadius || ballY + ballDY < ballRadius) {
                ballDY = -ballDY;
            }

            if(ballX + ballDX > canvas.width - ballRadius) {
                if(ballY > paddle2Y && ballY < paddle2Y + paddleHeight) {
                    ballDX = -ballDX;
                } else {
                    ballX = canvas.width / 2;
                    ballY = canvas.height / 2;
                }
            } else if(ballX + ballDX < ballRadius) {
                if(ballY > paddle1Y && ballY < paddle1Y + paddleHeight) {
                    ballDX = -ballDX;
                } else {
                    ballX = canvas.width / 2;
                    ballY = canvas.height / 2;
                }
            }

            document.addEventListener('keydown', function(event) {
                if(event.key == 'w' && paddle1Y > 0) paddle1Y -= paddleSpeed;
                if(event.key == 's' && paddle1Y < canvas.height - paddleHeight) paddle1Y += paddleSpeed;
                if(event.key == 'ArrowUp' && paddle2Y > 0) paddle2Y -= paddleSpeed;
                if(event.key == 'ArrowDown' && paddle2Y < canvas.height - paddleHeight) paddle2Y += paddleSpeed;
            });
        }

        setInterval(draw, 10);
    </script>
</body>
</html>
