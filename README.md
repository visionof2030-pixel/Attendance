<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>سجل حضور تجريبي</title>

<!-- خطوط عربية -->
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;700&display=swap" rel="stylesheet">

<!-- مكتبات PDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
    body {
        font-family: 'Tajawal', sans-serif;
        background: #f6f8fc;
        margin: 0;
        padding: 20px;
        direction: rtl;
    }
    .container {
        max-width: 700px;
        margin: auto;
        background: white;
        padding: 20px;
        border-radius: 10px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    }
    h2 {
        margin-top: 0;
        text-align: center;
        color: #1a73e8;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 15px;
    }
    th, td {
        border: 1px solid #ddd;
        padding: 10px;
        text-align: center;
    }
    th {
        background: #1a73e8;
        color: white;
    }
    button {
        padding: 10px 15px;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        font-weight: bold;
        margin: 5px;
    }
    .present { background: #34a853; color: white; }
    .absent { background: #ea4335; color: white; }
    .export { background: #1a73e8; color: white; width: 100%; }
</style>
</head>
<body>

<div class="container" id="captureArea">
    <h2>سجل حضور الطلاب - نسخة تجريبية</h2>

    <table>
        <thead>
            <tr>
                <th>م</th>
                <th>اسم الطالب</th>
                <th>الحضور</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>أحمد محمد</td>
                <td>
                    <button class="present" onclick="setStatus(this)">حاضر</button>
                    <button class="absent" onclick="setStatus(this)">غائب</button>
                </td>
            </tr>
            <tr>
                <td>2</td>
                <td>جسّار فهد</td>
                <td>
                    <button class="present" onclick="setStatus(this)">حاضر</button>
                    <button class="absent" onclick="setStatus(this)">غائب</button>
                </td>
            </tr>
        </tbody>
    </table>
</div>

<button class="export" onclick="exportPDF()">تصدير ملف PDF</button>

<script>
function setStatus(btn) {
    // إزالة التفعيل من الأزرار داخل نفس الخلية
    let parent = btn.parentElement.querySelectorAll("button");
    parent.forEach(b => b.style.opacity = "0.4");

    // تفعيل الزر المختار
    btn.style.opacity = "1";
}

async function exportPDF() {
    const { jsPDF } = window.jspdf;

    const element = document.getElementById("captureArea");
    const canvas = await html2canvas(element, { scale: 2 });

    const imgData = canvas.toDataURL("image/png");
    const pdf = new jsPDF("p", "mm", "a4");

    let width = pdf.internal.pageSize.getWidth();
    let height = (canvas.height * width) / canvas.width;

    pdf.addImage(imgData, "PNG", 0, 0, width, height);
    pdf.save("سجل-الحضور.pdf");
}
</script>

</body>
</html>
