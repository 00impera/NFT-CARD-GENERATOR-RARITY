<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NFT Card Generator</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            color: white;
        }
        .container { max-width: 900px; margin: 0 auto; }
        h1 { text-align: center; margin: 20px 0; font-size: 28px; text-shadow: 2px 2px 4px rgba(0,0,0,0.5); }
        .controls {
            background: rgba(255,255,255,0.15);
            backdrop-filter: blur(10px);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 20px;
        }
        .input-group { margin-bottom: 18px; }
        label { display: block; margin-bottom: 8px; font-weight: bold; font-size: 14px; }
        input[type="text"], input[type="number"], select { 
            width: 100%; 
            padding: 12px; 
            border-radius: 8px; 
            border: none; 
            font-size: 15px;
        }
        input[type="file"] {
            width: 100%;
            padding: 10px;
            background: white;
            border-radius: 8px;
            cursor: pointer;
        }
        input[type="range"] { width: 100%; }
        button {
            background: linear-gradient(135deg, #FF69B4, #FF1493);
            color: white;
            border: none;
            padding: 14px 28px;
            border-radius: 25px;
            font-weight: bold;
            font-size: 15px;
            cursor: pointer;
            margin: 8px 5px;
            transition: transform 0.2s;
        }
        button:hover { transform: scale(1.05); }
        button:active { transform: scale(0.95); }
        .download-btn { background: linear-gradient(135deg, #00C957, #00A040); }
        .video-btn { background: linear-gradient(135deg, #FF6B35, #F7931E); }
        #canvas { 
            max-width: 100%; 
            border-radius: 20px; 
            box-shadow: 0 20px 60px rgba(0,0,0,0.5);
            display: block;
            margin: 20px auto;
        }
        .colors { 
            display: grid; 
            grid-template-columns: repeat(4, 1fr); 
            gap: 8px; 
            margin-top: 10px; 
        }
        .color-btn {
            padding: 14px;
            border-radius: 10px;
            border: 3px solid transparent;
            cursor: pointer;
            font-size: 11px;
            font-weight: bold;
            transition: all 0.3s;
        }
        .color-btn.active { 
            border-color: white; 
            box-shadow: 0 0 20px rgba(255,255,255,0.9);
            transform: scale(1.05);
        }
        .button-row { text-align: center; margin-top: 15px; }
        .status {
            text-align: center;
            margin: 15px 0;
            padding: 10px;
            background: rgba(0,0,0,0.3);
            border-radius: 8px;
            font-size: 14px;
        }
        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎴 NFT CARD GENERATOR ⚡</h1>
        
        <div class="controls">
            <div class="input-group">
                <label>📸 STEP 1: Upload Your Image</label>
                <input type="file" id="imageInput" accept="image/*">
            </div>
            
            <div class="input-group">
                <label>🏷️ NFT Name:</label>
                <input type="text" id="nftName" value="EPIC NFT #001">
            </div>
            
            <div class="input-group">
                <label>⭐ Rarity Level:</label>
                <select id="rarityGrade">
                    <option value="SSS+|#ff00ff,#00ffff|👑">SSS+ Mythic 👑 (Holographic)</option>
                    <option value="SSS|#ff0080,#8000ff|💎">SSS Legendary 💎 (Pink/Purple)</option>
                    <option value="SS|#ffff00,#ff6600|⭐" selected>SS Epic ⭐ (Gold/Orange)</option>
                    <option value="S|#00ff00,#00ffff|🌟">S Rare 🌟 (Green/Cyan)</option>
                    <option value="A|#ff00ff,#ff0099|✨">A Uncommon ✨ (Magenta)</option>
                </select>
            </div>
            
            <div class="input-group">
                <label>⚡ Neon Lightning Colors (Click to Toggle):</label>
                <div class="colors">
                    <button class="color-btn active" style="background:#00ff00; color:#000;" data-color="#00ff00">Green</button>
                    <button class="color-btn active" style="background:#00ffff; color:#000;" data-color="#00ffff">Cyan</button>
                    <button class="color-btn active" style="background:#ff00ff; color:#000;" data-color="#ff00ff">Magenta</button>
                    <button class="color-btn active" style="background:#ffff00; color:#000;" data-color="#ffff00">Yellow</button>
                    <button class="color-btn active" style="background:#ff0080; color:#fff;" data-color="#ff0080">Pink</button>
                    <button class="color-btn active" style="background:#8000ff; color:#fff;" data-color="#8000ff">Purple</button>
                    <button class="color-btn active" style="background:#ff1744; color:#fff;" data-color="#ff1744">Red</button>
                    <button class="color-btn active" style="background:#00e5ff; color:#000;" data-color="#00e5ff">Blue</button>
                </div>
            </div>
            
            <div class="input-group">
                <label>💪 Power Stats:</label>
                <div class="stats-grid">
                    <div>
                        <label>Attack: <span id="attackVal">85</span></label>
                        <input type="range" id="attackLevel" min="0" max="100" value="85">
                    </div>
                    <div>
                        <label>Defense: <span id="defenseVal">70</span></label>
                        <input type="range" id="defenseLevel" min="0" max="100" value="70">
                    </div>
                    <div>
                        <label>Speed: <span id="speedVal">90</span></label>
                        <input type="range" id="speedLevel" min="0" max="100" value="90">
                    </div>
                    <div>
                        <label>Magic: <span id="magicVal">75</span></label>
                        <input type="range" id="magicLevel" min="0" max="100" value="75">
                    </div>
                </div>
            </div>

            <div class="input-group">
                <label>🔢 Edition & Serial:</label>
                <div class="stats-grid">
                    <input type="text" id="edition" placeholder="1/100" value="1/100">
                    <input type="text" id="serial" placeholder="#001" value="#001">
                </div>
            </div>
            
            <div class="button-row">
                <button onclick="generateCard()">✨ GENERATE CARD</button>
                <button class="download-btn" onclick="downloadCard()">💾 DOWNLOAD PNG</button>
                <button class="video-btn" onclick="downloadVideo()">🎬 DOWNLOAD VIDEO (5s)</button>
            </div>
            
            <div class="status" id="status">👆 Upload an image to start!</div>
        </div>

        <canvas id="canvas"></canvas>
    </div>

    <script>
        let img = null, animId = null, time = 0, isAnim = false;
        let colors = ['#00ff00','#00ffff','#ff00ff','#ffff00','#ff0080','#8000ff','#ff1744','#00e5ff'];
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        const status = document.getElementById('status');
        
        document.querySelectorAll('.color-btn').forEach(btn => {
            btn.onclick = function() {
                this.classList.toggle('active');
                colors = [];
                document.querySelectorAll('.color-btn.active').forEach(b => colors.push(b.dataset.color));
                if (colors.length === 0) colors = ['#ffffff'];
            };
        });
        
        ['attack', 'defense', 'speed', 'magic'].forEach(stat => {
            document.getElementById(stat + 'Level').oninput = function() {
                document.getElementById(stat + 'Val').textContent = this.value;
            };
        });
        
        document.getElementById('imageInput').onchange = function(e) {
            const file = e.target.files[0];
            if (file) {
                status.textContent = '📥 Loading image...';
                const reader = new FileReader();
                reader.onload = function(ev) {
                    const i = new Image();
                    i.onload = function() { 
                        img = i; 
                        status.textContent = '✅ Image loaded! Click "Generate Card"';
                        generateCard(); 
                    };
                    i.src = ev.target.result;
                };
                reader.readAsDataURL(file);
            }
        };
        
        function generateCard() {
            if (!img) { 
                alert('❌ Please upload an image first!'); 
                status.textContent = '⚠️ No image uploaded yet!';
                return; 
            }
            status.textContent = '🎨 Generating animated card...';
            if (animId) cancelAnimationFrame(animId);
            time = 0; 
            isAnim = true;
            animate();
            status.textContent = '✨ Card is animating! Use buttons to download.';
        }
        
        function animate() {
            if (!isAnim) return;
            time += 0.03;
            drawCard();
            animId = requestAnimationFrame(animate);
        }
        
        function drawCard() {
            const name = document.getElementById('nftName').value;
            const [grade, colorStr, icon] = document.getElementById('rarityGrade').value.split('|');
            const rarityColors = colorStr.split(',');
            const attack = parseInt(document.getElementById('attackLevel').value);
            const defense = parseInt(document.getElementById('defenseLevel').value);
            const speed = parseInt(document.getElementById('speedLevel').value);
            const magic = parseInt(document.getElementById('magicLevel').value);
            const edition = document.getElementById('edition').value;
            const serial = document.getElementById('serial').value;
            
            canvas.width = 800;
            canvas.height = 1200;
            
            const bg = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
            bg.addColorStop(0, '#0a0a0a');
            bg.addColorStop(1, '#1a1a2e');
            ctx.fillStyle = bg;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            for (let i = 0; i < 8; i++) {
                const c = colors[i % colors.length];
                const angle = time * 2 + i;
                const r = 200 + Math.sin(time * 3 + i) * 50;
                const x = canvas.width/2 + Math.cos(angle) * r;
                const y = canvas.height/2 + Math.sin(angle) * r;
                const grad = ctx.createRadialGradient(x, y, 0, x, y, 40);
                grad.addColorStop(0, c);
                grad.addColorStop(1, 'transparent');
                ctx.fillStyle = grad;
                ctx.beginPath();
                ctx.arc(x, y, 40, 0, Math.PI * 2);
                ctx.fill();
            }
            
            const borderC = colors[Math.floor(time * 2) % colors.length];
            ctx.strokeStyle = borderC;
            ctx.lineWidth = 8;
            ctx.shadowColor = borderC;
            ctx.shadowBlur = 20;
            ctx.strokeRect(20, 20, canvas.width - 40, canvas.height - 40);
            ctx.shadowBlur = 0;
            
            const imgW = canvas.width - 80;
            const imgH = 550;
            const imgX = 40;
            const imgY = 60;
            
            ctx.save();
            ctx.beginPath();
            ctx.roundRect(imgX, imgY, imgW, imgH, 15);
            ctx.clip();
            
            const scale = Math.max(imgW / img.width, imgH / img.height);
            const scaledW = img.width * scale;
            const scaledH = img.height * scale;
            const offsetX = imgX + (imgW - scaledW) / 2;
            const offsetY = imgY + (imgH - scaledH) / 2;
            
            ctx.drawImage(img, offsetX, offsetY, scaledW, scaledH);
            
            ctx.globalCompositeOperation = 'screen';
            ctx.fillStyle = colors[Math.floor(time) % colors.length] + '20';
            ctx.fillRect(imgX, imgY, imgW, imgH);
            ctx.globalCompositeOperation = 'source-over';
            ctx.restore();
            
            for (let i = 0; i < 12; i++) {
                const angle = time * 2 + i * 0.5;
                const px = imgX + imgW/2 + Math.cos(angle) * (imgW/2 - 60);
                const py = imgY + imgH/2 + Math.sin(angle) * (imgH/2 - 60);
                const size = 25 + Math.sin(time * 4 + i) * 8;
                const icons = ['⭐','💎','⚡','✨'];
                
                ctx.shadowColor = colors[i % colors.length];
                ctx.shadowBlur = 20;
                ctx.font = `${size}px Arial`;
                ctx.textAlign = 'center';
                ctx.fillStyle = 'white';
                ctx.fillText(icons[i % icons.length], px, py);
            }
            ctx.shadowBlur = 0;
            
            const badgeY = imgY + imgH - 40;
            const pulse = 20 + Math.sin(time * 3) * 8;
            
            const rarityGrad = ctx.createLinearGradient(canvas.width/2 - 120, badgeY, canvas.width/2 + 120, badgeY);
            rarityColors.forEach((c, i) => rarityGrad.addColorStop(i / (rarityColors.length - 1), c));
            
            ctx.fillStyle = 'rgba(0,0,0,0.9)';
            ctx.shadowColor = rarityColors[0];
            ctx.shadowBlur = pulse;
            ctx.beginPath();
            ctx.roundRect(canvas.width/2 - 120, badgeY - 30, 240, 60, 30);
            ctx.fill();
            
            ctx.strokeStyle = rarityGrad;
            ctx.lineWidth = 4;
            ctx.shadowBlur = 20;
            ctx.stroke();
            
            ctx.fillStyle = rarityGrad;
            ctx.font = 'bold 36px Arial';
            ctx.textAlign = 'center';
            ctx.shadowBlur = 30;
            ctx.fillText(`${icon} ${grade}`, canvas.width/2, badgeY);
            ctx.shadowBlur = 0;
            
            const panelY = imgY + imgH + 30;
            ctx.fillStyle = 'rgba(0,0,0,0.85)';
            ctx.fillRect(50, panelY, canvas.width - 100, 70);
            
            ctx.fillStyle = colors[Math.floor(time * 1.5) % colors.length];
            ctx.shadowColor = ctx.fillStyle;
            ctx.shadowBlur = 15;
            ctx.font = 'bold 28px Arial';
            ctx.textAlign = 'left';
            ctx.fillText(name, 70, panelY + 30);
            
            ctx.textAlign = 'right';
            ctx.font = '20px Arial';
            ctx.fillStyle = 'white';
            ctx.fillText(serial, canvas.width - 70, panelY + 30);
            
            ctx.textAlign = 'center';
            ctx.font = '18px Arial';
            ctx.fillStyle = '#aaa';
            ctx.shadowBlur = 0;
            ctx.fillText(`Edition: ${edition}`, canvas.width/2, panelY + 60);
            
            const statsY = panelY + 90;
            const stats = [
                { name: 'ATTACK', value: attack, icon: '⚔️' },
                { name: 'DEFENSE', value: defense, icon: '🛡️' },
                { name: 'SPEED', value: speed, icon: '⚡' },
                { name: 'MAGIC', value: magic, icon: '✨' }
            ];
            
            stats.forEach((stat, idx) => {
                const y = statsY + idx * 70;
                drawStatBar(60, y, stat.name, stat.value, stat.icon);
            });
        }
        
        function drawStatBar(x, y, label, val, icon) {
            const w = canvas.width - 120;
            const h = 25;
            const fill = (val / 100) * w;
            
            ctx.fillStyle = colors[Math.floor(time * 1.8) % colors.length];
            ctx.font = 'bold 18px Arial';
            ctx.textAlign = 'left';
            ctx.shadowColor = ctx.fillStyle;
            ctx.shadowBlur = 10;
            ctx.fillText(`${icon} ${label}`, x, y - 8);
            
            ctx.textAlign = 'right';
            ctx.fillText(`${val}/100`, x + w, y - 8);
            ctx.shadowBlur = 0;
            
            ctx.fillStyle = 'rgba(255,255,255,0.1)';
            ctx.fillRect(x, y, w, h);
            
            const barGrad = ctx.createLinearGradient(x, 0, x + fill, 0);
            for (let i = 0; i < 3; i++) {
                barGrad.addColorStop(i / 2, colors[(Math.floor(time * 2) + i) % colors.length]);
            }
            ctx.fillStyle = barGrad;
            ctx.shadowColor = colors[Math.floor(time) % colors.length];
            ctx.shadowBlur = 15;
            ctx.fillRect(x, y, fill, h);
            
            const shimmer = (time * 200) % (w + 100);
            const shimGrad = ctx.createLinearGradient(shimmer - 50, 0, shimmer + 50, 0);
            shimGrad.addColorStop(0, 'transparent');
            shimGrad.addColorStop(0.5, 'rgba(255,255,255,0.6)');
            shimGrad.addColorStop(1, 'transparent');
            ctx.fillStyle = shimGrad;
            ctx.shadowBlur = 0;
            ctx.fillRect(x, y, fill, h);
        }
        
        function downloadCard() {
            if (!img) { 
                alert('❌ Generate a card first!'); 
                return; 
            }
            status.textContent = '💾 Preparing download...';
            const wasAnim = isAnim;
            isAnim = false;
            setTimeout(() => {
                drawCard();
                canvas.toBlob(blob => {
                    const url = URL.createObjectURL(blob);
                    const a = document.createElement('a');
                    a.download = 'nft-card.png';
                    a.href = url;
                    a.click();
                    URL.revokeObjectURL(url);
                    status.textContent = '✅ PNG downloaded successfully!';
                    if (wasAnim) {
                        isAnim = true;
                        animate();
                    }
                }, 'image/png');
            }, 100);
        }
        
        function downloadVideo() {
            if (!img) { 
                alert('❌ Generate a card first!'); 
                return; 
            }
            if (!canvas.captureStream) {
                alert('❌ Video recording not supported in your browser! Try Chrome or Firefox.');
                return;
            }
            
            status.textContent = '🎬 Recording 5-second video of the NFT card...';
            
            const wasAnim = isAnim;
            if (!isAnim) {
                isAnim = true;
                animate();
            }
            
            const stream = canvas.captureStream(30);
            const recorder = new MediaRecorder(stream, { 
                mimeType: 'video/webm;codecs=vp9',
                videoBitsPerSecond: 8000000
            });
            const chunks = [];
            
            recorder.ondataavailable = e => { 
                if (e.data.size > 0) chunks.push(e.data); 
            };
            
            recorder.onstop = () => {
                const blob = new Blob(chunks, { type: 'video/webm' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.download = 'nft-card-animated.webm';
                a.href = url;
                a.click();
                URL.revokeObjectURL(url);
                status.textContent = '✅ Video downloaded! Open to view ONLY the NFT card animation (no player controls in file)';
                
                if (!wasAnim) {
                    isAnim = false;
                    cancelAnimationFrame(animId);
                }
            };
            
            recorder.start();
            setTimeout(() => recorder.stop(), 5000);
        }
    </script>
</body>
</html>
