<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>سجل التقويم الشامل للطلاب</title>

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
        --category-1: #5d4037;
        --category-2: #0277bd;
        --category-3: #6a1b9a;
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
        max-width: 1200px;
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
    
    .summary-box {
        background: linear-gradient(to right, rgba(44, 90, 160, 0.1), rgba(74, 138, 244, 0.1));
        border-radius: 15px;
        padding: 20px;
        margin-bottom: 25px;
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
        gap: 20px;
        border: 2px solid rgba(44, 90, 160, 0.2);
    }
    
    .summary-item {
        text-align: center;
        padding: 15px;
        border-radius: 10px;
        min-width: 150px;
        background: white;
        box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
    }
    
    .summary-count {
        font-size: 2.5rem;
        font-weight: 700;
        margin-bottom: 5px;
    }
    
    .present-count {
        color: var(--present-color);
    }
    
    .absent-count {
        color: var(--absent-color);
    }
    
    .summary-label {
        font-size: 1.1rem;
        color: #555;
    }
    
    /* تصميم الجدول الجديد */
    .table-container {
        overflow-x: auto;
        margin-top: 20px;
        border-radius: 15px;
        box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
    }
    
    table {
        width: 100%;
        border-collapse: separate;
        border-spacing: 0;
        min-width: 1100px;
        font-size: 1.1rem;
    }
    
    th, td {
        padding: 18px 12px;
        text-align: center;
        border: 1px solid var(--border-color);
    }
    
    th {
        background: linear-gradient(to right, #2c5aa0, #3a6bc5);
        color: white;
        font-weight: 700;
        font-size: 1.2rem;
        letter-spacing: 1px;
    }
    
    .category-header {
        background: linear-gradient(to right, var(--category-1), #795548);
    }
    
    .category-2-header {
        background: linear-gradient(to right, var(--category-2), #0288d1);
    }
    
    .category-3-header {
        background: linear-gradient(to right, var(--category-3), #8e24aa);
    }
    
    .sub-header {
        background: rgba(255, 255, 255, 0.15);
        font-size: 1.05rem;
        font-weight: 500;
    }
    
    .student-name {
        font-size: 1.3rem;
        font-weight: 600;
        text-align: right;
        padding-right: 20px;
    }
    
    tr:nth-child(even) {
        background-color: #f9f9f9;
    }
    
    tr:hover {
        background-color: #f0f7ff;
        transition: background-color 0.3s;
    }
    
    /* تصميم خلايا التقييم */
    .evaluation-cell {
        text-align: center;
        padding: 15px;
    }
    
    .status-icon {
        font-size: 2.8rem;
        cursor: pointer;
        transition: all 0.3s ease;
        display: inline-block;
        padding: 8px;
        border-radius: 50%;
        width: 70px;
        height: 70px;
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
        margin-top: 8px;
        font-weight: 700;
        font-size: 1.1rem;
    }
    
    .present-label {
        color: var(--present-color);
    }
    
    .absent-label {
        color: var(--absent-color);
    }
    
    /* أزرار التحكم */
    .controls-container {
        display: flex;
        flex-wrap: wrap;
        gap: 15px;
        margin: 30px 0;
        justify-content: center;
    }
    
    .category-controls {
        background: white;
        border-radius: 15px;
        padding: 20px;
        box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
        flex: 1;
        min-width: 300px;
        border: 2px solid rgba(44, 90, 160, 0.1);
    }
    
    .control-title {
        color: var(--primary-color);
        font-size: 1.4rem;
        margin-bottom: 15px;
        text-align: center;
        padding-bottom: 10px;
        border-bottom: 2px solid #f0f0f0;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
    }
    
    .category-buttons {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
        justify-content: center;
    }
    
    .status-btn {
        padding: 12px 20px;
        border: none;
        border-radius: 10px;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        font-size: 1rem;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        min-width: 140px;
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
    
    .admin-btn {
        background: linear-gradient(to right, #5d4037, #795548);
        color: white;
    }
    
    .admin-btn:hover {
        background: linear-gradient(to right, #3e2723, #4e342e);
    }
    
    .random-btn {
        background: linear-gradient(to right, #f57c00, #ff9800);
        color: white;
        display: none;
    }
    
    .random-btn:hover {
        background: linear-gradient(to right, #e65100, #ef6c00);
    }
    
    .category-1-btn {
        background: linear-gradient(to right, var(--category-1), #795548);
        color: white;
    }
    
    .category-1-btn:hover {
        background: linear-gradient(to right, #3e2723, #4e342e);
    }
    
    .category-2-btn {
        background: linear-gradient(to right, var(--category-2), #0288d1);
        color: white;
    }
    
    .category-2-btn:hover {
        background: linear-gradient(to right, #01579b, #0277bd);
    }
    
    .category-3-btn {
        background: linear-gradient(to right, var(--category-3), #8e24aa);
        color: white;
    }
    
    .category-3-btn:hover {
        background: linear-gradient(to right, #4a148c, #6a1b9a);
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
    
    /* تصميم قسم الإدارة */
    .admin-panel {
        max-height: 0;
        overflow: hidden;
        transition: max-height 0.8s ease, padding 0.8s ease, margin 0.8s ease;
        background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
        border-radius: 15px;
        margin: 0;
        padding: 0 20px;
        border: 2px solid transparent;
    }
    
    .admin-panel.active {
        max-height: 1000px;
        padding: 25px 20px;
        margin-top: 25px;
        border-color: rgba(44, 90, 160, 0.3);
        box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
    }
    
    .admin-panel-content {
        text-align: center;
        opacity: 0;
        transform: translateY(-10px);
        transition: opacity 0.5s ease 0.3s, transform 0.5s ease 0.3s;
    }
    
    .admin-panel.active .admin-panel-content {
        opacity: 1;
        transform: translateY(0);
    }
    
    /* تصميم قسم كلمة المرور */
    .password-section {
        margin-bottom: 25px;
        padding-bottom: 25px;
        border-bottom: 2px dashed #ccc;
    }
    
    .admin-title {
        color: #2c5aa0;
        font-size: 1.5rem;
        margin-bottom: 20px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
    }
    
    .admin-description {
        color: #666;
        margin-bottom: 20px;
        font-size: 1rem;
    }
    
    .password-container {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 15px;
        flex-wrap: wrap;
        margin-bottom: 15px;
    }
    
    .password-input {
        flex: 1;
        max-width: 300px;
        padding: 15px;
        border: 2px solid #ddd;
        border-radius: 10px;
        font-size: 1.1rem;
        font-family: 'Tajawal', sans-serif;
        text-align: center;
        transition: border-color 0.3s;
        min-width: 200px;
    }
    
    .password-input:focus {
        border-color: #2c5aa0;
        outline: none;
    }
    
    .password-buttons {
        display: flex;
        gap: 10px;
    }
    
    .password-submit, .password-cancel {
        padding: 12px 25px;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        font-weight: 700;
        font-size: 1rem;
        transition: all 0.3s ease;
        min-width: 100px;
    }
    
    .password-submit {
        background: linear-gradient(to right, #2c5aa0, #4a8af4);
        color: white;
    }
    
    .password-submit:hover {
        background: linear-gradient(to right, #1e3f7a, #2c5aa0);
    }
    
    .password-cancel {
        background: linear-gradient(to right, #757575, #9e9e9e);
        color: white;
    }
    
    .password-cancel:hover {
        background: linear-gradient(to right, #616161, #757575);
    }
    
    .password-error {
        color: #c62828;
        margin-top: 10px;
        font-size: 0.95rem;
        display: none;
    }
    
    /* تصميم قسم تحديد التاريخ - داخل الإدارة */
    .date-section {
        display: none;
        animation: fadeIn 0.8s ease;
    }
    
    .date-section.active {
        display: block;
    }
    
    .date-title {
        color: #2c5aa0;
        font-size: 1.3rem;
        margin-bottom: 15px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
    }
    
    .date-controls {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 15px;
        flex-wrap: wrap;
        margin-bottom: 15px;
    }
    
    .date-input {
        padding: 12px 15px;
        border: 2px solid #ddd;
        border-radius: 10px;
        font-size: 1.1rem;
        font-family: 'Tajawal', sans-serif;
        text-align: center;
        min-width: 200px;
        transition: border-color 0.3s;
    }
    
    .date-input:focus {
        border-color: var(--primary-color);
        outline: none;
    }
    
    .date-btn {
        padding: 12px 20px;
        border: none;
        border-radius: 10px;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        font-size: 1rem;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        background: linear-gradient(to right, var(--primary-color), #4a8af4);
        color: white;
    }
    
    .date-btn:hover {
        background: linear-gradient(to right, #1e3f7a, #2c5aa0);
        transform: translateY(-2px);
    }
    
    .selected-date-display {
        background: white;
        padding: 10px 20px;
        border-radius: 10px;
        border: 2px solid var(--present-color);
        margin-top: 15px;
        display: inline-block;
        font-weight: 700;
        color: #333;
    }
    
    /* تصميم متجاوب */
    @media (max-width: 1200px) {
        .container {
            margin: 10px;
            padding: 20px;
        }
        
        .table-container {
            border-radius: 10px;
        }
    }
    
    @media (max-width: 768px) {
        h2 {
            font-size: 1.8rem;
        }
        
        .summary-item {
            min-width: 120px;
            padding: 12px;
        }
        
        .summary-count {
            font-size: 2rem;
        }
        
        .status-icon {
            font-size: 2.2rem;
            width: 60px;
            height: 60px;
        }
        
        .status-label {
            font-size: 1rem;
        }
        
        .export {
            min-width: 100%;
            padding: 16px;
        }
        
        .status-btn {
            min-width: 120px;
            padding: 10px 15px;
            font-size: 0.9rem;
        }
        
        .password-container {
            flex-direction: column;
        }
        
        .password-input {
            max-width: 100%;
            min-width: auto;
        }
        
        .date-controls {
            flex-direction: column;
        }
        
        .date-input {
            min-width: 100%;
        }
        
        .category-controls {
            min-width: 100%;
        }
    }
    
    @media (max-width: 480px) {
        .status-icon {
            font-size: 1.8rem;
            width: 50px;
            height: 50px;
        }
        
        .student-name {
            font-size: 1.1rem;
        }
        
        th {
            font-size: 1rem;
            padding: 12px 8px;
        }
        
        td {
            padding: 12px 8px;
        }
    }
    
    /* أنيميشن */
    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-10px); }
        to { opacity: 1; transform: translateY(0); }
    }
    
    @keyframes shake {
        0%, 100% { transform: translateX(0); }
        10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
        20%, 40%, 60%, 80% { transform: translateX(5px); }
    }
    
    @keyframes pulse {
        0% { transform: scale(1); }
        50% { transform: scale(1.1); }
        100% { transform: scale(1); }
    }
    
    .status-icon.pulse {
        animation: pulse 0.5s ease;
    }
    
    /* شارة الحالة الجديدة */
    .status-badge {
        display: inline-block;
        padding: 5px 15px;
        border-radius: 20px;
        font-size: 0.9rem;
        font-weight: 700;
        margin-right: 10px;
    }
    
    .badge-present {
        background-color: rgba(46, 125, 50, 0.15);
        color: var(--present-color);
    }
    
    .badge-absent {
        background-color: rgba(198, 40, 40, 0.15);
        color: var(--absent-color);
    }
    
    /* مصفوفة التقييم */
    .evaluation-matrix {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
        margin: 25px 0;
        gap: 20px;
    }
    
    .matrix-item {
        flex: 1;
        min-width: 200px;
        background: white;
        border-radius: 15px;
        padding: 20px;
        box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
        text-align: center;
        border-top: 5px solid;
    }
    
    .matrix-1 {
        border-color: var(--category-1);
    }
    
    .matrix-2 {
        border-color: var(--category-2);
    }
    
    .matrix-3 {
        border-color: var(--category-3);
    }
    
    .matrix-title {
        font-size: 1.4rem;
        font-weight: 700;
        margin-bottom: 15px;
        color: #333;
    }
    
    .matrix-count {
        font-size: 2.5rem;
        font-weight: 800;
        margin: 10px 0;
    }
    
    .matrix-percent {
        font-size: 1.8rem;
        font-weight: 700;
        color: #666;
        margin-top: 10px;
    }
</style>
</head>
<body>

<div class="container" id="captureArea">
    <h2><i class="fas fa-chart-bar" style="margin-left: 15px;"></i> سجل التقويم الشامل للطلاب</h2>
    
    <div class="summary-box">
        <div class="summary-item">
            <div class="summary-count present-count" id="present-count">8</div>
            <div class="summary-label">طالب حاضر</div>
        </div>
        <div class="summary-item">
            <div class="summary-count absent-count" id="absent-count">0</div>
            <div class="summary-label">طالب غائب</div>
        </div>
        <div class="summary-item">
            <div class="summary-count" style="color: #2c5aa0;" id="total-count">8</div>
            <div class="summary-label">إجمالي الطلاب</div>
        </div>
        <div class="summary-item">
            <div class="summary-count" style="color: #f57c00;" id="attendance-rate">100%</div>
            <div class="summary-label">نسبة الحضور</div>
        </div>
    </div>

    <!-- مصفوفة التقييم -->
    <div class="evaluation-matrix">
        <div class="matrix-item matrix-1">
            <div class="matrix-title">Performance Tasks</div>
            <div class="matrix-count" id="category1-count">100%</div>
            <div class="matrix-percent" id="category1-percent">(16/16)</div>
            <div style="font-size: 0.9rem; color: #666;">مهام الطلاب الأدائية</div>
        </div>
        <div class="matrix-item matrix-2">
            <div class="matrix-title">Participation & Interaction</div>
            <div class="matrix-count" id="category2-count">100%</div>
            <div class="matrix-percent" id="category2-percent">(16/16)</div>
            <div style="font-size: 0.9rem; color: #666;">المشاركة والتفاعل</div>
        </div>
        <div class="matrix-item matrix-3">
            <div class="matrix-title">الحضور</div>
            <div class="matrix-count" id="category3-count">100%</div>
            <div class="matrix-percent" id="category3-percent">(8/8)</div>
            <div style="font-size: 0.9rem; color: #666;">سجل الحضور اليومي</div>
        </div>
    </div>

    <!-- الجدول المعدل -->
    <div class="table-container">
        <table>
            <thead>
                <tr>
                    <th rowspan="2" width="8%">الرقم</th>
                    <th rowspan="2" width="25%">اسم الطالب</th>
                    <th colspan="3" class="category-header">Performance Tasks<br>المهام الأدائية</th>
                    <th colspan="2" class="category-2-header">Participation & Interaction<br>المشاركة والتفاعل</th>
                    <th rowspan="2" width="12%" class="category-3-header">الحضور</th>
                </tr>
                <tr>
                    <th class="sub-header">Assignments<br>الواجبات</th>
                    <th class="sub-header">Projects<br>المشاريع</th>
                    <th class="sub-header">التقييم<br>الإجمالي</th>
                    <th class="sub-header">Classroom Apps & Activities<br>التطبيقات والأنشطة</th>
                    <th class="sub-header">Participation<br>المشاركة</th>
                </tr>
            </thead>
            <tbody>
                <!-- طالب 1 -->
                <tr>
                    <td>1</td>
                    <td class="student-name">أحمد محمد</td>
                    
                    <!-- Performance Tasks -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(1, 'assignments')" id="assignments-1">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-assignments-1">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(1, 'projects')" id="projects-1">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-projects-1">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleCategoryAll(1, 'performance')" id="performance-1">
                            <i class="fas fa-star"></i>
                        </div>
                        <span class="status-label present-label" id="label-performance-1">ممتاز</span>
                    </td>
                    
                    <!-- Participation & Interaction -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(1, 'classroomApps')" id="classroomApps-1">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-classroomApps-1">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(1, 'participation')" id="participation-1">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-participation-1">صح</span>
                    </td>
                    
                    <!-- الحضور -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleAttendance(1)" id="attendance-1">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-attendance-1">حاضر</span>
                    </td>
                </tr>
                
                <!-- طالب 2 -->
                <tr>
                    <td>2</td>
                    <td class="student-name">جسّار فهد</td>
                    
                    <!-- Performance Tasks -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(2, 'assignments')" id="assignments-2">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-assignments-2">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(2, 'projects')" id="projects-2">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-projects-2">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleCategoryAll(2, 'performance')" id="performance-2">
                            <i class="fas fa-star"></i>
                        </div>
                        <span class="status-label present-label" id="label-performance-2">ممتاز</span>
                    </td>
                    
                    <!-- Participation & Interaction -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(2, 'classroomApps')" id="classroomApps-2">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-classroomApps-2">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(2, 'participation')" id="participation-2">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-participation-2">صح</span>
                    </td>
                    
                    <!-- الحضور -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleAttendance(2)" id="attendance-2">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-attendance-2">حاضر</span>
                    </td>
                </tr>
                
                <!-- طالب 3 -->
                <tr>
                    <td>3</td>
                    <td class="student-name">سارة عبدالله</td>
                    
                    <!-- Performance Tasks -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(3, 'assignments')" id="assignments-3">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-assignments-3">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(3, 'projects')" id="projects-3">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-projects-3">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleCategoryAll(3, 'performance')" id="performance-3">
                            <i class="fas fa-star"></i>
                        </div>
                        <span class="status-label present-label" id="label-performance-3">ممتاز</span>
                    </td>
                    
                    <!-- Participation & Interaction -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(3, 'classroomApps')" id="classroomApps-3">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-classroomApps-3">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(3, 'participation')" id="participation-3">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-participation-3">صح</span>
                    </td>
                    
                    <!-- الحضور -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleAttendance(3)" id="attendance-3">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-attendance-3">حاضر</span>
                    </td>
                </tr>
                
                <!-- طالب 4 -->
                <tr>
                    <td>4</td>
                    <td class="student-name">يوسف خالد</td>
                    
                    <!-- Performance Tasks -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(4, 'assignments')" id="assignments-4">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-assignments-4">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(4, 'projects')" id="projects-4">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-projects-4">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleCategoryAll(4, 'performance')" id="performance-4">
                            <i class="fas fa-star"></i>
                        </div>
                        <span class="status-label present-label" id="label-performance-4">ممتاز</span>
                    </td>
                    
                    <!-- Participation & Interaction -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(4, 'classroomApps')" id="classroomApps-4">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-classroomApps-4">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(4, 'participation')" id="participation-4">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-participation-4">صح</span>
                    </td>
                    
                    <!-- الحضور -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleAttendance(4)" id="attendance-4">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-attendance-4">حاضر</span>
                    </td>
                </tr>
                
                <!-- طالب 5 -->
                <tr>
                    <td>5</td>
                    <td class="student-name">نورة سعيد</td>
                    
                    <!-- Performance Tasks -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(5, 'assignments')" id="assignments-5">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-assignments-5">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(5, 'projects')" id="projects-5">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-projects-5">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleCategoryAll(5, 'performance')" id="performance-5">
                            <i class="fas fa-star"></i>
                        </div>
                        <span class="status-label present-label" id="label-performance-5">ممتاز</span>
                    </td>
                    
                    <!-- Participation & Interaction -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(5, 'classroomApps')" id="classroomApps-5">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-classroomApps-5">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(5, 'participation')" id="participation-5">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-participation-5">صح</span>
                    </td>
                    
                    <!-- الحضور -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleAttendance(5)" id="attendance-5">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-attendance-5">حاضر</span>
                    </td>
                </tr>
                
                <!-- طالب 6 -->
                <tr>
                    <td>6</td>
                    <td class="student-name">فارس علي</td>
                    
                    <!-- Performance Tasks -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(6, 'assignments')" id="assignments-6">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-assignments-6">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(6, 'projects')" id="projects-6">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-projects-6">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleCategoryAll(6, 'performance')" id="performance-6">
                            <i class="fas fa-star"></i>
                        </div>
                        <span class="status-label present-label" id="label-performance-6">ممتاز</span>
                    </td>
                    
                    <!-- Participation & Interaction -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(6, 'classroomApps')" id="classroomApps-6">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-classroomApps-6">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(6, 'participation')" id="participation-6">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-participation-6">صح</span>
                    </td>
                    
                    <!-- الحضور -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleAttendance(6)" id="attendance-6">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-attendance-6">حاضر</span>
                    </td>
                </tr>
                
                <!-- طالب 7 -->
                <tr>
                    <td>7</td>
                    <td class="student-name">ليلى حسن</td>
                    
                    <!-- Performance Tasks -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(7, 'assignments')" id="assignments-7">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-assignments-7">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(7, 'projects')" id="projects-7">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-projects-7">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleCategoryAll(7, 'performance')" id="performance-7">
                            <i class="fas fa-star"></i>
                        </div>
                        <span class="status-label present-label" id="label-performance-7">ممتاز</span>
                    </td>
                    
                    <!-- Participation & Interaction -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(7, 'classroomApps')" id="classroomApps-7">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-classroomApps-7">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(7, 'participation')" id="participation-7">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-participation-7">صح</span>
                    </td>
                    
                    <!-- الحضور -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleAttendance(7)" id="attendance-7">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-attendance-7">حاضر</span>
                    </td>
                </tr>
                
                <!-- طالب 8 -->
                <tr>
                    <td>8</td>
                    <td class="student-name">محمد علي</td>
                    
                    <!-- Performance Tasks -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(8, 'assignments')" id="assignments-8">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-assignments-8">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(8, 'projects')" id="projects-8">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-projects-8">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleCategoryAll(8, 'performance')" id="performance-8">
                            <i class="fas fa-star"></i>
                        </div>
                        <span class="status-label present-label" id="label-performance-8">ممتاز</span>
                    </td>
                    
                    <!-- Participation & Interaction -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(8, 'classroomApps')" id="classroomApps-8">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-classroomApps-8">صح</span>
                    </td>
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleEvaluation(8, 'participation')" id="participation-8">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-participation-8">صح</span>
                    </td>
                    
                    <!-- الحضور -->
                    <td class="evaluation-cell">
                        <div class="status-icon present-icon" onclick="toggleAttendance(8)" id="attendance-8">
                            <i class="fas fa-check"></i>
                        </div>
                        <span class="status-label present-label" id="label-attendance-8">حاضر</span>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    
    <!-- أزرار التحكم حسب التصنيف -->
    <div class="controls-container">
        <div class="category-controls">
            <div class="control-title">
                <i class="fas fa-tasks"></i> Performance Tasks
            </div>
            <div class="category-buttons">
                <button class="status-btn category-1-btn" onclick="setAllCategory('performance', 'present')">
                    <i class="fas fa-check"></i> تعيين الكل "صح"
                </button>
                <button class="status-btn absent-btn" onclick="setAllCategory('performance', 'absent')">
                    <i class="fas fa-times"></i> تعيين الكل "خطأ"
                </button>
            </div>
        </div>
        
        <div class="category-controls">
            <div class="control-title">
                <i class="fas fa-comments"></i> Participation & Interaction
            </div>
            <div class="category-buttons">
                <button class="status-btn category-2-btn" onclick="setAllCategory('interaction', 'present')">
                    <i class="fas fa-check"></i> تعيين الكل "صح"
                </button>
                <button class="status-btn absent-btn" onclick="setAllCategory('interaction', 'absent')">
                    <i class="fas fa-times"></i> تعيين الكل "خطأ"
                </button>
            </div>
        </div>
        
        <div class="category-controls">
            <div class="control-title">
                <i class="fas fa-users"></i> الحضور والإدارة
            </div>
            <div class="category-buttons">
                <button class="status-btn present-btn" onclick="setAllAttendance('present')">
                    <i class="fas fa-user-check"></i> تعيين الكل حاضر
                </button>
                <button class="status-btn absent-btn" onclick="setAllAttendance('absent')">
                    <i class="fas fa-user-slash"></i> تعيين الكل غائب
                </button>
                <button class="status-btn admin-btn" onclick="toggleAdminPanel()">
                    <i class="fas fa-cog"></i> إدارة
                </button>
                <button class="status-btn random-btn" id="randomBtn" onclick="toggleRandom()">
                    <i class="fas fa-random"></i> تبديل عشوائي
                </button>
            </div>
        </div>
    </div>
    
    <!-- قسم الإدارة -->
    <div class="admin-panel" id="adminPanel">
        <div class="admin-panel-content">
            <!-- قسم كلمة المرور -->
            <div class="password-section" id="passwordSection">
                <div class="admin-title">
                    <i class="fas fa-lock"></i> التحقق من الهوية
                </div>
                <p class="admin-description">للدخول إلى خيارات الإدارة المتقدمة، يرجى إدخال كلمة المرور:</p>
                <div class="password-container">
                    <input type="password" class="password-input" id="passwordInput" placeholder="أدخل كلمة المرور هنا" autocomplete="off">
                    <div class="password-buttons">
                        <button class="password-submit" onclick="checkPassword()">تحقق</button>
                        <button class="password-cancel" onclick="toggleAdminPanel()">إلغاء</button>
                    </div>
                </div>
                <p class="password-error" id="passwordError">
                    <i class="fas fa-exclamation-triangle"></i> كلمة المرور غير صحيحة
                </p>
            </div>
            
            <!-- قسم تحديد التاريخ -->
            <div class="date-section" id="dateSection">
                <div class="date-title">
                    <i class="fas fa-calendar-alt"></i> تحديد وقت التحضير
                </div>
                <p class="admin-description">يمكنك تحديد تاريخ التقييم للأشهر الماضية أو القادمة:</p>
                <div class="date-controls">
                    <input type="date" class="date-input" id="attendanceDate" value="">
                    <button class="date-btn" onclick="setCustomDate()">
                        <i class="fas fa-check"></i> تطبيق التاريخ
                    </button>
                    <button class="date-btn" onclick="resetToToday()" style="background: linear-gradient(to right, #757575, #9e9e9e);">
                        <i class="fas fa-redo"></i> الرجوع لليوم
                    </button>
                </div>
                <div class="selected-date-display" id="selectedDateDisplay">
                    <i class="far fa-calendar-check"></i> تاريخ التقييم: <span id="currentDateDisplay">اليوم</span>
                </div>
            </div>
        </div>
    </div>
    
    <div class="footer-note">
        <i class="fas fa-info-circle" style="margin-left: 10px;"></i> النظام يعمل افتراضيًا على أن جميع التقييمات إيجابية. انقر على أيقونة أي تصنيف للتبديل بين "صح" و "خطأ". يتم تحديث الإحصائيات تلقائيًا.
    </div>
</div>

<div class="export-container">
    <button class="export" onclick="exportPDF()">
        <i class="fas fa-file-pdf"></i> تصدير سجل التقييم الشامل كملف PDF
    </button>
</div>

<script>
// تخزين حالة التقييمات للطلاب
let studentsData = {};
const totalStudents = 8;

// تهيئة جميع التقييمات كإيجابية (صح)
for (let i = 1; i <= totalStudents; i++) {
    studentsData[i] = {
        attendance: 'present',
        assignments: 'present',
        projects: 'present',
        classroomApps: 'present',
        participation: 'present'
    };
}

// متغير للتحقق من صلاحية الدخول
let isAdminAuthenticated = false;

// متغير لتخزين تاريخ التقييم المحدد
let selectedAttendanceDate = null;

// تهيئة حقل التاريخ بالقيمة الحالية
function initializeDateField() {
    const today = new Date();
    const formattedDate = today.toISOString().split('T')[0];
    document.getElementById('attendanceDate').value = formattedDate;
    updateDateDisplay();
}

// تحديث عرض التاريخ المحدد
function updateDateDisplay() {
    const dateInput = document.getElementById('attendanceDate').value;
    const dateDisplay = document.getElementById('currentDateDisplay');
    
    if (!dateInput) {
        dateDisplay.textContent = 'اليوم';
        selectedAttendanceDate = null;
        return;
    }
    
    const date = new Date(dateInput);
    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
    const formattedDate = date.toLocaleDateString('ar-SA', options);
    dateDisplay.textContent = formattedDate;
    selectedAttendanceDate = dateInput;
}

// تعيين تاريخ مخصص
function setCustomDate() {
    const dateInput = document.getElementById('attendanceDate').value;
    if (!dateInput) {
        showNotification('الرجاء اختيار تاريخ صحيح', 'absent');
        return;
    }
    
    updateDateDisplay();
    showNotification(`تم تعيين تاريخ التقييم إلى ${document.getElementById('currentDateDisplay').textContent}`, 'present');
}

// الرجوع إلى تاريخ اليوم
function resetToToday() {
    const today = new Date();
    const formattedDate = today.toISOString().split('T')[0];
    document.getElementById('attendanceDate').value = formattedDate;
    updateDateDisplay();
    showNotification('تم الرجوع إلى تاريخ اليوم', 'present');
}

// فتح/إغلاق لوحة الإدارة
function toggleAdminPanel() {
    const adminPanel = document.getElementById('adminPanel');
    const adminBtn = document.querySelector('.admin-btn');
    
    if (adminPanel.classList.contains('active')) {
        // إغلاق لوحة الإدارة
        adminPanel.classList.remove('active');
        adminBtn.innerHTML = '<i class="fas fa-cog"></i> إدارة';
        adminBtn.style.background = 'linear-gradient(to right, #5d4037, #795548)';
        
        // إخفاء قسم التاريخ إذا كان ظاهرًا
        document.getElementById('dateSection').classList.remove('active');
        
        // إعادة تعيين كلمة المرور
        document.getElementById('passwordInput').value = '';
        document.getElementById('passwordError').style.display = 'none';
    } else {
        // فتح لوحة الإدارة
        adminPanel.classList.add('active');
        adminBtn.innerHTML = '<i class="fas fa-times"></i> إغلاق الإدارة';
        adminBtn.style.background = 'linear-gradient(to right, #757575, #9e9e9e)';
        
        // إذا تم التحقق مسبقًا، إظهار قسم التاريخ مباشرة
        if (isAdminAuthenticated) {
            document.getElementById('passwordSection').style.display = 'none';
            document.getElementById('dateSection').classList.add('active');
            initializeDateField();
        } else {
            document.getElementById('passwordSection').style.display = 'block';
            document.getElementById('dateSection').classList.remove('active');
            document.getElementById('passwordInput').focus();
        }
    }
}

// التحقق من كلمة المرور
function checkPassword() {
    const password = document.getElementById('passwordInput').value;
    const errorElement = document.getElementById('passwordError');
    const passwordSection = document.getElementById('passwordSection');
    const dateSection = document.getElementById('dateSection');
    
    // كلمة المرور الصحيحة: Jassar1436
    if (password === 'Jassar1436') {
        // تم التحقق بنجاح
        isAdminAuthenticated = true;
        document.getElementById('randomBtn').style.display = 'flex';
        
        // إخفاء قسم كلمة المرور وإظهار قسم التاريخ
        passwordSection.style.display = 'none';
        dateSection.classList.add('active');
        
        // تهيئة حقل التاريخ
        initializeDateField();
        
        // إظهار رسالة نجاح
        showNotification('تم التحقق من الهوية بنجاح! خيارات الإدارة متاحة الآن.', 'present');
        
        // مسح حقل كلمة المرور
        document.getElementById('passwordInput').value = '';
        errorElement.style.display = 'none';
    } else {
        // كلمة مرور خاطئة
        errorElement.style.display = 'block';
        document.getElementById('passwordInput').value = '';
        document.getElementById('passwordInput').focus();
        
        // تأثير اهتزاز
        document.getElementById('passwordInput').style.animation = 'shake 0.5s';
        setTimeout(() => {
            document.getElementById('passwordInput').style.animation = '';
        }, 500);
    }
}

// السماح بإدخال كلمة المرور عند الضغط على Enter
document.getElementById('passwordInput').addEventListener('keypress', function(event) {
    if (event.key === 'Enter') {
        checkPassword();
    }
});

// تبديل تقييم معين للطالب
function toggleEvaluation(studentId, evaluationType) {
    const statusIcon = document.getElementById(`${evaluationType}-${studentId}`);
    const statusLabel = document.getElementById(`label-${evaluationType}-${studentId}`);
    
    // إضافة تأثير النبض
    statusIcon.classList.add('pulse');
    setTimeout(() => {
        statusIcon.classList.remove('pulse');
    }, 500);
    
    // تبديل الحالة
    if (studentsData[studentId][evaluationType] === 'present') {
        // تغيير إلى "خطأ"
        studentsData[studentId][evaluationType] = 'absent';
        statusIcon.className = 'status-icon absent-icon';
        statusIcon.innerHTML = '<i class="fas fa-times"></i>';
        statusLabel.className = 'status-label absent-label';
        statusLabel.textContent = 'خطأ';
    } else {
        // تغيير إلى "صح"
        studentsData[studentId][evaluationType] = 'present';
        statusIcon.className = 'status-icon present-icon';
        statusIcon.innerHTML = '<i class="fas fa-check"></i>';
        statusLabel.className = 'status-label present-label';
        statusLabel.textContent = 'صح';
    }
    
    // تحديث التقييم الإجمالي للتصنيف
    updateCategoryEvaluation(studentId);
    
    // تحديث الإحصائيات
    updateStatistics();
    
    // إشعار بصري
    const studentName = document.querySelector(`tr:nth-child(${studentId}) .student-name`).textContent;
    const evaluationNames = {
        'assignments': 'الواجبات',
        'projects': 'المشاريع',
        'classroomApps': 'التطبيقات والأنشطة',
        'participation': 'المشاركة'
    };
    showNotification(`تم تغيير ${evaluationNames[evaluationType]} لـ ${studentName}`, studentsData[studentId][evaluationType]);
}

// تبديل الحضور للطالب
function toggleAttendance(studentId) {
    const statusIcon = document.getElementById(`attendance-${studentId}`);
    const statusLabel = document.getElementById(`label-attendance-${studentId}`);
    
    // إضافة تأثير النبض
    statusIcon.classList.add('pulse');
    setTimeout(() => {
        statusIcon.classList.remove('pulse');
    }, 500);
    
    // تبديل الحالة
    if (studentsData[studentId].attendance === 'present') {
        // تغيير إلى غائب
        studentsData[studentId].attendance = 'absent';
        statusIcon.className = 'status-icon absent-icon';
        statusIcon.innerHTML = '<i class="fas fa-times"></i>';
        statusLabel.className = 'status-label absent-label';
        statusLabel.textContent = 'غائب';
    } else {
        // تغيير إلى حاضر
        studentsData[studentId].attendance = 'present';
        statusIcon.className = 'status-icon present-icon';
        statusIcon.innerHTML = '<i class="fas fa-check"></i>';
        statusLabel.className = 'status-label present-label';
        statusLabel.textContent = 'حاضر';
    }
    
    // تحديث الإحصائيات
    updateStatistics();
    
    // إشعار بصري
    const studentName = document.querySelector(`tr:nth-child(${studentId}) .student-name`).textContent;
    showNotification(`تم تغيير حالة حضور ${studentName}`, studentsData[studentId].attendance);
}

// تحديث التقييم الإجمالي للتصنيف
function updateCategoryEvaluation(studentId) {
    const performanceIcon = document.getElementById(`performance-${studentId}`);
    const performanceLabel = document.getElementById(`label-performance-${studentId}`);
    
    // حساب تقييم Performance Tasks
    const assignmentsStatus = studentsData[studentId].assignments;
    const projectsStatus = studentsData[studentId].projects;
    
    // إذا كان كلاهما "صح"
    if (assignmentsStatus === 'present' && projectsStatus === 'present') {
        performanceIcon.className = 'status-icon present-icon';
        performanceIcon.innerHTML = '<i class="fas fa-star"></i>';
        performanceLabel.className = 'status-label present-label';
        performanceLabel.textContent = 'ممتاز';
    } 
    // إذا كان أحدهما "خطأ"
    else if (assignmentsStatus === 'absent' && projectsStatus === 'absent') {
        performanceIcon.className = 'status-icon absent-icon';
        performanceIcon.innerHTML = '<i class="fas fa-times"></i>';
        performanceLabel.className = 'status-label absent-label';
        performanceLabel.textContent = 'ضعيف';
    }
    // إذا كان مختلط
    else {
        performanceIcon.className = 'status-icon present-icon';
        performanceIcon.innerHTML = '<i class="fas fa-check"></i>';
        performanceLabel.className = 'status-label present-label';
        performanceLabel.textContent = 'جيد';
    }
}

// تبديل جميع تقييمات التصنيف للطالب
function toggleCategoryAll(studentId, category) {
    if (category === 'performance') {
        // تبديل كل من assignments و projects
        toggleEvaluation(studentId, 'assignments');
        setTimeout(() => {
            toggleEvaluation(studentId, 'projects');
        }, 150);
    }
}

// تعيين جميع تقييمات تصنيف معين
function setAllCategory(category, status) {
    let evaluationTypes = [];
    
    if (category === 'performance') {
        evaluationTypes = ['assignments', 'projects'];
    } else if (category === 'interaction') {
        evaluationTypes = ['classroomApps', 'participation'];
    }
    
    for (let i = 1; i <= totalStudents; i++) {
        evaluationTypes.forEach((type, index) => {
            setTimeout(() => {
                // تعيين الحالة المطلوبة
                studentsData[i][type] = status;
                const statusIcon = document.getElementById(`${type}-${i}`);
                const statusLabel = document.getElementById(`label-${type}-${i}`);
                
                if (status === 'present') {
                    statusIcon.className = 'status-icon present-icon';
                    statusIcon.innerHTML = '<i class="fas fa-check"></i>';
                    statusLabel.className = 'status-label present-label';
                    statusLabel.textContent = 'صح';
                } else {
                    statusIcon.className = 'status-icon absent-icon';
                    statusIcon.innerHTML = '<i class="fas fa-times"></i>';
                    statusLabel.className = 'status-label absent-label';
                    statusLabel.textContent = 'خطأ';
                }
                
                // تحديث التقييم الإجمالي للتصنيف
                if (category === 'performance') {
                    updateCategoryEvaluation(i);
                }
                
                // تأثير لكل أيقونة
                statusIcon.classList.add('pulse');
                setTimeout(() => {
                    statusIcon.classList.remove('pulse');
                }, 500);
            }, i * 100 + index * 50);
        });
    }
    
    // تحديث الإحصائيات بعد التحديث
    setTimeout(() => {
        updateStatistics();
        const categoryNames = {
            'performance': 'المهام الأدائية',
            'interaction': 'المشاركة والتفاعل'
        };
        const statusNames = {
            'present': '"صح"',
            'absent': '"خطأ"'
        };
        showNotification(`تم تعيين جميع ${categoryNames[category]} كـ ${statusNames[status]}`, status);
    }, totalStudents * 150);
}

// تعيين جميع الحضور
function setAllAttendance(status) {
    for (let i = 1; i <= totalStudents; i++) {
        setTimeout(() => {
            studentsData[i].attendance = status;
            const statusIcon.getElementById(`attendance-${i}`);
            const statusLabel = document.getElementById(`label-attendance-${i}`);
            
            if (status === 'present') {
                statusIcon.className = 'status-icon present-icon';
                statusIcon.innerHTML = '<i class="fas fa-check"></i>';
                statusLabel.className = 'status-label present-label = document.getElementById(`attendance-${i}`);
            const statusLabel = document.getElementById(`label-attendance-${i}`);
            
            if (status === 'present') {
                statusIcon.className = 'status-icon present-icon';
                statusIcon.innerHTML = '<i class="fas fa-check"></i>';
                statusLabel.className = 'status-label present';
                statusLabel.textContent = 'حاضر';
            } else {
                statusIcon.className = 'status-icon absent-icon-label';
                statusLabel.textContent = 'حاضر';
            } else {
                statusIcon.className = 'status-icon absent-icon';
                statusIcon';
                statusIcon.innerHTML = '<i class="fas fa-times"></i>';
                statusLabel.className = 'status-label absent-label';
                statusLabel.innerHTML = '<i class="fas fa-times"></i>';
                statusLabel.className = 'status-label absent-label';
                statusLabel.textContent.textContent = 'غائب';
            }
            
            // تأثير لكل أيقونة
            statusIcon.classList.add('pulse');
            setTimeout(() => = 'غائب';
            }
            
            // تأثير لكل أيقونة
            statusIcon.classList.add('pulse');
            setTimeout(() => {
                statusIcon.classList.remove('pulse');
            }, 500);
        }, i * 80);
    }
    
    // تحديث الإحصائيات {
                statusIcon.classList.remove('pulse');
            }, 500);
        }, i * 80);
    }
    
    // تحديث الإح
    setTimeout(() => {
        updateStatistics();
        const statusNames = {
            'present': 'حاضرين',
            'absent': 'غصائيات
    setTimeout(() => {
        updateStatistics();
        const statusNames = {
            'present': 'حاضرين',
            'absent':ائبين'
        };
        showNotification(`تم تعيين جميع الطلاب كـ ${statusNames[status]}`, status);
    }, totalStudents 'غائبين'
        };
        showNotification(`تم تعيين جميع الطلاب كـ ${statusNames[status]}`, status);
    }, totalStudents * 100);
}

// تبديل عشوائي لت * 100);
}

// تبديل عشوائي لتقييمقييمات بعض الطلاب
function toggleRandom() {
    // التحقق من صلاحية الدخول
    if (!isAdminAuthenticated) {
        showNotification('يجبات بعض الطلاب
function toggleRandom() {
    // التحقق من صلاحية الدخول
    if (!isAdminAuthenticated) {
 التحقق من الهوية أولاً!', 'absent');
        toggleAdminPanel();
        return;
    }
    
        showNotification('يجب التحقق من الهوية أولاً!', 'absent');
        toggleAdminPanel();
        return;
    }
    
    // تغ    // تغيير 3-4 طلاب عشوائيًا في تقييمات مختلفة
    const changes = Math.floor(Math.random() * 3) + يير 3-4 طلاب عشوائيًا في تقييمات مختلفة
    const changes = Math.floor(Math.random() * 3) + 2;2; // بين 2 و 4 تغييرات
    
    for (let i = 0; i < changes; i++) {
 // بين 2 و 4 تغييرات
    
    for (let i = 0; i < changes; i++) {
               const randomStudent = Math.floor(Math.random() * totalStudents) + 1;
        const evaluationTypes = ['assignments', 'projects', 'classroom const randomStudent = Math.floor(Math.random() * totalStudents) + 1;
        const evaluationTypes = ['assignments', 'projects', 'classroomApps', 'partApps', 'participation', 'attendance'];
        const randomType = evaluationTypes[Math.floor(Math.random() * evaluationTypes.length)];
        
icipation', 'attendance'];
        const randomType = evaluationTypes[Math.floor(Math.random() * evaluationTypes.length)];
        
        setTimeout(() => {
            if (randomType === 'attendance') {
                toggleAttendance(randomStudent);
            } else {
                toggleEvaluation(randomStudent, randomType);
            }
               setTimeout(() => {
            if (randomType === 'attendance') {
                toggleAttendance(randomStudent);
            } else {
                toggleEvaluation(randomStudent, randomType);
            }
        }, i * 300);
    }
    
    showNotification(`تم تبديل }, i * 300);
    }
 تقييمات ${changes} طلاب عشوائيًا`, 'present');
}

// تحديث الإحصائيات
function updateStatistics() {
    let presentCount = 0    
    showNotification(`تم تبديل تقييمات ${changes} طلاب عشوائيًا`, 'present');
}

// تحديث الإحصائيات
function updateStatistics() {
    let presentCount = 0;
    let absent;
    let absentCount = 0;
    
    // حساب عدد الحاضرين والغائبين
    for (let i = 1; i <= totalCount = 0;
    
    // حساب عدد الحاضرين والغائبين
    for (let i = 1; i <= totalStudents; i++) {
        if (studentsData[i].attendance === 'present') {
            presentCount++;
        } else {
            absentCount++;
        }
    }
    
    // تحديث أرقام الإحصائيات
    document.getElementById('present-count').textContent = presentCount;
    document.getElementById('absent-count').textContent = absentCount;
    document.getElementById('total-count').textContent = totalStudents;
    
    // حساب نسبة الحضStudents; i++) {
        if (studentsData[i].attendance === 'present') {
            presentCount++;
        } else {
            absentCount++;
        }
    }
    
    // تحديث أرقام الإحصائيات
    document.getElementById('present-count').textContent = presentCount;
    document.getElementById('absent-count').textContent = absentCount;
    document.getElementById('total-count').textContent = totalStudents;
    
    // حساب نسبة الحور
    const attendanceRate = Math.round((presentCount / totalStudents) * 100);
    document.getElementById('attendance-rate').textContent = `${attendanceRate}%`;
    
    // تحديث إحصائيات التصنيفات
    updateCategoryStatistics();
}

// تحديث إحصائيات التصنيفات
function updateCategoryStatistics() {
    // Performance Tasks
    let performancePresent = 0;
    let performanceTotal = totalStudents * 2; // assignments + projects لكل طالب
    
    for (let i = 1; i <= totalStudents; i++) {
        if (studentsData[i].assignments === 'present') performancePresent++;
        if (studentsData[i].projects === 'present') performancePresent++;
    }
    
    const performanceRate = Math.round((performancePresentضور
    const attendanceRate = Math.round((presentCount / totalStudents) * 100);
    document.getElementById('attendance-rate').textContent = `${attendanceRate}%`;
    
    // تحديث إحصائيات التصنيفات
    updateCategoryStatistics();
}

// تحديث إحصائيات التصنيفات
function updateCategoryStatistics() {
    // Performance Tasks
    let performancePresent = 0;
    let performanceTotal = totalStudents * 2; // assignments + projects لكل طالب
    
    for (let i = 1; i <= totalStudents; i++) {
        if (studentsData[i].assignments === 'present') performancePresent++;
        if (studentsData[i].projects === 'present') performancePresent++;
    }
    
    const performanceRate = Math.round((performancePresent / performanceTotal) * 100 / performanceTotal) * 100);
    document.getElementById('category1-count').textContent = `${performanceRate}%`;
    document.getElementById('category1-percent').textContent = `(${performancePresent);
    document.getElementById('category1-count').textContent = `${performanceRate}%`;
    document.getElementById('category1-percent').textContent = `(${performancePresent}/${performanceTotal})`;
    
    // Participation & Interaction
    let interactionPresent = 0;
    let interactionTotal = totalStudents * 2; // classroomApps + participation لكل ط}/${performanceTotal})`;
    
    // Participation & Interaction
    let interactionPresent = 0;
    let interactionTotal = totalStudents * 2; // classroomالب
    
    for (let i = 1; i <= totalStudents; i++) {
        if (studentsData[i].classroomApps === 'present') interactionApps + participation لكل طالب
    
    for (let i = 1; i <= totalStudents; i++) {
        if (Present++;
        if (studentsData[i].participation === 'present') interactionPresent++;
    }
    
    const interactionRate = Math.round((interstudentsData[i].classroomApps === 'present') interactionPresent++;
        if (studentsData[i].participation === 'present') interactionPresent++;
    }
    
    const interactionRate = Math.round((interactionPresent / interactionTotal) * 100);
    document.getElementById('category2-countactionPresent / interactionTotal) * 100);
    document.getElementById('category2-count').textContent = `${inter').textContent = `${interactionRate}%`;
    document.getElementById('category2-percent').textContent = `(${interactionPresent}/${interactionactionRate}%`;
    document.getElementById('category2-percent').textContent = `(${interactionPresent}/${Total})`;
    
    // الحضور
    let attendancePresent = 0;
    
    for (let i = 1; i <= totalStudents; i++) {
        ifinteractionTotal})`;
    
    // الحضور
    let attendancePresent = 0;
    
    for (let i = 1; i <= totalStudents; i++) {
        (studentsData[i].attendance === 'present') attendancePresent++;
    }
    
    const attendanceRate = Math.round((attendancePresent / totalStudents) * 100);
    document if (studentsData[i].attendance === 'present') attendancePresent++;
    }
    
    const attendanceRate = Math.round((attendancePresent / totalStudents) * 100);
    document.getElementById('category3-count').textContent = `${attendanceRate}%`;
    document.getElementById('category3-percent').textContent = `(${.getElementById('category3-count').textContent = `${attendanceRate}%`;
    document.getElementById('category3-percent').textContent = `($attendancePresent}/${totalStudents})`;
}

// إظهار إشعار مؤقت
function showNotification(message{attendancePresent}/${totalStudents})`;
}

// إظهار إشعار مؤقت
function showNotification(message, type) {
    // إزالة أي إشعارات سابقة
    const existingNotification =, type) {
    // إزالة أي إشعارات سابقة
    const existingNotification = document.querySelector document.querySelector('.custom-notification');
    if (existingNotification) {
        existingNotification.remove();
    }
    
    // إنشاء إشعار جديد
    const notification = document.createElement('('.custom-notification');
    if (existingNotification) {
        existingNotification.remove();
    }
    
    // إنشاء إشعار جديد
    const notification = document.createElement('div');
div');
    notification.className = 'custom-notification';
    notification.textContent = message;
    notification.style.cssText = `
        position: fixed;
        top: 25px;
        right: 25    notification.className = 'custom-notification';
    notification.textContent = message;
    notification.style.cssText = `
        position: fixed;
        top: 25px;
        right:px;
        background: ${type === 'present' ? 'var(--present-color)' : 'var(--absent-color)'};
        color: white;
 25px;
        background: ${type === 'present' ? 'var(--present-color)' : 'var(--absent-color)'};
        color: white;
               padding: 15px 25px;
        border-radius: 10px;
        z-index: 1000;
        font-weight: bold;
        box-shadow padding: 15px 25px;
        border-radius: 10px;
        z-index: 1000;
        font-weight: bold;
: 0 8px 20px rgba(0,0,0,0.2);
        animation: fadeInOut 3s ease-in-out;
        font-size:         box-shadow: 0 8px 20px rgba(0,0,0,0.2);
        animation: fadeInOut 3s ease-in-out;
       1.1rem;
        display: flex;
        align-items: center;
        gap: 10px;
    `;
    
    // إ font-size: 1.1rem;
        display: flex;
        align-items: center;
        gap: 10px;
    `;
    
    // إضافة أيقونة للإشعار
    const icon = document.createElement('i');
    icon.classضافة أيقونة للإشعار
    const icon = document.createElement('i');
    icon.className = typeName = type === 'present' ? 'fas fa-check-circle' : 'fas fa-times-circle';
    notification.prepend(icon);
    
    document.body.appendChild(notification);
    
 === 'present' ? 'fas fa-check-circle' : 'fas fa-times-circle';
    notification.prepend(icon);
    
    document.body.appendChild    // إزالة الإشعار بعد 3 ثوان
    setTimeout(() => {
        notification.style.animation = 'fadeOut (notification);
    
    // إزالة الإشعار بعد 3 ثوان
    setTimeout(() => {
        notification.style.animation = 'fadeOut 0.50.5s ease-out forwards';
s ease-out forwards';
        setTimeout(() => {
            if (notification.parentNode) {
                notification.parentNode.removeChild(notification);
            }
        }, 500);
    }, 2500);
}

// إضافة أنيميشن للإ        setTimeout(() => {
            if (notification.parentNode) {
                notification.parentNode.removeChild(notification);
            }
        }, 500);
    }, 2500);
}

// إضافة أنيميشن للإششعارات
const style = document.createElement('style');
style.textContent = `
    @keyframes fadeInOut {
        0% { opacity: 0; transform: translateY(-عارات
const style = document.createElement('style');
style.textContent = `
    @keyframes fadeInOut {
        0% { opacity: 0; transform: translateY(-25px); }
25px); }
        15%        15% { opacity: 1; transform: translateY(0); }
        85% { opacity: 1; transform: translateY(0); }
        100% { opacity: 0; transform: translateY { opacity: 1; transform: translateY(0); }
        85% { opacity: 1; transform: translateY(0); }
        100% { opacity: 0; transform: translateY(-(-25px); }
    }
    @keyframes fadeOut {
        from { opacity: 1; }
        to { opacity: 0; }
    }
`;
document.head.appendChild(25px); }
    }
    @keyframes fadeOut {
        from { opacity: 1; }
        to { opacity: 0; }
    }
`;
document.head.appendChild(stylestyle);

// تحديث الإحصائيات عند تحميل الصفحة
window.addEventListener('load', () => {
    updateStatistics();
});

// د);

// تحديث الإحصائيات عند تحميل الصفحة
window.addEventListener('load', () => {
    updateStatistics();
});

// دالة تصدير PDF
async function exportPDF() {
    const { jsPDF } = window.jspdf;
    
    // تغيير نص زر التصدير مؤقتًا
   الة تصدير PDF
async function exportPDF() {
    const { jsPDF } = window.jspdf;
    
    // تغيير نص زر التصدير مؤقتًا
 const exportBtn = document.querySelector('.export');
    const originalText = exportBtn.innerHTML;
    exportBtn.innerHTML = '<i class="fas fa-spinner    const exportBtn = document.querySelector('.export');
    const originalText = exportBtn.innerHTML;
    exportBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> fa-spin"></i> جاري التصدير...';
    exportBtn.disabled = true;
    
    // إضافة تاريخ التصدير والتاريخ المحدد
 جاري التصدير...';
    exportBtn.disabled = true;
    
    // إضافة تاريخ التصدير والتاريخ المحدد
    const dateElement = document.createElement('div');
    
    // تحضير نص التاريخ
    let dateText    const dateElement = document.createElement('div');
    
    // تحضير نص التاريخ
    let dateText = '';
    if (selectedAttendanceDate) {
        const date = new Date(selectedAttendanceDate);
        const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
        const formattedDate = date = '';
    if (selectedAttendanceDate) {
        const date = new Date(selectedAttendanceDate);
        const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
        const formattedDate = date.toLocaleDateString('ar-SA', options);
        dateText = `تاريخ التقييم: ${formattedDate}`;
    } else {
        date.toLocaleDateString('ar-SA', options);
        dateText = `تاريخ التقييم: ${formattedDate}`;
    } else {
        dateText = `تاريخ التقيText = `تاريخ التقييم: اليوم (${new Date().toLocaleDateString('ar-SA')})`;
    }
    
    dateElement.style.cssText = 'text-alignيم: اليوم (${new Date().toLocaleDateString('ar-SA')})`;
    }
    
    dateElement.style.css: left; margin-bottom: 20px; color: #666; font-size: 1rem; padding: 10px 15px; background: #f8f9fa;Text = 'text-align: left; margin-bottom: 20px; color: #666; font-size: 1rem; padding: 10px 15px; background: #f8f border-radius: 8px; border-right: 4px solid #2c5aa0;';
    dateElement.innerHTML = `<i class="far fa-calendar-alt" style9fa; border-radius: 8px; border-right: 4px solid #2c5aa0;';
    dateElement.innerHTML = `<i class="far fa-calendar="margin-left: 8px;"></i> ${dateText} - <i class="far fa-clock" style="margin-left: 8px;"></i> وقت-alt" style="margin-left: 8px;"></i> ${dateText} - <i class="far fa-clock" style="margin-left: 8px التصدير: ${new Date().toLocaleTimeString('ar-SA', {hour: '2-digit', minute:'2-digit'})}`;
;"></i> وقت التصدير: ${new Date().toLocaleTimeString('ar-SA', {hour: '2-digit', minute:'2-digit'})}`;
    
    const    
    const captureArea = document.getElementById('captureArea');
    const originalContent = captureArea.innerHTML;
    
    // إضافة التاريخ أعلى المحتوى
    captureArea.insertBefore captureArea = document.getElementById('captureArea');
    const originalContent = captureArea.innerHTML;
    
    // إضافة التاريخ أعلى المحتوى
    captureArea.insertBefore(dateElement, capture(dateElement, captureArea.firstChild);
    
    const canvas = await html2canvas(captureArea, { 
        scale: 2,
        useCORS: true,
        backgroundColor:Area.firstChild);
    
    const canvas = await html2canvas(captureArea, { 
        scale: 2,
        useCORS: '#ffffff'
    });

    // إعادة المحتوى الأصلي
    captureArea.removeChild(dateElement);
    
    const imgData = canvas.toDataURL("image/png");
 true,
        backgroundColor: '#ffffff'
    });

    // إعادة المحتوى الأصلي
    captureArea.removeChild(dateElement);
    
    const imgData = canvas.toDataURL    const pdf = new jsPDF("p", "mm", "a4");

    let width = pdf.internal.pageSize.getWidth();
    let("image/png");
    const pdf = new jsPDF("p", "mm", "a4");

    let width = pdf.internal.pageSize.getWidth();
    let height = (canvas height = (canvas.height * width) / canvas.width;

    // إذا كان المحتوى طويلاً جداً، نقسمه على صفحات
    if (.height * width) / canvas.width;

    // إذا كان المحتوى طويلاً جداً، نقسمه على صفحات
    if (height > pdf.internal.pageSize.getHeight()) {
        let position = 0;
        const pageHeight = pdf.internal.pageSize.getHeightheight > pdf.internal.pageSize.getHeight()) {
        let position = 0;
        const pageHeight = pdf.internal.pageSize.getHeight();
        
        while (height > 0) {
            pdf.addImage(imgData, "PNG", 0, position, width, height);
            height -= pageHeight;
            position -= pageHeight;
            
            if (height > 0) {
               ();
        
        while (height > 0) {
            pdf.addImage(imgData, "PNG", 0, position, width, height);
            height -= pageHeight;
            position -= pageHeight;
            
            if (height pdf.addPage();
            }
        }
    } else {
        pdf.addImage(imgData, "PNG", 0, 0, > 0) {
                pdf.addPage();
            }
        }
    } else {
        pdf.addImage(imgData, "PNG", 0, 0, width width, height);
    }
    
    // إعداد اسم الملف بناءً على التاريخ المحدد
    let fileName = 'سجل-التقييم-الشامل';
    if, height);
    }
    
    // إعداد اسم الملف بناءً على التاريخ المحدد
    let fileName = 'سجل-التقييم-الشامل';
    if ( (selectedAttendanceDate) {
        const date = new Date(selectedAttendanceDateselectedAttendanceDate) {
        const date = new Date(selectedAttendanceDate);
        const dateStr = date.toISOString().slice(0, 10);
        fileName = `س);
        const dateStr = date.toISOString().slice(0, 10);
        fileName = `سجل-التقييم-${dateStr}`;
    } else {
        fileName = `سجل-التقييم-${new Date().toISOString().slice(0جل-التقييم-${dateStr}`;
    } else {
        fileName = `سجل-التقييم-${new Date().to,10)}`;
    }
    
    pdf.save(`${fileName}.pdf`);
    
    // إعادة زر التصدير إلى وضعه الأصليISOString().slice(0,10)}`;
    }
    
    pdf.save(`${fileName}.pdf`);
    
    // إعادة زر التصدير إلى وضعه الأصلي

    exportBtn.innerHTML = originalText;
    exportBtn.disabled = false;
    
    // إظهار إشعار نجاح
    showNotification('تم تصدير ملف PDF بنجاح!', '    exportBtn.innerHTML = originalText;
    exportBtn.disabled = false;
    
    // إظهار إشعار نجاح
    showNotification('تم تصدير ملف PDF بنجاح!', 'present');
present');
}
</script>

</body>
</html>
