<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Aloy Bot</title>
  <link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@400;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0a0c10;
      --panel: #0f1318;
      --border: #1e2a38;
      --accent: #00c2ff;
      --accent2: #ff6a00;
      --text: #c8d8e8;
      --muted: #4a6070;
    }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Rajdhani', sans-serif;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 60px 20px;
      overflow-x: hidden;
    }

    /* Grid bg */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image:
        linear-gradient(rgba(0,194,255,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,194,255,0.03) 1px, transparent 1px);
      background-size: 40px 40px;
      pointer-events: none;
      z-index: 0;
    }

    .container {
      position: relative;
      z-index: 1;
      max-width: 700px;
      width: 100%;
    }

    /* Top bar */
    .topbar {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 48px;
      opacity: 0;
      animation: fadeUp 0.6s ease forwards;
    }
    .topbar-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--accent); box-shadow: 0 0 8px var(--accent); }
    .topbar-label { font-family: 'Share Tech Mono', monospace; font-size: 11px; color: var(--muted); letter-spacing: 3px; text-transform: uppercase; }

    /* Avatar */
    .hero {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 24px;
      margin-bottom: 48px;
      opacity: 0;
      animation: fadeUp 0.6s ease 0.15s forwards;
    }

    .avatar-wrap {
      position: relative;
      width: 110px;
      height: 110px;
    }
    .avatar-wrap::before {
      content: '';
      position: absolute;
      inset: -6px;
      border-radius: 50%;
      border: 1px solid var(--accent);
      opacity: 0.4;
      animation: spin 8s linear infinite;
    }
    .avatar-wrap::after {
      content: '';
      position: absolute;
      inset: -14px;
      border-radius: 50%;
      border: 1px dashed var(--accent2);
      opacity: 0.2;
      animation: spin 14s linear infinite reverse;
    }
    .avatar {
      width: 110px;
      height: 110px;
      border-radius: 50%;
      background: linear-gradient(135deg, #0a1a2a, #112233);
      border: 2px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Share Tech Mono', monospace;
      font-size: 36px;
      color: var(--accent);
      overflow: hidden;
    }

    .bot-name {
      font-size: 48px;
      font-weight: 700;
      letter-spacing: 4px;
      text-transform: uppercase;
      background: linear-gradient(90deg, var(--accent), #ffffff, var(--accent2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .bot-sub {
      font-family: 'Share Tech Mono', monospace;
      font-size: 12px;
      color: var(--muted);
      letter-spacing: 2px;
    }

    /* Cards */
    .cards {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
      margin-bottom: 32px;
      opacity: 0;
      animation: fadeUp 0.6s ease 0.3s forwards;
    }

    .card {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 20px 24px;
      position: relative;
      overflow: hidden;
    }
    .card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, var(--accent), transparent);
    }
    .card-label {
      font-family: 'Share Tech Mono', monospace;
      font-size: 10px;
      color: var(--muted);
      letter-spacing: 2px;
      text-transform: uppercase;
      margin-bottom: 8px;
    }
    .card-value {
      font-size: 20px;
      font-weight: 600;
      color: #fff;
      letter-spacing: 1px;
    }

    /* Description */
    .desc {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 28px 32px;
      margin-bottom: 32px;
      line-height: 1.8;
      font-size: 16px;
      color: var(--text);
      opacity: 0;
      animation: fadeUp 0.6s ease 0.45s forwards;
      position: relative;
    }
    .desc::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, var(--accent2), transparent);
    }

    /* Privacy */
    .privacy {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 28px 32px;
      opacity: 0;
      animation: fadeUp 0.6s ease 0.6s forwards;
      position: relative;
    }
    .privacy::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, #00ff88, transparent);
    }
    .section-title {
      font-size: 11px;
      font-family: 'Share Tech Mono', monospace;
      letter-spacing: 3px;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 20px;
    }
    .privacy p {
      font-size: 15px;
      line-height: 1.8;
      color: var(--text);
      margin-bottom: 14px;
    }
    .privacy p:last-child { margin-bottom: 0; }

    /* Footer */
    footer {
      margin-top: 48px;
      font-family: 'Share Tech Mono', monospace;
      font-size: 11px;
      color: var(--muted);
      letter-spacing: 2px;
      text-align: center;
      opacity: 0;
      animation: fadeUp 0.6s ease 0.75s forwards;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(16px); }
      to { opacity: 1; transform: translateY(0); }
    }
    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    @media (max-width: 500px) {
      .cards { grid-template-columns: 1fr; }
      .bot-name { font-size: 32px; }
    }
  </style>
</head>
<body>
  <div class="container">

    <div class="topbar">
      <div class="topbar-dot"></div>
      <span class="topbar-label">System Online — v1.0.0</span>
    </div>

    <div class="hero">
      <div class="avatar-wrap">
        <div class="avatar">⚡</div>
      </div>
      <div style="text-align:center;">
        <div class="bot-name">Aloy Bot</div>
        <div class="bot-sub">// Fortnite STW Mission Tracker</div>
      </div>
    </div>

    <div class="cards">
      <div class="card">
        <div class="card-label">Creator</div>
        <div class="card-value">Dragic</div>
      </div>
      <div class="card">
        <div class="card-label">Platform</div>
        <div class="card-value">Discord</div>
      </div>
      <div class="card">
        <div class="card-label">Purpose</div>
        <div class="card-value">STW Data</div>
      </div>
      <div class="card">
        <div class="card-label">Status</div>
        <div class="card-value" style="color:#00ff88;">Active</div>
      </div>
    </div>

    <div class="desc">
      Aloy Bot is a utility bot created by Dragic for tracking Fortnite Save the World mission data.
      It interfaces with Epic Games' public APIs to retrieve mission alerts, rewards, and theater information —
      delivering clean, fast results directly in Discord.
    </div>

    <div class="privacy" id="privacy">
      <div class="section-title">// Privacy Policy</div>
      <p>Last updated: March 29, 2026</p>
      <p>
        Aloy Bot ("the Bot"), developed by Dragic, does not collect, store, or share any personal user data.
        The Bot interacts solely with Epic Games' public APIs to retrieve publicly available game information.
      </p>
      <p>
        No user credentials, personal information, account data, or message content is logged or retained by the Bot or its developer in any form.
      </p>
      <p>
        The Bot is provided as-is for informational and entertainment purposes. Use of this Bot is entirely at the user's own discretion.
        By using Aloy Bot, you agree to Epic Games' own Terms of Service and Privacy Policy.
      </p>
      <p>
        For questions or concerns, contact the developer directly via Discord.
      </p>
    </div>

    <footer>© 2026 Dragic — Aloy Bot — All rights reserved</footer>

  </div>
</body>
</html>
