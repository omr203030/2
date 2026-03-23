<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام نقاط طه متعب 🚀</title>
    <script src="https://www.gstatic.com"></script>
    <script src="https://www.gstatic.com"></script>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: #eef2f7; text-align: center; padding: 20px; }
        .card { max-width: 450px; background: white; padding: 30px; border-radius: 20px; margin: auto; box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
        h2 { color: #6f42c1; margin-bottom: 20px; border-bottom: 2px solid #f0f0f0; padding-bottom: 10px; }
        input { width: 100%; padding: 12px; margin: 10px 0; border: 2px solid #ddd; border-radius: 10px; box-sizing: border-box; font-size: 16px; text-align: center; }
        button { width: 100%; padding: 12px; margin: 5px 0; border: none; border-radius: 10px; color: white; font-weight: bold; font-size: 16px; cursor: pointer; transition: 0.3s; }
        .search-btn { background: #007bff; }
        .add-btn { background: #28a745; }
        .sub-btn { background: #dc3545; }
        .status { margin-top: 20px; padding: 15px; background: #f8f9fa; border-radius: 10px; display: none; }
        .points-val { font-size: 28px; color: #28a745; display: block; margin: 10px 0; }
    </style>
</head>
<body>

<div class="card">
    <h2>نظام نقاط طه متعب ✨</h2>
    <input type="tel" id="phone" placeholder="أدخل رقم موبايل العميل">
    <button class="search-btn" onclick="searchUser()">🔍 بحث وتحديث</button>

    <div id="userInfo" class="status">
        <p>رصيد النقاط الحالي للعميل:</p>
        <strong id="userPoints" class="points-val">0</strong>
        <input type="number" id="pointsAmount" placeholder="أدخل عدد النقاط">
        <button class="add-btn" onclick="updatePoints('add')">➕ إضافة نقاط شحن</button>
        <button class="sub-btn" onclick="updatePoints('sub')">🛒 استبدال في السوبر ماركت</button>
    </div>
</div>

<script>
    // إعدادات Firebase (يجب استبدالها ببيانات مشروعك لتفعيل الربط الحقيقي)
    const firebaseConfig = {
        databaseURL: "https://taha-points-default-rtdb.firebaseio.com" 
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    function searchUser() {
        let phone = document.getElementById('phone').value;
        if (!phone) return alert("يرجى إدخال الرقم");

        db.ref('users/' + phone).on('value', (snapshot) => {
            document.getElementById('userInfo').style.display = 'block';
            let data = snapshot.val();
            document.getElementById('userPoints').innerText = data ? data.points : 0;
        });
    }

    function updatePoints(type) {
        let phone = document.getElementById('phone').value;
        let amount = parseInt(document.getElementById('pointsAmount').value);
        let currentPoints = parseInt(document.getElementById('userPoints').innerText);

        if (isNaN(amount) || amount <= 0) return alert("أدخل رقم صحيح");

        let newPoints = (type === 'add') ? currentPoints + amount : currentPoints - amount;

        if (newPoints < 0) return alert("الرصيد لا يكفي للخصم!");

        db.ref('users/' + phone).set({ points: newPoints })
          .then(() => {
              alert("تم التحديث بنجاح ✅");
              document.getElementById('pointsAmount').value = "";
          });
    }
</script>
</body>
</html>
