:root{
  --paper: #F4F5F0;
  --paper-2: #EAEDE6;
  --paper-3: #FFFFFF;
  --ink: #1C2420;
  --ink-soft: #5E6A62;
  --ink-faint: #8B968D;
  --brand: #24464B;
  --brand-light: #3E7873;
  --positive: #3E7A57;
  --positive-bg: #E4EEE3;
  --pending: #B37F2A;
  --pending-bg: #F4E9D6;
  --danger: #A34B3F;
  --danger-bg: #F3E3DF;
  --border: #DCE0D6;
  --radius: 16px;
  --shadow: 0 1px 2px rgba(28,36,32,0.04), 0 6px 20px rgba(28,36,32,0.06);
  --font-display: 'Fraunces', serif;
  --font-body: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-mono: 'IBM Plex Mono', monospace;
}

*{box-sizing:border-box;}
html,body{margin:0;padding:0;}
body{
  background:var(--paper);
  color:var(--ink);
  font-family:var(--font-body);
  -webkit-tap-highlight-color:transparent;
  overscroll-behavior:none;
}
button{font-family:inherit;}
.hidden{display:none !important;}

#app{
  max-width:560px;
  margin:0 auto;
  min-height:100vh;
  display:flex;
  flex-direction:column;
  position:relative;
  padding-bottom:calc(78px + env(safe-area-inset-bottom));
}

/* ---------- TOPBAR ---------- */
.topbar{
  display:flex;
  align-items:center;
  justify-content:space-between;
  padding:calc(18px + env(safe-area-inset-top)) 20px 16px;
  position:sticky;
  top:0;
  background:var(--paper);
  z-index:10;
  border-bottom:1px solid var(--border);
}
.topbar-brand{display:flex;align-items:center;gap:12px;}
.brand-mark{
  font-family:var(--font-display);
  font-weight:600;
  font-size:15px;
  background:var(--brand);
  color:var(--paper);
  width:38px;height:38px;
  border-radius:10px;
  display:flex;align-items:center;justify-content:center;
  letter-spacing:0.5px;
}
.brand-text h1{
  font-family:var(--font-display);
  font-size:19px;
  font-weight:600;
  margin:0;
  color:var(--ink);
  letter-spacing:-0.2px;
}
.brand-text p{
  margin:1px 0 0;
  font-size:12.5px;
  color:var(--ink-faint);
}
.icon-btn{
  background:none;border:none;color:var(--ink-soft);
  width:40px;height:40px;border-radius:50%;
  display:flex;align-items:center;justify-content:center;
  cursor:pointer;
}
.icon-btn:active{background:var(--paper-2);}

/* ---------- MAIN ---------- */
#main-content{flex:1;padding:18px 18px 8px;}
.view{animation:fadein .18s ease;}
@keyframes fadein{from{opacity:0;transform:translateY(4px);}to{opacity:1;transform:translateY(0);}}

.section-head{
  display:flex;align-items:center;justify-content:space-between;
  margin:22px 0 12px;
}
.section-head:first-child{margin-top:2px;}
.section-head h2{
  font-family:var(--font-display);
  font-size:16.5px;
  font-weight:600;
  margin:0;
  color:var(--ink);
}

/* ---------- STATS ---------- */
.stats-grid{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:10px;
}
.stat-card{
  background:var(--paper-3);
  border:1px solid var(--border);
  border-radius:var(--radius);
  padding:16px 16px;
  display:flex;flex-direction:column;gap:6px;
  box-shadow:var(--shadow);
}
.stat-card.stat-accent{
  background:var(--brand);
  border-color:var(--brand);
}
.stat-card.stat-accent .stat-label{color:rgba(244,245,240,0.75);}
.stat-card.stat-accent .stat-value{color:var(--paper);}
.stat-label{
  font-size:12px;
  color:var(--ink-faint);
  font-weight:500;
  letter-spacing:0.2px;
}
.stat-value{
  font-family:var(--font-mono);
  font-size:21px;
  font-weight:600;
  color:var(--ink);
  font-variant-numeric:tabular-nums;
}
.stat-card.stat-small .stat-value{font-size:19px;}

/* ---------- LIST / CARDS ---------- */
.list{display:flex;flex-direction:column;gap:10px;}

.item-card{
  background:var(--paper-3);
  border:1px solid var(--border);
  border-radius:var(--radius);
  padding:14px 16px;
  box-shadow:var(--shadow);
  display:flex;
  flex-direction:column;
  gap:4px;
  cursor:pointer;
}
.item-top{display:flex;justify-content:space-between;align-items:flex-start;gap:10px;}
.item-title{font-weight:600;font-size:15px;color:var(--ink);}
.item-sub{font-size:12.5px;color:var(--ink-faint);}
.item-amount{
  font-family:var(--font-mono);
  font-weight:600;
  font-size:15px;
  white-space:nowrap;
}
.item-amount.positive{color:var(--positive);}
.item-amount.pending{color:var(--pending);}

.badge{
  display:inline-flex;
  align-items:center;
  font-size:11px;
  font-weight:600;
  padding:3px 9px;
  border-radius:20px;
  letter-spacing:0.2px;
}
.badge-positive{background:var(--positive-bg);color:var(--positive);}
.badge-pending{background:var(--pending-bg);color:var(--pending);}

.progress-track{
  height:6px;background:var(--paper-2);border-radius:6px;overflow:hidden;margin-top:6px;
}
.progress-fill{height:100%;background:var(--brand-light);border-radius:6px;}

/* payment item - ledger ticket style */
.pago-card{
  background:var(--paper-3);
  border:1px solid var(--border);
  border-radius:var(--radius);
  padding:14px 16px;
  box-shadow:var(--shadow);
  display:flex;
  align-items:center;
  justify-content:space-between;
  gap:10px;
  cursor:pointer;
  position:relative;
}
.pago-left{display:flex;flex-direction:column;gap:3px;min-width:0;}
.pago-cliente{font-weight:600;font-size:14.5px;color:var(--ink);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.pago-proyecto{font-size:12.5px;color:var(--ink-faint);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.pago-fecha{font-size:11.5px;color:var(--ink-faint);}
.pago-right{text-align:right;flex-shrink:0;}
.pago-valor{font-family:var(--font-mono);font-weight:700;font-size:15.5px;color:var(--positive);}

/* ---------- EMPTY STATES ---------- */
.empty-state{
  text-align:center;
  padding:40px 20px;
  color:var(--ink-faint);
}
.empty-title{font-family:var(--font-display);font-size:15.5px;color:var(--ink-soft);margin:0 0 4px;font-weight:600;}
.empty-sub{font-size:13px;margin:0;}

/* ---------- BUTTONS ---------- */
.btn-ghost{
  background:none;border:1px solid var(--border);
  color:var(--brand);
  font-weight:600;font-size:13px;
  padding:7px 13px;
  border-radius:20px;
  cursor:pointer;
}
.btn-ghost:active{background:var(--paper-2);}

.btn-primary{
  background:var(--brand);
  color:var(--paper);
  border:none;
  font-weight:600;font-size:14px;
  padding:12px 18px;
  border-radius:12px;
  cursor:pointer;
  flex:1;
}
.btn-secondary{
  background:var(--paper-2);
  color:var(--ink);
  border:1px solid var(--border);
  font-weight:600;font-size:14px;
  padding:12px 18px;
  border-radius:12px;
  cursor:pointer;
  flex:1;
}
.btn-danger{
  background:var(--danger);
  color:#fff;
  border:none;
  font-weight:600;font-size:14px;
  padding:12px 18px;
  border-radius:12px;
  cursor:pointer;
  width:100%;
}
.btn-row{display:flex;gap:10px;margin-top:12px;}

/* ---------- FILTER ---------- */
.filter-row{margin-bottom:14px;}
select{
  width:100%;
  padding:11px 14px;
  border-radius:12px;
  border:1px solid var(--border);
  background:var(--paper-3);
  font-family:var(--font-body);
  font-size:14px;
  color:var(--ink);
}

/* ---------- CARD BLOCK (ajustes) ---------- */
.card-block{
  background:var(--paper-3);
  border:1px solid var(--border);
  border-radius:var(--radius);
  padding:16px;
  margin-bottom:12px;
  box-shadow:var(--shadow);
}
.card-block.card-danger{border-color:#E4CFC9;}
.card-title{font-weight:600;font-size:14.5px;margin:0 0 4px;}
.card-desc{font-size:13px;color:var(--ink-soft);margin:0;line-height:1.5;}

/* ---------- FAB ---------- */
.fab{
  position:fixed;
  right:calc(50% - 260px + 20px);
  bottom:calc(88px + env(safe-area-inset-bottom));
  width:56px;height:56px;
  border-radius:50%;
  background:var(--brand);
  color:var(--paper);
  border:none;
  box-shadow:0 6px 18px rgba(36,70,75,0.35);
  display:flex;align-items:center;justify-content:center;
  cursor:pointer;
  z-index:20;
}
.fab:active{transform:scale(0.94);}
@media (max-width:600px){
  .fab{right:20px;}
}

/* ---------- BOTTOM NAV ---------- */
.bottom-nav{
  position:fixed;
  bottom:0;left:0;right:0;
  max-width:560px;
  margin:0 auto;
  background:var(--paper-3);
  border-top:1px solid var(--border);
  display:flex;
  padding:8px 6px calc(8px + env(safe-area-inset-bottom));
  z-index:15;
}
.nav-btn{
  flex:1;
  background:none;border:none;
  display:flex;flex-direction:column;align-items:center;gap:3px;
  color:var(--ink-faint);
  padding:6px 2px;
  border-radius:12px;
  cursor:pointer;
}
.nav-btn span{font-size:10.5px;font-weight:600;}
.nav-btn.active{color:var(--brand);}

/* ---------- MODAL ---------- */
.modal-overlay{
  position:fixed;inset:0;
  background:rgba(28,36,32,0.42);
  display:flex;align-items:flex-end;
  z-index:50;
}
.modal-sheet{
  background:var(--paper);
  width:100%;
  max-width:560px;
  margin:0 auto;
  border-radius:22px 22px 0 0;
  padding:22px 20px calc(22px + env(safe-area-inset-bottom));
  max-height:88vh;
  overflow-y:auto;
  animation:slideup .22s ease;
}
@keyframes slideup{from{transform:translateY(24px);opacity:0;}to{transform:translateY(0);opacity:1;}}

.modal-title{
  font-family:var(--font-display);
  font-size:18px;font-weight:600;
  margin:0 0 16px;
}
.field{margin-bottom:14px;}
.field label{
  display:block;font-size:12.5px;font-weight:600;
  color:var(--ink-soft);margin-bottom:6px;
}
.field input, .field select, .field textarea{
  width:100%;
  padding:12px 13px;
  border-radius:11px;
  border:1px solid var(--border);
  background:var(--paper-3);
  font-family:var(--font-body);
  font-size:15px;
  color:var(--ink);
}
.field input:focus, .field select:focus, .field textarea:focus{
  outline:2px solid var(--brand-light);
  outline-offset:1px;
}
.field-hint{font-size:11.5px;color:var(--ink-faint);margin-top:4px;}
.modal-actions{display:flex;gap:10px;margin-top:6px;}
.modal-close{
  position:absolute; top:16px; right:16px;
  background:var(--paper-2); border:none; width:32px;height:32px;
  border-radius:50%; color:var(--ink-soft); cursor:pointer;
  display:flex;align-items:center;justify-content:center;
}
.link-danger{
  background:none;border:none;color:var(--danger);
  font-size:13px;font-weight:600;cursor:pointer;
  margin-top:10px;
  padding:6px 0;
}

/* ---------- TOAST ---------- */
.toast{
  position:fixed;
  bottom:calc(96px + env(safe-area-inset-bottom));
  left:50%;
  transform:translateX(-50%);
  background:var(--ink);
  color:var(--paper);
  padding:10px 18px;
  border-radius:20px;
  font-size:13px;
  font-weight:500;
  z-index:60;
  box-shadow:var(--shadow);
  white-space:nowrap;
}

::-webkit-scrollbar{width:0;height:0;}
