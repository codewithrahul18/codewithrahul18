## Hi there 👋

<!--
**codewithrahul18/codewithrahul18** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
<!-- Hacker Style GitHub Profile README -->

<h1 align="center">⚡ 𝙲𝙾𝙳𝙴𝚆𝙸𝚃𝙷𝚁𝙰𝙷𝚄𝙻18 ⚡</h1>
<h3 align="center">💻 Hacker-Style Developer | AI Explorer | Open Source Enthusiast</h3>

<p align="center">
  <img src="https://readme-typing-svg.herokuapp.com?font=Hack&size=22&duration=3000&color=00FF00&center=true&vCenter=true&width=500&lines=Welcome+to+my+Hacker+Profile;Always+Learning+%26+Building;AI+%7C+Web+%7C+Open+Source;Code+Like+a+Hacker+💀" />
</p>


## 👨‍💻 About Me
```bash
> codewithrahul18 --whoami
-------------------------------
👾 Full Stack Developer  
🧠 AI & Machine Learning Explorer  
🔐 Cyber Security Enthusiast  
⚡ Always in Hacker Mode  

---

## 👨‍💻 About Me
- 🚀 Passionate **Developer & AI Enthusiast**  
- 💡 Exploring **Machine Learning | Cybersecurity | Web 3D Projects**  
- ⚡ Love creating **Hacker-Style UIs & Tools**  
- 🌱 Currently learning **Advanced AI & Full-Stack Development**  
- 🎯 Goal: Become a **Pro AI Researcher & Hacker-Style Developer**  

---

## 🔥 Skills & Tools
```bash
💻 Languages:  Python | C++ | JavaScript | C  
⚡ Frameworks: React | Flask | Node.js | Kivy  
🛠️ Tools: Git | VS Code | Linux | n8n  
🚀 Domains: AI | Cybersecurity | Web Development | Open Source  
> Initializing Profile...
> Loading Skills ████████▒▒▒▒
> Hacker Style Engaged ✔
> Status: ONLINE 💀
const { createCanvas, Image } = require("canvas");
const fs = require("fs");
const fetch = require("node-fetch");
const GIFEncoder = require("gifencoder");

const width = 800, height = 600;
const frames = 60;
const canvas = createCanvas(width, height);
const ctx = canvas.getContext("2d");
const encoder = new GIFEncoder(width, height);

const username = "THE-RAHUL-CHAUHAN"; // replace with your GitHub username

// Characters for Matrix rain
const matrixChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789アカサタナハマヤラワガザダバパ";

// Function to generate a single frame with Matrix rain
function drawMatrixRain(frameIndex) {
  ctx.fillStyle = "rgba(0,0,0,0.2)";
  ctx.fillRect(0, 0, width, height);

  ctx.fillStyle = "#0f0";
  ctx.font = "16px monospace";

  for (let i = 0; i < width; i += 20) {
    const y = (frameIndex * 10 + i * 3) % height;
    const char = matrixChars.charAt(Math.floor(Math.random() * matrixChars.length));
    ctx.fillText(char, i, y);
  }
}

// Fetch top languages from GitHub
async function fetchTopLanguages() {
  const res = await fetch(`https://api.github.com/users/${username}/repos?per_page=100`);
  const repos = await res.json();

  let langStats = {};
  for (let repo of repos) {
    if (repo.language) {
      langStats[repo.language] = (langStats[repo.language] || 0) + 1;
    }
  }

  const sorted = Object.entries(langStats).sort((a, b) => b[1] - a[1]);
  return sorted.slice(0, 5).map(l => l[0]); // top 5
}

// Main GIF generation
(async () => {
  const topLangs = await fetchTopLanguages();
  console.log("Top languages:", topLangs);

  const langCardRes = await fetch(`https://github-readme-stats.vercel.app/api/top-langs/?username=${username}&layout=compact&theme=radical`);
  const langCardBuffer = await langCardRes.buffer();
  const langCardImg = new Image();
  langCardImg.src = langCardBuffer;

  encoder.start();
  encoder.setRepeat(0);
  encoder.setDelay(100);
  encoder.setQuality(10);

  for (let f = 0; f < frames; f++) {
    drawMatrixRain(f);

    // Hacker heading
    ctx.fillStyle = "#00ff00";
    ctx.font = "28px monospace";
    ctx.fillText("⚡ HACKER 3D STATS ⚡", 180, 60);

    // Draw GitHub top languages card
    ctx.drawImage(langCardImg, 150, 120, 500, 400);

    // Draw language labels in 3D-style rotation
    topLangs.forEach((lang, i) => {
      const angle = f * 0.1 + i * 1.2;
      const x = 400 + 200 * Math.sin(angle);
      const y = 300 + 100 * Math.cos(angle);
      ctx.fillStyle = "#0ff";
      ctx.font = "24px monospace";
      ctx.fillText(lang, x, y);
    });

    encoder.addFrame(ctx);
  }

  encoder.finish();

  if (!fs.existsSync("output")) fs.mkdirSync("output");
  fs.writeFileSync("output/hacker-3d-stats.gif", encoder.out.getData());
  console.log("✅ Hacker 3D GIF with Matrix Rain generated!");
})();
name: Generate Hacker 3D Stats GIF

on:
  schedule:
    - cron: "0 */12 * * *" # every 12 hours
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: |
          npm install canvas node-fetch gifencoder

      - name: Generate GIF
        run: node scripts/hacker-stats.js

      - name: Commit & Push GIF
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add output/hacker-3d-stats.gif
          git commit -m "Update Hacker 3D Stats GIF" || echo "No changes"
          git push
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
