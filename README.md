<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="theme-color" content="#0d1117">
<title>核电站控制台</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; user-select: none; -webkit-tap-highlight-color: transparent; }
body {
    background: #0d1117;
    color: #d0d7de;
    font-family: 'Segoe UI', 'Microsoft YaHei', -apple-system, sans-serif;
    min-height: 100vh;
    display: flex; justify-content: center; align-items: flex-start;
    padding: 12px;
    background-image:
        radial-gradient(ellipse at 20% 0%, rgba(0, 212, 170, 0.08) 0%, transparent 60%),
        radial-gradient(ellipse at 80% 100%, rgba(255, 159, 67, 0.06) 0%, transparent 60%),
        linear-gradient(180deg, #0d1117 0%, #161b22 100%);
    overflow-x: hidden;
}
.game-container {
    max-width: 1000px; width: 100%;
    background: linear-gradient(180deg, rgba(22, 27, 34, 0.95) 0%, rgba(13, 17, 23, 0.98) 100%);
    border-radius: 16px;
    padding: 22px 20px 26px;
    border: 1px solid rgba(48, 54, 61, 0.8);
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5), inset 0 1px 0 rgba(255, 255, 255, 0.04);
    position: relative;
}
.game-container::before {
    content: '';
    position: absolute; top: 0; left: 10%; right: 10%; height: 2px;
    background: linear-gradient(90deg, transparent, #00d4aa, #ff9f43, transparent);
    border-radius: 2px; opacity: 0.7;
}

.header {
    display: flex; justify-content: space-between; align-items: center;
    flex-wrap: wrap; gap: 8px 12px; margin-bottom: 14px;
}
.title-wrap { display: flex; align-items: center; gap: 8px; flex-wrap: wrap; }
.title {
    font-weight: 900; font-size: 1.25rem;
    letter-spacing: 2px; color: #f0f6fc;
    display: flex; align-items: center; gap: 10px;
    text-shadow: 0 0 20px rgba(0, 212, 170, 0.3);
    white-space: nowrap;
}
.title-icon { width: 18px; height: 18px; position: relative; display: inline-block; flex-shrink: 0; }
.title-icon::before {
    content: ''; position: absolute; inset: 2px; border-radius: 50%;
    background: radial-gradient(circle at 35% 35%, #4dffc3, #00d4aa);
    box-shadow: 0 0 12px rgba(0, 212, 170, 0.9);
}
.title-icon::after {
    content: ''; position: absolute; inset: -3px;
    border: 1.5px solid rgba(0, 212, 170, 0.6); border-radius: 50%;
    border-top-color: transparent; border-bottom-color: transparent;
    animation: spin 3s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }
.help-btn, .music-btn {
    padding: 5px 12px; border-radius: 6px;
    border: 1px solid rgba(0, 212, 170, 0.35);
    background: rgba(0, 212, 170, 0.08);
    color: #4dffc3; font-weight: 700; font-size: 0.68rem;
    cursor: pointer; transition: all 0.2s;
    font-family: inherit; white-space: nowrap;
}
.help-btn:hover, .music-btn:hover {
    background: rgba(0, 212, 170, 0.2);
    border-color: #00d4aa;
}
.music-btn.playing {
    border-color: #ff9f43;
    color: #ff9f43;
    background: rgba(255, 159, 67, 0.12);
    animation: musicPulse 1.8s ease-in-out infinite;
}
@keyframes musicPulse {
    0%, 100% { box-shadow: 0 0 8px rgba(255, 159, 67, 0.3); }
    50% { box-shadow: 0 0 18px rgba(255, 159, 67, 0.6); }
}
.status-badge {
    display: flex; align-items: center; gap: 8px; font-size: 0.75rem;
    font-weight: 700; padding: 6px 14px; border-radius: 40px;
    background: rgba(0, 212, 170, 0.08); border: 1px solid rgba(0, 212, 170, 0.3);
    color: #4dffc3; white-space: nowrap;
}
.status-badge .dot {
    width: 10px; height: 10px; border-radius: 50%;
    animation: pulse-dot 1.4s ease-in-out infinite;
}
.dot.running { background: #00d4aa; box-shadow: 0 0 10px #00d4aa; }
.dot.warning { background: #ff9f43; box-shadow: 0 0 10px #ff9f43; animation-duration: 0.8s; }
.dot.danger { background: #ff5c5c; box-shadow: 0 0 10px #ff5c5c; animation-duration: 0.4s; }
.dot.meltdown { background: #ff1744; box-shadow: 0 0 14px #ff1744; animation-duration: 0.2s; }
.dot.victory { background: #ffd740; box-shadow: 0 0 10px #ffd740; }
@keyframes pulse-dot {
    0%, 100% { transform: scale(1); opacity: 1; }
    50% { transform: scale(1.5); opacity: 0.5; }
}

/* ===== 4 主仪表盘 ===== */
.dashboard {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 14px;
    margin-bottom: 16px;
}
.gauge {
    background: linear-gradient(180deg, rgba(30, 36, 44, 0.95) 0%, rgba(18, 22, 28, 0.95) 100%);
    border-radius: 12px;
    padding: 16px 18px 14px;
    border: 1px solid rgba(48, 54, 61, 0.8);
    box-shadow: inset 0 1px 0 rgba(255,255,255,0.04);
    position: relative; overflow: hidden;
}
.gauge::before {
    content: ''; position: absolute; top: 0; left: 0; right: 0; height: 3px; opacity: 0.9;
}
.gauge:nth-child(1)::before { background: linear-gradient(90deg, transparent, #ff9f43, transparent); }
.gauge:nth-child(2)::before { background: linear-gradient(90deg, transparent, #00d4aa, transparent); }
.gauge:nth-child(3)::before { background: linear-gradient(90deg, transparent, #ffd740, transparent); }
.gauge:nth-child(4)::before { background: linear-gradient(90deg, transparent, #58a6ff, transparent); }
.gauge .label {
    font-size: 0.7rem; font-weight: 700; letter-spacing: 1px;
    color: #8b949e; margin-bottom: 8px;
}
.gauge .value {
    font-family: 'Consolas', 'Courier New', monospace;
    font-size: 2.2rem; font-weight: 900; line-height: 1;
    text-shadow: 0 0 20px currentColor, 0 0 40px currentColor;
    letter-spacing: 1px;
}
.gauge .value.temp { color: #ff9f43; }
.gauge .value.power { color: #00d4aa; }
.gauge .value.money { color: #ffd740; }
.gauge .value.time { color: #58a6ff; }
.gauge .unit {
    font-family: 'Consolas', monospace; font-size: 0.8rem;
    font-weight: 700; color: #6e7681; margin-top: 4px;
}
.gauge .bar-track {
    width: 100%; height: 5px; background: rgba(0, 0, 0, 0.5);
    border-radius: 3px; margin-top: 10px; overflow: hidden;
    border: 1px solid rgba(48, 54, 61, 0.5); position: relative;
}
.gauge .bar-fill { height: 100%; transition: width 0.4s; position: relative; }
.gauge .bar-fill::after {
    content: ''; position: absolute; top: 0; left: 0; right: 0; bottom: 0;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.5), transparent);
    animation: shimmer 2s ease-in-out infinite;
}
@keyframes shimmer {
    0% { transform: translateX(-100%); }
    100% { transform: translateX(100%); }
}
.bar-fill.temp-bar { background: linear-gradient(90deg, #00d4aa, #ff9f43, #ff5c5c); }
.bar-fill.power-bar { background: linear-gradient(90deg, #ffd740, #ff9f43); }

/* ===== 参数面板 ===== */
.param-panel {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 4px; margin-bottom: 14px;
    background: linear-gradient(180deg, rgba(30, 36, 44, 0.7) 0%, rgba(18, 22, 28, 0.7) 100%);
    border-radius: 10px; padding: 12px 14px;
    border: 1px solid rgba(48, 54, 61, 0.6);
}
.param-item { text-align: center; padding: 2px 4px; min-width: 0; }
.param-item .p-label {
    font-size: 0.55rem; font-weight: 700; color: #6e7681;
    letter-spacing: 0.5px; margin-bottom: 5px;
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
}
.param-item .p-value {
    font-family: 'Consolas', monospace; font-size: 0.95rem;
    font-weight: 700; color: #d0d7de; transition: color 0.3s;
    white-space: nowrap;
}
.param-item .p-value.good { color: #4dffc3; text-shadow: 0 0 8px rgba(77, 255, 195, 0.4); }
.param-item .p-value.warn { color: #ff9f43; text-shadow: 0 0 8px rgba(255, 159, 67, 0.4); }
.param-item .p-value.danger { color: #ff5c5c; text-shadow: 0 0 8px rgba(255, 92, 92, 0.5); }

.panel-row {
    background: linear-gradient(180deg, rgba(30, 36, 44, 0.9) 0%, rgba(20, 25, 31, 0.9) 100%);
    border-radius: 10px; padding: 10px 16px; margin-bottom: 10px;
    border: 1px solid rgba(48, 54, 61, 0.7);
    display: flex; align-items: center; flex-wrap: wrap; gap: 12px;
}
.panel-row .row-label {
    font-weight: 700; font-size: 0.72rem; color: #8b949e; letter-spacing: 0.5px;
}
.panel-row .row-value {
    font-family: 'Consolas', monospace; font-size: 0.95rem;
    color: #ffd740; min-width: 48px; text-align: center;
    font-weight: 700; text-shadow: 0 0 12px rgba(255, 215, 64, 0.4);
}
.panel-row input[type="range"] {
    flex: 1; min-width: 100px; accent-color: #00d4aa; height: 6px; cursor: pointer;
}
.power-select .ps-btn {
    padding: 6px 20px; border-radius: 6px;
    border: 1px solid rgba(48, 54, 61, 0.8);
    background: rgba(48, 54, 61, 0.4);
    color: #8b949e; font-weight: 700; font-size: 0.72rem;
    cursor: pointer; transition: all 0.2s; font-family: inherit;
}
.power-select .ps-btn:hover { background: rgba(48, 54, 61, 0.7); color: #d0d7de; }
.power-select .ps-btn.active.main {
    background: linear-gradient(180deg, #00d4aa, #00a888);
    border-color: #4dffc3; color: #0d1117;
    box-shadow: 0 0 16px rgba(0, 212, 170, 0.5);
}
.power-select .ps-btn.active.backup {
    background: linear-gradient(180deg, #ff9f43, #e0821e);
    border-color: #ffb866; color: #0d1117;
    box-shadow: 0 0 16px rgba(255, 159, 67, 0.5);
}
.power-select .ps-info { font-size: 0.68rem; color: #6e7681; margin-left: auto; }
.relief-control .relief-btn {
    padding: 5px 18px; border-radius: 6px;
    border: 1px solid rgba(255, 159, 67, 0.5);
    background: rgba(255, 159, 67, 0.12);
    color: #ff9f43; font-weight: 700; font-size: 0.68rem;
    cursor: pointer; transition: all 0.2s; font-family: inherit;
}
.relief-control .relief-btn:hover:not(:disabled) {
    background: rgba(255, 159, 67, 0.25);
}
.relief-control .relief-btn:disabled { opacity: 0.4; cursor: not-allowed; }
.relief-control .relief-status {
    font-family: 'Consolas', monospace; font-size: 0.78rem;
    color: #4dffc3; min-width: 70px; font-weight: 700;
}
.relief-control .relief-status.warn { color: #ff9f43; }
.relief-control .relief-status.danger { color: #ff5c5c; }

.meltdown-warning {
    background: linear-gradient(90deg, rgba(220, 38, 38, 0.2), rgba(255, 23, 68, 0.15));
    border: 1px solid #ff1744;
    border-radius: 10px; padding: 12px 18px; margin-bottom: 12px;
    display: none; justify-content: space-between; align-items: center;
    animation: meltdownPulse 1s ease-in-out infinite;
}
.meltdown-warning.active { display: flex; }
@keyframes meltdownPulse {
    0%, 100% { box-shadow: 0 0 0 rgba(255, 23, 68, 0); }
    50% { box-shadow: 0 0 30px rgba(255, 23, 68, 0.5); }
}
.meltdown-warning .label { font-weight: 900; color: #ff5c5c; font-size: 0.95rem; letter-spacing: 1.5px; }
.meltdown-warning .timer {
    font-family: 'Consolas', monospace; font-size: 2rem; font-weight: 900; color: #ff1744;
    text-shadow: 0 0 20px rgba(255, 23, 68, 0.8);
}
.rod-single .rod-lock {
    padding: 5px 14px; border-radius: 6px;
    border: 1px solid rgba(48, 54, 61, 0.8); background: rgba(48, 54, 61, 0.4);
    color: #8b949e; font-size: 0.68rem; cursor: pointer; transition: all 0.2s;
    font-weight: 700; font-family: inherit;
}
.rod-single .rod-lock:hover { background: rgba(48, 54, 61, 0.7); color: #d0d7de; }
.rod-single .rod-lock.active {
    background: linear-gradient(180deg, #ff5c5c, #d94747);
    border-color: #ff8585; color: #fff;
}
.device-panel { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px; }
.device-group {
    background: linear-gradient(180deg, rgba(30, 36, 44, 0.9) 0%, rgba(20, 25, 31, 0.9) 100%);
    border-radius: 10px; padding: 12px 14px; border: 1px solid rgba(48, 54, 61, 0.7);
    min-width: 0;
}
.device-group h4 {
    font-size: 0.65rem; font-weight: 700; color: #8b949e;
    margin-bottom: 10px; display: flex; justify-content: space-between;
    letter-spacing: 0.5px;
    border-bottom: 1px solid rgba(48, 54, 61, 0.5);
    padding-bottom: 8px;
}
.device-row {
    display: flex; align-items: center; justify-content: space-between;
    padding: 5px 0; font-size: 0.65rem; gap: 6px; flex-wrap: wrap;
    border-bottom: 1px solid rgba(48, 54, 61, 0.3);
}
.device-row:last-child { border-bottom: none; }
.device-row .dev-led {
    width: 9px; height: 9px; border-radius: 50%; display: inline-block; margin-right: 5px;
    transition: all 0.3s;
}
.dev-led.on { background: #4dffc3; box-shadow: 0 0 8px #4dffc3; }
.dev-led.off { background: #484f58; }
.dev-led.fault { background: #ff5c5c; box-shadow: 0 0 8px #ff5c5c; animation: blink 0.5s infinite; }
@keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0.4; } }
.device-row .dev-btn {
    padding: 3px 9px; border-radius: 4px;
    border: 1px solid rgba(48, 54, 61, 0.8); background: rgba(48, 54, 61, 0.4);
    color: #8b949e; font-size: 0.55rem; cursor: pointer; transition: all 0.2s;
    font-weight: 700; font-family: inherit;
}
.device-row .dev-btn:hover:not(:disabled) { background: rgba(48, 54, 61, 0.8); color: #d0d7de; }
.device-row .dev-btn:disabled { opacity: 0.35; cursor: not-allowed; }
.device-row .dev-btn.repair.ready {
    border-color: #4dffc3; color: #4dffc3;
    background: rgba(77, 255, 195, 0.1);
    animation: repairGlow 1.5s ease-in-out infinite;
}
@keyframes repairGlow {
    0%, 100% { box-shadow: 0 0 0 rgba(77, 255, 195, 0); }
    50% { box-shadow: 0 0 12px rgba(77, 255, 195, 0.5); }
}
.device-row .power-slider { flex: 1; min-width: 40px; accent-color: #00d4aa; height: 4px; cursor: pointer; }
.device-row .power-label {
    font-family: 'Consolas', monospace; font-size: 0.6rem;
    color: #ffd740; min-width: 30px; text-align: center; font-weight: 700;
}
.device-row .cooldown-label {
    font-family: 'Consolas', monospace; font-size: 0.6rem;
    color: #ff5c5c; min-width: 50px; text-align: center; font-weight: 700;
}
.device-row .src-btn {
    padding: 2px 7px; border-radius: 4px;
    border: 1px solid rgba(48, 54, 61, 0.8); background: rgba(48, 54, 61, 0.4);
    color: #8b949e; font-size: 0.55rem; font-weight: 700; cursor: pointer; min-width: 26px;
    font-family: inherit;
}
.device-row .src-btn.main {
    background: linear-gradient(180deg, #00d4aa, #00a888);
    border-color: #4dffc3; color: #0d1117;
}
.device-row .src-btn.backup {
    background: linear-gradient(180deg, #ff9f43, #e0821e);
    border-color: #ffb866; color: #0d1117;
}
.terminal {
    background: #010409; border: 1px solid rgba(48, 54, 61, 0.8);
    border-radius: 10px; margin-bottom: 12px; overflow: hidden;
}
.terminal-header {
    display: flex; align-items: center; gap: 6px; padding: 8px 14px;
    background: linear-gradient(180deg, #1c2128, #161b22);
    border-bottom: 1px solid rgba(48, 54, 61, 0.8);
}
.terminal-dot { width: 10px; height: 10px; border-radius: 50%; }
.terminal-dot.red { background: #ff5f56; }
.terminal-dot.yellow { background: #ffbd2e; }
.terminal-dot.green { background: #27c93f; }
.terminal-title {
    font-family: 'Consolas', monospace; font-size: 0.65rem;
    color: #6e7681; letter-spacing: 1.5px; margin-left: 8px; font-weight: 700;
}
.terminal-body {
    padding: 10px 14px; height: 110px; overflow-y: auto;
    font-family: 'Consolas', monospace;
    font-size: 0.72rem; line-height: 1.6; background: #010409;
}
.terminal-body::-webkit-scrollbar { width: 6px; }
.terminal-body::-webkit-scrollbar-track { background: #010409; }
.terminal-body::-webkit-scrollbar-thumb { background: #30363d; border-radius: 3px; }
.terminal-line { white-space: pre-wrap; word-break: break-all; }
.terminal-line.info { color: #8b949e; }
.terminal-line.warn { color: #ff9f43; }
.terminal-line.danger { color: #ff5c5c; }
.terminal-line.success { color: #4dffc3; }
.terminal-line.system { color: #58a6ff; font-weight: 700; }
.controls { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; margin: 8px 0; }
.ctrl-btn {
    padding: 9px 20px; border-radius: 8px; font-weight: 700; font-size: 0.72rem;
    border: 1px solid rgba(48, 54, 61, 0.8);
    background: linear-gradient(180deg, rgba(48, 54, 61, 0.6), rgba(30, 36, 44, 0.6));
    color: #d0d7de; cursor: pointer; transition: all 0.2s;
    text-transform: uppercase; letter-spacing: 0.5px;
    font-family: inherit;
}
.ctrl-btn:hover:not(:disabled) {
    background: linear-gradient(180deg, rgba(48, 54, 61, 0.9), rgba(30, 36, 44, 0.9));
}
.ctrl-btn:active:not(:disabled) { transform: scale(0.96); }
.ctrl-btn:disabled { opacity: 0.35; cursor: not-allowed; }
.ctrl-btn.primary { border-color: rgba(88, 166, 255, 0.5); color: #58a6ff; background: rgba(88, 166, 255, 0.1); }
.ctrl-btn.danger { border-color: rgba(255, 92, 92, 0.5); color: #ff8585; background: rgba(255, 92, 92, 0.1); }
.ctrl-btn.warning { border-color: rgba(255, 159, 67, 0.5); color: #ffb866; background: rgba(255, 159, 67, 0.1); }
.ctrl-btn.success {
    border-color: #4dffc3; color: #4dffc3;
    background: rgba(77, 255, 195, 0.15);
    animation: restartPulse 1.5s ease-in-out infinite;
}
@keyframes restartPulse {
    0%, 100% { box-shadow: 0 0 0 rgba(77, 255, 195, 0); }
    50% { box-shadow: 0 0 20px rgba(77, 255, 195, 0.5); }
}
.message-area {
    margin-top: 8px; padding: 10px 16px;
    background: linear-gradient(90deg, rgba(88, 166, 255, 0.05), rgba(0, 212, 170, 0.05));
    border-radius: 8px;
    border: 1px solid rgba(48, 54, 61, 0.6);
    display: flex; align-items: center; min-height: 38px;
}
.message-area .msg { font-size: 0.78rem; color: #8b949e; flex: 1; font-weight: 600; }
.message-area .msg.warn { color: #ff9f43; }
.message-area .msg.danger { color: #ff5c5c; }
.message-area .msg.success { color: #4dffc3; }
.message-area .msg.info { color: #58a6ff; }
.modal-overlay {
    position: fixed; top: 0; left: 0; right: 0; bottom: 0;
    background: rgba(1, 4, 9, 0.9);
    backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px);
    display: none; justify-content: center; align-items: center;
    z-index: 1000; padding: 16px;
}
.modal-overlay.active { display: flex; }
.modal {
    max-width: 720px; width: 100%; max-height: 85vh;
    background: linear-gradient(180deg, #161b22 0%, #0d1117 100%);
    border-radius: 14px; border: 1px solid rgba(48, 54, 61, 0.9);
    display: flex; flex-direction: column; overflow: hidden;
}
.modal-header {
    display: flex; justify-content: space-between; align-items: center;
    padding: 16px 22px;
    background: linear-gradient(180deg, #1c2128, #161b22);
    border-bottom: 1px solid rgba(48, 54, 61, 0.8);
}
.modal-header h2 { font-size: 1.1rem; font-weight: 700; color: #f0f6fc; letter-spacing: 1px; }
.modal-close {
    width: 34px; height: 34px; border-radius: 6px;
    border: 1px solid rgba(48, 54, 61, 0.8); background: rgba(48, 54, 61, 0.4);
    color: #8b949e; font-size: 1rem; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    font-family: inherit;
}
.modal-close:hover { background: #ff5c5c; color: #fff; }
.modal-body { padding: 20px 24px; overflow-y: auto; flex: 1; line-height: 1.7; }
.modal-body::-webkit-scrollbar { width: 6px; }
.modal-body::-webkit-scrollbar-thumb { background: #30363d; border-radius: 3px; }
.help-section { margin-bottom: 22px; }
.help-section h3 {
    font-size: 0.9rem; font-weight: 700; color: #f0f6fc;
    margin-bottom: 12px; padding-bottom: 8px;
    border-bottom: 1px solid rgba(48, 54, 61, 0.6);
}
.help-section p { font-size: 0.85rem; color: #8b949e; margin-bottom: 8px; }
.help-section ul { list-style: none; padding-left: 4px; }
.help-section ul li {
    font-size: 0.85rem; color: #8b949e; padding: 4px 0 4px 18px; position: relative;
}
.help-section ul li::before {
    content: '·'; position: absolute; left: 4px; color: #00d4aa; font-weight: 700; font-size: 1rem;
}
.help-section .key {
    display: inline-block; padding: 2px 8px;
    background: #21262d; border: 1px solid #30363d; border-radius: 4px;
    font-family: 'Consolas', monospace; font-size: 0.75rem; color: #f0f6fc;
}
.help-section .highlight { color: #ffd740; font-weight: 700; }
.help-section .danger-text { color: #ff5c5c; font-weight: 700; }
.help-section .good-text { color: #4dffc3; font-weight: 700; }
.help-tip {
    background: rgba(77, 255, 195, 0.05);
    border: 1px solid rgba(77, 255, 195, 0.25);
    border-radius: 6px; padding: 10px 14px; margin: 8px 0;
    font-size: 0.82rem; color: #8b949e;
}
.help-tip .tip-icon { color: #4dffc3; font-weight: 700; margin-right: 4px; }
.scram-modal {
    max-width: 520px; background: linear-gradient(180deg, #161b22 0%, #0d1117 100%);
    border-radius: 14px; border: 2px solid; padding: 36px 32px; text-align: center;
}
.scram-modal.success { border-color: #4dffc3; }
.scram-modal.fail { border-color: #ff5c5c; }
.scram-modal .scram-icon { font-size: 4rem; line-height: 1; margin-bottom: 12px; display: block; }
.scram-modal.success .scram-icon { color: #4dffc3; }
.scram-modal.fail .scram-icon { color: #ff5c5c; }
.scram-modal h2 { font-size: 1.6rem; font-weight: 900; margin-bottom: 10px; }
.scram-modal.success h2 { color: #4dffc3; }
.scram-modal.fail h2 { color: #ff5c5c; }
.scram-modal .scram-desc { font-size: 0.95rem; color: #8b949e; margin-bottom: 20px; }
.scram-modal .scram-btn {
    padding: 10px 36px; border-radius: 8px; font-weight: 700;
    font-size: 0.9rem; cursor: pointer; border: 2px solid;
    text-transform: uppercase; font-family: inherit;
}
.scram-modal.success .scram-btn { border-color: #4dffc3; background: rgba(77,255,195,0.15); color: #4dffc3; }
.scram-modal.fail .scram-btn { border-color: #ff5c5c; background: rgba(255,92,92,0.15); color: #ff5c5c; }

/* ============================================
   响应式适配
   ============================================ */

/* 平板 (≤900px) */
@media (max-width: 900px) {
    .game-container { padding: 18px 16px 22px; }
    .dashboard { grid-template-columns: repeat(4, 1fr); gap: 10px; }
    .gauge { padding: 14px 14px 12px; }
    .gauge .value { font-size: 1.9rem; }
    .param-panel {
        grid-template-columns: repeat(4, 1fr);
        gap: 8px; padding: 12px;
    }
    .param-item .p-value { font-size: 0.85rem; }
    .device-panel { grid-template-columns: 1fr 1fr; gap: 10px; }
}

/* 手机横屏 / 大手机 (≤600px) */
@media (max-width: 600px) {
    body { padding: 8px; }
    .game-container {
        padding: 14px 12px 18px;
        border-radius: 12px;
    }
    .header {
        margin-bottom: 12px;
        gap: 6px;
    }
    .title-wrap { gap: 6px; }
    .title {
        font-size: 1rem; letter-spacing: 1px;
        gap: 6px;
    }
    .title-icon { width: 14px; height: 14px; }
    .help-btn, .music-btn {
        padding: 4px 9px; font-size: 0.6rem;
        border-radius: 5px;
    }
    .status-badge {
        padding: 5px 10px; font-size: 0.65rem;
        border-radius: 30px;
    }
    .status-badge .dot { width: 8px; height: 8px; }

    .dashboard {
        grid-template-columns: repeat(2, 1fr);
        gap: 10px;
        margin-bottom: 12px;
    }
    .gauge {
        padding: 12px 12px 10px;
        border-radius: 10px;
    }
    .gauge .label {
        font-size: 0.62rem; margin-bottom: 6px;
        letter-spacing: 0.5px;
    }
    .gauge .value {
        font-size: 1.6rem;
        letter-spacing: 0;
    }
    .gauge .unit {
        font-size: 0.65rem; margin-top: 3px;
    }
    .gauge .bar-track { margin-top: 8px; height: 4px; }

    .param-panel {
        grid-template-columns: repeat(2, 1fr);
        gap: 6px; padding: 10px;
        margin-bottom: 12px;
    }
    .param-item {
        padding: 4px 2px;
        border-bottom: 1px dashed rgba(48, 54, 61, 0.4);
    }
    .param-item:nth-last-child(-n+1) { border-bottom: none; }
    .param-item .p-label { font-size: 0.5rem; margin-bottom: 3px; }
    .param-item .p-value { font-size: 0.8rem; }

    .panel-row {
        padding: 10px 12px;
        gap: 8px;
        margin-bottom: 8px;
        flex-wrap: wrap;
    }
    .panel-row .row-label {
        font-size: 0.65rem;
        flex-shrink: 0;
    }
    .panel-row .row-value { font-size: 0.85rem; min-width: 42px; }
    .panel-row input[type="range"] { height: 8px; }

    .power-select .ps-btn {
        padding: 7px 16px; font-size: 0.68rem;
        flex: 1; min-width: 0;
    }
    .power-select .ps-info {
        font-size: 0.62rem;
        width: 100%; margin-left: 0;
        text-align: right;
    }

    .relief-control .relief-btn {
        padding: 6px 14px; font-size: 0.65rem;
    }
    .relief-control .relief-status {
        font-size: 0.72rem; min-width: 60px;
    }
    .relief-control span[style*="font-size:0.6rem"] {
        display: none;
    }

    .meltdown-warning {
        padding: 10px 14px;
    }
    .meltdown-warning .label { font-size: 0.8rem; }
    .meltdown-warning .timer { font-size: 1.6rem; }

    .rod-single .rod-lock {
        padding: 6px 12px; font-size: 0.65rem;
    }

    .device-panel {
        grid-template-columns: 1fr;
        gap: 10px;
        margin-bottom: 10px;
    }
    .device-group {
        padding: 10px 12px;
        border-radius: 8px;
    }
    .device-group h4 {
        font-size: 0.62rem; margin-bottom: 8px;
        padding-bottom: 6px;
    }
    .device-row {
        font-size: 0.62rem;
        padding: 6px 0;
        gap: 5px;
    }
    .device-row .dev-btn {
        padding: 4px 10px; font-size: 0.6rem;
        min-width: 32px;
    }
    .device-row .power-slider {
        min-width: 50px; height: 6px;
    }
    .device-row .power-label { font-size: 0.62rem; min-width: 34px; }
    .device-row .cooldown-label { font-size: 0.62rem; min-width: 52px; }
    .device-row .src-btn {
        padding: 3px 8px; font-size: 0.58rem;
        min-width: 30px;
    }

    .terminal-body {
        height: 90px; font-size: 0.65rem;
        padding: 8px 12px;
    }
    .terminal-title { font-size: 0.55rem; letter-spacing: 1px; }

    .controls {
        gap: 6px;
        margin: 6px 0;
    }
    .ctrl-btn {
        padding: 9px 12px; font-size: 0.65rem;
        flex: 1 1 calc(50% - 6px);
        min-width: 0;
        border-radius: 6px;
    }

    .message-area {
        padding: 8px 12px;
        min-height: 36px;
        margin-top: 6px;
    }
    .message-area .msg { font-size: 0.72rem; }

    .modal-body { padding: 16px; }
    .modal-header { padding: 12px 16px; }
    .modal-header h2 { font-size: 0.95rem; }
    .help-section h3 { font-size: 0.82rem; }
    .help-section p, .help-section ul li { font-size: 0.78rem; }
}

/* 小手机 (≤380px) */
@media (max-width: 380px) {
    body { padding: 6px; }
    .game-container {
        padding: 12px 10px 14px;
        border-radius: 10px;
    }
    .title { font-size: 0.9rem; }
    .title-icon { width: 12px; height: 12px; }
    .status-badge { font-size: 0.6rem; padding: 4px 8px; }
    .help-btn, .music-btn { font-size: 0.55rem; padding: 3px 8px; }

    .gauge { padding: 10px 10px 8px; }
    .gauge .value { font-size: 1.35rem; }
    .gauge .label { font-size: 0.55rem; }
    .gauge .unit { font-size: 0.58rem; }

    .param-item .p-value { font-size: 0.7rem; }
    .param-item .p-label { font-size: 0.45rem; }

    .panel-row { padding: 8px 10px; gap: 6px; }
    .panel-row .row-label { font-size: 0.6rem; }
    .panel-row .row-value { font-size: 0.78rem; }

    .power-select .ps-btn { padding: 6px 10px; font-size: 0.6rem; }
    .ctrl-btn { font-size: 0.6rem; padding: 8px 8px; }

    .device-row .dev-btn { padding: 3px 7px; font-size: 0.55rem; }
    .device-row { font-size: 0.58rem; }

    .terminal-body { height: 80px; font-size: 0.6rem; }
}

/* 触摸设备 - 加大滑块拖动区 */
@media (hover: none) and (pointer: coarse) {
    .panel-row input[type="range"] {
        height: 10px;
    }
    .device-row .power-slider {
        height: 8px;
    }
    .ctrl-btn {
        min-height: 42px;
    }
    .ps-btn {
        min-height: 38px;
    }
    .relief-btn {
        min-height: 38px;
    }
    .rod-lock {
        min-height: 36px;
    }
    /* 去掉 hover 效果，避免移动端粘滞 */
    .help-btn:hover, .music-btn:hover { transform: none; }
    .ctrl-btn:hover:not(:disabled) { transform: none; }
}

/* 横屏小高度适配 */
@media (max-height: 500px) and (orientation: landscape) {
    .terminal-body { height: 70px; }
    .gauge { padding: 10px 12px; }
    .gauge .value { font-size: 1.4rem; }
}
</style>
</head>
<body>

<div class="game-container" id="app">
    <div class="header">
        <div class="title-wrap">
            <div class="title"><span class="title-icon"></span>核电站控制台</div>
            <button class="music-btn" id="btnMusic">🔇 音乐</button>
            <button class="help-btn" id="btnHelp">📖 说明书</button>
        </div>
        <div class="status-badge">
            <span class="dot running" id="statusDot"></span>
            <span id="statusText">运行中</span>
        </div>
    </div>

    <div class="dashboard">
        <div class="gauge">
            <div class="label">核心温度</div>
            <div class="value temp" id="tempDisplay">500</div>
            <div class="unit">°C</div>
            <div class="bar-track"><div class="bar-fill temp-bar" id="tempBar" style="width:33%"></div></div>
        </div>
        <div class="gauge">
            <div class="label">电功率</div>
            <div class="value power" id="powerDisplay">0.0</div>
            <div class="unit">MW</div>
            <div class="bar-track"><div class="bar-fill power-bar" id="powerBar" style="width:0%"></div></div>
        </div>
        <div class="gauge">
            <div class="label">累计收益</div>
            <div class="value money" id="moneyDisplay">0.00</div>
            <div class="unit">元 · 无上限</div>
        </div>
        <div class="gauge">
            <div class="label">运行时间</div>
            <div class="value time" id="timeDisplay">0s</div>
            <div class="unit">实时</div>
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

    <div class="panel-row power-select">
        <span class="row-label">供电来源</span>
        <button class="ps-btn main active" id="psMain">主电源</button>
        <button class="ps-btn backup" id="psBackup">备用电源</button>
        <span class="ps-info" id="psInfo">主 0MW / 备 0MW</span>
    </div>

    <div class="panel-row load-control">
        <span class="row-label">电网目标负荷</span>
        <input type="range" class="load-slider" id="loadSlider" min="0" max="200" value="50" step="1">
        <span class="row-value" id="loadValue">50</span>
        <span style="font-size:0.65rem;color:#6e7681;">MW</span>
    </div>

    <div class="panel-row relief-control">
        <span class="row-label">💨 泄压阀</span>
        <button class="relief-btn" id="reliefBtn">立即泄压</button>
        <span class="relief-status" id="reliefStatus">就绪</span>
        <span style="font-size:0.6rem;color:#6e7681;">降1.5MPa / 降温8°C</span>
    </div>

    <div class="meltdown-warning" id="meltdownWarning">
        <span class="label">☢️ 核融倒计时</span>
        <span class="timer" id="meltdownTimer">60</span>
        <span style="font-size:0.85rem;color:#ff5c5c;">秒</span>
    </div>

    <div class="panel-row rod-single">
        <span class="row-label">🛑 控制棒 (512根)</span>
        <input type="range" class="rod-slider" id="rodSlider" min="0" max="100" value="30">
        <span class="row-value" id="rodValue">30%</span>
        <button class="rod-lock" id="rodLock">解锁</button>
    </div>

    <div class="device-panel">
        <div class="device-group">
            <h4>💧 冷却水泵 <span id="pumpSummary">0/4 运行</span></h4>
            <div id="pumpContainer"></div>
        </div>
        <div class="device-group">
            <h4>⚡ 发电机+变压器 <span id="genSummary">0/4 正常</span></h4>
            <div id="genContainer"></div>
            <div style="margin-top:6px;font-size:0.6rem;color:#6e7681;">
                停电: <span id="outageDisplay">无</span> | 水泵供电: <span id="pumpPowerStatus">正常</span>
            </div>
        </div>
    </div>

    <div class="terminal">
        <div class="terminal-header">
            <span class="terminal-dot red"></span>
            <span class="terminal-dot yellow"></span>
            <span class="terminal-dot green"></span>
            <span class="terminal-title">控制终端 / SYSTEM LOG</span>
        </div>
        <div class="terminal-body" id="terminalBody"></div>
    </div>

    <div class="controls">
        <button class="ctrl-btn primary" id="btnPause">⏸ 暂停</button>
        <button class="ctrl-btn danger" id="btnScram">🛑 紧急停堆</button>
        <button class="ctrl-btn success" id="btnRestart" style="display:none;">🚀 启动反应堆</button>
        <button class="ctrl-btn warning" id="btnReset">⟲ 重置</button>
    </div>

    <div class="message-area">
        <span class="msg info" id="message">系统就绪，等待指令...</span>
    </div>
</div>

<div class="modal-overlay" id="helpModal">
    <div class="modal">
        <div class="modal-header">
            <h2>📖 操作手册</h2>
            <button class="modal-close" id="btnCloseHelp">✕</button>
        </div>
        <div class="modal-body">
            <div class="help-section">
                <h3>1. 核电站操作基础</h3>
                <p>控制好核心温度（500°C为正常），不要让它超过 <span class="danger-text">1500°C</span>。发电量越高，收益越高。</p>
                <div class="help-tip"><span class="tip-icon">注意：</span>温度过低（<100°C）会导致冷停堆，无法发电。</div>
            </div>
            <div class="help-section">
                <h3>2. 控制棒操作</h3>
                <ul>
                    <li>控制棒插入越深，反应越慢，温度越低。</li>
                    <li>控制棒全部拔出（0%），温度会快速升高。</li>
                    <li>控制棒锁定后，滑块将被禁用，防止误操作。</li>
                </ul>
            </div>
            <div class="help-section">
                <h3>3. 冷却系统</h3>
                <p>四台冷却水泵，各自有独立的功率滑块（0~100%）。功率越高，冷却效果越强，但耗电量也越大。</p>
            </div>
            <div class="help-section">
                <h3>4. 发电机组</h3>
                <p>每台机组可独立设置功率和输出方向（主电源/备用电源）。当所有机组损坏时，会发生全厂停电。</p>
            </div>
            <div class="help-section">
                <h3>5. 电网调频</h3>
                <p>目标负荷与实际发电的差值会累积为电网偏差。偏差接近0最稳定。偏差过大可能会损坏机组。</p>
            </div>
            <div class="help-section">
                <h3>6. 音乐</h3>
                <p>点击顶部 <span class="key">🎵 音乐</span> 按钮开启工业环境音，会随温度变化紧张度。快捷键 <span class="key">M</span> 切换。</p>
            </div>
            <div class="help-section">
                <h3>7. 快捷键</h3>
                <ul>
                    <li><span class="key">空格</span> / <span class="key">P</span> — 暂停/继续</li>
                    <li><span class="key">R</span> — 重置</li>
                    <li><span class="key">S</span> — 紧急停堆</li>
                    <li><span class="key">V</span> — 泄压</li>
                    <li><span class="key">M</span> — 音乐开关</li>
                    <li><span class="key">Esc</span> — 关闭弹窗</li>
                </ul>
            </div>
        </div>
    </div>
</div>

<div class="modal-overlay" id="scramModal">
    <div class="scram-modal success" id="scramModalContent">
        <span class="scram-icon" id="scramIcon">✅</span>
        <h2 id="scramTitle">紧急停堆成功</h2>
        <p class="scram-desc" id="scramDesc">反应堆已安全关闭，核心温度回落至500°C。</p>
        <button class="scram-btn" id="scramOkBtn">确认</button>
    </div>
</div>

<script>
(() => {
'use strict';
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
const COLD_SHUTDOWN_TEMP = 100;
const HEAT_TO_ELECTRIC = 0.5;
const SCRAM_SUCCESS_RATE = 0.5;
const GRID_SENSITIVITY = 0.002;
const GRID_RANDOM_RANGE = 0.03;
const GRID_DEVIATION_LIMIT = 10;
const GRID_DAMAGE_THRESHOLD = 8;
const GRID_DAMAGE_CHANCE = 0.01;
const GRID_SELF_STABILIZE = 0.2;

let state = {
    time: 0, temperature: 500, reactorPressure: 8.0,
    electricPower: 0, thermalPower: 0, money: 0, totalEnergy: 0,
    rodDepth: ROD_DEFAULT, rodTarget: ROD_DEFAULT, rodLocked: false,
    pumps: [], generators: [], powerOutage: false,
    neutronFlux: 50, backupPower: 100, mainPowerBus: 0, powerSelect: 'main',
    gameOver: false, paused: false, meltdownActive: false,
    meltdownCounter: MELTDOWN_TIME, scramTriggered: false, coldShutdownLogged: false,
    updateTimer: null, rodAnimId: null, targetLoad: 50,
    gridDeviation: 0, reliefCooldown: 0,
};
let wasPausedBeforeHelp = false;
let wasPausedBeforeScramModal = false;

// ===== 音频系统 =====
let audioCtx = null;
let musicPlaying = false;
let musicNodes = null;
let musicMaster = null;
let lastAlarmTime = 0;

function initAudio() {
    if (audioCtx) return true;
    try {
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        return true;
    } catch (e) { return false; }
}

function startMusic() {
    if (!initAudio()) return;
    if (audioCtx.state === 'suspended') audioCtx.resume();
    if (musicPlaying) return;

    musicMaster = audioCtx.createGain();
    musicMaster.gain.value = 0.0001;
    musicMaster.connect(audioCtx.destination);

    const osc1 = audioCtx.createOscillator();
    osc1.type = 'sine'; osc1.frequency.value = 55;
    const gain1 = audioCtx.createGain(); gain1.gain.value = 0.5;
    osc1.connect(gain1); gain1.connect(musicMaster); osc1.start();

    const osc2 = audioCtx.createOscillator();
    osc2.type = 'triangle'; osc2.frequency.value = 110;
    const gain2 = audioCtx.createGain(); gain2.gain.value = 0.12;
    osc2.connect(gain2); gain2.connect(musicMaster); osc2.start();

    const lfo = audioCtx.createOscillator();
    lfo.type = 'sine'; lfo.frequency.value = 0.18;
    const lfoGain = audioCtx.createGain(); lfoGain.gain.value = 0.02;
    lfo.connect(lfoGain); lfoGain.connect(musicMaster.gain); lfo.start();

    const osc3 = audioCtx.createOscillator();
    osc3.type = 'sine'; osc3.frequency.value = 55.3;
    const gain3 = audioCtx.createGain(); gain3.gain.value = 0.25;
    osc3.connect(gain3); gain3.connect(musicMaster); osc3.start();

    musicNodes = { osc1, osc2, osc3, lfo, gain1, gain2, gain3, lfoGain };
    musicPlaying = true;
    musicMaster.gain.cancelScheduledValues(audioCtx.currentTime);
    musicMaster.gain.setValueAtTime(0.0001, audioCtx.currentTime);
    musicMaster.gain.exponentialRampToValueAtTime(0.06, audioCtx.currentTime + 1.5);
}

function stopMusic() {
    if (!musicPlaying || !musicMaster) return;
    const now = audioCtx.currentTime;
    musicMaster.gain.cancelScheduledValues(now);
    musicMaster.gain.setValueAtTime(musicMaster.gain.value, now);
    musicMaster.gain.exponentialRampToValueAtTime(0.0001, now + 0.5);
    const nodes = musicNodes;
    setTimeout(() => {
        try { nodes.osc1.stop(); nodes.osc2.stop(); nodes.osc3.stop(); nodes.lfo.stop(); } catch (e) {}
    }, 600);
    musicNodes = null;
    musicPlaying = false;
}

function toggleMusic() {
    if (musicPlaying) {
        stopMusic();
        btnMusic.classList.remove('playing');
        btnMusic.textContent = '🔇 音乐';
        log('音乐已关闭', 'info');
    } else {
        startMusic();
        btnMusic.classList.add('playing');
        btnMusic.textContent = '🎵 播放中';
        log('音乐已开启 · 工业环境音', 'info');
    }
}

function updateMusic() {
    if (!musicPlaying || !audioCtx || !musicNodes) return;
    const now = audioCtx.currentTime;
    const temp = state.temperature;
    let targetVolume = 0.06, targetFreq = 55;
    if (state.gameOver) { targetVolume = 0.03; targetFreq = 40; }
    else if (state.meltdownActive) { targetVolume = 0.11; targetFreq = 70; }
    else if (temp > 1200) { targetVolume = 0.10; targetFreq = 66; }
    else if (temp > 900) { targetVolume = 0.085; targetFreq = 60; }
    else if (temp < COLD_SHUTDOWN_TEMP) { targetVolume = 0.035; targetFreq = 48; }
    else {
        targetVolume = 0.055 + (temp - 500) / 1500 * 0.02;
        targetFreq = 55 + (temp - 500) / 1000 * 3;
    }
    try {
        musicMaster.gain.cancelScheduledValues(now);
        musicMaster.gain.linearRampToValueAtTime(targetVolume, now + 0.4);
        musicNodes.osc1.frequency.linearRampToValueAtTime(targetFreq, now + 0.4);
        musicNodes.osc3.frequency.linearRampToValueAtTime(targetFreq + 0.3, now + 0.4);
        musicNodes.osc2.frequency.linearRampToValueAtTime(targetFreq * 2, now + 0.4);
    } catch (e) {}
}

function playBeep(freq = 880, duration = 0.08, volume = 0.05) {
    if (!audioCtx) return;
    try {
        const now = audioCtx.currentTime;
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.type = 'square';
        osc.frequency.value = freq;
        gain.gain.setValueAtTime(0, now);
        gain.gain.linearRampToValueAtTime(volume, now + 0.005);
        gain.gain.exponentialRampToValueAtTime(0.0001, now + duration);
        osc.connect(gain); gain.connect(audioCtx.destination);
        osc.start(now); osc.stop(now + duration + 0.02);
    } catch (e) {}
}

function playAlarm() {
    if (!audioCtx) return;
    const now = audioCtx.currentTime;
    if (now - lastAlarmTime < 1.2) return;
    lastAlarmTime = now;
    try {
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.type = 'sawtooth';
        osc.frequency.setValueAtTime(660, now);
        osc.frequency.exponentialRampToValueAtTime(440, now + 0.4);
        gain.gain.setValueAtTime(0, now);
        gain.gain.linearRampToValueAtTime(0.08, now + 0.02);
        gain.gain.exponentialRampToValueAtTime(0.0001, now + 0.5);
        osc.connect(gain); gain.connect(audioCtx.destination);
        osc.start(now); osc.stop(now + 0.55);
    } catch (e) {}
}

const $ = id => document.getElementById(id);
const tempDisplay = $('tempDisplay'), powerDisplay = $('powerDisplay');
const moneyDisplay = $('moneyDisplay'), timeDisplay = $('timeDisplay');
const tempBar = $('tempBar'), powerBar = $('powerBar');
const statusDot = $('statusDot'), statusText = $('statusText');
const messageEl = $('message'), btnPause = $('btnPause'), btnScram = $('btnScram');
const btnRestart = $('btnRestart'), btnReset = $('btnReset');
const pumpContainer = $('pumpContainer'), genContainer = $('genContainer');
const pumpSummary = $('pumpSummary'), genSummary = $('genSummary');
const outageDisplay = $('outageDisplay'), pumpPowerStatus = $('pumpPowerStatus');
const thermalPowerEl = $('thermalPower'), reactorPressureEl = $('reactorPressure');
const mainPowerEl = $('mainPower'), backupPowerEl = $('backupPower');
const reactorStabilityEl = $('reactorStability'), neutronFluxEl = $('neutronFlux');
const gridDeviationEl = $('gridDeviation'), meltdownWarning = $('meltdownWarning');
const meltdownTimer = $('meltdownTimer'), rodSlider = $('rodSlider');
const rodValue = $('rodValue'), rodLock = $('rodLock');
const loadSlider = $('loadSlider'), loadValue = $('loadValue');
const reliefBtn = $('reliefBtn'), reliefStatus = $('reliefStatus');
const psMain = $('psMain'), psBackup = $('psBackup'), psInfo = $('psInfo');
const btnHelp = $('btnHelp'), helpModal = $('helpModal'), btnCloseHelp = $('btnCloseHelp');
const terminalBody = $('terminalBody'), scramModal = $('scramModal');
const scramModalContent = $('scramModalContent'), scramIcon = $('scramIcon');
const scramTitle = $('scramTitle'), scramDesc = $('scramDesc'), scramOkBtn = $('scramOkBtn');
const btnMusic = $('btnMusic');

function clamp(v, min, max) { return Math.max(min, Math.min(max, v)); }
function rand(min, max) { return Math.random() * (max - min) + min; }
function log(text, type = 'info') {
    const d = new Date();
    const t = `${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}:${String(d.getSeconds()).padStart(2,'0')}`;
    const line = document.createElement('div');
    line.className = 'terminal-line ' + type;
    line.textContent = `[${t}] ${text}`;
    terminalBody.appendChild(line);
    terminalBody.scrollTop = terminalBody.scrollHeight;
    while (terminalBody.children.length > 80) terminalBody.removeChild(terminalBody.firstChild);
}
function setMessage(text, type = 'info') {
    messageEl.textContent = text;
    messageEl.className = 'msg ' + type;
}
function updateGridDisplay() {
    gridDeviationEl.textContent = state.gridDeviation.toFixed(2) + '%';
    const abs = Math.abs(state.gridDeviation);
    if (abs < 1) gridDeviationEl.className = 'p-value good';
    else if (abs < 3) gridDeviationEl.className = 'p-value warn';
    else gridDeviationEl.className = 'p-value danger';
}
function initDevices() {
    state.pumps = [];
    for (let i = 0; i < 4; i++) state.pumps.push({ on: true, fault: false, power: 100, repairCooldown: 0 });
    state.generators = [];
    for (let i = 0; i < 4; i++) state.generators.push({ on: true, damaged: false, power: 100, repairCooldown: 0, powerTarget: 'main' });
    state.powerOutage = false; state.rodDepth = ROD_DEFAULT; state.rodTarget = ROD_DEFAULT;
    state.rodLocked = false; state.scramTriggered = false; state.coldShutdownLogged = false;
    if (state.rodAnimId) { cancelAnimationFrame(state.rodAnimId); state.rodAnimId = null; }
    state.gridDeviation = 0; state.targetLoad = 50; state.reliefCooldown = 0;
    state.reactorPressure = 8.0; state.backupPower = 100; state.mainPowerBus = 0;
    state.powerSelect = 'main';
    loadSlider.value = 50; loadValue.textContent = '50';
    rodSlider.value = ROD_DEFAULT; rodValue.textContent = ROD_DEFAULT + '%';
    rodSlider.disabled = false;
}
function animateRod() {
    if (state.rodLocked) { rodValue.textContent = Math.round(state.rodDepth) + '%'; rodSlider.value = Math.round(state.rodDepth); state.rodAnimId = null; return; }
    const diff = state.rodTarget - state.rodDepth;
    if (Math.abs(diff) < 0.3) { state.rodDepth = state.rodTarget; rodValue.textContent = Math.round(state.rodDepth) + '%'; rodSlider.value = Math.round(state.rodDepth); state.rodAnimId = null; return; }
    const step = (ROD_SPEED / 1000) * 16 * Math.sign(diff);
    let nv = state.rodDepth + step;
    if (Math.abs(nv - state.rodTarget) < 0.3) nv = state.rodTarget;
    state.rodDepth = clamp(nv, 0, 100);
    rodValue.textContent = Math.round(state.rodDepth) + '%'; rodSlider.value = Math.round(state.rodDepth);
    state.rodAnimId = requestAnimationFrame(animateRod);
}
function setRodTarget(val) {
    if (state.rodLocked) { rodSlider.value = Math.round(state.rodDepth); rodValue.textContent = Math.round(state.rodDepth) + '%'; setMessage('控制棒已锁定', 'warn'); return; }
    val = clamp(val, 0, 100); state.rodTarget = val;
    if (!state.rodAnimId && Math.abs(state.rodDepth - val) > 0.3) state.rodAnimId = requestAnimationFrame(animateRod);
    else if (Math.abs(state.rodDepth - val) < 0.3) {
        state.rodDepth = val; rodValue.textContent = Math.round(val) + '%'; rodSlider.value = Math.round(val);
        if (state.rodAnimId) { cancelAnimationFrame(state.rodAnimId); state.rodAnimId = null; }
    }
}
function scram() {
    if (state.gameOver) return;
    playBeep(440, 0.15, 0.06);
    if (!state.rodLocked) {
        state.rodTarget = 100; if (!state.rodAnimId) state.rodAnimId = requestAnimationFrame(animateRod);
        setMessage('🛑 紧急停堆！控制棒完全插入', 'danger'); log('手动紧急停堆', 'warn');
    } else setMessage('控制棒已锁定，无法插入', 'warn');
}
function restartReactor() {
    if (state.gameOver) return;
    if (state.temperature >= COLD_SHUTDOWN_TEMP) { setMessage('反应堆尚未冷停堆', 'warn'); return; }
    playBeep(880, 0.1, 0.06);
    state.temperature = 500; state.reactorPressure = 8.0; state.rodDepth = ROD_DEFAULT; state.rodTarget = ROD_DEFAULT;
    rodSlider.value = ROD_DEFAULT; rodValue.textContent = ROD_DEFAULT + '%';
    state.rodLocked = false; rodLock.classList.remove('active'); rodLock.textContent = '解锁'; rodSlider.disabled = false;
    state.coldShutdownLogged = false; state.scramTriggered = false;
    log('反应堆启动，温度回升至500°C', 'success'); setMessage('反应堆已启动', 'success');
    statusDot.className = 'dot running'; statusText.textContent = '运行中';
    renderDevices(); updateUI();
}
function showScramResult(success) {
    if (success) { scramModalContent.className = 'scram-modal success'; scramIcon.textContent = '✅'; scramTitle.textContent = '停堆成功'; scramDesc.textContent = '反应堆已安全关闭，核心温度回落至500°C。'; }
    else { scramModalContent.className = 'scram-modal fail'; scramIcon.textContent = '❌'; scramTitle.textContent = '停堆失败'; scramDesc.textContent = '控制棒卡死！反应堆进入核融状态，60秒倒计时开始。'; }
    wasPausedBeforeScramModal = state.paused;
    if (!state.paused && !state.gameOver) { state.paused = true; btnPause.textContent = '▶ 继续'; }
    scramModal.classList.add('active');
}
function closeScramModal() {
    scramModal.classList.remove('active');
    if (!wasPausedBeforeScramModal && !state.gameOver && !state.meltdownActive) { state.paused = false; btnPause.textContent = '⏸ 暂停'; }
}
function triggerEmergencyScram() {
    state.scramTriggered = true; state.rodLocked = false; rodLock.classList.remove('active'); rodLock.textContent = '解锁'; rodSlider.disabled = false;
    state.rodTarget = 100; if (!state.rodAnimId) state.rodAnimId = requestAnimationFrame(animateRod);
    log('温度达到1500°C，紧急停堆启动', 'danger');
    playAlarm();
    const success = Math.random() < SCRAM_SUCCESS_RATE;
    if (success) {
        log('停堆成功，温度回落', 'success'); setMessage('✅ 停堆成功', 'success');
        statusText.textContent = '安全停堆'; statusDot.className = 'dot running';
        state.temperature = 500; state.rodDepth = 100; state.rodTarget = 100; rodSlider.value = 100; rodValue.textContent = '100%';
        state.reactorPressure = 8.0; state.generators.forEach(g => { if (!g.damaged) g.on = true; });
        state.meltdownActive = false; state.meltdownCounter = MELTDOWN_TIME; meltdownWarning.classList.remove('active');
        setTimeout(() => { if (!state.gameOver) state.scramTriggered = false; }, 5000);
    } else {
        log('停堆失败，核融倒计时开始', 'danger'); setMessage('❌ 停堆失败，核融倒计时60秒', 'danger');
        statusText.textContent = '核融'; statusDot.className = 'dot meltdown';
        state.meltdownActive = true; state.meltdownCounter = MELTDOWN_TIME; meltdownWarning.classList.add('active');
    }
    showScramResult(success);
}
function doRelief() {
    if (state.gameOver || state.paused) return;
    if (state.reliefCooldown > 0) { setMessage(`泄压冷却中 ${Math.ceil(state.reliefCooldown)}s`, 'warn'); return; }
    playBeep(660, 0.1, 0.05);
    state.reactorPressure = clamp(state.reactorPressure - 1.5, PRESSURE_MIN, PRESSURE_MAX);
    state.temperature = clamp(state.temperature - 8, TEMP_MIN, TEMP_MAX);
    state.reliefCooldown = RELIEF_COOLDOWN;
    log('泄压：压力 −1.5 MPa，温度 −8°C', 'info'); setMessage('💨 泄压', 'info');
    updateReliefUI(); updateUI();
}
function updateReliefUI() {
    if (state.reliefCooldown > 0) { reliefBtn.disabled = true; reliefStatus.textContent = `冷却 ${state.reliefCooldown.toFixed(1)}s`; reliefStatus.className = 'relief-status warn'; }
    else {
        reliefBtn.disabled = false;
        if (state.reactorPressure > 15.0) { reliefStatus.textContent = '⚠️ 压力过高'; reliefStatus.className = 'relief-status danger'; }
        else if (state.reactorPressure > 12.0) { reliefStatus.textContent = '压力偏高'; reliefStatus.className = 'relief-status warn'; }
        else { reliefStatus.textContent = '就绪'; reliefStatus.className = 'relief-status'; }
    }
}
function renderDevices() {
    let html = '';
    state.pumps.forEach((p, idx) => {
        const ledClass = p.fault ? 'fault' : (p.on ? 'on' : 'off');
        let statusOrCooldown = '', repairBtn = '';
        if (p.fault) {
            if (p.repairCooldown > 0) { statusOrCooldown = `<span class="cooldown-label">🔧 ${p.repairCooldown.toFixed(1)}s</span>`; repairBtn = `<button class="dev-btn repair" disabled>修复</button>`; }
            else { statusOrCooldown = `<span style="font-size:0.55rem;color:#ff5c5c;">可修复</span>`; repairBtn = `<button class="dev-btn repair ready" data-pump="${idx}" data-action="repair">修复</button>`; }
        } else { statusOrCooldown = `<span style="font-size:0.55rem;color:#8b949e;">${p.on?'运行':'停机'}</span>`; repairBtn = `<button class="dev-btn" disabled style="opacity:0.2;">修复</button>`; }
        html += `<div class="device-row">
            <span>泵${idx+1} <span class="dev-led ${ledClass}"></span></span>
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
        const idx = parseInt(btn.dataset.pump), action = btn.dataset.action;
        if (action === 'toggle') btn.addEventListener('click', () => {
            if (state.powerOutage) { setMessage('停电中', 'warn'); return; }
            if (state.pumps[idx].fault) { setMessage('水泵故障中', 'warn'); return; }
            state.pumps[idx].on = !state.pumps[idx].on;
            playBeep(state.pumps[idx].on ? 880 : 440, 0.05, 0.03);
            renderDevices(); updateUI();
        });
        else if (action === 'repair') btn.addEventListener('click', () => {
            const pump = state.pumps[idx];
            if (!pump.fault || pump.repairCooldown > 0) return;
            pump.fault = false; pump.on = true; pump.repairCooldown = 0; renderDevices(); updateUI();
            playBeep(1200, 0.08, 0.05);
            log(`水泵${idx+1} 已修复`, 'success');
        });
    });
    pumpContainer.querySelectorAll('.power-slider').forEach(slider => {
        slider.addEventListener('input', () => {
            const idx = parseInt(slider.dataset.pump), val = parseInt(slider.value);
            state.pumps[idx].power = clamp(val, 0, 100);
            const label = slider.parentElement.querySelector('.power-label'); if (label) label.textContent = state.pumps[idx].power + '%';
            updateUI();
        });
    });

    let htmlG = '';
    state.generators.forEach((g, idx) => {
        const ledClass = g.damaged ? 'fault' : (g.on ? 'on' : 'off');
        let statusOrCooldown = '', repairBtn = '';
        if (g.damaged) {
            if (g.repairCooldown > 0) { statusOrCooldown = `<span class="cooldown-label">🔧 ${g.repairCooldown.toFixed(1)}s</span>`; repairBtn = `<button class="dev-btn repair" disabled>修复</button>`; }
            else { statusOrCooldown = `<span style="font-size:0.55rem;color:#ff5c5c;">可修复</span>`; repairBtn = `<button class="dev-btn repair ready" data-gen="${idx}" data-action="repair">修复</button>`; }
        } else { statusOrCooldown = `<span style="font-size:0.55rem;color:#8b949e;">${g.on?'运行':'停机'}</span>`; repairBtn = `<button class="dev-btn" disabled style="opacity:0.2;">修复</button>`; }
        const srcMainClass = g.powerTarget === 'main' ? 'src-btn main' : 'src-btn';
        const srcBackupClass = g.powerTarget === 'backup' ? 'src-btn backup' : 'src-btn';
        const srcBtns = g.damaged ? '' : `<button class="${srcMainClass}" data-gen="${idx}" data-action="srcMain">主</button><button class="${srcBackupClass}" data-gen="${idx}" data-action="srcBackup">备</button>`;
        htmlG += `<div class="device-row">
            <span>机组${idx+1} <span class="dev-led ${ledClass}"></span></span>
            ${statusOrCooldown} ${srcBtns}
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
        const idx = parseInt(btn.dataset.gen), action = btn.dataset.action, gen = state.generators[idx];
        if (action === 'toggle') btn.addEventListener('click', () => {
            if (state.powerOutage) { setMessage('停电中', 'warn'); return; }
            if (gen.damaged) { setMessage('机组损坏', 'warn'); return; }
            gen.on = !gen.on;
            playBeep(gen.on ? 880 : 440, 0.05, 0.03);
            renderDevices(); updateUI();
        });
        else if (action === 'repair') btn.addEventListener('click', () => {
            if (!gen.damaged || gen.repairCooldown > 0) return;
            gen.damaged = false; gen.on = true; gen.repairCooldown = 0; renderDevices(); updateUI();
            playBeep(1200, 0.08, 0.05);
            log(`机组${idx+1} 已修复`, 'success'); checkOutage();
        });
        else if (action === 'srcMain') btn.addEventListener('click', () => { if (!gen.damaged) { gen.powerTarget = 'main'; renderDevices(); updateUI(); } });
        else if (action === 'srcBackup') btn.addEventListener('click', () => { if (!gen.damaged) { gen.powerTarget = 'backup'; renderDevices(); updateUI(); } });
    });
    genContainer.querySelectorAll('.power-slider').forEach(slider => {
        slider.addEventListener('input', () => {
            const idx = parseInt(slider.dataset.gen), val = parseInt(slider.value);
            state.generators[idx].power = clamp(val, 0, 100);
            const label = slider.parentElement.querySelector('.power-label'); if (label) label.textContent = state.generators[idx].power + '%';
            updateUI();
        });
    });
    updateDeviceStatus();
}
function checkOutage() {
    const allDamaged = state.generators.every(g => g.damaged);
    if (allDamaged) {
        if (!state.powerOutage) { log('全厂停电', 'danger'); playAlarm(); }
        state.powerOutage = true; state.pumps.forEach(p => { if (!p.fault) p.on = false; });
        setMessage('⚡ 全厂停电', 'danger');
    } else state.powerOutage = false;
    renderDevices(); updateUI();
}
function updateDeviceStatus() {
    const pumpOn = state.pumps.filter(p => p.on && !p.fault).length;
    pumpSummary.textContent = `${pumpOn}/4 运行`;
    const genOk = state.generators.filter(g => !g.damaged).length;
    genSummary.textContent = `${genOk}/4 正常`;
    outageDisplay.textContent = state.powerOutage ? '是' : '无';
    outageDisplay.style.color = state.powerOutage ? '#ff5c5c' : '#4dffc3';
}
function setPowerSelect(sel) {
    state.powerSelect = sel; psMain.classList.toggle('active', sel === 'main'); psBackup.classList.toggle('active', sel === 'backup');
    playBeep(660, 0.06, 0.04);
    log(sel === 'main' ? '切换至主电源' : '切换至备用电源', 'info'); updateUI();
}
function updatePhysics() {
    if (state.gameOver || state.paused) return;
    state.time += 1;
    state.pumps.forEach(p => { if (p.repairCooldown > 0) p.repairCooldown = Math.max(0, p.repairCooldown - 1); });
    state.generators.forEach(g => { if (g.repairCooldown > 0) g.repairCooldown = Math.max(0, g.repairCooldown - 1); });
    if (state.reliefCooldown > 0) state.reliefCooldown = Math.max(0, state.reliefCooldown - 1);

    let coreHeat = BASE_HEAT_RATE * 1.5;
    const rodFactor = 1 - (state.rodDepth / 100) * 0.9;
    coreHeat *= rodFactor; coreHeat = Math.max(coreHeat, 0.1);

    let totalPumpPower = 0;
    for (const pump of state.pumps) if (pump.on && !pump.fault) totalPumpPower += pump.power / 100;
    const pumpDemand = totalPumpPower * PUMP_MAX_MW;
    let pumpPowered = true;
    if (state.powerSelect === 'main') { if (state.mainPowerBus < pumpDemand) pumpPowered = false; }
    else { if (state.backupPower <= 0) pumpPowered = false; }
    if (pumpPowered) coreHeat -= totalPumpPower * MAX_PUMP_POWER;

    coreHeat += (state.temperature - 500) * 0.02;
    state.temperature = clamp(state.temperature + coreHeat, TEMP_MIN, TEMP_MAX);
    const pressureBase = 8.0 + (state.temperature - 500) * 0.006;
    state.reactorPressure += (pressureBase - state.reactorPressure) * 0.05;
    state.reactorPressure = clamp(state.reactorPressure, PRESSURE_MIN, PRESSURE_MAX);
    if (state.temperature < COLD_SHUTDOWN_TEMP) state.thermalPower = 0;
    else state.thermalPower = ((state.temperature - COLD_SHUTDOWN_TEMP) / 400) * 150;
    state.thermalPower = clamp(state.thermalPower, 0, 400);

    let mainGenMW = 0, backupGenMW = 0, totalGenCapacity = 0;
    for (const gen of state.generators) {
        if (gen.on && !gen.damaged) {
            const mw = (gen.power / 100) * GEN_MAX_MW; totalGenCapacity += mw;
            if (gen.powerTarget === 'main') mainGenMW += mw; else backupGenMW += mw;
        }
    }
    state.mainPowerBus = mainGenMW;
    if (state.powerSelect === 'backup' && pumpPowered && pumpDemand > 0) state.backupPower = clamp(state.backupPower - pumpDemand * BACKUP_CHARGE_RATE * 0.05, 0, 100);
    else state.backupPower = clamp(state.backupPower + backupGenMW * BACKUP_CHARGE_RATE * 0.05, 0, 100);

    const maxFromHeat = state.thermalPower * HEAT_TO_ELECTRIC;
    state.electricPower = Math.min(maxFromHeat, totalGenCapacity);
    if (state.powerOutage) state.electricPower = 0;

    const loadDiff = state.electricPower - state.targetLoad;
    const randomWalk = rand(-GRID_RANDOM_RANGE, GRID_RANDOM_RANGE);
    const selfStabilize = -state.gridDeviation * GRID_SELF_STABILIZE;
    state.gridDeviation += loadDiff * GRID_SENSITIVITY + randomWalk + selfStabilize;
    state.gridDeviation = clamp(state.gridDeviation, -GRID_DEVIATION_LIMIT, GRID_DEVIATION_LIMIT);

    const kWhThisSecond = state.electricPower * 1000 / 3600;
    state.totalEnergy += kWhThisSecond; state.money += kWhThisSecond * PRICE_PER_KWH / 1000;
    state.neutronFlux = clamp(20 + (state.temperature / 1500) * 60, 20, 100);

    if (state.reactorPressure > 15.0 && Math.random() < 0.03) {
        const avail = state.pumps.filter(p => !p.fault);
        if (avail.length > 0) {
            const pump = avail[Math.floor(Math.random() * avail.length)];
            pump.fault = true; pump.on = false; pump.repairCooldown = REPAIR_COOLDOWN;
            log(`压力过高，水泵${state.pumps.indexOf(pump)+1}故障`, 'danger'); setMessage(`⚠️ 水泵${state.pumps.indexOf(pump)+1}故障`, 'danger'); renderDevices();
            playAlarm();
        }
    }
    if (state.temperature > 900 && Math.random() < 0.02) {
        const avail = state.generators.filter(g => !g.damaged);
        if (avail.length > 0) {
            const gen = avail[Math.floor(Math.random() * avail.length)];
            gen.damaged = true; gen.on = false; gen.repairCooldown = REPAIR_COOLDOWN;
            log(`高温，机组${state.generators.indexOf(gen)+1}损坏`, 'danger'); setMessage(`⚠️ 机组${state.generators.indexOf(gen)+1}损坏`, 'danger'); checkOutage();
            playAlarm();
        }
    }
    if (Math.abs(state.gridDeviation) > GRID_DAMAGE_THRESHOLD && Math.random() < GRID_DAMAGE_CHANCE) {
        const avail = state.generators.filter(g => !g.damaged);
        if (avail.length > 0) {
            const gen = avail[Math.floor(Math.random() * avail.length)];
            gen.damaged = true; gen.on = false; gen.repairCooldown = REPAIR_COOLDOWN;
            log(`电网异常，机组${state.generators.indexOf(gen)+1}损坏`, 'danger'); setMessage(`⚠️ 机组${state.generators.indexOf(gen)+1}损坏`, 'danger'); checkOutage();
            playAlarm();
        }
    }
    if (!pumpPowered && Math.random() < 0.03) {
        const avail = state.pumps.filter(p => !p.fault && p.on);
        if (avail.length > 0) {
            const pump = avail[Math.floor(Math.random() * avail.length)];
            pump.fault = true; pump.on = false; pump.repairCooldown = REPAIR_COOLDOWN;
            log(`供电不足，水泵${state.pumps.indexOf(pump)+1}故障`, 'danger'); setMessage(`⚠️ 水泵${state.pumps.indexOf(pump)+1}故障`, 'danger'); renderDevices();
        }
    }
    if (state.temperature >= MAX_TEMP && !state.scramTriggered && !state.gameOver) triggerEmergencyScram();
    if (state.meltdownActive) {
        state.meltdownCounter -= 1; meltdownTimer.textContent = state.meltdownCounter;
        if (state.meltdownCounter % 5 === 0) playAlarm();
        if (state.meltdownCounter <= 0) {
            state.gameOver = true; if (state.updateTimer) { clearInterval(state.updateTimer); state.updateTimer = null; }
            setMessage('💥 核融爆炸', 'danger'); log('核融爆炸，游戏结束', 'danger');
            statusDot.className = 'dot meltdown'; statusText.textContent = '爆炸'; meltdownWarning.classList.remove('active');
            playAlarm();
        }
    }
    if (state.temperature < COLD_SHUTDOWN_TEMP && !state.coldShutdownLogged && !state.gameOver) {
        state.coldShutdownLogged = true; log('进入冷停堆，不发电', 'warn'); log('点「启动反应堆」重启', 'info');
    }
    if (state.temperature >= COLD_SHUTDOWN_TEMP) state.coldShutdownLogged = false;
    renderDevices(); updateUI(); updateReliefUI();
    updateMusic();
}
function togglePause() {
    if (state.gameOver) return;
    state.paused = !state.paused; btnPause.textContent = state.paused ? '▶ 继续' : '⏸ 暂停';
    playBeep(660, 0.06, 0.04);
}
function resetGame() {
    if (state.updateTimer) { clearInterval(state.updateTimer); state.updateTimer = null; }
    state.time = 0; state.temperature = 500; state.reactorPressure = 8.0; state.electricPower = 0; state.thermalPower = 0; state.money = 0; state.totalEnergy = 0;
    state.gameOver = false; state.paused = false; state.meltdownActive = false; state.meltdownCounter = MELTDOWN_TIME; state.scramTriggered = false; state.coldShutdownLogged = false;
    state.gridDeviation = 0; state.targetLoad = 50; loadSlider.value = 50; loadValue.textContent = '50';
    meltdownWarning.classList.remove('active'); scramModal.classList.remove('active'); btnPause.textContent = '⏸ 暂停';
    initDevices(); rodSlider.value = ROD_DEFAULT; rodValue.textContent = ROD_DEFAULT + '%'; rodLock.textContent = '解锁'; rodLock.classList.remove('active'); rodSlider.disabled = false;
    psMain.classList.add('active'); psBackup.classList.remove('active'); terminalBody.innerHTML = ''; btnRestart.style.display = 'none';
    renderDevices(); updateUI(); updateReliefUI(); setMessage('已重置', 'info');
    statusDot.className = 'dot running'; statusText.textContent = '运行中'; log('系统重启完成', 'system');
    playBeep(1200, 0.1, 0.05);
    state.updateTimer = setInterval(() => updatePhysics(), UPDATE_INTERVAL);
}
function updateUI() {
    tempDisplay.textContent = Math.round(state.temperature); powerDisplay.textContent = state.electricPower.toFixed(1);
    moneyDisplay.textContent = state.money.toFixed(2); timeDisplay.textContent = Math.round(state.time) + 's';
    tempBar.style.width = clamp((state.temperature / MAX_TEMP) * 100, 0, 100) + '%';
    powerBar.style.width = clamp((state.electricPower / 200) * 100, 0, 100) + '%';
    thermalPowerEl.textContent = state.thermalPower.toFixed(1) + ' MW';
    if (state.temperature < COLD_SHUTDOWN_TEMP) thermalPowerEl.className = 'p-value warn';
    else thermalPowerEl.className = 'p-value' + (state.thermalPower > 300 ? ' danger' : (state.thermalPower > 200 ? ' warn' : ' good'));
    reactorPressureEl.textContent = state.reactorPressure.toFixed(2) + ' MPa';
    if (state.reactorPressure > 15.0) reactorPressureEl.className = 'p-value danger';
    else if (state.reactorPressure > 12.0) reactorPressureEl.className = 'p-value warn';
    else reactorPressureEl.className = 'p-value good';
    mainPowerEl.textContent = state.mainPowerBus.toFixed(1) + ' MW';
    mainPowerEl.className = 'p-value' + (state.mainPowerBus > 30 ? ' good' : (state.mainPowerBus > 10 ? ' warn' : ' danger'));
    backupPowerEl.textContent = Math.round(state.backupPower) + '%';
    if (state.backupPower > 50) backupPowerEl.className = 'p-value good';
    else if (state.backupPower > 20) backupPowerEl.className = 'p-value warn';
    else backupPowerEl.className = 'p-value danger';
    const stab = clamp(100 - Math.abs(state.temperature - 500) * 0.08, 0, 100);
    reactorStabilityEl.textContent = Math.round(stab) + '%';
    reactorStabilityEl.className = 'p-value' + (stab > 60 ? ' good' : (stab > 30 ? ' warn' : ' danger'));
    neutronFluxEl.textContent = Math.round(state.neutronFlux) + '%';
    neutronFluxEl.className = 'p-value' + (state.neutronFlux > 80 ? ' danger' : (state.neutronFlux > 60 ? ' warn' : ' good'));
    updateGridDisplay();

    let mainMW = 0, backupMW = 0;
    for (const g of state.generators) { if (g.on && !g.damaged) { const mw = (g.power / 100) * GEN_MAX_MW; if (g.powerTarget === 'main') mainMW += mw; else backupMW += mw; } }
    psInfo.textContent = `主 ${mainMW.toFixed(0)}MW / 备 ${backupMW.toFixed(0)}MW`;

    const totalPumpPower = state.pumps.filter(p => p.on && !p.fault).reduce((s, p) => s + p.power / 100, 0);
    const pumpDemand = totalPumpPower * PUMP_MAX_MW;
    let pumpPowered = true;
    if (state.powerSelect === 'main') { if (state.mainPowerBus < pumpDemand) pumpPowered = false; }
    else { if (state.backupPower <= 0) pumpPowered = false; }
    if (pumpPowered) { pumpPowerStatus.textContent = '正常'; pumpPowerStatus.style.color = '#4dffc3'; }
    else { pumpPowerStatus.textContent = '不足 ⚠️'; pumpPowerStatus.style.color = '#ff5c5c'; }
    updateDeviceStatus();

    if (state.gameOver) { statusDot.className = 'dot meltdown'; statusText.textContent = '爆炸'; }
    else if (state.meltdownActive) { statusDot.className = 'dot meltdown'; statusText.textContent = '核融'; }
    else if (state.temperature > 1200) { statusDot.className = 'dot danger'; statusText.textContent = '⚠️ 危急'; }
    else if (state.temperature > 900) { statusDot.className = 'dot warning'; statusText.textContent = '⚠️ 高温'; }
    else if (state.temperature < COLD_SHUTDOWN_TEMP) { statusDot.className = 'dot warning'; statusText.textContent = '❄️ 冷停堆'; }
    else { statusDot.className = 'dot running'; statusText.textContent = '运行中'; }

    btnPause.disabled = state.gameOver; btnScram.disabled = state.gameOver;
    btnRestart.style.display = (state.temperature < COLD_SHUTDOWN_TEMP && !state.gameOver) ? '' : 'none';

    if (state.meltdownActive && !state.gameOver) { meltdownWarning.classList.add('active'); meltdownTimer.textContent = state.meltdownCounter; }
    else meltdownWarning.classList.remove('active');
}
function handleKeydown(e) {
    if (e.key === ' ' || e.key === 'p') { e.preventDefault(); togglePause(); }
    if (e.key === 'r' || e.key === 'R') resetGame();
    if (e.key === 's' || e.key === 'S') scram();
    if (e.key === 'v' || e.key === 'V') doRelief();
    if (e.key === 'm' || e.key === 'M') toggleMusic();
    if (e.key === 'Escape') { if (scramModal.classList.contains('active')) closeScramModal(); else closeHelp(); }
}
function openHelp() {
    wasPausedBeforeHelp = state.paused;
    if (!state.gameOver && !state.paused) { state.paused = true; btnPause.textContent = '▶ 继续'; }
    helpModal.classList.add('active');
}
function closeHelp() {
    helpModal.classList.remove('active');
    if (!wasPausedBeforeHelp && !state.gameOver) { state.paused = false; btnPause.textContent = '⏸ 暂停'; }
}
function init() {
    state.temperature = 500; initDevices(); renderDevices(); updateUI(); updateReliefUI(); log('系统启动', 'system');
    btnPause.addEventListener('click', togglePause); btnScram.addEventListener('click', scram);
    btnRestart.addEventListener('click', restartReactor); btnReset.addEventListener('click', resetGame);
    reliefBtn.addEventListener('click', doRelief); psMain.addEventListener('click', () => setPowerSelect('main'));
    psBackup.addEventListener('click', () => setPowerSelect('backup')); btnHelp.addEventListener('click', openHelp);
    btnMusic.addEventListener('click', toggleMusic);
    btnCloseHelp.addEventListener('click', closeHelp); helpModal.addEventListener('click', (e) => { if (e.target === helpModal) closeHelp(); });
    scramOkBtn.addEventListener('click', closeScramModal);
    rodSlider.addEventListener('input', () => setRodTarget(parseInt(rodSlider.value)));
    rodLock.addEventListener('click', () => {
        state.rodLocked = !state.rodLocked; rodLock.textContent = state.rodLocked ? '锁定' : '解锁';
        rodLock.classList.toggle('active', state.rodLocked); rodSlider.disabled = state.rodLocked;
        playBeep(state.rodLocked ? 440 : 880, 0.06, 0.04);
        if (state.rodLocked) { if (state.rodAnimId) { cancelAnimationFrame(state.rodAnimId); state.rodAnimId = null; } rodSlider.value = Math.round(state.rodDepth); rodValue.textContent = Math.round(state.rodDepth) + '%'; }
    });
    loadSlider.addEventListener('input', () => { const val = parseInt(loadSlider.value); state.targetLoad = val; loadValue.textContent = val; });
    document.addEventListener('keydown', handleKeydown);
    state.updateTimer = setInterval(() => updatePhysics(), UPDATE_INTERVAL);
}
init();
})();
</script>
</body>
</html>
