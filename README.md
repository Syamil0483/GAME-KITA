<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Petualangan Romantis: Surat Cinta di Sekolah</title>

  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: 'Indie Flower', cursive;
      background-color: #fff0f5;
      color: #333;
      padding: 20px;
      text-align: center;
    }

    .container {
      max-width: 600px;
      margin: auto;
      padding: 20px;
      background-color: #ffe4e1;
      border-radius: 15px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    .question {
      font-size: 1.4em;
      margin-bottom: 20px;
    }

    .btn-group {
      display: flex;
      justify-content: center;
      gap: 20px;
      flex-wrap: wrap;
    }

    button {
      padding: 10px 20px;
      font-size: 1em;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      transition: 0.3s;
    }

    .yes-btn {
      background-color: #ff69b4;
      color: white;
    }

    .no-btn {
      background-color: #ccc;
      color: #333;
      position: relative;
    }

    .no-btn:hover {
      position: absolute;
      left: calc(50% - 150px + (Math.random() * 100px));
      top: calc(50% - 50px + (Math.random() * 100px));
    }
  </style>
</head>

<body>
  <div class="container">
    <h1>Petualangan Cinta di Sekolah 💌</h1>
    <p class="question">Intan, kamu mau nerima surat cinta ini gak? 😳</p>
    <div class="btn-group">
      <button class="yes-btn" onclick="jawabYa()">Mauuu ❤️</button>
      <button class="no-btn" onclick="jawabTidak()">Enggak 😢</button>
    </div>
  </div>

  <script>
    function jawabYa() {
      alert("Yeeey! Aku seneng banget!! 🥰");
    }

    function jawabTidak() {
      alert("Yakin? 😭 Tapi surat ini ditulis dengan sepenuh hati loh...");
    }

    // Efek kabur tombol "No"
    const noBtn = document.querySelector('.no-btn');
    noBtn.addEventListener('mouseover', () => {
      const i = Math.floor(Math.random() * 300) - 150;
      const j = Math.floor(Math.random() * 300) - 150;
      noBtn.style.transform = `translate(${i}px, ${j}px)`;
    });
  </script>
</body>
</html>
