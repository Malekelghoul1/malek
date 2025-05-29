<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>شاص سوبر - Super Shas</title>
<style>
  * {
    box-sizing: border-box;
  }
  body, html {
    margin: 0; padding: 0;
    font-family: Arial, sans-serif;
    background: #222;
    color: #fff;
    direction: rtl;
    user-select: none;
    overflow: hidden;
  }
  #langSelect, #startScreen, #gameUI, #shop {
    position: absolute;
    width: 100%; height: 100%;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    background: #111;
  }
  #langSelect button, #startScreen button, #shop button {
    margin: 10px;
    padding: 12px 25px;
    font-size: 18px;
    cursor: pointer;
    border-radius: 10px;
    border: none;
    background: #007bff;
    color: white;
    transition: background 0.3s;
  }
  #langSelect button:hover, #startScreen button:hover, #shop button:hover {
    background: #0056b3;
  }
  #startScreen {
    display: none;
    text-align: center;
  }
  #gameUI {
    display: none;
    flex-direction: column;
    justify-content: flex-start;
    padding: 10px;
    background: #333a;
  }
  #gameCanvas {
    background: #555;
    display: block;
    margin: 0 auto;
    border-radius: 10px;
  }
  #infoBar {
    display: flex;
    justify-content: space-between;
    margin-bottom: 5px;
    font-size: 20px;
  }
  #shop {
    display: none;
    flex-direction: column;
    background: #222;
    border-radius: 15px;
    padding: 15px;
    width: 300px;
  }
  #shop h3 {
    margin-top: 0;
  }
  #shop button {
    width: 100%;
  }
</style>
</head>
<body>

<!-- Language Selection -->
<div id="langSelect">
  <h2 id="langSelectTitle">اختر اللغة / Choisissez la langue / Choose Language / Wählen Sie die Sprache</h2>
  <div>
    <button data-lang="ar">العربية</button>
    <button data-lang="fr">Français</button>
    <button data-lang="en">English</button>
    <button data-lang="de">Deutsch</button>
  </div>
</div>

<!-- Start Screen -->
<div id="startScreen">
  <h1 id="gameTitle">شاص سوبر</h1>
  <div id="vehicleSelection">
    <h2 id="selectVehicleTitle">اختر مركبتك</h2>
    <button data-vehicle="shas">شاص 🚙</button>
    <button data-vehicle="ford">فورد 🚘 (30 ريال)</button>
    <button data-vehicle="bike">دراجة 🚲 (50 ريال)</button>
  </div>
  <div>
    <button id="startGameBtn">ابدأ اللعب</button>
  </div>
  <div id="playerMoneyDisplay" style="margin-top:10px; font-size:18px;"></div>
</div>

<!-- Game UI -->
<div id="gameUI">
  <div id="infoBar">
    <div id="moneyDisplay">💰: 0</div>
    <div id="levelDisplay">🌍: 1</div>
    <button id="openShopBtn">المتجر 🛒</button>
  </div>
  <canvas id="gameCanvas" width="800" height="400"></canvas>
</div>

<!-- Shop -->
<div id="shop">
  <h3 id="shopTitle">المتجر</h3>
  <button data-buy="ford">شراء فورد - 30 ريال 🚘</button>
  <button data-buy="bike">شراء دراجة - 50 ريال 🚲</button>
  <button id="closeShopBtn" style="margin-top: 15px; background:#900;">إغلاق المتجر</button>
</div>

<script>
const texts = {
  ar: {
    langSelectTitle: "اختر اللغة",
    gameTitle: "شاص سوبر",
    selectVehicleTitle: "اختر مركبتك",
    startGame: "ابدأ اللعب",
    money: "💰",
    level: "🌍",
    openShop: "المتجر 🛒",
    shopTitle: "المتجر",
    buyFord: "شراء فورد - 30 ريال 🚘",
    buyBike: "شراء دراجة - 50 ريال 🚲",
    closeShop: "إغلاق المتجر",
    alertNoMoney: "لا يوجد رصيد كافي!",
    alertBought: "تم الشراء بنجاح!",
    alertCrash: "💥 تحطمت المركبة! رجوع لأخر نقطة فحص!",
  },
  fr: {
    langSelectTitle: "Choisissez la langue",
    gameTitle: "Super Shas",
    selectVehicleTitle: "Choisissez votre véhicule",
    startGame: "Démarrer le jeu",
    money: "💰",
    level: "🌍",
    openShop: "Boutique 🛒",
    shopTitle: "Boutique",
    buyFord: "Acheter Ford - 30 Riyals 🚘",
    buyBike: "Acheter Vélo - 50 Riyals 🚲",
    closeShop: "Fermer la boutique",
    alertNoMoney: "Pas assez d'argent!",
    alertBought: "Achat réussi!",
    alertCrash: "💥 Véhicule détruit! Retour au dernier checkpoint!",
  },
  en: {
    langSelectTitle: "Choose Language",
    gameTitle: "Super Shas",
    selectVehicleTitle: "Select Your Vehicle",
    startGame: "Start Game",
    money: "💰",
    level: "🌍",
    openShop: "Shop 🛒",
    shopTitle: "Shop",
    buyFord: "Buy Ford - 30 Riyals 🚘",
    buyBike: "Buy Bike - 50 Riyals 🚲",
    closeShop: "Close Shop",
    alertNoMoney: "Not enough money!",
    alertBought: "Purchase successful!",
    alertCrash: "💥 Vehicle crashed! Returning to last checkpoint!",
  },
  de: {
    langSelectTitle: "Wählen Sie die Sprache",
    gameTitle: "Super Shas",
    selectVehicleTitle: "Wähle dein Fahrzeug",
    startGame: "Spiel starten",
    money: "💰",
    level: "🌍",
    openShop: "Shop 🛒",
    shopTitle: "Shop",
    buyFord: "Ford kaufen - 30 Riyals 🚘",
    buyBike: "Fahrrad kaufen - 50 Riyals 🚲",
    closeShop: "Shop schließen",
    alertNoMoney: "Nicht genug Geld!",
    alertBought: "Kauf erfolgreich!",
    alertCrash: "💥 Fahrzeug zerstört! Zurück zum letzten Checkpoint!",
  }
};

let lang = 'ar';
let money = 0;
let currentVehicle = 'shas';
let currentLevel = 0;
let checkpoint = { x: 100, y: 340 };
let gameStarted = false;

const vehicleImages = {
  shas: "https://i.imgur.com/7p8XwLo.png",
  ford: "https://i.imgur.com/bRjoWcX.png",
  bike: "https://i.imgur.com/sKIh1I7.png"
};

const levelBackgrounds = [
  "https://i.imgur.com/0Q9Y6x1.png", // السعودية (صحراء)
  "https://i.imgur.com/3LKDa8L.jpg", // اليابان (مدينة ليلية)
  "https://i.imgur.com/GBaQcOW.jpg", // أمريكا (طرق سريعة)
  "https://i.imgur.com/EjWwqk7.jpg", // النرويج (ثلوج)
];

const jumpSound = new Audio("https://actions.google.com/sounds/v1/cartoon/wood_plank_flicks.ogg");
const crashSound = new Audio("https://actions.google.com/sounds/v1/cartoon/cartoon_boing.ogg");

const langSelectDiv = document.getElementById('langSelect');
const startScreen = document.getElementById('startScreen');
const gameUI = document.getElementById('gameUI');
const shopDiv = document.getElementById('shop');
const startGameBtn = document.getElementById('startGameBtn');
const moneyDisplay = document.getElementById('moneyDisplay');
const levelDisplay = document.getElementById('levelDisplay');
const openShopBtn = document.getElementById('openShopBtn');
const playerMoneyDisplay = document.getElementById('playerMoneyDisplay');
const closeShopBtn = document.getElementById('closeShopBtn');
const gameCanvas = document.getElementById('gameCanvas');
const ctx = gameCanvas.getContext('2d');
const vehicleButtons = document.querySelectorAll('#vehicleSelection button');
const shopBuyButtons = document.querySelectorAll('#shop button[data-buy]');

let obstacles = [];
let groundLevel = 340;

const vehiclesData = {
  shas: {price: 0, unlocked: true, width: 60, height: 40, jumpForce: -13, gravity: 0.8, speed: 4},
  ford: {price: 30, unlocked: false, width: 70, height: 40, jumpForce: -14, gravity: 0.8, speed: 5},
  bike: {price: 50, unlocked: false, width: 50, height: 40, jumpForce: -16, gravity: 0.6, speed: 6},
};

let player = {
  x: 100,
  y: groundLevel,
  dy: 0,
  isJumping: false,
  vehicle: currentVehicle,
  width: vehiclesData.shas.width,
  height: vehiclesData.shas.height,
  jumpForce: vehiclesData.shas.jumpForce,
  gravity: vehiclesData.shas.gravity,
  speed: vehiclesData.shas.speed,
};

function setLanguage(selectedLang) {
  lang = selectedLang;
  const t = texts[lang];
  document.getElementById('langSelectTitle').textContent = t.langSelectTitle;
  document.getElementById('gameTitle').textContent = t.gameTitle;
  document.getElementById('selectVehicleTitle').textContent = t.selectVehicleTitle;
  startGameBtn.textContent = t.startGame;
  openShopBtn.textContent = t.openShop;
  document.getElementById('shopTitle').textContent = t.shopTitle;
  closeShopBtn.textContent = t.closeShop;
  shopBuyButtons.forEach(btn => {
    if (btn.dataset.buy === "ford") btn.textContent = t.buyFord;
    if (btn.dataset.buy === "bike") btn.textContent = t.buyBike;
  });
  moneyDisplay.textContent = `${t.money}: ${money}`;
  levelDisplay.textContent = `${t.level}: ${currentLevel + 1}`;
  updatePlayerMoneyDisplay();
}

function updatePlayerMoneyDisplay() {
  playerMoneyDisplay.textContent = `${texts[lang].money}: ${money}`;
}

function saveProgress() {
  const data = {
    money,
    unlockedVehicles: Object.keys(vehiclesData).filter(v => vehiclesData[v].unlocked),
    currentVehicle,
    currentLevel,
    checkpoint,
  };
  localStorage.setItem('superShasSave', JSON.stringify(data));
}

function loadProgress() {
  const data = JSON.parse(localStorage.getItem('superShasSave'));
  if (data) {
    money = data.money || 0;
    currentLevel = data.currentLevel || 0;
    checkpoint = data.checkpoint || {x: 100, y: groundLevel};
    currentVehicle = data.currentVehicle || 'shas';
    Object.keys(vehiclesData).forEach(v => vehiclesData[v].unlocked = false);
    data.unlockedVehicles.forEach(v => {
      if(vehiclesData[v]) vehiclesData[v].unlocked = true;
    });
  }
}

function updatePlayerVehicle(vehicleKey) {
  if(!vehiclesData[vehicleKey].unlocked) return;
  currentVehicle = vehicleKey;
  player.vehicle = vehicleKey;
  player.width = vehiclesData[vehicleKey].width;
  player.height = vehiclesData[vehicleKey].height;
  player.jumpForce = vehiclesData[vehicleKey].jumpForce;
  player.gravity = vehiclesData[vehicleKey].gravity;
  player.speed = vehiclesData[vehicleKey].speed;
  updateVehicleButtonsUI();
}

function updateVehicleButtonsUI() {
  vehicleButtons.forEach(btn => {
    const v = btn.dataset.vehicle;
    if(vehiclesData[v].unlocked) {
      btn.disabled = false;
      btn.style.opacity = '1';
      btn.textContent = `${v === 'shas' ? 'شاص 🚙' : v === 'ford' ? 'فورد 🚘' : 'دراجة 🚲'}`;
    } else {
      btn.disabled = false;
      btn.style.opacity = '0.6';
      btn.textContent = `${v === 'shas' ? 'شاص 🚙' : v === 'ford' ? 'فورد 🚘 ('+vehiclesData[v].price+' ريال)' : 'دراجة 🚲 ('+vehiclesData[v].price+' ريال)'}`;
    }
    if (v === currentVehicle) btn.style.border = '3px solid #0f0';
    else btn.style.border = 'none';
  });
}

function showAlert(message) {
  alert(message);
}

function startGame() {
  gameStarted = true;
  langSelectDiv.style.display = 'none';
  startScreen.style.display = 'none';
  shopDiv.style.display = 'none';
  gameUI.style.display = 'flex';

  resetGame();
  requestAnimationFrame(gameLoop);
}

function resetGame() {
  player.x = checkpoint.x;
  player.y = checkpoint.y;
  player.dy = 0;
  player.isJumping = false;
  obstacles = generateObstaclesForLevel(currentLevel);
  moneyDisplay.textContent = `${texts[lang].money}: ${money}`;
  levelDisplay.textContent = `${texts[lang].level}: ${currentLevel + 1}`;
  updatePlayerVehicle(currentVehicle);
}

function generateObstaclesForLevel(level) {
  // Simplified example: obstacles at fixed positions depending on level
  const baseObstacles = [
    {x: 400, y: groundLevel + 10, width: 30, height: 40},
    {x: 600, y: groundLevel + 10, width: 30, height: 40}
  ];
  // Add more or change obstacles per level
  if(level === 1) {
    return [
      {x: 300, y: groundLevel + 10, width: 40, height: 50},
      {x: 500, y: groundLevel + 10, width: 25, height: 30},
      {x: 700, y: groundLevel + 10, width: 30, height: 40},
    ];
  } else if(level === 2) {
    return [
      {x: 350, y: groundLevel + 10, width: 35, height: 45},
      {x: 550, y: groundLevel + 10, width: 40, height: 50},
      {x: 750, y: groundLevel + 10, width: 30, height: 40},
    ];
  }
  return baseObstacles;
}

function drawBackground() {
  const bg = new Image();
  bg.src = levelBackgrounds[currentLevel % levelBackgrounds.length];
  bg.onload = () => {
    ctx.drawImage(bg, 0, 0, gameCanvas.width, gameCanvas.height);
  }
  ctx.fillStyle = "#654321";
  ctx.fillRect(0, groundLevel + 40, gameCanvas.width, 60);
}

function drawPlayer() {
  const img = new Image();
  img.src = vehicleImages[player.vehicle];
  ctx.drawImage(img, player.x, player.y - player.height, player.width, player.height);
}

function drawObstacles() {
  ctx.fillStyle = 'red';
  obstacles.forEach(obs => {
    ctx.fillRect(obs.x, obs.y - obs.height, obs.width, obs.height);
  });
}

function checkCollision() {
  for(let obs of obstacles) {
    if(
      player.x < obs.x + obs.width &&
      player.x + player.width > obs.x &&
      player.y > obs.y - obs.height &&
      player.y - player.height < obs.y
    ) {
      return true;
    }
  }
  return false;
}

function update() {
  // Gravity and jump
  if(player.isJumping) {
    player.dy += player.gravity;
    player.y += player.dy;

    if(player.y >= groundLevel) {
      player.y = groundLevel;
      player.dy = 0;
      player.isJumping = false;
    }
  }
  // Move obstacles left (simulate forward movement)
  obstacles.forEach(obs => {
    obs.x -= player.speed;
    if(obs.x + obs.width < 0) {
      obs.x = gameCanvas.width + Math.random() * 300;
    }
  });
  if(checkCollision()) {
    crashSound.play();
    alert(texts[lang].alertCrash);
    player.x = checkpoint.x;
    player.y = checkpoint.y;
    player.dy = 0;
    player.isJumping = false;
  }
}

function clearCanvas() {
  ctx.clearRect(0, 0, gameCanvas.width, gameCanvas.height);
}

function gameLoop() {
  if(!gameStarted) return;
  clearCanvas();
  drawBackground();
  drawObstacles();
  drawPlayer();
  update();
  requestAnimationFrame(gameLoop);
}

// Event Listeners

// Language selection buttons
document.querySelectorAll('#langSelect button').forEach(btn => {
  btn.addEventListener('click', () => {
    setLanguage(btn.dataset.lang);
    loadProgress();
    updateVehicleButtonsUI();
    updatePlayerMoneyDisplay();
    langSelectDiv.style.display = 'none';
    startScreen.style.display = 'flex';
  });
});

// Vehicle selection buttons
vehicleButtons.forEach(btn => {
  btn.addEventListener('click', () => {
    const v = btn.dataset.vehicle;
    if(vehiclesData[v].unlocked) {
      updatePlayerVehicle(v);
    } else {
      // If locked, try to buy if player has enough money
      if(money >= vehiclesData[v].price) {
        money -= vehiclesData[v].price;
        vehiclesData[v].unlocked = true;
        updatePlayerVehicle(v);
        updatePlayerMoneyDisplay();
        saveProgress();
        showAlert(texts[lang].alertBought);
        updateVehicleButtonsUI();
      } else {
        showAlert(texts[lang].alertNoMoney);
      }
    }
  });
});

// Start game button
startGameBtn.addEventListener('click', () => {
  startGame();
});

// Open Shop button
openShopBtn.addEventListener('click', () => {
  shopDiv.style.display = 'flex';
  gameUI.style.display = 'none';
});

// Close Shop button
closeShopBtn.addEventListener('click', () => {
  shopDiv.style.display = 'none';
  gameUI.style.display = 'flex';
});

// Shop buy buttons
shopBuyButtons.forEach(btn => {
  btn.addEventListener('click', () => {
    const item = btn.dataset.buy;
    if(vehiclesData[item].unlocked) {
      showAlert(texts[lang].alertBought);
      return;
    }
    if(money >= vehiclesData[item].price) {
      money -= vehiclesData[item].price;
      vehiclesData[item].unlocked = true;
      updatePlayerMoneyDisplay();
      updateVehicleButtonsUI();
      saveProgress();
      showAlert(texts[lang].alertBought);
    } else {
      showAlert(texts[lang].alertNoMoney);
    }
  });
});

// Keyboard Controls
window.addEventListener('keydown', e => {
  if(!gameStarted) return;
  if(e.code === 'Space' || e.code === 'ArrowUp' || e.key === ' ') {
    if(!player.isJumping) {
      player.isJumping = true;
      player.dy = player.jumpForce;
      jumpSound.play();
    }
  }
});

setLanguage('ar');
</script>
</body>
</html>


