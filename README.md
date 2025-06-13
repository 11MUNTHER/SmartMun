<!DOCTYPE html>
<html lang="ar" dir="rtl" style="background:#fff;color:#000;font-family:'Cairo',sans-serif;">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />
<title>smartmun - منشورات ذكية</title>
<style>
  /* Reset بسيط */
  * {
    margin: 0; padding: 0; box-sizing: border-box;
  }
  body {
    background: #fff;
    color: #000;
    font-family: 'Cairo', sans-serif;
    overflow-x: hidden;
    transition: background 0.4s,color 0.4s;
  }

  /* الوضع الداكن */
  body.dark {
    background: #121212;
    color: #eee;
  }

  /* الحاوية */
  #app {
    display: flex;
    min-height: 100vh;
  }

  /* القائمة الجانبية */
  nav {
    position: fixed;
    top: 0; bottom: 0; right: 0;
    width: 280px;
    background: #000;
    color: #fff;
    padding: 25px 20px;
    font-weight: 600;
    display: flex;
    flex-direction: column;
    gap: 20px;
    transform: translateX(100%);
    transition: transform 0.3s ease;
    z-index: 100;
    box-shadow: -4px 0 15px rgba(0,0,0,0.3);
    border-left: 2px solid #444;
  }
  nav.dark {
    background: #222;
    border-left-color: #666;
  }
  nav.open {
    transform: translateX(0);
  }

  nav h2 {
    font-size: 28px;
    margin-bottom: 15px;
    color: #ffcc00;
    text-align: center;
  }

  nav a {
    color: #ddd;
    text-decoration: none;
    font-size: 18px;
    padding: 10px 12px;
    border-radius: 8px;
    transition: background 0.25s;
  }
  nav a:hover {
    background: #333;
    color: #fff;
  }
  nav.dark a:hover {
    background: #555;
  }

  /* زر اغلاق القائمة */
  #closeMenuBtn {
    align-self: flex-start;
    margin-bottom: 20px;
    cursor: pointer;
    background: none;
    border: none;
    font-size: 26px;
    color: #ffcc00;
    font-weight: bold;
    user-select: none;
    transition: color 0.2s;
  }
  #closeMenuBtn:hover {
    color: #fff;
  }

  /* زر فتح القائمة (الهامبرجر) */
  #hamburgerBtn {
    position: fixed;
    top: 20px;
    right: 20px;
    width: 36px;
    height: 28px;
    cursor: pointer;
    z-index: 110;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  #hamburgerBtn span {
    display: block;
    height: 4px;
    background: #000;
    border-radius: 3px;
    transition: background 0.3s;
  }
  body.dark #hamburgerBtn span {
    background: #eee;
  }

  /* لما تكون القائمة مفتوحة نغير شكل الهامبرجر */
  #hamburgerBtn.open span:nth-child(1) {
    transform: rotate(45deg) translate(5px, 6px);
  }
  #hamburgerBtn.open span:nth-child(2) {
    opacity: 0;
  }
  #hamburgerBtn.open span:nth-child(3) {
    transform: rotate(-45deg) translate(5px, -6px);
  }

  /* المحتوى الرئيسي */
  main {
    flex-grow: 1;
    padding: 70px 30px 40px 30px;
    max-width: 600px;
    margin: auto;
  }

  main h1 {
    font-size: 32px;
    margin-bottom: 30px;
    color: #ffcc00;
    text-align: center;
  }

  /* الحقول */
  input[type="text"], textarea {
    width: 100%;
    background: #f4f4f4;
    border: 2px solid #ddd;
    border-radius: 12px;
    padding: 14px 18px;
    font-size: 16px;
    color: #222;
    resize: none;
    transition: border-color 0.3s;
  }
  input[type="text"]:focus, textarea:focus {
    border-color: #ffcc00;
    outline: none;
  }
  body.dark input[type="text"], body.dark textarea {
    background: #222;
    border-color: #555;
    color: #eee;
  }
  body.dark input[type="text"]:focus, body.dark textarea:focus {
    border-color: #ffcc00;
  }

  textarea {
    min-height: 120px;
    font-family: 'Cairo', sans-serif;
  }

  /* أزرار */
  button {
    margin-top: 25px;
    background: #ffcc00;
    border: none;
    padding: 14px 25px;
    font-weight: 700;
    font-size: 18px;
    border-radius: 25px;
    cursor: pointer;
    transition: background 0.3s;
    color: #222;
    width: 100%;
  }
  button:hover {
    background: #ddb500;
  }

  body.dark button {
    color: #222;
  }

  /* قسم النتيجة */
  #postOutput {
    margin-top: 35px;
    background: #222;
    color: #ffcc00;
    border-radius: 16px;
    padding: 20px 25px;
    font-size: 18px;
    line-height: 1.6;
    white-space: pre-wrap;
    box-shadow: 0 0 10px #ffcc00aa;
    display: none;
  }
  body.dark #postOutput {
    background: #111;
    box-shadow: 0 0 20px #ffcc0077;
    color: #ffcc00;
  }

  /* زر نسخ */
  #copyBtn {
    margin-top: 15px;
    background: transparent;
    border: 2px solid #ffcc00;
    color: #ffcc00;
    border-radius: 20px;
    padding: 8px 18px;
    font-size: 16px;
    cursor: pointer;
    transition: background 0.3s,color 0.3s;
    display: none;
  }
  #copyBtn:hover {
    background: #ffcc00;
    color: #222;
  }

  /* قسم حفظ التاريخ */
  #historySection {
    margin-top: 45px;
  }
  #historySection h3 {
    margin-bottom: 15px;
    font-weight: 700;
    color: #ffcc00;
  }
  #historyList {
    max-height: 200px;
    overflow-y: auto;
    background: #f0f0f0;
    padding: 15px;
    border-radius: 10px;
    font-size: 15px;
    color: #222;
  }
  #historyList li {
    margin-bottom: 12px;
    cursor: pointer;
    padding: 7px 10px;
    border-radius: 6px;
    transition: background 0.3s;
  }
  #historyList li:hover {
    background: #ffcc00;
    color: #222;
  }
  body.dark #historyList {
    background: #222;
    color: #eee;
  }
  body.dark #historyList li:hover {
    background: #ffcc00;
    color: #222;
  }

  /* صفحة حول (مخبأة بالافتراضي) */
  #aboutPage {
    display: none;
    max-width: 600px;
    margin: 40px auto;
    padding: 20px;
    background: #f8f8f8;
    color: #222;
    border-radius: 16px;
    box-shadow: 0 0 10px #ccc;
    line-height: 1.7;
  }
  body.dark #aboutPage {
    background: #222;
    color: #eee;
    box-shadow: 0 0 15px #444;
  }

  /* تحسينات للهواتف */
  @media (max-width: 768px) {
    nav {
      width: 70vw;
    }
    main {
      padding: 90px 20px 40px 20px;
      margin: 0 10px;
    }
    #postOutput {
      font-size: 16px;
    }
    #historyList {
      max-height: 150px;
      font-size: 14px;
    }
  }
</style>
</head>
<body>
<div id="app">
  <!-- قائمة جانبية -->
  <nav id="sideMenu" aria-label="القائمة الجانبية">
    <button id="closeMenuBtn" aria-label="إغلاق القائمة">×</button>
    <h2>smartmun</h2>
    <a href="#" data-target="mainPage" class="nav-link">الرئيسية</a>
    <a href="#" data-target="aboutPage" class="nav-link">حول</a>
    <a href="#" id="toggleThemeBtn">الوضع الداكن</a>
  </nav>

  <!-- زر الهامبرجر -->
  <button id="hamburgerBtn" aria-label="فتح القائمة">
    <span></span><span></span><span></span>
  </button>

  <!-- المحتوى الرئيسي -->
  <main id="mainPage">
    <h1>بوست ذكي</h1>
    <input id="apiKey" type="text" placeholder="🔐 أدخل مفتاح OpenAI API هنا (سرّياً)" autocomplete="off" />
    <textarea id="inputText" placeholder="اكتب فكرة البوست أو كلمة بسيطة..."></textarea>
    <button id="generateBtn">توليد منشور ذكي</button>
    <div id="postOutput"></div>
    <button id="copyBtn">نسخ المنشور</button>

    <section id="historySection">
      <h3>تاريخ المنشورات</h3>
      <ul id="historyList"></ul>
    </section>
  </main>

  <!-- صفحة حول -->
  <section id="aboutPage" aria-label="صفحة حول">
    <h1>عن smartmun</h1>
    <p>هذا الموقع من تصميم وتطوير منذر سامي، يهدف إلى توليد منشورات ذكية وعفوية باللهجة العدنية باستخدام OpenAI API.</p>
    <p>يمكنك كتابة فكرة بسيطة، وسيتم توليد منشور جاهز للنشر على وسائل التواصل الاجتماعي بطريقة عفوية تناسب جمهورك.</p>
    <p>الموقع يدعم الوضع الفاتح والداكن، ويحفظ تاريخ منشوراتك السابقة للرجوع إليها بسهولة.</p>
    <p>أتمنى لك تجربة ممتعة وناجحة!</p>
  </section>
</div>

<!-- صوت التنبيه -->
<audio id="soundGen" src="https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg" preload="auto"></audio>

<script>
  // العناصر
  const sideMenu = document.getElementById('sideMenu');
  const hamburgerBtn = document.getElementById('hamburgerBtn');
  const closeMenuBtn = document.getElementById('closeMenuBtn');
  const toggleThemeBtn = document.getElementById('toggleThemeBtn');
  const generateBtn = document.getElementById('generateBtn');
  const apiKeyInput = document.getElementById('apiKey');
  const inputText = document.getElementById('inputText');
  const postOutput = document.getElementById('postOutput');
  const copyBtn = document.getElementById('copyBtn');
  const historyList = document.getElementById('historyList');
  const mainPage = document.getElementById('mainPage');
  const aboutPage = document.getElementById('aboutPage');
  const soundGen = document.getElementById('soundGen');

  // فتح القائمة
  hamburgerBtn.onclick = () => {
    sideMenu.classList.add('open');
    hamburgerBtn.classList.add('open');
  };

  // غلق القائمة
  closeMenuBtn.onclick = () => {
    sideMenu.classList.remove('open');
    hamburgerBtn.classList.remove('open');
  };

  // تبديل الصفحات (الرئيسية - حول)
  document.querySelectorAll('.nav-link').forEach(link => {
    link.onclick = e => {
      e.preventDefault();
      const target = link.getAttribute('data-target');
      if(target === 'mainPage') {
        mainPage.style.display = 'block';
        aboutPage.style.display = 'none';
      } else if(target === 'aboutPage') {
        mainPage.style.display = 'none';
        aboutPage.style.display = 'block';
      }
      sideMenu.classList.remove('open');
      hamburgerBtn.classList.remove('open');
    };
  });

  // وضع داكن/فاتح مع حفظ الاختيار
  function setTheme(dark) {
    if(dark) {
      document.body.classList.add('dark');
      toggleThemeBtn.textContent = 'الوضع الفاتح';
    } else {
      document.body.classList.remove('dark');
      toggleThemeBtn.textContent = 'الوضع الداكن';
    }
    localStorage.setItem('themeDark', dark);
  }
  toggleThemeBtn.onclick = () => {
    const dark = document.body.classList.contains('dark');
    setTheme(!dark);
  };
  // تحميل حالة الوضع
  const savedTheme = localStorage.getItem('themeDark') === 'true';
  setTheme(savedTheme);

  // توليد المنشور
  async function generatePost() {
    const apiKey = apiKeyInput.value.trim();
    const input = inputText.value.trim();
    if(!apiKey || !input) {
      alert('رجاءً أدخل الـ API Key والفكرة!');
      return;
    }
    postOutput.style.display = 'block';
    postOutput.textContent = '⏳ جاري توليد البوست...';
    copyBtn.style.display = 'none';

    try {
      const response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": "Bearer " + apiKey,
        },
        body: JSON.stringify({
          model: "gpt-3.5-turbo",
          messages: [
            { role: "system", content: "اكتب منشور بسيط وعفوي باللهجة العدنية حسب الموضوع." },
            { role: "user", content: input }
          ],
          max_tokens: 150,
        }),
      });
      const data = await response.json();
      if(data.choices && data.choices.length > 0) {
        const text = data.choices[0].message.content.trim();
        postOutput.textContent = text;
        copyBtn.style.display = 'inline-block';
        soundGen.play();
        saveToHistory(text);
      } else {
        postOutput.textContent = '⚠️ حصلت مشكلة في التوليد... تأكد من الـ API Key.';
      }
    } catch(err) {
      postOutput.textContent = '⚠️ خطأ في الاتصال بالإنترنت أو الـ API.';
    }
  }

  generateBtn.onclick = generatePost;

  // نسخ المنشور
  copyBtn.onclick = () => {
    if(postOutput.textContent) {
      navigator.clipboard.writeText(postOutput.textContent)
        .then(() => alert('تم نسخ المنشور'))
        .catch(() => alert('فشل النسخ'));
    }
  };

  // حفظ و تحميل التاريخ من localStorage
  function saveToHistory(text) {
    let history = JSON.parse(localStorage.getItem('postHistory') || '[]');
    // تخزين فقط 20 منشور آخر
    history.unshift({text: text, date: new Date().toISOString()});
    if(history.length > 20) history.pop();
    localStorage.setItem('postHistory', JSON.stringify(history));
    renderHistory();
  }

  function renderHistory() {
    let history = JSON.parse(localStorage.getItem('postHistory') || '[]');
    historyList.innerHTML = '';
    if(history.length === 0) {
      historyList.innerHTML = '<li>لا يوجد منشورات محفوظة</li>';
      return;
    }
    history.forEach((item,i) => {
      const li = document.createElement('li');
      let d = new Date(item.date);
      li.textContent = `${d.toLocaleDateString('ar-YE')} - ${item.text.slice(0,50)}${item.text.length>50?'...':''}`;
      li.title = item.text;
      li.onclick = () => {
        mainPage.style.display = 'block';
        aboutPage.style.display = 'none';
        postOutput.style.display = 'block';
        postOutput.textContent = item.text;
        copyBtn.style.display = 'inline-block';
        sideMenu.classList.remove('open');
        hamburgerBtn.classList.remove('open');
      };
      historyList.appendChild(li);
    });
  }

  // تحميل التاريخ عند بدء الصفحة
  renderHistory();

  // إخفاء المحتوى الغير رئيسي عند تحميل الصفحة
  aboutPage.style.display = 'none';

</script>
</body>
</html>
