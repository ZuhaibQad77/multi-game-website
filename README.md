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
      padding: 0;
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
    .scoreboard {
      margin-top: 30px;
    }
    .scoreboard h3 {
      text-align: center;
    }
    .scoreboard ul {
      list-style: none;
      padding: 0;
    }
    .scoreboard li {
      padding: 8px;
      background: #eee;
      margin: 5px 0;
      border-radius: 4px;
    }
    body.dark .scoreboard li {
      background: #333;
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

    <section id="quiz" class="hidden">
      <h2>AI Quiz Game</h2>
      <p id="quizQuestion">Loading...</p>
      <div id="quizOptions"></div>
      <button onclick="nextQuestion()">Next</button>
      <p id="quizScore"></p>
    </section>

    <section id="puzzle" class="hidden">
      <h2>Puzzle Game</h2>
      <div id="puzzleBoard">
        <p>Coming soon... (Add JavaScript logic here)</p>
      </div>
    </section>

    <section class="scoreboard">
      <h3>🏆 High Scores</h3>
      <ul id="scoreList">
        <li>Quiz: 4/5</li>
        <li>Puzzle: 120 seconds</li>
      </ul>
    </section>
  </main>

  <script>
    const questions = [
      { q: "What does AI stand for?", options: ["Artificial Intelligence", "Analog Input", "Advanced Interface", "Auto Information"], a: 0 },
      { q: "Which language is most used in AI?", options: ["Python", "JavaScript", "Ruby", "C++"], a: 0 },
      { q: "What is ML?", options: ["Machine Learning", "Maximum Logic", "Manual Looping", "Massive Listing"], a: 0 },
    ];

    let current = 0;
    let score = 0;

    function showGame(game) {
      document.getElementById('home').classList.add('hidden');
      document.getElementById(game).classList.remove('hidden');
      if (game === 'quiz') loadQuestion();
    }

    function loadQuestion() {
      const q = questions[current];
      document.getElementById("quizQuestion").textContent = q.q;
      const options = q.options.map((opt, i) => {
        return `<button onclick="checkAnswer(${i})">${opt}</button>`;
      }).join("");
      document.getElementById("quizOptions").innerHTML = options;
    }

    function checkAnswer(selected) {
      const q = questions[current];
      if (selected === q.a) {
        alert("Correct!");
        score++;
      } else {
        alert("Wrong!");
      }
    }

    function nextQuestion() {
      current++;
      if (current < questions.length) {
        loadQuestion();
      } else {
        document.getElementById("quizScore").textContent = `Final Score: ${score}/${questions.length}`;
      }
    }

    // Dark mode toggle
    const toggle = document.getElementById("darkModeToggle");
    toggle.onclick = () => {
      document.body.classList.toggle("dark");
    };
  </script>
</body>
</html>
