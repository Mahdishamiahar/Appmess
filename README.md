📁 VIPZEXNET (WebApp)

├── index.html ├── chat.html ├── js/ │   ├── firebase.js │   ├── auth.js │   └── chat.js ├── assets/ │   └── style.css ├── images/ │   └── logo.png (اختیاری) ├── README.md

✅ [index.html] - صفحه ورود با شماره موبایل

<!DOCTYPE html><html dir="rtl" lang="fa">
<head>
  <meta charset="UTF-8">
  <title>ورود - VIPZEXNET</title>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth-compat.js"></script>
  <link rel="stylesheet" href="assets/style.css">
</head>
<body>
  <h2>ورود با شماره موبایل</h2>
  <input type="text" id="phone" placeholder="مثلاً +989123456789"><br>
  <div id="recaptcha-container"></div>
  <button onclick="sendCode()">ارسال کد</button><br><br>
  <input type="text" id="code" placeholder="کد تایید"><br>
  <button onclick="verifyCode()">تأیید</button>  <script src="js/firebase.js"></script>  <script src="js/auth.js"></script></body>
</html>✅ [chat.html] - صفحه چت

<!DOCTYPE html><html dir="rtl" lang="fa">
<head>
  <meta charset="UTF-8">
  <title>چت - VIPZEXNET</title>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database-compat.js"></script>
  <link rel="stylesheet" href="assets/style.css">
</head>
<body>
  <h2>چت خصوصی</h2>
  <div id="chat-box"></div>
  <input type="text" id="message" placeholder="پیام بنویس..."><button onclick="sendMessage()">ارسال</button>  <script src="js/firebase.js"></script>  <script src="js/chat.js"></script></body>
</html>✅ [js/firebase.js] - تنظیمات Firebase

const firebaseConfig = { apiKey: "AIzaSyA7FvrR4df7bzAf2EvsJoY1BePSvNnQi5c", authDomain: "mahdi-ad5ff.firebaseapp.com", projectId: "mahdi-ad5ff", storageBucket: "mahdi-ad5ff.appspot.com", messagingSenderId: "602278205513", appId: "1:602278205513:web:f7adc45a903fa432d58798", measurementId: "G-V3W4QCGP1V" }; firebase.initializeApp(firebaseConfig);

✅ [js/auth.js] - ورود با شماره موبایل

window.recaptchaVerifier = new firebase.auth.RecaptchaVerifier('recaptcha-container', { size: 'normal' }); function sendCode() { const phone = document.getElementById("phone").value; firebase.auth().signInWithPhoneNumber(phone, window.recaptchaVerifier) .then((confirmationResult) => { window.confirmationResult = confirmationResult; alert("کد ارسال شد"); }).catch((error) => { alert("خطا: " + error.message); }); } function verifyCode() { const code = document.getElementById("code").value; window.confirmationResult.confirm(code).then((result) => { alert("ورود موفق بود!"); window.location.href = "chat.html"; }).catch((error) => { alert("کد نادرست یا منقضی شده است."); }); }

✅ [js/chat.js] - چت ساده

const db = firebase.database(); const chatBox = document.getElementById("chat-box"); function sendMessage() { const msg = document.getElementById("message").value; const user = firebase.auth().currentUser; if (msg && user) { db.ref("messages").push({ uid: user.uid, text: msg, time: new Date().toLocaleTimeString() }); document.getElementById("message").value = ""; } } db.ref("messages").on("child_added", (snapshot) => { const data = snapshot.val(); const p = document.createElement("p"); p.innerText = ${data.time} - ${data.text}; chatBox.appendChild(p); });

✅ [assets/style.css] - استایل ساده

body { font-family: Tahoma; background: #f5f7fa; text-align: center; padding: 40px; } input, button { padding: 10px; margin: 5px; font-size: 16px; } #chat-box { border: 1px solid #ccc; padding: 10px; height: 300px; overflow-y: scroll; background: white; margin-bottom: 10px; }
