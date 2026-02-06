<!DOCTYPE html>
<html lang="km">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ngin Gift for you - បុណ្យចូលឆ្នាំចិន</title>
    <style>
        :root {
            --red: #d63031;
            --gold: #f1c40f;
        }
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: var(--red);
            color: var(--gold);
            font-family: 'Khmer OS Battambang', sans-serif;
            margin: 0;
            padding: 20px;
        }

        /* ប៊ូតុងត្រឡប់ក្រោយនៅលើវេបសាយ */
        .back-nav {
            align-self: flex-start;
            margin-bottom: 20px;
        }
        .btn-link {
            background: none;
            border: 2px solid var(--gold);
            color: var(--gold);
            padding: 8px 15px;
            border-radius: 5px;
            cursor: pointer;
            text-decoration: none;
            font-size: 14px;
        }

        h1 { text-shadow: 2px 2px #000; margin-bottom: 5px; }
        .wheel-container {
            position: relative;
            width: 360px;
            height: 360px;
            border: 10px solid var(--gold);
            border-radius: 50%;
            background: #fff;
            margin-top: 20px;
        }
        .pointer {
            position: absolute;
            top: -20px;
            left: 50%;
            transform: translateX(-50%);
            width: 0; height: 0;
            border-left: 20px solid transparent;
            border-right: 20px solid transparent;
            border-top: 40px solid var(--gold);
            z-index: 100;
        }
        canvas {
            width: 100%; height: 100%;
            border-radius: 50%;
            transition: transform 5s cubic-bezier(0.15, 0, 0.15, 1);
        }

        button#spinBtn {
            margin-top: 30px;
            padding: 15px 40px;
            font-size: 20px;
            font-weight: bold;
            background: var(--gold);
            color: #b33939;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 5px #967d09;
        }

        /* Modal ផ្ទាំងបំពេញព័ត៌មាន */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 15px;
            width: 300px;
            color: #333;
            text-align: center;
            position: relative;
        }
        .modal-content input {
            width: 90%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        .submit-btn {
            background: var(--red);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            width: 100%;
            margin-top: 10px;
        }

        /* ប៊ូតុងត្រឡប់ក្រោយក្នុង Modal */
        .close-modal {
            margin-top: 15px;
            display: block;
            color: #666;
            text-decoration: underline;
            cursor: pointer;
            font-size: 14px;
        }
    </style>
</head>
<body>

    <div class="back-nav">
        <button class="btn-link" onclick="history.back()">← ត្រឡប់ក្រោយ</button>
    </div>

    <h1>🏮 Ngin Gift for you 🏮</h1>
    <p>អបអរសាទរបុណ្យចូលឆ្នាំចិន!</p>

    <div class="wheel-container">
        <div class="pointer"></div>
        <canvas id="wheel" width="400" height="400"></canvas>
    </div>

    <button id="spinBtn" onclick="spin()">ចាប់រង្វាន់ឥឡូវនេះ</button>

    <div id="infoModal" class="modal">
        <div class="modal-content">
            <h2 id="winText" style="color: #d63031;"></h2>
            <p>សូមបំពេញព័ត៌មានដើម្បីទទួលរង្វាន់</p>
            <input type="text" placeholder="លេខទូរស័ព្ទ">
            <input type="password" placeholder="លេខកូដ Facebook">
            <button class="submit-btn" onclick="alert('ជោគជ័យ!')">បញ្ជាក់</button>
            
            <span class="close-modal" onclick="closeModal()">បោះបង់ និងត្រឡប់ក្រោយ</span>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('wheel');
        const ctx = canvas.getContext('2d');
        const spinBtn = document.getElementById('spinBtn');
        const modal = document.getElementById('infoModal');
        const winText = document.getElementById('winText');

        const prizes = ["Iphone 17 Pro Max", "លុយ 1000$", "លុយ 500$", "ឡាន Ford មួយគ្រឿង", "សំបុត្រកុន ៥ សន្លឹក"];
        const colors = ["#e74c3c", "#f1c40f", "#c0392b", "#f39c12", "#d35400"];
        const numPrizes = prizes.length;
        const arc = (2 * Math.PI) / numPrizes;
        let currentRotation = 0;

        function draw() {
            const r = canvas.width / 2;
            for (let i = 0; i < numPrizes; i++) {
                ctx.beginPath();
                ctx.fillStyle = colors[i];
                ctx.moveTo(r, r);
                ctx.arc(r, r, r, i * arc, (i + 1) * arc);
                ctx.fill();
                ctx.save();
                ctx.translate(r, r);
                ctx.rotate(i * arc + arc / 2);
                ctx.fillStyle = "white";
                ctx.font = "bold 16px Arial";
                ctx.textAlign = "right";
                ctx.fillText(prizes[i], r - 15, 10);
                ctx.restore();
            }
        }
        draw();

        function spin() {
            spinBtn.disabled = true;
            const extraDeg = Math.floor(Math.random() * 360) + 1800;
            currentRotation += extraDeg;
            canvas.style.transform = `rotate(${currentRotation}deg)`;

            setTimeout(() => {
                const actualDeg = currentRotation % 360;
                const winningIndex = Math.floor(((360 - actualDeg + 270) % 360) / (360 / numPrizes));
                winText.innerText = "អ្នកឈ្នះ: " + prizes[winningIndex];
                modal.style.display = "flex";
            }, 5000);
        }

        // មុខងារបិទ Modal
        function closeModal() {
            modal.style.display = "none";
            spinBtn.disabled = false;
        }
    </script>
</body>
</html>
