    .badge.archived{background:linear-gradient(90deg,#0b3b73,#133b4f);opacity:0.8}

    .stats{display:flex;flex-direction:column;gap:8px;margin-top:12px}
    .stat{display:flex;justify-content:space-between;font-size:13px;padding:8px;border-radius:8px;background:var(--glass)}

    /* right column */
    .content{display:flex;flex-direction:column;gap:12px}
    section.card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.12));padding:12px;border-radius:12px;border:1px solid rgba(255,255,255,0.03)}
    .card h2{margin:0;font-size:15px}
    .card .sub{color:var(--muted);font-size:13px;margin-top:6px}

    table{width:100%;border-collapse:collapse;margin-top:12px}
    thead th{font-size:12px;text-align:left;padding:10px 6px;color:var(--muted);border-bottom:1px dashed rgba(255,255,255,0.03)}
    tbody tr{transition:background 0.15s;border-radius:8px}
    tbody tr:hover{background:rgba(255,255,255,0.02)}
    td{padding:12px 6px;vertical-align:middle}
    .name{font-weight:700}
    .meta-tags{font-size:12px;color:var(--muted);display:flex;gap:6px;flex-wrap:wrap}

    .progress{height:8px;background:rgba(255,255,255,0.04);border-radius:6px;overflow:hidden;width:140px}
    .progress > i{display:block;height:100%;background:linear-gradient(90deg,var(--accent),var(--accent-2));border-radius:6px}

    .actions{display:flex;gap:8px;align-items:center}
    .tiny{padding:6px;border-radius:8px;font-size:13px;background:rgba(255,255,255,0.02);border:1px solid rgba(255,255,255,0.03);cursor:pointer}

    .note{font-size:13px;color:var(--muted)}

    /* modal */
    .modal-backdrop{position:fixed;inset:0;background:linear-gradient(180deg,rgba(1,5,13,0.6),rgba(1,5,13,0.8));display:none;align-items:center;justify-content:center;padding:24px;z-index:60}
    .modal-backdrop.show{display:flex}
    .modal{width:100%;max-width:720px;background:linear-gradient(180deg,#021428,#041828);padding:16px;border-radius:12px;border:1px solid rgba(255,255,255,0.03)}
    .modal .row{display:flex;gap:8px}
    .modal label{font-size:13px;color:var(--muted)}
    .form-control{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:var(--text)}

    .history-list{max-height:220px;overflow:auto;padding:8px;border-radius:8px;background:rgba(255,255,255,0.02);}
    .history-item{padding:8px;border-radius:8px;border-bottom:1px dashed rgba(255,255,255,0.03)}

    footer{display:flex;justify-content:space-between;align-items:center;margin-top:14px;color:var(--muted);font-size:13px}

    /* responsive */
    @media (max-width:900px){
      .grid{grid-template-columns:1fr;}
      .title{flex-direction:column;align-items:flex-start}
    }
  </style>
</head>
<body>
  <div class="app" role="application" aria-label="Public Development Diary">
    <header>
      <div class="title">
        <div class="logo">DD</div>
        <div>
          <h1>Public Dev Diary</h1>
          <p class="lead">Live progress & release tracker — public view. Blue-themed, editable with passkey.</p>
        </div>
      </div>

      <div class="controls">
        <div class="indicator"><span class="dot" aria-hidden></span><span id="modeLabel">Read-only</span></div>
        <button class="btn small" id="unlockBtn">Unlock (passkey)</button>
        <button class="btn small ghost" id="exportBtn">Export JSON</button>
        <button class="btn small ghost" id="importBtn">Import JSON</button>
        <input id="fileInput" type="file" accept="application/json" style="display:none">
      </div>
    </header>

    <div class="grid">
      <aside class="panel">
        <div class="search">
          <input id="searchInput" placeholder="Search projects, tags, notes..." />
          <button id="addNewBtn" class="btn small primary" title="Add project (requires unlock)">+ New</button>
        </div>

        <div class="legend">
          <div style="display:flex;justify-content:space-between;align-items:center"><strong>Legend</strong><span class="note">What each status means</span></div>
          <div class="row" style="margin-top:8px"><div class="badge released">Released</div><div class="note">Product complete and available (may still receive updates).</div></div>
          <div class="row"><div class="badge active">Developed • Active</div><div class="note">Released but actively maintained / new features</div></div>
          <div class="row"><div class="badge inprogress">In development</div><div class="note">Feature is under construction — not released yet.</div></div>
          <div class="row"><div class="badge archived">Archived</div><div class="note">No longer maintained.</div></div>
        </div>

        <div class="stats">
          <div class="stat"><div>Projects</div><div id="statTotal">0</div></div>
          <div class="stat"><div>Developed / Released</div><div id="statDev">0</div></div>
          <div class="stat"><div>In Development</div><div id="statIn">0</div></div>
          <div class="stat"><div>Archived</div><div id="statArc">0</div></div>
        </div>

        <div style="margin-top:14px;display:flex;gap:8px;flex-direction:column">
          <button id="undoBtn" class="btn small ghost">Undo last change</button>
          <button id="resetBtn" class="btn small ghost">Reset defaults</button>
        </div>
      </aside>

      <main class="content">
        <section class="card" aria-labelledby="developedHeading">
          <h2 id="developedHeading">Developed / Released</h2>
          <div class="sub">Stable projects and products (includes items under active maintenance)</div>
          <table id="developedTable" aria-describedby="developedHeading">
            <thead>
              <tr><th>Project</th><th>Tags & Notes</th><th>Progress</th><th>Version</th><th style="width:160px">Actions</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </section>

        <section class="card" aria-labelledby="devHeading">
          <h2 id="devHeading">In Development</h2>
          <div class="sub">Currently being worked on — expect frequent updates and changes</div>
          <table id="devTable">
            <thead>
              <tr><th>Project</th><th>Tags & Notes</th><th>Progress</th><th>Version</th><th style="width:160px">Actions</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </section>

        <footer>
          <div>Tip: Click "Unlock" and enter the passkey to edit. This is client-side only and uses localStorage.</div>
          <div>Made with ♥ — editable diary</div>
        </footer>
      </main>
    </div>

    <!-- modal: edit/create -->
    <div class="modal-backdrop" id="modalBackdrop" role="dialog" aria-modal="true">
      <div class="modal" role="document">
        <h3 id="modalTitle">Edit project</h3>
        <div style="display:flex;gap:12px;margin-top:12px;flex-wrap:wrap">
          <div style="flex:1;min-width:220px">
            <label class="note">Name</label>
            <input id="f_name" class="form-control" />
          </div>
          <div style="width:170px">
            <label class="note">Status</label>
            <select id="f_status" class="form-control">
              <option value="released">Released</option>
              <option value="developed-active">Developed • Active</option>
              <option value="in-development">In Development</option>
              <option value="archived">Archived</option>
            </select>
          </div>
          <div style="width:140px">
            <label class="note">Version</label>
            <input id="f_version" class="form-control" placeholder="v1.0.0" />
          </div>
          <div style="width:120px">
            <label class="note">Progress %</label>
            <input id="f_progress" class="form-control" type="number" min="0" max="100" />
          </div>
        </div>

        <div style="margin-top:12px">
          <label class="note">Description / Notes</label>
          <textarea id="f_notes" rows="4" class="form-control"></textarea>
        </div>

        <div style="margin-top:12px;display:flex;gap:8px;justify-content:flex-end">
          <button class="btn ghost" id="modalCancel">Cancel</button>
          <button class="btn primary" id="modalSave">Save</button>
        </div>

        <hr style="border:none;border-top:1px dashed rgba(255,255,255,0.03);margin:12px 0">

        <div>
          <strong>History</strong>
          <div class="history-list" id="historyList">
            <!-- history items injected -->
          </div>
        </div>
      </div>
    </div>
  </div>

<script>
// --- Data & storage keys ---
const PASSKEY = 'tempebreault'; // as requested by you; client-side check only
const LS_KEY = 'devDiary.projects.v1';
const LS_UNLCK = 'devDiary.unlocked.v1';
const LS_UNDO = 'devDiary.undo.v1';

// Initial seed based on your list
const DEFAULT_PROJECTS = [
  {
    id: 'expiry-tracker',
    name: 'Expiry Tracker',
    status: 'developed-active',
    progress: 100,
    version: '1.2.0',
    notes: 'Developed and active — ongoing improvements and bug fixes.',
    tags: ['utility','tracking'],
    createdAt: Date.now() - 1000*60*60*24*60,
    history: []
  },
  { id:'word-search', name:'Word Search', status:'released', progress:100, version:'1.0.0', notes:'Product complete.', tags:['puzzle'], createdAt: Date.now()-1000*60*60*24*200, history:[] },
  { id:'plumbers-handbook', name:'Plumbers handbook', status:'in-development', progress:28, version:'0.3.0', notes:'Unfinished — core reference content being drafted.', tags:['reference','manual'], createdAt: Date.now()-1000*60*60*24*30, history:[] },
  { id:'keyboard-karate', name:'Keyboard Karate', status:'released', progress:100, version:'1.0.1', notes:'Product complete.', tags:['game','typing'], createdAt: Date.now()-1000*60*60*24*400, history:[] },
  { id:'emoji-game', name:'Emoji Game', status:'released', progress:100, version:'1.0.0', notes:'Product complete.', tags:['casual','game'], createdAt: Date.now()-1000*60*60*24*120, history:[] },
  { id:'reflex-reactor', name:'Reflex Reactor', status:'released', progress:100, version:'1.1.0', notes:'Product complete.', tags:['game','reflex'], createdAt: Date.now()-1000*60*60*24*90, history:[] },
  { id:'word-typing-simulator', name:'Word Typing Simulator', status:'released', progress:100, version:'1.0.0', notes:'Product complete.', tags:['simulator','typing'], createdAt: Date.now()-1000*60*60*24*150, history:[] },
  { id:'destiny-orb', name:'Destiny Orb', status:'in-development', progress:12, version:'0.1.0', notes:'**NEW** In development — Magic 8 Ball inspired. Demo phase.', tags:['new','demo','game'], createdAt: Date.now()-1000*60*60*24*2, history:[] }
];

let projects = loadProjects();
let unlocked = sessionStorage.getItem(LS_UNLCK) === '1';
let undoStack = loadJSON(LS_UNDO) || [];

// --- DOM refs ---
const developedBody = document.querySelector('#developedTable tbody');
const devBody = document.querySelector('#devTable tbody');
const statTotal = document.getElementById('statTotal');
const statDev = document.getElementById('statDev');
const statIn = document.getElementById('statIn');
const statArc = document.getElementById('statArc');
const unlockBtn = document.getElementById('unlockBtn');
const modeLabel = document.getElementById('modeLabel');
const addNewBtn = document.getElementById('addNewBtn');
const searchInput = document.getElementById('searchInput');
const modalBackdrop = document.getElementById('modalBackdrop');
const modalTitle = document.getElementById('modalTitle');
const f_name = document.getElementById('f_name');
const f_status = document.getElementById('f_status');
const f_version = document.getElementById('f_version');
const f_progress = document.getElementById('f_progress');
const f_notes = document.getElementById('f_notes');
const modalSave = document.getElementById('modalSave');
const modalCancel = document.getElementById('modalCancel');
const historyList = document.getElementById('historyList');
const exportBtn = document.getElementById('exportBtn');
const importBtn = document.getElementById('importBtn');
const fileInput = document.getElementById('fileInput');
const undoBtn = document.getElementById('undoBtn');
const resetBtn = document.getElementById('resetBtn');

let editingId = null; // null = creating

// --- helpers ---
function saveUndoSnapshot(){
  // keep shallow copy of projects array for undo
  undoStack.push(JSON.parse(JSON.stringify(projects)));
  if(undoStack.length>30) undoStack.shift();
  saveJSON(LS_UNDO, undoStack);
}

function loadJSON(key){try{return JSON.parse(localStorage.getItem(key))}catch(e){return null}}
function saveJSON(key,val){localStorage.setItem(key,JSON.stringify(val))}

function loadProjects(){
  const raw = loadJSON(LS_KEY);
  if(!raw){ localStorage.setItem(LS_KEY, JSON.stringify(DEFAULT_PROJECTS)); return JSON.parse(JSON.stringify(DEFAULT_PROJECTS)); }
  return raw;
}

function persist(){
  saveJSON(LS_KEY, projects);
  render();
}

function uid(){return 'p_'+Math.random().toString(36).slice(2,9)}
function slugify(s){return s.toLowerCase().replace(/[^a-z0-9]+/g,'-').replace(/(^-|-$)/g,'')}

// --- rendering ---
function render(){
  // stats
  const total = projects.length;
  const devCount = projects.filter(p=>p.status==='released' || p.status==='developed-active').length;
  const inCount = projects.filter(p=>p.status==='in-development').length;
  const arcCount = projects.filter(p=>p.status==='archived').length;
  statTotal.textContent = total; statDev.textContent = devCount; statIn.textContent = inCount; statArc.textContent = arcCount;
  modeLabel.textContent = unlocked? 'Edit mode' : 'Read-only';
  unlockBtn.textContent = unlocked? 'Locked (logout)' : 'Unlock (passkey)';

  // filter
  const q = (searchInput.value||'').trim().toLowerCase();
  const matches = p=>{
    if(!q) return true;
    return [p.name,(p.notes||''), (p.tags||[]).join(' ')].join(' ').toLowerCase().includes(q);
  }

  // clear tables
  developedBody.innerHTML=''; devBody.innerHTML='';

  // sort: developed first by createdAt desc
  const developed = projects.filter(p=>p.status==='released' || p.status==='developed-active' || p.status==='archived').filter(matches).sort((a,b)=>b.createdAt - a.createdAt);
  const indev = projects.filter(p=>p.status==='in-development').filter(matches).sort((a,b)=>b.progress - a.progress);

  developed.forEach(p=> developedBody.appendChild(rowFor(p)));
  indev.forEach(p=> devBody.appendChild(rowFor(p)));
}

function rowFor(p){
  const tr = document.createElement('tr');
  // name cell
  const tdName = document.createElement('td');
  tdName.innerHTML = `<div class=\"name\">${escapeHtml(p.name)} ${p.tags && p.tags.includes('new')? '<span style=\"font-size:11px;margin-left:8px;padding:4px 6px;border-radius:6px;background:linear-gradient(90deg,var(--accent-2),var(--accent));font-weight:800;color:#001\">NEW</span>':''}</div><div class=\"note\">${escapeHtml(p.notes||'')}</div>`;
  tr.appendChild(tdName);

  // tags
  const tdTags = document.createElement('td');
  tdTags.innerHTML = `<div class=\"meta-tags\">${(p.tags||[]).map(t=>`<span style=\"padding:4px 8px;border-radius:999px;background:rgba(255,255,255,0.02);font-size:12px;font-weight:700\">${escapeHtml(t)}</span>`).join('')}</div>`;
  tr.appendChild(tdTags);

  // progress
  const tdProg = document.createElement('td');
  tdProg.innerHTML = `<div class=\"progress\"><i style=\"width:${Math.max(0,Math.min(100,p.progress||0))}%\"></i></div><div style=\"font-size:12px;color:var(--muted);margin-top:6px\">${p.progress||0}%</div>`;
  tr.appendChild(tdProg);

  // version
  const tdVer = document.createElement('td');
  tdVer.innerHTML = `<div style=\"font-weight:700\">${escapeHtml(p.version||'—')}</div><div class=\"note\">${new Date(p.createdAt).toLocaleDateString()}</div>`;
  tr.appendChild(tdVer);

  // actions
  const tdAct = document.createElement('td');
  tdAct.className='actions';

  // view history button
  const btnHist = document.createElement('button'); btnHist.className='tiny'; btnHist.title='View history'; btnHist.textContent='History';
  btnHist.addEventListener('click',()=>openModal(p.id,true));
  tdAct.appendChild(btnHist);

  // edit
  const btnEdit = document.createElement('button'); btnEdit.textContent='Edit'; btnEdit.className='tiny';
  btnEdit.disabled = !unlocked;
  btnEdit.title = unlocked? 'Edit project' : 'Unlock to edit';
  btnEdit.addEventListener('click',()=>openModal(p.id,false));
  tdAct.appendChild(btnEdit);

  // quick status change dropdown (only when unlocked)
  const sel = document.createElement('select'); sel.className='tiny'; sel.style.padding='6px'; sel.disabled = !unlocked;
  ['released','developed-active','in-development','archived'].forEach(s=>{
    const o = document.createElement('option'); o.value=s; o.textContent = statusLabel(s); if(p.status===s) o.selected=true; sel.appendChild(o);
  });
  sel.addEventListener('change',()=>{
    saveUndoSnapshot();
    const old = {...p}; p.status = sel.value; if(p.status==='released') p.progress = 100; persist();
    p.history = p.history || []; p.history.push({when:Date.now(), who:'editor', action:`status->${p.status}`, snapshot:old});
  });
  tdAct.appendChild(sel);

  // reorder
  const up = document.createElement('button'); up.className='tiny'; up.title='Move up'; up.textContent='↑'; up.disabled = !unlocked; up.addEventListener('click',()=>moveProject(p.id,-1)); tdAct.appendChild(up);
  const down = document.createElement('button'); down.className='tiny'; down.title='Move down'; down.textContent='↓'; down.disabled = !unlocked; down.addEventListener('click',()=>moveProject(p.id,1)); tdAct.appendChild(down);

  // delete
  const del = document.createElement('button'); del.className='tiny'; del.style.borderColor='rgba(255,100,100,0.12)'; del.textContent='Delete'; del.disabled = !unlocked;
  del.addEventListener('click',()=>{
    if(!confirm('Delete this project? This cannot be undone except via Undo (if available).')) return;
    saveUndoSnapshot(); projects = projects.filter(x=>x.id!==p.id); persist();
  });
  tdAct.appendChild(del);

  tr.appendChild(tdAct);
  return tr;
}

function statusLabel(s){
  switch(s){
    case 'released': return 'Released';
    case 'developed-active': return 'Developed - Active';
    case 'in-development': return 'In development';
    case 'archived': return 'Archived';
    default: return s;
  }
}

function escapeHtml(s){ if(!s) return ''; return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

// --- modal / editor ---
function openModal(id=null, viewHistoryOnly=false){
  editingId = id;
  if(id){
    const p = projects.find(x=>x.id===id);
    if(!p) return;
    modalTitle.textContent = (viewHistoryOnly? 'History for ' : 'Edit ') + p.name;
    f_name.value = p.name; f_status.value = p.status; f_version.value = p.version; f_progress.value = p.progress||0; f_notes.value = p.notes||'';
    renderHistory(p.history);
  } else {
    modalTitle.textContent = 'Create new project'; f_name.value=''; f_status.value='in-development'; f_version.value='0.1.0'; f_progress.value=10; f_notes.value=''; historyList.innerHTML='';
  }
  // when viewing history only, disable save
  modalSave.disabled = viewHistoryOnly || !unlocked;
  modalBackdrop.classList.add('show');
}

function closeModal(){ modalBackdrop.classList.remove('show'); editingId=null; }

modalCancel.addEventListener('click',()=>closeModal());
modalBackdrop.addEventListener('click',(e)=>{ if(e.target===modalBackdrop) closeModal(); });
modalSave.addEventListener('click', ()=>{
  const name = f_name.value.trim(); if(!name){alert('Name required'); return;}
  const status = f_status.value; const version = f_version.value.trim(); const progress = Number(f_progress.value)||0; const notes = f_notes.value.trim();
  if(editingId){
    // update
    saveUndoSnapshot();
    const p = projects.find(x=>x.id===editingId);
    if(!p) return;
    const old = JSON.parse(JSON.stringify(p));
    p.name = name; p.status = status; p.version = version; p.progress = progress; p.notes = notes; p.tags = p.tags || [];
    p.history = p.history || []; p.history.push({when:Date.now(), who:'editor', action:'updated', snapshot:old});
  } else {
    // create new
    saveUndoSnapshot();
    const id = slugify(name) || uid();
    const newP = { id, name, status, version, progress, notes, tags:[], createdAt:Date.now(), history:[{when:Date.now(),who:'editor',action:'created'}] };
    projects.unshift(newP);
  }
  persist(); closeModal();
});

function renderHistory(arr){ historyList.innerHTML=''; (arr||[]).slice().reverse().forEach(h=>{
  const el = document.createElement('div'); el.className='history-item';
  el.innerHTML = `<div style=\"font-weight:700\">${h.action || 'change'}</div><div style=\"font-size:12px;color:var(--muted)\">${new Date(h.when).toLocaleString()} — ${h.who||'—'}</div><div style=\"margin-top:6px;font-size:13px\">Snapshot: <pre style=\"white-space:pre-wrap;margin:6px 0;max-height:140px;overflow:auto;background:rgba(255,255,255,0.02);padding:8px;border-radius:8px\">${escapeHtml(JSON.stringify(h.snapshot||{},null,2))}</pre></div>`;
  historyList.appendChild(el);
}); if(!(arr||[]).length) historyList.innerHTML = '<div class="note">No history yet for this project.</div>';
}

// --- other actions ---
unlockBtn.addEventListener('click', ()=>{
  if(unlocked){ // lock
    unlocked=false; sessionStorage.removeItem(LS_UNLCK); render(); return;
  }
  const val = prompt('Enter passkey to unlock edit mode:');
  if(val===PASSKEY){ unlocked=true; sessionStorage.setItem(LS_UNLCK,'1'); alert('Unlocked — you may now edit.'); render(); }
  else alert('Incorrect passkey.');
});

addNewBtn.addEventListener('click', ()=>{
  if(!unlocked){ alert('Unlock first to create or edit projects.'); return; }
  openModal(null,false);
});

searchInput.addEventListener('input', ()=> render());

exportBtn.addEventListener('click', ()=>{
  const data = JSON.stringify({ exportedAt:Date.now(), projects }, null, 2);
  const blob = new Blob([data],{type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href=url; a.download = 'dev-diary-export.json'; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
});

importBtn.addEventListener('click', ()=>{
  fileInput.click();
});

fileInput.addEventListener('change', async (e)=>{
  const f = e.target.files[0]; if(!f) return; try{
    const txt = await f.text(); const parsed = JSON.parse(txt);
    if(!parsed.projects) throw new Error('Invalid file format');
    if(!confirm('Import will replace current projects. Continue?')) return;
    saveUndoSnapshot(); projects = parsed.projects; persist(); alert('Import complete.');
  }catch(err){ alert('Import failed: '+ err.message); }
  fileInput.value='';
});

undoBtn.addEventListener('click', ()=>{
  if(!undoStack || !undoStack.length){ alert('Nothing to undo'); return; }
  const prev = undoStack.pop(); projects = prev; saveJSON(LS_UNDO, undoStack); persist(); alert('Reverted last change');
});

resetBtn.addEventListener('click', ()=>{
  if(!confirm('Reset to default seeded projects? This will overwrite current projects.')) return;
  saveUndoSnapshot(); projects = JSON.parse(JSON.stringify(DEFAULT_PROJECTS)); persist(); alert('Reset complete.');
});

function moveProject(id, dir){
  const idx = projects.findIndex(p=>p.id===id); if(idx===-1) return; const to = idx+dir; if(to<0||to>=projects.length) return; saveUndoSnapshot(); const [itm] = projects.splice(idx,1); projects.splice(to,0,itm); persist(); }

// initial render
render();

// expose some helpers to window for debugging (optional)
window.__devDiary = {get projects(){return projects}, save:()=>persist(), unlock:()=>{sessionStorage.setItem(LS_UNLCK,'1'); unlocked=true; render()}};
</script>
</body>
</html>
