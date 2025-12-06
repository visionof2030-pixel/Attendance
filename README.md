<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سجل متابعة الطلاب - 5 صفوف</title>
<style>
body {
    font-family: "Tajawal", sans-serif;
    margin: 0;
    padding: 0;
    background: #f7f7f7;
}

header {
    background: linear-gradient(135deg, #6ec9ff, #2a9d8f);
    color: #fff;
    text-align: center;
    padding: 15px 0;
    font-size: 22px;
    font-weight: bold;
    box-shadow: 0px 4px 6px rgba(0,0,0,0.1);
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
    background: #6ec9ff;
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
    border: 1px solid #6ec9ff;
    border-radius: 10px;
    background: #e0f2ff;
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

input[type="password"] {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
    width: 200px;
    margin-left: 10px;
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

@media print {
    button, .admin-panel, .status-filter, .class-tabs {
        display: none !important;
    }
    
    table {
        font-size: 10px;
    }
}
</style>
</head>
<body>

<header>سجل متابعة الطلاب - 5 صفوف دراسية</header>

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
        <input type="password" id="adminPass" placeholder="ادخل كلمة المرور للإدارة">
        <button onclick="checkAdmin()">🔓 فتح الإدارة</button>
    </div>

    <div class="admin-panel" id="adminPanel">
        <h3 style="text-align:center; margin-top:0;">لوحة الإدارة</h3>
        <div style="text-align:center;">
            <button onclick="addStudent()">➕ إضافة طالب</button>
            <button onclick="randomAttendance()">🎲 تحضير عشوائي</button>
            <button onclick="moveStudent()">↔️ نقل طالب</button>
            <button onclick="resetAll()">🔄 إعادة تعيين</button>
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

// تهيئة الصفحة
function initPage() {
    createClassTabs();
    createTables();
    updateStudentCount();
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
}

// تبديل النجمة
function toggleStar(cell) {
    if (adminActive) {
        cell.innerHTML = cell.innerHTML === "☆" ? "⭐" : "☆";
    } else {
        alert('يجب تفعيل وضع الإدارة أولا');
    }
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
    
    alert("تمت إعادة التعيين بنجاح");
}

// تصدير إلى Excel
function exportToExcel() {
    let tablesHTML = "";
    
    for (const className in studentsData) {
        tablesHTML += `<h2>الصف ${className}</h2>`;
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
    link.download = "تقرير_الطلاب.xls";
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
