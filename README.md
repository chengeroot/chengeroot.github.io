<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="theme-color" content="#0d1117">
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
.help-btn,.music-btn,.install-btn,.save-btn,.load-btn{padding:8px 18px;border-radius:8px;border:1px solid rgba(0,212,170,.35);background:rgba(0,212,170,.08);color:#4dffc3;font-weight:700;font-size:.85rem;cursor:pointer;transition:all .2s;font-family:inherit;white-space:nowrap}
.help-btn:hover,.music-btn:hover,.install-btn:hover,.save-btn:hover,.load-btn:hover{background:rgba(0,212,170,.2);border-color:#00d4aa}
.save-btn{border-color:rgba(88,166,255,.4);color:#58a6ff;background:rgba(88,166,255,.08)}
.save-btn:hover{background:rgba(88,166,255,.2);border-color:#58a6ff}
.load-btn{border-color:rgba(255,215,64,.4);color:#ffd740;background:rgba(255,215,64,.08)}
.load-btn:hover{background:rgba(255,215,64,.2);border-color:#ffd740}
.easy-btn{padding:8px 18px;border-radius:8px;border:1px solid rgba(77,255,195,.4);background:rgba(77,255,195,.08);color:#4dffc3;font-weight:700;font-size:.85rem;cursor:pointer;transition:all .2s;font-family:inherit;white-space:nowrap}
.easy-btn:hover{background:rgba(77,255,195,.2);border-color:#4dffc3}
.easy-btn.active{background:linear-gradient(180deg,#4dffc3,#00d4aa);border-color:#4dffc3;color:#0d1117;box-shadow:0 0 16px rgba(77,255,195,.5)}
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
.device-row{display:flex;align-items:center;justify-content:space-between;padding:9px 0;font-size:.78rem;gap:8px;flex-wrap:wrap;border-bottom:1px solid rgba(48,54,61,.3)}
.device-row:last-child{border-bottom:none}
.device-row>div{flex-shrink:0;display:flex;gap:4px;flex-wrap:nowrap;align-items:center;margin-left:auto}
.device-row .dev-led{width:11px;height:11px;border-radius:50%;display:inline-block;margin-right:6px}
.dev-led.on{background:#4dffc3;box-shadow:0 0 10px #4dffc3}
.dev-led.off{background:#484f58}
.dev-led.fault{background:#ff5c5c;box-shadow:0 0 10px #ff5c5c;animation:blink .5s infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.4}}
.device-row .dev-btn{padding:5px 12px;border-radius:5px;border:1px solid rgba(48,54,61,.8);background:rgba(48,54,61,.4);color:#8b949e;font-size:.7rem;cursor:pointer;font-weight:700;font-family:inherit;white-space:nowrap;flex-shrink:0}
.device-row .dev-btn:disabled{opacity:.35;cursor:not-allowed}
.device-row .dev-btn.repair.ready{border-color:#4dffc3;color:#4dffc3;background:rgba(77,255,195,.1);opacity:1}
.device-row .dev-btn.repair.rush{border-color:#ffd740;color:#ffd740;background:rgba(255,215,64,.1);opacity:1}
.device-row .power-slider{flex:1;min-width:60px;accent-color:#00d4aa;height:6px;cursor:pointer}
.device-row .power-label{font-family:'Consolas',monospace;font-size:.75rem;color:#ffd740;min-width:42px;text-align:center;font-weight:700;flex-shrink:0}
.device-row .cooldown-label{font-family:'Consolas',monospace;font-size:.75rem;color:#ff5c5c;min-width:58px;text-align:center;font-weight:700}
.device-row .src-btn{padding:3px 10px;border-radius:5px;border:1px solid rgba(48,54,61,.8);background:rgba(48,54,61,.4);color:#8b949e;font-size:.68rem;font-weight:700;cursor:pointer;min-width:34px;font-family:inherit;flex-shrink:0}
.device-row .src-btn.main{background:linear-gradient(180deg,#00d4aa,#00a888);border-color:#4dffc3;color:#0d1117}
.device-row .src-btn.backup{background:linear-gradient(180deg,#ff9f43,#e0821e);border-color:#ffb866;color:#0d1117}
.device-row .g-temp{font-family:'Consolas',monospace;font-size:.7rem;font-weight:700;padding:1px 6px;border-radius:4px;background:rgba(0,0,0,.4);border:1px solid rgba(48,54,61,.5);margin-left:4px;transition:color .3s,border-color .3s}
.device-row .g-temp.normal{color:#4dffc3}
.device-row .g-temp.warn{color:#ff9f43;border-color:rgba(255,159,67,.5)}
.device-row .g-temp.danger{color:#ff5c5c;border-color:rgba(255,92,92,.6);animation:blink .5s infinite}
.device-row .g-temp.off{color:#484f58}
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
.help-section p{font-size:.95rem;color:#c9d1d9;margin-bottom:10px;line-height:1.9}
.help-section ul{list-style:none;padding-left:4px}
.help-section ul li{font-size:.95rem;color:#c9d1d9;padding:6px 0 6px 20px;position:relative;line-height:1.8}
.help-section ul li::before{content:'·';position:absolute;left:4px;color:#00d4aa;font-weight:700;font-size:1.1rem}
.help-section .key{display:inline-block;padding:3px 10px;background:#21262d;border:1px solid #30363d;border-radius:5px;font-family:'Consolas',monospace;font-size:.85rem;color:#f0f6fc}
.help-section .danger-text{color:#ff5c5c;font-weight:700}
.help-section .good-text{color:#4dffc3;font-weight:700}
.help-section .warn-text{color:#ff9f43;font-weight:700}
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
.save-name-input{width:100%;padding:12px 16px;border-radius:8px;border:1px solid rgba(88,166,255,.4);background:rgba(88,166,255,.08);color:#f0f6fc;font-size:1rem;font-family:inherit;outline:none;transition:all .2s}
.save-name-input:focus{border-color:#58a6ff;background:rgba(88,166,255,.15);box-shadow:0 0 0 3px rgba(88,166,255,.15)}
.save-name-hint{font-size:.8rem;color:#6e7681;margin-bottom:14px;line-height:1.7}
.save-name-hint code{background:#21262d;border:1px solid #30363d;border-radius:4px;padding:2px 7px;color:#ffd740;font-family:'Consolas',monospace;font-size:.85em}
.save-name-actions{display:flex;gap:10px;justify-content:flex-end;margin-top:6px}
@media(max-width:1200px){.game-container{padding:22px 20px 26px}.gauge .value{font-size:2.3rem}.param-item .p-value{font-size:1rem}}
@media(max-width:900px){.game-container{padding:20px 18px 24px}.dashboard{gap:10px}.gauge{padding:14px}.gauge .value{font-size:1.9rem}.param-panel{grid-template-columns:repeat(4,1fr);gap:8px;padding:12px}.device-panel{grid-template-columns:1fr 1fr;gap:10px}.title{font-size:1.3rem}.title-icon{width:18px;height:18px}}
@media(max-width:600px){body{padding:8px}.game-container{padding:14px 12px 18px;border-radius:12px}.header{margin-bottom:12px;gap:6px}.title-wrap{gap:6px}.title{font-size:1rem;letter-spacing:1px;gap:6px}.title-icon{width:14px;height:14px}.help-btn,.music-btn,.install-btn,.save-btn,.load-btn,.easy-btn{padding:4px 9px;font-size:.6rem;border-radius:5px}.status-badge{padding:5px 10px;font-size:.65rem}.status-badge .dot{width:8px;height:8px}.difficulty-select{padding:5px 8px;font-size:.65rem}.dashboard{grid-template-columns:repeat(2,1fr);gap:10px;margin-bottom:12px}.gauge{padding:12px}.gauge .label{font-size:.62rem}.gauge .value{font-size:1.6rem}.gauge .unit{font-size:.65rem}.param-panel{grid-template-columns:repeat(2,1fr);gap:6px;padding:10px}.param-item{border-bottom:1px dashed rgba(48,54,61,.4)}.param-item .p-label{font-size:.5rem}.param-item .p-value{font-size:.8rem}.panel-row{padding:10px 12px;gap:8px;flex-wrap:wrap}.panel-row .row-label{font-size:.65rem}.panel-row .row-value{font-size:.85rem;min-width:42px}.power-select .ps-btn{padding:7px 16px;font-size:.68rem;flex:1;min-width:0}.power-select .ps-info{font-size:.62rem;width:100%;margin-left:0;text-align:right}.relief-control .relief-btn{padding:6px 14px;font-size:.65rem}.relief-control .relief-status{font-size:.72rem;min-width:60px}.boric-control{padding:10px 12px;gap:8px}.boric-control .boric-btn{padding:6px 14px;font-size:.65rem}.boric-control .boric-status{font-size:.72rem;min-width:90px}.device-panel{grid-template-columns:1fr;gap:10px}.device-group{padding:10px 12px}.device-group h4{font-size:.62rem}.device-row{font-size:.62rem;padding:6px 0;gap:4px}.device-row .dev-btn{padding:4px 8px;font-size:.58rem;min-width:28px}.device-row .g-temp{font-size:.55rem;padding:1px 4px}.shop-items{grid-template-columns:1fr 1fr;gap:8px}.shop-item{padding:8px 10px}.shop-item .item-name{font-size:.68rem}.shop-item .item-desc{font-size:.6rem}.shop-item .item-buy{padding:5px 10px;font-size:.65rem}.terminal-body{height:90px;font-size:.65rem;padding:8px 12px}.terminal-title{font-size:.55rem}.controls{gap:6px;margin:6px 0}.ctrl-btn{padding:9px 12px;font-size:.65rem;flex:1 1 calc(50% - 6px);min-width:0;border-radius:6px}.message-area{padding:8px 12px;min-height:36px}.message-area .msg{font-size:.72rem}.modal-body{padding:16px}.modal-header{padding:12px 16px}.modal-header h2{font-size:.95rem}.help-section h3{font-size:.82rem}.help-section p,.help-section ul li{font-size:.78rem}.scram-modal{padding:30px 24px}.scram-modal h2{font-size:1.4rem}.scram-modal .scram-icon{font-size:3.5rem}.hotkey-card{padding:20px 18px}.hotkey-card h3{font-size:.95rem}.chart-canvas{height:80px}}
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
            <button class="easy-btn" id="btnEasy">🟢 简单模式</button>
            <button class="music-btn" id="btnMusic">🔇 音乐</button>
            <button class="save-btn" id="btnSave">💾 存档</button>
            <button class="load-btn" id="btnLoad">📂 读档</button>
            <button class="install-btn" id="btnInstall" style="display:none;">📲 安装</button>
            <button class="help-btn" id="btnHelp">📖 说明书</button>
        </div>
        <div class="status-badge">
            <span class="dot running" id="statusDot"></span>
            <span id="statusText">运行中</span>
        </div>
    </div>

    <div class="dashboard">
        <div class="gauge"><div class="label">核心温度</div><div class="value temp" id="tempDisplay">500</div><div class="unit">°C</div><div class="bar-track"><div class="bar-fill temp-bar" id="tempBar" style="width:33%"></div></div></div>
        <div class="gauge"><div class="label">电功率</div><div class="value power" id="powerDisplay">0.0</div><div class="unit">MW</div><div class="bar-track"><div class="bar-fill power-bar" id="powerBar" style="width:0%"></div></div></div>
        <div class="gauge"><div class="label">累计收益</div><div class="value money" id="moneyDisplay">0.00</div><div class="unit">元 · 无上限</div></div>
        <div class="gauge"><div class="label">运行时间</div><div class="value time" id="timeDisplay">0s</div><div class="unit">实时</div></div>
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
        <span id="loadHint" style="font-size:.7rem;color:#4dffc3;font-weight:700;min-width:80px;">建议: 0 MW</span>
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
        <div class="modal-header"><h2>📖 操作手册</h2><button class="modal-close" id="btnCloseHelp">✕</button></div>
        <div class="modal-body">
            <div class="help-section"><h3>🎯 游戏目标</h3><p>你是一座核电站的站长。让反应堆持续稳定发电，能赚多久就赚多久。</p><p>但记住一句话：<strong>温度千万不能超过 1500°C</strong>，否则会熔毁爆炸，游戏结束。</p></div>
            <div class="help-section"><h3>1. 怎么看仪表盘</h3><ul><li><strong>核心温度</strong>：500°C 左右正常，越往上越危险</li><li><strong>电功率</strong>：现在发了多少电</li><li><strong>累计收益</strong>：一共赚了多少钱，无上限</li><li><strong>运行时间</strong>：本局玩了多久</li></ul></div>
            <div class="help-section"><h3>2. 控制棒滑块</h3><p>控制棒是反应堆的"油门"：</p><ul><li>插得深（数值大）→ 反应慢，温度低</li><li>拉出来（数值小）→ 反应快，温度高</li></ul><p>30% 是默认稳定点。想发更多电就慢慢往下拉，但别超过 1500°C。</p></div>
            <div class="help-section"><h3>3. 温度曲线图</h3><p>红线是 1200°C（危险区），橙线是 900°C（提醒线）。曲线快速上扬时马上插深控制棒。</p></div>
            <div class="help-section"><h3>4. 紧急降温</h3><ul><li><strong>💨 泄压阀</strong>：降 8°C、降 1.5MPa，3 秒冷却</li><li><strong>☢️ 硼酸注入</strong>：10 秒内降 200°C，之后 30 秒发电减半，60 秒冷却</li></ul></div>
            <div class="help-section"><h3>5. 水泵和发电机组</h3><ul><li>水泵负责降温，每台可单独调功率</li><li>发电机组负责发电，也是每台单独调</li><li>设备坏了要等 12 秒修，或花 10 元紧急修复</li></ul></div>
            <div class="help-section"><h3>6. ⚡ 机组温度</h3><p>每台发电机组旁边有个温度显示（默认 25°C）：</p><ul><li>功率拉得越高、反应堆越热、压力越大 → 机组越热</li><li>水泵越强 → 机组凉得越快</li></ul><p>颜色含义：</p><ul><li><span class="good-text">绿色</span>（&lt; 80°C）：正常</li><li><span class="warn-text">橙色</span>（80~100°C）：发电效率下降</li><li><span class="danger-text">红色闪烁</span>（&gt; 100°C）：每秒有概率过热损坏</li></ul></div>
            <div class="help-section"><h3>7. 简单模式</h3><p>顶部有个 <strong>🟢 简单模式</strong> 按钮，点一下开启（按钮会变绿发光）。开启后：</p><ul><li>设备故障概率降到 1/10</li><li>随机事件发生率降低，间隔加倍</li><li>电网异常不再损坏机组</li></ul><p>再点一下关闭。适合新手熟悉操作。</p></div>
            <div class="help-section"><h3>8. 电网调频</h3><p>滑块旁的"建议：XX MW"提示：绿=差&lt;5，橙=差5~15，红=差&gt;15。开局前 60 秒保护，不会因电网损坏设备。</p></div>
            <div class="help-section"><h3>9. 商店</h3><ul><li><strong>第 5 台水泵（50 元）</strong>：多一台降温</li><li><strong>冷却效率升级（30 元/级）</strong>：每级 +5% 冷却</li><li><strong>紧急修复（10 元/次）</strong>：设备坏了后的黄色按钮</li></ul></div>
            <div class="help-section"><h3>10. 随机事件</h3><ul><li>⚡ 电网需求突变</li><li>💧 冷却水泄漏（温度+20~40°C）</li><li>🌊 地震（2~3 台水泵同时故障）</li></ul></div>
            <div class="help-section"><h3>11. 难度分级</h3><ul><li><strong>🟢 简单</strong>：故障概率减半</li><li><strong>🟡 普通</strong>：默认</li><li><strong>🟠 困难</strong>：故障 x1.5</li><li><strong>🔴 硬核</strong>：故障 x2，无弹窗提示</li></ul></div>
            <div class="help-section"><h3>12. 存档 / 读档</h3><ul><li><strong>💾 存档</strong>：输入名字，保存为 <code>名字.fyd</code></li><li><strong>📂 读档</strong>：选 .fyd 文件恢复进度</li></ul><p>打开存档/读档弹窗时游戏会自动暂停。简单模式开关也会存进存档。</p></div>
            <div class="help-section"><h3>13. 没有通关</h3><p>这个游戏没有终点，唯一的结局是温度冲到 1500°C 触发紧急停堆，失败则 60 秒后熔毁爆炸。<span class="danger-text">活得越久，赚得越多</span>。</p></div>
            <div class="help-section"><h3>14. 快捷键</h3><p>按 <span class="key">H</span> 随时查看全部快捷键。</p></div>
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
        <canvas class="score-canvas" id="scoreCanvas" width="540" height="400"></canvas>
        <div class="score-actions">
            <button class="score-btn primary" id="btnSaveScore">💾 保存图片</button>
            <button class="score-btn secondary" id="btnCloseScore">关闭</button>
        </div>
    </div>
</div>

<div class="modal-overlay" id="saveNameModal">
    <div class="modal" style="max-width:440px">
        <div class="modal-header"><h2>💾 保存存档</h2><button class="modal-close" id="btnCloseSaveName">✕</button></div>
        <div class="modal-body">
            <p class="save-name-hint">给存档起个名字，保存为 <code>名字.fyd</code>。游戏会暂停，放心输入。</p>
            <input type="text" class="save-name-input" id="saveNameInput" maxlength="24" placeholder="输入名字（最多24字）" autocomplete="off">
            <div class="save-name-actions">
                <button class="score-btn secondary" id="btnCancelSaveName">取消</button>
                <button class="score-btn primary" id="btnConfirmSaveName">保存</button>
            </div>
        </div>
    </div>
</div>

<script>
(() => {
'use strict';
const MAX_TEMP=1500,MELTDOWN_TIME=60,ROD_SPEED=10,UPDATE_INTERVAL=1000;
const MAX_PUMP_POWER=1.23,TEMP_MIN=0,TEMP_MAX=2000,REPAIR_COOLDOWN=12,RELIEF_COOLDOWN=3;
const GEN_MAX_MW=50,PUMP_MAX_MW=4,PRESSURE_MIN=4.0,PRESSURE_MAX=20.0;
const ROD_DEFAULT=30,COLD_SHUTDOWN_TEMP=100,HEAT_TO_ELECTRIC=0.5,SCRAM_SUCCESS_RATE=0.5;
const PRICE_PER_KWH=0.009;
const GRID_SENSITIVITY=0.008,GRID_RANDOM_RANGE=0.01,GRID_DEVIATION_LIMIT=8;
const GRID_DAMAGE_THRESHOLD=8,GRID_DAMAGE_CHANCE=0.005,GRID_SELF_STABILIZE=0.5,GRID_SNAP_BOOST=3;
const GRID_START_GRACE=60;
const BACKUP_DRAIN_RATE=0.05,BACKUP_CHARGE_RATE=0.01;
const TEMP_BASE=50,TEMP_SPAN=1150,MAX_TEMP_RATE=8;
const GEN_TEMP_INIT=25,GEN_TEMP_MAX=150,GEN_TEMP_WARN=80,GEN_TEMP_FAULT=100;
const GEN_TEMP_IDLE_COOL=3,GEN_TEMP_RESPONSE=0.08;
const GEN_TEMP_POWER_HEAT=60,GEN_TEMP_ENV_GAIN=40,GEN_TEMP_ENV_BASE=400,GEN_TEMP_ENV_SPAN=1100;
const GEN_TEMP_PRESSURE_GAIN=25,GEN_TEMP_PRESSURE_BASE=10,GEN_TEMP_PRESSURE_SPAN=10;
const GEN_TEMP_PUMP_COOL=50,GEN_TEMP_BASE_OFFSET=25;
const PRICE_PUMP5=50,PRICE_COOL=30,PRICE_RUSH_REPAIR=10;
const BORIC_DURATION=10,BORIC_TOTAL_COOL=200,BORIC_CONTAMINATION=30,BORIC_COOLDOWN=60;
const CHART_SECONDS=60,SAVE_VERSION=1;

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
    tempHistory:[],maxTempReached:500,easyMode:false
};
let wasPausedBeforeHelp=false,wasPausedBeforeScramModal=false,wasPausedBeforeSave=false,wasPausedBeforeLoad=false;

let audioCtx=null,musicPlaying=false,musicNodes=null,musicMaster=null,lastAlarmTime=0;
function initAudio(){if(audioCtx)return true;try{audioCtx=new(window.AudioContext||window.webkitAudioContext)();return true}catch(e){return false}}
function startMusic(){
    if(!initAudio())return;if(audioCtx.state==='suspended')audioCtx.resume();if(musicPlaying)return;
    musicMaster=audioCtx.createGain();musicMaster.gain.value=0.0001;musicMaster.connect(audioCtx.destination);
    const osc1=audioCtx.createOscillator();osc1.type='sine';osc1.frequency.value=55;
    const g1=audioCtx.createGain();g1.gain.value=0.5;osc1.connect(g1);g1.connect(musicMaster);osc1.start();
    const osc2=audioCtx.createOscillator();osc2.type='triangle';osc2.frequency.value=110;
    const g2=audioCtx.createGain();g2.gain.value=0.12;osc2.connect(g2);g2.connect(musicMaster);osc2.start();
    const lfo=audioCtx.createOscillator();lfo.type='sine';lfo.frequency.value=0.18;
    const lfoG=audioCtx.createGain();lfoG.gain.value=0.02;lfo.connect(lfoG);lfoG.connect(musicMaster.gain);lfo.start();
    const osc3=audioCtx.createOscillator();osc3.type='sine';osc3.frequency.value=55.3;
    const g3=audioCtx.createGain();g3.gain.value=0.25;osc3.connect(g3);g3.connect(musicMaster);osc3.start();
    musicNodes={osc1,osc2,osc3,lfo};musicPlaying=true;
    musicMaster.gain.exponentialRampToValueAtTime(0.06,audioCtx.currentTime+1.5);
}
function stopMusic(){if(!musicPlaying||!musicMaster)return;const now=audioCtx.currentTime;musicMaster.gain.cancelScheduledValues(now);musicMaster.gain.setValueAtTime(musicMaster.gain.value,now);musicMaster.gain.exponentialRampToValueAtTime(0.0001,now+0.5);const n=musicNodes;setTimeout(()=>{try{n.osc1.stop();n.osc2.stop();n.osc3.stop();n.lfo.stop()}catch(e){}},600);musicNodes=null;musicPlaying=false}
function toggleMusic(){if(musicPlaying){stopMusic();btnMusic.classList.remove('playing');btnMusic.textContent='🔇 音乐';log('音乐已关闭','info')}else{startMusic();btnMusic.classList.add('playing');btnMusic.textContent='🎵 播放中';log('音乐已开启','info')}}
function updateMusic(){if(!musicPlaying||!audioCtx||!musicNodes)return;const now=audioCtx.currentTime;const temp=state.temperature;let tv=0.06,tf=55;if(state.gameOver){tv=0.03;tf=40}else if(state.meltdownActive){tv=0.11;tf=70}else if(temp>1200){tv=0.10;tf=66}else if(temp>900){tv=0.085;tf=60}else if(temp<COLD_SHUTDOWN_TEMP){tv=0.035;tf=48}else{tv=0.055+(temp-500)/1500*0.02;tf=55+(temp-500)/1000*3}try{musicMaster.gain.cancelScheduledValues(now);musicMaster.gain.linearRampToValueAtTime(tv,now+0.4);musicNodes.osc1.frequency.linearRampToValueAtTime(tf,now+0.4);musicNodes.osc3.frequency.linearRampToValueAtTime(tf+0.3,now+0.4);musicNodes.osc2.frequency.linearRampToValueAtTime(tf*2,now+0.4)}catch(e){}}
function playBeep(freq=880,dur=0.08,vol=0.05){if(!audioCtx)return;try{const now=audioCtx.currentTime;const osc=audioCtx.createOscillator();const gain=audioCtx.createGain();osc.type='square';osc.frequency.value=freq;gain.gain.setValueAtTime(0,now);gain.gain.linearRampToValueAtTime(vol,now+0.005);gain.gain.exponentialRampToValueAtTime(0.0001,now+dur);osc.connect(gain);gain.connect(audioCtx.destination);osc.start(now);osc.stop(now+dur+0.02)}catch(e){}}
function playAlarm(){if(!audioCtx)return;if(!DIFFICULTY[state.difficulty].showAlarm)return;const now=audioCtx.currentTime;if(now-lastAlarmTime<1.2)return;lastAlarmTime=now;try{const osc=audioCtx.createOscillator();const gain=audioCtx.createGain();osc.type='sawtooth';osc.frequency.setValueAtTime(660,now);osc.frequency.exponentialRampToValueAtTime(440,now+0.4);gain.gain.setValueAtTime(0,now);gain.gain.linearRampToValueAtTime(0.08,now+0.02);gain.gain.exponentialRampToValueAtTime(0.0001,now+0.5);osc.connect(gain);gain.connect(audioCtx.destination);osc.start(now);osc.stop(now+0.55)}catch(e){}}

const $=id=>document.getElementById(id);
const tempDisplay=$('tempDisplay'),powerDisplay=$('powerDisplay'),moneyDisplay=$('moneyDisplay'),timeDisplay=$('timeDisplay');
const tempBar=$('tempBar'),powerBar=$('powerBar'),statusDot=$('statusDot'),statusText=$('statusText');
const messageEl=$('message'),btnPause=$('btnPause'),btnScram=$('btnScram'),btnRestart=$('btnRestart'),btnReset=$('btnReset');
const pumpContainer=$('pumpContainer'),genContainer=$('genContainer'),pumpSummary=$('pumpSummary'),genSummary=$('genSummary');
const outageDisplay=$('outageDisplay'),pumpPowerStatus=$('pumpPowerStatus');
const thermalPowerEl=$('thermalPower'),reactorPressureEl=$('reactorPressure'),mainPowerEl=$('mainPower'),backupPowerEl=$('backupPower');
const reactorStabilityEl=$('reactorStability'),neutronFluxEl=$('neutronFlux'),gridDeviationEl=$('gridDeviation');
const meltdownWarning=$('meltdownWarning'),meltdownTimer=$('meltdownTimer'),rodSlider=$('rodSlider'),rodValue=$('rodValue'),rodLock=$('rodLock');
const loadSlider=$('loadSlider'),loadValue=$('loadValue'),reliefBtn=$('reliefBtn'),reliefStatus=$('reliefStatus'),loadHint=$('loadHint');
const boricBtn=$('boricBtn'),boricStatus=$('boricStatus');
const psMain=$('psMain'),psBackup=$('psBackup'),psInfo=$('psInfo');
const btnHelp=$('btnHelp'),helpModal=$('helpModal'),btnCloseHelp=$('btnCloseHelp'),btnMusic=$('btnMusic');
const terminalBody=$('terminalBody'),scramModal=$('scramModal'),scramModalContent=$('scramModalContent');
const scramIcon=$('scramIcon'),scramTitle=$('scramTitle'),scramDesc=$('scramDesc'),scramOkBtn=$('scramOkBtn');
const difficultySel=$('difficultySel'),shopMoney=$('shopMoney'),coolLv=$('coolLv');
const buyPump5=$('buyPump5'),buyCool=$('buyCool'),eventBanner=$('eventBanner'),evIcon=$('evIcon'),evText=$('evText'),evClose=$('evClose');
const tempChart=$('tempChart'),hotkeyOverlay=$('hotkeyOverlay');
const scoreModal=$('scoreModal'),scoreCanvas=$('scoreCanvas'),btnSaveScore=$('btnSaveScore'),btnCloseScore=$('btnCloseScore');
const btnInstall=$('btnInstall'),btnSave=$('btnSave'),btnLoad=$('btnLoad'),btnEasy=$('btnEasy');
const saveNameModal=$('saveNameModal'),saveNameInput=$('saveNameInput');
const btnCloseSaveName=$('btnCloseSaveName'),btnCancelSaveName=$('btnCancelSaveName'),btnConfirmSaveName=$('btnConfirmSaveName');

function clamp(v,a,b){return Math.max(a,Math.min(b,v))}
function rand(a,b){return Math.random()*(b-a)+a}
function diff(){return DIFFICULTY[state.difficulty]}
function log(text,type='info'){const d=new Date();const t=`${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}:${String(d.getSeconds()).padStart(2,'0')}`;const line=document.createElement('div');line.className='terminal-line '+type;line.textContent=`[${t}] ${text}`;terminalBody.appendChild(line);terminalBody.scrollTop=terminalBody.scrollHeight;while(terminalBody.children.length>80)terminalBody.removeChild(terminalBody.firstChild)}
function setMessage(text,type='info'){messageEl.textContent=text;messageEl.className='msg '+type}
function showEvent(icon,text){if(!diff().showPopup){log('[事件] '+text,'warn');return}evIcon.textContent=icon;evText.textContent=text;eventBanner.classList.add('active');setTimeout(()=>eventBanner.classList.remove('active'),6000)}
function updateGridDisplay(){const d=state.gridDeviation;let arrow='';if(d>0.15)arrow=' ↑';else if(d<-0.15)arrow=' ↓';else arrow=' ·';gridDeviationEl.textContent=d.toFixed(2)+'%'+arrow;const abs=Math.abs(d);if(abs<1)gridDeviationEl.className='p-value good';else if(abs<3)gridDeviationEl.className='p-value warn';else gridDeviationEl.className='p-value danger'}
function updateShop(){shopMoney.textContent='余额：'+state.money.toFixed(2)+' 元';coolLv.textContent=state.coolLevel;buyPump5.disabled=state.money<PRICE_PUMP5||state.pumpCount>=5;if(state.pumpCount>=5){buyPump5.textContent='已拥有'}else{buyPump5.textContent=`购买 ${PRICE_PUMP5} 元`}buyCool.disabled=state.money<PRICE_COOL;buyCool.textContent=`升级 ${PRICE_COOL} 元`}
function drawTempChart(){
    const ctx=tempChart.getContext('2d');const dpr=window.devicePixelRatio||1;
    const w=tempChart.clientWidth,h=tempChart.clientHeight;
    if(tempChart.width!==w*dpr||tempChart.height!==h*dpr){tempChart.width=w*dpr;tempChart.height=h*dpr}
    ctx.setTransform(dpr,0,0,dpr,0,0);ctx.clearRect(0,0,w,h);
    ctx.strokeStyle='rgba(48,54,61,0.5)';ctx.lineWidth=1;
    for(let i=0;i<=4;i++){const y=h*i/4;ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(w,y);ctx.stroke()}
    const yMax=1600;const y1200=h-(1200/yMax)*h;const y900=h-(900/yMax)*h;
    ctx.fillStyle='rgba(255,92,92,0.08)';ctx.fillRect(0,0,w,y1200);
    ctx.fillStyle='rgba(255,159,67,0.05)';ctx.fillRect(0,y1200,w,y900-y1200);
    ctx.strokeStyle='rgba(255,92,92,0.6)';ctx.setLineDash([4,4]);ctx.beginPath();ctx.moveTo(0,y1200);ctx.lineTo(w,y1200);ctx.stroke();
    ctx.strokeStyle='rgba(255,159,67,0.5)';ctx.beginPath();ctx.moveTo(0,y900);ctx.lineTo(w,y900);ctx.stroke();
    ctx.setLineDash([]);
    const history=state.tempHistory;
    if(history.length>1){
        ctx.strokeStyle='#ff9f43';ctx.lineWidth=2;ctx.beginPath();
        for(let i=0;i<history.length;i++){const x=(i/(CHART_SECONDS-1))*w;const y=h-clamp(history[i]/yMax,0,1)*h;if(i===0)ctx.moveTo(x,y);else ctx.lineTo(x,y)}
        ctx.stroke();
        const grad=ctx.createLinearGradient(0,0,0,h);grad.addColorStop(0,'rgba(255,159,67,0.3)');grad.addColorStop(1,'rgba(255,159,67,0)');
        ctx.fillStyle=grad;ctx.lineTo((history.length-1)/(CHART_SECONDS-1)*w,h);ctx.lineTo(0,h);ctx.closePath();ctx.fill();
        const lastX=(history.length-1)/(CHART_SECONDS-1)*w;const lastY=h-clamp(history[history.length-1]/yMax,0,1)*h;
        ctx.fillStyle='#ff9f43';ctx.beginPath();ctx.arc(lastX,lastY,4,0,Math.PI*2);ctx.fill();
        ctx.fillStyle='#fff';ctx.beginPath();ctx.arc(lastX,lastY,2,0,Math.PI*2);ctx.fill()
    }
    ctx.font='10px Consolas, monospace';ctx.fillStyle='rgba(255,92,92,0.7)';ctx.fillText('1200°C',4,y1200-3);
    ctx.fillStyle='rgba(255,159,67,0.7)';ctx.fillText('900°C',4,y900-3)
}
function pushTempHistory(){state.tempHistory.push(state.temperature);if(state.tempHistory.length>CHART_SECONDS)state.tempHistory.shift()}
function updateBoricUI(){
    if(state.boricCooldown>0){boricBtn.disabled=true;boricStatus.textContent=`冷却 ${state.boricCooldown}s`;boricStatus.className='boric-status warn';boricBtn.classList.remove('pulsing')}
    else if(state.boricActive){boricBtn.disabled=true;boricStatus.textContent=`注入中 ${state.boricRemaining}s`;boricStatus.className='boric-status info';boricBtn.classList.add('pulsing')}
    else if(state.boricContamination>0){boricBtn.disabled=true;boricStatus.textContent=`污染 ${state.boricContamination}s`;boricStatus.className='boric-status danger';boricBtn.classList.remove('pulsing')}
    else{boricBtn.disabled=false;boricStatus.textContent='就绪';boricStatus.className='boric-status';boricBtn.classList.remove('pulsing')}
}
function doBoricInjection(){
    if(state.gameOver||state.paused)return;
    if(state.boricCooldown>0){setMessage(`硼酸冷却中 ${state.boricCooldown}s`,'warn');return}
    if(state.boricActive){setMessage('硼酸正在注入','warn');return}
    if(state.boricContamination>0){setMessage('反应堆仍在污染中','warn');return}
    playBeep(440,0.2,0.08);state.boricActive=true;state.boricRemaining=BORIC_DURATION;state.boricCooldown=BORIC_COOLDOWN;
    log('☢️ 硼酸注入启动！10秒内降温200°C','danger');setMessage('☢️ 硼酸注入中','danger');updateBoricUI()
}
function initDevices(){
    state.pumps=[];for(let i=0;i<state.pumpCount;i++)state.pumps.push({on:true,fault:false,power:100,repairCooldown:0});
    state.generators=[];for(let i=0;i<4;i++)state.generators.push({on:true,damaged:false,power:100,repairCooldown:0,powerTarget:'main',temp:GEN_TEMP_INIT});
    state.powerOutage=false;state.rodDepth=ROD_DEFAULT;state.rodTarget=ROD_DEFAULT;state.rodLocked=false;state.scramTriggered=false;state.coldShutdownLogged=false;
    if(state.rodAnimId){cancelAnimationFrame(state.rodAnimId);state.rodAnimId=null}
    state.gridDeviation=0;state.targetLoad=50;state.reliefCooldown=0;state.reactorPressure=8.0;state.backupPower=100;state.powerSelect='main';
    state.boricActive=false;state.boricRemaining=0;state.boricCooldown=0;state.boricContamination=0;
    state.tempHistory=[];for(let i=0;i<CHART_SECONDS;i++)state.tempHistory.push(500);
    loadSlider.value=50;loadValue.textContent='50';rodSlider.value=ROD_DEFAULT;rodValue.textContent=ROD_DEFAULT+'%';rodSlider.disabled=false;state.nextEventTime=30;
    state.mainPowerBus=state.generators.filter(g=>g.on&&!g.damaged&&g.powerTarget==='main').reduce((s,g)=>s+(g.power/100)*GEN_MAX_MW,0);
    pumpEls=[];genEls=[];buildPumpRows();buildGenRows()
}
function animateRod(){
    if(state.paused||state.gameOver){state.rodAnimId=null;return}
    if(state.rodLocked){rodValue.textContent=Math.round(state.rodDepth)+'%';rodSlider.value=Math.round(state.rodDepth);state.rodAnimId=null;return}
    const diff_=state.rodTarget-state.rodDepth;
    if(Math.abs(diff_)<0.3){state.rodDepth=state.rodTarget;rodValue.textContent=Math.round(state.rodDepth)+'%';rodSlider.value=Math.round(state.rodDepth);state.rodAnimId=null;return}
    const step=(ROD_SPEED/1000)*16*Math.sign(diff_);let nv=state.rodDepth+step;if(Math.abs(nv-state.rodTarget)<0.3)nv=state.rodTarget;
    state.rodDepth=clamp(nv,0,100);rodValue.textContent=Math.round(state.rodDepth)+'%';rodSlider.value=Math.round(state.rodDepth);state.rodAnimId=requestAnimationFrame(animateRod)
}
function setRodTarget(val){
    if(state.rodLocked){rodSlider.value=Math.round(state.rodDepth);rodValue.textContent=Math.round(state.rodDepth)+'%';setMessage('控制棒已锁定','warn');return}
    val=clamp(val,0,100);state.rodTarget=val;
    if(!state.rodAnimId&&Math.abs(state.rodDepth-val)>0.3)state.rodAnimId=requestAnimationFrame(animateRod);
    else if(Math.abs(state.rodDepth-val)<0.3){state.rodDepth=val;rodValue.textContent=Math.round(val)+'%';rodSlider.value=Math.round(val);if(state.rodAnimId){cancelAnimationFrame(state.rodAnimId);state.rodAnimId=null}}
}
function scram(){if(state.gameOver)return;playBeep(440,0.15,0.06);if(!state.rodLocked){state.rodTarget=100;if(!state.rodAnimId&&!state.paused)state.rodAnimId=requestAnimationFrame(animateRod);setMessage('🛑 紧急停堆','danger');log('手动紧急停堆','warn')}else setMessage('控制棒已锁定','warn')}
function restartReactor(){
    if(state.gameOver)return;if(state.temperature>=COLD_SHUTDOWN_TEMP){setMessage('尚未冷停堆','warn');return}
    playBeep(880,0.1,0.06);
    state.temperature=500;state.reactorPressure=8.0;state.rodDepth=ROD_DEFAULT;state.rodTarget=ROD_DEFAULT;
    rodSlider.value=ROD_DEFAULT;rodValue.textContent=ROD_DEFAULT+'%';
    state.rodLocked=false;rodLock.classList.remove('active');rodLock.textContent='解锁';rodSlider.disabled=false;
    state.coldShutdownLogged=false;state.scramTriggered=false;
    log('反应堆启动','success');setMessage('反应堆已启动','success');statusDot.className='dot running';statusText.textContent='运行中';
    renderDevices();updateUI()
}
function showScramResult(success){if(success){scramModalContent.className='scram-modal success';scramIcon.textContent='✅';scramTitle.textContent='停堆成功';scramDesc.textContent='反应堆已安全关闭，温度回落至500°C。'}else{scramModalContent.className='scram-modal fail';scramIcon.textContent='❌';scramTitle.textContent='停堆失败';scramDesc.textContent='控制棒卡死！核融倒计时60秒。'}wasPausedBeforeScramModal=state.paused;if(!state.paused&&!state.gameOver){state.paused=true;btnPause.textContent='▶ 继续'}scramModal.classList.add('active')}
function closeScramModal(){scramModal.classList.remove('active');if(!wasPausedBeforeScramModal&&!state.gameOver){state.paused=false;btnPause.textContent='⏸ 暂停';if(state.rodAnimId===null&&Math.abs(state.rodTarget-state.rodDepth)>0.3&&!state.rodLocked)state.rodAnimId=requestAnimationFrame(animateRod)}}
function triggerEmergencyScram(){
    state.scramTriggered=true;state.rodLocked=false;rodLock.classList.remove('active');rodLock.textContent='解锁';rodSlider.disabled=false;
    state.rodTarget=100;if(!state.rodAnimId)state.rodAnimId=requestAnimationFrame(animateRod);
    log('温度达1500°C，紧急停堆启动','danger');playAlarm();
    const success=Math.random()<SCRAM_SUCCESS_RATE;
    if(success){
        log('停堆成功，温度回落','success');setMessage('✅ 停堆成功','success');statusText.textContent='安全停堆';statusDot.className='dot running';
        state.temperature=500;state.rodDepth=100;state.rodTarget=100;rodSlider.value=100;rodValue.textContent='100%';
        state.reactorPressure=8.0;state.generators.forEach(g=>{if(!g.damaged)g.on=true});
        state.meltdownActive=false;state.meltdownCounter=MELTDOWN_TIME;meltdownWarning.classList.remove('active');
        setTimeout(()=>{if(!state.gameOver)state.scramTriggered=false},5000)
    }else{
        log('停堆失败，核融倒计时','danger');setMessage('❌ 停堆失败','danger');statusText.textContent='核融';statusDot.className='dot meltdown';
        state.meltdownActive=true;state.meltdownCounter=MELTDOWN_TIME;meltdownWarning.classList.add('active')
    }
    showScramResult(success)
}
function doRelief(){
    if(state.gameOver||state.paused)return;
    if(state.reliefCooldown>0){setMessage(`泄压冷却 ${Math.ceil(state.reliefCooldown)}s`,'warn');return}
    playBeep(660,0.1,0.05);
    state.reactorPressure=clamp(state.reactorPressure-1.5,PRESSURE_MIN,PRESSURE_MAX);
    state.temperature=clamp(state.temperature-8,TEMP_MIN,TEMP_MAX);
    state.reliefCooldown=RELIEF_COOLDOWN;
    log('泄压：−1.5MPa，−8°C','info');setMessage('💨 泄压','info');updateReliefUI();updateUI()
}
function updateReliefUI(){
    if(state.reliefCooldown>0){reliefBtn.disabled=true;reliefStatus.textContent=`冷却 ${state.reliefCooldown.toFixed(1)}s`;reliefStatus.className='relief-status warn'}
    else{reliefBtn.disabled=false;if(state.reactorPressure>15.0){reliefStatus.textContent='⚠️ 压力过高';reliefStatus.className='relief-status danger'}else if(state.reactorPressure>12.0){reliefStatus.textContent='压力偏高';reliefStatus.className='relief-status warn'}else{reliefStatus.textContent='就绪';reliefStatus.className='relief-status'}}
}

let pumpEls=[],genEls=[];

function buildPumpRows(){
    pumpContainer.innerHTML='';pumpEls=[];
    state.pumps.forEach((p,idx)=>{
        const row=document.createElement('div');row.className='device-row';
        row.innerHTML=`
            <span>泵${idx+1} <span class="dev-led off"></span></span>
            <span class="p-status" style="font-size:.65rem;color:#8b949e;">运行</span>
            <input type="range" class="power-slider" min="0" max="100" value="${p.power}">
            <span class="power-label">${p.power}%</span>
            <div>
                <button class="dev-btn p-toggle">关</button>
                <button class="dev-btn repair p-repair" disabled>修复</button>
                <button class="dev-btn repair rush p-rush" style="display:none;">⚡10元</button>
            </div>`;
        const els={root:row,led:row.querySelector('.dev-led'),status:row.querySelector('.p-status'),slider:row.querySelector('.power-slider'),label:row.querySelector('.power-label'),toggle:row.querySelector('.p-toggle'),repair:row.querySelector('.p-repair'),rush:row.querySelector('.p-rush')};
        els.toggle.addEventListener('click',()=>{if(state.powerOutage){setMessage('停电中','warn');return}if(state.pumps[idx].fault){setMessage('水泵故障中','warn');return}state.pumps[idx].on=!state.pumps[idx].on;playBeep(state.pumps[idx].on?880:440,0.05,0.03);updatePumpRow(idx);updateUI()});
        els.repair.addEventListener('click',()=>{const pump=state.pumps[idx];if(!pump.fault||pump.repairCooldown>0)return;pump.fault=false;pump.on=true;pump.repairCooldown=0;playBeep(1200,0.08,0.05);log(`水泵${idx+1} 已修复`,'success');updatePumpRow(idx);updateUI()});
        els.rush.addEventListener('click',()=>{const pump=state.pumps[idx];if(!pump.fault)return;if(state.money<PRICE_RUSH_REPAIR){setMessage('余额不足','warn');return}state.money-=PRICE_RUSH_REPAIR;pump.fault=false;pump.on=true;pump.repairCooldown=0;playBeep(1400,0.1,0.05);log(`⚡ 紧急修复水泵${idx+1}，花费${PRICE_RUSH_REPAIR}元`,'success');setMessage(`⚡ 紧急修复水泵${idx+1}`,'success');updatePumpRow(idx);updateUI();updateShop()});
        els.slider.addEventListener('input',()=>{const val=parseInt(els.slider.value);state.pumps[idx].power=clamp(val,0,100);els.label.textContent=state.pumps[idx].power+'%';updateUI()});
        pumpEls.push(els);pumpContainer.appendChild(row)
    })
}
function updatePumpRow(idx){
    const p=state.pumps[idx],els=pumpEls[idx];if(!p||!els)return;
    els.led.className='dev-led '+(p.fault?'fault':(p.on?'on':'off'));els.slider.disabled=p.fault;
    if(p.fault){
        if(p.repairCooldown>0){els.status.textContent='🔧 '+p.repairCooldown.toFixed(1)+'s';els.status.style.color='#ff5c5c';els.repair.disabled=true;els.repair.classList.remove('ready');els.rush.style.display=''}
        else{els.status.textContent='可修复';els.status.style.color='#ff5c5c';els.repair.disabled=false;els.repair.classList.add('ready');els.rush.style.display='none'}
    }else{els.status.textContent=p.on?'运行':'停机';els.status.style.color='#8b949e';els.repair.disabled=true;els.repair.classList.remove('ready');els.rush.style.display='none'}
    els.toggle.textContent=p.on?'关':'开';if(p.on)els.toggle.classList.remove('success');else els.toggle.classList.add('success');
    els.slider.value=p.power;els.label.textContent=p.power+'%'
}
function buildGenRows(){
    genContainer.innerHTML='';genEls=[];
    state.generators.forEach((g,idx)=>{
        const row=document.createElement('div');row.className='device-row';
        row.innerHTML=`
            <span>机组${idx+1} <span class="dev-led off"></span> <span class="g-temp normal">${GEN_TEMP_INIT}°C</span></span>
            <span class="g-status" style="font-size:.65rem;color:#8b949e;">运行</span>
            <span class="g-src"></span>
            <input type="range" class="power-slider" min="0" max="100" value="${g.power}">
            <span class="power-label">${g.power}%</span>
            <div>
                <button class="dev-btn g-toggle">关</button>
                <button class="dev-btn repair g-repair" disabled>修复</button>
                <button class="dev-btn repair rush g-rush" style="display:none;">⚡10元</button>
            </div>`;
        const els={root:row,led:row.querySelector('.dev-led'),status:row.querySelector('.g-status'),gTemp:row.querySelector('.g-temp'),src:row.querySelector('.g-src'),slider:row.querySelector('.power-slider'),label:row.querySelector('.power-label'),toggle:row.querySelector('.g-toggle'),repair:row.querySelector('.g-repair'),rush:row.querySelector('.g-rush')};
        const btnMain=document.createElement('button');btnMain.className='src-btn';btnMain.textContent='主';
        const btnBack=document.createElement('button');btnBack.className='src-btn';btnBack.textContent='备';
        btnMain.addEventListener('click',()=>{if(!state.generators[idx].damaged){state.generators[idx].powerTarget='main';updateGenRow(idx);updateUI()}});
        btnBack.addEventListener('click',()=>{if(!state.generators[idx].damaged){state.generators[idx].powerTarget='backup';updateGenRow(idx);updateUI()}});
        els.src.appendChild(btnMain);els.src.appendChild(btnBack);els.btnMain=btnMain;els.btnBack=btnBack;
        els.toggle.addEventListener('click',()=>{if(state.powerOutage){setMessage('停电中','warn');return}if(state.generators[idx].damaged){setMessage('机组损坏','warn');return}state.generators[idx].on=!state.generators[idx].on;playBeep(state.generators[idx].on?880:440,0.05,0.03);updateGenRow(idx);updateUI()});
        els.repair.addEventListener('click',()=>{const g=state.generators[idx];if(!g.damaged||g.repairCooldown>0)return;g.damaged=false;g.on=true;g.repairCooldown=0;g.temp=GEN_TEMP_INIT;playBeep(1200,0.08,0.05);log(`机组${idx+1} 已修复`,'success');updateGenRow(idx);updateUI();checkOutage()});
        els.rush.addEventListener('click',()=>{const g=state.generators[idx];if(!g.damaged)return;if(state.money<PRICE_RUSH_REPAIR){setMessage('余额不足','warn');return}state.money-=PRICE_RUSH_REPAIR;g.damaged=false;g.on=true;g.repairCooldown=0;g.temp=GEN_TEMP_INIT;playBeep(1400,0.1,0.05);log(`⚡ 紧急修复机组${idx+1}，花费${PRICE_RUSH_REPAIR}元`,'success');setMessage(`⚡ 紧急修复机组${idx+1}`,'success');updateGenRow(idx);updateUI();updateShop();checkOutage()});
        els.slider.addEventListener('input',()=>{const val=parseInt(els.slider.value);state.generators[idx].power=clamp(val,0,100);els.label.textContent=state.generators[idx].power+'%';updateUI()});
        genEls.push(els);genContainer.appendChild(row)
    })
}
function updateGenRow(idx){
    const g=state.generators[idx],els=genEls[idx];if(!g||!els)return;
    els.led.className='dev-led '+(g.damaged?'fault':(g.on?'on':'off'));els.slider.disabled=g.damaged;
    if(g.damaged){
        if(g.repairCooldown>0){els.status.textContent='🔧 '+g.repairCooldown.toFixed(1)+'s';els.status.style.color='#ff5c5c';els.repair.disabled=true;els.repair.classList.remove('ready');els.rush.style.display=''}
        else{els.status.textContent='可修复';els.status.style.color='#ff5c5c';els.repair.disabled=false;els.repair.classList.add('ready');els.rush.style.display='none'}
        els.src.style.display='none';els.gTemp.textContent='—';els.gTemp.className='g-temp off'
    }else{
        els.status.textContent=g.on?'运行':'停机';els.status.style.color='#8b949e';els.repair.disabled=true;els.repair.classList.remove('ready');els.rush.style.display='none';els.src.style.display='';
        els.btnMain.className=g.powerTarget==='main'?'src-btn main':'src-btn';els.btnBack.className=g.powerTarget==='backup'?'src-btn backup':'src-btn';
        const t=Math.round(g.temp||0);
        if(!g.on){els.gTemp.textContent=t+'°C';els.gTemp.className='g-temp off'}
        else if(t>GEN_TEMP_FAULT){els.gTemp.textContent=t+'°C';els.gTemp.className='g-temp danger'}
        else if(t>GEN_TEMP_WARN){els.gTemp.textContent=t+'°C';els.gTemp.className='g-temp warn'}
        else{els.gTemp.textContent=t+'°C';els.gTemp.className='g-temp normal'}
    }
    els.toggle.textContent=g.on?'关':'开';if(g.on)els.toggle.classList.remove('success');else els.toggle.classList.add('success');
    els.slider.value=g.power;els.label.textContent=g.power+'%'
}
function renderDevices(){if(pumpEls.length!==state.pumps.length)buildPumpRows();if(genEls.length!==state.generators.length)buildGenRows();for(let i=0;i<state.pumps.length;i++)updatePumpRow(i);for(let i=0;i<state.generators.length;i++)updateGenRow(i);updateDeviceStatus()}
function checkOutage(){const allDamaged=state.generators.every(g=>g.damaged);if(allDamaged){if(!state.powerOutage){log('全厂停电','danger');playAlarm()}state.powerOutage=true;state.pumps.forEach(p=>{if(!p.fault)p.on=false});setMessage('⚡ 全厂停电','danger')}else state.powerOutage=false;renderDevices();updateUI()}
function updateDeviceStatus(){const pumpOn=state.pumps.filter(p=>p.on&&!p.fault).length;pumpSummary.textContent=`${pumpOn}/${state.pumpCount} 运行`;const genOk=state.generators.filter(g=>!g.damaged).length;genSummary.textContent=`${genOk}/4 正常`;outageDisplay.textContent=state.powerOutage?'是':'无';outageDisplay.style.color=state.powerOutage?'#ff5c5c':'#4dffc3'}
function setPowerSelect(sel){state.powerSelect=sel;psMain.classList.toggle('active',sel==='main');psBackup.classList.toggle('active',sel==='backup');playBeep(660,0.06,0.04);log(sel==='main'?'切换至主电源':'切换至备用电源','info');updateUI()}

function triggerRandomEvent(){
    const events=[
        ()=>{const add=Math.floor(rand(20,50));state.targetLoad=clamp(state.targetLoad+add,0,200);loadSlider.value=state.targetLoad;loadValue.textContent=state.targetLoad;log(`⚡ 电网需求突增 +${add}MW`,'warn');showEvent('⚡',`电网需求突增 +${add}MW`);playAlarm()},
        ()=>{const sub=Math.floor(rand(20,40));state.targetLoad=clamp(state.targetLoad-sub,0,200);loadSlider.value=state.targetLoad;loadValue.textContent=state.targetLoad;log(`🔻 电网需求骤降 -${sub}MW`,'warn');showEvent('🔻',`电网需求骤降 -${sub}MW`)},
        ()=>{const add=Math.floor(rand(20,40));state.temperature=clamp(state.temperature+add,TEMP_MIN,TEMP_MAX);log(`💧 冷却水泄漏 +${add}°C`,'danger');showEvent('💧',`冷却水泄漏 温度 +${add}°C`);playAlarm()},
        ()=>{const faultCount=Math.floor(rand(2,4));const pool=state.pumps.filter(p=>!p.fault);const faulted=[];for(let i=0;i<Math.min(faultCount,pool.length);i++){const pick=Math.floor(Math.random()*pool.length);const p=pool.splice(pick,1)[0];p.fault=true;p.on=false;p.repairCooldown=REPAIR_COOLDOWN;faulted.push('水泵'+(state.pumps.indexOf(p)+1))}if(faulted.length){log(`🌊 地震！${faulted.join('、')}故障`,'danger');showEvent('🌊','地震！'+faulted.join('、')+'故障');playAlarm();renderDevices()}}
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
        if(state.boricRemaining<=0){state.boricActive=false;state.boricContamination=BORIC_CONTAMINATION;log('硼酸污染生效！30秒内发电减半','danger');setMessage('☢️ 硼酸污染中','danger')}
    }
    if(state.boricContamination>0){state.boricContamination--;if(state.boricContamination<=0){log('硼酸污染已清除','success');setMessage('硼酸污染已清除','success')}}
    if(state.boricCooldown>0)state.boricCooldown=Math.max(0,state.boricCooldown-1);
    updateBoricUI();
    pushTempHistory();

    state.nextEventTime-=1;
    if(state.nextEventTime<=0&&!state.gameOver){
        const easyMul=state.easyMode?2:1;
        state.nextEventTime=Math.floor(60/diff().eventMul*rand(0.7,1.3)*easyMul);
        if(Math.random()<(state.easyMode?0.25:0.6))triggerRandomEvent()
    }

    let totalPumpPower=0;
    for(const pump of state.pumps)if(pump.on&&!pump.fault)totalPumpPower+=pump.power/100;
    const pumpDemand=totalPumpPower*PUMP_MAX_MW;
    let pumpPowered=true;
    if(state.powerSelect==='main'){if(state.mainPowerBus<pumpDemand)pumpPowered=false}
    else{if(state.backupPower<=0)pumpPowered=false}

    // 机组温度
    const envHeat=Math.max(0,(state.temperature-GEN_TEMP_ENV_BASE)/GEN_TEMP_ENV_SPAN)*GEN_TEMP_ENV_GAIN;
    const pressureHeat=Math.max(0,(state.reactorPressure-GEN_TEMP_PRESSURE_BASE)/GEN_TEMP_PRESSURE_SPAN)*GEN_TEMP_PRESSURE_GAIN;
    const pumpCoolFactor=(pumpPowered?totalPumpPower:0)/4;
    const pumpCool=pumpCoolFactor*GEN_TEMP_PUMP_COOL;

    for(const gen of state.generators){
        if(gen.damaged){gen.temp=Math.max(GEN_TEMP_INIT,gen.temp-GEN_TEMP_IDLE_COOL);continue}
        if(!gen.on){gen.temp=Math.max(GEN_TEMP_INIT,gen.temp-GEN_TEMP_IDLE_COOL);continue}
        const powerHeat=(gen.power/100)*GEN_TEMP_POWER_HEAT;
        const targetTemp=GEN_TEMP_INIT+powerHeat+envHeat+pressureHeat-pumpCool+GEN_TEMP_BASE_OFFSET;
        gen.temp=clamp(gen.temp+(targetTemp-gen.temp)*GEN_TEMP_RESPONSE,GEN_TEMP_INIT,GEN_TEMP_MAX);
        if(gen.temp>GEN_TEMP_FAULT){
            const failChance=(gen.temp-GEN_TEMP_FAULT)/100;
            if(Math.random()<failChance){
                gen.damaged=true;gen.on=false;gen.repairCooldown=REPAIR_COOLDOWN;
                log(`⚠️ 机组${state.generators.indexOf(gen)+1} 过热损坏！温度 ${Math.round(gen.temp)}°C`,'danger');
                if(diff().showPopup)setMessage(`⚠️ 机组${state.generators.indexOf(gen)+1} 过热损坏`,'danger');
                checkOutage();playAlarm()
            }
        }
    }

    let mainGenMW=0,backupGenMW=0,totalGenCapacity=0;
    for(const gen of state.generators){
        if(gen.on&&!gen.damaged){
            let eff=1;
            if(gen.temp>GEN_TEMP_WARN){eff=Math.max(0.4,1-(gen.temp-GEN_TEMP_WARN)/60)}
            const mw=(gen.power/100)*GEN_MAX_MW*eff;
            totalGenCapacity+=mw;
            if(gen.powerTarget==='main')mainGenMW+=mw;else backupGenMW+=mw
        }
    }
    state.mainPowerBus=mainGenMW;

    const rodFactor=1-(state.rodDepth/100)*0.9;
    const coolMul=1+(state.coolLevel-1)*0.05;
    let pumpEffect=pumpPowered?totalPumpPower*coolMul:0;
    pumpEffect=Math.max(pumpEffect,0.2);
    const eqTemp=clamp(TEMP_BASE+TEMP_SPAN*Math.pow(rodFactor,3)*(4/pumpEffect),TEMP_MIN,TEMP_MAX);
    const tempDelta=eqTemp-state.temperature;
    const step=clamp(tempDelta,-MAX_TEMP_RATE,MAX_TEMP_RATE);
    state.temperature=clamp(state.temperature+step,TEMP_MIN,TEMP_MAX);
    if(state.temperature>state.maxTempReached)state.maxTempReached=state.temperature;

    const pressureBase=8.0+(state.temperature-500)*0.006;
    state.reactorPressure+=(pressureBase-state.reactorPressure)*0.05;
    state.reactorPressure=clamp(state.reactorPressure,PRESSURE_MIN,PRESSURE_MAX);

    if(state.temperature<COLD_SHUTDOWN_TEMP)state.thermalPower=0;else state.thermalPower=((state.temperature-COLD_SHUTDOWN_TEMP)/400)*150;
    state.thermalPower=clamp(state.thermalPower,0,400);

    if(state.powerSelect==='backup'&&pumpPowered&&pumpDemand>0){state.backupPower=clamp(state.backupPower-pumpDemand*BACKUP_DRAIN_RATE,0,100)}
    else{state.backupPower=clamp(state.backupPower+backupGenMW*BACKUP_CHARGE_RATE,0,100)}

    let maxFromHeat=state.thermalPower*HEAT_TO_ELECTRIC;
    if(state.boricContamination>0)maxFromHeat*=0.5;
    state.electricPower=Math.min(maxFromHeat,totalGenCapacity);
    if(state.powerOutage)state.electricPower=0;

    const loadDiff=state.electricPower>1?(state.electricPower-state.targetLoad):0;
    const randomWalk=rand(-GRID_RANDOM_RANGE,GRID_RANDOM_RANGE);
    const selfStabilize=-state.gridDeviation*GRID_SELF_STABILIZE;
    let delta=loadDiff*GRID_SENSITIVITY+randomWalk+selfStabilize;
    if(Math.abs(state.gridDeviation)<2&&Math.abs(loadDiff)<15){delta-=state.gridDeviation*GRID_SNAP_BOOST*0.1}
    state.gridDeviation+=delta;
    state.gridDeviation=clamp(state.gridDeviation,-GRID_DEVIATION_LIMIT,GRID_DEVIATION_LIMIT);

    const kWhThisSecond=state.electricPower*1000/3600;
    state.totalEnergy+=kWhThisSecond;
    state.money+=kWhThisSecond*PRICE_PER_KWH;
    state.neutronFlux=clamp(20+(state.temperature/1500)*60,20,100);

    // 故障概率（简单模式 ×0.1）
    const fm=diff().faultMul*(state.easyMode?0.1:1);
    if(state.reactorPressure>15.0&&Math.random()<0.03*fm){const avail=state.pumps.filter(p=>!p.fault);if(avail.length>0){const pump=avail[Math.floor(Math.random()*avail.length)];pump.fault=true;pump.on=false;pump.repairCooldown=REPAIR_COOLDOWN;log(`压力过高，水泵${state.pumps.indexOf(pump)+1}故障`,'danger');if(diff().showPopup)setMessage(`⚠️ 水泵${state.pumps.indexOf(pump)+1}故障`,'danger');playAlarm()}}
    if(state.temperature>900&&Math.random()<0.02*fm){const avail=state.generators.filter(g=>!g.damaged);if(avail.length>0){const gen=avail[Math.floor(Math.random()*avail.length)];gen.damaged=true;gen.on=false;gen.repairCooldown=REPAIR_COOLDOWN;log(`高温，机组${state.generators.indexOf(gen)+1}损坏`,'danger');if(diff().showPopup)setMessage(`⚠️ 机组${state.generators.indexOf(gen)+1}损坏`,'danger');checkOutage();playAlarm()}}
    // 电网损坏（简单模式直接跳过）
    if(!state.easyMode && state.time>GRID_START_GRACE&&Math.abs(state.gridDeviation)>GRID_DAMAGE_THRESHOLD&&Math.random()<GRID_DAMAGE_CHANCE*fm){const avail=state.generators.filter(g=>!g.damaged);if(avail.length>0){const gen=avail[Math.floor(Math.random()*avail.length)];gen.damaged=true;gen.on=false;gen.repairCooldown=REPAIR_COOLDOWN;log(`电网异常，机组${state.generators.indexOf(gen)+1}损坏`,'danger');if(diff().showPopup)setMessage(`⚠️ 机组${state.generators.indexOf(gen)+1}损坏`,'danger');checkOutage();playAlarm()}}
    if(!pumpPowered&&Math.random()<0.03*fm){const avail=state.pumps.filter(p=>!p.fault&&p.on);if(avail.length>0){const pump=avail[Math.floor(Math.random()*avail.length)];pump.fault=true;pump.on=false;pump.repairCooldown=REPAIR_COOLDOWN;log(`供电不足，水泵${state.pumps.indexOf(pump)+1}故障`,'danger');if(diff().showPopup)setMessage(`⚠️ 水泵${state.pumps.indexOf(pump)+1}故障`,'danger')}}

    if(state.temperature>=MAX_TEMP&&!state.scramTriggered&&!state.gameOver)triggerEmergencyScram();
    if(state.meltdownActive){
        state.meltdownCounter-=1;meltdownTimer.textContent=state.meltdownCounter;
        if(state.meltdownCounter%5===0)playAlarm();
        if(state.meltdownCounter<=0){
            state.gameOver=true;if(state.updateTimer){clearInterval(state.updateTimer);state.updateTimer=null}
            setMessage('💥 核融爆炸','danger');log('核融爆炸，游戏结束','danger');
            statusDot.className='dot meltdown';statusText.textContent='爆炸';meltdownWarning.classList.remove('active');playAlarm();
            showScoreCard(false)
        }
    }
    if(state.temperature<COLD_SHUTDOWN_TEMP&&!state.coldShutdownLogged&&!state.gameOver){state.coldShutdownLogged=true;log('进入冷停堆，不发电','warn');log('点「启动反应堆」重启','info')}
    if(state.temperature>=COLD_SHUTDOWN_TEMP)state.coldShutdownLogged=false;

    renderDevices();updateUI();updateReliefUI();updateShop();updateMusic();drawTempChart()
}
function togglePause(){if(state.gameOver)return;state.paused=!state.paused;btnPause.textContent=state.paused?'▶ 继续':'⏸ 暂停';playBeep(660,0.06,0.04);if(!state.paused&&!state.rodAnimId&&Math.abs(state.rodTarget-state.rodDepth)>0.3&&!state.rodLocked){state.rodAnimId=requestAnimationFrame(animateRod)}}
function resetGame(){
    if(state.updateTimer){clearInterval(state.updateTimer);state.updateTimer=null}
    state.time=0;state.temperature=500;state.reactorPressure=8.0;state.electricPower=0;state.thermalPower=0;state.money=0;state.totalEnergy=0;
    state.gameOver=false;state.paused=false;state.meltdownActive=false;state.meltdownCounter=MELTDOWN_TIME;
    state.scramTriggered=false;state.coldShutdownLogged=false;
    state.gridDeviation=0;state.targetLoad=50;loadSlider.value=50;loadValue.textContent='50';
    state.pumpCount=4;state.coolLevel=1;state.maxTempReached=500;state.neutronFlux=50;
    meltdownWarning.classList.remove('active');scramModal.classList.remove('active');eventBanner.classList.remove('active');
    btnPause.textContent='⏸ 暂停';
    initDevices();rodSlider.value=ROD_DEFAULT;rodValue.textContent=ROD_DEFAULT+'%';
    rodLock.textContent='解锁';rodLock.classList.remove('active');rodSlider.disabled=false;
    psMain.classList.add('active');psBackup.classList.remove('active');
    terminalBody.innerHTML='';btnRestart.style.display='none';
    renderDevices();updateUI();updateReliefUI();updateShop();setMessage('已重置 · 按 H 查看快捷键','info');
    statusDot.className='dot running';statusText.textContent='运行中';log('系统重启完成','system');
    playBeep(1200,0.1,0.05);
    state.updateTimer=setInterval(()=>updatePhysics(),UPDATE_INTERVAL)
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
    if(loadHint){const sug=Math.round(state.electricPower);loadHint.textContent='建议: '+sug+' MW';const gap=Math.abs(state.targetLoad-sug);loadHint.style.color=gap<5?'#4dffc3':(gap<15?'#ff9f43':'#ff5c5c')}
    let mainMW=0,backupMW=0;
    for(const g of state.generators){if(g.on&&!g.damaged){const mw=(g.power/100)*GEN_MAX_MW;if(g.powerTarget==='main')mainMW+=mw;else backupMW+=mw}}
    psInfo.textContent=`主 ${mainMW.toFixed(0)}MW / 备 ${backupMW.toFixed(0)}MW`;
    const totalPumpPower=state.pumps.filter(p=>p.on&&!p.fault).reduce((s,p)=>s+p.power/100,0);
    const pumpDemand=totalPumpPower*PUMP_MAX_MW;
    let pumpPowered=true;
    if(state.powerSelect==='main'){if(state.mainPowerBus<pumpDemand)pumpPowered=false}else{if(state.backupPower<=0)pumpPowered=false}
    if(pumpPowered){pumpPowerStatus.textContent='正常';pumpPowerStatus.style.color='#4dffc3'}else{pumpPowerStatus.textContent='不足 ⚠️';pumpPowerStatus.style.color='#ff5c5c'}
    updateDeviceStatus();
    if(state.gameOver){statusDot.className='dot meltdown';statusText.textContent='爆炸'}
    else if(state.meltdownActive){statusDot.className='dot meltdown';statusText.textContent='核融'}
    else if(state.temperature>1200){statusDot.className='dot danger';statusText.textContent='⚠️ 危急'}
    else if(state.temperature>900){statusDot.className='dot warning';statusText.textContent='⚠️ 高温'}
    else if(state.temperature<COLD_SHUTDOWN_TEMP){statusDot.className='dot warning';statusText.textContent='❄️ 冷停堆'}
    else{statusDot.className='dot running';statusText.textContent='运行中'}
    btnPause.disabled=state.gameOver;btnScram.disabled=state.gameOver;
    btnRestart.style.display=(state.temperature<COLD_SHUTDOWN_TEMP&&!state.gameOver)?'':'none';
    if(state.meltdownActive&&!state.gameOver){meltdownWarning.classList.add('active');meltdownTimer.textContent=state.meltdownCounter}else{meltdownWarning.classList.remove('active')}
    btnSave.disabled=state.gameOver;btnSave.style.opacity=state.gameOver?'0.4':'1';btnSave.style.cursor=state.gameOver?'not-allowed':'pointer'
}
function showScoreCard(victory){
    const ctx=scoreCanvas.getContext('2d');const w=scoreCanvas.width,h=scoreCanvas.height;
    const grad=ctx.createLinearGradient(0,0,0,h);grad.addColorStop(0,'#0d1117');grad.addColorStop(1,'#161b22');
    ctx.fillStyle=grad;ctx.fillRect(0,0,w,h);
    const accent=victory?'#4dffc3':'#ff5c5c';
    const grad2=ctx.createLinearGradient(0,0,w,0);grad2.addColorStop(0,'transparent');grad2.addColorStop(0.5,accent);grad2.addColorStop(1,'transparent');
    ctx.fillStyle=grad2;ctx.fillRect(0,0,w,4);
    ctx.fillStyle=accent;ctx.font='bold 26px Consolas, monospace';ctx.textAlign='center';
    ctx.fillText(victory?'🏆 通关成功':'💥 核融爆炸',w/2,52);
    ctx.fillStyle='#6e7681';ctx.font='14px Consolas, monospace';ctx.fillText('Reactor Rising · 本局成绩',w/2,78);
    ctx.strokeStyle='rgba(48,54,61,0.6)';ctx.lineWidth=1;ctx.beginPath();ctx.moveTo(40,98);ctx.lineTo(w-40,98);ctx.stroke();
    const stats=[
        {label:'存活时间',value:Math.round(state.time)+' 秒',color:'#58a6ff'},
        {label:'总收益',value:state.money.toFixed(2)+' 元',color:'#ffd740'},
        {label:'总发电量',value:state.totalEnergy.toFixed(1)+' kWh',color:'#58a6ff'},
        {label:'最高温度',value:Math.round(state.maxTempReached)+' °C',color:'#ff9f43'},
        {label:'最终温度',value:Math.round(state.temperature)+' °C',color:'#ff9f43'},
        {label:'难度',value:diff().label+(state.easyMode?' + 简单模式':''),color:'#4dffc3'},
        {label:'冷却等级',value:'Lv.'+state.coolLevel,color:'#4dffc3'},
        {label:'水泵数量',value:state.pumpCount+' 台',color:'#4dffc3'},
        {label:'最终负荷',value:state.targetLoad+' MW',color:'#ffd740'}
    ];
    stats.forEach((s,i)=>{const y=125+i*28;ctx.fillStyle='#8b949e';ctx.font='14px Consolas, monospace';ctx.textAlign='left';ctx.fillText(s.label,w/2-120,y);ctx.fillStyle=s.color;ctx.font='bold 15px Consolas, monospace';ctx.textAlign='right';ctx.fillText(s.value,w/2+130,y)});
    ctx.textAlign='center';ctx.fillStyle='#6e7681';ctx.font='12px Consolas, monospace';
    const now=new Date();const timeStr=`${now.getFullYear()}-${String(now.getMonth()+1).padStart(2,'0')}-${String(now.getDate()).padStart(2,'0')} ${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;
    ctx.fillText(timeStr,w/2,h-15);scoreModal.classList.add('active')
}
function saveScoreImage(){try{const url=scoreCanvas.toDataURL('image/png');const a=document.createElement('a');a.href=url;a.download=`ReactorRising_${Date.now()}.png`;document.body.appendChild(a);a.click();document.body.removeChild(a);setMessage('成绩卡已保存','success')}catch(e){setMessage('保存失败','warn')}}

function openSaveNameModal(){if(state.gameOver){setMessage('游戏已结束，无法存档','warn');return}wasPausedBeforeSave=state.paused;if(!state.paused){state.paused=true;btnPause.textContent='▶ 继续'}saveNameInput.value='';saveNameModal.classList.add('active');setTimeout(()=>{try{saveNameInput.focus()}catch(e){}},100)}
function closeSaveNameModal(){saveNameModal.classList.remove('active');if(!wasPausedBeforeSave&&!state.gameOver){state.paused=false;btnPause.textContent='⏸ 暂停';if(!state.rodAnimId&&Math.abs(state.rodTarget-state.rodDepth)>0.3&&!state.rodLocked)state.rodAnimId=requestAnimationFrame(animateRod)}}
function confirmSave(){
    const raw=(saveNameInput.value||'').trim();
    if(!raw){setMessage('名字不能为空','warn');try{saveNameInput.focus()}catch(e){};return}
    const safeName=raw.replace(/[\\/:*?"<>|]/g,'_').slice(0,24);
    try{
        const snapshot=JSON.parse(JSON.stringify(state));snapshot.updateTimer=null;snapshot.rodAnimId=null;
        const saveData={format:'fyd',version:SAVE_VERSION,saveTime:new Date().toISOString(),playerName:raw,state:snapshot};
        const json=JSON.stringify(saveData,null,2);
        const fileName=safeName+'.fyd';
        const uri='data:application/json;charset=utf-8,'+encodeURIComponent(json);
        const a=document.createElement('a');a.href=uri;a.download=fileName;a.rel='noopener';a.style.position='fixed';a.style.left='-9999px';
        document.body.appendChild(a);a.click();
        setTimeout(()=>{try{document.body.removeChild(a)}catch(e){}},800);
        closeSaveNameModal();playBeep(1400,0.15,0.06);
        log(`💾 存档已保存：${fileName}`,'success');setMessage(`💾 存档已保存：${fileName}`,'success')
    }catch(err){
        console.error('[存档] 保存失败:',err);
        setMessage('保存失败：'+(err&&err.message?err.message:'未知错误'),'danger');log('保存失败：'+(err&&err.message?err.message:'未知错误'),'danger')
    }
}
function loadGameFromFile(file){
    const reader=new FileReader();
    reader.onload=(e)=>{
        let data;
        try{data=JSON.parse(e.target.result)}catch(err){setMessage('存档文件不是有效的 .fyd 格式','danger');log('读档失败：JSON 解析错误','danger');if(!wasPausedBeforeLoad&&!state.gameOver){state.paused=false;btnPause.textContent='⏸ 暂停'}return}
        if(!data||data.format!=='fyd'||!data.state){setMessage('存档文件格式不正确','danger');log('读档失败：缺少必要字段','danger');if(!wasPausedBeforeLoad&&!state.gameOver){state.paused=false;btnPause.textContent='⏸ 暂停'}return}
        const st=data.state;
        if(typeof st.temperature!=='number'||typeof st.money!=='number'||!Array.isArray(st.pumps)){setMessage('存档数据已损坏','danger');log('读档失败：数据校验不通过','danger');if(!wasPausedBeforeLoad&&!state.gameOver){state.paused=false;btnPause.textContent='⏸ 暂停'}return}
        if(state.updateTimer){clearInterval(state.updateTimer);state.updateTimer=null}
        if(state.rodAnimId){cancelAnimationFrame(state.rodAnimId);state.rodAnimId=null}
        Object.keys(st).forEach(k=>{if(k==='updateTimer'||k==='rodAnimId')return;state[k]=st[k]});
        state.updateTimer=null;state.rodAnimId=null;
        if(!Array.isArray(state.generators))state.generators=[];
        if(!Array.isArray(state.tempHistory))state.tempHistory=[];
        if(typeof state.pumpCount!=='number')state.pumpCount=state.pumps.length||4;
        if(typeof state.coolLevel!=='number')state.coolLevel=1;
        if(typeof state.difficulty!=='string'||!DIFFICULTY[state.difficulty])state.difficulty='normal';
        state.generators.forEach(g=>{if(typeof g.temp!=='number')g.temp=GEN_TEMP_INIT});
        if(typeof state.easyMode!=='boolean')state.easyMode=false;
        btnEasy.classList.toggle('active',state.easyMode);
        difficultySel.value=state.difficulty;
        rebuildAllUI();
        state.updateTimer=setInterval(()=>updatePhysics(),UPDATE_INTERVAL);
        playBeep(1200,0.15,0.06);log(`📂 已载入存档：${data.playerName||'未命名'}`,'success');setMessage(`📂 已载入存档：${data.playerName||'未命名'}`,'success')
    };
    reader.onerror=()=>{setMessage('读取文件失败','danger');log('读档失败：文件读取错误','danger');if(!wasPausedBeforeLoad&&!state.gameOver){state.paused=false;btnPause.textContent='⏸ 暂停'}};
    reader.readAsText(file)
}
function rebuildAllUI(){
    pumpEls=[];genEls=[];buildPumpRows();buildGenRows();
    rodSlider.value=Math.round(state.rodDepth||0);rodValue.textContent=Math.round(state.rodDepth||0)+'%';
    rodLock.textContent=state.rodLocked?'锁定':'解锁';rodLock.classList.toggle('active',!!state.rodLocked);rodSlider.disabled=!!state.rodLocked;
    loadSlider.value=state.targetLoad||50;loadValue.textContent=state.targetLoad||50;
    psMain.classList.toggle('active',state.powerSelect==='main');psBackup.classList.toggle('active',state.powerSelect==='backup');
    btnPause.textContent=state.paused?'▶ 继续':'⏸ 暂停';eventBanner.classList.remove('active');
    btnEasy.classList.toggle('active',state.easyMode);
    renderDevices();updateUI();updateReliefUI();updateShop();updateBoricUI();drawTempChart();
    if(!state.paused&&!state.gameOver&&!state.rodLocked&&Math.abs(state.rodTarget-state.rodDepth)>0.3)state.rodAnimId=requestAnimationFrame(animateRod)
}

function handleKeydown(e){
    if(e.target.tagName==='INPUT'||e.target.tagName==='SELECT')return;
    if(e.key===' '||e.key==='p'||e.key==='P'){e.preventDefault();togglePause()}
    if(e.key==='r'||e.key==='R')resetGame();
    if(e.key==='s'||e.key==='S')scram();
    if(e.key==='v'||e.key==='V')doRelief();
    if(e.key==='b'||e.key==='B')doBoricInjection();
    if(e.key==='m'||e.key==='M')toggleMusic();
    if(e.key==='h'||e.key==='H')hotkeyOverlay.classList.toggle('active');
    if(e.key==='Escape'){if(saveNameModal.classList.contains('active'))closeSaveNameModal();else if(hotkeyOverlay.classList.contains('active'))hotkeyOverlay.classList.remove('active');else if(scramModal.classList.contains('active'))closeScramModal();else if(scoreModal.classList.contains('active'))scoreModal.classList.remove('active');else if(helpModal.classList.contains('active'))closeHelp()}
}
function openHelp(){wasPausedBeforeHelp=state.paused;if(!state.gameOver&&!state.paused){state.paused=true;btnPause.textContent='▶ 继续'}helpModal.classList.add('active')}
function closeHelp(){helpModal.classList.remove('active');if(!wasPausedBeforeHelp&&!state.gameOver){state.paused=false;btnPause.textContent='⏸ 暂停';if(!state.rodAnimId&&Math.abs(state.rodTarget-state.rodDepth)>0.3&&!state.rodLocked)state.rodAnimId=requestAnimationFrame(animateRod)}}

function setupPWA(){
    const manifest={name:'Reactor Rising',short_name:'Reactor Rising',description:'实时核反应堆模拟游戏',start_url:'.',display:'standalone',background_color:'#0d1117',theme_color:'#00d4aa',orientation:'portrait',icons:[{src:"data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 512 512'%3E%3Crect width='512' height='512' fill='%230d1117'/%3E%3Ccircle cx='256' cy='256' r='140' fill='none' stroke='%2300d4aa' stroke-width='16'/%3E%3Ccircle cx='256' cy='256' r='60' fill='%2300d4aa'/%3E%3Ccircle cx='256' cy='256' r='20' fill='%23fff'/%3E%3C/svg%3E",sizes:'512x512',type:'image/svg+xml',purpose:'any maskable'}]};
    const manifestUrl='data:application/manifest+json;charset=utf-8,'+encodeURIComponent(JSON.stringify(manifest));
    let link=document.querySelector('link[rel="manifest"]');if(!link){link=document.createElement('link');link.rel='manifest';document.head.appendChild(link)}link.href=manifestUrl;
    let deferredPrompt=null;
    window.addEventListener('beforeinstallprompt',(e)=>{e.preventDefault();deferredPrompt=e;btnInstall.style.display='';});
    btnInstall.addEventListener('click',async()=>{if(!deferredPrompt){setMessage('当前环境不支持安装，请用浏览器菜单添加到主屏幕','warn');return}deferredPrompt.prompt();const{outcome}=await deferredPrompt.userChoice;if(outcome==='accepted'){log('PWA 已安装','success');setMessage('✅ 已添加到主屏幕','success')}deferredPrompt=null;btnInstall.style.display='none'});
    window.addEventListener('appinstalled',()=>{btnInstall.style.display='none';log('PWA 安装成功','success')});
}

function init(){
    state.temperature=500;initDevices();renderDevices();updateUI();updateReliefUI();updateShop();updateBoricUI();
    log('系统启动','system');log(`难度：${diff().label}`,'system');log('按 H 查看所有快捷键','system');

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
    rodLock.addEventListener('click',()=>{state.rodLocked=!state.rodLocked;rodLock.textContent=state.rodLocked?'锁定':'解锁';rodLock.classList.toggle('active',state.rodLocked);rodSlider.disabled=state.rodLocked;playBeep(state.rodLocked?440:880,0.06,0.04);if(state.rodLocked){if(state.rodAnimId){cancelAnimationFrame(state.rodAnimId);state.rodAnimId=null}rodSlider.value=Math.round(state.rodDepth);rodValue.textContent=Math.round(state.rodDepth)+'%'}else if(!state.paused&&Math.abs(state.rodTarget-state.rodDepth)>0.3&&!state.rodAnimId){state.rodAnimId=requestAnimationFrame(animateRod)}});
    loadSlider.addEventListener('input',()=>{const val=parseInt(loadSlider.value);state.targetLoad=val;loadValue.textContent=val});
    difficultySel.addEventListener('change',()=>{state.difficulty=difficultySel.value;log(`难度切换至：${diff().label}`,'system');setMessage(`难度：${diff().label}`,'info')});
    buyPump5.addEventListener('click',()=>{if(state.pumpCount>=5){setMessage('已有5台水泵','warn');return}if(state.money<PRICE_PUMP5){setMessage('余额不足','warn');return}state.money-=PRICE_PUMP5;state.pumpCount=5;state.pumps.push({on:true,fault:false,power:100,repairCooldown:0});playBeep(1400,0.15,0.06);log(`💰 购买第 5 台水泵，花费 ${PRICE_PUMP5} 元`,'success');setMessage('💧 购买第 5 台水泵成功','success');renderDevices();updateUI();updateShop()});
    buyCool.addEventListener('click',()=>{if(state.money<PRICE_COOL){setMessage('余额不足','warn');return}state.money-=PRICE_COOL;state.coolLevel++;playBeep(1400,0.15,0.06);log(`💰 冷却效率升级至 Lv.${state.coolLevel}，花费 ${PRICE_COOL} 元`,'success');setMessage(`⚡ 冷却效率升级至 Lv.${state.coolLevel}`,'success');renderDevices();updateUI();updateShop()});

    // 简单模式按钮
    btnEasy.addEventListener('click',()=>{
        state.easyMode=!state.easyMode;
        btnEasy.classList.toggle('active',state.easyMode);
        if(state.easyMode){
            log('🟢 简单模式已开启：故障概率大幅降低，事件减半，电网不损坏','success');
            setMessage('🟢 简单模式已开启','success');
        }else{
            log('简单模式已关闭','info');
            setMessage('简单模式已关闭','info');
        }
    });

    btnSave.addEventListener('click',openSaveNameModal);
    btnCloseSaveName.addEventListener('click',closeSaveNameModal);
    btnCancelSaveName.addEventListener('click',closeSaveNameModal);
    btnConfirmSaveName.addEventListener('click',confirmSave);
    saveNameModal.addEventListener('click',(e)=>{if(e.target===saveNameModal)closeSaveNameModal()});
    saveNameInput.addEventListener('keydown',(e)=>{if(e.key==='Enter'){e.preventDefault();confirmSave()}else if(e.key==='Escape'){e.preventDefault();closeSaveNameModal()}});
    btnLoad.addEventListener('click',()=>{
        wasPausedBeforeLoad=state.paused;if(!state.paused&&!state.gameOver){state.paused=true;btnPause.textContent='▶ 继续'}
        const input=document.createElement('input');input.type='file';input.accept='.fyd,application/json,.json';input.style.display='none';
        let handled=false;
        const cleanup=()=>{if(handled)return;handled=true;setTimeout(()=>{try{document.body.removeChild(input)}catch(e){}},100)};
        const restorePause=()=>{if(!wasPausedBeforeLoad&&!state.gameOver){state.paused=false;btnPause.textContent='⏸ 暂停';if(!state.rodAnimId&&Math.abs(state.rodTarget-state.rodDepth)>0.3&&!state.rodLocked)state.rodAnimId=requestAnimationFrame(animateRod)}};
        const onFocus=()=>{window.removeEventListener('focus',onFocus);setTimeout(()=>{if(!handled){cleanup();restorePause()}},400)};
        input.onchange=(e)=>{cleanup();const file=e.target.files&&e.target.files[0];if(file){loadGameFromFile(file)}else{restorePause()}};
        window.addEventListener('focus',onFocus);document.body.appendChild(input);input.click()
    });

    window.addEventListener('resize',()=>drawTempChart());
    document.addEventListener('keydown',handleKeydown);
    setupPWA();
    state.updateTimer=setInterval(()=>updatePhysics(),UPDATE_INTERVAL);
    drawTempChart();
    console.log('[系统] Reactor Rising 已启动')
}
init();
})();
</script>
</body>
</html>
