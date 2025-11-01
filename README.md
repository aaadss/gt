<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>圣诞快乐页面微信分享链接生成器</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.1/build/qrcode.min.js"></script>
    <style>
        /* 保持原有样式不变，只添加新样式 */
        .file-upload {
            margin-bottom: 15px;
        }
        
        .file-upload-btn {
            display: inline-block;
            background: linear-gradient(to right, #4caf50, #2e7d32);
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-right: 10px;
        }
        
        .file-name {
            display: inline-block;
            font-size: 14px;
            color: #666;
        }
        
        .preview-frame {
            width: 100%;
            height: 300px;
            border: 1px solid #ddd;
            border-radius: 5px;
            margin-top: 10px;
        }
        
        .alert {
            padding: 10px 15px;
            border-radius: 5px;
            margin: 10px 0;
        }
        
        .alert-info {
            background-color: #e3f2fd;
            border-left: 4px solid #2196f3;
            color: #1565c0;
        }
        
        .alert-warning {
            background-color: #fff8e1;
            border-left: 4px solid #ffc107;
            color: #ff8f00;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1><i class="fas fa-share-alt"></i> 圣诞快乐页面微信分享链接生成器</h1>
            <p class="subtitle">将您的圣诞HTML页面转换为微信可分享的链接</p>
        </header>
        
        <div class="instructions">
            <h3><i class="fas fa-info-circle"></i> 使用说明</h3>
            <ol>
                <li>在下方输入框中粘贴您的圣诞HTML页面代码，或上传HTML文件</li>
                <li>自定义微信分享的标题和描述（可选）</li>
                <li>点击"生成微信分享链接"按钮</li>
                <li>复制生成的链接或扫描二维码在微信中测试</li>
            </ol>
            <div class="alert alert-info">
                <strong>提示：</strong> 生成的数据URL可以直接在浏览器中打开，但在微信中分享时可能需要将页面部署到服务器。
            </div>
        </div>
        
        <div class="content">
            <div class="input-section">
                <h2 class="section-title"><i class="fas fa-code"></i> 输入HTML代码</h2>
                
                <div class="file-upload">
                    <label class="file-upload-btn">
                        <i class="fas fa-upload"></i> 上传HTML文件
                        <input type="file" id="fileInput" accept=".html,.htm" style="display: none;">
                    </label>
                    <span class="file-name" id="fileName">未选择文件</span>
                </div>
                
                <textarea id="htmlCode" placeholder="在此处粘贴您的HTML代码..."></textarea>
                
                <div class="form-group">
                    <label for="shareTitle"><i class="fas fa-heading"></i> 微信分享标题</label>
                    <input type="text" id="shareTitle" placeholder="例如：圣诞快乐！" value="圣诞快乐 - 带音乐&互动">
                </div>
                
                <div class="form-group">
                    <label for="shareDesc"><i class="fas fa-align-left"></i> 微信分享描述</label>
                    <input type="text" id="shareDesc" placeholder="例如：点击查看我的圣诞祝福" value="点击查看带有音乐和互动效果的圣诞祝福页面">
                </div>
                
                <button id="generateBtn" class="btn"><i class="fas fa-link"></i> 生成微信分享链接</button>
            </div>
            
            <div class="preview-section">
                <h2 class="section-title"><i class="fas fa-eye"></i> 预览与分享</h2>
                
                <div class="preview-container">
                    <div id="previewText">生成的页面预览将显示在这里...</div>
                    <iframe id="previewFrame" class="preview-frame" style="display: none;"></iframe>
                </div>
                
                <div class="url-container">
                    <label><i class="fas fa-share"></i> 微信分享链接</label>
                    <div class="alert alert-warning">
                        <strong>注意：</strong> 生成的是数据URL，适合测试使用。在生产环境中，请将页面部署到服务器。
                    </div>
                    <div id="urlText" class="url-text">生成链接后将显示在此处</div>
                    <div class="button-group">
                        <button id="copyBtn" class="btn btn-copy"><i class="fas fa-copy"></i> 复制链接</button>
                        <button id="testBtn" class="btn"><i class="fas fa-external-link-alt"></i> 测试链接</button>
                        <button id="previewBtn" class="btn"><i class="fas fa-desktop"></i> 预览页面</button>
                    </div>
                </div>
                
                <div class="qr-container">
                    <label><i class="fas fa-qrcode"></i> 微信扫描测试</label>
                    <div id="qrcode"></div>
                    <p style="margin-top: 10px; font-size: 14px; color: #666;">使用微信扫描二维码测试分享效果</p>
                </div>
            </div>
        </div>
        
        <footer>
            <p>© 2023 圣诞快乐页面微信分享链接生成器 | 提示：生成的链接可能需要部署到服务器才能在微信中完全正常工作</p>
        </footer>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // 设置初始代码
            const christmasCode = `<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>圣诞快乐 - 带音乐&互动</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { 
            background: #0b1a3a; 
            min-height: 100vh; 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            position: relative; 
            overflow-x: hidden;
        }
        /* 圣诞树样式 */
        .christmas-tree { position: relative; margin-top: 40px; z-index: 1; }
        .tree-top { width: 0; height: 0; border-left: 50px solid transparent; border-right: 50px solid transparent; border-bottom: 80px solid #009e2a; margin: 0 auto; }
        .tree-middle { width: 0; height: 0; border-left: 80px solid transparent; border-right: 80px solid transparent; border-bottom: 120px solid #008a26; margin: -15px auto 0; }
        .tree-bottom { width: 0; height: 0; border-left: 110px solid transparent; border-right: 110px solid transparent; border-bottom: 160px solid #007722; margin: -15px auto 0; }
        .tree-trunk { width: 25px; height: 50px; background: #8b4513; margin: 0 auto; }
        /* 装饰球（点击星星后会闪烁） */
        .ornament { 
            position: absolute; 
            width: 12px; 
            height: 12px; 
            border-radius: 50%; 
            transition: all 0.3s ease;
        }
        /* 树顶星星（可点击） */
        .star { 
            position: absolute; 
            top: -25px; 
            left: 50%; 
            transform: translateX(-50%); 
            width: 0; height: 0; 
            border-left: 18px solid transparent; 
            border-right: 18px solid transparent; 
            border-bottom: 30px solid #ffd700; 
            cursor: pointer; 
            transition: all 0.5s ease;
        }
        .star::after { 
            content: ''; 
            position: absolute; 
            top: 30px; 
            left: -18px; 
            width: 0; height: 0; 
            border-left: 18px solid transparent; 
            border-right: 18px solid transparent; 
            border-top: 30px solid #ffd700; 
        }
        .star:active { transform: translateX(-50%) scale(1.1); }
        /* 圣诞文字 */
        .text { 
            color: #fff; 
            font-size: 22px; 
            margin-top: 20px; 
            font-weight: bold; 
            animation: flash 1.5s infinite alternate; 
            z-index: 1;
        }
        /* 音乐播放按钮 */
        .music-btn { 
            position: fixed; 
            bottom: 30px; 
            width: 50px; 
            height: 50px; 
            border-radius: 50%; 
            background: #ffd700; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            cursor: pointer; 
            box-shadow: 0 0 10px rgba(255,215,0,0.8); 
            z-index: 10;
        }
        .music-btn img { width: 25px; height: 25px; }
        /* 礼物弹窗（默认隐藏） */
        .gift-modal { 
            position: fixed; 
            top: 0; 
            left: 0; 
            width: 100%; 
            height: 100%; 
            background: rgba(0,0,0,0.7); 
            display: none; 
            align-items: center; 
            justify-content: center; 
            z-index: 20;
        }
        .gift-box { 
            width: 280px; 
            background: #fff; 
            border-radius: 15px; 
            padding: 25px 15px; 
            text-align: center; 
        }
        .gift-box img { width: 120px; height: 120px; margin-bottom: 15px; }
        .gift-text { 
            font-size: 18px; 
            color: #0b1a3a; 
            margin-bottom: 20px; 
            font-weight: 500;
        }
        .close-btn { 
            padding: 8px 25px; 
            background: #ffd700; 
            border: none; 
            border-radius: 20px; 
            color: #0b1a3a; 
            font-weight: bold; 
            cursor: pointer; 
            font-size: 16px;
        }
        /* 动画效果 */
        @keyframes flash { 
            from { opacity: 0.7; } 
            to { opacity: 1; text-shadow: 0 0 12px #ffd700; } 
        }
        @keyframes fall { 
            0% { transform: translateY(0) rotate(0deg); opacity: 0.8; }
            100% { transform: translateY(100vh) rotate(360deg); opacity: 0; }
        }
        /* 装饰球闪烁动画 */
        @keyframes blink { 0%,100% { opacity: 1; } 50% { opacity: 0.5; } }
        .ornament.blink { animation: blink 0.5s infinite; }
    </style>
</head>
<body>
    <!-- 圣诞树 -->
    <div class="christmas-tree">
        <div class="star" id="star"></div>
        <div class="tree-top"></div>
        <div class="tree-middle"></div>
        <div class="tree-bottom"></div>
        <div class="tree-trunk"></div>
        <!-- 装饰球 -->
        <div class="ornament" style="background: #ff0000; top: 30px; left: 35%;"></div>
        <div class="ornament" style="background: #ffd700; top: 60px; left: 65%;"></div>
        <div class="ornament" style="background: #00bfff; top: 90px; left: 25%;"></div>
        <div class="ornament" style="background: #ff69b4; top: 130px; left: 75%;"></div>
        <div class="ornament" style="background: #ffff00; top: 170px; left: 50%;"></div>
    </div>
    <div class="text">圣诞快乐，万事顺遂！</div>

    <!-- 音乐播放按钮 -->
    <div class="music-btn" id="musicBtn">
        <img src="https://img.icons8.com/fluency/96/000000/play.png" alt="播放音乐" id="musicIcon">
    </div>
    <!-- 音频元素（隐藏原生控件） -->
    <audio id="christmasMusic" loop>
        <source src="https://cdn.jsdelivr.net/gh/wanghao221/music-resource/Christmas/WeWishYouAMerryChristmas.mp3" type="audio/mpeg">
     您的浏览器不支持音频播放
    </audio>

    <!-- 礼物弹窗 -->
    <div class="gift-modal" id="giftModal">
        <div class="gift-box">
            <img src="https://img.icons8.com/fluency/96/000000/gift.png" alt="礼物盒">
            <div class="gift-text">叮！您收到一份圣诞祝福<br>愿你平安喜乐，温暖过冬～</div>
            <button class="close-btn" id="closeBtn">收下祝福</button>
        </div>
    </div>

    <!-- 核心交互脚本 -->
    <script>
        // 1. 雪花效果（保留原功能）
        function createSnow() {
            const snow = document.createElement('div');
            snow.style.position = 'absolute';
            snow.style.width = `${Math.random() * 4 + 2}px`;
            snow.style.height = `${Math.random() * 4 + 2}px`;
            snow.style.background = '#fff';
            snow.style.borderRadius = '50%';
            snow.style.left = `${Math.random() * 100}vw`;
            snow.style.top = '-10px';
            snow.style.animation = `fall ${Math.random() * 4 + 6}s linear infinite`;
            document.body.appendChild(snow);
            setTimeout(() => snow.remove(), 10000);
        }
        setInterval(createSnow, 400);

        // 2. 音乐控制
        const musicBtn = document.getElementById('musicBtn');
        const musicIcon = document.getElementById('musicIcon');
        const christmasMusic = document.getElementById('christmasMusic');
        let isPlaying = false;

        musicBtn.addEventListener('click', () => {
            if (isPlaying) {
                christmasMusic.pause();
                musicIcon.src = "https://img.icons8.com/fluency/96/000000/play.png";
            } else {
                christmasMusic.play();
                musicIcon.src = "https://img.icons8.com/fluency/96/000000/pause.png";
            }
            isPlaying = !isPlaying;
        });

        // 3. 点击星星互动（弹窗+装饰球闪烁）
        const star = document.getElementById('star');
        const giftModal = document.getElementById('giftModal');
        const closeBtn = document.getElementById('closeBtn');
        const ornaments = document.querySelectorAll('.ornament');

        star.addEventListener('click', () => {
            // 装饰球闪烁
            ornaments.forEach(ornament => {
                ornament.classList.add('blink');
                setTimeout(() => ornament.classList.remove('blink'), 1500);
            });
            // 显示礼物弹窗
            giftModal.style.display = 'flex';
        });

        // 关闭弹窗
        closeBtn.addEventListener('click', () => {
            giftModal.style.display = 'none';
        });

        // 点击弹窗外部关闭
        giftModal.addEventListener('click', (e) => {
            if (e.target === giftModal) giftModal.style.display = 'none';
        });
    </script>
</body>
</html>`;
            
            document.getElementById('htmlCode').value = christmasCode;
            
            const generateBtn = document.getElementById('generateBtn');
            const copyBtn = document.getElementById('copyBtn');
            const testBtn = document.getElementById('testBtn');
            const previewBtn = document.getElementById('previewBtn');
            const fileInput = document.getElementById('fileInput');
            const fileName = document.getElementById('fileName');
            const urlText = document.getElementById('urlText');
            const previewText = document.getElementById('previewText');
            const previewFrame = document.getElementById('previewFrame');
            const qrcodeDiv = document.getElementById('qrcode');
            
            // 文件上传处理
            fileInput.addEventListener('change', function(e) {
                const file = e.target.files[0];
                if (file) {
                    fileName.textContent = file.name;
                    const reader = new FileReader();
                    reader.onload = function(e) {
                        document.getElementById('htmlCode').value = e.target.result;
                    };
                    reader.readAsText(file);
                }
            });
            
            // 生成微信分享链接
            generateBtn.addEventListener('click', function() {
                const htmlCode = document.getElementById('htmlCode').value;
                const shareTitle = document.getElementById('shareTitle').value;
                const shareDesc = document.getElementById('shareDesc').value;
                
                if (!htmlCode.trim()) {
                    alert('请输入HTML代码');
                    return;
                }
                
                // 创建数据URL
                const dataUrl = createDataUrl(htmlCode, shareTitle, shareDesc);
                
                // 显示结果
                urlText.textContent = dataUrl;
                previewText.innerHTML = `<strong>${shareTitle}</strong><br><p>${shareDesc}</p>`;
                
                // 生成二维码
                generateQRCode(dataUrl);
                
                // 显示预览按钮
                previewBtn.style.display = 'inline-block';
            });
            
            // 复制链接
            copyBtn.addEventListener('click', function() {
                const text = urlText.textContent;
                if (text && text !== '生成链接后将显示在此处') {
                    navigator.clipboard.writeText(text)
                        .then(() => {
                            alert('链接已复制到剪贴板！');
                        })
                        .catch(err => {
                            console.error('复制失败:', err);
                            // 降级方案
                            const textArea = document.createElement('textarea');
                            textArea.value = text;
                            document.body.appendChild(textArea);
                            textArea.select();
                            document.execCommand('copy');
                            document.body.removeChild(textArea);
                            alert('链接已复制到剪贴板！');
                        });
                }
            });
            
            // 测试链接
            testBtn.addEventListener('click', function() {
                const url = urlText.textContent;
                if (url && url !== '生成链接后将显示在此处') {
                    window.open(url, '_blank');
                }
            });
            
            // 预览页面
            previewBtn.addEventListener('click', function() {
                const url = urlText.textContent;
                if (url && url !== '生成链接后将显示在此处') {
                    previewFrame.style.display = 'block';
                    previewFrame.src = url;
                    previewText.style.display = 'none';
                }
            });
            
            // 创建数据URL
            function createDataUrl(html, title, desc) {
                // 添加微信分享的meta标签
                const updatedHtml = html.replace(/<title>.*?<\/title>/, `<title>${title}</title>`)
                    .replace(/<head>/, `<head>
        <meta property="og:title" content="${title}">
        <meta property="og:description" content="${desc}">
        <meta property="og:image" content="https://img.icons8.com/fluency/96/000000/christmas-tree.png">
        <meta property="og:url" content="https://example.com/christmas-page">
        <meta name="twitter:card" content="summary_large_image">
        <meta name="description" content="${desc}">`);
                
                // 创建数据URL
                const blob = new Blob([updatedHtml], {type: 'text/html'});
                return URL.createObjectURL(blob);
            }
            
            // 生成二维码
            function generateQRCode(url) {
                qrcodeDiv.innerHTML = '';
                if (url) {
                    new QRCode(qrcodeDiv, {
                        text: url,
                        width: 150,
                        height: 150,
                        colorDark: "#000000",
                        colorLight: "#ffffff",
                        correctLevel: QRCode.CorrectLevel.H
                    });
                }
            }
            
            // 初始化二维码占位符
            qrcodeDiv.innerHTML = '<div style="padding: 50px; text-align: center; color: #999;"><i class="fas fa-qrcode" style="font-size: 48px;"></i><p style="margin-top: 10px;">生成链接后将显示二维码</p></div>';
            
            // 隐藏预览按钮初始状态
            previewBtn.style.display = 'none';
        });
    </script>
</body>
</html>

