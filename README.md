<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>سجل متابعة الحضور - اللغة العربية</title>

<!-- خطوط عربية جميلة -->
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@300;400;500;700&family=Amiri:wght@400;700&display=swap" rel="stylesheet">

<!-- أيقونات -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- مكتبات: html2canvas + jsPDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
  :root{
    --primary:#1a73e8;
    --dark:#202124;
    --light:#f8f9fa;
    --success:#34a853;
    --danger:#ea4335;
    --card-bg:#fff;
    --muted:#666;
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    font-family:'Tajawal', 'Amiri', sans-serif;
    background:linear-gradient(180deg,#eef3ff 0%, #f7f9fc 100%);
    margin:0;
    color:var(--dark);
    direction:rtl;
    -webkit-font-smoothing:antialiased;
    padding-bottom:40px;
  }

  .container{
    max-width:1100px;
    margin:24px auto;
    padding:18px;
  }

  .header{
    background:linear-gradient(135deg,var(--primary), #0d47a1);
    color:white;
    padding:18px;
    border-radius:12px;
    box-shadow:0 6px 18px rgba(26,115,232,0.18);
    display:flex;
    gap:16px;
    align-items:center;
    justify-content:space-between;
    flex-wrap:wrap;
  }
  .header .title{
    display:flex;
    gap:12px;
    align-items:center;
  }
  .logo{
    width:56px;height:56px;border-radius:10px;background:white;color:var(--primary);display:flex;align-items:center;justify-content:center;font-size:28px;font-weight:700;
  }
  .header h1{font-size:18px;margin:0}
  .header p{margin:0;opacity:.95}

  .controls{
    display:flex;gap:8px;align-items:center;flex-wrap:wrap;
  }
  .date-input, .hijri-input {
    padding:8px 10px;border-radius:8px;border:1px solid #e0e6f0;background:white;text-align:center;font-size:14px;
  }
  .btn{
    padding:10px 14px;border-radius:8px;border:none;cursor:pointer;font-weight:700;
  }
  .btn-primary{background:var(--primary);color:white}
  .btn-warning{background:#fbbc04;color:#222}
  .btn-danger{background:var(--danger);color:white}

  .main{
    margin-top:18px;display:grid;grid-template-columns:1fr 360px;gap:18px;
  }

  /* بطاقة الجدول */
  .card{
    background:var(--card-bg);border-radius:10px;padding:14px;box-shadow:0 6px 18px rgba(0,0,0,0.06);
  }

  .students-card{
    overflow:auto;
  }

  .section-title{font-size:16px;color:var(--primary);margin-bottom:12px;font-weight:700}

  table{
    width:100%;border-collapse:collapse;font-size:14px;
    min-width:600px;
  }
  th,td{padding:10px;border-bottom:1px solid #edf0f7;text-align:center;vertical-align:middle}
  th{background:linear-gradient(90deg,var(--primary), #0d47a1);color:white;font-weight:700;position:sticky;top:0}
  td.name{ text-align:right;font-weight:700; padding-right:14px; word-break:keep-all; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }

  .attendance-buttons{display:flex;gap:8px;justify-content:center}
  .attendance-btn{padding:8px 10px;border-radius:8px;border:none;cursor:pointer;font-weight:700;min-width:80px}
  .present{background:var(--success);color:white}
  .absent{background:var(--danger);color:white}
  .attendance-btn.active{box-shadow:0 6px 18px rgba(0,0,0,0.12);transform:translateY(-2px)}

  textarea.notes{width:100%;min-height:64px;border-radius:8px;border:1px solid #e6e9f2;padding:8px;font-family:'Amiri', serif;resize:vertical}

  /* لوحة جانبية للادمن */
  .sidebar{height:100%;}
  .sidebar .small{font-size:13px;color:var(--muted);margin-bottom:8px}
  .student-list{max-height:220px;overflow:auto;border-radius:8px;padding:8px;border:1px solid #f0f2f8;background:#fff}
  .student-item{display:flex;justify-content:space-between;align-items:center;padding:8px;border-bottom:1px solid #f5f6fb}
  .student-item:last-child{border-bottom:none}
  .small-input{width:100%;padding:8px;border-radius:8px;border:1px solid #e6e9f2;margin-bottom:8px;text-align:center}

  .admin-actions{display:flex;gap:8px;margin-top:8px}
  .muted-note{font-size:13px;color:#7b7f88;margin-top:8px}

  /* إشعار بسيط */
  .toast{position:fixed;left:50%;transform:translateX(50%) translateY(-200px);top:18px;background:var(--success);color:#fff;padding:10px 14px;border-radius:8px;z-index:2000;transition:transform .35s}
  .toast.show{transform:translateX(50%) translateY(0)}

  /* تجاوب */
  @media(max-width:980px){
    .main{grid-template-columns:1fr; }
    table{min-width:unset}
  }

</style>
</head>
<body>
  <div class="container">
    <div class="header">
      <div class="title">
        <div class="logo"><i class="fas fa-book-open"></i></div>
        <div>
          <h1>سجل متابعة الحضور الإلكتروني</h1>
          <p>قابل للطباعة والتصدير إلى PDF باللغة العربية</p>
        </div>
      </div>

      <div class="controls" aria-hidden="false">
        <div>
          <label style="display:block;font-size:13px;color:#fff;opacity:.95;margin-bottom:6px;text-align:center">التاريخ الميلادي</label>
          <input id="dateInput" class="date-input" type="date">
        </div>
        <div>
          <label style="display:block;font-size:13px;color:#fff;opacity:.95;margin-bottom:6px;text-align:center">التاريخ الهجري</label>
          <input id="hijriInput" class="hijri-input" placeholder="—" readonly>
        </div>
        <div>
          <button class="btn btn-warning" id="setTodayBtn"><i class="fas fa-calendar-day"></i> اليوم</button>
        </div>
        <div>
          <button class="btn btn-primary" id="openAdminBtn"><i class="fas fa-user-shield"></i> المشرف</button>
        </div>
      </div>
    </div>

    <div class="main" id="captureArea">
      <!-- جدول الطلاب -->
      <div class="card students-card">
        <div class="section-title">قائمة الطلاب</div>
        <div style="overflow:auto">
          <table aria-label="قائمة الطلاب">
            <thead>
              <tr>
                <th style="width:6%">م</th>
                <th style="width:45%">اسم الطالب</th>
                <th style="width:24%">حالة الحضور</th>
                <th style="width:25%">ملاحظات المعلم</th>
              </tr>
            </thead>
            <tbody id="studentsTable">
              <!-- سيُملأ بواسطة الجافاسكريبت -->
            </tbody>
          </table>
        </div>
      </div>

      <!-- لوحة جانبية -->
      <div class="card sidebar">
        <div class="section-title">لوحة المشرف</div>

        <div>
          <div class="small">إضافة طالب جديد</div>
          <input id="newStudentName" class="small-input" placeholder="اكتب اسم الطالب هنا">
          <div style="display:flex;gap:8px">
            <button class="btn btn-primary" id="addStudentBtn"><i class="fas fa-plus"></i> إضافة</button>
            <button class="btn btn-danger" id="clearTodayBtn"><i class="fas fa-trash"></i> مسح بيانات اليوم</button>
          </div>
        </div>

        <div style="margin-top:12px">
          <div class="small">الطلاب الحاليون</div>
          <div class
