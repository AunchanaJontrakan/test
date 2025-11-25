<html lang="en">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tarot Card Reader</title>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Georgia', serif;
      width: 100%;
      height: 100%;
    }

    html {
      height: 100%;
    }

    .app-container {
      min-height: 100%;
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 40px 20px;
    }

    .header {
      text-align: center;
      margin-bottom: 40px;
    }

    .title {
      font-size: 48px;
      margin: 0 0 10px 0;
      font-weight: 600;
      letter-spacing: 2px;
    }

    .subtitle {
      font-size: 20px;
      margin: 0;
      opacity: 0.8;
    }

    .card-spread {
      display: flex;
      gap: 30px;
      margin-bottom: 40px;
      flex-wrap: wrap;
      justify-content: center;
      min-height: 320px;
      align-items: center;
    }

    .card {
      width: 180px;
      height: 280px;
      border-radius: 12px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      padding: 20px;
      text-align: center;
      position: relative;
      overflow: hidden;
    }

    .card:hover {
      transform: translateY(-5px);
      box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
    }

    .card.face-down {
      cursor: pointer;
    }

    .card.face-up {
      cursor: default;
    }

    .card-back {
      font-size: 80px;
      opacity: 0.7;
    }

    .card-content {
      display: none;
    }

    .card.face-up .card-back {
      display: none;
    }

    .card.face-up .card-content {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 15px;
    }

    .card-icon {
      font-size: 64px;
    }

    .card-name {
      font-size: 18px;
      font-weight: 600;
      line-height: 1.3;
    }

    .card-meaning {
      font-size: 14px;
      opacity: 0.85;
      line-height: 1.4;
    }

    .controls {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
      justify-content: center;
    }

    .btn {
      padding: 16px 36px;
      font-size: 18px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-family: 'Georgia', serif;
      transition: all 0.3s ease;
      font-weight: 500;
      letter-spacing: 0.5px;
    }

    .btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
    }

    .btn:active {
      transform: translateY(0);
    }

    .btn:disabled {
      opacity: 0.5;
      cursor: not-allowed;
      transform: none;
    }

    .instruction {
      text-align: center;
      font-size: 18px;
      margin-bottom: 30px;
      opacity: 0.7;
      font-style: italic;
    }

    @media (max-width: 768px) {
      .title {
        font-size: 36px;
      }

      .subtitle {
        font-size: 16px;
      }

      .card {
        width: 150px;
        height: 240px;
      }

      .card-icon {
        font-size: 48px;
      }

      .card-name {
        font-size: 16px;
      }

      .card-meaning {
        font-size: 12px;
      }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="app-container">
   <div class="header">
    <h1 class="title" id="siteTitle">Mystic Tarot</h1>
    <p class="subtitle" id="siteSubtitle">Discover Your Fortune</p>
   </div>
   <p class="instruction" id="instruction">Click "Draw Cards" to reveal your three-card reading</p>
   <div class="card-spread" id="cardSpread"></div>
   <div class="controls"><button class="btn" id="drawBtn">Draw Cards</button> <button class="btn" id="resetBtn" style="display: none;">Read Again</button>
   </div>
  </div>
  <script>
    const tarotCards = [
      { name: "The Fool", icon: "🌟", meaning: "New beginnings, innocence" },
      { name: "The Magician", icon: "✨", meaning: "Manifestation, power" },
      { name: "The High Priestess", icon: "🌙", meaning: "Intuition, mystery" },
      { name: "The Empress", icon: "👑", meaning: "Abundance, nurturing" },
      { name: "The Emperor", icon: "⚔️", meaning: "Authority, structure" },
      { name: "The Hierophant", icon: "📿", meaning: "Tradition, wisdom" },
      { name: "The Lovers", icon: "💕", meaning: "Love, harmony" },
      { name: "The Chariot", icon: "🏆", meaning: "Victory, determination" },
      { name: "Strength", icon: "🦁", meaning: "Courage, patience" },
      { name: "The Hermit", icon: "🔮", meaning: "Introspection, guidance" },
      { name: "Wheel of Fortune", icon: "🎡", meaning: "Change, destiny" },
      { name: "Justice", icon: "⚖️", meaning: "Fairness, truth" },
      { name: "The Hanged Man", icon: "🙃", meaning: "Sacrifice, perspective" },
      { name: "Death", icon: "🦋", meaning: "Transformation, endings" },
      { name: "Temperance", icon: "🕊️", meaning: "Balance, moderation" },
      { name: "The Devil", icon: "😈", meaning: "Bondage, materialism" },
      { name: "The Tower", icon: "⚡", meaning: "Upheaval, revelation" },
      { name: "The Star", icon: "⭐", meaning: "Hope, inspiration" },
      { name: "The Moon", icon: "🌕", meaning: "Illusion, intuition" },
      { name: "The Sun", icon: "☀️", meaning: "Joy, success" },
      { name: "Judgement", icon: "📯", meaning: "Rebirth, evaluation" },
      { name: "The World", icon: "🌍", meaning: "Completion, achievement" }
    ];

    const defaultConfig = {
      background_color: "#1a0033",
      card_color: "#2d1b4e",
      text_color: "#f0e6ff",
      button_color: "#7c3aed",
      button_text_color: "#ffffff",
      site_title: "Mystic Tarot",
      site_subtitle: "Discover Your Fortune",
      draw_button_text: "Draw Cards",
      reset_button_text: "Read Again",
      font_family: "Georgia",
      font_size: 16
    };

    let selectedCards = [];

    function shuffleArray(array) {
      const newArray = [...array];
      for (let i = newArray.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
      }
      return newArray;
    }

    function createCardElement(card, index) {
      const cardEl = document.createElement('div');
      cardEl.className = 'card face-down';
      cardEl.dataset.index = index;
      
      cardEl.innerHTML = `
        <div class="card-back">🔮</div>
        <div class="card-content">
          <div class="card-icon">${card.icon}</div>
          <div class="card-name">${card.name}</div>
          <div class="card-meaning">${card.meaning}</div>
        </div>
      `;
      
      cardEl.addEventListener('click', () => {
        if (cardEl.classList.contains('face-down')) {
          cardEl.classList.remove('face-down');
          cardEl.classList.add('face-up');
        }
      });
      
      return cardEl;
    }

    function drawCards() {
      const shuffled = shuffleArray(tarotCards);
      selectedCards = shuffled.slice(0, 3);
      
      const cardSpread = document.getElementById('cardSpread');
      cardSpread.innerHTML = '';
      
      selectedCards.forEach((card, index) => {
        const cardEl = createCardElement(card, index);
        cardSpread.appendChild(cardEl);
      });
      
      document.getElementById('drawBtn').style.display = 'none';
      document.getElementById('resetBtn').style.display = 'block';
      document.getElementById('instruction').textContent = 'Click each card to reveal its meaning';
    }

    function resetReading() {
      selectedCards = [];
      document.getElementById('cardSpread').innerHTML = '';
      document.getElementById('drawBtn').style.display = 'block';
      document.getElementById('resetBtn').style.display = 'none';
      document.getElementById('instruction').textContent = 'Click "Draw Cards" to reveal your three-card reading';
    }

    async function onConfigChange(config) {
      const baseSize = config.font_size || defaultConfig.font_size;
      const customFont = config.font_family || defaultConfig.font_family;
      const baseFontStack = 'Georgia, serif';
      const fontStack = `${customFont}, ${baseFontStack}`;

      document.body.style.fontFamily = fontStack;
      document.querySelector('.app-container').style.background = config.background_color || defaultConfig.background_color;
      document.querySelector('.app-container').style.color = config.text_color || defaultConfig.text_color;

      document.getElementById('siteTitle').textContent = config.site_title || defaultConfig.site_title;
      document.getElementById('siteTitle').style.fontSize = `${baseSize * 3}px`;
      document.getElementById('siteTitle').style.color = config.text_color || defaultConfig.text_color;

      document.getElementById('siteSubtitle').textContent = config.site_subtitle || defaultConfig.site_subtitle;
      document.getElementById('siteSubtitle').style.fontSize = `${baseSize * 1.25}px`;
      document.getElementById('siteSubtitle').style.color = config.text_color || defaultConfig.text_color;

      document.getElementById('instruction').style.fontSize = `${baseSize * 1.125}px`;
      document.getElementById('instruction').style.color = config.text_color || defaultConfig.text_color;

      document.getElementById('drawBtn').textContent = config.draw_button_text || defaultConfig.draw_button_text;
      document.getElementById('drawBtn').style.background = config.button_color || defaultConfig.button_color;
      document.getElementById('drawBtn').style.color = config.button_text_color || defaultConfig.button_text_color;
      document.getElementById('drawBtn').style.fontSize = `${baseSize * 1.125}px`;

      document.getElementById('resetBtn').textContent = config.reset_button_text || defaultConfig.reset_button_text;
      document.getElementById('resetBtn').style.background = config.button_color || defaultConfig.button_color;
      document.getElementById('resetBtn').style.color = config.button_text_color || defaultConfig.button_text_color;
      document.getElementById('resetBtn').style.fontSize = `${baseSize * 1.125}px`;

      const cards = document.querySelectorAll('.card');
      cards.forEach(card => {
        card.style.background = config.card_color || defaultConfig.card_color;
        card.style.color = config.text_color || defaultConfig.text_color;
        card.style.border = `2px solid ${config.button_color || defaultConfig.button_color}`;
      });

      const cardNames = document.querySelectorAll('.card-name');
      cardNames.forEach(name => {
        name.style.fontSize = `${baseSize * 1.125}px`;
      });

      const cardMeanings = document.querySelectorAll('.card-meaning');
      cardMeanings.forEach(meaning => {
        meaning.style.fontSize = `${baseSize * 0.875}px`;
      });
    }

    function mapToCapabilities(config) {
      return {
        recolorables: [
          {
            get: () => config.background_color || defaultConfig.background_color,
            set: (value) => {
              config.background_color = value;
              if (window.elementSdk) {
                window.elementSdk.setConfig({ background_color: value });
              }
            }
          },
          {
            get: () => config.card_color || defaultConfig.card_color,
            set: (value) => {
              config.card_color = value;
              if (window.elementSdk) {
                window.elementSdk.setConfig({ card_color: value });
              }
            }
          },
          {
            get: () => config.text_color || defaultConfig.text_color,
            set: (value) => {
              config.text_color = value;
              if (window.elementSdk) {
                window.elementSdk.setConfig({ text_color: value });
              }
            }
          },
          {
            get: () => config.button_color || defaultConfig.button_color,
            set: (value) => {
              config.button_color = value;
              if (window.elementSdk) {
                window.elementSdk.setConfig({ button_color: value });
              }
            }
          },
          {
            get: () => config.button_text_color || defaultConfig.button_text_color,
            set: (value) => {
              config.button_text_color = value;
              if (window.elementSdk) {
                window.elementSdk.setConfig({ button_text_color: value });
              }
            }
          }
        ],
        borderables: [],
        fontEditable: {
          get: () => config.font_family || defaultConfig.font_family,
          set: (value) => {
            config.font_family = value;
            if (window.elementSdk) {
              window.elementSdk.setConfig({ font_family: value });
            }
          }
        },
        fontSizeable: {
          get: () => config.font_size || defaultConfig.font_size,
          set: (value) => {
            config.font_size = value;
            if (window.elementSdk) {
              window.elementSdk.setConfig({ font_size: value });
            }
          }
        }
      };
    }

    function mapToEditPanelValues(config) {
      return new Map([
        ['site_title', config.site_title || defaultConfig.site_title],
        ['site_subtitle', config.site_subtitle || defaultConfig.site_subtitle],
        ['draw_button_text', config.draw_button_text || defaultConfig.draw_button_text],
        ['reset_button_text', config.reset_button_text || defaultConfig.reset_button_text]
      ]);
    }

    document.getElementById('drawBtn').addEventListener('click', drawCards);
    document.getElementById('resetBtn').addEventListener('click', resetReading);

    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig,
        onConfigChange,
        mapToCapabilities,
        mapToEditPanelValues
      });
    } else {
      onConfigChange(defaultConfig);
    }
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a41f7efa2ca1bd5',t:'MTc2NDA4MjMzMi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
