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
    transition: all 0.3s;
}

.date-info {
    font-size: 12px;
    color: #e0f7fa;
    margin-top: 2px;
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

.hijri-date-selector {
    background: #fff8e1;
    border: 1px solid #ffd54f;
    border-radius: 5px;
    padding: 10px;
    margin-top: 10px;
}

.starred-student {
    background-color: #fffde7 !important;
}

.random-period-section {
    background: #e8f5e9;
    border: 1px solid #2a9d8f;
    border-radius: 5px;
    padding: 10px;
    margin-top: 10px;
}

.period-info {
    display: inline-block;
    padding: 4px 10px;
    background: #2a9d8f;
    color: white;
    border-radius: 4px;
    font-weight: bold;
    margin-left: 10px;
    font-size: 12px;
}

.week-buttons {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
    gap: 8px;
    margin: 15px 0;
}

.week-button {
    padding: 12px 5px;
    background: #e0e0e0;
    border: 2px solid #ddd;
    border-radius: 5px;
    cursor: pointer;
    font-weight: bold;
    transition: all 0.3s;
    text-align: center;
    position: relative;
}

.week-button:hover {
    background: #d0d0d0;
    transform: translateY(-2px);
}

.week-button.selected {
    background: #4CAF50 !important;
    color: white !important;
    border-color: #388E3C !important;
    box-shadow: 0 2px 5px rgba(0,0,0,0.2);
}

.week-button.missing {
    background: #ffcccc;
    color: #666;
    cursor: not-allowed;
    opacity: 0.5;
}

.week-button.single-week {
    background: #bbdefb;
    border-color: #2196F3;
}

.selected-weeks-display {
    background: #e3f2fd;
    border: 1px solid #2196F3;
    border-radius: 5px;
    padding: 10px;
    margin: 10px 0;
    text-align: center;
}

.export-section {
    background: #fff8e1;
    border: 1px solid #ffb300;
    border-radius: 5px;
    padding: 15px;
    margin-top: 20px;
}

.week-info {
    font-size: 11px;
    color: #666;
    margin-top: 3px;
    display: block;
}

.week-select-controls {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-bottom: 15px;
    flex-wrap: wrap;
}

.checkbox-label {
    display: flex;
    align-items: center;
    gap: 5px;
    cursor: pointer;
}

.week-number {
    font-size: 16px;
    font-weight: bold;
    display: block;
}

.week-dates {
    font-size: 10px;
    color: #666;
}

.batch-selection {
    background: #f5f5f5;
    border-radius: 5px;
    padding: 10px;
    margin: 10px 0;
}

.batch-buttons {
    display: flex;
    justify-content: center;
    gap: 10px;
    flex-wrap: wrap;
    margin-top: 10px;
}

@media print {
    button, .admin-panel, .status-filter, .class-tabs, .date-controls, .week-buttons, .selected-weeks-display, .export-section {
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
<!-- مكتبة ummAlQura لحساب التاريخ الهجري -->
<script src="https://cdn.jsdelivr.net/npm/hijri-date/lib/simple.umd.min.js"></script>
</head>
<body>

<header>
    <div class="header-main">سجل متابعة الطلاب للمعلم / فهد الخالدي - المادة / اللغة الإنجليزية</div>
    <div class="header-sub">
        <div>المدرسة: سعيد بن العاص المتوسطة</div>
        <div class="current-date">
            <div>تاريخ اليوم:</div>
            <div id="gregorianDateText">تحميل...</div>
            <div class="date-info" id="hijriDateText">تحميل التاريخ الهجري...</div>
        </div>
    </div>
</header>

<div class="container">
    <div class="controls">
        <button onclick="exportToExcel()">📊 تصدير اليوم Excel</button>
        <button onclick="exportSelectedWeeks()">📅 تصدير الأسابيع المحددة</button>
        <button onclick="printPage()">🖨️ طباعة</button>
        <button onclick="showAllClasses()">👁️ عرض الكل</button>
        <button onclick="showTodayAttendance()">📅 عرض تحضير اليوم</button>
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
                    <input type="text" id="academicYear" value="١٤٤٦-١٤٤٧هـ" style="width: 100%;">
                </div>
            </div>
            <div style="text-align: center; margin-top: 10px;">
                <button onclick="saveSemesterSettings()">💾 حفظ إعدادات الفصل</button>
                <span class="semester-info" id="currentSemesterInfo">الفصل الثاني ١٤٤٦-١٤٤٧هـ</span>
            </div>
        </div>
        
        <div class="admin-section">
            <h4>📅 التحضير الأسبوعي (الأسابيع الدراسية الفعلية)</h4>
            
            <div class="selected-weeks-display" id="selectedWeeksDisplay">
                <strong>الأسابيع المحددة:</strong> <span id="selectedWeeksText">لا توجد أسابيع محددة</span>
                <br>
                <span id="selectedWeeksCount">0 أسبوع | 0 يوم</span>
            </div>
            
            <div class="week-select-controls">
                <button onclick="selectAllWeeks()">📋 تحديد الكل</button>
                <button onclick="clearSelectedWeeks()">🗑️ مسح الكل</button>
                <button onclick="selectFirstSemester()">📚 الفصل الأول (1-8)</button>
                <button onclick="selectSecondSemester()">📘 الفصل الثاني (9-19)</button>
            </div>
            
            <div class="batch-selection">
                <strong>تحديد دفعة:</strong>
                <div class="batch-buttons">
                    <button onclick="selectWeeksBatch(1, 4)">الأسابيع 1-4</button>
                    <button onclick="selectWeeksBatch(5, 8)">الأسابيع 5-8</button>
                    <button onclick="selectWeeksBatch(9, 12)">الأسابيع 9-12</button>
                    <button onclick="selectWeeksBatch(13, 16)">الأسابيع 13-16</button>
                    <button onclick="selectWeeksBatch(17, 19)">الأسابيع 17-19</button>
                </div>
            </div>
            
            <div class="week-buttons" id="weekButtons">
                <!-- سيتم إنشاء أزرار الأسابيع ديناميكياً -->
            </div>
            
            <div style="text-align: center; margin-top: 15px;">
                <button onclick="randomAttendanceForSelectedWeeks()" style="background: #4CAF50; padding: 10px 20px; font-size: 16px;">
                    🎲 تحضير عشوائي للأسابيع المحددة
                </button>
            </div>
            
            <div style="text-align:center; margin-top:10px; font-size:12px; color:#666;">
                ⭐ خاصية التحضير العشوائي: سيتم وضع ✓ لكل الخيارات للطلاب المتميزين (الذين لديهم نجمة ⭐)
            </div>
        </div>
        
        <div class="export-section">
            <h4>📤 تصدير التقارير</h4>
            <div style="text-align: center; margin-top: 10px;">
                <button onclick="exportSelectedWeeks()" style="background: #4CAF50; padding: 12px 24px; font-size: 14px;">
                    📥 تصدير الأسابيع المحددة إلى Excel
                </button>
                <button onclick="exportAllWeeks()" style="background: #2196F3; padding: 12px 24px; font-size: 14px;">
                    📚 تصدير جميع الأسابيع إلى Excel
                </button>
            </div>
            <div style="text-align: center; margin-top: 10px;">
                <button onclick="generateWeeklyReport()" style="background: #FF9800; padding: 12px 24px; font-size: 14px;">
                    📈 إنشاء تقرير إحصائي مفصل
                </button>
            </div>
        </div>
        
        <div class="admin-section">
            <h4>👨‍🏫 إدارة الطلاب</h4>
            <div style="text-align:center;">
                <button onclick="randomAttendance()">🎲 تحضير عشوائي للتاريخ الحالي</button>
                <button onclick="addStudent()">➕ إضافة طالب</button>
                <button onclick="resetAll()">🔄 إعادة تعيين</button>
            </div>
        </div>
        
        <div style="text-align:center; margin-top:20px;">
            <button onclick="checkAdmin()" style="background: #f44336;">🔒 إغلاق لوحة الإدارة</button>
        </div>
    </div>
</div>

<script>
// بيانات الطلاب لكل صف (محدثة حسب القائمة المقدمة)
const studentsData = {
    "3-1": [
        "إسماعيل محمد هاشم شفيق الرحمن",
        "ابراهيم علي ابو بكر محمد",
        "باسم محمد ابو طالب",
        "حسين بشير أمادو جازير",
        "حسين هارون عثمان عبدالمؤمن ادم",
        "حمد محمد عثمان بخش",
        "رمضان عيسى باكور محمد",
        "ريان عبد الرحمن موسى جيبو",
        "ريحان محمد مقبول حسين عمر حمزه",
        "عامر مولوي حسن شريف",
        "عبدالحليم نور كبير صديق احمد",
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
        "صالح عبدالله محمد قاسم يوسف علي",
        "عبدالعزيز اول اودو محمد",
        "عثمان عبد الرحمن باي محمد",
        "عدنان نور امير حسين",
        "عمر سراج محمد زكريا",
        "فهد محمد حسين عبداللّه مياه حسين",
        "محمد ابراهيم سعيد هو ساوي",
        "محمد محمد امين اسلام خليل الرحمن",
        "مشعل ابو طاهر ناظر حسين عبدالمطلب",
        "موسى ابو بكر الصديق عبدالجبار امة علي",
        "يوسف مهدي عابدين محمد"
    ],
    "3-3": [
        "ابراهيم جزولي اسدانور",
        "تركي عبدالصمد عبدالغني محمد حسين",
        "حسام حسن ابو الكلام مقبول احمد",
        "حسن عيسى بكوري محمد",
        "سعد سلام ستار ارشاد اللّٰه",
        "عايض سيف الاسلام نور احمد علي",
        "عبدالكريم عثمان ابكر كوجو",
        "عزام شمس العالم قاسم علي",
        "عماد محمد صديق محمد شفيع سيد",
        "عمر عبد القدوس عبدالسلام عبد السبحان",
        "عمر مورتلا أبو بكر محمد",
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
        "عبدالحليم محمد عبدالله عبدالحkيم",
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
        "عبدالله عيسى ابراهيم",
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

// بيانات الأسابيع الدراسية (محدثة حسب الجدول المقدم)
const studyWeeks = {
    1: {
        name: "الأسبوع 1",
        days: [
            { day: "الأحد", gregorian: "2025/08/24", hijri: "1447/03/01" },
            { day: "الاثنين", gregorian: "2025/08/25", hijri: "1447/03/02" },
            { day: "الثلاثاء", gregorian: "2025/08/26", hijri: "1447/03/03" },
            { day: "الأربعاء", gregorian: "2025/08/27", hijri: "1447/03/04" },
            { day: "الخميس", gregorian: "2025/08/28", hijri: "1447/03/05" }
        ]
    },
    2: {
        name: "الأسبوع 2",
        days: [
            { day: "الأحد", gregorian: "2025/08/31", hijri: "1447/03/08" },
            { day: "الاثنين", gregorian: "2025/09/01", hijri: "1447/03/09" },
            { day: "الثلاثاء", gregorian: "2025/09/02", hijri: "1447/03/10" },
            { day: "الأربعاء", gregorian: "2025/09/03", hijri: "1447/03/11" },
            { day: "الخميس", gregorian: "2025/09/04", hijri: "1447/03/12" }
        ]
    },
    3: {
        name: "الأسبوع 3",
        days: [
            { day: "الأحد", gregorian: "2025/09/07", hijri: "1447/03/15" },
            { day: "الاثنين", gregorian: "2025/09/08", hijri: "1447/03/16" },
            { day: "الثلاثاء", gregorian: "2025/09/09", hijri: "1447/03/17" },
            { day: "الأربعاء", gregorian: "2025/09/10", hijri: "1447/03/18" },
            { day: "الخميس", gregorian: "2025/09/11", hijri: "1447/03/19" }
        ]
    },
    4: {
        name: "الأسبوع 4",
        days: [
            { day: "الأحد", gregorian: "2025/09/14", hijri: "1447/03/22" },
            { day: "الاثنين", gregorian: "2025/09/15", hijri: "1447/03/23" },
            { day: "الثلاثاء", gregorian: "2025/09/16", hijri: "1447/03/24" },
            { day: "الأربعاء", gregorian: "2025/09/17", hijri: "1447/03/25" },
            { day: "الخميس", gregorian: "2025/09/18", hijri: "1447/03/26" }
        ]
    },
    5: {
        name: "الأسبوع 5",
        days: [
            { day: "الأحد", gregorian: "2025/09/21", hijri: "1447/03/29" },
            { day: "الاثنين", gregorian: "2025/09/22", hijri: "1447/03/30" },
            { day: "الثلاثاء", gregorian: "2025/09/23", hijri: "1447/03/31" },
            { day: "الأربعاء", gregorian: "2025/09/24", hijri: "1447/04/01" },
            { day: "الخميس", gregorian: "2025/09/25", hijri: "1447/04/02" }
        ]
    },
    6: {
        name: "الأسبوع 6",
        days: [
            { day: "الأحد", gregorian: "2025/09/28", hijri: "1447/04/05" },
            { day: "الاثنين", gregorian: "2025/09/29", hijri: "1447/04/06" },
            { day: "الثلاثاء", gregorian: "2025/09/30", hijri: "1447/04/07" },
            { day: "الأربعاء", gregorian: "2025/10/01", hijri: "1447/04/08" },
            { day: "الخميس", gregorian: "2025/10/02", hijri: "1447/04/09" }
        ]
    },
    7: {
        name: "الأسبوع 7",
        days: [
            { day: "الأحد", gregorian: "2025/10/05", hijri: "1447/04/12" },
            { day: "الاثنين", gregorian: "2025/10/06", hijri: "1447/04/13" },
            { day: "الثلاثاء", gregorian: "2025/10/07", hijri: "1447/04/14" },
            { day: "الأربعاء", gregorian: "2025/10/08", hijri: "1447/04/15" },
            { day: "الخميس", gregorian: "2025/10/09", hijri: "1447/04/16" }
        ]
    },
    8: {
        name: "الأسبوع 8",
        days: [
            { day: "الاثنين", gregorian: "2025/10/13", hijri: "1447/04/20" },
            { day: "الثلاثاء", gregorian: "2025/10/14", hijri: "1447/04/21" },
            { day: "الأربعاء", gregorian: "2025/10/15", hijri: "1447/04/22" },
            { day: "الخميس", gregorian: "2025/10/16", hijri: "1447/04/23" }
        ]
    },
    9: {
        name: "الأسبوع 9",
        days: [
            { day: "الأحد", gregorian: "2025/10/19", hijri: "1447/04/26" },
            { day: "الاثنين", gregorian: "2025/10/20", hijri: "1447/04/27" },
            { day: "الثلاثاء", gregorian: "2025/10/21", hijri: "1447/04/28" },
            { day: "الأربعاء", gregorian: "2025/10/22", hijri: "1447/04/29" },
            { day: "الخميس", gregorian: "2025/10/23", hijri: "1447/04/30" }
        ]
    },
    10: {
        name: "الأسبوع 10",
        days: [
            { day: "الأحد", gregorian: "2025/10/26", hijri: "1447/05/03" },
            { day: "الاثنين", gregorian: "2025/10/27", hijri: "1447/05/04" },
            { day: "الثلاثاء", gregorian: "2025/10/28", hijri: "1447/05/05" },
            { day: "الأربعاء", gregorian: "2025/10/29", hijri: "1447/05/06" },
            { day: "الخميس", gregorian: "2025/10/30", hijri: "1447/05/07" }
        ]
    },
    11: {
        name: "الأسبوع 11",
        days: [
            { day: "الأحد", gregorian: "2025/11/02", hijri: "1447/05/10" },
            { day: "الاثنين", gregorian: "2025/11/03", hijri: "1447/05/11" },
            { day: "الثلاثاء", gregorian: "2025/11/04", hijri: "1447/05/12" },
            { day: "الأربعاء", gregorian: "2025/11/05", hijri: "1447/05/13" },
            { day: "الخميس", gregorian: "2025/11/06", hijri: "1447/05/14" }
        ]
    },
    12: {
        name: "الأسبوع 12",
        days: [
            { day: "الأحد", gregorian: "2025/11/09", hijri: "1447/05/17" },
            { day: "الاثنين", gregorian: "2025/11/10", hijri: "1447/05/18" },
            { day: "الثلاثاء", gregorian: "2025/11/11", hijri: "1447/05/19" },
            { day: "الأربعاء", gregorian: "2025/11/12", hijri: "1447/05/20" },
            { day: "الخميس", gregorian: "2025/11/13", hijri: "1447/05/21" }
        ]
    },
    13: {
        name: "الأسبوع 13",
        days: [
            { day: "الأحد", gregorian: "2025/11/16", hijri: "1447/05/24" },
            { day: "الاثنين", gregorian: "2025/11/17", hijri: "1447/05/25" },
            { day: "الثلاثاء", gregorian: "2025/11/18", hijri: "1447/05/26" },
            { day: "الأربعاء", gregorian: "2025/11/19", hijri: "1447/05/27" },
            { day: "الخميس", gregorian: "2025/11/20", hijri: "1447/05/28" }
        ]
    },
    14: {
        name: "الأسبوع 14",
        days: [],
        missing: true
    },
    15: {
        name: "الأسبوع 15",
        days: [
            { day: "الأحد", gregorian: "2025/11/30", hijri: "1447/06/09" },
            { day: "الاثنين", gregorian: "2025/12/01", hijri: "1447/06/10" },
            { day: "الثلاثاء", gregorian: "2025/12/02", hijri: "1447/06/11" },
            { day: "الأربعاء", gregorian: "2025/12/03", hijri: "1447/06/12" }
        ]
    },
    16: {
        name: "الأسبوع 16",
        days: [
            { day: "الاثنين", gregorian: "2025/12/08", hijri: "1447/06/17" },
            { day: "الثلاثاء", gregorian: "2025/12/09", hijri: "1447/06/18" },
            { day: "الأربعاء", gregorian: "2025/12/10", hijri: "1447/06/19" },
            { day: "الخميس", gregorian: "2025/12/11", hijri: "1447/06/20" }
        ]
    },
    17: {
        name: "الأسبوع 17",
        days: [
            { day: "الأحد", gregorian: "2025/12/14", hijri: "1447/06/23" },
            { day: "الاثنين", gregorian: "2025/12/15", hijri: "1447/06/24" },
            { day: "الثلاثاء", gregorian: "2025/12/16", hijri: "1447/06/25" },
            { day: "الأربعاء", gregorian: "2025/12/17", hijri: "1447/06/26" },
            { day: "الخميس", gregorian: "2025/12/18", hijri: "1447/06/27" }
        ]
    },
    18: {
        name: "الأسبوع 18",
        days: [
            { day: "الأحد", gregorian: "2025/12/21", hijri: "1447/07/01" },
            { day: "الاثنين", gregorian: "2025/12/22", hijri: "1447/07/02" },
            { day: "الثلاثاء", gregorian: "2025/12/23", hijri: "1447/07/03" },
            { day: "الأربعاء", gregorian: "2025/12/24", hijri: "1447/07/04" },
            { day: "الخميس", gregorian: "2025/12/25", hijri: "1447/07/05" }
        ]
    },
    19: {
        name: "الأسبوع 19",
        days: [
            { day: "الأحد", gregorian: "2025/12/28", hijri: "1447/07/08" },
            { day: "الاثنين", gregorian: "2025/12/29", hijri: "1447/07/09" },
            { day: "الثلاثاء", gregorian: "2025/12/30", hijri: "1447/07/10" },
            { day: "الأربعاء", gregorian: "2025/12/31", hijri: "1447/07/11" },
            { day: "الخميس", gregorian: "2026/01/01", hijri: "1447/07/12" }
        ]
    }
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
    academicYear: "١٤٤٦-١٤٤٧هـ"
};

// الأسابيع المحددة
let selectedWeeks = [];

// بيانات التحضير المخزنة لكل يوم
let periodAttendanceData = {};

// التاريخ الهجري
let hijriDate = {
    day: 1,
    month: 1,
    year: 1446,
    monthName: "محرم"
};

// أسماء الأشهر الهجرية
const hijriMonths = [
    "محرم", "صفر", "ربيع الأول", "ربيع الثاني", 
    "جمادى الأولى", "جمادى الآخرة", "رجب", "شعبان", 
    "رمضان", "شوال", "ذو القعدة", "ذو الحجة"
];

// أسماء الأشهر الميلادية بالعربية
const gregorianMonths = [
    "يناير", "فبراير", "مارس", "أبريل", "مايو", "يونيو",
    "يوليو", "أغسطس", "سبتمبر", "أكتوبر", "نوفمبر", "ديسمبر"
];

// أيام الأسبوع بالعربية
const weekDays = [
    "الأحد", "الاثنين", "الثلاثاء", "الأربعاء", "الخميس", "الجمعة", "السبت"
];

// تحويل الأرقام الإنجليزية إلى عربية
function convertToArabicNumbers(num) {
    const arabicNumbers = ['٠', '١', '٢', '٣', '٤', '٥', '٦', '٧', '٨', '٩'];
    return num.toString().replace(/\d/g, digit => arabicNumbers[digit]);
}

// الحصول على التاريخ الميلادي قصير للتقرير
function getShortGregorianDate(date) {
    const day = date.getDate();
    const month = gregorianMonths[date.getMonth()];
    const year = date.getFullYear();
    
    const arabicDay = convertToArabicNumbers(day);
    const arabicYear = convertToArabicNumbers(year);
    
    return `${arabicDay} ${month} ${arabicYear}`;
}

// تهيئة الصفحة
function initPage() {
    currentDate = new Date();
    selectedDate = new Date(currentDate);
    
    // محاولة تحميل إعدادات الفصل الدراسي
    const savedSemester = localStorage.getItem('teacherTracker_semesterSettings');
    if (savedSemester) {
        semesterSettings = JSON.parse(savedSemester);
        document.getElementById('semesterSelect').value = semesterSettings.semester;
        document.getElementById('academicYear').value = semesterSettings.academicYear;
        updateSemesterInfo();
    }
    
    // محاولة تحميل الأسابيع المحددة
    const savedWeeks = localStorage.getItem('teacherTracker_selectedWeeks');
    if (savedWeeks) {
        selectedWeeks = JSON.parse(savedWeeks);
        updateSelectedWeeksDisplay();
    }
    
    // محاولة تحميل بيانات التحضير المحفوظة
    loadPeriodAttendanceData();
    
    // حساب التاريخ الهجري
    calculateHijriFromGregorian();
    
    createClassTabs();
    createTables();
    createWeekButtons();
    updateStudentCount();
    updateDateDisplay();
    
    alert("✨ النظام جاهز للاستخدام!\n\nلتفعيل خصائص التحضير العشوائي للأسابيع:\n1. أدخل كلمة المرور: 1406\n2. اختر الأسابيع المطلوبة\n3. اضغط على زر التحضير العشوائي");
}

// إنشاء أزرار الأسابيع
function createWeekButtons() {
    const weekButtonsContainer = document.getElementById('weekButtons');
    weekButtonsContainer.innerHTML = '';
    
    for (let week = 1; week <= 19; week++) {
        const weekData = studyWeeks[week];
        const button = document.createElement('button');
        button.className = 'week-button';
        
        // إضافة محتوى الزر
        button.innerHTML = `
            <span class="week-number">${week}</span>
            <span class="week-dates">${getWeekDateRange(week)}</span>
        `;
        
        button.title = weekData.name + getWeekDaysInfo(week);
        
        if (weekData.missing) {
            button.classList.add('missing');
            button.disabled = true;
            button.innerHTML += '<br><span style="color:#999; font-size:10px;">محذوف</span>';
        } else {
            button.onclick = function() { toggleWeekSelection(week); };
        }
        
        weekButtonsContainer.appendChild(button);
    }
    
    updateWeekButtons();
}

// الحصول على نطاق تواريخ الأسبوع
function getWeekDateRange(week) {
    const weekData = studyWeeks[week];
    if (!weekData.days || weekData.days.length === 0) return "";
    
    const firstDay = weekData.days[0];
    const lastDay = weekData.days[weekData.days.length - 1];
    
    const firstDate = firstDay.gregorian.split('/')[1] + '/' + firstDay.gregorian.split('/')[2].slice(-2);
    const lastDate = lastDay.gregorian.split('/')[1] + '/' + lastDay.gregorian.split('/')[2].slice(-2);
    
    return `${firstDate}-${lastDate}`;
}

// الحصول على معلومات أيام الأسبوع
function getWeekDaysInfo(week) {
    const weekData = studyWeeks[week];
    if (!weekData.days || weekData.days.length === 0) return "";
    
    return ` (${weekData.days.length} أيام)`;
}

// تبديل اختيار الأسبوع
function toggleWeekSelection(week) {
    const index = selectedWeeks.indexOf(week);
    if (index === -1) {
        selectedWeeks.push(week);
    } else {
        selectedWeeks.splice(index, 1);
    }
    
    // ترتيب الأسابيع تصاعدياً
    selectedWeeks.sort((a, b) => a - b);
    
    updateWeekButtons();
    updateSelectedWeeksDisplay();
    saveSelectedWeeks();
}

// تحديث مظهر أزرار الأسابيع
function updateWeekButtons() {
    document.querySelectorAll('.week-button').forEach(button => {
        const weekNum = parseInt(button.querySelector('.week-number').textContent);
        if (selectedWeeks.includes(weekNum)) {
            button.classList.add('selected');
        } else {
            button.classList.remove('selected');
        }
    });
}

// تحديث عرض الأسابيع المحددة
function updateSelectedWeeksDisplay() {
    const displayElement = document.getElementById('selectedWeeksText');
    const countElement = document.getElementById('selectedWeeksCount');
    
    if (selectedWeeks.length === 0) {
        displayElement.textContent = "لا توجد أسابيع محددة";
        countElement.textContent = "0 أسبوع | 0 يوم";
    } else {
        const weekNames = selectedWeeks.map(w => studyWeeks[w].name).join('، ');
        
        // حساب عدد الأيام
        let totalDays = 0;
        selectedWeeks.forEach(weekNum => {
            totalDays += studyWeeks[weekNum].days.length;
        });
        
        displayElement.textContent = `${weekNames}`;
        countElement.textContent = `${selectedWeeks.length} أسبوع | ${totalDays} يوم`;
    }
}

// حفظ الأسابيع المحددة
function saveSelectedWeeks() {
    localStorage.setItem('teacherTracker_selectedWeeks', JSON.stringify(selectedWeeks));
}

// تحديد جميع الأسابيع
function selectAllWeeks() {
    selectedWeeks = [];
    for (let week = 1; week <= 19; week++) {
        if (!studyWeeks[week].missing) {
            selectedWeeks.push(week);
        }
    }
    updateWeekButtons();
    updateSelectedWeeksDisplay();
    saveSelectedWeeks();
    alert(`تم تحديد جميع الأسابيع (${selectedWeeks.length} أسبوع)`);
}

// مسح جميع الأسابيع
function clearSelectedWeeks() {
    selectedWeeks = [];
    updateWeekButtons();
    updateSelectedWeeksDisplay();
    saveSelectedWeeks();
    alert("تم مسح جميع الأسابيع المحددة");
}

// تحديد الفصل الأول
function selectFirstSemester() {
    selectedWeeks = [];
    for (let week = 1; week <= 8; week++) {
        if (!studyWeeks[week].missing) {
            selectedWeeks.push(week);
        }
    }
    updateWeekButtons();
    updateSelectedWeeksDisplay();
    saveSelectedWeeks();
    alert("تم تحديد أسابيع الفصل الأول (1-8)");
}

// تحديد الفصل الثاني
function selectSecondSemester() {
    selectedWeeks = [];
    for (let week = 9; week <= 19; week++) {
        if (!studyWeeks[week].missing) {
            selectedWeeks.push(week);
        }
    }
    updateWeekButtons();
    updateSelectedWeeksDisplay();
    saveSelectedWeeks();
    alert("تم تحديد أسابيع الفصل الثاني (9-19)");
}

// تحديد دفعة من الأسابيع
function selectWeeksBatch(start, end) {
    for (let week = start; week <= end; week++) {
        if (!studyWeeks[week].missing && !selectedWeeks.includes(week)) {
            selectedWeeks.push(week);
        }
    }
    selectedWeeks.sort((a, b) => a - b);
    updateWeekButtons();
    updateSelectedWeeksDisplay();
    saveSelectedWeeks();
    alert(`تم إضافة الأسابيع ${start}-${end} إلى المحددة`);
}

// تحضير عشوائي للأسابيع المحددة
function randomAttendanceForSelectedWeeks() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا\nكلمة المرور: 1406');
        return;
    }
    
    if (selectedWeeks.length === 0) {
        alert("⚠️ لم تحدد أي أسابيع!\n\nالرجاء اختيار أسبوع أو أكثر من القائمة بالأعلى");
        return;
    }
    
    // حساب عدد الأيام
    let totalDays = 0;
    let dateRange = "";
    
    selectedWeeks.forEach(weekNum => {
        totalDays += studyWeeks[weekNum].days.length;
    });
    
    const firstWeek = studyWeeks[selectedWeeks[0]];
    const lastWeek = studyWeeks[selectedWeeks[selectedWeeks.length - 1]];
    
    if (firstWeek.days.length > 0 && lastWeek.days.length > 0) {
        const firstDate = firstWeek.days[0].gregorian;
        const lastDate = lastWeek.days[lastWeek.days.length - 1].gregorian;
        dateRange = `من ${firstDate} إلى ${lastDate}`;
    }
    
    const confirmMessage = `🎲 تحضير عشوائي للأسابيع المحددة\n\n` +
                          `✅ الأسابيع: ${selectedWeeks.map(w => studyWeeks[w].name).join(', ')}\n` +
                          `📅 عدد الأيام: ${totalDays} يوم\n` +
                          `📊 النطاق الزمني: ${dateRange}\n\n` +
                          `⭐ سيتم وضع ✓ لكل الخيارات للطلاب المتميزين (الذين لديهم نجمة ⭐)\n\n` +
                          `هل تريد المتابعة؟`;
    
    const confirmAction = confirm(confirmMessage);
    if (!confirmAction) return;
    
    let totalStudentsProcessed = 0;
    let totalStarredStudents = 0;
    let totalDaysProcessed = 0;
    
    // معالجة كل أسبوع
    selectedWeeks.forEach(weekNum => {
        const week = studyWeeks[weekNum];
        
        week.days.forEach(dayInfo => {
            // إنشاء تاريخ من السلسلة
            const dateParts = dayInfo.gregorian.split('/');
            const date = new Date(dateParts[0], dateParts[1] - 1, dateParts[2]);
            
            // إنشاء تحضير عشوائي لهذا اليوم
            const attendanceData = generateRandomAttendanceForDate(date, dayInfo.hijri);
            const dateKey = date.toISOString().split('T')[0];
            
            // حفظ بيانات اليوم
            periodAttendanceData[dateKey] = attendanceData;
            
            // حساب الإحصائيات لهذا اليوم
            let dayStudents = 0;
            let dayStarred = 0;
            
            for (const className in attendanceData.classes) {
                dayStudents += attendanceData.classes[className].stats.total;
                dayStarred += attendanceData.classes[className].stats.starred;
            }
            
            // تحديث المجاميع
            totalStudentsProcessed += dayStudents;
            totalStarredStudents += dayStarred;
            totalDaysProcessed++;
        });
    });
    
    // حفظ بيانات الفترة
    savePeriodAttendanceData();
    
    // حساب الإحصائيات
    const avgStudentsPerDay = totalStudentsProcessed / totalDaysProcessed;
    const avgStarredPerDay = totalStarredStudents / totalDaysProcessed;
    const avgRegularPerDay = (totalStudentsProcessed - totalStarredStudents) / totalDaysProcessed;
    
    // عرض تقرير النتائج
    const resultMessage = `✅ تم إنشاء التحضير العشوائي بنجاح!\n\n` +
                         `📊 الإحصائيات النهائية:\n` +
                         `   - عدد الأسابيع: ${selectedWeeks.length}\n` +
                         `   - عدد الأيام: ${totalDaysProcessed}\n` +
                         `   - إجمالي الطلاب المعالجين: ${totalStudentsProcessed}\n` +
                         `   - متوسط الطلاب في اليوم: ${avgStudentsPerDay.toFixed(1)}\n` +
                         `   - متوسط الطلاب المتميزين في اليوم: ${avgStarredPerDay.toFixed(1)}\n` +
                         `   - متوسط الطلاب العاديين في اليوم: ${avgRegularPerDay.toFixed(1)}\n\n` +
                         `💾 تم حفظ بيانات التحضير في النظام.\n` +
                         `📥 يمكنك الآن تصدير التقرير باستخدام زر "تصدير الأسابيع المحددة"`;
    
    alert(resultMessage);
}

// توليد تحضير عشوائي ليوم معين
function generateRandomAttendanceForDate(date, hijriDateStr) {
    const dateKey = date.toISOString().split('T')[0];
    
    // تحويل التاريخ الميلادي إلى تنسيق عربي
    const day = date.getDate();
    const month = gregorianMonths[date.getMonth()];
    const year = date.getFullYear();
    const gregorianDate = `${convertToArabicNumbers(day)} ${month} ${convertToArabicNumbers(year)}`;
    
    const attendanceData = {
        date: dateKey,
        gregorianDate: gregorianDate,
        hijriDate: hijriDateStr,
        classes: {}
    };
    
    // توليد بيانات لكل صف
    for (const className in studentsData) {
        attendanceData.classes[className] = {
            students: [],
            stats: {
                total: 0,
                present: 0,
                absent: 0,
                starred: 0
            }
        };
        
        studentsData[className].forEach((studentName, index) => {
            // تحديد عشوائياً إذا كان الطالب متميزاً (30% احتمال)
            const isStarred = Math.random() < 0.3;
            
            // إنشاء بيانات الطالب
            const studentData = {
                id: index + 1,
                name: studentName,
                isStarred: isStarred,
                attendance: [],
                hasStar: isStarred
            };
            
            // توليد بيانات الحضور (5 عناصر)
            for (let i = 0; i < 5; i++) {
                if (isStarred) {
                    // الطلاب المتميزون يحصلون على ✓ في كل الخيارات
                    studentData.attendance.push({
                        type: ['الحضور', 'الواجبات', 'المشروعات', 'تطبيقات وأنشطة', 'مشاركة'][i],
                        value: '✔',
                        isPresent: true
                    });
                    attendanceData.classes[className].stats.present++;
                } else {
                    // الطلاب العاديون يحصلون على تقييم عشوائي
                    const isPresent = Math.random() > 0.25; // 75% حضور
                    studentData.attendance.push({
                        type: ['الحضور', 'الواجبات', 'المشروعات', 'تطبيقات وأنشطة', 'مشاركة'][i],
                        value: isPresent ? '✔' : '✖',
                        isPresent: isPresent
                    });
                    
                    if (isPresent) {
                        attendanceData.classes[className].stats.present++;
                    } else {
                        attendanceData.classes[className].stats.absent++;
                    }
                }
            }
            
            attendanceData.classes[className].students.push(studentData);
            attendanceData.classes[className].stats.total++;
            
            if (isStarred) {
                attendanceData.classes[className].stats.starred++;
            }
        });
    }
    
    return attendanceData;
}

// تحميل بيانات التحضير المحفوظة للفترة
function loadPeriodAttendanceData() {
    const savedData = localStorage.getItem('teacherTracker_periodAttendanceData');
    if (savedData) {
        periodAttendanceData = JSON.parse(savedData);
    }
}

// حفظ بيانات التحضير للفترة
function savePeriodAttendanceData() {
    localStorage.setItem('teacherTracker_periodAttendanceData', JSON.stringify(periodAttendanceData));
}

// تصدير الأسابيع المحددة إلى Excel
function exportSelectedWeeks() {
    if (selectedWeeks.length === 0) {
        alert("⚠️ لم تحدد أي أسابيع!\n\nالرجاء اختيار أسبوع أو أكثر أولاً");
        return;
    }
    
    let tablesHTML = `<h2>تقرير التحضير للأسابيع المحددة</h2>`;
    tablesHTML += `<h3>المعلم: فهد الخالدي - المادة: اللغة الإنجليزية</h3>`;
    tablesHTML += `<h3>${document.getElementById('currentSemesterInfo').textContent}</h3>`;
    tablesHTML += `<h3>المدرسة: سعيد بن العاص المتوسطة</h3>`;
    
    const weekNames = selectedWeeks.map(w => studyWeeks[w].name).join('، ');
    tablesHTML += `<h3>الأسابيع: ${weekNames} (${selectedWeeks.length} أسابيع)</h3>`;
    
    let totalDays = 0;
    let totalStudents = 0;
    let totalPresent = 0;
    let totalAbsent = 0;
    let totalStarred = 0;
    
    // إضافة بيانات كل أسبوع
    selectedWeeks.forEach(weekNum => {
        const week = studyWeeks[weekNum];
        
        tablesHTML += `<h3 style="background:#e8f5e9; padding:10px; margin-top:20px;">${week.name}</h3>`;
        
        // إضافة بيانات كل يوم في الأسبوع
        week.days.forEach(dayInfo => {
            const dateParts = dayInfo.gregorian.split('/');
            const date = new Date(dateParts[0], dateParts[1] - 1, dateParts[2]);
            const dateKey = date.toISOString().split('T')[0];
            
            tablesHTML += `<h4>${dayInfo.day}: ${dayInfo.gregorian} (${dayInfo.hijri})</h4>`;
            
            if (periodAttendanceData[dateKey]) {
                const dayData = periodAttendanceData[dateKey];
                
                // إضافة جداول لكل صف في هذا اليوم
                for (const className in dayData.classes) {
                    const classData = dayData.classes[className];
                    
                    tablesHTML += `<h5>الصف ${className} (${classData.stats.total} طالب)</h5>`;
                    tablesHTML += `<table border="1" cellpadding="5" cellspacing="0" style="width:100%; border-collapse:collapse; margin-bottom:15px;">`;
                    tablesHTML += `<thead><tr>
                        <th width="5%">م</th>
                        <th>الاسم</th>
                        <th width="8%">الحضور</th>
                        <th width="8%">الواجبات</th>
                        <th width="8%">المشروعات</th>
                        <th width="8%">تطبيقات وأنشطة</th>
                        <th width="8%">مشاركة</th>
                        <th width="8%">⭐</th>
                    </tr></thead><tbody>`;
                    
                    classData.students.forEach(student => {
                        tablesHTML += `<tr>`;
                        tablesHTML += `<td>${student.id}</td>`;
                        tablesHTML += `<td>${student.name}</td>`;
                        
                        student.attendance.forEach(item => {
                            tablesHTML += `<td style="${item.value === '✔' ? 'background-color:#e8f5e9;' : 'background-color:#ffebee;'}">${item.value}</td>`;
                        });
                        
                        tablesHTML += `<td>${student.hasStar ? '⭐' : ''}</td>`;
                        tablesHTML += `</tr>`;
                    });
                    
                    tablesHTML += `</tbody></table>`;
                    
                    // تحديث الإحصائيات
                    totalDays++;
                    totalStudents += classData.stats.total;
                    totalPresent += classData.stats.present;
                    totalAbsent += classData.stats.absent;
                    totalStarred += classData.stats.starred;
                }
            } else {
                tablesHTML += `<p style="color:#999; text-align:center;">لا توجد بيانات تحضير لهذا اليوم</p>`;
            }
        });
    });
    
    // إضافة ملخص شامل
    tablesHTML += `<h3 style="background:#e0f7fa; padding:10px; margin-top:20px;">ملخص شامل للأسابيع المحددة</h3>`;
    tablesHTML += `<div style="padding:15px; background:#fff8e1; border-radius:5px; margin-bottom:20px;">
        <strong>إجمالي الأسابيع المحددة:</strong><br>
        - عدد الأسابيع: ${selectedWeeks.length} أسبوع<br>
        - عدد الأيام: ${totalDays} يوم<br>
        - إجمالي الطلاب: ${totalStudents} طالب<br>
        - إجمالي الحضور: ${totalPresent} حالة حضور<br>
        - إجمالي الغياب: ${totalAbsent} حالة غياب<br>
        - إجمالي المتميزين: ${totalStarred} طالب<br>
        - متوسط الحضور: ${totalDays > 0 ? ((totalPresent / (totalPresent + totalAbsent)) * 100).toFixed(1) : 0}%
    </div>`;
    
    // إنشاء ملف Excel
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
                   <x:Name>تقرير الأسابيع</x:Name>
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
    const weekRange = `الأسابيع_${selectedWeeks[0]}_إلى_${selectedWeeks[selectedWeeks.length - 1]}`;
    link.download = `تقرير_${weekRange}.xls`;
    link.click();
    
    alert(`✅ تم تصدير التقرير بنجاح!\n\n📊 يحتوي على:\n- ${selectedWeeks.length} أسبوع\n- ${totalDays} يوم\n- ${totalStudents} حالة حضور`);
}

// تصدير جميع الأسابيع
function exportAllWeeks() {
    // تحديد جميع الأسابيع (باستثناء الأسبوع 14 المحذوف)
    const allWeeks = [];
    for (let i = 1; i <= 19; i++) {
        if (i !== 14) {
            allWeeks.push(i);
        }
    }
    
    // حفظ الأسابيع الحالية مؤقتاً
    const tempWeeks = [...selectedWeeks];
    selectedWeeks = allWeeks;
    updateSelectedWeeksDisplay();
    
    exportSelectedWeeks();
    
    // استعادة الأسابيع الأصلية
    selectedWeeks = tempWeeks;
    updateSelectedWeeksDisplay();
}

// إنشاء تقرير إحصائي
function generateWeeklyReport() {
    if (selectedWeeks.length === 0) {
        alert("⚠️ لم تحدد أي أسابيع!\n\nالرجاء اختيار أسبوع أو أكثر أولاً");
        return;
    }
    
    let reportHTML = `<h2>التقرير الإحصائي للأسابيع المحددة</h2>`;
    reportHTML += `<h3>المعلم: فهد الخالدي - المادة: اللغة الإنجليزية</h3>`;
    reportHTML += `<h3>${document.getElementById('currentSemesterInfo').textContent}</h3>`;
    
    const weekNames = selectedWeeks.map(w => studyWeeks[w].name).join('، ');
    reportHTML += `<h3>الأسابيع: ${weekNames}</h3>`;
    reportHTML += `<h3>تاريخ التقرير: ${getShortGregorianDate(new Date())}</h3>`;
    
    // إحصائيات كل أسبوع
    selectedWeeks.forEach(weekNum => {
        const week = studyWeeks[weekNum];
        let weekStudents = 0;
        let weekPresent = 0;
        let weekAbsent = 0;
        let weekStarred = 0;
        let weekDaysCount = 0;
        
        week.days.forEach(dayInfo => {
            const dateParts = dayInfo.gregorian.split('/');
            const date = new Date(dateParts[0], dateParts[1] - 1, dateParts[2]);
            const dateKey = date.toISOString().split('T')[0];
            
            if (periodAttendanceData[dateKey]) {
                const dayData = periodAttendanceData[dateKey];
                
                for (const className in dayData.classes) {
                    const classData = dayData.classes[className];
                    weekStudents += classData.stats.total;
                    weekPresent += classData.stats.present;
                    weekAbsent += classData.stats.absent;
                    weekStarred += classData.stats.starred;
                }
                
                weekDaysCount++;
            }
        });
        
        const attendanceRate = weekPresent + weekAbsent > 0 ? 
            ((weekPresent / (weekPresent + weekAbsent)) * 100).toFixed(1) : 0;
        
        reportHTML += `<div style="background:#f5f5f5; padding:10px; margin:10px 0; border-radius:5px; border-left:5px solid #4CAF50;">
            <h4>${week.name} (${weekDaysCount} يوم)</h4>
            <p>إجمالي الطلاب: ${weekStudents}</p>
            <p>الحضور: ${weekPresent} | الغياب: ${weekAbsent}</p>
            <p>المتميزون: ${weekStarred}</p>
            <p>نسبة الحضور: ${attendanceRate}%</p>
        </div>`;
    });
    
    // إحصائيات عامة
    let totalDays = 0;
    let totalStudents = 0;
    let totalPresent = 0;
    let totalAbsent = 0;
    let totalStarred = 0;
    
    selectedWeeks.forEach(weekNum => {
        const week = studyWeeks[weekNum];
        totalDays += week.days.length;
        
        week.days.forEach(dayInfo => {
            const dateParts = dayInfo.gregorian.split('/');
            const date = new Date(dateParts[0], dateParts[1] - 1, dateParts[2]);
            const dateKey = date.toISOString().split('T')[0];
            
            if (periodAttendanceData[dateKey]) {
                const dayData = periodAttendanceData[dateKey];
                
                for (const className in dayData.classes) {
                    const classData = dayData.classes[className];
                    totalStudents += classData.stats.total;
                    totalPresent += classData.stats.present;
                    totalAbsent += classData.stats.absent;
                    totalStarred += classData.stats.starred;
                }
            }
        });
    });
    
    const overallRate = totalPresent + totalAbsent > 0 ? 
        ((totalPresent / (totalPresent + totalAbsent)) * 100).toFixed(1) : 0;
    
    reportHTML += `<div style="background:#e3f2fd; padding:15px; margin:20px 0; border-radius:5px; border:2px solid #2196F3;">
        <h3>📊 الإحصائيات العامة</h3>
        <p><strong>عدد الأسابيع:</strong> ${selectedWeeks.length}</p>
        <p><strong>عدد الأيام:</strong> ${totalDays}</p>
        <p><strong>إجمالي الطلاب:</strong> ${totalStudents}</p>
        <p><strong>إجمالي الحضور:</strong> ${totalPresent}</p>
        <p><strong>إجمالي الغياب:</strong> ${totalAbsent}</p>
        <p><strong>إجمالي المتميزين:</strong> ${totalStarred}</p>
        <p><strong>نسبة الحضور العامة:</strong> ${overallRate}%</p>
    </div>`;
    
    // عرض التقرير في نافذة جديدة
    const reportWindow = window.open('', '_blank');
    reportWindow.document.write(`
        <html dir="rtl">
        <head>
            <title>التقرير الإحصائي</title>
            <style>
                body { font-family: Arial, sans-serif; padding: 20px; background: #f5f5f5; }
                h2 { color: #1a5276; text-align: center; }
                h3 { color: #2a9d8f; }
                h4 { color: #4CAF50; }
                .report-container { max-width: 800px; margin: 0 auto; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
                .controls { text-align: center; margin-top: 20px; }
                button { padding: 10px 20px; margin: 5px; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; }
                .print-btn { background: #4CAF50; color: white; }
                .close-btn { background: #f44336; color: white; }
            </style>
        </head>
        <body>
            <div class="report-container">
                ${reportHTML}
                <div class="controls">
                    <button class="print-btn" onclick="window.print()">🖨️ طباعة التقرير</button>
                    <button class="close-btn" onclick="window.close()">✖ إغلاق</button>
                </div>
            </div>
        </body>
        </html>
    `);
    reportWindow.document.close();
}

// ======== باقي الوظائف الأساسية ========

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
    alert(`✅ تم حفظ إعدادات الفصل الدراسي`);
}

// حساب التاريخ الهجري من التاريخ الميلادي
function calculateHijriFromGregorian() {
    try {
        if (typeof HijriDate !== 'undefined') {
            const hijri = new HijriDate(selectedDate);
            hijriDate.day = hijri.date;
            hijriDate.month = hijri.month;
            hijriDate.year = hijri.year;
            hijriDate.monthName = hijriMonths[hijri.month - 1];
        }
    } catch (error) {
        hijriDate = { day: 1, month: 1, year: 1446, monthName: "محرم" };
    }
}

// تحديث عرض التاريخ
function updateDateDisplay() {
    const gregorianDateString = getShortGregorianDate(selectedDate);
    document.getElementById('gregorianDateText').innerHTML = gregorianDateString;
    
    const hijriDateString = `${convertToArabicNumbers(hijriDate.day)} ${hijriDate.monthName} ${convertToArabicNumbers(hijriDate.year)}هـ`;
    document.getElementById('hijriDateText').innerHTML = hijriDateString;
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
        
        fillClassTable(className);
    }
    
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

// عرض صف معين أو جميع الصفوف
function showClass(className) {
    currentClass = className;
    
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
}

// تبديل النجمة
function toggleStar(cell) {
    if (adminActive) {
        cell.innerHTML = cell.innerHTML === "☆" ? "⭐" : "☆";
        const row = cell.closest('tr');
        if (cell.innerHTML === "⭐") {
            row.classList.add('starred-student');
        } else {
            row.classList.remove('starred-student');
        }
    } else {
        alert('يجب تفعيل وضع الإدارة أولا\nكلمة المرور: 1406');
    }
}

// التحقق من كلمة المرور
function checkAdmin() {
    const pass = document.getElementById("adminPass").value;
    if (pass === "1406") {
        adminActive = !adminActive;
        document.getElementById("adminPanel").style.display = adminActive ? "block" : "none";
        document.getElementById("adminPass").value = "";
        
        if (adminActive) {
            alert("✅ تم تفعيل خصائص الإدارة بنجاح!\n\nيمكنك الآن:\n1. اختيار الأسابيع المطلوبة\n2. التحضير العشوائي\n3. تمييز الطلاب المتميزين");
        } else {
            alert("تم إغلاق لوحة الإدارة");
        }
    } else {
        alert("❌ كلمة مرور خاطئة!\nكلمة المرور الصحيحة: 1406");
    }
}

// تحضير عشوائي للتاريخ الحالي
function randomAttendance() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا\nكلمة المرور: 1406');
        return;
    }
    
    const confirmAction = confirm("هل تريد تعيين الحضور عشوائيا لجميع الطلاب للتاريخ الحالي؟");
    if (!confirmAction) return;
    
    document.querySelectorAll('.class-section').forEach(section => {
        const rows = section.querySelectorAll('tbody tr');
        rows.forEach(row => {
            const starCell = row.querySelector('.star-cell');
            const hasStar = starCell && starCell.innerHTML === "⭐";
            const attendanceCells = row.querySelectorAll('td[onclick="toggle(this)"]');
            
            attendanceCells.forEach(cell => {
                if (hasStar) {
                    cell.innerHTML = "✔";
                    cell.classList.remove('absent');
                    cell.classList.add('present');
                } else {
                    cell.innerHTML = Math.random() > 0.3 ? "✔" : "✖";
                    if (cell.innerHTML === "✔") {
                        cell.classList.remove('absent');
                        cell.classList.add('present');
                    } else {
                        cell.classList.remove('present');
                        cell.classList.add('absent');
                    }
                }
            });
        });
    });
    
    alert("تم تعيين الحضور عشوائيا للتاريخ الحالي!");
}

// إضافة طالب جديد
function addStudent() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا\nكلمة المرور: 1406');
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
        fillClassTable(className);
        updateStudentCount();
        document.querySelector(`#class-${className} .class-header`).textContent = 
            `الصف ${className} - ${studentsData[className].length} طالب`;
        alert("تمت إضافة الطالب بنجاح");
    }
}

// إعادة تعيين الكل
function resetAll() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا\nكلمة المرور: 1406');
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
        const row = cell.closest('tr');
        row.classList.remove('starred-student');
    });
    
    alert("تمت إعادة التعيين بنجاح");
}

// تصدير إلى Excel
function exportToExcel() {
    const gregorianDateForExcel = getShortGregorianDate(selectedDate);
    const hijriDateForExcel = `${convertToArabicNumbers(hijriDate.day)} ${hijriDate.monthName} ${convertToArabicNumbers(hijriDate.year)}هـ`;
    
    let tablesHTML = `<h2>سجل متابعة الطلاب - المعلم: فهد الخالدي</h2>`;
    tablesHTML += `<h3>المادة: اللغة الإنجليزية - ${document.getElementById('currentSemesterInfo').textContent}</h3>`;
    tablesHTML += `<h3>المدرسة: سعيد بن العاص المتوسطة</h3>`;
    tablesHTML += `<h3>التاريخ الميلادي: ${gregorianDateForExcel}</h3>`;
    tablesHTML += `<h3>التاريخ الهجري: ${hijriDateForExcel}</h3>`;
    
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
    const dateStr = selectedDate.toISOString().split('T')[0];
    link.download = `تقرير_حضور_${dateStr}.xls`;
    link.click();
}

// طباعة الصفحة
function printPage() {
    window.print();
}

// تصفية حسب الحالة
function filterByStatus(status) {
    currentFilter = status;
    
    document.querySelectorAll('.status-filter button').forEach(btn => {
        btn.classList.remove('active');
    });
    event.target.classList.add('active');
    
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

// عرض تحضير اليوم
function showTodayAttendance() {
    selectedDate = new Date();
    calculateHijriFromGregorian();
    updateDateDisplay();
    alert("✅ تم العرض بتاريخ اليوم الحقيقي");
}

// تهيئة الصفحة عند التحميل
window.onload = initPage;
</script>
</body>
</html>
