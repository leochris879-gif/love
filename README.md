<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>Ponnuh | Happy Valentine's</title> 

  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.3/dist/confetti.browser.min.js"></script>

  <style>
    :root {
      --bg1: #fff9e6; 
      --bg2: #ffeef6;
      --card: #ffffffcc;
      --yes: #ff4d6d; 
      --next: #ffcc00;
      --text: #5c4d00;
      --successBg1: #fff0f3;
      --successBg2: #ffccd5;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      min-height: 100svh;
      display: grid;
      place-items: center;
      background: radial-gradient(circle at top, var(--bg2), var(--bg1));
      font-family: system-ui, sans-serif;
      overflow: hidden;
      padding: 16px;
      transition: background 1s ease;
    }

    body.final-step {
      background: radial-gradient(circle at top, var(--successBg2), var(--successBg1));
    }

    #confettiCanvas {
      position: fixed;
      inset: 0;
      width: 100vw;
      height: 100vh;
      pointer-events: none;
      z-index: 9999;
    }

    .card {
      width: min(720px, 92vw);
      padding: 40px 22px;
      background: var(--card);
      backdrop-filter: blur(10px);
      border-radius: 22px;
      text-align: center;
      box-shadow: 0 18px 60px rgba(0,0,0,.15);
      position: relative;
      z-index: 10;
    }

    h1 {
      font-size: clamp(26px, 5vw, 44px);
      margin: 12px 0 24px;
      color: var(--text);
    }

    .art {
      width: min(200px, 60vw);
      margin: 0 auto 20px;
      display: block;
      filter: drop-shadow(0 10px 14px rgba(0,0,0,.12));
    }

    /* Shared Button Styles */
    button {
      padding: 16px 32px;
      font-size: 18px;
      font-weight: 800;
      border-radius: 999px;
      border: none;
      cursor: pointer;
      box-shadow: 0 10px 24px rgba(0,0,0,.14);
      user-select: none;
      transition: transform .12s ease, background .2s ease;
    }

    /* Page 1 Specific */
    #nextBtn { background: var(--next); color: #333; position: relative; }

    /* Page 2 Specific */
    .button-zone {
      position: relative;
      width: 100%;
      height: 250px;
      margin-top: 20px;
      touch-action: none;
    }

    #yesBtn {
      position: absolute;
      left: 20%;
      top: 50%;
      transform: translateY(-50%);
      background: var(--yes);
      color: white;
    }

    #noBtn {
      position: absolute;
      left: 60%;
      top: 50%;
      transform: translateY(-50%);
      background: #e5e7eb;
      color: #111827;
    }

    /* Results */
    #page2, #page3 { display: none; }

    .result-text {
      font-size: clamp(32px, 7vw, 60px);
      color: #ff4d6d;
      line-height: 1.2;
      font-weight: 800;
      animation: pop .5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    }

    @keyframes pop {
      from { transform: scale(0.5); opacity: 0; }
      to { transform: scale(1); opacity: 1; }
    }
  </style>
</head>

<body>
  <canvas id="confettiCanvas"></canvas>

  <main class="card">
    <div id="page1">
      <svg class="art" viewBox="0 0 320 240" xmlns="http://www.w3.org/2000/svg">
        <path d="M250 50 C250 33 270 25 282 38 C294 25 314 33 314 50 C314 78 282 92 282 106 C282 92 250 78 250 50Z" fill="#ffd700"/>
        <path d="M90 120 C90 70 140 40 190 60 C240 40 290 70 290 120 C290 180 240 210 190 210 C140 210 90 180 90 120Z" fill="#f7c7a1"/>
        <circle cx="160" cy="130" r="8"/><circle cx="220" cy="130" r="8"/>
        <path d="M190 144 C186 144 182 148 182 152 C182 160 190 164 190 170 C190 164 198 160 198 152 C198 148 194 144 190 144Z" fill="#ffcc00"/>
      </svg>
      <h1>Can You Click The Continue For Me <br>My Sweetheart! 🌹</h1>
      <button id="nextBtn">Continue ❤️</button>
    </div>

    <div id="page2">
      <h1>Will you be my valentine?</h1>
      <section class="button-zone" id="zone">
        <button id="yesBtn">Yes</button>
        <button id="noBtn">No</button>
      </section>
    </div>

    <div id="page3">
      <div class="result-text">I love you baby! <br>💗</div>
    </div>
  </main>

  <script>
    const p1 = document.getElementById("page1");
    const p2 = document.getElementById("page2");
    const p3 = document.getElementById("page3");
    
    const nextBtn = document.getElementById("nextBtn");
    const yesBtn = document.getElementById("yesBtn");
    const noBtn = document.getElementById("noBtn");
    const zone = document.getElementById("zone");
    
    const confettiInstance = confetti.create(document.getElementById("confettiCanvas"), { resize: true });
    const heartEmoji = confetti.shapeFromText({ text: '💗' });

    // Transition P1 to P2
    nextBtn.addEventListener("click", () => {
      p1.style.display = "none";
      p2.style.display = "block";
    });

    // Runaway No Button Logic
    function moveNo(clientX, clientY) {
      const z = zone.getBoundingClientRect();
      const b = noBtn.getBoundingClientRect();
      
      // Calculate a random position within the container bounds
      const maxX = z.width - b.width;
      const maxY = z.height - b.height;
      
      const newLeft = Math.random() * maxX;
      const newTop = Math.random() * maxY;

      noBtn.style.left = newLeft + "px";
      noBtn.style.top = newTop + "px";
      noBtn.style.transform = "none";
    }

    // Trigger move on hover or click
    noBtn.addEventListener("mouseover", (e) => moveNo(e.clientX, e.clientY));
    noBtn.addEventListener("click", (e) => moveNo(e.clientX, e.clientY));

    // Transition P2 to P3
    yesBtn.addEventListener("click", () => {
      document.body.classList.add('final-step');
      p2.style.display = "none";
      p3.style.display = "block";
      heartRain();
    });

    function heartRain() {
      const end = Date.now() + 30000; 
      (function frame() {
        confettiInstance({
          particleCount: 3,
          angle: 60,
          spread: 55,
          origin: { x: 0, y: 0.5 },
          shapes: [heartEmoji],
          scalar: 2
        });
        confettiInstance({
          particleCount: 3,
          angle: 120,
          spread: 55,
          origin: { x: 1, y: 0.5 },
          shapes: [heartEmoji],
          scalar: 2
        });

        if (Date.now() < end) requestAnimationFrame(frame);
      })();
    }
  </script>
</body>
</html>
