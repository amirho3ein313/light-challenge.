<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>پازل نورها - نسخه پیشرفته</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Vazirmatn&display=swap');

  /* ریست اولیه */
  * {
    box-sizing: border-box;
  }

  body {
    margin: 0; padding: 0;
    background: #111;
    font-family: 'Vazirmatn', Tahoma, sans-serif;
    color: #eee;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 100vh;
    user-select: none;
    overflow-x: hidden;
    direction: rtl;
  }

  h1 {
    margin: 25px 0 5px;
    font-weight: 700;
    text-align: center;
    font-size: 2.8rem;
    color: #ffd700;
    text-shadow: 0 0 8px #ffec41;
  }

  #grid {
    margin-top: 20px;
    display: grid;
    gap: 7px;
    filter: drop-shadow(0 0 7px #ffde59);
    touch-action: manipulation;
  }

  .cell {
    width: 55px;
    height: 55px;
    background: #222;
    border-radius: 10px;
    border: 2px solid #555;
    cursor: pointer;
    box-shadow: inset 0 0 6px #000;
    transition: background-color 0.3s, box-shadow 0.6s;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .cell.on {
    background: #ffd700;
    box-shadow:
      0 0 18px 5px #fff755,
      inset 0 0 20px #ffffa0;
    animation: pulse 1.8s infinite;
  }

  @keyframes pulse {
    0%, 100% { box-shadow: 0 0 10px 3px #fff755, inset 0 0 15px #ffffa0; }
    50% { box-shadow: 0 0 25px 7px #fff; inset 0 0 30px #fffacd; }
  }
    /* HEADER */
    header {
      position: fixed;
      top: 0; left: 0; right: 0;
      background: rgba(40, 39, 57, 0.95);
      box-shadow: 0 2px 8px rgba(0,0,0,0.3);
      height: 60px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.8rem;
      font-weight: 900;
      letter-spacing: 3px;
      color: #ffcc00;
      z-index: 1100;
      user-select: none;
      font-family: 'Vazirmatn', sans-serif;
      text-shadow: 1px 1px 5px #222;
      animation: glow 3s ease-in-out infinite alternate;
    }
    @keyframes glow {
      from { text-shadow: 0 0 5px #ffcc00; }
      to { text-shadow: 0 0 20px #ffea00; }
    }
    /* زیرنویس چرخشی */
    #marqueeWrapper {
      position: fixed;
      bottom: 0;
      width: 100%;
      background: #000000cc;
      padding: 6px 0;
      overflow: hidden;
      z-index: 1150;
      user-select: none;
    }
    #marqueeText {
      display: inline-block;
      white-space: nowrap;
      color: gold;
      font-weight: 700;
      font-size: 1.1rem;
      animation: marqueeAnim 15s linear infinite;
      text-shadow: 0 0 10px #ffcc00;
      font-family: 'Vazirmatn', sans-serif;
      letter-spacing: 1.3px;
    }
    @keyframes marqueeAnim {
      0% {
        transform: translateX(100%);
      }
      100% {
        transform: translateX(-100%);
      }
    }
    /* FOOTER */
    footer {
      position: fixed;
      bottom: 30px; /* زیر زیرنویس باشه */
      left: 0; right: 0;
      background: rgba(0, 0, 0, 0.7);
      padding: 10px 0;
      font-size: 0.9rem;
      text-align: center;
      color: #ddd;
      font-weight: 500;
      font-family: 'Vazirmatn', sans-serif;
      user-select: none;
      box-shadow: inset 0 1px 10px rgba(255, 255, 255, 0.1);
      z-index: 1100;
    }

    /* MENU ICON */
    #menuToggle {
      position: fixed;
      top: 12px; left: 12px;
      font-size: 2.5rem;
      color: #ffcc00;
      cursor: pointer;
      z-index: 1200;
      user-select: none;
      transition: color 0.3s ease;
      text-shadow: 0 0 6px #ffcc00;
    }
    #menuToggle:hover {
      color: #fff03c;
    }
    /* OVERLAY */
    #overlay {
      position: fixed;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      background: rgba(0,0,0,0.8);
      opacity: 0;
      visibility: hidden;
      transition: opacity 0.3s ease;
      z-index: 1150;
    }
    #overlay.active {
      opacity: 1;
      visibility: visible;
    }
    /* SIDE MENU */
    #sideMenu {
      position: fixed;
      top: 0; left: 0;
      height: 100vh;
      width: 80vw;
      max-width: 450px;
      background: linear-gradient(180deg, #232526, #1c1c1c);
      box-shadow: 4px 0 15px rgba(0,0,0,0.8);
      padding: 25px 30px;
      transform: translateX(-100%);
      transition: transform 0.35s ease;
      overflow-y: auto;
      z-index: 1200;
      border-top-right-radius: 20px;
      border-bottom-right-radius: 20px;
      color: #ddd;
      font-size: 1.05rem;
      user-select: text;
    }
    #sideMenu.active {
      transform: translateX(0);
    }
    /* MENU SECTIONS */
    #sideMenu h2 {
      font-size: 1.6rem;
      font-weight: 900;
      color: #ffcc00;
      margin-bottom: 20px;
      text-align: center;
      text-shadow: 0 0 7px #ffcc00;
    }
    .menuSection {
      margin-bottom: 25px;
    }
    .menuSection label {
      font-weight: 700;
      display: block;
      margin-bottom: 10px;
      color: #ffd633;
      user-select: none;
    }
    .menuSection textarea {
      width: 100%;
      height: 110px;
      background: #2c2c2c;
      border: none;
      border-radius: 12px;
      padding: 12px 14px;
      resize: vertical;
      font-family: 'Vazirmatn', sans-serif;
      font-size: 1rem;
      color: #fff;
      box-shadow: inset 0 0 8px #000;
      transition: box-shadow 0.3s ease;
    }
    .menuSection textarea:focus {
      outline: none;
      box-shadow: inset 0 0 12px #ffcc00;
      background: #3c3c3c;
    }
    
    /* متن خروجی جذاب */
    .outputBox {
      margin-top: 10px;
      background: #1a1a1a;
      border-radius: 14px;
      padding: 15px 18px;
      font-size: 1rem;
      line-height: 1.45;
      color: #ffee77;
      box-shadow: 0 0 10px #ffcc00aa;
      min-height: 100px;
      white-space: pre-wrap;
      user-select: text;
    }
    /* دکمه ها */
    .menuBtn {
      display: block;
      width: 100%;
      background: #ffcc00;
      border: none;
      padding: 12px 0;
      margin-top: 12px;
      border-radius: 18px;
      font-weight: 900;
      font-size: 1.15rem;
      color: #222;
      cursor: pointer;
      box-shadow: 0 0 10px #ffcc00aa;
      transition: background-color 0.3s ease;
      user-select: none;
    }
    .menuBtn:hover {
      background: #ffd633;
    }
    /* قسمت ارتباط با پشتیبان */
    #supportBox {
      font-size: 1rem;
      color: #ffe066;
      line-height: 1.4;
      border-radius: 14px;
      background: #222222ee;
      padding: 14px 18px;
      box-shadow: inset 0 0 8px #ffcc00cc;
      margin-bottom: 15px;
      user-select: text;
    }
    #supportBox a {
      color: #ffcc00;
      text-decoration: none;
      font-weight: 700;
      transition: color 0.3s ease;
    }
    #supportBox a:hover {
      color: #fff03c;
    }
    /* دکمه خروج */
    #closeMenuBtn {
      background: #ff4444;
      color: white;
      font-weight: 900;
      box-shadow: 0 0 10px #ff4444aa;
    }
    #closeMenuBtn:hover {
      background: #ff0000;
    }
    /* MAIN CONTENT */
    main {
      margin-top: 20px;
      max-width: 480px;
      width: 90vw;
      background: rgba(255 255 255 / 0.1);
      border-radius: 25px;
      box-shadow: 0 0 30px #ffcc00cc;
      padding: 30px 25px;
      user-select: none;
      color: #fff;
      text-align: center;
      font-weight: 600;
    }
    main h3 {
      font-size: 1.5rem;
      margin-bottom: 25px;
      letter-spacing: 1.1px;
      user-select: text;
      text-shadow: 1px 1px 6px #000;
    }
    main input[type="text"], main input[type="number"] {
      width: 80%;
      padding: 12px 15px;
      font-size: 1.1rem;
      border-radius: 14px;
      border: none;
      outline: none;
      margin-bottom: 20px;
      text-align: center;
      font-family: 'Vazirmatn', sans-serif;
      box-shadow: inset 0 0 8px #000;
      transition: box-shadow 0.3s ease;
      user-select: text;
      background: #222;
      color: #ffd633;
      font-weight: 700;
    }
    main input[type="text"]:focus, main input[type="number"]:focus {
      box-shadow: inset 0 0 14px #ffcc00;
      background: #3c3c3c;
    }
    main button#runBtn {
      background: #ffcc00;
      color: #222;
      font-weight: 900;
      font-size: 1.2rem;
      padding: 14px 0;
      width: 60%;
      border-radius: 30px;
      cursor: pointer;
      box-shadow: 0 0 20px #ffcc00cc;
      transition: background-color 0.3s ease;
      user-select: none;
    }
    main button#runBtn:hover {
      background: #ffd633;
    }
    /* پیام خطا / خروجی */
    #result {
      margin-top: 22px;
      min-height: 70px;
      font-size: 1rem;
      color: #ffeeaa;
      background: #222;
      padding: 14px 18px;
      border-radius: 16px;
      user-select: text;
      box-shadow: inset 0 0 10px #ffcc00cc;
      white-space: pre-wrap;
      text-align: center;
    }
    /* Scrollbar در منو */
    #sideMenu::-webkit-scrollbar {
      width: 8px;
    }
    #sideMenu::-webkit-scrollbar-thumb {
      background-color: #ffcc00bb;
      border-radius: 10px;
    }
  button {
    background: #333;
    border: none;
    border-radius: 12px;
    padding: 14px 28px;
    color: #ffd700;
    font-weight: 600;
    font-size: 1.1rem;
    cursor: pointer;
    margin: 25px 10px 40px;
    box-shadow: 0 0 12px #ffec41;
    transition: background-color 0.3s ease;
    user-select: none;
  }

  button:hover {
    background: #555;
  }

  #info {
    width: 320px;
    max-width: 90vw;
    display: flex;
    justify-content: space-between;
    font-size: 1.15rem;
    margin-bottom: 10px;
    user-select: none;
  }

  #clickCount, #timer {
    background: #222;
    padding: 8px 16px;
    border-radius: 12px;
    box-shadow: inset 0 0 10px #000;
    width: 45%;
    text-align: center;
    font-weight: 500;
  }

  /* تایمر رنگی */
  #timer.green { background-color: #1b5e20; color: #a5d6a7; box-shadow: 0 0 15px #4caf50;}
  #timer.yellow { background-color: #f9a825; color: #fffde7; box-shadow: 0 0 15px #ffeb3b;}
  #timer.orange { background-color: #ef6c00; color: #fff3e0; box-shadow: 0 0 15px #ff9800;}
  #timer.red { background-color: #b71c1c; color: #ffcdd2; box-shadow: 0 0 15px #f44336;}
  #timer.blink {
    animation: blink 1s infinite;
  }
  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  /* پاپ‌آپ تنظیمات */
  #settingsOverlay {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.82);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 999;
    animation: fadeScaleIn 0.4s ease forwards;
  }

  @keyframes fadeScaleIn {
    0% {opacity: 0; transform: scale(0.8);}
    100% {opacity: 1; transform: scale(1);}
  }

  #settingsModal {
    background: #222;
    border-radius: 18px;
    padding: 50px 60px 60px;  /* پدینگ بیشتر برای فضای داخلی بیشتر */
    box-shadow: 0 0 30px #ffde59;
    max-width: 600px;         /* عرض ماکزیمم بیشتر */
    width: 95vw;              /* برای اطمینان از پر شدن فضای بیشتر */
    color: #ffd700;
    text-align: center;
  }

  #settingsModal h2 {
    margin-bottom: 18px;
    font-weight: 700;
    font-size: 2rem;
    text-shadow: 0 0 12px #ffde59;
  }

  label {
    display: block;
    margin: 18px 0 8px;
    font-size: 1.15rem;
    font-weight: 600;
  }

  select {
    width: 100%;
    padding: 9px 12px;
    font-size: 1rem;
    border-radius: 12px;
    border: none;
    background: #444;
    color: #ffd700;
    box-shadow: inset 0 0 10px #000;
    cursor: pointer;
    user-select: none;
  }

  .difficulty-options {
    display: flex;
    overflow-x: auto;
    white-space: nowrap;
    gap: 8px;
    padding-bottom: 8px;
    scrollbar-width: none; /* فایرفاکس */
  }
  .difficulty-options label {
    flex: 1;
    margin: 5PX 5px;
    cursor: pointer;
    background: #333;
    padding: 8px 6px;
    border-radius: 10px;
    box-shadow: 0 0 8px #444;
    transition: background-color 0.25s;
  }
  .difficulty-options input[type="radio"] {
    display: none;
  }
  .difficulty-options input[type="radio"]:checked + label {
    background: #ffd700;
    color: #222;
    box-shadow: 0 0 15px #fff176;
    font-weight: 700;
  }

  /* پیام رکورد */
  #recordBoard {
    margin-top: -10px;
    margin-bottom: 15px;
    font-size: 0.95rem;
    color: #fff176;
    user-select: none;
    max-width: 350px;
    width: 90vw;
    text-align: center;
  }
</style>
</head>
<body>

  <h1>🎮 چالش چراغ‌ها 🎮</h1>
  <div id="pulseText"></div>


<div id="menuToggle" title="باز کردن منو">☰</div>
<div id="overlay"></div>

<nav id="sideMenu" aria-label="منوی برنامه">
  <h2>منوی برنامه</h2>

  <section class="menuSection">
    <label for="descInput">📝 نظرات و پیشنهادات :</label>
    <textarea id="descInput" placeholder="اینجا متن بنویسید..."></textarea>
    <button class="menuBtn" onclick="showDescription()">نمایش راهنمای بازی</button>
    <div id="descOutput" class="outputBox" aria-live="polite" aria-atomic="true"></div>
  </section>

  <section class="menuSection">
    <label>📚 آموزش نحوه انجام بازی :</label>
    <button class="menuBtn" onclick="showUsage()">نمایش روش انجام بازی</button>
    <div id="usageOutput" class="outputBox" aria-live="polite" aria-atomic="true"></div>
  </section>

  <section class="menuSection">
    <label>🧑‍💻 با سازنده آشنا شوید :</label>
    <button class="menuBtn" onclick="showTeacher()">نمایش اطلاعات سازنده </button>
    <div id="teacherOutput" class="outputBox" aria-live="polite" aria-atomic="true"></div>
  </section>

  <section class="menuSection" id="supportBox">
    <label>📞 ارتباط با پشتیبان :</label>
    <p>
      من امیرحسین امانی هستم.<br />
      </p>
      شماره تماس: <a href="tel:09102247414" target="_blank" rel="noopener noreferrer">09102247414</a>
      </p>
      ایمیل من: <a href="https://a42464849@gmail.com" target="_blank" rel="noopener noreferrer">a42464849@gmail.com</a>
      </p>
      سایت من: <a href="https://virgool.io/@m_77898233" target="_blank" rel="noopener noreferrer">https://virgool.io/@m_77898233</a>
    </p>
  </section>

  <button id="closeMenuBtn" class="menuBtn" onclick="closeMenu()">خروج از منو</button>
</nav>



<!-- متن ثابت بالای زیرنویس -->
<div style="position: fixed; bottom: 60px; width: 100%; text-align: center; color: #ffcc00; font-weight: 700; font-family: 'Vazirmatn', sans-serif; user-select: none; text-shadow: 0 0 6px #ffcc00;">
 نوین، ساخته‌ی امیرحسین امانی، پلی به دنیای بازی‌های هوشمند و خلاقانه شماست.
</div>

<!-- زیرنویس چرخشی -->
<div id="marqueeWrapper" aria-label="زیرنویس چرخشی">
  <div id="marqueeText">🌟 چالش چراغ‌ها، بازی هیجان‌انگیز و فکری برای تقویت تمرکز و سرعت عمل شما! 🌟 &nbsp; &nbsp; 🌟 این بازی جذاب و چالش‌برانگیز، ذهن شما را به تحرک وامی‌دارد 🌟 &nbsp; &nbsp;</div>
</div>

  <div id="info">
    <div id="clickCount">کلیک‌ها: 0</div>
    <div id="timer" class="green">زمان: 0 ثانیه</div>
  </div>

  <div id="grid"></div>

  <div>
    <button id="restartBtn">شروع دوباره 🔄</button>
    <button id="changeSettingsBtn">تغییر محیط ⚙️</button>
  </div>

  <div id="recordBoard"></div>

  <!-- پاپ‌آپ تنظیمات -->
  <div id="settingsOverlay">
    <div id="settingsModal">
      <h2>تنظیمات بازی</h2>

      <label for="gridSizeSelect">اندازه شبکه (مربع ۳ تا ۱۰):</label>
      <select id="gridSizeSelect" aria-label="انتخاب اندازه شبکه">
        <!-- آپشن‌ها با جاوااسکریپت اضافه میشه -->
      </select>

      <label>سطح سختی:</label>
      <div class="difficulty-options" role="radiogroup" aria-label="سطح سختی">
        <input type="radio" id="diff1" name="difficulty" value="1" />
        <label for="diff1">خیلی آسون</label>
        <input type="radio" id="diff2" name="difficulty" value="2" />
        <label for="diff2">آسون</label>
        <input type="radio" id="diff3" name="difficulty" value="3" checked />
        <label for="diff3">متوسط</label>
        <input type="radio" id="diff4" name="difficulty" value="4" />
        <label for="diff4">سخت</label>
        <input type="radio" id="diff5" name="difficulty" value="5" />
        <label for="diff5">خیلی سخت</label>
      </div>

      <button id="startGameBtn" style="margin-top:25px;">شروع بازی 🎲</button>
    </div>
  </div>

<script>
  const menuToggle = document.getElementById('menuToggle');
  const sideMenu = document.getElementById('sideMenu');
  const overlay = document.getElementById('overlay');

  menuToggle.addEventListener('click', () => {
    sideMenu.classList.add('active');
    overlay.classList.add('active');
  });

  overlay.addEventListener('click', () => {
    closeMenu();
  });

  function closeMenu() {
    sideMenu.classList.remove('active');
    overlay.classList.remove('active');
    // پاک کردن خروجی ها و textarea
    document.getElementById('descOutput').textContent = '';
    document.getElementById('usageOutput').textContent = '';
    document.getElementById('teacherOutput').textContent = '';
    document.getElementById('descInput').value = '';
  }

  // توضیحات برنامه با شکلک و متن جذاب
  function showDescription() {
    const inputText = document.getElementById('descInput').value.trim();
    if (!inputText) {
      alert('لطفاً ابتدا متنی وارد کنید.');
      return;
    }
    const output = `
🎮 پازل نورها یک بازی فکری و جذابه که ذهنت رو به چالش می‌کشه!

🧠  هدفت خاموش کردن همه چراغ‌هاست، ولی هر کلیک، چراغ‌های اطراف رو هم تغییر می‌ده!

✨ ویژگی‌های جذاب بازی:

    📏 قابلیت انتخاب اندازه شبکه از ۳×۳ تا ۱۰×۱۰

    🎯 سطوح سختی متنوع از خیلی آسون تا خیلی سخت

    ⏰ تایمر هوشمند با افکت‌های رنگی 

    🏆 سیستم ثبت رکورد و چالش با خودتان

    🌙 طراحی زیبا، افکت‌های جذاب، و محیط چشم‌نواز

🔄 با هر بار شروع بازی، چیدمان چراغ‌ها متفاوت است و ذهن شما به یک ماجراجویی جدید دعوت می‌شود!
اگر عاشق پازل، منطق و نور هستید، این بازی برای شما ساخته شده 💡

در هر زمان می‌توانید از طریق دکمه تغیر محیط، بازی را شخصی‌سازی کرده و چالش جدیدی بسازید.

💬 اگر پیشنهادی داشتید، خوشحال می‌شویم بشنویم!  
    `;
    document.getElementById('descOutput').textContent = output;
  }

  // روش استفاده
  function showUsage() {
    const output = `
📌 روش انجام بازی:

1️⃣ اندازه شبکه و سطح سختی را انتخاب کنید.
2️⃣ با کلیک خانه‌ها، چراغ‌ها را خاموش یا روشن کنید.
3️⃣ هدف شما خاموش کردن تمام چراغ‌ها در کمترین تعداد کلیک و زمان ممکن است.
4️⃣ اگر زمان تمام شد، می‌توانید زمان بیشتری بگیرید یا بازی را دوباره شروع کنید.

🎯 با تمرین، مهارت حل پازل و سرعت واکنش خود را افزایش دهید!
    `;
    document.getElementById('usageOutput').textContent = output;
  }

  // اطلاعات امیرحسین
  function showTeacher() {
    const output = `
🧑‍💻 نام: امیرحسین  
👤 نام خانوادگی: امانی  

🔶 دانشجوی رشته مهندسی کامپیوتر و علاقه‌مند به دنیای بازی‌سازی و توسعه اپلیکیشن‌های تحت وب است.

🔶 او تجربه‌ی ساخت انواع بازی‌های آنلاین و آفلاین تحت شبکه را دارد و همیشه به دنبال خلق پروژه‌های خلاقانه و هوشمندانه است.

🔶 تمرکز امیرحسین روی ساخت پلتفرم‌هایی است که بتوانند تجربه‌ای نوآورانه و جذاب برای کاربران فراهم کنند.

✳️ با ترکیب دانش مهندسی و علاقه‌مندی به فناوری‌های نوین، او تلاش می‌کند تا ایده‌ها را به بهترین شکل ممکن به واقعیت تبدیل کند.  
    `;
    document.getElementById('teacherOutput').textContent = output;
  }


  // المان‌ها
  const grid = document.getElementById('grid');
  const clickCountEl = document.getElementById('clickCount');
  const timerEl = document.getElementById('timer');
  const restartBtn = document.getElementById('restartBtn');
  const changeSettingsBtn = document.getElementById('changeSettingsBtn');
  const settingsOverlay = document.getElementById('settingsOverlay');
  const startGameBtn = document.getElementById('startGameBtn');
  const gridSizeSelect = document.getElementById('gridSizeSelect');

  const recordBoard = document.getElementById('recordBoard');

  // متغیرهای بازی
  let gridSize = 5;
  let difficulty = 3;
  let clickCount = 0;
  let timer = 0;
  let timerInterval = null;
  let timeLimit = 60; // زمان پیش فرض، محاسبه شده بعدا
  let timeOverCount = 0;
  let gameActive = false;

  // رکوردها در localStorage ذخیره میشه
  // ساختار: { "5x5-3": 42 }
  let records = JSON.parse(localStorage.getItem('lightPuzzleRecords') || '{}');

  // مقداردهی اولیه انتخاب اندازه شبکه
  function fillGridSizeOptions() {
    for(let i=3; i<=10; i++){
      const opt = document.createElement('option');
      opt.value = i;
      opt.textContent = `${i} × ${i}`;
      gridSizeSelect.appendChild(opt);
    }
  }

  fillGridSizeOptions();

  // مقداردهی اولیه فرم با مقدار پیشفرض
  gridSizeSelect.value = gridSize;
  document.querySelector(`#diff${difficulty}`).checked = true;

  // نمایش رکورد فعلی
  function updateRecordDisplay() {
    const key = `${gridSize}x${gridSize}-${difficulty}`;
    const rec = records[key] || 0;
    recordBoard.textContent = rec
      ? `🏆 بهترین رکورد برای ${key}: ${rec} کلیک`
      : '🎮 هنوز رکوردی ثبت نشده است!';
  }

  updateRecordDisplay();

  // تابع شروع بازی و ایجاد شبکه
  function createGrid() {
    grid.innerHTML = '';
    grid.style.gridTemplateColumns = `repeat(${gridSize}, 55px)`;
    grid.style.gridTemplateRows = `repeat(${gridSize}, 55px)`;

    // شروع همه خاموش
    for(let r=0; r<gridSize; r++){
      for(let c=0; c<gridSize; c++){
        const cell = document.createElement('div');
        cell.classList.add('cell');
        cell.dataset.row = r;
        cell.dataset.col = c;
        cell.addEventListener('click', onCellClick);
        grid.appendChild(cell);
      }
    }
  }

  // محاسبه زمان بر اساس اندازه و سختی (مثال)
  function calculateTimeLimit(size, diff) {
    // پایه: 10 ثانیه به ازای هر خانه، سطح ضریب میگیره
    const base = size * size * 10;
    const diffMultiplier = [1.8, 1.4, 1, 0.7, 0.5];
    return Math.max(10, Math.floor(base * diffMultiplier[diff - 1]));
  }

  // الگوریتم چیدمان اولیه با سطح سختی
  function setupInitialGrid() {
    // همه خاموش اول
    for(let i=0; i<grid.children.length; i++){
      grid.children[i].classList.remove('on');
    }
    // چیدمان تصادفی به تعداد خانه‌های روشن، بر اساس سطح سختی
    // سطح 1: 10% روشن
    // سطح 2: 20%
    // سطح 3: 35%
    // سطح 4: 50%
    // سطح 5: 70%

    const onPercent = [0.1, 0.2, 0.35, 0.5, 0.7][difficulty - 1];
    const totalCells = gridSize * gridSize;
    const onCount = Math.floor(totalCells * onPercent);

    // روشن کردن تعداد مشخص خانه‌ها بصورت تصادفی (بدون تضمین حل)
    let indices = [...Array(totalCells).keys()];
    indices = shuffle(indices);

    for(let i=0; i<onCount; i++){
      grid.children[indices[i]].classList.add('on');
    }
  }

  // شفل آرایه
  function shuffle(arr){
    for(let i=arr.length-1; i>0; i--){
      const j = Math.floor(Math.random()*(i+1));
      [arr[i], arr[j]] = [arr[j], arr[i]];
    }
    return arr;
  }

  // وقتی سلول کلیک شد
  function onCellClick(e){
    if(!gameActive) return;
    clickCount++;
    clickCountEl.textContent = `کلیک‌ها: ${clickCount}`;
    const r = +e.target.dataset.row;
    const c = +e.target.dataset.col;

    toggleCell(r, c);
    toggleCell(r-1, c);
    toggleCell(r+1, c);
    toggleCell(r, c-1);
    toggleCell(r, c+1);

    if(checkWin()){
      gameWin();
    }
  }

  function toggleCell(r, c){
    if(r<0 || c<0 || r>=gridSize || c>=gridSize) return;
    const idx = r*gridSize + c;
    grid.children[idx].classList.toggle('on');
  }

  // بررسی برد (همه خاموش)
  function checkWin(){
    for(let i=0; i<grid.children.length; i++){
      if(grid.children[i].classList.contains('on')) return false;
    }
    return true;
  }

  // وقتی بازی تموم شد
  function gameWin(){
    clearInterval(timerInterval);
    gameActive = false;
    alert(`🎉 تبریک! همه چراغ‌ها خاموش شدند!\nتعداد کلیک‌ها: ${clickCount}\nزمان سپری شده: ${timeLimit - timer} ثانیه`);

    // ذخیره رکورد
    const key = `${gridSize}x${gridSize}-${difficulty}`;
    const prevRecord = records[key] || Infinity;
    if(clickCount < prevRecord){
      records[key] = clickCount;
      localStorage.setItem('lightPuzzleRecords', JSON.stringify(records));
      updateRecordDisplay();
      alert('🏆 رکورد جدید ثبت شد!');
    }

    // بعد از برد، پاپ‌آپ تنظیمات میاد برای بازی جدید
    showSettings();
  }

  // شروع تایمر
  function startTimer(){
    timer = timeLimit;
    updateTimerDisplay();
    timerInterval = setInterval(() => {
      timer--;
      updateTimerDisplay();
      if(timer <= 0){
        clearInterval(timerInterval);
        gameActive = false;
        onTimeOver();
      }
    }, 1000);
  }

  // بروز رسانی رنگ و متن تایمر
  function updateTimerDisplay(){
    timerEl.textContent = `زمان: ${timer} ثانیه`;
    timerEl.classList.remove('green','yellow','orange','red','blink');

    if(timer > timeLimit*0.5) timerEl.classList.add('green');
    else if(timer > timeLimit*0.25) timerEl.classList.add('yellow');
    else if(timer > 10) timerEl.classList.add('orange');
    else if(timer > 5) timerEl.classList.add('red');
    else {
      timerEl.classList.add('red','blink');
    }
  }

  // زمان تموم شده
  function onTimeOver(){
    timeOverCount++;
    const again = confirm('⏰ زمان شما تمام شد!\nآیا می‌خواهید زمان بیشتری بگیرید؟');
    if(again){
      startTimer();
      timerEl.textContent += ` (زمان اضافه: ${timeOverCount})`;
      gameActive = true;
    } else {
      alert('بازی تمام شد! به صفحه تنظیمات برمی‌گردیم.');
      showSettings();
    }
  }

  // شروع بازی با تنظیمات فعلی
  function startGame(){
    clickCount = 0;
    clickCountEl.textContent = `کلیک‌ها: 0`;
    gameActive = true;
    timeOverCount = 0;

    createGrid();
    setupInitialGrid();

    timeLimit = calculateTimeLimit(gridSize, difficulty);
    startTimer();

    settingsOverlay.style.display = 'none';
  }

  // نمایش پاپ‌آپ تنظیمات
  function showSettings(){
    settingsOverlay.style.display = 'flex';
    updateRecordDisplay();
  }

  // دکمه شروع بازی در پاپ‌آپ
  startGameBtn.addEventListener('click', () => {
    const size = parseInt(gridSizeSelect.value);
    const diffRadio = document.querySelector('input[name="difficulty"]:checked');
    const diff = diffRadio ? parseInt(diffRadio.value) : 3;

    if(size < 3 || size > 10) {
      alert('اندازه باید بین ۳ و ۱۰ باشد.');
      return;
    }

    gridSize = size;
    difficulty = diff;
    startGame();
  });

  // دکمه شروع دوباره بازی با تنظیمات فعلی
  restartBtn.addEventListener('click', () => {
    if(!gameActive){
      alert('بازی در حال اجرا نیست.');
      return;
    }
    clickCount = 0;
    clickCountEl.textContent = `کلیک‌ها: 0`;
    setupInitialGrid();
    clearInterval(timerInterval);
    timeLimit = calculateTimeLimit(gridSize, difficulty);
    startTimer();
  });

  // دکمه تغییر محیط (نمایش مجدد پاپ‌آپ)
  changeSettingsBtn.addEventListener('click', () => {
    clearInterval(timerInterval);
    gameActive = false;
    showSettings();
  });

  // وقتی صفحه لود شد پاپ‌آپ نمایش داده شود
  window.addEventListener('load', () => {
    showSettings();
  });

  
</script>

</body>
</html>
