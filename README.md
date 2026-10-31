# Chocolat-d-amour
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Une douce attention pour Sarah ❤️</title>

<!-- Google Fonts -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@600;700&family=Playfair+Display:ital,wght@0,400;0,600;0,700;1,400&family=Plus+Jakarta+Sans:wght@300;400;500&display=swap" rel="stylesheet">

<!-- FontAwesome icons & Canvas Confetti CDN -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

<!-- Tailwind CSS pour layout et utilitaires -->
<script src="https://cdn.tailwindcss.com"></script>

<style>
    /* Variables de couleurs & Thème */
    :root {
        --color-bg-start: #2c1810;
        --color-bg-mid: #4a2511;
        --color-bg-end: #1d0c07;
        --color-cream: #FFF8F0;
        --color-rose-gold: #e8b4b8;
        --color-gold: #d4af37;
        --color-gold-glow: rgba(212, 175, 55, 0.4);
        --color-dark-choc: #2b1408;
        --color-milk-choc: #5c3219;
        --color-ruby-choc: #a82e3e;
    }

    body {
        font-family: 'Plus Jakarta Sans', sans-serif;
        background: radial-gradient(circle at center, var(--color-bg-mid) 0%, var(--color-bg-start) 50%, var(--color-bg-end) 100%);
        min-height: 100vh;
        color: var(--color-cream);
        overflow-x: hidden;
        position: relative;
    }

    .font-serif-title {
        font-family: 'Playfair Display', serif;
    }

    .font-cursive {
        font-family: 'Dancing Script', cursive;
    }

    /* Animation de fond (Particules & Cœurs) */
    #particles-canvas {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        pointer-events: none;
        z-index: 1;
    }

    /* Coffret Cadeau Luxueux */
    .box-container {
        position: relative;
        z-index: 10;
        background: linear-gradient(145deg, #3d1c10, #220e06);
        border: 2px solid rgba(212, 175, 55, 0.4);
        box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.8),
                    0 0 30px rgba(212, 175, 55, 0.15),
                    inset 0 1px 2px rgba(255, 255, 255, 0.1);
        border-radius: 2rem;
        padding: 2.5rem 1.5rem;
    }

    /* Ruban Décoratif autour du coffret */
    .box-ribbon-h {
        position: absolute;
        top: 50%;
        left: 0;
        right: 0;
        height: 12px;
        transform: translateY(-50%);
        background: linear-gradient(90deg, transparent, rgba(232, 180, 184, 0.2), rgba(212, 175, 55, 0.4), rgba(232, 180, 184, 0.2), transparent);
        pointer-events: none;
    }

    /* Cellule d'alvéole pour chaque chocolat */
    .chocolate-socket {
        background: radial-gradient(circle, #1a0803 0%, #0d0401 100%);
        border-radius: 1.25rem;
        box-shadow: inset 0 4px 8px rgba(0,0,0,0.9), 0 1px 1px rgba(255,255,255,0.05);
        aspect-ratio: 1 / 1;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        padding: 0.5rem;
    }

    /* Base du Chocolat */
    .choc-item {
        width: 85%;
        height: 85%;
        cursor: pointer;
        transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        user-select: none;
        touch-action: manipulation;
    }

    .choc-item:hover:not(.eaten) {
        transform: translateY(-6px) scale(1.08);
        filter: drop-shadow(0 10px 15px rgba(212, 175, 55, 0.35));
    }

    .choc-item:active:not(.eaten) {
        transform: translateY(-2px) scale(1.02);
    }

    /* Designs des Chocolats */

    /* 1. Truffe Poudrée (Ronde) */
    .choc-truffle {
        border-radius: 50%;
        background: radial-gradient(circle at 35% 35%, #6d391e, #3a1a0b, #1d0b04);
        box-shadow: 0 6px 12px rgba(0,0,0,0.6), inset -2px -2px 6px rgba(0,0,0,0.8), inset 2px 2px 6px rgba(255,255,255,0.2);
        border: 1px dashed rgba(212, 175, 55, 0.3);
    }

    /* 2. Dôme Doré (Ovale/Rond) */
    .choc-gold-dome {
        border-radius: 50%;
        background: radial-gradient(circle at 30% 30%, #ffe89e, #d4af37 40%, #8a6d1c 80%, #423207);
        box-shadow: 0 6px 12px rgba(0,0,0,0.6), inset -2px -2px 4px rgba(0,0,0,0.5), inset 2px 2px 8px rgba(255,255,255,0.8);
    }

    /* 3. Carré Praliné Sculpté */
    .choc-square {
        border-radius: 12px;
        background: linear-gradient(135deg, #4d2311, #2b1106);
        box-shadow: 0 6px 12px rgba(0,0,0,0.6), inset 1px 1px 2px rgba(255,255,255,0.2);
        position: relative;
    }
    .choc-square::after {
        content: '';
        position: absolute;
        width: 60%;
        height: 60%;
        border: 1.5px solid rgba(232, 180, 184, 0.4);
        transform: rotate(45deg);
        border-radius: 4px;
    }

    /* 4. Pyramide Gianduja */
    .choc-pyramid {
        clip-path: polygon(50% 0%, 100% 85%, 50% 100%, 0% 85%);
        background: linear-gradient(135deg, #7c4122, #47210e, #240e04);
        width: 80%;
        height: 80%;
        filter: drop-shadow(0 5px 5px rgba(0,0,0,0.6));
    }

    /* 5. CHOCOLAT SPÉCIAL - GRAND CŒUR CENTRAL */
    .choc-special-heart {
        background: radial-gradient(circle at 35% 30%, #ff758c, #ff7eb3 20%, #a82e3e 60%, #4a0c16 100%);
        clip-path: path('M 12,35 A 12,12 0 0,1 35,22 A 12,12 0 0,1 58,35 Q 58,50 35,66 Q 12,50 12,35 Z');
        width: 100%;
        height: 100%;
        filter: drop-shadow(0 8px 15px rgba(220, 38, 38, 0.5));
        animation: pulseGlow 2.5s infinite ease-in-out;
    }

    @keyframes pulseGlow {
        0%, 100% {
            transform: scale(1);
            filter: drop-shadow(0 6px 10px rgba(232, 180, 184, 0.4));
        }
        50% {
            transform: scale(1.06);
            filter: drop-shadow(0 10px 22px rgba(212, 175, 55, 0.8));
        }
    }

    /* SVG Heart Fallback for Cross-browser Precision */
    .heart-shape {
        width: 85%;
        height: 85%;
        filter: drop-shadow(0 6px 10px rgba(0,0,0,0.6));
        transition: all 0.3s ease;
    }

    /* 6. Feuille d'Or (Luxe) */
    .choc-gold-leaf {
        border-radius: 18px;
        background: radial-gradient(circle at 40% 40%, #522814, #281106);
        box-shadow: 0 6px 12px rgba(0,0,0,0.6);
        overflow: hidden;
    }
    .choc-gold-leaf::before {
        content: '';
        position: absolute;
        top: 20%;
        left: 20%;
        width: 35%;
        height: 35%;
        background: linear-gradient(135deg, #ffe58f, #d4af37);
        clip-path: polygon(10% 20%, 40% 10%, 90% 30%, 70% 80%, 30% 90%, 0% 50%);
        opacity: 0.9;
    }

    /* 7. Dôme Ruby Rose */
    .choc-ruby {
        border-radius: 50%;
        background: radial-gradient(circle at 35% 30%, #f4a261, #e76f51 30%, #9b2226 70%, #4a0404);
        box-shadow: 0 6px 12px rgba(0,0,0,0.6), inset 1px 1px 3px rgba(255,255,255,0.4);
    }

    /* 8. Rectangle Zébré */
    .choc-swirl {
        border-radius: 10px;
        background: linear-gradient(160deg, #3d1b0c, #1f0b04);
        box-shadow: 0 6px 12px rgba(0,0,0,0.6);
        position: relative;
        overflow: hidden;
    }
    .choc-swirl::before {
        content: '';
        position: absolute;
        width: 150%;
        height: 100%;
        background: repeating-linear-gradient(
            45deg,
            transparent,
            transparent 8px,
            rgba(255, 248, 240, 0.25) 8px,
            rgba(255, 248, 240, 0.25) 12px
        );
    }

    /* 9. Palet Or Cœur */
    .choc-round-decor {
        border-radius: 50%;
        background: radial-gradient(circle at 30% 30%, #5c2f17, #240e04);
        box-shadow: 0 6px 12px rgba(0,0,0,0.6);
        display: flex;
        align-items: center;
        justify-content: center;
        color: #d4af37;
        font-size: 1rem;
    }

    /* État MANGÉ / DÉGUSTÉ */
    .choc-item.eaten {
        opacity: 0.45;
        filter: grayscale(40%) contrast(90%);
        transform: scale(0.92) !important;
        cursor: default;
    }

    .choc-item.eaten::after {
        content: '✓';
        position: absolute;
        font-size: 1.25rem;
        color: var(--color-gold);
        font-weight: bold;
        text-shadow: 0 0 6px rgba(0,0,0,0.9);
        z-index: 5;
    }

    /* Modale / Pop-up carte flottante */
    .modal-backdrop {
        position: fixed;
        inset: 0;
        background: rgba(15, 5, 2, 0.75);
        backdrop-filter: blur(8px);
        -webkit-backdrop-filter: blur(8px);
        z-index: 50;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 1.25rem;
        opacity: 0;
        pointer-events: none;
        transition: opacity 0.4s ease;
    }

    .modal-backdrop.active {
        opacity: 1;
        pointer-events: auto;
    }

    .modal-card {
        background: linear-gradient(135deg, rgba(45, 20, 10, 0.95), rgba(25, 8, 3, 0.98));
        border: 1px solid var(--color-gold);
        box-shadow: 0 20px 40px rgba(0, 0, 0, 0.8),
                    0 0 25px rgba(212, 175, 55, 0.25);
        border-radius: 1.5rem;
        max-width: 460px;
        width: 100%;
        padding: 2.25rem 2rem;
        text-align: center;
        transform: scale(0.85) translateY(20px);
        transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        position: relative;
    }

    .modal-backdrop.active .modal-card {
        transform: scale(1) translateY(0);
    }

    /* Bouton de Fermeture */
    .close-btn {
        position: absolute;
        top: 1rem;
        right: 1rem;
        width: 2rem;
        height: 2rem;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.08);
        border: 1px solid rgba(212, 175, 55, 0.3);
        color: var(--color-rose-gold);
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: all 0.2s ease;
    }

    .close-btn:hover {
        background: rgba(212, 175, 55, 0.2);
        color: white;
        transform: rotate(90deg);
    }

    /* Progression badge */
    .counter-badge {
        background: rgba(212, 175, 55, 0.15);
        border: 1px solid rgba(212, 175, 55, 0.3);
        backdrop-filter: blur(4px);
    }
</style>
</head>
<body class="flex flex-col min-h-screen justify-between items-center py-8 px-4">

<!-- Canvas pour l'arrière-plan interactif -->
<canvas id="particles-canvas"></canvas>

<!-- Header -->
<header class="text-center max-w-2xl mx-auto z-10 mb-6 space-y-3">
    <div class="inline-flex items-center gap-2 px-4 py-1.5 rounded-full counter-badge text-xs tracking-widest uppercase text-amber-200/90 font-medium shadow-sm">
        <i class="fa-solid font-xs fa-heart text-rose-400"></i>
        <span>Un écrin de douceurs</span>
        <i class="fa-solid font-xs fa-heart text-rose-400"></i>
    </div>
    
    <h1 class="text-4xl md:text-5xl lg:text-6xl font-serif-title font-bold text-transparent bg-clip-text bg-gradient-to-r from-amber-100 via-rose-200 to-amber-200 leading-tight">
        Une petite douceur pour toi, Sarah
    </h1>
    
    <p class="text-base md:text-lg text-rose-100/80 font-light max-w-lg mx-auto">
        Chaque chocolat renferme une pensée sucrée... Clique sur chacun d'eux pour la découvrir.
    </p>

    <!-- Compteur de dégustation -->
    <div class="pt-1 text-xs text-amber-300/70 font-mono tracking-wider">
        Chocolats dégustés : <span id="eaten-count" class="text-amber-200 font-bold">0</span> / <span id="total-count">9</span>
    </div>
</header>

<!-- Conteneur Boîte de Chocolats -->
<main class="w-full max-w-md md:max-w-lg mx-auto my-auto z-10">
    <div class="box-container relative">
        <!-- Filigrane / Ruban visuel -->
        <div class="box-ribbon-h"></div>
        
        <!-- Grille 3x3 de Chocolats -->
        <div class="grid grid-cols-3 gap-4 md:gap-6 relative z-10" id="chocolate-grid">
            <!-- Généré dynamiquement en JS pour un contrôle propre -->
        </div>
    </div>
</main>

<!-- Footer -->
<footer class="text-center z-10 mt-8 text-xs text-amber-200/50 font-light">
    <p>Créé avec ❤️ spécialement pour Sarah</p>
</footer>

<!-- Modale Pop-up -->
<div id="secret-modal" class="modal-backdrop" onclick="closeModalOnBackdrop(event)">
    <div class="modal-card">
        <button class="close-btn" onclick="closeModal()" aria-label="Fermer">
            <i class="fa-solid fa-xmark"></i>
        </button>
        
        <div class="w-12 h-12 mx-auto mb-4 rounded-full bg-amber-500/10 border border-amber-400/30 flex items-center justify-center text-amber-300 text-xl">
            <i id="modal-icon" class="fa-solid fa-heart"></i>
        </div>

        <div id="modal-title" class="text-xs uppercase tracking-widest text-amber-300/70 font-semibold mb-2">
            Douceur N°1
        </div>

        <p id="modal-text" class="font-cursive text-2xl md:text-3xl text-rose-100 leading-relaxed my-4 min-h-[90px] flex items-center justify-center">
            <!-- Contenu du message -->
        </p>

        <div class="mt-6 pt-4 border-t border-amber-500/20 flex justify-center">
            <button onclick="closeModal()" class="px-6 py-2 rounded-full bg-gradient-to-r from-amber-600 to-amber-700 hover:from-amber-500 hover:to-amber-600 text-amber-50 font-medium text-sm transition-all shadow-lg hover:shadow-amber-900/50 active:scale-95">
                Garder ce doux souvenir ✨
            </button>
        </div>
    </div>
</div>

<!-- Script principal Logic & Animations -->
<script>
    // Data des chocolats et messages secrets
    const chocolatesData = [
        {
            id: 1,
            name: "Truffe Pralinée",
            type: "truffle",
            icon: "fa-seedling",
            text: "T’es la seule personne capable de me faire sourire même après la pire des journées."
        },
        {
            id: 2,
            name: "Dôme Éclat d'Or",
            type: "gold-dome",
            icon: "fa-face-smile-beam",
            text: "Ton rire est littéralement mon son préféré au monde."
        },
        {
            id: 3,
            name: "Carré Gianduja",
            type: "square",
            icon: "fa-compass",
            text: "Même à des kilomètres, t'arrives toujours à occuper toutes mes pensées."
        },
        {
            id: 4,
            name: "Feuille Croustillante",
            type: "gold-leaf",
            icon: "fa-sparkles",
            text: "T’es belle de l'extérieur, mais ta douceur et ton grand cœur me font complètement chavirer."
        },
        {
            // CHOCOLAT SPÉCIAL AU CENTRE (Index 4 dans le tableau 3x3)
            id: 5,
            name: "Cœur Suprême Ruby",
            type: "special-heart",
            isSpecial: true,
            icon: "fa-heart",
            text: "T’es ma favorite, mon évidence et la meilleure chose qui me soit arrivée. Je t’aime fort, Sarah. ❤️"
        },
        {
            id: 6,
            name: "Pyramide Caramel",
            type: "pyramid",
            icon: "fa-house-heart",
            text: "Avec toi, je me sens chez moi, peu importe où on est."
        },
        {
            id: 7,
            name: "Ruby Pistache",
            type: "ruby",
            icon: "fa-sun",
            text: "Ta présence illumine instantanément ma journée, comme un rayon de soleil."
        },
        {
            id: 8,
            name: "Fondant Zébré",
            type: "swirl",
            icon: "fa-gem",
            text: "Chaque petit moment passé à tes côtés est une pépite que je garde précieusement."
        },
        {
            id: 9,
            name: "Palet Moka",
            type: "round-decor",
            icon: "fa-infinity",
            text: "Merci d'être exactement qui tu es : extraordinaire, unique et irremplaçable."
        }
    ];

    let eatenSet = new Set();

    // Initialisation de la grille
    function initGrid() {
        const grid = document.getElementById('chocolate-grid');
        document.getElementById('total-count').textContent = chocolatesData.length;

        chocolatesData.forEach((choc, index) => {
            const socket = document.createElement('div');
            socket.className = 'chocolate-socket';

            const chocEl = document.createElement('div');
            chocEl.className = `choc-item`;
            chocEl.id = `choc-${choc.id}`;
            chocEl.setAttribute('role', 'button');
            chocEl.setAttribute('aria-label', choc.name);
            chocEl.onclick = () => openChocolate(choc.id);

            // Génération de la forme visuelle selon le type
            chocEl.appendChild(createChocVisual(choc.type, choc.isSpecial));

            socket.appendChild(chocEl);
            grid.appendChild(socket);
        });
    }

    // Crée les éléments HTML/SVG du chocolat
    function createChocVisual(type, isSpecial) {
        const wrapper = document.createElement('div');
        wrapper.className = 'w-full h-full flex items-center justify-center relative';

        if (isSpecial) {
            // Heart SVG pour une précision romantique parfaite
            wrapper.innerHTML = `
                <svg class="heart-shape animate-pulse" viewBox="0 0 32 29" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <defs>
                        <radialGradient id="heartGrad" cx="30%" cy="30%" r="70%">
                            <stop offset="0%" stop-color="#ff9a9e" />
                            <stop offset="40%" stop-color="#fecfef" />
                            <stop offset="70%" stop-color="#a82e3e" />
                            <stop offset="100%" stop-color="#4a0c16" />
                        </radialGradient>
                        <filter id="glow">
                            <feDropShadow dx="0" dy="2" stdDeviation="3" flood-color="#d4af37" flood-opacity="0.6"/>
                        </filter>
                    </defs>
                    <path d="M23.6,0 C20.6,0 17.9,1.4 16,3.6 C14.1,1.4 11.4,0 8.4,0 C3.8,0 0,3.8 0,8.4 C0,15 16,29 16,29 C16,29 32,15 32,8.4 C32,3.8 28.2,0 23.6,0 Z" fill="url(#heartGrad)" filter="url(#glow)"/>
                    <path d="M10,4 A 4 4 0 0 0 5,9" stroke="rgba(255,255,255,0.6)" stroke-width="1.5" stroke-linecap="round" fill="none" />
                </svg>
            `;
        } else {
            const inner = document.createElement('div');
            inner.className = `w-full h-full choc-${type}`;
            if (type === 'round-decor') {
                inner.innerHTML = '<i class="fa-solid fa-heart text-amber-300/80 text-xs"></i>';
            }
            wrapper.appendChild(inner);
        }

        return wrapper;
    }

    // Ouverture de la modale au clic
    function openChocolate(id) {
        const choc = chocolatesData.find(c => c.id === id);
        if (!choc) return;

        // Marquer comme mangé
        if (!eatenSet.has(id)) {
            eatenSet.add(id);
            document.getElementById(`choc-${id}`).classList.add('eaten');
            document.getElementById('eaten-count').textContent = eatenSet.size;
        }

        // Déclencher confettis si chocolat spécial cœur
        if (choc.isSpecial) {
            triggerHeartConfetti();
        }

        // Remplir la modale
        document.getElementById('modal-title').textContent = `${choc.name} • Message Secret`;
        document.getElementById('modal-text').textContent = `« ${choc.text} »`;
        
        const iconEl = document.getElementById('modal-icon');
        iconEl.className = `fa-solid ${choc.icon || 'fa-heart'}`;

        // Afficher la modale
        document.getElementById('secret-modal').classList.add('active');
    }

    // Fermeture de la modale
    function closeModal() {
        document.getElementById('secret-modal').classList.remove('active');
    }

    function closeModalOnBackdrop(e) {
        if (e.target.id === 'secret-modal') {
            closeModal();
        }
    }

    // Effet Spécial : Pluie de confettis en forme de cœurs
    function triggerHeartConfetti() {
        if (typeof confetti !== 'undefined') {
            // Salve 1
            confetti({
                particleCount: 70,
                spread: 80,
                origin: { y: 0.6 },
                colors: ['#e8b4b8', '#d4af37', '#ff4d6d', '#ffffff'],
                shapes: ['circle'],
                scalar: 1.2
            });

            // Salve 2 prolongée
            setTimeout(() => {
                confetti({
                    particleCount: 50,
                    angle: 60,
                    spread: 55,
                    origin: { x: 0 },
                    colors: ['#ff758c', '#d4af37']
                });
                confetti({
                    particleCount: 50,
                    angle: 120,
                    spread: 55,
                    origin: { x: 1 },
                    colors: ['#ff758c', '#e8b4b8']
                });
            }, 250);
        }
    }

    /* --- Animation de fond Canvas (Cœurs flottants & Étincelles) --- */
    const canvas = document.getElementById('particles-canvas');
    const ctx = canvas.getContext('2d');

    let width = canvas.width = window.innerWidth;
    let height = canvas.height = window.innerHeight;

    window.addEventListener('resize', () => {
        width = canvas.width = window.innerWidth;
        height = canvas.height = window.innerHeight;
    });

    class Particle {
        constructor() {
            this.reset();
        }

        reset() {
            this.x = Math.random() * width;
            this.y = height + 20;
            this.size = Math.random() * 12 + 6;
            this.speedY = Math.random() * 0.8 + 0.3;
            this.speedX = Math.sin(Math.random() * Math.PI) * 0.5;
            this.opacity = Math.random() * 0.5 + 0.2;
            this.isSparkle = Math.random() > 0.6;
            this.color = this.isSparkle ? '#d4af37' : '#e8b4b8';
            this.angle = Math.random() * 360;
            this.spin = (Math.random() - 0.5) * 1;
        }

        update() {
            this.y -= this.speedY;
            this.x += Math.sin(this.y * 0.01) * 0.5;
            this.angle += this.spin;

            if (this.y < -30) {
                this.reset();
            }
        }

        draw() {
            ctx.save();
            ctx.translate(this.x, this.y);
            ctx.rotate((this.angle * Math.PI) / 180);
            ctx.globalAlpha = this.opacity;

            if (this.isSparkle) {
                // Dessin d'une étincelle dorée
                ctx.fillStyle = this.color;
                ctx.beginPath();
                for (let i = 0; i < 4; i++) {
                    ctx.lineTo(Math.cos((i * Math.PI) / 2) * this.size, Math.sin((i * Math.PI) / 2) * this.size);
                    ctx.lineTo(Math.cos((i * Math.PI) / 2 + Math.PI / 4) * (this.size / 3), Math.sin((i * Math.PI) / 2 + Math.PI / 4) * (this.size / 3));
                }
                ctx.closePath();
                ctx.fill();
            } else {
                // Dessin d'un petit cœur rose doux
                ctx.fillStyle = this.color;
                ctx.beginPath();
                const topCurveHeight = this.size * 0.3;
                ctx.moveTo(0, topCurveHeight);
                ctx.bezierCurveTo(0, 0, -this.size / 2, 0, -this.size / 2, topCurveHeight);
                ctx.bezierCurveTo(-this.size / 2, (this.size + topCurveHeight) / 2, 0, this.size, 0, this.size);
                ctx.bezierCurveTo(0, this.size, this.size / 2, (this.size + topCurveHeight) / 2, this.size / 2, topCurveHeight);
                ctx.bezierCurveTo(this.size / 2, 0, 0, 0, 0, topCurveHeight);
                ctx.closePath();
                ctx.fill();
            }

            ctx.restore();
        }
    }

    const particles = Array.from({ length: 35 }, () => new Particle());

    function animateParticles() {
        ctx.clearRect(0, 0, width, height);
        particles.forEach(p => {
            p.update();
            p.draw();
        });
        requestAnimationFrame(animateParticles);
    }

    // Lancement au chargement de la page
    window.onload = () => {
        initGrid();
        animateParticles();
    };
</script>
</body>
</html>

