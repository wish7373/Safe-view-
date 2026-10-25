<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="referrer" content="no-referrer">
<title>SafeView | Private PPT</title>

<style>
* { box-sizing: border-box; }

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #0b1020;
  color: #f5f7ff;
  line-height: 1.6;
}

header {
  text-align: center;
  padding: 22px;
  border-bottom: 1px solid #29334c;
}

.logo { font-size: 28px; font-weight: bold; }
.logo span { color: #7c9cff; }

main {
  width: min(700px, 92%);
  margin: 28px auto;
}

.card {
  background: #151d32;
  border: 1px solid #2b3651;
  padding: 20px;
  border-radius: 16px;
  margin-bottom: 18px;
}

input, button {
  display: block;
  width: 100%;
  padding: 13px;
  margin: 10px 0;
  border-radius: 9px;
}

input {
  background: #202b43;
  color: white;
  border: 1px solid #465472;
}

button {
  border: 0;
  background: #7c9cff;
  color: #081020;
  font-weight: bold;
  cursor: pointer;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.notice {
  padding: 12px;
  border-radius: 9px;
  background: #202b43;
  overflow-wrap: anywhere;
}

small { color: #b7c2da; }
[hidden] { display: none !important; }
</style>
</head>

<body>
<header>
  <div class="logo">🔐 Safe<span>View</span></div>
  <div>Private PowerPoint Workspace</div>
</header>

<main>
  <section class="card">
    <h2>Account Login / Sign Up</h2>

    <label for="email">Email address</label>
    <input id="email" type="email"
      placeholder="Enter your email"
      autocomplete="email">

    <label for="password">Password</label>
    <input id="password" type="password"
      placeholder="At least 8 characters"
      autocomplete="current-password">

    <button id="signup" type="button">Create Account</button>
    <button id="login" type="button">Login</button>
    <button id="logout" type="button" hidden>Logout</button>

    <p id="authStatus" class="notice" aria-live="polite">
      Connecting to Supabase...
    </p>
  </section>

  <section class="card">
    <h2>📂 Upload Private PPT</h2>
    <p><small>Login required. Maximum file size: 50 MB.</small></p>

    <input id="ppt" type="file" accept=".ppt,.pptx">
    <button id="upload" type="button" disabled>
      Upload PPT
    </button>

    <p id="uploadStatus" class="notice" aria-live="polite">
      Login before uploading.
    </p>
  </section>

  <section class="card">
    <h3>🔒 Privacy</h3>
    <p><small>
      Files are stored in the private-ppts bucket.
      Storage RLS policies must restrict each user to their own folder.
      Never put a Supabase secret or service_role key in this website.
      This website does not prevent copying after an authorized download.
    </small></p>
  </section>
</main>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

<script>
"use strict";

// Replace these two values with your own Supabase project details.
const SUPABASE_URL = "https://ttknswttzxzsftowngkr.supabase.co/rest/v1/";
const SUPABASE_KEY = "sb_publishable_howesfkFfffCxF1J_QiBTw_xFht3crI";

const $ = id => document.getElementById(id);

let client = null;
let currentUser = null;
let busy = false;

function authMessage(message) {
  $("authStatus").textContent = message;
}

function uploadMessage(message) {
  $("uploadStatus").textContent = message;
}

function updateUI(user) {
  currentUser = user || null;

  $("signup").hidden = !!user;
  $("login").hidden = !!user;
  $("logout").hidden = !user;
  $("upload").disabled = !user || busy;

  if (user) {
    authMessage("Logged in: " + user.email);
  } else {
    authMessage("Not logged in.");
    uploadMessage("Login before uploading.");
  }
}

function setBusy(value) {
  busy = value;
  $("signup").disabled = value;
  $("login").disabled = value;
  $("logout").disabled = value;
  $("upload").disabled = value || !currentUser;
}

async function runAction(action) {
  if (busy) return;

  setBusy(true);

  try {
    await action();
  } catch (error) {
    authMessage("Error: " + (error.message || "Something went wrong."));
  } finally {
    setBusy(false);
  }
}

async function startApp() {
  if (!window.supabase) {
    authMessage("Supabase library failed to load. Check your internet connection.");
    return;
  }

  if (
    !SUPABASE_URL.startsWith("https://") ||
    SUPABASE_KEY === "PASTE_YOUR_SB_PUBLISHABLE_KEY_HERE" ||
    !SUPABASE_KEY.startsWith("sb_publishable_")
  ) {
    authMessage("Setup required: enter your Project URL and sb_publishable key in the code.");
    return;
  }

  client = window.supabase.createClient(
    SUPABASE_URL,
    SUPABASE_KEY
  );

  const { data, error } = await client.auth.getSession();

  if (error) {
    authMessage("Session error: " + error.message);
    return;
  }

  updateUI(data.session ? data.session.user : null);

  client.auth.onAuthStateChange((_event, session) => {
    updateUI(session ? session.user : null);
  });
}

$("signup").addEventListener("click", () => runAction(async () => {
  if (!client) throw new Error("Supabase is not configured.");

  const email = $("email").value.trim();
  const password = $("password").value;

  if (!email || password.length < 8) {
    authMessage("Enter a valid email and password of at least 8 characters.");
    return;
  }

  authMessage("Creating account...");

  const { data, error } = await client.auth.signUp({
    email,
    password,
    options: {
      emailRedirectTo: "https://wish7373.github.io/Safe-view-/"
    }
  });

  if (error) throw error;

  if (data.session) {
    updateUI(data.session.user);
    authMessage("Account created and logged in.");
  } else {
    authMessage("Signup submitted. Check your email for confirmation, then log in.");
  }
}));

$("login").addEventListener("click", () => runAction(async () => {
  if (!client) throw new Error("Supabase is not configured.");

  const email = $("email").value.trim();
  const password = $("password").value;

  if (!email || !password) {
    authMessage("Enter your email and password.");
    return;
  }

  authMessage("Logging in...");

  const { data, error } = await client.auth.signInWithPassword({
    email,
    password
  });

  if (error) throw error;

  updateUI(data.user);
}));

$("logout").addEventListener("click", () => runAction(async () => {
  const { error } = await client.auth.signOut();

  if (error) throw error;

  updateUI(null);
  uploadMessage("You have logged out.");
}));

$("upload").addEventListener("click", () => runAction(async () => {
  if (!client || !currentUser) {
    throw new Error("Please log in first.");
  }

  const file = $("ppt").files[0];

  if (!file) {
    uploadMessage("Choose a PPT file first.");
    return;
  }

  if (!/\.(ppt|pptx)$/i.test(file.name)) {
    uploadMessage("Only .ppt and .pptx files are allowed.");
    return;
  }

  if (file.size > 50 * 1024 * 1024) {
    uploadMessage("File must be 50 MB or smaller.");
    return;
  }

  // Random file name prevents accidental overwriting.
  // User ID folder must be enforced by Storage RLS policies.
  const extension = file.name.split(".").pop().toLowerCase();
  const path = currentUser.id + "/" + crypto.randomUUID() + "." + extension;

  uploadMessage("Uploading...");

  const { data, error } = await client.storage
    .from("private-ppts")
    .upload(path, file, {
      upsert: false,
      contentType: file.type || "application/octet-stream"
    });

  if (error) throw error;

  uploadMessage("Upload successful. Private path: " + data.path);
}));

startApp().catch(error => {
  authMessage("Startup error: " + error.message);
});
</script>
</body>
</html>
