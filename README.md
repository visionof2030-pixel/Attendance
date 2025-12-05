<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
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
        --neutral-color: #757575;
        --hover-present: #1b5e20;
        --hover-absent: #b71c1c;
        --hover-neutral: #424242;
        --category-1: #5d4037;
        --category-2: #0277bd;
        --category-3: #6a1b9a;
        --light-bg: #f8f9fa;
        --border-color: #dee2e6;
    }
    
    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }
    
    html, body {
        width: 100%;
        height: 100%;
        overflow-x: hidden;
    }
    
    body {
        font-family: 'Tajawal', sans-serif;
        background: linear-gradient(135deg, #f6f8fc 0%, #e9f2ff 100%);
        margin: 0;
        padding: 0;
        direction: rtl;
        min-height: 100vh;
        overflow-x: hidden;
        -webkit-text-size-adjust: 100%;
        -webkit-tap-highlight-color: transparent;
    }
    
    .container {
        width: 100%;
        margin: 0 auto;
        background: white;
        padding: 8px;
        border-radius: 0;
        box-shadow: none;
        border: none;
        overflow: hidden;
    }
    
    /* تحسينات للشاشات الصغيرة جداً */
    @media (max-width: 360px) {
        .container {
            padding: 6px;
        }
    }
    
    h2 {
        margin-top: 0;
        text-align: center;
        color: var(--primary-color);
        font-size: 1.2rem;
        padding-bottom: 10px;
        border-bottom: 1px solid #f0f0f0;
        margin-bottom: 12px;
        position: relative;
        word-break: break-word;
        line-height: 1.3;
        padding-top: 5px;
    }
    
    h2:after {
        content: '';
        position: absolute;
        bottom: -1px;
        right: 50%;
        transform: translateX(50%);
        width: 80px;
        height: 2px;
        background: linear-gradient(to right, #2c5aa0, #4a8af4);
        border-radius: 2px;
    }
    
    .summary-box {
        background: linear-gradient(to right, rgba(44, 90, 160, 0.1), rgba(74, 138, 244, 0.1));
        border-radius: 8px;
        padding: 10px;
        margin-bottom: 12px;
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 8px;
        border: 1px solid rgba(44, 90, 160, 0.2);
    }
    
    .summary-item {
        text-align: center;
        padding: 8px 4px;
        border-radius: 6px;
        background: white;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.04);
    }
    
    .summary-count {
        font-size: 1.5rem;
        font-weight: 700;
        margin-bottom: 3px;
    }
    
    .present-count {
        color: var(--present-color);
    }
    
    .absent-count {
        color: var(--absent-color);
    }
    
    .summary-label {
        font-size: 0.8rem;
        color: #555;
        line-height: 1.2;
    }
    
    /* تحسين الجدول للجوال - بدون هيدر */
    .table-wrapper {
        width: 100%;
        overflow-x: auto;
        -webkit-overflow-scrolling: touch;
        margin-top: 10px;
        border-radius: 8px;
        box-shadow: 0 3px 8px rgba(0, 0, 0, 0.05);
        border: 1px solid #eee;
        margin-left: -2px;
        margin-right: -2px;
        padding: 0 2px;
    }
    
    table {
        width: 100%;
        border-collapse: separate;
        border-spacing: 0;
        min-width: 600px;
        font-size: 0.8rem;
    }
    
    th, td {
        padding: 8px 4px;
        text-align: center;
        border: 1px solid var(--border-color);
        line-height: 1.2;
        white-space: nowrap;
    }
    
    th {
        background: linear-gradient(to right, #2c5aa0, #3a6bc5);
        color: white;
        font-weight: 700;
        font-size: 0.85rem;
        white-space: nowrap;
    }
    
    .student-name {
        font-size: 0.9rem;
        font-weight: 600;
        text-align: right;
        padding-right: 6px;
        min-width: 90px;
        word-break: break-word;
        white-space: normal;
    }
    
    tr:nth-child(even) {
        background-color: #f9f9f9;
    }
    
    tr:hover {
        background-color: #f0f7ff;
        transition: background-color 0.2s;
    }
    
    /* تحسين خلايا التقييم للجوال */
    .evaluation-cell {
        text-align: center;
        padding: 4px 2px;
        min-width: 50px;
    }
    
    .status-icon {
        font-size: 1.3rem;
        cursor: pointer;
        transition: all 0.2s ease;
        display: inline-block;
        padding: 4px;
        border-radius: 50%;
        width: 38px;
        height: 38px;
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0 auto;
        touch-action: manipulation;
    }
    
    .status-icon:hover {
        transform: scale(1.06);
        box-shadow: 0 3px 8px rgba(0, 0, 0, 0.1);
    }
    
    /* الحالة الحاضرة - حاضر / صح / ممتاز */
    .present-icon {
        color: var(--present-color);
        background-color: rgba(46, 125, 50, 0.08);
        border: 2px solid rgba(46, 125, 50, 0.2);
    }
    
    .present-icon:hover {
        background-color: rgba(46, 125, 50, 0.15);
        border-color: rgba(46, 125, 50, 0.4);
    }
    
    /* الحالة الغائبة - غائب / خطأ / ضعيف */
    .absent-icon {
        color: var(--absent-color);
        background-color: rgba(198, 40, 40, 0.08);
        border: 2px solid rgba(198, 40, 40, 0.2);
    }
    
    .absent-icon:hover {
        background-color: rgba(198, 40, 40, 0.15);
        border-color: rgba(198, 40, 40, 0.4);
    }
    
    /* الحالة المحايدة - رمادية (للتقييمات التي ليست صح ولا خطأ) */
    .neutral-icon {
        color: var(--neutral-color);
        background-color: rgba(117, 117, 117, 0.08);
        border: 2px solid rgba(117, 117, 117, 0.2);
    }
    
    .neutral-icon:hover {
        background-color: rgba(117, 117, 117, 0.15);
        border-color: rgba(117, 117, 117, 0.4);
    }
    
    .status-label {
        display: block;
        margin-top: 3px;
        font-weight: 700;
        font-size: 0.75rem;
        min-height: 16px;
        line-height: 1.1;
    }
    
    .present-label {
        color: var(--present-color);
    }
    
    .absent-label {
        color: var(--absent-color);
    }
    
    .neutral-label {
        color: var(--neutral-color);
    }
    
    /* تحسين أزرار التحكم للجوال */
    .controls-container {
        display: flex;
        flex-direction: column;
        gap: 10px;
        margin: 16px 0;
        width: 100%;
    }
    
    .category-controls {
        background: white;
        border-radius: 8px;
        padding: 10px;
        box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
        flex: 1;
        border: 1px solid rgba(44, 90, 160, 0.1);
        width: 100%;
    }
    
    .control-title {
        color: var(--primary-color);
        font-size: 1rem;
        margin-bottom: 8px;
        text-align: center;
        padding-bottom: 5px;
        border-bottom: 1px solid #f0f0f0;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 5px;
        flex-wrap: wrap;
    }
    
    .category-buttons {
        display: flex;
        flex-wrap: wrap;
        gap: 5px;
        justify-content: center;
    }
    
    .status-btn {
        padding: 7px 10px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        font-size: 0.8rem;
        transition: all 0.2s ease;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 4px;
        min-width: 0;
        flex: 1;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.06);
        touch-action: manipulation;
        min-height: 40px;
    }
    
    .status-btn:hover {
        transform: translateY(-1px);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    
    .status-btn:active {
        transform: translateY(0);
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.06);
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
    
    .neutral-btn {
        background: linear-gradient(to right, var(--neutral-color), #9e9e9e);
        color: white;
    }
    
    .neutral-btn:hover {
        background: linear-gradient(to right, var(--hover-neutral), #616161);
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
    
    .export-container {
        text-align: center;
        margin-top: 16px;
        padding-top: 12px;
        border-top: 1px dashed #ddd;
        width: 100%;
        padding-left: 5px;
        padding-right: 5px;
    }
    
    .export {
        background: linear-gradient(to right, #2c5aa0, #4a8af4);
        color: white;
        padding: 12px 16px;
        font-size: 0.95rem;
        border-radius: 6px;
        width: 100%;
        box-shadow: 0 4px 10px rgba(42, 91, 173, 0.2);
        border: none;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        transition: all 0.2s ease;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        gap: 7px;
        touch-action: manipulation;
        min-height: 50px;
    }
    
    .export:hover {
        background: linear-gradient(to right, #1e3f7a, #2c5aa0);
        box-shadow: 0 6px 12px rgba(42, 91, 173, 0.3);
        transform: translateY(-2px);
    }
    
    .footer-note {
        text-align: center;
        margin-top: 12px;
        color: #666;
        font-size: 0.8rem;
        padding: 8px;
        background-color: #f8f9fa;
        border-radius: 6px;
        border-right: 2px solid var(--primary-color);
        line-height: 1.3;
        width: 100%;
    }
    
    /* تحسين قسم الإدارة للجوال */
    .admin-panel {
        max-height: 0;
        overflow: hidden;
        transition: max-height 0.4s ease, padding 0.4s ease, margin 0.4s ease;
        background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
        border-radius: 8px;
        margin: 0;
        padding: 0 10px;
        border: 1px solid transparent;
        width: 100%;
    }
    
    .admin-panel.active {
        max-height: 700px;
        padding: 12px 10px;
        margin-top: 12px;
        border-color: rgba(44, 90, 160, 0.2);
        box-shadow: 0 2px 6px rgba(0, 0, 0, 0.04);
    }
    
    .admin-panel-content {
        text-align: center;
        opacity: 0;
        transform: translateY(-5px);
        transition: opacity 0.3s ease 0.2s, transform 0.3s ease 0.2s;
    }
    
    .admin-panel.active .admin-panel-content {
        opacity: 1;
        transform: translateY(0);
    }
    
    /* تحسين قسم كلمة المرور للجوال */
    .password-section {
        margin-bottom: 12px;
        padding-bottom: 12px;
        border-bottom: 1px dashed #ccc;
    }
    
    .admin-title {
        color: #2c5aa0;
        font-size: 1rem;
        margin-bottom: 10px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 5px;
        flex-wrap: wrap;
    }
    
    .admin-description {
        color: #666;
        margin-bottom: 10px;
        font-size: 0.85rem;
    }
    
    .password-container {
        display: flex;
        flex-direction: column;
        gap: 8px;
        margin-bottom: 10px;
        width: 100%;
    }
    
    .password-input {
        padding: 9px;
        border: 2px solid #ddd;
        border-radius: 5px;
        font-size: 0.9rem;
        font-family: 'Tajawal', sans-serif;
        text-align: center;
        width: 100%;
        transition: border-color 0.2s;
    }
    
    .password-input:focus {
        border-color: #2c5aa0;
        outline: none;
    }
    
    .password-buttons {
        display: flex;
        gap: 7px;
        justify-content: center;
        width: 100%;
    }
    
    .password-submit, .password-cancel {
        padding: 8px 14px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-weight: 700;
        font-size: 0.85rem;
        transition: all 0.2s ease;
        min-width: 80px;
        flex: 1;
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
        margin-top: 7px;
        font-size: 0.8rem;
        display: none;
    }
    
    /* تحسين قسم تحديد التاريخ للجوال */
    .date-section {
        display: none;
        animation: fadeIn 0.4s ease;
    }
    
    .date-section.active {
        display: block;
    }
    
    .date-title {
        color: #2c5aa0;
        font-size: 1rem;
        margin-bottom: 8px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 5px;
    }
    
    .date-controls {
        display: flex;
        flex-direction: column;
        gap: 8px;
        margin-bottom: 10px;
        width: 100%;
    }
    
    .date-input {
        padding: 9px;
        border: 2px solid #ddd;
        border-radius: 5px;
        font-size: 0.9rem;
        font-family: 'Tajawal', sans-serif;
        text-align: center;
        width: 100%;
        transition: border-color 0.2s;
    }
    
    .date-input:focus {
        border-color: var(--primary-color);
        outline: none;
    }
    
    .date-btn {
        padding: 9px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        font-size: 0.85rem;
        transition: all 0.2s ease;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 5px;
        background: linear-gradient(to right, var(--primary-color), #4a8af4);
        color: white;
        width: 100%;
    }
    
    .date-btn:hover {
        background: linear-gradient(to right, #1e3f7a, #2c5aa0);
        transform: translateY(-2px);
    }
    
    .selected-date-display {
        background: white;
        padding: 7px 10px;
        border-radius: 5px;
        border: 2px solid var(--present-color);
        margin-top: 10px;
        display: inline-block;
        font-weight: 700;
        color: #333;
        font-size: 0.85rem;
    }
    
    /* تحسين مصفوفة التقييم للجوال */
    .evaluation-matrix {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 8px;
        margin: 12px 0;
    }
    
    .matrix-item {
        background: white;
        border-radius: 8px;
        padding: 10px;
        box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
        text-align: center;
        border-top: 3px solid;
    }
    
    .matrix-1 {
        border-color: var(--category-1);
    }
    
    .matrix-2 {
        border-color: var(--category-2);
    }
    
    .matrix-3 {
        border-color: var(--category-3);
        grid-column: span 2;
    }
    
    .matrix-title {
        font-size: 0.9rem;
        font-weight: 700;
        margin-bottom: 6px;
        color: #333;
    }
    
    .matrix-count {
        font-size: 1.6rem;
        font-weight: 800;
        margin: 5px 0;
    }
    
    .matrix-percent {
        font-size: 1.2rem;
        font-weight: 700;
        color: #666;
        margin-top: 5px;
    }
    
    /* أنيميشن */
    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-5px); }
        to { opacity: 1; transform: translateY(0); }
    }
    
    @keyframes shake {
        0%, 100% { transform: translateX(0); }
        10%, 30%, 50%, 70%, 90% { transform: translateX(-2px); }
        20%, 40%, 60%, 80% { transform: translateX(2px); }
    }
    
    @keyframes pulse {
        0% { transform: scale(1); }
        50% { transform: scale(1.05); }
        100% { transform: scale(1); }
    }
    
    @keyframes fadeInOut {
        0% { opacity: 0; transform: translateY(-12px); }
        15% { opacity: 1; transform: translateY(0); }
        85% { opacity: 1; transform: translateY(0); }
        100% { opacity: 0; transform: translateY(-12px); }
    }
    
    @keyframes fadeOut {
        from { opacity: 1; }
        to { opacity: 0; }
    }
    
    .status-icon.pulse {
        animation: pulse 0.3s ease;
    }
    
    /* تحسينات إضافية للأجهزة الكبيرة */
    @media (min-width: 480px) {
        .container {
            padding: 10px;
        }
        
        h2 {
            font-size: 1.3rem;
        }
        
        .summary-box {
            grid-template-columns: repeat(4, 1fr);
            padding: 12px;
        }
        
        .evaluation-matrix {
            grid-template-columns: repeat(3, 1fr);
        }
        
        .matrix-3 {
            grid-column: span 1;
        }
        
        .controls-container {
            flex-direction: row;
            flex-wrap: wrap;
        }
        
        .category-controls {
            min-width: 220px;
            flex: 1;
        }
        
        .password-container {
            flex-direction: row;
        }
        
        .password-input {
            min-width: 160px;
        }
        
        .password-buttons {
            flex: none;
        }
        
        .date-controls {
            flex-direction: row;
        }
        
        .date-btn {
            width: auto;
            min-width: 120px;
        }
        
        .export {
            font-size: 1rem;
            padding: 14px 20px;
        }
        
        .status-icon {
            width: 42px;
            height: 42px;
            font-size: 1.5rem;
        }
        
        .table-wrapper {
            padding: 0 5px;
            margin-left: -5px;
            margin-right: -5px;
        }
        
        table {
            min-width: 650px;
        }
    }
    
    @media (min-width: 768px) {
        .container {
            padding: 15px;
            max-width: 95%;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            border: 1px solid var(--border-color);
        }
        
        h2 {
            font-size: 1.5rem;
        }
        
        .status-icon {
            width: 45px;
            height: 45px;
            font-size: 1.6rem;
        }
        
        .student-name {
            font-size: 1rem;
        }
        
        .controls-container {
            gap: 12px;
        }
        
        .category-controls {
            padding: 12px;
        }
        
        .control-title {
            font-size: 1.1rem;
        }
        
        .status-btn {
            min-width: 110px;
            padding: 9px 12px;
            font-size: 0.85rem;
        }
        
        table {
            font-size: 0.85rem;
            min-width: 700px;
        }
        
        th, td {
            padding: 10px 6px;
        }
    }
    
    @media (min-width: 1024px) {
        .container {
            max-width: 1200px;
            padding: 20px;
        }
        
        table {
            min-width: 750px;
            font-size: 0.9rem;
        }
        
        th, td {
            padding: 12px 8px;
        }
        
        .evaluation-cell {
            min-width: 60px;
        }
        
        .status-icon {
            width: 50px;
            height: 50px;
            font-size: 1.8rem;
        }
        
        .status-label {
            font-size: 0.85rem;
        }
    }
    
    /* تحسينات للشاشات الطويلة */
    @media (max-height: 700px) {
        .summary-box {
            padding: 8px;
            gap: 6px;
        }
        
        .evaluation-matrix {
            margin: 10px 0;
            gap: 6px;
        }
        
        .controls-container {
            margin: 12px 0;
            gap: 8px;
        }
        
        .export-container {
            margin-top: 12px;
            padding-top: 10px;
        }
    }
    
    /* منع التكبير التلقائي على iOS */
    input, textarea, select {
        font-size: 16px !important;
    }
    
    /* تحسين التمرير على iOS */
    .table-wrapper {
        -webkit-overflow-scrolling: touch;
    }
</style>
</head>
<body>

<div class="container" id="captureArea">
    <h2><i class="fas fa-chart-bar" style="margin-left: 6px;"></i> سجل التقويم الشامل للطلاب</h2>
    
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
            <div class="matrix-title">المهام الأدائية</div>
            <div class="matrix-count" id="category1-count">100%</div>
            <div class="matrix-percent" id="category1-percent">(16/16)</div>
            <div style="font-size: 0.75rem; color: #666;">Assignments & Projects</div>
        </div>
        <div class="matrix-item matrix-2">
            <div class="matrix-title">المشاركة والتفاعل</div>
            <div class="matrix-count" id="category2-count">100%</div>
            <div class="matrix-percent" id="category2-percent">(16/16)</div>
            <div style="font-size: 0.75rem; color: #666;">Participation & Interaction</div>
        </div>
        <div class="matrix-item matrix-3">
            <div class="matrix-title">الحضور</div>
            <div class="matrix-count" id="category3-count">100%</div>
            <div class="matrix-percent" id="category3-percent">(8/8)</div>
            <div style="font-size: 0.75rem; color: #666;">سجل الحضور اليومي</div>
        </div>
    </div>

    <!-- الجدول المعدل - بدون هيدر -->
    <div class="table-wrapper">
        <table>
            <thead>
                <tr>
                    <th width="8%">الرقم</th>
                    <th width="22%">اسم الطالب</th>
                    <th width="12%">الواجبات</th>
                    <th width="12%">المشاريع</th>
                    <th width="12%">التقييم</th>
                    <th width="12%">التطبيقات والأنشطة</th>
                    <th width="12%">المشاركة</th>
                    <th width="10%">الحضور</th>
                </tr>
            </thead>
            <tbody>
                <!-- سيتم إنشاء الصفوف ديناميكيًا -->
            </tbody>
        </table>
    </div>
    
    <!-- أزرار التحكم حسب التصنيف -->
    <div class="controls-container">
        <div class="category-controls">
            <div class="control-title">
                <i class="fas fa-tasks"></i> المهام الأدائية
            </div>
            <div class="category-buttons">
                <button class="status-btn present-btn" onclick="setAllCategory('performance', 'present')">
                    <i class="fas fa-check"></i> الكل "صح"
                </button>
                <button class="status-btn neutral-btn" onclick="setAllCategory('performance', 'neutral')">
                    <i class="fas fa-minus"></i> الكل "محايد"
                </button>
                <button class="status-btn absent-btn" onclick="setAllCategory('performance', 'absent')">
                    <i class="fas fa-times"></i> الكل "خطأ"
                </button>
            </div>
        </div>
        
        <div class="category-controls">
            <div class="control-title">
                <i class="fas fa-comments"></i> المشاركة والتفاعل
            </div>
            <div class="category-buttons">
                <button class="status-btn present-btn" onclick="setAllCategory('interaction', 'present')">
                    <i class="fas fa-check"></i> الكل "صح"
                </button>
                <button class="status-btn neutral-btn" onclick="setAllCategory('interaction', 'neutral')">
                    <i class="fas fa-minus"></i> الكل "محايد"
                </button>
                <button class="status-btn absent-btn" onclick="setAllCategory('interaction', 'absent')">
                    <i class="fas fa-times"></i> الكل "خطأ"
                </button>
            </div>
        </div>
        
        <div class="category-controls">
            <div class="control-title">
                <i class="fas fa-users"></i> الحضور والإدارة
            </div>
            <div class="category-buttons">
                <button class="status-btn present-btn" onclick="setAllAttendance('present')">
                    <i class="fas fa-user-check"></i> الكل حاضر
                </button>
                <button class="status-btn absent-btn" onclick="setAllAttendance('absent')">
                    <i class="fas fa-user-slash"></i> الكل غائب
                </button>
                <button class="status-btn admin-btn" onclick="toggleAdminPanel()">
                    <i class="fas fa-cog"></i> إدارة
                </button>
                <button class="status-btn random-btn" id="randomBtn" onclick="toggleRandom()">
                    <i class="fas fa-random"></i> عشوائي
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
        <i class="fas fa-info-circle" style="margin-left: 5px;"></i> انقر على أيقونة أي تقييم للتبديل بين "صح" (أخضر)، "محايد" (رمادي)، "خطأ" (أحمر). الحضور يتبدل بين "حاضر" و"غائب" فقط.
    </div>
</div>

<div class="export-container">
    <button class="export" onclick="exportPDF()">
        <i class="fas fa-file-pdf"></i> تصدير سجل التقييم كملف PDF
    </button>
</div>

<script>
// تخزين حالة التقييمات للطلاب
let studentsData = {};
const totalStudents = 8;

// أسماء الطلاب
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

// تهيئة جميع التقييمات كإيجابية (صح)
function initializeStudentsData() {
    for (let i = 1; i <= totalStudents; i++) {
        studentsData[i] = {
            attendance: 'present',      // حاضر/غائب فقط
            assignments: 'present',     // صح/محايد/خطأ
            projects: 'present',        // صح/محايد/خطأ
            classroomApps: 'present',   // صح/محايد/خطأ
            participation: 'present'    // صح/محايد/خطأ
        };
    }
}

// إنشاء الجدول ديناميكيًا
function createTable() {
    const tbody = document.querySelector('tbody');
    tbody.innerHTML = '';
    
    for (let i = 1; i <= totalStudents; i++) {
        const student = studentsData[i];
        const row = document.createElement('tr');
        
        // الحصول على أيقونة ونص الحالة
        const getStatusInfo = (status, type = 'evaluation') => {
            if (type === 'attendance') {
                // للحضور: حاضر أو غائب فقط
                if (status === 'present') {
                    return { icon: 'fa-check', text: 'حاضر', class: 'present' };
                } else {
                    return { icon: 'fa-times', text: 'غائب', class: 'absent' };
                }
            } else {
                // للتقييمات الأخرى: صح/محايد/خطأ
                if (status === 'present') {
                    return { icon: 'fa-check', text: 'صح', class: 'present' };
                } else if (status === 'neutral') {
                    return { icon: 'fa-minus', text: 'محايد', class: 'neutral' };
                } else {
                    return { icon: 'fa-times', text: 'خطأ', class: 'absent' };
                }
            }
        };
        
        // الحصول على أيقونة ونص التقييم الإجمالي
        const getOverallPerformance = () => {
            const assignments = student.assignments;
            const projects = student.projects;
            
            // حساب النقاط لكل حالة (صح=2, محايد=1, خطأ=0)
            const getScore = (status) => {
                if (status === 'present') return 2;
                if (status === 'neutral') return 1;
                return 0;
            };
            
            const totalScore = getScore(assignments) + getScore(projects);
            
            // تحديد التقييم بناءً على النقاط الإجمالية (النقاط من 0 إلى 4)
            if (totalScore === 4) { // صح + صح
                return { icon: 'fa-star', text: 'ممتاز', class: 'present' };
            } else if (totalScore === 3) { // صح + محايد أو محايد + صح
                return { icon: 'fa-check', text: 'جيد', class: 'present' };
            } else if (totalScore === 2) { 
                // محايد + محايد = متوسط
                // صح + خطأ أو خطأ + صح = جيد (نفس الدرجة)
                if (assignments === 'neutral' && projects === 'neutral') {
                    return { icon: 'fa-minus', text: 'متوسط', class: 'neutral' };
                } else {
                    return { icon: 'fa-check', text: 'جيد', class: 'present' };
                }
            } else if (totalScore === 1) { // محايد + خطأ أو خطأ + محايد
                return { icon: 'fa-minus', text: 'ضعيف', class: 'absent' };
            } else { // خطأ + خطأ
                return { icon: 'fa-times', text: 'ضعيف', class: 'absent' };
            }
        };
        
        const attendanceInfo = getStatusInfo(student.attendance, 'attendance');
        const assignmentsInfo = getStatusInfo(student.assignments);
        const projectsInfo = getStatusInfo(student.projects);
        const classroomAppsInfo = getStatusInfo(student.classroomApps);
        const participationInfo = getStatusInfo(student.participation);
        const performanceInfo = getOverallPerformance();
        
        row.innerHTML = `
            <td>${i}</td>
            <td class="student-name">${studentNames[i-1]}</td>
            
            <!-- Performance Tasks -->
            <td class="evaluation-cell">
                <div class="status-icon ${assignmentsInfo.class}-icon" onclick="toggleEvaluation(${i}, 'assignments')" id="assignments-${i}">
                    <i class="fas ${assignmentsInfo.icon}"></i>
                </div>
                <span class="status-label ${assignmentsInfo.class}-label" id="label-assignments-${i}">${assignmentsInfo.text}</span>
            </td>
            <td class="evaluation-cell">
                <div class="status-icon ${projectsInfo.class}-icon" onclick="toggleEvaluation(${i}, 'projects')" id="projects-${i}">
                    <i class="fas ${projectsInfo.icon}"></i>
                </div>
                <span class="status-label ${projectsInfo.class}-label" id="label-projects-${i}">${projectsInfo.text}</span>
            </td>
            <td class="evaluation-cell">
                <div class="status-icon ${performanceInfo.class}-icon" id="performance-${i}">
                    <i class="fas ${performanceInfo.icon}"></i>
                </div>
                <span class="status-label ${performanceInfo.class}-label" id="label-performance-${i}">${performanceInfo.text}</span>
            </td>
            
            <!-- Participation & Interaction -->
            <td class="evaluation-cell">
                <div class="status-icon ${classroomAppsInfo.class}-icon" onclick="toggleEvaluation(${i}, 'classroomApps')" id="classroomApps-${i}">
                    <i class="fas ${classroomAppsInfo.icon}"></i>
                </div>
                <span class="status-label ${classroomAppsInfo.class}-label" id="label-classroomApps-${i}">${classroomAppsInfo.text}</span>
            </td>
            <td class="evaluation-cell">
                <div class="status-icon ${participationInfo.class}-icon" onclick="toggleEvaluation(${i}, 'participation')" id="participation-${i}">
                    <i class="fas ${participationInfo.icon}"></i>
                </div>
                <span class="status-label ${participationInfo.class}-label" id="label-participation-${i}">${participationInfo.text}</span>
            </td>
            
            <!-- الحضور -->
            <td class="evaluation-cell">
                <div class="status-icon ${attendanceInfo.class}-icon" onclick="toggleAttendance(${i})" id="attendance-${i}">
                    <i class="fas ${attendanceInfo.icon}"></i>
                </div>
                <span class="status-label ${attendanceInfo.class}-label" id="label-attendance-${i}">${attendanceInfo.text}</span>
            </td>
        `;
        
        tbody.appendChild(row);
    }
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

// تبديل تقييم معين للطالب (ثلاثي الحالات: صح ← محايد ← خطأ ← صح)
function toggleEvaluation(studentId, evaluationType) {
    const student = studentsData[studentId];
    const currentStatus = student[evaluationType];
    
    // تحديد الحالة التالية
    let nextStatus;
    if (currentStatus === 'present') {
        nextStatus = 'neutral';
    } else if (currentStatus === 'neutral') {
        nextStatus = 'absent';
    } else {
        nextStatus = 'present';
    }
    
    // تحديث البيانات
    student[evaluationType] = nextStatus;
    
    // تحديث الواجهة
    updateStudentDisplay(studentId);
    
    // تحديث الإحصائيات
    updateStatistics();
    
    // إشعار بصري
    const studentName = studentNames[studentId-1];
    const evaluationNames = {
        'assignments': 'الواجبات',
        'projects': 'المشاريع',
        'classroomApps': 'التطبيقات والأنشطة',
        'participation': 'المشاركة'
    };
    
    const statusNames = {
        'present': 'صح',
        'neutral': 'محايد', 
        'absent': 'خطأ'
    };
    
    showNotification(`تم تغيير ${evaluationNames[evaluationType]} لـ ${studentName} إلى "${statusNames[nextStatus]}"`, nextStatus);
}

// تبديل الحضور للطالب (ثنائي الحالة: حاضر ← غائب ← حاضر)
function toggleAttendance(studentId) {
    const student = studentsData[studentId];
    
    // تبديل الحالة
    if (student.attendance === 'present') {
        student.attendance = 'absent';
    } else {
        student.attendance = 'present';
    }
    
    // تحديث الواجهة
    updateStudentDisplay(studentId);
    
    // تحديث الإحصائيات
    updateStatistics();
    
    // إشعار بصري
    const studentName = studentNames[studentId-1];
    const statusText = student.attendance === 'present' ? 'حاضر' : 'غائب';
    showNotification(`تم تغيير حالة حضور ${studentName} إلى "${statusText}"`, student.attendance);
}

// تحديث عرض الطالب في الجدول
function updateStudentDisplay(studentId) {
    const student = studentsData[studentId];
    
    // الحصول على أيقونة ونص الحالة
    const getStatusInfo = (status, type = 'evaluation') => {
        if (type === 'attendance') {
            // للحضور: حاضر أو غائب فقط
            if (status === 'present') {
                return { icon: 'fa-check', text: 'حاضر', class: 'present' };
            } else {
                return { icon: 'fa-times', text: 'غائب', class: 'absent' };
            }
        } else {
            // للتقييمات الأخرى: صح/محايد/خطأ
            if (status === 'present') {
                return { icon: 'fa-check', text: 'صح', class: 'present' };
            } else if (status === 'neutral') {
                return { icon: 'fa-minus', text: 'محايد', class: 'neutral' };
            } else {
                return { icon: 'fa-times', text: 'خطأ', class: 'absent' };
            }
        }
    };
    
    // الحصول على أيقونة ونص التقييم الإجمالي
    const getOverallPerformance = () => {
        const assignments = student.assignments;
        const projects = student.projects;
        
        // حساب النقاط لكل حالة (صح=2, محايد=1, خطأ=0)
        const getScore = (status) => {
            if (status === 'present') return 2;
            if (status === 'neutral') return 1;
            return 0;
        };
        
        const totalScore = getScore(assignments) + getScore(projects);
        
        // تحديد التقييم بناءً على النقاط الإجمالية (النقاط من 0 إلى 4)
        if (totalScore === 4) { // صح + صح
            return { icon: 'fa-star', text: 'ممتاز', class: 'present' };
        } else if (totalScore === 3) { // صح + محايد أو محايد + صح
            return { icon: 'fa-check', text: 'جيد', class: 'present' };
        } else if (totalScore === 2) { 
            // محايد + محايد = متوسط
            // صح + خطأ أو خطأ + صح = جيد (نفس الدرجة)
            if (assignments === 'neutral' && projects === 'neutral') {
                return { icon: 'fa-minus', text: 'متوسط', class: 'neutral' };
            } else {
                return { icon: 'fa-check', text: 'جيد', class: 'present' };
            }
        } else if (totalScore === 1) { // محايد + خطأ أو خطأ + محايد
            return { icon: 'fa-minus', text: 'ضعيف', class: 'absent' };
        } else { // خطأ + خطأ
            return { icon: 'fa-times', text: 'ضعيف', class: 'absent' };
        }
    };
    
    // تحديث كل عنصر في الصف
    const updateElement = (id, info) => {
        const icon = document.getElementById(id);
        const label = document.getElementById(`label-${id}`);
        
        if (icon && label) {
            icon.className = `status-icon ${info.class}-icon`;
            icon.innerHTML = `<i class="fas ${info.icon}"></i>`;
            label.className = `status-label ${info.class}-label`;
            label.textContent = info.text;
            
            // تأثير النبض
            icon.classList.add('pulse');
            setTimeout(() => {
                icon.classList.remove('pulse');
            }, 300);
        }
    };
    
    // تحديث جميع العناصر
    updateElement(`attendance-${studentId}`, getStatusInfo(student.attendance, 'attendance'));
    updateElement(`assignments-${studentId}`, getStatusInfo(student.assignments));
    updateElement(`projects-${studentId}`, getStatusInfo(student.projects));
    updateElement(`classroomApps-${studentId}`, getStatusInfo(student.classroomApps));
    updateElement(`participation-${studentId}`, getStatusInfo(student.participation));
    updateElement(`performance-${studentId}`, getOverallPerformance());
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
        evaluationTypes.forEach(type => {
            studentsData[i][type] = status;
        });
        
        // تحديث العرض مع تأثير متأخر
        setTimeout(() => {
            updateStudentDisplay(i);
        }, i * 40);
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
            'neutral': '"محايد"',
            'absent': '"خطأ"'
        };
        showNotification(`تم تعيين جميع ${categoryNames[category]} كـ ${statusNames[status]}`, status);
    }, totalStudents * 80);
}

// تعيين جميع الحضور
function setAllAttendance(status) {
    for (let i = 1; i <= totalStudents; i++) {
        studentsData[i].attendance = status;
        
        // تحديث العرض مع تأثير متأخر
        setTimeout(() => {
            updateStudentDisplay(i);
        }, i * 50);
    }
    
    // تحديث الإحصائيات
    setTimeout(() => {
        updateStatistics();
        const statusNames = {
            'present': 'حاضرين',
            'absent': 'غائبين'
        };
        showNotification(`تم تعيين جميع الطلاب كـ ${statusNames[status]}`, status);
    }, totalStudents * 80);
}

// تبديل عشوائي لتقييمات بعض الطلاب
function toggleRandom() {
    // التحقق من صلاحية الدخول
    if (!isAdminAuthenticated) {
        showNotification('يجب التحقق من الهوية أولاً!', 'absent');
        toggleAdminPanel();
        return;
    }
    
    // تغيير 3-4 طلاب عشوائيًا في تقييمات مختلفة
    const changes = Math.floor(Math.random() * 3) + 2; // بين 2 و 4 تغييرات
    
    for (let i = 0; i < changes; i++) {
        const randomStudent = Math.floor(Math.random() * totalStudents) + 1;
        const evaluationTypes = ['assignments', 'projects', 'classroomApps', 'participation', 'attendance'];
        const randomType = evaluationTypes[Math.floor(Math.random() * evaluationTypes.length)];
        
        setTimeout(() => {
            if (randomType === 'attendance') {
                toggleAttendance(randomStudent);
            } else {
                toggleEvaluation(randomStudent, randomType);
            }
        }, i * 250);
    }
    
    showNotification(`تم تبديل تقييمات ${changes} طلاب عشوائيًا`, 'present');
}

// تحديث الإحصائيات
function updateStatistics() {
    let presentCount = 0;
    let absentCount = 0;
    
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
    
    // حساب نسبة الحضور
    const attendanceRate = Math.round((presentCount / totalStudents) * 100);
    document.getElementById('attendance-rate').textContent = `${attendanceRate}%`;
    
    // تحديث إحصائيات التصنيفات
    updateCategoryStatistics();
}

// تحديث إحصائيات التصنيفات
function updateCategoryStatistics() {
    // Performance Tasks
    let performancePresent = 0;
    let performanceNeutral = 0;
    let performanceAbsent = 0;
    let performanceTotal = totalStudents * 2; // assignments + projects لكل طالب
    
    for (let i = 1; i <= totalStudents; i++) {
        if (studentsData[i].assignments === 'present') performancePresent++;
        else if (studentsData[i].assignments === 'neutral') performanceNeutral++;
        else performanceAbsent++;
        
        if (studentsData[i].projects === 'present') performancePresent++;
        else if (studentsData[i].projects === 'neutral') performanceNeutral++;
        else performanceAbsent++;
    }
    
    // حساب النسبة بناءً على النقاط (صح=2, محايد=1, خطأ=0)
    const performanceScore = (performancePresent * 2) + performanceNeutral;
    const maxScore = performanceTotal * 2;
    const performanceRate = Math.round((performanceScore / maxScore) * 100);
    
    document.getElementById('category1-count').textContent = `${performanceRate}%`;
    document.getElementById('category1-percent').textContent = `(${performancePresent + performanceNeutral + performanceAbsent}/${performanceTotal})`;
    
    // Participation & Interaction
    let interactionPresent = 0;
    let interactionNeutral = 0;
    let interactionAbsent = 0;
    let interactionTotal = totalStudents * 2; // classroomApps + participation لكل طالب
    
    for (let i = 1; i <= totalStudents; i++) {
        if (studentsData[i].classroomApps === 'present') interactionPresent++;
        else if (studentsData[i].classroomApps === 'neutral') interactionNeutral++;
        else interactionAbsent++;
        
        if (studentsData[i].participation === 'present') interactionPresent++;
        else if (studentsData[i].participation === 'neutral') interactionNeutral++;
        else interactionAbsent++;
    }
    
    // حساب النسبة بناءً على النقاط
    const interactionScore = (interactionPresent * 2) + interactionNeutral;
    const interactionMaxScore = interactionTotal * 2;
    const interactionRate = Math.round((interactionScore / interactionMaxScore) * 100);
    
    document.getElementById('category2-count').textContent = `${interactionRate}%`;
    document.getElementById('category2-percent').textContent = `(${interactionPresent + interactionNeutral + interactionAbsent}/${interactionTotal})`;
    
    // الحضور
    let attendancePresent = 0;
    
    for (let i = 1; i <= totalStudents; i++) {
        if (studentsData[i].attendance === 'present') attendancePresent++;
    }
    
    const attendanceRate = Math.round((attendancePresent / totalStudents) * 100);
    document.getElementById('category3-count').textContent = `${attendanceRate}%`;
    document.getElementById('category3-percent').textContent = `(${attendancePresent}/${totalStudents})`;
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
        top: 10px;
        right: 8px;
        left: 8px;
        background: ${type === 'present' ? 'var(--present-color)' : type === 'neutral' ? 'var(--neutral-color)' : 'var(--absent-color)'};
        color: white;
        padding: 9px 14px;
        border-radius: 5px;
        z-index: 1000;
        font-weight: bold;
        box-shadow: 0 4px 10px rgba(0,0,0,0.12);
        animation: fadeInOut 3s ease-in-out;
        font-size: 0.9rem;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 7px;
        text-align: center;
    `;
    
    // إضافة أيقونة للإشعار
    const icon = document.createElement('i');
    icon.className = type === 'present' ? 'fas fa-check-circle' : type === 'neutral' ? 'fas fa-minus-circle' : 'fas fa-times-circle';
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
        0% { opacity: 0; transform: translateY(-12px); }
        15% { opacity: 1; transform: translateY(0); }
        85% { opacity: 1; transform: translateY(0); }
        100% { opacity: 0; transform: translateY(-12px); }
    }
    @keyframes fadeOut {
        from { opacity: 1; }
        to { opacity: 0; }
    }
`;
document.head.appendChild(style);

// دالة تصدير PDF
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
        dateText = `تاريخ التقييم: ${formattedDate}`;
    } else {
        dateText = `تاريخ التقييم: اليوم (${new Date().toLocaleDateString('ar-SA')})`;
    }
    
    dateElement.style.cssText = 'text-align: left; margin-bottom: 10px; color: #666; font-size: 0.85rem; padding: 7px 9px; background: #f8f9fa; border-radius: 5px; border-right: 2px solid #2c5aa0;';
    dateElement.innerHTML = `<i class="far fa-calendar-alt" style="margin-left: 5px;"></i> ${dateText} - <i class="far fa-clock" style="margin-left: 5px;"></i> وقت التصدير: ${new Date().toLocaleTimeString('ar-SA', {hour: '2-digit', minute:'2-digit'})}`;
    
    const captureArea = document.getElementById('captureArea');
    const originalContent = captureArea.innerHTML;
    
    // إضافة التاريخ أعلى المحتوى
    captureArea.insertBefore(dateElement, captureArea.firstChild);
    
    const canvas = await html2canvas(captureArea, { 
        scale: 2,
        useCORS: true,
        backgroundColor: '#ffffff',
        scrollY: -window.scrollY
    });

    // إعادة المحتوى الأصلي
    captureArea.removeChild(dateElement);
    
    const imgData = canvas.toDataURL("image/png");
    const pdf = new jsPDF("p", "mm", "a4");

    const imgWidth = 190; // A4 width in mm minus margins
    const imgHeight = (canvas.height * imgWidth) / canvas.width;

    // إضافة الصورة إلى PDF
    pdf.addImage(imgData, "PNG", 10, 10, imgWidth, imgHeight);
    
    // إعداد اسم الملف بناءً على التاريخ المحدد
    let fileName = 'سجل-التقييم-الشامل';
    if (selectedAttendanceDate) {
        const date = new Date(selectedAttendanceDate);
        const dateStr = date.toISOString().slice(0, 10);
        fileName = `سجل-التقييم-${dateStr}`;
    } else {
        fileName = `سجل-التقييم-${new Date().toISOString().slice(0,10)}`;
    }
    
    pdf.save(`${fileName}.pdf`);
    
    // إعادة زر التصدير إلى وضعه الأصلي
    exportBtn.innerHTML = originalText;
    exportBtn.disabled = false;
    
    // إظهار إشعار نجاح
    showNotification('تم تصدير ملف PDF بنجاح!', 'present');
}

// تهيئة التطبيق عند تحميل الصفحة
document.addEventListener('DOMContentLoaded', function() {
    initializeStudentsData();
    createTable();
    updateStatistics();
});

// إضافة خاصية لمنع التكبير على الهواتف
document.addEventListener('touchmove', function(e) {
    if (e.touches.length > 1) {
        e.preventDefault();
    }
}, { passive: false });

// منع التكبير المزدوج
let lastTouchEnd = 0;
document.addEventListener('touchend', function(e) {
    const now = Date.now();
    if (now - lastTouchEnd <= 300) {
        e.preventDefault();
    }
    lastTouchEnd = now;
}, false);

</script>

</body>
</html>
