<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>⚛️ 反应堆综合体 - 实时核融模拟</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@400;600;700&display=swap');

        body {
            background: #0a0e17;
            color: #b0e0ff;
            font-family: 'Rajdhani', sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 16px;
        }

        .game-container {
            max-width: 1000px;
            width: 100%;
            background: rgba(10, 18, 30, 0.92);
            backdrop-filter: blur(8px);
            border-radius: 32px;
            padding: 24px 20px 28px;
            border: 1px solid rgba(60, 180, 255, 0.08);
            box-shadow: 0 0 60px rgba(0, 180, 255, 0.06);
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 8px 12px;
            margin-bottom: 12px;
        }
        .title {
            font-family: 'Orbitron', monospace;
            font-weight: 900;
            font-size: 1.5rem;
            background: linear-gradient(135deg, #4dd0ff, #0091ea);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .title small {
            font-size: 0.6rem;
            opacity: 0.5;
            -webkit-text-fill-color: rgba(100, 200, 255, 0.5);
        }
        .status-badge {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 0.8rem;
            font-weight: 600;
            padding: 6px 16px;
            border-radius: 40px;
            background: rgba(0, 40, 80, 0.4);
            border: 1px solid rgba(60, 180, 255, 0.15);
        }
        .status-badge .dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            animation: pulse-dot 1.4s ease-in-out infinite;
        }
        .dot.running {
            background: #00e676;
            box-shadow: 0 0 12px #00e676;
        }
        .dot.warning {
            background: #ffab00;
            box-shadow: 0 0 12px #ffab00;
            animation-duration: 0.8s;
        }
        .dot.danger {
            background: #ff1744;
            box-shadow: 0 0 12px #ff1744;
            animation-duration: 0.4s;
        }
        .dot.meltdown {
            background: #d50000;
            box-shadow: 0 0 12px #d50000;
            animation-duration: 0.2s;
        }
        .dot.victory {
            background: #ffea00;
            box-shadow: 0 0 12px #ffea00;
        }
        @keyframes pulse-dot {
            0%,
            100% {
                transform: scale(1);
                opacity: 1;
            }
            50% {
                transform: scale(1.6);
                opacity: 0.4;
            }
        }

        /* 仪表盘 */
        .dashboard {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
            margin-bottom: 12px;
        }
        .gauge {
            background: rgba(0, 30, 60, 0.4);
            border-radius: 14px;
            padding: 8px 10px 10px;
            border: 1px solid rgba(60, 180, 255, 0.06);
        }
        .gauge .label {
            font-size: 0.5rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: rgba(100, 200, 255, 0.5);
        }
        .gauge .value {
            font-family: 'Orbitron', monospace;
            font-size: 1.4rem;
            font-weight: 700;
            line-height: 1.2;
        }
        .gauge .value.temp {
            color: #ff6d00;
        }
        .gauge .value.energy {
            color: #ffd740;
        }
        .gauge .value.power {
            color: #69f0ae;
        }
        .gauge .value.money {
            color: #ffd740;
        }
        .gauge .sub {
            font-size: 0.5rem;
            color: rgba(100, 200, 255, 0.3);
        }
        .gauge .bar-track {
            width: 100%;
            height: 3px;
            background: rgba(255, 255, 255, 0.06);
            border-radius: 4px;
            margin-top: 4px;
            overflow: hidden;
        }
        .gauge .bar-fill {
            height: 100%;
            border-radius: 4px;
            transition: width 0.4s;
        }
        .bar-fill.temp-bar {
            background: linear-gradient(90deg, #00c853, #ffab00, #ff1744);
        }
        .bar-fill.power-bar {
            background: linear-gradient(90deg, #ffd740, #ff9100);
        }

        /* 额外参数 */
        .param-panel {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(90px, 1fr));
            gap: 4px;
            margin-bottom: 12px;
            background: rgba(0, 20, 40, 0.2);
            border-radius: 14px;
            padding: 6px 10px;
            border: 1px solid rgba(60, 180, 255, 0.04);
        }
        .param-item {
            text-align: center;
        }
        .param-item .p-label {
            font-size: 0.4rem;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            color: rgba(100, 200, 255, 0.4);
        }
        .param-item .p-value {
            font-family: 'Orbitron', monospace;
            font-size: 0.8rem;
            font-weight: 700;
        }
        .param-item .p-value.good {
            color: #69f0ae;
        }
        .param-item .p-value.warn {
            color: #ffab00;
        }
        .param-item .p-value.danger {
            color: #ff1744;
        }

        /* 核融倒计时 */
        .meltdown-warning {
            background: rgba(200, 0, 0, 0.15);
            border: 1px solid rgba(255, 0, 0, 0.3);
            border-radius: 16px;
            padding: 8px 16px;
            margin-bottom: 12px;
            display: none;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 6px;
        }
        .meltdown-warning.active {
            display: flex;
        }
        .meltdown-warning .label {
            font-weight: 700;
            color: #ff1744;
            font-size: 0.9rem;
            letter-spacing: 1px;
        }
        .meltdown-warning .timer {
            font-family: 'Orbitron', monospace;
            font-size: 1.8rem;
            font-weight: 900;
            color: #ff1744;
            text-shadow: 0 0 20px rgba(255, 0, 0, 0.3);
        }

        /* 控制棒分组 */
        .rod-group-panel {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 6px;
            margin-bottom: 12px;
            background: rgba(0, 20, 40, 0.3);
            border-radius: 16px;
            padding: 10px 12px;
            border: 1px solid rgba(60, 180, 255, 0.06);
        }
        .rod-group {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 2px;
        }
        .rod-group .rod-label {
            font-size: 0.55rem;
            color: rgba(160, 210, 255, 0.5);
        }
        .rod-group .rod-slider {
            width: 100%;
            accent-color: #4dd0ff;
            height: 4px;
        }
        .rod-group .rod-value {
            font-family: 'Orbitron', monospace;
            font-size: 0.7rem;
            color: #ffd740;
        }
        .rod-group .rod-lock {
            font-size: 0.45rem;
            padding: 0 6px;
            border-radius: 8px;
            border: 1px solid rgba(60, 180, 255, 0.1);
            background: rgba(0, 60, 120, 0.2);
            color: #7fc8ff;
            cursor: pointer;
        }
        .rod-group .rod-lock.active {
            background: rgba(255, 50, 50, 0.15);
            border-color: rgba(255, 50, 50, 0.2);
            color: #ff6d6d;
        }

        /* 设备面板 */
        .device-panel {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 12px;
        }
        .device-group {
            background: rgba(0, 20, 40, 0.3);
            border-radius: 14px;
            padding: 8px 10px;
            border: 1px solid rgba(60, 180, 255, 0.06);
        }
        .device-group h4 {
            font-size: 0.55rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: rgba(160, 210, 255, 0.5);
            margin-bottom: 4px;
            display: flex;
            justify-content: space-between;
        }
        .device-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 2px 0;
            font-size: 0.6rem;
        }
        .device-row .dev-led {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            display: inline-block;
            margin-right: 4px;
        }
        .dev-led.on {
            background: #69f0ae;
            box-shadow: 0 0 6px #69f0ae;
        }
        .dev-led.off {
            background: #555;
        }
        .dev-led.fault {
            background: #ff1744;
            box-shadow: 0 0 6px #ff1744;
        }
        .device-row .dev-btn {
            padding: 0 6px;
            border-radius: 6px;
            border: 1px solid rgba(60, 180, 255, 0.08);
            background: rgba(0, 60, 120, 0.2);
            color: #7fc8ff;
            font-size: 0.5rem;
            cursor: pointer;
            transition: 0.2s;
        }
        .device-row .dev-btn:hover {
            background: rgba(0, 80, 160, 0.3);
        }
        .device-row .dev-btn.danger {
            border-color: rgba(255, 50, 50, 0.15);
            color: #ff6d6d;
        }
        .device-row .dev-btn.danger:hover {
            background: rgba(255, 50, 50, 0.1);
        }
        .device-row .dev-btn.success {
            border-color: rgba(0, 230, 118, 0.15);
            color: #69f0ae;
        }

        /* 网格 */
        .grid-wrapper {
            margin: 8px auto 10px;
            aspect-ratio: 1 / 1;
            max-width: 400px;
            width: 100%;
            background: rgba(0, 20, 40, 0.5);
            border-radius: 20px;
            padding: 8px;
            border: 1px solid rgba(60, 180, 255, 0.06);
        }
        .grid {
            display: grid;
            gap: 4px;
            width: 100%;
            height: 100%;
            grid-template-columns: repeat(7, 1fr);
            grid-template-rows: repeat(7, 1fr);
        }
        .cell {
            background: rgba(10, 30, 50, 0.6);
            border-radius: 6px;
            border: 1px solid rgba(60, 180, 255, 0.06);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1rem;
            position: relative;
            aspect-ratio: 1 / 1;
            transition: 0.15s;
        }
        .cell:hover:not(.empty) {
            border-color: rgba(60, 180, 255, 0.3);
            transform: scale(1.04);
        }
        .cell.empty:hover {
            background: rgba(20, 60, 100, 0.3);
            transform: scale(1.02);
        }
        .cell .cell-temp {
            position: absolute;
            bottom: 2px;
            right: 4px;
            font-size: 0.35rem;
            color: rgba(255, 150, 50, 0.5);
            font-family: 'Orbitron', monospace;
        }
        .cell.core {
            background: radial-gradient(ellipse, rgba(255, 80, 0, 0.35), rgba(200, 30, 0, 0.15));
            border-color: rgba(255, 100, 0, 0.3);
            font-size: 1.6rem;
            cursor: default;
        }
        .cell.core .core-glow {
            position: absolute;
            inset: -4px;
            border-radius: 10px;
            background: radial-gradient(ellipse, rgba(255, 120, 0, 0.15), transparent 70%);
            pointer-events: none;
            animation: core-pulse 2s ease-in-out infinite;
        }
        @keyframes core-pulse {
            0%,
            100% {
                opacity: 0.5;
                transform: scale(1);
            }
            50% {
                opacity: 1;
                transform: scale(1.08);
            }
        }
        .cell .component-icon {
            font-size: 1rem;
            z-index: 1;
        }
        .cell .component-label {
            position: absolute;
            bottom: -2px;
            font-size: 0.3rem;
            font-weight: 600;
            color: rgba(100, 200, 255, 0.3);
        }
        .cell.temp-cool {
            background: rgba(0, 100, 200, 0.15);
        }
        .cell.temp-warm {
            background: rgba(200, 150, 0, 0.15);
        }
        .cell.temp-hot {
            background: rgba(200, 50, 0, 0.2);
        }
        .cell.temp-critical {
            background: rgba(200, 0, 0, 0.3);
            animation: critical-flash 0.8s infinite;
        }
        @keyframes critical-flash {
            0%,
            100% {
                box-shadow: inset 0 0 20px rgba(255, 0, 0, 0.1);
            }
            50% {
                box-shadow: inset 0 0 40px rgba(255, 0, 0, 0.3);
            }
        }

        /* 工具栏 */
        .toolbar {
            display: flex;
            gap: 4px;
            flex-wrap: wrap;
            justify-content: center;
            margin: 6px 0 8px;
            padding: 4px 8px;
            background: rgba(0, 20, 40, 0.3);
            border-radius: 12px;
        }
        .tool-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 2px 6px;
            background: rgba(0, 30, 60, 0.3);
            border: 1px solid rgba(60, 180, 255, 0.08);
            border-radius: 8px;
            color: #7fc8ff;
            font-size: 0.5rem;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            min-width: 36px;
        }
        .tool-btn .icon {
            font-size: 0.9rem;
        }
        .tool-btn.active {
            border-color: #4dd0ff;
            background: rgba(0, 80, 160, 0.3);
        }
        .tool-btn:hover {
            background: rgba(0, 60, 120, 0.3);
            transform: translateY(-1px);
        }

        /* 控制按钮 */
        .controls {
            display: flex;
            gap: 6px;
            justify-content: center;
            flex-wrap: wrap;
            margin: 4px 0 8px;
        }
        .ctrl-btn {
            padding: 4px 14px;
            border-radius: 30px;
            font-weight: 700;
            font-size: 0.65rem;
            border: 1px solid rgba(60, 180, 255, 0.12);
            background: rgba(0, 60, 120, 0.25);
            color: #7fc8ff;
            cursor: pointer;
            transition: 0.2s;
            text-transform: uppercase;
        }
        .ctrl-btn:hover:not(:disabled) {
            background: rgba(0, 80, 160, 0.3);
            transform: translateY(-2px);
        }
        .ctrl-btn:disabled {
            opacity: 0.3;
            cursor: not-allowed;
        }
        .ctrl-btn.primary {
            border-color: rgba(0, 180, 255, 0.2);
            color: #4dd0ff;
        }
        .ctrl-btn.danger {
            border-color: rgba(255, 50, 50, 0.15);
            color: #ff6d6d;
        }
        .ctrl-btn.warning {
            border-color: rgba(255, 170, 0, 0.15);
            color: #ffab00;
        }
        .ctrl-btn.success {
            border-color: rgba(0, 230, 118, 0.15);
            color: #69f0ae;
        }

        /* 消息 */
        .message-area {
            margin-top: 8px;
            padding: 6px 12px;
            background: rgba(0, 20, 40, 0.3);
            border-radius: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 4px;
            min-height: 32px;
        }
        .message-area .msg {
            font-size: 0.7rem;
            color: rgba(160, 210, 255, 0.7);
            flex: 1;
        }
        .message-area .msg.warn {
            color: #ffab00;
        }
        .message-area .msg.danger {
            color: #ff1744;
        }
        .message-area .msg.success {
            color: #69f0ae;
        }
        .message-area .msg.info {
            color: #4dd0ff;
        }

        /* 响应式 */
        @media (max-width: 800px) {
            .dashboard {
                grid-template-columns: repeat(2, 1fr);
            }
            .device-panel {
                grid-template-columns: 1fr;
            }
            .rod-group-panel {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        @media (max-width: 500px) {
            .dashboard {
                grid-template-columns: 1fr 1fr;
                gap: 4px;
            }
            .gauge .value {
                font-size: 1rem;
            }
            .param-panel {
                grid-template-columns: repeat(2, 1fr);
            }
            .rod-group-panel {
                grid-template-columns: 1fr 1fr;
            }
            .grid-wrapper {
                max-width: 100%;
            }
            .cell {
                font-size: 0.8rem;
            }
            .cell.core {
                font-size: 1rem;
            }
            .meltdown-warning .timer {
                font-size: 1.2rem;
            }
        }
    </style>
</head>
<body>

    <div class="game-container" id="app">
        <!-- 标题 -->
        <div class="header">
            <div class="title">⚛️ 反应堆 <small>实时核融</small></div>
            <div class="status-badge">
                <span class="dot running" id="statusDot"></span>
                <span id="statusText">运行中</span>
            </div>
        </div>

        <!-- 仪表盘 -->
        <div class="dashboard">
            <div class="gauge">
                <div class="label">🌡️ 核心温度</div>
                <div class="value temp" id="tempDisplay">500</div>
                <div class="sub">°C / 极限 1500°C</div>
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
                <div class="sub">元 (0.5元/度)</div>
            </div>
            <div class="gauge">
                <div class="label">⏱️ 运行时间</div>
                <div class="value turns" id="timeDisplay">0s</div>
                <div class="sub">已部署 <span id="placedCount">0</span> 个组件</div>
            </div>
        </div>

        <!-- 额外参数 -->
        <div class="param-panel">
            <div class="param-item"><div class="p-label">热功率</div><div class="p-value" id="thermalPower">0.0 MW</div></div>
            <div class="param-item"><div class="p-label">冷却剂温度</div><div class="p-value" id="coolantTemp">300°C</div></div>
            <div class="param-item"><div class="p-label">冷却剂压力</div><div class="p-value" id="coolantPressure">7.0 MPa</div></div>
            <div class="param-item"><div class="p-label">反应堆稳定性</div><div class="p-value" id="reactorStability">100%</div></div>
            <div class="param-item"><div class="p-label">中子通量</div><div class="p-value" id="neutronFlux">50%</div></div>
            <div class="param-item"><div class="p-label">备用电源</div><div class="p-value" id="backupPower">100%</div></div>
        </div>

        <!-- 核融倒计时警告 -->
        <div class="meltdown-warning" id="meltdownWarning">
            <span class="label">☢️ 核融倒计时</span>
            <span class="timer" id="meltdownTimer">60</span>
            <span style="font-size:0.8rem;color:#ff1744;">秒</span>
        </div>

        <!-- 控制棒分组 -->
        <div class="rod-group-panel">
            <div class="rod-group" data-group="0">
                <span class="rod-label">控制棒 A</span>
                <input type="range" class="rod-slider" min="0" max="100" value="50">
                <span class="rod-value">50%</span>
                <button class="rod-lock">🔓</button>
            </div>
            <div class="rod-group" data-group="1">
                <span class="rod-label">控制棒 B</span>
                <input type="range" class="rod-slider" min="0" max="100" value="50">
                <span class="rod-value">50%</span>
                <button class="rod-lock">🔓</button>
            </div>
            <div class="rod-group" data-group="2">
                <span class="rod-label">控制棒 C</span>
                <input type="range" class="rod-slider" min="0" max="100" value="50">
                <span class="rod-value">50%</span>
                <button class="rod-lock">🔓</button>
            </div>
            <div class="rod-group" data-group="3">
                <span class="rod-label">控制棒 D</span>
                <input type="range" class="rod-slider" min="0" max="100" value="50">
                <span class="rod-value">50%</span>
                <button class="rod-lock">🔓</button>
            </div>
        </div>

        <!-- 设备面板 -->
        <div class="device-panel">
            <div class="device-group">
                <h4>💧 冷却水泵 <span id="pumpSummary">0/4 运行</span></h4>
                <div id="pumpContainer"></div>
            </div>
            <div class="device-group">
                <h4>⚡ 发电机+变压器 <span id="genSummary">0/4 正常</span></h4>
                <div id="genContainer"></div>
                <div style="margin-top:4px;font-size:0.55rem;color:rgba(160,210,255,0.4);">
                    停电状态: <span id="outageDisplay">无</span>
                </div>
            </div>
        </div>

        <!-- 网格 -->
        <div class="grid-wrapper">
            <div class="grid" id="grid"></div>
        </div>

        <!-- 工具栏 -->
        <div class="toolbar">
            <button class="tool-btn" data-type="pipe"><span class="icon">🔵</span>冷却管</button>
            <button class="tool-btn" data-type="sink"><span class="icon">🌀</span>散热器</button>
            <button class="tool-btn" data-type="rod"><span class="icon">🛑</span>控制棒</button>
            <button class="tool-btn" data-type="converter"><span class="icon">⚡</span>转换器</button>
            <button class="tool-btn" data-type="remove"><span class="icon">🗑️</span>移除</button>
        </div>

        <!-- 控制按钮 -->
        <div class="controls">
            <button class="ctrl-btn primary" id="btnPause">⏸ 暂停</button>
            <button class="ctrl-btn warning" id="btnScram">🛑 紧急停堆</button>
            <button class="ctrl-btn danger" id="btnReset">⟲ 重启</button>
        </div>

        <!-- 消息 -->
        <div class="message-area">
            <span class="msg info" id="message">实时模拟，控制温度避免核融！</span>
            <button class="msg-undo" id="btnUndo" disabled>↩ 撤销</button>
        </div>
    </div>

    <script>
        (() => {
            'use strict';

            // ---------- 常量 ----------
            const GRID_SIZE = 7;
            const CORE_ROW = 3,
                CORE_COL = 3;
            const MAX_TEMP = 1500; // 触发核融的温度
            const MELTDOWN_TIME = 60; // 秒
            const BASE_HEAT_RATE = 8.0; // 每秒升温基数
            const ROD_SPEED = 10; // %/秒
            const NUM_ROD_GROUPS = 4;
            const PRICE_PER_KWH = 0.5; // 元/度
            const UPDATE_INTERVAL = 1000; // 每秒更新

            // ---------- 状态 ----------
            let state = {
                grid: [],
                time: 0, // 秒
                temperature: 500, // 起始500°C
                electricPower: 0,
                thermalPower: 0,
                money: 0,
                totalEnergy: 0, // kWh
                rodGroups: [], // { depth, target, locked }
                pumps: [], // { on, fault }
                generators: [], // { on, damaged }
                powerOutage: false,
                coolantTemp: 300,
                coolantPressure: 7.0,
                neutronFlux: 50,
                backupPower: 100,
                placedCount: 0,
                gameOver: false,
                victory: false,
                paused: false,
                selectedTool: 'pipe',
                history: [],
                isProcessing: false,
                rodAnimIds: [],
                // 核融倒计时
                meltdownActive: false,
                meltdownCounter: MELTDOWN_TIME,
                // 定时器
                updateTimer: null,
            };

            // ---------- DOM ----------
            const $ = id => document.getElementById(id);
            const gridEl = $('grid');
            const tempDisplay = $('tempDisplay');
            const powerDisplay = $('powerDisplay');
            const moneyDisplay = $('moneyDisplay');
            const timeDisplay = $('timeDisplay');
            const tempBar = $('tempBar');
            const powerBar = $('powerBar');
            const statusDot = $('statusDot');
            const statusText = $('statusText');
            const messageEl = $('message');
            const placedCountEl = $('placedCount');
            const btnPause = $('btnPause');
            const btnScram = $('btnScram');
            const btnReset = $('btnReset');
            const btnUndo = $('btnUndo');
            const toolBtns = document.querySelectorAll('.tool-btn');
            const pumpContainer = $('pumpContainer');
            const genContainer = $('genContainer');
            const pumpSummary = $('pumpSummary');
            const genSummary = $('genSummary');
            const outageDisplay = $('outageDisplay');
            const thermalPowerEl = $('thermalPower');
            const coolantTempEl = $('coolantTemp');
            const coolantPressureEl = $('coolantPressure');
            const reactorStabilityEl = $('reactorStability');
            const neutronFluxEl = $('neutronFlux');
            const backupPowerEl = $('backupPower');
            const meltdownWarning = $('meltdownWarning');
            const meltdownTimer = $('meltdownTimer');

            // ---------- 工具 ----------
            function clamp(v, min, max) { return Math.max(min, Math.min(max, v)); }

            function rand(min, max) { return Math.random() * (max - min) + min; }

            function isCore(r, c) { return r === CORE_ROW && c === CORE_COL; }

            function inBounds(r, c) { return r >= 0 && r < GRID_SIZE && c >= 0 && c < GRID_SIZE; }

            function getNeighbors(r, c) {
                const dirs = [
                    [-1, 0],
                    [1, 0],
                    [0, -1],
                    [0, 1]
                ];
                const result = [];
                for (const [dr, dc] of dirs) {
                    const nr = r + dr,
                        nc = c + dc;
                    if (inBounds(nr, nc)) result.push([nr, nc]);
                }
                return result;
            }

            // ---------- 初始化 ----------
            function initGrid() {
                const g = [];
                for (let r = 0; r < GRID_SIZE; r++) {
                    g[r] = [];
                    for (let c = 0; c < GRID_SIZE; c++) {
                        g[r][c] = { type: 'empty', temp: 0, energy: 0 };
                    }
                }
                g[CORE_ROW][CORE_COL] = { type: 'core', temp: 500, energy: 0 };
                return g;
            }

            function initDevices() {
                state.pumps = [];
                for (let i = 0; i < 4; i++) {
                    state.pumps.push({ on: true, fault: false });
                }
                state.generators = [];
                for (let i = 0; i < 4; i++) {
                    state.generators.push({ on: true, damaged: false });
                }
                state.powerOutage = false;
                state.rodGroups = [];
                for (let i = 0; i < NUM_ROD_GROUPS; i++) {
                    state.rodGroups.push({ depth: 50, target: 50, locked: false });
                }
                state.rodAnimIds = new Array(NUM_ROD_GROUPS).fill(null);
            }

            // ---------- 保存/撤销 (简化) ----------
            function saveHistory() {
                const snap = {
                    grid: state.grid.map(row => row.map(c => ({ ...c }))),
                    time: state.time,
                    temperature: state.temperature,
                    electricPower: state.electricPower,
                    thermalPower: state.thermalPower,
                    money: state.money,
                    totalEnergy: state.totalEnergy,
                    rodGroups: state.rodGroups.map(g => ({ ...g })),
                    pumps: state.pumps.map(p => ({ ...p })),
                    generators: state.generators.map(g => ({ ...g })),
                    powerOutage: state.powerOutage,
                    coolantTemp: state.coolantTemp,
                    coolantPressure: state.coolantPressure,
                    neutronFlux: state.neutronFlux,
                    backupPower: state.backupPower,
                    placedCount: state.placedCount,
                    meltdownActive: state.meltdownActive,
                    meltdownCounter: state.meltdownCounter,
                };
                state.history.push(snap);
                if (state.history.length > 20) state.history.shift();
                btnUndo.disabled = false;
            }

            function undoHistory() {
                if (state.history.length === 0 || state.isProcessing) return;
                if (state.gameOver) return;
                const snap = state.history.pop();
                state.grid = snap.grid;
                state.time = snap.time;
                state.temperature = snap.temperature;
                state.electricPower = snap.electricPower;
                state.thermalPower = snap.thermalPower;
                state.money = snap.money;
                state.totalEnergy = snap.totalEnergy;
                state.rodGroups = snap.rodGroups.map(g => ({ ...g }));
                state.pumps = snap.pumps.map(p => ({ ...p }));
                state.generators = snap.generators.map(g => ({ ...g }));
                state.powerOutage = snap.powerOutage;
                state.coolantTemp = snap.coolantTemp;
                state.coolantPressure = snap.coolantPressure;
                state.neutronFlux = snap.neutronFlux;
                state.backupPower = snap.backupPower;
                state.placedCount = snap.placedCount;
                state.meltdownActive = snap.meltdownActive;
                state.meltdownCounter = snap.meltdownCounter;
                if (state.history.length === 0) btnUndo.disabled = true;
                renderAll();
                updateUI();
                setMessage('已撤销', 'info');
            }

            // ---------- 控制棒动画 ----------
            function animateRod(groupIdx) {
                const group = state.rodGroups[groupIdx];
                if (group.locked) {
                    const slider = document.querySelector(`.rod-group[data-group="${groupIdx}"] .rod-slider`);
                    const valDisplay = document.querySelector(`.rod-group[data-group="${groupIdx}"] .rod-value`);
                    if (slider) slider.value = Math.round(group.depth);
                    if (valDisplay) valDisplay.textContent = Math.round(group.depth) + '%';
                    state.rodAnimIds[groupIdx] = null;
                    return;
                }
                const diff = group.target - group.depth;
                if (Math.abs(diff) < 0.3) {
                    group.depth = group.target;
                    const slider = document.querySelector(`.rod-group[data-group="${groupIdx}"] .rod-slider`);
                    const valDisplay = document.querySelector(`.rod-group[data-group="${groupIdx}"] .rod-value`);
                    if (slider) slider.value = Math.round(group.depth);
                    if (valDisplay) valDisplay.textContent = Math.round(group.depth) + '%';
                    state.rodAnimIds[groupIdx] = null;
                    return;
                }
                const step = (ROD_SPEED / 1000) * 16 * Math.sign(diff);
                let newVal = group.depth + step;
                if (Math.abs(newVal - group.target) < 0.3) newVal = group.target;
                group.depth = clamp(newVal, 0, 100);
                const slider = document.querySelector(`.rod-group[data-group="${groupIdx}"] .rod-slider`);
                const valDisplay = document.querySelector(`.rod-group[data-group="${groupIdx}"] .rod-value`);
                if (slider) slider.value = Math.round(group.depth);
                if (valDisplay) valDisplay.textContent = Math.round(group.depth) + '%';
                state.rodAnimIds[groupIdx] = requestAnimationFrame(() => animateRod(groupIdx));
            }

            function setRodTarget(groupIdx, val) {
                const group = state.rodGroups[groupIdx];
                if (group.locked) {
                    setMessage(`控制棒组 ${String.fromCharCode(65+groupIdx)} 已锁定`, 'warn');
                    return;
                }
                val = clamp(val, 0, 100);
                group.target = val;
                if (!state.rodAnimIds[groupIdx] && Math.abs(group.depth - val) > 0.3) {
                    state.rodAnimIds[groupIdx] = requestAnimationFrame(() => animateRod(groupIdx));
                } else if (Math.abs(group.depth - val) < 0.3) {
                    group.depth = val;
                    const slider = document.querySelector(`.rod-group[data-group="${groupIdx}"] .rod-slider`);
                    const valDisplay = document.querySelector(`.rod-group[data-group="${groupIdx}"] .rod-value`);
                    if (slider) slider.value = Math.round(val);
                    if (valDisplay) valDisplay.textContent = Math.round(val) + '%';
                    if (state.rodAnimIds[groupIdx]) {
                        cancelAnimationFrame(state.rodAnimIds[groupIdx]);
                        state.rodAnimIds[groupIdx] = null;
                    }
                }
            }

            // ---------- 紧急停堆 ----------
            function scram() {
                if (state.gameOver) return;
                for (let i = 0; i < NUM_ROD_GROUPS; i++) {
                    const group = state.rodGroups[i];
                    if (!group.locked) {
                        group.target = 100;
                        if (!state.rodAnimIds[i]) {
                            state.rodAnimIds[i] = requestAnimationFrame(() => animateRod(i));
                        }
                    }
                }
                setMessage('🛑 紧急停堆！控制棒完全插入！', 'danger');
            }

            // ---------- 渲染设备 ----------
            function renderDevices() {
                let html = '';
                state.pumps.forEach((p, idx) => {
                    const ledClass = p.fault ? 'fault' : (p.on ? 'on' : 'off');
                    html += `<div class="device-row">
                        <span>泵 ${idx+1} <span class="dev-led ${ledClass}"></span></span>
                        <span style="font-size:0.5rem;">${p.fault?'故障':(p.on?'运行':'停机')}</span>
                        <div>
                            <button class="dev-btn ${p.on?'':'success'}" data-pump="${idx}" data-action="toggle">${p.on?'关':'开'}</button>
                            <button class="dev-btn danger" data-pump="${idx}" data-action="fault">故障</button>
                        </div>
                    </div>`;
                });
                pumpContainer.innerHTML = html;
                pumpContainer.querySelectorAll('[data-pump]').forEach(btn => {
                    btn.addEventListener('click', (e) => {
                        const idx = parseInt(btn.dataset.pump);
                        const action = btn.dataset.action;
                        const pump = state.pumps[idx];
                        if (action === 'toggle') {
                            if (state.powerOutage) { setMessage('停电中，无法操作水泵', 'warn'); return; }
                            pump.on = !pump.on;
                            renderDevices();
                            updateUI();
                            setMessage(`水泵 ${idx+1} ${pump.on?'启动':'关闭'}`, 'info');
                        } else if (action === 'fault') {
                            pump.fault = !pump.fault;
                            if (pump.fault) pump.on = false;
                            else pump.on = true;
                            renderDevices();
                            updateUI();
                            setMessage(`水泵 ${idx+1} ${pump.fault?'故障':'修复'}`, pump.fault?'danger':'success');
                        }
                    });
                });

                let htmlG = '';
                state.generators.forEach((g, idx) => {
                    const ledClass = g.damaged ? 'fault' : (g.on ? 'on' : 'off');
                    const statusText = g.damaged ? '损坏' : (g.on ? '运行' : '停机');
                    htmlG += `<div class="device-row">
                        <span>机组 ${idx+1} <span class="dev-led ${ledClass}"></span></span>
                        <span style="font-size:0.5rem;">${statusText}</span>
                        <div>
                            <button class="dev-btn ${g.on?'':'success'}" data-gen="${idx}" data-action="toggle">${g.on?'关':'开'}</button>
                            <button class="dev-btn danger" data-gen="${idx}" data-action="damage">损坏</button>
                            <button class="dev-btn success" data-gen="${idx}" data-action="repair">维修</button>
                        </div>
                    </div>`;
                });
                genContainer.innerHTML = htmlG;
                genContainer.querySelectorAll('[data-gen]').forEach(btn => {
                    btn.addEventListener('click', (e) => {
                        const idx = parseInt(btn.dataset.gen);
                        const action = btn.dataset.action;
                        const gen = state.generators[idx];
                        if (action === 'toggle') {
                            if (state.powerOutage) { setMessage('停电中，无法操作发电机', 'warn'); return; }
                            if (gen.damaged) { setMessage('机组损坏，请维修', 'warn'); return; }
                            gen.on = !gen.on;
                            renderDevices();
                            updateUI();
                            setMessage(`机组 ${idx+1} ${gen.on?'启动':'关闭'}`, 'info');
                        } else if (action === 'damage') {
                            if (gen.damaged) { setMessage('已损坏', 'warn'); return; }
                            gen.damaged = true;
                            gen.on = false;
                            renderDevices();
                            updateUI();
                            setMessage(`机组 ${idx+1} 损坏！`, 'danger');
                            checkOutage();
                        } else if (action === 'repair') {
                            if (!gen.damaged) { setMessage('该机组完好', 'info'); return; }
                            gen.damaged = false;
                            gen.on = true;
                            renderDevices();
                            updateUI();
                            setMessage(`机组 ${idx+1} 已修复`, 'success');
                            checkOutage();
                        }
                    });
                });
                updateDeviceStatus();
            }

            function checkOutage() {
                const allDamaged = state.generators.every(g => g.damaged);
                if (allDamaged) {
                    state.powerOutage = true;
                    state.pumps.forEach(p => p.on = false);
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

            // ---------- 物理更新 (每秒调用) ----------
            function updatePhysics() {
                if (state.gameOver || state.paused) return;
                state.time += 1;

                // 网格组件影响 (温度变化)
                const g = state.grid;
                const tempChanges = Array.from({ length: GRID_SIZE }, () => Array(GRID_SIZE).fill(0));

                for (let r = 0; r < GRID_SIZE; r++) {
                    for (let c = 0; c < GRID_SIZE; c++) {
                        const cell = g[r][c];
                        if (cell.type === 'empty' || cell.type === 'core') continue;
                        switch (cell.type) {
                            case 'pipe':
                                const pipeCool = 2.0;
                                tempChanges[r][c] -= pipeCool;
                                for (const [nr, nc] of getNeighbors(r, c)) tempChanges[nr][nc] -= pipeCool * 0.6;
                                break;
                            case 'sink':
                                tempChanges[r][c] -= 4.0;
                                for (const [nr, nc] of getNeighbors(r, c)) tempChanges[nr][nc] += 0.5;
                                break;
                            case 'rod':
                                tempChanges[r][c] -= 1.0;
                                for (const [nr, nc] of getNeighbors(r, c)) tempChanges[nr][nc] -= 0.5;
                                break;
                            case 'converter':
                                // 转换器提升发电效率
                                break;
                        }
                    }
                }

                // 应用到非核心格
                for (let r = 0; r < GRID_SIZE; r++) {
                    for (let c = 0; c < GRID_SIZE; c++) {
                        if (g[r][c].type === 'core') continue;
                        let newTemp = (g[r][c].temp || 0) + tempChanges[r][c];
                        newTemp = clamp(newTemp, 0, 2000);
                        g[r][c].temp = newTemp;
                    }
                }

                // ---- 核心温度变化 ----
                let coreHeat = BASE_HEAT_RATE * 1.5; // 每秒升温基数

                // 周围组件影响
                for (const [nr, nc] of getNeighbors(CORE_ROW, CORE_COL)) {
                    if (g[nr][nc].type === 'rod') coreHeat -= 0.8;
                    if (g[nr][nc].type === 'pipe') coreHeat -= 0.3;
                    if (g[nr][nc].type === 'converter') coreHeat += 0.5;
                    if (g[nr][nc].type === 'sink') coreHeat -= 0.4;
                }

                // 周围温度影响
                let avgNeighbor = 0,
                    cnt = 0;
                for (const [nr, nc] of getNeighbors(CORE_ROW, CORE_COL)) {
                    avgNeighbor += (g[nr][nc].temp || 0);
                    cnt++;
                }
                if (cnt > 0) {
                    avgNeighbor /= cnt;
                    coreHeat += avgNeighbor * 0.04;
                }

                // 控制棒组平均深度
                let avgRodDepth = 0;
                for (let i = 0; i < NUM_ROD_GROUPS; i++) {
                    avgRodDepth += state.rodGroups[i].depth;
                }
                avgRodDepth /= NUM_ROD_GROUPS;
                const rodFactor = 1 - (avgRodDepth / 100) * 0.85;
                coreHeat *= rodFactor;
                coreHeat = Math.max(coreHeat, 0.1);

                // 水泵冷却
                const pumpCount = state.pumps.filter(p => p.on && !p.fault).length;
                const pumpCool = pumpCount * 0.4;
                coreHeat -= pumpCool;

                // 转换器数量
                let converterCount = 0;
                for (let r = 0; r < GRID_SIZE; r++) {
                    for (let c = 0; c < GRID_SIZE; c++) {
                        if (g[r][c].type === 'converter') converterCount++;
                    }
                }
                coreHeat += converterCount * 0.15;

                // 正反馈 (温度越高升温越快)
                coreHeat += (state.temperature - 500) * 0.02;

                // 应用
                let newCore = state.temperature + coreHeat;
                // 额外邻居冷却
                let extra = 0;
                for (const [nr, nc] of getNeighbors(CORE_ROW, CORE_COL)) {
                    if (g[nr][nc].type === 'pipe') extra += 0.8;
                    if (g[nr][nc].type === 'sink') extra += 0.6;
                }
                newCore -= extra;
                state.temperature = clamp(newCore, 100, 2000);
                g[CORE_ROW][CORE_COL].temp = state.temperature;

                // ---- 热功率 (MW) ----
                state.thermalPower = (state.temperature / 500) * 150; // 500°C -> 150MW
                state.thermalPower = clamp(state.thermalPower, 0, 400);

                // ---- 电功率 ----
                let efficiency = 0.35;
                const rodEffFactor = 1 - (avgRodDepth / 100) * 0.4;
                const workingGens = state.generators.filter(g => g.on && !g.damaged).length;
                const genFactor = workingGens / 4;
                const converterBoost = 1 + converterCount * 0.05;
                let electricPower = state.thermalPower * efficiency * rodEffFactor * genFactor * converterBoost;
                electricPower = clamp(electricPower, 0, 200);
                state.electricPower = electricPower;
                if (state.powerOutage) state.electricPower = 0;

                // ---- 收益 ----
                const kWhThisSecond = state.electricPower * 1000 / 3600; // MW -> kWh per second
                state.totalEnergy += kWhThisSecond;
                state.money += kWhThisSecond * PRICE_PER_KWH / 1000;

                // ---- 额外参数 ----
                state.coolantTemp = 250 + (state.temperature - 500) * 0.6;
                state.coolantTemp = clamp(state.coolantTemp, 200, 800);
                state.coolantPressure = 6.5 + (state.coolantTemp - 300) * 0.015;
                state.coolantPressure = clamp(state.coolantPressure, 5.0, 10.0);
                state.neutronFlux = 20 + (state.temperature / 1500) * 60;
                state.neutronFlux = clamp(state.neutronFlux, 20, 100);
                const stab = 100 - Math.abs(state.temperature - 500) * 0.08;
                const stabVal = clamp(stab, 0, 100);
                const backup = 20 + (state.generators.filter(g => !g.damaged).length / 4) * 80;
                state.backupPower = clamp(backup, 0, 100);

                // ---- 高温故障 ----
                if (state.temperature > 1000 && Math.random() < 0.02) {
                    const availGens = state.generators.filter(g => !g.damaged);
                    if (availGens.length > 0) {
                        const idx = state.generators.indexOf(availGens[Math.floor(Math.random() * availGens.length)]);
                        state.generators[idx].damaged = true;
                        state.generators[idx].on = false;
                        setMessage(`⚠️ 高温导致机组 ${idx+1} 损坏！`, 'danger');
                        checkOutage();
                    }
                }
                if (state.temperature > 900 && Math.random() < 0.015) {
                    const availPumps = state.pumps.filter(p => !p.fault);
                    if (availPumps.length > 0) {
                        const idx = state.pumps.indexOf(availPumps[Math.floor(Math.random() * availPumps.length)]);
                        state.pumps[idx].fault = true;
                        state.pumps[idx].on = false;
                        setMessage(`⚠️ 高温导致水泵 ${idx+1} 故障！`, 'danger');
                        renderDevices();
                    }
                }

                // ---- 核融检测 ----
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
                        // 爆炸！
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

                // 胜利条件：累计收益达到100元
                if (state.money >= 100 && !state.gameOver) {
                    state.victory = true;
                    state.gameOver = true;
                    if (state.updateTimer) {
                        clearInterval(state.updateTimer);
                        state.updateTimer = null;
                    }
                    setMessage('🎉 累计收益达到100元！你赢了！', 'success');
                    statusDot.className = 'dot victory';
                    statusText.textContent = '🏆 胜利';
                }

                // 更新UI
                renderGrid();
                renderDevices();
                updateUI();
            }

            // ---------- 暂停/继续 ----------
            function togglePause() {
                if (state.gameOver) return;
                state.paused = !state.paused;
                btnPause.textContent = state.paused ? '▶ 继续' : '⏸ 暂停';
                setMessage(state.paused ? '⏸ 已暂停' : '▶ 继续运行', 'info');
            }

            // ---------- 重置 ----------
            function resetGame() {
                if (state.updateTimer) {
                    clearInterval(state.updateTimer);
                    state.updateTimer = null;
                }
                state.grid = initGrid();
                state.time = 0;
                state.temperature = 500;
                state.electricPower = 0;
                state.thermalPower = 0;
                state.money = 0;
                state.totalEnergy = 0;
                state.placedCount = 0;
                state.gameOver = false;
                state.victory = false;
                state.paused = false;
                state.history = [];
                state.isProcessing = false;
                state.meltdownActive = false;
                state.meltdownCounter = MELTDOWN_TIME;
                meltdownWarning.classList.remove('active');
                btnPause.textContent = '⏸ 暂停';
                btnUndo.disabled = true;
                initDevices();
                // 重置控制棒UI
                document.querySelectorAll('.rod-group').forEach((el, idx) => {
                    const slider = el.querySelector('.rod-slider');
                    const valDisplay = el.querySelector('.rod-value');
                    const lockBtn = el.querySelector('.rod-lock');
                    if (slider) slider.value = 50;
                    if (valDisplay) valDisplay.textContent = '50%';
                    if (lockBtn) { lockBtn.textContent = '🔓';
                        lockBtn.classList.remove('active'); }
                });
                renderAll();
                updateUI();
                setMessage('🔄 已重置', 'info');
                statusDot.className = 'dot running';
                statusText.textContent = '运行中';
                // 重新开始定时器
                state.updateTimer = setInterval(() => updatePhysics(), UPDATE_INTERVAL);
            }

            // ---------- 放置组件 ----------
            function placeComponent(row, col) {
                if (state.isProcessing) return;
                if (state.gameOver) { setMessage('游戏已结束', 'warn'); return; }
                if (isCore(row, col)) { setMessage('核心不可放置', 'warn'); return; }
                const cell = state.grid[row][col];
                if (cell.type !== 'empty') {
                    if (state.selectedTool === 'remove') {
                        saveHistory();
                        const removed = cell.type;
                        cell.type = 'empty';
                        cell.temp = 0;
                        cell.energy = 0;
                        state.placedCount = Math.max(0, state.placedCount - 1);
                        renderGrid();
                        updateUI();
                        setMessage(`移除 ${removed}`, 'info');
                        return;
                    }
                    setMessage('已有组件', 'warn');
                    return;
                }
                if (state.selectedTool === 'remove') { setMessage('无组件可移除', 'warn'); return; }
                saveHistory();
                cell.type = state.selectedTool;
                cell.temp = 0;
                cell.energy = 0;
                state.placedCount += 1;
                renderGrid();
                updateUI();
                setMessage(`放置 ${state.selectedTool}`, 'info');
            }

            // ---------- 渲染 ----------
            function renderGrid() {
                const g = state.grid;
                gridEl.innerHTML = '';
                for (let r = 0; r < GRID_SIZE; r++) {
                    for (let c = 0; c < GRID_SIZE; c++) {
                        const cell = g[r][c];
                        const div = document.createElement('div');
                        div.className = 'cell';
                        div.dataset.row = r;
                        div.dataset.col = c;
                        const temp = cell.temp || 0;
                        if (isCore(r, c)) {
                            div.classList.add('core');
                            const glow = document.createElement('div');
                            glow.className = 'core-glow';
                            div.appendChild(glow);
                            div.innerHTML = '☢️';
                            const t = document.createElement('span');
                            t.className = 'cell-temp';
                            t.textContent = `${Math.round(state.temperature)}°`;
                            div.appendChild(t);
                        } else {
                            if (temp > 600) div.classList.add('temp-hot');
                            else if (temp > 300) div.classList.add('temp-warm');
                            else if (temp > 0) div.classList.add('temp-cool');
                            if (temp > 800) div.classList.add('temp-critical');

                            if (cell.type !== 'empty') {
                                const icons = { pipe: '🔵', sink: '🌀', rod: '🛑', converter: '⚡' };
                                const icon = document.createElement('span');
                                icon.className = 'component-icon';
                                icon.textContent = icons[cell.type] || '❓';
                                div.appendChild(icon);
                                const label = document.createElement('span');
                                label.className = 'component-label';
                                const labels = { pipe: '冷却', sink: '散热', rod: '控制', converter: '转换' };
                                label.textContent = labels[cell.type] || '';
                                div.appendChild(label);
                            } else {
                                div.classList.add('empty');
                            }
                            if (temp > 1) {
                                const t = document.createElement('span');
                                t.className = 'cell-temp';
                                t.textContent = `${Math.round(temp)}°`;
                                div.appendChild(t);
                            }
                            if (cell.energy && cell.energy > 0.5) {
                                const e = document.createElement('span');
                                e.className = 'cell-energy';
                                e.textContent = `⚡${Math.round(cell.energy)}`;
                                div.appendChild(e);
                            }
                        }
                        div.addEventListener('click', () => {
                            const row = parseInt(div.dataset.row);
                            const col = parseInt(div.dataset.col);
                            placeComponent(row, col);
                        });
                        gridEl.appendChild(div);
                    }
                }
            }

            function renderAll() {
                renderGrid();
                renderDevices();
                for (let i = 0; i < NUM_ROD_GROUPS; i++) {
                    const group = state.rodGroups[i];
                    const slider = document.querySelector(`.rod-group[data-group="${i}"] .rod-slider`);
                    const valDisplay = document.querySelector(`.rod-group[data-group="${i}"] .rod-value`);
                    const lockBtn = document.querySelector(`.rod-group[data-group="${i}"] .rod-lock`);
                    if (slider) slider.value = Math.round(group.depth);
                    if (valDisplay) valDisplay.textContent = Math.round(group.depth) + '%';
                    if (lockBtn) {
                        lockBtn.textContent = group.locked ? '🔒' : '🔓';
                        lockBtn.classList.toggle('active', group.locked);
                    }
                }
            }

            // ---------- 更新UI ----------
            function updateUI() {
                const temp = state.temperature;
                const power = state.electricPower;
                const money = state.money;

                tempDisplay.textContent = Math.round(temp);
                powerDisplay.textContent = power.toFixed(1);
                moneyDisplay.textContent = money.toFixed(2);
                timeDisplay.textContent = Math.round(state.time) + 's';
                placedCountEl.textContent = state.placedCount;

                const tempPct = clamp((temp / MAX_TEMP) * 100, 0, 100);
                const powerPct = clamp((power / 200) * 100, 0, 100);
                tempBar.style.width = tempPct + '%';
                powerBar.style.width = powerPct + '%';

                thermalPowerEl.textContent = state.thermalPower.toFixed(1) + ' MW';
                coolantTempEl.textContent = Math.round(state.coolantTemp) + '°C';
                coolantTempEl.className = 'p-value' + (state.coolantTemp > 600 ? ' danger' : (state.coolantTemp > 450 ?
                    ' warn' : ' good'));
                coolantPressureEl.textContent = state.coolantPressure.toFixed(1) + ' MPa';
                coolantPressureEl.className = 'p-value' + (state.coolantPressure > 8.5 ? ' danger' : (state.coolantPressure >
                    7.5 ? ' warn' : ' good'));
                const stab = 100 - Math.abs(state.temperature - 500) * 0.08;
                const stabVal = clamp(stab, 0, 100);
                reactorStabilityEl.textContent = Math.round(stabVal) + '%';
                reactorStabilityEl.className = 'p-value' + (stabVal > 60 ? ' good' : (stabVal > 30 ? ' warn' : ' danger'));
                neutronFluxEl.textContent = Math.round(state.neutronFlux) + '%';
                neutronFluxEl.className = 'p-value' + (state.neutronFlux > 80 ? ' danger' : (state.neutronFlux > 60 ?
                    ' warn' : ' good'));
                backupPowerEl.textContent = Math.round(state.backupPower) + '%';
                backupPowerEl.className = 'p-value' + (state.backupPower > 50 ? ' good' : ' warn');

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
                btnUndo.disabled = state.history.length === 0 || state.gameOver || state.isProcessing;

                // 核融警告显示
                if (state.meltdownActive && !state.gameOver) {
                    meltdownWarning.classList.add('active');
                    meltdownTimer.textContent = state.meltdownCounter;
                } else {
                    meltdownWarning.classList.remove('active');
                }
            }

            function setMessage(text, type = 'info') {
                messageEl.textContent = text;
                messageEl.className = 'msg ' + type;
            }

            // ---------- 工具选择 ----------
            function selectTool(type) {
                state.selectedTool = type;
                toolBtns.forEach(btn => {
                    btn.classList.toggle('active', btn.dataset.type === type);
                });
            }

            // ---------- 键盘 ----------
            function handleKeydown(e) {
                if (e.key === ' ' || e.key === 'p') {
                    e.preventDefault();
                    togglePause();
                }
                if (e.key === 'r' || e.key === 'R') resetGame();
                if (e.key === 's' || e.key === 'S') scram();
                if (e.key === 'z' && (e.ctrlKey || e.metaKey)) {
                    e.preventDefault();
                    undoHistory();
                }
                const map = { '1': 'pipe', '2': 'sink', '3': 'rod', '4': 'converter', '5': 'remove' };
                if (e.key in map) selectTool(map[e.key]);
            }

            // ---------- 初始化 ----------
            function init() {
                state.grid = initGrid();
                state.temperature = 500;
                state.grid[CORE_ROW][CORE_COL].temp = 500;
                initDevices();
                selectTool('pipe');
                renderAll();
                updateUI();
                setMessage('🟢 实时模拟，控制温度避免核融！', 'info');

                // 事件
                btnPause.addEventListener('click', togglePause);
                btnScram.addEventListener('click', scram);
                btnReset.addEventListener('click', resetGame);
                btnUndo.addEventListener('click', undoHistory);

                toolBtns.forEach(btn => {
                    btn.addEventListener('click', () => {
                        if (state.gameOver) { setMessage('游戏已结束', 'warn'); return; }
                        selectTool(btn.dataset.type);
                        setMessage(`工具: ${btn.dataset.type}`, 'info');
                    });
                });

                // 控制棒组事件
                document.querySelectorAll('.rod-group').forEach((el, idx) => {
                    const slider = el.querySelector('.rod-slider');
                    const lockBtn = el.querySelector('.rod-lock');
                    slider.addEventListener('input', () => {
                        const val = parseInt(slider.value);
                        setRodTarget(idx, val);
                    });
                    lockBtn.addEventListener('click', () => {
                        const group = state.rodGroups[idx];
                        group.locked = !group.locked;
                        lockBtn.textContent = group.locked ? '🔒' : '🔓';
                        lockBtn.classList.toggle('active', group.locked);
                        if (group.locked) {
                            if (state.rodAnimIds[idx]) {
                                cancelAnimationFrame(state.rodAnimIds[idx]);
                                state.rodAnimIds[idx] = null;
                            }
                            setMessage(`控制棒组 ${String.fromCharCode(65+idx)} 已锁定`, 'warn');
                        } else {
                            setMessage(`控制棒组 ${String.fromCharCode(65+idx)} 已解锁`, 'info');
                        }
                    });
                });

                document.addEventListener('keydown', handleKeydown);

                // 启动定时器
                state.updateTimer = setInterval(() => updatePhysics(), UPDATE_INTERVAL);
            }

            init();
        })();
    </script>

</body>
</html>
