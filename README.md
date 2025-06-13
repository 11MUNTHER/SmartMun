<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>بوست ذكي 🤖✍️</title>
  <style>
    body {
      background: linear-gradient(145deg, #1a1a2e, #16213e);
      color: #fff;
      font-family: 'Cairo', sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 40px 20px;
      min-height: 100vh;
      margin: 0;
    }

    h1 {
      margin-bottom: 30px;
      font-size: 30px;
      color: #ffcc00;
    }

    textarea {
      width: 100%;
      max-width: 500px;
      height: 100px;
      padding: 15px;
      font-size: 16px;
      border-radius: 12px;
      border: none;
      resize: none;
      outline: none;
      background: #2c2c54;
      color: white;
    }

    input[type="text"] {
      margin-top: 10px;
      padding: 10px;
      width: 100%;
      max-width: 500px;
      border-radius: 8px;
      border: none;
      background: #2c2c54;
      color: white;
      outline: none;
    }

    button {
      margin-top: 20px;
      padding: 12px 24px;
      background: linear-gradient(to right, #9d50bb, #6e48aa);
      color: white;
      font-size: 16px;
      border: none;
      border-radius: 20px;
      cursor: pointer;
      transition: transform 0.2s ease, box-shadow 0.3s ease;
    }

    button:hover {
      transform: scale(1.05);
      box-shadow: 0 0 15px #9d50bb;
    }

    .post {
      margin-top: 30px;
      background-color: #0f3460;
      padding: 20px;
      border-radius: 16px;
      max-width: 500px;
      width: 100%;
      font-size: 18px;
      line-height: 1.6;
      box-shadow: 0 4px 20px rgba(0,0,0,0.3);
      white-space: pre-wrap;
    }
  </style>
</head>
<body>
  <h1>بوست ذكي 🤖✍️</h1>

  <input id="apiKey" type="text" placeholder="🔐 أدخل مفتاح OpenAI API هنا (سرّياً)" />
  <textarea id="inputText" placeholder="اكتب فكرة البوست أو كلمه بسيطة..."></textarea>
  <button onclick="generatePost()">🎯 توليد منشور ذكي</button>
  <div id="postOutput" class="post" style="display:none;"></div>

  <script>
    async function generatePost() {
      const apiKey = document.getElementById("apiKey").value.trim();
      const input = document.getElementById("inputText").value.trim();
      const output = document.getElementById("postOutput");

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
            { role: "system", content: "اكتب منشور بسيط وعفوي باللهجة العدنية حسب الموضوع." },
            { role: "user", content: input }
          ],
          max_tokens: 150,
        }),
      });

      const data = await response.json();

      if (data.choices && data.choices.length > 0) {
        output.innerText = data.choices[0].message.content.trim();
      } else {
        output.innerText = "⚠️ حصلت مشكلة في التوليد... تأكد من الـ API Key.";
      }
    }
  </script>
</body>
</html>
