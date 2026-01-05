<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>المدير الذكي 2026</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <style>
        :root {
            --primary: #4361ee; --primary-dark: #3a0ca3;
            --bg: #f8f9fa; --surface: #ffffff;
            --text: #2b2d42; --text-light: #8d99ae;
            --border: #e0e0e0;
            --work: #4caf50; --holiday: #ffc107; --sick: #ff9800;
            --absent: #f44336; --eid: #9c27b0; --recup: #00bcd4;
            --radius: 16px;
        }

        body.dark-mode {
            --bg: #121212; --surface: #1e1e1e;
            --text: #e0e0e0; --text-light: #b0b0b0;
            --border: #333;
            --primary: #5c7cfa;
        }

        * { box-sizing: border-box; touch-action: manipulation; -webkit-tap-highlight-color: transparent; }
        body { font-family: 'Cairo', sans-serif; background-color: var(--bg); margin: 0; padding-bottom: 80px; color: var(--text); transition: background 0.3s, color 0.3s; }

        /* --- Print Styles (A4 Setup) --- */
        @media print {
            body * { visibility: hidden; }
            #print-area, #print-area * { visibility: visible; }
            #print-area { position: absolute; left: 0; top: 0; width: 100%; background: white; color: black; padding: 20px; direction: rtl; }
            .print-header { text-align: center; border-bottom: 2px solid #333; padding-bottom: 10px; margin-bottom: 20px; }
            .print-table { width: 100%; border-collapse: collapse; font-size: 12px; }
            .print-table th, .print-table td { border: 1px solid #333; padding: 6px; text-align: center; }
            .print-table th { background-color: #f0f0f0; }
            .no-print { display: none !important; }
        }
        #print-area { display: none; } /* Hidden normally */
        .show-print #print-area { display: block; } /* Visible when printing class added */

        /* Auth & App Styles (Standard) */
        #auth-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: linear-gradient(135deg, var(--primary), #4cc9f0); z-index: 9999; display: flex; justify-content: center; align-items: center; flex-direction: column; }
        .auth-card { background: rgba(255,255,255,0.98); padding: 30px; border-radius: 24px; width: 90%; max-width: 380px; text-align: center; box-shadow: 0 10px 40px rgba(0,0,0,0.2); }
        body.dark-mode .auth-card { background: #1e1e1e; color: #fff; }
        .input-group { position: relative; margin-bottom: 15px; }
        .app-input { width: 100%; padding: 12px; border: 2px solid var(--border); border-radius: 12px; font-family: inherit; font-size: 1rem; outline: none; transition: 0.3s; background: var(--surface); color: var(--text); }
        .app-input:focus { border-color: var(--primary); }
        .btn-main { width: 100%; padding: 12px; background: var(--primary); color: white; border: none; border-radius: 12px; font-weight: bold; cursor: pointer; margin-top: 10px; }
        .btn-secondary { background: transparent; color: var(--primary); border: 2px solid var(--primary); margin-top: 10px; }
        .btn-close-modal { width: 100%; padding: 12px; margin-top: 15px; background: var(--bg); color: var(--text-light); border: 1px solid var(--border); border-radius: 12px; font-weight: bold; cursor: pointer; display: flex; justify-content: center; align-items: center; gap: 8px; }
        .error-msg, .success-msg { display: none; padding: 8px; border-radius: 8px; margin-top: 10px; font-size: 0.85rem; }
        .error-msg { color: #d32f2f; background: #ffebee; } .success-msg { color: #2e7d32; background: #e8f5e9; }
        .view-section { display: none; } .view-section.active { display: block; }
        .loading { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5); z-index:10000; display:none; justify-content:center; align-items:center; }

        #app-container { display: none; padding: 15px; max-width: 600px; margin: 0 auto; }
        .header { display: flex; flex-wrap: wrap; justify-content: space-between; align-items: center; background: var(--surface); padding: 15px; border-radius: var(--radius); box-shadow: 0 4px 20px rgba(0,0,0,0.05); margin-bottom: 20px; gap: 10px; }
        .header-info { display: flex; flex-direction: column; min-width: 120px; }
        .app-main-title { margin: 0; font-size: 1.2rem; color: var(--text); font-weight: bold; }
        .user-sub-title { font-size: 0.9rem; color: var(--text-light); }
        .header-actions { display: flex; gap: 8px; }
        .action-btn { background: var(--bg); border: 1px solid var(--border); width: 36px; height: 36px; border-radius: 10px; cursor: pointer; font-size: 1.1rem; display: flex; justify-content: center; align-items: center; color: var(--text-light); position: relative; }
        .badge-count { position: absolute; top: -5px; left: -5px; background: #f44336; color: white; font-size: 0.7rem; width: 18px; height: 18px; border-radius: 50%; display: none; justify-content: center; align-items: center; border: 2px solid white; }

        .stats-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; margin-bottom: 20px; }
        .stat-card { background: var(--surface); padding: 15px; border-radius: var(--radius); text-align: center; box-shadow: 0 4px 20px rgba(0,0,0,0.05); cursor: pointer; }
        .stat-card h4 { margin: 0; font-size: 0.75rem; color: var(--text-light); }
        .stat-card .val { font-size: 1.3rem; font-weight: 700; color: var(--text); }
        .stat-card .sub { font-size: 0.6rem; color: #999; }
        .full-width { grid-column: span 2; }
        .txt-red { color: #f44336 !important; } .txt-green { color: #4caf50 !important; }

        /* Chart moved down */
        .chart-box { background: var(--surface); padding: 15px; border-radius: var(--radius); box-shadow: 0 4px 20px rgba(0,0,0,0.05); margin-top: 20px; margin-bottom: 20px; height: 260px; position: relative; }

        .calendar-box { background: var(--surface); border-radius: var(--radius); padding: 15px; box-shadow: 0 4px 20px rgba(0,0,0,0.05); }
        .cal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
        .days-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 6px; }
        .day-name { font-size: 0.75rem; color: var(--text-light); text-align: center; font-weight: bold; }
        .day-cell { aspect-ratio: 1; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; padding-top: 5px; border-radius: 10px; background: var(--bg); cursor: pointer; position: relative; border: 1px solid transparent; }
        .day-cell span { font-weight: bold; font-size: 0.9rem; color: var(--text); z-index: 2; position: absolute; top: 4px; right: 6px; line-height: 1; }
        .day-cell.today { border-color: var(--primary); background: rgba(67, 97, 238, 0.1) !important; }
        .day-cell.weekend { background-color: rgba(200,200,255,0.15); border: 1px dashed var(--border); }
        body.dark-mode .day-cell.weekend { background-color: rgba(255,255,255,0.05); }
        .day-cell.future { opacity: 0.5; cursor: default; }
        
        /* Cell Colors */
        .day-cell.st-work { background-color: var(--work) !important; color: white !important; }
        .day-cell.st-holiday { background-color: var(--holiday) !important; color: #333 !important; }
        .day-cell.st-sick { background-color: var(--sick) !important; color: white !important; }
        .day-cell.st-absent { background-color: var(--absent) !important; color: white !important; }
        .day-cell.st-eid { background-color: var(--eid) !important; color: white !important; }
        .day-cell.st-recup { background-color: var(--recup) !important; color: white !important; }
        .day-cell[class*="st-"] span { color: white !important; }
        .day-cell.st-holiday span { color: #333 !important; }
        .day-cell.nat-holiday { background-color: #f8bbd0 !important; border: 2px solid #ec407a !important; color: #880e4f !important; }
        body.dark-mode .day-cell.nat-holiday { background-color: #4a1c2d !important; color: #fce4ec !important; }

        .legend-container { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; margin-top: 20px; padding: 10px; background: var(--bg); border-radius: var(--radius); }
        .legend-dot { width: 20px; height: 20px; border-radius: 50%; cursor: pointer; border: 2px solid var(--surface); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .lg-work { background-color: var(--work); } .lg-holiday { background-color: var(--holiday); } .lg-sick { background-color: var(--sick); } .lg-absent { background-color: var(--absent); } .lg-recup { background-color: var(--recup); } .lg-eid { background-color: var(--eid); } .lg-nat { background-color: #f8bbd0; border: 2px solid #ec407a; }

        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); z-index: 2000; display: none; justify-content: center; align-items: flex-end; backdrop-filter: blur(2px); }
        .modal-content { background: var(--surface); width: 100%; max-width: 500px; border-radius: 24px 24px 0 0; padding: 25px; animation: slideUp 0.3s; max-height: 85vh; overflow-y: auto; color: var(--text); }
        @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
        
        .preset-item, .search-item, .detail-item { display: flex; justify-content: space-between; align-items: center; background: var(--bg); padding: 12px; border-radius: 10px; margin-bottom: 6px; font-size: 0.9rem; border-left: 4px solid transparent; }
        .detail-item.pos { border-left-color: var(--work); } .detail-item.neg { border-left-color: var(--absent); } .detail-item.neutral { border-left-color: var(--primary); }
        .del-icon { color: red; font-weight: bold; padding: 5px 10px; cursor: pointer; background: var(--surface); border-radius: 5px; }
        .d-val { font-weight: bold; direction: ltr; font-family: monospace; font-size: 1rem; }
        .details-header { font-weight: bold; margin: 15px 0 10px; color: var(--primary); font-size: 0.95rem; border-bottom: 2px solid var(--border); padding-bottom: 5px; }

        /* NFC Specific Styles */
        .nfc-scanning { animation: pulse 1.5s infinite; border-color: var(--primary); color: var(--primary); }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(67, 97, 238, 0.4); } 70% { box-shadow: 0 0 0 10px rgba(67, 97, 238, 0); } 100% { box-shadow: 0 0 0 0 rgba(67, 97, 238, 0); } }
        .switch-container { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; background: var(--bg); padding: 10px; border-radius: 10px; }
        .switch { position: relative; display: inline-block; width: 40px; height: 24px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #ccc; transition: .4s; border-radius: 24px; }
        .slider:before { position: absolute; content: ""; height: 16px; width: 16px; left: 4px; bottom: 4px; background-color: white; transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--primary); }
        input:checked + .slider:before { transform: translateX(16px); }

    </style>
</head>
<body>

    <div id="loader" class="loading"><div style="width:40px;height:40px;border:4px solid #ddd;border-top-color:var(--primary);border-radius:50%;animation:spin 1s infinite"></div></div>
    <div id="legend-toast"></div>

    <!-- Hidden Print Area -->
    <div id="print-area"></div>

    <!-- Auth System -->
    <div id="auth-overlay">
        <div class="auth-card">
            <div id="view-login" class="view-section active">
                <h2 style="color:var(--primary-dark)">نظام الحضور الذكي</h2>
                <div class="input-group"><input type="email" id="login-email" class="app-input" placeholder="البريد الإلكتروني"></div>
                <div class="input-group"><input type="password" id="login-pass" class="app-input" placeholder="كلمة المرور"></div>
                <div style="margin-bottom:15px;"><input type="checkbox" id="remember-me"> <label>تذكرني</label></div>
                <button class="btn-main" onclick="handleLogin()">دخول</button>
                <button class="btn-main btn-secondary" onclick="switchView('view-signup')">إنشاء حساب جديد</button>
                <div id="login-error" class="error-msg"></div>
            </div>
            <div id="view-signup" class="view-section">
                <h2>إنشاء حساب</h2>
                <div class="input-group"><input type="email" id="reg-email" class="app-input" placeholder="البريد الإلكتروني"></div>
                <div class="input-group"><input type="password" id="reg-pass" class="app-input" placeholder="كلمة المرور"></div>
                <div class="input-group"><input type="password" id="reg-confirm" class="app-input" placeholder="تأكيد كلمة المرور"></div>
                <button class="btn-main" onclick="handleSignup()">تسجيل</button>
                <button class="btn-main btn-secondary" onclick="switchView('view-login')">عودة</button>
                <div id="reg-error" class="error-msg"></div><div id="reg-success" class="success-msg"></div>
            </div>
        </div>
    </div>

    <!-- App -->
    <div id="app-container">
        <div class="header">
            <div class="header-info">
                <h2 id="header-title" class="app-main-title">نظام الحضور الذكي</h2>
                <div class="user-sub-title">مرحباً، <span id="u-name">...</span></div>
            </div>
            <div class="header-actions">
                <button id="btn-nfc-scan" class="action-btn" onclick="window.app.startNFCScan()" style="display:none; color:var(--primary);">📡</button>
                <button class="action-btn" onclick="window.app.openInbox()">🔔 <span id="msg-badge" class="badge-count">0</span></button>
                <button class="action-btn" onclick="window.app.toggleTheme()">🌓</button>
                <!-- Print Report Button -->
                <button class="action-btn" onclick="window.app.openPrintModal()" style="color:var(--work)">🖨️</button>
                <button class="action-btn" onclick="window.app.openSearchModal()">🔍</button>
                <button class="action-btn" id="btn-settings" onclick="window.app.openSettings()">⚙️</button>
                <button class="action-btn" onclick="handleLogout()" style="color:#ef4444; background:rgba(239,68,68,0.1);">↪️</button>
            </div>
        </div>

        <div class="stats-grid">
            <div class="stat-card" onclick="window.app.showDetails('net')"><h4>رصيد الساعات</h4><div class="val" id="st-net">0</div></div>
            <div class="stat-card" onclick="window.app.showDetails('sat')"><h4>رصيد السبت</h4><div class="val" id="st-sat">0</div></div>
            <div class="stat-card" onclick="window.app.showDetails('sunday')"><h4>تعويض الأحد</h4><div class="val" id="st-sunday">0</div></div>
            <div class="stat-card" onclick="window.app.showDetails('leave')"><h4>رصيد العطلة</h4><div class="val" id="st-leave">0</div></div>
            <div class="stat-card" onclick="window.app.showDetails('week')"><h4>الأسبوع</h4><div class="val" id="st-week">0</div></div>
            <div class="stat-card" onclick="window.app.showDetails('month')"><h4>الشهر</h4><div class="val" id="st-month">0</div></div>
            <div class="stat-card full-width" onclick="window.app.showDetails('year')"><h4>مجموع السنة</h4><div class="val" id="st-year">0</div></div>
        </div>

        <div class="calendar-box">
            <div class="cal-header">
                <button class="action-btn" onclick="window.app.navMonth(-1)">&#10094;</button>
                <div style="font-weight:bold; color:var(--text)" id="cal-title"></div>
                <button class="action-btn" onclick="window.app.navMonth(1)">&#10095;</button>
            </div>
            <div class="days-grid" id="cal-grid"></div>
            <div class="legend-container">
                <div class="legend-dot lg-work" onclick="window.app.showLegendToast('عمل')"></div>
                <div class="legend-dot lg-holiday" onclick="window.app.showLegendToast('عطلة')"></div>
                <div class="legend-dot lg-sick" onclick="window.app.showLegendToast('مرض')"></div>
                <div class="legend-dot lg-absent" onclick="window.app.showLegendToast('غياب')"></div>
                <div class="legend-dot lg-recup" onclick="window.app.showLegendToast('تعويض')"></div>
                <div class="legend-dot lg-eid" onclick="window.app.showLegendToast('عيد')"></div>
                <div class="legend-dot lg-nat" onclick="window.app.showLegendToast('عيد وطني')"></div>
            </div>
        </div>

        <!-- Chart Below Calendar -->
        <div class="chart-box">
            <canvas id="myChart"></canvas>
        </div>
    </div>

    <!-- Modals -->
    <div class="modal-overlay" id="confirmModal">
        <div class="modal-content">
            <h3 style="color:#f44336; margin-bottom:10px;">⚠️ تأكيد الحذف</h3>
            <p>هل أنت متأكد؟</p>
            <div class="modal-btns">
                <button class="btn-del" onclick="window.app.performDelete()">نعم</button>
                <button class="btn-save" style="background:var(--border); color:var(--text);" onclick="document.getElementById('confirmModal').style.display='none'">لا</button>
            </div>
        </div>
    </div>

    <!-- Print/Report Modal -->
    <div class="modal-overlay" id="printModal">
        <div class="modal-content">
            <h3 style="text-align:center;">طباعة تقرير (A4)</h3>
            <label class="form-label">نوع التقرير:</label>
            <select id="print-type" class="app-input" onchange="window.app.togglePrintInputs()">
                <option value="month">تقرير شهري (مفصل)</option>
                <option value="year">تقرير سنوي (ملخص)</option>
            </select>
            
            <div id="print-month-input" style="margin-top:10px;">
                <label class="form-label">اختر الشهر:</label>
                <input type="month" id="print-date" class="app-input" value="2026-01">
            </div>
             <div id="print-year-input" style="margin-top:10px; display:none;">
                <label class="form-label">اختر السنة:</label>
                <input type="number" id="print-year-val" class="app-input" value="2026">
            </div>

            <button class="btn-main" onclick="window.app.generatePrintReport()">عرض وطباعة</button>
            <button class="btn-close-modal" onclick="document.getElementById('printModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <div class="modal-overlay" id="nfcPopup">
        <div class="modal-content">
            <h3 style="color:var(--work);">📡 تم التعرف!</h3>
            <p>هل تريد تسجيل الحضور؟</p>
            <div class="modal-btns">
                <button class="btn-save" onclick="window.app.confirmNFCAttendance()">نعم</button>
                <button class="btn-del" onclick="document.getElementById('nfcPopup').style.display='none'">إلغاء</button>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="inboxModal">
        <div class="modal-content">
            <h3>الرسائل</h3>
            <div id="inbox-list" style="margin-top:15px; max-height:400px; overflow-y:auto;"></div>
            <button class="btn-close-modal" onclick="document.getElementById('inboxModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <div class="modal-overlay" id="dayModal">
        <div class="modal-content">
            <h3 id="modal-title" style="text-align:center; margin-bottom:20px;"></h3>
            <label class="form-label">نوع النشاط:</label>
            <select id="d-type" class="app-input" onchange="window.app.toggleFields()">
                <option value="work">✅ عمل عادي</option><option value="holiday">🏖️ عطلة سنوية</option><option value="eid">🎉 عيد / مناسبة</option>
                <option value="recup">🔄 استرجاع</option><option value="sick">💊 مرض</option><option value="absent">❌ غياب</option>
            </select>
            <div id="f-holiday" class="hidden" style="background:rgba(67, 97, 238, 0.1); padding:10px; border-radius:8px; margin-bottom:10px;"><label>عدد الأيام:</label><input type="number" id="d-count" class="app-input" value="1" min="1"></div>
            <div id="f-eid" class="hidden" style="background:rgba(156, 39, 176, 0.1); padding:10px; border-radius:8px; margin-bottom:10px;"><input type="text" id="d-eid-name" class="app-input" placeholder="اسم المناسبة"><select id="d-eid-status" class="app-input" onchange="window.app.toggleFields()"><option value="work">عملت</option><option value="rest">عطلة مدفوعة</option></select></div>
            <div id="f-recup" class="hidden"><label>تعويض عن:</label><select id="d-recup-target" class="app-input"></select></div>
            <div id="f-time">
                <label>التوقيت:</label><select id="d-preset" class="app-input" onchange="window.app.applyPreset()" style="margin-bottom:5px;"><option value="manual">-- اختيار توقيت --</option></select>
                <div style="display:flex; gap:10px;"><input type="time" id="d-start" class="app-input"><input type="time" id="d-end" class="app-input"></div>
            </div>
            <div style="margin-top:10px;"><label>ملاحظة:</label><textarea id="d-note" class="app-input" rows="2" placeholder="ملاحظات إضافية..."></textarea></div>
            <div class="modal-btns">
                <button class="btn-save" onclick="window.app.saveDay()">حفظ</button>
                <button class="btn-del" onclick="window.app.askDelete()">مسح</button>
            </div>
            <button class="btn-close-modal" onclick="document.getElementById('dayModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <div class="modal-overlay" id="searchModal">
        <div class="modal-content">
            <h3 id="search-title" style="text-align:center;">بحث / تفاصيل</h3>
            <div id="search-inputs">
                <!-- Search filters same as before -->
                 <label>فلترة البحث:</label>
                 <select id="search-month" class="app-input" onchange="window.app.performSearch()"><option value="">الأشهر</option><option value="1">يناير</option><option value="2">فبراير</option><option value="3">مارس</option><option value="4">أبريل</option><option value="5">مايو</option><option value="6">يونيو</option><option value="7">يوليو</option><option value="8">أغسطس</option><option value="9">سبتمبر</option><option value="10">أكتوبر</option><option value="11">نوفمبر</option><option value="12">ديسمبر</option></select>
            </div>
            <div id="search-results" style="max-height:400px; overflow-y:auto; margin-top:10px;"></div>
            <button class="btn-close-modal" onclick="document.getElementById('searchModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <div class="modal-overlay" id="settingsModal">
        <div class="modal-content">
            <h3 style="text-align:center;">الإعدادات</h3>
            <div id="admin-section" style="display:none; margin-bottom:15px;">
                <div style="background:rgba(67, 97, 238, 0.1); padding:10px; border-radius:10px; margin-bottom:10px;">
                     <label class="form-label" style="color:var(--primary); font-weight:bold;">اسم البرنامج:</label><input type="text" id="p-app-name" class="app-input">
                </div>
                <div style="background:rgba(67, 97, 238, 0.1); padding:10px; border-radius:10px;">
                    <label class="form-label" style="color:var(--primary);">التوقيتات:</label>
                    <div style="display:flex; gap:5px;"><input type="text" id="p-name" class="app-input" placeholder="اسم"><input type="time" id="p-start" class="app-input"><input type="time" id="p-end" class="app-input"></div>
                    <button class="btn-main" onclick="window.app.addPreset()" style="font-size:0.8rem; padding:8px;">+ إضافة</button>
                    <div id="presets-list" class="preset-list" style="margin-top:10px; max-height:100px; overflow-y:auto;"></div>
                </div>
            </div>
            <!-- NFC Settings -->
            <div style="background:var(--bg); padding:15px; border-radius:10px; margin-bottom:15px; border:1px solid var(--border);">
                <div class="switch-container">
                    <label style="font-weight:bold; color:var(--text)">NFC Tools</label>
                    <label class="switch"><input type="checkbox" id="s-nfc-toggle" onchange="window.app.toggleNFCConfig()"><span class="slider"></span></label>
                </div>
                <div id="nfc-config-area" class="hidden">
                    <div style="display:flex; gap:8px;"><input type="text" id="s-nfc-serial" class="app-input" placeholder="Serial Number"><button onclick="window.app.scanTagForSetup()" style="white-space:nowrap;">مسح</button></div>
                </div>
            </div>
            <div style="background:rgba(76, 175, 80, 0.1); padding:10px; border-radius:10px; margin-bottom:15px;">
                <label>الاسم الكامل:</label><input type="text" id="s-name" class="app-input">
                <label>تاريخ الالتحاق:</label><input type="date" id="s-join" class="app-input">
            </div>
            <div style="background:rgba(255, 152, 0, 0.1); padding:10px; border-radius:10px; margin-bottom:15px;">
                <label>رصيد عطلة إضافي:</label>
                <div style="display:flex; gap:5px;"><input type="number" id="adj-days" class="app-input" placeholder="أيام"><input type="text" id="adj-note" class="app-input" placeholder="سبب"></div>
                <button class="btn-main" onclick="window.app.addAdj()" style="background:#ff9800;">+ إضافة</button>
                <div id="adj-list" style="margin-top:10px;"></div>
            </div>
            <div class="modal-btns"><button class="btn-save" onclick="window.app.saveSettings()">حفظ الكل</button></div>
            <button class="btn-close-modal" onclick="document.getElementById('settingsModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, onSnapshot, updateDoc, deleteField, addDoc, serverTimestamp, query, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut, onAuthStateChanged, sendPasswordResetEmail, sendEmailVerification, setPersistence, browserLocalPersistence, browserSessionPersistence } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";

        const firebaseConfig = { apiKey: "AIzaSyDhpORuBt8k6YWDLUgRrnqfC8lSS97LexQ", authDomain: "sbota-37391.firebaseapp.com", projectId: "sbota-37391", storageBucket: "sbota-37391.firebasestorage.app", messagingSenderId: "1049902061223", appId: "1:1049902061223:web:68e7c10c349025ca7ead82", measurementId: "G-3B4ESSJWJ9" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app); const auth = getAuth(app);

        window.showLoader = (s) => document.getElementById('loader').style.display = s?'flex':'none';
        window.showError = (id, msg) => { const el=document.getElementById(id); el.textContent=msg; el.style.display='block'; };
        window.switchView = (id) => { document.querySelectorAll('.view-section').forEach(e=>e.classList.remove('active')); document.getElementById(id).classList.add('active'); document.querySelectorAll('.error-msg,.success-msg').forEach(e=>e.style.display='none'); };
        window.togglePass = (id) => { const el=document.getElementById(id); el.type = el.type==='password'?'text':'password'; };

        window.handleLogin = async () => { const e = document.getElementById('login-email').value, p = document.getElementById('login-pass').value; if(!e || !p) return window.showError('login-error', 'بيانات ناقصة'); window.showLoader(true); try { await setPersistence(auth, document.getElementById('remember-me').checked ? browserLocalPersistence : browserSessionPersistence); const cred = await signInWithEmailAndPassword(auth, e, p); if(!cred.user.emailVerified) { await signOut(auth); window.showError('login-error', 'يرجى تفعيل البريد'); window.showLoader(false); } } catch(e) { window.showLoader(false); window.showError('login-error', "بيانات خاطئة"); } };
        window.handleSignup = async () => { const e = document.getElementById('reg-email').value, p = document.getElementById('reg-pass').value, c = document.getElementById('reg-confirm').value; if(!e||!p||!c||p!==c||p.length<6) return window.showError('reg-error', 'خطأ في البيانات'); window.showLoader(true); try { const snap = await getDocs(collection(db, "users")); const role = snap.empty ? 'admin' : 'user'; const cred = await createUserWithEmailAndPassword(auth, e, p); await sendEmailVerification(cred.user); await setDoc(doc(db, "users", cred.user.uid), { email: e, role: role }); await setDoc(doc(db, "settings", cred.user.uid), { joinDate: '', fullName: '', adjustments: [], dismissedMsgs: [], deletedMsgs: [], nfc: {enabled:false, serial:''} }); if(role === 'admin') await setDoc(doc(db, "config", "general"), { presets: [{label:'عادي', start:'08:00', end:'16:00'}] }); await signOut(auth); document.getElementById('reg-success').textContent = "تم التسجيل!"; document.getElementById('reg-success').style.display = 'block'; } catch(err) { window.showError('reg-error', 'خطأ في التسجيل'); } finally { window.showLoader(false); } };
        window.handleReset = async () => { const e = document.getElementById('reset-email').value; if(e) try { await sendPasswordResetEmail(auth, e); } catch(err) {} };
        window.handleLogout = async () => { await signOut(auth); window.location.reload(); };

        window.saveData = async (type, data) => { const u = auth.currentUser; if(!u) return; try { if(type === 'personal_settings') await setDoc(doc(db, 'settings', u.uid), data, {merge:true}); else if(type === 'global_config') await setDoc(doc(db, 'config', 'general'), data, {merge:true}); else if(type === 'events') await setDoc(doc(db, 'attendance', u.uid), {events: data}, {merge:true}); } catch(e) {} };
        window.fbDeleteDay = async (dateKey) => { const u = auth.currentUser; if(!u) return; try { await updateDoc(doc(db, 'attendance', u.uid), { [`events.${dateKey}`]: deleteField() }); } catch(e) {} };
        
        onAuthStateChanged(auth, async (user) => {
            if(user && user.emailVerified) {
                document.getElementById('auth-overlay').style.display = 'none'; document.getElementById('app-container').style.display = 'block';
                document.getElementById('u-name').textContent = user.email.split('@')[0]; window.app.initTheme(); window.showLoader(true);
                const uDoc = await getDoc(doc(db, 'users', user.uid));
                if(uDoc.exists()) { window.appData.role = uDoc.data().role; if(window.appData.role === 'admin') document.getElementById('admin-section').style.display = 'block'; }
                onSnapshot(doc(db, "attendance", user.uid), (doc) => { if(doc.exists()) window.appData.events = doc.data().events || {}; window.app.renderCalendar(); window.app.checkAutoFill(); });
                onSnapshot(doc(db, "settings", user.uid), (doc) => { 
                    if(doc.exists()) window.appData.personal = doc.data() || {};
                    // Defaults
                    if(!window.appData.personal.nfc) window.appData.personal.nfc = {enabled: false, serial: ''};
                    if(!window.appData.personal.dismissedMsgs) window.appData.personal.dismissedMsgs = [];
                    if(!window.appData.personal.deletedMsgs) window.appData.personal.deletedMsgs = [];
                    document.getElementById('u-name').textContent = window.appData.personal.fullName || user.email.split('@')[0];
                    document.getElementById('btn-nfc-scan').style.display = window.appData.personal.nfc.enabled ? 'flex' : 'none';
                    window.app.calcStats();
                });
                onSnapshot(doc(db, "config", "general"), (doc) => { if(doc.exists()) { window.appData.global = doc.data() || {}; if(window.appData.global.appName) { document.title = window.appData.global.appName; document.getElementById('header-title').textContent = window.appData.global.appName; } } });
                onSnapshot(query(collection(db, "notifications"), orderBy("createdAt", "desc")), (s) => { let msgs = []; s.forEach((doc) => msgs.push({ id: doc.id, ...doc.data() })); window.appData.messages = msgs; });
                window.showLoader(false);
            } else { if(user) await signOut(auth); document.getElementById('auth-overlay').style.display = 'flex'; document.getElementById('app-container').style.display = 'none'; window.showLoader(false); }
        });
    </script>

    <script>
        const nationalHolidays = { "1-11":"وثيقة الاستقلال","1-14":"رأس السنة الأمازيغية","5-1":"عيد الشغل","7-30":"عيد العرش","8-14":"وادي الذهب","8-20":"ثورة الملك والشعب","8-21":"عيد الشباب","10-31":"عيد الوحدة","11-6":"المسيرة الخضراء","11-18":"عيد الاستقلال","12-9":"عيد الوساطة" };
        const dayNames = ["إثنين", "ثلاثاء", "أربعاء", "خميس", "جمعة", "سبت", "أحد"];
        const monthNames = ["يناير", "فبراير", "مارس", "أبريل", "مايو", "يونيو", "يوليو", "أغسطس", "سبتمبر", "أكتوبر", "نوفمبر", "ديسمبر"];
        let currentDate = new Date(2026, 0, 1);
        let selectedKey = null, deleteType = null, nfcReader = null;
        window.myChartInstance = null;
        window.appData = { role: 'user', events: {}, personal: {}, global: {}, messages: [] };

        window.app = {
            initTheme: () => { if(localStorage.getItem('theme') === 'dark') document.body.classList.add('dark-mode'); },
            toggleTheme: () => { document.body.classList.toggle('dark-mode'); localStorage.setItem('theme', document.body.classList.contains('dark-mode') ? 'dark' : 'light'); window.app.renderChart(); },

            // --- Printing System ---
            openPrintModal: () => { document.getElementById('printModal').style.display = 'flex'; },
            togglePrintInputs: () => {
                const type = document.getElementById('print-type').value;
                document.getElementById('print-month-input').style.display = type === 'month' ? 'block' : 'none';
                document.getElementById('print-year-input').style.display = type === 'year' ? 'block' : 'none';
            },
            generatePrintReport: () => {
                const type = document.getElementById('print-type').value;
                const printArea = document.getElementById('print-area');
                const userTitle = window.appData.personal.fullName || "تقرير الموظف";
                let html = `<div class="print-header"><h2>${window.appData.global.appName || "نظام الحضور"}</h2><h3>${userTitle}</h3>`;
                
                if (type === 'month') {
                    const monthInput = document.getElementById('print-date').value; // YYYY-MM
                    const [y, m] = monthInput.split('-').map(Number);
                    const daysInMonth = new Date(y, m, 0).getDate();
                    html += `<h4>تقرير شهر: ${monthNames[m-1]} ${y}</h4></div>`;
                    html += `<table class="print-table"><thead><tr><th>اليوم</th><th>التاريخ</th><th>الحالة</th><th>التوقيت</th><th>ملاحظات</th></tr></thead><tbody>`;
                    
                    for(let i=1; i<=daysInMonth; i++) {
                        const k = `${y}-${String(m).padStart(2,'0')}-${String(i).padStart(2,'0')}`;
                        const dateObj = new Date(y, m-1, i);
                        const evt = window.appData.events[k];
                        const dayName = dayNames[(dateObj.getDay()===0)?6:dateObj.getDay()-1];
                        let status = '', time = '-', note = '';
                        
                        if (evt) {
                            status = {work:'عمل', holiday:'عطلة', sick:'مرض', absent:'غياب', recup:'تعويض', eid:'عيد'}[evt.type] || evt.type;
                            if (evt.start && evt.end) time = `${evt.start} - ${evt.end} (${evt.hours}س)`;
                            else if(evt.hours) time = `${evt.hours}س`;
                            note = evt.note || '';
                        } else if (dateObj.getDay() === 0 || dateObj.getDay() === 6) {
                            status = 'عطلة أسبوعية';
                        }

                        html += `<tr><td>${dayName}</td><td>${k}</td><td>${status}</td><td>${time}</td><td>${note}</td></tr>`;
                    }
                    html += `</tbody></table>`;
                    
                } else if (type === 'year') {
                    const year = parseInt(document.getElementById('print-year-val').value);
                    html += `<h4>التقرير السنوي: ${year}</h4></div>`;
                    html += `<table class="print-table"><thead><tr><th>الشهر</th><th>أيام العمل</th><th>ساعات العمل</th><th>عطلة سنوية</th><th>مرض/غياب</th></tr></thead><tbody>`;
                    
                    for(let m=0; m<12; m++) {
                        let workDays=0, workHours=0, holDays=0, sickDays=0;
                        for(let d=1; d<=31; d++) {
                            const k = `${year}-${String(m+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
                            const evt = window.appData.events[k];
                            if(evt) {
                                if(evt.type==='work' || (evt.type==='eid' && evt.eidStatus==='work')) { workDays++; workHours+=evt.hours; }
                                else if(evt.type==='holiday') holDays++;
                                else if(evt.type==='sick' || evt.type==='absent') sickDays++;
                            }
                        }
                        html += `<tr><td>${monthNames[m]}</td><td>${workDays}</td><td>${workHours.toFixed(1)}</td><td>${holDays}</td><td>${sickDays}</td></tr>`;
                    }
                    html += `</tbody></table>`;
                }

                printArea.innerHTML = html;
                document.getElementById('printModal').style.display = 'none';
                document.body.classList.add('show-print');
                window.print();
                document.body.classList.remove('show-print');
            },

            // --- Details Logic (Balances) ---
            getLeaveBreakdown: () => {
                const joinD = window.appData.personal.joinDate ? new Date(window.appData.personal.joinDate) : null;
                let pools = [];
                // 1. Adjustments
                if(window.appData.personal.adjustments) {
                    window.appData.personal.adjustments.forEach((adj, i) => {
                        pools.push({ id: `adj_${i}`, label: `رصيد سابق/إضافي (${adj.reason})`, total: parseFloat(adj.amount), remaining: parseFloat(adj.amount) });
                    });
                }
                // 2. Yearly Balance
                if(joinD) {
                    const startCalc = Math.max(joinD.getFullYear(), 2026);
                    for(let y = startCalc; y <= 2026; y++) {
                        let months = 12;
                        if(y === joinD.getFullYear()) months = 12 - joinD.getMonth();
                        let seniority = Math.floor((y - joinD.getFullYear())/5) * 1.5;
                        let amount = Math.min((months * 1.5) + seniority, 30);
                        if(amount > 0) pools.push({ id: y, label: `رصيد سنة ${y}`, total: amount, remaining: amount });
                    }
                } else {
                     // Fallback if no date
                     pools.push({id: 'manual', label: 'رصيد افتراضي (بدون تاريخ التحاق)', total: 0, remaining: 0});
                }

                const holidays = Object.entries(window.appData.events)
                    .filter(([k, v]) => v.type === 'holiday')
                    .sort((a, b) => new Date(a[0]) - new Date(b[0]));

                let deductions = [];
                holidays.forEach(h => {
                    let consumed = false;
                    for(let pool of pools) {
                        if(pool.remaining > 0) {
                            pool.remaining--;
                            deductions.push({ date: h[0], note: `خصم من: ${pool.label}`, type: 'neg' });
                            consumed = true;
                            break;
                        }
                    }
                    if(!consumed) deductions.push({ date: h[0], note: 'رصيد غير كافٍ', type: 'neg' });
                });
                return { pools, deductions };
            },

            showDetails: (cat) => {
                document.getElementById('search-inputs').style.display = 'none';
                document.getElementById('search-title').textContent = 'التفاصيل';
                const list = document.getElementById('search-results'); list.innerHTML = '';
                
                if (cat === 'leave') {
                    const bd = window.app.getLeaveBreakdown();
                    list.innerHTML += `<div class="details-header">الأرصدة المتاحة:</div>`;
                    bd.pools.forEach(p => {
                         // Only show if it has value or was manually added
                        if(p.total > 0) list.innerHTML += `<div class="detail-item pos"><span>${p.label}</span><span class="d-val">${p.remaining.toFixed(1)} / ${p.total.toFixed(1)}</span></div>`;
                    });
                    
                    list.innerHTML += `<div class="details-header">سجل العطل:</div>`;
                    if(bd.deductions.length === 0) list.innerHTML += `<div style="text-align:center;padding:10px;">لا يوجد</div>`;
                    bd.deductions.reverse().forEach(d => {
                        list.innerHTML += `<div class="detail-item neg"><span>${d.date}</span><span>${d.note}</span></div>`;
                    });

                } else {
                    // Other categories logic (Simplified for length)
                    const yr = currentDate.getFullYear();
                    for (const [k, evt] of Object.entries(window.appData.events)) {
                        if(new Date(k).getFullYear() !== yr) continue;
                        let add = false, val='', type='neutral', txt='';
                        if(cat==='net' && (evt.type==='work'||evt.type==='absent')) { 
                            let diff = (evt.hours||0)-8; if(evt.type==='absent') diff=-8;
                            if(diff!==0) { add=true; val=(diff>0?'+':'')+diff; type=diff>0?'pos':'neg'; txt=evt.type; }
                        }
                        if(add) list.innerHTML += `<div class="detail-item ${type}" onclick="window.app.openDay('${k}')"><span>${k} (${txt})</span><span class="d-val ${type}">${val}</span></div>`;
                    }
                }
                document.getElementById('searchModal').style.display = 'flex';
            },

            // --- NFC ---
            toggleNFCConfig: () => { const en = document.getElementById('s-nfc-toggle').checked; document.getElementById('nfc-config-area').classList.toggle('hidden', !en); },
            scanTagForSetup: async () => {
                try {
                    const ndef = new NDEFReader(); await ndef.scan();
                    window.app.showLegendToast("قرّب الشريحة...");
                    ndef.onreading = e => { document.getElementById('s-nfc-serial').value = e.serialNumber; window.app.showLegendToast("تم القراءة!"); };
                } catch(e) { alert("NFC غير مدعوم: "+e); }
            },
            startNFCScan: async () => {
                try {
                    const ndef = new NDEFReader(); await ndef.scan();
                    window.app.showLegendToast("قرّب الشريحة...");
                    document.getElementById('btn-nfc-scan').classList.add('nfc-scanning');
                    ndef.onreading = e => {
                        if(e.serialNumber === window.appData.personal.nfc.serial) {
                            document.getElementById('btn-nfc-scan').classList.remove('nfc-scanning');
                            document.getElementById('nfcPopup').style.display = 'flex';
                        }
                    };
                } catch(e) { alert("خطأ NFC"); }
            },
            confirmNFCAttendance: () => {
                 const k = new Date().toISOString().slice(0,10);
                 if(window.appData.events[k]) { alert("مسجل مسبقاً"); return; }
                 let s='08:00', e='16:00';
                 if(window.appData.global.presets && window.appData.global.presets.length > 0) { s=window.appData.global.presets[0].start; e=window.appData.global.presets[0].end; }
                 const [h1,m1]=s.split(':').map(Number), [h2,m2]=e.split(':').map(Number);
                 let diff = (h2*60+m2)-(h1*60+m1); if(diff<0) diff+=24*60;
                 window.appData.events[k] = {type:'work', start:s, end:e, hours:parseFloat((diff/60).toFixed(2)), note:'NFC'};
                 window.saveData('events', window.appData.events);
                 document.getElementById('nfcPopup').style.display='none'; window.app.renderCalendar();
            },

            // --- Core ---
            calcStats: () => {
                // ... (Logic from previous versions preserved) ...
                let net=0, sat=0, leave=0, pending=0, tWeek=0, tMonth=0, tYear=0, yr=currentDate.getFullYear(), mth=currentDate.getMonth(), today=new Date();
                const weekStart=new Date(today); weekStart.setDate(today.getDate()-today.getDay()); weekStart.setHours(0,0,0,0);
                const weekEnd=new Date(weekStart); weekEnd.setDate(weekStart.getDate()+6); weekEnd.setHours(23,59,59,999);
                
                const bd = window.app.getLeaveBreakdown();
                leave = bd.pools.reduce((s, p) => s + p.remaining, 0);

                Object.entries(window.appData.events).forEach(([k, evt]) => {
                     const d = new Date(k);
                     if(d.getFullYear()===yr) {
                         let h=0; if(evt.type==='work'||(evt.type==='eid'&&evt.eidStatus==='work')) h=evt.hours; else if(['holiday','sick'].includes(evt.type)) h=8;
                         tYear+=h; if(d.getMonth()===mth) tMonth+=h; if(d>=weekStart&&d<=weekEnd) tWeek+=h;
                         if(evt.type==='work'||(evt.type==='eid'&&evt.eidStatus==='work')) net+=(evt.hours-8); else if(evt.type==='absent') net-=8;
                         if(d.getDay()===6) { if(evt.type==='work'||(evt.type==='eid'&&evt.eidStatus==='work')) sat+=4; else sat-=4; }
                     }
                });
                
                // Fix Saturday calc for the whole year days
                const startLoop = new Date(yr, 0, 1);
                const limitLoop = (yr === today.getFullYear()) ? today : new Date(yr, 11, 31);
                for (let d = new Date(startLoop); d <= limitLoop; d.setDate(d.getDate() + 1)) {
                    if(d.getDay() === 6) {
                        const k = `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
                        if(!window.appData.events[k]) sat-=4; // Deduct if no event on past Saturday
                    }
                }

                document.getElementById('st-net').innerHTML = `<span class="${net>=0?'txt-green':'txt-red'}">${net.toFixed(1)}</span>`;
                document.getElementById('st-sat').innerHTML = `<span class="${sat>=0?'txt-green':'txt-red'}">${sat}</span>`;
                document.getElementById('st-leave').textContent = leave.toFixed(1); 
                document.getElementById('st-week').textContent = tWeek.toFixed(1); document.getElementById('st-month').textContent = tMonth.toFixed(1); document.getElementById('st-year').textContent = tYear.toFixed(1);
                window.app.renderChart();
            },
            renderChart: () => {
                const ctx = document.getElementById('myChart'); if(!ctx) return;
                let counts = { work:0, holiday:0, sick:0, absent:0, eid:0 };
                Object.values(window.appData.events).forEach(e => { if(counts[e.type]!==undefined) counts[e.type]++; });
                const isDark = document.body.classList.contains('dark-mode');
                if(window.myChartInstance) window.myChartInstance.destroy();
                window.myChartInstance = new Chart(ctx, {
                    type: 'bar', // Changed to Bar for better layout below calendar
                    data: { labels: ['عمل', 'عطلة', 'مرض', 'غياب'], datasets: [{ label:'الأيام', data: [counts.work, counts.holiday, counts.sick, counts.absent], backgroundColor: ['#4caf50', '#ffc107', '#ff9800', '#f44336'] }] },
                    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { y: { beginAtZero: true, ticks: { color: isDark?'#ccc':'#333' } }, x: { ticks: { color: isDark?'#ccc':'#333', font: {family:'Cairo'} } } } }
                });
            },
            renderSettingsLists: () => {
                const pl = document.getElementById('presets-list'); pl
