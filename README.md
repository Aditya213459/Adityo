<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rath Yatra – Race of Realms</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            overflow: hidden;
            font-family: 'Amita', cursive;
            background: #000;
            color: #fff;
            touch-action: manipulation;
        }

        #gameContainer {
            position: relative;
            width: 100vw;
            height: 100vh;
            overflow: hidden;
        }

        #gameCanvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        /* UI Elements */
        #uiContainer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 2;
            pointer-events: none;
        }

        #healthBar {
            position: absolute;
            top: 20px;
            left: 20px;
            width: 200px;
            height: 20px;
            background: rgba(0, 0, 0, 0.5);
            border: 2px solid #ff3333;
            border-radius: 5px;
            overflow: hidden;
        }

        #healthFill {
            height: 100%;
            width: 100%;
            background: linear-gradient(to right, #ff3333, #ff9999);
            transition: width 0.3s;
        }

        #divineMeter {
            position: absolute;
            top: 50px;
            left: 20px;
            width: 200px;
            height: 10px;
            background: rgba(0, 0, 0, 0.5);
            border: 1px solid #33ccff;
            border-radius: 5px;
            overflow: hidden;
        }

        #divineFill {
            height: 100%;
            width: 0%;
            background: linear-gradient(to right, #33ccff, #99e6ff);
            transition: width 0.3s;
        }

        #scoreDisplay {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 24px;
            color: gold;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
        }

        #relicsCollected {
            position: absolute;
            top: 50px;
            right: 20px;
            font-size: 18px;
            color: #ffcc00;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
        }

        /* Music Control */
        #musicControl {
            position: fixed;
            top: 10px;
            right: 60px;
            z-index: 1000;
            background: rgba(0,0,0,0.5);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: white;
            font-size: 20px;
            pointer-events: auto;
        }

        /* Save/Load Controls */
        #saveLoadControls {
            position: fixed;
            top: 10px;
            right: 110px;
            z-index: 1000;
            display: flex;
            gap: 5px;
        }

        .saveLoadBtn {
            background: rgba(0,0,0,0.5);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: white;
            font-size: 16px;
            border: none;
            pointer-events: auto;
        }

        /* Cutscene Elements */
        #cutsceneContainer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 10;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 20px;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.5s;
        }

        #cutsceneText {
            font-size: 24px;
            color: #fff;
            margin-bottom: 30px;
            max-width: 800px;
            line-height: 1.5;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
        }

        #cutsceneImage {
            max-width: 80%;
            max-height: 50vh;
            margin-bottom: 30px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.5);
        }

        #cutsceneContinue {
            background: linear-gradient(to bottom, #ffcc00, #ff9900);
            border: none;
            color: #000;
            padding: 10px 30px;
            font-size: 20px;
            border-radius: 50px;
            cursor: pointer;
            pointer-events: auto;
            font-family: 'Amita', cursive;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            transition: transform 0.2s, box-shadow 0.2s;
        }

        #cutsceneContinue:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
        }

        #cutsceneContinue:active {
            transform: translateY(0);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }

        /* Menu System */
        #mainMenu {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url('https://example.com/menu-bg.jpg') no-repeat center center;
            background-size: cover;
            z-index: 20;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        #menuTitle {
            font-size: 72px;
            color: gold;
            text-shadow: 4px 4px 8px rgba(0, 0, 0, 0.7);
            margin-bottom: 50px;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .menuButton {
            background: linear-gradient(to bottom, #ffcc00, #ff9900);
            border: none;
            color: #000;
            padding: 15px 40px;
            font-size: 24px;
            border-radius: 50px;
            cursor: pointer;
            margin: 10px 0;
            font-family: 'Amita', cursive;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            transition: transform 0.2s, box-shadow 0.2s;
            width: 300px;
            text-align: center;
        }

        .menuButton:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.4);
        }

        .menuButton:active {
            transform: translateY(0);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }

        /* Mobile Controls */
        #mobileControls {
            position: absolute;
            bottom: 20px;
            width: 100%;
            display: flex;
            justify-content: space-between;
            padding: 0 20px;
            z-index: 3;
            display: none;
        }

        .mobileButton {
            width: 80px;
            height: 80px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 24px;
            backdrop-filter: blur(5px);
            border: 2px solid rgba(255, 255, 255, 0.3);
            pointer-events: auto;
            touch-action: manipulation;
        }

        /* Level Complete Screen */
        #levelComplete {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 15;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
        }

        #levelComplete h2 {
            font-size: 48px;
            color: gold;
            margin-bottom: 30px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
        }

        #levelCompleteStats {
            font-size: 24px;
            color: #fff;
            margin-bottom: 30px;
            line-height: 1.5;
        }

        /* Game Over Screen */
        #gameOver {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 15;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
        }

        #gameOver h2 {
            font-size: 48px;
            color: #ff3333;
            margin-bottom: 30px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
        }

        /* Loading Screen */
        #loadingScreen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            z-index: 30;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        #loadingProgress {
            width: 80%;
            max-width: 500px;
            height: 20px;
            background: #333;
            margin-top: 20px;
            border-radius: 10px;
            overflow: hidden;
        }

        #loadingBar {
            height: 100%;
            width: 0%;
            background: linear-gradient(to right, #ffcc00, #ff9900);
            transition: width 0.3s;
        }

        /* Save Slot Selection */
        #saveSlots {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            z-index: 25;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        .saveSlot {
            width: 80%;
            max-width: 600px;
            background: rgba(255, 215, 0, 0.1);
            border: 2px solid gold;
            border-radius: 10px;
            padding: 15px;
            margin: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .saveSlotInfo {
            text-align: left;
        }

        .saveSlotButtons {
            display: flex;
            gap: 10px;
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            #menuTitle {
                font-size: 48px;
                margin-bottom: 30px;
            }
            
            .menuButton {
                width: 250px;
                padding: 12px 30px;
                font-size: 20px;
            }
            
            #cutsceneText {
                font-size: 18px;
            }
            
            #mobileControls {
                display: flex;
            }

            #saveLoadControls {
                top: 60px;
                right: 10px;
            }

            #musicControl {
                top: 60px;
                right: 60px;
            }
        }

.rain {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  background-image: url('https://i.ibb.co/9VGyfx5/rain.gif');
  opacity: 0.5;
  z-index: 999;
}

    </style>
    <link href="https://fonts.googleapis.com/css2?family=Amita:wght@400;700&display=swap" rel="stylesheet">
</head>
<body>

<div id="weatherEffect"></div>

    <!-- Audio Element -->
    <audio id="bgMusic" loop>
        <source src="Astradhari - Epic Indian Music.mp3" type="audio/mpeg">
    </audio>

    <div id="gameContainer">
        <!-- Loading Screen -->
        <div id="loadingScreen">
            <h1>Loading Rath Yatra...</h1>
            <div id="loadingProgress">
                <div id="loadingBar"></div>
            </div>
        </div>

        <!-- Save Slot Selection -->
        <div id="saveSlots">
            <h1 style="color:gold; margin-bottom:30px;">Choose Save Slot</h1>
            <div id="slot1" class="saveSlot">
                <div class="saveSlotInfo">
                    <h3>Slot 1</h3>
                    <p id="slot1Info">Empty</p>
                </div>
                <div class="saveSlotButtons">
                    <button class="menuButton" onclick="loadFromSlot(1)">Load</button>
                    <button class="menuButton" onclick="saveToSlot(1)">Save</button>
                </div>
            </div>
            <div id="slot2" class="saveSlot">
                <div class="saveSlotInfo">
                    <h3>Slot 2</h3>
                    <p id="slot2Info">Empty</p>
                </div>
                <div class="saveSlotButtons">
                    <button class="menuButton" onclick="loadFromSlot(2)">Load</button>
                    <button class="menuButton" onclick="saveToSlot(2)">Save</button>
                </div>
            </div>
            <div id="slot3" class="saveSlot">
                <div class="saveSlotInfo">
                    <h3>Slot 3</h3>
                    <p id="slot3Info">Empty</p>
                </div>
                <div class="saveSlotButtons">
                    <button class="menuButton" onclick="loadFromSlot(3)">Load</button>
                    <button class="menuButton" onclick="saveToSlot(3)">Save</button>
                </div>
            </div>
            <button class="menuButton" style="margin-top:30px;" onclick="document.getElementById('saveSlots').style.display='none'">Back</button>
        </div>

        <!-- Main Menu -->
        <div id="mainMenu">
            <h1 id="menuTitle">रथ यात्रा</h1>
            <button class="menuButton" id="startGame">New Journey</button>
            <button class="menuButton" id="loadGame">Continue Journey</button>
            <button class="menuButton" id="levelSelect">Choose Realm</button>
            <button class="menuButton" id="controlsButton">Controls</button>
            <button class="menuButton" id="creditsButton">Credits</button>
        </div>

        <!-- Game Canvas -->
        <canvas id="gameCanvas"></canvas>

        <!-- UI Elements -->
        <div id="uiContainer">
            <div id="healthBar">
                <div id="healthFill"></div>
            </div>
            <div id="divineMeter">
                <div id="divineFill"></div>
            </div>
            <div id="scoreDisplay">Divine Power: 0</div>
            <div id="relicsCollected">Relics: 0/0</div>
        </div>

        <!-- Music Control -->
        <div id="musicControl">🔊</div>

        <!-- Save/Load Controls -->
        <div id="saveLoadControls">
            <button class="saveLoadBtn" title="Quick Save" onclick="quickSave()">💾</button>
            <button class="saveLoadBtn" title="Save Slots" onclick="showSaveSlots()">📁</button>
        </div>

        <!-- Mobile Controls -->
        <div id="mobileControls">
            <div class="mobileButton" id="leftButton">←</div>
            <div class="mobileButton" id="rightButton">→</div>
            <div class="mobileButton" id="jumpButton">↑</div>
            <div class="mobileButton" id="divineButton">✦</div>
        </div>

        <!-- Cutscene Container -->
        <div id="cutsceneContainer">
            <img id="cutsceneImage" src="" alt="Cutscene Image">
            <div id="cutsceneText"></div>
            <button id="cutsceneContinue">Continue</button>
        </div>

        <!-- Level Complete Screen -->
        <div id="levelComplete">
            <h2>Realm Completed!</h2>
            <div id="levelCompleteStats"></div>
            <button class="menuButton" id="nextLevelButton">Continue Journey</button>
            <button class="menuButton" id="levelCompleteMenuButton">Main Menu</button>
        </div>

        <!-- Game Over Screen -->
        <div id="gameOver">
            <h2>Game Over</h2>
            <div id="gameOverStats"></div>
            <button class="menuButton" id="retryButton">Retry</button>
            <button class="menuButton" id="gameOverMenuButton">Main Menu</button>
        </div>
    </div>

    <script>
        // Game Constants
        const REALMS = [
            {
                name: "Bhooloka",
                description: "The earthly realm where mortals dwell. The soil trembles with unrest. Cities burn. Forests weep.",
                bgColor: "#3a6b35",
                enemyTypes: ["rakshasa", "wild_beast"],
                obstacles: ["fire", "broken_bridge"],
                music: "bhooloka_theme.mp3",
                relics: 5,
                boss: false
            },
            {
                name: "Paatal Loka",
                description: "The underworld ruled by shadow-serpents and forgotten gods. The light is your only friend here.",
                bgColor: "#2d1a36",
                enemyTypes: ["naga", "cursed_soul"],
                obstacles: ["lava", "collapsing_ground"],
                music: "paatal_theme.mp3",
                relics: 7,
                boss: false
            },
            {
                name: "Swarga Loka",
                description: "The heavenly realm of celestial beings. Devas offer blessings... but not all trust your cause.",
                bgColor: "#3366cc",
                enemyTypes: ["deva", "storm_elemental"],
                obstacles: ["lightning", "floating_islands"],
                music: "swarga_theme.mp3",
                relics: 8,
                boss: false
            },
            {
                name: "Gandharva Loka",
                description: "The realm where music shapes the world. The road sings, the winds dance, and enemies strike in rhythm.",
                bgColor: "#9933cc",
                enemyTypes: ["gandharva", "sound_wave"],
                obstacles: ["sound_barrier", "rhythm_gate"],
                music: "gandharva_theme.mp3",
                relics: 10,
                boss: false
            },
            {
                name: "Yaksha Loka",
                description: "The realm of ancient guardians who once served the balance. Now they question your mission.",
                bgColor: "#006666",
                enemyTypes: ["yaksha", "guardian_spirit"],
                obstacles: ["puzzle_door", "maze"],
                music: "yaksha_theme.mp3",
                relics: 12,
                boss: false
            },
            {
                name: "Rakshasa Loka",
                description: "Corrupted and cruel, the Rakshasas thrive in this chaos. Their king commands the shadows.",
                bgColor: "#990000",
                enemyTypes: ["rakshasa_elite", "shadow_beast"],
                obstacles: ["darkness", "blood_river"],
                music: "rakshasa_theme.mp3",
                relics: 15,
                boss: true
            },
            {
                name: "Vaikuntha",
                description: "You have arrived at the gates of eternity. But Vaikuntha is sealed by a corrupted celestial.",
                bgColor: "#ffcc00",
                enemyTypes: ["celestial_guardian"],
                obstacles: ["divine_barrier"],
                music: "vaikuntha_theme.mp3",
                relics: 0,
                boss: true
            }
        ];

        // Game Variables
        let canvas, ctx;
        let gameActive = false;
        let currentLevel = 0;
        let player = {
            x: 0,
            y: 0,
            width: 60,
            height: 100,
            speed: 5,
            velocityY: 0,
            jumping: false,
            health: 100,
            maxHealth: 100,
            divine: 0,
            divineActive: false,
            divineDuration: 0,
            relicsCollected: 0,
            score: 0,
            facingRight: true
        };
        
        let chariot = {
            x: 0,
            y: 0,
            width: 120,
            height: 80,
            horses: 4,
            speedBoost: 0,
            protection: 0
        };

        let enemies = [];
        let obstacles = [];
        let relics = [];
        let platforms = [];
        let backgroundLayers = [];
        let camera = { x: 0, y: 0 };
        let keys = {};
        let touchControls = {
            left: false,
            right: false,
            jump: false,
            divine: false
        };

        // Music System
        const music = {
            element: document.getElementById('bgMusic'),
            isMuted: false,
            
            init: function() {
                this.element.volume = 0.6;
                document.addEventListener('click', this.unlockAudio.bind(this), { once: true });
                document.addEventListener('touchstart', this.unlockAudio.bind(this), { once: true });
                document.getElementById('musicControl').addEventListener('click', this.toggle.bind(this));
            },
            
            unlockAudio: function() {
                this.element.play().catch(e => {
                    console.log("Autoplay blocked - will play after user interaction");
                });
            },
            
            toggle: function() {
                this.isMuted = !this.isMuted;
                this.element.muted = this.isMuted;
                document.getElementById('musicControl').textContent = this.isMuted ? '🔇' : '🔈';
                if ('vibrate' in navigator) navigator.vibrate(20);
            }
        };

        // Save System
        const saveSystem = {
            currentSlot: 0,
            
            init: function() {
                this.updateSaveSlotDisplays();
            },
            
            getSaveKey: function(slot) {
                return `rathYatraSave_${slot}`;
            },
            
            updateSaveSlotDisplays: function() {
                for (let i = 1; i <= 3; i++) {
                    const saveData = this.loadFromStorage(i);
                    const slotInfo = document.getElementById(`slot${i}Info`);
                    if (saveData) {
                        const date = new Date(saveData.timestamp);
                        slotInfo.innerHTML = `
                            Realm: ${REALMS[saveData.currentLevel].name}<br>
                            Score: ${saveData.playerStats.score}<br>
                            Saved: ${date.toLocaleString()}
                        `;
                    } else {
                        slotInfo.textContent = "Empty";
                    }
                }
            },
            
            saveToStorage: function(slot, data) {
                data.timestamp = Date.now();
                localStorage.setItem(this.getSaveKey(slot), JSON.stringify(data));
                this.updateSaveSlotDisplays();
            },
            
            loadFromStorage: function(slot) {
                const data = localStorage.getItem(this.getSaveKey(slot));
                return data ? JSON.parse(data) : null;
            },
            
            deleteSave: function(slot) {
                localStorage.removeItem(this.getSaveKey(slot));
                this.updateSaveSlotDisplays();
            },
            
            quickSave: function() {
                if (!gameActive) return;
                
                const saveData = this.createSaveData();
                this.saveToStorage(this.currentSlot || 1, saveData);
                
                // Show quick save notification
                const notification = document.createElement('div');
                notification.textContent = 'Game Saved!';
                notification.style.position = 'fixed';
                notification.style.bottom = '20px';
                notification.style.left = '50%';
                notification.style.transform = 'translateX(-50%)';
                notification.style.backgroundColor = 'rgba(0,0,0,0.7)';
                notification.style.color = 'gold';
                notification.style.padding = '10px 20px';
                notification.style.borderRadius = '20px';
                notification.style.zIndex = '1000';
                document.body.appendChild(notification);
                
                setTimeout(() => {
                    document.body.removeChild(notification);
                }, 2000);
            },
            
            createSaveData: function() {
                return {
                    currentLevel: currentLevel,
                    playerStats: {
                        health: player.health,
                        maxHealth: player.maxHealth,
                        divine: player.divine,
                        relicsCollected: player.relicsCollected,
                        score: player.score
                    },
                    chariotStats: {
                        speedBoost: chariot.speedBoost,
                        protection: chariot.protection
                    },
                    relics: this.getCollectedRelics()
                };
            },
            
            getCollectedRelics: function() {
                const collected = {};
                REALMS.forEach((realm, index) => {
                    collected[index] = {
                        total: realm.relics,
                        collected: index < currentLevel ? realm.relics : 0
                    };
                });
                collected[currentLevel] = {
                    total: REALMS[currentLevel].relics,
                    collected: player.relicsCollected
                };
                return collected;
            },
            
            loadGame: function(slot) {
                const saveData = this.loadFromStorage(slot);
                if (!saveData) {
                    alert("No saved game in this slot!");
                    return false;
                }
                
                this.currentSlot = slot;
                currentLevel = saveData.currentLevel;
                
                // Restore player stats
                player.health = saveData.playerStats.health;
                player.maxHealth = saveData.playerStats.maxHealth;
                player.divine = saveData.playerStats.divine;
                player.relicsCollected = saveData.playerStats.relicsCollected;
                player.score = saveData.playerStats.score;
                
                // Restore chariot upgrades
                chariot.speedBoost = saveData.chariotStats.speedBoost;
                chariot.protection = saveData.chariotStats.protection;
                
                return true;
            }
        };

        // Game Initialization
        function init() {
            canvas = document.getElementById('gameCanvas');
            ctx = canvas.getContext('2d');
            
            // Set canvas size
            resizeCanvas();
            window.addEventListener('resize', resizeCanvas);
            
            // Initialize systems
            music.init();
            saveSystem.init();
            
            // Check for existing save
            checkForSavedGame();
            
            // Load assets (simulated)
            simulateLoading();
            
            // Event listeners
            setupEventListeners();
        }

        function checkForSavedGame() {
            let hasSave = false;
            for (let i = 1; i <= 3; i++) {
                if (saveSystem.loadFromStorage(i)) {
                    hasSave = true;
                    break;
                }
            }
            document.getElementById('loadGame').style.display = hasSave ? 'block' : 'none';
        }

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            
            if (gameActive) {
                player.x = canvas.width / 4;
                player.y = canvas.height - 150;
                chariot.x = player.x - 30;
                chariot.y = player.y + 50;
            }
        }

        function simulateLoading() {
            let progress = 0;
            const loadingInterval = setInterval(() => {
                progress += Math.random() * 10;
                if (progress >= 100) {
                    progress = 100;
                    clearInterval(loadingInterval);
                    setTimeout(() => {
                        document.getElementById('loadingScreen').style.display = 'none';
                        document.getElementById('mainMenu').style.display = 'flex';
                    }, 500);
                }
                document.getElementById('loadingBar').style.width = progress + '%';
            }, 200);
        }

        function setupEventListeners() {
            // Keyboard controls
            document.addEventListener('keydown', (e) => {
                keys[e.key] = true;
                
                if (document.getElementById('cutsceneContainer').style.opacity === '1') {
                    document.getElementById('cutsceneContinue').click();
                }
            });
            
            document.addEventListener('keyup', (e) => {
                keys[e.key] = false;
            });
            
            // Menu buttons
            document.getElementById('startGame').addEventListener('click', startNewGame);
            document.getElementById('loadGame').addEventListener('click', showSaveSlots);
            document.getElementById('levelSelect').addEventListener('click', showLevelSelect);
            document.getElementById('controlsButton').addEventListener('click', showControls);
            document.getElementById('creditsButton').addEventListener('click', showCredits);
            document.getElementById('nextLevelButton').addEventListener('click', nextLevel);
            document.getElementById('levelCompleteMenuButton').addEventListener('click', returnToMenu);
            document.getElementById('retryButton').addEventListener('click', retryLevel);
            document.getElementById('gameOverMenuButton').addEventListener('click', returnToMenu);
            document.getElementById('cutsceneContinue').addEventListener('click', continueCutscene);
            
            // Mobile controls
            document.getElementById('leftButton').addEventListener('touchstart', () => touchControls.left = true);
            document.getElementById('leftButton').addEventListener('touchend', () => touchControls.left = false);
            document.getElementById('rightButton').addEventListener('touchstart', () => touchControls.right = true);
            document.getElementById('rightButton').addEventListener('touchend', () => touchControls.right = false);
            document.getElementById('jumpButton').addEventListener('touchstart', () => touchControls.jump = true);
            document.getElementById('jumpButton').addEventListener('touchend', () => touchControls.jump = false);
            document.getElementById('divineButton').addEventListener('touchstart', () => touchControls.divine = true);
            document.getElementById('divineButton').addEventListener('touchend', () => touchControls.divine = false);
            
            // Prevent default touch behavior
            document.addEventListener('touchmove', (e) => {
                if (gameActive) e.preventDefault();
            }, { passive: false });
        }

        // Game State Management
        function startNewGame() {
            if (hasSavedGame() && !confirm("Start new journey? This will overwrite your current progress in slot 1.")) {
                return;
            }
            
            currentLevel = 0;
            resetPlayer();
            saveSystem.currentSlot = 1;
            document.getElementById('mainMenu').style.display = 'none';
            startLevel(currentLevel);
        }

        function hasSavedGame() {
            for (let i = 1; i <= 3; i++) {
                if (saveSystem.loadFromStorage(i)) return true;
            }
            return false;
        }

        function showSaveSlots() {
            saveSystem.updateSaveSlotDisplays();
            document.getElementById('mainMenu').style.display = 'none';
            document.getElementById('saveSlots').style.display = 'flex';
        }

        function saveToSlot(slot) {
            if (gameActive) {
                saveSystem.saveToStorage(slot, saveSystem.createSaveData());
                alert(`Game saved to Slot ${slot}!`);
            } else {
                alert("No game in progress to save!");
            }
        }

        function loadFromSlot(slot) {
            if (saveSystem.loadGame(slot)) {
                document.getElementById('saveSlots').style.display = 'none';
                document.getElementById('mainMenu').style.display = 'none';
                startLevel(currentLevel);
            }
        }

        function quickSave() {
            if (gameActive) {
                saveSystem.quickSave();
            } else {
                alert("No game in progress to save!");
            }
        }

        function returnToMenu() {
            gameActive = false;
            document.getElementById('levelComplete').style.display = 'none';
            document.getElementById('gameOver').style.display = 'none';
            document.getElementById('mainMenu').style.display = 'flex';
            document.getElementById('saveSlots').style.display = 'none';
            checkForSavedGame();
        }

        function retryLevel() {
            resetPlayer();
            document.getElementById('gameOver').style.display = 'none';
            startLevel(currentLevel);
        }

        function nextLevel() {
            // Auto-save progress
            saveSystem.quickSave();
            
            currentLevel++;
            if (currentLevel >= REALMS.length) {
                returnToMenu();
                alert("Congratulations! You have completed all realms and restored balance to the universe!");
            } else {
                resetPlayer();
                document.getElementById('levelComplete').style.display = 'none';
                startLevel(currentLevel);
            }
        }

        function resetPlayer() {
            player.health = player.maxHealth;
            player.divine = 0;
            player.divineActive = false;
            player.divineDuration = 0;
            player.relicsCollected = 0;
            player.x = canvas.width / 4;
            player.y = canvas.height - 150;
            player.velocityY = 0;
            player.jumping = false;
            player.facingRight = true;
            
            chariot.x = player.x - 30;
            chariot.y = player.y + 50;
            chariot.speedBoost = saveSystem.currentSlot ? chariot.speedBoost : 0;
            chariot.protection = saveSystem.currentSlot ? chariot.protection : 0;
            
            updateUI();
        }

        // Level Management
        function startLevel(levelIndex) {
            currentLevel = levelIndex;
            const level = REALMS[levelIndex];
            
            // Reset game objects
            enemies = [];
            obstacles = [];
            relics = [];
            platforms = [];
            backgroundLayers = [];
            
            // Set up level
            createLevel(level);
            
            // Show cutscene
            showCutscene(level.name, level.description);
            
            // Start game loop
            gameActive = true;
            requestAnimationFrame(gameLoop);
        }

        function createLevel(level) {
            // Create background layers for parallax effect
            for (let i = 0; i < 3; i++) {
                backgroundLayers.push({
                    x: 0,
                    y: 0,
                    width: canvas.width * 3,
                    height: canvas.height,
                    speed: 0.2 + (i * 0.2),
                    color: shadeColor(level.bgColor, -20 * i)
                });
            }
            
            // Create platforms
            const platformCount = 10 + (currentLevel * 2);
            for (let i = 0; i < platformCount; i++) {
                const width = 100 + Math.random() * 200;
                const x = Math.random() * canvas.width * 3;
                const y = canvas.height - 100 - (Math.random() * canvas.height / 2);
                
                platforms.push({
                    x: x,
                    y: y,
                    width: width,
                    height: 20,
                    type: i % 3 === 0 ? "moving" : "static",
                    moveDirection: Math.random() > 0.5 ? 1 : -1,
                    moveDistance: 50 + Math.random() * 100
                });
            }
            
            // Ground platform
            platforms.push({
                x: 0,
                y: canvas.height - 50,
                width: canvas.width * 3,
                height: 50,
                type: "static"
            });
            
            // Create enemies
            const enemyCount = 5 + currentLevel * 2;
            for (let i = 0; i < enemyCount; i++) {
                const type = level.enemyTypes[Math.floor(Math.random() * level.enemyTypes.length)];
                const x = 500 + Math.random() * (canvas.width * 2);
                const y = canvas.height - 150 - (Math.random() * canvas.height / 2);
                
                enemies.push({
                    x: x,
                    y: y,
                    width: 40,
                    height: 60,
                    speed: 1 + Math.random() * 2 + (currentLevel * 0.5),
                    health: 30 + (currentLevel * 10),
                    type: type,
                    direction: Math.random() > 0.5 ? 1 : -1,
                    attackCooldown: 0
                });
            }
            
            // Create obstacles
            const obstacleCount = 3 + currentLevel;
            for (let i = 0; i < obstacleCount; i++) {
                const type = level.obstacles[Math.floor(Math.random() * level.obstacles.length)];
                const x = 400 + Math.random() * (canvas.width * 2.5);
                const y = canvas.height - 100 - (Math.random() * canvas.height / 3);
                
                obstacles.push({
                    x: x,
                    y: y,
                    width: 80 + Math.random() * 100,
                    height: type === "fire" ? 60 : 30,
                    type: type,
                    active: true,
                    animationFrame: 0
                });
            }
            
            // Create relics
            for (let i = 0; i < level.relics; i++) {
                const x = 300 + Math.random() * (canvas.width * 2.7);
                const y = canvas.height - 200 - (Math.random() * canvas.height / 2);
                
                relics.push({
                    x: x,
                    y: y,
                    width: 30,
                    height: 30,
                    collected: false,
                    type: ["power", "health", "divine"][Math.floor(Math.random() * 3)]
                });
            }
            
            // Update UI
            document.getElementById('relicsCollected').textContent = `Relics: 0/${level.relics}`;
        }

        function shadeColor(color, percent) {
            let R = parseInt(color.substring(1, 3), 16);
            let G = parseInt(color.substring(3, 5), 16);
            let B = parseInt(color.substring(5, 7), 16);

            R = parseInt(R * (100 + percent) / 100);
            G = parseInt(G * (100 + percent) / 100);
            B = parseInt(B * (100 + percent) / 100);

            R = (R < 255) ? R : 255;
            G = (G < 255) ? G : 255;
            B = (B < 255) ? B : 255;

            return `#${R.toString(16).padStart(2, '0')}${G.toString(16).padStart(2, '0')}${B.toString(16).padStart(2, '0')}`;
        }

        // Cutscene System
        function showCutscene(title, text) {
            document.getElementById('cutsceneText').textContent = text;
            document.getElementById('cutsceneImage').src = `https://example.com/${title.toLowerCase().replace(' ', '_')}.jpg`;
            document.getElementById('cutsceneContainer').style.opacity = '1';
            document.getElementById('cutsceneContainer').style.pointerEvents = 'auto';
        }

        function continueCutscene() {
            document.getElementById('cutsceneContainer').style.opacity = '0';
            document.getElementById('cutsceneContainer').style.pointerEvents = 'none';
        }

        // Game Loop
        function gameLoop() {
            if (!gameActive) return;
            
            // Clear canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Update game state
            updatePlayer();
            updateEnemies();
            updateObstacles();
            updateCamera();
            
            // Draw game
            drawBackground();
            drawPlatforms();
            drawRelics();
            drawObstacles();
            drawEnemies();
            drawPlayer();
            drawChariot();
            
            // Check game conditions
            checkCollisions();
            checkLevelCompletion();
            
            // Continue loop
            requestAnimationFrame(gameLoop);
        }

        function updatePlayer() {
            // Horizontal movement
            let moveX = 0;
            
            if (keys['ArrowLeft'] || touchControls.left) {
                moveX = -player.speed;
                player.facingRight = false;
            }
            if (keys['ArrowRight'] || touchControls.right) {
                moveX = player.speed;
                player.facingRight = true;
            }
            
            // Apply speed boost from chariot
            moveX *= (1 + chariot.speedBoost * 0.5);
            
            player.x += moveX;
            chariot.x += moveX;
            
            // Jumping
            if ((keys[' '] || touchControls.jump) && !player.jumping) {
                player.velocityY = -15;
                player.jumping = true;
            }
            
            // Gravity
            player.velocityY += 0.8;
            player.y += player.velocityY;
            
            // Platform collision
            player.jumping = true;
            for (const platform of platforms) {
                if (player.x + player.width > platform.x && 
                    player.x < platform.x + platform.width &&
                    player.y + player.height > platform.y && 
                    player.y + player.height < platform.y + platform.height + player.velocityY &&
                    player.velocityY > 0) {
                    
                    player.y = platform.y - player.height;
                    player.velocityY = 0;
                    player.jumping = false;
                }
            }
            
            // Divine power activation
            if ((keys['d'] || touchControls.divine) && player.divine >= 100 && !player.divineActive) {
                player.divineActive = true;
                player.divineDuration = 300;
                player.divine = 0;
                updateUI();
            }
            
            // Divine power countdown
            if (player.divineActive) {
                player.divineDuration--;
                if (player.divineDuration <= 0) {
                    player.divineActive = false;
                }
            }
            
            // Screen boundaries
            if (player.x < 0) player.x = 0;
            if (player.x > canvas.width * 3 - player.width) player.x = canvas.width * 3 - player.width;
            if (player.y > canvas.height) {
                player.health = 0;
                updateUI();
            }
        }

        function updateEnemies() {
            for (const enemy of enemies) {
                enemy.x += enemy.speed * enemy.direction;
                
                if (Math.random() < 0.01 || 
                    (enemy.direction === -1 && enemy.x < 0) || 
                    (enemy.direction === 1 && enemy.x > canvas.width * 3 - enemy.width)) {
                    enemy.direction *= -1;
                }
                
                if (enemy.attackCooldown > 0) {
                    enemy.attackCooldown--;
                }
            }
        }

        function updateObstacles() {
            for (const obstacle of obstacles) {
                obstacle.animationFrame = (obstacle.animationFrame + 1) % 60;
            }
        }

        function updateCamera() {
            camera.x = player.x - canvas.width / 4;
            camera.y = player.y - canvas.height / 2;
            
            if (camera.x < 0) camera.x = 0;
            if (camera.x > canvas.width * 3 - canvas.width) camera.x = canvas.width * 3 - canvas.width;
            if (camera.y < 0) camera.y = 0;
            if (camera.y > canvas.height - canvas.height) camera.y = canvas.height - canvas.height;
        }

        function drawBackground() {
            const levelColor = REALMS[currentLevel].bgColor;
            
            const skyGradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
            skyGradient.addColorStop(0, shadeColor(levelColor, -50));
            skyGradient.addColorStop(1, levelColor);
            ctx.fillStyle = skyGradient;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            for (let i = 0; i < backgroundLayers.length; i++) {
                const layer = backgroundLayers[i];
                const x = -camera.x * layer.speed;
                
                ctx.fillStyle = layer.color;
                ctx.fillRect(x, 0, layer.width, layer.height);
                
                ctx.fillStyle = shadeColor(layer.color, -10);
                for (let j = 0; j < 10; j++) {
                    const height = 100 + Math.random() * 200;
                    ctx.beginPath();
                    ctx.moveTo(x + (j * 300), canvas.height);
                    ctx.lineTo(x + (j * 300) + 150, canvas.height - height);
                    ctx.lineTo(x + (j * 300) + 300, canvas.height);
                    ctx.fill();
                }
            }
        }

        function drawPlatforms() {
            ctx.fillStyle = '#8B4513';
            
            for (const platform of platforms) {
                const x = platform.x - camera.x;
                const y = platform.y - camera.y;
                
                if (x + platform.width < 0 || x > canvas.width) continue;
                
                ctx.fillRect(x, y, platform.width, platform.height);
                
                ctx.strokeStyle = '#A0522D';
                ctx.lineWidth = 2;
                for (let i = 0; i < platform.width; i += 20) {
                    ctx.beginPath();
                    ctx.moveTo(x + i, y);
                    ctx.lineTo(x + i, y + platform.height);
                    ctx.stroke();
                }
            }
        }

        function drawRelics() {
            for (const relic of relics) {
                if (relic.collected) continue;
                
                const x = relic.x - camera.x;
                const y = relic.y - camera.y;
                
                if (x + relic.width < 0 || x > canvas.width) continue;
                
                if (relic.type === "power") {
                    ctx.fillStyle = '#FF0000';
                } else if (relic.type === "health") {
                    ctx.fillStyle = '#00FF00';
                } else {
                    ctx.fillStyle = '#FFFF00';
                }
                
                ctx.beginPath();
                ctx.moveTo(x + relic.width/2, y);
                ctx.lineTo(x + relic.width, y + relic.height/2);
                ctx.lineTo(x + relic.width/2, y + relic.height);
                ctx.lineTo(x, y + relic.height/2);
                ctx.closePath();
                ctx.fill();
                
                if (Date.now() % 1000 < 500) {
                    ctx.shadowColor = ctx.fillStyle;
                    ctx.shadowBlur = 15;
                    ctx.fill();
                    ctx.shadowBlur = 0;
                }
            }
        }

        function drawObstacles() {
            for (const obstacle of obstacles) {
                if (!obstacle.active) continue;
                
                const x = obstacle.x - camera.x;
                const y = obstacle.y - camera.y;
                
                if (x + obstacle.width < 0 || x > canvas.width) continue;
                
                if (obstacle.type === "fire") {
                    ctx.fillStyle = `hsl(${obstacle.animationFrame * 6}, 100%, 50%)`;
                    ctx.beginPath();
                    ctx.moveTo(x + obstacle.width/2, y + obstacle.height);
                    for (let i = 0; i < 10; i++) {
                        const px = x + (i * obstacle.width/10);
                        const py = y + Math.sin(i + obstacle.animationFrame/10) * 20;
                        ctx.lineTo(px, py);
                    }
                    ctx.lineTo(x + obstacle.width/2, y + obstacle.height);
                    ctx.fill();
                } else {
                    ctx.fillStyle = '#999999';
                    ctx.fillRect(x, y, obstacle.width, obstacle.height);
                }
            }
        }

        function drawEnemies() {
            for (const enemy of enemies) {
                const x = enemy.x - camera.x;
                const y = enemy.y - camera.y;
                
                if (x + enemy.width < 0 || x > canvas.width) continue;
                
                if (enemy.type.includes("rakshasa")) {
                    ctx.fillStyle = '#990000';
                } else if (enemy.type.includes("naga")) {
                    ctx.fillStyle = '#006600';
                } else if (enemy.type.includes("deva")) {
                    ctx.fillStyle = '#0000FF';
                } else {
                    ctx.fillStyle = '#663300';
                }
                
                ctx.fillRect(x, y, enemy.width, enemy.height);
                
                const healthPercent = enemy.health / (30 + (currentLevel * 10));
                ctx.fillStyle = '#FF0000';
                ctx.fillRect(x, y - 10, enemy.width, 5);
                ctx.fillStyle = '#00FF00';
                ctx.fillRect(x, y - 10, enemy.width * healthPercent, 5);
                
                ctx.fillStyle = '#FFFFFF';
                const eyeX = enemy.direction > 0 ? x + enemy.width - 15 : x + 5;
                ctx.beginPath();
                ctx.arc(eyeX, y + 15, 5, 0, Math.PI * 2);
                ctx.fill();
                ctx.fillStyle = '#000000';
                ctx.beginPath();
                ctx.arc(eyeX, y + 15, 2, 0, Math.PI * 2);
                ctx.fill();
            }
        }

        function drawPlayer() {
            const x = player.x - camera.x;
            const y = player.y - camera.y;
            
            ctx.fillStyle = player.divineActive ? '#FFFF00' : '#0000FF';
            
            // Body
            ctx.fillRect(x + player.width/2 - 10, y + 20, 20, 40);
            
            // Head
            ctx.beginPath();
            ctx.arc(x + player.width/2, y + 10, 10, 0, Math.PI * 2);
            ctx.fill();
            
            // Arms
            ctx.lineWidth = 5;
            ctx.beginPath();
            ctx.moveTo(x + player.width/2 - 10, y + 30);
            ctx.lineTo(x + player.width/2 - 25, y + 20);
            ctx.moveTo(x + player.width/2 + 10, y + 30);
            ctx.lineTo(x + player.width/2 + 25, y + 20);
            ctx.stroke();
            
            // Legs
            ctx.beginPath();
            ctx.moveTo(x + player.width/2 - 10, y + 60);
            ctx.lineTo(x + player.width/2 - 15, y + 80);
            ctx.moveTo(x + player.width/2 + 10, y + 60);
            ctx.lineTo(x + player.width/2 + 15, y + 80);
            ctx.stroke();
            
            if (player.divineActive) {
                ctx.shadowColor = '#FFFF00';
                ctx.shadowBlur = 20;
                ctx.beginPath();
                ctx.arc(x + player.width/2, y + player.height/2, 30, 0, Math.PI * 2);
                ctx.fill();
                ctx.shadowBlur = 0;
            }
        }

        function drawChariot() {
            const x = chariot.x - camera.x;
            const y = chariot.y - camera.y;
            
            ctx.fillStyle = '#8B0000';
            ctx.fillRect(x, y, chariot.width, chariot.height);
            
            ctx.strokeStyle = '#DAA520';
            ctx.lineWidth = 3;
            ctx.strokeRect(x, y, chariot.width, chariot.height);
            
            ctx.fillStyle = '#000000';
            ctx.beginPath();
            ctx.arc(x + 20, y + chariot.height, 15, 0, Math.PI * 2);
            ctx.fill();
            ctx.beginPath();
            ctx.arc(x + chariot.width - 20, y + chariot.height, 15, 0, Math.PI * 2);
            ctx.fill();
            
            for (let i = 0; i < chariot.horses; i++) {
                const horseX = x - 40 - (i * 30);
                const horseY = y + 20;
                
                ctx.fillStyle = '#8B4513';
                ctx.fillRect(horseX, horseY, 30, 15);
                
                ctx.beginPath();
                ctx.arc(horseX + 35, horseY + 5, 5, 0, Math.PI * 2);
                ctx.fill();
                
                ctx.fillRect(horseX + 5, horseY + 15, 5, 15);
                ctx.fillRect(horseX + 20, horseY + 15, 5, 15);
            }
            
            if (chariot.protection > 0) {
                ctx.strokeStyle = '#00FFFF';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.arc(x + chariot.width/2, y + chariot.height/2, chariot.width/2 + 10, 0, Math.PI * 2);
                ctx.stroke();
            }
        }

        function checkCollisions() {
            for (const enemy of enemies) {
                if (rectCollision(player, enemy) && enemy.attackCooldown === 0) {
                    if (!player.divineActive) {
                        player.health -= 10;
                        updateUI();
                        
                        player.x += enemy.direction * 20;
                        player.velocityY = -5;
                        
                        if (player.health <= 0) {
                            gameOver();
                            return;
                        }
                    }
                    
                    enemy.attackCooldown = 60;
                }
            }
            
            for (const obstacle of obstacles) {
                if (obstacle.active && rectCollision(player, obstacle)) {
                    if (obstacle.type === "fire") {
                        if (!player.divineActive) {
                            player.health -= 5;
                            updateUI();
                            
                            if (player.health <= 0) {
                                gameOver();
                                return;
                            }
                        }
                    } else {
                        if (!player.divineActive) {
                            player.health = 0;
                            updateUI();
                            gameOver();
                            return;
                        }
                    }
                }
            }
            
            for (const relic of relics) {
                if (!relic.collected && rectCollision(player, relic)) {
                    relic.collected = true;
                    player.relicsCollected++;
                    
                    if (relic.type === "power") {
                        chariot.speedBoost += 0.1;
                    } else if (relic.type === "health") {
                        player.health = Math.min(player.maxHealth, player.health + 20);
                    } else if (relic.type === "divine") {
                        player.divine = Math.min(100, player.divine + 25);
                    }
                    
                    player.score += 100 * (currentLevel + 1);
                    
                    updateUI();
                }
            }
        }

        function rectCollision(rect1, rect2) {
            return rect1.x < rect2.x + rect2.width &&
                   rect1.x + rect1.width > rect2.x &&
                   rect1.y < rect2.y + rect2.height &&
                   rect1.y + rect1.height > rect2.y;
        }

        function checkLevelCompletion() {
            if (player.x > canvas.width * 3 - player.width - 100) {
                levelComplete();
            }
        }

        function levelComplete() {
            gameActive = false;
            
            const level = REALMS[currentLevel];
            const relicsBonus = (player.relicsCollected / level.relics) * 5000;
            const healthBonus = (player.health / player.maxHealth) * 3000;
            const totalScore = player.score + relicsBonus + healthBonus;
            
            // Auto-save progress
            saveSystem.quickSave();
            
            document.getElementById('levelComplete').style.display = 'flex';
            document.getElementById('levelCompleteStats').innerHTML = `
                <p>Relics Collected: ${player.relicsCollected}/${level.relics}</p>
                <p>Health Remaining: ${player.health}/${player.maxHealth}</p>
                <p>Final Score: ${Math.floor(totalScore)}</p>
            `;
        }

        function gameOver() {
            gameActive = false;
            
            document.getElementById('gameOver').style.display = 'flex';
            document.getElementById('gameOverStats').innerHTML = `
                <p>Realm: ${REALMS[currentLevel].name}</p>
                <p>Relics Collected: ${player.relicsCollected}/${REALMS[currentLevel].relics}</p>
                <p>Final Score: ${player.score}</p>
            `;
        }

        function updateUI() {
            const healthPercent = (player.health / player.maxHealth) * 100;
            document.getElementById('healthFill').style.width = healthPercent + '%';
            
            document.getElementById('divineFill').style.width = player.divine + '%';
            
            document.getElementById('scoreDisplay').textContent = `Divine Power: ${player.score}`;
            document.getElementById('relicsCollected').textContent = `Relics: ${player.relicsCollected}/${REALMS[currentLevel].relics}`;
        }

        function showControls() {
            alert("Controls:\n\nArrow Keys: Move\nSpace: Jump\nD: Activate Divine Power\n\nQuick Save: 💾 button\nSave Slots: 📁 button\n\nOn mobile, use the on-screen buttons.");
        }

        function showCredits() {
            alert("Rath Yatra – Race of Realms\n\nDeveloped by [Your Name]\n\nArt by [Artist Name]\nMusic: 'Astradhari - Epic Indian Music'");
        }

        function showLevelSelect() {
            alert("Level selection would appear here in a full implementation");
        }

        // Start the game
        init();

let isDay = true;

function toggleDayNight() {
  document.body.style.transition = "background-color 2s";
  if (isDay) {
    document.body.style.backgroundColor = "#0b1b2a"; // Night
  } else {
    document.body.style.backgroundColor = "#f5e8c4"; // Day
  }
  isDay = !isDay;
}

// Switch every 30 seconds
setInterval(toggleDayNight, 30000);

function applyWeather(type) {
  const weather = document.getElementById("weatherEffect");
  weather.className = ""; // reset
  if (type === "rain") weather.classList.add("rain");
  else if (type === "sandstorm") {
    weather.style.backgroundImage = "url('https://i.ibb.co/GsVHbYC/sandstorm.gif')";
    weather.style.opacity = 0.4;
  } else if (type === "divine") {
    weather.style.backgroundImage = "url('https://i.ibb.co/TLqXZbF/light-rays.gif')";
    weather.style.opacity = 0.6;
  }
}

// Change weather every 45 seconds
const weatherTypes = ["rain", "sandstorm", "divine", "clear"];
setInterval(() => {
  const type = weatherTypes[Math.floor(Math.random() * weatherTypes.length)];
  applyWeather(type);
}, 45000);

    </script>
</body>
</html>
