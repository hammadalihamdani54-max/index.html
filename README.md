<!DOCTYPE html>
<html lang="ur">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>Marble Shop Management</title>
    <!-- Font Awesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        body {
            background: #f0f4f8;
            direction: rtl;
            padding: 10px;
            padding-bottom: 80px;
        }
        .app-container {
            max-width: 500px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            overflow: hidden;
            padding: 15px;
        }
        /* Header */
        .header {
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: white;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 15px;
            text-align: center;
        }
        .header h1 {
            font-size: 24px;
            font-weight: 700;
        }
        .header p {
            font-size: 14px;
            opacity: 0.9;
            margin-top: 5px;
        }
        /* Cards */
        .card {
            background: white;
            border-radius: 15px;
            padding: 15px;
            margin-bottom: 15px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            border: 1px solid #e9ecef;
        }
        .card-title {
            font-size: 18px;
            font-weight: 600;
            color: #1e3c72;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        /* Form */
        .form-group {
            margin-bottom: 12px;
        }
        .form-group label {
            display: block;
            font-size: 14px;
            font-weight: 600;
            color: #495057;
            margin-bottom: 4px;
        }
        .form-group input, .form-group select {
            width: 100%;
            padding: 10px 12px;
            border: 2px solid #dee2e6;
            border-radius: 10px;
            font-size: 14px;
            transition: 0.3s;
            background: #f8f9fa;
        }
        .form-group input:focus, .form-group select:focus {
            border-color: #2a5298;
            outline: none;
            background: white;
        }
        .btn {
            padding: 10px 18px;
            border: none;
            border-radius: 10px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            width: 100%;
            justify-content: center;
        }
        .btn-primary {
            background: #2a5298;
            color: white;
        }
        .btn-primary:active {
            transform: scale(0.96);
        }
        .btn-success {
            background: #28a745;
            color: white;
        }
        .btn-danger {
            background: #dc3545;
            color: white;
        }
        .btn-warning {
            background: #ffc107;
            color: #212529;
        }
        .btn-outline {
            background: transparent;
            border: 2px solid #2a5298;
            color: #2a5298;
        }
        /* Grid */
        .grid-2 {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }
        /* Table */
        .table-wrap {
            overflow-x: auto;
            margin-top: 10px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 13px;
        }
        th {
            background: #1e3c72;
            color: white;
            padding: 8px 6px;
            text-align: center;
        }
        td {
            padding: 8px 6px;
            border-bottom: 1px solid #dee2e6;
            text-align: center;
        }
        tr:nth-child(even) {
            background: #f8f9fa;
        }
        .badge {
            display: inline-block;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 600;
        }
        .badge-danger {
            background: #f8d7da;
            color: #721c24;
        }
        .badge-success {
            background: #d4edda;
            color: #155724;
        }
        .badge-warning {
            background: #fff3cd;
            color: #856404;
        }
        /* Tabs */
        .tabs {
            display: flex;
            gap: 5px;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        .tab {
            flex: 1;
            padding: 10px;
            text-align: center;
            background: #e9ecef;
            border-radius: 10px;
            font-weight: 600;
            font-size: 13px;
            cursor: pointer;
            transition: 0.3s;
            min-width: 60px;
        }
        .tab.active {
            background: #2a5298;
            color: white;
        }
        .tab i {
            display: block;
            font-size: 20px;
            margin-bottom: 3px;
        }
        .section {
            display: none;
        }
        .section.active {
            display: block;
        }
        /* Stock warning */
        .stock-warning {
            background: #fff3cd;
            border-right: 4px solid #ffc107;
            padding: 10px;
            border-radius: 8px;
            margin: 8px 0;
            font-size: 13px;
        }
        /* Mobile bottom nav */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            display: flex;
            justify-content: space-around;
            padding: 8px 0;
            box-shadow: 0 -2px 15px rgba(0,0,0,0.1);
            border-top: 1px solid #dee2e6;
            z-index: 1000;
            max-width: 500px;
            margin: 0 auto;
        }
        .bottom-nav .nav-item {
            text-align: center;
            font-size: 11px;
            color: #6c757d;
            cursor: pointer;
            padding: 4px 10px;
            border-radius: 10px;
            transition: 0.3s;
        }
        .bottom-nav .nav-item i {
            font-size: 22px;
            display: block;
        }
        .bottom-nav .nav-item.active {
            color: #2a5298;
            font-weight: 600;
        }
        /* Responsive */
        @media (max-width: 480px) {
            .grid-2 {
                grid-template-columns: 1fr;
            }
            .header h1 {
                font-size: 20px;
            }
        }
        /* Scrollable list */
        .list-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid #eee;
            font-size: 14px;
        }
        .list-item .left {
            font-weight: 500;
        }
        .list-item .right {
            color: #2a5298;
            font-weight: 600;
        }
    </style>
</head>
<body>
    <div class="app-container" id="app">
        <!-- Header -->
        <div class="header">
            <h1><i class="fas fa-store"></i> ماربل شاپ</h1>
            <p>مکمل انتظامی نظام <span id="dateDisplay"></span></p>
        </div>

        <!-- Tabs -->
        <div class="tabs" id="tabHeaders">
            <div class="tab active" data-tab="dashboard"><i class="fas fa-home"></i> ڈیش بورڈ</div>
            <div class="tab" data-tab="items"><i class="fas fa-boxes"></i> آئٹمز</div>
            <div class="tab" data-tab="sales"><i class="fas fa-shopping-cart"></i> فروخت</div>
            <div class="tab" data-tab="reports"><i class="fas fa-chart-line"></i> رپورٹ</div>
        </div>

        <!-- ========== SECTION: DASHBOARD ========== -->
        <div id="section-dashboard" class="section active">
            <div class="card">
                <div class="card-title"><i class="fas fa-tachometer-alt"></i> فوری جائزہ</div>
                <div class="grid-2">
                    <div style="background:#e3f2fd;padding:15px;border-radius:12px;text-align:center;">
                        <div style="font-size:24px;font-weight:700;color:#0d47a1;" id="dashTotalItems">0</div>
                        <div style="font-size:13px;color:#555;">کل آئٹمز</div>
                    </div>
                    <div style="background:#e8f5e9;padding:15px;border-radius:12px;text-align:center;">
                        <div style="font-size:24px;font-weight:700;color:#1b5e20;" id="dashTotalSales">0</div>
                        <div style="font-size:13px;color:#555;">آج کی فروخت</div>
                    </div>
                </div>
                <div id="stockAlertContainer" style="margin-top:10px;"></div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-users"></i> گاہک</div>
                <div class="form-group">
                    <input type="text" id="customerName" placeholder="گاہک کا نام">
                </div>
                <div class="form-group">
                    <input type="text" id="customerPhone" placeholder="فون نمبر">
                </div>
                <button class="btn btn-primary" onclick="addCustomer()"><i class="fas fa-user-plus"></i> گاہک شامل کریں</button>
                <div id="customerList" style="margin-top:10px;"></div>
            </div>
        </div>

        <!-- ========== SECTION: ITEMS ========== -->
        <div id="section-items" class="section">
            <div class="card">
                <div class="card-title"><i class="fas fa-plus-circle"></i> نیا آئٹم</div>
                <div class="form-group">
                    <input type="text" id="itemName" placeholder="آئٹم کا نام (مثلاً اٹلانٹک ماربل)">
                </div>
                <div class="form-group">
                    <select id="itemUnit">
                        <option value="m²">m²</option>
                        <option value="pcs">pcs (ٹکڑے)</option>
                    </select>
                </div>
                <div class="grid-2">
                    <div class="form-group">
                        <input type="number" id="itemStock" placeholder="اسٹاک">
                    </div>
                    <div class="form-group">
                        <input type="number" id="itemMinStock" placeholder="کم از کم وارننگ">
                    </div>
                </div>
                <div class="grid-2">
                    <div class="form-group">
                        <input type="number" id="itemPurchasePrice" placeholder="خرید قیمت">
                    </div>
                    <div class="form-group">
                        <input type="number" id="itemSellPrice" placeholder="فروخت قیمت">
                    </div>
                </div>
                <button class="btn btn-success" onclick="addItem()"><i class="fas fa-save"></i> آئٹم محفوظ کریں</button>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-list"></i> موجودہ اسٹاک</div>
                <div id="itemListContainer"></div>
            </div>
        </div>

        <!-- ========== SECTION: SALES ========== -->
        <div id="section-sales" class="section">
            <div class="card">
                <div class="card-title"><i class="fas fa-cash-register"></i> نیا بل</div>
                <div class="form-group">
                    <select id="saleItemSelect"></select>
                </div>
                <div class="form-group">
                    <input type="number" id="saleQty" placeholder="مقدار">
                </div>
                <div class="form-group">
                    <input type="number" id="salePaid" placeholder="ادا کردہ رقم">
                </div>
                <div class="form-group">
                    <select id="saleCustomerSelect">
                        <option value="">کیش (بغیر گاہک)</option>
                    </select>
                </div>
                <button class="btn btn-primary" onclick="addSale()"><i class="fas fa-check"></i> فروخت کریں</button>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-history"></i> آج کی فروخت</div>
                <div id="todaySalesList"></div>
            </div>
        </div>

        <!-- ========== SECTION: REPORTS ========== -->
        <div id="section-reports" class="section">
            <div class="card">
                <div class="card-title"><i class="fas fa-calendar-day"></i> روزانہ رپورٹ</div>
                <button class="btn btn-primary" onclick="generateReport('daily')"><i class="fas fa-eye"></i> دیکھیں</button>
                <button class="btn btn-success" onclick="sendWhatsApp()" style="margin-top:6px;"><i class="fab fa-whatsapp"></i> واٹس ایپ بھیجیں</button>
                <div id="reportDisplay" style="margin-top:12px;background:#f8f9fa;padding:12px;border-radius:10px;font-size:14px;white-space:pre-wrap;"></div>
            </div>
            <div class="card">
                <div class="card-title"><i class="fas fa-file-invoice"></i> تمام لین دین</div>
                <div id="allTransactions"></div>
            </div>
        </div>
    </div>

    <!-- Bottom Navigation -->
    <div class="bottom-nav">
        <div class="nav-item active" data-tab="dashboard"><i class="fas fa-home"></i> ڈیش</div>
        <div class="nav-item" data-tab="items"><i class="fas fa-boxes"></i> آئٹمز</div>
        <div class="nav-item" data-tab="sales"><i class="fas fa-shopping-cart"></i> فروخت</div>
        <div class="nav-item" data-tab="reports"><i class="fas fa-chart-line"></i> رپورٹ</div>
    </div>

    <script>
        // =======================================================
        // 1. ڈیٹا بیس (localStorage)
        // =======================================================
        let db = {
            items: JSON.parse(localStorage.getItem('marble_items')) || [],
            customers: JSON.parse(localStorage.getItem('marble_customers')) || [],
            sales: JSON.parse(localStorage.getItem('marble_sales')) || [],
            transactions: JSON.parse(localStorage.getItem('marble_transactions')) || []
        };

        function saveDB() {
            localStorage.setItem('marble_items', JSON.stringify(db.items));
            localStorage.setItem('marble_customers', JSON.stringify(db.customers));
            localStorage.setItem('marble_sales', JSON.stringify(db.sales));
            localStorage.setItem('marble_transactions', JSON.stringify(db.transactions));
        }

        // =======================================================
        // 2. تاریخ
        // =======================================================
        document.getElementById('dateDisplay').textContent = new Date().toLocaleDateString('ur-PK', { weekday:'long', year:'numeric', month:'long', day:'numeric' });

        // =======================================================
        // 3. آئٹمز
        // =======================================================
        function addItem() {
            const name = document.getElementById('itemName').value.trim();
            const unit = document.getElementById('itemUnit').value;
            const stock = parseFloat(document.getElementById('itemStock').value) || 0;
            const minStock = parseFloat(document.getElementById('itemMinStock').value) || 0;
            const pp = parseFloat(document.getElementById('itemPurchasePrice').value) || 0;
            const sp = parseFloat(document.getElementById('itemSellPrice').value) || 0;
            if (!name) return alert('براہ کرم آئٹم کا نام درج کریں');
            if (db.items.find(i => i.name === name)) return alert('یہ آئٹم پہلے سے موجود ہے');
            db.items.push({ id: Date.now(), name, unit, stock, minStock, pp, sp });
            saveDB();
            renderItems();
            clearItemForm();
            updateDashboard();
            alert('✅ آئٹم شامل ہو گیا');
        }

        function clearItemForm() {
            document.getElementById('itemName').value = '';
            document.getElementById('itemStock').value = '';
            document.getElementById('itemMinStock').value = '';
            document.getElementById('itemPurchasePrice').value = '';
            document.getElementById('itemSellPrice').value = '';
        }

        function renderItems() {
            const container = document.getElementById('itemListContainer');
            if (db.items.length === 0) {
                container.innerHTML = '<div class="list-item">کوئی آئٹم نہیں</div>';
                return;
            }
            let html = `<div class="table-wrap"><table>
                <tr><th>نام</th><th>اسٹاک</th><th>یونٹ</th><th>منٹ</th><th>فروخت</th></tr>`;
            db.items.forEach(item => {
                const warn = item.stock < item.minStock ? '⚠️' : '';
                html += `<tr>
                    <td>${item.name}</td>
                    <td><strong>${item.stock}</strong> ${warn}</td>
                    <td>${item.unit}</td>
                    <td>${item.minStock}</td>
                    <td>${item.sp}</td>
                </tr>`;
            });
            html += '</table></div>';
            container.innerHTML = html;
            // Populate sale dropdown
            const select = document.getElementById('saleItemSelect');
            select.innerHTML = '';
            db.items.forEach(item => {
                const opt = document.createElement('option');
                opt.value = item.id;
                opt.textContent = `${item.name} (اسٹاک: ${item.stock} ${item.unit})`;
                select.appendChild(opt);
            });
            // Stock warning
            updateStockWarnings();
        }

        function updateStockWarnings() {
            const container = document.getElementById('stockAlertContainer');
            const low = db.items.filter(i => i.stock < i.minStock);
            if (low.length === 0) {
                container.innerHTML = '<div class="stock-warning" style="background:#d4edda;border-color:#28a745;">✅ تمام آئٹمز کا اسٹاک مناسب ہے</div>';
                return;
            }
            let html = '';
            low.forEach(i => {
                html += `<div class="stock-warning">⚠️ <strong>${i.name}</strong> کا اسٹاک ${i.stock} ${i.unit} رہ گیا ہے (وارننگ: ${i.minStock})</div>`;
            });
            container.innerHTML = html;
        }

        // =======================================================
        // 4. گاہک
        // =======================================================
        function addCustomer() {
            const name = document.getElementById('customerName').value.trim();
            const phone = document.getElementById('customerPhone').value.trim();
            if (!name) return alert('گاہک کا نام درج کریں');
            db.customers.push({ id: Date.now(), name, phone, balance: 0 });
            saveDB();
            renderCustomers();
            document.getElementById('customerName').value = '';
            document.getElementById('customerPhone').value = '';
            alert('✅ گاہک شامل ہو گیا');
        }

        function renderCustomers() {
            const container = document.getElementById('customerList');
            if (db.customers.length === 0) {
                container.innerHTML = '<div class="list-item">کوئی گاہک نہیں</div>';
                return;
            }
            let html = '';
            db.customers.forEach(c => {
                html += `<div class="list-item"><span class="left">${c.name} (${c.phone||'N/A'})</span><span class="right">ادھار: ${c.balance}</span></div>`;
            });
            container.innerHTML = html;
            // populate customer select in sales
            const sel = document.getElementById('saleCustomerSelect');
            sel.innerHTML = '<option value="">کیش (بغیر گاہک)</option>';
            db.customers.forEach(c => {
                const opt = document.createElement('option');
                opt.value = c.id;
                opt.textContent = `${c.name} (ادھار: ${c.balance})`;
                sel.appendChild(opt);
            });
        }

        // =======================================================
        // 5. فروخت
        // =======================================================
        function addSale() {
            const itemId = parseInt(document.getElementById('saleItemSelect').value);
            const qty = parseFloat(document.getElementById('saleQty').value);
            const paid = parseFloat(document.getElementById('salePaid').value) || 0;
            const customerId = document.getElementById('saleCustomerSelect').value;
            if (!itemId || !qty || qty <= 0) return alert('آئٹم اور مقدار درست کریں');
            const item = db.items.find(i => i.id === itemId);
            if (!item) return alert('آئٹم نہیں ملا');
            if (item.stock < qty) return alert(`ناکافی اسٹاک! دستیاب: ${item.stock} ${item.unit}`);
            const total = qty * item.sp;
            const remaining = total - paid;
            // Sale record
            const sale = {
                id: Date.now(),
                itemId,
                itemName: item.name,
                qty,
                unit: item.unit,
                rate: item.sp,
                total,
                paid,
                remaining,
                customerId: customerId || null,
                date: new Date().toISOString().split('T')[0],
                timestamp: Date.now()
            };
            db.sales.push(sale);
            // Update stock
            item.stock -= qty;
            // Update customer balance
            if (customerId && remaining > 0) {
                const cust = db.customers.find(c => c.id == customerId);
                if (cust) cust.balance += remaining;
            }
            // Transaction log
            db.transactions.push({
                id: Date.now(),
                type: 'sale',
                desc: `${item.name} x ${qty} ${item.unit}`,
                amount: total,
                date: sale.date
            });
            saveDB();
            renderItems();
            renderCustomers();
            renderTodaySales();
            updateDashboard();
            document.getElementById('saleQty').value = '';
            document.getElementById('salePaid').value = '';
            alert('✅ فروخت مکمل! کل رقم: ' + total);
        }

        function renderTodaySales() {
            const container = document.getElementById('todaySalesList');
            const today = new Date().toISOString().split('T')[0];
            const todaySales = db.sales.filter(s => s.date === today);
            if (todaySales.length === 0) {
                container.innerHTML = '<div class="list-item">آج کوئی فروخت نہیں</div>';
                return;
            }
            let html = '';
            let total = 0;
            todaySales.forEach(s => {
                total += s.total;
                html += `<div class="list-item"><span class="left">${s.itemName} x${s.qty}</span><span class="right">${s.total} (ادا: ${s.paid})</span></div>`;
            });
            html += `<div class="list-item" style="font-weight:700;border-top:2px solid #1e3c72;"><span class="left">کل آج کی فروخت</span><span class="right">${total}</span></div>`;
            container.innerHTML = html;
        }

        // =======================================================
        // 6. رپورٹ
        // =======================================================
        function generateReport(type) {
            const today = new Date().toISOString().split('T')[0];
            const sales = db.sales.filter(s => s.date === today);
            const totalSales = sales.reduce((sum, s) => sum + s.total, 0);
            const totalPaid = sales.reduce((sum, s) => sum + s.paid, 0);
            const totalRemaining = sales.reduce((sum, s) => sum + s.remaining, 0);
            const report = `
📊 روزانہ رپورٹ - ${today}
━━━━━━━━━━━━━━━━━━━
کل فروخت: ${totalSales}
وصول شدہ: ${totalPaid}
بقایا: ${totalRemaining}
کل بل: ${sales.length}
━━━━━━━━━━━━━━━━━━━
💎 ماربل شاپ مینجمنٹ
            `;
            document.getElementById('reportDisplay').textContent = report;
            return report;
        }

        function sendWhatsApp() {
            const report = document.getElementById('reportDisplay').textContent;
            if (!report || report.trim() === '') return alert('پہلے رپورٹ جنریٹ کریں');
            // WhatsApp API (mobile)
            const phone = '923001234567'; // اپنا نمبر ڈالیں
            const url = `https://wa.me/${phone}?text=${encodeURIComponent(report)}`;
            window.open(url, '_blank');
        }

        // =======================================================
        // 7. ڈیش بورڈ
        // =======================================================
        function updateDashboard() {
            document.getElementById('dashTotalItems').textContent = db.items.length;
            const today = new Date().toISOString().split('T')[0];
            const todaySales = db.sales.filter(s => s.date === today);
            const total = todaySales.reduce((sum, s) => sum + s.total, 0);
            document.getElementById('dashTotalSales').textContent = total;
            renderTransactions();
        }

        function renderTransactions() {
            const container = document.getElementById('allTransactions');
            if (db.transactions.length === 0) {
                container.innerHTML = '<div class="list-item">کوئی لین دین نہیں</div>';
                return;
            }
            let html = '<div class="table-wrap"><table><tr><th>تاریخ</th><th>تفصیل</th><th>رقم</th></tr>';
            const sorted = [...db.transactions].reverse().slice(0, 20);
            sorted.forEach(t => {
                html += `<tr><td>${t.date}</td><td>${t.desc}</td><td>${t.amount}</td></tr>`;
            });
            html += '</table></div>';
            container.innerHTML = html;
        }

        // =======================================================
        // 8. نیویگیشن (Tabs + Bottom Nav)
        // =======================================================
        function switchTab(tabId) {
            document.querySelectorAll('.section').forEach(el => el.classList.remove('active'));
            document.getElementById('section-' + tabId).classList.add('active');
            document.querySelectorAll('.tab').forEach(el => el.classList.remove('active'));
            document.querySelector(`.tab[data-tab="${tabId}"]`).classList.add('active');
            document.querySelectorAll('.bottom-nav .nav-item').forEach(el => el.classList.remove('active'));
            document.querySelector(`.bottom-nav .nav-item[data-tab="${tabId}"]`).classList.add('active');
            // Refresh data on switch
            if (tabId === 'dashboard') updateDashboard();
            if (tabId === 'items') renderItems();
            if (tabId === 'sales') { renderItems(); renderTodaySales(); }
            if (tabId === 'reports') generateReport('daily');
        }

        document.querySelectorAll('.tab').forEach(tab => {
            tab.addEventListener('click', function() {
                switchTab(this.dataset.tab);
            });
        });
        document.querySelectorAll('.bottom-nav .nav-item').forEach(item => {
            item.addEventListener('click', function() {
                switchTab(this.dataset.tab);
            });
        });

        // =======================================================
        // 9. انیشیالائز
        // =======================================================
        function init() {
            renderItems();
            renderCustomers();
            renderTodaySales();
            updateDashboard();
            generateReport('daily');
        }
        init();

        // Auto-save on any change (already saving in functions)
        console.log('🚀 ماربل شاپ ایپ لوڈ ہو گئی!');
        console.log('📦 کل آئٹمز:', db.items.length);
        console.log('👥 کل گاہک:', db.customers.length);
        console.log('💰 کل فروخت:', db.sales.length);
    </script>
</body>
</html>
