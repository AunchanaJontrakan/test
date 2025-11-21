<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trash Separation Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #e7f4e4;
            margin: 0;
            padding: 0;
            text-align: center;
        }
        h1 {
            margin-top: 32px;
        }
        #game-area {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin: 32px auto;
            gap: 40px;
        }
        .bin {
            border: 2px solid #333;
            border-radius: 10px;
            background: #fafafa;
            width: 160px;
            height: 220px;
            padding: 10px;
            box-shadow: 2px 2px 10px #ccc;
        }
        .bin h2 {
            margin-bottom: 10px;
        }
        #trash-items {
            margin: 32px auto;
        }
        .trash {
            display: inline-block;
            margin: 10px;
            padding: 8px 24px;
            background: #ffe1a4;
            border-radius: 8px;
            cursor: grab;
            border: 1px solid #c2a150;
            font-weight: bold;
        }
        .score {
            font-size: 1.3em;
            margin-bottom: 16px;
        }
    </style>
</head>
<body>
    <h1>Trash Separation Game</h1>
    <div class="score" id="scoreboard">Score: 0</div>
    <div id="trash-items">
        <!-- Trash items will appear here -->
    </div>
    <div id="game-area">
        <div class="bin" id="bin-recycle" ondrop="drop(event)" ondragover="allowDrop(event)">
            <h2>Recycle Bin</h2>
            <p>Paper, Plastic, Metal, Glass</p>
        </div>
        <div class="bin" id="bin-compost" ondrop="drop(event)" ondragover="allowDrop(event)">
            <h2>Compost Bin</h2>
            <p>Food, Leaves</p>
        </div>
        <div class="bin" id="bin-trash" ondrop="drop(event)" ondragover="allowDrop(event)">
            <h2>Trash Bin</h2>
            <p>Non-recyclables</p>
        </div>
    </div>
    <script>
        // Trash items: type, label, correct bin
        const trashData = [
            { type: 'paper', label: 'Newspaper', bin: 'bin-recycle' },
            { type: 'plastic', label: 'Plastic Bottle', bin: 'bin-recycle' },
            { type: 'metal', label: 'Tin Can', bin: 'bin-recycle' },
            { type: 'glass', label: 'Glass Jar', bin: 'bin-recycle' },
            { type: 'food', label: 'Apple Core', bin: 'bin-compost' },
            { type: 'leaves', label: 'Dry Leaves', bin: 'bin-compost' },
            { type: 'wrapper', label: 'Candy Wrapper', bin: 'bin-trash' },
            { type: 'styrofoam', label: 'Styrofoam Cup', bin: 'bin-trash' }
        ];

        let score = 0;

        function updateScore(points) {
            score += points;
            document.getElementById('scoreboard').textContent = `Score: ${score}`;
        }

        function renderTrashItems() {
            const container = document.getElementById('trash-items');
            container.innerHTML = "";
            trashData.forEach((item, idx) => {
                const div = document.createElement('div');
                div.className = 'trash';
                div.textContent = item.label;
                div.draggable = true;
                div.id = 'trash-' + idx;
                div.ondragstart = function (event) {
                    event.dataTransfer.setData("text", event.target.id);
                };
                container.appendChild(div);
            });
        }

        // Allow dropping
        function allowDrop(ev) {
            ev.preventDefault();
        }

        // Handle drop event
        function drop(ev) {
            ev.preventDefault();
            const data = ev.dataTransfer.getData("text");
            const trashDiv = document.getElementById(data);
            if (!trashDiv) return;

            const itemIndex = parseInt(data.split('-')[1]);
            const binId = ev.currentTarget.id;
            if (trashData[itemIndex].bin === binId) {
                updateScore(10);
                trashDiv.remove();
            } else {
                updateScore(-5);
                trashDiv.style.background = "#ffcccc";
                setTimeout(()=>{ trashDiv.style.background = "#ffe1a4" }, 600);
            }
        }

        // Init
        renderTrashItems();
    </script>
</body>
</html>
