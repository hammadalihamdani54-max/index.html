<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>Warda Marble Tabuk</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
    <style>
        *{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma,Geneva,Verdana,sans-serif}
        body{background:#f0f4f8;padding:10px;padding-bottom:80px}
        .app-container{max-width:520px;margin:0 auto;background:white;border-radius:20px;box-shadow:0 10px 30px rgba(0,0,0,0.1);overflow:hidden;padding:15px}
        .header{background:linear-gradient(135deg,#1a365d,#2d3748);color:white;padding:15px 20px;border-radius:15px;margin-bottom:15px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap}
        .header h1{font-size:18px;font-weight:700}
        .header-badge{background:rgba(255,255,255,0.2);padding:4px 12px;border-radius:20px;font-size:11px}
        .lang-toggle,.mode-toggle{background:rgba(255,255,255,0.15);border:none;color:white;padding:5px 12px;border-radius:20px;cursor:pointer;font-weight:600;font-size:11px;margin:2px}
        .lang-toggle:active,.mode-toggle:active{transform:scale(0.95)}
        .card{background:white;border-radius:15px;padding:15px;margin-bottom:15px;box-shadow:0 2px 8px rgba(0,0,0,0.06);border:1px solid #e2e8f0}
        .card-title{font-size:16px;font-weight:700;color:#1a365d;margin-bottom:10px;display:flex;align-items:center;gap:8px;border-bottom:2px solid #e2e8f0;padding-bottom:8px}
        .card-title .cat-badge{font-size:10px;padding:2px 10px;border-radius:20px;color:white}
        .cat-stone{background:#8B7355}
        .cat-material{background:#2b6cb0}
        .pos-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
        .pos-item{background:#f7fafc;border:2px solid #e2e8f0;border-radius:12px;padding:10px;text-align:center;cursor:pointer;transition:0.3s;font-size:12px}
        .pos-item:active{transform:scale(0.95);background:#edf2f7}
        .pos-item .name{font-weight:600;color:#2d3748;font-size:13px}
        .pos-item .price{color:#1a365d;font-weight:700;font-size:14px}
        .pos-item .stock{font-size:10px;color:#718096}
        .pos-item .unit-badge{font-size:9px;background:#e2e8f0;padding:1px 8px;border-radius:10px}
        .pos-item.out-of-stock{opacity:0.5;pointer-events:none}
        .cart-item{display:flex;justify-content:space-between;padding:6px 0;border-bottom:1px solid #e2e8f0;font-size:13px;align-items:center}
        .cart-item .qty-control{display:flex;gap:4px;align-items:center}
        .cart-item .qty-control button{background:#e2e8f0;border:none;border-radius:50%;width:26px;height:26px;font-weight:700;cursor:pointer;font-size:14px}
        .cart-item .qty-control button:active{transform:scale(0.9)}
        .btn-sm{padding:4px 10px;border:none;border-radius:8px;font-size:11px;font-weight:600;cursor:pointer}
        .btn-danger-sm{background:#fed7d7;color:#9b2c2c}
        .tabs{display:flex;gap:4px;margin-bottom:15px;flex-wrap:wrap;background:#f7fafc;padding:5px;border-radius:12px}
        .tab{flex:1;padding:8px 4px;text-align:center;background:transparent;border-radius:10px;font-weight:600;font-size:10px;cursor:pointer;transition:0.3s;min-width:45px;color:#4a5568}
        .tab i{display:block;font-size:16px;margin-bottom:2px}
        .tab.active{background:#1a365d;color:white}
        .section{display:none}
        .section.active{display:block}
        .bottom-nav{position:fixed;bottom:0;left:0;right:0;background:white;display:flex;justify-content:space-around;padding:6px 0;box-shadow:0 -2px 15px rgba(0,0,0,0.1);border-top:1px solid #e2e8f0;z-index:1000;max-width:520px;margin:0 auto}
        .bottom-nav .nav-item{text-align:center;font-size:9px;color:#718096;cursor:pointer;padding:4px 8px;border-radius:10px;transition:0.3s}
        .bottom-nav .nav-item i{font-size:18px;display:block}
        .bottom-nav .nav-item.active{color:#1a365d;font-weight:700}
        .form-group{margin-bottom:8px}
        .form-group label{display:block;font-size:12px;font-weight:600;color:#2d3748;margin-bottom:3px}
        .form-group input,.form-group select{width:100%;padding:8px 10px;border:2px solid #e2e8f0;border-radius:10px;font-size:13px;background:#f7fafc}
        .form-group input:focus{border-color:#1a365d;outline:none;background:white}
        .btn{padding:8px 14px;border:none;border-radius:10px;font-size:13px;font-weight:600;cursor:pointer;transition:0.3s;width:100%;display:flex;align-items:center;justify-content:center;gap:5px}
        .btn:active{transform:scale(0.96)}
        .btn-primary{background:#1a365d;color:white}
        .btn-success{background:#38a169;color:white}
        .btn-danger{background:#e53e3e;color:white}
        .btn-warning{background:#d69e2e;color:white}
        .btn-gold{background:#d69e2e;color:white}
        .btn-outline{background:transparent;border:2px solid #1a365d;color:#1a365d}
        .grid-2{display:grid;grid-template-columns:1fr 1fr;gap:8px}
        .grid-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:6px}
        .table-wrap{overflow-x:auto;margin-top:6px}
        table{width:100%;border-collapse:collapse;font-size:11px}
        th{background:#1a365d;color:white;padding:5px 3px;text-align:center;font-size:10px}
        td{padding:5px 3px;border-bottom:1px solid #e2e8f0;text-align:center;font-size:11px}
        tr:nth-child(even){background:#f7fafc}
        .badge{display:inline-block;padding:2px 8px;border-radius:20px;font-size:9px;font-weight:600}
        .badge-success{background:#c6f6d5;color:#22543d}
        .badge-danger{background:#fed7d7;color:#9b2c2c}
        .badge-warning{background:#fefcbf;color:#744210}
        .badge-stone{background:#e8d5c4;color:#5d4037}
        .badge-material{background:#bee3f8;color:#2a4365}
        .highlight{padding:8px;border-radius:10px;border-right:4px solid #1a365d;background:#ebf8ff;margin:5px 0;font-size:12px}
        .total-bar{background:#1a365d;color:white;padding:10px;border-radius:12px;display:flex;justify-content:space-between;font-weight:700;font-size:15px;margin-top:8px}
        .cat-tabs{display:flex;gap:4px;margin-bottom:8px}
        .cat-tab{padding:6px 12px;border-radius:10px;font-weight:600;font-size:11px;cursor:pointer;background:#e2e8f0;color:#4a5568;border:none;flex:1}
        .cat-tab.active{color:white}
        .cat-tab.stone-active{background:#8B7355;color:white}
        .cat-tab.material-active{background:#2b6cb0;color:white}
        .sync-status{font-size:11px;color:#a0aec0;display:flex;align-items:center;gap:6px}
        .dot{width:10px;height:10px;border-radius:50%;display:inline-block}
        .dot.green{background:#48bb78}
        .dot.red{background:#fc8181}
        .dot.yellow{background:#ecc94b;animation:blink 1s infinite}
        @keyframes blink{0%,100%{opacity:1}50%{opacity:0.3}}
        @media(max-width:480px){.grid-2{grid-template-columns:1fr}.grid-3{grid-template-columns:1fr 1fr}.pos-grid{grid-template-columns:1fr 1fr}.header h1{font-size:15px}}
        .view-only .btn,.view-only .pos-item,.view-only input,.view-only select{opacity:0.5;pointer-events:none}
        .share-card{background:#f0fff4;border:2px dashed #38a169;border-radius:12px;padding:12px;text-align:center;margin-top:10px}
    </style>
</head>
<body>
<div class="app-container" id="app">
    <!-- Header -->
    <div class="header">
        <div>
            <h1><i class="fas fa-store"></i> <span id="shopName">Warda Marble Tabuk</span></h1>
            <span class="header-badge" id="modeBadge">👑 Admin</span>
        </div>
        <div style="display:flex;flex-wrap:wrap;gap:4px;align-items:center;">
            <span class="sync-status"><span class="dot green" id="statusDot"></span><span id="statusText">Online</span></span>
            <button class="mode-toggle" onclick="toggleMode()"><i class="fas fa-user-shield"></i> <span id="modeLabel">View</span></button>
            <button class="lang-toggle" onclick="toggleLang()"><i class="fas fa-language"></i> <span id="langLabel">اردو</span></button>
        </div>
    </div>

    <!-- Tabs -->
    <div class="tabs" id="tabHeaders">
        <div class="tab active" data-tab="pos"><i class="fas fa-shopping-cart"></i> POS</div>
        <div class="tab" data-tab="stock"><i class="fas fa-boxes"></i> Stock</div>
        <div class="tab" data-tab="customers"><i class="fas fa-users"></i> Cust</div>
        <div class="tab" data-tab="expenses"><i class="fas fa-wallet"></i> Exp</div>
        <div class="tab" data-tab="reports"><i class="fas fa-chart-pie"></i> Report</div>
    </div>

    <!-- ==================== POS ==================== -->
    <div id="section-pos" class="section active">
        <div class="card">
            <div class="card-title"><i class="fas fa-list"></i> Items <span style="margin-right:auto;"></span><span class="cat-badge cat-stone">Stone</span><span class="cat-badge cat-material">Material</span></div>
            <div class="cat-tabs">
                <button class="cat-tab stone-active active" onclick="filterCategory('stone')" id="catStone">🪨 Stone</button>
                <button class="cat-tab material-active" onclick="filterCategory('material')" id="catMaterial">🧱 Material</button>
                <button class="cat-tab" onclick="filterCategory('all')" id="catAll">📦 All</button>
            </div>
            <div class="pos-grid" id="posGrid"></div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-shopping-cart"></i> Cart (<span id="cartCount">0</span>)</div>
            <div id="cartItems"></div>
            <div class="total-bar"><span>Total:</span><span id="cartTotal">0</span></div>
            <div class="grid-2" style="margin-top:8px;">
                <select id="saleType" style="padding:8px;border-radius:10px;border:2px solid #e2e8f0;">
                    <option value="cash">💵 Cash</option>
                    <option value="credit">📝 Credit</option>
                </select>
                <select id="saleCustomerSelect" style="padding:8px;border-radius:10px;border:2px solid #e2e8f0;">
                    <option value="">👤 Walk-in</option>
                </select>
            </div>
            <div class="form-group" style="margin-top:6px;">
                <input type="number" id="salePaid" placeholder="Amount Paid" value="0" style="width:100%;padding:8px;border-radius:10px;border:2px solid #e2e8f0;">
            </div>
            <button class="btn btn-success" onclick="completeSale()"><i class="fas fa-check"></i> Complete Sale</button>
        </div>
    </div>

    <!-- ==================== STOCK ==================== -->
    <div id="section-stock" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-plus-circle"></i> Add Item</div>
            <div class="form-group"><label>Item Name</label><input type="text" id="itemName" placeholder="e.g. Atlantic Marble"></div>
            <div class="grid-2">
                <div class="form-group"><label>Category</label><select id="itemCategory"><option value="stone">🪨 Stone</option><option value="material">🧱 Material</option></select></div>
                <div class="form-group"><label>Unit</label><select id="itemUnit"><option value="m²">m²</option><option value="sqft">sq.ft</option><option value="running_ft">Running ft</option><option value="pcs">Pieces</option></select></div>
            </div>
            <div class="grid-3">
                <div class="form-group"><label>Length (ft)</label><input type="number" id="itemLength" placeholder="L" value="0"></div>
                <div class="form-group"><label>Width (ft)</label><input type="number" id="itemWidth" placeholder="W" value="0"></div>
                <div class="form-group"><label>Area (m²)</label><input type="number" id="itemArea" placeholder="Auto" step="0.01" readonly style="background:#edf2f7;"></div>
            </div>
            <div class="grid-2">
                <div class="form-group"><label>Purchase Price</label><input type="number" id="itemPurchasePrice" placeholder="Cost"></div>
                <div class="form-group"><label>Sell Price</label><input type="number" id="itemSellPrice" placeholder="Sell"></div>
            </div>
            <div class="grid-2">
                <div class="form-group"><label>Stock</label><input type="number" id="itemStock" placeholder="Qty"></div>
                <div class="form-group"><label>Min Stock</label><input type="number" id="itemMinStock" placeholder="Warning"></div>
            </div>
            <button class="btn btn-primary" onclick="addItem()"><i class="fas fa-save"></i> Add Item</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-list"></i> Stock List</div>
            <div id="stockListContainer"></div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-arrow-right"></i> Stock In / Out</div>
            <div class="grid-2">
                <select id="stockItemSelect" style="padding:8px;border-radius:10px;border:2px solid #e2e8f0;width:100%;"></select>
                <input type="number" id="stockQty" placeholder="Quantity" style="padding:8px;border-radius:10px;border:2px solid #e2e8f0;width:100%;">
            </div>
            <div class="grid-2">
                <button class="btn btn-success" onclick="stockIn()"><i class="fas fa-plus"></i> Stock In</button>
                <button class="btn btn-danger" onclick="stockOut()"><i class="fas fa-minus"></i> Stock Out</button>
            </div>
        </div>
    </div>

    <!-- ==================== CUSTOMERS ==================== -->
    <div id="section-customers" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-user-plus"></i> Add Customer</div>
            <div class="form-group"><input type="text" id="custName" placeholder="Customer Name"></div>
            <div class="form-group"><input type="text" id="custPhone" placeholder="Phone Number"></div>
            <div class="form-group"><input type="text" id="custAddress" placeholder="Address"></div>
            <button class="btn btn-primary" onclick="addCustomer()"><i class="fas fa-user-plus"></i> Add Customer</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-users"></i> All Customers</div>
            <div id="customerListContainer"></div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-hand-holding-usd"></i> Receive Payment</div>
            <select id="receiveCustomerSelect" style="width:100%;padding:8px;border-radius:10px;border:2px solid #e2e8f0;margin-bottom:8px;"></select>
            <input type="number" id="receiveAmount" placeholder="Amount Received" style="width:100%;padding:8px;border-radius:10px;border:2px solid #e2e8f0;margin-bottom:8px;">
            <button class="btn btn-success" onclick="receivePayment()"><i class="fas fa-money-bill-wave"></i> Receive</button>
        </div>
    </div>

    <!-- ==================== EXPENSES ==================== -->
    <div id="section-expenses" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-plus-circle"></i> Add Expense</div>
            <div class="form-group"><input type="text" id="expenseHead" placeholder="Expense Head (e.g. Electricity)"></div>
            <div class="form-group"><input type="number" id="expenseAmount" placeholder="Amount"></div>
            <div class="form-group"><input type="text" id="expenseNote" placeholder="Note (optional)"></div>
            <button class="btn btn-danger" onclick="addExpense()"><i class="fas fa-minus-circle"></i> Add Expense</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-list"></i> Today's Expenses</div>
            <div id="todayExpensesList"></div>
        </div>
    </div>

    <!-- ==================== REPORTS ==================== -->
    <div id="section-reports" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-calendar-day"></i> Reports</div>
            <div class="grid-3">
                <button class="btn btn-primary" onclick="generateReport('daily')"><i class="fas fa-eye"></i> Daily</button>
                <button class="btn btn-warning" onclick="generateReport('weekly')"><i class="fas fa-calendar-week"></i> Weekly</button>
                <button class="btn btn-gold" onclick="generateReport('monthly')"><i class="fas fa-calendar-alt"></i> Monthly</button>
            </div>
            <div style="margin-top:6px;display:flex;gap:4px;">
                <button class="btn btn-success" onclick="shareReportImage()" style="flex:1;"><i class="fas fa-image"></i> Share</button>
                <button class="btn btn-outline" onclick="sendWhatsAppReport()" style="flex:1;"><i class="fab fa-whatsapp"></i> WhatsApp</button>
            </div>
            <div id="reportDisplay" style="margin-top:10px;background:#f7fafc;padding:12px;border-radius:10px;font-size:12px;white-space:pre-wrap;font-family:monospace;"></div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-chart-line"></i> Profit / Loss</div>
            <div id="profitLossDisplay"></div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-history"></i> All Transactions</div>
            <div id="allTransactionsList"></div>
        </div>
    </div>
</div>

<!-- Bottom Nav -->
<div class="bottom-nav">
    <div class="nav-item active" data-tab="pos"><i class="fas fa-shopping-cart"></i> POS</div>
    <div class="nav-item" data-tab="stock"><i class="fas fa-boxes"></i> Stock</div>
    <div class="nav-item" data-tab="customers"><i class="fas fa-users"></i> Cust</div>
    <div class="nav-item" data-tab="expenses"><i class="fas fa-wallet"></i> Exp</div>
    <div class="nav-item" data-tab="reports"><i class="fas fa-chart-pie"></i> Report</div>
</div>

<canvas id="reportCanvas" style="display:none;"></canvas>

<!-- ============================================================ -->
<!-- 🔑 FIREBASE CONFIG -->
<!-- ============================================================ -->
<script>
    const firebaseConfig = {
        apiKey: "AIzaSyBacJhHOK-xZ9TgQfbN8swRNYIhmsIh0nk",
        authDomain: "warda-marble.firebaseapp.com",
        databaseURL: "https://warda-marble-default-rtdb.firebaseio.com",
        projectId: "warda-marble",
        storageBucket: "warda-marble.firebasestorage.app",
        messagingSenderId: "696018619200",
        appId: "1:696018619200:web:a4239b1f35ea7a98ad08a5"
    };

    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();

    // ============================================================
    // FULL APPLICATION CODE
    // ============================================================
    let db = {
        items: [],
        customers: [],
        sales: [],
        expenses: [],
        transactions: []
    };
    
    let cart = [];
    let currentFilter = 'all';
    let isAdmin = true;
    let currentLang = 'en';
    let isSyncing = false;

    function loadLocal() {
        const data = localStorage.getItem('warda_data');
        if (data) {
            const parsed = JSON.parse(data);
            db.items = parsed.items || [];
            db.customers = parsed.customers || [];
            db.sales = parsed.sales || [];
            db.expenses = parsed.expenses || [];
            db.transactions = parsed.transactions || [];
        }
    }
    loadLocal();

    function saveLocal() {
        localStorage.setItem('warda_data', JSON.stringify(db));
    }

    function getToday() {
        return new Date().toISOString().split('T')[0];
    }

    // Language
    const lang = {
        en: { shopName: 'Warda Marble Tabuk', cash: 'Cash', credit: 'Credit', walkin: 'Walk-in' },
        ur: { shopName: 'وردہ ماربل تبوک', cash: 'نقد', credit: 'ادھار', walkin: 'بغیر گاہک' }
    };

    function t(key) { return lang[currentLang][key] || key; }

    function toggleLang() {
        currentLang = currentLang === 'en' ? 'ur' : 'en';
        document.getElementById('langLabel').textContent = currentLang === 'en' ? 'اردو' : 'English';
        document.getElementById('shopName').textContent = t('shopName');
        renderAll();
    }

    function toggleMode() {
        isAdmin = !isAdmin;
        document.getElementById('modeLabel').textContent = isAdmin ? 'View' : 'Admin';
        document.getElementById('modeBadge').textContent = isAdmin ? '👑 Admin' : '👁️ Viewer';
        if (!isAdmin) {
            document.querySelectorAll('.btn, .pos-item, input, select').forEach(el => { 
                el.style.opacity = '0.5'; 
                el.style.pointerEvents = 'none'; 
            });
        } else {
            document.querySelectorAll('.btn, .pos-item, input, select').forEach(el => { 
                el.style.opacity = '1'; 
                el.style.pointerEvents = 'auto'; 
            });
        }
        renderAll();
    }

    // Sync Functions
    function updateStatus(status) {
        const dot = document.getElementById('statusDot');
        const text = document.getElementById('statusText');
        if (status === 'online') { dot.className = 'dot green'; text.textContent = 'Online'; }
        else if (status === 'offline') { dot.className = 'dot red'; text.textContent = 'Offline'; }
        else { dot.className = 'dot yellow'; text.textContent = 'Syncing...'; }
    }

    function syncToCloud() {
        if (isSyncing) return;
        isSyncing = true;
        updateStatus('syncing');
        const data = {
            items: db.items,
            customers: db.customers,
            sales: db.sales,
            expenses: db.expenses,
            transactions: db.transactions,
            updated: Date.now()
        };
        database.ref('warda_data').set(data).then(() => {
            updateStatus('online');
            isSyncing = false;
            showToast('✅ Synced to cloud!');
        }).catch(err => {
            updateStatus('offline');
            isSyncing = false;
            showToast('❌ Sync failed!');
            console.error(err);
        });
    }

    function syncFromCloud() {
        if (isSyncing) return;
        isSyncing = true;
        updateStatus('syncing');
        database.ref('warda_data').once('value').then(snapshot => {
            const data = snapshot.val();
            if (data) {
                db.items = data.items || [];
                db.customers = data.customers || [];
                db.sales = data.sales || [];
                db.expenses = data.expenses || [];
                db.transactions = data.transactions || [];
                saveLocal();
                renderAll();
                showToast('✅ Synced from cloud!');
            }
            updateStatus('online');
            isSyncing = false;
        }).catch(err => {
            updateStatus('offline');
            isSyncing = false;
            showToast('❌ Sync failed!');
            console.error(err);
        });
    }

    function showToast(msg) {
        const t = document.createElement('div');
        t.style.cssText = 'position:fixed;bottom:100px;left:50%;transform:translateX(-50%);background:#1a365d;color:white;padding:10px 20px;border-radius:12px;font-size:14px;z-index:9999;max-width:90%;text-align:center;box-shadow:0 4px 15px rgba(0,0,0,0.3)';
        t.textContent = msg;
        document.body.appendChild(t);
        setTimeout(() => { t.style.opacity = '0'; t.style.transition = 'opacity 0.3s'; setTimeout(() => t.remove(), 300); }, 3000);
    }

    // Calculate Area
    function calcArea() {
        const l = parseFloat(document.getElementById('itemLength').value) || 0;
        const w = parseFloat(document.getElementById('itemWidth').value) || 0;
        document.getElementById('itemArea').value = ((l * w) / 10.764).toFixed(2);
    }
    document.getElementById('itemLength').addEventListener('input', calcArea);
    document.getElementById('itemWidth').addEventListener('input', calcArea);

    // Items
    function addItem() {
        if (!isAdmin) return alert('Only Admin can add!');
        const name = document.getElementById('itemName').value.trim();
        if (!name) return alert('Enter name!');
        const item = {
            id: Date.now(),
            name,
            category: document.getElementById('itemCategory').value,
            unit: document.getElementById('itemUnit').value,
            purchasePrice: parseFloat(document.getElementById('itemPurchasePrice').value) || 0,
            sellPrice: parseFloat(document.getElementById('itemSellPrice').value) || 0,
            stock: parseFloat(document.getElementById('itemStock').value) || 0,
            minStock: parseFloat(document.getElementById('itemMinStock').value) || 0,
            length: parseFloat(document.getElementById('itemLength').value) || 0,
            width: parseFloat(document.getElementById('itemWidth').value) || 0,
            area: parseFloat(document.getElementById('itemArea').value) || 0
        };
        db.items.push(item);
        saveLocal();
        syncToCloud();
        renderAll();
        showToast('✅ Item added!');
    }

    function deleteItem(id) {
        if (!isAdmin) return alert('Only Admin can delete!');
        if (!confirm('Delete?')) return;
        db.items = db.items.filter(i => i.id !== id);
        saveLocal();
        syncToCloud();
        renderAll();
    }

    function editItem(id) {
        if (!isAdmin) return alert('Only Admin can edit!');
        const item = db.items.find(i => i.id === id);
        if (!item) return;
        const newPrice = prompt('New price for ' + item.name, item.sellPrice);
        if (newPrice && !isNaN(newPrice) && newPrice > 0) {
            item.sellPrice = parseFloat(newPrice);
            saveLocal();
            syncToCloud();
            renderAll();
        }
    }

    function stockIn() {
        if (!isAdmin) return alert('Only Admin!');
        const id = parseInt(document.getElementById('stockItemSelect').value);
        const qty = parseFloat(document.getElementById('stockQty').value);
        if (!id || !qty || qty <= 0) return alert('Select item and qty!');
        const item = db.items.find(i => i.id === id);
        if (!item) return;
        item.stock += qty;
        db.transactions.push({ id: Date.now(), type: 'stock_in', desc: `${item.name} +${qty}`, amount: qty * item.purchasePrice, date: getToday() });
        saveLocal();
        syncToCloud();
        renderAll();
        document.getElementById('stockQty').value = '';
        showToast(`✅ ${qty} added!`);
    }

    function stockOut() {
        if (!isAdmin) return alert('Only Admin!');
        const id = parseInt(document.getElementById('stockItemSelect').value);
        const qty = parseFloat(document.getElementById('stockQty').value);
        if (!id || !qty || qty <= 0) return alert('Select item and qty!');
        const item = db.items.find(i => i.id === id);
        if (!item) return;
        if (item.stock < qty) return alert('Not enough stock!');
        item.stock -= qty;
        db.transactions.push({ id: Date.now(), type: 'stock_out', desc: `${item.name} -${qty}`, amount: qty * item.sellPrice, date: getToday() });
        saveLocal();
        syncToCloud();
        renderAll();
        document.getElementById('stockQty').value = '';
        showToast(`✅ ${qty} removed!`);
    }

    function renderStockList() {
        const container = document.getElementById('stockListContainer');
        if (db.items.length === 0) { container.innerHTML = 'No items'; return; }
        let html = '<div class="table-wrap"><table><tr><th>Name</th><th>Cat</th><th>Unit</th><th>Stock</th><th>Price</th><th>Action</th></tr>';
        db.items.forEach(item => {
            const warn = item.stock < item.minStock ? '⚠️' : '';
            const catBadge = item.category === 'stone' ? '<span class="badge badge-stone">Stone</span>' : '<span class="badge badge-material">Material</span>';
            html += `<tr><td>${item.name}</td><td>${catBadge}</td><td>${item.unit}</td><td>${item.stock} ${warn}</td><td>${item.sellPrice}</td><td>
                <button onclick="editItem(${item.id})" style="background:#e2e8f0;border:none;padding:2px 6px;border-radius:6px;cursor:pointer;">✏️</button>
                <button onclick="deleteItem(${item.id})" style="background:#fed7d7;border:none;padding:2px 6px;border-radius:6px;cursor:pointer;">🗑️</button>
            </td></tr>`;
        });
        html += '</table></div>';
        container.innerHTML = html;

        const sel = document.getElementById('stockItemSelect');
        sel.innerHTML = '';
        db.items.forEach(i => { const o = document.createElement('option'); o.value = i.id; o.textContent = `${i.name} (${i.stock})`; sel.appendChild(o); });
    }

    // POS
    function filterCategory(cat) {
        currentFilter = cat;
        document.querySelectorAll('.cat-tab').forEach(el => el.classList.remove('active', 'stone-active', 'material-active'));
        if (cat === 'stone') { document.getElementById('catStone').classList.add('active', 'stone-active'); }
        else if (cat === 'material') { document.getElementById('catMaterial').classList.add('active', 'material-active'); }
        else { document.getElementById('catAll').classList.add('active'); }
        renderPOS();
    }

    function renderPOS() {
        const grid = document.getElementById('posGrid');
        let items = db.items;
        if (currentFilter !== 'all') items = items.filter(i => i.category === currentFilter);
        if (items.length === 0) { grid.innerHTML = '<div style="grid-column:1/-1;text-align:center;color:#718096;padding:20px;">No items</div>'; return; }
        let html = '';
        items.forEach(item => {
            const out = item.stock <= 0 ? 'out-of-stock' : '';
            html += `<div class="pos-item ${out}" onclick="${out ? '' : `addToCart(${item.id})`}" style="${out ? 'opacity:0.5;pointer-events:none;' : ''}">
                <div class="name">${item.name}</div>
                <div class="price">${item.sellPrice}</div>
                <div class="stock">${item.stock} ${item.unit}</div>
            </div>`;
        });
        grid.innerHTML = html;
    }

    // Cart
    function addToCart(id) {
        const item = db.items.find(i => i.id === id);
        if (!item || item.stock <= 0) return alert('Out of stock!');
        const existing = cart.find(c => c.id === id);
        if (existing) {
            if (existing.qty >= item.stock) return alert('Not enough stock!');
            existing.qty++;
        } else {
            cart.push({ id: item.id, name: item.name, price: item.sellPrice, qty: 1, unit: item.unit, category: item.category, purchasePrice: item.purchasePrice || 0 });
        }
        renderCart();
    }

    function removeFromCart(id) {
        cart = cart.filter(c => c.id !== id);
        renderCart();
    }

    function updateQty(id, delta) {
        const item = cart.find(c => c.id === id);
        if (!item) return;
        const stockItem = db.items.find(i => i.id === id);
        if (delta > 0 && item.qty >= stockItem.stock) return alert('Not enough stock!');
        item.qty += delta;
        if (item.qty <= 0) cart = cart.filter(c => c.id !== id);
        renderCart();
    }

    function renderCart() {
        const container = document.getElementById('cartItems');
        if (cart.length === 0) {
            container.innerHTML = '<div style="text-align:center;color:#718096;padding:15px;">Cart empty</div>';
            document.getElementById('cartTotal').textContent = '0';
            document.getElementById('cartCount').textContent = '0';
            return;
        }
        let html = '', total = 0;
        cart.forEach(c => {
            total += c.qty * c.price;
            html += `<div class="cart-item">
                <span><strong>${c.name}</strong> (${c.price} x ${c.qty})</span>
                <div class="qty-control">
                    <button onclick="updateQty(${c.id}, -1)">−</button>
                    <span>${c.qty}</span>
                    <button onclick="updateQty(${c.id}, 1)">+</button>
                    <button onclick="removeFromCart(${c.id})" style="background:#fed7d7;border:none;border-radius:50%;width:26px;height:26px;cursor:pointer;">🗑️</button>
                </div>
            </div>`;
        });
        container.innerHTML = html;
        document.getElementById('cartTotal').textContent = total;
        document.getElementById('cartCount').textContent = cart.reduce((s, c) => s + c.qty, 0);
    }

    // Complete Sale
    function completeSale() {
        if (!isAdmin) return alert('Only Admin can complete sales!');
        if (cart.length === 0) return alert('Cart empty!');
        const type = document.getElementById('saleType').value;
        const customerId = document.getElementById('saleCustomerSelect').value;
        const paid = parseFloat(document.getElementById('salePaid').value) || 0;
        let total = 0, stoneTotal = 0, materialTotal = 0, items = [];
        cart.forEach(c => {
            const item = db.items.find(i => i.id === c.id);
            if (!item || item.stock < c.qty) return alert(`${c.name} not enough stock!`);
            const subtotal = c.qty * c.price;
            total += subtotal;
            if (item.category === 'stone') stoneTotal += subtotal;
            else materialTotal += subtotal;
            items.push({ id: c.id, name: c.name, qty: c.qty, price: c.price, unit: c.unit, category: c.category, purchasePrice: c.purchasePrice });
            item.stock -= c.qty;
        });
        const remaining = total - paid;
        const sale = { id: Date.now(), items, total, paid, remaining, type, customerId: customerId || null, date: getToday(), stoneTotal, materialTotal };
        db.sales.push(sale);
        if (customerId && type === 'credit') {
            const cust = db.customers.find(c => c.id == customerId);
            if (cust) cust.balance += remaining;
        }
        let cost = 0;
        items.forEach(i => cost += i.qty * (i.purchasePrice || 0));
        db.transactions.push({ id: Date.now(), type: 'sale', desc: `${items.length} items (${type})`, amount: total, profit: total - cost, date: getToday() });
        cart = [];
        saveLocal();
        syncToCloud();
        renderAll();
        document.getElementById('salePaid').value = '0';
        showToast(`✅ Sale: ${total} | Profit: ${total - cost}`);
    }

    // Customers
    function addCustomer() {
        if (!isAdmin) return alert('Only Admin!');
        const name = document.getElementById('custName').value.trim();
        if (!name) return alert('Enter name!');
        db.customers.push({ id: Date.now(), name, phone: document.getElementById('custPhone').value, address: document.getElementById('custAddress').value, balance: 0 });
        saveLocal();
        syncToCloud();
        renderAll();
        showToast('✅ Customer added!');
    }

    function receivePayment() {
        if (!isAdmin) return alert('Only Admin!');
        const id = document.getElementById('receiveCustomerSelect').value;
        const amount = parseFloat(document.getElementById('receiveAmount').value);
        if (!id || !amount || amount <= 0) return alert('Select customer and amount!');
        const cust = db.customers.find(c => c.id == id);
        if (!cust || cust.balance < amount) return alert(`Balance: ${cust.balance}`);
        cust.balance -= amount;
        db.transactions.push({ id: Date.now(), type: 'receive', desc: `Received from ${cust.name}`, amount, date: getToday() });
        saveLocal();
        syncToCloud();
        renderAll();
        document.getElementById('receiveAmount').value = '';
        showToast(`✅ Received ${amount}!`);
    }

    function renderCustomers() {
        const container = document.getElementById('customerListContainer');
        if (db.customers.length === 0) { container.innerHTML = 'No customers'; } else {
            let html = '<div class="table-wrap"><table><tr><th>Name</th><th>Phone</th><th>Balance</th></tr>';
            db.customers.forEach(c => { html += `<tr><td>${c.name}</td><td>${c.phone || '-'}</td><td>${c.balance}</td></tr>`; });
            html += '</table></div>';
            container.innerHTML = html;
        }
        const sel = document.getElementById('saleCustomerSelect');
        const rsel = document.getElementById('receiveCustomerSelect');
        [sel, rsel].forEach(s => {
            if (!s) return;
            const val = s.value;
            s.innerHTML = '<option value="">👤 Walk-in</option>';
            db.customers.forEach(c => {
                const o = document.createElement('option');
                o.value = c.id;
                o.textContent = `${c.name} (${c.balance})`;
                s.appendChild(o);
            });
            if (val) s.value = val;
        });
    }

    // Expenses
    function addExpense() {
        if (!isAdmin) return alert('Only Admin!');
        const head = document.getElementById('expenseHead').value.trim();
        const amount = parseFloat(document.getElementById('expenseAmount').value);
        if (!head || !amount || amount <= 0) return alert('Enter head and amount!');
        db.expenses.push({ id: Date.now(), head, amount, note: document.getElementById('expenseNote').value, date: getToday() });
        db.transactions.push({ id: Date.now(), type: 'expense', desc: head, amount, date: getToday() });
        saveLocal();
        syncToCloud();
        renderAll();
        showToast('✅ Expense added!');
    }

    function renderTodayExpenses() {
        const container = document.getElementById('todayExpensesList');
        const today = getToday();
        const exp = db.expenses.filter(e => e.date === today);
        if (exp.length === 0) { container.innerHTML = 'No expenses today'; return; }
        let html = '<div class="table-wrap"><table><tr><th>Head</th><th>Amount</th></tr>';
        let total = 0;
        exp.forEach(e => { total += e.amount; html += `<tr><td>${e.head}</td><td>${e.amount}</td></tr>`; });
        html += `<tr style="font-weight:700"><td>Total</td><td>${total}</td></tr></table></div>`;
        container.innerHTML = html;
    }

    // Reports
    function generateReport(type) {
        const today = getToday();
        let sales = [], expenses = [];
        if (type === 'daily') { sales = db.sales.filter(s => s.date === today); expenses = db.expenses.filter(e => e.date === today); }
        else if (type === 'weekly') {
            const d = new Date(); d.setDate(d.getDate() - 7);
            const start = d.toISOString().split('T')[0];
            sales = db.sales.filter(s => s.date >= start); expenses = db.expenses.filter(e => e.date >= start);
        } else {
            const d = new Date(); d.setMonth(d.getMonth() - 1);
            const start = d.toISOString().split('T')[0];
            sales = db.sales.filter(s => s.date >= start); expenses = db.expenses.filter(e => e.date >= start);
        }
        const totalSales = sales.reduce((s, i) => s + i.total, 0);
        const totalExp = expenses.reduce((s, i) => s + i.amount, 0);
        let stone = 0, material = 0, stoneCost = 0, materialCost = 0;
        sales.forEach(s => {
            s.items.forEach(i => {
                if (i.category === 'stone') { stone += i.qty * i.price; stoneCost += i.qty * (i.purchasePrice || 0); }
                else { material += i.qty * i.price; materialCost += i.qty * (i.purchasePrice || 0); }
            });
        });
        const profit = (stone + material) - (stoneCost + materialCost) - totalExp;
        const report = `📊 ${type.toUpperCase()} REPORT - ${today}\n━━━━━━━━━━━━━━━━━━━━━\n💰 Total Sales: ${totalSales}\n💸 Expenses: ${totalExp}\n🪨 Stone Sales: ${stone}\n🧱 Material Sales: ${material}\n📈 Profit: ${profit >= 0 ? '✅ ' + profit : '❌ ' + Math.abs(profit)}\n━━━━━━━━━━━━━━━━━━━━━`;
        document.getElementById('reportDisplay').textContent = report;
        document.getElementById('profitLossDisplay').innerHTML = `
            <div style="text-align:center;padding:15px;background:${profit>=0?'#f0fff4':'#fff5f5'};border-radius:12px;">
                <div style="font-size:24px;font-weight:700;color:${profit>=0?'#38a169':'#e53e3e'};">${profit >= 0 ? '✅ Profit: ' + profit : '❌ Loss: ' + Math.abs(profit)}</div>
                <div style="font-size:12px;color:#718096;">Stone: ${stone - stoneCost} | Material: ${material - materialCost}</div>
            </div>
        `;
        renderTransactions();
        return report;
    }

    function renderTransactions() {
        const container = document.getElementById('allTransactionsList');
        if (db.transactions.length === 0) { container.innerHTML = 'No transactions'; return; }
        let html = '<div class="table-wrap"><table><tr><th>Date</th><th>Desc</th><th>Amount</th></tr>';
        const sorted = [...db.transactions].reverse().slice(0, 20);
        sorted.forEach(t => {
            const color = t.type === 'expense' ? '#e53e3e' : '#38a169';
            html += `<tr><td>${t.date}</td><td>${t.desc}</td><td style="color:${color}">${t.amount}</td></tr>`;
        });
        html += '</table></div>';
        container.innerHTML = html;
    }

    function shareReportImage() {
        const report = document.getElementById('reportDisplay').textContent;
        if (!report) return alert('Generate report first!');
        const canvas = document.getElementById('reportCanvas');
        const ctx = canvas.getContext('2d');
        canvas.width = 600; canvas.height = 500;
        const g = ctx.createLinearGradient(0,0,0,canvas.height);
        g.addColorStop(0, '#1a365d'); g.addColorStop(1, '#2d3748');
        ctx.fillStyle = g; ctx.fillRect(0,0,canvas.width,canvas.height);
        ctx.fillStyle = '#fff'; ctx.font = 'bold 24px Arial'; ctx.textAlign = 'center';
        ctx.fillText('🏪 Warda Marble', canvas.width/2, 50);
        ctx.fillStyle = '#d69e2e'; ctx.font = '16px monospace'; ctx.textAlign = 'left';
        const lines = report.split('\n'); let y = 90;
        lines.forEach(line => { if(line.trim()) { ctx.fillText(line, 30, y); y += 26; } else y += 12; });
        ctx.fillStyle = '#d69e2e'; ctx.font = '12px Arial'; ctx.textAlign = 'center';
        ctx.fillText(new Date().toLocaleString(), canvas.width/2, canvas.height-20);
        const link = document.createElement('a');
        link.download = 'report.png';
        link.href = canvas.toDataURL('image/png');
        link.click();
        showToast('📷 Image downloaded!');
    }

    function sendWhatsAppReport() {
        const report = document.getElementById('reportDisplay').textContent;
        if (!report) return alert('Generate report first!');
        const phone = '923001234567';
        window.open(`https://wa.me/${phone}?text=${encodeURIComponent(report)}`, '_blank');
    }

    // Render All
    function renderAll() {
        renderPOS();
        renderCart();
        renderStockList();
        renderCustomers();
        renderTodayExpenses();
        renderTransactions();
        if (document.getElementById('section-reports').classList.contains('active')) generateReport('daily');
    }

    // Navigation
    document.querySelectorAll('.tab').forEach(tab => {
        tab.addEventListener('click', function() {
            const id = this.dataset.tab;
            document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
            this.classList.add('active');
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById('section-' + id).classList.add('active');
            document.querySelectorAll('.bottom-nav .nav-item').forEach(n => n.classList.remove('active'));
            document.querySelector(`.bottom-nav .nav-item[data-tab="${id}"]`).classList.add('active');
            if (id === 'pos') { renderPOS(); renderCart(); }
            if (id === 'stock') renderStockList();
            if (id === 'customers') renderCustomers();
            if (id === 'expenses') renderTodayExpenses();
            if (id === 'reports') { generateReport('daily'); renderTransactions(); }
        });
    });

    document.querySelectorAll('.bottom-nav .nav-item').forEach(item => {
        item.addEventListener('click', function() {
            const id = this.dataset.tab;
            document.querySelectorAll('.bottom-nav .nav-item').forEach(n => n.classList.remove('active'));
            this.classList.add('active');
            document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
            document.querySelector(`.tab[data-tab="${id}"]`).classList.add('active');
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById('section-' + id).classList.add('active');
            if (id === 'pos') { renderPOS(); renderCart(); }
            if (id === 'stock') renderStockList();
            if (id === 'customers') renderCustomers();
            if (id === 'expenses') renderTodayExpenses();
            if (id === 'reports') { generateReport('daily'); renderTransactions(); }
        });
    });

    // Init - Sample items from your system
    if (db.items.length === 0) {
        db.items = [
            // Materials
            { id: 1, name: 'زاویہ اپ ڈاون 4*4*3mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 9200, minStock: 100, length: 0, width: 0, area: 0 },
            { id: 2, name: 'زاویہ اپ ڈاون 4*4*2mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 6600, minStock: 100, length: 0, width: 0, area: 0 },
            { id: 3, name: 'زاویہ اپ ڈاون 4*3*3mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 4300, minStock: 100, length: 0, width: 0, area: 0 },
            { id: 4, name: 'زاویہ اپ ڈاون 4*3*2mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 2500, minStock: 100, length: 0, width: 0, area: 0 },
            { id: 5, name: 'زاویہ اپ ڈاون 4*5*3mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 1200, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 6, name: 'زاویہ اپ ڈاون 4*6*3mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 850, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 7, name: 'زاویہ اپ ڈاون 4*8*3mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 420, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 8, name: 'زاویہ اپ ڈاون 4*10*3mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 400, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 9, name: 'مسمار مغلوب 10*40', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 4800, minStock: 100, length: 0, width: 0, area: 0 },
            { id: 10, name: 'مسمار مغلوب 10*50', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 4800, minStock: 100, length: 0, width: 0, area: 0 },
            { id: 11, name: 'مسمار مغلوب 10*60', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 3500, minStock: 100, length: 0, width: 0, area: 0 },
            { id: 12, name: 'مسمار مغلوب 10*80', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 1500, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 13, name: 'مسمار مغلوب 10*100', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 1440, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 14, name: 'براغی سقف 10*72', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 2200, minStock: 100, length: 0, width: 0, area: 0 },
            { id: 15, name: 'براغی سقف 10*92', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 2100, minStock: 100, length: 0, width: 0, area: 0 },
            { id: 16, name: 'براغی سقف 10*112', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 1920, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 17, name: 'براغی سقف 10*115', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 2200, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 18, name: 'براغی سقف 10*202', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 1040, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 19, name: 'جولی ابو جمل Standard', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 5, minStock: 2, length: 0, width: 0, area: 0 },
            { id: 20, name: 'جولی گلیڈتیر Standard', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 0, minStock: 2, length: 0, width: 0, area: 0 },
            { id: 21, name: 'دسک حجر احمر 7 بوسہ', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 0, minStock: 2, length: 0, width: 0, area: 0 },
            { id: 22, name: 'دسک حجر احمر 4.50 بوسہ', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 7, minStock: 2, length: 0, width: 0, area: 0 },
            { id: 23, name: 'دسک جرانیت 7 بوسہ', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 3, minStock: 2, length: 0, width: 0, area: 0 },
            { id: 24, name: 'دسک بوش 9 بوسہ', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 20, minStock: 5, length: 0, width: 0, area: 0 },
            { id: 25, name: 'ریشہ ہلتی 20cm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 0, minStock: 5, length: 0, width: 0, area: 0 },
            { id: 26, name: 'ریشہ ہلتی 15cm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 24, minStock: 5, length: 0, width: 0, area: 0 },
            { id: 27, name: 'ریشہ ہلتی 10cm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 1, minStock: 5, length: 0, width: 0, area: 0 },
            { id: 28, name: 'ریشہ دریل 4mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 12, minStock: 5, length: 0, width: 0, area: 0 },
            { id: 29, name: 'ریشہ دریل 6mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 10, minStock: 5, length: 0, width: 0, area: 0 },
            { id: 30, name: 'ریشہ دریل 10mm', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 16, minStock: 5, length: 0, width: 0, area: 0 },
            { id: 31, name: 'گمتا چوڑی Standard', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 7, minStock: 2, length: 0, width: 0, area: 0 },
            { id: 32, name: 'گمتا سادہ Standard', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 7, minStock: 2, length: 0, width: 0, area: 0 },
            { id: 33, name: 'بکرا Standard', category: 'material', unit: 'pcs', purchasePrice: 0, sellPrice: 0, stock: 1, minStock: 2, length: 0, width: 0, area: 0 },
            // Polished Marble
            { id: 34, name: 'ابیض مجلی مبروم 3*7', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 35, name: 'ابیض مجلی مبروم 3*10', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 36, name: 'ابیض مجلی مبروم 3*15', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 37, name: 'ابیض مجلی مبروم 3*18', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 38, name: 'ابیض مجلی مبروم 3*20', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 39, name: 'ابیض مجلی مبروم 3*25', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 40, name: 'ابیض مجلی مبروم 3*27', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 41, name: 'ابیض مجلی مبروم 3*30', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 42, name: 'ابیض مجلی مبروم 3*32', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 43, name: 'ابیض مجلی مبروم 3*35', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 44, name: 'ابیض مجلی مبروم 3*37', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 45, name: 'ابیض مجلی مبروم 3*40', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 46, name: 'ابیض مجلی مبروم 3*50', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 47, name: 'ابیض مجلی مبروم 3*60', category: 'stone', unit: 'm²', purchasePrice: 50, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 48, name: 'ابیض مجلی سادہ 3*7', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 49, name: 'ابیض مجلی سادہ 3*10', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 50, name: 'ابیض مجلی سادہ 3*15', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 51, name: 'ابیض مجلی سادہ 3*18', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 52, name: 'ابیض مجلی سادہ 3*20', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 53, name: 'ابیض مجلی سادہ 3*25', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 54, name: 'ابیض مجلی سادہ 3*27', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 55, name: 'ابیض مجلی سادہ 3*30', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 56, name: 'ابیض مجلی سادہ 3*32', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 57, name: 'ابیض مجلی سادہ 3*35', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 58, name: 'ابیض مجلی سادہ 3*37', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 59, name: 'ابیض مجلی سادہ 3*40', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 60, name: 'ابیض مجلی سادہ 3*50', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 61, name: 'ابیض مجلی سادہ 3*60', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 60, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 62, name: '(A) ابیض مجلی فرزا 3*40*80', category: 'stone', unit: 'm²', purchasePrice: 63, sellPrice: 75, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 63, name: '(C) ابیض مجلی فرزا 3*40*60', category: 'stone', unit: 'm²', purchasePrice: 63, sellPrice: 75, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 64, name: 'ابیض مجلی مشطوف 3*30', category: 'stone', unit: 'm²', purchasePrice: 58, sellPrice: 70, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 65, name: 'ابیض بشارتہ فرزا 3*40*80', category: 'stone', unit: 'm²', purchasePrice: 73, sellPrice: 85, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 66, name: 'MNHOOD ABIAZ 3*20*1.20', category: 'stone', unit: 'm²', purchasePrice: 53, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            // Honed Marble
            { id: 67, name: 'ابیض مصنفر مبروم 3*7', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 68, name: 'ابیض مصنفر مبروم 3*10', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 69, name: 'ابیض مصنفر مبروم 3*15', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 70, name: 'ابیض مصنفر مبروم 3*18', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 71, name: 'ابیض مصنفر مبروم 3*20', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 72, name: 'ابیض مصنفر مبروم 3*25', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 73, name: 'ابیض مصنفر مبروم 3*27', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 74, name: 'ابیض مصنفر مبروم 3*30', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 75, name: 'ابیض مصنفر مبروم 3*32', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 76, name: 'ابیض مصنفر مبروم 3*35', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 77, name: 'ابیض مصنفر مبروم 3*37', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 78, name: 'ابیض مصنفر مبروم 3*40', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 79, name: 'ابیض مصنفر مبروم 3*50', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 80, name: 'ابیض مصنفر مبروم 3*60', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 81, name: 'ابیض مصنفر مبروم 4*7', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 82, name: 'ابیض مصنفر مبروم 5*7', category: 'stone', unit: 'm²', purchasePrice: 42, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 83, name: 'ابیض مصنفر سادہ 3*7', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 84, name: 'ابیض مصنفر سادہ 3*10', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 85, name: 'ابیض مصنفر سادہ 3*15', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 86, name: 'ابیض مصنفر سادہ 3*18', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 87, name: 'ابیض مصنفر سادہ 3*20', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 88, name: 'ابیض مصنفر سادہ 3*25', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 89, name: 'ابیض مصنفر سادہ 3*27', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 90, name: 'ابیض مصنفر سادہ 3*30', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 91, name: 'ابیض مصنفر سادہ 3*32', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 92, name: 'ابیض مصنفر سادہ 3*35', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 93, name: 'ابیض مصنفر سادہ 3*37', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 94, name: 'ابیض مصنفر سادہ 3*40', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 95, name: 'ابیض مصنفر سادہ 3*50', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 96, name: 'ابیض مصنفر سادہ 3*60', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 97, name: 'ابیض مصنفر سادہ 4*7', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 98, name: 'ابیض مصنفر سادہ 5*7', category: 'stone', unit: 'm²', purchasePrice: 38, sellPrice: 50, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 99, name: 'ابیض مصنفر فرزا 3*40*80', category: 'stone', unit: 'm²', purchasePrice: 55, sellPrice: 65, stock: 50, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 100, name: 'پیانو ابیض مصنفر 3*20*50', category: 'stone', unit: 'm²', purchasePrice: 45, sellPrice: 55, stock: 50, minStock: 10, length: 0, width: 0, area: 0 }
        ];
        saveLocal();
        syncToCloud();
    }

    renderAll();
    syncFromCloud();

    setInterval(() => {
        if (navigator.onLine) syncFromCloud();
        else updateStatus('offline');
    }, 30000);

    console.log('🚀 Warda Marble Tabuk Ready!');
    console.log('📦 Items:', db.items.length);
</script>
</body>
</html>
