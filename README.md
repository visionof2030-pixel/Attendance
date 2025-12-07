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
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 8px;
    margin: 15px 0;
}

.week-button {
    padding: 8px 12px;
    background: #e0e0e0;
    border: 2px solid #ddd;
    border-radius: 5px;
    cursor: pointer;
    font-weight: bold;
    transition: all 0.3s;
    min-width: 60px;
}

.week-button:hover {
    background: #d0d0d0;
}

.week-button.selected {
    background: #4CAF50;
    color: white;
    border-color: #388E3C;
}

.week-button.missing {
    background: #ffcccc;
    color: #666;
    cursor: not-allowed;
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
    font-size: 12px;
    color: #666;
    margin-top: 5px;
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
            </div>
            
            <div class="week-buttons" id="weekButtons">
                <!-- سيتم إنشاء أزرار الأسابيع ديناميكياً -->
            </div>
            
            <div style="text-align: center; margin-top: 15px;">
                <button onclick="clearSelectedWeeks()">🗑️ مسح الأسابيع المحددة</button>
                <button onclick="randomAttendanceForSelectedWeeks()">🎲 تحضير عشوائي للأسابيع المحددة</button>
            </div>
            
            <div style="text-align:center; margin-top:10px; font-size:12px; color:#666;">
                ⭐ خاصية التحضير العشوائي: سيتم وضع ✓ لكل الخيارات للطلاب المتميزين (الذين لديهم نجمة ⭐)
            </div>
        </div>
        
        <div class="export-section">
            <h4>📤 تصدير التقارير</h4>
            <div style="text-align: center; margin-top: 10px;">
                <button onclick="exportSelectedWeeks()" style="background: #4CAF50;">📥 تصدير الأسابيع المحددة Excel</button>
                <button onclick="exportAllWeeks()" style="background: #2196F3;">📚 تصدير جميع الأسابيع Excel</button>
                <button onclick="exportByMonth()" style="background: #9C27B0;">📅 تصدير حسب الشهر Excel</button>
            </div>
            <div style="text-align: center; margin-top: 10px;">
                <button onclick="generateWeeklyReport()" style="background: #FF9800;">📈 إنشاء تقرير إحصائي</button>
            </div>
        </div>
        
        <div class="admin-section">
            <h4>🕐 التحكم في التاريخ (للتعديل فقط)</h4>
            <div style="text-align:center; background:#ffebee; padding:10px; border-radius:5px; margin-bottom:10px;">
                <strong>ملاحظة:</strong> يتم عرض تاريخ اليوم الحقيقي تلقائياً. هذه الأدوات تستخدم فقط لتعديل التاريخ عند الحاجة.
            </div>
            <div class="date-controls">
                <button onclick="changeMonth(-1)">◀ الشهر السابق</button>
                <div class="date-display" id="adminDateDisplay">...</div>
                <button onclick="changeMonth(1)">الشهر القادم ▶</button>
            </div>
            <div style="text-align: center; margin: 10px 0;">
                <input type="date" id="datePicker" class="date-input" onchange="setCustomDate()">
                <button onclick="resetToToday()">🔄 الرجوع لليوم الحقيقي</button>
                <button onclick="saveCurrentDate()">💾 حفظ التعديلات</button>
            </div>
            
            <div class="hijri-date-selector">
                <h5 style="text-align:center; color: #d84315;">التاريخ الهجري (يمكن تعديله يدوياً)</h5>
                <div class="admin-row">
                    <div class="admin-label">اليوم:</div>
                    <div class="admin-input">
                        <input type="number" id="hijriDay" min="1" max="30" style="width: 70px;">
                    </div>
                </div>
                <div class="admin-row">
                    <div class="admin-label">الشهر:</div>
                    <div class="admin-input">
                        <select id="hijriMonth" style="width: 100%;">
                            <option value="1">محرم</option>
                            <option value="2">صفر</option>
                            <option value="3">ربيع الأول</option>
                            <option value="4">ربيع الثاني</option>
                            <option value="5">جمادى الأولى</option>
                            <option value="6">جمادى الآخرة</option>
                            <option value="7">رجب</option>
                            <option value="8">شعبان</option>
                            <option value="9">رمضان</option>
                            <option value="10">شوال</option>
                            <option value="11">ذو القعدة</option>
                            <option value="12">ذو الحجة</option>
                        </select>
                    </div>
                </div>
                <div class="admin-row">
                    <div class="admin-label">السنة:</div>
                    <div class="admin-input">
                        <input type="number" id="hijriYear" min="1300" max="1500" style="width: 100px;">
                    </div>
                </div>
                <div style="text-align: center; margin-top: 10px;">
                    <button onclick="updateHijriDate()">🔄 تحديث التاريخ الهجري</button>
                    <button onclick="resetHijriToToday()">🔄 الرجوع للتاريخ الفعلي</button>
                </div>
            </div>
        </div>
        
        <div class="admin-section">
            <h4>👨‍🏫 إدارة الطلاب</h4>
            <div style="text-align:center;">
                <button onclick="addStudent()">➕ إضافة طالب</button>
                <button onclick="moveStudent()">↔️ نقل طالب</button>
                <button onclick="resetAll()">🔄 إعادة تعيين</button>
            </div>
        </div>
        
        <p style="text-align:center; font-size:12px; color:#666;">بعد تفعيل الإدارة، يمكن تمييز الطلاب بالنجمة وإدارة جميع الخصائص.</p>
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

// تهيئة الصفحة
function initPage() {
    // دائماً نبدأ بتاريخ اليوم الحقيقي
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
    
    // حساب التاريخ الهجري الفعلي من التاريخ الميلادي
    calculateHijriFromGregorian();
    
    // محاولة تحميل بيانات الحضور المحفوظة لهذا التاريخ
    loadAttendanceData();
    
    createClassTabs();
    createTables();
    createWeekButtons();
    updateStudentCount();
    updateDateDisplay();
    
    // تعيين التاريخ الحالي في منتقي التاريخ
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('datePicker').value = today;
    
    // تحديث حقول التاريخ الهجري
    updateHijriFields();
}

// إنشاء أزرار الأسابيع
function createWeekButtons() {
    const weekButtonsContainer = document.getElementById('weekButtons');
    weekButtonsContainer.innerHTML = '';
    
    for (let week = 1; week <= 19; week++) {
        const weekData = studyWeeks[week];
        const button = document.createElement('button');
        button.className = 'week-button';
        button.textContent = week;
        button.title = weekData.name;
        
        if (weekData.missing) {
            button.classList.add('missing');
            button.disabled = true;
        } else {
            button.onclick = function() { toggleWeekSelection(week); };
        }
        
        // إضافة معلومات عن الأسبوع
        const infoDiv = document.createElement('div');
        infoDiv.className = 'week-info';
        if (weekData.days && weekData.days.length > 0) {
            const firstDay = weekData.days[0];
            const lastDay = weekData.days[weekData.days.length - 1];
            infoDiv.textContent = `${firstDay.gregorian.split('/')[1]}/${firstDay.gregorian.split('/')[2]} - ${lastDay.gregorian.split('/')[1]}/${lastDay.gregorian.split('/')[2]}`;
        }
        
        const container = document.createElement('div');
        container.style.textAlign = 'center';
        container.appendChild(button);
        container.appendChild(infoDiv);
        weekButtonsContainer.appendChild(container);
    }
    
    updateWeekButtons();
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
        const weekNum = parseInt(button.textContent);
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
    if (selectedWeeks.length === 0) {
        displayElement.textContent = "لا توجد أسابيع محددة";
    } else {
        const weekNames = selectedWeeks.map(w => studyWeeks[w].name).join('، ');
        displayElement.textContent = `${weekNames} (${selectedWeeks.length} أسابيع)`;
    }
}

// حفظ الأسابيع المحددة
function saveSelectedWeeks() {
    localStorage.setItem('teacherTracker_selectedWeeks', JSON.stringify(selectedWeeks));
}

// مسح الأسابيع المحددة
function clearSelectedWeeks() {
    selectedWeeks = [];
    updateWeekButtons();
    updateSelectedWeeksDisplay();
    saveSelectedWeeks();
}

// تحضير عشوائي للأسابيع المحددة
function randomAttendanceForSelectedWeeks() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    if (selectedWeeks.length === 0) {
        alert("لم تحدد أي أسابيع! الرجاء تحديد أسبوع أو أكثر.");
        return;
    }
    
    let totalDays = 0;
    let dateRange = "";
    
    // حساب عدد الأيام والنطاق الزمني
    selectedWeeks.forEach(weekNum => {
        const week = studyWeeks[weekNum];
        totalDays += week.days.length;
    });
    
    const firstWeek = studyWeeks[selectedWeeks[0]];
    const lastWeek = studyWeeks[selectedWeeks[selectedWeeks.length - 1]];
    
    if (firstWeek.days.length > 0 && lastWeek.days.length > 0) {
        const firstDate = firstWeek.days[0].gregorian.replace(/\//g, '-');
        const lastDate = lastWeek.days[lastWeek.days.length - 1].gregorian.replace(/\//g, '-');
        dateRange = `${firstDate} إلى ${lastDate}`;
    }
    
    const confirmMessage = `هل تريد تعيين الحضور عشوائيا للأسابيع المحددة؟\n\n` +
                          `الأسابيع: ${selectedWeeks.map(w => studyWeeks[w].name).join(', ')}\n` +
                          `عدد الأيام: ${totalDays} يوم\n` +
                          `النطاق الزمني: ${dateRange}\n\n` +
                          `ملاحظة: سيتم وضع ✓ لكل الخيارات للطلاب المتميزين (الذين لديهم نجمة ⭐)`;
    
    const confirmAction = confirm(confirmMessage);
    if (!confirmAction) return;
    
    let totalStudentsProcessed = 0;
    let totalStarredStudents = 0;
    let totalRegularStudents = 0;
    
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
            
            const dayRegular = dayStudents - dayStarred;
            
            // تحديث المجاميع
            totalStudentsProcessed += dayStudents;
            totalStarredStudents += dayStarred;
            totalRegularStudents += dayRegular;
        });
    });
    
    // حفظ بيانات الفترة
    savePeriodAttendanceData();
    
    // حساب الإحصائيات
    const avgStudentsPerDay = totalStudentsProcessed / totalDays;
    const avgStarredPerDay = totalStarredStudents / totalDays;
    const avgRegularPerDay = totalRegularStudents / totalDays;
    
    // عرض تقرير النتائج
    const resultMessage = `✅ تم إنشاء التحضير العشوائي للأسابيع المحددة بنجاح!\n\n` +
                         `📅 الأسابيع: ${selectedWeeks.map(w => studyWeeks[w].name).join('، ')}\n` +
                         `📊 الإحصائيات:\n` +
                         `   - عدد الأيام: ${totalDays} يوم\n` +
                         `   - إجمالي الطلاب المعالجين: ${totalStudentsProcessed} طالب\n` +
                         `   - متوسط الطلاب في اليوم: ${avgStudentsPerDay.toFixed(1)} طالب\n` +
                         `   - متوسط الطلاب المتميزين في اليوم: ${avgStarredPerDay.toFixed(1)} طالب\n` +
                         `   - متوسط الطلاب العاديين في اليوم: ${avgRegularPerDay.toFixed(1)} طالب\n\n` +
                         `💾 تم حفظ بيانات التحضير لكل يوم في النظام.`;
    
    alert(resultMessage);
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
            // تحديد عشوائياً إذا كان الطالب متميزاً (20% احتمال)
            const isStarred = Math.random() < 0.2;
            
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
                    const isPresent = Math.random() > 0.3;
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

// تصدير الأسابيع المحددة إلى Excel
function exportSelectedWeeks() {
    if (selectedWeeks.length === 0) {
        alert("لم تحدد أي أسابيع! الرجاء تحديد أسبوع أو أكثر أولاً.");
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
    
    alert(`تم تصدير تقرير الأسابيع المحددة بنجاح!\n\nيتضمن التقرير بيانات ${selectedWeeks.length} أسبوع (${totalDays} يوم)`);
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

// تصدير حسب الشهر
function exportByMonth() {
    alert("ميزة التصدير حسب الشهر قيد التطوير");
}

// إنشاء تقرير إحصائي
function generateWeeklyReport() {
    if (selectedWeeks.length === 0) {
        alert("لم تحدد أي أسابيع! الرجاء تحديد أسبوع أو أكثر أولاً.");
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
            <h4>${week.name}</h4>
            <p>عدد الأيام: ${weekDaysCount} يوم</p>
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
        <h3>إحصائيات عامة للأسابيع المحددة</h3>
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
                body { font-family: Arial, sans-serif; padding: 20px; }
                h2 { color: #1a5276; }
                h3 { color: #2a9d8f; }
                table { border-collapse: collapse; width: 100%; margin: 10px 0; }
                th, td { border: 1px solid #ddd; padding: 8px; text-align: center; }
                th { background: #f5f5f5; }
            </style>
        </head>
        <body>
            ${reportHTML}
            <div style="text-align:center; margin-top:20px;">
                <button onclick="window.print()">🖨️ طباعة التقرير</button>
                <button onclick="window.close()">✖ إغلاق</button>
            </div>
        </body>
        </html>
    `);
    reportWindow.document.close();
}

// باقي الوظائف تبقى كما هي مع تعديلات بسيطة
// ... (جميع الوظائف الأخرى من الكود السابق تبقى كما هي)

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

// تحديث عرض التاريخ
function updateDateDisplay() {
    // تحديث التاريخ الميلادي
    const gregorianDateString = getShortGregorianDate(selectedDate);
    document.getElementById('gregorianDateText').innerHTML = gregorianDateString;
    
    // تحديث التاريخ الهجري
    const hijriDateString = `${convertToArabicNumbers(hijriDate.day)} ${hijriDate.monthName} ${convertToArabicNumbers(hijriDate.year)}هـ`;
    document.getElementById('hijriDateText').innerHTML = hijriDateString;
    
    // تحديث عرض التاريخ في لوحة الإدارة
    document.getElementById('adminDateDisplay').innerHTML = gregorianDateString;
    
    // إضافة مؤشر إذا لم يكن تاريخ اليوم
    const today = new Date();
    const isToday = selectedDate.toDateString() === today.toDateString();
    if (!isToday) {
        document.getElementById('gregorianDateText').innerHTML += ' <span style="color:#ffcc00; font-size:11px;">(غير تاريخ اليوم)</span>';
    }
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
    // تنفيذ تحميل البيانات
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

// تحديث حقول التاريخ الهجري في واجهة الإدارة
function updateHijriFields() {
    document.getElementById('hijriDay').value = hijriDate.day;
    document.getElementById('hijriMonth').value = hijriDate.month;
    document.getElementById('hijriYear').value = hijriDate.year;
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
        } else {
            // طريقة احتياطية
            const fixedHijri = getApproximateHijriDate(selectedDate);
            hijriDate.day = fixedHijri.day;
            hijriDate.month = fixedHijri.month;
            hijriDate.year = fixedHijri.year;
            hijriDate.monthName = hijriMonths[fixedHijri.month - 1];
        }
    } catch (error) {
        hijriDate = { day: 1, month: 1, year: 1446, monthName: "محرم" };
    }
}

// طريقة تقريبية لحساب التاريخ الهجري
function getApproximateHijriDate(gregorianDate) {
    const startHijri = new Date(622, 6, 16);
    const diffTime = gregorianDate - startHijri;
    const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24));
    const hijriYear = Math.floor(diffDays / 354.367) + 1;
    const daysInCurrentYear = diffDays % 354.367;
    const hijriMonth = Math.floor(daysInCurrentYear / 29.53) + 1;
    const hijriDay = Math.floor(daysInCurrentYear % 29.53) + 1;
    
    return {
        day: Math.min(Math.max(1, hijriDay), 30),
        month: Math.min(Math.max(1, hijriMonth), 12),
        year: Math.max(1300, Math.min(1500, hijriYear))
    };
}

// تحديث التاريخ الهجري من حقول الإدخال
function updateHijriDate() {
    const day = parseInt(document.getElementById('hijriDay').value) || 1;
    const month = parseInt(document.getElementById('hijriMonth').value) || 1;
    const year = parseInt(document.getElementById('hijriYear').value) || 1446;
    
    hijriDate.day = Math.max(1, Math.min(30, day));
    hijriDate.month = Math.max(1, Math.min(12, month));
    hijriDate.year = Math.max(1300, Math.min(1500, year));
    hijriDate.monthName = hijriMonths[hijriDate.month - 1];
    
    localStorage.setItem('teacherTracker_hijriDate', JSON.stringify(hijriDate));
    updateDateDisplay();
    alert(`تم تحديث التاريخ الهجري إلى: ${hijriDate.day} ${hijriDate.monthName} ${hijriDate.year}هـ`);
}

// الرجوع إلى التاريخ الهجري الفعلي
function resetHijriToToday() {
    calculateHijriFromGregorian();
    updateHijriFields();
    localStorage.setItem('teacherTracker_hijriDate', JSON.stringify(hijriDate));
    updateDateDisplay();
    alert(`تم الرجوع إلى التاريخ الهجري الفعلي: ${hijriDate.day} ${hijriDate.monthName} ${hijriDate.year}هـ`);
}

// تغيير الشهر (للسابق أو القادم)
function changeMonth(offset) {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة لتغيير التاريخ');
        return;
    }
    
    selectedDate.setMonth(selectedDate.getMonth() + offset);
    calculateHijriFromGregorian();
    updateDateDisplay();
    updateHijriFields();
    loadAttendanceData();
}

// تعيين تاريخ مخصص
function setCustomDate() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة لتغيير التاريخ');
        return;
    }
    
    const datePicker = document.getElementById('datePicker');
    if (datePicker.value) {
        selectedDate = new Date(datePicker.value);
        calculateHijriFromGregorian();
        updateDateDisplay();
        updateHijriFields();
        loadAttendanceData();
    }
}

// الرجوع إلى تاريخ اليوم الحقيقي
function resetToToday() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة لتغيير التاريخ');
        return;
    }
    
    selectedDate = new Date();
    calculateHijriFromGregorian();
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('datePicker').value = today;
    updateDateDisplay();
    updateHijriFields();
    loadAttendanceData();
    alert("تم الرجوع إلى تاريخ اليوم الحقيقي");
}

// حفظ التاريخ الحالي
function saveCurrentDate() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة لحفظ التاريخ');
        return;
    }
    
    localStorage.setItem('teacherTracker_selectedDate', selectedDate.toISOString());
    localStorage.setItem('teacherTracker_hijriDate', JSON.stringify(hijriDate));
    alert(`تم حفظ التاريخ الميلادي والهجري`);
}

// عرض تحضير اليوم
function showTodayAttendance() {
    selectedDate = new Date();
    calculateHijriFromGregorian();
    updateDateDisplay();
    loadAttendanceData();
    alert("تم عرض تحضير تاريخ اليوم الحقيقي");
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
    
    saveAttendanceData();
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
        saveAttendanceData();
    } else {
        alert('يجب تفعيل وضع الإدارة أولا');
    }
}

// حفظ بيانات الحضور
function saveAttendanceData() {
    const dateKey = selectedDate.toISOString().split('T')[0];
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
        fillClassTable(className);
        updateStudentCount();
        document.querySelector(`#class-${className} .class-header`).textContent = 
            `الصف ${className} - ${studentsData[className].length} طالب`;
        alert("تمت إضافة الطالب بنجاح");
    }
}

// تحضير عشوائي للتاريخ الحالي
function randomAttendance() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    const confirmAction = confirm("هل تريد تعيين الحضور عشوائيا لجميع الطلاب للتاريخ الحالي؟");
    if (!confirmAction) return;
    
    let totalStudents = 0;
    let starredStudents = 0;
    let regularStudents = 0;
    
    const classSections = document.querySelectorAll('.class-section');
    
    classSections.forEach(section => {
        const rows = section.querySelectorAll('tbody tr');
        rows.forEach(row => {
            totalStudents++;
            const starCell = row.querySelector('.star-cell');
            const hasStar = starCell && starCell.innerHTML === "⭐";
            const attendanceCells = row.querySelectorAll('td[onclick="toggle(this)"]');
            
            attendanceCells.forEach(cell => {
                if (hasStar) {
                    cell.innerHTML = "✔";
                    cell.classList.remove('absent');
                    cell.classList.add('present');
                    starredStudents++;
                } else {
                    cell.innerHTML = Math.random() > 0.3 ? "✔" : "✖";
                    if (cell.innerHTML === "✔") {
                        cell.classList.remove('absent');
                        cell.classList.add('present');
                    } else {
                        cell.classList.remove('present');
                        cell.classList.add('absent');
                    }
                    regularStudents++;
                }
            });
        });
    });
    
    saveAttendanceData();
    alert(`تم تعيين الحضور عشوائيا بنجاح للتاريخ الحالي!\n\nالإحصائيات:\n- إجمالي الطلاب: ${totalStudents}`);
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
        const row = cell.closest('tr');
        row.classList.remove('starred-student');
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
        التاريخ الميلادي: ${getShortGregorianDate(selectedDate)}
        التاريخ الهجري: ${hijriDate.day} ${hijriDate.monthName} ${hijriDate.year}هـ
        ${document.getElementById('currentSemesterInfo').textContent}
    `;
    
    alert(statsMessage);
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

// تهيئة الصفحة عند التحميل
window.onload = initPage;
</script>
</body>
</html>
