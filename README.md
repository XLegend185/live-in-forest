# live-in-forest[deepseek_html_20251129_3ab8f8.html](https://github.com/user-attachments/files/23832940/deepseek_html_20251129_3ab8f8.html)
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Проклятие Старого Леса</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Courier New', monospace;
            user-select: none;
        }
        
        body {
            background-color: #000;
            color: #8B0000;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            touch-action: none;
        }
        
        #gameContainer {
            position: relative;
            width: 100%;
            max-width: 900px;
            height: 650px;
            border: 2px solid #8B0000;
            box-shadow: 0 0 20px #8B0000;
            overflow: hidden;
        }
        
        #gameCanvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle, #0a1f0a 0%, #000 70%);
        }
        
        #ui {
            position: absolute;
            top: 10px;
            left: 10px;
            z-index: 10;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 10px;
            border: 1px solid #8B0000;
            border-radius: 5px;
        }
        
        #stats {
            display: flex;
            gap: 15px;
            margin-bottom: 10px;
        }
        
        .stat {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .stat-value {
            font-size: 18px;
            font-weight: bold;
            margin-top: 5px;
        }
        
        .bar {
            width: 100px;
            height: 15px;
            background-color: #300;
            margin-top: 5px;
            border: 1px solid #8B0000;
            position: relative;
            overflow: hidden;
        }
        
        .bar-fill {
            height: 100%;
            position: absolute;
            left: 0;
            top: 0;
            transition: width 0.5s;
        }
        
        #healthBar .bar-fill {
            background: linear-gradient(to right, #8B0000, #ff0000);
            width: 100%;
        }
        
        #sanityBar .bar-fill {
            background: linear-gradient(to right, #4B0082, #8A2BE2);
            width: 100%;
        }
        
        #staminaBar .bar-fill {
            background: linear-gradient(to right, #006400, #32CD32);
            width: 100%;
        }
        
        #objectives {
            font-size: 14px;
            margin-top: 10px;
            max-width: 250px;
        }
        
        #message {
            position: absolute;
            bottom: 20px;
            left: 0;
            width: 100%;
            text-align: center;
            font-size: 18px;
            color: #8B0000;
            text-shadow: 0 0 10px #ff0000;
            z-index: 10;
            background-color: rgba(0, 0, 0, 0.5);
            padding: 10px;
            animation: pulse 2s infinite;
        }
        
        #startScreen, #gameOverScreen, #inventoryScreen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 20;
            color: #8B0000;
            text-align: center;
            padding: 20px;
        }
        
        h1 {
            font-size: 42px;
            margin-bottom: 20px;
            text-shadow: 0 0 10px #ff0000;
            animation: flicker 3s infinite;
        }
        
        h2 {
            font-size: 28px;
            margin-bottom: 15px;
            color: #8A2BE2;
        }
        
        p {
            margin-bottom: 15px;
            max-width: 80%;
            line-height: 1.5;
        }
        
        .story-text {
            font-size: 16px;
            max-width: 700px;
            text-align: left;
            margin-bottom: 20px;
            background: rgba(20, 0, 20, 0.5);
            padding: 15px;
            border-left: 3px solid #8B0000;
        }
        
        button {
            background-color: #000;
            color: #8B0000;
            border: 2px solid #8B0000;
            padding: 12px 25px;
            font-size: 18px;
            cursor: pointer;
            margin-top: 20px;
            transition: all 0.3s;
        }
        
        button:hover {
            background-color: #8B0000;
            color: #000;
            box-shadow: 0 0 15px #ff0000;
        }
        
        .hidden {
            display: none !important;
        }
        
        @keyframes flicker {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
        
        #flashlight {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 5;
            background: radial-gradient(circle at center, transparent 120px, rgba(0,0,0,0.95) 220px);
        }
        
        #monsterSight {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(139, 0, 0, 0.3);
            z-index: 6;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.5s;
        }
        
        #timer {
            position: absolute;
            top: 10px;
            right: 10px;
            z-index: 10;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 10px;
            border: 1px solid #8B0000;
            border-radius: 5px;
            font-size: 18px;
        }
        
        #controls {
            position: absolute;
            bottom: 10px;
            right: 10px;
            z-index: 10;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 10px;
            border: 1px solid #8B0000;
            border-radius: 5px;
            font-size: 14px;
            text-align: right;
        }
        
        #inventory {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-top: 20px;
            max-width: 400px;
        }
        
        .inventory-slot {
            width: 80px;
            height: 80px;
            border: 2px solid #8B0000;
            background-color: rgba(30, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
        }
        
        .inventory-slot.filled {
            background-color: rgba(70, 0, 0, 0.7);
        }
        
        #soundToggle {
            position: absolute;
            bottom: 10px;
            left: 10px;
            z-index: 10;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 5px 10px;
            border: 1px solid #8B0000;
            border-radius: 5px;
            font-size: 14px;
            cursor: pointer;
        }
        
        .objective-item {
            margin-bottom: 5px;
            padding-left: 15px;
            position: relative;
        }
        
        .objective-item::before {
            content: "■";
            position: absolute;
            left: 0;
            color: #8B0000;
        }
        
        .objective-item.completed {
            color: #32CD32;
            text-decoration: line-through;
        }
        
        .objective-item.completed::before {
            color: #32CD32;
        }
        
        #mobileControls {
            position: absolute;
            bottom: 20px;
            left: 20px;
            z-index: 10;
            display: none;
        }
        
        .joystick-area {
            width: 120px;
            height: 120px;
            background: rgba(0, 0, 0, 0.5);
            border: 2px solid #8B0000;
            border-radius: 50%;
            position: relative;
        }
        
        .joystick {
            width: 50px;
            height: 50px;
            background: rgba(139, 0, 0, 0.7);
            border-radius: 50%;
            position: absolute;
            top: 35px;
            left: 35px;
            touch-action: none;
        }
        
        #actionButtons {
            position: absolute;
            bottom: 20px;
            right: 20px;
            z-index: 10;
            display: none;
            gap: 10px;
        }
        
        .action-btn {
            width: 70px;
            height: 70px;
            background: rgba(0, 0, 0, 0.5);
            border: 2px solid #8B0000;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: #8B0000;
            touch-action: none;
        }
        
        @media (max-width: 768px) {
            #mobileControls, #actionButtons {
                display: flex;
            }
            
            #controls {
                display: none;
            }
        }
        
        #debug {
            position: absolute;
            top: 150px;
            left: 10px;
            z-index: 100;
            color: white;
            font-size: 12px;
            background: rgba(0,0,0,0.7);
            padding: 5px;
            display: none;
        }
    </style>
</head>
<body>
    <div id="gameContainer">
        <canvas id="gameCanvas"></canvas>
        <div id="flashlight"></div>
        <div id="monsterSight"></div>
        
        <div id="ui">
            <div id="stats">
                <div class="stat">
                    <div>ЗДОРОВЬЕ</div>
                    <div class="bar" id="healthBar"><div class="bar-fill"></div></div>
                    <div class="stat-value" id="healthValue">100</div>
                </div>
                
                <div class="stat">
                    <div>РАЗУМ</div>
                    <div class="bar" id="sanityBar"><div class="bar-fill"></div></div>
                    <div class="stat-value" id="sanityValue">100</div>
                </div>
                
                <div class="stat">
                    <div>ЭНЕРГИЯ</div>
                    <div class="bar" id="staminaBar"><div class="bar-fill"></div></div>
                    <div class="stat-value" id="staminaValue">100</div>
                </div>
            </div>
            
            <div id="objectives">
                <h3>ЦЕЛИ:</h3>
                <div class="objective-item" id="obj1">Найти убежище</div>
                <div class="objective-item" id="obj2">Собрать дрова для костра (0/5)</div>
                <div class="objective-item" id="obj3">Найти лекарства</div>
                <div class="objective-item" id="obj4">Найти ключ от машины</div>
                <div class="objective-item" id="obj5">Дожить до рассвета</div>
            </div>
        </div>
        
        <div id="timer">ВРЕМЯ ДО РАССВЕТА: <span id="timeValue">06:00</span></div>
        
        <div id="controls">
            WASD - движение<br>
            SHIFT - бег<br>
            F - фонарик<br>
            E - взаимодействие<br>
            I - инвентарь<br>
            ПРОБЕЛ - атака
        </div>
        
        <div id="soundToggle">🔊 ЗВУК ВКЛ</div>
        
        <div id="mobileControls">
            <div class="joystick-area">
                <div class="joystick" id="joystick"></div>
            </div>
        </div>
        
        <div id="actionButtons">
            <div class="action-btn" id="actionBtn">E</div>
            <div class="action-btn" id="flashlightBtn">F</div>
            <div class="action-btn" id="attackBtn">⚔</div>
        </div>
        
        <div id="message">НАЙДИТЕ УБЕЖИЩЕ И ПЕРЕЖДИТЕ НОЧЬ</div>
        
        <div id="startScreen">
            <h1>ПРОКЛЯТИЕ СТАРОГО ЛЕСА</h1>
            
            <div class="story-text">
                <p>Ваша машина сломалась на заброшенной лесной дороге. Связь отсутствует, а до ближайшего города несколько десятков километров.</p>
                <p>Местные жители шепотом рассказывают о Проклятии Старого Леса - древней силе, что пробуждается с наступлением темноты.</p>
                <p>Говорят, те, кто остался в лесу после заката, бесследно исчезали... или возвращались не теми, кем были.</p>
                <p>Солнце садится, и вы понимаете - нужно найти укрытие и пережить эту ночь любой ценой.</p>
            </div>
            
            <p>Управление: WASD - движение, SHIFT - бег, F - фонарик, E - взаимодействие, I - инвентарь</p>
            <button id="startButton">НАЧАТЬ ВЫЖИВАНИЕ</button>
        </div>
        
        <div id="gameOverScreen" class="hidden">
            <h1 id="gameOverTitle">ВЫ НЕ ВЫЖИЛИ</h1>
            <p id="gameOverText">Темнота поглотила вас...</p>
            <div class="story-text">
                <p id="gameOverStory">Лесу не нужны новые жертвы... он жаждет только новых обитателей для своей вечной ночи.</p>
            </div>
            <p>Выполнено целей: <span id="finalObjectives">0</span>/5</p>
            <p>Пережито времени: <span id="finalTime">00:00</span></p>
            <button id="restartButton">ПОПРОБОВАТЬ СНОВА</button>
        </div>
        
        <div id="inventoryScreen" class="hidden">
            <h2>ИНВЕНТАРЬ</h2>
            <div id="inventory">
                <div class="inventory-slot" id="slot1"></div>
                <div class="inventory-slot" id="slot2"></div>
                <div class="inventory-slot" id="slot3"></div>
                <div class="inventory-slot" id="slot4"></div>
                <div class="inventory-slot" id="slot5"></div>
                <div class="inventory-slot" id="slot6"></div>
            </div>
            <button id="closeInventory">ЗАКРЫТЬ (I)</button>
        </div>
        
        <div id="debug"></div>
    </div>

    <script>
        // Инициализация игры
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        canvas.width = 900;
        canvas.height = 650;

        // Элементы UI
        const startScreen = document.getElementById('startScreen');
        const gameOverScreen = document.getElementById('gameOverScreen');
        const inventoryScreen = document.getElementById('inventoryScreen');
        const startButton = document.getElementById('startButton');
        const restartButton = document.getElementById('restartButton');
        const closeInventory = document.getElementById('closeInventory');
        const message = document.getElementById('message');
        const healthValue = document.getElementById('healthValue');
        const sanityValue = document.getElementById('sanityValue');
        const staminaValue = document.getElementById('staminaValue');
        const healthFill = document.getElementById('healthBar').querySelector('.bar-fill');
        const sanityFill = document.getElementById('sanityBar').querySelector('.bar-fill');
        const staminaFill = document.getElementById('staminaBar').querySelector('.bar-fill');
        const timeValue = document.getElementById('timeValue');
        const monsterSight = document.getElementById('monsterSight');
        const flashlight = document.getElementById('flashlight');
        const finalObjectives = document.getElementById('finalObjectives');
        const finalTime = document.getElementById('finalTime');
        const gameOverTitle = document.getElementById('gameOverTitle');
        const gameOverText = document.getElementById('gameOverText');
        const gameOverStory = document.getElementById('gameOverStory');
        const soundToggle = document.getElementById('soundToggle');
        const debug = document.getElementById('debug');
        const objectives = {
            shelter: document.getElementById('obj1'),
            firewood: document.getElementById('obj2'),
            medicine: document.getElementById('obj3'),
            key: document.getElementById('obj4'),
            survive: document.getElementById('obj5')
        };

        // Мобильные элементы управления
        const joystick = document.getElementById('joystick');
        const joystickArea = document.querySelector('.joystick-area');
        const actionBtn = document.getElementById('actionBtn');
        const flashlightBtn = document.getElementById('flashlightBtn');
        const attackBtn = document.getElementById('attackBtn');

        // Игровые переменные
        let gameActive = false;
        let soundsEnabled = true;
        let player = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            radius: 15,
            speed: 2.5,
            health: 100,
            sanity: 100,
            stamina: 100,
            items: [],
            flashlightOn: false,
            inShelter: false,
            hasMedicine: false,
            hasKey: false,
            firewood: 0,
            lastAttack: 0
        };
        
        // Управление
        let keys = {};
        let touch = {
            joystick: { x: 0, y: 0, active: false },
            action: false,
            flashlight: false,
            attack: false
        };
        
        let items = [];
        let obstacles = [];
        let shelters = [];
        let monsters = [];
        let environment = [];
        let gameTime = 360; // 6 минут в секундах
        let messageTimeout;
        let lastStaminaUse = 0;
        let lastMonsterSpawn = 0;
        let monsterSpawnInterval = 15000; // 15 секунд
        let lastHeartbeat = 0;
        let heartbeatInterval = 1000;
        let lastUpdate = 0;
        let fps = 0;
        let frameCount = 0;
        let lastFpsUpdate = 0;
        
        // Звуковая система
        const audioContext = new (window.AudioContext || window.webkitAudioContext)();
        let backgroundNoise;
        
        function playSound(frequency, duration, type = 'sine', volume = 0.1) {
            if (!soundsEnabled) return;
            
            try {
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                oscillator.type = type;
                oscillator.frequency.value = frequency;
                
                gainNode.gain.value = volume;
                gainNode.gain.exponentialRampToValueAtTime(0.001, audioContext.currentTime + duration / 1000);
                
                oscillator.start();
                oscillator.stop(audioContext.currentTime + duration / 1000);
            } catch (e) {
                console.log("Audio error:", e);
            }
        }
        
        function playBackgroundNoise() {
            if (!soundsEnabled) return;
            
            try {
                // Создаем белый шум для фоновых звуков леса
                const bufferSize = 2 * audioContext.sampleRate;
                const noiseBuffer = audioContext.createBuffer(1, bufferSize, audioContext.sampleRate);
                const output = noiseBuffer.getChannelData(0);
                
                for (let i = 0; i < bufferSize; i++) {
                    output[i] = Math.random() * 2 - 1;
                }
                
                backgroundNoise = audioContext.createBufferSource();
                backgroundNoise.buffer = noiseBuffer;
                backgroundNoise.loop = true;
                
                const filter = audioContext.createBiquadFilter();
                filter.type = 'bandpass';
                filter.frequency.value = 300;
                filter.Q.value = 1;
                
                const gainNode = audioContext.createGain();
                gainNode.gain.value = 0.02;
                
                backgroundNoise.connect(filter);
                filter.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                backgroundNoise.start();
            } catch (e) {
                console.log("Background noise error:", e);
            }
        }
        
        function playHeartbeat() {
            if (!soundsEnabled) return;
            
            try {
                // Сердцебиение при низком здоровье или рассудке
                if (player.health < 30 || player.sanity < 30) {
                    heartbeatInterval = 600 - (Math.min(player.health, player.sanity) * 15);
                    
                    if (Date.now() - lastHeartbeat > heartbeatInterval) {
                        lastHeartbeat = Date.now();
                        playSound(80, 100, 'sine', 0.1);
                    }
                }
            } catch (e) {
                console.log("Heartbeat error:", e);
            }
        }
        
        function stopBackgroundNoise() {
            if (backgroundNoise) {
                backgroundNoise.stop();
                backgroundNoise = null;
            }
        }

        // Создание игрового мира
        function createWorld() {
            // Создаем препятствия (деревья, камни)
            obstacles = [];
            for (let i = 0; i < 40; i++) {
                obstacles.push({
                    x: Math.random() * canvas.width,
                    y: Math.random() * canvas.height,
                    radius: 12 + Math.random() * 20,
                    type: 'tree'
                });
            }
            
            // Добавляем камни
            for (let i = 0; i < 20; i++) {
                obstacles.push({
                    x: Math.random() * canvas.width,
                    y: Math.random() * canvas.height,
                    radius: 8 + Math.random() * 10,
                    type: 'rock'
                });
            }
            
            // Создаем убежища
            shelters = [];
            const shelter = {
                x: 150,
                y: 150,
                width: 80,
                height: 60,
                type: 'cabin'
            };
            shelters.push(shelter);
            
            // Создаем предметы для сбора
            items = [];
            
            // Лекарства
            items.push({
                x: 700,
                y: 500,
                radius: 12,
                type: 'medicine',
                collected: false
            });
            
            // Ключ от машины
            items.push({
                x: 650,
                y: 100,
                radius: 10,
                type: 'key',
                collected: false
            });
            
            // Дрова (5 штук)
            for (let i = 0; i < 5; i++) {
                items.push({
                    x: Math.random() * (canvas.width - 100) + 50,
                    y: Math.random() * (canvas.height - 100) + 50,
                    radius: 8,
                    type: 'firewood',
                    collected: false
                });
            }
            
            // Оружие (топор)
            items.push({
                x: 500,
                y: 300,
                radius: 15,
                type: 'axe',
                collected: false
            });
            
            // Монстры
            monsters = [];
            
            // Окружающая среда (особые зоны)
            environment = [
                { x: 400, y: 400, radius: 60, type: 'dark_zone', sanityDrain: 0.05 },
                { x: 750, y: 300, radius: 80, type: 'whispering_zone', sanityDrain: 0.08 }
            ];
            
            // Позиционируем игрока в центре
            player.x = canvas.width / 2;
            player.y = canvas.height / 2;
            player.health = 100;
            player.sanity = 100;
            player.stamina = 100;
            player.items = [];
            player.flashlightOn = false;
            player.inShelter = false;
            player.hasMedicine = false;
            player.hasKey = false;
            player.firewood = 0;
            player.lastAttack = 0;
            
            // Сбрасываем таймер
            gameTime = 360;
            
            // Сбрасываем цели
            objectives.shelter.classList.remove('completed');
            objectives.firewood.classList.remove('completed');
            objectives.medicine.classList.remove('completed');
            objectives.key.classList.remove('completed');
            objectives.survive.classList.remove('completed');
            objectives.firewood.textContent = 'Собрать дрова для костра (0/5)';
            
            // Обновляем UI
            updateUI();
            
            // Показываем начальное сообщение
            showMessage("Найдите убежище до наступления темноты!", 5000);
            
            // Запускаем фоновые звуки
            playBackgroundNoise();
        }

        // Отображение сообщений
        function showMessage(text, duration = 3000) {
            message.textContent = text;
            message.classList.remove('hidden');
            
            clearTimeout(messageTimeout);
            messageTimeout = setTimeout(() => {
                message.classList.add('hidden');
            }, duration);
        }

        // Обновление интерфейса
        function updateUI() {
            healthValue.textContent = Math.round(player.health);
            sanityValue.textContent = Math.round(player.sanity);
            staminaValue.textContent = Math.round(player.stamina);
            
            healthFill.style.width = `${player.health}%`;
            sanityFill.style.width = `${player.sanity}%`;
            staminaFill.style.width = `${player.stamina}%`;
            
            // Обновляем таймер
            const minutes = Math.floor(gameTime / 60);
            const seconds = Math.floor(gameTime % 60);
            timeValue.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            
            // Обновляем видимость монстра
            let monsterNearby = false;
            monsters.forEach(monster => {
                const distance = Math.sqrt(
                    Math.pow(player.x - monster.x, 2) + Math.pow(player.y - monster.y, 2)
                );
                
                if (distance < 200) {
                    monsterNearby = true;
                    monsterSight.style.opacity = (200 - distance) / 200;
                }
            });
            
            if (!monsterNearby) {
                monsterSight.style.opacity = 0;
            }
            
            // Обновляем фонарик
            if (player.flashlightOn) {
                flashlight.style.background = 'radial-gradient(circle at center, transparent 120px, rgba(0,0,0,0.85) 220px)';
            } else {
                flashlight.style.background = 'radial-gradient(circle at center, transparent 80px, rgba(0,0,0,0.95) 150px)';
            }
            
            // Обновляем инвентарь
            updateInventory();
            
            // Обновляем отладочную информацию
            updateDebugInfo();
        }
        
        // Обновление инвентаря
        function updateInventory() {
            for (let i = 1; i <= 6; i++) {
                const slot = document.getElementById(`slot${i}`);
                slot.classList.remove('filled');
                slot.textContent = '';
            }
            
            player.items.forEach((item, index) => {
                if (index < 6) {
                    const slot = document.getElementById(`slot${index + 1}`);
                    slot.classList.add('filled');
                    
                    switch(item.type) {
                        case 'medicine':
                            slot.textContent = '💊';
                            break;
                        case 'key':
                            slot.textContent = '🔑';
                            break;
                        case 'firewood':
                            slot.textContent = '🪵';
                            break;
                        case 'axe':
                            slot.textContent = '🪓';
                            break;
                        default:
                            slot.textContent = '?';
                    }
                }
            });
        }
        
        // Обновление отладочной информации
        function updateDebugInfo() {
            debug.innerHTML = `
                FPS: ${fps}<br>
                Monsters: ${monsters.length}<br>
                Items: ${items.filter(i => !i.collected).length}<br>
                X: ${Math.round(player.x)} Y: ${Math.round(player.y)}<br>
                Touch: ${touch.joystick.active ? 'Active' : 'Inactive'}
            `;
        }

        // Отрисовка игры
        function draw() {
            // Очистка canvas
            ctx.fillStyle = '#000';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Рисуем туман войны
            ctx.fillStyle = 'rgba(10, 20, 10, 0.9)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Рисуем область видимости игрока
            const gradient = ctx.createRadialGradient(
                player.x, player.y, 0,
                player.x, player.y, player.flashlightOn ? 220 : 150
            );
            gradient.addColorStop(0, 'rgba(20, 40, 20, 0.1)');
            gradient.addColorStop(1, 'rgba(5, 10, 5, 0.9)');
            
            ctx.globalCompositeOperation = 'destination-out';
            ctx.fillStyle = gradient;
            ctx.beginPath();
            ctx.arc(player.x, player.y, player.flashlightOn ? 220 : 150, 0, Math.PI * 2);
            ctx.fill();
            ctx.globalCompositeOperation = 'source-over';
            
            // Рисуем особые зоны
            environment.forEach(zone => {
                if (zone.type === 'dark_zone') {
                    ctx.fillStyle = 'rgba(0, 0, 10, 0.7)';
                } else if (zone.type === 'whispering_zone') {
                    ctx.fillStyle = 'rgba(30, 0, 30, 0.6)';
                }
                
                ctx.beginPath();
                ctx.arc(zone.x, zone.y, zone.radius, 0, Math.PI * 2);
                ctx.fill();
            });
            
            // Рисуем убежища
            shelters.forEach(shelter => {
                ctx.fillStyle = '#3A2618';
                ctx.fillRect(shelter.x - shelter.width/2, shelter.y - shelter.height/2, shelter.width, shelter.height);
                
                // Крыша
                ctx.fillStyle = '#2F1E0F';
                ctx.beginPath();
                ctx.moveTo(shelter.x - shelter.width/2 - 10, shelter.y - shelter.height/2);
                ctx.lineTo(shelter.x + shelter.width/2 + 10, shelter.y - shelter.height/2);
                ctx.lineTo(shelter.x, shelter.y - shelter.height/2 - 30);
                ctx.closePath();
                ctx.fill();
                
                // Дверь
                ctx.fillStyle = '#5D4037';
                ctx.fillRect(shelter.x - 15, shelter.y - shelter.height/2 + 10, 30, 40);
                
                // Окно
                ctx.fillStyle = '#1A237E';
                ctx.fillRect(shelter.x + 20, shelter.y - 10, 20, 20);
            });
            
            // Рисуем препятствия
            obstacles.forEach(obstacle => {
                if (obstacle.type === 'tree') {
                    ctx.fillStyle = '#0A300A';
                    ctx.beginPath();
                    ctx.arc(obstacle.x, obstacle.y, obstacle.radius, 0, Math.PI * 2);
                    ctx.fill();
                    
                    // Крона дерева
                    ctx.fillStyle = '#006400';
                    ctx.beginPath();
                    ctx.arc(obstacle.x, obstacle.y - 15, obstacle.radius + 5, 0, Math.PI * 2);
                    ctx.fill();
                } else if (obstacle.type === 'rock') {
                    ctx.fillStyle = '#3E3E3E';
                    ctx.beginPath();
                    ctx.arc(obstacle.x, obstacle.y, obstacle.radius, 0, Math.PI * 2);
                    ctx.fill();
                    
                    // Текстура камня
                    ctx.strokeStyle = '#2A2A2A';
                    ctx.lineWidth = 2;
                    ctx.beginPath();
                    ctx.arc(obstacle.x, obstacle.y, obstacle.radius - 2, 0, Math.PI * 2);
                    ctx.stroke();
                }
            });
            
            // Рисуем предметы для сбора
            items.forEach(item => {
                if (!item.collected) {
                    switch(item.type) {
                        case 'medicine':
                            ctx.fillStyle = '#FF5252';
                            ctx.beginPath();
                            ctx.arc(item.x, item.y, item.radius, 0, Math.PI * 2);
                            ctx.fill();
                            
                            ctx.fillStyle = '#FFFFFF';
                            ctx.fillRect(item.x - 4, item.y - 8, 8, 16);
                            break;
                            
                        case 'key':
                            ctx.fillStyle = '#FFD700';
                            ctx.beginPath();
                            ctx.arc(item.x, item.y, item.radius, 0, Math.PI * 2);
                            ctx.fill();
                            
                            ctx.fillStyle = '#000';
                            ctx.fillRect(item.x - 6, item.y - 2, 12, 4);
                            ctx.beginPath();
                            ctx.arc(item.x, item.y, 4, 0, Math.PI * 2);
                            ctx.fill();
                            break;
                            
                        case 'firewood':
                            ctx.fillStyle = '#8B4513';
                            ctx.fillRect(item.x - item.radius, item.y - item.radius/2, item.radius*2, item.radius);
                            break;
                            
                        case 'axe':
                            ctx.fillStyle = '#7A7A7A';
                            ctx.fillRect(item.x - 12, item.y - 3, 15, 6);
                            ctx.fillStyle = '#5D4037';
                            ctx.fillRect(item.x + 3, item.y - 8, 5, 16);
                            break;
                    }
                    
                    // Эффект свечения
                    ctx.shadowColor = item.type === 'key' ? '#FFD700' : '#FF5252';
                    ctx.shadowBlur = 10;
                    ctx.beginPath();
                    ctx.arc(item.x, item.y, item.radius, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.shadowBlur = 0;
                }
            });
            
            // Рисуем монстров
            monsters.forEach(monster => {
                ctx.fillStyle = '#300000';
                ctx.beginPath();
                ctx.arc(monster.x, monster.y, monster.radius, 0, Math.PI * 2);
                ctx.fill();
                
                // Глаза монстра
                ctx.fillStyle = '#ff0000';
                ctx.beginPath();
                ctx.arc(monster.x - 8, monster.y - 5, 4, 0, Math.PI * 2);
                ctx.arc(monster.x + 8, monster.y - 5, 4, 0, Math.PI * 2);
                ctx.fill();
                
                // Эффект свечения глаз
                ctx.shadowColor = '#ff0000';
                ctx.shadowBlur = 10;
                ctx.beginPath();
                ctx.arc(monster.x - 8, monster.y - 5, 4, 0, Math.PI * 2);
                ctx.arc(monster.x + 8, monster.y - 5, 4, 0, Math.PI * 2);
                ctx.fill();
                ctx.shadowBlur = 0;
            });
            
            // Рисуем игрока
            ctx.fillStyle = player.inShelter ? '#1a531b' : '#2d8b2d';
            ctx.beginPath();
            ctx.arc(player.x, player.y, player.radius, 0, Math.PI * 2);
            ctx.fill();
            
            // Направление игрока
            ctx.strokeStyle = '#4CAF50';
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.moveTo(player.x, player.y);
            ctx.lineTo(
                player.x + Math.cos(getPlayerDirection()) * player.radius * 1.5,
                player.y + Math.sin(getPlayerDirection()) * player.radius * 1.5
            );
            ctx.stroke();
            
            // Если у игрока есть топор, показываем его
            if (player.items.some(item => item.type === 'axe')) {
                ctx.fillStyle = '#7A7A7A';
                ctx.fillRect(player.x + 10, player.y - 5, 12, 4);
                ctx.fillStyle = '#5D4037';
                ctx.fillRect(player.x + 22, player.y - 8, 4, 10);
            }
        }

        // Получение направления игрока
        function getPlayerDirection() {
            // Используем джойстик для мобильных устройств
            if (touch.joystick.active) {
                return Math.atan2(touch.joystick.y, touch.joystick.x);
            }
            
            // Используем клавиатуру для десктопов
            let dx = 0, dy = 0;
            
            if (keys['KeyW'] || keys['ArrowUp']) dy -= 1;
            if (keys['KeyS'] || keys['ArrowDown']) dy += 1;
            if (keys['KeyA'] || keys['ArrowLeft']) dx -= 1;
            if (keys['KeyD'] || keys['ArrowRight']) dx += 1;
            
            // Если игрок не двигается, возвращаем предыдущее направление
            if (dx === 0 && dy === 0) {
                return player.direction || 0;
            }
            
            player.direction = Math.atan2(dy, dx);
            return player.direction;
        }

        // Обновление игрового состояния
        function update(timestamp) {
            if (!gameActive) return;
            
            // Расчет FPS
            frameCount++;
            if (timestamp - lastFpsUpdate >= 1000) {
                fps = Math.round((frameCount * 1000) / (timestamp - lastFpsUpdate));
                frameCount = 0;
                lastFpsUpdate = timestamp;
            }
            
            // Сохраняем предыдущую позицию для обработки столкновений
            const prevX = player.x;
            const prevY = player.y;
            
            // Определяем скорость и направление движения
            let moveX = 0, moveY = 0;
            let currentSpeed = player.speed;
            
            // Обработка управления с клавиатуры
            if (keys['KeyW'] || keys['ArrowUp']) moveY -= 1;
            if (keys['KeyS'] || keys['ArrowDown']) moveY += 1;
            if (keys['KeyA'] || keys['ArrowLeft']) moveX -= 1;
            if (keys['KeyD'] || keys['ArrowRight']) moveX += 1;
            
            // Обработка управления с джойстика
            if (touch.joystick.active) {
                moveX = touch.joystick.x;
                moveY = touch.joystick.y;
            }
            
            // Нормализация вектора движения (чтобы диагональное движение не было быстрее)
            const moveLength = Math.sqrt(moveX * moveX + moveY * moveY);
            if (moveLength > 0) {
                moveX /= moveLength;
                moveY /= moveLength;
            }
            
            // Бег (тратит выносливость)
            if ((keys['ShiftLeft'] || keys['ShiftRight'] || touch.joystick.active) && player.stamina > 0) {
                currentSpeed = player.speed * 1.8;
                player.stamina = Math.max(0, player.stamina - 0.8);
                lastStaminaUse = Date.now();
            } else if (Date.now() - lastStaminaUse > 1000) {
                player.stamina = Math.min(100, player.stamina + 0.5);
            }
            
            // Применяем движение
            player.x += moveX * currentSpeed;
            player.y += moveY * currentSpeed;
            
            // Ограничение движения в пределах canvas
            player.x = Math.max(player.radius, Math.min(canvas.width - player.radius, player.x));
            player.y = Math.max(player.radius, Math.min(canvas.height - player.radius, player.y));
            
            // Проверка столкновений с препятствиями
            let collision = false;
            obstacles.forEach(obstacle => {
                const distance = Math.sqrt(
                    Math.pow(player.x - obstacle.x, 2) + Math.pow(player.y - obstacle.y, 2)
                );
                
                if (distance < player.radius + obstacle.radius) {
                    collision = true;
                    // Отталкиваем игрока от препятствия
                    const angle = Math.atan2(player.y - obstacle.y, player.x - obstacle.x);
                    player.x = obstacle.x + Math.cos(angle) * (player.radius + obstacle.radius);
                    player.y = obstacle.y + Math.sin(angle) * (player.radius + obstacle.radius);
                }
            });
            
            // Проверка нахождения в убежище
            player.inShelter = false;
            shelters.forEach(shelter => {
                if (player.x > shelter.x - shelter.width/2 && player.x < shelter.x + shelter.width/2 &&
                    player.y > shelter.y - shelter.height/2 && player.y < shelter.y + shelter.height/2) {
                    player.inShelter = true;
                    if (!objectives.shelter.classList.contains('completed')) {
                        objectives.shelter.classList.add('completed');
                        showMessage("Вы в безопасности! Но ненадолго...", 4000);
                        playSound(400, 200, 'sine', 0.2);
                    }
                }
            });
            
            // Проверка нахождения в особых зонах
            environment.forEach(zone => {
                const distance = Math.sqrt(
                    Math.pow(player.x - zone.x, 2) + Math.pow(player.y - zone.y, 2)
                );
                
                if (distance < zone.radius) {
                    player.sanity = Math.max(0, player.sanity - zone.sanityDrain);
                    
                    if (zone.type === 'whispering_zone' && Math.random() < 0.01) {
                        showMessage("Шепот... они повсюду...", 2000);
                        playSound(150 + Math.random() * 50, 500, 'sawtooth', 0.1);
                    }
                }
            });
            
            // Проверка сбора предметов
            items.forEach(item => {
                if (!item.collected) {
                    const distance = Math.sqrt(
                        Math.pow(player.x - item.x, 2) + Math.pow(player.y - item.y, 2)
                    );
                    
                    if (distance < player.radius + item.radius) {
                        item.collected = true;
                        player.items.push(item);
                        
                        switch(item.type) {
                            case 'medicine':
                                player.hasMedicine = true;
                                objectives.medicine.classList.add('completed');
                                showMessage("Лекарства найдены! Используйте в инвентаре.", 4000);
                                playSound(600, 300, 'sine', 0.2);
                                break;
                                
                            case 'key':
                                player.hasKey = true;
                                objectives.key.classList.add('completed');
                                showMessage("Ключ от машины найден!", 3000);
                                playSound(500, 300, 'sine', 0.2);
                                break;
                                
                            case 'firewood':
                                player.firewood++;
                                objectives.firewood.textContent = `Собрать дрова для костра (${player.firewood}/5)`;
                                if (player.firewood === 5) {
                                    objectives.firewood.classList.add('completed');
                                    showMessage("Дрова собраны! Вернитесь в убежище.", 4000);
                                }
                                playSound(300, 200, 'sine', 0.1);
                                break;
                                
                            case 'axe':
                                showMessage("Топор найден! Теперь вы можете защищаться.", 4000);
                                playSound(200, 400, 'sine', 0.2);
                                break;
                        }
                    }
                }
            });
            
            // Использование предметов из инвентаря
            if ((keys['KeyE'] && !keys['KeyE'].processed) || touch.action) {
                if (keys['KeyE']) keys['KeyE'].processed = true;
                touch.action = false;
                
                // Использование лекарств
                if (player.hasMedicine) {
                    const medicineIndex = player.items.findIndex(item => item.type === 'medicine');
                    if (medicineIndex !== -1) {
                        player.items.splice(medicineIndex, 1);
                        player.health = Math.min(100, player.health + 40);
                        player.hasMedicine = false;
                        showMessage("Здоровье восстановлено!", 3000);
                        playSound(800, 500, 'sine', 0.3);
                    }
                }
                
                // Попытка уехать на машине
                if (player.hasKey && player.x > 800 && player.y > 550) {
                    if (player.firewood >= 5) {
                        winGame();
                    } else {
                        showMessage("Нужно собрать дрова для костра, чтобы осветить путь!", 4000);
                    }
                }
            }
            
            // Атака монстров
            if (((keys['Space'] && !keys['Space'].processed) || touch.attack) && Date.now() - player.lastAttack > 1000) {
                if (keys['Space']) keys['Space'].processed = true;
                touch.attack = false;
                player.lastAttack = Date.now();
                
                const hasAxe = player.items.some(item => item.type === 'axe');
                if (hasAxe) {
                    // Проверяем, есть ли монстры в радиусе атаки
                    for (let i = monsters.length - 1; i >= 0; i--) {
                        const monster = monsters[i];
                        const distance = Math.sqrt(
                            Math.pow(player.x - monster.x, 2) + Math.pow(player.y - monster.y, 2)
                        );
                        
                        if (distance < player.radius + monster.radius + 30) {
                            monsters.splice(i, 1);
                            showMessage("Монстр отброшен! Бегите!", 2000);
                            playSound(150, 300, 'square', 0.3);
                            break;
                        }
                    }
                } else {
                    showMessage("У вас нет оружия для атаки!", 2000);
                }
            }
            
            // Спавн монстров
            if (Date.now() - lastMonsterSpawn > monsterSpawnInterval && monsters.length < 3) {
                lastMonsterSpawn = Date.now();
                
                // Увеличиваем частоту появления монстров со временем
                monsterSpawnInterval = Math.max(5000, 15000 - (360 - gameTime) * 30);
                
                const monster = {
                    x: Math.random() < 0.5 ? -50 : canvas.width + 50,
                    y: Math.random() < 0.5 ? -50 : canvas.height + 50,
                    radius: 20,
                    speed: 1.2,
                    lastMove: Date.now()
                };
                
                monsters.push(monster);
                playSound(100, 800, 'sawtooth', 0.1);
                
                if (monsters.length === 1) {
                    showMessage("Что-то приближается... Будьте осторожны!", 4000);
                }
            }
            
            // Движение монстров
            monsters.forEach((monster, index) => {
                // Простой AI: двигаемся к игроку, но медленнее в убежище
                const dx = player.x - monster.x;
                const dy = player.y - monster.y;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                if (distance > 0) {
                    const speed = player.inShelter ? monster.speed * 0.5 : monster.speed;
                    monster.x += (dx / distance) * speed;
                    monster.y += (dy / distance) * speed;
                }
                
                // Проверка столкновения с игроком
                if (distance < player.radius + monster.radius) {
                    player.health -= 5;
                    playSound(50, 200, 'sawtooth', 0.2);
                    
                    if (player.health <= 0) {
                        gameOver("Монстры растерзали вас");
                    } else if (player.health < 30) {
                        showMessage("Вы ранены! Найдите лекарства!", 3000);
                    }
                }
                
                // Увеличение скорости монстров со временем
                monster.speed = Math.min(2.5, 1.2 + (360 - gameTime) / 120);
            });
            
            // Уменьшение рассудка со временем (медленнее в убежище)
            if (player.sanity > 0) {
                const drainRate = player.inShelter ? 0.01 : 0.03;
                player.sanity = Math.max(0, player.sanity - drainRate);
                
                if (player.sanity < 50 && player.sanity > 49) {
                    showMessage("Вы чувствуете беспокойство...", 3000);
                }
                
                if (player.sanity < 30) {
                    if (Math.random() < 0.01) {
                        showMessage("Голоса... они зовут вас...", 2000);
                        playSound(120 + Math.random() * 50, 1000, 'sawtooth', 0.1);
                    }
                    
                    // Визуальные искажения при низком рассудке
                    if (Math.random() < 0.02) {
                        monsterSight.style.opacity = 0.3;
                        setTimeout(() => {
                            if (gameActive) monsterSight.style.opacity = 0;
                        }, 200);
                    }
                }
                
                if (player.sanity <= 0) {
                    gameOver("Вы сошли с ума от ужаса");
                }
            }
            
            // Уменьшение здоровья от голода/холода (медленнее в убежище)
            if (player.health > 0) {
                const drainRate = player.inShelter ? 0.005 : 0.01;
                player.health = Math.max(0, player.health - drainRate);
                
                if (player.health <= 0) {
                    gameOver("Вы умерли от истощения");
                }
            }
            
            // Обновление таймера
            gameTime -= 1/60; // 60 FPS
            
            if (gameTime <= 0) {
                if (player.hasKey && player.firewood >= 5) {
                    winGame();
                } else {
                    gameOver("Рассвет наступил, но вы не смогли уехать");
                }
            }
            
            // Проверка победы по условиям
            if (player.hasKey && player.firewood >= 5 && player.x > 800 && player.y > 550) {
                winGame();
            }
            
            // Обновление UI
            updateUI();
            
            // Воспроизведение сердцебиения при необходимости
            playHeartbeat();
            
            // Перерисовка игры
            draw();
            
            // Продолжаем игровой цикл
            requestAnimationFrame(update);
        }

        // Завершение игры (поражение)
        function gameOver(reason) {
            gameActive = false;
            stopBackgroundNoise();
            
            gameOverTitle.textContent = "ВЫ НЕ ВЫЖИЛИ";
            gameOverText.textContent = reason;
            
            let storyText = "";
            if (reason.includes("монстры")) {
                storyText = "Лесу не нужны новые жертвы... он жаждет только новых обитателей для своей вечной ночи.";
            } else if (reason.includes("рассуд")) {
                storyText = "Голоса в вашей голове становятся громче... теперь они будут шептать здесь вечно.";
            } else if (reason.includes("рассвет")) {
                storyText = "Новая ночь придет совсем скоро... и на этот раз у леса будет больше времени.";
            } else {
                storyText = "Тьма поглотила вас, как поглощала многих до вас... и будет поглощать многих после.";
            }
            
            gameOverStory.textContent = storyText;
            
            // Подсчет выполненных целей
            let completed = 0;
            if (objectives.shelter.classList.contains('completed')) completed++;
            if (objectives.firewood.classList.contains('completed')) completed++;
            if (objectives.medicine.classList.contains('completed')) completed++;
            if (objectives.key.classList.contains('completed')) completed++;
            
            finalObjectives.textContent = completed;
            
            const minutes = Math.floor((360 - gameTime) / 60);
            const seconds = Math.floor((360 - gameTime) % 60);
            finalTime.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            
            gameOverScreen.classList.remove('hidden');
            playSound(80, 2000, 'sawtooth', 0.3);
        }

        // Завершение игры (победа)
        function winGame() {
            gameActive = false;
            stopBackgroundNoise();
            
            gameOverTitle.textContent = "ВЫ ВЫЖИЛИ!";
            gameOverText.textContent = "Вам удалось уехать из проклятого леса!";
            gameOverStory.textContent = "Вы оглядываетесь на темнеющий лес в зеркало заднего вида. Что-то шевелится между деревьями... но теперь это не ваша забота. По крайней мере, до следующей ночи.";
            
            objectives.survive.classList.add('completed');
            finalObjectives.textContent = 5;
            
            const minutes = Math.floor((360 - gameTime) / 60);
            const seconds = Math.floor((360 - gameTime) % 60);
            finalTime.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            
            gameOverScreen.classList.remove('hidden');
            
            // Победный звук
            playSound(523, 300, 'sine', 0.2);
            setTimeout(() => playSound(659, 300, 'sine', 0.2), 300);
            setTimeout(() => playSound(784, 500, 'sine', 0.2), 600);
        }

        // Обработка нажатий клавиш
        document.addEventListener('keydown', (e) => {
            if (!keys[e.code]) {
                keys[e.code] = { pressed: true, processed: false };
            } else {
                keys[e.code].pressed = true;
                keys[e.code].processed = false;
            }
            
            // Включение/выключение фонарика
            if (e.code === 'KeyF' && gameActive) {
                player.flashlightOn = !player.flashlightOn;
                playSound(200, 100, 'square', 0.1);
                
                if (player.flashlightOn) {
                    showMessage("Фонарик включен", 1500);
                } else {
                    showMessage("Фонарик выключен", 1500);
                }
            }
            
            // Открытие/закрытие инвентаря
            if (e.code === 'KeyI' && gameActive) {
                inventoryScreen.classList.toggle('hidden');
            }
            
            // Закрытие инвентаря
            if (e.code === 'KeyI' && inventoryScreen.classList.contains('hidden') === false) {
                inventoryScreen.classList.add('hidden');
            }
            
            // Предотвращение действий по умолчанию для игровых клавиш
            if (['KeyW', 'KeyA', 'KeyS', 'KeyD', 'ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight', 'Space', 'ShiftLeft', 'ShiftRight'].includes(e.code)) {
                e.preventDefault();
            }
        });

        document.addEventListener('keyup', (e) => {
            if (keys[e.code]) {
                keys[e.code].pressed = false;
                keys[e.code].processed = false;
            }
        });

        // Мобильное управление - джойстик
        let joystickStartX = 0, joystickStartY = 0;
        
        joystick.addEventListener('touchstart', (e) => {
            e.preventDefault();
            const touch = e.touches[0];
            const rect = joystickArea.getBoundingClientRect();
            joystickStartX = rect.left + rect.width / 2;
            joystickStartY = rect.top + rect.height / 2;
            touch.joystick.active = true;
        });
        
        document.addEventListener('touchmove', (e) => {
            if (!touch.joystick.active) return;
            
            e.preventDefault();
            const touch = e.touches[0];
            const deltaX = touch.clientX - joystickStartX;
            const deltaY = touch.clientY - joystickStartY;
            
            // Ограничиваем движение джойстика в пределах области
            const distance = Math.min(60, Math.sqrt(deltaX * deltaX + deltaY * deltaY));
            const angle = Math.atan2(deltaY, deltaX);
            
            touch.joystick.x = Math.cos(angle) * (distance / 60);
            touch.joystick.y = Math.sin(angle) * (distance / 60);
            
            // Перемещаем визуальный джойстик
            joystick.style.transform = `translate(${touch.joystick.x * 35}px, ${touch.joystick.y * 35}px)`;
        }, { passive: false });
        
        document.addEventListener('touchend', (e) => {
            touch.joystick.active = false;
            touch.joystick.x = 0;
            touch.joystick.y = 0;
            joystick.style.transform = 'translate(0, 0)';
        });
        
        // Мобильные кнопки действий
        actionBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            touch.action = true;
        });
        
        flashlightBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            if (gameActive) {
                player.flashlightOn = !player.flashlightOn;
                playSound(200, 100, 'square', 0.1);
                
                if (player.flashlightOn) {
                    showMessage("Фонарик включен", 1500);
                } else {
                    showMessage("Фонарик выключен", 1500);
                }
            }
        });
        
        attackBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            touch.attack = true;
        });
        
        // Запуск игры
        startButton.addEventListener('click', () => {
            startScreen.classList.add('hidden');
            gameActive = true;
            createWorld();
            playSound(300, 500, 'sine', 0.3);
            setTimeout(() => playSound(400, 500, 'sine', 0.3), 500);
            
            // Запускаем игровой цикл
            requestAnimationFrame(update);
        });

        // Перезапуск игры
        restartButton.addEventListener('click', () => {
            gameOverScreen.classList.add('hidden');
            startScreen.classList.remove('hidden');
        });

        // Закрытие инвентаря
        closeInventory.addEventListener('click', () => {
            inventoryScreen.classList.add('hidden');
        });

        // Включение/выключение звука
        soundToggle.addEventListener('click', () => {
            soundsEnabled = !soundsEnabled;
            soundToggle.textContent = soundsEnabled ? "🔊 ЗВУК ВКЛ" : "🔇 ЗВУК ВЫКЛ";
            
            if (!soundsEnabled) {
                stopBackgroundNoise();
            } else if (gameActive) {
                playBackgroundNoise();
            }
        });

        // Предотвращение контекстного меню
        document.addEventListener('contextmenu', (e) => {
            e.preventDefault();
        });

        // Инициализация игры при загрузке
        window.addEventListener('load', () => {
            draw(); // Первоначальная отрисовка
            
            // Определяем, мобильное ли устройство
            if ('ontouchstart' in window || navigator.maxTouchPoints) {
                document.getElementById('mobileControls').style.display = 'flex';
                document.getElementById('actionButtons').style.display = 'flex';
            }
        });
    </script>
</body>
</html>
