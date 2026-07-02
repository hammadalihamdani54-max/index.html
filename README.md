<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Warda Marble - Professional System</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
    <style>
        *{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Arial,sans-serif;}
        body{background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);padding:12px;min-height:100vh;}
        .container{max-width:1500px;margin:0 auto;background:white;border-radius:20px;overflow:hidden;box-shadow:0 20px 40px rgba(0,0,0,0.3);}
        .header{background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);padding:15px 20px;text-align:center;position:relative;border-bottom:3px solid #ffd700;}
        .header h2{color:#ffd700;font-size:26px;}
        .header p{color:#a0c4e0;font-size:11px;margin-top:5px;}
        .header-controls{position:absolute;top:15px;right:20px;display:flex;gap:8px;flex-wrap:wrap;align-items:center;}
        .header-controls button{padding:5px 12px;background:rgba(255,255,255,0.15);color:white;border:1px solid #ffd700;border-radius:6px;cursor:pointer;font-weight:bold;font-size:11px;transition:0.3s;}
        .header-controls button.active{background:#ffd700;color:#1a1a2e;}
        .admin-badge{background:#ffd700;color:#1a1a2e;padding:4px 10px;border-radius:20px;font-size:10px;font-weight:bold;}
        .viewer-badge{background:#6b7a8a;color:white;padding:4px 10px;border-radius:20px;font-size:10px;font-weight:bold;}
        .tabs{display:flex;background:#1a1a2e;flex-wrap:wrap;padding:0 5px;}
        .tab{padding:12px 22px;background:transparent;color:white;border:none;cursor:pointer;font-weight:bold;font-size:14px;transition:0.3s;border-bottom:3px solid transparent;}
        .tab.active{background:#2c3e50;color:#ffd700;border-bottom-color:#ffd700;}
        .tab-content{display:none;padding:20px;background:#f0f2f5;min-height:550px;}
        .tab-content.active{display:block;}
        .pos-grid{display:grid;grid-template-columns:1fr 1.2fr;gap:20px;}
        @media(max-width:900px){.pos-grid{grid-template-columns:1fr;}}
        .panel{background:white;padding:18px;border-radius:12px;box-shadow:0 2px 8px rgba(0,0,0,0.08);}
        .panel h3{margin-bottom:15px;color:#1a1a2e;border-left:4px solid #ffd700;padding-left:12px;font-size:18px;}
        .product-list{display:grid;grid-template-columns:repeat(auto-fill,minmax(190px,1fr));gap:10px;max-height:450px;overflow-y:auto;padding-right:5px;}
        .product-card{background:white;padding:10px;border:1px solid #e2e8f0;border-radius:10px;cursor:pointer;transition:0.2s;}
        .product-card:hover{border-color:#ffd700;transform:translateY(-2px);box-shadow:0 4px 12px rgba(0,0,0,0.1);}
        .product-name{font-weight:bold;font-size:14px;}
        .product-size{font-size:11px;color:#666;margin:4px 0;}
        .product-price{color:#27ae60;font-weight:bold;font-size:12px;}
        .group-filter{display:flex;gap:8px;margin-bottom:15px;flex-wrap:wrap;}
        .group-btn{padding:6px 16px;background:#e2e8f0;border:none;border-radius:25px;cursor:pointer;font-weight:bold;font-size:12px;}
        .group-btn.active{background:#ffd700;}
        .search-box input{width:100%;padding:10px;border:1px solid #ddd;border-radius:8px;font-size:13px;margin-bottom:10px;}
        .form-group{margin-bottom:12px;}
        label{display:block;font-size:12px;font-weight:bold;margin-bottom:4px;color:#334155;}
        input{width:100%;padding:8px 12px;border:1px solid #ddd;border-radius:6px;font-size:13px;}
        button{background:linear-gradient(135deg,#667eea,#764ba2);color:white;border:none;padding:7px 14px;border-radius:6px;cursor:pointer;margin:3px;font-size:12px;transition:0.2s;}
        button:hover{transform:translateY(-1px);filter:brightness(1.05);}
        .btn-success{background:#27ae60;}
        .btn-danger{background:#e74c3c;}
        .btn-warning{background:#f39c12;}
        .btn-info{background:#3498db;}
        .btn-whatsapp{background:#25d366;}
        .mode-btns{display:flex;gap:8px;margin-bottom:15px;}
        .mode-btn{flex:1;background:#e2e8f0;padding:8px;text-align:center;border-radius:8px;cursor:pointer;font-weight:bold;font-size:12px;}
        .mode-btn.active{background:#ffd700;}
        .input-row{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:10px;}
        .input-group{flex:1;min-width:100px;}
        .total-box{background:#1a1a2e;color:white;padding:15px;border-radius:10px;margin-top:15px;text-align:right;}
        .total-box p{margin:5px 0;font-size:13px;}
        table{width:100%;border-collapse:collapse;font-size:12px;}
        th,td{padding:8px;text-align:left;border-bottom:1px solid #e2e8f0;}
        th{background:#1a1a2e;color:#ffd700;font-weight:bold;}
        .badge{padding:3px 10px;border-radius:20px;font-size:10px;font-weight:bold;display:inline-block;}
        .badge-paid{background:#27ae60;color:white;}
        .badge-unpaid{background:#e74c3c;color:white;}
        .badge-partial{background:#f39c12;color:white;}
        .customer-card{background:white;border:1px solid #e2e8f0;border-radius:12px;padding:12px;margin-bottom:10px;transition:0.2s;}
        .customer-card:hover{box-shadow:0 2px 8px rgba(0,0,0,0.1);}
        .modal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.6);z-index:1000;justify-content:center;align-items:center;}
        .modal-content{background:white;padding:20px;border-radius:15px;max-width:650px;width:95%;max-height:85vh;overflow:auto;}
        .toast{position:fixed;bottom:20px;right:20px;background:#1a1a2e;color:#ffd700;padding:10px 20px;border-radius:10px;z-index:1000;font-size:13px;}
        .stats-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:15px;margin-bottom:20px;}
        .stat-card{background:linear-gradient(135deg,#1a1a2e,#2c3e50);padding:15px;border-radius:12px;text-align:center;color:white;}
        .stat-card h4{font-size:13px;margin-bottom:8px;}
        .stat-card .value{font-size:26px;font-weight:bold;color:#ffd700;}
        .daily-card{background:white;border-radius:10px;padding:12px;margin-bottom:10px;border-left:3px solid #ffd700;box-shadow:0 1px 3px rgba(0,0,0,0.05);}
        .invoice-box{font-family:Arial;max-width:650px;margin:auto;border:2px solid #ffd700;border-radius:12px;background:white;overflow:hidden;}
        .invoice-box .header{background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);color:#ffd700;padding:15px;text-align:center;border-radius:10px 10px 0 0;}
        .invoice-box .body{padding:20px;}
        .invoice-box table{width:100%;margin-top:10px;}
        .invoice-box th{background:#1a1a2e;color:#ffd700;padding:8px;text-align:center;}
        .invoice-box td{padding:8px;text-align:center;border-bottom:1px solid #ddd;}
        .action-btns{display:flex;flex-wrap:wrap;gap:5px;margin-top:8px;justify-content:center;}
        .balance-positive{color:#27ae60;font-weight:bold;}
        .balance-negative{color:#e74c3c;font-weight:bold;}
        .balance-zero{color:#ffd700;font-weight:bold;}
        .rtl{direction:rtl;text-align:right;}
        .rtl .total-box{text-align:left;}
        .rtl th,.rtl td{text-align:right;}
        .customer-summary{background:#1a1a2e;color:#ffd700;padding:12px;border-radius:10px;margin-top:15px;text-align:center;font-weight:bold;}
        .summary-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:10px;margin-bottom:15px;}
        .summary-box{background:white;padding:12px;border-radius:10px;text-align:center;border:1px solid #e2e8f0;box-shadow:0 1px 3px rgba(0,0,0,0.05);}
        .summary-box .num{font-size:24px;font-weight:bold;}
        .summary-box .lbl{font-size:11px;color:#666;margin-top:4px;}
        .summary-box.positive .num{color:#27ae60;}
        .summary-box.negative .num{color:#e74c3c;}
        .summary-box.warning .num{color:#f39c12;}
        .stock-row{display:flex;justify-content:space-between;padding:4px 0;border-bottom:1px solid #eee;font-size:12px;}
        .stock-row .item{font-weight:bold;}
        .stock-row .val{color:#1a1a2e;}
        .login-overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.8);display:flex;justify-content:center;align-items:center;z-index:9999;}
        .login-box{background:white;padding:30px;border-radius:15px;max-width:380px;width:90%;text-align:center;}
        .login-box h2{color:#1a1a2e;margin-bottom:10px;}
        .login-box p{color:#666;margin-bottom:15px;}
        .login-box input{width:100%;padding:12px;border:2px solid #ddd;border-radius:8px;font-size:16px;text-align:center;margin-bottom:12px;}
        .login-box input:focus{border-color:#ffd700;outline:none;}
        .login-box .error{color:#e74c3c;font-size:12px;margin-top:5px;display:none;}
        .view-only .btn,.view-only .pos-item,.view-only input,.view-only select{opacity:0.5;pointer-events:none;}
        .view-only .product-card{cursor:default;}
        .view-only .product-card:hover{transform:none;border-color:#e2e8f0;}
        .view-only .mode-btn{cursor:default;}
        .view-only .mode-btn.active{background:#e2e8f0;color:#666;}
    </style>
</head>
<body>
<div class="container" id="appContainer">
    <div class="header">
        <h2 id="companyName">🏢 WARDA MARBLE</h2>
        <p id="companyTagline">Premium Marble Solutions | Since 2020</p>
        <div class="header-controls">
            <span id="roleBadge" class="admin-badge">👑 Admin</span>
            <button onclick="toggleLang()">🌐 AR</button>
            <button onclick="toggleCurrency()">💱 USD</button>
            <button onclick="logout()" id="logoutBtn" style="background:#e74c3c;border-color:#e74c3c;">🚪 Logout</button>
        </div>
    </div>
    <div class="tabs">
        <button class="tab active" onclick="showTab('pos')">🛒 POS</button>
        <button class="tab" onclick="showTab('customers')">👥 Customers</button>
        <button class="tab" onclick="showTab('inventory')">📦 Inventory</button>
        <button class="tab" onclick="showTab('invoices')">📄 Invoices</button>
        <button class="tab" onclick="showTab('summary')">📊 Summary</button>
        <button class="tab" onclick="showTab('daily')">📅 Daily</button>
        <button class="tab" onclick="showTab('monthly')">📈 Monthly</button>
        <button class="tab" onclick="showTab('settings')">⚙️ Settings</button>
    </div>

    <!-- ========== POS TAB ========== -->
    <div id="posTab" class="tab-content active">
        <div class="pos-grid">
            <div class="panel">
                <h3>📦 Products</h3>
                <div class="group-filter">
                    <button class="group-btn active" data-group="all" onclick="filterProducts('all')">All</button>
                    <button class="group-btn" data-group="mwad" onclick="filterProducts('mwad')">مواد بناء</button>
                    <button class="group-btn" data-group="mojali" onclick="filterProducts('mojali')">حجر مجلي</button>
                    <button class="group-btn" data-group="mosnafar" onclick="filterProducts('mosnafar')">حجر مصنفر</button>
                </div>
                <div class="search-box"><input type="text" id="searchProduct" placeholder="🔍 Search products..." onkeyup="displayProducts()"></div>
                <div class="product-list" id="productList"></div>
            </div>
            <div class="panel">
                <h3>🛒 Shopping Cart</h3>
                <div class="mode-btns">
                    <div class="mode-btn active" onclick="setMode('sqm')">📐 SQM (Marble)</div>
                    <div class="mode-btn" onclick="setMode('manual')">📏 Manual m²</div>
                    <div class="mode-btn" onclick="setMode('pieces')">🔧 Pieces</div>
                </div>
                <div class="input-row">
                    <div class="input-group"><label>Item Name</label><input type="text" id="itemName" placeholder="Select item"></div>
                    <div class="input-group"><label>Cost Price</label><input type="number" id="costPrice" step="0.01"></div>
                </div>
                <div id="sqmFields"><div class="input-row"><div class="input-group"><label>Thick(cm)</label><input type="number" id="thick" value="3"></div><div class="input-group"><label>Width(m)</label><input type="number" id="width" value="0.40" step="0.01"></div><div class="input-group"><label>Length(m)</label><input type="number" id="length" value="0.80" step="0.01"></div><div class="input-group"><label>Price/m²</label><input type="number" id="sqmPrice" step="0.01"></div></div></div>
                <div id="manualFields" style="display:none"><div class="input-row"><div class="input-group"><label>Area(m²)</label><input type="number" id="manualArea" step="0.01"></div><div class="input-group"><label>Price/m²</label><input type="number" id="manualPrice" step="0.01"></div></div></div>
                <div id="piecesFields" style="display:none"><div class="input-row"><div class="input-group"><label>Size</label><input type="text" id="pieceSize"></div><div class="input-group"><label>Qty</label><input type="number" id="pieceQty" value="1"></div><div class="input-group"><label>Price/pc</label><input type="number" id="piecePrice" step="0.01"></div></div></div>
                <button onclick="addToCart()" class="btn-success" style="width:100%">➕ Add to Cart</button>
                <div style="margin-top:12px"><label>👤 Customer Name</label><input type="text" id="customerName"></div>
                <div><label>📞 Phone Number</label><input type="text" id="customerPhone"></div>
                <div><label>💰 Amount Received</label><input type="number" id="amountReceived" value="0" step="0.01" oninput="updateTotals()"></div>
                <div style="max-height:220px;overflow:auto;margin-top:10px">
                    <table id="cartTable">
                        <thead><tr><th>Item</th><th>Size</th><th>Qty</th><th>Price</th><th>Total</th><th></th></tr></thead>
                        <tbody id="cartBody"></tbody>
                    </table>
                </div>
                <div class="total-box">
                    <p><strong>Subtotal:</strong> <span id="subtotal">SAR 0.00</span></p>
                    <p><strong>Tax (<span id="taxRateDisplay">15</span>%):</strong> <span id="taxAmount">SAR 0.00</span></p>
                    <p><strong>Total:</strong> <span id="grandTotal">SAR 0.00</span></p>
                    <p><strong>Received:</strong> <span id="received">SAR 0.00</span></p>
                    <p id="balanceMsg" style="font-weight:bold"></p>
                </div>
                <div class="action-btns">
                    <button onclick="confirmOrder()" class="btn-success">✅ Confirm Order</button>
                    <button onclick="clearCart()" class="btn-danger">🗑️ Clear</button>
                    <button onclick="undoCart()" class="btn-warning">↩️ Undo</button>
                    <button onclick="generateCartPDF()" class="btn-info">📄 PDF</button>
                </div>
            </div>
        </div>
    </div>

    <!-- ========== CUSTOMERS TAB ========== -->
    <div id="customersTab" class="tab-content">
        <div class="panel">
            <h3>👥 Customer Accounts</h3>
            <div class="search-box"><input type="text" id="customerSearch" placeholder="🔍 Search..." onkeyup="displayCustomers()"></div>
            <div id="customerList" style="max-height:450px;overflow-y:auto;"></div>
            <div id="customerTotalBox" class="customer-summary"></div>
        </div>
    </div>

    <!-- ========== INVENTORY TAB ========== -->
    <div id="inventoryTab" class="tab-content">
        <div class="panel">
            <h3>📦 Inventory Management</h3>
            <div class="group-filter">
                <button class="group-btn active" onclick="filterInventory('all')">All</button>
                <button class="group-btn" onclick="filterInventory('mwad')">مواد بناء</button>
                <button class="group-btn" onclick="filterInventory('mojali')">حجر مجلي</button>
                <button class="group-btn" onclick="filterInventory('mosnafar')">حجر مصنفر</button>
            </div>
            <div class="search-box"><input type="text" id="inventorySearch" placeholder="🔍 Search..." onkeyup="loadInventory()"></div>
            <button onclick="addNewProduct()" class="btn-success" style="margin-bottom:10px">➕ Add Product</button>
            <div style="overflow-x:auto;max-height:400px;overflow-y:auto">
                <table id="inventoryTable">
                    <thead><tr><th>Group</th><th>Name</th><th>Size</th><th>Selling</th><th>Cost</th><th>Stock</th><th>Actions</th></tr></thead>
                    <tbody id="inventoryBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- ========== INVOICES TAB ========== -->
    <div id="invoicesTab" class="tab-content">
        <div class="panel">
            <h3>📄 All Invoices</h3>
            <div class="search-box"><input type="text" id="invoiceSearch" placeholder="🔍 Search..." onkeyup="displayInvoices()"></div>
            <div style="overflow-x:auto;max-height:450px;overflow-y:auto">
                <table id="invoicesTable">
                    <thead><tr><th>Invoice</th><th>Date</th><th>Customer</th><th>Total</th><th>Status</th><th>Actions</th></tr></thead>
                    <tbody id="invoicesBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- ========== SUMMARY TAB ========== -->
    <div id="summaryTab" class="tab-content">
        <div class="panel">
            <h3>📊 Daily Summary - <span id="summaryDate"></span></h3>
            <div class="summary-grid" id="summaryGrid">
                <div class="summary-box positive"><div class="num" id="sumTodaySale">0.00</div><div class="lbl">Today's Sale</div></div>
                <div class="summary-box warning"><div class="num" id="sumUdharSale">0.00</div><div class="lbl">Udhar Sale</div></div>
                <div class="summary-box negative"><div class="num" id="sumIkhrajat">0.00</div><div class="lbl">Ikhrajat</div></div>
                <div class="summary-box positive"><div class="num" id="sumSabqaWusool">0.00</div><div class="lbl">Sabqa Wusool</div></div>
                <div class="summary-box positive"><div class="num" id="sumAdvanceWusool">0.00</div><div class="lbl">Advance Wusool</div></div>
                <div class="summary-box positive"><div class="num" id="sumTotalJamma">0.00</div><div class="lbl">Total Jamma</div></div>
                <div class="summary-box" id="paymentMinusBox"><div class="num" id="sumPaymentMinus">0.00</div><div class="lbl">Payment Minus</div></div>
            </div>
            <div style="font-size:11px;color:#666;text-align:center;margin-bottom:10px;">Auto calculated from today's data</div>
            
            <h3>📦 Opening / Closing Stock</h3>
            <div id="stockSummaryDisplay" style="background:#f8fafc;padding:10px;border-radius:8px;max-height:200px;overflow-y:auto;"></div>
            <div style="font-size:10px;color:#666;text-align:center;margin-top:5px;">Opening = Previous day closing | Closing = Opening + In - Out - Sales</div>
            
            <h3 style="margin-top:15px;">📅 Monthly Summary - <span id="monthlyDate"></span></h3>
            <div style="overflow-x:auto;max-height:200px;overflow-y:auto">
                <table id="monthlySummaryTable">
                    <thead><tr><th>Date</th><th>Sale</th><th>Udhar</th><th>Ikhrajat</th><th>Jamma</th></tr></thead>
                    <tbody id="monthlySummaryBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- ========== DAILY REPORT TAB ========== -->
    <div id="dailyTab" class="tab-content">
        <div class="panel">
            <h3>📅 Daily Sales Report</h3>
            <div style="display:flex;gap:10px;align-items:center;margin-bottom:15px;flex-wrap:wrap">
                <label style="margin:0">Select Date:</label>
                <input type="date" id="dailyDate" style="width:200px" onchange="loadDailyReport()">
                <button onclick="loadDailyReport()" class="btn-info">Load</button>
            </div>
            <div class="stats-grid">
                <div class="stat-card"><h4>Today's Sales</h4><div class="value" id="dailyTotalSales">SAR 0</div></div>
                <div class="stat-card"><h4>Today's Profit</h4><div class="value" id="dailyTotalProfit">SAR 0</div></div>
                <div class="stat-card"><h4>Today's Orders</h4><div class="value" id="dailyTotalOrders">0</div></div>
            </div>
            <div id="dailyList" style="max-height:350px;overflow-y:auto;"></div>
        </div>
    </div>

    <!-- ========== MONTHLY REPORT TAB ========== -->
    <div id="monthlyTab" class="tab-content">
        <div class="panel">
            <h3>📊 Monthly Transactions</h3>
            <div style="display:flex;gap:10px;align-items:center;margin-bottom:15px;flex-wrap:wrap">
                <label style="margin:0">Select Month:</label>
                <input type="month" id="monthPicker" style="width:200px" onchange="loadMonthlyReport()">
                <button onclick="loadMonthlyReport()" class="btn-info">Load</button>
            </div>
            <div class="stats-grid">
                <div class="stat-card"><h4>Total Sales</h4><div class="value" id="monthlyTotalSales">SAR 0</div></div>
                <div class="stat-card"><h4>Total Profit</h4><div class="value" id="monthlyTotalProfit">SAR 0</div></div>
                <div class="stat-card"><h4>Total Orders</h4><div class="value" id="monthlyTotalOrders">0</div></div>
                <div class="stat-card"><h4>Avg Order</h4><div class="value" id="monthlyAvgOrder">SAR 0</div></div>
            </div>
            <div style="overflow-x:auto;max-height:350px;overflow-y:auto">
                <table id="monthlyTable">
                    <thead><tr><th>Date</th><th>Invoice</th><th>Customer</th><th>Phone</th><th>Items</th><th>Total</th><th>Profit</th><th>Status</th><th>Actions</th></tr></thead>
                    <tbody id="monthlyBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- ========== SETTINGS TAB ========== -->
    <div id="settingsTab" class="tab-content">
        <div class="panel">
            <h3>⚙️ System Settings</h3>
            <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:20px">
                <div>
                    <h4>🏢 Company Info</h4>
                    <div class="form-group"><label>Company Name</label><input type="text" id="companyNameSetting"></div>
                    <div class="form-group"><label>Tagline</label><input type="text" id="companyTaglineSetting"></div>
                    <div class="form-group"><label>Address</label><input type="text" id="companyAddressSetting"></div>
                    <div class="form-group"><label>Phone</label><input type="text" id="companyPhoneSetting"></div>
                    <button onclick="saveCompanySettings()" class="btn-success">💾 Save</button>
                </div>
                <div>
                    <h4>💰 Tax Settings</h4>
                    <div class="form-group"><label><input type="checkbox" id="taxEnableCheck"> Enable Tax</label></div>
                    <div class="form-group"><label>Tax Rate (%)</label><input type="number" id="taxRateSetting" value="15" step="0.5"></div>
                    <div class="form-group"><label>Tax Name</label><input type="text" id="taxNameSetting" value="VAT"></div>
                    <button onclick="saveTaxSettings()" class="btn-success">💾 Save</button>
                </div>
                <div>
                    <h4>💾 Data Management</h4>
                    <button onclick="backupData()" class="btn-info">📥 Backup</button>
                    <button onclick="document.getElementById('restoreFile').click()" class="btn-warning">📤 Restore</button>
                    <input type="file" id="restoreFile" style="display:none" onchange="restoreData(this)">
                    <button onclick="resetAll()" class="btn-danger">⚠️ Reset All</button>
                    <button onclick="syncToCloud()" class="btn-success" style="margin-top:5px;">☁️ Sync to Cloud</button>
                    <button onclick="syncFromCloud()" class="btn-info" style="margin-top:5px;">☁️ Sync from Cloud</button>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- ========== MODALS ========== -->
<div id="invoiceModal" class="modal"><div class="modal-content"><div id="invoiceDetails"></div><div style="text-align:center;margin-top:20px;display:flex;flex-wrap:wrap;justify-content:center;gap:8px"><button onclick="closeModal()">Close</button><button onclick="printInvoice()">🖨️ Print</button><button onclick="downloadPDF()" class="btn-info">📄 PDF</button><button onclick="shareInvoiceImage()" class="btn-whatsapp">📷 Share</button><button onclick="sendWhatsApp()" class="btn-success">📱 WhatsApp</button></div></div></div>
<div id="paymentModal" class="modal"><div class="modal-content"><h3>💰 Receive Payment</h3><input type="number" id="paymentAmount" step="0.01" placeholder="Enter amount"><br><br><input type="text" id="paymentNotes" placeholder="Notes"><br><br><button onclick="confirmPayment()" class="btn-success">Confirm</button><button onclick="closePaymentModal()" class="btn-danger">Cancel</button></div></div>
<div id="editModal" class="modal"><div class="modal-content"><h3>✏️ Edit Invoice</h3><div class="form-group"><label>Customer Name</label><input type="text" id="editCustomer"></div><div class="form-group"><label>Phone</label><input type="text" id="editPhone"></div><div class="form-group"><label>Discount %</label><input type="number" id="editDiscount" step="1"></div><button onclick="saveEdit()" class="btn-success">Save</button><button onclick="closeEditModal()" class="btn-danger">Cancel</button></div></div>
<div id="pdfTemplate" style="position:fixed;top:-9999px;left:-9999px;"></div>

<!-- ========== LOGIN OVERLAY ========== -->
<div id="loginOverlay" class="login-overlay">
    <div class="login-box">
        <h2>🔐 Admin Login</h2>
        <p>Enter password to manage system</p>
        <input type="password" id="loginPassword" placeholder="Enter password..." onkeydown="if(event.key==='Enter') checkLogin()">
        <button onclick="checkLogin()" class="btn-success" style="width:100%;padding:12px;">🔓 Login</button>
        <div id="loginError" class="error">❌ Wrong password! Try again.</div>
        <div style="margin-top:10px;font-size:11px;color:#999;">Default: <strong>Hamdani5</strong></div>
    </div>
</div>

<script>
// ==================== FIREBASE CONFIG ====================
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

// ==================== LOGIN SYSTEM ====================
const ADMIN_PASSWORD = "Hamdani5";
let isAdmin = false;

function checkLogin() {
    const input = document.getElementById('loginPassword').value;
    const error = document.getElementById('loginError');
    if (input === ADMIN_PASSWORD) {
        isAdmin = true;
        document.getElementById('loginOverlay').style.display = 'none';
        document.getElementById('roleBadge').textContent = '👑 Admin';
        document.getElementById('roleBadge').className = 'admin-badge';
        document.getElementById('logoutBtn').style.display = 'inline-block';
        document.getElementById('appContainer').classList.remove('view-only');
        showToast('✅ Admin mode activated!');
    } else {
        error.style.display = 'block';
        setTimeout(() => error.style.display = 'none', 3000);
    }
}

function logout() {
    isAdmin = false;
    document.getElementById('loginOverlay').style.display = 'flex';
    document.getElementById('loginPassword').value = '';
    document.getElementById('roleBadge').textContent = '👁️ Viewer';
    document.getElementById('roleBadge').className = 'viewer-badge';
    document.getElementById('logoutBtn').style.display = 'none';
    document.getElementById('appContainer').classList.add('view-only');
    showToast('👁️ Viewer mode activated');
}

// ==================== DATA ==================
let products = [], orders = [], customers = [], cart = [], cartHistory = [];
let currentLang = 'en', currentCurrency = 'SAR', currentMode = 'sqm';
let currentGroup = 'all', currentInvGroup = 'all';
let taxEnabled = true, taxRate = 15, taxName = "VAT";
let company = { name: "WARDA MARBLE", tagline: "Premium Marble Solutions | Since 2020", address: "Tabuk, Saudi Arabia", phone: "+966 59 807 4287" };
let currentInvoiceNo = null, currentPayInvoice = null, currentEditInvoice = null;

// ==================== BUILD PRODUCTS ==================
function buildProducts() {
    let prods = []; let id = 1;
    let mwad = [
        ["زاویہ اپ ڈاون","4*4*3mm",9200],["زاویہ اپ ڈاون","4*4*2mm",6600],["زاویہ اپ ڈاون","4*3*3mm",4300],["زاویہ اپ ڈاون","4*3*2mm",2500],
        ["زاویہ اپ ڈاون","4*5*3mm",1200],["زاویہ اپ ڈاون","4*6*3mm",850],["زاویہ اپ ڈاون","4*8*3mm",420],["زاویہ اپ ڈاون","4*10*3mm",400],
        ["مسمار مغلوب","10*40",4800],["مسمار مغلوب","10*50",4800],["مسمار مغلوب","10*60",3500],["مسمار مغلوب","10*80",1500],["مسمار مغلوب","10*100",1440],
        ["براغی سقف","10*72",2200],["براغی سقف","10*92",2100],["براغی سقف","10*112",1920],["براغی سقف","10*115",2200],["براغی سقف","10*202",1040],
        ["جولی ابو جمل","Standard",5],["جولی گلیڈتیر","Standard",0],["دسک حجر احمر","7 بوسہ",0],["دسک حجر احمر","4.50 بوسہ",7],
        ["دسک جرانیت","7 بوسہ",3],["دسک بوش","9 بوسہ",20],["ریشہ ہلتی","20cm",0],["ریشہ ہلتی","15cm",24],["ریشہ ہلتی","10cm",1],
        ["ریشہ دریل","4mm",12],["ریشہ دریل","6mm",10],["ریشہ دریل","10mm",16],["گمتا چوڑی","Standard",7],["گمتا سادہ","Standard",7],["بکرا","Standard",1]
    ];
    mwad.forEach(m => { prods.push({ id: id++, name: m[0], size: m[1], group: 'mwad', selling: 0, cost: 0, stock: m[2], type: 'pieces' }); });
    
    let mojaliSizes = ["3*7","3*10","3*15","3*18","3*20","3*25","3*27","3*30","3*32","3*35","3*37","3*40","3*50","3*60"];
    mojaliSizes.forEach(s => { prods.push({ id: id++, name: "ابیض مجلی مبروم", size: s, group: 'mojali', selling: 65, cost: 50, stock: 50, type: 'sqm' }); });
    mojaliSizes.forEach(s => { prods.push({ id: id++, name: "ابیض مجلی سادہ", size: s, group: 'mojali', selling: 60, cost: 45, stock: 50, type: 'sqm' }); });
    let mojaliSpecial = [["(A) ابیض مجلی فرزا","3*40*80",75],["(C) ابیض مجلی فرزا","3*40*60",75],["ابیض مجلی مشطوف","3*30",70],["ابیض بشارتہ فرزا","3*40*80",85],["MNHOOD ABIAZ","3*20*1.20",65]];
    mojaliSpecial.forEach(m => { prods.push({ id: id++, name: m[0], size: m[1], group: 'mojali', selling: m[2], cost: m[2]-12, stock: 50, type: 'sqm' }); });
    
    let mosnafarSizes = ["3*7","3*10","3*15","3*18","3*20","3*25","3*27","3*30","3*32","3*35","3*37","3*40","3*50","3*60","4*7","5*7"];
    mosnafarSizes.forEach(s => { prods.push({ id: id++, name: "ابیض مصنفر مبروم", size: s, group: 'mosnafar', selling: 55, cost: 42, stock: 50, type: 'sqm' }); });
    mosnafarSizes.forEach(s => { prods.push({ id: id++, name: "ابیض مصنفر سادہ", size: s, group: 'mosnafar', selling: 50, cost: 38, stock: 50, type: 'sqm' }); });
    let mosnafarSpecial = [["ابیض مصنفر فرزا","3*40*80",65],["پیانو ابیض مصنفر","3*20*50",55]];
    mosnafarSpecial.forEach(m => { prods.push({ id: id++, name: m[0], size: m[1], group: 'mosnafar', selling: m[2], cost: m[2]-10, stock: 50, type: 'sqm' }); });
    return prods;
}

// ==================== HELPERS ==================
function formatNumber(n) { let v = n.toFixed(2); if(currentLang === 'ar') return v.replace(/\d/g, d => '٠١٢٣٤٥٦٧٨٩'[d]); return v; }
function formatCurrency(amt) { let r = currentCurrency === 'SAR' ? 1 : 0.27; let s = currentCurrency === 'SAR' ? 'SAR ' : '$ '; return s + formatNumber(amt * r); }
function showToast(msg) { let t = document.createElement('div'); t.className='toast'; t.innerText=msg; document.body.appendChild(t); setTimeout(()=>t.remove(),2500); }
function saveData() { localStorage.setItem('products', JSON.stringify(products)); localStorage.setItem('orders', JSON.stringify(orders)); localStorage.setItem('customers', JSON.stringify(customers)); localStorage.setItem('taxEnabled', taxEnabled); localStorage.setItem('taxRate', taxRate); localStorage.setItem('taxName', taxName); localStorage.setItem('company', JSON.stringify(company)); }
function loadData() { let p = localStorage.getItem('products'); products = (p && JSON.parse(p).length) ? JSON.parse(p) : buildProducts(); let o = localStorage.getItem('orders'); if(o) orders = JSON.parse(o); let c = localStorage.getItem('customers'); if(c) customers = JSON.parse(c); let te = localStorage.getItem('taxEnabled'); if(te !== null) taxEnabled = te === 'true'; let tr = localStorage.getItem('taxRate'); if(tr) taxRate = parseFloat(tr); let tn = localStorage.getItem('taxName'); if(tn) taxName = tn; let comp = localStorage.getItem('company'); if(comp) company = JSON.parse(comp); }

function toggleLang() { currentLang = currentLang === 'en' ? 'ar' : 'en'; if(currentLang==='ar') document.body.classList.add('rtl'); else document.body.classList.remove('rtl'); displayProducts(); displayCustomers(); displayInvoices(); loadInventory(); loadMonthlyReport(); loadDailyReport(); updateCartDisplay(); showToast(currentLang==='ar'?'العربية':'English'); }
function toggleCurrency() { currentCurrency = currentCurrency === 'SAR' ? 'USD' : 'SAR'; displayProducts(); displayCustomers(); displayInvoices(); loadInventory(); loadMonthlyReport(); loadDailyReport(); updateCartDisplay(); showToast(currentCurrency); }
function setMode(mode) { currentMode = mode; document.querySelectorAll('.mode-btn').forEach(b=>b.classList.remove('active')); document.querySelectorAll('.mode-btn')[mode==='sqm'?0:mode==='manual'?1:2].classList.add('active'); document.getElementById('sqmFields').style.display = mode==='sqm'?'block':'none'; document.getElementById('manualFields').style.display = mode==='manual'?'block':'none'; document.getElementById('piecesFields').style.display = mode==='pieces'?'block':'none'; }
function showTab(tab) { document.querySelectorAll('.tab-content').forEach(t=>t.classList.remove('active')); document.getElementById(tab+'Tab').classList.add('active'); document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active')); event.target.classList.add('active'); if(tab==='inventory') loadInventory(); if(tab==='invoices') displayInvoices(); if(tab==='customers') displayCustomers(); if(tab==='daily') loadDailyReport(); if(tab==='monthly') loadMonthlyReport(); if(tab==='summary') updateSummary(); }

// ==================== PRODUCTS ==================
function displayProducts() {
    let search = document.getElementById('searchProduct').value.toLowerCase();
    let filtered = products.filter(p => (currentGroup==='all' || p.group===currentGroup) && (p.name.toLowerCase().includes(search) || p.size.toLowerCase().includes(search)));
    let c = document.getElementById('productList'); c.innerHTML='';
    filtered.forEach(p => { let d = document.createElement('div'); d.className='product-card'; d.innerHTML=`<div class="product-name">${p.name}</div><div class="product-size">${p.size}</div><div class="product-price">${p.selling>0?formatCurrency(p.selling):'Set Price'}${p.type==='sqm'?'/m²':'/pc'}</div><div>Stock: ${p.stock}</div>`; d.onclick=()=>{ if(!isAdmin) return showToast('🔒 Login first!'); document.getElementById('itemName').value=p.name; document.getElementById('costPrice').value=p.cost; if(p.type==='sqm'){ setMode('sqm'); document.getElementById('sqmPrice').value=p.selling; }else{ setMode('pieces'); document.getElementById('pieceSize').value=p.size; document.getElementById('piecePrice').value=p.selling; } showToast(p.name+' loaded'); }; c.appendChild(d); });
}
function filterProducts(g) { currentGroup=g; document.querySelectorAll('#productFilter .group-btn').forEach(b=>b.classList.remove('active')); event.target.classList.add('active'); displayProducts(); }
function filterInventory(g) { currentInvGroup=g; document.querySelectorAll('#inventoryFilter .group-btn').forEach(b=>b.classList.remove('active')); event.target.classList.add('active'); loadInventory(); }
function loadInventory() {
    let search = document.getElementById('inventorySearch').value.toLowerCase();
    let filtered = products.filter(p => (currentInvGroup==='all' || p.group===currentInvGroup) && (p.name.toLowerCase().includes(search) || p.size.toLowerCase().includes(search)));
    let tb = document.getElementById('inventoryBody'); tb.innerHTML='';
    filtered.forEach(p => { let r = tb.insertRow(); r.insertCell(0).innerHTML = p.group==='mwad'?'Materials':(p.group==='mojali'?'Polished':'Honed'); r.insertCell(1).innerHTML = p.name; r.insertCell(2).innerHTML = p.size; r.insertCell(3).innerHTML = `<span style="cursor:pointer;color:#3b82f6" onclick="editProduct(${p.id},'selling')">✏️ ${formatCurrency(p.selling)}</span>`; r.insertCell(4).innerHTML = `<span style="cursor:pointer;color:#8b5cf6" onclick="editProduct(${p.id},'cost')">✏️ ${formatCurrency(p.cost)}</span>`; r.insertCell(5).innerHTML = `<span style="cursor:pointer;color:#10b981" onclick="editProduct(${p.id},'stock')">✏️ ${p.stock}</span>`; let c = r.insertCell(6); let d = document.createElement('button'); d.innerHTML='Delete'; d.style.background='#e74c3c'; d.style.padding='3px 8px'; d.onclick=()=>{ if(!isAdmin) return showToast('🔒 Login first!'); if(confirm('Delete?')){ products=products.filter(x=>x.id!==p.id); saveData(); loadInventory(); displayProducts(); showToast('Deleted'); } }; c.appendChild(d); });
}
function editProduct(id,field) { if(!isAdmin) return showToast('🔒 Login first!'); let p=products.find(x=>x.id===id); if(p){ let v=prompt(`Edit ${field}:`,p[field]); if(v!==null && !isNaN(parseFloat(v))){ p[field]=parseFloat(v); saveData(); loadInventory(); displayProducts(); showToast('Updated'); } } }
function addNewProduct() { if(!isAdmin) return showToast('🔒 Login first!'); let type = confirm('Marble (SQM) - OK, Fitting (Pieces) - Cancel') ? 'sqm' : 'pieces'; let g = prompt('Group: 1-Materials, 2-Polished, 3-Honed'); let gn = 'mwad'; if(g === '2') gn = 'mojali'; else if(g === '3') gn = 'mosnafar'; let name = prompt('Product name:'); if(!name) return; let id = Math.max(...products.map(p=>p.id),0)+1; let np = { id, name, size: prompt('Size:'), group: gn, selling: parseFloat(prompt('Selling Price:')), cost: parseFloat(prompt('Cost Price:'))||0, stock: parseInt(prompt('Stock:'))||50, type }; if(type === 'sqm') np.thickness = 3; products.push(np); saveData(); displayProducts(); loadInventory(); showToast('Product added'); }

// ==================== CART ==================
function addToCart() { if(!isAdmin) return showToast('🔒 Login first!'); let name = document.getElementById('itemName').value; if(!name){ showToast('Select item first'); return; }
    cartHistory.push(JSON.parse(JSON.stringify(cart)));
    let cost = parseFloat(document.getElementById('costPrice').value)||0;
    if(currentMode==='sqm'){
        let t=parseFloat(document.getElementById('thick').value)||3, w=parseFloat(document.getElementById('width').value)||0.40, l=parseFloat(document.getElementById('length').value)||0.80, p=parseFloat(document.getElementById('sqmPrice').value);
        if(!p){ showToast('Enter price'); return; }
        let area=w*l; cart.push({ id:Date.now(), name, size:`${t}cm x ${w.toFixed(2)}m x ${l.toFixed(2)}m`, qty:area.toFixed(2)+' m²', price:p, cost:cost||p*0.7, total:area*p });
    }else if(currentMode==='manual'){
        let area=parseFloat(document.getElementById('manualArea').value), p=parseFloat(document.getElementById('manualPrice').value);
        if(!area || !p){ showToast('Enter area and price'); return; }
        cart.push({ id:Date.now(), name, size:'Manual', qty:area.toFixed(2)+' m²', price:p, cost:cost||p*0.7, total:area*p });
    }else{
        let size=document.getElementById('pieceSize').value, qty=parseFloat(document.getElementById('pieceQty').value), p=parseFloat(document.getElementById('piecePrice').value);
        if(!p){ showToast('Enter price'); return; }
        cart.push({ id:Date.now(), name, size:size, qty:qty+' pcs', price:p, cost:cost||p*0.6, total:qty*p });
    }
    updateCartDisplay(); showToast('Added to cart');
    if(currentMode==='sqm') document.getElementById('sqmPrice').value='';
    else if(currentMode==='manual'){ document.getElementById('manualArea').value=''; document.getElementById('manualPrice').value=''; }
    else{ document.getElementById('piecePrice').value=''; document.getElementById('pieceQty').value=1; }
}
function updateCartDisplay() {
    let tb = document.getElementById('cartBody'); tb.innerHTML=''; let sub=0;
    cart.forEach((item,idx)=>{ sub+=item.total; let r=tb.insertRow(); r.insertCell(0).innerHTML=item.name; r.insertCell(1).innerHTML=item.size; r.insertCell(2).innerHTML=item.qty; r.insertCell(3).innerHTML=formatCurrency(item.price); r.insertCell(4).innerHTML=formatCurrency(item.total); let btn=document.createElement('button'); btn.innerHTML='✖'; btn.style.background='#e74c3c'; btn.style.padding='2px 8px'; btn.onclick=()=>{ cart.splice(idx,1); updateCartDisplay(); }; r.insertCell(5).appendChild(btn); });
    updateTotals(sub);
}
function updateTotals(sub) { if(!sub) sub=cart.reduce((s,i)=>s+i.total,0); let tax=taxEnabled?sub*taxRate/100:0; let total=sub+tax; document.getElementById('subtotal').innerHTML=formatCurrency(sub); document.getElementById('taxAmount').innerHTML=formatCurrency(tax); document.getElementById('grandTotal').innerHTML=formatCurrency(total); let rec=parseFloat(document.getElementById('amountReceived').value)||0; document.getElementById('received').innerHTML=formatCurrency(rec); let bal=total-rec; let custName = document.getElementById('customerName').value || 'Customer';
    if(bal>0){ document.getElementById('balanceMsg').innerHTML=`<span class="balance-negative">Due from ${custName}: ${formatCurrency(bal)}</span>`; }
    else if(bal<0){ document.getElementById('balanceMsg').innerHTML=`<span class="balance-positive">Refund to ${custName}: ${formatCurrency(Math.abs(bal))}</span>`; }
    else{ document.getElementById('balanceMsg').innerHTML=`<span class="balance-zero">Fully Paid ✓</span>`; } }
function clearCart() { if(confirm('Clear entire cart?')){ cartHistory.push(JSON.parse(JSON.stringify(cart))); cart=[]; updateCartDisplay(); } }
function undoCart() { if(cartHistory.length){ cart=cartHistory.pop(); updateCartDisplay(); showToast('Undo successful'); } else showToast('Nothing to undo'); }
function confirmOrder() { if(!isAdmin) return showToast('🔒 Login first!'); if(cart.length===0){ showToast('Cart is empty'); return; }
    let cust=document.getElementById('customerName').value||'Walk-in Customer';
    let phone=document.getElementById('customerPhone').value||'';
    let rec=parseFloat(document.getElementById('amountReceived').value)||0;
    let sub=cart.reduce((s,i)=>s+i.total,0);
    let tax=taxEnabled?sub*taxRate/100:0;
    let total=sub+tax;
    let invNo='INV-'+String(orders.length+1).padStart(4,'0');
    cart.forEach(i=>{ let p=products.find(pr=>pr.name===i.name); if(p && p.stock>0) p.stock--; });
    let paid=Math.min(rec,total);
    let status=paid>=total?'paid':(paid>0?'partial':'unpaid');
    let order={ invoiceNo:invNo, date:new Date().toISOString(), customer:cust, phone:phone, items:[...cart], subtotal:sub, taxAmount:tax, total, cost:cart.reduce((s,i)=>s+i.cost,0), profit:total-cart.reduce((s,i)=>s+i.cost,0), status, paid, payments:paid>0?[{amount:paid,notes:'POS Payment',date:new Date().toISOString()}]:[] };
    orders.push(order);
    let custObj=customers.find(c=>c.phone===phone);
    if(!custObj){ custObj={ name:cust, phone:phone, invoices:[] }; customers.push(custObj); }
    custObj.invoices.push({ invoiceNo:invNo, date:order.date, amount:total, paid:paid, status:status, payments:order.payments });
    saveData();
    showToast('Order confirmed! '+invNo);
    cart=[]; cartHistory=[]; updateCartDisplay(); displayProducts(); loadInventory(); displayCustomers(); displayInvoices(); loadMonthlyReport(); loadDailyReport(); updateSummary();
    document.getElementById('customerName').value=''; document.getElementById('customerPhone').value=''; document.getElementById('amountReceived').value=0;
}

// ==================== CUSTOMERS ==================
function displayCustomers() {
    let search=document.getElementById('customerSearch').value.toLowerCase();
    let filtered=customers.filter(c=>c.name.toLowerCase().includes(search)||(c.phone && c.phone.includes(search)));
    let container=document.getElementById('customerList'); container.innerHTML='';
    let totalDueAll=0, totalPaidAll=0;
    filtered.forEach(c=>{ let due=c.invoices.reduce((s,i)=>s+(i.amount-i.paid),0); let paidAmt=c.invoices.reduce((s,i)=>s+i.paid,0); totalDueAll+=due; totalPaidAll+=paidAmt; let div=document.createElement('div'); div.className='customer-card'; div.innerHTML=`<div style="display:flex;justify-content:space-between;flex-wrap:wrap;margin-bottom:8px"><div><strong>${c.name}</strong> ${c.phone?`📞 ${c.phone}`:''}</div><div><span class="badge ${due>0?'badge-unpaid':'badge-paid'}">${due>0?'Due: '+formatCurrency(due):'Settled'}</span></div></div>`; c.invoices.forEach(i=>{ let invDue = i.amount - i.paid; div.innerHTML+=`<div style="font-size:11px;border-top:1px solid #eee;padding:6px 0"><div style="display:flex;justify-content:space-between;flex-wrap:wrap"><div><strong>${i.invoiceNo}</strong> | ${new Date(i.date).toLocaleDateString()}</div><div>Total: ${formatCurrency(i.amount)} | Paid: ${formatCurrency(i.paid)} | ${invDue>0?'Due: '+formatCurrency(invDue):'Settled'}</div></div><div class="action-btns" style="margin-top:5px"><button onclick="openPayment('${i.invoiceNo}')" class="btn-success" style="padding:2px 10px">Pay</button><button onclick="viewInvoice('${i.invoiceNo}')" class="btn-info" style="padding:2px 10px">View</button><button onclick="editInvoice('${i.invoiceNo}')" class="btn-warning" style="padding:2px 10px">Edit</button><button onclick="deleteInvoice('${i.invoiceNo}')" class="btn-danger" style="padding:2px 10px">Del</button><button onclick="shareInvoiceFromInv('${i.invoiceNo}')" class="btn-whatsapp" style="padding:2px 10px">Share</button></div></div>`; }); container.appendChild(div); });
    document.getElementById('customerTotalBox').innerHTML = `<strong>Overall Summary</strong><br>Total Receivable: ${formatCurrency(totalDueAll)}<br>Total Received: ${formatCurrency(totalPaidAll)}<br>Net Balance: ${formatCurrency(totalDueAll)}`;
}

// ==================== INVOICES ==================
function displayInvoices() {
    let search=document.getElementById('invoiceSearch').value.toLowerCase();
    let filtered=orders.filter(o=>o.invoiceNo.toLowerCase().includes(search)||o.customer.toLowerCase().includes(search)).reverse();
    let tb=document.getElementById('invoicesBody'); tb.innerHTML='';
    filtered.forEach(o=>{ let r=tb.insertRow(); r.insertCell(0).innerHTML=o.invoiceNo; r.insertCell(1).innerHTML=new Date(o.date).toLocaleDateString(); r.insertCell(2).innerHTML=o.customer; r.insertCell(3).innerHTML=formatCurrency(o.total); r.insertCell(4).innerHTML=`<span class="badge badge-${o.status}">${o.status}</span>`; let c=r.insertCell(5); c.innerHTML=`<div class="action-btns"><button onclick="viewInvoice('${o.invoiceNo}')" class="btn-info" style="padding:2px 8px">View</button><button onclick="editInvoice('${o.invoiceNo}')" class="btn-warning" style="padding:2px 8px">Edit</button><button onclick="openPayment('${o.invoiceNo}')" class="btn-success" style="padding:2px 8px">Pay</button><button onclick="deleteInvoice('${o.invoiceNo}')" class="btn-danger" style="padding:2px 8px">Del</button><button onclick="shareInvoiceFromInv('${o.invoiceNo}')" class="btn-whatsapp" style="padding:2px 8px">Share</button></div>`; });
}
function openPayment(invNo) { if(!isAdmin) return showToast('🔒 Login first!'); currentPayInvoice=invNo; document.getElementById('paymentModal').style.display='flex'; document.getElementById('paymentAmount').value=''; document.getElementById('paymentNotes').value=''; }
function closePaymentModal() { document.getElementById('paymentModal').style.display='none'; }
function confirmPayment() { if(!isAdmin) return showToast('🔒 Login first!'); let amt=parseFloat(document.getElementById('paymentAmount').value); let notes=document.getElementById('paymentNotes').value||'Payment'; if(isNaN(amt)||amt<=0){ showToast('Enter valid amount'); return; } let order=orders.find(o=>o.invoiceNo===currentPayInvoice); if(order){ if(!order.payments) order.payments=[]; order.payments.push({amount:amt,notes:notes,date:new Date().toISOString()}); order.paid=(order.paid||0)+amt; order.status=order.paid>=order.total?'paid':(order.paid>0?'partial':'unpaid'); let cust=customers.find(c=>c.phone===order.phone); if(cust){ let inv=cust.invoices.find(i=>i.invoiceNo===currentPayInvoice); if(inv){ inv.paid=order.paid; inv.status=order.status; inv.payments=order.payments; } } saveData(); displayCustomers(); displayInvoices(); loadMonthlyReport(); loadDailyReport(); updateSummary(); showToast(`Received ${formatCurrency(amt)} - ${notes}`); } closePaymentModal(); }
function editInvoice(invNo) { if(!isAdmin) return showToast('🔒 Login first!'); let o=orders.find(x=>x.invoiceNo===invNo); if(o){ currentEditInvoice=invNo; document.getElementById('editCustomer').value=o.customer; document.getElementById('editPhone').value=o.phone||''; document.getElementById('editDiscount').value=0; document.getElementById('editModal').style.display='flex'; } }
function saveEdit() { if(!isAdmin) return showToast('🔒 Login first!'); let o=orders.find(x=>x.invoiceNo===currentEditInvoice); if(o){ let newCust=document.getElementById('editCustomer').value; if(newCust) o.customer=newCust; let newPhone=document.getElementById('editPhone').value; if(newPhone) o.phone=newPhone; let disc=parseFloat(document.getElementById('editDiscount').value)||0; if(disc>0){ let newTotal=o.subtotal-(o.subtotal*disc/100); o.taxAmount=taxEnabled?newTotal*taxRate/100:0; o.total=newTotal+o.taxAmount; o.profit=o.total-o.cost; } saveData(); displayCustomers(); displayInvoices(); loadMonthlyReport(); updateSummary(); showToast('Invoice updated'); } closeEditModal(); }
function closeEditModal() { document.getElementById('editModal').style.display='none'; }
function deleteInvoice(invNo) { if(!isAdmin) return showToast('🔒 Login first!'); if(confirm('Delete this invoice permanently?')){ orders=orders.filter(o=>o.invoiceNo!==invNo); customers.forEach(c=>{ c.invoices=c.invoices.filter(i=>i.invoiceNo!==invNo); }); customers=customers.filter(c=>c.invoices.length>0); saveData(); displayCustomers(); displayInvoices(); loadMonthlyReport(); loadDailyReport(); updateSummary(); showToast('Invoice deleted'); } }

// ==================== SUMMARY ==================
function updateSummary() {
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('summaryDate').textContent = today;
    document.getElementById('monthlyDate').textContent = today.substring(0,7);
    
    const todaySales = orders.filter(o => o.date.substring(0,10) === today);
    const todayCash = todaySales.filter(o => o.status === 'paid' || o.paid >= o.total).reduce((s,o) => s + o.total, 0);
    const todayUdhar = todaySales.filter(o => o.status === 'unpaid' || o.status === 'partial').reduce((s,o) => s + (o.total - (o.paid||0)), 0);
    const todayIkhrajat = 0; // Will be added later
    
    // Sabqa Wusool (previous days' received)
    const prevSales = orders.filter(o => o.date.substring(0,10) < today);
    const sabqaWusool = prevSales.reduce((s,o) => s + (o.paid||0), 0);
    
    // Advance Wusool (today's received)
    const advanceWusool = todaySales.reduce((s,o) => s + (o.paid||0), 0);
    
    const totalJamma = todayCash + sabqaWusool + advanceWusool;
    const paymentMinus = totalJamma < todayIkhrajat ? todayIkhrajat - totalJamma : 0;
    
    document.getElementById('sumTodaySale').textContent = formatCurrency(todayCash + todayUdhar);
    document.getElementById('sumUdharSale').textContent = formatCurrency(todayUdhar);
    document.getElementById('sumIkhrajat').textContent = formatCurrency(todayIkhrajat);
    document.getElementById('sumSabqaWusool').textContent = formatCurrency(sabqaWusool);
    document.getElementById('sumAdvanceWusool').textContent = formatCurrency(advanceWusool);
    document.getElementById('sumTotalJamma').textContent = formatCurrency(totalJamma);
    document.getElementById('sumPaymentMinus').textContent = formatCurrency(paymentMinus);
    
    const pmBox = document.getElementById('paymentMinusBox');
    pmBox.className = 'summary-box ' + (paymentMinus > 0 ? 'negative' : 'positive');
    
    // Opening/Closing Stock
    calculateStockSummary();
    
    // Monthly Summary
    const month = today.substring(0,7);
    const monthSales = orders.filter(o => o.date.substring(0,7) === month);
    const monthData = {};
    monthSales.forEach(o => {
        if (!monthData[o.date.substring(0,10)]) monthData[o.date.substring(0,10)] = { sale: 0, udhar: 0, ikhrajat: 0 };
        if (o.status === 'paid' || o.paid >= o.total) monthData[o.date.substring(0,10)].sale += o.total;
        else monthData[o.date.substring(0,10)].udhar += (o.total - (o.paid||0));
    });
    let mhtml = '';
    Object.keys(monthData).sort().forEach(d => {
        const data = monthData[d];
        const total = data.sale + data.udhar;
        mhtml += `<tr><td>${d}</td><td>${formatCurrency(data.sale)}</td><td>${formatCurrency(data.udhar)}</td><td>${formatCurrency(data.ikhrajat)}</td><td>${formatCurrency(total)}</td></tr>`;
    });
    document.getElementById('monthlySummaryBody').innerHTML = mhtml || '<tr><td colspan="5">No data</td></tr>';
}

function calculateStockSummary() {
    const today = new Date().toISOString().split('T')[0];
    const container = document.getElementById('stockSummaryDisplay');
    let html = '<div style="display:grid;grid-template-columns:1fr 1fr 1fr 1fr 1fr 1fr;font-weight:bold;border-bottom:2px solid #ccc;padding:4px 0;"><span>Item</span><span>Opening</span><span>In</span><span>Out</span><span>Sold</span><span>Closing</span></div>';
    
    products.forEach(item => {
        const todayIn = orders.filter(o => o.date.substring(0,10) === today).reduce((s,o) => {
            let total = 0;
            o.items.forEach(i => { if(i.name === item.name) total += parseFloat(i.qty) || 0; });
            return s + total;
        }, 0);
        const opening = item.stock + todayIn; // Simplified
        const closing = item.stock;
        html += `<div style="display:grid;grid-template-columns:1fr 1fr 1fr 1fr 1fr 1fr;padding:3px 0;border-bottom:1px solid #eee;font-size:11px;">
            <span>${item.name}</span>
            <span>${opening}</span>
            <span style="color:#27ae60;">0</span>
            <span style="color:#e74c3c;">0</span>
            <span style="color:#f39c12;">${todayIn}</span>
            <span style="font-weight:bold;">${closing}</span>
        </div>`;
    });
    container.innerHTML = html;
}

// ==================== REPORTS ==================
function loadDailyReport() {
    let date = document.getElementById('dailyDate').value;
    if(!date){ let d = new Date(); date = d.toISOString().slice(0,10); document.getElementById('dailyDate').value = date; }
    let dayOrders = orders.filter(o => o.date.slice(0,10) === date);
    let total = dayOrders.reduce((s,o) => s + o.total, 0);
    let profit = dayOrders.reduce((s,o) => s + o.profit, 0);
    document.getElementById('dailyTotalSales').innerHTML = formatCurrency(total);
    document.getElementById('dailyTotalProfit').innerHTML = formatCurrency(profit);
    document.getElementById('dailyTotalOrders').innerHTML = dayOrders.length;
    let container = document.getElementById('dailyList'); container.innerHTML = '';
    if(dayOrders.length === 0){ container.innerHTML = '<div style="text-align:center;padding:30px;color:#666;">No orders found</div>'; return; }
    dayOrders.forEach(o => { let div = document.createElement('div'); div.className = 'daily-card'; div.innerHTML = `<div style="display:flex;justify-content:space-between;flex-wrap:wrap"><div><strong>${o.invoiceNo}</strong><br><small>${new Date(o.date).toLocaleTimeString()}</small></div><div><strong>${formatCurrency(o.total)}</strong><br>Paid: ${formatCurrency(o.paid||0)}</div></div><div>Customer: ${o.customer}</div><div class="action-btns" style="margin-top:6px"><button onclick="viewInvoice('${o.invoiceNo}')" class="btn-info" style="padding:2px 8px">View</button><button onclick="openPayment('${o.invoiceNo}')" class="btn-success" style="padding:2px 8px">Pay</button><button onclick="shareInvoiceFromInv('${o.invoiceNo}')" class="btn-whatsapp" style="padding:2px 8px">Share</button></div>`; container.appendChild(div); });
}
function loadMonthlyReport() {
    let m = document.getElementById('monthPicker').value;
    if(!m){ let d = new Date(); m = d.toISOString().slice(0,7); document.getElementById('monthPicker').value = m; }
    let filtered = orders.filter(o => o.date.substring(0,7) === m);
    let totalSales = filtered.reduce((s,o) => s + o.total, 0);
    let totalProfit = filtered.reduce((s,o) => s + o.profit, 0);
    let totalOrders = filtered.length;
    let avgOrder = totalOrders > 0 ? totalSales / totalOrders : 0;
    document.getElementById('monthlyTotalSales').innerHTML = formatCurrency(totalSales);
    document.getElementById('monthlyTotalProfit').innerHTML = formatCurrency(totalProfit);
    document.getElementById('monthlyTotalOrders').innerHTML = totalOrders;
    document.getElementById('monthlyAvgOrder').innerHTML = formatCurrency(avgOrder);
    let tb = document.getElementById('monthlyBody'); tb.innerHTML = '';
    if(filtered.length === 0){ tb.innerHTML = '<tr><td colspan="9" style="text-align:center">No orders found</td></tr>'; return; }
    filtered.forEach(o => { let r = tb.insertRow(); r.insertCell(0).innerHTML = new Date(o.date).toLocaleDateString(); r.insertCell(1).innerHTML = o.invoiceNo; r.insertCell(2).innerHTML = o.customer; r.insertCell(3).innerHTML = o.phone || '-'; r.insertCell(4).innerHTML = o.items.length; r.insertCell(5).innerHTML = formatCurrency(o.total); r.insertCell(6).innerHTML = formatCurrency(o.profit); r.insertCell(7).innerHTML = `<span class="badge badge-${o.status}">${o.status}</span>`; let c = r.insertCell(8); c.innerHTML = `<div class="action-btns"><button onclick="viewInvoice('${o.invoiceNo}')" class="btn-info" style="padding:2px 8px">View</button><button onclick="editInvoice('${o.invoiceNo}')" class="btn-warning" style="padding:2px 8px">Edit</button><button onclick="openPayment('${o.invoiceNo}')" class="btn-success" style="padding:2px 8px">Pay</button><button onclick="shareInvoiceFromInv('${o.invoiceNo}')" class="btn-whatsapp" style="padding:2px 8px">Share</button></div>`; });
}

// ==================== INVOICE VIEW & SHARE ==================
function viewInvoice(invNo) {
    let o=orders.find(x=>x.invoiceNo===invNo); if(!o) return;
    let items='<table style="width:100%"><thead><tr><th>Item</th><th>Size</th><th>Qty</th><th>Price</th><th>Total</th></tr></thead><tbody>';
    o.items.forEach(i=>{ items+=`<tr><td style="padding:6px">${i.name}</td><td style="padding:6px">${i.size}</td><td style="padding:6px">${i.qty}</td><td style="padding:6px">${formatCurrency(i.price)}</td><td style="padding:6px">${formatCurrency(i.total)}</td></tr>`; });
    items+='</tbody></table>';
    let rem=o.total-(o.paid||0);
    let html=`<div id="invoicePrint" class="invoice-box"><div class="header"><h2>🏢 ${company.name}</h2><p>${company.tagline}</p><p>${company.address} | ${company.phone}</p></div><div class="body"><div style="margin-bottom:15px"><strong>Invoice:</strong> ${o.invoiceNo}<br><strong>Date:</strong> ${new Date(o.date).toLocaleString()}<br><strong>Customer:</strong> ${o.customer}<br><strong>Phone:</strong> ${o.phone}</div>${items}<div style="text-align:right;margin-top:15px"><h3>Total: ${formatCurrency(o.total)}</h3><p>Paid: ${formatCurrency(o.paid||0)}</p><p><strong>${rem>0?`Due from ${o.customer}: ${formatCurrency(rem)}`: (rem<0?`Refund to ${o.customer}: ${formatCurrency(Math.abs(rem))}`:'Fully Paid ✓')}</strong></p></div></div></div>`;
    if(currentLang==='ar'){ html=html.replace('Item','المنتج').replace('Size','المقاس').replace('Qty','الكمية').replace('Price','السعر').replace('Total','الإجمالي').replace('Invoice','الفاتورة').replace('Date','التاريخ').replace('Customer','العميل').replace('Phone','الجوال'); }
    document.getElementById('invoiceDetails').innerHTML=html;
    document.getElementById('invoiceModal').style.display='flex';
    currentInvoiceNo=invNo;
}
async function downloadPDF() { if(!currentInvoiceNo) return; let o=orders.find(x=>x.invoiceNo===currentInvoiceNo); if(!o) return; let items=''; o.items.forEach(i=>{ items+=`<tr><td style="padding:5px">${i.name}</td><td style="padding:5px">${i.size}</td><td style="padding:5px">${i.qty}</td><td style="padding:5px">${formatCurrency(i.price)}</td><td style="padding:5px">${formatCurrency(i.total)}</td></tr>`; }); let html=`<div style="font-family:Arial;max-width:600px;margin:auto;border:2px solid #ffd700;border-radius:10px"><div style="background:linear-gradient(135deg,#0f2027,#203a43);color:#ffd700;padding:15px;text-align:center"><h2>🏢 ${company.name}</h2><p>${company.tagline}</p></div><div style="padding:15px"><div><strong>Invoice:</strong> ${o.invoiceNo}<br><strong>Date:</strong> ${new Date(o.date).toLocaleString()}<br><strong>Customer:</strong> ${o.customer}</div><table style="width:100%;margin-top:10px"><thead><tr style="background:#1a1a2e;color:#ffd700"><th>Item</th><th>Size</th><th>Qty</th><th>Price</th><th>Total</th></tr></thead><tbody>${items}</tbody></table><div style="text-align:right;margin-top:10px"><h3>Total: ${formatCurrency(o.total)}</h3></div></div></div>`;
    let tpl=document.getElementById('pdfTemplate'); tpl.innerHTML=html;
    try{ let canvas=await html2canvas(tpl,{scale:2}); let img=canvas.toDataURL('image/png'); let {jsPDF}=window.jspdf; let doc=new jsPDF('p','mm','a4'); doc.addImage(img,'PNG',10,0,190,(canvas.height*190)/canvas.width); doc.save(`${company.name}_${o.invoiceNo}.pdf`); showToast('PDF Downloaded'); }catch(e){ showToast('Error generating PDF'); } }
async function shareInvoiceImage() { let el=document.getElementById('invoicePrint'); if(!el) return; let canvas=await html2canvas(el,{scale:2}); let img=canvas.toDataURL('image/png'); let w=window.open('','_blank'); w.document.write(`<html><body style="margin:0"><img src="${img}" style="max-width:100%"></body></html>`); w.document.close(); showToast('Image ready - share from new tab'); }
async function shareInvoiceFromInv(invNo) { let o=orders.find(x=>x.invoiceNo===invNo); if(!o) return; let items=''; o.items.forEach(i=>{ items+=`<tr><td style="padding:3px">${i.name}</td><td style="padding:3px">${i.size}</td><td style="padding:3px">${i.qty}</td><td style="padding:3px">${formatCurrency(i.price)}</td><td style="padding:3px">${formatCurrency(i.total)}</td></tr>`; }); let div=document.createElement('div'); div.style.cssText='font-family:Arial;background:white;padding:10px;border:2px solid #ffd700;border-radius:10px;max-width:500px'; div.innerHTML=`<div style="background:#1a1a2e;color:#ffd700;padding:8px;text-align:center"><strong>🏢 ${company.name}</strong></div><div style="padding:8px"><strong>Invoice:</strong> ${o.invoiceNo}<br><strong>Date:</strong> ${new Date(o.date).toLocaleString()}<br><strong>Customer:</strong> ${o.customer}</div><table style="width:100%"><thead><tr style="background:#ffd700"><th>Item</th><th>Size</th><th>Qty</th><th>Price</th><th>Total</th></tr></thead><tbody>${items}</tbody></table><div style="text-align:right;margin-top:8px"><strong>Total: ${formatCurrency(o.total)}</strong><br>Paid: ${formatCurrency(o.paid||0)}<br>Balance: ${formatCurrency(o.total-(o.paid||0))}</div>`;
    document.body.appendChild(div); let canvas=await html2canvas(div,{scale:2}); let img=canvas.toDataURL('image/png'); document.body.removeChild(div); let w=window.open('','_blank'); w.document.write(`<html><body style="margin:0"><img src="${img}" style="max-width:100%"></body></html>`); w.document.close(); showToast('Image ready'); }
function sendWhatsApp() { let o=orders.find(x=>x.invoiceNo===currentInvoiceNo); if(!o || !o.phone){ showToast('No phone number'); return; } let p=o.phone.replace(/\D/g,''); if(p.startsWith('05')) p='966'+p.substring(1); let msg=`${company.name} Invoice ${o.invoiceNo}%0ATotal: ${formatCurrency(o.total)}%0APaid: ${formatCurrency(o.paid||0)}%0ABalance: ${formatCurrency(o.total-(o.paid||0))}%0A${company.tagline}`; window.open(`https://wa.me/${p}?text=${msg}`); }
function printInvoice() { let w=window.open('','_blank'); w.document.write(`<html><head><title>Invoice</title><style>body{padding:20px} table{width:100%} th,td{padding:8px;border-bottom:1px solid #ddd} th{background:#1a1a2e;color:#ffd700}</style></head><body>${document.getElementById('invoiceDetails').innerHTML}</body></html>`); w.document.close(); w.print(); }
function closeModal() { document.getElementById('invoiceModal').style.display='none'; }
async function generateCartPDF() { if(cart.length===0){ showToast('Cart empty'); return; } let cust=document.getElementById('customerName').value; let sub=cart.reduce((s,i)=>s+i.total,0); let tax=taxEnabled?sub*taxRate/100:0; let total=sub+tax; let items=''; cart.forEach(i=>{ items+=`<tr><td style="padding:5px">${i.name}</td><td style="padding:5px">${i.size}</td><td style="padding:5px">${i.qty}</td><td style="padding:5px">${formatCurrency(i.price)}</td><td style="padding:5px">${formatCurrency(i.total)}</td></tr>`; }); let html=`<div style="font-family:Arial;max-width:600px;margin:auto;border:2px solid #ffd700;border-radius:10px"><div style="background:#1a1a2e;color:#ffd700;padding:12px;text-align:center"><h2>🏢 ${company.name} - Cart Preview</h2></div><div style="padding:15px"><p><strong>Customer:</strong> ${cust}</p><table style="width:100%"><thead><tr style="background:#ffd700"><th>Item</th><th>Size</th><th>Qty</th><th>Price</th><th>Total</th></tr></thead><tbody>${items}</tbody></table><div style="text-align:right;margin-top:10px"><h3>Total: ${formatCurrency(total)}</h3></div></div></div>`;
    let tpl=document.getElementById('pdfTemplate'); tpl.innerHTML=html;
    try{ let canvas=await html2canvas(tpl,{scale:2}); let img=canvas.toDataURL('image/png'); let {jsPDF}=window.jspdf; let doc=new jsPDF('p','mm','a4'); doc.addImage(img,'PNG',10,0,190,(canvas.height*190)/canvas.width); doc.save(`${company.name}_Cart.pdf`); showToast('PDF Downloaded'); }catch(e){ showToast('Error'); } }

// ==================== SETTINGS ==================
function saveCompanySettings() { company.name=document.getElementById('companyNameSetting').value; company.tagline=document.getElementById('companyTaglineSetting').value; company.address=document.getElementById('companyAddressSetting').value; company.phone=document.getElementById('companyPhoneSetting').value; saveData(); document.getElementById('companyName').innerHTML=`🏢 ${company.name}`; document.getElementById('companyTagline').innerHTML=company.tagline; showToast('Company settings saved'); }
function saveTaxSettings() { taxEnabled=document.getElementById('taxEnableCheck').checked; taxRate=parseFloat(document.getElementById('taxRateSetting').value); taxName=document.getElementById('taxNameSetting').value; saveData(); document.getElementById('taxRateDisplay').innerHTML=taxRate; updateCartDisplay(); showToast('Tax settings saved'); }
function backupData() { let data={products,orders,customers,taxEnabled,taxRate,taxName,company}; let a=document.createElement('a'); a.href=URL.createObjectURL(new Blob([JSON.stringify(data)],{type:'application/json'})); a.download=`backup_${new Date().toISOString().slice(0,19)}.json`; a.click(); showToast('Backup created'); }
function restoreData(file) { let r=new FileReader(); r.onload=e=>{ try{ let d=JSON.parse(e.target.result); if(d.products) products=d.products; if(d.orders) orders=d.orders; if(d.customers) customers=d.customers; if(d.taxEnabled!==undefined) taxEnabled=d.taxEnabled; if(d.taxRate) taxRate=d.taxRate; if(d.taxName) taxName=d.taxName; if(d.company) company=d.company; saveData(); location.reload(); } catch(err){ showToast('Invalid backup file'); } }; r.readAsText(file.files[0]); }
function resetAll() { if(prompt('Type CONFIRM to delete ALL data:')==='CONFIRM'){ localStorage.clear(); location.reload(); } }

// ==================== FIREBASE SYNC ==================
function syncToCloud() {
    const data = { products, orders, customers, taxEnabled, taxRate, taxName, company };
    database.ref('warda_data').set(data).then(() => {
        showToast('✅ Synced to cloud!');
    }).catch(err => {
        showToast('❌ Sync failed!');
        console.error(err);
    });
}
function syncFromCloud() {
    database.ref('warda_data').once('value').then(snapshot => {
        const data = snapshot.val();
        if (data) {
            if (data.products) products = data.products;
            if (data.orders) orders = data.orders;
            if (data.customers) customers = data.customers;
            if (data.taxEnabled !== undefined) taxEnabled = data.taxEnabled;
            if (data.taxRate) taxRate = data.taxRate;
            if (data.taxName) taxName = data.taxName;
            if (data.company) company = data.company;
            saveData();
            location.reload();
            showToast('✅ Synced from cloud!');
        } else {
            showToast('❌ No data in cloud');
        }
    }).catch(err => {
        showToast('❌ Sync failed!');
        console.error(err);
    });
}

// ==================== INIT ==================
loadData(); displayProducts(); loadInventory(); updateCartDisplay(); displayCustomers(); displayInvoices(); loadMonthlyReport(); loadDailyReport(); updateSummary();
document.getElementById('monthPicker').value=new Date().toISOString().slice(0,7);
document.getElementById('dailyDate').value=new Date().toISOString().slice(0,10);
document.getElementById('companyNameSetting').value=company.name;
document.getElementById('companyTaglineSetting').value=company.tagline;
document.getElementById('companyAddressSetting').value=company.address;
document.getElementById('companyPhoneSetting').value=company.phone;
document.getElementById('taxEnableCheck').checked=taxEnabled;
document.getElementById('taxRateSetting').value=taxRate;
document.getElementById('taxNameSetting').value=taxName;
document.getElementById('taxRateDisplay').innerHTML=taxRate;

// Show login on load
document.getElementById('loginOverlay').style.display = 'flex';
document.getElementById('appContainer').classList.add('view-only');

window.openPayment=openPayment; window.viewInvoice=viewInvoice; window.deleteInvoice=deleteInvoice; window.editInvoice=editInvoice; window.shareInvoiceFromInv=shareInvoiceFromInv;
</script>
</body>
</html>
