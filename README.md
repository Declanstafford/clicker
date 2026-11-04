<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Cookie Clicker</title>
  <style>
    * { box-sizing: border-box; }

    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      background: #f7e7c6;
      color: #382719;
      font-family: system-ui, sans-serif;
    }

    .game {
      width: min(420px, 92vw);
      padding: 2rem;
      text-align: center;
      background: #fffaf0;
      border: 3px solid #8b5e3c;
      border-radius: 20px;
      box-shadow: 0 10px 30px #38271922;
    }

    h1 { margin-top: 0; }

    #cookie {
      width: 180px;
      height: 180px;
      margin: 1.25rem auto;
      border: 6px solid #8b5e3c;
      border-radius: 50%;
      background: #c98243;
      box-shadow: inset -10px -12px #9f6034;
      font-size: 5rem;
      cursor: pointer;
      transition: transform 0.08s;
    }

    #cookie:active { transform: scale(0.92); }

    .stats { font-size: 1.2rem; }
    #count { font-size: 2rem; font-weight: 800; }

    button.upgrade {
      width: 100%;
      margin-top: 0.75rem;
      padding: 0.8rem;
      border: 0;
      border-radius: 10px;
      background: #8b5e3c;
      color: white;
      font: inherit;
      cursor: pointer;
    }

    button.upgrade:hover:not(:disabled) { background: #70472b; }
    button:disabled { opacity: 0.5; cursor: not-allowed; }

    #message {
      min-height: 1.5em;
      color: #70472b;
    }
  </style>
</head>
<body>
  <main class="game">
    <h1>🍪 Cookie Clicker</h1>
    <div class="stats">
      <div id="count">0 cookies</div>
      <div>Per click: <span id="power">1</span></div>
      <div>Per second: <span id="rate">0</span></div>
    </div>

    <button id="cookie" aria-label="Bake a cookie">🍪</button>
    <p id="message">Click the cookie to get started!</p>

    <button class="upgrade" id="clickUpgrade">
      Stronger Click — Cost: <span id="clickCost">10</span>
    </button>
    <button class="upgrade" id="autoUpgrade">
      Grandma — Cost: <span id="autoCost">25</span> (+1 cookie/sec)
    </button>
  </main>

  <script>
    const saved = JSON.parse(localStorage.getItem("cookieGame") || "{}");

    const game = {
      cookies: saved.cookies || 0,
      power: saved.power || 1,
      rate: saved.rate || 0,
      clickCost: saved.clickCost || 10,
      autoCost: saved.autoCost || 25
    };

    const countEl = document.querySelector("#count");
    const powerEl = document.querySelector("#power");
    const rateEl = document.querySelector("#rate");
    const clickCostEl = document.querySelector("#clickCost");
    const autoCostEl = document.querySelector("#autoCost");
    const clickUpgrade = document.querySelector("#clickUpgrade");
    const autoUpgrade = document.querySelector("#autoUpgrade");
    const message = document.querySelector("#message");

    function save() {
      localStorage.setItem("cookieGame", JSON.stringify(game));
    }

    function render() {
      countEl.textContent = `${Math.floor(game.cookies)} cookies`;
      powerEl.textContent = game.power;
      rateEl.textContent = game.rate;
      clickCostEl.textContent = game.clickCost;
      autoCostEl.textContent = game.autoCost;
      clickUpgrade.disabled = game.cookies < game.clickCost;
      autoUpgrade.disabled = game.cookies < game.autoCost;
      save();
    }

    document.querySelector("#cookie").addEventListener("click", () => {
      game.cookies += game.power;
      message.textContent = `+${game.power} cookie${game.power === 1 ? "" : "s"}!`;
      render();
    });

    clickUpgrade.addEventListener("click", () => {
      if (game.cookies < game.clickCost) return;
      game.cookies -= game.clickCost;
      game.power += 1;
      game.clickCost = Math.ceil(game.clickCost * 1.6);
      message.textContent = "Your clicks are stronger!";
      render();
    });

    autoUpgrade.addEventListener("click", () => {
      if (game.cookies < game.autoCost) return;
      game.cookies -= game.autoCost;
      game.rate += 1;
      game.autoCost = Math.ceil(game.autoCost * 1.7);
      message.textContent = "Grandma is baking cookies!";
      render();
    });

    setInterval(() => {
      game.cookies += game.rate;
      render();
    }, 1000);

    render();
  </script>
</body>
</html>
