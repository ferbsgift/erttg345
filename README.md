<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ultimate Fake Robux Generator</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: radial-gradient(circle, #111, #000);
      font-family: 'Segoe UI', sans-serif;
      color: #fff;
      overflow-x: hidden;
    }

    .container {
      max-width: 500px;
      margin: 50px auto;
      background: #1f1f1f;
      border-radius: 20px;
      padding: 40px;
      box-shadow: 0 0 30px #00ff88;
      position: relative;
    }

    h1 {
      color: #00ff88;
      text-shadow: 0 0 10px #00ff88;
      font-size: 28px;
      margin-bottom: 20px;
    }

    input, select, button {
      width: 100%;
      padding: 12px;
      margin-top: 12px;
      border-radius: 10px;
      font-size: 16px;
      border: none;
    }

    input, select {
      background-color: #2e2e2e;
      color: white;
    }

    button {
      background: linear-gradient(to right, #00cc66, #00ff88);
      font-weight: bold;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      transform: scale(1.05);
    }

    .loading-bar {
      height: 10px;
      background: #444;
      border-radius: 5px;
      overflow: hidden;
      margin-top: 20px;
      display: none;
    }

    .loading-fill {
      height: 100%;
      background: linear-gradient(90deg, #00ff88, #00cc66);
      width: 0%;
      transition: width 0.3s;
    }

    .profile {
      margin-top: 20px;
      display: none;
      align-items: center;
      flex-direction: column;
    }

    .profile img {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      border: 3px solid #00ff88;
      box-shadow: 0 0 10px #00ff88;
    }

    .profile-name {
      margin-top: 10px;
      font-size: 18px;
      font-weight: bold;
      color: #00ff88;
    }

    .status {
      margin-top: 20px;
      font-style: italic;
      color: yellow;
      display: none;
    }

    .success {
      margin-top: 20px;
      color: #00ff88;
      font-size: 20px;
      font-weight: bold;
      display: none;
    }

    .confetti {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      overflow: hidden;
    }

    .confetti-piece {
      width: 10px;
      height: 10px;
      background: #00ff88;
      position: absolute;
      animation: fall 3s linear infinite;
    }

    @keyframes fall {
      0% {transform: translateY(-10px);}
      100% {transform: translateY(500px);}
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Ultimate Fake Robux Generator</h1>
    <input type="text" id="username" placeholder="Enter Roblox Username" />
    <select id="amount">
      <option value="100">💸 100 Robux</option>
      <option value="500">💰 500 Robux</option>
      <option value="1000">💎 1000 Robux</option>
      <option value="5000">👑 5000 Robux</option>
      <option value="10000">🚀 10000 Robux</option>
    </select>
    <button onclick="start()">Generate Robux</button>

    <div class="profile" id="profile">
      <img id="avatar" src="" alt="Profile">
      <div class="profile-name" id="profile-name"></div>
    </div>

    <div class="loading-bar" id="loading-bar">
      <div class="loading-fill" id="loading-fill"></div>
    </div>

    <div class="status" id="status">Verifying user...</div>
    <div class="success" id="success">✅ Robux sent successfully!</div>

    <div class="confetti" id="confetti"></div>
  </div>

  <audio id="bg-music" src="https://cdn.pixabay.com/download/audio/2022/11/03/audio_7f32a22e99.mp3" loop></audio>
  <audio id="success-sound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_86f5b02f63.mp3"></audio>

  <script>
    window.onload = () => {
      document.getElementById("bg-music").play().catch(() => {});
    };

    async function start() {
      const username = document.getElementById("username").value.trim();
      const profile = document.getElementById("profile");
      const avatar = document.getElementById("avatar");
      const nameBox = document.getElementById("profile-name");
      const loadingBar = document.getElementById("loading-bar");
      const loadingFill = document.getElementById("loading-fill");
      const status = document.getElementById("status");
      const success = document.getElementById("success");
      const confetti = document.getElementById("confetti");
      const successSound = document.getElementById("success-sound");

      if (username === "") {
        alert("Please enter a Roblox username.");
        return;
      }

      // Reset
      profile.style.display = "none";
      loadingBar.style.display = "block";
      status.style.display = "block";
      success.style.display = "none";
      confetti.innerHTML = "";

      try {
        // Get userId by username
        const userRes = await fetch(`https://users.roblox.com/v1/usernames/users`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ usernames: [username] })
        });
        const userData = await userRes.json();
        const userId = userData.data[0]?.id;

        if (!userId) throw new Error("Invalid username");

        // Get avatar using userId
        const avatarRes = await fetch(`https://thumbnails.roblox.com/v1/users/avatar-headshot?userIds=${userId}&size=420x420&format=Png&isCircular=false`);
        const avatarData = await avatarRes.json();
        const avatarUrl = avatarData.data[0]?.imageUrl;

        avatar.src = avatarUrl;
        nameBox.textContent = username;
        profile.style.display = "flex";
      } catch (error) {
        alert("Failed to fetch Roblox profile. Please check the username.");
        loadingBar.style.display = "none";
        status.style.display = "none";
        return;
      }

      // Fake loading animation
      let progress = 0;
      loadingFill.style.width = "0%";
      status.textContent = "🔍 Verifying username...";

      const interval = setInterval(() => {
        progress += 10;
        loadingFill.style.width = `${progress}%`;

        if (progress === 30) status.textContent = "📡 Connecting to Roblox servers...";
        if (progress === 60) status.textContent = "📦 Generating Robux...";
        if (progress === 90) status.textContent = "🎁 Sending Robux...";

        if (progress >= 100) {
          clearInterval(interval);
          loadingBar.style.display = "none";
          status.style.display = "none";
          success.style.display = "block";
          successSound.play();
          throwConfetti();
          setTimeout(() => {
            alert("📩 Confirmation email sent to your Roblox account!");
          }, 1000);
        }
      }, 500);
    }

    function throwConfetti() {
      const container = document.getElementById("confetti");
      for (let i = 0; i < 50; i++) {
        const piece = document.createElement("div");
        piece.classList.add("confetti-piece");
        piece.style.left = Math.random() * 100 + "%";
        piece.style.background = i % 2 === 0 ? "#00ff88" : "#00cc66";
        piece.style.animationDuration = (Math.random() * 2 + 2) + "s";
        container.appendChild(piece);
      }
    }
  </script>
</body>
</html>

