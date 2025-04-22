<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Math Quiz</title>
  <link href="https://fonts.googleapis.com/css2?family=Comic+Neue:wght@700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Comic Neue', cursive;
      background-color: #fff7e6;
      text-align: center;
      padding: 30px;
    }

    h1 {
      color: #d6659a;
      font-size: 40px;
    }

    .shapes img {
      width: 200px;
      margin: 20px 0;
    }

    input, button {
      padding: 10px;
      margin: 10px;
      font-size: 16px;
      border-radius: 10px;
      border: none;
    }

    .hidden {
      display: none;
    }

    #leaderboard {
      margin-top: 20px;
    }

    .question {
      font-size: 20px;
      margin: 20px 0;
    }

    button {
      background-color: #f9c4d2;
      cursor: pointer;
    }

    button:hover {
      background-color: #f48fb1;
    }

    #result, #finalScore {
      margin-top: 15px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>Math Quiz Land</h1>
  <div class="shapes">
    <img src="https://cdn-icons-png.flaticon.com/512/8982/8982147.png" alt="Cute Calculator">
  </div>

  <div id="nameForm">
    <input type="text" id="playerName" placeholder="Masukkan namamu dulu ya">
    <button onclick="registerName()">Mulai!</button>
  </div>

  <div id="quizBox" class="hidden">
    <div class="question" id="questionText"></div>
    <input type="text" id="answerInput" placeholder="Jawaban kamu">
    <button onclick="submitAnswer()">Submit</button>
    <div id="result"></div>
  </div>

  <div id="finalScreen" class="hidden">
    <h2>Yey, kamu sudah selesai!</h2>
    <div id="finalScore"></div>
    <h3>Leaderboard</h3>
    <ul id="leaderboard"></ul>
  </div>

  <script>
    const questions = [
      { q: "x + 3 = 7, berapa nilai x?", a: "4" },
      { q: "2x = 10, maka x = ?", a: "5" },
      { q: "x - 4 = 9, nilai x adalah?", a: "13" },
      { q: "3x + 2 = 11, maka x = ?", a: "3" },
      { q: "5x = 25, maka x = ?", a: "5" },
      { q: "x/2 = 6, maka x = ?", a: "12" },
      { q: "4x - 1 = 15, x = ?", a: "4" },
      { q: "x + 6 = 14, nilai x?", a: "8" },
      { q: "2x + 3 = 9, x = ?", a: "3" },
      { q: "x - 7 = 2, maka x = ?", a: "9" }
    ];

    let playerName = "";
    let score = 0;
    let currentQuestion = 0;
    let leaderboard = [];

    function registerName() {
      const nameInput = document.getElementById("playerName").value.trim();
      if (nameInput === "") {
        alert("Tulis dulu namamu ya!");
        return;
      }
      playerName = nameInput;
      document.getElementById("nameForm").classList.add("hidden");
      document.getElementById("quizBox").classList.remove("hidden");
      showQuestion();
    }

    function showQuestion() {
      if (currentQuestion < questions.length) {
        document.getElementById("questionText").innerText = questions[currentQuestion].q;
        document.getElementById("answerInput").value = "";
        document.getElementById("result").innerText = "";
      } else {
        endQuiz();
      }
    }

    function submitAnswer() {
      const userAnswer = document.getElementById("answerInput").value.trim();
      if (userAnswer === questions[currentQuestion].a) {
        score++;
        document.getElementById("result").innerText = "Benar!";
      } else {
        document.getElementById("result").innerText = "Salah. Jawaban yang benar: " + questions[currentQuestion].a;
      }
      currentQuestion++;
      setTimeout(showQuestion, 1000);
    }

    function endQuiz() {
      document.getElementById("quizBox").classList.add("hidden");
      document.getElementById("finalScreen").classList.remove("hidden");
      document.getElementById("finalScore").innerText = playerName + ", skor kamu: " + score + " dari 10";

      leaderboard.push({ name: playerName, score: score });
      updateLeaderboard();
    }

    function updateLeaderboard() {
      const list = document.getElementById("leaderboard");
      list.innerHTML = "";
      leaderboard.sort((a, b) => b.score - a.score);
      leaderboard.forEach(entry => {
        const li = document.createElement("li");
        li.innerText = `${entry.name}: ${entry.score}`;
        list.appendChild(li);
      });
    }
  </script>
</body>
</html>
