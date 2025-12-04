<!doctype html>
<html lang="th">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Student E-Health Profile Dashboard</title>
  <script src="/_sdk/data_sdk.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&amp;display=swap" rel="stylesheet">
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Sarabun', 'Noto Sans Thai', -apple-system, sans-serif;
    }
    
    .fade-in {
      animation: fadeIn 0.6s ease-in;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(30px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .progress-bar {
      transition: width 0.4s ease-in-out;
    }
    
    .radio-custom {
      appearance: none;
      width: 22px;
      height: 22px;
      border: 2.5px solid #cbd5e1;
      border-radius: 50%;
      cursor: pointer;
      position: relative;
      transition: all 0.2s;
    }
    
    .radio-custom:checked {
      border-color: #3b82f6;
      background: #3b82f6;
    }
    
    .radio-custom:checked::after {
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
    
    .question-card {
      transition: all 0.3s ease;
    }
    
    .question-card:hover {
      box-shadow: 0 12px 24px rgba(0,0,0,0.12);
      transform: translateY(-2px);
    }
    
    .loading-spinner {
      border: 3px solid #f3f4f6;
      border-top: 3px solid #3b82f6;
      border-radius: 50%;
      width: 28px;
      height: 28px;
      animation: spin 0.8s linear infinite;
    }
    
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    
    .toast {
      position: fixed;
      top: 24px;
      right: 24px;
      padding: 18px 28px;
      border-radius: 12px;
      background: white;
      box-shadow: 0 10px 30px rgba(0,0,0,0.25);
      z-index: 1000;
      animation: slideIn 0.4s ease-out;
      border-left: 5px solid;
    }
    
    @keyframes slideIn {
      from { transform: translateX(400px); opacity: 0; }
      to { transform: translateX(0); opacity: 1; }
    }
    
    .result-card {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      border-radius: 20px;
      padding: 40px;
      margin-top: 24px;
    }
    
    .score-circle {
      width: 160px;
      height: 160px;
      border-radius: 50%;
      background: white;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 52px;
      font-weight: bold;
      box-shadow: 0 8px 20px rgba(0,0,0,0.25);
    }

    .recommendation-box {
      background: rgba(255,255,255,0.18);
      border: 2px solid rgba(255,255,255,0.35);
      border-radius: 16px;
      padding: 24px;
      margin-top: 28px;
      backdrop-filter: blur(12px);
    }

    .risk-badge {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      padding: 10px 20px;
      border-radius: 25px;
      font-weight: 700;
      font-size: 16px;
      animation: pulse 2s ease-in-out infinite;
    }

    @keyframes pulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.85; transform: scale(0.98); }
    }

    .category-badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 6px 14px;
      border-radius: 8px;
      font-size: 13px;
      font-weight: 600;
    }

    .score-bar-container {
      background: #e2e8f0;
      height: 12px;
      border-radius: 6px;
      overflow: hidden;
      position: relative;
    }

    .score-bar {
      height: 100%;
      border-radius: 6px;
      transition: width 0.8s ease-out;
      background: linear-gradient(90deg, #10b981, #34d399);
    }

    .dashboard-card {
      background: white;
      border-radius: 16px;
      padding: 28px;
      box-shadow: 0 4px 16px rgba(0,0,0,0.08);
      transition: all 0.3s ease;
    }

    .dashboard-card:hover {
      box-shadow: 0 8px 24px rgba(0,0,0,0.12);
      transform: translateY(-4px);
    }

    .stat-icon {
      width: 64px;
      height: 64px;
      border-radius: 16px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 32px;
    }

    .portfolio-section {
      background: linear-gradient(135deg, #f0f9ff 0%, #e0f2fe 100%);
      border-radius: 16px;
      padding: 32px;
      margin-top: 24px;
      border: 2px solid #bae6fd;
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
 </head>
 <body>
  <div id="app" style="width: 100%; min-height: 100%; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);"><!-- Assessment View -->
   <div id="assessment-view" style="display: block;">
    <header style="background: white; padding: 28px; box-shadow: 0 4px 12px rgba(0,0,0,0.12);">
     <div style="max-width: 1000px; margin: 0 auto;">
      <h1 id="system-title" style="font-size: 36px; font-weight: bold; color: #1e293b; margin: 0; line-height: 1.2;">Student E-Health Profile Dashboard</h1>
      <p id="institution-name" style="color: #64748b; margin-top: 10px; font-size: 18px; font-weight: 500;">"แฟ้มสะสม (Portfolio) สุขภาพดิจิทัลส่วนตัวแบบอัตโนมัติ"</p>
     </div>
    </header>
    <main style="max-width: 1000px; margin: 0 auto; padding: 40px 24px;"><!-- Welcome Card -->
     <div class="fade-in" style="background: white; padding: 40px; border-radius: 20px; box-shadow: 0 8px 24px rgba(0,0,0,0.12); margin-bottom: 32px;">
      <div style="text-align: center; margin-bottom: 28px;">
       <div style="font-size: 72px; margin-bottom: 20px;">
        🏥
       </div>
       <h2 id="welcome-message" style="font-size: 28px; font-weight: bold; color: #1e293b; margin: 0 0 14px 0; line-height: 1.3;">ประเมินศักยภาพสุขภาพดิจิทัลของคุณ</h2>
       <p style="color: #64748b; font-size: 17px; margin: 0 0 8px 0;">แบบประเมิ��� 10 ข้อ 6 ด้านตามกรอบ Norman &amp; Skinner</p>
       <div style="display: flex; gap: 12px; justify-content: center; flex-wrap: wrap; margin-top: 16px;"><span class="category-badge" style="background: #dbeafe; color: #1e40af;">💻 Digital Literacy</span> <span class="category-badge" style="background: #fef3c7; color: #92400e;">🔍 Information Literacy</span> <span class="category-badge" style="background: #dcfce7; color: #166534;">🏥 Health Literacy</span> <span class="category-badge" style="background: #fce7f3; color: #9f1239;">💬 Communication</span> <span class="category-badge" style="background: #f3e8ff; color: #6b21a8;">🧠 Critical Thinking</span> <span class="category-badge" style="background: #ffedd5; color: #9a3412;">🔒 Privacy &amp; Security</span>
       </div>
      </div><!-- Progress Bar -->
      <div style="margin: 28px 0;">
       <div style="display: flex; justify-content: space-between; margin-bottom: 10px;"><span style="font-size: 15px; color: #64748b; font-weight: 600;">ความคืบหน้าการทำแบบประเมิน</span> <span id="progress-text" style="font-size: 15px; font-weight: 700; color: #3b82f6;">0%</span>
       </div>
       <div style="background: #e2e8f0; height: 14px; border-radius: 7px; overflow: hidden;">
        <div id="progress-bar" class="progress-bar" style="background: linear-gradient(90deg, #3b82f6, #8b5cf6); height: 100%; width: 0%; border-radius: 7px;"></div>
       </div>
      </div>
     </div><!-- Questions Section -->
     <form id="student-form">
      <div id="questions-container"></div><!-- Submit Button -->
      <div style="background: white; padding: 32px; border-radius: 20px; box-shadow: 0 8px 24px rgba(0,0,0,0.12); text-align: center;"><button type="submit" id="submit-btn" style="background: linear-gradient(135deg, #3b82f6, #8b5cf6); color: white; padding: 18px 56px; border: none; border-radius: 14px; font-size: 20px; font-weight: 700; cursor: pointer; transition: all 0.3s; box-shadow: 0 4px 12px rgba(59,130,246,0.3);" onmouseover="this.style.transform='scale(1.05)'; this.style.boxShadow='0 10px 30px rgba(59,130,246,0.5)'" onmouseout="this.style.transform='scale(1)'; this.style.boxShadow='0 4px 12px rgba(59,130,246,0.3)'"> ส่งแบบป��ะเมิน 🚀 </button>
      </div>
     </form>
    </main>
   </div><!-- Dashboard View -->
   <div id="dashboard-view" style="display: none;">
    <header style="background: white; padding: 28px; box-shadow: 0 4px 12px rgba(0,0,0,0.12);">
     <div style="max-width: 1400px; margin: 0 auto; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 20px;">
      <div>
       <h1 style="font-size: 36px; font-weight: bold; color: #1e293b; margin: 0; display: flex; align-items: center; gap: 12px;"><span style="font-size: 40px;">📊</span> Dashboard ผู้บริหาร/ครู</h1>
       <p style="color: #64748b; margin-top: 10px; font-size: 17px; font-weight: 500;">ระบบรายงานและวิเคราะห์ผลแฟ้มสะสมสุขภาพดิจิทัล</p>
      </div><button onclick="showAssessmentView()" style="background: #3b82f6; color: white; padding: 14px 28px; border: none; border-radius: 10px; font-size: 17px; font-weight: 700; cursor: pointer; transition: all 0.2s; box-shadow: 0 2px 8px rgba(59,130,246,0.3);" onmouseover="this.style.background='#2563eb'" onmouseout="this.style.background='#3b82f6'"> ← กลับไปแบบประเมิน </button>
     </div>
    </header>
    <main style="max-width: 1400px; margin: 0 auto; padding: 40px 24px;"><!-- Summary Cards -->
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 28px; margin-bottom: 40px;">
      <div class="dashboard-card">
       <div style="display: flex; align-items: center; gap: 20px;">
        <div class="stat-icon" style="background: #dbeafe;">
         👥
        </div>
        <div>
         <p style="color: #64748b; font-size: 15px; margin: 0; font-weight: 600;">จำนวนนักเรียนทั้งหม��</p>
         <p id="dash-total" style="font-size: 40px; font-weight: bold; color: #1e293b; margin: 10px 0 0 0;">0</p>
        </div>
       </div>
      </div>
      <div class="dashboard-card">
       <div style="display: flex; align-items: center; gap: 20px;">
        <div class="stat-icon" style="background: #dcfce7;">
         ✅
        </div>
        <div>
         <p style="color: #64748b; font-size: 15px; margin: 0; font-weight: 600;">ระดับดี</p>
         <p id="dash-good" style="font-size: 40px; font-weight: bold; color: #16a34a; margin: 10px 0 0 0;">0</p>
        </div>
       </div>
      </div>
      <div class="dashboard-card">
       <div style="display: flex; align-items: center; gap: 20px;">
        <div class="stat-icon" style="background: #fef3c7;">
         ⚠️
        </div>
        <div>
         <p style="color: #64748b; font-size: 15px; margin: 0; font-weight: 600;">ควรพัฒนา</p>
         <p id="dash-medium" style="font-size: 40px; font-weight: bold; color: #ea580c; margin: 10px 0 0 0;">0</p>
        </div>
       </div>
      </div>
      <div class="dashboard-card">
       <div style="display: flex; align-items: center; gap: 20px;">
        <div class="stat-icon" style="background: #fee2e2;">
         🚨
        </div>
        <div>
         <p style="color: #64748b; font-size: 15px; margin: 0; font-weight: 600;">กลุ่มเสี่ยง</p>
         <p id="dash-risk" style="font-size: 40px; font-weight: bold; color: #dc2626; margin: 10px 0 0 0;">0</p>
        </div>
       </div>
      </div>
     </div><!-- Student List -->
     <div class="dashboard-card">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 28px; flex-wrap: wrap; gap: 20px;">
       <h2 style="font-size: 26px; font-weight: bold; color: #1e293b; margin: 0; display: flex; align-items: center; gap: 10px;"><span style="font-size: 32px;">📋</span> รายชื่อนักเรียนและผลประเมิน</h2>
       <div style="display: flex; gap: 14px; align-items: center;"><label for="dash-filter" style="color: #475569; font-weight: 600; font-size: 15px;">กรองตามระดับ:</label> <select id="dash-filter" style="padding: 10px 18px; border: 2.5px solid #e2e8f0; border-radius: 10px; font-size: 15px; font-weight: 600; cursor: pointer; font-family: 'Sarabun', sans-serif;"> <option value="">ทั้งหมด</option> <option value="ดี">ระดับดี</option> <option value="ควรพัฒนา">ควรพัฒนา</option> <option value="เสี่ยง">กลุ่มเสี่ยง</option> </select>
       </div>
      </div>
      <div id="dashboard-list"></div>
     </div>
    </main>
   </div>
   <footer style="background: rgba(255,255,255,0.12); color: white; text-align: center; padding: 28px; margin-top: 60px; backdrop-filter: blur(10px);">
    <p id="footer-text" style="margin: 0; font-size: 15px; font-weight: 500;">E-Health Literacy Assessment System - พัฒนาตามกรอบ Norman &amp; Skinner</p>
   </footer>
  </div>
  <script>
    const defaultConfig = {
      system_title: "Student E-Health Profile Dashboard",
      institution_name: '"แฟ้มสะสม (Portfolio) สุขภาพดิจิทัลส่วนตัวแบบอัตโนมัติ"',
      welcome_message: "ประเมินศักยภาพสุขภาพดิจิทัลของคุณ",
      good_recommendation: "🎉 ยอดเยี่ย���! คุณมีทักษะ E-Health Literacy ที่ดีมาก สามารถค้นหา ประเมิน และใช้ข้อมูลสุขภาพดิจิทัลได้��ย่างมีประสิทธิภาพ แนะนำ��ห้ช่วยเหลือเพื่อนที่ต้องการพัฒนาทักษะ และใช้ความรู้นี้ในการดูแลสุขภาพของตัวเองและครอบครัว",
      medium_recommendation: "💪 ดีมาก! คุณมีพื้นฐานที่ดี แต่ยังมีโอกาสพัฒนาเพิ่มเติม แนะนำให้ฝึกฝนทักษะการประเมินความน่าเชื่อถือของแหล่งข้อมูลสุขภาพ และการคิดวิเคราะห์อย่างมีวิจารณญาณ ลองค้นคว้าข้อมูลสุขภาพจากแหล่งที่น่าเชื่อถือเพิ่มเติม เช่น เว็บไซต์กระทรวงสาธารณสุข หรือสถาบันการแพทย์ชั้นนำ",
      risk_recommendation: "⚠️ ควรให้ความสำคัญ! คุณอาจต้องการความช่วยเหลือในการพัฒนาทักษะ E-Health Literacy แนะนำให้ปรึกษาครู ผู้ปกครอง หรือเข้าร่วมโปรแกรมพัฒนาทักษะเพื่อเพิ่มความรู้และความมั่นใจในการใช้ข้อมูลสุขภาพดิจิทัล เริ่มต้นจากการเรียนรู้เครื่องมือค้นหาข้อมูลสุขภาพที่เชื่อถือได้ และฝึกการตั้งคำถามเมื่อพบข้อมูลที่ไม่แน่ใจ",
      footer_text: "E-Health Literacy Assessment System - พัฒนาตามกรอบ Norman & Skinner",
      background_color: "#667eea",
      card_color: "#ffffff",
      text_color: "#1e293b",
      primary_button_color: "#3b82f6",
      accent_color: "#8b5cf6"
    };

    const questions = [
      { 
        category: "Digital Literacy", 
        emoji: "💻",
        question: "ฉันสามารถใช้อุปกรณ์ดิจิทัล (สมาร์ทโฟน/แท็บเล็ต/คอมพิวเตอร์) ในการค้นหาข้อมูลสุขภาพได้อย่างมั่นใจ",
        categoryName: "digital_literacy_score",
        categoryColor: "#1e40af",
        categoryBg: "#dbeafe"
      },
      { 
        category: "Digital Literacy", 
        emoji: "📱",
        question: "ฉันรู้วิธีดาวน์โหลดและใช้งานแอปพลิเคชันสุขภาพบนสมาร์ทโฟนได้",
        categoryName: "digital_literacy_score",
        categoryColor: "#1e40af",
        categoryBg: "#dbeafe"
      },
      { 
        category: "Information Literacy", 
        emoji: "🔍",
        question: "ฉันสาม���รถค้นหาข้อมูลสุขภาพที่น่าเชื่อถือจากอินเทอร์เน็ตได้",
        categoryName: "information_literacy_score",
        categoryColor: "#92400e",
        categoryBg: "#fef3c7"
      },
      { 
        category: "Information Literacy", 
        emoji: "✅",
        question: "ฉันรู้วิธีประเมินความน่าเชื่อถือของเว็บไซต์ข้อมูลสุขภาพ",
        categoryName: "information_literacy_score",
        categoryColor: "#92400e",
        categoryBg: "#fef3c7"
      },
      { 
        category: "Health Literacy", 
        emoji: "🏥",
        question: "ฉันเข้าใจคำศัพท์ทางการแพทย์และสุขภาพที่พบในเว็บ���ซต์หรือแอปพลิเคชัน",
        categoryName: "health_literacy_score",
        categoryColor: "#166534",
        categoryBg: "#dcfce7"
      },
      { 
        category: "Communication Literacy", 
        emoji: "💬",
        question: "ฉันสามารถสื่อสารเรื่องสุขภาพกับผู้อื่นผ่านช่องทางออนไลน์ได้อย่างเหมาะสม",
        categoryName: "communication_literacy_score",
        categoryColor: "#9f1239",
        categoryBg: "#fce7f3"
      },
      { 
        category: "Communication Literacy", 
        emoji: "📤",
        question: "ฉันระมัดระวังในการแชร์ข้อมูลสุขภาพส่วนตัวบนโซเชียลมีเดีย",
        categoryName: "communication_literacy_score",
        categoryColor: "#9f1239",
        categoryBg: "#fce7f3"
      },
      { 
        category: "Critical Thinking", 
        emoji: "🧠",
        question: "ฉันตั้งคำถาม��ละไตร่ตรองก่อนเชื่อข้อมูลสุขภาพที่พบออนไลน์",
        categoryName: "critical_thinking_score",
        categoryColor: "#6b21a8",
        categoryBg: "#f3e8ff"
      },
      { 
        category: "Critical Thinking", 
        emoji: "⚖️",
        question: "ฉันเปรียบเทียบข้อมูลจากหลายแหล่งก่อนตัดสินใจเรื่องสุขภาพ",
        categoryName: "critical_thinking_score",
        categoryColor: "#6b21a8",
        categoryBg: "#f3e8ff"
      },
      { 
        category: "Privacy & Security", 
        emoji: "🔒",
        question: "ฉันเข้าใจและระวังเรื่องความปลอดภัยของข้อมูลส่วนตัวเมื่อใช้บริการสุขภาพออนไลน์",
        categoryName: "privacy_security_score",
        categoryColor: "#9a3412",
        categoryBg: "#ffedd5"
      }
    ];

    let assessments = [];
    let currentView = 'assessment';

    const dataHandler = {
      onDataChanged(data) {
        assessments = data;
        if (currentView === 'dashboard') {
          updateDashboard();
        }
      }
    };

    async function onConfigChange(config) {
      document.getElementById('system-title').textContent = config.system_title || defaultConfig.system_title;
      document.getElementById('institution-name').textContent = config.institution_name || defaultConfig.institution_name;
      document.getElementById('welcome-message').textContent = config.welcome_message || defaultConfig.welcome_message;
      document.getElementById('footer-text').textContent = config.footer_text || defaultConfig.footer_text;
      
      const bgColor = config.background_color || defaultConfig.background_color;
      const primaryColor = config.primary_button_color || defaultConfig.primary_button_color;
      
      document.getElementById('app').style.background = `linear-gradient(135deg, ${bgColor} 0%, ${bgColor}dd 100%)`;
      
      const submitBtn = document.getElementById('submit-btn');
      if (submitBtn) {
        submitBtn.style.background = `linear-gradient(135deg, ${primaryColor}, ${config.accent_color || defaultConfig.accent_color})`;
      }
    }

    async function init() {
      const initResult = await window.dataSdk.init(dataHandler);
      if (!initResult.isOk) {
        showToast('❌ ไม่สามารถเชื่อมต่อระบบได้', 'error');
        return;
      }

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
                get: () => config.primary_button_color || defaultConfig.primary_button_color,
                set: (value) => {
                  window.elementSdk.config.primary_button_color = value;
                  window.elementSdk.setConfig({ primary_button_color: value });
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
            ["system_title", config.system_title || defaultConfig.system_title],
            ["institution_name", config.institution_name || defaultConfig.institution_name],
            ["welcome_message", config.welcome_message || defaultConfig.welcome_message],
            ["good_recommendation", config.good_recommendation || defaultConfig.good_recommendation],
            ["medium_recommendation", config.medium_recommendation || defaultConfig.medium_recommendation],
            ["risk_recommendation", config.risk_recommendation || defaultConfig.risk_recommendation],
            ["footer_text", config.footer_text || defaultConfig.footer_text]
          ])
        });
      }

      renderQuestions();
      setupEventListeners();
      
      const urlParams = new URLSearchParams(window.location.hash.substring(1));
      if (urlParams.get('view') === 'dashboard') {
        showDashboardView();
      }
    }

    function renderQuestions() {
      const container = document.getElementById('questions-container');
      
      container.innerHTML = questions.map((q, index) => {
        const questionNum = index + 1;
        return `
          <div class="fade-in question-card" style="background: white; padding: 36px; border-radius: 20px; box-shadow: 0 8px 24px rgba(0,0,0,0.12); margin-bottom: 32px;">
            <div style="display: flex; align-items: center; gap: 14px; margin-bottom: 20px;">
              <span style="font-size: 36px;">${q.emoji}</span>
              <span class="category-badge" style="background: ${q.categoryBg}; color: ${q.categoryColor};">${q.category}</span>
            </div>
            
            <label style="display: block; color: #1e293b; font-weight: 600; margin-bottom: 20px; font-size: 17px; line-height: 1.7;">
              <span style="background: linear-gradient(135deg, #3b82f6, #8b5cf6); color: white; padding: 6px 14px; border-radius: 8px; font-weight: 700; margin-right: 12px; display: inline-block;">${questionNum}</span>
              ${q.question}
            </label>
            
            <div style="display: grid; gap: 14px;">
              ${[
                { value: 5, label: 'เห็นด้วยอย่างยิ่ง', color: '#10b981', bg: '#d1fae5' },
                { value: 4, label: 'เห็นด้วย', color: '#84cc16', bg: '#ecfccb' },
                { value: 3, label: 'ไม่แน่ใจ', color: '#f59e0b', bg: '#fef3c7' },
                { value: 2, label: 'ไม่เห็นด้วย', color: '#f97316', bg: '#ffedd5' },
                { value: 1, label: 'ไม่เห็นด้วยอย่างยิ่ง', color: '#ef4444', bg: '#fee2e2' }
              ].map(option => `
                <label style="display: flex; align-items: center; gap: 14px; cursor: pointer; padding: 16px 20px; border: 2.5px solid #e2e8f0; border-radius: 12px; transition: all 0.2s; background: white;" onmouseover="this.style.borderColor='${option.color}'; this.style.background='${option.bg}'; this.style.transform='translateX(4px)'" onmouseout="if(!this.querySelector('input').checked) { this.style.borderColor='#e2e8f0'; this.style.background='white'; this.style.transform='translateX(0)' }">
                  <input type="radio" name="q${questionNum}" value="${option.value}" class="radio-custom" required onchange="updateProgress(); this.parentElement.style.borderColor='${option.color}'; this.parentElement.style.background='${option.bg}'">
                  <span style="font-size: 16px; color: #1e293b; font-weight: 600;">${option.label}</span>
                </label>
              `).join('')}
            </div>
          </div>
        `;
      }).join('');
    }

    function setupEventListeners() {
      const form = document.getElementById('student-form');
      form.addEventListener('submit', async (e) => {
        e.preventDefault();
        
        if (assessments.length >= 999) {
          showToast('⚠️ ถึงขีดจำกัด 999 รายการ กรุณาลบข้อมูลเก่าก่อน', 'error');
          return;
        }

        const submitBtn = document.getElementById('submit-btn');
        const originalText = submitBtn.innerHTML;
        submitBtn.innerHTML = '<div class="loading-spinner" style="margin: 0 auto;"></div>';
        submitBtn.disabled = true;

        const scores = {};
        for (let i = 1; i <= 10; i++) {
          const value = document.querySelector(`input[name="q${i}"]:checked`).value;
          scores[`q${i}_score`] = parseInt(value);
        }

        const categoryScores = {
          digital_literacy_score: ((scores.q1_score + scores.q2_score) / 2) * 20,
          information_literacy_score: ((scores.q3_score + scores.q4_score) / 2) * 20,
          health_literacy_score: scores.q5_score * 20,
          communication_literacy_score: ((scores.q6_score + scores.q7_score) / 2) * 20,
          critical_thinking_score: ((scores.q8_score + scores.q9_score) / 2) * 20,
          privacy_security_score: scores.q10_score * 20
        };

        const overall_score = (categoryScores.digital_literacy_score + 
                              categoryScores.information_literacy_score + 
                              categoryScores.health_literacy_score + 
                              categoryScores.communication_literacy_score + 
                              categoryScores.critical_thinking_score + 
                              categoryScores.privacy_security_score) / 6;

        let risk_level = 'ดี';
        if (overall_score < 50) {
          risk_level = 'เส��่ยง';
        } else if (overall_score < 70) {
          risk_level = 'ควรพัฒนา';
        }

        const assessmentData = {
          student_id: 'USER-' + Date.now(),
          class_level: '-',
          assessment_date: new Date().toISOString(),
          ...scores,
          ...categoryScores,
          overall_score: Math.round(overall_score * 10) / 10,
          risk_level: risk_level
        };

        const result = await window.dataSdk.create(assessmentData);
        
        submitBtn.innerHTML = originalText;
        submitBtn.disabled = false;

        if (result.isOk) {
          showResultModal(assessmentData);
          form.reset();
          updateProgress();
        } else {
          showToast('❌ เกิดข้อผิดพลาดในการบันทึก', 'error');
        }
      });

      document.getElementById('dash-filter').addEventListener('change', updateDashboard);
    }

    function updateProgress() {
      const totalQuestions = 10;
      let answered = 0;
      
      for (let i = 1; i <= totalQuestions; i++) {
        if (document.querySelector(`input[name="q${i}"]:checked`)) {
          answered++;
        }
      }
      
      const percentage = (answered / totalQuestions) * 100;
      document.getElementById('progress-bar').style.width = percentage + '%';
      document.getElementById('progress-text').textContent = Math.round(percentage) + '%';
    }

    function getRecommendation(riskLevel) {
      const config = window.elementSdk ? window.elementSdk.config : defaultConfig;
      
      if (riskLevel === 'ดี') {
        return config.good_recommendation || defaultConfig.good_recommendation;
      } else if (riskLevel === 'ควรพัฒนา') {
        return config.medium_recommendation || defaultConfig.medium_recommendation;
      } else {
        return config.risk_recommendation || defaultConfig.risk_recommendation;
      }
    }

    function showResultModal(data) {
      const modal = document.createElement('div');
      modal.style.cssText = 'position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.75); display: flex; align-items: center; justify-content: center; z-index: 2000; padding: 20px; overflow-y: auto;';
      
      const riskColor = data.risk_level === 'ดี' ? '#10b981' : data.risk_level === 'ควรพัฒนา' ? '#ea580c' : '#dc2626';
      const riskIcon = data.risk_level === 'ดี' ? '✅' : data.risk_level === 'ควรพัฒนา' ? '⚠️' : '🚨';
      const recommendation = getRecommendation(data.risk_level);
      
      modal.innerHTML = `
        <div class="fade-in" style="background: white; border-radius: 24px; max-width: 800px; width: 100%; max-height: 90%; overflow-y: auto; box-shadow: 0 25px 70px rgba(0,0,0,0.35);">
          <div class="result-card">
            <div style="text-align: center;">
              <div style="font-size: 72px; margin-bottom: 20px;">🎉</div>
              <h2 style="font-size: 32px; font-weight: bold; margin: 0 0 12px 0;">ส่งแบบประเมินสำเร็จ!</h2>
              <p style="opacity: 0.95; margin: 0; font-size: 20px; font-weight: 600;">รหัส: ${data.student_id} | ���ั้น: ${data.class_level}</p>
              <p style="opacity: 0.9; margin: 8px 0 0 0; font-size: 16px;">วันที่: ${new Date(data.assessment_date).toLocaleDateString('th-TH', { year: 'numeric', month: 'long', day: 'numeric' })}</p>
            </div>
            
            <div style="display: flex; justify-content: center; margin: 36px 0;">
              <div class="score-circle" style="color: ${riskColor};">
                ${Math.round(data.overall_score)}
              </div>
            </div>
            
            <div style="text-align: center; margin-bottom: 28px;">
              <div class="risk-badge" style="background: white; color: ${riskColor}; display: inline-flex;">
                <span style="font-size: 28px;">${riskIcon}</span>
                <span style="font-size: 22px;">${data.risk_level}</span>
              </div>
            </div>

            <!-- Recommendation Box -->
            <div class="recommendation-box">
              <div style="display: flex; align-items: start; gap: 14px;">
                <span style="font-size: 32px;">💡</span>
                <div>
                  <h3 style="font-size: 20px; font-weight: bold; margin: 0 0 14px 0;">คำแนะนำสำหรับคุณ</h3>
                  <p style="margin: 0; line-height: 1.8; font-size: 16px; opacity: 0.95;">${recommendation}</p>
                </div>
              </div>
            </div>
          </div>
          
          <div style="padding: 40px;">
            <!-- Portfolio Section -->
            <div class="portfolio-section">
              <h3 style="font-size: 24px; font-weight: bold; color: #0c4a6e; margin: 0 0 20px 0; display: flex; align-items: center; gap: 10px;">
                <span style="font-size: 32px;">📊</span>
                แฟ้มสะสมผลการประเมิน 6 ด้าน
              </h3>
              <p style="color: #0369a1; margin: 0 0 24px 0; font-size: 15px; font-weight: 500;">ตามกรอบ Norman & Skinner's E-Health Literacy Framework</p>
              
              <div style="display: grid; gap: 20px;">
                ${createScoreDetail('💻 Digital Literacy', 'ทักษะการใช้เทคโนโลยีดิจิทัล', data.digital_literacy_score)}
                ${createScoreDetail('🔍 Information Literacy', 'ทักษะการค้นหาข้อมูล', data.information_literacy_score)}
                ${createScoreDetail('🏥 Health Literacy', 'ความรอบรู้ด้านสุขภาพ', data.health_literacy_score)}
                ${createScoreDetail('💬 Communication Literacy', 'ทักษะการสื่อสาร', data.communication_literacy_score)}
                ${createScoreDetail('🧠 Critical Thinking', 'การคิดวิเคราะห์', data.critical_thinking_score)}
                ${createScoreDetail('🔒 Privacy & Security', 'ความปลอดภัยข้อมูล', data.privacy_security_score)}
              </div>
            </div>
            
            <div style="margin-top: 36px; display: flex; gap: 14px; flex-wrap: wrap;">
              <button onclick="this.closest('div[style*=fixed]').remove(); showDashboardView()" style="flex: 1; min-width: 220px; background: #3b82f6; color: white; padding: 16px; border: none; border-radius: 12px; font-size: 17px; font-weight: 700; cursor: pointer; transition: all 0.2s;" onmouseover="this.style.background='#2563eb'; this.style.transform='translateY(-2px)'" onmouseout="this.style.background='#3b82f6'; this.style.transform='translateY(0)'">
                📊 ดู Dashboard
              </button>
              <button onclick="this.closest('div[style*=fixed]').remove()" style="flex: 1; min-width: 220px; background: #64748b; color: white; padding: 16px; border: none; border-radius: 12px; font-size: 17px; font-weight: 700; cursor: pointer; transition: all 0.2s;" onmouseover="this.style.background='#475569'; this.style.transform='translateY(-2px)'" onmouseout="this.style.background='#64748b'; this.style.transform='translateY(0)'">
                ปิด
              </button>
            </div>
          </div>
        </div>
      `;
      
      document.body.appendChild(modal);
      modal.addEventListener('click', (e) => {
        if (e.target === modal) modal.remove();
      });
    }

    function createScoreDetail(label, description, score) {
      const color = score >= 70 ? '#10b981' : score >= 50 ? '#ea580c' : '#dc2626';
      const bgColor = score >= 70 ? '#d1fae5' : score >= 50 ? '#ffedd5' : '#fee2e2';
      const level = score >= 70 ? 'ดีมาก' : score >= 50 ? 'ปานกลาง' : 'ควรพัฒนา';
      
      return `
        <div style="background: white; padding: 20px; border-radius: 12px; border: 2px solid #e0f2fe;">
          <div style="display: flex; justify-content: space-between; align-items: start; margin-bottom: 10px;">
            <div>
              <span style="font-size: 16px; color: #1e293b; font-weight: 700; display: block; margin-bottom: 4px;">${label}</span>
              <span style="font-size: 13px; color: #64748b; font-weight: 500;">${description}</span>
            </div>
            <div style="text-align: right;">
              <span style="font-size: 24px; font-weight: 800; color: ${color}; display: block;">${Math.round(score)}</span>
              <span style="font-size: 12px; font-weight: 600; color: ${color}; background: ${bgColor}; padding: 4px 10px; border-radius: 6px; display: inline-block; margin-top: 4px;">${level}</span>
            </div>
          </div>
          <div class="score-bar-container">
            <div class="score-bar" style="width: ${score}%; background: ${color};"></div>
          </div>
        </div>
      `;
    }

    function showDashboardView() {
      currentView = 'dashboard';
      document.getElementById('assessment-view').style.display = 'none';
      document.getElementById('dashboard-view').style.display = 'block';
      window.location.hash = 'view=dashboard';
      updateDashboard();
    }

    function showAssessmentView() {
      currentView = 'assessment';
      document.getElementById('assessment-view').style.display = 'block';
      document.getElementById('dashboard-view').style.display = 'none';
      window.location.hash = '';
    }

    function updateDashboard() {
      const filter = document.getElementById('dash-filter').value;
      const filtered = filter ? assessments.filter(a => a.risk_level === filter) : assessments;
      
      document.getElementById('dash-total').textContent = assessments.length;
      document.getElementById('dash-good').textContent = assessments.filter(a => a.risk_level === 'ดี').length;
      document.getElementById('dash-medium').textContent = assessments.filter(a => a.risk_level === 'ควรพัฒนา').length;
      document.getElementById('dash-risk').textContent = assessments.filter(a => a.risk_level === 'เสี่ยง').length;
      
      const container = document.getElementById('dashboard-list');
      
      if (filtered.length === 0) {
        container.innerHTML = `
          <div style="text-align: center; padding: 80px 20px; color: #94a3b8;">
            <div style="font-size: 56px; margin-bottom: 20px;">📋</div>
            <p style="font-size: 20px; margin: 0; font-weight: 600;">ยังไม่มีข้อมูลการประเมิน</p>
            <p style="font-size: 16px; margin: 12px 0 0 0;">เริ่มต้นทำแบบประเมิน���พื่อสร้างแ���้มสะสมสุขภาพดิจิทัล</p>
          </div>
        `;
        return;
      }
      
      container.innerHTML = `
        <div style="overflow-x: auto;">
          <table style="width: 100%; border-collapse: collapse;">
            <thead>
              <tr style="background: linear-gradient(135deg, #f8fafc, #f1f5f9); border-bottom: 3px solid #cbd5e1;">
                <th style="padding: 18px; text-align: left; font-weight: 700; color: #1e293b; font-size: 15px;">รหัสนักเรียน</th>
                <th style="padding: 18px; text-align: left; font-weight: 700; color: #1e293b; font-size: 15px;">ระดับชั้น</th>
                <th style="padding: 18px; text-align: center; font-weight: 700; color: #1e293b; font-size: 15px;">คะแนนรวม</th>
                <th style="padding: 18px; text-align: center; font-weight: 700; color: #1e293b; font-size: 15px;">ร��ดับ</th>
                <th style="padding: 18px; text-align: center; font-weight: 700; color: #1e293b; font-size: 15px;">วันที่ประเมิน</th>
                <th style="padding: 18px; text-align: center; font-weight: 700; color: #1e293b; font-size: 15px;">จัดการ</th>
              </tr>
            </thead>
            <tbody>
              ${filtered.map(student => {
                const riskColor = student.risk_level === 'ดี' ? '#10b981' : student.risk_level === 'ควรพัฒนา' ? '#ea580c' : '#dc2626';
                const riskBg = student.risk_level === 'ดี' ? '#d1fae5' : student.risk_level === 'ควรพัฒนา' ? '#ffedd5' : '#fee2e2';
                const riskIcon = student.risk_level === 'ดี' ? '✅' : student.risk_level === 'ควรพัฒนา' ? '⚠️' : '🚨';
                
                return `
                  <tr style="border-bottom: 1px solid #e2e8f0; transition: all 0.2s;" onmouseover="this.style.background='#f8fafc'" onmouseout="this.style.background='white'">
                    <td style="padding: 18px; color: #1e293b; font-weight: 600; font-size: 15px;">${student.student_id}</td>
                    <td style="padding: 18px; color: #475569; font-weight: 600; font-size: 15px;">${student.class_level}</td>
                    <td style="padding: 18px; text-align: center; font-weight: 800; color: ${riskColor}; font-size: 24px;">${Math.round(student.overall_score)}</td>
                    <td style="padding: 18px; text-align: center;">
                      <span style="background: ${riskBg}; color: ${riskColor}; padding: 8px 16px; border-radius: 14px; font-size: 14px; font-weight: 700; display: inline-flex; align-items: center; gap: 8px;">
                        ${riskIcon} ${student.risk_level}
                      </span>
                    </td>
                    <td style="padding: 18px; text-align: center; color: #64748b; font-size: 14px; font-weight: 500;">${new Date(student.assessment_date).toLocaleDateString('th-TH')}</td>
                    <td style="padding: 18px; text-align: center;">
                      <button onclick="viewDetail('${student.__backendId}')" style="background: #3b82f6; color: white; border: none; padding: 10px 18px; border-radius: 8px; cursor: pointer; font-size: 14px; font-weight: 600; margin-right: 8px; transition: all 0.2s;" onmouseover="this.style.background='#2563eb'; this.style.transform='scale(1.05)'" onmouseout="this.style.background='#3b82f6'; this.style.transform='scale(1)'">
                        👁️ ดู
                      </button>
                      <button onclick="deleteAssessment('${student.__backendId}')" style="background: #ef4444; color: white; border: none; padding: 10px 18px; border-radius: 8px; cursor: pointer; font-size: 14px; font-weight: 600; transition: all 0.2s;" onmouseover="this.style.background='#dc2626'; this.style.transform='scale(1.05)'" onmouseout="this.style.background='#ef4444'; this.style.transform='scale(1)'">
                        🗑️ ลบ
                      </button>
                    </td>
                  </tr>
                `;
              }).join('')}
            </tbody>
          </table>
        </div>
      `;
    }

    function viewDetail(backendId) {
      const student = assessments.find(s => s.__backendId === backendId);
      if (!student) return;
      
      showResultModal(student);
    }

    async function deleteAssessment(backendId) {
      const student = assessments.find(s => s.__backendId === backendId);
      if (!student) return;

      const confirmBtn = event.target;
      const originalText = confirmBtn.innerHTML;
      confirmBtn.innerHTML = '⏳ กำลังลบ...';
      confirmBtn.disabled = true;

      const result = await window.dataSdk.delete(student);
      
      if (result.isOk) {
        showToast('✅ ลบข้อมูลสำเร็จ', 'success');
      } else {
        showToast('❌ เกิดข้อผิดพลาด', 'error');
        confirmBtn.innerHTML = originalText;
        confirmBtn.disabled = false;
      }
    }

    function showToast(message, type) {
      const toast = document.createElement('div');
      toast.className = 'toast';
      
      if (type === 'success') {
        toast.style.background = '#d1fae5';
        toast.style.color = '#065f46';
        toast.style.borderLeftColor = '#10b981';
      } else {
        toast.style.background = '#fee2e2';
        toast.style.color = '#991b1b';
        toast.style.borderLeftColor = '#ef4444';
      }
      
      toast.style.fontWeight = '700';
      toast.style.fontSize = '15px';
      toast.textContent = message;
      
      document.body.appendChild(toast);
      
      setTimeout(() => {
        toast.style.animation = 'slideIn 0.3s ease-out reverse';
        setTimeout(() => toast.remove(), 300);
      }, 3500);
    }

    init();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a886c3e85e8ce9b',t:'MTc2NDgyMTA5OS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
