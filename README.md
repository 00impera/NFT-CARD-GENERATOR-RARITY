<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NFT Rarity Card Generator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            color: white;
        }
        
        h1 {
            margin: 20px 0;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
        
        .controls {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 20px;
            width: 100%;
            max-width: 800px;
        }
        
        .input-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        input, select, textarea {
            width: 100%;
            padding: 10px;
            border-radius: 8px;
            border: none;
            font-size: 14px;
        }
        
        textarea {
            resize: vertical;
            min-height: 60px;
        }
        
        button {
            background: linear-gradient(135deg, #FF69B4, #FF1493);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 25px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            margin: 10px 5px;
            transition: transform 0.2s;
        }
        
        button:hover {
            transform: scale(1.05);
        }
        
        #imageInput {
            background: white;
            cursor: pointer;
        }
        
        .card-container {
            position: relative;
            max-width: 800px;
            margin: 20px auto;
        }
        
        #canvas {
            max-width: 100%;
            height: auto;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.5);
        }
        
        .download-btn {
            background: linear-gradient(135deg, #00C957, #00A040);
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-top: 10px;
        }
        
        .stat-input {
            display: flex;
            flex-direction: column;
        }
        
        .stat-input input {
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <h1>🎴 NFT Rarity Card Generator</h1>
    
    <div class="controls">
        <div class="input-group">
            <label>📸 Upload Image:</label>
            <input type="file" id="imageInput" accept="image/*">
        </div>
        
        <div class="input-group">
            <label>🏷️ NFT Name:</label>
            <input type="text" id="nftName" placeholder="Flora Femme #1" value="Flora Femme #1">
        </div>
        
        <div class="input-group">
            <label>⭐ Rarity Grade:</label>
            <select id="rarityGrade">
                <option value="SSS+|#FF69B4">SSS+ Mythic</option>
                <option value="SSS|#FF1493">SSS Legendary</option>
                <option value="SS|#FFC300">SS Epic</option>
                <option value="S|#00C957">S Rare</option>
                <option value="A|#DA70D6">A Uncommon</option>
                <option value="B|#87CEEB">B Common</option>
            </select>
        </div>
        
        <div class="input-group">
            <label>💪 Power Stats:</label>
            <div class="stats-grid">
                <div class="stat-input">
                    <label>Power Level:</label>
                    <input type="number" id="powerLevel" value="80" min="0" max="100">
                </div>
                <div class="stat-input">
                    <label>Rarity Score:</label>
                    <input type="number" id="rarityScore" value="425" min="0" max="1000">
                </div>
                <div class="stat-input">
                    <label>Edition:</label>
                    <input type="text" id="edition" value="1/100" placeholder="1/100">
                </div>
                <div class="stat-input">
                    <label>Serial:</label>
                    <input type="text" id="serial" value="#001" placeholder="#001">
                </div>
            </div>
        </div>
        
        <div class="input-group">
            <label>🎨 Card Style:</label>
            <select id="cardStyle">
                <option value="unicorn">🦄 Unicorn Rainbow</option>
                <option value="holographic">Holographic Premium</option>
                <option value="minimal">Minimal Clean</option>
                <option value="retro">Retro Gaming</option>
            </select>
        </div>
        
        <button onclick="generateCard()">✨ Generate Card</button>
        <button class="download-btn" onclick="downloadCard()">💾 Download Card</button>
    </div>
    
    <div class="card-container">
        <canvas id="canvas"></canvas>
    </div>

    <script>
        let uploadedImage = null;
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        
        document.getElementById('imageInput').addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(event) {
                    const img = new Image();
                    img.onload = function() {
                        uploadedImage = img;
                        generateCard();
                    };
                    img.src = event.target.result;
                };
                reader.readAsDataURL(file);
            }
        });
        
        function generateCard() {
            if (!uploadedImage) {
                alert('Please upload an image first!');
                return;
            }
            
            const name = document.getElementById('nftName').value;
            const [grade, color] = document.getElementById('rarityGrade').value.split('|');
            const powerLevel = document.getElementById('powerLevel').value;
            const rarityScore = document.getElementById('rarityScore').value;
            const edition = document.getElementById('edition').value;
            const serial = document.getElementById('serial').value;
            const style = document.getElementById('cardStyle').value;
            
            // Set canvas size
            canvas.width = 1000;
            canvas.height = 1400;
            
            // Draw background gradient
            const bgGradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
            if (style === 'unicorn') {
                bgGradient.addColorStop(0, '#FF69B4');
                bgGradient.addColorStop(0.2, '#FF1493');
                bgGradient.addColorStop(0.4, '#9B59B6');
                bgGradient.addColorStop(0.6, '#3498DB');
                bgGradient.addColorStop(0.8, '#1ABC9C');
                bgGradient.addColorStop(1, '#F1C40F');
            } else {
                bgGradient.addColorStop(0, '#1a1a2e');
                bgGradient.addColorStop(1, '#16213e');
            }
            ctx.fillStyle = bgGradient;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Draw holographic effect if selected
            if (style === 'holographic') {
                drawHolographicEffect();
            } else if (style === 'unicorn') {
                drawUnicornEffect();
            }
            
            // Draw border
            if (style === 'unicorn') {
                // Rainbow gradient border
                const borderGradient = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
                borderGradient.addColorStop(0, '#FF69B4');
                borderGradient.addColorStop(0.2, '#FF00FF');
                borderGradient.addColorStop(0.4, '#00BFFF');
                borderGradient.addColorStop(0.6, '#00FF7F');
                borderGradient.addColorStop(0.8, '#FFD700');
                borderGradient.addColorStop(1, '#FF69B4');
                ctx.strokeStyle = borderGradient;
                ctx.lineWidth = 12;
                ctx.shadowColor = '#FF69B4';
                ctx.shadowBlur = 30;
                ctx.strokeRect(20, 20, canvas.width - 40, canvas.height - 40);
                ctx.shadowBlur = 0;
            } else {
                ctx.strokeStyle = color;
                ctx.lineWidth = 8;
                ctx.strokeRect(20, 20, canvas.width - 40, canvas.height - 40);
            }
            
            // Draw image
            const imgWidth = canvas.width - 80;
            const imgHeight = 800;
            const imgX = 40;
            const imgY = 60;
            
            ctx.save();
            ctx.beginPath();
            ctx.roundRect(imgX, imgY, imgWidth, imgHeight, 20);
            ctx.clip();
            
            const scale = Math.max(imgWidth / uploadedImage.width, imgHeight / uploadedImage.height);
            const scaledWidth = uploadedImage.width * scale;
            const scaledHeight = uploadedImage.height * scale;
            const offsetX = imgX + (imgWidth - scaledWidth) / 2;
            const offsetY = imgY + (imgHeight - scaledHeight) / 2;
            
            ctx.drawImage(uploadedImage, offsetX, offsetY, scaledWidth, scaledHeight);
            ctx.restore();
            
            // Draw rarity badge
            const badgeY = imgY + imgHeight - 80;
            drawRarityBadge(canvas.width / 2, badgeY, grade, color);
            
            // Draw info panel
            const panelY = imgY + imgHeight + 30;
            drawInfoPanel(panelY, name, powerLevel, rarityScore, edition, serial, color);
            
            // Draw power bars
            drawPowerBars(panelY + 150, powerLevel, rarityScore, color);
        }
        
        function drawHolographicEffect() {
            const gradient = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
            gradient.addColorStop(0, 'rgba(255,0,255,0.1)');
            gradient.addColorStop(0.25, 'rgba(0,255,255,0.1)');
            gradient.addColorStop(0.5, 'rgba(255,255,0,0.1)');
            gradient.addColorStop(0.75, 'rgba(0,255,0,0.1)');
            gradient.addColorStop(1, 'rgba(255,0,0,0.1)');
            ctx.fillStyle = gradient;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
        }
        
        function drawUnicornEffect() {
            // Rainbow shimmer overlay
            const gradient1 = ctx.createRadialGradient(canvas.width / 2, canvas.height / 3, 0, canvas.width / 2, canvas.height / 3, canvas.width / 2);
            gradient1.addColorStop(0, 'rgba(255,105,180,0.3)');
            gradient1.addColorStop(0.33, 'rgba(138,43,226,0.2)');
            gradient1.addColorStop(0.66, 'rgba(0,191,255,0.2)');
            gradient1.addColorStop(1, 'rgba(255,215,0,0.1)');
            ctx.fillStyle = gradient1;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Sparkle effects
            ctx.fillStyle = 'rgba(255,255,255,0.8)';
            for (let i = 0; i < 50; i++) {
                const x = Math.random() * canvas.width;
                const y = Math.random() * canvas.height;
                const size = Math.random() * 4 + 1;
                ctx.beginPath();
                ctx.arc(x, y, size, 0, Math.PI * 2);
                ctx.fill();
                
                // Add star sparkles
                if (Math.random() > 0.7) {
                    ctx.save();
                    ctx.translate(x, y);
                    ctx.strokeStyle = 'rgba(255,255,255,0.6)';
                    ctx.lineWidth = 2;
                    ctx.beginPath();
                    ctx.moveTo(0, -size * 2);
                    ctx.lineTo(0, size * 2);
                    ctx.moveTo(-size * 2, 0);
                    ctx.lineTo(size * 2, 0);
                    ctx.stroke();
                    ctx.restore();
                }
            }
        }
        
        function drawRarityBadge(x, y, grade, color) {
            ctx.save();
            
            // Badge background
            ctx.fillStyle = 'rgba(0,0,0,0.9)';
            ctx.beginPath();
            ctx.roundRect(x - 150, y - 40, 300, 80, 40);
            ctx.fill();
            
            // Border glow
            ctx.strokeStyle = color;
            ctx.lineWidth = 4;
            ctx.shadowColor = color;
            ctx.shadowBlur = 20;
            ctx.stroke();
            
            // Text
            ctx.shadowBlur = 0;
            ctx.fillStyle = color;
            ctx.font = 'bold 48px Arial';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText(grade, x, y);
            
            ctx.restore();
        }
        
        function drawInfoPanel(y, name, powerLevel, rarityScore, edition, serial, color) {
            ctx.save();
            
            // Panel background
            ctx.fillStyle = 'rgba(0,0,0,0.7)';
            ctx.fillRect(60, y, canvas.width - 120, 120);
            
            // Name
            ctx.fillStyle = color;
            ctx.font = 'bold 36px Arial';
            ctx.textAlign = 'left';
            ctx.fillText(name, 80, y + 35);
            
            // Serial
            ctx.fillStyle = '#ffffff';
            ctx.font = '24px Arial';
            ctx.textAlign = 'right';
            ctx.fillText(serial, canvas.width - 80, y + 35);
            
            // Stats row
            ctx.fillStyle = '#cccccc';
            ctx.font = '20px Arial';
            ctx.textAlign = 'left';
            ctx.fillText(`Power: ${powerLevel}`, 80, y + 75);
            ctx.fillText(`Score: ${rarityScore}`, 300, y + 75);
            ctx.fillText(`Edition: ${edition}`, 550, y + 75);
            
            ctx.restore();
        }
        
        function drawPowerBars(y, powerLevel, rarityScore, color) {
            ctx.save();
            
            // Power Level Bar
            drawStatBar(100, y, 'POWER LEVEL', powerLevel, 100, color);
            
            // Rarity Score Bar
            drawStatBar(100, y + 80, 'RARITY SCORE', rarityScore, 1000, color);
            
            ctx.restore();
        }
        
        function drawStatBar(x, y, label, value, max, color) {
            const barWidth = canvas.width - 200;
            const barHeight = 30;
            const fillWidth = (value / max) * barWidth;
            
            // Label
            ctx.fillStyle = '#ffffff';
            ctx.font = 'bold 18px Arial';
            ctx.textAlign = 'left';
            ctx.fillText(label, x, y - 10);
            
            // Value
            ctx.textAlign = 'right';
            ctx.fillText(`${value}/${max}`, x + barWidth, y - 10);
            
            // Bar background
            ctx.fillStyle = 'rgba(255,255,255,0.1)';
            ctx.fillRect(x, y, barWidth, barHeight);
            
            // Bar fill
            const gradient = ctx.createLinearGradient(x, 0, x + fillWidth, 0);
            gradient.addColorStop(0, color);
            gradient.addColorStop(1, '#ffffff');
            ctx.fillStyle = gradient;
            ctx.fillRect(x, y, fillWidth, barHeight);
            
            // Bar border
            ctx.strokeStyle = color;
            ctx.lineWidth = 2;
            ctx.strokeRect(x, y, barWidth, barHeight);
        }
        
        function downloadCard() {
            if (!uploadedImage) {
                alert('Please generate a card first!');
                return;
            }
            
            try {
                canvas.toBlob(function(blob) {
                    const url = URL.createObjectURL(blob);
                    const link = document.createElement('a');
                    const name = document.getElementById('nftName').value.replace(/\s+/g, '-').toLowerCase();
                    link.download = `${name}-card.png`;
                    link.href = url;
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);
                    URL.revokeObjectURL(url);
                }, 'image/png');
            } catch (error) {
                console.error('Download error:', error);
                alert('Download failed. Please try right-clicking the card and selecting "Save image as..."');
            }
        }
    </script>
</body>
</html>
