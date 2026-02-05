<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arena dos Amigos - Sorteio e Placar</title>
    <style>
        :root {
            --bg-dark: #0f172a;
            --card-bg: #1e293b;
            --primary: #22c55e;
            --secondary: #3b82f6;
            --yellow: #facc15;
            --red: #ef4444;
            --text: #f8fafc;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background-color: var(--bg-dark);
            color: var(--text);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        .main-container {
            max-width: 1000px;
            width: 100%;
            flex-grow: 1;
        }

        .card {
            background: var(--card-bg);
            padding: 24px;
            border-radius: 16px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
            margin-bottom: 24px;
        }

        h1 {
            text-align: center;
            font-size: 2.5rem;
            margin-bottom: 30px;
            color: var(--primary);
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        h2 { font-size: 1.25rem; margin-top: 0; color: #94a3b8; }
        h3 { font-size: 1rem; color: var(--text); margin-bottom: 10px; }

        /* Registro de Jogadores */
        .input-row {
            display: flex;
            gap: 12px;
            margin-bottom: 20px;
        }

        input, textarea {
            flex: 1;
            background: #0f172a;
            border: 1px solid #334155;
            color: white;
            padding: 12px 16px;
            border-radius: 8px;
            outline: none;
            font-size: 1rem;
            font-family: inherit;
        }

        textarea { height: 80px; resize: none; }
        input:focus, textarea:focus { border-color: var(--primary); }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 700;
            transition: all 0.2s;
            text-transform: uppercase;
        }

        .btn-add { background: var(--primary); color: #fff; }
        .btn-add:hover { background: #16a34a; transform: translateY(-2px); }
        
        .btn-secondary { background: #334155; color: #fff; }
        .btn-secondary:hover { background: #475569; }

        .btn-start-ready {
            background: #8b5cf6; 
            color: white;
            width: 100%;
            margin-top: 10px;
            font-size: 1.1rem;
        }

        .player-tag-list {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-bottom: 20px;
            min-height: 40px;
            padding: 10px;
            background: rgba(0,0,0,0.1);
            border-radius: 8px;
        }

        .player-tag {
            background: #334155;
            padding: 6px 12px;
            border-radius: 20px;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9rem;
        }

        .player-tag span { cursor: pointer; color: var(--red); font-weight: bold; }

        .btn-draw {
            width: 100%;
            background: var(--secondary);
            color: white;
            font-size: 1.2rem;
            margin-top: 10px;
        }

        /* Dashboard de Partida */
        .match-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            background: #0f172a;
            padding: 20px;
            border-radius: 12px;
        }

        .timer-box {
            text-align: center;
        }

        #timer {
            font-size: 3rem;
            font-family: monospace;
            color: var(--primary);
            display: block;
        }

        .timer-controls button {
            background: #334155;
            color: white;
            border: none;
            padding: 5px 15px;
            border-radius: 4px;
            margin: 0 5px;
            cursor: pointer;
        }

        .btn-undo {
            background: #475569 !important;
            font-size: 0.8rem;
            margin-top: 10px;
        }

        .teams-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
        }

        .team-column {
            background: rgba(15, 23, 42, 0.5);
            padding: 20px;
            border-radius: 12px;
            border: 2px solid transparent;
        }

        .team-a-border { border-color: var(--primary); }
        .team-b-border { border-color: var(--secondary); }

        .score-display {
            font-size: 4rem;
            font-weight: 900;
            text-align: center;
            margin: 10px 0;
        }

        .player-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: var(--card-bg);
            padding: 12px;
            margin-bottom: 8px;
            border-radius: 8px;
            transition: 0.3s;
        }

        .player-info { flex: 1; }
        .player-name { font-weight: bold; display: block; }
        .player-stats-mini { font-size: 0.75rem; color: #94a3b8; }

        .action-btns {
            display: flex;
            gap: 8px;
        }

        .action-btn {
            width: 32px;
            height: 32px;
            border-radius: 4px;
            border: none;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }

        .btn-goal { background: #fff; color: #000; }
        .btn-y-card { background: var(--yellow); color: #000; }
        .btn-r-card { background: var(--red); color: #fff; }

        .log-container {
            margin-top: 30px;
            background: #0f172a;
            padding: 15px;
            border-radius: 8px;
            max-height: 200px;
            overflow-y: auto;
        }

        .log-entry {
            font-size: 0.85rem;
            border-bottom: 1px solid #1e293b;
            padding: 5px 0;
            color: #94a3b8;
        }

        .hidden { display: none; }

        .bulk-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 15px;
        }

        /* Rodapé */
        footer {
            margin-top: 40px;
            padding: 20px;
            text-align: center;
            color: #64748b;
            font-size: 0.9rem;
            border-top: 1px solid #1e293b;
            width: 100%;
        }

        @media (max-width: 768px) {
            .teams-grid, .bulk-grid { grid-template-columns: 1fr; }
            .match-header { flex-direction: column; gap: 20px; }
        }
    </style>
</head>
<body>

<div class="main-container">
    <h1>🏆 Arena Premier</h1>

    <!-- Registro -->
    <div class="card" id="setupCard">
        <h2>Opção 1: Convocação para Sorteio</h2>
        <div class="input-row">
            <input type="text" id="playerName" placeholder="Nome do jogador..." onkeypress="handleEnter(event)">
            <button class="btn btn-add" id="btnAddPlayer">Adicionar</button>
        </div>
        <div class="player-tag-list" id="playerTagList"></div>
        <button class="btn btn-draw" id="btnDraw">Sortear e Iniciar Jogo</button>

        <hr style="border: 0; border-top: 1px solid #334155; margin: 30px 0;">

        <h2>Opção 2: Iniciar com Times Prontos</h2>
        <div class="bulk-grid">
            <div>
                <h3>Time Verde</h3>
                <textarea id="bulkTeamA" placeholder="Nomes do Time Verde (um por linha ou vírgula)..."></textarea>
            </div>
            <div>
                <h3>Time Azul</h3>
                <textarea id="bulkTeamB" placeholder="Nomes do Time Azul (um por linha ou vírgula)..."></textarea>
            </div>
        </div>
        <button class="btn btn-start-ready" id="btnStartReady">Iniciar Jogo com Times Prontos</button>
    </div>

    <!-- Placar e Jogo -->
    <div id="gameDashboard" class="card hidden">
        <div class="match-header">
            <div>
                <h3 style="margin:0">Tempo de Jogo</h3>
                <div class="timer-controls" style="margin-top: 5px;">
                    <button id="playPauseBtn">Iniciar</button>
                    <button id="resetTimerBtn">Reset</button>
                </div>
            </div>
            <div class="timer-box">
                <span id="timer">00:00</span>
                <button class="timer-controls btn btn-undo" id="btnUndoGoal">Anular Último Golo</button>
            </div>
            <div style="text-align: right">
                <!-- Botão Voltar ao Início Ajustado -->
                <button class="btn btn-add" style="background: var(--red)" id="btnExitGame">Voltar ao Início</button>
            </div>
        </div>

        <div class="teams-grid">
            <!-- Time A -->
            <div class="team-column team-a-border">
                <h2 style="color: var(--primary); text-align: center;">TIME VERDE</h2>
                <div class="score-display" id="scoreA">0</div>
                <div id="playersTeamA"></div>
            </div>

            <!-- Time B -->
            <div class="team-column team-b-border">
                <h2 style="color: var(--secondary); text-align: center;">TIME AZUL</h2>
                <div class="score-display" id="scoreB">0</div>
                <div id="playersTeamB"></div>
            </div>
        </div>

        <div class="log-container" id="matchLog">
            <div class="log-entry" id="initialLog">Aguardando início da partida...</div>
        </div>
    </div>
</div>

<footer>
    © 2026 – FUT-FBK – Criado por CLX
</footer>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getFirestore, collection, addDoc, deleteDoc, doc, onSnapshot, query, orderBy } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

    const firebaseConfig = {
      apiKey: "AIzaSyDTM6OzYrYqREizcQlrJx-mTWc-opPaB0o",
      authDomain: "fut-fbk.firebaseapp.com",
      projectId: "fut-fbk",
      storageBucket: "fut-fbk.firebasestorage.app",
      messagingSenderId: "844620142867",
      appId: "1:844620142867:web:44d77a9ac66a807efae2e6"
    };

    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);
    const appId = "fut-fbk-app";

    let playersForDraw = [];
    let activeMatchPlayers = [];
    let goalHistory = []; 

    const playersRef = collection(db, 'artifacts', appId, 'public', 'data', 'players');

    function loadDrawPlayers() {
        onSnapshot(playersRef, (snapshot) => {
            playersForDraw = snapshot.docs.map(doc => ({
                dbId: doc.id,
                ...doc.data()
            }));
            renderTags();
        });
    }

    loadDrawPlayers();

    window.addPlayer = async () => {
        const input = document.getElementById('playerName');
        const name = input.value.trim();
        if (name) {
            await addDoc(playersRef, { 
                name: name, 
                createdAt: Date.now() 
            });
            input.value = ""; 
        }
    };

    window.removePlayer = async (dbId) => {
        const playerDoc = doc(db, 'artifacts', appId, 'public', 'data', 'players', dbId);
        await deleteDoc(playerDoc);
    };

    function renderTags() {
        const list = document.getElementById('playerTagList');
        list.innerHTML = playersForDraw.map(p => `
            <div class="player-tag">
                ${p.name}
                <span onclick="removePlayer('${p.dbId}')">&times;</span>
            </div>
        `).join('');
    }

    function shuffleArray(array) {
        let currentIndex = array.length, randomIndex;
        while (currentIndex != 0) {
            randomIndex = Math.floor(Math.random() * currentIndex);
            currentIndex--;
            [array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
        }
        return array;
    }

    window.drawTeams = () => {
        if (playersForDraw.length < 2) {
            alert("Adicione pelo menos 2 jogadores para o sorteio!");
            return;
        }
        let shuffled = shuffleArray([...playersForDraw]);
        const mid = Math.ceil(shuffled.length / 2);
        const teamA = shuffled.slice(0, mid);
        const teamB = shuffled.slice(mid);
        setupMatch(teamA, teamB);
    };

    window.startWithReadyTeams = () => {
        const elA = document.getElementById('bulkTeamA');
        const elB = document.getElementById('bulkTeamB');
        const textA = elA.value.trim();
        const textB = elB.value.trim();
        if (!textA || !textB) {
            alert("Preencha os nomes para os dois times!");
            return;
        }
        const namesA = textA.split(/[,\n]/).map(n => n.trim()).filter(n => n.length > 0);
        const namesB = textB.split(/[,\n]/).map(n => n.trim()).filter(n => n.length > 0);
        const teamA = namesA.map((name, index) => ({ dbId: 'readyA' + Date.now() + index, name: name }));
        const teamB = namesB.map((name, index) => ({ dbId: 'readyB' + Date.now() + index, name: name }));
        elA.value = "";
        elB.value = "";
        setupMatch(teamA, teamB);
    };

    function setupMatch(teamA, teamB) {
        activeMatchPlayers = [...teamA.map(p => ({...p, goals: 0, yellowCards: 0, redCards: 0})), 
                             ...teamB.map(p => ({...p, goals: 0, yellowCards: 0, redCards: 0}))];
        renderMatchPlayers('playersTeamA', teamA, 'A');
        renderMatchPlayers('playersTeamB', teamB, 'B');
        document.getElementById('setupCard').classList.add('hidden');
        document.getElementById('gameDashboard').classList.remove('hidden');
        window.scrollTo({ top: 0, behavior: 'smooth' });
        log("Partida iniciada!");
    }

    function renderMatchPlayers(containerId, teamPlayers, teamSide) {
        const container = document.getElementById(containerId);
        container.innerHTML = teamPlayers.map(p => `
            <div class="player-row" id="p-row-${p.dbId}">
                <div class="player-info">
                    <span class="player-name">${p.name}</span>
                    <span class="player-stats-mini" id="stats-${p.dbId}">Gols: 0 | CA: 0</span>
                </div>
                <div class="action-btns">
                    <button class="action-btn btn-goal" onclick="recordAction('${p.dbId}', 'goal', '${teamSide}')">⚽</button>
                    <button class="action-btn btn-y-card" onclick="recordAction('${p.dbId}', 'yellow')">🟨</button>
                    <button class="action-btn btn-r-card" onclick="recordAction('${p.dbId}', 'red')">🟥</button>
                </div>
            </div>
        `).join('');
    }

    let timerInterval;
    let seconds = 0;
    let isRunning = false;
    let scoreA = 0;
    let scoreB = 0;

    window.recordAction = (playerId, type, teamSide) => {
        const player = activeMatchPlayers.find(p => p.dbId === playerId);
        if (!player) return;
        const timeStr = document.getElementById('timer').innerText;
        if (type === 'goal') {
            player.goals++;
            if (teamSide === 'A') {
                scoreA++;
                document.getElementById('scoreA').innerText = scoreA;
            } else {
                scoreB++;
                document.getElementById('scoreB').innerText = scoreB;
            }
            goalHistory.push({ playerId, teamSide });
            log(`${timeStr} - GOL! ${player.name} marcou.`);
        } 
        else if (type === 'yellow') {
            player.yellowCards++;
            log(`${timeStr} - Cartão Amarelo: ${player.name}.`);
            if (player.yellowCards >= 2) recordAction(playerId, 'red');
        } 
        else if (type === 'red') {
            player.redCards++;
            const row = document.getElementById(`p-row-${playerId}`);
            if (row) {
                row.style.opacity = '0.3';
                row.style.background = '#450a0a';
            }
            log(`${timeStr} - CARTÃO VERMELHO! ${player.name} expulso.`);
        }
        updatePlayerStatsUI(player);
    };

    window.undoLastGoal = () => {
        if (goalHistory.length === 0) return;
        const lastGoal = goalHistory.pop();
        const player = activeMatchPlayers.find(p => p.dbId === lastGoal.playerId);
        if (player && player.goals > 0) {
            player.goals--;
            if (lastGoal.teamSide === 'A') {
                scoreA--;
                document.getElementById('scoreA').innerText = scoreA;
            } else {
                scoreB--;
                document.getElementById('scoreB').innerText = scoreB;
            }
            updatePlayerStatsUI(player);
            log(`Golo de ${player.name} foi ANULADO.`);
        }
    };

    window.exitGame = () => {
        document.getElementById('gameDashboard').classList.add('hidden');
        document.getElementById('setupCard').classList.remove('hidden');
        
        clearInterval(timerInterval);
        seconds = 0;
        isRunning = false;
        document.getElementById('timer').innerText = "00:00";
        document.getElementById('playPauseBtn').innerText = "Iniciar";
        
        scoreA = 0;
        scoreB = 0;
        document.getElementById('scoreA').innerText = "0";
        document.getElementById('scoreB').innerText = "0";
        document.getElementById('matchLog').innerHTML = '<div class="log-entry">Aguardando início da partida...</div>';
        
        activeMatchPlayers = [];
        goalHistory = [];
        
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    function updatePlayerStatsUI(player) {
        const statsEl = document.getElementById(`stats-${player.dbId}`);
        if (statsEl) statsEl.innerText = `Gols: ${player.goals} | CA: ${player.yellowCards}`;
    }

    function log(msg) {
        const logBox = document.getElementById('matchLog');
        const entry = document.createElement('div');
        entry.className = 'log-entry';
        entry.innerText = msg;
        logBox.prepend(entry);
    }

    window.toggleTimer = () => {
        const btn = document.getElementById('playPauseBtn');
        if (isRunning) {
            clearInterval(timerInterval);
            btn.innerText = "Retomar";
            isRunning = false;
        } else {
            timerInterval = setInterval(() => {
                seconds++;
                const m = Math.floor(seconds / 60);
                const s = seconds % 60;
                document.getElementById('timer').innerText = `${String(m).padStart(2, '0')}:${String(s).padStart(2, '0')}`;
            }, 1000);
            btn.innerText = "Pausar";
            isRunning = true;
        }
    };

    window.resetTimer = () => {
        clearInterval(timerInterval);
        seconds = 0;
        isRunning = false;
        document.getElementById('timer').innerText = "00:00";
        document.getElementById('playPauseBtn').innerText = "Iniciar";
    };

    window.handleEnter = (e) => { if (e.key === 'Enter') window.addPlayer(); };
    
    document.getElementById('btnAddPlayer').addEventListener('click', window.addPlayer);
    document.getElementById('btnDraw').addEventListener('click', window.drawTeams);
    document.getElementById('btnStartReady').addEventListener('click', window.startWithReadyTeams);
    document.getElementById('playPauseBtn').addEventListener('click', window.toggleTimer);
    document.getElementById('resetTimerBtn').addEventListener('click', window.resetTimer);
    document.getElementById('btnUndoGoal').addEventListener('click', window.undoLastGoal);
    document.getElementById('btnExitGame').addEventListener('click', window.exitGame);
</script>

</body>
</html>
