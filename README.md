<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة ذاكرة الأعلام</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            color: white;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        
        .container {
            width: 100%;
            max-width: 900px;
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            text-align: center;
            position: relative;
        }
        
        .total-coins-top {
            position: absolute;
            top: 20px;
            left: 20px;
            background: linear-gradient(135deg, #FFD700, #FFA500);
            color: #8B4513;
            padding: 10px 20px;
            border-radius: 15px;
            font-size: 1.5rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            border: 3px solid #8B4513;
            z-index: 10;
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
            color: #FFD700;
        }
        
        .welcome-text {
            font-size: 1.2rem;
            margin-bottom: 30px;
            line-height: 1.6;
        }
        
        .screen {
            display: none;
            width: 100%;
        }
        
        #main-screen {
            display: block;
        }
        
        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 50px;
            background: linear-gradient(135deg, #FF6B6B, #FF8E8E);
            color: white;
            font-size: 1.2rem;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            display: inline-block;
        }
        
        .btn:hover {
            transform: translateY(-5px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
        }
        
        .btn-play {
            background: linear-gradient(135deg, #00b09b, #96c93d);
            font-size: 1.5rem;
            padding: 18px 40px;
        }
        
        .levels-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin: 30px 0;
        }
        
        .level-btn {
            background: linear-gradient(135deg, #4A90E2, #6A5ACD);
            padding: 20px;
            border-radius: 15px;
            font-size: 1.5rem;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .level-btn:hover {
            transform: scale(1.05);
        }
        
        .game-board {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin: 20px 0;
        }
        
        .card {
            aspect-ratio: 1;
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            cursor: pointer;
            transition: all 0.3s ease;
            transform-style: preserve-3d;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            position: relative;
        }
        
        .card .front, .card .back {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            backface-visibility: hidden;
            border-radius: 10px;
        }
        
        .card .front {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
        }
        
        .card .back {
            background: linear-gradient(135deg, #4A90E2, #6A5ACD);
            transform: rotateY(180deg);
        }
        
        .card.flipped {
            transform: rotateY(180deg);
        }
        
        .card.matched {
            background: linear-gradient(135deg, #00b09b, #96c93d);
            transform: rotateY(180deg) scale(0.95);
            cursor: default;
        }
        
        .game-info {
            display: flex;
            justify-content: space-around;
            background-color: rgba(255, 255, 255, 0.2);
            padding: 15px;
            border-radius: 15px;
            margin: 20px 0;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            border: 2px solid rgba(255, 255, 255, 0.3);
        }
        
        .info-box {
            text-align: center;
            padding: 10px;
            background: linear-gradient(135deg, rgba(74, 144, 226, 0.3), rgba(106, 90, 205, 0.3));
            border-radius: 12px;
            min-width: 120px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .info-label {
            font-size: 1rem;
            margin-bottom: 5px;
            color: #FFD700;
        }
        
        .info-value {
            font-size: 1.5rem;
            font-weight: bold;
        }
        
        .coins-container {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
            margin-top: 5px;
        }
        
        .coin {
            width: 20px;
            height: 20px;
            background: linear-gradient(135deg, #FFD700, #FFA500);
            border-radius: 50%;
            display: inline-block;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        
        .special-bonus {
            background: linear-gradient(135deg, #FFD700, #FFA500);
            color: #8B4513;
            padding: 15px;
            border-radius: 12px;
            margin: 15px 0;
            font-weight: bold;
            display: none;
            animation: pulse 1.5s infinite;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            border: 2px solid #8B4513;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .win-message {
            background: linear-gradient(135deg, #00b09b, #96c93d);
            padding: 30px;
            border-radius: 15px;
            margin: 20px 0;
            display: none;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
            border: 3px solid rgba(255, 255, 255, 0.5);
        }
        
        .win-title {
            font-size: 2.2rem;
            margin-bottom: 15px;
            color: #FFD700;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.3);
        }
        
        .win-coins {
            font-size: 2rem;
            font-weight: bold;
            color: #FFD700;
            margin: 15px 0;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.3);
        }
        
        @media (max-width: 768px) {
            .game-board {
                grid-template-columns: repeat(4, 1fr);
            }
            
            .game-info {
                flex-direction: column;
                gap: 15px;
            }
            
            .info-box {
                min-width: auto;
            }
        }
        
        @media (max-width: 600px) {
            .levels-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .game-board {
                grid-template-columns: repeat(3, 1fr);
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .btn {
                padding: 12px 25px;
                font-size: 1rem;
            }
            
            .btn-play {
                font-size: 1.2rem;
                padding: 15px 30px;
            }
            
            .total-coins-top {
                top: 10px;
                left: 10px;
                font-size: 1.2rem;
                padding: 8px 15px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="total-coins-top">
            <i class="fas fa-coins"></i>
            <span id="total-coins">370</span>
        </div>
        
        <h1>لعبة ذاكرة الأعلام</h1>
        
        <!-- الشاشة الرئيسية -->
        <div id="main-screen" class="screen">
            <p class="welcome-text">اختر مستوى للعب واجمع العملات! اذا وجدت علم البرازيل 🇧🇷 ستحصل على 100 عملة!</p>
            <button class="btn btn-play" onclick="showScreen('levels-screen')">
                ابدأ اللعب <i class="fas fa-play"></i>
            </button>
            <div style="margin-top: 30px;">
                <button class="btn" onclick="showScreen('instructions-screen')">
                    <i class="fas fa-info-circle"></i> كيفية اللعب
                </button>
            </div>
        </div>
        
        <!-- شاشة المستويات -->
        <div id="levels-screen" class="screen">
            <h2>اختر مستوى</h2>
            <div class="levels-grid">
                <div class="level-btn" onclick="startGame(4)">سهل<br>4x4</div>
                <div class="level-btn" onclick="startGame(5)">متوسط<br>5x4</div>
                <div class="level-btn" onclick="startGame(6)">صعب<br>6x5</div>
            </div>
            <button class="btn" onclick="showScreen('main-screen')">
                <i class="fas fa-arrow-right"></i> رجوع
            </button>
        </div>
        
        <!-- شاشة التعليمات -->
        <div id="instructions-screen" class="screen">
            <h2>كيفية اللعب</h2>
            <div style="text-align: right; line-height: 1.8; margin: 20px 0;">
                <p>1. اختر مستوى من مستويات الصعوبة</p>
                <p>2. انقر على البطاقات لقلبها والعثور على الأزواج المتطابقة</p>
                <p>3. عندما تجد زوجًا متطابقًا، ستحصل على 10 عملات</p>
                <p>4. اذا وجدت علم البرازيل 🇧🇷 ستحصل على 100 عملة!</p>
                <p>5. حاول إكمال اللعبة في أقل عدد من المحاولات</p>
            </div>
            <button class="btn" onclick="showScreen('main-screen')">
                <i class="fas fa-arrow-right"></i> رجوع
            </button>
        </div>
        
        <!-- شاشة اللعبة -->
        <div id="game-screen" class="screen">
            <h2>المستوى: <span id="level-name">سهل</span></h2>
            
            <div class="game-info">
                <div class="info-box">
                    <div class="info-label">المحاولات</div>
                    <div class="info-value" id="attempts">0</div>
                </div>
                <div class="info-box">
                    <div class="info-label">الأزواج المتبقية</div>
                    <div class="info-value" id="pairs-left">8</div>
                </div>
                <div class="info-box">
                    <div class="info-label">العملات</div>
                    <div class="info-value" id="game-coins">0</div>
                    <div class="coins-container" id="coins-visual"></div>
                </div>
            </div>
            
            <div class="special-bonus" id="special-bonus">
                <i class="fas fa-gift"></i> مبروك! لقد وجدت علم البرازيل وحصلت على 100 عملة!
            </div>
            
            <div class="game-board" id="game-board"></div>
            
            <div style="display: flex; justify-content: center; gap: 15px; flex-wrap: wrap;">
                <button class="btn" onclick="showScreen('levels-screen')">
                    <i class="fas fa-arrow-right"></i> رجوع للمستويات
                </button>
                <button class="btn" onclick="resetGame()">
                    <i class="fas fa-redo"></i> إعادة اللعب
                </button>
            </div>
            
            <div class="win-message" id="win-message">
                <div class="win-title">مبروك! لقد فزت!</div>
                <div class="win-coins">ربحت <span id="won-coins">0</span> عملة</div>
                <p>عدد المحاولات: <span id="final-attempts">0</span></p>
                <button class="btn" onclick="showScreen('levels-screen')">
                    العب مرة أخرى
                </button>
            </div>
        </div>
    </div>

    <script>
        // المتغيرات العامة
        let totalCoins = 370;
        let gameCoins = 0;
        let attempts = 0;
        let matchedPairs = 0;
        let totalPairs = 0;
        let flippedCards = [];
        let lockBoard = false;
        let currentLevel = 4;
        
        // الأعلام المتاحة للعبة (أعلام عربية وأوروبية)
        const flags = [
            '🇯🇴', // الأردن
            '🇸🇦', // السعودية
            '🇪🇬', // مصر
            '🇦🇪', // الإمارات
            '🇶🇦', // قطر
            '🇰🇼', // الكويت
            '🇧🇭', // البحرين
            '🇹🇳', // تونس
            '🇩🇪', // ألمانيا
            '🇫🇷', // فرنسا
            '🇮🇹', // إيطاليا
            '🇪🇸', // إسبانيا
            '🇬🇧', // بريطانيا
            '🇧🇷'  // البرازيل (المكافأة الكبيرة)
        ];
        
        // عرض شاشة معينة وإخفاء الأخرى
        function showScreen(screenId) {
            document.querySelectorAll('.screen').forEach(screen => {
                screen.style.display = 'none';
            });
            document.getElementById(screenId).style.display = 'block';
        }
        
        // بدء اللعبة بمستوى معين
        function startGame(level) {
            currentLevel = level;
            document.getElementById('level-name').textContent = 
                level === 4 ? 'سهل' : level === 5 ? 'متوسط' : 'صعب';
            
            attempts = 0;
            gameCoins = 0;
            matchedPairs = 0;
            flippedCards = [];
            lockBoard = false;
            
            // حساب عدد الأزواج
            totalPairs = (level * (level === 4 ? 4 : level === 5 ? 4 : 5)) / 2;
            document.getElementById('pairs-left').textContent = totalPairs;
            document.getElementById('attempts').textContent = attempts;
            document.getElementById('game-coins').textContent = gameCoins;
            updateCoinsVisual();
            document.getElementById('special-bonus').style.display = 'none';
            document.getElementById('win-message').style.display = 'none';
            
            // إنشاء البطاقات
            createGameBoard(level);
            
            // الانتقال إلى شاشة اللعبة
            showScreen('game-screen');
        }
        
        // إنشاء لوحة اللعبة
        function createGameBoard(level) {
            const gameBoard = document.getElementById('game-board');
            gameBoard.innerHTML = '';
            
            // تحديد عدد الأعمدة بناءً على المستوى
            const columns = level === 4 ? 4 : level === 5 ? 5 : 5;
            const rows = level === 4 ? 4 : level === 5 ? 4 : 6;
            gameBoard.style.gridTemplateColumns = `repeat(${columns}, 1fr)`;
            
            // عدد البطاقات الإجمالي
            totalPairs = (columns * rows) / 2;
            
            // إنشاء مصفوفة البطاقات
            let cards = [];
            
            // اختيار أعلام عشوائية بدون تكرار
            const selectedFlags = selectRandomFlags(totalPairs);
            
            // إضافة أزواج البطاقات
            for (let i = 0; i < totalPairs; i++) {
                const flag = selectedFlags[i];
                cards.push(flag, flag);
            }
            
            // خلط البطاقات
            cards = shuffleArray(cards);
            
            // إنشاء البطاقات في لوحة اللعبة
            cards.forEach((flag, index) => {
                const card = document.createElement('div');
                card.classList.add('card');
                card.dataset.flag = flag;
                card.dataset.index = index;
                
                // إنشاء وجهي البطاقة
                const front = document.createElement('div');
                front.classList.add('front');
                front.innerHTML = '<i class="fas fa-flag" style="font-size: 2rem;"></i>';
                
                const back = document.createElement('div');
                back.classList.add('back');
                back.innerHTML = flag;
                
                card.appendChild(front);
                card.appendChild(back);
                
                card.addEventListener('click', flipCard);
                gameBoard.appendChild(card);
            });
        }
        
        // اختيار أعلام عشوائية بدون تكرار
        function selectRandomFlags(count) {
            const shuffled = [...flags].sort(() => 0.5 - Math.random());
            return shuffled.slice(0, count);
        }
        
        // تحديث العرض البصري للعملات
        function updateCoinsVisual() {
            const coinsContainer = document.getElementById('coins-visual');
            coinsContainer.innerHTML = '';
            
            const coinsCount = Math.min(gameCoins, 20); // عرض حتى 20 عملة كحد أقصى
            
            for (let i = 0; i < coinsCount; i++) {
                const coin = document.createElement('div');
                coin.classList.add('coin');
                coinsContainer.appendChild(coin);
            }
            
            if (gameCoins > 20) {
                const moreCoins = document.createElement('span');
                moreCoins.textContent = `+${gameCoins - 20}`;
                moreCoins.style.marginLeft = '5px';
                coinsContainer.appendChild(moreCoins);
            }
        }
        
        // خلط البطاقات
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }
        
        // قلب البطاقة
        function flipCard() {
            if (lockBoard) return;
            if (this.classList.contains('flipped') || this.classList.contains('matched')) return;
            
            this.classList.add('flipped');
            flippedCards.push(this);
            
            if (flippedCards.length === 2) {
                lockBoard = true;
                attempts++;
                document.getElementById('attempts').textContent = attempts;
                
                setTimeout(checkMatch, 500);
            }
        }
        
        // التحقق من تطابق البطاقات
        function checkMatch() {
            const [card1, card2] = flippedCards;
            const isMatch = card1.dataset.flag === card2.dataset.flag;
            
            if (isMatch) {
                card1.classList.add('matched');
                card2.classList.add('matched');
                
                // منح العملات
                let coinsEarned = 10;
                if (card1.dataset.flag === '🇧🇷') {
                    coinsEarned = 100;
                    document.getElementById('special-bonus').style.display = 'block';
                }
                
                gameCoins += coinsEarned;
                totalCoins += coinsEarned;
                
                document.getElementById('game-coins').textContent = gameCoins;
                document.getElementById('total-coins').textContent = totalCoins;
                updateCoinsVisual();
                
                matchedPairs++;
                document.getElementById('pairs-left').textContent = totalPairs - matchedPairs;
                
                // التحقق من الفوز
                if (matchedPairs === totalPairs) {
                    setTimeout(showWinMessage, 500);
                }
            } else {
                setTimeout(() => {
                    card1.classList.remove('flipped');
                    card2.classList.remove('flipped');
                }, 500);
            }
            
            flippedCards = [];
            lockBoard = false;
        }
        
        // عرض رسالة الفوز
        function showWinMessage() {
            document.getElementById('won-coins').textContent = gameCoins;
            document.getElementById('final-attempts').textContent = attempts;
            document.getElementById('win-message').style.display = 'block';
        }
        
        // إعادة اللعبة
        function resetGame() {
            startGame(currentLevel);
        }
        
        // تهيئة الصفحة عند التحميل
        document.addEventListener('DOMContentLoaded', function() {
            showScreen('main-screen');
            document.getElementById('total-coins').textContent = totalCoins;
        });
    </script>
</body>
</html>