<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سجل متابعة الطلاب</title>
<style>
body {
    font-family: "Tajawal", sans-serif;
    margin: 0;
    padding: 0;
    background: #f7f7f7;
}

header {
    background: #6ec9ff; /* أزرق سماوي */
    color: #fff;
    text-align: center;
    padding: 15px 0;
    font-size: 20px;
    font-weight: bold;
}

.container {
    width: 95%;
    margin: 10px auto;
    background: white;
    padding: 10px;
    border-radius: 10px;
    box-shadow: 0px 2px 6px rgba(0,0,0,0.1);
}

table {
    width: 100%;
    border-collapse: collapse;
    font-size: 12px;
}

th, td {
    border: 1px solid #ccc;
    padding: 6px;
    text-align: center;
}

th {
    background: #dedede;
    font-size: 11px;
}

td {
    cursor: pointer;
    user-select: none;
}

button {
    margin: 5px;
    padding: 6px 10px;
    border: none;
    border-radius: 5px;
    background: #6ec9ff;
    color: white;
    font-weight: bold;
    cursor: pointer;
}

.admin-panel {
    display: none;
    margin-top: 10px;
    padding: 10px;
    border: 1px solid #6ec9ff;
    border-radius: 10px;
    background: #e0f2ff;
}
</style>
</head>
<body>

<header>سجل متابعة الطلاب</header>

<div class="container">
    <button onclick="exportToExcel()">تصدير Excel</button>
    <table id="studentTable">
        <thead>
            <tr>
                <th>الاسم</th>
                <th>الحضور</th>
                <th>الواجبات</th>
                <th>المشروعات</th>
                <th>تطبيقات وأنشطة</th>
                <th>مشاركة</th>
                <th>⭐</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>أحمد</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggleStar(this)">☆</td>
            </tr>
            <tr>
                <td>سارة</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggle(this)">✔</td>
                <td onclick="toggleStar(this)">☆</td>
            </tr>
        </tbody>
    </table>

    <div>
        <input type="password" id="adminPass" placeholder="ادخل كلمة المرور">
        <button onclick="checkAdmin()">فتح الإدارة</button>
    </div>

    <div class="admin-panel" id="adminPanel">
        <button onclick="addStudent()">إضافة طالب</button>
        <button onclick="randomAttendance()">تحضير عشوائي</button>
        <button onclick="moveStudent()">نقل طالب</button>
        <p>بعد تفعيل الإدارة، يمكن تمييز الطلاب بالنجمة.</p>
    </div>
</div>

<script>
// تبديل ✔ و ✖
function toggle(cell) {
    cell.innerHTML = cell.innerHTML === "✔" ? "✖" : "✔";
}

// النجمة تعمل فقط بعد فتح الإدارة
let adminActive = false;
function toggleStar(cell) {
    if(adminActive) {
        cell.innerHTML = cell.innerHTML === "☆" ? "⭐" : "☆";
    }
}

// التحقق من كلمة المرور
function checkAdmin() {
    const pass = document.getElementById("adminPass").value;
    if(pass === "1406") {
        adminActive = true;
        document.getElementById("adminPanel").style.display = "block";
        alert("تم تفعيل خصائص الإدارة");
    } else {
        alert("كلمة مرور خاطئة");
    }
}

// إضافة طالب جديد
function addStudent() {
    const name = prompt("ادخل اسم الطالب");
    if(name) {
        const table = document.getElementById("studentTable").getElementsByTagName('tbody')[0];
        const newRow = table.insertRow();
        newRow.innerHTML = `<td>${name}</td>
                            <td onclick="toggle(this)">✔</td>
                            <td onclick="toggle(this)">✔</td>
                            <td onclick="toggle(this)">✔</td>
                            <td onclick="toggle(this)">✔</td>
                            <td onclick="toggle(this)">✔</td>
                            <td onclick="toggleStar(this)">☆</td>`;
    }
}

// تحضير عشوائي
function randomAttendance() {
    const rows = document.getElementById("studentTable").getElementsByTagName('tbody')[0].rows;
    for(let row of rows) {
        for(let i=1; i<=5; i++) {
            row.cells[i].innerHTML = Math.random() > 0.5 ? "✔" : "✖";
        }
    }
}

// نقل طالب
function moveStudent() {
    alert("ميزة النقل من صف لآخر يمكن تخصيصها حسب الحاجة.");
}

// تصدير Excel
function exportToExcel() {
    let table = document.getElementById("studentTable").outerHTML;
    let uri = 'data:application/vnd.ms-excel;base64,';
    let template = `<html xmlns:o="urn:schemas-microsoft-com:office:office" xmlns:x="urn:schemas-microsoft-com:office:excel" xmlns="http://www.w3.org/TR/REC-html40">
    <head><!--[if gte mso 9]><xml><x:ExcelWorkbook><x:ExcelWorksheets><x:ExcelWorksheet><x:Name>Sheet1</x:Name>
    <x:WorksheetOptions><x:DisplayGridlines/></x:WorksheetOptions></x:ExcelWorksheet></x:ExcelWorksheets></x:ExcelWorkbook></xml><![endif]-->
    </head><body>${table}</body></html>`;
    let link = document.createElement("a");
    link.href = uri + btoa(unescape(encodeURIComponent(template)));
    link.download = "تقرير_الطلاب.xls";
    link.click();
}
</script>

</body>
</html>
