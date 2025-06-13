<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>بوست ذكي</title>
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap" rel="stylesheet">
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: 'Cairo', sans-serif;
      background-color: #fff;
      color: #000;
      display: flex;
      flex-direction: column;
      min-height: 100vh;
    }

    header {
      padding: 30px 20px;
      text-align: center;
      border-bottom: 1px solid #ccc;
    }

    header h1 {
      font-size: 32px;
      margin: 0;
    }

    main {
      flex: 1;
      padding: 40px 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    input[type="text"], textarea {
      width: 100%;
      max-width: 600px;
      margin-top: 20px;
      padding: 15px;
      font-size: 16px;
      border: 1px solid #000;
      border-radius: 10px;
      background-color: #fff;
      color: #000;
      outline: none;
      transition: border 0.2s ease;
    }

    textarea {
      resize: none;
      height: 120px;
    }

    input:focus, textarea:focus {
      border: 2px solid #000;
    }

    button {
      margin-top: 25px;
      padding: 12px 30px;
      font-size: 16px;
      background-color: #000;
      color: #fff;
      border: none;
      border-radius: 30px;
      cursor: pointer;
      transition: transform 0.2s ease, box-shadow 0.3s ease;
    }

    button:hover {
      transform: scale(1.05);
      box-shadow: 0 0 10px rgba(0,0,0,0.3);
    }

    .post {
      margin-top: 30px;
      background-color: #f2f2f2;
      color: #000;
      padding: 20px;
      border-radius: 12px;
      max-width: 600px;
      width: 100%;
      font-size: 17px;
      line-height: 1.6;
      white-space: pre-wrap;
      display: none;
    }

    footer {
      text-align: center;
      padding: 20px;
      font-size: 14px;
      color: #666;
      border-top: 1px solid #ddd;
    }
  </style>
</head>
<body>
  <header>
    <h1>بوست ذكي</h1>
  </header>

  <main>
    <input id="apiKey" type="text" placeholder="أدخل مفتاح OpenAI API" />
    <textarea id="inputText" placeholder="اكتب فكرة البوست..."></textarea>
    <button onclick="generatePost()">توليد المنشور</button>
    <div id="postOutput" class="post"></div>
  </main>

  <footer>
    &copy; 2025 منذر. جميع الحقوق محفوظة.
  </footer>

  <script>
    async function generatePost() {
      const apiKey = document.getElementById("apiKey").value.trim();
      const input = document.getElementById("inputText").value.trim();
      const output = document.getElementById("postOutput");

      if (!apiKey || !input) {
        alert("رجاءً أدخل المفتاح والفكرة!");
        return;
      }

      output.style.display = "block";
      output.textContent = "جاري التوليد...";

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
        output.textContent = data.choices[0].message.content.trim();
      } else {
        output.textContent = "حصل خطأ أثناء التوليد. تأكد من المفتاح.";
      }
    }
  </script>
</body>
</html>
