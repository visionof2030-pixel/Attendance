<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سجل متابعة الطلاب - اللغة الإنجليزية</title>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: 'Cairo', sans-serif;
    }
    
    body {
        background: #f5f7fa;
        color: #333;
        padding-bottom: 20px;
    }
    
    header {
        background: linear-gradient(135deg, #0d47a1, #1565c0);
        color: white;
        padding: 20px;
        text-align: center;
        box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        margin-bottom: 20px;
    }
    
    .admin-btn {
        position: absolute;
        left: 20px;
        top: 20px;
        background: #6a1b9a;
        color: white;
        border: none;
        padding: 10px 20px;
        border-radius: 8px;
        cursor: pointer;
        font-weight: bold;
    }
    
    .modal {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0,0,0,0.8);
        z-index: 1000;
        justify-content: center;
        align-items: center;
    }
    
    .modal-content {
        background: white;
        padding: 30px;
        border-radius: 15px;
        width: 90%;
        max-width: 800px;
        max-height: 90vh;
        overflow-y: auto;
    }
    
    .class-selector {
        display: flex;
        justify-content: center;
        gap: 10px;
        margin: 20px;
        flex-wrap: wrap;
    }
    
    .class-btn {
        background: #0d47a1;
        color: white;
        border: none;
        padding: 10px 20px;
        border-radius: 8px;
        cursor: pointer;
        font-weight: bold;
    }
    
    .class-btn.active {
        background: #1565c0;
        transform: scale(1.05);
    }
    
    .date-container {
        background: white;
        padding: 20px;
        margin: 20px;
        border-radius: 10px;
        box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    
    .date-input {
        width: 100%;
        padding: 10px;
        margin: 10px 0;
        border: 1px solid #ddd;
        border-radius: 5px;
        font-size: 16px;
    }
    
    .container {
        background: white;
        padding: 20px;
        margin: 20px;
        border-radius: 10px;
        box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        overflow-x: auto;
    }
    
    table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 20px;
    }
    
    th, td {
        padding: 12px;
        text-align: center;
        border-bottom: 1px solid #ddd;
    }
    
    th {
        background: #0d47a1;
        color: white;
    }
    
    .attendance-btn {
        padding: 8px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-weight: bold;
        margin: 5px;
    }
    
    .present-btn {
        background: #4CAF50;
        color: white;
    }
    
    .absent-btn {
        background: #f44336;
        color: white;
    }
    
    .active-btn {
        box-shadow: 0 0 0 3px rgba(0,0,0,0.2);
    }
    
    .notes-input {
        width: 100%;
        padding: 8px;
        border: 1px solid #ddd;
        border-radius: 5px;
        resize: vertical;
        min-height: 60px;
    }
    
    .export-btn {
        background: #673AB7;
        color: white;
        border: none;
        padding: 15px;
        margin: 20px;
        width: calc(100% - 40px);
        border-radius: 10px;
        font-size: 18px;
        font-weight: bold;
        cursor: pointer;
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 10px;
    }
    
    .notification {
        position: fixed;
        top: 20px;
        right: 20px;
        background: #4CAF50;
        color: white;
        padding: 15px 25px;
        border-radius: 8px;
        display: none;
        z-index: 1001;
    }
    
    .admin-section {
        background: #f8f9fa;
        padding: 20px;
        margin: 15px 0;
        border-radius: 10px;
        border: 1px solid #ddd;
    }
    
    .star-icon {
        color: gold;
        cursor: pointer;
        margin-right: 10px;
    }
</style>
</head>
<body>

<header>
    <button class="admin-btn" onclick="showAdminLogin()">
        <i class="fas fa-user-shield"></i> Admin
    </button>
    <h1>سجل متابعة الطلاب - اللغة الإنجليزية</h1>
    <h2>المعلم: فهد الخالدي</h2>
</header>

<!-- نافذة إدخال كلمة المرور -->
<div id="passwordModal" class="modal">
    <div class="modal-content">
        <h2 style="color: #6a1b9a; text-align: center; margin-bottom: 20px;">
            <i class="fas fa-lock"></i> الوصول الإداري
        </h2>
        <input type="password" id="adminPassword" class="date-input" placeholder="كلمة المرور">
        <p id="passwordError" style="color: red; display: none; text-align: center;">
            <i class="fas fa-exclamation-circle"></i> كلمة المرور غير صحيحة
        </p>
        <div style="display: flex; gap: 10px; margin-top: 20px;">
            <button onclick="checkAdminPassword()" style="flex: 1; background: #6a1b9a; color: white; padding: 12px; border: none; border-radius: 5px; cursor: pointer;">
                <i class="fas fa-sign-in-alt"></i> دخول
            </button>
            <button onclick="hideAdminLogin()" style="flex: 1; background: #757575; color: white; padding: 12px; border: none; border-radius: 5px; cursor: pointer;">
                <i class="fas fa-times"></i> إلغاء
            </button>
        </div>
    </div>
</div>

<!-- نافذة Admin -->
<div id="adminModal" class="modal">
    <div class="modal-content">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
            <h2 style="color: #6a1b9a;">
                <i class="fas fa-cogs"></i> لوحة التحكم الإدارية
            </h2>
            <button onclick="hideAdminPanel()" style="background: #f44336; color: white; border: none; width: 40px; height: 40px; border-radius: 50%; cursor: pointer;">
                <i class="fas fa-times"></i>
            </button>
        </div>
        
        <div class="admin-section">
            <h3><i class="fas fa-star"></i> النجوم للطلاب المميزين</h3>
            <select id="studentSelect" style="width: 100%; padding: 10px; margin: 10px 0;">
                <option value="">اختر طالباً</option>
            </select>
            <div style="text-align: center; margin: 20px 0;">
                <i class="fas fa-star star-icon" id="toggleStar" style="font-size: 40px; color: #ccc;" onclick="toggleStudentStar()"></i>
                <p id="starStatus">غير مميز</p>
            </div>
            <div id="starredStudentsList">
                <h4>الطلاب المميزين:</h4>
                <!-- سيتم ملؤها بالجافاسكريبت -->
            </div>
        </div>
        
        <div class="admin-section">
            <h3><i class="fas fa-random"></i> التحضير العشوائي</h3>
            <p>تعيين حضور عشوائي بنسبة 75% حضور و 25% غياب</p>
            <select id="randomClassSelect" style="width: 100%; padding: 10px; margin: 10px 0;">
                <option value="all">جميع الفصول</option>
                <option value="c3_1">الصف ٣/١</option>
                <option value="c2_3">الصف ٢/٣</option>
                <option value="c3_3">الصف ٣/٣</option>
                <option value="c4_3">الصف ٤/٣</option>
                <option value="c5_3">الصف ٥/٣</option>
            </select>
            <button onclick="generateRandomAttendance()" style="width: 100%; background: #FF9800; color: white; padding: 12px; border: none; border-radius: 5px; margin-top: 10px; cursor: pointer;">
                <i class="fas fa-magic"></i> توليد تحضير عشوائي
            </button>
        </div>
        
        <div class="admin-section">
            <h3><i class="fas fa-users-cog"></i> إدارة الطلاب</h3>
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
                <div>
                    <h4>إضافة طالب جديد</h4>
                    <input type="text" id="newStudentName" class="date-input" placeholder="اسم الطالب">
                    <select id="newStudentClass" class="date-input">
                        <option value="c3_1">الصف ٣/١</option>
                        <option value="c2_3">الصف ٢/٣</option>
                        <option value="c3_3">الصف ٣/٣</option>
                        <option value="c4_3">الصف ٤/٣</option>
                        <option value="c5_3">الصف ٥/٣</option>
                    </select>
                    <button onclick="addNewStudent()" style="width: 100%; background: #4CAF50; color: white; padding: 12px; border: none; border-radius: 5px; margin-top: 10px; cursor: pointer;">
                        <i class="fas fa-plus"></i> إضافة طالب
                    </button>
                </div>
                <div>
                    <h4>نقل طالب</h4>
                    <select id="transferStudentSelect" class="date-input">
                        <option value="">اختر طالباً</option>
                    </select>
                    <select id="transferToClass" class="date-input">
                        <option value="c3_1">الصف ٣/١</option>
                        <option value="c2_3">الصف ٢/٣</option>
                        <option value="c3_3">الصف ٣/٣</option>
                        <option value="c4_3">الصف ٤/٣</option>
                        <option value="c5_3">الصف ٥/٣</option>
                    </select>
                    <button onclick="transferStudent()" style="width: 100%; background: #2196F3; color: white; padding: 12px; border: none; border-radius: 5px; margin-top: 10px; cursor: pointer;">
                        <i class="fas fa-exchange-alt"></i> نقل طالب
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- اختيار الفصل -->
<div class="class-selector">
    <button class="class-btn active" onclick="showClass('c3_1')">الصف ٣/١</button>
    <button class="class-btn" onclick="showClass('c2_3')">الصف ٢/٣</button>
    <button class="class-btn" onclick="showClass('c3_3')">الصف ٣/٣</button>
    <button class="class-btn" onclick="showClass('c4_3')">الصف ٤/٣</button>
    <button class="class-btn" onclick="showClass('c5_3')">الصف ٥/٣</button>
</div>

<!-- التاريخ -->
<div class="date-container">
    <label><i class="fas fa-calendar-alt"></i> التاريخ:</label>
    <input type="date" id="currentDate" class="date-input" onchange="updateAttendance()">
    <button onclick="setToday()" style="width: 100%; background: #FF9800; color: white; padding: 12px; border: none; border-radius: 5px; margin-top: 10px; cursor: pointer;">
        <i class="fas fa-calendar-day"></i> تعيين تاريخ اليوم
    </button>
</div>

<!-- جدول الطلاب -->
<div id="classContent" class="container">
    <!-- سيتم ملؤها بالجافاسكريبت -->
</div>

<!-- زر التصدير -->
<button class="export-btn" onclick="generatePDF()">
    <i class="fas fa-file-pdf"></i> تصدير التقرير بصيغة PDF
</button>

<!-- إشعارات -->
<div id="notification" class="notification"></div>

<script>
// ================== المتغيرات الرئيسية ==================
const ADMIN_PASSWORD = "Jassar1436";
let starredStudents = JSON.parse(localStorage.getItem('starredStudents')) || {};
let studentsData = JSON.parse(localStorage.getItem('studentsData')) || {};
let attendanceData = JSON.parse(localStorage.getItem('attendanceData')) || {};
let currentClass = "c3_1";
let currentDate = new Date().toISOString().split('T')[0];

// ================== تهيئة النظام ==================
document.addEventListener('DOMContentLoaded', function() {
    document.getElementById('currentDate').value = currentDate;
    showClass('c3_1');
    updateStudentSelects();
    updateStarredStudentsList();
});

// ================== إدارة Admin ==================
function showAdminLogin() {
    document.getElementById('passwordModal').style.display = 'flex';
}

function hideAdminLogin() {
    document.getElementById('passwordModal').style.display = 'none';
    document.getElementById('passwordError').style.display = 'none';
}

function checkAdminPassword() {
    const password = document.getElementById('adminPassword').value;
    if (password === ADMIN_PASSWORD) {
        hideAdminLogin();
        showAdminPanel();
        showNotification('تم الدخول إلى لوحة التحكم', 'success');
    } else {
        document.getElementById('passwordError').style.display = 'block';
        document.getElementById('adminPassword').value = '';
    }
}

function showAdminPanel() {
    document.getElementById('adminModal').style.display = 'flex';
}

function hideAdminPanel() {
    document.getElementById('adminModal').style.display = 'none';
}

// ================== إدارة النجوم ==================
function updateStudentSelects() {
    const studentSelect = document.getElementById('studentSelect');
    const transferSelect = document.getElementById('transferStudentSelect');
    
    studentSelect.innerHTML = '<option value="">اختر طالباً</option>';
    transferSelect.innerHTML = '<option value="">اختر طالباً</option>';
    
    const allStudents = getAllStudents();
    allStudents.forEach(student => {
        const option = `<option value="${student.id}">${student.name} (${student.className})</option>`;
        studentSelect.innerHTML += option;
        transferSelect.innerHTML += option;
    });
}

function getAllStudents() {
    const allStudents = [];
    const classes = ['c3_1', 'c2_3', 'c3_3', 'c4_3', 'c5_3'];
    const classNames = {
        'c3_1': '٣/١',
        'c2_3': '٢/٣',
        'c3_3': '٣/٣',
        'c4_3': '٤/٣',
        'c5_3': '٥/٣'
    };
    
    classes.forEach(classId => {
        const classData = studentsData[classId] || getDefaultStudents(classId);
        classData.forEach((student, index) => {
            allStudents.push({
                id: `${classId}-${index}`,
                name: student,
                classId: classId,
                className: classNames[classId]
            });
        });
    });
    
    return allStudents;
}

function showSelectedStudent() {
    const studentId = document.getElementById('studentSelect').value;
    const starIcon = document.getElementById('toggleStar');
    const starStatus = document.getElementById('starStatus');
    
    if (studentId) {
        const isStarred = starredStudents[studentId] || false;
        starIcon.style.color = isStarred ? 'gold' : '#ccc';
        starStatus.textContent = isStarred ? 'مميز بنجمة' : 'غير مميز';
    }
}

function toggleStudentStar() {
    const studentId = document.getElementById('studentSelect').value;
    if (!studentId) {
        showNotification('الرجاء اختيار طالب أولاً', 'warning');
        return;
    }
    
    starredStudents[studentId] = !starredStudents[studentId];
    localStorage.setItem('starredStudents', JSON.stringify(starredStudents));
    
    const starIcon = document.getElementById('toggleStar');
    const starStatus = document.getElementById('starStatus');
    starIcon.style.color = starredStudents[studentId] ? 'gold' : '#ccc';
    starStatus.textContent = starredStudents[studentId] ? 'مميز بنجمة' : 'غير مميز';
    
    updateStarredStudentsList();
    updateClassDisplay();
}

function updateStarredStudentsList() {
    const listElement = document.getElementById('starredStudentsList');
    const allStudents = getAllStudents();
    const starredList = allStudents.filter(student => starredStudents[student.id]);
    
    let html = '<h4>الطلاب المميزين:</h4>';
    if (starredList.length === 0) {
        html += '<p style="text-align: center; color: #888;">لا يوجد طلاب مميزين</p>';
    } else {
        starredList.forEach(student => {
            html += `<div style="padding: 10px; border-bottom: 1px solid #ddd;">
                <i class="fas fa-star" style="color: gold;"></i> ${student.name} (${student.className})
            </div>`;
        });
    }
    listElement.innerHTML = html;
}

// ================== التحضير العشوائي ==================
function generateRandomAttendance() {
    const classId = document.getElementById('randomClassSelect').value;
    const date = document.getElementById('currentDate').value;
    
    if (!date) {
        showNotification('الرجاء تحديد التاريخ أولاً', 'warning');
        return;
    }
    
    if (classId === 'all') {
        ['c3_1', 'c2_3', 'c3_3', 'c4_3', 'c5_3'].forEach(cid => {
            generateClassRandomAttendance(cid, date);
        });
        showNotification('تم توليد تحضير عشوائي لجميع الفصول', 'success');
    } else {
        generateClassRandomAttendance(classId, date);
        showNotification(`تم توليد تحضير عشوائي للفصل ${classId}`, 'success');
    }
    
    // تحديث العرض إذا كان الفصل الحالي هو نفسه
    if (classId === 'all' || classId === currentClass) {
        showClass(currentClass);
    }
}

function generateClassRandomAttendance(classId, date) {
    if (!attendanceData[classId]) attendanceData[classId] = {};
    
    const classStudents = studentsData[classId] || getDefaultStudents(classId);
    attendanceData[classId][date] = {};
    
    classStudents.forEach((student, index) => {
        const isPresent = Math.random() < 0.75; // 75% حضور
        attendanceData[classId][date][index] = {
            present: isPresent,
            absent: !isPresent,
            note: isPresent ? 'حضور عشوائي' : 'غياب عشوائي'
        };
    });
    
    localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
}

// ================== إدارة الطلاب ==================
function addNewStudent() {
    const name = document.getElementById('newStudentName').value.trim();
    const classId = document.getElementById('newStudentClass').value;
    
    if (!name) {
        showNotification('الرجاء إدخال اسم الطالب', 'warning');
        return;
    }
    
    if (!studentsData[classId]) studentsData[classId] = getDefaultStudents(classId);
    studentsData[classId].push(name);
    localStorage.setItem('studentsData', JSON.stringify(studentsData));
    
    updateStudentSelects();
    if (classId === currentClass) showClass(currentClass);
    
    document.getElementById('newStudentName').value = '';
    showNotification(`تم إضافة الطالب ${name}`, 'success');
}

function transferStudent() {
    const studentId = document.getElementById('transferStudentSelect').value;
    const newClassId = document.getElementById('transferToClass').value;
    
    if (!studentId) {
        showNotification('الرجاء اختيار طالب', 'warning');
        return;
    }
    
    const [oldClassId, index] = studentId.split('-');
    const studentIndex = parseInt(index);
    
    if (oldClassId === newClassId) {
        showNotification('الطالب موجود بالفعل في هذا الفصل', 'warning');
        return;
    }
    
    const studentName = studentsData[oldClassId][studentIndex];
    
    if (!studentsData[newClassId]) studentsData[newClassId] = getDefaultStudents(newClassId);
    studentsData[newClassId].push(studentName);
    studentsData[oldClassId].splice(studentIndex, 1);
    
    localStorage.setItem('studentsData', JSON.stringify(studentsData));
    updateStudentSelects();
    
    if (oldClassId === currentClass || newClassId === currentClass) {
        showClass(currentClass);
    }
    
    showNotification(`تم نقل ${studentName}`, 'success');
}

// ================== عرض الفصول ==================
function showClass(classId) {
    currentClass = classId;
    document.querySelectorAll('.class-btn').forEach(btn => btn.classList.remove('active'));
    event.target.classList.add('active');
    
    const students = studentsData[classId] || getDefaultStudents(classId);
    const date = document.getElementById('currentDate').value;
    
    let html = `<h2>طلاب الفصل ${classId.replace('c', '').replace('_', '/')}</h2>
                <table>
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>اسم الطالب</th>
                            <th>حضور</th>
                            <th>غياب</th>
                            <th>ملاحظات</th>
                        </tr>
                    </thead>
                    <tbody>`;
    
    students.forEach((student, index) => {
        const studentId = `${classId}-${index}`;
        const isStarred = starredStudents[studentId] || false;
        
        // جلب بيانات الحضور
        let isPresent = false;
        let isAbsent = false;
        let note = '';
        
        if (date && attendanceData[classId] && attendanceData[classId][date]) {
            const attendance = attendanceData[classId][date][index];
            if (attendance) {
                isPresent = attendance.present || false;
                isAbsent = attendance.absent || false;
                note = attendance.note || '';
            }
        }
        
        html += `<tr>
            <td>${index + 1}</td>
            <td style="text-align: right;">
                ${isStarred ? '<i class="fas fa-star star-icon"></i>' : ''}
                ${student}
            </td>
            <td>
                <button class="attendance-btn present-btn ${isPresent ? 'active-btn' : ''}" 
                        onclick="markAttendance('${classId}', ${index}, 'present')">
                    <i class="fas fa-check"></i> حاضر
                </button>
            </td>
            <td>
                <button class="attendance-btn absent-btn ${isAbsent ? 'active-btn' : ''}" 
                        onclick="markAttendance('${classId}', ${index}, 'absent')">
                    <i class="fas fa-times"></i> غائب
                </button>
            </td>
            <td>
                <textarea class="notes-input" onchange="saveNote('${classId}', ${index}, this.value)">${note}</textarea>
            </td>
        </tr>`;
    });
    
    html += `</tbody></table>`;
    document.getElementById('classContent').innerHTML = html;
}

function markAttendance(classId, index, type) {
    const date = document.getElementById('currentDate').value;
    if (!date) {
        showNotification('الرجاء تحديد التاريخ أولاً', 'warning');
        return;
    }
    
    if (!attendanceData[classId]) attendanceData[classId] = {};
    if (!attendanceData[classId][date]) attendanceData[classId][date] = {};
    
    attendanceData[classId][date][index] = {
        present: type === 'present',
        absent: type === 'absent',
        note: document.querySelectorAll('.notes-input')[index]?.value || ''
    };
    
    localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
    showClass(classId);
    showNotification(`تم تعيين ${type === 'present' ? 'حضور' : 'غياب'} للطالب`, 'success');
}

function saveNote(classId, index, note) {
    const date = document.getElementById('currentDate').value;
    if (!date) return;
    
    if (!attendanceData[classId]) attendanceData[classId] = {};
    if (!attendanceData[classId][date]) attendanceData[classId][date] = {};
    
    if (!attendanceData[classId][date][index]) {
        attendanceData[classId][date][index] = { present: false, absent: false };
    }
    
    attendanceData[classId][date][index].note = note;
    localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
}

// ================== التواريخ ==================
function setToday() {
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('currentDate').value = today;
    currentDate = today;
    showClass(currentClass);
    showNotification('تم تعيين تاريخ اليوم', 'success');
}

function updateAttendance() {
    currentDate = document.getElementById('currentDate').value;
    showClass(currentClass);
}

// ================== تصدير PDF ==================
function generatePDF() {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF('p', 'mm', 'a4');
    const date = document.getElementById('currentDate').value;
    const className = document.querySelector('.class-btn.active').textContent;
    
    // العنوان
    doc.setFontSize(20);
    doc.setTextColor(13, 71, 161);
    doc.text('تقرير متابعة الطلاب', 105, 20, { align: 'center' });
    
    doc.setFontSize(16);
    doc.setTextColor(0, 0, 0);
    doc.text('مادة اللغة الإنجليزية - المعلم: فهد الخالدي', 105, 30, { align: 'center' });
    
    // المعلومات الأساسية
    doc.setFontSize(12);
    doc.text(`الفصل: ${className}`, 20, 50);
    doc.text(`التاريخ: ${date}`, 20, 60);
    
    // جدول البيانات
    const students = studentsData[currentClass] || getDefaultStudents(currentClass);
    const tableData = [];
    
    students.forEach((student, index) => {
        let status = 'لم يتم التحديد';
        let note = '';
        
        if (date && attendanceData[currentClass] && attendanceData[currentClass][date]) {
            const attendance = attendanceData[currentClass][date][index];
            if (attendance) {
                status = attendance.present ? 'حاضر' : (attendance.absent ? 'غائب' : 'لم يتم التحديد');
                note = attendance.note || '';
            }
        }
        
        tableData.push([
            (index + 1).toString(),
            student,
            status,
            note || 'لا توجد ملاحظات'
        ]);
    });
    
    // إنشاء الجدول
    doc.autoTable({
        startY: 70,
        head: [['#', 'اسم الطالب', 'الحالة', 'ملاحظات']],
        body: tableData,
        theme: 'grid',
        styles: {
            font: 'helvetica',
            fontSize: 10,
            textColor: [0, 0, 0],
            cellPadding: 5,
            overflow: 'linebreak',
            halign: 'right'
        },
        headStyles: {
            fillColor: [13, 71, 161],
            textColor: [255, 255, 255],
            fontStyle: 'bold',
            halign: 'center'
        },
        bodyStyles: {
            halign: 'right'
        },
        columnStyles: {
            0: { cellWidth: 15, halign: 'center' },
            1: { cellWidth: 60 },
            2: { cellWidth: 30, halign: 'center' },
            3: { cellWidth: 85 }
        },
        margin: { left: 15, right: 15 }
    });
    
    // التوقيع
    const finalY = doc.lastAutoTable.finalY + 20;
    doc.setFontSize(10);
    doc.text('توقيع المعلم: ________________', 20, finalY);
    doc.text(`تاريخ الإصدار: ${new Date().toLocaleDateString('ar-SA')}`, 180, finalY);
    
    // حفظ الملف
    doc.save(`تقرير-${className}-${date}.pdf`);
    showNotification('تم تصدير التقرير بنجاح', 'success');
}

// ================== الدعم ==================
function getDefaultStudents(classId) {
    const defaultClasses = {
        "c3_1": [
            "إسماعيل محمد هاشم شفيق الرحمن",
            "ابراهيم علي ابو بكر محمد",
            "باسم محمد - ابو طالب",
            "حسين بشير أمادو جازير",
            "حسين هارون عثمان عبدالمؤمن ادم",
            "حمد محمد عثمان بخش",
            "رمضان عيسى باكور محمد",
            "ريان عبد الرحمن موسى جيبو",
            "ريحان محمد مقبول حسين عمر حمزه",
            "عامر مولوي حسن شريف",
            "عبدالحليم نور كبير - صديق احمد",
            "عمران يعقوب محمد محمد مسلم",
            "عمير محمد محمد شفيع حكيم علي",
            "فارس محمد ابو البشر واعظ علي",
            "محمد احمد فضل الرحمن فايز اللّٰه",
            "حمد انوار رشيد احمد اظهار مياه",
            "حمد عبدالرزاق محمد عبدالقادر",
            "حمد عبدالشكور عبدالحميد عبد الرشيد",
            "مهدي محمد محمد اسلام عبدالسلام",
            "مهدي موسى حميد الحق احمد",
            "ياسين محمد يوسف"
        ],
        "c2_3": [
            "إبراهيم إدريس إبراهيم اولوجيوم",
            "إدريس محمد حسن أحمد",
            "امين عبداللّه دايابو عثمان",
            "بسام عبدالسلام هاشم انور علي",
            "حافظ بيلو موسى سليمان",
            "حسين علي حسن مهاوش",
            "خالد طيب اسماعيل محمد",
            "خالد عبد الحميد محمد هاشم",
            "خالد وليد محمد محمد",
            "ريان عبدالرحمن عمر نانتومي",
            "سليمان ابراهيم ديقوقا",
            "صالح عبدالله محمد قاسم يوسف علي عبدالعزيز اول اودو محمد",
            "عثمان عبد الرحمن باي محمد",
            "عدنان نور امير حسين",
            "عمر سراج محمد زكريا",
            "فهد محمد حسين عبداللّه مياه حسين محمد ابراهيم سعيد هو ساوي محمد محمد امين اسلام خليل الرحمن مشعل ابو طاهر ناظر حسين عبدالمطلب موسى ابو بكر الصديق عبدالجبار امة علي",
            "يوسف مهدي عابدين محمد"
        ],
        "c3_3": [
            "ابراهيم جزولي - اسدانور",
            "تركي عبدالصمد عبدالغني محمد حسين",
            "حسام حسن ابو الكلام مقبول احمد",
            "حسن عيسى بكوري محمد",
            "سعد سلام ستار ارشاد اللّٰه",
            "عايض سيف الاسلام نور احمد علي عبدالكريم عثمان ابكر كوجو",
            "عزام شمس العالم قاسم علي",
            "عماد محمد صديق محمد شفيع سيد عمر عبد القدوس عبدالسلام عبد السبحان عمر مورتلا أبو بكر محمد",
            "فيصل احمد ابو بكر محمد",
            "محمد اسحاق محمد اسلام عبدالحكيم",
            "محمد عبدالله ابو سعيد مياه",
            "حمد محمد اسماعيل امير حسين ابو بكر",
            "حمد موسى ساليفو ديقوقا",
            "مشاري شيهو اسماعيل محمد بكر",
            "ياسر عبدالرحيم محمد علي سفر علي",
            "يوسف محمد عبد الرحمن علي"
        ],
        "c4_3": [
            "ابراهيم عوض احمد فليس",
            "احمد ابراهيم ابن زكريا الهوسه",
            "احمد عبد القيوم محمد يعقوب",
            "اسماعيل اول اودو محمد",
            "اوسامة سعيدو دو غويد",
            "تامر عبد الصمد عبد الغني",
            "تركي هارون حسن شريف",
            "ريان محمد مقبول حسين حسين",
            "ريان هارون الرشيد طفيل احمد نذير احمد",
            "عبدالحليم محمد عبدالله عبدالحكيم",
            "عبدالله حفيظ اللّٰه سلطان أحمد",
            "عيسى عثمان سعيد عالم حبيب الرحمن",
            "فهد أسار رشيد احمد",
            "فهد محمد نور مقبول اشرف",
            "محمد محمد ادريس نبية حسين يعقوب علي",
            "مصلح محمد ولي احمد",
            "معاذ عثمان صديق كالو",
            "يوسف بدماسي ابراهيم البد ماسي"
        ],
        "c5_3": [
            "ابراهيم خالد سليمان ابراهيم",
            "انس عبدالعزيز نور احمد",
            "بدر بكر عمر محمد",
            "حمد محمد حسين مياه شمس العالم اظهر مياه",
            "رضوان رشيد أحمد نور محمد لال مياه",
            "سعيد عبدالله سعيد محمد",
            "عامر رحمة اللّٰه محمد شفيع",
            "عبد اللّٰه حسين علي فليس",
            "عبد العزيز سراج ابكر عثمان",
            "عبدالله عيسى - ابراهيم",
            "عمر محمد عمر صالح",
            "غسان عثمان اسماعيل عبدالله عبد اللّٰه",
            "فاضل عادل صالح الرايس",
            "محمد فريد كبير احمد عباد اللّٰه",
            "محمد محمد سلطان احمد محمد",
            "محمد موسى أدامو محمد",
            "محمد نور محمد زكريا آمال حسين",
            "مشاري محمد هارو",
            "مشاري يعقوب أبو بكر ابراهيم",
            "منذر علي عمر قوني",
            "هود حسن عبدالكريم الياس",
            "يعقوب محمد إسحاق يار محمد فضل على"
        ]
    };
    
    return studentsData[classId] || defaultClasses[classId] || [];
}

function showNotification(message, type = 'success') {
    const notification = document.getElementById('notification');
    notification.textContent = message;
    notification.style.background = type === 'success' ? '#4CAF50' : 
                                   type === 'warning' ? '#FF9800' : '#f44336';
    notification.style.display = 'block';
    
    setTimeout(() => {
        notification.style.display = 'none';
    }, 3000);
}

function updateClassDisplay() {
    const rows = document.querySelectorAll('#classContent tr');
    rows.forEach((row, index) => {
        const studentId = `${currentClass}-${index}`;
        const starIcon = row.querySelector('.star-icon');
        if (starIcon && starredStudents[studentId]) {
            starIcon.style.color = 'gold';
        }
    });
}
</script>
</body>
</html>
