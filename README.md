
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>SafeView | Private PPT Upload</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial;background:#0b1020;color:#f5f7ff;line-height:1.6}
header{text-align:center;padding:24px;border-bottom:1px solid #29334c}
.logo{font-size:28px;font-weight:bold}
.logo span{color:#7c9cff}
main{width:min(700px,92%);margin:28px auto}
.card{background:#151d32;border:1px solid #2b3651;padding:22px;border-radius:16px;margin-bottom:18px}
input,button{width:100%;padding:12px;margin:7px 0;border-radius:8px}
input{background:#202b43;color:white;border:1px solid #465472}
button{background:#7c9cff;color:#081020;border:0;font-weight:bold;cursor:pointer}
button:disabled{opacity:.5}
.notice{background:#202b43;padding:12px;border-radius:8px;overflow-wrap:anywhere}
small{color:#b7c2da}
</style>
</head>
<body>
<header>
  <div class="logo">🔐 Safe<span>View</span></div>
  Private PowerPoint Workspace
</header>

<main>
<section class="card">
<h2>Account Login / Sign Up</h2>
<input id="email" type="email" placeholder="Email address" autocomplete="email">
<input id="password" type="password" placeholder="Password (minimum 8 characters)" autocomplete="current-password">
<button id="signup">Create Account</button>
<button id="login">Login</button>
<button id="logout" hidden>Logout</button>
<p id="authStatus" class="notice" aria-live="polite">Not logged in.</p>
</section>

<section class="card">
<h2>Upload Private PPT</h2>
<p><small>Login first. Maximum file size: 50 MB.</small></p>
<input id="ppt" type="file" accept=".ppt,.pptx">
<button id="upload" disabled>Upload to Private Storage</button>
<p id="uploadStatus" class="notice" aria-live="polite">Choose a PPT file.</p>
</section>

<section class="card">
<h3>Security information</h3>
<p><small>Files are uploaded to the private-ppts bucket. Storage policies must restrict access to the authenticated file owner. This prototype does not create secure sharing links or prevent copying after download.</small></p>
</section>
</main>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script>
const SUPABASE_URL = "[YOUR_SUPABASE_URL](https://ttknswttzxzsftowngkr.supabase.co/rest/v1/)";
const SUPABASE_KEY = "sb_publishable_howesfkFfffCxF1J_QiBTw_xFht3crI";

const client = window.supabase.createClient(
  SUPABASE_URL,
  SUPABASE_KEY
);

const $ = id => document.getElementById(id);
let currentUser = null;

function authMessage(message) {
  $("authStatus").textContent = message;
}

function uploadMessage(message) {
  $("uploadStatus").textContent = message;
}

function showUser(user) {
  currentUser = user;
  $("upload").disabled = !user;
  $("logout").hidden = !user;
  $("login").hidden = !!user;
  $("signup").hidden = !!user;
  authMessage(user ? "Logged in: " + user.email : "Not logged in.");
}

async function refreshSession() {
  const {data, error} = await client.auth.getSession();
  if (error) authMessage(error.message);
  showUser(data?.session?.user || null);
}

$("signup").onclick = async () => {
  const email = $("email").value.trim();
  const password = $("password").value;
  if (!email || password.length < 8) {
    authMessage("Enter a valid email and password of at least 8 characters.");
    return;
  }

  const {data, error} = await client.auth.signUp({email, password});
  if (error) return authMessage(error.message);

  if (data.session) {
    showUser(data.session.user);
  } else {
    authMessage("Check your email to confirm your account, then log in.");
  }
};

$("login").onclick = async () => {
  const email = $("email").value.trim();
  const password = $("password").value;
  if (!email || !password) return authMessage("Enter email and password.");

  const {data, error} = await client.auth.signInWithPassword({
    email, password
  });
  if (error) return authMessage(error.message);
  showUser(data.user);
};

$("logout").onclick = async () => {
  const {error} = await client.auth.signOut();
  if (error) return authMessage(error.message);
  showUser(null);
  uploadMessage("Logged out.");
};

$("upload").onclick = async () => {
  if (!currentUser) return uploadMessage("Please log in first.");

  const file = $("ppt").files[0];
  if (!file) return uploadMessage("Choose a PPT file first.");

  if (!/\.(ppt|pptx)$/i.test(file.name)) {
    return uploadMessage("Only .ppt and .pptx files are accepted.");
  }

  if (file.size > 50 * 1024 * 1024) {
    return uploadMessage("File must be 50 MB or smaller.");
  }

  const ext = file.name.split(".").pop().toLowerCase();
  const path = currentUser.id + "/" + crypto.randomUUID() + "." + ext;

  $("upload").disabled = true;
  uploadMessage("Uploading...");

  try {
    const {data, error} = await client.storage
      .from("private-ppts")
      .upload(path, file, {
        upsert: false,
        contentType: file.type || "application/octet-stream"
      });

    if (error) throw error;

    uploadMessage("Upload successful. Private storage path: " + data.path);
  } catch (error) {
    uploadMessage("Upload failed: " + error.message);
  } finally {
    $("upload").disabled = !currentUser;
  }
};

client.auth.onAuthStateChange((_event, session) => {
  showUser(session?.user || null);
});
refreshSession();
</script>
</body>
</html>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
