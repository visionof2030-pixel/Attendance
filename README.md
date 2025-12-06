<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سجل متابعة الطلاب - فهد الخالدي</title>
<style>
body {
    font-family: "Tajawal", sans-serif;
    margin: 0;
    padding: 0;
    background: #f7f7f7;
}

header {
    background: linear-gradient(135deg, #1a5276, #2a9d8f);
    color: #fff;
    text-align: center;
    padding: 12px 0;
    box-shadow: 0px 4px 6px rgba(0,0,0,0.1);
}

.header-main {
    font-size: 22px;
    font-weight: bold;
    margin-bottom: 5px;
}

.header-sub {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 20px;
    font-size: 14px;
    margin-top: 5px;
}

.header-sub div {
    padding: 4px 10px;
    background: rgba(255,255,255,0.15);
    border-radius: 4px;
}

.current-date {
    background: rgba(38, 70, 83, 0.8) !important;
    cursor: pointer;
    transition: all 0.3s;
}

.current-date:hover {
    background: rgba(29, 53, 87, 0.9) !important;
    transform: scale(1.05);
}

.class-header {
    background: #2a9d8f;
    color: white;
    padding: 8px;
    margin: 15px 0 5px 0;
    border-radius: 5px;
    text-align: center;
    font-size: 16px;
}

.container {
    width: 95%;
    margin: 10px auto;
    background: white;
    padding: 15px;
    border-radius: 10px;
    box-shadow: 0px 4px 10px rgba(0,0,0,0.1);
}

table {
    width: 100%;
    border-collapse: collapse;
    font-size: 12px;
    margin-bottom: 15px;
}

th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: center;
}

th {
    background: #e9f5f4;
    color: #264653;
    font-size: 11px;
    font-weight: bold;
}

td {
    cursor: pointer;
    user-select: none;
}

button {
    margin: 5px;
    padding: 8px 15px;
    border: none;
    border-radius: 5px;
    background: #1a5276;
    color: white;
    font-weight: bold;
    cursor: pointer;
    transition: all 0.3s;
}

button:hover {
    background: #2a9d8f;
    transform: translateY(-2px);
}

.controls {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-bottom: 15px;
    gap: 10px;
}

.admin-panel {
    display: none;
    margin-top: 15px;
    padding: 15px;
    border: 1px solid #1a5276;
    border-radius: 10px;
    background: #f0f8ff;
}

.star-cell {
    color: #ffd700;
    font-size: 16px;
}

.present {
    background-color: #e8f5e9;
}

.absent {
    background-color: #ffebee;
}

.status-filter {
    margin: 10px 0;
    text-align: center;
}

.status-filter button {
    background: #ddd;
    color: #333;
}

.status-filter button.active {
    background: #2a9d8f;
    color: white;
}

input[type="password"], input[type="text"], select {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-family: "Tajawal", sans-serif;
}

.class-tabs {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 15px;
    gap: 5px;
}

.class-tab {
    padding: 8px 15px;
    background: #e0e0e0;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s;
}

.class-tab.active {
    background: #2a9d8f;
    color: white;
}

.class-tab:hover {
    background: #c0c0c0;
}

.student-count {
    text-align: center;
    margin: 10px 0;
    color: #264653;
    font-weight: bold;
}

.date-controls {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
    margin: 10px 0;
    flex-wrap: wrap;
}

.date-controls button {
    padding: 6px 12px;
    font-size: 14px;
}

.date-display {
    font-size: 16px;
    font-weight: bold;
    color: #264653;
    padding: 5px 15px;
    background: #f0f8ff;
    border-radius: 5px;
    border: 1px solid #1a5276;
}

.date-input {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-family: "Tajawal", sans-serif;
}

.admin-section {
    margin: 15px 0;
    padding: 10px;
    background: #f9f9f9;
    border-radius: 8px;
    border: 1px solid #ddd;
}

.admin-section h4 {
    margin-top: 0;
    color: #1a5276;
    text-align: center;
    border-bottom: 1px solid #ddd;
    padding-bottom: 8px;
}

.admin-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin: 10px 0;
    flex-wrap: wrap;
}

.admin-label {
    font-weight: bold;
    color: #264653;
    min-width: 150px;
}

.admin-input {
    flex: 1;
    min-width: 200px;
}

.semester-info {
    display: inline-block;
    padding: 4px 10px;
    background: #e8f5e9;
    border-radius: 4px;
    color: #2a9d8f;
    font-weight: bold;
    margin-left: 10px;
}

@media print {
    button, .admin-panel, .status-filter, .class-tabs, .date-controls {
        display: none !important;
    }
    
    table {
        font-size: 10px;
    }
    
    .header-sub {
        background: white;
        color: black;
        border: 1px solid #ccc;
    }
    
    .current-date {
        background: white !important;
        color: black;
        border: 1px solid #ccc;
    }
}
</style>
</head>
<body>

<header>
    <div class="header-main">سجل متابعة الطلاب للمعلم / فهد الخالدي - المادة / اللغة الإنجليزية</div>
    <div class="header-sub">
        <div>المدرسة: ثانوية الملك فهد</div>
        <div class="current-date" id="currentDateDisplay" onclick="showDateSelector()">
            التاريخ: <span id="dateText">تحميل...</span>
        </div>
    </div>
</header>

<div class="container">
    <div class="controls">
        <button onclick="exportToExcel()">📊 تصدير Excel</button>
        <button onclick="printPage()">🖨️ طباعة</button>
        <button onclick="showAllClasses()">👁️ عرض الكل</button>
    </div>
    
    <div class="class-tabs" id="classTabs">
        <!-- سيتم إنشاء الألسنة ديناميكياً -->
    </div>
    
    <div class="status-filter">
        <button onclick="filterByStatus('all')" class="active">الكل</button>
        <button onclick="filterByStatus('present')">الحاضرون</button>
        <button onclick="filterByStatus('absent')">الغائبون</button>
        <button onclick="filterByStatus('star')">المتميزون ⭐</button>
    </div>
    
    <div id="tablesContainer">
        <!-- سيتم إنشاء الجداول ديناميكياً -->
    </div>
    
    <div class="student-count" id="studentCount">إجمالي الطلاب: 0</div>
    
    <div style="text-align: center; margin-top: 20px;">
        <input type="password" id="adminPass" placeholder="ادخل كلمة المرور للإدارة" style="width: 200px;">
        <button onclick="checkAdmin()">🔓 فتح الإدارة</button>
    </div>

    <div class="admin-panel" id="adminPanel">
        <h3 style="text-align:center; margin-top:0; color: #1a5276;">لوحة الإدارة - الخصائص الإدارية</h3>
        
        <div class="admin-section">
            <h4>🎓 إعدادات الفصل الدراسي</h4>
            <div class="admin-row">
                <div class="admin-label">الفصل الدراسي:</div>
                <div class="admin-input">
                    <select id="semesterSelect" onchange="updateSemester()" style="width: 100%;">
                        <option value="1">الفصل الدراسي الأول</option>
                        <option value="2" selected>الفصل الدراسي الثاني</option>
                        <option value="3">الفصل الدراسي الصيفي</option>
                    </select>
                </div>
            </div>
            <div class="admin-row">
                <div class="admin-label">السنة الدراسية:</div>
                <div class="admin-input">
                    <input type="text" id="academicYear" value="١٤٤٦هـ" style="width: 100%;">
                </div>
            </div>
            <div style="text-align: center; margin-top: 10px;">
                <button onclick="saveSemesterSettings()">💾 حفظ إعدادات الفصل</button>
                <span class="semester-info" id="currentSemesterInfo">الفصل الثاني ١٤٤٦هـ</span>
            </div>
        </div>
        
        <div class="admin-section">
            <h4>🕐 التحكم في التاريخ</h4>
            <div class="date-controls">
                <button onclick="changeMonth(-1)">◀ الشهر السابق</button>
                <div class="date-display" id="adminDateDisplay">...</div>
                <button onclick="changeMonth(1)">الشهر القادم ▶</button>
            </div>
            <div style="text-align: center; margin: 10px 0;">
                <input type="date" id="datePicker" class="date-input" onchange="setCustomDate()">
                <button onclick="resetToToday()">اليوم</button>
                <button onclick="saveCurrentDate()">💾 حفظ التاريخ</button>
            </div>
            <p style="text-align:center; font-size:12px; color:#666;">يمكنك الرجوع إلى أشهر سابقة أو قادمة لمشاهدة السجلات القديمة أو تحضير مستقبلية.</p>
        </div>
        
        <div class="admin-section">
            <h4>👨‍🏫 إدارة الطلاب</h4>
            <div style="text-align:center;">
                <button onclick="addStudent()">➕ إضافة طالب</button>
                <button onclick="randomAttendance()">🎲 تحضير عشوائي</button>
                <button onclick="moveStudent()">↔️ نقل طالب</button>
                <button onclick="resetAll()">🔄 إعادة تعيين</button>
            </div>
        </div>
        
        <div class="admin-section">
            <h4>📊 الإحصائيات</h4>
            <div style="text-align:center;">
                <button onclick="showStatistics()">📈 عرض الإحصائيات</button>
                <button onclick="backupData()">💾 نسخ احتياطي</button>
                <button onclick="loadBackup()">📂 استعادة نسخة</button>
            </div>
        </div>
        
        <p style="text-align:center; font-size:12px; color:#666;">بعد تفعيل الإدارة، يمكن تمييز الطلاب بالنجمة وإدارة جميع الخصائص.</p>
    </div>
</div>

<script>
// بيانات الطلاب لكل صف
const studentsData = {
    "3-1": [
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
    "2-3": [
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
    "3-3": [
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
    "4-3": [
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
    "5-3": [
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

// حالة الإدارة
let adminActive = false;
let currentFilter = 'all';
let currentClass = 'all';

// إدارة التاريخ
let currentDate = new Date();
let selectedDate = new Date();

// إعدادات الفصل الدراسي
let semesterSettings = {
    semester: "2",
    academicYear: "١٤٤٦هـ"
};

// تهيئة الصفحة
function initPage() {
    // محاولة تحميل التاريخ المحفوظ
    const savedDate = localStorage.getItem('teacherTracker_selectedDate');
    if (savedDate) {
        selectedDate = new Date(savedDate);
    }
    
    // محاولة تحميل إعدادات الفصل الدراسي
    const savedSemester = localStorage.getItem('teacherTracker_semesterSettings');
    if (savedSemester) {
        semesterSettings = JSON.parse(savedSemester);
        document.getElementById('semesterSelect').value = semesterSettings.semester;
        document.getElementById('academicYear').value = semesterSettings.academicYear;
        updateSemesterInfo();
    }
    
    // محاولة تحميل بيانات الحضور المحفوظة لهذا التاريخ
    loadAttendanceData();
    
    createClassTabs();
    createTables();
    updateStudentCount();
    updateDateDisplay();
    
    // تعيين التاريخ الحالي في منتقي التاريخ
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('datePicker').value = today;
}

// تحديث عرض التاريخ
function updateDateDisplay() {
    const options = { 
        weekday: 'long', 
        year: 'numeric', 
        month: 'long', 
        day: 'numeric' 
    };
    
    const hijriOptions = {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        calendar: 'islamic',
        numberingSystem: 'arab'
    };
    
    const gregorianDate = selectedDate.toLocaleDateString('ar-SA', options);
    const hijriDate = selectedDate.toLocaleDateString('ar-SA-u-ca-islamic', hijriOptions);
    
    document.getElementById('dateText').innerHTML = 
        `${gregorianDate}<br><span style="font-size:12px; color:#e0f7fa">${hijriDate}</span>`;
    
    document.getElementById('adminDateDisplay').innerHTML = 
        `${selectedDate.toLocaleDateString('ar-SA', { day: 'numeric', month: 'long', year: 'numeric' })}`;
}

// تحديث معلومات الفصل الدراسي المعروضة
function updateSemesterInfo() {
    const semesterNames = {
        "1": "الفصل الدراسي الأول",
        "2": "الفصل الدراسي الثاني", 
        "3": "الفصل الدراسي الصيفي"
    };
    
    const semesterName = semesterNames[semesterSettings.semester] || "الفصل الدراسي";
    document.getElementById('currentSemesterInfo').textContent = 
        `${semesterName} ${semesterSettings.academicYear}`;
}

// تحديث إعدادات الفصل الدراسي
function updateSemester() {
    semesterSettings.semester = document.getElementById('semesterSelect').value;
    semesterSettings.academicYear = document.getElementById('academicYear').value;
    updateSemesterInfo();
}

// حفظ إعدادات الفصل الدراسي
function saveSemesterSettings() {
    updateSemester();
    localStorage.setItem('teacherTracker_semesterSettings', JSON.stringify(semesterSettings));
    alert(`تم حفظ إعدادات الفصل الدراسي: ${document.getElementById('currentSemesterInfo').textContent}`);
}

// عرض منتقي التاريخ
function showDateSelector() {
    if (adminActive) {
        document.getElementById('datePicker').showPicker();
    } else {
        alert('يجب تفعيل وضع الإدارة لتغيير التاريخ');
    }
}

// تغيير الشهر (للسابق أو القادم)
function changeMonth(offset) {
    selectedDate.setMonth(selectedDate.getMonth() + offset);
    updateDateDisplay();
    saveCurrentDate();
    
    // تحميل بيانات الحضور للتاريخ الجديد
    loadAttendanceData();
    updateTablesWithLoadedData();
}

// تعيين تاريخ مخصص
function setCustomDate() {
    const datePicker = document.getElementById('datePicker');
    if (datePicker.value) {
        selectedDate = new Date(datePicker.value);
        updateDateDisplay();
        saveCurrentDate();
        
        // تحميل بيانات الحضور للتاريخ الجديد
        loadAttendanceData();
        updateTablesWithLoadedData();
    }
}

// الرجوع إلى تاريخ اليوم
function resetToToday() {
    selectedDate = new Date();
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('datePicker').value = today;
    updateDateDisplay();
    saveCurrentDate();
    
    // تحميل بيانات الحضور للتاريخ الجديد
    loadAttendanceData();
    updateTablesWithLoadedData();
}

// حفظ التاريخ الحالي
function saveCurrentDate() {
    localStorage.setItem('teacherTracker_selectedDate', selectedDate.toISOString());
    alert(`تم حفظ التاريخ: ${selectedDate.toLocaleDateString('ar-SA')}`);
}

// إنشاء ألسنة الصفوف
function createClassTabs() {
    const classTabs = document.getElementById('classTabs');
    classTabs.innerHTML = '<div class="class-tab active" onclick="showClass(\'all\')">جميع الصفوف</div>';
    
    for (const className in studentsData) {
        classTabs.innerHTML += `<div class="class-tab" onclick="showClass('${className}')">الصف ${className}</div>`;
    }
}

// إنشاء الجداول للصفوف
function createTables() {
    const container = document.getElementById('tablesContainer');
    container.innerHTML = '';
    
    for (const className in studentsData) {
        const classDiv = document.createElement('div');
        classDiv.className = 'class-section';
        classDiv.id = `class-${className}`;
        
        const classHeader = document.createElement('div');
        classHeader.className = 'class-header';
        classHeader.textContent = `الصف ${className} - ${studentsData[className].length} طالب`;
        
        const table = document.createElement('table');
        table.innerHTML = `
            <thead>
                <tr>
                    <th width="5%">م</th>
                    <th>الاسم</th>
                    <th width="10%">الحضور</th>
                    <th width="10%">الواجبات</th>
                    <th width="10%">المشروعات</th>
                    <th width="10%">تطبيقات وأنشطة</th>
                    <th width="10%">مشاركة</th>
                    <th width="10%">⭐</th>
                </tr>
            </thead>
            <tbody id="tbody-${className}">
            </tbody>
        `;
        
        classDiv.appendChild(classHeader);
        classDiv.appendChild(table);
        container.appendChild(classDiv);
        
        // ملء الجدول بالطلاب
        fillClassTable(className);
    }
    
    // عرض جميع الصفوف افتراضياً
    showClass('all');
}

// ملء جدول الصف بالطلاب
function fillClassTable(className) {
    const tbody = document.getElementById(`tbody-${className}`);
    tbody.innerHTML = '';
    
    studentsData[className].forEach((student, index) => {
        const row = document.createElement('tr');
        row.innerHTML = `
            <td>${index + 1}</td>
            <td>${student}</td>
            <td onclick="toggle(this)" class="present">✔</td>
            <td onclick="toggle(this)" class="present">✔</td>
            <td onclick="toggle(this)" class="present">✔</td>
            <td onclick="toggle(this)" class="present">✔</td>
            <td onclick="toggle(this)" class="present">✔</td>
            <td onclick="toggleStar(this)" class="star-cell">☆</td>
        `;
        tbody.appendChild(row);
    });
}

// تحميل بيانات الحضور المحفوظة
function loadAttendanceData() {
    // هذه الدالة ستقوم بتحميل بيانات الحضور المحفوظة للتاريخ المحدد
    // في هذه النسخة المبسطة، سنقوم فقط بتهيئة البيانات الفارغة
    console.log(`تحميل بيانات الحضور للتاريخ: ${selectedDate.toLocaleDateString()}`);
}

// تحديث الجداول بالبيانات المحملة
function updateTablesWithLoadedData() {
    // في تطبيق حقيقي، ستقوم بتحديث حالات الحضور بناء على البيانات المحملة
    console.log(`تحديث الجداول للتاريخ: ${selectedDate.toLocaleDateString()}`);
}

// عرض صف معين أو جميع الصفوف
function showClass(className) {
    currentClass = className;
    
    // تحديث الألسنة النشطة
    document.querySelectorAll('.class-tab').forEach(tab => {
        tab.classList.remove('active');
    });
    
    if (className === 'all') {
        document.querySelectorAll('.class-tab')[0].classList.add('active');
        document.querySelectorAll('.class-section').forEach(section => {
            section.style.display = 'block';
        });
    } else {
        document.querySelector(`.class-tab[onclick="showClass('${className}')"]`).classList.add('active');
        document.querySelectorAll('.class-section').forEach(section => {
            section.style.display = 'none';
        });
        document.getElementById(`class-${className}`).style.display = 'block';
    }
    
    // تطبيق الفلتر الحالي
    filterByStatus(currentFilter);
    updateStudentCount();
}

// عرض جميع الصفوف
function showAllClasses() {
    showClass('all');
}

// تبديل حالة ✔ و ✖
function toggle(cell) {
    if (cell.innerHTML === "✔") {
        cell.innerHTML = "✖";
        cell.classList.remove('present');
        cell.classList.add('absent');
    } else {
        cell.innerHTML = "✔";
        cell.classList.remove('absent');
        cell.classList.add('present');
    }
    
    // حفظ تغيير الحضور للتاريخ الحالي
    saveAttendanceData();
}

// تبديل النجمة
function toggleStar(cell) {
    if (adminActive) {
        cell.innerHTML = cell.innerHTML === "☆" ? "⭐" : "☆";
        saveAttendanceData();
    } else {
        alert('يجب تفعيل وضع الإدارة أولا');
    }
}

// حفظ بيانات الحضور
function saveAttendanceData() {
    // في تطبيق حقيقي، ستقوم بحفظ بيانات الحضور للتاريخ المحدد
    const dateKey = selectedDate.toISOString().split('T')[0];
    console.log(`حفظ بيانات الحضور للتاريخ: ${dateKey}`);
    
    // تخزين مؤقت في localStorage للتجربة
    localStorage.setItem(`teacherTracker_attendance_${dateKey}`, 'بيانات الحضور المحفوظة');
}

// التحقق من كلمة المرور
function checkAdmin() {
    const pass = document.getElementById("adminPass").value;
    if (pass === "1406") {
        adminActive = true;
        document.getElementById("adminPanel").style.display = "block";
        alert("تم تفعيل خصائص الإدارة بنجاح");
    } else {
        alert("كلمة مرور خاطئة");
    }
}

// إضافة طالب جديد
function addStudent() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    const className = prompt("ادخل رقم الصف (مثال: 3-1)");
    if (!className || !studentsData[className]) {
        alert("رقم الصف غير صحيح");
        return;
    }
    
    const name = prompt("ادخل اسم الطالب");
    if (name) {
        studentsData[className].push(name);
        
        // إعادة ملء الجدول
        fillClassTable(className);
        updateStudentCount();
        
        // تحديث عنوان الصف
        document.querySelector(`#class-${className} .class-header`).textContent = 
            `الصف ${className} - ${studentsData[className].length} طالب`;
        
        alert("تمت إضافة الطالب بنجاح");
    }
}

// تحضير عشوائي
function randomAttendance() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    const confirmAction = confirm("هل تريد تعيين الحضور عشوائيا لجميع الطلاب؟");
    if (!confirmAction) return;
    
    document.querySelectorAll('td[onclick="toggle(this)"]').forEach(cell => {
        cell.innerHTML = Math.random() > 0.3 ? "✔" : "✖";
        if (cell.innerHTML === "✔") {
            cell.classList.remove('absent');
            cell.classList.add('present');
        } else {
            cell.classList.remove('present');
            cell.classList.add('absent');
        }
    });
    
    saveAttendanceData();
    alert("تم تعيين الحضور عشوائيا");
}

// نقل طالب
function moveStudent() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    alert("ميزة النقل: سيتم تطويرها في النسخة القادمة");
}

// إعادة تعيين الكل
function resetAll() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    const confirmAction = confirm("هل تريد إعادة تعيين جميع البيانات؟");
    if (!confirmAction) return;
    
    document.querySelectorAll('td[onclick="toggle(this)"]').forEach(cell => {
        cell.innerHTML = "✔";
        cell.classList.remove('absent');
        cell.classList.add('present');
    });
    
    document.querySelectorAll('.star-cell').forEach(cell => {
        cell.innerHTML = "☆";
    });
    
    saveAttendanceData();
    alert("تمت إعادة التعيين بنجاح");
}

// عرض الإحصائيات
function showStatistics() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    let presentCount = 0;
    let absentCount = 0;
    let starCount = 0;
    let totalStudents = 0;
    
    document.querySelectorAll('td[onclick="toggle(this)"]').forEach(cell => {
        if (cell.innerHTML === "✔") presentCount++;
        else absentCount++;
    });
    
    document.querySelectorAll('.star-cell').forEach(cell => {
        if (cell.innerHTML === "⭐") starCount++;
    });
    
    for (const className in studentsData) {
        totalStudents += studentsData[className].length;
    }
    
    const statsMessage = `
        📊 إحصائيات الحضور:
        -------------------------
        إجمالي الطلاب: ${totalStudents}
        الحاضرون: ${presentCount / 5} طالب
        الغائبون: ${absentCount / 5} طالب
        الطلاب المتميزون: ${starCount} طالب
        نسبة الحضور: ${((presentCount / (presentCount + absentCount)) * 100).toFixed(1)}%
        التاريخ: ${selectedDate.toLocaleDateString('ar-SA')}
        ${document.getElementById('currentSemesterInfo').textContent}
    `;
    
    alert(statsMessage);
}

// نسخ احتياطي للبيانات
function backupData() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    const backup = {
        studentsData: studentsData,
        selectedDate: selectedDate.toISOString(),
        semesterSettings: semesterSettings,
        backupDate: new Date().toISOString()
    };
    
    localStorage.setItem('teacherTracker_backup', JSON.stringify(backup));
    alert("تم إنشاء نسخة احتياطية بنجاح");
}

// استعادة نسخة احتياطية
function loadBackup() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    const backup = localStorage.getItem('teacherTracker_backup');
    if (!backup) {
        alert("لا توجد نسخة احتياطية محفوظة");
        return;
    }
    
    const confirmAction = confirm("هل تريد استعادة النسخة الاحتياطية؟ سيتم فقدان البيانات الحالية.");
    if (!confirmAction) return;
    
    try {
        const backupData = JSON.parse(backup);
        // في تطبيق حقيقي، ستقوم باستعادة البيانات من backupData
        alert("تم استعادة النسخة الاحتياطية بنجاح");
    } catch (error) {
        alert("حدث خطأ في استعادة النسخة الاحتياطية");
    }
}

// تصدير إلى Excel
function exportToExcel() {
    let tablesHTML = `<h2>سجل متابعة الطلاب - المعلم: فهد الخالدي</h2>`;
    tablesHTML += `<h3>المادة: اللغة الإنجليزية - ${document.getElementById('currentSemesterInfo').textContent}</h3>`;
    tablesHTML += `<h3>التاريخ: ${selectedDate.toLocaleDateString('ar-SA')}</h3>`;
    
    for (const className in studentsData) {
        tablesHTML += `<h3>الصف ${className}</h3>`;
        tablesHTML += document.getElementById(`class-${className}`).querySelector('table').outerHTML;
    }
    
    let uri = 'data:application/vnd.ms-excel;base64,';
    let template = `<html xmlns:o="urn:schemas-microsoft-com:office:office" 
                   xmlns:x="urn:schemas-microsoft-com:office:excel" 
                   xmlns="http://www.w3.org/TR/REC-html40">
                   <head>
                   <meta charset="UTF-8">
                   <!--[if gte mso 9]>
                   <xml>
                   <x:ExcelWorkbook>
                   <x:ExcelWorksheets>
                   <x:ExcelWorksheet>
                   <x:Name>تقرير الطلاب</x:Name>
                   <x:WorksheetOptions><x:DisplayGridlines/></x:WorksheetOptions>
                   </x:ExcelWorksheet>
                   </x:ExcelWorksheets>
                   </x:ExcelWorkbook>
                   </xml>
                   <![endif]-->
                   </head>
                   <body dir="rtl">${tablesHTML}</body></html>`;
    
    let link = document.createElement("a");
    link.href = uri + btoa(unescape(encodeURIComponent(template)));
    link.download = `تقرير_الطلاب_${selectedDate.toISOString().split('T')[0]}.xls`;
    link.click();
}

// طباعة الصفحة
function printPage() {
    window.print();
}

// تصفية حسب الحالة
function filterByStatus(status) {
    currentFilter = status;
    
    // تحديث أزرار الفلتر
    document.querySelectorAll('.status-filter button').forEach(btn => {
        btn.classList.remove('active');
    });
    event.target.classList.add('active');
    
    // تحديد الصفوف المراد عرضها
    let classSections = document.querySelectorAll('.class-section');
    if (currentClass !== 'all') {
        classSections = [document.getElementById(`class-${currentClass}`)];
    }
    
    classSections.forEach(section => {
        const rows = section.querySelectorAll('tbody tr');
        rows.forEach(row => {
            let showRow = false;
            
            if (status === 'all') {
                showRow = true;
            } else if (status === 'present') {
                const attendanceCells = row.querySelectorAll('td[onclick="toggle(this)"]');
                const allPresent = Array.from(attendanceCells).every(cell => cell.innerHTML === "✔");
                showRow = allPresent;
            } else if (status === 'absent') {
                const attendanceCells = row.querySelectorAll('td[onclick="toggle(this)"]');
                const anyAbsent = Array.from(attendanceCells).some(cell => cell.innerHTML === "✖");
                showRow = anyAbsent;
            } else if (status === 'star') {
                const starCell = row.querySelector('.star-cell');
                showRow = starCell && starCell.innerHTML === "⭐";
            }
            
            row.style.display = showRow ? '' : 'none';
        });
    });
}

// تحديث عدد الطلاب
function updateStudentCount() {
    let totalStudents = 0;
    
    if (currentClass === 'all') {
        for (const className in studentsData) {
            totalStudents += studentsData[className].length;
        }
    } else {
        totalStudents = studentsData[currentClass].length;
    }
    
    document.getElementById('studentCount').textContent = `إجمالي الطلاب: ${totalStudents}`;
}

// تهيئة الصفحة عند التحميل
window.onload = initPage;
</script>
</body>
</html>
