<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Warda Marble - Pro</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
    <style>
        *{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma,Geneva,Verdana,sans-serif}
        html,body{height:100%;overflow:hidden;background:#eef2f7}
        body{padding:4px;padding-bottom:56px}
        .app-container{max-width:100%;height:100%;margin:0 auto;background:white;border-radius:12px;overflow:hidden;padding:4px;display:flex;flex-direction:column;box-shadow:0 2px 12px rgba(0,0,0,0.06)}
        .header{background:linear-gradient(135deg,#0f1a2e,#1a2a4a);color:white;padding:5px 10px;border-radius:8px;display:flex;justify-content:space-between;align-items:center;flex-shrink:0;min-height:32px}
        .header h1{font-size:12px;font-weight:700}
        .header-controls{display:flex;gap:3px;align-items:center}
        .header-controls button{background:rgba(255,255,255,0.1);border:none;color:white;padding:2px 7px;border-radius:12px;cursor:pointer;font-size:7px;font-weight:600;transition:0.2s}
        .header-controls button:hover{background:rgba(255,255,255,0.2)}
        .settings-btn{background:rgba(255,255,255,0.08);padding:2px 8px;border-radius:50%;font-size:12px}
        .settings-btn:hover{transform:rotate(90deg)}
        .admin-badge{background:#d4a017;color:white;padding:1px 6px;border-radius:8px;font-size:6px;font-weight:600}
        .viewer-badge{background:#6b7a8a;color:white;padding:1px 6px;border-radius:8px;font-size:6px;font-weight:600}
        .tabs{display:flex;gap:2px;margin:2px 0;flex-shrink:0;background:#f0f4f8;padding:2px;border-radius:6px;flex-wrap:wrap}
        .tab{flex:1;padding:3px 2px;text-align:center;border-radius:5px;font-weight:600;font-size:6px;cursor:pointer;transition:0.2s;min-width:28px;color:#6b7a8a;background:transparent}
        .tab i{display:block;font-size:11px;margin-bottom:1px}
        .tab.active{background:#0f1a2e;color:white}
        .section{display:none;flex:1;overflow:hidden;padding-top:2px}
        .section.active{display:flex;flex-direction:column}
        .card{background:white;border-radius:6px;padding:3px 4px;margin-bottom:2px;border:1px solid #e8ecf0;flex-shrink:0}
        .card-title{font-size:8px;font-weight:700;color:#0f1a2e;display:flex;align-items:center;gap:3px;border-bottom:1px solid #eef2f6;padding-bottom:2px;margin-bottom:2px}
        .pos-grid{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:2px;max-height:80px;overflow-y:auto;padding:2px 0}
        .pos-item{background:#f7f9fb;border:1px solid #e2e6ea;border-radius:4px;padding:3px 2px;text-align:center;cursor:pointer;transition:0.15s;font-size:7px}
        .pos-item:active{transform:scale(0.92);background:#edf1f5}
        .pos-item .name{font-weight:600;color:#1a2a3a;font-size:7px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
        .pos-item .price{color:#0f1a2e;font-weight:700;font-size:8px}
        .pos-item .stock{font-size:5px;color:#8a9aaa}
        .pos-item .size{font-size:5px;color:#6b7a8a}
        .pos-item.out-of-stock{opacity:0.4;pointer-events:none}
        .cart-area{flex:1;display:flex;flex-direction:column;min-height:0}
        .cart-items{flex:1;overflow-y:auto;padding:2px 0;max-height:60px}
        .cart-item{display:flex;justify-content:space-between;padding:2px 0;border-bottom:1px solid #eef2f6;font-size:7px;align-items:center}
        .cart-item .qty-control{display:flex;gap:2px;align-items:center}
        .cart-item .qty-control button{background:#e8ecf0;border:none;border-radius:50%;width:16px;height:16px;font-weight:700;cursor:pointer;font-size:8px}
        .cart-item .qty-control button:active{transform:scale(0.85)}
        .total-bar{background:linear-gradient(135deg,#0f1a2e,#1a2a4a);color:white;padding:3px 6px;border-radius:4px;display:flex;justify-content:space-between;font-weight:700;font-size:10px;flex-shrink:0}
        .bottom-actions{display:grid;grid-template-columns:1fr 1fr 1fr;gap:2px;flex-shrink:0;padding-top:2px}
        .bottom-actions .btn{padding:2px;font-size:6px}
        .btn{padding:2px 5px;border:none;border-radius:3px;font-size:7px;font-weight:600;cursor:pointer;transition:0.15s;display:flex;align-items:center;justify-content:center;gap:2px;width:100%}
        .btn:active{transform:scale(0.92)}
        .btn-primary{background:#0f1a2e;color:white}
        .btn-success{background:#218838;color:white}
        .btn-danger{background:#c0392b;color:white}
        .btn-warning{background:#d4a017;color:white}
        .btn-outline{background:transparent;border:1px solid #0f1a2e;color:#0f1a2e}
        .btn-sm{padding:1px 3px;font-size:5px}
        .grid-2{display:grid;grid-template-columns:1fr 1fr;gap:2px}
        .grid-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:2px}
        .grid-4{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:2px}
        .form-group{margin-bottom:1px}
        .form-group label{display:block;font-size:6px;font-weight:600;color:#2d3748;margin-bottom:1px}
        .form-group input,.form-group select{width:100%;padding:2px 3px;border:1px solid #e2e6ea;border-radius:3px;font-size:7px;background:#f7f9fb}
        .form-group input:focus{border-color:#0f1a2e;outline:none;background:white}
        .table-wrap{overflow:auto;flex:1;min-height:0}
        table{width:100%;border-collapse:collapse;font-size:6px}
        th{background:#0f1a2e;color:white;padding:2px 2px;text-align:center;font-size:5px;position:sticky;top:0;z-index:2}
        td{padding:2px 2px;border-bottom:1px solid #eef2f6;text-align:center;font-size:6px}
        tr:nth-child(even){background:#f8fafc}
        .badge{display:inline-block;padding:1px 4px;border-radius:6px;font-size:5px;font-weight:600}
        .badge-success{background:#d4edda;color:#155724}
        .badge-danger{background:#f8d7da;color:#721c24}
        .badge-warning{background:#fff3cd;color:#856404}
        .badge-info{background:#d1ecf1;color:#0c5460}
        .bottom-nav{position:fixed;bottom:0;left:0;right:0;background:white;display:flex;justify-content:space-around;padding:2px 0;box-shadow:0 -1px 8px rgba(0,0,0,0.06);border-top:1px solid #eef2f6;z-index:1000}
        .bottom-nav .nav-item{text-align:center;font-size:5px;color:#8a9aaa;cursor:pointer;padding:2px 4px;border-radius:4px;transition:0.15s}
        .bottom-nav .nav-item i{font-size:12px;display:block}
        .bottom-nav .nav-item.active{color:#0f1a2e;font-weight:700}
        .scroll-area{flex:1;overflow-y:auto;min-height:0;padding-right:2px}
        .sync-status{font-size:5px;color:rgba(255,255,255,0.6);display:flex;align-items:center;gap:2px}
        .dot{width:4px;height:4px;border-radius:50%;display:inline-block}
        .dot.green{background:#4ade80}
        .dot.red{background:#f87171}
        .dot.yellow{background:#fbbf24;animation:blink 1s infinite}
        @keyframes blink{0%,100%{opacity:1}50%{opacity:0.3}}
        .view-only .btn,.view-only .pos-item,.view-only input,.view-only select{opacity:0.5;pointer-events:none}
        .cat-tabs{display:flex;gap:2px;margin-bottom:2px;flex-wrap:wrap}
        .cat-tab{padding:1px 5px;border-radius:3px;font-weight:600;font-size:5px;cursor:pointer;background:#e8ecf0;color:#6b7a8a;border:none;flex:1;transition:0.15s}
        .cat-tab.active{color:white}
        .cat-tab.stone-active{background:#8B7355;color:white}
        .cat-tab.material-active{background:#2b6cb0;color:white}
        .edit-btn{background:#e8ecf0;border:none;padding:0 3px;border-radius:2px;cursor:pointer;font-size:5px}
        .invoice-box{background:white;border:1px solid #d4a017;border-radius:5px;padding:8px;font-size:8px;font-family:monospace;white-space:pre-wrap;max-height:160px;overflow-y:auto;box-shadow:0 1px 4px rgba(212,160,23,0.15)}
        .invoice-box .inv-header{text-align:center;font-weight:700;font-size:11px;border-bottom:2px solid #0f1a2e;padding-bottom:4px;margin-bottom:4px}
        .invoice-box .inv-footer{text-align:center;font-size:7px;color:#6b7a8a;border-top:1px dashed #ccc;padding-top:4px;margin-top:4px}
        .modal-overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.5);display:flex;align-items:center;justify-content:center;z-index:9999;backdrop-filter:blur(3px)}
        .modal-box{background:white;padding:12px;border-radius:10px;max-width:320px;width:92%;max-height:80vh;overflow-y:auto;box-shadow:0 16px 48px rgba(0,0,0,0.2)}
        .modal-box h2{font-size:13px;color:#0f1a2e;margin-bottom:6px;display:flex;align-items:center;gap:5px;border-bottom:2px solid #eef2f6;padding-bottom:5px}
        .modal-box .modal-close{float:right;background:none;border:none;font-size:14px;cursor:pointer;color:#8a9aaa}
        .modal-box .modal-close:hover{color:#0f1a2e}
        .modal-box .setting-item{padding:4px 0;border-bottom:1px solid #eef2f6;display:flex;justify-content:space-between;align-items:center}
        .modal-box .setting-item:last-child{border-bottom:none}
        .modal-box .setting-item .label{font-size:10px;font-weight:600;color:#1a2a3a}
        .modal-box .setting-item .desc{font-size:7px;color:#8a9aaa}
        .modal-box input[type="password"]{width:100%;padding:4px 6px;border:1.5px solid #e2e6ea;border-radius:4px;font-size:10px;margin:2px 0}
        .modal-box input[type="password"]:focus{border-color:#0f1a2e;outline:none}
        .modal-box .btn{width:100%;padding:4px;border:none;border-radius:4px;font-size:10px;font-weight:600;cursor:pointer;margin-top:2px}
        .modal-box .error{color:#c0392b;font-size:8px;margin-top:2px;display:none}
        .summary-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:2px}
        .summary-box{background:#f8fafc;padding:3px;border-radius:4px;text-align:center;border:1px solid #e2e6ea}
        .summary-box .num{font-size:11px;font-weight:700;color:#0f1a2e}
        .summary-box .lbl{font-size:5px;color:#6b7a8a}
        .summary-box.positive .num{color:#218838}
        .summary-box.negative .num{color:#c0392b}
        .summary-box.warning .num{color:#d4a017}
        .pos-inputs{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:2px;padding:2px 0}
        .pos-inputs input{width:100%;padding:2px 3px;border:1px solid #e2e6ea;border-radius:3px;font-size:7px;background:#f7f9fb;text-align:center}
        .pos-inputs input:focus{border-color:#0f1a2e;outline:none;background:white}
        .pos-inputs .label{font-size:5px;color:#6b7a8a;text-align:center}
        .bill-item{cursor:pointer;padding:3px;border:1px solid #e2e6ea;border-radius:4px;margin:2px 0;background:#f8fafc;font-size:7px;transition:0.15s}
        .bill-item:hover{background:#edf1f5;border-color:#d4a017}
        .bill-item .bill-header{display:flex;justify-content:space-between;font-weight:600}
        .bill-item .bill-footer{font-size:5px;color:#6b7a8a;display:flex;justify-content:space-between}
        .stock-summary{background:#f0f4f8;padding:3px;border-radius:4px;margin:2px 0;font-size:6px}
        .stock-summary .row{display:flex;justify-content:space-between;padding:1px 0}
        @media(max-width:480px){.pos-grid{grid-template-columns:1fr 1fr 1fr}.header h1{font-size:10px}.bottom-actions{grid-template-columns:1fr 1fr}.summary-grid{grid-template-columns:1fr 1fr}}
        @media(min-width:768px){.pos-grid{grid-template-columns:1fr 1fr 1fr 1fr 1fr}}
        .undo-btn{background:#d4a017;color:white;border:none;padding:1px 6px;border-radius:3px;cursor:pointer;font-size:6px}
        .undo-btn:hover{background:#b8890f}
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
        <div class="tab" data-tab="summary"><i class="fas fa-chart-bar"></i> Summary</div>
        <div class="tab" data-tab="reports"><i class="fas fa-chart-pie"></i> Report</div>
        <div class="tab" data-tab="invoices"><i class="fas fa-file-invoice"></i> Bills</div>
    </div>

    <!-- ========== POS ========== -->
    <div id="section-pos" class="section active">
        <div class="card" style="flex-shrink:0;padding:3px;">
            <div class="card-title" style="font-size:7px;margin-bottom:1px;">
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
            <div class="card" style="flex:1;display:flex;flex-direction:column;padding:3px;margin-bottom:0;">
                <div class="card-title" style="font-size:7px;margin-bottom:1px;">
                    <i class="fas fa-shopping-cart"></i> Cart (<span id="cartCount">0</span>)
                    <span style="margin-right:auto;"></span>
                    <button class="btn btn-sm btn-danger" onclick="clearCart()" style="width:auto;padding:0 4px;font-size:5px;"><i class="fas fa-trash"></i></button>
                </div>
                <div class="cart-items" id="cartItems"></div>
                <div class="total-bar"><span>Total:</span><span id="cartTotal">0.00</span></div>
                
                <!-- POS Input Boxes -->
                <div class="pos-inputs" id="posInputs">
                    <div><div class="label">Length</div><input type="number" id="posLength" placeholder="L" value="0" step="0.01"></div>
                    <div><div class="label">Width</div><input type="number" id="posWidth" placeholder="W" value="0" step="0.01"></div>
                    <div><div class="label">SQM</div><input type="number" id="posSqm" placeholder="m²" value="0" step="0.01" readonly style="background:#edf2f7;"></div>
                    <div><div class="label">Pieces</div><input type="number" id="posPieces" placeholder="Pcs" value="0" step="1"></div>
                </div>
                
                <div class="bottom-actions">
                    <select id="saleType" style="padding:2px 3px;border-radius:3px;border:1px solid #e2e6ea;font-size:6px;">
                        <option value="cash">Cash</option>
                        <option value="credit">Credit</option>
                    </select>
                    <select id="saleCustomerSelect" style="padding:2px 3px;border-radius:3px;border:1px solid #e2e6ea;font-size:6px;">
                        <option value="">Walk-in</option>
                    </select>
                    <input type="number" id="salePaid" placeholder="Paid" value="0" style="padding:2px 3px;border-radius:3px;border:1px solid #e2e6ea;font-size:6px;grid-column:1/-1;">
                    <button class="btn btn-success" onclick="completeSale()" style="grid-column:1/-1;padding:3px;font-size:7px;"><i class="fas fa-check"></i> Complete Sale</button>
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
                    <div class="form-group"><label>Size</label><input type="text" id="itemSize" placeholder="e.g. 3*7"></div>
                </div>
                <div class="grid-2">
                    <div class="form-group"><label>Sell Price</label><input type="number" id="itemSellPrice" placeholder="Sell"></div>
                    <div class="form-group"><label>Purchase Price</label><input type="number" id="itemPurchasePrice" placeholder="Cost"></div>
                </div>
                <div class="form-group"><label>Stock</label><input type="number" id="itemStock" placeholder="Qty"></div>
                <div class="grid-3">
                    <button class="btn btn-primary" onclick="addItem()"><i class="fas fa-save"></i> Add</button>
                    <button class="btn btn-warning" onclick="updateItem()"><i class="fas fa-edit"></i> Update</button>
                    <button class="btn btn-danger" onclick="deleteItem()"><i class="fas fa-trash"></i> Delete</button>
                </div>
                <input type="hidden" id="editItemId" value="">
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-list"></i> Stock List</div>
                <div class="table-wrap" style="max-height:90px;">
                    <table><thead><tr><th>Name</th><th>Size</th><th>Stock</th><th>Price</th><th>Action</th></tr></thead>
                    <tbody id="stockListBody"></tbody></table>
                </div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-arrow-right"></i> Stock In/Out</div>
                <div class="grid-2">
                    <select id="stockSelect" style="padding:2px 3px;border-radius:3px;border:1px solid #e2e6ea;"></select>
                    <input type="number" id="stockQty" placeholder="Qty" style="padding:2px 3px;border-radius:3px;border:1px solid #e2e6ea;">
                </div>
                <div class="grid-2">
                    <button class="btn btn-success" onclick="stockIn()"><i class="fas fa-plus"></i> In</button>
                    <button class="btn btn-danger" onclick="stockOut()"><i class="fas fa-minus"></i> Out</button>
                </div>
            </div>
            
            <div class="card">
                <div class="card-title"><i class="fas fa-clock"></i> Stock History</div>
                <div class="table-wrap" style="max-height:60px;">
                    <table><thead><tr><th>Date</th><th>Item</th><th>Size</th><th>Qty</th><th>Type</th></tr></thead>
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
                <div class="table-wrap" style="max-height:90px;">
                    <table><thead><tr><th>Name</th><th>Phone</th><th>Balance</th><th>Bills</th></tr></thead>
                    <tbody id="customerListBody"></tbody></table>
                </div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-hand-holding-usd"></i> Receive Payment</div>
                <div class="grid-2">
                    <select id="receiveSelect" style="padding:2px 3px;border-radius:3px;border:1px solid #e2e6ea;"></select>
                    <input type="number" id="receiveAmount" placeholder="Amount" style="padding:2px 3px;border-radius:3px;border:1px solid #e2e6ea;">
                </div>
                <button class="btn btn-success" onclick="receivePayment()"><i class="fas fa-money-bill-wave"></i> Receive</button>
            </div>
        </div>
    </div>

    <!-- ========== SUMMARY ========== -->
    <div id="section-summary" class="section">
        <div class="scroll-area">
            <div class="card">
                <div class="card-title"><i class="fas fa-chart-bar"></i> Daily Summary - <span id="summaryDate"></span></div>
                <div class="summary-grid" id="summaryGrid">
                    <div class="summary-box positive"><div class="num" id="sumTodaySale">0.00</div><div class="lbl">Today's Sale</div></div>
                    <div class="summary-box warning"><div class="num" id="sumUdharSale">0.00</div><div class="lbl">Udhar Sale</div></div>
                    <div class="summary-box negative"><div class="num" id="sumIkhrajat">0.00</div><div class="lbl">Ikhrajat</div></div>
                    <div class="summary-box positive"><div class="num" id="sumSabqaWusool">0.00</div><div class="lbl">Sabqa Wusool</div></div>
                    <div class="summary-box positive"><div class="num" id="sumAdvanceWusool">0.00</div><div class="lbl">Advance Wusool</div></div>
                    <div class="summary-box positive"><div class="num" id="sumTotalJamma">0.00</div><div class="lbl">Total Jamma</div></div>
                    <div class="summary-box" id="paymentMinusBox"><div class="num" id="sumPaymentMinus">0.00</div><div class="lbl">Payment Minus</div></div>
                </div>
                <div style="font-size:5px;color:#6b7a8a;text-align:center;padding:2px;">Auto calculated from today's data</div>
            </div>

            <!-- ========== OPENING/CLOSING STOCK ========== -->
            <div class="card">
                <div class="card-title"><i class="fas fa-boxes"></i> Opening / Closing Stock</div>
                <div id="stockSummaryDisplay" class="stock-summary"></div>
                <div style="font-size:5px;color:#6b7a8a;text-align:center;padding:2px;">Opening = Previous day closing | Closing = Opening + In - Out - Sales</div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-calendar-alt"></i> Monthly Summary - <span id="monthlyDate"></span></div>
                <div class="table-wrap" style="max-height:100px;">
                    <table><thead><tr><th>Date</th><th>Sale</th><th>Udhar</th><th>Ikhrajat</th><th>Jamma</th></tr></thead>
                    <tbody id="monthlySummaryBody"></tbody></table>
                </div>
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
                <div style="display:flex;gap:2px;margin-top:2px;">
                    <button class="btn btn-success" onclick="printReport()" style="flex:1;"><i class="fas fa-print"></i> Print</button>
                    <button class="btn btn-outline" onclick="shareReport()" style="flex:1;"><i class="fab fa-whatsapp"></i> Share</button>
                </div>
                <div id="reportDisplay" class="invoice-box" style="margin-top:3px;"></div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-chart-line"></i> Profit / Loss</div>
                <div id="plDisplay" style="text-align:center;padding:4px;font-size:12px;font-weight:700;"></div>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-history"></i> All Transactions</div>
                <div class="table-wrap" style="max-height:70px;">
                    <table><thead><tr><th>Date</th><th>Desc</th><th>Amount</th></tr></thead>
                    <tbody id="transactionsBody"></tbody></table>
                </div>
            </div>
        </div>
