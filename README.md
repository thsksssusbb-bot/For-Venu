<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>for VENU</title>
  <!-- readable font Quicksand -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@300..700&display=swap" rel="stylesheet">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      min-height: 100vh;
      background-color: #ffd9df;  /* soft pink */
      font-family: 'Quicksand', sans-serif;
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
      margin: 0;
      overflow-x: hidden;
    }

    /* ---------- background roses (floating, low opacity) ---------- */
    .rose-garden {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 1;
      overflow: hidden;
    }

    .rose-garden span {
      position: absolute;
      user-select: none;
      font-size: 3rem;
      opacity: 0.15;
      transform-origin: center;
      color: #d64f7c;
      filter: drop-shadow(0 0 2px rgba(255, 240, 245, 0.3));
      animation: softDrift 12s infinite alternate ease-in-out;
    }

    @keyframes softDrift {
      0% { transform: translateY(0px) rotate(0deg); }
      100% { transform: translateY(15px) rotate(4deg); }
    }

    /* ---------- envelope wrapper (contains envelope, side bouquets, button) ---------- */
    .envelope-wrapper {
      position: relative;
      z-index: 20;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      width: 100%;
      padding: 1rem;
    }

    /* envelope container */
    .envelope {
      position: relative;
      width: 540px;
      height: 380px;
      margin: 1rem auto 0.5rem auto;
      transition: opacity 0.4s ease;
    }

    /* hide envelope when phase 2 triggers */
    .envelope.hide-phase2 {
      opacity: 0;
      pointer-events: none;
    }

    /* envelope body – realistic details */
    .envelope-body {
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 85%;
      background: #ffffff;
      background: linear-gradient(145deg, #fff 0%, #fcf3f3 100%);
      border-radius: 24px 24px 20px 20px;
      border: 3px solid #f0b0b8;
      box-shadow: 0 20px 28px rgba(170, 80, 100, 0.3), inset 0 -6px 10px rgba(0,0,0,0.02);
      z-index: 5;
    }

    .envelope-body::before {
      content: '';
      position: absolute;
      top: -10px;
      left: 10%;
      width: 80%;
      height: 24px;
      background: radial-gradient(circle at 30% 50%, rgba(255,240,240,0.9) 0%, rgba(255,255,255,0.2) 70%);
      border-radius: 50%;
      filter: blur(6px);
      opacity: 0.8;
      pointer-events: none;
      z-index: 2;
    }

    .envelope-body::after {
      content: '';
      position: absolute;
      top: 15px;
      left: 8%;
      width: 84%;
      height: 3px;
      background: rgba(220, 150, 160, 0.2);
      border-radius: 2px;
      box-shadow: 0 1px 2px rgba(255,255,255,0.8);
      z-index: 3;
    }

    /* heart flap */
    .heart-flap {
      position: absolute;
      top: -28px;
      left: 50%;
      transform: translateX(-50%);
      width: 200px;
      height: 170px;
      cursor: pointer;
      z-index: 30;
      transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1), opacity 0.3s;
      transform-origin: top center;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .heart-flap svg {
      width: 100%;
      height: 100%;
      display: block;
      filter: drop-shadow(2px 10px 12px rgba(190, 70, 90, 0.4));
    }

    .click-text {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-family: 'Quicksand', sans-serif;
      font-weight: 600;
      font-size: 1.3rem;
      letter-spacing: 0.5px;
      color: #ffffff;
      text-shadow: 2px 2px 6px #b33b5c, 0 0 8px #a12a4a;
      background: rgba(230, 120, 140, 0.25);
      padding: 5px 18px;
      border-radius: 40px;
      white-space: nowrap;
      pointer-events: none;
      z-index: 35;
      backdrop-filter: blur(4px);
      border: 1px solid rgba(255,255,255,0.6);
    }

    /* paper message */
    .paper-message {
      position: absolute;
      bottom: 30px;
      left: 12%;
      width: 76%;
      background: #ffffff;
      background: linear-gradient(145deg, #fff, #fff7f2);
      border-radius: 30px 30px 22px 22px;
      padding: 32px 22px;
      text-align: center;
      font-family: 'Quicksand', sans-serif;
      font-size: 2rem;
      font-weight: 500;
      color: #b13e5c;
      box-shadow: 0 18px 26px rgba(150, 40, 60, 0.3), inset 0 0 0 3px #ffe2e2;
      z-index: 2;
      transform: translateY(100%);
      opacity: 0;
      transition: transform 0.65s ease-out 0.1s, opacity 0.45s ease-in;
      line-height: 1.4;
      border: 2px solid #fecaca;
      word-break: break-word;
    }

    .paper-message::before {
      content: '📬';
      position: absolute;
      top: -20px;
      left: 20px;
      font-size: 2.4rem;
      opacity: 0.25;
      transform: rotate(-12deg);
      filter: drop-shadow(0 5px 5px #f5b0b0);
    }

    .envelope.open .paper-message {
      transform: translateY(-20%) translateZ(0);
      opacity: 1;
      z-index: 40;
      box-shadow: 0 32px 48px rgba(160, 60, 80, 0.45);
    }

    .envelope.open .heart-flap {
      transform: translateX(-50%) rotateX(70deg) translateY(-28px);
      opacity: 0.7;
    }

    /* ---------- ROSE BOUQUETS (left & right) that remain through both phases ---------- */
    .bouquet {
      position: absolute;
      bottom: 30px;
      width: 130px;
      pointer-events: none;
      z-index: 15;
      opacity: 0;
      transform: translateY(100%);
      transition: transform 0.7s ease-out 0.15s, opacity 0.5s ease-in 0.1s;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 6px 2px;
      font-size: 2.5rem;
      line-height: 1;
      filter: drop-shadow(0 10px 12px rgba(180, 70, 90, 0.2));
    }

    .left-bouquet {
      left: 0px;
      transform-origin: bottom center;
    }

    .right-bouquet {
      right: 0px;
      transform-origin: bottom center;
    }

    .bouquet span {
      display: inline-block;
      animation: gentleWobble 4s infinite alternate ease-in-out;
    }

    .bouquet span:nth-child(odd) {
      font-size: 2.8rem;
      transform: rotate(-5deg);
    }
    .bouquet span:nth-child(even) {
      font-size: 2.2rem;
      transform: rotate(5deg);
      margin-top: -10px;
    }
    .bouquet .leaf {
      font-size: 2rem;
      opacity: 0.7;
      transform: rotate(15deg);
    }

    @keyframes gentleWobble {
      0% { transform: translateY(0px) rotate(-2deg); }
      100% { transform: translateY(6px) rotate(4deg); }
    }

    /* when envelope opens, bouquets rise */
    .envelope.open ~ .left-bouquet,
    .envelope.open ~ .right-bouquet {
      opacity: 1;
      transform: translateY(0);
    }

    /* ---------- CLICK AGAIN BUTTON (appears after first open) ---------- */
    .again-button {
      margin-top: 20px;
      padding: 15px 40px;
      background: #ffb3c6;
      border: 3px solid #ffffff;
      border-radius: 60px;
      font-family: 'Quicksand', sans-serif;
      font-size: 1.8rem;
      font-weight: 600;
      color: #b3415c;
      box-shadow: 0 8px 0 #b35f7a, 0 12px 20px rgba(180, 60, 80, 0.3);
      cursor: pointer;
      transition: all 0.1s ease;
      letter-spacing: 1px;
      text-shadow: 0 2px 5px white;
      display: none;  /* hidden initially */
      z-index: 50;
    }

    .again-button:active {
      transform: translateY(7px);
      box-shadow: 0 4px 0 #b35f7a, 0 12px 20px rgba(180, 60, 80, 0.3);
    }

    .again-button.show {
      display: block;
    }

    /* ---------- GRAND BOUQUET (full page, elastic, with card) ---------- */
    .grand-bouquet {
      position: fixed;            /* covers whole viewport */
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background-color: rgba(255, 220, 230, 0.3); /* soft transparent pink */
      backdrop-filter: blur(2px); /* dreamy effect */
      z-index: 100;               /* above most things */
      opacity: 0;
      transform: scale(0.5);
      transition: opacity 0.4s ease, transform 0.8s cubic-bezier(0.34, 1.56, 0.64, 1); /* elastic */
      pointer-events: none;       /* so clicks pass through if needed, but card might need pointer? we'll set card to auto */
    }

    .grand-bouquet.show {
      opacity: 1;
      transform: scale(1);
    }

    /* massive flower arrangement covering whole screen */
    .flower-field {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: center;
      gap: 2rem 3rem;
      padding: 2rem;
      pointer-events: none;       /* allow clicks to pass to card */
      z-index: 101;
    }

    .flower-field span {
      font-size: 8vw;              /* responsive huge flowers */
      line-height: 1;
      display: inline-block;
      animation: float 6s infinite alternate ease-in-out;
      filter: drop-shadow(0 10px 20px rgba(200, 80, 110, 0.5));
      transform: rotate(var(--rot, 0deg));
    }

    /* random rotation for each flower */
    .flower-field span:nth-child(odd) {
      --rot: -8deg;
    }
    .flower-field span:nth-child(even) {
      --rot: 12deg;
    }
    .flower-field span:nth-child(3n) {
      --rot: 5deg;
    }
    .flower-field span:nth-child(5n) {
      --rot: -12deg;
    }

    @keyframes float {
      0% { transform: translateY(0px) rotate(var(--rot)); }
      100% { transform: translateY(-30px) rotate(calc(var(--rot) + 4deg)); }
    }

    /* the beautiful card */
    .card-message {
      position: relative;
      z-index: 110;               /* on top of flowers */
      background: #fff5f0;
      border-radius: 60px 60px 30px 30px;
      padding: 30px 50px;
      font-family: 'Quicksand', sans-serif;
      font-size: 3.5rem;
      font-weight: 500;
      color: #b13e5c;
      box-shadow: 0 30px 40px rgba(160, 60, 80, 0.5), inset 0 0 0 6px #ffe2e2;
      border: 4px solid #ffb6c1;
      text-align: center;
      line-height: 1.3;
      max-width: 90vw;
      transform: rotate(-1deg);
      pointer-events: auto;        /* can be clicked if needed */
    }

    .card-message span {
      font-size: 4rem;
      display: inline-block;
      margin: 0 0.5rem;
    }

    /* ensure side bouquets are above the grand bouquet */
    .left-bouquet, .right-bouquet {
      z-index: 200;                /* higher than grand bouquet's 100 */
    }

    /* hide envelope and button in phase 2 */
    .phase2-hidden {
      opacity: 0 !important;
      pointer-events: none !important;
    }

    /* responsive */
    @media (max-width: 700px) {
      .envelope {
        width: 460px;
        height: 330px;
      }
      .bouquet {
        width: 100px;
        font-size: 2rem;
      }
      .card-message {
        font-size: 2.5rem;
        padding: 20px 30px;
      }
    }

    @media (max-width: 550px) {
      .envelope {
        width: 380px;
        height: 280px;
      }
      .heart-flap {
        width: 160px;
        height: 135px;
        top: -22px;
      }
      .paper-message {
        font-size: 1.6rem;
        padding: 24px 16px;
      }
      .click-text {
        font-size: 1.1rem;
      }
      .bouquet {
        width: 80px;
        font-size: 1.8rem;
        bottom: 20px;
      }
      .again-button {
        font-size: 1.5rem;
        padding: 12px 30px;
      }
      .card-message {
        font-size: 2rem;
        padding: 15px 20px;
      }
      .flower-field span {
        font-size: 15vw;          /* even bigger on small screens */
      }
    }
  </style>
</head>
<body>
  <!-- background roses -->
  <div class="rose-garden" id="roseGarden"></div>

  <!-- main container -->
  <div class="envelope-wrapper">
    <!-- envelope (phase 1) -->
    <div class="envelope" id="envelope">
      <div class="envelope-body"></div>

      <div class="heart-flap" id="heartFlap" onclick="openEnvelope()">
        <svg viewBox="0 0 100 100" preserveAspectRatio="none">
          <path d="M50,20 C20,20 20,60 50,80 C80,60 80,20 50,20" fill="#ff9eb5" stroke="#dc6e8a" stroke-width="3"/>
        </svg>
        <span class="click-text">click here</span>
      </div>

      <div class="paper-message" id="apologyPaper">
        I am sorry for not sending the voice note. 💕
      </div>
    </div>

    <!-- rose bouquets (side) – they stay visible through both phases -->
    <div class="bouquet left-bouquet" id="leftBouquet">
      <span>🌹</span><span class="leaf">🌿</span><span>🌹</span><span>🌹</span><span class="leaf">🌱</span><span>🌹</span>
    </div>
    <div class="bouquet right-bouquet" id="rightBouquet">
      <span>🌹</span><span>🌹</span><span class="leaf">🌿</span><span>🌹</span><span>🌹</span><span class="leaf">🌿</span>
    </div>

    <!-- "Click again" button (appears after first open) -->
    <button class="again-button" id="againBtn" onclick="phaseTwo()">Click again</button>

    <!-- GRAND BOUQUET – full page, covers everything, with card -->
    <div class="grand-bouquet" id="grandBouquet">
      <div class="flower-field" id="flowerField">
        <!-- lots of flower emojis to cover the page -->
        <span>🌹</span><span>🌺</span><span>🌸</span><span>🌼</span><span>🌷</span><span>🌻</span>
        <span>🌹</span><span>🌺</span><span>🌸</span><span>🌼</span><span>🌷</span><span>🌻</span>
        <span>🌹</span><span>🌺</span><span>🌸</span><span>🌼</span><span>🌷</span><span>🌻</span>
        <span>🌹</span><span>🌺</span><span>🌸</span><span>🌼</span><span>🌷</span><span>🌻</span>
        <span>🌹</span><span>🌺</span><span>🌸</span><span>🌼</span><span>🌷</span><span>🌻</span>
        <span>🌹</span><span>🌺</span><span>🌸</span><span>🌼</span><span>🌷</span><span>🌻</span>
        <span>🌹</span><span>🌺</span><span>🌸</span><span>🌼</span><span>🌷</span><span>🌻</span>
      </div>
      <div class="card-message">
        <span>💌</span> smile everyday my dear VENU <span>💌</span>
      </div>
    </div>
  </div>

  <script>
    (function() {
      // background roses
      const roseGarden = document.getElementById('roseGarden');
      const totalRoses = 70;
      for (let i = 0; i < totalRoses; i++) {
        const rose = document.createElement('span');
        rose.textContent = '🌹';
        const left = Math.random() * 100;
        const top = Math.random() * 100;
        const size = 1.4 + Math.random() * 3.5;
        const opacity = 0.08 + Math.random() * 0.15;
        const rotate = Math.random() * 40 - 20;
        const delay = Math.random() * 6;
        rose.style.left = left + '%';
        rose.style.top = top + '%';
        rose.style.fontSize = size + 'rem';
        rose.style.opacity = opacity;
        rose.style.transform = `rotate(${rotate}deg)`;
        rose.style.animationDelay = delay + 's';
        rose.style.animationDuration = 8 + Math.random() * 8 + 's';
        roseGarden.appendChild(rose);
      }

      // elements
      const envelope = document.getElementById('envelope');
      const heartFlap = document.getElementById('heartFlap');
      const againBtn = document.getElementById('againBtn');
      const grandBouquet = document.getElementById('grandBouquet');
      const leftBouquet = document.getElementById('leftBouquet');
      const rightBouquet = document.getElementById('rightBouquet');

      let firstOpened = false;

      // phase 1: open envelope (called from heart flap)
      window.openEnvelope = function() {
        if (firstOpened) return;
        firstOpened = true;
        envelope.classList.add('open');
        heartFlap.style.pointerEvents = 'none';  // disable further clicks

        // show the "Click again" button after a short delay (once paper is up)
        setTimeout(() => {
          againBtn.classList.add('show');
        }, 400);
      };

      // phase 2: hide envelope, button; show full‑page grand bouquet; keep side bouquets
      window.phaseTwo = function() {
        // hide envelope and again button
        envelope.classList.add('phase2-hidden');
        againBtn.classList.remove('show');
        againBtn.style.display = 'none';

        // side bouquets remain, but ensure they are above the grand bouquet
        leftBouquet.style.zIndex = '200';
        rightBouquet.style.zIndex = '200';

        // show grand bouquet with elastic animation
        grandBouquet.classList.add('show');
      };
    })();
  </script>
  <!-- Phase 1: heart flap opens → paper rises → "Click again" button.
       Phase 2: click again → envelope fades, side bouquets stay, and a gigantic, elastic bouquet fills the whole screen with the card. -->
</body>
</html>
