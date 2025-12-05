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
    
    table {
        width: 100%;
        border-collapse: separate;
        border-spacing: 0;
        margin-top: 15px;
        overflow: hidden;
        border-radius: 15px;
        box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
        font-size: 1.1rem;
    }
    
    th, td {
        padding: 20px 15px;
        text-align: center;
        border: 1px solid var(--border-color);
    }
    
    th {
        background: linear-gradient(to right, #2c5aa0, #3a6bc5);
        color: white;
        font-weight: 700;
        font-size: 1.3rem;
        letter-spacing: 1px;
    }
    
    tr:nth-child(even) {
        background-color: #f9f9f9;
    }
    
    tr:hover {
        background-color: #f0f7ff;
        transition: background-color 0.3s;
    }
    
    /* تصميم خلايا الحضور */
    .attendance-cell {
        text-align: center;
        padding: 15px;
    }
    
    .status-icon {
        font-size: 3rem;
        cursor: pointer;
        transition: all 0.3s ease;
        display: inline-block;
        padding: 10px;
        border-radius: 50%;
        width: 80px;
        height: 80px;
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
        margin-top: 10px;
        font-weight: 700;
        font-size: 1.2rem;
    }
    
    .present-label {
        color: var(--present-color);
    }
    
    .absent-label {
        color: var(--absent-color);
    }
    
    .status-controls {
        display: flex;
        justify-content: center;
        gap: 15px;
        margin-top: 25px;
        flex-wrap: wrap;
    }
    
    .status-btn {
        padding: 12px 25px;
        border: none;
        border-radius: 10px;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        font-size: 1.1rem;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        min-width: 150px;
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
        display: none; /* مخفي في البداية */
    }
    
    .random-btn:hover {
        background: linear-gradient(to right, #e65100, #ef6c00);
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
    
    .student-name {
        font-size: 1.3rem;
        font-weight: 600;
    }
    
    /* تصميم قسم الإدارة */
    .admin-panel {
        max-height: 0;
        overflow: hidden;
        transition: max-height 1s ease, padding 1s ease, margin 1s ease;
        background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
        border-radius: 15px;
        margin: 0;
        padding: 0 20px;
        border: 2px solid transparent;
    }
    
    .admin-panel.active {
        max-height: 1500px;
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
    
    /* تصميم قسم تحديد التاريخ */
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
    
    /* تصميم قسم التحضير الشهري */
    .monthly-section {
        display: none;
        animation: fadeIn 0.8s ease;
        margin-top: 25px;
        padding-top: 25px;
        border-top: 2px dashed #ccc;
    }
    
    .monthly-section.active {
        display: block;
    }
    
    .monthly-title {
        color: #2c5aa0;
        font-size: 1.3rem;
        margin-bottom: 15px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
    }
    
    .monthly-controls {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 15px;
        flex-wrap: wrap;
        margin-bottom: 20px;
    }
    
    .month-select {
        padding: 12px 15px;
        border: 2px solid #ddd;
        border-radius: 10px;
        font-size: 1.1rem;
        font-family: 'Tajawal', sans-serif;
        text-align: center;
        min-width: 150px;
        transition: border-color 0.3s;
    }
    
    .month-select:focus, .year-select:focus {
        border-color: var(--primary-color);
        outline: none;
    }
    
    .year-select {
        padding: 12px 15px;
        border: 2px solid #ddd;
        border-radius: 10px;
        font-size: 1.1rem;
        font-family: 'Tajawal', sans-serif;
        text-align: center;
        min-width: 120px;
        transition: border-color 0.3s;
    }
    
    .monthly-btn {
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
        background: linear-gradient(to right, #8e44ad, #9b59b6);
        color: white;
    }
    
    .monthly-btn:hover {
        background: linear-gradient(to right, #6c3483, #7d3c98);
        transform: translateY(-2px);
    }
    
    .monthly-table-container {
        overflow-x: auto;
        margin-top: 20px;
        border-radius: 10px;
        border: 2px solid #ddd;
        max-height: 500px;
        overflow-y: auto;
    }
    
    .monthly-table {
        width: 100%;
        border-collapse: collapse;
        min-width: 800px;
    }
    
    .monthly-table th {
        background: linear-gradient(to right, #8e44ad, #9b59b6);
        color: white;
        padding: 12px 8px;
        position: sticky;
        top: 0;
        z-index: 10;
    }
    
    .monthly-table td {
        padding: 10px 8px;
        border: 1px solid #eee;
        text-align: center;
    }
    
    .monthly-table tr:nth-child(even) {
        background-color: #f9f9f9;
    }
    
    .day-header {
        font-weight: bold;
        background-color: rgba(142, 68, 173, 0.1);
    }
    
    .weekend-day {
        background-color: rgba(231, 76, 60, 0.1);
        color: #c0392b;
        font-weight: bold;
    }
    
    .day-cell {
        min-width: 100px;
    }
    
    .monthly-summary {
        margin-top: 20px;
        padding: 15px;
        background: linear-gradient(to right, rgba(142, 68, 173, 0.1), rgba(155, 89, 182, 0.1));
        border-radius: 10px;
        border: 2px solid rgba(142, 68, 173, 0.3);
    }
    
    .monthly-summary h4 {
        color: #8e44ad;
        margin-top: 0;
        margin-bottom: 10px;
    }
    
    .monthly-summary-stats {
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
        gap: 15px;
    }
    
    .monthly-stat {
        text-align: center;
        padding: 10px;
        background: white;
        border-radius: 8px;
        min-width: 120px;
        box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
    }
    
    /* تصميم متجاوب */
    @media (max-width: 768px) {
        .container {
            padding: 20px;
            margin: 10px;
        }
        
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
            font-size: 2.5rem;
            width: 70px;
            height: 70px;
        }
        
        table {
            font-size: 1rem;
        }
        
        th, td {
            padding: 15px 10px;
        }
        
        .export {
            min-width: 100%;
            padding: 16px;
        }
        
        .status-btn {
            min-width: 130px;
            padding: 10px 20px;
        }
        
        .password-container {
            flex-direction: column;
        }
        
        .password-input {
            max-width: 100%;
            min-width: auto;
        }
        
        .date-controls, .monthly-controls {
            flex-direction: column;
        }
        
        .date-input, .month-select, .year-select {
            min-width: 100%;
        }
        
        .monthly-table-container {
            font-size: 0.9rem;
        }
        
        .monthly-table th, .monthly-table td {
            padding: 8px 5px;
        }
    }
    
    @media (max-width: 480px) {
        .status-icon {
            font-size: 2rem;
            width: 60px;
            height: 60px;
        }
        
        th {
            font-size: 1.1rem;
        }
        
        .student-name {
            font-size: 1.1rem;
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
</style>
</head>
<body>

<div class="container" id="captureArea">
    <h2><i class="fas fa-users" style="margin-left: 15px;"></i> سجل حضور الطلاب - النظام الذكي</h2>
    
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

    <table>
        <thead>
            <tr>
                <th width="15%">الرقم</th>
                <th width="45%">اسم الطالب</th>
                <th width="40%">الحضور <span class="status-badge badge-present" id="status-summary">الكل حاضرين</span></th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td class="student-name">أحمد محمد</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(1)" id="status-1">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-1">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>2</td>
                <td class="student-name">جسّار فهد</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(2)" id="status-2">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-2">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>3</td>
                <td class="student-name">سارة عبدالله</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(3)" id="status-3">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-3">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>4</td>
                <td class="student-name">يوسف خالد</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(4)" id="status-4">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-4">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>5</td>
                <td class="student-name">نورة سعيد</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(5)" id="status-5">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-5">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>6</td>
                <td class="student-name">فارس علي</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(6)" id="status-6">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-6">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>7</td>
                <td class="student-name">ليلى حسن</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(7)" id="status-7">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-7">حاضر</span>
                </td>
            </tr>
            <tr>
                <td>8</td>
                <td class="student-name">محمد علي</td>
                <td class="attendance-cell">
                    <div class="status-icon present-icon" onclick="toggleStatus(8)" id="status-8">
                        <i class="fas fa-check"></i>
                    </div>
                    <span class="status-label present-label" id="label-8">حاضر</span>
                </td>
            </tr>
        </tbody>
    </table>
    
    <div class="status-controls">
        <button class="status-btn present-btn" onclick="setAllPresent()">
            <i class="fas fa-user-check"></i> تعيين الكل حاضر
        </button>
        <button class="status-btn absent-btn" onclick="setAllAbsent()">
            <i class="fas fa-user-slash"></i> تعيين الكل غائب
        </button>
        <button class="status-btn admin-btn" onclick="toggleAdminPanel()">
            <i class="fas fa-cog"></i> إدارة
        </button>
        <button class="status-btn random-btn" id="randomBtn" onclick="toggleRandom()">
            <i class="fas fa-random"></i> تبديل عشوائي
        </button>
    </div>
    
    <!-- قسم الإدارة الكامل -->
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
            
            <!-- قسم تحديد التاريخ - مخفي حتى التحقق -->
            <div class="date-section" id="dateSection">
                <div class="date-title">
                    <i class="fas fa-calendar-alt"></i> تحديد وقت التحضير
                </div>
                <p class="admin-description">يمكنك تحديد تاريخ الحضور للأشهر الماضية أو القادمة:</p>
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
                    <i class="far fa-calendar-check"></i> تاريخ الحضور: <span id="currentDateDisplay">اليوم</span>
                </div>
            </div>
            
            <!-- قسم التحضير الشهري - مخفي حتى التحقق -->
            <div class="monthly-section" id="monthlySection">
                <div class="monthly-title">
                    <i class="fas fa-calendar-day"></i> التحضير الشهري
                </div>
                <p class="admin-description">التحضير الشهري من يوم 1 إلى 26 من الشهر (مستثنى الجمعة والسبت):</p>
                <div class="monthly-controls">
                    <select class="month-select" id="monthSelect">
                        <option value="0">يناير</option>
                        <option value="1">فبراير</option>
                        <option value="2">مارس</option>
                        <option value="3">أبريل</option>
                        <option value="4">مايو</option>
                        <option value="5">يونيو</option>
                        <option value="6">يوليو</option>
                        <option value="7">أغسطس</option>
                        <option value="8">سبتمبر</option>
                        <option value="9">أكتوبر</option>
                        <option value="10">نوفمبر</option>
                        <option value="11">ديسمبر</option>
                    </select>
                    <select class="year-select" id="yearSelect"></select>
                    <button class="monthly-btn" onclick="generateMonthlyTable()">
                        <i class="fas fa-calendar-plus"></i> إنشاء جدول الشهر
                    </button>
                    <button class="monthly-btn" onclick="exportMonthlyPDF()" style="background: linear-gradient(to right, #27ae60, #2ecc71);">
                        <i class="fas fa-file-pdf"></i> تصدير الشهر
                    </button>
                </div>
                
                <div class="monthly-table-container" id="monthlyTableContainer">
                    <!-- سيتم إنشاء جدول الشهري هنا -->
                </div>
                
                <div class="monthly-summary" id="monthlySummary" style="display: none;">
                    <h4><i class="fas fa-chart-bar"></i> إحصائيات الشهر</h4>
                    <div class="monthly-summary-stats" id="monthlySummaryStats">
                        <!-- سيتم ملؤها بالإحصائيات -->
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <div class="footer-note">
        <i class="fas fa-info-circle" style="margin-left: 10px;"></i> النظام يعمل افتراضيًا على أن جميع الطلاب حاضرين. انقر على أيقونة أي طالب للتبديل بين الحضور والغياب. يتم تحديث الإحصائيات تلقائيًا.
    </div>
</div>

<div class="export-container">
    <button class="export" onclick="exportPDF()">
        <i class="fas fa-file-pdf"></i> تصدير سجل الحضور كملف PDF
    </button>
</div>

<script>
// تخزين حالة الطلاب - جميعهم حاضرين افتراضيًا
let studentsStatus = {};
const totalStudents = 8;

// تهيئة جميع الطلاب كحاضرين
for (let i = 1; i <= totalStudents; i++) {
    studentsStatus[i] = 'present';
}

// متغير للتحقق من صلاحية الدخول
let isAdminAuthenticated = false;

// متغير لتخزين تاريخ الحضور المحدد
let selectedAttendanceDate = null;

// تخزين بيانات التحضير الشهري
let monthlyAttendanceData = {};

// تهيئة سنوات في القائمة المنسدلة
function initializeYearSelect() {
    const yearSelect = document.getElementById('yearSelect');
    const currentYear = new Date().getFullYear();
    
    // إضافة سنوات من 2020 إلى 2030
    for (let year = 2020; year <= 2030; year++) {
        const option = document.createElement('option');
        option.value = year;
        option.textContent = year;
        if (year === currentYear) {
            option.selected = true;
        }
        yearSelect.appendChild(option);
    }
}

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
    showNotification(`تم تعيين تاريخ الحضور إلى ${document.getElementById('currentDateDisplay').textContent}`, 'present');
}

// الرجوع إلى تاريخ اليوم
function resetToToday() {
    const today = new Date();
    const formattedDate = today.toISOString().split('T')[0];
    document.getElementById('attendanceDate').value = formattedDate;
    updateDateDisplay();
    showNotification('تم الرجوع إلى تاريخ اليوم', 'present');
}

// إنشاء جدول التحضير الشهري
function generateMonthlyTable() {
    const month = parseInt(document.getElementById('monthSelect').value);
    const year = parseInt(document.getElementById('yearSelect').value);
    
    // إنشاء تاريخ للشهر المحدد
    const date = new Date(year, month, 1);
    const monthName = date.toLocaleDateString('ar-SA', { month: 'long' });
    const yearName = date.toLocaleDateString('ar-SA', { year: 'numeric' });
    
    // إعداد بيانات الشهر في حالة عدم وجودها
    const monthKey = `${year}-${month + 1}`;
    if (!monthlyAttendanceData[monthKey]) {
        monthlyAttendanceData[monthKey] = {};
        
        // تهيئة بيانات جميع الطلاب لجميع الأيام
        for (let day = 1; day <= 26; day++) {
            const dayDate = new Date(year, month, day);
            // تخطي الجمعة (5) والسبت (6)
            if (dayDate.getDay() !== 5 && dayDate.getDay() !== 6) {
                const dayKey = `${year}-${month + 1}-${day}`;
                monthlyAttendanceData[monthKey][dayKey] = {};
                
                // تعيين جميع الطلاب كحاضرين افتراضيًا
                for (let i = 1; i <= totalStudents; i++) {
                    monthlyAttendanceData[monthKey][dayKey][i] = 'present';
                }
            }
        }
    }
    
    // إنشاء جدول HTML
    let tableHTML = `
        <table class="monthly-table">
            <thead>
                <tr>
                    <th>اسم الطالب</th>
    `;
    
    // إضافة عناوين الأيام (1 إلى 26) مع استثناء الجمعة والسبت
    for (let day = 1; day <= 26; day++) {
        const dayDate = new Date(year, month, day);
        const dayOfWeek = dayDate.getDay();
        const dayName = getArabicDayName(dayOfWeek);
        
        // تخطي الجمعة (5) والسبت (6)
        if (dayOfWeek === 5 || dayOfWeek === 6) {
            tableHTML += `<th class="weekend-day">${day}<br>${dayName}</th>`;
        } else {
            tableHTML += `<th class="day-header">${day}<br>${dayName}</th>`;
        }
    }
    
    tableHTML += `</tr></thead><tbody>`;
    
    // إضافة صفوف الطلاب
    const studentNames = [
        "أحمد محمد", "جسّار فهد", "سارة عبدالله", "يوسف خالد",
        "نورة سعيد", "فارس علي", "ليلى حسن", "محمد علي"
    ];
    
    for (let i = 0; i < totalStudents; i++) {
        const studentId = i + 1;
        tableHTML += `<tr><td class="student-name">${studentNames[i]}</td>`;
        
        for (let day = 1; day <= 26; day++) {
            const dayDate = new Date(year, month, day);
            const dayOfWeek = dayDate.getDay();
            const dayKey = `${year}-${month + 1}-${day}`;
            
            // تحديد حالة الحضور للطالب لهذا اليوم
            let attendanceStatus = 'present';
            if (monthlyAttendanceData[monthKey] && monthlyAttendanceData[monthKey][dayKey]) {
                attendanceStatus = monthlyAttendanceData[monthKey][dayKey][studentId] || 'present';
            }
            
            // تخطي الجمعة (5) والسبت (6)
            if (dayOfWeek === 5 || dayOfWeek === 6) {
                tableHTML += `<td class="weekend-day"><i class="fas fa-ban" style="color: #95a5a6;"></i><br>عطلة</td>`;
            } else {
                const isPresent = attendanceStatus === 'present';
                const iconClass = isPresent ? 'fas fa-check present-icon-small' : 'fas fa-times absent-icon-small';
                const iconColor = isPresent ? '#2e7d32' : '#c62828';
                
                tableHTML += `
                    <td class="day-cell">
                        <div class="monthly-attendance-icon" onclick="toggleMonthlyAttendance(${studentId}, ${day}, ${month}, ${year})" 
                             style="cursor: pointer; font-size: 1.5rem; color: ${iconColor};">
                            <i class="${iconClass}"></i>
                        </div>
                        <div style="font-size: 0.8rem; margin-top: 5px;">
                            ${isPresent ? 'حاضر' : 'غائب'}
                        </div>
                    </td>
                `;
            }
        }
        
        tableHTML += `</tr>`;
    }
    
    tableHTML += `</tbody></table>`;
    
    // إضافة الجدول إلى الصفحة
    document.getElementById('monthlyTableContainer').innerHTML = tableHTML;
    
    // إظهار قسم الإحصائيات
    updateMonthlySummary(monthKey);
    document.getElementById('monthlySummary').style.display = 'block';
    
    showNotification(`تم إنشاء جدول التحضير الشهري لـ ${monthName} ${yearName}`, 'present');
}

// الحصول على اسم اليوم بالعربية
function getArabicDayName(dayOfWeek) {
    const days = ['الأحد', 'الإثنين', 'الثلاثاء', 'الأربعاء', 'الخميس', 'الجمعة', 'السبت'];
    return days[dayOfWeek];
}

// تبديل حالة الحضور في الجدول الشهري
function toggleMonthlyAttendance(studentId, day, month, year) {
    const monthKey = `${year}-${month + 1}`;
    const dayKey = `${year}-${month + 1}-${day}`;
    
    // التأكد من وجود البيانات
    if (!monthlyAttendanceData[monthKey]) {
        monthlyAttendanceData[monthKey] = {};
    }
    if (!monthlyAttendanceData[monthKey][dayKey]) {
        monthlyAttendanceData[monthKey][dayKey] = {};
    }
    
    // تبديل الحالة
    const currentStatus = monthlyAttendanceData[monthKey][dayKey][studentId] || 'present';
    const newStatus = currentStatus === 'present' ? 'absent' : 'present';
    
    monthlyAttendanceData[monthKey][dayKey][studentId] = newStatus;
    
    // تحديث الجدول
    generateMonthlyTable();
    
    // إشعار بصري
    const studentNames = [
        "أحمد محمد", "جسّار فهد", "سارة عبدالله", "يوسف خالد",
        "نورة سعيد", "فارس علي", "ليلى حسن", "محمد علي"
    ];
    
    showNotification(`تم تغيير حالة ${studentNames[studentId-1]} ليوم ${day}`, newStatus);
}

// تحديث إحصائيات الشهر
function updateMonthlySummary(monthKey) {
    if (!monthlyAttendanceData[monthKey]) return;
    
    let totalDays = 0;
    let totalPresent = 0;
    let totalAbsent = 0;
    const studentStats = {};
    
    // تهيئة إحصائيات الطلاب
    for (let i = 1; i <= totalStudents; i++) {
        studentStats[i] = { present: 0, absent: 0 };
    }
    
    // حساب الإحصائيات
    for (const dayKey in monthlyAttendanceData[monthKey]) {
        totalDays++;
        
        for (let i = 1; i <= totalStudents; i++) {
            const status = monthlyAttendanceData[monthKey][dayKey][i] || 'present';
            
            if (status === 'present') {
                totalPresent++;
                studentStats[i].present++;
            } else {
                totalAbsent++;
                studentStats[i].absent++;
            }
        }
    }
    
    // تحديث عرض الإحصائيات
    const summaryHTML = `
        <div class="monthly-stat">
            <div style="font-size: 1.8rem; color: #2c5aa0; font-weight: bold;">${totalDays}</div>
            <div>أيام الدراسة</div>
        </div>
        <div class="monthly-stat">
            <div style="font-size: 1.8rem; color: #2e7d32; font-weight: bold;">${totalPresent}</div>
            <div>حضور كلي</div>
        </div>
        <div class="monthly-stat">
            <div style="font-size: 1.8rem; color: #c62828; font-weight: bold;">${totalAbsent}</div>
            <div>غياب كلي</div>
        </div>
        <div class="monthly-stat">
            <div style="font-size: 1.8rem; color: #f57c00; font-weight: bold;">${totalDays > 0 ? Math.round((totalPresent / (totalDays * totalStudents)) * 100) : 0}%</div>
            <div>نسبة الحضور</div>
        </div>
    `;
    
    document.getElementById('monthlySummaryStats').innerHTML = summaryHTML;
}

// تصدير جدول الشهر كملف PDF
async function exportMonthlyPDF() {
    const month = parseInt(document.getElementById('monthSelect').value);
    const year = parseInt(document.getElementById('yearSelect').value);
    
    const date = new Date(year, month, 1);
    const monthName = date.toLocaleDateString('ar-SA', { month: 'long' });
    const yearName = date.toLocaleDateString('ar-SA', { year: 'numeric' });
    
    // إضافة عنوان للجدول الشهري
    const monthlyTableContainer = document.getElementById('monthlyTableContainer');
    const originalContent = monthlyTableContainer.innerHTML;
    
    const titleElement = document.createElement('div');
    titleElement.style.cssText = 'text-align: center; margin-bottom: 20px; color: #2c5aa0; font-size: 1.5rem; font-weight: bold;';
    titleElement.innerHTML = `<i class="fas fa-calendar-alt"></i> جدول التحضير الشهري - ${monthName} ${yearName}`;
    
    monthlyTableContainer.insertBefore(titleElement, monthlyTableContainer.firstChild);
    
    const { jsPDF } = window.jspdf;
    
    // تغيير نص زر التصدير مؤقتًا
    const exportBtn = document.querySelector('.monthly-btn');
    const originalText = exportBtn.innerHTML;
    exportBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> جاري التصدير...';
    exportBtn.disabled = true;
    
    const canvas = await html2canvas(monthlyTableContainer, { 
        scale: 2,
        useCORS: true,
        backgroundColor: '#ffffff'
    });

    // إعادة المحتوى الأصلي
    monthlyTableContainer.removeChild(titleElement);
    
    const imgData = canvas.toDataURL("image/png");
    const pdf = new jsPDF("l", "mm", "a4"); // اتجاه أفقي للجدول الطويل

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
    
    pdf.save(`سجل-الحضور-الشهري-${monthName}-${yearName}.pdf`);
    
    // إعادة زر التصدير إلى وضعه الأصلي
    exportBtn.innerHTML = originalText;
    exportBtn.disabled = false;
    
    showNotification('تم تصدير جدول الشهر كملف PDF بنجاح!', 'present');
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
        
        // إعادة تعيين كلمة المرور
        document.getElementById('passwordInput').value = '';
        document.getElementById('passwordError').style.display = 'none';
    } else {
        // فتح لوحة الإدارة
        adminPanel.classList.add('active');
        adminBtn.innerHTML = '<i class="fas fa-times"></i> إغلاق الإدارة';
        adminBtn.style.background = 'linear-gradient(to right, #757575, #9e9e9e)';
        
        // إذا تم التحقق مسبقًا، إظهار أقسام الإدارة مباشرة
        if (isAdminAuthenticated) {
            document.getElementById('passwordSection').style.display = 'none';
            document.getElementById('dateSection').classList.add('active');
            document.getElementById('monthlySection').classList.add('active');
            initializeDateField();
        } else {
            document.getElementById('passwordSection').style.display = 'block';
            document.getElementById('dateSection').classList.remove('active');
            document.getElementById('monthlySection').classList.remove('active');
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
    const monthlySection = document.getElementById('monthlySection');
    
    // كلمة المرور الصحيحة: Jassar1436
    if (password === 'Jassar1436') {
        // تم التحقق بنجاح
        isAdminAuthenticated = true;
        document.getElementById('randomBtn').style.display = 'flex';
        
        // إخفاء قسم كلمة المرور وإظهار أقسام الإدارة
        passwordSection.style.display = 'none';
        dateSection.classList.add('active');
        monthlySection.classList.add('active');
        
        // تهيئة حقول الإدارة
        initializeDateField();
        initializeYearSelect();
        
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

// تحديث الإحصائيات
function updateStatistics() {
    let presentCount = 0;
    let absentCount = 0;
    
    // حساب عدد الحاضرين والغائبين
    for (let i = 1; i <= totalStudents; i++) {
        if (studentsStatus[i] === 'present') {
            presentCount++;
        } else {
            absentCount++;
        }
    }
    
    // تحديث أرقام الإحصائيات
    document.getElementById('present-count').textContent = presentCount;
    document.getElementById('absent-count').textContent = absentCount;
    document.getElementById('total-count').textContent = totalStudents;
    
    // حساب نسبة الحضور
    const attendanceRate = Math.round((presentCount / totalStudents) * 100);
    document.getElementById('attendance-rate').textContent = `${attendanceRate}%`;
    
    // تحديث شارة حالة المجموعة
    const statusSummary = document.getElementById('status-summary');
    if (absentCount === 0) {
        statusSummary.className = 'status-badge badge-present';
        statusSummary.textContent = 'الكل حاضرين';
    } else if (presentCount === 0) {
        statusSummary.className = 'status-badge badge-absent';
        statusSummary.textContent = 'الكل غائبين';
    } else {
        statusSummary.className = 'status-badge';
        statusSummary.style.backgroundColor = 'rgba(44, 90, 160, 0.15)';
        statusSummary.style.color = '#2c5aa0';
        statusSummary.textContent = `${presentCount} حاضر - ${absentCount} غائب`;
    }
}

// تبديل حالة الطالب بين حاضر وغائب
function toggleStatus(studentId) {
    const statusIcon = document.getElementById(`status-${studentId}`);
    const statusLabel = document.getElementById(`label-${studentId}`);
    
    // إضافة تأثير النبض
    statusIcon.classList.add('pulse');
    setTimeout(() => {
        statusIcon.classList.remove('pulse');
    }, 500);
    
    // تبديل الحالة
    if (studentsStatus[studentId] === 'present') {
        // تغيير إلى غائب
        studentsStatus[studentId] = 'absent';
        statusIcon.className = 'status-icon absent-icon';
        statusIcon.innerHTML = '<i class="fas fa-times"></i>';
        statusLabel.className = 'status-label absent-label';
        statusLabel.textContent = 'غائب';
    } else {
        // تغيير إلى حاضر
        studentsStatus[studentId] = 'present';
        statusIcon.className = 'status-icon present-icon';
        statusIcon.innerHTML = '<i class="fas fa-check"></i>';
        statusLabel.className = 'status-label present-label';
        statusLabel.textContent = 'حاضر';
    }
    
    // تحديث الإحصائيات
    updateStatistics();
    
    // إشعار بصري
    const studentName = document.querySelector(`tr:nth-child(${studentId}) .student-name`).textContent;
    showNotification(`تم تغيير حالة ${studentName}`, studentsStatus[studentId]);
}

// تعيين جميع الطلاب كحاضرين
function setAllPresent() {
    for (let i = 1; i <= totalStudents; i++) {
        studentsStatus[i] = 'present';
        const statusIcon = document.getElementById(`status-${i}`);
        const statusLabel = document.getElementById(`label-${i}`);
        
        statusIcon.className = 'status-icon present-icon';
        statusIcon.innerHTML = '<i class="fas fa-check"></i>';
        statusLabel.className = 'status-label present-label';
        statusLabel.textContent = 'حاضر';
        
        // تأثير لكل أيقونة
        setTimeout(() => {
            statusIcon.classList.add('pulse');
            setTimeout(() => {
                statusIcon.classList.remove('pulse');
            }, 500);
        }, i * 80);
    }
    
    // تحديث الإحصائيات
    updateStatistics();
    
    showNotification('تم تعيين جميع الطلاب كحاضرين', 'present');
}

// تعيين جميع الطلاب كغائبين
function setAllAbsent() {
    for (let i = 1; i <= totalStudents; i++) {
        studentsStatus[i] = 'absent';
        const statusIcon = document.getElementById(`status-${i}`);
        const statusLabel = document.getElementById(`label-${i}`);
        
        statusIcon.className = 'status-icon absent-icon';
        statusIcon.innerHTML = '<i class="fas fa-times"></i>';
        statusLabel.className = 'status-label absent-label';
        statusLabel.textContent = 'غائب';
        
        // تأثير لكل أيقونة
        setTimeout(() => {
            statusIcon.classList.add('pulse');
            setTimeout(() => {
                statusIcon.classList.remove('pulse');
            }, 500);
        }, i * 80);
    }
    
    // تحديث الإحصائيات
    updateStatistics();
    
    showNotification('تم تعيين جميع الطلاب كغائبين', 'absent');
}

// تبديل عشوائي لحالات بعض الطلاب
function toggleRandom() {
    // التحقق من صلاحية الدخول
    if (!isAdminAuthenticated) {
        showNotification('يجب التحقق من الهوية أولاً!', 'absent');
        toggleAdminPanel();
        return;
    }
    
    // تغيير 3-4 طلاب عشوائيًا
    const changes = Math.floor(Math.random() * 3) + 2; // بين 2 و 4 تغييرات
    
    for (let i = 0; i < changes; i++) {
        const randomStudent = Math.floor(Math.random() * totalStudents) + 1;
        setTimeout(() => {
            toggleStatus(randomStudent);
        }, i * 300);
    }
    
    showNotification(`تم تبديل حالة ${changes} طلاب عشوائيًا`, 'present');
}

// إظهار إشعار مؤقت
function showNotification(message, type) {
    // إزالة أي إشعارات سابقة
    const existingNotification = document.querySelector('.custom-notification');
    if (existingNotification) {
        existingNotification.remove();
    }
    
    // إنشاء إشعار جديد
    const notification = document.createElement('div');
    notification.className = 'custom-notification';
    notification.textContent = message;
    notification.style.cssText = `
        position: fixed;
        top: 25px;
        right: 25px;
        background: ${type === 'present' ? 'var(--present-color)' : 'var(--absent-color)'};
        color: white;
        padding: 15px 25px;
        border-radius: 10px;
        z-index: 1000;
        font-weight: bold;
        box-shadow: 0 8px 20px rgba(0,0,0,0.2);
        animation: fadeInOut 3s ease-in-out;
        font-size: 1.1rem;
        display: flex;
        align-items: center;
        gap: 10px;
    `;
    
    // إضافة أيقونة للإشعار
    const icon = document.createElement('i');
    icon.className = type === 'present' ? 'fas fa-check-circle' : 'fas fa-times-circle';
    notification.prepend(icon);
    
    document.body.appendChild(notification);
    
    // إزالة الإشعار بعد 3 ثوان
    setTimeout(() => {
        notification.style.animation = 'fadeOut 0.5s ease-out forwards';
        setTimeout(() => {
            if (notification.parentNode) {
                notification.parentNode.removeChild(notification);
            }
        }, 500);
    }, 2500);
}

// إضافة أنيميشن للإشعارات
const style = document.createElement('style');
style.textContent = `
    @keyframes fadeInOut {
        0% { opacity: 0; transform: translateY(-25px); }
        15% { opacity: 1; transform: translateY(0); }
        85% { opacity: 1; transform: translateY(0); }
        100% { opacity: 0; transform: translateY(-25px); }
    }
    @keyframes fadeOut {
        from { opacity: 1; }
        to { opacity: 0; }
    }
`;
document.head.appendChild(style);

// تحديث الإحصائيات عند تحميل الصفحة
window.addEventListener('load', () => {
    updateStatistics();
});

// دالة تصدير PDF - معدلة لتضمين التاريخ المحدد
async function exportPDF() {
    const { jsPDF } = window.jspdf;
    
    // تغيير نص زر التصدير مؤقتًا
    const exportBtn = document.querySelector('.export');
    const originalText = exportBtn.innerHTML;
    exportBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> جاري التصدير...';
    exportBtn.disabled = true;
    
    // إضافة تاريخ التصدير والتاريخ المحدد
    const dateElement = document.createElement('div');
    
    // تحضير نص التاريخ
    let dateText = '';
    if (selectedAttendanceDate) {
        const date = new Date(selectedAttendanceDate);
        const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
        const formattedDate = date.toLocaleDateString('ar-SA', options);
        dateText = `تاريخ الحضور: ${formattedDate}`;
    } else {
        dateText = `تاريخ الحضور: اليوم (${new Date().toLocaleDateString('ar-SA')})`;
    }
    
    dateElement.style.cssText = 'text-align: left; margin-bottom: 20px; color: #666; font-size: 1rem; padding: 10px 15px; background: #f8f9fa; border-radius: 8px; border-right: 4px solid #2c5aa0;';
    dateElement.innerHTML = `<i class="far fa-calendar-alt" style="margin-left: 8px;"></i> ${dateText} - <i class="far fa-clock" style="margin-left: 8px;"></i> وقت التصدير: ${new Date().toLocaleTimeString('ar-SA', {hour: '2-digit', minute:'2-digit'})}`;
    
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
    
    // إعداد اسم الملف بناءً على التاريخ المحدد
    let fileName = 'سجل-الحضور';
    if (selectedAttendanceDate) {
        const date = new Date(selectedAttendanceDate);
        const dateStr = date.toISOString().slice(0, 10);
        fileName = `سجل-الحضور-${dateStr}`;
    } else {
        fileName = `سجل-الحضور-${new Date().toISOString().slice(0,10)}`;
    }
    
    pdf.save(`${fileName}.pdf`);
    
    // إعادة زر التصدير إلى وضعه الأصلي
    exportBtn.innerHTML = originalText;
    exportBtn.disabled = false;
    
    // إظهار إشعار نجاح
    showNotification('تم تصدير ملف PDF بنجاح!', 'present');
}
</script>

</body>
</html>
