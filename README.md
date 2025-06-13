<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>مولد البوست الذكي</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap');

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      background-color: #fff;
      font-family: 'Cairo', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
      color: #111;
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
      color: #000;
    }

    header p {
      margin-top: 10px;
      font-size: 1.2rem;
      color: #555;
    }

    textarea {
      width: 100%;
      max-width: 600px;
      min-height: 120px;
      padding: 20px;
      font-size: 1.1rem;
      border: 2px solid #111;
      border-radius: 15px;
      resize: vertical;
      outline: none;
      font-family: 'Cairo', sans-serif;
      background-color: #fff;
      color: #111;
      transition: border-color 0.3s ease;
    }

    textarea:focus {
      border-color: #000;
      box-shadow: 0 0 8px rgba(0,0,0
