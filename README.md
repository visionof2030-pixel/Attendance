<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام متابعة الحضور - اللغة الإنجليزية</title>
    
    <!-- خطوط عربية -->
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@300;400;500;700&family=Amiri:wght@400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- مكتبات -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    
    <style>
        /* إعادة ضبط كاملة */
        :root {
            --primary: #1a73e8;
            --primary-dark: #0d47a1;
            --success: #34a853;
            --success-dark: #2e7d32;
            --danger: #ea4335;
            --danger-dark: #c62828;
            --warning: #fbbc04;
            --admin: #8e24aa;
            --dark: #202124;
            --light: #f8f9fa;
            --white: #ffffff;
            --gray: #dadce0;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Tajawal', 'Amiri', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4e8f0 100%);
            min-height: 100vh;
            color: var(--dark);
            direction: rtl;
            line-height: 1.6;
        }
        
        /* إصلاح كامل لمشكلة blur */
        body.body-blurred {
            overflow: hidden;
        }
        
        body.body-blurred::before {
            content: '';
            position: fixed;
            top: 0;
            right: 0;
            width: 100%;
            height: 100%;
            backdrop-filter: blur(5px);
            z-index: 998;
            pointer-events: none;
        }
        
        /* نافذة مشتركة للنوافذ المنبثقة */
        .modal-overlay {
            position: fixed;
            top: 0;
            right: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.6);
            z-index: 999;
            display: none;
            align-items: center;
            justify-content: center;
            animation: fadeIn 0.3s ease;
        }
        
        .modal-overlay.active {
            display: flex;
        }
        
        .modal-box {
            background: var(--white);
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            overflow: hidden;
            animation: slideUp 0.3s ease;
            max-width: 90%;
            max-height: 90vh;
            overflow-y: auto;
        }
        
        /* الهيدر */
        .header {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
            padding: 1.5rem;
            text-align: center;
            box-shadow: 0 4px 12px rgba(26, 115, 232, 0.3);
            position: sticky;
            top: 0;
            z-index: 100;
        }
        
        .header-content {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        .school-info {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 1rem;
            margin-bottom: 1rem;
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
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .header h1 {
            font-size: 1.8rem;
            margin-bottom: 0.5rem;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
        }
        
        .header h2 {
            font-size: 1.1rem;
            opacity: 0.9;
            font-weight: 400;
        }
        
        .admin-btn {
            background: linear-gradient(135deg, var(--admin), #7b1fa2);
            color: white;
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 700;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            margin-top: 1rem;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(142, 36, 170, 0.3);
        }
        
        .admin-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(142, 36, 170, 0.4);
        }
        
        /* التاريخ */
        .date-section {
            background: white;
            padding: 1.5rem;
            margin: 1rem auto;
            max-width: 500px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            text-align: center;
            border-right: 4px solid var(--primary);
        }
        
        .date-row {
            display: flex;
            gap: 1rem;
            margin-bottom: 1rem;
        }
        
        .date-group {
            flex: 1;
        }
        
        .date-label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: var(--primary-dark);
            text-align: right;
        }
        
        .date-input {
            width: 100%;
            padding: 0.8rem;
            border: 2px solid var(--gray);
            border-radius: 8px;
            font-size: 1rem;
            transition: all 0.3s ease;
            text-align: center;
        }
        
        .date-input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(26, 115, 232, 0.1);
        }
        
        .today-btn {
            background: var(--warning);
            color: var(--dark);
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            margin-top: 0.5rem;
            transition: all 0.3s ease;
        }
        
        .today-btn:hover {
            background: #f9ab00;
            transform: translateY(-2px);
        }
        
        /* جدول الطلاب */
        .students-section {
            background: white;
            padding: 1.5rem;
            margin: 1rem auto;
            max-width: 800px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            overflow-x: auto;
        }
        
        .section-title {
            text-align: center;
            color: var(--primary-dark);
            margin-bottom: 1.5rem;
            padding-bottom: 0.8rem;
            border-bottom: 2px solid var(--gray);
            font-size: 1.4rem;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            min-width: 600px;
        }
        
        th {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
            padding: 1rem;
            text-align: center;
            font-weight: 700;
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
            background: var(--light);
        }
        
        .student-name {
            text-align: right;
            font-weight: 700;
            color: var(--dark);
            padding-right: 1rem;
            position: relative;
        }
        
        .attendance-buttons {
            display: flex;
            gap: 0.5rem;
            justify-content: center;
        }
        
        .attendance-btn {
            padding: 0.6rem 1.2rem;
            border: none;
            border-radius: 6px;
            font-size: 0.95rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            min-width: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }
        
        .attendance-btn.present {
            background: var(--success);
            color: white;
        }
        
        .attendance-btn.present:hover,
        .attendance-btn.present.active {
            background: var(--success-dark);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(52, 168, 83, 0.3);
        }
        
        .attendance-btn.absent {
            background: var(--danger);
            color: white;
        }
        
        .attendance-btn.absent:hover,
        .attendance-btn.absent.active {
            background: var(--danger-dark);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(234, 67, 53, 0.3);
        }
        
        .notes-input {
            width: 100%;
            padding: 0.8rem;
            border: 1px solid var(--gray);
            border-radius: 6px;
            font-family: 'Amiri', serif;
            font-size: 1rem;
            min-height: 80px;
            resize: vertical;
            transition: all 0.3s ease;
        }
        
        .notes-input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(26, 115, 232, 0.1);
        }
        
        /* زر PDF */
        .pdf-section {
            margin: 2rem auto;
            max-width: 500px;
            text-align: center;
        }
        
        .pdf-btn {
            background: linear-gradient(135deg, #673ab7, #5e35b1);
            color: white;
            border: none;
            padding: 1.2rem 2rem;
            border-radius: 10px;
            font-size: 1.3rem;
            font-weight: 700;
            cursor: pointer;
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 1rem;
            transition: all 0.3s ease;
            box-shadow: 0 6px 15px rgba(103, 58, 183, 0.3);
        }
        
        .pdf-btn:hover {
            background: linear-gradient(135deg, #5e35b1, #512da8);
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(94, 53, 177, 0.4);
        }
        
        /* نافذة كلمة المرور */
        .password-modal .modal-box {
            max-width: 400px;
            padding: 2rem;
            text-align: center;
        }
        
        .password-icon {
            font-size: 3rem;
            color: var(--admin);
            margin-bottom: 1rem;
        }
        
        .password-input {
            width: 100%;
            padding: 1rem;
            margin: 1rem 0;
            border: 2px solid var(--gray);
            border-radius: 8px;
            font-size: 1.1rem;
            text-align: center;
            letter-spacing: 2px;
        }
        
        .password-input:focus {
            outline: none;
            border-color: var(--admin);
        }
        
        .password-error {
            color: var(--danger);
            margin: 0.5rem 0;
            font-size: 0.9rem;
            display: none;
            animation: shake 0.5s ease;
        }
        
        .modal-buttons {
            display: flex;
            gap: 1rem;
            margin-top: 1.5rem;
        }
        
        .modal-btn {
            flex: 1;
            padding: 0.8rem;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }
        
        .login-btn {
            background: var(--admin);
            color: white;
        }
        
        .login-btn:hover {
            background: #7b1fa2;
            transform: translateY(-2px);
        }
        
        .cancel-btn {
            background: var(--gray);
            color: var(--dark);
        }
        
        /* نافذة Admin */
        .admin-modal .modal-box {
            max-width: 600px;
            padding: 2rem;
        }
        
        .admin-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 2rem;
            padding-bottom: 1rem;
            border-bottom: 2px solid var(--gray);
        }
        
        .admin-title {
            color: var(--admin);
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
            font-size: 1.2rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }
        
        .close-btn:hover {
            background: var(--danger-dark);
            transform: rotate(90deg);
        }
        
        .admin-section {
            background: var(--light);
            padding: 1.5rem;
            border-radius: 8px;
            margin-bottom: 1.5rem;
            border: 1px solid var(--gray);
        }
        
        .section-subtitle {
            color: var(--primary-dark);
            margin-bottom: 1rem;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            gap: 0.8rem;
        }
        
        .admin-actions {
            display: flex;
            gap: 1rem;
            margin-top: 1rem;
        }
        
        .admin-action-btn {
            flex: 1;
            background: var(--primary);
            color: white;
            border: none;
            padding: 0.8rem;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }
        
        .admin-action-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(26, 115, 232, 0.3);
        }
        
        .admin-action-btn.warning {
            background: var(--warning);
            color: var(--dark);
        }
        
        /* إشعارات */
        .notification {
            position: fixed;
            top: 20px;
            right: 50%;
            transform: translateX(50%) translateY(-100px);
            background: var(--success);
            color: white;
            padding: 1rem 1.5rem;
            border-radius: 8px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            z-index: 1001;
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
            color: var(--dark);
        }
        
        /* التجاوب */
        @media (max-width: 768px) {
            .date-row {
                flex-direction: column;
            }
            
            .modal-box {
                margin: 1rem;
            }
            
            .admin-actions {
                flex-direction: column;
            }
            
            .attendance-buttons {
                flex-direction: column;
            }
            
            .attendance-btn {
                width: 100%;
            }
        }
        
        /* تأثيرات */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes slideUp {
            from { transform: translateY(30px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
            20%, 40%, 60%, 80% { transform: translateX(5px); }
        }
    </style>
</head>
<body>
    <!-- الهيدر -->
    <div class="header">
        <div class="header-content">
            <div class="school-info">
                <div class="school-logo">
                    <i class="fas fa-book-open"></i>
                </div>
                <div>
                    <h1>نظام متابعة الحضور - اللغة الإنجليزية</h1>
                    <h2>المعلم: فهد الخالدي - الفصل الدراسي الأول ١٤٤٥</h2>
                </div>
            </div>
            <button class="admin-btn" onclick="showAdminLogin()">
                <i class="fas fa-user-shield"></i>
                <span>دخول المشرف</span>
            </button>
        </div>
    </div>

    <!-- التاريخ -->
    <div class="date-section">
        <div class="date-row">
            <div class="date-group">
                <label class="date-label">
                    <i class="fas fa-calendar-alt"></i>
                    التاريخ الميلادي
                </label>
                <input type="date" id="dateInput" class="date-input">
            </div>
            <div class="date-group">
                <label class="date-label">
                    <i class="fas fa-moon"></i>
                    التاريخ الهجري
                </label>
                <input type="text" id="hijriInput" class="date-input" placeholder="١٥/٠٩/١٤٤٥">
            </div>
        </div>
        <button class="today-btn" onclick="setToday()">
            <i class="fas fa-calendar-day"></i>
            <span>تعيين تاريخ اليوم</span>
        </button>
    </div>

    <!-- جدول الطلاب -->
    <div class="students-section">
        <h3 class="section-title">قائمة الطلاب - الصف الأول/١</h3>
        <table>
            <thead>
                <tr>
                    <th width="40%">اسم الطالب</th>
                    <th width="25%">حالة الحضور</th>
                    <th width="35%">ملاحظات المعلم</th>
                </tr>
            </thead>
            <tbody id="studentsTable">
                <!-- سيتم تعبئته بالجافاسكريبت -->
            </tbody>
        </table>
    </div>

    <!-- زر تصدير PDF -->
    <div class="pdf-section">
        <button class="pdf-btn" onclick="generatePDF()">
            <i class="fas fa-file-pdf"></i>
            <span>تصدير تقرير الحضور بصيغة PDF</span>
        </button>
    </div>

    <!-- نافذة كلمة المرور -->
    <div class="modal-overlay" id="passwordModal">
        <div class="modal-box">
            <div class="password-icon">
                <i class="fas fa-lock"></i>
            </div>
            <h2 style="color: var(--admin); margin-bottom: 1rem;">الدخول للنظام الإداري</h2>
            <p style="margin-bottom: 1.5rem; color: var(--dark);">الرجاء إدخال كلمة المرور للوصول إلى لوحة التحكم</p>
            
            <input type="password" id="adminPassword" class="password-input" placeholder="أدخل كلمة المرور هنا">
            <div class="password-error" id="passwordError">
                <i class="fas fa-exclamation-circle"></i>
                <span>كلمة المرور غير صحيحة، الرجاء المحاولة مرة أخرى</span>
            </div>
            
            <div class="modal-buttons">
                <button class="modal-btn login-btn" onclick="checkPassword()">
                    <i class="fas fa-sign-in-alt"></i>
                    <span>دخول</span>
                </button>
                <button class="modal-btn cancel-btn" onclick="hideAdminLogin()">
                    <i class="fas fa-times"></i>
                    <span>إلغاء</span>
                </button>
            </div>
        </div>
    </div>

    <!-- نافذة Admin -->
    <div class="modal-overlay" id="adminModal">
        <div class="modal-box">
            <div class="admin-header">
                <h2 class="admin-title">
                    <i class="fas fa-cogs"></i>
                    لوحة التحكم الإدارية
                </h2>
                <button class="close-btn" onclick="hideAdminPanel()">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            
            <div class="admin-section">
                <h3 class="section-subtitle">
                    <i class="fas fa-random"></i>
                    التحضير العشوائي
                </h3>
                <p style="margin-bottom: 1rem;">تعيين حضور عشوائي للطلاب بنسبة محددة</p>
                
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">
                    <div>
                        <label style="display: block; margin-bottom: 0.5rem; font-weight: 600;">نسبة الحضور</label>
                        <select id="attendanceRate" class="date-input" style="text-align: center;">
                            <option value="75">٧٥٪ حضور</option>
                            <option value="80">٨٠٪ حضور</option>
                            <option value="90">٩٠٪ حضور</option>
                        </select>
                    </div>
                    <div>
                        <label style="display: block; margin-bottom: 0.5rem; font-weight: 600;">ملاحظات العشوائية</label>
                        <select id="randomNotes" class="date-input" style="text-align: center;">
                            <option value="حضور عشوائي">حضور عشوائي</option>
                            <option value="غياب عشوائي">غياب عشوائي</option>
                            <option value="توليد آلي">توليد آلي</option>
                        </select>
                    </div>
                </div>
                
                <div class="admin-actions">
                    <button class="admin-action-btn" onclick="generateRandomAttendance()">
                        <i class="fas fa-magic"></i>
                        <span>توليد تحضير عشوائي</span>
                    </button>
                </div>
            </div>
            
            <div class="admin-section">
                <h3 class="section-subtitle">
                    <i class="fas fa-user-plus"></i>
                    إدارة الطلاب
                </h3>
                
                <div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
                    <input type="text" id="newStudentName" class="date-input" placeholder="اسم الطالب الجديد">
                    <button class="admin-action-btn" onclick="addNewStudent()" style="flex: 0 0 auto;">
                        <i class="fas fa-plus"></i>
                        <span>إضافة</span>
                    </button>
                </div>
                
                <div id="currentStudents" style="background: white; padding: 1rem; border-radius: 6px; max-height: 150px; overflow-y: auto;">
                    <!-- قائمة الطلاب الحاليين -->
                </div>
            </div>
            
            <div class="admin-section">
                <h3 class="section-subtitle">
                    <i class="fas fa-broom"></i>
                    تنظيف البيانات
                </h3>
                <p style="margin-bottom: 1rem;">حذف جميع بيانات الحضور لليوم الحالي</p>
                
                <div class="admin-actions">
                    <button class="admin-action-btn warning" onclick="clearTodayData()">
                        <i class="fas fa-trash-alt"></i>
                        <span>حذف بيانات اليوم</span>
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- إشعارات -->
    <div class="notification" id="notification"></div>

    <script>
        // ============= إعدادات النظام =============
        const ADMIN_PASSWORD = "admin123";
        let students = [
            "أحمد محمد علي",
            "سعيد عبد الله حسن"
        ];
        let attendanceData = {};
        let currentDate = "";

        // ============= تهيئة النظام =============
        window.onload = function() {
            // تعيين تاريخ اليوم
            setToday();
            
            // تحميل الطلاب
            loadStudents();
            
            // تحميل البيانات المحفوظة
            loadSavedData();
            
            // إضافة مستمعات الأحداث
            setupEventListeners();
        };

        function setupEventListeners() {
            // كلمة المرور بالإنتر
            document.getElementById('adminPassword').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') checkPassword();
            });
            
            // تغيير التاريخ
            document.getElementById('dateInput').addEventListener('change', function() {
                currentDate = this.value;
                loadStudents();
                convertToHijri();
            });
        }

        function loadSavedData() {
            // تحميل الطلاب من التخزين المحلي
            const savedStudents = localStorage.getItem('attendanceStudents');
            if (savedStudents) {
                students = JSON.parse(savedStudents);
            }
            
            // تحميل بيانات الحضور
            const savedAttendance = localStorage.getItem('attendanceData');
            if (savedAttendance) {
                attendanceData = JSON.parse(savedAttendance);
            }
        }

        function saveData() {
            localStorage.setItem('attendanceStudents', JSON.stringify(students));
            localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
        }

        // ============= إدارة الطلاب =============
        function loadStudents() {
            currentDate = document.getElementById('dateInput').value || currentDate;
            const tableBody = document.getElementById('studentsTable');
            const studentsList = document.getElementById('currentStudents');
            
            tableBody.innerHTML = '';
            studentsList.innerHTML = '<h4 style="margin-bottom: 0.5rem;">الطلاب الحاليين:</h4>';
            
            students.forEach((student, index) => {
                // صف الجدول
                const row = document.createElement('tr');
                const studentKey = `${currentDate}-${index}`;
                const savedData = attendanceData[studentKey] || {};
                
                row.innerHTML = `
                    <td class="student-name">${student}</td>
                    <td>
                        <div class="attendance-buttons">
                            <button class="attendance-btn present ${savedData.status === 'present' ? 'active' : ''}" 
                                    onclick="markAttendance(${index}, 'present')">
                                <i class="fas fa-user-check"></i>
                                حاضر
                            </button>
                            <button class="attendance-btn absent ${savedData.status === 'absent' ? 'active' : ''}" 
                                    onclick="markAttendance(${index}, 'absent')">
                                <i class="fas fa-user-times"></i>
                                غائب
                            </button>
                        </div>
                    </td>
                    <td>
                        <textarea class="notes-input" placeholder="اكتب ملاحظات المعلم هنا..."
                                  onchange="saveNote(${index}, this.value)">${savedData.note || ''}</textarea>
                    </td>
                `;
                tableBody.appendChild(row);
                
                // قائمة الطلاب
                const studentItem = document.createElement('div');
                studentItem.style.cssText = 'padding: 0.5rem; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; align-items: center;';
                studentItem.innerHTML = `
                    <span>${student}</span>
                    <button onclick="removeStudent(${index})" style="background: var(--danger); color: white; border: none; width: 25px; height: 25px; border-radius: 50%; cursor: pointer;">
                        <i class="fas fa-times"></i>
                    </button>
                `;
                studentsList.appendChild(studentItem);
            });
        }

        function markAttendance(studentIndex, status) {
            if (!currentDate) {
                showNotification('الرجاء تحديد التاريخ أولاً', 'error');
                return;
            }
            
            const studentKey = `${currentDate}-${studentIndex}`;
            
            if (!attendanceData[studentKey]) {
                attendanceData[studentKey] = {};
            }
            
            attendanceData[studentKey].status = status;
            attendanceData[studentKey].timestamp = new Date().toISOString();
            
            saveData();
            loadStudents();
            
            showNotification(`تم تسجيل ${status === 'present' ? 'الحضور' : 'الغياب'} للطالب`, 'success');
        }

        function saveNote(studentIndex, note) {
            if (!currentDate) return;
            
            const studentKey = `${currentDate}-${studentIndex}`;
            
            if (!attendanceData[studentKey]) {
                attendanceData[studentKey] = {};
            }
            
            attendanceData[studentKey].note = note;
            saveData();
        }

        function addNewStudent() {
            const nameInput = document.getElementById('newStudentName');
            const name = nameInput.value.trim();
            
            if (!name) {
                showNotification('الرجاء إدخال اسم الطالب', 'error');
                return;
            }
            
            students.push(name);
            nameInput.value = '';
            saveData();
            loadStudents();
            showNotification('تم إضافة الطالب بنجاح', 'success');
        }

        function removeStudent(index) {
            if (confirm('هل أنت متأكد من حذف هذا الطالب؟')) {
                students.splice(index, 1);
                saveData();
                loadStudents();
                showNotification('تم حذف الطالب بنجاح', 'success');
            }
        }

        // ============= التاريخ =============
        function setToday() {
            const today = new Date();
            const year = today.getFullYear();
            const month = String(today.getMonth() + 1).padStart(2, '0');
            const day = String(today.getDate()).padStart(2, '0');
            
            const formattedDate = `${year}-${month}-${day}`;
            document.getElementById('dateInput').value = formattedDate;
            currentDate = formattedDate;
            
            convertToHijri();
            loadStudents();
            showNotification('تم تعيين تاريخ اليوم', 'success');
        }

        function convertToHijri() {
            // تحويل بسيط للتجربة
            const hijriDate = "١٥/٠٩/١٤٤٥";
            document.getElementById('hijriInput').value = hijriDate;
        }

        // ============= إدارة Admin =============
        function showAdminLogin() {
            document.getElementById('passwordModal').classList.add('active');
            document.body.classList.add('body-blurred');
            document.getElementById('adminPassword').focus();
        }

        function hideAdminLogin() {
            document.getElementById('passwordModal').classList.remove('active');
            document.body.classList.remove('body-blurred');
            document.getElementById('adminPassword').value = '';
            document.getElementById('passwordError').style.display = 'none';
        }

        function checkPassword() {
            const password = document.getElementById('adminPassword').value;
            
            if (password === ADMIN_PASSWORD) {
                hideAdminLogin();
                showAdminPanel();
            } else {
                const errorElement = document.getElementById('passwordError');
                errorElement.style.display = 'block';
                document.getElementById('adminPassword').value = '';
                document.getElementById('adminPassword').focus();
                
                // تأثير اهتزاز
                errorElement.style.animation = 'none';
                setTimeout(() => {
                    errorElement.style.animation = 'shake 0.5s ease';
                }, 10);
            }
        }

        function showAdminPanel() {
            document.getElementById('adminModal').classList.add('active');
            document.body.classList.add('body-blurred');
        }

        function hideAdminPanel() {
            document.getElementById('adminModal').classList.remove('active');
            document.body.classList.remove('body-blurred');
        }

        function generateRandomAttendance() {
            if (!currentDate) {
                showNotification('الرجاء تحديد التاريخ أولاً', 'error');
                return;
            }
            
            const rate = parseInt(document.getElementById('attendanceRate').value) / 100;
            const noteType = document.getElementById('randomNotes').value;
            
            students.forEach((student, index) => {
                const studentKey = `${currentDate}-${index}`;
                const isPresent = Math.random() < rate;
                
                if (!attendanceData[studentKey]) {
                    attendanceData[studentKey] = {};
                }
                
                attendanceData[studentKey].status = isPresent ? 'present' : 'absent';
                attendanceData[studentKey].note = isPresent ? noteType : 'غياب عشوائي';
                attendanceData[studentKey].random = true;
                attendanceData[studentKey].timestamp = new Date().toISOString();
            });
            
            saveData();
            loadStudents();
            showNotification(`تم توليد تحضير عشوائي بنسبة ${rate * 100}٪ حضور`, 'success');
        }

        function clearTodayData() {
            if (!currentDate || !confirm('هل أنت متأكد من حذف جميع بيانات الحضور لليوم الحالي؟')) {
                return;
            }
            
            students.forEach((student, index) => {
                const studentKey = `${currentDate}-${index}`;
                delete attendanceData[studentKey];
            });
            
            saveData();
            loadStudents();
            showNotification('تم حذف جميع بيانات اليوم بنجاح', 'success');
        }

        // ============= تصدير PDF بالعربية =============
        async function generatePDF() {
            if (!currentDate) {
                showNotification('الرجاء تحديد التاريخ أولاً', 'error');
                return;
            }
            
            try {
                const { jsPDF } = window.jspdf;
                const doc = new jsPDF({
                    orientation: 'portrait',
                    unit: 'mm',
                    format: 'a4'
                });
                
                // معلومات التقرير
                const hijriDate = document.getElementById('hijriInput').value || 'غير محدد';
                const todayArabic = new Date().toLocaleDateString('ar-SA');
                
                // عنوان التقرير
                doc.setFontSize(20);
                doc.setTextColor(26, 115, 232);
                doc.text('تقرير حضور طلاب اللغة الإنجليزية', 105, 20, { align: 'center' });
                
                doc.setFontSize(16);
                doc.setTextColor(52, 168, 83);
                doc.text('المعلم: فهد الخالدي', 105, 30, { align: 'center' });
                
                doc.setFontSize(12);
                doc.setTextColor(32, 33, 36);
                doc.text(`التاريخ الميلادي: ${currentDate}`, 20, 45);
                doc.text(`التاريخ الهجري: ${hijriDate}`, 20, 55);
                doc.text(`تاريخ الإصدار: ${todayArabic}`, 180, 55, { align: 'right' });
                
                // خط فاصل
                doc.setDrawColor(26, 115, 232);
                doc.setLineWidth(0.5);
                doc.line(20, 60, 190, 60);
                
                // جمع بيانات الطلاب
                const tableData = [];
                let presentCount = 0;
                let absentCount = 0;
                
                students.forEach((student, index) => {
                    const studentKey = `${currentDate}-${index}`;
                    const data = attendanceData[studentKey] || {};
                    const status = data.status === 'present' ? 'حاضر' : data.status === 'absent' ? 'غائب' : 'لم يسجل';
                    const note = data.note || 'لا توجد ملاحظات';
                    
                    if (data.status === 'present') presentCount++;
                    if (data.status === 'absent') absentCount++;
                    
                    tableData.push([
                        (index + 1).toString(),
                        student,
                        status,
                        note
                    ]);
                });
                
                // إنشاء الجدول مع دعم العربية
                doc.autoTable({
                    startY: 65,
                    head: [['م', 'اسم الطالب', 'حالة الحضور', 'ملاحظات المعلم']],
                    body: tableData,
                    theme: 'grid',
                    styles: {
                        font: 'helvetica',
                        fontSize: 10,
                        textColor: [32, 33, 36],
                        cellPadding: 4,
                        overflow: 'linebreak',
                        halign: 'right'
                    },
                    headStyles: {
                        fillColor: [26, 115, 232],
                        textColor: [255, 255, 255],
                        fontStyle: 'bold',
                        halign: 'center'
                    },
                    columnStyles: {
                        0: { cellWidth: 15, halign: 'center' },
                        1: { cellWidth: 60, halign: 'right' },
                        2: { cellWidth: 30, halign: 'center', fontStyle: 'bold' },
                        3: { cellWidth: 70, halign: 'right' }
                    },
                    didParseCell: function(data) {
                        // تلوين حالة الحضور
                        if (data.column.index === 2 && data.row.index > 0) {
                            const status = data.cell.raw;
                            if (status === 'حاضر') {
                                data.cell.styles.textColor = [52, 168, 83];
                            } else if (status === 'غائب') {
                                data.cell.styles.textColor = [234, 67, 53];
                            }
                        }
                    },
                    margin: { right: 20, left: 20 }
                });
                
                // الإحصائيات
                const finalY = doc.lastAutoTable.finalY + 15;
                doc.setFontSize(12);
                doc.setTextColor(32, 33, 36);
                doc.text('إحصائيات الحضور:', 20, finalY);
                
                doc.setFontSize(11);
                doc.text(`عدد الطلاب: ${students.length}`, 20, finalY + 8);
                doc.text(`عدد الحاضرين: ${presentCount}`, 20, finalY + 16);
                doc.text(`عدد الغائبين: ${absentCount}`, 20, finalY + 24);
                
                // نسبة الحضور
                const attendanceRate = students.length > 0 ? ((presentCount / students.length) * 100).toFixed(1) : 0;
                doc.text(`نسبة الحضور: ${attendanceRate}٪`, 20, finalY + 32);
                
                // التوقيع
                const signatureY = finalY + 45;
                doc.setFontSize(10);
                doc.text('توقيع المعلم: ______________', 150, signatureY);
                doc.text('التاريخ: ______________', 150, signatureY + 8);
                
                // تذييل الصفحة
                doc.setFontSize(9);
                doc.setTextColor(128, 128, 128);
                doc.text('نظام متابعة الحضور - جميع الحقوق محفوظة © 2024', 105, 285, { align: 'center' });
                
                // حفظ الملف
                const fileName = `تقرير_حضور_${currentDate}.pdf`;
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
    </script>
</body>
</html>
