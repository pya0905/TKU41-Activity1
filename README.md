# TKU41-Activity1[code_artifact (4).html](https://github.com/user-attachments/files/33675338/code_artifact.4.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>工作室打卡集點卡</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700;900&display=swap" rel="stylesheet">
    <!-- html5-qrcode CDN -->
    <script src="https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js"></script>
    <style>
        body { font-family: 'Noto Sans TC', sans-serif; }
        .stamp-animation { animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards; }
        @keyframes popIn {
            0% { transform: scale(2) rotate(-15deg); opacity: 0; }
            100% { transform: scale(1) rotate(-12deg); opacity: 1; }
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen pb-12">

    <!-- Navigation Bar -->
    <nav class="bg-slate-800/80 backdrop-blur-md border-b border-slate-700/60 sticky top-0 z-40">
        <div class="max-w-md mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center gap-2">
                <div class="w-8 h-8 rounded-xl bg-indigo-600 flex items-center justify-center text-white font-black text-sm shadow-lg shadow-indigo-500/30">
                    <i class="fa-solid fa-stamp"></i>
                </div>
                <span class="font-bold text-lg text-white tracking-wide">工作室打卡</span>
            </div>
            <!-- Tab Switcher -->
            <div class="bg-slate-950 p-1 rounded-xl flex border border-slate-800 text-xs font-semibold">
                <button id="tab-card-btn" onclick="switchTab('card')" class="px-3 py-1.5 rounded-lg bg-indigo-600 text-white transition-all shadow-sm">
                    <i class="fa-solid fa-id-card mr-1"></i> 我的點數
                </button>
                <button id="tab-admin-btn" onclick="openAdminModal()" class="px-3 py-1.5 rounded-lg text-slate-400 hover:text-slate-200 transition-all">
                    <i class="fa-solid fa-lock mr-1"></i> 店家後台
                </button>
            </div>
        </div>
    </nav>

    <main class="max-w-md mx-auto px-4 mt-6 space-y-6">

        <!-- Banner / Header Info -->
        <div class="bg-gradient-to-br from-indigo-900/40 via-purple-900/30 to-slate-800 border border-indigo-500/30 rounded-3xl p-5 shadow-2xl relative overflow-hidden">
            <div class="absolute -right-6 -bottom-6 w-32 h-32 bg-indigo-500/10 rounded-full blur-2xl"></div>
            <div class="relative z-10 space-y-2">
                <div class="inline-flex items-center gap-1.5 bg-indigo-500/20 text-indigo-300 border border-indigo-500/30 text-xs font-medium px-2.5 py-0.5 rounded-full">
                    <i class="fa-solid fa-sparkles"></i> 6 大工作室聯合打卡
                </div>
                <h1 class="text-2xl font-black text-white">工作室打卡集點卡</h1>
                <p class="text-xs text-slate-300 leading-relaxed">走訪 6 家工作室並掃描現場 QR Code，集滿章即可兌換限定好禮！</p>
            </div>
        </div>

        <!-- Stamp Card Grid -->
        <div class="bg-slate-800 border border-slate-700/80 rounded-3xl p-5 shadow-xl space-y-4">
            <div class="flex justify-between items-center border-b border-slate-700/80 pb-3">
                <span class="text-sm font-bold text-slate-200 flex items-center gap-1.5">
                    <i class="fa-solid fa-map-pin text-indigo-400"></i> 打卡進度
                </span>
                <span class="text-xs font-mono bg-indigo-950 text-indigo-300 border border-indigo-800/60 px-2.5 py-1 rounded-full font-bold">
                    <span id="collected-count">0</span> / 6 完成
                </span>
            </div>

            <!-- 6 Studio Stamp Cells -->
            <div id="stamp-grid" class="grid grid-cols-2 gap-3.5">
                <!-- JS Dynamic Rendered -->
            </div>

            <!-- Primary Camera Scan Button -->
            <button onclick="startCameraScan()" class="w-full mt-2 bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-500 hover:to-purple-500 active:scale-95 text-white font-bold py-3.5 px-5 rounded-2xl shadow-lg shadow-indigo-600/30 flex items-center justify-center gap-2.5 text-sm transition-all border border-indigo-400/30">
                <i class="fa-solid fa-camera text-base"></i> 開啟相機掃描 QR Code 打卡
            </button>

            <!-- Scan Notice / Mobile Tips -->
            <div class="bg-slate-900/60 border border-slate-700/50 p-3 rounded-2xl text-[11px] text-slate-400 space-y-1">
                <div class="font-bold text-slate-300 flex items-center gap-1">
                    <i class="fa-solid fa-circle-info text-indigo-400"></i> 手機掃描提醒（防坑指南）
                </div>
                <ul class="list-disc list-inside space-y-0.5 text-slate-400">
                    <li>必須使用 <strong class="text-amber-300">HTTPS</strong> 或 <strong class="text-amber-300">localhost</strong> 環境方可存取相機。</li>
                    <li>iOS 請用 Safari / Chrome；若由 LINE 開啟，請用右上角選單轉「以預設瀏覽器開啟」。</li>
                </ul>
            </div>

            <!-- Manual Code Toggle Link -->
            <div class="text-center pt-1">
                <button onclick="toggleManualInput()" class="text-xs text-slate-400 hover:text-indigo-300 underline underline-offset-4">
                    改用輸入文字碼打卡
                </button>
            </div>

            <!-- Manual Input Drawer (Hidden by default) -->
            <div id="manual-input-box" class="hidden bg-slate-900/80 border border-slate-700 p-3.5 rounded-2xl space-y-2 mt-2">
                <label class="text-xs text-slate-400 font-medium block">請輸入打卡代碼：</label>
                <div class="flex gap-2">
                    <input type="text" id="manual-code-input" placeholder="例如：FISH-STUDIO" class="bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-100 uppercase tracking-wider w-full focus:outline-none focus:border-indigo-500 font-mono">
                    <button onclick="submitManualCode()" class="bg-indigo-600 hover:bg-indigo-500 text-white font-bold px-4 py-2 rounded-xl text-xs whitespace-nowrap">
                        確認打卡
                    </button>
                </div>
            </div>
        </div>

        <!-- Reward Banner -->
        <div id="reward-card" class="bg-slate-800/80 border border-slate-700 rounded-2xl p-4 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-amber-500/20 border border-amber-500/40 text-amber-400 flex items-center justify-center text-xl">
                    <i class="fa-solid fa-gift"></i>
                </div>
                <div>
                    <h4 class="text-sm font-bold text-slate-200">全打卡完滿獎勵</h4>
                    <p id="reward-status-text" class="text-xs text-slate-400">尚未完成所有打卡</p>
                </div>
            </div>
            <button id="claim-btn" disabled class="opacity-50 cursor-not-allowed bg-amber-500 text-slate-950 font-bold text-xs px-3.5 py-2 rounded-xl">
                領取獎勵
            </button>
        </div>

    </main>

    <!-- Fullscreen Camera Scanner Modal -->
    <div id="scanner-modal" class="fixed inset-0 bg-slate-950/90 backdrop-blur-md z-50 hidden flex flex-col items-center justify-center p-4">
        <div class="bg-slate-800 border border-slate-700/80 rounded-3xl p-5 max-w-sm w-full space-y-4 shadow-2xl relative">
            
            <div class="flex justify-between items-center border-b border-slate-700/80 pb-3">
                <h3 class="font-bold text-slate-100 flex items-center gap-2 text-sm">
                    <i class="fa-solid fa-camera text-indigo-400"></i> 對準工作室 QR Code 掃描
                </h3>
                <button onclick="stopCameraScan()" class="text-slate-400 hover:text-slate-200 w-8 h-8 rounded-full bg-slate-700/50 flex items-center justify-center transition-colors">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <!-- Video Preview Container -->
            <div class="relative overflow-hidden rounded-2xl bg-black border border-slate-700 min-h-[260px] flex items-center justify-center">
                <div id="qr-reader" class="w-full"></div>
                <p id="loading-spinner" class="absolute text-xs text-slate-400 flex items-center gap-2">
                    <i class="fa-solid fa-circle-notch fa-spin text-indigo-400 text-lg"></i> 啟動後置相機中...
                </p>
            </div>

            <p id="scan-error-msg" class="text-xs text-rose-400 text-center hidden leading-relaxed font-medium bg-rose-950/50 border border-rose-800/50 p-2.5 rounded-xl"></p>

            <button onclick="stopCameraScan()" class="w-full bg-slate-700 hover:bg-slate-600 text-slate-200 py-2.5 rounded-xl text-sm font-medium transition-colors">
                取消關閉
            </button>
        </div>
    </div>

    <!-- Admin Login Modal -->
    <div id="admin-login-modal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-md z-50 hidden flex items-center justify-center p-4">
        <div class="bg-slate-800 border border-slate-700 rounded-3xl p-6 max-w-xs w-full space-y-4 text-center shadow-2xl">
            <div class="w-12 h-12 bg-indigo-600/20 text-indigo-400 border border-indigo-500/30 rounded-2xl flex items-center justify-center mx-auto text-xl">
                <i class="fa-solid fa-lock"></i>
            </div>
            <div>
                <h3 class="font-bold text-white text-base">店家後台驗證</h3>
                <p class="text-xs text-slate-400 mt-1">請輸入店家專屬管理密碼</p>
            </div>
            <input type="password" id="admin-password-input" placeholder="請輸入密碼" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2.5 text-center text-sm text-white focus:outline-none focus:border-indigo-500">
            <p id="admin-login-err" class="text-xs text-rose-400 hidden">密碼錯誤，請重新輸入</p>
            <div class="flex gap-2">
                <button onclick="closeAdminModal()" class="w-1/2 bg-slate-700 text-slate-300 py-2 rounded-xl text-xs font-bold">取消</button>
                <button onclick="verifyAdminPassword()" class="w-1/2 bg-indigo-600 text-white py-2 rounded-xl text-xs font-bold">驗證登入</button>
            </div>
        </div>
    </div>

    <!-- Admin Panel Container (Hidden by default) -->
    <div id="admin-panel" class="hidden max-w-md mx-auto px-4 mt-6 space-y-6">
        <div class="bg-slate-800 border border-slate-700 rounded-3xl p-5 space-y-4">
            <div class="flex justify-between items-center border-b border-slate-700 pb-3">
                <div>
                    <h2 class="font-bold text-white text-base flex items-center gap-2">
                        <i class="fa-solid fa-qrcode text-indigo-400"></i> 店家專屬 QR Code
                    </h2>
                    <p class="text-xs text-slate-400">可供現場展示或讓參與者掃描打卡</p>
                </div>
                <button onclick="exitAdmin()" class="text-xs bg-slate-700 text-slate-300 px-3 py-1.5 rounded-lg">離開後台</button>
            </div>

            <!-- List of Studios with QR Codes -->
            <div id="admin-qr-list" class="space-y-4">
                <!-- JS Rendered -->
            </div>
        </div>
    </div>

    <script>
        // 6 大工作室基本資料與專屬打卡碼
        const STUDIOS = [
            { id: 'fish', name: '自漁自樂', code: 'FISH-STUDIO', icon: 'fa-fish-fins', color: 'from-blue-500 to-cyan-500' },
            { id: 'para', name: '平行工作室', code: 'PARA-STUDIO', icon: 'fa-shapes', color: 'from-purple-500 to-indigo-500' },
            { id: 'iso7', name: 'ISO7', code: 'ISO7-STUDIO', icon: 'fa-cube', color: 'from-emerald-500 to-teal-500' },
            { id: 'peach', name: '玖桃', code: 'PEACH-STUDIO', icon: 'fa-peach-puff', color: 'from-pink-500 to-rose-500' },
            { id: 'ladybug', name: 'Ladybug', code: 'LADY-STUDIO', icon: 'fa-bugs', color: 'from-red-500 to-orange-500' },
            { id: 'moshi', name: 'Moshi Moshi', code: 'MOSHI-STUDIO', icon: 'fa-headset', color: 'from-amber-500 to-yellow-500' }
        ];

        // 顧客集點狀態（紀錄已完成的 studio id）
        let userProgress = new Set();
        let html5QrCode = null;

        // 初始化
        document.addEventListener('DOMContentLoaded', () => {
            renderStampGrid();
            renderAdminQRList();
        });

        /**
         * 渲染顧客 6 個工作室卡牌集點格
         */
        function renderStampGrid() {
            const grid = document.getElementById('stamp-grid');
            grid.innerHTML = '';

            STUDIOS.forEach(studio => {
                const isStamped = userProgress.has(studio.id);
                
                const cell = document.createElement('div');
                cell.className = `relative p-4 rounded-2xl border transition-all flex flex-col items-center justify-center text-center gap-2 overflow-hidden ${
                    isStamped 
                    ? 'bg-slate-800/90 border-indigo-500/60 shadow-lg shadow-indigo-950/50' 
                    : 'bg-slate-900/50 border-slate-800 text-slate-500'
                }`;

                cell.innerHTML = `
                    <div class="w-12 h-12 rounded-2xl flex items-center justify-center text-xl ${
                        isStamped 
                        ? `bg-gradient-to-tr ${studio.color} text-white shadow-md` 
                        : 'bg-slate-800 text-slate-600'
                    }">
                        <i class="fa-solid ${studio.icon}"></i>
                    </div>
                    <span class="text-xs font-bold ${isStamped ? 'text-slate-100' : 'text-slate-400'}">${studio.name}</span>
                    
                    ${isStamped ? `
                        <div class="absolute inset-0 bg-indigo-950/40 backdrop-blur-[1px] flex items-center justify-center stamp-animation">
                            <div class="border-2 border-emerald-400 text-emerald-400 rounded-full px-3 py-1 font-black text-xs uppercase tracking-widest bg-slate-950/80 shadow-2xl flex items-center gap-1">
                                <i class="fa-solid fa-check"></i> 已打卡
                            </div>
                        </div>
                    ` : ''}
                `;
                grid.appendChild(cell);
            });

            // 更新計數
            document.getElementById('collected-count').innerText = userProgress.size;

            // 檢查全集滿
            const rewardText = document.getElementById('reward-status-text');
            const claimBtn = document.getElementById('claim-btn');

            if (userProgress.size === STUDIOS.length) {
                rewardText.innerText = '恭喜完成全工作室打卡！已解鎖好禮';
                rewardText.className = 'text-xs text-emerald-400 font-bold';
                claimBtn.disabled = false;
                claimBtn.className = 'bg-amber-400 hover:bg-amber-300 text-slate-950 font-bold text-xs px-3.5 py-2 rounded-xl shadow-lg shadow-amber-500/20 active:scale-95 transition-all';
                claimBtn.onclick = () => alert('🎉 恭喜！請向現場工作人員出示此畫面領取限定紀念品！');
            } else {
                rewardText.innerText = `還差 ${STUDIOS.length - userProgress.size} 個工作室即可領取`;
                rewardText.className = 'text-xs text-slate-400';
                claimBtn.disabled = true;
                claimBtn.className = 'opacity-50 cursor-not-allowed bg-amber-500 text-slate-950 font-bold text-xs px-3.5 py-2 rounded-xl';
            }
        }

        /**
         * 啟動手機相機（強制使用後置主鏡頭 facingMode: "environment"）
         */
        async function startCameraScan() {
            const modal = document.getElementById('scanner-modal');
            const errorMsg = document.getElementById('scan-error-msg');
            const loadingSpinner = document.getElementById('loading-spinner');

            errorMsg.classList.add('hidden');
            errorMsg.innerText = '';
            loadingSpinner.classList.remove('hidden');
            modal.classList.remove('hidden');

            if (!html5QrCode) {
                html5QrCode = new Html5Qrcode("qr-reader");
            }

            // 設置相機掃描參數，強制指定 facingMode: "environment" (後置鏡頭)
            const cameraConfig = { facingMode: { exact: "environment" } };
            const scanConfig = { fps: 10, qrbox: { width: 220, height: 220 } };

            try {
                // 1. 主動請求視訊權限（指定後置鏡頭）
                await navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } });

                // 2. 啟動 html5QrCode 掃描器
                await html5QrCode.start(
                    { facingMode: "environment" }, // 強制調用後置主鏡頭
                    scanConfig,
                    onScanSuccess,
                    onScanError
                );
                loadingSpinner.classList.add('hidden');
            } catch (err) {
                loadingSpinner.classList.add('hidden');
                console.warn("後置鏡頭啟動失敗或不受支援，嘗試通用模式：", err);
                
                // 3. Fallback: 部分手機不接受 exact 或嚴格模式，改用軟性 environment 模式
                try {
                    await html5QrCode.start(
                        { facingMode: "user" },
                        scanConfig,
                        onScanSuccess,
                        onScanError
                    );
                } catch (fallbackErr) {
                    showScanError("無法啟動相機！請確認：\n1. 網頁是否使用 HTTPS 加密連線\n2. 是否允許瀏覽器存取相機權限\n3. 若在 LINE/FB 內建瀏覽器，請轉以 Safari/Chrome 開啟。");
                }
            }
        }

        /**
         * 掃描成功後的回呼函式
         */
        function onScanSuccess(decodedText, decodedResult) {
            // 立即停止相機並關閉視窗
            stopCameraScan();
            // 執行打卡比對邏輯
            processCheckInCode(decodedText.trim());
        }

        function onScanError(error) {
            // 掃描過程中的尋找條碼訊息，可無視
        }

        /**
         * 停止相機與關閉視窗
         */
        function stopCameraScan() {
            const modal = document.getElementById('scanner-modal');
            if (html5QrCode && html5QrCode.isScanning) {
                html5QrCode.stop().then(() => {
                    modal.classList.add('hidden');
                }).catch(() => {
                    modal.classList.add('hidden');
                });
            } else {
                modal.classList.add('hidden');
            }
        }

        function showScanError(msg) {
            const errorMsg = document.getElementById('scan-error-msg');
            errorMsg.innerText = msg;
            errorMsg.classList.remove('hidden');
        }

        /**
         * 處理輸入/掃描到的打卡代碼
         */
        function processCheckInCode(code) {
            const matchedStudio = STUDIOS.find(s => s.code.toUpperCase() === code.toUpperCase());

            if (!matchedStudio) {
                alert(`❌ 無效的打卡碼或 QR Code（內容: ${code}），請確認是否為 6 大工作室專屬碼！`);
                return;
            }

            if (userProgress.has(matchedStudio.id)) {
                alert(`⚠️ 您已經完成「${matchedStudio.name}」的打卡了，不可重複打卡！`);
                return;
            }

            // 成功打卡
            userProgress.add(matchedStudio.id);
            renderStampGrid();
            alert(`🎉 成功完成「${matchedStudio.name}」打卡！`);
        }

        /**
         * 切換手動代碼輸入框
         */
        function toggleManualInput() {
            const box = document.getElementById('manual-input-box');
            box.classList.toggle('hidden');
        }

        /**
         * 手動提交代碼
         */
        function submitManualCode() {
            const input = document.getElementById('manual-code-input');
            const code = input.value.trim();
            if (!code) return;
            processCheckInCode(code);
            input.value = '';
        }

        // ================= 店家後台模組 (密碼: tku41) =================

        function openAdminModal() {
            document.getElementById('admin-password-input').value = '';
            document.getElementById('admin-login-err').classList.add('hidden');
            document.getElementById('admin-login-modal').classList.remove('hidden');
        }

        function closeAdminModal() {
            document.getElementById('admin-login-modal').classList.add('hidden');
        }

        function verifyAdminPassword() {
            const pwd = document.getElementById('admin-password-input').value.trim();
            if (pwd === 'tku41') {
                closeAdminModal();
                switchTab('admin');
            } else {
                document.getElementById('admin-login-err').classList.remove('hidden');
            }
        }

        function switchTab(tab) {
            const main = document.querySelector('main');
            const adminPanel = document.getElementById('admin-panel');
            const cardBtn = document.getElementById('tab-card-btn');
            const adminBtn = document.getElementById('tab-admin-btn');

            if (tab === 'card') {
                main.classList.remove('hidden');
                adminPanel.classList.add('hidden');
                cardBtn.className = 'px-3 py-1.5 rounded-lg bg-indigo-600 text-white transition-all shadow-sm';
                adminBtn.className = 'px-3 py-1.5 rounded-lg text-slate-400 hover:text-slate-200 transition-all';
            } else {
                main.classList.add('hidden');
                adminPanel.classList.remove('hidden');
                adminBtn.className = 'px-3 py-1.5 rounded-lg bg-indigo-600 text-white transition-all shadow-sm';
                cardBtn.className = 'px-3 py-1.5 rounded-lg text-slate-400 hover:text-slate-200 transition-all';
            }
        }

        function exitAdmin() {
            switchTab('card');
        }

        /**
         * 渲染店家後台專屬固定 QR Code
         */
        function renderAdminQRList() {
            const container = document.getElementById('admin-qr-list');
            container.innerHTML = '';

            STUDIOS.forEach(studio => {
                const qrUrl = `https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=${encodeURIComponent(studio.code)}`;

                const card = document.createElement('div');
                card.className = 'bg-slate-900 border border-slate-700 p-4 rounded-2xl flex items-center justify-between gap-4';
                card.innerHTML = `
                    <div class="space-y-1">
                        <span class="text-xs font-bold text-indigo-400 uppercase tracking-wider">Studio</span>
                        <h4 class="text-base font-bold text-white">${studio.name}</h4>
                        <p class="text-xs font-mono text-slate-400 bg-slate-950 px-2.5 py-1 rounded-lg inline-block border border-slate-800">${studio.code}</p>
                    </div>
                    <div class="bg-white p-2 rounded-xl shadow-md flex-shrink-0">
                        <img src="${qrUrl}" alt="${studio.name} QR Code" class="w-20 h-20">
                    </div>
                `;
                container.appendChild(card);
            });
        }
    </script>
</body>
</html>
