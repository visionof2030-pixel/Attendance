
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سجل متابعة الطلاب</title>
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
<style>
    body {
        font-family: 'Tajawal', sans-serif;
        margin: 0;
        padding: 0;
        background-color: #f4f6f8;
    }
    header {
        background-color: #6ec9ff;
        color: #fff;
        text-align: center;
        padding: 15px 0;
        font-size: 1.2rem;
        font-weight: bold;
    }
    .controls {
        display: flex;
        justify-content: space-between;
        padding: 10px;
        flex-wrap: wrap;
    }
    .controls button, .controls input {
        padding: 8px 12px;
        margin: 5px 0;
        border: none;
        border-radius: 5px;
        font-size: 0.9rem;
    }
    .controls button {
        background-color: #6ec9ff;
        color: #fff;
        cursor: pointer;
    }
    #adminPanel {
        display: none;
        padding: 10px;
        background-color: #e0f0ff;
        border-radius: 5px;
        margin: 5px 0;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        font-size: 0.85rem;
        background-color: #fff;
        margin-bottom: 20px;
    }
    th, td {
        border: 1px solid #ccc;
        text-align: center;
        padding: 6px;
    }
    th {
        background-color: #6ec9ff;
        color: #fff;
    }
    td button {
        width: 30px;
        height: 30px;
        font-size: 1rem;
        cursor: pointer;
        border-radius: 50%;
        border: none;
    }
    .present { background-color: #4caf50; color: #fff; }
    .absent { background-color: #f44336; color: #fff; }
    .star { cursor: pointer; color: gold; font-weight: bold; }
</style>
</head>
<body>

<header>سجل متابعة الطلاب</header>

<div class="controls">
    <button onclick="exportToExcel()">تصدير Excel</button>
    <input type="password" id="adminPass" placeholder="ادخل كلمة السر للإدارة">
    <button onclick="unlockAdmin()">فتح الإدارة</button>
</div>

<div id="adminPanel">
    <button onclick="addStudent()">إضافة طالب</button>
    <button onclick="randomAttendance()">تحضير عشوائي</button>
    <button onclick="transferStudent()">نقل طالب</button>
</div>

<table id="studentTable">
    <thead>
        <tr>
            <th>اسم الطالب</th>
            <th>حضور</th>
            <th>واجبات</th>
            <th>مشروعات</th>
            <th>تطبيقات وأنشطة</th>
            <th>مشاركة</th>
            <th>تمييز</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>أحمد علي</td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><span class="star" onclick="toggleStar(this)">☆</span></td>
        </tr>
        <tr>
            <td>سارة محمد</td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
            <td><span class="star" onclick="toggleStar(this)">☆</span></td>
        </tr>
    </tbody>
</table>

<script>
let adminUnlocked = false;

function toggleStatus(btn) {
    if (btn.classList.contains('absent')) {
        btn.classList.remove('absent');
        btn.classList.add('present');
        btn.textContent = '✓';
    } else {
        btn.classList.remove('present');
        btn.classList.add('absent');
        btn.textContent = '×';
    }
}

function unlockAdmin() {
    const pass = document.getElementById('adminPass').value;
    if (pass === '1406') {
        adminUnlocked = true;
        document.getElementById('adminPanel').style.display = 'block';
        alert('تم فتح خصائص الإدارة');
    } else {
        alert('كلمة السر خاطئة');
    }
}

function toggleStar(span) {
    if (!adminUnlocked) return;
    span.textContent = span.textContent === '☆' ? '★' : '☆';
}

function addStudent() {
    const table = document.getElementById('studentTable').tBodies[0];
    const name = prompt("أدخل اسم الطالب الجديد:");
    if (!name) return;
    const tr = document.createElement('tr');
    tr.innerHTML = `
        <td>${name}</td>
        <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
        <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
        <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
        <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
        <td><button class="absent" onclick="toggleStatus(this)">×</button></td>
        <td><span class="star" onclick="toggleStar(this)">☆</span></td>
    `;
    table.appendChild(tr);
}

function randomAttendance() {
    const rows = document.getElementById('studentTable').tBodies[0].rows;
    for (let r of rows) {
        for (let i = 1; i <= 5; i++) {
            const btn = r.cells[i].querySelector('button');
            if (Math.random() > 0.5) {
                btn.classList.remove('absent');
                btn.classList.add('present');
                btn.textContent = '✓';
            } else {
                btn.classList.remove('present');
                btn.classList.add('absent');
                btn.textContent = '×';
            }
        }
    }
}

function transferStudent() {
    alert("خاصية نقل الطالب سيتم تطويرها لاحقًا.");
}

function exportToExcel() {
    const table = document.getElementById("studentTable");
    let tableHTML = `<table border="1" style="border-collapse:collapse; width:100%; font-family:Tajawal; text-align:center; direction:rtl;">
        <thead style="background-color:#6ec9ff; color:#fff;">${table.tHead.innerHTML}</thead>
        <tbody>${table.tBodies[0].innerHTML}</tbody>
    </table>`;

    const html = `
    <html xmlns:o="urn:schemas-microsoft-com:office:office"
          xmlns:x="urn:schemas-microsoft-com:office:excel"
          xmlns="http://www.w3.org/TR/REC-html40">
    <head><meta charset="UTF-8"></head>
    <body>${tableHTML}</body></html>`;

    const blob = new Blob([html], { type: "application/vnd.ms-excel;charset=utf-8" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = "تقرير_الطلاب.xls";
    a.click();
    URL.revokeObjectURL(url);
}
</script>

</body>
</html>
