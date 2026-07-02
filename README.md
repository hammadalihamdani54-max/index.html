<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Warda Marble</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box;font-family:Arial,sans-serif;}
        body{background:#0f2027;padding:10px;}
        .container{max-width:1200px;margin:auto;background:white;border-radius:15px;padding:20px;box-shadow:0 10px 30px rgba(0,0,0,0.3);}
        h1{color:#ffd700;text-align:center;padding:10px 0;border-bottom:2px solid #ffd700;}
        .tabs{display:flex;gap:5px;margin:15px 0;flex-wrap:wrap;}
        .tab{padding:10px 20px;background:#1a1a2e;color:white;border:none;border-radius:8px;cursor:pointer;font-weight:bold;}
        .tab.active{background:#ffd700;color:#1a1a2e;}
        .panel{background:#f0f2f5;padding:15px;border-radius:10px;margin-top:10px;}
        .pos-grid{display:grid;grid-template-columns:1fr 1fr;gap:20px;}
        @media(max-width:700px){.pos-grid{grid-template-columns:1fr;}}
        .product-card{background:white;padding:10px;border-radius:8px;border:1px solid #ddd;cursor:pointer;margin:5px 0;}
        .product-card:hover{border-color:#ffd700;}
        input,select{width:100%;padding:8px;margin:5px 0;border:1px solid #ddd;border-radius:5px;}
        button{padding:8px 15px;border:none;border-radius:5px;cursor:pointer;font-weight:bold;}
        .btn-success{background:#27ae60;color:white;}
        .btn-danger{background:#e74c3c;color:white;}
        .btn-warning{background:#f39c12;color:white;}
        .btn-info{background:#3498db;color:white;}
        .total-box{background:#1a1a2e;color:white;padding:15px;border-radius:10px;margin-top:15px;}
        table{width:100%;border-collapse:collapse;font-size:12px;}
        th,td{padding:8px;border-bottom:1px solid #ddd;text-align:left;}
        th{background:#1a1a2e;color:#ffd700;}
        .badge{padding:3px 10px;border-radius:20px;font-size:10px;font-weight:bold;display:inline-block;}
        .badge-paid{background:#27ae60;color:white;}
        .badge-unpaid{background:#e74c3c;color:white;}
        .badge-partial{background:#f39c12;color:white;}
        .login-overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.8);display:flex;justify-content:center;align-items:center;z-index:9999;}
        .login-box{background:white;padding:30px;border-radius:15px;max-width:380px;width:90%;text-align:center;}
        .login-box input{width:100%;padding:12px;border:2px solid #ddd;border-radius:8px;font-size:16px;text-align:center;margin-bottom:12px;}
        .toast{position:fixed;bottom:20px;right:20px;background:#1a1a2e;color:#ffd700;padding:10px 20px;border-radius:10px;z-index:1000;}
        .product-list{max-height:400px;overflow-y:auto;}
        .cart-items{max-height:200px;overflow-y:auto;}
    </style>
</head>
<body>
<div class="container" id="app">
    <h1>🏢 WARDA MARBLE</h1>
    <p style="text-align:center;color:#666;margin-bottom:15px;">Premium Marble Solutions</p>
    
    <div class="tabs">
        <button class="tab active" onclick="showTab('pos')">🛒 POS</button>
        <button class="tab" onclick="showTab('customers')">👥 Customers</button>
        <button class="tab" onclick="showTab('inventory')">📦 Stock</button>
        <button class="tab" onclick="showTab('invoices')">📄 Invoices</button>
        <button class="tab" onclick="showTab('reports')">📊 Reports</button>
    </div>

    <!-- ===== POS TAB ===== -->
    <div id="posTab" class="panel">
        <div class="pos-grid">
            <div>
                <h3>📦 Products</h3>
                <input type="text" id="searchProd" placeholder="Search..." onkeyup="displayProducts()">
                <div class="product-list" id="productList"></div>
            </div>
            <div>
                <h3>🛒 Cart</h3>
                <input type="text" id="custName" placeholder="Customer Name">
                <input type="text" id="custPhone" placeholder="Phone">
                <div class="cart-items" id="cartItems"></div>
                <div class="total-box">
                    <p>Total: <span id="cartTotal">SAR 0</span></p>
                    <p>Received: <input type="number" id="receivedAmt" value="0" style="width:100px;display:inline-block;" oninput="updateCart()"></p>
                    <p id="balanceDisplay">Balance: SAR 0</p>
                </div>
                <button class="btn-success" onclick="confirmOrder()">✅ Confirm Order</button>
                <button class="btn-danger" onclick="clearCart()">🗑️ Clear</button>
            </div>
        </div>
    </div>

    <!-- ===== CUSTOMERS TAB ===== -->
    <div id="customersTab" class="panel" style="display:none;">
        <h3>👥 Customers</h3>
        <input type="text" id="searchCust" placeholder="Search..." onkeyup="displayCustomers()">
        <div id="customerList"></div>
    </div>

    <!-- ===== INVENTORY TAB ===== -->
    <div id="inventoryTab" class="panel" style="display:none;">
        <h3>📦 Stock Management</h3>
        <input type="text" id="searchStock" placeholder="Search..." onkeyup="loadStock()">
        <button class="btn-success" onclick="addProduct()">➕ Add Product</button>
        <div id="stockList" style="margin-top:10px;"></div>
    </div>

    <!-- ===== INVOICES TAB ===== -->
    <div id="invoicesTab" class="panel" style="display:none;">
        <h3>📄 Invoices</h3>
        <input type="text" id="searchInv" placeholder="Search..." onkeyup="displayInvoices()">
        <div id="invoiceList"></div>
    </div>

    <!-- ===== REPORTS TAB ===== -->
    <div id="reportsTab" class="panel" style="display:none;">
        <h3>📊 Reports</h3>
        <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px;margin-bottom:15px;">
            <div style="background:#1a1a2e;color:#ffd700;padding:15px;border-radius:10px;text-align:center;">
                <h4>Today's Sales</h4>
                <p id="todaySales" style="font-size:24px;">SAR 0</p>
            </div>
            <div style="background:#1a1a2e;color:#ffd700;padding:15px;border-radius:10px;text-align:center;">
                <h4>Today's Profit</h4>
                <p id="todayProfit" style="font-size:24px;">SAR 0</p>
            </div>
            <div style="background:#1a1a2e;color:#ffd700;padding:15px;border-radius:10px;text-align:center;">
                <h4>Orders</h4>
                <p id="todayOrders" style="font-size:24px;">0</p>
            </div>
        </div>
        <div id="dailyReportList"></div>
    </div>
</div>

<!-- ===== LOGIN ===== -->
<div id="loginOverlay" class="login-overlay">
    <div class="login-box">
        <h2>🔐 Admin Login</h2>
        <p>Enter password to manage system</p>
        <input type="password" id="loginPass" placeholder="Enter password..." onkeydown="if(event.key==='Enter') login()">
        <button onclick="login()" class="btn-success" style="width:100%;padding:12px;">🔓 Login</button>
        <p id="loginError" style="color:red;display:none;">Wrong password!</p>
        <p style="margin-top:10px;font-size:11px;color:#999;">Default: <strong>Hamdani5</strong></p>
    </div>
</div>

<script>
// ===== LOGIN =====
const PASSWORD = "Hamdani5";
let isAdmin = false;

function login() {
    const input = document.getElementById('loginPass').value;
    if (input === PASSWORD) {
        isAdmin = true;
        document.getElementById('loginOverlay').style.display = 'none';
        showToast('✅ Admin mode activated!');
        loadData();
    } else {
        document.getElementById('loginError').style.display = 'block';
        setTimeout(() => document.getElementById('loginError').style.display = 'none', 3000);
    }
}

// ===== DATA =====
let products = [], orders = [], customers = [], cart = [];

function buildProducts() {
    return [
        { id: 1, name: "زاویہ اپ ڈاون", size: "4*4*3mm", group: 'mwad', selling: 0, cost: 0, stock: 9200, type: 'pieces' },
        { id: 2, name: "زاویہ اپ ڈاون", size: "4*4*2mm", group: 'mwad', selling: 0, cost: 0, stock: 6600, type: 'pieces' },
        { id: 3, name: "ابیض مجلی مبروم", size: "3*7", group: 'mojali', selling: 65, cost: 50, stock: 50, type: 'sqm' },
        { id: 4, name: "ابیض مجلی مبروم", size: "3*10", group: 'mojali', selling: 65, cost: 50, stock: 50, type: 'sqm' },
        { id: 5, name: "ابیض مجلی سادہ", size: "3*7", group: 'mojali', selling: 60, cost: 45, stock: 50, type: 'sqm' },
        { id: 6, name: "ابیض مصنفر مبروم", size: "3*7", group: 'mosnafar', selling: 55, cost: 42, stock: 50, type: 'sqm' },
        { id: 7, name: "مسمار مغلوب", size: "10*40", group: 'mwad', selling: 0, cost: 0, stock: 4800, type: 'pieces' },
        { id: 8, name: "براغی سقف", size: "10*72", group: 'mwad', selling: 0, cost: 0, stock: 2200, type: 'pieces' }
    ];
}

function saveData() {
    localStorage.setItem('products', JSON.stringify(products));
    localStorage.setItem('orders', JSON.stringify(orders));
    localStorage.setItem('customers', JSON.stringify(customers));
}

function loadData() {
    let p = localStorage.getItem('products');
    products = p ? JSON.parse(p) : buildProducts();
    let o = localStorage.getItem('orders');
    orders = o ? JSON.parse(o) : [];
    let c = localStorage.getItem('customers');
    customers = c ? JSON.parse(c) : [];
    displayProducts();
    loadStock();
    displayCustomers();
    displayInvoices();
    updateReports();
}

function showToast(msg) {
    let t = document.createElement('div');
    t.className = 'toast';
    t.innerText = msg;
    document.body.appendChild(t);
    setTimeout(() => t.remove(), 2500);
}

function showTab(tab) {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.panel').forEach(p => p.style.display = 'none');
    document.querySelector(`.tab[onclick*="${tab}"]`).classList.add('active');
    document.getElementById(tab + 'Tab').style.display = 'block';
    if (tab === 'reports') updateReports();
}

// ===== PRODUCTS =====
function displayProducts() {
    let search = document.getElementById('searchProd').value.toLowerCase();
    let filtered = products.filter(p => p.name.toLowerCase().includes(search) || p.size.toLowerCase().includes(search));
    let html = '';
    filtered.forEach(p => {
        html += `<div class="product-card" onclick="addToCart(${p.id})">
            <strong>${p.name}</strong> ${p.size}<br>
            Price: ${p.selling > 0 ? 'SAR ' + p.selling : 'Set Price'} | Stock: ${p.stock}
        </div>`;
    });
    document.getElementById('productList').innerHTML = html || '<p>No products</p>';
}

function addToCart(id) {
    if (!isAdmin) return showToast('🔒 Login first!');
    let p = products.find(x => x.id === id);
    if (!p || p.stock <= 0) return showToast('Out of stock!');
    let existing = cart.find(c => c.id === id);
    if (existing) {
        if (existing.qty >= p.stock) return showToast('Not enough stock!');
        existing.qty++;
    } else {
        cart.push({ id: p.id, name: p.name, size: p.size, price: p.selling, qty: 1, cost: p.cost || p.selling * 0.7 });
    }
    updateCart();
}

function updateCart() {
    let html = '', total = 0;
    cart.forEach((c, i) => {
        total += c.qty * c.price;
        html += `<div style="display:flex;justify-content:space-between;padding:5px;border-bottom:1px solid #eee;">
            <span>${c.name} ${c.size} x${c.qty}</span>
            <span>SAR ${(c.qty * c.price).toFixed(2)} 
            <button onclick="removeFromCart(${i})" style="background:#e74c3c;color:white;border:none;border-radius:50%;width:20px;height:20px;cursor:pointer;">✕</button></span>
        </div>`;
    });
    document.getElementById('cartItems').innerHTML = html || '<p>Cart empty</p>';
    document.getElementById('cartTotal').textContent = 'SAR ' + total.toFixed(2);
    let rec = parseFloat(document.getElementById('receivedAmt').value) || 0;
    document.getElementById('balanceDisplay').textContent = 'Balance: SAR ' + (total - rec).toFixed(2);
}

function removeFromCart(index) {
    cart.splice(index, 1);
    updateCart();
}

function clearCart() {
    if (!confirm('Clear cart?')) return;
    cart = [];
    updateCart();
}

function confirmOrder() {
    if (!isAdmin) return showToast('🔒 Login first!');
    if (cart.length === 0) return showToast('Cart is empty!');
    let cust = document.getElementById('custName').value || 'Walk-in';
    let phone = document.getElementById('custPhone').value || '';
    let total = cart.reduce((s, c) => s + c.qty * c.price, 0);
    let rec = parseFloat(document.getElementById('receivedAmt').value) || 0;
    let invNo = 'INV-' + String(orders.length + 1).padStart(4, '0');
    let paid = Math.min(rec, total);
    let status = paid >= total ? 'paid' : (paid > 0 ? 'partial' : 'unpaid');
    let order = {
        invoiceNo: invNo,
        date: new Date().toISOString(),
        customer: cust,
        phone: phone,
        items: JSON.parse(JSON.stringify(cart)),
        total: total,
        paid: paid,
        status: status,
        cost: cart.reduce((s, c) => s + c.qty * c.cost, 0)
    };
    order.profit = order.total - order.cost;
    orders.push(order);
    cart.forEach(c => {
        let p = products.find(x => x.id === c.id);
        if (p) p.stock -= c.qty;
    });
    let custObj = customers.find(c => c.phone === phone);
    if (!custObj) { custObj = { name: cust, phone: phone, invoices: [] }; customers.push(custObj); }
    custObj.invoices.push({ invoiceNo: invNo, date: order.date, amount: total, paid: paid, status: status });
    saveData();
    cart = [];
    updateCart();
    displayProducts();
    loadStock();
    displayCustomers();
    displayInvoices();
    updateReports();
    document.getElementById('custName').value = '';
    document.getElementById('custPhone').value = '';
    document.getElementById('receivedAmt').value = '0';
    showToast('✅ Order confirmed! ' + invNo);
}

// ===== STOCK =====
function loadStock() {
    let search = document.getElementById('searchStock').value.toLowerCase();
    let filtered = products.filter(p => p.name.toLowerCase().includes(search) || p.size.toLowerCase().includes(search));
    let html = `<table><thead><tr><th>Name</th><th>Size</th><th>Stock</th><th>Price</th><th>Action</th></tr></thead><tbody>`;
    filtered.forEach(p => {
        html += `<tr>
            <td>${p.name}</td>
            <td>${p.size}</td>
            <td>${p.stock}</td>
            <td>${p.selling}</td>
            <td><button onclick="editStock(${p.id})" class="btn-info" style="padding:2px 8px;">✏️</button></td>
        </tr>`;
    });
    html += '</tbody></table>';
    document.getElementById('stockList').innerHTML = html;
}

function editStock(id) {
    if (!isAdmin) return showToast('🔒 Login first!');
    let p = products.find(x => x.id === id);
    if (!p) return;
    let newPrice = prompt('New selling price for ' + p.name, p.selling);
    if (newPrice !== null && !isNaN(newPrice)) { p.selling = parseFloat(newPrice); saveData(); loadStock(); displayProducts(); showToast('Updated!'); }
}

function addProduct() {
    if (!isAdmin) return showToast('🔒 Login first!');
    let name = prompt('Product name:');
    if (!name) return;
    let size = prompt('Size:');
    let price = prompt('Selling price:');
    let stock = prompt('Stock:');
    let id = products.length ? Math.max(...products.map(p => p.id)) + 1 : 1;
    products.push({ id, name, size, group: 'mwad', selling: parseFloat(price) || 0, cost: 0, stock: parseInt(stock) || 0, type: 'pieces' });
    saveData();
    loadStock();
    displayProducts();
    showToast('✅ Product added!');
}

// ===== CUSTOMERS =====
function displayCustomers() {
    let search = document.getElementById('searchCust').value.toLowerCase();
    let filtered = customers.filter(c => c.name.toLowerCase().includes(search) || (c.phone && c.phone.includes(search)));
    let html = '';
    filtered.forEach(c => {
        let due = c.invoices.reduce((s, i) => s + (i.amount - i.paid), 0);
        html += `<div style="border:1px solid #ddd;padding:10px;border-radius:8px;margin-bottom:10px;">
            <strong>${c.name}</strong> ${c.phone ? '📞 ' + c.phone : ''}
            <span style="float:right;">Due: SAR ${due.toFixed(2)}</span>
            <div style="font-size:11px;color:#666;margin-top:5px;">
                ${c.invoices.map(i => `${i.invoiceNo} | ${new Date(i.date).toLocaleDateString()} | SAR ${i.amount.toFixed(2)} | <span class="badge badge-${i.status}">${i.status}</span>`).join('<br>')}
            </div>
        </div>`;
    });
    document.getElementById('customerList').innerHTML = html || '<p>No customers</p>';
}

// ===== INVOICES =====
function displayInvoices() {
    let search = document.getElementById('searchInv').value.toLowerCase();
    let filtered = orders.filter(o => o.invoiceNo.toLowerCase().includes(search) || o.customer.toLowerCase().includes(search)).reverse();
    let html = `<table><thead><tr><th>Invoice</th><th>Date</th><th>Customer</th><th>Total</th><th>Status</th><th>Actions</th></tr></thead><tbody>`;
    filtered.forEach(o => {
        html += `<tr>
            <td>${o.invoiceNo}</td>
            <td>${new Date(o.date).toLocaleDateString()}</td>
            <td>${o.customer}</td>
            <td>SAR ${o.total.toFixed(2)}</td>
            <td><span class="badge badge-${o.status}">${o.status}</span></td>
            <td><button onclick="viewInvoice('${o.invoiceNo}')" class="btn-info" style="padding:2px 8px;">View</button></td>
        </tr>`;
    });
    html += '</tbody></table>';
    document.getElementById('invoiceList').innerHTML = html || '<p>No invoices</p>';
}

function viewInvoice(invNo) {
    let o = orders.find(x => x.invoiceNo === invNo);
    if (!o) return;
    let items = o.items.map(i => `${i.name} ${i.size} x${i.qty} = SAR ${(i.qty * i.price).toFixed(2)}`).join('\n');
    alert(`Invoice: ${o.invoiceNo}\nDate: ${new Date(o.date).toLocaleString()}\nCustomer: ${o.customer}\nPhone: ${o.phone}\n\nItems:\n${items}\n\nTotal: SAR ${o.total.toFixed(2)}\nPaid: SAR ${o.paid.toFixed(2)}\nBalance: SAR ${(o.total - o.paid).toFixed(2)}`);
}

// ===== REPORTS =====
function updateReports() {
    let today = new Date().toISOString().split('T')[0];
    let todayOrders = orders.filter(o => o.date.split('T')[0] === today);
    let totalSales = todayOrders.reduce((s, o) => s + o.total, 0);
    let totalProfit = todayOrders.reduce((s, o) => s + o.profit, 0);
    document.getElementById('todaySales').textContent = 'SAR ' + totalSales.toFixed(2);
    document.getElementById('todayProfit').textContent = 'SAR ' + totalProfit.toFixed(2);
    document.getElementById('todayOrders').textContent = todayOrders.length;
    let html = '';
    todayOrders.forEach(o => {
        html += `<div style="background:white;padding:10px;border-radius:8px;margin-bottom:8px;border-left:3px solid #ffd700;">
            <strong>${o.invoiceNo}</strong> | ${o.customer} | SAR ${o.total.toFixed(2)} | <span class="badge badge-${o.status}">${o.status}</span>
        </div>`;
    });
    document.getElementById('dailyReportList').innerHTML = html || '<p>No orders today</p>';
}

// ===== INIT =====
// Show login first
document.getElementById('loginOverlay').style.display = 'flex';
</script>
</body>
</html>
