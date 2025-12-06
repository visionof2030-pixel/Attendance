
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<!-- ضبط viewport لتصرف طبيعي على الأجهزة -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=yes" />
<title>سجل التقويم الشامل للطلاب</title>

<!-- خطوط عربية -->
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- مكتبات PDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
    :root{
        --primary-color:#2c5aa0;
        --present-color:#2e7d32;
        --absent-color:#c62828;
        --neutral-color:#757575;
        --light-bg:#f8f9fa;
        --border-color:#dee2e6;
    }

    *{box-sizing:border-box;margin:0;padding:0}
    html,body{width:100%;height:100%;font-size:14px;background:linear-gradient(135deg,#f6f8fc 0%,#e9f2ff 100%);direction:rtl;-webkit-text-size-adjust:100%;}
    body{font-family:'Tajawal',sans-serif;min-height:100vh;padding:10px;}

    .container{
        max-width:1000px;
        margin:0 auto;
        background:#fff;
        padding:12px;
        border-radius:6px;
        box-shadow:0 3px 9px rgba(0,0,0,0.06);
        border:1px solid var(--border-color);
    }

    h2{
        text-align:center;color:var(--primary-color);font-size:1.25rem;padding-bottom:8px;border-bottom:1px solid #f0f0f0;margin-bottom:10px;position:relative;
    }
    h2:after{content:'';position:absolute;bottom:-1px;right:50%;transform:translateX(50%);width:50px;height:3px;background:linear-gradient(to right,#2c5aa0,#4a8af4);border-radius:2px;}

    .table-wrapper{
        width:100%;overflow-x:auto;-webkit-overflow-scrolling:touch;border:1px solid #eee;border-radius:6px;padding:4px;background:#fff;
        box-shadow:0 2px 6px rgba(0,0,0,0.03);
    }

    table{width:100%;border-collapse:separate;border-spacing:0;min-width:480px;font-size:0.95rem;}
    th,td{padding:8px 6px;text-align:center;border:1px solid var(--border-color);vertical-align:middle}
    th{background:linear-gradient(to right,#2c5aa0,#3a6bc5);color:#fff;font-weight:700;white-space:nowrap}
    .student-name{text-align:right;font-weight:600;white-space:normal;word-break:break-word;min-width:120px;padding-right:10px}

    tr:nth-child(even){background:#fafbfd}
    tr:hover{background:#f0f7ff;transition:background-color .15s}

    .evaluation-cell{min-width:48px;padding:6px}
    .multiline-header{white-space:normal;word-break:break-word}
    .multiline-cell{min-width:50px;max-width:80px;white-space:normal;word-break:break-word}

    .status-icon{width:36px;height:36px;display:flex;align-items:center;justify-content:center;border-radius:50%;cursor:pointer;transition:all .16s}
    .present-icon{color:var(--present-color);background:rgba(46,125,50,0.06);border:1px solid rgba(46,125,50,0.15)}
    .absent-icon{color:var(--absent-color);background:rgba(198,40,40,0.06);border:1px solid rgba(198,40,40,0.15)}
    .neutral-icon{color:var(--neutral-color);background:rgba(117,117,117,0.06);border:1px solid rgba(117,117,117,0.15)}
    .status-label{display:block;margin-top:6px;font-weight:700;font-size:0.85rem}

    .controls-container{display:flex;flex-direction:column;gap:10px;margin:14px 0}
    .category-controls{background:#fff;border-radius:6px;padding:8px;border:1px solid rgba(44,90,160,0.06);box-shadow:0 1px 4px rgba(0,0,0,0.03)}
    .control-title{color:var(--primary-color);font-weight:700;display:flex;align-items:center;justify-content:center;gap:6px;margin-bottom:6px}

    .category-buttons{display:flex;gap:8px;flex-wrap:wrap;justify-content:center}
    .status-btn{padding:8px 10px;border-radius:6px;border:none;cursor:pointer;font-weight:700;min-width:90px;display:inline-flex;align-items:center;gap:8px}
    .present-btn{background:linear-gradient(to right,var(--present-color),#4caf50);color:#fff}
    .absent-btn{background:linear-gradient(to right,var(--absent-color),#f44336);color:#fff}
    .neutral-btn{background:linear-gradient(to right,var(--neutral-color),#9e9e9e);color:#fff}
    .admin-btn{background:linear-gradient(to right,#5d4037,#795548);color:#fff}
    .random-btn{background:linear-gradient(to right,#f57c00,#ff9800);color:#fff;display:none}

    .export-container{text-align:center;margin-top:8px}
    .export{background:linear-gradient(to right,#2c5aa0,#4a8af4);color:#fff;padding:10px 14px;border-radius:8px;border:none;font-weight:800;cursor:pointer;box-shadow:0 4px 12px rgba(42,91,173,0.15)}

    .footer-note{text-align:center;margin-top:10px;color:#666;padding:8px;background:#f8f9fa;border-radius:6px;border-right:3px solid var(--primary-color)}

    /* إدارة */
    .admin-panel{max-height:0;overflow:hidden;transition:max-height .35s;padding:0;margin-top:8px;border-radius:6px}
    .admin-panel.active{max-height:420px;padding:10px;background:linear-gradient(135deg,#f8f9fa 0%,#e9ecef 100%);border:1px solid rgba(44,90,160,0.08)}

    .password-input,.date-input{padding:8px;border:1px solid #ddd;border-radius:6px;width:100%;font-size:0.95rem}
    .password-buttons{display:flex;gap:8px;margin-top:8px}
    .password-submit,.password-cancel{flex:1;padding:8px;border-radius:6px;border:none;font-weight:700;cursor:pointer}
    .password-submit{background:linear-gradient(to right,#2c5aa0,#4a8af4);color:#fff}
    .password-cancel{background:linear-gradient(to right,#757575,#9e9e9e);color:#fff}
    .password-error{color:var(--absent-color);display:none;margin-top:8px}

    .selected-date-display{display:inline-block;padding:8px;border-radius:6px;border:2px solid var(--present-color);background:#fff;margin-top:8px;font-weight:700}

    /* responsive */
    @media(max-width:600px){
        .category-buttons{gap:6px}
        .status-btn{min-width:70px;padding:8px;font-size:0.9rem}
        table{min-width:520px;font-size:0.9rem}
        .student-name{min-width:100px}
    }
</style>
</head>
<body>

<div class="container" id="captureArea">
    <h2><i class="fas fa-chart-bar" style="margin-left:8px"></i> سجل التقويم الشامل للطلاب</h2>

    <div class="table-wrapper">
        <table>
            <thead>
                <tr>
                    <th width="8%">الرقم</th>
                    <th width="30%">اسم الطالب</th>
                    <th width="12%">الواجبات</th>
                    <th width="12%" class="multiline-header">المشاريع</th>
                    <th width="12%" class="multiline-header">التطبيقات<br>والأنشطة</th>
                    <th width="12%">المشاركة</th>
                    <th width="10%">الحضور</th>
                </tr>
            </thead>
            <tbody>
                <!-- rows dynamic -->
            </tbody>
        </table>
    </div>

    <div class="controls-container">
        <div class="category-controls">
            <div class="control-title"><i class="fas fa-tasks"></i> المهام الأدائية</div>
            <div class="category-buttons">
                <button class="status-btn present-btn" onclick="setAllCategory('performance','present')"><i class="fas fa-check"></i> الكل "صح"</button>
                <button class="status-btn neutral-btn" onclick="setAllCategory('performance','neutral')"><i class="fas fa-minus"></i> الكل "محايد"</button>
                <button class="status-btn absent-btn" onclick="setAllCategory('performance','absent')"><i class="fas fa-times"></i> الكل "خطأ"</button>
            </div>
        </div>

        <div class="category-controls">
            <div class="control-title"><i class="fas fa-comments"></i> المشاركة والتفاعل</div>
            <div class="category-buttons">
                <button class="status-btn present-btn" onclick="setAllCategory('interaction','present')"><i class="fas fa-check"></i> الكل "صح"</button>
                <button class="status-btn neutral-btn" onclick="setAllCategory('interaction','neutral')"><i class="fas fa-minus"></i> الكل "محايد"</button>
                <button class="status-btn absent-btn" onclick="setAllCategory('interaction','absent')"><i class="fas fa-times"></i> الكل "خطأ"</button>
            </div>
        </div>

        <div class="category-controls">
            <div class="control-title"><i class="fas fa-users"></i> الحضور والإدارة</div>
            <div class="category-buttons">
                <button class="status-btn present-btn" onclick="setAllAttendance('present')"><i class="fas fa-user-check"></i> الكل حاضر</button>
                <button class="status-btn absent-btn" onclick="setAllAttendance('absent')"><i class="fas fa-user-slash"></i> الكل غائب</button>
                <button class="status-btn admin-btn" onclick="toggleAdminPanel()"><i class="fas fa-cog"></i> إدارة</button>
                <button class="status-btn random-btn" id="randomBtn" onclick="toggleRandom()"><i class="fas fa-random"></i> عشوائي</button>
            </div>
        </div>
    </div>

    <div class="admin-panel" id="adminPanel">
        <div class="admin-panel-content">
            <div class="password-section" id="passwordSection">
                <div class="admin-title"><i class="fas fa-lock"></i> التحقق من الهوية</div>
                <p class="admin-description">للدخول إلى خيارات الإدارة المتقدمة، يرجى إدخال كلمة المرور:</p>
                <div class="password-container">
                    <input type="password" class="password-input" id="passwordInput" placeholder="أدخل كلمة المرور هنا" autocomplete="off">
                    <div class="password-buttons">
                        <button class="password-submit" onclick="checkPassword()">تحقق</button>
                        <button class="password-cancel" onclick="toggleAdminPanel()">إلغاء</button>
                    </div>
                </div>
                <p class="password-error" id="passwordError"><i class="fas fa-exclamation-triangle"></i> كلمة المرور غير صحيحة</p>
            </div>

            <div class="date-section" id="dateSection">
                <div class="date-title"><i class="fas fa-calendar-alt"></i> تحديد وقت التحضير</div>
                <p class="admin-description">يمكنك تحديد تاريخ التقييم للأشهر الماضية أو القادمة:</p>
                <div class="date-controls">
                    <input type="date" class="date-input" id="attendanceDate" value="">
                    <div style="display:flex;gap:8px;margin-top:8px">
                        <button class="date-btn" onclick="setCustomDate()" style="flex:1;background:linear-gradient(to right,var(--primary-color),#4a8af4);color:#fff;padding:8px;border-radius:6px;border:none">تطبيق التاريخ</button>
                        <button class="date-btn" onclick="resetToToday()" style="flex:1;background:linear-gradient(to right,#757575,#9e9e9e);color:#fff;padding:8px;border-radius:6px;border:none">الرجوع لليوم</button>
                    </div>
                </div>
                <div class="selected-date-display" id="selectedDateDisplay"><i class="far fa-calendar-check"></i> تاريخ التقييم: <span id="currentDateDisplay">اليوم</span></div>
            </div>
        </div>
    </div>

    <div class="footer-note"><i class="fas fa-info-circle" style="margin-left:8px"></i> انقر على أيقونة أي تقييم للتبديل بين "صح" (أخضر)، "محايد" (رمادي)، "خطأ" (أحمر). الحضور يتبدل بين "حاضر" و"غائب" فقط.</div>
</div>

<div class="export-container">
    <button class="export" onclick="exportPDF()"><i class="fas fa-file-pdf"></i> تصدير سجل التقييم كملف PDF</button>
</div>

<script>
/* ===== بيانات وتجهيز ===== */
let studentsData = {};
const totalStudents = 8;
const studentNames = [
    "أحمد محمد",
    "جسّار فهد",
    "سارة عبدالله",
    "يوسف خالد",
    "نورة سعيد",
    "فارس علي",
    "ليلى حسن",
    "محمد علي"
];

function initializeStudentsData(){
    for(let i=1;i<=totalStudents;i++){
        studentsData[i]={
            attendance:'present',
            assignments:'present',
            projects:'present',
            classroomApps:'present',
            participation:'present'
        };
    }
}

/* إنشاء الجدول ديناميكيًا */
function createTable(){
    const tbody=document.querySelector('tbody');
    tbody.innerHTML='';
    for(let i=1;i<=totalStudents;i++){
        const s=studentsData[i];
        const getStatusInfo=(status,type='evaluation')=>{
            if(type==='attendance'){
                return status==='present' ? {icon:'fa-check',text:'حاضر',cls:'present'} : {icon:'fa-times',text:'غائب',cls:'absent'};
            } else {
                if(status==='present') return {icon:'fa-check',text:'صح',cls:'present'};
                if(status==='neutral') return {icon:'fa-minus',text:'محايد',cls:'neutral'};
                return {icon:'fa-times',text:'خطأ',cls:'absent'};
            }
        };
        const att=getStatusInfo(s.attendance,'attendance');
        const asg=getStatusInfo(s.assignments);
        const prj=getStatusInfo(s.projects);
        const app=getStatusInfo(s.classroomApps);
        const part=getStatusInfo(s.participation);
        const row=document.createElement('tr');
        row.innerHTML=`
            <td>${i}</td>
            <td class="student-name">${studentNames[i-1]}</td>
            <td class="evaluation-cell multiline-cell">
                <div class="status-icon ${asg.cls}-icon" onclick="toggleEvaluation(${i},'assignments')" id="assignments-${i}"><i class="fas ${asg.icon}"></i></div>
                <span class="status-label ${asg.cls}-label" id="label-assignments-${i}">${asg.text}</span>
            </td>
            <td class="evaluation-cell multiline-cell">
                <div class="status-icon ${prj.cls}-icon" onclick="toggleEvaluation(${i},'projects')" id="projects-${i}"><i class="fas ${prj.icon}"></i></div>
                <span class="status-label ${prj.cls}-label" id="label-projects-${i}">${prj.text}</span>
            </td>
            <td class="evaluation-cell multiline-cell">
                <div class="status-icon ${app.cls}-icon" onclick="toggleEvaluation(${i},'classroomApps')" id="classroomApps-${i}"><i class="fas ${app.icon}"></i></div>
                <span class="status-label ${app.cls}-label" id="label-classroomApps-${i}">${app.text}</span>
            </td>
            <td class="evaluation-cell">
                <div class="status-icon ${part.cls}-icon" onclick="toggleEvaluation(${i},'participation')" id="participation-${i}"><i class="fas ${part.icon}"></i></div>
                <span class="status-label ${part.cls}-label" id="label-participation-${i}">${part.text}</span>
            </td>
            <td class="evaluation-cell">
                <div class="status-icon ${att.cls}-icon" onclick="toggleAttendance(${i})" id="attendance-${i}"><i class="fas ${att.icon}"></i></div>
                <span class="status-label ${att.cls}-label" id="label-attendance-${i}">${att.text}</span>
            </td>
        `;
        tbody.appendChild(row);
    }
}

/* ===== إدارة و تواريخ و تفاعل ===== */
let isAdminAuthenticated=false;
let selectedAttendanceDate=null;

function initializeDateField(){
    const today=new Date();
    const formattedDate=today.toISOString().split('T')[0];
    document.getElementById('attendanceDate').value=formattedDate;
    updateDateDisplay();
}
function updateDateDisplay(){
    const dateInput=document.getElementById('attendanceDate').value;
    const dateDisplay=document.getElementById('currentDateDisplay');
    if(!dateInput){ dateDisplay.textContent='اليوم'; selectedAttendanceDate=null; return; }
    const date=new Date(dateInput);
    const options={weekday:'long',year:'numeric',month:'long',day:'numeric'};
    dateDisplay.textContent = date.toLocaleDateString('ar-SA',options);
    selectedAttendanceDate = dateInput;
}
function setCustomDate(){
    const dateInput=document.getElementById('attendanceDate').value;
    if(!dateInput){ showNotification('الرجاء اختيار تاريخ صحيح','absent'); return; }
    updateDateDisplay();
    showNotification(`تم تعيين تاريخ التقييم إلى ${document.getElementById('currentDateDisplay').textContent}`,'present');
}
function resetToToday(){
    const today=new Date();
    const formattedDate=today.toISOString().split('T')[0];
    document.getElementById('attendanceDate').value=formattedDate;
    updateDateDisplay();
    showNotification('تم الرجوع إلى تاريخ اليوم','present');
}
function toggleAdminPanel(){
    const adminPanel=document.getElementById('adminPanel');
    const adminBtn=document.querySelector('.admin-btn');
    if(adminPanel.classList.contains('active')){
        adminPanel.classList.remove('active');
        adminBtn.innerHTML='<i class="fas fa-cog"></i> إدارة';
        adminBtn.style.background='';
        document.getElementById('dateSection').classList.remove('active');
        document.getElementById('passwordInput').value='';
        document.getElementById('passwordError').style.display='none';
    } else {
        adminPanel.classList.add('active');
        adminBtn.innerHTML='<i class="fas fa-times"></i> إغلاق الإدارة';
        adminBtn.style.background='';
        if(isAdminAuthenticated){
            document.getElementById('passwordSection').style.display='none';
            document.getElementById('dateSection').classList.add('active');
            initializeDateField();
        } else {
            document.getElementById('passwordSection').style.display='block';
            document.getElementById('dateSection').classList.remove('active');
            setTimeout(()=>document.getElementById('passwordInput').focus(),150);
        }
    }
}
function checkPassword(){
    const password=document.getElementById('passwordInput').value;
    const errorElement=document.getElementById('passwordError');
    const passwordSection=document.getElementById('passwordSection');
    const dateSection=document.getElementById('dateSection');
    if(password==='Jassar1436'){
        isAdminAuthenticated=true;
        document.getElementById('randomBtn').style.display='inline-flex';
        passwordSection.style.display='none';
        dateSection.classList.add('active');
        initializeDateField();
        showNotification('تم التحقق من الهوية بنجاح! خيارات الإدارة متاحة الآن.','present');
        document.getElementById('passwordInput').value='';
        errorElement.style.display='none';
    } else {
        errorElement.style.display='block';
        document.getElementById('passwordInput').value='';
        document.getElementById('passwordInput').focus();
        document.getElementById('passwordInput').style.animation='shake .5s';
        setTimeout(()=>{document.getElementById('passwordInput').style.animation='';},500);
    }
}
document.getElementById('passwordInput').addEventListener('keypress',function(e){ if(e.key==='Enter') checkPassword(); });

/* ===== تبديل التقييمات والحضور ===== */
function toggleEvaluation(studentId,evaluationType){
    const student=studentsData[studentId];
    const current=student[evaluationType];
    let next = current==='present' ? 'neutral' : current==='neutral' ? 'absent' : 'present';
    student[evaluationType]=next;
    updateStudentDisplay(studentId);
    const evaluationNames = {'assignments':'الواجبات','projects':'المشاريع','classroomApps':'التطبيقات والأنشطة','participation':'المشاركة'};
    const statusNames = {'present':'صح','neutral':'محايد','absent':'خطأ'};
    showNotification(`تم تغيير ${evaluationNames[evaluationType]} لـ ${studentNames[studentId-1]} إلى "${statusNames[next]}"`, next);
}
function toggleAttendance(studentId){
    const student=studentsData[studentId];
    student.attendance = student.attendance==='present' ? 'absent' : 'present';
    updateStudentDisplay(studentId);
    const statusText = student.attendance==='present' ? 'حاضر' : 'غائب';
    showNotification(`تم تغيير حالة حضور ${studentNames[studentId-1]} إلى "${statusText}"`, student.attendance);
}
function updateStudentDisplay(studentId){
    const s=studentsData[studentId];
    const getStatusInfo=(status,type='evaluation')=>{
        if(type==='attendance') return status==='present'?{icon:'fa-check',text:'حاضر',cls:'present'}:{icon:'fa-times',text:'غائب',cls:'absent'};
        if(status==='present') return {icon:'fa-check',text:'صح',cls:'present'};
        if(status==='neutral') return {icon:'fa-minus',text:'محايد',cls:'neutral'};
        return {icon:'fa-times',text:'خطأ',cls:'absent'};
    };
    const updateElement=(id,info)=>{
        const icon=document.getElementById(id);
        const label=document.getElementById(`label-${id}`);
        if(icon && label){
            icon.className=`status-icon ${info.cls}-icon`;
            icon.innerHTML=`<i class="fas ${info.icon}"></i>`;
            label.className=`status-label ${info.cls}-label`;
            label.textContent=info.text;
            icon.classList.add('pulse');
            setTimeout(()=>icon.classList.remove('pulse'),300);
        }
    };
    updateElement(`attendance-${studentId}`, getStatusInfo(s.attendance,'attendance'));
    updateElement(`assignments-${studentId}`, getStatusInfo(s.assignments));
    updateElement(`projects-${studentId}`, getStatusInfo(s.projects));
    updateElement(`classroomApps-${studentId}`, getStatusInfo(s.classroomApps));
    updateElement(`participation-${studentId}`, getStatusInfo(s.participation));
}

/* ===== عمليات شاملة ===== */
function setAllCategory(category,status){
    let types=[];
    if(category==='performance') types=['assignments','projects'];
    if(category==='interaction') types=['classroomApps','participation'];
    for(let i=1;i<=totalStudents;i++){
        types.forEach(t=>studentsData[i][t]=status);
        setTimeout(()=>updateStudentDisplay(i), i*30);
    }
    setTimeout(()=>showNotification(`تم تعيين جميع ${category==='performance' ? 'المهام الأدائية' : 'المشاركة والتفاعل'} كـ ${status==='present'?'\"صح\"':status==='neutral'?'\"محايد\"':'\"خطأ\"'}`, status), totalStudents*60);
}
function setAllAttendance(status){
    for(let i=1;i<=totalStudents;i++){
        studentsData[i].attendance=status;
        setTimeout(()=>updateStudentDisplay(i), i*40);
    }
    setTimeout(()=>showNotification(`تم تعيين جميع الطلاب كـ ${status==='present'?'حاضرين':'غائبين'}`, status), totalStudents*80);
}
function toggleRandom(){
    if(!isAdminAuthenticated){ showNotification('يجب التحقق من الهوية أولاً!','absent'); toggleAdminPanel(); return; }
    const changes=Math.floor(Math.random()*3)+2;
    for(let i=0;i<changes;i++){
        const randomStudent=Math.floor(Math.random()*totalStudents)+1;
        const evaluationTypes=['assignments','projects','classroomApps','participation','attendance'];
        const randomType=evaluationTypes[Math.floor(Math.random()*evaluationTypes.length)];
        setTimeout(()=>{ if(randomType==='attendance') toggleAttendance(randomStudent); else toggleEvaluation(randomStudent,randomType); }, i*220);
    }
    showNotification(`تم تبديل تقييمات ${changes} طلاب عشوائيًا`,'present');
}

/* ===== إشعارات صغيرة ===== */
function showNotification(message,type='present'){
    const existing=document.querySelector('.custom-notification');
    if(existing) existing.remove();
    const n=document.createElement('div');
    n.className='custom-notification';
    n.style.cssText=`position:fixed;top:12px;left:12px;right:12px;z-index:9999;padding:10px;border-radius:8px;color:#fff;text-align:center;font-weight:800;box-shadow:0 4px 12px rgba(0,0,0,0.12);animation:fadeInOut 3s ease-in-out;background:${type==='present'? 'var(--present-color)': type==='neutral'? 'var(--neutral-color)' : 'var(--absent-color)'};`;
    n.innerHTML = `<i class="${type==='present'?'fas fa-check-circle': type==='neutral'?'fas fa-minus-circle':'fas fa-times-circle'}" style="margin-left:8px"></i>${message}`;
    document.body.appendChild(n);
    setTimeout(()=>{ n.style.opacity='0'; n.style.transition='opacity .5s'; setTimeout(()=>{ if(n.parentNode) n.parentNode.removeChild(n); },500); },2600);
}

/* ===== تصدير PDF متعدد الصفحات (تحسين) ===== */
async function exportPDF(){
    const { jsPDF } = window.jspdf;

    const exportBtn=document.querySelector('.export');
    const originalText=exportBtn.innerHTML;
    exportBtn.innerHTML='<i class="fas fa-spinner fa-spin"></i> جاري التصدير...';
    exportBtn.disabled=true;

    // تحضير تاريخ أعلى الصفحة
    const dateElement=document.createElement('div');
    let dateText='';
    if(selectedAttendanceDate){
        const d=new Date(selectedAttendanceDate);
        dateText=`تاريخ التقييم: ${d.toLocaleDateString('ar-SA',{year:'numeric',month:'long',day:'numeric'})}`;
    } else {
        dateText=`تاريخ التقييم: اليوم (${new Date().toLocaleDateString('ar-SA')})`;
    }
    dateElement.style.cssText='text-align:left;margin-bottom:8px;color:#666;padding:6px;background:#f8f9fa;border-radius:6px;border-right:3px solid var(--primary-color);font-weight:700';
    dateElement.innerHTML=`<i class="far fa-calendar-alt" style="margin-left:8px"></i> ${dateText} - <i class="far fa-clock" style="margin-left:8px"></i> وقت التصدير: ${new Date().toLocaleTimeString('ar-SA',{hour:'2-digit',minute:'2-digit'})}`;

    const captureArea=document.getElementById('captureArea');
    captureArea.insertBefore(dateElement, captureArea.firstChild);

    // إلتقاط الصفحة كـ canvas
    // نخلي scale أعلى للحصول على جودة جيدة
    const canvas = await html2canvas(captureArea, {scale:2,useCORS:true,backgroundColor:'#ffffff',scrollY:-window.scrollY});

    // إزالة التاريخ المؤقت
    captureArea.removeChild(dateElement);

    // تحويل الـ canvas إلى صور مفصولة إذا لزم
    const imgDataFull = canvas.toDataURL('image/png');
    const pdf = new jsPDF('p','mm','a4');

    // ضبط القياسات
    const pageWidthMm = 210;
    const pageHeightMm = 297;
    const marginMm = 10;
    const usableWidthMm = pageWidthMm - marginMm*2; // 190
    const pxPerMm = canvas.width / usableWidthMm; // نسبة البيكسل لكل مم على الصورة
    const pageHeightPx = Math.floor((pageHeightMm - marginMm*2) * pxPerMm); // ارتفاع صفحة واحدة بالبيكسل

    // تقسيم الـ canvas إلى صفحات
    let y = 0;
    while(y < canvas.height){
        const sliceHeight = Math.min(pageHeightPx, canvas.height - y);
        const tmpCanvas = document.createElement('canvas');
        tmpCanvas.width = canvas.width;
        tmpCanvas.height = sliceHeight;
        const ctx = tmpCanvas.getContext('2d');
        ctx.fillStyle = '#ffffff';
        ctx.fillRect(0,0,tmpCanvas.width,tmpCanvas.height);
        ctx.drawImage(canvas, 0, y, canvas.width, sliceHeight, 0, 0, canvas.width, sliceHeight);

        const imgData = tmpCanvas.toDataURL('image/png');
        const imgHeightMm = (sliceHeight / canvas.width) * usableWidthMm;

        if(y > 0) pdf.addPage();
        pdf.addImage(imgData, 'PNG', marginMm, marginMm, usableWidthMm, imgHeightMm);

        y += sliceHeight;
    }

    // اسم الملف
    let fileName = 'سجل-التقييم-الشامل';
    if(selectedAttendanceDate){
        const dt = new Date(selectedAttendanceDate);
        fileName = `سجل-التقييم-${dt.toISOString().slice(0,10)}`;
    } else {
        fileName = `سجل-التقييم-${new Date().toISOString().slice(0,10)}`;
    }

    pdf.save(`${fileName}.pdf`);

    exportBtn.innerHTML=originalText;
    exportBtn.disabled=false;
    showNotification('تم تصدير ملف PDF بنجاح!','present');
}

/* ===== تهيئة عند التحميل ===== */
document.addEventListener('DOMContentLoaded',function(){
    initializeStudentsData();
    createTable();
    initializeDateField();
});
</script>

</body>
</html>
