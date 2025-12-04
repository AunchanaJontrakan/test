<!doctype html>
<html lang="th">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Student E-Health Profile Dashboard</title>
  <script src="/_sdk/data_sdk.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Sarabun', 'Noto Sans Thai', -apple-system, sans-serif;
    }
    
    .fade-in {
      animation: fadeIn 0.5s ease-in;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .progress-bar {
      transition: width 0.3s ease-in-out;
    }
    
    .radio-custom {
      appearance: none;
      width: 20px;
      height: 20px;
      border: 2px solid #cbd5e1;
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
      width: 8px;
      height: 8px;
      background: white;
      border-radius: 50%;
    }
    
    .question-card {
      transition: all 0.3s ease;
    }
    
    .question-card:hover {
      box-shadow: 0 8px 16px rgba(0,0,0,0.1);
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
      top: 20px;
      right: 20px;
      padding: 16px 24px;
      border-radius: 12px;
      background: white;
      box-shadow: 0 8px 24px rgba(0,0,0,0.2);
      z-index: 1000;
      animation: slideIn 0.3s ease-out;
    }
    
    @keyframes slideIn {
      from { transform: translateX(400px); opacity: 0; }
      to { transform: translateX(0); opacity: 1; }
    }
    
    .result-card {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      border-radius: 16px;
      padding: 32px;
      margin-top: 24px;
    }
    
    .score-circle {
      width: 120px;
      height: 120px;
      border-radius: 50%;
      background: white;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 36px;
      font-weight: bold;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
 </head>
 <body>
  <div id="app" style="width: 100%; min-height: 100%; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);"><!-- Assessment View -->
   <div id="assessment-view" style="display: block;">
    <header style="background: white; padding: 24px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);">
     <div style="max-width: 900px; margin: 0 auto;">
      <h1 id="system-title" style="font-size: 32px; font-weight: bold; color: #1e293b; margin: 0;">Student E-Health Profile Dashboard</h1>
      <p id="institution-name" style="color: #64748b; margin-top: 8px; font-size: 16px;">แบบประเมินศักยภาพสุขภาพดิจิทัล</p>
     </div>
    </header>
    <main style="max-width: 900px; margin: 0 auto; padding: 32px 24px;"><!-- Welcome Card -->
     <div class="fade-in" style="background: white; padding: 32px; border-radius: 16px; box-shadow: 0 4px 16px rgba(0,0,0,0.1); margin-bottom: 24px;">
      <div style="text-align: center; margin-bottom: 24px;">
       <div style="font-size: 64px; margin-bottom: 16px;">
        🏥
       </div>
       <h2 id="welcome-message" style="font-size: 24px; font-weight: bold; color: #1e293b; margin: 0 0 12px 0;">ประเมินศักยภาพสุขภาพดิจิทัลของคุณ</h2>
       <p style="color: #64748b; font-size: 16px; margin: 0;">แบบประเมิน 6 ด้านตามกรอบ Norman &amp; Skinner</p>
      </div><!-- Progress Bar -->
      <div style="margin: 24px 0;">
       <div style="display: flex; justify-content: space-between; margin-bottom: 8px;"><span style="font-size: 14px; color: #64748b;">ความคืบหน้า</span> <span id="progress-text" style="font-size: 14px; font-weight: 600; color: #3b82f6;">0%</span>
       </div>
       <div style="background: #e2e8f0; height: 12px; border-radius: 6px; overflow: hidden;">
        <div id="progress-bar" class="progress-bar" style="background: linear-gradient(90deg, #3b82f6, #8b5cf6); height: 100%; width: 0%; border-radius: 6px;"></div>
       </div>
      </div>
     </div><!-- Student Info Form -->
     <form id="student-form">
      <div class="fade-in" style="background: white; padding: 32px; border-radius: 16px; box-shadow: 0 4px 16px rgba(0,0,0,0.1); margin-bottom: 24px;">
       <h3 style="font-size: 20px; font-weight: bold; color: #1e293b; margin: 0 0 20px 0;">📝 ข้อมูลนักเรียน</h3>
       <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 16px;">
        <div><label for="student-name" style="display: block; color: #475569; font-weight: 500; margin-bottom: 8px;">ชื่อ *</label> <input type="text" id="student-name" required style="width: 100%; padding: 12px; border: 2px solid #e2e8f0; border-radius: 8px; font-size: 14px; transition: border 0.2s;" onfocus="this.style.borderColor='#3b82f6'" onblur="this.style.borderColor='#e2e8f0'">
        </div>
        <div><label for="student-surname" style="display: block; color: #475569; font-weight: 500; margin-bottom: 8px;">นามสกุล *</label> <input type="text" id="student-surname" required style="width: 100%; padding: 12px; border: 2px solid #e2e8f0; border-radius: 8px; font-size: 14px; transition: border 0.2s;" onfocus="this.style.borderColor='#3b82f6'" onblur="this.style.borderColor='#e2e8f0'">
        </div>
        <div><label for="student-id" style="display: block; color: #475569; font-weight: 500; margin-bottom: 8px;">รหัสนักเรียน *</label> <input type="text" id="student-id" required style="width: 100%; padding: 12px; border: 2px solid #e2e8f0; border-radius: 8px; font-size: 14px; transition: border 0.2s;" onfocus="this.style.borderColor='#3b82f6'" onblur="this.style.borderColor='#e2e8f0'">
        </div>
        <div><label for="class-level" style="display: block; color: #475569; font-weight: 500; margin-bottom: 8px;">ระดับชั้น *</label> <select id="class-level" required style="width: 100%; padding: 12px; border: 2px solid #e2e8f0; border-radius: 8px; font-size: 14px; background: white;"> <option value="">เลือกระดับชั้น</option> <option value="ม.1">มัธยมศึกษาปีที่ 1</option> <option value="ม.2">มัธยมศึกษาปีที่ 2</option> <option value="ม.3">มัธยมศึกษาปีที่ 3</option> <option value="ม.4">มัธยมศึกษาปีที่ 4</option> <option value="ม.5">มัธยมศึกษาปีที่ 5</option> <option value="ม.6">มัธยมศึกษาปีที่ 6</option> </select>
        </div>
       </div>
      </div><!-- Questions Section -->
      <div id="questions-container"></div><!-- Submit Button -->
      <div style="background: white; padding: 24px; border-radius: 16px; box-shadow: 0 4px 16px rgba(0,0,0,0.1); text-align: center;"><button type="submit" id="submit-btn" style="background: linear-gradient(135deg, #3b82f6, #8b5cf6); color: white; padding: 16px 48px; border: none; border-radius: 12px; font-size: 18px; font-weight: 600; cursor: pointer; transition: transform 0.2s, box-shadow 0.2s;" onmouseover="this.style.transform='scale(1.05)'; this.style.boxShadow='0 8px 24px rgba(59,130,246,0.4)'" onmouseout="this.style.transform='scale(1)'; this.style.boxShadow='none'"> ส่งแบบประเมิน 🚀 </button>
      </div>
     </form>
    </main>
   </div><!-- Dashboard View -->
   <div id="dashboard-view" style="display: none;">
    <header style="background: white; padding: 24px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);">
     <div style="max-width: 1400px; margin: 0 auto; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 16px;">
      <div>
       <h1 style="font-size: 32px; font-weight: bold; color: #1e293b; margin: 0;">📊 Dashboard ผู้บริหาร/ครู</h1>
       <p style="color: #64748b; margin-top: 8px; font-size: 16px;">ระบบรายงานและวิเคราะห์ผล</p>
      </div><button onclick="showAssessmentView()" style="background: #3b82f6; color: white; padding: 12px 24px; border: none; border-radius: 8px; font-size: 16px; font-weight: 600; cursor: pointer;"> ← กลับไปแบบประเมิน </button>
     </div>
    </header>
    <main style="max-width: 1400px; margin: 0 auto; padding: 32px 24px;"><!-- Summary Cards -->
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 24px; margin-bottom: 32px;">
      <div style="background: white; padding: 24px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.08);">
       <div style="display: flex; align-items: center; gap: 16px;">
        <div style="width: 56px; height: 56px; background: #dbeafe; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 28px;">
         👥
        </div>
        <div>
         <p style="color: #64748b; font-size: 14px; margin: 0;">จำนวนนักเรียน</p>
         <p id="dash-total" style="font-size: 32px; font-weight: bold; color: #1e293b; margin: 8px 0 0 0;">0</p>
        </div>
       </div>
      </div>
      <div style="background: white; padding: 24px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.08);">
       <div style="display: flex; align-items: center; gap: 16px;">
        <div style="width: 56px; height: 56px; background: #dcfce7; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 28px;">
         ✅
        </div>
        <div>
         <p style="color: #64748b; font-size: 14px; margin: 0;">ระดับดี</p>
         <p id="dash-good" style="font-size: 32px; font-weight: bold; color: #16a34a; margin: 8px 0 0 0;">0</p>
        </div>
       </div>
      </div>
      <div style="background: white; padding: 24px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.08);">
       <div style="display: flex; align-items: center; gap: 16px;">
        <div style="width: 56px; height: 56px; background: #fef3c7; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 28px;">
         ⚠️
        </div>
        <div>
         <p style="color: #64748b; font-size: 14px; margin: 0;">ควรพัฒนา</p>
         <p id="dash-medium" style="font-size: 32px; font-weight: bold; color: #ea580c; margin: 8px 0 0 0;">0</p>
        </div>
       </div>
      </div>
      <div style="background: white; padding: 24px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.08);">
       <div style="display: flex; align-items: center; gap: 16px;">
        <div style="width: 56px; height: 56px; background: #fee2e2; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 28px;">
         🚨
        </div>
        <div>
         <p style="color: #64748b; font-size: 14px; margin: 0;">กลุ่มเสี่ยง</p>
         <p id="dash-risk" style="font-size: 32px; font-weight: bold; color: #dc2626; margin: 8px 0 0 0;">0</p>
        </div>
       </div>
      </div>
     </div><!-- Student List -->
     <div style="background: white; padding: 32px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.08);">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; flex-wrap: wrap; gap: 16px;">
       <h2 style="font-size: 24px; font-weight: bold; color: #1e293b; margin: 0;">📋 รายชื่อนักเรียนและผลประเมิน</h2>
       <div style="display: flex; gap: 12px; align-items: center;"><label for="dash-filter" style="color: #475569; font-weight: 500;">กรอง:</label> <select id="dash-filter" style="padding: 8px 16px; border: 2px solid #e2e8f0; border-radius: 8px; font-size: 14px;"> <option value="">ทั้งหมด</option> <option value="ดี">ระดับดี</option> <option value="ควรพัฒนา">ควรพัฒนา</option> <option value="เสี่ยง">กลุ่มเสี่ยง</option> </select>
       </div>
      </div>
      <div id="dashboard-list"></div>
     </div>
    </main>
   </div>
   <footer id="footer-text" style="background: rgba(255,255,255,0.1); color: white; text-align: center; padding: 24px; margin-top: 48px;">
    <p style="margin: 0; font-size: 14px;">E-Health Literacy Assessment System - พัฒนาตามกรอบ Norman &amp; Skinner</p>
   </footer>
  </div>
  <script>
    const defaultConfig = {
      system_title: "Student E-Health Profile Dashboard",
      institution_name: "แบบประเมินศักยภาพสุขภาพดิจิทัล",
      welcome_message: "ประเมินศักยภาพสุขภาพดิจิทัลของคุณ",
      footer_text: "E-Health Literacy Assessment System - พัฒนาตามกรอบ Norman & Skinner",
      background_color: "#667eea",
      card_color: "#ffffff",
      text_color: "#1e293b",
      primary_button_color: "#3b82f6",
      accent_color: "#8b5cf6"
    };

    const questions = [
      {
        category: "Digital Literacy (ความรอบรู้ดิจิทัล)",
        emoji: "💻",
        questions: [
          "ฉันสามารถใช้อุปกรณ์ดิจิทัล (สมาร์ทโฟน/แท็บเล็ต/คอมพิวเตอร์) ในการค้นหาข้อมูลสุขภาพได้",
          "ฉันรู้วิธีดาวน์โหลดและติดตั้งแอปพลิเคชันสุขภาพบนสมาร์ทโฟน"
        ]
      },
      {
        category: "Information Literacy (ความรอบรู้สารสนเทศ)",
        emoji: "🔍",
        questions: [
          "ฉันสามารถค้นหาข้อมูลสุขภาพที่น่าเชื่อถือจากอินเทอร์เน็ตได้",
          "ฉันรู้วิธีประเมินความน่าเชื่อถือของเว็บไซต์ข้อมูลสุขภาพ"
        ]
      },
      {
        category: "Health Literacy (ความรอบรู้สุขภาพ)",
        emoji: "🏥",
        questions: [
          "ฉันเข้าใจคำศัพท์ทางการแพทย์และสุขภาพที่พบในเว็บไซต์",
          "ฉันสามารถนำข้อมูลสุขภาพที่ได้ไปใช้ในชีวิตประจำวันได้"
        ]
      },
      {
        category: "Science Literacy (ความรอบรู้วิทยาศาสตร์)",
        emoji: "🔬",
        questions: [
          "ฉันเข้าใจหลักการทางวิทยาศาสตร์ที่เกี่ยวข้องกับข้อมูลสุขภาพ",
          "ฉันสามารถแยกแยะข้อมูลสุขภาพที่มีหลักฐานทางวิทยาศาสตร์และไม่มีหลักฐาน"
        ]
      },
      {
        category: "Media Literacy (ความรอบรู้สื่อ)",
        emoji: "📱",
        questions: [
          "ฉันระมัดระวังในการแชร์ข้อมูลสุขภาพบนโซเชียลมีเดีย",
          "ฉันตระหนักถึงข้อมูลสุขภาพที่เป็นเท็จ (Fake News) บนสื่อออนไลน์"
        ]
      },
      {
        category: "Critical Thinking (ความคิดวิเคราะห์)",
        emoji: "🧠",
        questions: [
          "ฉันตั้งคำถามและไตร่ตรองก่อนเชื่อข้อมูลสุขภาพที่พบออนไลน์",
          "ฉันเปรียบเทียบข้อมูลจากหลายแหล่งก่อนตัดสินใจเรื่องสุขภาพ"
        ]
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
      document.getElementById('footer-text').querySelector('p').textContent = config.footer_text || defaultConfig.footer_text;
      
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
        showToast('ไม่สามารถเชื่อมต่อระบบได้', 'error');
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
      let questionIndex = 1;
      
      container.innerHTML = questions.map((section, sectionIndex) => {
        return `
          <div class="fade-in question-card" style="background: white; padding: 32px; border-radius: 16px; box-shadow: 0 4px 16px rgba(0,0,0,0.1); margin-bottom: 24px;">
            <h3 style="font-size: 20px; font-weight: bold; color: #1e293b; margin: 0 0 20px 0;">
              ${section.emoji} ${section.category}
            </h3>
            
            ${section.questions.map((q, qIndex) => {
              const currentQ = questionIndex++;
              return `
                <div style="margin-bottom: ${qIndex === section.questions.length - 1 ? '0' : '24px'}; padding-bottom: ${qIndex === section.questions.length - 1 ? '0' : '24px'}; border-bottom: ${qIndex === section.questions.length - 1 ? 'none' : '1px solid #e2e8f0'};">
                  <label style="display: block; color: #1e293b; font-weight: 500; margin-bottom: 12px; font-size: 15px;">
                    ${currentQ}. ${q}
                  </label>
                  
                  <div style="display: flex; gap: 16px; flex-wrap: wrap;">
                    ${[
                      { value: 5, label: 'เห็นด้วยอย่างยิ่ง', color: '#10b981' },
                      { value: 4, label: 'เห็นด้วย', color: '#84cc16' },
                      { value: 3, label: 'ไม่แน่ใจ', color: '#f59e0b' },
                      { value: 2, label: 'ไม่เห็นด้วย', color: '#f97316' },
                      { value: 1, label: 'ไม่เห็นด้วยอย่างยิ่ง', color: '#ef4444' }
                    ].map(option => `
                      <label style="display: flex; align-items: center; gap: 8px; cursor: pointer; padding: 10px 16px; border: 2px solid #e2e8f0; border-radius: 8px; transition: all 0.2s;" onmouseover="this.style.borderColor='${option.color}'; this.style.background='${option.color}10'" onmouseout="if(!this.querySelector('input').checked) { this.style.borderColor='#e2e8f0'; this.style.background='transparent' }">
                        <input type="radio" name="q${currentQ}" value="${option.value}" class="radio-custom" required onchange="updateProgress(); this.parentElement.style.borderColor='${option.color}'; this.parentElement.style.background='${option.color}10'">
                        <span style="font-size: 14px; color: #475569;">${option.label}</span>
                      </label>
                    `).join('')}
                  </div>
                </div>
              `;
            }).join('')}
          </div>
        `;
      }).join('');
    }

    function setupEventListeners() {
      const form = document.getElementById('student-form');
      form.addEventListener('submit', async (e) => {
        e.preventDefault();
        
        if (assessments.length >= 999) {
          showToast('ถึงขีดจำกัด 999 รายการ กรุณาลบข้อมูลเก่าก่อน', 'error');
          return;
        }

        const submitBtn = document.getElementById('submit-btn');
        const originalText = submitBtn.innerHTML;
        submitBtn.innerHTML = '<div class="loading-spinner" style="margin: 0 auto;"></div>';
        submitBtn.disabled = true;

        const scores = {};
        for (let i = 1; i <= 12; i++) {
          const value = document.querySelector(`input[name="q${i}"]:checked`).value;
          scores[`q${i}_score`] = parseInt(value);
        }

        const categoryScores = {
          digital_literacy_score: (scores.q1_score + scores.q2_score) / 2 * 20,
          information_literacy_score: (scores.q3_score + scores.q4_score) / 2 * 20,
          health_literacy_score: (scores.q5_score + scores.q6_score) / 2 * 20,
          science_literacy_score: (scores.q7_score + scores.q8_score) / 2 * 20,
          media_literacy_score: (scores.q9_score + scores.q10_score) / 2 * 20,
          critical_thinking_score: (scores.q11_score + scores.q12_score) / 2 * 20
        };

        const overall_score = (categoryScores.digital_literacy_score + 
                              categoryScores.information_literacy_score + 
                              categoryScores.health_literacy_score + 
                              categoryScores.science_literacy_score + 
                              categoryScores.media_literacy_score + 
                              categoryScores.critical_thinking_score) / 6;

        let risk_level = 'ดี';
        if (overall_score < 50) {
          risk_level = 'เสี่ยง';
        } else if (overall_score < 70) {
          risk_level = 'ควรพัฒนา';
        }

        const assessmentData = {
          student_name: document.getElementById('student-name').value,
          student_surname: document.getElementById('student-surname').value,
          student_id: document.getElementById('student-id').value,
          class_level: document.getElementById('class-level').value,
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
          showToast('เกิดข้อผิดพลาดในการบันทึก', 'error');
        }
      });

      document.getElementById('dash-filter').addEventListener('change', updateDashboard);
    }

    function updateProgress() {
      const totalQuestions = 12;
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

    function showResultModal(data) {
      const modal = document.createElement('div');
      modal.style.cssText = 'position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.7); display: flex; align-items: center; justify-content: center; z-index: 2000; padding: 20px;';
      
      const riskColor = data.risk_level === 'ดี' ? '#16a34a' : data.risk_level === 'ควรพัฒนา' ? '#ea580c' : '#dc2626';
      
      modal.innerHTML = `
        <div class="fade-in" style="background: white; border-radius: 20px; max-width: 600px; width: 100%; max-height: 90%; overflow-y: auto; box-shadow: 0 20px 60px rgba(0,0,0,0.3);">
          <div class="result-card">
            <div style="text-align: center;">
              <div style="font-size: 64px; margin-bottom: 16px;">🎉</div>
              <h2 style="font-size: 28px; font-weight: bold; margin: 0 0 8px 0;">ส่งแบบประเมินสำเร็จ!</h2>
              <p style="opacity: 0.9; margin: 0;">${data.student_name} ${data.student_surname}</p>
            </div>
            
            <div style="display: flex; justify-content: center; margin: 32px 0;">
              <div class="score-circle" style="color: ${riskColor};">
                ${Math.round(data.overall_score)}
              </div>
            </div>
            
            <div style="text-align: center; margin-bottom: 24px;">
              <div style="display: inline-block; background: white; color: ${riskColor}; padding: 12px 24px; border-radius: 20px; font-weight: 600; font-size: 18px;">
                ${data.risk_level === 'ดี' ? '✅' : data.risk_level === 'ควรพัฒนา' ? '⚠️' : '🚨'} ${data.risk_level}
              </div>
            </div>
          </div>
          
          <div style="padding: 32px;">
            <h3 style="font-size: 20px; font-weight: bold; color: #1e293b; margin: 0 0 20px 0;">คะแนนรายด้าน</h3>
            
            <div style="display: grid; gap: 16px;">
              ${createScoreDetail('💻 Digital Literacy', data.digital_literacy_score)}
              ${createScoreDetail('🔍 Information Literacy', data.information_literacy_score)}
              ${createScoreDetail('🏥 Health Literacy', data.health_literacy_score)}
              ${createScoreDetail('🔬 Science Literacy', data.science_literacy_score)}
              ${createScoreDetail('📱 Media Literacy', data.media_literacy_score)}
              ${createScoreDetail('🧠 Critical Thinking', data.critical_thinking_score)}
            </div>
            
            <div style="margin-top: 32px; display: flex; gap: 12px;">
              <button onclick="this.closest('div[style*=fixed]').remove(); showDashboardView()" style="flex: 1; background: #3b82f6; color: white; padding: 14px; border: none; border-radius: 10px; font-size: 16px; font-weight: 600; cursor: pointer;">
                ดู Dashboard 📊
              </button>
              <button onclick="this.closest('div[style*=fixed]').remove()" style="flex: 1; background: #64748b; color: white; padding: 14px; border: none; border-radius: 10px; font-size: 16px; font-weight: 600; cursor: pointer;">
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

    function createScoreDetail(label, score) {
      const color = score >= 70 ? '#16a34a' : score >= 50 ? '#ea580c' : '#dc2626';
      return `
        <div>
          <div style="display: flex; justify-content: space-between; margin-bottom: 8px;">
            <span style="font-size: 14px; color: #64748b;">${label}</span>
            <span style="font-size: 14px; font-weight: 600; color: #1e293b;">${Math.round(score)}</span>
          </div>
          <div style="background: #e2e8f0; height: 8px; border-radius: 4px; overflow: hidden;">
            <div style="background: ${color}; height: 100%; width: ${score}%; border-radius: 4px; transition: width 0.5s;"></div>
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
          <div style="text-align: center; padding: 60px 20px; color: #94a3b8;">
            <div style="font-size: 48px; margin-bottom: 16px;">📋</div>
            <p style="font-size: 18px; margin: 0;">ไม่พบข้อมูล</p>
          </div>
        `;
        return;
      }
      
      container.innerHTML = `
        <div style="overflow-x: auto;">
          <table style="width: 100%; border-collapse: collapse;">
            <thead>
              <tr style="background: #f8fafc; border-bottom: 2px solid #e2e8f0;">
                <th style="padding: 16px; text-align: left; font-weight: 600; color: #475569;">ชื่อ-นามสกุล</th>
                <th style="padding: 16px; text-align: left; font-weight: 600; color: #475569;">รหัส</th>
                <th style="padding: 16px; text-align: left; font-weight: 600; color: #475569;">ชั้น</th>
                <th style="padding: 16px; text-align: center; font-weight: 600; color: #475569;">คะแนนรวม</th>
                <th style="padding: 16px; text-align: center; font-weight: 600; color: #475569;">ระดับ</th>
                <th style="padding: 16px; text-align: center; font-weight: 600; color: #475569;">วันที่</th>
                <th style="padding: 16px; text-align: center; font-weight: 600; color: #475569;">จัดการ</th>
              </tr>
            </thead>
            <tbody>
              ${filtered.map(student => {
                const riskColor = student.risk_level === 'ดี' ? '#16a34a' : student.risk_level === 'ควรพัฒนา' ? '#ea580c' : '#dc2626';
                const riskBg = student.risk_level === 'ดี' ? '#dcfce7' : student.risk_level === 'ควรพัฒนา' ? '#ffedd5' : '#fee2e2';
                
                return `
                  <tr style="border-bottom: 1px solid #e2e8f0;">
                    <td style="padding: 16px; color: #1e293b; font-weight: 500;">${student.student_name} ${student.student_surname}</td>
                    <td style="padding: 16px; color: #64748b;">${student.student_id}</td>
                    <td style="padding: 16px; color: #64748b;">${student.class_level}</td>
                    <td style="padding: 16px; text-align: center; font-weight: 600; color: ${riskColor}; font-size: 18px;">${Math.round(student.overall_score)}</td>
                    <td style="padding: 16px; text-align: center;">
                      <span style="background: ${riskBg}; color: ${riskColor}; padding: 6px 12px; border-radius: 12px; font-size: 13px; font-weight: 600;">
                        ${student.risk_level}
                      </span>
                    </td>
                    <td style="padding: 16px; text-align: center; color: #64748b; font-size: 14px;">${new Date(student.assessment_date).toLocaleDateString('th-TH')}</td>
                    <td style="padding: 16px; text-align: center;">
                      <button onclick="viewDetail('${student.__backendId}')" style="background: #3b82f6; color: white; border: none; padding: 8px 16px; border-radius: 6px; cursor: pointer; font-size: 13px; font-weight: 500; margin-right: 6px;">
                        👁️ ดูรายละเอียด
                      </button>
                      <button onclick="deleteAssessment('${student.__backendId}')" style="background: #ef4444; color: white; border: none; padding: 8px 16px; border-radius: 6px; cursor: pointer; font-size: 13px; font-weight: 500;">
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
      toast.style.background = type === 'success' ? '#dcfce7' : '#fee2e2';
      toast.style.color = type === 'success' ? '#166534' : '#991b1b';
      toast.style.fontWeight = '600';
      toast.textContent = message;
      
      document.body.appendChild(toast);
      
      setTimeout(() => {
        toast.style.animation = 'slideIn 0.3s ease-out reverse';
        setTimeout(() => toast.remove(), 300);
      }, 3000);
    }

    init();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a8852fbe2f6ce9b',t:'MTc2NDgyMDA2NC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
