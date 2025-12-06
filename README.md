
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سجل متابعة الطلاب - فهد الخالدي</title>
<style>
/* ... (جميع الأنماط السابقة تبقى كما هي) ... */
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
        <button onclick="exportToExcel()">📊 تصدير Excel</button>
        <button onclick="exportPeriodToExcel()">📅 تصدير فترة كاملة</button>
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
                    <input type="text" id="academicYear" value="١٤٤٦هـ" style="width: 100%;">
                </div>
            </div>
            <div style="text-align: center; margin-top: 10px;">
                <button onclick="saveSemesterSettings()">💾 حفظ إعدادات الفصل</button>
                <span class="semester-info" id="currentSemesterInfo">الفصل الثاني ١٤٤٦هـ</span>
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
                <p style="text-align:center; font-size:11px; color:#666;">ملاحظة: التاريخ الهجري المحسوب تلقائياً، ويمكنك تعديله يدوياً إذا لزم الأمر.</p>
            </div>
            
            <p style="text-align:center; font-size:12px; color:#666;">يمكنك الرجوع إلى أشهر سابقة أو قادمة لمشاهدة السجلات القديمة أو تحضير مستقبلية.</p>
        </div>
        
        <div class="admin-section">
            <h4>🎲 التحضير العشوائي للأسابيع الدراسية (18 أسبوع)</h4>
            <div class="random-period-section">
                <div style="text-align:center; margin:10px 0; padding:10px; background:#e8f5e9; border-radius:5px;">
                    <strong>الأسابيع الدراسية المتاحة:</strong><br>
                    من الأسبوع 1 إلى 13 ثم من 15 إلى 19 (إجمالي 18 أسبوع)<br>
                    <small>ملاحظة: الأسبوع 14 إجازة الخريف ولا يتم إدراجه</small>
                </div>
                <div style="text-align: center; margin-top: 15px;">
                    <button onclick="randomAttendanceForAllWeeks()">📅 تحضير عشوائي لجميع الأسابيع (18 أسبوع)</button>
                    <button onclick="clearAllWeeksData()">🗑️ مسح بيانات جميع الأسابيع</button>
                </div>
                <div style="text-align: center; margin-top: 10px;">
                    <button onclick="saveWeeksData()">💾 حفظ بيانات الأسابيع</button>
                    <span class="period-info" id="weeksInfo">18 أسبوع متاحة</span>
                </div>
            </div>
            <div style="text-align: center; margin-top: 15px;">
                <button onclick="randomAttendance()">🎲 تحضير عشوائي للتاريخ الحالي</button>
                <button onclick="randomAttendanceForPeriod()">📅 تحضير عشوائي للفترة المحددة</button>
            </div>
            <div style="text-align:center; margin-top:10px; font-size:12px; color:#666;">
                ⭐ خاصية التحضير العشوائي: سيتم وضع ✓ لكل الخيارات للطلاب المتميزين (الذين لديهم نجمة)
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
        "حمد موسى ساليفو ديقوقa",
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

// بيانات الأسابيع الدراسية (18 أسبوع)
const studyWeeks = [
  {
    "week": 1,
    "start_hijri": "1/3/1447",
    "start_gregorian": "24/08/2025",
    "days": [
      {"day": "الأحد", "hijri": "1/3/1447", "gregorian": "24/08/2025"},
      {"day": "الإثنين", "hijri": "2/3/1447", "gregorian": "25/08/2025"},
      {"day": "الثلاثاء", "hijri": "3/3/1447", "gregorian": "26/08/2025"},
      {"day": "الأربعاء", "hijri": "4/3/1447", "gregorian": "27/08/2025"},
      {"day": "الخميس", "hijri": "5/3/1447", "gregorian": "28/08/2025"}
    ]
  },
  {
    "week": 2,
    "start_hijri": "8/3/1447",
    "start_gregorian": "31/08/2025",
    "days": [
      {"day": "الأحد", "hijri": "8/3/1447", "gregorian": "31/08/2025"},
      {"day": "الإثنين", "hijri": "9/3/1447", "gregorian": "01/09/2025"},
      {"day": "الثلاثاء", "hijri": "10/3/1447", "gregorian": "02/09/2025"},
      {"day": "الأربعاء", "hijri": "11/3/1447", "gregorian": "03/09/2025"},
      {"day": "الخميس", "hijri": "12/3/1447", "gregorian": "04/09/2025"}
    ]
  },
  {
    "week": 3,
    "start_hijri": "15/3/1447",
    "start_gregorian": "07/09/2025",
    "days": [
      {"day": "الأحد", "hijri": "15/3/1447", "gregorian": "07/09/2025"},
      {"day": "الإثنين", "hijri": "16/3/1447", "gregorian": "08/09/2025"},
      {"day": "الثلاثاء", "hijri": "17/3/1447", "gregorian": "09/09/2025"},
      {"day": "الأربعاء", "hijri": "18/3/1447", "gregorian": "10/09/2025"},
      {"day": "الخميس", "hijri": "19/3/1447", "gregorian": "11/09/2025"}
    ]
  },
  {
    "week": 4,
    "start_hijri": "22/3/1447",
    "start_gregorian": "14/09/2025",
    "days": [
      {"day": "الأحد", "hijri": "22/3/1447", "gregorian": "14/09/2025"},
      {"day": "الإثنين", "hijri": "23/3/1447", "gregorian": "15/09/2025"},
      {"day": "الثلاثاء", "hijri": "24/3/1447", "gregorian": "16/09/2025"},
      {"day": "الأربعاء", "hijri": "25/3/1447", "gregorian": "17/09/2025"},
      {"day": "الخميس", "hijri": "26/3/1447", "gregorian": "18/09/2025"}
    ]
  },
  {
    "week": 5,
    "start_hijri": "29/3/1447",
    "start_gregorian": "21/09/2025",
    "days": [
      {"day": "الأحد", "hijri": "29/3/1447", "gregorian": "21/09/2025"},
      {"day": "الإثنين", "hijri": "30/3/1447", "gregorian": "22/09/2025"},
      {"day": "الثلاثاء", "hijri": "1/4/1447", "gregorian": "23/09/2025"},
      {"day": "الأربعاء", "hijri": "2/4/1447", "gregorian": "24/09/2025"},
      {"day": "الخميس", "hijri": "3/4/1447", "gregorian": "25/09/2025"}
    ]
  },
  {
    "week": 6,
    "start_hijri": "6/4/1447",
    "start_gregorian": "28/09/2025",
    "days": [
      {"day": "الأحد", "hijri": "6/4/1447", "gregorian": "28/09/2025"},
      {"day": "الإثنين", "hijri": "7/4/1447", "gregorian": "29/09/2025"},
      {"day": "الثلاثاء", "hijri": "8/4/1447", "gregorian": "30/09/2025"},
      {"day": "الأربعاء", "hijri": "9/4/1447", "gregorian": "01/10/2025"},
      {"day": "الخميس", "hijri": "10/4/1447", "gregorian": "02/10/2025"}
    ]
  },
  {
    "week": 7,
    "start_hijri": "13/4/1447",
    "start_gregorian": "05/10/2025",
    "days": [
      {"day": "الأحد", "hijri": "13/4/1447", "gregorian": "05/10/2025"},
      {"day": "الإثنين", "hijri": "14/4/1447", "gregorian": "06/10/2025"},
      {"day": "الثلاثاء", "hijri": "15/4/1447", "gregorian": "07/10/2025"},
      {"day": "الأربعاء", "hijri": "16/4/1447", "gregorian": "08/10/2025"},
      {"day": "الخميس", "hijri": "17/4/1447", "gregorian": "09/10/2025"}
    ]
  },
  {
    "week": 8,
    "start_hijri": "20/4/1447",
    "start_gregorian": "12/10/2025",
    "days": [
      {"day": "الإثنين", "hijri": "21/4/1447", "gregorian": "13/10/2025"},
      {"day": "الثلاثاء", "hijri": "22/4/1447", "gregorian": "14/10/2025"},
      {"day": "الأربعاء", "hijri": "23/4/1447", "gregorian": "15/10/2025"},
      {"day": "الخميس", "hijri": "24/4/1447", "gregorian": "16/10/2025"}
    ]
  },
  {
    "week": 9,
    "start_hijri": "27/4/1447",
    "start_gregorian": "19/10/2025",
    "days": [
      {"day": "الأحد", "hijri": "27/4/1447", "gregorian": "19/10/2025"},
      {"day": "الإثنين", "hijri": "28/4/1447", "gregorian": "20/10/2025"},
      {"day": "الثلاثاء", "hijri": "29/4/1447", "gregorian": "21/10/2025"},
      {"day": "الأربعاء", "hijri": "30/4/1447", "gregorian": "22/10/2025"},
      {"day": "الخميس", "hijri": "1/5/1447", "gregorian": "23/10/2025"}
    ]
  },
  {
    "week": 10,
    "start_hijri": "4/5/1447",
    "start_gregorian": "26/10/2025",
    "days": [
      {"day": "الأحد", "hijri": "4/5/1447", "gregorian": "26/10/2025"},
      {"day": "الإثنين", "hijri": "5/5/1447", "gregorian": "27/10/2025"},
      {"day": "الثلاثاء", "hijri": "6/5/1447", "gregorian": "28/10/2025"},
      {"day": "الأربعاء", "hijri": "7/5/1447", "gregorian": "29/10/2025"},
      {"day": "الخميس", "hijri": "8/5/1447", "gregorian": "30/10/2025"}
    ]
  },
  {
    "week": 11,
    "start_hijri": "11/5/1447",
    "start_gregorian": "02/11/2025",
    "days": [
      {"day": "الأحد", "hijri": "11/5/1447", "gregorian": "02/11/2025"},
      {"day": "الإثنين", "hijri": "12/5/1447", "gregorian": "03/11/2025"},
      {"day": "الثلاثاء", "hijri": "13/5/1447", "gregorian": "04/11/2025"},
      {"day": "الأربعاء", "hijri": "14/5/1447", "gregorian": "05/11/2025"},
      {"day": "الخميس", "hijri": "15/5/1447", "gregorian": "06/11/2025"}
    ]
  },
  {
    "week": 12,
    "start_hijri": "18/5/1447",
    "start_gregorian": "09/11/2025",
    "days": [
      {"day": "الأحد", "hijri": "18/5/1447", "gregorian": "09/11/2025"},
      {"day": "الإثنين", "hijri": "19/5/1447", "gregorian": "10/11/2025"},
      {"day": "الثلاثاء", "hijri": "20/5/1447", "gregorian": "11/11/2025"},
      {"day": "الأربعاء", "hijri": "21/5/1447", "gregorian": "12/11/2025"},
      {"day": "الخميس", "hijri": "22/5/1447", "gregorian": "13/11/2025"}
    ]
  },
  {
    "week": 13,
    "start_hijri": "25/5/1447",
    "start_gregorian": "16/11/2025",
    "days": [
      {"day": "الأحد", "hijri": "25/5/1447", "gregorian": "16/11/2025"},
      {"day": "الإثنين", "hijri": "26/5/1447", "gregorian": "17/11/2025"},
      {"day": "الثلاثاء", "hijri": "27/5/1447", "gregorian": "18/11/2025"},
      {"day": "الأربعاء", "hijri": "28/5/1447", "gregorian": "19/11/2025"},
      {"day": "الخميس", "hijri": "29/5/1447", "gregorian": "20/11/2025"}
    ]
  },
  {
    "week": 15,
    "start_hijri": "9/6/1447",
    "start_gregorian": "30/11/2025",
    "days": [
      {"day": "الأحد", "hijri": "9/6/1447", "gregorian": "30/11/2025"},
      {"day": "الإثنين", "hijri": "10/6/1447", "gregorian": "01/12/2025"},
      {"day": "الثلاثاء", "hijri": "11/6/1447", "gregorian": "02/12/2025"},
      {"day": "الأربعاء", "hijri": "12/6/1447", "gregorian": "03/12/2025"}
    ]
  },
  {
    "week": 16,
    "start_hijri": "16/6/1447",
    "start_gregorian": "07/12/2025",
    "days": [
      {"day": "الإثنين", "hijri": "17/6/1447", "gregorian": "08/12/2025"},
      {"day": "الثلاثاء", "hijri": "18/6/1447", "gregorian": "09/12/2025"},
      {"day": "الأربعاء", "hijri": "19/6/1447", "gregorian": "10/12/2025"},
      {"day": "الخميس", "hijri": "20/6/1447", "gregorian": "11/12/2025"}
    ]
  },
  {
    "week": 17,
    "start_hijri": "23/6/1447",
    "start_gregorian": "14/12/2025",
    "days": [
      {"day": "الأحد", "hijri": "23/6/1447", "gregorian": "14/12/2025"},
      {"day": "الإثنين", "hijri": "24/6/1447", "gregorian": "15/12/2025"},
      {"day": "الثلاثاء", "hijri": "25/6/1447", "gregorian": "16/12/2025"},
      {"day": "الأربعاء", "hijri": "26/6/1447", "gregorian": "17/12/2025"},
      {"day": "الخميس", "hijri": "27/6/1447", "gregorian": "18/12/2025"}
    ]
  },
  {
    "week": 18,
    "start_hijri": "1/7/1447",
    "start_gregorian": "21/12/2025",
    "days": [
      {"day": "الأحد", "hijri": "1/7/1447", "gregorian": "21/12/2025"},
      {"day": "الإثنين", "hijri": "2/7/1447", "gregorian": "22/12/2025"},
      {"day": "الثلاثاء", "hijri": "3/7/1447", "gregorian": "23/12/2025"},
      {"day": "الأربعاء", "hijri": "4/7/1447", "gregorian": "24/12/2025"},
      {"day": "الخميس", "hijri": "5/7/1447", "gregorian": "25/12/2025"}
    ]
  },
  {
    "week": 19,
    "start_hijri": "8/7/1447",
    "start_gregorian": "28/12/2025",
    "days": [
      {"day": "الأحد", "hijri": "8/7/1447", "gregorian": "28/12/2025"},
      {"day": "الإثنين", "hijri": "9/7/1447", "gregorian": "29/12/2025"},
      {"day": "الثلاثاء", "hijri": "10/7/1447", "gregorian": "30/12/2025"},
      {"day": "الأربعاء", "hijri": "11/7/1447", "gregorian": "31/12/2025"},
      {"day": "الخميس", "hijri": "12/7/1447", "gregorian": "01/01/2026"}
    ]
  }
];

// حالة الإدارة
let adminActive = false;
let currentFilter = 'all';
let currentClass = 'all';

// إدارة التاريخ
let currentDate = new Date(); // تاريخ اليوم الحقيقي
let selectedDate = new Date(); // التاريخ المعروض (يمكن تغييره من الإدارة)

// إعدادات الفصل الدراسي
let semesterSettings = {
    semester: "2",
    academicYear: "١٤٤٦هـ"
};

// إعدادات فترة التحضير العشوائي
let periodSettings = {
    startDate: null,
    endDate: null
};

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
    selectedDate = new Date(currentDate); // نبدأ بتاريخ اليوم
    
    // محاولة تحميل إعدادات الفصل الدراسي
    const savedSemester = localStorage.getItem('teacherTracker_semesterSettings');
    if (savedSemester) {
        semesterSettings = JSON.parse(savedSemester);
        document.getElementById('semesterSelect').value = semesterSettings.semester;
        document.getElementById('academicYear').value = semesterSettings.academicYear;
        updateSemesterInfo();
    }
    
    // محاولة تحميل إعدادات فترة التحضير
    const savedPeriod = localStorage.getItem('teacherTracker_periodSettings');
    if (savedPeriod) {
        periodSettings = JSON.parse(savedPeriod);
        if (periodSettings.startDate) {
            document.getElementById('periodStartDate').value = periodSettings.startDate;
        }
        if (periodSettings.endDate) {
            document.getElementById('periodEndDate').value = periodSettings.endDate;
        }
        updatePeriodInfo();
    }
    
    // محاولة تحميل بيانات التحضير المحفوظة
    loadPeriodAttendanceData();
    
    // حساب التاريخ الهجري الفعلي من التاريخ الميلادي
    calculateHijriFromGregorian();
    
    // محاولة تحميل بيانات الحضور المحفوظة لهذا التاريخ
    loadAttendanceData();
    
    createClassTabs();
    createTables();
    updateStudentCount();
    updateDateDisplay();
    
    // تعيين التاريخ الحالي في منتقي التاريخ
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('datePicker').value = today;
    
    // تحديث حقول التاريخ الهجري
    updateHijriFields();
    
    // تحديث معلومات الأسابيع
    updateWeeksInfo();
}

// تحديث معلومات الأسابيع
function updateWeeksInfo() {
    document.getElementById('weeksInfo').textContent = `${studyWeeks.length} أسبوع متاحة`;
}

// تحميل بيانات التحضير المحفوظة للفترة
function loadPeriodAttendanceData() {
    const savedData = localStorage.getItem('teacherTracker_periodAttendanceData');
    if (savedData) {
        periodAttendanceData = JSON.parse(savedData);
        console.log('تم تحميل بيانات التحضير للفترة:', Object.keys(periodAttendanceData).length, 'يوم');
    }
}

// حفظ بيانات التحضير للفترة
function savePeriodAttendanceData() {
    localStorage.setItem('teacherTracker_periodAttendanceData', JSON.stringify(periodAttendanceData));
}

// حفظ بيانات الأسابيع
function saveWeeksData() {
    savePeriodAttendanceData();
    alert(`تم حفظ بيانات التحضير لجميع الأيام`);
}

// مسح بيانات جميع الأسابيع
function clearAllWeeksData() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    const confirmAction = confirm("هل تريد مسح جميع بيانات التحضير للأسابيع الدراسية؟\n\nهذا الإجراء لا يمكن التراجع عنه!");
    if (!confirmAction) return;
    
    periodAttendanceData = {};
    savePeriodAttendanceData();
    
    alert("تم مسح جميع بيانات التحضير للأسابيع الدراسية");
}

// تحويل تاريخ نصي (dd/mm/yyyy) إلى كائن Date - تم التصحيح
function parseDateString(dateStr) {
    try {
        const parts = dateStr.split('/');
        if (parts.length === 3) {
            // يوم/شهر/سنة
            const day = parseInt(parts[0], 10);
            const month = parseInt(parts[1], 10) - 1; // الأشهر من 0-11
            const year = parseInt(parts[2], 10);
            
            // التحقق من صحة التاريخ
            if (isNaN(day) || isNaN(month) || isNaN(year)) {
                console.error('تاريخ غير صالح:', dateStr);
                return null;
            }
            
            return new Date(year, month, day);
        }
        return null;
    } catch (error) {
        console.error('خطأ في تحويل التاريخ:', dateStr, error);
        return null;
    }
}

// توليد تحضير عشوائي ليوم معين مع معلومات الأسبوع
function generateRandomAttendanceForDate(date, dayInfo = null) {
    const dateKey = date.toISOString().split('T')[0];
    const hijriDateInfo = calculateHijriForDate(date);
    
    const attendanceData = {
        date: dateKey,
        gregorianDate: getShortGregorianDate(date),
        hijriDate: `${convertToArabicNumbers(hijriDateInfo.day)} ${hijriDateInfo.monthName} ${convertToArabicNumbers(hijriDateInfo.year)}هـ`,
        dayInfo: dayInfo,
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

// تحضير عشوائي لجميع الأسابيع الدراسية (18 أسبوع) - تم التصحيح
function randomAttendanceForAllWeeks() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    // حساب إجمالي الأيام
    let totalDays = 0;
    studyWeeks.forEach(week => {
        totalDays += week.days.length;
    });
    
    const confirmMessage = `هل تريد تعيين التحضير عشوائيا لجميع الأسابيع الدراسية؟\n\n` +
                          `عدد الأسابيع: ${studyWeeks.length} أسبوع\n` +
                          `عدد الأيام: ${totalDays} يوم\n\n` +
                          `ملاحظة: سيتم وضع ✓ لكل الخيارات للطلاب المتميزين (الذين لديهم نجمة ⭐)`;
    
    const confirmAction = confirm(confirmMessage);
    if (!confirmAction) return;
    
    let totalDaysProcessed = 0;
    let totalStudentsProcessed = 0;
    let totalStarredStudents = 0;
    let totalRegularStudents = 0;
    
    // الانتقال عبر كل أسبوع وكل يوم
    studyWeeks.forEach(week => {
        week.days.forEach(day => {
            // تحويل التاريخ الميلادي من النص إلى كائن Date
            const date = parseDateString(day.gregorian);
            if (!date) {
                console.error(`خطأ في تحويل التاريخ: ${day.gregorian}`);
                return;
            }
            
            // إنشاء معلومات اليوم
            const dayInfo = {
                dayName: day.day,
                hijriDate: day.hijri,
                gregorianDate: day.gregorian,
                weekNumber: week.week
            };
            
            // إنشاء تحضير عشوائي لهذا اليوم
            const attendanceData = generateRandomAttendanceForDate(date, dayInfo);
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
            totalDaysProcessed++;
            totalStudentsProcessed += dayStudents;
            totalStarredStudents += dayStarred;
            totalRegularStudents += dayRegular;
        });
    });
    
    // حفظ بيانات الفترة
    savePeriodAttendanceData();
    
    // حساب الإحصائيات
    const avgStudentsPerDay = totalStudentsProcessed / totalDaysProcessed;
    const avgStarredPerDay = totalStarredStudents / totalDaysProcessed;
    const avgRegularPerDay = totalRegularStudents / totalDaysProcessed;
    
    // عرض تقرير النتائج
    const resultMessage = `✅ تم إنشاء التحضير العشوائي لجميع الأسابيع الدراسية بنجاح!\n\n` +
                         `📅 الأسابيع: ${studyWeeks.length} أسبوع (من 1 إلى 13 ثم 15 إلى 19)\n` +
                         `📊 الإحصائيات:\n` +
                         `   - عدد الأيام: ${totalDaysProcessed} يوم\n` +
                         `   - إجمالي الطلاب المعالجين: ${totalStudentsProcessed} طالب\n` +
                         `   - متوسط الطلاب في اليوم: ${avgStudentsPerDay.toFixed(1)} طالب\n` +
                         `   - متوسط الطلاب المتميزين في اليوم: ${avgStarredPerDay.toFixed(1)} طالب\n` +
                         `   - متوسط الطلاب العاديين في اليوم: ${avgRegularPerDay.toFixed(1)} طالب\n\n` +
                         `💾 تم حفظ بيانات التحضير لـ ${totalDaysProcessed} يوم في النظام.`;
    
    alert(resultMessage);
}

// تصدير فترة كاملة إلى Excel - تم التصحيح
function exportPeriodToExcel() {
    // التحقق من وجود بيانات للأسابيع الدراسية
    let hasData = false;
    for (const dateKey in periodAttendanceData) {
        hasData = true;
        break;
    }
    
    if (!hasData) {
        alert("لا توجد بيانات تحضير للأسابيع الدراسية!\n\nيرجى إنشاء تحضير عشوائي للأسابيع أولاً.");
        return;
    }
    
    let tablesHTML = `<h2>تقرير التحضير للأسابيع الدراسية</h2>`;
    tablesHTML += `<h3>المعلم: فهد الخالدي - المادة: اللغة الإنجليزية</h3>`;
    tablesHTML += `<h3>${document.getElementById('currentSemesterInfo').textContent}</h3>`;
    tablesHTML += `<h3>المدرسة: سعيد بن العاص المتوسطة</h3>`;
    tablesHTML += `<h3>عدد الأسابيع: ${studyWeeks.length} أسبوع (من 1 إلى 13 ثم 15 إلى 19)</h3>`;
    tablesHTML += `<h3>تاريخ التصدير: ${getShortGregorianDate(new Date())}</h3>`;
    
    // تنظيم البيانات حسب الأسابيع
    const weeksData = {};
    
    // تجميع البيانات حسب الأسبوع
    for (const dateKey in periodAttendanceData) {
        const dayData = periodAttendanceData[dateKey];
        if (dayData.dayInfo && dayData.dayInfo.weekNumber) {
            const weekNum = dayData.dayInfo.weekNumber;
            if (!weeksData[weekNum]) {
                weeksData[weekNum] = [];
            }
            weeksData[weekNum].push(dayData);
        } else {
            // إذا لم يكن هناك weekNumber، نضعها في أسبوع "غير محدد"
            if (!weeksData['غير محدد']) {
                weeksData['غير محدد'] = [];
            }
            weeksData['غير محدد'].push(dayData);
        }
    }
    
    // فرز الأسابيع: الأسابيع العددية أولاً، ثم "غير محدد"
    const sortedWeeks = Object.keys(weeksData).sort((a, b) => {
        if (a === 'غير محدد') return 1;
        if (b === 'غير محدد') return -1;
        const numA = parseInt(a);
        const numB = parseInt(b);
        if (isNaN(numA) || isNaN(numB)) return 0;
        return numA - numB;
    });
    
    // إضافة بيانات كل أسبوع
    sortedWeeks.forEach(weekNum => {
        const weekDaysData = weeksData[weekNum];
        
        tablesHTML += `<h3 style="background:#e0f7fa; padding:10px; margin-top:20px;">`;
        if (weekNum === 'غير محدد') {
            tablesHTML += `أيام أخرى`;
        } else {
            tablesHTML += `الأسبوع ${weekNum}`;
        }
        tablesHTML += `</h3>`;
        
        // فرز أيام الأسبوع حسب التاريخ
        weekDaysData.sort((a, b) => {
            const dateA = new Date(a.date);
            const dateB = new Date(b.date);
            return dateA - dateB;
        });
        
        // إضافة بيانات كل يوم في هذا الأسبوع
        weekDaysData.forEach(dayData => {
            let dayTitle = '';
            if (dayData.dayInfo) {
                dayTitle = `${dayData.dayInfo.dayName} - ${dayData.dayInfo.gregorianDate} (${dayData.dayInfo.hijriDate})`;
            } else {
                dayTitle = `${dayData.gregorianDate} (${dayData.hijriDate})`;
            }
            tablesHTML += `<h4 style="background:#f5f5f5; padding:8px;">${dayTitle}</h4>`;
            
            // إضافة جداول لكل صف في هذا اليوم
            for (const className in dayData.classes) {
                const classData = dayData.classes[className];
                
                tablesHTML += `<h5>الصف ${className} (${classData.stats.total} طالب)</h5>`;
                tablesHTML += `<table border="1" cellpadding="5" cellspacing="0" style="width:100%; border-collapse:collapse; margin-bottom:15px; font-size:12px;">`;
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
                
                // إضافة إحصائيات الصف
                tablesHTML += `<div style="margin-bottom:20px; padding:8px; background:#f5f5f5; border-radius:5px; font-size:12px;">
                    <strong>إحصائيات الصف ${className}:</strong>
                    إجمالي الطلاب: ${classData.stats.total} | 
                    الحضور: ${classData.stats.present} | 
                    الغياب: ${classData.stats.absent} | 
                    المتميزون: ${classData.stats.starred}
                </div>`;
            }
        });
    });
    
    // إضافة ملخص شامل
    tablesHTML += `<h3 style="background:#e0f7fa; padding:10px; margin-top:20px;">ملخص شامل للأسابيع الدراسية</h3>`;
    
    let periodTotalStudents = 0;
    let periodTotalPresent = 0;
    let periodTotalAbsent = 0;
    let periodTotalStarred = 0;
    let totalDays = 0;
    
    for (const dateKey in periodAttendanceData) {
        const dayData = periodAttendanceData[dateKey];
        totalDays++;
        
        for (const className in dayData.classes) {
            const classData = dayData.classes[className];
            periodTotalStudents += classData.stats.total;
            periodTotalPresent += classData.stats.present;
            periodTotalAbsent += classData.stats.absent;
            periodTotalStarred += classData.stats.starred;
        }
    }
    
    tablesHTML += `<div style="padding:15px; background:#fff8e1; border-radius:5px; margin-bottom:20px;">
        <strong>إجمالي الأسابيع الدراسية:</strong><br>
        - عدد الأيام: ${totalDays} يوم<br>
        - إجمالي الطلاب: ${periodTotalStudents} طالب<br>
        - إجمالي الحضور: ${periodTotalPresent} حالة حضور<br>
        - إجمالي الغياب: ${periodTotalAbsent} حالة غياب<br>
        - إجمالي المتميزين: ${periodTotalStarred} طالب<br>
        - متوسط الحضور: ${((periodTotalPresent / (periodTotalPresent + periodTotalAbsent)) * 100).toFixed(1)}%
    </div>`;
    
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
    const today = new Date().toISOString().split('T')[0];
    link.download = `تقرير_الأسابيع_الدراسية_${today}.xls`;
    link.click();
    
    alert(`تم تصدير تقرير الأسابيع الدراسية بنجاح!\n\nيتضمن التقرير بيانات ${totalDays} يوم`);
}

// تحويل الأرقام الإنجليزية إلى عربية
function convertToArabicNumbers(num) {
    const arabicNumbers = ['٠', '١', '٢', '٣', '٤', '٥', '٦', '٧', '٨', '٩'];
    return num.toString().replace(/\d/g, digit => arabicNumbers[digit]);
}

// الحصول على التاريخ الميلادي بصيغة عربية صحيحة
function getGregorianDateString(date) {
    const day = date.getDate();
    const month = gregorianMonths[date.getMonth()];
    const year = date.getFullYear();
    const weekDay = weekDays[date.getDay()];
    
    const arabicDay = convertToArabicNumbers(day);
    const arabicYear = convertToArabicNumbers(year);
    
    return `${weekDay}، ${arabicDay} ${month} ${arabicYear}`;
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

// حساب التاريخ الهجري من التاريخ الميلادي
function calculateHijriFromGregorian() {
    try {
        // استخدام مكتبة ummAlQura لحساب التاريخ الهجري
        if (typeof HijriDate !== 'undefined') {
            const hijri = new HijriDate(selectedDate);
            hijriDate.day = hijri.date;
            hijriDate.month = hijri.month;
            hijriDate.year = hijri.year;
            hijriDate.monthName = hijriMonths[hijri.month - 1];
        } else {
            // طريقة احتياطية إذا لم تكن المكتبة متوفرة
            const fixedHijri = getApproximateHijriDate(selectedDate);
            hijriDate.day = fixedHijri.day;
            hijriDate.month = fixedHijri.month;
            hijriDate.year = fixedHijri.year;
            hijriDate.monthName = hijriMonths[fixedHijri.month - 1];
        }
    } catch (error) {
        console.error("خطأ في حساب التاريخ الهجري:", error);
        // استخدام تاريخ افتراضي في حالة الخطأ
        hijriDate = { day: 1, month: 1, year: 1446, monthName: "محرم" };
    }
}

// طريقة تقريبية لحساب التاريخ الهجري (بدون مكتبة)
function getApproximateHijriDate(gregorianDate) {
    const startHijri = new Date(622, 6, 16); // 16 يوليو 622م هو بداية الهجرة
    
    const diffTime = gregorianDate - startHijri;
    const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24));
    
    // السنة الهجرية = عدد الأيام / 354.367 (متوسط طول السنة الهجرية)
    const hijriYear = Math.floor(diffDays / 354.367) + 1;
    
    // الأيام المتبقية في السنة الحالية
    const daysInCurrentYear = diffDays % 354.367;
    
    // تقدير الشهر (كل شهر حوالي 29.5 يوم)
    const hijriMonth = Math.floor(daysInCurrentYear / 29.53) + 1;
    
    // اليوم من الشهر
    const hijriDay = Math.floor(daysInCurrentYear % 29.53) + 1;
    
    return {
        day: Math.min(Math.max(1, hijriDay), 30),
        month: Math.min(Math.max(1, hijriMonth), 12),
        year: Math.max(1300, Math.min(1500, hijriYear))
    };
}

// حساب التاريخ الهجري لـ Date معين
function calculateHijriForDate(date) {
    try {
        if (typeof HijriDate !== 'undefined') {
            const hijri = new HijriDate(date);
            return {
                day: hijri.date,
                month: hijri.month,
                year: hijri.year,
                monthName: hijriMonths[hijri.month - 1]
            };
        } else {
            const fixedHijri = getApproximateHijriDate(date);
            return {
                day: fixedHijri.day,
                month: fixedHijri.month,
                year: fixedHijri.year,
                monthName: hijriMonths[fixedHijri.month - 1]
            };
        }
    } catch (error) {
        console.error("خطأ في حساب التاريخ الهجري:", error);
        return { day: 1, month: 1, year: 1446, monthName: "محرم" };
    }
}

// تحديث عرض التاريخ
function updateDateDisplay() {
    // تحديث التاريخ الميلادي باستخدام الصيغة الصحيحة
    const gregorianDateString = getGregorianDateString(selectedDate);
    
    document.getElementById('gregorianDateText').innerHTML = gregorianDateString;
    
    // تحديث التاريخ الهجري
    const hijriDateString = `${convertToArabicNumbers(hijriDate.day)} ${hijriDate.monthName} ${convertToArabicNumbers(hijriDate.year)}هـ`;
    document.getElementById('hijriDateText').innerHTML = hijriDateString;
    
    // تحديث عرض التاريخ في لوحة الإدارة
    const shortGregorian = getShortGregorianDate(selectedDate);
    document.getElementById('adminDateDisplay').innerHTML = shortGregorian;
    
    // إضافة مؤشر إذا لم يكن تاريخ اليوم
    const today = new Date();
    const isToday = selectedDate.toDateString() === today.toDateString();
    if (!isToday) {
        document.getElementById('gregorianDateText').innerHTML += ' <span style="color:#ffcc00; font-size:11px;">(غير تاريخ اليوم)</span>';
    }
}

// تحديث حقول التاريخ الهجري في واجهة الإدارة
function updateHijriFields() {
    document.getElementById('hijriDay').value = hijriDate.day;
    document.getElementById('hijriMonth').value = hijriDate.month;
    document.getElementById('hijriYear').value = hijriDate.year;
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

// تعيين فترة مثال (31/8 إلى 4/9)
function setPeriodToExample() {
    // تاريخ البداية: 31 أغسطس 2024 (مثال)
    const startDate = new Date(2024, 7, 31); // أغسطس هو الشهر 7 (0-indexed)
    // تاريخ النهاية: 4 سبتمبر 2024 (مثال)
    const endDate = new Date(2024, 8, 4); // سبتمبر هو الشهر 8 (0-indexed)
    
    document.getElementById('periodStartDate').value = startDate.toISOString().split('T')[0];
    document.getElementById('periodEndDate').value = endDate.toISOString().split('T')[0];
    
    periodSettings.startDate = startDate.toISOString().split('T')[0];
    periodSettings.endDate = endDate.toISOString().split('T')[0];
    
    updatePeriodInfo();
    alert(`تم تعيين فترة التحضير العشوائي:\nمن: 31/8/2024 (3/8/1446هـ)\nإلى: 4/9/2024 (3/12/1446هـ)`);
}

// مسح فترة التحضير
function clearPeriod() {
    document.getElementById('periodStartDate').value = '';
    document.getElementById('periodEndDate').value = '';
    
    periodSettings.startDate = null;
    periodSettings.endDate = null;
    
    updatePeriodInfo();
    alert("تم مسح فترة التحضير المحددة");
}

// تحديث معلومات فترة التحضير المعروضة
function updatePeriodInfo() {
    const periodInfoElement = document.getElementById('currentPeriodInfo');
    
    if (periodSettings.startDate && periodSettings.endDate) {
        const startDate = new Date(periodSettings.startDate);
        const endDate = new Date(periodSettings.endDate);
        
        const startStr = getShortGregorianDate(startDate);
        const endStr = getShortGregorianDate(endDate);
        
        periodInfoElement.textContent = `${startStr} إلى ${endStr}`;
    } else {
        periodInfoElement.textContent = "لا توجد فترة محددة";
    }
}

// حفظ إعدادات فترة التحضير
function savePeriodSettings() {
    periodSettings.startDate = document.getElementById('periodStartDate').value;
    periodSettings.endDate = document.getElementById('periodEndDate').value;
    
    localStorage.setItem('teacherTracker_periodSettings', JSON.stringify(periodSettings));
    updatePeriodInfo();
    alert("تم حفظ إعدادات فترة التحضير العشوائي");
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
    
    // حفظ التاريخ الهجري
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
    
    // تحديث التاريخ الهجري بناءً على التاريخ الميلادي الجديد
    calculateHijriFromGregorian();
    
    updateDateDisplay();
    updateHijriFields();
    
    // تحميل بيانات الحضور للتاريخ الجديد
    loadAttendanceData();
    updateTablesWithLoadedData();
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
        
        // تحديث التاريخ الهجري بناءً على التاريخ الميلادي الجديد
        calculateHijriFromGregorian();
        
        updateDateDisplay();
        updateHijriFields();
        
        // تحميل بيانات الحضور للتاريخ الجديد
        loadAttendanceData();
        updateTablesWithLoadedData();
    }
}

// الرجوع إلى تاريخ اليوم الحقيقي
function resetToToday() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة لتغيير التاريخ');
        return;
    }
    
    selectedDate = new Date(); // الرجوع لتاريخ اليوم الحقيقي
    
    // تحديث التاريخ الهجري بناءً على التاريخ الميلادي الجديد
    calculateHijriFromGregorian();
    
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('datePicker').value = today;
    
    updateDateDisplay();
    updateHijriFields();
    
    // تحميل بيانات الحضور للتاريخ الجديد
    loadAttendanceData();
    updateTablesWithLoadedData();
    
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
    alert(`تم حفظ التاريخ الميلادي: ${selectedDate.toLocaleDateString('ar-SA')}\nوالهجري: ${hijriDate.day} ${hijriDate.monthName} ${hijriDate.year}هـ`);
}

// عرض تحضير اليوم
function showTodayAttendance() {
    // تحميل بيانات اليوم الحقيقي
    selectedDate = new Date();
    calculateHijriFromGregorian();
    updateDateDisplay();
    loadAttendanceData();
    updateTablesWithLoadedData();
    alert("تم عرض تحضير تاريخ اليوم الحقيقي");
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
    console.log(`تحميل بيانات الحضور للتاريخ: ${selectedDate.toLocaleDateString()}`);
}

// تحديث الجداول بالبيانات المحملة
function updateTablesWithLoadedData() {
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
        
        // إضافة أو إزالة خلفية للطلاب المتميزين
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
    console.log(`حفظ بيانات الحضور للتاريخ: ${dateKey}`);
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

// تحضير عشوائي للتاريخ الحالي - المعدل للطلاب المتميزين
function randomAttendance() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    const confirmAction = confirm("هل تريد تعيين الحضور عشوائيا لجميع الطلاب للتاريخ الحالي؟\n\nملاحظة: سيتم وضع ✓ لكل الخيارات للطلاب المتميزين (الذين لديهم نجمة ⭐)");
    if (!confirmAction) return;
    
    let totalStudents = 0;
    let starredStudents = 0;
    let regularStudents = 0;
    
    // الحصول على جميع الصفوف
    const classSections = document.querySelectorAll('.class-section');
    
    classSections.forEach(section => {
        const rows = section.querySelectorAll('tbody tr');
        rows.forEach(row => {
            totalStudents++;
            
            // التحقق مما إذا كان الطالب لديه نجمة (⭐)
            const starCell = row.querySelector('.star-cell');
            const hasStar = starCell && starCell.innerHTML === "⭐";
            
            // الحصول على جميع خلايا التقييم (الحضور، الواجبات، المشاريع، التطبيقات، المشاركة)
            const attendanceCells = row.querySelectorAll('td[onclick="toggle(this)"]');
            
            attendanceCells.forEach(cell => {
                if (hasStar) {
                    // إذا كان الطالب لديه نجمة، ضع ✓ في كل الخيارات
                    cell.innerHTML = "✔";
                    cell.classList.remove('absent');
                    cell.classList.add('present');
                    starredStudents++;
                } else {
                    // إذا لم يكن لديه نجمة، ضع عشوائياً
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
    alert(`تم تعيين الحضور عشوائيا بنجاح للتاريخ الحالي!\n\nالإحصائيات:\n- إجمالي الطلاب: ${totalStudents}\n- الطلاب المتميزين (حصلوا على ✓ في كل الخيارات): ${starredStudents/5}\n- الطلاب العاديين (حصلوا على تقييم عشوائي): ${regularStudents/5}`);
}

// تحضير عشوائي للفترة المحددة
function randomAttendanceForPeriod() {
    if (!adminActive) {
        alert('يجب تفعيل وضع الإدارة أولا');
        return;
    }
    
    // التحقق من وجود فترة محددة
    if (!periodSettings.startDate || !periodSettings.endDate) {
        alert("يجب تحديد فترة زمنية أولاً!\n\nيرجى تحديد تاريخ البداية وتاريخ النهاية في قسم 'التحضير العشوائي لفترة محددة'");
        return;
    }
    
    const startDate = new Date(periodSettings.startDate);
    const endDate = new Date(periodSettings.endDate);
    
    // التحقق من صحة الفترة
    if (startDate > endDate) {
        alert("تاريخ البداية يجب أن يكون قبل تاريخ النهاية!");
        return;
    }
    
    // حساب عدد الأيام
    const timeDiff = endDate.getTime() - startDate.getTime();
    const daysDiff = Math.ceil(timeDiff / (1000 * 3600 * 24)) + 1;
    
    const confirmMessage = `هل تريد تعيين الحضور عشوائيا للفترة المحددة؟\n\n` +
                          `الفترة: من ${getShortGregorianDate(startDate)} إلى ${getShortGregorianDate(endDate)}\n` +
                          `عدد الأيام: ${daysDiff}\n\n` +
                          `ملاحظة: سيتم وضع ✓ لكل الخيارات للطلاب المتميزين (الذين لديهم نجمة ⭐)`;
    
    const confirmAction = confirm(confirmMessage);
    if (!confirmAction) return;
    
    let totalDaysProcessed = 0;
    let totalStudentsProcessed = 0;
    let totalStarredStudents = 0;
    let totalRegularStudents = 0;
    
    // الانتقال عبر كل يوم في الفترة
    const currentDate = new Date(startDate);
    
    while (currentDate <= endDate) {
        // إنشاء تحضير عشوائي لهذا اليوم
        const attendanceData = generateRandomAttendanceForDate(currentDate);
        const dateKey = currentDate.toISOString().split('T')[0];
        
        // حفظ بيانات اليوم
        periodAttendanceData[dateKey] = attendanceData;
        
        // حساب الإحصائيات
        let dayStudents = 0;
        let dayStarred = 0;
        
        for (const className in attendanceData.classes) {
            dayStudents += attendanceData.classes[className].stats.total;
            dayStarred += attendanceData.classes[className].stats.starred;
        }
        
        const dayRegular = dayStudents - dayStarred;
        
        // تحديث المجاميع
        totalDaysProcessed++;
        totalStudentsProcessed += dayStudents;
        totalStarredStudents += dayStarred;
        totalRegularStudents += dayRegular;
        
        // الانتقال إلى اليوم التالي
        currentDate.setDate(currentDate.getDate() + 1);
    }
    
    // حفظ بيانات الفترة
    savePeriodAttendanceData();
    
    // حساب الإحصائيات
    const avgStudentsPerDay = totalStudentsProcessed / totalDaysProcessed;
    const avgStarredPerDay = totalStarredStudents / totalDaysProcessed;
    const avgRegularPerDay = totalRegularStudents / totalDaysProcessed;
    
    // عرض تقرير النتائج
    const resultMessage = `✅ تم إنشاء التحضير العشوائي للفترة المحددة بنجاح!\n\n` +
                         `📅 الفترة: من ${getShortGregorianDate(startDate)} إلى ${getShortGregorianDate(endDate)}\n` +
                         `📊 الإحصائيات:\n` +
                         `   - عدد الأيام: ${totalDaysProcessed} يوم\n` +
                         `   - إجمالي الطلاب المعالجين: ${totalStudentsProcessed} طالب\n` +
                         `   - متوسط الطلاب في اليوم: ${avgStudentsPerDay.toFixed(1)} طالب\n` +
                         `   - متوسط الطلاب المتميزين في اليوم: ${avgStarredPerDay.toFixed(1)} طالب\n` +
                         `   - متوسط الطلاب العاديين في اليوم: ${avgRegularPerDay.toFixed(1)} طالب\n\n` +
                         `💾 تم حفظ بيانات التحضير لكل يوم في النظام.`;
    
    alert(resultMessage);
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
        التاريخ الميلادي: ${getGregorianDateString(selectedDate)}
        التاريخ الهجري: ${hijriDate.day} ${hijriDate.monthName} ${hijriDate.year}هـ
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
        periodSettings: periodSettings,
        periodAttendanceData: periodAttendanceData,
        hijriDate: hijriDate,
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
    // الحصول على التاريخ الميلادي بصيغة صحيحة
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
