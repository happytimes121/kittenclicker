<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Kitten Clicker</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background: #fce4ec;
    margin: 0;
    padding-top: 80px;
    text-align: center;
  }
  h1 {
    color: #ad1457;
    margin-bottom: 0;
  }
  #score {
    font-size: 24px;
    margin: 5px 0 10px 0;
  }
  #stats {
    margin-bottom: 30px;
  }
  #kitten {
    width: 300px;
    cursor: pointer;
    border-radius: 20px;
    transition: transform 0.2s ease;
  }
  .pulse {
    animation: pulseAnim 0.3s ease forwards;
  }
  @keyframes pulseAnim {
    0% { transform: scale(1); }
    50% { transform: scale(1.2); }
    100% { transform: scale(1); }
  }
  .top-buttons {
    position: fixed;
    top: 20px;
    right: 20px;
    display: flex;
    flex-direction: column;
    gap: 10px;
    z-index: 1001;
  }
  .top-buttons button {
    padding: 10px 20px;
    background-color: #880e4f;
    color: white;
    font-size: 16px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
  }
  .top-buttons button:hover {
    background-color: #ad1457;
  }
  .overlay {
    display: none;
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: rgba(255,255,255,0.95);
    overflow-y: auto;
    z-index: 1000;
    padding: 60px 20px 20px;
  }
  .overlay.active {
    display: block;
  }
  .overlay h2 {
    margin-bottom: 30px;
    color: #880e4f;
  }
  .shop-item {
    background: #f8bbd0;
    margin: 15px auto;
    padding: 15px;
    width: 60%;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  }
  .shop-item button {
    padding: 10px 20px;
    border: none;
    border-radius: 6px;
    background-color: #c2185b;
    color: white;
    cursor: pointer;
  }
  .shop-item button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  .closeOverlay {
    position: absolute;
    top: 20px; left: 20px;
    background: red;
    color: white;
    border: none;
    padding: 5px 15px;
    font-size: 20px;
    border-radius: 8px;
    cursor: pointer;
  }
  #progressOverlay textarea {
    width: 90%;
    height: 150px;
    margin-top: 10px;
    font-family: monospace;
    font-size: 14px;
    resize: none;
  }
  #progressOverlay input {
    width: 90%;
    padding: 8px;
    font-size: 16px;
    margin-top: 10px;
  }
  #progressOverlay button {
    margin-top: 10px;
    padding: 8px 16px;
    border-radius: 6px;
    border: none;
    background-color: #c2185b;
    color: white;
    cursor: pointer;
    font-size: 16px;
  }
</style>
</head>
<body>

<h1>Kitten Clicker</h1>
<p id="score">Score: 0</p>
<p id="stats">Click Value: <span id="clickValue">1</span> | CPS: <span id="autoClickers">0</span></p>

<img id="kitten" src="https://www.zooplus.co.uk/magazine/wp-content/uploads/2021/01/striped-grey-kitten.jpg" alt="Cute Kitten" />

<div class="top-buttons">
  <button onclick="openOverlay('shopOverlay')">Shop</button>
  <button onclick="openOverlay('progressOverlay')">Progress</button>
  <button onclick="openOverlay('settingsOverlay')">Settings</button>
  <button onclick="openOverlay('achievementsOverlay')">Achievements</button>
</div>

<!-- Shop Overlay -->
<div id="shopOverlay" class="overlay">
  <button class="closeOverlay" onclick="closeOverlay('shopOverlay')">X</button>
  <h2>Kitten Shop</h2>
  <div class="shop-item" id="milkBowlItem">
    <strong>Milk Bowl</strong><br />
    +0.5 click value<br />
    <button id="milkBowlBuy" onclick="buyUpgrade('milkBowl', 0.5)">Buy for <span id="milkBowlCost">25</span></button>
  </div>
  <div class="shop-item" id="catFoodItem">
    <strong>Cat Food</strong><br />
    +2 click value<br />
    <button id="catFoodBuy" onclick="buyUpgrade('catFood', 2)">Buy for <span id="catFoodCost">115</span></button>
  </div>
  <div class="shop-item" id="scratcherItem">
    <strong>Scratcher</strong><br />
    +7 click value<br />
    <button id="scratcherBuy" onclick="buyUpgrade('scratcher', 7)">Buy for <span id="scratcherCost">700</span></button>
  </div>
  <div class="shop-item" id="autoClickerItem">
    <strong>Auto-Mouse</strong><br />
    +5 CPS<br />
    <button id="autoClickerBuy" onclick="buyAutoClicker()">Buy for <span id="autoClickerCost">2000</span></button>
  </div>
  <div class="shop-item" id="offlineItem">
    <strong>Offline Clicker</strong><br />
    Earn while offline (max 70%)<br />
    <button id="offlineBuy" onclick="buyOffline()">Buy for <span id="offlineCost">10000</span></button>
  </div>
</div>

<!-- Progress Overlay -->
<div id="progressOverlay" class="overlay">
  <button class="closeOverlay" onclick="closeOverlay('progressOverlay')">X</button>
  <h2>Save / Load Progress</h2>
  <button onclick="exportSave()">Get Save Code</button><br />
  <textarea id="saveCodeBox" readonly placeholder="Your save code will appear here after clicking 'Get Save Code'"></textarea><br />
  <input id="importCode" placeholder="Paste save code here" /><br />
  <button onclick="importSave()">Load Save</button><br /><br />
  <button onclick="resetGame()">Reset Game</button>
</div>

<!-- Settings Overlay -->
<div id="settingsOverlay" class="overlay">
  <button class="closeOverlay" onclick="closeOverlay('settingsOverlay')">X</button>
  <h2>Settings</h2>
  <p>Coming soon!</p>
</div>

<!-- Achievements Overlay -->
<div id="achievementsOverlay" class="overlay">
  <button class="closeOverlay" onclick="closeOverlay('achievementsOverlay')">X</button>
  <h2>Achievements</h2>
  <p>Coming soon!</p>
</div>

<script>
  // Game state variables
  let score = 0;
  let clickValue = 1;
  let autoClickers = 0;
  let autoClickerCost = 2000;
  let offlineLevel = 0; // max 70%
  let offlineCost = 10000;

  const upgradeCosts = {
    milkBowl: 25,
    catFood: 115,
    scratcher: 700
  };

  // Number of upgrades bought (for cost scaling)
  let upgradesBought = {
    milkBowl: 0,
    catFood: 0,
    scratcher: 0
  };

  const kitten = document.getElementById('kitten');
  const scoreDisplay = document.getElementById('score');
  const clickValDisplay = document.getElementById('clickValue');
  const autoClickerDisplay = document.getElementById('autoClickers');
  const autoClickerCostSpan = document.getElementById('autoClickerCost');
  const offlineCostSpan = document.getElementById('offlineCost');
  const milkBowlCostSpan = document.getElementById('milkBowlCost');
  const catFoodCostSpan = document.getElementById('catFoodCost');
  const scratcherCostSpan = document.getElementById('scratcherCost');
  const saveCodeBox = document.getElementById('saveCodeBox');
  const importCodeInput = document.getElementById('importCode');

  const milkBowlBuyBtn = document.getElementById('milkBowlBuy');
  const catFoodBuyBtn = document.getElementById('catFoodBuy');
  const scratcherBuyBtn = document.getElementById('scratcherBuy');
  const autoClickerBuyBtn = document.getElementById('autoClickerBuy');
  const offlineBuyBtn = document.getElementById('offlineBuy');

  const localStorageKey = 'kittenClickerSave';

  // Utility: cost scaling for upgrades
  function getUpgradeCost(baseCost, boughtCount) {
    // Increase cost by 15% per upgrade bought, rounded
    return Math.floor(baseCost * Math.pow(1.15, boughtCount));
  }

  // Update UI text & button states
  function updateUI() {
    scoreDisplay.textContent = 'Score: ' + Math.floor(score);
    clickValDisplay.textContent = clickValue.toFixed(2);
    autoClickerDisplay.textContent = autoClickers;

    // Update upgrade costs & button enable/disable based on score
    milkBowlCostSpan.textContent = getUpgradeCost(upgradeCosts.milkBowl, upgradesBought.milkBowl);
    catFoodCostSpan.textContent = getUpgradeCost(upgradeCosts.catFood, upgradesBought.catFood);
    scratcherCostSpan.textContent = getUpgradeCost(upgradeCosts.scratcher, upgradesBought.scratcher);
    autoClickerCostSpan.textContent = autoClickerCost;
    offlineCostSpan.textContent = offlineCost;

    milkBowlBuyBtn.disabled = score < getUpgradeCost(upgradeCosts.milkBowl, upgradesBought.milkBowl);
    catFoodBuyBtn.disabled = score < getUpgradeCost(upgradeCosts.catFood, upgradesBought.catFood);
    scratcherBuyBtn.disabled = score < getUpgradeCost(upgradeCosts.scratcher, upgradesBought.scratcher);
    autoClickerBuyBtn.disabled = score < autoClickerCost;
    offlineBuyBtn.disabled = score < offlineCost || offlineLevel >= 7;
  }

  // Open and close overlays
  function openOverlay(id) {
    document.querySelectorAll('.overlay').forEach(ov => ov.classList.remove('active'));
    document.getElementById(id).classList.add('active');
  }
  function closeOverlay(id) {
    document.getElementById(id).classList.remove('active');
  }

  // Click on kitten: add points
  kitten.onclick = () => {
    score += clickValue;
    kitten.classList.add('pulse');
    setTimeout(() => kitten.classList.remove('pulse'), 300);
    updateUI();
  };

  // Buy upgrades
  function buyUpgrade(name, valueIncrease) {
    const cost = getUpgradeCost(upgradeCosts[name], upgradesBought[name]);
    if (score >= cost) {
      score -= cost;
      clickValue += valueIncrease;
      upgradesBought[name]++;
      updateUI();
    }
  }

  // Buy auto clicker
  function buyAutoClicker() {
    if (score >= autoClickerCost) {
      score -= autoClickerCost;
      autoClickers++;
      autoClickerCost = Math.floor(autoClickerCost * 1.25);
      updateUI();
    }
  }

  // Buy offline level (max 7)
  function buyOffline() {
    if (score >= offlineCost && offlineLevel < 7) {
      score -= offlineCost;
      offlineLevel++;
      offlineCost = Math.floor(offlineCost * 1.4);
      updateUI();
    }
  }

  // Auto-clicker interval
  setInterval(() => {
    if (autoClickers > 0) {
      score += autoClickers * 5;
      updateUI();
    }
  }, 1000);

  // Save game state to localStorage
  function saveGame() {
    const saveData = {
      score,
      clickValue,
      autoClickers,
      autoClickerCost,
      offlineLevel,
      offlineCost,
      upgradesBought,
      lastSaveTime: Date.now()
    };
    localStorage.setItem(localStorageKey, JSON.stringify(saveData));
  }

  // Load game state from localStorage
  function loadGame() {
    const saved = localStorage.getItem(localStorageKey);
    if (!saved) return false;
    try {
      const data = JSON.parse(saved);
      // Apply saved values, with fallback defaults
      score = data.score || 0;
      clickValue = data.clickValue || 1;
      autoClickers = data.autoClickers || 0;
      autoClickerCost = data.autoClickerCost || 2000;
      offlineLevel = data.offlineLevel || 0;
      offlineCost = data.offlineCost || 10000;
      upgradesBought = data.upgradesBought || {milkBowl:0, catFood:0, scratcher:0};
      // Calculate offline earnings:
      if (data.lastSaveTime) {
        const now = Date.now();
        let diffSeconds = Math.floor((now - data.lastSaveTime) / 1000);
        if (diffSeconds > 0) {
          const cps = autoClickers * 5;
          // max offline earnings = 70% * cps * diffSeconds * offlineLevel/7
          const maxOfflineRatio = 0.7 * (offlineLevel / 7);
          let offlineEarn = cps * diffSeconds * maxOfflineRatio;
          if (offlineEarn > 0) {
            alert(`Welcome back! You earned ${Math.floor(offlineEarn)} points while offline.`);
            score += offlineEarn;
          }
        }
      }
      updateUI();
      return true;
    } catch(e) {
      console.error("Load error:", e);
      return false;
    }
  }

  // Export save data as base64 string for copy/paste
  function exportSave() {
    const saveData = localStorage.getItem(localStorageKey);
    if (!saveData) {
      alert('No save data found.');
      return;
    }
    const b64 = btoa(saveData);
    saveCodeBox.value = b64;
  }

  // Import save data from base64 string
  function importSave() {
    const importStr = importCodeInput.value.trim();
    if (!importStr) {
      alert('Please paste a save code to import.');
      return;
    }
    try {
      const jsonStr = atob(importStr);
      JSON.parse(jsonStr); // test validity
      localStorage.setItem(localStorageKey, jsonStr);
      alert('Save imported successfully! Reloading...');
      location.reload();
    } catch(e) {
      alert('Invalid save code.');
    }
  }

  // Reset game completely
  function resetGame() {
    if (confirm('Are you sure you want to reset your game? This cannot be undone.')) {
      localStorage.removeItem(localStorageKey);
      location.reload();
    }
  }

  // Save game every second
  setInterval(saveGame, 1000);

  // Save on unload (close or reload)
  window.addEventListener('beforeunload', saveGame);

  // Initial load
  if (!loadGame()) {
    updateUI();
  }
</script>

</body>
</html>
