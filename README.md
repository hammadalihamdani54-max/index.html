<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Warda Marble Tabuk</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
    <style>
        *{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma,Geneva,Verdana,sans-serif}
        html,body{height:100%;overflow:hidden;background:#eef2f7}
        body{padding:6px;padding-bottom:60px}
        .app-container{max-width:100%;height:100%;margin:0 auto;background:white;border-radius:14px;overflow:hidden;padding:6px;display:flex;flex-direction:column;box-shadow:0 2px 16px rgba(0,0,0,0.06)}
        .header{background:linear-gradient(135deg,#0f1a2e,#1a2a4a);color:white;padding:6px 12px;border-radius:10px;display:flex;justify-content:space-between;align-items:center;flex-shrink:0;min-height:38px}
        .header h1{font-size:14px;font-weight:700}
        .header-controls{display:flex;gap:4px;align-items:center}
        .header-controls button{background:rgba(255,255,255,0.1);border:none;color:white;padding:3px 8px;border-radius:16px;cursor:pointer;font-size:9px;font-weight:600;transition:0.2s;display:flex;align-items:center;gap:3px}
        .header-controls button:hover{background:rgba(255,255,255,0.2)}
        .header-controls button:active{transform:scale(0.93)}
        .settings-btn{background:rgba(255,255,255,0.08);padding:4px 10px;border-radius:50%;font-size:14px}
        .settings-btn:hover{background:rgba(255,255,255,0.2);transform:rotate(90deg)}
        .admin-badge{background:#d4a017;color:white;padding:1px 8px;border-radius:12px;font-size:7px;font-weight:600}
        .viewer-badge{background:#6b7a8a;color:white;padding:1px 8px;border-radius:12px;font-size:7px;font-weight:600}
        .tabs{display:flex;gap:2px;margin:4px 0;flex-shrink:0;background:#f0f4f8;padding:2px;border-radius:8px;flex-wrap:wrap}
        .tab{flex:1;padding:4px 2px;text-align:center;border-radius:6px;font-weight:600;font-size:7px;cursor:pointer;transition:0.2s;min-width:32px;color:#6b7a8a;background:transparent}
        .tab i{display:block;font-size:13px;margin-bottom:1px}
        .tab.active{background:#0f1a2e;color:white}
        .section{display:none;flex:1;overflow:hidden;padding-top:3px}
        .section.active{display:flex;flex-direction:column}
        .card{background:white;border-radius:8px;padding:5px 6px;margin-bottom:3px;border:1px solid #e8ecf0;flex-shrink:0}
        .card-title{font-size:10px;font-weight:700;color:#0f1a2e;display:flex;align-items:center;gap:4px;border-bottom:1px solid #eef2f6;padding-bottom:3px;margin-bottom:4px}
        .pos-grid{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:3px;max-height:100px;overflow-y:auto;padding:2px 0}
        .pos-item{background:#f7f9fb;border:1px solid #e2e6ea;border-radius:6px;padding:4px 2px;text-align:center;cursor:pointer;transition:0.15s;font-size:8px}
        .pos-item:active{transform:scale(0.94);background:#edf1f5}
        .pos-item .name{font-weight:600;color:#1a2a3a;font-size:9px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
        .pos-item .price{color:#0f1a2e;font-weight:700;font-size:10px}
        .pos-item .stock{font-size:6px;color:#8a9aaa}
        .pos-item.out-of-stock{opacity:0.4;pointer-events:none}
        .cart-area{flex:1;display:flex;flex-direction:column;min-height:0}
        .cart-items{flex:1;overflow-y:auto;padding:2px 0;max-height:80px}
        .cart-item{display:flex;justify-content:space-between;padding:3px 0;border-bottom:1px solid #eef2f6;font-size:9px;align-items:center}
        .cart-item .qty-control{display:flex;gap:2px;align-items:center}
        .cart-item .qty-control button{background:#e8ecf0;border:none;border-radius:50%;width:20px;height:20px;font-weight:700;cursor:pointer;font-size:10px;transition:0.15s}
        .cart-item .qty-control button:active{transform:scale(0.88);background:#d0d6dc}
        .total-bar{background:linear-gradient(135deg,#0f1a2e,#1a2a4a);color:white;padding:5px 10px;border-radius:6px;display:flex;justify-content:space-between;font-weight:700;font-size:13px;flex-shrink:0}
        .bottom-actions{display:grid;grid-template-columns:1fr 1fr 1fr;gap:2px;flex-shrink:0;padding-top:3px}
        .bottom-actions .btn{padding:4px;font-size:8px}
        .btn{padding:4px 8px;border:none;border-radius:5px;font-size:9px;font-weight:600;cursor:pointer;transition:0.15s;display:flex;align-items:center;justify-content:center;gap:3px;width:100%}
        .btn:active{transform:scale(0.94)}
        .btn-primary{background:#0f1a2e;color:white}
        .btn-success{background:#218838;color:white}
        .btn-danger{background:#c0392b;color:white}
        .btn-warning{background:#d4a017;color:white}
        .btn-outline{background:transparent;border:1px solid #0f1a2e;color:#0f1a2e}
        .btn-sm{padding:1px 5px;font-size:7px}
        .grid-2{display:grid;grid-template-columns:1fr 1fr;gap:3px}
        .grid-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:2px}
        .form-group{margin-bottom:2px}
        .form-group label{display:block;font-size:8px;font-weight:600;color:#2d3748;margin-bottom:1px}
        .form-group input,.form-group select{width:100%;padding:3px 5px;border:1px solid #e2e6ea;border-radius:5px;font-size:9px;background:#f7f9fb}
        .form-group input:focus{border-color:#0f1a2e;outline:none;background:white}
        .table-wrap{overflow:auto;flex:1;min-height:0}
        table{width:100%;border-collapse:collapse;font-size:8px}
        th{background:#0f1a2e;color:white;padding:3px 2px;text-align:center;font-size:7px;position:sticky;top:0;z-index:2}
        td{padding:3px 2px;border-bottom:1px solid #eef2f6;text-align:center;font-size:8px}
        tr:nth-child(even){background:#f8fafc}
        .badge{display:inline-block;padding:1px 5px;border-radius:10px;font-size:6px;font-weight:600}
        .badge-success{background:#d4edda;color:#155724}
        .badge-danger{background:#f8d7da;color:#721c24}
        .badge-warning{background:#fff3cd;color:#856404}
        .bottom-nav{position:fixed;bottom:0;left:0;right:0;background:white;display:flex;justify-content:space-around;padding:3px 0;box-shadow:0 -1px 10px rgba(0,0,0,0.06);border-top:1px solid #eef2f6;z-index:1000}
        .bottom-nav .nav-item{text-align:center;font-size:6px;color:#8a9aaa;cursor:pointer;padding:2px 5px;border-radius:5px;transition:0.15s}
        .bottom-nav .nav-item i{font-size:14px;display:block}
        .bottom-nav .nav-item.active{color:#0f1a2e;font-weight:700}
        .scroll-area{flex:1;overflow-y:auto;min-height:0;padding-right:2px}
        .sync-status{font-size:7px;color:rgba(255,255,255,0.6);display:flex;align-items:center;gap:3px}
        .dot{width:6px;height:6px;border-radius:50%;display:inline-block}
        .dot.green{background:#4ade80}
        .dot.red{background:#f87171}
        .dot.yellow{background:#fbbf24;animation:blink 1s infinite}
        @keyframes blink{0%,100%{opacity:1}50%{opacity:0.3}}
        .view-only .btn,.view-only .pos-item,.view-only input,.view-only select{opacity:0.5;pointer-events:none}
        .cat-tabs{display:flex;gap:2px;margin-bottom:2px;flex-wrap:wrap}
        .cat-tab{padding:2px 8px;border-radius:4px;font-weight:600;font-size:7px;cursor:pointer;background:#e8ecf0;color:#6b7a8a;border:none;flex:1;transition:0.15s}
        .cat-tab.active{color:white}
        .cat-tab.stone-active{background:#8B7355;color:white}
        .cat-tab.material-active{background:#2b6cb0;color:white}
        .edit-btn{background:#e8ecf0;border:none;padding:1px 5px;border-radius:3px;cursor:pointer;font-size:7px}
        .edit-btn:hover{background:#d0d6dc}
        .invoice-box{background:#f8fafc;border:1px solid #e2e6ea;border-radius:6px;padding:8px;font-size:9px;font-family:monospace;white-space:pre-wrap;max-height:160px;overflow-y:auto}
        .modal-overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.5);display:flex;align-items:center;justify-content:center;z-index:9999;backdrop-filter:blur(3px)}
        .modal-box{background:white;padding:16px;border-radius:12px;max-width:340px;width:92%;max-height:80vh;overflow-y:auto;box-shadow:0 16px 48px rgba(0,0,0,0.2)}
        .modal-box h2{font-size:16px;color:#0f1a2e;margin-bottom:10px;display:flex;align-items:center;gap:6px;border-bottom:2px solid #eef2f6;padding-bottom:8px}
        .modal-box .modal-close{float:right;background:none;border:none;font-size:18px;cursor:pointer;color:#8a9aaa}
        .modal-box .modal-close:hover{color:#0f1a2e}
        .modal-box .setting-item{padding:8px 0;border-bottom:1px solid #eef2f6;display:flex;justify-content:space-between;align-items:center}
        .modal-box .setting-item:last-child{border-bottom:none}
        .modal-box .setting-item .label{font-size:12px;font-weight:600;color:#1a2a3a}
        .modal-box .setting-item .desc{font-size:9px;color:#8a9aaa}
        .modal-box input[type="password"]{width:100%;padding:6px 10px;border:1.5px solid #e2e6ea;border-radius:6px;font-size:12px;margin:3px 0}
        .modal-box input[type="password"]:focus{border-color:#0f1a2e;outline:none}
        .modal-box .btn{width:100%;padding:6px;border:none;border-radius:6px;font-size:12px;font-weight:600;cursor:pointer;margin-top:2px}
        .modal-box .error{color:#c0392b;font-size:10px;margin-top:3px;display:none}
        .sessions-box{background:#f8fafc;border:1px solid #e2e6ea;border-radius:6px;padding:6px;margin-top:3px;font-size:8px}
        .sessions-box .session-item{display:flex;justify-content:space-between;padding:2px 0;border-bottom:1px solid #eef2f6;align-items:center}
        .sessions-box .session-item:last-child{border-bottom:none}
        @media(max-width:480px){.pos-grid{grid-template-columns:1fr 1fr 1fr}.header h1{font-size:12px}.bottom-actions{grid-template-columns:1fr 1fr}}
        @media(min-width:768px){.pos-grid{grid-template-columns:1fr 1fr 1fr 1fr 1fr}}
    </style>
</head>
<body>
<div class="app-container">
    <!-- Header -->
    <div class="header">
        <h1><i class="fas fa-store"></i> <span id="shopName">Warda Marble</span></h1>
        <div class="header-controls">
            <span class="sync-status"><span class="dot green" id="statusDot"></span><span id="statusText">Online</span></span>
            <span id="roleBadge" class="viewer-badge">👁️ Viewer</span>
            <button class="settings-btn" onclick="toggleSettings()" title="Settings"><i class="fas fa-cog"></i></button>
        </div>
    </div>

    <!-- Tabs -->
    <div class="tabs">
        <div class="tab active" data-tab="pos"><i class="fas fa-shopping-cart"></i> POS</div>
        <div class="tab" data-tab="stock"><i class="fas fa-boxes"></i> Stock</div>
        <div class="tab" data-tab="customers"><i class="fas fa-users"></i> Cust</div>
        <div class="tab" data-tab="reports"><i class="fas fa-chart-pie"></i> Report</div>
        <div class="tab" data-tab="details"><i class="fas fa-list-ul"></i> Details</div>
    </div>

    <!-- ========== POS ========== -->
    <div id="section-pos" class="section active">
        <div class="card" style="flex-shrink:0;padding:4px;">
            <div class="card-title" style="font-size:9px;margin-bottom:2px;">
                <i class="fas fa-list"></i> Items
                <span style="margin-right:auto;"></span>
                <span class="badge" style="background:#8B7355;color:white;">Stone</span>
                <span class="badge" style="background:#2b6cb0;color:white;">Material</span>
            </div>
            <div class="cat-tabs">
                <button class="cat-tab stone-active active" onclick="filterCategory('stone')" id="catStone">Stone</button>
                <button class="cat-tab material-active" onclick="filterCategory('material')" id="catMaterial">Material</button>
                <button class="cat-tab" onclick="filterCategory('all')" id="catAll">All</button>
            </div>
            <div class="pos-grid" id="posGrid"></div>
        </div>

        <div class="cart-area">
            <div class="card" style="flex:1;display:flex;flex-direction:column;padding:4px;margin-bottom:0;">
                <div class="card-title" style="font-size:9px;margin-bottom:2px;">
                    <i class="fas fa-shopping-cart"></i> Cart (<span id="cartCount">0</span>)
                    <span style="margin-right:auto;"></span>
                    <button class="btn btn-sm btn-danger" onclick="clearCart()" style="width:auto;padding:0 6px;font-size:7px;"><i class="fas fa-trash"></i></button>
                </div>
                <div class="cart-items" id="cartItems"></div>
                <div class="total-bar"><span>Total:</span><span id="cartTotal">0.00</span></div>
                <div class="bottom-actions">
                    <select id="saleType" style="padding:3px 4px;border-radius:4px;border:1px solid #e2e6ea;font-size:8px;">
                        <option value="cash">Cash</option>
                        <option value="credit">Credit</option>
                    </select>
                    <select id="saleCustomerSelect" style="padding:3px 4px;border-radius:4px;border:1px solid #e2e6ea;font-size:8px;">
                        <option value="">Walk-in</option>
                    </select>
                    <input type="number" id="salePaid" placeholder="Paid" value="0" style="padding:3px 4px;border-radius:4px;border:1px solid #e2e6ea;font-size:8px;grid-column:1/-1;">
                    <button class="btn btn-success" onclick="completeSale()" style="grid-column:1/-1;padding:4px;font-size:9px;"><i class="fas fa-check"></i> Complete Sale</button>
                </div>
            </div>
        </div>
    </div>

    <!-- ========== STOCK ========== -->
    <div id="section-stock" class="section">
        <div class="scroll-area">
            <div class="card">
                <div class="card-title"><i class="fas fa-plus-circle"></i> Add/Edit Item</div>
                <div class="grid-2">
                    <div class="form-group"><label>Name</label><input type="text" id="itemName" placeholder="Item name"></div>
                    <div class="form-group"><label>Category</label><select id="itemCategory"><option value="stone">Stone</option><option value="material">Material</option></select></div>
                </div>
                <div class="grid-2">
                    <div class="form-group"><label>Unit</label><select id="itemUnit"><option value="m²">m²</option><option value="running_ft">Running ft</option><option value="pcs">Pieces</option></select></div>
                    <div class="form-group"><label>Sell Price</label><input type="number" id="itemSellPrice" placeholder="Sell"></div>
                </div>
                <div class="grid-2">
                    <div class="form-group"><label>Purchase Price</label><input type="number" id="itemPurchasePrice" placeholder="Cost"></div>
                    <div class="form-group"><label>Stock</label><input type="number" id="itemStock" placeholder="Qty"></div>
                </div>
                <div class="grid-3">
                    <button class="btn btn-primary" onclick="addItem()"><i class="fas fa-save"></i> Add</button>
                    <button class="btn btn-warning" onclick="updateItem()"><i class="fas fa-edit"></i> Update</button>
                    <button class="btn btn-danger" onclick="deleteItem()"><i class="fas fa-trash"></i> Delete</button>
                </div>
                <input type="hidden" id="editItemId" value="">
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-list"></i> Stock List</div>
                <div class="table-wrap" style="max-height:110px;">
                    <table><thead><tr><th>Name</th><th>Unit</th><th>Stock</th><th>Price</th><th>Action</th></tr></thead>
                    <tbody id="stockListBody"></tbody></table>
                </div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-arrow-right"></i> Stock In/Out</div>
                <div class="grid-2">
                    <select id="stockSelect" style="padding:3px 4px;border-radius:4px;border:1px solid #e2e6ea;"></select>
                    <input type="number" id="stockQty" placeholder="Qty" style="padding:3px 4px;border-radius:4px;border:1px solid #e2e6ea;">
                </div>
                <div class="grid-2">
                    <button class="btn btn-success" onclick="stockIn()"><i class="fas fa-plus"></i> In</button>
                    <button class="btn btn-danger" onclick="stockOut()"><i class="fas fa-minus"></i> Out</button>
                </div>
            </div>
            
            <div class="card">
                <div class="card-title"><i class="fas fa-clock"></i> Stock History</div>
                <div class="table-wrap" style="max-height:70px;">
                    <table><thead><tr><th>Date</th><th>Item</th><th>Qty</th><th>Type</th></tr></thead>
                    <tbody id="stockHistoryBody"></tbody></table>
                </div>
            </div>
        </div>
    </div>

    <!-- ========== CUSTOMERS ========== -->
    <div id="section-customers" class="section">
        <div class="scroll-area">
            <div class="card">
                <div class="card-title"><i class="fas fa-user-plus"></i> Add Customer</div>
                <div class="grid-2">
                    <div class="form-group"><input type="text" id="custName" placeholder="Name"></div>
                    <div class="form-group"><input type="text" id="custPhone" placeholder="Phone"></div>
                </div>
                <button class="btn btn-primary" onclick="addCustomer()"><i class="fas fa-user-plus"></i> Add</button>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-users"></i> Customers</div>
                <div class="table-wrap" style="max-height:110px;">
                    <table><thead><tr><th>Name</th><th>Phone</th><th>Balance</th></tr></thead>
                    <tbody id="customerListBody"></tbody></table>
                </div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-hand-holding-usd"></i> Receive Payment</div>
                <div class="grid-2">
                    <select id="receiveSelect" style="padding:3px 4px;border-radius:4px;border:1px solid #e2e6ea;"></select>
                    <input type="number" id="receiveAmount" placeholder="Amount" style="padding:3px 4px;border-radius:4px;border:1px solid #e2e6ea;">
                </div>
                <button class="btn btn-success" onclick="receivePayment()"><i class="fas fa-money-bill-wave"></i> Receive</button>
            </div>
        </div>
    </div>

    <!-- ========== REPORTS ========== -->
    <div id="section-reports" class="section">
        <div class="scroll-area">
            <div class="card">
                <div class="card-title"><i class="fas fa-calendar-day"></i> Reports</div>
                <div class="grid-3">
                    <button class="btn btn-primary" onclick="genReport('daily')"><i class="fas fa-eye"></i> Daily</button>
                    <button class="btn btn-warning" onclick="genReport('weekly')"><i class="fas fa-calendar-week"></i> Weekly</button>
                    <button class="btn btn-danger" onclick="genReport('monthly')"><i class="fas fa-calendar-alt"></i> Monthly</button>
                </div>
                <div style="display:flex;gap:2px;margin-top:3px;">
                    <button class="btn btn-success" onclick="printReport()" style="flex:1;"><i class="fas fa-print"></i> Print</button>
                    <button class="btn btn-outline" onclick="shareReport()" style="flex:1;"><i class="fab fa-whatsapp"></i> Share</button>
                </div>
                <div id="reportDisplay" class="invoice-box" style="margin-top:4px;"></div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-chart-line"></i> Profit / Loss</div>
                <div id="plDisplay" style="text-align:center;padding:6px;font-size:14px;font-weight:700;"></div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-history"></i> All Transactions</div>
                <div class="table-wrap" style="max-height:90px;">
                    <table><thead><tr><th>Date</th><th>Desc</th><th>Amount</th></tr></thead>
                    <tbody id="transactionsBody"></tbody></table>
                </div>
            </div>
        </div>
    </div>

    <!-- ========== DETAILS ========== -->
    <div id="section-details" class="section">
        <div class="scroll-area">
            <div class="card">
                <div class="card-title"><i class="fas fa-list-ul"></i> Item Sales Details</div>
                <div class="table-wrap" style="max-height:170px;">
                    <table><thead><tr><th>Item</th><th>Unit</th><th>Sold</th><th>Revenue</th></tr></thead>
                    <tbody id="itemDetailsBody"></tbody></table>
                </div>
            </div>
            <div class="card">
                <div class="card-title"><i class="fas fa-calendar-alt"></i> Monthly Summary</div>
                <div class="table-wrap" style="max-height:110px;">
                    <table><thead><tr><th>Month</th><th>Sales</th><th>Orders</th></tr></thead>
                    <tbody id="monthlySummaryBody"></tbody></table>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- Bottom Nav -->
<div class="bottom-nav">
    <div class="nav-item active" data-tab="pos"><i class="fas fa-shopping-cart"></i> POS</div>
    <div class="nav-item" data-tab="stock"><i class="fas fa-boxes"></i> Stock</div>
    <div class="nav-item" data-tab="customers"><i class="fas fa-users"></i> Cust</div>
    <div class="nav-item" data-tab="reports"><i class="fas fa-chart-pie"></i> Report</div>
    <div class="nav-item" data-tab="details"><i class="fas fa-list-ul"></i> Details</div>
</div>

<!-- ========== SETTINGS MODAL ========== -->
<div id="settingsModal" class="modal-overlay" style="display:none;">
    <div class="modal-box">
        <h2><i class="fas fa-cog"></i> Settings <button class="modal-close" onclick="toggleSettings()">&times;</button></h2>
        
        <div class="setting-item">
            <div><div class="label"><i class="fas fa-user-shield"></i> Admin Access</div><div class="desc">Login to manage system</div></div>
            <span id="settingsStatus" class="badge badge-warning">Viewer</span>
        </div>
        
        <div id="loginSection">
            <input type="password" id="settingsPassword" placeholder="Enter password..." onkeydown="if(event.key==='Enter') settingsLogin()">
            <button class="btn btn-success" onclick="settingsLogin()"><i class="fas fa-sign-in-alt"></i> Login</button>
            <div id="settingsError" class="error">❌ Wrong password!</div>
        </div>
        
        <div id="logoutSection" style="display:none;">
            <button class="btn btn-danger" onclick="settingsLogout()"><i class="fas fa-sign-out-alt"></i> Logout</button>
        </div>
        
        <div class="setting-item" style="margin-top:8px;">
            <div><div class="label"><i class="fas fa-language"></i> Language</div><div class="desc">Switch between English / Urdu</div></div>
            <button class="btn btn-outline" onclick="settingsToggleLang()" style="width:auto;padding:2px 10px;font-size:10px;"><span id="settingsLangLabel">اردو</span></button>
        </div>
        
        <div class="setting-item">
            <div><div class="label"><i class="fas fa-key"></i> Change Password</div><div class="desc">Set new admin password</div></div>
            <div><input type="password" id="newPasswordInput" placeholder="New password..." style="width:130px;padding:3px 6px;border:1px solid #e2e6ea;border-radius:4px;font-size:9px;">
            <button class="btn btn-warning" onclick="changePassword()" style="width:auto;padding:2px 8px;font-size:9px;margin-top:2px;">Change</button></div>
        </div>
        
        <div class="setting-item">
            <div><div class="label"><i class="fas fa-users-cog"></i> Active Sessions</div><div class="desc">Manage logged in devices</div></div>
            <button class="btn btn-outline" onclick="showSessionsInSettings()" style="width:auto;padding:2px 10px;font-size:9px;">View</button>
        </div>
        
        <div id="sessionsInSettings" style="display:none;margin-top:6px;padding:6px;background:#f8fafc;border-radius:6px;border:1px solid #e2e6ea;max-height:120px;overflow-y:auto;">
            <div id="settingsSessionsList">Loading...</div>
        </div>
        
        <div style="text-align:center;font-size:8px;color:#8a9aaa;margin-top:10px;padding-top:8px;border-top:1px solid #eef2f6;">
            Warda Marble v2.0
        </div>
    </div>
</div>

<script>
    // ============================================================
    // FIREBASE
    // ============================================================
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
    // PASSWORD SYSTEM
    // ============================================================
    let ADMIN_PASSWORD = "Hamdani5";
    let isAdmin = false;
    let sessionId = null;

    function getDeviceInfo() {
        const ua = navigator.userAgent;
        if (/Android/i.test(ua)) return 'Android';
        if (/iPhone|iPad|iPod/i.test(ua)) return 'iOS';
        if (/Windows/i.test(ua)) return 'Windows';
        if (/Macintosh/i.test(ua)) return 'Mac';
        return 'Device';
    }

    function getBrowserInfo() {
        const ua = navigator.userAgent;
        if (/Chrome/i.test(ua) && !/Edge/i.test(ua)) return 'Chrome';
        if (/Firefox/i.test(ua)) return 'Firefox';
        if (/Safari/i.test(ua) && !/Chrome/i.test(ua)) return 'Safari';
        if (/Edge/i.test(ua)) return 'Edge';
        return 'Browser';
    }

    function getSessionKey() {
        return 'warda_session_' + window.location.hostname;
    }

    function saveSession() {
        const session = {
            id: sessionId || 'session_' + Date.now(),
            device: getDeviceInfo(),
            browser: getBrowserInfo(),
            time: new Date().toLocaleString(),
            active: true,
            lastSeen: Date.now()
        };
        sessionId = session.id;
        localStorage.setItem(getSessionKey(), JSON.stringify(session));
        syncSessionToCloud();
    }

    function loadSession() {
        const data = localStorage.getItem(getSessionKey());
        if (data) {
            try {
                const session = JSON.parse(data);
                if (session.active) {
                    sessionId = session.id;
                    return session;
                }
            } catch(e) {}
        }
        return null;
    }

    function clearSession() {
        localStorage.removeItem(getSessionKey());
        if (sessionId) {
            database.ref('warda_sessions').child(sessionId).update({ active: false });
        }
    }

    // ============================================================
    // LOGIN / LOGOUT
    // ============================================================
    function settingsLogin() {
        const input = document.getElementById('settingsPassword').value;
        const error = document.getElementById('settingsError');
        if (input === ADMIN_PASSWORD) {
            isAdmin = true;
            if (!sessionId) sessionId = 'session_' + Date.now();
            saveSession();
            
            document.getElementById('roleBadge').textContent = '👑 Admin';
            document.getElementById('roleBadge').className = 'admin-badge';
            document.getElementById('settingsStatus').textContent = 'Admin';
            document.getElementById('settingsStatus').className = 'badge badge-success';
            document.getElementById('loginSection').style.display = 'none';
            document.getElementById('logoutSection').style.display = 'block';
            document.getElementById('settingsPassword').value = '';
            
            enableEditing(true);
            showToast('✅ Admin mode activated!');
            renderAll();
        } else {
            error.style.display = 'block';
            setTimeout(() => error.style.display = 'none', 3000);
        }
    }

    function logout() {
        isAdmin = false;
        clearSession();
        
        document.getElementById('roleBadge').textContent = '👁️ Viewer';
        document.getElementById('roleBadge').className = 'viewer-badge';
        document.getElementById('settingsStatus').textContent = 'Viewer';
        document.getElementById('settingsStatus').className = 'badge badge-warning';
        document.getElementById('loginSection').style.display = 'block';
        document.getElementById('logoutSection').style.display = 'none';
        
        enableEditing(false);
        showToast('👁️ Viewer mode activated');
        renderAll();
    }

    function settingsLogout() {
        logout();
        document.getElementById('settingsModal').style.display = 'none';
    }

    function enableEditing(enable) {
        document.querySelectorAll('.btn, .pos-item, input, select').forEach(el => {
            el.style.opacity = enable ? '1' : '0.5';
            el.style.pointerEvents = enable ? 'auto' : 'none';
        });
    }

    function toggleSettings() {
        const modal = document.getElementById('settingsModal');
        if (modal.style.display === 'flex') {
            modal.style.display = 'none';
        } else {
            modal.style.display = 'flex';
            updateSettingsUI();
            loadSessionsForSettings();
        }
    }

    function updateSettingsUI() {
        if (isAdmin) {
            document.getElementById('settingsStatus').textContent = 'Admin';
            document.getElementById('settingsStatus').className = 'badge badge-success';
            document.getElementById('loginSection').style.display = 'none';
            document.getElementById('logoutSection').style.display = 'block';
        } else {
            document.getElementById('settingsStatus').textContent = 'Viewer';
            document.getElementById('settingsStatus').className = 'badge badge-warning';
            document.getElementById('loginSection').style.display = 'block';
            document.getElementById('logoutSection').style.display = 'none';
            document.getElementById('settingsPassword').value = '';
        }
    }

    function changePassword() {
        if (!isAdmin) return alert('🔒 Login first!');
        const newPass = document.getElementById('newPasswordInput').value.trim();
        if (!newPass || newPass.length < 4) return alert('Password must be at least 4 characters!');
        if (!confirm('Change password to "' + newPass + '"?')) return;
        ADMIN_PASSWORD = newPass;
        document.getElementById('newPasswordInput').value = '';
        showToast('✅ Password changed!');
    }

    // ============================================================
    // SESSIONS
    // ============================================================
    function syncSessionToCloud() {
        if (!sessionId) return;
        database.ref('warda_sessions').child(sessionId).set({
            device: getDeviceInfo(),
            browser: getBrowserInfo(),
            time: new Date().toLocaleString(),
            active: true,
            lastSeen: Date.now()
        });
    }

    function loadSessionsForSettings() {
        database.ref('warda_sessions').once('value').then(snapshot => {
            const data = snapshot.val();
            const container = document.getElementById('settingsSessionsList');
            if (!data) {
                container.innerHTML = '<div style="color:#8a9aaa;font-size:8px;">No active sessions</div>';
                return;
            }
            let html = '';
            Object.keys(data).forEach(key => {
                const s = data[key];
                if (s.active) {
                    const isCurrent = key === sessionId;
                    html += `<div class="session-item" style="font-size:8px;padding:2px 0;">
                        <span>${isCurrent ? '👑 ' : ''}${s.device || 'Unknown'} (${s.browser || 'Browser'})</span>
                        <span>
                            <span class="badge ${isCurrent ? 'badge-success' : 'badge-warning'}">${isCurrent ? 'Current' : 'Active'}</span>
                            ${isAdmin && !isCurrent ? `<button onclick="removeSession('${key}')" class="btn-sm btn-danger" style="padding:0 4px;font-size:6px;">✕</button>` : ''}
                        </span>
                    </div>`;
                }
            });
            container.innerHTML = html || '<div style="color:#8a9aaa;font-size:8px;">No active sessions</div>';
            document.getElementById('sessionsInSettings').style.display = 'block';
        });
    }

    function showSessionsInSettings() {
        const container = document.getElementById('sessionsInSettings');
        if (container.style.display === 'block') {
            container.style.display = 'none';
        } else {
            loadSessionsForSettings();
        }
    }

    function removeSession(sessionKey) {
        if (!isAdmin) return alert('🔒 Admin only!');
        if (!confirm('Remove this session?')) return;
        database.ref('warda_sessions').child(sessionKey).update({ active: false });
        showToast('✅ Session removed!');
        loadSessionsForSettings();
    }

    // ============================================================
    // LANGUAGE
    // ============================================================
    let currentLang = 'en';
    const langMap = {
        en: { shop: 'Warda Marble', cash: 'Cash', credit: 'Credit', walkin: 'Walk-in' },
        ur: { shop: 'وردہ ماربل', cash: 'نقد', credit: 'ادھار', walkin: 'بغیر گاہک' }
    };

    function t(key) { return langMap[currentLang][key] || key; }

    function toggleLang() {
        currentLang = currentLang === 'en' ? 'ur' : 'en';
        document.getElementById('shopName').textContent = t('shop');
        renderAll();
    }

    function settingsToggleLang() {
        toggleLang();
        document.getElementById('settingsLangLabel').textContent = currentLang === 'en' ? 'اردو' : 'English';
    }

    // ============================================================
    // DATABASE
    // ============================================================
    let db = { items: [], customers: [], sales: [], expenses: [], transactions: [], stockHistory: [] };
    let cart = [];
    let currentFilter = 'all';
    let isSyncing = false;

    function loadLocal() {
        const data = localStorage.getItem('warda_data');
        if (data) { const p = JSON.parse(data); db = p; }
    }
    loadLocal();

    function saveLocal() {
        localStorage.setItem('warda_data', JSON.stringify(db));
    }

    function getToday() { return new Date().toISOString().split('T')[0]; }

    // ============================================================
    // SYNC
    // ============================================================
    function updateStatus(s) {
        const d = document.getElementById('statusDot'), t = document.getElementById('statusText');
        if (s === 'online') { d.className = 'dot green'; t.textContent = 'Online'; }
        else if (s === 'offline') { d.className = 'dot red'; t.textContent = 'Offline'; }
        else { d.className = 'dot yellow'; t.textContent = 'Syncing...'; }
    }

    function syncToCloud() {
        if (isSyncing) return;
        isSyncing = true; updateStatus('syncing');
        database.ref('warda_data').set({ ...db, updated: Date.now() }).then(() => {
            updateStatus('online'); isSyncing = false; showToast('✅ Synced!');
        }).catch(() => { updateStatus('offline'); isSyncing = false; showToast('❌ Sync failed!'); });
    }

    function syncFromCloud() {
        if (isSyncing) return;
        isSyncing = true; updateStatus('syncing');
        database.ref('warda_data').once('value').then(s => {
            const data = s.val();
            if (data) { db = data; saveLocal(); renderAll(); showToast('✅ Synced!'); }
            updateStatus('online'); isSyncing = false;
        }).catch(() => { updateStatus('offline'); isSyncing = false; showToast('❌ Sync failed!'); });
    }

    function showToast(msg) {
        const t = document.createElement('div');
        t.style.cssText = 'position:fixed;bottom:65px;left:50%;transform:translateX(-50%);background:#0f1a2e;color:white;padding:4px 14px;border-radius:6px;font-size:10px;z-index:9999;max-width:90%;text-align:center;box-shadow:0 4px 12px rgba(0,0,0,0.15)';
        t.textContent = msg;
        document.body.appendChild(t);
        setTimeout(() => { t.style.opacity = '0'; t.style.transition = 'opacity 0.3s'; setTimeout(() => t.remove(), 300); }, 1800);
    }

    // ============================================================
    // ITEMS
    // ============================================================
    function addItem() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const name = document.getElementById('itemName').value.trim();
        if (!name) return alert('Enter name!');
        const item = {
            id: Date.now(),
            name,
            category: document.getElementById('itemCategory').value,
            unit: document.getElementById('itemUnit').value,
            sellPrice: parseFloat(document.getElementById('itemSellPrice').value) || 0,
            purchasePrice: parseFloat(document.getElementById('itemPurchasePrice').value) || 0,
            stock: parseFloat(document.getElementById('itemStock').value) || 0,
            minStock: 5
        };
        db.items.push(item);
        saveLocal(); syncToCloud(); renderAll();
        clearItemForm();
        showToast('✅ Item added!');
    }

    function updateItem() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const id = parseInt(document.getElementById('editItemId').value);
        if (!id) return alert('Select an item first!');
        const item = db.items.find(i => i.id === id);
        if (!item) return;
        item.name = document.getElementById('itemName').value.trim() || item.name;
        item.category = document.getElementById('itemCategory').value;
        item.unit = document.getElementById('itemUnit').value;
        item.sellPrice = parseFloat(document.getElementById('itemSellPrice').value) || 0;
        item.purchasePrice = parseFloat(document.getElementById('itemPurchasePrice').value) || 0;
        item.stock = parseFloat(document.getElementById('itemStock').value) || 0;
        saveLocal(); syncToCloud(); renderAll();
        clearItemForm();
        showToast('✅ Item updated!');
    }

    function deleteItem() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const id = parseInt(document.getElementById('editItemId').value);
        if (!id) return alert('Select an item first!');
        if (!confirm('Delete this item?')) return;
        db.items = db.items.filter(i => i.id !== id);
        saveLocal(); syncToCloud(); renderAll();
        clearItemForm();
        showToast('✅ Item deleted!');
    }

    function selectItem(id) {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const item = db.items.find(i => i.id === id);
        if (!item) return;
        document.getElementById('editItemId').value = id;
        document.getElementById('itemName').value = item.name;
        document.getElementById('itemCategory').value = item.category;
        document.getElementById('itemUnit').value = item.unit;
        document.getElementById('itemSellPrice').value = item.sellPrice;
        document.getElementById('itemPurchasePrice').value = item.purchasePrice;
        document.getElementById('itemStock').value = item.stock;
    }

    function clearItemForm() {
        document.getElementById('editItemId').value = '';
        document.getElementById('itemName').value = '';
        document.getElementById('itemSellPrice').value = '';
        document.getElementById('itemPurchasePrice').value = '';
        document.getElementById('itemStock').value = '';
    }

    function stockIn() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const id = parseInt(document.getElementById('stockSelect').value);
        const qty = parseFloat(document.getElementById('stockQty').value);
        if (!id || !qty || qty <= 0) return alert('Select item and qty!');
        const item = db.items.find(i => i.id === id);
        if (!item) return;
        item.stock += qty;
        db.stockHistory.push({ date: getToday(), item: item.name, qty: qty, type: 'In' });
        db.transactions.push({ id: Date.now(), type: 'stock_in', desc: `${item.name} +${qty}`, amount: qty * item.purchasePrice, date: getToday() });
        saveLocal(); syncToCloud(); renderAll();
        document.getElementById('stockQty').value = '';
        showToast(`✅ ${qty} added!`);
    }

    function stockOut() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const id = parseInt(document.getElementById('stockSelect').value);
        const qty = parseFloat(document.getElementById('stockQty').value);
        if (!id || !qty || qty <= 0) return alert('Select item and qty!');
        const item = db.items.find(i => i.id === id);
        if (!item) return;
        if (item.stock < qty) return alert('Not enough stock!');
        item.stock -= qty;
        db.stockHistory.push({ date: getToday(), item: item.name, qty: qty, type: 'Out' });
        db.transactions.push({ id: Date.now(), type: 'stock_out', desc: `${item.name} -${qty}`, amount: qty * item.sellPrice, date: getToday() });
        saveLocal(); syncToCloud(); renderAll();
        document.getElementById('stockQty').value = '';
        showToast(`✅ ${qty} removed!`);
    }

    function renderStockList() {
        const tbody = document.getElementById('stockListBody');
        if (!db.items.length) { tbody.innerHTML = '<tr><td colspan="5">No items</td></tr>'; return; }
        let html = '';
        db.items.forEach(item => {
            const warn = item.stock < item.minStock ? '⚠️' : '';
            html += `<tr><td>${item.name}</td><td>${item.unit}</td><td>${item.stock} ${warn}</td><td>${item.sellPrice}</td><td><button class="edit-btn" onclick="selectItem(${item.id})">✏️</button></td></tr>`;
        });
        tbody.innerHTML = html;

        const sel = document.getElementById('stockSelect');
        sel.innerHTML = '';
        db.items.forEach(i => { const o = document.createElement('option'); o.value = i.id; o.textContent = `${i.name} (${i.stock})`; sel.appendChild(o); });

        const htbody = document.getElementById('stockHistoryBody');
        if (!db.stockHistory.length) { htbody.innerHTML = '<tr><td colspan="4">No history</td></tr>'; return; }
        let hhtml = '';
        db.stockHistory.slice(-20).reverse().forEach(h => {
            hhtml += `<tr><td>${h.date}</td><td>${h.item}</td><td>${h.qty}</td><td class="${h.type === 'In' ? 'badge-success' : 'badge-danger'}">${h.type}</td></tr>`;
        });
        htbody.innerHTML = hhtml;
    }

    // ============================================================
    // POS
    // ============================================================
    function filterCategory(cat) {
        currentFilter = cat;
        document.querySelectorAll('.cat-tab').forEach(el => el.classList.remove('active', 'stone-active', 'material-active'));
        if (cat === 'stone') document.getElementById('catStone').classList.add('active', 'stone-active');
        else if (cat === 'material') document.getElementById('catMaterial').classList.add('active', 'material-active');
        else document.getElementById('catAll').classList.add('active');
        renderPOS();
    }

    function renderPOS() {
        const grid = document.getElementById('posGrid');
        let items = db.items;
        if (currentFilter !== 'all') items = items.filter(i => i.category === currentFilter);
        if (!items.length) { grid.innerHTML = '<div style="grid-column:1/-1;text-align:center;color:#8a9aaa;padding:6px;font-size:8px;">No items</div>'; return; }
        let html = '';
        items.forEach(item => {
            const out = item.stock <= 0 ? 'out-of-stock' : '';
            html += `<div class="pos-item ${out}" onclick="${out ? '' : `addToCart(${item.id})`}" style="${out ? 'opacity:0.4;pointer-events:none;' : ''}">
                <div class="name">${item.name}</div>
                <div class="price">${item.sellPrice}</div>
                <div class="stock">${item.stock} ${item.unit}</div>
            </div>`;
        });
        grid.innerHTML = html;
    }

    // ============================================================
    // CART
    // ============================================================
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

    function clearCart() {
        if (!cart.length) return;
        if (!confirm('Clear cart?')) return;
        cart = [];
        renderCart();
    }

    function renderCart() {
        const container = document.getElementById('cartItems');
        if (!cart.length) {
            container.innerHTML = '<div style="text-align:center;color:#8a9aaa;padding:6px;font-size:8px;">Cart empty</div>';
            document.getElementById('cartTotal').textContent = '0.00';
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
                    <span style="min-width:14px;text-align:center;">${c.qty}</span>
                    <button onclick="updateQty(${c.id}, 1)">+</button>
                    <button onclick="removeFromCart(${c.id})" style="background:#f8d7da;border:none;border-radius:50%;width:18px;height:18px;cursor:pointer;font-size:8px;color:#721c24;">✕</button>
                </div>
            </div>`;
        });
        container.innerHTML = html;
        document.getElementById('cartTotal').textContent = total.toFixed(2);
        document.getElementById('cartCount').textContent = cart.reduce((s, c) => s + c.qty, 0);
    }

    // ============================================================
    // COMPLETE SALE
    // ============================================================
    function completeSale() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        if (!cart.length) return alert('Cart empty!');
        const type = document.getElementById('saleType').value;
        const customerId = document.getElementById('saleCustomerSelect').value;
        const paid = parseFloat(document.getElementById('salePaid').value) || 0;
        let total = 0, stoneTotal = 0, materialTotal = 0, items = [];
        let valid = true;
        cart.forEach(c => {
            const item = db.items.find(i => i.id === c.id);
            if (!item || item.stock < c.qty) { alert(`${c.name} not enough stock!`); valid = false; return; }
            const subtotal = c.qty * c.price;
            total += subtotal;
            if (item.category === 'stone') stoneTotal += subtotal;
            else materialTotal += subtotal;
            items.push({ id: c.id, name: c.name, qty: c.qty, price: c.price, unit: c.unit, category: c.category, purchasePrice: c.purchasePrice });
            item.stock -= c.qty;
        });
        if (!valid) return;
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
        saveLocal(); syncToCloud(); renderAll();
        document.getElementById('salePaid').value = '0';
        showToast(`✅ Sale: ${total} | Profit: ${(total - cost).toFixed(2)}`);
    }

    // ============================================================
    // CUSTOMERS
    // ============================================================
    function addCustomer() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const name = document.getElementById('custName').value.trim();
        if (!name) return alert('Enter name!');
        db.customers.push({ id: Date.now(), name, phone: document.getElementById('custPhone').value, balance: 0 });
        saveLocal(); syncToCloud(); renderAll();
        document.getElementById('custName').value = ''; document.getElementById('custPhone').value = '';
        showToast('✅ Customer added!');
    }

    function receivePayment() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const id = document.getElementById('receiveSelect').value;
        const amount = parseFloat(document.getElementById('receiveAmount').value);
        if (!id || !amount || amount <= 0) return alert('Select customer and amount!');
        const cust = db.customers.find(c => c.id == id);
        if (!cust || cust.balance < amount) return alert(`Balance: ${cust.balance}`);
        cust.balance -= amount;
        db.transactions.push({ id: Date.now(), type: 'receive', desc: `Received from ${cust.name}`, amount, date: getToday() });
        saveLocal(); syncToCloud(); renderAll();
        document.getElementById('receiveAmount').value = '';
        showToast(`✅ Received ${amount}!`);
    }

    function renderCustomers() {
        const tbody = document.getElementById('customerListBody');
        if (!db.customers.length) { tbody.innerHTML = '<tr><td colspan="3">No customers</td></tr>'; } else {
            let html = '';
            db.customers.forEach(c => { html += `<tr><td>${c.name}</td><td>${c.phone || '-'}</td><td>${c.balance}</td></tr>`; });
            tbody.innerHTML = html;
        }
        const sel = document.getElementById('saleCustomerSelect'), rsel = document.getElementById('receiveSelect');
        [sel, rsel].forEach(s => {
            if (!s) return;
            const val = s.value;
            s.innerHTML = '<option value="">Walk-in</option>';
            db.customers.forEach(c => { const o = document.createElement('option'); o.value = c.id; o.textContent = `${c.name} (${c.balance})`; s.appendChild(o); });
            if (val) s.value = val;
        });
    }

    // ============================================================
    // REPORTS
    // ============================================================
    function genReport(type) {
        const today = getToday();
        let sales = [];
        if (type === 'daily') { sales = db.sales.filter(s => s.date === today); }
        else if (type === 'weekly') {
            const d = new Date(); d.setDate(d.getDate() - 7);
            sales = db.sales.filter(s => s.date >= d.toISOString().split('T')[0]);
        } else {
            const d = new Date(); d.setMonth(d.getMonth() - 1);
            sales = db.sales.filter(s => s.date >= d.toISOString().split('T')[0]);
        }
        const totalSales = sales.reduce((s, i) => s + i.total, 0);
        let stone = 0, material = 0, stoneCost = 0, materialCost = 0;
        sales.forEach(s => {
            s.items.forEach(i => {
                if (i.category === 'stone') { stone += i.qty * i.price; stoneCost += i.qty * (i.purchasePrice || 0); }
                else { material += i.qty * i.price; materialCost += i.qty * (i.purchasePrice || 0); }
            });
        });
        const profit = (stone + material) - (stoneCost + materialCost);
        
        let report = `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        report += `  🏪 WARDA MARBLE\n`;
        report += `  📍 ${type.toUpperCase()} REPORT\n`;
        report += `  📅 ${today}\n`;
        report += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        report += `  💰 Total Sales    : ${totalSales.toFixed(2)}\n`;
        report += `  🪨 Stone Sales    : ${stone.toFixed(2)}\n`;
        report += `  🧱 Material Sales : ${material.toFixed(2)}\n`;
        report += `  📈 Profit/Loss    : ${profit >= 0 ? '✅ ' + profit.toFixed(2) : '❌ ' + Math.abs(profit).toFixed(2)}\n`;
        report += `  📦 Total Orders   : ${sales.length}\n`;
        report += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        if (sales.length > 0) {
            report += `  📋 ORDER DETAILS\n`;
            report += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
            sales.slice(0, 10).forEach((s, idx) => {
                report += `  #${idx+1} | ${s.date} | ${s.total.toFixed(2)}\n`;
                s.items.forEach(i => {
                    report += `     ${i.name} x${i.qty} = ${(i.qty * i.price).toFixed(2)}\n`;
                });
                report += `  ─────────────────────────────\n`;
            });
            if (sales.length > 10) report += `  ... and ${sales.length - 10} more\n`;
        }
        report += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        report += `  📱 Thank you! Visit again.\n`;
        report += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        
        document.getElementById('reportDisplay').innerHTML = `<pre style="font-family:monospace;font-size:9px;margin:0;white-space:pre-wrap;line-height:1.4;">${report}</pre>`;
        document.getElementById('plDisplay').innerHTML = `
            <div style="padding:6px;background:${profit>=0?'#d4edda':'#f8d7da'};border-radius:6px;">
                <div style="font-size:18px;font-weight:700;color:${profit>=0?'#155724':'#721c24'};">${profit >= 0 ? '✅ Profit: ' + profit.toFixed(2) : '❌ Loss: ' + Math.abs(profit).toFixed(2)}</div>
                <div style="font-size:8px;color:#6c757d;">Stone: ${(stone - stoneCost).toFixed(2)} | Material: ${(material - materialCost).toFixed(2)}</div>
            </div>
        `;
        renderTransactions();
        renderItemDetails();
        return report;
    }

    function renderTransactions() {
        const tbody = document.getElementById('transactionsBody');
        if (!db.transactions.length) { tbody.innerHTML = '<tr><td colspan="3">No transactions</td></tr>'; return; }
        let html = '';
        const sorted = [...db.transactions].reverse().slice(0, 25);
        sorted.forEach(t => {
            const color = t.type === 'expense' ? '#c0392b' : '#218838';
            html += `<tr><td>${t.date}</td><td>${t.desc}</td><td style="color:${color};font-weight:600;">${t.amount}</td></tr>`;
        });
        tbody.innerHTML = html;
    }

    // ============================================================
    // ITEM DETAILS
    // ============================================================
    function renderItemDetails() {
        const tbody = document.getElementById('itemDetailsBody');
        const mbody = document.getElementById('monthlySummaryBody');
        if (!db.items.length) {
            tbody.innerHTML = '<tr><td colspan="4">No items</td></tr>';
            mbody.innerHTML = '<tr><td colspan="3">No data</td></tr>';
            return;
        }
        let html = '';
        db.items.forEach(item => {
            const sales = db.sales.filter(s => s.items.some(i => i.id === item.id));
            let totalQty = 0, totalRevenue = 0;
            sales.forEach(s => {
                s.items.forEach(i => {
                    if (i.id === item.id) {
                        totalQty += i.qty;
                        totalRevenue += i.qty * i.price;
                    }
                });
            });
            html += `<tr><td>${item.name}</td><td>${item.unit}</td><td>${totalQty}</td><td>${totalRevenue.toFixed(2)}</td></tr>`;
        });
        tbody.innerHTML = html;

        const months = {};
        db.sales.forEach(s => {
            const m = s.date.substring(0, 7);
            if (!months[m]) months[m] = { total: 0, count: 0 };
            months[m].total += s.total;
            months[m].count += 1;
        });
        let mhtml = '';
        Object.keys(months).sort().forEach(m => {
            mhtml += `<tr><td>${m}</td><td>${months[m].total.toFixed(2)}</td><td>${months[m].count}</td></tr>`;
        });
        mbody.innerHTML = mhtml || '<tr><td colspan="3">No data</td></tr>';
    }

    // ============================================================
    // PRINT & SHARE
    // ============================================================
    function printReport() {
        const content = document.querySelector('#reportDisplay pre');
        if (!content) return alert('Generate report first!');
        const w = window.open('', '', 'width=600,height=500');
        w.document.write(`<style>body{font-family:monospace;padding:20px;font-size:14px;background:white;}</style>`);
        w.document.write(content.textContent);
        w.document.close();
        w.print();
    }

    function shareReport() {
        const content = document.querySelector('#reportDisplay pre');
        if (!content) return alert('Generate report first!');
        const phone = '923001234567';
        window.open(`https://wa.me/${phone}?text=${encodeURIComponent(content.textContent)}`, '_blank');
    }

    // ============================================================
    // RENDER ALL
    // ============================================================
    function renderAll() {
        renderPOS();
        renderCart();
        renderStockList();
        renderCustomers();
        renderTransactions();
        renderItemDetails();
        const active = document.querySelector('.section.active');
        if (active && active.id === 'section-reports') genReport('daily');
        if (active && active.id === 'section-details') renderItemDetails();
    }

    // ============================================================
    // NAVIGATION
    // ============================================================
    document.querySelectorAll('.tab, .bottom-nav .nav-item').forEach(el => {
        el.addEventListener('click', function() {
            const id = this.dataset.tab;
            document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
            document.querySelector(`.tab[data-tab="${id}"]`)?.classList.add('active');
            document.querySelectorAll('.bottom-nav .nav-item').forEach(n => n.classList.remove('active'));
            document.querySelector(`.bottom-nav .nav-item[data-tab="${id}"]`)?.classList.add('active');
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById('section-' + id).classList.add('active');
            if (id === 'pos') { renderPOS(); renderCart(); }
            if (id === 'stock') renderStockList();
            if (id === 'customers') renderCustomers();
            if (id === 'reports') genReport('daily');
            if (id === 'details') renderItemDetails();
        });
    });

    // ============================================================
    // INIT
    // ============================================================
    if (!db.items.length) {
        db.items = [
            { id: 1, name: 'زاویہ 4*4*3mm', category: 'material', unit: 'pcs', sellPrice: 0, purchasePrice: 0, stock: 9200, minStock: 100 },
            { id: 2, name: 'زاویہ 4*4*2mm', category: 'material', unit: 'pcs', sellPrice: 0, purchasePrice: 0, stock: 6600, minStock: 100 },
            { id: 3, name: 'ابیض مجلی 3*7', category: 'stone', unit: 'm²', sellPrice: 65, purchasePrice: 50, stock: 50, minStock: 10 },
            { id: 4, name: 'ابیض مجلی 3*10', category: 'stone', unit: 'm²', sellPrice: 65, purchasePrice: 50, stock: 50, minStock: 10 },
            { id: 5, name: 'ابیض مجلی 3*15', category: 'stone', unit: 'm²', sellPrice: 65, purchasePrice: 50, stock: 50, minStock: 10 }
        ];
        saveLocal();
    }

    const savedSession = loadSession();
    if (savedSession && savedSession.active) {
        isAdmin = true;
        document.getElementById('roleBadge').textContent = '👑 Admin';
        document.getElementById('roleBadge').className = 'admin-badge';
        enableEditing(true);
        updateSettingsUI();
    }

    renderAll();
    syncFromCloud();
    setInterval(() => { 
        if (navigator.onLine) {
            syncFromCloud();
            if (isAdmin && sessionId) syncSessionToCloud();
        } else {
            updateStatus('offline');
        }
    }, 30000);

    console.log('🚀 Warda Marble Ready! Items:', db.items.length);
</script>
</body>
</html>
