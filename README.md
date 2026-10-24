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

    <p id="authStatus" class="notice" aria
