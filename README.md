
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="referrer" content="no-referrer">
<title>SafeView | PPT Privacy</title>
<style>
*{box-sizing:border-box}
body{
  margin:0;font-family:Arial,sans-serif;
  background:#0b1020;color:#f5f7ff;
  line-height:1.6
}
header{text-align:center;padding:24px;border-bottom:1px solid #29334c}
.logo{font-size:28px;font-weight:bold}
.logo span{color:#7c9cff}
main{width:min(850px,92%);margin:30px auto}
.hero,.card{
  background:#151d32;border:1px solid #2b3651;
  padding:22px;border-radius:18px;margin-bottom:20px
}
h1{font-size:clamp(30px,6vw,44px);line-height:1.2}
h2{font-size:21px}
p,li{color:#b7c2da}
button,.upload{
  display:inline-block;padding:12px 17px;
  border:0;border-radius:10px;
  background:#7c9cff;color:#081020;
  font-weight:bold;cursor:pointer
}
input[type=file]{display:block;margin:18px 0;max-width:100%;color:#b7c2da}
.notice{background:#202b43;padding:13px;border-radius:10px;overflow-wrap:anywhere}
label{display:block;margin:10px 0}
footer{text-align:center;padding:25px;color:#aab5d0}
</style>
</head>
<body>
<header>
  <div class="logo">🔐 Safe<span>View</span></div>
  <div>Private PowerPoint Workspace</div>
</header>

<main>
  <section class="hero">
    <p>YOUR FILES. YOUR CONTROL.</p>
    <h1>Share Smarter. Protect Your Privacy.</h1>
    <p>
      Select a PowerPoint file to view its name and size.
      This demo does not upload your file to a server.
    </p>

    <label class="upload" for="ppt">📂 Select PowerPoint File</label>
    <input id="ppt" type="file" accept=".ppt,.pptx">
    <div id="result" class="notice" aria-live="polite">
      No file selected.
    </div>
  </section>

  <section class="card">
    <h2>🛡️ Privacy Controls</h2>
    <ul>
      <li>Files selected here are not uploaded by this demo.</li>
      <li>Review your presentation before sharing it.</li>
      <li>User access controls are not available in this demo.</li>
    </ul>
  </section>

  <section class="card">
    <h2>🔒 Safe Sharing Checklist</h2>
    <label><input type="checkbox"> I checked the presentation for sensitive information.</label>
    <label><input type="checkbox"> I verified the intended recipients.</label>
    <label><input type="checkbox"> I have permission to share this file.</label>
  </section>

  <section class="card">
    <h2>Security Information</h2>
    <p>
      SafeView is currently a static website prototype.
      It does not provide user accounts, encrypted cloud storage,
      password-protected downloads, or server-side access controls.
      Do not use this demo to share confidential files.
    </p>
  </section>
</main>

<footer>SafeView © 2026 | Privacy-first prototype</footer>

<script>
const picker = document.getElementById("ppt");
const result = document.getElementById("result");

picker.addEventListener("change", () => {
  const file = picker.files[0];
  if (!file) {
    result.textContent = "No file selected.";
    return;
  }

  if (!/\.(ppt|pptx)$/i.test(file.name)) {
    picker.value = "";
    result.textContent = "Please select a .ppt or .pptx file.";
    return;
  }

  if (file.size > 50 * 1024 * 1024) {
    picker.value = "";
    result.textContent = "File must be smaller than 50 MB.";
    return;
  }

  result.textContent =
    "Selected file: " + file.name +
    " | Size: " + (file.size / (1024 * 1024)).toFixed(2) +
    " MB. The file has not been uploaded.";
});
</script>
</body>
</html>
