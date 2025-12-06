<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1" />
<title>سجل متابعة الطلاب - مضغوط</title>

<!-- خطوط وأيقونات -->
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@300;400;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- مكتبات PDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
:root{
  --bg:#f5f7fb;
  --card:#ffffff;
  --accent1:#6c63ff;
  --accent2:#7b93ff;
  --ok-bg: #d4f7d8;
  --no-bg: #ffdfe0;
  --text:#25303b;
  --muted:#6b7280;
  --shadow: 0 6px 20px rgba(16,24,40,0.06);
}

/* عام */
html,body{height:100%}
body{
  font-family:"Tajawal",sans-serif;
  background: linear-gradient(180deg, var(--bg) 0%, #eef4ff 100%);
  margin:0;
  padding:14px;
  color:var(--text);
  -webkit-font-smoothing:antialiased;
  -moz-osx-font-smoothing:grayscale;
  direction:rtl;
}

/* الحاوية */
.container{
  max-width:920px;
  margin:0 auto;
}

/* رأس البطاقة */
.header-card{
  display:flex;
  gap:10px;
  align-items:center;
  justify-content:space-between;
  margin-bottom:10px;
}

.title {
  display:flex;
  gap:12px;
  align-items:center;
}
.title .logo{
  width:44px;
  height:44px;
  border-radius:10px;
  background:linear-gradient(135deg,var(--accent1),var(--accent2));
  display:flex;
  align-items:center;
  justify-content:center;
  color:white;
  font-weight:700;
  box-shadow:var(--shadow);
}
.title h1{
  margin:0;
  font-size:16px;
  font-weight:600;
  color:var(--text);
}

/* أزرار التحكم العلوية */
.controls{
  display:flex;
  gap:8px;
  align-items:center;
  flex-wrap:wrap;
}
.control-btn{
  background:var(--card);
  border:1px solid rgba(99,102,241,0.08);
  padding:8px 12px;
  border-radius:10px;
  cursor:pointer;
  font-size:13px;
  box-shadow:var(--shadow);
  display:inline-flex;
  gap:8px;
  align-items:center;
}
.control-btn.primary{
  background:linear-gradient(90deg,var(--accent1),var(--accent2));
  color:white;
  border:none;
}
.small{
  font-size:12px;
  padding:6px 8px;
}

/* بطاقة الجدول */
.card{
  background:var(--card);
  border-radius:14px;
  padding:10px;
  box-shadow:var(--shadow);
  overflow:hidden;
}

/* جدول مدمج مضغوط */
.table {
  width:100%;
  border-collapse:collapse;
  font-size:12px;
  min-width:100%;
}
.table thead th{
  font-weight:600;
  font-size:12px;
  color:white;
  background:linear-gradient(90deg,var(--accent1),var(--accent2));
  padding:8px 6px;
  text-align:center;
  vertical-align:middle;
}
.table tbody td{
  padding:8px 6px;
  text-align:center;
  vertical-align:middle;
  border-bottom:1px solid #f0f3ff;
  color:var(--text);
}

/* عمود اسم الطالب */
.col-name{
  text-align:right;
  padding-right:12px;
  max-width:170px;
  white-space:nowrap;
  overflow:hidden;
  text-overflow:ellipsis;
  font-weight:600;
  font-size:13px;
}

/* أزرار التبديل الصغيرة (دائرة) */
.toggle {
  width:34px;
  height:30px;
  border-radius:8px;
  display:inline-flex;
  align-items:center;
  justify-content:center;
  font-weight:700;
  cursor:pointer;
  border:none;
  transition:transform .14s ease, box-shadow .14s ease;
  user-select:none;
  box-shadow: 0 4px 10px rgba(2,6,23,0.06);
  font-size:15px;
}
.toggle:active{transform:scale(.96)}
.toggle.yes{ background:var(--ok-bg); color:#0f7a24; }
.toggle.no{ background:var(--no-bg); color:#9a1e1e; }

/* أيقونات العمليات */
.row-actions{
  display:flex;
  gap:6px;
  justify-content:center;
  align-items:center;
}
.icon-btn{
  width:34px;
  height:30px;
  border-radius:8px;
  display:inline-flex;
  align-items:center;
  justify-content:center;
  background:#fff;
  border:1px solid rgba(15,23,42,0.04);
  cursor:pointer;
  font-size:13px;
  color:var(--muted);
  box-shadow: 0 3px 8px rgba(2,6,23,0.04);
}

/* شكل الحقول الإضافة */
.add-row{
  display:flex;
  gap:8px;
  align-items:center;
  margin-bottom:8px;
}
.input{
  flex:1;
  min-width:0;
  padding:8px 10px;
  border-radius:10px;
  border:1px solid rgba(15,23,42,0.06);
  background:#fbfdff;
  font-size:13px;
  color:var(--text);
}
.add-btn{
  padding:8px 12px;
  border-radius:10px;
  background:linear-gradient(90deg,var(--accent1),var(--accent2));
  color:white;
  border:none;
  cursor:pointer;
  font-weight:700;
  font-size:13px;
}

/* عدادات سريعة */
.stats{
  display:flex;
  gap:8px;
  align-items:center;
  justify-content:flex-start;
  margin-top:8px;
  flex-wrap:wrap;
}
.stat{
  background:linear-gradient(180deg,#ffffff,#fbfdff);
  padding:8px 10px;
  border-radius:10px;
  font-size:12px;
  color:var(--muted);
  box-shadow: 0 4px 12px rgba(13,20,40,0.03);
  display:inline-flex;
  gap:8px;
  align-items:center;
}

/* إشعار سريع */
.toast{
  position:fixed;
  left:14px;
  bottom:18px;
  background:rgba(16,24,40,0.95);
  color:white;
  padding:10px 14px;
  border-radius:10px;
  font-size:13px;
  z-index:9999;
  box-shadow:0 6px 20px rgba(2,6,23,0.4);
}

/* استجابة للجوال - الحفاظ على كل الأعمدة مرئية */
@media (max-width:420px){
  .title h1{font-size:14px}
  .toggle{ width:30px; height:28px; font-size:14px }
  .icon-btn{ width:30px; height:28px; font-size:12px}
  .col-name{ max-width:120px; font-size:13px }
  .table thead th{ font-size:11px; padding:6px 4px }
  .table tbody td{ padding:6px 4px; font-size:12px }
  .add-btn{ padding:7px 10px; font-size:12px }
  .input{ padding:8px 8px; font-size:13px }
}
</style>
</head>
<body>
<div class="container">

  <!-- رأس صغير -->
  <div class="header-card">
    <div class="title">
      <div class="logo">س</div>
      <h1>سجل متابعة الطلاب (مضغوط للجوال)</h1>
    </div>

    <div class="controls">
      <button class="control-btn" id="resetBtn" title="إعادة تعيين (حذف الحفظ)">
        <i class="fa-solid fa-rotate-left"></i> إعادة تعيين
      </button>

      <button class="control-btn" id="exportBtn" title="تصدير PDF">
        <i class="fa-solid fa-file-pdf"></i> تصدير PDF
      </button>
    </div>
  </div>

  <div class="card" id="cardRoot">
    <!-- إضافة طالب -->
    <div class="add-row" style="margin-bottom:10px;">
      <input id="newStudentInput" class="input" placeholder="أضف اسم طالب جديد ثم اضغط إضافة" />
      <button id="addBtn" class="add-btn"><i class="fa-solid fa-plus"></i> إضافة</button>
    </div>

    <!-- عدادات -->
    <div class="stats" id="statsArea" style="margin-bottom:8px;">
      <div class="stat"><strong id="totalCount">0</strong> إجمالي</div>
      <div class="stat"><strong id="presentCount">0</strong> حاضر</div>
      <div class="stat"><strong id="absentCount">0</strong> غائب</div>
      <div class="stat"><strong id="homeworkCount">0</strong> واجبات ✓</div>
    </div>

    <!-- الجدول -->
    <div style="overflow:hidden;">
      <table class="table" id="studentsTable" role="table" aria-label="جدول متابعة الطلاب">
        <thead>
          <tr>
            <th style="width:28%;">الطالب</th>
            <th style="width:14%;">حضور</th>
            <th style="width:12%;">واجب</th>
            <th style="width:12%;">مشروع</th>
            <th style="width:18%;">تطبيق/نشاط</th>
            <th style="width:12%;">مشاركة</th>
            <th style="width:4%;"></th>
          </tr>
        </thead>
        <tbody id="tbody"></tbody>
      </table>
    </div>
  </div>

</div>

<!-- إشعار -->
<div id="toast" class="toast" style="display:none"></div>

<script>
// مفتاح التخزين
const STORAGE_KEY = 'compact_attendance_v1';

// بنية الطلاب الافتراضية
const defaultStudents = [
  { id: genId(), name: "أحمد محمد", attendance: false, homework: false, project: false, activity: false, participation: false },
  { id: genId(), name: "جسّار فهد", attendance: false, homework: false, project: false, activity: false, participation: false },
  { id: genId(), name: "سارة عبدالله", attendance: false, homework: false, project: false, activity: false, participation: false },
  { id: genId(), name: "يوسف خالد", attendance: false, homework: false, project: false, activity: false, participation: false },
  { id: genId(), name: "نورة سعيد", attendance: false, homework: false, project: false, activity: false, participation: false },
  { id: genId(), name: "فارس علي", attendance: false, homework: false, project: false, activity: false, participation: false },
  { id: genId(), name: "ليلى حسن", attendance: false, homework: false, project: false, activity: false, participation: false },
  { id: genId(), name: "محمد علي", attendance: false, homework: false, project: false, activity: false, participation: false }
];

// حالة التطبيق (قائمة الطلاب)
let students = [];

/* --- أدوات صغيرة --- */
function genId(){
  return 's' + Date.now().toString(36) + Math.floor(Math.random()*999).toString(36);
}
function toast(msg, time=1800){
  const el = document.getElementById('toast');
  el.textContent = msg;
  el.style.display = 'block';
  clearTimeout(el._t);
  el._t = setTimeout(()=> el.style.display='none', time);
}

/* --- تحميل / حفظ --- */
function loadData(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(raw){
      students = JSON.parse(raw);
    } else {
      students = defaultStudents;
      saveData();
    }
  } catch(e){
    console.error('load error', e);
    students = defaultStudents;
    saveData();
  }
}
function saveData(){
  localStorage.setItem(STORAGE_KEY, JSON.stringify(students));
  updateStats();
}

/* --- بناء صفوف الجدول --- */
function buildRow(student, index){
  // خلايا التبديل: كل خانة زر واحد يتبدل بين ✓ و ✕ ويحمل class yes/no
  const tr = document.createElement('tr');
  tr.dataset.id = student.id;

  // اسم الطالب
  const tdName = document.createElement('td');
  tdName.className = 'col-name';
  tdName.title = student.name;
  tdName.textContent = student.name;
  tr.appendChild(tdName);

  // helper to create toggle cell
  function mkToggleCell(key){
    const td = document.createElement('td');
    const btn = document.createElement('button');
    btn.className = 'toggle ' + (student[key] ? 'yes' : 'no');
    btn.innerText = student[key] ? '✓' : '✕';
    btn.addEventListener('click', () => {
      student[key] = !student[key];
      btn.className = 'toggle ' + (student[key] ? 'yes' : 'no');
      btn.innerText = student[key] ? '✓' : '✕';
      saveData();
      // بصمة صوتية/إشعار صغير
      toast(`تحديث: ${student.name}`);
    });
    td.appendChild(btn);
    return td;
  }

  tr.appendChild(mkToggleCell('attendance'));
  tr.appendChild(mkToggleCell('homework'));
  tr.appendChild(mkToggleCell('project'));
  tr.appendChild(mkToggleCell('activity'));
  tr.appendChild(mkToggleCell('participation'));

  // عمليات (حذف)
  const tdActions = document.createElement('td');
  tdActions.className = 'row-actions';
  const delBtn = document.createElement('button');
  delBtn.className = 'icon-btn';
  delBtn.title = 'حذف الطالب';
  delBtn.innerHTML = '<i class="fa-solid fa-trash"></i>';
  delBtn.addEventListener('click', () => {
    if(confirm(`هل تريد حذف "${student.name}"؟`)){
      students = students.filter(s => s.id !== student.id);
      saveData();
      renderTable();
      toast('تم الحذف');
    }
  });
  tdActions.appendChild(delBtn);
  tr.appendChild(tdActions);

  return tr;
}

function renderTable(){
  const tbody = document.getElementById('tbody');
  tbody.innerHTML = '';
  // نعرض الطلاب بالترتيب المخزن
  students.forEach((s,i) => {
    tbody.appendChild(buildRow(s,i));
  });
  updateStats();
}

/* --- إحصائيات موجزة --- */
function updateStats(){
  const total = students.length;
  const present = students.filter(s=>s.attendance).length;
  const absent = total - present;
  const hw = students.filter(s=>s.homework).length;

  document.getElementById('totalCount').textContent = total;
  document.getElementById('presentCount').textContent = present;
  document.getElementById('absentCount').textContent = absent;
  document.getElementById('homeworkCount').textContent = hw;
}

/* --- إضافة طالب --- */
document.getElementById('addBtn').addEventListener('click', onAddStudent);
document.getElementById('newStudentInput').addEventListener('keypress', (e)=> {
  if(e.key==='Enter') onAddStudent();
});

function onAddStudent(){
  const nameInput = document.getElementById('newStudentInput');
  const name = nameInput.value.trim();
  if(!name){ toast('الرجاء إدخال اسم الطالب'); return; }
  const newS = { id: genId(), name, attendance:false, homework:false, project:false, activity:false, participation:false };
  students.push(newS);
  saveData();
  renderTable();
  nameInput.value = '';
  toast('تمت الإضافة');
}

/* --- إعادة تعيين --- */
document.getElementById('resetBtn').addEventListener('click', ()=>{
  if(!confirm('إعادة التعيين ستحذف جميع البيانات المحفوظة. هل ترغب بالمتابعة؟')) return;
  localStorage.removeItem(STORAGE_KEY);
  loadData();
  renderTable();
  toast('تمت إعادة التعيين');
});

/* --- تصدير إلى PDF --- */
document.getElementById('exportBtn').addEventListener('click', async ()=>{
  await exportPDF();
});

async function exportPDF(){
  const exportBtn = document.getElementById('exportBtn');
  exportBtn.disabled = true;
  exportBtn.innerHTML = '<i class="fa-solid fa-spinner fa-spin"></i> جاري التحضير...';

  // ننسخ البطاقة إلى عنصر مؤقت مع تعديل نسق الطباعة
  const card = document.getElementById('cardRoot');
  const clone = card.cloneNode(true);

  // نزيل أزرار التفاعل ونحول الأزرار إلى نصوص ✓/✕ للوضوح في PDF
  clone.querySelectorAll('button.toggle').forEach(b => {
    const span = document.createElement('div');
    span.style.display = 'inline-block';
    span.style.minWidth = '24px';
    span.style.textAlign = 'center';
    span.style.fontWeight = '700';
    span.style.fontSize = '14px';
    span.textContent = b.classList.contains('yes') ? '✓' : '✕';
    b.replaceWith(span);
  });
  clone.querySelectorAll('.icon-btn, .add-row, .stats, .control-btn, #exportBtn').forEach(n => {
    if(n) n.remove();
  });

  // وضع العُنصر على الصفحة مؤقتًا (خفي)
  clone.style.background = '#fff';
  clone.style.padding = '14px';
  clone.style.borderRadius = '10px';
  clone.style.width = '100%';
  clone.style.boxSizing = 'border-box';
  clone.style.position = 'fixed';
  clone.style.left = '50%';
  clone.style.top = '-9999px';
  document.body.appendChild(clone);

  // إعداد html2canvas (مقياس يعتمد على DPR الهاتف)
  const scale = Math.min(2, window.devicePixelRatio || 1.5);
  const canvas = await html2canvas(clone, { scale, useCORS:true, backgroundColor:'#ffffff' });

  // إزالة العنصر المؤقت
  document.body.removeChild(clone);

  const imgData = canvas.toDataURL('image/png');
  const { jsPDF } = window.jspdf;
  // PDF بحجم A4 طولي مناسب للعرض المدمج
  const pdf = new jsPDF('p', 'mm', 'a4');
  const pageWidth = pdf.internal.pageSize.getWidth();
  const pageHeight = pdf.internal.pageSize.getHeight();

  // حساب نسب الصورة
  const imgWidth = pageWidth;
  const imgHeight = (canvas.height * imgWidth) / canvas.width;

  if(imgHeight <= pageHeight){
    pdf.addImage(imgData, 'PNG', 0, 0, imgWidth, imgHeight);
  } else {
    // تقسيم الصورة لصفحات إن لزم
    let position = 0;
    let remainingHeight = imgHeight;
    const imgPageHeight = pageHeight;
    while(remainingHeight > 0){
      pdf.addImage(imgData, 'PNG', 0, position, imgWidth, imgHeight);
      remainingHeight -= imgPageHeight;
      position -= imgPageHeight;
      if(remainingHeight > 0) pdf.addPage();
    }
  }

  const fileName = `سجل-المتابعة-${new Date().toISOString().slice(0,10)}.pdf`;
  pdf.save(fileName);

  exportBtn.disabled = false;
  exportBtn.innerHTML = '<i class="fa-solid fa-file-pdf"></i> تصدير PDF';
  toast('تم تصدير PDF بنجاح');
}

/* --- تهيئة التطبيق --- */
function init(){
  loadData();
  renderTable();
  // تحسين تجربة اللمس: عند فتح الصفحة، نضع المؤشر في الإدخال
  const input = document.getElementById('newStudentInput');
  if(input) input.placeholder = 'مثال: هاجر علي';
}
init();
</script>

</body>
</html>
