# sake-game
贪吃蛇小游戏<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>贪吃蛇小游戏</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: #111;
      color: white;
      font-family: Arial, sans-serif;
    }
    canvas {
      background: #222;
      border: 2px solid #fff;
    }
    #score {
      position: absolute;
      top: 20px;
      font-size: 24px;
    }
  </style>
</head>
<body>
  <div id="score">得分: 0</div>
  <canvas id="game" width="400" height="400"></canvas>
  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");
    const box = 20; // 格子大小
    let snake = [{x: 9 * box, y: 10 * box}];
    let direction;
    let food = {
      x: Math.floor(Math.random() * 19 + 1) * box,
      y: Math.floor(Math.random() * 19 + 1) * box
    };
    let score = 0;

    document.addEventListener("keydown", dir);

    function dir(event) {
      if (event.keyCode == 37 && direction != "RIGHT") direction = "LEFT";
      else if (event.keyCode == 38 && direction != "DOWN") direction = "UP";
      else if (event.keyCode == 39 && direction != "LEFT") direction = "RIGHT";
      else if (event.keyCode == 40 && direction != "UP") direction = "DOWN";
    }

    function draw() {
      ctx.fillStyle = "#222";
      ctx.fillRect(0, 0, 400, 400);

      for (let i = 0; i < snake.length; i++) {
        ctx.fillStyle = (i == 0) ? "lime" : "green";
        ctx.fillRect(snake[i].x, snake[i].y, box, box);
      }

      ctx.fillStyle = "red";
      ctx.fillRect(food.x, food.y, box, box);

      let snakeX = snake[0].x;
      let snakeY = snake[0].y;

      if (direction == "LEFT") snakeX -= box;
      if (direction == "UP") snakeY -= box;
      if (direction == "RIGHT") snakeX += box;
      if (direction == "DOWN") snakeY += box;

      // 吃到食物
      if (snakeX == food.x && snakeY == food.y) {
        score++;
        document.getElementById("score").innerText = "得分: " + score;
        food = {
          x: Math.floor(Math.random() * 19 + 1) * box,
          y: Math.floor(Math.random() * 19 + 1) * box
        };
      } else {
        snake.pop();
      }

      let newHead = {x: snakeX, y: snakeY};

      // 撞到边界或自己
      if (
        snakeX < 0 || snakeY < 0 ||
        snakeX >= canvas.width || snakeY >= canvas.height ||
        collision(newHead, snake)
      ) {
        clearInterval(game);
        alert("游戏结束！得分: " + score);
      }

      snake.unshift(newHead);
    }

    function collision(head, array) {
      for (let i = 0; i < array.length; i++) {
        if (head.x == array[i].x && head.y == array[i].y) {
          return true;
        }
      }
      return false;
    }

    let game = setInterval(draw, 100);
  </script>
</body>
</html>
