<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MED MENTORY</title>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <meta name="theme-color" content="#060912">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=JetBrains+Mono:wght@500&display=swap" rel="stylesheet">
    <style>
        /* ==========================================================
           MED MENTORY — TEMA HIGH-TECH
           Escuro por padrão (HUD clínico); o botão "Modo Escuro"
           alterna para a variante clara "laboratório".
           ========================================================== */
        :root {
            --bg-color: #eaf0f8;
            --card-bg: #ffffff;
            --text-color: #0f1b33;
            --header-bg: #ffffff;
            --border-color: #d3dcec;
            --primary-color: #3d6bff;
            --primary-hover: #2f58de;
            --accent-color: #0b7fa6;
            --danger-color: #e5384f;
            --warning-bg: #e08a00;
            --sobrevivencia-bg: #7c5cf0;
            --sidebar-bg: #f8faff;
            --shadow-soft: 0 14px 32px -18px rgba(20, 40, 90, 0.28);
            --shadow-btn: 0 2px 6px rgba(20, 40, 90, 0.14);
            --glow: 0 0 0 rgba(0, 0, 0, 0);
            --edge: rgba(61, 107, 255, 0.35);
            --grid-dot: rgba(15, 27, 51, 0.07);
            --aura-1: rgba(61, 107, 255, 0.10);
            --aura-2: rgba(11, 127, 166, 0.08);
            --muted: rgba(15, 27, 51, 0.62);
            --font-display: 'Plus Jakarta Sans', 'Segoe UI', system-ui, sans-serif;
            --font-body: 'Plus Jakarta Sans', 'Segoe UI', system-ui, sans-serif;
            --font-mono: 'JetBrains Mono', ui-monospace, Consolas, monospace;
        }

        .dark-mode {
            --bg-color: #060912;
            --card-bg: #0d1424;
            --text-color: #e6edf8;
            --header-bg: #0d1424;
            --border-color: rgba(130, 165, 255, 0.16);
            --primary-color: #4170ff;
            --primary-hover: #6a92ff;
            --accent-color: #22d3ee;
            --danger-color: #ff4d6a;
            --warning-bg: #ffb454;
            --sobrevivencia-bg: #9b7bff;
            --sidebar-bg: #080d19;
            --shadow-soft: 0 20px 40px -26px rgba(0, 0, 0, 0.9);
            --shadow-btn: 0 2px 8px rgba(0, 0, 0, 0.45);
            --glow: 0 0 22px rgba(34, 211, 238, 0.16);
            --edge: rgba(34, 211, 238, 0.45);
            --grid-dot: rgba(160, 190, 255, 0.07);
            --aura-1: rgba(65, 112, 255, 0.16);
            --aura-2: rgba(34, 211, 238, 0.08);
            --muted: rgba(230, 237, 248, 0.62);
            color-scheme: dark;
        }

        * { scrollbar-width: thin; scrollbar-color: var(--border-color) transparent; }

        body {
            font-family: var(--font-body);
            margin: 0; padding: 0;
            background:
                radial-gradient(900px 520px at 88% -8%, var(--aura-1), transparent 62%),
                radial-gradient(700px 420px at -4% 104%, var(--aura-2), transparent 60%),
                var(--bg-color);
            color: var(--text-color);
            transition: background 0.3s, color 0.3s;
            height: 100vh;
            overflow: hidden;
            -webkit-font-smoothing: antialiased;
        }

        h1, h2, h3, h4, .stat-box h3, .sidebar-logo h1 { font-family: var(--font-display); letter-spacing: -0.01em; }

        /* ESTRUTURA GLOBAL */
        .app-container { display: flex; height: 100vh; width: 100%; position: relative; }

        .sidebar {
            width: 272px;
            background: var(--sidebar-bg);
            border-right: 1px solid var(--border-color);
            display: flex; flex-direction: column;
            padding: 24px 16px;
            box-sizing: border-box;
            z-index: 100;
        }
        .sidebar-logo { margin-bottom: 28px; padding: 0 10px; }
        .sidebar-logo h1 {
            font-size: 19px; margin: 0; font-weight: 700; display: flex; align-items: center; gap: 8px;
            background: linear-gradient(100deg, var(--text-color) 30%, var(--accent-color));
            -webkit-background-clip: text; background-clip: text; color: transparent;
        }
        .sidebar-logo p { margin: 6px 0 0; font-size: 11.5px; color: var(--muted); line-height: 1.4; }

        .sidebar-nav { display: flex; flex-direction: column; gap: 4px; flex: 1; overflow-y: auto; padding-right: 4px; }
        .sidebar-btn {
            background: transparent; color: var(--muted);
            border: 1px solid transparent; padding: 11px 14px; border-radius: var(--r-md);
            text-align: left; font-size: 13.5px; font-weight: 500; font-family: inherit;
            cursor: pointer; transition: background 0.2s, color 0.2s;
            width: 100%; margin: 0; box-shadow: none;
        }
        .sidebar-btn:hover { background: color-mix(in srgb, var(--primary-color) 9%, transparent); color: var(--text-color); transform: none; filter: none; box-shadow: none; }
        .sidebar-btn.active {
            background: color-mix(in srgb, var(--primary-color) 16%, transparent);
            color: var(--text-color); font-weight: 600;
            border-color: color-mix(in srgb, var(--primary-color) 38%, transparent);
            box-shadow: inset 3px 0 0 var(--accent-color), var(--glow);
        }

        .main-content {
            flex: 1; display: flex; flex-direction: column;
            height: 100vh; overflow-y: auto; padding: 24px 40px 40px;
            box-sizing: border-box; scroll-behavior: smooth;
            background-image: radial-gradient(var(--grid-dot) 1px, transparent 1px);
            background-size: 26px 26px;
            background-attachment: local;
        }

        header {
            display: flex; justify-content: space-between; align-items: center;
            margin-bottom: 24px; padding-bottom: 16px;
            border-bottom: 1px solid var(--border-color);
        }
        #btnMenuMobile { display: none; }

        /* MODO FOCO, LOGIN E ALERTAS */
        .focus-overlay { display: none; position: fixed; inset: 0; background: radial-gradient(800px 500px at 50% 30%, var(--aura-1), transparent 70%), var(--bg-color); z-index: 99999; flex-direction: column; justify-content: center; align-items: center; gap: 20px; }
        body.focus-mode .focus-overlay { display: flex; }
        body.focus-mode .app-container { display: none !important; }
        .focus-dashboard { display: flex; gap: 28px; background: var(--card-bg); padding: 16px 32px; border-radius: var(--r-lg); border: 1px solid var(--border-color); text-align: center; box-shadow: var(--shadow-soft), var(--glow); }

        #loginOverlay { display: flex; position: fixed; inset: 0; background: radial-gradient(700px 480px at 50% 35%, rgba(65,112,255,0.22), rgba(3,6,14,0.94) 70%); z-index: 100000; flex-direction: column; justify-content: center; align-items: center; backdrop-filter: blur(6px); }
        .login-box { background: var(--card-bg); color: var(--text-color); padding: 40px; border-radius: var(--r-xl); width: 90%; max-width: 420px; text-align: center; border: 1px solid var(--border-color); box-shadow: 0 30px 70px rgba(0,0,0,0.55), var(--glow); }
        .login-box h2 { color: var(--accent-color); margin-top: 0; }

        .alert-bar { background-color: var(--danger-color); color: #fff; padding: 12px; text-align: center; font-weight: 600; border-radius: var(--r-md); margin-bottom: 20px; box-shadow: var(--shadow-btn); }

        /* PAINÉIS */
        .card, .stat-box, .modal-content, .login-box { position: relative; }
        .card {
            background: var(--card-bg); padding: 24px;
            border-radius: var(--r-lg); box-shadow: var(--shadow-soft);
            margin-bottom: 24px; border: 1px solid var(--border-color);
        }
        .card::before, .stat-box::before {
            content: ""; position: absolute; inset: 0 14px auto 14px; height: 1px;
            background: linear-gradient(90deg, transparent, var(--edge), transparent);
            pointer-events: none;
        }
        h2 { font-size: 17px; margin-top: 0; color: var(--text-color); font-weight: 600; }
        h3, h4 { color: var(--text-color); }

        /* CAMPOS E BOTÕES */
        label { font-weight: 500; display: block; margin-top: 10px; font-size: 12.5px; color: var(--muted); }
        select, input, textarea {
            width: 100%; padding: 10px 12px; margin-top: 6px; margin-bottom: 12px;
            border: 1px solid var(--border-color); border-radius: var(--r-md);
            background: var(--bg-color); color: var(--text-color);
            box-sizing: border-box; font-family: inherit; font-size: 14px;
            transition: border-color 0.2s, box-shadow 0.2s;
        }
        select:focus, input:focus, textarea:focus { outline: none; border-color: var(--accent-color); box-shadow: 0 0 0 3px color-mix(in srgb, var(--accent-color) 22%, transparent); }

        button {
            background: linear-gradient(135deg, var(--primary-color), color-mix(in srgb, var(--primary-color) 62%, var(--accent-color)));
            color: #fff; padding: 12px 18px; border: none; border-radius: var(--r-md);
            cursor: pointer; font-size: 14px; font-family: inherit; width: 100%; margin-top: 10px;
            font-weight: 600; transition: filter 0.2s, box-shadow 0.2s, transform 0.1s; box-shadow: var(--shadow-btn);
        }
        button:hover { filter: brightness(1.12); box-shadow: var(--shadow-btn), var(--glow); }
        button:active { transform: scale(0.98); }
        button:focus-visible, a:focus-visible, .accordion-header:focus-visible { outline: 2px solid var(--accent-color); outline-offset: 2px; }

        .btn-acao { background: var(--primary-color); width: auto; padding: 8px 14px; font-size: 12px; margin: 2px; }
        .btn-excluir { background: #d92d4b; }
        .btn-edit { background: #f0a020; color: #1a1200; }

        .sub-nav-abas { display: flex; gap: 8px; margin-bottom: 15px; flex-wrap: wrap; }
        .sub-aba-btn {
            background: var(--bg-color); color: var(--text-color); border: 1px solid var(--border-color);
            padding: 8px 14px; cursor: pointer; border-radius: var(--r-sm); font-weight: 500; width: auto; font-size: 13px; margin: 0; box-shadow: none;
        }
        .sub-aba-btn:hover { border-color: var(--accent-color); filter: none; box-shadow: none; }
        .sub-aba-btn.active {
            background: color-mix(in srgb, var(--primary-color) 20%, var(--bg-color));
            color: var(--text-color); border-color: var(--primary-color); box-shadow: var(--glow);
        }

        /* TABELAS */
        table { width: 100%; border-collapse: collapse; margin-top: 10px; font-size: 13px; font-variant-numeric: tabular-nums; }
        th, td { border-bottom: 1px solid var(--border-color); padding: 12px 10px; text-align: left; }
        th { background: transparent; color: var(--muted); font-weight: 600; font-size: 12px; border-top: 1px solid var(--border-color); }
        tr:hover { background-color: color-mix(in srgb, var(--accent-color) 6%, transparent); }
        tr.atrasado { background: linear-gradient(90deg, color-mix(in srgb, var(--danger-color) 14%, transparent), transparent 60%); box-shadow: inset 3px 0 0 var(--danger-color); }

        .badge { padding: 4px 9px; border-radius: var(--r-pill); color: #fff; text-shadow: 0 1px 1px rgba(0,0,0,0.35); font-weight: 600; font-size: 11px; display: inline-block; }
        .badge-macro { margin-bottom: 5px; }

        /* DASHBOARD */
        .dashboard-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 16px; margin-bottom: 24px; }
        .stat-box { background: var(--card-bg); padding: 20px; border-radius: var(--r-lg); border: 1px solid var(--border-color); text-align: center; box-shadow: var(--shadow-soft); }
        .stat-box h3 { margin: 0; font-size: 32px; font-weight: 700; color: var(--accent-color); font-variant-numeric: tabular-nums; text-shadow: var(--glow); }
        .stat-box p { margin: 6px 0 0; font-size: 12.5px; color: var(--muted); font-weight: 500; }

        /* POMODORO, SEMANA E KANBAN */
        .pomodoro-box { display: flex; align-items: center; justify-content: space-between; background: var(--card-bg); padding: 14px 24px; border-radius: var(--r-lg); border: 1px solid var(--border-color); margin-bottom: 24px; flex-wrap: wrap; gap: 12px; box-shadow: var(--shadow-soft); }
        .timer-display { font-size: 28px; font-weight: 500; font-family: var(--font-mono); color: var(--accent-color); text-shadow: var(--glow); letter-spacing: 0.02em; }
        body.modo-sobrevivencia .pomodoro-box { border: 1px solid var(--sobrevivencia-bg); box-shadow: 0 0 22px color-mix(in srgb, var(--sobrevivencia-bg) 30%, transparent); }

        .semana-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 8px; margin-top: 15px; }
        .dia-col { background: var(--bg-color); border: 1px solid var(--border-color); border-radius: var(--r-md); padding: 8px; text-align: center; }
        .dia-col h4 { font-size: 12px; margin: 0 0 8px 0; background: var(--card-bg); color: var(--text-color); padding: 6px; border-radius: var(--r-sm); border: 1px solid var(--border-color); }
        .sel-turno { font-size: 11px; padding: 4px; width: 100%; border-radius: var(--r-sm); border: 1px solid var(--border-color); margin-bottom: 6px; }

        /* MAPA DO RODÍZIO */
        .accordion-header { background: var(--bg-color); color: var(--text-color); padding: 12px 18px; cursor: pointer; border-radius: var(--r-md); margin-top: 10px; display: flex; justify-content: space-between; align-items: center; font-weight: 600; border: 1px solid var(--border-color); transition: border-color 0.2s; }
        .accordion-header:hover { border-color: var(--accent-color); }
        .accordion-content { display: none; padding: 15px; border: 1px solid var(--border-color); border-top: none; border-radius: 0 0 10px 10px; background: var(--card-bg); overflow-x: auto; margin-top: -4px; }
        .accordion-content.active { display: block; }
        .progress-bar-bg { width: 100%; background: color-mix(in srgb, var(--text-color) 10%, transparent); border-radius: var(--r-pill); height: 8px; overflow: hidden; margin-top: 8px; }
        .progress-bar-fill { height: 100%; background: linear-gradient(90deg, var(--primary-color), var(--accent-color)); border-radius: var(--r-pill); box-shadow: var(--glow); transition: width 0.4s ease; }

        /* MODAL */
        .modal { display: none; position: fixed; inset: 0; background: rgba(3, 6, 14, 0.72); justify-content: center; align-items: center; z-index: 10000; backdrop-filter: blur(8px); }
        .modal-content { background: var(--card-bg); color: var(--text-color); padding: 30px; border-radius: var(--r-xl); width: 90%; max-width: 550px; max-height: 85vh; overflow-y: auto; border: 1px solid var(--border-color); box-shadow: 0 30px 70px rgba(0,0,0,0.5), var(--glow); }
        .grafico-container { position: relative; height: 300px; width: 100%; }

        .row-form { display: flex; gap: 12px; flex-wrap: wrap; align-items: flex-end; }
        .col-form { flex: 1; min-width: 150px; }

        /* ABAS */
        .tab-content { display: none; animation: fadeIn 0.3s ease; }
        .tab-content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }

        /* MANUAL DE BORDO */
        .manual-section { margin-bottom: 30px; }
        .manual-section h3 { color: var(--accent-color); border-bottom: 1px solid var(--border-color); padding-bottom: 8px; margin-bottom: 15px; font-size: 18px; }
        .manual-box { background: var(--bg-color); border-left: 3px solid var(--accent-color); padding: 15px 20px; border-radius: 0 10px 10px 0; margin-bottom: 15px; font-size: 14px; line-height: 1.65; }
        .manual-list li { margin-bottom: 10px; font-size: 14px; line-height: 1.65; }
        .manual-list b { color: var(--text-color); }

        /* ========================================================
           RESPONSIVIDADE MOBILE
           ======================================================== */
        .sidebar-overlay { display: none; position: fixed; inset: 0; background: rgba(3, 6, 14, 0.62); z-index: 90; backdrop-filter: blur(3px); }
        .sidebar-overlay.active { display: block; }

        @media (max-width: 850px) {
            .sidebar { position: fixed; top: 0; left: -320px; height: 100vh; transition: left 0.3s cubic-bezier(0.4, 0, 0.2, 1); box-shadow: 8px 0 30px rgba(0,0,0,0.35); }
            .sidebar.active { left: 0; }
            .main-content { padding: 15px; }
            #btnMenuMobile {
                display: flex !important; align-items: center; justify-content: center;
                width: 45px; height: 45px; border-radius: var(--r-md);
                border: 1px solid var(--border-color); background: var(--card-bg);
                color: var(--text-color); font-size: 22px; cursor: pointer;
                padding: 0; margin-right: 12px; margin-top: 0; box-shadow: var(--shadow-btn);
            }
            header { flex-direction: column; align-items: flex-start; gap: 15px; padding-bottom: 15px; }
            .header-top-row { display: flex; align-items: center; width: 100%; }
            .header-actions { width: 100%; justify-content: space-between; overflow-x: auto; padding-bottom: 5px; }
            #headerAbaTitulo { font-size: 18px !important; }
            .dashboard-grid { grid-template-columns: 1fr 1fr; }
            .pomodoro-box { flex-direction: column; text-align: center; }
            .semana-grid { grid-template-columns: 1fr; }
            .dia-col { margin-bottom: 10px; }
        }
        @media (max-width: 480px) {
            .dashboard-grid { grid-template-columns: 1fr; }
            .row-form { flex-direction: column; align-items: stretch; }
        }

        @media (prefers-reduced-motion: reduce) {
            *, *::before, *::after { animation: none !important; transition: none !important; scroll-behavior: auto !important; }
        }
    
        /* ---------- TOKENS ---------- */
        :root { --sp-1:4px; --sp-2:8px; --sp-3:12px; --sp-4:16px; --sp-5:24px; --sp-6:32px; --r-sm:8px; --r-md:10px; --r-lg:14px; --r-xl:18px; --r-pill:999px; --fs-xs:12px; --fs-sm:13px; --fs-md:14px; --fs-lg:17px; --fs-xl:22px; --ok-color:#22c55e; }

        /* ---------- ÍCONES SVG ---------- */
        .ic { width:1.2em; height:1.2em; vertical-align:-0.22em; margin-right:.4em; flex:none; }
        .ic:only-child { margin:0; }
        .ic-dot-red { color:var(--danger-color); } .ic-dot-green { color:var(--ok-color); } .ic-dot-gray { color:var(--muted); }
        .sidebar-logo h1 .ic { color:var(--accent-color); }
        .empty-ico .ic { width:1em; height:1em; margin:0; }

        /* ---------- PAINEL "REVISÕES DE HOJE" ---------- */
        .hero-review { border-color:color-mix(in srgb, var(--accent-color) 35%, var(--border-color)); box-shadow:var(--shadow-soft), var(--glow); }
        .hero-head { display:flex; justify-content:space-between; align-items:center; gap:var(--sp-4); flex-wrap:wrap; margin-bottom:var(--sp-4); }
        .hero-head h2 { font-size:var(--fs-xl); margin:0 0 var(--sp-1); }
        .btn-hero { width:auto; margin:0; padding:14px 28px; font-size:15px; border-radius:var(--r-lg); }
        .btn-hero:disabled { opacity:.45; cursor:not-allowed; filter:none; box-shadow:none; }
        .rev-list, .alert-list { display:flex; flex-direction:column; gap:var(--sp-2); }
        .rev-item { display:flex; justify-content:space-between; align-items:center; gap:var(--sp-3); padding:var(--sp-3) var(--sp-4); background:var(--bg-color); border:1px solid var(--border-color); border-left:3px solid var(--macro, var(--accent-color)); border-radius:var(--r-md); cursor:pointer; transition:border-color .2s; }
        .rev-item:hover { border-color:var(--accent-color); border-left-color:var(--macro, var(--accent-color)); }
        .rev-item strong { display:block; font-size:var(--fs-md); }
        .rev-item small, .steps small, .alert-item small { display:block; color:var(--muted); font-size:var(--fs-xs); margin-top:2px; }
        .tag { padding:3px 10px; border-radius:var(--r-pill); font-size:var(--fs-xs); font-weight:600; white-space:nowrap; }
        .tag-danger { background:color-mix(in srgb, var(--danger-color) 18%, transparent); color:var(--danger-color); }
        .tag-ok { background:color-mix(in srgb, var(--accent-color) 16%, transparent); color:var(--accent-color); }
        .peso-tag { font-size:11px; font-weight:700; padding:2px 7px; border-radius:var(--r-pill); background:color-mix(in srgb, var(--text-color) 10%, transparent); color:var(--muted); margin-left:6px; white-space:nowrap; }
        .peso-tag.p4, .peso-tag.p5 { background:color-mix(in srgb, var(--warning-bg) 22%, transparent); color:var(--warning-bg); }

        /* ---------- ONBOARDING E ALERTAS ---------- */
        .steps { list-style:none; padding:0; margin:var(--sp-4) 0 0; display:flex; flex-direction:column; gap:var(--sp-2); }
        .steps li { display:flex; align-items:center; gap:var(--sp-3); padding:var(--sp-3); background:var(--bg-color); border:1px solid var(--border-color); border-radius:var(--r-md); }
        .steps li div { flex:1; } .steps li.done { opacity:.6; }
        .step-n { width:28px; height:28px; border-radius:50%; display:grid; place-items:center; font-weight:700; font-size:var(--fs-sm); background:color-mix(in srgb, var(--accent-color) 18%, transparent); color:var(--accent-color); flex:none; }
        .step-n .ic { margin:0; }
        .alert-item { display:flex; align-items:center; gap:var(--sp-3); padding:var(--sp-3) var(--sp-4); border-radius:var(--r-md); border:1px solid var(--border-color); background:var(--bg-color); }
        .alert-item div { flex:1; }
        .alert-item.warn { border-left:3px solid var(--warning-bg); } .alert-item.warn .alert-ico { color:var(--warning-bg); }
        .alert-item.danger { border-left:3px solid var(--danger-color); } .alert-item.danger .alert-ico { color:var(--danger-color); }

        /* ---------- HEATMAP ---------- */
        .heatmap { display:grid; grid-auto-flow:column; grid-template-rows:repeat(7, 12px); grid-auto-columns:12px; gap:3px; overflow-x:auto; padding:var(--sp-2) 0; }
        .hm { display:block; width:12px; height:12px; border-radius:3px; background:color-mix(in srgb, var(--text-color) 8%, transparent); }
        .hm.l1 { background:color-mix(in srgb, var(--accent-color) 28%, transparent); } .hm.l2 { background:color-mix(in srgb, var(--accent-color) 50%, transparent); }
        .hm.l3 { background:color-mix(in srgb, var(--accent-color) 75%, transparent); } .hm.l4 { background:var(--accent-color); }
        .heat-legend { display:flex; align-items:center; gap:4px; font-size:var(--fs-xs); color:var(--muted); margin-top:var(--sp-2); }

        /* ---------- UTILITÁRIOS (substituem estilos inline repetidos) ---------- */
        .ta-c { text-align:center; } .flex-1 { flex:1; } .m-0 { margin:0; } .mt-0 { margin-top:0; } .mt-20 { margin-top:20px; }
        .hint { font-size:var(--fs-sm); color:var(--muted); }
        .fx-120 { flex:1; min-width:120px; }
        .empty-state { text-align:center; padding:var(--sp-5) var(--sp-4); color:var(--muted); }
        .empty-state b { color:var(--text-color); }
        .empty-ico { font-size:30px; margin-bottom:10px; color:var(--accent-color); }
        .list-tight { padding-left:15px; margin:0; }
        .btn-neutral { background:var(--border-color); color:var(--text-color); }
</style>
</head>
<body class="dark-mode">

    <!-- OVERLAY ESCURO PARA O MENU MOBILE -->
    <div class="sidebar-overlay" id="sidebarOverlay" onclick="toggleSidebar()"></div>

    <!-- TELA DE LOGIN FIREBASE -->
    <div id="loginOverlay">
        <div class="login-box">
            <h2>🧠⚕️ MED MENTORY CLOUD</h2>
            <p style="font-size:13px; margin-bottom:25px; opacity:0.8;">Sincronização em Nuvem Ativa</p>
            <input type="email" id="loginEmail" placeholder="Seu E-mail">
            <input type="password" id="loginPassword" placeholder="Sua Senha">
            <div style="display: flex; gap:10px;">
                <button onclick="realizarLogin()" style="background-color: var(--accent-color);">Entrar</button>
                <button onclick="realizarCadastro()" style="background-color: var(--text-color);">Criar Conta</button>
            </div>
            <button onclick="pularLoginOffline()" style="background-color: transparent; border:1px solid #7f8c8d; color:#7f8c8d; box-shadow:none; margin-top: 15px;">Continuar Offline (Usar Local)</button>
            <p id="loginError" style="color:var(--danger-color); font-size:12px; margin-top:10px;"></p>
        </div>
    </div>

    <!-- MODO FOCO -->
    <div class="focus-overlay">
        <h2>🎯 MODO FOCO ATIVO</h2>
        <div class="timer-display" id="focusTimerDisplay" style="font-size: 72px;">25:00</div>
        <div style="display: flex; gap: 10px; margin-bottom: 20px;">
            <button class="sub-aba-btn active" onclick="iniciarPomodoro()">▶️ Iniciar</button>
            <button class="sub-aba-btn" onclick="pausarPomodoro()">⏸️ Pausar</button>
            <button class="sub-aba-btn" onclick="resetarPomodoro()">🔄 Reiniciar</button>
        </div>
        <div class="focus-dashboard">
            <div><span style="font-size: 28px; font-weight: bold; color: var(--accent-color);" id="focusTotalTime">0</span><br><span style="font-size: 13px;">Minutos Hoje</span></div>
            <div><span style="font-size: 28px; font-weight: bold; color: var(--primary-color);" id="focusMetaStatus">0/0</span><br><span style="font-size: 13px;">Revisões Feitas</span></div>
            <div><span style="font-size: 28px; font-weight: bold; color: #f39c12;" id="focusStreak">0</span><br><span style="font-size: 13px;">Dias Seguidos</span></div>
        </div>
        <div style="display: flex; gap: 15px; margin-top: 30px;">
            <button onclick="toggleDarkMode()" style="background: var(--bg-color); color:var(--text-color); border:1px solid var(--border-color); width: auto; padding: 12px 25px; box-shadow:none;">🌓 Modo Escuro</button>
            <button onclick="toggleFocusMode()" style="background: var(--danger-color); width: auto; padding: 12px 30px;">Sair do Foco</button>
        </div>
    </div>

    <div class="app-container">
        
        <!-- SIDEBAR DE NAVEGAÇÃO -->
        <aside class="sidebar">
            <div class="sidebar-logo">
                <h1>🧠⚕️ MED MENTORY</h1>
                <p>Sistema Preditivo para Residência Médica</p>
            </div>
            
            <nav class="sidebar-nav">
                <button class="sidebar-btn active" onclick="mudarAba('tab-inicio', this)">🏠 Dashboard Inicial</button>
                <button class="sidebar-btn" onclick="mudarAba('tab-planejamento', this)">📅 Cronograma de Atividades</button>
                <button class="sidebar-btn" onclick="mudarAba('tab-rastreio', this)">🧭 Rastreio de Temas</button>
                <button class="sidebar-btn" onclick="mudarAba('tab-estudo', this)">✍️ Registro de Estudo</button>
                <button class="sidebar-btn" onclick="mudarAba('tab-analytics', this)">📊 Radar de Evolução</button>
                <button class="sidebar-btn" onclick="mudarAba('tab-nerd', this)">🤓 Carga de Estudo</button>
                <button class="sidebar-btn" onclick="mudarAba('tab-caderno', this)">📚 Metas & Incidência</button>
                <button class="sidebar-btn" onclick="mudarAba('tab-manual', this)">📖 Manual de Bordo</button>
                <button class="sidebar-btn" onclick="mudarAba('tab-config', this)">⚙️ Configurações</button>
            </nav>

            <div style="margin-top:auto; padding-top:15px; font-size:11px; opacity:0.5; text-align:center;">
                MED MENTORY v5.9
            </div>
        </aside>

        <!-- CONTEÚDO PRINCIPAL -->
        <main class="main-content">
            
            <!-- CABEÇALHO DO MAIN (COM BOTÃO MOBILE) -->
            <header>
                <div class="header-top-row">
                    <button id="btnMenuMobile" aria-label="Abrir menu" onclick="toggleSidebar()">☰</button>
                    <h2 id="headerAbaTitulo" style="margin:0; font-size:22px; font-weight:700;">🏠 Dashboard Inicial</h2>
                </div>
                <div class="header-actions" style="display: flex; gap: 8px; align-items:center;">
                    <span id="userInfoHeader" style="font-size:12px; margin-right:15px; font-weight:bold; background:var(--bg-color); padding:6px 12px; border-radius:20px;">Offline (Local)</span>
                    <button class="sub-aba-btn m-0" onclick="toggleSobrevivencia()" id="btnSobrevivencia">🛡️ Sobrevivência</button>
                    <button class="sub-aba-btn active m-0" onclick="toggleFocusMode()">🎯 Foco</button>
                    <button class="sub-aba-btn" onclick="fazerLogout()" style="background:var(--danger-color); color:white; border-color:var(--danger-color); display:none; margin:0;" id="btnLogout">Sair</button>
                </div>
            </header>

            <div id="retaFinalAlert" class="alert-bar" style="display:none;"></div>
            <div id="sobrevivenciaAlert" class="alert-bar" style="display:none; background:var(--sobrevivencia-bg);">🛡️ MODO SOBREVIVÊNCIA ATIVO: Exibindo estritamente revisões para hoje e atrasadas.</div>
            <div id="sprintAlert" class="alert-bar" style="display:none; background:var(--accent-color);">⚡ SPRINT ATIVO: Exibindo estritamente temas com Pesos 4 e 5 para reta final.</div>

            <!-- ABA 1: PÁGINA INICIAL -->
            <div id="tab-inicio" class="tab-content active">

                <section class="card onboarding" id="onboardingCard" style="display:none;"></section>

                <section class="card hero-review" id="heroRevisoes">
                    <div class="hero-head">
                        <div><h2>Revisões de hoje</h2><p class="hint m-0" id="heroResumo">Carregando...</p></div>
                        <button class="btn-hero" id="btnComecarRevisao" onclick="comecarRevisao()">▶️ Começar revisão</button>
                    </div>
                    <div id="listaRevisoesHoje" class="rev-list"></div>
                </section>

                <div class="dashboard-grid">
                    <div class="stat-box"><h3 id="statHoje">0</h3><p>Para Hoje</p></div>
                    <div class="stat-box"><h3 id="statAtrasadas" style="color: var(--danger-color);">0</h3><p>Atrasadas</p></div>
                    <div class="stat-box"><h3 id="statPendentesTeoria" style="color: #f39c12;">0</h3><p>Aguardando Questões</p></div>
                    <div class="stat-box">
                        <h3 id="statCarga3Dias">0</h3><p>Carga Próx. 3 dias</p>
                        <div id="burnoutAlert" style="display:none; color:var(--danger-color); font-weight:bold; font-size:12px; margin-top:8px;">⚠️ Risco de Saturação!</div>
                    </div>
                </div>

                <section class="card" id="alertasCard"><h2>Alertas inteligentes</h2><div id="alertasInteligentes" class="alert-list"></div></section>

                
                <div class="pomodoro-box">
                    <div style="font-size:14px;">🔥 <strong>Sequência:</strong> <span id="streakCount">0</span> dias | 🎯 <strong>Metas FSRS:</strong> <span id="metaStatus">0/0</span> concluídas.</div>
                    <div class="timer-display" id="timerDisplay">25:00</div>
                    <div style="display: flex; gap: 8px; width: auto;">
                        <button class="btn-acao" style="padding:10px 15px; font-size:14px; margin:0; border-radius:8px;" onclick="iniciarPomodoro()" aria-label="Iniciar">▶️</button>
                        <button class="btn-acao" style="padding:10px 15px; font-size:14px; margin:0; border-radius:8px; background:var(--text-color);" onclick="pausarPomodoro()" aria-label="Pausar">⏸️</button>
                        <button class="btn-acao" style="padding:10px 15px; font-size:14px; margin:0; border-radius:8px; background:var(--border-color); color:var(--text-color);" onclick="resetarPomodoro()" aria-label="Reiniciar">🔄</button>
                    </div>
                </div>

                <!-- PAINEL DE RITMO PREDITIVO -->
                <div class="card">
                    <h2 style="font-size: 16px; margin-bottom: 15px;">🎯 Meta Anual de Questões: Painel de Ritmo Preditivo</h2>
                    <div style="display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 13px; font-weight: bold;">
                        <span id="metaAnualProgressoText">0 / 10000 questões resolvidas</span>
                        <span id="metaAnualPercent">0%</span>
                    </div>
                    <div class="progress-bar-bg" style="margin-bottom: 20px;">
                        <div id="metaAnualBar" class="progress-bar-fill" style="width: 0%;"></div>
                    </div>
                    <div class="dashboard-grid" style="margin-bottom:0; gap:15px;">
                        <div class="stat-box" style="padding:15px; box-shadow:none; background:var(--bg-color);">
                            <h4 style="margin:0; font-size:11px; opacity:0.6; text-transform:uppercase;">RITMO ATUAL</h4>
                            <p id="metaRitmoAtual" style="font-size:18px; font-weight:bold; color:var(--accent-color); margin:8px 0;">0 qts/dia</p>
                        </div>
                        <div class="stat-box" style="padding:15px; border-left:4px solid #f39c12; box-shadow:none; background:var(--bg-color);">
                            <h4 style="margin:0; font-size:11px; opacity:0.6; text-transform:uppercase;">RITMO NECESSÁRIO</h4>
                            <p id="metaRitmoNecessario" style="font-size:18px; font-weight:bold; color:#f39c12; margin:8px 0;">0 qts/dia</p>
                            <small id="metaDiasRestantes" style="font-size:11px; opacity:0.8;">Até a prova principal</small>
                        </div>
                        <div class="stat-box" style="padding:15px; box-shadow:none; background:var(--bg-color);">
                            <h4 style="margin:0; font-size:11px; opacity:0.6; text-transform:uppercase;">STATUS PREDITIVO</h4>
                            <p id="metaStatusRitmo" style="font-size:14px; font-weight:bold; margin:10px 0;">Aguardando dados</p>
                        </div>
                    </div>
                </div>

                <!-- WIDGET DE PROVAS -->
                <div class="card">
                    <h2 style="font-size: 16px; margin-bottom: 15px;">⏳ Contagem Regressiva</h2>
                    <div id="widgetProvasContainer"></div>
                </div>
            </div>

            <!-- NOVA ABA: RASTREIO DE TEMAS -->
            <div id="tab-rastreio" class="tab-content">
                <div class="card">
                    <h2 style="color:var(--accent-color);">🧭 Auditoria e Rastreio de Temas</h2>
                    <p class="hint">Visualize facilmente onde cada tema do edital está alocado no seu planejamento de estudos.</p>
                    
                    <div class="row-form" style="margin-bottom:20px; background:var(--bg-color); padding:15px; border-radius:10px; border:1px solid var(--border-color);">
                        <div class="col-form">
                            <label>Macro-Área:</label>
                            <select id="rastreio_macro" onchange="popularEspecialidadesRastreio()"></select>
                        </div>
                        <div class="col-form">
                            <label>Especialidade:</label>
                            <select id="rastreio_esp" onchange="renderizarRastreio()"><option value="todas">📌 Todas</option></select>
                        </div>
                        <div class="col-form">
                            <label>Filtro de Status:</label>
                            <select id="rastreio_status" onchange="renderizarRastreio()">
                                <option value="todos">👁️ Todos os Temas</option>
                                <option value="livre">⚪ Não Alocados (Livres)</option>
                                <option value="alocado">🗓️ Alocados no Cronograma</option>
                                <option value="backlog">📦 No Backlog (Adiados)</option>
                                <option value="estudado">✅ Já Estudados</option>
                            </select>
                        </div>
                    </div>

                    <div style="overflow-x:auto;">
                        <table>
                            <thead>
                                <tr>
                                    <th>Subtema</th>
                                    <th class="ta-c">Peso</th>
                                    <th>Status / Localização no Tempo</th>
                                </tr>
                            </thead>
                            <tbody id="tabela-rastreio"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- ABA 2: CRONOGRAMA ADAPTADO AO INTERNATO -->
            <div id="tab-planejamento" class="tab-content">
                
                <div class="card">
                    <h2>🏥 Data de Rodízios no Internato</h2>
                    <p class="hint">Cadastre os blocos de rodízio do seu internato. O sistema calculará o número exato de semanas e montará o calendário completo.</p>
                    <form onsubmit="adicionarRodizio(event)" class="row-form">
                        <div class="col-form"><label>Início do Rodízio:</label><input type="date" id="rodInicio" required></div>
                        <div class="col-form"><label>Fim do Rodízio:</label><input type="date" id="rodFim" required></div>
                        <div class="col-form">
                            <label>Macro-Área:</label>
                            <select id="rodMacro" required></select>
                        </div>
                        <div><button type="submit" style="margin:0; padding:11px 20px;">Adicionar</button></div>
                    </form>
                    <div id="listaRodizios" style="margin-top:20px; font-size:13px;"></div>
                </div>

                <div class="card">
                    <div style="display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:10px;">
                        <h2 class="m-0">📅 Cronograma de Estudos Personalizado</h2>
                        <button onclick="gerarTodoORodizio()" style="background:var(--primary-color); width:auto; padding:10px 20px; margin:0;">🚀 Gerar Cronograma do Rodízio</button>
                    </div>
                    <p style="font-size:13px; opacity:0.8; margin-top:10px;">Alterne entre as semanas abaixo. Altere turnos para <b>Ocupado</b> em imprevistos e recalcule estritamente aquela semana sem afetar as demais.</p>
                    
                    <div class="row-form" style="margin-bottom:20px; margin-top:20px;">
                        <div class="col-form">
                            <label>Selecione a Semana Ativa:</label>
                            <select id="seletorSemanaInternato" onchange="mudarSemanaAtiva(this.value)" style="font-weight:bold; color:var(--accent-color);">
                                <option value="">Cadastre um rodízio primeiro...</option>
                            </select>
                        </div>
                        <div class="col-form" style="display:flex; align-items:flex-end;">
                            <label style="display:flex; align-items:center; gap:10px; margin:0; cursor:pointer; height:42px; background:var(--bg-color); padding:0 15px; border-radius:8px; border:1px solid var(--border-color);">
                                <input type="checkbox" id="chkMisto" checked style="width:18px; height:18px; margin:0;">
                                <span>Modo Misto (65% Rodízio / 35% Revisão Global)</span>
                            </label>
                        </div>
                    </div>

                    <!-- BOTÕES DE AÇÃO EM LOTE -->
                    <div style="background:var(--bg-color); padding:15px; border-radius:10px; border:1px solid var(--border-color); margin-bottom:20px;">
                        <b style="font-size:13px; display:block; margin-bottom:12px; color:var(--header-bg);">⚡ Ações Rápidas da Grade (Semana Ativa)</b>
                        
                        <div style="display:flex; gap:10px; flex-wrap:wrap; margin-bottom:12px;">
                            <button type="button" class="sub-aba-btn" style="background:white;" onclick="copiarSemanaAnterior()">📋 Copiar da Semana Anterior</button>
                        </div>
                        
                        <div style="display:flex; gap:10px; flex-wrap:wrap; margin-bottom:12px;">
                            <button type="button" class="sub-aba-btn" style="background:var(--accent-color); color:white;" onclick="aplicarLote(['Seg','Ter','Qua','Qui','Sex','Sáb','Dom'], ['m'], 'estudo')">📘 Manhãs Livres</button>
                            <button type="button" class="sub-aba-btn" style="background:var(--accent-color); color:white;" onclick="aplicarLote(['Seg','Ter','Qua','Qui','Sex','Sáb','Dom'], ['t'], 'estudo')">📘 Tardes Livres</button>
                            <button type="button" class="sub-aba-btn" style="background:var(--accent-color); color:white;" onclick="aplicarLote(['Seg','Ter','Qua','Qui','Sex','Sáb','Dom'], ['n'], 'estudo')">📘 Noites Livres</button>
                            <button type="button" class="sub-aba-btn" style="background:var(--primary-color); color:white;" onclick="aplicarLote(['Sáb','Dom'], ['m','t','n'], 'descanso')">🌿 FDS Livre</button>
                        </div>

                        <div style="display:flex; gap:10px; flex-wrap:wrap; margin-bottom:12px;">
                            <button type="button" class="sub-aba-btn btn-neutral" onclick="aplicarLote(['Seg','Ter','Qua','Qui','Sex','Sáb','Dom'], ['m'], 'ocupado')">🏥 Manhãs Ocupadas</button>
                            <button type="button" class="sub-aba-btn btn-neutral" onclick="aplicarLote(['Seg','Ter','Qua','Qui','Sex','Sáb','Dom'], ['t'], 'ocupado')">🏥 Tardes Ocupadas</button>
                            <button type="button" class="sub-aba-btn btn-neutral" onclick="aplicarLote(['Seg','Ter','Qua','Qui','Sex','Sáb','Dom'], ['n'], 'ocupado')">🏥 Noites Ocupadas</button>
                            <button type="button" class="sub-aba-btn btn-neutral" onclick="aplicarLote(['Sáb','Dom'], ['m','t','n'], 'ocupado')">🏥 FDS Ocupado</button>
                        </div>

                        <div style="display:flex; gap:10px; flex-wrap:wrap; border-top:1px dashed var(--border-color); padding-top:15px;">
                            <button type="button" class="sub-aba-btn" style="background:var(--danger-color); color:white;" onclick="aplicarLote(['Seg','Ter','Qua','Qui','Sex','Sáb','Dom'], ['m','t','n'], 'ocupado')">🗑️ Limpar Semana Atual (Deixar Tudo Ocupado)</button>
                            <button type="button" class="sub-aba-btn" style="background:#8b0000; color:white;" onclick="limparTodoRodizio()">⚠️ Excluir Cronograma do Rodízio</button>
                        </div>
                    </div>

                    <div class="semana-grid" id="gridSemanaSetup"></div>
                    
                    <div style="display:flex; gap:15px; margin-top:25px;">
                        <button onclick="gerarPlanoSemanaAtiva()" style="background:var(--accent-color); flex:2;">🔄 Recalcular Apenas Esta Semana (Efeito Local Isolado)</button>
                        <button onclick="salvarGradeTurnosSemana()" style="background:var(--text-color); flex:1;">💾 Salvar Turnos Desta Semana</button>
                    </div>

                    <div id="resultadoInternato" style="margin-top:30px; display:none;"></div>
                </div>

                <!-- PAINEL DE BUSCA LIVRE UNIVERSAL -->
                <div class="card" id="cardResgateEspecial" style="border: 2px dashed var(--accent-color); background: rgba(52, 152, 219, 0.02);">
                    <h2 style="color:var(--accent-color);">🔎 Busca Livre de Temas no Edital</h2>
                    <p class="hint">Pesquise qualquer assunto do edital para alocar diretamente em um turno livre desta semana (ideal para casos vistos em plantão ou temas de interesse). Adicione e depois edite manualmente no cronograma semanal.</p>
                    <input type="text" id="inputBuscaLivre" placeholder="Digite para buscar no edital (mínimo 3 letras)..." onkeyup="renderizarBuscaLivre()" style="border: 1px solid var(--accent-color); background:white;">
                    <div id="listaBuscaLivre" style="max-height:250px; overflow-y:auto; margin-top:10px;"></div>
                </div>

                <!-- TEMAS ADIADOS -->
                <div class="card">
                    <h2>📦 Temas Adiados</h2>
                    <p style="font-size:13px; opacity:0.8; margin-bottom:15px;">Temas cancelados por falta de tempo. Eles estão ordenados por peso (prioridade). Resgate-os para um turno livre ou envie direto para as revisões práticas.</p>
                    
                    <div class="sub-nav-abas" style="margin-bottom: 15px;">
                        <button class="sub-aba-btn active" id="btnFiltroBacklog_todas" onclick="setFiltroBacklog('todas')">Todas</button>
                        <button class="sub-aba-btn" id="btnFiltroBacklog_CM" onclick="setFiltroBacklog('Clínica Médica')">Clínica</button>
                        <button class="sub-aba-btn" id="btnFiltroBacklog_CIR" onclick="setFiltroBacklog('Cirurgia')">Cirurgia</button>
                        <button class="sub-aba-btn" id="btnFiltroBacklog_GO" onclick="setFiltroBacklog('Ginecologia e Obstetrícia')">GO</button>
                        <button class="sub-aba-btn" id="btnFiltroBacklog_PED" onclick="setFiltroBacklog('Pediatria')">Pediatria</button>
                        <button class="sub-aba-btn" id="btnFiltroBacklog_PREV" onclick="setFiltroBacklog('Preventiva')">Preventiva</button>
                    </div>

                    <div id="listaBacklogAdiados" style="max-height:300px; overflow-y:auto;"></div>
                </div>

                <!-- MAPA DO RODÍZIO VIGENTE -->
                <div class="card" id="cardMapaRodizio">
                    <h2>🗺️ Mapa do Rodízio Vigente</h2>
                    <p class="hint">Acompanhe o status completo de todos os temas do edital da macro-área atual.</p>
                    <div id="mapaRodizioContainer">
                        <!-- Preenchido via JS -->
                    </div>
                </div>
            </div>

            <!-- ABA 3: REGISTRO DE ESTUDO -->
            <div id="tab-estudo" class="tab-content">
                <div class="card">
                    <h2>✍️ Registrar Novo Estudo</h2>
                    <form onsubmit="salvarNovoEstudo(event)" id="formEstudo">
                        <div class="row-form">
                            <div class="col-form">
                                <label>1. Macro-Área:</label>
                                <select id="reg_macro" onchange="popularEspecialidades('reg_macro', 'reg_esp', 'reg_subtema', false)" required></select>
                            </div>
                            <div class="col-form">
                                <label>2. Especialidade:</label>
                                <select id="reg_esp" onchange="popularSubtemas('reg_macro', 'reg_esp', 'reg_subtema', false)" required disabled><option value="">Aguarde...</option></select>
                            </div>
                        </div>
                        <div style="margin-top:10px;">
                            <label>3. Subtema:</label>
                            <select id="reg_subtema" required disabled><option value="">Aguarde...</option></select>
                        </div>

                        <div style="display: flex; gap: 15px; margin-top:20px;">
                            <div class="flex-1"><label>Data da Teoria:</label><input type="date" id="reg_data" required></div>
                            <div class="flex-1"><label>Tempo Gasto (min):</label><input type="number" id="reg_tempo" placeholder="Ex: 45" required></div>
                        </div>

                        <div style="background: var(--bg-color); padding: 15px; border-radius: 8px; margin: 20px 0; border: 1px solid var(--border-color);">
                            <label style="display:flex; align-items:center; gap:12px; margin:0; cursor:pointer;">
                                <input type="checkbox" id="checkFezQuestoes" checked onchange="toggleQuestoesForm()" style="width:20px; height:20px; margin:0; accent-color:var(--primary-color);">
                                <span style="font-size:14px;">Fiz a 1ª bateria de questões hoje.</span>
                            </label>
                        </div>

                        <div id="boxQuestoesIniciais" style="display: flex; gap: 15px;">
                            <div class="flex-1"><label>Questões Feitas:</label><input type="number" id="reg_feitas" placeholder="Ex: 25"></div>
                            <div class="flex-1"><label>Acertos:</label><input type="number" id="reg_acertos" placeholder="Ex: 20"></div>
                        </div>

                        <div style="display: flex; gap: 15px; margin-top:10px;">
                            <div class="flex-1"><label>Dificuldade da Aula/Teoria:</label>
                                <select id="reg_dificuldade" required>
                                    <option value="medio">Médio</option><option value="facil">Fácil</option><option value="dificil">Difícil</option>
                                </select>
                            </div>
                            <div class="flex-1"><label>Nível de Energia:</label>
                                <select id="reg_energia" required>
                                    <option value="alta">⚡ Alta</option><option value="media" selected>🔋 Média</option><option value="baixa">🪫 Baixa</option>
                                </select>
                            </div>
                        </div>

                        <button type="submit" style="margin-top:20px; padding:14px; font-size:15px;">Salvar Estudo e Programar Revisão</button>
                    </form>
                </div>

                <div class="card">
                    <div style="display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:10px; margin-bottom:20px;">
                        <h2 class="m-0">🔁 Cronograma de Revisões </h2>
                        <div style="text-align: right;">
                            <div>
                                <button class="btn-acao" onclick="ativarSprint()" id="btnSprintConfig" style="background:var(--text-color); box-shadow:none;">⚡ Ativar Sprint</button>
                                <button class="btn-acao" onclick="limparSprint()" style="background:var(--bg-color); color:var(--text-color); border:1px solid var(--border-color); box-shadow:none;">Limpar Sprint</button>
                            </div>
                            <div style="font-size: 11px; margin-top: 8px;">Status do Sprint: <span id="sprintStatusText" style="font-weight:bold; color:var(--danger-color);">Inativo</span></div>
                        </div>
                    </div>
                    
                    <div class="sub-nav-abas" style="margin-bottom:15px;">
                        <button class="sub-aba-btn active" onclick="mudarFiltroTabela('todas', this)">Todas</button>
                        <button class="sub-aba-btn" onclick="mudarFiltroTabela('atrasadas', this)">Atrasadas 🔴</button>
                        <button class="sub-aba-btn" onclick="mudarFiltroTabela('hoje', this)">Para Hoje 🎯</button>
                    </div>
                    <table>
                        <thead><tr><th>Área / Tema</th><th>Fase / Próxima</th><th>Ações</th></tr></thead>
                        <tbody id="tabela-estudos"></tbody>
                    </table>
                </div>
            </div>

            <!-- ABA 4: MÉTRICAS DE ESTUDO -->
            <div id="tab-analytics" class="tab-content">
                <div class="card">
                    <h2>Radar & Evolução de Acertos por Macro-Áreas e Subtema</h2>
                    <div class="dashboard-grid">
                        <div class="stat-box" style="padding:25px;">
                            <h4 style="margin-top:0; margin-bottom:20px; opacity:0.8;">Radar das 5 Macro-Áreas</h4>
                            <div style="display: flex; gap: 20px; align-items: center; flex-wrap: wrap;">
                                <div class="grafico-container" style="height:250px; flex: 2; min-width: 260px;"><canvas id="graficoRadar"></canvas></div>
                                <div id="radarValoresLista" style="flex: 1; min-width: 160px; font-size: 14px; text-align: left; background:var(--bg-color); padding:15px; border-radius:12px;"></div>
                            </div>
                        </div>
                        <div class="stat-box" style="text-align: left; padding:25px;">
                            <h4 style="margin-top:0; text-align:center; margin-bottom:20px; opacity:0.8;">Evolução de Acertos por Subtema</h4>
                            <div class="row-form" style="margin-bottom:20px; font-size:13px;">
                                <div class="col-form">
                                    <select id="evo_macro" onchange="popularEspecialidades('evo_macro', 'evo_esp', 'evo_subtema', true)"></select>
                                </div>
                                <div class="col-form">
                                    <select id="evo_esp" onchange="popularSubtemas('evo_macro', 'evo_esp', 'evo_subtema', true)"><option value="todas">📌 Todas</option></select>
                                </div>
                                <div class="col-form">
                                    <select id="evo_subtema" onchange="atualizarGraficoEvolucao()"><option value="todas">📌 Todos</option></select>
                                </div>
                            </div>
                            <div class="grafico-container" style="height:200px;"><canvas id="graficoEvolucao"></canvas></div>
                        </div>
                    </div>
                </div>

                <div class="card"><h2>Constância de estudo</h2><p class="hint" id="heatmapResumo"></p><div class="heatmap" id="heatmapEstudo"></div><div class="heat-legend"><span>menos</span><i class="hm l0"></i><i class="hm l1"></i><i class="hm l2"></i><i class="hm l3"></i><i class="hm l4"></i><span>mais</span></div></div>

                <div class="card"><h2>Tendência de acertos por macro-área</h2><p class="hint">Percentual por semana. A linha tracejada marca a meta de 60%.</p><div class="grafico-container"><canvas id="graficoTendencia"></canvas></div></div>

                <div class="card">
                    <h2>📝 Simulados: Registro e Desempenho</h2>
                    <form onsubmit="salvarSimulado(event)" style="display:flex; gap:15px; flex-wrap:wrap; align-items:flex-end; background:var(--bg-color); padding:20px; border-radius:12px;">
                        <div style="flex:2; min-width:200px;"><label>Nome (Ex: ENARE #1):</label><input type="text" id="simNome" required></div>
                        <div style="flex:1; min-width:100px;"><label>Acertos:</label><input type="number" id="simAcertos" required></div>
                        <div style="flex:1; min-width:100px;"><label>Total:</label><input type="number" id="simTotal" required></div>
                        
                        <div style="width:100%; display:flex; gap:10px; margin-top:5px; flex-wrap:wrap;">
                            <input class="fx-120" type="number" id="simCM" placeholder="Acertos Clínica">
                            <input class="fx-120" type="number" id="simCIR" placeholder="Acertos Cirurgia">
                            <input class="fx-120" type="number" id="simGO" placeholder="Acertos GO">
                            <input class="fx-120" type="number" id="simPED" placeholder="Acertos Pediatria">
                            <input class="fx-120" type="number" id="simPREV" placeholder="Acertos Preventiva">
                        </div>
                        <button type="submit" style="margin:0; width:auto; padding:12px 25px;">Lançar Simulado</button>
                    </form>
                    
                    <div style="margin-top: 40px; padding-top: 20px;">
                        <h4 style="text-align:center; opacity:0.8;">Espelho de Simulados</h4>
                        <div class="grafico-container" style="height:250px;"><canvas id="graficoSimulados"></canvas></div>
                        <div id="listaSimulados" style="margin-top: 20px; font-size:13px; background:var(--bg-color); border-radius:12px; padding:15px;"></div>
                    </div>
                </div>
            </div>

            <!-- ABA 5: TEMPO TOTAL DE ESTUDO -->
            <div id="tab-nerd" class="tab-content">
                <div class="card">
                    <h2>🤓 Tempo Total de Estudo</h2>
                    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 20px; margin-top:20px;">
                        <div class="stat-box" style="border-left: 5px solid var(--primary-color);">
                            <h4 style="opacity:0.6; margin-top:0;">HOJE</h4><h3 id="nerdTempoHoje">0h 0m</h3><p>Focado</p><hr style="border:none; border-top:1px dashed var(--border-color); margin:15px 0;"><h3 id="nerdQtsHoje" style="color:var(--text-color);">0</h3><p>Questões</p>
                        </div>
                        <div class="stat-box" style="border-left: 5px solid var(--accent-color);">
                            <h4 style="opacity:0.6; margin-top:0;">7 DIAS</h4><h3 id="nerdTempoSemana">0h 0m</h3><p>Focado</p><hr style="border:none; border-top:1px dashed var(--border-color); margin:15px 0;"><h3 id="nerdQtsSemana" style="color:var(--text-color);">0</h3><p>Questões</p>
                        </div>
                        <div class="stat-box" style="border-left: 5px solid #8e44ad;">
                            <h4 style="opacity:0.6; margin-top:0;">30 DIAS</h4><h3 id="nerdTempoMes">0h 0m</h3><p>Focado</p><hr style="border:none; border-top:1px dashed var(--border-color); margin:15px 0;"><h3 id="nerdQtsMes" style="color:var(--text-color);">0</h3><p>Questões</p>
                        </div>
                    </div>
                    <h4 style="margin-top:40px; text-align:center; opacity:0.8;">🏆 Top 3 Áreas de Dedicação (Últimos 30 Dias)</h4>
                    <ul id="nerdTopAreas" style="font-size:15px; line-height:2; margin:0 auto; max-width:400px; background:var(--bg-color); padding:20px 40px; border-radius:12px; list-style:none;"></ul>
                </div>
            </div>

            <!-- ABA 6: METAS + INCIDÊNCIA DE ASSUNTOS -->
            <div id="tab-caderno" class="tab-content">
                <div class="card">
                    <h2>🎯 Metas de Questões por Subtema</h2>
                    <div class="row-form" style="margin-bottom:20px;">
                        <div class="col-form">
                            <label>Macro-Área:</label>
                            <select id="meta_macro" onchange="popularEspecialidades('meta_macro', 'meta_esp', null, true)"></select>
                        </div>
                        <div class="col-form">
                            <label>Especialidade:</label>
                            <select id="meta_esp" onchange="renderizarMetasSubtemas()"><option value="todas">📌 Selecione...</option></select>
                        </div>
                    </div>
                    <table>
                        <thead><tr><th>Subtema</th><th>Meta Sugerida</th><th>Feitas</th><th>Status</th></tr></thead>
                        <tbody id="tabela-metas-subtemas"></tbody>
                    </table>
                </div>

                <div class="card">
                    <h2>📋 Planilha de Incidência Focada</h2>
                    <div class="row-form" style="margin-bottom:20px;">
                        <div class="col-form">
                            <label>Macro-Área:</label>
                            <select id="plan_macro" onchange="popularEspecialidades('plan_macro', 'plan_esp', null, true)"></select>
                        </div>
                        <div class="col-form">
                            <label>Especialidade:</label>
                            <select id="plan_esp" onchange="renderizarPlanilha()"><option value="todas">📌 Selecione...</option></select>
                        </div>
                    </div>
                    <table>
                        <thead><tr><th style="width:30%">Subtema</th><th style="width:35%">O que focar (Texto)</th><th style="width:35%">Imagem (Cole o Link)</th></tr></thead>
                        <tbody id="tabela-planilha"></tbody>
                    </table>
                </div>
            </div>
            
            <!-- ABA: MANUAL DE BORDO -->
            <div id="tab-manual" class="tab-content">
                <div class="card">
                    <h2 style="font-size:24px; color:var(--accent-color); margin-bottom:5px;">📖 Guia Operacional</h2>
                    <p style="opacity:0.7; font-size:14px; margin-bottom:30px;">Tudo o que você precisa saber para extrair o máximo do MED MENTORY.</p>

                    <div class="manual-section">
                        <h3>1. Como Usar a Plataforma (Passo a Passo)</h3>
                        <div class="manual-box">
                            <ul class="manual-list list-tight">
                                <li><b>Passo 1: Configuração de Metas e Provas</b><br>Cadastre a data da sua Prova Principal (alvo) na aba de Configurações e defina sua Meta Anual de Questões.</li>
                                <li><b>Passo 2: Montagem do Internato e Grade Semanal</b><br>Na aba de Cronograma, insira as datas dos seus rodízios hospitalares. Na semana ativa, use as "Ações Rápidas" para preencher com um clique seus horários de plantão ou turnos livres. Depois, clique em <i>Gerar Cronograma</i> para o sistema distribuir os temas do edital nos seus turnos de estudo livres.</li>
                                <li><b>Passo 3: Registro do Estudo Teórico</b><br>Ao assistir a uma aula ou ler a teoria, vá em <i>Registro de Estudo</i>. Selecione o tema, informe o tempo gasto, sua percepção de dificuldade e o resultado das questões. O sistema agendará a revisão automaticamente.</li>
                                <li><b>Passo 4: Cumprimento das Revisões Diárias</b><br>Monitore o painel diário na aba de Registro ou no Dashboard Inicial. Execute as revisões agendadas clicando em <i>Executar</i> e lance seus acertos, erros e notas clínicas.</li>
                                <li><b>Passo 5: Gestão de Imprevistos</b><br>Teve plantão extra ou cansou? Use o botão <i>Adiar</i> no Kanban da semana. O sistema perguntará se quer realocar o tema para outro turno livre ou enviá-lo ao Backlog (Temas Adiados) para resgate posterior.</li>
                            </ul>
                        </div>
                    </div>

                    <div class="manual-section">
                        <h3>2. Como funciona o motor FSRS (Revisões)</h3>
                        <div class="manual-box">
                            <p class="mt-0">O motor do sistema utiliza uma arquitetura baseada em Repetição Espaçada adaptada para medicina, considerando quatro variáveis:</p>
                            <ul class="manual-list" style="padding-left:15px; margin-bottom:0;">
                                <li><b>Incidência em Prova (Pesos 1 a 5):</b> Temas de Peso 5 (ex: Hipertensão, Pré-natal) têm intervalos de repetição mais curtos. Temas Peso 1 demoram mais para voltar.</li>
                                <li><b>Taxa de Acerto nas Questões:</b> Ao revisar, seu percentual de acertos ajusta a memória. Taxas menores que 50% encurtam o intervalo drasticamente; acima de 85% dão mais fôlego.</li>
                                <li><b>Percepção de Dificuldade:</b> A autoavaliação (Fácil, Médio, Difícil) calibra a velocidade em que o tema avança no calendário.</li>
                                <li><b>Trava de Segurança (Reta Final):</b> Quando faltarem 45 dias ou menos para sua Prova Principal, o algoritmo trava todos os intervalos futuros em no máximo 10 dias.</li>
                            </ul>
                        </div>
                    </div>

                    <div class="manual-section">
                        <h3>3. Benefícios Práticos da Plataforma</h3>
                        <div class="manual-box">
                            <ul class="manual-list list-tight">
                                <li><b>Fim da Fadiga de Decisão:</b> Você não perde energia pensando "o que devo estudar hoje?". A lista de prioridades já está pronta ao abrir o app.</li>
                                <li><b>Adaptação ao Internato Real:</b> Diferente de PDFs estáticos, a grade semanal é maleável. Mudou a escala do hospital? Basta reconfigurar a semana isoladamente.</li>
                                <li><b>Flexibilidade com Busca Livre:</b> Permite pesquisar e puxar qualquer tema do edital para estudar no dia, ideal para revisar casos vistos à beira do leito.</li>
                                <li><b>Prevenção de Fugas:</b> Temas adiados não desaparecem. Eles ficam organizados por prioridade no Backlog esperando um turno livre.</li>
                            </ul>
                        </div>
                    </div>

                    <div class="manual-section">
                        <h3>4. Pontos Fortes que Ela Desenvolve no Aluno</h3>
                        <div class="manual-box">
                            <ul class="manual-list list-tight">
                                <li><b>Consistência Diária e Ritmo Preditivo:</b> O painel de ritmo calcula a velocidade real de resolução de questões, mostrando se o aluno está no caminho certo para bater a meta antes do exame.</li>
                                <li><b>Cobertura Equilibrada das 5 Grandes Áreas:</b> O gráfico de Radar e a lista quantitativa impedem o vício comum de estudar apenas a área do rodízio atual e negligenciar as demais.</li>
                                <li><b>Retenção Ativa de Longo Prazo:</b> Substitui o estudo passivo (apenas assistir aulas) por ciclos programados de questões e flashcards na frequência exata da curva do esquecimento.</li>
                            </ul>
                        </div>
                    </div>


                    <div class="manual-section">
                        <h3>5. Recursos Estratégicos (Avançado)</h3>
                        <div class="manual-box">
                            <ul class="manual-list list-tight">
                                <li><b>Modo Sobrevivência:</b> Em dias de plantão pesado, clique no botão "Sobrevivência" no topo da tela. Ele oculta tarefas futuras e foca só no que vence hoje e no que está atrasado.</li>
                                <li><b>Busca Livre Universal:</b> Permite pesquisar e puxar qualquer tema do edital para estudar no dia atual. Ideal para revisar casos reais atendidos no hospital (Ex: atendeu Asma, puxa a teoria na hora).</li>
                                <li><b>Espelhamento de MFC e Psiquiatria:</b> Se você cadastrar o rodízio de Psiquiatria ou MFC, o sistema adapta a grade sem desconfigurar o banco de dados oficial de Clínica Médica e Preventiva.</li>
                                <li><b>Acompanhamento de Simulados com Desdobramento por Área:</b> Além da nota geral, o sistema registra o desempenho específico nas 5 áreas, facilitando identificar onde estão os pontos fracos.</li>
                            </ul>
                        </div>
                    </div>

                </div>
            </div>

            <!-- ABA 7: CONFIGURAÇÃO -->
            <div id="tab-config" class="tab-content">
                <!-- CALENDÁRIO DE PROVAS -->
                <div class="card">
                    <h2>🎯 Calendário de Provas</h2>
                    <p class="hint">Cadastre suas provas. A "Prova Principal" ativará automaticamente o alerta e o bloqueio de revisão curta aos 45 dias.</p>
                    <form onsubmit="adicionarOuEditarProva(event)" class="row-form" style="background:var(--bg-color); padding:20px; border-radius:12px;">
                        <input type="hidden" id="provaIdEdit">
                        <div class="col-form"><label>Nome da Prova:</label><input type="text" id="provaNome" placeholder="Ex: ENARE 2028" required></div>
                        <div class="col-form"><label>Data da Prova:</label><input type="date" id="provaData" required></div>
                        <div class="col-form">
                            <label>Tipo:</label>
                            <select id="provaTipo"><option value="principal">Alvo Principal (Reta Final 45d)</option><option value="secundaria">Secundária (Sprints)</option></select>
                        </div>
                        <div><button type="submit" id="btnSalvarProva" style="margin:0; padding:12px 20px;">Adicionar Prova</button></div>
                    </form>
                    <ul id="listaProvas" style="font-size:14px; margin-top:20px; padding-left:20px; line-height:2;"></ul>
                </div>
                
                <div class="card">
                    <h2>⚙️ Calibragem do Algoritmo FSRS (Pesos 1 a 5)</h2>
                    <div style="display:grid; grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); gap:15px; margin-top:15px;">
                        <div><label>Fator Peso 5:</label><input type="number" step="0.1" id="configPeso5" value="0.5"></div>
                        <div><label>Fator Peso 4:</label><input type="number" step="0.1" id="configPeso4" value="0.7"></div>
                        <div><label>Fator Peso 3:</label><input type="number" step="0.1" id="configPeso3" value="0.9"></div>
                        <div><label>Fator Peso 2:</label><input type="number" step="0.1" id="configPeso2" value="1.1"></div>
                        <div><label>Fator Peso 1:</label><input type="number" step="0.1" id="configPeso1" value="1.4"></div>
                    </div>
                    <label class="mt-20">Intervalos Base (Dias):</label>
                    <input type="text" id="configIntervalos" value="1, 7, 14, 30, 60, 90">
                    <label>Limite de Saturação (Aviso de Burnout em 3 Dias):</label>
                    <input type="number" id="configBurnout" value="15">
                    <button class="mt-20" onclick="salvarConfiguracoes()">Salvar Configurações</button>
                </div>

                <div class="card">
                    <h2>🎨 Tema & Dados Pessoais</h2>
                    <button class="sub-aba-btn" onclick="toggleDarkMode()" style="width:100%; padding:14px; margin-bottom:25px; border:1px solid var(--border-color); background:var(--bg-color);">🌓 Alternar Tema Claro / Escuro</button>
                    
                    <label>Meta Anual de Questões:</label>
                    <input type="number" id="metaInputConfig" value="10000" onchange="atualizarMetaAnual()">

                    <h4 style="margin-top:30px; margin-bottom:10px;">Backup Manual (JSON)</h4>
                    <div style="display: flex; gap: 15px;">
                        <button class="sub-aba-btn" onclick="exportarBackup()" style="flex:1; background:var(--accent-color); color:white; padding:12px; box-shadow:var(--shadow-btn); border:none;">💾 Exportar JSON</button>
                        <button class="sub-aba-btn" onclick="document.getElementById('importFile').click()" style="flex:1; background:var(--primary-color); color:white; padding:12px; box-shadow:var(--shadow-btn); border:none;">📂 Importar JSON</button>
                    </div>
                    <input type="file" id="importFile" style="display:none" onchange="importarBackup(event)">
                </div>
            </div>

        </main>
    </div> <!-- FIM .app-container -->

    <!-- MODAL REVISÃO -->
    <div class="modal" id="modalRevisao">
        <div class="modal-content">
            <form onsubmit="salvarRevisaoModal(event)">
                <div id="modalBodyContent"></div>
                <button type="submit">Salvar Revisão e Atualizar FSRS</button>
                <button type="button" class="btn-excluir" style="width:100%; margin-top:10px; padding:12px; border:none; color:white; border-radius:8px; font-weight:bold; cursor:pointer;" onclick="fecharModal('modalRevisao')">Cancelar</button>
            </form>
        </div>
    </div>

    <!-- MODAL ADIAR TEMA -->
    <div class="modal" id="modalAdiarTema">
        <div class="modal-content" style="max-width: 480px;">
            <h3 class="mt-0">📦 Adiar Tema</h3>
            <p id="adiarTemaTitulo" style="font-weight:bold; color:var(--accent-color); margin-bottom: 20px; font-size:14px; background:var(--bg-color); padding:10px; border-radius:8px;"></p>
            
            <label>1. O que fazer com o horário liberado?</label>
            <select id="adiarTurnoDestino">
                <option value="ocupado">🏥 Marcar como Ocupado (Plantão / Cansaço)</option>
                <option value="revisao">🔁 Marcar como Revisão FSRS</option>
                <option value="estudo">📘 Deixar como Horário de Estudo Livre</option>
            </select>

            <label class="mt-20">2. Para onde enviar este tema?</label>
            <select id="adiarTemaDestino" onchange="toggleOpcaoRealocar(this.value)">
                <option value="backlog">📦 Colocar no Temas Adiados (Backlog)</option>
                <option value="realocar">🗓️ Alocar em outro horário livre nesta semana</option>
            </select>

            <div id="boxRealocarTurno" style="display:none; margin-top:15px; padding:15px; background:var(--bg-color); border-radius:8px; border:1px solid var(--border-color);">
                <label class="mt-0">Selecione o turno vago de destino:</label>
                <select id="selSlotRealocar" style="margin-bottom:0;"></select>
            </div>

            <div id="boxMotivoBacklog" style="margin-top:15px; padding:15px; background:var(--bg-color); border-radius:8px; border:1px solid var(--border-color);">
                <label class="mt-0">Motivo do adiamento:</label>
                <input type="text" id="adiarMotivoInput" placeholder="Ex: Plantão extra, cansaço, imprevisto..." style="margin-bottom:0;">
            </div>

            <div style="display:flex; gap:15px; margin-top:25px;">
                <button type="button" onclick="confirmarAdiarTema()" style="flex:1; margin:0;">Confirmar</button>
                <button type="button" class="btn-excluir" style="flex:1; margin:0;" onclick="fecharModal('modalAdiarTema')">Cancelar</button>
            </div>
        </div>
    </div>

    <!-- MODAL DETALHES -->
    <div class="modal" id="modalDetalhes">
        <div class="modal-content">
            <div id="detalhesBodyContent"></div>
            <button type="button" class="sub-aba-btn" style="width:100%; margin-top:20px; padding:12px; border:1px solid var(--border-color); background:var(--bg-color);" onclick="fecharModal('modalDetalhes')">Fechar</button>
        </div>
    </div>
<!-- SCRIPT DE INFRAESTRUTURA FIREBASE -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyDbggm0tW-E3NaeSYJ_d4_wje6ncE4gk3o",
            authDomain: "aprovados-da44a.firebaseapp.com",
            projectId: "aprovados-da44a",
            storageBucket: "aprovados-da44a.firebasestorage.app",
            messagingSenderId: "248559853817",
            appId: "1:248559853817:web:f9b6e1f973b92254120228"
        };

        let app, auth, db;
        try {
            app = initializeApp(firebaseConfig);
            auth = getAuth(app);
            db = getFirestore(app);
            let fw = document.getElementById('firebaseWarning');
            if(fw) fw.style.display = 'none';
        } catch(e) {
            console.error("Erro ao inicializar Firebase", e);
        }

        window.realizarLogin = async () => {
            if(!auth) return alert("Configure as chaves do Firebase primeiro no HTML!");
            const email = document.getElementById('loginEmail').value;
            const pass = document.getElementById('loginPassword').value;
            try {
                await signInWithEmailAndPassword(auth, email, pass);
            } catch(e) { document.getElementById('loginError').innerText = "Erro ao logar: " + e.message; }
        };

        window.realizarCadastro = async () => {
            if(!auth) return alert("Configure as chaves do Firebase primeiro no HTML!");
            const email = document.getElementById('loginEmail').value;
            const pass = document.getElementById('loginPassword').value;
            try {
                await createUserWithEmailAndPassword(auth, email, pass);
            } catch(e) { document.getElementById('loginError').innerText = "Erro ao criar: " + e.message; }
        };

        window.fazerLogout = async () => {
            if(auth) await signOut(auth);
            window.currentUserUid = null;
            document.getElementById('loginOverlay').style.display = 'flex';
            document.getElementById('userInfoHeader').innerText = "Offline (Local)";
            document.getElementById('btnLogout').style.display = 'none';
        };

        window.pularLoginOffline = () => {
            document.getElementById('loginOverlay').style.display = 'none';
            window.loadLocalState();
        };

        window.firebaseSaveData = async (uid, data) => {
            if(!db) return;
            try {
                await setDoc(doc(db, "usuarios", uid), data);
            } catch(e) { console.error("Erro ao salvar na nuvem:", e); }
        };

        if(auth) {
            onAuthStateChanged(auth, async (user) => {
                if(user) {
                    window.currentUserUid = user.uid;
                    document.getElementById('loginOverlay').style.display = 'none';
                    document.getElementById('userInfoHeader').innerText = user.email;
                    document.getElementById('btnLogout').style.display = 'inline-block';
                    
                    try {
                        const docSnap = await getDoc(doc(db, "usuarios", user.uid));
                        if(docSnap.exists()) {
                            window.hydrateData(docSnap.data());
                        } else {
                            window.loadLocalState();
                            window.syncToCloud();
                        }
                    } catch(e) {
                        window.loadLocalState();
                    }
                } else {
                    document.getElementById('loginOverlay').style.display = 'flex';
                }
            });
        } else {
            setTimeout(() => {
                document.getElementById('loginOverlay').style.display = 'none';
                window.loadLocalState();
            }, 300);
        }
    </script>

    <!-- SCRIPT DE LÓGICA DA APLICAÇÃO -->
    <script>
        
        // ------------------------------------------------------------------
        // UTILITÁRIOS GERAIS (CORES POR MACRO-ÁREA E STATUS)
        // ------------------------------------------------------------------
        function getMacroColor(macro) {
            // Vermelho (var(--danger-color)) é reservado para alertas e atrasos; nenhuma macro-área usa vermelho.
            const cores = { 'Clínica Médica': '#3b82f6', 'Cirurgia': '#14b8a6', 'Ginecologia e Obstetrícia': '#a855f7', 'Pediatria': '#ec4899', 'Preventiva': '#16a34a', 'MFC': '#84cc16', 'Psiquiatria': '#eab308' };
            return cores[macro] || '#3b82f6';
        }

        // Função de Auditoria: Retorna onde o tema está (Estudado, Alocado, Backlog, Livre)
        function getTemaStatus(temaNome) {
            let estudado = listaEstudos.find(e => e.tema === temaNome);
            if(estudado) return { type: 'estudado', text: '✅ Já Estudado' };

            for (let sId in cronogramaSemanasStore) {
                let cg = cronogramaSemanasStore[sId];
                for (let d of diasSemanaArr) {
                    for (let t of turnosArr) {
                        if (cg[d] && cg[d][t] && cg[d][t].tipo === 'teoria' && cg[d][t].tema === temaNome) {
                            let partes = sId.split('_');
                            let rIdx = parseInt(partes[1]);
                            let sNum = parseInt(partes[3]);
                            let rod = cronogramaInternato[rIdx];
                            let rodName = rod ? rod.macro : "Rodízio";
                            let labelT = t === 'm' ? 'Manhã' : t === 't' ? 'Tarde' : 'Noite';
                            return { type: 'alocado', text: `🗓️ Alocado: ${rodName} (Semana ${sNum}) - ${d} (${labelT})` };
                        }
                    }
                }
            }

            let inBacklog = backlogAdiados.find(b => b.tema === temaNome);
            if(inBacklog) return { type: 'backlog', text: '📦 No Backlog (Fila de Atrasados)' };

            return { type: 'livre', text: '⚪ Livre (Não Alocado)' };
        }

        // Remove alocações futuras de um tema para evitar duplicatas ao resgatar
        function removerAlocacaoAnterior(temaNome) {
            for (let sId in cronogramaSemanasStore) {
                let cg = cronogramaSemanasStore[sId];
                for (let d of diasSemanaArr) {
                    for (let t of turnosArr) {
                        if (cg[d] && cg[d][t] && cg[d][t].tipo === 'teoria' && cg[d][t].tema === temaNome) {
                            cg[d][t] = { tipo: 'estudo' }; // Devolve o turno como Estudo Livre
                        }
                    }
                }
            }
            backlogAdiados = backlogAdiados.filter(b => b.tema !== temaNome);
        }

        // ------------------------------------------------------------------
        // CONTROLE DA SIDEBAR RESPONSIVA (MOBILE)
        // ------------------------------------------------------------------
        function toggleSidebar() {
            document.querySelector('.sidebar').classList.toggle('active');
            let overlay = document.getElementById('sidebarOverlay');
            if (overlay) overlay.classList.toggle('active');
        }

        // ------------------------------------------------------------------
        // BUSCA LIVRE UNIVERSAL
        // ------------------------------------------------------------------
        function renderizarBuscaLivre() {
            let container = document.getElementById("listaBuscaLivre");
            let query = document.getElementById("inputBuscaLivre").value.toLowerCase();
            container.innerHTML = "";
            if (query.length < 3) {
                container.innerHTML = `<div class="empty-state"><div class="empty-ico">🔍</div><b>Digite pelo menos 3 letras para buscar...</b></div>`;
                return;
            }

            let resultados = [];
            Object.keys(taxonomia).forEach(m => {
                Object.keys(taxonomia[m]).forEach(e => {
                    taxonomia[m][e].forEach(sub => {
                        if (sub.n.toLowerCase().includes(query)) {
                            resultados.push({ macro: m, esp: e, tema: sub.n, peso: sub.p });
                        }
                    });
                });
            });

            if (resultados.length === 0) {
                container.innerHTML = `<div class="empty-state"><div class="empty-ico">📭</div><b>Nenhum tema encontrado no edital.</b></div>`;
                return;
            }

            resultados.forEach(res => {
                let mColor = getMacroColor(res.macro);
                let badgeStyle = `background:${mColor}; color:white; text-shadow: 0px 1px 2px rgba(0,0,0,0.4);`;
                
                let statusObj = getTemaStatus(res.tema);
                let statusHtml = "";
                let btnHtml = "";

                if(statusObj.type === 'estudado') {
                    statusHtml = `<div style="color:#27ae60; font-size:11px; font-weight:bold; margin-top:4px;">${statusObj.text}</div>`;
                    btnHtml = `<button class="btn-acao" style="margin:0; padding:6px 12px; font-size:10px; background:var(--bg-color); color:var(--text-color); border:1px solid var(--border-color); box-shadow:none;" onclick="adicionarTemaLivreSemana('${res.macro}', '${res.esp}', '${res.tema}', ${res.peso})">Re-estudar p/ Semana Atual</button>`;
                } else if (statusObj.type === 'alocado') {
                    statusHtml = `<div style="color:var(--accent-color); font-size:11px; font-weight:bold; margin-top:4px;">${statusObj.text}</div>`;
                    btnHtml = `<button class="btn-acao" style="margin:0; padding:6px 12px; font-size:10px; background:var(--bg-color); color:var(--text-color); border:1px solid var(--border-color); box-shadow:none;" onclick="adicionarTemaLivreSemana('${res.macro}', '${res.esp}', '${res.tema}', ${res.peso})">Mover p/ cá</button>`;
                } else if (statusObj.type === 'backlog') {
                    statusHtml = `<div style="color:#e67e22; font-size:11px; font-weight:bold; margin-top:4px;">${statusObj.text}</div>`;
                    btnHtml = `<button class="btn-acao" style="margin:0; padding:6px 12px; font-size:10px; box-shadow:var(--shadow-btn);" onclick="adicionarTemaLivreSemana('${res.macro}', '${res.esp}', '${res.tema}', ${res.peso})">Resgatar</button>`;
                } else {
                    statusHtml = `<div style="color:#7f8c8d; font-size:11px; font-weight:bold; margin-top:4px;">${statusObj.text}</div>`;
                    btnHtml = `<button class="btn-acao" style="margin:0; padding:8px 15px; font-size:11px; box-shadow:var(--shadow-btn);" onclick="adicionarTemaLivreSemana('${res.macro}', '${res.esp}', '${res.tema}', ${res.peso})">Adicionar</button>`;
                }
                
                container.innerHTML += `<div style="padding:12px; border-bottom:1px solid var(--border-color); display:flex; justify-content:space-between; align-items:center; font-size:13px; background:var(--card-bg);">
                    <div>
                        <span class="badge" style="${badgeStyle} margin-bottom:6px;">${res.macro}</span> <b style="color:var(--accent-color);">⭐ ${res.peso}</b><br>
                        ${res.tema}
                        ${statusHtml}
                    </div>
                    ${btnHtml}
                </div>`;
            });
        }

        function adicionarTemaLivreSemana(macro, esp, tema, peso) {
            let cg = cronogramaSemanasStore[semanaAtivaId];
            if(!cg) return alert("Gere o cronograma da semana ativa primeiro!");

            let slotsLivres = [];
            diasSemanaArr.forEach((d, dIdx) => {
                turnosArr.forEach(t => {
                    if(cg[d] && cg[d][t] && (cg[d][t].tipo === 'revisao' || cg[d][t].tipo === 'descanso' || cg[d][t].tipo === 'estudo')) {
                        let labelT = t==='m'?'Manhã':t==='t'?'Tarde':'Noite';
                        let dt = calcularDataExataRodizio(semanaAtivaId, dIdx);
                        slotsLivres.push({ d, t, desc: `${d} (${dt}) - ${labelT}` });
                    }
                });
            });

            if(slotsLivres.length === 0) return alert("Não há turnos de revisão, descanso ou estudo livres nesta semana para alocar!");

            let msg = `Escolha o turno para alocar o tema:\n` + slotsLivres.map((s, i) => `${i+1}. ${s.desc}`).join('\n');
            let esc = prompt(msg, "1");
            if(esc === null) return;
            let num = parseInt(esc) - 1;
            if(isNaN(num) || num < 0 || num >= slotsLivres.length) return alert("Opção inválida!");

            let destino = slotsLivres[num];

            // Limpa o tema se ele estiver no backlog ou em outra semana no futuro
            removerAlocacaoAnterior(tema);

            cg[destino.d][destino.t] = { tipo: 'teoria', macro: macro, esp: esp, tema: tema, peso: peso };
            
            cronogramaSemanasStore[semanaAtivaId] = cg;
            window.syncToCloud();
            
            renderizarKanbanSemanaAtiva();
            renderizarBuscaLivre();
            if(document.getElementById('tab-rastreio').classList.contains('active')) renderizarRastreio(); 
            
            alert(`Tema adicionado com sucesso na ${destino.d} (${destino.t === 'm' ? 'Manhã' : destino.t === 't' ? 'Tarde' : 'Noite'})!`);
        }

        // ------------------------------------------------------------------
        // ABA: RASTREIO E AUDITORIA DE TEMAS
        // ------------------------------------------------------------------
        function popularEspecialidadesRastreio() {
            let m = document.getElementById('rastreio_macro').value;
            let sE = document.getElementById('rastreio_esp');
            sE.innerHTML = '<option value="todas">📌 Todas as Especialidades</option>';
            if(m && m !== 'todas' && taxonomia[m]) {
                Object.keys(taxonomia[m]).forEach(e => sE.innerHTML += `<option value="${e}">${e}</option>`);
            }
            renderizarRastreio();
        }

        function renderizarRastreio() {
            let m = document.getElementById('rastreio_macro').value;
            let e = document.getElementById('rastreio_esp').value;
            let st = document.getElementById('rastreio_status').value;
            let tbody = document.getElementById('tabela-rastreio');
            tbody.innerHTML = "";

            if (!m || m === 'todas') {
                tbody.innerHTML = `<tr><td colspan='3' style='text-align:center; padding:20px; opacity:0.7;'>Selecione uma Macro-Área acima para auditar os temas.</td></tr>`;
                return;
            }

            let esps = (e === 'todas') ? Object.keys(taxonomia[m]) : [e];
            let hasResults = false;

            esps.forEach(esp => {
                if(!taxonomia[m][esp]) return;
                taxonomia[m][esp].forEach(sub => {
                    let statusObj = getTemaStatus(sub.n);
                    
                    if(st !== 'todos' && statusObj.type !== st) return;
                    hasResults = true;

                    let corType = statusObj.type === 'estudado' ? '#27ae60' : 
                                  statusObj.type === 'alocado' ? 'var(--accent-color)' : 
                                  statusObj.type === 'backlog' ? '#e67e22' : '#7f8c8d';

                    tbody.innerHTML += `<tr>
                        <td><small style="opacity:0.7">${esp}</small><br><strong style="font-size:14px; color:var(--text-color);">${sub.n}</strong><span class="peso-tag p${sub.p}">P${sub.p}</span></td>
                        <td style="text-align:center; color:var(--accent-color);"><b>⭐ ${sub.p}</b></td>
                        <td style="color:${corType}; font-weight:bold; font-size:12px;">${statusObj.text}</td>
                    </tr>`;
                });
            });

            if(!hasResults) {
                tbody.innerHTML = `<tr><td colspan='3' style='text-align:center; padding:20px; opacity:0.7;'>Nenhum tema encontrado com os filtros selecionados.</td></tr>`;
            }
        }

        // ------------------------------------------------------------------
        // MAPA DO RODÍZIO VIGENTE
        // ------------------------------------------------------------------
        let filtroMapaRodizio = 'todos';

        function setFiltroMapa(f) {
            filtroMapaRodizio = f;
            renderizarMapaRodizio();
            if(f !== 'todos') expandirTodosAcordeoes();
        }

        function toggleAccordion(id) {
            document.getElementById(id).classList.toggle('active');
        }

        function expandirTodosAcordeoes() {
            document.querySelectorAll('.accordion-content').forEach(el => el.classList.add('active'));
        }

        function recolherTodosAcordeoes() {
            document.querySelectorAll('.accordion-content').forEach(el => el.classList.remove('active'));
        }

        function renderizarMapaRodizio() {
            let container = document.getElementById("mapaRodizioContainer");
            if(!container) return;
            
            if(!semanaAtivaId || cronogramaInternato.length === 0) {
                container.innerHTML = `<div style="text-align:center; padding:30px; opacity:0.7;"><div style="font-size:36px; margin-bottom:15px;">🗺️</div><b>Nenhum rodízio ativo no momento.</b></div>`;
                return;
            }
            
            let rIdx = parseInt(semanaAtivaId.split('_')[1]);
            let rod = cronogramaInternato[rIdx];
            if(!rod) return;
            
            let macro = rod.macro;
            let temasMacro = null;
            let mapTitle = macro;

            if (macro === "Psiquiatria") {
                temasMacro = { "Psiquiatria": taxonomia["Clínica Médica"]["Psiquiatria"] };
                mapTitle = "Psiquiatria (Espelho da Clínica Médica)";
            } else if (macro === "MFC") {
                temasMacro = taxonomia["Preventiva"];
                mapTitle = "MFC (Espelho da Medicina Preventiva)";
            } else {
                temasMacro = taxonomia[macro];
            }

            if(!temasMacro) {
                container.innerHTML = "<p style='font-size:13px; opacity:0.7; padding:20px; text-align:center;'>Edital não mapeado para este rodízio.</p>";
                return;
            }
            
            let mapaAlocados = {};
            Object.keys(cronogramaSemanasStore).forEach(sId => {
                if(sId.startsWith(`rod_${rIdx}_`)) {
                    let sNum = sId.split('_')[3];
                    let cg = cronogramaSemanasStore[sId];
                    diasSemanaArr.forEach(d => {
                        turnosArr.forEach(t => {
                            if(cg[d] && cg[d][t] && cg[d][t].tipo === 'teoria') {
                                let labelT = t==='m'?'Manhã':t==='t'?'Tarde':'Noite';
                                mapaAlocados[cg[d][t].tema] = `Sem. ${sNum} (${d} - ${labelT})`;
                            }
                        });
                    });
                }
            });
            
            let mapaBacklog = {};
            backlogAdiados.forEach(b => mapaBacklog[b.tema] = true);
            
            let totalTemas = 0;
            let iniciados = 0;
            let htmlAcordeoes = "";
            
            Object.keys(temasMacro).forEach((esp, espIdx) => {
                let trs = "";
                let espTotal = 0;
                let espIniciados = 0;
                
                temasMacro[esp].forEach(sub => {
                    totalTemas++;
                    espTotal++;
                    
                    let est = listaEstudos.find(e => e.tema === sub.n);
                    if(est) { iniciados++; espIniciados++; }
                    
                    let feitas = 0, acertos = 0;
                    if(est) est.historico.forEach(h => { feitas += (h.feitas||0); acertos += (h.acertos||0); });
                    
                    let taxa = feitas > 0 ? ((acertos/feitas)*100).toFixed(0) : 0;
                    
                    let inK = mapaAlocados[sub.n];
                    let inB = mapaBacklog[sub.n];
                    
                    let statusK = est ? "✅ Estudado" : (inK ? `🗓️ ${inK}` : (inB ? "📦 Backlog" : "⚪ Não Alocado"));
                    let teoria = est ? "✅ Feito" : "⏳ Pend";
                    let anki = est ? (est.fase > 1 ? "✅ Feito" : "⏳ Pend") : "—";
                    let faseStr = est ? (est.fase === 0.5 ? "Anki Pend." : (est.fase === 1 ? "1ª Rev" : `${Math.floor(est.fase)}ª Rev`)) : "—";
                    let corDesemp = taxa >= 70 ? "#27ae60" : (taxa >= 50 ? "#f39c12" : "#c0392b");
                    let desempStr = feitas > 0 ? `<span style="color:${corDesemp}; font-weight:bold;">${taxa}%</span>` : "—";
                    
                    let mostrar = true;
                    if(filtroMapaRodizio === 'nao_alocados' && statusK !== "⚪ Não Alocado" && statusK !== "📦 Backlog") mostrar = false;
                    if(filtroMapaRodizio === 'pesos45' && sub.p < 4) mostrar = false;
                    if(filtroMapaRodizio === 'pesos32' && (sub.p > 3 || sub.p < 2)) mostrar = false;
                    if(filtroMapaRodizio === 'peso1' && sub.p !== 1) mostrar = false;
                    if(filtroMapaRodizio === 'baixo_desempenho' && (!est || feitas === 0 || taxa >= 60)) mostrar = false;
                    
                    let targetMacroForStudy = (macro === "Psiquiatria" || macro === "MFC") ? (macro === "Psiquiatria" ? "Clínica Médica" : "Preventiva") : macro;
                    let clickAcao = est ? `mudarAba('tab-estudo', document.querySelectorAll('.sidebar-btn')[3]); setTimeout(() => mudarFiltroTabela('todas', document.querySelectorAll('.sub-nav-abas .sub-aba-btn')[0]), 300);` : `iniciarTeoriaKanban('${targetMacroForStudy}', '${esp}', '${sub.n}')`;
                    
                    if(mostrar) {
                        trs += `<tr>
                            <td><b style="color:var(--text-color);">${sub.n}</b><span class="peso-tag p${sub.p}">P${sub.p}</span></td>
                            <td class="ta-c">⭐ ${sub.p}</td>
                            <td style="font-size:12px; color:#555;">${statusK}</td>
                            <td class="ta-c">${teoria}</td>
                            <td class="ta-c">${anki}</td>
                            <td class="ta-c">${feitas}</td>
                            <td class="ta-c">${desempStr}</td>
                            <td style="text-align:center; font-size:12px;">${faseStr}</td>
                            <td class="ta-c"><button class="btn-acao" style="margin:0; padding:6px 12px; font-size:11px; box-shadow:var(--shadow-btn);" onclick="${clickAcao}">${est ? 'Ver Histórico' : 'Estudar'}</button></td>
                        </tr>`;
                    }
                });
                
                if(trs !== "") {
                    let accId = `acc_esp_${espIdx}`;
                    let pctEsp = espTotal > 0 ? Math.round((espIniciados/espTotal)*100) : 0;
                    htmlAcordeoes += `
                        <div class="accordion-header" onclick="toggleAccordion('${accId}')">
                            <span>▼ ${esp}</span>
                            <span style="background:var(--card-bg); color:var(--text-color); padding:4px 10px; border-radius:12px; font-size:11px;">${espIniciados}/${espTotal} (${pctEsp}%)</span>
                        </div>
                        <div class="accordion-content" id="${accId}">
                            <table>
                                <thead><tr>
                                    <th>Subtema</th><th class="ta-c">Peso</th><th>Status</th>
                                    <th class="ta-c">Teoria</th><th class="ta-c">Flashcards</th>
                                    <th class="ta-c">Qts.</th><th class="ta-c">Desemp.</th>
                                    <th class="ta-c">Fase FSRS</th><th class="ta-c">Ação</th>
                                </tr></thead>
                                <tbody>${trs}</tbody>
                            </table>
                        </div>
                    `;
                }
            });
            
            if(htmlAcordeoes === "") htmlAcordeoes = `<div style="text-align:center; padding:30px; opacity:0.7;"><div style="font-size:36px; margin-bottom:15px;">📭</div><b>Nenhum tema para este filtro.</b></div>`;
            
            let pctGlobal = totalTemas > 0 ? Math.round((iniciados/totalTemas)*100) : 0;
            
            let painelHTML = `
                <div style="background:var(--bg-color); border:1px solid var(--border-color); padding:20px; border-radius:12px; margin-bottom:20px;">
                    <div style="display:flex; justify-content:space-between; font-weight:bold; font-size:15px; margin-bottom:8px;">
                        <span>Cobertura da Grande Área (${mapTitle})</span>
                        <span style="color:var(--primary-color);">${iniciados} / ${totalTemas} temas (${pctGlobal}%)</span>
                    </div>
                    <div class="progress-bar-bg" style="height:14px;"><div class="progress-bar-fill" style="width:${pctGlobal}%;"></div></div>
                </div>
                
                <div style="display:flex; gap:8px; flex-wrap:wrap; margin-bottom:15px; align-items:center; background:var(--bg-color); padding:12px; border-radius:10px; border:1px solid var(--border-color);">
                    <button class="sub-aba-btn" style="background:white; border-color:#ccc;" onclick="expandirTodosAcordeoes()">➕ Expandir Todos</button>
                    <button class="sub-aba-btn" style="background:white; border-color:#ccc;" onclick="recolherTodosAcordeoes()">➖ Recolher Todos</button>
                    <div style="border-left:1px solid #ccc; height:20px; margin:0 5px;"></div>
                    <button class="sub-aba-btn ${filtroMapaRodizio==='todos'?'active':''}" onclick="setFiltroMapa('todos')">Todos</button>
                    <button class="sub-aba-btn ${filtroMapaRodizio==='nao_alocados'?'active':''}" onclick="setFiltroMapa('nao_alocados')">⚪ Não Aloc. / Backlog</button>
                    <button class="sub-aba-btn ${filtroMapaRodizio==='pesos45'?'active':''}" onclick="setFiltroMapa('pesos45')">⭐ Pesos 4 e 5</button>
                    <button class="sub-aba-btn ${filtroMapaRodizio==='pesos32'?'active':''}" onclick="setFiltroMapa('pesos32')">⭐ Pesos 3 e 2</button>
                    <button class="sub-aba-btn ${filtroMapaRodizio==='peso1'?'active':''}" onclick="setFiltroMapa('peso1')">⭐ Peso 1</button>
                    <button class="sub-aba-btn ${filtroMapaRodizio==='baixo_desempenho'?'active':''}" onclick="setFiltroMapa('baixo_desempenho')">⚠️ Risco (< 60%)</button>
                </div>
                
                ${htmlAcordeoes}
            `;
            
            container.innerHTML = painelHTML;
        }

        // ------------------------------------------------------------------
        // TAXONOMIA 3 NÍVEIS OFICIAL COM NOVOS PESOS (v5.9)
        // ------------------------------------------------------------------
        const taxonomia = {
            "Clínica Médica": {
                "Cardiologia": [
                    { num: 1, nome: "Parada Cardiorrespiratória (PCR)", peso: 4 },
                    { num: 2, nome: "Bradicardias", peso: 1 },
                    { num: 3, nome: "Taquiarritmias", peso: 2 },
                    { num: 4, nome: "Fibrilação e Flutter Atrial", peso: 2 },
                    { num: 5, nome: "Síncope", peso: 1 },
                    { num: 6, nome: "Dislipidemia e Estratificação de Risco Cardiovascular", peso: 3 },
                    { num: 7, nome: "Doença Arterial Coronariana Estável (DAC Estável)", peso: 3 },
                    { num: 8, nome: "SCASSST - Síndrome Coronária Aguda Sem Supra do Segmento ST", peso: 3 },
                    { num: 9, nome: "IAMCSST (Infarto Agudo do Miocárdio com Supradesnivelamento do ST)", peso: 3 },
                    { num: 10, nome: "Síndromes Aórticas Agudas", peso: 2 },
                    { num: 11, nome: "Hipertensão Arterial Sistêmica (Parte 1) Diagnóstico...", peso: 5 },
                    { num: 12, nome: "Hipertensão Arterial Sistêmica (Parte 2) Tratamento", peso: 5 },
                    { num: 13, nome: "Hipertensão Arterial Sistêmica (Parte 3) Secundária...", peso: 5 },
                    { num: 14, nome: "Insuficiência Cardíaca (Parte 1) Classificação...", peso: 3 },
                    { num: 15, nome: "Insuficiência Cardíaca (Parte 2) Tratamento", peso: 4 },
                    { num: 16, nome: "Insuficiência Cardíaca Aguda", peso: 3 },
                    { num: 17, nome: "Choque", peso: 3 },
                    { num: 18, nome: "Semiologia Cardíaca", peso: 3 },
                    { num: 19, nome: "Valvopatias", peso: 2 },
                    { num: 20, nome: "Cardiomiopatias", peso: 3 },
                    { num: 21, nome: "Pericardiopatias", peso: 3 },
                    { num: 22, nome: "Eletrocardiograma", peso: 4 }
                ],
                "Endocrinologia": [
                    { num: 1, nome: "Introdução ao Diabetes Mellitus", peso: 4 },
                    { num: 2, nome: "Diabetes Mellitus Tipo 2", peso: 4 },
                    { num: 3, nome: "Diabetes Mellitus - Insulinoterapia e Cirurgia Metabólica", peso: 4 },
                    { num: 4, nome: "Diabetes Mellitus - Complicações Agudas", peso: 5 },
                    { num: 5, nome: "Diabetes Mellitus - Complicações Crônicas", peso: 2 },
                    { num: 6, nome: "Diabetes Mellitus - Hiperglicemia Hospitalar", peso: 2 },
                    { num: 7, nome: "Tireoide - Hipotireoidismo", peso: 5 },
                    { num: 8, nome: "Tireoide - Tireotoxicose Diagnóstico, Etiologia, Tratamento", peso: 4 },
                    { num: 9, nome: "Obesidade - Obesidade e Síndrome Metabólica", peso: 2 },
                    { num: 10, nome: "Cirurgia Bariátrica e Metabólica", peso: 4 },
                    { num: 11, nome: "Metabolismo Ósseo e Mineral - Princípios e Fisiologia", peso: 1 },
                    { num: 12, nome: "Metabolismo Ósseo e Mineral - Hipercalcemia", peso: 2 },
                    { num: 13, nome: "Metabolismo Ósseo e Mineral - Hipocalcemia", peso: 2 },
                    { num: 14, nome: "Metabolismo Ósseo e Mineral - Magnésio e Fosfato", peso: 1 },
                    { num: 15, nome: "Metabolismo Ósseo e Mineral - Vitamina D e Osteomalácia", peso: 1 },
                    { num: 16, nome: "Raquitismo", peso: 2 },
                    { num: 17, nome: "Metabolismo Ósseo e Mineral - Osteoporose e Paget", peso: 2 },
                    { num: 18, nome: "Adrenal - Morfofisiologia Adrenal", peso: 2 },
                    { num: 19, nome: "Adrenal - Hipocortisolismo (Insuficiência Adrenal)", peso: 1 },
                    { num: 20, nome: "Adrenal - Hipercortisolismo (Síndrome de Cushing)", peso: 1 },
                    { num: 21, nome: "Adrenal - Feocromocitoma e Hiperaldosteronismo", peso: 1 },
                    { num: 22, nome: "Perioperatório - Controle Glicêmico e Glicocorticóides", peso: 3 },
                    { num: 23, nome: "Hipófise - Fisiologia da Hipófise e Hipopituitarismo", peso: 2 },
                    { num: 24, nome: "Hipófise - Acromegalia e Incidentaloma Hipofisário", peso: 1 },
                    { num: 25, nome: "Hipófise - Hiperprolactinemia", peso: 2 },
                    { num: 26, nome: "Neoplasias Endócrinas Múltiplas", peso: 1 },
                    { num: 27, nome: "Hipoglicemia no Paciente não Diabético", peso: 2 },
                    { num: 28, nome: "Doenças das Adrenais e Hipófise e Noções de Obesidade", peso: 2 },
                    { num: 29, nome: "Incongruência de Gênero", peso: 1 }
                ],
                "Gastroenterologia": [
                    { num: 1, nome: "Hemorragia Digestiva Alta Não Varicosa", peso: 2 },
                    { num: 2, nome: "Hemorragia Digestiva Alta Varicosa", peso: 2 },
                    { num: 3, nome: "Hemorragia Digestiva Baixa", peso: 2 },
                    { num: 4, nome: "Doença do Refluxo Gastroesofágico e Esofagites", peso: 4 },
                    { num: 5, nome: "Disfagia, Alterações Estruturais e Motilidade", peso: 2 },
                    { num: 6, nome: "Anatomofisiologia Gástrica, Gastrites e Dispepsia", peso: 4 },
                    { num: 7, nome: "Doença Ulcerosa Péptica e Inibidores de Bomba de Prótons", peso: 4 },
                    { num: 8, nome: "Pancreatite Aguda e Crônica (Pancreatites)", peso: 4 },
                    { num: 9, nome: "Anatomia/Fisiologia do Cólon e Síndrome do Intestino Irritável", peso: 2 },
                    { num: 10, nome: "Doença Inflamatória Intestinal", peso: 5 },
                    { num: 11, nome: "Distúrbios Disabsortivos", peso: 3 },
                    { num: 12, nome: "Divertículos Colônicos e Divertículo de Meckel", peso: 5 },
                    { num: 13, nome: "Polipose Intestinal e Câncer Colorretal", peso: 5 },
                    { num: 14, nome: "Neoplasias de Estômago e Esôfago", peso: 4 },
                    { num: 15, nome: "Anatomia e Fisiologia do Pâncreas e Neoplasias Pancreáticas", peso: 2 },
                    { num: 16, nome: "Tumores Neuroendócrinos", peso: 1 }
                ],
                "Hematologia": [
                    { num: 1, nome: "Introdução ao Estudo das Anemias", peso: 3 },
                    { num: 2, nome: "Anemias Microcíticas", peso: 4 },
                    { num: 3, nome: "Anemias Macrocíticas", peso: 2 },
                    { num: 4, nome: "Anemias Hemolíticas", peso: 4 },
                    { num: 5, nome: "Anemias Associadas a Condições Não Hematológicas", peso: 4 },
                    { num: 6, nome: "Leucemias Agudas", peso: 3 },
                    { num: 7, nome: "Leucemias Crônicas, Linfomas, Mielodisplasias", peso: 1 },
                    { num: 8, nome: "Mieloma Múltiplo (Gamopatias Monoclonais)", peso: 1 },
                    { num: 9, nome: "Hemostasia I - Conceitos Básicos e Anticoagulantes", peso: 3 },
                    { num: 10, nome: "Hemostasia II - Doenças Hemostáticas", peso: 3 },
                    { num: 11, nome: "Medicina Transfusional", peso: 2 }
                ],
                "Hepatologia": [
                    { num: 1, nome: "Introdução à Hepatologia", peso: 1 },
                    { num: 2, nome: "Hepatites Virais", peso: 3 },
                    { num: 3, nome: "Hepatopatias Autoimunes", peso: 1 },
                    { num: 4, nome: "Outras Causas de Hepatopatia Crônica", peso: 1 },
                    { num: 5, nome: "Cirrose Hepática", peso: 3 },
                    { num: 6, nome: "Complicações da Cirrose Hepática", peso: 4 },
                    { num: 7, nome: "Síndrome Hepatorrenal e Síndrome Hepatopulmonar", peso: 1 },
                    { num: 8, nome: "Tumores Hepáticos", peso: 2 },
                    { num: 9, nome: "Transplante Hepático e Hepatite Fulminante", peso: 1 }
                ],
                "Infectologia": [
                    { num: 1, nome: "Meningites (Infecções do SNC)", peso: 1 },
                    { num: 2, nome: "Leptospirose", peso: 2 },
                    { num: 3, nome: "Malária", peso: 1 },
                    { num: 4, nome: "Animais Peçonhentos", peso: 1 },
                    { num: 5, nome: "Gripe", peso: 1 },
                    { num: 6, nome: "HIV", peso: 4 },
                    { num: 7, nome: "Antibióticos", peso: 1 },
                    { num: 8, nome: "Arboviroses (Dengue, Chikungunya e Zika)", peso: 4 },
                    { num: 9, nome: "Micoses Invasivas", peso: 2 },
                    { num: 10, nome: "Tuberculose", peso: 4 },
                    { num: 11, nome: "Parasitoses", peso: 1 },
                    { num: 12, nome: "Neutropenia Febril e Febre de Origem Indeterminada", peso: 1 },
                    { num: 13, nome: "Endocardite Bacteriana - Endocardite Infecciosa", peso: 1 },
                    { num: 14, nome: "Hepatoesplenomegalias Infecciosas", peso: 1 },
                    { num: 15, nome: "Síndrome Febril Íctero-Hemorrágica", peso: 1 },
                    { num: 16, nome: "Covid 19", peso: 1 },
                    { num: 17, nome: "Aula Bônus - Sífilis e Outras ISTs", peso: 4 },
                    { num: 18, nome: "Pneumonias Bacterianas", peso: 4 }
                ],
                "Nefrologia": [
                    { num: 1, nome: "Doenças Glomerulares", peso: 4 },
                    { num: 2, nome: "Lesão Renal Aguda (LRA)", peso: 3 },
                    { num: 3, nome: "Doença Renal Crônica (RDC) - Parte I", peso: 1 },
                    { num: 4, nome: "Doença Renal Crônica (RDC) - Parte II", peso: 1 },
                    { num: 5, nome: "Distúrbios Ácido-Básicos", peso: 3 },
                    { num: 6, nome: "Análise da Gasometria Arterial", peso: 1 },
                    { num: 7, nome: "Distúrbios do Sódio - Disnatremias", peso: 2 },
                    { num: 8, nome: "Distúrbios do Potássio", peso: 2 },
                    { num: 9, nome: "Túbulo-Interstício Renal", peso: 1 },
                    { num: 10, nome: "ITU - Infecção do Trato Urinário", peso: 4 },
                    { num: 11, nome: "Nefrolitíase", peso: 3 }
                ],
                "Neurologia": [
                    { num: 1, nome: "Coma e Alterações da Consciência", peso: 1 },
                    { num: 2, nome: "Anatomia, Fisiologia e Semiologia Neurológica", peso: 2 },
                    { num: 3, nome: "Cefaleias", peso: 3 },
                    { num: 4, nome: "Demências", peso: 3 },
                    { num: 5, nome: "Epilepsias", peso: 2 },
                    { num: 6, nome: "Acidentes Vasculares Cerebrais", peso: 4 },
                    { num: 7, nome: "Doenças Neuromusculares", peso: 1 },
                    { num: 8, nome: "Distúrbios do Movimento", peso: 3 },
                    { num: 9, nome: "Traumatismo Cranioencefálico - TCE", peso: 4 },
                    { num: 10, nome: "Distúrbios do Sono", peso: 1 },
                    { num: 11, nome: "Doenças Desmielinizantes e Encefalites Autoimunes", peso: 1 },
                    { num: 12, nome: "Tumores Intracranianos", peso: 1 }
                ],
                "Pneumologia": [
                    { num: 1, nome: "Derrame Pleural", peso: 4 },
                    { num: 2, nome: "Doença Pulmonar Obstrutiva Crônica (DPOC)", peso: 4 },
                    { num: 3, nome: "Tromboembolismo Pulmonar (TEP)", peso: 2 },
                    { num: 4, nome: "Introdução à Pneumologia", peso: 2 },
                    { num: 5, nome: "Pneumologia Intensiva", peso: 1 },
                    { num: 6, nome: "Asma", peso: 4 },
                    { num: 7, nome: "Miscelânea (Interstício, Bronquiectasias...)", peso: 2 },
                    { num: 8, nome: "Neoplasias Pulmonares", peso: 2 }
                ],
                "Psiquiatria": [
                    { num: 1, nome: "Transtornos do Humor", peso: 3 },
                    { num: 2, nome: "Transtornos de Ansiedade", peso: 2 },
                    { num: 3, nome: "Transtornos Psicóticos", peso: 1 },
                    { num: 4, nome: "Dependência Química", peso: 3 },
                    { num: 5, nome: "TOC e Transtornos Relacionados ao Estresse", peso: 1 },
                    { num: 6, nome: "Psiquiatria Infantil", peso: 3 },
                    { num: 7, nome: "Transtornos de Personalidade", peso: 1 },
                    { num: 8, nome: "Transtornos Alimentares", peso: 3 },
                    { num: 9, nome: "Psicofarmacologia", peso: 3 },
                    { num: 10, nome: "Psicopatologia", peso: 1 },
                    { num: 11, nome: "Psiquiatria Social e Reforma Psiquiátrica", peso: 2 },
                    { num: 12, nome: "Intoxicações Exógenas", peso: 3 }
                ],
                "Reumatologia": [
                    { num: 1, nome: "Introdução à Reumatologia", peso: 1 },
                    { num: 2, nome: "Doenças Autoimunes do Tecido Conjuntivo", peso: 3 },
                    { num: 3, nome: "Doenças Inflamatórias do Tecido Conjuntivo II", peso: 3 },
                    { num: 4, nome: "Artrite Reumatoide", peso: 4 },
                    { num: 5, nome: "Espondiloartrites", peso: 2 },
                    { num: 6, nome: "Vasculites", peso: 2 },
                    { num: 7, nome: "Artrites Microcristalinas", peso: 3 },
                    { num: 8, nome: "Síndromes Dolorosas Crônicas", peso: 1 },
                    { num: 9, nome: "Doenças do Osso e da Cartilagem", peso: 1 },
                    { num: 10, nome: "Artropatias Infecciosas", peso: 1 },
                    { num: 11, nome: "Miscelânea", peso: 1 },
                    { num: 12, nome: "Reumatologia Pediátrica", peso: 3 },
                    { num: 13, nome: "Manifestações Reumatológicas e Doenças Sistêmicas", peso: 1 }
                ],
                "Dermatologia": [
                    { num: 1, nome: "Histologia, Fisiologia da Pele e Lesões Elementares", peso: 1 },
                    { num: 2, nome: "Hanseníase", peso: 2 },
                    { num: 3, nome: "Oncologia Cutânea (Câncer de Pele)", peso: 3 },
                    { num: 4, nome: "Dermatoses Infecciosas", peso: 3 },
                    { num: 5, nome: "Dermatoses Eczematosas", peso: 1 },
                    { num: 6, nome: "Farmacodermias", peso: 2 },
                    { num: 7, nome: "Dermatoses Papuloescamosas", peso: 2 },
                    { num: 8, nome: "Dermatoses Vesicobolhosas", peso: 1 },
                    { num: 9, nome: "Miscelânea Dermatológica", peso: 3 },
                    { num: 10, nome: "Síndromes Verrucosas", peso: 2 },
                    { num: 11, nome: "Piodermites", peso: 2 }
                ]
            },
            "Cirurgia": {
                "Cirurgia Geral": [
                    { num: 1, nome: "Trauma - Avaliação Inicial, Vias Aéreas e Trauma Torácico", peso: 5 },
                    { num: 2, nome: "Trauma - Choque", peso: 2 },
                    { num: 3, nome: "Trauma Abdominal e Pélvico", peso: 4 },
                    { num: 4, nome: "Trauma de Face e Cervical", peso: 3 },
                    { num: 5, nome: "Trauma Vascular de Extremidades e Musculoesquelético", peso: 3 },
                    { num: 6, nome: "Trauma Populações Especiais (Pediátrico, Gestante e Idosos)", peso: 2 },
                    { num: 7, nome: "Queimaduras e Traumas Elétricos", peso: 3 },
                    { num: 8, nome: "Resposta Endócrino-Metabólica ao Trauma", peso: 2 },
                    { num: 9, nome: "Urgências Abdominais - Abdome Agudo", peso: 5 },
                    { num: 10, nome: "Abdome Agudo Inflamatório - Apendicite Aguda", peso: 5 },
                    { num: 11, nome: "Abdome Agudo Inflamatório - Colecistite e Colangite Aguda", peso: 5 },
                    { num: 12, nome: "Abdome Agudo Inflamatório - Diverticulite Aguda", peso: 5 },
                    { num: 13, nome: "Abdômen Agudo Vascular", peso: 1 },
                    { num: 14, nome: "Abdome Agudo Hemorrágico", peso: 2 },
                    { num: 15, nome: "Abdome Agudo Perfurativo", peso: 1 },
                    { num: 16, nome: "Abdome Agudo Obstrutivo", peso: 5 },
                    { num: 17, nome: "Vesícula e Vias Biliares", peso: 5 },
                    { num: 18, nome: "Complicações Pós-Operatórias", peso: 5 },
                    { num: 19, nome: "Hérnias da Parede Abdominal", peso: 5 },
                    { num: 20, nome: "Cirurgia Vascular", peso: 1 },
                    { num: 21, nome: "Cirurgia Torácica", peso: 1 },
                    { num: 22, nome: "Cirurgia Plástica", peso: 2 },
                    { num: 23, nome: "Urologia", peso: 5 },
                    { num: 24, nome: "Cirurgia Infantil - Parte I", peso: 3 },
                    { num: 25, nome: "Cirurgia Infantil - Parte II", peso: 3 },
                    { num: 26, nome: "Cirurgia Infantil - Parte III", peso: 3 },
                    { num: 27, nome: "Proctologia", peso: 5 },
                    { num: 28, nome: "Neoplasias do Apêndice Cecal", peso: 2 },
                    { num: 29, nome: "Nutrição em Cirurgia e Aceleração da Recuperação...", peso: 1 },
                    { num: 30, nome: "Cicatrização de Feridas", peso: 2 },
                    { num: 31, nome: "Cirurgia Bariátrica e Metabólica", peso: 2 },
                    { num: 32, nome: "Temas Gerais em Cirurgia", peso: 3 },
                    { num: 33, nome: "Medicina Perioperatória", peso: 4 }
                ],
                "Oftalmologia": [
                    { num: 1, nome: "Anatomia e Fisiologia do Aparelho Visual", peso: 1 },
                    { num: 2, nome: "Córnea e Cristalino", peso: 2 },
                    { num: 3, nome: "Glaucoma", peso: 1 },
                    { num: 4, nome: "Distúrbios da Refração", peso: 1 },
                    { num: 5, nome: "Neurooftalmologia", peso: 1 },
                    { num: 6, nome: "Traumatismos Oculares", peso: 3 },
                    { num: 7, nome: "Tumores Oculares Malignos", peso: 1 },
                    { num: 8, nome: "Síndrome do Olho Vermelho", peso: 3 },
                    { num: 9, nome: "Doenças do Polo Posterior", peso: 1 }
                ],
                "Ortopedia": [
                    { num: 1, nome: "Quadril Pediátrico", peso: 2 },
                    { num: 2, nome: "Politraumatismo Ortopédico", peso: 3 },
                    { num: 3, nome: "Doenças da Coluna Vertebral", peso: 3 },
                    { num: 4, nome: "Complicações do Trauma Ortopédico", peso: 3 },
                    { num: 5, nome: "Maus Tratos", peso: 2 },
                    { num: 6, nome: "Oncologia Ortopédica e Osteomielite", peso: 2 },
                    { num: 7, nome: "Fraturas e Luxações", peso: 2 },
                    { num: 8, nome: "Fratura Exposta", peso: 3 },
                    { num: 9, nome: "Ombro e Joelho Pediátrico", peso: 2 },
                    { num: 10, nome: "Conceitos Básicos do Trauma Ortopédico", peso: 1 },
                    { num: 11, nome: "Conceitos Básicos de Anatomia e Semiologia", peso: 1 },
                    { num: 12, nome: "Desenvolvimento Ortopédico da Criança", peso: 1 },
                    { num: 13, nome: "Doenças do Ombro e Cotovelo", peso: 1 },
                    { num: 14, nome: "Doenças da Mão e Síndromes Compressivas", peso: 1 },
                    { num: 15, nome: "Coluna Pediátrica", peso: 1 },
                    { num: 16, nome: "Doenças Neuromusculares Pediátricas", peso: 1 },
                    { num: 17, nome: "Doenças de Partes Moles", peso: 1 },
                    { num: 18, nome: "Miscelânea Ortopédica", peso: 1 }
                ],
                "Otorrinolaringologia": [
                    { num: 1, nome: "Infecções de Vias Aéreas Superiores - Parte I", peso: 4 },
                    { num: 2, nome: "Infecções das Vias Aéreas Superiores - Parte II", peso: 4 },
                    { num: 3, nome: "Infecções de Vias Aéreas Superiores - Parte III", peso: 4 },
                    { num: 4, nome: "Otoneurologia, Vertigens e Audiologia", peso: 1 },
                    { num: 5, nome: "Cirurgia de Cabeça e Pescoço - Laringologia e Apneia", peso: 1 },
                    { num: 6, nome: "Cirurgia de Cabeça e Pescoço - Nódulos de Tireoide...", peso: 3 },
                    { num: 7, nome: "Cirurgia de Cabeça e Pescoço - Neoplasias Benignas e Malignas...", peso: 1 }
                ]
            },
            "Ginecologia e Obstetrícia": {
                "Ginecologia": [
                    { num: 1, nome: "Ciclo Menstrual", peso: 2 },
                    { num: 2, nome: "Planejamento Familiar", peso: 5 },
                    { num: 3, nome: "Amenorreia", peso: 3 },
                    { num: 4, nome: "Síndrome dos Ovários Policísticos (SOP)", peso: 2 },
                    { num: 5, nome: "Endometriose", peso: 4 },
                    { num: 6, nome: "Adenomiose", peso: 1 },
                    { num: 7, nome: "Pólipos Uterinos", peso: 1 },
                    { num: 8, nome: "Rastreamento do Câncer de Mama", peso: 4 },
                    { num: 9, nome: "Sangramento Uterino Anormal (SUA)", peso: 5 },
                    { num: 10, nome: "Doenças Benignas da Mama", peso: 2 },
                    { num: 11, nome: "Incontinência Urinária", peso: 2 },
                    { num: 12, nome: "Câncer de Mama", peso: 4 },
                    { num: 13, nome: "Câncer do Corpo do Útero", peso: 5 },
                    { num: 14, nome: "Tumores Anexiais e Câncer de Ovário", peso: 4 },
                    { num: 15, nome: "Doenças da Vulva e Vagina", peso: 4 },
                    { num: 16, nome: "Vulvovaginites", peso: 5 },
                    { num: 17, nome: "Cervicites", peso: 5 },
                    { num: 18, nome: "Doença Inflamatória Pélvica (DIP)", peso: 4 },
                    { num: 19, nome: "Úlceras Genitais", peso: 5 },
                    { num: 20, nome: "Assistência à Vítima de Violência Sexual", peso: 3 },
                    { num: 21, nome: "Rastreamento do Câncer de Colo Úterino", peso: 5 },
                    { num: 22, nome: "Câncer de Colo do Útero", peso: 3 },
                    { num: 23, nome: "Climatério e Terapia Hormonal", peso: 4 },
                    { num: 24, nome: "Miomatose Uterina", peso: 1 },
                    { num: 25, nome: "Prolapsos de Órgãos Pélvicos", peso: 2 },
                    { num: 26, nome: "Anatomia e Embriologia do Trato Genital Feminino", peso: 1 },
                    { num: 27, nome: "Infertilidade Conjugal", peso: 1 },
                    { num: 28, nome: "Dor Pélvica Crônica e Dismenorreia", peso: 1 },
                    { num: 29, nome: "Sexualidade", peso: 1 },
                    { num: 30, nome: "Síndrome Pré-Menstrual", peso: 1 },
                    { num: 31, nome: "Abdome Agudo em Ginecologia", peso: 2 }
                ],
                "Obstetrícia": [
                    { num: 1, nome: "TPP - Prematuridade e Trabalho de Parto Prematuro", peso: 4 },
                    { num: 2, nome: "Rotura Prematura de Membranas (RPM)", peso: 2 },
                    { num: 3, nome: "HPP – Hemorragia Pós-Parto", peso: 4 },
                    { num: 4, nome: "Gestação Múltipla", peso: 1 },
                    { num: 5, nome: "Partograma e Distocias", peso: 3 },
                    { num: 6, nome: "Bacia Obstétrica, Pelvimetria e Estática Fetal", peso: 3 },
                    { num: 7, nome: "Infecção Puerperal", peso: 2 },
                    { num: 8, nome: "Sífilis na Gestação e Sífilis Congênitas", peso: 3 },
                    { num: 9, nome: "Infecções Congênitas na Gestação", peso: 3 },
                    { num: 10, nome: "Aloimunização Materna e Doença Hemolítica Perinatal", peso: 3 },
                    { num: 11, nome: "Alteração do volume de líquido amniótico", peso: 2 },
                    { num: 12, nome: "Modificações Fisiológicas da Gestação", peso: 2 },
                    { num: 13, nome: "Abortamento de Repetição", peso: 3 },
                    { num: 14, nome: "Síndromes Hipertensivas da Gestação", peso: 5 },
                    { num: 15, nome: "Vitalidade Fetal", peso: 5 },
                    { num: 16, nome: "Assistência ao Parto", peso: 3 },
                    { num: 17, nome: "Parto Vaginal Operatório", peso: 3 },
                    { num: 18, nome: "Mecanismo de Parto e Fases Clínicas do Parto", peso: 3 },
                    { num: 19, nome: "Indução do Parto e Pós-Datismo", peso: 2 },
                    { num: 20, nome: "Restrição de Crescimento Fetal e Óbito Fetal", peso: 3 },
                    { num: 21, nome: "Pré-natal", peso: 5 },
                    { num: 22, nome: "Ultrassom em Obstetrícia", peso: 2 },
                    { num: 23, nome: "Diabetes Mellitus na Gestação", peso: 5 },
                    { num: 24, nome: "Sangramento da Primeira Metade", peso: 5 },
                    { num: 25, nome: "Sangramento da Segunda Metade", peso: 5 }
                ]
            },
            "Pediatria": {
                "Pediatria": [
                    { num: 1, nome: "Puberdade", peso: 2 },
                    { num: 2, nome: "BRUE SMSL", peso: 1 },
                    { num: 3, nome: "Infecção de Trato Urinário em Pediatria (ITU)", peso: 3 },
                    { num: 4, nome: "Alergia Alimentar", peso: 2 },
                    { num: 5, nome: "Obesidade Infantil e na Adolescência", peso: 2 },
                    { num: 6, nome: "Outros distúrbios do período neonatal", peso: 3 },
                    { num: 7, nome: "Anafilaxia e Urticária", peso: 1 },
                    { num: 8, nome: "Crescimento", peso: 2 },
                    { num: 9, nome: "Constipação Intestinal", peso: 2 },
                    { num: 10, nome: "Hiperplasia Adrenal Congênita", peso: 1 },
                    { num: 11, nome: "Distúrbios Respiratórios do Período Neonatal", peso: 1 },
                    { num: 12, nome: "Febre Reumática", peso: 2 },
                    { num: 13, nome: "Síndromes Genéticas, Erros Inatos do Metabolismo", peso: 1 },
                    { num: 14, nome: "Enurese Noturna", peso: 1 },
                    { num: 15, nome: "Infecções Congênitas", peso: 3 },
                    { num: 16, nome: "Hipotireoidismo Congênito", peso: 1 },
                    { num: 17, nome: "Aleitamento Materno", peso: 4 },
                    { num: 18, nome: "Pneumonias na Infância", peso: 5 },
                    { num: 19, nome: "Desnutrição na Infância", peso: 1 },
                    { num: 20, nome: "Imunizações", peso: 5 },
                    { num: 21, nome: "Asma em Pediatria", peso: 4 },
                    { num: 22, nome: "Sífilis na Gestação e Sífilis Congênitas", peso: 2 },
                    { num: 23, nome: "Cuidados Neonatais", peso: 2 },
                    { num: 24, nome: "Choque em Pediatria", peso: 2 },
                    { num: 25, nome: "Deficiências Vitamínicas e Profilaxias", peso: 3 },
                    { num: 26, nome: "Convulsão Febril", peso: 3 },
                    { num: 27, nome: "Reanimação Neonatal", peso: 3 },
                    { num: 28, nome: "Doença de Kawasaki", peso: 2 },
                    { num: 29, nome: "Hipertensão Arterial na Criança e Adolescente", peso: 1 },
                    { num: 30, nome: "Púrpura de Hennoch Schonlein-Vasculite por IGA", peso: 2 },
                    { num: 31, nome: "Doenças Exantemáticas", peso: 4 },
                    { num: 32, nome: "Cardiopatias Congênitas", peso: 3 },
                    { num: 33, nome: "Doença Celíaca em Pediatria", peso: 2 },
                    { num: 34, nome: "Icterícia e Sepse Neonatal", peso: 3 },
                    { num: 35, nome: "Diagnóstico Nutricional", peso: 1 },
                    { num: 36, nome: "Coqueluche", peso: 1 },
                    { num: 37, nome: "Cefaleias na Infância", peso: 1 },
                    { num: 38, nome: "Febre na Pediatria", peso: 1 },
                    { num: 39, nome: "Segurança em Pediatria", peso: 3 },
                    { num: 40, nome: "Desenvolvimento Neuropsicomotor (DNPM)", peso: 4 },
                    { num: 41, nome: "Doença do Refluxo Gastroesofágico em Pediatria", peso: 2 },
                    { num: 42, nome: "Bronquiolite", peso: 4 },
                    { num: 43, nome: "Emergências Pediátricas", peso: 5 },
                    { num: 44, nome: "Tópicos em Pediatria", peso: 1 },
                    { num: 45, nome: "Tuberculose na Infância", peso: 1 },
                    { num: 46, nome: "Fibrose Cística", peso: 2 },
                    { num: 47, nome: "Artrite Idiopática Juvenil (AIJ)", peso: 1 },
                    { num: 48, nome: "Diarreia", peso: 4 }
                ]
            },
            "Preventiva": {
                "Medicina Preventiva": [
                    { num: 1, nome: "Sistemas de Informação em Saúde", peso: 3 },
                    { num: 2, nome: "Ética Médica", peso: 5 },
                    { num: 3, nome: "Princípios e Diretrizes do SUS", peso: 4 },
                    { num: 4, nome: "Bases de Saúde do Trabalhador e Normas Regulamentadoras", peso: 5 },
                    { num: 5, nome: "Medidas de Saúde Coletiva - Parte I Indicadores de Morbidade", peso: 2 },
                    { num: 6, nome: "Medidas de Saúde Coletiva - Parte II Indicadores de Mortalidade", peso: 2 },
                    { num: 7, nome: "Medidas de Saúde Coletiva - Parte III Indicadores Demográficos...", peso: 2 },
                    { num: 8, nome: "Processos Epidêmicos e Epidemiologia das Doenças Infecciosas", peso: 4 },
                    { num: 9, nome: "Processo Saúde-Doença", peso: 3 },
                    { num: 10, nome: "Vigilância em Saúde", peso: 4 },
                    { num: 11, nome: "Estatística Médica", peso: 5 },
                    { num: 12, nome: "Pesquisa Epidemiológica e Medidas de Associação", peso: 4 },
                    { num: 13, nome: "Testes Diagnósticos", peso: 4 },
                    { num: 14, nome: "História do SUS", peso: 2 },
                    { num: 15, nome: "Processos de Descentralização e Regionalização do SUS", peso: 1 },
                    { num: 16, nome: "Atenção Primária à Saúde", peso: 5 },
                    { num: 17, nome: "Políticas de Saúde", peso: 3 },
                    { num: 18, nome: "Medicina de Família e Comunidade", peso: 5 },
                    { num: 19, nome: "Saúde do Idoso", peso: 3 }
                ]
            }
        };

        // Cada tema tem: id estável, num, nome, peso e fonte (referência bibliográfica, preencha em `fonte`).
        // `n` (rótulo "N. Nome") e `p` são derivados e mantidos por compatibilidade com dados já salvos.
        (function normalizarTaxonomia() {
            const slug = s => s.normalize('NFD').replace(/[\u0300-\u036f]/g, '').toLowerCase().replace(/[^a-z0-9]+/g, '-').replace(/^-|-$/g, '');
            Object.keys(taxonomia).forEach(m => Object.keys(taxonomia[m]).forEach(e => taxonomia[m][e].forEach(t => {
                t.id = slug(m) + '.' + slug(e) + '.' + String(t.num).padStart(2, '0');
                t.fonte = t.fonte || '';
                t.n = t.num + '. ' + t.nome;
                t.p = t.peso;
            })));
        })();

        // ------------------------------------------------------------------
        // GLOBALS & HYDRATION
        // ------------------------------------------------------------------
        let listaEstudos = [];
        let configAlgoritmo = { intervalos: [1, 7, 14, 30, 60, 90], p5: 0.5, p4: 0.7, p3: 0.9, p2: 1.1, p1: 1.4, burnout: 15 };
        let agendaProvas = [];
        let cronogramaInternato = [];
        let gradesSemanaisStore = {};
        let cronogramaSemanasStore = {};
        let backlogAdiados = [];
        let pomodoroDiario = {};
        let notasIncidencia = {};
        let listaSimulados = [];
        let pomodoroTemas = { foco: 25, pausa: 5 };
        let metaAnual = 10000;
        let streak = 0;
        
        let isSobrevivencia = false;
        let sprintAtivo = false;
        let semanaAtivaId = "";
        let filtroBacklog = 'todas';

        const diasSemanaArr = ['Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sáb', 'Dom'];
        const turnosArr = ['m', 't', 'n'];

        // MIGRAÇÃO SILENCIOSA DOS PESOS
        function migrarPesosAntigos() {
            let mapaLimpo = {};
            Object.keys(taxonomia).forEach(m => {
                Object.keys(taxonomia[m]).forEach(e => {
                    taxonomia[m][e].forEach(sub => {
                        let nomeLimpo = sub.n.split(' — Peso:')[0].trim();
                        mapaLimpo[nomeLimpo] = { n: sub.n, p: sub.p };
                    });
                });
            });

            let mudou = false;

            listaEstudos.forEach(i => {
                let limpo = i.tema.split(' — Peso:')[0].trim();
                if(mapaLimpo[limpo] && (i.tema !== mapaLimpo[limpo].n || i.peso !== mapaLimpo[limpo].p)) {
                    i.tema = mapaLimpo[limpo].n;
                    i.peso = mapaLimpo[limpo].p;
                    mudou = true;
                }
            });

            backlogAdiados.forEach(b => {
                let limpo = b.tema.split(' — Peso:')[0].trim();
                if(mapaLimpo[limpo] && (b.tema !== mapaLimpo[limpo].n || b.peso !== mapaLimpo[limpo].p)) {
                    b.tema = mapaLimpo[limpo].n;
                    b.peso = mapaLimpo[limpo].p;
                    mudou = true;
                }
            });

            Object.keys(cronogramaSemanasStore).forEach(sId => {
                let cg = cronogramaSemanasStore[sId];
                diasSemanaArr.forEach(d => {
                    turnosArr.forEach(t => {
                        if(cg[d] && cg[d][t] && cg[d][t].tipo === 'teoria') {
                            let limpo = cg[d][t].tema.split(' — Peso:')[0].trim();
                            if(mapaLimpo[limpo] && (cg[d][t].tema !== mapaLimpo[limpo].n || cg[d][t].peso !== mapaLimpo[limpo].p)) {
                                cg[d][t].tema = mapaLimpo[limpo].n;
                                cg[d][t].peso = mapaLimpo[limpo].p;
                                mudou = true;
                            }
                        }
                    });
                });
            });

            Object.keys(notasIncidencia).forEach(k => { let limpo = k.split(' — Peso:')[0].trim(); if (limpo !== k && mapaLimpo[limpo]) { if (!notasIncidencia[limpo]) notasIncidencia[limpo] = notasIncidencia[k]; delete notasIncidencia[k]; mudou = true; } });
            if(mudou) {
                console.log("Migração de pesos concluída.");
                window.syncToCloud();
            }
        }

        function calcularStreak() {
            let datasAtivas = new Set();
            listaEstudos.forEach(i => i.historico.forEach(h => {
                if(h.data) datasAtivas.add(h.data);
            }));
            Object.keys(pomodoroDiario).forEach(k => {
                if(pomodoroDiario[k] > 0) datasAtivas.add(k);
            });

            let dStr = (d) => {
                let y = d.getFullYear();
                let m = String(d.getMonth()+1).padStart(2,'0');
                let dia = String(d.getDate()).padStart(2,'0');
                return `${y}-${m}-${dia}`;
            };

            let streakAtual = 0;
            let dataCheck = new Date(); 

            let hojeStr = dStr(dataCheck);
            let checkOntem = new Date(); checkOntem.setDate(checkOntem.getDate() - 1);
            let ontemStr = dStr(checkOntem);

            if (datasAtivas.has(hojeStr)) {
            } else if (datasAtivas.has(ontemStr)) {
                dataCheck.setDate(dataCheck.getDate() - 1);
            } else {
                streak = 0;
                return;
            }

            while(datasAtivas.has(dStr(dataCheck))) {
                streakAtual++;
                dataCheck.setDate(dataCheck.getDate() - 1); 
            }
            streak = streakAtual;
        }

        window.loadLocalState = function() {
            listaEstudos = JSON.parse(localStorage.getItem('estudos_med')) || [];
            configAlgoritmo = JSON.parse(localStorage.getItem('config_alg')) || { intervalos: [1, 7, 14, 30, 60, 90], p5: 0.5, p4: 0.7, p3: 0.9, p2: 1.1, p1: 1.4, burnout: 15 };
            agendaProvas = JSON.parse(localStorage.getItem('agenda_provas')) || [];
            cronogramaInternato = JSON.parse(localStorage.getItem('cronograma_internato')) || [];
            gradesSemanaisStore = JSON.parse(localStorage.getItem('grades_semanais_internato')) || {};
            cronogramaSemanasStore = JSON.parse(localStorage.getItem('crono_semanas_store')) || {};
            backlogAdiados = JSON.parse(localStorage.getItem('backlog_adiados')) || [];
            pomodoroDiario = JSON.parse(localStorage.getItem('pomodoro_diario')) || {};
            notasIncidencia = JSON.parse(localStorage.getItem('incidencia_notas')) || {};
            listaSimulados = JSON.parse(localStorage.getItem('simulados_med')) || [];
            metaAnual = parseInt(localStorage.getItem('meta_questoes')) || 10000;
            
            migrarPesosAntigos();
            initApp();
        };

        window.hydrateData = function(d) {
            listaEstudos = d.estudos || JSON.parse(localStorage.getItem('estudos_med')) || [];
            configAlgoritmo = d.config || JSON.parse(localStorage.getItem('config_alg')) || { intervalos: [1, 7, 14, 30, 60, 90], p5: 0.5, p4: 0.7, p3: 0.9, p2: 1.1, p1: 1.4, burnout: 15 };
            agendaProvas = d.provas || JSON.parse(localStorage.getItem('agenda_provas')) || [];
            cronogramaInternato = d.internato || JSON.parse(localStorage.getItem('cronograma_internato')) || [];
            gradesSemanaisStore = d.grades_semanais || JSON.parse(localStorage.getItem('grades_semanais_internato')) || {};
            cronogramaSemanasStore = d.crono_semanas || JSON.parse(localStorage.getItem('crono_semanas_store')) || {};
            backlogAdiados = d.backlog || JSON.parse(localStorage.getItem('backlog_adiados')) || [];
            pomodoroDiario = d.pomodoro || JSON.parse(localStorage.getItem('pomodoro_diario')) || {};
            notasIncidencia = d.notas || JSON.parse(localStorage.getItem('incidencia_notas')) || {};
            listaSimulados = d.simulados || JSON.parse(localStorage.getItem('simulados_med')) || [];
            metaAnual = d.metaAnual || parseInt(localStorage.getItem('meta_questoes')) || 10000;
            
            migrarPesosAntigos();
            initApp();
        };

        function initApp() {
            document.getElementById('reg_data').value = hojeLocal();
            popularSelectsMacros();
            atualizarSeletorSemanasPorRodizios();
            renderizarListasPlanejamento();
            renderizarBacklogAdiados();
            
            document.getElementById("configIntervalos").value = configAlgoritmo.intervalos.join(', ');
            [1,2,3,4,5].forEach(n => document.getElementById(`configPeso${n}`).value = configAlgoritmo[`p${n}`]);
            document.getElementById("configBurnout").value = configAlgoritmo.burnout || 15;
            document.getElementById("metaInputConfig").value = metaAnual;
            
            renderizarTabela(); 
            atualizarRadar(); 
            atualizarGraficoSimulados(); 
            renderizarMetasSubtemas(); 
            renderizarPlanilha(); 
            atualizarEstatisticasNerds();
            renderizarMapaRodizio();
            if(document.getElementById('tab-rastreio').classList.contains('active')) renderizarRastreio();
        }

        window.syncToCloud = function() {
            localStorage.setItem('estudos_med', JSON.stringify(listaEstudos));
            localStorage.setItem('config_alg', JSON.stringify(configAlgoritmo));
            localStorage.setItem('agenda_provas', JSON.stringify(agendaProvas));
            localStorage.setItem('cronograma_internato', JSON.stringify(cronogramaInternato));
            localStorage.setItem('grades_semanais_internato', JSON.stringify(gradesSemanaisStore));
            localStorage.setItem('crono_semanas_store', JSON.stringify(cronogramaSemanasStore));
            localStorage.setItem('backlog_adiados', JSON.stringify(backlogAdiados));
            localStorage.setItem('pomodoro_diario', JSON.stringify(pomodoroDiario));
            localStorage.setItem('incidencia_notas', JSON.stringify(notasIncidencia));
            localStorage.setItem('simulados_med', JSON.stringify(listaSimulados));
            localStorage.setItem('meta_questoes', metaAnual);

            const dataToSave = {
                estudos: listaEstudos, config: configAlgoritmo, provas: agendaProvas,
                internato: cronogramaInternato, grades_semanais: gradesSemanaisStore,
                crono_semanas: cronogramaSemanasStore, backlog: backlogAdiados,
                pomodoro: pomodoroDiario, notas: notasIncidencia, simulados: listaSimulados,
                metaAnual: metaAnual
            };
            if(window.firebaseSaveData && window.currentUserUid) {
                window.firebaseSaveData(window.currentUserUid, dataToSave);
            }
        };

        // ------------------------------------------------------------------
        // CASCATAS DINÂMICAS E RASTREIO
        // ------------------------------------------------------------------
        function popularSelectsMacros() {
            let optionsReg = '<option value="">1. Selecione a Macro-Área...</option>';
            let optionsRod = '<option value="">Selecione a Macro-Área...</option>';
            let optionsFiltro = '<option value="todas">🌐 Todas as Macro-Áreas</option>';
            
            Object.keys(taxonomia).forEach(m => {
                let opt = `<option value="${m}">${m}</option>`;
                optionsReg += opt; optionsRod += opt; optionsFiltro += opt;
            });
            
            optionsRod += '<option value="Psiquiatria">🧠 Psiquiatria (Especial)</option>';
            optionsRod += '<option value="MFC">🩺 MFC / APS (Especial)</option>';
            
            document.getElementById("reg_macro").innerHTML = optionsReg;
            document.getElementById("rodMacro").innerHTML = optionsRod;
            
            let evo = document.getElementById("evo_macro"); if(evo) evo.innerHTML = optionsFiltro;
            let meta = document.getElementById("meta_macro"); if(meta) meta.innerHTML = optionsFiltro;
            let plan = document.getElementById("plan_macro"); if(plan) plan.innerHTML = optionsFiltro;
            let rast = document.getElementById("rastreio_macro"); if(rast) rast.innerHTML = '<option value="">📌 Selecione...</option>' + optionsFiltro;
        }

        function popularEspecialidades(idMacro, idEsp, idSub, isFiltro) {
            let m = document.getElementById(idMacro).value;
            let sE = document.getElementById(idEsp);
            if(!sE) return;
            sE.innerHTML = isFiltro ? '<option value="todas">📌 Todas as Especialidades</option>' : '<option value="">2. Selecione a Especialidade...</option>';
            if(idSub) document.getElementById(idSub).innerHTML = isFiltro ? '<option value="todas">📌 Todos os Subtemas</option>' : '<option value="">3. Aguardando...</option>';
            
            if(!m || m === 'todas') { 
                sE.disabled = true; if(idSub) document.getElementById(idSub).disabled = true; 
            } else {
                sE.disabled = false;
                Object.keys(taxonomia[m]).forEach(e => sE.innerHTML += `<option value="${e}">${e}</option>`);
            }
            if(isFiltro) {
                if(idMacro === 'evo_macro') atualizarGraficoEvolucao();
                if(idMacro === 'meta_macro') renderizarMetasSubtemas();
                if(idMacro === 'plan_macro') renderizarPlanilha();
            }
        }

        function popularSubtemas(idMacro, idEsp, idSub, isFiltro) {
            let m = document.getElementById(idMacro).value;
            let e = document.getElementById(idEsp).value;
            let sS = document.getElementById(idSub);
            if(!sS) return;
            sS.innerHTML = isFiltro ? '<option value="todas">📌 Todos os Subtemas</option>' : '<option value="">3. Selecione o Subtema...</option>';
            
            if(!e || e === 'todas') { sS.disabled = true; } 
            else {
                sS.disabled = false;
                taxonomia[m][e].forEach(sub => {
                    sS.innerHTML += `<option value="${sub.n}" data-peso="${sub.p}">${sub.n} (peso ${sub.p})</option>`;
                });
            }
            if(isFiltro && idMacro === 'evo_macro') atualizarGraficoEvolucao();
        }

        // ------------------------------------------------------------------
        // POMODORO & FOCO
        // ------------------------------------------------------------------
        let timeLeft = pomodoroTemas.foco * 60;
        let timerInterval = null, isRunning = false;
        atualizarDisplayTimer();

        function iniciarPomodoro() {
            if (isRunning) return; isRunning = true;
            timerInterval = setInterval(() => {
                if (timeLeft > 0) { 
                    timeLeft--; atualizarDisplayTimer(); 
                    if(timeLeft % 60 === 0) {
                        let hj = hojeLocal();
                        pomodoroDiario[hj] = (pomodoroDiario[hj] || 0) + 1;
                        window.syncToCloud();
                        atualizarEstatisticasNerds();
                        atualizarDashboard(); 
                    }
                } else { pausarPomodoro(); alert("Pomodoro Finalizado!"); timeLeft = pomodoroTemas.pausa * 60; atualizarDisplayTimer(); }
            }, 1000);
        }
        function pausarPomodoro() { isRunning = false; clearInterval(timerInterval); }
        function resetarPomodoro() { pausarPomodoro(); timeLeft = pomodoroTemas.foco * 60; atualizarDisplayTimer(); }
        function atualizarDisplayTimer() {
            let m = Math.floor(timeLeft/60), s = timeLeft%60; let str = `${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
            document.getElementById("timerDisplay").innerText = str; document.getElementById("focusTimerDisplay").innerText = str;
        }
        function toggleDarkMode() { document.body.classList.toggle('dark-mode'); setTimeout(() => { try { atualizarRadar(); atualizarGraficoSimulados(); atualizarGraficoEvolucao(); } catch (e) {} }, 0); }
        function toggleFocusMode() { document.body.classList.toggle('focus-mode'); atualizarEstatisticasNerds(); }

        function mudarAba(tabId, btn) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.sidebar-btn').forEach(b => b.classList.remove('active'));
            
            document.getElementById(tabId).classList.add('active'); 
            if(btn) {
                btn.classList.add('active');
                document.getElementById('headerAbaTitulo').innerText = btn.innerText;
            }
            
            if(window.innerWidth <= 850) {
                document.querySelector('.sidebar').classList.remove('active');
                let overlay = document.getElementById('sidebarOverlay');
                if(overlay) overlay.classList.remove('active');
            }

            if (tabId === 'tab-analytics') { atualizarRadar(); atualizarGraficoEvolucao(); atualizarGraficoSimulados(); }
            if (tabId === 'tab-planejamento') { renderizarListasPlanejamento(); atualizarSeletorSemanasPorRodizios(); renderizarKanbanSemanaAtiva(); }
            if (tabId === 'tab-nerd') { atualizarEstatisticasNerds(); }
            if (tabId === 'tab-rastreio') { popularEspecialidadesRastreio(); }
        }

        // ------------------------------------------------------------------
        // REGISTRO DE ESTUDO & FSRS
        // ------------------------------------------------------------------
        function toggleQuestoesForm() {
            document.getElementById("boxQuestoesIniciais").style.display = document.getElementById("checkFezQuestoes").checked ? "flex" : "none";
        }

        function salvarNovoEstudo(e) {
            e.preventDefault();
            let macro = document.getElementById("reg_macro").value;
            let esp = document.getElementById("reg_esp").value;
            let selSub = document.getElementById("reg_subtema");
            let temaNome = selSub.value;
            let peso = parseInt(selSub.options[selSub.selectedIndex].getAttribute("data-peso"));
            let dataEstudo = document.getElementById("reg_data").value;
            let tempo = parseInt(document.getElementById("reg_tempo").value);
            let percepcao = document.getElementById("reg_dificuldade").value;
            let energia = document.getElementById("reg_energia").value;
            let fezQ = document.getElementById("checkFezQuestoes").checked;

            let item = { id: Date.now(), macroArea: macro, especialidade: esp, tema: temaNome, peso: peso, easeFactor: 2.5, fase: 1, historico: [] };

            if (fezQ) {
                let feitas = parseInt(document.getElementById("reg_feitas").value) || 0;
                let acertos = parseInt(document.getElementById("reg_acertos").value) || 0;
                item.proximaData = calcularNovaData(dataEstudo, peso, percepcao, 1, 2.5, feitas, acertos);
                item.historico.push({ fase: "Teoria + Questões", tempo, feitas, acertos, percepcao, energia, data: dataEstudo });
            } else {
                item.fase = 0.5;
                let d = new Date(dataEstudo + 'T00:00:00');
                d.setDate(d.getDate() + 1);
                item.proximaData = hojeLocal(d);
                item.historico.push({ fase: "Apenas Teoria", tempo, feitas: 0, acertos: 0, percepcao, energia, data: dataEstudo });
            }

            listaEstudos.push(item);
            
            salvarEAtualizar(); 
            document.getElementById("formEstudo").reset(); 
            document.getElementById('reg_data').value = hojeLocal();
            document.getElementById("checkFezQuestoes").checked = true; toggleQuestoesForm();
            
            document.getElementById("reg_esp").innerHTML = '<option value="">Selecione Macro...</option>';
            document.getElementById("reg_esp").disabled = true;
            document.getElementById("reg_subtema").innerHTML = '<option value="">Selecione Especialidade...</option>';
            document.getElementById("reg_subtema").disabled = true;

            renderizarTabela();
            alert("Estudo Registrado com Sucesso! Revisão FSRS programada.");
        }

        function getFatorPeso(peso) {
            if (peso === 5) return configAlgoritmo.p5; if (peso === 4) return configAlgoritmo.p4; if (peso === 3) return configAlgoritmo.p3;
            if (peso === 2) return configAlgoritmo.p2; if (peso === 1) return configAlgoritmo.p1; return 1.0;
        }

        function calcularNovaData(dataBaseStr, peso, percepcao, faseDestino, easeFactor, feitas, acertos) {
            let baseDias = (faseDestino <= 1) ? 1 : configAlgoritmo.intervalos[Math.min(faseDestino - 1, 5)];
            let fp = getFatorPeso(peso);
            let fd = (percepcao === 'dificil') ? 0.6 : (percepcao === 'facil') ? 1.3 : 1.0;
            let fDes = 1.0;
            if (faseDestino > 1 && feitas > 0) { let tx = (acertos / feitas) * 100; fDes = tx < 50 ? 0.5 : tx < 70 ? 0.8 : 1.3; }
            let fEase = easeFactor / 2.5;

            let dias = Math.max(1, Math.round(baseDias * fp * fDes * fd * fEase));
            
            let pp = agendaProvas.find(p => p.tipo === "principal");
            if (pp) {
                let dP = new Date(pp.data + 'T00:00:00'); let dH = new Date(); let diff = Math.ceil((dP - dH) / (1000 * 60 * 60 * 24));
                if (diff <= 45 && dH < dP) dias = Math.min(dias, 10);
            }

            let nD = new Date(dataBaseStr + 'T00:00:00');
            nD.setDate(nD.getDate() + dias);
            return hojeLocal(nD);
        }

        // ------------------------------------------------------------------
        // DASHBOARD & CRONOGRAMA DE REVISÕES
        // ------------------------------------------------------------------
        let filtroTab = 'todas';
        function mudarFiltroTabela(f, btn) {
            filtroTab = f;
            document.querySelectorAll('#tab-estudo .sub-nav-abas .sub-aba-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active'); renderizarTabela();
        }

        function toggleSobrevivencia() {
            isSobrevivencia = !isSobrevivencia; document.body.classList.toggle('modo-sobrevivencia');
            document.getElementById("btnSobrevivencia").style.background = isSobrevivencia ? "var(--sobrevivencia-bg)" : "var(--bg-color)";
            document.getElementById("btnSobrevivencia").style.color = isSobrevivencia ? "white" : "var(--text-color)";
            document.getElementById("sobrevivenciaAlert").style.display = isSobrevivencia ? "block" : "none";
            renderizarTabela();
        }

        function ativarSprint() { 
            sprintAtivo = true; 
            document.getElementById("sprintAlert").style.display = "block"; 
            let sts = document.getElementById("sprintStatusText");
            if(sts) { sts.innerText = "Ativo"; sts.style.color = "var(--primary-color)"; }
            renderizarTabela(); 
        }
        function limparSprint() { 
            sprintAtivo = false; 
            document.getElementById("sprintAlert").style.display = "none"; 
            let sts = document.getElementById("sprintStatusText");
            if(sts) { sts.innerText = "Inativo"; sts.style.color = "var(--danger-color)"; }
            renderizarTabela(); 
        }

        function renderizarTabela() {
            let tbody = document.getElementById("tabela-estudos"); if(!tbody) return;
            tbody.innerHTML = "";
            let hj = hojeLocal();

            atualizarDashboard();

            let filtrados = listaEstudos.filter(i => {
                if (i.fase > 6) return false;
                if (isSobrevivencia && i.proximaData > hj) return false;
                if (sprintAtivo && i.peso < 4) return false;
                if (filtroTab === 'atrasadas') return i.proximaData < hj;
                if (filtroTab === 'hoje') return i.proximaData === hj;
                if (filtroTab === 'futuras') return i.proximaData > hj;
                return true;
            });

            if (filtrados.length === 0) { 
                tbody.innerHTML = `<tr><td colspan="3" style="text-align:center; padding:30px;"><div class="empty-ico">✨</div><b style="color:var(--primary-color);">Tudo limpo por aqui!</b><br><span style="font-size:12px;opacity:0.7;">Nenhum estudo pendente para esta visualização.</span></td></tr>`; 
                return; 
            }

            filtrados.forEach(item => {
                let rIdx = listaEstudos.indexOf(item); let atrasado = item.proximaData < hj;
                let tr = document.createElement("tr"); if (atrasado) tr.classList.add('atrasado');
                let fText = item.fase === 0.5 ? "Anki (Teoria)" : item.fase === 1 ? "1ª Rev (Anki)" : `${Math.floor(item.fase)}ª Rev (Questões)`;
                
                let bColor = getMacroColor(item.macroArea);

                tr.innerHTML = `
                    <td><div class="badge badge-macro" style="background:${bColor}; text-shadow: 0px 1px 2px rgba(0,0,0,0.4);">${item.macroArea.substring(0,8)}</div><br><small style="opacity:0.7">${item.especialidade}</small><br><strong>${item.tema}</strong></td>
                    <td>${fText}<br><small>${item.proximaData} ${atrasado ? '🔴' : ''}</small></td>
                    <td>
                        <button class="btn-acao" onclick="abrirModal(${rIdx})">Executar</button>
                        <button class="btn-acao" style="background:var(--text-color);" onclick="abrirDetalhes(${rIdx})">Ver</button>
                        <button class="btn-acao btn-excluir" onclick="excluirEstudo(${rIdx})">X</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function atualizarDashboard() { setTimeout(renderizarPainelInicial, 0);
            let hj = hojeLocal(); let h=0, a=0, p=0, cg=0;
            let d3 = new Date(); d3.setDate(d3.getDate() + 3); let d3S = hojeLocal(d3);

            let pp = agendaProvas.find(x => x.tipo === "principal");
            let alertBar = document.getElementById("retaFinalAlert");
            if (pp) {
                let dP = new Date(pp.data + 'T00:00:00');
                let dH = new Date();
                let diff = Math.ceil((dP - dH) / (1000 * 60 * 60 * 24));
                if (diff <= 45 && diff >= 0) {
                    alertBar.style.display = "block";
                    alertBar.innerText = `🔥 RETA FINAL ATIVA: Faltam ${diff} dias para a Prova Principal! O FSRS assumiu teto máximo de 10 dias.`;
                } else {
                    alertBar.style.display = "none";
                }
            } else {
                alertBar.style.display = "none";
            }

            listaEstudos.forEach(i => {
                if (i.fase <= 6) {
                    if (i.proximaData === hj) h++;
                    if (i.proximaData < hj) a++;
                    if (i.fase === 0.5) p++;
                    if (i.proximaData >= hj && i.proximaData <= d3S) cg++;
                }
            });

            calcularStreak();

            document.getElementById("statHoje").innerText = h; document.getElementById("statAtrasadas").innerText = a;
            document.getElementById("statPendentesTeoria").innerText = p; document.getElementById("statCarga3Dias").innerText = cg;
            document.getElementById("burnoutAlert").style.display = cg >= configAlgoritmo.burnout ? "block" : "none";
            
            document.getElementById("streakCount").innerText = streak; 
            document.getElementById("focusStreak").innerText = streak;
            document.getElementById("metaStatus").innerText = `${h}/${Math.max(1, h)}`; document.getElementById("focusMetaStatus").innerText = `${h}/${Math.max(1, h)}`;
            
            atualizarMetaAnualPainel();
        }

        function atualizarMetaAnualPainel() {
            let totalFeitas = 0;
            listaEstudos.forEach(i => i.historico.forEach(h => { if(h.feitas) totalFeitas += h.feitas; }));
            
            let meta = parseInt(document.getElementById("metaInputConfig").value) || metaAnual;
            let progresso = Math.min(100, (totalFeitas / meta) * 100).toFixed(1);
            
            document.getElementById("metaAnualProgressoText").innerText = `${totalFeitas} / ${meta} questões resolvidas`;
            document.getElementById("metaAnualPercent").innerText = `${progresso}%`;
            document.getElementById("metaAnualBar").style.width = `${progresso}%`;
            
            let hoje = new Date();
            let inicioAno = new Date(hoje.getFullYear(), 0, 1);
            let diasCorridos = Math.max(1, Math.ceil((hoje - inicioAno) / (1000 * 60 * 60 * 24)));
            let ritmoAtual = (totalFeitas / diasCorridos).toFixed(1);
            document.getElementById("metaRitmoAtual").innerText = `${ritmoAtual} qts/dia`;
            
            let pp = agendaProvas.find(x => x.tipo === "principal");
            let dAlvo = pp ? new Date(pp.data + 'T00:00:00') : new Date(hoje.getFullYear(), 11, 31);
            let diffDias = Math.ceil((dAlvo - hoje) / (1000 * 60 * 60 * 24));
            
            let statusEl = document.getElementById("metaStatusRitmo");
            if (diffDias > 0) {
                let restantes = Math.max(0, meta - totalFeitas);
                let ritmoNecessario = (restantes / diffDias).toFixed(1);
                document.getElementById("metaRitmoNecessario").innerText = `${ritmoNecessario} qts/dia`;
                document.getElementById("metaDiasRestantes").innerText = `Até ${pp ? pp.nome : 'o fim do ano'} (${diffDias} dias)`;
                
                if (restantes === 0) {
                    statusEl.innerText = "🎉 Meta Batida!";
                    statusEl.style.color = "var(--primary-color)";
                } else if (parseFloat(ritmoAtual) >= parseFloat(ritmoNecessario)) {
                    statusEl.innerText = "🟢 Adiantado / No Ritmo";
                    statusEl.style.color = "var(--primary-color)";
                } else {
                    statusEl.innerText = "🔴 Abaixo do Ritmo";
                    statusEl.style.color = "var(--danger-color)";
                }
            } else {
                document.getElementById("metaRitmoNecessario").innerText = "-";
                document.getElementById("metaDiasRestantes").innerText = "Data alvo expirada";
                statusEl.innerText = "-";
                statusEl.style.color = "inherit";
            }
        }

        // ------------------------------------------------------------------
        // MODAIS DE EXECUÇÃO & ADIAMENTO
        // ------------------------------------------------------------------
        let idxModal = null;
        function abrirModal(idx) {
            idxModal = idx; let item = listaEstudos[idx]; let m = document.getElementById("modalBodyContent");
            if (item.fase <= 1) {
                m.innerHTML = `<h3>${item.fase === 0.5 ? "Anki Pós-Teoria" : "1ª Rev (Anki)"}: ${item.tema}</h3>
                <label><input type="checkbox" id="modAnki" required> Fiz os flashcards hoje.</label>
                <label>Percepção:</label><select id="modPerc" required><option value="medio">Médio</option><option value="facil">Fácil</option><option value="dificil">Difícil</option></select>`;
            } else {
                m.innerHTML = `<h3>${Math.floor(item.fase)}ª Rev: ${item.tema}</h3>
                <label>Tempo (min):</label><input type="number" id="modTempo" value="15" required>
                <label>Feitas:</label><input type="number" id="modFeitas" required>
                <label>Acertos:</label><input type="number" id="modAcertos" required>
                <label>Dificuldade:</label><select id="modPerc" required><option value="medio">Médio</option><option value="facil">Fácil</option><option value="dificil">Difícil</option></select>`;
            }
            document.getElementById("modalRevisao").style.display = "flex";
        }

        function salvarRevisaoModal(e) { const _seq = window._seqRevisao;
            e.preventDefault(); let item = listaEstudos[idxModal]; let hoje = hojeLocal();
            if (!item.easeFactor) item.easeFactor = 2.5;

            if (item.fase <= 1) {
                if (!document.getElementById("modAnki").checked) return alert("Confirme o Anki!");
                let p = document.getElementById("modPerc").value;
                if (item.fase === 0.5) {
                    let d = (p === 'dificil') ? 3 : (p === 'facil') ? 7 : 5;
                    let nD = new Date(hoje + 'T00:00:00'); nD.setDate(nD.getDate() + d); item.proximaData = hojeLocal(nD);
                } else {
                    let r = item.historico[0] || { feitas: 20, acertos: 16 };
                    item.proximaData = calcularNovaData(hoje, item.peso, p, 2, item.easeFactor, r.feitas, r.acertos);
                }
                item.historico.push({ fase: "Anki", percepcao: p, data: hoje }); item.fase = 2;
            } else {
                let tmp = parseInt(document.getElementById("modTempo").value); let f = parseInt(document.getElementById("modFeitas").value);
                let a = parseInt(document.getElementById("modAcertos").value); let p = document.getElementById("modPerc").value;
                let tx = (a / f) * 100;
                if (tx < 50) item.easeFactor = Math.max(1.3, item.easeFactor - 0.2); else if (tx >= 85) item.easeFactor += 0.15;
                item.proximaData = calcularNovaData(hoje, item.peso, p, item.fase + 1, item.easeFactor, f, a);
                item.historico.push({ fase: `${Math.floor(item.fase)}ª Rev`, tempo: tmp, feitas: f, acertos: a, percepcao: p, data: hoje });
                item.fase += 1;
            }
            salvarEAtualizar(); fecharModal('modalRevisao'); window._seqRevisao = _seq; if (_seq) setTimeout(() => { if (!comecarRevisao()) window._seqRevisao = false; }, 250);
        }

        function abrirDetalhes(idx) {
            let item = listaEstudos[idx]; if (!item.easeFactor) item.easeFactor = 2.5;
            let html = `<h3>${item.tema}</h3><p style="font-size:12px;opacity:0.8;">Fator FSRS: <b>${item.easeFactor.toFixed(2)}</b> | Peso: <b>${item.peso}</b></p><ul style="padding-left:15px;">`;
            item.historico.forEach(h => html += `<li><b>${h.fase}</b> (${h.data}) ${h.feitas ? `- Acertos: ${h.acertos}/${h.feitas}` : ''}</li>`);
            html += `</ul><h4 class="mt-20">🔮 Previsão (Fim de Ciclo):</h4><ul style="padding-left:15px; opacity:0.8;">`;
            let tF = Math.max(2, Math.floor(item.fase)); let tD = item.proximaData || hojeLocal();
            let last = item.historico.filter(h => h.feitas > 0).pop() || { feitas: 20, acertos: 16, percepcao: 'medio' };
            for (let i = tF; i <= 6; i++) { html += `<li><b>${i}ª Rev.:</b> ${tD}</li>`; tD = calcularNovaData(tD, item.peso, last.percepcao, i + 1, item.easeFactor, last.feitas, last.acertos); }
            html += `</ul><p style="color:var(--primary-color);font-weight:bold;margin-top:15px; font-size:15px;">Estimativa de Alta: ${tD}</p>`;
            document.getElementById("detalhesBodyContent").innerHTML = html; document.getElementById("modalDetalhes").style.display = "flex";
        }

        // LÓGICA DO MODAL DE ADIAMENTO PERSONALIZADO
        let adiarContext = null;

        function adiarTemaParaBacklog(d, t, dtExata) {
            let cg = cronogramaSemanasStore[semanaAtivaId];
            if(!cg || !cg[d] || !cg[d][t] || cg[d][t].tipo !== 'teoria') return;

            let topico = cg[d][t];
            adiarContext = { d, t, dtExata, topico };

            document.getElementById("adiarTemaTitulo").innerText = `${topico.tema} — (${d} às ${t === 'm' ? 'Manhã' : t === 't' ? 'Tarde' : 'Noite'})`;
            document.getElementById("adiarMotivoInput").value = "Plantão / Imprevisto no internato";
            document.getElementById("adiarTurnoDestino").value = "ocupado";
            document.getElementById("adiarTemaDestino").value = "backlog";
            document.getElementById("boxMotivoBacklog").style.display = "block";
            document.getElementById("boxRealocarTurno").style.display = "none";

            let selSlot = document.getElementById("selSlotRealocar");
            selSlot.innerHTML = "";
            let slotsDisponiveis = [];

            diasSemanaArr.forEach(diaKey => {
                turnosArr.forEach(trnKey => {
                    if(diaKey === d && trnKey === t) return;
                    if(cg[diaKey] && cg[diaKey][trnKey] && (cg[diaKey][trnKey].tipo === 'revisao' || cg[diaKey][trnKey].tipo === 'descanso' || cg[diaKey][trnKey].tipo === 'estudo')) {
                        let labelT = trnKey === 'm' ? 'Manhã' : trnKey === 't' ? 'Tarde' : 'Noite';
                        slotsDisponiveis.push({ d: diaKey, t: trnKey, desc: `${diaKey} (${labelT}) - [${cg[diaKey][trnKey].tipo}]` });
                    }
                });
            });

            if(slotsDisponiveis.length === 0) {
                selSlot.innerHTML = `<option value="">Nenhum turno livre nesta semana</option>`;
            } else {
                slotsDisponiveis.forEach(s => {
                    selSlot.innerHTML += `<option value="${s.d}|${s.t}">${s.desc}</option>`;
                });
            }

            document.getElementById("modalAdiarTema").style.display = "flex";
        }

        function toggleOpcaoRealocar(val) {
            document.getElementById("boxMotivoBacklog").style.display = (val === 'backlog') ? "block" : "none";
            document.getElementById("boxRealocarTurno").style.display = (val === 'realocar') ? "block" : "none";
        }

        function confirmarAdiarTema() {
            if(!adiarContext) return;
            let cg = cronogramaSemanasStore[semanaAtivaId];
            let { d, t, dtExata, topico } = adiarContext;

            let novoTipoTurno = document.getElementById("adiarTurnoDestino").value;
            let acaoTema = document.getElementById("adiarTemaDestino").value;

            cg[d][t] = { tipo: novoTipoTurno };

            if(acaoTema === 'backlog') {
                let motivo = document.getElementById("adiarMotivoInput").value.trim() || "Adiado pelo internato";
                backlogAdiados.push({
                    macro: topico.macro,
                    esp: topico.esp,
                    tema: topico.tema,
                    peso: topico.peso,
                    dataAdiado: dtExata,
                    dataRealAdiado: new Date().toISOString(),
                    motivo: motivo
                });
                alert(`Tema enviado para o Backlog! Horário liberado marcado como '${novoTipoTurno}'.`);
            } else if(acaoTema === 'realocar') {
                let destinoVal = document.getElementById("selSlotRealocar").value;
                if(!destinoVal) {
                    alert("Nenhum turno selecionado para realocação! O tema será mantido no Backlog.");
                    backlogAdiados.push({
                        macro: topico.macro, esp: topico.esp, tema: topico.tema, peso: topico.peso,
                        dataAdiado: dtExata, dataRealAdiado: new Date().toISOString(), motivo: "Sem turnos livres para realocar"
                    });
                } else {
                    let [destD, destT] = destinoVal.split('|');
                    cg[destD][destT] = { tipo: 'teoria', macro: topico.macro, esp: topico.esp, tema: topico.tema, peso: topico.peso };
                    let labelT = destT === 'm' ? 'Manhã' : destT === 't' ? 'Tarde' : 'Noite';
                    alert(`Tema realocado para ${destD} (${labelT})!`);
                }
            }

            cronogramaSemanasStore[semanaAtivaId] = cg;
            window.syncToCloud();

            fecharModal('modalAdiarTema');
            renderizarKanbanSemanaAtiva();
            renderizarBacklogAdiados();
            adiarContext = null;
        }

        function excluirEstudo(idx) { if (confirm("Excluir este estudo?")) { listaEstudos.splice(idx, 1); salvarEAtualizar(); } }
        function fecharModal(id) { if (id === "modalRevisao") window._seqRevisao = false; document.getElementById(id).style.display = "none"; }

        // ------------------------------------------------------------------
        // KANBAN BASEADO EM RODÍZIOS COM DATAS EXATAS E ISOLAMENTO
        // ------------------------------------------------------------------
        function atualizarSeletorSemanasPorRodizios() {
            let sel = document.getElementById("seletorSemanaInternato");
            sel.innerHTML = "";
            
            if(cronogramaInternato.length === 0) {
                sel.innerHTML = `<option value="">Cadastre um rodízio primeiro...</option>`;
                document.getElementById("gridSemanaSetup").innerHTML = `<div class="empty-state"><div class="empty-ico">🗺️</div><b>Nenhum rodízio cadastrado ou ativo.</b></div>`;
                document.getElementById("resultadoInternato").style.display = "none";
                return;
            }

            let primeiraValida = "";
            cronogramaInternato.forEach((rod, rIdx) => {
                let dIni = new Date(rod.inicio + 'T00:00:00');
                let dFim = new Date(rod.fim + 'T00:00:00');
                let diffDias = Math.ceil((dFim - dIni) / (1000 * 60 * 60 * 24)) + 1;
                let numSemanasRod = Math.max(1, Math.ceil(diffDias / 7));

                for(let s=1; s<=numSemanasRod; s++) {
                    let sId = `rod_${rIdx}_sem_${s}`;
                    if(!primeiraValida) primeiraValida = sId;
                    let dtIniSem = new Date(dIni);
                    dtIniSem.setDate(dIni.getDate() + (s-1)*7);
                    let dtFimSem = new Date(dtIniSem);
                    dtFimSem.setDate(dtIniSem.getDate() + 6);
                    let fD = d => `${String(d.getDate()).padStart(2,'0')}/${String(d.getMonth()+1).padStart(2,'0')}`;

                    let labelText = `${rod.macro} — Sem. ${s} (${fD(dtIniSem)} a ${fD(dtFimSem)})`;
                    sel.innerHTML += `<option value="${sId}">${labelText}</option>`;
                }
            });

            if(!semanaAtivaId || !document.querySelector(`#seletorSemanaInternato option[value="${semanaAtivaId}"]`)) {
                semanaAtivaId = primeiraValida;
            }
            sel.value = semanaAtivaId;
            mudarSemanaAtiva(semanaAtivaId);
        }

        function mudarSemanaAtiva(idSemana) {
            if(!idSemana) return;
            semanaAtivaId = idSemana;
            gerarGridSemanaAtiva();
            renderizarKanbanSemanaAtiva();
        }

        function calcularDataExataRodizio(semanaId, diaIndex) {
            let partes = semanaId.split('_');
            let rIdx = parseInt(partes[1]);
            let numSemana = parseInt(partes[3]);
            let rod = cronogramaInternato[rIdx];
            if(!rod) return "00/00";

            let dIni = new Date(rod.inicio + 'T00:00:00');
            let dAlvo = new Date(dIni);
            dAlvo.setDate(dIni.getDate() + ((numSemana - 1) * 7) + diaIndex);

            let dd = String(dAlvo.getDate()).padStart(2, '0');
            let mm = String(dAlvo.getMonth() + 1).padStart(2, '0');
            return `${dd}/${mm}`;
        }

        function gerarGridSemanaAtiva() {
            let g = document.getElementById("gridSemanaSetup"); g.innerHTML = "";
            if(!semanaAtivaId) return;

            let gradeAtual = gradesSemanaisStore[semanaAtivaId] || { 
                Seg:{m:'ocupado',t:'ocupado',n:'ocupado'}, Ter:{m:'ocupado',t:'ocupado',n:'ocupado'}, 
                Qua:{m:'ocupado',t:'ocupado',n:'ocupado'}, Qui:{m:'ocupado',t:'ocupado',n:'ocupado'}, 
                Sex:{m:'ocupado',t:'ocupado',n:'ocupado'}, Sáb:{m:'ocupado',t:'ocupado',n:'ocupado'}, 
                Dom:{m:'descanso',t:'descanso',n:'descanso'} 
            };

            diasSemanaArr.forEach((d, idx) => {
                let trns = gradeAtual[d] || { m: 'ocupado', t: 'ocupado', n: 'ocupado' };
                let dtExata = calcularDataExataRodizio(semanaAtivaId, idx);
                let renderSel = (trn, v) => `
                    <select class="sel-turno" data-dia="${d}" data-trn="${trn}" onchange="salvarGradeTurnosSemanaSilencioso()">
                        <option value="ocupado" ${v==='ocupado'?'selected':''}>🏥 Ocupado</option>
                        <option value="estudo" ${v==='estudo'?'selected':''}>📘 Estudo</option>
                        <option value="descanso" ${v==='descanso'?'selected':''}>🌿 Descanso</option>
                    </select>
                `;
                g.innerHTML += `<div class="dia-col"><h4>${d}<br><small style="font-size:10px; font-weight:normal; opacity:0.8;">(${dtExata})</small></h4>
                <div style="font-size:10px; opacity:0.8; margin-bottom:4px;">Manhã</div>${renderSel('m', trns.m)}
                <div style="font-size:10px; opacity:0.8; margin-bottom:4px;">Tarde</div>${renderSel('t', trns.t)}
                <div style="font-size:10px; opacity:0.8; margin-bottom:4px;">Noite</div>${renderSel('n', trns.n)}</div>`;
            });
        }

        function salvarGradeTurnosSemanaSilencioso() {
            if(!gradesSemanaisStore[semanaAtivaId]) gradesSemanaisStore[semanaAtivaId] = {};
            document.querySelectorAll('.sel-turno').forEach(sel => {
                let d = sel.dataset.dia; let t = sel.dataset.trn;
                if(!gradesSemanaisStore[semanaAtivaId][d]) gradesSemanaisStore[semanaAtivaId][d] = {};
                gradesSemanaisStore[semanaAtivaId][d][t] = sel.value;
            });
            window.syncToCloud();
        }

        function salvarGradeTurnosSemana() {
            salvarGradeTurnosSemanaSilencioso();
            alert("Grade de turnos salva com sucesso!");
        }

        function aplicarLote(dias, turnos, valor) {
            if(!semanaAtivaId) return alert("Selecione uma semana primeiro.");
            document.querySelectorAll('.sel-turno').forEach(sel => {
                if(dias.includes(sel.dataset.dia) && turnos.includes(sel.dataset.trn)) {
                    sel.value = valor;
                }
            });
            salvarGradeTurnosSemanaSilencioso();
        }

        function copiarSemanaAnterior() {
            if(!semanaAtivaId) return alert("Selecione uma semana primeiro.");
            let partes = semanaAtivaId.split('_'); 
            let sNum = parseInt(partes[3]);
            if(sNum <= 1) return alert("Esta já é a primeira semana do rodízio!");
            
            let antId = `rod_${partes[1]}_sem_${sNum-1}`;
            let antGrade = gradesSemanaisStore[antId];
            
            if(!antGrade) return alert("Semana anterior sem grade salva!");
            
            diasSemanaArr.forEach(d => {
                turnosArr.forEach(t => {
                    let sel = document.querySelector(`.sel-turno[data-dia="${d}"][data-trn="${t}"]`);
                    if(sel && antGrade[d] && antGrade[d][t]) sel.value = antGrade[d][t];
                });
            });
            salvarGradeTurnosSemanaSilencioso();
            alert("Grade da semana anterior copiada com sucesso!");
        }

        function limparTodoRodizio() {
            if(!semanaAtivaId) return alert("Selecione um rodízio primeiro.");
            let rIdx = parseInt(semanaAtivaId.split('_')[1]);
            let rod = cronogramaInternato[rIdx];
            if(!rod) return;
            
            if(!confirm(`⚠️ ATENÇÃO: AÇÃO IRREVERSÍVEL!\n\nVocê está prestes a APAGAR todo o planejamento (turnos e alocações de temas) de TODAS as semanas do rodízio atual (${rod.macro}).\n\nDeseja realmente continuar?`)) {
                return;
            }

            let dIni = new Date(rod.inicio + 'T00:00:00');
            let dFim = new Date(rod.fim + 'T00:00:00');
            let diffDias = Math.ceil((dFim - dIni) / (1000 * 60 * 60 * 24)) + 1;
            let numSemanasRod = Math.max(1, Math.ceil(diffDias / 7));

            for(let s=1; s<=numSemanasRod; s++) {
                let sId = `rod_${rIdx}_sem_${s}`;
                
                let gradeVazia = { 
                    Seg:{m:'ocupado',t:'ocupado',n:'ocupado'}, Ter:{m:'ocupado',t:'ocupado',n:'ocupado'}, 
                    Qua:{m:'ocupado',t:'ocupado',n:'ocupado'}, Qui:{m:'ocupado',t:'ocupado',n:'ocupado'}, 
                    Sex:{m:'ocupado',t:'ocupado',n:'ocupado'}, Sáb:{m:'ocupado',t:'ocupado',n:'ocupado'}, 
                    Dom:{m:'ocupado',t:'ocupado',n:'ocupado'} 
                };
                gradesSemanaisStore[sId] = gradeVazia;
                
                if(cronogramaSemanasStore[sId]) {
                    delete cronogramaSemanasStore[sId];
                }
            }
            
            backlogAdiados = backlogAdiados.filter(item => item.macro !== rod.macro);

            window.syncToCloud();
            gerarGridSemanaAtiva();
            renderizarKanbanSemanaAtiva();
            renderizarBacklogAdiados();
            
            alert(`Rodízio de ${rod.macro} limpo com sucesso! Todos os turnos voltaram para 'Ocupado'.`);
        }

        function gerarTodoORodizio() {
            if(cronogramaInternato.length === 0) return alert("Cadastre pelo menos um rodízio!");
            salvarGradeTurnosSemanaSilencioso();
            
            let rIdx = parseInt(semanaAtivaId.split('_')[1]);
            let rod = cronogramaInternato[rIdx];
            if(!rod) return alert("Selecione um rodízio válido!");

            let dIni = new Date(rod.inicio + 'T00:00:00');
            let dFim = new Date(rod.fim + 'T00:00:00');
            let diffDias = Math.ceil((dFim - dIni) / (1000 * 60 * 60 * 24)) + 1;
            let numSemanasRod = Math.max(1, Math.ceil(diffDias / 7));
            let misto = document.getElementById("chkMisto").checked;

            let pool = [];
            backlogAdiados.forEach(item => { if(item.macro === rod.macro) pool.push(item); });
            backlogAdiados = backlogAdiados.filter(item => item.macro !== rod.macro);

            if(taxonomia[rod.macro]) {
                Object.keys(taxonomia[rod.macro]).forEach(esp => {
                    taxonomia[rod.macro][esp].forEach(sub => {
                        let jaFez = listaEstudos.some(e => e.tema === sub.n);
                        let jaNoPool = pool.some(p => p.tema === sub.n);
                        if(!jaFez && !jaNoPool) pool.push({ macro: rod.macro, esp: esp, tema: sub.n, peso: sub.p });
                    });
                });
            }
            pool.sort((a,b) => b.peso - a.peso);

            for(let s=1; s<=numSemanasRod; s++) {
                let sId = `rod_${rIdx}_sem_${s}`;
                let grade = gradesSemanaisStore[sId] || { 
                    Seg:{m:'ocupado',t:'ocupado',n:'estudo'}, Ter:{m:'ocupado',t:'ocupado',n:'estudo'}, 
                    Qua:{m:'ocupado',t:'ocupado',n:'estudo'}, Qui:{m:'ocupado',t:'ocupado',n:'estudo'}, 
                    Sex:{m:'ocupado',t:'ocupado',n:'estudo'}, Sáb:{m:'estudo',t:'estudo',n:'descanso'}, 
                    Dom:{m:'descanso',t:'descanso',n:'descanso'} 
                };
                gradesSemanaisStore[sId] = grade;

                let slotsEstudo = [];
                diasSemanaArr.forEach(d => { turnosArr.forEach(t => { if(grade[d] && grade[d][t] === 'estudo') slotsEstudo.push({d, t}); }); });

                let qTeoria = misto ? Math.round(slotsEstudo.length * 0.65) : slotsEstudo.length;
                let cronoSemanal = {};
                diasSemanaArr.forEach(d => {
                    cronoSemanal[d] = {};
                    turnosArr.forEach(t => cronoSemanal[d][t] = { tipo: grade[d] ? grade[d][t] : 'ocupado' });
                });

                let count = 0;
                slotsEstudo.forEach(slot => {
                    if(count < qTeoria && pool.length > 0) {
                        let t = pool.shift();
                        cronoSemanal[slot.d][slot.t] = { tipo: 'teoria', macro: t.macro, esp: t.esp, tema: t.tema, peso: t.peso };
                        count++;
                    } else {
                        cronoSemanal[slot.d][slot.t] = { tipo: 'revisao' };
                    }
                });

                cronogramaSemanasStore[sId] = cronoSemanal;
            }

            if(pool.length > 0) {
                pool.forEach(sobra => {
                    backlogAdiados.push({
                        macro: sobra.macro, esp: sobra.esp, tema: sobra.tema, peso: sobra.peso,
                        dataAdiado: "Fim do Rodízio",
                        dataRealAdiado: new Date().toISOString(),
                        motivo: "Sem espaço no rodízio completo"
                    });
                });
            }

            window.syncToCloud();
            renderizarKanbanSemanaAtiva();
            renderizarBacklogAdiados();
            
            if(rod.macro === "Psiquiatria" || rod.macro === "MFC") {
                alert(`Rodízio de ${rod.macro} gerado! Os turnos de estudo estão livres para você resgatar os temas manualmente na Busca Livre.`);
            } else {
                alert(`Todo o rodízio de ${rod.macro} (${numSemanasRod} semanas) foi planejado com sucesso!`);
            }
        }

        function gerarPlanoSemanaAtiva() {
            salvarGradeTurnosSemanaSilencioso();
            let misto = document.getElementById("chkMisto").checked;
            let gradeAtual = gradesSemanaisStore[semanaAtivaId];
            if(!gradeAtual) return alert("Salve a grade de turnos primeiro!");

            let rIdx = parseInt(semanaAtivaId.split('_')[1]);
            let rod = cronogramaInternato[rIdx];
            let macroRod = rod ? rod.macro : "Clínica Médica";

            let slotsEstudo = [];
            diasSemanaArr.forEach(d => { turnosArr.forEach(t => { if(gradeAtual[d] && gradeAtual[d][t] === 'estudo') slotsEstudo.push({d, t}); }); });
            
            if(slotsEstudo.length === 0) return alert("Defina pelo menos um turno como '📘 Estudo' nesta semana!");

            let qTeoria = misto ? Math.round(slotsEstudo.length * 0.65) : slotsEstudo.length;

            let poolTemas = [];
            let antigoCrono = cronogramaSemanasStore[semanaAtivaId];
            if(antigoCrono) {
                diasSemanaArr.forEach(d => {
                    turnosArr.forEach(t => {
                        if(antigoCrono[d] && antigoCrono[d][t] && antigoCrono[d][t].tipo === 'teoria') {
                            poolTemas.push(antigoCrono[d][t]);
                        }
                    });
                });
            }

            while(poolTemas.length < qTeoria && backlogAdiados.length > 0) {
                poolTemas.push(backlogAdiados.shift());
            }

            if(poolTemas.length < qTeoria && taxonomia[macroRod]) {
                Object.keys(taxonomia[macroRod]).forEach(esp => {
                    taxonomia[macroRod][esp].forEach(sub => {
                        let jaFez = listaEstudos.some(e => e.tema === sub.n);
                        let jaNoPool = poolTemas.some(p => p.tema === sub.n);
                        if(!jaFez && !jaNoPool) poolTemas.push({ macro: macroRod, esp: esp, tema: sub.n, peso: sub.p });
                    });
                });
                poolTemas.sort((a,b) => b.peso - a.peso);
            }

            let novoCrono = {};
            diasSemanaArr.forEach(d => {
                novoCrono[d] = {};
                turnosArr.forEach(t => novoCrono[d][t] = { tipo: gradeAtual[d] ? gradeAtual[d][t] : 'ocupado' });
            });

            let count = 0;
            slotsEstudo.forEach(slot => {
                if(count < qTeoria && poolTemas.length > 0) {
                    let topico = poolTemas.shift();
                    novoCrono[slot.d][slot.t] = { tipo: 'teoria', macro: topico.macro, esp: topico.esp, tema: topico.tema, peso: topico.peso };
                    count++;
                } else {
                    novoCrono[slot.d][slot.t] = { tipo: 'revisao' };
                }
            });

            if(poolTemas.length > 0) {
                poolTemas.forEach(sobra => {
                    backlogAdiados.push({
                        macro: sobra.macro, esp: sobra.esp, tema: sobra.tema, peso: sobra.peso,
                        dataAdiado: calcularDataExataRodizio(semanaAtivaId, 0),
                        dataRealAdiado: new Date().toISOString(),
                        motivo: "Sem espaço livre após recalcular a semana"
                    });
                });
            }

            cronogramaSemanasStore[semanaAtivaId] = novoCrono;
            window.syncToCloud();

            renderizarKanbanSemanaAtiva();
            renderizarBacklogAdiados();
            alert("Esta semana foi recalculada isoladamente sem afetar as outras!");
        }

        function renderizarKanbanSemanaAtiva() {
            let cg = cronogramaSemanasStore[semanaAtivaId];
            let res = document.getElementById("resultadoInternato");
            
            if(!cg) { res.style.display = "none"; return; }

            let html = `<div style="display:grid; grid-template-columns: repeat(7, 1fr); gap:10px; margin-top:10px;" class="semana-grid">`;
            
            diasSemanaArr.forEach((d, idx) => {
                let dtExata = calcularDataExataRodizio(semanaAtivaId, idx);
                html += `<div style="background:var(--card-bg); border:1px solid var(--border-color); border-radius:10px; padding:8px; box-shadow:var(--shadow-btn);" class="dia-col">
                            <h4 style="text-align:center; margin-top:0; border-bottom:1px solid var(--border-color); padding-bottom:8px; font-size:13px;">${d}<br><small style="font-size:10px; opacity:0.7; font-weight:normal;">${dtExata}</small></h4>`;
                turnosArr.forEach(t => {
                    let b = cg[d] && cg[d][t] ? cg[d][t] : {tipo: 'ocupado'};
                    let label = t==='m'?'Manhã':t==='t'?'Tarde':'Noite';
                    let bg, cont;

                    if(b.tipo === 'ocupado') {
                        bg = 'var(--bg-color)'; cont = `<div style="font-size:11px; color:#7f8c8d; text-align:center; padding:10px 0;">🏥 Ocupado</div>`;
                    } else if(b.tipo === 'descanso') {
                        bg = '#2ecc71'; cont = `<div style="font-size:11px; color:white; text-align:center; padding:10px 0;">🌿 Descanso</div>`;
                    } else if(b.tipo === 'estudo') {
                        bg = 'rgba(52, 152, 219, 0.05)'; 
                        cont = `<div style="font-size:11px; color:var(--accent-color); text-align:center; padding:10px 0; border: 1px dashed var(--accent-color); border-radius:6px;">📘 Estudo Livre</div>`;
                    } else if(b.tipo === 'teoria') {
                        bg = getMacroColor(b.macro);
                        
                        let jaEstudou = listaEstudos.some(e => e.tema === b.tema);
                        
                        if (jaEstudou) {
                            cont = `<div style="font-size:10px; color:white; text-shadow: 0px 1px 3px rgba(0,0,0,0.4);">
                                      <b>✅ Estudado</b><br>${b.tema.substring(0,30)}...
                                    </div>`;
                        } else {
                            cont = `<div style="font-size:10px; color:white; text-shadow: 0px 1px 3px rgba(0,0,0,0.4);"><b>📘 Teoria</b><br>${b.tema.substring(0,30)}...
                                      <div style="display:flex; gap:4px; margin-top:8px;">
                                          <button style="padding:4px; font-size:9px; margin:0; border-radius:4px; box-shadow:none; text-shadow:none;" onclick="iniciarTeoriaKanban('${b.macro}', '${b.esp}', '${b.tema}')">✔️ Estudar</button>
                                          <button style="padding:4px; font-size:9px; margin:0; border-radius:4px; box-shadow:none; background:#f39c12; text-shadow:none;" onclick="adiarTemaParaBacklog('${d}', '${t}', '${dtExata}')">📦 Adiar</button>
                                      </div></div>`;
                        }
                    } else if(b.tipo === 'revisao') {
                        bg = '#e67e22';
                        cont = `<div style="font-size:11px; color:white; text-align:center; padding:10px 0;"><b>🔁 FSRS</b><br>Revisões</div>`;
                    }

                    html += `<div style="background:${bg}; padding:8px; border-radius:6px; margin-bottom:8px; min-height:60px; display:flex; flex-direction:column; justify-content:center;">
                                <div style="font-size:9px; opacity:0.8; margin-bottom:4px; text-transform:uppercase; font-weight:bold; color:${(b.tipo==='ocupado' || b.tipo==='estudo') ?'#95a5a6':'white'};">${label}</div>
                                ${cont}
                             </div>`;
                });
                html += `</div>`;
            });
            res.innerHTML = html + "</div>";
            res.style.display = "block";
            renderizarMapaRodizio();
        }

        function setFiltroBacklog(macro) {
            filtroBacklog = macro;
            document.querySelectorAll('#listaBacklogAdiados').forEach(el => {
                el.parentElement.querySelectorAll('.sub-aba-btn').forEach(b => b.classList.remove('active'));
            });
            let idBtn = macro === 'todas' ? 'btnFiltroBacklog_todas' : 
                        macro === 'Clínica Médica' ? 'btnFiltroBacklog_CM' : 
                        macro === 'Cirurgia' ? 'btnFiltroBacklog_CIR' : 
                        macro === 'Ginecologia e Obstetrícia' ? 'btnFiltroBacklog_GO' : 
                        macro === 'Pediatria' ? 'btnFiltroBacklog_PED' : 'btnFiltroBacklog_PREV';
            let btnElement = document.getElementById(idBtn);
            if(btnElement) btnElement.classList.add('active');
            renderizarBacklogAdiados();
        }

        function renderizarBacklogAdiados() {
            let div = document.getElementById("listaBacklogAdiados"); div.innerHTML = "";
            if(backlogAdiados.length === 0) { 
                div.innerHTML = `<div class="empty-state"><div class="empty-ico">📦</div><b>Nenhum tema no Backlog.</b></div>`; 
                return; 
            }
            
            let filtrados = backlogAdiados.map((item, idx) => ({item, idxOriginal: idx}))
                .filter(x => filtroBacklog === 'todas' || x.item.macro === filtroBacklog)
                .sort((a,b) => b.item.peso - a.item.peso);

            if(filtrados.length === 0) { 
                div.innerHTML = `<div class="empty-state"><div class="empty-ico">📭</div><b>Nenhum tema para este filtro.</b></div>`; 
                return; 
            }

            let hj = new Date();
            filtrados.forEach((obj) => {
                let item = obj.item;
                let idx = obj.idxOriginal;
                let dAdiado = new Date(item.dataRealAdiado || hj.toISOString());
                let diffDias = Math.floor((hj - dAdiado)/(1000*60*60*24));
                let alertaTempo = diffDias > 14 ? `<span style="color:var(--danger-color); font-weight:bold;">⚠️ Há ${diffDias} dias</span>` : `<span style="color:#7f8c8d;">Há ${diffDias} dias</span>`;

                div.innerHTML += `<div style="padding:15px; border-bottom:1px solid var(--border-color); display:flex; justify-content:space-between; align-items:center; font-size:13px; gap:10px; background: ${item.peso >= 4 ? 'rgba(243, 156, 18, 0.05)' : 'var(--card-bg)'}; flex-wrap:wrap;">
                    <div style="flex:1; min-width:200px;">
                        <span class="badge" style="background:var(--header-bg); margin-bottom:6px;">Peso ${item.peso}</span> ${alertaTempo}<br>
                        <b style="font-size:14px;">${item.tema}</b><br><em style="opacity:0.7; font-size:11px;">Motivo: ${item.motivo} (${item.dataAdiado})</em>
                    </div>
                    <div style="display:flex; flex-direction:column; gap:8px; min-width:130px; flex:1;">
                        <button class="btn-acao" style="margin:0; padding:8px; font-size:11px; background:var(--primary-color); box-shadow:var(--shadow-btn);" onclick="abrirModalResgate(${idx})">📥 Resgatar p/ Turno</button>
                        <button class="btn-acao" style="margin:0; padding:8px; font-size:11px; background:var(--accent-color); box-shadow:var(--shadow-btn);" onclick="converterParaQuestoes(${idx})">⚡ Ir p/ Questões</button>
                    </div>
                </div>`;
            });
        }

        function converterParaQuestoes(idx) {
            if(!confirm("Isso removerá a teoria do backlog e a enviará direto para o fluxo de Questões (Fase 2). Confirmar?")) return;
            let item = backlogAdiados[idx];
            let hoje = hojeLocal();
            listaEstudos.push({
                id: Date.now(), macroArea: item.macro, especialidade: item.esp, tema: item.tema, 
                peso: item.peso, easeFactor: 2.5, fase: 2, 
                proximaData: hoje,
                historico: [{fase: "Conversão (Direto p/ Questões)", percepcao: "medio", feitas: 0, acertos: 0, data: hoje}]
            });
            backlogAdiados.splice(idx, 1);
            salvarEAtualizar(); 
            renderizarBacklogAdiados();
            alert("Tema movido para o Cronograma de Revisões (Questões) com sucesso!");
        }

        function abrirModalResgate(idx) {
            let item = backlogAdiados[idx];
            let cg = cronogramaSemanasStore[semanaAtivaId];
            if(!cg) return alert("Gere o cronograma da semana ativa primeiro!");

            let slotsLivres = [];
            diasSemanaArr.forEach((d, dIdx) => {
                turnosArr.forEach(t => {
                    if(cg[d] && cg[d][t] && (cg[d][t].tipo === 'revisao' || cg[d][t].tipo === 'descanso' || cg[d][t].tipo === 'estudo')) {
                        let labelT = t==='m'?'Manhã':t==='t'?'Tarde':'Noite';
                        let dt = calcularDataExataRodizio(semanaAtivaId, dIdx);
                        slotsLivres.push({ d, t, desc: `${d} (${dt}) - ${labelT}` });
                    }
                });
            });

            if(slotsLivres.length === 0) return alert("Não há turnos de revisão, descanso ou estudo livres nesta semana para resgate!");

            let msg = `Escolha o turno para encaixar:\n` + slotsLivres.map((s, i) => `${i+1}. ${s.desc}`).join('\n');
            let esc = prompt(msg, "1");
            if(esc === null) return;
            let num = parseInt(esc) - 1;
            if(isNaN(num) || num < 0 || num >= slotsLivres.length) return alert("Opção inválida!");

            let destino = slotsLivres[num];

            removerAlocacaoAnterior(item.tema);

            cg[destino.d][destino.t] = { tipo: 'teoria', macro: item.macro, esp: item.esp, tema: item.tema, peso: item.peso };
            
            cronogramaSemanasStore[semanaAtivaId] = cg;
            window.syncToCloud();

            renderizarKanbanSemanaAtiva();
            renderizarBacklogAdiados();
            if(document.getElementById('tab-rastreio').classList.contains('active')) renderizarRastreio();

            alert("Tema resgatado com sucesso para o turno escolhido!");
        }

        function iniciarTeoriaKanban(macro, esp, tema) {
            mudarAba('tab-estudo', document.querySelectorAll('.sidebar-btn')[3]);
            document.getElementById('reg_macro').value = macro;
            popularEspecialidades('reg_macro', 'reg_esp', 'reg_subtema', false);
            document.getElementById('reg_esp').value = esp;
            popularSubtemas('reg_macro', 'reg_esp', 'reg_subtema', false);
            document.getElementById('reg_subtema').value = tema;
        }

        // ------------------------------------------------------------------
        // PROVAS E LINHA DO TEMPO
        // ------------------------------------------------------------------
        function adicionarOuEditarProva(e) {
            e.preventDefault();
            let pObj = { nome: document.getElementById("provaNome").value, data: document.getElementById("provaData").value, tipo: document.getElementById("provaTipo").value };
            let editId = document.getElementById("provaIdEdit").value;
            if (editId !== "") agendaProvas[parseInt(editId)] = pObj; else agendaProvas.push(pObj);
            
            document.getElementById("provaIdEdit").value = ""; e.target.reset(); document.getElementById("btnSalvarProva").innerText = "Adicionar Prova";
            salvarEAtualizar();
            renderizarListasPlanejamento(); 
        }

        function editarProva(idx) {
            let p = agendaProvas[idx];
            document.getElementById("provaNome").value = p.nome; document.getElementById("provaData").value = p.data;
            document.getElementById("provaTipo").value = p.tipo; document.getElementById("provaIdEdit").value = idx;
            document.getElementById("btnSalvarProva").innerText = "Salvar Alterações";
        }

        function adicionarRodizio(e) {
            e.preventDefault();
            cronogramaInternato.push({ inicio: document.getElementById("rodInicio").value, fim: document.getElementById("rodFim").value, macro: document.getElementById("rodMacro").value });
            cronogramaInternato.sort((a, b) => new Date(a.inicio + 'T00:00:00') - new Date(b.inicio + 'T00:00:00'));
            e.target.reset(); 
            window.syncToCloud();
            renderizarListasPlanejamento();
            atualizarSeletorSemanasPorRodizios();
        }

        function renderizarWidgetProvas() {
            let div = document.getElementById("widgetProvasContainer");
            if (!div) return;
            div.innerHTML = "";
            
            let pp = agendaProvas.find(x => x.tipo === "principal");
            let hj = new Date(); hj.setHours(0,0,0,0);

            let html = "";
            if (pp) {
                let dP = new Date(pp.data + 'T00:00:00');
                let diff = Math.ceil((dP - hj) / (1000 * 60 * 60 * 24));
                if(diff > 0) {
                    html += `<div style="text-align:center; padding: 25px; background: var(--bg-color); border: 2px solid var(--accent-color); border-radius: 12px; margin-bottom:20px;">
                                <h3 style="margin:0; color:var(--accent-color); font-size:28px;">⏳ Faltam ${diff} dias</h3>
                                <p style="margin:8px 0 0; font-size:15px; font-weight:bold;">Para o ${pp.nome} (${pp.data.split('-').reverse().join('/')})</p>
                             </div>`;
                } else if (diff === 0) {
                    html += `<div style="text-align:center; padding: 25px; background: var(--bg-color); border: 2px solid var(--primary-color); border-radius: 12px; margin-bottom:20px;">
                                <h3 style="margin:0; color:var(--primary-color); font-size:28px;">🎉 É Hoje!</h3>
                                <p style="margin:8px 0 0; font-size:15px; font-weight:bold;">Boa prova no ${pp.nome}!</p>
                             </div>`;
                } else {
                     html += `<div style="text-align:center; padding: 25px; border: 2px solid var(--border-color); border-radius: 12px; margin-bottom:20px; opacity: 0.6;">
                                <h3 style="margin:0; color:#7f8c8d; font-size:20px;">Prova Concluída</h3>
                                <p style="margin:8px 0 0; font-size:15px; font-weight:bold;">${pp.nome}</p>
                             </div>`;
                }
            } else {
                html += `<div style="text-align:center; padding: 30px; border: 2px dashed var(--border-color); border-radius: 12px; margin-bottom:20px; opacity:0.7;">
                            <p style="margin:0; font-size:15px;">Nenhuma Prova Principal cadastrada.</p>
                         </div>`;
            }

            let secundarias = agendaProvas.filter(x => x.tipo === "secundaria");
            if (secundarias.length > 0) {
                html += `<div style="display:flex; gap:12px; flex-wrap:wrap; justify-content:center;">`;
                secundarias.forEach(sec => {
                    let dS = new Date(sec.data + 'T00:00:00');
                    let diffS = Math.ceil((dS - hj) / (1000 * 60 * 60 * 24));
                    if(diffS >= 0) {
                        html += `<div style="background:var(--bg-color); border:1px solid var(--border-color); padding:12px 15px; border-radius:10px; font-size:13px; text-align:center; flex:1; min-width:140px; box-shadow:var(--shadow-btn);">
                                    <b>${sec.nome}</b><br><span style="color:var(--accent-color); font-weight:bold; font-size:16px;">${diffS} dias</span>
                                 </div>`;
                    }
                });
                html += `</div>`;
            }
            div.innerHTML = html;
        }

        function renderizarListasPlanejamento() {
            let ulP = document.getElementById("listaProvas"); ulP.innerHTML = "";
            agendaProvas.forEach((p, i) => ulP.innerHTML += `<li style="margin-bottom:10px; display:flex; align-items:center; gap:10px;"><b>${p.nome}</b> (${p.data}) - [${p.tipo}] <button class="btn-acao btn-edit" onclick="editarProva(${i})">Editar</button> <span style="color:red;cursor:pointer; font-weight:bold;" onclick="agendaProvas.splice(${i},1); window.syncToCloud(); renderizarListasPlanejamento();">[X]</span></li>`);
            
            let divR = document.getElementById("listaRodizios"); divR.innerHTML = "";
            let hj = hojeLocal();
            cronogramaInternato.forEach((r, i) => {
                let isActive = (r.inicio <= hj && r.fim >= hj);
                divR.innerHTML += `<div style="padding:12px; border-radius:8px; border:1px solid var(--border-color); margin-bottom:8px; ${isActive ? 'background:rgba(39, 174, 96, 0.05); font-weight:bold; border-left:4px solid var(--primary-color);' : 'background:var(--bg-color);'}">${isActive ? '🟢 [ATUAL] ' : ''}<b>${r.macro}</b>: ${r.inicio} até ${r.fim} <span style="color:var(--danger-color); cursor:pointer; float:right; font-weight:bold;" onclick="cronogramaInternato.splice(${i},1); window.syncToCloud(); renderizarListasPlanejamento(); atualizarSeletorSemanasPorRodizios();">[X]</span></div>`;
            });

            renderizarWidgetProvas();
        }

        // ------------------------------------------------------------------
        // ESTATÍSTICAS NERDS E METAS
        // ------------------------------------------------------------------
        function atualizarEstatisticasNerds() {
            let hj = hojeLocal();
            let d7 = new Date(); d7.setDate(d7.getDate() - 7); let s7 = hojeLocal(d7);
            let d30 = new Date(); d30.setDate(d30.getDate() - 30); let s30 = hojeLocal(d30);
            let tH=0, qH=0, tS=0, qS=0, tM=0, qM=0; let aTop = {};

            listaEstudos.forEach(i => {
                i.historico.forEach(h => {
                    let d = h.data; let t = h.tempo || 0; let q = h.feitas || 0;
                    if(d === hj) { tH += t; qH += q; }
                    if(d >= s7) { tS += t; qS += q; }
                    if(d >= s30) { tM += t; qM += q; aTop[i.macroArea] = (aTop[i.macroArea] || 0) + t; }
                });
            });

            Object.keys(pomodoroDiario).forEach(k => {
                let val = pomodoroDiario[k] || 0;
                if(k === hj) tH += val; if(k >= s7) tS += val; if(k >= s30) tM += val;
            });

            let fT = m => `${Math.floor(m / 60)}h ${m % 60}m`;
            document.getElementById("nerdTempoHoje").innerText = fT(tH); document.getElementById("nerdQtsHoje").innerText = qH;
            document.getElementById("nerdTempoSemana").innerText = fT(tS); document.getElementById("nerdQtsSemana").innerText = qS;
            document.getElementById("nerdTempoMes").innerText = fT(tM); document.getElementById("nerdQtsMes").innerText = qM;
            document.getElementById("focusTotalTime").innerText = tH;

            let top = Object.entries(aTop).sort((a, b) => b[1] - a[1]).slice(0, 3);
            let ul = document.getElementById("nerdTopAreas"); ul.innerHTML = "";
            if(top.length === 0) ul.innerHTML = "<li>Sem registros de tempo focado no mês.</li>";
            top.forEach((x, i) => ul.innerHTML += `<li>${i === 0 ? '🥇' : i === 1 ? '🥈' : '🥉'} <b>${x[0]}</b>: ${fT(x[1])}</li>`);
        }

        function renderizarMetasSubtemas() {
            let m = document.getElementById("meta_macro").value; let e = document.getElementById("meta_esp").value;
            let tbody = document.getElementById("tabela-metas-subtemas"); tbody.innerHTML = "";
            if (!m || m === 'todas') return;
            
            let esps = (e === 'todas') ? Object.keys(taxonomia[m]) : [e];
            esps.forEach(esp => {
                if(!taxonomia[m][esp]) return;
                taxonomia[m][esp].forEach(sub => {
                    let meta = sub.p === 5 ? 120 : sub.p === 4 ? 80 : sub.p === 3 ? 50 : sub.p === 2 ? 30 : 15;
                    let ft = 0; listaEstudos.forEach(i => { if (i.tema === sub.n) i.historico.forEach(h => ft += (h.feitas || 0)); });
                    let pct = (ft / meta) * 100;
                    let bg = pct >= 100 ? "#27ae60" : pct >= 75 ? "#2980b9" : pct >= 50 ? "#e67e22" : "#c0392b";
                    tbody.innerHTML += `<tr><td><small style="opacity:0.7">${esp}</small><br><strong>${sub.n}</strong><span class="peso-tag p${sub.p}">P${sub.p}</span></td><td class="ta-c">${meta} qts</td><td class="ta-c">${ft}</td><td class="ta-c"><span class="badge" style="background:${bg}; padding:6px 10px; font-size:12px;">${pct.toFixed(0)}%</span></td></tr>`;
                });
            });
        }

        function renderizarPlanilha() {
            let m = document.getElementById("plan_macro").value; let e = document.getElementById("plan_esp").value;
            let tbody = document.getElementById("tabela-planilha"); tbody.innerHTML = "";
            if (!m || m === 'todas' || !e || e === 'todas') { tbody.innerHTML = `<tr><td colspan='3' style='text-align:center; padding:20px; opacity:0.7;'>Selecione uma Macro-Área e Especialidade nos filtros acima!</td></tr>`; return; }
            
            taxonomia[m][e].forEach(sub => {
                if (!notasIncidencia[sub.n]) notasIncidencia[sub.n] = { text: "", img: "" }; let nota = notasIncidencia[sub.n];
                tbody.innerHTML += `<tr><td><strong>${sub.n}</strong><span class="peso-tag p${sub.p}">P${sub.p}</span></td>
                <td><input class="m-0" type="text" value="${nota.text}" placeholder="O que mais cai..." onchange="sNI('${sub.n}', 'text', this.value)"></td>
                <td><input class="m-0" type="url" value="${nota.img}" placeholder="URL Imagem" onchange="sNI('${sub.n}', 'img', this.value)">${nota.img ? `<br><img src="${nota.img}" style="max-width:100%;border-radius:4px;margin-top:5px; box-shadow:var(--shadow-btn);">` : ''}</td></tr>`;
            });
        }
        function sNI(tema, tipo, val) { if (!notasIncidencia[tema]) notasIncidencia[tema] = {}; notasIncidencia[tema][tipo] = val; window.syncToCloud(); renderizarPlanilha(); }

        // ------------------------------------------------------------------
        // MÉTRICAS DE ESTUDO & SIMULADOS
        // ------------------------------------------------------------------
        function salvarSimulado(e) {
            e.preventDefault();
            listaSimulados.push({ id: Date.now(), data: hojeLocal(), nome: document.getElementById("simNome").value, acertos: parseInt(document.getElementById("simAcertos").value), total: parseInt(document.getElementById("simTotal").value), areas: { CM: document.getElementById("simCM").value, CIR: document.getElementById("simCIR").value, GO: document.getElementById("simGO").value, PED: document.getElementById("simPED").value, PREV: document.getElementById("simPREV").value } });
            e.target.reset(); salvarEAtualizar();
        }

        window.excluirSimulado = function(idx) {
            if(confirm("Deseja realmente excluir este simulado? O gráfico será atualizado imediatamente.")) {
                listaSimulados.splice(idx, 1);
                salvarEAtualizar();
            }
        };

        let chartRadar = null, chartSim = null, chartEvo = null;
        function atualizarRadar() { try { temaChart(); renderizarHeatmap(); atualizarGraficoTendencia(); } catch (e) { console.error(e); }
            let ctx = document.getElementById('graficoRadar').getContext('2d'); if (chartRadar) chartRadar.destroy();
            let macros = ["Clínica Médica", "Cirurgia", "Ginecologia e Obstetrícia", "Pediatria", "Preventiva"];
            let counts = [0, 0, 0, 0, 0]; let totals = [0, 0, 0, 0, 0];

            Object.keys(taxonomia).forEach(m => {
                let idx = macros.indexOf(m);
                if (idx !== -1) { Object.values(taxonomia[m]).forEach(arr => totals[idx] += arr.length); }
            });
            listaEstudos.forEach(i => { let idx = macros.indexOf(i.macroArea); if (idx !== -1) counts[idx]++; });
            let data = counts.map((c, idx) => Math.min(100, Math.round((c / totals[idx]) * 100) || 0));

            chartRadar = new Chart(ctx, { type: 'radar', data: { labels: ["Clínica", "Cirurgia", "GO", "Pediatria", "Preventiva"], datasets: [{ label: '% Cobertura', data: data, backgroundColor: 'rgba(52, 152, 219, 0.2)', borderColor: '#3498db', pointBackgroundColor: '#2980b9' }] }, options: { scales: { r: { max: 100, min: 0 } } } });

            let listaHtml = "<ul style='list-style:none; padding:0; margin:0; line-height:2.4;'>";
            let labelsCurto = ["Clínica", "Cirurgia", "GO", "Pediatria", "Preventiva"];
            macros.forEach((m, idx) => {
                let pct = data[idx];
                let cTotal = totals[idx];
                let cFeito = counts[idx];
                let color = pct >= 70 ? "var(--primary-color)" : pct >= 40 ? "#f39c12" : "var(--danger-color)";
                listaHtml += `<li><b>${labelsCurto[idx]}:</b> <span style="color:${color}; font-weight:bold; font-size:16px;">${pct}%</span> <small style="opacity:0.6; margin-left:5px;">(${cFeito}/${cTotal})</small></li>`;
            });
            listaHtml += "</ul>";
            let elLista = document.getElementById('radarValoresLista');
            if(elLista) elLista.innerHTML = listaHtml;
        }

        function atualizarGraficoSimulados() {
            let ctx = document.getElementById('graficoSimulados').getContext('2d'); if (chartSim) chartSim.destroy();
            chartSim = new Chart(ctx, { type: 'line', data: { labels: listaSimulados.map(x => x.nome), datasets: [{ label: '% Acertos', data: listaSimulados.map(x => Math.round((x.acertos / x.total) * 100)), borderColor: '#e74c3c', backgroundColor: 'rgba(231, 76, 60, 0.1)', fill:true, tension: 0.3 }] }, options: { scales: { y: { max: 100, min: 0 } } } });

            let div = document.getElementById("listaSimulados"); div.innerHTML = "";
            listaSimulados.forEach((x, idx) => {
                let st = Object.entries(x.areas).filter(y => y[1]).map(y => `${y[0]}: ${y[1]}`).join(' | ');
                div.innerHTML += `<div style="padding:10px 0; border-bottom:1px solid var(--border-color); font-size:13px; display:flex; justify-content:space-between; align-items:center;">
                    <div>
                        <b>${x.nome}</b>
                        <div style="font-size:11px; opacity:0.7; margin-top:4px;">${st ? `[${st}]` : 'Geral'}</div>
                    </div>
                    <div style="display:flex; align-items:center; gap:15px;">
                        <span><span style="color:var(--danger-color); font-weight:bold; font-size:14px;">${Math.round((x.acertos/x.total)*100)}%</span> <small style="opacity:0.6;">(${x.acertos}/${x.total})</small></span>
                        <button onclick="excluirSimulado(${idx})" style="background:var(--danger-color); color:white; border:none; border-radius:4px; padding:4px 8px; font-size:10px; cursor:pointer; box-shadow:var(--shadow-btn);">Excluir</button>
                    </div>
                </div>`;
            });
        }

        function atualizarGraficoEvolucao() {
            let ctx = document.getElementById('graficoEvolucao').getContext('2d'); if (chartEvo) chartEvo.destroy();
            let m = document.getElementById("evo_macro").value; let e = document.getElementById("evo_esp").value; let s = document.getElementById("evo_subtema").value;
            let hist = [];
            listaEstudos.forEach(i => {
                if (m !== 'todas' && i.macroArea !== m) return;
                if (e !== 'todas' && i.especialidade !== e) return;
                if (s !== 'todas' && i.tema !== s) return;
                i.historico.forEach(h => { if (h.feitas > 0) hist.push({ d: h.data, n: i.tema.substring(0, 10), t: (h.acertos / h.feitas) * 100 }); });
            });
            hist.sort((x, y) => new Date(x.d) - new Date(y.d));
            chartEvo = new Chart(ctx, { type: 'line', data: { labels: hist.map(x => x.n), datasets: [{ label: '% Acertos', data: hist.map(x => x.t), borderColor: '#2ecc71', backgroundColor: 'rgba(46, 204, 113, 0.1)', fill:true, tension: 0.3 }] }, options: { scales: { y: { max: 100, min: 0 } } } });
        }

        // ------------------------------------------------------------------
        // CONFIGURAÇÕES & BACKUP
        // ------------------------------------------------------------------
        function salvarConfiguracoes() {
            let intStr = document.getElementById("configIntervalos").value;
            configAlgoritmo = {
                intervalos: intStr.split(',').map(n => parseInt(n.trim())).filter(n => !isNaN(n)),
                p5: parseFloat(document.getElementById("configPeso5").value),
                p4: parseFloat(document.getElementById("configPeso4").value),
                p3: parseFloat(document.getElementById("configPeso3").value),
                p2: parseFloat(document.getElementById("configPeso2").value),
                p1: parseFloat(document.getElementById("configPeso1").value),
                burnout: parseInt(document.getElementById("configBurnout").value)
            };
            window.syncToCloud();
            alert("Configurações salvas!"); atualizarDashboard();
        }

        function atualizarMetaAnual() {
            metaAnual = parseInt(document.getElementById("metaInputConfig").value) || 10000;
            salvarEAtualizar();
        }

        function exportarBackup() {
            let dados = { 
                estudos: listaEstudos, notas: notasIncidencia, simulados: listaSimulados, config: configAlgoritmo, 
                provas: agendaProvas, internato: cronogramaInternato, 
                grades_semanais: gradesSemanaisStore,
                crono_semanas: cronogramaSemanasStore, backlog: backlogAdiados,
                pomodoro: pomodoroDiario, streak: streak 
            };
            let blob = new Blob([JSON.stringify(dados, null, 2)], { type: 'json' }); let url = URL.createObjectURL(blob); let a = document.createElement('a'); a.href = url; a.download = `backup_residencia_${hojeLocal()}.json`; a.click();
        }

        function importarBackup(e) {
            let file = e.target.files[0]; if (!file) return;
            let reader = new FileReader();
            reader.onload = function(evt) {
                try {
                    let d = JSON.parse(evt.target.result);
                    window.hydrateData(d);
                    window.syncToCloud();
                    alert("Backup restaurado com sucesso!");
                } catch(err) { alert("Arquivo JSON inválido."); }
            }; reader.readAsText(file);
        }

        function salvarEAtualizar() {
            window.syncToCloud();
            renderizarTabela(); 
            atualizarRadar(); 
            atualizarGraficoSimulados(); 
            renderizarMetasSubtemas(); 
            renderizarPlanilha(); 
            atualizarEstatisticasNerds();
            renderizarMapaRodizio();
            if(document.getElementById('tab-rastreio').classList.contains('active')) renderizarRastreio();
        }
    
        // ==================================================================
        // UTILITÁRIOS
        // ==================================================================
        function hojeLocal(d) { d = d || new Date(); return d.getFullYear() + '-' + String(d.getMonth() + 1).padStart(2, '0') + '-' + String(d.getDate()).padStart(2, '0'); }
        function esc(s) { return String(s).replace(/[&<>"']/g, c => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c])); }
        function diasEntre(a, b) { return Math.floor((new Date(b + 'T00:00:00') - new Date(a + 'T00:00:00')) / 864e5); }
        function cssVar(n) { return getComputedStyle(document.body).getPropertyValue(n).trim(); }
        function irParaAba(id) { const b = [...document.querySelectorAll('.sidebar-btn')].find(x => (x.getAttribute('onclick') || '').includes("'" + id + "'")); mudarAba(id, b); }
        function temaChart() { if (!window.Chart) return; Chart.defaults.color = cssVar('--muted'); Chart.defaults.borderColor = cssVar('--border-color'); Chart.defaults.font.family = cssVar('--font-body'); }

        // ==================================================================
        // DASHBOARD: REVISÕES DE HOJE, ONBOARDING E ALERTAS
        // ==================================================================
        function renderizarPainelInicial() { renderizarOnboarding(); renderizarHoje(); renderizarAlertas(); }

        function revisoesDoDia() {
            const hj = hojeLocal();
            return listaEstudos.map((i, idx) => ({ i, idx }))
                .filter(({ i }) => i.fase <= 6 && i.proximaData <= hj && !(sprintAtivo && i.peso < 4))
                .sort((a, b) => a.i.proximaData.localeCompare(b.i.proximaData) || b.i.peso - a.i.peso);
        }
        function comecarRevisao() {
            const lista = revisoesDoDia(); if (!lista.length) return false;
            window._seqRevisao = true; abrirModal(lista[0].idx); return true;
        }
        function renderizarHoje() {
            const el = document.getElementById('listaRevisoesHoje'); if (!el) return;
            const lista = revisoesDoDia(), hj = hojeLocal(), atras = lista.filter(x => x.i.proximaData < hj).length;
            document.getElementById('heroResumo').textContent = lista.length
                ? lista.length + (lista.length === 1 ? ' revisão pendente' : ' revisões pendentes') + (atras ? ' · ' + atras + ' em atraso' : '')
                : 'Nada pendente por enquanto.';
            document.getElementById('btnComecarRevisao').disabled = !lista.length;
            if (!lista.length) {
                el.innerHTML = `<div class="empty-state"><div class="empty-ico">✨</div><b>Tudo em dia.</b><br><span class="hint">${listaEstudos.length ? 'Aproveite para estudar um tema novo.' : 'Registre seu primeiro estudo para gerar revisões.'}</span></div>`;
                return;
            }
            el.innerHTML = lista.slice(0, 6).map(({ i, idx }) => {
                const atr = i.proximaData < hj;
                const fase = i.fase === 0.5 ? 'Anki (teoria)' : i.fase === 1 ? '1ª rev. (Anki)' : Math.floor(i.fase) + 'ª rev. (questões)';
                return `<div class="rev-item" role="button" tabindex="0" style="--macro:${getMacroColor(i.macroArea)}" onclick="window._seqRevisao=false;abrirModal(${idx})" onkeydown="if(event.key==='Enter')this.click()"><div><strong>${esc(i.tema)}</strong><small>${esc(i.especialidade)} · ${fase}</small></div><span class="tag ${atr ? 'tag-danger' : 'tag-ok'}">${atr ? 'Atrasada' : 'Hoje'}</span></div>`;
            }).join('') + (lista.length > 6 ? `<p class="hint ta-c m-0">+ ${lista.length - 6} na fila</p>` : '');
        }
        function renderizarOnboarding() {
            const el = document.getElementById('onboardingCard'); if (!el) return;
            const passos = [
                { ok: agendaProvas.some(p => p.tipo === 'principal'), t: 'Defina sua prova principal', d: 'Ativa a contagem regressiva e a reta final.', aba: 'tab-planejamento', b: 'Definir prova' },
                { ok: cronogramaInternato.length > 0, t: 'Monte seu rodízio', d: 'Distribui os temas do edital pelas semanas.', aba: 'tab-planejamento', b: 'Criar rodízio' },
                { ok: listaEstudos.length > 0, t: 'Registre o primeiro estudo', d: 'Gera as revisões espaçadas automaticamente.', aba: 'tab-estudo', b: 'Registrar estudo' }
            ];
            const feitos = passos.filter(p => p.ok).length;
            if (feitos === passos.length) { el.style.display = 'none'; return; }
            el.style.display = 'block';
            el.innerHTML = `<h2>Comece por aqui</h2><p class="hint">${feitos} de ${passos.length} passos concluídos</p><div class="progress-bar-bg"><div class="progress-bar-fill" style="width:${feitos / passos.length * 100}%"></div></div><ol class="steps">${passos.map((p, k) => `<li class="${p.ok ? 'done' : ''}"><span class="step-n">${p.ok ? '✅' : k + 1}</span><div><strong>${p.t}</strong><small>${p.d}</small></div>${p.ok ? '' : `<button class="btn-acao" onclick="irParaAba('${p.aba}')">${p.b}</button>`}</li>`).join('')}</ol>`;
        }
        function renderizarAlertas() {
            const el = document.getElementById('alertasInteligentes'); if (!el) return;
            const hj = hojeLocal(), alertas = [], lim = configAlgoritmo.burnout || 15;
            const d3 = new Date(); d3.setDate(d3.getDate() + 3); const fim = hojeLocal(d3);
            const carga = listaEstudos.filter(i => i.fase <= 6 && i.proximaData >= hj && i.proximaData <= fim).length;
            if (carga >= lim) alertas.push({ nivel: 'danger', t: 'Risco de saturação', d: carga + ' revisões nos próximos 3 dias (limite: ' + lim + ').' });
            const frios = listaEstudos.filter(i => {
                if (i.peso !== 5 || i.fase > 6) return false;
                const h = i.historico || [], q = h.filter(x => x.feitas > 0).map(x => x.data).sort().pop();
                const ref = q || (h[0] || {}).data; return ref && diasEntre(ref, hj) >= 30;
            });
            if (frios.length) alertas.push({ nivel: 'warn', t: frios.length + (frios.length === 1 ? ' tema de peso 5 está' : ' temas de peso 5 estão') + ' há 30 dias ou mais sem questões', d: frios.slice(0, 4).map(i => esc(i.tema)).join(' · ') + (frios.length > 4 ? ' …' : '') });
            const por = {};
            listaEstudos.forEach(i => (i.historico || []).forEach(x => { if (x.feitas > 0) { const o = por[i.macroArea] = por[i.macroArea] || { f: 0, a: 0 }; o.f += x.feitas; o.a += x.acertos; } }));
            Object.keys(por).forEach(m => { const o = por[m], pct = Math.round(o.a / o.f * 100); if (o.f >= 20 && pct < 60) alertas.push({ nivel: 'danger', t: esc(m) + ': acerto de ' + pct + '%', d: o.a + ' acertos em ' + o.f + ' questões, abaixo da meta de 60%.', aba: 'tab-analytics' }); });
            el.innerHTML = alertas.length
                ? alertas.map(a => `<div class="alert-item ${a.nivel}"><span class="alert-ico">⚠️</span><div><strong>${a.t}</strong><small>${a.d}</small></div>${a.aba ? `<button class="btn-acao btn-neutral" onclick="irParaAba('${a.aba}')">Ver evolução</button>` : ''}</div>`).join('')
                : '<div class="empty-state"><div class="empty-ico">✅</div><b>Nenhum alerta no momento.</b></div>';
        }

        // ==================================================================
        // GRÁFICOS: HEATMAP DE ESTUDO E TENDÊNCIA DE ACERTOS
        // ==================================================================
        function renderizarHeatmap() {
            const el = document.getElementById('heatmapEstudo'); if (!el) return;
            const min = {}; listaEstudos.forEach(i => (i.historico || []).forEach(x => { if (x.data) min[x.data] = (min[x.data] || 0) + (x.tempo || 0); }));
            const hoje = new Date(); hoje.setHours(0, 0, 0, 0);
            const ini = new Date(hoje); ini.setDate(ini.getDate() - 175 - hoje.getDay());
            let html = '', total = 0, dias = 0;
            for (let d = new Date(ini); d <= hoje; d.setDate(d.getDate() + 1)) {
                const k = hojeLocal(d), m = min[k] || 0; total += m; if (m) dias++;
                html += `<i class="hm l${!m ? 0 : m < 30 ? 1 : m < 60 ? 2 : m < 120 ? 3 : 4}" title="${k.split('-').reverse().join('/')}: ${m} min"></i>`;
            }
            el.innerHTML = html;
            document.getElementById('heatmapResumo').textContent = Math.round(total / 60) + ' h em ' + dias + ' dias de estudo nos últimos 6 meses';
        }
        let chartTend = null;
        function atualizarGraficoTendencia() {
            const cv = document.getElementById('graficoTendencia'); if (!cv || !window.Chart) return; if (chartTend) chartTend.destroy();
            const seg = s => { const x = new Date(s + 'T00:00:00'); x.setDate(x.getDate() - ((x.getDay() + 6) % 7)); return hojeLocal(x); };
            const base = new Date(seg(hojeLocal()) + 'T00:00:00'), semanas = [];
            for (let k = 11; k >= 0; k--) { const x = new Date(base); x.setDate(x.getDate() - 7 * k); semanas.push(hojeLocal(x)); }
            const macros = { 'Clínica Médica': 'Clínica', 'Cirurgia': 'Cirurgia', 'Ginecologia e Obstetrícia': 'GO', 'Pediatria': 'Pediatria', 'Preventiva': 'Preventiva' };
            const datasets = Object.keys(macros).map(m => {
                const acc = {};
                listaEstudos.forEach(i => { if (i.macroArea !== m) return; (i.historico || []).forEach(x => { if (x.feitas > 0 && x.data) { const o = acc[seg(x.data)] = acc[seg(x.data)] || { f: 0, a: 0 }; o.f += x.feitas; o.a += x.acertos; } }); });
                return { label: macros[m], data: semanas.map(s => acc[s] ? Math.round(acc[s].a / acc[s].f * 100) : null), borderColor: getMacroColor(m), backgroundColor: getMacroColor(m), tension: .3, spanGaps: true, pointRadius: 3, borderWidth: 2 };
            });
            datasets.push({ label: 'Meta 60%', data: semanas.map(() => 60), borderColor: cssVar('--danger-color'), borderDash: [5, 5], pointRadius: 0, borderWidth: 1 });
            chartTend = new Chart(cv.getContext('2d'), { type: 'line', data: { labels: semanas.map(s => s.slice(8) + '/' + s.slice(5, 7)), datasets }, options: { responsive: true, maintainAspectRatio: false, scales: { y: { min: 0, max: 100, ticks: { callback: v => v + '%' } } }, plugins: { legend: { position: 'bottom' } } } });
        }

        // ==================================================================
        // ÍCONES SVG (estilo Lucide) — substitui emojis em todo o DOM, inclusive conteúdo dinâmico
        // ==================================================================
        const ICONES = {
            'home': '<path d="M15 21v-8a1 1 0 0 0-1-1h-4a1 1 0 0 0-1 1v8"/><path d="M3 10a2 2 0 0 1 .709-1.528l7-5.999a2 2 0 0 1 2.582 0l7 5.999A2 2 0 0 1 21 10v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/>',
            'calendar': '<path d="M8 2v4M16 2v4"/><rect width="18" height="18" x="3" y="4" rx="2"/><path d="M3 10h18"/>',
            'compass': '<circle cx="12" cy="12" r="10"/><path d="m16.24 7.76-2.12 6.36-6.36 2.12 2.12-6.36z"/>',
            'pen': '<path d="M12 20h9"/><path d="M16.376 3.622a1 1 0 0 1 3.002 3.002L7.368 18.635a2 2 0 0 1-.855.506l-2.872.838a.5.5 0 0 1-.62-.62l.838-2.872a2 2 0 0 1 .506-.854z"/>',
            'chart': '<path d="M3 3v16a2 2 0 0 0 2 2h16"/><path d="M18 17V9M13 17V5M8 17v-3"/>',
            'gauge': '<path d="m12 14 4-4"/><path d="M3.34 19a10 10 0 1 1 17.32 0"/>',
            'book': '<path d="M12 7v14"/><path d="M3 18a1 1 0 0 1-1-1V4a1 1 0 0 1 1-1h5a4 4 0 0 1 4 4 4 4 0 0 1 4-4h5a1 1 0 0 1 1 1v13a1 1 0 0 1-1 1h-6a3 3 0 0 0-3 3 3 3 0 0 0-3-3z"/>',
            'file': '<path d="M15 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7z"/><path d="M14 2v4a2 2 0 0 0 2 2h4M10 9H8M16 13H8M16 17H8"/>',
            'sliders': '<path d="M10 5H3M12 19H3M14 3v4M16 17v4M21 12h-9M21 19h-5M21 5h-3M8 10v4M8 12H3"/>',
            'shield': '<path d="M20 13c0 5-3.5 7.5-7.66 8.95a1 1 0 0 1-.67-.01C7.5 20.5 4 18 4 13V6a1 1 0 0 1 1-1c2 0 4.5-1.2 6.24-2.72a1.17 1.17 0 0 1 1.52 0C14.51 3.81 17 5 19 5a1 1 0 0 1 1 1z"/>',
            'target': '<circle cx="12" cy="12" r="10"/><circle cx="12" cy="12" r="6"/><circle cx="12" cy="12" r="2"/>',
            'flame': '<path d="M8.5 14.5A2.5 2.5 0 0 0 11 12c0-1.38-.5-2-1-3-1.072-2.143-.224-4.054 2-6 .5 2.5 2 4.9 4 6.5 2 1.6 3 3.5 3 5.5a7 7 0 1 1-14 0c0-1.153.433-2.294 1-3a2.5 2.5 0 0 0 2.5 2.5z"/>',
            'zap': '<path d="M4 14a1 1 0 0 1-.78-1.63l9.9-10.2a.5.5 0 0 1 .86.46l-1.92 6.02A1 1 0 0 0 13 10h7a1 1 0 0 1 .78 1.63l-9.9 10.2a.5.5 0 0 1-.86-.46l1.92-6.02A1 1 0 0 0 11 14z"/>',
            'play': '<polygon points="6 3 20 12 6 21 6 3"/>',
            'pause': '<rect x="14" y="4" width="4" height="16" rx="1"/><rect x="6" y="4" width="4" height="16" rx="1"/>',
            'rotate': '<path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"/><path d="M3 3v5h5"/>',
            'moon': '<path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9z"/>',
            'alert': '<path d="m21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3"/><path d="M12 9v4M12 17h.01"/>',
            'check': '<path d="M21.801 10A10 10 0 1 1 17 3.335"/><path d="m9 11 3 3L22 4"/>',
            'star': '<path d="M11.525 2.295a.53.53 0 0 1 .95 0l2.31 4.679a2.123 2.123 0 0 0 1.595 1.16l5.166.756a.53.53 0 0 1 .294.904l-3.736 3.638a2.123 2.123 0 0 0-.611 1.878l.882 5.14a.53.53 0 0 1-.771.56l-4.618-2.428a2.122 2.122 0 0 0-1.973 0L6.396 21.01a.53.53 0 0 1-.77-.56l.881-5.139a2.122 2.122 0 0 0-.611-1.879L2.16 9.795a.53.53 0 0 1 .294-.906l5.165-.755a2.122 2.122 0 0 0 1.597-1.16z"/>',
            'pin': '<path d="M20 10c0 4.993-5.539 10.193-7.399 11.799a1 1 0 0 1-1.202 0C9.539 20.193 4 14.993 4 10a8 8 0 0 1 16 0"/><circle cx="12" cy="10" r="3"/>',
            'package': '<path d="M11 21.73a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73z"/><path d="M12 22V12M3.29 7 12 12l8.71-5"/>',
            'hourglass': '<path d="M5 22h14M5 2h14M17 22v-4.172a2 2 0 0 0-.586-1.414L12 12l-4.414 4.414A2 2 0 0 0 7 17.828V22M7 2v4.172a2 2 0 0 0 .586 1.414L12 12l4.414-4.414A2 2 0 0 0 17 6.172V2"/>',
            'inbox': '<path d="M22 12h-6l-2 3h-4l-2-3H2"/><path d="M5.45 5.11 2 12v6a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2v-6l-3.45-6.89A2 2 0 0 0 16.76 4H7.24a2 2 0 0 0-1.79 1.11z"/>',
            'sparkles': '<path d="M9.937 15.5A2 2 0 0 0 8.5 14.063l-6.135-1.582a.5.5 0 0 1 0-.962L8.5 9.936A2 2 0 0 0 9.937 8.5l1.582-6.135a.5.5 0 0 1 .963 0L14.063 8.5A2 2 0 0 0 15.5 9.937l6.135 1.581a.5.5 0 0 1 0 .964L15.5 14.063a2 2 0 0 0-1.437 1.437l-1.582 6.135a.5.5 0 0 1-.963 0z"/><path d="M20 3v4M22 5h-4M4 17v2M5 18H3"/>',
            'save': '<path d="M15.2 3a2 2 0 0 1 1.4.6l3.8 3.8a2 2 0 0 1 .6 1.4V19a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2z"/><path d="M17 21v-7a1 1 0 0 0-1-1H8a1 1 0 0 0-1 1v7M7 3v4a1 1 0 0 0 1 1h7"/>',
            'download': '<path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4M7 10l5 5 5-5M12 15V3"/>',
            'trash': '<path d="M3 6h18M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2"/>',
            'search': '<circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/>',
            'menu': '<path d="M4 6h16M4 12h16M4 18h16"/>', 'plus': '<path d="M5 12h14M12 5v14"/>', 'minus': '<path d="M5 12h14"/>',
            'eye': '<path d="M2.062 12.348a1 1 0 0 1 0-.696 10.75 10.75 0 0 1 19.876 0 1 1 0 0 1 0 .696 10.75 10.75 0 0 1-19.876 0"/><circle cx="12" cy="12" r="3"/>',
            'battery': '<rect x="2" y="7" width="16" height="10" rx="2"/><path d="M22 11v2M6 11v2M10 11v2M14 11v2"/>',
            'battery-low': '<rect x="2" y="7" width="16" height="10" rx="2"/><path d="M22 11v2M6 11v2"/>',
            'leaf': '<path d="M11 20A7 7 0 0 1 9.8 6.1C15.5 5 17 4.48 19 2c1 2 2 4.18 2 8 0 5.5-4.78 10-10 10z"/><path d="M2 21c0-3 1.85-5.36 5.08-6C9.5 14.52 12 13 13 12"/>',
            'globe': '<circle cx="12" cy="12" r="10"/><path d="M12 2a14.5 14.5 0 0 0 0 20 14.5 14.5 0 0 0 0-20M2 12h20"/>',
            'heart': '<path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7z"/><path d="M3.22 12H9.5l.5-1 2 4.5 2-7 1.5 3.5h5.27"/>',
            'activity': '<path d="M22 12h-4l-3 9L9 3l-3 9H2"/>',
            'dot-red': '<circle cx="12" cy="12" r="6" fill="currentColor" stroke="none"/>',
            'dot-green': '<circle cx="12" cy="12" r="6" fill="currentColor" stroke="none"/>',
            'dot-gray': '<circle cx="12" cy="12" r="6" fill="currentColor" stroke="none"/>'
        };
        const EMOJI_ICONE = {
            '🧠⚕': 'activity', '🧠': 'activity', '🩺': 'heart', '🏥': 'heart', '🏠': 'home', '📅': 'calendar', '🗓': 'calendar', '🧭': 'compass', '🗺': 'compass',
            '✍': 'pen', '📊': 'chart', '🤓': 'gauge', '📚': 'book', '📘': 'book', '📖': 'book', '📋': 'file', '📝': 'file', '📂': 'file', '⚙': 'sliders', '🎨': 'sliders',
            '🛡': 'shield', '🎯': 'target', '🔥': 'flame', '⚡': 'zap', '🚀': 'zap', '▶': 'play', '⏸': 'pause', '🔄': 'rotate', '🔁': 'rotate', '🌓': 'moon', '⚠': 'alert',
            '✅': 'check', '✔': 'check', '🔴': 'dot-red', '🟢': 'dot-green', '⚪': 'dot-gray', '⭐': 'star', '🥇': 'star', '🥈': 'star', '🥉': 'star', '☰': 'menu', '🏆': 'star', '📌': 'pin',
            '📦': 'package', '⏳': 'hourglass', '📭': 'inbox', '🎉': 'sparkles', '✨': 'sparkles', '🔮': 'sparkles', '💾': 'save', '📥': 'download', '🗑': 'trash',
            '🔎': 'search', '🔍': 'search', '➕': 'plus', '➖': 'minus', '👁': 'eye', '🔋': 'battery', '🪫': 'battery-low', '🌿': 'leaf', '🌐': 'globe'
        };
        const RX_EMOJI = new RegExp('(' + Object.keys(EMOJI_ICONE).sort((a, b) => b.length - a.length).map(k => k.replace(/[.*+?^${}()|[\]\\]/g, '\\$&')).join('|') + ')\uFE0F?\\s*', 'g');
        function iconeSvg(n) { return '<svg class="ic ic-' + n + '" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">' + ICONES[n] + '</svg>'; }
        function iconizar(raiz) {
            if (!raiz) return;
            const nos = [];
            if (raiz.nodeType === 3) nos.push(raiz);
            else if (raiz.nodeType === 1 && !/^(SCRIPT|STYLE|TEXTAREA|SVG|CANVAS)$/i.test(raiz.tagName)) {
                const w = document.createTreeWalker(raiz, NodeFilter.SHOW_TEXT);
                for (let n = w.nextNode(); n; n = w.nextNode()) nos.push(n);
            }
            nos.forEach(n => {
                RX_EMOJI.lastIndex = 0; if (!RX_EMOJI.test(n.nodeValue)) return;
                const pai = n.parentNode; if (!pai || /^(SCRIPT|STYLE|TEXTAREA|TITLE)$/i.test(pai.nodeName)) return;
                RX_EMOJI.lastIndex = 0;
                if (/^(OPTION|OPTGROUP)$/i.test(pai.nodeName)) { n.nodeValue = n.nodeValue.replace(RX_EMOJI, ''); return; }
                const t = document.createElement('template');
                t.innerHTML = esc(n.nodeValue).replace(RX_EMOJI, (m, k) => iconeSvg(EMOJI_ICONE[k]));
                pai.replaceChild(t.content, n);
            });
        }
        const obsIcones = new MutationObserver(muts => {
            muts.forEach(m => { m.addedNodes.forEach(n => { if (n.nodeType === 1 || n.nodeType === 3) iconizar(n); }); if (m.type === 'characterData') iconizar(m.target); });
            obsIcones.takeRecords();
        });
        document.querySelectorAll('[placeholder]').forEach(e => { RX_EMOJI.lastIndex = 0; e.placeholder = e.placeholder.replace(RX_EMOJI, ''); });
        iconizar(document.body);
        obsIcones.observe(document.body, { childList: true, subtree: true, characterData: true });
    </script>
</body>
</html>
