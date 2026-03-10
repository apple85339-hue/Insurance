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

        /* 載入中動畫 */
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

    <!-- Chosen Palette: 專業深度色系 (Teal-700 核心專業、Slate-50 背景、Amber-500 強調關鍵動作) -->
    <!-- Application Structure Plan: 在原本的四階段導覽基礎上，深化「Step 4: AI 數位工具」的互動性。整合 Gemini 2.5 Flash API 提供即時的文案生成與 TTS 語音朗讀功能。這能讓使用者從「聽說 AI 很強」轉變為「親手嘗試 AI」，藉此大幅降低數位工具的進入門檻。 -->
    <!-- Visualization & Content Choices: 
         1. 平台對比：雷達圖展示。
         2. 人設選擇：互動式卡片。
         3. 賽道潛力：水平清單。
         4. AI 實戰：Gemini API 整合，包含文案生成與語音合成 (TTS)。
         確認：未使用 SVG/Mermaid。 -->
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
                    這份指南整合了最新的 Gemini AI 技術，讓你不再只是學習觀念，更能直接現場生成專業文案。從現在開始，你就是數位時代的保險菁英。
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
                <p class="text-slate-500 mt-2">不同的戰場，需要不同的武器</p>
            </div>
            
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-10 items-center">
                <div class="glass-card p-6 rounded-3xl">
                    <div class="chart-container">
                        <canvas id="platformChart"></canvas>
                    </div>
                </div>
                <div class="space-y-4">
                    <div class="glass-card p-5 rounded-2xl border-l-8 border-blue-600">
                        <h4 class="font-bold text-blue-800 flex items-center gap-2">📱 Facebook (臉書)</h4>
                        <p class="text-sm text-slate-600 mt-1"><strong>強項：</strong>擴散力最強。適合分享專業見解與長文。</p>
                    </div>
                    <div class="glass-card p-5 rounded-2xl border-l-8 border-purple-600">
                        <h4 class="font-bold text-purple-800 flex items-center gap-2">📸 Instagram (IG)</h4>
                        <p class="text-sm text-slate-600 mt-1"><strong>強項：</strong>視覺形象。適合經營個人生活質感與短影音。</p>
                    </div>
                    <div class="glass-card p-5 rounded-2xl border-l-8 border-green-600">
                        <h4 class="font-bold text-green-800 flex items-center gap-2">💬 LINE@ (官方帳號)</h4>
                        <p class="text-sm text-slate-600 mt-1"><strong>強項：</strong>深度服務與信任。適合一對一諮詢與精準關懷。</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Step 2: 建立自己的人設 -->
        <section id="step2" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第二步：建立自己的人設</h2>
                <p class="text-slate-500 mt-2">你是誰？客戶為什麼要找你？</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <div id="p-newborn" onclick="selectPersona('newborn')" class="persona-card glass-card p-8 rounded-3xl text-center cursor-pointer transition-all hover:shadow-xl">
                    <div class="text-5xl mb-4">👶</div>
                    <h3 class="text-xl font-bold text-slate-800 mb-2">新生兒保單專家</h3>
                    <p class="text-xs text-slate-500">針對新手爸媽，提供最周全的第一份保障建議。</p>
                </div>
                <div id="p-asset" onclick="selectPersona('asset')" class="persona-card glass-card p-8 rounded-3xl text-center cursor-pointer transition-all hover:shadow-xl">
                    <div class="text-5xl mb-4">🏰</div>
                    <h3 class="text-xl font-bold text-slate-800 mb-2">資產傳承專家</h3>
                    <p class="text-xs text-slate-500">專精於高資產族群的稅務規劃與財富傳承。</p>
                </div>
                <div id="p-saving" onclick="selectPersona('saving')" class="persona-card glass-card p-8 rounded-3xl text-center cursor-pointer transition-all hover:shadow-xl">
                    <div class="text-5xl mb-4">💰</div>
                    <h3 class="text-xl font-bold text-slate-800 mb-2">小資存錢達人</h3>
                    <p class="text-xs text-slate-500">用最低預算配置最高保障，教年輕人聰明存錢。</p>
                </div>
            </div>

            <div id="persona-detail" class="mt-10 glass-card p-8 rounded-3xl border-t-4 border-teal-500 hidden">
                <!-- 由 JS 動態注入內容 -->
            </div>
        </section>

        <!-- Step 3: 選擇自己的賽道 -->
        <section id="step3" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第三步：選擇自己的賽道</h2>
                <p class="text-slate-500 mt-2">根據你的天賦與體力，選擇最適合的輸出方式</p>
            </div>

            <div class="space-y-6">
                <div class="glass-card p-6 rounded-3xl flex flex-col md:flex-row items-center gap-6 group hover:shadow-lg transition-all">
                    <div class="w-20 h-20 bg-teal-100 rounded-2xl flex items-center justify-center text-3xl group-hover:scale-110 transition-transform">🎨</div>
                    <div class="flex-1">
                        <h3 class="text-xl font-bold text-slate-800">圖文賽道 (FB/IG 貼文)</h3>
                        <p class="text-sm text-slate-600 mt-1">一張圖加上 300 字就能開始。門檻最低，最適合打地基。</p>
                    </div>
                </div>
                <div class="glass-card p-6 rounded-3xl flex flex-col md:flex-row items-center gap-6 group hover:shadow-lg transition-all">
                    <div class="w-20 h-20 bg-amber-100 rounded-2xl flex items-center justify-center text-3xl group-hover:scale-110 transition-transform">🎬</div>
                    <div class="flex-1">
                        <h3 class="text-xl font-bold text-slate-800">影片賽道 (Shorts/Reels)</h3>
                        <p class="text-sm text-slate-600 mt-1">目前最夯的流量密碼。適合敢上鏡、說話有節奏感的人。</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Step 4: AI 數位工具輔助 -->
        <section id="step4" class="tab-content">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-slate-900">第四步：AI 實戰演練助手</h2>
                <p class="text-slate-500 mt-2">別再說不會寫！讓 Gemini 為你現場服務</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
                <!-- 左側：輸入區 -->
                <div class="lg:col-span-5 space-y-6">
                    <div class="glass-card p-6 rounded-3xl">
                        <h3 class="text-xl font-bold text-slate-800 mb-4">✨ AI 內容生成</h3>
                        <p class="text-sm text-slate-500 mb-4">請先完成第二步選擇「人設」，AI 會根據你的身份量身定做內容。</p>
                        
                        <div class="space-y-4">
                            <div>
                                <label class="block text-sm font-bold text-slate-700 mb-2">今天想聊的主題？</label>
                                <input id="ai-topic" type="text" placeholder="例如：為什麼新生兒要買意外險..." class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-teal-500 outline-none">
                            </div>
                            <div>
                                <label class="block text-sm font-bold text-slate-700 mb-2">期望語氣？</label>
                                <select id="ai-tone" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-teal-500 outline-none">
                                    <option value="溫暖且專業">溫暖且專業</option>
                                    <option value="活潑親切">活潑親切</option>
                                    <option value="嚴肅權威">嚴肅權威</option>
                                </select>
                            </div>
                            <button id="btn-generate" onclick="generateAIContent()" class="w-full bg-teal-600 hover:bg-teal-700 text-white font-bold py-4 rounded-xl transition-all shadow-lg flex items-center justify-center gap-2">
                                ✨ 立即生成專業文案
                            </button>
                        </div>
                    </div>
                </div>

                <!-- 右側：結果輸出 -->
                <div class="lg:col-span-7">
                    <div id="ai-result-card" class="glass-card p-8 rounded-3xl min-h-[400px] flex flex-col relative border-2 border-dashed border-slate-200">
                        <div id="ai-placeholder" class="m-auto text-center text-slate-400">
                            <div class="text-6xl mb-4">📝</div>
                            <p>填寫左側資訊並點擊按鈕<br>AI 文案將出現在這裡</p>
                        </div>
                        
                        <div id="ai-content-display" class="hidden h-full flex flex-col">
                            <div class="flex items-center justify-between mb-4 pb-4 border-b border-slate-100">
                                <span class="bg-teal-100 text-teal-700 px-3 py-1 rounded-full text-xs font-bold">✨ AI 生成建議</span>
                                <div class="flex gap-2">
                                    <button onclick="playTTS()" id="btn-play-tts" class="p-2 bg-slate-100 hover:bg-slate-200 rounded-lg text-slate-600 title='語音朗讀'">🔊</button>
                                    <button onclick="copyToClipboard()" class="p-2 bg-slate-100 hover:bg-slate-200 rounded-lg text-slate-600" title='複製內容'>📋</button>
                                </div>
                            </div>
                            <div id="ai-output-text" class="text-slate-700 leading-relaxed whitespace-pre-wrap flex-1 overflow-y-auto pr-2 custom-scrollbar"></div>
                            <div id="ai-loading" class="hidden absolute inset-0 bg-white/80 flex flex-col items-center justify-center z-10 rounded-3xl">
                                <div class="loading-spinner mb-4"></div>
                                <p class="text-teal-700 font-bold">教練助理正在撰寫中...</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <script>
        // --- 全域變數 ---
        let selectedPersona = null;
        let lastGeneratedText = "";
        const apiKey = ""; // 執行環境會自動提供

        // --- 導覽切換 ---
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-link').forEach(el => el.classList.remove('active'));

            document.getElementById(tabId).classList.add('active');
            document.getElementById('btn-' + tabId).classList.add('active');

            if (tabId === 'step1' && !window.myRadarChart) initRadarChart();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // --- Step 1 圖表 ---
        function initRadarChart() {
            const ctx = document.getElementById('platformChart').getContext('2d');
            window.myRadarChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: ['開發新客', '建立信任', '深度服務', '品牌擴散', '內容持久'],
                    datasets: [
                        { label: 'Facebook', data: [5, 4, 2, 5, 3], borderColor: '#2563EB', backgroundColor: 'rgba(37, 99, 235, 0.2)' },
                        { label: 'Instagram', data: [4, 3, 2, 4, 2], borderColor: '#9333EA', backgroundColor: 'rgba(147, 51, 234, 0.2)' },
                        { label: 'LINE@', data: [2, 5, 5, 1, 1], borderColor: '#16A34A', backgroundColor: 'rgba(22, 163, 74, 0.2)' }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: { r: { angleLines: { display: false }, suggestedMin: 0, suggestedMax: 5, ticks: { display: false } } },
                    plugins: { legend: { position: 'bottom' } }
                }
            });
        }

        // --- Step 2 人設選擇 ---
        const personaData = {
            newborn: { title: "新生兒保單專家", desc: "目標：新手爸媽。發文：罐頭保單、理賠案例。" },
            asset: { title: "資產傳承專家", desc: "目標：中高資產族。發文：遺贈稅法規、分紅保單。" },
            saving: { title: "小資存錢達人", desc: "目標：20-30 歲小資。發文：小錢配置大保障、存錢計畫。" }
        };

        function selectPersona(type) {
            selectedPersona = type;
            document.querySelectorAll('.persona-card').forEach(c => c.classList.remove('selected'));
            document.getElementById('p-' + type).classList.add('selected');

            const detail = document.getElementById('persona-detail');
            const data = personaData[type];
            detail.innerHTML = `<h3 class="text-2xl font-bold text-slate-800 mb-3">${data.title} 行動指南</h3><p class="text-slate-600">${data.desc}</p>`;
            detail.classList.remove('hidden');
        }

        // --- Step 4 Gemini API 核心邏輯 ---
        async function callGemini(userQuery, systemPrompt) {
            const endpoint = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] }
            };

            for (let i = 0; i < 5; i++) {
                try {
                    const response = await fetch(endpoint, {
                        method: 'POST',
                        body: JSON.stringify(payload)
                    });
                    const result = await response.json();
                    return result.candidates?.[0]?.content?.parts?.[0]?.text;
                } catch (e) {
                    await new Promise(r => setTimeout(r, Math.pow(2, i) * 1000));
                }
            }
            throw new Error("無法連結 AI 教練助理");
        }

        async function generateAIContent() {
            if (!selectedPersona) {
                alert("請先在第二步選擇你的人設！");
                switchTab('step2');
                return;
            }

            const topic = document.getElementById('ai-topic').value || "分享一份保險的價值";
            const tone = document.getElementById('ai-tone').value;
            const personaName = personaData[selectedPersona].title;

            // 顯示載入狀態
            document.getElementById('ai-placeholder').classList.add('hidden');
            document.getElementById('ai-content-display').classList.remove('hidden');
            document.getElementById('ai-loading').classList.remove('hidden');
            document.getElementById('ai-output-text').innerText = "";

            const systemPrompt = `你是一位資深的保險行銷教練。請根據使用者的人設、主題與語氣，撰寫一篇適合發布在社群平台（如 FB 或 LINE）的貼文。請確保內容溫暖、專業且易於理解，並加入適當的 Emoji。請使用繁體中文回覆。`;
            const userQuery = `我目前的人設是「${personaName}」，我想寫一篇關於「${topic}」的貼文，語氣要「${tone}」。`;

            try {
                const text = await callGemini(userQuery, systemPrompt);
                lastGeneratedText = text;
                document.getElementById('ai-output-text').innerText = text;
                document.getElementById('ai-loading').classList.add('hidden');
                document.getElementById('ai-result-card').classList.remove('border-dashed');
                document.getElementById('ai-result-card').classList.add('border-solid', 'border-teal-100');
            } catch (error) {
                document.getElementById('ai-output-text').innerText = "哎呀！AI 教練好像斷線了，請稍後再試。";
                document.getElementById('ai-loading').classList.add('hidden');
            }
        }

        // --- TTS 功能 (文字轉語音) ---
        async function playTTS() {
            if (!lastGeneratedText) return;
            const btn = document.getElementById('btn-play-tts');
            btn.innerText = "⏳";
            btn.disabled = true;

            const endpoint = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`;
            const payload = {
                contents: [{ parts: [{ text: "說話溫暖且充滿熱忱：" + lastGeneratedText }] }],
                generationConfig: {
                    responseModalities: ["AUDIO"],
                    speechConfig: { voiceConfig: { prebuiltVoiceConfig: { voiceName: "Kore" } } }
                }
            };

            try {
                const response = await fetch(endpoint, { method: 'POST', body: JSON.stringify(payload) });
                const result = await response.json();
                const base64Audio = result.candidates?.[0]?.content?.parts?.find(p => p.inlineData)?.inlineData?.data;
                
                if (base64Audio) {
                    const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                    const audioBuffer = await pcmToWav(base64Audio, 24000); // 預設 24kHz
                    const blob = new Blob([audioBuffer], { type: 'audio/wav' });
                    const url = URL.createObjectURL(blob);
                    const audio = new Audio(url);
                    audio.play();
                }
            } catch (e) {
                console.error("TTS Error:", e);
            } finally {
                btn.innerText = "🔊";
                btn.disabled = false;
            }
        }

        // PCM16 to WAV 轉換函數
        async function pcmToWav(base64Data, sampleRate) {
            const binaryString = atob(base64Data);
            const len = binaryString.length;
            const buffer = new ArrayBuffer(len + 44);
            const view = new DataView(buffer);
            
            // WAV Header
            const writeString = (offset, string) => { for (let i = 0; i < string.length; i++) view.setUint8(offset + i, string.charCodeAt(i)); };
            writeString(0, 'RIFF'); view.setUint32(4, 36 + len, true); writeString(8, 'WAVE');
            writeString(12, 'fmt '); view.setUint32(16, 16, true); view.setUint16(20, 1, true); view.setUint16(22, 1, true);
            view.setUint32(24, sampleRate, true); view.setUint32(28, sampleRate * 2, true); view.setUint16(32, 2, true); view.setUint16(34, 16, true);
            writeString(36, 'data'); view.setUint32(40, len, true);
            
            for (let i = 0; i < len; i++) view.setUint8(44 + i, binaryString.charCodeAt(i));
            return buffer;
        }

        function copyToClipboard() {
            const text = document.getElementById('ai-output-text').innerText;
            const temp = document.createElement('textarea');
            temp.value = text;
            document.body.appendChild(temp);
            temp.select();
            document.execCommand('copy');
            document.body.removeChild(temp);
            alert("文案已複製到剪貼簿！");
        }
    </script>
</body>
</html>
