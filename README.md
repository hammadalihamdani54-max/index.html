<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>Marble POS Pro - Cloud Sync</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
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
            --gold: #d69e2e;
            --stone: #8B7355;
            --material: #2b6cb0;
            --bg: #f0f4f8;
            --card: #ffffff;
        }
        body {
            background: var(--bg);
            padding: 10px;
            padding-bottom: 80px;
        }
        .app-container {
            max-width: 520px;
            margin: 0 auto;
            background: var(--card);
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            overflow: hidden;
            padding: 15px;
        }
        .header {
            background: linear-gradient(135deg, #1a365d, #2d3748);
            color: white;
            padding: 12px 18px;
            border-radius: 15px;
            margin-bottom: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }
        .header h1 {
            font-size: 17px;
            font-weight: 700;
        }
        .header-badge {
            background: rgba(255,255,255,0.2);
            padding: 3px 10px;
            border-radius: 20px;
            font-size: 10px;
        }
        .cloud-status {
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 11px;
        }
        .cloud-status .dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            display: inline-block;
        }
        .dot.online { background: #48bb78; }
        .dot.offline { background: #fc8181; }
        .dot.syncing { background: #ecc94b; animation: blink 1s infinite; }
        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.3; }
        }
        .lang-toggle, .mode-toggle, .sync-btn {
            background: rgba(255,255,255,0.15);
            border: none;
            color: white;
            padding: 4px 10px;
            border-radius: 20px;
            cursor: pointer;
            font-weight: 600;
            font-size: 11px;
            margin: 2px;
        }
        .lang-toggle:active, .mode-toggle:active, .sync-btn:active {
            transform: scale(0.95);
        }
        .card {
            background: white;
            border-radius: 15px;
            padding: 12px;
            margin-bottom: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.06);
            border: 1px solid #e2e8f0;
        }
        .card-title {
            font-size: 14px;
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 6px;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 6px;
        }
        .card-title .cat-badge {
            font-size: 9px;
            padding: 2px 8px;
            border-radius: 20px;
            color: white;
        }
        .cat-stone { background: var(--stone); }
        .cat-material { background: var(--material); }
        .pos-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 6px;
        }
        .pos-item {
            background: #f7fafc;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            padding: 8px;
            text-align: center;
            cursor: pointer;
            transition: 0.3s;
            font-size: 11px;
        }
        .pos-item:active {
            transform: scale(0.95);
            background: #edf2f7;
        }
        .pos-item .name {
            font-weight: 600;
            color: var(--secondary);
            font-size: 12px;
        }
        .pos-item .price {
            color: var(--primary);
            font-weight: 700;
            font-size: 13px;
        }
        .pos-item .stock {
            font-size: 9px;
            color: #718096;
        }
        .pos-item .unit-badge {
            font-size: 8px;
            background: #e2e8f0;
            padding: 1px 6px;
            border-radius: 10px;
        }
        .pos-item.out-of-stock {
            opacity: 0.5;
            pointer-events: none;
        }
        .cart-item {
            display: flex;
            justify-content: space-between;
            padding: 5px 0;
            border-bottom: 1px solid #e2e8f0;
            font-size: 12px;
            align-items: center;
        }
        .cart-item .qty-control {
            display: flex;
            gap: 3px;
            align-items: center;
        }
        .cart-item .qty-control button {
            background: #e2e8f0;
            border: none;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            font-weight: 700;
            cursor: pointer;
            font-size: 13px;
        }
        .cart-item .qty-control button:active {
            transform: scale(0.9);
        }
        .btn-sm {
            padding: 3px 8px;
            border: none;
            border-radius: 8px;
            font-size: 10px;
            font-weight: 600;
            cursor: pointer;
        }
        .btn-danger-sm {
            background: #fed7d7;
            color: #9b2c2c;
        }
        .tabs {
            display: flex;
            gap: 3px;
            margin-bottom: 10px;
            flex-wrap: wrap;
            background: #f7fafc;
            padding: 4px;
            border-radius: 12px;
        }
        .tab {
            flex: 1;
            padding: 5px 3px;
            text-align: center;
            background: transparent;
            border-radius: 10px;
            font-weight: 600;
            font-size: 8px;
            cursor: pointer;
            transition: 0.3s;
            min-width: 38px;
            color: #4a5568;
        }
        .tab i {
            display: block;
            font-size: 13px;
            margin-bottom: 1px;
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
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            display: flex;
            justify-content: space-around;
            padding: 4px 0;
            box-shadow: 0 -2px 15px rgba(0,0,0,0.1);
            border-top: 1px solid #e2e8f0;
            z-index: 1000;
            max-width: 520px;
            margin: 0 auto;
        }
        .bottom-nav .nav-item {
            text-align: center;
            font-size: 7px;
            color: #718096;
            cursor: pointer;
            padding: 3px 4px;
            border-radius: 10px;
            transition: 0.3s;
        }
        .bottom-nav .nav-item i {
            font-size: 15px;
            display: block;
        }
        .bottom-nav .nav-item.active {
            color: var(--primary);
            font-weight: 700;
        }
        .form-group {
            margin-bottom: 6px;
        }
        .form-group label {
            display: block;
            font-size: 11px;
            font-weight: 600;
            color: var(--secondary);
            margin-bottom: 2px;
        }
        .form-group input, .form-group select {
            width: 100%;
            padding: 6px 8px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            font-size: 12px;
            background: #f7fafc;
        }
        .form-group input:focus {
            border-color: var(--primary);
            outline: none;
            background: white;
        }
        .btn {
            padding: 7px 12px;
            border: none;
            border-radius: 10px;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.3s;
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 4px;
        }
        .btn:active {
            transform: scale(0.96);
        }
        .btn-primary { background: var(--primary); color: white; }
        .btn-success { background: var(--success); color: white; }
        .btn-danger { background: var(--danger); color: white; }
        .btn-warning { background: var(--warning); color: white; }
        .btn-gold { background: var(--gold); color: white; }
        .btn-outline { background: transparent; border: 2px solid var(--primary); color: var(--primary); }
        .grid-2 {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 6px;
        }
        .grid-3 {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 5px;
        }
        .table-wrap {
            overflow-x: auto;
            margin-top: 5px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 10px;
        }
        th {
            background: var(--primary);
            color: white;
            padding: 4px 3px;
            text-align: center;
            font-size: 9px;
        }
        td {
            padding: 4px 3px;
            border-bottom: 1px solid #e2e8f0;
            text-align: center;
            font-size: 10px;
        }
        tr:nth-child(even) { background: #f7fafc; }
        .badge {
            display: inline-block;
            padding: 1px 6px;
            border-radius: 20px;
            font-size: 8px;
            font-weight: 600;
        }
        .badge-success { background: #c6f6d5; color: #22543d; }
        .badge-danger { background: #fed7d7; color: #9b2c2c; }
        .badge-warning { background: #fefcbf; color: #744210; }
        .badge-stone { background: #e8d5c4; color: #5d4037; }
        .badge-material { background: #bee3f8; color: #2a4365; }
        .highlight {
            padding: 6px;
            border-radius: 10px;
            border-right: 4px solid var(--primary);
            background: #ebf8ff;
            margin: 4px 0;
            font-size: 11px;
        }
        .total-bar {
            background: var(--primary);
            color: white;
            padding: 8px;
            border-radius: 12px;
            display: flex;
            justify-content: space-between;
            font-weight: 700;
            font-size: 14px;
            margin-top: 6px;
        }
        .cat-tabs {
            display: flex;
            gap: 3px;
            margin-bottom: 6px;
        }
        .cat-tab {
            padding: 4px 8px;
            border-radius: 10px;
            font-weight: 600;
            font-size: 10px;
            cursor: pointer;
            background: #e2e8f0;
            color: #4a5568;
            border: none;
            flex: 1;
        }
        .cat-tab.active {
            color: white;
        }
        .cat-tab.stone-active { background: var(--stone); color: white; }
        .cat-tab.material-active { background: var(--material); color: white; }
        .view-only {
            opacity: 0.7;
            pointer-events: none;
        }
        .view-only .btn, .view-only .pos-item {
            opacity: 0.5;
            pointer-events: none;
        }
        .sync-progress {
            background: #edf2f7;
            padding: 4px 8px;
            border-radius: 10px;
            font-size: 10px;
            color: #4a5568;
            text-align: center;
        }
        @media (max-width: 480px) {
            .grid-2 { grid-template-columns: 1fr; }
            .grid-3 { grid-template-columns: 1fr 1fr; }
            .pos-grid { grid-template-columns: 1fr 1fr; }
            .header h1 { font-size: 14px; }
        }
    </style>
</head>
<body>
<div class="app-container" id="app">
    <!-- Header -->
    <div class="header">
        <div>
            <h1><i class="fas fa-cloud"></i> <span id="shopName">Marble POS</span></h1>
            <span class="header-badge" id="modeBadge">👑 Admin</span>
        </div>
        <div style="display:flex;flex-wrap:wrap;gap:4px;align-items:center;">
            <span class="cloud-status">
                <span class="dot online" id="cloudDot"></span>
                <span id="cloudStatusText">Online</span>
            </span>
            <button class="sync-btn" onclick="syncNow()"><i class="fas fa-sync"></i></button>
            <button class="mode-toggle" onclick="toggleMode()"><i class="fas fa-user-shield"></i> <span id="modeLabel">View</span></button>
            <button class="lang-toggle" onclick="toggleLang()"><i class="fas fa-language"></i> <span id="langLabel">اردو</span></button>
        </div>
    </div>

    <!-- Sync Progress -->
    <div id="syncProgress" class="sync-progress" style="display:none;">🔄 Syncing...</div>

    <!-- Tabs -->
    <div class="tabs" id="tabHeaders">
        <div class="tab active" data-tab="pos"><i class="fas fa-shopping-cart"></i> POS</div>
        <div class="tab" data-tab="stock"><i class="fas fa-boxes"></i> Stock</div>
        <div class="tab" data-tab="customers"><i class="fas fa-users"></i> Cust</div>
        <div class="tab" data-tab="expenses"><i class="fas fa-wallet"></i> Exp</div>
        <div class="tab" data-tab="reports"><i class="fas fa-chart-pie"></i> Report</div>
    </div>

    <!-- ==================== POS SECTION ==================== -->
    <div id="section-pos" class="section active">
        <div class="card">
            <div class="card-title">
                <i class="fas fa-list"></i> Items
                <span style="margin-right:auto;"></span>
                <span class="cat-badge cat-stone" style="font-size:9px;">Stone</span>
                <span class="cat-badge cat-material" style="font-size:9px;">Material</span>
            </div>
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
            <div class="total-bar">
                <span>Total:</span>
                <span id="cartTotal">0</span>
            </div>
            <div class="grid-2" style="margin-top:6px;">
                <select id="saleType" style="padding:6px;border-radius:10px;border:2px solid #e2e8f0;">
                    <option value="cash">💵 Cash</option>
                    <option value="credit">📝 Credit</option>
                </select>
                <select id="saleCustomerSelect" style="padding:6px;border-radius:10px;border:2px solid #e2e8f0;">
                    <option value="">👤 Walk-in</option>
                </select>
            </div>
            <div class="form-group" style="margin-top:4px;">
                <input type="number" id="salePaid" placeholder="Amount Paid" value="0" style="width:100%;padding:6px;border-radius:10px;border:2px solid #e2e8f0;">
            </div>
            <button class="btn btn-success" onclick="completeSale()"><i class="fas fa-check"></i> Complete Sale</button>
        </div>
    </div>

    <!-- ==================== STOCK SECTION ==================== -->
    <div id="section-stock" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-plus-circle"></i> Add Item</div>
            <div class="form-group">
                <label>Item Name</label>
                <input type="text" id="itemName" placeholder="e.g. Atlantic Marble">
            </div>
            <div class="grid-2">
                <div class="form-group">
                    <label>Category</label>
                    <select id="itemCategory">
                        <option value="stone">🪨 Stone (m²)</option>
                        <option value="material">🧱 Material (pcs)</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Unit</label>
                    <select id="itemUnit">
                        <option value="m²">m²</option>
                        <option value="sqft">sq.ft</option>
                        <option value="running_ft">Running ft</option>
                        <option value="pcs">Pieces</option>
                    </select>
                </div>
            </div>
            <div class="grid-3" id="dimensionFields">
                <div class="form-group">
                    <label>Length (ft)</label>
                    <input type="number" id="itemLength" placeholder="L" value="0">
                </div>
                <div class="form-group">
                    <label>Width (ft)</label>
                    <input type="number" id="itemWidth" placeholder="W" value="0">
                </div>
                <div class="form-group">
                    <label>Area (m²)</label>
                    <input type="number" id="itemArea" placeholder="Auto" step="0.01" readonly style="background:#edf2f7;">
                </div>
            </div>
            <div class="grid-2">
                <div class="form-group">
                    <label>Purchase Price</label>
                    <input type="number" id="itemPurchasePrice" placeholder="Cost Price">
                </div>
                <div class="form-group">
                    <label>Sell Price</label>
                    <input type="number" id="itemSellPrice" placeholder="Sell Price">
                </div>
            </div>
            <div class="grid-2">
                <div class="form-group">
                    <label>Stock</label>
                    <input type="number" id="itemStock" placeholder="Quantity">
                </div>
                <div class="form-group">
                    <label>Min Stock</label>
                    <input type="number" id="itemMinStock" placeholder="Warning Level">
                </div>
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
                <div class="form-group">
                    <select id="stockItemSelect" style="padding:6px;border-radius:10px;border:2px solid #e2e8f0;width:100%;"></select>
                </div>
                <div class="form-group">
                    <input type="number" id="stockQty" placeholder="Quantity" style="padding:6px;border-radius:10px;border:2px solid #e2e8f0;width:100%;">
                </div>
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
            <div class="form-group">
                <input type="text" id="custName" placeholder="Customer Name">
            </div>
            <div class="form-group">
                <input type="text" id="custPhone" placeholder="Phone Number (for search)">
            </div>
            <div class="form-group">
                <input type="text" id="custAddress" placeholder="Address">
            </div>
            <button class="btn btn-primary" onclick="addCustomer()"><i class="fas fa-user-plus"></i> Add Customer</button>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-users"></i> All Customers</div>
            <div id="customerListContainer"></div>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-hand-holding-usd"></i> Receive Payment</div>
            <select id="receiveCustomerSelect" style="width:100%;padding:6px;border-radius:10px;border:2px solid #e2e8f0;margin-bottom:6px;"></select>
            <input type="number" id="receiveAmount" placeholder="Amount Received" style="width:100%;padding:6px;border-radius:10px;border:2px solid #e2e8f0;margin-bottom:6px;">
            <button class="btn btn-success" onclick="receivePayment()"><i class="fas fa-money-bill-wave"></i> Receive</button>
        </div>
    </div>

    <!-- ==================== EXPENSES ==================== -->
    <div id="section-expenses" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-plus-circle"></i> Add Expense</div>
            <div class="form-group">
                <input type="text" id="expenseHead" placeholder="Expense Head (e.g. Electricity)">
            </div>
            <div class="form-group">
                <input type="number" id="expenseAmount" placeholder="Amount">
            </div>
            <div class="form-group">
                <input type="text" id="expenseNote" placeholder="Note (optional)">
            </div>
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
            <div style="margin-top:4px;display:flex;gap:4px;">
                <button class="btn btn-success" onclick="shareReportImage()" style="flex:1;"><i class="fas fa-image"></i> Share</button>
                <button class="btn btn-outline" onclick="sendWhatsAppReport()" style="flex:1;"><i class="fab fa-whatsapp"></i> WhatsApp</button>
            </div>
            <div id="reportDisplay" style="margin-top:8px;background:#f7fafc;padding:10px;border-radius:10px;font-size:11px;white-space:pre-wrap;font-family:monospace;"></div>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-chart-line"></i> Profit / Loss</div>
            <div class="grid-2" id="profitLossDisplay">
                <div class="stat-box" style="background:#f0fff4;padding:8px;border-radius:10px;text-align:center;">
                    <div style="font-size:18px;font-weight:700;color:#38a169;" id="plStone">0</div>
                    <div style="font-size:10px;color:#718096;">🪨 Stone</div>
                </div>
                <div class="stat-box" style="background:#ebf8ff;padding:8px;border-radius:10px;text-align:center;">
                    <div style="font-size:18px;font-weight:700;color:#2b6cb0;" id="plMaterial">0</div>
                    <div style="font-size:10px;color:#718096;">🧱 Material</div>
                </div>
                <div class="stat-box" style="background:#fefcbf;padding:8px;border-radius:10px;text-align:center;grid-column:1/-1;">
                    <div style="font-size:22px;font-weight:700;color:#744210;" id="plTotal">0</div>
                    <div style="font-size:10px;color:#718096;">📊 Total Profit / Loss</div>
                </div>
            </div>
        </div>

        <div class="card">
            <div class="card-title"><i class="fas fa-history"></i> All Transactions</div>
            <div id="allTransactionsList"></div>
        </div>
    </div>
</div>

<!-- Bottom Navigation -->
<div class="bottom-nav">
    <div class="nav-item active" data-tab="pos"><i class="fas fa-shopping-cart"></i> POS</div>
    <div class="nav-item" data-tab="stock"><i class="fas fa-boxes"></i> Stock</div>
    <div class="nav-item" data-tab="customers"><i class="fas fa-users"></i> Cust</div>
    <div class="nav-item" data-tab="expenses"><i class="fas fa-wallet"></i> Exp</div>
    <div class="nav-item" data-tab="reports"><i class="fas fa-chart-pie"></i> Report</div>
</div>

<!-- Canvas for image sharing -->
<canvas id="reportCanvas" style="display:none;"></canvas>

<script>
    // ============================================================
    // FIREBASE CONFIGURATION
    // ============================================================
    // 🔑 IMPORTANT: Replace these with your own Firebase config
    // Go to Firebase Console > Project Settings > Your apps > Config
    const firebaseConfig = {
        apiKey: "AIzaSyDummyKeyReplaceWithYourOwn",  // Replace with your API Key
        authDomain: "your-project-id.firebaseapp.com",  // Replace
        databaseURL: "https://your-project-id-default-rtdb.firebaseio.com",  // Replace
        projectId: "your-project-id",  // Replace
        storageBucket: "your-project-id.appspot.com",  // Replace
        messagingSenderId: "123456789",  // Replace
        appId: "1:123456789:web:abcdef"  // Replace
    };

    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();

    // ============================================================
    // CLOUD SYNC SYSTEM
    // ============================================================
    let isOnline = false;
    let syncInProgress = false;
    let lastSyncTime = null;

    function updateCloudStatus(status) {
        const dot = document.getElementById('cloudDot');
        const text = document.getElementById('cloudStatusText');
        if (status === 'online') {
            dot.className = 'dot online';
            text.textContent = 'Online';
        } else if (status === 'offline') {
            dot.className = 'dot offline';
            text.textContent = 'Offline';
        } else if (status === 'syncing') {
            dot.className = 'dot syncing';
            text.textContent = 'Syncing...';
        }
        isOnline = status === 'online' || status === 'syncing';
    }

    function showSyncProgress(show) {
        document.getElementById('syncProgress').style.display = show ? 'block' : 'none';
    }

    // ============================================================
    // SYNC TO CLOUD
    // ============================================================
    function syncToCloud() {
        if (!isOnline) {
            alert('⚠️ No internet connection! Please connect and try again.');
            return;
        }

        syncInProgress = true;
        updateCloudStatus('syncing');
        showSyncProgress(true);

        const data = {
            items: db.items,
            customers: db.customers,
            sales: db.sales,
            expenses: db.expenses,
            transactions: db.transactions,
            lastUpdated: Date.now()
        };

        database.ref('marble_pos_data').set(data)
            .then(() => {
                console.log('✅ Data synced to cloud successfully!');
                updateCloudStatus('online');
                showSyncProgress(false);
                syncInProgress = false;
                lastSyncTime = Date.now();
                showNotification('✅ Data synced to cloud!');
            })
            .catch((error) => {
                console.error('❌ Sync failed:', error);
                updateCloudStatus('offline');
                showSyncProgress(false);
                syncInProgress = false;
                showNotification('❌ Sync failed! Check internet.');
            });
    }

    // ============================================================
    // SYNC FROM CLOUD
    // ============================================================
    function syncFromCloud() {
        if (!isOnline) {
            alert('⚠️ No internet connection! Please connect and try again.');
            return;
        }

        syncInProgress = true;
        updateCloudStatus('syncing');
        showSyncProgress(true);

        database.ref('marble_pos_data').once('value')
            .then((snapshot) => {
                const cloudData = snapshot.val();
                if (cloudData) {
                    // Merge cloud data with local
                    // Compare timestamps - keep latest
                    const localTime = localStorage.getItem('marble_last_update') || 0;
                    const cloudTime = cloudData.lastUpdated || 0;

                    if (cloudTime > localTime) {
                        // Cloud is newer - update local
                        db.items = cloudData.items || [];
                        db.customers = cloudData.customers || [];
                        db.sales = cloudData.sales || [];
                        db.expenses = cloudData.expenses || [];
                        db.transactions = cloudData.transactions || [];
                        localStorage.setItem('marble_last_update', cloudTime);
                        saveDB();
                        renderAll();
                        showNotification('✅ Data synced from cloud!');
                    } else {
                        // Local is newer - push to cloud
                        syncToCloud();
                    }
                } else {
                    // No data in cloud - push local
                    syncToCloud();
                }
                updateCloudStatus('online');
                showSyncProgress(false);
                syncInProgress = false;
            })
            .catch((error) => {
                console.error('❌ Sync from cloud failed:', error);
                updateCloudStatus('offline');
                showSyncProgress(false);
                syncInProgress = false;
                showNotification('❌ Sync failed! Check internet.');
            });
    }

    // ============================================================
    // SYNC NOW (Manual)
    // ============================================================
    function syncNow() {
        if (syncInProgress) {
            showNotification('⏳ Sync already in progress...');
            return;
        }
        syncFromCloud();
    }

    // ============================================================
    // AUTO SYNC (Every 30 seconds when online)
    // ============================================================
    function autoSync() {
        if (isOnline && !syncInProgress) {
            syncFromCloud();
        }
    }

    // ============================================================
    // CHECK INTERNET CONNECTION
    // ============================================================
    function checkInternet() {
        if (navigator.onLine) {
            updateCloudStatus('online');
            if (!lastSyncTime || (Date.now() - lastSyncTime) > 30000) {
                syncFromCloud();
            }
        } else {
            updateCloudStatus('offline');
        }
    }

    window.addEventListener('online', checkInternet);
    window.addEventListener('offline', () => updateCloudStatus('offline'));

    // ============================================================
    // NOTIFICATION SYSTEM
    // ============================================================
    function showNotification(msg) {
        const existing = document.querySelector('.notification-toast');
        if (existing) existing.remove();

        const toast = document.createElement('div');
        toast.className = 'notification-toast';
        toast.style.cssText = `
            position: fixed; bottom: 80px; left: 50%; transform: translateX(-50%);
            background: #1a365d; color: white; padding: 10px 20px;
            border-radius: 12px; font-size: 13px; font-weight: 600;
            z-index: 9999; max-width: 90%; text-align: center;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
            animation: slideUp 0.3s ease;
        `;
        toast.textContent = msg;
        document.body.appendChild(toast);

        setTimeout(() => {
            toast.style.opacity = '0';
            toast.style.transition = 'opacity 0.3s';
            setTimeout(() => toast.remove(), 300);
        }, 3000);
    }

    // Add animation
    const style = document.createElement('style');
    style.textContent = `
        @keyframes slideUp {
            from { opacity: 0; transform: translateX(-50%) translateY(20px); }
            to { opacity: 1; transform: translateX(-50%) translateY(0); }
        }
    `;
    document.head.appendChild(style);

    // ============================================================
    // LANGUAGE SYSTEM
    // ============================================================
    let currentLang = 'en';
    let currentCategory = 'all';
    let isAdmin = true;
    let cart = [];
    let currentFilter = 'all';

    const translations = {
        en: {
            shopName: 'Marble POS Pro',
            pos: 'POS', stock: 'Stock', customers: 'Cust', expenses: 'Exp', reports: 'Report',
            addItem: 'Add Item', addCustomer: 'Add Customer', addExpense: 'Add Expense',
            receive: 'Receive', daily: 'Daily', weekly: 'Weekly', monthly: 'Monthly',
            whatsapp: 'WhatsApp', shareImage: 'Share Image', total: 'Total',
            cash: 'Cash', credit: 'Credit', walkin: 'Walk-in', amountPaid: 'Amount Paid',
            itemName: 'Item Name', category: 'Category', unit: 'Unit',
            purchasePrice: 'Purchase Price', sellPrice: 'Sell Price',
            stock: 'Stock', minStock: 'Min Stock', customerName: 'Customer Name',
            phone: 'Phone', address: 'Address', expenseHead: 'Expense Head',
            amount: 'Amount', note: 'Note', receivePayment: 'Receive Payment',
            length: 'Length (ft)', width: 'Width (ft)', area: 'Area (m²)',
            stone: 'Stone', material: 'Material', all: 'All',
            completeSale: 'Complete Sale', stockIn: 'Stock In', stockOut: 'Stock Out'
        },
        ur: {
            shopName: 'ماربل پی او ایس',
            pos: 'فروخت', stock: 'اسٹاک', customers: 'گاہک', expenses: 'اخراجات', reports: 'رپورٹس',
            addItem: 'آئٹم شامل کریں', addCustomer: 'گاہک شامل کریں', addExpense: 'خرچ شامل کریں',
            receive: 'وصول کریں', daily: 'روزانہ', weekly: 'ہفتہ وار', monthly: 'ماہانہ',
            whatsapp: 'واٹس ایپ', shareImage: 'تصویر شیئر کریں', total: 'کل',
            cash: 'نقد', credit: 'ادھار', walkin: 'بغیر گاہک', amountPaid: 'ادا کردہ رقم',
            itemName: 'آئٹم کا نام', category: 'قسم', unit: 'یونٹ',
            purchasePrice: 'خرید قیمت', sellPrice: 'فروخت قیمت',
            stock: 'اسٹاک', minStock: 'کم از کم اسٹاک', customerName: 'گاہک کا نام',
            phone: 'فون نمبر', address: 'پتہ', expenseHead: 'خرچ کا عنوان',
            amount: 'رقم', note: 'تفصیل', receivePayment: 'ادھار وصولی',
            length: 'لمبائی (فٹ)', width: 'چوڑائی (فٹ)', area: 'رقبہ (m²)',
            stone: 'حجر', material: 'مواد', all: 'تمام',
            completeSale: 'فروخت مکمل کریں', stockIn: 'اسٹاک شامل کریں', stockOut: 'اسٹاک نکالیں'
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
        document.getElementById('shopName').textContent = t('shopName');
        document.querySelectorAll('input[placeholder]').forEach(el => {
            const key = el.placeholder.toLowerCase().replace(/[^a-z]/g, '');
            const map = {
                'egatlanticmarble': 'itemName',
                'sellprice': 'sellPrice',
                'costprice': 'purchasePrice',
                'quantity': 'stock',
                'warninglevel': 'minStock',
                'customername': 'customerName',
                'phonenumberforsearch': 'phone',
                'address': 'address',
                'expenseheadegelectricity': 'expenseHead',
                'amount': 'amount',
                'noteoptional': 'note',
                'amountpaid': 'amountPaid',
                'amountreceived': 'amountPaid',
                'lengthft': 'length',
                'widthft': 'width',
                'areaauto': 'area'
            };
            const translated = t(map[key] || key);
            if (translated) el.placeholder = translated;
        });
        document.querySelectorAll('option').forEach(el => {
            if (el.value === 'cash') el.textContent = '💵 ' + t('cash');
            if (el.value === 'credit') el.textContent = '📝 ' + t('credit');
            if (el.value === '') el.textContent = '👤 ' + t('walkin');
            if (el.value === 'stone') el.textContent = '🪨 ' + t('stone') + ' (m²)';
            if (el.value === 'material') el.textContent = '🧱 ' + t('material') + ' (pcs)';
        });
    }

    // ============================================================
    // MODE (Admin / View)
    // ============================================================
    function toggleMode() {
        isAdmin = !isAdmin;
        document.getElementById('modeLabel').textContent = isAdmin ? 'View' : 'Admin';
        document.getElementById('modeBadge').textContent = isAdmin ? '👑 Admin' : '👁️ Viewer';
        if (!isAdmin) {
            document.querySelectorAll('.btn, .pos-item, .form-group input, .form-group select').forEach(el => {
                el.style.opacity = '0.6';
                el.style.pointerEvents = 'none';
            });
        } else {
            document.querySelectorAll('.btn, .pos-item, .form-group input, .form-group select').forEach(el => {
                el.style.opacity = '1';
                el.style.pointerEvents = 'auto';
            });
        }
        renderAll();
    }

    // ============================================================
    // DATABASE (localStorage)
    // ============================================================
    let db = {
        items: JSON.parse(localStorage.getItem('marble_items')) || [],
        customers: JSON.parse(localStorage.getItem('marble_customers')) || [],
        sales: JSON.parse(localStorage.getItem('marble_sales')) || [],
        expenses: JSON.parse(localStorage.getItem('marble_expenses')) || [],
        transactions: JSON.parse(localStorage.getItem('marble_transactions')) || [],
        stockHistory: JSON.parse(localStorage.getItem('marble_stock_history')) || []
    };

    function saveDB() {
        localStorage.setItem('marble_items', JSON.stringify(db.items));
        localStorage.setItem('marble_customers', JSON.stringify(db.customers));
        localStorage.setItem('marble_sales', JSON.stringify(db.sales));
        localStorage.setItem('marble_expenses', JSON.stringify(db.expenses));
        localStorage.setItem('marble_transactions', JSON.stringify(db.transactions));
        localStorage.setItem('marble_stock_history', JSON.stringify(db.stockHistory));
        localStorage.setItem('marble_last_update', Date.now());
    }

    // ============================================================
    // AUTO AREA CALCULATION
    // ============================================================
    function calculateArea() {
        const length = parseFloat(document.getElementById('itemLength').value) || 0;
        const width = parseFloat(document.getElementById('itemWidth').value) || 0;
        const area = (length * width) / 10.764;
        document.getElementById('itemArea').value = area.toFixed(2);
        return area;
    }

    document.getElementById('itemLength').addEventListener('input', calculateArea);
    document.getElementById('itemWidth').addEventListener('input', calculateArea);

    // ============================================================
    // ITEMS (Stock Management)
    // ============================================================
    function addItem() {
        if (!isAdmin) return alert('Only Admin can add items!');
        const name = document.getElementById('itemName').value.trim();
        const category = document.getElementById('itemCategory').value;
        const unit = document.getElementById('itemUnit').value;
        const purchasePrice = parseFloat(document.getElementById('itemPurchasePrice').value);
        const sellPrice = parseFloat(document.getElementById('itemSellPrice').value);
        const stock = parseFloat(document.getElementById('itemStock').value) || 0;
        const minStock = parseFloat(document.getElementById('itemMinStock').value) || 0;
        const length = parseFloat(document.getElementById('itemLength').value) || 0;
        const width = parseFloat(document.getElementById('itemWidth').value) || 0;
        const area = parseFloat(document.getElementById('itemArea').value) || 0;

        if (!name || !sellPrice || sellPrice <= 0) return alert('Please enter name and sell price');
        if (db.items.find(i => i.name.toLowerCase() === name.toLowerCase())) return alert('Item already exists!');

        db.items.push({
            id: Date.now(),
            name,
            category,
            unit,
            purchasePrice: purchasePrice || 0,
            sellPrice,
            stock,
            minStock,
            length,
            width,
            area: area || 0
        });

        if (stock > 0) {
            db.transactions.push({
                id: Date.now(),
                type: 'stock_in',
                desc: `Stock In: ${name} x ${stock} ${unit}`,
                amount: stock * purchasePrice,
                date: getToday()
            });
        }

        saveDB();
        renderAll();
        document.getElementById('itemName').value = '';
        document.getElementById('itemPurchasePrice').value = '';
        document.getElementById('itemSellPrice').value = '';
        document.getElementById('itemStock').value = '';
        document.getElementById('itemMinStock').value = '';
        document.getElementById('itemLength').value = '0';
        document.getElementById('itemWidth').value = '0';
        document.getElementById('itemArea').value = '';
        showNotification('✅ Item added successfully!');
        // Sync to cloud
        if (isOnline) syncToCloud();
    }

    function deleteItem(id) {
        if (!isAdmin) return alert('Only Admin can delete!');
        if (!confirm('Delete this item?')) return;
        db.items = db.items.filter(i => i.id !== id);
        saveDB();
        renderAll();
        if (isOnline) syncToCloud();
    }

    function editItem(id) {
        if (!isAdmin) return alert('Only Admin can edit!');
        const item = db.items.find(i => i.id === id);
        if (!item) return;
        const newPrice = prompt('Enter new sell price for ' + item.name, item.sellPrice);
        if (newPrice !== null && !isNaN(newPrice) && newPrice > 0) {
            item.sellPrice = parseFloat(newPrice);
            saveDB();
            renderAll();
            if (isOnline) syncToCloud();
        }
    }

    function stockIn() {
        if (!isAdmin) return alert('Only Admin can manage stock!');
        const itemId = parseInt(document.getElementById('stockItemSelect').value);
        const qty = parseFloat(document.getElementById('stockQty').value);
        if (!itemId || !qty || qty <= 0) return alert('Select item and quantity');
        const item = db.items.find(i => i.id === itemId);
        if (!item) return alert('Item not found');
        item.stock += qty;
        db.transactions.push({
            id: Date.now(),
            type: 'stock_in',
            desc: `Stock In: ${item.name} +${qty} ${item.unit}`,
            amount: qty * item.purchasePrice,
            date: getToday()
        });
        saveDB();
        renderAll();
        document.getElementById('stockQty').value = '';
        showNotification(`✅ ${qty} ${item.unit} added to ${item.name}`);
        if (isOnline) syncToCloud();
    }

    function stockOut() {
        if (!isAdmin) return alert('Only Admin can manage stock!');
        const itemId = parseInt(document.getElementById('stockItemSelect').value);
        const qty = parseFloat(document.getElementById('stockQty').value);
        if (!itemId || !qty || qty <= 0) return alert('Select item and quantity');
        const item = db.items.find(i => i.id === itemId);
        if (!item) return alert('Item not found');
        if (item.stock < qty) return alert(`Not enough stock! Available: ${item.stock}`);
        item.stock -= qty;
        db.transactions.push({
            id: Date.now(),
            type: 'stock_out',
            desc: `Stock Out: ${item.name} -${qty} ${item.unit}`,
            amount: qty * item.sellPrice,
            date: getToday()
        });
        saveDB();
        renderAll();
        document.getElementById('stockQty').value = '';
        showNotification(`✅ ${qty} ${item.unit} removed from ${item.name}`);
        if (isOnline) syncToCloud();
    }

    function renderStockList() {
        const container = document.getElementById('stockListContainer');
        if (db.items.length === 0) {
            container.innerHTML = '<div class="list-item">No items</div>';
            return;
        }
        let html = `<div class="table-wrap"><table>
            <tr><th>Name</th><th>Cat</th><th>Unit</th><th>Stock</th><th>Price</th><th>Action</th></tr>`;
        db.items.forEach(item => {
            const warn = item.stock < item.minStock ? '⚠️' : '';
            const catBadge = item.category === 'stone' ? 
                '<span class="badge badge-stone">Stone</span>' : 
                '<span class="badge badge-material">Material</span>';
            html += `<tr>
                <td>${item.name}</td>
                <td>${catBadge}</td>
                <td>${item.unit}</td>
                <td>${item.stock} ${warn}</td>
                <td>${item.sellPrice}</td>
                <td>
                    <button class="btn-sm btn-danger-sm" onclick="deleteItem(${item.id})"><i class="fas fa-trash"></i></button>
                    <button class="btn-sm" style="background:#e2e8f0;" onclick="editItem(${item.id})"><i class="fas fa-edit"></i></button>
                </td>
            </tr>`;
        });
        html += '</table></div>';
        container.innerHTML = html;

        const sel = document.getElementById('stockItemSelect');
        sel.innerHTML = '';
        db.items.forEach(item => {
            const opt = document.createElement('option');
            opt.value = item.id;
            opt.textContent = `${item.name} (${item.stock} ${item.unit})`;
            sel.appendChild(opt);
        });
    }

    // ============================================================
    // POS GRID with Category Filter
    // ============================================================
    function filterCategory(cat) {
        currentFilter = cat;
        document.querySelectorAll('.cat-tab').forEach(el => el.classList.remove('active', 'stone-active', 'material-active'));
        if (cat === 'stone') {
            document.getElementById('catStone').classList.add('active', 'stone-active');
        } else if (cat === 'material') {
            document.getElementById('catMaterial').classList.add('active', 'material-active');
        } else {
            document.getElementById('catAll').classList.add('active');
        }
        renderPOS();
    }

    function renderPOS() {
        const grid = document.getElementById('posGrid');
        let items = db.items;
        if (currentFilter !== 'all') {
            items = items.filter(i => i.category === currentFilter);
        }
        if (items.length === 0) {
            grid.innerHTML = '<div style="grid-column:1/-1;text-align:center;color:#718096;padding:15px;">No items found</div>';
            return;
        }
        let html = '';
        items.forEach(item => {
            const outOfStock = item.stock <= 0;
            const catIcon = item.category === 'stone' ? '🪨' : '🧱';
            html += `<div class="pos-item ${outOfStock ? 'out-of-stock' : ''}" onclick="${outOfStock ? '' : `addToCart(${item.id})`}">
                <div class="name">${catIcon} ${item.name}</div>
                <div class="price">${item.sellPrice}</div>
                <div class="stock">${item.stock} ${item.unit} <span class="unit-badge">${item.category}</span></div>
                ${item.area > 0 ? `<div style="font-size:8px;color:#718096;">${item.area.toFixed(2)} m²</div>` : ''}
            </div>`;
        });
        grid.innerHTML = html;
    }

    // ============================================================
    // CART SYSTEM
    // ============================================================
    function addToCart(itemId) {
        const item = db.items.find(i => i.id === itemId);
        if (!item) return;
        if (item.stock <= 0) return alert('Out of stock!');
        const existing = cart.find(c => c.itemId === itemId);
        if (existing) {
            if (existing.qty >= item.stock) return alert('Not enough stock!');
            existing.qty++;
        } else {
            cart.push({ 
                itemId, 
                qty: 1, 
                name: item.name, 
                price: item.sellPrice, 
                unit: item.unit,
                category: item.category,
                purchasePrice: item.purchasePrice || 0
            });
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
            container.innerHTML = '<div style="text-align:center;color:#718096;padding:12px;">Cart is empty</div>';
            document.getElementById('cartTotal').textContent = '0';
            document.getElementById('cartCount').textContent = '0';
            return;
        }
        let html = '';
        let total = 0;
        cart.forEach(c => {
            const subtotal = c.qty * c.price;
            total += subtotal;
            const catIcon = c.category === 'stone' ? '🪨' : '🧱';
            html += `<div class="cart-item">
                <span><strong>${catIcon} ${c.name}</strong> (${c.price} x ${c.qty})</span>
                <div class="qty-control">
                    <button onclick="updateCartQty(${c.itemId}, -1)">−</button>
                    <span style="min-width:18px;text-align:center;">${c.qty}</span>
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
    // COMPLETE SALE
    // ============================================================
    function completeSale() {
        if (!isAdmin) return alert('Only Admin can complete sales!');
        if (cart.length === 0) return alert('Cart is empty!');
        const type = document.getElementById('saleType').value;
        const customerId = document.getElementById('saleCustomerSelect').value;
        const paid = parseFloat(document.getElementById('salePaid').value) || 0;

        let total = 0;
        let saleItems = [];
        let stoneTotal = 0;
        let materialTotal = 0;

        cart.forEach(c => {
            const item = db.items.find(i => i.id === c.itemId);
            if (!item) return;
            if (item.stock < c.qty) return alert(`${item.name} has only ${item.stock} in stock!`);
            const subtotal = c.qty * c.price;
            total += subtotal;
            if (item.category === 'stone') stoneTotal += subtotal;
            else materialTotal += subtotal;
            saleItems.push({ 
                itemId: c.itemId, 
                name: item.name, 
                qty: c.qty, 
                price: c.price, 
                unit: item.unit,
                category: item.category,
                purchasePrice: item.purchasePrice || 0
            });
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
            timestamp: Date.now(),
            stoneTotal,
            materialTotal
        };

        db.sales.push(sale);

        if (customerId && type === 'credit') {
            const cust = db.customers.find(c => c.id == customerId);
            if (cust) cust.balance += remaining;
        }

        let totalCost = 0;
        saleItems.forEach(item => {
            totalCost += item.qty * (item.purchasePrice || 0);
        });
        const profit = total - totalCost;

        db.transactions.push({
            id: Date.now(),
            type: 'sale',
            desc: `${saleItems.length} items (${type})`,
            amount: total,
            profit: profit,
            stoneTotal: stoneTotal,
            materialTotal: materialTotal,
            date: getToday()
        });

        cart = [];
        saveDB();
        renderAll();
        document.getElementById('salePaid').value = '0';
        showNotification(`✅ Sale Complete! Total: ${total} | Profit: ${profit}`);
        if (isOnline) syncToCloud();
    }

    // ============================================================
    // CUSTOMERS
    // ============================================================
    function addCustomer() {
        if (!isAdmin) return alert('Only Admin can add customers!');
        const name = document.getElementById('custName').value.trim();
        const phone = document.getElementById('custPhone').value.trim();
        const address = document.getElementById('custAddress').value.trim();
        if (!name) return alert('Enter customer name');
        
        if (phone) {
            const existing = db.customers.find(c => c.phone === phone);
            if (existing) {
                if (!confirm(`Customer "${existing.name}" already exists. Add as new?`)) return;
            }
        }

        db.customers.push({ 
            id: Date.now(), 
            name, 
            phone, 
            address, 
            balance: 0,
            history: []
        });
        saveDB();
        renderAll();
        document.getElementById('custName').value = '';
        document.getElementById('custPhone').value = '';
        document.getElementById('custAddress').value = '';
        showNotification('✅ Customer added!');
        if (isOnline) syncToCloud();
    }

    function renderCustomers() {
        const container = document.getElementById('customerListContainer');
        if (db.customers.length === 0) {
            container.innerHTML = '<div class="list-item">No customers</div>';
        } else {
            let html = '<div class="table-wrap"><table><tr><th>Name</th><th>Phone</th><th>Balance</th><th>History</th></tr>';
            db.customers.forEach(c => {
                const saleCount = db.sales.filter(s => s.customerId == c.id).length;
                html += `<tr>
                    <td>${c.name}</td>
                    <td>${c.phone || '-'}</td>
                    <td>${c.balance}</td>
                    <td><button class="btn-sm" style="background:#e2e8f0;" onclick="viewCustomerHistory(${c.id})"><i class="fas fa-eye"></i></button></td>
                </tr>`;
            });
            html += '</table></div>';
            container.innerHTML = html;
        }

        ['saleCustomerSelect', 'receiveCustomerSelect'].forEach(id => {
            const sel = document.getElementById(id);
            if (!sel) return;
            const currentVal = sel.value;
            sel.innerHTML = '<option value="">👤 Walk-in</option>';
            db.customers.forEach(c => {
                const opt = document.createElement('option');
                opt.value = c.id;
                opt.textContent = `${c.name} (${c.phone || 'no phone'}) - ${c.balance}`;
                sel.appendChild(opt);
            });
            if (currentVal) sel.value = currentVal;
        });
    }

    function viewCustomerHistory(customerId) {
        const cust = db.customers.find(c => c.id === customerId);
        if (!cust) return;
        const sales = db.sales.filter(s => s.customerId == customerId);
        let history = `📋 History for ${cust.name}\n`;
        history += `Phone: ${cust.phone || 'N/A'}\n`;
        history += `Total Balance: ${cust.balance}\n`;
        history += `━━━━━━━━━━━━━━━━━━━\n`;
        if (sales.length === 0) {
            history += 'No sales yet';
        } else {
            sales.forEach(s => {
                history += `${s.date} | ${s.items.length} items | Total: ${s.total} | Paid: ${s.paid}\n`;
            });
        }
        alert(history);
    }

    function receivePayment() {
        if (!isAdmin) return alert('Only Admin can receive payments!');
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
        showNotification(`✅ Received ${amount}! New balance: ${cust.balance}`);
        if (isOnline) syncToCloud();
    }

    // ============================================================
    // EXPENSES
    // ============================================================
    function addExpense() {
        if (!isAdmin) return alert('Only Admin can add expenses!');
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
        showNotification('✅ Expense added!');
        if (isOnline) syncToCloud();
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

        let stoneSales = 0, materialSales = 0;
        let stoneCost = 0, materialCost = 0;
        sales.forEach(s => {
            s.items.forEach(item => {
                const subtotal = item.qty * item.price;
                const cost = item.qty * (item.purchasePrice || 0);
                if (item.category === 'stone') {
                    stoneSales += subtotal;
                    stoneCost += cost;
                } else {
                    materialSales += subtotal;
                    materialCost += cost;
                }
            });
        });

        const stoneProfit = stoneSales - stoneCost;
        const materialProfit = materialSales - materialCost;
        const totalProfit = (stoneSales + materialSales) - (stoneCost + materialCost) - totalExpenses;

        const report = `
📊 ${type.toUpperCase()} REPORT - ${today}
━━━━━━━━━━━━━━━━━━━━━━━━━
💰 SALES
   Total Sales: ${totalSales}
   Amount Paid: ${totalPaid}
   Remaining: ${totalRemaining}
   Total Bills: ${sales.length}

🪨 STONE SALES: ${stoneSales}
🧱 MATERIAL SALES: ${materialSales}

💸 EXPENSES
   Total: ${totalExpenses}

📈 PROFIT / LOSS
   🪨 Stone Profit: ${stoneProfit}
   🧱 Material Profit: ${materialProfit}
   📊 Total: ${totalProfit >= 0 ? '✅ Profit: ' + totalProfit : '❌ Loss: ' + Math.abs(totalProfit)}
━━━━━━━━━━━━━━━━━━━━━━━━━
🏪 Marble POS Pro
        `;

        document.getElementById('reportDisplay').textContent = report;

        document.getElementById('plStone').textContent = stoneProfit >= 0 ? `+${stoneProfit}` : `${stoneProfit}`;
        document.getElementById('plStone').style.color = stoneProfit >= 0 ? '#38a169' : '#e53e3e';
        document.getElementById('plMaterial').textContent = materialProfit >= 0 ? `+${materialProfit}` : `${materialProfit}`;
        document.getElementById('plMaterial').style.color = materialProfit >= 0 ? '#38a169' : '#e53e3e';
        document.getElementById('plTotal').textContent = totalProfit >= 0 ? `+${totalProfit}` : `${totalProfit}`;
        document.getElementById('plTotal').style.color = totalProfit >= 0 ? '#38a169' : '#e53e3e';

        renderAllTransactions();
        return report;
    }

    // ============================================================
    // SHARE REPORT AS IMAGE
    // ============================================================
    function shareReportImage() {
        const reportText = document.getElementById('reportDisplay').textContent;
        if (!reportText || reportText.trim() === '') {
            alert('Generate report first!');
            return;
        }

        const canvas = document.getElementById('reportCanvas');
        const ctx = canvas.getContext('2d');
        canvas.width = 600;
        canvas.height = 650;

        const gradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
        gradient.addColorStop(0, '#1a365d');
        gradient.addColorStop(1, '#2d3748');
        ctx.fillStyle = gradient;
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        ctx.strokeStyle = '#d69e2e';
        ctx.lineWidth = 4;
        ctx.strokeRect(10, 10, canvas.width - 20, canvas.height - 20);

        ctx.fillStyle = '#ffffff';
        ctx.font = 'bold 26px Arial';
        ctx.textAlign = 'center';
        ctx.fillText('🏪 Marble POS Pro', canvas.width/2, 55);

        ctx.strokeStyle = '#d69e2e';
        ctx.lineWidth = 2;
        ctx.beginPath();
        ctx.moveTo(50, 75);
        ctx.lineTo(canvas.width - 50, 75);
        ctx.stroke();

        ctx.fillStyle = '#ffffff';
        ctx.font = '15px monospace';
        ctx.textAlign = 'left';
        const lines = reportText.split('\n');
        let y = 110;
        lines.forEach(line => {
            if (line.trim()) {
                ctx.fillText(line, 30, y);
                y += 26;
            } else {
                y += 12;
            }
        });

        ctx.fillStyle = '#d69e2e';
        ctx.font = '11px Arial';
        ctx.textAlign = 'center';
        ctx.fillText(`Generated: ${new Date().toLocaleString()}`, canvas.width/2, canvas.height - 25);

        const link = document.createElement('a');
        link.download = `report-${getToday()}.png`;
        link.href = canvas.toDataURL('image/png');
        link.click();

        if (confirm('Share this image on WhatsApp?')) {
            canvas.toBlob(function(blob) {
                const file = new File([blob], 'report.png', { type: 'image/png' });
                if (navigator.share) {
                    navigator.share({
                        title: 'Marble POS Report',
                        files: [file]
                    }).catch(err => console.log('Share cancelled'));
                } else {
                    const phone = '923001234567';
                    const url = `https://wa.me/${phone}?text=${encodeURIComponent('📊 Report attached')}`;
                    window.open(url, '_blank');
                }
            });
        }
    }

    function sendWhatsAppReport() {
        const report = document.getElementById('reportDisplay').textContent;
        if (!report || report.trim() === '') return alert('Generate report first!');
        const phone = '923001234567';
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
            const color = t.type === 'expense' ? '#e53e3e' : t.type === 'receive' ? '#38a169' : '#2b6cb0';
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
        if (tabId === 'stock') { renderStockList(); }
        if (tabId === 'customers') { renderCustomers(); }
        if (tabId === 'expenses') { renderTodayExpenses(); }
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
            { id: 1, name: 'Atlantic Marble', category: 'stone', unit: 'm²', purchasePrice: 800, sellPrice: 1200, stock: 100, minStock: 10, length: 0, width: 0, area: 0 },
            { id: 2, name: 'White Marble', category: 'stone', unit: 'm²', purchasePrice: 1000, sellPrice: 1500, stock: 80, minStock: 8, length: 0, width: 0, area: 0 },
            { id: 3, name: 'Black Granite', category: 'stone', unit: 'm²', purchasePrice: 1400, sellPrice: 2000, stock: 50, minStock: 5, length: 0, width: 0, area: 0 },
            { id: 4, name: 'Tiles', category: 'material', unit: 'pcs', purchasePrice: 150, sellPrice: 250, stock: 500, minStock: 50, length: 0, width: 0, area: 0 },
            { id: 5, name: 'Adhesive', category: 'material', unit: 'pcs', purchasePrice: 80, sellPrice: 120, stock: 200, minStock: 20, length: 0, width: 0, area: 0 }
        ];
        saveDB();
    }

    // Check internet and sync
    setTimeout(() => {
        checkInternet();
    }, 1000);

    // Auto sync every 30 seconds
    setInterval(autoSync, 30000);

    renderAll();
    console.log('🚀 Marble POS Pro with Cloud Sync Ready!');
    console.log('📦 Items:', db.items.length);
    console.log('👥 Customers:', db.customers.length);
    console.log('💰 Sales:', db.sales.length);
</script>
</body>
</html>
