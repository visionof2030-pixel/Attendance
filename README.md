<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>سجل متابعة الطلاب - مادة اللغة الإنجليزية</title>

<!-- خط جميل وواضح -->
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">

<!-- أيقونات -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- مكتبة moment للتواريخ -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.29.4/moment.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment-hijri/2.1.2/moment-hijri.min.js"></script>

<!-- مكتبة jsPDF مع دعم العربية -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>

<style>
    :root {
        --primary-color: #0d47a1;
        --primary-dark: #0a3a8a;
        --primary-light: #5472d3;
        --secondary-color: #1565c0;
        --success-color: #2e7d32;
        --success-dark: #1b5e20;
        --success-light: #4caf50;
        --danger-color: #c62828;
        --danger-dark: #b71c1c;
        --danger-light: #ef5350;
        --warning-color: #f57c00;
        --warning-light: #ff9800;
        --admin-color: #6a1b9a;
        --admin-dark: #4a148c;
        --light-color: #f5f7fa;
        --dark-color: #263238;
        --white: #ffffff;
        --gray-light: #eceff1;
        --gray-medium: #b0bec5;
        --shadow: rgba(0,0,0,0.08);
        --star-color: #ffd700;
    }

    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    body {
        font-family: "Cairo", sans-serif;
        background: linear-gradient(135deg, #f5f7fa 0%, #e4e8f0 100%);
        color: #333;
        line-height: 1.6;
        min-height: 100vh;
        transition: filter 0.3s ease;
    }

    /* تأثير blur على المحتوى الرئيسي فقط */
    body.blurred {
        overflow: hidden;
    }

    .main-blur {
        transition: filter 0.3s ease;
    }

    body.blurred .main-blur {
        filter: blur(5px);
        pointer-events: none;
        user-select: none;
    }

    /* إصلاح: منع تأثير blur على النوافذ المنبثقة */
    .password-modal,
    .admin-modal {
        filter: none !important;
        backdrop-filter: none !important;
    }

    /* ============= الهيدر مع زر Admin ============= */
    header {
        background: linear-gradient(90deg, var(--primary-dark), var(--primary-color));
        padding: 20px 15px;
        color: white;
        text-align: center;
        box-shadow: 0 4px 12px rgba(13, 71, 161, 0.2);
        border-bottom: 4px solid var(--secondary-color);
        position: sticky;
        top: 0;
        z-index: 100;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .header-content {
        flex: 1;
    }

    .school-logo {
        width: 50px;
        height: 50px;
        background: white;
        border-radius: 50%;
        margin: 0 auto 10px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: var(--primary-color);
        font-size: 24px;
        box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    }

    header h1 {
        margin: 0;
        font-size: 24px;
        font-weight: 700;
        text-shadow: 1px 1px 3px rgba(0,0,0,0.3);
    }

    header h2 {
        margin: 8px 0 0 0;
        font-size: 16px;
        opacity: .95;
        font-weight: 600;
    }

    .admin-btn {
        background: linear-gradient(135deg, var(--admin-color), #8e24aa);
        color: white;
        border: none;
        padding: 10px 20px;
        border-radius: 8px;
        font-size: 16px;
        font-weight: 700;
        cursor: pointer;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        gap: 8px;
        box-shadow: 0 3px 8px rgba(106, 27, 154, 0.3);
        margin-left: 10px;
        position: relative;
        overflow: hidden;
    }

    .admin-btn:hover {
        background: linear-gradient(135deg, var(--admin-dark), var(--admin-color));
        transform: translateY(-2px);
        box-shadow: 0 5px 12px rgba(106, 27, 154, 0.4);
    }

    .admin-btn i {
        font-size: 18px;
    }

    .admin-btn.clicked {
        animation: adminClick 0.4s ease;
    }

    @keyframes adminClick {
        0% { transform: scale(1); }
        50% { transform: scale(0.95); box-shadow: 0 1px 3px rgba(106, 27, 154, 0.4); }
        100% { transform: scale(1); }
    }

    /* ============= نافذة إدخال كلمة المرور ============= */
    .password-modal {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.7);
        z-index: 1000;
        align-items: center;
        justify-content: center;
        animation: fadeIn 0.3s ease;
        backdrop-filter: none;
    }

    .password-modal.active {
        display: flex;
    }

    .password-box {
        background: white;
        width: 90%;
        max-width: 400px;
        padding: 30px;
        border-radius: 15px;
        box-shadow: 0 15px 35px rgba(0,0,0,0.5);
        text-align: center;
        border-top: 5px solid var(--admin-color);
        transform: scale(0.9);
        animation: scaleIn 0.3s ease forwards;
        position: relative;
        z-index: 1001;
        filter: none !important;
    }

    @keyframes scaleIn {
        to { transform: scale(1); }
    }

    .password-box h3 {
        color: var(--admin-dark);
        margin-bottom: 20px;
        font-size: 22px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
    }

    .password-input {
        width: 100%;
        padding: 15px;
        font-size: 18px;
        border: 2px solid var(--gray-medium);
        border-radius: 10px;
        text-align: center;
        letter-spacing: 2px;
        font-family: "Cairo", sans-serif;
        margin-bottom: 20px;
        transition: all 0.3s ease;
        background: #f8f9fa;
    }

    .password-input:focus {
        border-color: var(--admin-color);
        box-shadow: 0 0 0 3px rgba(106, 27, 154, 0.2);
        outline: none;
        background: white;
    }

    .password-btn {
        background: linear-gradient(135deg, var(--admin-color), var(--admin-dark));
        color: white;
        border: none;
        padding: 12px 25px;
        border-radius: 10px;
        font-size: 16px;
        font-weight: 700;
        cursor: pointer;
        transition: all 0.3s ease;
        width: 100%;
        margin-bottom: 15px;
        position: relative;
        overflow: hidden;
    }

    .password-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 5px 15px rgba(106, 27, 154, 0.4);
    }

    .password-btn.clicked {
        animation: buttonClick 0.3s ease;
    }

    @keyframes buttonClick {
        0% { transform: scale(1); }
        50% { transform: scale(0.95); }
        100% { transform: scale(1); }
    }

    .password-error {
        color: var(--danger-color);
        font-size: 14px;
        margin-top: 10px;
        display: none;
        animation: shake 0.5s ease;
    }

    @keyframes shake {
        0%, 100% { transform: translateX(0); }
        10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
        20%, 40%, 60%, 80% { transform: translateX(5px); }
    }

    /* ============= نافذة Admin الرئيسية ============= */
    .admin-modal {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.75);
        z-index: 1001;
        overflow-y: auto;
        animation: fadeIn 0.3s ease;
        backdrop-filter: none;
    }

    .admin-modal.active {
        display: block;
    }

    .admin-container {
        background: white;
        width: 95%;
        max-width: 1000px;
        margin: 30px auto;
        padding: 30px;
        border-radius: 15px;
        box-shadow: 0 20px 50px rgba(0,0,0,0.5);
        border-top: 5px solid var(--admin-color);
        position: relative;
        max-height: 90vh;
        overflow-y: auto;
        animation: slideUp 0.4s ease;
        filter: none !important;
    }

    @keyframes slideUp {
        from { transform: translateY(50px); opacity: 0; }
        to { transform: translateY(0); opacity: 1; }
    }

    .admin-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 30px;
        padding-bottom: 15px;
        border-bottom: 2px solid var(--gray-light);
    }

    .admin-header h2 {
        color: var(--admin-dark);
        font-size: 26px;
        margin: 0;
        display: flex;
        align-items: center;
        gap: 10px;
    }

    .close-admin {
        background: var(--danger-color);
        color: white;
        border: none;
        width: 40px;
        height: 40px;
        border-radius: 50%;
        font-size: 20px;
        cursor: pointer;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .close-admin:hover {
        background: var(--danger-dark);
        transform: rotate(90deg);
    }

    .admin-section {
        margin-bottom: 30px;
        padding: 20px;
        border-radius: 10px;
        background: #f8f9fa;
        border: 1px solid var(--gray-light);
        box-shadow: 0 3px 10px rgba(0,0,0,0.05);
    }

    .admin-section h3 {
        color: var(--primary-dark);
        margin-bottom: 15px;
        font-size: 20px;
        display: flex;
        align-items: center;
        gap: 10px;
    }

    .admin-section h3 i {
        color: var(--admin-color);
    }

    /* ============= أزرار الحضور والغياب ============= */
    .attendance-buttons {
        display: flex;
        justify-content: center;
        gap: 12px;
    }

    .btn {
        padding: 12px 20px;
        border-radius: 10px;
        font-size: 16px;
        cursor: pointer;
        transition: all 0.3s ease;
        border: none;
        min-width: 85px;
        font-weight: 700;
        position: relative;
        overflow: hidden;
        box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        transform: translateY(0);
    }

    .present {
        background: linear-gradient(135deg, var(--success-color), var(--success-light));
        color: white;
        border: 2px solid transparent;
    }
    
    .present:hover {
        background: linear-gradient(135deg, var(--success-dark), var(--success-color));
        transform: translateY(-3px);
        box-shadow: 0 8px 15px rgba(46, 125, 50, 0.3);
    }
    
    .present.active {
        background: linear-gradient(135deg, var(--success-dark), var(--success-color));
        transform: scale(1.05) translateY(-2px);
        box-shadow: 0 0 0 4px rgba(76, 175, 80, 0.3), 0 8px 20px rgba(46, 125, 50, 0.4);
        animation: pulse-present 2s infinite;
        border: 2px solid #ffffff;
    }

    @keyframes pulse-present {
        0% {
            box-shadow: 0 0 0 0 rgba(76, 175, 80, 0.7);
        }
        70% {
            box-shadow: 0 0 0 15px rgba(76, 175, 80, 0);
        }
        100% {
            box-shadow: 0 0 0 0 rgba(76, 175, 80, 0);
        }
    }

    .absent {
        background: linear-gradient(135deg, var(--danger-color), var(--danger-light));
        color: white;
        border: 2px solid transparent;
    }
    
    .absent:hover {
        background: linear-gradient(135deg, var(--danger-dark), var(--danger-color));
        transform: translateY(-3px);
        box-shadow: 0 8px 15px rgba(198, 40, 40, 0.3);
    }
    
    .absent.active {
        background: linear-gradient(135deg, var(--danger-dark), var(--danger-color));
        transform: scale(1.05) translateY(-2px);
        box-shadow: 0 0 0 4px rgba(239, 83, 80, 0.3), 0 8px 20px rgba(198, 40, 40, 0.4);
        animation: pulse-absent 2s infinite;
        border: 2px solid #ffffff;
    }

    @keyframes pulse-absent {
        0% {
            box-shadow: 0 0 0 0 rgba(239, 83, 80, 0.7);
        }
        70% {
            box-shadow: 0 0 0 15px rgba(239, 83, 80, 0);
        }
        100% {
            box-shadow: 0 0 0 0 rgba(239, 83, 80, 0);
        }
    }

    .btn.clicked {
        animation: enhancedClick 0.4s ease;
    }

    @keyframes enhancedClick {
        0% { transform: scale(1) translateY(0); }
        30% { transform: scale(0.85) translateY(2px); box-shadow: 0 2px 5px rgba(0,0,0,0.2); }
        70% { transform: scale(1.05) translateY(-1px); }
        100% { transform: scale(1) translateY(0); }
    }

    .confirmation-indicator {
        position: absolute;
        top: -10px;
        right: -10px;
        background: white;
        color: var(--success-dark);
        border-radius: 50%;
        width: 28px;
        height: 28px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 15px;
        box-shadow: 0 3px 8px rgba(0,0,0,0.2);
        z-index: 1;
        opacity: 0;
        transform: scale(0) rotate(0deg);
        transition: all 0.5s ease;
        border: 2px solid var(--success-dark);
    }

    .confirmation-indicator.show {
        opacity: 1;
        transform: scale(1) rotate(360deg);
        animation: bounceIn 0.5s ease;
    }

    @keyframes bounceIn {
        0% { transform: scale(0.3) rotate(0deg); opacity: 0; }
        50% { transform: scale(1.2) rotate(180deg); opacity: 1; }
        100% { transform: scale(1) rotate(360deg); opacity: 1; }
    }

    .confirmation-indicator.absent-check {
        color: var(--danger-dark);
        border-color: var(--danger-dark);
    }

    /* ============= بقية التصميم ============= */
    .class-selector {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        gap: 10px;
        margin: 20px 15px;
        padding: 15px;
        background: var(--white);
        border-radius: 10px;
        box-shadow: 0 4px 12px var(--shadow);
        border: 1px solid var(--gray-light);
    }

    .class-selector button {
        background: var(--primary-color);
        color: white;
        border: none;
        padding: 12px 18px;
        border-radius: 8px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
        transition: all 0.3s ease;
        flex: 1;
        min-width: 80px;
        max-width: 110px;
        box-shadow: 0 2px 5px rgba(13, 71, 161, 0.2);
        position: relative;
        overflow: hidden;
    }

    .class-selector button:hover {
        background: var(--primary-dark);
        transform: translateY(-3px);
        box-shadow: 0 4px 8px rgba(13, 71, 161, 0.3);
    }

    .class-selector button.active {
        background: var(--secondary-color);
        transform: translateY(-2px);
        box-shadow: 0 4px 8px rgba(21, 101, 192, 0.4);
        animation: activePulse 2s infinite;
    }

    @keyframes activePulse {
        0% { box-shadow: 0 4px 8px rgba(21, 101, 192, 0.4); }
        50% { box-shadow: 0 6px 12px rgba(21, 101, 192, 0.6); }
        100% { box-shadow: 0 4px 8px rgba(21, 101, 192, 0.4); }
    }

    .date-container {
        width: 95%;
        max-width: 1200px;
        margin: 20px auto;
        background: var(--white);
        padding: 25px;
        border-radius: 12px;
        box-shadow: 0 6px 20px var(--shadow);
        border: 1px solid var(--gray-light);
        border-top: 5px solid var(--secondary-color);
    }

    .date-row {
        display: flex;
        gap: 25px;
        flex-wrap: wrap;
    }

    .date-group {
        flex: 1;
        min-width: 280px;
    }

    .date-group label {
        font-weight: 700;
        margin-bottom: 10px;
        font-size: 17px;
        color: var(--primary-dark);
        display: flex;
        align-items: center;
        gap: 10px;
    }

    .date-group label i {
        color: var(--primary-color);
        background: var(--gray-light);
        padding: 8px;
        border-radius: 8px;
    }

    .date-input {
        padding: 15px;
        font-size: 16px;
        border-radius: 10px;
        border: 2px solid var(--gray-medium);
        font-family: "Cairo", sans-serif;
        width: 100%;
        transition: all 0.3s ease;
        background: var(--white);
    }

    .date-input:focus {
        border-color: var(--primary-color);
        box-shadow: 0 0 0 3px rgba(13, 71, 161, 0.1);
        outline: none;
    }

    .conversion-notice {
        background: #e8f5e9;
        border: 1px solid #a5d6a7;
        border-radius: 8px;
        padding: 12px 16px;
        margin-top: 10px;
        font-size: 14px;
        color: #2e7d32;
        display: flex;
        align-items: center;
        gap: 12px;
        animation: fadeIn 0.5s ease;
    }

    .container {
        width: 95%;
        max-width: 1200px;
        margin: 25px auto;
        background: var(--white);
        padding: 25px;
        border-radius: 12px;
        box-shadow: 0 6px 20px var(--shadow);
        border: 1px solid var(--gray-light);
        overflow-x: auto;
        border-top: 5px solid var(--primary-color);
    }

    .container h3 {
        text-align: center;
        color: var(--primary-dark);
        margin-bottom: 25px;
        font-size: 22px;
        padding-bottom: 15px;
        border-bottom: 2px solid var(--gray-light);
    }

    .table-wrapper {
        overflow-x: auto;
        border-radius: 10px;
        border: 1px solid var(--gray-light);
        box-shadow: 0 3px 10px rgba(0,0,0,0.05);
    }

    table {
        width: 100%;
        border-collapse: separate;
        border-spacing: 0;
        min-width: 700px;
    }

    th {
        background: linear-gradient(135deg, var(--primary-color), var(--primary-dark));
        color: white;
        font-size: 17px;
        font-weight: 700;
        padding: 18px 15px;
        border: none;
        position: sticky;
        top: 0;
        z-index: 10;
    }

    td {
        padding: 16px 15px;
        text-align: center;
        border-bottom: 1px solid var(--gray-light);
        background: var(--white);
        transition: background 0.3s ease;
    }

    tr:hover td {
        background: #f8f9fa;
    }

    .student-name {
        font-size: 18px;
        font-weight: 700;
        text-align: right;
        padding-right: 20px;
        color: var(--dark-color);
        border-right: 3px solid var(--primary-light);
        position: relative;
    }

    .student-name .fa-star {
        cursor: pointer;
        margin-right: 10px;
        font-size: 18px;
    }

    .notes-cell textarea {
        width: 100%;
        padding: 10px;
        border-radius: 8px;
        border: 1px solid var(--gray-medium);
        font-family: "Cairo", sans-serif;
        font-size: 14px;
        min-height: 60px;
        resize: vertical;
    }

    .notes-cell textarea:focus {
        outline: none;
        border-color: var(--primary-color);
        box-shadow: 0 0 0 2px rgba(13, 71, 161, 0.1);
    }

    .pdf-container {
        width: 95%;
        max-width: 1200px;
        margin: 30px auto;
    }

    #exportPDF {
        width: 100%;
        background: linear-gradient(135deg, #5e35b1, #4527a0);
        padding: 22px;
        border: none;
        border-radius: 12px;
        color: white;
        font-size: 22px;
        font-weight: 800;
        cursor: pointer;
        transition: all 0.3s ease;
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 15px;
        box-shadow: 0 6px 15px rgba(94, 53, 177, 0.3);
        letter-spacing: 0.5px;
        text-transform: uppercase;
        border: 2px solid transparent;
        position: relative;
        overflow: hidden;
    }

    #exportPDF:hover {
        background: linear-gradient(135deg, #4527a0, #311b92);
        transform: translateY(-4px);
        box-shadow: 0 10px 20px rgba(69, 39, 160, 0.4);
        border-color: rgba(255, 255, 255, 0.2);
    }

    #exportPDF.clicked {
        animation: pdfClick 0.4s ease;
    }

    @keyframes pdfClick {
        0% { transform: scale(1) translateY(0); }
        30% { transform: scale(0.95) translateY(2px); }
        70% { transform: scale(1.05) translateY(-1px); }
        100% { transform: scale(1) translateY(0); }
    }

    .notification {
        position: fixed;
        top: 20px;
        left: 50%;
        transform: translateX(-50%) translateY(-100px);
        background: var(--success-color);
        color: white;
        padding: 15px 25px;
        border-radius: 10px;
        box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        z-index: 1000;
        display: flex;
        align-items: center;
        gap: 12px;
        font-size: 16px;
        font-weight: 600;
        transition: transform 0.4s ease;
    }

    .notification.show {
        transform: translateX(-50%) translateY(0);
        animation: notificationSlide 0.4s ease;
    }

    @keyframes notificationSlide {
        0% { transform: translateX(-50%) translateY(-100px); }
        70% { transform: translateX(-50%) translateY(10px); }
        100% { transform: translateX(-50%) translateY(0); }
    }

    .notification.error {
        background: var(--danger-color);
    }

    .notification.warning {
        background: var(--warning-color);
    }

    @keyframes fadeIn {
        from { opacity: 0; }
        to { opacity: 1; }
    }

    /* ============= التجاوب ============= */
    @media (max-width: 768px) {
        .admin-container {
            width: 98%;
            padding: 20px;
            margin: 15px auto;
        }
        
        .admin-header h2 {
            font-size: 22px;
        }
        
        header {
            flex-direction: column;
            gap: 10px;
            padding: 15px 10px;
        }
        
        .admin-btn {
            margin-left: 0;
            margin-top: 10px;
        }
        
        .btn {
            padding: 10px 15px;
            font-size: 14px;
            min-width: 70px;
        }
        
        .confirmation-indicator {
            width: 22px;
            height: 22px;
            font-size: 13px;
            top: -8px;
            right: -8px;
        }
        
        .student-name {
            font-size: 16px;
        }
    }

    ::-webkit-scrollbar {
        width: 10px;
        height: 10px;
    }

    ::-webkit-scrollbar-track {
        background: #f1f1f1;
        border-radius: 5px;
    }

    ::-webkit-scrollbar-thumb {
        background: var(--primary-light);
        border-radius: 5px;
    }

    ::-webkit-scrollbar-thumb:hover {
        background: var(--primary-color);
    }
</style>
</head>
<body>

<div class="main-blur">
    <header>
        <button class="admin-btn" onclick="showAdminLogin()">
            <i class="fas fa-user-shield"></i> Admin
        </button>
        
        <div class="header-content">
            <div class="school-logo">
                <i class="fas fa-graduation-cap"></i>
            </div>
            <h1>سجل متابعة الطلاب - النظام الأكاديمي</h1>
            <h2>مادة اللغة الإنجليزية — المعلم: فهد الخالدي</h2>
        </div>
    </header>

    <!-- اختيار الفصل -->
    <div class="class-selector">
        <button onclick="showClass('c3_1')" class="active">الصف ٣/١</button>
        <button onclick="showClass('c2_3')">الصف ٢/٣</button>
        <button onclick="showClass('c3_3')">الصف ٣/٣</button>
        <button onclick="showClass('c4_3')">الصف ٤/٣</button>
        <button onclick="showClass('c5_3')">الصف ٥/٣</button>
    </div>

    <!-- التاريخ -->
    <div class="date-container">
        <div class="date-row">
            <div class="date-group">
                <label for="gregorianDate">
                    <i class="fas fa-calendar-alt"></i>
                    التاريخ الميلادي:
                </label>
                <input type="date" id="gregorianDate" class="date-input">
                <div id="gregorianNotice" class="conversion-notice" style="display: none;">
                    <i class="fas fa-sync-alt"></i>
                    <span>سيتم تحويل التاريخ تلقائياً إلى الهجري</span>
                </div>
            </div>
            
            <div class="date-group">
                <label for="hijriDate">
                    <i class="fas fa-moon"></i>
                    التاريخ الهجري:
                </label>
                <input type="text" id="hijriDate" class="date-input" placeholder="يوم / شهر / سنة هـ (مثال: 15 / 9 / 1445)">
                <div id="hijriNotice" class="conversion-notice" style="display: none;">
                    <i class="fas fa-sync-alt"></i>
                    <span>سيتم تحويل التاريخ تلقائياً إلى الميلادي</span>
                </div>
            </div>
        </div>
        
        <div style="text-align: center;">
            <button id="todayBtn" class="btn" style="background: var(--warning-color); color: white;">
                <i class="fas fa-calendar-day"></i> تعيين تاريخ اليوم
            </button>
        </div>
    </div>

    <!-- 🔥 محتوى الفصول -->
    <div id="classContent"></div>

    <!-- PDF -->
    <div class="pdf-container">
        <button id="exportPDF" onclick="generatePDF()">
            <i class="fas fa-file-pdf"></i>
            <span>تصدير التقرير بصيغة PDF</span>
        </button>
    </div>
</div>

<!-- نافذة إدخال كلمة مرور Admin -->
<div class="password-modal" id="passwordModal">
    <div class="password-box">
        <h3><i class="fas fa-lock"></i> الوصول الإداري</h3>
        <p>الرجاء إدخال كلمة المرور للوصول إلى لوحة التحكم</p>
        <input type="password" id="adminPassword" class="password-input" placeholder="كلمة المرور" autocomplete="off">
        <div class="password-error" id="passwordError">
            <i class="fas fa-exclamation-circle"></i> كلمة المرور غير صحيحة
        </div>
        <button class="password-btn" onclick="checkAdminPassword()" id="loginBtn">
            <i class="fas fa-sign-in-alt"></i> دخول
        </button>
        <button class="password-btn" style="background: var(--gray-medium);" onclick="hideAdminLogin()">
            <i class="fas fa-times"></i> إلغاء
        </button>
    </div>
</div>

<!-- نافذة Admin الرئيسية -->
<div class="admin-modal" id="adminModal">
    <div class="admin-container">
        <div class="admin-header">
            <h2><i class="fas fa-cogs"></i> لوحة التحكم الإدارية</h2>
            <button class="close-admin" onclick="hideAdminPanel()">
                <i class="fas fa-times"></i>
            </button>
        </div>
        
        <!-- الميزة 1: النجوم للطلاب المميزين -->
        <div class="admin-section">
            <h3><i class="fas fa-star"></i> النجوم للطلاب المميزين</h3>
            <p>إضافة علامة نجمية للطلاب المميزين تظهر بجانب أسمائهم</p>
            
            <div style="display: flex; gap: 15px; margin-bottom: 15px; flex-wrap: wrap;">
                <div style="flex: 1; min-width: 200px;">
                    <strong>الطلاب الحاليين:</strong>
                    <select id="studentSelect" onchange="showSelectedStudent()" style="width: 100%; padding: 10px; margin-top: 5px;">
                        <option value="">اختر طالباً</option>
                    </select>
                </div>
                <div style="display: flex; align-items: center; gap: 15px;">
                    <i class="fas fa-star student-star" id="toggleStar" onclick="toggleStudentStar()" 
                       style="font-size: 30px; color: #ccc; cursor: pointer;"></i>
                    <span id="starStatus">غير مميز</span>
                </div>
            </div>
            
            <div id="starredStudentsList" style="max-height: 200px; overflow-y: auto; padding: 10px; background: white; border-radius: 8px;">
                <h4>الطلاب المميزين:</h4>
                <!-- سيتم ملؤها بالجافاسكريبت -->
            </div>
        </div>
        
        <!-- الميزة 2: التحضير العشوائي - معدّلة -->
        <div class="admin-section">
            <h3><i class="fas fa-random"></i> التحضير العشوائي للفصول</h3>
            <p>تعيين حضور عشوائي للفصل المحدد بنسبة 75% حضور و 25% غياب</p>
            
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 20px;">
                <div>
                    <label for="randomClassSelect"><i class="fas fa-chalkboard"></i> الفصل:</label>
                    <select id="randomClassSelect" style="width: 100%; padding: 10px; margin-top: 5px;">
                        <option value="all">جميع الفصول</option>
                        <option value="c3_1">الصف ٣/١</option>
                        <option value="c2_3">الصف ٢/٣</option>
                        <option value="c3_3">الصف ٣/٣</option>
                        <option value="c4_3">الصف ٤/٣</option>
                        <option value="c5_3">الصف ٥/٣</option>
                    </select>
                </div>
                
                <div>
                    <label><i class="fas fa-calendar"></i> التاريخ المستخدم:</label>
                    <div style="padding: 10px; margin-top: 5px; background: #f8f9fa; border-radius: 5px; border: 1px solid #ddd;">
                        <span id="currentDateDisplay">سيستخدم التاريخ المحدد في الأعلى</span>
                    </div>
                </div>
            </div>
            
            <div style="background: #fff3cd; border: 1px solid #ffeaa7; border-radius: 8px; padding: 15px; margin-bottom: 20px;">
                <h4 style="color: #856404; margin-top: 0;"><i class="fas fa-info-circle"></i> ملاحظة:</h4>
                <p style="color: #856404; margin-bottom: 0;">
                    سيتم استخدام التاريخ المحدد في الأعلى (التاريخ الميلادي) لتوليد التحضير العشوائي.
                    نسبة التحضير: 75% حضور و 25% غياب.
                </p>
            </div>
            
            <button onclick="generateRandomAttendance()" style="background: linear-gradient(135deg, var(--warning-color), var(--warning-light)); color: white; border: none; padding: 12px 25px; border-radius: 10px; font-size: 16px; font-weight: 700; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 10px; margin-top: 10px; width: 100%;">
                <i class="fas fa-magic"></i> توليد تحضير عشوائي
            </button>
        </div>
        
        <!-- الميزة 3: إدارة الطلاب -->
        <div class="admin-section">
            <h3><i class="fas fa-users-cog"></i> إدارة الطلاب</h3>
            <p>إضافة طلاب جدد أو نقلهم بين الفصول مع الحفاظ على البيانات</p>
            
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px;">
                <div style="background: white; padding: 20px; border-radius: 10px;">
                    <h4><i class="fas fa-user-plus"></i> إضافة طالب جديد</h4>
                    <div style="margin-bottom: 15px;">
                        <label for="newStudentName">اسم الطالب:</label>
                        <input type="text" id="newStudentName" placeholder="اسم الطالب الكامل" style="width: 100%; padding: 10px; margin-top: 5px;">
                    </div>
                    <div style="margin-bottom: 15px;">
                        <label for="newStudentClass">الفصل:</label>
                        <select id="newStudentClass" style="width: 100%; padding: 10px; margin-top: 5px;">
                            <option value="c3_1">الصف ٣/١</option>
                            <option value="c2_3">الصف ٢/٣</option>
                            <option value="c3_3">الصف ٣/٣</option>
                            <option value="c4_3">الصف ٤/٣</option>
                            <option value="c5_3">الصف ٥/٣</option>
                        </select>
                    </div>
                    <button onclick="addNewStudent()" style="background: linear-gradient(135deg, var(--success-color), var(--success-light)); color: white; border: none; padding: 10px; border-radius: 8px; width: 100%; cursor: pointer;">
                        <i class="fas fa-plus"></i> إضافة طالب
                    </button>
                </div>
                
                <div style="background: white; padding: 20px; border-radius: 10px;">
                    <h4><i class="fas fa-exchange-alt"></i> نقل طالب بين الفصول</h4>
                    <div style="margin-bottom: 15px;">
                        <label for="transferStudentSelect">الطالب:</label>
                        <select id="transferStudentSelect" style="width: 100%; padding: 10px; margin-top: 5px;">
                            <option value="">اختر طالباً</option>
                        </select>
                    </div>
                    <div style="margin-bottom: 15px;">
                        <label for="transferToClass">الفصل الجديد:</label>
                        <select id="transferToClass" style="width: 100%; padding: 10px; margin-top: 5px;">
                            <option value="c3_1">الصف ٣/١</option>
                            <option value="c2_3">الصف ٢/٣</option>
                            <option value="c3_3">الصف ٣/٣</option>
                            <option value="c4_3">الصف ٤/٣</option>
                            <option value="c5_3">الصف ٥/٣</option>
                        </select>
                    </div>
                    <button onclick="transferStudent()" style="background: linear-gradient(135deg, var(--primary-color), var(--primary-light)); color: white; border: none; padding: 10px; border-radius: 8px; width: 100%; cursor: pointer;">
                        <i class="fas fa-exchange-alt"></i> نقل طالب
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- إشعارات -->
<div class="notification" id="notification"></div>

<script>
// ================== تهيئة المتغيرات ==================
const ADMIN_PASSWORD = "Jassar1436";
let starredStudents = JSON.parse(localStorage.getItem('starredStudents')) || {};
let studentsData = JSON.parse(localStorage.getItem('studentsData')) || {};
let attendanceData = JSON.parse(localStorage.getItem('attendanceData')) || {};
let isAdminLoggedIn = false;
let isConverting = false;

// ================== إدارة Admin ==================
function showAdminLogin() {
    // تأثير النقر على زر Admin
    const adminBtn = document.querySelector('.admin-btn');
    adminBtn.classList.add('clicked');
    setTimeout(() => {
        adminBtn.classList.remove('clicked');
    }, 400);
    
    // إظهار نافذة كلمة المرور
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
    // تأثير النقر على زر الدخول
    const loginBtn = document.getElementById('loginBtn');
    loginBtn.classList.add('clicked');
    setTimeout(() => {
        loginBtn.classList.remove('clicked');
    }, 300);
    
    const password = document.getElementById('adminPassword').value;
    const errorElement = document.getElementById('passwordError');
    
    if (password === ADMIN_PASSWORD) {
        isAdminLoggedIn = true;
        hideAdminLogin();
        showAdminPanel();
        showNotification('تم الدخول بنجاح إلى لوحة التحكم', 'success');
    } else {
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
    document.body.classList.add('blurred');
    loadAdminData();
    updateAdminDateDisplay();
}

function hideAdminPanel() {
    document.getElementById('adminModal').classList.remove('active');
    document.body.classList.remove('blurred');
}

function updateAdminDateDisplay() {
    const gregorianDate = document.getElementById('gregorianDate').value;
    const hijriDate = document.getElementById('hijriDate').value;
    const dateDisplay = document.getElementById('currentDateDisplay');
    
    if (gregorianDate && hijriDate) {
        dateDisplay.textContent = `${gregorianDate} (${hijriDate})`;
    } else if (gregorianDate) {
        dateDisplay.textContent = gregorianDate;
    } else {
        dateDisplay.textContent = 'الرجاء تحديد تاريخ في الأعلى';
    }
}

// ================== الميزة 1: النجوم للطلاب المميزين ==================
function loadAdminData() {
    updateStudentSelects();
    updateStarredStudentsList();
}

function updateStudentSelects() {
    const allStudents = getAllStudents();
    const studentSelect = document.getElementById('studentSelect');
    const transferSelect = document.getElementById('transferStudentSelect');
    
    studentSelect.innerHTML = '<option value="">اختر طالباً</option>';
    transferSelect.innerHTML = '<option value="">اختر طالباً</option>';
    
    allStudents.forEach(student => {
        const option = document.createElement('option');
        option.value = student.id;
        option.textContent = `${student.name} (${student.className})`;
        
        studentSelect.appendChild(option);
        transferSelect.appendChild(option.cloneNode(true));
    });
}

function getAllStudents() {
    const allStudents = [];
    const classes = ['c3_1', 'c2_3', 'c3_3', 'c4_3', 'c5_3'];
    const classNames = {
        'c3_1': '٣/١',
        'c2_3': '٢/٣',
        'c3_3': '٣/٣',
        'c4_3': '٤/٣',
        'c5_3': '٥/٣'
    };
    
    classes.forEach(classId => {
        const classData = studentsData[classId] || getDefaultStudents(classId);
        classData.forEach((student, index) => {
            allStudents.push({
                id: `${classId}-${index}`,
                name: student,
                classId: classId,
                className: classNames[classId]
            });
        });
    });
    
    return allStudents;
}

function showSelectedStudent() {
    const studentId = document.getElementById('studentSelect').value;
    const starIcon = document.getElementById('toggleStar');
    const starStatus = document.getElementById('starStatus');
    
    if (studentId) {
        const isStarred = starredStudents[studentId] || false;
        starIcon.style.color = isStarred ? '#ffd700' : '#ccc';
        starIcon.classList.toggle('active', isStarred);
        starStatus.textContent = isStarred ? 'مميز بنجمة' : 'غير مميز';
    }
}

function toggleStudentStar() {
    const studentId = document.getElementById('studentSelect').value;
    
    if (!studentId) {
        showNotification('الرجاء اختيار طالب أولاً', 'warning');
        return;
    }
    
    starredStudents[studentId] = !starredStudents[studentId];
    localStorage.setItem('starredStudents', JSON.stringify(starredStudents));
    
    const starIcon = document.getElementById('toggleStar');
    const starStatus = document.getElementById('starStatus');
    
    starIcon.style.color = starredStudents[studentId] ? '#ffd700' : '#ccc';
    starStatus.textContent = starredStudents[studentId] ? 'مميز بنجمة' : 'غير مميز';
    
    updateStarredStudentsList();
    updateClassDisplay();
    
    showNotification(
        starredStudents[studentId] ? 'تم إضافة نجمة للطالب' : 'تم إزالة النجمة من الطالب',
        'success'
    );
}

function updateStarredStudentsList() {
    const listElement = document.getElementById('starredStudentsList');
    const allStudents = getAllStudents();
    
    let html = '<h4>الطلاب المميزين:</h4>';
    const starredList = allStudents.filter(student => starredStudents[student.id]);
    
    if (starredList.length === 0) {
        html += '<p style="color: #b0bec5; text-align: center; padding: 20px;">لا يوجد طلاب مميزين</p>';
    } else {
        starredList.forEach(student => {
            html += `
                <div style="display: flex; justify-content: space-between; align-items: center; padding: 8px 12px; border-bottom: 1px solid #eee;">
                    <span>${student.name}</span>
                    <span style="color: #0d47a1; font-weight: 600;">${student.className}</span>
                    <i class="fas fa-star" style="color: #ffd700;"></i>
                </div>
            `;
        });
    }
    
    listElement.innerHTML = html;
}

function getStudentStarElement(studentId, studentName) {
    const isStarred = starredStudents[studentId] || false;
    
    return `
        <div class="student-name">
            ${studentName}
            <i class="fas fa-star" onclick="toggleStudentStarInline('${studentId}', this)" 
               style="color: ${isStarred ? '#ffd700' : '#ccc'}; cursor: pointer; margin-right: 10px; font-size: 18px;"
               title="${isStarred ? 'طالب مميز' : 'إضافة نجمة'}">
            </i>
        </div>
    `;
}

function toggleStudentStarInline(studentId, element) {
    if (!isAdminLoggedIn) {
        showNotification('يجب تسجيل الدخول كمسؤول لتعديل النجوم', 'warning');
        showAdminLogin();
        return;
    }
    
    starredStudents[studentId] = !starredStudents[studentId];
    localStorage.setItem('starredStudents', JSON.stringify(starredStudents));
    
    element.style.color = starredStudents[studentId] ? '#ffd700' : '#ccc';
    element.title = starredStudents[studentId] ? 'طالب مميز' : 'إضافة نجمة';
    
    updateStarredStudentsList();
    showNotification(starredStudents[studentId] ? 'تم إضافة نجمة للطالب' : 'تم إزالة النجمة من الطالب', 'success');
}

// ================== الميزة 2: التحضير العشوائي - معدّلة ==================
function generateRandomAttendance() {
    const classId = document.getElementById('randomClassSelect').value;
    const gregorianDate = document.getElementById('gregorianDate').value;
    
    if (!gregorianDate) {
        showNotification('الرجاء تحديد التاريخ الميلادي أولاً', 'warning');
        return;
    }
    
    if (classId === 'all') {
        ['c3_1', 'c2_3', 'c3_3', 'c4_3', 'c5_3'].forEach(cid => {
            generateClassRandomAttendance(cid, gregorianDate);
        });
        showNotification('تم توليد تحضير عشوائي لجميع الفصول', 'success');
    } else {
        generateClassRandomAttendance(classId, gregorianDate);
        showNotification(`تم توليد تحضير عشوائي للفصل ${classId}`, 'success');
    }
    
    // تحديث عرض الفصل الحالي إذا كان ضمن الفصول المحددة
    const activeButton = document.querySelector('.class-selector button.active');
    const activeClassId = activeButton.getAttribute('onclick').match(/'([^']+)'/)[1];
    
    if (classId === 'all' || classId === activeClassId) {
        // تحديث الواجهة لتظهر التحضير العشوائي
        setTimeout(() => {
            applyRandomAttendanceToUI(activeClassId, gregorianDate);
        }, 500);
    }
}

function generateClassRandomAttendance(classId, date) {
    if (!attendanceData[classId]) {
        attendanceData[classId] = {};
    }
    
    const classStudents = studentsData[classId] || getDefaultStudents(classId);
    
    // توليد بيانات عشوائية بنسبة 75% حضور و25% غياب
    attendanceData[classId][date] = {};
    
    classStudents.forEach((student, index) => {
        const random = Math.random(); // قيمة بين 0 و 1
        const isPresent = random < 0.75; // 75% احتمال حضور
        
        attendanceData[classId][date][index] = {
            present: isPresent,
            absent: !isPresent,
            note: isPresent ? 'حضور عشوائي' : 'غياب عشوائي'
        };
    });
    
    localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
}

function applyRandomAttendanceToUI(classId, date) {
    if (!attendanceData[classId] || !attendanceData[classId][date]) {
        return;
    }
    
    const classAttendance = attendanceData[classId][date];
    const rows = document.querySelectorAll("#classContent table tbody tr");
    
    rows.forEach((row, index) => {
        if (classAttendance[index]) {
            const { present, absent, note } = classAttendance[index];
            
            if (present) {
                row.cells[1].querySelector('.btn.present').classList.add('active');
                row.cells[2].querySelector('.btn.absent').classList.remove('active');
            } else if (absent) {
                row.cells[1].querySelector('.btn.present').classList.remove('active');
                row.cells[2].querySelector('.btn.absent').classList.add('active');
            }
            
            if (note) {
                row.cells[3].querySelector('textarea').value = note;
            }
        }
    });
    
    showNotification('تم تطبيق التحضير العشوائي على الواجهة', 'success');
}

// ================== الميزة 3: إدارة الطلاب ==================
function addNewStudent() {
    const name = document.getElementById('newStudentName').value.trim();
    const classId = document.getElementById('newStudentClass').value;
    
    if (!name) {
        showNotification('الرجاء إدخال اسم الطالب', 'warning');
        return;
    }
    
    if (!studentsData[classId]) {
        studentsData[classId] = getDefaultStudents(classId);
    }
    
    studentsData[classId].push(name);
    localStorage.setItem('studentsData', JSON.stringify(studentsData));
    
    updateStudentSelects();
    const activeButton = document.querySelector('.class-selector button.active');
    if (activeButton && activeButton.getAttribute('onclick').includes(classId)) {
        loadClassStudents(classId);
    }
    
    document.getElementById('newStudentName').value = '';
    showNotification(`تم إضافة الطالب ${name} إلى الفصل بنجاح`, 'success');
}

function transferStudent() {
    const studentId = document.getElementById('transferStudentSelect').value;
    const newClassId = document.getElementById('transferToClass').value;
    
    if (!studentId) {
        showNotification('الرجاء اختيار طالب للنقل', 'warning');
        return;
    }
    
    const [oldClassId, studentIndex] = studentId.split('-');
    const studentIndexNum = parseInt(studentIndex);
    
    if (oldClassId === newClassId) {
        showNotification('الطالب موجود بالفعل في هذا الفصل', 'warning');
        return;
    }
    
    if (!studentsData[oldClassId] || !studentsData[oldClassId][studentIndexNum]) {
        showNotification('لم يتم العثور على بيانات الطالب', 'error');
        return;
    }
    
    const studentName = studentsData[oldClassId][studentIndexNum];
    
    if (!studentsData[newClassId]) {
        studentsData[newClassId] = getDefaultStudents(newClassId);
    }
    studentsData[newClassId].push(studentName);
    studentsData[oldClassId].splice(studentIndexNum, 1);
    
    localStorage.setItem('studentsData', JSON.stringify(studentsData));
    updateStudentSelects();
    
    const activeButton = document.querySelector('.class-selector button.active');
    if (activeButton) {
        const activeClassId = activeButton.getAttribute('onclick').match(/'([^']+)'/)[1];
        if (activeClassId === oldClassId || activeClassId === newClassId) {
            loadClassStudents(activeClassId);
        }
    }
    
    showNotification(`تم نقل الطالب ${studentName} إلى الفصل الجديد`, 'success');
}

// ================== النظام الرئيسي ==================
function convertToHijri(gregorianDate) {
    if (!gregorianDate || isConverting) return;
    
    isConverting = true;
    try {
        const m = moment(gregorianDate);
        const hijriDate = m.format('iD / iM / iYYYY هـ');
        document.getElementById('hijriDate').value = hijriDate;
        
        const notice = document.getElementById('gregorianNotice');
        notice.style.display = 'flex';
        setTimeout(() => {
            notice.style.display = 'none';
        }, 3000);
        
        saveDatesToStorage();
    } catch (error) {
        console.error('خطأ في التحويل إلى التاريخ الهجري:', error);
    } finally {
        setTimeout(() => { isConverting = false; }, 100);
    }
}

function convertToGregorian(hijriDateString) {
    if (!hijriDateString || isConverting) return;
    
    isConverting = true;
    try {
        let cleaned = hijriDateString.replace(/هـ/g, '').trim();
        const numbers = cleaned.match(/\d+/g);
        
        if (numbers && numbers.length >= 3) {
            const day = parseInt(numbers[0]);
            const month = parseInt(numbers[1]);
            const year = parseInt(numbers[2]);
            
            if (day >= 1 && day <= 30 && month >= 1 && month <= 12 && year >= 1300 && year <= 1500) {
                const hijriMoment = moment().iYear(year).iMonth(month - 1).iDate(day);
                const gregorianDate = hijriMoment.format('YYYY-MM-DD');
                document.getElementById('gregorianDate').value = gregorianDate;
                
                const notice = document.getElementById('hijriNotice');
                notice.style.display = 'flex';
                setTimeout(() => {
                    notice.style.display = 'none';
                }, 3000);
                
                saveDatesToStorage();
            }
        }
    } catch (error) {
        console.error('خطأ في التحويل إلى التاريخ الميلادي:', error);
    } finally {
        setTimeout(() => { isConverting = false; }, 100);
    }
}

function setToday() {
    const today = new Date();
    const formattedDate = today.toISOString().split('T')[0];
    document.getElementById('gregorianDate').value = formattedDate;
    convertToHijri(formattedDate);
    
    const btn = document.getElementById('todayBtn');
    btn.classList.add('clicked');
    setTimeout(() => {
        btn.classList.remove('clicked');
    }, 300);
}

function saveDatesToStorage() {
    const gregorianDate = document.getElementById('gregorianDate').value;
    const hijriDate = document.getElementById('hijriDate').value;
    
    if (gregorianDate && hijriDate) {
        localStorage.setItem('lastGregorianDate', gregorianDate);
        localStorage.setItem('lastHijriDate', hijriDate);
    }
}

function loadDatesFromStorage() {
    const savedGregorian = localStorage.getItem('lastGregorianDate');
    const savedHijri = localStorage.getItem('lastHijriDate');
    
    if (savedGregorian) {
        document.getElementById('gregorianDate').value = savedGregorian;
    }
    
    if (savedHijri) {
        document.getElementById('hijriDate').value = savedHijri;
    } else if (savedGregorian) {
        convertToHijri(savedGregorian);
    }
}

// تحميل الفصول
function showClass(classId) {
    document.querySelectorAll(".class-selector button").forEach(btn => {
        btn.classList.remove("active");
        btn.classList.remove('clicked');
    });
    event.target.classList.add("active");
    
    // تأثير النقر على زر الفصل
    event.target.classList.add('clicked');
    setTimeout(() => {
        event.target.classList.remove('clicked');
    }, 300);
    
    loadClassStudents(classId);
}

// الحصول على الطلاب الافتراضيين
function getDefaultStudents(classId) {
    const defaultClasses = {
        "c3_1": [
            "إسماعيل محمد هاشم شفيق الرحمن",
            "ابراهيم علي ابو بكر محمد",
            "باسم محمد - ابو طالب",
            "حسين بشير أمادو جازير",
            "حسين هارون عثمان عبدالمؤمن ادم",
            "حمد محمد عثمان بخش",
            "رمضان عيسى باكور محمد",
            "ريان عبد الرحمن موسى جيبو",
            "ريحان محمد مقبول حسين عمر حمزه",
            "عامر مولوي حسن شريف",
            "عبدالحليم نور كبير - صديق احمد",
            "عمران يعقوب محمد محمد مسلم",
            "عمير محمد محمد شفيع حكيم علي",
            "فارس محمد ابو البشر واعظ علي",
            "محمد احمد فضل الرحمن فايز اللّٰه",
            "حمد انوار رشيد احمد اظهار مياه",
            "حمد عبدالرزاق محمد عبدالقادر",
            "حمد عبدالشكور عبدالحميد عبد الرشيد",
            "مهدي محمد محمد اسلام عبدالسلام",
            "مهدي موسى حميد الحق احمد",
            "ياسين محمد يوسف"
        ],
        "c2_3": [
            "إبراهيم إدريس إبراهيم اولوجيوم",
            "إدريس محمد حسن أحمد",
            "امين عبداللّه دايابو عثمان",
            "بسام عبدالسلام هاشم انور علي",
            "حافظ بيلو موسى سليمان",
            "حسين علي حسن مهاوش",
            "خالد طيب اسماعيل محمد",
            "خالد عبد الحميد محمد هاشم",
            "خالد وليد محمد محمد",
            "ريان عبدالرحمن عمر نانتومي",
            "سليمان ابراهيم ديقوقا",
            "صالح عبدالله محمد قاسم يوسف علي عبدالعزيز اول اودو محمد",
            "عثمان عبد الرحمن باي محمد",
            "عدنان نور امير حسين",
            "عمر سراج محمد زكريا",
            "فهد محمد حسين عبداللّه مياه حسين محمد ابراهيم سعيد هو ساوي محمد محمد امين اسلام خليل الرحمن مشعل ابو طاهر ناظر حسين عبدالمطلب موسى ابو بكر الصديق عبدالجبار امة علي",
            "يوسف مهدي عابدين محمد"
        ],
        "c3_3": [
            "ابراهيم جزولي - اسدانور",
            "تركي عبدالصمد عبدالغني محمد حسين",
            "حسام حسن ابو الكلام مقبول احمد",
            "حسن عيسى بكوري محمد",
            "سعد سلام ستار ارشاد اللّٰه",
            "عايض سيف الاسلام نور احمد علي عبدالكريم عثمان ابكر كوجو",
            "عزام شمس العالم قاسم علي",
            "عماد محمد صديق محمد شفيع سيد عمر عبد القدوس عبدالسلام عبد السبحان عمر مورتلا أبو بكر محمد",
            "فيصل احمد ابو بكر محمد",
            "محمد اسحاق محمد اسلام عبدالحكيم",
            "محمد عبدالله ابو سعيد مياه",
            "حمد محمد اسماعيل امير حسين ابو بكر",
            "حمد موسى ساليفو ديقوقا",
            "مشاري شيهو اسماعيل محمد بكر",
            "ياسر عبدالرحيم محمد علي سفر علي",
            "يوسف محمد عبد الرحمن علي"
        ],
        "c4_3": [
            "ابراهيم عوض احمد فليس",
            "احمد ابراهيم ابن زكريا الهوسه",
            "احمد عبد القيوم محمد يعقوب",
            "اسماعيل اول اودو محمد",
            "اوسامة سعيدو دو غويد",
            "تامر عبد الصمد عبد الغني",
            "تركي هارون حسن شريف",
            "ريان محمد مقبول حسين حسين",
            "ريان هارون الرشيد طفيل احمد نذير احمد",
            "عبدالحليم محمد عبدالله عبدالحكيم",
            "عبدالله حفيظ اللّٰه سلطان أحمد",
            "عيسى عثمان سعيد عالم حبيب الرحمن",
            "فهد أسار رشيد احمد",
            "فهد محمد نور مقبول اشرف",
            "محمد محمد ادريس نبية حسين يعقوب علي",
            "مصلح محمد ولي احمد",
            "معاذ عثمان صديق كالو",
            "يوسف بدماسي ابراهيم البد ماسي"
        ],
        "c5_3": [
            "ابراهيم خالد سليمان ابراهيم",
            "انس عبدالعزيز نور احمد",
            "بدر بكر عمر محمد",
            "حمد محمد حسين مياه شمس العالم اظهر مياه",
            "رضوان رشيد أحمد نور محمد لال مياه",
            "سعيد عبدالله سعيد محمد",
            "عامر رحمة اللّٰه محمد شفيع",
            "عبد اللّٰه حسين علي فليس",
            "عبد العزيز سراج ابكر عثمان",
            "عبدالله عيسى - ابراهيم",
            "عمر محمد عمر صالح",
            "غسان عثمان اسماعيل عبدالله عبد اللّٰه",
            "فاضل عادل صالح الرايس",
            "محمد فريد كبير احمد عباد اللّٰه",
            "محمد محمد سلطان احمد محمد",
            "محمد موسى أدامو محمد",
            "محمد نور محمد زكريا آمال حسين",
            "مشاري محمد هارو",
            "مشاري يعقوب أبو بكر ابراهيم",
            "منذر علي عمر قوني",
            "هود حسن عبدالكريم الياس",
            "يعقوب محمد إسحاق يار محمد فضل على"
        ]
    };
    
    return studentsData[classId] || defaultClasses[classId] || [];
}

function loadClassStudents(classId) {
    const classStudents = getDefaultStudents(classId);
    
    let html = `
    <div class='container'>
        <h3>قائمة طلاب الفصل ${classId.replace('c', '').replace('_', '/')}</h3>
        <div class="table-wrapper">
        <table>
            <thead>
                <tr>
                    <th width="40%">اسم الطالب</th>
                    <th width="15%">حضور</th>
                    <th width="15%">غياب</th>
                    <th width="30%">ملاحظات</th>
                </tr>
            </thead>
            <tbody>`;

    classStudents.forEach((name, index) => {
        const studentId = `${classId}-${index}`;
        
        html += `
                <tr>
                    <td>${getStudentStarElement(studentId, name)}</td>
                    <td>
                        <div style="position: relative;">
                            <div class="confirmation-indicator" id="present-indicator-${classId}-${index}">
                                <i class="fas fa-check"></i>
                            </div>
                            <button class="btn present" onclick="toggleSelect(this,'present', '${classId}-${index}')">
                                <i class="fas fa-user-check"></i> حضور
                            </button>
                        </div>
                    </td>
                    <td>
                        <div style="position: relative;">
                            <div class="confirmation-indicator absent-check" id="absent-indicator-${classId}-${index}">
                                <i class="fas fa-times"></i>
                            </div>
                            <button class="btn absent" onclick="toggleSelect(this,'absent', '${classId}-${index}')">
                                <i class="fas fa-user-times"></i> غياب
                            </button>
                        </div>
                    </td>
                    <td class="notes-cell"><textarea placeholder="اكتب ملاحظاتك هنا..."></textarea></td>
                </tr>`;
    });

    html += `</tbody></table></div></div>`;
    document.getElementById("classContent").innerHTML = html;
    
    // إضافة النجوم المحفوظة
    updateClassDisplay();
    
    // تطبيق بيانات التحضير المخزنة للتاريخ الحالي
    const gregorianDate = document.getElementById('gregorianDate').value;
    if (gregorianDate) {
        setTimeout(() => {
            applyRandomAttendanceToUI(classId, gregorianDate);
        }, 100);
    }
}

function updateClassDisplay() {
    const rows = document.querySelectorAll("#classContent table tbody tr");
    rows.forEach((row, index) => {
        const classId = document.querySelector('.class-selector button.active')
            .getAttribute('onclick').match(/'([^']+)'/)[1];
        const studentId = `${classId}-${index}`;
        const starIcon = row.querySelector('.student-name i.fa-star');
        
        if (starIcon && starredStudents[studentId]) {
            starIcon.style.color = '#ffd700';
            starIcon.title = 'طالب مميز';
        }
    });
}

// ================== تفعيل/تعطيل الحضور والغياب مع حفظ البيانات ==================
function toggleSelect(btn, type, indicatorId) {
    const row = btn.closest("tr");
    
    // تأثير النقر المحسّن
    btn.classList.add('clicked');
    setTimeout(() => {
        btn.classList.remove('clicked');
    }, 400);
    
    // إزالة النشاط من جميع الأزرار في الصف
    row.querySelectorAll(".btn.present, .btn.absent")
        .forEach(b => b.classList.remove("active"));
    
    // إزالة جميع مؤشرات التأكيد في الصف
    const indicators = row.querySelectorAll(".confirmation-indicator");
    indicators.forEach(indicator => {
        indicator.classList.remove("show");
    });
    
    // تفعيل الزر المحدد
    btn.classList.add("active");
    
    // إظهار مؤشر التأكيد المناسب
    if (type === 'present') {
        const presentIndicator = document.getElementById(`present-indicator-${indicatorId}`);
        if (presentIndicator) {
            presentIndicator.classList.add("show");
            setTimeout(() => {
                presentIndicator.classList.remove("show");
            }, 3000);
        }
        row.querySelector(".btn.absent").classList.remove("active");
    } else {
        const absentIndicator = document.getElementById(`absent-indicator-${indicatorId}`);
        if (absentIndicator) {
            absentIndicator.classList.add("show");
            setTimeout(() => {
                absentIndicator.classList.remove("show");
            }, 3000);
        }
        row.querySelector(".btn.present").classList.remove("active");
    }
    
    // حفظ حالة الحضور
    const [classId, index] = indicatorId.split('-');
    const gregorianDate = document.getElementById('gregorianDate').value;
    
    if (gregorianDate) {
        if (!attendanceData[classId]) {
            attendanceData[classId] = {};
        }
        if (!attendanceData[classId][gregorianDate]) {
            attendanceData[classId][gregorianDate] = {};
        }
        
        attendanceData[classId][gregorianDate][index] = {
            present: type === 'present',
            absent: type === 'absent',
            note: row.querySelector('.notes-cell textarea').value || ''
        };
        
        localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
    }
}

// ================== استخراج PDF مع دعم العربية الكامل ==================
function generatePDF() {
    // تأثير النقر على زر PDF
    const pdfBtn = document.getElementById('exportPDF');
    pdfBtn.classList.add('clicked');
    setTimeout(() => {
        pdfBtn.classList.remove('clicked');
    }, 400);
    
    try {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF({
            orientation: 'landscape',
            unit: 'mm',
            format: 'a4'
        });
        
        // الحصول على البيانات
        const gregorianDate = document.getElementById("gregorianDate").value || "غير محدد";
        const hijriDate = document.getElementById("hijriDate").value || "غير محدد";
        const activeClass = document.querySelector(".class-selector button.active").textContent;
        
        // عنوان التقرير
        doc.setFontSize(20);
        doc.setTextColor(13, 71, 161); // أزرق
        doc.text("تقرير متابعة الطلاب - مادة اللغة الإنجليزية", 150, 20, { align: 'center' });
        
        doc.setFontSize(16);
        doc.setTextColor(33, 33, 33);
        doc.text("المعلم: فهد الخالدي", 150, 30, { align: 'center' });
        
        // معلومات الفصل والتاريخ
        doc.setFontSize(12);
        doc.text(`الفصل: ${activeClass}`, 15, 45);
        doc.text(`التاريخ الميلادي: ${gregorianDate}`, 15, 55);
        doc.text(`التاريخ الهجري: ${hijriDate}`, 15, 65);
        
        // جمع بيانات الطلاب من الجدول
        const tableRows = document.querySelectorAll("#classContent table tbody tr");
        const tableData = [];
        
        tableRows.forEach((row, index) => {
            const name = row.cells[0].querySelector('.student-name').textContent.trim();
            const isPresent = row.cells[1].querySelector(".btn.present").classList.contains("active");
            const isAbsent = row.cells[2].querySelector(".btn.absent").classList.contains("active");
            const notes = row.cells[3].querySelector("textarea").value || "";
            
            let status = "لم يتم التحديد";
            let statusColor = [128, 128, 128]; // رمادي
            
            if (isPresent) {
                status = "حاضر";
                statusColor = [46, 125, 50]; // أخضر
            } else if (isAbsent) {
                status = "غائب";
                statusColor = [198, 40, 40]; // أحمر
            }
            
            tableData.push([
                (index + 1).toString(),
                name,
                status,
                notes || "لا توجد ملاحظات"
            ]);
        });
        
        // إنشاء الجدول في PDF
        doc.autoTable({
            startY: 75,
            head: [['#', 'اسم الطالب', 'الحالة', 'ملاحظات']],
            body: tableData,
            theme: 'grid',
            styles: {
                font: 'helvetica',
                fontSize: 10,
                textColor: [33, 33, 33],
                cellPadding: 5,
                overflow: 'linebreak',
                halign: 'right',
                valign: 'middle'
            },
            headStyles: {
                fillColor: [13, 71, 161], // أزرق داكن
                textColor: [255, 255, 255],
                fontStyle: 'bold',
                halign: 'center'
            },
            bodyStyles: {
                halign: 'right'
            },
            columnStyles: {
                0: { cellWidth: 15, halign: 'center' }, // العمود الأول (الرقم)
                1: { cellWidth: 60, halign: 'right' },  // اسم الطالب
                2: { 
                    cellWidth: 25, 
                    halign: 'center',
                    fontStyle: 'bold'
                }, // الحالة
                3: { cellWidth: 70, halign: 'right' }   // الملاحظات
            },
            didParseCell: function(data) {
                // تلوين خلية الحالة
                if (data.column.index === 2 && data.row.index > 0) {
                    const status = data.cell.raw;
                    if (status === "حاضر") {
                        data.cell.styles.textColor = [46, 125, 50]; // أخضر
                    } else if (status === "غائب") {
                        data.cell.styles.textColor = [198, 40, 40]; // أحمر
                    }
                }
            },
            margin: { left: 15, right: 15 }
        });
        
        // توقيع وتاريخ الإصدار
        const finalY = doc.lastAutoTable.finalY + 20;
        doc.setFontSize(10);
        doc.text("توقيع المعلم: ________________", 15, finalY);
        doc.text(`تاريخ الإصدار: ${new Date().toLocaleDateString('ar-SA')}`, 260, finalY);
        doc.text("نظام متابعة الطلاب - الإصدار 1.0", 150, finalY + 10, { align: 'center' });
        
        // حفظ الملف
        const fileName = `تقرير-الحضور-${activeClass.replace('/', '-')}-${gregorianDate}.pdf`;
        doc.save(fileName);
        
        showNotification("تم تصدير التقرير بنجاح بصيغة PDF!", "success");
    } catch (error) {
        console.error("خطأ في إنشاء PDF:", error);
        showNotification("حدث خطأ أثناء إنشاء التقرير", "error");
    }
}

// ================== إشعارات ==================
function showNotification(message, type = 'success') {
    const notification = document.getElementById('notification');
    notification.textContent = message;
    notification.className = `notification ${type}`;
    notification.classList.add('show');
    
    setTimeout(() => {
        notification.classList.remove('show');
    }, 3000);
}

// ================== تهيئة النظام عند التحميل ==================
window.onload = function() {
    // تحميل الفصل الأول
    showClass("c3_1");
    
    // تحميل التواريخ المحفوظة
    loadDatesFromStorage();
    
    // إذا لم تكن هناك تواريخ محفوظة، تعيين التاريخ الحالي
    if (!localStorage.getItem('lastGregorianDate')) {
        setToday();
    }
    
    // إضافة مستمعي الأحداث للتواريخ
    document.getElementById('gregorianDate').addEventListener('change', function(e) {
        convertToHijri(e.target.value);
        if (document.getElementById('adminModal').classList.contains('active')) {
            updateAdminDateDisplay();
        }
        
        // تحديث بيانات التحضير عند تغيير التاريخ
        const activeButton = document.querySelector('.class-selector button.active');
        if (activeButton) {
            const activeClassId = activeButton.getAttribute('onclick').match(/'([^']+)'/)[1];
            setTimeout(() => {
                applyRandomAttendanceToUI(activeClassId, e.target.value);
            }, 300);
        }
    });
    
    document.getElementById('hijriDate').addEventListener('input', function(e) {
        clearTimeout(window.hijriTimeout);
        window.hijriTimeout = setTimeout(() => {
            convertToGregorian(e.target.value);
        }, 500);
    });
    
    document.getElementById('hijriDate').addEventListener('blur', function(e) {
        convertToGregorian(e.target.value);
    });
    
    // زر اليوم الحالي
    document.getElementById('todayBtn').addEventListener('click', setToday);
    
    // السماح بإدخال كلمة المرور بالضغط على Enter
    document.getElementById('adminPassword').addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            checkAdminPassword();
        }
    });
    
    // تحميل بيانات Admin
    updateStudentSelects();
    updateStarredStudentsList();
};

// ================== السماح لجميع الوظائف بالعمل ==================
window.showClass = showClass;
window.toggleSelect = toggleSelect;
window.generatePDF = generatePDF;
window.showAdminLogin = showAdminLogin;
window.checkAdminPassword = checkAdminPassword;
window.hideAdminLogin = hideAdminLogin;
window.showAdminPanel = showAdminPanel;
window.hideAdminPanel = hideAdminPanel;
window.toggleStudentStar = toggleStudentStar;
window.showSelectedStudent = showSelectedStudent;
window.generateRandomAttendance = generateRandomAttendance;
window.addNewStudent = addNewStudent;
window.transferStudent = transferStudent;
window.toggleStudentStarInline = toggleStudentStarInline;
window.setToday = setToday;
</script>
</body>
</html>
