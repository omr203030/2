<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام نقاط طه متعب 🚀</title>
    <!-- استدعاء مكتبات Firebase -->
    <script src="https://www.gstatic.com"></script>
    <script src="https://www.gstatic.com"></script>
    <style>
        body { font-family: 'Segoe UI', sans-serif; background-color: #f0f2f5; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
        .card { background: white; padding: 30px; border-radius: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); width: 100%; max-width: 400px; text-align: center; }
        h2 { color: #4a148c; margin-bottom: 25px; border-bottom: 2px solid #eee; padding-bottom: 10px; }
        input { width: 100%; padding: 12px; margin: 10px 0; border: 2px solid #ddd; border-radius: 10px; box-sizing: border-box; font-size: 16px; text-align: center; }
        button { width: 100%; padding: 12px; margin: 8px 0; border: none; border-radius: 10px; color: white; font-weight: bold; font-size: 16px; cursor: pointer; transition: 0.3s; }
        .search-btn { background: #007bff; }
        .add-btn { background: #28a745; }
        .sub-btn { background: #dc3545; }
        .result-box { margin-top: 20px; padding: 15px; background: #f9f9f9; border-radius: 12px; display: none; border: 1px solid #eee; }
        .points-display { font-size: 32px; font-bold: bold; color: #28a745; display: block; margin: 10px 0; }
        .loader { color: #777; font-size: 14px; margin-top: 10px; display: none; }
    </style>
</head>
<body>

<div class="card">
    <h2>نظام نقاط طه متعب ✨</h2>
    <input type="tel" id="phone" placeholder="أدخل رقم موبايل العميل">
    <button class="search-btn" onclick="searchUser()">🔍 بحث وتحديث الرصيد</button>
    <div id="loader" class="loader">جاري الاتصال بقاعدة البيانات...</div>

    <div id="userInfo" class="result-box">
        <p>رصيد العميل الحالي:</p>
        <strong id="userPoints" class="points-display">0</strong>
        <input type="number" id="pointsAmount" placeholder="عدد النقاط">
        <button class="add-btn" onclick="updatePoints('add')">➕ إضافة نقاط (شحن)</button>
        <button class="sub-btn" onclick="updatePoints('sub')">🛒 خصم (سوبر ماركت)</button>
    </div>
</div>

<script>
    // ضع رابط قاعدة البيانات الخاص بك هنا (الذي نسخته من Firebase)
    const firebaseConfig = {
        databaseURL: "أكتب_هنا_رابط_قاعدة_بيانات_فيربيز_الخاص_بك" 
    };
    
    // بدء تشغيل Firebase
    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();

    function searchUser() {
        const phone = document.getElementById('phone').value;
        if (!phone) return alert("يرجى إدخال رقم الموبايل!");
        
        document.getElementById('loader').style.display = 'block';
        
        // البحث عن العميل في قاعدة البيانات
        database.ref('customers/' + phone).on('value', (snapshot) => {
            document.getElementById('loader').style.display = 'none';
            document.getElementById('userInfo').style.display = 'block';
            const data = snapshot.val();
            document.getElementById('userPoints').innerText = data ? data.points : 0;
        });
    }

    function updatePoints(action) {
        const phone = document.getElementById('phone').value;
        const amount = parseInt(document.getElementById('pointsAmount').value);
        const currentPoints = parseInt(document.getElementById('userPoints').innerText);

        if (isNaN(amount) || amount <= 0) return alert("يرجى إدخال عدد نقاط صحيح!");

        let finalPoints = (action === 'add') ? currentPoints + amount : currentPoints - amount;

        if (finalPoints < 0) return alert("خطأ: رصيد العميل لا يكفي للخصم!");

        // حفظ البيانات في السحاب (لحظياً)
        database.ref('customers/' + phone).set({
            points: finalPoints,
            last_update: new Date().toLocaleString()
        }).then(() => {
            alert("تم التحديث بنجاح! الرصيد الجديد: " + finalPoints);
            document.getElementById('pointsAmount').value = "";
        }).catch(error => {
            alert("حدث خطأ في الاتصال: " + error.message);
        });
    }
</script>

</body>
</html>
