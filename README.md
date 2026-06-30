<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>Marble POS System</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        :root {
            --primary: #1a365d;
            --secondary: #2d3748;
            --success: #38a169;
            --danger: #e53e3e;
            --warning: #d69e2e;
            --bg: #f0f4f8;
            --card: #ffffff;
        }
        body {
            background: var(--bg);
            padding: 10px;
            padding-bottom: 80px;
            direction: rtl;
        }
        .app-container {
            max-width: 500px;
            margin: 0 auto;
            background: var(--card);
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            overflow: hidden;
            padding: 15px;
        }
        /* Header */
        .header {
            background: linear-gradient(135deg, #1a365d, #2d3748);
            color: white;
            padding: 15px 20px;
            border-radius: 15px;
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }
        .header h1 {
            font-size: 20px;
            font-weight: 700;
        }
        .lang-toggle {
            background: rgba(255,255,255,0.2);
            border: none;
            color: white;
            padding: 6px 15px;
            border-radius: 20px;
            cursor: pointer;
            font-weight: 600;
            font-size: 13px;
        }
        .lang-toggle:active {
            transform: scale(0.95);
        }
        /* Cards */
        .card {
            background: white;
            border-radius: 15px;
            padding: 15px;
            margin-bottom: 15px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.06);
            border: 1px solid #e2e8f0;
        }
        .card-title {
            font-size: 16px;
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 8px;
        }
        /* POS Grid */
        .pos-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
        }
        .pos-item {
            background: #f7fafc;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            padding: 12px;
            text-align: center;
            cursor: pointer;
            transition: 0.3s;
            font-size: 13px;
        }
        .pos-item:active {
            transform: scale(0.95);
            background: #edf2f7;
        }
        .pos-item .name {
            font-weight: 600;
            color: var(--secondary);
        }
        .pos-item .price {
            color: var(--primary);
            font-weight: 700;
            font-size: 14px;
        }
        .pos-item .stock {
            font-size: 11px;
            color: #718096;
        }
        .pos-item.out-of-stock {
            opacity: 0.5;
            pointer-events: none;
        }
        /* Cart */
        .cart-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px solid #e2e8f0;
            font-size: 14px;
            align-items: center;
        }
        .cart-item .qty-control {
            display: flex;
            gap: 6px;
            align-items: center;
        }
        .cart-item .qty-control button {
            background: #e2e8f0;
            border: none;
            border-radius: 50%;
            width: 28px;
            height: 28px;
            font-weight: 700;
            cursor: pointer;
            font-size: 16px;
        }
        .cart-item .qty-control button:active {
            transform: scale(0.9);
        }
        .btn-sm {
            padding: 6px 12px;
            border: none;
            border-radius: 8px;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
        }
        .btn-danger-sm {
            background: #fed7d7;
            color: #9b2c2c;
        }
        /* Tabs */
        .tabs {
            display: flex;
            gap: 4px;
            margin-bottom: 15px;
            flex-wrap: wrap;
            background: #f7fafc;
            padding: 5px;
            border-radius: 12px;
        }
        .tab {
            flex: 1;
            padding: 8px 4px;
            text-align: center;
            background: transparent;
            border-radius: 10px;
            font-weight: 600;
            font-size: 10px;
            cursor: pointer;
            transition: 0.3s;
            min-width: 45px;
            color: #4a5568;
        }
        .tab i {
            display: block;
            font-size: 16px;
            margin-bottom: 2px;
        }
        .tab.active {
            background: var(--primary);
            color: white;
        }
        .section {
            display: none;
        }
        .section.active {
            display: block;
        }
        /* Bottom Nav */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            display: flex;
            justify-content: space-around;
            padding: 5px 0;
            box-shadow: 0 -2px 15px rgba(0,0,0,0.1);
            border-top: 1px solid #e2e8f0;
            z-index: 1000;
            max-width: 500px;
            margin: 0 auto;
        }
        .bottom-nav .nav-item {
            text-align: center;
            font-size: 9px;
            color: #718096;
            cursor: pointer;
            padding: 4px 6px;
            border-radius: 10px;
            transition: 0.3s;
        }
        .bottom-nav .nav-item i {
            font-size: 18px;
            display: block;
        }
        .bottom-nav .nav-item.active {
            color: var(--primary);
            font-weight: 700;
        }
        /* Form */
        .form-group {
            margin-bottom: 10px;
        }
        .form-group label {
            display: block;
            font-size: 13px;
            font-weight: 600;
            color: var(--secondary);
            margin-bottom: 4px;
        }
        .form-group input, .form-group select {
            width: 100%;
            padding: 10px 12px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            font-size: 14px;
            background: #f7fafc;
        }
        .form-group input:focus {
            border-color: var(--primary);
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
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
        }
        .btn:active {
            transform: scale(0.96);
        }
        .btn-primary { background: var(--primary); color: white; }
        .btn-success { background: var(--success); color: white; }
        .btn-danger { background: var(--danger); color: white; }
        .btn-warning { background: var(--warning); color: white; }
        .btn-outline { background: transparent; border: 2px solid var(--primary); color: var(--primary); }
        .grid-2 {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }
        /* Table */
        .table-wrap {
            overflow-x: auto;
            margin-top: 8px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 12px;
        }
        th {
            background: var(--primary);
            color: white;
            padding: 6px 4px;
            text-align: center;
            font-size: 11px;
        }
        td {
            padding: 6px 4px;
            border-bottom: 1px solid #e2e8f0;
            text-align: center;
        }
        tr:nth-child(even) { background: #f7fafc; }
        .badge {
            display: inline-block;
            padding: 2px 10px;
            border-radius: 20px;
            font-size: 10px;
            font-weight: 600;
        }
        .badge-success { background: #c6f6d5; color: #22543d; }
        .badge-danger { background: #fed7d7; color: #9b2c2c; }
        .badge-warning { background: #fefcbf; color: #744210; }
        .highlight {
            padding: 10px;
            border-radius: 10px;
            border-right: 4px solid var(--primary);
            background: #ebf8ff;
            margin: 6px 0;
            font-size: 13px;
        }
        .total-bar {
            background: var(--primary);
            color: white;
            padding: 12px;
            border-radius: 12px;
            display: flex;
            justify-content: space-between;
            font-weight: 700;
            font-size: 16px;
            margin-top: 10px;
        }
        @media (max-width: 480px) {
            .grid-2 { grid-template-columns: 1fr; }
            .pos-grid { grid-template-columns: 1fr 1fr; }
            .header h1 { font-size: 16px; }
        }
    </style>
</head>
<body>
<div class="app-container" id="app">
    <!-- Header -->
    <div class="header">
        <h1><i class="fas fa-store"></i> <span id="shopName">Marble POS</span></h1>
        <button class="lang-toggle" onclick="toggleLang()">
            <i class="fas fa-language"></i> <span id="langLabel">اردو</span>
        </button>
    </div>

    <!-- Tabs -->
    <div class="tabs" id="tabHeaders">
        <div class="tab active" data-tab="pos"><i class="fas fa-shopping-cart"></i> <span class="tabLabel">POS</span></div>
        <div class="tab" data-tab="stock"><i class="fas fa-boxes"></i> <span class="tabLabel">Stock</span></div>
        <div class="tab" data-tab="customers"><i class="fas fa-users"></i> <span class="tabLabel">Customers</span></div>
        <div class="tab" data-tab="expenses"><i class="fas fa-wallet"></i> <span class="tabLabel">Expenses</span></div>
        <div class="tab" data-tab="reports"><i class="fas fa-chart-pie"></i> <span class="tabLabel">Reports</span></div>
    </div>

    <!-- ==================== POS SECTION ==================== -->
    <div id="section-pos" class="section active">
        <div class="card">
            <div class="card-title"><i class="fas fa-list"></i> <span class="label">Select Item</span></div>
            <div class="pos-grid" id="posGrid"></div>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-shopping-cart"></i> <span class="label">Cart</span> (<span id="cartCount">0</span>)</div>
            <div id="cartItems"></div>
            <div class="total-bar">
                <span><span class="label">Total</span>:</span>
                <span id="cartTotal">0</span>
            </div>
            <div class="grid-2" style="margin-top:10px;">
                <select id="saleType" style="padding:10px;border-radius:10px;border:2px solid #e2e8f0;">
                    <option value="cash">💵 Cash</option>
                    <option value="credit">📝 Credit</option>
                </select>
                <select id="saleCustomerSelect" style="padding:10px;border-radius:10px;border:2px solid #e2e8f0;">
                    <option value="">👤 Walk-in</option>
                </select>
            </div>
            <div class="form-group" style="margin-top:8px;">
                <input type="number" id="salePaid" placeholder="Amount Paid" value="0" style="width:100%;padding:10px;border-radius:10px;border:2px solid #e2e8f0;">
            </div>
            <button class="btn btn-success" onclick="completeSale()"><i class="fas fa-check"></i> <span class="label">Complete Sale</span></button>
        </div>
    </div>

    <!-- ==================== STOCK SECTION ==================== -->
    <div id="section-stock" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-plus-circle"></i> <span class="label">Add New Item</span></div>
            <div class="form-group">
                <label class="label">Item Name</label>
                <input type="text" id="itemName" placeholder="e.g. Atlantic Marble">
            </div>
            <div class="grid-2">
                <div class="form-group">
                    <label class="label">Unit</label>
                    <select id="itemUnit">
                        <option value="m²">m²</option>
                        <option value="pcs">Pieces</option>
                    </select>
                </div>
                <div class="form-group">
                    <label class="label">Price</label>
                    <input type="number" id="itemPrice" placeholder="Sell Price">
                </div>
            </div>
            <div class="grid-2">
                <div class="form-group">
                    <label class="label">Stock</label>
                    <input type="number" id="itemStock" placeholder="Quantity">
                </div>
                <div class="form-group">
                    <label class="label">Min Stock</label>
                    <input type="number" id="itemMinStock" placeholder="Warning Level">
                </div>
            </div>
            <button class="btn btn-primary" onclick="addItem()"><i class="fas fa-save"></i> <span class="label">Add Item</span></button>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-list"></i> <span class="label">Stock List</span></div>
            <div id="stockListContainer"></div>
        </div>
    </div>

    <!-- ==================== CUSTOMERS ==================== -->
    <div id="section-customers" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-user-plus"></i> <span class="label">Add Customer</span></div>
            <div class="form-group">
                <input type="text" id="custName" placeholder="Customer Name">
            </div>
            <div class="form-group">
                <input type="text" id="custPhone" placeholder="Phone">
            </div>
            <button class="btn btn-primary" onclick="addCustomer()"><i class="fas fa-user-plus"></i> <span class="label">Add Customer</span></button>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-users"></i> <span class="label">All Customers</span></div>
            <div id="customerListContainer"></div>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-hand-holding-usd"></i> <span class="label">Receive Payment</span></div>
            <select id="receiveCustomerSelect" style="width:100%;padding:10px;border-radius:10px;border:2px solid #e2e8f0;margin-bottom:10px;"></select>
            <input type="number" id="receiveAmount" placeholder="Amount Received" style="width:100%;padding:10px;border-radius:10px;border:2px solid #e2e8f0;margin-bottom:10px;">
            <button class="btn btn-success" onclick="receivePayment()"><i class="fas fa-money-bill-wave"></i> <span class="label">Receive</span></button>
        </div>
    </div>

    <!-- ==================== EXPENSES ==================== -->
    <div id="section-expenses" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-plus-circle"></i> <span class="label">Add Expense</span></div>
            <div class="form-group">
                <input type="text" id="expenseHead" placeholder="Expense Head (e.g. Electricity)">
            </div>
            <div class="form-group">
                <input type="number" id="expenseAmount" placeholder="Amount">
            </div>
            <div class="form-group">
                <input type="text" id="expenseNote" placeholder="Note (optional)">
            </div>
            <button class="btn btn-danger" onclick="addExpense()"><i class="fas fa-minus-circle"></i> <span class="label">Add Expense</span></button>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-list"></i> <span class="label">Today's Expenses</span></div>
            <div id="todayExpensesList"></div>
        </div>
    </div>

    <!-- ==================== REPORTS ==================== -->
    <div id="section-reports" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-calendar-day"></i> <span class="label">Daily Report</span></div>
            <div class="grid-2">
                <button class="btn btn-primary" onclick="generateReport('daily')"><i class="fas fa-eye"></i> <span class="label">Daily</span></button>
                <button class="btn btn-warning" onclick="generateReport('weekly')"><i class="fas fa-calendar-week"></i> <span class="label">Weekly</span></button>
                <button class="btn btn-info" onclick="generateReport('monthly')"><i class="fas fa-calendar-alt"></i> <span class="label">Monthly</span></button>
                <button class="btn btn-success" onclick="sendWhatsAppReport()"><i class="fab fa-whatsapp"></i> <span class="label">WhatsApp</span></button>
            </div>
            <div id="reportDisplay" style="margin-top:12px;background:#f7fafc;padding:15px;border-radius:10px;font-size:13px;white-space:pre-wrap;font-family:monospace;"></div>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-chart-line"></i> <span class="label">Profit / Loss</span></div>
            <div id="profitLossDisplay"></div>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-history"></i> <span class="label">All Transactions</span></div>
            <div id="allTransactionsList"></div>
        </div>
    </div>
</div>

<!-- Bottom Navigation -->
<div class="bottom-nav">
    <div class="nav-item active" data-tab="pos"><i class="fas fa-shopping-cart"></i> <span class="tabLabel">POS</span></div>
    <div class="nav-item" data-tab="stock"><i class="fas fa-boxes"></i> <span class="tabLabel">Stock</span></div>
    <div class="nav-item" data-tab="customers"><i class="fas fa-users"></i> <span class="tabLabel">Customers</span></div>
    <div class="nav-item" data-tab="expenses"><i class="fas fa-wallet"></i> <span class="tabLabel">Expenses</span></div>
    <div class="nav-item" data-tab="reports"><i class="fas fa-chart-pie"></i> <span class="tabLabel">Reports</span></div>
</div>

<script>
    // ============================================================
    // LANGUAGE SYSTEM
    // ============================================================
    let currentLang = 'en';
    const translations = {
        en: {
            shopName: 'Marble POS',
            pos: 'POS',
            stock: 'Stock',
            customers: 'Customers',
            expenses: 'Expenses',
            reports: 'Reports',
            selectItem: 'Select Item',
            cart: 'Cart',
            total: 'Total',
            completeSale: 'Complete Sale',
            addItem: 'Add Item',
            addCustomer: 'Add Customer',
            addExpense: 'Add Expense',
            receive: 'Receive',
            daily: 'Daily',
            weekly: 'Weekly',
            monthly: 'Monthly',
            whatsapp: 'WhatsApp',
            profitLoss: 'Profit / Loss',
            allTransactions: 'All Transactions',
            cash: 'Cash',
            credit: 'Credit',
            walkin: 'Walk-in',
            amountPaid: 'Amount Paid',
            itemName: 'Item Name',
            unit: 'Unit',
            price: 'Price',
            stock: 'Stock',
            minStock: 'Min Stock',
            customerName: 'Customer Name',
            phone: 'Phone',
            expenseHead: 'Expense Head',
            amount: 'Amount',
            note: 'Note',
            receivePayment: 'Receive Payment',
            addNewItem: 'Add New Item',
            allCustomers: 'All Customers',
            todayExpenses: "Today's Expenses",
            dailyReport: 'Daily Report'
        },
        ur: {
            shopName: 'ماربل پی او ایس',
            pos: 'فروخت',
            stock: 'اسٹاک',
            customers: 'گاہک',
            expenses: 'اخراجات',
            reports: 'رپورٹس',
            selectItem: 'آئٹم منتخب کریں',
            cart: 'کارٹ',
            total: 'کل',
            completeSale: 'فروخت مکمل کریں',
            addItem: 'آئٹم شامل کریں',
            addCustomer: 'گاہک شامل کریں',
            addExpense: 'خرچ شامل کریں',
            receive: 'وصول کریں',
            daily: 'روزانہ',
            weekly: 'ہفتہ وار',
            monthly: 'ماہانہ',
            whatsapp: 'واٹس ایپ',
            profitLoss: 'منافع / نقصان',
            allTransactions: 'تمام لین دین',
            cash: 'نقد',
            credit: 'ادھار',
            walkin: 'بغیر گاہک',
            amountPaid: 'ادا کردہ رقم',
            itemName: 'آئٹم کا نام',
            unit: 'یونٹ',
            price: 'قیمت',
            stock: 'اسٹاک',
            minStock: 'کم از کم اسٹاک',
            customerName: 'گاہک کا نام',
            phone: 'فون نمبر',
            expenseHead: 'خرچ کا عنوان',
            amount: 'رقم',
            note: 'تفصیل',
            receivePayment: 'ادھار وصولی',
            addNewItem: 'نیا آئٹم شامل کریں',
            allCustomers: 'تمام گاہک',
            todayExpenses: 'آج کے اخراجات',
            dailyReport: 'روزانہ رپورٹ'
        }
    };

    function t(key) {
        return translations[currentLang][key] || key;
    }

    function toggleLang() {
        currentLang = currentLang === 'en' ? 'ur' : 'en';
        document.getElementById('langLabel').textContent = currentLang === 'en' ? 'اردو' : 'English';
        updateLanguage();
        renderAll();
    }

    function updateLanguage() {
        document.querySelectorAll('.label').forEach(el => {
            const key = el.textContent.trim().toLowerCase().replace(/\s+/g, '');
            // Map keys
            const map = {
                'selectitem': 'selectItem',
                'cart': 'cart',
                'total': 'total',
                'completesale': 'completeSale',
                'additem': 'addItem',
                'addcustomer': 'addCustomer',
                'addexpense': 'addExpense',
                'receive': 'receive',
                'daily': 'daily',
                'weekly': 'weekly',
                'monthly': 'monthly',
                'whatsapp': 'whatsapp',
                'profit/loss': 'profitLoss',
                'alltransactions': 'allTransactions',
                'itemname': 'itemName',
                'unit': 'unit',
                'price': 'price',
                'stock': 'stock',
                'minstock': 'minStock',
                'customername': 'customerName',
                'phone': 'phone',
                'expensehead': 'expenseHead',
                'amount': 'amount',
                'note': 'note',
                'receivepayment': 'receivePayment',
                'addnewitem': 'addNewItem',
                'allcustomers': 'allCustomers',
                'today\'sexpenses': 'todayExpenses',
                'dailyreport': 'dailyReport'
            };
            const translated = t(map[key] || key);
            if (translated) el.textContent = translated;
        });
        document.querySelectorAll('.tabLabel').forEach(el => {
            const key = el.textContent.trim().toLowerCase();
            const map = { 'pos': 'pos', 'stock': 'stock', 'customers': 'customers', 'expenses': 'expenses', 'reports': 'reports' };
            el.textContent = t(map[key] || key);
        });
        document.getElementById('shopName').textContent = t('shopName');
        document.querySelectorAll('input[placeholder]').forEach(el => {
            const key = el.placeholder.toLowerCase().replace(/\s+/g, '');
            const map = {
                'e.g.atlanticmarble': 'itemName',
                'sellprice': 'price',
                'quantity': 'stock',
                'warninglevel': 'minStock',
                'customername': 'customerName',
                'phone': 'phone',
                'expensehead(e.g.electricity)': 'expenseHead',
                'amount': 'amount',
                'note(optional)': 'note',
                'amountpaid': 'amountPaid',
                'amountreceived': 'amountPaid'
            };
            const translated = t(map[key] || key);
            if (translated) el.placeholder = translated;
        });
        document.querySelectorAll('option').forEach(el => {
            if (el.value === 'cash') el.textContent = '💵 ' + t('cash');
            if (el.value === 'credit') el.textContent = '📝 ' + t('credit');
            if (el.value === '') el.textContent = '👤 ' + t('walkin');
        });
    }

    // ============================================================
    // DATABASE (localStorage)
    // ============================================================
    let db = {
        items: JSON.parse(localStorage.getItem('marble_items')) || [],
        customers: JSON.parse(localStorage.getItem('marble_customers')) || [],
        sales: JSON.parse(localStorage.getItem('marble_sales')) || [],
        expenses: JSON.parse(localStorage.getItem('marble_expenses')) || [],
        transactions: JSON.parse(localStorage.getItem('marble_transactions')) || []
    };

    function saveDB() {
        localStorage.setItem('marble_items', JSON.stringify(db.items));
        localStorage.setItem('marble_customers', JSON.stringify(db.customers));
        localStorage.setItem('marble_sales', JSON.stringify(db.sales));
        localStorage.setItem('marble_expenses', JSON.stringify(db.expenses));
        localStorage.setItem('marble_transactions', JSON.stringify(db.transactions));
    }

    // ============================================================
    // CART SYSTEM
    // ============================================================
    let cart = [];

    function addToCart(itemId) {
        const item = db.items.find(i => i.id === itemId);
        if (!item) return;
        if (item.stock <= 0) return alert('Out of stock!');
        const existing = cart.find(c => c.itemId === itemId);
        if (existing) {
            if (existing.qty >= item.stock) return alert('Not enough stock!');
            existing.qty++;
        } else {
            cart.push({ itemId, qty: 1, name: item.name, price: item.price, unit: item.unit });
        }
        renderCart();
    }

    function removeFromCart(itemId) {
        cart = cart.filter(c => c.itemId !== itemId);
        renderCart();
    }

    function updateCartQty(itemId, delta) {
        const item = cart.find(c => c.itemId === itemId);
        if (!item) return;
        const stockItem = db.items.find(i => i.id === itemId);
        if (delta > 0 && item.qty >= stockItem.stock) return alert('Not enough stock!');
        item.qty += delta;
        if (item.qty <= 0) {
            cart = cart.filter(c => c.itemId !== itemId);
        }
        renderCart();
    }

    function renderCart() {
        const container = document.getElementById('cartItems');
        if (cart.length === 0) {
            container.innerHTML = '<div style="text-align:center;color:#718096;padding:20px;">Cart is empty</div>';
            document.getElementById('cartTotal').textContent = '0';
            document.getElementById('cartCount').textContent = '0';
            return;
        }
        let html = '';
        let total = 0;
        cart.forEach(c => {
            const subtotal = c.qty * c.price;
            total += subtotal;
            html += `<div class="cart-item">
                <span><strong>${c.name}</strong> (${c.price} x ${c.qty})</span>
                <div class="qty-control">
                    <button onclick="updateCartQty(${c.itemId}, -1)">−</button>
                    <span style="min-width:25px;text-align:center;">${c.qty}</span>
                    <button onclick="updateCartQty(${c.itemId}, 1)">+</button>
                    <button class="btn-sm btn-danger-sm" onclick="removeFromCart(${c.itemId})"><i class="fas fa-trash"></i></button>
                </div>
            </div>`;
        });
        container.innerHTML = html;
        document.getElementById('cartTotal').textContent = total;
        document.getElementById('cartCount').textContent = cart.reduce((sum, c) => sum + c.qty, 0);
    }

    // ============================================================
    // POS GRID
    // ============================================================
    function renderPOS() {
        const grid = document.getElementById('posGrid');
        if (db.items.length === 0) {
            grid.innerHTML = '<div style="grid-column:1/-1;text-align:center;color:#718096;padding:20px;">No items. Add some in Stock section!</div>';
            return;
        }
        let html = '';
        db.items.forEach(item => {
            const outOfStock = item.stock <= 0;
            html += `<div class="pos-item ${outOfStock ? 'out-of-stock' : ''}" onclick="${outOfStock ? '' : `addToCart(${item.id})`}">
                <div class="name">${item.name}</div>
                <div class="price">${item.price}</div>
                <div class="stock">${item.stock} ${item.unit}</div>
            </div>`;
        });
        grid.innerHTML = html;
    }

    // ============================================================
    // COMPLETE SALE
    // ============================================================
    function completeSale() {
        if (cart.length === 0) return alert('Cart is empty!');
        const type = document.getElementById('saleType').value;
        const customerId = document.getElementById('saleCustomerSelect').value;
        const paid = parseFloat(document.getElementById('salePaid').value) || 0;

        let total = 0;
        let saleItems = [];
        cart.forEach(c => {
            const item = db.items.find(i => i.id === c.itemId);
            if (!item) return;
            if (item.stock < c.qty) return alert(`${item.name} has only ${item.stock} in stock!`);
            const subtotal = c.qty * c.price;
            total += subtotal;
            saleItems.push({ itemId: c.itemId, name: item.name, qty: c.qty, price: c.price, unit: item.unit });
            item.stock -= c.qty;
        });

        const remaining = total - paid;
        const sale = {
            id: Date.now(),
            items: saleItems,
            total,
            paid,
            remaining,
            type,
            customerId: customerId || null,
            date: getToday(),
            timestamp: Date.now()
        };

        db.sales.push(sale);

        if (customerId && type === 'credit') {
            const cust = db.customers.find(c => c.id == customerId);
            if (cust) cust.balance += remaining;
        }

        db.transactions.push({
            id: Date.now(),
            type: 'sale',
            desc: `${saleItems.length} items (${type})`,
            amount: total,
            date: getToday()
        });

        cart = [];
        saveDB();
        renderAll();
        document.getElementById('salePaid').value = '0';
        alert(`✅ Sale Complete! Total: ${total} | Paid: ${paid} | Remaining: ${remaining}`);
    }

    // ============================================================
    // ITEMS (Stock Management)
    // ============================================================
    function addItem() {
        const name = document.getElementById('itemName').value.trim();
        const unit = document.getElementById('itemUnit').value;
        const price = parseFloat(document.getElementById('itemPrice').value);
        const stock = parseFloat(document.getElementById('itemStock').value) || 0;
        const minStock = parseFloat(document.getElementById('itemMinStock').value) || 0;

        if (!name || !price || price <= 0) return alert('Please enter name and price');
        if (db.items.find(i => i.name.toLowerCase() === name.toLowerCase())) return alert('Item already exists!');

        db.items.push({
            id: Date.now(),
            name,
            unit,
            price,
            stock,
            minStock
        });

        saveDB();
        renderAll();
        document.getElementById('itemName').value = '';
        document.getElementById('itemPrice').value = '';
        document.getElementById('itemStock').value = '';
        document.getElementById('itemMinStock').value = '';
        alert('✅ Item added successfully!');
    }

    function deleteItem(id) {
        if (!confirm('Delete this item?')) return;
        db.items = db.items.filter(i => i.id !== id);
        saveDB();
        renderAll();
    }

    function renderStockList() {
        const container = document.getElementById('stockListContainer');
        if (db.items.length === 0) {
            container.innerHTML = '<div class="list-item">No items</div>';
            return;
        }
        let html = `<div class="table-wrap"><table>
            <tr><th>Name</th><th>Stock</th><th>Price</th><th>Action</th></tr>`;
        db.items.forEach(item => {
            const warn = item.stock < item.minStock ? '⚠️' : '';
            html += `<tr>
                <td>${item.name}</td>
                <td>${item.stock} ${item.unit} ${warn}</td>
                <td>${item.price}</td>
                <td><button class="btn-sm btn-danger-sm" onclick="deleteItem(${item.id})"><i class="fas fa-trash"></i></button></td>
            </tr>`;
        });
        html += '</table></div>';
        container.innerHTML = html;
    }

    // ============================================================
    // CUSTOMERS
    // ============================================================
    function addCustomer() {
        const name = document.getElementById('custName').value.trim();
        const phone = document.getElementById('custPhone').value.trim();
        if (!name) return alert('Enter customer name');
        db.customers.push({ id: Date.now(), name, phone, balance: 0 });
        saveDB();
        renderAll();
        document.getElementById('custName').value = '';
        document.getElementById('custPhone').value = '';
        alert('✅ Customer added!');
    }

    function renderCustomers() {
        const container = document.getElementById('customerListContainer');
        if (db.customers.length === 0) {
            container.innerHTML = '<div class="list-item">No customers</div>';
        } else {
            let html = '<div class="table-wrap"><table><tr><th>Name</th><th>Phone</th><th>Balance</th></tr>';
            db.customers.forEach(c => {
                html += `<tr><td>${c.name}</td><td>${c.phone || '-'}</td><td>${c.balance}</td></tr>`;
            });
            html += '</table></div>';
            container.innerHTML = html;
        }

        // Populate selects
        ['saleCustomerSelect', 'receiveCustomerSelect'].forEach(id => {
            const sel = document.getElementById(id);
            if (!sel) return;
            const currentVal = sel.value;
            sel.innerHTML = '<option value="">👤 Walk-in</option>';
            db.customers.forEach(c => {
                const opt = document.createElement('option');
                opt.value = c.id;
                opt.textContent = `${c.name} (${c.balance})`;
                sel.appendChild(opt);
            });
            if (currentVal) sel.value = currentVal;
        });
    }

    function receivePayment() {
        const customerId = document.getElementById('receiveCustomerSelect').value;
        const amount = parseFloat(document.getElementById('receiveAmount').value);
        if (!customerId) return alert('Select customer');
        if (!amount || amount <= 0) return alert('Enter valid amount');
        const cust = db.customers.find(c => c.id == customerId);
        if (!cust) return alert('Customer not found');
        if (cust.balance < amount) return alert(`Balance is ${cust.balance}, cannot receive more`);

        cust.balance -= amount;
        db.transactions.push({
            id: Date.now(),
            type: 'receive',
            desc: `Received from ${cust.name}`,
            amount: amount,
            date: getToday()
        });
        saveDB();
        renderAll();
        document.getElementById('receiveAmount').value = '';
        alert(`✅ Received ${amount}! New balance: ${cust.balance}`);
    }

    // ============================================================
    // EXPENSES
    // ============================================================
    function addExpense() {
        const head = document.getElementById('expenseHead').value.trim();
        const amount = parseFloat(document.getElementById('expenseAmount').value);
        const note = document.getElementById('expenseNote').value.trim();
        if (!head || !amount || amount <= 0) return alert('Enter head and amount');

        db.expenses.push({ id: Date.now(), head, amount, note, date: getToday() });
        db.transactions.push({
            id: Date.now(),
            type: 'expense',
            desc: head,
            amount: amount,
            date: getToday()
        });
        saveDB();
        renderAll();
        document.getElementById('expenseHead').value = '';
        document.getElementById('expenseAmount').value = '';
        document.getElementById('expenseNote').value = '';
        alert('✅ Expense added!');
    }

    function renderTodayExpenses() {
        const container = document.getElementById('todayExpensesList');
        const today = getToday();
        const todayExp = db.expenses.filter(e => e.date === today);

        if (todayExp.length === 0) {
            container.innerHTML = '<div class="list-item">No expenses today</div>';
            return;
        }

        let html = '<div class="table-wrap"><table><tr><th>Head</th><th>Amount</th></tr>';
        let total = 0;
        todayExp.forEach(e => {
            total += e.amount;
            html += `<tr><td>${e.head}</td><td>${e.amount}</td></tr>`;
        });
        html += `<tr style="font-weight:700;background:#edf2f7;"><td>Total</td><td>${total}</td></tr>`;
        html += '</table></div>';
        container.innerHTML = html;
    }

    // ============================================================
    // REPORTS
    // ============================================================
    function getToday() {
        return new Date().toISOString().split('T')[0];
    }

    function generateReport(type) {
        const today = getToday();
        let sales = [];
        let expenses = [];

        if (type === 'daily') {
            sales = db.sales.filter(s => s.date === today);
            expenses = db.expenses.filter(e => e.date === today);
        } else if (type === 'weekly') {
            const weekAgo = new Date();
            weekAgo.setDate(weekAgo.getDate() - 7);
            const weekStart = weekAgo.toISOString().split('T')[0];
            sales = db.sales.filter(s => s.date >= weekStart);
            expenses = db.expenses.filter(e => e.date >= weekStart);
        } else if (type === 'monthly') {
            const monthAgo = new Date();
            monthAgo.setMonth(monthAgo.getMonth() - 1);
            const monthStart = monthAgo.toISOString().split('T')[0];
            sales = db.sales.filter(s => s.date >= monthStart);
            expenses = db.expenses.filter(e => e.date >= monthStart);
        }

        const totalSales = sales.reduce((sum, s) => sum + s.total, 0);
        const totalPaid = sales.reduce((sum, s) => sum + s.paid, 0);
        const totalRemaining = sales.reduce((sum, s) => sum + s.remaining, 0);
        const totalExpenses = expenses.reduce((sum, e) => sum + e.amount, 0);
        const profit = totalSales - totalExpenses;

        const report = `
📊 ${type.toUpperCase()} REPORT - ${today}
━━━━━━━━━━━━━━━━━━━━━━━━━
💰 SALES
   Total Sales: ${totalSales}
   Amount Paid: ${totalPaid}
   Remaining: ${totalRemaining}
   Total Bills: ${sales.length}

💸 EXPENSES
   Total: ${totalExpenses}

📈 PROFIT / LOSS
   ${profit >= 0 ? '✅ Profit: ' + profit : '❌ Loss: ' + Math.abs(profit)}
━━━━━━━━━━━━━━━━━━━━━━━━━
🏪 Marble POS System
        `;

        document.getElementById('reportDisplay').textContent = report;

        // Profit/Loss display
        const plContainer = document.getElementById('profitLossDisplay');
        const plColor = profit >= 0 ? '#38a169' : '#e53e3e';
        plContainer.innerHTML = `
            <div style="text-align:center;padding:20px;background:${profit>=0?'#f0fff4':'#fff5f5'};border-radius:12px;">
                <div style="font-size:32px;font-weight:700;color:${plColor};">${profit >= 0 ? '✅ ' + profit : '❌ ' + Math.abs(profit)}</div>
                <div style="font-size:14px;color:#718096;">Sales ${totalSales} - Expenses ${totalExpenses}</div>
            </div>
        `;

        renderAllTransactions();
        return report;
    }

    function sendWhatsAppReport() {
        const report = document.getElementById('reportDisplay').textContent;
        if (!report || report.trim() === '') return alert('Generate report first!');
        const phone = '923001234567'; // Change to your number
        const url = `https://wa.me/${phone}?text=${encodeURIComponent(report)}`;
        window.open(url, '_blank');
    }

    function renderAllTransactions() {
        const container = document.getElementById('allTransactionsList');
        if (db.transactions.length === 0) {
            container.innerHTML = '<div class="list-item">No transactions</div>';
            return;
        }
        let html = '<div class="table-wrap"><table><tr><th>Date</th><th>Description</th><th>Amount</th></tr>';
        const sorted = [...db.transactions].reverse().slice(0, 30);
        sorted.forEach(t => {
            const color = t.type === 'expense' ? '#e53e3e' : '#38a169';
            html += `<tr><td>${t.date}</td><td>${t.desc}</td><td style="color:${color};font-weight:600;">${t.amount}</td></tr>`;
        });
        html += '</table></div>';
        container.innerHTML = html;
    }

    // ============================================================
    // NAVIGATION
    // ============================================================
    function switchTab(tabId) {
        document.querySelectorAll('.section').forEach(el => el.classList.remove('active'));
        document.getElementById('section-' + tabId).classList.add('active');
        document.querySelectorAll('.tab').forEach(el => el.classList.remove('active'));
        document.querySelector(`.tab[data-tab="${tabId}"]`).classList.add('active');
        document.querySelectorAll('.bottom-nav .nav-item').forEach(el => el.classList.remove('active'));
        document.querySelector(`.bottom-nav .nav-item[data-tab="${tabId}"]`).classList.add('active');

        if (tabId === 'pos') { renderPOS(); renderCart(); }
        if (tabId === 'stock') renderStockList();
        if (tabId === 'customers') renderCustomers();
        if (tabId === 'expenses') renderTodayExpenses();
        if (tabId === 'reports') { generateReport('daily'); renderAllTransactions(); }
    }

    document.querySelectorAll('.tab').forEach(tab => {
        tab.addEventListener('click', function() { switchTab(this.dataset.tab); });
    });
    document.querySelectorAll('.bottom-nav .nav-item').forEach(item => {
        item.addEventListener('click', function() { switchTab(this.dataset.tab); });
    });

    // ============================================================
    // INIT
    // ============================================================
    function renderAll() {
        renderPOS();
        renderCart();
        renderStockList();
        renderCustomers();
        renderTodayExpenses();
        renderAllTransactions();
        if (document.getElementById('section-reports').classList.contains('active')) {
            generateReport('daily');
        }
        updateLanguage();
    }

    // Sample items if empty
    if (db.items.length === 0) {
        db.items = [
            { id: 1, name: 'Atlantic Marble', unit: 'm²', price: 1200, stock: 100, minStock: 10 },
            { id: 2, name: 'White Marble', unit: 'm²', price: 1500, stock: 80, minStock: 8 },
            { id: 3, name: 'Black Granite', unit: 'm²', price: 2000, stock: 50, minStock: 5 },
            { id: 4, name: 'Tiles', unit: 'pcs', price: 250, stock: 500, minStock: 50 }
        ];
        saveDB();
    }

    renderAll();
    console.log('🚀 Marble POS System Ready!');
    console.log('📦 Items:', db.items.length);
    console.log('👥 Customers:', db.customers.length);
    console.log('💰 Sales:', db.sales.length);
</script>
</body>
</html>
