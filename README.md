<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StreetPower — Player Guide</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Oswald:wght@400;500;600;700&family=Roboto+Condensed:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-dark:        #1a1a17;
            --bg-panel:       #32322e;
            --bg-panel-dark:  #252522;
            --bg-panel-light: #3d3d37;
            --accent-rust:    #8b602b;
            --accent-rust-br: #cd6a2c;
            --accent-gold:    #e5c463;
            --text-light:     #e5e5e5;
            --text-muted:     #8c8c8c;
            --text-dim:       #6a6a66;
            --green-active:   #6abe32;
            --red-cancel:     #993333;
            --yellow-warn:    #d4a017;
        }

        * { box-sizing: border-box; }

        html, body {
            margin: 0;
            padding: 0;
            background-color: #0d0d0b;
            background-image:
                radial-gradient(circle at 25% 15%, rgba(139, 96, 43, 0.06) 0%, transparent 50%),
                radial-gradient(circle at 75% 85%, rgba(139, 96, 43, 0.04) 0%, transparent 50%),
                linear-gradient(135deg, #14140f 0%, #0d0d0b 100%);
            color: var(--text-light);
            font-family: 'Roboto Condensed', 'Segoe UI', Tahoma, sans-serif;
            line-height: 1.65;
            min-height: 100vh;
        }

        #document {
            max-width: 920px;
            margin: 40px auto;
            padding: 0;
            background: var(--bg-panel);
            background-image:
                linear-gradient(180deg, rgba(255,255,255,0.02) 0%, transparent 100%),
                repeating-linear-gradient(45deg, transparent 0, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px);
            border: 2px solid #1f1f1c;
            box-shadow:
                0 0 0 1px #4a4a42 inset,
                0 20px 60px rgba(0,0,0,0.7),
                0 0 80px rgba(139, 96, 43, 0.08);
            position: relative;
        }

        #document::before, #document::after,
        .corner-tl, .corner-tr, .corner-bl, .corner-br {
            content: '';
            position: absolute;
            width: 14px; height: 14px;
            background: radial-gradient(circle, #6a6a5a 30%, #2a2a26 70%);
            border-radius: 50%;
            box-shadow: 0 0 4px rgba(0,0,0,0.6) inset, 0 1px 2px rgba(0,0,0,0.4);
        }
        #document::before { top: 10px; left: 10px; }
        #document::after  { top: 10px; right: 10px; }
        .corner-bl { bottom: 10px; left: 10px; }
        .corner-br { bottom: 10px; right: 10px; }

        .doc-header {
            background: var(--bg-panel-dark);
            background-image: linear-gradient(180deg, #2a2a26 0%, #1f1f1c 100%);
            border-bottom: 4px solid var(--accent-rust);
            padding: 36px 40px 28px;
            position: relative;
        }
        .doc-header::after {
            content: '';
            position: absolute;
            left: 0; right: 0; bottom: -4px;
            height: 2px;
            background: linear-gradient(90deg, transparent, var(--accent-gold) 50%, transparent);
            opacity: 0.6;
        }
        .doc-title {
            font-family: 'Oswald', sans-serif;
            font-size: 2.6em;
            font-weight: 700;
            color: var(--accent-gold);
            text-shadow: 0 2px 4px rgba(0,0,0,0.6), 0 0 20px rgba(229, 196, 99, 0.2);
            letter-spacing: 2px;
            text-transform: uppercase;
            margin: 0;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .doc-title .bolt {
            font-size: 0.9em;
            color: var(--accent-rust-br);
            text-shadow: 0 0 12px rgba(205, 106, 44, 0.6);
        }
        .doc-subtitle {
            color: var(--text-muted);
            font-size: 0.95em;
            letter-spacing: 4px;
            text-transform: uppercase;
            margin-top: 6px;
            font-weight: 400;
        }
        .doc-meta {
            position: absolute;
            top: 18px;
            right: 40px;
            font-size: 0.78em;
            color: var(--text-dim);
            text-align: right;
            line-height: 1.4;
        }
        .doc-meta .ver {
            color: var(--accent-gold);
            font-weight: 700;
            font-family: 'Oswald', sans-serif;
        }

        .feature-strip {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1px;
            background: #1f1f1c;
            border-bottom: 1px solid #1f1f1c;
            margin: 0;
            padding: 0;
        }
        .feature-strip .feat {
            background: var(--bg-panel-dark);
            padding: 14px 10px;
            text-align: center;
            font-family: 'Oswald', sans-serif;
            font-size: 0.78em;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: var(--text-light);
        }
        .feature-strip .feat .ico {
            display: block;
            font-size: 1.6em;
            color: var(--accent-rust-br);
            margin-bottom: 4px;
            text-shadow: 0 0 8px rgba(205, 106, 44, 0.4);
        }
        .feature-strip .feat strong {
            color: var(--accent-gold);
            display: block;
            font-size: 1.15em;
            margin-bottom: 2px;
        }

        .doc-body { padding: 36px 44px 24px; }

        h2 {
            font-family: 'Oswald', sans-serif;
            color: var(--accent-gold);
            font-size: 1.55em;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            margin: 36px 0 18px;
            padding: 8px 0 8px 16px;
            border-left: 5px solid var(--accent-rust-br);
            background: linear-gradient(90deg, rgba(205, 106, 44, 0.12) 0%, transparent 60%);
            position: relative;
        }
        h2:first-of-type { margin-top: 0; }
        h2 .ico {
            display: inline-block;
            margin-right: 8px;
            color: var(--accent-rust-br);
        }

        h3 {
            font-family: 'Oswald', sans-serif;
            color: var(--text-light);
            font-size: 1.15em;
            font-weight: 500;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin: 24px 0 10px;
        }

        p { margin: 0 0 14px; color: var(--text-light); }
        strong { color: var(--accent-gold); font-weight: 700; }
        em { color: var(--accent-rust-br); font-style: italic; }

        ul {
            margin: 0 0 16px;
            padding-left: 0;
            list-style: none;
        }
        ul li {
            position: relative;
            padding: 4px 0 4px 26px;
            color: var(--text-light);
        }
        ul li::before {
            content: '▶';
            position: absolute;
            left: 8px;
            top: 4px;
            color: var(--accent-rust-br);
            font-size: 0.75em;
        }

        .step {
            display: flex;
            gap: 18px;
            margin: 18px 0;
            padding: 16px 18px;
            background: rgba(0,0,0,0.18);
            border-left: 3px solid var(--accent-rust);
            border-radius: 0 4px 4px 0;
        }
        .step-num {
            flex-shrink: 0;
            width: 38px; height: 38px;
            background: linear-gradient(180deg, var(--accent-rust-br) 0%, #8a4520 100%);
            color: #1a1a17;
            font-family: 'Oswald', sans-serif;
            font-weight: 700;
            font-size: 1.3em;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 4px;
            box-shadow:
                0 0 0 2px #1f1f1c inset,
                0 2px 4px rgba(0,0,0,0.5);
        }
        .step-content { flex: 1; }
        .step-content h3 { margin: 0 0 6px; color: var(--accent-gold); }
        .step-content p { margin: 0; font-size: 0.97em; color: var(--text-light); }

        .ui-mockup {
            margin: 24px auto;
            max-width: 380px;
            background: var(--bg-panel);
            border: 2px solid #1f1f1c;
            box-shadow:
                0 0 0 1px #4a4a42 inset,
                0 8px 24px rgba(0,0,0,0.6);
            font-family: 'Roboto Condensed', sans-serif;
            position: relative;
        }
        .ui-mockup-accent {
            height: 6px;
            background: var(--accent-rust);
            background-image: linear-gradient(180deg, #a06d33 0%, var(--accent-rust) 100%);
        }
        .ui-mockup-header {
            background: var(--bg-panel-dark);
            padding: 10px 14px;
            border-bottom: 1px solid rgba(139, 96, 43, 0.4);
            text-align: center;
        }
        .ui-mockup-header .title {
            font-family: 'Roboto Condensed', sans-serif;
            font-weight: 700;
            font-size: 1.15em;
            color: var(--accent-gold);
            letter-spacing: 1.5px;
            text-transform: uppercase;
        }
        .ui-mockup-body { padding: 14px 16px; }
        .ui-row { margin-bottom: 12px; }
        .ui-row-label {
            font-size: 0.7em;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 1.5px;
            margin-bottom: 3px;
            font-weight: 700;
        }
        .ui-row-value {
            font-size: 0.95em;
            color: var(--text-light);
            font-weight: 700;
        }
        .ui-row-value.green { color: var(--green-active); }
        .ui-row-value.red { color: #c85a5a; }
        .ui-row-value.gold { color: var(--accent-gold); }

        .ui-power-row {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-top: 4px;
        }
        .ui-btn-sm {
            width: 28px; height: 28px;
            background: #3a3a34;
            color: var(--text-light);
            border: 1px solid #1f1f1c;
            font-weight: 700;
            font-size: 1.1em;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 2px;
            box-shadow: 0 1px 2px rgba(0,0,0,0.4) inset;
        }
        .ui-value-box {
            flex: 1;
            text-align: center;
            color: var(--text-light);
            font-weight: 700;
            font-size: 1em;
            padding: 4px 0;
        }
        .ui-progress-bg {
            background: #1a1a17;
            height: 10px;
            border-radius: 1px;
            overflow: hidden;
            margin-top: 4px;
            border: 1px solid #0d0d0b;
        }
        .ui-progress-fill {
            height: 100%;
            background: var(--green-active);
            width: 65%;
            box-shadow: 0 0 6px rgba(106, 190, 50, 0.5);
        }
        .ui-mockup-footer {
            display: flex;
            gap: 8px;
            padding: 12px 16px 14px;
            background: var(--bg-panel-dark);
            border-top: 1px solid #1f1f1c;
        }
        .ui-btn-buy {
            flex: 1;
            background: linear-gradient(180deg, #5e8a23 0%, #3d5a17 100%);
            color: #fff;
            text-align: center;
            padding: 10px 0;
            font-weight: 700;
            font-size: 0.95em;
            text-transform: uppercase;
            letter-spacing: 1px;
            border-radius: 2px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.4) inset, 0 1px 0 rgba(255,255,255,0.1);
        }
        .ui-btn-close {
            flex: 1;
            background: linear-gradient(180deg, #4a1f17 0%, #2d120c 100%);
            color: #c8c8c8;
            text-align: center;
            padding: 10px 0;
            font-weight: 700;
            font-size: 0.95em;
            text-transform: uppercase;
            letter-spacing: 1px;
            border-radius: 2px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.4) inset, 0 1px 0 rgba(255,255,255,0.05);
        }
        .ui-caption {
            text-align: center;
            color: var(--text-dim);
            font-size: 0.82em;
            font-style: italic;
            margin-top: 8px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .tip-block {
            background: rgba(0,0,0,0.25);
            border-left: 4px solid var(--accent-gold);
            padding: 12px 16px 12px 20px;
            margin-bottom: 12px;
            position: relative;
            overflow: hidden;
        }
        .tip-block::before {
            content: '';
            position: absolute;
            left: 0; top: 0; bottom: 0;
            width: 4px;
            background: repeating-linear-gradient(
                45deg,
                var(--accent-gold) 0,
                var(--accent-gold) 6px,
                #1a1a17 6px,
                #1a1a17 12px
            );
            opacity: 0.4;
        }
        .tip-block p { margin: 0; color: var(--text-light); }
        .tip-block strong { color: var(--accent-gold); }

        .tip-block.danger {
            border-left-color: var(--red-cancel);
            background: rgba(153, 51, 51, 0.12);
        }
        .tip-block.danger::before {
            background: repeating-linear-gradient(
                45deg,
                var(--red-cancel) 0,
                var(--red-cancel) 6px,
                #1a1a17 6px,
                #1a1a17 12px
            );
            opacity: 0.5;
        }
        .tip-block.danger strong { color: #d97a7a; }

        .tip-block.success {
            border-left-color: var(--green-active);
            background: rgba(106, 190, 50, 0.10);
        }
        .tip-block.success::before {
            background: repeating-linear-gradient(
                45deg,
                var(--green-active) 0,
                var(--green-active) 6px,
                #1a1a17 6px,
                #1a1a17 12px
            );
            opacity: 0.5;
        }
        .tip-block.success strong { color: #8fdb4f; }

        .tip-block.info {
            border-left-color: var(--accent-rust-br);
            background: rgba(205, 106, 44, 0.10);
        }
        .tip-block.info::before {
            background: repeating-linear-gradient(
                45deg,
                var(--accent-rust-br) 0,
                var(--accent-rust-br) 6px,
                #1a1a17 6px,
                #1a1a17 12px
            );
            opacity: 0.5;
        }
        .tip-block.info strong { color: var(--accent-rust-br); }

        .badge-new {
            display: inline-block;
            background: var(--green-active);
            color: #1a1a17;
            font-size: 0.65em;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
            padding: 2px 7px;
            border-radius: 2px;
            vertical-align: middle;
            margin-left: 8px;
            box-shadow: 0 0 8px rgba(106, 190, 50, 0.4);
            font-family: 'Oswald', sans-serif;
        }

        .actions-table {
            width: 100%;
            border-collapse: collapse;
            margin: 16px 0;
            font-size: 0.93em;
        }
        .actions-table th {
            background: var(--bg-panel-dark);
            color: var(--accent-gold);
            font-family: 'Oswald', sans-serif;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-size: 0.85em;
            font-weight: 600;
            padding: 10px 12px;
            text-align: left;
            border-bottom: 2px solid var(--accent-rust);
        }
        .actions-table td {
            padding: 10px 12px;
            border-bottom: 1px solid rgba(139, 96, 43, 0.18);
            color: var(--text-light);
        }
        .actions-table tr:last-child td { border-bottom: none; }
        .actions-table tr:nth-child(even) td { background: rgba(0,0,0,0.15); }
        .yes { color: var(--green-active); font-weight: 700; }
        .no  { color: #d97a7a; font-weight: 700; }

        .faq-item {
            background: rgba(0,0,0,0.2);
            padding: 14px 18px;
            margin-bottom: 10px;
            border-left: 3px solid var(--accent-rust);
        }
        .faq-q {
            color: var(--accent-gold);
            font-weight: 700;
            margin: 0 0 6px;
            font-family: 'Oswald', sans-serif;
            font-size: 1.02em;
            letter-spacing: 0.5px;
        }
        .faq-q::before { content: '› '; color: var(--accent-rust-br); }
        .faq-a { margin: 0; color: var(--text-light); font-size: 0.95em; }

        .whats-new {
            background: linear-gradient(135deg, rgba(106, 190, 50, 0.10) 0%, rgba(139, 96, 43, 0.10) 100%);
            border: 1px solid rgba(106, 190, 50, 0.3);
            padding: 18px 22px;
            margin: 24px 0;
            border-radius: 2px;
        }
        .whats-new h3 {
            margin: 0 0 12px;
            color: var(--green-active);
            font-family: 'Oswald', sans-serif;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            font-size: 1.1em;
        }
        .whats-new ul { margin: 0; }
        .whats-new li { font-size: 0.93em; padding: 3px 0 3px 26px; }

        .doc-footer {
            background: var(--bg-panel-dark);
            background-image: linear-gradient(180deg, #1f1f1c 0%, #181816 100%);
            border-top: 3px solid var(--accent-rust);
            padding: 22px 40px;
            text-align: center;
            color: var(--text-muted);
            font-size: 0.88em;
            letter-spacing: 1px;
            position: relative;
        }
        .doc-footer::before {
            content: '';
            position: absolute;
            top: -3px; left: 0; right: 0;
            height: 1px;
            background: linear-gradient(90deg, transparent, var(--accent-gold) 50%, transparent);
            opacity: 0.5;
        }
        .doc-footer .credits {
            color: var(--accent-gold);
            font-family: 'Oswald', sans-serif;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            font-size: 1em;
        }
        .doc-footer .sub {
            margin-top: 4px;
            color: var(--text-dim);
            font-size: 0.85em;
        }

        .download-btn {
            display: block;
            width: 220px;
            margin: 24px auto 40px;
            padding: 14px;
            background: linear-gradient(180deg, var(--accent-rust-br) 0%, #8a4520 100%);
            color: #1a1a17;
            text-align: center;
            text-decoration: none;
            font-family: 'Oswald', sans-serif;
            font-weight: 700;
            font-size: 1em;
            text-transform: uppercase;
            letter-spacing: 2px;
            border: 2px solid #1f1f1c;
            cursor: pointer;
            box-shadow:
                0 0 0 1px #6a3a18 inset,
                0 4px 12px rgba(0,0,0,0.6),
                0 0 16px rgba(205, 106, 44, 0.3);
            transition: all 0.15s ease;
        }
        .download-btn:hover {
            background: linear-gradient(180deg, #e07a35 0%, #9a5025 100%);
            box-shadow:
                0 0 0 1px #8a4520 inset,
                0 4px 16px rgba(0,0,0,0.7),
                0 0 24px rgba(205, 106, 44, 0.5);
            transform: translateY(-1px);
        }
        .download-btn:active { transform: translateY(1px); }

        @media print {
            .download-btn { display: none; }
            body { background: #fff; padding: 0; }
            #document { box-shadow: none; margin: 0; max-width: 100%; }
        }

        @media (max-width: 640px) {
            .doc-header { padding: 24px 20px; }
            .doc-title { font-size: 1.8em; }
            .doc-meta { position: static; text-align: left; margin-top: 8px; }
            .doc-body { padding: 24px 20px; }
            h2 { font-size: 1.3em; }
            .feature-strip { grid-template-columns: repeat(2, 1fr); }
        }
    </style>
</head>
<body>

<div id="document">
    <span class="corner-bl"></span>
    <span class="corner-br"></span>

    <div class="doc-header">
        <div class="doc-meta">
            VERSION <span class="ver">1.9.3</span><br>
            Oxide / uMod / Carbon
        </div>
        <h1 class="doc-title"><span class="bolt">⚡</span>StreetPower</h1>
        <div class="doc-subtitle">Player Guide</div>
    </div>

    <div class="feature-strip">
        <div class="feat"><span class="ico">⚡</span><strong>Buy Power</strong>at any street light</div>
        <div class="feat"><span class="ico">🛡️</span><strong>Indestructible</strong>generators</div>
        <div class="feat"><span class="ico">💰</span><strong>Scrap</strong>or any currency</div>
        <div class="feat"><span class="ico">🔄</span><strong>Persists</strong>across restarts</div>
    </div>

    <div class="doc-body">

        <h2><span class="ico">▣</span>What is StreetPower?</h2>
        <p><strong>StreetPower</strong> turns the vanilla <strong>street light poles</strong> scattered across the map into public electrical power sources that any player can rent with <strong>Scrap</strong>. When you make a purchase, a small generator is activated at the base of the pole and you can run wires from it to power your own machines and equipment — exactly like you would with any regular generator.</p>
        <p>It is the ideal solution for running <strong>temporary auto turrets at monuments</strong>, lighting for events, improvised electrical setups out in the open, or simply when you don't want to carry fuel and a generator of your own. Stop farming low-grade, just pay Scrap and plug in.</p>

        <div class="whats-new">
            <h3>✦ What's new in this version</h3>
            <ul>
                <li><strong>Indestructible generator</strong> — weapons, explosives, axes and hammer can no longer damage it</li>
                <li><strong>Wire theft protection</strong> — no one but the owner, admins and team members can connect or disconnect wires</li>
                <li><strong>Automatic refund</strong> — if spawning fails (rare), your Scrap is returned to your inventory instantly</li>
                <li><strong>Survives restarts</strong> — generator and remaining time are restored after the server restarts</li>
                <li><strong>Compatible with all Rust versions</strong> — automatically detects the generator class (FuelGenerator / ElectricGenerator / IOEntity)</li>
            </ul>
        </div>

        <h2><span class="ico">⚙</span>How it works — Step by step</h2>

        <div class="step">
            <div class="step-num">1</div>
            <div class="step-content">
                <h3>Find a street light pole</h3>
                <p>Look for the tall power poles scattered along the roads and around monuments on the vanilla map. When you get close, you will see a <strong>yellow floating text</strong> above the pole showing the price in Scrap.</p>
            </div>
        </div>

        <div class="step">
            <div class="step-num">2</div>
            <div class="step-content">
                <h3>Look at the pole and press E</h3>
                <p>Walk up to it (within 4 meters), aim at the pole and press <strong>E</strong> (the use key). A window like the one shown below will open on your screen.</p>
            </div>
        </div>

        <div class="step">
            <div class="step-num">3</div>
            <div class="step-content">
                <h3>Configure power and duration</h3>
                <p>Use the <strong>+</strong> and <strong>−</strong> buttons in the window to pick how many <strong>Watts</strong> of power you want and for how many <strong>minutes</strong>. The price is calculated automatically: <em>(power × price per watt) + (duration × price per minute)</em>.</p>
            </div>
        </div>

        <div class="step">
            <div class="step-num">4</div>
            <div class="step-content">
                <h3>Click BUY</h3>
                <p>If you have enough Scrap in your inventory, click the <strong>BUY</strong> button. The Scrap is deducted instantly, the generator is activated and a "gift" effect flashes at the pole to confirm the purchase.</p>
            </div>
        </div>

        <div class="step">
            <div class="step-num">5</div>
            <div class="step-content">
                <h3>Connect your wires</h3>
                <p>Grab your <strong>Wire Tool</strong>. A small generator has spawned at the base of the pole. Run a wire from its output to your equipment (auto turret, lights, etc.) — it works exactly like any other power source in Rust.</p>
            </div>
        </div>

        <div class="step">
            <div class="step-num">6</div>
            <div class="step-content">
                <h3>Track the remaining time</h3>
                <p>Look at the pole again at any moment to see the progress bar and the time left. When the time hits zero, the generator is removed automatically and any connected equipment loses power.</p>
            </div>
        </div>

        <h2><span class="ico">▣</span>The in-game interface</h2>
        <p>This is the window that pops up when you press <strong>E</strong> on a pole. Memorize the fields to speed up your purchases:</p>

        <div class="ui-mockup">
            <div class="ui-mockup-accent"></div>
            <div class="ui-mockup-header">
                <div class="title">⚡ STREET POWER</div>
            </div>
            <div class="ui-mockup-body">
                <div class="ui-row">
                    <div class="ui-row-label">Status</div>
                    <div class="ui-row-value green">● ACTIVE</div>
                </div>
                <div class="ui-row">
                    <div class="ui-row-label">Price</div>
                    <div class="ui-row-value">230 SCRAP &nbsp;<span style="color: var(--text-muted); font-weight: 400;">(450 available)</span></div>
                </div>
                <div class="ui-row">
                    <div class="ui-row-label">Power</div>
                    <div class="ui-power-row">
                        <div class="ui-btn-sm">−</div>
                        <div class="ui-value-box">100 W</div>
                        <div class="ui-btn-sm">+</div>
                    </div>
                </div>
                <div class="ui-row">
                    <div class="ui-row-label">Duration</div>
                    <div class="ui-power-row">
                        <div class="ui-btn-sm">−</div>
                        <div class="ui-value-box">60 min</div>
                        <div class="ui-btn-sm">+</div>
                    </div>
                </div>
                <div class="ui-row">
                    <div class="ui-row-label">Time Remaining</div>
                    <div class="ui-row-value gold" style="text-align:right; margin-bottom: 4px;">39:12</div>
                    <div class="ui-progress-bg">
                        <div class="ui-progress-fill"></div>
                    </div>
                </div>
            </div>
            <div class="ui-mockup-footer">
                <div class="ui-btn-buy">⚡ BUY</div>
                <div class="ui-btn-close">CLOSE</div>
            </div>
        </div>
        <div class="ui-caption">StreetPower in-game window</div>

        <h2><span class="ico">🛡</span>Generator protection</h2>
        <p>The StreetPower generator is <strong>completely indestructible</strong>. No one can mess with your power — it only ends when the time runs out or when you cancel it yourself.</p>

        <table class="actions-table">
            <thead>
                <tr>
                    <th>Action by another player</th>
                    <th style="text-align:center;">Allowed?</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>Shoot the generator with weapons</td>
                    <td style="text-align:center;" class="no">BLOCKED</td>
                </tr>
                <tr>
                    <td>Hit the generator with an axe/pickaxe</td>
                    <td style="text-align:center;" class="no">BLOCKED</td>
                </tr>
                <tr>
                    <td>Blow it up with C4, satchels, rockets or grenades</td>
                    <td style="text-align:center;" class="no">BLOCKED</td>
                </tr>
                <tr>
                    <td>Pick up the generator with a hammer</td>
                    <td style="text-align:center;" class="no">BLOCKED</td>
                </tr>
                <tr>
                    <td>Connect a wire to another player's generator</td>
                    <td style="text-align:center;" class="no">BLOCKED</td>
                </tr>
                <tr>
                    <td>Disconnect another player's wire (right-click)</td>
                    <td style="text-align:center;" class="no">BLOCKED</td>
                </tr>
                <tr>
                    <td>You (owner) or a team member connecting wires</td>
                    <td style="text-align:center;" class="yes">ALLOWED</td>
                </tr>
                <tr>
                    <td>Admins managing the generator</td>
                    <td style="text-align:center;" class="yes">ALLOWED</td>
                </tr>
            </tbody>
        </table>

        <div class="tip-block success">
            <p><strong>Your power is safe.</strong> The generator is only removed in three situations: (1) when the purchased time runs out, (2) when you click <strong>END</strong> in the window yourself, or (3) when an admin runs the reset command. No one else can interfere.</p>
        </div>

        <h2><span class="ico">!</span>Important tips</h2>

        <div class="tip-block">
            <p><strong>The pole is shared.</strong> If another player has already purchased power at that pole, you will see the status as <strong>ACTIVE</strong> and won't be able to buy until their time expires. Find another pole nearby.</p>
        </div>
        <div class="tip-block">
            <p><strong>Power expires on its own.</strong> When the time runs out, the generator disappears and the wires lose power. Track it through the green floating text above the pole.</p>
        </div>
        <div class="tip-block">
            <p><strong>Your wires stay in place.</strong> If the power expires and you want to reactivate, just purchase again at the same pole. The existing connections will start working automatically.</p>
        </div>
        <div class="tip-block success">
            <p><strong>Survives restarts.</strong> If the server restarts while your power is active, the generator is recreated and the remaining time is restored automatically. You lose nothing.</p>
        </div>
        <div class="tip-block">
            <p><strong>Scrap in inventory.</strong> The charge happens the moment you click BUY. If you don't have enough Scrap, the button is greyed out and the purchase is blocked.</p>
        </div>
        <div class="tip-block">
            <p><strong>Teams can share.</strong> Members of your Rust team can also connect and disconnect wires on your generator. Useful when defending monuments as a group.</p>
        </div>
        <div class="tip-block info">
            <p><strong>Guaranteed refund.</strong> If for any technical reason the generator can't be spawned (extremely rare), the Scrap is automatically returned to your inventory with a chat message.</p>
        </div>
        <div class="tip-block danger">
            <p><strong>Don't try to steal power.</strong> If you attempt to connect a wire to another player's generator, the wire is destroyed in your hand and a warning message appears in chat.</p>
        </div>

        <h2><span class="ico">?</span>Frequently asked questions</h2>

        <div class="faq-item">
            <p class="faq-q">Can I use any pole on the map?</p>
            <p class="faq-a">Yes, any vanilla street light pole on the map works with StreetPower. Additional poles may also have been registered manually by the admin on custom maps.</p>
        </div>
        <div class="faq-item">
            <p class="faq-q">How many devices can I run?</p>
            <p class="faq-a">It depends on the power (Watts) you purchased. The power is split equally among all connected wires, just like a normal Rust generator. More devices = less power for each one.</p>
        </div>
        <div class="faq-item">
            <p class="faq-q">What happens if I die or leave the server?</p>
            <p class="faq-a">The power stays active at the pole normally until the time runs out. The generator does not disappear when you disconnect — it stays in the world, running.</p>
        </div>
        <div class="faq-item">
            <p class="faq-q">Can I buy power at multiple poles at the same time?</p>
            <p class="faq-a">Yes, each pole is independent. You can activate as many poles as you want, as long as there's no configured cooldown on the server.</p>
        </div>
        <div class="faq-item">
            <p class="faq-q">Can I end my power before the time runs out?</p>
            <p class="faq-a">Yes. Open the pole window (press E while looking at it) and click <strong>END</strong>. The generator is removed immediately. There is no Scrap refund.</p>
        </div>
        <div class="faq-item">
            <p class="faq-q">Can another player destroy my generator?</p>
            <p class="faq-a"><strong>No.</strong> The generator is indestructible — weapons, explosives, axes and hammer don't work on it. Only the owner, admins or the timer can remove it.</p>
        </div>
        <div class="faq-item">
            <p class="faq-q">What if there's an internal error when buying?</p>
            <p class="faq-a">If the generator can't be spawned for any technical reason, the system detects the failure and <strong>refunds all your Scrap</strong> automatically to your inventory, with a chat message explaining what happened.</p>
        </div>
        <div class="faq-item">
            <p class="faq-q">Why can't I interact with the pole?</p>
            <p class="faq-a">Make sure you are within 4 meters of the pole and aiming directly at it. If the server requires a special permission, talk to an administrator.</p>
        </div>

    </div>

    <div class="doc-footer">
        <div class="credits">⚡ StreetPower — by Tropury</div>
        <div class="sub">Version 1.9.3 &nbsp;·&nbsp; Compatible with Oxide / uMod / Carbon</div>
    </div>

</div>

<button class="download-btn" onclick="generatePDF()">⤓ Download PDF</button>

<script>
    function generatePDF() {
        const element = document.getElementById('document');
        const opt = {
            margin:       8,
            filename:     'StreetPower_Player_Guide.pdf',
            image:        { type: 'jpeg', quality: 0.98 },
            html2canvas:  { scale: 2, backgroundColor: '#0d0d0b' },
            jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' },
            pagebreak:    { mode: ['avoid-all', 'css', 'legacy'] }
        };

        const btn = document.querySelector('.download-btn');
        btn.style.display = 'none';

        html2pdf().set(opt).from(element).save().then(() => {
            btn.style.display = 'block';
        }).catch(() => {
            btn.style.display = 'block';
        });
    }
</script>

</body>
</html>
