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
            opacity: 1 !important;
            visibility: visible !important;
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
        .pro { background-color: #16a085; }
        .math { background-color: #8e44ad; }
        .reverse { background-color: #c0392b; }
        .daily { background-color: #1abc9c; }
        .social { background-color: #3498db; margin: 5px; }
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
        .equation {
            font-size: 24px;
            margin: 20px 0;
            color: #2c3e50;
            font-weight: bold;
        }
        .celebration {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            animation: fadeIn 0.5s;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        .celebration-content {
            background: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            animation: zoomIn 0.5s;
        }
        @keyframes zoomIn {
            from { transform: scale(0.5); }
            to { transform: scale(1); }
        }
        .high-score {
            background-color: #f1c40f;
            color: #2c3e50;
            padding: 10px;
            border-radius: 5px;
            margin-top: 20px;
            font-weight: bold;
        }
        .achievement-popup {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
            z-index: 1001;
            text-align: center;
            animation: bounce 0.5s;
        }
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% {transform: translate(-50%, -50%);}
            40% {transform: translate(-50%, -60%);}
            60% {transform: translate(-50%, -40%);}
        }
        .popup {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
            z-index: 1001;
            max-width: 90%;
            width: 300px;
        }
        .leaderboard-entry {
            display: flex;
            justify-content: space-between;
            padding: 10px;
            border-bottom: 1px solid #eee;
        }
        .leaderboard-entry .rank {
            font-weight: bold;
            color: #e74c3c;
        }
        .leaderboard-entry .avatar {
            font-size: 20px;
        }
        #mainMenu {
            display: block !important;
        }
        .player-input {
            padding: 10px;
            margin: 5px;
            width: 80%;
            border: 1px solid #bdc3c7;
            border-radius: 5px;
        }
        .computer-guess-buttons {
            margin-top: 10px;
            display: flex;
            flex-direction: column;
            gap: 10px;
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
                <button class="btn daily" onclick="startDailyChallenge()">التحدي اليومي 🗓️</button>
            </div>
            
            <div id="difficultyMenu" style="display: none;">
                <p>اختر مستوى الصعوبة:</p>
                <div class="difficulty-buttons">
                    <button class="btn easy" onclick="setDifficulty('easy')">سهل (1-100)</button>
                    <button class="btn medium" onclick="setDifficulty('medium')">متوسط (1-300)</button>
                    <button class="btn hard" onclick="setDifficulty('hard')">صعب (1-500)</button>
                    <button class="btn challenge" onclick="setDifficulty('challenge')">تحدي سريع (30 ثانية) 🏆 300 نقطة</button>
                    <button class="btn pro" onclick="setDifficulty('pro')">محترف (1-1000) 🏅</button>
                    <button class="btn math" onclick="setDifficulty('math')">تحدي رياضي ➗</button>
                    <button class="btn reverse" onclick="setDifficulty('reverse')">وضع المقلوب 🔄</button>
                </div>
            </div>
            
            <div id="multiplayerSetup" style="display: none;">
                <h3>إعداد اللعبة الثنائية</h3>
                <input type="text" id="player1Name" placeholder="اسم اللاعب الأول" class="player-input">
                <input type="text" id="player2Name" placeholder="اسم اللاعب الثاني" class="player-input">
                <button class="btn" onclick="startMultiplayerGame()">ابدأ اللعب</button>
            </div>

            <div id="socialButtons" style="display: none; margin-top: 20px;">
                <button class="btn social" onclick="shareResult()">مشاركة النتيجة</button>
                <button class="btn social" onclick="showFriendsList()">تحدي صديق</button>
                <button class="btn social" onclick="showLeaderboard()">لوحة المتصدرين</button>
            </div>
        </div>
        
        <div id="gameScreen">
            <div id="playerTurnDisplay" class="player-turn"></div>
            <div id="equationDisplay" class="equation" style="display: none;"></div>
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

    <div class="celebration" style="display: none;">
        <div class="celebration-content">
            <h2 style="color: #e74c3c; font-size: 28px;">🎉 فوز رائع! 🎉</h2>
            <p id="celebrationMessage" style="font-size: 20px;"></p>
            <div id="highScoreDisplay" class="high-score" style="display: none;"></div>
            <button class="btn" style="margin-top: 20px; background-color: #2ecc71;" onclick="hideCelebration()">متابعة اللعب</button>
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
        let currentEquation = "";
        let computerGuesses = [];
        let playerSecretNumber = 0;
        let isPlayerThinking = false;
        let highScore = localStorage.getItem('highScore') || 0;
        let soundEnabled = true;

        // التحدي اليومي
        let dailyChallenge = {
            active: false,
            secretNumber: null,
            date: null,
            completed: false,
            bestScore: 0,
            attempts: 0
        };

        // الشارات والإنجازات
        const achievements = {
            beginner: { name: "المبتدئ", desc: "فاز بـ 5 مرات", target: 5, progress: 0, unlocked: false },
            math_wiz: { name: "العبقري الرياضي", desc: "حل 10 معادلات رياضية", target: 10, progress: 0, unlocked: false },
            daily_player: { name: "اللاعب اليومي", desc: "أكمل 7 تحديات يومية", target: 7, progress: 0, unlocked: false },
            speedy: { name: "السريع", desc: "فاز في أقل من 10 ثواني", target: 1, progress: 0, unlocked: false },
            helper: { name: "المساعد", desc: "استخدم المساعدة 20 مرة", target: 20, progress: 0, unlocked: false },
            reverser: { name: "المقلوب", desc: "فاز 5 مرات في الوضع المقلوب", target: 5, progress: 0, unlocked: false },
            challenger: { name: "المتحدي", desc: "فاز بجميع مستويات الصعوبة", target: 7, progress: 0, unlocked: false },
            social: { name: "الاجتماعي", desc: "شارك 10 نتائج", target: 10, progress: 0, unlocked: false }
        };

        // الميزات الاجتماعية
        let socialFeatures = {
            sharedCount: 0,
            friends: [],
            challengesSent: 0,
            challengesReceived: []
        };

        // لوحة المتصدرين
        let leaderboard = [
            { name: "حمودي", score: 2500, avatar: "👑" },
            { name: "أحمد", score: 1800, avatar: "🥈" },
            { name: "سارة", score: 1650, avatar: "🥉" }
        ];

        // عناصر الصفحة
        const mainMenu = document.getElementById("mainMenu");
        const difficultyMenu = document.getElementById("difficultyMenu");
        const multiplayerSetup = document.getElementById("multiplayerSetup");
        const gameScreen = document.getElementById("gameScreen");
        const playerTurnDisplay = document.getElementById("playerTurnDisplay");
        const equationDisplay = document.getElementById("equationDisplay");
        const guessInput = document.getElementById("guessInput");
        const attemptsDisplay = document.getElementById("attemptsDisplay");
        const rangeDisplay = document.getElementById("rangeDisplay");
        const scoreDisplay = document.getElementById("scoreDisplay");
        const timerDisplay = document.getElementById("timerDisplay");
        const resultMessage = document.getElementById("resultMessage");
        const celebrationDiv = document.querySelector('.celebration');
        const celebrationMessage = document.getElementById('celebrationMessage');
        const highScoreDisplay = document.getElementById('highScoreDisplay');

        // إنشاء زر الصوت
        const soundButton = document.createElement('button');
        soundButton.className = 'btn';
        soundButton.textContent = '🔊 الصوت';
        soundButton.style.position = 'fixed';
        soundButton.style.bottom = '20px';
        soundButton.style.left = '20px';
        soundButton.style.zIndex = '1000';
        soundButton.onclick = function() {
            soundEnabled = !soundEnabled;
            this.textContent = soundEnabled ? '🔊 الصوت' : '🔇 كتم';
            playSound('click');
        };
        document.querySelector('.container').appendChild(soundButton);

        // عند تحميل الصفحة
        window.addEventListener('DOMContentLoaded', () => {
            document.getElementById('mainMenu').style.display = 'block';
            document.getElementById('gameScreen').style.display = 'none';
            document.getElementById('difficultyMenu').style.display = 'none';
            document.getElementById('multiplayerSetup').style.display = 'none';
            celebrationDiv.style.display = 'none';
            initSocialFeatures();
            loadAchievements();
        });

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
            playSound('click');
        }

        // بدء اللعبة الجماعية
        function startMultiplayerGame() {
            player1Name = document.getElementById("player1Name").value || "اللاعب 1";
            player2Name = document.getElementById("player2Name").value || "اللاعب 2";
            currentPlayer = 1;
            player1Score = 0;
            player2Score = 0;
            
            difficulty = 'medium';
            minRange = 1;
            maxRange = 300;
            guessesLeft = 10;
            
            startGame();
        }

        // بدء التحدي اليومي
        function startDailyChallenge() {
            const today = new Date().toDateString();
            
            if (dailyChallenge.date === today && dailyChallenge.completed) {
                alert(`لقد أكملت التحدي اليومي بالفعل! 🎉\nأفضل محاولة: ${dailyChallenge.bestScore}`);
                return;
            }
            
            if (dailyChallenge.date !== today) {
                dailyChallenge = {
                    active: true,
                    secretNumber: generateDailyNumber(),
                    date: today,
                    completed: false,
                    bestScore: Infinity,
                    attempts: 0
                };
            }
            
            difficulty = 'daily';
            minRange = 1;
            maxRange = 1000;
            guessesLeft = 7;
            totalScore = 0;
            
            startGame();
        }

        function generateDailyNumber() {
            const today = new Date();
            const seed = today.getFullYear() + today.getMonth() + today.getDate();
            return Math.abs(Math.sin(seed) * 1000) | 0 + 1;
        }

        // بدء اللعبة حسب المستوى
        function startGame() {
            equationDisplay.style.display = 'none';
            isPlayerThinking = false;
            
            if (difficulty === 'math') {
                generateMathEquation();
                equationDisplay.style.display = 'block';
            } else if (difficulty === 'reverse') {
                startReverseMode();
                return;
            } else if (difficulty === 'daily') {
                secretNumber = dailyChallenge.secretNumber;
            } else {
                secretNumber = Math.floor(Math.random() * (maxRange - minRange + 1)) + minRange;
            }
            
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
            
            mainMenu.style.display = 'none';
            gameScreen.style.display = 'block';
            
            if (difficulty === 'challenge') {
                timeLeft = 30;
                timerDisplay.style.display = 'block';
                startTimer();
            } else {
                timerDisplay.style.display = 'none';
            }
            
            guessInput.focus();
        }

        // إنشاء معادلة رياضية لوضع التحدي الرياضي
        function generateMathEquation() {
            const equationTypes = [
                {
                    type: "ضرب",
                    num1: Math.floor(Math.random() * 12) + 1,
                    num2: Math.floor(Math.random() * 12) + 1,
                    answer: function() { return this.num1 * this.num2; },
                    text: function() { return `رقم إذا ضربته في ${this.num1} يكون الناتج ${this.answer()}`; }
                },
                {
                    type: "جمع",
                    num1: Math.floor(Math.random() * 50) + 10,
                    num2: Math.floor(Math.random() * 50) + 10,
                    answer: function() { return this.num1 + this.num2; },
                    text: function() { return `رقم إذا أضفت إليه ${this.num1} يصبح ${this.answer()}`; }
                },
                {
                    type: "طرح",
                    num1: Math.floor(Math.random() * 50) + 50,
                    num2: Math.floor(Math.random() * 40) + 10,
                    answer: function() { return this.num1 - this.num2; },
                    text: function() { return `رقم إذا طرحته من ${this.num1} يكون الناتج ${this.num2}`; }
                },
                {
                    type: "قسمة",
                    num1: Math.floor(Math.random() * 10) + 1,
                    num2: Math.floor(Math.random() * 10) + 1,
                    answer: function() { return this.num1 * this.num2; },
                    text: function() { return `رقم إذا قسمته على ${this.num1} يكون الناتج ${this.num2}`; }
                }
            ];
            
            const selectedEquation = equationTypes[Math.floor(Math.random() * equationTypes.length)];
            secretNumber = selectedEquation.answer();
            currentEquation = selectedEquation.text();
            equationDisplay.textContent = currentEquation;
            guessesLeft = 3;
            attemptsDisplay.textContent = `المحاولات المتبقية: ${guessesLeft}`;
        }

        // بدء وضع المقلوب
        function startReverseMode() {
            isPlayerThinking = true;
            computerGuesses = [];
            minRange = 1;
            maxRange = 100;
            guessesLeft = 7;
            
            attemptsDisplay.textContent = `محاولات الكمبيوتر: ${guessesLeft}`;
            rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
            scoreDisplay.textContent = `النقاط: ${totalScore}`;
            playerTurnDisplay.style.display = 'none';
            
            resultMessage.textContent = "اختر رقمًا سريًا بين 1 و 100 ثم اضغط موافق";
            guessInput.value = '';
            guessInput.placeholder = "ادخل رقمك السري";
            
            mainMenu.style.display = 'none';
            gameScreen.style.display = 'block';
        }

        // تخمين الكمبيوتر في وضع المقلوب
        function computerGuess() {
            if (guessesLeft <= 0) {
                resultMessage.textContent = `فزت! الكمبيوتر لم يخمن رقمك (${playerSecretNumber}) +200 نقطة`;
                totalScore += 200;
                scoreDisplay.textContent = `النقاط: ${totalScore}`;
                updateAchievement('reverser');
                setTimeout(() => {
                    startReverseMode();
                }, 3000);
                return;
            }
            
            let guess;
            do {
                guess = Math.floor(Math.random() * (maxRange - minRange + 1)) + minRange;
            } while (computerGuesses.includes(guess));
            
            computerGuesses.push(guess);
            guessesLeft--;
            attemptsDisplay.textContent = `محاولات الكمبيوتر: ${guessesLeft}`;
            
            resultMessage.textContent = `هل رقمك هو ${guess}؟`;
            
            const buttonsDiv = document.createElement('div');
            buttonsDiv.innerHTML = `
                <button class="action-btn" onclick="handleComputerGuessResponse(${guess}, 'correct')">نعم، هذا رقمي</button>
                <button class="action-btn" onclick="handleComputerGuessResponse(${guess}, 'higher')">رقمي أكبر</button>
                <button class="action-btn" onclick="handleComputerGuessResponse(${guess}, 'lower')">رقمي أصغر</button>
            `;
            
            const oldButtons = document.querySelectorAll('.computer-guess-buttons');
            oldButtons.forEach(btn => btn.remove());
            
            buttonsDiv.className = 'computer-guess-buttons';
            resultMessage.appendChild(buttonsDiv);
        }

        // معالجة إجابة اللاعب على تخمين الكمبيوتر
        function handleComputerGuessResponse(guess, response) {
            const buttonsDiv = document.querySelector('.computer-guess-buttons');
            if (buttonsDiv) buttonsDiv.remove();
            
            if (response === 'correct') {
                resultMessage.textContent = `الكمبيوتر خمن رقمك (${guess}) في ${7 - guessesLeft} محاولات!`;
                totalScore -= (7 - guessesLeft) * 50;
                if (totalScore < 0) totalScore = 0;
                scoreDisplay.textContent = `النقاط: ${totalScore}`;
                
                setTimeout(() => {
                    startReverseMode();
                }, 3000);
            } else {
                if (response === 'higher') {
                    minRange = guess + 1;
                    resultMessage.textContent = `جيد! سأخمن رقمًا أكبر من ${guess}`;
                } else {
                    maxRange = guess - 1;
                    resultMessage.textContent = `جيد! سأخمن رقمًا أصغر من ${guess}`;
                }
                
                rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
                setTimeout(computerGuess, 1500);
            }
        }

        // التحقق من التخمين
        function checkGuess() {
            playSound('click');
            
            if (isPlayerThinking) {
                playerSecretNumber = parseInt(guessInput.value);
                if (isNaN(playerSecretNumber)) {
                    resultMessage.textContent = "الرجاء إدخال رقم صحيح";
                    return;
                }
                
                if (playerSecretNumber < 1 || playerSecretNumber > 100) {
                    resultMessage.textContent = "الرجاء إدخال رقم بين 1 و 100";
                    return;
                }
                
                isPlayerThinking = false;
                guessInput.placeholder = "ادخل رقمك";
                computerGuess();
                return;
            }
            
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
            attemptsDisplay.textContent = difficulty === 'reverse' ? 
                `محاولات الكمبيوتر: ${guessesLeft}` : 
                `المحاولات المتبقية: ${guessesLeft}`;
            
            if (userGuess === secretNumber) {
                let pointsWon;
                
                if (difficulty === 'challenge') {
                    pointsWon = 300;
                    clearInterval(timer);
                } else if (difficulty === 'pro') {
                    pointsWon = 500 - (100 * (5 - guessesLeft));
                } else if (difficulty === 'math') {
                    pointsWon = 200 - (50 * (3 - guessesLeft));
                } else if (difficulty === 'daily') {
                    pointsWon = 500 - (dailyChallenge.attempts * 50);
                    dailyChallenge.completed = true;
                    if (guessesLeft < dailyChallenge.bestScore) {
                        dailyChallenge.bestScore = guessesLeft;
                    }
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
                    
                    currentPlayer = currentPlayer === 1 ? 2 : 1;
                } else {
                    totalScore += pointsWon;
                    const isNewHighScore = checkHighScore();
                    
                    if (difficulty === 'math') {
                        showCelebration(`🎉 فوز! +${pointsWon} نقطة (الجواب: ${secretNumber})`, isNewHighScore);
                    } else if (difficulty === 'daily') {
                        showCelebration(`أكملت التحدي اليومي! 🏆\nالمحاولات: ${dailyChallenge.attempts}\nالمكافأة: ${pointsWon} نقطة`, isNewHighScore);
                        updateAchievement('daily_player');
                        if (dailyChallenge.attempts === 1) {
                            updateAchievement('daily_flawless');
                        }
                    } else {
                        showCelebration(`🎉 فوز! +${pointsWon} نقطة (الرقم: ${secretNumber})`, isNewHighScore);
                    }
                    scoreDisplay.textContent = `النقاط: ${totalScore}`;
                    
                    updateAchievement('beginner');
                    if (difficulty === 'math') updateAchievement('math_wiz');
                    if (difficulty === 'reverse') updateAchievement('reverser');
                }
                
                guessInput.value = '';
                
                setTimeout(() => {
                    if (difficulty === 'math') {
                        generateMathEquation();
                    } else if (difficulty === 'reverse') {
                        startReverseMode();
                    } else if (difficulty === 'daily') {
                        backToMenu();
                    } else {
                        secretNumber = Math.floor(Math.random() * (maxRange - minRange + 1)) + minRange;
                        guessesLeft = difficulty === 'hard' ? 7 : 
                                      difficulty === 'pro' ? 5 : 
                                      difficulty === 'math' ? 3 : 10;
                        attemptsDisplay.textContent = `المحاولات المتبقية: ${guessesLeft}`;
                        rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
                        resultMessage.textContent = '';
                        guessInput.focus();
                    }
                    
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
                playSound('wrong');
                
                if (difficulty === 'daily') {
                    dailyChallenge.attempts++;
                }
                
                if (difficulty !== 'math') {
                    if (userGuess < secretNumber) {
                        resultMessage.textContent = "الرقم أكبر ↑";
                        minRange = userGuess + 1;
                    } else {
                        resultMessage.textContent = "الرقم أصغر ↓";
                        maxRange = userGuess - 1;
                    }
                    
                    rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
                } else {
                    resultMessage.textContent = "إجابة خاطئة! حاول مرة أخرى";
                }
                
                if (guessesLeft <= 0) {
                    if (gameMode === 'multi') {
                        resultMessage.textContent = `انتهت المحاولات! ${difficulty === 'math' ? 'الجواب كان' : 'الرقم كان'} ${secretNumber}. دور ${currentPlayer === 1 ? player2Name : player1Name}`;
                        currentPlayer = currentPlayer === 1 ? 2 : 1;
                    } else {
                        resultMessage.textContent = `انتهت محاولاتك! ${difficulty === 'math' ? 'الجواب كان' : 'الرقم كان'} ${secretNumber}.`;
                    }
                    
                    setTimeout(() => {
                        if (difficulty === 'math') {
                            generateMathEquation();
                        } else if (difficulty === 'reverse') {
                            startReverseMode();
                        } else if (difficulty === 'daily') {
                            backToMenu();
                        } else {
                            secretNumber = Math.floor(Math.random() * (maxRange - minRange + 1)) + minRange;
                            guessesLeft = difficulty === 'hard' ? 7 : 
                                          difficulty === 'pro' ? 5 : 
                                          difficulty === 'math' ? 3 : 10;
                            attemptsDisplay.textContent = `المحاولات المتبقية: ${guessesLeft}`;
                            rangeDisplay.textContent = `المدى: ${minRange} - ${maxRange}`;
                            resultMessage.textContent = '';
                            guessInput.focus();
                        }
                        
                        if (difficulty === 'challenge') {
                            timeLeft = 30;
                            startTimer();
                        }
                        
                        if (gameMode === 'multi') {
                            playerTurnDisplay.textContent = `دور ${currentPlayer === 1 ? player1Name : player2Name}`;
                        }
                    }, 2000);
                } else if (gameMode === 'multi') {
                    currentPlayer = currentPlayer === 1 ? 2 : 1;
                    playerTurnDisplay.textContent = `دور ${currentPlayer === 1 ? player1Name : player2Name}`;
                }
            }
        }

        // نظام المساعدة
        function getHint() {
            playSound('click');
            
            if (gameMode === 'multi') {
                resultMessage.textContent = "المساعدة غير متاحة في اللعب الثنائي";
                return;
            }
            
            if (difficulty === 'reverse') {
                resultMessage.textContent = "المساعدة غير متاحة في وضع المقلوب";
                return;
            }
            
            if (totalScore < 150) {
                resultMessage.textContent = "نقاطك غير كافية للمساعدة (تحتاج 150 نقطة)";
                return;
            }
            
            totalScore -= 150;
            scoreDisplay.textContent = `النقاط: ${totalScore}`;
            updateAchievement('helper');
            
            if (difficulty === 'math') {
                resultMessage.textContent = "تلميح: " + currentEquation.replace("رقم إذا", "حاول أن");
                return;
            }
            
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
            playSound('click');
            if (difficulty === 'challenge' && timer) {
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
                case 'pro':
                    minRange = 1;
                    maxRange = 1000;
                    guessesLeft = 5;
                    break;
                case 'math':
                    minRange = 1;
                    maxRange = 100;
                    guessesLeft = 3;
                    break;
                case 'reverse':
                    minRange = 1;
                    maxRange = 100;
                    guessesLeft = 7;
                    break;
            }
            
            startGame();
        }

        // الميزات الاجتماعية
        function initSocialFeatures() {
            document.getElementById('socialButtons').style.display = 'block';
            checkForChallenges();
        }

        function shareResult() {
            if (navigator.share) {
                navigator.share({
                    title: 'نتيجة لعبة تخمين الأرقام',
                    text: `حصلت على ${totalScore} نقطة في لعبة تخمين الأرقام! هل يمكنك التغلب على نتيجتي؟`,
                    url: window.location.href
                }).then(() => {
                    socialFeatures.sharedCount++;
                    updateAchievement('social');
                    alert('تم المشاركة بنجاح! +50 نقطة');
                    totalScore += 50;
                }).catch(err => {
                    console.log('Error sharing:', err);
                    fallbackShare();
                });
            } else {
                fallbackShare();
            }
        }

        function fallbackShare() {
            const shareUrl = `${window.location.href}?challenge=${totalScore}`;
            prompt('انسخ الرابط لتحدي أصدقائك:', shareUrl);
            socialFeatures.sharedCount++;
            updateAchievement('social');
            totalScore += 50;
        }

        function showFriendsList() {
            const div = document.createElement('div');
            div.className = 'popup';
            div.innerHTML = `
                <h3>تحدي صديق</h3>
                <input type="text" id="friendName" placeholder="اسم الصديق">
                <button class="btn" onclick="sendChallenge(document.getElementById('friendName').value)">إرسال التحدي</button>
                <button onclick="this.parentElement.remove()">إغلاق</button>
            `;
            document.body.appendChild(div);
        }

        function sendChallenge(friend) {
            const challenge = {
                id: Date.now(),
                sender: player1Name || 'اللاعب 1',
                score: totalScore,
                secretNumber: secretNumber,
                date: new Date(),
                accepted: false
            };
            
            alert(`تم إرسال التحدي إلى ${friend}!\nانسخ الرابط: ${window.location.href}?challenge=${challenge.id}`);
            socialFeatures.challengesSent++;
        }

        function checkForChallenges() {
            const params = new URLSearchParams(window.location.search);
            const challengeId = params.get('challenge');
            
            if (challengeId) {
                alert('لديك دعوة تحدي من صديق!');
                socialFeatures.challengesReceived.push({
                    id: challengeId,
                    sender: 'صديقك'
                });
            }
        }

        function showLeaderboard() {
            const div = document.createElement('div');
            div.className = 'popup';
            div.innerHTML = `
                <h2>لوحة المتصدرين 🏆</h2>
                <div id="leaderboardList"></div>
                <button onclick="this.parentElement.remove()">إغلاق</button>
            `;
            
            const list = div.querySelector('#leaderboardList');
            leaderboard.forEach((player, index) => {
                const playerDiv = document.createElement('div');
                playerDiv.className = 'leaderboard-entry';
                playerDiv.innerHTML = `
                    <span class="rank">${index + 1}</span>
                    <span class="avatar">${player.avatar}</span>
                    <span class="name">${player.name}</span>
                    <span class="score">${player.score}</span>
                `;
                list.appendChild(playerDiv);
            });
            
            document.body.appendChild(div);
        }

        // نظام الإنجازات
        function updateAchievement(id, amount = 1) {
            if (achievements[id].unlocked) return;
            
            achievements[id].progress += amount;
            if (achievements[id].progress >= achievements[id].target) {
                achievements[id].unlocked = true;
                showAchievementUnlocked(id);
            }
            
            saveAchievements();
        }

        function showAchievementUnlocked(id) {
            const ach = achievements[id];
            const div = document.createElement('div');
            div.className = 'achievement-popup';
            div.innerHTML = `
                <h3>إنجاز جديد! 🏅</h3>
                <p><strong>${ach.name}</strong></p>
                <p>${ach.desc}</p>
                <button onclick="this.parentElement.remove()">حسناً</button>
            `;
            document.body.appendChild(div);
            playSound('achievement');
        }

        function saveAchievements() {
            localStorage.setItem('achievements', JSON.stringify(achievements));
        }

        function loadAchievements() {
            const saved = localStorage.getItem('achievements');
            if (saved) {
                Object.assign(achievements, JSON.parse(saved));
            }
        }

        // نظام النقاط القياسية
        function checkHighScore() {
            if (totalScore > highScore) {
                highScore = totalScore;
                localStorage.setItem('highScore', highScore);
                return true;
            }
            return false;
        }

        // شاشة الاحتفال
        function showCelebration(message, isNewHighScore = false) {
            celebrationMessage.textContent = message;
            
            if (isNewHighScore) {
                highScoreDisplay.style.display = 'block';
                highScoreDisplay.textContent = `🎖️ رقمك القياسي الجديد: ${highScore}`;
            } else {
                highScoreDisplay.style.display = 'none';
            }
            
            celebrationDiv.style.display = 'flex';
            playSound('win');
        }

        function hideCelebration() {
            celebrationDiv.style.display = 'none';
            playSound('click');
        }

        // المؤثرات الصوتية
        function playSound(type) {
            if (!soundEnabled) return;
            
            const sounds = {
                win: 'https://assets.mixkit.co/sfx/preview/mixkit-winning-chimes-2015.mp3',
                lose: 'https://assets.mixkit.co/sfx/preview/mixkit-retro-arcade-lose-2027.mp3',
                click: 'https://assets.mixkit.co/sfx/preview/mixkit-select-click-1109.mp3',
                correct: 'https://assets.mixkit.co/sfx/preview/mixkit-correct-answer-tone-2870.mp3',
                wrong: 'https://assets.mixkit.co/sfx/preview/mixkit-wrong-answer-fail-notification-946.mp3',
                achievement: 'https://assets.mixkit.co/sfx/preview/mixkit-unlock-game-notification-253.mp3'
            };
            
            const audio = new Audio(sounds[type]);
            audio.play();
        }
    </script>
</body>
</html>