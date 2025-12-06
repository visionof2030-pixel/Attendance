
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0, user-scalable=yes" />
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
        --light-bg: #f8f9fa;
        --border-color: #dee2e6;
    }
    
    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
        -webkit-tap-highlight-color: rgba(0,0,0,0);
    }
    
    html, body {
        width: 100%;
        height: 100%;
        overflow-x: hidden;
        font-size: 14px;
        -webkit-text-size-adjust: 100%;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }
    
    body {
        font-family: 'Tajawal', sans-serif;
        background: linear-gradient(135deg, #f6f8fc 0%, #e9f2ff 100%);
        margin: 0;
        padding: 10px;
        direction: rtl;
        min-height: 100vh;
        overflow-x: hidden;
        display: flex;
        justify-content: center;
        align-items: flex-start;
    }
    
    .container {
        width: 100%;
        max-width: 1200px;
        background: white;
        padding: 15px;
        border-radius: 10px;
        box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
        border: 1px solid #ddd;
        overflow: hidden;
        margin: 0 auto;
    }
    
    /* تحسينات للهواتف الصغيرة */
    @media screen and (max-width: 480px) {
        body {
            padding: 5px;
        }
        
        .container {
            padding: 10px;
            border-radius: 8px;
        }
        
        h2 {
            font-size: 1.2rem;
            padding-bottom: 6px;
            margin-bottom: 8px;
        }
        
        h2:after {
            width: 60px;
            height: 2px;
        }
        
        .footer-note {
            font-size: 0.7rem;
            padding: 5px;
            margin-top: 8px;
        }
        
        .export {
            padding: 8px 12px;
            font-size: 0.85rem;
            min-height: 40px;
        }
    }
    
    /* تحسينات للهواتف المتوسطة */
    @media screen and (min-width: 481px) and (max-width: 768px) {
        .container {
            padding: 12px;
        }
        
        h2 {
            font-size: 1.3rem;
        }
    }
    
    /* تحسينات للأجهزة اللوحية */
    @media screen and (min-width: 769px) and (max-width: 1024px) {
        .container {
            padding: 15px;
            max-width: 95%;
        }
    }
    
    /* تحسينات للشاشات الكبيرة */
    @media screen and (min-width: 1025px) {
        .container {
            max-width: 1200px;
        }
    }
    
    @media print {
        body {
            background: white !important;
            padding: 0 !important;
            margin: 0 !important;
            zoom: 0.9;
        }
        
        .container {
            width: 100% !important;
            max-width: 100% !important;
            box-shadow: none !important;
            border: none !important;
            padding: 5px !important;
            border-radius: 0 !important;
        }
        
        .export-container, .admin-btn, .random-btn {
            display: none !important;
        }
        
        .table-wrapper {
            max-height: none !important;
            overflow: visible !important;
        }
        
        table {
            min-width: 100% !important;
            font-size: 10px !important;
        }
        
        th, td {
            padding: 3px 2px !important;
        }
        
        .status-icon {
            width: 22px !important;
            height: 22px !important;
            font-size: 0.9rem !important;
        }
    }
    
    h2 {
        margin-top: 0;
        text-align: center;
        color: var(--primary-color);
        font-size: 1.4rem;
        padding-bottom: 8px;
        border-bottom: 1px solid #f0f0f0;
        margin-bottom: 10px;
        position: relative;
        word-break: break-word;
        line-height: 1.2;
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
    
    /* تحسين الجدول للجوال */
    .table-wrapper {
        width: 100%;
        overflow-x: auto;
        -webkit-overflow-scrolling: touch;
        margin-top: 10px;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
        border: 1px solid #e0e0e0;
        background: white;
        position: relative;
    }
    
    /* تخصيص شريط التمرير للجوال */
    .table-wrapper::-webkit-scrollbar {
        height: 6px;
    }
    
    .table-wrapper::-webkit-scrollbar-track {
        background: #f1f1f1;
        border-radius: 3px;
    }
    
    .table-wrapper::-webkit-scrollbar-thumb {
        background: #c1c1c1;
        border-radius: 3px;
    }
    
    .table-wrapper::-webkit-scrollbar-thumb:hover {
        background: #a8a8a8;
    }
    
    table {
        width: 100%;
        border-collapse: collapse;
        min-width: 600px;
        font-size: 0.85rem;
    }
    
    th, td {
        padding: 8px 4px;
        text-align: center;
        border: 1px solid var(--border-color);
        line-height: 1.2;
        white-space: nowrap;
        transition: background-color 0.2s;
    }
    
    th {
        background: linear-gradient(to right, #2c5aa0, #3a6bc5);
        color: white;
        font-weight: 700;
        font-size: 0.9rem;
        white-space: nowrap;
        position: sticky;
        top: 0;
        z-index: 10;
        min-width: 50px;
    }
    
    .student-name {
        font-size: 0.9rem;
        font-weight: 600;
        text-align: right;
        padding-right: 6px;
        min-width: 90px;
        max-width: 120px;
        word-break: break-word;
        white-space: normal;
    }
    
    tr:nth-child(even) {
        background-color: #f9f9f9;
    }
    
    tr:hover {
        background-color: #f0f7ff;
    }
    
    /* تحسين خلايا التقييم للجوال */
    .evaluation-cell {
        text-align: center;
        padding: 4px 2px;
        min-width: 40px;
        max-width: 60px;
    }
    
    /* خلايا خاصة للعناوين المتعددة الأسطر */
    .multiline-cell {
        min-width: 35px !important;
        max-width: 50px !important;
        line-height: 1.1 !important;
        white-space: normal !important;
        word-break: break-word !important;
        padding: 2px 1px !important;
    }
    
    .multiline-header {
        font-size: 0.8rem !important;
        line-height: 1.1 !important;
        white-space: normal !important;
        word-break: break-word !important;
        padding: 6px 2px !important;
    }
    
    .status-icon {
        font-size: 1rem;
        cursor: pointer;
        transition: all 0.2s ease;
        display: inline-block;
        padding: 4px;
        border-radius: 50%;
        width: 32px;
        height: 32px;
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0 auto;
        touch-action: manipulation;
        -webkit-tap-highlight-color: transparent;
    }
    
    .status-icon:hover {
        transform: scale(1.08);
        box-shadow: 0 3px 6px rgba(0, 0, 0, 0.15);
    }
    
    /* تحسين للأجهزة التي باللمس */
    .status-icon:active {
        transform: scale(0.95);
    }
    
    /* الحالة الحاضرة - حاضر / صح / ممتاز */
    .present-icon {
        color: var(--present-color);
        background-color: rgba(46, 125, 50, 0.08);
        border: 1.5px solid rgba(46, 125, 50, 0.2);
    }
    
    .present-icon:hover {
        background-color: rgba(46, 125, 50, 0.15);
        border-color: rgba(46, 125, 50, 0.4);
    }
    
    /* الحالة الغائبة - غائب / خطأ / ضعيف */
    .absent-icon {
        color: var(--absent-color);
        background-color: rgba(198, 40, 40, 0.08);
        border: 1.5px solid rgba(198, 40, 40, 0.2);
    }
    
    .absent-icon:hover {
        background-color: rgba(198, 40, 40, 0.15);
        border-color: rgba(198, 40, 40, 0.4);
    }
    
    /* الحالة المحايدة - رمادية (للتقييمات التي ليست صح ولا خطأ) */
    .neutral-icon {
        color: var(--neutral-color);
        background-color: rgba(117, 117, 117, 0.08);
        border: 1.5px solid rgba(117, 117, 117, 0.2);
    }
    
    .neutral-icon:hover {
        background-color: rgba(117, 117, 117, 0.15);
        border-color: rgba(117, 117, 117, 0.4);
    }
    
    .status-label {
        display: block;
        margin-top: 3px;
        font-weight: 700;
        font-size: 0.7rem;
        min-height: 14px;
        line-height: 1;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
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
    
    /* زر الإدارة في الواجهة الرئيسية */
    .admin-main-btn-container {
        text-align: center;
        margin: 15px 0;
    }
    
    .admin-main-btn {
        background: linear-gradient(to right, #5d4037, #795548);
        color: white;
        padding: 12px 20px;
        font-size: 0.95rem;
        border-radius: 8px;
        width: 100%;
        max-width: 250px;
        box-shadow: 0 4px 10px rgba(93, 64, 55, 0.2);
        border: none;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        transition: all 0.2s ease;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        touch-action: manipulation;
        min-height: 50px;
    }
    
    .admin-main-btn:hover {
        background: linear-gradient(to right, #3e2723, #4e342e);
        box-shadow: 0 6px 12px rgba(93, 64, 55, 0.3);
        transform: translateY(-2px);
    }
    
    .admin-main-btn:active {
        transform: translateY(0);
    }
    
    /* تحسين أزرار التحكم للجوال */
    .controls-container {
        display: flex;
        flex-direction: column;
        gap: 8px;
        margin: 15px 0;
        width: 100%;
    }
    
    .category-controls {
        background: white;
        border-radius: 8px;
        padding: 10px;
        box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
        flex: 1;
        border: 1px solid rgba(44, 90, 160, 0.1);
        width: 100%;
        display: none;
    }
    
    .category-controls.admin-only {
        display: block;
    }
    
    .control-title {
        color: var(--primary-color);
        font-size: 0.95rem;
        margin-bottom: 8px;
        text-align: center;
        padding-bottom: 6px;
        border-bottom: 1px solid #f0f0f0;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 6px;
        flex-wrap: wrap;
    }
    
    .category-buttons {
        display: flex;
        flex-wrap: wrap;
        gap: 6px;
        justify-content: center;
    }
    
    .status-btn {
        padding: 8px 10px;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        font-size: 0.8rem;
        transition: all 0.2s ease;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 5px;
        min-width: 0;
        flex: 1;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        touch-action: manipulation;
        min-height: 36px;
        -webkit-tap-highlight-color: transparent;
    }
    
    .status-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
    }
    
    .status-btn:active {
        transform: translateY(0);
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
    
    /* أزرار الحضور الخاصة بالإدارة */
    .attendance-controls {
        display: flex;
        flex-wrap: wrap;
        gap: 6px;
        justify-content: center;
        margin-top: 12px;
    }
    
    .export-container {
        text-align: center;
        margin-top: 20px;
        padding-top: 15px;
        border-top: 1px dashed #ddd;
        width: 100%;
    }
    
    .export {
        background: linear-gradient(to right, #2c5aa0, #4a8af4);
        color: white;
        padding: 12px 20px;
        font-size: 0.95rem;
        border-radius: 8px;
        width: 100%;
        max-width: 400px;
        box-shadow: 0 4px 12px rgba(42, 91, 173, 0.25);
        border: none;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        transition: all 0.2s ease;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        touch-action: manipulation;
        min-height: 50px;
        -webkit-tap-highlight-color: transparent;
    }
    
    .export:hover {
        background: linear-gradient(to right, #1e3f7a, #2c5aa0);
        box-shadow: 0 6px 15px rgba(42, 91, 173, 0.35);
        transform: translateY(-2px);
    }
    
    .export:active {
        transform: translateY(0);
    }
    
    .footer-note {
        text-align: center;
        margin-top: 15px;
        color: #666;
        font-size: 0.8rem;
        padding: 8px 10px;
        background-color: #f8f9fa;
        border-radius: 8px;
        border-right: 3px solid var(--primary-color);
        line-height: 1.4;
        width: 100%;
    }
    
    /* تحسين قسم الإدارة للجوال */
    .admin-panel {
        max-height: 0;
        overflow: hidden;
        transition: max-height 0.4s ease, padding 0.4s ease, margin 0.4s ease;
        background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
        border-radius: 10px;
        margin: 0;
        padding: 0 10px;
        border: 1px solid transparent;
        width: 100%;
    }
    
    .admin-panel.active {
        max-height: 1000px;
        padding: 15px 10px;
        margin-top: 15px;
        border-color: rgba(44, 90, 160, 0.2);
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
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
        margin-bottom: 15px;
        padding-bottom: 15px;
        border-bottom: 1px dashed #ccc;
    }
    
    .admin-title {
        color: #2c5aa0;
        font-size: 1rem;
        margin-bottom: 10px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 6px;
        flex-wrap: wrap;
    }
    
    .admin-description {
        color: #666;
        margin-bottom: 10px;
        font-size: 0.85rem;
        line-height: 1.4;
    }
    
    .password-container {
        display: flex;
        flex-direction: column;
        gap: 8px;
        margin-bottom: 10px;
        width: 100%;
    }
    
    .password-input {
        padding: 10px;
        border: 2px solid #ddd;
        border-radius: 6px;
        font-size: 0.9rem;
        font-family: 'Tajawal', sans-serif;
        text-align: center;
        width: 100%;
        transition: border-color 0.2s;
        -webkit-appearance: none;
    }
    
    .password-input:focus {
        border-color: #2c5aa0;
        outline: none;
        box-shadow: 0 0 0 2px rgba(44, 90, 160, 0.1);
    }
    
    .password-buttons {
        display: flex;
        gap: 8px;
        justify-content: center;
        width: 100%;
    }
    
    .password-submit, .password-cancel {
        padding: 10px 15px;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        font-weight: 700;
        font-size: 0.85rem;
        transition: all 0.2s ease;
        min-width: 80px;
        flex: 1;
        touch-action: manipulation;
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
        margin-top: 8px;
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
        gap: 6px;
    }
    
    .date-controls {
        display: flex;
        flex-direction: column;
        gap: 8px;
        margin-bottom: 12px;
        width: 100%;
    }
    
    .date-input {
        padding: 10px;
        border: 2px solid #ddd;
        border-radius: 6px;
        font-size: 0.9rem;
        font-family: 'Tajawal', sans-serif;
        text-align: center;
        width: 100%;
        transition: border-color 0.2s;
        -webkit-appearance: none;
    }
    
    .date-input:focus {
        border-color: var(--primary-color);
        outline: none;
        box-shadow: 0 0 0 2px rgba(44, 90, 160, 0.1);
    }
    
    .date-btn {
        padding: 10px;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        font-weight: 700;
        font-family: 'Tajawal', sans-serif;
        font-size: 0.85rem;
        transition: all 0.2s ease;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 6px;
        background: linear-gradient(to right, var(--primary-color), #4a8af4);
        color: white;
        width: 100%;
        touch-action: manipulation;
    }
    
    .date-btn:hover {
        background: linear-gradient(to right, #1e3f7a, #2c5aa0);
        transform: translateY(-2px);
    }
    
    .selected-date-display {
        background: white;
        padding: 10px 12px;
        border-radius: 6px;
        border: 2px solid var(--present-color);
        margin-top: 10px;
        display: inline-block;
        font-weight: 700;
        color: #333;
        font-size: 0.85rem;
    }
    
    /* قسم التحكم في الإدارة */
    .management-controls {
        display: none;
        margin-top: 20px;
        padding-top: 15px;
        border-top: 1px dashed #ccc;
    }
    
    .management-controls.active {
        display: block;
    }
    
    .management-grid {
        display: grid;
        grid-template-columns: 1fr;
        gap: 12px;
        margin-top: 12px;
    }
    
    @media (min-width: 768px) {
        .management-grid {
            grid-template-columns: repeat(2, 1fr);
        }
    }
    
    /* تحسينات للأجهزة الصغيرة جداً */
    @media screen and (max-width: 320px) {
        html, body {
            font-size: 12px;
        }
        
        .container {
            padding: 8px;
        }
        
        h2 {
            font-size: 1.1rem;
        }
        
        .student-name {
            font-size: 0.8rem;
            min-width: 70px;
            max-width: 90px;
        }
        
        .status-icon {
            width: 28px;
            height: 28px;
            font-size: 0.9rem;
        }
        
        .status-label {
            font-size: 0.65rem;
        }
        
        .admin-main-btn, .export {
            font-size: 0.85rem;
            padding: 10px 15px;
        }
    }
    
    /* تحسينات للأجهزة التي تدعم hover */
    @media (hover: hover) {
        .status-icon:hover, .status-btn:hover, .export:hover, .admin-main-btn:hover {
            transform: translateY(-2px);
        }
    }
    
    /* تحسينات للأجهزة التي لا تدعم hover (الهواتف) */
    @media (hover: none) {
        .status-icon:active, .status-btn:active, .export:active, .admin-main-btn:active {
            transform: scale(0.98);
        }
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
        0% { opacity: 0; transform: translateY(-10px); }
        15% { opacity: 1; transform: translateY(0); }
        85% { opacity: 1; transform: translateY(0); }
        100% { opacity: 0; transform: translateY(-10px); }
    }
    
    @keyframes fadeOut {
        from { opacity: 1; }
        to { opacity: 0; }
    }
    
    .status-icon.pulse {
        animation: pulse 0.3s ease;
    }
    
    /* منع التكبير التلقائي على iOS */
    input, textarea, select {
        font-size: 16px !important;
        -webkit-appearance: none;
        border-radius: 0;
    }
    
    /* تحسين التمرير على iOS */
    .table-wrapper {
        -webkit-overflow-scrolling: touch;
    }
    
    /* إصلاح مشكلة الهواتف التي تخفي شريط العناوين */
    @supports (-webkit-touch-callout: none) {
        body {
            padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
        }
    }
    
    /* إصلاح مشكلة iPhone X وما فوق */
    @media screen and (max-width: 375px) and (max-height: 812px) {
        body {
            padding-left: env(safe-area-inset-left);
            padding-right: env(safe-area-inset-right);
        }
    }
</style>
</head>
<body>

<div class="container" id="captureArea">
    <h2><i class="fas fa-chart-bar" style="margin-left: 4px;"></i> سجل التقويم الشامل للطلاب</h2>
    
    <!-- الجدول المعدل - بدون خانة التقييم -->
    <div class="table-wrapper">
        <table>
            <thead>
                <tr>
                    <th width="8%">الرقم</th>
                    <th width="25%">اسم الطالب</th>
                    <th width="12%">الواجبات</th>
                    <th width="12%" class="multiline-header">المشاريع</th>
                    <th width="12%" class="multiline-header">التطبيقات<br>والأنشطة</th>
                    <th width="12%">المشاركة</th>
                    <th width="10%">الحضور</th>
                </tr>
            </thead>
            <tbody>
                <!-- سيتم إنشاء الصفوف ديناميكيًا -->
            </tbody>
        </table>
    </div>
    
    <!-- زر الإدارة فقط في الواجهة الرئيسية -->
    <div class="admin-main-btn-container">
        <button class="admin-main-btn" onclick="toggleAdminPanel()">
            <i class="fas fa-cog"></i> فتح لوحة الإدارة
        </button>
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
                    <input type="password" class="password-input" id="passwordInput" placeholder="أدخل كلمة المرور هنا" autocomplete="off" inputmode="text">
                    <div class="password-buttons">
                        <button class="password-submit" onclick="checkPassword()">تحقق</button>
                        <button class="password-cancel" onclick="toggleAdminPanel()">إلغاء</button>
                    </div>
                </div>
                <p class="password-error" id="passwordError">
                    <i class="fas fa-exclamation-triangle"></i> كلمة المرور غير صحيحة
                </p>
            </div>
            
            <!-- قسم التحكم في الإدارة (يظهر بعد التحقق) -->
            <div class="management-controls" id="managementControls">
                <div class="admin-title">
                    <i class="fas fa-sliders-h"></i> أدوات التحكم المتقدمة
                </div>
                
                <div class="management-grid">
                    <!-- المهام الأدائية -->
                    <div class="category-controls admin-only">
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
                    
                    <!-- المشاركة والتفاعل -->
                    <div class="category-controls admin-only">
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
                    
                    <!-- الحضور -->
                    <div class="category-controls admin-only" style="grid-column: span 2;">
                        <div class="control-title">
                            <i class="fas fa-users"></i> التحكم في الحضور
                        </div>
                        <div class="attendance-controls">
                            <button class="status-btn present-btn" onclick="setAllAttendance('present')" style="flex: 1;">
                                <i class="fas fa-user-check"></i> تعيين الكل حاضر
                            </button>
                            <button class="status-btn absent-btn" onclick="setAllAttendance('absent')" style="flex: 1;">
                                <i class="fas fa-user-slash"></i> تعيين الكل غائب
                            </button>
                        </div>
                    </div>
                </div>
                
                <!-- أزرار التحكم الإضافية -->
                <div style="margin-top: 15px; display: flex; gap: 10px; justify-content: center;">
                    <button class="status-btn random-btn" id="randomBtnAdmin" onclick="toggleRandom()" style="flex: 1;">
                        <i class="fas fa-random"></i> تبديل عشوائي
                    </button>
                </div>
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
        <i class="fas fa-info-circle" style="margin-left: 4px;"></i> انقر على أيقونة أي تقييم للتبديل بين "صح" (أخضر)، "محايد" (رمادي)، "خطأ" (أحمر). الحضور يتبدل بين "حاضر" و"غائب" فقط.
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
        
        const attendanceInfo = getStatusInfo(student.attendance, 'attendance');
        const assignmentsInfo = getStatusInfo(student.assignments);
        const projectsInfo = getStatusInfo(student.projects);
        const classroomAppsInfo = getStatusInfo(student.classroomApps);
        const participationInfo = getStatusInfo(student.participation);
        
        row.innerHTML = `
            <td>${i}</td>
            <td class="student-name">${studentNames[i-1]}</td>
            
            <!-- Performance Tasks -->
            <td class="evaluation-cell multiline-cell">
                <div class="status-icon ${assignmentsInfo.class}-icon" onclick="toggleEvaluation(${i}, 'assignments')" id="assignments-${i}">
                    <i class="fas ${assignmentsInfo.icon}"></i>
                </div>
                <span class="status-label ${assignmentsInfo.class}-label" id="label-assignments-${i}">${assignmentsInfo.text}</span>
            </td>
            <td class="evaluation-cell multiline-cell">
                <div class="status-icon ${projectsInfo.class}-icon" onclick="toggleEvaluation(${i}, 'projects')" id="projects-${i}">
                    <i class="fas ${projectsInfo.icon}"></i>
                </div>
                <span class="status-label ${projectsInfo.class}-label" id="label-projects-${i}">${projectsInfo.text}</span>
            </td>
            
            <!-- Participation & Interaction -->
            <td class="evaluation-cell multiline-cell">
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
    
    if (adminPanel.classList.contains('active')) {
        // إغلاق لوحة الإدارة
        adminPanel.classList.remove('active');
        
        // إخفاء قسم التاريخ إذا كان ظاهرًا
        document.getElementById('dateSection').classList.remove('active');
        document.getElementById('managementControls').classList.remove('active');
        
        // إعادة تعيين كلمة المرور
        document.getElementById('passwordInput').value = '';
        document.getElementById('passwordError').style.display = 'none';
        
        // إخفاء قسم التحكم إذا كان ظاهرًا
        if (!isAdminAuthenticated) {
            document.getElementById('passwordSection').style.display = 'block';
        }
    } else {
        // فتح لوحة الإدارة
        adminPanel.classList.add('active');
        
        // إذا تم التحقق مسبقًا، إظهار أقسام الإدارة مباشرة
        if (isAdminAuthenticated) {
            document.getElementById('passwordSection').style.display = 'none';
            document.getElementById('managementControls').classList.add('active');
            document.getElementById('dateSection').classList.add('active');
            initializeDateField();
        } else {
            document.getElementById('passwordSection').style.display = 'block';
            document.getElementById('managementControls').classList.remove('active');
            document.getElementById('dateSection').classList.remove('active');
            // تأخير التركيز حتى تكتمل الرسوم المتحركة
            setTimeout(() => {
                document.getElementById('passwordInput').focus();
            }, 300);
        }
    }
}

// التحقق من كلمة المرور
function checkPassword() {
    const password = document.getElementById('passwordInput').value;
    const errorElement = document.getElementById('passwordError');
    const passwordSection = document.getElementById('passwordSection');
    const dateSection = document.getElementById('dateSection');
    const managementControls = document.getElementById('managementControls');
    
    // كلمة المرور الصحيحة: Jassar1436
    if (password === 'Jassar1436') {
        // تم التحقق بنجاح
        isAdminAuthenticated = true;
        document.getElementById('randomBtnAdmin').style.display = 'flex';
        
        // إخفاء قسم كلمة المرور وإظهار أقسام الإدارة
        passwordSection.style.display = 'none';
        managementControls.classList.add('active');
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
        top: 20px;
        right: 10px;
        left: 10px;
        background: ${type === 'present' ? 'var(--present-color)' : type === 'neutral' ? 'var(--neutral-color)' : 'var(--absent-color)'};
        color: white;
        padding: 12px 15px;
        border-radius: 8px;
        z-index: 10000;
        font-weight: bold;
        box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        animation: fadeInOut 3s ease-in-out;
        font-size: 0.9rem;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
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
    
    dateElement.style.cssText = 'text-align: left; margin-bottom: 10px; color: #666; font-size: 0.8rem; padding: 8px 10px; background: #f8f9fa; border-radius: 6px; border-right: 2px solid #2c5aa0;';
    dateElement.innerHTML = `<i class="far fa-calendar-alt" style="margin-left: 4px;"></i> ${dateText} - <i class="far fa-clock" style="margin-left: 4px;"></i> وقت التصدير: ${new Date().toLocaleTimeString('ar-SA', {hour: '2-digit', minute:'2-digit'})}`;
    
    const captureArea = document.getElementById('captureArea');
    const originalContent = captureArea.innerHTML;
    
    // إضافة التاريخ أعلى المحتوى
    captureArea.insertBefore(dateElement, captureArea.firstChild);
    
    // إغلاق لوحة الإدارة قبل التصوير
    const adminPanel = document.getElementById('adminPanel');
    const wasAdminPanelOpen = adminPanel.classList.contains('active');
    if (wasAdminPanelOpen) {
        adminPanel.classList.remove('active');
    }
    
    // إزالة أي تأثيرات CSS قد تؤثر على التصدير
    const originalContainerWidth = captureArea.style.width;
    const originalContainerMaxWidth = captureArea.style.maxWidth;
    const originalContainerPadding = captureArea.style.padding;
    const originalContainerBoxShadow = captureArea.style.boxShadow;
    const originalContainerBorder = captureArea.style.border;
    const originalContainerBorderRadius = captureArea.style.borderRadius;
    
    // تعيين أبعاد A4 للتصدير
    captureArea.style.width = '21cm';
    captureArea.style.maxWidth = '21cm';
    captureArea.style.padding = '20px';
    captureArea.style.boxShadow = 'none';
    captureArea.style.border = '1px solid #ccc';
    captureArea.style.borderRadius = '0';
    
    // تعيين خلفية بيضاء للتأكد من عدم وجود شفافية
    captureArea.style.backgroundColor = 'white';
    
    // إخفاء عناصر قد تسبب مشاكل في التصدير
    const adminMainBtnContainer = document.querySelector('.admin-main-btn-container');
    const originalAdminBtnDisplay = adminMainBtnContainer.style.display;
    adminMainBtnContainer.style.display = 'none';
    
    const exportContainer = document.querySelector('.export-container');
    const originalExportDisplay = exportContainer.style.display;
    exportContainer.style.display = 'none';
    
    const tableWrapper = document.querySelector('.table-wrapper');
    const originalTableWrapperMaxHeight = tableWrapper.style.maxHeight;
    tableWrapper.style.maxHeight = 'none';
    tableWrapper.style.overflow = 'visible';
    
    // إضافة تأخير قصير لضمان تطبيق التغييرات
    await new Promise(resolve => setTimeout(resolve, 300));
    
    const canvas = await html2canvas(captureArea, { 
        scale: 2,
        useCORS: true,
        backgroundColor: '#ffffff',
        scrollY: -window.scrollY,
        windowWidth: 794, // 21cm في 96 DPI
        windowHeight: 1123, // 29.7cm في 96 DPI
        logging: false,
        allowTaint: true,
        removeContainer: true
    });

    // إعادة المحتوى الأصلي
    captureArea.removeChild(dateElement);
    captureArea.style.width = originalContainerWidth;
    captureArea.style.maxWidth = originalContainerMaxWidth;
    captureArea.style.padding = originalContainerPadding;
    captureArea.style.boxShadow = originalContainerBoxShadow;
    captureArea.style.border = originalContainerBorder;
    captureArea.style.borderRadius = originalContainerBorderRadius;
    captureArea.style.backgroundColor = '';
    
    // إعادة عرض العناصر المخفية
    adminMainBtnContainer.style.display = originalAdminBtnDisplay;
    exportContainer.style.display = originalExportDisplay;
    tableWrapper.style.maxHeight = originalTableWrapperMaxHeight;
    tableWrapper.style.overflow = 'auto';
    
    // إعادة فتح لوحة الإدارة إذا كانت مفتوحة
    if (wasAdminPanelOpen) {
        adminPanel.classList.add('active');
    }
    
    const imgData = canvas.toDataURL("image/png", 1.0);
    const pdf = new jsPDF({
        orientation: "portrait",
        unit: "mm",
        format: "a4"
    });

    const imgWidth = 190; // A4 width in mm minus margins
    const pageHeight = 297; // A4 height in mm
    const imgHeight = (canvas.height * imgWidth) / canvas.width;
    
    // حساب عدد الصفحات المطلوبة
    let heightLeft = imgHeight;
    let position = 10; // هامش علوي
    const pageWidth = 210; // عرض A4
    
    // إضافة الصفحة الأولى
    pdf.addImage(imgData, "PNG", 10, position, imgWidth, imgHeight);
    heightLeft -= pageHeight - 20; // 20mm للهوامش العلوية والسفلية
    
    // إضافة صفحات إضافية إذا لزم الأمر
    while (heightLeft > 0) {
        position = heightLeft - imgHeight;
        pdf.addPage();
        pdf.addImage(imgData, "PNG", 10, position, imgWidth, imgHeight);
        heightLeft -= pageHeight - 20;
    }
    
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
    
    // تحسين تجربة الهواتف
    if ('ontouchstart' in window) {
        // إضافة فئة للجسم للإشارة إلى أن الجهاز يدعم اللمس
        document.body.classList.add('touch-device');
        
        // تحسين تجربة اللمس للأيقونات
        const statusIcons = document.querySelectorAll('.status-icon');
        statusIcons.forEach(icon => {
            icon.addEventListener('touchstart', function(e) {
                this.classList.add('active');
            }, { passive: true });
            
            icon.addEventListener('touchend', function(e) {
                this.classList.remove('active');
            }, { passive: true });
        });
    }
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

// إصلاح مشكلة iOS مع input type="date"
if (navigator.userAgent.match(/iPhone|iPad|iPod/i)) {
    const dateInput = document.getElementById('attendanceDate');
    if (dateInput) {
        dateInput.addEventListener('focus', function() {
            this.type = 'text';
            setTimeout(() => {
                this.type = 'date';
            }, 100);
        });
    }
}

// إضافة listener لتحسين تجربة الجوال
window.addEventListener('resize', function() {
    // إعادة حساب ارتفاع الجدول عند تغيير حجم الشاشة
    const tableWrapper = document.querySelector('.table-wrapper');
    if (window.innerHeight < 700) {
        tableWrapper.style.maxHeight = '30vh';
    } else {
        tableWrapper.style.maxHeight = '40vh';
    }
});

// تشغيل مرة واحدة عند التحميل
setTimeout(function() {
    const tableWrapper = document.querySelector('.table-wrapper');
    if (window.innerHeight < 700) {
        tableWrapper.style.maxHeight = '30vh';
    }
}, 500);

</script>

</body>
</html>
