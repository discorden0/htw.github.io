<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Card Management System</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            /* Light Theme */
            --primary: #4361ee;
            --primary-dark: #3a0ca3;
            --secondary: #4cc9f0;
            --bg-body: #f8f9fa;
            --bg-panel: #ffffff;
            --text-main: #2b2d42;
            --text-muted: #8d99ae;
            --border: #e9ecef;
            --success: #2a9d8f;
            --danger: #ef233c;
            --warning: #ffb703;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
            --radius: 12px;
            --font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        /* Dark Theme */
        body.dark-mode {
            --primary: #4cc9f0;
            --primary-dark: #4895ef;
            --bg-body: #121212;
            --bg-panel: #1e1e1e;
            --text-main: #edf2f4;
            --text-muted: #a0a0a0;
            --border: #333333;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.5);
        }

        /* RTL for Arabic */
        body.rtl {
            direction: rtl;
            text-align: right;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; transition: background-color 0.3s, color 0.3s; }
        
        body {
            font-family: var(--font-family);
            background-color: var(--bg-body);
            color: var(--text-main);
            height: 100vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }

        /* --- Utilities --- */
        .hidden { display: none !important; }
        .flex { display: flex; }
        .flex-col { flex-direction: column; }
        .items-center { align-items: center; }
        .justify-between { justify-content: space-between; }
        .justify-center { justify-content: center; }
        .gap-2 { gap: 0.5rem; }
        .gap-4 { gap: 1rem; }
        .p-4 { padding: 1rem; }
        .m-4 { margin: 1rem; }
        .w-full { width: 100%; }
        .text-center { text-align: center; }
        .text-sm { font-size: 0.875rem; }
        .text-lg { font-size: 1.25rem; }
        .font-bold { font-weight: bold; }
        
        /* --- Buttons & Inputs --- */
        .btn {
            padding: 0.6rem 1.2rem;
            border: none;
            border-radius: var(--radius);
            cursor: pointer;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            font-size: 0.9rem;
        }
        /* Important: Button base styles to ensure pointer events work */
        .btn:hover { opacity: 0.9; transform: translateY(-1px); }
        .btn:active { transform: translateY(0); }

        .btn-primary { background-color: var(--primary); color: white; }
        .btn-secondary { background-color: var(--border); color: var(--text-main); }
        .btn-danger { background-color: var(--danger); color: white; }
        .btn-success { background-color: var(--success); color: white; }
        
        .input-group { margin-bottom: 1rem; text-align: left; }
        body.rtl .input-group { text-align: right; }
        
        .input-group label { display: block; margin-bottom: 0.4rem; font-size: 0.9rem; color: var(--text-muted); }
        
        .form-control {
            width: 100%;
            padding: 0.7rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            background-color: var(--bg-panel);
            color: var(--text-main);
            font-size: 1rem;
        }
        .form-control:focus { outline: 2px solid var(--primary); border-color: transparent; }

        /* --- Layout --- */
        #app-container { display: flex; height: 100%; width: 100%; }

        /* Sidebar */
        .sidebar {
            width: 260px;
            background-color: var(--bg-panel);
            border-left: 1px solid var(--border);
            display: flex;
            flex-direction: column;
            padding: 1rem;
            z-index: 10;
        }
        body.rtl .sidebar { border-left: none; border-right: 1px solid var(--border); }

        .brand { font-size: 1.5rem; font-weight: bold; color: var(--primary); margin-bottom: 2rem; display: flex; align-items: center; gap: 0.5rem; }

        .nav-item {
            padding: 0.8rem 1rem;
            margin-bottom: 0.5rem;
            border-radius: var(--radius);
            cursor: pointer;
            color: var(--text-muted);
            display: flex;
            align-items: center;
            gap: 0.8rem;
        }
        .nav-item:hover, .nav-item.active {
            background-color: rgba(67, 97, 238, 0.1);
            color: var(--primary);
        }

        /* Main Content */
        .main-content {
            flex: 1;
            overflow-y: auto;
            padding: 1.5rem;
            position: relative;
        }

        /* Cards Grid */
        .card-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 1.5rem; }
        
        .card-item {
            background-color: var(--bg-panel);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            position: relative;
        }
        .card-img-top { width: 100%; height: 150px; object-fit: cover; background-color: #eee; }
        .card-body { padding: 1rem; flex: 1; }
        .card-title { font-weight: bold; margin-bottom: 0.5rem; }
        .card-price { color: var(--primary); font-weight: bold; font-size: 1.1rem; }
        .card-stock { font-size: 0.85rem; color: var(--text-muted); }
        .card-actions { padding: 1rem; border-top: 1px solid var(--border); display: flex; justify-content: space-between; }

        /* Login Screen */
        #login-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background-color: var(--bg-body);
            z-index: 2000;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .login-box {
            background-color: var(--bg-panel);
            padding: 2.5rem;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            width: 100%;
            max-width: 400px;
            text-align: center;
        }

        /* Modals */
        .modal-overlay {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(0,0,0,0.5);
            display: flex; align-items: center; justify-content: center;
            z-index: 1500;
            opacity: 0; pointer-events: none; transition: opacity 0.3s;
        }
        .modal-overlay.open { opacity: 1; pointer-events: auto; }
        
        .modal-content {
            background-color: var(--bg-panel);
            padding: 1.5rem;
            border-radius: var(--radius);
            width: 90%; max-width: 500px;
            max-height: 90vh; overflow-y: auto;
        }

        /* Toast */
        #toast-container { position: fixed; bottom: 20px; right: 20px; z-index: 3000; display: flex; flex-direction: column; gap: 10px; }
        body.rtl #toast-container { right: auto; left: 20px; }
        .toast {
            background-color: var(--bg-panel); color: var(--text-main);
            padding: 1rem 1.5rem; border-radius: var(--radius);
            box-shadow: var(--shadow); border-left: 4px solid var(--primary);
            animation: slideIn 0.3s ease-out; min-width: 250px;
            display: flex; align-items: center; gap: 10px;
        }
        @keyframes slideIn { from { transform: translateX(100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }

        /* Tables */
        .table-container { overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; margin-top: 1rem; }
        th, td { padding: 0.8rem; text-align: left; border-bottom: 1px solid var(--border); }
        body.rtl th, body.rtl td { text-align: right; }
        th { color: var(--text-muted); font-weight: 600; font-size: 0.9rem; }

        /* Stats */
        .stat-card { background-color: var(--bg-panel); padding: 1.5rem; border-radius: var(--radius); box-shadow: var(--shadow); display: flex; align-items: center; justify-content: space-between; }
        .stat-icon { font-size: 2rem; opacity: 0.2; }

        /* Print */
        #print-area { display: none; }
        @media print { body * { visibility: hidden; } #print-area, #print-area * { visibility: visible; } #print-area { display: block; position: absolute; left: 0; top: 0; width: 100%; background: white; color: black; padding: 20px; } .no-print { display: none; } }

        /* Responsive */
        @media (max-width: 768px) {
            .sidebar { position: fixed; bottom: 0; left: 0; width: 100%; height: 60px; flex-direction: row; border-left: none; border-top: 1px solid var(--border); padding: 0; justify-content: space-around; }
            .brand, .user-info { display: none; }
            .nav-item span { display: none; } .nav-item { margin: 0; font-size: 1.5rem; height: 100%; padding: 0; justify-content: center; }
            .main-content { padding-bottom: 80px; }
        }
    </style>
</head>
<body class="light-mode ltr">

    <div id="toast-container"></div>

    <!-- LOGIN SCREEN -->
    <div id="login-screen">
        <div class="login-box">
            <div class="brand justify-center mb-4">
                <i class="fa-solid fa-layer-group"></i> <span id="login-title">Card Manager</span>
            </div>
            <div class="flex justify-between mb-4 text-sm">
                <button class="btn btn-secondary" onclick="app.toggleTheme()" id="theme-toggle-btn"><i class="fa-solid fa-moon"></i></button>
                <button class="btn btn-secondary" onclick="app.toggleLang()" id="lang-toggle-btn">EN</button>
            </div>
            <form id="login-form">
                <div class="input-group">
                    <input type="text" id="username" class="form-control" placeholder="Username (admin)" required>
                </div>
                <div class="input-group">
                    <input type="password" id="password" class="form-control" placeholder="Password (admin)" required>
                </div>
                <button type="submit" class="btn btn-primary w-full" id="login-btn">Login</button>
            </form>
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="app-container" class="hidden">
        <aside class="sidebar">
            <div class="brand"><i class="fa-solid fa-layer-group"></i> <span data-i18n="appTitle">Card System</span></div>
            <nav>
                <div class="nav-item active" onclick="app.navigate('dashboard')"><i class="fa-solid fa-chart-pie"></i> <span data-i18n="navDashboard">Dashboard</span></div>
                <div class="nav-item" onclick="app.navigate('inventory')"><i class="fa-solid fa-box"></i> <span data-i18n="navInventory">Inventory</span></div>
                <div class="nav-item" onclick="app.navigate('pos')"><i class="fa-solid fa-cash-register"></i> <span data-i18n="navPOS">Point of Sale</span></div>
                <div class="nav-item" onclick="app.navigate('debt')"><i class="fa-solid fa-hand-holding-dollar"></i> <span data-i18n="navDebt">Debtors</span></div>
                <div class="nav-item" onclick="app.navigate('reports')"><i class="fa-solid fa-file-invoice"></i> <span data-i18n="navReports">Reports</span></div>
                <div class="nav-item" onclick="app.navigate('users')" id="nav-users"><i class="fa-solid fa-users-gear"></i> <span data-i18n="navUsers">Users</span></div>
                <div class="nav-item" onclick="app.navigate('shift')"><i class="fa-solid fa-clock"></i> <span data-i18n="navShift">Shift</span></div>
            </nav>
            <div style="margin-top: auto;">
                <div class="user-info p-4 border-top" style="border-color: var(--border)">
                    <div id="current-user-display" class="font-bold">Admin</div>
                    <div class="text-sm text-muted" id="shift-status-display">Shift: Off</div>
                </div>
                <button class="btn btn-danger w-full" onclick="auth.logout()"><i class="fa-solid fa-sign-out-alt"></i> <span data-i18n="btnLogout">Logout</span></button>
            </div>
        </aside>

        <main class="main-content">
            <header class="flex justify-between items-center mb-4">
                <h2 id="page-title" class="text-xl">Dashboard</h2>
                <div class="flex gap-2">
                    <button class="btn btn-secondary" onclick="app.toggleTheme()"><i class="fa-solid fa-adjust"></i></button>
                    <button class="btn btn-secondary" onclick="app.toggleLang()"><i class="fa-solid fa-globe"></i></button>
                </div>
            </header>

            <!-- DASHBOARD -->
            <section id="view-dashboard" class="view-section">
                <div class="card-grid mb-4" style="grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));">
                    <div class="stat-card"><div><div class="text-muted text-sm" data-i18n="statSales">Sales Today</div><div class="text-xl font-bold" id="dash-sales">$0</div></div><i class="fa-solid fa-coins stat-icon text-success"></i></div>
                    <div class="stat-card"><div><div class="text-muted text-sm" data-i18n="statTransactions">Transactions</div><div class="text-xl font-bold" id="dash-trans">0</div></div><i class="fa-solid fa-receipt stat-icon text-primary"></i></div>
                    <div class="stat-card"><div><div class="text-muted text-sm" data-i18n="statDebt">Total Debt</div><div class="text-xl font-bold" id="dash-debt">$0</div></div><i class="fa-solid fa-hand-holding-dollar stat-icon text-danger"></i></div>
                </div>
                <div class="card-grid" style="grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));">
                    <div class="stat-card"><div><div class="text-muted text-sm" data-i18n="statCardsLeft">Cards Left</div><div class="text-xl font-bold" id="dash-stock">0</div></div><i class="fa-solid fa-box-open stat-icon text-warning"></i></div>
                    <div class="stat-card"><div><div class="text-muted text-sm" data-i18n="statActiveUsers">Active Users</div><div class="text-xl font-bold" id="dash-users">1</div></div><i class="fa-solid fa-users stat-icon text-secondary"></i></div>
                </div>
            </section>

            <!-- INVENTORY -->
            <section id="view-inventory" class="view-section hidden">
                <div class="flex justify-between mb-4">
                    <input type="text" id="inv-search" class="form-control w-full" placeholder="Search..." onkeyup="app.renderInventory()">
                    <button class="btn btn-primary" onclick="app.openCardModal()"><i class="fa-solid fa-plus"></i> <span data-i18n="btnAddCard">Add Card</span></button>
                </div>
                <div id="inventory-list" class="card-grid"></div>
            </section>

            <!-- POS -->
            <section id="view-pos" class="view-section hidden">
                <div class="flex gap-4" style="height: 100%;">
                    <div class="w-full md:w-2/3">
                        <div class="input-group">
                            <label data-i18n="lblSelectCard">Select Card</label>
                            <select id="pos-card-select" class="form-control" onchange="pos.updatePrice()">
                                <!-- Options injected -->
                            </select>
                        </div>
                        <div class="card-grid" id="pos-card-preview"></div>
                    </div>
                    <div class="w-full md:w-1/3 bg-panel p-4" style="background-color: var(--bg-panel); border-radius: var(--radius); height: fit-content;">
                        <h3 class="mb-4" data-i18n="titleNewSale">New Sale</h3>
                        <!-- Removed <form> tag to prevent submit reload, using div instead -->
                        <div>
                            <div class="input-group">
                                <label data-i18n="lblQuantity">Quantity</label>
                                <input type="number" id="pos-qty" class="form-control" value="1" min="1" onchange="pos.updateTotal()">
                            </div>
                            <div class="input-group">
                                <label data-i18n="lblPricePerUnit">Price Per Unit</label>
                                <input type="number" id="pos-price" class="form-control" readonly>
                            </div>
                            <div class="input-group">
                                <label data-i18n="lblTotal">Total</label>
                                <input type="number" id="pos-total" class="form-control" readonly>
                            </div>
                            <div class="input-group">
                                <label data-i18n="lblPaymentType">Payment Type</label>
                                <div class="flex gap-2">
                                    <button class="btn btn-secondary w-full active" id="btn-pay-cash" onclick="pos.setPaymentType('cash')"><i class="fa-solid fa-money-bill"></i> <span data-i18n="btnCash">Cash</span></button>
                                    <button class="btn btn-secondary w-full" id="btn-pay-debt" onclick="pos.setPaymentType('debt')"><i class="fa-solid fa-book"></i> <span data-i18n="btnDebt">Credit</span></button>
                                </div>
                            </div>
                            <div id="pos-debtor-select-container" class="input-group hidden">
                                <label data-i18n="lblSelectDebtor">Select Debtor</label>
                                <select id="pos-debtor-select" class="form-control"></select>
                            </div>
                            <!-- Changed to type="button" to prevent reload -->
                            <button type="button" class="btn btn-primary w-full" onclick="pos.completeSale()" data-i18n="btnConfirmSale">Confirm Sale</button>
                        </div>
                    </div>
                </div>
            </section>

            <!-- DEBT -->
            <section id="view-debt" class="view-section hidden">
                <div class="flex justify-between mb-4">
                    <h3 data-i18n="titleDebtors">Debtors List</h3>
                    <button class="btn btn-primary" onclick="app.openDebtorModal()"><i class="fa-solid fa-user-plus"></i> <span data-i18n="btnAddDebtor">Add Person</span></button>
                </div>
                <div class="table-container"><table id="debt-table"><thead><tr><th data-i18n="colName">Name</th><th data-i18n="colTotalDebt">Total Debt</th><th data-i18n="colActions">Actions</th></tr></thead><tbody></tbody></table></div>
            </section>

            <!-- REPORTS -->
            <section id="view-reports" class="view-section hidden">
                <div class="flex gap-2 mb-4">
                    <button class="btn btn-secondary" onclick="reports.generate('daily')">Daily</button>
                    <button class="btn btn-secondary" onclick="reports.generate('weekly')">Weekly</button>
                    <button class="btn btn-secondary" onclick="reports.generate('monthly')">Monthly</button>
                    <button class="btn btn-primary" onclick="reports.exportData()" data-i18n="btnExportData">Export Data</button>
                </div>
                <div id="report-content" class="bg-panel p-4" style="background-color: var(--bg-panel); border-radius: var(--radius);"><p class="text-center text-muted" data-i18n="msgSelectPeriod">Select a period to view report.</p></div>
            </section>

            <!-- USERS -->
            <section id="view-users" class="view-section hidden">
                <div class="flex justify-between mb-4">
                    <h3 data-i18n="titleUserMgmt">User Management</h3>
                    <button class="btn btn-primary" onclick="app.openUserModal()"><i class="fa-solid fa-user-plus"></i> <span data-i18n="btnAddUser">Add User</span></button>
                </div>
                <div class="table-container"><table id="users-table"><thead><tr><th data-i18n="colUsername">Username</th><th data-i18n="colRole">Role</th><th data-i18n="colActions">Actions</th></tr></thead><tbody></tbody></table></div>
            </section>

            <!-- SHIFT -->
            <section id="view-shift" class="view-section hidden">
                <div class="text-center mt-4">
                    <div id="shift-status-icon" class="text-xl mb-2"><i class="fa-regular fa-circle text-muted"></i></div>
                    <h2 id="shift-state-text" class="mb-4">Shift Not Started</h2>
                    <div id="shift-actions">
                        <button id="btn-start-shift" class="btn btn-success" onclick="shiftManager.startShiftPrompt()"><i class="fa-solid fa-play"></i> <span data-i18n="btnStartShift">Start Shift</span></button>
                        <button id="btn-end-shift" class="btn btn-danger hidden" onclick="shiftManager.endShiftPrompt()"><i class="fa-solid fa-stop"></i> <span data-i18n="btnEndShift">End Shift</span></button>
                    </div>
                </div>
                <div id="shift-info" class="mt-4 hidden p-4 border-top"><h4>Current Shift Details</h4><p>Started: <span id="shift-start-time">--:--</span></p><p>Sales: $<span id="shift-sales-total">0.00</span></p></div>
            </section>
        </main>
    </div>

    <!-- MODALS -->
    <div id="modal-card" class="modal-overlay">
        <div class="modal-content">
            <h3 class="mb-4" id="modal-card-title">Add Card</h3>
            <form onsubmit="app.saveCard(event)">
                <input type="hidden" id="card-id">
                <div class="input-group"><label>Name</label><input type="text" id="card-name" class="form-control" required></div>
                <div class="input-group"><label>Category</label><select id="card-category" class="form-control"><option value="Standard">Standard</option><option value="Premium">Premium</option></select></div>
                <div class="input-group"><label>Price</label><input type="number" id="card-price" class="form-control" step="0.01" required></div>
                <div class="input-group"><label>Current Stock</label><input type="number" id="card-stock" class="form-control" required></div>
                <div class="input-group"><label>Image URL</label><input type="text" id="card-img" class="form-control" placeholder="https://..."></div>
                <div class="flex justify-between">
                    <button type="button" class="btn btn-secondary" onclick="app.closeModal('modal-card')">Cancel</button>
                    <button type="submit" class="btn btn-primary">Save</button>
                </div>
            </form>
        </div>
    </div>

    <div id="modal-debtor" class="modal-overlay">
        <div class="modal-content">
            <h3 class="mb-4">Add Debtor</h3>
            <form onsubmit="app.saveDebtor(event)">
                <div class="input-group"><label>Name</label><input type="text" id="debtor-name" class="form-control" required></div>
                <div class="flex justify-between">
                    <button type="button" class="btn btn-secondary" onclick="app.closeModal('modal-debtor')">Cancel</button>
                    <button type="submit" class="btn btn-primary">Save</button>
                </div>
            </form>
        </div>
    </div>

    <div id="modal-user" class="modal-overlay">
        <div class="modal-content">
            <h3 class="mb-4" id="modal-user-title">Add User</h3>
            <form onsubmit="app.saveUser(event)">
                <input type="hidden" id="user-id">
                <div class="input-group"><label>Username</label><input type="text" id="user-name" class="form-control" required></div>
                <div class="input-group"><label>Password</label><input type="password" id="user-pass" class="form-control" required></div>
                <div class="input-group"><label>Role</label><select id="user-role" class="form-control"><option value="admin">Admin</option><option value="normal">Normal</option></select></div>
                <div class="flex justify-between">
                    <button type="button" class="btn btn-secondary" onclick="app.closeModal('modal-user')">Cancel</button>
                    <button type="submit" class="btn btn-primary">Save</button>
                </div>
            </form>
        </div>
    </div>

    <div id="modal-stock-count" class="modal-overlay">
        <div class="modal-content">
            <h3 class="mb-4" id="stock-count-title">Count Inventory</h3>
            <p class="text-sm text-muted mb-4">Please verify the physical count.</p>
            <form onsubmit="shiftManager.submitStockCount(event)">
                <div id="stock-count-list" class="flex flex-col gap-2"></div>
                <div class="flex justify-between mt-4">
                    <button type="button" class="btn btn-secondary" onclick="app.closeModal('modal-stock-count')">Cancel</button>
                    <button type="submit" class="btn btn-primary">Confirm</button>
                </div>
            </form>
        </div>
    </div>

    <div id="print-area"></div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // This ensures the script runs only after the HTML is fully parsed
            // Solving potential "buttons not found" issues
            
            const translations = {
                en: { appTitle: "Card Manager", navDashboard: "Dashboard", navInventory: "Inventory", navPOS: "Point of Sale", navDebt: "Debtors", navReports: "Reports", navUsers: "Users", navShift: "Shift", btnLogout: "Logout", statSales: "Sales Today", statTransactions: "Transactions", statDebt: "Total Debt", statCardsLeft: "Cards Left", statActiveUsers: "Active Users", btnAddCard: "Add Card", lblSelectCard: "Select Card", lblQuantity: "Quantity", lblPricePerUnit: "Price Per Unit", lblTotal: "Total", lblPaymentType: "Payment Type", btnCash: "Cash", btnDebt: "Credit", lblSelectDebtor: "Select Debtor", btnConfirmSale: "Confirm Sale", titleDebtors: "Debtors List", btnAddDebtor: "Add Person", colName: "Name", colTotalDebt: "Total Debt", colActions: "Actions", titleUserMgmt: "User Management", btnAddUser: "Add User", colUsername: "Username", colRole: "Role", btnStartShift: "Start Shift", btnEndShift: "End Shift", msgSelectPeriod: "Select a period to view report.", btnExportData: "Export Data", toastSaved: "Saved Successfully", toastDeleted: "Deleted Successfully", toastSaleComplete: "Sale Completed", toastShiftStarted: "Shift Started", toastShiftEnded: "Shift Ended" },
                ar: { appTitle: "نظام إدارة البطاقات", navDashboard: "لوحة التحكم", navInventory: "المخزون", navPOS: "نقطة البيع", navDebt: "الديون", navReports: "التقارير", navUsers: "المستخدمين", navShift: "الورديات", btnLogout: "تسجيل خروج", statSales: "مبيعات اليوم", statTransactions: "عمليات البيع", statDebt: "إجمالي الديون", statCardsLeft: "البطاقات المتبقية", statActiveUsers: "المستخدمون النشطون", btnAddCard: "إضافة بطاقة", lblSelectCard: "اختر البطاقة", lblQuantity: "الكمية", lblPricePerUnit: "سعر الوحدة", lblTotal: "الإجمالي", lblPaymentType: "طريقة الدفع", btnCash: "نقدي", btnDebt: "آجل", lblSelectDebtor: "اختر المدين", btnConfirmSale: "تأكيد البيع", titleDebtors: "قائمة المدينين", btnAddDebtor: "إضافة شخص", colName: "الاسم", colTotalDebt: "إجمالي الدين", colActions: "إجراءات", titleUserMgmt: "إدارة المستخدمين", btnAddUser: "إضافة مستخدم", colUsername: "اسم المستخدم", colRole: "الدور", btnStartShift: "بدء الوردية", btnEndShift: "إنهاء الوردية", msgSelectPeriod: "اختر فترة لعرض التقرير.", btnExportData: "تصدير البيانات", toastSaved: "تم الحفظ", toastDeleted: "تم الحذف", toastSaleComplete: "تم البيع", toastShiftStarted: "بدأت الوردية", toastShiftEnded: "انتهت الوردية" }
            };

            const utils = {
                id: () => '_' + Math.random().toString(36).substr(2, 9),
                hash: (str) => { let h=0; for(let i=0;i<str.length;i++) h = Math.imul(31,h)+str.charCodeAt(i)|0; return h.toString(); },
                formatMoney: (a) => '$'+parseFloat(a).toFixed(2),
                getDate: () => new Date().toISOString().split('T')[0],
                toast: (msg, type='success') => {
                    const c = document.getElementById('toast-container');
                    const el = document.createElement('div'); el.className='toast';
                    el.innerHTML = `<i class="fa-solid fa-${type==='success'?'check-circle':'exclamation-circle'} text-${type==='success'?'success':'danger'}"></i> <span>${msg}</span>`;
                    c.appendChild(el);
                    setTimeout(()=>{el.style.opacity='0';setTimeout(()=>el.remove(),300)},3000);
                }
            };

            const store = {
                dbName: 'cardManagerDB',
                data: { users: [], cards: [], debtors: [], sales: [], shifts: [], settings: { shopName: 'My Card Shop' } },
                load: () => {
                    const s = localStorage.getItem(store.dbName);
                    if(s) store.data = JSON.parse(s);
                    else {
                        store.data.users.push({id:'a1', username:'admin', passwordHash:utils.hash('admin'), role:'admin'});
                        store.data.cards.push({id:utils.id(), name:'$10 Card', category:'Standard', price:10, stock:50, img:''});
                        store.save();
                    }
                },
                save: () => localStorage.setItem(store.dbName, JSON.stringify(store.data))
            };

            const auth = {
                currentUser: null,
                login: (u,p) => {
                    const user = store.data.users.find(x=>x.username===u && x.passwordHash===utils.hash(p));
                    if(user){
                        auth.currentUser = user;
                        document.getElementById('login-screen').classList.add('hidden');
                        document.getElementById('app-container').classList.remove('hidden');
                        app.init();
                        return true;
                    }
                    return false;
                },
                logout: () => {
                    auth.currentUser = null;
                    document.getElementById('login-screen').classList.remove('hidden');
                    document.getElementById('app-container').classList.add('hidden');
                },
                can: (action) => auth.currentUser.role === 'admin'
            };

            const shiftManager = {
                activeShift: null,
                startShiftPrompt: () => shiftManager.openStockCount('opening'),
                endShiftPrompt: () => shiftManager.openStockCount('closing'),
                openStockCount: (type) => {
                    const list = document.getElementById('stock-count-list');
                    list.innerHTML = '';
                    document.getElementById('stock-count-title').innerText = type==='opening'?'Opening Stock':'Closing Stock';
                    store.data.cards.forEach(c => {
                        const row = document.createElement('div');
                        row.className = 'flex justify-between items-center input-group'; row.style.margin='0';
                        row.innerHTML = `<span>${c.name}</span><input type="number" class="form-control stock-count-input" data-id="${c.id}" value="${c.stock}" style="max-width:80px">`;
                        list.appendChild(row);
                    });
                    document.getElementById('modal-stock-count').classList.add('open');
                    shiftManager.pendingAction = type;
                },
                submitStockCount: (e) => {
                    e.preventDefault();
                    const counts = {};
                    document.querySelectorAll('.stock-count-input').forEach(i => counts[i.dataset.id] = parseInt(i.value));
                    if(shiftManager.pendingAction === 'opening') shiftManager.start(counts);
                    else shiftManager.end(counts);
                    app.closeModal('modal-stock-count');
                },
                start: (counts) => {
                    shiftManager.activeShift = { id:utils.id(), userId:auth.currentUser.username, startTime:Date.now(), openingStock:counts, sales:[] };
                    shiftManager.updateUI();
                    utils.toast(translations[app.lang].toastShiftStarted);
                },
                end: (counts) => {
                    const s = shiftManager.activeShift;
                    s.endTime = Date.now(); s.closingStock = counts;
                    store.data.shifts.push(s); store.save();
                    reports.generateShiftReport(s);
                    shiftManager.activeShift = null; shiftManager.updateUI();
                    utils.toast(translations[app.lang].toastShiftEnded);
                    setTimeout(auth.logout, 2000);
                },
                updateUI: () => {
                    const active = !!shiftManager.activeShift;
                    document.getElementById('btn-start-shift').classList.toggle('hidden', active);
                    document.getElementById('btn-end-shift').classList.toggle('hidden', !active);
                    document.getElementById('shift-info').classList.toggle('hidden', !active);
                    document.getElementById('shift-state-text').innerText = active?"Shift Active":"Shift Not Started";
                    document.getElementById('shift-status-display').innerText = active?"Shift: Active":"Shift: Off";
                }
            };

            const pos = {
                paymentType: 'cash',
                init: () => { pos.renderSelects(); },
                renderSelects: () => {
                    const c = document.getElementById('pos-card-select');
                    c.innerHTML = '<option value="">-- Select --</option>';
                    store.data.cards.forEach(x => c.innerHTML += `<option value="${x.id}" data-price="${x.price}" data-stock="${x.stock}">${x.name} ($${x.price})</option>`);
                    const d = document.getElementById('pos-debtor-select');
                    d.innerHTML = '<option value="">-- New Debtor --</option>';
                    store.data.debtors.forEach(x => d.innerHTML += `<option value="${x.id}">${x.name}</option>`);
                },
                updatePrice: () => {
                    const opt = document.getElementById('pos-card-select').options[document.getElementById('pos-card-select').selectedIndex];
                    if(opt && opt.value){
                        document.getElementById('pos-price').value = opt.dataset.price;
                        pos.updateTotal();
                        const card = store.data.cards.find(x=>x.id===opt.value);
                        document.getElementById('pos-card-preview').innerHTML = `<div class="card-item"><img src="${card.img||'https://picsum.photos/seed/'+card.id+'/200/150'}" class="card-img-top"><div class="card-body"><div class="card-title">${card.name}</div><div class="card-stock">${card.stock} left</div></div></div>`;
                    } else {
                        document.getElementById('pos-total').value = 0;
                        document.getElementById('pos-card-preview').innerHTML = '';
                    }
                },
                updateTotal: () => {
                    const p = parseFloat(document.getElementById('pos-price').value)||0;
                    const q = parseInt(document.getElementById('pos-qty').value)||0;
                    document.getElementById('pos-total').value = (p*q).toFixed(2);
                },
                setPaymentType: (t) => {
                    pos.paymentType = t;
                    document.getElementById('pos-debtor-select-container').classList.toggle('hidden', t!=='debt');
                    document.getElementById('btn-pay-cash').className = `btn w-full ${t==='cash'?'btn-primary':'btn-secondary'}`;
                    document.getElementById('btn-pay-debt').className = `btn w-full ${t==='debt'?'btn-primary':'btn-secondary'}`;
                },
                completeSale: () => {
                    const cid = document.getElementById('pos-card-select').value;
                    const qty = parseInt(document.getElementById('pos-qty').value);
                    const total = parseFloat(document.getElementById('pos-total').value);
                    if(!cid || qty<=0) return utils.toast('Invalid details', 'error');
                    const card = store.data.cards.find(x=>x.id===cid);
                    if(card.stock < qty) return utils.toast('Out of stock', 'error');
                    
                    card.stock -= qty;
                    const sale = { id:utils.id(), cardId:cid, cardName:card.name, qty, total, type:pos.paymentType, date:Date.now(), user:auth.currentUser.username, dateStr:utils.getDate() };
                    
                    if(pos.paymentType === 'debt'){
                        const did = document.getElementById('pos-debtor-select').value;
                        if(!did) return utils.toast('Select debtor', 'error');
                        const debtor = store.data.debtors.find(x=>x.id===did);
                        debtor.totalDebt += total;
                        sale.debtorName = debtor.name;
                    }
                    
                    store.data.sales.push(sale);
                    if(shiftManager.activeShift) shiftManager.activeShift.sales.push(sale);
                    store.save();
                    utils.toast(translations[app.lang].toastSaleComplete);
                    pos.renderSelects();
                    document.getElementById('pos-qty').value = 1;
                    document.getElementById('pos-card-preview').innerHTML = '';
                    document.getElementById('pos-price').value = '';
                    document.getElementById('pos-total').value = '';
                }
            };

            const reports = {
                generate: (p) => {
                    const sales = store.data.sales;
                    const filtered = p==='daily'?sales.filter(x=>x.dateStr===utils.getDate()):sales; // simplified filter
                    const total = filtered.reduce((a,b)=>a+b.total,0);
                    document.getElementById('report-content').innerHTML = `
                        <h4 class="mb-2">${p} Report</h4>
                        <div class="flex justify-between mb-2 border-bottom p-2"><span>Total Sales:</span><span class="font-bold">${utils.formatMoney(total)}</span></div>
                        <table class="text-sm"><thead><tr><th>Date</th><th>Item</th><th>Total</th></tr></thead><tbody>${filtered.map(s=>`<tr><td>${new Date(s.date).toLocaleTimeString()}</td><td>${s.cardName}</td><td>${utils.formatMoney(s.total)}</td></tr>`).join('')}</tbody></table>`;
                },
                generateShiftReport: (s) => {
                    const total = s.sales.reduce((a,b)=>a+b.total,0);
                    document.getElementById('print-area').innerHTML = `<div style="font-family:monospace;text-align:center"><h3>${store.data.settings.shopName}</h3><p>User: ${s.userId}</p><hr><table style="width:100%;text-align:left">${s.sales.map(x=>`<tr><td>${x.cardName}</td><td>${x.qty}</td><td>${utils.formatMoney(x.total)}</td></tr>`).join('')}</table><h3>Total: ${utils.formatMoney(total)}</h3></div>`;
                    window.print();
                },
                exportData: () => {
                    const a = document.createElement('a'); a.href="data:text/json;charset=utf-8,"+encodeURIComponent(JSON.stringify(store.data)); a.download="backup.json"; a.click();
                }
            };

            const app = {
                lang: 'en',
                init: () => {
                    app.updateLang();
                    app.renderDashboard(); app.renderInventory(); app.renderUsers(); app.renderDebtors(); pos.init(); shiftManager.updateUI();
                    if(auth.currentUser.role !== 'admin') document.getElementById('nav-users').classList.add('hidden');
                    document.getElementById('current-user-display').innerText = auth.currentUser.username;
                },
                toggleLang: () => {
                    app.lang = app.lang==='en'?'ar':'en';
                    document.body.classList.toggle('rtl');
                    document.getElementById('lang-toggle-btn').innerText = app.lang.toUpperCase();
                    app.updateLang();
                },
                toggleTheme: () => document.body.classList.toggle('dark-mode'),
                updateLang: () => {
                    document.querySelectorAll('[data-i18n]').forEach(el => el.innerText = translations[app.lang][el.getAttribute('data-i18n')] || el.innerText);
                },
                navigate: (v) => {
                    document.querySelectorAll('.view-section').forEach(e=>e.classList.add('hidden'));
                    document.getElementById('view-'+v).classList.remove('hidden');
                    document.querySelectorAll('.nav-item').forEach(e=>e.classList.remove('active'));
                    const target = Array.from(document.querySelectorAll('.nav-item')).find(e => e.getAttribute('onclick').includes(v));
                    if(target) target.classList.add('active');
                    document.getElementById('page-title').innerText = translations[app.lang]['nav'+v.charAt(0).toUpperCase()+v.slice(1)];
                    if(v==='dashboard') app.renderDashboard();
                    if(v==='inventory') app.renderInventory();
                },
                renderDashboard: () => {
                    const today = utils.getDate();
                    const sales = store.data.sales.filter(x=>x.dateStr===today);
                    document.getElementById('dash-sales').innerText = utils.formatMoney(sales.reduce((a,b)=>a+b.total,0));
                    document.getElementById('dash-trans').innerText = sales.length;
                    document.getElementById('dash-debt').innerText = utils.formatMoney(store.data.debtors.reduce((a,b)=>a+b.totalDebt,0));
                    document.getElementById('dash-stock').innerText = store.data.cards.reduce((a,b)=>a+b.stock,0);
                },
                renderInventory: () => {
                    const list = document.getElementById('inventory-list'); list.innerHTML = '';
                    const search = document.getElementById('inv-search').value.toLowerCase();
                    store.data.cards.forEach(c => {
                        if(c.name.toLowerCase().includes(search)){
                            const div = document.createElement('div');
                            div.className = 'card-item';
                            div.innerHTML = `<img src="${c.img||'https://picsum.photos/seed/'+c.id+'/200/150'}" class="card-img-top"><div class="card-body"><div class="card-title">${c.name}</div><div class="text-sm text-muted">${c.category}</div><div class="card-price mt-2">${utils.formatMoney(c.price)}</div><div class="card-stock mt-1">Stock: ${c.stock}</div></div><div class="card-actions"><button class="btn btn-secondary text-sm" onclick="app.editCard('${c.id}')"><i class="fa-solid fa-pen"></i></button><button class="btn btn-danger text-sm" onclick="app.deleteCard('${c.id}')"><i class="fa-solid fa-trash"></i></button></div>`;
                            list.appendChild(div);
                        }
                    });
                },
                renderUsers: () => {
                    const tb = document.querySelector('#users-table tbody'); tb.innerHTML='';
                    store.data.users.forEach(u => {
                        tb.innerHTML += `<tr><td>${u.username}</td><td>${u.role}</td><td>${u.id!=='a1'?`<button class="btn btn-danger text-sm" onclick="app.deleteUser('${u.id}')"><i class="fa-solid fa-trash"></i></button>`:''}</td></tr>`;
                    });
                },
                renderDebtors: () => {
                    const tb = document.querySelector('#debt-table tbody'); tb.innerHTML='';
                    store.data.debtors.forEach(d => {
                        tb.innerHTML += `<tr><td>${d.name}</td><td style="color:var(--danger)">${utils.formatMoney(d.totalDebt)}</td><td><button class="btn btn-secondary text-sm" onclick="app.repayDebt('${d.id}')">Repay</button></td></tr>`;
                    });
                },
                // Modals
                openCardModal: () => { document.getElementById('card-id').value=''; document.getElementById('modal-card').classList.add('open'); },
                editCard: (id) => {
                    const c = store.data.cards.find(x=>x.id===id);
                    document.getElementById('card-id').value = c.id; document.getElementById('card-name').value = c.name; document.getElementById('card-category').value = c.category; document.getElementById('card-price').value = c.price; document.getElementById('card-stock').value = c.stock;
                    document.getElementById('modal-card').classList.add('open');
                },
                saveCard: (e) => {
                    e.preventDefault();
                    const id = document.getElementById('card-id').value;
                    const data = { name:document.getElementById('card-name').value, category:document.getElementById('card-category').value, price:parseFloat(document.getElementById('card-price').value), stock:parseInt(document.getElementById('card-stock').value) };
                    if(id) Object.assign(store.data.cards.find(x=>x.id===id), data);
                    else store.data.cards.push({...data, id:utils.id(), img:''});
                    store.save(); app.closeModal('modal-card'); app.renderInventory(); utils.toast(translations[app.lang].toastSaved);
                },
                deleteCard: (id) => { if(confirm('Delete?')){ store.data.cards = store.data.cards.filter(x=>x.id!==id); store.save(); app.renderInventory(); utils.toast(translations[app.lang].toastDeleted); }},
                openDebtorModal: () => { document.getElementById('debtor-name').value=''; document.getElementById('modal-debtor').classList.add('open'); },
                saveDebtor: (e) => { e.preventDefault(); store.data.debtors.push({id:utils.id(), name:document.getElementById('debtor-name').value, totalDebt:0}); store.save(); app.closeModal('modal-debtor'); app.renderDebtors(); pos.renderSelects(); },
                repayDebt: (id) => { const amt = parseFloat(prompt('Amount?')); if(!isNaN(amt)){ const d = store.data.debtors.find(x=>x.id===id); d.totalDebt -= amt; store.save(); app.renderDebtors(); }},
                openUserModal: () => { document.getElementById('user-id').value=''; document.getElementById('modal-user').classList.add('open'); },
                saveUser: (e) => { e.preventDefault(); store.data.users.push({id:utils.id(), username:document.getElementById('user-name').value, passwordHash:utils.hash(document.getElementById('user-pass').value), role:document.getElementById('user-role').value}); store.save(); app.closeModal('modal-user'); app.renderUsers(); },
                deleteUser: (id) => { if(confirm('Delete?')){ store.data.users = store.data.users.filter(x=>x.id!==id); store.save(); app.renderUsers(); }},
                closeModal: (id) => document.getElementById(id).classList.remove('open')
            };

            // --- Startup ---
            store.load();
            
            document.getElementById('login-form').addEventListener('submit', (e) => {
                e.preventDefault();
                if(!auth.login(document.getElementById('username').value, document.getElementById('password').value)) utils.toast('Invalid credentials','error');
            });

            // Expose functions to window for onclick handlers
            window.app = app;
            window.auth = auth;
            window.pos = pos;
            window.shiftManager = shiftManager;
            window.reports = reports;
        });
    </script>
</body>
</html>
