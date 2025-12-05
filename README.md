<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام متابعة الحضور</title>
    
    <!-- خط عربي -->
    <link href="https://fonts.googleapis.com/css2?family=Amiri:wght@400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- مكتبات -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Amiri', serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4e8f0 100%);
            min-height: 100vh;
            color: #333;
            direction: rtl;
            text-align: right;
        }
        
        /* الهيدر */
        .header {
            background: linear-gradient(90deg, #0a3a8a, #0d47a1);
            color: white;
            padding: 1rem 2rem;
            text-align: center;
            box-shadow: 0 4px 12px rgba(13, 71, 161, 0.3);
        }
        
        .header h1 {
            font-size: 1.5rem;
            margin-bottom: 0.5rem;
        }
        
        .header h2 {
            font-size: 1rem;
            opacity: 0.9;
        }
        
        /* زر Admin */
        .admin-btn {
            background: #6a1b9a;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 0.5rem;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        /* التاريخ */
        .date-section {
            background: white;
            padding: 1rem;
            margin: 1rem auto;
            max-width: 500px;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            text-align: center;
        }
        
        .date-input {
            width: 100%;
            padding: 0.5rem;
            margin: 0.5rem 0;
            border: 1px solid #ddd;
            border-radius: 5px;
            text-align: center;
            font-size: 1rem;
        }
        
        .today-btn {
            background: #ff9800;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 0.5rem;
        }
        
        /* جدول الطلاب */
        .students-table {
            background: white;
            padding: 1rem;
            margin: 1rem auto;
            max-width: 800px;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            overflow-x: auto;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
        }
        
        th {
            background: #0d47a1;
            color: white;
            padding: 0.8rem;
            text-align: center;
        }
        
        td {
            padding: 0.8rem;
            border-bottom: 1px solid #eee;
            text-align: center;
        }
        
        .student-name {
            text-align: right;
            font-weight: bold;
            padding-right: 1rem;
        }
        
        /* أزرار الحضور */
        .attendance-btn {
            padding: 0.5rem 1rem;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 0 0.2rem;
            min-width: 80px;
        }
        
        .present {
            background: #4caf50;
            color: white;
        }
        
        .present.active {
            background: #2e7d32;
            box-shadow: 0 0 0 2px rgba(76, 175, 80, 0.3);
        }
        
        .absent {
            background: #f44336;
            color: white;
        }
        
        .absent.active {
            background: #c62828;
            box-shadow: 0 0 0 2px rgba(244, 67, 54, 0.3);
        }
        
        /* الملاحظات */
        .notes {
            width: 100%;
            padding: 0.5rem;
            border: 1px solid #ddd;
            border-radius: 5px;
            min-height: 60px;
            font-family: 'Amiri', serif;
        }
        
        /* زر PDF */
        .pdf-btn {
            background: #5e35b1;
            color: white;
            border: none;
            padding: 1rem 2rem;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.2rem;
            margin: 1rem auto;
            display: block;
            max-width: 500px;
            width: 100%;
        }
        
        .pdf-btn:hover {
            background: #4527a0;
        }
        
        /* نافذة كلمة المرور */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            right: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }
        
        .modal.active {
            display: flex;
        }
        
        .modal-content {
            background: white;
            padding: 2rem;
            border-radius: 10px;
            max-width: 400px;
            width: 90%;
            text-align: center;
        }
        
        .modal-content h3 {
            color: #6a1b9a;
            margin-bottom: 1rem;
        }
        
        .password-input {
            width: 100%;
            padding: 0.8rem;
            margin: 1rem 0;
            border: 2px solid #ddd;
            border-radius: 5px;
            text-align: center;
            font-size: 1.1rem;
        }
        
        .modal-buttons {
            display: flex;
            gap: 0.5rem;
            margin-top: 1rem;
        }
        
        .modal-btn {
            flex: 1;
            padding: 0.8rem;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        
        .login-btn {
            background: #6a1b9a;
            color: white;
        }
        
        .cancel-btn {
            background: #757575;
            color: white;
        }
        
        .error {
            color: #f44336;
            margin: 0.5rem 0;
            display: none;
        }
        
        /* نافذة Admin */
        .admin-modal-content {
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
        }
        
        .admin-section {
            background: #f5f5f5;
            padding: 1rem;
            border-radius: 5px;
            margin: 1rem 0;
        }
        
        .admin-section h4 {
            color: #0d47a1;
            margin-bottom: 0.5rem;
        }
        
        .close-btn {
            background: #f44336;
            color: white;
            border: none;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            cursor: pointer;
            float: left;
        }
        
        /* إشعارات */
        .notification {
            position: fixed;
            top: 20px;
            right: 50%;
            transform: translateX(50%) translateY(-100px);
            background: #4caf50;
            color: white;
            padding: 1rem 1.5rem;
            border-radius: 5px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.2);
            z-index: 1001;
            transition: transform 0.3s ease;
        }
        
        .notification.show {
            transform: translateX(50%) translateY(0);
        }
        
        .notification.error {
            background: #f44336;
        }
        
        /* إصلاح مشكلة blur */
        body.blurred {
            overflow: hidden;
        }
        
        body.blurred > *:not(.modal) {
            filter: blur(5px);
        }
        
        .modal {
            filter: none !important;
        }
    </style>
</head>
<body>
    <!-- الهيدر -->
    <div class="header">
        <h1>نظام متابعة الحضور</h1>
        <h2>مادة اللغة الإنجليزية - المعلم: فهد الخالدي</h2>
        <button class="admin-btn" onclick="showAdminLogin()">
            <i class="fas fa-user-shield"></i>
            دخول المشرف
        </button>
    </div>

    <!-- التاريخ -->
    <div class="date-section">
        <h3>تاريخ اليوم</h3>
        <input type="date" id="dateInput" class="date-input">
        <div>
            <button class="today-btn" onclick="setToday()">
                <i class="fas fa-calendar-day"></i>
                تعيين تاريخ اليوم
            </button>
        </div>
    </div>

    <!-- جدول الطلاب -->
    <div class="students-table">
        <h3 style="text-align: center; margin-bottom: 1rem; color: #0d47a1;">قائمة الطلاب</h3>
        <table>
            <thead>
                <tr>
                    <th>اسم الطالب</th>
                    <th>الحضور</th>
                    <th>ملاحظات</th>
                </tr>
            </thead>
            <tbody id="studentsBody">
                <!-- سيتم تعبئته بالجافاسكريبت -->
            </tbody>
        </table>
    </div>

    <!-- زر تصدير PDF -->
    <button class="pdf-btn" onclick="generatePDF()">
        <i class="fas fa-file-pdf"></i>
        تصدير التقرير بصيغة PDF
    </button>

    <!-- نافذة كلمة المرور -->
    <div class="modal" id="passwordModal">
        <div class="modal-content">
            <h3><i class="fas fa-lock"></i> الدخول للنظام الإداري</h3>
            <p>الرجاء إدخال كلمة المرور</p>
            <input type="password" id="adminPassword" class="password-input" placeholder="كلمة المرور">
            <div class="error" id="passwordError">
                <i class="fas fa-exclamation-circle"></i>
                كلمة المرور غير صحيحة
            </div>
            <div class="modal-buttons">
                <button class="modal-btn login-btn" onclick="checkPassword()">دخول</button>
                <button class="modal-btn cancel-btn" onclick="hideAdminLogin()">إلغاء</button>
            </div>
        </div>
    </div>

    <!-- نافذة Admin -->
    <div class="modal" id="adminModal">
        <div class="modal-content admin-modal-content">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem;">
                <h3 style="color: #6a1b9a; margin: 0;">
                    <i class="fas fa-cogs"></i>
                    لوحة التحكم
                </h3>
                <button class="close-btn" onclick="hideAdminPanel()">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            
            <div class="admin-section">
                <h4><i class="fas fa-random"></i> التحضير العشوائي</h4>
                <p>تعيين حضور عشوائي للطلاب</p>
                <div style="display: flex; gap: 0.5rem; margin-top: 0.5rem;">
                    <button onclick="generateRandomAttendance(70)" style="flex:1; background: #4caf50; color: white; border: none; padding: 0.5rem; border-radius: 5px; cursor: pointer;">
                        ٧٠٪ حضور
                    </button>
                    <button onclick="generateRandomAttendance(50)" style="flex:1; background: #ff9800; color: white; border: none; padding: 0.5rem; border-radius: 5px; cursor: pointer;">
                        ٥٠٪ حضور
                    </button>
                </div>
            </div>
            
            <div class="admin-section">
                <h4><i class="fas fa-plus-circle"></i> إضافة طالب جديد</h4>
                <div style="display: flex; gap: 0.5rem;">
                    <input type="text" id="newStudentName" placeholder="اسم الطالب" style="flex:1; padding: 0.5rem; border: 1px solid #ddd; border-radius: 5px;">
                    <button onclick="addStudent()" style="background: #2196f3; color: white; border: none; padding: 0.5rem 1rem; border-radius: 5px; cursor: pointer;">
                        إضافة
                    </button>
                </div>
            </div>
            
            <div class="admin-section">
                <h4><i class="fas fa-users"></i> الطلاب الحاليين</h4>
                <div id="studentsList" style="max-height: 150px; overflow-y: auto;">
                    <!-- قائمة الطلاب -->
                </div>
            </div>
        </div>
    </div>

    <!-- إشعارات -->
    <div class="notification" id="notification"></div>

    <script>
        // ============= تهيئة النظام =============
        const ADMIN_PASSWORD = "1234";
        let students = [
            "أحمد محمد علي",
            "سعيد عبد الله حسن"
        ];
        let attendanceData = {};
        let currentDate = "";

        // ============= تحميل النظام =============
        window.onload = function() {
            setToday();
            loadStudents();
            
            // إضافة مستمع حدث لكلمة المرور
            document.getElementById('adminPassword').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    checkPassword();
                }
            });
            
            // تحديث الحضور عند تغيير التاريخ
            document.getElementById('dateInput').addEventListener('change', function() {
                currentDate = this.value;
                loadStudents();
            });
        };

        // ============= إدارة الطلاب =============
        function loadStudents() {
            const tableBody = document.getElementById('studentsBody');
            currentDate = document.getElementById('dateInput').value;
            
            tableBody.innerHTML = '';
            
            students.forEach((student, index) => {
                const studentId = `${currentDate}-${index}`;
                const storedAttendance = attendanceData[studentId];
                
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td class="student-name">${student}</td>
                    <td>
                        <button class="attendance-btn present ${storedAttendance === 'present' ? 'active' : ''}" 
                                onclick="markAttendance(this, 'present', ${index})">
                            حاضر
                        </button>
                        <button class="attendance-btn absent ${storedAttendance === 'absent' ? 'active' : ''}" 
                                onclick="markAttendance(this, 'absent', ${index})">
                            غائب
                        </button>
                    </td>
                    <td>
                        <textarea class="notes" placeholder="اكتب ملاحظات..." 
                                  onchange="saveNote(${index}, this.value)">${getNote(index) || ''}</textarea>
                    </td>
                `;
                tableBody.appendChild(row);
            });
            
            updateStudentsList();
        }

        function markAttendance(button, status, index) {
            // إزالة النشاط من جميع الأزرار في الصف
            const row = button.closest('tr');
            row.querySelectorAll('.attendance-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // تفعيل الزر المحدد
            button.classList.add('active');
            
            // حفظ الحضور
            const studentId = `${currentDate}-${index}`;
            attendanceData[studentId] = status;
            saveToLocalStorage();
            
            showNotification(`تم تسجيل ${status === 'present' ? 'الحضور' : 'الغياب'}`, 'success');
        }

        function saveNote(index, note) {
            const noteId = `${currentDate}-${index}-note`;
            attendanceData[noteId] = note;
            saveToLocalStorage();
        }

        function getNote(index) {
            const noteId = `${currentDate}-${index}-note`;
            return attendanceData[noteId] || '';
        }

        function addStudent() {
            const nameInput = document.getElementById('newStudentName');
            const name = nameInput.value.trim();
            
            if (name) {
                students.push(name);
                loadStudents();
                nameInput.value = '';
                showNotification('تم إضافة الطالب بنجاح', 'success');
                saveToLocalStorage();
            } else {
                showNotification('الرجاء إدخال اسم الطالب', 'error');
            }
        }

        function updateStudentsList() {
            const list = document.getElementById('studentsList');
            list.innerHTML = '';
            
            students.forEach((student, index) => {
                const div = document.createElement('div');
                div.style.cssText = 'padding: 0.5rem; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; align-items: center;';
                div.innerHTML = `
                    <span>${student}</span>
                    <button onclick="removeStudent(${index})" style="background: #f44336; color: white; border: none; width: 25px; height: 25px; border-radius: 50%; cursor: pointer; font-size: 0.8rem;">
                        <i class="fas fa-times"></i>
                    </button>
                `;
                list.appendChild(div);
            });
        }

        function removeStudent(index) {
            if (confirm('هل أنت متأكد من حذف هذا الطالب؟')) {
                students.splice(index, 1);
                loadStudents();
                showNotification('تم حذف الطالب', 'success');
                saveToLocalStorage();
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
            
            loadStudents();
        }

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

        function checkPassword() {
            const password = document.getElementById('adminPassword').value;
            
            if (password === ADMIN_PASSWORD) {
                hideAdminLogin();
                showAdminPanel();
            } else {
                document.getElementById('passwordError').style.display = 'block';
                document.getElementById('adminPassword').value = '';
                document.getElementById('adminPassword').focus();
            }
        }

        function showAdminPanel() {
            document.getElementById('adminModal').classList.add('active');
            document.body.classList.add('blurred');
        }

        function hideAdminPanel() {
            document.getElementById('adminModal').classList.remove('active');
            document.body.classList.remove('blurred');
        }

        function generateRandomAttendance(percentage) {
            if (!currentDate) {
                showNotification('الرجاء تحديد التاريخ أولاً', 'error');
                return;
            }
            
            students.forEach((student, index) => {
                const studentId = `${currentDate}-${index}`;
                const isPresent = Math.random() * 100 < percentage;
                
                attendanceData[studentId] = isPresent ? 'present' : 'absent';
                attendanceData[`${currentDate}-${index}-note`] = isPresent ? 'حضور عشوائي' : 'غياب عشوائي';
            });
            
            loadStudents();
            showNotification(`تم تعيين حضور عشوائي بنسبة ${percentage}٪`, 'success');
            saveToLocalStorage();
        }

        // ============= PDF بالعربية =============
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
                
                // إضافة نص عربي
                doc.setFontSize(22);
                doc.setTextColor(13, 71, 161);
                doc.text('تقرير حضور طلاب اللغة الإنجليزية', 105, 20, { align: 'center' });
                
                doc.setFontSize(16);
                doc.setTextColor(0, 0, 0);
                doc.text('المعلم: فهد الخالدي', 105, 30, { align: 'center' });
                
                doc.setFontSize(14);
                doc.text(`التاريخ: ${currentDate}`, 105, 40, { align: 'center' });
                
                // خط فاصل
                doc.setDrawColor(0, 0, 0);
                doc.setLineWidth(0.5);
                doc.line(20, 45, 190, 45);
                
                // جدول الطلاب
                const tableData = [];
                let presentCount = 0;
                let absentCount = 0;
                
                students.forEach((student, index) => {
                    const studentId = `${currentDate}-${index}`;
                    const status = attendanceData[studentId] || 'لم يحضر';
                    const note = attendanceData[`${currentDate}-${index}-note`] || '';
                    
                    if (status === 'present') presentCount++;
                    if (status === 'absent') absentCount++;
                    
                    tableData.push([
                        (index + 1).toString(),
                        student,
                        status === 'present' ? 'حاضر' : status === 'absent' ? 'غائب' : 'لم يحضر',
                        note
                    ]);
                });
                
                // إنشاء الجدول
                doc.autoTable({
                    startY: 50,
                    head: [['الرقم', 'اسم الطالب', 'الحالة', 'ملاحظات']],
                    body: tableData,
                    theme: 'grid',
                    styles: {
                        font: 'helvetica',
                        fontSize: 12,
                        textColor: [0, 0, 0],
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
                        0: { cellWidth: 20, halign: 'center' },
                        1: { cellWidth: 50 },
                        2: { cellWidth: 30, halign: 'center' },
                        3: { cellWidth: 70 }
                    },
                    didParseCell: function(data) {
                        // تلوين حالة الحضور
                        if (data.column.index === 2 && data.row.index > 0) {
                            const status = data.cell.raw;
                            if (status === 'حاضر') {
                                data.cell.styles.textColor = [46, 125, 50];
                            } else if (status === 'غائب') {
                                data.cell.styles.textColor = [198, 40, 40];
                            }
                        }
                    }
                });
                
                // الإحصائيات
                const finalY = doc.lastAutoTable.finalY + 15;
                doc.setFontSize(14);
                doc.setTextColor(0, 0, 0);
                doc.text('إحصائيات الحضور:', 20, finalY);
                
                doc.setFontSize(12);
                doc.text(`عدد الطلاب: ${students.length}`, 20, finalY + 10);
                doc.text(`عدد الحاضرين: ${presentCount}`, 20, finalY + 20);
                doc.text(`عدد الغائبين: ${absentCount}`, 20, finalY + 30);
                
                // النسبة المئوية
                const attendanceRate = students.length > 0 ? Math.round((presentCount / students.length) * 100) : 0;
                doc.text(`نسبة الحضور: ${attendanceRate}٪`, 20, finalY + 40);
                
                // التوقيع
                const signatureY = finalY + 60;
                doc.text('توقيع المعلم: ______________', 150, signatureY);
                doc.text('التاريخ: ______________', 150, signatureY + 10);
                
                // حفظ الملف
                const fileName = `تقرير_حضور_${currentDate}.pdf`;
                doc.save(fileName);
                
                showNotification('تم تصدير التقرير بنجاح بصيغة PDF', 'success');
                
            } catch (error) {
                console.error('خطأ في إنشاء PDF:', error);
                showNotification('حدث خطأ أثناء إنشاء التقرير', 'error');
            }
        }

        // ============= التخزين المحلي =============
        function saveToLocalStorage() {
            localStorage.setItem('studentsData', JSON.stringify(students));
            localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
        }

        function loadFromLocalStorage() {
            const savedStudents = localStorage.getItem('studentsData');
            const savedAttendance = localStorage.getItem('attendanceData');
            
            if (savedStudents) {
                students = JSON.parse(savedStudents);
            }
            
            if (savedAttendance) {
                attendanceData = JSON.parse(savedAttendance);
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

        // تحميل البيانات المحفوظة
        loadFromLocalStorage();
    </script>
</body>
</html>
