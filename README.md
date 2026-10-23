
<!DOCTYPE html>
<html lang="gu">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="referrer" content="no-referrer">
<title>SafeView | PPT Privacy</title>
<style>
:root {
  color-scheme: dark;
  --bg: #0b1020;
  --panel: #151d32;
  --text: #f5f7ff;
  --muted: #aab5d0;
  --accent: #7c9cff;
}
* { box-sizing: border-box; }
body {
  margin: 0;
  font-family: system-ui, sans-serif;
  background: var(--bg);
  color: var(--text);
  line-height: 1.6;
}
header {
  padding: 22px 18px;
  border-bottom: 1px solid #27314a;
  text-align: center;
}
.logo { font-size: 26px; font-weight: 800; }
.logo span { color: var(--accent); }
main { width: min(900px, 92%); margin: 30px auto; }
.hero, .card {
  background: var(--panel);
  border: 1px solid #2b3651;
  border-radius: 18px;
  padding: 22px;
  margin-bottom: 18px;
}
h1 { font-size: clamp(27px, 6vw, 42px); line-height: 1.2; }
h2 { font-size: 21px; }
p, li { color: var(--muted); }
button, .upload {
  display: inline-block;
  padding: 12px 16px;
  background: var(--accent);
  color: #081020;
  border: 0;
  border-radius: 10px;
  font-size: 15px;
  font-weight: 700;
  cursor: pointer;
}
input[type=file] {
  display: block;
  max-width: 100%;
  margin: 16px 0;
  color: var(--muted);
}
.notice {
  padding: 12px;
  border-radius: 10px;
  background: #202b43;
  color: #dce5ff;
}
footer {
  text-align: center;
  padding: 25px;
  color: var(--muted);
  font-size: 13px;
}
a { color: #9db5ff; }
</style>
</head>
<body>
<header>
  <div class="logo">🔐 Safe<span>View</span></div>
  <div>PPT Privacy Workspace</div>
</header>

<main>
  <section class="hero">
    <p>YOUR FILES • YOUR CONTROL</p>
    <h1>તમારી PPT, તમારી Privacy.</h1>
    <p>
      PowerPoint ફાઇલ પસંદ કરો અને આ demo માં
      તેની વિગતો જુઓ. આ પેજ ફાઇલને server પર
      upload કરતું નથી.
    </p>
    <label class="upload" for="ppt">📂 PPT પસંદ કરો</label>
    <input id="ppt" type="file"
      accept=".ppt,.pptx,application/vnd.ms-powerpoint,application/vnd.openxmlformats-officedocument.presentationml.presentation">
    <div id="result" class="notice" aria-live="polite">
      હજી કોઈ ફાઇલ પસંદ કરી નથી.
    </div>
  </section>

  <section class="card">
    <h2>🛡️ Privacy Controls</h2>
    <ul>
      <li>ફાઇલ પસંદ કરવાથી તે આ demo માં server પર મોકલાતી નથી.</li>
      <li>ફાઇલ શેર કરતા પહેલાં તેમાં રહેલી ખાનગી માહિતી તપાસો.</li>
      <li>ફાઇલ માટે access control આ demo માં ઉપલબ્ધ નથી.</li>
    </ul>
  </section>

  <section class="card">
    <h2>🔒 Safe Sharing Checklist</h2>
    <label><input type="checkbox"> PPT માંથી ખાનગી વિગતો તપાસી</label><br>
    <label><input type="checkbox"> યોગ્ય લોકોને જ ફાઇલ મોકલવાની ખાતરી કરી</label><br>
    <label><input type="checkbox"> શેર કરવાની મંજૂરી તપાસી</label>
  </section>

  <section class="card">
    <h2>ℹ️ સુરક્ષા વિશે</h2>
    <p>
      આ એક static website prototype છે.
      તેમાં account login, encrypted cloud storage,
      password-protected downloads અથવા server-side
      permissions નથી. તેથી તેને confidential PPT
      સુરક્ષિત રીતે શેર કરવાની સેવા ન માનશો.
    </p>
  </section>
</main>

<footer>
  SafeView • Privacy-first prototype
</footer>

<script>
const picker = document.getElementById("ppt");
const result = document.getElementById("result");

picker.addEventListener("change", () => {
  const file = picker.files[0];

  if (!file) {
    result.textContent = "હજી કોઈ ફાઇલ પસંદ કરી નથી.";
    return;
  }

  const allowed = /\.(ppt|pptx)$/i.test(file.name);
  const maxSize = 50 * 1024 * 1024;

  if (!allowed) {
    picker.value = "";
    result.textContent = "કૃપા કરીને .ppt અથવા .pptx ફાઇલ પસંદ કરો.";
    return;
  }

  if (file.size > maxSize) {
    picker.value = "";
    result.textContent = "ફાઇલ 50 MB કરતાં નાની હોવી જોઈએ.";
    return;
  }

  result.textContent =
    "પસંદ કરેલી ફાઇલ: " + file.name +
    " | કદ: " + (file.size / (1024 * 1024)).toFixed(2) +
    " MB. ફાઇલ upload કરવામાં આવી નથી.";
});
</script>
</body>
</html>
