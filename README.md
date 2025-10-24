<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة كرة الطائرة</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(to bottom, #1a2a6c, #b21f1f, #fdbb2d);
            color: white;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            width: 100%;
            text-align: center;
        }

        header {
            margin-bottom: 20px;
        }

        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        .game-info {
            background-color: rgba(0, 0, 0, 0.5);
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .score-board {
            display: flex;
            justify-content: space-around;
            margin-bottom: 20px;
        }

        .team {
            background-color: rgba(255, 255, 255, 0.2);
            padding: 15px;
            border-radius: 10px;
            width: 45%;
        }

        .team h2 {
            margin-bottom: 10px;
        }

        .score {
            font-size: 3rem;
            font-weight: bold;
        }

        .game-area {
            position: relative;
            width: 100%;
            height: 400px;
            background: linear-gradient(to bottom, #1e9600, #fff200, #ff0000);
            border: 5px solid white;
            border-radius: 10px;
            overflow: hidden;
            margin-bottom: 20px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
        }

        .net {
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 10px;
            height: 100%;
            background-color: white;
            z-index: 1;
        }

        .net::before {
            content: '';
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 100px;
            height: 100%;
            border-left: 5px dashed white;
            border-right: 5px dashed white;
        }

        .player {
            position: absolute;
            width: 60px;
            height: 60px;
            background-color: #3498db;
            border-radius: 50%;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 2;
            transition: all 0.1s;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
        }

        .player::after {
            content: '';
            position: absolute;
            top: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 30px;
            height: 30px;
            background-color: rgba(255, 255, 255, 0.3);
            border-radius: 50%;
        }

        .ball {
            position: absolute;
            width: 40px;
            height: 40px;
            background: radial-gradient(circle at 10px 10px, #ff9d00, #ff0000);
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 3;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 20px;
        }

        button {
            padding: 12px 25px;
            font-size: 1.2rem;
            background-color: #2ecc71;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
        }

        button:hover {
            background-color: #27ae60;
        }

        button:active {
            transform: translateY(2px);
            box-shadow: 0 2px 3px rgba(0, 0, 0, 0.3);
        }

        #resetBtn {
            background-color: #e74c3c;
        }

        #resetBtn:hover {
            background-color: #c0392b;
        }

        .instructions {
            background-color: rgba(0, 0, 0, 0.5);
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            text-align: right;
        }

        .instructions h3 {
            margin-bottom: 10px;
        }

        .instructions ul {
            list-style-type: none;
            padding-right: 20px;
        }

        .instructions li {
            margin-bottom: 8px;
            position: relative;
        }

        .instructions li::before {
            content: "•";
            color: #2ecc71;
            font-weight: bold;
            position: absolute;
            right: -15px;
        }

        .message {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.8);
            padding: 20px;
            border-radius: 10px;
            font-size: 1.5rem;
            z-index: 10;
            display: none;
        }

        @media (max-width: 600px) {
            .game-area {
                height: 300px;
            }
            
            .player {
                width: 50px;
                height: 50px;
            }
            
            .ball {
                width: 30px;
                height: 30px;
            }
            
            h1 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>لعبة كرة الطائرة</h1>
            <div class="game-info">
                <p>استخدم مفاتيح الأسهم للتحكم في اللاعب وصد الكرة!</p>
            </div>
        </header>

        <div class="score-board">
            <div class="team">
                <h2>فريقك</h2>
                <div class="score" id="playerScore">0</div>
            </div>
            <div class="team">
                <h2>الخصم</h2>
                <div class="score" id="opponentScore">0</div>
            </div>
        </div>

        <div class="game-area">
            <div class="net"></div>
            <div class="player" id="player"></div>
            <div class="ball" id="ball"></div>
            <div class="message" id="message"></div>
        </div>

        <div class="controls">
            <button id="startBtn">بدء اللعبة</button>
            <button id="resetBtn">إعادة التعيين</button>
        </div>

        <div class="instructions">
            <h3>كيفية اللعب:</h3>
            <ul>
                <li>استخدم مفتاح السهم الأيسر للتحرك لليسار</li>
                <li>استخدم مفتاح السهم الأيمن للتحرك لليمين</li>
                <li>استخدم مفتاح السهم الأعلى للقفز</li>
                <li>اضغط على الكرة لترتد إلى منطقة الخصم</li>
                <li>احرص على عدم سقوط الكرة في منطقتك</li>
            </ul>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const player = document.getElementById('player');
            const ball = document.getElementById('ball');
            const playerScoreElement = document.getElementById('playerScore');
            const opponentScoreElement = document.getElementById('opponentScore');
            const startBtn = document.getElementById('startBtn');
            const resetBtn = document.getElementById('resetBtn');
            const message = document.getElementById('message');
            
            let playerScore = 0;
            let opponentScore = 0;
            let gameActive = false;
            let ballSpeedX = 5;
            let ballSpeedY = 5;
            let playerX = 0;
            let playerY = 0;
            let ballX = 0;
            let ballY = 0;
            let keys = {};
            let gameAreaWidth = document.querySelector('.game-area').offsetWidth;
            let gameAreaHeight = document.querySelector('.game-area').offsetHeight;
            let animationId;
            
            // تهيئة اللعبة
            function initGame() {
                playerScore = 0;
                opponentScore = 0;
                playerScoreElement.textContent = playerScore;
                opponentScoreElement.textContent = opponentScore;
                
                resetPositions();
                gameActive = true;
                startBtn.textContent = 'إيقاف اللعبة';
                message.style.display = 'none';
                
                // بدء حركة الكرة
                if (animationId) {
                    cancelAnimationFrame(animationId);
                }
                gameLoop();
            }
            
            // إعادة تعيين المواضع
            function resetPositions() {
                playerX = (gameAreaWidth - player.offsetWidth) / 2;
                playerY = gameAreaHeight - player.offsetHeight - 20;
                
                ballX = (gameAreaWidth - ball.offsetWidth) / 2;
                ballY = (gameAreaHeight - ball.offsetHeight) / 2;
                
                updatePlayerPosition();
                updateBallPosition();
            }
            
            // تحديث موضع اللاعب
            function updatePlayerPosition() {
                player.style.left = playerX + 'px';
                player.style.bottom = playerY + 'px';
            }
            
            // تحديث موضع الكرة
            function updateBallPosition() {
                ball.style.left = ballX + 'px';
                ball.style.top = ballY + 'px';
            }
            
            // التحكم في الحركة
            document.addEventListener('keydown', function(e) {
                keys[e.key] = true;
            });
            
            document.addEventListener('keyup', function(e) {
                keys[e.key] = false;
            });
            
            // معالجة حركة اللاعب
            function handlePlayerMovement() {
                const playerSpeed = 8;
                
                if (keys['ArrowRight'] && playerX < gameAreaWidth - player.offsetWidth) {
                    playerX += playerSpeed;
                }
                
                if (keys['ArrowLeft'] && playerX > 0) {
                    playerX -= playerSpeed;
                }
                
                if (keys['ArrowUp'] && playerY === gameAreaHeight - player.offsetHeight - 20) {
                    // القفز ممكن فقط عندما يكون اللاعب على الأرض
                    playerY -= 100;
                    setTimeout(() => {
                        playerY = gameAreaHeight - player.offsetHeight - 20;
                        updatePlayerPosition();
                    }, 500);
                }
                
                updatePlayerPosition();
            }
            
            // كشف التصادم
            function checkCollision() {
                const playerRect = player.getBoundingClientRect();
                const ballRect = ball.getBoundingClientRect();
                
                // كشف تصادم اللاعب مع الكرة
                if (
                    playerRect.left < ballRect.right &&
                    playerRect.right > ballRect.left &&
                    playerRect.top < ballRect.bottom &&
                    playerRect.bottom > ballRect.top
                ) {
                    // عكس اتجاه الكرة عند التصادم
                    ballSpeedY = -Math.abs(ballSpeedY);
                    
                    // تغيير الاتجاه الأفقي بناءً على مكان الاصطدام باللاعب
                    const hitPosition = (ballRect.left + ballRect.width / 2) - (playerRect.left + playerRect.width / 2);
                    ballSpeedX = hitPosition / 10;
                }
                
                // كشف تصادم الكرة مع الجدران
                if (ballX <= 0 || ballX >= gameAreaWidth - ball.offsetWidth) {
                    ballSpeedX = -ballSpeedX;
                }
                
                // كشف تصادم الكرة مع الشبكة
                const netX = gameAreaWidth / 2;
                if (
                    (ballX + ball.offsetWidth / 2 > netX - 5 && ballX + ball.offsetWidth / 2 < netX + 5) &&
                    (ballY <= gameAreaHeight / 2)
                ) {
                    ballSpeedX = -ballSpeedX;
                }
                
                // كشف سقوط الكرة
                if (ballY >= gameAreaHeight - ball.offsetHeight) {
                    // سقوط الكرة في منطقة اللاعب
                    opponentScore++;
                    opponentScoreElement.textContent = opponentScore;
                    showMessage('نقطة للخصم!');
                    resetBall();
                } else if (ballY <= 0) {
                    // سقوط الكرة في منطقة الخصم
                    playerScore++;
                    playerScoreElement.textContent = playerScore;
                    showMessage('نقطة لك!');
                    resetBall();
                }
            }
            
            // إعادة تعيين الكرة
            function resetBall() {
                ballX = (gameAreaWidth - ball.offsetWidth) / 2;
                ballY = (gameAreaHeight - ball.offsetHeight) / 2;
                
                // اتجاه عشوائي للكرة
                ballSpeedX = (Math.random() > 0.5 ? 1 : -1) * 5;
                ballSpeedY = 5;
                
                updateBallPosition();
                
                // إيقاف مؤقت قبل متابعة اللعبة
                gameActive = false;
                setTimeout(() => {
                    gameActive = true;
                    message.style.display = 'none';
                }, 1500);
            }
            
            // عرض رسالة
            function showMessage(text) {
                message.textContent = text;
                message.style.display = 'block';
            }
            
            // حركة الكرة
            function moveBall() {
                if (!gameActive) return;
                
                ballX += ballSpeedX;
                ballY += ballSpeedY;
                
                // تأثير الجاذبية
                if (ballY < gameAreaHeight - ball.offsetHeight) {
                    ballSpeedY += 0.1;
                }
                
                updateBallPosition();
            }
            
            // حلقة اللعبة الرئيسية
            function gameLoop() {
                handlePlayerMovement();
                moveBall();
                checkCollision();
                
                animationId = requestAnimationFrame(gameLoop);
            }
            
            // أحداث الأزرار
            startBtn.addEventListener('click', function() {
                if (!gameActive) {
                    initGame();
                } else {
                    gameActive = false;
                    startBtn.textContent = 'استئناف اللعبة';
                    if (animationId) {
                        cancelAnimationFrame(animationId);
                    }
                }
            });
            
            resetBtn.addEventListener('click', function() {
                gameActive = false;
                if (animationId) {
                    cancelAnimationFrame(animationId);
                }
                initGame();
            });
            
            // تهيئة اللعبة عند تحميل الصفحة
            window.addEventListener('load', function() {
                gameAreaWidth = document.querySelector('.game-area').offsetWidth;
                gameAreaHeight = document.querySelector('.game-area').offsetHeight;
                resetPositions();
            });
            
            // تحديث أبعاد منطقة اللعبة عند تغيير حجم النافذة
            window.addEventListener('resize', function() {
                gameAreaWidth = document.querySelector('.game-area').offsetWidth;
                gameAreaHeight = document.querySelector('.game-area').offsetHeight;
                resetPositions();
            });
        });
    </script>
</body>
</html>
