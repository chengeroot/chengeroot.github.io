<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="theme-color" content="#0d1117">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Reactor Rising">
<title>Reactor Rising</title>
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Ccircle cx='50' cy='50' r='40' fill='%2300d4aa'/%3E%3Ccircle cx='50' cy='50' r='15' fill='%23fff'/%3E%3C/svg%3E">
<style>
*{margin:0;padding:0;box-sizing:border-box;user-select:none;-webkit-tap-highlight-color:transparent}
body{background:#0d1117;color:#d0d7de;font-family:'Segoe UI','Microsoft YaHei',sans-serif;min-height:100vh;display:flex;justify-content:center;align-items:flex-start;padding:20px;background-image:radial-gradient(ellipse at 20% 0%,rgba(0,212,170,.08) 0%,transparent 60%),radial-gradient(ellipse at 80% 100%,rgba(255,159,67,.06) 0%,transparent 60%),linear-gradient(180deg,#0d1117 0%,#161b22 100%);overflow-x:hidden}
.game-container{max-width:min(1500px,98vw);width:100%;background:linear-gradient(180deg,rgba(22,27,34,.95) 0%,rgba(13,17,23,.98) 100%);border-radius:18px;padding:28px 32px 32px;border:1px solid rgba(48,54,61,.8);box-shadow:0 20px 60px rgba(0,0,0,.5);position:relative}
.game-container::before{content:'';position:absolute;top:0;left:10%;right:10%;height:2px;background:linear-gradient(90deg,transparent,#00d4aa,#ff9f43,transparent);border-radius:2px;opacity:.7}
.header{display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px 14px;margin-bottom:20px}
.title-wrap{display:flex;align-items:center;gap:12px;flex-wrap:wrap}
.title{font-weight:900;font-size:1.7rem;letter-spacing:2px;color:#f0f6fc;display:flex;align-items:center;gap:12px;text-shadow:0 0 24px rgba(0,212,170,.35);white-space:nowrap}
.title-icon{width:24px;height:24px;position:relative;display:inline-block;flex-shrink:0}
.title-icon::before{content:'';position:absolute;inset:2px;border-radius:50%;background:radial-gradient(circle at 35% 35%,#4dffc3,#00d4aa);box-shadow:0 0 16px rgba(0,212,170,.9)}
.title-icon::after{content:'';position:absolute;inset:-4px;border:2px solid rgba(0,212,170,.6);border-radius:50%;border-top-color:transparent;border-bottom-color:transparent;animation:spin 3s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.help-btn,.music-btn,.install-btn{padding:8px 18px;border-radius:8px;border:1px solid rgba(0,212,170,.35);background:rgba(0,212,170,.08);color:#4dffc3;font-weight:700;font-size:.85rem;cursor:pointer;transition:all .2s;font-family:inherit;white-space:nowrap}
.help-btn:hover,.music-btn:hover,.install-btn:hover{background:rgba(0,212,170,.2);border-color:#00d4aa}
.music-btn.playing{border-color:#ff9f43;color:#ff9f43;background:rgba(255,159,67,.12);animation:musicPulse 1.8s ease-in-out infinite}
@keyframes musicPulse{0%,100%{box-shadow:0 0 8px rgba(255,159,67,.3)}50%{box-shadow:0 0 22px rgba(255,159,67,.6)}}
.difficulty-select{padding:8px 14px;border-radius:8px;border:1px solid rgba(88,166,255,.4);background:rgba(88,166,255,.08);color:#58a6ff;font-weight:700;font-size:.8rem;cursor:pointer;font-family:inherit;outline:none}
.difficulty-select option{background:#161b22;color:#d0d7de}
.status-badge{display:flex;align-items:center;gap:10px;font-size:.95rem;font-weight:700;padding:9px 22px;border-radius:40px;background:rgba(0,212,170,.08);border:1px solid rgba(0,212,170,.3);color:#4dffc3;white-space:nowrap}
.status-badge .dot{width:12px;height:12px;border-radius:50%;animation:pulse-dot 1.4s ease-in-out infinite}
.dot.running{background:#00d4aa;box-shadow:0 0 12px #00d4aa}
.dot.warning{background:#ff9f43;box-shadow:0 0 12px #ff9f43;animation-duration:.8s}
.dot.danger{background:#ff5c5c;box-shadow:0 0 12px #ff5c5c;animation-duration:.4s}
.dot.meltdown{background:#ff1744;box-shadow:0 0 16px #ff1744;animation-duration:.2s}
@keyframes pulse-dot{0%,100%{transform:scale(1);opacity:1}50%{transform:scale(1.5);opacity:.5}}
.dashboard{display:grid;grid-template-columns:repeat(4,1fr);gap:18px;margin-bottom:20px}
.gauge{background:linear-gradient(180deg,rgba(30,36,44,.95) 0%,rgba(18,22,28,.95) 100%);border-radius:14px;padding:22px 24px 20px;border:1px solid rgba(48,54,61,.8);position:relative;overflow:hidden}
.gauge::before{content:'';position:absolute;top:0;left:0;right:0;height:4px;opacity:.9}
.gauge:nth-child(1)::before{background:linear-gradient(90deg,transparent,#ff9f43,transparent)}
.gauge:nth-child(2)::before{background:linear-gradient(90deg,transparent,#00d4aa,transparent)}
.gauge:nth-child(3)::before{background:linear-gradient(90deg,transparent,#ffd740,transparent)}
.gauge:nth-child(4)::before{background:linear-gradient(90deg,transparent,#58a6ff,transparent)}
.gauge .label{font-size:.85rem;font-weight:700;letter-spacing:1.2px;color:#8b949e;margin-bottom:12px}
.gauge .value{font-family:'Consolas',monospace;font-size:2.8rem;font-weight:900;line-height:1;text-shadow:0 0 24px currentColor,0 0 48px currentColor}
.gauge .value.temp{color:#ff9f43}
.gauge .value.power{color:#00d4aa}
.gauge .value.money{color:#ffd740}
.gauge .value.time{color:#58a6ff}
.gauge .unit{font-family:'Consolas',monospace;font-size:.95rem;font-weight:700;color:#6e7681;margin-top:6px}
.gauge .bar-track{width:100%;height:7px;background:rgba(0,0,0,.5);border-radius:4px;margin-top:14px;overflow:hidden;border:1px solid rgba(48,54,61,.5)}
.gauge .bar-fill{height:100%;transition:width .4s}
.bar-fill.temp-bar{background:linear-gradient(90deg,#00d4aa,#ff9f43,#ff5c5c)}
.bar-fill.power-bar{background:linear-gradient(90deg,#ffd740,#ff9f43)}
.param-panel{display:grid;grid-template-columns:repeat(7,1fr);gap:8px;margin-bottom:14px;background:linear-gradient(180deg,rgba(30,36,44,.7) 0%,rgba(18,22,28,.7) 100%);border-radius:12px;padding:16px 20px;border:1px solid rgba(48,54,61,.6)}
.param-item{text-align:center;padding:4px 6px;min-width:0}
.param-item .p-label{font-size:.65rem;font-weight:700;color:#6e7681;margin-bottom:8px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.param-item .p-value{font-family:'Consolas',monospace;font-size:1.15rem;font-weight:700;color:#d0d7de;white-space:nowrap}
.param-item .p-value.good{color:#4dffc3;text-shadow:0 0 10px rgba(77,255,195,.4)}
.param-item .p-value.warn{color:#ff9f43;text-shadow:0 0 10px rgba(255,159,67,.4)}
.param-item .p-value.danger{color:#ff5c5c;text-shadow:0 0 10px rgba(255,92,92,.5)}
.chart-panel{background:linear-gradient(180deg,rgba(30,36,44,.9) 0%,rgba(20,25,31,.9) 100%);border-radius:12px;padding:14px 18px;margin-bottom:14px;border:1px solid rgba(48,54,61,.7)}
.chart-panel .chart-title{font-size:.75rem;color:#8b949e;font-weight:700;margin-bottom:8px;display:flex;justify-content:space-between;align-items:center;letter-spacing:.6px}
.chart-panel .chart-title .chart-legend{font-size:.65rem;color:#6e7681}
.chart-panel .chart-legend span{margin-left:12px}
.chart-panel .chart-legend .legend-danger{color:#ff5c5c}
.chart-panel .chart-legend .legend-warn{color:#ff9f43}
.chart-panel .chart-legend .legend-safe{color:#4dffc3}
.chart-canvas{width:100%;height:100px;display:block;border-radius:8px;background:rgba(0,0,0,.35)}
.panel-row{background:linear-gradient(180deg,rgba(30,36,44,.9) 0%,rgba(20,25,31,.9) 100%);border-radius:12px;padding:16px 22px;margin-bottom:14px;border:1px solid rgba(48,54,61,.7);display:flex;align-items:center;flex-wrap:wrap;gap:16px}
.panel-row .row-label{font-weight:700;font-size:.9rem;color:#8b949e}
.panel-row .row-value{font-family:'Consolas',monospace;font-size:1.15rem;color:#ffd740;min-width:60px;text-align:center;font-weight:700}
.panel-row input[type="range"]{flex:1;min-width:140px;accent-color:#00d4aa;height:8px;cursor:pointer}
.power-select .ps-btn{padding:10px 26px;border-radius:8px;border:1px solid rgba(48,54,61,.8);background:rgba(48,54,61,.4);color:#8b949e;font-weight:700;font-size:.9rem;cursor:pointer;font-family:inherit}
.power-select .ps-btn.active.main{background:linear-gradient(180deg,#00d4aa,#00a888);border-color:#4dffc3;color:#0d1117}
.power-select .ps-btn.active.backup{background:linear-gradient(180deg,#ff9f43,#e0821e);border-color:#ffb866;color:#0d1117}
.power-select .ps-info{font-size:.85rem;color:#6e7681;margin-left:auto}
.relief-control .relief-btn{padding:8px 22px;border-radius:8px;border:1px solid rgba(255,159,67,.5);background:rgba(255,159,67,.12);color:#ff9f43;font-weight:700;font-size:.85rem;cursor:pointer;font-family:inherit}
.relief-control .relief-btn:disabled{opacity:.4;cursor:not-allowed}
.relief-control .relief-status{font-family:'Consolas',monospace;font-size:.95rem;color:#4dffc3;min-width:90px;font-weight:700}
.relief-control .relief-status.warn{color:#ff9f43}
.relief-control .relief-status.danger{color:#ff5c5c}
.boric-control{background:linear-gradient(180deg,rgba(50,30,30,.5) 0%,rgba(30,20,20,.5) 100%);border-radius:12px;padding:16px 22px;margin-bottom:14px;border:1px solid rgba(255,92,92,.4);display:flex;align-items:center;flex-wrap:wrap;gap:16px}
.boric-control .row-label{font-weight:700;font-size:.9rem;color:#ff8585}
.boric-control .boric-btn{padding:8px 22px;border-radius:8px;border:1px solid rgba(255,92,92,.6);background:rgba(255,92,92,.15);color:#ff8585;font-weight:700;font-size:.85rem;cursor:pointer;font-family:inherit}
.boric-control .boric-btn:disabled{opacity:.4;cursor:not-allowed}
.boric-control .boric-btn.pulsing{animation:boricPulse 1s ease-in-out infinite}
@keyframes boricPulse{0%,100%{box-shadow:0 0 0 rgba(255,92,92,0)}50%{box-shadow:0 0 20px rgba(255,92,92,.7)}}
.boric-control .boric-status{font-family:'Consolas',monospace;font-size:.95rem;color:#4dffc3;min-width:120px;font-weight:700}
.boric-control .boric-status.warn{color:#ff9f43}
.boric-control .boric-status.danger{color:#ff5c5c}
.boric-control .boric-status.info{color:#58a6ff}
.meltdown-warning{background:linear-gradient(90deg,rgba(220,38,38,.2),rgba(255,23,68,.15));border:1px solid #ff1744;border-radius:12px;padding:16px 24px;margin-bottom:14px;display:none;justify-content:space-between;align-items:center}
.meltdown-warning.active{display:flex;animation:meltdownPulse 1s ease-in-out infinite}
@keyframes meltdownPulse{0%,100%{box-shadow:0 0 0 rgba(255,23,68,0)}50%{box-shadow:0 0 36px rgba(255,23,68,.5)}}
.meltdown-warning .label{font-weight:900;color:#ff5c5c;font-size:1.15rem;letter-spacing:2px}
.meltdown-warning .timer{font-family:'Consolas',monospace;font-size:2.4rem;font-weight:900;color:#ff1744}
.rod-single .rod-lock{padding:8px 18px;border-radius:8px;border:1px solid rgba(48,54,61,.8);background:rgba(48,54,61,.4);color:#8b949e;font-size:.85rem;cursor:pointer;font-weight:700;font-family:inherit}
.rod-single .rod-lock.active{background:linear-gradient(180deg,#ff5c5c,#d94747);border-color:#ff8585;color:#fff}
.shop-panel{background:linear-gradient(180deg,rgba(30,36,44,.9) 0%,rgba(20,25,31,.9) 100%);border-radius:12px;padding:16px 22px;margin-bottom:14px;border:1px solid rgba(255,215,64,.3)}
.shop-panel h3{font-size:.9rem;color:#ffd740;margin-bottom:12px;display:flex;justify-content:space-between;align-items:center;letter-spacing:.6px}
.shop-panel h3 .shop-money{font-family:'Consolas',monospace;font-size:1rem;color:#ffd740}
.shop-items{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.shop-item{background:rgba(0,0,0,.3);border:1px solid rgba(48,54,61,.6);border-radius:8px;padding:12px 14px;display:flex;flex-direction:column;gap:8px}
.shop-item .item-name{font-size:.8rem;font-weight:700;color:#d0d7de}
.shop-item .item-desc{font-size:.7rem;color:#6e7681;line-height:1.4}
.shop-item .item-buy{padding:7px 14px;border-radius:6px;border:1px solid rgba(255,215,64,.4);background:rgba(255,215,64,.08);color:#ffd740;font-weight:700;font-size:.75rem;cursor:pointer;font-family:inherit}
.shop-item .item-buy:disabled{opacity:.35;cursor:not-allowed;background:rgba(48,54,61,.3);border-color:rgba(48,54,61,.5);color:#6e7681}
.device-panel{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:16px}
.device-group{background:linear-gradient(180deg,rgba(30,36,44,.9) 0%,rgba(20,25,31,.9) 100%);border-radius:12px;padding:18px 20px;border:1px solid rgba(48,54,61,.7);min-width:0}
.device-group h4{font-size:.85rem;font-weight:700;color:#8b949e;margin-bottom:14px;display:flex;justify-content:space-between;border-bottom:1px solid rgba(48,54,61,.5);padding-bottom:12px}
.device-row{display:flex;align-items:center;justify-content:space-between;padding:9px 0;font-size:.78rem;gap:10px;flex-wrap:wrap;border-bottom:1px solid rgba(48,54,61,.3)}
.device-row:last-child{border-bottom:none}
.device-row .dev-led{width:11px;height:11px;border-radius:50%;display:inline-block;margin-right:6px}
.dev-led.on{background:#4dffc3;box-shadow:0 0 10px #4dffc3}
.dev-led.off{background:#484f58}
.dev-led.fault{background:#ff5c5c;box-shadow:0 0 10px #ff5c5c;animation:blink .5s infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.4}}
.device-row .dev-btn{padding:5px 14px;border-radius:5px;border:1px solid rgba(48,54,61,.8);background:rgba(48,54,61,.4);color:#8b949e;font-size:.7rem;cursor:pointer;font-weight:700;font-family:inherit}
.device-row .dev-btn:disabled{opacity:.35;cursor:not-allowed}
.device-row .dev-btn.repair.ready{border-color:#4dffc3;color:#4dffc3;background:rgba(77,255,195,.1)}
.device-row .dev-btn.repair.rush{border-color:#ffd740;color:#ffd740;background:rgba(255,215,64,.1)}
.device-row .power-slider{flex:1;min-width:70px;accent-color:#00d4aa;height:6px;cursor:pointer}
.device-row .power-label{font-family:'Consolas',monospace;font-size:.75rem;color:#ffd740;min-width:42px;text-align:center;font-weight:700}
.device-row .cooldown-label{font-family:'Consolas',monospace;font-size:.75rem;color:#ff5c5c;min-width:58px;text-align:center;font-weight:700}
.device-row .src-btn{padding:4px 12px;border-radius:5px;border:1px solid rgba(48,54,61,.8);background:rgba(48,54,61,.4);color:#8b949e;font-size:.68rem;font-weight:700;cursor:pointer;min-width:38px;font-family:inherit}
.device-row .src-btn.main{background:linear-gradient(180deg,#00d4aa,#00a888);border-color:#4dffc3;color:#0d1117}
.device-row .src-btn.backup{background:linear-gradient(180deg,#ff9f43,#e0821e);border-color:#ffb866;color:#0d1117}
.event-banner{background:linear-gradient(90deg,rgba(255,159,67,.15),rgba(255,159,67,.05));border:1px solid #ff9f43;border-radius:12px;padding:12px 20px;margin-bottom:14px;display:none;align-items:center;gap:12px}
.event-banner.active{display:flex;animation:eventPulse 1.5s ease-in-out infinite}
@keyframes eventPulse{0%,100%{box-shadow:0 0 0 rgba(255,159,67,0)}50%{box-shadow:0 0 24px rgba(255,159,67,.4)}}
.event-banner .ev-icon{font-size:1.5rem}
.event-banner .ev-text{font-weight:700;color:#ff9f43;font-size:.9rem;flex:1}
.event-banner .ev-close{padding:4px 12px;border-radius:6px;border:1px solid rgba(255,159,67,.4);background:transparent;color:#ff9f43;font-size:.75rem;cursor:pointer;font-family:inherit}
.terminal{background:#010409;border:1px solid rgba(48,54,61,.8);border-radius:12px;margin-bottom:16px;overflow:hidden}
.terminal-header{display:flex;align-items:center;gap:8px;padding:10px 18px;background:linear-gradient(180deg,#1c2128,#161b22);border-bottom:1px solid rgba(48,54,61,.8)}
.terminal-dot{width:12px;height:12px;border-radius:50%}
.terminal-dot.red{background:#ff5f56}
.terminal-dot.yellow{background:#ffbd2e}
.terminal-dot.green{background:#27c93f}
.terminal-title{font-family:'Consolas',monospace;font-size:.75rem;color:#6e7681;letter-spacing:1.8px;margin-left:10px;font-weight:700}
.terminal-body{padding:14px 18px;height:160px;overflow-y:auto;font-family:'Consolas',monospace;font-size:.85rem;line-height:1.7;background:#010409}
.terminal-body::-webkit-scrollbar{width:8px}
.terminal-body::-webkit-scrollbar-thumb{background:#30363d;border-radius:4px}
.terminal-line{white-space:pre-wrap;word-break:break-all}
.terminal-line.info{color:#8b949e}
.terminal-line.warn{color:#ff9f43}
.terminal-line.danger{color:#ff5c5c}
.terminal-line.success{color:#4dffc3}
.terminal-line.system{color:#58a6ff;font-weight:700}
.controls{display:flex;gap:12px;justify-content:center;flex-wrap:wrap;margin:12px 0}
.ctrl-btn{padding:12px 30px;border-radius:10px;font-weight:700;font-size:.85rem;border:1px solid rgba(48,54,61,.8);background:linear-gradient(180deg,rgba(48,54,61,.6),rgba(30,36,44,.6));color:#d0d7de;cursor:pointer;text-transform:uppercase;letter-spacing:.6px;font-family:inherit}
.ctrl-btn:disabled{opacity:.35;cursor:not-allowed}
.ctrl-btn.primary{border-color:rgba(88,166,255,.5);color:#58a6ff;background:rgba(88,166,255,.1)}
.ctrl-btn.danger{border-color:rgba(255,92,92,.5);color:#ff8585;background:rgba(255,92,92,.1)}
.ctrl-btn.warning{border-color:rgba(255,159,67,.5);color:#ffb866;background:rgba(255,159,67,.1)}
.ctrl-btn.success{border-color:#4dffc3;color:#4dffc3;background:rgba(77,255,195,.15);animation:restartPulse 1.5s ease-in-out infinite}
@keyframes restartPulse{0%,100%{box-shadow:0 0 0 rgba(77,255,195,0)}50%{box-shadow:0 0 22px rgba(77,255,195,.5)}}
.message-area{margin-top:12px;padding:14px 20px;background:linear-gradient(90deg,rgba(88,166,255,.05),rgba(0,212,170,.05));border-radius:10px;border:1px solid rgba(48,54,61,.6);display:flex;align-items:center;min-height:46px}
.message-area .msg{font-size:.9rem;color:#8b949e;flex:1;font-weight:600}
.message-area .msg.warn{color:#ff9f43}
.message-area .msg.danger{color:#ff5c5c}
.message-area .msg.success{color:#4dffc3}
.message-area .msg.info{color:#58a6ff}
.modal-overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(1,4,9,.9);backdrop-filter:blur(8px);display:none;justify-content:center;align-items:center;z-index:1000;padding:16px}
.modal-overlay.active{display:flex}
.modal{max-width:760px;width:100%;max-height:85vh;background:linear-gradient(180deg,#161b22 0%,#0d1117 100%);border-radius:16px;border:1px solid rgba(48,54,61,.9);display:flex;flex-direction:column;overflow:hidden}
.modal-header{display:flex;justify-content:space-between;align-items:center;padding:20px 26px;background:linear-gradient(180deg,#1c2128,#161b22);border-bottom:1px solid rgba(48,54,61,.8)}
.modal-header h2{font-size:1.3rem;font-weight:700;color:#f0f6fc}
.modal-close{width:40px;height:40px;border-radius:8px;border:1px solid rgba(48,54,61,.8);background:rgba(48,54,61,.4);color:#8b949e;font-size:1.1rem;cursor:pointer;font-family:inherit}
.modal-body{padding:24px 28px;overflow-y:auto;flex:1;line-height:1.8}
.help-section{margin-bottom:26px}
.help-section h3{font-size:1.05rem;font-weight:700;color:#f0f6fc;margin-bottom:14px;padding-bottom:10px;border-bottom:1px solid rgba(48,54,61,.6)}
.help-section p{font-size:.95rem;color:#8b949e;margin-bottom:10px}
.help-section ul{list-style:none;padding-left:4px}
.help-section ul li{font-size:.95rem;color:#8b949e;padding:5px 0 5px 20px;position:relative}
.help-section ul li::before{content:'·';position:absolute;left:4px;color:#00d4aa;font-weight:700;font-size:1.1rem}
.help-section .key{display:inline-block;padding:3px 10px;background:#21262d;border:1px solid #30363d;border-radius:5px;font-family:'Consolas',monospace;font-size:.85rem;color:#f0f6fc}
.help-section .danger-text{color:#ff5c5c;font-weight:700}
.help-section .good-text{color:#4dffc3;font-weight:700}
.help-tip{background:rgba(77,255,195,.05);border:1px solid rgba(77,255,195,.25);border-radius:8px;padding:12px 16px;margin:10px 0;font-size:.9rem;color:#8b949e}
.help-tip .tip-icon{color:#4dffc3;font-weight:700;margin-right:6px}
.scram-modal{max-width:560px;background:linear-gradient(180deg,#161b22 0%,#0d1117 100%);border-radius:16px;border:2px solid;padding:44px 40px;text-align:center}
.scram-modal.success{border-color:#4dffc3}
.scram-modal.fail{border-color:#ff5c5c}
.scram-modal .scram-icon{font-size:5rem;line-height:1;margin-bottom:16px;display:block}
.scram-modal.success .scram-icon{color:#4dffc3}
.scram-modal.fail .scram-icon{color:#ff5c5c}
.scram-modal h2{font-size:2rem;font-weight:900;margin-bottom:12px}
.scram-modal.success h2{color:#4dffc3}
.scram-modal.fail h2{color:#ff5c5c}
.scram-modal .scram-desc{font-size:1.05rem;color:#8b949e;margin-bottom:24px}
.scram-modal .scram-btn{padding:14px 48px;border-radius:10px;font-weight:700;font-size:1rem;cursor:pointer;border:2px solid;text-transform:uppercase;font-family:inherit;margin:4px}
.scram-modal.success .scram-btn{border-color:#4dffc3;background:rgba(77,255,195,.15);color:#4dffc3}
.scram-modal.fail .scram-btn{border-color:#ff5c5c;background:rgba(255,92,92,.15);color:#ff5c5c}
.hotkey-overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(1,4,9,.75);backdrop-filter:blur(6px);display:none;justify-content:center;align-items:center;z-index:900;padding:16px}
.hotkey-overlay.active{display:flex}
.hotkey-card{max-width:520px;width:100%;background:linear-gradient(180deg,rgba(22,27,34,.98),rgba(13,17,23,.98));border:1px solid rgba(0,212,170,.4);border-radius:16px;padding:28px 32px;box-shadow:0 0 60px rgba(0,212,170,.2)}
.hotkey-card h3{font-size:1.1rem;color:#4dffc3;margin-bottom:20px;text-align:center;letter-spacing:1.5px}
.hotkey-list{display:grid;grid-template-columns:1fr;gap:12px}
.hotkey-row{display:flex;justify-content:space-between;align-items:center;padding:8px 0;border-bottom:1px dashed rgba(48,54,61,.4)}
.hotkey-row:last-child{border-bottom:none}
.hotkey-row .hk-key{font-family:'Consolas',monospace;background:rgba(0,212,170,.1);border:1px solid rgba(0,212,170,.3);color:#4dffc3;padding:4px 14px;border-radius:6px;font-size:.85rem;font-weight:700;min-width:60px;text-align:center}
.hotkey-row .hk-desc{color:#d0d7de;font-size:.9rem;font-weight:600}
.hotkey-hint{text-align:center;color:#6e7681;font-size:.75rem;margin-top:16px;font-style:italic}
.score-modal{max-width:600px;width:100%;background:linear-gradient(180deg,#161b22,#0d1117);border-radius:16px;border:2px solid #ffd740;padding:32px;text-align:center;max-height:90vh;overflow-y:auto}
.score-modal h2{font-size:1.6rem;color:#ffd740;margin-bottom:20px;font-weight:900;letter-spacing:1.5px}
.score-canvas{width:100%;max-width:540px;display:block;margin:0 auto 20px;border-radius:10px}
.score-actions{display:flex;gap:10px;justify-content:center;flex-wrap:wrap;margin-top:16px}
.score-btn{padding:10px 26px;border-radius:8px;font-weight:700;font-size:.85rem;cursor:pointer;border:1px solid;font-family:inherit;transition:all .2s}
.score-btn.primary{border-color:#ffd740;background:rgba(255,215,64,.15);color:#ffd740}
.score-btn.primary:hover{background:rgba(255,215,64,.3)}
.score-btn.secondary{border-color:rgba(88,166,255,.5);background:rgba(88,166,255,.1);color:#58a6ff}
.score-btn.secondary:hover{background:rgba(88,166,255,.2)}
@media(max-width:1200px){.game-container{padding:22px 20px 26px}.gauge .value{font-size:2.3rem}.param-item .p-value{font-size:1rem}}
@media(max-width:900px){.game-container{padding:20px 18px 24px}.dashboard{gap:10px}.gauge{padding:14px}.gauge .value{font-size:1.9rem}.param-panel{grid-template-columns:repeat(4,1fr);gap:8px;padding:12px}.device-panel{grid-template-columns:1fr 1fr;gap:10px}.title{font-size:1.3rem}.title-icon{width:18px;height:18px}}
@media(max-width:600px){body{padding:8px}.game-container{padding:14px 12px 18px;border-radius:12px}.header{margin-bottom:12px;gap:6px}.title-wrap{gap:6px}.title{font-size:1rem;letter-spacing:1px;gap:6px}.title-icon{width:14px;height:14px}.help-btn,.music-btn,.install-btn{padding:4px 9px;font-size:.6rem;border-radius:5px}.status-badge{padding:5px 10px;font-size:.65rem}.status-badge .dot{width:8px;height:8px}.difficulty-select{padding:5px 8px;font-size:.65rem}.dashboard{grid-template-columns:repeat(2,1fr);gap:10px;margin-bottom:12px}.gauge{padding:12px}.gauge .label{font-size:.62rem}.gauge .value{font-size:1.6rem}.gauge .unit{font-size:.65rem}.param-panel{grid-template-columns:repeat(2,1fr);gap:6px;padding:10px}.param-item{border-bottom:1px dashed rgba(48,54,61,.4)}.param-item .p-label{font-size:.5rem}.param-item .p-value{font-size:.8rem}.panel-row{padding:10px 12px;gap:8px;flex-wrap:wrap}.panel-row .row-label{font-size:.65rem}.panel-row .row-value{font-size:.85rem;min-width:42px}.power-select .ps-btn{padding:7px 16px;font-size:.68rem;flex:1;min-width:0}.power-select .ps-info{font-size:.62rem;width:100%;margin-left:0;text-align:right}.relief-control .relief-btn{padding:6px 14px;font-size:.65rem}.relief-control .relief-status{font-size:.72rem;min-width:60px}.boric-control{padding:10px 12px;gap:8px}.boric-control .boric-btn{padding:6px 14px;font-size:.65rem}.boric-control .boric-status{font-size:.72rem;min-width:90px}.device-panel{grid-template-columns:1fr;gap:10px}.device-group{padding:10px 12px}.device-group h4{font-size:.62rem}.device-row{font-size:.62rem;padding:6px 0;gap:5px}.device-row .dev-btn{padding:4px 10px;font-size:.6rem;min-width:32px}.shop-items{grid-template-columns:1fr 1fr;gap:8px}.shop-item{padding:8px 10px}.shop-item .item-name{font-size:.68rem}.shop-item .item-desc{font-size:.6rem}.shop-item .item-buy{padding:5px 10px;font-size:.65rem}.terminal-body{height:90px;font-size:.65rem;padding:8px 12px}.terminal-title{font-size:.55rem}.controls{gap:6px;margin:6px 0}.ctrl-btn{padding:9px 12px;font-size:.65rem;flex:1 1 calc(50% - 6px);min-width:0;border-radius:6px}.message-area{padding:8px 12px;min-height:36px}.message-area .msg{font-size:.72rem}.modal-body{padding:16px}.modal-header{padding:12px 16px}.modal-header h2{font-size:.95rem}.help-section h3{font-size:.82rem}.help-section p,.help-section ul li{font-size:.78rem}.scram-modal{padding:30px 24px}.scram-modal h2{font-size:1.4rem}.scram-modal .scram-icon{font-size:3.5rem}.hotkey-card{padding:20px 18px}.hotkey-card h3{font-size:.95rem}.chart-canvas{height:80px}}
@media(max-width:380px){body{padding:6px}.game-container{padding:12px 10px 14px}.title{font-size:.9rem}.gauge .value{font-size:1.35rem}.param-item .p-value{font-size:.7rem}.ctrl-btn{font-size:.6rem;padding:8px}.shop-items{grid-template-columns:1fr}.terminal-body{height:80px;font-size:.6rem}}
@media(hover:none) and (pointer:coarse){.panel-row input[type="range"]{height:12px}.device-row .power-slider{height:8px}.ctrl-btn{min-height:44px}.ps-btn{min-height:40px}.relief-btn,.boric-btn{min-height:40px}.rod-lock{min-height:38px}.help-btn,.music-btn,.install-btn{min-height:34px}}
@media(min-width:1600px){.game-container{max-width:min(1700px,96vw);padding:32px 40px 36px}.dashboard{gap:22px;margin-bottom:24px}.gauge{padding:26px 28px}.gauge .value{font-size:3.2rem}.param-panel{padding:20px 24px}.param-item .p-value{font-size:1.25rem}.panel-row{padding:18px 26px}.device-group{padding:22px 26px}.device-row{font-size:.88rem}.device-row .dev-btn{padding:6px 16px;font-size:.78rem}.terminal-body{height:180px;font-size:.95rem}.ctrl-btn{padding:14px 36px;font-size:.95rem}}
</style>
</head>
<body>

<div class="game-container" id="app">
    <div class="header">
        <div class="title-wrap">
            <div class="title"><span class="title-icon"></span>Reactor Rising</div>
            <select class="difficulty-select" id="difficultySel">
                <option value="easy">🟢 简单</option>
                <option value="normal" selected>🟡 普通</option>
                <option value="hard">🟠 困难</option>
                <option value="hardcore">🔴 硬核</option>
            </select>
            <button class="music-btn" id="btnMusic">🔇 音乐</button>
            <button class="install-btn" id="btnInstall" style="display:none;">📲 安装</button>
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

    <div class="chart-panel">
        <div class="chart-title">
            <span>🌡️ 温度趋势（最近 60 秒）</span>
            <span class="chart-legend">
                <span class="legend-danger">■ 危险 ≥1200°C</span>
                <span class="legend-warn">■ 高温 900°C</span>
                <span class="legend-safe">■ 安全区</span>
            </span>
        </div>
        <canvas class="chart-canvas" id="tempChart"></canvas>
    </div>

    <div class="event-banner" id="eventBanner">
        <span class="ev-icon" id="evIcon">⚠️</span>
        <span class="ev-text" id="evText">事件</span>
        <button class="ev-close" id="evClose">知道了</button>
    </div>

    <div class="shop-panel">
        <h3>💎 商店 <span class="shop-money" id="shopMoney">余额：0.00 元</span></h3>
        <div class="shop-items">
            <div class="shop-item">
                <div class="item-name">💧 第 5 台水泵</div>
                <div class="item-desc">增加一台独立冷却水泵</div>
                <button class="item-buy" id="buyPump5" disabled>购买 50 元</button>
            </div>
            <div class="shop-item">
                <div class="item-name">⚡ 冷却效率升级</div>
                <div class="item-desc">每级 +5% 冷却 · 当前 Lv.<span id="coolLv">1</span></div>
                <button class="item-buy" id="buyCool" disabled>升级 30 元</button>
            </div>
            <div class="shop-item">
                <div class="item-name">🔧 紧急修复</div>
                <div class="item-desc">故障设备旁的黄色按钮</div>
                <button class="item-buy" disabled>每次 10 元</button>
            </div>
        </div>
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
        <span style="font-size:.75rem;color:#6e7681;">MW</span>
    </div>

    <div class="panel-row relief-control">
        <span class="row-label">💨 泄压阀</span>
        <button class="relief-btn" id="reliefBtn">立即泄压</button>
        <span class="relief-status" id="reliefStatus">就绪</span>
        <span style="font-size:.7rem;color:#6e7681;">降1.5MPa / 降温8°C</span>
    </div>

    <div class="boric-control">
        <span class="row-label">☢️ 硼酸注入</span>
        <button class="boric-btn" id="boricBtn">注入硼酸</button>
        <span class="boric-status" id="boricStatus">就绪</span>
        <span style="font-size:.7rem;color:#8b949e;">10秒内降温200°C · 30秒发电减半</span>
    </div>

    <div class="meltdown-warning" id="meltdownWarning">
        <span class="label">☢️ 核融倒计时</span>
        <span class="timer" id="meltdownTimer">60</span>
        <span style="font-size:1rem;color:#ff5c5c;">秒</span>
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
            <div style="margin-top:8px;font-size:.75rem;color:#6e7681;">
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
        <span class="msg info" id="message">系统就绪 · 按 H 查看快捷键</span>
    </div>
</div>

<div class="hotkey-overlay" id="hotkeyOverlay">
    <div class="hotkey-card">
        <h3>⌨️ 快捷键</h3>
        <div class="hotkey-list">
            <div class="hotkey-row"><span class="hk-key">空格</span><span class="hk-desc">暂停 / 继续</span></div>
            <div class="hotkey-row"><span class="hk-key">P</span><span class="hk-desc">暂停 / 继续</span></div>
            <div class="hotkey-row"><span class="hk-key">R</span><span class="hk-desc">重置游戏</span></div>
            <div class="hotkey-row"><span class="hk-key">S</span><span class="hk-desc">紧急停堆</span></div>
            <div class="hotkey-row"><span class="hk-key">V</span><span class="hk-desc">立即泄压</span></div>
            <div class="hotkey-row"><span class="hk-key">B</span><span class="hk-desc">硼酸注入</span></div>
            <div class="hotkey-row"><span class="hk-key">M</span><span class="hk-desc">音乐开关</span></div>
            <div class="hotkey-row"><span class="hk-key">H</span><span class="hk-desc">显示 / 隐藏本面板</span></div>
            <div class="hotkey-row"><span class="hk-key">Esc</span><span class="hk-desc">关闭所有弹窗</span></div>
        </div>
        <div class="hotkey-hint">再按一次 H 或点击空白处关闭</div>
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
                <h3>1. 基础操作</h3>
                <p>核心温度 500°C 正常，别超过 <span class="danger-text">1500°C</span>。温度 <100°C 冷停堆不发电。</p>
            </div>
            <div class="help-section">
                <h3>2. 控制棒</h3>
                <ul><li>插得越深反应越慢，温度越低</li><li>0% 升温最快，100% 完全停堆</li><li>锁定后滑块禁用</li></ul>
            </div>
            <div class="help-section">
                <h3>3. 温度曲线图</h3>
                <p>参数面板下方的曲线显示最近 60 秒温度变化。看到曲线快速上扬就该提前拉控制棒了。</p>
            </div>
            <div class="help-section">
                <h3>4. 硼酸注入</h3>
                <p><strong>应急手段</strong>：10 秒内降温 200°C，但会污染反应堆——后续 30 秒发电效率减半。按钮有 60 秒冷却。</p>
            </div>
            <div class="help-section">
                <h3>5. 冷却水泵 & 发电机组</h3>
                <p>每台独立功率 0-100%。发电机组可选主/备电源，全坏 → 全厂停电。</p>
            </div>
            <div class="help-section">
                <h3>6. 电网调频</h3>
                <p>偏差=发电-负荷。偏差为正降发电，为负增发电。接近 0 最稳。</p>
            </div>
            <div class="help-section">
                <h3>7. 商店系统</h3>
                <ul><li><strong>第5台水泵</strong>：50元</li><li><strong>冷却效率升级</strong>：30元/级，每级+5%</li><li><strong>紧急修复</strong>：10元/次，点故障设备旁的黄色按钮</li></ul>
            </div>
            <div class="help-section">
                <h3>8. 随机事件</h3>
                <ul><li>电网需求突增 / 骤降</li><li>冷却水泄漏（温度+30°C）</li><li>地震警报（2~3 台设备同时故障）</li></ul>
            </div>
            <div class="help-section">
                <h3>9. 难度分级</h3>
                <ul>
                    <li><strong>简单</strong>：故障概率减半</li>
                    <li><strong>普通</strong>：默认</li>
                    <li><strong>困难</strong>：故障概率 x1.5</li>
                    <li><strong>硬核</strong>：故障 x2，无弹窗提示，只能看终端</li>
                </ul>
            </div>
            <div class="help-section">
                <h3>10. 快捷键</h3>
                <p>按 <span class="key">H</span> 随时查看快捷键面板。</p>
            </div>
        </div>
    </div>
</div>

<div class="modal-overlay" id="scramModal">
    <div class="scram-modal success" id="scramModalContent">
        <span class="scram-icon" id="scramIcon">✅</span>
        <h2 id="scramTitle">停堆成功</h2>
        <p class="scram-desc" id="scramDesc">反应堆已安全关闭。</p>
        <button class="scram-btn" id="scramOkBtn">确认</button>
    </div>
</div>

<div class="modal-overlay" id="scoreModal">
    <div class="score-modal">
        <h2>📊 本局成绩</h2>
        <canvas class="score-canvas" id="scoreCanvas" width="540" height="380"></canvas>
        <div class="score-actions">
            <button class="score-btn primary" id="btnSaveScore">💾 保存图片</button>
            <button class="score-btn secondary" id="btnCloseScore">关闭</button>
        </div>
    </div>
</div>

<script>
(() => {
'use strict';
const MAX_TEMP=1500,MELTDOWN_TIME=60,BASE_HEAT_RATE=4.5,ROD_SPEED=10,PRICE_PER_KWH=0.5,UPDATE_INTERVAL=1000;
const MAX_PUMP_POWER=1.23,TEMP_MIN=0,TEMP_MAX=2000,REPAIR_COOLDOWN=12,RELIEF_COOLDOWN=3;
const GEN_MAX_MW=50,PUMP_MAX_MW=4,BACKUP_CHARGE_RATE=1/20,PRESSURE_MIN=4.0,PRESSURE_MAX=20.0;
const ROD_DEFAULT=30,COLD_SHUTDOWN_TEMP=100,HEAT_TO_ELECTRIC=0.5,SCRAM_SUCCESS_RATE=0.5;
const GRID_SENSITIVITY=0.0012,GRID_RANDOM_RANGE=0.015,GRID_DEVIATION_LIMIT=10,GRID_DAMAGE_THRESHOLD=8,GRID_DAMAGE_CHANCE=0.01,GRID_SELF_STABILIZE=0.45,GRID_SNAP_BOOST=3;
const PRICE_PUMP5=50,PRICE_COOL=30,PRICE_RUSH_REPAIR=10;
const BORIC_DURATION=10,BORIC_TOTAL_COOL=200,BORIC_CONTAMINATION=30,BORIC_COOLDOWN=60;
const CHART_SECONDS=60;

const DIFFICULTY={
    easy:{faultMul:0.5,eventMul:0.5,showPopup:true,showAlarm:true,label:'简单'},
    normal:{faultMul:1,eventMul:1,showPopup:true,showAlarm:true,label:'普通'},
    hard:{faultMul:1.5,eventMul:1.5,showPopup:true,showAlarm:true,label:'困难'},
    hardcore:{faultMul:2,eventMul:2,showPopup:false,showAlarm:false,label:'硬核'}
};

let state={
    time:0,temperature:500,reactorPressure:8.0,electricPower:0,thermalPower:0,money:0,totalEnergy:0,
    rodDepth:ROD_DEFAULT,rodTarget:ROD_DEFAULT,rodLocked:false,
    pumps:[],generators:[],powerOutage:false,
    neutronFlux:50,backupPower:100,mainPowerBus:0,powerSelect:'main',
    gameOver:false,paused:false,meltdownActive:false,meltdownCounter:MELTDOWN_TIME,
    scramTriggered:false,coldShutdownLogged:false,
    updateTimer:null,rodAnimId:null,targetLoad:50,gridDeviation:0,reliefCooldown:0,
    pumpCount:4,coolLevel:1,difficulty:'normal',
    nextEventTime:30,activeEvent:null,
    boricActive:false,boricRemaining:0,boricCooldown:0,boricContamination:0,
    tempHistory:[],
    maxTempReached:500
};
let wasPausedBeforeHelp=false,wasPausedBeforeScramModal=false;

let audioCtx=null,musicPlaying=false,musicNodes=null,musicMaster=null,lastAlarmTime=0;
function initAudio(){if(audioCtx)return true;try{audioCtx=new(window.AudioContext||window.webkitAudioContext)();return true}catch(e){return false}}
function startMusic(){
    if(!initAudio())return;if(audioCtx.state==='suspended')audioCtx.resume();if(musicPlaying)return;
    musicMaster=audioCtx.createGain();musicMaster.gain.value=0.0001;musicMaster.connect(audioCtx.destination);
    const osc1=audioCtx.createOscillator();osc1.type='sine';osc1.frequency.value=55;
    const gain1=audioCtx.createGain();gain1.gain.value=0.5;osc1.connect(gain1);gain1.connect(musicMaster);osc1.start();
    const osc2=audioCtx.createOscillator();osc2.type='triangle';osc2.frequency.value=110;
    const gain2=audioCtx.createGain();gain2.gain.value=0.12;osc2.connect(gain2);gain2.connect(musicMaster);osc2.start();
    const lfo=audioCtx.createOscillator();lfo.type='sine';lfo.frequency.value=0.18;
    const lfoGain=audioCtx.createGain();lfoGain.gain.value=0.02;lfo.connect(lfoGain);lfoGain.connect(musicMaster.gain);lfo.start();
    const osc3=audioCtx.createOscillator();osc3.type='sine';osc3.frequency.value=55.3;
    const gain3=audioCtx.createGain();gain3.gain.value=0.25;osc3.connect(gain3);gain3.connect(musicMaster);osc3.start();
    musicNodes={osc1,osc2,osc3,lfo};musicPlaying=true;
    musicMaster.gain.exponentialRampToValueAtTime(0.06,audioCtx.currentTime+1.5);
}
function stopMusic(){
    if(!musicPlaying||!musicMaster)return;const now=audioCtx.currentTime;
    musicMaster.gain.cancelScheduledValues(now);musicMaster.gain.setValueAtTime(musicMaster.gain.value,now);
    musicMaster.gain.exponentialRampToValueAtTime(0.0001,now+0.5);
    const nodes=musicNodes;setTimeout(()=>{try{nodes.osc1.stop();nodes.osc2.stop();nodes.osc3.stop();nodes.lfo.stop()}catch(e){}},600);
    musicNodes=null;musicPlaying=false;
}
function toggleMusic(){
    if(musicPlaying){stopMusic();btnMusic.classList.remove('playing');btnMusic.textContent='🔇 音乐';log('音乐已关闭','info')}
    else{startMusic();btnMusic.classList.add('playing');btnMusic.textContent='🎵 播放中';log('音乐已开启','info')}
}
function updateMusic(){
    if(!musicPlaying||!audioCtx||!musicNodes)return;const now=audioCtx.currentTime;const temp=state.temperature;
    let tv=0.06,tf=55;
    if(state.gameOver){tv=0.03;tf=40}
    else if(state.meltdownActive){tv=0.11;tf=70}
    else if(temp>1200){tv=0.10;tf=66}
    else if(temp>900){tv=0.085;tf=60}
    else if(temp<COLD_SHUTDOWN_TEMP){tv=0.035;tf=48}
    else{tv=0.055+(temp-500)/1500*0.02;tf=55+(temp-500)/1000*3}
    try{
        musicMaster.gain.cancelScheduledValues(now);musicMaster.gain.linearRampToValueAtTime(tv,now+0.4);
        musicNodes.osc1.frequency.linearRampToValueAtTime(tf,now+0.4);
        musicNodes.osc3.frequency.linearRampToValueAtTime(tf+0.3,now+0.4);
        musicNodes.osc2.frequency.linearRampToValueAtTime(tf*2,now+0.4);
    }catch(e){}
}
function playBeep(freq=880,dur=0.08,vol=0.05){
    if(!audioCtx)return;try{
        const now=audioCtx.currentTime;const osc=audioCtx.createOscillator();const gain=audioCtx.createGain();
        osc.type='square';osc.frequency.value=freq;
        gain.gain.setValueAtTime(0,now);gain.gain.linearRampToValueAtTime(vol,now+0.005);
        gain.gain.exponentialRampToValueAtTime(0.0001,now+dur);
        osc.connect(gain);gain.connect(audioCtx.destination);osc.start(now);osc.stop(now+dur+0.02);
    }catch(e){}
}
function playAlarm(){
    if(!audioCtx)return;if(!DIFFICULTY[state.difficulty].showAlarm)return;
    const now=audioCtx.currentTime;if(now-lastAlarmTime<1.2)return;lastAlarmTime=now;
    try{
        const osc=audioCtx.createOscillator();const gain=audioCtx.createGain();
        osc.type='sawtooth';osc.frequency.setValueAtTime(660,now);osc.frequency.exponentialRampToValueAtTime(440,now+0.4);
        gain.gain.setValueAtTime(0,now);gain.gain.linearRampToValueAtTime(0.08,now+0.02);
        gain.gain.exponentialRampToValueAtTime(0.0001,now+0.5);
        osc.connect(gain);gain.connect(audioCtx.destination);osc.start(now);osc.stop(now+0.55);
    }catch(e){}
}

const $=id=>document.getElementById(id);
const tempDisplay=$('tempDisplay'),powerDisplay=$('powerDisplay'),moneyDisplay=$('moneyDisplay'),timeDisplay=$('timeDisplay');
const tempBar=$('tempBar'),powerBar=$('powerBar'),statusDot=$('statusDot'),statusText=$('statusText');
const messageEl=$('message'),btnPause=$('btnPause'),btnScram=$('btnScram'),btnRestart=$('btnRestart'),btnReset=$('btnReset');
const pumpContainer=$('pumpContainer'),genContainer=$('genContainer'),pumpSummary=$('pumpSummary'),genSummary=$('genSummary');
const outageDisplay=$('outageDisplay'),pumpPowerStatus=$('pumpPowerStatus');
const thermalPowerEl=$('thermalPower'),reactorPressureEl=$('reactorPressure'),mainPowerEl=$('mainPower'),backupPowerEl=$('backupPower');
const reactorStabilityEl=$('reactorStability'),neutronFluxEl=$('neutronFlux'),gridDeviationEl=$('gridDeviation');
const meltdownWarning=$('meltdownWarning'),meltdownTimer=$('meltdownTimer'),rodSlider=$('rodSlider'),rodValue=$('rodValue'),rodLock=$('rodLock');
const loadSlider=$('loadSlider'),loadValue=$('loadValue'),reliefBtn=$('reliefBtn'),reliefStatus=$('reliefStatus');
const boricBtn=$('boricBtn'),boricStatus=$('boricStatus');
const psMain=$('psMain'),psBackup=$('psBackup'),psInfo=$('psInfo');
const btnHelp=$('btnHelp'),helpModal=$('helpModal'),btnCloseHelp=$('btnCloseHelp'),btnMusic=$('btnMusic');
const terminalBody=$('terminalBody'),scramModal=$('scramModal'),scramModalContent=$('scramModalContent');
const scramIcon=$('scramIcon'),scramTitle=$('scramTitle'),scramDesc=$('scramDesc'),scramOkBtn=$('scramOkBtn');
const difficultySel=$('difficultySel'),shopMoney=$('shopMoney'),coolLv=$('coolLv');
const buyPump5=$('buyPump5'),buyCool=$('buyCool'),eventBanner=$('eventBanner'),evIcon=$('evIcon'),evText=$('evText'),evClose=$('evClose');
const tempChart=$('tempChart'),hotkeyOverlay=$('hotkeyOverlay');
const scoreModal=$('scoreModal'),scoreCanvas=$('scoreCanvas'),btnSaveScore=$('btnSaveScore'),btnCloseScore=$('btnCloseScore');
const btnInstall=$('btnInstall');

function clamp(v,a,b){return Math.max(a,Math.min(b,v))}
function rand(a,b){return Math.random()*(b-a)+a}
function diff(){return DIFFICULTY[state.difficulty]}
function log(text,type='info'){
    const d=new Date();
    const t=`${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}:${String(d.getSeconds()).padStart(2,'0')}`;
    const line=document.createElement('div');line.className='terminal-line '+type;line.textContent=`[${t}] ${text}`;
    terminalBody.appendChild(line);terminalBody.scrollTop=terminalBody.scrollHeight;
    while(terminalBody.children.length>80)terminalBody.removeChild(terminalBody.firstChild);
}
function setMessage(text,type='info'){messageEl.textContent=text;messageEl.className='msg '+type}
function showEvent(icon,text){
    if(!diff().showPopup){log('[事件] '+text,'warn');return}
    evIcon.textContent=icon;evText.textContent=text;eventBanner.classList.add('active');
    setTimeout(()=>eventBanner.classList.remove('active'),6000);
}
function updateGridDisplay(){
    const d=state.gridDeviation;
    let arrow='';
    if(d>0.15)arrow=' ↑';
    else if(d<-0.15)arrow=' ↓';
    else arrow=' ·';
    gridDeviationEl.textContent=d.toFixed(2)+'%'+arrow;
    const abs=Math.abs(d);
    if(abs<1)gridDeviationEl.className='p-value good';
    else if(abs<3)gridDeviationEl.className='p-value warn';
    else gridDeviationEl.className='p-value danger';
}
function updateShop(){
    shopMoney.textContent='余额：'+state.money.toFixed(2)+' 元';
    coolLv.textContent=state.coolLevel;
    buyPump5.disabled=state.money<PRICE_PUMP5||state.pumpCount>=5;
    if(state.pumpCount>=5){buyPump5.textContent='已拥有'}
    else{buyPump5.textContent=`购买 ${PRICE_PUMP5} 元`}
    buyCool.disabled=state.money<PRICE_COOL;
    buyCool.textContent=`升级 ${PRICE_COOL} 元`;
}
function drawTempChart(){
    const ctx=tempChart.getContext('2d');
    const dpr=window.devicePixelRatio||1;
    const w=tempChart.clientWidth,h=tempChart.clientHeight;
    if(tempChart.width!==w*dpr||tempChart.height!==h*dpr){
        tempChart.width=w*dpr;tempChart.height=h*dpr;
    }
    ctx.setTransform(dpr,0,0,dpr,0,0);
    ctx.clearRect(0,0,w,h);
    ctx.strokeStyle='rgba(48,54,61,0.5)';ctx.lineWidth=1;
    for(let i=0;i<=4;i++){const y=h*i/4;ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(w,y);ctx.stroke()}
    const yMax=1600;
    const y1200=h-(1200/yMax)*h;
    const y900=h-(900/yMax)*h;
    ctx.fillStyle='rgba(255,92,92,0.08)';ctx.fillRect(0,0,w,y1200);
    ctx.fillStyle='rgba(255,159,67,0.05)';ctx.fillRect(0,y1200,w,y900-y1200);
    ctx.strokeStyle='rgba(255,92,92,0.6)';ctx.setLineDash([4,4]);ctx.beginPath();ctx.moveTo(0,y1200);ctx.lineTo(w,y1200);ctx.stroke();
    ctx.strokeStyle='rgba(255,159,67,0.5)';ctx.beginPath();ctx.moveTo(0,y900);ctx.lineTo(w,y900);ctx.stroke();
    ctx.setLineDash([]);
    const history=state.tempHistory;
    if(history.length>1){
        ctx.strokeStyle='#ff9f43';ctx.lineWidth=2;ctx.beginPath();
        for(let i=0;i<history.length;i++){
            const x=(i/(CHART_SECONDS-1))*w;
            const y=h-clamp(history[i]/yMax,0,1)*h;
            if(i===0)ctx.moveTo(x,y);else ctx.lineTo(x,y);
        }
        ctx.stroke();
        const grad=ctx.createLinearGradient(0,0,0,h);
        grad.addColorStop(0,'rgba(255,159,67,0.3)');grad.addColorStop(1,'rgba(255,159,67,0)');
        ctx.fillStyle=grad;
        ctx.lineTo((history.length-1)/(CHART_SECONDS-1)*w,h);ctx.lineTo(0,h);ctx.closePath();ctx.fill();
        const lastX=(history.length-1)/(CHART_SECONDS-1)*w;
        const lastY=h-clamp(history[history.length-1]/yMax,0,1)*h;
        ctx.fillStyle='#ff9f43';ctx.beginPath();ctx.arc(lastX,lastY,4,0,Math.PI*2);ctx.fill();
        ctx.fillStyle='#fff';ctx.beginPath();ctx.arc(lastX,lastY,2,0,Math.PI*2);ctx.fill();
    }
    ctx.font='10px Consolas, monospace';ctx.fillStyle='rgba(255,92,92,0.7)';ctx.fillText('1200°C',4,y1200-3);
    ctx.fillStyle='rgba(255,159,67,0.7)';ctx.fillText('900°C',4,y900-3);
}
function pushTempHistory(){
    state.tempHistory.push(state.temperature);
    if(state.tempHistory.length>CHART_SECONDS)state.tempHistory.shift();
}
function updateBoricUI(){
    if(state.boricCooldown>0){
        boricBtn.disabled=true;
        boricStatus.textContent=`冷却 ${state.boricCooldown}s`;
        boricStatus.className='boric-status warn';
        boricBtn.classList.remove('pulsing');
    }else if(state.boricActive){
        boricBtn.disabled=true;
        boricStatus.textContent=`注入中 ${state.boricRemaining}s`;
        boricStatus.className='boric-status info';
        boricBtn.classList.add('pulsing');
    }else if(state.boricContamination>0){
        boricBtn.disabled=true;
        boricStatus.textContent=`污染 ${state.boricContamination}s`;
        boricStatus.className='boric-status danger';
        boricBtn.classList.remove('pulsing');
    }else{
        boricBtn.disabled=false;
        boricStatus.textContent='就绪';
        boricStatus.className='boric-status';
        boricBtn.classList.remove('pulsing');
    }
}
function doBoricInjection(){
    if(state.gameOver||state.paused)return;
    if(state.boricCooldown>0){setMessage(`硼酸冷却中 ${state.boricCooldown}s`,'warn');return}
    if(state.boricActive){setMessage('硼酸正在注入','warn');return}
    if(state.boricContamination>0){setMessage('反应堆仍在污染中','warn');return}
    playBeep(440,0.2,0.08);
    state.boricActive=true;
    state.boricRemaining=BORIC_DURATION;
    state.boricCooldown=BORIC_COOLDOWN;
    log('☢️ 硼酸注入启动！10秒内降温200°C','danger');
    setMessage('☢️ 硼酸注入中','danger');
    updateBoricUI();
}
function initDevices(){
    state.pumps=[];
    for(let i=0;i<state.pumpCount;i++)state.pumps.push({on:true,fault:false,power:100,repairCooldown:0});
    state.generators=[];
    for(let i=0;i<4;i++)state.generators.push({on:true,damaged:false,power:100,repairCooldown:0,powerTarget:'main'});
    state.powerOutage=false;state.rodDepth=ROD_DEFAULT;state.rodTarget=ROD_DEFAULT;
    state.rodLocked=false;state.scramTriggered=false;state.coldShutdownLogged=false;
    if(state.rodAnimId){cancelAnimationFrame(state.rodAnimId);state.rodAnimId=null}
    state.gridDeviation=0;state.targetLoad=50;state.reliefCooldown=0;
    state.reactorPressure=8.0;state.backupPower=100;state.mainPowerBus=0;
    state.powerSelect='main';
    state.boricActive=false;state.boricRemaining=0;state.boricCooldown=0;state.boricContamination=0;
    state.tempHistory=[];
    for(let i=0;i<CHART_SECONDS;i++)state.tempHistory.push(500);
    loadSlider.value=50;loadValue.textContent='50';
    rodSlider.value=ROD_DEFAULT;rodValue.textContent=ROD_DEFAULT+'%';
    rodSlider.disabled=false;
    state.nextEventTime=30;
}
function animateRod(){
    if(state.rodLocked){rodValue.textContent=Math.round(state.rodDepth)+'%';rodSlider.value=Math.round(state.rodDepth);state.rodAnimId=null;return}
    const diff_=state.rodTarget-state.rodDepth;
    if(Math.abs(diff_)<0.3){state.rodDepth=state.rodTarget;rodValue.textContent=Math.round(state.rodDepth)+'%';rodSlider.value=Math.round(state.rodDepth);state.rodAnimId=null;return}
    const step=(ROD_SPEED/1000)*16*Math.sign(diff_);
    let nv=state.rodDepth+step;if(Math.abs(nv-state.rodTarget)<0.3)nv=state.rodTarget;
    state.rodDepth=clamp(nv,0,100);
    rodValue.textContent=Math.round(state.rodDepth)+'%';rodSlider.value=Math.round(state.rodDepth);
    state.rodAnimId=requestAnimationFrame(animateRod);
}
function setRodTarget(val){
    if(state.rodLocked){rodSlider.value=Math.round(state.rodDepth);rodValue.textContent=Math.round(state.rodDepth)+'%';setMessage('控制棒已锁定','warn');return}
    val=clamp(val,0,100);state.rodTarget=val;
    if(!state.rodAnimId&&Math.abs(state.rodDepth-val)>0.3)state.rodAnimId=requestAnimationFrame(animateRod);
    else if(Math.abs(state.rodDepth-val)<0.3){state.rodDepth=val;rodValue.textContent=Math.round(val)+'%';rodSlider.value=Math.round(val);
        if(state.rodAnimId){cancelAnimationFrame(state.rodAnimId);state.rodAnimId=null}}
}
function scram(){
    if(state.gameOver)return;playBeep(440,0.15,0.06);
    if(!state.rodLocked){state.rodTarget=100;if(!state.rodAnimId)state.rodAnimId=requestAnimationFrame(animateRod);
        setMessage('🛑 紧急停堆','danger');log('手动紧急停堆','warn')}
    else setMessage('控制棒已锁定','warn');
}
function restartReactor(){
    if(state.gameOver)return;if(state.temperature>=COLD_SHUTDOWN_TEMP){setMessage('尚未冷停堆','warn');return}
    playBeep(880,0.1,0.06);
    state.temperature=500;state.reactorPressure=8.0;state.rodDepth=ROD_DEFAULT;state.rodTarget=ROD_DEFAULT;
    rodSlider.value=ROD_DEFAULT;rodValue.textContent=ROD_DEFAULT+'%';
    state.rodLocked=false;rodLock.classList.remove('active');rodLock.textContent='解锁';rodSlider.disabled=false;
    state.coldShutdownLogged=false;state.scramTriggered=false;
    log('反应堆启动','success');setMessage('反应堆已启动','success');
    statusDot.className='dot running';statusText.textContent='运行中';
    renderDevices();updateUI();
}
function showScramResult(success){
    if(success){scramModalContent.className='scram-modal success';scramIcon.textContent='✅';scramTitle.textContent='停堆成功';scramDesc.textContent='反应堆已安全关闭，温度回落至500°C。'}
    else{scramModalContent.className='scram-modal fail';scramIcon.textContent='❌';scramTitle.textContent='停堆失败';scramDesc.textContent='控制棒卡死！核融倒计时60秒。'}
    wasPausedBeforeScramModal=state.paused;
    if(!state.paused&&!state.gameOver){state.paused=true;btnPause.textContent='▶ 继续'}
    scramModal.classList.add('active');
}
// ★ 修复：核融时也要恢复游戏运行，否则倒计时不走
function closeScramModal(){
    scramModal.classList.remove('active');
    if(!wasPausedBeforeScramModal&&!state.gameOver){
        state.paused=false;
        btnPause.textContent='⏸ 暂停';
    }
}
function triggerEmergencyScram(){
    state.scramTriggered=true;state.rodLocked=false;rodLock.classList.remove('active');rodLock.textContent='解锁';rodSlider.disabled=false;
    state.rodTarget=100;if(!state.rodAnimId)state.rodAnimId=requestAnimationFrame(animateRod);
    log('温度达1500°C，紧急停堆启动','danger');playAlarm();
    const success=Math.random()<SCRAM_SUCCESS_RATE;
    if(success){
        log('停堆成功，温度回落','success');setMessage('✅ 停堆成功','success');
        statusText.textContent='安全停堆';statusDot.className='dot running';
        state.temperature=500;state.rodDepth=100;state.rodTarget=100;rodSlider.value=100;rodValue.textContent='100%';
        state.reactorPressure=8.0;state.generators.forEach(g=>{if(!g.damaged)g.on=true});
        state.meltdownActive=false;state.meltdownCounter=MELTDOWN_TIME;meltdownWarning.classList.remove('active');
        setTimeout(()=>{if(!state.gameOver)state.scramTriggered=false},5000);
    }else{
        log('停堆失败，核融倒计时','danger');setMessage('❌ 停堆失败','danger');
        statusText.textContent='核融';statusDot.className='dot meltdown';
        state.meltdownActive=true;state.meltdownCounter=MELTDOWN_TIME;meltdownWarning.classList.add('active');
    }
    showScramResult(success);
}
function doRelief(){
    if(state.gameOver||state.paused)return;
    if(state.reliefCooldown>0){setMessage(`泄压冷却 ${Math.ceil(state.reliefCooldown)}s`,'warn');return}
    playBeep(660,0.1,0.05);
    state.reactorPressure=clamp(state.reactorPressure-1.5,PRESSURE_MIN,PRESSURE_MAX);
    state.temperature=clamp(state.temperature-8,TEMP_MIN,TEMP_MAX);
    state.reliefCooldown=RELIEF_COOLDOWN;
    log('泄压：−1.5MPa，−8°C','info');setMessage('💨 泄压','info');
    updateReliefUI();updateUI();
}
function updateReliefUI(){
    if(state.reliefCooldown>0){reliefBtn.disabled=true;reliefStatus.textContent=`冷却 ${state.reliefCooldown.toFixed(1)}s`;reliefStatus.className='relief-status warn'}
    else{
        reliefBtn.disabled=false;
        if(state.reactorPressure>15.0){reliefStatus.textContent='⚠️ 压力过高';reliefStatus.className='relief-status danger'}
        else if(state.reactorPressure>12.0){reliefStatus.textContent='压力偏高';reliefStatus.className='relief-status warn'}
        else{reliefStatus.textContent='就绪';reliefStatus.className='relief-status'}
    }
}
function renderDevices(){
    let html='';
    state.pumps.forEach((p,idx)=>{
        const ledClass=p.fault?'fault':(p.on?'on':'off');
        let statusOrCooldown='',repairBtn='',rushBtn='';
        if(p.fault){
            if(p.repairCooldown>0){statusOrCooldown=`<span class="cooldown-label">🔧 ${p.repairCooldown.toFixed(1)}s</span>`;
                repairBtn=`<button class="dev-btn repair" disabled>修复</button>`;
                rushBtn=`<button class="dev-btn repair rush" data-pump="${idx}" data-action="rush">⚡10元</button>`}
            else{statusOrCooldown=`<span style="font-size:.65rem;color:#ff5c5c;">可修复</span>`;
                repairBtn=`<button class="dev-btn repair ready" data-pump="${idx}" data-action="repair">修复</button>`}
        }else{statusOrCooldown=`<span style="font-size:.65rem;color:#8b949e;">${p.on?'运行':'停机'}</span>`;
            repairBtn=`<button class="dev-btn" disabled style="opacity:0.2;">修复</button>`}
        html+=`<div class="device-row">
            <span>泵${idx+1} <span class="dev-led ${ledClass}"></span></span>
            ${statusOrCooldown}
            <input type="range" class="power-slider" min="0" max="100" value="${p.power}" data-pump="${idx}" data-action="power" ${p.fault?'disabled':''}>
            <span class="power-label">${p.power}%</span>
            <div>
                <button class="dev-btn ${p.on?'':'success'}" data-pump="${idx}" data-action="toggle" ${p.fault?'disabled':''}>${p.on?'关':'开'}</button>
                ${repairBtn}${rushBtn}
            </div>
        </div>`;
    });
    pumpContainer.innerHTML=html;
    pumpContainer.querySelectorAll('[data-pump]').forEach(btn=>{
        const idx=parseInt(btn.dataset.pump),action=btn.dataset.action;
        if(action==='toggle')btn.addEventListener('click',()=>{
            if(state.powerOutage){setMessage('停电中','warn');return}
            if(state.pumps[idx].fault){setMessage('水泵故障中','warn');return}
            state.pumps[idx].on=!state.pumps[idx].on;playBeep(state.pumps[idx].on?880:440,0.05,0.03);renderDevices();updateUI();
        });
        else if(action==='repair')btn.addEventListener('click',()=>{
            const pump=state.pumps[idx];if(!pump.fault||pump.repairCooldown>0)return;
            pump.fault=false;pump.on=true;pump.repairCooldown=0;renderDevices();updateUI();
            playBeep(1200,0.08,0.05);log(`水泵${idx+1} 已修复`,'success');
        });
        else if(action==='rush')btn.addEventListener('click',()=>{
            const pump=state.pumps[idx];if(!pump.fault)return;
            if(state.money<PRICE_RUSH_REPAIR){setMessage('余额不足','warn');return}
            state.money-=PRICE_RUSH_REPAIR;
            pump.fault=false;pump.on=true;pump.repairCooldown=0;
            playBeep(1400,0.1,0.05);
            log(`⚡ 紧急修复水泵${idx+1}，花费${PRICE_RUSH_REPAIR}元`,'success');
            setMessage(`⚡ 紧急修复水泵${idx+1}`,'success');
            renderDevices();updateUI();updateShop();
        });
    });
    pumpContainer.querySelectorAll('.power-slider').forEach(slider=>{
        slider.addEventListener('input',()=>{
            const idx=parseInt(slider.dataset.pump),val=parseInt(slider.value);
            state.pumps[idx].power=clamp(val,0,100);
            const label=slider.parentElement.querySelector('.power-label');if(label)label.textContent=state.pumps[idx].power+'%';
            updateUI();
        });
    });

    let htmlG='';
    state.generators.forEach((g,idx)=>{
        const ledClass=g.damaged?'fault':(g.on?'on':'off');
        let statusOrCooldown='',repairBtn='',rushBtn='';
        if(g.damaged){
            if(g.repairCooldown>0){statusOrCooldown=`<span class="cooldown-label">🔧 ${g.repairCooldown.toFixed(1)}s</span>`;
                repairBtn=`<button class="dev-btn repair" disabled>修复</button>`;
                rushBtn=`<button class="dev-btn repair rush" data-gen="${idx}" data-action="rush">⚡10元</button>`}
            else{statusOrCooldown=`<span style="font-size:.65rem;color:#ff5c5c;">可修复</span>`;
                repairBtn=`<button class="dev-btn repair ready" data-gen="${idx}" data-action="repair">修复</button>`}
        }else{statusOrCooldown=`<span style="font-size:.65rem;color:#8b949e;">${g.on?'运行':'停机'}</span>`;
            repairBtn=`<button class="dev-btn" disabled style="opacity:0.2;">修复</button>`}
        const sm=g.powerTarget==='main'?'src-btn main':'src-btn';
        const sb=g.powerTarget==='backup'?'src-btn backup':'src-btn';
        const srcBtns=g.damaged?'':`<button class="${sm}" data-gen="${idx}" data-action="srcMain">主</button><button class="${sb}" data-gen="${idx}" data-action="srcBackup">备</button>`;
        htmlG+=`<div class="device-row">
            <span>机组${idx+1} <span class="dev-led ${ledClass}"></span></span>
            ${statusOrCooldown} ${srcBtns}
            <input type="range" class="power-slider" min="0" max="100" value="${g.power}" data-gen="${idx}" data-action="power" ${g.damaged?'disabled':''}>
            <span class="power-label">${g.power}%</span>
            <div>
                <button class="dev-btn ${g.on?'':'success'}" data-gen="${idx}" data-action="toggle" ${g.damaged?'disabled':''}>${g.on?'关':'开'}</button>
                ${repairBtn}${rushBtn}
            </div>
        </div>`;
    });
    genContainer.innerHTML=htmlG;
    genContainer.querySelectorAll('[data-gen]').forEach(btn=>{
        const idx=parseInt(btn.dataset.gen),action=btn.dataset.action,gen=state.generators[idx];
        if(action==='toggle')btn.addEventListener('click',()=>{
            if(state.powerOutage){setMessage('停电中','warn');return}
            if(gen.damaged){setMessage('机组损坏','warn');return}
            gen.on=!gen.on;playBeep(gen.on?880:440,0.05,0.03);renderDevices();updateUI();
        });
        else if(action==='repair')btn.addEventListener('click',()=>{
            if(!gen.damaged||gen.repairCooldown>0)return;
            gen.damaged=false;gen.on=true;gen.repairCooldown=0;renderDevices();updateUI();
            playBeep(1200,0.08,0.05);log(`机组${idx+1} 已修复`,'success');checkOutage();
        });
        else if(action==='rush')btn.addEventListener('click',()=>{
            if(!gen.damaged)return;
            if(state.money<PRICE_RUSH_REPAIR){setMessage('余额不足','warn');return}
            state.money-=PRICE_RUSH_REPAIR;
            gen.damaged=false;gen.on=true;gen.repairCooldown=0;
            playBeep(1400,0.1,0.05);
            log(`⚡ 紧急修复机组${idx+1}，花费${PRICE_RUSH_REPAIR}元`,'success');
            setMessage(`⚡ 紧急修复机组${idx+1}`,'success');
            renderDevices();updateUI();updateShop();checkOutage();
        });
        else if(action==='srcMain')btn.addEventListener('click',()=>{if(!gen.damaged){gen.powerTarget='main';renderDevices();updateUI()}});
        else if(action==='srcBackup')btn.addEventListener('click',()=>{if(!gen.damaged){gen.powerTarget='backup';renderDevices();updateUI()}});
    });
    genContainer.querySelectorAll('.power-slider').forEach(slider=>{
        slider.addEventListener('input',()=>{
            const idx=parseInt(slider.dataset.gen),val=parseInt(slider.value);
            state.generators[idx].power=clamp(val,0,100);
            const label=slider.parentElement.querySelector('.power-label');if(label)label.textContent=state.generators[idx].power+'%';
            updateUI();
        });
    });
    updateDeviceStatus();
}
function checkOutage(){
    const allDamaged=state.generators.every(g=>g.damaged);
    if(allDamaged){if(!state.powerOutage){log('全厂停电','danger');playAlarm()}
        state.powerOutage=true;state.pumps.forEach(p=>{if(!p.fault)p.on=false});
        setMessage('⚡ 全厂停电','danger')}
    else state.powerOutage=false;
    renderDevices();updateUI();
}
function updateDeviceStatus(){
    const pumpOn=state.pumps.filter(p=>p.on&&!p.fault).length;
    pumpSummary.textContent=`${pumpOn}/${state.pumpCount} 运行`;
    const genOk=state.generators.filter(g=>!g.damaged).length;
    genSummary.textContent=`${genOk}/4 正常`;
    outageDisplay.textContent=state.powerOutage?'是':'无';
    outageDisplay.style.color=state.powerOutage?'#ff5c5c':'#4dffc3';
}
function setPowerSelect(sel){
    state.powerSelect=sel;psMain.classList.toggle('active',sel==='main');psBackup.classList.toggle('active',sel==='backup');
    playBeep(660,0.06,0.04);log(sel==='main'?'切换至主电源':'切换至备用电源','info');updateUI();
}
function triggerRandomEvent(){
    const events=[
        ()=>{const add=Math.floor(rand(20,50));state.targetLoad=clamp(state.targetLoad+add,0,200);loadSlider.value=state.targetLoad;loadValue.textContent=state.targetLoad;
            log(`⚡ 电网需求突增 +${add}MW`,'warn');showEvent('⚡',`电网需求突增 +${add}MW`);playAlarm()},
        ()=>{const sub=Math.floor(rand(20,40));state.targetLoad=clamp(state.targetLoad-sub,0,200);loadSlider.value=state.targetLoad;loadValue.textContent=state.targetLoad;
            log(`🔻 电网需求骤降 -${sub}MW`,'warn');showEvent('🔻',`电网需求骤降 -${sub}MW`)},
        ()=>{const add=Math.floor(rand(20,40));state.temperature=clamp(state.temperature+add,TEMP_MIN,TEMP_MAX);
            log(`💧 冷却水泄漏 +${add}°C`,'danger');showEvent('💧',`冷却水泄漏 温度 +${add}°C`);playAlarm()},
        ()=>{const faultCount=Math.floor(rand(2,4));let faulted=[];
            const avail=state.pumps.filter(p=>!p.fault);
            for(let i=0;i<Math.min(faultCount,avail.length);i++){const p=avail[Math.floor(Math.random()*avail.length)];if(p.fault)continue;
                p.fault=true;p.on=false;p.repairCooldown=REPAIR_COOLDOWN;faulted.push('水泵'+(state.pumps.indexOf(p)+1))}
            if(faulted.length){log(`🌊 地震！${faulted.join('、')}故障`,'danger');showEvent('🌊','地震！'+faulted.join('、')+'故障');playAlarm();renderDevices()}}
    ];
    const ev=events[Math.floor(Math.random()*events.length)];try{ev()}catch(e){}
}
function updatePhysics(){
    if(state.gameOver||state.paused)return;
    state.time+=1;
    state.pumps.forEach(p=>{if(p.repairCooldown>0)p.repairCooldown=Math.max(0,p.repairCooldown-1)});
    state.generators.forEach(g=>{if(g.repairCooldown>0)g.repairCooldown=Math.max(0,g.repairCooldown-1)});
    if(state.reliefCooldown>0)state.reliefCooldown=Math.max(0,state.reliefCooldown-1);

    if(state.boricActive){
        state.boricRemaining--;
        state.temperature=clamp(state.temperature-(BORIC_TOTAL_COOL/BORIC_DURATION),TEMP_MIN,TEMP_MAX);
        if(state.boricRemaining<=0){
            state.boricActive=false;
            state.boricContamination=BORIC_CONTAMINATION;
            log('硼酸污染生效！30秒内发电减半','danger');
            setMessage('☢️ 硼酸污染中','danger');
        }
    }
    if(state.boricContamination>0){
        state.boricContamination--;
        if(state.boricContamination<=0){
            log('硼酸污染已清除','success');
            setMessage('硼酸污染已清除','success');
        }
    }
    if(state.boricCooldown>0)state.boricCooldown=Math.max(0,state.boricCooldown-1);
    updateBoricUI();

    pushTempHistory();

    state.nextEventTime-=1;
    if(state.nextEventTime<=0&&!state.gameOver){
        state.nextEventTime=Math.floor(60/diff().eventMul*rand(0.7,1.3));
        if(Math.random()<0.6)triggerRandomEvent();
    }

    let coreHeat=BASE_HEAT_RATE*1.5;
    const rodFactor=1-(state.rodDepth/100)*0.9;
    coreHeat*=rodFactor;coreHeat=Math.max(coreHeat,0.1);

    let totalPumpPower=0;
    for(const pump of state.pumps)if(pump.on&&!pump.fault)totalPumpPower+=pump.power/100;
    const pumpDemand=totalPumpPower*PUMP_MAX_MW;
    let pumpPowered=true;
    if(state.powerSelect==='main'){if(state.mainPowerBus<pumpDemand)pumpPowered=false}
    else{if(state.backupPower<=0)pumpPowered=false}
    if(pumpPowered){
        const coolMul=1+(state.coolLevel-1)*0.05;
        coreHeat-=totalPumpPower*MAX_PUMP_POWER*coolMul;
    }

    coreHeat+=(state.temperature-500)*0.02;
    state.temperature=clamp(state.temperature+coreHeat,TEMP_MIN,TEMP_MAX);
    if(state.temperature>state.maxTempReached)state.maxTempReached=state.temperature;

    const pressureBase=8.0+(state.temperature-500)*0.006;
    state.reactorPressure+=(pressureBase-state.reactorPressure)*0.05;
    state.reactorPressure=clamp(state.reactorPressure,PRESSURE_MIN,PRESSURE_MAX);
    if(state.temperature<COLD_SHUTDOWN_TEMP)state.thermalPower=0;
    else state.thermalPower=((state.temperature-COLD_SHUTDOWN_TEMP)/400)*150;
    state.thermalPower=clamp(state.thermalPower,0,400);

    let mainGenMW=0,backupGenMW=0,totalGenCapacity=0;
    for(const gen of state.generators){
        if(gen.on&&!gen.damaged){const mw=(gen.power/100)*GEN_MAX_MW;totalGenCapacity+=mw;
            if(gen.powerTarget==='main')mainGenMW+=mw;else backupGenMW+=mw}
    }
    state.mainPowerBus=mainGenMW;
    if(state.powerSelect==='backup'&&pumpPowered&&pumpDemand>0)state.backupPower=clamp(state.backupPower-pumpDemand*BACKUP_CHARGE_RATE*0.05,0,100);
    else state.backupPower=clamp(state.backupPower+backupGenMW*BACKUP_CHARGE_RATE*0.05,0,100);

    let maxFromHeat=state.thermalPower*HEAT_TO_ELECTRIC;
    if(state.boricContamination>0)maxFromHeat*=0.5;
    state.electricPower=Math.min(maxFromHeat,totalGenCapacity);
    if(state.powerOutage)state.electricPower=0;

    const loadDiff=state.electricPower-state.targetLoad;
    const randomWalk=rand(-GRID_RANDOM_RANGE,GRID_RANDOM_RANGE);
    const selfStabilize=-state.gridDeviation*GRID_SELF_STABILIZE;
    let delta=loadDiff*GRID_SENSITIVITY+randomWalk+selfStabilize;
    if(Math.abs(state.gridDeviation)<2&&Math.abs(loadDiff)<15){
        delta-=state.gridDeviation*GRID_SNAP_BOOST*0.1;
    }
    state.gridDeviation+=delta;
    state.gridDeviation=clamp(state.gridDeviation,-GRID_DEVIATION_LIMIT,GRID_DEVIATION_LIMIT);

    const kWhThisSecond=state.electricPower*1000/3600;
    state.totalEnergy+=kWhThisSecond;state.money+=kWhThisSecond*PRICE_PER_KWH/1000;
    state.neutronFlux=clamp(20+(state.temperature/1500)*60,20,100);

    const fm=diff().faultMul;
    if(state.reactorPressure>15.0&&Math.random()<0.03*fm){
        const avail=state.pumps.filter(p=>!p.fault);
        if(avail.length>0){const pump=avail[Math.floor(Math.random()*avail.length)];
            pump.fault=true;pump.on=false;pump.repairCooldown=REPAIR_COOLDOWN;
            log(`压力过高，水泵${state.pumps.indexOf(pump)+1}故障`,'danger');
            if(diff().showPopup)setMessage(`⚠️ 水泵${state.pumps.indexOf(pump)+1}故障`,'danger');
            renderDevices();playAlarm()}
    }
    if(state.temperature>900&&Math.random()<0.02*fm){
        const avail=state.generators.filter(g=>!g.damaged);
        if(avail.length>0){const gen=avail[Math.floor(Math.random()*avail.length)];
            gen.damaged=true;gen.on=false;gen.repairCooldown=REPAIR_COOLDOWN;
            log(`高温，机组${state.generators.indexOf(gen)+1}损坏`,'danger');
            if(diff().showPopup)setMessage(`⚠️ 机组${state.generators.indexOf(gen)+1}损坏`,'danger');
            checkOutage();playAlarm()}
    }
    if(Math.abs(state.gridDeviation)>GRID_DAMAGE_THRESHOLD&&Math.random()<GRID_DAMAGE_CHANCE*fm){
        const avail=state.generators.filter(g=>!g.damaged);
        if(avail.length>0){const gen=avail[Math.floor(Math.random()*avail.length)];
            gen.damaged=true;gen.on=false;gen.repairCooldown=REPAIR_COOLDOWN;
            log(`电网异常，机组${state.generators.indexOf(gen)+1}损坏`,'danger');
            if(diff().showPopup)setMessage(`⚠️ 机组${state.generators.indexOf(gen)+1}损坏`,'danger');
            checkOutage();playAlarm()}
    }
    if(!pumpPowered&&Math.random()<0.03*fm){
        const avail=state.pumps.filter(p=>!p.fault&&p.on);
        if(avail.length>0){const pump=avail[Math.floor(Math.random()*avail.length)];
            pump.fault=true;pump.on=false;pump.repairCooldown=REPAIR_COOLDOWN;
            log(`供电不足，水泵${state.pumps.indexOf(pump)+1}故障`,'danger');
            if(diff().showPopup)setMessage(`⚠️ 水泵${state.pumps.indexOf(pump)+1}故障`,'danger');
            renderDevices()}
    }

    if(state.temperature>=MAX_TEMP&&!state.scramTriggered&&!state.gameOver)triggerEmergencyScram();
    if(state.meltdownActive){
        state.meltdownCounter-=1;meltdownTimer.textContent=state.meltdownCounter;
        if(state.meltdownCounter%5===0)playAlarm();
        if(state.meltdownCounter<=0){
            state.gameOver=true;if(state.updateTimer){clearInterval(state.updateTimer);state.updateTimer=null}
            setMessage('💥 核融爆炸','danger');log('核融爆炸，游戏结束','danger');
            statusDot.className='dot meltdown';statusText.textContent='爆炸';meltdownWarning.classList.remove('active');playAlarm();
            showScoreCard(false);
        }
    }
    if(state.temperature<COLD_SHUTDOWN_TEMP&&!state.coldShutdownLogged&&!state.gameOver){
        state.coldShutdownLogged=true;log('进入冷停堆，不发电','warn');log('点「启动反应堆」重启','info');
    }
    if(state.temperature>=COLD_SHUTDOWN_TEMP)state.coldShutdownLogged=false;

    renderDevices();updateUI();updateReliefUI();updateShop();updateMusic();
    drawTempChart();
}
function togglePause(){
    if(state.gameOver)return;
    state.paused=!state.paused;btnPause.textContent=state.paused?'▶ 继续':'⏸ 暂停';playBeep(660,0.06,0.04);
}
function resetGame(){
    if(state.updateTimer){clearInterval(state.updateTimer);state.updateTimer=null}
    state.time=0;state.temperature=500;state.reactorPressure=8.0;state.electricPower=0;state.thermalPower=0;state.money=0;state.totalEnergy=0;
    state.gameOver=false;state.paused=false;state.meltdownActive=false;state.meltdownCounter=MELTDOWN_TIME;
    state.scramTriggered=false;state.coldShutdownLogged=false;
    state.gridDeviation=0;state.targetLoad=50;loadSlider.value=50;loadValue.textContent='50';
    state.pumpCount=4;state.coolLevel=1;
    state.maxTempReached=500;
    meltdownWarning.classList.remove('active');scramModal.classList.remove('active');eventBanner.classList.remove('active');
    btnPause.textContent='⏸ 暂停';
    initDevices();rodSlider.value=ROD_DEFAULT;rodValue.textContent=ROD_DEFAULT+'%';
    rodLock.textContent='解锁';rodLock.classList.remove('active');rodSlider.disabled=false;
    psMain.classList.add('active');psBackup.classList.remove('active');
    terminalBody.innerHTML='';btnRestart.style.display='none';
    renderDevices();updateUI();updateReliefUI();updateShop();setMessage('已重置 · 按 H 查看快捷键','info');
    statusDot.className='dot running';statusText.textContent='运行中';log('系统重启完成','system');
    playBeep(1200,0.1,0.05);
    state.updateTimer=setInterval(()=>updatePhysics(),UPDATE_INTERVAL);
}
function updateUI(){
    tempDisplay.textContent=Math.round(state.temperature);powerDisplay.textContent=state.electricPower.toFixed(1);
    moneyDisplay.textContent=state.money.toFixed(2);timeDisplay.textContent=Math.round(state.time)+'s';
    tempBar.style.width=clamp((state.temperature/MAX_TEMP)*100,0,100)+'%';
    powerBar.style.width=clamp((state.electricPower/200)*100,0,100)+'%';
    thermalPowerEl.textContent=state.thermalPower.toFixed(1)+' MW';
    if(state.temperature<COLD_SHUTDOWN_TEMP)thermalPowerEl.className='p-value warn';
    else thermalPowerEl.className='p-value'+(state.thermalPower>300?' danger':(state.thermalPower>200?' warn':' good'));
    reactorPressureEl.textContent=state.reactorPressure.toFixed(2)+' MPa';
    if(state.reactorPressure>15.0)reactorPressureEl.className='p-value danger';
    else if(state.reactorPressure>12.0)reactorPressureEl.className='p-value warn';
    else reactorPressureEl.className='p-value good';
    mainPowerEl.textContent=state.mainPowerBus.toFixed(1)+' MW';
    mainPowerEl.className='p-value'+(state.mainPowerBus>30?' good':(state.mainPowerBus>10?' warn':' danger'));
    backupPowerEl.textContent=Math.round(state.backupPower)+'%';
    if(state.backupPower>50)backupPowerEl.className='p-value good';
    else if(state.backupPower>20)backupPowerEl.className='p-value warn';
    else backupPowerEl.className='p-value danger';
    const stab=clamp(100-Math.abs(state.temperature-500)*0.08,0,100);
    reactorStabilityEl.textContent=Math.round(stab)+'%';
    reactorStabilityEl.className='p-value'+(stab>60?' good':(stab>30?' warn':' danger'));
    neutronFluxEl.textContent=Math.round(state.neutronFlux)+'%';
    neutronFluxEl.className='p-value'+(state.neutronFlux>80?' danger':(state.neutronFlux>60?' warn':' good'));
    updateGridDisplay();
    let mainMW=0,backupMW=0;
    for(const g of state.generators){if(g.on&&!g.damaged){const mw=(g.power/100)*GEN_MAX_MW;if(g.powerTarget==='main')mainMW+=mw;else backupMW+=mw}}
    psInfo.textContent=`主 ${mainMW.toFixed(0)}MW / 备 ${backupMW.toFixed(0)}MW`;
    const totalPumpPower=state.pumps.filter(p=>p.on&&!p.fault).reduce((s,p)=>s+p.power/100,0);
    const pumpDemand=totalPumpPower*PUMP_MAX_MW;
    let pumpPowered=true;
    if(state.powerSelect==='main'){if(state.mainPowerBus<pumpDemand)pumpPowered=false}
    else{if(state.backupPower<=0)pumpPowered=false}
    if(pumpPowered){pumpPowerStatus.textContent='正常';pumpPowerStatus.style.color='#4dffc3'}
    else{pumpPowerStatus.textContent='不足 ⚠️';pumpPowerStatus.style.color='#ff5c5c'}
    updateDeviceStatus();
    if(state.gameOver){statusDot.className='dot meltdown';statusText.textContent='爆炸'}
    else if(state.meltdownActive){statusDot.className='dot meltdown';statusText.textContent='核融'}
    else if(state.temperature>1200){statusDot.className='dot danger';statusText.textContent='⚠️ 危急'}
    else if(state.temperature>900){statusDot.className='dot warning';statusText.textContent='⚠️ 高温'}
    else if(state.temperature<COLD_SHUTDOWN_TEMP){statusDot.className='dot warning';statusText.textContent='❄️ 冷停堆'}
    else{statusDot.className='dot running';statusText.textContent='运行中'}
    btnPause.disabled=state.gameOver;btnScram.disabled=state.gameOver;
    btnRestart.style.display=(state.temperature<COLD_SHUTDOWN_TEMP&&!state.gameOver)?'':'none';
    if(state.meltdownActive&&!state.gameOver){meltdownWarning.classList.add('active');meltdownTimer.textContent=state.meltdownCounter}
    else meltdownWarning.classList.remove('active');
}
function showScoreCard(victory){
    const ctx=scoreCanvas.getContext('2d');
    const w=scoreCanvas.width,h=scoreCanvas.height;
    const grad=ctx.createLinearGradient(0,0,0,h);
    grad.addColorStop(0,'#0d1117');grad.addColorStop(1,'#161b22');
    ctx.fillStyle=grad;ctx.fillRect(0,0,w,h);
    const accent=victory?'#4dffc3':'#ff5c5c';
    const grad2=ctx.createLinearGradient(0,0,w,0);
    grad2.addColorStop(0,'transparent');grad2.addColorStop(0.5,accent);grad2.addColorStop(1,'transparent');
    ctx.fillStyle=grad2;ctx.fillRect(0,0,w,4);
    ctx.fillStyle=accent;ctx.font='bold 28px Consolas, monospace';ctx.textAlign='center';
    ctx.fillText(victory?'✅ 安全运行':'💥 核融爆炸',w/2,55);
    ctx.fillStyle='#6e7681';ctx.font='14px Consolas, monospace';
    ctx.fillText('Reactor Rising · 本局成绩',w/2,80);
    ctx.strokeStyle='rgba(48,54,61,0.6)';ctx.lineWidth=1;
    ctx.beginPath();ctx.moveTo(40,100);ctx.lineTo(w-40,100);ctx.stroke();
    const stats=[
        {label:'存活时间',value:Math.round(state.time)+' 秒',color:'#58a6ff'},
        {label:'总收益',value:state.money.toFixed(2)+' 元',color:'#ffd740'},
        {label:'最高温度',value:Math.round(state.maxTempReached)+' °C',color:'#ff9f43'},
        {label:'最终温度',value:Math.round(state.temperature)+' °C',color:'#ff9f43'},
        {label:'难度',value:diff().label,color:'#4dffc3'},
        {label:'冷却等级',value:'Lv.'+state.coolLevel,color:'#4dffc3'},
        {label:'水泵数量',value:state.pumpCount+' 台',color:'#4dffc3'},
        {label:'最终收益',value:state.money.toFixed(2)+' 元',color:'#ffd740'}
    ];
    const startY=135,rowH=28;
    ctx.textAlign='left';ctx.font='14px Consolas, monospace';
    stats.forEach((s,i)=>{
        const y=startY+i*rowH;
        ctx.fillStyle='#8b949e';ctx.fillText(s.label,w/2-110,y);
        ctx.fillStyle=s.color;ctx.font='bold 15px Consolas, monospace';
        ctx.textAlign='right';ctx.fillText(s.value,w/2+110,y);
        ctx.textAlign='left';ctx.font='14px Consolas, monospace';
    });
    ctx.textAlign='center';
    ctx.fillStyle='#6e7681';ctx.font='12px Consolas, monospace';
    const now=new Date();
    const timeStr=`${now.getFullYear()}-${String(now.getMonth()+1).padStart(2,'0')}-${String(now.getDate()).padStart(2,'0')} ${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;
    ctx.fillText(timeStr,w/2,h-15);
    scoreModal.classList.add('active');
}
function saveScoreImage(){
    try{
        const url=scoreCanvas.toDataURL('image/png');
        const a=document.createElement('a');
        a.href=url;a.download=`ReactorRising_${Date.now()}.png`;
        document.body.appendChild(a);a.click();document.body.removeChild(a);
        setMessage('成绩卡已保存','success');
    }catch(e){setMessage('保存失败','warn')}
}
function handleKeydown(e){
    if(e.target.tagName==='INPUT'||e.target.tagName==='SELECT')return;
    if(e.key===' '||e.key==='p'||e.key==='P'){e.preventDefault();togglePause()}
    if(e.key==='r'||e.key==='R')resetGame();
    if(e.key==='s'||e.key==='S')scram();
    if(e.key==='v'||e.key==='V')doRelief();
    if(e.key==='b'||e.key==='B')doBoricInjection();
    if(e.key==='m'||e.key==='M')toggleMusic();
    if(e.key==='h'||e.key==='H'){hotkeyOverlay.classList.toggle('active')}
    if(e.key==='Escape'){
        if(hotkeyOverlay.classList.contains('active'))hotkeyOverlay.classList.remove('active');
        else if(scramModal.classList.contains('active'))closeScramModal();
        else if(scoreModal.classList.contains('active'))scoreModal.classList.remove('active');
        else if(helpModal.classList.contains('active'))closeHelp();
    }
}
function openHelp(){wasPausedBeforeHelp=state.paused;
    if(!state.gameOver&&!state.paused){state.paused=true;btnPause.textContent='▶ 继续'}
    helpModal.classList.add('active')}
function closeHelp(){helpModal.classList.remove('active');
    if(!wasPausedBeforeHelp&&!state.gameOver){state.paused=false;btnPause.textContent='⏸ 暂停'}}

function setupPWA(){
    const manifest={
        name:'Reactor Rising',
        short_name:'Reactor Rising',
        description:'实时核反应堆模拟游戏',
        start_url:'.',
        display:'standalone',
        background_color:'#0d1117',
        theme_color:'#00d4aa',
        orientation:'portrait',
        icons:[{
            src:"data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 512 512'%3E%3Crect width='512' height='512' fill='%230d1117'/%3E%3Ccircle cx='256' cy='256' r='140' fill='none' stroke='%2300d4aa' stroke-width='16'/%3E%3Ccircle cx='256' cy='256' r='60' fill='%2300d4aa'/%3E%3Ccircle cx='256' cy='256' r='20' fill='%23fff'/%3E%3C/svg%3E",
            sizes:'512x512',
            type:'image/svg+xml',
            purpose:'any maskable'
        }]
    };
    const manifestUrl='data:application/manifest+json;charset=utf-8,'+encodeURIComponent(JSON.stringify(manifest));
    let link=document.querySelector('link[rel="manifest"]');
    if(!link){link=document.createElement('link');link.rel='manifest';document.head.appendChild(link)}
    link.href=manifestUrl;
    let deferredPrompt=null;
    window.addEventListener('beforeinstallprompt',(e)=>{
        e.preventDefault();
        deferredPrompt=e;
        btnInstall.style.display='';
    });
    btnInstall.addEventListener('click',async()=>{
        if(!deferredPrompt){setMessage('当前环境不支持安装，请用浏览器菜单添加到主屏幕','warn');return}
        deferredPrompt.prompt();
        const {outcome}=await deferredPrompt.userChoice;
        if(outcome==='accepted'){log('PWA 已安装','success');setMessage('✅ 已添加到主屏幕','success')}
        deferredPrompt=null;
        btnInstall.style.display='none';
    });
    window.addEventListener('appinstalled',()=>{btnInstall.style.display='none';log('PWA 安装成功','success')});
}

function init(){
    state.temperature=500;initDevices();renderDevices();updateUI();updateReliefUI();updateShop();updateBoricUI();
    log('系统启动','system');
    log(`难度：${diff().label}`,'system');
    log('按 H 查看所有快捷键','system');

    btnPause.addEventListener('click',togglePause);
    btnScram.addEventListener('click',scram);
    btnRestart.addEventListener('click',restartReactor);
    btnReset.addEventListener('click',resetGame);
    reliefBtn.addEventListener('click',doRelief);
    boricBtn.addEventListener('click',doBoricInjection);
    psMain.addEventListener('click',()=>setPowerSelect('main'));
    psBackup.addEventListener('click',()=>setPowerSelect('backup'));
    btnHelp.addEventListener('click',openHelp);
    btnMusic.addEventListener('click',toggleMusic);
    btnCloseHelp.addEventListener('click',closeHelp);
    helpModal.addEventListener('click',(e)=>{if(e.target===helpModal)closeHelp()});
    scramOkBtn.addEventListener('click',closeScramModal);
    evClose.addEventListener('click',()=>eventBanner.classList.remove('active'));
    hotkeyOverlay.addEventListener('click',(e)=>{if(e.target===hotkeyOverlay)hotkeyOverlay.classList.remove('active')});
    btnSaveScore.addEventListener('click',saveScoreImage);
    btnCloseScore.addEventListener('click',()=>scoreModal.classList.remove('active'));
    rodSlider.addEventListener('input',()=>setRodTarget(parseInt(rodSlider.value)));
    rodLock.addEventListener('click',()=>{
        state.rodLocked=!state.rodLocked;rodLock.textContent=state.rodLocked?'锁定':'解锁';
        rodLock.classList.toggle('active',state.rodLocked);rodSlider.disabled=state.rodLocked;
        playBeep(state.rodLocked?440:880,0.06,0.04);
        if(state.rodLocked){if(state.rodAnimId){cancelAnimationFrame(state.rodAnimId);state.rodAnimId=null}
            rodSlider.value=Math.round(state.rodDepth);rodValue.textContent=Math.round(state.rodDepth)+'%'}
    });
    loadSlider.addEventListener('input',()=>{const val=parseInt(loadSlider.value);state.targetLoad=val;loadValue.textContent=val});
    difficultySel.addEventListener('change',()=>{
        state.difficulty=difficultySel.value;
        log(`难度切换至：${diff().label}`,'system');
        setMessage(`难度：${diff().label}`,'info');
    });
    buyPump5.addEventListener('click',()=>{
        if(state.pumpCount>=5){setMessage('已有5台水泵','warn');return}
        if(state.money<PRICE_PUMP5){setMessage('余额不足','warn');return}
        state.money-=PRICE_PUMP5;state.pumpCount=5;
        state.pumps.push({on:true,fault:false,power:100,repairCooldown:0});
        playBeep(1400,0.15,0.06);
        log(`💰 购买第 5 台水泵，花费 ${PRICE_PUMP5} 元`,'success');
        setMessage('💧 购买第 5 台水泵成功','success');
        renderDevices();updateUI();updateShop();
    });
    buyCool.addEventListener('click',()=>{
        if(state.money<PRICE_COOL){setMessage('余额不足','warn');return}
        state.money-=PRICE_COOL;state.coolLevel++;
        playBeep(1400,0.15,0.06);
        log(`💰 冷却效率升级至 Lv.${state.coolLevel}，花费 ${PRICE_COOL} 元`,'success');
        setMessage(`⚡ 冷却效率升级至 Lv.${state.coolLevel}`,'success');
        renderDevices();updateUI();updateShop();
    });
    window.addEventListener('resize',()=>drawTempChart());

    document.addEventListener('keydown',handleKeydown);
    setupPWA();
    state.updateTimer=setInterval(()=>updatePhysics(),UPDATE_INTERVAL);
    drawTempChart();
}
init();
})();
</script>
</body>
</html>
