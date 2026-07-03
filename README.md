<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>名水と、名作と。 - 愛媛県西条市 特産品ショーケース</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Noto Sans JP', sans-serif;
            background-color: #faf8f5;
            color: #333333;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 450px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 400px;
        }
        .fade-in {
            animation: fadeIn 0.5s ease-out forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .active-tab {
            background-color: #ffffff;
            border-color: #d1d5db;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            transform: scale(1.02);
        }
        .glass-panel {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        .bg-gradient-hero {
            background: linear-gradient(135deg, #e0f2fe 0%, #f3e8ff 50%, #ffedd5 100%);
        }
    </style>
</head>
<body class="antialiased min-h-screen flex flex-col">

    <!-- Chosen Palette: Setouchi Calm (Background: #faf8f5 Warm Neutral, Accents: Soft Blue/Water, Earthy Green, Purple/Eggplant, Orange/Persimmon) -->
    <!-- Application Structure Plan: The SPA is structured as a modern "Interactive Tasting Menu". Instead of reading a static document, users are first introduced to the overarching concept (the water and mountain) via a hero section. Then, they enter an interactive "Showcase" section where they can click on different local products. This updates a central display with detailed text, "modern ways to eat", and a dynamic Chart.js Radar Chart visualizing the product's characteristics (invented based on qualitative descriptions to satisfy visualization requirements). Finally, a filterable "Souvenir Guide" section allows users to view recommendations tailored to their user type (Tourist vs. Local). This non-linear, exploratory structure encourages engagement and makes the qualitative data feel dynamic and discoverable. -->
    <!-- Visualization & Content Choices: 
        1. Hero & Concept -> Goal: Inspire/Set Tone -> Presentation: Gradient background with elegant typography and Unicode icons (🏔️, 💧).
        2. Product Details -> Goal: Inform/Explore -> Presentation: Interactive Tabbed Interface. Justification: Prevents overwhelming the user with text; allows focused reading on one product at a time.
        3. Product Characteristics -> Goal: Compare/Visualize -> Presentation: Chart.js Radar Chart (Scale 1-5 for traits like Sweetness, Tradition, Modernity). Justification: Translates qualitative "sizzle" descriptions into an engaging visual metric, satisfying the Chart.js requirement while avoiding SVG.
        4. Souvenir Guide -> Goal: Organize/Action -> Presentation: Filterable Grid (HTML/Tailwind + JS). Justification: Helps target audiences quickly find relevant information based on their needs. 
        Library used: Chart.js (Canvas). Confirming NO SVG/Mermaid. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <header class="bg-gradient-hero py-20 px-4 sm:px-6 lg:px-8 text-center relative overflow-hidden shadow-sm">
        <div class="max-w-4xl mx-auto relative z-10 fade-in">
            <h1 class="text-3xl md:text-5xl font-bold tracking-tight text-gray-900 mb-6 leading-tight">
                名水と、名作と。<br>
                <span class="text-2xl md:text-4xl text-gray-700 font-medium mt-2 block">石鎚山が育む、西条のごちそう</span>
            </h1>
            <p class="text-lg md:text-xl text-gray-600 max-w-2xl mx-auto font-light">
                ── 水の都、愛媛県西条市から。<br class="md:hidden">ひとくちで心が潤う、特別な食体験をあなたに。
            </p>
        </div>
    </header>

    <main class="flex-grow w-full max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        
        <section class="mb-20 fade-in" style="animation-delay: 0.2s;">
            <div class="glass-panel rounded-3xl p-8 md:p-12 text-center max-w-3xl mx-auto shadow-sm">
                <div class="text-4xl mb-4">🏔️💧</div>
                <h2 class="text-2xl font-bold mb-4 text-gray-800">石鎚山が磨き、水が繋ぐ、いのちの物語。</h2>
                <p class="text-gray-600 leading-relaxed text-left md:text-center">
                    西日本最高峰「石鎚山（いしづちさん）」。その豊かな山々に降り注いだ雨や雪は、何十年もの歳月をかけてゆっくりと地中深くで濾過され、西条の街に「うちぬき」という奇跡の自噴水（名水）となって湧き出ます。<br><br>
                    西条市のすべての美味しさは、この日本一に輝いたおいしい水から始まります。清らかな水、降り注ぐ太陽、そして大地の恵み。日常を少し贅沢にする、西条の「美しい味」をのぞいてみませんか？
                </p>
            </div>
        </section>

        <section class="mb-24" id="showcase-section">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-gray-800 mb-4">西条の特産品ショーケース</h2>
                <p class="text-gray-600">気になる特産品を選択して、魅力と新しい楽しみ方を発見してください。</p>
            </div>

            <div class="flex flex-col lg:flex-row gap-8">
                
                <div class="w-full lg:w-1/3 flex flex-row lg:flex-col gap-4 overflow-x-auto lg:overflow-x-visible pb-4 lg:pb-0 hide-scrollbar" id="product-tabs">
                </div>

                <div class="w-full lg:w-2/3 glass-panel rounded-3xl p-6 md:p-8 shadow-sm min-h-[500px] flex flex-col" id="product-content-area">
                    <div id="product-header" class="mb-6 fade-in">
                        <span id="product-icon" class="text-5xl mb-4 block"></span>
                        <h3 id="product-catch" class="text-xl md:text-2xl font-bold text-gray-800 leading-snug mb-2"></h3>
                    </div>

                    <div class="flex flex-col md:flex-row gap-8 flex-grow fade-in" id="product-body">
                        <div class="w-full md:w-1/2 flex flex-col justify-between">
                            <div>
                                <h4 class="font-bold text-gray-700 mb-2 border-b-2 border-gray-200 pb-1 inline-block">特徴と魅力</h4>
                                <p id="product-desc" class="text-gray-600 leading-relaxed text-sm md:text-base mb-6"></p>
                            </div>
                            
                            <div class="bg-gray-50 rounded-2xl p-5 border border-gray-100">
                                <h4 class="font-bold text-gray-800 mb-3 text-sm tracking-wide">✨ モダンな楽しみ方</h4>
                                <ul id="product-eats" class="space-y-4">
                                </ul>
                            </div>
                        </div>

                        <div class="w-full md:w-1/2 flex flex-col items-center justify-center">
                            <h4 class="font-bold text-gray-700 mb-2 text-center text-sm">フレーバー＆特性プロファイル</h4>
                            <div class="chart-container">
                                <canvas id="flavorChart"></canvas>
                            </div>
                            <p class="text-xs text-gray-400 mt-2 text-center">※各指標は特産品の個性を表すイメージ値です</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section class="mb-16 border-t border-gray-200 pt-16">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-bold text-gray-800 mb-4">ターゲット別 お土産・ギフトガイド</h2>
                <p class="text-gray-600 mb-8">あなたにぴったりの「西条の持ち帰り方」をご提案します。</p>
                
                <div class="inline-flex bg-gray-200 rounded-full p-1 mb-8" role="group">
                    <button id="btn-tourist" class="px-6 py-2 rounded-full text-sm font-medium transition-all duration-300 bg-white text-gray-800 shadow-sm" onclick="filterSouvenirs('tourist')">🧳 観光客向け</button>
                    <button id="btn-local" class="px-6 py-2 rounded-full text-sm font-medium transition-all duration-300 text-gray-500 hover:text-gray-700" onclick="filterSouvenirs('local')">🎁 地元・日常使い</button>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-6 max-w-4xl mx-auto" id="souvenir-grid">
            </div>
        </section>

    </main>

    <footer class="bg-gray-900 text-gray-300 py-10 text-center">
        <p class="text-sm">愛媛県西条市 地域ブランディングプロジェクト</p>
        <p class="text-xs mt-2 text-gray-500">※本サイトは架空の構成案に基づくプロトタイプです。</p>
    </footer>

    <script>
        const productsData = {
            'uchinuki': {
                id: 'uchinuki',
                name: 'うちぬき（名水）',
                icon: '💧',
                color: 'rgba(56, 189, 248, 0.7)', 
                borderColor: 'rgb(56, 189, 248)',
                catch: "澄みきる、潤う。西条のストリートに湧き出る、神様からのギフト。",
                desc: "西条市の街のいたるところからコンコンと湧き出る「うちぬき」。石鎚山系からのおくりものであるこの水は、名水百選の「おいしい水日本一」に何度も輝いた折り紙つきの天然水です。雑味がなく、驚くほどまろやかで、ほんのりとした甘みを感じる口当たり。西条の美しさを体現する、すべての源です。",
                eats: [
                    { title: "マイボトルで名水ホッピング", text: "お気に入りのボトルを片手に、市内に点在する「うちぬき広場」を巡るサステナブルな名水散歩。" },
                    { title: "水出しクラフトコールドブリュー", text: "超軟水の「うちぬき」で一晩じっくり抽出する水出しコーヒー。豆本来のフルーティーな甘みが引き立ちます。" }
                ],
                chartData: [5, 4, 2, 3, 5] 
            },
            'kinukawa': {
                id: 'kinukawa',
                name: 'きぬかわなす',
                icon: '🍆',
                color: 'rgba(168, 85, 247, 0.7)', 
                borderColor: 'rgb(168, 85, 247)',
                catch: "スプーンで食べる、もぎたての絹。驚くほど甘く、とろける「贅沢なナス」。",
                desc: "持った瞬間にわかる、圧倒的な柔らかさ。「絹」のようなキメ細やかな肌と、ナスとは思えないジューシーな甘みが特徴の「きぬかわなす」。西条の清らかな水と温暖な気候だからこそ育つ希少なブランド野菜です。アクが少なく、果汁がジュワッと溢れ出す野菜の芸術品です。",
                eats: [
                    { title: "きぬかわなすのカルパッチョ", text: "薄切り生なすに、トマト、モッツァレラを合わせオリーブオイルと塩で。リンゴのようなシャキシャキ食感。" },
                    { title: "丸ごとステーキ 焦がしバター醤油", text: "厚切りステーキに。中がトロトロのフォアグラのような食感に変化し、ジューシーなごちそうに早変わり。" }
                ],
                chartData: [4, 3, 4, 5, 4] 
            },
            'kaki': {
                id: 'kaki',
                name: '西条柿（愛宕柿）',
                icon: '🧡',
                color: 'rgba(249, 115, 22, 0.7)', 
                borderColor: 'rgb(249, 115, 22)',
                catch: "歴史が磨いた、極上の「和製和菓子」。ひとくちで、秋の黄金色にとろけて。",
                desc: "伊予のお殿様にも愛された「西条柿」。ドライフルーツにすることで、砂糖不使用とは思えない濃厚な甘みともっちり食感が生まれます。大ぶりな「愛宕柿」はじっくり渋抜きし、サクサクとした上品な食感とリッチな甘みを楽しめる冬の風物詩。進化し続ける究極のナチュラルスイーツです。",
                eats: [
                    { title: "干し柿＆ゴルゴンゾーラのピンチョス", text: "もっちり干し柿にブルーチーズとクルミを。ワインやクラフトビールに最適な、大人のスタイリッシュデザート。" },
                    { title: "愛宕柿と生ハムの秋色サラダ", text: "サクサクの愛宕柿スライスと生ハムを合わせ、バルサミコ酢を。おうちパーティの主役になる華やかさです。" }
                ],
                chartData: [2, 5, 5, 4, 3] 
            },
            'kurocha': {
                id: 'kurocha',
                name: '石鎚黒茶',
                icon: '🍵',
                color: 'rgba(132, 204, 22, 0.7)', 
                borderColor: 'rgb(132, 204, 22)',
                catch: "日本で唯一無二、ゴールドに輝く「和のハーブティー」。",
                desc: "日本にわずか4つしか残っていない、伝統製法で作られる希少な「後発酵茶」。手作業で2度の発酵を経て生まれるこのお茶は、美しい琥珀色と、さわやかでフルーティーな酸味が特徴。乳酸菌によるすっきりとした後味で、現代のヘルシーライフにぴったりフィットします。",
                eats: [
                    { title: "レイヤード・黒茶スパークリング", text: "濃いめの黒茶を炭酸水で割り、ジンジャーやミントを添えて。酸味と爽快感が心地よいノンアルコールカクテルに。" },
                    { title: "ボタニカル黒茶ミルクティー", text: "酸味があるため豆乳やオーツミルクと相性抜群。シナモンを振れば、チャイ風のモダンなドリンクが楽しめます。" }
                ],
                chartData: [5, 5, 1, 5, 5] 
            }
        };

        const souvenirsData = [
            {
                type: 'tourist',
                title: '「おうちで西条バル」セット',
                desc: 'きぬかわなすのピクルスや瓶詰め、西条柿とチーズのテリーヌ、石鎚黒茶のティーバッグをセットに。帰宅後の週末に、旅の余韻に浸るプレミアムなひとときを提案します。',
                icon: '🍷'
            },
            {
                type: 'tourist',
                title: '「名水ドリップ」トラベルキット',
                desc: '「うちぬき」のペットボトルと、西条のローカルロースターが焙煎したコーヒーバッグ、西条柿のドライフルーツをワンパッケージに。自宅で西条の朝を完全再現。',
                icon: '☕'
            },
            {
                type: 'local',
                title: '「クラフト黒茶ティーバッグ」ギフト缶',
                desc: 'ポップでスタイリッシュな北欧風デザインの缶に入った石鎚黒茶。オフィスでのティータイムや、美容意識の高い友人へのバースデーギフトに。',
                icon: '🎁'
            },
            {
                type: 'local',
                title: '「週末プチ贅沢」なローカルおうちごはん',
                desc: '直売所の新鮮な野菜をシンプルに贅沢に食べる。日常の小さな幸せをInstagramで「#西条のごちそう」とともにシェアしたくなるライフスタイル提案。',
                icon: '🍽️'
            }
        ];

        let currentChart = null;

        function initApp() {
            renderTabs();
            selectProduct('uchinuki');
            filterSouvenirs('tourist');
        }

        function renderTabs() {
            const tabsContainer = document.getElementById('product-tabs');
            Object.values(productsData).forEach(prod => {
                const btn = document.createElement('button');
                btn.className = `flex-none lg:flex-auto flex items-center justify-start gap-3 p-4 rounded-2xl border-2 border-transparent transition-all duration-300 hover:bg-white/50 text-left w-64 lg:w-full`;
                btn.id = `tab-${prod.id}`;
                btn.onclick = () => selectProduct(prod.id);
                btn.innerHTML = `
                    <span class="text-2xl">${prod.icon}</span>
                    <span class="font-bold text-gray-700">${prod.name}</span>
                `;
                tabsContainer.appendChild(btn);
            });
        }

        function selectProduct(id) {
            Object.keys(productsData).forEach(key => {
                const tab = document.getElementById(`tab-${key}`);
                if(key === id) {
                    tab.classList.add('active-tab');
                    tab.classList.remove('border-transparent');
                } else {
                    tab.classList.remove('active-tab');
                    tab.classList.add('border-transparent');
                }
            });

            const prod = productsData[id];
            
            const header = document.getElementById('product-header');
            const body = document.getElementById('product-body');
            header.classList.remove('fade-in');
            body.classList.remove('fade-in');
            void header.offsetWidth; 
            header.classList.add('fade-in');
            body.classList.add('fade-in');

            document.getElementById('product-icon').textContent = prod.icon;
            document.getElementById('product-catch').textContent = prod.catch;
            document.getElementById('product-desc').textContent = prod.desc;

            const eatsContainer = document.getElementById('product-eats');
            eatsContainer.innerHTML = '';
            prod.eats.forEach(eat => {
                const li = document.createElement('li');
                li.innerHTML = `<strong class="block text-gray-800 text-sm mb-1">${eat.title}</strong><span class="text-xs text-gray-600 block">${eat.text}</span>`;
                eatsContainer.appendChild(li);
            });

            updateChart(prod);
        }

        function updateChart(prod) {
            const ctx = document.getElementById('flavorChart').getContext('2d');
            
            const chartConfig = {
                type: 'radar',
                data: {
                    labels: ['爽快感・リフレッシュ', '伝統・歴史', '甘み・コク', '希少性', 'モダン・洋風アレンジ'],
                    datasets: [{
                        label: prod.name,
                        data: prod.chartData,
                        backgroundColor: prod.color,
                        borderColor: prod.borderColor,
                        pointBackgroundColor: prod.borderColor,
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: prod.borderColor,
                        borderWidth: 2,
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        r: {
                            angleLines: { color: 'rgba(0, 0, 0, 0.1)' },
                            grid: { color: 'rgba(0, 0, 0, 0.1)' },
                            pointLabels: {
                                font: { family: "'Noto Sans JP', sans-serif", size: 10 },
                                color: '#666'
                            },
                            ticks: {
                                display: false,
                                min: 0,
                                max: 5,
                                stepSize: 1
                            }
                        }
                    },
                    plugins: {
                        legend: { display: false },
                        tooltip: {
                            backgroundColor: 'rgba(255, 255, 255, 0.9)',
                            titleColor: '#333',
                            bodyColor: '#666',
                            borderColor: '#e5e7eb',
                            borderWidth: 1,
                            padding: 10,
                            displayColors: false,
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) { label += ': '; }
                                    if (context.parsed.r !== null) {
                                        label += context.parsed.r + ' / 5';
                                    }
                                    return label;
                                }
                            }
                        }
                    }
                }
            };

            if (currentChart) {
                currentChart.destroy();
            }
            currentChart = new Chart(ctx, chartConfig);
        }

        function filterSouvenirs(type) {
            const btnTourist = document.getElementById('btn-tourist');
            const btnLocal = document.getElementById('btn-local');

            if (type === 'tourist') {
                btnTourist.className = 'px-6 py-2 rounded-full text-sm font-medium transition-all duration-300 bg-white text-gray-800 shadow-sm';
                btnLocal.className = 'px-6 py-2 rounded-full text-sm font-medium transition-all duration-300 text-gray-500 hover:text-gray-700 bg-transparent shadow-none';
            } else {
                btnLocal.className = 'px-6 py-2 rounded-full text-sm font-medium transition-all duration-300 bg-white text-gray-800 shadow-sm';
                btnTourist.className = 'px-6 py-2 rounded-full text-sm font-medium transition-all duration-300 text-gray-500 hover:text-gray-700 bg-transparent shadow-none';
            }

            const grid = document.getElementById('souvenir-grid');
            grid.innerHTML = '';
            
            const filtered = souvenirsData.filter(item => item.type === type);
            
            filtered.forEach((item, index) => {
                const delay = index * 0.1;
                const card = document.createElement('div');
                card.className = 'glass-panel p-6 rounded-3xl fade-in transition-transform duration-300 hover:-translate-y-1';
                card.style.animationDelay = `${delay}s`;
                card.innerHTML = `
                    <div class="text-4xl mb-3">${item.icon}</div>
                    <h3 class="text-lg font-bold text-gray-800 mb-2">${item.title}</h3>
                    <p class="text-gray-600 text-sm leading-relaxed">${item.desc}</p>
                `;
                grid.appendChild(card);
            });
        }

        document.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
