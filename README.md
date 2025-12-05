<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام متابعة طلاب اللغة الإنجليزية</title>
    
    <!-- خطوط عربية -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Naskh+Arabic:wght@400;500;600;700&family=Amiri:wght@400;700&family=Tajawal:wght@300;400;500;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- مكتبات -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/moment@2.29.4/moment.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/moment-hijri@2.1.2/moment-hijri.min.js"></script>
    
    <style>
        /* إعادة ضبط CSS لدعم اللغة العربية بالكامل */
        :root {
            --primary: #1a5fb4;
            --primary-dark: #1c71d8;
            --secondary: #26a269;
            --secondary-dark: #2ec27e;
            --danger: #c01c28;
            --warning: #e5a50a;
            --dark: #241f31;
            --light: #f6f5f4;
            --white: #ffffff;
            --gray: #deddda;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Noto Naskh Arabic', 'Amiri', serif;
            background-color: #f5f7fa;
            color: var(--dark);
            line-height: 1.8;
            direction: rtl;
            text-align: right;
            min-height: 100vh;
        }
        
        /* إصلاح مشكلة blur */
        body.blurred {
            overflow: hidden;
        }
        
        body.blurred > *:not(.modal-overlay):not(.modal-container) {
            filter: blur(5px);
            pointer-events: none;
            user-select: none;
        }
        
        .modal-overlay {
            position: fixed;
            top: 0;
            right: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            display: none;
            backdrop-filter: blur(0);
            filter: none !important;
        }
        
        .modal-overlay.active {
            display: block;
            animation: fadeIn 0.3s ease;
        }
        
        .modal-container {
            position: fixed;
            top: 50%;
            right: 50%;
            transform: translate(50%, -50%);
            background: var(--white);
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            z-index: 1001;
            min-width: 400px;
            max-width: 90%;
            max-height: 90vh;
            overflow-y: auto;
            filter: none !important;
            backdrop-filter: none !important;
        }
        
        /* الهيدر */
        .header {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
            padding: 1.5rem 2rem;
            box-shadow: 0 4px 12px rgba(26, 95, 180, 0.3);
            position: sticky;
            top: 0;
            z-index: 100;
        }
        
        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 1rem;
        }
        
        .school-info {
            display: flex;
            align-items: center;
            gap: 1rem;
        }
        
        .school-logo {
            width: 60px;
            height: 60px;
            background: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary);
            font-size: 1.8rem;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .school-name h1 {
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 0.3rem;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
        }
        
        .school-name h2 {
            font-size: 1rem;
            opacity: 0.9;
            font-weight: 500;
        }
        
        /* زر Admin */
        .admin-btn {
            background: linear-gradient(135deg, #813d9c, #9a52b3);
            color: white;
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 8px;
            font-family: 'Tajawal', sans-serif;
            font-size: 1rem;
            font-weight: 700;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(129, 61, 156, 0.3);
        }
        
        .admin-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(129, 61, 156, 0.4);
            background: linear-gradient(135deg, #9a52b3, #813d9c);
        }
        
        /* الفصول */
        .class-selector {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 0.8rem;
            margin: 1.5rem auto;
            padding: 1.2rem;
            background: var(--white);
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            max-width: 1200px;
            border: 1px solid var(--gray);
        }
        
        .class-btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 0.8rem 1.2rem;
            border-radius: 8px;
            font-family: 'Tajawal', sans-serif;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            flex: 1;
            min-width: 100px;
            max-width: 150px;
        }
        
        .class-btn:hover {
            background: var(--primary-dark);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(26, 95, 180, 0.3);
        }
        
        .class-btn.active {
            background: var(--secondary);
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(38, 162, 105, 0.4);
        }
        
        /* التاريخ */
        .date-container {
            background: var(--white);
            padding: 1.5rem;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            margin: 1.5rem auto;
            max-width: 1200px;
            border-right: 4px solid var(--secondary);
        }
        
        .date-row {
            display: flex;
            gap: 1.5rem;
            flex-wrap: wrap;
            margin-bottom: 1rem;
        }
        
        .date-group {
            flex: 1;
            min-width: 250px;
        }
        
        .date-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: var(--dark);
            font-size: 1.1rem;
        }
        
        .date-input {
            width: 100%;
            padding: 0.8rem;
            border: 2px solid var(--gray);
            border-radius: 8px;
            font-family: 'Noto Naskh Arabic', serif;
            font-size: 1rem;
            transition: all 0.3s ease;
        }
        
        .date-input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(26, 95, 180, 0.1);
        }
        
        /* الجدول */
        .table-container {
            background: var(--white);
            padding: 1.5rem;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            margin: 1.5rem auto;
            max-width: 1200px;
            overflow-x: auto;
        }
        
        .table-title {
            text-align: center;
            color: var(--primary);
            margin-bottom: 1.5rem;
            padding-bottom: 0.8rem;
            border-bottom: 2px solid var(--gray);
            font-size: 1.4rem;
            font-weight: 700;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            min-width: 700px;
        }
        
        th {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
            padding: 1rem;
            text-align: center;
            font-weight: 700;
            font-size: 1.1rem;
            position: sticky;
            top: 0;
        }
        
        td {
            padding: 1rem;
            border-bottom: 1px solid var(--gray);
            text-align: center;
            transition: background 0.3s ease;
        }
        
        tr:hover td {
            background: #f8f9fa;
        }
        
        .student-name {
            text-align: right;
            font-weight: 600;
            color: var(--dark);
            font-size: 1.1rem;
            padding-right: 1rem;
            border-right: 3px solid var(--primary);
        }
        
        .student-name i {
            cursor: pointer;
            margin-left: 0.5rem;
            color: #ffc107;
            transition: color 0.3s ease;
        }
        
        .student-name i:hover {
            color: #ff9800;
        }
        
        /* أزرار الحضور */
        .attendance-btns {
            display: flex;
            gap: 0.5rem;
            justify-content: center;
        }
        
        .attendance-btn {
            padding: 0.6rem 1.2rem;
            border: none;
            border-radius: 6px;
            font-family: 'Tajawal', sans-serif;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            min-width: 90px;
            justify-content: center;
        }
        
        .attendance-btn.present {
            background: var(--secondary);
            color: white;
        }
        
        .attendance-btn.present:hover,
        .attendance-btn.present.active {
            background: var(--secondary-dark);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(38, 162, 105, 0.3);
        }
        
        .attendance-btn.absent {
            background: var(--danger);
            color: white;
        }
        
        .attendance-btn.absent:hover,
        .attendance-btn.absent.active {
            background: #e01b24;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(192, 28, 40, 0.3);
        }
        
        /* الملاحظات */
        .notes-cell textarea {
            width: 100%;
            padding: 0.8rem;
            border: 1px solid var(--gray);
            border-radius: 6px;
            font-family: 'Noto Naskh Arabic', serif;
            font-size: 0.95rem;
            min-height: 80px;
            resize: vertical;
            line-height: 1.6;
        }
        
        .notes-cell textarea:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(26, 95, 180, 0.1);
        }
        
        /* زر PDF */
        .pdf-btn-container {
            margin: 2rem auto;
            max-width: 1200px;
            text-align: center;
        }
        
        .pdf-btn {
            background: linear-gradient(135deg, #5e35b1, #4527a0);
            color: white;
            border: none;
            padding: 1.2rem 2rem;
            border-radius: 10px;
            font-family: 'Tajawal', sans-serif;
            font-size: 1.3rem;
            font-weight: 800;
            cursor: pointer;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            gap: 1rem;
            box-shadow: 0 6px 15px rgba(94, 53, 177, 0.3);
            width: 100%;
            justify-content: center;
        }
        
        .pdf-btn:hover {
            background: linear-gradient(135deg, #4527a0, #311b92);
            transform: translateY(-4px);
            box-shadow: 0 10px 20px rgba(69, 39, 160, 0.4);
        }
        
        /* نافذة كلمة المرور */
        .password-modal .modal-container {
            text-align: center;
            max-width: 400px;
        }
        
        .password-icon {
            font-size: 3rem;
            color: #813d9c;
            margin-bottom: 1rem;
        }
        
        .password-input {
            width: 100%;
            padding: 1rem;
            margin: 1rem 0;
            border: 2px solid var(--gray);
            border-radius: 8px;
            font-family: 'Tajawal', sans-serif;
            font-size: 1.1rem;
            text-align: center;
            letter-spacing: 2px;
        }
        
        .password-input:focus {
            outline: none;
            border-color: #813d9c;
        }
        
        .password-error {
            color: var(--danger);
            margin: 0.5rem 0;
            font-size: 0.9rem;
            display: none;
        }
        
        /* نافذة Admin */
        .admin-modal .modal-container {
            max-width: 900px;
            min-width: 500px;
        }
        
        .admin-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
            padding-bottom: 1rem;
            border-bottom: 2px solid var(--gray);
        }
        
        .admin-title {
            color: #813d9c;
            font-size: 1.5rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 0.8rem;
        }
        
        .close-btn {
            background: var(--danger);
            color: white;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }
        
        .close-btn:hover {
            background: #e01b24;
            transform: rotate(90deg);
        }
        
        .admin-section {
            background: #f8f9fa;
            padding: 1.5rem;
            border-radius: 8px;
            margin-bottom: 1.5rem;
            border: 1px solid var(--gray);
        }
        
        .section-title {
            color: var(--primary);
            margin-bottom: 1rem;
            font-size: 1.2rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 0.8rem;
        }
        
        /* إشعارات */
        .notification {
            position: fixed;
            top: 20px;
            right: 50%;
            transform: translateX(50%) translateY(-100px);
            background: var(--secondary);
            color: white;
            padding: 1rem 1.5rem;
            border-radius: 8px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            z-index: 2000;
            display: flex;
            align-items: center;
            gap: 0.8rem;
            font-weight: 600;
            transition: transform 0.4s ease;
        }
        
        .notification.show {
            transform: translateX(50%) translateY(0);
        }
        
        .notification.error {
            background: var(--danger);
        }
        
        .notification.warning {
            background: var(--warning);
        }
        
        /* التجاوب */
        @media (max-width: 768px) {
            .header-content {
                flex-direction: column;
                text-align: center;
            }
            
            .school-info {
                flex-direction: column;
            }
            
            .modal-container {
                min-width: 95%;
                padding: 1.5rem;
            }
            
            .date-row {
                flex-direction: column;
            }
            
            .class-selector {
                padding: 1rem;
            }
            
            .class-btn {
                min-width: 80px;
                padding: 0.6rem 1rem;
            }
        }
        
        /* تأثيرات */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes slideUp {
            from { transform: translate(50%, 30px); opacity: 0; }
            to { transform: translate(50%, 0); opacity: 1; }
        }
    </style>
</head>
<body>
    <!-- الهيدر -->
    <header class="header">
        <div class="header-content">
            <div class="school-info">
                <div class="school-logo">
                    <i class="fas fa-graduation-cap"></i>
                </div>
                <div class="school-name">
                    <h1>نظام متابعة طلاب اللغة الإنجليزية</h1>
                    <h2>المعلم: فهد الخالدي - المدرسة السعودية النموذجية</h2>
                </div>
            </div>
            <button class="admin-btn" onclick="showAdminLogin()">
                <i class="fas fa-user-shield"></i>
                <span>دخول المشرف</span>
            </button>
        </div>
    </header>

    <!-- اختيار الفصل -->
    <div class="class-selector">
        <button class="class-btn active" onclick="showClass('الصف الثالث/١')">الصف الثالث/١</button>
        <button class="class-btn" onclick="showClass('الصف الثاني/٣')">الصف الثاني/٣</button>
        <button class="class-btn" onclick="showClass('الصف الثالث/٣')">الصف الثالث/٣</button>
        <button class="class-btn" onclick="showClass('الصف الرابع/٣')">الصف الرابع/٣</button>
        <button class="class-btn" onclick="showClass('الصف الخامس/٣')">الصف الخامس/٣</button>
    </div>

    <!-- التاريخ -->
    <div class="date-container">
        <div class="date-row">
            <div class="date-group">
                <label for="gregorianDate">
                    <i class="fas fa-calendar-alt"></i>
                    التاريخ الميلادي
                </label>
                <input type="date" id="gregorianDate" class="date-input">
            </div>
            <div class="date-group">
                <label for="hijriDate">
                    <i class="fas fa-moon"></i>
                    التاريخ الهجري
                </label>
                <input type="text" id="hijriDate" class="date-input" placeholder="يوم/شهر/سنة هـ (مثال: ١٥/٩/١٤٤٥)">
            </div>
        </div>
        <div style="text-align: center; margin-top: 1rem;">
            <button class="attendance-btn" onclick="setToday()" style="background: var(--warning);">
                <i class="fas fa-calendar-day"></i>
                تعيين تاريخ اليوم
            </button>
        </div>
    </div>

    <!-- جدول الطلاب -->
    <div id="classContent" class="table-container">
        <h3 class="table-title">قائمة طلاب الصف الثالث/١</h3>
        <div style="overflow-x: auto;">
            <table>
                <thead>
                    <tr>
                        <th width="40%">اسم الطالب</th>
                        <th width="20%">الحضور</th>
                        <th width="40%">ملاحظات المعلم</th>
                    </tr>
                </thead>
                <tbody id="studentsTable">
                    <!-- سيتم تعبئته بالجافاسكريبت -->
                </tbody>
            </table>
        </div>
    </div>

    <!-- زر تصدير PDF -->
    <div class="pdf-btn-container">
        <button class="pdf-btn" onclick="generateArabicPDF()">
            <i class="fas fa-file-pdf"></i>
            <span>تصدير التقرير بصيغة PDF</span>
        </button>
    </div>

    <!-- نافذة كلمة المرور -->
    <div class="modal-overlay" id="passwordModal">
        <div class="modal-container">
            <div class="password-icon">
                <i class="fas fa-lock"></i>
            </div>
            <h2 style="color: #813d9c; margin-bottom: 1rem;">الدخول للنظام الإداري</h2>
            <p style="margin-bottom: 1.5rem; color: var(--dark);">الرجاء إدخال كلمة المرور للوصول إلى لوحة التحكم</p>
            
            <input type="password" id="adminPassword" class="password-input" placeholder="أدخل كلمة المرور" autocomplete="off">
            <div class="password-error" id="passwordError">
                <i class="fas fa-exclamation-circle"></i>
                <span>كلمة المرور غير صحيحة، حاول مرة أخرى</span>
            </div>
            
            <div style="display: flex; gap: 1rem; margin-top: 1.5rem;">
                <button class="attendance-btn" onclick="checkAdminPassword()" style="flex: 1; background: #813d9c;">
                    <i class="fas fa-sign-in-alt"></i>
                    دخول
                </button>
                <button class="attendance-btn" onclick="hideAdminLogin()" style="flex: 1; background: var(--gray); color: var(--dark);">
                    <i class="fas fa-times"></i>
                    إلغاء
                </button>
            </div>
        </div>
    </div>

    <!-- نافذة Admin -->
    <div class="modal-overlay" id="adminModal">
        <div class="modal-container">
            <div class="admin-header">
                <h2 class="admin-title">
                    <i class="fas fa-cogs"></i>
                    لوحة التحكم الإدارية
                </h2>
                <button class="close-btn" onclick="hideAdminPanel()">
                    <i class="fas fa-times"></i>
                </button>
            </div>

            <!-- قسم التحضير العشوائي -->
            <div class="admin-section">
                <h3 class="section-title">
                    <i class="fas fa-random"></i>
                    التحضير العشوائي
                </h3>
                <p style="margin-bottom: 1rem;">إنشاء تحضير عشوائي للفصل بنسبة ٧٥٪ حضور و ٢٥٪ غياب</p>
                
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; margin-bottom: 1rem;">
                    <div>
                        <label style="display: block; margin-bottom: 0.5rem; font-weight: 600;">الفصل الدراسي</label>
                        <select id="randomClass" class="date-input">
                            <option value="الصف الثالث/١">الصف الثالث/١</option>
                            <option value="الصف الثاني/٣">الصف الثاني/٣</option>
                            <option value="الصف الثالث/٣">الصف الثالث/٣</option>
                            <option value="الصف الرابع/٣">الصف الرابع/٣</option>
                            <option value="الصف الخامس/٣">الصف الخامس/٣</option>
                        </select>
                    </div>
                    <div>
                        <label style="display: block; margin-bottom: 0.5rem; font-weight: 600;">نسبة الحضور</label>
                        <input type="range" id="attendanceRate" min="50" max="95" value="75" class="date-input" style="padding: 0.5rem;">
                        <div style="text-align: center; font-weight: 600; margin-top: 0.5rem;">
                            <span id="rateValue">٧٥٪</span> حضور
                        </div>
                    </div>
                </div>
                
                <button class="attendance-btn" onclick="generateRandomAttendance()" style="width: 100%; background: var(--warning);">
                    <i class="fas fa-magic"></i>
                    توليد تحضير عشوائي
                </button>
            </div>

            <!-- قسم إدارة الطلاب -->
            <div class="admin-section">
                <h3 class="section-title">
                    <i class="fas fa-users-cog"></i>
                    إدارة الطلاب
                </h3>
                
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem;">
                    <div>
                        <h4 style="margin-bottom: 0.8rem; color: var(--primary);">
                            <i class="fas fa-user-plus"></i>
                            إضافة طالب جديد
                        </h4>
                        <input type="text" id="newStudentName" class="date-input" placeholder="اسم الطالب الكامل" style="margin-bottom: 0.8rem;">
                        <select id="newStudentClass" class="date-input" style="margin-bottom: 0.8rem;">
                            <option value="الصف الثالث/١">الصف الثالث/١</option>
                            <option value="الصف الثاني/٣">الصف الثاني/٣</option>
                            <option value="الصف الثالث/٣">الصف الثالث/٣</option>
                            <option value="الصف الرابع/٣">الصف الرابع/٣</option>
                            <option value="الصف الخامس/٣">الصف الخامس/٣</option>
                        </select>
                        <button class="attendance-btn" onclick="addNewStudent()" style="width: 100%;">
                            <i class="fas fa-plus"></i>
                            إضافة طالب
                        </button>
                    </div>
                    
                    <div>
                        <h4 style="margin-bottom: 0.8rem; color: var(--primary);">
                            <i class="fas fa-exchange-alt"></i>
                            حذف طالب
                        </h4>
                        <select id="deleteStudentSelect" class="date-input" style="margin-bottom: 0.8rem;">
                            <option value="">اختر طالباً للحذف</option>
                        </select>
                        <button class="attendance-btn" onclick="deleteStudent()" style="width: 100%; background: var(--danger);">
                            <i class="fas fa-trash"></i>
                            حذف الطالب
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- إشعارات -->
    <div class="notification" id="notification"></div>

    <script>
        // ============= تهيئة النظام =============
        const ADMIN_PASSWORD = "مدرسة123";
        let currentClass = "الصف الثالث/١";
        let attendanceData = JSON.parse(localStorage.getItem('attendanceData')) || {};
        let studentsData = JSON.parse(localStorage.getItem('studentsData')) || {};
        let starredStudents = JSON.parse(localStorage.getItem('starredStudents')) || {};

        // ============= بيانات الطلاب الافتراضية =============
        const defaultStudents = {
            "الصف الثالث/١": [
                "إسماعيل محمد هاشم شفيق الرحمن",
                "إبراهيم علي أبو بكر محمد",
                "باسم محمد أبو طالب",
                "حسين بشير أمادو جازير",
                "حسين هارون عثمان عبد المؤمن",
                "حمد محمد عثمان بخش",
                "رمضان عيسى باكور محمد",
                "ريان عبد الرحمن موسى جيبو",
                "ريحان محمد مقبول حسين",
                "عامر مولوي حسن شريف"
            ],
            "الصف الثاني/٣": [
                "إبراهيم إدريس إبراهيم أولوجيوم",
                "إدريس محمد حسن أحمد",
                "أمين عبد الله دايابو عثمان",
                "بسام عبد السلام هاشم أنور",
                "حافظ بيلو موسى سليمان",
                "حسين علي حسن مهاوش",
                "خالد طيب إسماعيل محمد",
                "خالد عبد الحميد محمد هاشم",
                "خالد وليد محمد محمد",
                "ريان عبد الرحمن عمر نانتومي"
            ],
            "الصف الثالث/٣": [
                "إبراهيم جزولي أستانور",
                "تركي عبد الصمد عبد الغني",
                "حسام حسن أبو الكلام",
                "حسن عيسى بكوري محمد",
                "سعد سلام ستار إرشاد الله",
                "عايض سيف الإسلام نور أحمد",
                "عزام شمس العالم قاسم علي",
                "عماد محمد صديق محمد",
                "فيصل أحمد أبو بكر محمد",
                "محمد إسحاق محمد إسلام"
            ],
            "الصف الرابع/٣": [
                "إبراهيم عوض أحمد فليس",
                "أحمد إبراهيم ابن زكريا",
                "أحمد عبد القيوم محمد يعقوب",
                "إسماعيل أول أودو محمد",
                "أسامة سعيدو دو غويد",
                "تامر عبد الصمد عبد الغني",
                "تركي هارون حسن شريف",
                "ريان محمد مقبول حسين",
                "ريان هارون الرشيد طفيل",
                "عبد الحليم محمد عبد الله"
            ],
            "الصف الخامس/٣": [
                "إبراهيم خالد سليمان إبراهيم",
                "أنس عبد العزيز نور أحمد",
                "بدر بكر عمر محمد",
                "حمد محمد حسين مياه",
                "رضوان رشيد أحمد نور",
                "سعيد عبد الله سعيد محمد",
                "عامر رحمة الله محمد شفيع",
                "عبد الله حسين علي فليس",
                "عبد العزيز سراج أكبر عثمان",
                "عبد الله عيسى إبراهيم"
            ]
        };

        // ============= إدارة Admin =============
        function showAdminLogin() {
            document.getElementById('passwordModal').classList.add('active');
            document.body.classList.add('blurred');
            document.getElementById('adminPassword').focus();
        }

        function hideAdminLogin() {
            document.getElementById('passwordModal').classList.remove('active');
            document.body.classList.remove('blurred');
            document.getElementById('adminPassword').value = '';
            document.getElementById('passwordError').style.display = 'none';
        }

        function checkAdminPassword() {
            const password = document.getElementById('adminPassword').value;
            const errorElement = document.getElementById('passwordError');
            
            if (password === ADMIN_PASSWORD) {
                hideAdminLogin();
                showAdminPanel();
                showNotification('تم الدخول إلى لوحة التحكم بنجاح', 'success');
            } else {
                errorElement.style.display = 'block';
                document.getElementById('adminPassword').value = '';
                document.getElementById('adminPassword').focus();
                
                // تأثير الاهتزاز
                errorElement.style.animation = 'none';
                setTimeout(() => {
                    errorElement.style.animation = 'shake 0.5s ease';
                }, 10);
            }
        }

        function showAdminPanel() {
            document.getElementById('adminModal').classList.add('active');
            document.body.classList.add('blurred');
            loadAdminData();
        }

        function hideAdminPanel() {
            document.getElementById('adminModal').classList.remove('active');
            document.body.classList.remove('blurred');
        }

        // ============= إدارة الفصول والطلاب =============
        function showClass(className) {
            // تحديث أزرار الفصول
            document.querySelectorAll('.class-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            currentClass = className;
            loadClassStudents(className);
        }

        function loadClassStudents(className) {
            const students = studentsData[className] || defaultStudents[className] || [];
            const tableBody = document.getElementById('studentsTable');
            tableBody.innerHTML = '';
            
            // تحديث عنوان الجدول
            document.querySelector('.table-title').textContent = `قائمة طلاب ${className}`;
            
            // إنشاء صفوف الجدول
            students.forEach((student, index) => {
                const studentId = `${className}-${index}`;
                const isStarred = starredStudents[studentId] || false;
                
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td class="student-name">
                        ${student}
                        <i class="fas fa-star ${isStarred ? 'starred' : ''}" 
                           onclick="toggleStar('${studentId}', this)"
                           style="color: ${isStarred ? '#ffc107' : '#ddd'};">
                        </i>
                    </td>
                    <td>
                        <div class="attendance-btns">
                            <button class="attendance-btn present" 
                                    onclick="markAttendance(this, 'present', '${className}', ${index})">
                                <i class="fas fa-user-check"></i>
                                حاضر
                            </button>
                            <button class="attendance-btn absent" 
                                    onclick="markAttendance(this, 'absent', '${className}', ${index})">
                                <i class="fas fa-user-times"></i>
                                غائب
                            </button>
                        </div>
                    </td>
                    <td class="notes-cell">
                        <textarea placeholder="اكتب ملاحظات المعلم هنا..." 
                                  onchange="saveNote('${className}', ${index}, this.value)">${getNote(className, index) || ''}</textarea>
                    </td>
                `;
                tableBody.appendChild(row);
                
                // تطبيق حالة الحضور المخزنة
                applyStoredAttendance(className, index, row);
            });
        }

        function applyStoredAttendance(className, index, row) {
            const date = document.getElementById('gregorianDate').value;
            if (!date || !attendanceData[date] || !attendanceData[date][className]) return;
            
            const attendance = attendanceData[date][className][index];
            if (attendance) {
                const presentBtn = row.querySelector('.present');
                const absentBtn = row.querySelector('.absent');
                const notesTextarea = row.querySelector('textarea');
                
                if (attendance.status === 'present') {
                    presentBtn.classList.add('active');
                    absentBtn.classList.remove('active');
                } else if (attendance.status === 'absent') {
                    absentBtn.classList.add('active');
                    presentBtn.classList.remove('active');
                }
                
                if (attendance.note) {
                    notesTextarea.value = attendance.note;
                }
            }
        }

        function markAttendance(button, status, className, index) {
            const row = button.closest('tr');
            const buttons = row.querySelectorAll('.attendance-btn');
            
            // إزالة النشاط من جميع الأزرار في الصف
            buttons.forEach(btn => {
                btn.classList.remove('active');
            });
            
            // تفعيل الزر المحدد
            button.classList.add('active');
            
            // حفظ البيانات
            const date = document.getElementById('gregorianDate').value;
            if (date) {
                if (!attendanceData[date]) attendanceData[date] = {};
                if (!attendanceData[date][className]) attendanceData[date][className] = {};
                
                attendanceData[date][className][index] = {
                    status: status,
                    timestamp: new Date().toISOString()
                };
                
                localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
                showNotification(`تم تسجيل ${status === 'present' ? 'الحضور' : 'الغياب'} بنجاح`, 'success');
            }
        }

        function saveNote(className, index, note) {
            const date = document.getElementById('gregorianDate').value;
            if (date) {
                if (!attendanceData[date]) attendanceData[date] = {};
                if (!attendanceData[date][className]) attendanceData[date][className] = {};
                if (!attendanceData[date][className][index]) attendanceData[date][className][index] = {};
                
                attendanceData[date][className][index].note = note;
                localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
            }
        }

        function getNote(className, index) {
            const date = document.getElementById('gregorianDate').value;
            if (date && attendanceData[date] && attendanceData[date][className] && attendanceData[date][className][index]) {
                return attendanceData[date][className][index].note;
            }
            return '';
        }

        // ============= ميزات Admin =============
        function loadAdminData() {
            // تحديث سجل نسب الحضور
            document.getElementById('attendanceRate').addEventListener('input', function() {
                document.getElementById('rateValue').textContent = `${this.value}٪`;
            });
            
            // تحديث قائمة الطلاب للحذف
            updateDeleteStudentList();
        }

        function generateRandomAttendance() {
            const className = document.getElementById('randomClass').value;
            const rate = parseInt(document.getElementById('attendanceRate').value) / 100;
            const date = document.getElementById('gregorianDate').value;
            
            if (!date) {
                showNotification('الرجاء تحديد التاريخ أولاً', 'warning');
                return;
            }
            
            const students = studentsData[className] || defaultStudents[className];
            if (!students) return;
            
            if (!attendanceData[date]) attendanceData[date] = {};
            attendanceData[date][className] = {};
            
            students.forEach((student, index) => {
                const isPresent = Math.random() < rate;
                
                attendanceData[date][className][index] = {
                    status: isPresent ? 'present' : 'absent',
                    note: isPresent ? 'حضور عشوائي' : 'غياب عشوائي',
                    timestamp: new Date().toISOString(),
                    random: true
                };
            });
            
            localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
            
            // إذا كان الفصل المعروض هو نفسه، تحديث العرض
            if (className === currentClass) {
                loadClassStudents(className);
            }
            
            showNotification(`تم إنشاء تحضير عشوائي لـ ${className} بنجاح`, 'success');
        }

        function addNewStudent() {
            const name = document.getElementById('newStudentName').value.trim();
            const className = document.getElementById('newStudentClass').value;
            
            if (!name) {
                showNotification('الرجاء إدخال اسم الطالب', 'warning');
                return;
            }
            
            if (!studentsData[className]) {
                studentsData[className] = defaultStudents[className] ? [...defaultStudents[className]] : [];
            }
            
            studentsData[className].push(name);
            localStorage.setItem('studentsData', JSON.stringify(studentsData));
            
            document.getElementById('newStudentName').value = '';
            updateDeleteStudentList();
            
            if (className === currentClass) {
                loadClassStudents(className);
            }
            
            showNotification(`تم إضافة الطالب "${name}" بنجاح`, 'success');
        }

        function updateDeleteStudentList() {
            const select = document.getElementById('deleteStudentSelect');
            select.innerHTML = '<option value="">اختر طالباً للحذف</option>';
            
            for (const className in studentsData) {
                studentsData[className].forEach((student, index) => {
                    const option = document.createElement('option');
                    option.value = `${className}|${index}`;
                    option.textContent = `${student} (${className})`;
                    select.appendChild(option);
                });
            }
        }

        function deleteStudent() {
            const value = document.getElementById('deleteStudentSelect').value;
            if (!value) {
                showNotification('الرجاء اختيار طالب للحذف', 'warning');
                return;
            }
            
            const [className, index] = value.split('|');
            const studentName = studentsData[className][index];
            
            if (confirm(`هل أنت متأكد من حذف الطالب "${studentName}"؟`)) {
                studentsData[className].splice(index, 1);
                localStorage.setItem('studentsData', JSON.stringify(studentsData));
                
                updateDeleteStudentList();
                
                if (className === currentClass) {
                    loadClassStudents(className);
                }
                
                showNotification(`تم حذف الطالب "${studentName}" بنجاح`, 'success');
            }
        }

        function toggleStar(studentId, element) {
            starredStudents[studentId] = !starredStudents[studentId];
            localStorage.setItem('starredStudents', JSON.stringify(starredStudents));
            
            element.style.color = starredStudents[studentId] ? '#ffc107' : '#ddd';
            element.classList.toggle('starred', starredStudents[studentId]);
            
            showNotification(starredStudents[studentId] ? 'تم تمييز الطالب بنجمة' : 'تم إزالة النجمة', 'success');
        }

        // ============= التاريخ =============
        function setToday() {
            const today = new Date();
            const formattedDate = today.toISOString().split('T')[0];
            document.getElementById('gregorianDate').value = formattedDate;
            convertToHijri(formattedDate);
            showNotification('تم تعيين تاريخ اليوم', 'success');
        }

        function convertToHijri(gregorianDate) {
            if (!gregorianDate) return;
            
            try {
                const [year, month, day] = gregorianDate.split('-');
                const hijri = moment(gregorianDate).format('iYYYY/iM/iD');
                const hijriArabic = hijri.replace(/\d/g, d => '٠١٢٣٤٥٦٧٨٩'[d]);
                document.getElementById('hijriDate').value = hijriArabic;
            } catch (error) {
                console.error('خطأ في تحويل التاريخ:', error);
            }
        }

        document.getElementById('gregorianDate').addEventListener('change', function() {
            convertToHijri(this.value);
            // تحديث عرض الحضور للتاريخ الجديد
            if (currentClass) {
                loadClassStudents(currentClass);
            }
        });

        // ============= تصدير PDF بالعربية =============
        async function generateArabicPDF() {
            try {
                const { jsPDF } = window.jspdf;
                const doc = new jsPDF({
                    orientation: 'landscape',
                    unit: 'mm',
                    format: 'a4'
                });
                
                // معلومات التقرير
                const className = currentClass;
                const gregorianDate = document.getElementById('gregorianDate').value || 'غير محدد';
                const hijriDate = document.getElementById('hijriDate').value || 'غير محدد';
                const todayArabic = new Date().toLocaleDateString('ar-SA');
                
                // عنوان التقرير
                doc.setFont('Noto Naskh Arabic', 'normal');
                doc.setFontSize(24);
                doc.setTextColor(26, 95, 180);
                doc.text('تقرـير متابعة طلاب اللغة الإنجليزية', 148, 20, { align: 'center' });
                
                doc.setFontSize(18);
                doc.setTextColor(38, 162, 105);
                doc.text('المعلم: فهد الخالدي', 148, 30, { align: 'center' });
                
                // معلومات الفصل والتاريخ
                doc.setFontSize(14);
                doc.setTextColor(36, 31, 49);
                doc.text(`الفصل الدراسي: ${className}`, 20, 45);
                doc.text(`التاريخ الميلادي: ${gregorianDate}`, 20, 55);
                doc.text(`التاريخ الهجري: ${hijriDate}`, 20, 65);
                doc.text(`تاريخ الإصدار: ${todayArabic}`, 270, 65, { align: 'right' });
                
                // خط للفصل
                doc.setDrawColor(26, 95, 180);
                doc.setLineWidth(0.5);
                doc.line(20, 70, 275, 70);
                
                // جمع بيانات الطلاب
                const students = studentsData[className] || defaultStudents[className] || [];
                const tableData = [];
                
                students.forEach((student, index) => {
                    const date = gregorianDate;
                    let status = 'لم يتم التسجيل';
                    let note = '';
                    
                    if (date !== 'غير محدد' && attendanceData[date] && attendanceData[date][className] && attendanceData[date][className][index]) {
                        const att = attendanceData[date][className][index];
                        status = att.status === 'present' ? 'حاضر' : 'غائب';
                        note = att.note || '';
                    }
                    
                    tableData.push([
                        (index + 1).toString(),
                        student,
                        status,
                        note || 'لا توجد ملاحظات'
                    ]);
                });
                
                // إنشاء الجدول مع دعم العربية
                doc.autoTable({
                    startY: 75,
                    head: [['م', 'اسم الطالب', 'حالة الحضور', 'ملاحظات المعلم']],
                    body: tableData,
                    theme: 'grid',
                    styles: {
                        font: 'Noto Naskh Arabic',
                        fontSize: 11,
                        textColor: [36, 31, 49],
                        cellPadding: 4,
                        overflow: 'linebreak',
                        halign: 'right'
                    },
                    headStyles: {
                        fillColor: [26, 95, 180],
                        textColor: [255, 255, 255],
                        fontStyle: 'bold',
                        halign: 'center'
                    },
                    columnStyles: {
                        0: { cellWidth: 15, halign: 'center' },
                        1: { cellWidth: 70, halign: 'right' },
                        2: { cellWidth: 30, halign: 'center', fontStyle: 'bold' },
                        3: { cellWidth: 90, halign: 'right' }
                    },
                    didParseCell: function(data) {
                        // تلوين حالة الحضور
                        if (data.column.index === 2 && data.row.index > 0) {
                            const status = data.cell.raw;
                            if (status === 'حاضر') {
                                data.cell.styles.textColor = [38, 162, 105];
                            } else if (status === 'غائب') {
                                data.cell.styles.textColor = [192, 28, 40];
                            }
                        }
                        
                        // تمييز الطلاب المميزين
                        if (data.column.index === 1 && data.row.index > 0) {
                            const student = data.cell.raw;
                            const studentId = `${className}-${data.row.index - 1}`;
                            if (starredStudents[studentId]) {
                                data.cell.styles.fontStyle = 'bold';
                                data.cell.styles.textColor = [255, 193, 7];
                            }
                        }
                    },
                    margin: { right: 20, left: 20 }
                });
                
                // التوقيع
                const finalY = doc.lastAutoTable.finalY + 20;
                doc.setFontSize(12);
                doc.text('توقيع المعلم:', 275, finalY, { align: 'right' });
                doc.line(230, finalY + 2, 270, finalY + 2);
                
                doc.text('ختم المدرسة:', 50, finalY);
                doc.line(20, finalY + 2, 40, finalY + 2);
                
                // تذييل الصفحة
                doc.setFontSize(10);
                doc.setTextColor(128, 128, 128);
                doc.text('نظام متابعة الطلاب - الإصدار ٢.٠ - جميع الحقوق محفوظة © ٢٠٢٤', 148, 200, { align: 'center' });
                
                // حفظ الملف
                const fileName = `تقرير_الحضور_${className}_${gregorianDate}.pdf`;
                doc.save(fileName);
                
                showNotification('تم تصدير التقرير بنجاح بصيغة PDF', 'success');
                
            } catch (error) {
                console.error('خطأ في إنشاء PDF:', error);
                showNotification('حدث خطأ أثناء إنشاء التقرير', 'error');
            }
        }

        // ============= الإشعارات =============
        function showNotification(message, type = 'success') {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type}`;
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // ============= تهيئة النظام =============
        window.onload = function() {
            // تحميل الفصل الأول
            loadClassStudents(currentClass);
            
            // تعيين تاريخ اليوم
            setToday();
            
            // تحميل البيانات من التخزين المحلي
            if (localStorage.getItem('studentsData')) {
                studentsData = JSON.parse(localStorage.getItem('studentsData'));
            }
            
            // إضافة مستمع حدث لكلمة المرور
            document.getElementById('adminPassword').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    checkAdminPassword();
                }
            });
            
            // إضافة تأثير اهتزاز لزر الدخول
            document.querySelector('.admin-btn').addEventListener('click', function() {
                this.style.transform = 'scale(0.95)';
                setTimeout(() => {
                    this.style.transform = 'scale(1)';
                }, 150);
            });
        };
    </script>
</body>
</html>
