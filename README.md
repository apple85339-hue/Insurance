<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>保險數位行銷四部曲 - 智匯互動版</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --primary: #0D9488;
            --accent: #F59E0B;
            --bg-main: #F8FAFC;
        }

        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: var(--bg-main);
            color: #1E293B;
            overflow-x: hidden;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.4);
            box-shadow: 0 10px 30px -5px rgba(0, 0, 0, 0.05);
        }

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

        .chart-container {
            position: relative;
            width: 100%;
            max-width: 450px; 
            margin: 0 auto;
            height: 300px;
        }

        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        .persona-card.selected {
            border: 3px solid var(--primary);
            background-color: #F0FDFA;
            transform: translateY(-5px);
        }

        .loading-spinner {
            border: 3px solid #f3f3f3;
            border-top: 3px solid var(--primary);
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
            display: inline-block;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        #api-guide-modal {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.5);
            z-index: 100;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
    </style>

    <!-- Chosen Palette: 專業深度色系 -->
    <!-- Application Structure Plan: 修正模型調用路徑為穩定的 gemini-1.5-flash，並將語音朗讀功能替換為 Web Speech API 以適應公開環境，解決模型不支援 native audio modality 的報錯問題。 -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
</head>
<body class="min-h-screen">

    <!-- API 申請教學彈窗 -->
    <div id="api-guide-modal">
        <div class="glass-card bg-white rounded-3xl max-w-lg w-full p-8 shadow-2xl relative">
            <button onclick="closeGuide()" class="absolute top-4 right-4 text-slate-400 hover:text-slate-600 text-2xl">✕</button>
            <h3 class="text-2xl font-bold text-slate-800 mb-6 flex items-center gap-2">🔌 如何領取 AI 腦袋？</h3>
            <div class="space-y-6 text-slate-600 text-sm">
                <div class="flex gap-4">
                    <div class="w-8 h-8 bg-teal-600 text-white rounded-full flex-shrink-0 flex items-center justify-center font-bold">1</div>
                    <p>前往 <a href="https://aistudio.google.com/app/apikey" target="_blank" class="text-teal-600 underline font-bold">Google AI Studio</a> 登入。</p>
                </div>
                <div class="flex gap-4">
                    <div class="w-8 h-8 bg-teal-600 text-white rounded-full flex-shrink-0 flex items-center justify-center font-bold">2</div>
                    <p>點擊 <strong>"Get API key"</strong>，再點擊 <strong>"Create API key in new project"</strong>。</p>
                </div>
                <div class="flex gap-4">
                    <div class="w-8 h-8 bg-teal-600 text-white rounded-full flex-shrink-0 flex items-center justify-center font-bold">3</div>
                    <p>點擊 <strong>"Copy"</strong> 複製那串英文數字，回到本頁面貼上儲存即可！</p>
                </div>
                <div class="pt-4 border-t border-slate-100 flex justify-center text-xs text-slate-400 text-center">
                    (完全免費，僅需 Google 帳號即可申請)
                </div>
            </div>
        </div>
    </div>

    <header class="sticky top-0 z-50 px-4 py-3">
        <nav class="max-w-5xl mx-auto glass-card rounded-2xl px-6 py-2 flex items-center justify-between">
            <div class="flex items-center gap-2">
                <span class="text-2xl">🏆</span>
                <span class="font-bold text-slate-800 tracking-tight hidden sm:inline">數位行銷四部曲</span>
            </div>
            <div class="flex gap-1 overflow-x-auto no-scrollbar py-1">
                <button onclick="switchTab('intro')" id="btn-intro" class="nav-link active px-3 py-2 text-sm text-slate-500 hover:text-teal-600">理念</button>
                <button onclick="switchTab('step1')" id="btn-step1" class="nav-link px-3 py-2 text-sm text-slate-500 hover:text-teal-600">1.平台</button>
                <button onclick="switchTab('step2')" id="btn-step2" class="nav-link px-3 py-2 text-sm text-slate-500 hover:text-teal-600">2.人設</button>
                <button onclick="switchTab('step3')" id="btn-step3" class="nav-link px-3 py-2 text-sm text-slate-500 hover:text-teal-600">3.賽道</button>
                <button onclick="switchTab('step4')" id="btn-step4" class="nav-link px-3 py-2 text-sm text-slate-500 hover:text-teal-600">4.AI工具</button>
            </div>
        </nav>
    </header>

    <main class="max-w-5xl mx-auto px-4 py-8 md:py-12">
        
        <section id="intro" class="tab-content active">
            <div class="text-center mb-12">
                <span class="bg-teal-100 text-teal-700 px-3 py-1 rounded-full text-xs font-bold tracking-widest uppercase mb-4 inline-block">Digital AI Coach</span>
                <h1 class="text-4xl md:text-5xl font-extrabold text-slate-900 leading-tight">
                    數位轉型，<br><span class="text-teal-600">讓 AI 成為您的專屬助理</span>
                </h1>
                <p class="text-lg text-slate-600 mt-6 max-w-2xl mx-auto">
                    這份指南針對 Google 公開伺服器進行了優化，請在第四步輸入您的金鑰以開啟「文案生成」功能。
                </p>
                <button onclick="switchTab('step1')" class="mt-8 bg-teal-600 hover:bg-teal-700 text-white px-10 py-4 rounded-2xl font-bold transition-all shadow-xl shadow-teal-100">
                    立即開啟之旅
                </button>
            </div>
        </section>

        <section id="step1" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第一步：認識社群平台</h2>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-10 items-center mt-8">
                    <div class="glass-card p-6 rounded-3xl">
                        <div class="chart-container">
                            <canvas id="platformChart"></canvas>
                        </div>
                    </div>
                    <div class="space-y-4 text-left text-sm">
                        <div class="glass-card p-5 rounded-2xl border-l-8 border-blue-600">
                            <h4 class="font-bold text-blue-800">Facebook: 最強擴散力</h4>
                        </div>
                        <div class="glass-card p-5 rounded-2xl border-l-8 border-purple-600">
                            <h4 class="font-bold text-purple-800">Instagram: 生活形象感</h4>
                        </div>
                        <div class="glass-card p-5 rounded-2xl border-l-8 border-green-600">
                            <h4 class="font-bold text-green-800">LINE@: 深度信任關係</h4>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="step2" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第二步：建立自己的人設</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mt-8">
                    <div id="p-newborn" onclick="selectPersona('newborn')" class="persona-card glass-card p-8 rounded-3xl text-center cursor-pointer transition-all hover:shadow-xl">
                        <div class="text-5xl mb-4">👶</div>
                        <h3 class="text-xl font-bold text-slate-800">新生兒保單專家</h3>
                    </div>
                    <div id="p-asset" onclick="selectPersona('asset')" class="persona-card glass-card p-8 rounded-3xl text-center cursor-pointer transition-all hover:shadow-xl">
                        <div class="text-5xl mb-4">🏰</div>
                        <h3 class="text-xl font-bold text-slate-800">資產傳承專家</h3>
                    </div>
                    <div id="p-saving" onclick="selectPersona('saving')" class="persona-card glass-card p-8 rounded-3xl text-center cursor-pointer transition-all hover:shadow-xl">
                        <div class="text-5xl mb-4">💰</div>
                        <h3 class="text-xl font-bold text-slate-800">小資存錢達人</h3>
                    </div>
                </div>
                <div id="persona-detail" class="mt-10 glass-card p-8 rounded-3xl border-t-4 border-teal-500 hidden text-left text-sm"></div>
            </div>
        </section>

        <section id="step3" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第三步：選擇自己的賽道</h2>
                <div class="space-y-4 mt-8">
                    <div class="glass-card p-6 rounded-2xl text-left border-l-4 border-teal-500">
                        <h3 class="font-bold">圖文 (低門檻)</h3>
                        <p class="text-xs text-slate-500">FB/IG 靜態貼文，最穩定。</p>
                    </div>
                    <div class="glass-card p-6 rounded-2xl text-left border-l-4 border-amber-500">
                        <h3 class="font-bold">短影片 (高流量)</h3>
                        <p class="text-xs text-slate-500">Reels/Shorts，目前紅利最高。</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="step4" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第四步：AI 實戰演練助手</h2>
                <p class="text-slate-500 mt-2">使用 Gemini 1.5 官方模型，為您打造爆款文案</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
                <div class="lg:col-span-5 space-y-6">
                    <div class="glass-card p-6 rounded-3xl border-2 border-teal-100 shadow-inner">
                        <h3 class="text-lg font-bold text-slate-800 mb-2">🔑 AI 金鑰設定</h3>
                        <div class="space-y-3">
                            <input id="api-key-input" type="password" placeholder="貼上您的 Gemini API Key" class="w-full px-4 py-2 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-teal-500 text-sm">
                            <div class="flex gap-2">
                                <button onclick="saveApiKey()" class="flex-1 bg-slate-800 text-white py-2 rounded-xl text-xs font-bold hover:bg-slate-700 transition-all">儲存金鑰</button>
                                <button onclick="clearApiKey()" class="bg-red-50 text-red-600 px-4 py-2 rounded-xl text-xs font-bold hover:bg-red-100 transition-all">清除</button>
                            </div>
                            <button onclick="openGuide()" class="w-full text-center text-teal-600 text-xs font-bold underline hover:text-teal-700 mt-2">💡 找不到金鑰？點我查看免費領取教學</button>
                        </div>
                    </div>

                    <div class="glass-card p-6 rounded-3xl">
                        <h3 class="text-lg font-bold text-slate-800 mb-4">✨ AI 貼文生成</h3>
                        <div class="space-y-4">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-2">發文主題</label>
                                <input id="ai-topic" type="text" placeholder="例如：新生兒醫療險的重要性..." class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-teal-500 outline-none text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-2">風格語氣</label>
                                <select id="ai-tone" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-teal-500 outline-none text-sm">
                                    <option value="溫暖且專業">溫暖且專業</option>
                                    <option value="親切好懂">親切好懂</option>
                                    <option value="分析精闢">分析精闢</option>
                                </select>
                            </div>
                            <button id="btn-generate" onclick="generateAIContent()" class="w-full bg-teal-600 hover:bg-teal-700 text-white font-bold py-4 rounded-xl transition-all shadow-lg flex items-center justify-center gap-2">
                                ✨ 讓教練幫我寫文案
                            </button>
                        </div>
                    </div>
                </div>

                <div class="lg:col-span-7">
                    <div id="ai-result-card" class="glass-card p-8 rounded-3xl min-h-[450px] flex flex-col relative border-2 border-dashed border-slate-200">
                        <div id="ai-placeholder" class="m-auto text-center text-slate-400">
                            <div class="text-6xl mb-4">🤖</div>
                            <p>完成金鑰設定後<br>點擊按鈕即可生成</p>
                        </div>
                        
                        <div id="ai-content-display" class="hidden h-full flex flex-col">
                            <div class="flex items-center justify-between mb-4 pb-4 border-b border-slate-100">
                                <span class="bg-teal-100 text-teal-700 px-3 py-1 rounded-full text-[10px] font-bold">Stable Mode (1.5 Flash)</span>
                                <div class="flex gap-2">
                                    <button onclick="playTTS()" id="btn-play-tts" class="p-2 bg-slate-100 hover:bg-slate-200 rounded-lg text-slate-600" title="語音朗讀">🔊</button>
                                    <button onclick="copyToClipboard()" class="p-2 bg-slate-100 hover:bg-slate-200 rounded-lg text-slate-600" title="複製文案">📋</button>
                                </div>
                            </div>
                            <div id="ai-output-text" class="text-slate-700 leading-relaxed whitespace-pre-wrap flex-1 overflow-y-auto pr-2 custom-scrollbar text-sm"></div>
                            <div id="ai-loading" class="hidden absolute inset-0 bg-white/95 flex flex-col items-center justify-center z-10 rounded-3xl">
                                <div class="loading-spinner mb-4"></div>
                                <p class="text-teal-700 font-bold">AI 行銷教練正在幫您動腦...</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <script>
        let selectedPersona = null;
        let lastGeneratedText = "";
        let currentApiKey = localStorage.getItem('gemini_api_key') || "";

        window.onload = () => {
            if (currentApiKey) {
                document.getElementById('api-key-input').value = "********";
                checkApiKeyStatus();
            }
        };

        function openGuide() { document.getElementById('api-guide-modal').style.display = 'flex'; }
        function closeGuide() { document.getElementById('api-guide-modal').style.display = 'none'; }

        function saveApiKey() {
            const val = document.getElementById('api-key-input').value;
            if (!val || val === "********") return alert("請輸入有效的金鑰");
            currentApiKey = val;
            localStorage.setItem('gemini_api_key', val);
            document.getElementById('api-key-input').value = "********";
            alert("金鑰已設定！現在可以使用 AI 了。");
            checkApiKeyStatus();
        }

        function clearApiKey() {
            localStorage.removeItem('gemini_api_key');
            currentApiKey = "";
            document.getElementById('api-key-input').value = "";
            checkApiKeyStatus();
        }

        function checkApiKeyStatus() {
            const placeholder = document.getElementById('ai-placeholder');
            placeholder.innerHTML = currentApiKey ? `<div class="text-6xl mb-4">📝</div><p>金鑰就緒！<br>請輸入主題並點擊生成</p>` : `<div class="text-6xl mb-4">🔑</div><p>請先設定金鑰<br>才能啟動 AI 助理</p>`;
        }

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-link').forEach(el => el.classList.remove('active'));
            document.getElementById(tabId).classList.add('active');
            document.getElementById('btn-' + tabId).classList.add('active');
            if (tabId === 'step1' && !window.myRadarChart) initRadarChart();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function initRadarChart() {
            const ctx = document.getElementById('platformChart').getContext('2d');
            window.myRadarChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: ['新客開發', '信任建立', '深度服務', '品牌擴散', '內容持久'],
                    datasets: [
                        { label: 'FB', data: [5, 4, 2, 5, 3], borderColor: '#2563EB', backgroundColor: 'rgba(37, 99, 235, 0.1)' },
                        { label: 'LINE', data: [2, 5, 5, 1, 1], borderColor: '#16A34A', backgroundColor: 'rgba(22, 163, 74, 0.1)' }
                    ]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        const personaData = {
            newborn: { title: "新生兒保單專家", desc: "目標：新手爸媽。重點：解決育兒焦慮，打造第一份防護網。" },
            asset: { title: "資產傳承專家", desc: "目標：高資產族群。重點：遺贈規劃、稅務節省。" },
            saving: { title: "小資存錢達人", desc: "目標：新鮮人。重點：低門檻高保障、聰明消費。" }
        };

        function selectPersona(type) {
            selectedPersona = type;
            document.querySelectorAll('.persona-card').forEach(c => c.classList.remove('selected'));
            document.getElementById('p-' + type).classList.add('selected');
            const detail = document.getElementById('persona-detail');
            detail.innerHTML = `<h3 class="font-bold text-teal-700">${personaData[type].title}</h3><p class="mt-1">${personaData[type].desc}</p>`;
            detail.classList.remove('hidden');
        }

        // 核心 AI 文字生成 (使用 v1beta 的 1.5-flash)
        async function callGemini(userQuery, systemPrompt) {
            if (!currentApiKey) throw new Error("尚未設定 API 金鑰");
            // 使用最穩定的 1.5-flash 型號路徑
            const endpoint = `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${currentApiKey}`;
            
            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] }
            };

            const response = await fetch(endpoint, { method: 'POST', body: JSON.stringify(payload) });
            const result = await response.json();
            
            if (result.error) throw new Error(result.error.message);
            return result.candidates?.[0]?.content?.parts?.[0]?.text;
        }

        async function generateAIContent() {
            if (!currentApiKey) return alert("請先設定金鑰！");
            if (!selectedPersona) return alert("請先在第二步選擇一個標籤！");

            const topic = document.getElementById('ai-topic').value || "保險的重要性";
            const tone = document.getElementById('ai-tone').value;

            document.getElementById('ai-placeholder').classList.add('hidden');
            document.getElementById('ai-content-display').classList.remove('hidden');
            document.getElementById('ai-loading').classList.remove('hidden');

            const system = `你是保險專家，身份為「${personaData[selectedPersona].title}」。請撰寫一篇關於「${topic}」的社群貼文，語氣為「${tone}」。請使用繁體中文，加入表情符號。`;

            try {
                const text = await callGemini(`寫一篇關於${topic}的貼文`, system);
                lastGeneratedText = text;
                document.getElementById('ai-output-text').innerText = text;
            } catch (err) {
                document.getElementById('ai-output-text').innerText = "❌ 錯誤原因：" + err.message + "\n\n(提示：請檢查金鑰是否正確，或是否在 Google AI Studio 成功建立 Project)";
            } finally {
                document.getElementById('ai-loading').classList.add('hidden');
            }
        }

        // --- 語音朗讀大調整：使用瀏覽器內建 Web Speech API (不需金鑰，更穩定) ---
        function playTTS() {
            if (!lastGeneratedText) return;
            
            // 先停止正在說的話
            window.speechSynthesis.cancel();

            const utterance = new SpeechSynthesisUtterance(lastGeneratedText);
            utterance.lang = 'zh-TW';
            utterance.rate = 1.0;
            utterance.pitch = 1.0;
            
            // 找到比較自然的台灣聲音（如果有的話）
            const voices = window.speechSynthesis.getVoices();
            const preferredVoice = voices.find(v => v.lang.includes('zh-TW') || v.name.includes('Google 國語'));
            if (preferredVoice) utterance.voice = preferredVoice;

            window.speechSynthesis.speak(utterance);
        }

        function copyToClipboard() {
            const el = document.createElement('textarea');
            el.value = document.getElementById('ai-output-text').innerText;
            document.body.appendChild(el); el.select(); document.execCommand('copy'); document.body.removeChild(el);
            alert("文案已複製！");
        }

        // 初始化語音（確保部分瀏覽器載入聲音列表）
        window.speechSynthesis.onvoiceschanged = () => { window.speechSynthesis.getVoices(); };
    </script>
</body>
</html>
