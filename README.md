<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pirate Maze Adventure</title>
    <style>
        body {
            text-align: center;
            background-color: #001f3f; /* Dark Blue */
            color: white;
            font-family: Arial, sans-serif;
        }
        canvas {
            border: 2px solid white;
            background-color: transparent;
        }
        #popup {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.9);
            color: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            font-size: 18px;
            border: 2px solid white;
        }
        #popup button {
            margin-top: 10px;
            padding: 10px 20px;
            border: none;
            background: white;
            color: black;
            font-weight: bold;
            cursor: pointer;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <h1>Pirate Maze Adventure</h1>
    <p>Use W, A, S, D or Arrow keys to move the ship!</p>
    <canvas id="gameCanvas" width="600" height="600"></canvas>

    <div id="popup">
        <p>🏴‍☠️ Congratulations! You've found the treasure! 🎉</p>
        <p><strong>Upside down, find what’s hidden under Base62 - Shift, flip</strong></p>
        <p><strong>1qckzAOm5MbkOWkeQiBFlAgXZ6tciQEZcsI</strong></p>
        <button onclick="closePopup()">OK</button>
    </div>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const tileSize = 40;
        const rows = 15, cols = 15; // Larger maze
        const shipImg = new Image();
        shipImg.src = "Ship.png";  // Provide ship image path
        const treasureImg = new Image();
        treasureImg.src = "Treasure.png";  // Use uploaded image

        let maze = [
            "111111111111111",
            "100000001000001",
            "101110101011101",
            "101000101000101",
            "101011111011101",
            "100000000000001",
            "101110111110101",
            "100010000010001",
            "111010111010111",
            "100010000010001",
            "101011101110101",
            "101000001000101",
            "101011101011101",
            "100000000000001",
            "111111111111111"
        ];

        let player = { x: 1, y: 1 };
        let goal = { x: 13, y: 13 };

        function drawMaze() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let y = 0; y < rows; y++) {
                for (let x = 0; x < cols; x++) {
                    if (maze[y][x] === "1") {
                        ctx.fillStyle = "white"; // White maze walls
                        ctx.fillRect(x * tileSize, y * tileSize, tileSize, tileSize);
                    }
                }
            }
            ctx.drawImage(shipImg, player.x * tileSize, player.y * tileSize, tileSize, tileSize);
            ctx.drawImage(treasureImg, goal.x * tileSize, goal.y * tileSize, tileSize, tileSize);
        }

        function movePlayer(dx, dy) {
            let newX = player.x + dx;
            let newY = player.y + dy;
            if (maze[newY][newX] === "0") {
                player.x = newX;
                player.y = newY;
                drawMaze();
                checkWin();
            }
        }

        function checkWin() {
            if (player.x === goal.x && player.y === goal.y) {
                document.getElementById("popup").style.display = "block";
            }
        }

        function closePopup() {
            document.getElementById("popup").style.display = "none";
        }

        document.addEventListener("keydown", function(event) {
            const key = event.key.toLowerCase();
            if (key === "arrowup" || key === "w") movePlayer(0, -1);
            if (key === "arrowdown" || key === "s") movePlayer(0, 1);
            if (key === "arrowleft" || key === "a") movePlayer(-1, 0);
            if (key === "arrowright" || key === "d") movePlayer(1, 0);
        });

        shipImg.onload = treasureImg.onload = drawMaze;

        // ⛔ Block Right Click
        document.addEventListener("contextmenu", function(event) {
            event.preventDefault();
        });

        // ⛔ Block Dev Tools Shortcuts
        document.addEventListener("keydown", function(event) {
            if (event.ctrlKey && (event.key === "u" || event.key === "U")) {
                event.preventDefault();
            }
            if (
                event.ctrlKey && event.shiftKey && (event.key === "i" || event.key === "I") || 
                event.key === "F12"
            ) {
                event.preventDefault();
            }
        });
    </script>
</body>
</html>
