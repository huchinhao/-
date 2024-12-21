document.getElementById('registrationForm').addEventListener('submit', async (event) => {
    event.preventDefault(); // 防止表單默認提交行為

    const formData = new FormData(event.target); // 獲取表單數據
    const payload = {
        name: formData.get('name'),
        phone: formData.get('phone'),
        email: formData.get('email'),
    };

    try {
        const response = await fetch('[YOUR_GOOGLE_SCRIPT_URL](https://script.google.com/macros/library/d/1q3lRJF7uZHiR26TWjVqLEAoRvkuhXQ65wGm5RaSINPxKO9wWz9gpUyBz/1)', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json', // 確保格式是 JSON
            },
            body: JSON.stringify(payload), // 將資料轉換為 JSON 字串
        });

        const result = await response.json(); // 從伺服器獲取回應
        document.getElementById('responseMessage').textContent = result.message;
    } catch (error) {
        console.error('Error submitting form:', error);
        document.getElementById('responseMessage').textContent = '提交失敗，請稍後再試！';
    }
});
