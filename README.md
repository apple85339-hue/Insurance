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
    </style>

    <!-- Chosen Palette: 專業深度色系 -->
    <!-- Application Structure Plan: 在 Step 4 整合 API Key 管理介面，確保 GitHub Pages 部署後的安全性。使用者需輸入自己的金鑰才能啟用 AI 功能，金鑰儲存於瀏覽器 localStorage。 -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
</head>
<body class="min-h-screen">

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
        
        <!-- Intro -->
        <section id="intro" class="tab-content active">
            <div class="text-center mb-12">
                <span class="bg-teal-100 text-teal-700 px-3 py-1 rounded-full text-xs font-bold tracking-widest uppercase mb-4 inline-block">Digital AI Coach</span>
                <h1 class="text-4xl md:text-5xl font-extrabold text-slate-900 leading-tight">
                    數位工具不是取代你，<br><span class="text-teal-600">是讓你擁有 AI 大腦</span>
                </h1>
                <p class="text-lg text-slate-600 mt-6 max-w-2xl mx-auto">
                    這份指南整合了最新的 Gemini AI 技術。請注意：為了保護隱私，在 GitHub 上使用時需填入您自己的 API 金鑰，系統會安全地儲存在您的瀏覽器中。
                </p>
                <button onclick="switchTab('step1')" class="mt-8 bg-teal-600 hover:bg-teal-700 text-white px-10 py-4 rounded-2xl font-bold transition-all shadow-xl shadow-teal-100">
                    立即開始體驗
                </button>
            </div>
        </section>

        <!-- Step 1: 認識社群平台 -->
        <section id="step1" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第一步：認識社群平台</h2>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-10 items-center mt-8">
                    <div class="glass-card p-6 rounded-3xl">
                        <div class="chart-container">
                            <canvas id="platformChart"></canvas>
                        </div>
                    </div>
                    <div class="space-y-4 text-left">
                        <div class="glass-card p-5 rounded-2xl border-l-8 border-blue-600">
                            <h4 class="font-bold text-blue-800 flex items-center gap-2">📱 Facebook (臉書)</h4>
                            <p class="text-sm text-slate-600 mt-1">擴散力最強，適合長文分享。</p>
                        </div>
                        <div class="glass-card p-5 rounded-2xl border-l-8 border-purple-600">
                            <h4 class="font-bold text-purple-800 flex items-center gap-2">📸 Instagram (IG)</h4>
                            <p class="text-sm text-slate-600 mt-1">視覺導向，適合質感生活與短影音。</p>
                        </div>
                        <div class="glass-card p-5 rounded-2xl border-l-8 border-green-600">
                            <h4 class="font-bold text-green-800 flex items-center gap-2">💬 LINE@ (官方帳號)</h4>
                            <p class="text-sm text-slate-600 mt-1">深度服務，最強的客戶信任連結。</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Step 2: 建立自己的人設 -->
        <section id="step2" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第二步：建立自己的人設</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mt-8">
                    <div id="p-newborn" onclick="selectPersona('newborn')" class="persona-card glass-card p-8 rounded-3xl text-center cursor-pointer transition-all hover:shadow-xl">
                        <div class="text-5xl mb-4">👶</div>
                        <h3 class="text-xl font-bold text-slate-800 mb-2">新生兒保單專家</h3>
                    </div>
                    <div id="p-asset" onclick="selectPersona('asset')" class="persona-card glass-card p-8 rounded-3xl text-center cursor-pointer transition-all hover:shadow-xl">
                        <div class="text-5xl mb-4">🏰</div>
                        <h3 class="text-xl font-bold text-slate-800 mb-2">資產傳承專家</h3>
                    </div>
                    <div id="p-saving" onclick="selectPersona('saving')" class="persona-card glass-card p-8 rounded-3xl text-center cursor-pointer transition-all hover:shadow-xl">
                        <div class="text-5xl mb-4">💰</div>
                        <h3 class="text-xl font-bold text-slate-800 mb-2">小資存錢達人</h3>
                    </div>
                </div>
                <div id="persona-detail" class="mt-10 glass-card p-8 rounded-3xl border-t-4 border-teal-500 hidden text-left"></div>
            </div>
        </section>

        <!-- Step 3: 選擇自己的賽道 -->
        <section id="step3" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第三步：選擇自己的賽道</h2>
                <div class="space-y-6 mt-8 text-left">
                    <div class="glass-card p-6 rounded-3xl flex items-center gap-6 group hover:shadow-lg transition-all">
                        <div class="text-3xl">🎨</div>
                        <div>
                            <h3 class="text-xl font-bold text-slate-800">圖文賽道</h3>
                            <p class="text-sm text-slate-600">最穩定的基礎，適合深耕專業知識。</p>
                        </div>
                    </div>
                    <div class="glass-card p-6 rounded-3xl flex items-center gap-6 group hover:shadow-lg transition-all">
                        <div class="text-3xl">🎬</div>
                        <div>
                            <h3 class="text-xl font-bold text-slate-800">影片賽道</h3>
                            <p class="text-sm text-slate-600">流量紅利期，適合建立活生生的個人感。</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Step 4: AI 數位工具輔助 -->
        <section id="step4" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第四步：AI 實戰演練助手</h2>
                <p class="text-slate-500 mt-2">請先完成 API 金鑰設定，即可啟動您的 AI 教練</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
                <!-- 左側：設定與輸入 -->
                <div class="lg:col-span-5 space-y-6">
                    <!-- API Key 設定區 (重要！) -->
                    <div class="glass-card p-6 rounded-3xl border-2 border-teal-100">
                        <h3 class="text-xl font-bold text-slate-800 mb-2 flex items-center gap-2">🔑 金鑰管理</h3>
                        <p class="text-xs text-slate-500 mb-4">金鑰僅儲存於您的瀏覽器，不會傳送至 GitHub 伺服器。</p>
                        <div class="space-y-3">
                            <input id="api-key-input" type="password" placeholder="貼上您的 Gemini API Key" class="w-full px-4 py-2 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-teal-500 text-sm">
                            <div class="flex gap-2">
                                <button onclick="saveApiKey()" class="flex-1 bg-slate-800 text-white py-2 rounded-xl text-xs font-bold hover:bg-slate-700 transition-all">儲存金鑰</button>
                                <button onclick="clearApiKey()" class="bg-red-50 text-red-600 px-4 py-2 rounded-xl text-xs font-bold hover:bg-red-100 transition-all">清除</button>
                            </div>
                            <p class="text-[10px] text-slate-400">尚未取得金鑰？<a href="https://aistudio.google.com/app/apikey" target="_blank" class="text-teal-600 underline">前往 Google AI Studio 免費申請</a></p>
                        </div>
                    </div>

                    <!-- AI 生成輸入 -->
                    <div class="glass-card p-6 rounded-3xl">
                        <h3 class="text-xl font-bold text-slate-800 mb-4">✨ 貼文生成器</h3>
                        <div class="space-y-4">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-2">今天想聊的主題？</label>
                                <input id="ai-topic" type="text" placeholder="例如：新生兒首張保單怎麼選..." class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-teal-500 outline-none text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-2">語氣</label>
                                <select id="ai-tone" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-teal-500 outline-none text-sm">
                                    <option value="溫暖且專業">溫暖且專業</option>
                                    <option value="親切好懂">親切好懂</option>
                                    <option value="具備權威感">具備權威感</option>
                                </select>
                            </div>
                            <button id="btn-generate" onclick="generateAIContent()" class="w-full bg-teal-600 hover:bg-teal-700 text-white font-bold py-4 rounded-xl transition-all shadow-lg flex items-center justify-center gap-2">
                                ✨ 生成文案
                            </button>
                        </div>
                    </div>
                </div>

                <!-- 右側：結果輸出 -->
                <div class="lg:col-span-7">
                    <div id="ai-result-card" class="glass-card p-8 rounded-3xl min-h-[450px] flex flex-col relative border-2 border-dashed border-slate-200">
                        <div id="ai-placeholder" class="m-auto text-center text-slate-400">
                            <div class="text-6xl mb-4">🤖</div>
                            <p>完成左側「金鑰設定」並輸入主題<br>即可開始撰寫文案</p>
                        </div>
                        
                        <div id="ai-content-display" class="hidden h-full flex flex-col">
                            <div class="flex items-center justify-between mb-4 pb-4 border-b border-slate-100">
                                <span class="bg-teal-100 text-teal-700 px-3 py-1 rounded-full text-xs font-bold italic">Generated by Gemini</span>
                                <div class="flex gap-2">
                                    <button onclick="playTTS()" id="btn-play-tts" class="p-2 bg-slate-100 hover:bg-slate-200 rounded-lg text-slate-600">🔊</button>
                                    <button onclick="copyToClipboard()" class="p-2 bg-slate-100 hover:bg-slate-200 rounded-lg text-slate-600">📋</button>
                                </div>
                            </div>
                            <div id="ai-output-text" class="text-slate-700 leading-relaxed whitespace-pre-wrap flex-1 overflow-y-auto pr-2 custom-scrollbar text-sm"></div>
                            <div id="ai-loading" class="hidden absolute inset-0 bg-white/90 flex flex-col items-center justify-center z-10 rounded-3xl">
                                <div class="loading-spinner mb-4"></div>
                                <p class="text-teal-700 font-bold">教練正在思考文案...</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <script>
        // --- 變數與初始化 ---
        let selectedPersona = null;
        let lastGeneratedText = "";
        let currentApiKey = localStorage.getItem('gemini_api_key') || "";

        window.onload = () => {
            if (currentApiKey) {
                document.getElementById('api-key-input').value = "********";
                checkApiKeyStatus();
            }
        };

        // --- 金鑰管理 ---
        function saveApiKey() {
            const val = document.getElementById('api-key-input').value;
            if (!val || val === "********") return alert("請輸入有效的金鑰");
            currentApiKey = val;
            localStorage.setItem('gemini_api_key', val);
            document.getElementById('api-key-input').value = "********";
            alert("金鑰已儲存於本地瀏覽器！");
            checkApiKeyStatus();
        }

        function clearApiKey() {
            localStorage.removeItem('gemini_api_key');
            currentApiKey = "";
            document.getElementById('api-key-input').value = "";
            alert("金鑰已清除");
            checkApiKeyStatus();
        }

        function checkApiKeyStatus() {
            const placeholder = document.getElementById('ai-placeholder');
            if (currentApiKey) {
                placeholder.innerHTML = `<div class="text-6xl mb-4">📝</div><p>金鑰已就緒！<br>點擊「生成文案」開始練習</p>`;
            } else {
                placeholder.innerHTML = `<div class="text-6xl mb-4">🔑</div><p>請先完成「金鑰設定」<br>即可開始撰寫文案</p>`;
            }
        }

        // --- 導覽與 UI ---
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
                    labels: ['開發新客', '建立信任', '深度服務', '品牌擴散', '內容持久'],
                    datasets: [
                        { label: 'FB', data: [5, 4, 2, 5, 3], borderColor: '#2563EB', backgroundColor: 'rgba(37, 99, 235, 0.2)' },
                        { label: 'LINE', data: [2, 5, 5, 1, 1], borderColor: '#16A34A', backgroundColor: 'rgba(22, 163, 74, 0.2)' }
                    ]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        const personaData = {
            newborn: { title: "新生兒保單專家", desc: "目標：新手爸媽。核心：解決焦慮、建立第一份防護。" },
            asset: { title: "資產傳承專家", desc: "目標：中高資產。核心：稅務節省、財富永續。" },
            saving: { title: "小資存錢達人", desc: "目標：社會新鮮人。核心：高 CP 值、輕鬆存錢。" }
        };

        function selectPersona(type) {
            selectedPersona = type;
            document.querySelectorAll('.persona-card').forEach(c => c.classList.remove('selected'));
            document.getElementById('p-' + type).classList.add('selected');
            const detail = document.getElementById('persona-detail');
            detail.innerHTML = `<h3 class="text-xl font-bold text-slate-800">${personaData[type].title}</h3><p class="text-sm mt-2">${personaData[type].desc}</p>`;
            detail.classList.remove('hidden');
        }

        // --- Gemini API 實作 ---
        async function callGemini(userQuery, systemPrompt) {
            if (!currentApiKey) throw new Error("請先填入 API 金鑰");
            const endpoint = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${currentApiKey}`;
            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] }
            };

            for (let i = 0; i < 3; i++) {
                try {
                    const response = await fetch(endpoint, { method: 'POST', body: JSON.stringify(payload) });
                    const result = await response.json();
                    if (result.error) throw new Error(result.error.message);
                    return result.candidates?.[0]?.content?.parts?.[0]?.text;
                } catch (e) {
                    if (i === 2) throw e;
                    await new Promise(r => setTimeout(r, Math.pow(2, i) * 1000));
                }
            }
        }

        async function generateAIContent() {
            if (!currentApiKey) return alert("請先完成金鑰設定！");
            if (!selectedPersona) return alert("請先在第二步選擇人設！");

            const topic = document.getElementById('ai-topic').value || "保險的價值";
            const tone = document.getElementById('ai-tone').value;

            document.getElementById('ai-placeholder').classList.add('hidden');
            document.getElementById('ai-content-display').classList.remove('hidden');
            document.getElementById('ai-loading').classList.remove('hidden');

            const system = `你是保險行銷專家，人設為「${personaData[selectedPersona].title}」。請撰寫一篇關於「${topic}」的社群貼文。語氣應為「${tone}」。請使用繁體中文，加入表情符號。`;

            try {
                const text = await callGemini(`寫一篇關於${topic}的貼文`, system);
                lastGeneratedText = text;
                document.getElementById('ai-output-text').innerText = text;
            } catch (err) {
                document.getElementById('ai-output-text').innerText = "❌ 錯誤：" + err.message;
            } finally {
                document.getElementById('ai-loading').classList.add('hidden');
            }
        }

        async function playTTS() {
            if (!lastGeneratedText || !currentApiKey) return;
            const btn = document.getElementById('btn-play-tts');
            btn.innerText = "⏳";
            const endpoint = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${currentApiKey}`;
            try {
                const res = await fetch(endpoint, {
                    method: 'POST',
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: "語氣親切地朗讀：" + lastGeneratedText }] }],
                        generationConfig: { responseModalities: ["AUDIO"], speechConfig: { voiceConfig: { prebuiltVoiceConfig: { voiceName: "Kore" } } } }
                    })
                });
                const result = await res.json();
                const base64 = result.candidates[0].content.parts.find(p => p.inlineData).inlineData.data;
                const audioBuffer = await pcmToWav(base64, 24000);
                const audio = new Audio(URL.createObjectURL(new Blob([audioBuffer], { type: 'audio/wav' })));
                audio.play();
            } catch (e) { console.error(e); }
            finally { btn.innerText = "🔊"; }
        }

        async function pcmToWav(base64Data, sampleRate) {
            const binaryString = atob(base64Data);
            const len = binaryString.length;
            const buffer = new ArrayBuffer(len + 44);
            const view = new DataView(buffer);
            const writeString = (offset, string) => { for (let i = 0; i < string.length; i++) view.setUint8(offset + i, string.charCodeAt(i)); };
            writeString(0, 'RIFF'); view.setUint32(4, 36 + len, true); writeString(8, 'WAVE');
            writeString(12, 'fmt '); view.setUint32(16, 16, true); view.setUint16(20, 1, true); view.setUint16(22, 1, true);
            view.setUint32(24, sampleRate, true); view.setUint32(28, sampleRate * 2, true); view.setUint16(32, 2, true); view.setUint16(34, 16, true);
            writeString(36, 'data'); view.setUint32(40, len, true);
            for (let i = 0; i < len; i++) view.setUint8(44 + i, binaryString.charCodeAt(i));
            return buffer;
        }

        function copyToClipboard() {
            const el = document.createElement('textarea');
            el.value = document.getElementById('ai-output-text').innerText;
            document.body.appendChild(el); el.select(); document.execCommand('copy'); document.body.removeChild(el);
            alert("已複製！");
        }
    </script>
</body>
</html>
