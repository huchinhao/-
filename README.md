<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>報名表單</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      padding: 20px;
    }
    form {
      max-width: 400px;
      margin: auto;
    }
    label {
      display: block;
      margin-bottom: 8px;
      font-weight: bold;
    }
    input[type="text"], input[type="email"], input[type="tel"] {
      width: 100%;
      padding: 8px;
      margin-bottom: 20px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    button {
      background-color: #4CAF50;
      color: white;
      padding: 10px 15px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover {
      background-color: #45a049;
    }
    .message {
      margin-top: 20px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>報名表單</h1>
  <form id="registrationForm">
    <label for="name">姓名：</label>
    <input type="text" id="name" name="name" required>
    <br>
    <label for="phone">連絡電話：</label>
    <input type="text" id="phone" name="phone" required>
    <br>
    <label for="email">EMAIL：</label>
    <input type="email" id="email" name="email" required>
    <br>
    <button type="submit">送出</button>
  </form>
  <p id="responseMessage" class="message"></p>

  <script>
    document.getElementById('registrationForm').addEventListener('submit', async (event) => {
      event.preventDefault(); // 防止表單預設提交行為

      const formData = new FormData(event.target);
      const payload = {
        name: formData.get('name'),
        phone: formData.get('phone'),
        email: formData.get('email'),
      };

      try {
        const response = await fetch('https://script.google.com/macros/s/AKfycbzXCqY8brgkKdKtraOPLTYrWzTCsVmc6XTtZ-7zu_HrtfPYCYRnbfKYclXS9t-Qqdg2oA/exec', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload),
        });

        const result = await response.json(); // 從伺服器獲取 JSON 回應
        document.getElementById('responseMessage').textContent = result.message;
      } catch (error) {
        console.error('Error submitting form:', error);
        document.getElementById('responseMessage').textContent = '提交失敗，請稍後再試！';
      }
    });
  </script>
</body>
</html>
