<!DOCTYPE html
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Math Quiz Algebra</title>
  <link href="https://fonts.googleapis.com/css2?family=Comic+Neue:wght@700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Comic Neue', cursive;
      background-color: #fff7e6;
      text-align: center;
      padding: 30px;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      position: relative;
    }

    h1 {
      color: #d6659a;
      font-size: 40px;
      margin-top: 20px;
    }

    .shapes img {
      width: 200px;
      margin: 20px 0;
    }

    input, button {
      padding: 10px;
      margin: 5px;
      font-size: 16px;
      border-radius: 10px;
      border: none;
    }

    .question {
      font-size: 20px;
      margin: 20px 0;
    }

    button {
      background-color: #f9c4d2;
      cursor: pointer;
      width: 150px;
    }

    button:hover {
      background-color: #f48fb1;
    }

    #result, #finalScore {
      margin-top: 15px;
      font-weight: bold;
    }

    #result.correct {
      color: #4caf50; /* Hijau untuk benar */
    }

    #result.incorrect {
      color: #f44336; /* Merah untuk salah */
    }

    #leaderboardList {
      list-style: none;
      padding: 0;
    }

    .footer {
      margin-top: auto;
      font-size: 12px;
      color: #777;
      padding: 20px;
    }

    #settingButton {
      position: absolute;
      top: 10px;
      left: 10px;
      background-color: #f9c4d2;
      border: none;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      font-size: 20px;
      font-weight: bold;
      cursor: pointer;
    }

    #settingButton:hover {
      background-color: #f48fb1;
    }

    .hidden {
      display: none;
    }
  </style>
</head>
<body>

  <button id="settingButton" onclick="openSettings()">⚙️</button>

  <h1>Math Quiz</h1>
  <div class="shapes">
    <img src="https://cdn-icons-png.flaticon.com/512/8982/8982147.png" alt="Cute Calculator" />
  </div>

  <!-- Menu Utama -->
  <div id="menu">
    <button onclick="goToNameForm()">Start</button>
    <button onclick="showHelp()">Help</button>
    <button onclick="showLeaderboard()">Skor</button>
  </div>

  <!-- Form Nama -->
  <div id="nameForm" class="hidden">
    <input type="text" id="playerName" placeholder="Masukkan namamu dulu ya">
    <button onclick="registerName()">Mulai Quiz!</button>
  </div>

  <!-- Kuis -->
  <div id="quizBox" class="hidden">
    <div class="question" id="questionText"></div>
    <input type="text" id="answerInput" placeholder="Jawaban kamu">
    <br>
    <button onclick="submitAnswer()">Submit</button>
    <button onclick="useHelp()">Help</button>
    <div id="result"></div>
  </div>

  <!-- Akhir Quiz + Leaderboard -->
  <div id="finalScreen" class="hidden">
    <h2>Dan yap, selesai!</h2>
    <div id="finalScore"></div>
    <h3>Leaderboard</h3>
    <ul id="leaderboardList"></ul>
    <button onclick="backToMenu()">Kembali ke Menu</button>
  </div>

  <!-- Menu Setting -->
  <div id="settingsMenu" class="hidden">
    <h2>Setting</h2>
    <button onclick="toggleMusic()">Play/Pause Musik</button>
    <br><br>
    <button onclick="backToMenu()">Kembali ke Menu</button>
  </div>

  <div class="footer">
    Game ini dibuat untuk hiburan dan latihan matematika.<br><br>
    &copy; 2025 MathQuiz by cukurukuk
  </div>

  <!-- Musik Fun Jazz -->
  <audio id="backgroundMusic" loop preload="auto">
    <source src="https://cdn.pixabay.com/download/audio/2022/10/21/audio_fcbc27b39e.mp3?filename=smooth-jazz-ambient-126167.mp3" type="audio/mp3">
  </audio>

  <!-- Efek Klik -->
  <audio id="clickSound" preload="auto">
    <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_98b45bb77b.mp3?filename=click-button-140881.mp3" type="audio/mp3">
  </audio>

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

    const music = document.getElementById('backgroundMusic');
    const clickSound = document.getElementById('clickSound');

    // Musik baru jalan saat user klik tombol apa saja
function ensureMusicPlaying() {
  if (music.paused) {
    music.volume = 100;
    music.play().catch(() => {});
  }
}

    function playClick() {
      clickSound.play();
    }

    function hideAll() {
      document.getElementById('menu').classList.add('hidden');
      document.getElementById('nameForm').classList.add('hidden');
      document.getElementById('quizBox').classList.add('hidden');
      document.getElementById('finalScreen').classList.add('hidden');
      document.getElementById('settingsMenu').classList.add('hidden');
    }

    function goToNameForm() {
      hideAll();
      playClick();
      document.getElementById('nameForm').classList.remove('hidden');
    }

    function registerName() {
      const nameInput = document.getElementById("playerName").value.trim();
      if (nameInput === "") {
        alert("Tulis dulu namamu ya!");
        return;
      }
      playerName = nameInput;
      score = 0;
      currentQuestion = 0;
      hideAll();
      playClick();
      document.getElementById('quizBox').classList.remove('hidden');
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
      const result = document.getElementById("result");

      if (userAnswer === questions[currentQuestion].a) {
        score++;
        result.innerText = "Benar!";
        result.className = "correct";
      } else {
        result.innerText = "Salah. Jawaban: " + questions[currentQuestion].a;
        result.className = "incorrect";
      }
      currentQuestion++;
      setTimeout(showQuestion, 1000);
    }

    function useHelp() {
      const helpText = "Bantuan: Jawabannya adalah " + questions[currentQuestion].a;
      const result = document.getElementById("result");
      result.innerText = helpText;
      result.className = "";
    }

    function endQuiz() {
      leaderboard.push({ name: playerName, score: score });
      leaderboard.sort((a, b) => b.score - a.score);
      updateLeaderboard();
      hideAll();
      document.getElementById('finalScreen').classList.remove('hidden');
      document.getElementById('finalScore').innerText = `${playerName}, skor kamu: ${score} dari 10`;
    }

    function updateLeaderboard() {
      const list = document.getElementById('leaderboardList');
      list.innerHTML = "";
      leaderboard.forEach(entry => {
        const li = document.createElement("li");
        li.innerText = `${entry.name}: ${entry.score}`;
        list.appendChild(li);
      });
    }

    function showLeaderboard() {
      hideAll();
      playClick();
      document.getElementById('finalScreen').classList.remove('hidden');
      updateLeaderboard();
      document.getElementById('finalScore').innerText = "Cek daftar skor!";
    }

    function openSettings() {
      hideAll();
      playClick();
      document.getElementById('settingsMenu').classList.remove('hidden');
    }

    function toggleMusic() {
      playClick();
      if (music.paused) {
        music.play().catch(() => {});
      } else {
        music.pause();
      }
    }

    function backToMenu() {
      hideAll();
      playClick();
      document.getElementById('menu').classList.remove('hidden');
    }

    function showHelp() {
      alert("Petunjuk:\n\n1. Klik Start dan isi nama.\n2. Jawab 10 soal matematika.\n3. Klik 'Bantuan' saat kuis untuk lihat jawaban.\n4. Skor dihitung dari jumlah jawaban benar!");
    }
  </script>

</body>
</html>