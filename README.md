<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Multi Game Hub</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      background: #f4f4f4;
      color: #222;
      transition: background 0.3s, color 0.3s;
    }
    body.dark {
      background: #121212;
      color: #eee;
    }
    header {
      padding: 20px;
      text-align: center;
      background: #007bff;
      color: white;
      position: relative;
    }
    #darkModeToggle {
      position: absolute;
      right: 20px;
      top: 20px;
      padding: 8px 12px;
      border: none;
      background: white;
      color: #007bff;
      border-radius: 4px;
      cursor: pointer;
    }
    main {
      padding: 20px;
      max-width: 800px;
      margin: auto;
    }
    .game-buttons button {
      display: block;
      width: 100%;
      padding: 15px;
      margin: 10px 0;
      font-size: 1.2em;
      border-radius: 6px;
      border: none;
      background: #007bff;
      color: white;
      cursor: pointer;
    }
    .hidden {
      display: none;
    }
    .game-section h2 {
      margin-top: 0;
    }
    .puzzle-grid {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      gap: 5px;
      justify-content: center;
      margin: 20px 0;
    }
    .tile {
      width: 100px;
      height: 100px;
      font-size: 2em;
      text-align: center;
      line-height: 100px;
      background: #ddd;
      border-radius: 4px;
      cursor: pointer;
    }
    .tile.empty {
      background: transparent;
      cursor: default;
    }
    #quizOptions button {
      display: block;
      margin: 10px 0;
      padding: 10px;
      width: 100%;
      font-size: 1em;
      border: 1px solid #007bff;
      border-radius: 5px;
      cursor: pointer;
    }
    #quizOptions button:hover {
      background-color: #e6f0ff;
    }
  </style>
</head>
<body>
  <header>
    <h1>🎮 Multi Game Hub</h1>
    <button id="darkModeToggle">Dark Mode</button>
  </header>

  <main>
    <section id="home">
      <h2>Choose a Game</h2>
      <div class="game-buttons">
        <button onclick="showGame('quiz')">🧠 AI Quiz Game</button>
        <button onclick="showGame('puzzle')">🧩 Puzzle Game</button>
      </div>
    </section>

    <!-- Quiz Game -->
    <section id="quiz" class="hidden game-section">
      <h2>AI Quiz Game</h2>
      <p id="quizQuestion">Loading...</p>
      <div id="quizOptions"></div>
      <button onclick="nextQuestion()">Next</button>
      <p id="quizScore"></p>
      <button onclick="goHome()">🔙 Home</button>
    </section>

    <!-- Puzzle Game -->
    <section id="puzzle" class="hidden game-section">
      <h2>Sliding Puzzle Game</h2>
      <div class="puzzle-grid" id="puzzleGrid"></div>
      <p id="puzzleStatus"></p>
      <button onclick="shufflePuzzle()">🔄 Shuffle</button>
      <button onclick="goHome()">🔙 Home</button>
    </section>
  </main>

  <script>
    // ========== DARK MODE ==========
    const toggle = document.getElementById("darkModeToggle");
    toggle.onclick = () => {
      document.body.classList.toggle("dark");
    };

    // ========== NAVIGATION ==========
    function showGame(game) {
      document.getElementById('home').classList.add('hidden');
      document.getElementById('quiz').classList.add('hidden');
      document.getElementById('puzzle').classList.add('hidden');
      document.getElementById(game).classList.remove('hidden');
      if (game === 'quiz') loadQuestion();
      if (game === 'puzzle') setupPuzzle();
    }

    function goHome() {
      document.getElementById('quiz').classList.add('hidden');
      document.getElementById('puzzle').classList.add('hidden');
      document.getElementById('home').classList.remove('hidden');
    }

    // ========== QUIZ GAME ==========
    const questions = [
      { q: "What does AI stand for?", options: ["Artificial Intelligence", "Analog Input", "Advanced Interface", "Auto Information"], a: 0 },
      { q: "Which language is most used in AI?", options: ["Python", "JavaScript", "Ruby", "C++"], a: 0 },
      { q: "What is ML?", options: ["Machine Learning", "Maximum Logic", "Manual Looping", "Massive Listing"], a: 0 },
    ];
    let current = 0;
    let score = 0;

    function loadQuestion() {
      const q = questions[current];
      document.getElementById("quizQuestion").textContent = q.q;
      const options = q.options.map((opt, i) => {
        return `<button onclick="checkAnswer(${i})">${opt}</button>`;
      }).join("");
      document.getElementById("quizOptions").innerHTML = options;
      document.getElementById("quizScore").textContent = `Score: ${score}/${questions.length}`;
    }

    function checkAnswer(selected) {
      const q = questions[current];
      if (selected === q.a) {
        alert("✅ Correct!");
        score++;
      } else {
        alert("❌ Wrong!");
      }
      document.querySelectorAll("#quizOptions button").forEach(btn => btn.disabled = true);
    }

    function nextQuestion() {
      current++;
      if (current < questions.length) {
        loadQuestion();
      } else {
        document.getElementById("quizQuestion").textContent = "Quiz Finished!";
        document.getElementById("quizOptions").innerHTML = "";
        document.getElementById("quizScore").textContent = `Final Score: ${score}/${questions.length}`;
      }
    }

    // ========== PUZZLE GAME ==========
    let tiles = [];
    const puzzleSize = 3;

    function setupPuzzle() {
      const grid = document.getElementById("puzzleGrid");
      grid.innerHTML = '';
      tiles = Array.from({length: 8}, (_, i) => i + 1);
      tiles.push(null); // empty space
      shuffleArray(tiles);
      renderPuzzle();
    }

    function renderPuzzle() {
      const grid = document.getElementById("puzzleGrid");
      grid.innerHTML = '';
      tiles.forEach((val, idx) => {
        const tile = document.createElement("div");
        tile.className = 'tile';
        if (val === null) {
          tile.classList.add('empty');
        } else {
          tile.textContent = val;
          tile.onclick = () => moveTile(idx);
        }
        grid.appendChild(tile);
      });
    }

    function moveTile(index) {
      const empty = tiles.indexOf(null);
      const validMoves = [index - 1, index + 1, index - puzzleSize, index + puzzleSize];
      if (validMoves.includes(empty) && isAdjacent(index, empty)) {
        [tiles[empty], tiles[index]] = [tiles[index], tiles[empty]];
        renderPuzzle();
        checkWin();
      }
    }

    function isAdjacent(i1, i2) {
      const x1 = i1 % puzzleSize, y1 = Math.floor(i1 / puzzleSize);
      const x2 = i2 % puzzleSize, y2 = Math.floor(i2 / puzzleSize);
      return Math.abs(x1 - x2) + Math.abs(y1 - y2) === 1;
    }

    function checkWin() {
      const solved = [...tiles.slice(0, 8)].every((val, idx) => val === idx + 1);
      if (solved && tiles[8] === null) {
        document.getElementById("puzzleStatus").textContent = "🎉 Puzzle Solved!";
      } else {
        document.getElementById("puzzleStatus").textContent = "";
      }
    }

    function shufflePuzzle() {
      shuffleArray(tiles);
      renderPuzzle();
      document.getElementById("puzzleStatus").textContent = "";
    }

    function shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    }
  </script>
</body>
</html>
