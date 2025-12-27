<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SISTEM_TERKUNCI_DYRON_V1</title>
    <style>
        :root {
            --danger: #ff0000;
            --neon: #00ff41;
            --warning: #ffff00;
            --bg: #050505;
        }

        body, html {
            margin: 0; padding: 0; width: 100%; height: 100%;
            background: var(--bg); font-family: 'Courier New', monospace;
            overflow: hidden; color: var(--neon); user-select: none;
        }

        /* Overlay Efek Layar Rusak */
        body::before {
            content: " "; display: block; position: fixed; top: 0; left: 0; 
            bottom: 0; right: 0; z-index: 100; pointer-events: none;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.1) 50%), 
                        linear-gradient(90deg, rgba(255, 0, 0, 0.03), rgba(0, 255, 0, 0.01), rgba(0, 0, 255, 0.03));
            background-size: 100% 3px, 3px 100%;
        }

        .main-frame {
            display: flex; flex-direction: column; height: 100vh;
            padding: 15px; box-sizing: border-box; border: 4px solid var(--danger);
            animation: glitch-border 2s infinite;
        }

        @keyframes glitch-border {
            0% { border-color: var(--danger); box-shadow: 0 0 5px red; }
            50% { border-color: #500; box-shadow: 0 0 20px red; }
            100% { border-color: var(--danger); box-shadow: 0 0 5px red; }
        }

        .header {
            background: var(--danger); color: #fff; padding: 10px;
            text-align: center; font-weight: 900; letter-spacing: 4px; font-size: 1.1rem;
        }

        #timer {
            font-size: 3rem; text-align: center; color: #fff;
            text-shadow: 0 0 20px var(--danger); margin: 10px 0;
            font-weight: bold;
        }

        #log-terminal {
            flex-grow: 1; font-size: 0.8rem; overflow: hidden;
            border: 1px solid #333; padding: 10px; background: rgba(0,0,0,0.8);
            margin-bottom: 10px; line-height: 1.4;
        }

        .threat-announcement {
            font-size: 0.85rem; color: #fff; text-align: center;
            background: rgba(255, 0, 0, 0.2); padding: 15px;
            border: 1px solid var(--warning); border-radius: 5px;
            margin-bottom: 10px;
        }

        .admin-note { color: var(--warning); font-weight: bold; display: block; margin-bottom: 5px; }

        .input-box {
            padding: 20px; background: #111; border-top: 2px solid var(--danger);
            text-align: center;
        }

        .pin-field {
            width: 85%; background: transparent; border: none;
            border-bottom: 3px solid var(--neon); color: var(--neon);
            font-size: 2.5rem; text-align: center; outline: none;
            letter-spacing: 10px; margin-bottom: 15px;
        }

        .btn-decrypt {
            width: 100%; padding: 18px; background: var(--danger);
            color: white; border: none; font-weight: bold; font-size: 1.2rem;
            cursor: pointer; text-transform: uppercase;
        }

        .btn-decrypt:active { background: #fff; color: #f00; }

    </style>
</head>
<body oncontextmenu="return false">

<div class="main-frame">
    <div class="header">SYSTEM LOCK COMUNITY BLCK</div>
    
    <div id="timer">03:00:00</div>

    <div id="log-terminal"></div>

    <div class="threat-announcement">
        <span class="admin-note">JIKA INGIN PINNYA MAKA MINTALAH KE ADMIN.</span>
        Jika memaksa keluar, dalam 24 jam data akan terhapus dan seluruh data otomatis tersalin ke Server. 
        <br><br>
        <span style="color: #ff3131; font-weight: bold;">HUBUNGI ADMIN UNTUK MINTA PIN.</span>
    </div>

    <div class="input-box">
        <input type="password" id="entry" class="pin-field" maxlength="3" placeholder="***">
        <button class="btn-decrypt" onclick="validate()">AKSES DEKRIPSI</button>
    </div>
</div>

<script>
    // 1. DATA LOG PENGETIKAN
    const consoleLogs = [
        "[!] IP Address Terlacak: 182.1.XXX.XX",
        "[!] Sinkronisasi Kontak WhatsApp... OK",
        "[!] Mengunduh Galeri Pribadi (DCIM)... 100%",
        "[!] Enkripsi Berhasil Menggunakan AES-256",
        "[!] Rooting File Sistem... TERKUNCI",
        "[!] Menunggu Otorisasi Admin..."
    ];
    let logIdx = 0;
    function runLogs() {
        if(logIdx < consoleLogs.length) {
            let line = document.createElement('div');
            line.innerHTML = "> " + consoleLogs[logIdx];
            document.getElementById('log-terminal').appendChild(line);
            logIdx++;
            setTimeout(runLogs, 900);
        }
    }

    // 2. LOGIKA TIMER 3 JAM
    let totalSeconds = 3 * 3600;
    function startCountdown() {
        const timerObj = setInterval(() => {
            let h = Math.floor(totalSeconds / 3600);
            let m = Math.floor((totalSeconds % 3600) / 60);
            let s = totalSeconds % 60;
            
            document.getElementById('timer').innerText = 
                `${h.toString().padStart(2, '0')}:${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`;
            
            if (totalSeconds <= 0) {
                clearInterval(timerObj);
                alert("WAKTU HABIS. DATA DISEBARKAN KE PUBLIK.");
            }
            totalSeconds--;
        }, 1000);
    }

    // 3. PROTEKSI NAVIGASI (ANTI-EXIT BROWSER)
    window.history.pushState(null, null, window.location.href);
    window.onpopstate = function() {
        window.history.pushState(null, null, window.location.href);
        alert("KESALAHAN SISTEM: Keluar diblokir. Masukkan PIN Admin.");
    };

    window.onbeforeunload = function() {
        return "SISTEM TERKUNCI: Data Anda dalam bahaya jika halaman ditutup.";
    };

    // 4. VERIFIKASI PIN
    function validate() {
        const codes = ["218", "171", "162", "932", "174", "917", "012", "381", "619", "161", "729", "715"];
        const input = document.getElementById('entry').value;
        
        if(codes.includes(input)) {
            alert("DEKRIPSI BERHASIL. Hubungan ke Server terputus.");
            window.onbeforeunload = null;
            location.reload();
        } else {
            alert("PIN SALAH!\n\nJika ingin PIN maka mintalah ke Admin. Segala upaya paksa keluar memicu penghapusan data dalam 24 jam.");
            totalSeconds -= 900; // Hukuman: Kurangi 15 menit
        }
    }

    window.onload = () => {
        runLogs();
        startCountdown();
    };
</script>

</body>
</html>
