<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CZONESTORE</title>
    <!-- Tailwind CSS & FontAwesome -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;700;900&display=swap" rel="stylesheet">
    
    <style>
        /* Base Styles */
        body { 
            font-family: 'Outfit', sans-serif; 
            background-color: #0b0f1a; 
            color: white; 
            overflow-x: hidden; 
        }
        .glass { 
            background: rgba(30, 41, 59, 0.6); 
            backdrop-filter: blur(15px); 
            border: 1px solid rgba(255, 255, 255, 0.1); 
        }
        .bottom-nav { 
            background: rgba(11, 15, 26, 0.95); 
            backdrop-filter: blur(10px); 
            border-top: 1px solid rgba(255, 255, 255, 0.1); 
            position: fixed; 
            bottom: 0; 
            width: 100%; 
            z-index: 100; 
        }
        .btn-grad { 
            background: linear-gradient(135deg, #6366f1, #a855f7); 
            transition: 0.3s; 
            box-shadow: 0 10px 20px -10px #6366f1; 
        }
        .btn-grad:hover {
            transform: scale(1.02);
        }
        .nav-active { 
            color: #6366f1 !important; 
            transform: translateY(-5px); 
        }
        .hist-tab-active { 
            border-bottom: 2px solid #6366f1; 
            color: white; 
        }
        .hidden { 
            display: none !important; 
        }
        .no-scrollbar::-webkit-scrollbar { 
            display: none; 
        }
        
        /* Code Display Styles */
        .code-block-wrapper { 
            position: relative; 
        }
        pre { 
            background: #010409; 
            padding: 15px; 
            border-radius: 12px; 
            font-size: 11px; 
            border: 1px solid #30363d; 
            color: #79c0ff; 
            overflow-x: auto; 
            white-space: pre-wrap; 
            word-wrap: break-word; 
        }
        .copy-btn { 
            position: absolute; 
            top: 10px; 
            right: 10px; 
            background-color: #1e293b; 
            color: #94a3b8; 
            border: 1px solid #334155; 
            padding: 6px 12px; 
            border-radius: 8px; 
            font-size: 10px; 
            font-weight: bold; 
            cursor: pointer; 
            transition: 0.2s; 
            box-shadow: 0 4px 6px rgba(0,0,0,0.3); 
        }
        .copy-btn:hover { 
            background-color: #6366f1; 
            color: white; 
            border-color: #6366f1; 
        }

        /* Status Badges */
        .status-pending { color: #fbbf24; background: rgba(251, 191, 36, 0.1); padding: 4px 10px; border-radius: 12px; }
        .status-approved { color: #10b981; background: rgba(16, 185, 129, 0.1); padding: 4px 10px; border-radius: 12px; }
        .status-rejected { color: #ef4444; background: rgba(239, 68, 68, 0.1); padding: 4px 10px; border-radius: 12px; }
        
        /* Upload Inputs */
        .image-upload-box {
            aspect-ratio: 1/1; 
            border: 2px dashed #475569; 
            border-radius: 16px; 
            display: flex; 
            align-items: center;
            justify-content: center; 
            cursor: pointer; 
            transition: 0.3s; 
            background: #161e2d; 
            position: relative; 
            overflow: hidden;
        }
        .image-upload-box:hover { 
            border-color: #6366f1; 
            background: #1e293b; 
        }
        .image-upload-box img { 
            position: absolute; 
            width: 100%; 
            height: 100%; 
            object-fit: cover; 
        }
        
        .file-upload-label {
            display: block; 
            padding: 1rem; 
            background-color: #1e293b; 
            border: 1px solid #334155;
            border-radius: 1rem; 
            cursor: pointer; 
            transition: .3s;
        }
        .file-upload-label:hover { 
            border-color: #6366f1; 
        }
        .file-upload-label .file-name { 
            color: #94a3b8; 
            font-size: 12px; 
            white-space: nowrap; 
            overflow: hidden; 
            text-overflow: ellipsis; 
        }
    </style>
</head>
<body class="pb-24">

    <!-- 🔐 Authentication Section -->
    <div id="auth-section" class="flex items-center justify-center min-h-screen px-6">
        <div class="glass p-8 rounded-[2.5rem] w-full max-w-md border-indigo-500/20 shadow-2xl">
            <div class="text-center mb-8">
                <h2 id="auth-title" class="text-4xl font-black bg-gradient-to-r from-indigo-400 to-purple-400 bg-clip-text text-transparent italic tracking-tighter">CZONESTORE</h2>
                <p class="text-slate-400 text-sm mt-1 uppercase tracking-widest font-bold">Safe & Secure Access</p>
            </div>
            <div class="space-y-4">
                <input type="email" id="auth-email" placeholder="Email Address" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none focus:border-indigo-500">
                <input type="password" id="auth-pass" placeholder="Password" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none focus:border-indigo-500">
                
                <!-- Referral Input Field -->
                <div id="auth-referral-box" class="hidden">
                    <p class="text-[10px] text-indigo-400 ml-2 mb-1 font-bold tracking-wider uppercase">Referral Code</p>
                    <input type="text" id="auth-referral" placeholder="Optional" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none focus:border-indigo-500 transition">
                </div>

                <button onclick="handleAuth()" id="auth-btn" class="w-full btn-grad p-4 rounded-2xl font-bold">Login</button>
                <p class="text-center text-slate-400 text-xs cursor-pointer mt-4" onclick="toggleAuthMode()">New here? <span class="text-indigo-400 font-bold">Create Account</span></p>
            </div>
        </div>
    </div>

    <!-- 📱 Main App Container -->
    <div id="app-section" class="hidden">
        
        <!-- Header -->
        <header class="p-5 flex justify-between items-center sticky top-0 bg-[#0b0f1a]/95 backdrop-blur-md z-50">
            <h1 class="text-2xl font-black italic tracking-tighter text-indigo-400">CZONE<span class="text-white">STORE</span></h1>
            <div class="glass px-4 py-2 rounded-2xl border-indigo-500/20 text-right">
                <p class="text-[9px] text-slate-500 uppercase font-bold tracking-widest">Main Wallet</p>
                <p id="nav-balance" class="text-sm font-bold text-green-400">0.00 BDT</p>
            </div>
        </header>

        <!-- 🛒 View: Marketplace -->
        <div id="view-market" class="view px-4 space-y-6">
            <div class="relative mt-2">
                <i class="fas fa-search absolute left-4 top-4 text-slate-500"></i>
                <input type="text" placeholder="Search codes, apps, scripts..." class="w-full p-4 pl-12 bg-slate-900 rounded-2xl border border-slate-700 outline-none focus:border-indigo-500">
            </div>
            <div id="market-grid" class="grid grid-cols-1 gap-6 pb-20">
                <!-- Codes dynamically loaded here -->
            </div>
        </div>

        <!-- 📤 View: Sell Code & Dashboard -->
        <div id="view-sell" class="view hidden px-4 space-y-6 pb-20">
            <!-- Upload Section -->
            <div>
                <h2 class="text-2xl font-bold">Upload Your <span class="text-indigo-400">Work</span></h2>
                <div class="space-y-4 mt-4">
                    
                    <div class="space-y-2">
                        <label class="text-[10px] text-slate-400 font-bold uppercase ml-1 tracking-wider">Project Screenshots (up to 8)</label>
                        <div id="image-upload-grid" class="grid grid-cols-4 gap-3"></div>
                    </div>
                    
                    <input type="text" id="up-title" placeholder="Project Name" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none focus:border-indigo-500">
                    <textarea id="up-desc" placeholder="Write a detailed description..." class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 h-24 outline-none focus:border-indigo-500"></textarea>
                    <input type="url" id="up-live-url" placeholder="Live Preview URL (https://...)" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none focus:border-indigo-500">
                    <input type="url" id="up-video-url" placeholder="Video/Docs URL (Optional)" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none focus:border-indigo-500">
                    
                    <div class="grid grid-cols-2 gap-4">
                        <select id="up-cat" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none"></select>
                        <input type="number" id="up-price" oninput="calculateSellPricing()" placeholder="Your Price (BDT)" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none">
                    </div>
                    
                    <div class="glass p-4 rounded-2xl text-xs space-y-1 border-indigo-500/20">
                        <div class="flex justify-between">
                            <span>Admin Fee (<span id="sell-comm-pct">10</span>%):</span> 
                            <span id="sell-comm-val" class="text-red-400">0 BDT</span>
                        </div>
                        <div class="flex justify-between font-bold text-lg text-green-400 border-t border-white/5 pt-2">
                            <span>Customer Price:</span> 
                            <span id="sell-total">0 BDT</span>
                        </div>
                    </div>
                    
                    <!-- 3 Separate Code File Uploads -->
                    <div class="space-y-3 pt-2">
                        <div class="space-y-2">
                            <label class="text-[10px] text-slate-400 font-bold uppercase ml-1 tracking-wider">Main User Code (.txt, .html, .js)</label>
                            <label for="up-user-code-file" class="file-upload-label">
                                <div class="flex items-center gap-3">
                                    <i class="fas fa-file-code text-indigo-400 text-lg"></i>
                                    <span id="user-code-file-name" class="file-name">Select text code file...</span>
                                </div>
                            </label>
                            <input type="file" id="up-user-code-file" class="hidden" accept=".txt,.html,.js,.php,.css" onchange="handleCodeUpload(event, 'userCode', 'user-code-file-name')">
                        </div>
                        
                        <div class="space-y-2">
                            <label class="text-[10px] text-slate-400 font-bold uppercase ml-1 tracking-wider">Admin Panel Code (.txt, .php)</label>
                            <label for="up-admin-code-file" class="file-upload-label">
                                <div class="flex items-center gap-3">
                                    <i class="fas fa-shield-halved text-cyan-400 text-lg"></i>
                                    <span id="admin-code-file-name" class="file-name">Select text code file (Optional)</span>
                                </div>
                            </label>
                            <input type="file" id="up-admin-code-file" class="hidden" accept=".txt,.html,.js,.php,.css" onchange="handleCodeUpload(event, 'adminCode', 'admin-code-file-name')">
                        </div>
                        
                         <div class="space-y-2">
                            <label class="text-[10px] text-slate-400 font-bold uppercase ml-1 tracking-wider">Extra Files / Docs (.txt, .sql)</label>
                            <label for="up-extra-code-file" class="file-upload-label">
                                <div class="flex items-center gap-3">
                                    <i class="fas fa-folder-plus text-purple-400 text-lg"></i>
                                    <span id="extra-code-file-name" class="file-name">Select text file (Optional)</span>
                                </div>
                            </label>
                            <input type="file" id="up-extra-code-file" class="hidden" accept=".txt,.sql,.json" onchange="handleCodeUpload(event, 'extraCode', 'extra-code-file-name')">
                        </div>
                    </div>

                    <button onclick="publishProduct()" class="w-full btn-grad p-5 rounded-2xl font-bold shadow-xl">List Item for Sale</button>
                </div>
            </div>
            
            <!-- Seller Dashboard Section -->
            <div id="my-sales-dashboard" class="mt-10 pt-6 border-t border-slate-800">
                <h3 class="text-xl font-bold mb-4">My Sales <span class="text-green-400">Dashboard</span></h3>
                
                <div class="grid grid-cols-2 gap-4 mb-6">
                    <div class="glass p-4 rounded-2xl text-center">
                        <p class="text-[10px] uppercase font-bold text-slate-400">Total Revenue</p>
                        <p id="total-revenue" class="text-2xl font-black text-green-400">0 BDT</p>
                    </div>
                    <div class="glass p-4 rounded-2xl text-center">
                        <p class="text-[10px] uppercase font-bold text-slate-400">Total Sales</p>
                        <p id="total-sales-count" class="text-2xl font-black text-white">0</p>
                    </div>
                </div>
                
                <h4 class="text-sm font-bold text-slate-300 mb-3">My Uploaded Posts & Stats</h4>
                <div id="my-products-list" class="space-y-4">
                    <!-- Products will be populated here by JavaScript -->
                </div>
            </div>
        </div>

        <!-- 💼 View: Wallet & History -->
        <div id="view-wallet" class="view hidden px-4 space-y-6 pb-24">
            
            <div class="glass p-8 rounded-[2.5rem] text-center bg-gradient-to-br from-indigo-950/20 to-slate-900 shadow-2xl">
                <p class="text-slate-400 text-[10px] font-bold uppercase tracking-[0.2em] mb-2">Total Balance</p>
                <h2 id="wallet-balance" class="text-5xl font-black text-white">0.00 BDT</h2>
            </div>

            <!-- Profile Information Box -->
            <div class="glass p-5 rounded-3xl border border-indigo-500/20 relative shadow-xl">
                <div class="flex justify-between items-center mb-4 border-b border-white/5 pb-3">
                    <span class="text-[11px] text-slate-400 font-bold uppercase tracking-widest"><i class="fas fa-user-circle mr-1"></i> My Profile</span>
                    <span class="text-xs font-black text-indigo-400 bg-indigo-500/10 px-3 py-1.5 rounded-xl border border-indigo-500/20">ID: <span id="profile-id">---</span></span>
                </div>
                <div class="space-y-4">
                    <div>
                        <p class="text-[9px] text-slate-500 uppercase font-bold mb-1">Email Address</p>
                        <p id="profile-email" class="text-sm font-bold text-white tracking-wide">---</p>
                    </div>
                    <div class="relative">
                        <p class="text-[9px] text-slate-500 uppercase font-bold mb-1">Password</p>
                        <div class="flex items-center justify-between bg-slate-900/50 p-3 rounded-xl border border-white/5">
                            <p id="profile-password" class="text-sm font-bold text-slate-300 tracking-[0.3em]">********</p>
                            <button onclick="togglePasswordView()" class="text-slate-400 hover:text-indigo-400 transition ml-4 bg-slate-800 p-2 rounded-lg">
                                <i id="eye-icon" class="fas fa-eye"></i>
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Referral System Section -->
            <div id="referral-section" class="glass p-6 rounded-[2.5rem] hidden shadow-xl border border-indigo-500/20">
                <h3 class="font-bold text-lg text-indigo-400 mb-1"><i class="fas fa-link mr-2"></i>Refer & Earn</h3>
                <p class="text-[10px] text-slate-400 mb-4">Share your link to invite friends and earn <span id="referral-reward-amt" class="text-green-400 font-bold">0</span> BDT per successful signup!</p>
                
                <div class="flex gap-2 mb-4">
                    <div class="flex-1 p-3 bg-slate-900 rounded-2xl border border-slate-700 flex flex-col justify-center overflow-hidden">
                        <span class="text-[9px] uppercase font-bold text-slate-500 mb-1">Your Referral Link:</span>
                        <span id="my-referral-link" class="font-bold text-indigo-300 text-xs truncate">---</span>
                    </div>
                    <button onclick="copyReferralLink()" class="bg-indigo-500 hover:bg-indigo-600 text-white px-5 rounded-2xl transition shadow-lg flex items-center justify-center">
                        <i class="far fa-copy text-lg"></i>
                    </button>
                </div>
                
                <div class="grid grid-cols-2 gap-4">
                    <div class="p-3 bg-slate-900/50 rounded-2xl text-center border border-white/5">
                        <p class="text-[9px] uppercase font-bold text-slate-400">Total Referred</p>
                        <p id="total-referred-count" class="text-xl font-black text-white">0</p>
                    </div>
                    <div class="p-3 bg-slate-900/50 rounded-2xl text-center border border-white/5">
                        <p class="text-[9px] uppercase font-bold text-slate-400">Total Earned</p>
                        <p id="total-referred-earned" class="text-xl font-black text-green-400">0 BDT</p>
                    </div>
                </div>
            </div>
            
            <!-- Deposit / Withdraw Buttons -->
            <div class="grid grid-cols-2 gap-4">
                <button onclick="openFinance('deposit')" class="p-4 glass rounded-2xl font-bold text-green-400 border-green-500/20">
                    <i class="fas fa-plus-circle mr-2"></i>Deposit
                </button>
                <button onclick="openFinance('withdrawal')" class="p-4 glass rounded-2xl font-bold text-orange-400 border-orange-500/20">
                    <i class="fas fa-minus-circle mr-2"></i>Withdraw
                </button>
            </div>
            
            <!-- Finance Form -->
            <div id="fin-form" class="hidden glass p-6 rounded-[2.5rem] space-y-4">
                <h3 id="fin-title" class="font-bold text-lg">Transaction</h3>
                <div class="space-y-3">
                    <select id="fin-method" onchange="updatePaymentInfo()" class="w-full p-4 bg-slate-900 rounded-2xl outline-none border border-slate-700">
                        <option value="bKash">bKash</option>
                        <option value="Nagad">Nagad</option>
                    </select>
                    
                    <div id="payment-details-box" class="hidden glass p-4 rounded-2xl text-center space-y-2 border-green-500/20 bg-green-500/5">
                        <p class="text-[10px] text-slate-400">Send money to this <span id="payment-method-name" class="font-bold"></span> number:</p>
                        <p id="payment-number" class="text-2xl font-black bg-gradient-to-r from-green-300 to-emerald-400 bg-clip-text text-transparent cursor-pointer" onclick="copyToClipboard(this.innerText, 'Number')">... (Click to Copy)</p>
                        <p class="text-[10px] text-slate-500">Then enter the Transaction ID below.</p>
                    </div>
                    
                    <input type="number" id="fin-amt" oninput="calculateWithdrawFee()" placeholder="Amount (BDT)" class="w-full p-4 bg-slate-900 rounded-2xl outline-none border border-slate-700">
                    
                    <div id="with-fee-box" class="hidden glass p-4 rounded-2xl text-[11px] space-y-1 border-orange-500/20">
                        <div class="flex justify-between">
                            <span>Withdraw Fee (<span id="with-comm-pct">5</span>%):</span> 
                            <span id="with-comm-val" class="text-red-400">0 BDT</span>
                        </div>
                        <div class="flex justify-between font-bold text-white text-sm border-t border-white/5 pt-2">
                            <span>Total Receive:</span> 
                            <span id="with-receive" class="text-green-400">0 BDT</span>
                        </div>
                    </div>
                    
                    <input type="text" id="fin-info" placeholder="Transaction ID / Account No" class="w-full p-4 bg-slate-900 rounded-2xl outline-none border border-slate-700">
                    <button onclick="submitFinanceRequest()" class="w-full btn-grad p-4 rounded-2xl font-bold">Confirm Request</button>
                </div>
            </div>

            <!-- Real-Time Transaction History -->
            <div class="space-y-4 mt-6">
                <h3 class="font-bold text-slate-300 ml-1">My Activity History</h3>
                <div class="flex gap-4 border-b border-white/5 text-[10px] font-black uppercase tracking-widest no-scrollbar overflow-x-auto">
                    <button onclick="switchHistoryTab('deposits')" id="hist-tab-deposits" class="pb-2 hist-tab-active">Deposits</button>
                    <button onclick="switchHistoryTab('withdrawals')" id="hist-tab-withdrawals" class="pb-2 text-slate-500">Withdrawals</button>
                    <button onclick="switchHistoryTab('purchases')" id="hist-tab-purchases" class="pb-2 text-slate-500">Purchases</button>
                </div>
                <div id="hist-content-list" class="space-y-3">
                    <!-- History will load here dynamically -->
                </div>
            </div>

            <!-- Logout Button -->
            <div class="mt-8 pt-6 border-t border-white/10">
                <button onclick="logout()" class="w-full p-4 bg-red-500/10 text-red-500 border border-red-500/20 rounded-2xl font-bold hover:bg-red-500 hover:text-white transition flex justify-center items-center gap-2 shadow-lg">
                    <i class="fas fa-sign-out-alt"></i> Secure Logout
                </button>
            </div>
        </div>

        <!-- 🧭 Navigation Bar -->
        <nav class="bottom-nav p-4 flex justify-around items-center">
            <button onclick="showTab('market')" id="nav-market" class="nav-active flex flex-col items-center w-1/3">
                <i class="fas fa-store text-xl"></i><span class="text-[10px] font-bold mt-1">Market</span>
            </button>
            <button onclick="showTab('sell')" id="nav-sell" class="text-slate-500 flex flex-col items-center w-1/3 border-l border-r border-white/5">
                <i class="fas fa-upload text-xl"></i><span class="text-[10px] font-bold mt-1">Sell</span>
            </button>
            <button onclick="showTab('wallet')" id="nav-wallet" class="text-slate-500 flex flex-col items-center w-1/3">
                <i class="fas fa-wallet text-xl"></i><span class="text-[10px] font-bold mt-1">Wallet</span>
            </button>
        </nav>
    </div>

    <!-- 📦 Details Modal -->
    <div id="item-modal" class="hidden fixed inset-0 bg-black/95 z-[200] overflow-y-auto no-scrollbar">
        <div id="modal-body" class="p-6 pb-32 max-w-2xl mx-auto space-y-6">
            <!-- Modal Content Populated via JS -->
        </div>
        <div class="fixed bottom-0 left-0 right-0 p-4 glass backdrop-blur-2xl flex gap-3 z-[210]">
            <button onclick="closeModal('item-modal')" class="flex-1 p-4 bg-slate-800 rounded-2xl font-bold">Back</button>
            <div id="buy-action-area" class="flex-[2]">
                <!-- Buy Button Populated via JS -->
            </div>
        </div>
    </div>

    <!-- ✏️ Edit Product Modal -->
    <div id="edit-modal" class="hidden fixed inset-0 bg-black/95 z-[200] overflow-y-auto flex items-center justify-center p-4">
        <div class="glass w-full max-w-md p-6 rounded-3xl space-y-4">
            <h3 class="text-xl font-bold text-indigo-400">Edit Post Details</h3>
            <input type="hidden" id="edit-id">
            <div class="space-y-3">
                <input type="text" id="edit-title" placeholder="Project Name" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none">
                <textarea id="edit-desc" placeholder="Description..." class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 h-24 outline-none"></textarea>
                <input type="number" id="edit-price" placeholder="Your Price (BDT)" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none">
                <input type="url" id="edit-live-url" placeholder="Live Preview URL" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none">
                <input type="url" id="edit-video-url" placeholder="Video URL" class="w-full p-4 bg-slate-900 rounded-2xl border border-slate-700 outline-none">
            </div>
            <div class="flex gap-3 pt-4 border-t border-white/10">
                <button onclick="closeModal('edit-modal')" class="flex-1 p-3 bg-slate-800 rounded-xl font-bold">Cancel</button>
                <button onclick="saveEditProduct()" class="flex-1 btn-grad p-3 rounded-xl font-bold">Save Changes</button>
            </div>
        </div>
    </div>

    <!-- Firebase Script Logic -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { 
            getAuth, 
            signInWithEmailAndPassword, 
            createUserWithEmailAndPassword, 
            onAuthStateChanged, 
            signOut 
        } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { 
            getFirestore, 
            doc, 
            setDoc, 
            getDoc, 
            getDocs, 
            collection, 
            addDoc, 
            query, 
            where, 
            onSnapshot, 
            serverTimestamp, 
            updateDoc, 
            increment, 
            deleteDoc, 
            runTransaction 
        } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // 🔥 Configuration
        const firebaseConfig = {
            apiKey: "AIzaSyAA50JdtkZzivc9ObuBqL-DxCbrr6bwtjU",
            authDomain: "czone-1234e.firebaseapp.com",
            projectId: "czone-1234e",
            storageBucket: "czone-1234e.firebasestorage.app",
            messagingSenderId: "115067621737",
            appId: "1:115067621737:web:773f9cf9bbd267cffa2df8"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // Global Variables
        let currentUser = null;
        let currentUserBalance = 0; 
        window.userDbPassword = ""; 
        
        let adminSettings = { 
            sellComm: 10, 
            withComm: 5, 
            categories: ["Web", "Mobile", "Script"], 
            paymentMethods: { bKash: "Not Set", Nagad: "Not Set" },
            referralEnabled: true, 
            referralReward: 20     
        };
        
        let uploadedImagesData = Array(8).fill(null);
        let uploadedCodeFiles = { 
            userCode: null, 
            adminCode: null, 
            extraCode: null 
        };
        let financeRequestType = 'deposit';

        // --- URL Parameter Check for Referral ---
        window.onload = () => {
            const urlParams = new URLSearchParams(window.location.search);
            const refCode = urlParams.get('ref');
            
            if(refCode) {
                document.getElementById('auth-title').innerText = "Join Us";
                document.getElementById('auth-btn').innerText = "Create Account";
                
                const refBox = document.getElementById('auth-referral-box');
                const refInput = document.getElementById('auth-referral');
                
                refBox.classList.remove('hidden');
                refInput.value = refCode;
                refInput.setAttribute('readonly', true);
                refInput.classList.add('bg-indigo-900/30', 'text-indigo-300', 'cursor-not-allowed', 'border-indigo-500');
            }
        };

        // --- Navigation ---
        window.showTab = (tab) => {
            document.querySelectorAll('.view').forEach(v => v.classList.add('hidden'));
            document.querySelectorAll('nav button').forEach(b => b.classList.replace('nav-active', 'text-slate-500'));
            
            document.getElementById('view-' + tab).classList.remove('hidden');
            document.getElementById('nav-' + tab).classList.add('nav-active');
            
            if (tab === 'sell' && currentUser) {
                loadMyProductsAndStats();
            }
        };
        
        // --- Authentication Check ---
        onAuthStateChanged(auth, (user) => {
            if (user) {
                currentUser = user;
                document.getElementById('auth-section').classList.add('hidden');
                document.getElementById('app-section').classList.remove('hidden');
                runApp();
            } else {
                currentUser = null;
                document.getElementById('auth-section').classList.remove('hidden');
                document.getElementById('app-section').classList.add('hidden');
            }
        });

        window.toggleAuthMode = () => { 
            const isLogin = document.getElementById('auth-title').innerText === "CodeMarket"; 
            document.getElementById('auth-title').innerText = isLogin ? "Join Us" : "CodeMarket"; 
            document.getElementById('auth-btn').innerText = isLogin ? "Create Account" : "Login"; 
            
            if(isLogin) {
                document.getElementById('auth-referral-box').classList.remove('hidden');
            } else {
                document.getElementById('auth-referral-box').classList.add('hidden');
            }
        };
        
        // --- Handle Registration & Login ---
        window.handleAuth = async () => { 
            const email = document.getElementById('auth-email').value; 
            const pass = document.getElementById('auth-pass').value; 
            const referCodeInput = document.getElementById('auth-referral').value.trim();
            
            if(!email || !pass) return alert("Please enter email and password");
            
            try { 
                if (document.getElementById('auth-title').innerText === "CodeMarket") { 
                    // Login Mode
                    await signInWithEmailAndPassword(auth, email, pass); 
                } else { 
                    // Registration Mode
                    const res = await createUserWithEmailAndPassword(auth, email, pass); 
                    
                    let newUserIdStr = "1001";
                    try {
                        const counterRef = doc(db, "settings", "counters");
                        await runTransaction(db, async (transaction) => {
                            const sfDoc = await transaction.get(counterRef);
                            if (!sfDoc.exists()) {
                                transaction.set(counterRef, { lastId: 1001 });
                            } else {
                                let newId = sfDoc.data().lastId + 1;
                                transaction.update(counterRef, { lastId: newId });
                                newUserIdStr = newId.toString();
                            }
                        });
                    } catch(e) { 
                        console.error("Counter error:", e);
                        newUserIdStr = Math.floor(1000 + Math.random() * 9000).toString(); 
                    }

                    // Save user to Firestore including password for profile view
                    await setDoc(doc(db, "users", res.user.uid), { 
                        email: email, 
                        password: pass, 
                        balance: 0, 
                        uid: res.user.uid, 
                        referId: newUserIdStr, 
                        totalReferred: 0, 
                        referralEarnings: 0, 
                        createdAt: serverTimestamp() 
                    }); 

                    // Check and reward referrer
                    if (referCodeInput && adminSettings.referralEnabled) {
                        const q = query(collection(db, "users"), where("referId", "==", referCodeInput));
                        const querySnapshot = await getDocs(q);
                        
                        if (!querySnapshot.empty) {
                            const referrerDoc = querySnapshot.docs[0];
                            
                            await updateDoc(doc(db, "users", referrerDoc.id), {
                                balance: increment(adminSettings.referralReward),
                                totalReferred: increment(1),
                                referralEarnings: increment(adminSettings.referralReward)
                            });
                            
                            await addDoc(collection(db, "deposits"), {
                                userId: referrerDoc.id, 
                                email: referrerDoc.data().email,
                                amount: adminSettings.referralReward, 
                                method: "Referral Bonus",
                                info: "Referred New User ID: " + newUserIdStr, 
                                status: 'approved', 
                                timestamp: serverTimestamp()
                            });
                        }
                    }
                    
                    // Clear Referral Params from URL
                    window.history.replaceState({}, document.title, window.location.pathname);
                } 
            } catch (e) { 
                alert(e.message); 
            } 
        };
        
        window.logout = () => signOut(auth);

        // --- Profile UI Logic ---
        window.togglePasswordView = () => {
            const passEl = document.getElementById('profile-password');
            const icon = document.getElementById('eye-icon');
            
            if(passEl.innerText === "********") {
                passEl.innerText = window.userDbPassword || "No Password Found";
                passEl.classList.remove('tracking-[0.3em]');
                icon.classList.replace('fa-eye', 'fa-eye-slash');
            } else {
                passEl.innerText = "********";
                passEl.classList.add('tracking-[0.3em]');
                icon.classList.replace('fa-eye-slash', 'fa-eye');
            }
        };

        // --- Core Initialization ---
        function runApp() {
            // Watch Settings
            onSnapshot(doc(db, "settings", "global"), (d) => { 
                if (d.exists()) { 
                    adminSettings = { ...adminSettings, ...d.data() }; 
                    document.getElementById('sell-comm-pct').innerText = adminSettings.sellComm; 
                    document.getElementById('with-comm-pct').innerText = adminSettings.withComm; 
                    document.getElementById('up-cat').innerHTML = adminSettings.categories.map(c => `<option value="${c}">${c}</option>`).join(''); 
                    
                    if(adminSettings.referralEnabled) {
                        document.getElementById('referral-section').classList.remove('hidden');
                        document.getElementById('referral-reward-amt').innerText = adminSettings.referralReward || 0;
                    } else {
                        document.getElementById('referral-section').classList.add('hidden');
                    }
                } 
            });
            
            // Watch User Data
            onSnapshot(doc(db, "users", currentUser.uid), (d) => { 
                if (d.exists()) {
                    const data = d.data();
                    currentUserBalance = data.balance || 0; 
                    
                    document.getElementById('nav-balance').innerText = currentUserBalance.toFixed(2) + " BDT"; 
                    document.getElementById('wallet-balance').innerText = currentUserBalance.toFixed(2) + " BDT"; 
                    
                    document.getElementById('profile-id').innerText = data.referId || "N/A";
                    document.getElementById('profile-email').innerText = data.email || "N/A";
                    window.userDbPassword = data.password || "N/A"; 
                    
                    const refLink = window.location.origin + window.location.pathname + "?ref=" + (data.referId || "N/A");
                    document.getElementById('my-referral-link').innerText = refLink;
                    document.getElementById('total-referred-count').innerText = data.totalReferred || 0;
                    document.getElementById('total-referred-earned').innerText = (data.referralEarnings || 0) + " BDT";
                }
            });
            
            // Watch Marketplace Codes
            onSnapshot(query(collection(db, "codes")), (snap) => {
                const grid = document.getElementById('market-grid');
                if (snap.empty) { 
                    grid.innerHTML = `<p class="text-center text-slate-500 col-span-full py-20">Market is currently empty.</p>`; 
                    return; 
                }
                
                let html = '';
                snap.forEach(doc => {
                    const d = doc.data(); 
                    if (!d.images || d.images.length === 0) return; 
                    
                    const total = d.basePrice + (d.basePrice * adminSettings.sellComm / 100); 
                    
                    html += `
                        <div onclick="viewItem('${doc.id}')" class="glass rounded-[2rem] overflow-hidden border border-white/5 group transition-all active:scale-95 cursor-pointer">
                            <img src="${d.images[0]}" class="w-full h-48 object-cover group-hover:scale-105 transition duration-500">
                            <div class="p-5 flex justify-between items-center">
                                <div>
                                    <h3 class="font-bold truncate w-44 text-slate-100">${d.title}</h3>
                                    <p class="text-green-400 font-black text-lg">${total.toFixed(0)} <span class="text-[10px]">BDT</span></p>
                                </div>
                                <div class="bg-indigo-600/20 text-indigo-400 p-3 rounded-2xl"><i class="fas fa-chevron-right"></i></div>
                            </div>
                        </div>`; 
                });
                grid.innerHTML = html;
            });
            
            initializeSellForm();
            switchHistoryTab('deposits');
        }

        // --- Sell Form Image & Code Handlers ---
        function initializeSellForm() { 
            const grid = document.getElementById('image-upload-grid'); 
            let html = '';
            for(let i=0; i<8; i++) {
                html += `
                    <label for="file-input-${i}" class="image-upload-box">
                        <i class="fas fa-plus text-2xl text-slate-600"></i>
                        <img id="preview-${i}" src="" class="hidden">
                    </label>
                    <input type="file" id="file-input-${i}" class="hidden" accept="image/*" onchange="handleImageUpload(event, ${i})">
                `;
            }
            grid.innerHTML = html;
        }

        window.handleImageUpload = (event, index) => { 
            const file = event.target.files[0]; 
            if (!file) return; 
            const reader = new FileReader(); 
            reader.onload = (e) => { 
                document.getElementById(`preview-${index}`).src = e.target.result; 
                document.getElementById(`preview-${index}`).classList.remove('hidden'); 
                uploadedImagesData[index] = e.target.result; 
            }; 
            reader.readAsDataURL(file); 
        };

        window.calculateSellPricing = () => { 
            const base = parseFloat(document.getElementById('up-price').value) || 0; 
            const comm = (base * adminSettings.sellComm) / 100; 
            document.getElementById('sell-comm-val').innerText = comm.toFixed(0) + " BDT"; 
            document.getElementById('sell-total').innerText = (base + comm).toFixed(0) + " BDT"; 
        };
        
        window.handleCodeUpload = (event, codeType, nameElementId) => {
            const file = event.target.files[0];
            const nameEl = document.getElementById(nameElementId);
            if (!file) { 
                uploadedCodeFiles[codeType] = null; 
                nameEl.textContent = 'Upload file...'; 
                return; 
            }
            nameEl.textContent = file.name;
            const reader = new FileReader();
            reader.onload = (e) => { uploadedCodeFiles[codeType] = e.target.result; };
            reader.readAsText(file); 
        };
        
        // --- Publish Product to Market ---
        window.publishProduct = async () => {
            const finalImages = uploadedImagesData.filter(img => img !== null);
            if (finalImages.length === 0) return alert("Please upload at least one screenshot!");
            
            const payload = {
                title: document.getElementById('up-title').value, 
                desc: document.getElementById('up-desc').value,
                basePrice: parseFloat(document.getElementById('up-price').value), 
                liveUrl: document.getElementById('up-live-url').value,
                videoUrl: document.getElementById('up-video-url').value, 
                category: document.getElementById('up-cat').value, 
                images: finalImages,
                userCode: uploadedCodeFiles.userCode, 
                adminCode: uploadedCodeFiles.adminCode, 
                extraCode: uploadedCodeFiles.extraCode,
                sellerId: currentUser.uid, 
                sellerEmail: currentUser.email, 
                createdAt: serverTimestamp(), 
                salesCount: 0, 
                totalEarned: 0 
            };
            
            if(!payload.title || !payload.basePrice || !payload.userCode || !payload.desc) {
                return alert("Please fill required fields and upload the Main Code File!");
            }
            
            await addDoc(collection(db, "codes"), payload);
            alert("Success! Your project is now live."); 
            
            document.getElementById('up-title').value = '';
            document.getElementById('up-price').value = '';
            document.getElementById('up-desc').value = '';
            
            showTab('market');
        };

        // --- Seller Dashboard Logic ---
        function loadMyProductsAndStats() {
            const q = query(collection(db, "codes"), where("sellerId", "==", currentUser.uid));
            onSnapshot(q, (snap) => {
                let totalRev = 0;
                let totalSales = 0;
                const listEl = document.getElementById('my-products-list');
                
                if (snap.empty) { 
                    listEl.innerHTML = `<p class="text-center text-[10px] text-slate-600 py-10 uppercase tracking-widest">You haven't posted anything yet</p>`; 
                } else {
                    let html = '';
                    snap.forEach(doc => { 
                        const d = doc.data(); 
                        totalRev += (d.totalEarned || 0); 
                        totalSales += (d.salesCount || 0);
                        
                        const escTitle = d.title.replace(/'/g, "\\'");
                        const escDesc = d.desc.replace(/'/g, "\\'").replace(/\n/g, "\\n");
                        
                        html += `
                            <div class="glass p-4 rounded-2xl border-l-4 border-indigo-500 flex justify-between items-center relative overflow-hidden">
                                <div class="w-full">
                                    <h4 class="font-bold text-slate-100 text-lg truncate">${d.title}</h4>
                                    <div class="flex gap-4 mt-2">
                                        <p class="text-[10px] text-slate-400 font-bold uppercase"><i class="fas fa-users text-indigo-400"></i> ${d.salesCount || 0} Buyers</p>
                                        <p class="text-[10px] text-slate-400 font-bold uppercase"><i class="fas fa-coins text-green-400"></i> Earned: ${d.totalEarned || 0} BDT</p>
                                    </div>
                                    <div class="flex gap-2 mt-4 pt-3 border-t border-white/5">
                                        <button onclick="openEditModal('${doc.id}', '${escTitle}', '${escDesc}', ${d.basePrice}, '${d.liveUrl||''}', '${d.videoUrl||''}')" class="bg-indigo-500/20 text-indigo-400 px-4 py-2 rounded-xl text-xs font-bold hover:bg-indigo-500 hover:text-white transition"><i class="fas fa-edit"></i> Edit</button>
                                        <button onclick="deleteProduct('${doc.id}')" class="bg-red-500/20 text-red-400 px-4 py-2 rounded-xl text-xs font-bold hover:bg-red-500 hover:text-white transition"><i class="fas fa-trash"></i> Delete</button>
                                    </div>
                                </div>
                            </div>
                        `;
                    });
                    listEl.innerHTML = html;
                }
                document.getElementById('total-revenue').innerText = `${totalRev.toFixed(0)} BDT`;
                document.getElementById('total-sales-count').innerText = totalSales;
            });
        }

        window.deleteProduct = async (id) => { 
            if (confirm("Are you sure you want to completely DELETE this post?")) { 
                await deleteDoc(doc(db, "codes", id)); 
                alert("Product deleted!"); 
            } 
        };

        window.openEditModal = (id, title, desc, price, liveUrl, videoUrl) => {
            document.getElementById('edit-id').value = id; 
            document.getElementById('edit-title').value = title; 
            document.getElementById('edit-desc').value = desc.replace(/\\n/g, '\n');
            document.getElementById('edit-price').value = price; 
            document.getElementById('edit-live-url').value = liveUrl; 
            document.getElementById('edit-video-url').value = videoUrl;
            document.getElementById('edit-modal').classList.remove('hidden');
        };

        window.saveEditProduct = async () => {
            const id = document.getElementById('edit-id').value;
            const updates = { 
                title: document.getElementById('edit-title').value, 
                desc: document.getElementById('edit-desc').value, 
                basePrice: parseFloat(document.getElementById('edit-price').value), 
                liveUrl: document.getElementById('edit-live-url').value, 
                videoUrl: document.getElementById('edit-video-url').value 
            };
            if(!updates.title || !updates.basePrice) return alert("Required fields missing.");
            
            await updateDoc(doc(db, "codes", id), updates); 
            alert("Post updated successfully!"); 
            closeModal('edit-modal');
        };

        // --- Item Viewing & Purchase Area ---
        window.copyCode = (button, codeId) => {
            const text = document.getElementById(codeId).innerText;
            navigator.clipboard.writeText(text).then(() => {
                button.innerHTML = '<i class="fas fa-check"></i> Copied!'; 
                button.style.backgroundColor = '#10b981'; 
                button.style.color = 'white';
                setTimeout(() => { 
                    button.innerHTML = '<i class="far fa-copy"></i> Copy Code'; 
                    button.style.backgroundColor = ''; 
                    button.style.color = ''; 
                }, 2000);
            });
        };
        
        window.copyReferralLink = () => {
            const text = document.getElementById('my-referral-link').innerText;
            navigator.clipboard.writeText(text).then(() => { 
                alert("Referral Link Copied!"); 
            });
        };

        window.viewItem = async (id) => {
            try {
                const d = (await getDoc(doc(db, "codes", id))).data();
                const total = d.basePrice + (d.basePrice * adminSettings.sellComm / 100);
                
                let isPurchased = false;
                if (d.sellerId === currentUser.uid) {
                    isPurchased = true;
                } else {
                    const qCheck = query(collection(db, "purchases"), where("buyerId", "==", currentUser.uid), where("codeId", "==", id));
                    isPurchased = !(await getDocs(qCheck)).empty;
                }

                let codeAccessHTML = '';
                if (isPurchased) {
                    const createCodeBlock = (code, label, blockId, colorClass = "text-indigo-400") => {
                        if (!code) return '';
                        let displayCode = code;
                        if (code.startsWith('data:')) { 
                            try { displayCode = decodeURIComponent(escape(atob(code.split(',')[1]))); } 
                            catch(e) { displayCode = code; } 
                        }
                        displayCode = displayCode.replace(/</g, "&lt;").replace(/>/g, "&gt;");
                        
                        return `
                            <div class="space-y-2">
                                <p class="text-sm font-bold flex items-center gap-2"><i class="fas fa-code ${colorClass}"></i> ${label}</p>
                                <div class="code-block-wrapper">
                                    <pre><code id="${blockId}">${displayCode}</code></pre>
                                    <button onclick="copyCode(this, '${blockId}')" class="copy-btn"><i class="far fa-copy"></i> Copy Code</button>
                                </div>
                            </div>
                        `;
                    };
                    
                    codeAccessHTML = `
                        <div class="space-y-6">
                            ${createCodeBlock(d.userCode, 'Main User Code', 'user-code-block')} 
                            ${createCodeBlock(d.adminCode, 'Admin Panel Code', 'admin-code-block', 'text-cyan-400')} 
                            ${createCodeBlock(d.extraCode, 'Extra Files / Docs', 'extra-code-block', 'text-purple-400')}
                        </div>
                    `;
                } else {
                    codeAccessHTML = `
                        <div class="p-12 glass rounded-[2.5rem] text-center border-dashed border-indigo-500/30">
                            <i class="fas fa-lock text-4xl text-indigo-500 mb-3"></i>
                            <p class="text-xs font-bold text-slate-400">Complete purchase to view and copy the original code.</p>
                        </div>
                    `;
                }

                let linksHtml = '';
                if(d.liveUrl) linksHtml += `<a href="${d.liveUrl}" target="_blank" class="flex-1 text-center p-3 glass rounded-xl font-bold text-cyan-400 border-cyan-500/20"><i class="fas fa-globe mr-2"></i>Live Demo</a>`;
                if(d.videoUrl) linksHtml += `<a href="${d.videoUrl}" target="_blank" class="flex-1 text-center p-3 glass rounded-xl font-bold text-red-400 border-red-500/20"><i class="fab fa-youtube mr-2"></i>Video</a>`;

                let imagesHtml = d.images.map(img => `<img src="${img}" class="w-full h-72 object-cover rounded-3xl flex-shrink-0 border border-white/5 shadow-2xl snap-center">`).join('');

                document.getElementById('modal-body').innerHTML = `
                    <div class="flex overflow-x-auto gap-3 no-scrollbar snap-x snap-mandatory">${imagesHtml}</div>
                    <div class="space-y-1">
                        <h2 class="text-3xl font-black text-slate-100">${d.title}</h2>
                        <div class="flex gap-2 flex-wrap">
                            <span class="bg-indigo-600/20 text-indigo-400 px-3 py-1 rounded-full text-[10px] font-bold uppercase tracking-widest">${d.category}</span>
                            <span class="bg-slate-800 text-slate-400 px-3 py-1 rounded-full text-[10px] font-bold uppercase tracking-widest">By ${d.sellerEmail.split('@')[0]}</span>
                        </div>
                    </div>
                    <p class="text-slate-400 text-sm leading-relaxed glass p-4 rounded-2xl">${d.desc.replace(/\n/g, '<br>')}</p>
                    <div class="flex gap-2">${linksHtml}</div>
                    <div class="p-5 glass rounded-3xl flex justify-between items-center border-indigo-500/20">
                        <span class="text-xs text-slate-500 font-bold uppercase">Market Price</span>
                        <span class="text-2xl font-black text-green-400">${total.toFixed(0)} BDT</span>
                    </div>
                    <div class="space-y-3">
                        <h3 class="font-bold flex items-center gap-2"><i class="fas fa-cogs text-indigo-400"></i> Code Access Area</h3>
                        ${codeAccessHTML}
                    </div>
                `;
                
                document.getElementById('buy-action-area').innerHTML = isPurchased 
                    ? `<button class="w-full p-4 bg-green-600 rounded-2xl font-bold">Successfully Owned ✓</button>` 
                    : `<button onclick="confirmPurchase('${id}', ${total}, ${d.basePrice}, '${d.sellerId}')" class="w-full btn-grad p-4 rounded-2xl font-bold shadow-xl shadow-indigo-500/20">Pay & Unlock Access</button>`;
                
                document.getElementById('item-modal').classList.remove('hidden');
            } catch(error) { 
                alert("Something went wrong. Please try again."); 
                console.error(error);
            }
        };

        window.confirmPurchase = async (id, total, base, sId) => {
            if (sId === currentUser.uid) return alert("You cannot buy your own item.");
            if (currentUserBalance < total) return alert("❌ Insufficient Balance! Deposit funds first.");
            
            if(confirm(`Confirm purchase for ${total} BDT? This action is final.`)) {
                // Deduct balance
                await updateDoc(doc(db, "users", currentUser.uid), { balance: currentUserBalance - total }); 
                
                // Credit seller
                const sSnap = await getDoc(doc(db, "users", sId));
                await updateDoc(doc(db, "users", sId), { balance: (sSnap.data().balance || 0) + base }); 
                
                // Update Code stats
                const codeData = (await getDoc(doc(db, "codes", id))).data();
                await updateDoc(doc(db, "codes", id), { salesCount: increment(1), totalEarned: increment(base) });
                
                // Record history
                await addDoc(collection(db, "purchases"), { 
                    buyerId: currentUser.uid, 
                    sellerId: sId, 
                    codeId: id, 
                    codeTitle: codeData.title, 
                    amount: total, 
                    sellerAmount: base, 
                    timestamp: serverTimestamp() 
                });
                
                alert("Purchase successful! You now have access to the code."); 
                viewItem(id);
            }
        };

        // --- Wallet & Finance Control ---
        window.openFinance = (type) => { 
            financeRequestType = type; 
            const form = document.getElementById('fin-form'); 
            form.classList.toggle('hidden'); 
            if(form.classList.contains('hidden')) return; 
            
            document.getElementById('fin-title').innerText = type.toUpperCase() + " FUNDS"; 
            document.getElementById('with-fee-box').classList.toggle('hidden', type === 'deposit'); 
            document.getElementById('fin-info').placeholder = type === 'deposit' ? 'Enter Transaction ID' : 'Enter Receiver Account Number'; 
            document.getElementById('payment-details-box').classList.toggle('hidden', type !== 'deposit'); 
            if (type === 'deposit') updatePaymentInfo(); 
        };
        
        window.updatePaymentInfo = () => { 
            if (financeRequestType !== 'deposit') return; 
            const method = document.getElementById('fin-method').value; 
            document.getElementById('payment-method-name').innerText = method; 
            document.getElementById('payment-number').innerText = adminSettings.paymentMethods[method] || "Not available"; 
        };
        
        window.copyToClipboard = (text, type) => { 
            navigator.clipboard.writeText(text).then(() => { alert(`${type} copied!`); }); 
        };
        
        window.calculateWithdrawFee = () => { 
            if(financeRequestType !== 'withdrawal') return; 
            const amt = parseFloat(document.getElementById('fin-amt').value) || 0; 
            const comm = (amt * adminSettings.withComm) / 100; 
            document.getElementById('with-comm-val').innerText = comm.toFixed(0) + " BDT"; 
            document.getElementById('with-receive').innerText = (amt - comm).toFixed(0) + " BDT"; 
        };
        
        window.submitFinanceRequest = async () => { 
            const amt = parseFloat(document.getElementById('fin-amt').value); 
            const info = document.getElementById('fin-info').value; 
            const method = document.getElementById('fin-method').value; 
            
            if(!amt || !info) return alert("Please fill all fields!"); 
            
            if(financeRequestType === 'withdrawal') { 
                if(amt > currentUserBalance) return alert("❌ Error: Insufficient Balance!"); 
                await updateDoc(doc(db, "users", currentUser.uid), { balance: currentUserBalance - amt }); 
            }
            
            const payload = { 
                userId: currentUser.uid, 
                email: currentUser.email, 
                amount: amt, 
                method, 
                info, 
                status: 'pending', 
                timestamp: serverTimestamp() 
            }; 
            
            if(financeRequestType === 'withdrawal') {
                payload.fee = (amt * adminSettings.withComm / 100); 
            }
            
            const colName = financeRequestType === 'deposit' ? 'deposits' : 'withdrawals';
            await addDoc(collection(db, colName), payload); 
            
            alert("Request submitted successfully!"); 
            document.getElementById('fin-form').classList.add('hidden'); 
            document.getElementById('fin-amt').value = ''; 
            document.getElementById('fin-info').value = '';
        };
        
        // --- Activity History Engine ---
        window.switchHistoryTab = (type) => {
            document.querySelectorAll('#view-wallet button[id^="hist-tab-"]').forEach(btn => { 
                btn.classList.remove('hist-tab-active', 'text-white'); 
                btn.classList.add('text-slate-500'); 
            });
            document.getElementById('hist-tab-' + type).classList.add('hist-tab-active', 'text-white'); 
            document.getElementById('hist-tab-' + type).classList.remove('text-slate-500');
            
            const fieldName = type === 'purchases' ? 'buyerId' : 'userId';
            
            onSnapshot(query(collection(db, type), where(fieldName, '==', currentUser.uid)), (snap) => {
                const list = document.getElementById('hist-content-list');
                
                if(snap.empty) { 
                    list.innerHTML = `<p class="text-center text-[10px] text-slate-600 py-10 uppercase tracking-widest">No Records Found</p>`; 
                    return; 
                }
                
                let items = []; 
                snap.forEach(doc => items.push(doc.data()));
                
                items.sort((a, b) => {
                    const timeA = a.timestamp ? a.timestamp.toMillis() : Date.now();
                    const timeB = b.timestamp ? b.timestamp.toMillis() : Date.now();
                    return timeB - timeA;
                });
                
                let html = '';
                items.forEach(d => { 
                    const title = type === 'purchases' ? d.codeTitle : type.charAt(0).toUpperCase() + type.slice(1, -1);
                    const prefix = type === 'purchases' ? '-' : (type === 'deposits' ? '+' : '-');
                    const badgeClass = d.status === 'approved' ? 'border-green-500' : (d.status === 'rejected' ? 'border-red-500' : 'border-amber-500');
                    const timeText = d.timestamp ? new Date(d.timestamp.seconds * 1000).toLocaleString() : 'Just Now';
                    const valueClass = prefix === '+' ? 'text-green-400' : 'text-white';
                    
                    html += `
                        <div class="glass p-4 rounded-2xl flex justify-between items-center border-l-4 ${badgeClass}">
                            <div>
                                <p class="text-sm font-bold text-slate-100 truncate w-48">${title}</p>
                                <p class="text-[9px] text-slate-400 font-bold">${d.method || 'Market'} | ${timeText}</p>
                            </div>
                            <div class="text-right">
                                <p class="font-bold text-lg ${valueClass}">${prefix}${d.amount.toFixed(0)} <span class="text-xs">BDT</span></p>
                                <span class="status-${d.status || 'pending'} text-[8px] font-black uppercase tracking-tighter">${d.status || 'Pending'}</span>
                            </div>
                        </div>
                    `; 
                });
                list.innerHTML = html;
            });
        };

        // Standard Modal Close
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');

    </script>
</body>
</html>
