<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>نظام متابعة الطلاب - نسخة اختبارية</title>

<!-- خطوط وأيقونات -->
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- مكتبات -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>

<style>
    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    body {
        font-family: "Cairo", sans-serif;
        background: linear-gradient(135deg, #f5f7fa 0%, #e4e8f0 100%);
        color: #333;
        line-height: 1.6;
        min-height: 100vh;
        transition: filter 0.3s ease;
    }

    /* إصلاح مشكلة blur */
    body.blurred .main-blur {
        filter: blur(5px);
        pointer-events: none;
    }

    /* إصلاح: إزالة تأثير blur على النوافذ المنبثقة */
    .password-modal,
    .admin-modal {
        filter: none !important;
        backdrop-filter: none !important;
    }

    /* ============= الهيدر ============= */
    header {
        background: linear-gradient(90deg, #0a3a8a, #0d47a1);
        padding: 15px;
        color: white;
        text-align: center;
        box-shadow: 0 4px 12px rgba(13, 71, 161, 0.2);
        border-bottom: 4px solid #1565c0;
    }

    .school-logo {
        width: 50px;
        height: 50px;
        background: white;
        border-radius: 50%;
        margin: 0 auto 10px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #0d47a1;
        font-size: 24px;
    }

    header h1 {
        margin: 0;
        font-size: 22px;
        font-weight: 700;
    }

    .admin-btn {
        background: linear-gradient(135deg, #6a1b9a, #8e24aa);
        color: white;
        border: none;
        padding: 10px 20px;
        border-radius: 8px;
        font-size: 16px;
        font-weight: 700;
        cursor: pointer;
        transition: all 0.3s ease;
        display: inline-flex;
        align-items: center;
        gap: 8px;
        margin-top: 10px;
    }

    .admin-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 5px 12px rgba(106, 27, 154, 0.4);
    }

    /* ============= نافذة كلمة المرور ============= */
    .password-modal {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.7);
        z-index: 1000;
        align-items: center;
        justify-content: center;
    }

    .password-modal.active {
        display: flex;
    }

    .password-box {
        background: white;
        width: 90%;
        max-width: 400px;
        padding: 30px;
        border-radius: 15px;
        box-shadow: 0 15px 35px rgba(0,0,0,0.5);
        text-align: center;
        border-top: 5px solid #6a1b9a;
    }

    .password-input {
        width: 100%;
        padding: 15px;
        font-size: 18px;
        border: 2px solid #b0bec5;
        border-radius: 10px;
        text-align: center;
        font-family: "Cairo", sans-serif;
        margin: 15px 0;
    }

    .password-btn {
        background: linear-gradient(135deg, #6a1b9a, #4a148c);
        color: white;
        border: none;
        padding: 12px 25px;
        border-radius: 10px;
        font-size: 16px;
        font-weight: 700;
        cursor: pointer;
        width: 100%;
        margin: 5px 0;
    }

    /* ============= نافذة Admin ============= */
    .admin-modal {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.75);
        z-index: 1001;
        overflow-y: auto;
    }

    .admin-modal.active {
        display: block;
    }

    .admin-container {
        background: white;
        width: 95%;
        max-width: 800px;
        margin: 20px auto;
        padding: 25px;
        border-radius: 15px;
        box-shadow: 0 20px 50px rgba(0,0,0,0.5);
        border-top: 5px solid #6a1b9a;
    }

    .close-admin {
        background: #c62828;
        color: white;
        border: none;
        width: 40px;
        height: 40px;
        border-radius: 50%;
        font-size: 20px;
        cursor: pointer;
        float: left;
    }

    /* ============= الفصول ============= */
    .class-selector {
        display: flex;
        justify-content: center;
        gap: 10px;
        margin: 20px;
        flex-wrap: wrap;
    }

    .class-selector button {
        background: #0d47a1;
        color: white;
        border: none;
        padding: 12px 18px;
        border-radius: 8px;
        font-size: 16px;
        cursor: pointer;
        min-width: 80px;
    }

    .class-selector button.active {
        background: #1565c0;
        box-shadow: 0 4px 8px rgba(21, 101, 192, 0.4);
    }

    /* ============= الجدول ============= */
    .container {
        width: 95%;
        margin: 20px auto;
        background: white;
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0 6px 20px rgba(0,0,0,0.08);
        overflow-x: auto;
    }

    table {
        width: 100%;
        border-collapse: collapse;
        min-width: 600px;
    }

    th {
        background: linear-gradient(135deg, #0d47a1, #0a3a8a);
        color: white;
        padding: 15px;
        text-align: center;
    }

    td {
        padding: 12px;
        text-align: center;
        border-bottom: 1px solid #eceff1;
    }

    .btn {
        padding: 10px 15px;
        border-radius: 8px;
        font-size: 14px;
        cursor: pointer;
        border: none;
        min-width: 70px;
        font-weight: 700;
    }

    .present {
        background: #2e7d32;
        color: white;
    }
    
    .absent {
        background: #c62828;
        color: white;
    }
    
    .present.active {
        background: #1b5e20;
        box-shadow: 0 0 0 3px rgba(46, 125, 50, 0.3);
    }
    
    .absent.active {
        background: #b71c1c;
        box-shadow: 0 0 0 3px rgba(198, 40, 40, 0.3);
    }

    textarea {
        width: 100%;
        padding: 10px;
        border-radius: 8px;
        border: 1px solid #b0bec5;
        font-family: "Cairo", sans-serif;
        min-height: 60px;
        resize: vertical;
    }

    /* ============= زر PDF ============= */
    #exportPDF {
        width: 95%;
        margin: 20px auto;
        display: block;
        background: linear-gradient(135deg, #5e35b1, #4527a0);
        padding: 20px;
        border: none;
        border-radius: 12px;
        color: white;
        font-size: 20px;
        font-weight: 800;
        cursor: pointer;
    }

    /* ============= إشعارات ============= */
    .notification {
        position: fixed;
        top: 20px;
        left: 50%;
        transform: translateX(-50%) translateY(-100px);
        background: #2e7d32;
        color: white;
        padding: 15px 25px;
        border-radius: 10px;
        box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        z-index: 1000;
        transition: transform 0.4s ease;
    }

    .notification.show {
        transform: translateX(-50%) translateY(0);
    }

    .notification.error {
        background: #c62828;
    }

    /* ============= التجاوب ============= */
    @media (max-width: 768px) {
        .admin-container {
            width: 98%;
            padding: 15px;
        }
        
        header h1 {
            font-size: 18px;
        }
        
        .btn {
            padding: 8px 12px;
            font-size: 13px;
        }
    }
</style>
</head>
<body>

<div class="main-blur">
    <header>
        <div class="school-logo">
            <i class="fas fa-graduation-cap"></i>
        </div>
        <h1>نظام متابعة الطلاب - نسخة اختبارية</h1>
        <h2>مادة اللغة الإنجليزية</h2>
        <button class="admin-btn" onclick="showAdminLogin()">
            <i class="fas fa-user-shield"></i> دخول المسؤول
        </button>
    </header>

    <!-- اختيار الفصل -->
    <div class="class-selector">
        <button onclick="showClass('c3_1')" class="active">الصف ٣/١</button>
        <button onclick="showClass('c2_3')">الصف ٢/٣</button>
        <button onclick="showClass('c3_3')">الصف ٣/٣</button>
    </div>

    <!-- التاريخ -->
    <div class="container">
        <div style="text-align: center; margin-bottom: 15px;">
            <label for="gregorianDate" style="display: block; margin-bottom: 10px; font-weight: 700;">
                التاريخ الميلادي:
            </label>
            <input type="date" id="gregorianDate" style="padding: 12px; border-radius: 8px; border: 2px solid #b0bec5; font-size: 16px;">
            <button onclick="setToday()" style="background: #f57c00; color: white; border: none; padding: 12px 20px; border-radius: 8px; margin-right: 10px; cursor: pointer;">
                <i class="fas fa-calendar-day"></i> تاريخ اليوم
            </button>
        </div>
    </div>

    <!-- محتوى الفصول -->
    <div id="classContent"></div>

    <!-- زر تصدير PDF -->
    <button id="exportPDF" onclick="generatePDF()">
        <i class="fas fa-file-pdf"></i> تصدير التقرير بصيغة PDF
    </button>
</div>

<!-- نافذة كلمة مرور Admin -->
<div class="password-modal" id="passwordModal">
    <div class="password-box">
        <h3><i class="fas fa-lock"></i> الوصول الإداري</h3>
        <p>الرجاء إدخال كلمة المرور للوصول إلى لوحة التحكم</p>
        <input type="password" id="adminPassword" class="password-input" placeholder="كلمة المرور" autocomplete="off">
        <div style="color: #c62828; display: none;" id="passwordError">
            <i class="fas fa-exclamation-circle"></i> كلمة المرور غير صحيحة
        </div>
        <button class="password-btn" onclick="checkAdminPassword()">
            <i class="fas fa-sign-in-alt"></i> دخول
        </button>
        <button class="password-btn" style="background: #b0bec5;" onclick="hideAdminLogin()">
            <i class="fas fa-times"></i> إلغاء
        </button>
    </div>
</div>

<!-- نافذة Admin الرئيسية -->
<div class="admin-modal" id="adminModal">
    <div class="admin-container">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px;">
            <h2 style="color: #4a148c; margin: 0;"><i class="fas fa-cogs"></i> لوحة التحكم الإدارية</h2>
            <button class="close-admin" onclick="hideAdminPanel()">
                <i class="fas fa-times"></i>
            </button>
        </div>
        
        <!-- التحضير العشوائي -->
        <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin-bottom: 20px;">
            <h3><i class="fas fa-random"></i> التحضير العشوائي</h3>
            <p style="margin-bottom: 15px;">تعيين حضور عشوائي للفصل المحدد بنسبة 75% حضور و 25% غياب</p>
            
            <div style="margin-bottom: 15px;">
                <label for="randomClassSelect" style="display: block; margin-bottom: 5px;">الفصل:</label>
                <select id="randomClassSelect" style="width: 100%; padding: 10px; border-radius: 8px;">
                    <option value="c3_1">الصف ٣/١</option>
                    <option value="c2_3">الصف ٢/٣</option>
                    <option value="c3_3">الصف ٣/٣</option>
                </select>
            </div>
            
            <button onclick="generateRandomAttendance()" style="background: linear-gradient(135deg, #f57c00, #ff9800); color: white; border: none; padding: 12px; border-radius: 10px; font-size: 16px; width: 100%; cursor: pointer;">
                <i class="fas fa-magic"></i> توليد تحضير عشوائي
            </button>
        </div>
        
        <!-- إدارة الطلاب -->
        <div style="background: #f8f9fa; padding: 20px; border-radius: 10px;">
            <h3><i class="fas fa-users-cog"></i> إدارة الطلاب</h3>
            <p style="margin-bottom: 15px;">إضافة طالب جديد إلى الفصل المحدد</p>
            
            <div style="display: grid; gap: 15px;">
                <div>
                    <label for="newStudentName" style="display: block; margin-bottom: 5px;">اسم الطالب:</label>
                    <input type="text" id="newStudentName" placeholder="اسم الطالب الكامل" style="width: 100%; padding: 10px; border-radius: 8px; border: 1px solid #b0bec5;">
                </div>
                
                <div>
                    <label for="newStudentClass" style="display: block; margin-bottom: 5px;">الفصل:</label>
                    <select id="newStudentClass" style="width: 100%; padding: 10px; border-radius: 8px;">
                        <option value="c3_1">الصف ٣/١</option>
                        <option value="c2_3">الصف ٢/٣</option>
                        <option value="c3_3">الصف ٣/٣</option>
                    </select>
                </div>
                
                <button onclick="addNewStudent()" style="background: linear-gradient(135deg, #2e7d32, #4caf50); color: white; border: none; padding: 12px; border-radius: 10px; font-size: 16px; cursor: pointer;">
                    <i class="fas fa-plus"></i> إضافة طالب
                </button>
            </div>
        </div>
    </div>
</div>

<!-- إشعارات -->
<div class="notification" id="notification"></div>

<script>
// ================== تهيئة المتغيرات ==================
const ADMIN_PASSWORD = "admin123";
let attendanceData = JSON.parse(localStorage.getItem('attendanceData')) || {};
let studentsData = JSON.parse(localStorage.getItem('studentsData')) || {};
let currentClass = 'c3_1';

// ================== إدارة Admin ==================
function showAdminLogin() {
    document.getElementById('passwordModal').classList.add('active');
    document.body.classList.add('blurred');
    document.getElementById('adminPassword').focus();
}

function hideAdminLogin() {
    document.getElementById('passwordModal').classList.remove('active');
    document.body.classList.remove('blurred');
    document.getElementById('adminPassword').value = '';
    document.getElementById('passwordError').style.display = 'none';
}

function checkAdminPassword() {
    const password = document.getElementById('adminPassword').value;
    const errorElement = document.getElementById('passwordError');
    
    if (password === ADMIN_PASSWORD) {
        hideAdminLogin();
        showAdminPanel();
        showNotification('تم الدخول بنجاح إلى لوحة التحكم', 'success');
    } else {
        errorElement.style.display = 'block';
        document.getElementById('adminPassword').value = '';
        document.getElementById('adminPassword').focus();
    }
}

function showAdminPanel() {
    document.getElementById('adminModal').classList.add('active');
    document.body.classList.add('blurred');
}

function hideAdminPanel() {
    document.getElementById('adminModal').classList.remove('active');
    document.body.classList.remove('blurred');
}

// ================== الميزة 2: التحضير العشوائي - معدّلة ==================
function generateRandomAttendance() {
    const classId = document.getElementById('randomClassSelect').value;
    const gregorianDate = document.getElementById('gregorianDate').value;
    
    if (!gregorianDate) {
        showNotification('الرجاء تحديد التاريخ أولاً', 'warning');
        return;
    }
    
    if (!attendanceData[classId]) {
        attendanceData[classId] = {};
    }
    
    const classStudents = studentsData[classId] || getDefaultStudents(classId);
    
    // توليد بيانات عشوائية بنسبة 75% حضور و25% غياب
    attendanceData[classId][gregorianDate] = {};
    
    classStudents.forEach((student, index) => {
        const random = Math.random();
        const isPresent = random < 0.75;
        
        attendanceData[classId][gregorianDate][index] = {
            present: isPresent,
            absent: !isPresent,
            note: isPresent ? 'حضور عشوائي' : 'غياب عشوائي'
        };
    });
    
    localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
    
    // تطبيق التحضير على الواجهة إذا كان الفصل النشط هو نفسه
    if (classId === currentClass) {
        applyAttendanceToUI(classId, gregorianDate);
    }
    
    showNotification('تم توليد تحضير عشوائي بنجاح', 'success');
}

function applyAttendanceToUI(classId, date) {
    if (!attendanceData[classId] || !attendanceData[classId][date]) {
        return;
    }
    
    const classAttendance = attendanceData[classId][date];
    const rows = document.querySelectorAll("#classContent table tbody tr");
    
    rows.forEach((row, index) => {
        if (classAttendance[index]) {
            const { present, absent, note } = classAttendance[index];
            
            if (present) {
                row.cells[1].querySelector('.btn.present').classList.add('active');
                row.cells[2].querySelector('.btn.absent').classList.remove('active');
            } else if (absent) {
                row.cells[1].querySelector('.btn.present').classList.remove('active');
                row.cells[2].querySelector('.btn.absent').classList.add('active');
            }
            
            if (note) {
                row.cells[3].querySelector('textarea').value = note;
            }
        }
    });
}

// ================== الميزة 3: إدارة الطلاب ==================
function addNewStudent() {
    const name = document.getElementById('newStudentName').value.trim();
    const classId = document.getElementById('newStudentClass').value;
    
    if (!name) {
        showNotification('الرجاء إدخال اسم الطالب', 'warning');
        return;
    }
    
    if (!studentsData[classId]) {
        studentsData[classId] = getDefaultStudents(classId);
    }
    
    studentsData[classId].push(name);
    localStorage.setItem('studentsData', JSON.stringify(studentsData));
    
    // تحديث العرض إذا كان الفصل النشط هو نفسه
    if (classId === currentClass) {
        loadClassStudents(classId);
    }
    
    document.getElementById('newStudentName').value = '';
    showNotification(`تم إضافة الطالب ${name} بنجاح`, 'success');
}

// ================== النظام الرئيسي ==================
function setToday() {
    const today = new Date();
    const formattedDate = today.toISOString().split('T')[0];
    document.getElementById('gregorianDate').value = formattedDate;
    
    // تطبيق بيانات التحضير للتاريخ الجديد
    applyAttendanceToUI(currentClass, formattedDate);
}

function showClass(classId) {
    document.querySelectorAll(".class-selector button").forEach(btn => {
        btn.classList.remove("active");
    });
    event.target.classList.add("active");
    
    currentClass = classId;
    loadClassStudents(classId);
    
    // تطبيق بيانات التحضير للتاريخ الحالي
    const gregorianDate = document.getElementById('gregorianDate').value;
    if (gregorianDate) {
        applyAttendanceToUI(classId, gregorianDate);
    }
}

function getDefaultStudents(classId) {
    const defaultClasses = {
        "c3_1": [
            "إسماعيل محمد هاشم شفيق الرحمن",
            "ابراهيم علي ابو بكر محمد",
            "باسم محمد - ابو طالب",
            "حسين بشير أمادو جازير"
        ],
        "c2_3": [
            "إبراهيم إدريس إبراهيم اولوجيوم",
            "إدريس محمد حسن أحمد",
            "امين عبداللّه دايابو عثمان",
            "بسام عبدالسلام هاشم انور علي"
        ],
        "c3_3": [
            "ابراهيم جزولي - اسدانور",
            "تركي عبدالصمد عبدالغني محمد حسين",
            "حسام حسن ابو الكلام مقبول احمد",
            "حسن عيسى بكوري محمد"
        ]
    };
    
    return studentsData[classId] || defaultClasses[classId] || [];
}

function loadClassStudents(classId) {
    const classStudents = getDefaultStudents(classId);
    
    let html = `
    <div class='container'>
        <h3>قائمة طلاب الفصل ${classId.replace('c', '').replace('_', '/')}</h3>
        <div style="overflow-x: auto;">
        <table>
            <thead>
                <tr>
                    <th width="40%">اسم الطالب</th>
                    <th width="15%">حضور</th>
                    <th width="15%">غياب</th>
                    <th width="30%">ملاحظات</th>
                </tr>
            </thead>
            <tbody>`;

    classStudents.forEach((name, index) => {
        html += `
                <tr>
                    <td style="text-align: right; font-weight: 700;">${name}</td>
                    <td>
                        <button class="btn present" onclick="toggleSelect(this,'present', '${classId}-${index}')">
                            <i class="fas fa-user-check"></i> حضور
                        </button>
                    </td>
                    <td>
                        <button class="btn absent" onclick="toggleSelect(this,'absent', '${classId}-${index}')">
                            <i class="fas fa-user-times"></i> غياب
                        </button>
                    </td>
                    <td><textarea placeholder="اكتب ملاحظاتك هنا..."></textarea></td>
                </tr>`;
    });

    html += `</tbody></table></div></div>`;
    document.getElementById("classContent").innerHTML = html;
}

function toggleSelect(btn, type, indicatorId) {
    const row = btn.closest("tr");
    
    // إزالة النشاط من جميع الأزرار في الصف
    row.querySelectorAll(".btn.present, .btn.absent")
        .forEach(b => b.classList.remove("active"));
    
    // تفعيل الزر المحدد
    btn.classList.add("active");
    
    // حفظ حالة الحضور
    const [classId, index] = indicatorId.split('-');
    const gregorianDate = document.getElementById('gregorianDate').value;
    
    if (gregorianDate) {
        if (!attendanceData[classId]) {
            attendanceData[classId] = {};
        }
        if (!attendanceData[classId][gregorianDate]) {
            attendanceData[classId][gregorianDate] = {};
        }
        
        attendanceData[classId][gregorianDate][index] = {
            present: type === 'present',
            absent: type === 'absent',
            note: row.querySelector('textarea').value || ''
        };
        
        localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
    }
}

// ================== استخراج PDF مع دعم العربية الكامل ==================
function generatePDF() {
    try {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF({
            orientation: 'landscape',
            unit: 'mm',
            format: 'a4'
        });
        
        // الحصول على البيانات
        const gregorianDate = document.getElementById("gregorianDate").value || "غير محدد";
        const activeClass = document.querySelector(".class-selector button.active").textContent;
        
        // عنوان التقرير بالعربية
        doc.setFontSize(20);
        doc.setTextColor(13, 71, 161);
        doc.text("تقرير متابعة الطلاب - مادة اللغة الإنجليزية", 150, 20, { align: 'center' });
        
        doc.setFontSize(16);
        doc.setTextColor(33, 33, 33);
        doc.text("المعلم: فهد الخالدي", 150, 30, { align: 'center' });
        
        // معلومات الفصل والتاريخ
        doc.setFontSize(12);
        doc.text(`الفصل: ${activeClass}`, 15, 45);
        doc.text(`التاريخ: ${gregorianDate}`, 15, 55);
        
        // جمع بيانات الطلاب
        const tableRows = document.querySelectorAll("#classContent table tbody tr");
        const tableData = [];
        
        tableRows.forEach((row, index) => {
            const name = row.cells[0].textContent.trim();
            const isPresent = row.cells[1].querySelector(".btn.present").classList.contains("active");
            const isAbsent = row.cells[2].querySelector(".btn.absent").classList.contains("active");
            const notes = row.cells[3].querySelector("textarea").value || "";
            
            let status = "لم يتم التحديد";
            
            if (isPresent) {
                status = "حاضر";
            } else if (isAbsent) {
                status = "غائب";
            }
            
            tableData.push([
                (index + 1).toString(),
                name,
                status,
                notes || "لا توجد ملاحظات"
            ]);
        });
        
        // إنشاء الجدول في PDF مع دعم العربية
        doc.autoTable({
            startY: 65,
            head: [['#', 'اسم الطالب', 'الحالة', 'ملاحظات']],
            body: tableData,
            theme: 'grid',
            styles: {
                font: 'helvetica',
                fontSize: 10,
                textColor: [33, 33, 33],
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
                1: { cellWidth: 60, halign: 'right' },
                2: { cellWidth: 25, halign: 'center', fontStyle: 'bold' },
                3: { cellWidth: 70, halign: 'right' }
            },
            didParseCell: function(data) {
                if (data.column.index === 2 && data.row.index > 0) {
                    const status = data.cell.raw;
                    if (status === "حاضر") {
                        data.cell.styles.textColor = [46, 125, 50];
                    } else if (status === "غائب") {
                        data.cell.styles.textColor = [198, 40, 40];
                    }
                }
            }
        });
        
        // توقيع وتاريخ الإصدار
        const finalY = doc.lastAutoTable.finalY + 15;
        doc.setFontSize(10);
        doc.text("توقيع المعلم: ________________", 15, finalY);
        doc.text(`تاريخ الإصدار: ${new Date().toLocaleDateString('ar-SA')}`, 260, finalY);
        
        // حفظ الملف
        const fileName = `تقرير-الحضور-${activeClass.replace('/', '-')}-${gregorianDate}.pdf`;
        doc.save(fileName);
        
        showNotification("تم تصدير التقرير بنجاح بصيغة PDF!", "success");
    } catch (error) {
        console.error("خطأ في إنشاء PDF:", error);
        showNotification("حدث خطأ أثناء إنشاء التقرير", "error");
    }
}

// ================== إشعارات ==================
function showNotification(message, type = 'success') {
    const notification = document.getElementById('notification');
    notification.textContent = message;
    notification.className = `notification ${type}`;
    notification.classList.add('show');
    
    setTimeout(() => {
        notification.classList.remove('show');
    }, 3000);
}

// ================== تهيئة النظام عند التحميل ==================
window.onload = function() {
    // تحميل الفصل الأول
    showClass("c3_1");
    
    // تعيين تاريخ اليوم
    setToday();
    
    // إضافة مستمع حدث لتاريخ اليوم
    document.getElementById('gregorianDate').addEventListener('change', function(e) {
        // تطبيق بيانات التحضير عند تغيير التاريخ
        applyAttendanceToUI(currentClass, e.target.value);
    });
    
    // السماح بإدخال كلمة المرور بالضغط على Enter
    document.getElementById('adminPassword').addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            checkAdminPassword();
        }
    });
};

// ================== السماح لجميع الوظائف بالعمل ==================
window.showClass = showClass;
window.toggleSelect = toggleSelect;
window.generatePDF = generatePDF;
window.showAdminLogin = showAdminLogin;
window.checkAdminPassword = checkAdminPassword;
window.hideAdminLogin = hideAdminLogin;
window.showAdminPanel = showAdminPanel;
window.hideAdminPanel = hideAdminPanel;
window.generateRandomAttendance = generateRandomAttendance;
window.addNewStudent = addNewStudent;
window.setToday = setToday;
</script>
</body>
</html>
