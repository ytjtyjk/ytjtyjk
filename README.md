
 <!DOCTYPE html>
<html>
<head>
    <title>Gioco di Corsa</title>
    <style>
        canvas { background: #e1e1e1; display: block; margin: 0 auto; }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="800" height="400"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        let player = { x: 50, y: 350, width: 50, height: 50, dy: 0 };
        let gravity = 1;
        let jumpPower = -15;
        let isJumping = false;
        let obstacles = [];
        let gameSpeed = 3;

        document.addEventListener('keydown', (e) => {
            if (e.code === 'Space' && !isJumping) {
                player.dy = jumpPower;
                isJumping = true;
            }
        });

        function update() {
            player.dy += gravity;
            player.y += player.dy;
            if (player.y > 350) {
                player.y = 350;
                player.dy = 0;
                isJumping = false;
            }

            if (Math.random() < 0.02) {
                obstacles.push({ x: 800, y: 350, width: 50, height: 50 });
            }

            for (let i = 0; i < obstacles.length; i++) {
                obstacles[i].x -= gameSpeed;

                if (obstacles[i].x + obstacles[i].width < 0) {
                    obstacles.splice(i, 1);
                }

                if (player.x < obstacles[i].x + obstacles[i].width &&
                    player.x + player.width > obstacles[i].x &&
                    player.y < obstacles[i].y + obstacles[i].height &&
                    player.height + player.y > obstacles[i].y) {
                    alert('Game Over');
                    document.location.reload();
                }
            }
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = 'red';
            ctx.fillRect(player.x, player.y, player.width, player.height);

            ctx.fillStyle = 'black';
            for (let i = 0; i < obstacles.length; i++) {
                ctx.fillRect(obstacles[i].x, obstacles[i].y, obstacles[i].width, obstacles[i].height);
            }
        }

        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        gameLoop();
    </script>
</body>
</html>
