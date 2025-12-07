<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BAR EPHEMERA</title>
<style>
    /* --- Fonts --- */
    @import url('https://fonts.googleapis.com/css2?family=Shippori+Mincho:wght@400;500;700&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400&display=swap');

    /* --- Reset & Base --- */
    * { box-sizing: border-box; }

    body, html {
        margin: 0 !important;
        padding: 0 !important;
        width: 100%;
        height: 100%;
        background-color: #050505;
        color: #e0d8c8;
        font-family: 'Shippori Mincho', serif !important;
        overflow: hidden; 
    }

    .artifact-bar-wrapper {
        position: fixed;
        top: 0; left: 0; width: 100%; height: 100%;
        display: flex; justify-content: center; align-items: center;
        overflow: hidden; z-index: 0;
    }

    /* --- Backgrounds (Double Buffer for Cross-fade) --- */
    .bar-bg {
        position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: -1;
        background-size: cover; background-position: center;
        opacity: 0; /* Default invisible */
        filter: sepia(0.4) contrast(1.1) blur(3px);
        transition: opacity 2.0s ease-in-out; /* Smooth Cross-fade */
    }
    .bar-bg.visible {
        opacity: 0.3; /* Target opacity */
    }

    .spotlight {
        position: absolute; top: 0; left: 0; width: 100%; height: 100%;
        background: radial-gradient(circle at center, rgba(20,20,20,0.4) 0%, rgba(0,0,0,0.95) 85%);
        z-index: 0; pointer-events: none;
    }
    .noise {
        position: absolute; top: 0; left: 0; width: 100%; height: 100%;
        opacity: 0.06; pointer-events: none; z-index: 1;
        background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyMDAiIGhlaWdodD0iMjAwIj48ZmlsdGVyIGlkPSJnoiPjxmZVR1cmJ1bGVuY2UgdHlwZT0iZnJhY3RhbE5vaXNlIiBiYXNlRnJlcXVlbmN5PSIwLjY1IiBudW1PY3RhdmVzPSIzIiBzdGl0Y2hUaWxlcz0ic3RpdGNoIi8+PC9maWx0ZXI+PHJlY3Qgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgZmlsdGVyPSJ1cmwoI2cpIiBvcGFjaXR5PSIxIi8+PC9zdmc+');
    }

    /* --- Screens --- */
    .screen {
        position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 10;
        display: flex; flex-direction: column; justify-content: center; align-items: center;
        transition: opacity 0.8s ease, visibility 0.8s;
    }
    .hidden { opacity: 0; visibility: hidden; pointer-events: none; }

    /* --- Landing --- */
    #landing-page { z-index: 30; width: 100%; padding: 0 20px; }
    .landing-title {
        font-family: 'Cinzel', serif !important; font-size: clamp(2.5rem, 8vw, 5rem);
        color: #fff; letter-spacing: 0.15em; margin-bottom: 20px;
        text-shadow: 0 0 30px rgba(255,255,255,0.3); text-align: center;
        border-bottom: 1px solid rgba(255,255,255,0.2); padding-bottom: 15px;
        width: 100%; max-width: 800px;
    }
    .landing-subtitle {
        font-size: 0.8rem; letter-spacing: 0.4em; color: #888; margin-bottom: 40px; text-transform: uppercase; text-align: center;
    }
    .landing-text {
        text-align: center; opacity: 0.8; margin-bottom: 50px; font-size: 0.9rem; line-height: 2; width: 100%; max-width: 600px;
    }
    .start-btn {
        padding: 15px 50px; border: 1px solid rgba(255,255,255,0.3);
        background: rgba(255,255,255,0.05); color: #fff;
        font-family: 'Shippori Mincho', serif; font-size: 1.1rem;
        letter-spacing: 0.3em; cursor: pointer; transition: all 0.4s ease;
        text-transform: uppercase; backdrop-filter: blur(5px); text-decoration: none;
    }
    .start-btn:hover {
        background: rgba(255,255,255,0.15); border-color: #fff;
        box-shadow: 0 0 25px rgba(255,255,255,0.15); letter-spacing: 0.5em;
    }

    /* --- Main App --- */
    #main-app { z-index: 20; width: 100%; }
    .fade-wrapper {
        opacity: 0; filter: blur(10px); transform: scale(0.97);
        transition: all 1.5s cubic-bezier(0.22, 1, 0.36, 1);
        display: flex; flex-direction: column; align-items: center; width: 100%; max-width: 600px;
        margin: 0 auto; padding: 0 20px;
    }
    .fade-wrapper.visible { opacity: 1; filter: blur(0); transform: scale(1); }

    #bartender-icon {
        width: 100px; height: 100px; border-radius: 50%; object-fit: cover;
        border: 1px solid rgba(150, 150, 150, 0.4); box-shadow: 0 0 40px rgba(0, 0, 0, 0.7);
        margin-bottom: 10px; filter: brightness(0.7) contrast(1.2) sepia(0.2);
        background-color: #111; display: block; margin-left: auto; margin-right: auto;
    }
    #bartender-name {
        font-size: 0.7rem; color: #888; margin-bottom: 15px;
        letter-spacing: 0.15em; font-family: sans-serif !important; text-transform: uppercase; text-align: center;
    }

    /* 高さ固定のコンテナ（レイアウト揺れ防止） */
    #award-container {
        height: 30px; margin-bottom: 15px; display: flex; align-items: center; justify-content: center;
        width: 100%;
    }
    #award-display {
        font-family: 'Cinzel', serif !important; font-size: 0.8rem; color: #d4af37;
        border: 1px solid #d4af37; padding: 4px 12px;
        letter-spacing: 0.1em; text-transform: uppercase;
        background: rgba(212, 175, 55, 0.05);
        opacity: 0; transition: opacity 0.5s;
    }
    #award-display.show { opacity: 1; }

    .name-group {
        display: flex; flex-direction: column; align-items: center;
        margin-bottom: 30px; width: 100%;
    }
    
    .prefix-text, .suffix-text {
        font-size: 0.9rem; letter-spacing: 0.2em; color: #aaa; text-align: center;
    }
    
    /* 名前エリアの固定化と改行制御 */
    #name-display {
        color: #fff; 
        font-size: clamp(1.8rem, 6vw, 2.8rem); /* レスポンシブフォント */
        line-height: 1.3; 
        margin: 10px 0;
        padding: 0; 
        width: 100%; text-align: center;
        font-weight: 500;
        letter-spacing: 0.05em;
        
        /* 高さ確保 */
        min-height: 1.3em; 
        display: flex; justify-content: center; align-items: center;

        /* 改行の制御 */
        white-space: normal; 
        word-wrap: break-word;
        overflow-wrap: break-word;
    }
    /* PCなど画面が広い時はなるべく1行 */
    @media (min-width: 600px) {
        #name-display {
            white-space: nowrap; 
        }
    }

    #price-display {
        font-family: "Garamond", serif !important; font-size: 1.4rem; color: #c0b283;
        margin-bottom: 40px; font-style: italic; letter-spacing: 0.1em; text-align: center;
        position: relative;
    }
    #price-display::before {
        content: ''; display: block; width: 30px; height: 1px; background: #c0b283; opacity: 0.4;
        margin: 0 auto 15px auto;
    }

    .recipe-card {
        font-size: 0.85rem; color: #999; line-height: 1.8;
        border-top: 1px solid rgba(255,255,255,0.1); padding-top: 20px; margin-top: 0; width: 100%;
        display: flex; flex-wrap: wrap; justify-content: center; gap: 15px 30px; text-align: center;
    }
    .recipe-item { display: flex; flex-direction: column; align-items: center; min-width: 60px; }
    .recipe-item span { color: #555; font-size: 0.65rem; display: block; margin-bottom: 4px; letter-spacing: 0.1em; }
    .recipe-comment { width: 100%; margin-top: 20px; font-style: italic; text-align: center; color: #bbb; font-size: 0.85rem; opacity: 0.8; letter-spacing: 0.1em; }

    .action-bar {
        margin-top: 50px; display: flex; gap: 20px; justify-content: center;
        opacity: 0; transition: opacity 1s 1s;
    }
    .fade-wrapper.visible .action-bar { opacity: 1; }
    .action-btn {
        background: transparent; border: 1px solid #555; color: #888;
        padding: 10px 25px; font-family: sans-serif; font-size: 0.8rem;
        cursor: pointer; transition: all 0.3s; letter-spacing: 0.1em; display: flex; align-items: center; gap: 8px;
    }
    .action-btn:hover { border-color: #fff; color: #fff; background: rgba(255,255,255,0.05); }

    #status {
        position: fixed; bottom: 20px; right: 20px; font-size: 0.7rem;
        color: #444; font-family: sans-serif !important; letter-spacing: 0.1em; z-index: 100;
    }
    #time-bar {
        position: fixed; bottom: 0; left: 0; height: 2px; width: 0%;
        background: #c0b283; opacity: 0.5; z-index: 200;
    }
    
    h1 { display: none !important; }
</style>

<script>document.title = "BAR EPHEMERA";</script>

<div class="artifact-bar-wrapper">
    <div id="bg-layer-1" class="bar-bg"></div>
    <div id="bg-layer-2" class="bar-bg"></div>
    
    <div class="spotlight"></div>
    <div class="noise"></div>
    <div id="time-bar"></div>

    <div id="landing-page" class="screen">
        <div class="landing-title">BAR EPHEMERA</div>
        <div class="landing-subtitle">DIGITAL SPEAKEASY</div>
        <div class="landing-text">
            唯一無二のバーテンダーが<br>
            あなただけのカクテルをお作りします。
        </div>
        <button id="start-btn" class="start-btn">オーダーする</button>
        <div style="margin-top: 20px; font-size: 0.7rem; color: #555;">Preparing...</div>
    </div>

    <div id="main-app" class="screen hidden">
        <div class="fade-wrapper" id="content-wrapper">
            <img id="bartender-icon" src="" alt="Bartender">
            <div id="bartender-name">Connecting...</div>
            
            <div id="award-container">
                <div id="award-display"></div>
            </div>

            <div class="name-group">
                <div class="prefix-text">こちら</div>
                <div id="name-display">Loading...</div>
                <div class="suffix-text">でございます</div>
            </div>
            
            <div id="price-display"></div>

            <div class="recipe-card">
                <div class="recipe-item"><span>BASE</span><b id="recipe-base">...</b></div>
                <div class="recipe-item"><span>STYLE</span><b id="recipe-style">...</b></div>
                <div class="recipe-item"><span>TASTE</span><b id="recipe-taste">...</b></div>
                <div class="recipe-item"><span>ALC.</span><b id="recipe-alc">...</b></div>
                <div class="recipe-comment" id="recipe-comment">...</div>
            </div>

            <div class="action-bar">
                <button class="action-btn" id="share-btn">SHARE</button>
                <button class="action-btn" id="next-btn" style="border-color:#888; color:#ddd;">NEXT ORDER ↻</button>
            </div>
        </div>
    </div>
    <div id="status">System Ready.</div>
</div>

<script>
    // --- Data ---
    const SAFE_WORDS = ["月光", "サイバーパンク", "シュレーディンガー", "モナ・リザ", "ミッドナイト", "レトロ", "メランコリー", "ユートピア", "シンギュラリティ", "パラドックス", "エターナル", "トワイライト", "蜃気楼", "カオス", "ノスタルジア", "リグレット", "アビス"];
    const SAFE_KATAKANA = ["アンドロメダ", "プロトコル", "エデン", "マトリックス", "リリス", "フェニックス", "リフレイン", "シンドローム", "ファントム", "クリスタル", "メトロポリス", "アルカディア", "ラグナロク", "シリウス", "プラズマ", "オメガ"];
    const SAFE_POEMS = ["静寂と再生の味わい。", "失われた時間を求めて。", "深い夜の底で輝く光。", "記憶の片隅に残る香り。"];
    
    const BG_IMAGES = [
        "https://images.unsplash.com/photo-1572116469696-31de0f17cc34?q=80&w=1920&auto=format&fit=crop",
        "https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?q=80&w=1920&auto=format&fit=crop",
        "https://images.unsplash.com/photo-1470337458703-46ad1756a187?q=80&w=1920&auto=format&fit=crop",
        "https://images.unsplash.com/photo-1566417713940-fe7c737a9ef2?q=80&w=1920&auto=format&fit=crop"
    ];

    const BAN_KEYWORDS = ["人物", "生", "没", "氏", "家", "男", "女", "子", "王", "皇", "帝", "将軍", "選手", "歌手", "俳優", "声優", "作家", "政治", "軍人", "学者", "アイドル", "監督", "メンバー", "一覧", "出身", "在住", "年度", "世紀", "年", "月", "日", "地理", "都市", "町村", "県", "市", "区", "町", "村", "駅", "賞", "事件", "性", "セックス", "風俗", "アダルト", "エロ", "ヌード", "淫", "排泄", "糞", "尿", "便", "犯罪", "殺", "死", "暴力", "虐待", "テロ", "戦争", "兵器", "病", "障害", "菌", "毒", "差別"];

    const BASES = ["Gin", "Vodka", "Rum", "Tequila", "Whisky", "Brandy", "Absinthe", "Shochu", "Champagne"];
    const STYLES = ["Short", "Long", "Rock", "Mist", "Frozen", "Straight"];
    const TASTES = ["Dry", "Medium", "Sweet", "Bitter", "Sour", "Spicy", "Refreshing"];

    // --- State ---
    let currentCocktail = null;
    let nextCocktailPromise = null;
    let isProcessing = false;
    let offlineMode = false;
    let autoTimer = null;
    
    // 背景管理用（アクティブなレイヤー番号 1 or 2）
    let activeBgIndex = 1;

    // --- Helpers ---
    const wait = (ms) => new Promise(r => setTimeout(r, ms));
    const randomPick = (arr) => arr[Math.floor(Math.random() * arr.length)];
    const cleanWord = (w) => w.replace(/（.*?）/g, '').replace(/\(.*?\)/g, '').replace(/_/g, ' ').replace(/一覧/g, '').replace(/カテゴリー:/g, '').trim();

    function isValidConcept(pageData) {
        const title = pageData.title;
        if (title.includes("氏") || title.includes("天皇") || /^[0-9]+$/.test(title)) return false;
        if (BAN_KEYWORDS.some(k => title.includes(k))) return false;
        if (!pageData.categories) return true; 
        for (let cat of pageData.categories) {
            if (BAN_KEYWORDS.some(k => cat.title.includes(k))) return false;
        }
        return true;
    }

    // --- Logic ---
    async function fetchWordFromWiki(type = "concept", retry = 0) {
        if (retry > 4) return type === "katakana" ? randomPick(FALLBACK_KATAKANA) : randomPick(SAFE_WORDS);
        if (retry > 0) await wait(300); 

        let generatorParams = "generator=random&grnnamespace=0";
        const url = `https://ja.wikipedia.org/w/api.php?origin=*&action=query&${generatorParams}&grnlimit=20&prop=categories&cllimit=max&format=json`;

        try {
            const controller = new AbortController();
            const timeoutId = setTimeout(() => controller.abort(), 3000);
            const res = await fetch(url, { signal: controller.signal });
            clearTimeout(timeoutId);
            
            const data = await res.json();
            if (!data.query?.pages) throw new Error("No Data");

            const pages = Object.values(data.query.pages);
            for (let page of pages) {
                const title = cleanWord(page.title);
                if (!isValidConcept(page)) continue;
                
                if (type === "katakana") {
                    if (/^[ァ-ヶー・＝=]+$/.test(title) && title.length >= 2 && title.length <= 15) return title;
                } else if (type === "poetic") {
                    if (!/^[0-9]+$/.test(title) && title.length >= 2 && title.length <= 6) return title;
                } else {
                    if (title.length >= 2 && title.length <= 10 && !title.includes(" ")) return title;
                }
            }
            return await fetchWordFromWiki(type, retry + 1);
        } catch (e) {
            offlineMode = true; 
            return type === "katakana" ? randomPick(SAFE_KATAKANA) : randomPick(SAFE_WORDS);
        }
    }

    async function generateCocktailData() {
        let bartender, word1, word2, award, comment;

        try {
            const results = await Promise.all([
                fetchWordFromWiki("katakana"),
                fetchWordFromWiki("concept"),
                fetchWordFromWiki("katakana"),
            ]);
            bartender = results[0];
            word1 = results[1];
            word2 = results[2];
        } catch (e) {
            bartender = randomPick(SAFE_KATAKANA);
            word1 = randomPick(SAFE_WORDS);
            word2 = randomPick(SAFE_KATAKANA);
        }

        if (Math.random() > 0.4) {
            award = null;
        } else {
            const w = await fetchWordFromWiki("katakana");
            const s = randomPick(["Award", "Prize", "Gold", "Cup", "Grand Prix", "Selection"]);
            const y = Math.floor(Math.random() * (2050 - 1980) + 1980);
            award = `${w} ${s} ${y}`;
        }

        if (offlineMode) {
            comment = randomPick(SAFE_POEMS);
        } else {
            const w1 = await fetchWordFromWiki("poetic");
            const w2 = await fetchWordFromWiki("poetic");
            
            // --- 大量追加したバリエーション ---
            const patterns = [
                // --- ベーシック（味・香りの描写） ---
                `「${w1}」と「${w2}」の余韻。`,
                `まるで${w1}のような、${w2}。`,
                `後味に${w1}を感じる、${w2}の香り。`,
                `テーマは「${w1}」。隠し味に${w2}を。`,
                `${w1}の甘さと、${w2}の苦味の調和。`,
                `口に含めば${w1}、喉を通れば${w2}。`,
                `トップノートは${w1}、ラストは${w2}。`,
                `洗練された${w1}に、一滴の${w2}を。`,
                `その味わいは${w1}よりも深く、${w2}よりも淡い。`,
                `${w1}をベースに、${w2}でアクセントを。`,

                // --- アトモスフィア（情景・時間） ---
                `夜明けの${w1}。黄昏の${w2}。`,
                `雨上がりの${w1}と、路地裏の${w2}。`,
                `真夜中の${w1}に溶ける、${w2}の光。`,
                `都会の${w1}を忘れさせる、${w2}の静寂。`,
                `遠い夏の${w1}、近い冬の${w2}。`,
                `グラスの中に浮かぶ${w1}、沈む${w2}。`,
                `かつて見た${w1}の風景。今ここにある${w2}。`,
                `月明かりの下で味わう、${w1}と${w2}。`,
                `騒がしい${w1}から逃れ、${w2}の海へ。`,
                `旅の終わりの${w1}。旅の始まりの${w2}。`,

                // --- コンセプチュアル（哲学・感情） ---
                `失われた${w1}を求めて。${w2}を添えて。`,
                `それは${w1}への問いかけ。答えは${w2}の中に。`,
                `愛という名の${w1}。孤独という名の${w2}。`,
                `完璧な${w1}などない。あるのは${w2}だけ。`,
                `この一杯は、${w1}であり、${w2}でもある。`,
                `記憶の片隅にある${w1}。予感としての${w2}。`,
                `矛盾する${w1}と${w2}の融合。`,
                `破壊的な${w1}。創造的な${w2}。`,
                `儚い${w1}を閉じ込めた、永遠の${w2}。`,
                `純粋な${w1}への憧憬と、${w2}への嫉妬。`,

                // --- ショート＆パンチ（短文・抽象的） ---
                `ただ、${w1}。そして${w2}。`,
                `Re: ${w1} & ${w2}.`,
                `No ${w1}, No ${w2}.`,
                `${w1}・ミーツ・${w2}。`,
                `究極の${w1} vs 至高の${w2}。`,
                `0%の${w1}、100%の${w2}。`,
                `あるいは、${w1}。もしくは、${w2}。`,
                `刹那の${w1}。無限の${w2}。`,
                `禁断の${w1}、約束の${w2}。`,
                `青い${w1}。赤い${w2}。`,

                // --- ストーリーテリング（物語性） ---
                `昔々、ある${w1}が${w2}に出会った。`,
                `彼が残した${w1}。彼女が愛した${w2}。`,
                `その${w1}は、まるで${w2}への手紙のように。`,
                `事件の鍵は${w1}。犯人は${w2}。`,
                `魔法が解けた後の${w1}、夢から覚めた${w2}。`,
                `禁酒法時代の${w1}、現代の${w2}。`,
                `ハードボイルドな${w1}に、${w2}の優しさを。`,
                `裏切りの${w1}、復讐の${w2}。`,
                `天使の${w1}。悪魔の${w2}。`,
                `終わらない${w1}の歌。響き渡る${w2}。`
            ];
            comment = randomPick(patterns);
        }

        const price = `¥${(Math.floor(Math.random() * (35 - 5) + 5) * 100).toLocaleString()}-`;
        const recipe = {
            base: randomPick(BASES),
            style: randomPick(STYLES),
            taste: randomPick(TASTES),
            alc: Math.floor(Math.random() * (45 - 4) + 4) + "%",
            comment: comment
        };

        const faceUrl = `https://xsgames.co/randomusers/avatar.php?g=${Math.random()<0.5?'male':'female'}&v=${new Date().getTime()}`;
        const bgUrl = randomPick(BG_IMAGES);
        
        const img = new Image();
        img.src = faceUrl;
        const bgImg = new Image();
        bgImg.src = bgUrl;

        await Promise.race([
            new Promise(r => img.onload = r),
            wait(3000)
        ]);

        return {
            bartender,
            name: `${word1}の${word2}`,
            award,
            price,
            recipe,
            faceUrl,
            bgUrl
        };
    }

    // --- Main Update ---
    async function serveCocktail() {
        if (isProcessing) return;
        isProcessing = true;
        if (autoTimer) clearTimeout(autoTimer);
        
        const wrapper = document.getElementById('content-wrapper');
        const awardUI = document.getElementById('award-display');
        const timeBar = document.getElementById('time-bar');
        
        // フェードアウト
        wrapper.classList.remove('visible');
        awardUI.classList.remove('show');
        timeBar.style.transition = 'none';
        timeBar.style.width = '0%';
        document.getElementById('status').textContent = "Brewing...";

        await wait(1200);

        try {
            if (!nextCocktailPromise) {
                nextCocktailPromise = generateCocktailData();
            }
            const data = await nextCocktailPromise;
            currentCocktail = data;

            // --- 背景のクロスフェード処理 ---
            const nextBgIndex = activeBgIndex === 1 ? 2 : 1;
            const activeBgLayer = document.getElementById(`bg-layer-${activeBgIndex}`);
            const nextBgLayer = document.getElementById(`bg-layer-${nextBgIndex}`);

            nextBgLayer.style.backgroundImage = `url('${data.bgUrl}')`;
            nextBgLayer.classList.add('visible');
            activeBgLayer.classList.remove('visible');
            activeBgIndex = nextBgIndex;
            // -----------------------------

            document.getElementById('bartender-icon').src = data.faceUrl;
            document.getElementById('bartender-name').textContent = `BARTENDER: ${data.bartender}`;
            document.getElementById('name-display').textContent = data.name;
            document.getElementById('price-display').textContent = data.price;
            
            document.getElementById('recipe-base').textContent = data.recipe.base;
            document.getElementById('recipe-style').textContent = data.recipe.style;
            document.getElementById('recipe-taste').textContent = data.recipe.taste;
            document.getElementById('recipe-alc').textContent = data.recipe.alc;
            document.getElementById('recipe-comment').textContent = data.recipe.comment;

            // アワード表示（中身を変えてフェードイン）
            if (data.award) {
                awardUI.textContent = `★ ${data.award}`;
                setTimeout(() => awardUI.classList.add('show'), 300);
            } else {
                awardUI.textContent = ""; 
            }

            // フェードイン
            wrapper.classList.add('visible');
            document.getElementById('status').textContent = "";
            isProcessing = false;

            // 次回分プリロード
            nextCocktailPromise = generateCocktailData();

            requestAnimationFrame(() => {
                timeBar.style.transition = 'width 20s linear';
                timeBar.style.width = '100%';
            });
            
            autoTimer = setTimeout(() => {
                serveCocktail();
            }, 20000);

        } catch (e) {
            console.error(e);
            nextCocktailPromise = null; 
            setTimeout(serveCocktail, 500); 
            isProcessing = false;
        }
    }

    // --- Events ---
    window.addEventListener('DOMContentLoaded', () => {
        nextCocktailPromise = generateCocktailData();
    });

    document.getElementById('start-btn').addEventListener('click', () => {
        document.getElementById('landing-page').classList.add('hidden');
        document.getElementById('main-app').classList.remove('hidden');
        serveCocktail();
    });

    document.getElementById('next-btn').addEventListener('click', serveCocktail);

    document.getElementById('share-btn').addEventListener('click', () => {
        if (!currentCocktail || !currentCocktail.name) return;
        const text = `BAR EPHEMERAでオーダーしました。\n\n『${currentCocktail.name}』\n${currentCocktail.price}\n\nBase: ${currentCocktail.recipe.base} / Taste: ${currentCocktail.recipe.taste}\nコメント: ${currentCocktail.recipe.comment}\n担当: ${document.getElementById('bartender-name').textContent}\n\n#BarEphemera`;
        const url = `https://twitter.com/intent/tweet?text=${encodeURIComponent(text)}`;
        window.open(url, '_blank');
    });
</script>

<style>
  h1:first-of-type { display: none !important; }
</style>
