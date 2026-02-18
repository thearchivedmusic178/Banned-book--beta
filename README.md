# Banned-book--beta
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>BANNED BOOK | STUDIO</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');
        :root { --banned-red: #dc2626; --banned-radius: 2.5rem; }
        body { font-family: 'Inter', sans-serif; background: #f8fafc; -webkit-tap-highlight-color: transparent; }
        .hidden { display: none !important; }

        /* RED UI THEME */
        .banned-box { background: white; border-radius: var(--banned-radius); border: 4px solid var(--banned-red); box-shadow: 0 15px 35px rgba(220, 38, 38, 0.2); padding: 2.5rem; }
        .post-card { background: white; border-radius: 1.5rem; border: 1px solid #e2e8f0; padding: 1rem; margin-bottom: 1rem; box-shadow: 0 2px 10px rgba(0,0,0,0.02); }
        .media-shell { width: 100%; border-radius: 1.5rem; background: #000; overflow: hidden; aspect-ratio: 1/1; border: 3px solid white; box-shadow: 0 5px 15px rgba(0,0,0,0.1); position: relative; }
        .media-shell img, .media-shell video { width: 100%; height: 100%; object-fit: cover; }
        
        .btn-banned { background: var(--banned-red); color: white; font-weight: 900; padding: 1.25rem; border-radius: 1.5rem; width: 100%; text-transform: uppercase; letter-spacing: 0.5px; }
        .grid-item { aspect-ratio: 1/1; cursor: pointer; border-radius: 0.5rem; overflow: hidden; background: #111; border: 1px solid rgba(0,0,0,0.05); }
        
        /* NAV BUTTONS */
        .nav-pill { font-weight: 900; font-size: 10px; text-transform: uppercase; letter-spacing: 1px; padding: 10px 20px; border-radius: 99px; transition: 0.2s; }
        .nav-pill:active { transform: scale(0.9); opacity: 0.8; }
    </style>
</head>
<body>

    <div id="auth-layer" class="fixed inset-0 bg-slate-900 z-[100] flex items-center justify-center p-6 text-center">
        <div class="banned-box w-full max-w-sm">
            <h1 class="text-5xl font-black text-red-600 italic mb-2 tracking-tighter">BANNED</h1>
            <p class="text-[10px] font-bold text-slate-400 uppercase mb-8 tracking-widest">Identity Studio</p>
            <input id="auth-user" type="text" placeholder="Username" class="w-full p-4 bg-slate-100 rounded-2xl text-center font-bold mb-3 outline-none border-2 border-transparent focus:border-red-500">
            <input id="auth-pass" type="password" placeholder="Password" class="w-full p-4 bg-slate-100 rounded-2xl text-center font-bold mb-4 outline-none border-2 border-transparent focus:border-red-500">
            <button onclick="app.handleAuth()" class="btn-banned">Login</button>
        </div>
    </div>

    <div id="app-layer" class="hidden">
        <nav class="bg-white border-b p-5 sticky top-0 z-50 flex justify-between items-center">
            <span onclick="app.view('feed')" class="font-black text-2xl italic text-red-600 cursor-pointer tracking-tighter">BANNED BOOK</span>
            <div id="nav-buttons">
                </div>
        </nav>

        <section id="view-feed" class="max-w-xl mx-auto p-4 pb-24">
            <div class="post-card text-center"><button onclick="app.startCam()" class="btn-banned">Capture New Post</button></div>
            <div id="feed"></div>
        </section>

        <section id="view-profile" class="hidden max-w-xl mx-auto p-4 pb-24 text-center">
            <div class="py-10">
                <div class="w-32 h-32 mx-auto mb-5 border-4 border-red-500 p-1 rounded-full shadow-2xl relative">
                    <img id="prof-avatar" class="w-full h-full rounded-full object-cover bg-slate-200">
                    <input type="file" id="pfp-upload" class="hidden" accept="image/*" onchange="app.uploadPFP(this)">
                    <button onclick="document.getElementById('pfp-upload').click()" class="absolute bottom-1 right-1 bg-red-600 text-white w-8 h-8 rounded-full text-[10px] font-black shadow-lg border-2 border-white flex items-center justify-center">EDIT</button>
                </div>
                <h2 id="prof-name" class="text-3xl font-black italic tracking-tighter"></h2>
                <button onclick="app.sync()" class="mt-4 text-[10px] font-black text-red-400 uppercase tracking-widest bg-slate-100 px-4 py-2 rounded-full">↺ REFRESH ALL DATA</button>
            </div>
            
            <div id="profile-grid" class="grid grid-cols-3 gap-1.5 border-t pt-6"></div>
        </section>

        <div id="detail-modal" class="hidden fixed inset-0 bg-white z-[250] flex flex-col">
            <div class="p-4 border-b flex justify-between items-center sticky top-0 bg-white">
                <button onclick="app.closeDetail()" class="font-black text-2xl px-2">✕</button>
                <span class="font-black text-[10px] uppercase tracking-widest">Studio Detail</span>
                <div class="w-8"></div>
            </div>
            <div id="detail-content" class="p-4 max-w-xl mx-auto w-full overflow-y-auto"></div>
        </div>

        <div id="camera-box" class="hidden fixed inset-0 bg-black z-[150] p-6 flex flex-col items-center">
            <div class="media-shell mb-10 mt-10"><video id="viewfinder" autoplay playsinline muted></video></div>
            <div class="flex gap-10">
                <button onclick="app.snap()" class="w-20 h-20 bg-white rounded-full border-4 border-slate-400"></button>
                <button onclick="app.record()" id="rec-trigger" class="w-20 h-20 bg-red-600 rounded-full border-4 border-white"></button>
            </div>
            <button onclick="app.stopCam()" class="mt-auto text-white font-black opacity-50 mb-10 text-xs uppercase">Exit Camera</button>
        </div>

        <div id="preview-area" class="hidden fixed inset-0 bg-white z-[300] p-6 flex flex-col">
            <div id="preview-container" class="media-shell mb-6"></div>
            <textarea id="caption-in" placeholder="Say something..." class="w-full p-5 bg-slate-100 rounded-3xl mb-6 font-bold text-lg outline-none"></textarea>
            <button onclick="app.saveToDB()" class="btn-banned py-6 text-xl">Publish to Studio</button>
        </div>
    </div>

    <script>
        let db, stream, recorder, chunks = [], tempMedia = null;

        const app = {
            init() {
                const req = indexedDB.open("BannedBook_v15", 1);
                req.onsuccess = e => { db = e.target.result; this.checkSession(); this.render(); };
                req.onupgradeneeded = e => {
                    const d = e.target.result;
                    if(!d.objectStoreNames.contains("posts")) d.createObjectStore("posts", {keyPath: "id"});
                };
            },

            handleAuth() {
                const user = document.getElementById('auth-user').value.trim();
                if(!user) return;
                localStorage.setItem('bb_user', user); 
                this.checkSession();
            },

            checkSession() {
                const u = localStorage.getItem('bb_user');
                document.getElementById('auth-layer').classList.toggle('hidden', !!u);
                document.getElementById('app-layer').classList.toggle('hidden', !u);
                if(u) {
                    document.getElementById('prof-name').innerText = `@${u.toLowerCase()}`;
                    this.loadPFP(u);
                    this.render();
                    this.view('feed'); 
                }
            },

            loadPFP(user) {
                const img = document.getElementById('prof-avatar');
                const lowerName = user.toLowerCase();
                const custom = localStorage.getItem(`pfp_${lowerName}`) || localStorage.getItem(`pfp_${user}`);
                img.src = custom || `https://api.dicebear.com/7.x/avataaars/svg?seed=${lowerName}`;
            },

            uploadPFP(input) {
                const file = input.files[0];
                if(!file) return;
                const reader = new FileReader();
                reader.onload = e => {
                    const u = localStorage.getItem('bb_user').toLowerCase();
                    localStorage.setItem(`pfp_${u}`, e.target.result);
                    this.loadPFP(u);
                };
                reader.readAsDataURL(file);
            },

            view(type) {
                const nav = document.getElementById('nav-buttons');
                ['view-feed', 'view-profile'].forEach(v => document.getElementById(v).classList.add('hidden'));
                document.getElementById(`view-${type}`).classList.remove('hidden');
                
                // DYNAMIC NAV BUTTONS (FIXED)
                if(type === 'feed') {
                    nav.innerHTML = `<button onclick="app.view('profile')" class="nav-pill bg-slate-100 text-slate-500">My Profile</button>`;
                } else {
                    nav.innerHTML = `<button onclick="app.view('feed')" class="nav-pill bg-red-600 text-white shadow-lg shadow-red-100">Back to Feed</button>`;
                    this.renderProfile();
                }
            },

            sync() {
                this.render();
                this.renderProfile();
            },

            renderProfile() {
                if(!db) return;
                const currentUser = localStorage.getItem('bb_user').toLowerCase();
                const store = db.transaction("posts", "readonly").objectStore("posts");
                store.getAll().onsuccess = e => {
                    const allPosts = e.target.result;
                    const mine = allPosts.filter(p => p.user.toLowerCase() === currentUser).reverse();
                    const grid = document.getElementById('profile-grid');
                    grid.innerHTML = mine.map(p => `
                        <div class="grid-item" onclick="app.openDetail(${p.id})">
                            ${p.type.includes('video') ? `<video src="${p.media}" class="w-full h-full object-cover"></video>` : `<img src="${p.media}" class="w-full h-full object-cover">`}
                        </div>
                    `).join('') || `<div class="col-span-3 py-20 opacity-20 font-black italic text-xl uppercase">No Posts</div>`;
                };
            },

            openDetail(postId) {
                const store = db.transaction("posts", "readonly").objectStore("posts");
                store.get(postId).onsuccess = e => {
                    const p = e.target.result;
                    document.getElementById('detail-content').innerHTML = `
                        <div class="post-card">
                            <div class="flex items-center gap-2 mb-4 font-black text-xs uppercase">@${p.user}</div>
                            <div class="media-shell h-[450px] mb-4">
                                ${p.type.includes('video') ? `<video src="${p.media}" controls autoplay loop></video>` : `<img src="${p.media}">`}
                            </div>
                            <p class="font-bold text-lg text-slate-800 mb-6">${p.caption || ''}</p>
                            <button onclick="app.deletePost(${p.id})" class="w-full py-4 text-red-500 font-black text-[10px] uppercase border border-red-100 rounded-2xl">Delete Memory</button>
                        </div>
                    `;
                    document.getElementById('detail-modal').classList.remove('hidden');
                };
            },

            closeDetail() { document.getElementById('detail-modal').classList.add('hidden'); },

            render() {
                if(!db) return;
                const store = db.transaction("posts", "readonly").objectStore("posts");
                store.getAll().onsuccess = e => {
                    const posts = e.target.result.reverse();
                    document.getElementById('feed').innerHTML = posts.map(p => `
                        <div class="post-card">
                            <div class="font-black mb-3 uppercase text-[10px] flex items-center gap-2">
                                <div class="w-6 h-6 rounded-full bg-red-500 overflow-hidden"><img src="https://api.dicebear.com/7.x/avataaars/svg?seed=${p.user.toLowerCase()}"></div>
                                @${p.user}
                            </div>
                            <div class="media-shell" onclick="app.openDetail(${p.id})">
                                ${p.type.includes('video') ? `<video src="${p.media}" muted loop autoplay></video>` : `<img src="${p.media}">`}
                            </div>
                            <p class="mt-3 font-bold text-slate-700">${p.caption || ''}</p>
                        </div>
                    `).join('');
                };
            },

            deletePost(id) {
                if(!confirm("Are you sure?")) return;
                const tx = db.transaction("posts", "readwrite");
                tx.objectStore("posts").delete(id);
                tx.oncomplete = () => { this.closeDetail(); this.sync(); };
            },

            async startCam() {
                try {
                    stream = await navigator.mediaDevices.getUserMedia({video:true, audio:true});
                    document.getElementById('viewfinder').srcObject = stream;
                    document.getElementById('camera-box').classList.remove('hidden');
                } catch(e) { alert("Camera not found."); }
            },
            stopCam() { if(stream) stream.getTracks().forEach(t => t.stop()); document.getElementById('camera-box').classList.add('hidden'); },
            snap() {
                const v = document.getElementById('viewfinder');
                const c = document.createElement('canvas');
                c.width = v.videoWidth; c.height = v.videoHeight;
                c.getContext('2d').drawImage(v, 0, 0);
                this.prepareForSave(c.toDataURL('image/jpeg'), 'image/jpeg');
                this.stopCam();
            },
            record() {
                const btn = document.getElementById('rec-trigger');
                if(!recorder || recorder.state === 'inactive') {
                    chunks = []; recorder = new MediaRecorder(stream);
                    recorder.ondataavailable = e => chunks.push(e.data);
                    recorder.onstop = () => {
                        const blob = new Blob(chunks, {type: 'video/mp4'});
                        const reader = new FileReader();
                        reader.onload = e => this.prepareForSave(e.target.result, 'video/mp4');
                        reader.readAsDataURL(blob);
                    };
                    recorder.start();
                    btn.classList.add('bg-black');
                } else { recorder.stop(); this.stopCam(); btn.classList.remove('bg-black'); }
            },
            prepareForSave(data, type) {
                tempMedia = { data, type };
                document.getElementById('preview-container').innerHTML = type.includes('video') ? `<video src="${data}" autoplay muted loop></video>` : `<img src="${data}">`;
                document.getElementById('preview-area').classList.remove('hidden');
            },
            saveToDB() {
                const tx = db.transaction("posts", "readwrite");
                tx.objectStore("posts").add({
                    id: Date.now(),
                    user: localStorage.getItem('bb_user'),
                    media: tempMedia.data,
                    type: tempMedia.type,
                    caption: document.getElementById('caption-in').value || ""
                });
                tx.oncomplete = () => { 
                    document.getElementById('preview-area').classList.add('hidden'); 
                    document.getElementById('caption-in').value = "";
                    this.sync(); 
                    this.view('feed'); 
                };
            }
        };
        window.onload = () => app.init();
    </script>
</body>
</html>

