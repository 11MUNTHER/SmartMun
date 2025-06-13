<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SmartMun</title>
  <link href="https://fonts.googleapis.com/css2?family=Cairo&display=swap" rel="stylesheet">
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      font-family: 'Cairo', sans-serif;
      margin: 0;
      background-color: #fff;
      color: #000;
    }

    header {
      background-color: #000;
      color: #fff;
      padding: 15px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      position: relative;
    }

    .logo {
      font-size: 22px;
      font-weight: bold;
    }

    .menu-toggle {
      font-size: 24px;
      cursor: pointer;
      background: none;
      border: none;
      color: white;
      z-index: 1001;
    }

    .side-nav {
      position: fixed;
      top: 0;
      right: -250px;
      height: 100%;
      width: 250px;
      background-color: #000;
      display: flex;
      flex-direction: column;
      padding-top: 60px;
      transition: right 0.3s ease;
      z-index: 1000;
    }

    .side-nav.show {
      right: 0;
    }

    .side-nav a {
      padding: 15px 20px;
      text-decoration: none;
      color: #fff;
      border-bottom: 1px solid #333;
    }

    .side-nav a:hover {
      background-color: #222;
    }

    main {
      padding: 30px 20px;
      max-width: 600px;
      margin: auto;
    }

    textarea, input[type="text"] {
      width: 100%;
      padding: 12px;
      margin-bottom: 10px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 16px;
    }

    button {
      padding: 12px 24px;
      background-color: #000;
      color: #fff;
      font-size: 16px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }

    button:hover {
      background-color: #333;
    }

    .post {
      margin-top: 20px;
      background: #f0f0f0;
      padding: 15px;
      border-radius: 8px;
      color: #000;
      white-space: pre-wrap;
    }

    .copy-btn {
      margin-top: 10px;
      background-color: #444;
      color: #fff;
      border: none;
      padding: 10px;
      border-radius: 6px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <header>
    <div class="logo">SmartMun</div>
    <button class="menu-toggle" onclick="toggleMenu()">☰</button>
  </header>

  <div id="sideNav" class="side-nav">
    <a href="index.html">الرئيسية</a>
    <a href="#" onclick="toggleTheme()">الوضع</a>
    <a href="about.html">حول</a>
  </div>

  <main>
    <input id="apiKey" type="text" placeholder="🔐 أدخل مفتاح OpenAI API هنا (سرّياً)" />
    <textarea id="inputText" placeholder="اكتب فكرة البوست أو كلمه بسيطة..."></textarea>
    <button onclick="generatePost()">توليد منشور</button>
    <div id="postOutput" class="post" style="display:none;"></div>
    <button class="copy-btn" onclick="copyPost()" style="display:none;">نسخ المنشور</button>
  </main>

  <script>
    function toggleMenu() {
      const sideNav = document.getElementById('sideNav');
      sideNav.classList.toggle('show');
    }

    function toggleTheme() {
      const body = document.body;
      if (body.style.backgroundColor === 'black') {
        body.style.backgroundColor = 'white';
        body.style.color = 'black';
      } else {
        body.style.backgroundColor = 'black';
        body.style.color = 'white';
      }
    }

    function copyPost() {
      const post = document.getElementById("postOutput").innerText;
      navigator.clipboard.writeText(post).then(() => {
        alert("تم نسخ المنشور!");
      });
    }

    async function generatePost() {
      const apiKey = document.getElementById("apiKey").value.trim();
      const input = document.getElementById("inputText").value.trim();
      const output = document.getElementById("postOutput");
      const copyBtn = document.querySelector(".copy-btn");

      if (!apiKey || !input) {
        alert("رجاءً أدخل الـ API Key والفكرة!");
        return;
      }

      output.innerText = "⏳ جاري توليد البوست...";
      output.style.display = "block";

      const response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": "Bearer " + apiKey,
        },
        body: JSON.stringify({
          model: "gpt-3.5-turbo",
          messages: [
            { role: "system", content: "اكتب منشور بسيط باللهجة العدنية حسب الموضوع." },
            { role: "user", content: input }
          ],
          max_tokens: 150,
        }),
      });

      const data = await response.json();

      if (data.choices && data.choices.length > 0) {
        output.innerText = data.choices[0].message.content.trim();
        copyBtn.style.display = "inline-block";
        new Audio("https://cdn.pixabay.com/download/audio/2023/03/06/audio_c73d42b2e2.mp3?filename=interface-124464.mp3").play();
      } else {
        output.innerText = "⚠️ حصلت مشكلة في التوليد... تأكد من الـ API Key.";
      }
    }
  </script>
</body>
</html>
