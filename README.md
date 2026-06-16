<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D Clay Studio</title>
    <style>
        body { 
            margin: 0; background: #e0e0e0; font-family: sans-serif;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            height: 100vh; overflow: hidden;
        }

        /* 3D-эффект пластилина */
        .clay-board {
            width: 90vw; height: 70vh;
            background: #ffcc00; /* Цвет пластилина */
            border-radius: 40px;
            box-shadow: 10px 10px 0 #b38f00, inset -10px -10px 20px rgba(0,0,0,0.2);
            position: relative;
            touch-action: none;
        }

        canvas { width: 100%; height: 100%; border-radius: 40px; }

        .controls { margin-top: 20px; display: flex; gap: 10px; }
        button { padding: 15px 25px; border-radius: 20px; border: none; background: #fff; box-shadow: 0 5px 0 #ccc; font-weight: bold; }
    </style>
</head>
<body>

<div class="clay-board">
    <canvas id="canvas"></canvas>
</div>

<div class="controls">
    <button onclick="changeColor('#ff5e57')">Красный</button>
    <button onclick="changeColor('#3c40c6')">Синий</button>
    <button onclick="clearCanvas()">Сброс</button>
</div>

<script>
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    let color = '#ff5e57';

    function resize() {
        canvas.width = canvas.offsetWidth;
        canvas.height = canvas.offsetHeight;
    }
    window.onload = resize;

    function changeColor(c) { color = c; }
    function clearCanvas() { ctx.clearRect(0, 0, canvas.width, canvas.height); }

    let painting = false;

    function start(e) { painting = true; draw(e); }
    function end() { painting = false; ctx.beginPath(); }
    
    function draw(e) {
        if (!painting) return;
        ctx.lineWidth = 20;
        ctx.lineCap = 'round';
        ctx.strokeStyle = color;
        
        const rect = canvas.getBoundingClientRect();
        const x = (e.touches ? e.touches[0].clientX : e.clientX) - rect.left;
        const y = (e.touches ? e.touches[0].clientY : e.clientY) - rect.top;
        
        ctx.lineTo(x, y);
        ctx.stroke();
        ctx.beginPath();
        ctx.moveTo(x, y);
    }

    canvas.addEventListener('touchstart', start);
    canvas.addEventListener('touchend', end);
    canvas.addEventListener('touchmove', draw);
</script>
</body>
</html>
