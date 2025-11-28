<!doctype html>
<html lang="th">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>วันนี้กินอะไรดี</title>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Kanit', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      width: 100%;
      height: 100%;
      overflow-x: hidden;
    }

    html {
      height: 100%;
    }

    .app-container {
      width: 100%;
      min-height: 100%;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 2rem;
    }

    .content-wrapper {
      background: white;
      border-radius: 24px;
      padding: 3rem 2.5rem;
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
      max-width: 500px;
      width: 100%;
      text-align: center;
    }

    h1 {
      color: #667eea;
      font-size: 2.5rem;
      margin: 0 0 0.5rem 0;
      font-weight: 700;
    }

    .subtitle {
      color: #6b7280;
      font-size: 1.1rem;
      margin: 0 0 2.5rem 0;
      font-weight: 400;
    }

    .menu-card {
      background: #f9fafb;
      border-radius: 16px;
      padding: 2rem;
      margin: 2rem 0;
      min-height: 200px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      border: 2px solid #e5e7eb;
      transition: all 0.3s ease;
    }

    .menu-card.active {
      border-color: #667eea;
      background: #f0f4ff;
    }

    .food-emoji {
      font-size: 5rem;
      margin-bottom: 1rem;
    }

    .food-name {
      font-size: 1.8rem;
      font-weight: 600;
      color: #1f2937;
      margin-bottom: 1.5rem;
    }

    .nutrition-info {
      display: flex;
      gap: 2rem;
      justify-content: center;
      flex-wrap: wrap;
    }

    .nutrition-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 0.25rem;
    }

    .nutrition-label {
      font-size: 0.85rem;
      color: #6b7280;
      font-weight: 500;
    }

    .nutrition-value {
      font-size: 1.4rem;
      font-weight: 700;
      color: #667eea;
    }

    .placeholder-text {
      color: #9ca3af;
      font-size: 1.2rem;
      font-weight: 500;
    }

    .random-button {
      background: #667eea;
      color: white;
      border: none;
      padding: 1rem 3rem;
      font-size: 1.2rem;
      font-weight: 600;
      border-radius: 12px;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 4px 14px rgba(102, 126, 234, 0.4);
      font-family: inherit;
    }

    .random-button:hover {
      background: #5568d3;
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(102, 126, 234, 0.5);
    }

    .random-button:active {
      transform: translateY(0);
    }

    .random-button:disabled {
      background: #9ca3af;
      cursor: not-allowed;
      box-shadow: none;
    }

    @media (max-width: 640px) {
      .content-wrapper {
        padding: 2rem 1.5rem;
      }

      h1 {
        font-size: 2rem;
      }

      .subtitle {
        font-size: 1rem;
      }

      .food-emoji {
        font-size: 4rem;
      }

      .food-name {
        font-size: 1.5rem;
      }

      .nutrition-value {
        font-size: 1.2rem;
      }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="app-container">
   <div class="content-wrapper">
    <h1 id="main-title">วันนี้กินอะไรดี</h1>
    <p class="subtitle" id="subtitle">สุ่มเมนูอาหารแสนอร่อย</p>
    <div class="menu-card" id="menu-card">
     <p class="placeholder-text">กดปุ่มด้านล่างเพื่อสุ่มเมนู! 🎲</p>
    </div><button class="random-button" id="random-button" onclick="randomizeFood()"> <span id="button-text">สุ่มเมนู</span> </button>
   </div>
  </div>
  <script>
    const defaultConfig = {
      main_title: "วันนี้กินอะไรดี",
      subtitle: "สุ่มเมนูอาหารแสนอร่อย",
      random_button_text: "สุ่มเมนู",
      primary_color: "#667eea",
      secondary_color: "#764ba2",
      background_color: "#ffffff",
      text_color: "#1f2937",
      button_color: "#667eea",
      font_family: "Kanit",
      font_size: 16
    };

    const foodMenu = [
      { name: "ข้าวผัด", emoji: "🍚", kcal: 450, sugar: 5 },
      { name: "ก๋วยเตี๋ยว", emoji: "🍜", kcal: 380, sugar: 8 },
      { name: "ส้มตำ", emoji: "🥗", kcal: 150, sugar: 15 },
      { name: "ผัดไทย", emoji: "🍝", kcal: 520, sugar: 12 },
      { name: "ข้าวมันไก่", emoji: "🍗", kcal: 650, sugar: 3 },
      { name: "ต้มยำกุ้ง", emoji: "🍲", kcal: 200, sugar: 6 },
      { name: "แกงเขียวหวาน", emoji: "🍛", kcal: 420, sugar: 8 },
      { name: "ข้าวขาหมู", emoji: "🍖", kcal: 750, sugar: 10 },
      { name: "ข้าวกะเพรา", emoji: "🍽️", kcal: 580, sugar: 7 },
      { name: "ข้าวซอย", emoji: "🥘", kcal: 680, sugar: 9 },
      { name: "ผัดกะเพรา", emoji: "🌶️", kcal: 490, sugar: 6 },
      { name: "ข้าวหมูแดง", emoji: "🥩", kcal: 620, sugar: 11 },
      { name: "ยำวุ้นเส้น", emoji: "🥢", kcal: 180, sugar: 13 },
      { name: "ข้าวหน้าเป็ด", emoji: "🦆", kcal: 710, sugar: 14 },
      { name: "ราดหน้า", emoji: "🍱", kcal: 550, sugar: 8 }
    ];

    function randomizeFood() {
      const menuCard = document.getElementById('menu-card');
      const randomButton = document.getElementById('random-button');
      
      randomButton.disabled = true;
      menuCard.innerHTML = '<p class="placeholder-text">กำลังสุ่ม... 🎲</p>';
      
      setTimeout(() => {
        const randomIndex = Math.floor(Math.random() * foodMenu.length);
        const food = foodMenu[randomIndex];
        
        menuCard.classList.add('active');
        menuCard.innerHTML = `
          <div class="food-emoji">${food.emoji}</div>
          <div class="food-name">${food.name}</div>
          <div class="nutrition-info">
            <div class="nutrition-item">
              <span class="nutrition-label">แคลอรี</span>
              <span class="nutrition-value">${food.kcal} kcal</span>
            </div>
            <div class="nutrition-item">
              <span class="nutrition-label">น้ำตาล</span>
              <span class="nutrition-value">${food.sugar} g</span>
            </div>
          </div>
        `;
        
        randomButton.disabled = false;
      }, 800);
    }

    async function onConfigChange(config) {
      const customFont = config.font_family || defaultConfig.font_family;
      const baseFontSize = config.font_size || defaultConfig.font_size;
      const baseFontStack = "'Segoe UI', Tahoma, Geneva, Verdana, sans-serif";

      document.body.style.fontFamily = `'${customFont}', ${baseFontStack}`;
      
      const appContainer = document.querySelector('.app-container');
      const primaryColor = config.primary_color || defaultConfig.primary_color;
      const secondaryColor = config.secondary_color || defaultConfig.secondary_color;
      appContainer.style.background = `linear-gradient(135deg, ${primaryColor} 0%, ${secondaryColor} 100%)`;

      const contentWrapper = document.querySelector('.content-wrapper');
      const backgroundColor = config.background_color || defaultConfig.background_color;
      contentWrapper.style.backgroundColor = backgroundColor;

      const mainTitle = document.getElementById('main-title');
      mainTitle.textContent = config.main_title || defaultConfig.main_title;
      mainTitle.style.color = primaryColor;
      mainTitle.style.fontSize = `${baseFontSize * 2.5}px`;

      const subtitle = document.getElementById('subtitle');
      subtitle.textContent = config.subtitle || defaultConfig.subtitle;
      subtitle.style.fontSize = `${baseFontSize * 1.1}px`;

      const buttonText = document.getElementById('button-text');
      buttonText.textContent = config.random_button_text || defaultConfig.random_button_text;

      const randomButton = document.getElementById('random-button');
      const buttonColor = config.button_color || defaultConfig.button_color;
      randomButton.style.backgroundColor = buttonColor;
      randomButton.style.fontSize = `${baseFontSize * 1.2}px`;

      const textColor = config.text_color || defaultConfig.text_color;
      document.querySelectorAll('.food-name').forEach(el => {
        el.style.color = textColor;
      });

      document.querySelectorAll('.nutrition-value').forEach(el => {
        el.style.color = primaryColor;
      });
    }

    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig,
        onConfigChange,
        mapToCapabilities: (config) => ({
          recolorables: [
            {
              get: () => config.primary_color || defaultConfig.primary_color,
              set: (value) => {
                config.primary_color = value;
                window.elementSdk.setConfig({ primary_color: value });
              }
            },
            {
              get: () => config.secondary_color || defaultConfig.secondary_color,
              set: (value) => {
                config.secondary_color = value;
                window.elementSdk.setConfig({ secondary_color: value });
              }
            },
            {
              get: () => config.background_color || defaultConfig.background_color,
              set: (value) => {
                config.background_color = value;
                window.elementSdk.setConfig({ background_color: value });
              }
            },
            {
              get: () => config.text_color || defaultConfig.text_color,
              set: (value) => {
                config.text_color = value;
                window.elementSdk.setConfig({ text_color: value });
              }
            },
            {
              get: () => config.button_color || defaultConfig.button_color,
              set: (value) => {
                config.button_color = value;
                window.elementSdk.setConfig({ button_color: value });
              }
            }
          ],
          borderables: [],
          fontEditable: {
            get: () => config.font_family || defaultConfig.font_family,
            set: (value) => {
              config.font_family = value;
              window.elementSdk.setConfig({ font_family: value });
            }
          },
          fontSizeable: {
            get: () => config.font_size || defaultConfig.font_size,
            set: (value) => {
              config.font_size = value;
              window.elementSdk.setConfig({ font_size: value });
            }
          }
        }),
        mapToEditPanelValues: (config) => new Map([
          ["main_title", config.main_title || defaultConfig.main_title],
          ["subtitle", config.subtitle || defaultConfig.subtitle],
          ["random_button_text", config.random_button_text || defaultConfig.random_button_text]
        ])
      });
    }
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a5650d062510886',t:'MTc2NDI5NTY4OC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
