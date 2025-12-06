<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سجل متابعة الطلاب</title>

<!-- مكتبة jsPDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

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
    letter-spacing: 1px;
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

.admin-panel {
    display: none;
    margin-top: 15px;
    padding: 10px;
    background: #fff;
    border-radius: 8px;
    border: 1px solid #dcdcdc;
}

.input-field {
    width: 100%;
    padding: 8px;
    margin: 5px 0;
    border: 1px solid #ccc;
    border-radius: 6px;
}

button {
    width: 100%;
    padding: 10px;
    margin-top: 8px;
    font-size: 14px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    background: #6ec9ff;
    color: white;
    font-weight: bold;
}

#admin-btn {
    background: #e0e0e0;
    color: black;
    margin-top: 25px;
}

#pdf-btn {
    background: #0099ff;
    margin-bottom: 10px;
}

.star {
    color: gold;
    font-size: 16px;
    font-weight: bold;
    display: inline-block;
    width: 18px;
}
</style>
</head>

<body>

<header>سجل متابعة الطلاب</header>

<div class="container">

    <button id="pdf-btn" onclick="exportPDF()">📄 استخراج PDF</button>

    <table id="students-table">
        <thead>
            <tr>
                <th>م</th>
                <th>اسم الطالب</th>
                <th>حضور</th>
                <th>واجب</th>
                <th>مشروع</th>
                <th>تطبيق</th>
                <th>مشاركة</th>
                <th>⭐</th>
            </tr>
        </thead>
        <tbody id="table-body"></tbody>
    </table>

    <!-- زر الإدارة -->
    <button id="admin-btn" onclick="toggleAdminLogin()">⚙️ إدارة</button>

    <!-- إدخال كلمة السر -->
    <div id="admin-login" style="display:none; margin-top:10px;">
        <input type="password" id="admin-pass" class="input-field" placeholder="ادخل كلمة المرور">
        <button onclick="checkPassword()">دخول</button>
    </div>

    <!-- خصائص الإدارة -->
    <div class="admin-panel" id="admin-panel">

        <h3 style="text-align:center; margin-bottom:10px;">خصائص الإدارة</h3>

        <input id="new-student" class="input-field" placeholder="اسم الطالب الجديد">
        <button onclick="addStudent()">➕ إضافة طالب</button>

        <input id="move-from" class="input-field" placeholder="اسم الطالب المراد نقله">
        <input id="move-to" class="input-field" placeholder="الصف الجديد">
        <button onclick="moveStudent()">🔄 نقل طالب</button>

        <button onclick="randomAttendance()">🎲 تحضير عشوائي</button>
    </div>

</div>

<script>
let adminEnabled = false;

const students = [
    { name: "أحمد محمد", attend: false, hw: false, proj: false, app: false, part: false, star: false },
    { name: "سعود ناصر", attend: false, hw: false, proj: false, app: false, part: false, star: false },
];

function renderTable() {
    const body = document.getElementById("table-body");
    body.innerHTML = "";

    students.forEach((s, i) => {
        body.innerHTML += `
        <tr>
            <td>${i+1}</td>
            <td>${s.name}</td>
            <td onclick="toggle(${i}, 'attend')">${s.attend ? "✓" : "✕"}</td>
            <td onclick="toggle(${i}, 'hw')">${s.hw ? "✓" : "✕"}</td>
            <td onclick="toggle(${i}, 'proj')">${s.proj ? "✓" : "✕"}</td>
            <td onclick="toggle(${i}, 'app')">${s.app ? "✓" : "✕"}</td>
            <td onclick="toggle(${i}, 'part')">${s.part ? "✓" : "✕"}</td>
            <td>
                <span class="star" onclick="toggleStar(${i})">
                    ${s.star ? "⭐" : ""}
                </span>
            </td>
        </tr>
        `;
    });
}

function toggle(index, field) {
    students[index][field] = !students[index][field];
    renderTable();
}

// ⭐ النجمة — تعمل فقط بعد تفعيل الإدارة
function toggleStar(i) {
    if (!adminEnabled) return;
    students[i].star = !students[i].star;
    renderTable();
}

function toggleAdminLogin() {
    const loginDiv = document.getElementById("admin-login");
    loginDiv.style.display = loginDiv.style.display === "none" ? "block" : "none";
}

function checkPassword() {
    const pass = document.getElementById("admin-pass").value;
    if (pass === "1406") {
        adminEnabled = true;
        document.getElementById("admin-panel").style.display = "block";
        document.getElementById("admin-login").style.display = "none";
        document.getElementById("admin-pass").value = "";
        renderTable(); // لتفعيل النجمة بعد الدخول
    }
}

function addStudent() {
    let name = document.getElementById("new-student").value.trim();
    if (name.length < 2) return;
    students.push({ name, attend:false, hw:false, proj:false, app:false, part:false, star:false });
    document.getElementById("new-student").value = "";
    renderTable();
}

function moveStudent() {
    let from = document.getElementById("move-from").value.trim();
    let to = document.getElementById("move-to").value.trim();
    let student = students.find(s => s.name === from);
    if (student) {
        student.name = `${from} (${to})`;
        renderTable();
    }
}

function randomAttendance() {
    students.forEach(s => s.attend = Math.random() < 0.5);
    renderTable();
}

// تصدير PDF فعلي
function exportPDF() {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ orientation: "landscape", unit: "pt", format: "a4" });

    doc.setFontSize(14);
    doc.text("سجل متابعة الطلاب", 40, 40);

    let rows = students.map((s, i) => [
        i+1,
        s.name,
        s.attend ? "✓" : "✕",
        s.hw ? "✓" : "✕",
        s.proj ? "✓" : "✕",
        s.app ? "✓" : "✕",
        s.part ? "✓" : "✕",
        s.star ? "⭐" : ""
    ]);

    doc.autoTable({
        head: [["م","اسم الطالب","حضور","واجب","مشروع","تطبيق","مشاركة","⭐"]],
        body: rows,
        startY: 60,
        theme: "grid",
        headStyles: { fillColor: [110, 201, 255] }
    });

    doc.save("Attendance_Report.pdf");
}

renderTable();
</script>

<!-- jsPDF AutoTable plugin -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>

</body>
</html>
