<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>SmartMun - منشورات ذكية باللهجة العدنية</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    body {
      background: #fff;
      color: #000;
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
      color: #000;
    }

    input, textarea, select, button {
      width: 100%;
      max-width: 500px;
      border-radius: 12px;
      border: 1px solid #000;
      padding: 10px 15px;
      font-size: 16px;
      outline: none;
      box-sizing: border-box;
      font-family: 'Cairo', sans-serif;
      color: #000;
      background: #fff;
      margin-top: 10px;
      transition: border-color 0.3s ease;
    }

    input:focus, textarea:focus, select:focus {
      border-color: #555;
    }

    button {
      background: #000;
      color: #fff;
      font-weight: bold;
      cursor: pointer;
      border: none;
      border-radius: 20px;
      padding: 12px 24px;
      margin-top: 20px;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background: #444;
    }

    .post {
      margin-top: 30px;
      background-color: #f0f0f0;
      padding: 20px;
      border-radius: 16px;
      max-width: 500px;
      width: 100%;
      font-size: 18px;
      line-height: 1.6;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      white-space: pre-wrap;
      color: #000;
      min-height: 80px;
    }

    #copyBtn {
      display: none;
      width: auto;
      padding: 8px 20px;
      margin-top: 10px;
      border-radius: 12px;
    }

    h3 {
      max-width: 500px;
      width: 100%;
      margin-top: 40px;
      color: #000;
    }

    #historyList {
      max-width: 500px;
      width: 100%;
      list-style: none;
      padding: 0;
      margin-top: 10px;
      color: #222;
    }

    #historyList li {
      padding: 10px 8px;
      border-bottom: 1px solid #ccc;
      cursor: pointer;
      transition: background-color 0.2s ease;
    }

    #historyList li:hover {
      background-color: #eee;
    }

    footer {
      margin-top: 50px;
      color: #555;
    }

    footer a {
      color: #555;
      text-decoration: none;
    }

    footer a:hover {
      text-decoration: underline;
    }

    @media (max-width: 600px) {
      h1 {
        font-size: 24px;
      }
      input, textarea, select, button {
        font-size: 14px;
      }
    }
  </style>
</head>
<body>
  <h1>SmartMun - منشورات ذكية باللهجة العدنية</h1>

  <input id="apiKey" type="text" placeholder="أدخل مفتاح OpenAI API هنا (سرّياً)" autocomplete="off" />
  <textarea id="inputText" placeholder="اكتب فكرة البوست أو كلمة بسيطة..."></textarea>

  <select id="tone" aria-label="اختيار نغمة البوست">
    <option value="عفوي">نغمة عفوية</option>
    <option value="رسمي">نغمة رسمية</option>
    <option value="تفاعلي">نغمة تفاعلية</option>
    <option value="عاطفي">نغمة عاطفية</option>
  </select>

  <button onclick="generatePost()">توليد منشور ذكي</button>
  <button id="copyBtn">نسخ المنشور</button>

  <div id="postOutput" class="post" style="display:none;"></div>

  <h3>المنشورات السابقة:</h3>
  <ul id="historyList"></ul>

  <footer>
    <p>© 2025 SmartMun - <a href="about.html">حول</a></p>
  </footer>

  <script>
    async function generatePost() {
      const apiKey = document.getElementById("apiKey").value.trim();
      const input = document.getElementById("inputText").value.trim();
      const tone = document.getElementById("tone").value;
      const output = document.getElementById("postOutput");
      const copyBtn = document.getElementById("copyBtn");

      if (!apiKey || !input) {
        alert("رجاءً أدخل الـ API Key والفكرة!");
        return;
      }

      output.innerText = "⏳ جاري توليد البوست...";
      output.style.display = "block";
      copyBtn.style.display = "none";

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
              { role: "system", content: `اكتب منشور بنغمة "${tone}" وباللهجة العدنية حسب الموضوع.` },
              { role: "user", content: input }
            ],
            max_tokens: 150,
          }),
        });

        const data = await response.json();

        if (data.choices && data.choices.length > 0) {
          output.innerText = data.choices[0].message.content.trim();
          copyBtn.style.display = "inline-block";

          // حفظ المنشور
          let history = JSON.parse(localStorage.getItem("posts") || "[]");
          history.unshift(output.innerText);
          localStorage.setItem("posts", JSON.stringify(history));
          updateHistory();
        } else {
          output.innerText = "⚠️ حصلت مشكلة في التوليد... تأكد من الـ API Key.";
          copyBtn.style.display = "none";
        }
      } catch (error) {
        output.innerText = "⚠️ خطأ في الاتصال، حاول مرة أخرى.";
        copyBtn.style.display = "none";
      }
    }

    // زر النسخ
    document.getElementById("copyBtn").onclick = () => {
      const text = document.getElementById("postOutput").innerText;
      navigator.clipboard.writeText(text);
      alert("✅ تم نسخ المنشور!");
    };

    // عرض التاريخ
    function updateHistory() {
      const list = document.getElementById("historyList");
      list.innerHTML = "";
      let posts = JSON.parse(localStorage.getItem("posts") || "[]");
      posts.slice(0, 5).forEach((post) => {
        const li = document.createElement("li");
        li.innerText = post;
        li.title = "اضغط لنسخ";
        li.onclick = () => {
          navigator.clipboard.writeText(post);
          alert("✅ تم نسخ المنشور من التاريخ!");
        };
        list.appendChild(li);
      });
    }

    // شغل التحديث أول ما تحمل الصفحة
    updateHistory();
  </script>
</body>
</html>
