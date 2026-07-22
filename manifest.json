/* ============================================================
   CUENTAS CLARAS — lógica de la app
   Almacenamiento 100% local en el dispositivo, vía IndexedDB.
   ============================================================ */

const DB_NAME = 'cuentasClarasDB';
const DB_VERSION = 1;
let db = null;

function openDB() {
  return new Promise((resolve, reject) => {
    const req = indexedDB.open(DB_NAME, DB_VERSION);
    req.onupgradeneeded = (e) => {
      const _db = e.target.result;
      if (!_db.objectStoreNames.contains('clientes')) {
        _db.createObjectStore('clientes', { keyPath: 'id', autoIncrement: true });
      }
      if (!_db.objectStoreNames.contains('proyectos')) {
        const p = _db.createObjectStore('proyectos', { keyPath: 'id', autoIncrement: true });
        p.createIndex('clienteId', 'clienteId', { unique: false });
      }
      if (!_db.objectStoreNames.contains('pagos')) {
        const pg = _db.createObjectStore('pagos', { keyPath: 'id', autoIncrement: true });
        pg.createIndex('proyectoId', 'proyectoId', { unique: false });
      }
    };
    req.onsuccess = (e) => { db = e.target.result; resolve(db); };
    req.onerror = (e) => reject(e.target.error);
  });
}

function tx(storeName, mode = 'readonly') {
  return db.transaction(storeName, mode).objectStore(storeName);
}

function idbAll(storeName) {
  return new Promise((resolve, reject) => {
    const req = tx(storeName).getAll();
    req.onsuccess = () => resolve(req.result);
    req.onerror = () => reject(req.error);
  });
}
function idbGet(storeName, id) {
  return new Promise((resolve, reject) => {
    const req = tx(storeName).get(id);
    req.onsuccess = () => resolve(req.result);
    req.onerror = () => reject(req.error);
  });
}
function idbPut(storeName, obj) {
  return new Promise((resolve, reject) => {
    const req = tx(storeName, 'readwrite').put(obj);
    req.onsuccess = () => resolve(req.result);
    req.onerror = () => reject(req.error);
  });
}
function idbDelete(storeName, id) {
  return new Promise((resolve, reject) => {
    const req = tx(storeName, 'readwrite').delete(id);
    req.onsuccess = () => resolve();
    req.onerror = () => reject(req.error);
  });
}
function idbClear(storeName) {
  return new Promise((resolve, reject) => {
    const req = tx(storeName, 'readwrite').clear();
    req.onsuccess = () => resolve();
    req.onerror = () => reject(req.error);
  });
}

/* ---------------- Helpers de formato ---------------- */
const fmtMoney = new Intl.NumberFormat('es-CO', { style: 'currency', currency: 'COP', maximumFractionDigits: 0 });
const fmtDate = new Intl.DateTimeFormat('es-CO', { day: '2-digit', month: 'short', year: 'numeric' });

function money(n) { return fmtMoney.format(n || 0); }
function dateStr(iso) {
  if (!iso) return '';
  const d = new Date(iso + 'T00:00:00');
  return fmtDate.format(d);
}
function todayISO() {
  const d = new Date();
  return d.toISOString().slice(0, 10);
}

function toast(msg) {
  const el = document.getElementById('toast');
  el.textContent = msg;
  el.classList.remove('hidden');
  clearTimeout(toast._t);
  toast._t = setTimeout(() => el.classList.add('hidden'), 2200);
}

/* ---------------- Estado en memoria (cache) ---------------- */
let CACHE = { clientes: [], proyectos: [], pagos: [] };

async function refreshCache() {
  const [clientes, proyectos, pagos] = await Promise.all([
    idbAll('clientes'), idbAll('proyectos'), idbAll('pagos')
  ]);
  CACHE.clientes = clientes.sort((a, b) => a.nombre.localeCompare(b.nombre));
  CACHE.proyectos = proyectos;
  CACHE.pagos = pagos.sort((a, b) => (b.fecha || '').localeCompare(a.fecha || '') || b.id - a.id);
}

function clienteNombre(id) {
  const c = CACHE.clientes.find(c => c.id === id);
  return c ? c.nombre : 'Cliente eliminado';
}
function proyectoById(id) { return CACHE.proyectos.find(p => p.id === id); }
function proyectosDeCliente(clienteId) { return CACHE.proyectos.filter(p => p.clienteId === clienteId); }
function pagosDeProyecto(proyectoId) { return CACHE.pagos.filter(p => p.proyectoId === proyectoId); }
function totalRecibidoProyecto(proyectoId) {
  return pagosDeProyecto(proyectoId).reduce((s, p) => s + Number(p.valor || 0), 0);
}

/* ============================================================
   NAVEGACIÓN
   ============================================================ */
const views = ['dashboard', 'clientes', 'proyectos', 'pagos', 'ajustes'];
const subtitles = {
  dashboard: 'Resumen general',
  clientes: 'Directorio de clientes',
  proyectos: 'Proyectos y servicios',
  pagos: 'Historial de pagos',
  ajustes: 'Ajustes y respaldo'
};

function showView(name) {
  views.forEach(v => {
    document.getElementById('view-' + v)?.classList.toggle('hidden', v !== name);
  });
  document.querySelectorAll('.nav-btn').forEach(b => {
    b.classList.toggle('active', b.dataset.target === name);
  });
  document.getElementById('view-subtitle').textContent = subtitles[name] || '';
  document.getElementById('fab-pago').classList.toggle('hidden', name === 'ajustes');
  renderView(name);
}

document.querySelectorAll('.nav-btn').forEach(btn => {
  btn.addEventListener('click', () => showView(btn.dataset.target));
});
document.getElementById('btn-settings').addEventListener('click', () => showView('ajustes'));

function renderView(name) {
  if (name === 'dashboard') renderDashboard();
  if (name === 'clientes') renderClientes();
  if (name === 'proyectos') renderProyectos();
  if (name === 'pagos') renderPagos();
}

/* ============================================================
   DASHBOARD
   ============================================================ */
function renderDashboard() {
  const totalPactado = CACHE.proyectos.reduce((s, p) => s + Number(p.valorTotal || 0), 0);
  const totalRecibido = CACHE.pagos.reduce((s, p) => s + Number(p.valor || 0), 0);
  const saldoPendiente = totalPactado - totalRecibido;

  document.getElementById('stat-recibido').textContent = money(totalRecibido);
  document.getElementById('stat-pendiente').textContent = money(saldoPendiente);
  document.getElementById('stat-pagos').textContent = CACHE.pagos.length;
  document.getElementById('stat-clientes').textContent = CACHE.clientes.length;

  const recent = CACHE.pagos.slice(0, 6);
  const list = document.getElementById('dashboard-recent');
  const empty = document.getElementById('dashboard-empty');
  list.innerHTML = '';
  empty.classList.toggle('hidden', recent.length > 0);

  recent.forEach(p => list.appendChild(pagoCardEl(p)));
}

/* ============================================================
   CLIENTES
   ============================================================ */
function renderClientes() {
  const list = document.getElementById('clientes-list');
  const empty = document.getElementById('clientes-empty');
  list.innerHTML = '';
  empty.classList.toggle('hidden', CACHE.clientes.length > 0);

  CACHE.clientes.forEach(c => {
    const nProyectos = proyectosDeCliente(c.id).length;
    const el = document.createElement('div');
    el.className = 'item-card';
    el.innerHTML = `
      <div class="item-top">
        <div>
          <div class="item-title">${escapeHtml(c.nombre)}</div>
          <div class="item-sub">${nProyectos} proyecto${nProyectos === 1 ? '' : 's'}${c.telefono ? ' · ' + escapeHtml(c.telefono) : ''}</div>
        </div>
      </div>
    `;
    el.addEventListener('click', () => openClienteModal(c.id));
    list.appendChild(el);
  });
}

document.getElementById('btn-add-cliente').addEventListener('click', () => openClienteModal(null));

function openClienteModal(id) {
  const editing = id != null;
  const c = editing ? CACHE.clientes.find(x => x.id === id) : { nombre: '', telefono: '', correo: '' };

  openModal(`
    <button class="modal-close" data-close>✕</button>
    <h3 class="modal-title">${editing ? 'Editar cliente' : 'Nuevo cliente'}</h3>
    <div class="field">
      <label>Nombre</label>
      <input type="text" id="f-nombre" value="${escapeAttr(c.nombre)}" placeholder="Nombre del cliente" />
    </div>
    <div class="field">
      <label>Teléfono</label>
      <input type="tel" id="f-telefono" value="${escapeAttr(c.telefono || '')}" placeholder="Opcional" />
    </div>
    <div class="field">
      <label>Correo</label>
      <input type="email" id="f-correo" value="${escapeAttr(c.correo || '')}" placeholder="Opcional" />
    </div>
    <div class="modal-actions">
      <button class="btn-primary" id="f-save">Guardar</button>
    </div>
    ${editing ? '<button class="link-danger" id="f-delete">Eliminar cliente</button>' : ''}
  `);

  document.getElementById('f-save').addEventListener('click', async () => {
    const nombre = document.getElementById('f-nombre').value.trim();
    if (!nombre) { toast('Escribe un nombre'); return; }
    const obj = {
      nombre,
      telefono: document.getElementById('f-telefono').value.trim(),
      correo: document.getElementById('f-correo').value.trim()
    };
    if (editing) obj.id = id;
    await idbPut('clientes', obj);
    await refreshCache();
    closeModal();
    renderClientes();
    populateFiltroCliente();
    toast(editing ? 'Cliente actualizado' : 'Cliente agregado');
  });

  if (editing) {
    document.getElementById('f-delete').addEventListener('click', async () => {
      const nProy = proyectosDeCliente(id).length;
      const msg = nProy > 0
        ? `Este cliente tiene ${nProy} proyecto(s) asociados. Al eliminarlo también se borrarán sus proyectos y pagos. ¿Continuar?`
        : '¿Eliminar este cliente?';
      if (!confirm(msg)) return;
      const proys = proyectosDeCliente(id);
      for (const p of proys) {
        for (const pg of pagosDeProyecto(p.id)) await idbDelete('pagos', pg.id);
        await idbDelete('proyectos', p.id);
      }
      await idbDelete('clientes', id);
      await refreshCache();
      closeModal();
      renderClientes();
      populateFiltroCliente();
      toast('Cliente eliminado');
    });
  }
}

/* ============================================================
   PROYECTOS
   ============================================================ */
function renderProyectos() {
  const list = document.getElementById('proyectos-list');
  const empty = document.getElementById('proyectos-empty');
  list.innerHTML = '';
  empty.classList.toggle('hidden', CACHE.proyectos.length > 0);

  const ordered = [...CACHE.proyectos].sort((a, b) => clienteNombre(a.clienteId).localeCompare(clienteNombre(b.clienteId)));

  ordered.forEach(p => {
    const recibido = totalRecibidoProyecto(p.id);
    const total = Number(p.valorTotal || 0);
    const pct = total > 0 ? Math.min(100, Math.round((recibido / total) * 100)) : 0;
    const completo = total > 0 && recibido >= total;

    const el = document.createElement('div');
    el.className = 'item-card';
    el.innerHTML = `
      <div class="item-top">
        <div>
          <div class="item-title">${escapeHtml(p.nombre)}</div>
          <div class="item-sub">${escapeHtml(clienteNombre(p.clienteId))}</div>
        </div>
        <span class="badge ${completo ? 'badge-positive' : 'badge-pending'}">${completo ? 'Al día' : 'Pendiente'}</span>
      </div>
      ${total > 0 ? `
      <div class="item-sub" style="margin-top:6px;">${money(recibido)} de ${money(total)}</div>
      <div class="progress-track"><div class="progress-fill" style="width:${pct}%"></div></div>
      ` : `<div class="item-sub" style="margin-top:6px;">${money(recibido)} recibido</div>`}
    `;
    el.addEventListener('click', () => openProyectoModal(p.id));
    list.appendChild(el);
  });
}

document.getElementById('btn-add-proyecto').addEventListener('click', () => {
  if (CACHE.clientes.length === 0) {
    toast('Primero agrega un cliente');
    showView('clientes');
    return;
  }
  openProyectoModal(null);
});

function openProyectoModal(id) {
  const editing = id != null;
  const p = editing ? CACHE.proyectos.find(x => x.id === id) : { clienteId: CACHE.clientes[0]?.id, nombre: '', valorTotal: '', pagosPactados: '' };

  const opcionesClientes = CACHE.clientes.map(c =>
    `<option value="${c.id}" ${c.id === p.clienteId ? 'selected' : ''}>${escapeHtml(c.nombre)}</option>`
  ).join('');

  openModal(`
    <button class="modal-close" data-close>✕</button>
    <h3 class="modal-title">${editing ? 'Editar proyecto' : 'Nuevo proyecto o servicio'}</h3>
    <div class="field">
      <label>Cliente</label>
      <select id="f-cliente">${opcionesClientes}</select>
    </div>
    <div class="field">
      <label>Nombre del proyecto o servicio</label>
      <input type="text" id="f-nombre" value="${escapeAttr(p.nombre)}" placeholder="Ej. Diseño web, Consultoría..." />
    </div>
    <div class="field">
      <label>Valor total acordado</label>
      <input type="number" inputmode="numeric" id="f-valor" value="${p.valorTotal || ''}" placeholder="0" />
      <p class="field-hint">Déjalo vacío si aún no tienes un valor fijo.</p>
    </div>
    <div class="field">
      <label>Número de pagos pactados</label>
      <input type="number" inputmode="numeric" id="f-pactados" value="${p.pagosPactados || ''}" placeholder="Opcional" />
    </div>
    <div class="modal-actions">
      <button class="btn-primary" id="f-save">Guardar</button>
    </div>
    ${editing ? '<button class="link-danger" id="f-delete">Eliminar proyecto</button>' : ''}
  `);

  document.getElementById('f-save').addEventListener('click', async () => {
    const nombre = document.getElementById('f-nombre').value.trim();
    if (!nombre) { toast('Escribe un nombre para el proyecto'); return; }
    const obj = {
      clienteId: Number(document.getElementById('f-cliente').value),
      nombre,
      valorTotal: document.getElementById('f-valor').value ? Number(document.getElementById('f-valor').value) : null,
      pagosPactados: document.getElementById('f-pactados').value ? Number(document.getElementById('f-pactados').value) : null
    };
    if (editing) obj.id = id;
    await idbPut('proyectos', obj);
    await refreshCache();
    closeModal();
    renderProyectos();
    toast(editing ? 'Proyecto actualizado' : 'Proyecto agregado');
  });

  if (editing) {
    document.getElementById('f-delete').addEventListener('click', async () => {
      const nPagos = pagosDeProyecto(id).length;
      const msg = nPagos > 0
        ? `Este proyecto tiene ${nPagos} pago(s) registrados que también se eliminarán. ¿Continuar?`
        : '¿Eliminar este proyecto?';
      if (!confirm(msg)) return;
      for (const pg of pagosDeProyecto(id)) await idbDelete('pagos', pg.id);
      await idbDelete('proyectos', id);
      await refreshCache();
      closeModal();
      renderProyectos();
      toast('Proyecto eliminado');
    });
  }
}

/* ============================================================
   PAGOS
   ============================================================ */
function populateFiltroCliente() {
  const sel = document.getElementById('filtro-cliente');
  const current = sel.value;
  sel.innerHTML = '<option value="">Todos los clientes</option>' +
    CACHE.clientes.map(c => `<option value="${c.id}">${escapeHtml(c.nombre)}</option>`).join('');
  sel.value = current;
}
document.getElementById('filtro-cliente').addEventListener('change', renderPagos);

function pagoCardEl(pg) {
  const proy = proyectoById(pg.proyectoId);
  const el = document.createElement('div');
  el.className = 'pago-card';
  el.innerHTML = `
    <div class="pago-left">
      <div class="pago-cliente">${escapeHtml(proy ? clienteNombre(proy.clienteId) : 'Cliente eliminado')}</div>
      <div class="pago-proyecto">${escapeHtml(proy ? proy.nombre : 'Proyecto eliminado')}</div>
      <div class="pago-fecha">${dateStr(pg.fecha)}</div>
    </div>
    <div class="pago-right">
      <div class="pago-valor">${money(pg.valor)}</div>
    </div>
  `;
  el.addEventListener('click', () => openPagoModal(pg.id));
  return el;
}

function renderPagos() {
  populateFiltroCliente();
  const clienteFiltro = document.getElementById('filtro-cliente').value;
  let pagos = CACHE.pagos;
  if (clienteFiltro) {
    const proyIds = new Set(proyectosDeCliente(Number(clienteFiltro)).map(p => p.id));
    pagos = pagos.filter(pg => proyIds.has(pg.proyectoId));
  }
  const list = document.getElementById('pagos-list');
  const empty = document.getElementById('pagos-empty');
  list.innerHTML = '';
  empty.classList.toggle('hidden', pagos.length > 0);
  pagos.forEach(pg => list.appendChild(pagoCardEl(pg)));
}

document.getElementById('btn-add-pago').addEventListener('click', () => openRegistrarPago());
document.getElementById('fab-pago').addEventListener('click', () => openRegistrarPago());

function openRegistrarPago() {
  if (CACHE.clientes.length === 0) {
    toast('Primero agrega un cliente');
    showView('clientes');
    return;
  }
  if (CACHE.proyectos.length === 0) {
    toast('Primero agrega un proyecto');
    showView('proyectos');
    return;
  }
  openPagoModal(null);
}

function proyectoOptionsHtml(clienteId, selectedId) {
  const proys = proyectosDeCliente(clienteId);
  if (proys.length === 0) return '<option value="">Sin proyectos para este cliente</option>';
  return proys.map(p => `<option value="${p.id}" ${p.id === selectedId ? 'selected' : ''}>${escapeHtml(p.nombre)}</option>`).join('');
}

function openPagoModal(id) {
  const editing = id != null;
  const pg = editing ? CACHE.pagos.find(x => x.id === id) : null;
  const proyActual = editing ? proyectoById(pg.proyectoId) : null;
  const clienteInicial = editing ? proyActual?.clienteId : CACHE.clientes[0]?.id;

  const opcionesClientes = CACHE.clientes.map(c =>
    `<option value="${c.id}" ${c.id === clienteInicial ? 'selected' : ''}>${escapeHtml(c.nombre)}</option>`
  ).join('');

  openModal(`
    <button class="modal-close" data-close>✕</button>
    <h3 class="modal-title">${editing ? 'Editar pago' : 'Registrar pago'}</h3>
    <div class="field">
      <label>Cliente</label>
      <select id="f-cliente">${opcionesClientes}</select>
    </div>
    <div class="field">
      <label>Proyecto o servicio</label>
      <select id="f-proyecto">${proyectoOptionsHtml(clienteInicial, editing ? pg.proyectoId : undefined)}</select>
    </div>
    <div class="field">
      <label>Valor recibido</label>
      <input type="number" inputmode="numeric" id="f-valor" value="${editing ? pg.valor : ''}" placeholder="0" />
    </div>
    <div class="field">
      <label>Fecha</label>
      <input type="date" id="f-fecha" value="${editing ? pg.fecha : todayISO()}" />
    </div>
    <div class="field">
      <label>Nota</label>
      <input type="text" id="f-nota" value="${escapeAttr(editing ? (pg.nota || '') : '')}" placeholder="Opcional" />
    </div>
    <div class="modal-actions">
      <button class="btn-primary" id="f-save">Guardar</button>
    </div>
    ${editing ? '<button class="link-danger" id="f-delete">Eliminar pago</button>' : ''}
  `);

  document.getElementById('f-cliente').addEventListener('change', (e) => {
    const cid = Number(e.target.value);
    document.getElementById('f-proyecto').innerHTML = proyectoOptionsHtml(cid);
  });

  document.getElementById('f-save').addEventListener('click', async () => {
    const proyectoId = Number(document.getElementById('f-proyecto').value);
    const valor = Number(document.getElementById('f-valor').value);
    const fecha = document.getElementById('f-fecha').value;
    if (!proyectoId) { toast('Selecciona un proyecto'); return; }
    if (!valor || valor <= 0) { toast('Escribe un valor válido'); return; }
    if (!fecha) { toast('Selecciona una fecha'); return; }
    const obj = {
      proyectoId,
      valor,
      fecha,
      nota: document.getElementById('f-nota').value.trim()
    };
    if (editing) obj.id = id;
    await idbPut('pagos', obj);
    await refreshCache();
    closeModal();
    renderView(currentView());
    toast(editing ? 'Pago actualizado' : 'Pago registrado');
  });

  if (editing) {
    document.getElementById('f-delete').addEventListener('click', async () => {
      if (!confirm('¿Eliminar este pago?')) return;
      await idbDelete('pagos', id);
      await refreshCache();
      closeModal();
      renderView(currentView());
      toast('Pago eliminado');
    });
  }
}

function currentView() {
  return views.find(v => !document.getElementById('view-' + v).classList.contains('hidden')) || 'dashboard';
}

/* ============================================================
   AJUSTES — exportar / importar / borrar
   ============================================================ */
document.getElementById('btn-export').addEventListener('click', async () => {
  const data = {
    version: 1,
    exportedAt: new Date().toISOString(),
    clientes: CACHE.clientes,
    proyectos: CACHE.proyectos,
    pagos: CACHE.pagos
  };
  const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  const fecha = todayISO();
  a.href = url;
  a.download = `cuentas-claras-backup-${fecha}.json`;
  document.body.appendChild(a);
  a.click();
  a.remove();
  URL.revokeObjectURL(url);
  toast('Copia de seguridad exportada');
});

document.getElementById('btn-import').addEventListener('click', () => {
  document.getElementById('input-import').click();
});
document.getElementById('input-import').addEventListener('change', async (e) => {
  const file = e.target.files[0];
  if (!file) return;
  try {
    const text = await file.text();
    const data = JSON.parse(text);
    if (!data.clientes || !data.proyectos || !data.pagos) throw new Error('Formato inválido');
    if (!confirm('Importar reemplazará todos los datos actuales en este equipo. ¿Continuar?')) return;
    await idbClear('clientes');
    await idbClear('proyectos');
    await idbClear('pagos');
    for (const c of data.clientes) await idbPut('clientes', c);
    for (const p of data.proyectos) await idbPut('proyectos', p);
    for (const pg of data.pagos) await idbPut('pagos', pg);
    await refreshCache();
    populateFiltroCliente();
    renderView(currentView());
    toast('Datos importados correctamente');
  } catch (err) {
    toast('No se pudo leer el archivo');
  } finally {
    e.target.value = '';
  }
});

document.getElementById('btn-clear').addEventListener('click', async () => {
  if (!confirm('Esto borrará TODOS tus clientes, proyectos y pagos guardados en este equipo. ¿Estás seguro?')) return;
  if (!confirm('Esta acción no se puede deshacer. ¿Confirmas que deseas borrar todo?')) return;
  await idbClear('clientes');
  await idbClear('proyectos');
  await idbClear('pagos');
  await refreshCache();
  populateFiltroCliente();
  renderView(currentView());
  toast('Todos los datos fueron eliminados');
});

/* ============================================================
   MODAL genérico
   ============================================================ */
function openModal(html) {
  document.getElementById('modal-sheet').innerHTML = html;
  const overlay = document.getElementById('modal-root');
  overlay.classList.remove('hidden');
  overlay.querySelectorAll('[data-close]').forEach(b => b.addEventListener('click', closeModal));
}
function closeModal() {
  document.getElementById('modal-root').classList.add('hidden');
}
document.getElementById('modal-root').addEventListener('click', (e) => {
  if (e.target === e.currentTarget) closeModal();
});

/* ---------------- Seguridad básica de escape ---------------- */
function escapeHtml(str) {
  return String(str ?? '').replace(/[&<>"']/g, (c) => ({
    '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;'
  }[c]));
}
function escapeAttr(str) { return escapeHtml(str); }

/* ============================================================
   INICIO
   ============================================================ */
(async function init() {
  await openDB();
  await refreshCache();
  populateFiltroCliente();
  showView('dashboard');

  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('sw.js').catch(() => {});
  }
})();
