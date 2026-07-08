<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sophfishy's Fav Songs and Albums</title>
    <style>
        body {
            margin: 0;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-color: #1a1a1a;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            color: #ffffff;
            overflow: hidden;
        }

        /* Outer wrapper for layout */
        .gallery-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 100%;
            max-width: 800px;
        }

        /* 3D Scene viewport */
        .slider-viewport {
            position: relative;
            width: 300px;
            height: 300px;
            perspective: 1000px; /* Gives the 3D flipping effect depth */
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Individual Card Wrapper */
        .card {
            position: absolute;
            width: 100%;
            height: 100%;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            transition: transform 0.6s cubic-bezier(0.25, 1, 0.5, 1), opacity 0.6s;
            cursor: pointer;
            pointer-events: none; /* Prevents clicking background cards */
            opacity: 0;
            transform: translateX(0) scale(0.5) rotateY(0deg);
        }

        /* Make active and surrounding cards visible and clickable */
        .card.active {
            opacity: 1;
            pointer-events: auto;
            transform: translateX(0) scale(1) rotateY(0deg);
            z-index: 5;
            box-shadow: 0 15px 40px rgba(0, 173, 181, 0.3);
        }

        /* Card positioned to the left */
        .card.prev {
            opacity: 0.4;
            pointer-events: auto;
            transform: translateX(-110%) scale(0.85) rotateY(35deg);
            z-index: 3;
        }

        /* Card positioned to the right */
        .card.next {
            opacity: 0.4;
            pointer-events: auto;
            transform: translateX(110%) scale(0.85) rotateY(-35deg);
            z-index: 3;
        }

        /* The actual image */
        .card img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border: 2px solid #333;
            border-radius: 12px;
            box-sizing: border-box;
        }

        .card.active img {
            border-color: #00adb5;
        }

        /* Dynamic Text Description Panel */
        .description-panel {
            margin-top: 30px;
            text-align: center;
            min-height: 80px;
            max-width: 500px;
            padding: 10px 20px;
        }

        .description-panel h2 {
            margin: 0 0 8px 0;
            color: #00adb5;
            font-size: 24px;
        }

        .description-panel p {
            margin: 0;
            color: #ccc;
            font-size: 16px;
            line-height: 1.4;
        }

        /* Navigation Buttons UI */
        .controls {
            margin-top: 20px;
            display: flex;
            gap: 20px;
        }

        .nav-btn {
            background-color: #333;
            border: 1px solid #444;
            color: white;
            padding: 10px 20px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.2s;
        }

        .nav-btn:hover {
            background-color: #00adb5;
            border-color: #00adb5;
        }

        /* Micro tip for accessibility */
        .hint {
            margin-top: 15px;
            font-size: 12px;
            color: #555;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
    </style>
</head>
<body>

    <div class="gallery-container">
        
        <!-- 3D Flipping Card Stack -->
        <div class="slider-viewport">
            
            <!-- Item 1 -->
            <div class="card" data-title="Nurture By Porter Robinson" data-desc="A breathtaking view of snowy mountain peaks hitting the morning clouds.">
                <img src="nurture.png" alt="Nurture By Porter Robinson Album Cover">
            </div>
            
            <!-- Item 2 -->
            <div class="card" data-title="Fratality By Jane Remover" data-desc="Cyberpunk styled streets filled with glowing neon signs and rainy reflections.">
                <img src="fratality.jpg" alt="Fratality Albumn by Porter Robinson">
            </div>
            
            <!-- Item 3 -->
            <div class="card" data-title="Crest By Bladee and Ecco2k" data-desc="Sunlight piercing through thick canopy layers in an ancient woodland.">
                <img src="crest.jpg" alt="Crest Ablum by Bladee and Ecco2k">
            </div>
            
            <!-- Item 4 -->
            <div class="card" data-title="Sacred Play Secret Place by Matryoshka" data-desc="Crisp blue ocean waves crashing onto a secluded tropical sand beach.">
                <img src="matryoshka.jpg" alt="A song by Matryoshka">
            </div>
            
            <!-- Item 5 -->
            <div class="card" data-title="Re: Wired by Kuru" data-desc="Golden sand ridges stretching infinitely into a warm evening sunset.">
                <img src="kuru.jpeg" alt="rewired by Kuru">
            </div>

        </div>

        <!-- Dynamic Description Box -->
        <div class="description-panel">
            <h2 id="galleryTitle">Loading...</h2>
            <p id="galleryDesc">Please wait.</p>
        </div>

        <!-- Visual Click Controls -->
        <div class="controls">
            <button class="nav-btn" id="prevBtn">◀ PREV</button>
            <button class="nav-btn" id="nextBtn">NEXT ▶</button>
        </div>

        <div class="hint">Or Use Left / Right Arrow Keys</div>

    </div>

    <script>
        const cards = document.querySelectorAll('.card');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');
        const titleEl = document.getElementById('galleryTitle');
        const descEl = document.getElementById('galleryDesc');
        
        let currentIndex = 0;

        // Function to update classes and slide items
        function updateGallery() {
            cards.forEach((card, index) => {
                // Strip old layouts
                card.classList.remove('active', 'prev', 'next');

                if (index === currentIndex) {
                    card.classList.add('active');
                    // Update Text below dynamically from data- attributes
                    titleEl.textContent = card.getAttribute('data-title');
                    descEl.textContent = card.getAttribute('data-desc');
                } else if (index === (currentIndex - 1 + cards.length) % cards.length) {
                    card.classList.add('prev');
                } else if (index === (currentIndex + 1) % cards.length) {
                    card.classList.add('next');
                }
            });
        }

        // Move Forward
        function slideNext() {
            currentIndex = (currentIndex + 1) % cards.length;
            updateGallery();
        }

        // Move Backward
        function slidePrev() {
            currentIndex = (currentIndex - 1 + cards.length) % cards.length;
            updateGallery();
        }

        // 1. Button Click Events
        nextBtn.addEventListener('click', slideNext);
        prevBtn.addEventListener('click', slidePrev);

        // 2. Direct Card Click Events (allows skipping directly to adjacent cards)
        cards.forEach((card, index) => {
            card.addEventListener('click', () => {
                if (index !== currentIndex) {
                    currentIndex = index;
                    updateGallery();
                }
            });
        });

        // 3. Keyboard Arrow Key Listeners
        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowRight') {
                slideNext();
            } else if (e.key === 'ArrowLeft') {
                slidePrev();
            }
        });

        // Initialize display on startup
        updateGallery();
    </script>
</body>
</html>
