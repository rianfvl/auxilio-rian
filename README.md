<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Legit Rian</title>
<style>
  html, body {
    margin: 0; padding: 0; background: #000; height: 100%;
    font-family: Arial, sans-serif; overflow: hidden;
  }
  canvas { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 0; }

  /* --- LOGIN --- */
  #loginOverlay {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    background: #000; display: flex; justify-content: center;
    align-items: center; z-index: 100; transition: opacity 0.5s ease;
  }
  .login-card {
    background: rgba(20, 20, 20, 0.95); padding: 30px; border-radius: 15px;
    width: 280px; text-align: center; border: 1px solid #7b59ff;
    box-shadow: 0 0 20px rgba(123, 89, 255, 0.3);
  }
  .login-card input {
    width: 90%; padding: 12px; margin-bottom: 15px; background: #111;
    border: 1px solid #333; border-radius: 8px; color: #fff; text-align: center;
  }
  .login-card button {
    width: 100%; padding: 12px; background: #7b59ff; color: white;
    border: none; border-radius: 8px; font-weight: bold; cursor: pointer;
  }

  /* --- PAINEL --- */
  .container {
    display: none; z-index: 1; position: absolute;
    top: 50%; left: 50%; transform: translate(-50%, -50%);
    width: 90%; max-width: 280px; background: rgba(10,10,10,0.85);
    border-radius: 14px; padding: 18px; box-shadow: 0 0 15px rgba(147,112,219,0.25);
    backdrop-filter: blur(5px); color: white;
  }
  details { background: rgba(30,30,30,0.9); border-radius: 10px; padding: 10px; margin-bottom: 12px; }
  summary { cursor: pointer; font-weight: bold; }
  label { display: flex; align-items: center; margin-top: 8px; font-size: 14px; cursor: pointer; }
  input[type="checkbox"] { margin-right: 10px; accent-color: #9370DB; cursor: pointer; }
  
  .inject-btn {
    margin-top: 20px; width: 100%; padding: 12px; border-radius: 12px; border: none;
    font-weight: bold; background: linear-gradient(90deg, #7b59ff, #b59fff);
    color: white; cursor: pointer;
  }
  #injectMsg { margin-top: 10px; text-align: center; color: #b59fff; font-size: 13px; }
  .error-msg { color: #ff4444; font-size: 12px; display: none; margin-bottom: 10px; }
</style>
</head>
<body>

<div id="loginOverlay">
  <div class="login-card">
    <h2>Acesso Rian</h2>
    <input type="text" id="keyInput" placeholder="Digite sua Key">
    <div id="errorText" class="error-msg">Key Incorreta!</div>
    <button onclick="validaLogin()">Entrar</button>
  </div>
</div>

<canvas id="bg"></canvas>

<div class="container" id="painelPrincipal">
  <h2>Painel Rian Xiter</h2>
  <details>
    <summary>Sistema Avançado</summary>
    <label><input type="checkbox" onchange="playToggleSound()"> 120 FPS</label>
    <label><input type="checkbox" onchange="playToggleSound()"> No Recoil</label>
  </details>
  <details>
    <summary>Ajustes de Mira</summary>
    <label><input type="checkbox" onchange="playToggleSound()"> Aimbot 90%</label>
    <label><input type="checkbox" onchange="playToggleSound()"> Neural Aim</label>
  </details>
  <button class="inject-btn" onclick="inject()">Injetar Scripts</button>
  <div id="injectMsg"></div>
</div>

<script>
// Gerador de sons via Web Audio API (Não precisa de arquivos externos)
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

function playSound(freq, type, duration) {
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.type = type;
    osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
    gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + duration);
    osc.connect(gain);
    gain.connect(audioCtx.destination);
    osc.start();
    osc.stop(audioCtx.currentTime + duration);
}

function playToggleSound() {
    playSound(800, 'sine', 0.1); // Som de clique curto
}

function playSuccessSound() {
    playSound(600, 'square', 0.1);
    setTimeout(() => playSound(900, 'square', 0.2), 100);
}

function validaLogin() {
    const key = document.getElementById('keyInput').value;
    if (key === "Rianxiter26") {
        playSuccessSound();
        document.getElementById('loginOverlay').style.opacity = '0';
        setTimeout(() => {
            document.getElementById('loginOverlay').style.display = 'none';
            document.getElementById('painelPrincipal').style.display = 'block';
            animate();
        }, 500);
    } else {
        document.getElementById('errorText').style.display = 'block';
        playSound(150, 'sawtooth', 0.3); // Som de erro grave
    }
}

function inject() {
    let btn = document.querySelector(".inject-btn");
    let msg = document.getElementById("injectMsg");
    btn.disabled = true;
    
    // Som de carregamento repetido
    let loop = setInterval(() => playSound(400, 'sine', 0.05), 200);
    
    btn.innerText = "Injetando...";
    setTimeout(() => {
        clearInterval(loop);
        playSuccessSound();
        btn.innerText = "Sucesso!";
        msg.innerText = "Pastas injetadas com sucesso!";
        btn.disabled = false;
    }, 2000);
}

// --- CÓDIGO DO CANVAS (FUNDO) ---
const canvas = document.getElementById('bg');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
let particles = [];

for (let i = 0; i < 70; i++) {
  particles.push({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    vx: (Math.random() - 0.5) * 1,
    vy: (Math.random() - 0.5) * 1,
  });
}

function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  particles.forEach((p, i) => {
    p.x += p.vx; p.y += p.vy;
    if (p.x < 0 || p.x > canvas.width) p.vx *= -1;
    if (p.y < 0 || p.y > canvas.height) p.vy *= -1;
    ctx.fillStyle = '#fff';
    ctx.beginPath(); ctx.arc(p.x, p.y, 1.5, 0, Math.PI*2); ctx.fill();

    for (let j = i + 1; j < particles.length; j++) {
      let q = particles[j];
      let dist = Math.hypot(p.x - q.x, p.y - q.y);
      if (dist < 100) {
        ctx.strokeStyle = `rgba(147,112,219,${1 - dist/100})`;
        ctx.beginPath(); ctx.moveTo(p.x, p.y); ctx.lineTo(q.x, q.y); ctx.stroke();
      }
    }
  });
  requestAnimationFrame(animate);
}
</script>
</body>
</html>
