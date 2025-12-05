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
        max-width: 800px;
        margin: 20px auto;
        background: white;
        padding: 25px;
        border-radius: 15px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
        border: 1px solid var(--border-color);
    }
    
    h2 {
        margin-top: 0;
        text-align: center;
        color: var(--primary-color);
        font-size: 1.8rem;
        padding-bottom: 15px;
        border-bottom: 2px solid #f0f0f0;
        margin-bottom: 25px;
        position: relative;
    }
    
    h2:after {
        content: '';
        position: absolute;
        bottom: -2px;
        right: 50%;
        transform: translateX(50%);
        width: 100px;
        height: 3px;
        background: linear-gradient(to right, #2c5aa0, #4a8af4);
        border-radius: 3px;
    }
    
    table {
        width: 100%;
        border-collapse: separate;
        border-spacing: 0;
        margin-top: 15px;
        overflow: hidden;
        border-radius: 10px;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
    }
    
    th, td {
        padding: 15px 12px;
        text-align: center;
        border: 1px solid var(--border-color);
    }
    
    th {
        background: linear-gradient(to right, #2c5aa0, #3a6bc5);
        color: white;
        font-weight: 700;
        font-size: 1.1rem;
    }
    
    tr:nth-child(even) {
        background-color: #f9f9f9;
    }
    
    tr:hover {
        background-color: #f0f7ff;
        transition: background-color 0.2s;
    }
    
    /* تحسين تصميم الأزرار */
    .btn-group {
        display: flex;
        justify-content: center;
        gap: 8px;
    }
    
    .btn {
        padding: 10px 18px;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        font-weight: 600;
        font-family: 'Tajawal', sans-serif;
        font-size: 0.95rem;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 6px;
        min-width: 100px;
        box-shadow: 0 3px 5px rgba(0, 0, 0, 0.1);
    }
    
    .btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 5px 10px rgba(0, 0, 0, 0.15);
    }
    
    .btn:active {
        transform: translateY(0);
        box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1);
    }
    
    .present {
        background: linear-gradient(to right, var(--present-color), #4caf50);
        color: white;
    }
    
    .present:hover {
        background: linear-gradient(to right, var(--hover-present), #2e7d32);
    }
    
    .absent {
        background: linear-gradient(to right, var(--absent-color), #f44336);
        color: white;
    }
    
    .absent:hover {
        background: linear-gradient(to right, var(--hover-absent), #c62828);
    }
    
    .export-container {
        text-align: center;
        margin-top: 30px;
        padding-top: 20px;
        border-top: 1px dashed #ddd;
    }
    
    .export {
        background: linear-gradient(to right, #2c5aa0, #4a8af4);
        color: white;
        padding: 14px 28px;
        font-size: 1.1rem;
        border-radius: 10px;
        min-width: 200px;
        box-shadow: 0 5px 15px rgba(42, 91, 173, 0.3);
    }
    
    .export:hover {
        background: linear-gradient(to right, #1e3f7a, #2c5aa0);
        box-shadow: 0 7px 18px rgba(42, 91, 173, 0.4);
    }
    
    .status-indicator {
        display: inline-block;
        width: 10px;
        height: 10px;
        border-radius: 50%;
        margin-left: 8px;
        vertical-align: middle;
    }
    
    .status-present {
        background-color: var(--present-color);
    }
    
    .status-absent {
        background-color: var(--absent-color);
    }
    
    /* تحسين مظهر حالة التحديد */
    .selected {
        box-shadow: 0 0 0 3px rgba(46, 125, 50, 0.3);
        font-weight: 700;
    }
    
    .selected.absent {
        box-shadow: 0 0 0 3px rgba(198, 40, 40, 0.3);
    }
    
    .footer-note {
        text-align: center;
        margin-top: 15px;
        color: #666;
        font-size: 0.9rem;
        padding: 10px;
        background-color: #f8f9fa;
        border-radius: 8px;
        border-right: 4px solid var(--primary-color);
    }
    
    /* تصميم متجاوب */
    @media (max-width: 768px) {
        .container {
            padding: 15px;
            margin: 10px;
        }
        
        .btn-group {
            flex-direction: column;
            align-items: center;
        }
        
        .btn {
            width: 100%;
            max-width: 180px;
        }
        
        table {
            font-size: 0.9rem;
        }
        
        th, td {
            padding: 10px 8px;
        }
    }
</style>
</head>
<body>

<div class="container" id="captureArea">
    <h2><i class="fas fa-users" style="margin-left: 10px;"></i> سجل حضور الطلاب - نسخة مطورة</h2>

    <table>
        <thead>
            <tr>
                <th width="15%">الرقم</th>
                <th width="50%">اسم الطالب</th>
                <th width="35%">الحضور</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>أحمد محمد</td>
                <td>
                    <div class="btn-group">
                        <button class="btn present" onclick="setStatus(this, 1)">
                            <i class="fas fa-check"></i> حاضر
                        </button>
                        <button class="btn absent" onclick="setStatus(this, 1)">
                            <i class="fas fa-times"></i> غائب
                        </button>
                    </div>
                </td>
            </tr>
            <tr>
                <td>2</td>
                <td>جسّار فهد</td>
                <td>
                    <div class="btn-group">
                        <button class="btn present" onclick="setStatus(this, 2)">
                            <i class="fas fa-check"></i> حاضر
                        </button>
                        <button class="btn absent" onclick="setStatus(this, 2)">
                            <i class="fas fa-times"></i> غائب
                        </button>
                    </div>
                </td>
            </tr>
            <tr>
                <td>3</td>
                <td>سارة عبدالله</td>
                <td>
                    <div class="btn-group">
                        <button class="btn present" onclick="setStatus(this, 3)">
                            <i class="fas fa-check"></i> حاضر
                        </button>
                        <button class="btn absent" onclick="setStatus(this, 3)">
                            <i class="fas fa-times"></i> غائب
                        </button>
                    </div>
                </td>
            </tr>
            <tr>
                <td>4</td>
                <td>يوسف خالد</td>
                <td>
                    <div class="btn-group">
                        <button class="btn present" onclick="setStatus(this, 4)">
                            <i class="fas fa-check"></i> حاضر
                        </button>
                        <button class="btn absent" onclick="setStatus(this, 4)">
                            <i class="fas fa-times"></i> غائب
                        </button>
                    </div>
                </td>
            </tr>
        </tbody>
    </table>
    
    <div class="footer-note">
        <i class="fas fa-info-circle"></i> يتم حفظ حالة الحضور فقط خلال هذه الجلسة. لاحتفاظ دائم، يرجى تصدير الملف كـ PDF.
    </div>
</div>

<div class="export-container">
    <button class="btn export" onclick="exportPDF()">
        <i class="fas fa-file-pdf"></i> تصدير سجل الحضور كملف PDF
    </button>
</div>

<script>
// تخزين حالة الطلاب
let studentsStatus = {
    1: null,
    2: null,
    3: null,
    4: null
};

function setStatus(btn, studentId) {
    // تحديد نوع الزر (حاضر أم غائب)
    const isPresent = btn.classList.contains('present');
    
    // إزالة التحديد من جميع أزرار هذا الطالب
    const parent = btn.parentElement;
    const allButtons = parent.querySelectorAll("button");
    allButtons.forEach(b => {
        b.classList.remove('selected');
        b.style.opacity = "0.7";
    });
    
    // تطبيق التحديد على الزر المختار
    btn.classList.add('selected');
    btn.style.opacity = "1";
    
    // حفظ الحالة
    studentsStatus[studentId] = isPresent ? 'present' : 'absent';
    
    // إظهار إشعار بصري بسيط
    showStatusNotification(isPresent ? 'حاضر' : 'غائب', studentId);
}

function showStatusNotification(status, studentId) {
    // إنشاء عنصر الإشعار المؤقت
    const notification = document.createElement('div');
    notification.textContent = `تم تسجيل حالة الطالب ${studentId} كـ ${status}`;
    notification.style.cssText = `
        position: fixed;
        top: 20px;
        right: 20px;
        background: ${status === 'حاضر' ? 'var(--present-color)' : 'var(--absent-color)'};
        color: white;
        padding: 12px 20px;
        border-radius: 8px;
        z-index: 1000;
        font-weight: bold;
        box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        animation: fadeInOut 2.5s ease-in-out;
    `;
    
    document.body.appendChild(notification);
    
    // إزالة الإشعار بعد 2.5 ثانية
    setTimeout(() => {
        notification.style.animation = 'fadeOut 0.5s ease-out forwards';
        setTimeout(() => {
            if (notification.parentNode) {
                notification.parentNode.removeChild(notification);
            }
        }, 500);
    }, 2000);
}

// إضافة أنيميشن للإشعارات
const style = document.createElement('style');
style.textContent = `
    @keyframes fadeInOut {
        0% { opacity: 0; transform: translateY(-20px); }
        15% { opacity: 1; transform: translateY(0); }
        85% { opacity: 1; transform: translateY(0); }
        100% { opacity: 0; transform: translateY(-20px); }
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
    dateElement.style.cssText = 'text-align: left; margin-bottom: 15px; color: #666; font-size: 0.9rem;';
    dateElement.innerHTML = `<i class="far fa-calendar-alt"></i> تاريخ التصدير: ${new Date().toLocaleDateString('ar-SA')}`;
    
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
    showExportNotification();
}

function showExportNotification() {
    const notification = document.createElement('div');
    notification.textContent = 'تم تصدير ملف PDF بنجاح!';
    notification.style.cssText = `
        position: fixed;
        top: 20px;
        left: 20px;
        background: var(--present-color);
        color: white;
        padding: 12px 20px;
        border-radius: 8px;
        z-index: 1000;
        font-weight: bold;
        box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        animation: fadeInOut 2.5s ease-in-out;
    `;
    
    document.body.appendChild(notification);
    
    setTimeout(() => {
        notification.style.animation = 'fadeOut 0.5s ease-out forwards';
        setTimeout(() => {
            if (notification.parentNode) {
                notification.parentNode.removeChild(notification);
            }
        }, 500);
    }, 2000);
}
</script>

</body>
</html>
