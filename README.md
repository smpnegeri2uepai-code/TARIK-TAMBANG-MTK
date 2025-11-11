<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tarik Tambang Matematika - Balapan Cepat</title>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* CSS (Styling) */
        :root {
            --player1-color: #d9534f; /* Merah */
            --player2-color: #0275d8; /* Biru */
            --light-bg: #f0f8ff;
            --white: #ffffff;
            --dark-text: #333;
            --grey: #ccc;
            --robot-skin: #fbe0c3; 
            --robot-metal: #a0a0a0; 
            --robot-hair: #333333; 
        }

        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background-color: var(--light-bg);
            color: var(--dark-text);
            text-align: center;
            margin: 0;
            padding: 20px; 
            box-sizing: border-box;
            position: relative; 
        }
        
        @keyframes jiggle {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-2px); }
        }

        /* Tombol Fullscreen (BARU) */
        #fullscreen-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            z-index: 1000; /* Pastikan berada di atas semua elemen */
            background: #f0ad4e;
            color: white;
            border: none;
            padding: 10px 15px;
            font-size: 1.2em;
            border-radius: 8px;
            cursor: pointer;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            transition: background-color 0.2s;
        }

        #fullscreen-btn:hover {
            background-color: #e09b3d;
        }


        /* Judul dan Heading */
        .main-title {
            color: #0056b3;
            margin-bottom: 20px;
            font-size: 2.5em; 
            font-weight: 800; 
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
        }

        h1 {
            color: #0056b3;
            margin-bottom: 20px;
            font-size: 2em; 
        }

        /* 1. LAYAR PEMILIHAN MODE */
        #selection-screen {
            width: 100%;
            max-width: 700px; 
            padding: 20px;
            background: var(--white);
            border-radius: 12px;
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
        }

        .mode-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); 
            gap: 15px;
            margin-top: 20px;
        }

        .mode-btn {
            padding: 20px;
            font-size: 1.1em;
            font-weight: bold;
            color: var(--white);
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            background-color: #007bff;
        }
        
        /* Warna Tombol */
        .mode-btn[data-mode="tambah"] { background-color: #28a745; }
        .mode-btn[data-mode="kurang"] { background-color: #dc3545; }
        .mode-btn[data-mode="kali"] { background-color: #ffc107; color: #333;}
        .mode-btn[data-mode="bagi"] { background-color: #17a2b8; }
        .mode-btn[data-mode="pangkat"] { background-color: #6f42c1; }
        .mode-btn[data-mode="akar"] { background-color: #fd7e14; }
        .mode-btn[data-mode="kpk"] { background-color: #20c997; }
        .mode-btn[data-mode="fpb"] { background-color: #e83e8c; }
        .mode-btn[data-mode="persen"] { background-color: #008080; }
        .mode-btn[data-mode="campur"] { background-color: #343a40; }
        .mode-btn[data-mode="desimal"] { background-color: #9c27b0; } 
        .mode-btn[data-mode="pecahan"] { background-color: #e91e63; } 
        
        .mode-btn.disabled {
            background-color: var(--grey);
            cursor: not-allowed;
            opacity: 0.7;
        }

        .mode-btn:not(.disabled):hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
        }

        /* 2. LAYAR GAME */
        #game-screen {
            width: 100%;
            max-width: 1000px; 
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .game-ui {
            display: flex; 
            justify-content: space-between; 
            align-items: flex-start; 
            width: 100%;
            margin-top: 20px;
            flex-wrap: nowrap; 
        }

        /* Panel Pemain (Keypad Area) */
        .player-panel {
            width: 300px;
            min-width: 280px;
            padding: 20px;
            background: var(--white);
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            transition: box-shadow 0.3s, transform 0.3s;
        }

        .player-panel.player1 { border: 4px solid var(--player1-color); }
        .player-panel.player2 { border: 4px solid var(--player2-color); }

        .player-panel h3 {
            margin-top: 0;
            font-size: 1.8em;
        }
        .player-panel.player1 h3 { color: var(--player1-color); }
        .player-panel.player2 h3 { color: var(--player2-color); }

        /* Tampilan Jawaban */
        .answer-display {
            width: 100%;
            height: 50px;
            line-height: 50px;
            font-size: 1.8em;
            font-weight: bold;
            background: #f4f4f4;
            border: 2px solid var(--grey);
            border-radius: 8px;
            margin-bottom: 20px;
            padding: 0 10px;
            box-sizing: border-box;
            text-align: right;
            color: #555;
            min-height: 54px;
            background: #fff;
        }

        /* Keypad */
        .keypad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
        }

        .keypad button {
            padding: 18px 0;
            font-size: 1.6em;
            font-weight: bold;
            border: none;
            border-radius: 8px;
            background: #f0f0f0;
            cursor: pointer;
            transition: background-color 0.2s, transform 0.1s;
        }
        .keypad button:hover:not(:disabled) {
            background-color: #e0e0e0;
        }
        .keypad button:active:not(:disabled) {
            transform: scale(0.95);
        }

        .keypad button.btn-clear { background-color: var(--player1-color); color: white; }
        .keypad button.btn-submit { background-color: var(--player2-color); color: white; }
        .keypad button.btn-dot { background-color: #343a40; color: white; } 
        
        .keypad button:disabled {
            background-color: #f9f9f9;
            color: #aaa;
            cursor: not-allowed;
            opacity: 0.7;
        }

        /* Area Tengah (Soal & Tali) */
        .game-world {
            flex-grow: 1; 
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            min-width: 300px;
            margin: 0 20px; 
        }

        #question-display {
            font-size: 2.5em; 
            font-weight: bold;
            color: #004a99;
            padding: 15px 30px;
            background: var(--white);
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            margin-bottom: 30px;
            min-height: 1.2em;
        }

        /* Visual Tarik Tambang */
        .tug-of-war {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 100%;
            min-height: 100px;
        }

        .rope-area {
            width: 200px;
            height: 10px;
            background: #8B4513;
            position: relative;
            margin: 0 -10px;
        }

        #marker {
            width: 10px;
            height: 40px;
            background: #f0ad4e;
            border: 2px solid #a0522d;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            transition: left 0.5s ease-in-out;
            z-index: 2;
        }
        
        /* Karakter Robot */
        .player-char {
            width: 60px; 
            height: 100px; 
            position: relative; 
            margin: 0 15px; 
            animation: jiggle 0.5s infinite alternate; 
            padding-bottom: 20px; 
            box-sizing: content-box;
        }

        .player1-char { transform: scaleX(1); } 
        .player2-char { transform: scaleX(-1); } 

        .player-char .head {
            width: 45px; 
            height: 40px; 
            background: var(--robot-skin); 
            border-radius: 5px;
            position: absolute; 
            left: 50%; 
            top: 0;
            transform: translateX(-50%); 
            z-index: 3; 
            border: 2px solid var(--dark-text);
        }
        
        .player-char .hair {
            width: 49px; 
            height: 15px; 
            background: var(--robot-hair); 
            border-radius: 5px 5px 0 0;
            position: absolute; 
            top: -2px;
            left: 50%; 
            transform: translateX(-50%); 
            z-index: 4;
            border: 2px solid var(--dark-text);
            border-bottom: none;
        }

        .player-char .headband {
            width: 49px; 
            height: 10px; 
            background: var(--player1-color); 
            position: absolute; 
            top: 10px;
            left: 50%; 
            transform: translateX(-50%); 
            z-index: 5; 
            border: 1px solid var(--dark-text);
        }
        .player-char .headband::after {
            content: ''; 
            width: 100%; 
            height: 3px; 
            background: var(--white); 
            position: absolute;
            top: 50%; 
            left: 0; 
            transform: translateY(-50%);
        }
        
        .player-char .eyes {
            position: absolute;
            top: 18px;
            width: 100%;
            display: flex;
            justify-content: space-around;
            padding: 0 8px;
            box-sizing: border-box;
            z-index: 6;
        }
        .player-char .eyes .eye {
            width: 5px;
            height: 5px;
            background: var(--dark-text);
            border-radius: 50%;
        }
        .player-char .mouth {
            width: 15px;
            height: 3px;
            background: var(--dark-text);
            position: absolute;
            top: 28px;
            left: 50%;
            transform: translateX(-50%);
            border-radius: 2px;
            z-index: 6;
        }

        .player-char .body {
            width: 45px; 
            height: 40px; 
            position: absolute; 
            top: 40px;
            left: 50%; 
            transform: translateX(-50%); 
            border: 2px solid var(--dark-text); 
            border-top: none;
            border-radius: 0 0 5px 5px;
            z-index: 1;
        }
        .player1-char .body { background: var(--player1-color); }
        .player2-char .body { background: var(--player2-color); }
        
        .player-char .arm {
            position: absolute;
            width: 8px;
            height: 30px;
            background: var(--robot-metal);
            border: 1px solid var(--dark-text);
            z-index: 0;
            top: 45px;
        }
        .player-char .arm.left {
            left: 10px; 
            transform: rotate(20deg);
        }
        .player-char .arm.right {
            right: 10px; 
            transform: rotate(-20deg);
        }
        
        .player-char .leg {
            position: absolute;
            width: 10px;
            height: 20px;
            background: var(--robot-metal);
            border: 2px solid var(--dark-text);
            border-radius: 0 0 2px 2px;
            bottom: 0;
            z-index: 0;
        }
        .player-char .leg.left { left: 15px; }
        .player-char .leg.right { right: 15px; }


        /* Area Pesan dan Tombol */
        #message-display {
            margin-top: 25px;
            font-size: 1.5em;
            font-weight: bold;
            min-height: 1.5em;
        }
        .message-correct { color: #4caf50; }
        .message-wrong { color: #d9534f; }
        .message-win { color: #f0ad4e; font-size: 2.5em; } 

        .game-controls { margin-top: 15px; }

        #restart-btn, #back-btn {
            font-size: 1.1em;
            padding: 10px 20px;
            background-color: #0275d8;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
        }
        #restart-btn { display: none; background-color: #5cb85c; }
        
        /* Creator Text (Paling Bawah) */
        .creator-text {
            position: absolute;
            bottom: 10px; 
            font-size: 0.9em;
            color: #777;
            display: flex; 
            align-items: center; 
            gap: 10px; 
        }

        .creator-text span {
            display: flex; 
            align-items: center;
            gap: 5px; 
        }

        .creator-text .fab, .creator-text .fas {
            font-size: 1.2em; 
        }

        .hidden {
            display: none !important;
        }

        /* Media Query untuk Layar Kecil (Ponsel/Tablet Vertikal) */
        @media (max-width: 900px) {
            .game-ui {
                flex-direction: column; 
                align-items: center;
                gap: 20px;
            }
            .game-world {
                order: -1; 
                margin-bottom: 20px;
                margin-left: 0;
                margin-right: 0;
            }
            #selection-screen {
                 max-width: 500px;
            }
            .mode-grid {
                grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); 
            }
            .player-panel {
                width: 90%; 
                max-width: 350px; 
            }
            #fullscreen-btn {
                top: 10px;
                right: 10px;
                padding: 8px 12px;
                font-size: 1em;
            }
        }
    </style>
</head>
<body>

    <button id="fullscreen-btn" onclick="toggleFullscreen()">
        <i class="fas fa-expand"></i> Fullscreen
    </button>

    <div id="selection-screen">
        <div class="main-title">Tarik Tambang Matematika (Cepat Tangkas)</div>
        
        <h2>Pilih Mode Permainan</h2>
        <div class="mode-grid">
            <button class="mode-btn" data-mode="tambah">Tambah</button>
            <button class="mode-btn" data-mode="kurang">Kurang</button>
            <button class="mode-btn" data-mode="kali">Kali</button>
            <button class="mode-btn" data-mode="bagi">Bagi</button>
            
            <button class="mode-btn" data-mode="pangkat">Pangkat</button>
            <button class="mode-btn" data-mode="akar">Akar</button>
            <button class="mode-btn" data-mode="persen">Persen</button>
            <button class="mode-btn" data-mode="kpk">KPK</button>
            <button class="mode-btn" data-mode="fpb">FPB</button>
            
            <button class="mode-btn" data-mode="campur">Campur</button>
            
            <button class="mode-btn" data-mode="desimal">Desimal</button>
            <button class="mode-btn" data-mode="pecahan">Pecahan</button>
        </div>
    </div>

    <div id="game-screen" class="hidden">
        <h1>Tarik Tambang Matematika</h1>

        <div class="game-ui">
            <div class="player-panel player1" id="panel-p1">
                <h3>Player 1</h3>
                <div class="answer-display" id="p1-answer-display"></div>
                <div class="keypad" id="keypad-p1">
                    <button data-key="1">1</button>
                    <button data-key="2">2</button>
                    <button data-key="3">3</button>
                    <button data-key="4">4</button>
                    <button data-key="5">5</button>
                    <button data-key="6">6</button>
                    <button data-key="7">7</button>
                    <button data-key="8">8</button>
                    <button data-key="9">9</button>
                    <button data-key="clear" class="btn-clear">C</button>
                    <button data-key="0">0</button>
                    <button data-key="dot" class="btn-dot">.</button> 
                    <button data-key="submit" class="btn-submit">Go</button>
                </div>
            </div>
            
            <div class="game-world">
                <div id="question-display">Bersiap...</div>
                <div class="tug-of-war">
                    
                    <div class="player-char player1-char">
                        <div class="hair"></div>
                        <div class="head">
                            <div class="headband"></div>
                            <div class="eyes">
                                <div class="eye"></div>
                                <div class="eye"></div>
                            </div>
                            <div class="mouth"></div>
                        </div>
                        <div class="body"></div>
                        <div class="arm left"></div>
                        <div class="arm right"></div>
                        <div class="leg left"></div>
                        <div class="leg right"></div>
                    </div>
                    <div class="rope-area">
                        <div id="marker"></div>
                    </div>
                    
                    <div class="player-char player2-char">
                        <div class="hair"></div>
                        <div class="head">
                            <div class="headband"></div>
                            <div class="eyes">
                                <div class="eye"></div>
                                <div class="eye"></div>
                            </div>
                            <div class="mouth"></div>
                        </div>
                        <div class="body"></div>
                        <div class="arm left"></div>
                        <div class="arm right"></div>
                        <div class="leg left"></div>
                        <div class="leg right"></div>
                    </div>
                </div>
                <div id="message-display"></div>
                <div class="game-controls">
                    <button id="restart-btn">Mulai Lagi</button>
                    <button id="back-btn">Pilih Mode</button>
                </div>
            </div>

            <div class="player-panel player2" id="panel-p2">
                <h3>Player 2</h3>
                <div class="answer-display" id="p2-answer-display"></div>
                <div class="keypad" id="keypad-p2">
                    <button data-key="1">1</button>
                    <button data-key="2">2</button>
                    <button data-key="3">3</button>
                    <button data-key="4">4</button>
                    <button data-key="5">5</button>
                    <button data-key="6">6</button>
                    <button data-key="7">7</button>
                    <button data-key="8">8</button>
                    <button data-key="9">9</button>
                    <button data-key="clear" class="btn-clear">C</button>
                    <button data-key="0">0</button>
                    <button data-key="dot" class="btn-dot">.</button>
                    <button data-key="submit" class="btn-submit">Go</button>
                </div>
            </div>
        </div>
    </div>

    <div class="creator-text">
        <span><i class="fab fa-instagram"></i> aes_435</span>
        <span><i class="fab fa-tiktok"></i> aes435_</span>
        <span><i class="fab fa-facebook"></i> Ayu Elma Sari</span>
    </div>

    <script>
        // JAVASCRIPT (Logika Game)

        // 1. Ambil Elemen DOM
        const selectionScreen = document.getElementById('selection-screen');
        const gameScreen = document.getElementById('game-screen');
        const modeGrid = document.querySelector('.mode-grid');
        
        const marker = document.getElementById('marker');
        const questionDisplay = document.getElementById('question-display');
        const messageDisplay = document.getElementById('message-display');
        
        const panelP1 = document.getElementById('panel-p1');
        const panelP2 = document.getElementById('panel-p2');
        const keypadP1 = document.getElementById('keypad-p1');
        const keypadP2 = document.getElementById('keypad-p2');
        const answerDisplayP1 = document.getElementById('p1-answer-display');
        const answerDisplayP2 = document.getElementById('p2-answer-display');
        
        const restartBtn = document.getElementById('restart-btn');
        const backBtn = document.getElementById('back-btn');
        const fullscreenBtn = document.getElementById('fullscreen-btn'); // Ambil tombol fullscreen

        // 2. Variabel Status Game
        const winPosition = 5; 
        const allModes = ['tambah', 'kurang', 'kali', 'bagi', 'pangkat', 'akar', 'kpk', 'fpb', 'persen', 'desimal', 'pecahan']; 
        let currentMode = 'tambah';
        let markerPosition = 0;
        let currentCorrectAnswer = 0;
        let gameActive = false; 
        let playerAnswers = { p1: '', p2: '' };

        // 3. Fungsi Bantu untuk KPK dan FPB
        function gcd(a, b) {
            while (b) {
                [a, b] = [b, a % b];
            }
            return a;
        }

        function lcm(a, b) {
            return (a * b) / gcd(a, b);
        }
        
        function formatFraction(numerator, denominator) {
            return `${numerator}/${denominator}`;
        }
        
        function createDecimal(maxInt, maxDecimalPlaces) {
            const integerPart = Math.floor(Math.random() * maxInt) + 1;
            const decimalPlaces = Math.floor(Math.random() * maxDecimalPlaces) + 1;
            let decimalPart = Math.floor(Math.random() * (10 ** decimalPlaces));
            
            const powerOfTen = 10 ** decimalPlaces;
            const decimalValue = decimalPart / powerOfTen;
            
            return integerPart + decimalValue;
        }

        // 4. Fungsi Utama
        
        function showScreen(screenName) {
            selectionScreen.classList.add('hidden');
            gameScreen.classList.add('hidden');
            
            if (screenName === 'selection') {
                selectionScreen.classList.remove('hidden');
            } else if (screenName === 'game') {
                gameScreen.classList.remove('hidden');
            }
        }

        function initializeGame(mode) {
            currentMode = mode;
            showScreen('game');
            startGame();
        }

        function startGame() {
            markerPosition = 0;
            gameActive = false; 
            playerAnswers.p1 = '';
            playerAnswers.p2 = '';
            
            updateMarkerPosition();
            updateAnswerDisplays();
            
            restartBtn.style.display = 'none';
            backBtn.style.display = 'inline-block';

            startCountdown(); 
        }

        function startCountdown() {
            let count = 3;
            
            questionDisplay.textContent = 'Bersiap...';
            messageDisplay.textContent = '';
            deactivateGameControls();
            
            const countdownInterval = setInterval(() => {
                if (count > 0) {
                    messageDisplay.textContent = count;
                    messageDisplay.className = 'message-win'; 
                    count--;
                } else if (count === 0) {
                    messageDisplay.textContent = 'Mulai!';
                    count--;
                } else {
                    clearInterval(countdownInterval);
                    startRound(); 
                }
            }, 1000); 
        }
        
        function startRound() {
            generateQuestion();
            gameActive = true;
            activateGameControls(); 
            messageDisplay.textContent = 'Siapa Cepat... Jawab!';
            messageDisplay.className = '';
        }

        function generateQuestion() {
            let num1, num2, decimalNum1, decimalNum2;
            let questionText = '';
            let modeToUse = currentMode;

            if (currentMode === 'campur') {
                const randomIndex = Math.floor(Math.random() * allModes.length);
                modeToUse = allModes[randomIndex];
            }
            
            currentCorrectAnswer = 0;

            switch (modeToUse) {
                // Mode Lama
                case 'tambah':
                    num1 = Math.floor(Math.random() * 25) + 1;
                    num2 = Math.floor(Math.random() * 25) + 1;
                    currentCorrectAnswer = num1 + num2;
                    questionText = `${num1} + ${num2} = ?`;
                    break;
                case 'kurang':
                    num1 = Math.floor(Math.random() * 25) + 1;
                    num2 = Math.floor(Math.random() * 25) + 1;
                    if (num1 < num2) [num1, num2] = [num2, num1];
                    currentCorrectAnswer = num1 - num2;
                    questionText = `${num1} - ${num2} = ?`;
                    break;
                case 'kali':
                    num1 = Math.floor(Math.random() * 10) + 1;
                    num2 = Math.floor(Math.random() * 8) + 2; 
                    currentCorrectAnswer = num1 * num2;
                    questionText = `${num1} × ${num2} = ?`;
                    break;
                case 'bagi':
                    currentCorrectAnswer = Math.floor(Math.random() * 5) + 2;
                    num2 = Math.floor(Math.random() * 5) + 2;
                    num1 = currentCorrectAnswer * num2;
                    questionText = `${num1} ÷ ${num2} = ?`;
                    break;
                case 'pangkat':
                    num1 = Math.floor(Math.random() * 4) + 2; 
                    num2 = Math.floor(Math.random() * 2) + 2; 
                    currentCorrectAnswer = Math.pow(num1, num2);
                    questionText = `${num1} pangkat ${num2} = ?`;
                    break;
                case 'akar':
                    num1 = Math.floor(Math.random() * 9) + 2; 
                    const perfectSquare = num1 * num1;
                    currentCorrectAnswer = num1;
                    questionText = `Akar Kuadrat dari ${perfectSquare} = ?`;
                    break;
                case 'kpk':
                    num1 = Math.floor(Math.random() * 5) + 3; 
                    num2 = Math.floor(Math.random() * 5) + 3; 
                    currentCorrectAnswer = lcm(num1, num2);
                    questionText = `KPK dari ${num1} dan ${num2} = ?`;
                    break;
                case 'fpb':
                    num1 = Math.floor(Math.random() * 8) + 10; 
                    num2 = Math.floor(Math.random() * 8) + 10; 
                    if (num1 < num2) [num1, num2] = [num2, num1];
                    currentCorrectAnswer = gcd(num1, num2);
                    questionText = `FPB dari ${num1} dan ${num2} = ?`;
                    break;
                case 'persen':
                    const easyPercents = [10, 20, 25, 50];
                    const percent = easyPercents[Math.floor(Math.random() * easyPercents.length)];
                    num1 = Math.floor(Math.random() * 5) + 2; 
                    num2 = num1 * 10; 
                    currentCorrectAnswer = (percent / 100) * num2;
                    questionText = `${percent}% dari ${num2} = ?`;
                    break;
                    
                // MODE DESIMAL
                case 'desimal':
                    decimalNum1 = createDecimal(5, 2); 
                    decimalNum2 = createDecimal(5, 2); 
                    
                    const operators = ['+', '-', '×'];
                    const op = operators[Math.floor(Math.random() * operators.length)];

                    let result;
                    if (op === '+') {
                        result = decimalNum1 + decimalNum2;
                    } else if (op === '-') {
                        if (decimalNum1 < decimalNum2) { [decimalNum1, decimalNum2] = [decimalNum2, decimalNum1]; }
                        result = decimalNum1 - decimalNum2;
                    } else if (op === '×') {
                        decimalNum1 = createDecimal(3, 1);
                        decimalNum2 = createDecimal(2, 1);
                        result = decimalNum1 * decimalNum2;
                    }

                    currentCorrectAnswer = parseFloat(result.toFixed(2));
                    questionText = `${parseFloat(decimalNum1.toFixed(2))} ${op} ${parseFloat(decimalNum2.toFixed(2))} = ?`;
                    break;

                // MODE PECAHAN
                case 'pecahan':
                    let n1 = Math.floor(Math.random() * 5) + 1; 
                    let d1 = Math.floor(Math.random() * 5) + 2; 
                    let n2 = Math.floor(Math.random() * 5) + 1; 
                    let d2 = Math.floor(Math.random() * 5) + 2; 
                    
                    const fracOperators = ['+', '-'];
                    const fracOp = fracOperators[Math.floor(Math.random() * fracOperators.length)];

                    const commonDenominator = lcm(d1, d2);
                    const newN1 = n1 * (commonDenominator / d1);
                    const newN2 = n2 * (commonDenominator / d2);
                    
                    let finalNumerator;
                    
                    if (fracOp === '+') {
                        finalNumerator = newN1 + newN2;
                    } else { 
                        if (newN1 < newN2) { [newN1, newN2] = [newN2, newN1]; [n1, n2] = [n2, n1]; [d1, d2] = [d2, d1]; }
                        finalNumerator = newN1 - newN2;
                    }
                    
                    const commonDivisor = gcd(finalNumerator, commonDenominator);
                    const simplifiedNumerator = finalNumerator / commonDivisor;
                    const simplifiedDenominator = commonDenominator / commonDivisor;

                    currentCorrectAnswer = simplifiedNumerator / simplifiedDenominator;

                    const frac1 = formatFraction(n1, d1);
                    const frac2 = formatFraction(n2, d2);
                    
                    questionText = `${frac1} ${fracOp} ${frac2} = ? (Tulis Desimal)`;
                    break;
            }
            
            questionDisplay.textContent = questionText;
        }

        function activateGameControls() {
            panelP1.classList.remove('active-panel'); 
            panelP2.classList.remove('active-panel');
            panelP1.style.opacity = 1;
            panelP2.style.opacity = 1;
            
            keypadP1.querySelectorAll('button').forEach(btn => btn.disabled = false);
            keypadP2.querySelectorAll('button').forEach(btn => btn.disabled = false);
        }
        
        function deactivateGameControls() {
            keypadP1.querySelectorAll('button').forEach(btn => btn.disabled = true);
            keypadP2.querySelectorAll('button').forEach(btn => btn.disabled = true);
        }

        function updateMarkerPosition() {
            const totalSteps = winPosition * 2;
            const percentage = ((markerPosition + winPosition) / totalSteps) * 100;
            marker.style.left = `${percentage}%`;
        }
        
        function updateAnswerDisplays() {
            answerDisplayP1.textContent = playerAnswers.p1;
            answerDisplayP2.textContent = playerAnswers.p2;
        }

        function handleKeypadInput(key, playerNum) {
            if (!gameActive) return;

            let currentAnswer = playerAnswers[`p${playerNum}`];

            if (key === 'clear') {
                currentAnswer = '';
            } else if (key === 'submit') {
                if (currentAnswer !== '') {
                    checkAnswer(playerNum, parseFloat(currentAnswer)); 
                }
                return; 
            } else if (key === 'dot') {
                if (!currentAnswer.includes('.')) {
                    currentAnswer += '.';
                }
            } else {
                if (currentAnswer.length < 6) {
                    currentAnswer += key;
                }
            }

            playerAnswers[`p${playerNum}`] = currentAnswer;
            updateAnswerDisplays();
        }

        function checkAnswer(playerNum, playerAnswer) {
            if (!gameActive) return;

            gameActive = false;
            deactivateGameControls(); 

            const tolerance = 0.001; 
            const isCorrect = Math.abs(playerAnswer - currentCorrectAnswer) < tolerance;

            if (isCorrect) {
                messageDisplay.textContent = `Player ${playerNum} Benar! Tarik! (Jawaban: ${currentCorrectAnswer})`;
                messageDisplay.className = 'message-correct';
                
                if (playerNum === 1) markerPosition--; 
                else markerPosition++; 
            } else {
                messageDisplay.textContent = `Player ${playerNum} Salah! Jawaban benar: ${currentCorrectAnswer}`;
                messageDisplay.className = 'message-wrong';
            }

            playerAnswers.p1 = '';
            playerAnswers.p2 = '';
            updateAnswerDisplays();

            updateMarkerPosition();
            
            setTimeout(() => {
                if (checkWin()) return; 

                generateQuestion();
                messageDisplay.textContent = "Soal baru... Jawab!";
                gameActive = true;
                activateGameControls(); 

            }, 2000); 
        }

        function checkWin() {
            if (markerPosition <= -winPosition) {
                endGame('Player 1 Menang!');
                return true;
            } else if (markerPosition >= winPosition) {
                endGame('Player 2 Menang!');
                return true;
            }
            return false;
        }

        function endGame(message) {
            gameActive = false;
            messageDisplay.textContent = message;
            messageDisplay.className = 'message-win';
            restartBtn.style.display = 'inline-block';
            backBtn.style.display = 'inline-block';
            
            deactivateGameControls();
            
            panelP1.classList.remove('active-panel');
            panelP2.classList.remove('active-panel');
            panelP1.style.opacity = 1;
            panelP2.style.opacity = 1;

            runConfetti();
        }

        function runConfetti() {
            confetti({
                particleCount: 100,
                spread: 70,
                origin: { x: 0.2, y: 0.6 },
                colors: ['#D9534F', '#FFFFFF'], 
                disableForReducedMotion: true
            });

            confetti({
                particleCount: 100,
                spread: 70,
                origin: { x: 0.8, y: 0.6 }, 
                colors: ['#D9534F', '#FFFFFF'],
                disableForReducedMotion: true
            });
            
            confetti({
                particleCount: 50,
                spread: 360,
                origin: { x: 0.5, y: 0.4 },
                colors: ['#D9534F', '#FFFFFF', '#f0ad4e'],
                disableForReducedMotion: true
            });
        }
        
        // FUNGSI BARU: Mengaktifkan/Nonaktifkan Fullscreen
        function toggleFullscreen() {
            const doc = document.documentElement;
            const isFullscreen = document.fullscreenElement || document.mozFullScreenElement || document.webkitFullscreenElement || document.msFullscreenElement;

            if (!isFullscreen) {
                if (doc.requestFullscreen) {
                    doc.requestFullscreen();
                } else if (doc.mozRequestFullScreen) { /* Firefox */
                    doc.mozRequestFullScreen();
                } else if (doc.webkitRequestFullscreen) { /* Chrome, Safari and Opera */
                    doc.webkitRequestFullscreen();
                } else if (doc.msRequestFullscreen) { /* IE/Edge */
                    doc.msRequestFullscreen();
                }
                fullscreenBtn.innerHTML = '<i class="fas fa-compress"></i> Exit Fullscreen';
            } else {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                } else if (document.mozCancelFullScreen) { /* Firefox */
                    document.mozCancelFullScreen();
                } else if (document.webkitExitFullscreen) { /* Chrome, Safari and Opera */
                    document.webkitExitFullscreen();
                } else if (document.msExitFullscreen) { /* IE/Edge */
                    document.msExitFullscreen();
                }
                fullscreenBtn.innerHTML = '<i class="fas fa-expand"></i> Fullscreen';
            }
        }


        // 5. Event Listeners
        
        modeGrid.addEventListener('click', (e) => {
            if (e.target.classList.contains('mode-btn')) {
                const mode = e.target.dataset.mode;
                initializeGame(mode);
            }
        });

        keypadP1.addEventListener('click', (e) => {
            if (e.target.tagName === 'BUTTON') {
                handleKeypadInput(e.target.dataset.key, 1);
            }
        });

        keypadP2.addEventListener('click', (e) => {
            if (e.target.tagName === 'BUTTON') {
                handleKeypadInput(e.target.dataset.key, 2);
            }
        });

        restartBtn.addEventListener('click', startGame);
        backBtn.addEventListener('click', () => {
            showScreen('selection');
        });
        
        // Event listener untuk Tombol Fullscreen ada di HTML: onclick="toggleFullscreen()"

        // Menangani perubahan tampilan tombol Fullscreen saat pengguna keluar menggunakan tombol ESC
        document.addEventListener('fullscreenchange', () => {
            if (!document.fullscreenElement) {
                fullscreenBtn.innerHTML = '<i class="fas fa-expand"></i> Fullscreen';
            }
        });
        document.addEventListener('webkitfullscreenchange', () => {
            if (!document.webkitFullscreenElement) {
                fullscreenBtn.innerHTML = '<i class="fas fa-expand"></i> Fullscreen';
            }
        });


        // Mulai aplikasi
        showScreen('selection');

    </script>

</body>
</html>
