<html lang="th">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>แบบประเมินสุขภาพดิจิทัลส่วนตัว</title>
  <script src="/_sdk/element_sdk.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700;800&amp;display=swap" rel="stylesheet">
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Sarabun', 'Noto Sans Thai', -apple-system, sans-serif;
    }
    
    html, body {
      height: 100%;
    }
    
    * {
      box-sizing: border-box;
    }
    
    .fade-in {
      animation: fadeIn 0.8s ease-out;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(40px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .slide-up {
      animation: slideUp 0.6s ease-out;
    }
    
    @keyframes slideUp {
      from { opacity: 0; transform: translateY(60px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .scale-in {
      animation: scaleIn 0.5s ease-out;
    }
    
    @keyframes scaleIn {
      from { opacity: 0; transform: scale(0.9); }
      to { opacity: 1; transform: scale(1); }
    }
    
    .progress-bar {
      transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    }
    
    .radio-option {
      appearance: none;
      width: 24px;
      height: 24px;
      border: 3px solid #cbd5e1;
      border-radius: 50%;
      cursor: pointer;
      position: relative;
      transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
      flex-shrink: 0;
    }
    
    .radio-option:checked {
      border-color: #3b82f6;
      background: #3b82f6;
      transform: scale(1.1);
    }
    
    .radio-option:checked::after {
      content: '';
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 10px;
      height: 10px;
      background: white;
      border-radius: 50%;
    }
    
    .radio-option:hover {
      border-color: #3b82f6;
      transform: scale(1.05);
    }
    
    .question-card {
      transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
      border: 3px solid transparent;
    }
    
    .question-card:hover {
      transform: translateY(-8px);
      box-shadow: 0 20px 40px rgba(59, 130, 246, 0.15);
      border-color: rgba(59, 130, 246, 0.2);
    }
    
    .option-label {
      transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }
    
    .option-label:hover {
      transform: translateX(8px);
      background: linear-gradient(135deg, #eff6ff, #dbeafe) !important;
    }
    
    .badge {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 8px 16px;
      border-radius: 12px;
      font-size: 14px;
      font-weight: 700;
    }
    
    .result-modal {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.85);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 9999;
      padding: 20px;
      animation: fadeIn 0.4s ease-out;
    }
    
    .result-content {
      background: white;
      border-radius: 28px;
      max-width: 900px;
      width: 100%;
      max-height: 90%;
      overflow-y: auto;
      box-shadow: 0 30px 80px rgba(0, 0, 0, 0.4);
    }
    
    .score-circle {
      width: 180px;
      height: 180px;
      border-radius: 50%;
      background: linear-gradient(135deg, #3b82f6, #8b5cf6);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      box-shadow: 0 15px 40px rgba(59, 130, 246, 0.4);
      animation: pulse 2s ease-in-out infinite;
    }
    
    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.05); }
    }
    
    .dimension-card {
      background: linear-gradient(135deg, #f8fafc, #f1f5f9);
      border-radius: 16px;
      padding: 24px;
      border: 2px solid #e2e8f0;
      transition: all 0.3s ease;
    }
    
    .dimension-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 12px 28px rgba(0, 0, 0, 0.12);
      border-color: #3b82f6;
    }
    
    .score-bar-container {
      background: #e2e8f0;
      height: 14px;
      border-radius: 7px;
      overflow: hidden;
      position: relative;
    }
    
    .score-bar {
      height: 100%;
      border-radius: 7px;
      transition: width 1s cubic-bezier(0.4, 0, 0.2, 1);
      position: relative;
    }
    
    .score-bar::after {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
      animation: shimmer 2s infinite;
    }
    
    @keyframes shimmer {
      0% { transform: translateX(-100%); }
      100% { transform: translateX(100%); }
    }
    
    .recommendation-box {
      background: linear-gradient(135deg, #eff6ff, #dbeafe);
      border: 3px solid #3b82f6;
      border-radius: 20px;
      padding: 32px;
      margin-top: 32px;
    }
    
    .loading-spinner {
      border: 4px solid #e2e8f0;
      border-top: 4px solid #3b82f6;
      border-radius: 50%;
      width: 32px;
      height: 32px;
      animation: spin 0.8s linear infinite;
    }
    
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .btn-primary {
      background: linear-gradient(135deg, #3b82f6, #2563eb);
      transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .btn-primary:hover {
      background: linear-gradient(135deg, #2563eb, #1d4ed8);
      transform: translateY(-2px);
      box-shadow: 0 12px 32px rgba(59, 130, 246, 0.4);
    }

    .btn-primary:active {
      transform: translateY(0);
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body style="width: 100%; min-height: 100%; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);">
  <div id="app-container" style="width: 100%; min-height: 100%;"><!-- Assessment Form -->
   <div id="assessment-section"><!-- Header -->
    <header style="background: white; padding: 40px 24px; box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);">
     <div style="max-width: 1000px; margin: 0 auto; text-align: center;">
      <div class="fade-in" style="font-size: 80px; margin-bottom: 24px;">
       🏥💻
      </div>
      <h1 id="main-title" style="font-size: 42px; font-weight: 800; color: #1e293b; margin: 0 0 16px 0; line-height: 1.2;">แบบประเมินสุขภาพดิจิทัลส่วนตัวแบบอัตโนมัติ</h1>
      <p id="subtitle" style="font-size: 20px; color: #64748b; margin: 0 0 20px 0; font-weight: 500; line-height: 1.6;">ประเมินตนเอง 10 ข้อ ตามกรอบ Norman &amp; Skinner และ Okan, Dadaczynski</p>
      <div style="display: flex; gap: 12px; justify-content: center; flex-wrap: wrap; margin-top: 24px;"><span class="badge" style="background: #dbeafe; color: #1e40af;">6 ด้านของ Norman &amp; Skinner</span> <span class="badge" style="background: #fce7f3; color: #9f1239;">3 มิติของ Okan &amp; Dadaczynski</span>
      </div>
     </div>
    </header>
    <main style="max-width: 1000px; margin: 0 auto; padding: 48px 24px;"><!-- Instructions Card -->
     <div class="fade-in" style="background: white; padding: 32px; border-radius: 24px; box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12); margin-bottom: 40px;">
      <div style="display: flex; align-items: center; gap: 16px; margin-bottom: 20px;"><span style="font-size: 48px;">📝</span>
       <h2 style="font-size: 28px; font-weight: 700; color: #1e293b; margin: 0;">คำแนะนำในการทำแบบประเมิน</h2>
      </div>
      <p id="instruction" style="font-size: 18px; color: #475569; margin: 0 0 20px 0; line-height: 1.8; font-weight: 500;">กรุณาเลือกคำตอบที่ตรงกับตัวคุณมากที่สุด ในแต่ละข้อคำถามจะมีตัวเลือก 5 ระดับ</p><!-- Progress Bar -->
      <div style="margin-top: 28px;">
       <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;"><span style="font-size: 16px; color: #64748b; font-weight: 600;">ความคืบหน้า</span> <span id="progress-text" style="font-size: 18px; font-weight: 800; color: #3b82f6;">0/10 ข้อ</span>
       </div>
       <div style="background: #e2e8f0; height: 16px; border-radius: 8px; overflow: hidden; box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);">
        <div id="progress-bar" class="progress-bar" style="background: linear-gradient(90deg, #3b82f6, #8b5cf6); height: 100%; width: 0%; border-radius: 8px;"></div>
       </div>
      </div>
     </div><!-- Questions Form -->
     <form id="assessment-form">
      <div id="questions-container"></div><!-- Submit Button -->
      <div class="slide-up" style="background: white; padding: 40px; border-radius: 24px; box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12); text-align: center; margin-top: 40px;"><button type="submit" id="submit-btn" class="btn-primary" style="color: white; padding: 20px 64px; border: none; border-radius: 16px; font-size: 22px; font-weight: 800; cursor: pointer; box-shadow: 0 8px 24px rgba(59, 130, 246, 0.3); border: 3px solid transparent;"> <span style="display: flex; align-items: center; gap: 12px; justify-content: center;"> <span id="button-text">ส่งแบบประเมิน</span> <span style="font-size: 28px;">🚀</span> </span> </button>
      </div>
     </form>
    </main>
   </div><!-- Footer -->
   <footer style="background: rgba(255, 255, 255, 0.15); backdrop-filter: blur(10px); color: white; text-align: center; padding: 32px 24px; margin-top: 80px;">
    <p id="footer-text" style="margin: 0; font-size: 16px; font-weight: 600;">Digital Health Literacy Assessment © 2024</p>
   </footer>
  </div>
  <script>
    const defaultConfig = {
      main_title: "แบบประเมินสุขภาพดิจิทัลส่วนตัวแบบอัตโนมัติ",
      subtitle: "ประเมินตนเอง 10 ข้อ ตามกรอบ Norman & Skinner และ Okan, Dadaczynski",
      instruction: "กรุณาเลือกคำตอบที่ตรงกับตัวคุณมากที่สุด ในแต่ละข้อคำถามจะมีตัวเลือก 5 ระดับ",
      button_text: "ส่งแบบประเมิน",
      footer_text: "Digital Health Literacy Assessment © 2024",
      background_color: "#667eea",
      card_color: "#ffffff",
      text_color: "#1e293b",
      primary_color: "#3b82f6",
      accent_color: "#8b5cf6"
    };

    // คำถาม 10 ข้อ แบ่งตาม 6 ด้าน (Norman & Skinner) และ 3 มิติ (Okan, Dadaczynski)
    const questions = [
      {
        id: 1,
        question: "ฉันสามารถใช้อุปกรณ์ดิจิทัล (สมาร์ทโฟน/แท็บเล็ต/คอมพิวเตอร์) เพื่อค้นหาข้อมูลสุขภาพได้อย่างมั่นใจ",
        dimension_ns: "Digital Literacy",
        dimension_od: "Functional",
        emoji: "💻",
        color: "#3b82f6",
        bg: "#dbeafe"
      },
      {
        id: 2,
        question: "ฉันสามารถค้นหาและระบุแหล่งข้อมูลสุขภาพที่น่าเชื่อถือบนอินเทอร์เน็ตได้",
        dimension_ns: "Information Literacy",
        dimension_od: "Interactive",
        emoji: "🔍",
        color: "#8b5cf6",
        bg: "#f3e8ff"
      },
      {
        id: 3,
        question: "ฉันเข้าใจคำศัพท์ทางการแพทย์และสุขภาพที่พบในเว็บไซต์หรือแอปพลิเคชัน",
        dimension_ns: "Health Literacy",
        dimension_od: "Functional",
        emoji: "🏥",
        color: "#10b981",
        bg: "#d1fae5"
      },
      {
        id: 4,
        question: "ฉันสามารถสื่อสารและแบ่งปันข้อมูลสุขภาพกับผู้อื่นผ่านช่องทางออนไลน์ได้อย่างเหมาะสม",
        dimension_ns: "Communication Literacy",
        dimension_od: "Interactive",
        emoji: "💬",
        color: "#f59e0b",
        bg: "#fef3c7"
      },
      {
        id: 5,
        question: "ฉันประเมินและพิจารณาความน่าเชื่อถือของข้อมูลสุขภาพที่พบออนไลน์อย่างรอบคอบ",
        dimension_ns: "Critical Thinking",
        dimension_od: "Critical",
        emoji: "🧠",
        color: "#ec4899",
        bg: "#fce7f3"
      },
      {
        id: 6,
        question: "ฉันรู้วิธีปกป้องข้อมูลส่วนตัวและความเป็นส่วนตัวเมื่อใช้บริการสุขภาพออนไลน์",
        dimension_ns: "Privacy & Security",
        dimension_od: "Critical",
        emoji: "🔒",
        color: "#ef4444",
        bg: "#fee2e2"
      },
      {
        id: 7,
        question: "ฉันสามารถใช้แอปพลิเคชันสุขภาพเพื่อติดตามและจัดการสุขภาพของตนเองได้",
        dimension_ns: "Digital Literacy",
        dimension_od: "Functional",
        emoji: "📱",
        color: "#3b82f6",
        bg: "#dbeafe"
      },
      {
        id: 8,
        question: "ฉันสามารถเปรียบเทียบข้อมูลจากหลายแหล่งเพื่อตัดสินใจเรื่องสุขภาพได้",
        dimension_ns: "Information Literacy",
        dimension_od: "Critical",
        emoji: "⚖️",
        color: "#8b5cf6",
        bg: "#f3e8ff"
      },
      {
        id: 9,
        question: "ฉันสามารถใช้ข้อมูลสุขภาพที่ได้จากออนไลน์เพื่อปรับปรุงพฤติกรรมสุขภาพของตนเอง",
        dimension_ns: "Health Literacy",
        dimension_od: "Interactive",
        emoji: "✨",
        color: "#10b981",
        bg: "#d1fae5"
      },
      {
        id: 10,
        question: "ฉันระมัดระวังในการแชร์ข้อมูลสุขภาพส่วนตัวบนโซเชียลมีเดีย",
        dimension_ns: "Privacy & Security",
        dimension_od: "Critical",
        emoji: "🛡️",
        color: "#ef4444",
        bg: "#fee2e2"
      }
    ];

    const answerOptions = [
      { value: 5, label: "เห็นด้วยอย่างยิ่ง", color: "#10b981", bg: "#d1fae5" },
      { value: 4, label: "เห็นด้วย", color: "#84cc16", bg: "#ecfccb" },
      { value: 3, label: "ไม่แน่ใจ", color: "#f59e0b", bg: "#fef3c7" },
      { value: 2, label: "ไม่เห็นด้วย", color: "#f97316", bg: "#ffedd5" },
      { value: 1, label: "ไม่เห็นด้วยอย่างยิ่ง", color: "#ef4444", bg: "#fee2e2" }
    ];

    async function onConfigChange(config) {
      document.getElementById('main-title').textContent = config.main_title || defaultConfig.main_title;
      document.getElementById('subtitle').textContent = config.subtitle || defaultConfig.subtitle;
      document.getElementById('instruction').textContent = config.instruction || defaultConfig.instruction;
      document.getElementById('button-text').textContent = config.button_text || defaultConfig.button_text;
      document.getElementById('footer-text').textContent = config.footer_text || defaultConfig.footer_text;

      const bgColor = config.background_color || defaultConfig.background_color;
      document.getElementById('app-container').style.background = `linear-gradient(135deg, ${bgColor} 0%, ${bgColor}dd 100%)`;
    }

    function init() {
      if (window.elementSdk) {
        window.elementSdk.init({
          defaultConfig,
          onConfigChange,
          mapToCapabilities: (config) => ({
            recolorables: [
              {
                get: () => config.background_color || defaultConfig.background_color,
                set: (value) => {
                  window.elementSdk.config.background_color = value;
                  window.elementSdk.setConfig({ background_color: value });
                }
              },
              {
                get: () => config.card_color || defaultConfig.card_color,
                set: (value) => {
                  window.elementSdk.config.card_color = value;
                  window.elementSdk.setConfig({ card_color: value });
                }
              },
              {
                get: () => config.text_color || defaultConfig.text_color,
                set: (value) => {
                  window.elementSdk.config.text_color = value;
                  window.elementSdk.setConfig({ text_color: value });
                }
              },
              {
                get: () => config.primary_color || defaultConfig.primary_color,
                set: (value) => {
                  window.elementSdk.config.primary_color = value;
                  window.elementSdk.setConfig({ primary_color: value });
                }
              },
              {
                get: () => config.accent_color || defaultConfig.accent_color,
                set: (value) => {
                  window.elementSdk.config.accent_color = value;
                  window.elementSdk.setConfig({ accent_color: value });
                }
              }
            ],
            borderables: [],
            fontEditable: undefined,
            fontSizeable: undefined
          }),
          mapToEditPanelValues: (config) => new Map([
            ["main_title", config.main_title || defaultConfig.main_title],
            ["subtitle", config.subtitle || defaultConfig.subtitle],
            ["instruction", config.instruction || defaultConfig.instruction],
            ["button_text", config.button_text || defaultConfig.button_text],
            ["footer_text", config.footer_text || defaultConfig.footer_text]
          ])
        });
      }

      renderQuestions();
      setupForm();
    }

    function renderQuestions() {
      const container = document.getElementById('questions-container');
      
      container.innerHTML = questions.map((q, index) => `
        <div class="question-card scale-in" style="background: white; padding: 36px; border-radius: 24px; box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12); margin-bottom: 32px; animation-delay: ${index * 0.1}s;">
          
          <!-- Question Header -->
          <div style="display: flex; align-items: center; gap: 16px; margin-bottom: 24px; flex-wrap: wrap;">
            <span style="font-size: 48px;">${q.emoji}</span>
            <div style="flex: 1;">
              <div style="display: flex; gap: 10px; margin-bottom: 12px; flex-wrap: wrap;">
                <span class="badge" style="background: ${q.bg}; color: ${q.color}; font-size: 13px;">
                  ${q.dimension_ns}
                </span>
                <span class="badge" style="background: #fef3c7; color: #92400e; font-size: 13px;">
                  ${q.dimension_od}
                </span>
              </div>
            </div>
          </div>

          <!-- Question Text -->
          <label style="display: block; margin-bottom: 24px;">
            <div style="display: flex; align-items: start; gap: 12px;">
              <span style="background: linear-gradient(135deg, #3b82f6, #8b5cf6); color: white; padding: 8px 16px; border-radius: 12px; font-weight: 800; font-size: 18px; flex-shrink: 0;">${q.id}</span>
              <span style="color: #1e293b; font-weight: 600; font-size: 19px; line-height: 1.7; flex: 1;">${q.question}</span>
            </div>
          </label>

          <!-- Answer Options -->
          <div style="display: grid; gap: 14px;">
            ${answerOptions.map(option => `
              <label class="option-label" style="display: flex; align-items: center; gap: 16px; cursor: pointer; padding: 18px 24px; border: 3px solid #e2e8f0; border-radius: 16px; background: white;" 
                onmouseover="if(!this.querySelector('input').checked) { this.style.background='${option.bg}'; this.style.borderColor='${option.color}'; }"
                onmouseout="if(!this.querySelector('input').checked) { this.style.background='white'; this.style.borderColor='#e2e8f0'; }">
                <input type="radio" name="q${q.id}" value="${option.value}" class="radio-option" required 
                  onchange="this.parentElement.style.background='${option.bg}'; this.parentElement.style.borderColor='${option.color}'; updateProgress();">
                <span style="font-size: 17px; color: #1e293b; font-weight: 600; flex: 1;">${option.label}</span>
                <span style="font-size: 16px; font-weight: 700; color: ${option.color}; min-width: 28px; text-align: center;">${option.value}</span>
              </label>
            `).join('')}
          </div>

        </div>
      `).join('');
    }

    function setupForm() {
      const form = document.getElementById('assessment-form');
      
      form.addEventListener('submit', (e) => {
        e.preventDefault();
        calculateAndShowResults();
      });
    }

    function updateProgress() {
      const totalQuestions = questions.length;
      let answeredCount = 0;

      questions.forEach(q => {
        if (document.querySelector(`input[name="q${q.id}"]:checked`)) {
          answeredCount++;
        }
      });

      const percentage = (answeredCount / totalQuestions) * 100;
      document.getElementById('progress-bar').style.width = percentage + '%';
      document.getElementById('progress-text').textContent = `${answeredCount}/${totalQuestions} ข้อ`;
    }

    function calculateAndShowResults() {
      const submitBtn = document.getElementById('submit-btn');
      const originalHTML = submitBtn.innerHTML;
      submitBtn.innerHTML = '<div class="loading-spinner" style="margin: 0 auto;"></div>';
      submitBtn.disabled = true;

      // รวบรวมคะแนน
      const scores = {};
      const nsDimensions = {};
      const odDimensions = {};

      questions.forEach(q => {
        const answer = document.querySelector(`input[name="q${q.id}"]:checked`);
        const score = parseInt(answer.value);
        scores[`q${q.id}`] = score;

        // คำนวณคะแนนตาม Norman & Skinner (6 ด้าน)
        if (!nsDimensions[q.dimension_ns]) {
          nsDimensions[q.dimension_ns] = { total: 0, count: 0 };
        }
        nsDimensions[q.dimension_ns].total += score;
        nsDimensions[q.dimension_ns].count += 1;

        // คำนวณคะแนนตาม Okan & Dadaczynski (3 มิติ)
        if (!odDimensions[q.dimension_od]) {
          odDimensions[q.dimension_od] = { total: 0, count: 0 };
        }
        odDimensions[q.dimension_od].total += score;
        odDimensions[q.dimension_od].count += 1;
      });

      // คำนวณคะแนนเฉลี่ยแต่ละด้าน (0-100)
      const nsScores = {};
      Object.keys(nsDimensions).forEach(dim => {
        nsScores[dim] = (nsDimensions[dim].total / nsDimensions[dim].count / 5) * 100;
      });

      const odScores = {};
      Object.keys(odDimensions).forEach(dim => {
        odScores[dim] = (odDimensions[dim].total / odDimensions[dim].count / 5) * 100;
      });

      // คะแนนรวมทั้งหมด
      const totalScore = Object.values(scores).reduce((a, b) => a + b, 0);
      const averageScore = (totalScore / (questions.length * 5)) * 100;

      setTimeout(() => {
        submitBtn.innerHTML = originalHTML;
        submitBtn.disabled = false;
        showResultModal(averageScore, nsScores, odScores, scores);
      }, 1000);
    }

    function showResultModal(overallScore, nsScores, odScores, rawScores) {
      const modal = document.createElement('div');
      modal.className = 'result-modal';

      const level = getScoreLevel(overallScore);
      const recommendation = getRecommendation(overallScore, nsScores, odScores);

      modal.innerHTML = `
        <div class="result-content scale-in">
          
          <!-- Header Section -->
          <div style="background: linear-gradient(135deg, #667eea, #764ba2); padding: 48px 40px; border-radius: 28px 28px 0 0; text-align: center; color: white;">
            <div style="font-size: 80px; margin-bottom: 20px;">🎉</div>
            <h2 style="font-size: 36px; font-weight: 800; margin: 0 0 16px 0;">ผลการประเมินของคุณ</h2>
            <p style="font-size: 18px; opacity: 0.95; margin: 0;">Digital Health Literacy Assessment</p>
          </div>

          <!-- Score Section -->
          <div style="padding: 48px 40px; text-align: center;">
            <div style="display: flex; justify-content: center; margin-bottom: 32px;">
              <div class="score-circle">
                <div style="font-size: 56px; font-weight: 800; color: white; line-height: 1;">${Math.round(overallScore)}</div>
                <div style="font-size: 18px; font-weight: 600; color: white; opacity: 0.9;">คะแนน</div>
              </div>
            </div>

            <div style="display: inline-flex; align-items: center; gap: 12px; padding: 14px 32px; border-radius: 20px; font-size: 22px; font-weight: 800; background: ${level.bg}; color: ${level.color}; box-shadow: 0 4px 16px ${level.color}40;">
              <span style="font-size: 32px;">${level.emoji}</span>
              <span>${level.label}</span>
            </div>

            <!-- Norman & Skinner Dimensions -->
            <div style="margin-top: 48px; text-align: left;">
              <h3 style="font-size: 26px; font-weight: 700; color: #1e293b; margin: 0 0 28px 0; display: flex; align-items: center; gap: 12px;">
                <span style="font-size: 36px;">📊</span>
                6 ด้านของ Norman & Skinner
              </h3>
              <div style="display: grid; gap: 20px;">
                ${Object.keys(nsScores).map(dim => createDimensionBar(dim, nsScores[dim], getDimensionEmoji(dim, 'ns'))).join('')}
              </div>
            </div>

            <!-- Okan & Dadaczynski Dimensions -->
            <div style="margin-top: 48px; text-align: left;">
              <h3 style="font-size: 26px; font-weight: 700; color: #1e293b; margin: 0 0 28px 0; display: flex; align-items: center; gap: 12px;">
                <span style="font-size: 36px;">🎯</span>
                3 มิติของ Okan & Dadaczynski
              </h3>
              <div style="display: grid; gap: 20px;">
                ${Object.keys(odScores).map(dim => createDimensionBar(dim, odScores[dim], getDimensionEmoji(dim, 'od'))).join('')}
              </div>
            </div>

            <!-- Recommendation -->
            <div class="recommendation-box">
              <div style="display: flex; align-items: start; gap: 16px;">
                <span style="font-size: 48px; flex-shrink: 0;">💡</span>
                <div style="text-align: left;">
                  <h3 style="font-size: 24px; font-weight: 700; color: #1e40af; margin: 0 0 16px 0;">คำแนะนำสำหรับคุณ</h3>
                  <div style="font-size: 17px; color: #1e293b; line-height: 1.8; font-weight: 500;">
                    ${recommendation}
                  </div>
                </div>
              </div>
            </div>

            <!-- Action Buttons -->
            <div style="display: flex; gap: 16px; margin-top: 40px; flex-wrap: wrap;">
              <button onclick="this.closest('.result-modal').remove()" class="btn-primary" style="flex: 1; min-width: 200px; color: white; padding: 18px 32px; border: none; border-radius: 14px; font-size: 18px; font-weight: 700; cursor: pointer;">
                ปิดหน้าต่าง
              </button>
              <button onclick="this.closest('.result-modal').remove(); window.scrollTo({top: 0, behavior: 'smooth'}); document.getElementById('assessment-form').reset(); updateProgress();" style="flex: 1; min-width: 200px; background: #64748b; color: white; padding: 18px 32px; border: none; border-radius: 14px; font-size: 18px; font-weight: 700; cursor: pointer; transition: all 0.3s;" onmouseover="this.style.background='#475569'" onmouseout="this.style.background='#64748b'">
                ทำแบบประเมินใหม่
              </button>
            </div>

          </div>

        </div>
      `;

      document.body.appendChild(modal);

      // Close on backdrop click
      modal.addEventListener('click', (e) => {
        if (e.target === modal) {
          modal.remove();
        }
      });
    }

    function createDimensionBar(dimension, score, emoji) {
      const barColor = score >= 70 ? '#10b981' : score >= 50 ? '#f59e0b' : '#ef4444';
      const level = score >= 70 ? 'ดีมาก' : score >= 50 ? 'ปานกลาง' : 'ควรพัฒนา';
      const levelBg = score >= 70 ? '#d1fae5' : score >= 50 ? '#fef3c7' : '#fee2e2';

      return `
        <div class="dimension-card">
          <div style="display: flex; justify-content: space-between; align-items: start; margin-bottom: 14px; flex-wrap: wrap; gap: 12px;">
            <div style="display: flex; align-items: center; gap: 10px;">
              <span style="font-size: 28px;">${emoji}</span>
              <span style="font-size: 17px; font-weight: 700; color: #1e293b;">${dimension}</span>
            </div>
            <div style="text-align: right;">
              <div style="font-size: 28px; font-weight: 800; color: ${barColor}; line-height: 1;">${Math.round(score)}</div>
              <span style="display: inline-block; margin-top: 4px; padding: 4px 12px; background: ${levelBg}; color: ${barColor}; border-radius: 8px; font-size: 13px; font-weight: 700;">${level}</span>
            </div>
          </div>
          <div class="score-bar-container">
            <div class="score-bar" style="width: ${score}%; background: ${barColor};"></div>
          </div>
        </div>
      `;
    }

    function getDimensionEmoji(dimension, type) {
      const emojiMap = {
        'ns': {
          'Digital Literacy': '💻',
          'Information Literacy': '🔍',
          'Health Literacy': '🏥',
          'Communication Literacy': '💬',
          'Critical Thinking': '🧠',
          'Privacy & Security': '🔒'
        },
        'od': {
          'Functional': '⚙️',
          'Interactive': '🤝',
          'Critical': '🎯'
        }
      };
      return emojiMap[type][dimension] || '📌';
    }

    function getScoreLevel(score) {
      if (score >= 80) {
        return { label: 'ดีเยี่ยม', emoji: '🌟', color: '#10b981', bg: '#d1fae5' };
      } else if (score >= 70) {
        return { label: 'ดี', emoji: '✅', color: '#84cc16', bg: '#ecfccb' };
      } else if (score >= 60) {
        return { label: 'ปานกลาง', emoji: '👍', color: '#f59e0b', bg: '#fef3c7' };
      } else if (score >= 50) {
        return { label: 'พอใช้', emoji: '⚠️', color: '#f97316', bg: '#ffedd5' };
      } else {
        return { label: 'ควรพัฒนา', emoji: '📚', color: '#ef4444', bg: '#fee2e2' };
      }
    }

    function getRecommendation(overallScore, nsScores, odScores) {
      let recommendations = [];

      // คำแนะนำตามคะแนนรวม
      if (overallScore >= 80) {
        recommendations.push(`<p><strong>🎉 ยอดเยี่ยม!</strong> คุณมีทักษะสุขภาพดิจิทัลที่ดีมาก สามารถค้นหา ประเมิน และใช้ข้อมูลสุขภาพดิจิทัลได้อย่างมีประสิทธิภาพและปลอดภัย</p>`);
      } else if (overallScore >= 70) {
        recommendations.push(`<p><strong>👏 ดีมาก!</strong> คุณมีพื้นฐานทักษะสุขภาพดิจิทัลที่ดี แต่ยังมีโอกาสพัฒนาให้ดียิ่งขึ้น</p>`);
      } else if (overallScore >= 60) {
        recommendations.push(`<p><strong>💪 ดี!</strong> คุณมีทักษะพื้นฐาน แต่ควรเพิ่มเติมความรู้และฝึกฝนในบางด้าน</p>`);
      } else if (overallScore >= 50) {
        recommendations.push(`<p><strong>📖 ควรพัฒนา!</strong> คุณควรเรียนรู้เพิ่มเติมเกี่ยวกับการใช้เทคโนโลยีเพื่อสุขภาพอย่างปลอดภัยและมีประสิทธิภาพ</p>`);
      } else {
        recommendations.push(`<p><strong>🎯 ต้องการความช่วยเหลือ!</strong> แนะนำให้หาความรู้เพิ่มเติมหรือปรึกษาผู้เชี่ยวชาญเพื่อพัฒนาทักษะสุขภาพดิจิทัล</p>`);
      }

      // คำแนะนำเฉพาะด้านที่ควรพัฒนา (Norman & Skinner)
      const weakNS = Object.keys(nsScores).filter(dim => nsScores[dim] < 60);
      if (weakNS.length > 0) {
        recommendations.push(`<p><strong>📌 ด้านที่ควรพัฒนา (Norman & Skinner):</strong></p><ul style="margin: 8px 0; padding-left: 24px;">`);
        weakNS.forEach(dim => {
          const advice = getNSDimensionAdvice(dim);
          recommendations.push(`<li style="margin: 6px 0;">${advice}</li>`);
        });
        recommendations.push(`</ul>`);
      }

      // คำแนะนำเฉพาะมิติที่ควรพัฒนา (Okan & Dadaczynski)
      const weakOD = Object.keys(odScores).filter(dim => odScores[dim] < 60);
      if (weakOD.length > 0) {
        recommendations.push(`<p><strong>🎯 มิติที่ควรพัฒนา (Okan & Dadaczynski):</strong></p><ul style="margin: 8px 0; padding-left: 24px;">`);
        weakOD.forEach(dim => {
          const advice = getODDimensionAdvice(dim);
          recommendations.push(`<li style="margin: 6px 0;">${advice}</li>`);
        });
        recommendations.push(`</ul>`);
      }

      // คำแนะนำทั่วไป
      recommendations.push(`<p style="margin-top: 16px;"><strong>💡 คำแนะนำทั่วไป:</strong> ติดตามข้อมูลสุขภาพจากแหล่งที่เชื่อถือได้ เช่น กระทรวงสาธารณสุข โรงพยาบาลชั้นนำ และองค์กรสุขภาพระดับสากล ระมัดระวังข้อมูลที่ไม่มีแหล่งที่มา และตรวจสอบข้อมูลจากหลายแหล่งก่อนนำไปใช้</p>`);

      return recommendations.join('');
    }

    function getNSDimensionAdvice(dimension) {
      const adviceMap = {
        'Digital Literacy': '<strong>💻 Digital Literacy:</strong> ฝึกฝนการใช้อุปกรณ์ดิจิทัลและแอปพลิเคชันสุขภาพเพิ่มเติม',
        'Information Literacy': '<strong>🔍 Information Literacy:</strong> เรียนรู้วิธีค้นหาและประเมินแหล่งข้อมูลที่เชื่อถือได้',
        'Health Literacy': '<strong>🏥 Health Literacy:</strong> ศึกษาคำศัพท์ทางการแพทย์และทำความเข้าใจข้อมูลสุขภาพให้มากขึ้น',
        'Communication Literacy': '<strong>💬 Communication Literacy:</strong> พัฒนาทักษะการสื่อสารเรื่องสุขภาพอย่างเหมาะสม',
        'Critical Thinking': '<strong>🧠 Critical Thinking:</strong> ฝึกการคิดวิเคราะห์และตั้งคำถามกับข้อมูลที่พบ',
        'Privacy & Security': '<strong>🔒 Privacy & Security:</strong> เรียนรู้เรื่องความปลอดภัยและการปกป้องข้อมูลส่วนตัว'
      };
      return adviceMap[dimension] || dimension;
    }

    function getODDimensionAdvice(dimension) {
      const adviceMap = {
        'Functional': '<strong>⚙️ Functional:</strong> พัฒนาทักษะพื้นฐานในการเข้าถึงและใช้งานข้อมูลสุขภาพดิจิทัล',
        'Interactive': '<strong>🤝 Interactive:</strong> ฝึกฝนการโต้ตอบและใช้ข้อมูลสุขภาพในชีวิตประจำวัน',
        'Critical': '<strong>🎯 Critical:</strong> เสริมสร้างทักษะการวิเคราะห์และประเมินข้อมูลอย่างมีวิจารณญาณ'
      };
      return adviceMap[dimension] || dimension;
    }

    // Initialize
    init();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a887609c181ce9b',t:'MTc2NDgyMTUwMC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
