<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="referrer" content="no-referrer">
<meta name="theme-color" content="#0b1020">
<title>SafeView | Private PPT Workspace</title>
<style>
  :root {
    color-scheme: dark;
    --bg: #080d19;
    --panel: rgba(20, 29, 49, .88);
    --panel-soft: #111a2d;
    --line: #2a3854;
    --text: #f5f7ff;
    --muted: #a8b5cf;
    --blue: #8aa7ff;
    --blue-strong: #6688f5;
    --green: #75ddb2;
    --red: #ff9aab;
  }
  * { box-sizing: border-box; }
  body {
    margin: 0;
    min-height: 100vh;
    font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    color: var(--text);
    line-height: 1.55;
    background:
      radial-gradient(ellipse at 12% 0%, rgba(89, 119, 220, .19), transparent 38rem),
      radial-gradient(ellipse at 100% 15%, rgba(88, 90, 180, .13), transparent 32rem),
      var(--bg);
  }
  button, input { font: inherit; }
  button { cursor: pointer; }
  button:focus-visible, input:focus-visible { outline: 3px solid rgba(138,167,255,.6); outline-offset: 2px; }
  .shell { width: min(1080px, calc(100% - 36px)); margin: 0 auto; }
  header { padding: 26px 0 20px; border-bottom: 1px solid rgba(130,150,190,.15); }
  .topbar { display: flex; align-items: center; justify-content: space-between; gap: 18px; }
  .brand { display: flex; align-items: center; gap: 12px; font-size: 1.3rem; font-weight: 800; letter-spacing: -.04em; }
  .brand-mark {
    display: grid; place-items: center; width: 44px; height: 44px; border-radius: 14px;
    background: linear-gradient(145deg, #a4b9ff, #6e8df4); color: #0b1223;
    box-shadow: 0 8px 28px rgba(102,136,245,.24); font-size: 22px;
  }
  .brand span { color: var(--blue); }
  .top-tag { color: var(--muted); font-size: .88rem; border: 1px solid var(--line); border-radius: 999px; padding: 7px 12px; }
  main { padding: 46px 0 54px; }
  .hero { max-width: 730px; margin-bottom: 30px; }
  .eyebrow { display: inline-flex; align-items: center; gap: 8px; color: var(--blue); font-size: .78rem; font-weight: 800; text-transform: uppercase; letter-spacing: .13em; }
  h1 { font-size: clamp(2.15rem, 5vw, 3.65rem); line-height: 1.08; letter-spacing: -.055em; margin: 13px 0 15px; }
  .hero p { max-width: 650px; color: var(--muted); font-size: 1.06rem; margin: 0; }
  .layout { display: grid; grid-template-columns: minmax(0, 1fr) minmax(300px, .82fr); gap: 22px; align-items: start; }
  .card {
    background: linear-gradient(160deg, rgba(23,33,55,.97), rgba(14,22,39,.97));
    border: 1px solid var(--line); border-radius: 22px; padding: 26px;
    box-shadow: 0 18px 55px rgba(0,0,0,.18);
  }
  .card-head { display: flex; align-items: flex-start; gap: 13px; margin-bottom: 23px; }
  .icon-box { display: grid; place-items: center; flex: 0 0 44px; height: 44px; border-radius: 13px; background: rgba(138,167,255,.12); font-size: 21px; }
  h2 { margin: 0 0 5px; font-size: 1.2rem; letter-spacing: -.025em; }
  .subhead { margin: 0; color: var(--muted); font-size: .91rem; }
  .field { margin-bottom: 16px; }
  label { display: block; font-size: .9rem; font-weight: 650; margin-bottom: 7px; }
  input[type="email"], input[type="password"] {
    display: block; width: 100%; min-height: 49px; padding: 12px 14px;
    border-radius: 11px; border: 1px solid #394966; background: #0c1425; color: var(--text);
    transition: border-color .18s, box-shadow .18s;
  }
  input::placeholder { color: #74829e; }
  input[type="email"]:focus, input[type="password"]:focus { border-color: var(--blue); box-shadow: 0 0 0 3px rgba(138,167,255,.1); outline: none; }
  .actions { display: flex; gap: 10px; margin-top: 20px; }
  .btn {
    display: inline-flex; justify-content: center; align-items: center; gap: 8px;
    min-height: 47px; padding: 11px 16px; border-radius: 11px; border: 1px solid transparent;
    font-weight: 750; transition: transform .15s, background .15s, border-color .15s;
  }
  .btn:hover:not(:disabled) { transform: translateY(-1px); }
  .btn-primary { background: linear-gradient(135deg, #a2b8ff, #7695ff); color: #0a1224; flex: 1; }
  .btn-secondary { background: #202c44; color: var(--text); border-color: #3a4967; flex: 1; }
  .btn-quiet { background: transparent; color: var(--muted); border-color: var(--line); }
  .btn:disabled { opacity: .5; cursor: not-allowed; }
  .notice { margin: 17px 0 0; padding: 12px 14px; border: 1px solid #2c3b58; background: rgba(10,17,31,.72); border-radius: 11px; color: var(--muted); font-size: .9rem; overflow-wrap: anywhere; }
  .notice[data-kind="success"] { color: var(--green); border-color: rgba(117,221,178,.28); }
  .notice[data-kind="error"] { color: var(--red); border-color: rgba(255,154,171,.28); }
  .upload-card { grid-column: 2; grid-row: 1; }
  .upload-zone {
    border: 1px dashed #4b5e83; border-radius: 15px; padding: 24px 18px; text-align: center;
    background: rgba(9,16,30,.48); margin-bottom: 15px;
  }
  .file-icon { font-size: 34px; margin-bottom: 8px; }
  .upload-zone strong { display: block; margin-bottom: 4px; }
  .upload-zone p { color: var(--muted); font-size: .88rem; margin: 0 0 15px; }
  input[type="file"] { display: block; width: 100%; color: var(--muted); font-size: .86rem; text-align: left; }
  input[type="file"]::file-selector-button {
    border: 1px solid #465879; border-radius: 8px; background: #202c44; color: var(--text);
    padding: 9px 11px; margin-right: 10px; cursor: pointer;
  }
  .upload-btn { width: 100%; margin-top: 16px; }
  .privacy { margin-top: 22px; padding: 18px 20px; display: flex; gap: 14px; align-items: flex-start; border-radius: 16px; border: 1px solid rgba(117,221,178,.2); background: rgba(28,82,68,.12); }
  .privacy-icon { font-size: 22px; }
  .privacy h3 { font-size: .97rem; margin: 0 0 4px; }
  .privacy p { color: var(--muted); font-size: .87rem; margin: 0; }
  footer { padding: 20px 0 30px; border-top: 1px solid rgba(130,150,190,.15); color: #7887a3; text-align: center; font-size: .82rem; }
  [hidden] { display: none !important; }
  @media (max-width: 760px) {
    .shell { width: min(100% - 28px, 560px); }
    header { padding-top: 18px; }
    .top-tag { font-size: .75rem; padding: 6px 9px; }
    main { padding-top: 32px; }
    .layout { grid-template-columns: 1fr; }
    .upload-card { grid-column: auto; grid-row: auto; }
    .card { padding: 21px; border-radius: 18px; }
    .actions { flex-direction: column; }
    .btn { width: 100%; }
  }
  @media (prefers-reduced-motion: reduce) { *, *::before, *::after { transition: none !important; } }
</style>
</head>
<body>
<header>
  <div class="shell topbar">
    <div class="brand"><div class="brand-mark" aria-hidden="true">🔐</div><div>Safe<span>View</span></div></div>
    <div class="top-tag">Private PowerPoint Workspace</div>
  </div>
</header>

<main class="shell">
  <section class="hero">
    <div class="eyebrow">✦ Your files, your space</div>
    <h1>Keep your presentations<br>private and secure.</h1>
    <p>Sign in to your private workspace and store PowerPoint files in one simple place, designed to feel clear on desktop and mobile.</p>
  </section>

  <div class="layout">
    <section class="card">
      <div class="card-head">
        <div class="icon-box" aria-hidden="true">👤</div>
        <div><h2>Welcome to SafeView</h2><p class="subhead">Create an account or sign in to continue.</p></div>
      </div>
      <div class="field">
        <label for="email">Email address</label>
        <input id="email" type="email" placeholder="you@example.com" autocomplete="email">
      </div>
      <div class="field">
        <label for="password">Password</label>
        <input id="password" type="password" placeholder="At least 8 characters" autocomplete="current-password">
      </div>
      <div class="actions">
        <button class="btn btn-primary" id="signup" type="button">Create account <span aria-hidden="true">→</span></button>
        <button class="btn btn-secondary" id="login" type="button">Sign in</button>
        <button class="btn btn-quiet" id="logout" type="button" hidden>Log out</button>
      </div>
      <p id="authStatus" class="notice" aria-live="polite" data-kind="info">Connecting to your secure workspace…</p>
    </section>

    <section class="card upload-card">
      <div class="card-head">
        <div class="icon-box" aria-hidden="true">📁</div>
        <div><h2>Upload a presentation</h2><p class="subhead">Keep your PowerPoint files in your private storage.</p></div>
      </div>
      <div class="upload-zone">
        <div class="file-icon" aria-hidden="true">📄</div>
        <strong>Choose a PPT or PPTX file</strong>
        <p>Maximum file size: 50 MB</p>
        <input id="ppt" type="file" accept=".ppt,.pptx,application/vnd.ms-powerpoint,application/vnd.openxmlformats-officedocument.presentationml.presentation" aria-label="Choose a PowerPoint file">
      </div>
      <button class="btn btn-primary upload-btn" id="upload" type="button" disabled>Upload presentation <span aria-hidden="true">↑</span></button>
      <p id="uploadStatus" class="notice" aria-live="polite" data-kind="info">Sign in to enable uploads.</p>
    </section>
  </div>

  <section class="privacy">
    <div class="privacy-icon" aria-hidden="true">🛡️</div>
    <div><h3>Privacy comes first</h3><p>Use a private Supabase Storage bucket with row-level security (RLS) policies that restrict access to each user’s own folder. Never expose a service-role key in browser code. A private bucket and correct policies are required for access control.</p></div>
  </section>
</main>

<footer><div class="shell">SafeView · Private PowerPoint Workspace</div></footer>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script>
"use strict";

// Replace with your Supabase project's URL and publishable key.
// Never put a Supabase service_role/secret key in frontend code.
const SUPABASE_URL = "https://ttknswttzxzsftowngkr.supabase.co";
const SUPABASE_KEY = "sb_publishable_howesfkFfffCxF1J_QiBTw_xFht3crI";

const $ = (id) => document.getElementById(id);
let client = null;
let currentUser = null;
let busy = false;

function setMessage(id, message, kind = "info") {
  const el = $(id);
  el.textContent = message;
  el.dataset.kind = kind;
}
function authMessage(message, kind) { setMessage("authStatus", message, kind); }
function uploadMessage(message, kind) { setMessage("uploadStatus", message, kind); }

function updateUI(user) {
  currentUser = user || null;
  $("signup").hidden = !!user;
  $("login").hidden = !!user;
  $("logout").hidden = !user;
  $("upload").disabled = !user || busy;
  if (user) {
    authMessage("Signed in as " + (user.email || "your account") + ".", "success");
    uploadMessage("You’re signed in. Choose a presentation to upload.", "success");
  } else {
    authMessage("You’re not signed in yet.");
    uploadMessage("Sign in to enable uploads.");
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
    authMessage("Error: " + (error?.message || "Something went wrong."), "error");
  } finally {
    setBusy(false);
  }
}
async function startApp() {
  if (!window.supabase) {
    authMessage("Couldn’t load the Supabase library. Check your internet connection and reload.", "error");
    return;
  }
  if (!SUPABASE_URL.startsWith("https://") || !SUPABASE_KEY.startsWith("sb_publishable_")) {
    authMessage("Setup needed: add your Supabase project URL and publishable key in this file.", "error");
    return;
  }
  client = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);
  const { data, error } = await client.auth.getSession();
  if (error) throw error;
  updateUI(data.session?.user || null);
  client.auth.onAuthStateChange((_event, session) => {
    updateUI(session?.user || null);
  });
}

$("signup").addEventListener("click", () => runAction(async () => {
  if (!client) throw new Error("Supabase is not configured.");
  const email = $("email").value.trim();
  const password = $("password").value;
  if (!email || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
    authMessage("Please enter a valid email address.", "error"); return;
  }
  if (password.length < 8) {
    authMessage("Your password must be at least 8 characters.", "error"); return;
  }
  authMessage("Creating your account…");
  const { data, error } = await client.auth.signUp({
    email, password,
    options: { emailRedirectTo: "https://wish7373.github.io/Safe-view-/" }
  });
  if (error) throw error;
  if (data.session) updateUI(data.session.user);
  else authMessage("Signup submitted. Check your email for a confirmation link, then sign in.", "success");
}));

$("login").addEventListener("click", () => runAction(async () => {
  if (!client) throw new Error("Supabase is not configured.");
  const email = $("email").value.trim();
  const password = $("password").value;
  if (!email || !password) {
    authMessage("Enter your email address and password.", "error"); return;
  }
  authMessage("Signing you in…");
  const { data, error } = await client.auth.signInWithPassword({ email, password });
  if (error) throw error;
  updateUI(data.user);
}));

$("logout").addEventListener("click", () => runAction(async () => {
  if (!client) throw new Error("Supabase is not configured.");
  const { error } = await client.auth.signOut();
  if (error) throw error;
  updateUI(null);
  uploadMessage("You’ve signed out.");
}));

$("upload").addEventListener("click", () => runAction(async () => {
  if (!client || !currentUser) throw new Error("Please sign in before uploading.");
  const file = $("ppt").files[0];
  if (!file) { uploadMessage("Choose a PowerPoint file first.", "error"); return; }
  if (!/\.(ppt|pptx)$/i.test(file.name)) {
    uploadMessage("Only .ppt and .pptx files are allowed.", "error"); return;
  }
  if (file.size > 50 * 1024 * 1024) {
    uploadMessage("Your file must be 50 MB or smaller.", "error"); return;
  }
  const extension = file.name.split(".").pop().toLowerCase();
  const path = currentUser.id + "/" + crypto.randomUUID() + "." + extension;
  uploadMessage("Uploading your presentation…");
  const { data, error } = await client.storage.from("private-ppts").upload(path, file, {
    upsert: false,
    contentType: file.type || "application/octet-stream"
  });
  if (error) throw error;
  uploadMessage("Upload complete. Stored at: " + data.path, "success");
}));

startApp().catch((error) => authMessage("Startup error: " + (error?.message || "Unknown error."), "error"));
</script>
</body>
</html>
