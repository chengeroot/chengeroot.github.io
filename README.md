<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>⚛️ 反应堆 - 完整版</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; user-select: none; }
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@400;600;700&display=swap');
        body {
            background: #0a0e17; color: #b0e0ff;
            font-family: 'Rajdhani', sans-serif;
            min-height: 100vh; display: flex; justify-content: center; align-items: center;
            padding: 16px;
        }
        .game-container {
            max-width: 1000px; width: 100%;
            background: rgba(10, 18, 30, 0.92);
            border-radius: 32px; padding: 24px 20px 28px;
            border: 1px solid rgba(60, 180, 255, 0.08);
            box-shadow: 0 0 60px rgba(0, 180, 255, 0.06);
            position: relative;
        }
        .header {
            display: flex; justify-content: space-between; align-items: center;
            flex-wrap: wrap; gap: 8px 12px; margin-bottom: 12px;
        }
        .title-wrap { display: flex; align-items: center; gap: 12px; flex-wrap: wrap; }
        .title {
            font-family: 'Orbitron', monospace; font-weight: 900; font-size: 1.5rem;
            background: linear-gradient(135deg, #4dd0ff, #0091ea);
            -webkit-background-clip: text; -webkit-text-fill-color: transparent;
        }
        .title small { font-size: 0.6rem; opacity: 0.5; -webkit-text-fill-color: rgba(100, 200, 255, 0.5); }
        .help-btn {
            padding: 5px 14px; border-radius: 20px;
            border: 1px solid rgba(60, 180, 255, 0.2);
            background: rgba(0, 60, 120, 0.25);
            color: #7fc8ff; font-weight: 700; font-size: 0.7rem;
            cursor: pointer; transition: 0.2s;
            font-family: 'Rajdhani', sans-serif;
            letter-spacing: 0.5px;
        }
        .help-btn:hover {
            background: rgba(0, 80, 160, 0.35);
            border-color: #4dd0ff;
            box-shadow: 0 0 20px rgba(0, 180, 255, 0.15);
            transform: translateY(-1px);
        }
        .status-badge {
            display: flex; align-items: center; gap: 10px; font-size: 0.8rem;
            font-weight: 600; padding: 6px 16px; border-radius: 40px;
            background: rgba(0, 40, 80, 0.4); border: 1px solid rgba(60, 180, 255, 0.15);
        }
        .status-badge .dot {
            width: 10px; height: 10px; border-radius: 50%;
            animation: pulse-dot 1.4s ease-in-out infinite;
        }
        .dot.running { background: #00e676; box-shadow: 0 0 12px #00e676; }
        .dot.warning { background: #ffab00; box-shadow: 0 0 12px #ffab00; animation-duration: 0.8s; }
        .dot.danger { background: #ff1744; box-shadow: 0 0 12px #ff1744; animation-duration: 0.4s; }
        .dot.meltdown { background: #d50000; box-shadow: 0 0 12px #d50000; animation-duration: 0.2s; }
        .dot.victory { background: #ffea00; box-shadow: 0 0 12px #ffea00; }
        @keyframes pulse-dot {
            0%, 100% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.6); opacity: 0.4; }
        }
        .dashboard { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; margin-bottom: 12px; }
        .gauge {
            background: rgba(0, 30, 60, 0.4); border-radius: 14px;
            padding: 8px 10px 10px; border: 1px solid rgba(60, 180, 255, 0.06);
        }
        .gauge .label {
            font-size: 0.5rem; text-transform: uppercase; letter-spacing: 1px;
            color: rgba(100, 200, 255, 0.5);
        }
        .gauge .value {
            font-family: 'Orbitron', monospace; font-size: 1.4rem;
            font-weight: 700; line-height: 1.2;
        }
        .gauge .value.temp { color: #ff6d00; }
        .gauge .value.power { color: #69f0ae; }
        .gauge .value.money { color: #ffd740; }
        .gauge .value.time { color: #4dd0ff; }
        .gauge .sub { font-size: 0.5rem; color: rgba(100, 200, 255, 0.3); }
        .gauge .bar-track {
            width: 100%; height: 3px; background: rgba(255, 255, 255, 0.06);
            border-radius: 4px; margin-top: 4px; overflow: hidden;
        }
        .gauge .bar-fill { height: 100%; border-radius: 4px; transition: width 0.4s; }
        .bar-fill.temp-bar { background: linear-gradient(90deg, #00c853, #ffab00, #ff1744); }
        .bar-fill.power-bar { background: linear-gradient(90deg, #ffd740, #ff9100); }
        .param-panel {
            display: grid; grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
            gap: 4px; margin-bottom: 12px; background: rgba(0, 20, 40, 0.2);
            border-radius: 14px; padding: 6px 10px;
            border: 1px solid rgba(60, 180, 255, 0.04);
        }
        .param-item { text-align: center; }
        .param-item .p-label {
            font-size: 0.4rem; text-transform: uppercase; letter-spacing: 0.5px;
            color: rgba(100, 200, 255, 0.4);
        }
        .param-item .p-value {
            font-family: 'Orbitron', monospace; font-size: 0.8rem; font-weight: 700;
        }
        .param-item .p-value.good { color: #69f0ae; }
        .param-item .p-value.warn { color: #ffab00; }
        .param-item .p-value.danger { color: #ff1744; }
        .power-select {
            background: rgba(0, 20, 40, 0.35); border-radius: 16px;
            padding: 10px 14px; margin-bottom: 12px;
            border: 1px solid rgba(60, 180, 255, 0.08);
            display: flex; align-items: center; flex-wrap: wrap; gap: 10px 14px;
        }
        .power-select .ps-label {
            font-weight: 700; font-size: 0.75rem; color: #7fc8ff;
            letter-spacing: 0.5px;
        }
        .power-select .ps-btn {
            padding: 6px 20px; border-radius: 20px;
            border: 1px solid rgba(60, 180, 255, 0.15);
            background: rgba(0, 60, 120, 0.2);
            color: #7fc8ff; font-weight: 700; font-size: 0.75rem;
            cursor: pointer; transition: 0.2s; letter-spacing: 0.5px;
        }
        .power-select .ps-btn:hover { background: rgba(0, 80, 160, 0.3); }
        .power-select .ps-btn.active.main {
            background: rgba(0, 230, 118, 0.15);
            border-color: rgba(0, 230, 118, 0.4); color: #69f0ae;
            box-shadow: 0 0 20px rgba(0, 230, 118, 0.1);
        }
        .power-select .ps-btn.active.backup {
            background: rgba(255, 170, 0, 0.15);
            border-color: rgba(255, 170, 0, 0.4); color: #ffab00;
            box-shadow: 0 0 20px rgba(255, 170, 0, 0.1);
        }
        .power-select .ps-info {
            font-size: 0.65rem; color: rgba(160, 210, 255, 0.5);
            margin-left: auto;
        }
        .load-control {
            background: rgba(0, 20, 40, 0.3); border-radius: 16px;
            padding: 8px 14px; margin-bottom: 12px;
            border: 1px solid rgba(60, 180, 255, 0.06);
            display: flex; align-items: center; flex-wrap: wrap; gap: 10px 16px;
        }
        .load-control .load-label { font-weight: 600; font-size: 0.7rem; color: #7fc8ff; }
        .load-control .load-slider { flex: 1; min-width: 120px; accent-color: #4dd0ff; height: 4px; }
        .load-control .load-value {
            font-family: 'Orbitron', monospace; font-size: 0.9rem;
            color: #ffd740; min-width: 50px; text-align: center;
        }
        .load-control .load-unit { font-size: 0.6rem; color: rgba(160, 210, 255, 0.4); }
        .relief-control {
            background: rgba(0, 20, 40, 0.3); border-radius: 16px;
            padding: 8px 14px; margin-bottom: 12px;
            border: 1px solid rgba(60, 180, 255, 0.06);
            display: flex; align-items: center; flex-wrap: wrap; gap: 10px 16px;
        }
        .relief-control .relief-label {
            font-weight: 600; font-size: 0.7rem; color: #7fc8ff;
            display: flex; align-items: center; gap: 6px;
        }
        .relief-control .relief-btn {
            padding: 5px 18px; border-radius: 20px;
            border: 1px solid rgba(255, 170, 0, 0.25);
            background: rgba(255, 170, 0, 0.1);
            color: #ffab00; font-weight: 700; font-size: 0.7rem;
            cursor: pointer; transition: 0.2s; letter-spacing: 0.5px;
        }
        .relief-control .relief-btn:hover:not(:disabled) {
            background: rgba(255, 170, 0, 0.25);
            transform: translateY(-1px);
            box-shadow: 0 0 20px rgba(255, 170, 0, 0.15);
        }
        .relief-control .relief-btn:disabled { opacity: 0.35; cursor: not-allowed; }
        .relief-control .relief-status {
            font-family: 'Orbitron', monospace; font-size: 0.75rem;
            color: #69f0ae; min-width: 80px;
        }
        .relief-control .relief-status.warn { color: #ffab00; }
        .relief-control .relief-status.danger { color: #ff1744; }
        .meltdown-warning {
            background: rgba(200, 0, 0, 0.15);
            border: 1px solid rgba(255, 0, 0, 0.3);
            border-radius: 16px; padding: 8px 16px; margin-bottom: 12px;
            display: none; justify-content: space-between; align-items: center;
            flex-wrap: wrap; gap: 6px;
        }
        .meltdown-warning.active { display: flex; }
        .meltdown-warning .label {
            font-weight: 700; color: #ff1744; font-size: 0.9rem; letter-spacing: 1px;
        }
        .meltdown-warning .timer {
            font-family: 'Orbitron', monospace; font-size: 1.8rem;
            font-weight: 900; color: #ff1744;
            text-shadow: 0 0 20px rgba(255, 0, 0, 0.3);
        }
        .rod-single {
            background: rgba(0, 20, 40, 0.3); border-radius: 16px;
            padding: 10px 16px; margin-bottom: 12px;
            border: 1px solid rgba(60, 180, 255, 0.06);
            display: flex; align-items: center; flex-wrap: wrap; gap: 10px 16px;
        }
        .rod-single .rod-label {
            font-weight: 600; font-size: 0.8rem; color: #7fc8ff;
            display: flex; align-items: center; gap: 6px;
        }
        .rod-single .rod-slider { flex: 1; min-width: 120px; accent-color: #4dd0ff; height: 4px; }
        .rod-single .rod-slider:disabled {
            opacity: 0.4;
            cursor: not-allowed;
            filter: grayscale(0.5);
        }
        .rod-single .rod-value {
            font-family: 'Orbitron', monospace; font-size: 1rem;
            color: #ffd740; min-width: 50px; text-align: center;
        }
        .rod-single .rod-lock {
            padding: 4px 14px; border-radius: 20px;
            border: 1px solid rgba(60, 180, 255, 0.1);
            background: rgba(0, 60, 120, 0.2); color: #7fc8ff;
            font-size: 0.7rem; cursor: pointer; transition: 0.2s;
        }
        .rod-single .rod-lock.active {
            background: rgba(255, 50, 50, 0.15);
            border-color: rgba(255, 50, 50, 0.2); color: #ff6d6d;
        }
        .device-panel { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 12px; }
        .device-group {
            background: rgba(0, 20, 40, 0.3); border-radius: 14px;
            padding: 8px 10px; border: 1px solid rgba(60, 180, 255, 0.06);
        }
        .device-group h4 {
            font-size: 0.55rem; text-transform: uppercase; letter-spacing: 1px;
            color: rgba(160, 210, 255, 0.5); margin-bottom: 4px;
            display: flex; justify-content: space-between;
        }
        .device-row {
            display: flex; align-items: center; justify-content: space-between;
            padding: 3px 0; font-size: 0.6rem; gap: 4px; flex-wrap: wrap;
            border-bottom: 1px solid rgba(60, 180, 255, 0.03);
        }
        .device-row:last-child { border-bottom: none; }
        .device-row .dev-led {
            width: 8px; height: 8px; border-radius: 50%;
            display: inline-block; margin-right: 2px;
        }
        .dev-led.on { background: #69f0ae; box-shadow: 0 0 6px #69f0ae; }
        .dev-led.off { background: #555; }
        .dev-led.fault { background: #ff1744; box-shadow: 0 0 6px #ff1744; animation: blink 0.6s infinite; }
        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.3; }
        }
        .device-row .dev-btn {
            padding: 1px 8px; border-radius: 6px;
            border: 1px solid rgba(60, 180, 255, 0.08);
            background: rgba(0, 60, 120, 0.2); color: #7fc8ff;
            font-size: 0.5rem; cursor: pointer; transition: 0.2s;
        }
        .device-row .dev-btn:hover:not(:disabled) { background: rgba(0, 80, 160, 0.3); }
        .device-row .dev-btn:disabled { opacity: 0.3; cursor: not-allowed; }
        .device-row .dev-btn.repair.ready {
            border-color: rgba(0, 230, 118, 0.3); color: #69f0ae;
            background: rgba(0, 230, 118, 0.08);
        }
        .device-row .power-slider { flex: 1; min-width: 40px; accent-color: #4dd0ff; height: 3px; }
        .device-row .power-label {
            font-family: 'Orbitron', monospace; font-size: 0.55rem;
            color: #ffd740; min-width: 28px; text-align: center;
        }
        .device-row .cooldown-label {
            font-family: 'Orbitron', monospace; font-size: 0.55rem;
            color: #ff6d6d; min-width: 50px; text-align: center;
        }
        .device-row .src-btn {
            padding: 1px 6px; border-radius: 5px;
            border: 1px solid rgba(60, 180, 255, 0.1);
            background: rgba(0, 60, 120, 0.2);
            color: #7fc8ff; font-size: 0.5rem; font-weight: 700;
            cursor: pointer; transition: 0.2s; min-width: 28px;
        }
        .device-row .src-btn.main {
            background: rgba(0, 230, 118, 0.15);
            border-color: rgba(0, 230, 118, 0.3); color: #69f0ae;
        }
        .device-row .src-btn.backup {
            background: rgba(255, 170, 0, 0.15);
            border-color: rgba(255, 170, 0, 0.3); color: #ffab00;
        }
        .controls { display: flex; gap: 6px; justify-content: center; flex-wrap: wrap; margin: 4px 0 8px; }
        .ctrl-btn {
            padding: 6px 18px; border-radius: 30px; font-weight: 700; font-size: 0.7rem;
            border: 1px solid rgba(60, 180, 255, 0.12);
            background: rgba(0, 60, 120, 0.25); color: #7fc8ff;
            cursor: pointer; transition: 0.2s; text-transform: uppercase;
        }
        .ctrl-btn:hover:not(:disabled) { background: rgba(0, 80, 160, 0.3); transform: translateY(-2px); }
        .ctrl-btn:disabled { opacity: 0.3; cursor: not-allowed; }
        .ctrl-btn.primary { border-color: rgba(0, 180, 255, 0.2); color: #4dd0ff; }
        .ctrl-btn.danger { border-color: rgba(255, 50, 50, 0.15); color: #ff6d6d; }
        .ctrl-btn.warning { border-color: rgba(255, 170, 0, 0.15); color: #ffab00; }
        .message-area {
            margin-top: 8px; padding: 6px 12px;
            background: rgba(0, 20, 40, 0.3); border-radius: 12px;
            display: flex; justify-content: space-between; align-items: center;
            flex-wrap: wrap; gap: 4px; min-height: 32px;
        }
        .message-area .msg {
            font-size: 0.7rem; color: rgba(160, 210, 255, 0.7); flex: 1;
        }
        .message-area .msg.warn { color: #ffab00; }
        .message-area .msg.danger { color: #ff1744; }
        .message-area .msg.success { color: #69f0ae; }
        .message-area .msg.info { color: #4dd0ff; }

        /* ===== 说明书模态框 ===== */
        .modal-overlay {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(5, 8, 15, 0.85);
            backdrop-filter: blur(8px);
            display: none;
            justify-content: center; align-items: center;
            z-index: 1000; padding: 16px;
            animation: fadeIn 0.25s ease;
        }
        .modal-overlay.active { display: flex; }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        .modal {
            max-width: 720px; width: 100%;
            max-height: 85vh;
            background: rgba(10, 20, 35, 0.98);
            border-radius: 24px;
            border: 1px solid rgba(60, 180, 255, 0.15);
            box-shadow: 0 0 80px rgba(0, 180, 255, 0.15);
            display: flex; flex-direction: column;
            animation: slideUp 0.3s ease;
            overflow: hidden;
        }
        @keyframes slideUp {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        .modal-header {
            display: flex; justify-content: space-between; align-items: center;
            padding: 16px 22px;
            border-bottom: 1px solid rgba(60, 180, 255, 0.1);
            background: rgba(0, 30, 60, 0.4);
        }
        .modal-header h2 {
            font-family: 'Orbitron', monospace; font-size: 1.1rem;
            font-weight: 700; color: #4dd0ff;
            letter-spacing: 1.5px;
            display: flex; align-items: center; gap: 8px;
        }
        .modal-close {
            width: 32px; height: 32px;
            border-radius: 50%;
            border: 1px solid rgba(60, 180, 255, 0.15);
            background: rgba(0, 60, 120, 0.2);
            color: #7fc8ff; font-size: 1.1rem;
            cursor: pointer; transition: 0.2s;
            display: flex; align-items: center; justify-content: center;
            font-family: 'Rajdhani', sans-serif;
            font-weight: 700;
        }
        .modal-close:hover {
            background: rgba(255, 50, 50, 0.15);
            border-color: rgba(255, 50, 50, 0.3);
            color: #ff6d6d;
            transform: rotate(90deg);
        }
        .modal-body {
            padding: 20px 24px;
            overflow-y: auto;
            flex: 1;
            line-height: 1.7;
        }
        .modal-body::-webkit-scrollbar { width: 6px; }
        .modal-body::-webkit-scrollbar-track { background: rgba(0, 20, 40, 0.3); }
        .modal-body::-webkit-scrollbar-thumb {
            background: rgba(60, 180, 255, 0.25);
            border-radius: 3px;
        }
        .modal-body::-webkit-scrollbar-thumb:hover {
            background: rgba(60, 180, 255, 0.4);
        }

        .help-section {
            margin-bottom: 22px;
        }
        .help-section:last-child { margin-bottom: 0; }
        .help-section h3 {
            font-family: 'Orbitron', monospace;
            font-size: 0.85rem;
            font-weight: 700;
            color: #4dd0ff;
            letter-spacing: 1px;
            margin-bottom: 8px;
            padding-bottom: 6px;
            border-bottom: 1px solid rgba(60, 180, 255, 0.1);
            display: flex; align-items: center; gap: 8px;
        }
        .help-section p {
            font-size: 0.85rem;
            color: rgba(180, 220, 255, 0.85);
            margin-bottom: 6px;
        }
        .help-section ul {
            list-style: none;
            padding-left: 4px;
        }
        .help-section ul li {
            font-size: 0.82rem;
            color: rgba(180, 220, 255, 0.8);
            padding: 3px 0 3px 20px;
            position: relative;
        }
        .help-section ul li::before {
            content: '▸';
            position: absolute;
            left: 4px;
            color: #4dd0ff;
            font-weight: 700;
        }
        .help-section .key {
            display: inline-block;
            padding: 1px 8px;
            background: rgba(0, 60, 120, 0.4);
            border: 1px solid rgba(60, 180, 255, 0.2);
            border-radius: 4px;
            font-family: 'Orbitron', monospace;
            font-size: 0.7rem;
            color: #4dd0ff;
            margin: 0 2px;
        }
        .help-section .highlight {
            color: #ffd740;
            font-weight: 700;
        }
        .help-section .danger-text {
            color: #ff6d6d;
            font-weight: 700;
        }
        .help-section .good-text {
            color: #69f0ae;
            font-weight: 700;
        }
        .help-section .warn-text {
            color: #ffab00;
            font-weight: 700;
        }

        @media (max-width: 800px) {
            .dashboard { grid-template-columns: repeat(2, 1fr); }
            .device-panel { grid-template-columns: 1fr; }
        }
        @media (max-width: 500px) {
            .dashboard { grid-template-columns: 1fr 1fr; gap: 4px; }
            .gauge .value { font-size: 1rem; }
            .param-panel { grid-template-columns: repeat(2, 1fr); }
            .meltdown-warning .timer { font-size: 1.2rem; }
            .modal-body { padding: 16px; }
            .modal-header { padding: 12px 16px; }
            .modal-header h2 { font-size: 0.9rem; }
        }
    </style>
</head>
<body>

    <div class="game-container" id="app">
        <div class="header">
            <div class="title-wrap">
                <div class="title">⚛️ 反应堆 <small>完整版</small></div>
                <button class="help-btn" id="btnHelp">📖 玩法说明</button>
            </div>
            <div class="status-badge">
                <span class="dot running" id="statusDot"></span>
                <span id="statusText">运行中</span>
            </div>
        </div>

        <div class="dashboard">
            <div class="gauge">
                <div class="label">🌡️ 核心温度</div>
                <div class="value temp" id="tempDisplay">500</div>
                <div class="sub">°C</div>
                <div class="bar-track"><div class="bar-fill temp-bar" id="tempBar" style="width:33%"></div></div>
            </div>
            <div class="gauge">
                <div class="label">⚡ 电功率</div>
                <div class="value power" id="powerDisplay">0.0</div>
                <div class="sub">MW</div>
                <div class="bar-track"><div class="bar-fill power-bar" id="powerBar" style="width:0%"></div></div>
            </div>
            <div class="gauge">
                <div class="label">💰 累计收益</div>
                <div class="value money" id="moneyDisplay">0.00</div>
                <div class="sub">元 (0.5元/度) · 无上限</div>
            </div>
            <div class="gauge">
                <div class="label">⏱️ 运行时间</div>
                <div class="value time" id="timeDisplay">0s</div>
                <div class="sub">实时运行</div>
            </div>
        </div>

        <div class="param-panel">
            <div class="param-item"><div class="p-label">热功率</div><div class="p-value" id="thermalPower">0.0 MW</div></div>
            <div class="param-item"><div class="p-label">反应堆压力</div><div class="p-value" id="reactorPressure">8.0 MPa</div></div>
            <div class="param-item"><div class="p-label">主电源</div><div class="p-value" id="mainPower">0.0 MW</div></div>
            <div class="param-item"><div class="p-label">备用电源</div><div class="p-value" id="backupPower">100%</div></div>
            <div class="param-item"><div class="p-label">反应堆稳定性</div><div class="p-value" id="reactorStability">100%</div></div>
            <div class="param-item"><div class="p-label">中子通量</div><div class="p-value" id="neutronFlux">50%</div></div>
            <div class="param-item"><div class="p-label">电网频率偏差</div><div class="p-value" id="gridDeviation">0.00%</div></div>
        </div>

        <div class="power-select">
            <span class="ps-label">⚡ 当前供电来源</span>
            <button class="ps-btn main active" id="psMain">主电源</button>
            <button class="ps-btn backup" id="psBackup">备用电源</button>
            <span class="ps-info" id="psInfo">机组分配：主 0MW / 备 0MW</span>
        </div>

        <div class="load-control">
            <span class="load-label">🎚️ 电网目标负荷</span>
            <input type="range" class="load-slider" id="loadSlider" min="0" max="200" value="50" step="1">
            <span class="load-value" id="loadValue">50</span>
            <span class="load-unit">MW</span>
        </div>

        <div class="relief-control">
            <span class="relief-label">💨 泄压阀</span>
            <button class="relief-btn" id="reliefBtn">立即泄压</button>
            <span class="relief-status" id="reliefStatus">就绪</span>
            <span style="font-size:0.55rem;color:rgba(160,210,255,0.3);">降压力1.5MPa / 降温8°C</span>
        </div>

        <div class="meltdown-warning" id="meltdownWarning">
            <span class="label">☢️ 核融倒计时</span>
            <span class="timer" id="meltdownTimer">60</span>
            <span style="font-size:0.8rem;color:#ff1744;">秒</span>
        </div>

        <div class="rod-single">
            <span class="rod-label">🛑 控制棒 (512根)</span>
            <input type="range" class="rod-slider" id="rodSlider" min="0" max="100" value="30">
            <span class="rod-value" id="rodValue">30%</span>
            <button class="rod-lock" id="rodLock">🔓 解锁</button>
        </div>

        <div class="device-panel">
            <div class="device-group">
                <h4>💧 冷却水泵 <span id="pumpSummary">0/4 运行</span></h4>
                <div id="pumpContainer"></div>
            </div>
            <div class="device-group">
                <h4>⚡ 发电机+变压器 <span id="genSummary">0/4 正常</span></h4>
                <div id="genContainer"></div>
                <div style="margin-top:4px;font-size:0.55rem;color:rgba(160,210,255,0.4);">
                    停电状态: <span id="outageDisplay">无</span> &nbsp;|&nbsp; 水泵供电: <span id="pumpPowerStatus">正常</span>
                </div>
            </div>
        </div>

        <div class="controls">
            <button class="ctrl-btn primary" id="btnPause">⏸ 暂停</button>
            <button class="ctrl-btn warning" id="btnScram">🛑 紧急停堆</button>
            <button class="ctrl-btn danger" id="btnReset">⟲ 重启</button>
        </div>

        <div class="message-area">
            <span class="msg info" id="message">控制棒默认30%，水泵满功率可降温！</span>
        </div>
    </div>

    <!-- ===== 玩法说明书模态框 ===== -->
    <div class="modal-overlay" id="helpModal">
        <div class="modal">
            <div class="modal-header">
                <h2>📖 玩法说明书 <span style="font-size:0.6rem;opacity:0.5;font-weight:400;">(游戏已暂停)</span></h2>
                <button class="modal-close" id="btnCloseHelp">✕</button>
            </div>
            <div class="modal-body">
                <div class="help-section">
                    <h3>🎯 游戏目标</h3>
                    <p>管理一座核反应堆，保持核心温度稳定，持续发电赚钱。收益<span class="highlight">无上限</span>，游戏可以一直进行下去。</p>
                    <p>唯一的失败条件是：若核心温度达到 <span class="danger-text">1500°C</span>，将触发核融倒计时 <span class="danger-text">60秒</span>。倒计时结束前未降温则爆炸，游戏结束。</p>
                </div>

                <div class="help-section">
                    <h3>🛑 控制棒</h3>
                    <ul>
                        <li>全部512根联动，深度0%-100%，默认 <span class="highlight">30%</span></li>
                        <li><span class="good-text">插入越深</span> → 反应性越低 → 升温越慢，但发电效率也下降</li>
                        <li>调节后以 <span class="highlight">10%/秒</span> 速度平滑移动</li>
                        <li>点击「锁定」可固定当前深度，防止误操作（锁定时滑块禁用）</li>
                        <li>紧急停堆按钮会让所有控制棒立刻插入100%</li>
                    </ul>
                </div>

                <div class="help-section">
                    <h3>💧 冷却水泵 (4台)</h3>
                    <ul>
                        <li>每台有独立的 <span class="highlight">0-100%</span> 功率滑块</li>
                        <li>功率越高，冷却效果越强，但耗电也越大（满功率每台耗4MW）</li>
                        <li>可以通过「开/关」按钮单独启停</li>
                        <li>只要有一台运行时温度>800°C，可能随机故障</li>
                    </ul>
                </div>

                <div class="help-section">
                    <h3>⚡ 发电机 + 变压器 (4台)</h3>
                    <ul>
                        <li>每台有独立的 <span class="highlight">0-100%</span> 功率滑块，满功率最大50MW</li>
                        <li>每台可分别指定电力输送到 <span class="good-text">主电源</span> 或 <span class="warn-text">备用电源</span>（行内的「主」「备」按钮）</li>
                        <li>全部损坏会引发 <span class="danger-text">全厂停电</span>，水泵自动停运</li>
                    </ul>
                </div>

                <div class="help-section">
                    <h3>🔌 主/备用电源选择</h3>
                    <ul>
                        <li>顶部按钮切换当前使用哪个电源驱动水泵</li>
                        <li><span class="good-text">主电源</span>：实时电力，必须 ≥ 水泵总需求（否则冷却失效）</li>
                        <li><span class="warn-text">备用电源</span>：容量0-100%，使用时放电，不用时由备用侧机组充电</li>
                    </ul>
                </div>

                <div class="help-section">
                    <h3>📊 电网调频</h3>
                    <ul>
                        <li>用「电网目标负荷」滑块设定需求（0-200MW）</li>
                        <li>偏差 = (实际发电 - 目标负荷) 累积计算</li>
                        <li>当<span class="good-text">偏差为正</span>（频率偏高）→ 请<span class="highlight">降低</span>发电功率</li>
                        <li>当<span class="warn-text">偏差为负</span>（频率偏低）→ 请<span class="highlight">增加</span>发电功率</li>
                        <li>偏差超过 ±8 时可能随机损坏机组（12秒冷却后才能修复）</li>
                        <li>电网自带稳定机制，偏差越大会自动回拉</li>
                    </ul>
                </div>

                <div class="help-section">
                    <h3>💨 泄压阀</h3>
                    <ul>
                        <li>点击「立即泄压」可降低反应堆压力 <span class="good-text">1.5 MPa</span> 并降温 <span class="good-text">8°C</span></li>
                        <li>每次使用后有 <span class="highlight">3秒</span> 冷却</li>
                        <li>反应堆压力随温度升高，超过 <span class="danger-text">15 MPa</span> 会导致水泵随机故障</li>
                    </ul>
                </div>

                <div class="help-section">
                    <h3>🔧 随机故障与修复</h3>
                    <ul>
                        <li>设备<span class="danger-text">不会手动损坏</span>，只会因高温、高压、电网异常而<span class="warn-text">随机故障</span></li>
                        <li>故障后需要等 <span class="highlight">12秒</span> 冷却才能点击「修复」</li>
                        <li>冷却期间按钮显示 <span class="danger-text">🔧 倒计时</span></li>
                    </ul>
                </div>

                <div class="help-section">
                    <h3>💰 收益计算</h3>
                    <ul>
                        <li>按发电量计算：<span class="highlight">0.5元/度</span>（1度 = 1kWh）</li>
                        <li>每MW持续1秒 = 约 0.14元</li>
                        <li><span class="good-text">收益无上限</span>，可以一直经营下去，看你能赚多少！</li>
                    </ul>
                </div>

                <div class="help-section">
                    <h3>⌨️ 快捷键</h3>
                    <ul>
                        <li><span class="key">空格</span> / <span class="key">P</span> — 暂停 / 继续</li>
                        <li><span class="key">R</span> — 重启游戏</li>
                        <li><span class="key">S</span> — 紧急停堆</li>
                        <li><span class="key">V</span> — 泄压</li>
                        <li><span class="key">Esc</span> — 关闭说明书</li>
                    </ul>
                </div>

                <div class="help-section">
                    <h3>💡 推荐开局</h3>
                    <ul>
                        <li>控制棒保持30%，4台水泵100%功率</li>
                        <li>4台机组各50%功率并都分配给主电源（总发电100MW）</li>
                        <li>电网负荷设为100MW，偏差就会稳定在0附近</li>
                        <li>想赚更多钱：提高控制棒深度提升反应性，同时增强冷却来平衡</li>
                    </ul>
                </div>
            </div>
        </div>
    </div>

    <script>
        (() => {
            'use strict';

            // ============ 常量 ============
            const MAX_TEMP = 1500;
            const MELTDOWN_TIME = 60;
            const BASE_HEAT_RATE = 4.5;
            const ROD_SPEED = 10;
            const PRICE_PER_KWH = 0.5;
            const UPDATE_INTERVAL = 1000;
            const MAX_PUMP_POWER = 1.23;
            const TEMP_MIN = 0;
            const TEMP_MAX = 2000;
            const REPAIR_COOLDOWN = 12;
            const RELIEF_COOLDOWN = 3;
            const GEN_MAX_MW = 50;
            const PUMP_MAX_MW = 4;
            const BACKUP_CHARGE_RATE = 1 / 20;
            const PRESSURE_MIN = 4.0;
            const PRESSURE_MAX = 20.0;
            const ROD_DEFAULT = 30;

            const GRID_SENSITIVITY = 0.008;
            const GRID_RANDOM_RANGE = 0.1;
            const GRID_DEVIATION_LIMIT = 10;
            const GRID_DAMAGE_THRESHOLD = 8;
            const GRID_DAMAGE_CHANCE = 0.01;
            const GRID_SELF_STABILIZE = 0.08;

            let state = {
                time: 0,
                temperature: 500,
                reactorPressure: 8.0,
                electricPower: 0,
                thermalPower: 0,
                money: 0,
                totalEnergy: 0,
                rodDepth: ROD_DEFAULT,
                rodTarget: ROD_DEFAULT,
                rodLocked: false,
                pumps: [],
                generators: [],
                powerOutage: false,
                neutronFlux: 50,
                backupPower: 100,
                mainPowerBus: 0,
                powerSelect: 'main',
                gameOver: false,
                victory: false,
                paused: false,
                meltdownActive: false,
                meltdownCounter: MELTDOWN_TIME,
                updateTimer: null,
                rodAnimId: null,
                targetLoad: 50,
                gridDeviation: 0,
                reliefCooldown: 0,
            };

            let wasPausedBeforeHelp = false;

            const $ = id => document.getElementById(id);
            const tempDisplay = $('tempDisplay');
            const powerDisplay = $('powerDisplay');
            const moneyDisplay = $('moneyDisplay');
            const timeDisplay = $('timeDisplay');
            const tempBar = $('tempBar');
            const powerBar = $('powerBar');
            const statusDot = $('statusDot');
            const statusText = $('statusText');
            const messageEl = $('message');
            const btnPause = $('btnPause');
            const btnScram = $('btnScram');
            const btnReset = $('btnReset');
            const pumpContainer = $('pumpContainer');
            const genContainer = $('genContainer');
            const pumpSummary = $('pumpSummary');
            const genSummary = $('genSummary');
            const outageDisplay = $('outageDisplay');
            const pumpPowerStatus = $('pumpPowerStatus');
            const thermalPowerEl = $('thermalPower');
            const reactorPressureEl = $('reactorPressure');
            const mainPowerEl = $('mainPower');
            const backupPowerEl = $('backupPower');
            const reactorStabilityEl = $('reactorStability');
            const neutronFluxEl = $('neutronFlux');
            const gridDeviationEl = $('gridDeviation');
            const meltdownWarning = $('meltdownWarning');
            const meltdownTimer = $('meltdownTimer');
            const rodSlider = $('rodSlider');
            const rodValue = $('rodValue');
            const rodLock = $('rodLock');
            const loadSlider = $('loadSlider');
            const loadValue = $('loadValue');
            const reliefBtn = $('reliefBtn');
            const reliefStatus = $('reliefStatus');
            const psMain = $('psMain');
            const psBackup = $('psBackup');
            const psInfo = $('psInfo');
            const btnHelp = $('btnHelp');
            const helpModal = $('helpModal');
            const btnCloseHelp = $('btnCloseHelp');

            function clamp(v, min, max) { return Math.max(min, Math.min(max, v)); }
            function rand(min, max) { return Math.random() * (max - min) + min; }

            function updateGridDeviationDisplay() {
                gridDeviationEl.textContent = state.gridDeviation.toFixed(2) + '%';
                const abs = Math.abs(state.gridDeviation);
                if (abs < 1) {
                    gridDeviationEl.className = 'p-value good';
                } else if (abs < 3) {
                    gridDeviationEl.className = 'p-value warn';
                } else {
                    gridDeviationEl.className = 'p-value danger';
                }
            }

            function initDevices() {
                state.pumps = [];
                for (let i = 0; i < 4; i++) {
                    state.pumps.push({
                        on: true, fault: false, power: 100, repairCooldown: 0,
                    });
                }
                state.generators = [];
                for (let i = 0; i < 4; i++) {
                    state.generators.push({
                        on: true, damaged: false, power: 100, repairCooldown: 0,
                        powerTarget: 'main',
                    });
                }
                state.powerOutage = false;
                state.rodDepth = ROD_DEFAULT;
                state.rodTarget = ROD_DEFAULT;
                state.rodLocked = false;
                if (state.rodAnimId) {
                    cancelAnimationFrame(state.rodAnimId);
                    state.rodAnimId = null;
                }
                state.gridDeviation = 0;
                state.targetLoad = 50;
                state.reliefCooldown = 0;
                state.reactorPressure = 8.0;
                state.backupPower = 100;
                state.mainPowerBus = 0;
                state.powerSelect = 'main';
                loadSlider.value = 50;
                loadValue.textContent = '50';
                rodSlider.value = ROD_DEFAULT;
                rodValue.textContent = ROD_DEFAULT + '%';
                rodSlider.disabled = false;
            }

            function animateRod() {
                if (state.rodLocked) {
                    rodValue.textContent = Math.round(state.rodDepth) + '%';
                    rodSlider.value = Math.round(state.rodDepth);
                    state.rodAnimId = null;
                    return;
                }
                const diff = state.rodTarget - state.rodDepth;
                if (Math.abs(diff) < 0.3) {
                    state.rodDepth = state.rodTarget;
                    rodValue.textContent = Math.round(state.rodDepth) + '%';
                    rodSlider.value = Math.round(state.rodDepth);
                    state.rodAnimId = null;
                    return;
                }
                const step = (ROD_SPEED / 1000) * 16 * Math.sign(diff);
                let newVal = state.rodDepth + step;
                if (Math.abs(newVal - state.rodTarget) < 0.3) newVal = state.rodTarget;
                state.rodDepth = clamp(newVal, 0, 100);
                rodValue.textContent = Math.round(state.rodDepth) + '%';
                rodSlider.value = Math.round(state.rodDepth);
                state.rodAnimId = requestAnimationFrame(animateRod);
            }

            function setRodTarget(val) {
                if (state.rodLocked) {
                    rodSlider.value = Math.round(state.rodDepth);
                    rodValue.textContent = Math.round(state.rodDepth) + '%';
                    setMessage('控制棒已锁定', 'warn');
                    return;
                }
                val = clamp(val, 0, 100);
                state.rodTarget = val;
                if (!state.rodAnimId && Math.abs(state.rodDepth - val) > 0.3) {
                    state.rodAnimId = requestAnimationFrame(animateRod);
                } else if (Math.abs(state.rodDepth - val) < 0.3) {
                    state.rodDepth = val;
                    rodValue.textContent = Math.round(val) + '%';
                    rodSlider.value = Math.round(val);
                    if (state.rodAnimId) {
                        cancelAnimationFrame(state.rodAnimId);
                        state.rodAnimId = null;
                    }
                }
            }

            function scram() {
                if (state.gameOver) return;
                if (!state.rodLocked) {
                    state.rodTarget = 100;
                    if (!state.rodAnimId) {
                        state.rodAnimId = requestAnimationFrame(animateRod);
                    }
                    setMessage('🛑 紧急停堆！控制棒完全插入！', 'danger');
                } else {
                    setMessage('控制棒已锁定，无法紧急插入', 'warn');
                }
            }

            function doRelief() {
                if (state.gameOver || state.paused) return;
                if (state.reliefCooldown > 0) {
                    setMessage(`泄压阀冷却中，还需 ${Math.ceil(state.reliefCooldown)} 秒`, 'warn');
                    return;
                }
                state.reactorPressure = clamp(state.reactorPressure - 1.5, PRESSURE_MIN, PRESSURE_MAX);
                state.temperature = clamp(state.temperature - 8, TEMP_MIN, TEMP_MAX);
                state.reliefCooldown = RELIEF_COOLDOWN;
                setMessage('💨 泄压阀开启，反应堆压力与温度下降', 'info');
                updateReliefUI();
                updateUI();
            }

            function updateReliefUI() {
                if (state.reliefCooldown > 0) {
                    reliefBtn.disabled = true;
                    reliefStatus.textContent = `冷却 ${state.reliefCooldown.toFixed(1)}s`;
                    reliefStatus.className = 'relief-status warn';
                } else {
                    reliefBtn.disabled = false;
                    if (state.reactorPressure > 15.0) {
                        reliefStatus.textContent = '⚠️ 压力过高';
                        reliefStatus.className = 'relief-status danger';
                    } else if (state.reactorPressure > 12.0) {
                        reliefStatus.textContent = '压力偏高';
                        reliefStatus.className = 'relief-status warn';
                    } else {
                        reliefStatus.textContent = '就绪';
                        reliefStatus.className = 'relief-status';
                    }
                }
            }

            function renderDevices() {
                let html = '';
                state.pumps.forEach((p, idx) => {
                    const ledClass = p.fault ? 'fault' : (p.on ? 'on' : 'off');
                    let statusOrCooldown = '';
                    let repairBtn = '';
                    if (p.fault) {
                        if (p.repairCooldown > 0) {
                            statusOrCooldown = `<span class="cooldown-label">🔧 ${p.repairCooldown.toFixed(1)}s</span>`;
                            repairBtn = `<button class="dev-btn repair" disabled>修复</button>`;
                        } else {
                            statusOrCooldown = `<span style="font-size:0.5rem;color:#ff6d6d;">可修复</span>`;
                            repairBtn = `<button class="dev-btn repair ready" data-pump="${idx}" data-action="repair">修复</button>`;
                        }
                    } else {
                        statusOrCooldown = `<span style="font-size:0.5rem;">${p.on?'运行':'停机'}</span>`;
                        repairBtn = `<button class="dev-btn" disabled style="opacity:0.2;">修复</button>`;
                    }
                    html += `<div class="device-row">
                        <span>泵 ${idx+1} <span class="dev-led ${ledClass}"></span></span>
                        ${statusOrCooldown}
                        <input type="range" class="power-slider" min="0" max="100" value="${p.power}" data-pump="${idx}" data-action="power" ${p.fault?'disabled':''}>
                        <span class="power-label">${p.power}%</span>
                        <div>
                            <button class="dev-btn ${p.on?'':'success'}" data-pump="${idx}" data-action="toggle" ${p.fault?'disabled':''}>${p.on?'关':'开'}</button>
                            ${repairBtn}
                        </div>
                    </div>`;
                });
                pumpContainer.innerHTML = html;
                pumpContainer.querySelectorAll('[data-pump]').forEach(btn => {
                    const idx = parseInt(btn.dataset.pump);
                    const action = btn.dataset.action;
                    if (action === 'toggle') {
                        btn.addEventListener('click', () => {
                            if (state.powerOutage) { setMessage('停电中，无法操作水泵', 'warn'); return; }
                            if (state.pumps[idx].fault) { setMessage('水泵故障中', 'warn'); return; }
                            state.pumps[idx].on = !state.pumps[idx].on;
                            renderDevices();
                            updateUI();
                            setMessage(`水泵 ${idx+1} ${state.pumps[idx].on?'启动':'关闭'}`, 'info');
                        });
                    } else if (action === 'repair') {
                        btn.addEventListener('click', () => {
                            const pump = state.pumps[idx];
                            if (!pump.fault) return;
                            if (pump.repairCooldown > 0) {
                                setMessage(`修复冷却中，还需 ${pump.repairCooldown.toFixed(1)} 秒`, 'warn');
                                return;
                            }
                            pump.fault = false;
                            pump.on = true;
                            pump.repairCooldown = 0;
                            renderDevices();
                            updateUI();
                            setMessage(`水泵 ${idx+1} 已修复`, 'success');
                        });
                    }
                });
                pumpContainer.querySelectorAll('.power-slider').forEach(slider => {
                    slider.addEventListener('input', () => {
                        const idx = parseInt(slider.dataset.pump);
                        const val = parseInt(slider.value);
                        state.pumps[idx].power = clamp(val, 0, 100);
                        const label = slider.parentElement.querySelector('.power-label');
                        if (label) label.textContent = state.pumps[idx].power + '%';
                        updateUI();
                    });
                });

                let htmlG = '';
                state.generators.forEach((g, idx) => {
                    const ledClass = g.damaged ? 'fault' : (g.on ? 'on' : 'off');
                    let statusOrCooldown = '';
                    let repairBtn = '';
                    if (g.damaged) {
                        if (g.repairCooldown > 0) {
                            statusOrCooldown = `<span class="cooldown-label">🔧 ${g.repairCooldown.toFixed(1)}s</span>`;
                            repairBtn = `<button class="dev-btn repair" disabled>修复</button>`;
                        } else {
                            statusOrCooldown = `<span style="font-size:0.5rem;color:#ff6d6d;">可修复</span>`;
                            repairBtn = `<button class="dev-btn repair ready" data-gen="${idx}" data-action="repair">修复</button>`;
                        }
                    } else {
                        statusOrCooldown = `<span style="font-size:0.5rem;">${g.on?'运行':'停机'}</span>`;
                        repairBtn = `<button class="dev-btn" disabled style="opacity:0.2;">修复</button>`;
                    }
                    const srcMainClass = g.powerTarget === 'main' ? 'src-btn main' : 'src-btn';
                    const srcBackupClass = g.powerTarget === 'backup' ? 'src-btn backup' : 'src-btn';
                    const srcBtns = g.damaged ? '' :
                        `<button class="${srcMainClass}" data-gen="${idx}" data-action="srcMain" title="分配主电源">主</button>
                         <button class="${srcBackupClass}" data-gen="${idx}" data-action="srcBackup" title="分配备用电源">备</button>`;
                    htmlG += `<div class="device-row">
                        <span>机组 ${idx+1} <span class="dev-led ${ledClass}"></span></span>
                        ${statusOrCooldown}
                        ${srcBtns}
                        <input type="range" class="power-slider" min="0" max="100" value="${g.power}" data-gen="${idx}" data-action="power" ${g.damaged?'disabled':''}>
                        <span class="power-label">${g.power}%</span>
                        <div>
                            <button class="dev-btn ${g.on?'':'success'}" data-gen="${idx}" data-action="toggle" ${g.damaged?'disabled':''}>${g.on?'关':'开'}</button>
                            ${repairBtn}
                        </div>
                    </div>`;
                });
                genContainer.innerHTML = htmlG;
                genContainer.querySelectorAll('[data-gen]').forEach(btn => {
                    const idx = parseInt(btn.dataset.gen);
                    const action = btn.dataset.action;
                    const gen = state.generators[idx];
                    if (action === 'toggle') {
                        btn.addEventListener('click', () => {
                            if (state.powerOutage) { setMessage('停电中，无法操作发电机', 'warn'); return; }
                            if (gen.damaged) { setMessage('机组损坏，请修复', 'warn'); return; }
                            gen.on = !gen.on;
                            renderDevices();
                            updateUI();
                            setMessage(`机组 ${idx+1} ${gen.on?'启动':'关闭'}`, 'info');
                        });
                    } else if (action === 'repair') {
                        btn.addEventListener('click', () => {
                            if (!gen.damaged) return;
                            if (gen.repairCooldown > 0) {
                                setMessage(`修复冷却中，还需 ${gen.repairCooldown.toFixed(1)} 秒`, 'warn');
                                return;
                            }
                            gen.damaged = false;
                            gen.on = true;
                            gen.repairCooldown = 0;
                            renderDevices();
                            updateUI();
                            setMessage(`机组 ${idx+1} 已修复`, 'success');
                            checkOutage();
                        });
                    } else if (action === 'srcMain') {
                        btn.addEventListener('click', () => {
                            if (gen.damaged) return;
                            gen.powerTarget = 'main';
                            renderDevices();
                            updateUI();
                            setMessage(`机组 ${idx+1} 电力 → 主电源`, 'info');
                        });
                    } else if (action === 'srcBackup') {
                        btn.addEventListener('click', () => {
                            if (gen.damaged) return;
                            gen.powerTarget = 'backup';
                            renderDevices();
                            updateUI();
                            setMessage(`机组 ${idx+1} 电力 → 备用电源`, 'info');
                        });
                    }
                });
                genContainer.querySelectorAll('.power-slider').forEach(slider => {
                    slider.addEventListener('input', () => {
                        const idx = parseInt(slider.dataset.gen);
                        const val = parseInt(slider.value);
                        state.generators[idx].power = clamp(val, 0, 100);
                        const label = slider.parentElement.querySelector('.power-label');
                        if (label) label.textContent = state.generators[idx].power + '%';
                        updateUI();
                    });
                });
                updateDeviceStatus();
            }

            function checkOutage() {
                const allDamaged = state.generators.every(g => g.damaged);
                if (allDamaged) {
                    state.powerOutage = true;
                    state.pumps.forEach(p => { if (!p.fault) p.on = false; });
                    setMessage('⚡ 所有发电机损坏！全厂停电！', 'danger');
                } else {
                    state.powerOutage = false;
                }
                renderDevices();
                updateUI();
            }

            function updateDeviceStatus() {
                const pumpOn = state.pumps.filter(p => p.on && !p.fault).length;
                pumpSummary.textContent = `${pumpOn}/4 运行`;
                const genOk = state.generators.filter(g => !g.damaged).length;
                genSummary.textContent = `${genOk}/4 正常`;
                outageDisplay.textContent = state.powerOutage ? '是' : '无';
                outageDisplay.style.color = state.powerOutage ? '#ff1744' : '#69f0ae';
            }

            function setPowerSelect(sel) {
                state.powerSelect = sel;
                psMain.classList.toggle('active', sel === 'main');
                psBackup.classList.toggle('active', sel === 'backup');
                setMessage(sel === 'main' ? '⚡ 已切换至主电源供电' : '🔋 已切换至备用电源供电', 'info');
                updateUI();
            }

            function updatePhysics() {
                if (state.gameOver || state.paused) return;
                state.time += 1;

                state.pumps.forEach(p => {
                    if (p.repairCooldown > 0) p.repairCooldown = Math.max(0, p.repairCooldown - 1);
                });
                state.generators.forEach(g => {
                    if (g.repairCooldown > 0) g.repairCooldown = Math.max(0, g.repairCooldown - 1);
                });
                if (state.reliefCooldown > 0) {
                    state.reliefCooldown = Math.max(0, state.reliefCooldown - 1);
                }

                let coreHeat = BASE_HEAT_RATE * 1.5;
                const rodFactor = 1 - (state.rodDepth / 100) * 0.9;
                coreHeat *= rodFactor;
                coreHeat = Math.max(coreHeat, 0.1);

                let totalPumpPower = 0;
                for (const pump of state.pumps) {
                    if (pump.on && !pump.fault) {
                        totalPumpPower += pump.power / 100;
                    }
                }
                const pumpDemand = totalPumpPower * PUMP_MAX_MW;

                let pumpPowered = true;
                if (state.powerSelect === 'main') {
                    if (state.mainPowerBus < pumpDemand) pumpPowered = false;
                } else {
                    if (state.backupPower <= 0) pumpPowered = false;
                }

                if (pumpPowered) {
                    const pumpCool = totalPumpPower * MAX_PUMP_POWER;
                    coreHeat -= pumpCool;
                } else {
                    if (state.temperature > 600 && Math.random() < 0.05) {
                        setMessage('⚠️ 水泵供电不足！冷却失效！', 'danger');
                    }
                }

                coreHeat += (state.temperature - 500) * 0.02;
                state.temperature = clamp(state.temperature + coreHeat, TEMP_MIN, TEMP_MAX);

                const pressureBase = 8.0 + (state.temperature - 500) * 0.006;
                state.reactorPressure += (pressureBase - state.reactorPressure) * 0.05;
                state.reactorPressure = clamp(state.reactorPressure, PRESSURE_MIN, PRESSURE_MAX);

                state.thermalPower = (state.temperature / 500) * 150;
                state.thermalPower = clamp(state.thermalPower, 0, 400);

                let mainGenMW = 0, backupGenMW = 0;
                let totalGenPower = 0;
                for (const gen of state.generators) {
                    if (gen.on && !gen.damaged) {
                        const mw = (gen.power / 100) * GEN_MAX_MW;
                        totalGenPower += mw;
                        if (gen.powerTarget === 'main') mainGenMW += mw;
                        else backupGenMW += mw;
                    }
                }
                state.mainPowerBus = mainGenMW;

                if (state.powerSelect === 'backup' && pumpPowered && pumpDemand > 0) {
                    state.backupPower = clamp(state.backupPower - pumpDemand * BACKUP_CHARGE_RATE * 0.05, 0, 100);
                } else {
                    state.backupPower = clamp(state.backupPower + backupGenMW * BACKUP_CHARGE_RATE * 0.05, 0, 100);
                }

                state.electricPower = totalGenPower;
                if (state.powerOutage) state.electricPower = 0;

                const loadDiff = state.electricPower - state.targetLoad;
                const randomWalk = rand(-GRID_RANDOM_RANGE, GRID_RANDOM_RANGE);
                const selfStabilize = -state.gridDeviation * GRID_SELF_STABILIZE;
                state.gridDeviation += loadDiff * GRID_SENSITIVITY + randomWalk + selfStabilize;
                state.gridDeviation = clamp(state.gridDeviation, -GRID_DEVIATION_LIMIT, GRID_DEVIATION_LIMIT);

                const kWhThisSecond = state.electricPower * 1000 / 3600;
                state.totalEnergy += kWhThisSecond;
                state.money += kWhThisSecond * PRICE_PER_KWH / 1000;

                state.neutronFlux = clamp(20 + (state.temperature / 1500) * 60, 20, 100);

                if (state.reactorPressure > 15.0 && Math.random() < 0.03) {
                    const availPumps = state.pumps.filter(p => !p.fault);
                    if (availPumps.length > 0) {
                        const pump = availPumps[Math.floor(Math.random() * availPumps.length)];
                        pump.fault = true;
                        pump.on = false;
                        pump.repairCooldown = REPAIR_COOLDOWN;
                        setMessage(`⚠️ 反应堆压力过高导致水泵 ${state.pumps.indexOf(pump)+1} 故障！12秒后可修复`, 'danger');
                        renderDevices();
                    }
                }
                if (state.temperature > 900 && Math.random() < 0.02) {
                    const availGens = state.generators.filter(g => !g.damaged);
                    if (availGens.length > 0) {
                        const gen = availGens[Math.floor(Math.random() * availGens.length)];
                        gen.damaged = true;
                        gen.on = false;
                        gen.repairCooldown = REPAIR_COOLDOWN;
                        setMessage(`⚠️ 高温导致机组 ${state.generators.indexOf(gen)+1} 损坏！12秒后可修复`, 'danger');
                        checkOutage();
                    }
                }
                if (Math.abs(state.gridDeviation) > GRID_DAMAGE_THRESHOLD && Math.random() < GRID_DAMAGE_CHANCE) {
                    const availGens = state.generators.filter(g => !g.damaged);
                    if (availGens.length > 0) {
                        const gen = availGens[Math.floor(Math.random() * availGens.length)];
                        gen.damaged = true;
                        gen.on = false;
                        gen.repairCooldown = REPAIR_COOLDOWN;
                        setMessage(`⚠️ 电网频率异常导致机组 ${state.generators.indexOf(gen)+1} 损坏！12秒后可修复`, 'danger');
                        checkOutage();
                    }
                }
                if (!pumpPowered && Math.random() < 0.03) {
                    const availPumps = state.pumps.filter(p => !p.fault && p.on);
                    if (availPumps.length > 0) {
                        const pump = availPumps[Math.floor(Math.random() * availPumps.length)];
                        pump.fault = true;
                        pump.on = false;
                        pump.repairCooldown = REPAIR_COOLDOWN;
                        setMessage(`⚠️ 供电不足导致水泵 ${state.pumps.indexOf(pump)+1} 故障！12秒后可修复`, 'danger');
                        renderDevices();
                    }
                }

                if (state.temperature >= MAX_TEMP && !state.meltdownActive) {
                    state.meltdownActive = true;
                    state.meltdownCounter = MELTDOWN_TIME;
                    meltdownWarning.classList.add('active');
                    setMessage('☢️ 温度达到1500°C！核融倒计时60秒！', 'danger');
                }
                if (state.meltdownActive) {
                    state.meltdownCounter -= 1;
                    meltdownTimer.textContent = state.meltdownCounter;
                    if (state.meltdownCounter <= 0) {
                        state.gameOver = true;
                        state.victory = false;
                        if (state.updateTimer) {
                            clearInterval(state.updateTimer);
                            state.updateTimer = null;
                        }
                        setMessage('💥 核融爆炸！反应堆摧毁！', 'danger');
                        statusDot.className = 'dot meltdown';
                        statusText.textContent = '💥 爆炸';
                        meltdownWarning.classList.remove('active');
                    }
                }

                // ★ 移除收益胜利条件：不再有 state.money >= 100 的胜利判定

                renderDevices();
                updateUI();
                updateReliefUI();
            }

            function togglePause() {
                if (state.gameOver) return;
                state.paused = !state.paused;
                btnPause.textContent = state.paused ? '▶ 继续' : '⏸ 暂停';
                setMessage(state.paused ? '⏸ 已暂停' : '▶ 继续运行', 'info');
            }

            function resetGame() {
                if (state.updateTimer) {
                    clearInterval(state.updateTimer);
                    state.updateTimer = null;
                }
                state.time = 0;
                state.temperature = 500;
                state.reactorPressure = 8.0;
                state.electricPower = 0;
                state.thermalPower = 0;
                state.money = 0;
                state.totalEnergy = 0;
                state.gameOver = false;
                state.victory = false;
                state.paused = false;
                state.meltdownActive = false;
                state.meltdownCounter = MELTDOWN_TIME;
                state.gridDeviation = 0;
                state.targetLoad = 50;
                loadSlider.value = 50;
                loadValue.textContent = '50';
                meltdownWarning.classList.remove('active');
                btnPause.textContent = '⏸ 暂停';
                initDevices();
                rodSlider.value = ROD_DEFAULT;
                rodValue.textContent = ROD_DEFAULT + '%';
                rodLock.textContent = '🔓 解锁';
                rodLock.classList.remove('active');
                rodSlider.disabled = false;
                psMain.classList.add('active');
                psBackup.classList.remove('active');
                renderDevices();
                updateUI();
                updateReliefUI();
                setMessage('🔄 已重置（控制棒 30%）', 'info');
                statusDot.className = 'dot running';
                statusText.textContent = '运行中';
                state.updateTimer = setInterval(() => updatePhysics(), UPDATE_INTERVAL);
            }

            function updateUI() {
                const temp = state.temperature;
                const power = state.electricPower;
                const money = state.money;

                tempDisplay.textContent = Math.round(temp);
                powerDisplay.textContent = power.toFixed(1);
                moneyDisplay.textContent = money.toFixed(2);
                timeDisplay.textContent = Math.round(state.time) + 's';

                const tempPct = clamp((temp / MAX_TEMP) * 100, 0, 100);
                const powerPct = clamp((power / 200) * 100, 0, 100);
                tempBar.style.width = tempPct + '%';
                powerBar.style.width = powerPct + '%';

                thermalPowerEl.textContent = state.thermalPower.toFixed(1) + ' MW';

                reactorPressureEl.textContent = state.reactorPressure.toFixed(2) + ' MPa';
                if (state.reactorPressure > 15.0) {
                    reactorPressureEl.className = 'p-value danger';
                } else if (state.reactorPressure > 12.0) {
                    reactorPressureEl.className = 'p-value warn';
                } else {
                    reactorPressureEl.className = 'p-value good';
                }

                mainPowerEl.textContent = state.mainPowerBus.toFixed(1) + ' MW';
                mainPowerEl.className = 'p-value' + (state.mainPowerBus > 30 ? ' good' : (state.mainPowerBus > 10 ? ' warn' : ' danger'));

                backupPowerEl.textContent = Math.round(state.backupPower) + '%';
                if (state.backupPower > 50) {
                    backupPowerEl.className = 'p-value good';
                } else if (state.backupPower > 20) {
                    backupPowerEl.className = 'p-value warn';
                } else {
                    backupPowerEl.className = 'p-value danger';
                }

                const stab = 100 - Math.abs(state.temperature - 500) * 0.08;
                const stabVal = clamp(stab, 0, 100);
                reactorStabilityEl.textContent = Math.round(stabVal) + '%';
                reactorStabilityEl.className = 'p-value' + (stabVal > 60 ? ' good' : (stabVal > 30 ? ' warn' : ' danger'));

                neutronFluxEl.textContent = Math.round(state.neutronFlux) + '%';
                neutronFluxEl.className = 'p-value' + (state.neutronFlux > 80 ? ' danger' : (state.neutronFlux > 60 ? ' warn' : ' good'));

                updateGridDeviationDisplay();

                let mainMW = 0, backupMW = 0;
                for (const g of state.generators) {
                    if (g.on && !g.damaged) {
                        const mw = (g.power / 100) * GEN_MAX_MW;
                        if (g.powerTarget === 'main') mainMW += mw;
                        else backupMW += mw;
                    }
                }
                psInfo.textContent = `机组分配：主 ${mainMW.toFixed(0)}MW / 备 ${backupMW.toFixed(0)}MW`;

                const totalPumpPower = state.pumps.filter(p => p.on && !p.fault).reduce((s, p) => s + p.power / 100, 0);
                const pumpDemand = totalPumpPower * PUMP_MAX_MW;
                let pumpPowered = true;
                if (state.powerSelect === 'main') {
                    if (state.mainPowerBus < pumpDemand) pumpPowered = false;
                } else {
                    if (state.backupPower <= 0) pumpPowered = false;
                }
                if (pumpPowered) {
                    pumpPowerStatus.textContent = '正常';
                    pumpPowerStatus.style.color = '#69f0ae';
                } else {
                    pumpPowerStatus.textContent = '不足 ⚠️';
                    pumpPowerStatus.style.color = '#ff1744';
                }

                updateDeviceStatus();

                if (state.gameOver) {
                    if (state.victory) {
                        statusDot.className = 'dot victory';
                        statusText.textContent = '🏆 胜利';
                    } else {
                        statusDot.className = 'dot meltdown';
                        statusText.textContent = '💥 爆炸';
                    }
                } else if (state.temperature > 1200) {
                    statusDot.className = 'dot danger';
                    statusText.textContent = '⚠️ 危急';
                } else if (state.temperature > 900) {
                    statusDot.className = 'dot warning';
                    statusText.textContent = '⚠️ 高温';
                } else {
                    statusDot.className = 'dot running';
                    statusText.textContent = '✅ 稳定';
                }

                btnPause.disabled = state.gameOver;
                btnScram.disabled = state.gameOver;

                if (state.meltdownActive && !state.gameOver) {
                    meltdownWarning.classList.add('active');
                    meltdownTimer.textContent = state.meltdownCounter;
                } else {
                    meltdownWarning.classList.remove('active');
                }
            }

            function setMessage(text, type='info') {
                messageEl.textContent = text;
                messageEl.className = 'msg ' + type;
            }

            function handleKeydown(e) {
                if (e.key === ' ' || e.key === 'p') { e.preventDefault(); togglePause(); }
                if (e.key === 'r' || e.key === 'R') resetGame();
                if (e.key === 's' || e.key === 'S') scram();
                if (e.key === 'v' || e.key === 'V') doRelief();
                if (e.key === 'Escape') {
                    closeHelp();
                }
            }

            function openHelp() {
                wasPausedBeforeHelp = state.paused;
                if (!state.gameOver && !state.paused) {
                    state.paused = true;
                    btnPause.textContent = '▶ 继续';
                }
                helpModal.classList.add('active');
            }

            function closeHelp() {
                helpModal.classList.remove('active');
                if (!wasPausedBeforeHelp && !state.gameOver) {
                    state.paused = false;
                    btnPause.textContent = '⏸ 暂停';
                }
            }

            function init() {
                state.temperature = 500;
                initDevices();
                renderDevices();
                updateUI();
                updateReliefUI();
                setMessage('🟢 控制棒默认30%，收益无上限，尽情经营吧！', 'info');

                btnPause.addEventListener('click', togglePause);
                btnScram.addEventListener('click', scram);
                btnReset.addEventListener('click', resetGame);
                reliefBtn.addEventListener('click', doRelief);
                psMain.addEventListener('click', () => setPowerSelect('main'));
                psBackup.addEventListener('click', () => setPowerSelect('backup'));

                btnHelp.addEventListener('click', openHelp);
                btnCloseHelp.addEventListener('click', closeHelp);
                helpModal.addEventListener('click', (e) => {
                    if (e.target === helpModal) {
                        closeHelp();
                    }
                });

                rodSlider.addEventListener('input', () => {
                    setRodTarget(parseInt(rodSlider.value));
                });
                rodLock.addEventListener('click', () => {
                    state.rodLocked = !state.rodLocked;
                    rodLock.textContent = state.rodLocked ? '🔒 锁定' : '🔓 解锁';
                    rodLock.classList.toggle('active', state.rodLocked);
                    rodSlider.disabled = state.rodLocked;
                    if (state.rodLocked) {
                        if (state.rodAnimId) {
                            cancelAnimationFrame(state.rodAnimId);
                            state.rodAnimId = null;
                        }
                        rodSlider.value = Math.round(state.rodDepth);
                        rodValue.textContent = Math.round(state.rodDepth) + '%';
                        setMessage('控制棒已锁定', 'warn');
                    } else {
                        setMessage('控制棒已解锁', 'info');
                    }
                });

                loadSlider.addEventListener('input', () => {
                    const val = parseInt(loadSlider.value);
                    state.targetLoad = val;
                    loadValue.textContent = val;
                });

                document.addEventListener('keydown', handleKeydown);
                state.updateTimer = setInterval(() => updatePhysics(), UPDATE_INTERVAL);
            }

            init();
        })();
    </script>

</body>
</html>
