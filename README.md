
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Random Tarot Card Picker</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5e9d3;
      text-align: center;
      padding-top: 50px;
    }
    .card-result {
      margin-top: 30px;
      font-size: 2em;
      color: #6e4892;
    }
    button {
      padding: 12px 24px;
      font-size: 1.2em;
      color: #fff;
      background: #6e4892;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s;
    }
    button:hover {
      background: #8155a6;
    }
  </style>
</head>
<body>
  <h1>Tarot Card Picker</h1>
  <p>Click the button to randomly choose a Tarot card!</p>
  <button onclick="pickCard()">Pick a Card</button>
  <div class="card-result" id="result"></div>

  <script>
    const tarotCards = [
      "The Fool", "The Magician", "The High Priestess", "The Empress", "The Emperor",
      "The Hierophant", "The Lovers", "The Chariot", "Strength", "The Hermit",
      "Wheel of Fortune", "Justice", "The Hanged Man", "Death", "Temperance",
      "The Devil", "The Tower", "The Star", "The Moon", "The Sun",
      "Judgement", "The World"
    ];

    function pickCard() {
      const card = tarotCards[Math.floor(Math.random() * tarotCards.length)];
      document.getElementById('result').textContent = `You got: ${card}`;
    }
  </script>
</body>
</html>
