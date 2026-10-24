
<!DOCTYPE html>
<html lang="gu">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SafeView | Secure PPT Viewer</title>
  <style>
    body {
      background: #0b1020;
      color: #f5f7ff;
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
      padding: 20px;
    }
    .viewer-card {
      background: #151d32;
      border: 1px solid #2b3651;
      padding: 20px;
      border-radius: 16px;
      max-width: 600px;
      margin: auto;
    }
    .slide-box {
      width: 100%;
      height: 320px;
      background: #202b43;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 8px;
      margin-bottom: 15px;
      overflow: hidden;
    }
    .slide-box img {
      max-width: 100%;
      max-height: 100%;
      object-fit: contain;
    }
    .controls button {
      padding: 10px 18px;
      margin: 5px;
      background: #7c9cff;
      border: none;
      border-radius: 8px;
      font-weight: bold;
      cursor: pointer;
    }
    .controls button:disabled {
      opacity: 0.4;
    }
    .badge {
      display: inline-block;
      padding: 4px 10px;
      background: #ff4d4d;
      border-radius: 12px;
      font-size: 12px;
      margin-bottom: 10px;
    }
  </style>
</head>
<body>

<div class="viewer-card">
  <span class="badge">🔒 One-Time View Protected</span>
  <h2 id="title">PPT Slide Viewer</h2>

  <div class="slide-box">
    <img id="slideImg" src="" alt="Slide Image" style="display:none;" />
    <div id="loadingText">Loading Slides...</div>
  </div>

  <div class="controls">
    <button id="prevBtn" onclick="changeSlide(-1)">◀ Prev</button>
    <button id="playBtn" onclick="toggleAutoPlay()">▶ Play Auto</button>
    <button id="nextBtn" onclick="changeSlide(1)">Next ▶</button>
  </div>

  <p id="statusMsg" style="margin-top: 15px; font-size: 14px; color: #b7c2da;"></p>
</div>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script>
  // Tamara Supabase Details
  const SUPABASE_URL = "https://ttknswttzxzsftowngkr.supabase.co";
  const SUPABASE_KEY = "sb_publishable_howesfkFfffCxF1J_QiBTw_xFht3crI";
  const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

  // Demo Slide Images (PPT mathi convert thaya pachhi aavti images)
  let slides = []; 
  let currentIndex = 0;
  let autoPlayTimer = null;

  async function checkOneTimeView() {
    // 1. Check URL parameters for file/session ID
    const urlParams = new URLSearchParams(window.location.search);
    const fileId = urlParams.get('id');

    // 2. LocalStorage athva Supabase Database thi check karo ke view thailu chhe ke nai
    const isViewed = localStorage.getItem("viewed_" + fileId);

    if (isViewed) {
      document.body.innerHTML = "<h2 style='color:red; margin-top:50px;'>❌ Aa One-Time View Link expire thai gai chhe!</h2>";
      return;
    }

    // Mark as viewed for One-Time View protection
    localStorage.setItem("viewed_" + fileId, "true");
    loadSlides();
  }

  function loadSlides() {
    // Fake/Demo Slides for testing (In production, load images from Supabase Storage folder)
    slides = [
      "https://via.placeholder.com/600x350/202b43/ffffff?text=Slide+1",
      "https://via.placeholder.com/600x350/151d32/ffffff?text=Slide+2",
      "https://via.placeholder.com/600x350/2b3651/ffffff?text=Slide+3"
    ];

    document.getElementById("loadingText").style.display = "none";
    document.getElementById("slideImg").style.display = "block";
    showSlide(0);
  }

  function showSlide(index) {
    if (index < 0 || index >= slides.length) return;
    currentIndex = index;
    document.getElementById("slideImg").src = slides[currentIndex];
    document.getElementById("statusMsg").textContent = `Slide ${currentIndex + 1} of ${slides.length}`;
    
    document.getElementById("prevBtn").disabled = (currentIndex === 0);
    document.getElementById("nextBtn").disabled = (currentIndex === slides.length - 1);
  }

  function changeSlide(direction) {
    showSlide(currentIndex + direction);
  }

  function toggleAutoPlay() {
    const playBtn = document.getElementById("playBtn");
    if (autoPlayTimer) {
      clearInterval(autoPlayTimer);
      autoPlayTimer = null;
      playBtn.textContent = "▶ Play Auto";
    } else {
      playBtn.textContent = "⏹ Stop Auto";
      autoPlayTimer = setInterval(() => {
        if (currentIndex < slides.length - 1) {
          changeSlide(1);
        } else {
          toggleAutoPlay(); // Stop when reached end
        }
      }, 2500); // 2.5 seconds per slide
    }
  }

  window.onload = checkOneTimeView;
</script>
</body>
</html>
