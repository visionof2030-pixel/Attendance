<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>سجل حضور بالرموز</title>

<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
    body {
        font-family: 'Tajawal', sans-serif;
        background: #f0f2f7;
        padding: 20px;
    }
    .container {
        max-width: 750px;
        margin: auto;
        background: #fff;
        padding: 18px;
        border-radius: 10px;
        box-shadow: 0 6px 14px rgba(0,0,0,0.08);
    }
    h2 {
        text-align: center;
        color: #1a73e8;
        margin-top: 0;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 15px;
    }
    th, td {
        border: 1px solid #ddd;
        padding: 12px;
        text-align: center;
    }
    th {
        background: #1a73e8;
        color: white;
    }

    /* أزرار الحضور */
    button {
        padding: 8px 12px;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        font-weight: bold;
        transition: 0.15s;
        display: inline-flex;
        align-items: center;
        gap: 6px;
        font-size: 15px;
    }

    .present { background: #34a853; color: white; }
    .absent { background: #ea4335; color: white; }

    .activeBtn {
        transform: scale(1.08);
        box-shadow: 0 0 12px rgba(0,0,0,0.18);
    }

    /* ألوان الصفوف */
    .row-present { background: #e8f5e9 !important; }
    .row-absent { background: #ffebee !important; }

    .export {
        width: 100%;
        background: #1a73e8;
        color: white;
        margin-top: 18px;
        font-size: 16px;
        padding: 12px;
    }
</style>
</head>
<body>

<div class="container">
    <h2>سجل حضور الطلاب</h2>

    <table id="studentsTable">
        <thead>
            <tr>
                <th>م</th>
                <th>اسم الطالب</th>
                <th>الحالة</th>
            </tr>
        </thead>

        <tbody>

            <tr data-status="">
                <td>1</td>
                <td>أحمد محمد</td>
                <td>
                    <button class="present" onclick="setStatus(this,'حاضر','✔️')">✔️ حاضر</button>
                    <button class="absent" onclick="setStatus(this,'غائب','❌')">❌ غائب</button>
                </td>
            </tr>

            <tr data-status="">
                <td>2</td>
                <td>جسّار فهد</td>
                <td>
                    <button class="present" onclick="setStatus(this,'حاضر','✔️')">✔️ حاضر</button>
                    <button class="absent" onclick="setStatus(this,'غائب','❌')">❌ غائب</button>
                </td>
            </tr>

        </tbody>
    </table>
</div>

<button class="export" onclick="exportPDF()">تصدير PDF</button>

<script>
function setStatus(btn, status, symbol) {

    let row = btn.closest("tr");
    let buttons = row.querySelectorAll("button");

    // إزالة تنشيط باقي الأزرار
    buttons.forEach(b => b.classList.remove("activeBtn"));

    // تفعيل الزر الحالي
    btn.classList.add("activeBtn");

    // حفظ النتيجة داخل الصف
    row.dataset.status = status;
    row.dataset.symbol = symbol;

    // تغيير لون الصف
    row.classList.remove("row-present", "row-absent");

    if (status === "حاضر") {
        row.classList.add("row-present");
    } else {
        row.classList.add("row-absent");
    }
}

function exportPDF() {
    const { jsPDF } = window.jspdf;
    let pdf = new jsPDF("p", "mm", "a4");

    pdf.setFontSize(16);
    pdf.text("تقرير حضور الطلاب", 105, 15, { align: "center" });

    pdf.setFontSize(13);

    let y = 30;
    const rows = document.querySelectorAll("#studentsTable tbody tr");

    rows.forEach((row, index) => {
        let name = row.children[1].innerText;
        let status = row.dataset.status || "—";
        let symbol = row.dataset.symbol || "";

        pdf.text(`${index + 1}- ${name} : ${symbol} ${status}`, 12, y);
        y += 10;
    });

    pdf.save("تقرير-الحضور.pdf");
}
</script>

</body>
</html>
