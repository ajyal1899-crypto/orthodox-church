<!doctype html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>كنيسة العدرا والأنبا أثناسيوس</title>
  <style>
    :root{--accent:#2b6cb0;--bg:#f7fafc}
    *{box-sizing:border-box;font-family:Tahoma, Arial, "Segoe UI", sans-serif}
    body{margin:0;background:var(--bg);color:#111}
    header{background:var(--accent);color:#fff;padding:16px 12px;text-align:center}
    header h1{margin:0;font-size:20px}
    .container{max-width:980px;margin:20px auto;padding:18px;background:#fff;border-radius:10px;box-shadow:0 6px 18px rgba(0,0,0,0.06)}
    form label{display:block;margin-bottom:8px}
    input,select,textarea{width:100%;padding:10px;border-radius:8px;border:1px solid #ddd}
    button{padding:10px 14px;border-radius:8px;border:none;background:var(--accent);color:#fff;cursor:pointer}
    .view{display:none}
    .view.active{display:block}
  </style>
</head>
<body>
  <header>
    <h1>كنيسة العدرا والأنبا أثناسيوس</h1>
  </header>
  <div class="container">
    <!-- Role Selection -->
    <section id="roleSelect" class="view active">
      <h2>اختر نوع الحساب</h2>
      <button id="adminBtn">أدمن</button>
      <button id="priestBtn">كاهن</button>
      <button id="userBtn">زائر</button>
    </section>

    <!-- Password Entry -->
    <section id="passwordEntry" class="view">
      <h2 id="passwordTitle"></h2>
      <form id="passwordForm">
        <label>كلمة المرور<input type="password" id="rolePassword" required></label>
        <div style="margin-top:12px"><button type="submit">دخول</button></div>
      </form>
    </section>

    <!-- Main Views -->
    <section id="main" class="view">
      <h2 id="mainTitle"></h2>
      <div id="mainContent"></div>
    </section>
  </div>

  <script>
    const roleSelect = document.getElementById('roleSelect');
    const passwordEntry = document.getElementById('passwordEntry');
    const mainView = document.getElementById('main');
    const passwordTitle = document.getElementById('passwordTitle');
    const mainTitle = document.getElementById('mainTitle');
    const mainContent = document.getElementById('mainContent');
    const rolePassword = document.getElementById('rolePassword');
    const passwordForm = document.getElementById('passwordForm');

    const adminPassword = 'admin123';
    const priestPasswords = {"ابونا مينا خلف":"mina123","ابونا عبد المسيح":"abdo123","ابونا بولا":"bola123","ابونا ذكريا":"zak123","ابونا يوسف":"youssef123"};
    let currentRole = '';
    let currentPriest = '';

    function showView(view) {
      document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
      view.classList.add('active');
    }

    document.getElementById('adminBtn').addEventListener('click', () => {
      currentRole = 'admin';
      passwordTitle.textContent = 'أدخل كلمة مرور الأدمن';
      showView(passwordEntry);
      rolePassword.focus();
    });

    document.getElementById('priestBtn').addEventListener('click', () => {
      currentRole = 'priest';
      let priestName = prompt('أدخل اسم الكاهن');
      if (priestName && priestPasswords[priestName]) {
        currentPriest = priestName;
        passwordTitle.textContent = `أدخل كلمة مرور ${priestName}`;
        showView(passwordEntry);
        rolePassword.focus();
      } else {
        alert('اسم الكاهن غير صحيح');
      }
    });

    document.getElementById('userBtn').addEventListener('click', () => {
      currentRole = 'user';
      mainTitle.textContent = 'مرحبا بالزائر';
      mainContent.innerHTML = '<p>يمكنك تصفح الأخبار ومواعيد القداسات.</p>';
      showView(mainView);
    });

    passwordForm.addEventListener('submit', e => {
      e.preventDefault();
      const pass = rolePassword.value.trim();
      if (currentRole === 'admin' && pass === adminPassword) {
        mainTitle.textContent = 'لوحة الأدمن';
        mainContent.innerHTML = '<p>هنا يمكن للأدمن إدارة الموقع، الأخبار، والحجوزات.</p>';
        showView(mainView);
      } else if (currentRole === 'priest' && priestPasswords[currentPriest] === pass) {
        mainTitle.textContent = `لوحة ${currentPriest}`;
        mainContent.innerHTML = '<p>يمكن للكاهن الاطلاع على الحجوزات الخاصة به ونشر الأخبار.</p>';
        showView(mainView);
      } else {
        alert('كلمة المرور غير صحيحة');
      }
      rolePassword.value = '';
    });
  </script>
</body>
</html>
