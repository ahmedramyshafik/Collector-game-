<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Ahmed AI | Easy Collector</title>
    <style>
        :root {
            --primary: #0f172a;
            --player-color: #38bdf8;
            --item-color: #fbbf24;
            --enemy-color: #f87171;
            --menu-btn: #334151;
            --input-bg: #1e293b;
        }
        body {
            margin: 0;
            background-color: #f1f5f9;
            font-family: 'Segoe UI', sans-serif;
            overflow: hidden;
            touch-action: none;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        #game-container {
            width: 100%;
            max-width: 450px;
            height: 90vh;
            background: var(--primary);
            position: relative;
            border-radius: 20px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.3);
            overflow: hidden;
            border: 4px solid #334151;
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="80" height="60" viewBox="0 0 80 60"><text x="50%" y="50%" text-anchor="middle" font-size="6" fill="rgba(255,255,255,0.04)" font-family="Arial" font-weight="900" transform="rotate(-25 40 30)">AHMED AI</text></svg>');
        }
        .score-board {
            position: absolute;
            top: 15px;
            left: 0;
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 15px;
            box-sizing: border-box;
            color: white;
            font-size: 14px;
            font-weight: bold;
            z-index: 10;
        }
        #pause-btn {
            background: rgba(255, 255, 255, 0.2);
            border: none;
            border-radius: 8px;
            padding: 5px 10px;
            font-size: 16px;
            cursor: pointer;
            color: white;
            backdrop-filter: blur(5px);
        }
        #player {
            width: 60px;
            height: 60px;
            background: var(--player-color);
            position: absolute;
            bottom: 30px;
            left: calc(50% - 30px);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            font-weight: bold;
            color: var(--primary);
            box-shadow: 0 0 20px var(--player-color);
            transition: transform 0.1s ease-out;
        }
        .falling-item {
            position: absolute;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
        }
        .overlay {
            position: absolute;
            inset: 0;
            background: rgba(15, 23, 42, 0.95);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: white;
            z-index: 100;
            text-align: center;
        }
        .menu-btn {
            background: var(--item-color);
            border: none;
            padding: 12px 30px;
            border-radius: 50px;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
            width: 220px;
            box-shadow: 0 4px 15px rgba(251, 191, 36, 0.3);
            transition: transform 0.1s;
        }
        .menu-btn:active {
            transform: scale(0.95);
        }
        .btn-unlimited { background: var(--player-color); box-shadow: 0 4px 15px rgba(56, 189, 248, 0.3); margin-bottom: 10px; }
        .btn-back { background: var(--menu-btn); box-shadow: none; width: auto; font-size: 1rem; padding: 10px 25px; margin-top: 20px; }
        .input-group {
            background: var(--input-bg);
            padding: 15px;
            border-radius: 15px;
            margin-bottom: 15px;
            border: 2px solid var(--menu-btn);
            width: 240px;
            box-sizing: border-box;
        }
        .input-group label {
            display: block;
            font-size: 0.9rem;
            color: #94a3b8;
            margin-bottom: 8px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .input-group input {
            width: 100%;
            background: var(--primary);
            border: 2px solid var(--player-color);
            color: white;
            font-size: 1.2rem;
            font-weight: bold;
            text-align: center;
            padding: 5px;
            border-radius: 8px;
            outline: none;
            box-sizing: border-box;
        }
    </style>
</head>
<body>

<div id="game-container">
    <div class="score-board" id="top-bar"></div>
    
    <div id="start-screen" class="overlay">
        <h1 style="color: var(--player-color); margin: 0; font-size: 2.5rem;">AHMED AI</h1>
        <p style="margin-bottom: 20px;">Collect Batteries 🔋 | Avoid Gears ⚙️</p>
        
        <div id="menu-main">
            <button class="menu-btn btn-unlimited" onclick="startGame('unlimited')">UNLIMITED ♾️</button>
            <button class="menu-btn" onclick="showLimited()">LIMITED MODES ⏱️</button>
        </div>

        <div id="menu-limited" style="display: none;">
            <div class="input-group">
                <label>Time Rush (Seconds)</label>
                <input type="number" id="time-input" value="60" min="10" max="300" step="10">
                <button class="menu-btn" onclick="startGame('time')" style="width: 100%; font-size: 1rem; margin-top: 10px;">PLAY TIME RUSH ⏳</button>
            </div>

            <div class="input-group">
                <label>Target Mode (Energy)</label>
                <input type="number" id="target-input" value="500" min="50" max="5000" step="50">
                <button class="menu-btn" onclick="startGame('target')" style="width: 100%; font-size: 1rem; margin-top: 10px;">PLAY TARGET 🎯</button>
            </div>

            <button class="menu-btn btn-back" onclick="showMain()">🔙 BACK</button>
        </div>
    </div>

    <div id="pause-screen" class="overlay" style="display: none;">
        <h1 style="color: var(--item-color); margin: 0;">PAUSED</h1>
        <button class="menu-btn btn-unlimited" onclick="togglePause()">RESUME ▶️</button>
        <button class="menu-btn btn-back" onclick="quitToMenu()">QUIT TO MENU 🚪</button>
    </div>

    <div id="game-over-screen" class="overlay" style="display: none;">
        <h1 id="game-over-title" style="margin: 0; font-size: 2.5rem;">GAME OVER</h1>
        <p id="game-over-msg" style="margin: 15px 0; font-size: 1.2rem; color: #94a3b8;"></p>
        <button class="menu-btn btn-unlimited" onclick="restartGame()">PLAY AGAIN 🔄</button>
        <button class="menu-btn btn-back" onclick="quitToMenu()">MAIN MENU 🚪</button>
    </div>

    <div id="player">🤖</div>
</div>

<script>
    const container = document.getElementById('game-container');
    const player = document.getElementById('player');
    const topBar = document.getElementById('top-bar');
    const startScreen = document.getElementById('start-screen');
    const menuMain = document.getElementById('menu-main');
    const menuLimited = document.getElementById('menu-limited');
    const pauseScreen = document.getElementById('pause-screen');
    const gameOverScreen = document.getElementById('game-over-screen');
    const gameOverTitle = document.getElementById('game-over-title');
    const gameOverMsg = document.getElementById('game-over-msg');

    let score = 0;
    let level = 1;
    let gameActive = false;
    let isPaused = false;
    let playerX = container.offsetWidth / 2 - 30;
    let items = [];
    let speed = 3;
    let currentMode = 'unlimited'; 
    let currentLimit = 0; 

    let lastTime = 0;
    let spawnTimer = 0;
    let levelTimer = 0;
    let modeTimer = 0; 

    container.addEventListener('mousemove', (e) => movePlayer(e.clientX));
    container.addEventListener('touchmove', (e) => {
        if(!isPaused) e.preventDefault();
        movePlayer(e.touches[0].clientX);
    }, {passive: false});

    function movePlayer(clientX) {
        if (!gameActive || isPaused) return;
        const rect = container.getBoundingClientRect();
        let x = clientX - rect.left - 30;
        if (x < 0) x = 0;
        if (x > container.offsetWidth - 60) x = container.offsetWidth - 60;
        playerX = x;
        player.style.left = playerX + 'px';
    }

    function showLimited() {
        menuMain.style.display = 'none';
        menuLimited.style.display = 'block';
    }
    
    function showMain() {
        menuLimited.style.display = 'none';
        menuMain.style.display = 'block';
    }

    function togglePause() {
        if (!gameActive) return;
        isPaused = !isPaused;
        
        if (isPaused) {
            pauseScreen.style.display = 'flex';
        } else {
            pauseScreen.style.display = 'none';
            lastTime = 0; 
            requestAnimationFrame(update);
        }
    }

    function quitToMenu() {
        gameActive = false;
        pauseScreen.style.display = 'none';
        gameOverScreen.style.display = 'none';
        startScreen.style.display = 'flex';
        showMain();
        items.forEach(i => i.remove());
        items = [];
    }

    function getHighScore(mode, limit) {
        if (mode === 'unlimited') return localStorage.getItem('ahmed_ai_best_unlimited') || 0;
        if (mode === 'time') return localStorage.getItem(`ahmed_ai_best_time_${limit}`) || 0;
        if (mode === 'target') return localStorage.getItem(`ahmed_ai_best_target_${limit}`) || "999.9";
    }

    function saveHighScore(mode, limit, value) {
        if (mode === 'unlimited') localStorage.setItem('ahmed_ai_best_unlimited', value);
        if (mode === 'time') localStorage.setItem(`ahmed_ai_best_time_${limit}`, value);
        if (mode === 'target') localStorage.setItem(`ahmed_ai_best_target_${limit}`, value);
    }

    function startGame(mode) {
        currentMode = mode;
        score = 0;
        level = 1;
        speed = 3;
        gameActive = true;
        isPaused = false;
        
        startScreen.style.display = 'none';
        pauseScreen.style.display = 'none';
        gameOverScreen.style.display = 'none';
        
        items.forEach(i => i.remove());
        items = [];
        
        lastTime = 0; 
        spawnTimer = 0;
        levelTimer = 0;
        
        if (currentMode === 'time') {
            currentLimit = parseInt(document.getElementById('time-input').value) || 60;
            modeTimer = currentLimit * 1000;
        } else if (currentMode === 'target') {
            currentLimit = parseInt(document.getElementById('target-input').value) || 500;
            modeTimer = 0;
        } else {
            currentLimit = 0;
        }

        setupScoreboard();
        spawnItem();
        requestAnimationFrame(update);
    }

    function restartGame() {
        startGame(currentMode);
    }

    function setupScoreboard() {
        let html = '';
        let best = getHighScore(currentMode, currentLimit);
        
        if (currentMode === 'unlimited') {
            html = `<span>LVL: <span id="ui-level">1</span> | ENG: <span id="ui-score">0</span> | BEST: ${best}</span>`;
        } else if (currentMode === 'time') {
            html = `<span>⏳ <span id="ui-timer">${currentLimit.toFixed(1)}</span>s | ENG: <span id="ui-score">0</span> | BEST: ${best}</span>`;
        } else if (currentMode === 'target') {
            let bestDisplay = best === "999.9" ? "--" : `${best}s`;
            html = `<span>ENG: <span id="ui-score">0</span>/${currentLimit} | ⏳ <span id="ui-timer">0.0</span>s | BEST: ${bestDisplay}</span>`;
        }
        html += `<button id="pause-btn" onclick="togglePause()">⏸️</button>`;
        topBar.innerHTML = html;
    }

    function updateUI() {
        const uiScore = document.getElementById('ui-score');
        const uiLevel = document.getElementById('ui-level');
        const uiTimer = document.getElementById('ui-timer');
        
        if (uiScore) uiScore.innerText = score;
        if (uiLevel && currentMode === 'unlimited') uiLevel.innerText = level;
        
        if (uiTimer) {
            let seconds = (modeTimer / 1000).toFixed(1);
            uiTimer.innerText = Math.max(0, seconds);
        }
    }

    function spawnItem() {
        const item = document.getElementById('game-container').appendChild(document.createElement('div'));
        const isEnemy = Math.random() > 0.7;
        item.className = 'falling-item';
        item.innerHTML = isEnemy ? '⚙️' : '🔋';
        item.dataset.type = isEnemy ? 'enemy' : 'point';
        item.style.left = Math.random() * (container.offsetWidth - 40) + 'px';
        item.style.top = '-50px';
        items.push(item);
    }

    function update(timestamp) {
        if (!gameActive || isPaused) return;

        if (!lastTime) {
            lastTime = timestamp;
            requestAnimationFrame(update);
            return;
        }

        let deltaTime = timestamp - lastTime;
        lastTime = timestamp;

        if (deltaTime > 100) {
            requestAnimationFrame(update);
            return;
        }

        const frameRatio = deltaTime / 16.666;
        spawnTimer += deltaTime;
        
        if (currentMode === 'unlimited') {
            levelTimer += deltaTime;
            if (levelTimer >= 10000) {
                level++;
                speed += 1;
                levelTimer = 0;
            }
        } else if (currentMode === 'time') {
            modeTimer -= deltaTime;
            if (modeTimer <= 0) {
                endGame('time_up');
                return;
            }
            speed = 3 + (((currentLimit * 1000) - modeTimer) / 15000); 
        } else if (currentMode === 'target') {
            modeTimer += deltaTime;
            speed = 3 + (score / 100);
        }

        updateUI();

        let spawnInterval = Math.max(300, 800 - Math.min(score, 500));
        if (spawnTimer >= spawnInterval) {
            spawnItem();
            spawnTimer = 0;
        }

        const pRect = player.getBoundingClientRect();

        for (let i = items.length - 1; i >= 0; i--) {
            let item = items[i];
            let top = parseFloat(item.style.top) || -50;
            
            top += (speed * frameRatio);
            item.style.top = top + 'px';
            
            const iRect = item.getBoundingClientRect();

            if (iRect.bottom > pRect.top && iRect.top < pRect.bottom && iRect.left < pRect.right && iRect.right > pRect.left) {
                if (item.dataset.type === 'point') {
                    score += 10;
                    
                    if (currentMode === 'target' && score >= currentLimit) {
                        endGame('target_reached');
                        return;
                    }

                    item.remove();
                    items.splice(i, 1);
                } else {
                    endGame('crash');
                    return;
                }
            }
            else if (top > container.offsetHeight) {
                item.remove();
                items.splice(i, 1);
            }
        }

        if (gameActive && !isPaused) {
            requestAnimationFrame(update);
        }
    }

    function endGame(reason) {
        gameActive = false;
        let title = "GAME OVER";
        let msg = "";
        let currentBest = getHighScore(currentMode, currentLimit);

        if (reason === 'crash') {
            title = "💥 CRASHED!";
            if (currentMode === 'target') {
                msg = `You hit a gear! Target was ${currentLimit}.`;
            } else {
                msg = `Final Score: ${score} Energy`;
                if (score > parseInt(currentBest)) {
                    saveHighScore(currentMode, currentLimit, score);
                    msg += `<br><span style="color:var(--item-color)">🎉 NEW HIGH SCORE!</span>`;
                }
            }
        } else if (reason === 'time_up') {
            title = "⏱️ TIME'S UP!";
            msg = `Final Score: ${score} Energy`;
            if (score > parseInt(currentBest)) {
                saveHighScore(currentMode, currentLimit, score);
                msg += `<br><span style="color:var(--item-color)">🎉 NEW HIGH SCORE!</span>`;
            }
        } else if (reason === 'target_reached') {
            title = "🎯 WINNER!";
            let timeTaken = (modeTimer / 1000).toFixed(1);
            msg = `Reached ${currentLimit} energy in ${timeTaken}s!`;
            if (currentBest === "999.9" || parseFloat(timeTaken) < parseFloat(currentBest)) {
                saveHighScore(currentMode, currentLimit, timeTaken);
                msg += `<br><span style="color:var(--item-color)">🎉 NEW BEST TIME!</span>`;
            }
        }

        gameOverTitle.innerHTML = title;
        gameOverMsg.innerHTML = msg;
        gameOverScreen.style.display = 'flex';
    }
</script>

</body>
</html>

