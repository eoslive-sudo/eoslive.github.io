<html lang="ko">
<head>
<meta charset="UTF-8" />
<title>로트번호 사용기한 계산기</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 20px;
    background-color: #f9fafd;
    color: #222;
    padding: 30px;
    max-width: 600px;
    margin: auto;
  }
  h2 {
    text-align: center;
    color: #004080;
    margin-bottom: 30px;
  }
  label {
    display: block;
    margin-bottom: 8px;
    font-weight: 600;
  }
  input[type="text"] {
    width: 100%;
    padding: 8px 12px;
    font-size: 20px;
    border: 2px solid #ccc;
    border-radius: 6px;
    box-sizing: border-box;
    transition: border-color 0.3s ease;
  }
  input[type="text"]:focus {
    border-color: #004080;
    outline: none;
  }
  button {
    display: block;
    width: 100%;
    margin-top: 15px;
    padding: 12px;
    font-size: 20px;
    background-color: #004080;
    color: white;
    font-weight: 700;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  button:hover {
    background-color: #003060;
  }
  .result {
    margin-top: 30px;
    background: #e6f0ff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 8px rgba(0,64,128,0.15);
  }
  .expiry-date {
    color: #002060;
    font-weight: bold;
    font-size: 22px;
    margin-top: 8px;
  }
  .error {
    color: #d93025;
    font-weight: 600;
  }
</style>
</head>
<body>

  <h2>로트번호 사용기한 계산기</h2>

  <label for="lotNumber">로트번호 입력 (7자리, 예: A15Y001)</label>
  <input type="text" id="lotNumber" maxlength="7" placeholder="A부터 L (1~12월), 날짜2자리, 년도(X,Y,Z), 배치3자리">

  <button onclick="calculateExpiryFromLot()">날짜로 계산</button>

  <div class="result" id="result"></div>

  <script>
    function monthCharToNumber(char) {
      const code = char.toUpperCase().charCodeAt(0);
      if (code >= 65 && code <= 76) return code - 64; // A=1 ~ L=12
      return null;
    }

    function yearCharToYear(char) {
      switch(char.toUpperCase()) {
        case 'X': return 2024;
        case 'Y': return 2025;
        case 'Z': return 2026;
        default: return null;
      }
    }

    function addMonthsAndSubtractOneDay(date, months) {
      const tempDate = new Date(date);
      tempDate.setMonth(tempDate.getMonth() + months);
      tempDate.setDate(tempDate.getDate() - 1);
      return tempDate;
    }

    function formatDate(date) {
      return date.getFullYear() + '-' +
             String(date.getMonth() + 1).padStart(2, '0') + '-' +
             String(date.getDate()).padStart(2, '0');
    }

    function calculateExpiryFromLot() {
      const lot = document.getElementById('lotNumber').value.trim();
      const resultDiv = document.getElementById('result');
      resultDiv.innerHTML = '';

      if (lot.length !== 7) {
        resultDiv.innerHTML = '<p class="error">로트번호는 정확히 7자리여야 합니다.</p>';
        return;
      }

      const month = monthCharToNumber(lot[0]);
      if (month === null) {
        resultDiv.innerHTML = '<p class="error">월 문자는 A부터 L (1~12월)까지만 허용됩니다.</p>';
        return;
      }

      const day = parseInt(lot.substring(1, 3), 10);
      if (isNaN(day) || day < 1 || day > 31) {
        resultDiv.innerHTML = '<p class="error">2~3번째 문자는 유효한 날짜여야 합니다.</p>';
        return;
      }

      const year = yearCharToYear(lot[3]);
      if (year === null) {
        resultDiv.innerHTML = '<p class="error">년도 문자는 X, Y, Z 중 하나여야 합니다.</p>';
        return;
      }

      const baseDate = new Date(year, month - 1, day);
      if (baseDate.getMonth() !== month - 1 || baseDate.getDate() !== day) {
        resultDiv.innerHTML = '<p class="error">유효하지 않은 날짜입니다.</p>';
        return;
      }

      const expiry36 = addMonthsAndSubtractOneDay(baseDate, 36);
      const expiry30 = addMonthsAndSubtractOneDay(baseDate, 30);
      const expiry24 = addMonthsAndSubtractOneDay(baseDate, 24);

      resultDiv.innerHTML = `
        <p>해석된 기준 날짜: <span class="expiry-date">${formatDate(baseDate)}</span></p>
        <p>36개월 - 1일 사용기한: <span class="expiry-date">${formatDate(expiry36)}</span></p>
        <p>30개월 - 1일 사용기한: <span class="expiry-date">${formatDate(expiry30)}</span></p>
        <p>24개월 - 1일 사용기한: <span class="expiry-date">${formatDate(expiry24)}</span></p>
      `;
    }
  </script>

</body>






<html lang="ko">
<head>
<meta charset="UTF-8" />
<title>사용기한 계산기</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 20px;
    background-color: #f9fafd;
    color: #222;
    padding: 40px;
    max-width: 480px;
    margin: auto;
  }
  h2 {
    text-align: center;
    margin-bottom: 40px;
    color: #004080;
  }
  label {
    display: block;
    margin-bottom: 10px;
    font-weight: 600;
  }
  input[type="date"] {
    width: 100%;
    padding: 10px 14px;
    font-size: 20px;
    border: 2px solid #ccc;
    border-radius: 8px;
    box-sizing: border-box;
    transition: border-color 0.3s ease;
    margin-bottom: 15px;
  }
  input[type="date"]:focus {
    border-color: #004080;
    outline: none;
  }
  button {
    width: 100%;
    padding: 14px 0;
    font-size: 22px;
    font-weight: 700;
    background-color: #004080;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  button:hover {
    background-color: #002a5a;
  }
  .result {
    margin-top: 30px;
    background: #e6f0ff;
    padding: 22px 28px;
    border-radius: 12px;
    box-shadow: 0 0 12px rgba(0,64,128,0.15);
    font-weight: 600;
  }
  .expiry-date {
    color: #002060;
    font-size: 24px;
    font-weight: 700;
    margin-top: 8px;
  }
  .error {
    color: #d93025;
    font-weight: 700;
  }
</style>
</head>
<body>

<h2>날짜 입력 사용기한 계산기</h2>

<label for="inputDate">기준 날짜 입력</label>
<input type="date" id="inputDate" />
<button onclick="calculateExpiry()">사용기한 계산</button>

<div class="result" id="result"></div>

<script>
  function addMonthsAndSubtractOneDay(date, months) {
    const tempDate = new Date(date);
    tempDate.setMonth(tempDate.getMonth() + months);
    tempDate.setDate(tempDate.getDate() - 1);
    return tempDate;
  }

  function formatDate(date) {
    return date.getFullYear() + '-' +
           String(date.getMonth() + 1).padStart(2, '0') + '-' +
           String(date.getDate()).padStart(2, '0');
  }

  function calculateExpiry() {
    const input = document.getElementById('inputDate').value;
    const resultDiv = document.getElementById('result');
    resultDiv.innerHTML = '';

    if (!input) {
      resultDiv.innerHTML = '<p class="error">날짜를 선택해주세요.</p>';
      return;
    }

    const baseDate = new Date(input);

    const expiry36 = addMonthsAndSubtractOneDay(baseDate, 36);
    const expiry30 = addMonthsAndSubtractOneDay(baseDate, 30);
    const expiry24 = addMonthsAndSubtractOneDay(baseDate, 24);

    resultDiv.innerHTML = `
      <p>입력 날짜: <span class="expiry-date">${formatDate(baseDate)}</span></p>
      <p>36개월 - 1일 사용기한: <span class="expiry-date">${formatDate(expiry36)}</span></p>
      <p>30개월 - 1일 사용기한: <span class="expiry-date">${formatDate(expiry30)}</span></p>
      <p>24개월 - 1일 사용기한: <span class="expiry-date">${formatDate(expiry24)}</span></p>
    `;
  }
</script>

</body>
