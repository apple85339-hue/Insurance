<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>保險業務員數位轉型互動指南 - 尊榮美化版</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --primary: #0D9488;
            --primary-light: #F0FDFA;
            --accent: #F59E0B;
            --bg-main: #F8FAFC;
        }

        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: var(--bg-main);
            color: #1E293B;
            overflow-x: hidden;
        }

        /* 磨砂玻璃效果 */
        .glass-card {
            background: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.3);
            box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.07);
        }

        /* 漸層背景裝飾 */
        .bg-gradient-circle {
            position: fixed;
            z-index: -1;
            border-radius: 50%;
            filter: blur(80px);
            opacity: 0.4;
        }

        /* 圖表容器約束 */
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 450px; 
            margin: 0 auto;
            height: 320px;
        }

        /* 內容切換動畫 */
        .tab-content {
            display: none;
            animation: slideUp 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .tab-content.active {
            display: block;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* 自定義導覽按鈕 */
        .nav-link {
            position: relative;
            transition: all 0.3s ease;
        }

        .nav-link.active {
            color: var(--primary);
            font-weight: 700;
        }

        .nav-link.active::after {
            content: '';
            position: absolute;
            bottom: -2px;
            left: 1rem;
            right: 1rem;
            height: 3px;
            background: var(--primary);
            border-radius: 99px;
        }

        /* 隱藏捲軸但保持功能 */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        /* 模擬聊天氣泡動畫 */
        .chat-bubble {
            animation: popIn 0.3s ease-out forwards;
            transform-origin: left bottom;
        }

        @keyframes popIn {
            from { opacity: 0; scale: 0.8; }
            to { opacity: 1; scale: 1; }
        }
    </style>

    <!-- Chosen Palette: 尊榮專業色系 (Teal-700 作為權威色, Amber-500 作為行動呼籲, Slate-50 作為背景, 並使用半透明白色增加呼吸感) -->
    <!-- Application Structure Plan: 採用「沉浸式分頁系統」。導覽列被設計為輕量化的頂部懸浮感列（在行動端更顯精緻）。主體內容區使用單獨的「大卡片」設計，將原本的四個階段轉化為四個充滿視覺深度的互動區域。結構邏輯：從宏觀理念到微觀行動，透過漸進式的資訊揭露，讓學習曲線平緩且愉悅。 -->
    <!-- Visualization & Content Choices: 
         1. 導覽：頂部響應式按鈕，具備底線指示器。
         2. 階段一：進度圓環感設計，搭配卡片式 Checkbox。
         3. 階段二：Chart.js 甜甜圈圖，點擊後觸發右側玻璃質感的資訊卡片更換。
         4. 階段三：高仿真的 iOS 風格聊天介面，增加代入感。
         5. 階段四：帶有動態陰影的垂直步驟清單。
         確認：未使用 SVG/Mermaid。圖表使用 Chart.js，圖示使用 Emoji。 -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
</head>
<body class="min-h-screen">

    <!-- 背景裝飾球 -->
    <div class="bg-gradient-circle bg-teal-200 w-96 h-96 -top-20 -left-20"></div>
    <div class="bg-gradient-circle bg-amber-100 w-80 h-80 bottom-10 right-10"></div>

    <!-- 頂部導覽列 -->
    <header class="sticky top-0 z-50 px-4 py-3">
        <nav class="max-w-5xl mx-auto glass-card rounded-2xl px-6 py-2 flex items-center justify-between">
            <div class="flex items-center gap-2">
                <span class="text-2xl">💎</span>
                <span class="font-bold text-slate-800 tracking-tight hidden sm:inline">數位菁英教練</span>
            </div>
            <div class="flex gap-1 overflow-x-auto no-scrollbar py-1">
                <button onclick="switchTab('intro')" id="btn-intro" class="nav-link active px-3 py-2 text-sm text-slate-500 hover:text-teal-600">理念</button>
                <button onclick="switchTab('phase1')" id="btn-phase1" class="nav-link px-3 py-2 text-sm text-slate-500 hover:text-teal-600">店面</button>
                <button onclick="switchTab('phase2')" id="btn-phase2" class="nav-link px-3 py-2 text-sm text-slate-500 hover:text-teal-600">內容</button>
                <button onclick="switchTab('phase3')" id="btn-phase3" class="nav-link px-3 py-2 text-sm text-slate-500 hover:text-teal-600">AI</button>
                <button onclick="switchTab('phase4')" id="btn-phase4" class="nav-link px-3 py-2 text-sm text-slate-500 hover:text-teal-600">行動</button>
            </div>
        </nav>
    </header>

    <main class="max-w-5xl mx-auto px-4 py-8 md:py-12">
        
        <!-- Section: Intro -->
        <section id="intro" class="tab-content active">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-center">
                <div class="lg:col-span-7">
                    <span class="bg-teal-100 text-teal-700 px-3 py-1 rounded-full text-xs font-bold tracking-widest uppercase mb-4 inline-block">Digital Transformation</span>
                    <h2 class="text-4xl md:text-5xl font-extrabold text-slate-900 leading-tight mb-6">
                        數位轉型，<br><span class="text-teal-600">是信任的無限延伸</span>
                    </h2>
                    <p class="text-lg text-slate-600 leading-relaxed mb-8">
                        教練告訴你：數位化不是要你變成機器人，而是要讓你那顆「關懷的心」，透過網路被更多人看見。讓我們一起把專業溫度，轉化為持續的影響力。
                    </p>
                    <div class="flex flex-wrap gap-4">
                        <button onclick="switchTab('phase1')" class="bg-teal-600 hover:bg-teal-700 text-white px-8 py-3 rounded-xl font-bold transition-all shadow-lg shadow-teal-200">
                            立即開始裝潢
                        </button>
                    </div>
                </div>
                <div class="lg:col-span-5">
                    <div class="glass-card p-8 rounded-3xl relative overflow-hidden">
                        <div class="absolute -top-10 -right-10 text-9xl opacity-10">☕</div>
                        <h3 class="text-xl font-bold text-slate-800 mb-4">💡 轉型關鍵字</h3>
                        <div class="space-y-4">
                            <div class="flex items-center gap-4">
                                <div class="w-12 h-12 bg-white rounded-xl shadow-sm flex items-center justify-center text-xl">📢</div>
                                <div>
                                    <div class="font-bold text-slate-800">擴音器</div>
                                    <div class="text-xs text-slate-500">讓你的專業聲音傳得更遠</div>
                                </div>
                            </div>
                            <div class="flex items-center gap-4">
                                <div class="w-12 h-12 bg-white rounded-xl shadow-sm flex items-center justify-center text-xl">⚖️</div>
                                <div>
                                    <div class="font-bold text-slate-800">省力槓桿</div>
                                    <div class="text-xs text-slate-500">用更少的時間維護更多客戶</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Section: Phase 1 -->
        <section id="phase1" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第一階段：數位店面優化</h2>
                <p class="text-slate-500 mt-2">給客戶最好的數位第一印象</p>
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="space-y-4">
                    <div class="glass-card p-6 rounded-2xl hover:shadow-md transition-shadow group">
                        <label class="flex items-center gap-4 cursor-pointer">
                            <input type="checkbox" class="profile-check w-6 h-6 rounded-lg text-teal-600 focus:ring-teal-500" onchange="updateProgress()">
                            <div class="flex-1">
                                <h4 class="font-bold text-slate-800">專業大頭照</h4>
                                <p class="text-xs text-slate-500">避免使用生活雜亂照，展現親和與專業</p>
                            </div>
                            <span class="text-2xl group-hover:scale-110 transition-transform">📸</span>
                        </label>
                    </div>
                    <div class="glass-card p-6 rounded-2xl hover:shadow-md transition-shadow group">
                        <label class="flex items-center gap-4 cursor-pointer">
                            <input type="checkbox" class="profile-check w-6 h-6 rounded-lg text-teal-600 focus:ring-teal-500" onchange="updateProgress()">
                            <div class="flex-1">
                                <h4 class="font-bold text-slate-800">服務理念狀態</h4>
                                <p class="text-xs text-slate-500">用一句話寫出你能為客戶解決的問題</p>
                            </div>
                            <span class="text-2xl group-hover:scale-110 transition-transform">✍️</span>
                        </label>
                    </div>
                    <div class="glass-card p-6 rounded-2xl hover:shadow-md transition-shadow group">
                        <label class="flex items-center gap-4 cursor-pointer">
                            <input type="checkbox" class="profile-check w-6 h-6 rounded-lg text-teal-600 focus:ring-teal-500" onchange="updateProgress()">
                            <div class="flex-1">
                                <h4 class="font-bold text-slate-800">貼文串暖身</h4>
                                <p class="text-xs text-slate-500">準備好第一篇自我介紹或價值分享</p>
                            </div>
                            <span class="text-2xl group-hover:scale-110 transition-transform">🚀</span>
                        </label>
                    </div>
                </div>

                <div class="glass-card p-10 rounded-3xl flex flex-col items-center justify-center text-center">
                    <div class="relative w-40 h-40 flex items-center justify-center mb-6">
                        <div class="absolute inset-0 rounded-full border-8 border-slate-100"></div>
                        <div id="progress-circle" class="absolute inset-0 rounded-full border-8 border-teal-500 transition-all duration-700" style="clip-path: polygon(50% 50%, 50% 0%, 50% 0%, 50% 0%, 50% 0%, 50% 0%)"></div>
                        <div class="text-5xl" id="profile-emoji">👤</div>
                    </div>
                    <h3 class="text-2xl font-bold text-slate-800" id="profile-status-text">店面尚未裝修</h3>
                    <p class="text-slate-500 mt-2" id="profile-progress-text">完成度 0%</p>
                    <div class="mt-6 px-4 py-2 bg-teal-50 text-teal-700 rounded-full text-xs font-bold" id="progress-tip">從第一項開始動手吧！</div>
                </div>
            </div>
        </section>

        <!-- Section: Phase 2 -->
        <section id="phase2" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第二階段：3:2:1 內容黃金比例</h2>
                <p class="text-slate-500 mt-2">不只是發廣告，而是發「價值」</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-10 items-center">
                <div class="glass-card p-8 rounded-3xl">
                    <div class="chart-container">
                        <canvas id="contentRatioChart"></canvas>
                    </div>
                </div>
                <div id="chart-info-box" class="glass-card p-8 rounded-3xl border-l-8 border-teal-500 min-h-[350px] flex flex-col justify-center transition-all duration-300">
                    <div class="text-5xl mb-4">🎯</div>
                    <h3 class="text-2xl font-bold text-slate-800 mb-4">點擊圖表探索</h3>
                    <p class="text-slate-600 text-lg leading-relaxed">請點擊左側的圓餅圖區塊，教練將直接告訴你每一類型的發文內容該怎麼寫，才能吸引客戶目光。</p>
                </div>
            </div>
        </section>

        <!-- Section: Phase 3 -->
        <section id="phase3" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第三階段：AI 數位秘書</h2>
                <p class="text-slate-500 mt-2">文案寫不出來？讓 AI 來代勞</p>
            </div>

            <div class="max-w-2xl mx-auto glass-card rounded-3xl overflow-hidden shadow-2xl">
                <!-- Chat Header -->
                <div class="bg-gradient-to-r from-slate-800 to-slate-900 p-5 flex items-center justify-between">
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 bg-teal-500 rounded-full flex items-center justify-center shadow-lg text-white">🤖</div>
                        <div>
                            <div class="text-white font-bold text-sm">Gemini AI 助理</div>
                            <div class="text-teal-400 text-[10px] flex items-center gap-1">● 線上服務中</div>
                        </div>
                    </div>
                    <div class="flex gap-2 text-white/30 text-xs font-mono">
                        <span>AI-2.5-FLASH</span>
                    </div>
                </div>

                <!-- Chat Body -->
                <div id="chat-window" class="bg-slate-50 h-[400px] overflow-y-auto p-6 flex flex-col gap-4 no-scrollbar">
                    <div class="chat-bubble bg-white p-4 rounded-2xl rounded-tl-none shadow-sm max-w-[85%] border border-slate-100 text-sm leading-relaxed">
                        教練你好！我是你的 AI 文案助手。如果你有任何想對客戶說的話，但不知道怎麼修飾，儘管交給我！<br><br>請點選下方的模擬任務：
                    </div>
                </div>

                <!-- Chat Footer -->
                <div class="p-4 bg-white border-t border-slate-100">
                    <div class="grid grid-cols-1 sm:grid-cols-3 gap-2">
                        <button onclick="simulateChat('polish')" class="py-2 px-3 bg-teal-50 hover:bg-teal-100 text-teal-700 rounded-xl text-xs font-bold transition-all border border-teal-100">✨ 語氣潤飾</button>
                        <button onclick="simulateChat('ideas')" class="py-2 px-3 bg-amber-50 hover:bg-amber-100 text-amber-700 rounded-xl text-xs font-bold transition-all border border-amber-100">💡 內容發想</button>
                        <button onclick="simulateChat('greetings')" class="py-2 px-3 bg-blue-50 hover:bg-blue-100 text-blue-700 rounded-xl text-xs font-bold transition-all border border-blue-100">🎁 節慶問候</button>
                    </div>
                </div>
            </div>
        </section>

        <!-- Section: Phase 4 -->
        <section id="phase4" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第四階段：15 分鐘數位微行動</h2>
                <p class="text-slate-500 mt-2">每天一點點，數位影響力大無限</p>
            </div>

            <div class="max-w-3xl mx-auto space-y-6">
                <div class="glass-card p-1 rounded-3xl overflow-hidden hover:shadow-xl transition-all cursor-pointer" onclick="toggleTimeline('step1')">
                    <div class="flex items-center gap-6 p-6">
                        <div class="w-16 h-16 bg-amber-100 rounded-2xl flex items-center justify-center text-3xl shadow-inner">🤝</div>
                        <div class="flex-1">
                            <span class="text-[10px] font-bold text-amber-600 tracking-widest uppercase">0 - 5 Minutes</span>
                            <h3 class="text-xl font-bold text-slate-800">數位互動：維持關係熱度</h3>
                        </div>
                        <div id="arrow-step1" class="text-slate-400 transition-transform">▼</div>
                    </div>
                    <div id="step1-details" class="hidden px-8 pb-8 pt-2 text-slate-600 border-t border-slate-50 mt-2">
                        回覆客戶留言、點讚動態。這就像在路上打招呼，雖然簡單，卻能讓客戶覺得你「一直都在」。
                    </div>
                </div>

                <div class="glass-card p-1 rounded-3xl overflow-hidden hover:shadow-xl transition-all cursor-pointer" onclick="toggleTimeline('step2')">
                    <div class="flex items-center gap-6 p-6">
                        <div class="w-16 h-16 bg-teal-100 rounded-2xl flex items-center justify-center text-3xl shadow-inner">📸</div>
                        <div class="flex-1">
                            <span class="text-[10px] font-bold text-teal-600 tracking-widest uppercase">5 - 10 Minutes</span>
                            <h3 class="text-xl font-bold text-slate-800">維持曝光：展現專業日常</h3>
                        </div>
                        <div id="arrow-step2" class="text-slate-400 transition-transform">▼</div>
                    </div>
                    <div id="step2-details" class="hidden px-8 pb-8 pt-2 text-slate-600 border-t border-slate-50 mt-2">
                        發布一則限時動態。運用 3:2:1 法則，拍一張你正在研讀條款或服務客戶的背影。
                    </div>
                </div>

                <div class="glass-card p-1 rounded-3xl overflow-hidden hover:shadow-xl transition-all cursor-pointer" onclick="toggleTimeline('step3')">
                    <div class="flex items-center gap-6 p-6">
                        <div class="w-16 h-16 bg-blue-100 rounded-2xl flex items-center justify-center text-3xl shadow-inner">📚</div>
                        <div class="flex-1">
                            <span class="text-[10px] font-bold text-blue-600 tracking-widest uppercase">10 - 15 Minutes</span>
                            <h3 class="text-xl font-bold text-slate-800">素材累積：投資明天的自己</h3>
                        </div>
                        <div id="arrow-step3" class="text-slate-400 transition-transform">▼</div>
                    </div>
                    <div id="step3-details" class="hidden px-8 pb-8 pt-2 text-slate-600 border-t border-slate-50 mt-2">
                        閱讀一則財經或醫療新知。將心得記在筆記本或手機裡，這就是你下一篇爆款文案的來源。
                    </div>
                </div>
            </div>
        </section>

    </main>

    <script>
        // --- 導覽切換 ---
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-link').forEach(el => el.classList.remove('active'));

            document.getElementById(tabId).classList.add('active');
            document.getElementById('btn-' + tabId).classList.add('active');

            if (tabId === 'phase2' && !window.myDonutChart) {
                initChart();
            }
            // 平滑滾動到頂部
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // --- 階段一：檢核表與圓環進度 ---
        function updateProgress() {
            const checkboxes = document.querySelectorAll('.profile-check');
            const checkedCount = Array.from(checkboxes).filter(cb => cb.checked).length;
            const percentage = Math.round((checkedCount / checkboxes.length) * 100);
            
            const progressCircle = document.getElementById('progress-circle');
            const progressText = document.getElementById('profile-progress-text');
            const statusText = document.getElementById('profile-status-text');
            const emoji = document.getElementById('profile-emoji');
            const tip = document.getElementById('progress-tip');

            // 圓環動畫模擬 (clip-path 簡單模擬)
            const deg = (percentage / 100) * 360;
            if (percentage === 0) progressCircle.style.clipPath = 'polygon(50% 50%, 50% 0%, 50% 0%, 50% 0%, 50% 0%, 50% 0%)';
            else if (percentage <= 25) progressCircle.style.clipPath = `polygon(50% 50%, 50% 0%, ${50 + percentage * 2}% 0%, ${50 + percentage * 2}% 0%)`;
            else progressCircle.style.clipPath = 'none'; // 簡化處理

            progressText.innerText = `完成度 ${percentage}%`;

            if (percentage === 100) {
                statusText.innerText = "頂級數位名店！";
                emoji.innerText = "👑";
                tip.innerText = "太棒了！你的門面已經超過 90% 的業務員。";
                tip.className = "mt-6 px-4 py-2 bg-teal-500 text-white rounded-full text-xs font-bold shadow-md";
            } else if (percentage > 0) {
                statusText.innerText = "持續優化中...";
                emoji.innerText = "⚡";
                tip.innerText = "做得好！再接再厲。";
            }
        }

        // --- 階段二：Chart.js ---
        const infoData = {
            0: { title: "生活感貼文 (50%)", icon: "☕", color: "#F59E0B", desc: "展現你的真實生活與興趣。例如：登山紀錄、週末親子時光、參與志工活動。", tip: "客戶會因為喜歡你這個『人』，才開始信任你賣的『專業』。" },
            1: { title: "專業知識 (33%)", icon: "📊", color: "#0D9488", desc: "白話解釋財經新聞、理賠常見爭議、保險條款陷阱。", tip: "這部分要展現你的權威感，但切記不要用太多專業術語。" },
            2: { title: "產品/案例 (17%)", icon: "🛡️", color: "#3B82F6", desc: "分享理賠溫馨故事、某個規劃如何拯救一個家庭。", tip: "這不是推銷產品，而是展現『價值的實現』。" }
        };

        function initChart() {
            const ctx = document.getElementById('contentRatioChart').getContext('2d');
            window.myDonutChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['生活感', '專業知識', '案例價值'],
                    datasets: [{
                        data: [3, 2, 1],
                        backgroundColor: ['#F59E0B', '#0D9488', '#3B82F6'],
                        borderWidth: 0,
                        hoverOffset: 20
                    }]
                },
                options: {
                    cutout: '70%',
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    onClick: (e, elements) => {
                        if (elements.length > 0) {
                            const i = elements[0].index;
                            const box = document.getElementById('chart-info-box');
                            box.style.opacity = 0;
                            setTimeout(() => {
                                box.style.borderColor = infoData[i].color;
                                box.innerHTML = `
                                    <div class="text-5xl mb-4">${infoData[i].icon}</div>
                                    <h3 class="text-2xl font-bold text-slate-800 mb-2">${infoData[i].title}</h3>
                                    <p class="text-slate-600 text-lg leading-relaxed mb-4">${infoData[i].desc}</p>
                                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100 italic text-sm text-slate-500">
                                        教練私房話：${infoData[i].tip}
                                    </div>
                                `;
                                box.style.opacity = 1;
                            }, 200);
                        }
                    }
                }
            });
        }

        // --- 階段三：AI 聊天 ---
        const responses = {
            polish: "你：幫我把「買保險很重要，快來找我」改得溫潤。<br><br>助理🤖：改成：『生命的風雨無法預報，但我們可以為愛的人提前撐傘。如果您想討論家庭保障，隨時喝杯咖啡聊聊，我都在。』",
            ideas: "你：長照議題發文靈感？<br><br>助理🤖：試試『老老照護』：分享 70 歲照顧 90 歲的現狀，引發對於提前規劃退休與看護費用的共鳴。",
            greetings: "你：中秋節不要長輩圖祝賀詞。<br><br>助理🤖：『比起月餅，更希望您與家人團聚的時光是甜的。願這輪明月，帶給您一整年的平安與健康。』"
        };

        function simulateChat(type) {
            const win = document.getElementById('chat-window');
            const bubble = document.createElement('div');
            bubble.className = "chat-bubble bg-teal-600 text-white p-4 rounded-2xl rounded-tr-none shadow-md max-w-[85%] self-end text-sm leading-relaxed";
            bubble.innerHTML = responses[type];
            win.appendChild(bubble);
            win.scrollTop = win.scrollHeight;
        }

        // --- 階段四：時間軸 ---
        function toggleTimeline(id) {
            const el = document.getElementById(id + '-details');
            const arrow = document.getElementById('arrow-' + id);
            if (el.classList.contains('hidden')) {
                el.classList.remove('hidden');
                arrow.style.transform = 'rotate(180deg)';
            } else {
                el.classList.add('hidden');
                arrow.style.transform = 'rotate(0deg)';
            }
        }
    </script>
</body>
</html>
