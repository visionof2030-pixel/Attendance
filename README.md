<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>سجل حضور تجريبي</title>

<!-- خطوط عربية -->
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- مكتبات PDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
    :root {
        --primary-color: #2c5aa0;
        --present-color: #2e7d32;
        --absent-color: #c62828;
        --hover-present: #1b5e20;
        --hover-absent: #b71c1c;
        --light-bg: #f8f9fa;
        --border-color: #dee2e6;
    }
    
    body {
        font-family: 'Tajawal', sans-serif;
        background: linear-gradient(135deg, #f6f8fc 0%, #e9f2ff 100%);
        margin: 0;
        padding: 20px;
        direction: rtl;
        min-height: 100vh;
    }
    
    .container {
        max-width: 900px;
        margin: 20px auto;
        background: white;
        padding: 30px;
        border-radius: 20px;
        box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
        border: 1px solid var(--border-color);
    }
    
    h2 {
        margin-top: 0;
        text-align: center;
        color: var(--primary-color);
        font-size: 2.2rem;
        padding-bottom: 20px;
        border-bottom: 2px solid #f0f0f0;
        margin-bottom: 30px;
        position: relative;
    }
    
    h2:after {
        content: '';
        position: absolute;
        bottom: -2px;
        right: 50%;
        transform: translateX(50%);
        width: 150px;
        height: 4px;
        background: linear-gradient(to right, #2c5aa0, #4a8af4);
        border-radius: 4px;
    }
    
    table {
        width: 100%;
        border-collapse: separate;
        border-spacing: 0;
        margin-top: 15px;
        overflow: hidden;
        border-radius: 15px;
        box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
        font-size: 1.1rem;
    }
    
    th, td {
        padding: 20px 15px;
        text-align: center;
        border: 1px solid var(--border-color);
    }
    
    th {
        background: linear-gradient(to right, #2c5aa0, #3a6bc5);
        color: white;
        font-weight: 700;
        font-size: 1.3rem;
        letter-spacing: 1px;
    }
    
    tr:nth-child(even) {
        background-color: #f9f9f9;
    }
    
    tr:hover {
        background-color: #f0f7ff;
        transition: background-color 0.3s;
    }
    
    /* تصميم خلايا الحضور */
    .attendance-cell {
        text-align: center;
        padding: 15px;
    }
    
    .status-icon {
        font-size: 3rem;
        cursor: pointer;
        transition: all 0.3s ease;
        display: inline-block;
        padding: 10px;
        border-radius: 50%;
        width: 80px;
        height: 80px;
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0 auto;
    }
    
    .status-icon:hover {
        transform: scale(1.15);
        box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
    }
    
    .present-icon {
        color: var(--present-color);
        background-color: rgba(46, 125, 50, 0.1);
        border: 3px solid rgba(46, 125, 50, 0.3);
    }
    
    .present-icon:hover {
        background-color: rgba(46, 125, 50, 0.2);
        border-color: rgba(46, 125, 50, 0.5);
    }
    
    .absent-icon {
        color: var(--absent-color);
        background-color: rgba(198, 40, 40, 0.1);
        border: 3px solid rgba(198, 40, 40, 0.3);
    }
    
    .absent-icon:hover {
        background-color: rgba(198, 40, 40, 0.2);
        border-color: rgba(198, 40, 40, 0.5);
    }
    
    .status-label {
        display: block;
        margin-top: 10px;
        font-weight: 700;
        font-size: 1.2rem;
    }
    
    .present-label {
        color: var(--present-color);
    }
    
    .absent-label {
        color: var(--absent-color);
    }
    
    .status-controls {
        display: flex;
        justify-content: center;
        gap: 15px;
        margin-top: 20px;
        flex-wrap: wrap;
    }
    
    .status-btn {
        padding: 12px 25px;
        border: none;
        border-radius: 10px;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        font-size: 1.1rem;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        min-width: 150px;
        box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
    }
    
    .status-btn:hover {
        transform: translateY(-3px);
        box-shadow: 0 8px 15px rgba(0, 0, 0, 0.15);
    }
    
    .status-btn:active {
        transform: translateY(0);
        box-shadow: 0 3px 5px rgba(0, 0, 0, 0.1);
    }
    
    .present-btn {
        background: linear-gradient(to right, var(--present-color), #4caf50);
        color: white;
    }
    
    .present-btn:hover {
        background: linear-gradient(to right, var(--hover-present), #2e7d32);
    }
    
    .absent-btn {
        background: linear-gradient(to right, var(--absent-color), #f44336);
        color: white;
    }
    
    .absent-btn:hover {
        background: linear-gradient(to right, var(--hover-absent), #c62828);
    }
    
    .export-container {
        text-align: center;
        margin-top: 40px;
        padding-top: 30px;
        border-top: 2px dashed #ddd;
    }
    
    .export {
        background: linear-gradient(to right, #2c5aa0, #4a8af4);
        color: white;
        padding: 18px 35px;
        font-size: 1.3rem;
        border-radius: 12px;
        min-width: 280px;
        box-shadow: 0 8px 20px rgba(42, 91, 173, 0.3);
        border: none;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        transition: all 0.3s ease;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        gap: 15px;
    }
    
    .export:hover {
        background: linear-gradient(to right, #1e3f7a, #2c5aa0);
        box-shadow: 0 12px 25px rgba(42, 91, 173, 0.4);
        transform: translateY(-3px);
    }
    
    .footer-note {
        text-align: center;
        margin-top: 25px;
        color: #666;
        font-size: 1rem;
        padding: 15px;
        background-color: #f8f9fa;
        border-radius: 12px;
        border-right: 5px solid var(--primary-color);
        line-height: 1.6;
    }
    
    .student-name {
        font-size: 1.3rem;
        font-weight: 600;
    }
    
    /* تصميم متجاوب */
    @media (max-width: 768px) {
        .container {
            padding: 20px;
            margin: 10px;
        }
        
        h2 {
            font-size: 1.8rem;
        }
        
        .status-icon {
            font-size: 2.5rem;
            width: 70px;
            height: 70px;
        }
        
        table {
            font-size: 1rem;
        }
        
        th, td {
            padding: 15px 10px;
        }
        
        .export {
            min-width: 100%;
            padding: 16px;
        }
        
        .status-btn {
            min-width: 130px;
            padding: 10px 20px;
        }
    }
    
    @media (max-width: 480px) {
        .status-icon {
            font-size: 2rem;
            width: 60px;
            height: 60px;
        }
        
        th {
            font-size: 1.1rem;
        }
        
        .student-name {
            font-size: 1.1rem;
        }
    }
    
    /* أنيميشن للأيقونات */
    @keyframes pulse {
        0% { transform: scale(1); }
        50% { transform: scale(1.1); }
        100% { transform: scale(1); }
    }
    
    .status-icon.pulse {
        animation: pulse 0.5s ease;
    }
</style>
</head>
<body>

<div class="container" id="captureArea">
    <h2><i class="fas fa-users" style="margin-left: 15px;"></i> سجل حضور الطلاب - نظام الأيقونات</h2>

    <table>
        <thead>
            <tr>
                <th width="15%">الرقم</th>
                <th width="45%">اسم الطالب</th>
                <th width="40%">الحضور</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td class="student-name">أحمد محمد</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(1)" id="status-1">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-1">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>2</td>
                <td class="student-name">جسّار فهد</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(2)" id="status-2">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-2">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>3</td>
                <td class="student-name">سارة عبدالله</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(3)" id="status-3">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-3">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>4</td>
                <td class="student-name">يوسف خالد</td>
                <td class="attendance-cell">
                    <div class="status-icon absent-icon" onclick="toggleStatus(4)" id="status-4">
                        <i class="fas fa-times"></i>
                    </div>
                    <span class="status-label absent-label" id="label-4">غائب</span>
                </td>
            </tr>
            <tr>
                <td>5</td>
                <td class="student-name">نورة سعيد</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(5)" id="status-5">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-5">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>6</td>
                <td class="student-name">فارس علي</td>
                <td class="attendance-cell">
                    <div class="status-icon absent-icon" onclick="toggleStatus(6)" id="status-6">
                        <i class="fas fa-times"></i>
                    </div>
                    <span class="status-label absent-label" id="label-6">غائب</span>
                </td>
            </tr>
        </tbody>
    </table>
    
    <div class="status-controls">
        <button class="status-btn present-btn" onclick="setAllPresent()">
            <i class="fas fa-user-check"></i> تعيين الكل حاضر
        </button>
        <button class="status-btn absent-btn" onclick="setAllAbsent()">
            <i class="fas fa-user-slash"></i> تعيين الكل غائب
        </button>
    </div>
    
    <div class="footer-note">
        <i class="fas fa-info-circle" style="margin-left: 10px;"></i> انقر على أيقونة الحضور أو الغياب لتغيير حالة الطالب. يتم حفظ حالة الحضور فقط خلال هذه الجلسة. لاحتفاظ دائم، يرجى تصدير الملف كـ PDF.
    </div>
</div>

<div class="export-container">
    <button class="export" onclick="exportPDF()">
        <i class="fas fa-file-pdf"></i> تصدير سجل الحضور كملف PDF
    </button>
</div>

<script>
// تخزين حالة الطلاب
let studentsStatus = {
    1: 'present',
    2: 'present',
    3: 'present',
    4: 'absent',
    5: 'present',
    6: 'absent'
};

// تبديل حالة الطالب بين حاضر وغائب
function toggleStatus(studentId) {
    const statusIcon = document.getElementById(`status-${studentId}`);
    const statusLabel = document.getElementById(`label-${studentId}`);
    
    // إضافة تأثير النبض
    statusIcon.classList.add('pulse');
    setTimeout(() => {
        statusIcon.classList.remove('pulse');
    }, 500);
    
    // تبديل الحالة
    if (studentsStatus[studentId] === 'present') {
        // تغيير إلى غائب
        studentsStatus[studentId] = 'absent';
        statusIcon.className = 'status-icon absent-icon';
        statusIcon.innerHTML = '<i class="fas fa-times"></i>';
        statusLabel.className = 'status-label absent-label';
        statusLabel.textContent = 'غائب';
        
        // إشعار بصري
        showNotification(`تم تغيير حالة الطالب ${studentId} إلى غائب`, 'absent');
    } else {
        // تغيير إلى حاضر
        studentsStatus[studentId] = 'present';
        statusIcon.className = 'status-icon present-icon';
        statusIcon.innerHTML = '<i class="fas fa-check"></i>';
        statusLabel.className = 'status-label present-label';
        statusLabel.textContent = 'حاضر';
        
        // إشعار بصري
        showNotification(`تم تغيير حالة الطالب ${studentId} إلى حاضر`, 'present');
    }
}

// تعيين جميع الطلاب كحاضرين
function setAllPresent() {
    for (let i = 1; i <= 6; i++) {
        studentsStatus[i] = 'present';
        const statusIcon = document.getElementById(`status-${i}`);
        const statusLabel = document.getElementById(`label-${i}`);
        
        statusIcon.className = 'status-icon present-icon';
        statusIcon.innerHTML = '<i class="fas fa-check"></i>';
        statusLabel.className = 'status-label present-label';
        statusLabel.textContent = 'حاضر';
        
        // تأثير لكل أيقونة
        setTimeout(() => {
            statusIcon.classList.add('pulse');
            setTimeout(() => {
                statusIcon.classList.remove('pulse');
            }, 500);
        }, i * 100);
    }
    
    showNotification('تم تعيين جميع الطلاب كحاضرين', 'present');
}

// تعيين جميع الطلاب كغائبين
function setAllAbsent() {
    for (let i = 1; i <= 6; i++) {
        studentsStatus[i] = 'absent';
        const statusIcon = document.getElementById(`status-${i}`);
        const statusLabel = document.getElementById(`label-${i}`);
        
        statusIcon.className = 'status-icon absent-icon';
        statusIcon.innerHTML = '<i class="fas fa-times"></i>';
        statusLabel.className = 'status-label absent-label';
        statusLabel.textContent = 'غائب';
        
        // تأثير لكل أيقونة
        setTimeout(() => {
            statusIcon.classList.add('pulse');
            setTimeout(() => {
                statusIcon.classList.remove('pulse');
            }, 500);
        }, i * 100);
    }
    
    showNotification('تم تعيين جميع الطلاب كغائبين', 'absent');
}

// إظهار إشعار مؤقت
function showNotification(message, type) {
    // إزالة أي إشعارات سابقة
    const existingNotification = document.querySelector('.custom-notification');
    if (existingNotification) {
        existingNotification.remove();
    }
    
    // إنشاء إشعار جديد
    const notification = document.createElement('div');
    notification.className = 'custom-notification';
    notification.textContent = message;
    notification.style.cssText = `
        position: fixed;
        top: 25px;
        right: 25px;
        background: ${type === 'present' ? 'var(--present-color)' : 'var(--absent-color)'};
        color: white;
        padding: 15px 25px;
        border-radius: 10px;
        z-index: 1000;
        font-weight: bold;
        box-shadow: 0 8px 20px rgba(0,0,0,0.2);
        animation: fadeInOut 3s ease-in-out;
        font-size: 1.1rem;
        display: flex;
        align-items: center;
        gap: 10px;
    `;
    
    // إضافة أيقونة للإشعار
    const icon = document.createElement('i');
    icon.className = type === 'present' ? 'fas fa-check-circle' : 'fas fa-times-circle';
    notification.prepend(icon);
    
    document.body.appendChild(notification);
    
    // إزالة الإشعار بعد 3 ثوان
    setTimeout(() => {
        notification.style.animation = 'fadeOut 0.5s ease-out forwards';
        setTimeout(() => {
            if (notification.parentNode) {
                notification.parentNode.removeChild(notification);
            }
        }, 500);
    }, 2500);
}

// إضافة أنيميشن للإشعارات
const style = document.createElement('style');
style.textContent = `
    @keyframes fadeInOut {
        0% { opacity: 0; transform: translateY(-25px); }
        15% { opacity: 1; transform: translateY(0); }
        85% { opacity: 1; transform: translateY(0); }
        100% { opacity: 0; transform: translateY(-25px); }
    }
    @keyframes fadeOut {
        from { opacity: 1; }
        to { opacity: 0; }
    }
`;
document.head.appendChild(style);

// دالة تصدير PDF - محفوظة كما هي بدون تغيير
async function exportPDF() {
    const { jsPDF } = window.jspdf;
    
    // تغيير نص زر التصدير مؤقتًا
    const exportBtn = document.querySelector('.export');
    const originalText = exportBtn.innerHTML;
    exportBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> جاري التصدير...';
    exportBtn.disabled = true;
    
    // إضافة تاريخ التصدير
    const dateElement = document.createElement('div');
    dateElement.style.cssText = 'text-align: left; margin-bottom: 20px; color: #666; font-size: 1rem; padding: 10px 15px; background: #f8f9fa; border-radius: 8px; border-right: 4px solid #2c5aa0;';
    dateElement.innerHTML = `<i class="far fa-calendar-alt" style="margin-left: 8px;"></i> تاريخ التصدير: ${new Date().toLocaleDateString('ar-SA')} - <i class="far fa-clock" style="margin-left: 8px;"></i> الوقت: ${new Date().toLocaleTimeString('ar-SA', {hour: '2-digit', minute:'2-digit'})}`;
    
    const captureArea = document.getElementById('captureArea');
    const originalContent = captureArea.innerHTML;
    
    // إضافة التاريخ أعلى المحتوى
    captureArea.insertBefore(dateElement, captureArea.firstChild);
    
    const canvas = await html2canvas(captureArea, { 
        scale: 2,
        useCORS: true,
        backgroundColor: '#ffffff'
    });

    // إعادة المحتوى الأصلي
    captureArea.removeChild(dateElement);
    
    const imgData = canvas.toDataURL("image/png");
    const pdf = new jsPDF("p", "mm", "a4");

    let width = pdf.internal.pageSize.getWidth();
    let height = (canvas.height * width) / canvas.width;

    // إذا كان المحتوى طويلاً جداً، نقسمه على صفحات
    if (height > pdf.internal.pageSize.getHeight()) {
        let position = 0;
        const pageHeight = pdf.internal.pageSize.getHeight();
        
        while (height > 0) {
            pdf.addImage(imgData, "PNG", 0, position, width, height);
            height -= pageHeight;
            position -= pageHeight;
            
            if (height > 0) {
                pdf.addPage();
            }
        }
    } else {
        pdf.addImage(imgData, "PNG", 0, 0, width, height);
    }
    
    pdf.save(`سجل-الحضور-${new Date().toISOString().slice(0,10)}.pdf`);
    
    // إعادة زر التصدير إلى وضعه الأصلي
    exportBtn.innerHTML = originalText;
    exportBtn.disabled = false;
    
    // إظهار إشعار نجاح
    showNotification('تم تصدير ملف PDF بنجاح!', 'present');
}
</script>

</body>
</html>
