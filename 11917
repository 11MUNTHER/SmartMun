<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>مولّد البوستات العدني ✨</title>
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
      font-size: 32px;
      color: #ffcc00;
    }

    textarea {
      width: 100%;
      max-width: 500px;
      height: 120px;
      padding: 15px;
      font-size: 16px;
      border-radius: 12px;
      border: none;
      resize: none;
      outline: none;
      background: #2c2c54;
      color: white;
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
    }

    @media (max-width: 600px) {
      textarea, .post {
        font-size: 16px;
      }
    }
  </style>
</head>
<body>
  <h1>✍️ اكتب بوستك العدني الحماسي</h1>
  <textarea id="inputText" placeholder="اكتب موضوع البوست هنا..."></textarea>
  <button onclick="generatePost()">🎯 توليد البوست</button>
  <div id="postOutput" class="post" style="display:none;"></div>

  <script>
    function generatePost() {
      const input = document.getElementById("inputText").value.trim();
      const output = document.getElementById("postOutput");
      if (input === "") {
        output.style.display = "none";
        return;
      }
      output.innerText = `🚀 منشور جديد:\n\n${input}`;
      output.style.display = "block";
    }
  </script>
</body>
</html>
