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
    </div>

    <!-- ========== INVOICES ========== -->
    <div id="section-invoices" class="section">
        <div class="scroll-area">
            <div class="card">
                <div class="card-title"><i class="fas fa-file-invoice"></i> All Invoices 
                    <span style="margin-right:auto;"></span>
                    <button class="undo-btn" onclick="undoLastDelete()"><i class="fas fa-undo"></i> Undo</button>
                </div>
                <div id="invoicesList"></div>
            </div>
            <div class="card">
                <div class="card-title"><i class="fas fa-edit"></i> Edit Invoice</div>
                <div class="grid-2">
                    <div class="form-group"><label>Invoice ID</label><input type="text" id="editInvoiceId" placeholder="ID"></div>
                    <div class="form-group"><label>Customer</label><input type="text" id="editInvoiceCustomer" placeholder="Name"></div>
                </div>
                <div class="grid-2">
                    <div class="form-group"><label>Total</label><input type="number" id="editInvoiceTotal" placeholder="Total"></div>
                    <div class="form-group"><label>Paid</label><input type="number" id="editInvoicePaid" placeholder="Paid"></div>
                </div>
                <div class="grid-2">
                    <button class="btn btn-warning" onclick="updateInvoice()"><i class="fas fa-save"></i> Update</button>
                    <button class="btn btn-danger" onclick="deleteInvoice()"><i class="fas fa-trash"></i> Delete</button>
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
    <div class="nav-item" data-tab="summary"><i class="fas fa-chart-bar"></i> Summary</div>
    <div class="nav-item" data-tab="reports"><i class="fas fa-chart-pie"></i> Report</div>
    <div class="nav-item" data-tab="invoices"><i class="fas fa-file-invoice"></i> Bills</div>
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
        <div class="setting-item" style="margin-top:4px;">
            <div><div class="label"><i class="fas fa-language"></i> Language</div><div class="desc">Switch between English / Urdu</div></div>
            <button class="btn btn-outline" onclick="settingsToggleLang()" style="width:auto;padding:2px 8px;font-size:8px;"><span id="settingsLangLabel">اردو</span></button>
        </div>
        <div class="setting-item">
            <div><div class="label"><i class="fas fa-key"></i> Change Password</div><div class="desc">Set new admin password</div></div>
            <div><input type="password" id="newPasswordInput" placeholder="New password..." style="width:100px;padding:2px 4px;border:1px solid #e2e6ea;border-radius:3px;font-size:7px;">
            <button class="btn btn-warning" onclick="changePassword()" style="width:auto;padding:2px 6px;font-size:7px;margin-top:1px;">Change</button></div>
        </div>
        <div style="text-align:center;font-size:6px;color:#8a9aaa;margin-top:6px;padding-top:4px;border-top:1px solid #eef2f6;">Warda Marble Pro v5.0</div>
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
    let lastDeletedInvoice = null;

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

    function getSessionKey() { return 'warda_session_' + window.location.hostname; }

    function saveSession() {
        const session = { id: sessionId || 'session_' + Date.now(), device: getDeviceInfo(), browser: getBrowserInfo(), time: new Date().toLocaleString(), active: true, lastSeen: Date.now() };
        sessionId = session.id;
        localStorage.setItem(getSessionKey(), JSON.stringify(session));
        if (sessionId) database.ref('warda_sessions').child(sessionId).set(session);
    }

    function loadSession() {
        const data = localStorage.getItem(getSessionKey());
        if (data) {
            try { const s = JSON.parse(data); if (s.active) { sessionId = s.id; return s; } } catch(e) {}
        }
        return null;
    }

    function clearSession() {
        localStorage.removeItem(getSessionKey());
        if (sessionId) database.ref('warda_sessions').child(sessionId).update({ active: false });
    }

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

    function settingsLogout() { logout(); document.getElementById('settingsModal').style.display = 'none'; }
    function toggleSettings() {
        const modal = document.getElementById('settingsModal');
        modal.style.display = modal.style.display === 'flex' ? 'none' : 'flex';
        if (modal.style.display === 'flex') updateSettingsUI();
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

    function enableEditing(enable) {
        document.querySelectorAll('.btn, .pos-item, input, select').forEach(el => {
            el.style.opacity = enable ? '1' : '0.5';
            el.style.pointerEvents = enable ? 'auto' : 'none';
        });
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
    function settingsToggleLang() { toggleLang(); document.getElementById('settingsLangLabel').textContent = currentLang === 'en' ? 'اردو' : 'English'; }

    // ============================================================
    // DATABASE
    // ============================================================
    let db = { items: [], customers: [], sales: [], expenses: [], transactions: [], stockHistory: [], dailyStock: [] };
    let cart = [];
    let currentFilter = 'all';
    let isSyncing = false;
    let selectedItemId = null;

    function loadLocal() {
        const data = localStorage.getItem('warda_data');
        if (data) { const p = JSON.parse(data); db = p; }
        if (!db.stockHistory) db.stockHistory = [];
        if (!db.expenses) db.expenses = [];
        if (!db.dailyStock) db.dailyStock = [];
    }
    loadLocal();

    function saveLocal() {
        localStorage.setItem('warda_data', JSON.stringify(db));
    }

    function getToday() { return new Date().toISOString().split('T')[0]; }
    function getMonth() { return new Date().toISOString().substring(0, 7); }

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
            if (data) { db = data; if (!db.stockHistory) db.stockHistory = []; if (!db.expenses) db.expenses = []; if (!db.dailyStock) db.dailyStock = []; saveLocal(); renderAll(); showToast('✅ Synced!'); }
            updateStatus('online'); isSyncing = false;
        }).catch(() => { updateStatus('offline'); isSyncing = false; showToast('❌ Sync failed!'); });
    }

    function showToast(msg) {
        const t = document.createElement('div');
        t.style.cssText = 'position:fixed;bottom:58px;left:50%;transform:translateX(-50%);background:#0f1a2e;color:white;padding:3px 10px;border-radius:4px;font-size:8px;z-index:9999;max-width:90%;text-align:center;box-shadow:0 4px 12px rgba(0,0,0,0.15)';
        t.textContent = msg;
        document.body.appendChild(t);
        setTimeout(() => { t.style.opacity = '0'; t.style.transition = 'opacity 0.3s'; setTimeout(() => t.remove(), 300); }, 1400);
    }

    // ============================================================
    // POS INPUTS - Auto Calculate SQM
    // ============================================================
    function calculateSQM() {
        const length = parseFloat(document.getElementById('posLength').value) || 0;
        const width = parseFloat(document.getElementById('posWidth').value) || 0;
        const sqm = (length * width) / 10.764;
        document.getElementById('posSqm').value = sqm.toFixed(2);
        return sqm;
    }

    document.getElementById('posLength').addEventListener('input', calculateSQM);
    document.getElementById('posWidth').addEventListener('input', calculateSQM);

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
            size: document.getElementById('itemSize').value.trim(),
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
        item.size = document.getElementById('itemSize').value.trim();
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
        document.getElementById('itemSize').value = item.size || '';
        document.getElementById('itemSellPrice').value = item.sellPrice;
        document.getElementById('itemPurchasePrice').value = item.purchasePrice;
        document.getElementById('itemStock').value = item.stock;
    }

    function clearItemForm() {
        document.getElementById('editItemId').value = '';
        document.getElementById('itemName').value = '';
        document.getElementById('itemSize').value = '';
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
        db.stockHistory.push({ date: getToday(), item: item.name, size: item.size || '-', qty: qty, type: 'In' });
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
        db.stockHistory.push({ date: getToday(), item: item.name, size: item.size || '-', qty: qty, type: 'Out' });
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
            html += `<tr><td>${item.name}</td><td>${item.size || '-'}</td><td>${item.stock} ${warn}</td><td>${item.sellPrice}</td><td><button class="edit-btn" onclick="selectItem(${item.id})">✏️</button></td></tr>`;
        });
        tbody.innerHTML = html;

        const sel = document.getElementById('stockSelect');
        sel.innerHTML = '';
        db.items.forEach(i => { const o = document.createElement('option'); o.value = i.id; o.textContent = `${i.name} (${i.stock})`; sel.appendChild(o); });

        const htbody = document.getElementById('stockHistoryBody');
        if (!db.stockHistory.length) { htbody.innerHTML = '<tr><td colspan="5">No history</td></tr>'; return; }
        let hhtml = '';
        db.stockHistory.slice(-20).reverse().forEach(h => {
            hhtml += `<tr><td>${h.date}</td><td>${h.item}</td><td>${h.size}</td><td>${h.qty}</td><td class="${h.type === 'In' ? 'badge-success' : 'badge-danger'}">${h.type}</td></tr>`;
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
        if (!items.length) { grid.innerHTML = '<div style="grid-column:1/-1;text-align:center;color:#8a9aaa;padding:4px;font-size:6px;">No items</div>'; return; }
        let html = '';
        items.forEach(item => {
            const out = item.stock <= 0 ? 'out-of-stock' : '';
            html += `<div class="pos-item ${out}" onclick="${out ? '' : `selectPOSItem(${item.id})`}" style="${out ? 'opacity:0.4;pointer-events:none;' : ''}">
                <div class="name">${item.name}</div>
                <div class="size">${item.size || ''}</div>
                <div class="price">${item.sellPrice}</div>
                <div class="stock">${item.stock} ${item.unit}</div>
            </div>`;
        });
        grid.innerHTML = html;
    }

    function selectPOSItem(id) {
        const item = db.items.find(i => i.id === id);
        if (!item) return;
        selectedItemId = id;
        document.getElementById('posLength').value = '0';
        document.getElementById('posWidth').value = '0';
        document.getElementById('posSqm').value = '0';
        document.getElementById('posPieces').value = '0';
        showToast(`✅ ${item.name} selected! Enter quantity`);
    }

    // ============================================================
    // CART
    // ============================================================
    function addToCartFromInputs() {
        if (!selectedItemId) return alert('Select an item first!');
        const item = db.items.find(i => i.id === selectedItemId);
        if (!item) return;
        if (item.stock <= 0) return alert('Out of stock!');
        
        let qty = 0;
        let sqm = 0;
        let pieces = parseFloat(document.getElementById('posPieces').value) || 0;
        let length = parseFloat(document.getElementById('posLength').value) || 0;
        let width = parseFloat(document.getElementById('posWidth').value) || 0;
        let inputSqm = parseFloat(document.getElementById('posSqm').value) || 0;
        
        if (item.category === 'stone') {
            if (length > 0 && width > 0) {
                sqm = (length * width) / 10.764;
            } else if (inputSqm > 0) {
                sqm = inputSqm;
            } else if (pieces > 0 && length > 0 && width > 0) {
                sqm = ((length * width) / 10.764) * pieces;
            } else if (pieces > 0) {
                qty = pieces;
            } else {
                return alert('Enter Length, Width, SQM or Pieces!');
            }
            if (sqm > 0) qty = sqm;
        } else {
            qty = pieces;
            if (qty <= 0) return alert('Enter Pieces!');
        }
        
        if (qty <= 0) return alert('Invalid quantity!');
        if (item.stock < qty) return alert(`Not enough stock! Available: ${item.stock}`);
        
        const existing = cart.find(c => c.id === item.id);
        if (existing) {
            existing.qty += qty;
        } else {
            cart.push({
                id: item.id,
                name: item.name,
                size: item.size || '',
                price: item.sellPrice,
                qty: qty,
                unit: item.unit,
                category: item.category,
                purchasePrice: item.purchasePrice || 0,
                sqm: sqm,
                pieces: pieces,
                length: length,
                width: width
            });
        }
        renderCart();
        document.getElementById('posLength').value = '0';
        document.getElementById('posWidth').value = '0';
        document.getElementById('posSqm').value = '0';
        document.getElementById('posPieces').value = '0';
        showToast(`✅ Added to cart!`);
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
            container.innerHTML = '<div style="text-align:center;color:#8a9aaa;padding:4px;font-size:6px;">Cart empty</div>';
            document.getElementById('cartTotal').textContent = '0.00';
            document.getElementById('cartCount').textContent = '0';
            return;
        }
        let html = '', total = 0;
        cart.forEach(c => {
            total += c.qty * c.price;
            const qtyDisplay = c.category === 'stone' ? c.qty.toFixed(2) + ' m²' : c.qty + ' pcs';
            html += `<div class="cart-item">
                <span><strong>${c.name}</strong> ${c.size} (${qtyDisplay})</span>
                <div class="qty-control">
                    <button onclick="updateQty(${c.id}, -1)">−</button>
                    <span style="min-width:12px;text-align:center;">${c.category === 'stone' ? c.qty.toFixed(1) : c.qty}</span>
                    <button onclick="updateQty(${c.id}, 1)">+</button>
                    <button onclick="removeFromCart(${c.id})" style="background:#f8d7da;border:none;border-radius:50%;width:16px;height:16px;cursor:pointer;font-size:6px;color:#721c24;">✕</button>
                </div>
            </div>`;
        });
        container.innerHTML = html;
        document.getElementById('cartTotal').textContent = total.toFixed(2);
        document.getElementById('cartCount').textContent = cart.length;
    }

    // ============================================================
    // COMPLETE SALE
    // ============================================================
    function completeSale() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        
        const length = parseFloat(document.getElementById('posLength').value) || 0;
        const width = parseFloat(document.getElementById('posWidth').value) || 0;
        const sqm = parseFloat(document.getElementById('posSqm').value) || 0;
        const pieces = parseFloat(document.getElementById('posPieces').value) || 0;
        if (length > 0 || width > 0 || sqm > 0 || pieces > 0) {
            addToCartFromInputs();
        }
        
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
            items.push({ id: c.id, name: c.name, size: c.size || '', qty: c.qty, price: c.price, unit: c.unit, category: c.category, purchasePrice: c.purchasePrice, sqm: c.sqm || 0, pieces: c.pieces || 0 });
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
        const profit = total - cost;
        db.transactions.push({ id: Date.now(), type: 'sale', desc: `${items.length} items (${type})`, amount: total, profit: profit, date: getToday() });
        cart = [];
        saveLocal(); syncToCloud(); renderAll();
        document.getElementById('salePaid').value = '0';
        document.getElementById('posLength').value = '0';
        document.getElementById('posWidth').value = '0';
        document.getElementById('posSqm').value = '0';
        document.getElementById('posPieces').value = '0';
        showToast(`✅ Sale: ${total.toFixed(2)} | Profit: ${profit.toFixed(2)}`);
    }

    // ============================================================
    // CUSTOMERS
    // ============================================================
    function addCustomer() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const name = document.getElementById('custName').value.trim();
        if (!name) return alert('Enter name!');
        db.customers.push({ id: Date.now(), name, phone: document.getElementById('custPhone').value, balance: 0, advance: 0 });
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
        if (!db.customers.length) { tbody.innerHTML = '<tr><td colspan="4">No customers</td></tr>'; } else {
            let html = '';
            db.customers.forEach(c => {
                const billCount = db.sales.filter(s => s.customerId == c.id).length;
                html += `<tr><td>${c.name}</td><td>${c.phone || '-'}</td><td>${c.balance}</td>
                <td><button class="btn-sm btn-primary" onclick="viewCustomerBills(${c.id})" style="padding:0 4px;font-size:5px;">📄</button></td></tr>`;
            });
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
    // OPENING / CLOSING STOCK
    // ============================================================
    function calculateStockSummary() {
        const today = getToday();
        const container = document.getElementById('stockSummaryDisplay');
        
        // Get previous day's closing stock
        const prevDate = new Date();
        prevDate.setDate(prevDate.getDate() - 1);
        const prevDateStr = prevDate.toISOString().split('T')[0];
        
        // Find saved daily stock
        const prevStock = db.dailyStock.find(d => d.date === prevDateStr);
        const todayStock = db.dailyStock.find(d => d.date === today);
        
        let html = '<div class="row"><strong>Item</strong><strong>Opening</strong><strong>In</strong><strong>Out</strong><strong>Sold</strong><strong>Closing</strong></div>';
        
        db.items.forEach(item => {
            // Opening = previous day's closing or current stock if no history
            let opening = 0;
            if (prevStock && prevStock.items) {
                const found = prevStock.items.find(i => i.id === item.id);
                opening = found ? found.closing : 0;
            } else {
                // If no history, use current stock as opening (for first day)
                opening = item.stock;
            }
            
            // Calculate today's in/out from stock history
            const todayIn = db.stockHistory.filter(h => h.date === today && h.item === item.name && h.type === 'In')
                .reduce((sum, h) => sum + h.qty, 0);
            const todayOut = db.stockHistory.filter(h => h.date === today && h.item === item.name && h.type === 'Out')
                .reduce((sum, h) => sum + h.qty, 0);
            
            // Calculate today's sales
            const todaySales = db.sales.filter(s => s.date === today);
            let sold = 0;
            todaySales.forEach(s => {
                s.items.forEach(i => {
                    if (i.id === item.id) sold += i.qty;
                });
            });
            
            // Closing = Opening + In - Out - Sold
            const closing = opening + todayIn - todayOut - sold;
            
            // Save daily stock
            if (!todayStock) {
                const newDailyStock = { date: today, items: [] };
                db.dailyStock.push(newDailyStock);
            }
            const currentDaily = db.dailyStock.find(d => d.date === today);
            if (currentDaily) {
                const existing = currentDaily.items.find(i => i.id === item.id);
                if (existing) {
                    existing.opening = opening;
                    existing.closing = closing;
                } else {
                    currentDaily.items.push({ id: item.id, name: item.name, opening: opening, closing: closing });
                }
            }
            
            html += `<div class="row">
                <span>${item.name} ${item.size || ''}</span>
                <span>${opening.toFixed(2)}</span>
                <span style="color:#218838;">${todayIn.toFixed(2)}</span>
                <span style="color:#c0392b;">${todayOut.toFixed(2)}</span>
                <span style="color:#d4a017;">${sold.toFixed(2)}</span>
                <span style="font-weight:700;">${closing.toFixed(2)}</span>
            </div>`;
        });
        
        container.innerHTML = html;
        saveLocal();
    }

    // ============================================================
    // SUMMARY
    // ============================================================
    function updateSummary() {
        const today = getToday();
        document.getElementById('summaryDate').textContent = today;
        document.getElementById('monthlyDate').textContent = getMonth();
        
        const todaySales = db.sales.filter(s => s.date === today);
        const todayCash = todaySales.filter(s => s.type === 'cash').reduce((sum, s) => sum + s.total, 0);
        const todayUdhar = todaySales.filter(s => s.type === 'credit').reduce((sum, s) => sum + s.total, 0);
        const todayIkhrajat = db.expenses.filter(e => e.date === today).reduce((sum, e) => sum + e.amount, 0);
        
        const prevSales = db.sales.filter(s => s.date < today);
        const sabqaWusool = prevSales.reduce((sum, s) => sum + s.paid, 0);
        const advanceWusool = todaySales.reduce((sum, s) => sum + s.paid, 0);
        const totalJamma = todayCash + sabqaWusool + advanceWusool;
        const paymentMinus = totalJamma < todayIkhrajat ? todayIkhrajat - totalJamma : 0;
        
        document.getElementById('sumTodaySale').textContent = (todayCash + todayUdhar).toFixed(2);
        document.getElementById('sumUdharSale').textContent = todayUdhar.toFixed(2);
        document.getElementById('sumIkhrajat').textContent = todayIkhrajat.toFixed(2);
        document.getElementById('sumSabqaWusool').textContent = sabqaWusool.toFixed(2);
        document.getElementById('sumAdvanceWusool').textContent = advanceWusool.toFixed(2);
        document.getElementById('sumTotalJamma').textContent = totalJamma.toFixed(2);
        document.getElementById('sumPaymentMinus').textContent = paymentMinus.toFixed(2);
        
        const pmBox = document.getElementById('paymentMinusBox');
        pmBox.className = 'summary-box ' + (paymentMinus > 0 ? 'negative' : 'positive');
        
        // Calculate stock summary
        calculateStockSummary();
        
        // Monthly Summary
        const month = getMonth();
        const monthSales = db.sales.filter(s => s.date.startsWith(month));
        const monthData = {};
        monthSales.forEach(s => {
            if (!monthData[s.date]) monthData[s.date] = { sale: 0, udhar: 0, ikhrajat: 0 };
            if (s.type === 'cash') monthData[s.date].sale += s.total;
            else monthData[s.date].udhar += s.total;
        });
        db.expenses.filter(e => e.date.startsWith(month)).forEach(e => {
            if (!monthData[e.date]) monthData[e.date] = { sale: 0, udhar: 0, ikhrajat: 0 };
            monthData[e.date].ikhrajat += e.amount;
        });
        
        let mhtml = '';
        Object.keys(monthData).sort().forEach(d => {
            const data = monthData[d];
            const total = data.sale + data.udhar;
            mhtml += `<tr><td>${d}</td><td>${total.toFixed(2)}</td><td>${data.udhar.toFixed(2)}</td><td>${data.ikhrajat.toFixed(2)}</td><td>${(total - data.ikhrajat).toFixed(2)}</td></tr>`;
        });
        document.getElementById('monthlySummaryBody').innerHTML = mhtml || '<tr><td colspan="5">No data</td></tr>';
    }

    // ============================================================
    // INVOICES
    // ============================================================
    function viewCustomerBills(customerId) {
        const cust = db.customers.find(c => c.id === customerId);
        if (!cust) return;
        const bills = db.sales.filter(s => s.customerId == customerId);
        if (!bills.length) { alert(`No bills for ${cust.name}`); return; }
        let msg = `📋 BILLS FOR ${cust.name}\n━━━━━━━━━━━━━━━━━━━\n`;
        bills.forEach((b, i) => {
            msg += `#${i+1} | ${b.date} | ${b.total.toFixed(2)}\n`;
            b.items.forEach(item => {
                const qtyDisplay = item.category === 'stone' ? item.qty.toFixed(2) + 'm²' : item.qty + 'pcs';
                msg += `   ${item.name} ${qtyDisplay} = ${(item.qty * item.price).toFixed(2)}\n`;
            });
            msg += `─────────────────────────\n`;
        });
        alert(msg);
        renderInvoices();
    }

    function renderInvoices() {
        const container = document.getElementById('invoicesList');
        if (!db.sales.length) { container.innerHTML = '<div style="color:#8a9aaa;font-size:7px;padding:3px;">No invoices</div>'; return; }
        let html = '';
        const sorted = [...db.sales].reverse();
        sorted.slice(0, 20).forEach(s => {
            const cust = db.customers.find(c => c.id == s.customerId);
            const custName = cust ? cust.name : 'Walk-in';
            html += `<div class="bill-item" onclick="viewInvoice(${s.id})">
                <div class="bill-header">
                    <span><strong>#${s.id}</strong> ${custName}</span>
                    <span>${s.date} | ${s.total.toFixed(2)}</span>
                </div>
                <div class="bill-footer">
                    <span>${s.items.length} items | ${s.type}</span>
                    <span>Paid: ${s.paid.toFixed(2)}</span>
                </div>
            </div>`;
        });
        container.innerHTML = html;
    }

    function viewInvoice(saleId) {
        const sale = db.sales.find(s => s.id === saleId);
        if (!sale) return alert('Invoice not found!');
        const cust = db.customers.find(c => c.id == sale.customerId);
        const custName = cust ? cust.name : 'Walk-in';
        let invoice = `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        invoice += `  🏪 WARDA MARBLE\n`;
        invoice += `  📍 INVOICE #${sale.id}\n`;
        invoice += `  📅 ${sale.date}\n`;
        invoice += `  👤 ${custName}\n`;
        invoice += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        invoice += `  ITEM          QTY  PRICE  TOTAL\n`;
        sale.items.forEach(i => {
            const name = i.name + (i.size ? ' ' + i.size : '');
            const qtyDisplay = i.category === 'stone' ? i.qty.toFixed(2) + 'm²' : i.qty + 'pcs';
            invoice += `  ${name.padEnd(12)} ${qtyDisplay.padStart(6)}  ${i.price.toString().padStart(5)}  ${(i.qty*i.price).toFixed(2).padStart(6)}\n`;
        });
        invoice += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        invoice += `  TOTAL: ${sale.total.toFixed(2)}\n`;
        invoice += `  PAID: ${sale.paid.toFixed(2)}\n`;
        invoice += `  REMAINING: ${sale.remaining.toFixed(2)}\n`;
        invoice += `  TYPE: ${sale.type.toUpperCase()}\n`;
        invoice += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        invoice += `  📱 Thank you! Visit again.\n`;
        invoice += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        
        document.getElementById('reportDisplay').innerHTML = `<pre style="font-family:monospace;font-size:8px;margin:0;white-space:pre-wrap;line-height:1.3;">${invoice}</pre>`;
        
        document.getElementById('editInvoiceId').value = sale.id;
        document.getElementById('editInvoiceCustomer').value = custName;
        document.getElementById('editInvoiceTotal').value = sale.total;
        document.getElementById('editInvoicePaid').value = sale.paid;
        
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        document.querySelector('.tab[data-tab="reports"]').classList.add('active');
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
        document.getElementById('section-reports').classList.add('active');
    }

    function updateInvoice() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const id = parseInt(document.getElementById('editInvoiceId').value);
        if (!id) return alert('Enter Invoice ID!');
        const sale = db.sales.find(s => s.id === id);
        if (!sale) return alert('Invoice not found!');
        const newTotal = parseFloat(document.getElementById('editInvoiceTotal').value);
        const newPaid = parseFloat(document.getElementById('editInvoicePaid').value);
        if (!newTotal || newTotal <= 0) return alert('Enter valid total!');
        sale.total = newTotal;
        sale.paid = newPaid || 0;
        sale.remaining = newTotal - sale.paid;
        if (sale.customerId) {
            const cust = db.customers.find(c => c.id == sale.customerId);
            if (cust) {
                const allSales = db.sales.filter(s => s.customerId == sale.customerId);
                let totalBalance = 0;
                allSales.forEach(s => { totalBalance += s.remaining; });
                cust.balance = totalBalance;
            }
        }
        saveLocal(); syncToCloud(); renderAll();
        showToast('✅ Invoice updated!');
    }

    function deleteInvoice() {
        if (!isAdmin) return alert('🔒 Admin only! Login first.');
        const id = parseInt(document.getElementById('editInvoiceId').value);
        if (!id) return alert('Enter Invoice ID!');
        if (!confirm('Delete this invoice?')) return;
        const sale = db.sales.find(s => s.id === id);
        if (!sale) return alert('Invoice not found!');
        
        lastDeletedInvoice = {
            sale: JSON.parse(JSON.stringify(sale)),
            index: db.sales.findIndex(s => s.id === id)
        };
        
        sale.items.forEach(item => {
            const dbItem = db.items.find(i => i.id === item.id);
            if (dbItem) dbItem.stock += item.qty;
        });
        db.sales = db.sales.filter(s => s.id !== id);
        if (sale.customerId) {
            const cust = db.customers.find(c => c.id == sale.customerId);
            if (cust) {
                const allSales = db.sales.filter(s => s.customerId == sale.customerId);
                let totalBalance = 0;
                allSales.forEach(s => { totalBalance += s.remaining; });
                cust.balance = totalBalance;
            }
        }
        saveLocal(); syncToCloud(); renderAll();
        document.getElementById('editInvoiceId').value = '';
        document.getElementById('editInvoiceCustomer').value = '';
        document.getElementById('editInvoiceTotal').value = '';
        document.getElementById('editInvoicePaid').value = '';
        showToast('✅ Invoice deleted! (Use Undo to restore)');
    }

    function undoLastDelete() {
        if (!lastDeletedInvoice) return alert('Nothing to undo!');
        if (!confirm('Restore deleted invoice?')) return;
        const { sale } = lastDeletedInvoice;
        db.sales.push(sale);
        sale.items.forEach(item => {
            const dbItem = db.items.find(i => i.id === item.id);
            if (dbItem) dbItem.stock -= item.qty;
        });
        if (sale.customerId) {
            const cust = db.customers.find(c => c.id == sale.customerId);
            if (cust) {
                const allSales = db.sales.filter(s => s.customerId == sale.customerId);
                let totalBalance = 0;
                allSales.forEach(s => { totalBalance += s.remaining; });
                cust.balance = totalBalance;
            }
        }
        lastDeletedInvoice = null;
        saveLocal(); syncToCloud(); renderAll();
        showToast('✅ Invoice restored!');
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
                const cust = db.customers.find(c => c.id == s.customerId);
                report += `  #${idx+1} | ${s.date} | ${cust ? cust.name : 'Walk-in'} | ${s.total.toFixed(2)}\n`;
                s.items.forEach(i => {
                    const qtyDisplay = i.category === 'stone' ? i.qty.toFixed(2) + 'm²' : i.qty + 'pcs';
                    report += `     ${i.name}${i.size ? ' '+i.size : ''} ${qtyDisplay} = ${(i.qty * i.price).toFixed(2)}\n`;
                });
                report += `  ─────────────────────────────\n`;
            });
            if (sales.length > 10) report += `  ... and ${sales.length - 10} more\n`;
        }
        report += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        report += `  📱 Thank you! Visit again.\n`;
        report += `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`;
        
        document.getElementById('reportDisplay').innerHTML = `<pre style="font-family:monospace;font-size:7px;margin:0;white-space:pre-wrap;line-height:1.3;">${report}</pre>`;
        document.getElementById('plDisplay').innerHTML = `
            <div style="padding:4px;background:${profit>=0?'#d4edda':'#f8d7da'};border-radius:4px;">
                <div style="font-size:14px;font-weight:700;color:${profit>=0?'#155724':'#721c24'};">${profit >= 0 ? '✅ Profit: ' + profit.toFixed(2) : '❌ Loss: ' + Math.abs(profit).toFixed(2)}</div>
                <div style="font-size:6px;color:#6c757d;">Stone: ${(stone - stoneCost).toFixed(2)} | Material: ${(material - materialCost).toFixed(2)}</div>
            </div>
        `;
        renderTransactions();
        renderInvoices();
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
        renderInvoices();
        updateSummary();
        const active = document.querySelector('.section.active');
        if (active && active.id === 'section-reports') genReport('daily');
        if (active && active.id === 'section-summary') updateSummary();
        if (active && active.id === 'section-invoices') renderInvoices();
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
            if (id === 'summary') updateSummary();
            if (id === 'reports') genReport('daily');
            if (id === 'invoices') renderInvoices();
        });
    });

    // ============================================================
    // INIT
    // ============================================================
    if (!db.items.length) {
        db.items = [
            { id: 1, name: 'زاویہ', category: 'material', unit: 'pcs', size: '4*4*3mm', sellPrice: 0, purchasePrice: 0, stock: 9200, minStock: 100 },
            { id: 2, name: 'زاویہ', category: 'material', unit: 'pcs', size: '4*4*2mm', sellPrice: 0, purchasePrice: 0, stock: 6600, minStock: 100 },
            { id: 3, name: 'ابیض مجلی', category: 'stone', unit: 'm²', size: '3*7', sellPrice: 65, purchasePrice: 50, stock: 50, minStock: 10 },
            { id: 4, name: 'ابیض مجلی', category: 'stone', unit: 'm²', size: '3*10', sellPrice: 65, purchasePrice: 50, stock: 50, minStock: 10 },
            { id: 5, name: 'ابیض مجلی', category: 'stone', unit: 'm²', size: '3*15', sellPrice: 65, purchasePrice: 50, stock: 50, minStock: 10 }
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
            if (isAdmin && sessionId) saveSession();
        } else {
            updateStatus('offline');
        }
    }, 30000);

    console.log('🚀 Warda Marble Pro Ready! Items:', db.items.length);
</script>
</body>
</html>
