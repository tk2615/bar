<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Bar Experience - Shippori Mincho</title>
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Shippori+Mincho:wght@400;500;700&display=swap" rel="stylesheet">

    <style>
        /* ベース設定 */
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            background-color: #050505;
            color: #e0d8c8;
            /* フォント指定を「Shippori Mincho」最優先に変更 */
            font-family: 'Shippori Mincho', "Yu Mincho", "YuMincho", serif;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            user-select: none;
        }

        /* 背景レイヤー */
        .bar-bg {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -3;
            /* 背景画像（Unsplashのバーカウンター画像） */
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
            z-index: -2;
        }

        .noise {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            opacity: 0.08;
            pointer-events: none;
            z-index: -1;
            background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyMDAiIGhlaWdodD0iMjAwIj48ZmlsdGVyIGlkPSJnoiPjxmZVR1cmJ1bGVuY2UgdHlwZT0iZnJhY3RhbE5vaXNlIiBiYXNlRnJlcXVlbmN5PSIwLjY1IiBudW1PY3RhdmVzPSIzIiBzdGl0Y2hUaWxlcz0ic3RpdGNoIi8+PC9maWx0ZXI+PHJlY3Qgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgZmlsdGVyPSJ1cmwoI2cpIiBvcGFjaXR5PSIxIi8+PC9zdmc+');
        }

        /* メインコンテナ */
        #container {
            text-align: center;
            width: 90%;
            max-width: 1000px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            text-shadow: 0 0 15px rgba(200, 180, 150, 0.3);
        }

        /* フェードアニメーション */
        .fade-wrapper {
            opacity: 0;
            filter: blur(10px) grayscale(50%);
            transform: scale(0.98);
            transition: all 2.5s cubic-bezier(0.22, 1, 0.36, 1);
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .fade-wrapper.visible {
            opacity: 1;
            filter: blur(0) grayscale(0%);
            transform: scale(1);
        }

        /* バーテンダーアイコン */
        #bartender-icon {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            object-fit: cover;
            border: 2px solid rgba(120, 120, 120, 0.5);
            box-shadow: 0 0 40px rgba(0, 0, 0, 0.8);
            margin-bottom: 10px;
            filter: brightness(0.8) contrast(1.2) sepia(0.2);
            transition: filter 3s ease;
            background-color: #000;
        }

        /* バーテンダー名（コードネーム） */
        #bartender-name {
            font-size: 0.9rem;
            color: #888;
            margin-bottom: 30px;
            letter-spacing: 0.15em;
            font-family: sans-serif; /* ここだけは英語なのでゴシック系でもOK */
            text-transform: uppercase;
        }

        /* 統一テキストスタイル（しっぽり明朝が適用される） */
        .uniform-text {
            font-size: clamp(1.5rem, 4vw, 3.0rem);
            line-height: 1.6;
            letter-spacing: 0.1em;
            font-weight: 500;
            margin: 5px 0;
            color: #e0e0e0;
        }

        /* カクテル名 */
        #name-display {
            color: #fff;
            border-bottom: 1px solid rgba(150, 150, 150, 0.3);
            display: inline-block;
            padding-bottom: 5px;
        }

        /* 金額 */
        #price-display {
            /* 金額は高級感のあるセリフ体で */
            font-family: "Garamond", "Times New Roman", serif;
            font-size: clamp(1.2rem, 3vw, 2.0rem);
            color: #c0b283; 
            letter-spacing: 0.1em;
            margin-top: 20px;
            margin-bottom: 20px;
            font-style: italic;
        }

        /* ステータス表示 */
        #status {
            position: absolute;
            bottom: 15px;
            right: 20px;
            font-size: 0.6rem;
            color: #555;
            font-family: sans-serif;
            letter-spacing: 0.1em;
        }
    </style>
</head>
<body>

    <div class="bar-bg"></div>
    <div class="spotlight"></div>
    <div class="noise"></div>

    <div id="container">
        <div id="content-wrapper" class="fade-wrapper">
            <img id="bartender-icon" src="" alt="">
            <div id="bartender-name">Connecting...</div>
            
            <div class="uniform-text">こちら</div>
            <div id="name-display" class="uniform-text">Loading...</div>
            <div id="price-display"></div>
            <div class="uniform-text">でございます</div>
        </div>
    </div>

    <div id="status">Font Loading...</div>

    <script>
        const contentWrapper = document.getElementById('content-wrapper');
        const nameDisplay = document.getElementById('name-display');
        const priceDisplay = document.getElementById('price-display');
        const bartenderIcon = document.getElementById('bartender-icon');
        const bartenderNameDisplay = document.getElementById('bartender-name');
        const statusDisplay = document.getElementById('status');

        const banKeywords = ["人物", "生", "没", "氏", "家", "男", "女", "子", "王", "皇", "帝", "将軍", "大名", "武将", "貴族", "選手", "歌手", "俳優", "声優", "作家", "政治家", "軍人", "学者", "実業家", "画家", "タレント", "アイドル", "監督", "グループ", "メンバー", "人々", "一覧", "出身", "在住", "年度", "世紀", "年", "月", "日", "地理", "都市", "町村", "都", "道", "府", "県", "市", "区", "町", "村", "郡", "国", "地域", "場所", "駅"];

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
            const min = 8; 
            const max = 35;
            const price = Math.floor(Math.random() * (max - min + 1) + min) * 100;
            return `¥${price.toLocaleString()}-`;
        }

        function getNewAIFaceUrl() {
            const gender = Math.random() < 0.5 ? 'male' : 'female';
            return `https://xsgames.co/randomusers/avatar.php?g=${gender}&v=${new Date().getTime()}`;
        }

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
            } catch (e) {
                return "ミラージュ";
            }
        }

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

        async function fetchMaterial() {
            try {
                const [bartenderName, conceptWord, alcoholWord] = await Promise.all([
                    fetchKatakanaWord(),
                    fetchConceptWord(),
                    fetchKatakanaWord()
                ]);

                return {
                    bartender: bartenderName,
                    cocktail: `${conceptWord}の${alcoholWord}`
                };

            } catch (error) { console.error("Error:", error); return null; }
        }

        async function serveCocktail() {
            contentWrapper.classList.remove('visible');
            statusDisplay.textContent = "Brewing...";
            
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
</body>
</html>
