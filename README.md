<style>
    /* Google Fonts */
    @import url('https://fonts.googleapis.com/css2?family=Shippori+Mincho:wght@400;500;700&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&display=swap');

    * { box-sizing: border-box; }

    /* ★修正点1: bodyとhtmlの高さをvhで強制確保 */
    body, html {
        margin: 0 !important;
        padding: 0 !important;
        width: 100vw;
        height: 100vh; /* ここ重要 */
        min-height: 100vh;
        background-color: #050505;
        color: #e0d8c8;
        font-family: 'Shippori Mincho', serif !important;
        overflow: hidden; 
    }

    /* ★修正点2: ラッパーも画面いっぱいに広げる */
    .artifact-bar-wrapper {
        position: fixed; /* fixedにして画面に張り付かせる */
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow: hidden;
        z-index: 0;
    }

    /* 背景 */
    .bar-bg {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1; /* ラッパー内の相対順序 */
        background-image: url('https://images.unsplash.com/photo-1572116469696-31de0f17cc34?q=80&w=1920&auto=format&fit=crop'); 
        background-size: cover;
        background-position: center;
        opacity: 0.25; 
        filter: sepia(0.3) contrast(1.1) blur(2px);
    }
    .spotlight {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: radial-gradient(circle at center, rgba(30,30,30,0.3) 0%, rgba(0,0,0,0.95) 90%);
        z-index: 0;
        pointer-events: none;
    }
    .noise {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        opacity: 0.08;
        pointer-events: none;
        z-index: 1;
        background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyMDAiIGhlaWdodD0iMjAwIj48ZmlsdGVyIGlkPSJnoiPjxmZVR1cmJ1bGVuY2UgdHlwZT0iZnJhY3RhbE5vaXNlIiBiYXNlRnJlcXVlbmN5PSIwLjY1IiBudW1PY3RhdmVzPSIzIiBzdGl0Y2hUaWxlcz0ic3RpdGNoIi8+PC9maWx0ZXI+PHJlY3Qgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgZmlsdGVyPSJ1cmwoI2cpIiBvcGFjaXR5PSIxIi8+PC9zdmc+');
    }

    /* 画面切り替え */
    .screen {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: 10;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        transition: opacity 1.0s ease-in-out, visibility 1.0s;
    }
    .hidden {
        opacity: 0;
        visibility: hidden;
        pointer-events: none;
    }

    /* トップページ */
    #landing-page { z-index: 20; width: 100%; }
    
    .landing-title {
        font-family: 'Cinzel', serif !important;
        font-size: clamp(2rem, 6vw, 4rem);
        color: #fff;
        letter-spacing: 0.2em;
        margin-bottom: 30px;
        text-shadow: 0 0 20px rgba(255,255,255,0.3);
        text-align: center;
        width: 100%;
    }
    .landing-text {
        font-size: clamp(0.9rem, 3vw, 1.1rem);
        line-height: 2.0;
        letter-spacing: 0.1em;
        margin-bottom: 50px;
        text-align: center;
        opacity: 0.9;
        padding: 0 20px;
        width: 100%;
    }
    .start-btn {
        padding: 12px 30px;
        border: 1px solid rgba(255,255,255,0.4);
        background: rgba(0,0,0,0.4);
        color: #fff;
        font-family: 'Shippori Mincho', serif;
        font-size: 1.0rem;
        letter-spacing: 0.2em;
        cursor: pointer;
        transition: all 0.3s ease;
        text-decoration: none;
        display: inline-block;
    }
    .start-btn:active { transform: scale(0.95); }

    /* メイン画面 */
    #main-app-container {
        width: 100%;
        max-width: 600px;
        padding: 0 20px;
        text-align: center;
    }
    .fade-wrapper {
        opacity: 0;
        filter: blur(10px) grayscale(50%);
        transform: scale(0.98);
        transition: all 2.0s cubic-bezier(0.22, 1, 0.36, 1);
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 100%;
    }
    .fade-wrapper.visible {
        opacity: 1;
        filter: blur(0) grayscale(0%);
        transform: scale(1);
    }

    #bartender-icon {
        width: 100px;
        height: 100px;
        border-radius: 50%;
        object-fit: cover;
        border: 1px solid rgba(120, 120, 120, 0.5);
        box-shadow: 0 0 30px rgba(0, 0, 0, 0.8);
        margin-bottom: 10px;
        filter: brightness(0.8) contrast(1.2) sepia(0.2);
        background-color: #111;
        display: block; /* ブロック要素にして崩れ防止 */
        margin-left: auto;
        margin-right: auto;
    }
    #bartender-name {
        font-size: 0.7rem;
        color: #888;
        margin-bottom: 20px;
        letter-spacing: 0.15em;
        font-family: sans-serif !important;
        text-transform: uppercase;
    }
    #award-display {
        font-family: 'Cinzel', serif !important;
        font-size: 0.8rem;
        color: #ffd700;
        border: 1px solid #ffd700;
        padding: 5px 15px;
        margin-bottom: 15px;
        letter-spacing: 0.1em;
        text-transform: uppercase;
        background: rgba(255, 215, 0, 0.05);
        opacity: 0;
        transform: translateY(10px);
        transition: all 1s ease;
        display: none;
        max-width: 100%;
        word-wrap: break-word;
    }
    #award-display.show {
        opacity: 1;
        transform: translateY(0);
        display: inline-block;
    }
    .uniform-text {
        font-size: clamp(1.0rem, 4vw, 1.4rem);
        line-height: 2.0;
        letter-spacing: 0.15em;
        font-weight: 500;
        margin: 5px 0;
        color: #e0e0e0;
        white-space: normal;
        word-wrap: break-word;
    }
    #name-display {
        color: #fff;
        border-bottom: 1px solid rgba(150, 150, 150, 0.3);
        padding-bottom: 15px;
        margin-bottom: 20px;
        margin-top: 10px;
        font-size: clamp(1.6rem, 7vw, 2.8rem); 
        width: 100%;
        line-height: 1.3;
        white-space: normal;
        word-wrap: break-word;
    }
    #price-display {
        font-family: "Garamond", "Times New Roman", serif !important;
        font-size: clamp(1.0rem, 4vw, 1.5rem);
        color: #c0b283; 
        letter-spacing: 0.1em;
        margin-top: 30px; 
        margin-bottom: 10px;
        font-style: italic;
    }
    #status {
        position: fixed;
        bottom: 15px;
        right: 20px;
        font-size: 0.6rem;
        color: #444;
        font-family: sans-serif !important;
        letter-spacing: 0.1em;
        z-index: 20;
    }
</style>

<div class="artifact-bar-wrapper">
    <div class="bar-bg"></div>
    <div class="spotlight"></div>
    <div class="noise"></div>

    <div id="landing-page" class="screen">
        <div class="landing-title">BAR EPHEMERA</div>
        <div class="landing-text">
            唯一無二のバーテンダーが<br>
            あなただけのカクテルをお作りします。
        </div>
        <button id="start-btn" class="start-btn">オーダーする</button>
    </div>

    <div id="main-app" class="screen hidden">
        <div id="main-app-container">
            <div id="content-wrapper" class="fade-wrapper">
                <img id="bartender-icon" src="" alt="">
                <div id="bartender-name">Connecting...</div>

                <div id="award-display"></div>
                
                <div class="uniform-text">こちら</div>
                <div id="name-display" class="uniform-text">Loading...</div>
                <div class="uniform-text">でございます</div>
                
                <div id="price-display"></div>
            </div>
        </div>
        <div id="status">System Ready.</div>
    </div>
</div>

<script>
    const landingPage = document.getElementById('landing-page');
    const mainApp = document.getElementById('main-app');
    const startBtn = document.getElementById('start-btn');
    
    const contentWrapper = document.getElementById('content-wrapper');
    const nameDisplay = document.getElementById('name-display');
    const priceDisplay = document.getElementById('price-display');
    const bartenderIcon = document.getElementById('bartender-icon');
    const bartenderNameDisplay = document.getElementById('bartender-name');
    const awardDisplay = document.getElementById('award-display');
    const statusDisplay = document.getElementById('status');

    let isProcessing = false;

    // スタートボタン処理
    startBtn.addEventListener('click', () => {
        if (isProcessing) return;
        isProcessing = true;
        
        landingPage.classList.add('hidden');
        mainApp.classList.remove('hidden');
        
        // 確実にDOMが描画されてから処理開始
        setTimeout(() => {
            serveCocktail();
        }, 500);
    });

    const banKeywords = ["人物", "生", "没", "氏", "家", "男", "女", "子", "王", "皇", "帝", "将軍", "選手", "歌手", "俳優", "声優", "作家", "政治", "軍人", "学者", "アイドル", "監督", "メンバー", "一覧", "出身", "在住", "年度", "世紀", "年", "月", "日", "地理", "都市", "町村", "県", "市", "区", "町", "村", "駅", "賞", "事件", "性", "セックス", "風俗", "アダルト", "エロ", "ヌード", "淫", "排泄", "糞", "尿", "便", "犯罪", "殺", "死", "暴力", "虐待", "テロ", "戦争", "兵器", "病", "障害", "菌", "毒", "差別"];

    function cleanWord(word) { return word.replace(/（.*?）/g, '').replace(/\(.*?\)/g, '').replace(/_/g, ' ').replace(/一覧/g, '').replace(/カテゴリー:/g, '').trim(); }
    
    function isValidConcept(pageData) {
        const title = pageData.title;
        if (title.includes("氏") || title.includes("天皇") || /^[0-9]+$/.test(title)) return false;
        if (banKeywords.some(keyword => title.includes(keyword))) return false;
        if (!pageData.categories) return true; 
        for (let cat of pageData.categories) {
            if (banKeywords.some(keyword => cat.title.includes(keyword))) return false;
        }
        return true;
    }

    function generatePrice() {
        const min = 5; const max = 30; 
        const price = Math.floor(Math.random() * (max - min + 1) + min) * 100;
        return `¥${price.toLocaleString()}-`;
    }

    function getNewAIFaceUrl() {
        const gender = Math.random() < 0.5 ? 'male' : 'female';
        return `https://xsgames.co/randomusers/avatar.php?g=${gender}&v=${new Date().getTime()}`;
    }

    const wait = (ms) => new Promise(resolve => setTimeout(resolve, ms));

    async function fetchKatakanaWord(retryCount = 0) {
        if (retryCount > 6) return "カオス"; 
        if (retryCount > 0) await wait(300);

        const url = "https://ja.wikipedia.org/w/api.php?origin=*&action=query&generator=random&grnnamespace=0&grnlimit=20&prop=categories&cllimit=max&format=json";
        
        try {
            const res = await fetch(url);
            const data = await res.json();
            if (!data.query || !data.query.pages) return fetchKatakanaWord(retryCount + 1);

            const pages = data.query.pages;
            for (let pageId in pages) {
                const page = pages[pageId];
                const title = cleanWord(page.title);
                if (isValidConcept(page)) {
                    if (/^[ァ-ヶー・＝=]+$/.test(title) && title.length >= 2 && title.length <= 15) {
                        return title;
                    }
                }
            }
            return await fetchKatakanaWord(retryCount + 1);
        } catch (e) { return "ミラージュ"; }
    }

    async function fetchConceptWord(retryCount = 0) {
        if (retryCount > 5) return "無限"; 
        if (retryCount > 0) await wait(300);

        const url = "https://ja.wikipedia.org/w/api.php?origin=*&action=query&generator=random&grnnamespace=0&grnlimit=10&prop=categories&cllimit=max&format=json";
        try {
            const res = await fetch(url);
            const data = await res.json();
            if (!data.query || !data.query.pages) return fetchConceptWord(retryCount + 1);
            const pages = data.query.pages;
            for (let pageId in pages) {
                const page = pages[pageId];
                if (isValidConcept(page)) {
                    let word = cleanWord(page.title);
                    if (word.length >= 2 && word.length <= 8 && !word.includes(" ")) return word;
                }
            }
            return await fetchConceptWord(retryCount + 1);
        } catch (e) { return "虚空"; }
    }

    async function generateAward() {
        if (Math.random() > 0.3) return null;
        const word = await fetchKatakanaWord(); 
        const suffixes = ["Award", "Prize", "Gold Medal", "Selection", "Cup", "Grand Prix"];
        const suffix = suffixes[Math.floor(Math.random() * suffixes.length)];
        const year = Math.floor(Math.random() * (2050 - 1980 + 1) + 1980);
        return `${word} ${suffix} ${year}`;
    }

    async function fetchMaterial() {
        try {
            const [bartenderName, conceptWord, alcoholWord, awardName] = await Promise.all([
                fetchKatakanaWord(),
                fetchConceptWord(),
                fetchKatakanaWord(),
                generateAward()
            ]);

            return {
                bartender: bartenderName,
                cocktail: `${conceptWord}の${alcoholWord}`,
                award: awardName
            };

        } catch (error) { console.error("Error:", error); return null; }
    }

    async function serveCocktail() {
        contentWrapper.classList.remove('visible');
        statusDisplay.textContent = "Brewing...";
        awardDisplay.classList.remove('show');
        
        await wait(2000); 

        const newImageSrc = getNewAIFaceUrl();
        
        const data = await fetchMaterial();
        
        await new Promise((resolve) => {
            const img = new Image();
            img.onload = () => { bartenderIcon.src = newImageSrc; resolve(); };
            img.onerror = () => { resolve(); }; 
            img.src = newImageSrc;
        });

        if (data) {
            bartenderNameDisplay.textContent = `Bartender: ${data.bartender}`;
            nameDisplay.textContent = data.cocktail;
            priceDisplay.textContent = generatePrice();

            if (data.award) {
                awardDisplay.textContent = `★ ${data.award}`;
                awardDisplay.style.display = "inline-block";
                setTimeout(() => awardDisplay.classList.add('show'), 500);
            } else {
                awardDisplay.style.display = "none";
            }
            
            contentWrapper.classList.add('visible');
            statusDisplay.textContent = ""; 
            
            setTimeout(serveCocktail, 10000); 
        } else {
            setTimeout(serveCocktail, 1000);
        }
    }
</script>

<style>
  /* 念の為のh1消し */
  h1:first-of-type {
    display: none !important;
  }
</style>
