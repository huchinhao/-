function doPost(e) {
  try {
    const data = JSON.parse(e.postData.contents); // 解析 JSON 資料
    const sheet = SpreadsheetApp.openById('YOUR_SPREADSHEET_ID').getSheetByName('Sheet1'); // 替換為試算表 ID 和名稱

    // 確保試算表有標題行
    if (sheet.getLastRow() === 0) {
      sheet.appendRow(['姓名', '連絡電話', 'EMAIL', '填表日期', 'IP 位置']);
    }

    // 獲取資料
    const name = data.name || '未知';
    const phone = data.phone || '未知';
    const email = data.email || '未知';
    const date = new Date();
    const ip = e.parameter['x-forwarded-for'] || '未知 IP';

    // 驗證重複提交
    const existingData = sheet.getDataRange().getValues();
    const emailExists = existingData.some(row => row[2] === email);
    if (emailExists) {
      return ContentService.createTextOutput(
        JSON.stringify({ success: false, message: '此 EMAIL 已提交過！' })
      ).setMimeType(ContentService.MimeType.JSON);
    }

    // 寫入資料
    sheet.appendRow([name, phone, email, date, ip]);

    // 返回成功回應
    return ContentService.createTextOutput(
      JSON.stringify({ success: true, message: '提交成功！' })
    ).setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    // 返回錯誤訊息
    return ContentService.createTextOutput(
      JSON.stringify({ success: false, message: '提交失敗，請稍後再試！', error: error.message })
    ).setMimeType(ContentService.MimeType.JSON);
  }
}
