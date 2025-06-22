<ROMANTICA VIVI DAVA>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Petualangan Romantis: Surat Cinta di Sekolah</title>
    
    <!-- Impor Font dari Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&family=Playfair+Display:wght@700&display=swap" rel="stylesheet">
    
    <!-- Impor Library Suara Tone.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.7.77/Tone.min.js"></script>
    
    <style>
        :root {
            --primary-color: #d81b60;
            --secondary-color: #fce4ec;
            --text-color: #333;
            --wall-color: #aebfd1;
            --floor-color: #f0f4f8;
            --player1-color: #ff7171;
            --player2-color: #42a5f5;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background-color: var(--secondary-color);
            color: var(--text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            overflow: hidden;
        }

        .game-container {
            width: 100%;
            max-width: 900px;
            background-color: white;
            border-radius: 20px;
            box-shadow: 0 15px 30px rgba(0,0,0,0.15);
            padding: 20px;
            box-sizing: border-box;
            text-align: center;
        }
        
        #start-screen, #end-screen {
            padding: 40px;
        }

        h1, h2 {
            font-family: 'Playfair Display', serif;
            color: var(--primary-color);
        }

        #game-canvas {
            background-color: var(--floor-color);
            border-radius: 10px;
            border: 2px solid #e0e0e0;
            width: 100%;
            height: auto;
        }

        #game-screen {
            display: none;
        }
        
        #question-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.6);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            padding: 20px;
            box-sizing: border-box;
        }

        .modal-content {
            background-color: white;
            padding: 40px;
            border-radius: 15px;
            max-width: 500px;
            width: 100%;
            text-align: center;
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.9); }
            to { opacity: 1; transform: scale(1); }
        }

        .modal-content h2 {
            margin-top: 0;
            font-size: 24px;
        }

        .modal-content p {
            font-size: 18px;
            line-height: 1.6;
        }

        button {
            font-family: 'Poppins', sans-serif;
            padding: 12px 25px;
            font-size: 16px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            background-color: var(--primary-color);
            color: white;
            transition: transform 0.2s ease;
            margin-top: 20px;
        }

        button:hover {
            transform: scale(1.05);
        }
        
        #room-info {
            background-color: #f1f1f1;
            padding: 15px;
            border-radius: 8px;
            margin-top: 20px;
            font-size: 16px;
            word-wrap: break-word;
        }

        #player-info {
            background: rgba(255,255,255,0.8);
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 10px;
            display: inline-block;
        }
        
        #join-section input {
            padding: 10px;
            font-size: 16px;
            width: calc(100% - 100px);
            max-width: 300px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }

        #join-room-btn {
            margin-top: 0;
            padding: 11px 15px;
            vertical-align: middle;
        }

    </style>
</head>
<body>
    <div class="game-container">
        <!-- Tampilan Awal -->
        <div id="start-screen">
            <h1>Surat Cinta di Sekolah</h1>
            <p>Sebuah petualangan deeptalk untuk kita berdua.</p>
            <p style="font-size:14px; color:#555;">
                <strong>Cara Bermain:</strong><br>
                1. Satu orang klik <strong>Buat Room Baru</strong> dan bagikan <strong>Room ID</strong> ke pasangan.<br>
                2. Pasanganmu memasukkan Room ID lalu klik <strong>Gabung</strong>.<br>
                3. Lakukan panggilan suara (WA/Discord/Telepon).<br>
                4. Gunakan tombol panah di keyboard untuk bergerak!
            </p>
            
            <!-- PERBAIKAN: Menambahkan cara bergabung lewat ID -->
            <div id="join-section" style="margin-top: 20px;">
                <input type="text" id="room-id-input" placeholder="Masukkan Room ID">
                <button id="join-room-btn">Gabung</button>
            </div>
            <p style="margin: 10px 0;">atau</p>
            <button id="create-room-btn">Buat Room Baru</button>
            <div id="room-info" style="display:none;">
                <!-- Konten akan diisi oleh JavaScript -->
            </div>
        </div>

        <!-- Tampilan Game -->
        <div id="game-screen">
            <h2 id="game-title">Jelajahi Sekolah & Temukan Suratnya!</h2>
            <div id="player-info">
                <span style="color:var(--player1-color)">⬤</span> Syamil &nbsp;&nbsp; 
                <span style="color:var(--player2-color)">⬤</span> Intan
            </div>
            <canvas id="game-canvas"></canvas>
        </div>
        
        <!-- Tampilan Akhir -->
        <div id="end-screen" style="display:none;">
            <h1>Misi Selesai!</h1>
            <p>Terima kasih sudah meluangkan waktu untuk petualangan ini, sayang. Semua jawabanmu sangat berarti. Ini adalah awal dari babak baru kita yang lebih kuat.</p>
            <p>To be continued...</p>
            <button onclick="window.location.reload()">Main Lagi</button>
        </div>
    </div>

    <!-- Modal Pertanyaan -->
    <div id="question-modal">
        <div class="modal-content">
            <h2 id="modal-title">💌 Surat Ditemukan! 💌</h2>
            <p id="modal-question">Ini adalah isi dari pertanyaannya.</p>
            <button id="close-modal-btn">Oke, kita diskusikan!</button>
        </div>
    </div>
    
    <script type="module">
        // Impor fungsi-fungsi Firebase
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";

        // Konfigurasi Firebase dan App ID
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-romantic-game';

        // Inisialisasi Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);

        // Elemen-elemen UI
        const startScreen = document.getElementById('start-screen');
        const gameScreen = document.getElementById('game-screen');
        const endScreen = document.getElementById('end-screen');
        const createRoomBtn = document.getElementById('create-room-btn');
        const joinRoomBtn = document.getElementById('join-room-btn');
        const roomIdInput = document.getElementById('room-id-input');
        const roomInfo = document.getElementById('room-info');
        const canvas = document.getElementById('game-canvas');
        const ctx = canvas.getContext('2d');
        const modal = document.getElementById('question-modal');
        const modalQuestion = document.getElementById('modal-question');
        const closeModalBtn = document.getElementById('close-modal-btn');
        const gameTitle = document.getElementById('game-title');

        // State Game
        let localPlayerId = null;
        let gameRoomId = null;
        let unsubscribe = null;
        let gameState = {};
        let synth;
        
        const deeptalkQuestions = [ "Apa satu kenangan kecil tentang kita yang paling sering kamu putar ulang di kepala?", "Menurutmu, apa 'bahasa cinta' yang paling sering aku gunakan untukmu, dan apa yang paling kamu suka terima?", "Jika hubungan kita adalah sebuah lagu, judulnya apa dan kenapa?", "Hal apa tentang diriku yang membuatmu merasa paling aman dan nyaman?", "Apa mimpi atau tujuan pribadi yang belum pernah kamu ceritakan, dan bagaimana aku bisa mendukungmu?", "Bagaimana cara terbaik untuk menghiburmu saat kamu sedang sedih atau stres?", "Momen seperti apa yang membuatmu merasa paling terhubung dan 'satu frekuensi' denganku?" ];
        const playerSize = 20;
        const playerSpeed = 4;
        let walls = [], mapLabels = [], envelopes = [];

        function resizeCanvas() {
            const container = document.querySelector('.game-container');
            const newWidth = container.clientWidth - 40;
            canvas.width = newWidth;
            canvas.height = newWidth * (5 / 8);
            defineMapLayout();
            if (gameState && gameState.players) draw();
        }
        
        function defineMapLayout() {
            const w = canvas.width, h = canvas.height;
            walls = [ { x: 0, y: 0, width: w, height: 10 }, { x: 0, y: h - 10, width: w, height: 10 }, { x: 0, y: 0, width: 10, height: h }, { x: w - 10, y: 0, width: 10, height: h }, { x: w * 0.31, y: 10, width: 10, height: h * 0.4 }, { x: 10, y: h * 0.4, width: w * 0.31, height: 10 }, { x: w * 0.31, y: h * 0.3, width: w * 0.19, height: 10 }, { x: w * 0.62, y: 10, width: 10, height: h * 0.7 }, { x: w * 0.62, y: h * 0.4, width: w * 0.38, height: 10 }, ];
            mapLabels = [ { text: "Kelas Kenangan", x: w * 0.1, y: h * 0.2 }, { text: "Koridor", x: w * 0.41, y: h * 0.37 }, { text: "Taman Belakang", x: w * 0.12, y: h * 0.7 }, { text: "Perpustakaan", x: w * 0.75, y: h * 0.2 }, { text: "Kantin Cinta", x: w * 0.77, y: h * 0.6 }, ];
            envelopes = deeptalkQuestions.map((q, i) => { const positions = [ { x: w * 0.06, y: h * 0.1, requiresBoth: false }, { x: w * 0.93, y: h * 0.1, requiresBoth: false }, { x: w * 0.5, y: h * 0.9, requiresBoth: true },  { x: w * 0.87, y: h * 0.8, requiresBoth: false }, { x: w * 0.18, y: h * 0.8, requiresBoth: false }, { x: w * 0.56, y: h * 0.2, requiresBoth: false }, { x: w * 0.56, y: h * 0.5, requiresBoth: true }, ]; return { id: i, x: positions[i].x, y: positions[i].y, found: false, question: q, requiresBoth: positions[i].requiresBoth }; });
        }
        
        function draw() {
            if (!ctx || !gameState.players || !gameState.envelopes) return;
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = 'var(--wall-color)';
            walls.forEach(wall => ctx.fillRect(wall.x, wall.y, wall.width, wall.height));
            ctx.fillStyle = '#aaa';
            ctx.font = `italic ${canvas.width / 50}px Poppins`;
            mapLabels.forEach(label => ctx.fillText(label.text, label.x, label.y));
            ctx.font = `${canvas.width / 30}px Poppins`;
            gameState.envelopes.forEach(env => { if (!env.found) ctx.fillText("💌", env.x, env.y); });
            Object.values(gameState.players).forEach(player => {
                ctx.fillStyle = player.name === "Syamil" ? 'var(--player1-color)' : 'var(--player2-color)';
                ctx.beginPath();
                ctx.arc(player.x, player.y, playerSize / 2, 0, Math.PI * 2);
                ctx.fill();
                ctx.fillStyle = "white";
                ctx.font = `bold ${playerSize * 0.6}px Poppins`;
                ctx.textAlign = "center";
                ctx.fillText(player.name.charAt(0), player.x, player.y + (playerSize * 0.2));
                ctx.textAlign = "start";
            });
        }

        function handleKeyDown(e) {
            if (!gameState.players || !gameState.players[localPlayerId]) return;
            let player = gameState.players[localPlayerId];
            let newPos = { x: player.x, y: player.y };
            switch (e.key) { case "ArrowUp": newPos.y -= playerSpeed; break; case "ArrowDown": newPos.y += playerSpeed; break; case "ArrowLeft": newPos.x -= playerSpeed; break; case "ArrowRight": newPos.x += playerSpeed; break; default: return; }
            let playerRadius = playerSize / 2;
            let collided = walls.some(wall => newPos.x + playerRadius > wall.x && newPos.x - playerRadius < wall.x + wall.width && newPos.y + playerRadius > wall.y && newPos.y - playerRadius < wall.y + wall.height);
            if (!collided) { player.x = newPos.x; player.y = newPos.y; updatePlayerPositionInFirestore(); checkEnvelopeCollision(); }
        }
        
        async function updatePlayerPositionInFirestore() {
            if (!gameRoomId || !localPlayerId) return;
            const roomRef = doc(db, `artifacts/${appId}/public/data/romantic-games`, gameRoomId);
            await setDoc(roomRef, { [`players.${localPlayerId}`]: gameState.players[localPlayerId] }, { merge: true });
        }

        function checkEnvelopeCollision() {
            const player = gameState.players[localPlayerId];
            for (const env of gameState.envelopes) { if (!env.found) { const dist = Math.hypot(player.x - env.x, player.y - env.y); if (dist < playerSize / 2 + 10) { handleFoundEnvelope(env.id); break; } } }
        }
        
        async function handleFoundEnvelope(envelopeId) {
            const foundEnvelope = gameState.envelopes.find(e => e.id === envelopeId);
            if (!foundEnvelope || foundEnvelope.found) return;
            if (foundEnvelope.requiresBoth) { const playerIds = Object.keys(gameState.players); if (playerIds.length < 2) { gameTitle.innerText = "Kamu butuh pasanganmu di sini!"; setTimeout(()=> gameTitle.innerText = "Jelajahi Sekolah & Temukan Suratnya!", 2000); return; } const p1 = gameState.players[playerIds[0]]; const p2 = gameState.players[playerIds[1]]; if (Math.hypot(p1.x - p2.x, p1.y - p2.y) > 100) { gameTitle.innerText = "Tunggu pasanganmu mendekat!"; setTimeout(()=> gameTitle.innerText = "Jelajahi Sekolah & Temukan Suratnya!", 2000); return; } }
            await Tone.start().catch(e => console.warn("Tone.js context error:", e));
            if (!synth) { synth = new Tone.Synth({ oscillator: { type: 'sine' }, envelope: { attack: 0.005, decay: 0.1, sustain: 0.3, release: 1 } }).toDestination(); }
            synth.triggerAttackRelease("E5", "8n");
            const roomRef = doc(db, `artifacts/${appId}/public/data/romantic-games`, gameRoomId);
            await setDoc(roomRef, { [`envelopes.${envelopeId}.found`]: true, activeQuestionId: envelopeId }, { merge: true });
        }
        
        function showQuestionModal(envelopeId) {
            const envelope = gameState.envelopes.find(e => e.id === envelopeId);
            if (!envelope) return;
            modalQuestion.innerText = envelope.question;
            modal.style.display = 'flex';
        }
        
        closeModalBtn.addEventListener('click', async () => {
            modal.style.display = 'none';
            const roomRef = doc(db, `artifacts/${appId}/public/data/romantic-games`, gameRoomId);
            await setDoc(roomRef, { activeQuestionId: null }, { merge: true });
            const allFound = gameState.envelopes.every(e => e.found);
            if (allFound) await setDoc(roomRef, { gameOver: true }, { merge: true });
        });
        
        async function init() {
            try {
                const userCredential = await signInAnonymously(auth);
                localPlayerId = userCredential.user.uid;
                const urlParams = new URLSearchParams(window.location.search);
                if (urlParams.has('room')) {
                    const roomIdFromUrl = urlParams.get('room');
                    roomIdInput.value = roomIdFromUrl;
                    joinRoomBtn.disabled = true;
                    createRoomBtn.disabled = true;
                    await joinRoom(roomIdFromUrl);
                }
            } catch (error) {
                console.error("Kesalahan inisialisasi/autentikasi:", error);
                alert("Gagal terhubung ke server. Coba refresh halaman.");
            }
        }
        
        createRoomBtn.addEventListener('click', async () => {
            gameRoomId = `room-${Math.random().toString(36).substr(2, 6)}`;
            const roomRef = doc(db, `artifacts/${appId}/public/data/romantic-games`, gameRoomId);
            const newPlayer = { [localPlayerId]: { id: localPlayerId, name: "Syamil", x: canvas.width * 0.1, y: canvas.height * 0.3, } };
            const initialState = { players: newPlayer, envelopes: envelopes, activeQuestionId: null, gameOver: false };
            await setDoc(roomRef, initialState);
            
            // PERBAIKAN: Menghapus history.pushState dan menampilkan ID untuk dibagikan.
            roomInfo.innerHTML = `Bagikan ID ini ke pasanganmu: <strong style="font-size: 1.2em; color: var(--primary-color); user-select: all;">${gameRoomId}</strong>`;
            roomInfo.style.display = 'block';
            document.getElementById('join-section').style.display = 'none';

            createRoomBtn.innerText = "Menunggu Pasangan Bergabung...";
            createRoomBtn.disabled = true;
            await joinRoom(gameRoomId);
        });
        
        // PERBAIKAN: Listener baru untuk tombol gabung.
        joinRoomBtn.addEventListener('click', async () => {
            const roomId = roomIdInput.value.trim();
            if (roomId) {
                joinRoomBtn.disabled = true;
                createRoomBtn.disabled = true;
                await joinRoom(roomId);
            } else {
                alert("Harap masukkan Room ID.");
            }
        });

        async function joinRoom(roomId) {
            gameRoomId = roomId;
            const roomRef = doc(db, `artifacts/${appId}/public/data/romantic-games`, gameRoomId);
            
            unsubscribe = onSnapshot(roomRef, (doc) => {
                if (!doc.exists()) { alert("Room ini sudah tidak ada."); window.location.search = ""; return; }
                gameState = doc.data();
                if(!gameState) return;
                if (Object.keys(gameState.players).length === 2 && !gameState.gameOver) { startScreen.style.display = 'none'; gameScreen.style.display = 'block'; endScreen.style.display = 'none'; }
                if (gameState.activeQuestionId !== null && modal.style.display === 'none') { showQuestionModal(gameState.activeQuestionId); }
                if (gameState.gameOver) { startScreen.style.display = 'none'; gameScreen.style.display = 'none'; endScreen.style.display = 'block'; if (unsubscribe) unsubscribe(); }
                draw();
            });

            const roomSnap = await getDoc(roomRef);
            if (!roomSnap.exists()) return;
            let currentPlayers = roomSnap.data().players || {};
            if (!currentPlayers[localPlayerId] && Object.keys(currentPlayers).length < 2) {
                const newPlayerData = { id: localPlayerId, name: "Intan", x: canvas.width * 0.9, y: canvas.height * 0.3 };
                await setDoc(roomRef, { [`players.${localPlayerId}`]: newPlayerData }, { merge: true });
            }
            
            window.addEventListener('keydown', handleKeyDown);
        }

        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();
        init();
    </script>
</body>
</html>
