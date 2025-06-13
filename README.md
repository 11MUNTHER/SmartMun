<!DOCTYPE html><html lang="ar" dir="rtl" style="display: contents;">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>SmartMun</title>
  <style>
    :root {
      --bg-light: #ffffff;
      --bg-dark: #121212;
      --text-light: #000000;
      --text-dark: #ffffff;
      --primary: #1f1f1f;
      --secondary: #f0f0f0;
    }body {
  font-family: 'Cairo', sans-serif;
  background-color: var(--bg-dark);
  color: var(--text-dark);
  margin: 0;
  padding: 0;
  transition: background 0.3s, color 0.3s;
}

.light-mode {
  background-color: var(--bg-light);
  color: var(--text-light);
}

header {
  background-color: #1a1a1a;
  color: white;
  padding: 15px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.light-mode header {
  background-color: #e0e0e0;
  color: black;
}

.nav-links a {
  color: inherit;
  text-decoration: none;
  margin-left: 15px;
  font-weight: bold;
}

main {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40px 20px;
}

h1 {
  font-size: 28px;
  margin-bottom: 20px;
}

textarea,
input[type="text"] {
  width: 100%;
  max-width: 500px;
  padding: 12px;
  font-size: 16px;
  border-radius: 8px;
  border: none;
  margin-bottom: 10px;
  background: #2c2c2c;
  color: white;
  outline: none;
}

.light-mode textarea,
.light-mode input[type="text"] {
  background: #f5f5f5;
  color: black;
}

button {
  margin-top: 10px;
  padding: 10px 20px;
  font-size: 14px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  background: #333;
  color: white;
  transition: 0.3s;
}

button:hover {
  background: #555;
}

.post {
  margin-top: 20px;
  background-color: #1e1e2f;
  padding: 20px;
  border-radius: 10px;
  max-width: 500px;
  width: 100%;
  font-size: 16px;
  line-height: 1.6;
  box-shadow: 0 4px 20px rgba(0,0,0,0.3);
  white-space: pre-wrap;
  position: relative;
}

.light-mode .post {
  background-color: #eee;
  color: #000;
}

#copyBtn {
  position: absolute;
  top: 10px;
  left: 10px;
  font-size: 12px;
  padding: 4px 8px;
  background-color: #555;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

@media screen and (max-width: 600px) {
  main {
    padding: 20px 10px;
  }
  textarea,
  input[type="text"],
  .post {
    font-size: 14px;
  }
}

  </style>
</head>
<body>
  <header>
    <div class="logo">SmartMun</div>
    <div class="nav-links">
      <a href="index.html">الرئيسية</a>
      <a href="about.html">حول</a>
      <a href="#" onclick="toggleTheme()">الوضع</a>
    </div>
  </header>
  <main>
    <h1>منشور ذكي</h1>
    <input id="apiKey" type="text" placeholder="أدخل مفتاح OpenAI API هنا" />
    <textarea id="inputText" placeholder="اكتب فكرة البوست أو كلمه بسيطة..."></textarea>
    <button onclick="generatePost()">توليد المنشور</button>
    <div id="postOutput" class="post" style="display:none;"></div>
    <audio id="dingSound" src="https://www.soundjay.com/buttons/sounds/button-3.mp3" preload="auto"></audio>
  </main>
  <script>
    function toggleTheme() {
      document.body.classList.toggle("light-mode");
    }async function generatePost() {
  const apiKey = document.getElementById("apiKey").value.trim();
  const input = document.getElementById("inputText").value.trim();
  const output = document.getElementById("postOutput");
  const dingSound = document.getElementById("dingSound");

  if (!apiKey || !input) {
    alert("رجاءً أدخل الـ API Key والفكرة!");
    return;
  }

  output.innerText = "جاري التوليد...";
  output.style.display = "block";

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
          { role: "system", content: "اكتب منشور بسيط باللهجة العدنية حسب الموضوع." },
          { role: "user", content: input }
        ],
        max_tokens: 150,
      }),
    });

    const data = await response.json();

    if (data.choices && data.choices.length > 0) {
      output.innerHTML = `<button id=\"copyBtn\" onclick=\"copyText()\">نسخ</button>` + data.choices[0].message.content.trim();
      dingSound.play();
    } else {
      output.innerText = "حصلت مشكلة... تأكد من الـ API Key.";
    }
  } catch (error) {
    output.innerText = "فشل الاتصال. تأكد من الانترنت والـ API Key.";
  }
}

function copyText() {
  const output = document.getElementById("postOutput");
  const text = output.innerText.replace("نسخ", "").trim();
  navigator.clipboard.writeText(text).then(() => {
    alert("تم نسخ المنشور!");
  });
}

  </script>
</body>
</html>
