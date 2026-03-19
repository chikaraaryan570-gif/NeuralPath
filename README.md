# NeuralPath
NeuralPath is an AI-powered platform that helps students overcome career confusion by providing personalized learning roadmaps based on their goals, skills, and available time. It breaks down complex career paths into clear, step-by-step plans, making learning more structured and efficient.
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NeuralPath — AI Career Roadmap</title>
<link href="https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;0,700;1,400&family=Nunito:wght@400;500;600;700;800&family=Fira+Code:wght@400;500&display=swap" rel="stylesheet">
<style>
  @import url('https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;0,700;1,400&family=Nunito:wght@400;500;600;700;800&family=Fira+Code:wght@400;500&display=swap');

  :root {
    --bg:     #05080f;
    --s1:     #0a0e1a;
    --s2:     #0f1525;
    --s3:     #161d30;
    --border: rgba(60,120,255,0.15);
    --a:      #3b82f6;
    --a2:     #60a5fa;
    --g:      #34d399;
    --r:      #f87171;
    --y:      #fbbf24;
    --b:      #818cf8;
    --t:      #e2e8f8;
    --t2:     #6b7fa8;
    --t3:     #2a3550;
    --shadow: rgba(0,0,0,0.5);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Nunito', sans-serif;
    background: var(--bg);
    color: var(--t);
    min-height: 100vh;
    /* subtle paper grain texture */
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='300' height='300'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='300' height='300' filter='url(%23n)' opacity='0.03'/%3E%3C/svg%3E");
  }

  /* LAYOUT */
  #app { display: flex; min-height: 100vh; }
  #sidebar {
    width: 230px; flex-shrink: 0;
    background: var(--s1);
    border-right: 1px solid var(--border);
    display: flex; flex-direction: column;
    position: fixed; top: 0; left: 0; bottom: 0;
    z-index: 10; overflow: hidden;
    box-shadow: 2px 0 20px rgba(0,0,0,0.3);
  }
  #content { margin-left: 230px; flex: 1; }

  /* SIDEBAR LOGO */
  .logo {
    padding: 22px 20px 18px;
    border-bottom: 1px solid var(--border);
    flex-shrink: 0;
  }
  .logo h1 {
    font-family: 'Lora', serif;
    font-size: 1.25rem;
    font-weight: 700;
    color: var(--a);
    letter-spacing: -0.01em;
  }
  .logo p {
    font-family: 'Fira Code', monospace;
    font-size: 0.55rem;
    color: var(--t3);
    margin-top: 3px;
    letter-spacing: 0.14em;
    text-transform: uppercase;
  }

  /* NAV */
  .nav { padding: 10px 8px; flex: 1; overflow-y: auto; min-height: 0; }
  .nav-group-label {
    font-family: 'Fira Code', monospace;
    font-size: 0.55rem;
    color: var(--t3);
    text-transform: uppercase;
    letter-spacing: 0.18em;
    padding: 14px 10px 5px;
  }
  .nav-btn {
    display: flex; align-items: center; gap: 10px;
    width: 100%; padding: 9px 12px; border-radius: 10px;
    border: none; background: none;
    color: var(--t2); cursor: pointer;
    font-family: 'Nunito', sans-serif;
    font-size: 0.87rem; font-weight: 600;
    text-align: left; transition: all 0.18s;
  }
  .nav-btn:hover {
    background: var(--s2);
    color: var(--t);
    transform: translateX(2px);
  }
  .nav-btn.active {
    background: rgba(59,130,246,0.12);
    color: var(--a2);
    border-left: 3px solid var(--a);
    padding-left: 9px;
  }
  .nav-ico {
    width: 15px; height: 15px; flex-shrink: 0;
    color: var(--t3); transition: color 0.18s;
  }
  .nav-btn:hover .nav-ico { color: var(--t2); }
  .nav-btn.active .nav-ico { color: var(--a); }

  /* LEVEL BADGE */
  .level-badge {
    margin: 0 10px 10px;
    padding: 13px 14px;
    background: linear-gradient(135deg, rgba(59,130,246,0.1), rgba(96,165,250,0.07));
    border: 1px solid rgba(59,130,246,0.28);
    border-radius: 14px;
    display: flex; align-items: center; gap: 11px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.3);
  }
  .lvl-icon { font-size: 1.7rem; line-height: 1; flex-shrink: 0; }
  .lvl-info { flex: 1; min-width: 0; }
  .lvl-name {
    font-family: 'Lora', serif;
    font-size: 0.95rem; font-weight: 700;
    color: var(--a2);
  }
  .lvl-num {
    font-family: 'Fira Code', monospace;
    font-size: 0.6rem; color: var(--t2);
    margin-top: 2px;
  }
  .lvl-xp-bar {
    height: 4px; background: rgba(255,255,255,0.08);
    border-radius: 4px; margin-top: 7px; overflow: hidden;
  }
  .lvl-xp-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--a), var(--a2));
    border-radius: 4px; transition: width 0.8s ease;
  }

  /* SIDEBAR BOTTOM */
  .sidebar-bottom { padding: 12px 14px 14px; border-top: 1px solid var(--border); flex-shrink: 0; }
  .xp-row {
    display: flex; justify-content: space-between; align-items: center;
    font-size: 0.72rem; color: var(--t2); margin-bottom: 5px;
  }
  .xp-row span:last-child {
    font-family: 'Fira Code', monospace; font-size: 0.65rem; color: var(--a);
  }
  .xp-track { height: 5px; background: var(--s3); border-radius: 5px; overflow: hidden; }
  .xp-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--a), var(--a2));
    transition: width 0.8s;
  }
  .streak-text {
    font-family: 'Fira Code', monospace;
    font-size: 0.68rem; color: var(--y); margin-top: 7px;
  }

  /* TOPBAR */
  .topbar {
    position: sticky; top: 0; z-index: 5;
    background: rgba(5,8,15,0.95);
    backdrop-filter: blur(14px);
    border-bottom: 1px solid var(--border);
    padding: 14px 30px;
    display: flex; justify-content: space-between; align-items: center;
    box-shadow: 0 1px 20px rgba(0,0,0,0.4);
  }
  .topbar h2 {
    font-family: 'Lora', serif;
    font-size: 1.05rem; font-weight: 600;
    color: var(--t);
    letter-spacing: -0.01em;
  }
  .chip {
    font-family: 'Fira Code', monospace; font-size: 0.6rem;
    padding: 4px 10px; border-radius: 20px;
    border: 1px solid var(--border); color: var(--t2);
  }
  .chip.on {
    border-color: rgba(107,191,142,0.4);
    color: var(--g);
    background: rgba(107,191,142,0.08);
  }

  /* OPTIONS BUTTON */
  .opts-wrap { position: relative; }
  .opts-btn {
    width: 32px; height: 32px; border-radius: 9px;
    background: var(--s2); border: 1px solid var(--border);
    color: var(--t2); cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    font-size: 1.1rem; font-weight: 700; transition: all 0.15s;
    font-family: 'Fira Code', monospace; line-height: 1;
  }
  .opts-btn:hover { background: var(--s3); color: var(--t); border-color: rgba(59,130,246,0.4); }
  .opts-btn.open { background: rgba(59,130,246,0.12); color: var(--a2); border-color: rgba(59,130,246,0.5); }
  .opts-panel {
    display: none;
    position: absolute; right: 0; top: 38px; z-index: 999;
    background: var(--s2); border: 1px solid var(--border); border-radius: 12px;
    padding: 6px; min-width: 200px;
    box-shadow: 0 16px 48px rgba(0,0,0,0.5);
    animation: up 0.15s ease;
  }
  .opts-panel.open { display: block; }
  .opt-item {
    display: flex; align-items: center; gap: 9px;
    padding: 9px 12px; border-radius: 8px; cursor: pointer;
    font-size: 0.84rem; font-weight: 600; color: var(--t2);
    transition: all 0.1s; white-space: nowrap; user-select: none;
  }
  .opt-item:hover { background: var(--s3); color: var(--t); }
  .opt-item.danger { color: var(--r); }
  .opt-item.danger:hover { background: rgba(224,112,112,0.1); }
  .opt-sep { border-top: 1px solid var(--border); margin: 4px 3px; }

  /* LEVEL UP TOAST */
  #lvlToast {
    position: fixed; top: 22px; left: 50%;
    transform: translateX(-50%) translateY(-90px);
    z-index: 9999;
    background: linear-gradient(135deg, var(--a), var(--a2));
    color: #fff; padding: 12px 26px; border-radius: 40px;
    font-family: 'Lora', serif; font-size: 0.92rem; font-weight: 700;
    box-shadow: 0 8px 32px rgba(59,130,246,0.4);
    transition: transform 0.4s cubic-bezier(0.175,0.885,0.32,1.275);
    pointer-events: none; white-space: nowrap;
  }
  #lvlToast.show { transform: translateX(-50%) translateY(0); }

  /* PAGES */
  .page { display: none; padding: 32px 30px; }
  .page.show { display: block; animation: up 0.28s ease; }
  @keyframes up { from { opacity:0; transform: translateY(8px); } to { opacity:1; transform: translateY(0); } }

  /* CARDS */
  .card {
    background: var(--s1);
    border: 1px solid var(--border);
    border-radius: 16px; padding: 22px;
    box-shadow: 0 2px 16px var(--shadow);
  }
  .card.glow { position: relative; overflow: hidden; }
  .card.glow::before {
    content: '';
    position: absolute; top: 0; left: 0; right: 0; height: 1px;
    background: linear-gradient(90deg, transparent, var(--a), transparent);
    opacity: 0.6;
  }

  /* TYPOGRAPHY */
  .page-title {
    font-family: 'Lora', serif;
    font-size: 1.5rem; font-weight: 700;
    margin-bottom: 5px;
    color: var(--t);
    letter-spacing: -0.02em;
  }
  .page-sub { color: var(--t2); font-size: 0.87rem; margin-bottom: 24px; line-height: 1.6; }

  /* GRID */
  .g2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  .g3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 14px; }
  .g4 { display: grid; grid-template-columns: repeat(4,1fr); gap: 14px; }

  /* FORM */
  .label { display: block; font-size: 0.78rem; font-weight: 700; color: var(--t2); margin-bottom: 6px; letter-spacing: 0.02em; }
  input, select, textarea {
    width: 100%; background: var(--s2);
    border: 1px solid var(--border);
    color: var(--t); border-radius: 10px; padding: 11px 14px;
    font-family: 'Nunito', sans-serif; font-size: 0.9rem;
    outline: none; transition: border 0.18s, box-shadow 0.18s;
  }
  input:focus, select:focus, textarea:focus {
    border-color: rgba(59,130,246,0.6);
    box-shadow: 0 0 0 3px rgba(59,130,246,0.1);
  }
  input::placeholder { color: var(--t3); }
  select option { background: var(--s2); }
  textarea { resize: vertical; min-height: 78px; }

  /* BUTTONS */
  .btn {
    display: inline-flex; align-items: center; gap: 7px;
    padding: 10px 20px; border-radius: 10px; border: none; cursor: pointer;
    font-family: 'Nunito', sans-serif; font-size: 0.88rem; font-weight: 700;
    transition: all 0.18s; letter-spacing: 0.01em;
  }
  .btn-p {
    background: linear-gradient(135deg, var(--a), var(--a2));
    color: #fff;
    box-shadow: 0 3px 16px rgba(59,130,246,0.3);
  }
  .btn-p:hover { transform: translateY(-2px); box-shadow: 0 6px 24px rgba(59,130,246,0.45); }
  .btn-p:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }
  .btn-g {
    background: var(--s2); color: var(--t2);
    border: 1px solid var(--border);
  }
  .btn-g:hover { background: var(--s3); color: var(--t); }

  /* TAGS */
  .tag {
    display: inline-block; font-family: 'Fira Code', monospace; font-size: 0.6rem;
    padding: 2px 9px; border-radius: 20px; margin: 2px; border: 1px solid;
  }
  .tb { color: var(--b);  border-color: rgba(106,159,216,0.3); background: rgba(106,159,216,0.1); }
  .tp { color: #c4a0f0; border-color: rgba(196,160,240,0.3); background: rgba(196,160,240,0.1); }
  .tg { color: var(--g);  border-color: rgba(107,191,142,0.3); background: rgba(107,191,142,0.08); }
  .tr { color: var(--r);  border-color: rgba(224,112,112,0.3); background: rgba(224,112,112,0.08); }
  .to { color: var(--y);  border-color: rgba(240,200,74,0.3);  background: rgba(240,200,74,0.08); }

  /* STAT CARD */
  .stat {
    background: var(--s1); border: 1px solid var(--border);
    border-radius: 14px; padding: 18px;
    box-shadow: 0 2px 10px var(--shadow);
  }
  .stat-val { font-family: 'Lora', serif; font-size: 2rem; font-weight: 700; }
  .stat-lbl { font-family: 'Fira Code', monospace; font-size: 0.62rem; color: var(--t2); margin-top: 4px; letter-spacing: 0.05em; }

  /* LOADER */
  .loader {
    display: none; align-items: center; gap: 10px; padding: 16px 0;
    font-family: 'Fira Code', monospace; font-size: 0.76rem; color: var(--t2);
  }
  .loader.show { display: flex; }
  .dots { display: flex; gap: 5px; }
  .dots span {
    width: 7px; height: 7px; border-radius: 50%;
    background: var(--a); animation: bop 1.2s infinite;
  }
  .dots span:nth-child(2) { animation-delay: 0.2s; }
  .dots span:nth-child(3) { animation-delay: 0.4s; }
  @keyframes bop { 0%,80%,100%{transform:translateY(0)} 40%{transform:translateY(-7px)} }

  /* ERROR BOX */
  .errbox {
    display: none;
    background: rgba(224,112,112,0.07);
    border: 1px solid rgba(224,112,112,0.25);
    border-radius: 10px; padding: 14px;
    color: var(--r); font-size: 0.85rem;
    line-height: 1.65; margin-top: 12px;
  }
  .errbox.show { display: block; }

  /* AI OUTPUT */
  .ai-out {
    background: var(--s2); border: 1px solid var(--border); border-radius: 10px;
    padding: 18px; white-space: pre-wrap;
    font-family: 'Fira Code', monospace; font-size: 0.82rem; line-height: 1.8;
    max-height: 480px; overflow-y: auto;
    color: var(--t);
  }

  /* ROADMAP NODES */
  .rnode { display: flex; gap: 14px; padding: 16px 0; position: relative; }
  .rnode:not(:last-child)::after {
    content:''; position:absolute; left:16px; top:50px; bottom:-16px;
    width: 2px;
    background: linear-gradient(to bottom, var(--border), transparent);
  }
  .rcircle {
    width:34px; height:34px; border-radius:50%;
    display:flex; align-items:center; justify-content:center;
    font-family:'Fira Code',monospace; font-weight:700; font-size:0.78rem;
    flex-shrink:0; border:2px solid; position:relative; z-index:1;
  }
  .rc-done { border-color:var(--g); color:var(--g); background:rgba(107,191,142,0.1); }
  .rc-active {
    border-color:var(--a); color:var(--a2); background:rgba(59,130,246,0.1);
    box-shadow: 0 0 14px rgba(59,130,246,0.3);
  }
  .rc-pend { border-color:var(--t3); color:var(--t3); background:var(--s2); }
  .rbody { flex:1; }
  .rtitle { font-weight:700; font-size:0.95rem; margin-bottom:4px; font-family:'Nunito',sans-serif; }
  .rdesc { font-size:0.8rem; color:var(--t2); line-height:1.55; }

  /* PHASE PROGRESS BAR */
  .phase-pbar { height: 5px; background: var(--s3); border-radius: 5px; margin-top: 9px; overflow: hidden; }
  .phase-pbar-fill {
    height: 100%; border-radius: 5px;
    background: linear-gradient(90deg, var(--a), var(--a2));
    transition: width 0.8s ease;
  }
  .phase-pbar-lbl {
    font-family: 'Fira Code', monospace; font-size: 0.58rem;
    color: var(--t3); margin-top: 4px;
  }

  /* COMPLETE BUTTON */
  .btn-done {
    padding: 4px 12px; font-size: 0.72rem; border-radius: 8px;
    cursor: pointer; transition: all 0.15s;
    border: 1px solid rgba(107,191,142,0.35);
    background: rgba(107,191,142,0.08); color: var(--g);
    font-family: 'Nunito', sans-serif; font-weight: 700;
  }
  .btn-done:hover { background: rgba(107,191,142,0.2); }
  .btn-done:disabled { opacity: 0.4; cursor: not-allowed; }

  /* PROGRESS RING */
  .prog-wrap { display: flex; align-items: center; gap: 24px; padding: 6px 0; }
  .prog-ring-svg { flex-shrink: 0; }
  .prog-ring-info { flex: 1; }
  .prog-ring-info .prow { display: flex; gap: 24px; flex-wrap: wrap; }
  .prog-ring-info .pitem .plbl { font-size: 0.72rem; color: var(--t2); margin-bottom: 3px; }
  .prog-ring-info .pitem .pval { font-family: 'Lora', serif; font-size: 1rem; font-weight: 700; }

  /* QUIZ OPTIONS */
  .qopt {
    display:flex; align-items:center; gap:10px;
    padding:12px 15px; margin-bottom:8px;
    background:var(--s2); border:1px solid var(--border);
    border-radius:12px; cursor:pointer; font-size:0.89rem;
    transition:all 0.18s; user-select:none; font-weight:600;
  }
  .qopt:hover { border-color:rgba(59,130,246,0.45); background:rgba(59,130,246,0.07); }
  .qopt.ok { border-color:var(--g); background:rgba(107,191,142,0.09); pointer-events:none; }
  .qopt.no { border-color:var(--r); background:rgba(224,112,112,0.09); pointer-events:none; }
  .qopt.done { pointer-events:none; }
  .qkey {
    width:26px; height:26px; border-radius:8px; background:var(--s3);
    display:flex; align-items:center; justify-content:center;
    font-family:'Fira Code',monospace; font-size:0.68rem; font-weight:600; flex-shrink:0;
  }

  /* TIMER */
  .tring { width:46px; height:46px; position:relative; flex-shrink:0; }
  .tring svg { transform:rotate(-90deg); }
  .tring circle { fill:none; stroke-width:4; }
  .t-track { stroke:var(--s3); }
  .t-prog { stroke:var(--a); stroke-linecap:round; transition:stroke-dashoffset 1s linear; }
  .tnum {
    position:absolute; inset:0; display:flex; align-items:center; justify-content:center;
    font-family:'Fira Code',monospace; font-size:0.68rem; font-weight:700;
  }

  /* TABS */
  .tabs {
    display:flex; gap:2px; border-bottom:1px solid var(--border);
    margin-bottom:20px; overflow-x:auto;
  }
  .tab {
    padding:9px 16px; font-size:0.84rem; font-weight:600;
    cursor:pointer; color:var(--t2);
    border-bottom:2px solid transparent; white-space:nowrap; transition:all 0.15s;
    font-family:'Nunito',sans-serif;
  }
  .tab:hover { color:var(--t); }
  .tab.on { color:var(--a); border-bottom-color:var(--a); }

  /* LANG BUTTONS */
  .langbtn {
    padding:7px 13px; border-radius:8px; border:1px solid var(--border);
    background:var(--s2); color:var(--t2); cursor:pointer;
    font-family:'Fira Code',monospace; font-size:0.72rem; transition:all 0.15s;
  }
  .langbtn:hover, .langbtn.on {
    border-color:rgba(59,130,246,0.5); color:var(--a2);
    background:rgba(59,130,246,0.1);
  }
  .langrow { display:flex; flex-wrap:wrap; gap:7px; margin-bottom:18px; }

  /* VIDEO CARDS */
  .yt-card {
    display:flex; gap:13px; text-decoration:none;
    background:var(--s2); border:1px solid var(--border); border-radius:14px; padding:14px;
    transition:all 0.18s; cursor:pointer;
  }
  .yt-card:hover {
    border-color:rgba(59,130,246,0.35);
    transform:translateY(-2px);
    box-shadow: 0 6px 20px var(--shadow);
  }
  .yt-thumb {
    width:78px; height:54px; border-radius:8px; background:var(--s3);
    display:flex; align-items:center; justify-content:center; font-size:1.4rem; flex-shrink:0;
  }

  /* MISC */
  .row { display:flex; align-items:center; }
  .between { display:flex; justify-content:space-between; align-items:center; }
  .wrap { flex-wrap:wrap; }
  .gap8{gap:8px;} .gap10{gap:10px;} .gap12{gap:12px;} .gap14{gap:14px;} .gap16{gap:16px;}
  .mb8{margin-bottom:8px;} .mb12{margin-bottom:12px;}
  .mb16{margin-bottom:16px;} .mb20{margin-bottom:20px;} .mb24{margin-bottom:24px;}
  .mt12{margin-top:12px;} .mt16{margin-top:16px;}
  .sm{font-size:0.82rem;} .xs{font-family:'Fira Code',monospace;font-size:0.7rem;}
  .muted{color:var(--t2);}
  .ac{color:var(--a);} .gc{color:var(--g);} .rc2{color:var(--r);}
  .bold{font-weight:700;}
  .mono{font-family:'Fira Code',monospace;}
  .hr{border:none;border-top:1px solid var(--border);margin:18px 0;}

  ::-webkit-scrollbar { width:5px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--s3); border-radius:3px; }
  ::-webkit-scrollbar-thumb:hover { background: var(--t3); }

  /* ===== AUTH SCREEN ===== */
  #authScreen {
    position: fixed; inset: 0; z-index: 9000;
    background: var(--bg);
    display: flex; align-items: center; justify-content: center;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='300' height='300'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='300' height='300' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
  }
  /* ===== AUTH SCREEN ===== */
  #authScreen {
    position: fixed; inset: 0; z-index: 9000;
    background: var(--bg);
    align-items: center; justify-content: center;
  }
  #authScreen .auth-glow {
    position: absolute; width: 600px; height: 600px; border-radius: 50%;
    background: radial-gradient(circle, rgba(59,130,246,0.1) 0%, transparent 70%);
    top: 50%; left: 50%; transform: translate(-50%,-50%);
    pointer-events: none;
  }
  .auth-box {
    width: 420px; max-width: 92vw;
    background: var(--s1);
    border: 1px solid rgba(59,130,246,0.2);
    border-radius: 24px;
    padding: 40px 36px;
    box-shadow: 0 24px 80px rgba(0,0,0,0.6), 0 0 0 1px rgba(59,130,246,0.05);
    position: relative; z-index: 1;
    animation: up 0.35s cubic-bezier(0.175,0.885,0.32,1.275);
  }
  .auth-logo {
    display: flex; align-items: center; gap: 10px;
    margin-bottom: 6px;
  }
  .auth-logo h1 {
    font-family: 'Lora', serif; font-size: 1.6rem; font-weight: 700;
    color: var(--a2);
  }
  .auth-tagline {
    font-family: 'Fira Code', monospace; font-size: 0.6rem;
    color: var(--t3); letter-spacing: 0.14em; text-transform: uppercase;
    margin-bottom: 28px;
  }

  /* AUTH TAB SWITCHER — pill style */
  .auth-tabs {
    display: flex; gap: 0; margin-bottom: 26px;
    border: 1px solid var(--border); border-radius: 12px;
    overflow: hidden; background: var(--s2);
  }
  .auth-tab {
    flex: 1; padding: 10px 0; border: none;
    background: transparent; cursor: pointer;
    font-family: 'Nunito', sans-serif; font-size: 0.88rem; font-weight: 700;
    color: var(--t3); transition: all 0.2s; letter-spacing: 0.01em;
  }
  .auth-tab.active {
    background: var(--a);
    color: #fff;
    box-shadow: inset 0 0 0 1px rgba(255,255,255,0.1);
  }
  .auth-tab:not(.active):hover { color: var(--t2); background: var(--s3); }

  /* AUTH FIELD */
  .auth-field { margin-bottom: 16px; }
  .auth-field label {
    display: block; font-size: 0.76rem; font-weight: 700;
    color: var(--t2); margin-bottom: 6px; letter-spacing: 0.03em;
  }
  .auth-field input {
    width: 100%; background: var(--s2);
    border: 1.5px solid var(--border);
    color: var(--t); border-radius: 12px; padding: 13px 16px;
    font-family: 'Nunito', sans-serif; font-size: 0.93rem;
    outline: none; transition: border 0.18s, box-shadow 0.18s;
  }
  .auth-field input:focus {
    border-color: rgba(59,130,246,0.6);
    box-shadow: 0 0 0 3px rgba(59,130,246,0.12);
  }
  .auth-field input::placeholder { color: var(--t3); }

  /* AUTH SUBMIT — full-width with arrow */
  .auth-submit {
    width: 100%; padding: 14px; margin-top: 8px;
    border-radius: 12px; border: none; cursor: pointer;
    background: linear-gradient(135deg, #2563eb, #3b82f6);
    color: #fff;
    font-family: 'Nunito', sans-serif; font-size: 0.95rem; font-weight: 800;
    letter-spacing: 0.03em; transition: all 0.2s;
    box-shadow: 0 4px 20px rgba(59,130,246,0.35);
    display: flex; align-items: center; justify-content: center; gap: 8px;
  }
  .auth-submit:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 32px rgba(59,130,246,0.5);
    background: linear-gradient(135deg, #1d4ed8, #2563eb);
  }
  .auth-err {
    background: rgba(248,113,113,0.09); border: 1px solid rgba(248,113,113,0.3);
    border-radius: 10px; padding: 11px 14px; color: var(--r);
    font-size: 0.83rem; margin-top: 12px; display: none;
  }
  .auth-err.show { display: block; }
  .auth-footer {
    text-align: center; margin-top: 20px;
    font-size: 0.75rem; color: var(--t3);
    font-family: 'Fira Code', monospace;
  }

  /* ===== USER PILL — topbar ===== */
  .user-pill {
    display: flex; align-items: center; gap: 8px;
    padding: 4px 10px 4px 4px;
    background: var(--s2);
    border: 1px solid rgba(59,130,246,0.2);
    border-radius: 40px;
    cursor: default;
    transition: border-color 0.15s;
  }
  .user-pill:hover { border-color: rgba(59,130,246,0.4); }
  .user-avatar {
    width: 28px; height: 28px; border-radius: 50%;
    background: linear-gradient(135deg, #2563eb, #60a5fa);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Nunito', sans-serif; font-size: 0.72rem; font-weight: 800;
    color: #fff; flex-shrink: 0;
    box-shadow: 0 0 8px rgba(59,130,246,0.4);
  }
  .user-name-display {
    font-family: 'Nunito', sans-serif; font-size: 0.82rem; font-weight: 700;
    color: var(--t); max-width: 90px;
    overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
  }

  /* ===== LOGOUT BUTTON — topbar ===== */
  .logout-btn {
    display: flex; align-items: center; gap: 6px;
    padding: 7px 14px; border-radius: 10px;
    border: 1px solid rgba(59,130,246,0.25);
    background: rgba(59,130,246,0.08);
    color: var(--a2);
    cursor: pointer;
    font-family: 'Nunito', sans-serif; font-size: 0.8rem; font-weight: 700;
    transition: all 0.18s; letter-spacing: 0.01em;
  }
  .logout-btn:hover {
    background: rgba(248,113,113,0.12);
    border-color: rgba(248,113,113,0.4);
    color: var(--r);
  }
  .logout-btn svg { transition: transform 0.18s; }
  .logout-btn:hover svg { transform: translateX(2px); }
</style>
</head>
<body>

<!-- ===== AUTH SCREEN ===== -->
<div id="authScreen" style="display:flex; opacity:1; position:fixed; inset:0; z-index:9999; background:#05080f; align-items:center; justify-content:center;">
  <div style="position:absolute;width:700px;height:700px;border-radius:50%;background:radial-gradient(circle,rgba(59,130,246,0.12) 0%,transparent 70%);top:50%;left:50%;transform:translate(-50%,-50%);pointer-events:none;"></div>

  <div style="width:440px;max-width:94vw;background:#0a0e1a;border:1px solid rgba(59,130,246,0.22);border-radius:24px;padding:38px 36px;box-shadow:0 24px 80px rgba(0,0,0,0.7);position:relative;z-index:1;">

    <!-- Logo -->
    <div style="display:flex;align-items:center;gap:10px;margin-bottom:4px;">
      <svg width="30" height="30" viewBox="0 0 28 28" fill="none">
        <circle cx="14" cy="14" r="13" fill="url(#ag1)" opacity="0.15"/>
        <circle cx="14" cy="5" r="2.5" fill="url(#ag1)"/>
        <circle cx="5" cy="14" r="2.5" fill="url(#ag1)"/>
        <circle cx="23" cy="14" r="2.5" fill="url(#ag1)"/>
        <circle cx="14" cy="23" r="2.5" fill="url(#ag1)"/>
        <circle cx="14" cy="14" r="3" fill="url(#ag1)"/>
        <line x1="14" y1="7.5" x2="14" y2="11" stroke="#60a5fa" stroke-width="1.2" opacity="0.8"/>
        <line x1="14" y1="17" x2="14" y2="20.5" stroke="#60a5fa" stroke-width="1.2" opacity="0.8"/>
        <line x1="7.5" y1="14" x2="11" y2="14" stroke="#60a5fa" stroke-width="1.2" opacity="0.8"/>
        <line x1="17" y1="14" x2="20.5" y2="14" stroke="#60a5fa" stroke-width="1.2" opacity="0.8"/>
        <defs>
          <linearGradient id="ag1" x1="0" y1="0" x2="1" y2="1">
            <stop offset="0%" stop-color="#3b82f6"/>
            <stop offset="100%" stop-color="#60a5fa"/>
          </linearGradient>
        </defs>
      </svg>
      <span style="font-family:'Lora',serif;font-size:1.55rem;font-weight:700;color:#60a5fa;">NeuralPath</span>
    </div>
    <div style="font-family:'Fira Code',monospace;font-size:0.58rem;color:#2a3550;letter-spacing:0.14em;text-transform:uppercase;margin-bottom:26px;">Your AI Career Roadmap</div>

    <!-- Tab switcher -->
    <div style="display:flex;background:#0f1525;border:1px solid rgba(59,130,246,0.15);border-radius:12px;overflow:hidden;margin-bottom:24px;">
      <button id="tabLogin" onclick="switchAuthTab('login')"
        style="flex:1;padding:11px 0;border:none;cursor:pointer;font-family:'Nunito',sans-serif;font-size:0.9rem;font-weight:800;letter-spacing:0.02em;transition:all 0.2s;background:#3b82f6;color:#fff;">
        Sign In
      </button>
      <button id="tabSignup" onclick="switchAuthTab('signup')"
        style="flex:1;padding:11px 0;border:none;cursor:pointer;font-family:'Nunito',sans-serif;font-size:0.9rem;font-weight:800;letter-spacing:0.02em;transition:all 0.2s;background:transparent;color:#6b7fa8;">
        Sign Up
      </button>
    </div>

    <!-- LOGIN FORM -->
    <div id="loginForm">
      <div style="margin-bottom:14px;">
        <label style="display:block;font-family:'Nunito',sans-serif;font-size:0.78rem;font-weight:700;color:#6b7fa8;margin-bottom:6px;">Email or Username</label>
        <input type="text" id="loginId" placeholder="you@example.com" autocomplete="username"
          style="width:100%;background:#0f1525;border:1.5px solid rgba(59,130,246,0.18);color:#e2e8f8;border-radius:12px;padding:13px 16px;font-family:'Nunito',sans-serif;font-size:0.92rem;outline:none;"
          onfocus="this.style.borderColor='rgba(59,130,246,0.6)';this.style.boxShadow='0 0 0 3px rgba(59,130,246,0.12)'"
          onblur="this.style.borderColor='rgba(59,130,246,0.18)';this.style.boxShadow='none'"
          onkeydown="if(event.key==='Enter')doLogin()">
      </div>
      <div style="margin-bottom:6px;">
        <label style="display:block;font-family:'Nunito',sans-serif;font-size:0.78rem;font-weight:700;color:#6b7fa8;margin-bottom:6px;">Password</label>
        <input type="password" id="loginPw" placeholder="••••••••" autocomplete="current-password"
          style="width:100%;background:#0f1525;border:1.5px solid rgba(59,130,246,0.18);color:#e2e8f8;border-radius:12px;padding:13px 16px;font-family:'Nunito',sans-serif;font-size:0.92rem;outline:none;"
          onfocus="this.style.borderColor='rgba(59,130,246,0.6)';this.style.boxShadow='0 0 0 3px rgba(59,130,246,0.12)'"
          onblur="this.style.borderColor='rgba(59,130,246,0.18)';this.style.boxShadow='none'"
          onkeydown="if(event.key==='Enter')doLogin()">
      </div>
      <button onclick="doLogin()"
        style="width:100%;margin-top:16px;padding:14px;border-radius:12px;border:none;cursor:pointer;background:linear-gradient(135deg,#2563eb,#3b82f6);color:#fff;font-family:'Nunito',sans-serif;font-size:0.95rem;font-weight:800;letter-spacing:0.03em;box-shadow:0 4px 20px rgba(59,130,246,0.35);display:flex;align-items:center;justify-content:center;gap:8px;transition:all 0.2s;"
        onmouseover="this.style.transform='translateY(-2px)';this.style.boxShadow='0 8px 28px rgba(59,130,246,0.5)'"
        onmouseout="this.style.transform='';this.style.boxShadow='0 4px 20px rgba(59,130,246,0.35)'">
        Sign In
        <svg width="15" height="15" viewBox="0 0 16 16" fill="none"><path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
      <div id="loginErr" style="display:none;margin-top:12px;padding:11px 14px;border-radius:10px;font-family:'Nunito',sans-serif;font-size:0.83rem;"></div>
    </div>

    <!-- SIGNUP FORM -->
    <div id="signupForm" style="display:none;">
      <div style="margin-bottom:13px;">
        <label style="display:block;font-family:'Nunito',sans-serif;font-size:0.78rem;font-weight:700;color:#6b7fa8;margin-bottom:6px;">Your Name</label>
        <input type="text" id="signupName" placeholder="e.g. Arjun Sharma" autocomplete="name"
          style="width:100%;background:#0f1525;border:1.5px solid rgba(59,130,246,0.18);color:#e2e8f8;border-radius:12px;padding:13px 16px;font-family:'Nunito',sans-serif;font-size:0.92rem;outline:none;"
          onfocus="this.style.borderColor='rgba(59,130,246,0.6)';this.style.boxShadow='0 0 0 3px rgba(59,130,246,0.12)'"
          onblur="this.style.borderColor='rgba(59,130,246,0.18)';this.style.boxShadow='none'">
      </div>
      <div style="margin-bottom:13px;">
        <label style="display:block;font-family:'Nunito',sans-serif;font-size:0.78rem;font-weight:700;color:#6b7fa8;margin-bottom:6px;">Username</label>
        <input type="text" id="signupUser" placeholder="e.g. arjun_dev" autocomplete="username"
          style="width:100%;background:#0f1525;border:1.5px solid rgba(59,130,246,0.18);color:#e2e8f8;border-radius:12px;padding:13px 16px;font-family:'Nunito',sans-serif;font-size:0.92rem;outline:none;"
          onfocus="this.style.borderColor='rgba(59,130,246,0.6)';this.style.boxShadow='0 0 0 3px rgba(59,130,246,0.12)'"
          onblur="this.style.borderColor='rgba(59,130,246,0.18)';this.style.boxShadow='none'">
      </div>
      <div style="margin-bottom:13px;">
        <label style="display:block;font-family:'Nunito',sans-serif;font-size:0.78rem;font-weight:700;color:#6b7fa8;margin-bottom:6px;">Email</label>
        <input type="email" id="signupEmail" placeholder="you@example.com" autocomplete="email"
          style="width:100%;background:#0f1525;border:1.5px solid rgba(59,130,246,0.18);color:#e2e8f8;border-radius:12px;padding:13px 16px;font-family:'Nunito',sans-serif;font-size:0.92rem;outline:none;"
          onfocus="this.style.borderColor='rgba(59,130,246,0.6)';this.style.boxShadow='0 0 0 3px rgba(59,130,246,0.12)'"
          onblur="this.style.borderColor='rgba(59,130,246,0.18)';this.style.boxShadow='none'">
      </div>
      <div style="margin-bottom:6px;">
        <label style="display:block;font-family:'Nunito',sans-serif;font-size:0.78rem;font-weight:700;color:#6b7fa8;margin-bottom:6px;">Password</label>
        <input type="password" id="signupPw" placeholder="Min. 6 characters" autocomplete="new-password"
          style="width:100%;background:#0f1525;border:1.5px solid rgba(59,130,246,0.18);color:#e2e8f8;border-radius:12px;padding:13px 16px;font-family:'Nunito',sans-serif;font-size:0.92rem;outline:none;"
          onfocus="this.style.borderColor='rgba(59,130,246,0.6)';this.style.boxShadow='0 0 0 3px rgba(59,130,246,0.12)'"
          onblur="this.style.borderColor='rgba(59,130,246,0.18)';this.style.boxShadow='none'"
          onkeydown="if(event.key==='Enter')doSignup()">
      </div>
      <button onclick="doSignup()"
        style="width:100%;margin-top:16px;padding:14px;border-radius:12px;border:none;cursor:pointer;background:linear-gradient(135deg,#2563eb,#3b82f6);color:#fff;font-family:'Nunito',sans-serif;font-size:0.95rem;font-weight:800;letter-spacing:0.03em;box-shadow:0 4px 20px rgba(59,130,246,0.35);display:flex;align-items:center;justify-content:center;gap:8px;transition:all 0.2s;"
        onmouseover="this.style.transform='translateY(-2px)';this.style.boxShadow='0 8px 28px rgba(59,130,246,0.5)'"
        onmouseout="this.style.transform='';this.style.boxShadow='0 4px 20px rgba(59,130,246,0.35)'">
        Create Account
        <svg width="15" height="15" viewBox="0 0 16 16" fill="none"><path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
      <div id="signupErr" style="display:none;margin-top:12px;padding:11px 14px;border-radius:10px;font-family:'Nunito',sans-serif;font-size:0.83rem;"></div>
    </div>

    <!-- FOOTER -->
    <div style="text-align:center;margin-top:20px;font-family:'Fira Code',monospace;font-size:0.68rem;color:#2a3550;">
      Built for learners serious about their career 🚀
    </div>
  </div>
</div>

<!-- LEVEL UP TOAST -->
<div id="lvlToast">🎉 Level Up! <span id="lvlToastName"></span></div>

<div id="app" style="display:none;">

<!-- SIDEBAR -->
<nav id="sidebar">
  <div class="logo">
    <div style="display:flex;align-items:center;gap:10px;margin-bottom:4px">
      <svg width="28" height="28" viewBox="0 0 28 28" fill="none" xmlns="http://www.w3.org/2000/svg">
        <circle cx="14" cy="14" r="13" fill="url(#lg1)" opacity="0.15"/>
        <!-- nodes -->
        <circle cx="14" cy="5"  r="2.5" fill="url(#lg1)"/>
        <circle cx="5"  cy="14" r="2.5" fill="url(#lg1)"/>
        <circle cx="23" cy="14" r="2.5" fill="url(#lg1)"/>
        <circle cx="14" cy="23" r="2.5" fill="url(#lg1)"/>
        <circle cx="7"  cy="7"  r="2"   fill="url(#lg2)"/>
        <circle cx="21" cy="7"  r="2"   fill="url(#lg2)"/>
        <circle cx="7"  cy="21" r="2"   fill="url(#lg2)"/>
        <circle cx="21" cy="21" r="2"   fill="url(#lg2)"/>
        <circle cx="14" cy="14" r="3"   fill="url(#lg1)"/>
        <!-- edges -->
        <line x1="14" y1="7.5"  x2="14" y2="11"  stroke="url(#lg1)" stroke-width="1" opacity="0.6"/>
        <line x1="14" y1="17"   x2="14" y2="20.5" stroke="url(#lg1)" stroke-width="1" opacity="0.6"/>
        <line x1="7.5" y1="14"  x2="11" y2="14"  stroke="url(#lg1)" stroke-width="1" opacity="0.6"/>
        <line x1="17"  y1="14"  x2="20.5" y2="14" stroke="url(#lg1)" stroke-width="1" opacity="0.6"/>
        <line x1="8.5" y1="8.5" x2="11.5" y2="11.5" stroke="url(#lg2)" stroke-width="0.8" opacity="0.5"/>
        <line x1="19.5" y1="8.5" x2="16.5" y2="11.5" stroke="url(#lg2)" stroke-width="0.8" opacity="0.5"/>
        <line x1="8.5" y1="19.5" x2="11.5" y2="16.5" stroke="url(#lg2)" stroke-width="0.8" opacity="0.5"/>
        <line x1="19.5" y1="19.5" x2="16.5" y2="16.5" stroke="url(#lg2)" stroke-width="0.8" opacity="0.5"/>
        <defs>
          <linearGradient id="lg1" x1="0" y1="0" x2="1" y2="1">
            <stop offset="0%" stop-color="#3b82f6"/>
            <stop offset="100%" stop-color="#60a5fa"/>
          </linearGradient>
          <linearGradient id="lg2" x1="0" y1="0" x2="1" y2="1">
            <stop offset="0%" stop-color="#60a5fa"/>
            <stop offset="100%" stop-color="#818cf8"/>
          </linearGradient>
        </defs>
      </svg>
      <h1>NeuralPath</h1>
    </div>
    <p>AI CAREER ROADMAP</p>
  </div>
  <div class="nav">
    <div class="nav-group-label">Core</div>
    <button class="nav-btn active" onclick="go('home',this)">
      <svg class="nav-ico" viewBox="0 0 16 16" fill="none"><path d="M8 2L2 7v7h4v-4h4v4h4V7L8 2z" stroke="currentColor" stroke-width="1.4" stroke-linejoin="round"/></svg>Dashboard</button>
    <button class="nav-btn" onclick="go('roadmap',this)">
      <svg class="nav-ico" viewBox="0 0 16 16" fill="none"><path d="M2 12c2-4 4-2 6-6s4-2 6-2" stroke="currentColor" stroke-width="1.4" stroke-linecap="round"/><circle cx="2" cy="12" r="1.2" fill="currentColor"/><circle cx="8" cy="6" r="1.2" fill="currentColor"/><circle cx="14" cy="4" r="1.2" fill="currentColor"/></svg>My Roadmap</button>
    <div class="nav-group-label">Learn</div>
    <button class="nav-btn" onclick="go('learn',this)">
      <svg class="nav-ico" viewBox="0 0 16 16" fill="none"><path d="M8 2L1 6l7 4 7-4-7-4z" stroke="currentColor" stroke-width="1.4" stroke-linejoin="round"/><path d="M1 10l7 4 7-4" stroke="currentColor" stroke-width="1.4" stroke-linecap="round"/></svg>Learning Path</button>
    <button class="nav-btn" onclick="go('daily',this)">
      <svg class="nav-ico" viewBox="0 0 16 16" fill="none"><circle cx="8" cy="8" r="6" stroke="currentColor" stroke-width="1.4"/><path d="M8 5v3.5l2.5 1.5" stroke="currentColor" stroke-width="1.4" stroke-linecap="round"/></svg>Daily Question</button>
    <button class="nav-btn" onclick="go('test',this)">
      <svg class="nav-ico" viewBox="0 0 16 16" fill="none"><rect x="3" y="2" width="10" height="12" rx="1.5" stroke="currentColor" stroke-width="1.4"/><path d="M6 6h4M6 9h4M6 12h2" stroke="currentColor" stroke-width="1.3" stroke-linecap="round"/></svg>Test Series</button>
    <div class="nav-group-label">Track</div>
    <button class="nav-btn" onclick="go('progress',this)">
      <svg class="nav-ico" viewBox="0 0 16 16" fill="none"><path d="M2 12l3-4 3 2 3-5 3 3" stroke="currentColor" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/><path d="M2 14h12" stroke="currentColor" stroke-width="1.2" stroke-linecap="round"/></svg>Progress Charts</button>
    <button class="nav-btn" onclick="go('videos',this)">
      <svg class="nav-ico" viewBox="0 0 16 16" fill="none"><rect x="1" y="3" width="10" height="10" rx="1.5" stroke="currentColor" stroke-width="1.4"/><path d="M11 6l4-2v8l-4-2V6z" stroke="currentColor" stroke-width="1.4" stroke-linejoin="round"/></svg>Video Resources</button>
    <div class="nav-group-label">DSA</div>
    <button class="nav-btn" onclick="go('dsa',this)">
      <svg class="nav-ico" viewBox="0 0 16 16" fill="none"><rect x="2" y="2" width="5" height="5" rx="1" stroke="currentColor" stroke-width="1.4"/><rect x="9" y="2" width="5" height="5" rx="1" stroke="currentColor" stroke-width="1.4"/><rect x="2" y="9" width="5" height="5" rx="1" stroke="currentColor" stroke-width="1.4"/><path d="M11.5 9v5M9 11.5h5" stroke="currentColor" stroke-width="1.4" stroke-linecap="round"/></svg>DSA Tracker</button>
  </div>

  <!-- LEVEL BADGE -->
  <div class="level-badge">
    <div class="lvl-icon" id="lvlIcon">🌱</div>
    <div class="lvl-info">
      <div style="display:flex;align-items:center;gap:6px;margin-bottom:2px">
        <div class="lvl-name" id="lvlName">Novice</div>
        <span style="font-family:'JetBrains Mono',monospace;font-size:0.6rem;padding:1px 6px;background:rgba(79,142,247,0.25);border:1px solid rgba(79,142,247,0.4);border-radius:20px;color:var(--a)">Lv.<span id="lvlNum">1</span></span>
      </div>
      <div class="lvl-num" id="lvlXpLeft">60 XP to Apprentice</div>
      <div class="lvl-xp-bar"><div class="lvl-xp-fill" id="lvlXpFill" style="width:0%"></div></div>
    </div>
  </div>

  <div class="sidebar-bottom">
    <div class="xp-row">
      <span>Total XP</span>
      <span id="xpTxt">240</span>
    </div>
    <div class="xp-track"><div class="xp-fill" id="xpBar" style="width:0%"></div></div>
    <div class="streak-text">🔥 <span id="streakN">7</span> day streak</div>
  </div>
</nav>

<!-- CONTENT -->
<div id="content">
  <div class="topbar">
    <h2 id="ptitle">Dashboard</h2>
    <div class="row gap12">
      <span class="chip on" id="connChip">● Checking...</span>
      <span class="chip" id="dateChip"></span>

      <!-- USER PILL -->
      <div class="user-pill">
        <div class="user-avatar" id="userAvatar">?</div>
        <span class="user-name-display" id="userNameDisplay">—</span>
      </div>

      <!-- LOGOUT BUTTON -->
      <button class="logout-btn" onclick="doLogout()" title="Sign out">
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none">
          <path d="M6 2H3a1 1 0 00-1 1v10a1 1 0 001 1h3" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
          <path d="M11 11l3-3-3-3M14 8H6" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        Sign Out
      </button>

      <!-- ··· OPTIONS BUTTON -->
      <div class="opts-wrap">
        <button class="opts-btn" id="optsBtn" onclick="toggleOpts(event)" title="More options">···</button>
        <div class="opts-panel" id="optsPanel">
          <div class="opt-item" onclick="go('progress',null);closeOpts()">📊 View Progress</div>
          <div class="opt-item" onclick="exportProgress()">💾 Export Summary</div>
          <div class="opt-item" onclick="go('roadmap',null);closeOpts()">🗺️ View Roadmap</div>
          <div class="opt-sep"></div>
          <div class="opt-item" onclick="copyShareLink()">🔗 Copy Share Link</div>
          <div class="opt-item" onclick="go('videos',null);closeOpts()">▶️ Video Resources</div>
          <div class="opt-sep"></div>
          <div class="opt-item danger" onclick="resetAllData()">🗑️ Reset All Data</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ===== HOME ===== -->
  <div id="page-home" class="page show">
    <div class="page-title">Build Your Career Path 🚀</div>
    <div class="page-sub">Enter your goal and AI will generate your personalized roadmap</div>

    <div class="card glow mb24">
      <div class="g2 gap16 mb16">
        <div>
          <label class="label">Your Goal / Role</label>
          <input id="i-goal" placeholder="e.g. Full Stack Developer, ML Engineer, DSA...">
        </div>
        <div>
          <label class="label">Experience Level</label>
          <select id="i-level">
            <option value="absolute beginner">🌱 Absolute Beginner</option>
            <option value="beginner">📗 Beginner</option>
            <option value="intermediate" selected>📘 Intermediate</option>
            <option value="advanced">📕 Advanced</option>
          </select>
        </div>
        <div>
          <label class="label">Primary Language</label>
          <select id="i-lang">
            <option>Python</option><option>JavaScript</option><option>Java</option>
            <option>C++</option><option>TypeScript</option><option>Go</option><option>Rust</option>
          </select>
        </div>
        <div>
          <label class="label">Hours / Day</label>
          <select id="i-hours">
            <option value="1">1 hr/day</option>
            <option value="2" selected>2 hrs/day</option>
            <option value="3">3 hrs/day</option>
            <option value="4">4+ hrs/day</option>
          </select>
        </div>
      </div>
      <div class="mb16">
        <label class="label">Current Skills (optional)</label>
        <textarea id="i-skills" placeholder="e.g. I know basic HTML and CSS..."></textarea>
      </div>
      <button class="btn btn-p" id="genBtn" onclick="genRoadmap()">⚡ Generate My AI Roadmap</button>
      <div class="loader" id="homeLoad"><div class="dots"><span></span><span></span><span></span></div><span id="loadTxt">AI is building your roadmap... please wait 15–20 seconds</span></div>
      <div class="errbox" id="homeErr"></div>
    </div>

    <div class="g4 mb24" id="statsRow" style="display:none">
      <div class="stat"><div class="stat-val ac" id="s-days">0</div><div class="stat-lbl">DAYS ACTIVE</div></div>
      <div class="stat"><div class="stat-val gc" id="s-topics">0</div><div class="stat-lbl">PHASES DONE</div></div>
      <div class="stat"><div class="stat-val" style="color:var(--y)" id="s-score">–</div><div class="stat-lbl">AVG SCORE</div></div>
      <div class="stat"><div class="stat-val" style="color:var(--a2)" id="s-xp">240</div><div class="stat-lbl">TOTAL XP</div></div>
    </div>

    <!-- OVERALL PROGRESS RING -->
    <div id="homeProgCard" class="card glow mb24" style="display:none">
      <div class="between mb16">
        <div class="bold">Overall Roadmap Progress</div>
        <span class="tag tg" id="progPctTag">0% Complete</span>
      </div>
      <div class="prog-wrap">
        <!-- SVG ring -->
        <svg class="prog-ring-svg" width="88" height="88" viewBox="0 0 88 88">
          <defs>
            <linearGradient id="rg" x1="0%" y1="0%" x2="100%" y2="0%">
              <stop offset="0%" stop-color="#4f8ef7"/>
              <stop offset="100%" stop-color="#a78bfa"/>
            </linearGradient>
          </defs>
          <circle fill="none" stroke="var(--s3)" stroke-width="8" cx="44" cy="44" r="36"/>
          <circle id="progRingCircle" fill="none" stroke="url(#rg)" stroke-width="8"
            stroke-linecap="round" cx="44" cy="44" r="36"
            stroke-dasharray="226.2" stroke-dashoffset="226.2"
            transform="rotate(-90 44 44)"
            style="transition: stroke-dashoffset 1.1s cubic-bezier(0.4,0,0.2,1)"/>
          <text x="44" y="40" text-anchor="middle" fill="var(--t)"
            font-family="Syne,sans-serif" font-size="14" font-weight="800" id="progRingPct">0%</text>
          <text x="44" y="53" text-anchor="middle" fill="var(--t3)"
            font-family="JetBrains Mono,monospace" font-size="7">DONE</text>
        </svg>
        <div class="prog-ring-info">
          <div class="prow">
            <div class="pitem">
              <div class="plbl">Phases Done</div>
              <div class="pval ac" id="progPhasesDone">0 / 0</div>
            </div>
            <div class="pitem">
              <div class="plbl">Active Phase</div>
              <div class="pval" style="color:var(--a2)" id="progActivePh">—</div>
            </div>
            <div class="pitem">
              <div class="plbl">Next Up</div>
              <div class="pval" style="color:var(--y)" id="progNextUp">—</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div id="homeRoadmap" style="display:none">
      <div class="page-title mb8" style="font-size:1.1rem">Your Roadmap Preview</div>
      <div class="card" id="homeRoadmapInner"></div>
    </div>
  </div>

  <!-- ===== ROADMAP ===== -->
  <div id="page-roadmap" class="page">
    <div class="page-title">🗺️ Detailed Roadmap</div>
    <div class="page-sub">Your full learning journey — mark phases complete to earn XP</div>
    <div class="card glow">
      <div class="between mb16">
        <div class="row gap8">
          <span class="tag tb" id="rmGoal">Generate roadmap first</span>
          <span class="tag tp" id="rmLevel">–</span>
        </div>
        <button class="btn btn-g sm" onclick="genRoadmap()">🔄 Regenerate</button>
      </div>
      <div id="rmDetail"><div class="muted sm" style="text-align:center;padding:40px 0">Go to Dashboard and generate your roadmap first →</div></div>
    </div>
  </div>

  <!-- ===== LEARN ===== -->
  <div id="page-learn" class="page">
    <div class="page-title">🧠 AI Learning Path</div>
    <div class="page-sub">Get a full structured guide for any topic</div>
    <div class="card glow mb20">
      <div class="g2 gap16 mb16">
        <div><label class="label">Topic to Learn</label><input id="l-topic" placeholder="e.g. Binary Search, React Hooks..."></div>
        <div><label class="label">Language</label>
          <select id="l-lang"><option>Python</option><option>JavaScript</option><option>Java</option><option>C++</option></select>
        </div>
      </div>
      <button class="btn btn-p mb16" onclick="genLearn()">🧠 Create Learning Path</button>
      <div class="loader" id="learnLoad"><div class="dots"><span></span><span></span><span></span></div><span>Building your guide...</span></div>
      <div class="errbox" id="learnErr"></div>
      <div id="learnOut" style="display:none"><hr class="hr"><div class="ai-out" id="learnTxt"></div></div>
    </div>
  </div>

  <!-- ===== DAILY ===== -->
  <div id="page-daily" class="page">
    <div class="page-title">⚡ Daily Question</div>
    <div class="page-sub">Personalized MCQ with timer and XP rewards</div>
    <div class="card mb16">
      <div class="row gap12 wrap">
        <input id="d-topic" placeholder="Topic..." style="width:170px">
        <select id="d-diff" style="width:130px">
          <option value="easy">🟢 Easy</option>
          <option value="medium" selected>🟡 Medium</option>
          <option value="hard">🔴 Hard</option>
        </select>
        <button class="btn btn-p" onclick="genDaily()">⚡ Get Question</button>
      </div>
    </div>
    <div class="loader" id="dailyLoad"><div class="dots"><span></span><span></span><span></span></div><span>Generating question...</span></div>
    <div class="errbox" id="dailyErr"></div>

    <div id="dailyCard" style="display:none">
      <div class="card glow mb16">
        <div class="between mb16">
          <div class="row gap8"><span class="tag" id="qDiff">MEDIUM</span><span class="tag tp" id="qTopic">–</span></div>
          <div class="row gap10">
            <div class="tring">
              <svg width="46" height="46" viewBox="0 0 46 46"><circle class="t-track" cx="23" cy="23" r="19" stroke-dasharray="119.4" stroke-dashoffset="0"/><circle class="t-prog" id="tCirc" cx="23" cy="23" r="19" stroke-dasharray="119.4" stroke-dashoffset="0"/></svg>
              <div class="tnum" id="tNum">60</div>
            </div>
          </div>
        </div>
        <div id="qText" class="mb16" style="font-size:0.97rem;line-height:1.65;font-weight:500"></div>
        <div id="qOpts"></div>
        <div id="qExp" style="display:none" class="mt16"><div class="ai-out" id="qExpTxt"></div></div>
        <div class="row gap8 mt16">
          <button class="btn btn-g" id="expBtn" onclick="showExp()" style="display:none">💡 Explain</button>
          <button class="btn btn-p" onclick="genDaily()">➡️ Next Question</button>
        </div>
      </div>
      <div class="g3 gap14">
        <div class="stat"><div class="stat-val gc" id="dCorrect">0</div><div class="stat-lbl">CORRECT</div></div>
        <div class="stat"><div class="stat-val rc2" id="dWrong">0</div><div class="stat-lbl">WRONG</div></div>
        <div class="stat"><div class="stat-val ac" id="dAcc">–</div><div class="stat-lbl">ACCURACY</div></div>
      </div>
    </div>
  </div>

  <!-- ===== TEST ===== -->
  <div id="page-test" class="page">
    <div class="page-title">📝 Test Series</div>
    <div class="page-sub">AI-generated full tests with detailed review</div>
    <div class="card mb20" id="testSetup">
      <div class="g2 gap16 mb16">
        <div><label class="label">Topic</label><input id="t-topic" placeholder="e.g. Arrays, OOP, System Design..."></div>
        <div><label class="label">Questions</label>
          <select id="t-count"><option value="5">5 (Quick)</option><option value="10" selected>10 (Standard)</option><option value="15">15 (Full)</option></select></div>
        <div><label class="label">Difficulty</label>
          <select id="t-diff"><option value="easy">🟢 Easy</option><option value="mixed" selected>🔀 Mixed</option><option value="hard">🔴 Hard</option></select></div>
        <div><label class="label">Language</label>
          <select id="t-lang"><option>Any</option><option>Python</option><option>JavaScript</option><option>Java</option><option>C++</option></select></div>
      </div>
      <button class="btn btn-p" onclick="startTest()">🚀 Start Test</button>
      <div class="loader" id="testLoad"><div class="dots"><span></span><span></span><span></span></div><span>Generating questions...</span></div>
      <div class="errbox" id="testErr"></div>
    </div>
    <div id="testArea" style="display:none">
      <div class="between mb12"><span class="mono sm ac" id="tProg">1/10</span><span class="mono sm gc" id="tScore">Score: 0</span></div>
      <div class="xp-track mb16" style="height:5px"><div class="xp-fill" id="tBar" style="width:0%"></div></div>
      <div class="card glow mb16">
        <div class="between mb12"><span class="tag" id="tQTag">–</span><span class="xs muted" id="tQNum">Q1</span></div>
        <div id="tQText" class="mb16" style="font-size:0.97rem;line-height:1.65;font-weight:500"></div>
        <div id="tQOpts"></div>
        <div class="mt12"><button class="btn btn-g" id="tNext" onclick="nextTQ()" style="display:none">Next →</button></div>
      </div>
    </div>
    <div id="testRes" style="display:none">
      <div class="card glow" style="text-align:center;padding:30px">
        <div style="font-size:3rem;margin-bottom:8px" id="resEmoji">🎉</div>
        <div class="page-title mb8">Test Complete!</div>
        <div class="g3 gap14 mb20">
          <div class="stat"><div class="stat-val gc" id="resC">–</div><div class="stat-lbl">CORRECT</div></div>
          <div class="stat"><div class="stat-val ac" id="resPct">–</div><div class="stat-lbl">SCORE</div></div>
          <div class="stat"><div class="stat-val" style="color:var(--y)" id="resXP">–</div><div class="stat-lbl">XP EARNED</div></div>
        </div>
        <button class="btn btn-p" onclick="resetTest()">🔄 Another Test</button>
        <div id="testRev" class="mt16" style="text-align:left"></div>
      </div>
    </div>
  </div>

  <!-- ===== PROGRESS ===== -->
  <div id="page-progress" class="page">
    <div class="page-title">📊 Progress Charts</div>
    <div class="page-sub">Your real learning journey — updates as you learn, test and complete phases</div>

    <!-- SUMMARY STAT ROW -->
    <div class="g4 mb16" id="progSummaryRow">
      <div class="stat"><div class="stat-val ac" id="pg-xp">0</div><div class="stat-lbl">TOTAL XP</div></div>
      <div class="stat"><div class="stat-val gc" id="pg-streak">0</div><div class="stat-lbl">DAY STREAK</div></div>
      <div class="stat"><div class="stat-val" style="color:var(--y)" id="pg-acc">—</div><div class="stat-lbl">DAILY ACC</div></div>
      <div class="stat"><div class="stat-val" style="color:var(--a2)" id="pg-tests">0</div><div class="stat-lbl">TESTS DONE</div></div>
    </div>

    <div class="g2 gap16 mb16">
      <div class="card">
        <div class="between mb12">
          <div class="bold">XP Growth</div>
          <span class="tag tb" id="pg-xp-tag">+0 XP today</span>
        </div>
        <canvas id="c4" height="145"></canvas>
      </div>
      <div class="card">
        <div class="between mb12">
          <div class="bold">Test Scores</div>
          <span class="tag tg" id="pg-avg-tag">Avg: —</span>
        </div>
        <canvas id="c2" height="145"></canvas>
      </div>
    </div>

    <div class="g2 gap16 mb16">
      <div class="card">
        <div class="between mb12">
          <div class="bold">Daily Q Accuracy</div>
          <span class="xs muted" id="pg-qcount">0 answered</span>
        </div>
        <canvas id="c1" height="145"></canvas>
      </div>
      <div class="card">
        <div class="between mb12">
          <div class="bold">Roadmap Completion</div>
          <span class="tag tg" id="pg-phases-tag">0 / 0 phases</span>
        </div>
        <canvas id="c3" height="145"></canvas>
      </div>
    </div>

    <div class="card">
      <div class="between mb12">
        <div class="bold">Skill Coverage Radar</div>
        <span class="xs muted">Based on topics attempted</span>
      </div>
      <canvas id="c5" height="200"></canvas>
    </div>
  </div>

  <!-- ===== VIDEOS ===== -->
  <div id="page-videos" class="page">
    <div class="page-title">▶️ Video Resources</div>
    <div class="page-sub">AI-curated YouTube videos in your language</div>
    <div class="card mb20">
      <div class="row gap12 wrap">
        <input id="v-topic" placeholder="Topic you need help with..." style="flex:1;min-width:180px">
        <select id="v-lang" style="width:150px">
          <option>English</option><option>Hindi</option><option>Spanish</option><option>Urdu</option><option>Portuguese</option>
        </select>
        <button class="btn btn-p" onclick="findVideos()">🔍 Find Videos</button>
      </div>
      <div class="loader" id="vidLoad"><div class="dots"><span></span><span></span><span></span></div><span>Finding best videos...</span></div>
      <div class="errbox" id="vidErr"></div>
    </div>
    <div id="vidGrid"></div>
  </div>

  <!-- ===== DSA ===== -->
  <div id="page-dsa" class="page">
    <div class="page-title">⚙️ DSA Tracker</div>
    <div class="page-sub">Data Structures & Algorithms — all languages</div>
    <div class="langrow">
      <button class="langbtn on" onclick="setDSALang(this,'Python')">Python</button>
      <button class="langbtn" onclick="setDSALang(this,'JavaScript')">JavaScript</button>
      <button class="langbtn" onclick="setDSALang(this,'Java')">Java</button>
      <button class="langbtn" onclick="setDSALang(this,'C++')">C++</button>
      <button class="langbtn" onclick="setDSALang(this,'Go')">Go</button>
      <button class="langbtn" onclick="setDSALang(this,'Rust')">Rust</button>
      <button class="langbtn" onclick="setDSALang(this,'C#')">C#</button>
      <button class="langbtn" onclick="setDSALang(this,'TypeScript')">TypeScript</button>
    </div>
    <div class="tabs">
      <div class="tab on" onclick="setDSATab(this,'arrays')">Arrays</div>
      <div class="tab" onclick="setDSATab(this,'linkedlist')">Linked List</div>
      <div class="tab" onclick="setDSATab(this,'trees')">Trees</div>
      <div class="tab" onclick="setDSATab(this,'graphs')">Graphs</div>
      <div class="tab" onclick="setDSATab(this,'dp')">Dynamic Prog</div>
      <div class="tab" onclick="setDSATab(this,'sorting')">Sorting</div>
      <div class="tab" onclick="setDSATab(this,'searching')">Searching</div>
      <div class="tab" onclick="setDSATab(this,'stackqueue')">Stack & Queue</div>
    </div>
    <div class="between mb16">
      <div class="row gap8"><span class="tag tb" id="dsaLangTag">Python</span><span class="tag tg" id="dsaTopicTag">Arrays</span></div>
      <button class="btn btn-g sm" onclick="loadDSA()">🤖 Load AI Content</button>
    </div>
    <div class="g2 gap16">
      <div class="card"><div class="bold mb12">📋 Practice Problems</div><div id="dsaProbs"></div></div>
      <div class="card"><div class="bold mb12">💡 Concepts & Code</div>
        <div id="dsaConcepts" class="muted sm">Click "Load AI Content" to see concepts and code examples.</div>
        <div class="loader" id="dsaLoad"><div class="dots"><span></span><span></span><span></span></div><span>Loading DSA content...</span></div>
      </div>
    </div>
  </div>

</div><!-- /content -->
</div><!-- /app -->

<script>
// ===========================================
// AUTH — login / signup / logout
// ===========================================
var USERS_KEY = 'np_users';
var SESSION_KEY = 'np_session';

function getUsers() { try { return JSON.parse(localStorage.getItem(USERS_KEY)||'{}'); } catch(e) { return {}; } }
function saveUsers(u) { try { localStorage.setItem(USERS_KEY, JSON.stringify(u)); } catch(e) {} }
function getSession() { try { return JSON.parse(localStorage.getItem(SESSION_KEY)||'null'); } catch(e) { return null; } }
function saveSession(u) { try { localStorage.setItem(SESSION_KEY, JSON.stringify(u)); } catch(e) {} }
function clearSession() { try { localStorage.removeItem(SESSION_KEY); } catch(e) {} }

function switchAuthTab(tab) {
  var isLogin = tab === 'login';
  var tl = document.getElementById('tabLogin');
  var ts = document.getElementById('tabSignup');
  var lf = document.getElementById('loginForm');
  var sf = document.getElementById('signupForm');
  if (!tl || !ts || !lf || !sf) return;

  tl.style.background = isLogin ? '#3b82f6' : 'transparent';
  tl.style.color       = isLogin ? '#fff'    : '#6b7fa8';
  ts.style.background  = isLogin ? 'transparent' : '#3b82f6';
  ts.style.color       = isLogin ? '#6b7fa8' : '#fff';

  lf.style.display = isLogin ? 'block' : 'none';
  sf.style.display = isLogin ? 'none'  : 'block';

  hideAuthErr('loginErr');
  hideAuthErr('signupErr');
}

function showAuthErr(id, msg, isSuccess) {
  var e = document.getElementById(id);
  if (!e) return;
  e.textContent = (isSuccess ? '✅ ' : '⚠️ ') + msg;
  e.style.display = 'block';
  e.style.background  = isSuccess ? 'rgba(52,211,153,0.09)' : 'rgba(248,113,113,0.09)';
  e.style.border      = isSuccess ? '1px solid rgba(52,211,153,0.3)' : '1px solid rgba(248,113,113,0.3)';
  e.style.color       = isSuccess ? '#34d399' : '#f87171';
}
function hideAuthErr(id) {
  var e = document.getElementById(id);
  if (e) e.style.display = 'none';
}

function doLogin() {
  var id = (document.getElementById('loginId').value || '').trim();
  var pw =  document.getElementById('loginPw').value || '';
  if (!id || !pw) { showAuthErr('loginErr', 'Please fill in all fields.'); return; }
  var users = getUsers();
  var user = Object.values(users).find(function(u) { return u.email === id || u.username === id; });
  if (!user) { showAuthErr('loginErr', 'No account found. Please sign up first.'); return; }
  if (user.password !== btoa(pw)) { showAuthErr('loginErr', 'Wrong password. Try again.'); return; }
  saveSession(user);
  enterApp(user);
}

function doSignup() {
  var name  = (document.getElementById('signupName').value  || '').trim();
  var uname = (document.getElementById('signupUser').value  || '').trim().toLowerCase().replace(/\s+/g,'_');
  var email = (document.getElementById('signupEmail').value || '').trim().toLowerCase();
  var pw    =  document.getElementById('signupPw').value    || '';
  if (!name || !uname || !email || !pw) { showAuthErr('signupErr', 'Please fill in all fields.'); return; }
  if (pw.length < 6) { showAuthErr('signupErr', 'Password must be at least 6 characters.'); return; }
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) { showAuthErr('signupErr', 'Please enter a valid email.'); return; }
  var users = getUsers();
  var exists = Object.values(users).find(function(u) { return u.email === email || u.username === uname; });
  if (exists) { showAuthErr('signupErr', 'Email or username already taken.'); return; }
  var user = { name: name, username: uname, email: email, password: btoa(pw), id: Date.now() };
  users[user.id] = user;
  saveUsers(users);
  // Clear fields
  document.getElementById('signupName').value = '';
  document.getElementById('signupUser').value = '';
  document.getElementById('signupEmail').value = '';
  document.getElementById('signupPw').value = '';
  // Go to login with pre-filled email and success message
  switchAuthTab('login');
  document.getElementById('loginId').value = email;
  document.getElementById('loginPw').value = '';
  document.getElementById('loginPw').focus();
  showAuthErr('loginErr', 'Account created! Enter your password to sign in.', true);
}

function enterApp(user) {
  CURRENT_UID = user.id;
  try { loadProgress(user.id); } catch(e) {}
  // Update topbar
  var initials = user.name.split(' ').map(function(w){return w[0];}).join('').toUpperCase().slice(0,2);
  document.getElementById('userAvatar').textContent = initials;
  document.getElementById('userNameDisplay').textContent = user.name.split(' ')[0];
  // Hide auth, show app — INSTANT, no fade that can break
  document.getElementById('authScreen').style.display = 'none';
  document.getElementById('app').style.display = 'flex';
  try { updateStreak(); saveProgress(); } catch(e) {}
}

function doLogout() {
  try { saveProgress(); } catch(e) {}
  clearSession();
  CURRENT_UID = null;
  // Reset state
  S.goal=''; S.level=''; S.lang='Python'; S.xp=0; S.streak=0;
  S.dc=0; S.dw=0; S.scores=[]; S.phaseDone=[]; S.roadmap=null;
  S.act=[0,0,0,0,0,0,0,0,0,0,0,0,0,0]; S.lastActive=null;
  // Reset UI
  try { updateLevelUI(); } catch(e) {}
  try { document.getElementById('streakN').textContent='0'; } catch(e) {}
  try { document.getElementById('statsRow').style.display='none'; } catch(e) {}
  try { document.getElementById('homeRoadmap').style.display='none'; } catch(e) {}
  try { document.getElementById('homeProgCard').style.display='none'; } catch(e) {}
  try { document.getElementById('userAvatar').textContent='?'; } catch(e) {}
  try { document.getElementById('userNameDisplay').textContent='—'; } catch(e) {}
  try { document.getElementById('loginId').value=''; } catch(e) {}
  try { document.getElementById('loginPw').value=''; } catch(e) {}
  // Switch screens — INSTANT
  document.getElementById('app').style.display = 'none';
  document.getElementById('authScreen').style.display = 'flex';
  switchAuthTab('login');
}

// Auto-login on page load
(function() {
  var session = getSession();
  if (session && session.name) { enterApp(session); }
})();

// ===========================================
// API
// ===========================================
const KEY = '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';
const MODEL = 'claude-opus-4-5';

async function ask(system, user) {
  const r = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'content-type': 'application/json',
      'x-api-key': KEY,
      'anthropic-version': '2023-06-01',
      'anthropic-dangerous-direct-browser-access': 'true'
    },
    body: JSON.stringify({
      model: MODEL,
      max_tokens: 1500,
      system: system,
      messages: [{ role: 'user', content: user }]
    })
  });
  if (!r.ok) {
    const e = await r.text();
    throw new Error('API ' + r.status + ': ' + e);
  }
  const d = await r.json();
  if (d.error) throw new Error(d.error.message || JSON.stringify(d.error));
  return d.content[0].text;
}

async function askJSON(system, user) {
  const raw = await ask(system, user);
  var clean = raw.replace(/```json\s*/gi,'').replace(/```/g,'').trim();
  try { return JSON.parse(clean); } catch {}
  var o = clean.match(/\{[\s\S]*\}/);
  if (o) try { return JSON.parse(o[0]); } catch {}
  var a = clean.match(/\[[\s\S]*\]/);
  if (a) try { return JSON.parse(a[0]); } catch {}
  throw new Error('Bad JSON: ' + clean.slice(0, 120));
}

// ===========================================
// STATE
// ===========================================
var S = {
  goal: '', level: '', lang: 'Python',
  xp: 0, streak: 0,
  dc: 0, dw: 0,
  curQ: null, dAns: null,
  tData: [], tIdx: 0, tRight: 0,
  dsaLang: 'Python', dsaTopic: 'arrays',
  scores: [],
  act:    [0,0,0,0,0,0,0,0,0,0,0,0,0,0],
  roadmap: null,
  phaseDone: [],
  lastActive: null,   // date string for streak tracking
};

// Current logged-in user id
var CURRENT_UID = null;

// ===========================================
// PROGRESS PERSISTENCE
// ===========================================
function progressKey(uid) { return 'np_progress_' + uid; }

function saveProgress() {
  if (!CURRENT_UID) return;
  var toSave = {
    goal:      S.goal,
    level:     S.level,
    lang:      S.lang,
    xp:        S.xp,
    streak:    S.streak,
    dc:        S.dc,
    dw:        S.dw,
    scores:    S.scores,
    act:       S.act,
    roadmap:   S.roadmap,
    phaseDone: S.phaseDone,
    lastActive: S.lastActive,
    savedAt:   new Date().toISOString(),
  };
  try { localStorage.setItem(progressKey(CURRENT_UID), JSON.stringify(toSave)); } catch(e) {}
}

function loadProgress(uid) {
  try {
    var raw = localStorage.getItem(progressKey(uid));
    if (!raw) return;
    var p = JSON.parse(raw);
    S.goal      = p.goal      || '';
    S.level     = p.level     || '';
    S.lang      = p.lang      || 'Python';
    S.xp        = p.xp        || 0;
    S.streak    = p.streak    || 0;
    S.dc        = p.dc        || 0;
    S.dw        = p.dw        || 0;
    S.scores    = p.scores    || [];
    S.act       = p.act       || [0,0,0,0,0,0,0,0,0,0,0,0,0,0];
    S.roadmap   = p.roadmap   || null;
    S.phaseDone = p.phaseDone || [];
    S.lastActive = p.lastActive || null;

    // Update streak based on last active date
    updateStreak();

    // Restore UI
    updateLevelUI();
    setTxt('streakN', S.streak);

    // Restore roadmap if it existed
    if (S.roadmap && S.roadmap.phases) {
      renderPreview(S.roadmap);
      renderDetail(S.roadmap);
      updateProgRing();
      el('statsRow').style.display = 'grid';
      el('homeRoadmap').style.display = 'block';
      el('homeProgCard').style.display = 'block';
      setTxt('s-days', S.streak);
      setTxt('s-topics', S.phaseDone.filter(Boolean).length);
      setTxt('s-score', S.scores.length ? Math.round(S.scores.reduce(function(a,b){return a+b;},0)/S.scores.length) + '%' : '–');
      setTxt('s-xp', S.xp);
      el('rmGoal').textContent  = S.goal  || 'Generate roadmap first';
      el('rmLevel').textContent = S.level || '–';
    }
  } catch(e) { console.warn('Could not load progress:', e); }
}

function updateStreak() {
  var today = new Date().toDateString();
  if (!S.lastActive) {
    S.lastActive = today;
    return;
  }
  var last = new Date(S.lastActive);
  var now  = new Date();
  var diffDays = Math.floor((now - last) / 86400000);
  if (diffDays === 0) return;           // same day, no change
  if (diffDays === 1) { S.streak++; }   // consecutive day
  else { S.streak = 1; }               // streak broken
  S.lastActive = today;
}

function bumpActivity() {
  // Add 1 to today's activity slot (last in array)
  S.act[S.act.length - 1] = (S.act[S.act.length - 1] || 0) + 1;
}

// ===========================================
// LEVEL SYSTEM
// ===========================================
var LEVELS = [
  { name: 'Novice',     icon: '🌱', minXP: 0    },
  { name: 'Apprentice', icon: '📗', minXP: 300  },
  { name: 'Developer',  icon: '💻', minXP: 700  },
  { name: 'Expert',     icon: '🔥', minXP: 1400 },
  { name: 'Master',     icon: '🏆', minXP: 2500 },
  { name: 'Legend',     icon: '⚡', minXP: 4000 },
];

function getLvl(xp) {
  var idx = 0;
  for (var i = LEVELS.length - 1; i >= 0; i--) {
    if (xp >= LEVELS[i].minXP) { idx = i; break; }
  }
  var next = LEVELS[idx + 1] || null;
  var base = LEVELS[idx].minXP;
  var ceil = next ? next.minXP : base + 1000;
  var pct  = Math.min(100, Math.round((xp - base) / (ceil - base) * 100));
  var left = next ? (ceil - xp) : 0;
  return { idx, name: LEVELS[idx].name, icon: LEVELS[idx].icon, num: idx + 1, pct, left, next: next ? next.name : null };
}

function updateLevelUI(prevXP) {
  var info     = getLvl(S.xp);
  var prevInfo = getLvl(prevXP !== undefined ? prevXP : S.xp);

  // Sidebar badge
  el('lvlIcon').textContent = info.icon;
  el('lvlName').textContent = info.name;
  el('lvlNum').textContent  = info.num;
  el('lvlXpLeft').textContent = info.next ? info.left + ' XP to ' + info.next : '🏆 MAX LEVEL';
  el('lvlXpFill').style.width = info.pct + '%';

  // Bottom XP bar
  var MAX_DISPLAY = LEVELS[LEVELS.length - 1].minXP + 1000;
  el('xpTxt').textContent = S.xp;
  el('xpBar').style.width = Math.min(100, S.xp / MAX_DISPLAY * 100) + '%';

  // Dashboard stat
  var sxp = el('s-xp'); if (sxp) sxp.textContent = S.xp;

  // Level-up toast
  if (prevXP !== undefined && info.num > prevInfo.num) {
    el('lvlToast').innerHTML = '🎉 Level Up! ' + info.icon + ' <strong>' + info.name + '</strong> (Level ' + info.num + ')';
    var t = el('lvlToast');
    t.classList.add('show');
    setTimeout(function(){ t.classList.remove('show'); }, 3500);
  }
}

function addXP(n) {
  var prev = S.xp;
  S.xp += n;
  bumpActivity();
  updateLevelUI(prev);
  saveProgress();
}

// ===========================================
// HELPERS
// ===========================================
function el(id) { return document.getElementById(id); }
function setHTML(id, h) { el(id).innerHTML = h; }
function setTxt(id, t) { el(id).textContent = t; }
function showErr(id, msg) { var e = el(id); e.innerHTML = '❌ ' + msg; e.classList.add('show'); }
function hideErr(id) { el(id).innerHTML = ''; el(id).classList.remove('show'); }
function startLoad(id) { el(id).classList.add('show'); }
function stopLoad(id)  { el(id).classList.remove('show'); }

// ===========================================
// OPTIONS MENU
// ===========================================
function toggleOpts(e) {
  e.stopPropagation();
  var p = el('optsPanel'), b = el('optsBtn');
  var open = p.classList.toggle('open');
  b.classList.toggle('open', open);
}
function closeOpts() {
  el('optsPanel').classList.remove('open');
  el('optsBtn').classList.remove('open');
}
document.addEventListener('click', function(){ closeOpts(); });

function exportProgress() {
  closeOpts();
  var info = getLvl(S.xp);
  var done = S.phaseDone.filter(Boolean).length;
  var lines = [
    '=== NeuralPath Progress Export ===',
    'Date: ' + new Date().toLocaleDateString(),
    '',
    'Goal: ' + (S.goal || 'Not set'),
    'Level: ' + info.icon + ' ' + info.name + ' (Level ' + info.num + ')',
    'Total XP: ' + S.xp,
    'Streak: ' + S.streak + ' days',
    '',
    'Daily Questions — Correct: ' + S.dc + ' | Wrong: ' + S.dw + ' | Accuracy: ' + (S.dc + S.dw > 0 ? Math.round(S.dc / (S.dc + S.dw) * 100) + '%' : '—'),
    'Test Scores: ' + (S.scores.length ? S.scores.join(', ') + ' (avg ' + Math.round(S.scores.reduce(function(a,b){return a+b;},0) / S.scores.length) + '%)' : 'none'),
    'Phases Completed: ' + done + ' / ' + S.phaseDone.length,
  ];
  var blob = new Blob([lines.join('\n')], { type: 'text/plain' });
  var a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'neuralpath-progress.txt';
  a.click();
}

function copyShareLink() {
  closeOpts();
  var info = getLvl(S.xp);
  var txt = 'I\'m a ' + info.icon + ' ' + info.name + ' on NeuralPath! 🚀 Goal: ' + (S.goal || 'Learning to code') + ' | XP: ' + S.xp + ' | Streak: ' + S.streak + ' days 🔥';
  if (navigator.clipboard) {
    navigator.clipboard.writeText(txt).then(function(){ showToast('Copied to clipboard!'); });
  } else {
    alert(txt);
  }
}

function resetAllData() {
  closeOpts();
  if (!confirm('Reset ALL progress? This cannot be undone.')) return;
  S.xp = 0; S.streak = 0; S.dc = 0; S.dw = 0;
  S.scores = []; S.phaseDone = []; S.roadmap = null; S.goal = '';
  S.act = [0,0,0,0,0,0,0,0,0,0,0,0,0,0]; S.lastActive = null;
  updateLevelUI();
  setTxt('streakN', 0);
  el('statsRow').style.display = 'none';
  el('homeRoadmap').style.display = 'none';
  el('homeProgCard').style.display = 'none';
  setHTML('rmDetail', '<div class="muted sm" style="text-align:center;padding:40px 0">Go to Dashboard and generate your roadmap first →</div>');
  setTxt('rmGoal', 'Generate roadmap first');
  setTxt('rmLevel', '–');
  // Wipe saved progress for this user
  if (CURRENT_UID) {
    try { localStorage.removeItem(progressKey(CURRENT_UID)); } catch(e) {}
  }
  showToast('Progress reset!');
}

function showToast(msg) {
  el('lvlToastName').textContent = msg;
  el('lvlToast').textContent = msg;
  el('lvlToast').classList.add('show');
  setTimeout(function(){ el('lvlToast').classList.remove('show'); }, 2000);
}

// ===========================================
// NAVIGATION
// ===========================================
var TITLES = {
  home:'Dashboard', roadmap:'My Roadmap', learn:'Learning Path',
  daily:'Daily Question', test:'Test Series', progress:'Progress Charts',
  videos:'Video Resources', dsa:'DSA Tracker'
};

function go(page, btn) {
  closeOpts();
  document.querySelectorAll('.page').forEach(function(p){ p.classList.remove('show'); });
  document.querySelectorAll('.nav-btn').forEach(function(b){ b.classList.remove('active'); });
  el('page-' + page).classList.add('show');
  if (btn) btn.classList.add('active');
  setTxt('ptitle', TITLES[page] || page);
  if (page === 'progress') setTimeout(drawCharts, 100);
}

// Date chip
el('dateChip').textContent = new Date().toLocaleDateString('en-US',{month:'short',day:'numeric',year:'numeric'});

// Init level UI on load
updateLevelUI();

// Auto-save every 30 seconds and on tab close
setInterval(function(){ if(CURRENT_UID) saveProgress(); }, 30000);
window.addEventListener('beforeunload', function(){ if(CURRENT_UID) saveProgress(); });

// ===========================================
// CONNECTION CHECK
// ===========================================
(async function checkConn() {
  try {
    await ask('Reply with just: ok', 'ping');
    el('connChip').textContent = '● Connected';
    el('connChip').className = 'chip on';
  } catch(e) {
    el('connChip').textContent = '● Error';
    el('connChip').style.color = 'var(--r)';
    console.error('Connection check failed:', e.message);
  }
})();

// ===========================================
// GENERATE ROADMAP
// ===========================================
async function genRoadmap() {
  var goal  = el('i-goal').value.trim();
  var level = el('i-level').value;
  var lang  = el('i-lang').value;
  var hrs   = el('i-hours').value;
  var sk    = el('i-skills').value.trim();

  if (!goal) { alert('Please enter your career goal first!'); return; }

  S.goal = goal; S.level = level; S.lang = lang;
  el('genBtn').disabled = true;
  startLoad('homeLoad');
  hideErr('homeErr');
  setTxt('loadTxt', 'AI is building your roadmap... please wait 15–20 seconds');

  try {
    var SYS = 'You are a senior career coach. Output ONLY valid JSON, no markdown, no extra text.';
    var USR = 'Create a 5-phase learning roadmap for: "' + goal + '"\n' +
      'Level: ' + level + '\nLanguage: ' + lang + '\nHours/day: ' + hrs +
      '\nCurrent skills: ' + (sk || 'none') + '\n\n' +
      'Return ONLY this JSON:\n' +
      '{"title":"Roadmap Title","duration":"X months","phases":[' +
      '{"phase":1,"name":"Phase Name","duration":"2 weeks","topics":["t1","t2","t3"],"milestone":"What they achieve","status":"active"},' +
      '{"phase":2,"name":"Phase Name","duration":"3 weeks","topics":["t1","t2","t3"],"milestone":"What they achieve","status":"pending"},' +
      '{"phase":3,"name":"Phase Name","duration":"4 weeks","topics":["t1","t2","t3"],"milestone":"What they achieve","status":"pending"},' +
      '{"phase":4,"name":"Phase Name","duration":"4 weeks","topics":["t1","t2","t3"],"milestone":"What they achieve","status":"pending"},' +
      '{"phase":5,"name":"Phase Name","duration":"6 weeks","topics":["t1","t2","t3"],"milestone":"What they achieve","status":"pending"}]}';

    var data = await askJSON(SYS, USR);
    if (!data || !data.phases) throw new Error('No phases in response');

    S.roadmap = data;
    S.phaseDone = data.phases.map(function(){ return false; });

    renderPreview(data);
    renderDetail(data);
    updateProgRing();

    el('statsRow').style.display = 'grid';
    el('homeRoadmap').style.display = 'block';
    el('homeProgCard').style.display = 'block';
    setTxt('s-days', S.streak);
    setTxt('s-topics', '0');
    setTxt('s-score', S.scores.length ? Math.round(S.scores.reduce(function(a,b){return a+b;},0)/S.scores.length) + '%' : '–');
    el('rmGoal').textContent = goal;
    el('rmLevel').textContent = level;
    addXP(50);  // addXP already calls saveProgress
  } catch(err) {
    showErr('homeErr', err.message + '<br><br>Check your internet connection and try again.');
    console.error(err);
  }

  stopLoad('homeLoad');
  el('genBtn').disabled = false;
}

// ===========================================
// RENDER PREVIEW (home page)
// ===========================================
function renderPreview(d) {
  var h = '<div class="between mb16"><strong>' + d.title + '</strong><span class="tag tg">~' + d.duration + '</span></div>';
  d.phases.forEach(function(p, i) {
    var done = S.phaseDone[i];
    var cls  = done ? 'rc-done' : (p.status === 'active' ? 'rc-active' : 'rc-pend');
    var lbl  = done ? '✓' : p.phase;
    h += '<div class="rnode"><div class="rcircle ' + cls + '">' + lbl + '</div><div class="rbody">';
    h += '<div class="rtitle">' + p.name + ' <span class="tag tb" style="font-size:.58rem">' + p.duration + '</span></div>';
    h += '<div>' + p.topics.map(function(t){ return '<span class="tag tp">' + t + '</span>'; }).join('') + '</div>';
    h += '<div class="rdesc mt12" style="margin-top:5px">🎯 ' + p.milestone + '</div>';
    h += '</div></div>';
  });
  setHTML('homeRoadmapInner', h);
}

// ===========================================
// RENDER DETAIL (roadmap page) — with progress bars + complete button
// ===========================================
function renderDetail(d) {
  var h = '';
  d.phases.forEach(function(p, i) {
    var done    = S.phaseDone[i];
    var isFirst = !done && (i === 0 || S.phaseDone[i-1]);  // first incomplete after all dones
    var cls     = done ? 'rc-done' : (isFirst ? 'rc-active' : 'rc-pend');
    var lbl     = done ? '✓' : p.phase;
    var tc      = done ? 'tg' : (isFirst ? 'tb' : 'tr');
    var statusTxt = done ? 'DONE' : (isFirst ? 'IN PROGRESS' : 'PENDING');
    var pct     = done ? 100 : (isFirst ? 20 : 0);

    h += '<div class="rnode">';
    h += '<div class="rcircle ' + cls + '" id="pc-' + i + '">' + lbl + '</div>';
    h += '<div class="rbody">';

    // Header row with status + complete button
    h += '<div class="between mb8">';
    h += '<strong>Phase ' + p.phase + ': ' + p.name + '</strong>';
    h += '<div class="row gap8">';
    h += '<span class="tag ' + tc + '" id="pt-' + i + '">' + statusTxt + '</span>';
    if (!done) {
      h += '<button class="btn-done" onclick="completePhase(' + i + ')">✓ Mark Done</button>';
    } else {
      h += '<button class="btn-done" disabled>✓ Completed</button>';
    }
    h += '</div></div>';

    h += '<div class="xs muted mb8">⏱ ' + p.duration + '</div>';
    h += '<div class="mb8">' + p.topics.map(function(t){ return '<span class="tag tp">' + t + '</span>'; }).join('') + '</div>';
    h += '<div class="rdesc">🏆 ' + p.milestone + '</div>';

    // Phase progress bar
    h += '<div class="phase-pbar"><div class="phase-pbar-fill" id="ppb-' + i + '" style="width:' + pct + '%"></div></div>';
    h += '<div class="phase-pbar-lbl" id="ppbl-' + i + '">' + pct + '% complete</div>';

    h += '<div class="row gap8 mt12" style="margin-top:9px">';
    h += '<button class="btn btn-g sm" onclick="quickLearn(\'' + esc(p.name) + '\')">📚 Learn</button>';
    h += '<button class="btn btn-g sm" onclick="quickTest(\'' + esc(p.topics[0]) + '\')">📝 Test</button>';
    h += '</div>';

    h += '</div></div>';
  });
  setHTML('rmDetail', h);
}

// ===========================================
// COMPLETE PHASE
// ===========================================
function completePhase(i) {
  if (!S.roadmap || S.phaseDone[i]) return;
  S.phaseDone[i] = true;

  // Update circle
  var circ = el('pc-' + i);
  if (circ) { circ.className = 'rcircle rc-done'; circ.textContent = '✓'; }

  // Update tag
  var tag = el('pt-' + i);
  if (tag) { tag.className = 'tag tg'; tag.textContent = 'DONE'; }

  // Update progress bar to 100%
  var bar = el('ppb-' + i);
  if (bar) bar.style.width = '100%';
  var lbl = el('ppbl-' + i);
  if (lbl) lbl.textContent = '100% complete';

  // Activate next phase if exists
  var next = i + 1;
  if (next < S.phaseDone.length && !S.phaseDone[next]) {
    var nc = el('pc-' + next);
    if (nc) nc.className = 'rcircle rc-active';
    var nt = el('pt-' + next);
    if (nt) { nt.className = 'tag tb'; nt.textContent = 'IN PROGRESS'; }
    var nb = el('ppb-' + next);
    if (nb) nb.style.width = '20%';
    var nl = el('ppbl-' + next);
    if (nl) nl.textContent = '20% complete';
  }

  // XP reward
  addXP(100);

  // Update stats
  var doneCount = S.phaseDone.filter(Boolean).length;
  var sdone = el('s-topics'); if (sdone) setTxt('s-topics', doneCount);

  updateProgRing();
  renderPreview(S.roadmap);
}

// ===========================================
// UPDATE PROGRESS RING
// ===========================================
function updateProgRing() {
  if (!S.roadmap) return;
  var total = S.phaseDone.length;
  var done  = S.phaseDone.filter(Boolean).length;
  var pct   = total > 0 ? Math.round(done / total * 100) : 0;

  // Ring circle
  var circ = 226.2;
  var offset = circ - (circ * pct / 100);
  var ring = el('progRingCircle');
  if (ring) ring.setAttribute('stroke-dashoffset', offset);
  var pctTxt = el('progRingPct');
  if (pctTxt) pctTxt.textContent = pct + '%';

  // Tag
  var ptag = el('progPctTag');
  if (ptag) ptag.textContent = pct + '% Complete';

  // Phases done
  var pd = el('progPhasesDone');
  if (pd) pd.textContent = done + ' / ' + total;

  // Active phase name
  var activeIdx = S.phaseDone.indexOf(false);
  var aph = el('progActivePh');
  if (aph) aph.textContent = activeIdx >= 0 ? S.roadmap.phases[activeIdx].name : '🎉 All Done!';

  // Next up (phase after active)
  var nextIdx = activeIdx + 1;
  var nup = el('progNextUp');
  if (nup) nup.textContent = (nextIdx > 0 && nextIdx < total) ? S.roadmap.phases[nextIdx].name : (activeIdx < 0 ? '—' : '—');
}

function esc(s) { return s.replace(/'/g, "\\'").replace(/"/g, '\\"'); }
function quickLearn(n) { el('l-topic').value = n; go('learn', null); setTimeout(genLearn, 200); }
function quickTest(t)  { el('t-topic').value = t; go('test', null); }

// ===========================================
// LEARN PATH
// ===========================================
async function genLearn() {
  var topic = el('l-topic').value.trim();
  var lang  = el('l-lang').value;
  if (!topic) { alert('Enter a topic!'); return; }
  startLoad('learnLoad');
  el('learnOut').style.display = 'none';
  hideErr('learnErr');
  try {
    var txt = await ask(
      'You are an expert programming mentor. Give clear, practical explanations.',
      'Create a complete learning guide for "' + topic + '" in ' + lang + '.\n' +
      'Sections: 1)📖 What it is  2)🧩 Key concepts  3)💻 Code example with comments  ' +
      '4)🔄 Practice exercises  5)⚡ Common mistakes  6)🚀 Next steps  7)📊 Complexity.\n' +
      'Be practical and use emojis for section headers.'
    );
    setTxt('learnTxt', txt);
    el('learnOut').style.display = 'block';
    addXP(20);
  } catch(err) { showErr('learnErr', err.message); }
  stopLoad('learnLoad');
}

// ===========================================
// DAILY QUESTION
// ===========================================
var dTimer = null;

async function genDaily() {
  var topic = el('d-topic').value.trim() || S.goal || 'Arrays';
  var diff  = el('d-diff').value;
  clearInterval(dTimer);
  startLoad('dailyLoad');
  el('dailyCard').style.display = 'none';
  hideErr('dailyErr');
  S.dAns = null;

  try {
    var SYS = 'You are a coding interview expert. Return ONLY valid JSON, no markdown, no backticks.';
    var USR = 'Generate a ' + diff + ' MCQ question about "' + topic + '".\n' +
      'Return ONLY:\n{"question":"text?","options":["A","B","C","D"],"correct":0,' +
      '"explanation":"why correct","difficulty":"' + diff + '","topic":"' + topic + '"}\n' +
      'correct = 0-based index. Make it educational.';
    var d = await askJSON(SYS, USR);
    if (!d || !d.question) throw new Error('Invalid question received');
    S.curQ = d;
    renderDaily(d);
    startDTimer(60);
  } catch(err) { showErr('dailyErr', err.message); }
  stopLoad('dailyLoad');
}

function renderDaily(d) {
  var keys = ['A','B','C','D'];
  var dc = {easy:'tg',medium:'to',hard:'tr'};
  el('qDiff').className = 'tag ' + (dc[d.difficulty] || 'tb');
  setTxt('qDiff', (d.difficulty||'MEDIUM').toUpperCase());
  setTxt('qTopic', (d.topic||'GENERAL').toUpperCase());
  setTxt('qText', d.question);
  el('qExp').style.display = 'none';
  el('expBtn').style.display = 'none';
  setHTML('qOpts', d.options.map(function(o,i){
    return '<div class="qopt" id="dopt' + i + '" onclick="pickDaily(' + i + ')">' +
      '<span class="qkey">' + keys[i] + '</span><span>' + o + '</span></div>';
  }).join(''));
  el('dailyCard').style.display = 'block';
}

function pickDaily(idx) {
  if (S.dAns !== null) return;
  S.dAns = idx;
  clearInterval(dTimer);
  var c = S.curQ.correct;
  for (var i = 0; i < 4; i++) {
    var opt = el('dopt' + i); if (!opt) continue;
    opt.classList.add('done');
    if (i === c) opt.classList.add('ok');
    else if (i === idx) opt.classList.add('no');
  }
  if (idx === c) { S.dc++; addXP(15); }  // addXP saves
  else { S.dw++; saveProgress(); }
  var tot = S.dc + S.dw;
  setTxt('dCorrect', S.dc);
  setTxt('dWrong', S.dw);
  setTxt('dAcc', Math.round(S.dc / tot * 100) + '%');
  el('expBtn').style.display = 'inline-flex';
}

function showExp() {
  if (!S.curQ) return;
  el('qExp').style.display = 'block';
  setTxt('qExpTxt', S.curQ.explanation);
}

function startDTimer(sec) {
  var circ = el('tCirc'), total = 119.4, left = sec;
  clearInterval(dTimer);
  dTimer = setInterval(function() {
    left--;
    setTxt('tNum', left);
    circ.style.strokeDashoffset = total * (1 - left / sec);
    if (left <= 0) {
      clearInterval(dTimer);
      if (S.dAns === null) {
        S.dw++;
        for (var i = 0; i < 4; i++) {
          var o = el('dopt' + i); if (!o) continue;
          o.classList.add('done');
          if (i === S.curQ.correct) o.classList.add('ok');
        }
        el('expBtn').style.display = 'inline-flex';
        setTxt('dWrong', S.dw);
        var tot = S.dc + S.dw;
        setTxt('dAcc', Math.round(S.dc / tot * 100) + '%');
      }
    }
  }, 1000);
}

// ===========================================
// TEST SERIES
// ===========================================
var TD = [], tIdx = 0, tRight = 0;

async function startTest() {
  var topic = el('t-topic').value.trim();
  var cnt   = parseInt(el('t-count').value);
  var diff  = el('t-diff').value;
  var lang  = el('t-lang').value;
  if (!topic) { alert('Enter a topic!'); return; }
  startLoad('testLoad');
  el('testSetup').style.pointerEvents = 'none';
  hideErr('testErr');

  try {
    var SYS = 'You are a coding test generator. Return ONLY a valid JSON array, no markdown, no backticks.';
    var USR = 'Generate exactly ' + cnt + ' MCQ questions about "' + topic + '"' +
      (lang !== 'Any' ? ' in ' + lang : '') + '. Difficulty: ' + diff + '.\n' +
      'Return ONLY:\n[{"question":"?","options":["A","B","C","D"],"correct":0,' +
      '"explanation":"why","difficulty":"easy"},...]\n' +
      'Return exactly ' + cnt + ' items.';
    var data = await askJSON(SYS, USR);
    if (!Array.isArray(data) || data.length === 0) throw new Error('No questions received');
    TD = data; tIdx = 0; tRight = 0;
    el('testSetup').style.display = 'none';
    el('testArea').style.display = 'block';
    el('testRes').style.display = 'none';
    renderTQ();
  } catch(err) { showErr('testErr', err.message); }

  stopLoad('testLoad');
  el('testSetup').style.pointerEvents = 'auto';
}

function renderTQ() {
  var q = TD[tIdx], tot = TD.length, keys = ['A','B','C','D'];
  var dc = {easy:'tg', medium:'to', hard:'tr', mixed:'tb'};
  setTxt('tProg', (tIdx+1) + '/' + tot);
  el('tBar').style.width = (tIdx / tot * 100) + '%';
  setTxt('tQNum', 'Q' + (tIdx+1));
  el('tQTag').className = 'tag ' + (dc[q.difficulty] || 'tb');
  setTxt('tQTag', (q.difficulty||'').toUpperCase());
  setTxt('tQText', q.question);
  setTxt('tScore', 'Score: ' + tRight);
  el('tNext').style.display = 'none';
  q._a = null;
  setHTML('tQOpts', q.options.map(function(o,i){
    return '<div class="qopt" id="topt' + i + '" onclick="pickTest(' + i + ')">' +
      '<span class="qkey">' + keys[i] + '</span><span>' + o + '</span></div>';
  }).join(''));
}

function pickTest(idx) {
  var q = TD[tIdx];
  if (q._a !== null && q._a !== undefined) return;
  q._a = idx;
  for (var i = 0; i < 4; i++) {
    var o = el('topt' + i); if (!o) continue;
    o.classList.add('done');
    if (i === q.correct) o.classList.add('ok');
    else if (i === idx) o.classList.add('no');
  }
  if (idx === q.correct) tRight++;
  setTxt('tScore', 'Score: ' + tRight);
  el('tNext').style.display = 'inline-flex';
  el('tNext').textContent = tIdx < TD.length - 1 ? 'Next →' : 'Finish ✓';
}

function nextTQ() {
  if (tIdx >= TD.length - 1) { showTestResults(); return; }
  tIdx++;
  renderTQ();
}

function showTestResults() {
  var tot = TD.length, pct = Math.round(tRight / tot * 100), xpe = tRight * 20;
  S.scores.push(pct);   // push score BEFORE addXP so saveProgress gets it
  addXP(xpe);           // this also calls saveProgress

  // Update avg score stat
  var avg = Math.round(S.scores.reduce(function(a,b){return a+b;},0) / S.scores.length);
  var ss = el('s-score'); if (ss) ss.textContent = avg + '%';

  el('testArea').style.display = 'none';
  el('testRes').style.display = 'block';
  setTxt('resEmoji', pct >= 80 ? '🏆' : pct >= 60 ? '👍' : '📚');
  setTxt('resC', tRight + '/' + tot);
  setTxt('resPct', pct + '%');
  setTxt('resXP', '+' + xpe + ' XP');
  el('tBar').style.width = '100%';

  var rev = '<hr class="hr"><div class="bold mb12">Review:</div>';
  TD.forEach(function(q,i){
    rev += '<div style="background:var(--s2);border-radius:8px;padding:11px;margin-bottom:8px">' +
      '<div class="between mb8"><span class="sm">Q' + (i+1) + ': ' + q.question.slice(0,70) + '…</span>' +
      '<span class="' + (q._a === q.correct ? 'gc' : 'rc2') + '">' + (q._a === q.correct ? '✓' : '✗') + '</span></div>' +
      '<div class="xs muted">' + (q.explanation||'').slice(0,120) + '…</div></div>';
  });
  setHTML('testRev', rev);
}

function resetTest() {
  el('testSetup').style.display = 'block';
  el('testRes').style.display = 'none';
  TD = []; tIdx = 0; tRight = 0;
}

// ===========================================
// CHARTS — all driven by real S state
// ===========================================
function drawCharts() {
  if (typeof Chart === 'undefined') { setTimeout(drawCharts, 500); return; }

  // --- update summary stats ---
  el('pg-xp').textContent     = S.xp;
  el('pg-streak').textContent = S.streak;
  var tot = S.dc + S.dw;
  el('pg-acc').textContent    = tot > 0 ? Math.round(S.dc / tot * 100) + '%' : '—';
  el('pg-tests').textContent  = S.scores.length;
  el('pg-qcount').textContent = tot + ' answered';

  // XP tag
  var lvl = getLvl(S.xp);
  el('pg-xp-tag').textContent = lvl.icon + ' ' + lvl.name + ' Lv.' + lvl.num;

  // Avg score tag
  if (S.scores.length) {
    var avg = Math.round(S.scores.reduce(function(a,b){return a+b;},0) / S.scores.length);
    el('pg-avg-tag').textContent = 'Avg: ' + avg + '%';
  } else {
    el('pg-avg-tag').textContent = 'No tests yet';
  }

  // Phases tag
  var pDone = S.phaseDone.filter(Boolean).length;
  el('pg-phases-tag').textContent = pDone + ' / ' + S.phaseDone.length + ' phases';

  var gridOpts = {
    responsive: true,
    plugins: { legend: { display: false } },
    scales: {
      x: { grid: { color: 'rgba(255,255,255,0.04)' }, ticks: { color: '#7a88a8', font: { size: 9 } } },
      y: { grid: { color: 'rgba(255,255,255,0.04)' }, ticks: { color: '#7a88a8', font: { size: 9 } } }
    }
  };

  function mkLine(id, lbs, data, col) {
    var c = el(id); if (!c) return; if (c._ch) c._ch.destroy();
    c._ch = new Chart(c, {
      type: 'line',
      data: { labels: lbs, datasets: [{ data: data, borderColor: col,
        backgroundColor: col + '18', borderWidth: 2, fill: true, tension: 0.4,
        pointBackgroundColor: col, pointRadius: 3 }] },
      options: gridOpts
    });
  }

  // --- C4: XP Growth — real XP checkpoints ---
  // Build XP history: start from 0, go up by (xp / sessions) each step
  var xpHistory = (function() {
    var pts = Math.min(S.xp > 0 ? 10 : 1, 10);
    var base = Math.max(0, S.xp - pts * Math.round(S.xp / pts));
    return Array.from({length: pts}, function(_, i) {
      return Math.round(base + (S.xp - base) * (i + 1) / pts);
    });
  })();
  var xpLabels = Array.from({length: xpHistory.length}, function(_, i) {
    var d = new Date(); d.setDate(d.getDate() - (xpHistory.length - 1 - i));
    return d.toLocaleDateString('en', {month:'short', day:'numeric'});
  });
  mkLine('c4', xpLabels, xpHistory, '#a78bfa');

  // --- C2: Test Scores — real scores from S.scores ---
  if (S.scores.length > 0) {
    var scoreLabels = S.scores.map(function(_, i) { return 'Test ' + (i + 1); });
    mkLine('c2', scoreLabels, S.scores, '#34d399');
  } else {
    var c2 = el('c2'); if (c2) { if(c2._ch) c2._ch.destroy();
      c2._ch = new Chart(c2, { type: 'line',
        data: { labels: ['No tests yet'], datasets: [{ data: [0], borderColor: '#34d399',
          backgroundColor: '#34d39918', borderWidth: 2, fill: true, tension: 0.4 }] },
        options: gridOpts });
    }
  }

  // --- C1: Daily Q accuracy — correct vs wrong bars ---
  var c1 = el('c1'); if (c1) { if (c1._ch) c1._ch.destroy();
    c1._ch = new Chart(c1, {
      type: 'bar',
      data: {
        labels: ['Correct', 'Wrong', 'Accuracy %'],
        datasets: [{
          data: [S.dc, S.dw, tot > 0 ? Math.round(S.dc / tot * 100) : 0],
          backgroundColor: ['rgba(52,211,153,0.7)', 'rgba(248,113,113,0.7)', 'rgba(79,142,247,0.7)'],
          borderColor:     ['#34d399', '#f87171', '#4f8ef7'],
          borderWidth: 1, borderRadius: 5
        }]
      },
      options: {
        responsive: true,
        plugins: { legend: { display: false } },
        scales: {
          x: { grid: { color: 'rgba(255,255,255,0.04)' }, ticks: { color: '#7a88a8', font: { size: 10 } } },
          y: { grid: { color: 'rgba(255,255,255,0.04)' }, ticks: { color: '#7a88a8', font: { size: 9 } }, beginAtZero: true }
        }
      }
    });
  }

  // --- C3: Roadmap completion — doughnut phases done vs remaining ---
  var c3 = el('c3'); if (c3) { if (c3._ch) c3._ch.destroy();
    var pTotal = S.phaseDone.length || 5;
    var pDoneN = S.phaseDone.filter(Boolean).length;
    var pLeft  = pTotal - pDoneN;
    c3._ch = new Chart(c3, {
      type: 'doughnut',
      data: {
        labels: ['Completed', 'Remaining'],
        datasets: [{
          data: [pDoneN || 0, pLeft > 0 ? pLeft : (pDoneN === 0 ? 1 : 0)],
          backgroundColor: ['rgba(52,211,153,0.8)', 'rgba(58,74,106,0.5)'],
          borderColor: ['#34d399', 'transparent'],
          borderWidth: 2
        }]
      },
      options: {
        responsive: true,
        cutout: '68%',
        plugins: {
          legend: { display: true, labels: { color: '#7a88a8', font: { size: 9 }, boxWidth: 10 } }
        }
      }
    });
  }

  // --- C5: Skill radar — built from test scores + dsa topics + phase completions ---
  // Score each skill area: tests done on that topic boost its score
  var skillScores = [
    Math.min(100, 30 + S.dc * 3),                                          // Arrays (daily q)
    Math.min(100, 20 + (S.scores.length > 0 ? S.scores[S.scores.length-1] * 0.6 : 0)), // Strings
    Math.min(100, pDoneN >= 2 ? 60 + pDoneN * 8 : 20),                     // Trees (phase progress)
    Math.min(100, pDoneN >= 3 ? 50 + pDoneN * 6 : 15),                     // Graphs
    Math.min(100, S.scores.length > 0 ? Math.round(S.scores.reduce(function(a,b){return a+b;},0)/S.scores.length * 0.9) : 10), // DP
    Math.min(100, 25 + S.streak * 3),                                       // Sorting (streak)
    Math.min(100, 20 + S.dw * 2 + S.dc * 2),                               // Searching
    Math.min(100, 30 + pDoneN * 10)                                         // OOP (phases)
  ];
  var c5 = el('c5'); if (c5) { if (c5._ch) c5._ch.destroy();
    c5._ch = new Chart(c5, {
      type: 'radar',
      data: {
        labels: ['Arrays', 'Strings', 'Trees', 'Graphs', 'DP', 'Sorting', 'Searching', 'OOP'],
        datasets: [{
          data: skillScores,
          backgroundColor: 'rgba(79,142,247,0.12)',
          borderColor: '#4f8ef7', borderWidth: 2,
          pointBackgroundColor: '#4f8ef7', pointRadius: 4
        }]
      },
      options: {
        responsive: true,
        plugins: { legend: { display: false } },
        scales: {
          r: {
            grid: { color: 'rgba(255,255,255,0.06)' },
            ticks: { display: false },
            pointLabels: { color: '#7a88a8', font: { size: 10 } },
            min: 0, max: 100
          }
        }
      }
    });
  }
}

// ===========================================
// VIDEOS
// ===========================================
async function findVideos() {
  var topic = el('v-topic').value.trim();
  var lang  = el('v-lang').value;
  if (!topic) { alert('Enter a topic!'); return; }
  startLoad('vidLoad');
  setHTML('vidGrid', '');
  hideErr('vidErr');
  try {
    var d = await askJSON(
      'You are a learning resource curator. Return ONLY valid JSON, no markdown.',
      'Suggest 6 YouTube videos to learn "' + topic + '" in ' + lang + '.\n' +
      'Return ONLY:\n{"videos":[{"title":"Video Title","channel":"Channel","' +
      'url":"https://www.youtube.com/results?search_query=' + encodeURIComponent(topic) + '+tutorial",' +
      '"description":"what it covers","level":"Beginner","duration":"~30 min"}]}\n' +
      (lang !== 'English' ? 'Prefer ' + lang + ' language channels.' : '')
    );
    if (!d || !d.videos) throw new Error('No videos returned');
    var dc = {Beginner:'tg', Intermediate:'tb', Advanced:'tr'};
    var h = '<div class="g2 gap14">';
    d.videos.forEach(function(v){
      h += '<a class="yt-card" href="' + v.url + '" target="_blank">' +
        '<div class="yt-thumb">▶️</div>' +
        '<div><div class="sm bold mb8" style="line-height:1.4">' + v.title + '</div>' +
        '<div class="xs muted">' + v.channel + '</div>' +
        '<div class="row gap8 mt12" style="margin-top:6px">' +
        '<span class="tag ' + (dc[v.level]||'tb') + '" style="font-size:.6rem">' + v.level + '</span>' +
        '<span class="xs muted">' + v.duration + '</span></div>' +
        '<div class="xs muted" style="margin-top:5px;line-height:1.4">' + v.description + '</div></div></a>';
    });
    setHTML('vidGrid', h + '</div>');
  } catch(err) { showErr('vidErr', err.message); }
  stopLoad('vidLoad');
}

// ===========================================
// DSA
// ===========================================
var dsaMap = {
  arrays:     ['Two Sum','Maximum Subarray','Product Except Self','Rotate Array','Find Duplicate','Merge Intervals','Container With Most Water','Best Time to Buy Stock'],
  linkedlist: ['Reverse Linked List','Detect Cycle','Merge Sorted Lists','Remove Nth Node','LRU Cache','Add Two Numbers','Find Middle','Palindrome Check'],
  trees:      ['Inorder Traversal','Level Order','Max Depth','Path Sum','LCA','Validate BST','Serialize/Deserialize','Construct from Preorder'],
  graphs:     ['Number of Islands','Clone Graph','Course Schedule','Dijkstra','BFS/DFS','Topological Sort','Union Find','Shortest Path'],
  dp:         ['Climbing Stairs','Coin Change','LCS','0/1 Knapsack','Edit Distance','LIS','House Robber','Unique Paths'],
  sorting:    ['Merge Sort','Quick Sort','Heap Sort','Counting Sort','Radix Sort','Sort Colors','Kth Largest','Find Median'],
  searching:  ['Binary Search','Search Rotated Array','First & Last Position','Search 2D Matrix','Peak Element','Koko Eating Bananas'],
  stackqueue: ['Valid Parentheses','Min Stack','Largest Rectangle','Daily Temperatures','Sliding Window Max','Queue via Stacks']
};

function setDSALang(btn, lang) {
  document.querySelectorAll('.langbtn').forEach(function(b){ b.classList.remove('on'); });
  btn.classList.add('on'); S.dsaLang = lang;
  setTxt('dsaLangTag', lang);
}
function setDSATab(tab, topic) {
  document.querySelectorAll('.tab').forEach(function(t){ t.classList.remove('on'); });
  tab.classList.add('on'); S.dsaTopic = topic;
  setTxt('dsaTopicTag', tab.textContent);
  renderDSAProbs();
  setTxt('dsaConcepts', 'Click "Load AI Content" to see concepts and code examples.');
}
function renderDSAProbs() {
  var ps = dsaMap[S.dsaTopic] || [];
  setHTML('dsaProbs', ps.map(function(p,i){
    return '<div class="qopt" style="cursor:default;margin-bottom:6px">' +
      '<span class="qkey">' + (i+1) + '</span>' +
      '<span style="flex:1;font-size:.85rem">' + p + '</span>' +
      '<button class="btn btn-g" style="padding:4px 10px;font-size:.72rem" onclick="quickLearn(\'' + esc(p) + '\')">Learn</button>' +
      '</div>';
  }).join(''));
}

async function loadDSA() {
  el('dsaLoad').classList.add('show');
  setTxt('dsaConcepts', '');
  try {
    var txt = await ask(
      'You are an expert DSA teacher. Give clear, practical explanations with working code.',
      'Explain ' + S.dsaTopic.replace('stackqueue','Stack & Queue') + ' in ' + S.dsaLang + ' with a code example.\n' +
      '1)Core concept  2)Operations & complexity  3)Working ' + S.dsaLang + ' code with comments  4)When to use  5)Interview tips'
    );
    setTxt('dsaConcepts', txt);
  } catch(err) {
    el('dsaConcepts').innerHTML = '<span style="color:var(--r)">❌ ' + err.message + '</span>';
  }
  el('dsaLoad').classList.remove('show');
}

// Init
renderDSAProbs();

// Load Chart.js
var sc = document.createElement('script');
sc.src = 'https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js';
document.head.appendChild(sc);
</script>
</body>
</html>
