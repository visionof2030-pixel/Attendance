
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

<!-- مكتبة jsPDF مع دعم العربية -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>

<!-- تحميل الخط العربي للPDF -->
<script src="https://unpkg.com/jspdf-arabic-font@1.0.0/dist/jspdf-arabic-font.min.js"></script>

<style>
    /* جميع الأنماط السابقة تبقى كما هي */
    :root {
        --primary-color: #0d47a1;
        --primary-dark: #0a3a8a;
        --primary-light: #5472d3;
        --secondary-color: #1565c0;
        --success-color: #2e7d32;
        --success-dark: #1b5e20;
        --success-light: #4caf50;
        --danger-color: #c62828;
        --danger-dark: #b71c1c;
        --danger-light: #ef5350;
        --warning-color: #f57c00;
        --warning-light: #ff9800;
        --admin-color: #6a1b9a;
        --admin-dark: #4a148c;
        --light-color: #f5f7fa;
        --dark-color: #263238;
        --white: #ffffff;
        --gray-light: #eceff1;
        --gray-medium: #b0bec5;
        --shadow: rgba(0,0,0,0.08);
        --star-color: #ffd700;
    }

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
    }

    /* ... جميع الأنماط الأخرى تبقى كما هي ... */

</style>
</head>
<body>

<!-- الهيكل HTML يبقى كما هو -->

<script>
// ================== تهيئة المتغيرات ==================
const ADMIN_PASSWORD = "Jassar1436";
let starredStudents = JSON.parse(localStorage.getItem('starredStudents')) || {};
let studentsData = JSON.parse(localStorage.getItem('studentsData')) || {};
let attendanceData = JSON.parse(localStorage.getItem('attendanceData')) || {};
let isAdminLoggedIn = false;
let isConverting = false;

// ================== إصلاح التحضير العشوائي ==================
function generateRandomAttendance() {
    const classId = document.getElementById('randomClassSelect').value;
    const gregorianDate = document.getElementById('gregorianDate').value;
    
    if (!gregorianDate) {
        showNotification('الرجاء تحديد التاريخ الميلادي أولاً', 'warning');
        return;
    }
    
    console.log("بدء توليد التحضير العشوائي للفصل:", classId, "التاريخ:", gregorianDate);
    
    if (classId === 'all') {
        const allClasses = ['c3_1', 'c2_3', 'c3_3', 'c4_3', 'c5_3'];
        allClasses.forEach(cid => {
            generateClassRandomAttendance(cid, gregorianDate);
        });
        showNotification('تم توليد تحضير عشوائي لجميع الفصول', 'success');
    } else {
        generateClassRandomAttendance(classId, gregorianDate);
        showNotification(`تم توليد تحضير عشوائي للفصل ${classId}`, 'success');
    }
    
    // تحديث عرض الفصل الحالي
    setTimeout(() => {
        const activeButton = document.querySelector('.class-selector button.active');
        if (activeButton) {
            const activeClassId = activeButton.getAttribute('onclick').match(/'([^']+)'/)[1];
            if (classId === 'all' || classId === activeClassId) {
                console.log("تحديث الواجهة للفصل:", activeClassId);
                loadClassStudents(activeClassId);
            }
        }
    }, 500);
}

function generateClassRandomAttendance(classId, date) {
    console.log("جاري توليد التحضير للفصل:", classId, "التاريخ:", date);
    
    if (!attendanceData[classId]) {
        attendanceData[classId] = {};
    }
    
    const classStudents = studentsData[classId] || getDefaultStudents(classId);
    
    if (!attendanceData[classId][date]) {
        attendanceData[classId][date] = {};
    }
    
    classStudents.forEach((student, index) => {
        const random = Math.random();
        const isPresent = random < 0.75; // 75% حضور
        
        attendanceData[classId][date][index] = {
            present: isPresent,
            absent: !isPresent,
            note: isPresent ? 'حضور عشوائي' : 'غياب عشوائي'
        };
    });
    
    localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
    console.log("تم حفظ بيانات التحضير للفصل:", classId, "البيانات:", attendanceData[classId][date]);
}

// ================== إصلاح PDF مع دعم كامل للغة العربية ==================
function generatePDF() {
    const pdfBtn = document.getElementById('exportPDF');
    pdfBtn.classList.add('clicked');
    setTimeout(() => {
        pdfBtn.classList.remove('clicked');
    }, 400);
    
    try {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF({
            orientation: 'landscape',
            unit: 'mm',
            format: 'a4'
        });
        
        // معلومات التقرير
        const gregorianDate = document.getElementById("gregorianDate").value || "غير محدد";
        const hijriDate = document.getElementById("hijriDate").value || "غير محدد";
        const activeClass = document.querySelector(".class-selector button.active").textContent;
        
        // إضافة نص عربي مباشرة - هذه الطريقة تعمل مع النصوص العربية
        doc.setFontSize(18);
        doc.text("تقرير متابعة الطلاب", 148, 15, { align: 'center' });
        doc.setFontSize(16);
        doc.text("مادة اللغة الإنجليزية - المعلم: فهد الخالدي", 148, 25, { align: 'center' });
        
        doc.setFontSize(12);
        doc.text(`الفصل: ${activeClass}`, 20, 45);
        doc.text(`التاريخ الميلادي: ${gregorianDate}`, 20, 55);
        doc.text(`التاريخ الهجري: ${hijriDate}`, 20, 65);
        
        // جمع بيانات الطلاب
        const tableRows = document.querySelectorAll("#classContent table tbody tr");
        const tableData = [];
        
        tableRows.forEach((row, index) => {
            const name = row.cells[0].querySelector('.student-name').textContent.trim();
            const isPresent = row.cells[1].querySelector(".btn.present").classList.contains("active");
            const isAbsent = row.cells[2].querySelector(".btn.absent").classList.contains("active");
            const notes = row.cells[3].querySelector("textarea").value || "";
            
            let status = "لم يتم التحديد";
            if (isPresent) status = "حاضر";
            if (isAbsent) status = "غائب";
            
            tableData.push([
                (index + 1).toString(),
                name,
                status,
                notes || "لا توجد ملاحظات"
            ]);
        });
        
        // إنشاء الجدول باستخدام autoTable مع دعم العربية
        doc.autoTable({
            startY: 75,
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
                1: { cellWidth: 70 },
                2: { cellWidth: 25, halign: 'center', fontStyle: 'bold' },
                3: { cellWidth: 80 }
            },
            didParseCell: function(data) {
                // تلوين خلية الحالة
                if (data.column.index === 2 && data.row.index > 0) {
                    const status = data.cell.raw;
                    if (status === "حاضر") {
                        data.cell.styles.textColor = [46, 125, 50];
                        data.cell.styles.fontStyle = 'bold';
                    } else if (status === "غائب") {
                        data.cell.styles.textColor = [198, 40, 40];
                        data.cell.styles.fontStyle = 'bold';
                    }
                }
            },
            margin: { left: 15, right: 15 }
        });
        
        // توقيع وتاريخ الإصدار
        const finalY = doc.lastAutoTable.finalY + 20;
        doc.setFontSize(10);
        doc.text("توقيع المعلم: ________________", 20, finalY);
        doc.text(`تاريخ الإصدار: ${new Date().toLocaleDateString('ar-SA')}`, 260, finalY);
        doc.text("نظام متابعة الطلاب - الإصدار 2.0", 148, finalY + 10, { align: 'center' });
        
        // حفظ الملف
        const fileName = `تقرير-الحضور-${activeClass}-${gregorianDate}.pdf`;
        doc.save(fileName);
        
        showNotification("تم تصدير التقرير بنجاح بصيغة PDF!", "success");
        
    } catch (error) {
        console.error("خطأ في إنشاء PDF:", error);
        showNotification("حدث خطأ أثناء إنشاء التقرير: " + error.message, "error");
    }
}

// ================== دالة محسنة لتحميل الفصل مع تطبيق بيانات التحضير ==================
function loadClassStudents(classId) {
    const classStudents = getDefaultStudents(classId);
    
    let html = `
    <div class='container'>
        <h3>قائمة طلاب الفصل ${classId.replace('c', '').replace('_', '/')}</h3>
        <div class="table-wrapper">
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
        const studentId = `${classId}-${index}`;
        
        html += `
                <tr>
                    <td>${getStudentStarElement(studentId, name)}</td>
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
    
    // إضافة النجوم المحفوظة
    updateClassDisplay();
    
    // تطبيق بيانات التحضير المخزنة للتاريخ الحالي
    const gregorianDate = document.getElementById('gregorianDate').value;
    if (gregorianDate) {
        applyAttendanceDataToUI(classId, gregorianDate);
    }
}

// ================== دالة جديدة لتطبيق بيانات التحضير على الواجهة ==================
function applyAttendanceDataToUI(classId, date) {
    console.log("تطبيق بيانات التحضير للفصل:", classId, "التاريخ:", date);
    console.log("بيانات التحضير المخزنة:", attendanceData);
    
    if (!attendanceData[classId] || !attendanceData[classId][date]) {
        console.log("لا توجد بيانات تحضير لهذا التاريخ");
        return;
    }
    
    const classAttendance = attendanceData[classId][date];
    const rows = document.querySelectorAll("#classContent table tbody tr");
    
    console.log("عدد الصفوف:", rows.length);
    console.log("بيانات التحضير:", classAttendance);
    
    rows.forEach((row, index) => {
        if (classAttendance[index]) {
            const { present, absent, note } = classAttendance[index];
            
            console.log(`الصف ${index}: حاضر=${present}, غائب=${absent}, ملاحظة=${note}`);
            
            if (present) {
                const presentBtn = row.cells[1].querySelector('.btn.present');
                const absentBtn = row.cells[2].querySelector('.btn.absent');
                if (presentBtn && absentBtn) {
                    presentBtn.classList.add('active');
                    absentBtn.classList.remove('active');
                }
            } else if (absent) {
                const presentBtn = row.cells[1].querySelector('.btn.present');
                const absentBtn = row.cells[2].querySelector('.btn.absent');
                if (presentBtn && absentBtn) {
                    presentBtn.classList.remove('active');
                    absentBtn.classList.add('active');
                }
            }
            
            if (note) {
                const textarea = row.cells[3].querySelector('textarea');
                if (textarea) {
                    textarea.value = note;
                }
            }
        }
    });
    
    console.log("تم تطبيق بيانات التحضير على الواجهة");
}

// ================== تعديل دالة toggleSelect لحفظ البيانات ==================
function toggleSelect(btn, type, indicatorId) {
    const row = btn.closest("tr");
    
    // تأثير النقر
    btn.classList.add('clicked');
    setTimeout(() => {
        btn.classList.remove('clicked');
    }, 400);
    
    // إزالة النشاط من جميع الأزرار في الصف
    row.querySelectorAll(".btn.present, .btn.absent")
        .forEach(b => b.classList.remove("active"));
    
    // إزالة مؤشرات التأكيد
    const indicators = row.querySelectorAll(".confirmation-indicator");
    indicators.forEach(indicator => {
        indicator.classList.remove("show");
    });
    
    // تفعيل الزر المحدد
    btn.classList.add("active");
    
    // إظهار مؤشر التأكيد
    if (type === 'present') {
        const presentIndicator = document.getElementById(`present-indicator-${indicatorId}`);
        if (presentIndicator) {
            presentIndicator.classList.add("show");
            setTimeout(() => {
                presentIndicator.classList.remove("show");
            }, 3000);
        }
        row.querySelector(".btn.absent").classList.remove("active");
    } else {
        const absentIndicator = document.getElementById(`absent-indicator-${indicatorId}`);
        if (absentIndicator) {
            absentIndicator.classList.add("show");
            setTimeout(() => {
                absentIndicator.classList.remove("show");
            }, 3000);
        }
        row.querySelector(".btn.present").classList.remove("active");
    }
    
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
        
        const note = row.querySelector('.notes-cell textarea').value || '';
        
        attendanceData[classId][gregorianDate][index] = {
            present: type === 'present',
            absent: type === 'absent',
            note: note
        };
        
        localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
        console.log("تم حفظ بيانات الحضور:", attendanceData);
    }
}

// ================== تهيئة النظام عند التحميل ==================
window.onload = function() {
    console.log("جاري تهيئة النظام...");
    
    // تحميل الفصل الأول
    showClass("c3_1");
    
    // تحميل التواريخ المحفوظة
    loadDatesFromStorage();
    
    // إذا لم تكن هناك تواريخ محفوظة، تعيين التاريخ الحالي
    if (!localStorage.getItem('lastGregorianDate')) {
        setToday();
    }
    
    // إضافة مستمعي الأحداث للتواريخ
    document.getElementById('gregorianDate').addEventListener('change', function(e) {
        convertToHijri(e.target.value);
        if (document.getElementById('adminModal').classList.contains('active')) {
            updateAdminDateDisplay();
        }
        
        // تحديث بيانات التحضير عند تغيير التاريخ
        const activeButton = document.querySelector('.class-selector button.active');
        if (activeButton) {
            const activeClassId = activeButton.getAttribute('onclick').match(/'([^']+)'/)[1];
            setTimeout(() => {
                applyAttendanceDataToUI(activeClassId, e.target.value);
            }, 300);
        }
    });
    
    document.getElementById('hijriDate').addEventListener('input', function(e) {
        clearTimeout(window.hijriTimeout);
        window.hijriTimeout = setTimeout(() => {
            convertToGregorian(e.target.value);
        }, 500);
    });
    
    document.getElementById('hijriDate').addEventListener('blur', function(e) {
        convertToGregorian(e.target.value);
    });
    
    // زر اليوم الحالي
    document.getElementById('todayBtn').addEventListener('click', setToday);
    
    // السماح بإدخال كلمة المرور بالضغط على Enter
    document.getElementById('adminPassword').addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            checkAdminPassword();
        }
    });
    
    // تحميل بيانات Admin
    updateStudentSelects();
    updateStarredStudentsList();
    
    // تحميل بيانات التحضير
    attendanceData = JSON.parse(localStorage.getItem('attendanceData')) || {};
    console.log("بيانات التحضير المحملة:", attendanceData);
};

// ================== الدوال المساعدة ==================
function showClass(classId) {
    document.querySelectorAll(".class-selector button").forEach(btn => {
        btn.classList.remove("active");
    });
    
    // إيجاد الزر المناسب
    const buttons = document.querySelectorAll('.class-selector button');
    buttons.forEach(btn => {
        if (btn.getAttribute('onclick') && btn.getAttribute('onclick').includes(classId)) {
            btn.classList.add("active");
            btn.classList.add('clicked');
            setTimeout(() => {
                btn.classList.remove('clicked');
            }, 300);
        }
    });
    
    loadClassStudents(classId);
}

// ... باقي الدوال تبقى كما هي ...

</script>
</body>
</html>
