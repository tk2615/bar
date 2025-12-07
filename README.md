<style>
    /* Google Fonts */
    @import url('https://fonts.googleapis.com/css2?family=Shippori+Mincho:wght@400;500;700&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&display=swap'); /* アワード用のカッコいいフォント */

    /* ★修正1：白枠を消すためのリセットCSS */
    body, html {
        margin: 0 !important;
        padding: 0 !important;
        width: 100%;
        height: 100%;
        background-color: #050505;
        color: #e0d8c8;
        font-family: 'Shippori Mincho', serif !important;
        overflow: hidden; /* スクロールバーも消す */
    }

    /* 全体ラッパー */
    .artifact-bar-wrapper {
        position: relative;
        width: 100vw;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow: hidden;
    }

    /* ★修正2：背景を完璧に全画面にする */
    .bar-bg {
        position: fixed; /* absoluteからfixedに変更して画面に固定 */
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        z-index: 0;
        background-image: url('https://images.unsplash.com/photo-1572116469696-31de0f17cc34?q=80&w=1920&auto=format&fit=crop'); 
        background-size: cover;
        background-position: center;
        opacity: 0.25; 
        filter: sepia(0.3) contrast(1.1) blur(2px);
    }

    .spotlight {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: radial-gradient(circle at center, rgba(30,30,30,0.3) 0%, rgba(0,0,0,0.95) 90%);
        z-index: 1;
        pointer-events: none;
    }

    .noise {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        opacity: 0.08;
        pointer-events: none;
        z-index: 2;
        background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyMDAiIGhlaWdodD0iMjAwIj48ZmlsdGVyIGlkPSJnoiPjxmZVR1cmJ1bGVuY2UgdHlwZT0iZnJhY3RhbE5vaXNlIiBiYXNlRnJlcXVlbmN5PSIwLjY1IiBudW1PY3RhdmVzPSIzIiBzdGl0Y2hUaWxlcz0ic3RpdGNoIi8+PC9maWx0ZXI+PHJlY3Qgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgZmlsdGVyPSJ1cmwoI2cpIiBvcGFjaXR5PSIxIi8+PC9zdmc+');
    }

    /* コンテナ */
    #container {
        position: relative;
        z-index: 10;
        text-align: center;
        width: 90%;
        max-width: 600px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        text-shadow: 0 0 15px rgba(200, 180, 150, 0.3);
    }

    .fade-wrapper {
        opacity: 0;
        filter: blur(10px) grayscale(50%);
        transform: scale(0.98);
        transition: all 2.5s cubic-bezier(0.22, 1, 0.36, 1);
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
        box-shadow: 0 0 40px rgba(0, 0, 0, 0.8);
        margin-bottom: 10px;
        filter: brightness(0.8) contrast(1.2) sepia(0.2);
        transition: filter 3s ease;
        background-color: #000;
    }

    #bartender-name {
        font-size: 0.7rem;
        color: #888;
        margin-bottom: 20px;
        letter-spacing: 0.15em;
        font-family: sans-serif !important;
        text-transform: uppercase;
    }

    /* ★追加：アワード表示スタイル */
    #award-display {
        font-family: 'Cinzel', serif !important; /* 洋風の賞状っぽいフォント */
        font-size: 0.8rem;
        color: #ffd700; /* ゴールド */
        border: 1px solid #ffd700;
        padding: 5px 15px;
        margin-bottom: 15px;
        letter-spacing: 0.1em;
        text-transform: uppercase;
        background: rgba(255, 215, 0, 0.05);
        box-shadow: 0 0 10px rgba(255, 215, 0, 0.2);
        opacity: 0; /* 最初は隠す */
        transform: translateY(10px);
        transition: all 1s ease;
        display: none; /* JSで制御 */
    }
    #award-display.show {
        opacity: 1;
        transform: translateY(0);
        display: inline-block;
    }

    .uniform-text {
        font-size: clamp(1.2rem, 5vw, 2.0rem);
        line-height: 1.8;
        letter-spacing: 0.15em;
        font-weight: 500;
        margin: 5px 0;
        color: #e0e0e0;
        word-break: keep-all;
    }

    #name-display {
        color: #fff;
        border-bottom: 1px solid rgba(150, 150, 150, 0.3);
        padding-bottom: 10px;
        margin-bottom: 10px;
        font-size: clamp(1.5rem, 6vw, 2.8rem); /* カクテル名は少し大きく */
    }

    #price-display {
        font-family: "Garamond", "Times New Roman", serif !important;
        font-size: clamp(1.0rem, 4vw, 1.5rem);
        color: #c0b283; 
        letter-spacing: 0.1em;
        margin-top: 15px;
        margin-bottom: 25px;
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

    /* スマホ調整 */
    @media screen and (max-width: 480px) {
        #bartender-icon { width: 80px; height: 80px; }
        .uniform-text { font-size: 1.1rem; }
        #name-display { font-size: 1.6rem; }
    }
</style>

<div class="artifact-bar-wrapper">
    <div class="bar-bg"></div>
    <div class="spotlight"></div>
    <div class="noise"></div>

    <div id="container">
        <div id="content-wrapper" class="fade-wrapper">
            <img id="bartender-icon" src="" alt="">
            <div id="bartender-name">Connecting...</div>

            <div id="award-display"></div>
            
            <div class="uniform-text">こちら</div>
            <div id="name-display" class="uniform-text">Loading...</div>
            <div id="price-display"></div>
            <div class="uniform-text">でございます</div>
        </div>
    </div>

    <div id="status">System Ready.</div>
</div>

<script>
    const contentWrapper = document.getElementById('content-wrapper');
    const nameDisplay = document.getElementById('name-display');
    const priceDisplay = document.getElementById('price-display');
    const bartenderIcon = document.getElementById('bartender-icon');
    const bartenderNameDisplay = document.getElementById('bartender-name');
    const awardDisplay = document.getElementById('award-display');
    const statusDisplay = document.getElementById('status');

    // NGワードリスト
    const banKeywords = ["人物", "生", "没", "氏", "家", "男", "女", "子", "王", "皇", "帝", "将軍", "大名", "武将", "貴族", "選手", "歌手", "俳優", "声優", "作家", "政治家", "軍人", "学者", "実業家", "画家", "タレント", "アイドル", "監督", "グループ", "メンバー", "人々", "一覧", "出身", "在住", "年度", "世紀", "年", "月", "日", "地理", "都市", "町村", "都", "道", "府", "県", "市", "区", "町", "村", "郡", "国", "地域", "場所", "駅", "賞", "事件"];

    function cleanWord(word) { return word.replace(/（.*?）/g, '').replace(/\(.*?\)/g, '').replace(/_/g, ' ').replace(/一覧/g, '').replace(/カテゴリー:/g, '').trim(); }
    
    function isValidConcept(pageData) {
        const title = pageData.title;
        if (title.includes("氏") || title.includes("天皇") || /^[0-9]+$/.test(title)) return false;
        if (!pageData.categories) return true; 
        for (let cat of pageData.categories) {
            if (banKeywords.some(keyword => cat.title.includes(keyword))) return false;
        }
        return true;
    }

    function generatePrice() {
        const min = 8; const max = 35;
        const price = Math.floor(Math.random() * (max - min + 1) + min) * 100;
        return `¥${price.toLocaleString()}-`;
    }

    function getNewAIFaceUrl() {
        const gender = Math.random() < 0.5 ? 'male' : 'female';
        return `https://xsgames.co/randomusers/avatar.php?g=${gender}&v=${new Date().getTime()}`;
    }

    // カタカナ単語抽出（バーテンダー名・アワード名・カクテル名の一部に使用）
    async function fetchKatakanaWord(retryCount = 0) {
        if (retryCount > 10) return "カオス"; 
        const url = "https://ja.wikipedia.org/w/api.php?origin=*&action=query&generator=random&grnnamespace=0&grnlimit=50&prop=categories&cllimit=max&format=json";
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

    // 概念抽出
    async function fetchConceptWord(retryCount = 0) {
        if (retryCount > 8) return "無限"; 
        const url = "https://ja.wikipedia.org/w/api.php?origin=*&action=query&generator=random&grnnamespace=0&grnlimit=20&prop=categories&cllimit=max&format=json";
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

    // ★アワード生成機能（確率で発動）
    async function generateAward() {
        // 30%の確率でアワードを表示
        if (Math.random() > 0.3) return null;

        // テンプレを使わず、ネットから拾った単語で賞を作る
        const word = await fetchKatakanaWord(); // カタカナ語の方が賞っぽい（例：オメガ賞、フェニックス賞）
        
        // 賞の種類をランダムに決定（ここは最低限の構造）
        const suffixes = ["Award", "Prize", "Gold Medal", "Selection", "Cup", "Grand Prix"];
        const suffix = suffixes[Math.floor(Math.random() * suffixes.length)];
        
        // 年号もランダム（未来や過去）
        const year = Math.floor(Math.random() * (2050 - 1980 + 1) + 1980);

        return `${word} ${suffix} ${year}`;
    }

    async function fetchMaterial() {
        try {
            const [bartenderName, conceptWord, alcoholWord, awardName] = await Promise.all([
                fetchKatakanaWord(), // バーテンダー
                fetchConceptWord(),  // カクテル前半
                fetchKatakanaWord(), // カクテル後半
                generateAward()      // アワード（nullの可能性あり）
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
        
        // アワードを一旦隠す
        awardDisplay.classList.remove('show');
        
        setTimeout(async () => {
            const newImageSrc = getNewAIFaceUrl();
            
            const [data] = await Promise.all([
                fetchMaterial(),
                new Promise((resolve) => {
                    const img = new Image();
                    img.onload = () => { bartenderIcon.src = newImageSrc; resolve(true); };
                    img.onerror = () => { resolve(false); };
                    img.src = newImageSrc;
                })
            ]);
            
            if (data) {
                bartenderNameDisplay.textContent = `Bartender: ${data.bartender}`;
                nameDisplay.textContent = data.cocktail;
                priceDisplay.textContent = generatePrice();

                // アワードがある場合のみ表示
                if (data.award) {
                    awardDisplay.textContent = `★ ${data.award}`;
                    awardDisplay.style.display = "inline-block";
                    // 少し遅れて表示させる演出
                    setTimeout(() => awardDisplay.classList.add('show'), 500);
                } else {
                    awardDisplay.style.display = "none";
                }
                
                contentWrapper.classList.add('visible');
                statusDisplay.textContent = ""; 
                setTimeout(serveCocktail, 10000); 
            } else {
                serveCocktail();
            }
        }, 3000); 
    }

    serveCocktail();
</script>

<style>
  /* 最初の見出し（h1）を消す */
  h1:first-of-type {
    display: none !important;
  }
</style>
