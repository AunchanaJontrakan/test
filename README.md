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
      font-family: 'Kanit', 'Sarabun', 'Segoe UI', Tahoma, sans-serif;
      width: 100%;
      height: 100%;
    }

    html {
      height: 100%;
    }

    .app-wrapper {
      width: 100%;
      min-height: 100%;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      padding: 2.5rem 1rem;
    }

    .header {
      text-align: center;
      color: white;
      margin-bottom: 2.5rem;
    }

    .main-title {
      font-size: 3rem;
      margin: 0 0 0.5rem 0;
      font-weight: 700;
      text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.3);
    }

    .subtitle {
      font-size: 1.25rem;
      margin: 0;
      opacity: 0.95;
    }

    .categories-grid {
      max-width: 1200px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
      gap: 2rem;
      padding: 0 1rem;
    }

    .category-card {
      background: white;
      border-radius: 24px;
      padding: 2rem;
      box-shadow: 0 10px 40px rgba(0, 0, 0, 0.25);
      display: flex;
      flex-direction: column;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }

    .category-card:hover {
      transform: translateY(-8px);
      box-shadow: 0 15px 50px rgba(0, 0, 0, 0.3);
    }

    .category-header {
      text-align: center;
      margin-bottom: 1.5rem;
    }

    .category-icon {
      font-size: 3rem;
      margin-bottom: 0.5rem;
    }

    .category-title {
      font-size: 1.75rem;
      font-weight: 700;
      color: #667eea;
      margin: 0;
    }

    .result-display {
      background: linear-gradient(135deg, #f9fafb 0%, #f3f4f6 100%);
      border-radius: 20px;
      padding: 2rem;
      min-height: 200px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      border: 3px solid #e5e7eb;
      margin-bottom: 1.5rem;
      transition: all 0.4s ease;
    }

    .result-display.has-result {
      border-color: #667eea;
      background: linear-gradient(135deg, #f0f4ff 0%, #e8efff 100%);
    }

    .placeholder-message {
      color: #9ca3af;
      font-size: 1.1rem;
      text-align: center;
      font-weight: 500;
    }

    .food-emoji {
      font-size: 5rem;
      margin-bottom: 1rem;
      animation: fadeInScale 0.5s ease;
    }

    @keyframes fadeInScale {
      from {
        opacity: 0;
        transform: scale(0.5);
      }
      to {
        opacity: 1;
        transform: scale(1);
      }
    }

    .food-name {
      font-size: 1.75rem;
      font-weight: 700;
      color: #1f2937;
      margin-bottom: 1.25rem;
      text-align: center;
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
      font-size: 0.875rem;
      color: #6b7280;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }

    .nutrition-value {
      font-size: 1.5rem;
      font-weight: 700;
      color: #667eea;
    }

    .random-btn {
      background: #667eea;
      color: white;
      border: none;
      padding: 1rem 2.5rem;
      font-size: 1.2rem;
      font-weight: 700;
      border-radius: 16px;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 6px 20px rgba(102, 126, 234, 0.4);
      font-family: inherit;
      width: 100%;
    }

    .random-btn:hover {
      background: #5568d3;
      transform: translateY(-3px);
      box-shadow: 0 8px 25px rgba(102, 126, 234, 0.5);
    }

    .random-btn:active {
      transform: translateY(-1px);
    }

    .random-btn:disabled {
      background: #9ca3af;
      cursor: not-allowed;
      box-shadow: none;
      transform: none;
    }

    @media (max-width: 768px) {
      .main-title {
        font-size: 2.25rem;
      }

      .subtitle {
        font-size: 1.1rem;
      }

      .categories-grid {
        grid-template-columns: 1fr;
      }

      .food-emoji {
        font-size: 4rem;
      }

      .food-name {
        font-size: 1.5rem;
      }

      .app-wrapper {
        padding: 1.5rem 0.75rem;
      }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="app-wrapper">
   <header class="header">
    <h1 class="main-title" id="mainTitle">วันนี้กินอะไรดี</h1>
    <p class="subtitle" id="subtitle">สุ่มเมนูอาหารแสนอร่อย 🍽️</p>
   </header>
   <main class="categories-grid"><!-- จานหลัก -->
    <article class="category-card">
     <div class="category-header">
      <div class="category-icon">
       🍽️
      </div>
      <h2 class="category-title" id="mainDishLabel">จานหลัก</h2>
     </div>
     <div class="result-display" id="mainDishDisplay">
      <p class="placeholder-message">กดปุ่มด้านล่างเพื่อสุ่ม! 🎲</p>
     </div><button class="random-btn" id="mainDishBtn" type="button"> <span id="mainDishBtnText">สุ่มเมนู</span> </button>
    </article><!-- ของว่าง -->
    <article class="category-card">
     <div class="category-header">
      <div class="category-icon">
       🍰
      </div>
      <h2 class="category-title" id="snackLabel">ของว่าง</h2>
     </div>
     <div class="result-display" id="snackDisplay">
      <p class="placeholder-message">กดปุ่มด้านล่างเพื่อสุ่ม! 🎲</p>
     </div><button class="random-btn" id="snackBtn" type="button"> <span id="snackBtnText">สุ่มเมนู</span> </button>
    </article><!-- เครื่องดื่ม -->
    <article class="category-card">
     <div class="category-header">
      <div class="category-icon">
       🥤
      </div>
      <h2 class="category-title" id="drinkLabel">เครื่องดื่ม</h2>
     </div>
     <div class="result-display" id="drinkDisplay">
      <p class="placeholder-message">กดปุ่มด้านล่างเพื่อสุ่ม! 🎲</p>
     </div><button class="random-btn" id="drinkBtn" type="button"> <span id="drinkBtnText">สุ่มเมนู</span> </button>
    </article>
   </main>
  </div>
  <script>
    const defaultConfig = {
      main_title: "วันนี้กินอะไรดี",
      subtitle: "สุ่มเมนูอาหารแสนอร่อย 🍽️",
      main_dish_label: "จานหลัก",
      snack_label: "ของว่าง",
      drink_label: "เครื่องดื่ม",
      random_button_text: "สุ่มเมนู",
      background_color: "#667eea",
      secondary_color: "#764ba2",
      card_background: "#ffffff",
      text_color: "#1f2937",
      accent_color: "#667eea",
      font_family: "Kanit",
      font_size: 16
    };

    const menuData = {
      mainDish: [
        { name: "ข้าวผัด", emoji: "🍚", kcal: 450, sugar: 5 },
        { name: "ก๋วยเตี๋ยว", emoji: "🍜", kcal: 380, sugar: 8 },
        { name: "ส้มตำ", emoji: "🥗", kcal: 150, sugar: 15 },
        { name: "ผัดไทย", emoji: "🍝", kcal: 520, sugar: 12 },
        { name: "ข้าวมันไก่", emoji: "🍗", kcal: 650, sugar: 3 },
        { name: "ต้มยำกุ้ง", emoji: "🍲", kcal: 200, sugar: 6 },
        { name: "แกงเขียวหวาน", emoji: "🍛", kcal: 420, sugar: 8 },
        { name: "ข้าวขาหมู", emoji: "🍖", kcal: 750, sugar: 10 },
        { name: "ข้าวกะเพรา", emoji: "🍽️", kcal: 580, sugar: 7 },
        { name: "ข้าวซอย", emoji: "🥘", kcal: 680, sugar: 9 }
      ],
      snack: [
        { name: "ขนมปังปิ้ง", emoji: "🍞", kcal: 180, sugar: 8 },
        { name: "ปอปคอร์น", emoji: "🍿", kcal: 150, sugar: 2 },
        { name: "กล้วยทอด", emoji: "🍌", kcal: 220, sugar: 18 },
        { name: "ข้าวเหนียวมะม่วง", emoji: "🥭", kcal: 350, sugar: 25 },
        { name: "บัวลอย", emoji: "🍡", kcal: 280, sugar: 22 },
        { name: "ขนมครก", emoji: "🥞", kcal: 200, sugar: 15 },
        { name: "ลูกชิ้นทอด", emoji: "🍢", kcal: 240, sugar: 3 },
        { name: "ถั่วทอง", emoji: "🥜", kcal: 190, sugar: 20 },
        { name: "ขนมถ้วยฟู", emoji: "🧁", kcal: 160, sugar: 14 },
        { name: "ไอศกรีม", emoji: "🍦", kcal: 210, sugar: 21 }
      ],
      drink: [
        { name: "ชาเขียว", emoji: "🍵", kcal: 50, sugar: 8 },
        { name: "กาแฟเย็น", emoji: "☕", kcal: 120, sugar: 15 },
        { name: "น้ำส้ม", emoji: "🍊", kcal: 110, sugar: 21 },
        { name: "ชาไทย", emoji: "🥤", kcal: 180, sugar: 28 },
        { name: "น้ำแตงโม", emoji: "🍉", kcal: 90, sugar: 18 },
        { name: "นมสด", emoji: "🥛", kcal: 150, sugar: 12 },
        { name: "โกโก้", emoji: "🍫", kcal: 200, sugar: 24 },
        { name: "สมูทตี้", emoji: "🥤", kcal: 220, sugar: 32 },
        { name: "ชามะนาว", emoji: "🍋", kcal: 80, sugar: 16 },
        { name: "น้ำมะพร้าว", emoji: "🥥", kcal: 95, sugar: 12 }
      ]
    };

    function randomizeMenu(category) {
      const displayId = category + 'Display';
      const btnId = category + 'Btn';
      const display = document.getElementById(displayId);
      const button = document.getElementById(btnId);
      
      if (!display || !button) return;
      
      button.disabled = true;
      display.classList.remove('has-result');
      display.innerHTML = '<p class="placeholder-message">กำลังสุ่ม... 🎲</p>';
      
      setTimeout(function() {
        const foods = menuData[category];
        const randomIndex = Math.floor(Math.random() * foods.length);
        const food = foods[randomIndex];
        
        display.classList.add('has-result');
        display.innerHTML = 
          '<div class="food-emoji">' + food.emoji + '</div>' +
          '<div class="food-name">' + food.name + '</div>' +
          '<div class="nutrition-info">' +
            '<div class="nutrition-item">' +
              '<span class="nutrition-label">แคลอรี</span>' +
              '<span class="nutrition-value">' + food.kcal + ' kcal</span>' +
            '</div>' +
            '<div class="nutrition-item">' +
              '<span class="nutrition-label">น้ำตาล</span>' +
              '<span class="nutrition-value">' + food.sugar + ' g</span>' +
            '</div>' +
          '</div>';
        
        button.disabled = false;
      }, 800);
    }

    async function onConfigChange(config) {
      const customFont = config.font_family || defaultConfig.font_family;
      const baseFontSize = config.font_size || defaultConfig.font_size;
      const baseFontStack = "'Sarabun', 'Segoe UI', Tahoma, sans-serif";

      document.body.style.fontFamily = customFont + ', ' + baseFontStack;
      
      const appWrapper = document.querySelector('.app-wrapper');
      const bgColor = config.background_color || defaultConfig.background_color;
      const secondaryColor = config.secondary_color || defaultConfig.secondary_color;
      appWrapper.style.background = 'linear-gradient(135deg, ' + bgColor + ' 0%, ' + secondaryColor + ' 100%)';

      const cards = document.querySelectorAll('.category-card');
      const cardBg = config.card_background || defaultConfig.card_background;
      cards.forEach(function(card) {
        card.style.backgroundColor = cardBg;
      });

      const mainTitle = document.getElementById('mainTitle');
      mainTitle.textContent = config.main_title || defaultConfig.main_title;
      mainTitle.style.fontSize = (baseFontSize * 3) + 'px';

      const subtitle = document.getElementById('subtitle');
      subtitle.textContent = config.subtitle || defaultConfig.subtitle;
      subtitle.style.fontSize = (baseFontSize * 1.25) + 'px';

      const accentColor = config.accent_color || defaultConfig.accent_color;

      const mainDishLabel = document.getElementById('mainDishLabel');
      mainDishLabel.textContent = config.main_dish_label || defaultConfig.main_dish_label;
      mainDishLabel.style.color = accentColor;
      mainDishLabel.style.fontSize = (baseFontSize * 1.75) + 'px';

      const snackLabel = document.getElementById('snackLabel');
      snackLabel.textContent = config.snack_label || defaultConfig.snack_label;
      snackLabel.style.color = accentColor;
      snackLabel.style.fontSize = (baseFontSize * 1.75) + 'px';

      const drinkLabel = document.getElementById('drinkLabel');
      drinkLabel.textContent = config.drink_label || defaultConfig.drink_label;
      drinkLabel.style.color = accentColor;
      drinkLabel.style.fontSize = (baseFontSize * 1.75) + 'px';

      const buttonText = config.random_button_text || defaultConfig.random_button_text;
      document.getElementById('mainDishBtnText').textContent = buttonText;
      document.getElementById('snackBtnText').textContent = buttonText;
      document.getElementById('drinkBtnText').textContent = buttonText;

      const buttons = document.querySelectorAll('.random-btn');
      buttons.forEach(function(button) {
        button.style.backgroundColor = accentColor;
        button.style.fontSize = (baseFontSize * 1.2) + 'px';
      });

      const textColor = config.text_color || defaultConfig.text_color;
      document.querySelectorAll('.food-name').forEach(function(el) {
        el.style.color = textColor;
        el.style.fontSize = (baseFontSize * 1.75) + 'px';
      });

      document.querySelectorAll('.nutrition-value').forEach(function(el) {
        el.style.color = accentColor;
        el.style.fontSize = (baseFontSize * 1.5) + 'px';
      });

      document.querySelectorAll('.nutrition-label').forEach(function(el) {
        el.style.fontSize = (baseFontSize * 0.875) + 'px';
      });

      document.querySelectorAll('.placeholder-message').forEach(function(el) {
        el.style.fontSize = (baseFontSize * 1.1) + 'px';
      });

      document.querySelectorAll('.result-display.has-result').forEach(function(el) {
        el.style.borderColor = accentColor;
      });
    }

    document.getElementById('mainDishBtn').addEventListener('click', function() {
      randomizeMenu('mainDish');
    });
    
    document.getElementById('snackBtn').addEventListener('click', function() {
      randomizeMenu('snack');
    });
    
    document.getElementById('drinkBtn').addEventListener('click', function() {
      randomizeMenu('drink');
    });

    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig: defaultConfig,
        onConfigChange: onConfigChange,
        mapToCapabilities: function(config) {
          return {
            recolorables: [
              {
                get: function() { return config.background_color || defaultConfig.background_color; },
                set: function(value) {
                  config.background_color = value;
                  window.elementSdk.setConfig({ background_color: value });
                }
              },
              {
                get: function() { return config.secondary_color || defaultConfig.secondary_color; },
                set: function(value) {
                  config.secondary_color = value;
                  window.elementSdk.setConfig({ secondary_color: value });
                }
              },
              {
                get: function() { return config.card_background || defaultConfig.card_background; },
                set: function(value) {
                  config.card_background = value;
                  window.elementSdk.setConfig({ card_background: value });
                }
              },
              {
                get: function() { return config.text_color || defaultConfig.text_color; },
                set: function(value) {
                  config.text_color = value;
                  window.elementSdk.setConfig({ text_color: value });
                }
              },
              {
                get: function() { return config.accent_color || defaultConfig.accent_color; },
                set: function(value) {
                  config.accent_color = value;
                  window.elementSdk.setConfig({ accent_color: value });
                }
              }
            ],
            borderables: [],
            fontEditable: {
              get: function() { return config.font_family || defaultConfig.font_family; },
              set: function(value) {
                config.font_family = value;
                window.elementSdk.setConfig({ font_family: value });
              }
            },
            fontSizeable: {
              get: function() { return config.font_size || defaultConfig.font_size; },
              set: function(value) {
                config.font_size = value;
                window.elementSdk.setConfig({ font_size: value });
              }
            }
          };
        },
        mapToEditPanelValues: function(config) {
          return new Map([
            ["main_title", config.main_title || defaultConfig.main_title],
            ["subtitle", config.subtitle || defaultConfig.subtitle],
            ["main_dish_label", config.main_dish_label || defaultConfig.main_dish_label],
            ["snack_label", config.snack_label || defaultConfig.snack_label],
            ["drink_label", config.drink_label || defaultConfig.drink_label],
            ["random_button_text", config.random_button_text || defaultConfig.random_button_text]
          ]);
        }
      });
    }
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a565d6eb2b00886',t:'MTc2NDI5NjIwNC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
