<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>مولد البوست الذكي - منشورات بعدنية بعصرية</title>
  <style>
    /* خطوط */
    @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap');

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      background: linear-gradient(135deg, #4e54c8, #8f94fb);
      font-family: 'Cairo', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
      color: #fff;
      flex-direction: column;
    }

    header {
      text-align: center;
      margin-bottom: 40px;
    }

    header h1 {
      font-size: 3rem;
      margin: 0;
      font-weight: 700;
      letter-spacing: 2px;
      text-shadow: 0 2px 8px rgba(0,0,0,0.3);
    }

    header p {
      margin-top: 10px;
      font-size: 1.2rem;
      color: #d6d9ffcc;
    }

    textarea {
      width: 100%;
      max-width: 600px;
      min-height: 120px;
      padding: 20px;
      font-size: 1.1rem;
      border: none;
      border-radius: 15px;
      resize: vertical;
      box-shadow: 0 8px 20px rgba(0,0,0,0.2);
      outline: none;
      transition: box-shadow 0.3s ease;
      font-family: 'Cairo', sans-serif;
      background-color: #5a63d8;
      color: white;
    }

    textarea:focus {
      box-shadow: 0 0 12px 3px #b0aaff;
      background-color: #6f77e9;
    }

    button {
      margin-top: 25px;
      background: linear-gradient(45deg, #f3a683, #f7d794);
      border: none;
      padding: 15px 45px;
      border-radius: 30px;
      font-size: 1.3rem;
      color: #3a2f0b;
      font-weight: 700;
      cursor: pointer;
      box-shadow: 0 6px 12px rgba(243,166,131,0.6);
      transition: transform 0.25s ease, box-shadow 0.25s ease;
    }

    button:hover {
      transform: scale(1.05);
      box-shadow: 0 10px 25px rgba(243,166,131,0.9);
    }

    #postOutput {
      margin-top: 35px;
      max-width: 600px;
      background-color: #382f6c;
      border-radius: 15px;
      padding: 25px;
      font-size: 1.2rem;
      box-shadow: 0 8px 18px rgba(0,0,0,0.4);
      line-height: 1.6;
      white-space: pre-wrap;
      display: none;
    }

    footer {
      margin-top: 50px;
      font-size: 0.9rem;
      color: #ccc;
      text-align: center;
    }

    @media (max-width: 640px) {
      header h1 {
        font-size: 2.2rem;
      }
      textarea, #postOutput {
        max-width: 100%;
      }
    }
  </style>
</head>
<body>
  <header>
    <h1>مولد البوست الذكي</h1>
    <p>اكتب فكرة بسيطة وخلك جاهز لمنشور بعدني عفوي وعصري</p>
  </header>

  <textarea id="inputText" placeholder="اكتب فكرتك هنا..."></textarea>
  <button onclick="generatePost()">🎯 طلع لي بوست</button>

  <div id="postOutput" class="post"></div>

  <footer>تصميم وبرمجة بواسطة منذر ❤️</footer>

  <script>
    async function generatePost() {
      const input = document.getElementById("inputText").value.trim();
      const output = document.getElementById("postOutput");

      if (!input) {
        alert("رجاءً اكتب فكرتك!");
        return;
      }

      output.style.display = "block";
      output.textContent = "⏳ شغال على توليد البوست...";

      try {
        const response = await fetch("http://localhost:3000/generate-post", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ input }),
        });
        const data = await response.json();

        if (data.post) {
          output.textContent = data.post;
        } else {
          output.textContent = "⚠️ في مشكلة في التوليد، جرب مرة ثانية.";
        }
      } catch (e) {
        output.textContent = "⚠️ فشل الاتصال بالسيرفر، تأكد انه شغال.";
      }
    }
  </script>
</body>
</html>
