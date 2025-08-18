<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة تخمين الأرقام ابن رجيب (حمودي)</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            background-color: #f5f5f5;
            padding: 20px;
            margin: 0;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            background-color: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(0,0,0,0.1);
        }
        h1 {
            color: #2c3e50;
            margin-bottom: 30px;
            font-size: 24px;
        }
        .mode-buttons, .difficulty-buttons {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin: 20px 0;
        }
        .btn {
            padding: 15px;
            border: none;
            border-radius: 8px;
            font-size: 18px;
            font-weight: bold;
            color: white;
            cursor: pointer;
            transition: all 0.3s;
        }
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }
        .single { background-color: #3498db; }
        .multi { background-color: #e67e22; }
        .easy { background-color: #27ae60; }
        .medium { background-color: #f39c12; }
        .hard { background-color: #e74c3c; }
        .challenge { background-color: #9b59b6; }
        #gameScreen, #multiplayerSetup, #playerTurnDisplay {
            display: none;
        }
        .game-info {
            background-color: #ecf0f1;
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
            font-weight: bold;
        }
        #guessInput {
            padding: 12px;
            font-size: 16px;
            width: 200px;
            text-align: center;
            border: 2px solid #bdc3c7;
            border-radius: 8px;
        }
        .action-btn {
            padding: 12px 25px;
            margin: 10px;
            border: none;
            border-radius: 8px;
            background-color: #3498db;
            color: white;
            font-weight: bold;
            cursor: pointer;
        }
        #resultMessage {
            min-height: 24px;
            margin: 20px 0;
            font-weight: bold;
            font-size: 18px;
        }
        #backBtn {
            background-color: #7f8c8d;
            margin-top: 20px;
        }
        #timerDisplay {
            color: #9b59b6;
            font-weight: bold;
        }
        .pulse {
            animation: pulse 1s infinite;
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        .player-turn {
            font-size: 20px;
            color: #9b59b6;
            margin: 10px 0;
            font-weight: bold;
        }
        .game-title {
            font-size: 28px;
            color: #e74c3c;
            margin-bottom: 10px;
            font-weight: bold;
        }
        .creator-name {
            font-size: 16px;
            color: #7f8c8d;
            margin-bottom: 20px;
            font-style: italic;
        }
    </style>
</head>
<body>
    <div class="container">
        <div id="mainMenu">
            <div class="game-title">لعبة تخمين الأرقام</div>
            <div class="creator-name">ابن رجيب (حمودي)</div>
            
            <div class="mode-buttons">
                <button class="btn single" onclick="setGameMode('single')">لعبة فردية</button>
                <button class="btn multi" onclick="setGameMode('multi')">لعبة ثنائية</button>
            </div>
            
            <div id="difficultyMenu" style="display: none;">
                <p>اختر مستوى الصعوبة:</p>
                <div class="difficulty-buttons">
                    <button class="btn easy" onclick="setDifficulty('easy')">سهل (1-100)</button>
                    <button class="btn medium" onclick="setDifficulty('medium')">متوسط (1-300)</button>
                    <button class="btn hard" onclick="setDifficulty('hard')">صعب (1-500)</button>
                    <button class="btn challenge" onclick="setDifficulty('challenge')">تحدي سريع (30 ثانية) 🏆 300 نقطة</button>
                </div>
            </div>
            
            <div id="multiplayerSetup" style="display: none;">
                <h3>إعداد اللعبة الثنائية</h3>
                <input type="text" id="player1Name" placeholder="اسم اللاعب الأول" class="player-input">
                <input type="text" id="player2Name" placeholder="اسم اللاعب الثاني" class="player-input">
                <button class="btn" onclick="startMultiplayerGame()">ابدأ اللعب</button>
            </div>
        </div>
        
        <div id="gameScreen">
            <div id="playerTurnDisplay" class="player-turn"></div>
            <div class="game-info" id="scoreDisplay">النقاط: 0</div>
            <div class="game-info" id="attemptsDisplay">المحاولات المتبقية: 10</div>
            <div class="game-info" id="rangeDisplay">المدى: 1 - 100</div>
            <div class="game-info" id="timerDisplay" style="display: none;">الوقت المتبقي: 30 ثانية</div>
            
            <input type="number" id="guessInput" placeholder="ادخل رقمك">
            <div>
                <button class="action-btn" onclick="checkGuess()">تحقق</button>
                <button class="action-btn" onclick="getHint()">مساعدة (-150 نقطة)</button>
            </div>
            
            <div id="resultMessage"></div>
            
            <button id="backBtn" class="btn" onclick="backToMenu()">رجوع للقائمة</button>
        </div>
    </div>

    <script>
        // متغيرات اللعبة
        let secretNumber;
        let guessesLeft;
        let minRange;
        let maxRange;
        let totalScore = 0;
        let difficulty;
        let gameMode;
        let timer;
        let timeLeft;
        let currentPlayer = 1;
        let player1Name = "اللاعب 1";
        let player2Name = "اللاعب 2";
        let player1Score = 0;
        let player2Score = 0;
        
        // عناصر الصفحة
        const mainMenu = document.getElementById("mainMenu");
        const difficultyMenu = document.getElementById("difficultyMenu");
        const multiplayerSetup = document.getElementById("multiplayerSetup");
        const gameScreen = document.getElementById("gameScreen");
        const playerTurnDisplay = document.getElementById("playerTurnDisplay");
        const guessInput = document.getElementById("guessInput");
        const attemptsDisplay = document.getElementById("attemptsDisplay");
        const rangeDisplay = document.getElementById("rangeDisplay");
        const scoreDisplay = document.getElementById("scoreDisplay");
        const timerDisplay = document.getElementById("timerDisplay");
        const resultMessage = document.getElementById("resultMessage");
        
        // تحديد نمط اللعبة
        function setGameMode(mode) {
            gameMode = mode;
            if (mode === 'single') {
                difficultyMenu.style.display = 'block';
                multiplayerSetup.style.display = 'none';
            } else {
                difficultyMenu.style.display = 'none';
                multiplayerSetup.style.display = 'block';
            }
        }
        
        // بدء اللعبة الجماعية
        function startMultiplayerGame() {
            player1Name = document.getElementById("player1Name").value || "اللاعب 1";
            player2Name = document.getElementById("player2Name").value || "اللاعب 2";
            currentPlayer = 1;
            player1Score = 0;
            player2Score = 0;
            
            // استخدام إعدادات المتوسط كلعبة أساسية
            difficulty = 'medium';
            minRange = 1;
            maxRange = 300;
            guessesLeft = 10;
            
            startGame();
        }
        
        // بدء اللعبة حسب المستوى
        function startGame() {
            secretNumber = Math.floor(Math.random() * (maxRange - minRange + 1)) + minRange;
            
            // تحديث الواجهة
            attemptsDisplay.textContent = `المحاولات المتبقية: ${guessesLeft}`;
            rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
            
            if (gameMode === 'multi') {
                playerTurnDisplay.textContent = `دور ${currentPlayer === 1 ? player1Name : player2Name}`;
                scoreDisplay.textContent = `${player1Name}: ${player1Score} | ${player2Name}: ${player2Score}`;
                playerTurnDisplay.style.display = 'block';
            } else {
                scoreDisplay.textContent = `النقاط: ${totalScore}`;
                playerTurnDisplay.style.display = 'none';
            }
            
            resultMessage.textContent = '';
            guessInput.value = '';
            
            // تبديل الشاشات
            mainMenu.style.display = 'none';
            gameScreen.style.display = 'block';
            
            // بدء المؤقت إذا كان وضع التحدي
            if (difficulty === 'challenge') {
                timeLeft = 30;
                timerDisplay.style.display = 'block';
                startTimer();
            } else {
                timerDisplay.style.display = 'none';
            }
            
            // التركيز على حقل الإدخال
            guessInput.focus();
        }
        
        // التحقق من التخمين
        function checkGuess() {
            const userGuess = parseInt(guessInput.value);
            
            if (isNaN(userGuess)) {
                resultMessage.textContent = "الرجاء إدخال رقم صحيح";
                return;
            }
            
            if (userGuess < minRange || userGuess > maxRange) {
                resultMessage.textContent = `الرجاء إدخال رقم بين ${minRange} و ${maxRange}`;
                return;
            }
            
            guessesLeft--;
            attemptsDisplay.textContent = `المحاولات المتبقية: ${guessesLeft}`;
            
            if (userGuess === secretNumber) {
                let pointsWon;
                
                // نظام النقاط
                if (difficulty === 'challenge') {
                    pointsWon = 300; // نقاط ثابتة للتحدي السريع
                    clearInterval(timer);
                } else {
                    switch(difficulty) {
                        case 'easy':
                            pointsWon = 70 - (7 * (10 - guessesLeft));
                            break;
                        case 'medium':
                            pointsWon = 150 - (15 * (10 - guessesLeft));
                            break;
                        case 'hard':
                            pointsWon = 300 - (30 * (7 - guessesLeft));
                            break;
                    }
                }
                
                if (gameMode === 'multi') {
                    if (currentPlayer === 1) {
                        player1Score += pointsWon;
                    } else {
                        player2Score += pointsWon;
                    }
                    resultMessage.textContent = `🎉 ${currentPlayer === 1 ? player1Name : player2Name} فاز! +${pointsWon} نقطة`;
                    scoreDisplay.textContent = `${player1Name}: ${player1Score} | ${player2Name}: ${player2Score}`;
                    
                    // تبديل اللاعب
                    currentPlayer = currentPlayer === 1 ? 2 : 1;
                } else {
                    totalScore += pointsWon;
                    resultMessage.textContent = `🎉 فوز! +${pointsWon} نقطة (الرقم: ${secretNumber})`;
                    scoreDisplay.textContent = `النقاط: ${totalScore}`;
                }
                
                guessInput.value = '';
                
                // بدء جولة جديدة بعد ثانيتين
                setTimeout(() => {
                    secretNumber = Math.floor(Math.random() * (maxRange - minRange + 1)) + minRange;
                    guessesLeft = difficulty === 'hard' ? 7 : 10;
                    attemptsDisplay.textContent = `المحاولات المتبقية: ${guessesLeft}`;
                    rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
                    resultMessage.textContent = '';
                    guessInput.focus();
                    
                    if (difficulty === 'challenge') {
                        timeLeft = 30;
                        startTimer();
                    }
                    
                    if (gameMode === 'multi') {
                        playerTurnDisplay.textContent = `دور ${currentPlayer === 1 ? player1Name : player2Name}`;
                    }
                }, 2000);
            } 
            else {
                guessInput.value = '';
                
                if (userGuess < secretNumber) {
                    resultMessage.textContent = "الرقم أكبر ↑";
                    minRange = userGuess + 1;
                } else {
                    resultMessage.textContent = "الرقم أصغر ↓";
                    maxRange = userGuess - 1;
                }
                
                rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
                
                if (guessesLeft <= 0) {
                    if (gameMode === 'multi') {
                        resultMessage.textContent = `انتهت المحاولات! الرقم كان ${secretNumber}. دور ${currentPlayer === 1 ? player2Name : player1Name}`;
                        currentPlayer = currentPlayer === 1 ? 2 : 1;
                    } else {
                        resultMessage.textContent = `انتهت محاولاتك! الرقم كان ${secretNumber}.`;
                    }
                    
                    // بدء جولة جديدة بعد ثانيتين
                    setTimeout(() => {
                        secretNumber = Math.floor(Math.random() * (maxRange - minRange + 1)) + minRange;
                        guessesLeft = difficulty === 'hard' ? 7 : 10;
                        attemptsDisplay.textContent = `المحاولات المتبقية: ${guessesLeft}`;
                        rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
                        resultMessage.textContent = '';
                        guessInput.focus();
                        
                        if (difficulty === 'challenge') {
                            timeLeft = 30;
                            startTimer();
                        }
                        
                        if (gameMode === 'multi') {
                            playerTurnDisplay.textContent = `دور ${currentPlayer === 1 ? player1Name : player2Name}`;
                        }
                    }, 2000);
                } else if (gameMode === 'multi') {
                    // تبديل اللاعب بعد كل محاولة خاطئة
                    currentPlayer = currentPlayer === 1 ? 2 : 1;
                    playerTurnDisplay.textContent = `دور ${currentPlayer === 1 ? player1Name : player2Name}`;
                }
            }
        }
        
        // نظام المساعدة
        function getHint() {
            if (gameMode === 'multi') {
                resultMessage.textContent = "المساعدة غير متاحة في اللعب الثنائي";
                return;
            }
            
            if (totalScore < 150) {
                resultMessage.textContent = "نقاطك غير كافية للمساعدة (تحتاج 150 نقطة)";
                return;
            }
            
            totalScore -= 150;
            scoreDisplay.textContent = `النقاط: ${totalScore}`;
            
            const hints = [
                `الرقم هو ${secretNumber % 2 === 0 ? 'زوجي' : 'فردي'}`,
                `الرقم بين ${Math.max(minRange, secretNumber-10)} و ${Math.min(maxRange, secretNumber+10)}`,
                `آخر رقم هو ${secretNumber % 10}`,
                `مجموع أرقامه هو ${String(secretNumber).split('').reduce((a,b) => parseInt(a)+parseInt(b))}`
            ];
            
            resultMessage.textContent = hints[Math.floor(Math.random() * hints.length)];
        }
        
        // العودة للقائمة
        function backToMenu() {
            if (difficulty === 'challenge') {
                clearInterval(timer);
            }
            gameScreen.style.display = 'none';
            difficultyMenu.style.display = 'none';
            multiplayerSetup.style.display = 'none';
            mainMenu.style.display = 'block';
        }
        
        // المؤقت للتحدي السريع
        function startTimer() {
            clearInterval(timer);
            timeLeft = 30;
            updateTimerDisplay();
            
            timer = setInterval(() => {
                timeLeft--;
                updateTimerDisplay();
                
                if (timeLeft <= 0) {
                    clearInterval(timer);
                    resultMessage.textContent = "⏰ انتهى الوقت! حاول مرة أخرى";
                    
                    // بدء جولة جديدة بعد ثانيتين
                    setTimeout(() => {
                        secretNumber = Math.floor(Math.random() * (maxRange - minRange + 1)) + minRange;
                        guessesLeft = 10;
                        attemptsDisplay.textContent = `المحاولات المتبقية: ${guessesLeft}`;
                        rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
                        resultMessage.textContent = '';
                        guessInput.focus();
                        timeLeft = 30;
                        startTimer();
                    }, 2000);
                }
            }, 1000);
        }
        
        function updateTimerDisplay() {
            timerDisplay.textContent = `الوقت المتبقي: ${timeLeft} ثانية`;
            
            if (timeLeft < 10) {
                timerDisplay.style.color = '#e74c3c';
                timerDisplay.classList.add('pulse');
            } else {
                timerDisplay.style.color = '#9b59b6';
                timerDisplay.classList.remove('pulse');
            }
        }
        
        // تحديد مستوى الصعوبة
        function setDifficulty(level) {
            difficulty = level;
            
            switch(level) {
                case 'easy':
                    minRange = 1;
                    maxRange = 100;
                    guessesLeft = 10;
                    break;
                case 'medium':
                    minRange = 1;
                    maxRange = 300;
                    guessesLeft = 10;
                    break;
                case 'hard':
                    minRange = 1;
                    maxRange = 500;
                    guessesLeft = 7;
                    break;
                case 'challenge':
                    minRange = 1;
                    maxRange = 100;
                    guessesLeft = 999;
                    break;
            }
            
            startGame();
        }
    </script>
</body>
</html>
