<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Apo & Nisa - Sonsuzluk</title>
    <script src="https://www.gstatic.com/firebasejs/9.15.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.15.0/firebase-firestore-compat.js"></script>
    <style>
        :root { --ana: #ff4d6d; --bg: #fff0f3; --yazi: #800e13; }
        body { font-family: 'Segoe UI', sans-serif; background: var(--bg); margin: 0; text-align: center; }
        #login { height: 100vh; display: flex; flex-direction: column; justify-content: center; align-items: center; }
        .box { background: white; padding: 30px; border-radius: 25px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); width: 85%; max-width: 320px; }
        input { width: 100%; padding: 12px; margin: 10px 0; border: 2px solid #ffccd5; border-radius: 12px; box-sizing: border-box; text-align: center; outline: none; }
        .btn { background: var(--ana); color: white; border: none; padding: 12px 25px; border-radius: 12px; font-weight: bold; cursor: pointer; width: 100%; }
        #main { display: none; padding: 15px; }
        .gallery { display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin-top: 20px; padding: 10px; }
        .polaroid { background: white; padding: 10px 10px 20px 10px; box-shadow: 0 4px 10px rgba(0,0,0,0.15); border-radius: 5px; transform: rotate(-2deg); }
        .polaroid:nth-child(even) { transform: rotate(2deg); }
        .polaroid img { width: 100%; height: 150px; object-fit: cover; border-radius: 3px; }
        .upload-card { background: white; padding: 15px; border-radius: 20px; margin-bottom: 20px; box-shadow: 0 5px 15px rgba(0,0,0,0.05); }
    </style>
</head>
<body>

    <audio id="bgMusic" loop><source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg"></audio>

    <div id="login">
        <div class="box">
            <h2 style="color: var(--yazi);">🌹 Apo & Nisa 🌹</h2>
            <input type="text" id="user" placeholder="İsmin (apo/nisa)">
            <input type="password" id="pass" placeholder="Şifre (aponisa)">
            <button class="btn" onclick="giris()">Giriş Yap</button>
        </div>
    </div>

    <div id="main">
        <h2 id="welcome" style="color: var(--yazi);"></h2>
        <div class="upload-card">
            <p style="font-size: 13px; color: #666;">Fotoğraf Linkini Buraya Yapıştır 👇</p>
            <input type="text" id="linkInput" placeholder="https://.../resim.jpg">
            <button class="btn" onclick="ekle()" id="upBtn" style="background:#4caf50;">Duvara As ❤️</button>
        </div>
        <div class="gallery" id="galeri"></div>
    </div>

    <script>
        const firebaseConfig = {
            apiKey: "AIzaSyD76YDSmL28K6yD8I_fu8n04hIOZ6afDso",
            authDomain: "aponisa-ff62b.firebaseapp.com",
            projectId: "aponisa-ff62b",
            storageBucket: "aponisa-ff62b.firebasestorage.app",
            messagingSenderId: "416528485059",
            appId: "1:416528485059:web:116add72489fe3315c7076"
        };

        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();

        function giris() {
            const u = document.getElementById('user').value.toLowerCase().trim();
            const p = document.getElementById('pass').value.trim();
            if ((u === 'apo' || u === 'nisa') && p === 'aponisa') {
                document.getElementById('bgMusic').play();
                document.getElementById('login').style.display = 'none';
                document.getElementById('main').style.display = 'block';
                document.getElementById('welcome').innerText = "Seni Seviyorum " + (u === 'apo' ? 'Apo' : 'Nisa');
                anilarıGetir();
            } else { alert("Yanlış giriş sevgilim!"); }
        }

        async function ekle() {
            const url = document.getElementById('linkInput').value.trim();
            if (!url) return alert("Lütfen bir resim linki yapıştır!");
            
            try {
                await db.collection('anilar').add({ url: url, tarih: Date.now() });
                document.getElementById('linkInput').value = "";
            } catch (e) {
                alert("Hata: " + e.message);
            }
        }

        function anilarıGetir() {
            db.collection('anilar').orderBy('tarih', 'desc').onSnapshot(snap => {
                const gal = document.getElementById('galeri');
                gal.innerHTML = "";
                snap.forEach(doc => {
                    const div = document.createElement('div');
                    div.className = 'polaroid';
                    div.innerHTML = `<img src="${doc.data().url}">`;
                    gal.appendChild(div);
                });
            });
        }
    </script>
</body>
</html>
