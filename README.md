<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Aloy Bot</title>
  <meta name="description" content="Aloy Bot is a Fortnite Save the World Discord bot for mission alerts, rewards, and theater data." />
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
      --success: #00ff88;
    }

    html { scroll-behavior: smooth; }

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
      max-width: 860px;
      width: 100%;
    }

    .nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 16px;
      margin-bottom: 40px;
      opacity: 0;
      animation: fadeUp 0.6s ease forwards;
      flex-wrap: wrap;
    }

    .nav-left {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .nav-dot {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: var(--success);
      box-shadow: 0 0 10px var(--success);
    }

    .nav-label {
      font-family: 'Share Tech Mono', monospace;
      font-size: 11px;
      color: var(--muted);
      letter-spacing: 3px;
      text-transform: uppercase;
    }

    .nav-links {
      display: flex;
      gap: 12px;
      flex-wrap: wrap;
    }

    .nav-links a,
    .btn {
      text-decoration: none;
      border: 1px solid var(--border);
      background: var(--panel);
      color: var(--text);
      padding: 10px 14px;
      border-radius: 4px;
      font-family: 'Share Tech Mono', monospace;
      font-size: 11px;
      letter-spacing: 1px;
      text-transform: uppercase;
      transition: 0.2s ease;
    }

    .nav-links a:hover,
    .btn:hover {
      border-color: var(--accent);
      color: #fff;
      box-shadow: 0 0 14px rgba(0,194,255,0.12);
    }

    .hero {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 24px;
      margin-bottom: 44px;
      text-align: center;
      opacity: 0;
      animation: fadeUp 0.6s ease 0.12s forwards;
    }

    .avatar-wrap {
      position: relative;
      width: 120px;
      height: 120px;
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
      width: 120px;
      height: 120px;
      border-radius: 50%;
      background: linear-gradient(135deg, #0a1a2a, #112233);
      border: 2px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Share Tech Mono', monospace;
      font-size: 42px;
      color: var(--accent);
      overflow: hidden;
    }

    .bot-name {
      font-size: 54px;
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
      text-transform: uppercase;
    }

    .hero-text {
      max-width: 680px;
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 28px 32px;
      line-height: 1.8;
      font-size: 17px;
      position: relative;
    }

    .hero-text::before,
    .section::before,
    .feature-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, var(--accent), transparent);
    }

    .stats {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 16px;
      margin-bottom: 32px;
      opacity: 0;
      animation: fadeUp 0.6s ease 0.25s forwards;
    }

    .card {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 20px 22px;
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

    .section {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 28px 32px;
      margin-bottom: 24px;
      position: relative;
      opacity: 0;
      animation: fadeUp 0.6s ease 0.38s forwards;
    }

    .section-title {
      font-size: 11px;
      font-family: 'Share Tech Mono', monospace;
      letter-spacing: 3px;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 18px;
    }

    .section p {
      font-size: 16px;
      line-height: 1.8;
      color: var(--text);
    }

    .features {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
      margin-top: 16px;
    }

    .feature-card {
      background: #0d1116;
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 20px;
      position: relative;
    }

    .feature-card h3 {
      font-size: 18px;
      margin-bottom: 8px;
      color: #fff;
    }

    .feature-card p {
      font-size: 15px;
      line-height: 1.7;
      color: var(--text);
    }

    .cta-row {
      display: flex;
      gap: 12px;
      margin-top: 20px;
      flex-wrap: wrap;
    }

    footer {
      margin-top: 36px;
      font-family: 'Share Tech Mono', monospace;
      font-size: 11px;
      color: var(--muted);
      letter-spacing: 2px;
      text-align: center;
      opacity: 0;
      animation: fadeUp 0.6s ease 0.5s forwards;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(16px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    @media (max-width: 760px) {
      .stats { grid-template-columns: 1fr 1fr; }
      .features { grid-template-columns: 1fr; }
      .bot-name { font-size: 38px; }
    }

    @media (max-width: 500px) {
      .stats { grid-template-columns: 1fr; }
      .bot-name { font-size: 30px; }
      .section, .hero-text { padding: 22px 20px; }
    }
  </style>
</head>
<body>
  <div class="container">

    <div class="nav">
      <div class="nav-left">
        <div class="nav-dot"></div>
        <span class="nav-label">System Online — v1.0.0</span>
      </div>

      <div class="nav-links">
        <a href="./index.html">Home</a>
        <a href="./privacy.html">Privacy</a>
      </div>
    </div>

    <section class="hero">
      <div class="avatar-wrap">
        <div class="avatar">⚡</div>
      </div>

      <div>
        <div class="bot-name">Aloy Bot</div>
        <div class="bot-sub">Fortnite STW Mission Tracker</div>
      </div>

      <div class="hero-text">
        Aloy Bot is a Discord utility bot built for Fortnite Save the World players who want clean access to mission alerts, rewards, and theater information without digging through clutter. It is designed to deliver fast, readable STW data directly where your community already is.
      </div>
    </section>

    <section class="stats">
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
    </section>

    <section class="section">
      <div class="section-title">Overview</div>
      <p>
        Aloy Bot interfaces with Epic-connected game data sources to surface Fortnite Save the World mission information in a direct and structured format. The goal is simple: quick access, minimal noise, and a layout that feels polished enough to match the rest of your brand.
      </p>
    </section>

    <section class="section">
      <div class="section-title">Core Functions</div>
      <div class="features">
        <div class="feature-card">
          <h3>Mission Tracking</h3>
          <p>Surface current mission alerts, rewards, and relevant STW rotations in a readable format for Discord users.</p>
        </div>
        <div class="feature-card">
          <h3>Reward Visibility</h3>
          <p>Help users check what matters fast, including perk materials, evolution materials, and other key mission outputs.</p>
        </div>
        <div class="feature-card">
          <h3>Discord Delivery</h3>
          <p>Designed for instant use inside servers, so users do not need to leave Discord to find basic STW information.</p>
        </div>
        <div class="feature-card">
          <h3>Clean Utility Focus</h3>
          <p>Built around practical use instead of feature bloat, with a strong focus on speed, clarity, and presentation.</p>
        </div>
      </div>

      <div class="cta-row">
        <a class="btn" href="./privacy.html">View Privacy Policy</a>
      </div>
    </section>

    <footer>© 2026 Dragic — Aloy Bot — All rights reserved</footer>

  </div>
</body>
</html>
