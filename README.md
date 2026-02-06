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
            --accent: #8b5cf6;
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

        .header-area {
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
            margin-bottom: 30px;
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
            margin: 0;
            color: var(--primary);
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .header-actions {
            position: absolute;
            right: 0;
            display: flex;
            gap: 15px;
            align-items: center;
            z-index: 100;
        }

        .action-icon-btn {
            background: none;
            border: none;
            color: #94a3b8;
            font-size: 1.6rem;
            cursor: pointer;
            transition: transform 0.3s ease, color 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 5px;
        }

        .action-icon-btn:hover {
            color: var(--primary);
            transform: scale(1.1);
        }

        .logout-btn:hover {
            color: var(--red) !important;
        }

        #loginScreen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--bg-dark);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 10000;
        }

        .login-card {
            width: 100%;
            max-width: 450px;
            text-align: center;
        }

        .login-input {
            width: 100%;
            margin-bottom: 15px;
            box-sizing: border-box;
        }

        .access-toggle {
            display: flex;
            background: #0f172a;
            padding: 5px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .access-btn {
            flex: 1;
            padding: 10px;
            border: none;
            background: transparent;
            color: #94a3b8;
            cursor: pointer;
            border-radius: 8px;
            font-weight: bold;
        }

        .access-btn.active {
            background: var(--primary);
            color: white;
        }

        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 9999;
        }

        .modal-content {
            background: var(--card-bg);
            width: 90%;
            max-width: 600px;
            padding: 30px;
            border-radius: 20px;
            position: relative;
            max-height: 80vh;
            overflow-y: auto;
        }

        .close-modal {
            position: absolute;
            top: 15px;
            right: 20px;
            font-size: 2rem;
            cursor: pointer;
            color: #94a3b8;
        }

        .tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            justify-content: center;
        }

        .tab-btn {
            padding: 10px 20px;
            background: #334155;
            border: none;
            border-radius: 8px;
            color: white;
            cursor: pointer;
            font-weight: bold;
            transition: 0.2s;
        }

        .tab-btn.active {
            background: var(--primary);
        }

        h2 { font-size: 1.25rem; margin-top: 0; color: #94a3b8; }
        h3 { font-size: 1rem; color: var(--text); margin-bottom: 10px; }

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
        .btn-add:hover { transform: translateY(-2px); }

        .btn-draw {
            width: 100%;
            background: var(--secondary);
            color: white;
            font-size: 1.2rem;
            margin-top: 10px;
        }

        .btn-start-ready {
            background: var(--accent); 
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
            cursor: pointer;
            transition: 0.2s;
            position: relative;
        }

        .player-tag.confirmed {
            border: 2px solid var(--primary);
            background: #064e3b;
        }

        .player-tag span.remove { color: var(--red); font-weight: bold; margin-left: 5px; }

        .bulk-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-bottom: 15px;
        }

        .teams-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .team-column {
            background: rgba(15, 23, 42, 0.5);
            padding: 20px;
            border-radius: 12px;
            border: 2px solid transparent;
        }

        .team-1-border { border-color: var(--primary); }
        .team-2-border { border-color: var(--secondary); }

        .match-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            background: #0f172a;
            padding: 20px;
            border-radius: 12px;
        }

        #timer { font-size: 3rem; font-family: monospace; color: var(--primary); }

        .score-display { font-size: 4rem; font-weight: 900; text-align: center; margin: 10px 0; }

        .player-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: var(--card-bg);
            padding: 12px;
            margin-bottom: 8px;
            border-radius: 8px;
        }

        .action-btns { display: flex; gap: 8px; }
        .action-btn { width: 32px; height: 32px; border-radius: 4px; border: none; cursor: pointer; font-weight: bold; }
        .btn-goal { background: #fff; }
        .btn-y-card { background: var(--yellow); }
        .btn-r-card { background: var(--red); color: white; }

        .log-container {
            margin-top: 30px;
            background: #0f172a;
            padding: 15px;
            border-radius: 8px;
            max-height: 200px;
            overflow-y: auto;
        }

        .log-entry { font-size: 0.85rem; border-bottom: 1px solid #1e293b; padding: 5px 0; color: #94a3b8; }

        .hidden { display: none !important; }

        .prompt-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9);
            display: flex; justify-content: center; align-items: center;
            z-index: 20000;
        }

        input, textarea, select {
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

        textarea { height: 150px; resize: none; }

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
            .bulk-grid { grid-template-columns: 1fr; }
            .match-header { flex-direction: column; gap: 20px; }
            .header-actions { position: relative; justify-content: center; margin-top: 10px; }
            .header-area { flex-direction: column; }
        }
    </style>
</head>
<body>

<!-- Login Principal -->
<div id="loginScreen">
    <div class="card login-card">
        <h1>🏆 Arena Premier</h1>
        <p style="color: #94a3b8; margin-bottom: 20px;">Bem-vindo ao sistema de gestão.</p>
        
        <div class="access-toggle">
            <button class="access-btn active" id="toggleMember" onclick="window.switchLogin('member')">Membro</button>
            <button class="access-btn" id="toggleMaster" onclick="window.switchLogin('master')">Master</button>
        </div>

        <!-- Formulário Master -->
        <div id="masterForm" class="hidden">
            <input type="text" id="loginUser" class="login-input" placeholder="Usuário Master">
            <input type="password" id="loginPass" class="login-input" placeholder="Senha Master">
            <button class="btn btn-add" id="btnLoginMaster" style="width: 100%;">Aceder como Master</button>
        </div>

        <!-- Formulário Membro -->
        <div id="memberForm">
            <select id="loginMemberSelect" class="login-input">
                <option value="">Seleciona o teu nome...</option>
            </select>
            <input type="password" id="loginMemberPin" class="login-input" placeholder="Teu PIN">
            <button class="btn btn-add" id="btnLoginMember" style="width: 100%; background: var(--secondary);">Entrar</button>
        </div>

        <div id="loginError" style="color: var(--red); margin-top: 15px; font-size: 0.9rem; display: none;">Credenciais incorretas!</div>
    </div>
</div>

<!-- Admin Panel -->
<div id="configModal" class="modal-overlay hidden">
    <div class="modal-content">
        <span class="close-modal" id="closeModal">&times;</span>
        <h2 style="color: var(--primary);">⚙️ Painel de Administração</h2>
        <div class="card" style="background: #0f172a; border: 1px solid #334155;">
            <h3>Cadastrar Novo Membro</h3>
            <div style="display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px;">
                <input type="text" id="playerName" placeholder="Nome do jogador...">
                <input type="password" id="playerPin" placeholder="PIN de acesso (Ex: 1234)">
                <button class="btn btn-add" id="btnAddPlayer">Adicionar à Base</button>
            </div>
            <h3>Gerir Jogadores e PINs</h3>
            <div class="player-tag-list" id="playerTagList"></div>
        </div>
    </div>
</div>

<!-- Conteúdo Principal -->
<div class="main-container hidden" id="appContent">
    <div class="header-area">
        <h1>🏆 Arena Premier</h1>
        <div class="header-actions">
            <button class="action-icon-btn hidden" id="openConfig" title="Configurações Master">⚙️</button>
            <button class="action-icon-btn logout-btn" id="btnMasterLogout" title="Sair do Sistema">🚪</button>
        </div>
    </div>

    <div class="tabs" id="mainTabs">
        <button class="tab-btn active" id="tabConfirm" onclick="window.switchTab('confirm')">Presença</button>
        <button class="tab-btn hidden" id="tabManual" onclick="window.switchTab('manual')">Equipas Prontas</button>
    </div>

    <div id="confirmSection">
        <div class="card">
            <h2>Lista de Convocados</h2>
            <p id="welcomeMsg" style="font-size: 1rem; color: var(--primary); margin-bottom: 15px; font-weight: bold;"></p>
            <p style="font-size: 0.85rem; color: #94a3b8; margin-bottom: 15px;">
                Clica no teu nome para alternar a tua presença. <br>
                <span id="lockInfo" style="color: var(--accent);">* Apenas o Master pode gerar a partida.</span>
            </p>
            <div class="player-tag-list" id="confirmTagList"></div>
            <div id="countInfo" style="text-align: center; font-weight: bold; margin-bottom: 15px; font-size: 1.1rem; color: var(--primary);">Confirmados: 0</div>
            <button class="btn btn-draw hidden" id="btnDraw2Teams">Gerar Partida (2 Equipas)</button>
        </div>
    </div>

    <div id="manualSection" class="hidden">
        <div class="card">
            <h2>Iniciar com Equipas Prontas</h2>
            <div class="bulk-grid">
                <div>
                    <h3 style="color: var(--primary)">Equipa Verde</h3>
                    <textarea id="bulkTeam1" placeholder="Um nome por linha..."></textarea>
                </div>
                <div>
                    <h3 style="color: var(--secondary)">Equipa Azul</h3>
                    <textarea id="bulkTeam2" placeholder="Um nome por linha..."></textarea>
                </div>
            </div>
            <button class="btn btn-start-ready" id="btnStartReady">Iniciar Jogo Rápido</button>
        </div>
    </div>

    <div id="gameDashboard" class="card hidden">
        <div class="match-header">
            <div>
                <button class="btn btn-add hidden" style="background: var(--red)" id="btnExitGame">Sair do Jogo</button>
            </div>
            <div style="text-align: center">
                <span id="timer">00:00</span>
                <div id="timerControls" style="margin-top: 10px;" class="hidden">
                    <button class="tab-btn" id="playPauseBtn">Iniciar</button>
                    <button class="tab-btn" id="resetTimerBtn">Reset</button>
                </div>
            </div>
            <div style="text-align: right">
                <button class="btn hidden" id="btnUndoGoal" style="background: #475569; color: white;">Anular Golo</button>
            </div>
        </div>
        <div class="teams-grid" id="teamsContainer"></div>
        <div class="log-container" id="matchLog">
            <div class="log-entry">Aguardando início...</div>
        </div>
    </div>
</div>

<footer>
    © 2026 – FUT-FBK – Painel Master CLX
</footer>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getFirestore, collection, addDoc, deleteDoc, updateDoc, doc, onSnapshot } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
    const playersRef = collection(db, 'artifacts', appId, 'public', 'data', 'players');

    let allPlayers = [];
    let goalHistory = [];
    let scores = { 1: 0, 2: 0 };
    let sec = 0, tInt = null;
    let isMaster = false;
    let currentUser = null; // Guardará o objeto do membro logado ou "master"

    // --- NAVEGAÇÃO E LOGIN ---
    window.switchLogin = (mode) => {
        const memberBtn = document.getElementById('toggleMember');
        const masterBtn = document.getElementById('toggleMaster');
        const memberForm = document.getElementById('memberForm');
        const masterForm = document.getElementById('masterForm');

        if(mode === 'master') {
            masterBtn.classList.add('active');
            memberBtn.classList.remove('active');
            masterForm.classList.remove('hidden');
            memberForm.classList.add('hidden');
        } else {
            memberBtn.classList.add('active');
            masterBtn.classList.remove('active');
            memberForm.classList.remove('hidden');
            masterForm.classList.add('hidden');
        }
    };

    window.switchTab = (tab) => {
        document.getElementById('manualSection').classList.add('hidden');
        document.getElementById('confirmSection').classList.add('hidden');
        document.getElementById('gameDashboard').classList.add('hidden');
        document.getElementById('tabManual').classList.remove('active');
        document.getElementById('tabConfirm').classList.remove('active');
        document.getElementById('mainTabs').classList.remove('hidden');

        if (tab === 'manual') {
            document.getElementById('manualSection').classList.remove('hidden');
            document.getElementById('tabManual').classList.add('active');
        } else if (tab === 'confirm') {
            document.getElementById('confirmSection').classList.remove('hidden');
            document.getElementById('tabConfirm').classList.add('active');
        }
    };

    // --- AÇÕES DO JOGO ---
    window.goal = (name, tid) => {
        if(!isMaster) return;
        scores[tid]++;
        const el = document.getElementById(`score${tid}`);
        if(el) el.innerText = scores[tid];
        goalHistory.push({ tid });
        logMatch(`GOL: ${name}`);
    };

    window.yellowCard = (name) => {
        if(!isMaster) return;
        logMatch(`Amarelo: ${name}`);
    };

    window.redCard = (name, pid) => {
        if(!isMaster) return;
        logMatch(`Vermelho: ${name}`);
        const row = document.getElementById(`row-${pid}`);
        if(row) row.style.opacity = '0.3';
    };

    // --- GESTÃO DE DADOS ---
    window.deletePlayer = async (id) => {
        if(!isMaster) return;
        if(confirm("Eliminar jogador da base de dados?")) {
            await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'players', id));
        }
    };

    window.togglePresence = async (id) => {
        const player = allPlayers.find(p => p.id === id);
        // Regra: Se for Master, pode mudar qualquer um. Se for Membro, só pode mudar o próprio.
        if (isMaster || (currentUser && currentUser.id === id)) {
            await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'players', id), { confirmed: !player.confirmed });
        } else {
            alert("Só podes marcar a tua própria presença!");
        }
    };

    // --- EVENTOS DE CLIQUE ---

    // Login Master
    document.getElementById('btnLoginMaster').onclick = () => {
        const user = document.getElementById('loginUser').value.trim();
        const pass = document.getElementById('loginPass').value.trim();
        if (user === "CLX" && pass === "02072007") {
            isMaster = true;
            currentUser = { name: "Master CLX", id: "master" };
            enterApp();
        } else {
            showLoginError();
        }
    };

    // Login Membro
    document.getElementById('btnLoginMember').onclick = () => {
        const pid = document.getElementById('loginMemberSelect').value;
        const pin = document.getElementById('loginMemberPin').value;
        const player = allPlayers.find(p => p.id === pid);

        if (player && String(player.pin) === String(pin)) {
            isMaster = false;
            currentUser = player;
            enterApp();
        } else {
            showLoginError();
        }
    };

    function enterApp() {
        document.getElementById('loginScreen').style.display = 'none';
        document.getElementById('appContent').classList.remove('hidden');
        document.getElementById('welcomeMsg').innerText = `Olá, ${currentUser.name}!`;
        
        // Ajustar visibilidade de recursos Master
        if (isMaster) {
            document.getElementById('openConfig').classList.remove('hidden');
            document.getElementById('tabManual').classList.remove('hidden');
            document.getElementById('btnDraw2Teams').classList.remove('hidden');
            document.getElementById('btnExitGame').classList.remove('hidden');
            document.getElementById('btnUndoGoal').classList.remove('hidden');
            document.getElementById('timerControls').classList.remove('hidden');
        } else {
            document.getElementById('openConfig').classList.add('hidden');
            document.getElementById('tabManual').classList.add('hidden');
            document.getElementById('btnDraw2Teams').classList.add('hidden');
            document.getElementById('btnExitGame').classList.add('hidden');
            document.getElementById('btnUndoGoal').classList.add('hidden');
            document.getElementById('timerControls').classList.add('hidden');
        }
        window.switchTab('confirm');
    }

    function showLoginError() {
        document.getElementById('loginError').style.display = 'block';
        setTimeout(() => { document.getElementById('loginError').style.display = 'none'; }, 3000);
    }

    document.getElementById('btnMasterLogout').onclick = () => {
        if(confirm("Encerrar sessão?")) {
            isMaster = false;
            currentUser = null;
            document.getElementById('loginUser').value = "";
            document.getElementById('loginPass').value = "";
            document.getElementById('loginMemberPin').value = "";
            document.getElementById('appContent').classList.add('hidden');
            document.getElementById('loginScreen').style.display = 'flex';
        }
    };

    document.getElementById('btnExitGame').onclick = () => {
        if(confirm("Desejas sair da partida?")) {
            if(tInt) clearInterval(tInt);
            tInt = null;
            sec = 0;
            window.switchTab('confirm');
        }
    };

    document.getElementById('openConfig').onclick = () => document.getElementById('configModal').classList.remove('hidden');
    document.getElementById('closeModal').onclick = () => document.getElementById('configModal').classList.add('hidden');

    document.getElementById('btnAddPlayer').onclick = async () => {
        const n = document.getElementById('playerName').value.trim();
        const p = document.getElementById('playerPin').value.trim();
        if (n && p) {
            await addDoc(playersRef, { name: n, pin: p, confirmed: false });
            document.getElementById('playerName').value = "";
            document.getElementById('playerPin').value = "";
        }
    };

    document.getElementById('btnDraw2Teams').onclick = () => {
        const conf = allPlayers.filter(p => p.confirmed);
        if(conf.length < 2) return alert("Precisas de pelo menos 2 confirmados.");
        
        let shuf = [...conf].sort(() => Math.random() - 0.5);
        const mid = Math.ceil(shuf.length / 2);
        
        startMatch([
            { name: "VERDE", color: "team-1-border", players: shuf.slice(0, mid), id: 1 },
            { name: "AZUL", color: "team-2-border", players: shuf.slice(mid), id: 2 }
        ]);
    };

    document.getElementById('btnStartReady').onclick = () => {
        const t1 = document.getElementById('bulkTeam1').value.split('\n').filter(x=>x.trim());
        const t2 = document.getElementById('bulkTeam2').value.split('\n').filter(x=>x.trim());
        
        if(!t1.length || !t2.length) return alert("Preenche as duas equipas.");

        startMatch([
            { name: "VERDE", color: "team-1-border", players: t1.map(n=>({name:n,id:Math.random()})), id: 1 },
            { name: "AZUL", color: "team-2-border", players: t2.map(n=>({name:n,id:Math.random()})), id: 2 }
        ]);
    };

    document.getElementById('playPauseBtn').onclick = () => {
        const btn = document.getElementById('playPauseBtn');
        if(btn.innerText !== "Pausar") {
            tInt = setInterval(() => {
                sec++;
                const m = String(Math.floor(sec/60)).padStart(2,'0');
                const s = String(sec%60).padStart(2,'0');
                document.getElementById('timer').innerText = `${m}:${s}`;
            }, 1000);
            btn.innerText = "Pausar";
        } else {
            clearInterval(tInt);
            btn.innerText = "Retomar";
        }
    };

    document.getElementById('resetTimerBtn').onclick = () => {
        if(tInt) clearInterval(tInt);
        sec = 0;
        document.getElementById('timer').innerText = "00:00";
        document.getElementById('playPauseBtn').innerText = "Iniciar";
    };

    document.getElementById('btnUndoGoal').onclick = () => {
        if(!goalHistory.length) return;
        const last = goalHistory.pop();
        scores[last.tid]--;
        document.getElementById(`score${last.tid}`).innerText = scores[last.tid];
        logMatch("Golo anulado");
    };

    // --- FIRESTORE SYNC ---
    onSnapshot(playersRef, (snapshot) => {
        allPlayers = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
        renderAll();
    });

    function renderAll() {
        // Atualizar lista de Administração
        const setupList = document.getElementById('playerTagList');
        if (setupList) {
            setupList.innerHTML = allPlayers.map(p => `
                <div class="player-tag">
                    ${p.name} <span style="font-size:0.7rem; opacity:0.6;">(PIN:${p.pin})</span>
                    <span class="remove" onclick="window.deletePlayer('${p.id}')">&times;</span>
                </div>
            `).join('');
        }

        // Atualizar Seletor de Login de Membro
        const select = document.getElementById('loginMemberSelect');
        const currentVal = select.value;
        select.innerHTML = '<option value="">Seleciona o teu nome...</option>' + 
            allPlayers.map(p => `<option value="${p.id}">${p.name}</option>`).join('');
        select.value = currentVal;

        // Atualizar Lista de Confirmação
        const confirmList = document.getElementById('confirmTagList');
        if (confirmList) {
            const confirmed = allPlayers.filter(p => p.confirmed);
            document.getElementById('countInfo').innerText = `Confirmados: ${confirmed.length}`;
            confirmList.innerHTML = allPlayers.map(p => `
                <div class="player-tag ${p.confirmed ? 'confirmed' : ''}" onclick="window.togglePresence('${p.id}')">
                    ${p.name}
                </div>
            `).join('');
        }
    }

    function startMatch(teams) {
        document.getElementById('confirmSection').classList.add('hidden');
        document.getElementById('manualSection').classList.add('hidden');
        document.getElementById('mainTabs').classList.add('hidden');
        document.getElementById('gameDashboard').classList.remove('hidden');
        
        scores = { 1: 0, 2: 0 };
        goalHistory = [];
        sec = 0;
        if(tInt) clearInterval(tInt);
        document.getElementById('timer').innerText = "00:00";
        document.getElementById('playPauseBtn').innerText = "Iniciar";
        document.getElementById('matchLog').innerHTML = '<div class="log-entry">Partida iniciada!</div>';

        const container = document.getElementById('teamsContainer');
        container.innerHTML = teams.map(t => `
            <div class="team-column ${t.color}">
                <h2 style="text-align:center">${t.name}</h2>
                <div class="score-display" id="score${t.id}">0</div>
                <div>
                    ${t.players.map(p => `
                        <div class="player-row" id="row-${p.id}">
                            <span>${p.name}</span>
                            <div class="action-btns ${!isMaster ? 'hidden' : ''}">
                                <button class="action-btn btn-goal" onclick="window.goal('${p.name}', ${t.id})" title="Golo">⚽</button>
                                <button class="action-btn btn-y-card" onclick="window.yellowCard('${p.name}')" title="Cartão Amarelo"></button>
                                <button class="action-btn btn-r-card" onclick="window.redCard('${p.name}', '${p.id}')" title="Cartão Vermelho"></button>
                            </div>
                        </div>
                    `).join('')}
                </div>
            </div>
        `).join('');
    }

    function logMatch(m) {
        const entry = document.createElement('div');
        entry.className = 'log-entry';
        const time = document.getElementById('timer').innerText;
        entry.innerText = `[${time}] ${m}`;
        document.getElementById('matchLog').prepend(entry);
    }
</script>
</body>
</html>
