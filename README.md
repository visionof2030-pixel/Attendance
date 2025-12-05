<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>سجل متابعة الطلاب - مادة اللغة الإنجليزية</title>

<!-- خط جميل وواضح -->
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">

<!-- أيقونات -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- مكتبة moment للتواريخ -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.29.4/moment.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment-hijri/2.1.2/moment-hijri.min.js"></script>

<style>
    :root {
        --primary-color: #2563eb;
        --primary-dark: #1e40af;
        --success-color: #10b981;
        --success-dark: #059669;
        --success-light: #d1fae5;
        --danger-color: #ef4444;
        --danger-dark: #b91c1c;
        --danger-light: #fee2e2;
        --purple-color: #7c3aed;
        --purple-dark: #5b21b6;
        --light-gray: #f5f7fa;
        --white: #ffffff;
        --shadow: rgba(0,0,0,0.1);
        --gray-200: #e5e7eb;
        --warning-color: #f59e0b;
    }

    * {
        box-sizing: border-box;
    }

    body {
        font-family: "Cairo", sans-serif;
        background: var(--light-gray);
        margin: 0;
        padding: 0;
        line-height: 1.6;
    }

    /* ============= الهيدر ============= */
    header {
        background: linear-gradient(90deg, #1d4ed8, #2563eb);
        padding: 15px 10px;
        color: white;
        text-align: center;
        box-shadow: 0px 3px 10px rgba(0,0,0,0.2);
        position: sticky;
        top: 0;
        z-index: 100;
    }

    header h1 {
        margin: 0;
        font-size: 22px;
        font-weight: 700;
    }

    header h2 {
        margin: 5px 0 0 0;
        font-size: 16px;
        opacity: .9;
    }

    /* ============= القائمة العلوية ============= */
    .class-selector {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        gap: 8px;
        margin: 15px 10px;
        padding: 10px 0;
    }

    .class-selector button {
        background: var(--primary-color);
        color: white;
        border: none;
        padding: 10px 14px;
        border-radius: 6px;
        font-size: 16px;
        cursor: pointer;
        transition: .3s;
        flex: 1;
        min-width: 70px;
        max-width: 100px;
    }

    .class-selector button:hover,
    .class-selector button.active {
        background: var(--primary-dark);
        transform: translateY(-2px);
    }

    /* =============== التاريخ =============== */
    .date-container {
        width: 95%;
        max-width: 1100px;
        margin: 15px auto;
        background: var(--white);
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0px 4px 12px var(--shadow);
        display: flex;
        flex-direction: column;
        gap: 20px;
    }

    .date-group {
        display: flex;
        flex-direction: column;
        width: 100%;
    }

    .date-group label {
        font-weight: 600;
        margin-bottom: 8px;
        font-size: 16px;
        display: flex;
        align-items: center;
        gap: 8px;
    }

    .date-group label i {
        color: var(--primary-color);
    }

    .date-input {
        padding: 14px;
        font-size: 16px;
        border-radius: 8px;
        border: 2px solid #ddd;
        font-family: "Cairo", sans-serif;
        width: 100%;
        transition: all 0.3s ease;
    }

    .date-input:focus {
        border-color: var(--primary-color);
        box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
        outline: none;
    }

    .date-row {
        display: flex;
        gap: 20px;
        flex-wrap: wrap;
    }

    .date-row .date-group {
        flex: 1;
        min-width: 250px;
    }

    .conversion-notice {
        background: #f0f9ff;
        border: 1px solid #bae6fd;
        border-radius: 8px;
        padding: 12px 16px;
        margin-top: 10px;
        font-size: 14px;
        color: #0369a1;
        display: flex;
        align-items: center;
        gap: 10px;
        animation: fadeIn 0.5s ease;
    }

    .conversion-notice i {
        color: #0ea5e9;
    }

    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-10px); }
        to { opacity: 1; transform: translateY(0); }
    }

    /* =============== جدول الطلاب =============== */
    .container {
        width: 95%;
        max-width: 1100px;
        margin: 15px auto;
        background: var(--white);
        padding: 15px;
        border-radius: 12px;
        box-shadow: 0px 4px 12px var(--shadow);
        overflow-x: auto;
    }

    .table-wrapper {
        overflow-x: auto;
        margin-top: 15px;
        border-radius: 8px;
        border: 1px solid #eee;
    }

    table {
        width: 100%;
        border-collapse: collapse;
        min-width: 600px;
    }

    th, td {
        padding: 14px 10px;
        text-align: center;
        border-bottom: 1px solid #eee;
    }

    th {
        background: var(--primary-dark);
        color: white;
        font-size: 16px;
        font-weight: 600;
        white-space: nowrap;
    }

    /* =============== الأسماء =============== */
    .student-name {
        font-size: 18px;
        font-weight: 700;
        text-align: right;
        padding-right: 15px;
    }

    /* ============= أزرار الحضور والغياب ============= */
    .attendance-buttons {
        display: flex;
        justify-content: center;
        gap: 10px;
    }

    .btn {
        padding: 10px 16px;
        border-radius: 8px;
        font-size: 16px;
        cursor: pointer;
        transition: all 0.3s ease;
        border: none;
        min-width: 70px;
        font-weight: 600;
        position: relative;
        overflow: hidden;
    }

    .btn::after {
        content: '';
        position: absolute;
        top: 50%;
        left: 50%;
        width: 5px;
        height: 5px;
        background: rgba(255, 255, 255, 0.5);
        opacity: 0;
        border-radius: 100%;
        transform: scale(1, 1) translate(-50%);
        transform-origin: 50% 50%;
    }

    .btn:focus:not(:active)::after {
        animation: ripple 1s ease-out;
    }

    @keyframes ripple {
        0% {
            transform: scale(0, 0);
            opacity: 0.5;
        }
        20% {
            transform: scale(25, 25);
            opacity: 0.3;
        }
        100% {
            opacity: 0;
            transform: scale(40, 40);
        }
    }

    .present {
        background: var(--success-color);
        color: white;
    }
    
    .present:hover {
        background: var(--success-dark);
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(16, 185, 129, 0.4);
    }
    
    .present.active {
        background: var(--success-dark);
        transform: scale(1.05);
        box-shadow: 0 0 0 3px var(--success-light), 0 4px 12px rgba(16, 185, 129, 0.4);
        animation: pulse-present 2s infinite;
    }

    .absent {
        background: var(--danger-color);
        color: white;
    }
    
    .absent:hover {
        background: var(--danger-dark);
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(239, 68, 68, 0.4);
    }
    
    .absent.active {
        background: var(--danger-dark);
        transform: scale(1.05);
        box-shadow: 0 0 0 3px var(--danger-light), 0 4px 12px rgba(239, 68, 68, 0.4);
        animation: pulse-absent 2s infinite;
    }

    @keyframes pulse-present {
        0% {
            box-shadow: 0 0 0 0 rgba(16, 185, 129, 0.7);
        }
        70% {
            box-shadow: 0 0 0 10px rgba(16, 185, 129, 0);
        }
        100% {
            box-shadow: 0 0 0 0 rgba(16, 185, 129, 0);
        }
    }

    @keyframes pulse-absent {
        0% {
            box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.7);
        }
        70% {
            box-shadow: 0 0 0 10px rgba(239, 68, 68, 0);
        }
        100% {
            box-shadow: 0 0 0 0 rgba(239, 68, 68, 0);
        }
    }

    /* تأثير النقر */
    .btn-clicked {
        animation: click-effect 0.3s ease;
    }

    @keyframes click-effect {
        0% {
            transform: scale(1);
        }
        50% {
            transform: scale(0.9);
        }
        100% {
            transform: scale(1);
        }
    }

    /* مؤشر التأكيد */
    .confirmation-indicator {
        position: absolute;
        top: -8px;
        right: -8px;
        background: white;
        color: var(--success-dark);
        border-radius: 50%;
        width: 24px;
        height: 24px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 14px;
        box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        z-index: 1;
        opacity: 0;
        transform: scale(0);
        transition: all 0.3s ease;
    }

    .confirmation-indicator.show {
        opacity: 1;
        transform: scale(1);
    }

    .confirmation-indicator.absent-check {
        color: var(--danger-dark);
    }

    /* =============== الملاحظات =============== */
    .notes-cell {
        min-width: 200px;
    }

    textarea {
        width: 100%;
        height: 80px;
        font-size: 16px;
        padding: 10px;
        border-radius: 8px;
        border: 1px solid #ccc;
        resize: vertical;
        font-family: "Cairo", sans-serif;
        line-height: 1.5;
        transition: all 0.3s ease;
    }

    textarea:focus {
        border-color: var(--primary-color);
        box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
        outline: none;
    }

    /* =============== زر الـ PDF =============== */
    .pdf-container {
        width: 95%;
        max-width: 1100px;
        margin: 25px auto;
    }

    #exportPDF {
        width: 100%;
        background: var(--purple-color);
        padding: 18px;
        border: none;
        border-radius: 12px;
        color: white;
        font-size: 20px;
        font-weight: 700;
        cursor: pointer;
        transition: .3s;
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 10px;
    }

    #exportPDF:hover {
        background: var(--purple-dark);
        transform: translateY(-3px);
        box-shadow: 0 8px 15px rgba(124, 58, 237, 0.3);
    }

    /* ============= الجوال - تصميم متجاوب ============= */
    @media (max-width: 768px) {
        header h1 {
            font-size: 20px;
        }
        
        header h2 {
            font-size: 15px;
        }
        
        .class-selector {
            gap: 5px;
            margin: 10px 5px;
        }
        
        .class-selector button {
            padding: 10px 8px;
            font-size: 15px;
            min-width: 60px;
        }
        
        .container, .date-container, .pdf-container {
            width: 98%;
            padding: 12px;
        }
        
        .date-row {
            flex-direction: column;
            gap: 15px;
        }
        
        .student-name {
            font-size: 16px;
            padding-right: 10px;
        }
        
        th, td {
            padding: 12px 8px;
            font-size: 15px;
        }
        
        .btn {
            padding: 10px 12px;
            font-size: 15px;
            min-width: 60px;
        }
        
        textarea {
            height: 70px;
            font-size: 15px;
        }
        
        #exportPDF {
            padding: 16px;
            font-size: 18px;
        }
        
        .confirmation-indicator {
            width: 20px;
            height: 20px;
            font-size: 12px;
            top: -6px;
            right: -6px;
        }
    }

    @media (max-width: 480px) {
        header {
            padding: 12px 8px;
        }
        
        header h1 {
            font-size: 18px;
        }
        
        header h2 {
            font-size: 14px;
        }
        
        .class-selector button {
            font-size: 14px;
            padding: 8px 6px;
            min-width: 55px;
        }
        
        .date-group label {
            font-size: 15px;
        }
        
        .date-input {
            padding: 12px;
            font-size: 15px;
        }
        
        .student-name {
            font-size: 15px;
        }
        
        .btn {
            padding: 8px 10px;
            font-size: 14px;
            min-width: 55px;
        }
        
        th, td {
            padding: 10px 6px;
            font-size: 14px;
        }
        
        textarea {
            height: 65px;
            font-size: 14px;
        }
        
        #exportPDF {
            font-size: 17px;
            padding: 15px;
        }
        
        .confirmation-indicator {
            width: 18px;
            height: 18px;
            font-size: 11px;
            top: -5px;
            right: -5px;
        }
    }

    /* تصميم خاص للشاشات الأفقية على الجوال */
    @media (max-height: 500px) and (orientation: landscape) {
        header {
            position: relative;
            padding: 10px;
        }
        
        .class-selector {
            margin: 10px 5px;
            padding: 5px 0;
        }
        
        .class-selector button {
            padding: 8px 6px;
            font-size: 14px;
        }
        
        .container {
            margin: 10px auto;
            padding: 10px;
        }
        
        textarea {
            height: 60px;
        }
    }

    /* تحسينات للوضع الداكن */
    @media (prefers-color-scheme: dark) {
        body {
            background: #1a1a1a;
            color: #f0f0f0;
        }
        
        .container, .date-container {
            background: #2d2d2d;
            color: #f0f0f0;
        }
        
        .date-input, textarea {
            background: #3d3d3d;
            color: #f0f0f0;
            border-color: #555;
        }
        
        .conversion-notice {
            background: #1e3a8a;
            border-color: #3b82f6;
            color: #dbeafe;
        }
        
        table {
            color: #f0f0f0;
        }
        
        th {
            background: #1e3a8a;
        }
        
        td {
            border-color: #444;
        }
        
        .confirmation-indicator {
            background: #1a1a1a;
        }
    }
</style>
</head>
<body>

<header>
    <h1>سجل متابعة الطلاب</h1>
    <h2>مادة اللغة الإنجليزية — المعلم: فهد الخالدي</h2>
</header>

<!-- اختيار الفصل -->
<div class="class-selector">
    <button onclick="showClass('c3_1')" class="active">٣/١</button>
    <button onclick="showClass('c2_3')">٢/٣</button>
    <button onclick="showClass('c3_3')">٣/٣</button>
    <button onclick="showClass('c4_3')">٤/٣</button>
    <button onclick="showClass('c5_3')">٥/٣</button>
</div>

<!-- التاريخ -->
<div class="date-container">
    <div class="date-row">
        <div class="date-group">
            <label for="gregorianDate">
                <i class="fas fa-calendar-alt"></i>
                التاريخ الميلادي:
            </label>
            <input type="date" id="gregorianDate" class="date-input">
            <div id="gregorianNotice" class="conversion-notice" style="display: none;">
                <i class="fas fa-sync-alt"></i>
                <span>سيتم تحويل التاريخ تلقائياً إلى الهجري</span>
            </div>
        </div>
        
        <div class="date-group">
            <label for="hijriDate">
                <i class="fas fa-moon"></i>
                التاريخ الهجري:
            </label>
            <input type="text" id="hijriDate" class="date-input" placeholder="يوم / شهر / سنة هـ (مثال: 15 / 9 / 1445)">
            <div id="hijriNotice" class="conversion-notice" style="display: none;">
                <i class="fas fa-sync-alt"></i>
                <span>سيتم تحويل التاريخ تلقائياً إلى الميلادي</span>
            </div>
        </div>
    </div>
    
    <div style="text-align: center; margin-top: 10px;">
        <button id="todayBtn" class="btn" style="background: var(--warning-color);">
            <i class="fas fa-calendar-day"></i> اليوم الحالي
        </button>
    </div>
</div>

<!-- 🔥 محتوى الفصول -->
<div id="classContent"></div>

<!-- PDF -->
<div class="pdf-container">
    <button id="exportPDF" onclick="generatePDF()">
        <span>📄</span>
        <span>استخراج تقرير PDF</span>
    </button>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<script>
// تهيئة مكتبة التواريخ الهجرية
moment.locale('ar');

/* ================== التحويل بين التواريخ ================== */
let isConverting = false;

// تحويل الميلادي إلى هجري
function convertToHijri(gregorianDate) {
    if (!gregorianDate || isConverting) return;
    
    isConverting = true;
    try {
        const m = moment(gregorianDate);
        const hijriDate = m.format('iD / iM / iYYYY هـ');
        document.getElementById('hijriDate').value = hijriDate;
        
        // إظهار إشعار التحويل
        const notice = document.getElementById('gregorianNotice');
        notice.style.display = 'flex';
        setTimeout(() => {
            notice.style.display = 'none';
        }, 3000);
        
        // حفظ التاريخ في localStorage
        saveDatesToStorage();
    } catch (error) {
        console.error('خطأ في التحويل إلى التاريخ الهجري:', error);
    } finally {
        setTimeout(() => { isConverting = false; }, 100);
    }
}

// تحويل الهجري إلى ميلادي
function convertToGregorian(hijriDateString) {
    if (!hijriDateString || isConverting) return;
    
    isConverting = true;
    try {
        // تنظيف النص من "هـ" والمسافات
        let cleaned = hijriDateString.replace(/هـ/g, '').trim();
        
        // البحث عن الأرقام في النص
        const numbers = cleaned.match(/\d+/g);
        
        if (numbers && numbers.length >= 3) {
            const day = parseInt(numbers[0]);
            const month = parseInt(numbers[1]);
            const year = parseInt(numbers[2]);
            
            // التحقق من صحة الأرقام
            if (day >= 1 && day <= 30 && month >= 1 && month <= 12 && year >= 1300 && year <= 1500) {
                // استخدام moment-hijri لإنشاء تاريخ هجري
                const hijriMoment = moment().iYear(year).iMonth(month - 1).iDate(day);
                
                // التحويل إلى ميلادي
                const gregorianDate = hijriMoment.format('YYYY-MM-DD');
                document.getElementById('gregorianDate').value = gregorianDate;
                
                // إظهار إشعار التحويل
                const notice = document.getElementById('hijriNotice');
                notice.style.display = 'flex';
                setTimeout(() => {
                    notice.style.display = 'none';
                }, 3000);
                
                // حفظ التاريخ في localStorage
                saveDatesToStorage();
            }
        }
    } catch (error) {
        console.error('خطأ في التحويل إلى التاريخ الميلادي:', error);
    } finally {
        setTimeout(() => { isConverting = false; }, 100);
    }
}

// تعيين التاريخ الحالي
function setToday() {
    const today = new Date();
    const formattedDate = today.toISOString().split('T')[0];
    document.getElementById('gregorianDate').value = formattedDate;
    convertToHijri(formattedDate);
}

// حفظ التواريخ في localStorage
function saveDatesToStorage() {
    const gregorianDate = document.getElementById('gregorianDate').value;
    const hijriDate = document.getElementById('hijriDate').value;
    
    if (gregorianDate && hijriDate) {
        localStorage.setItem('lastGregorianDate', gregorianDate);
        localStorage.setItem('lastHijriDate', hijriDate);
    }
}

// تحميل التواريخ المحفوظة
function loadDatesFromStorage() {
    const savedGregorian = localStorage.getItem('lastGregorianDate');
    const savedHijri = localStorage.getItem('lastHijriDate');
    
    if (savedGregorian) {
        document.getElementById('gregorianDate').value = savedGregorian;
    }
    
    if (savedHijri) {
        document.getElementById('hijriDate').value = savedHijri;
    } else if (savedGregorian) {
        // إذا كان هناك تاريخ ميلادي محفوظ ولكن لا يوجد هجري، قم بالتحويل
        convertToHijri(savedGregorian);
    }
}

/* ================== نظام عرض الفصول ================== */
function showClass(classId) {
    document.querySelectorAll(".class-selector button").forEach(btn => btn.classList.remove("active"));
    event.target.classList.add("active");
    
    loadClassStudents(classId);
}

/* ================== تحميل الطلاب ================== */
function loadClassStudents(classId) {
    const classes = {
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
            "حمد موسى ساليفو ديقوقa",
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

    let html = `
    <div class='container'>
        <h3 style="margin: 0 0 15px 0; text-align: center; color: var(--primary-dark);">الفصل ${classId.replace('c', '').replace('_', '/')}</h3>
        <div class="table-wrapper">
        <table>
            <thead>
                <tr>
                    <th>اسم الطالب</th>
                    <th>حضور</th>
                    <th>غياب</th>
                    <th class="notes-cell">ملاحظات</th>
                </tr>
            </thead>
            <tbody>`;

    classes[classId].forEach((name, index) => {
        html += `
                <tr>
                    <td class="student-name">${name}</td>
                    <td>
                        <div style="position: relative;">
                            <div class="confirmation-indicator" id="present-indicator-${classId}-${index}">
                                <i class="fas fa-check"></i>
                            </div>
                            <button class="btn present" onclick="toggleSelect(this,'present', '${classId}-${index}')">
                                <i class="fas fa-user-check"></i> حضور
                            </button>
                        </div>
                    </td>
                    <td>
                        <div style="position: relative;">
                            <div class="confirmation-indicator absent-check" id="absent-indicator-${classId}-${index}">
                                <i class="fas fa-times"></i>
                            </div>
                            <button class="btn absent" onclick="toggleSelect(this,'absent', '${classId}-${index}')">
                                <i class="fas fa-user-times"></i> غياب
                            </button>
                        </div>
                    </td>
                    <td class="notes-cell"><textarea placeholder="اكتب ملاحظاتك هنا..."></textarea></td>
                </tr>`;
    });

    html += `</tbody></table></div></div>`;
    document.getElementById("classContent").innerHTML = html;
}

/* ================== تفعيل/تعطيل الحضور والغياب مع تأثيرات ================== */
function toggleSelect(btn, type, indicatorId) {
    const row = btn.closest("tr");
    
    // تأثير النقر
    btn.classList.add('btn-clicked');
    setTimeout(() => {
        btn.classList.remove('btn-clicked');
    }, 300);
    
    // إزالة النشاط من جميع الأزرار في الصف
    row.querySelectorAll(".btn.present, .btn.absent")
        .forEach(b => b.classList.remove("active"));
    
    // إزالة جميع مؤشرات التأكيد في الصف
    const indicators = row.querySelectorAll(".confirmation-indicator");
    indicators.forEach(indicator => {
        indicator.classList.remove("show");
    });
    
    // تفعيل الزر المحدد
    btn.classList.add("active");
    
    // إظهار مؤشر التأكيد المناسب
    if (type === 'present') {
        const presentIndicator = document.getElementById(`present-indicator-${indicatorId}`);
        if (presentIndicator) {
            presentIndicator.classList.add("show");
            
            // إخفاء المؤشر بعد 3 ثواني (اختياري)
            setTimeout(() => {
                presentIndicator.classList.remove("show");
            }, 3000);
        }
        
        row.querySelector(".btn.absent").classList.remove("active");
    } else {
        const absentIndicator = document.getElementById(`absent-indicator-${indicatorId}`);
        if (absentIndicator) {
            absentIndicator.classList.add("show");
            
            // إخفاء المؤشر بعد 3 ثواني (اختياري)
            setTimeout(() => {
                absentIndicator.classList.remove("show");
            }, 3000);
        }
        
        row.querySelector(".btn.present").classList.remove("active");
    }
}

/* ================== استخراج PDF ================== */
function generatePDF() {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ 
        orientation: "p", 
        unit: "pt", 
        format: "a4",
        compress: true
    });
    
    // إضافة عنوان التقرير
    doc.setFontSize(22);
    doc.text("تقرير الغياب والحضور", 40, 40);
    
    doc.setFontSize(16);
    doc.text("مادة اللغة الإنجليزية", 40, 65);
    doc.text("إعداد: المعلم فهد الخالدي", 40, 85);
    
    // تاريخ التقرير
    const gregorianDate = document.getElementById("gregorianDate").value || "غير محدد";
    const hijriDate = document.getElementById("hijriDate").value || "غير محدد";
    
    doc.setFontSize(14);
    doc.text(`التاريخ الميلادي: ${gregorianDate}`, 40, 110);
    doc.text(`التاريخ الهجري: ${hijriDate}`, 40, 130);
    
    // بيانات الطلاب
    let y = 170;
    doc.setFontSize(12);
    
    document.querySelectorAll("table tbody tr").forEach((row, index) => {
        if (y > 700) {
            doc.addPage();
            y = 40;
        }
        
        const name = row.children[0].innerText;
        const present = row.children[1].querySelector("button").classList.contains("active") ? "✔" : "";
        const absent = row.children[2].querySelector("button").classList.contains("active") ? "✖" : "";
        const note = row.children[3].querySelector("textarea").value || "لا توجد ملاحظات";
        
        // اسم الطالب
        doc.setFont(undefined, 'bold');
        doc.text(`${index + 1}. ${name}`, 40, y);
        doc.setFont(undefined, 'normal');
        
        // حالة الحضور
        const status = present ? "حاضر" : (absent ? "غائب" : "لم يتم التحديد");
        const statusColor = present ? [16, 185, 129] : absent ? [239, 68, 68] : [107, 114, 128];
        doc.setTextColor(...statusColor);
        doc.text(`الحالة: ${status}`, 40, y + 20);
        doc.setTextColor(0, 0, 0);
        
        // الملاحظات
        const lines = doc.splitTextToSize(`ملاحظات: ${note}`, 450);
        doc.text(lines, 40, y + 40);
        
        y += 60 + (lines.length * 15);
    });
    
    // حفظ الملف
    doc.save("تقرير-الحضور-والغياب.pdf");
    
    // إظهار رسالة نجاح
    alert("تم استخراج التقرير بنجاح!");
}

/* ================== تهيئة الصفحة ================== */
window.onload = function() {
    // تحميل الفصل الأول
    showClass("c3_1");
    
    // تحميل التواريخ المحفوظة
    loadDatesFromStorage();
    
    // إذا لم تكن هناك تواريخ محفوظة، تعيين التاريخ الحالي
    if (!localStorage.getItem('lastGregorianDate')) {
        setToday();
    }
    
    // إضافة مستمعي الأحداث لحقول التاريخ
    document.getElementById('gregorianDate').addEventListener('change', function(e) {
        convertToHijri(e.target.value);
    });
    
    document.getElementById('hijriDate').addEventListener('input', function(e) {
        // استخدام debounce لمنع التحويل مع كل ضغطة زر
        clearTimeout(window.hijriTimeout);
        window.hijriTimeout = setTimeout(() => {
            convertToGregorian(e.target.value);
        }, 500);
    });
    
    document.getElementById('hijriDate').addEventListener('blur', function(e) {
        convertToGregorian(e.target.value);
    });
    
    // إضافة مستمع لزر اليوم الحالي
    document.getElementById('todayBtn').addEventListener('click', setToday);
    
    // إضافة تأثير عند النقر على زر اليوم
    document.getElementById('todayBtn').addEventListener('click', function() {
        this.classList.add('btn-clicked');
        setTimeout(() => {
            this.classList.remove('btn-clicked');
        }, 300);
    });
};
</script>

</body>
</html>
