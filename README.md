<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Click Rotating Image Circle</title>
    <style>
        :root {
            --circle-size: 400px;
            --image-size: 80px;
        }

        body {
            margin: 0;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-color: #1a1a1a;
            font-family: sans-serif;
            overflow: hidden;
        }

        /* Container that holds the wheel */
        .wheel-container {
            position: relative;
            width: var(--circle-size);
            height: var(--circle-size);
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* The actual rotating circle */
        .image-circle {
            position: absolute;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            border: 2px dashed #444;
            transition: transform 0.8s cubic-bezier(0.25, 1, 0.5, 1);
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Individual image wrapper */
        .img-wrap {
            position: absolute;
            width: var(--image-size);
            height: var(--image-size);
            /* Formula to distribute items dynamically via JS */
            transform: rotate(var(--angle)) translate(calc(var(--circle-size) / 2)) rotate(calc(-1 * var(--angle)));
        }

        /* The clickable images */
        .img-wrap img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: 50%;
            border: 3px solid #fff;
            cursor: pointer;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
            transition: transform 0.2s, border-color 0.2s;
        }

        .img-wrap img:hover {
            transform: scale(1.1);
            border-color: #00adb5;
        }

        /* Control Button in the center */
        .rotate-btn {
            position: absolute;
            z-index: 10;
            width: 90px;
            height: 90px;
            border-radius: 50%;
            border: none;
            background-color: #00adb5;
            color: white;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0, 173, 181, 0.4);
            transition: transform 0.1s, background-color 0.2s;
        }

        .rotate-btn:hover {
            background-color: #00fff2;
            color: #1a1a1a;
        }

        .rotate-btn:active {
            transform: scale(0.95);
        }
    </style>
</head>
<body>

    <div class="wheel-container">
        <!-- Center Action Button -->
        <button class="rotate-btn" id="rotateBtn">ROTATE</button>

        <!-- Rotating Circle Wheel -->
        <div class="image-circle" id="imageCircle">
            
            <!-- Image 1 -->
            <div class="img-wrap">
                <a href="crest.jpg" target="_blank">
                    <img src="image1.jpg" alt="Crest by Bladee and Ecco2k">
                </a>
            </div>
            
            <!-- Image 2 -->
            <div class="img-wrap">
                <a href="fratality.jpg" target="_blank">
                    <img src="image2.jpg" alt="Fratality by Jane remover">
                </a>
            </div>
            
            <!-- Image 3 -->
            <div class="img-wrap">
                <a href="kuru.jpeg" target="_blank">
                    <img src="image3.jpg" alt="Re:wired by Kuru">
                </a>
            </div>
            
            <!-- Image 4 -->
            <div class="img-wrap">
                <a href="let me be with you.jpeg" target="_blank">
                    <img src="image4.jpg" alt=" Round Table Ft. Nino's songs">
                </a>
            </div>
            
            <!-- Image 5 -->
            <div class="img-wrap">
                <a href="matryoshka.jpg target="_blank">
                    <img src="image5.jpg" alt="Laideronnette by Matryoshka">
                </a>
            </div>

            <!-- Image 6 -->
            <div class="img-wrap">
                <a href="image6.jpg" target="_blank">
                    <img src="nurture.png" alt="Nurture by Porter Robinson">
                </a>
            </div>

        </div>
    </div>

    <script>
        const circle = document.getElementById('imageCircle');
        const button = document.getElementById('rotateBtn');
        const items = document.querySelectorAll('.img-wrap');
        
        const totalItems = items.length;
        let currentRotation = 0;

        // 1. Automatically space the images perfectly in a circle
        items.forEach((item, index) => {
            const angle = (360 / totalItems) * index;
            item.style.setProperty('--angle', `${angle}deg`);
        });

        // 2. Rotate the circle when the center button is clicked
        button.addEventListener('click', () => {
            // Rotates by the exact angle chunk to bring the next image forward
            currentRotation -= (360 / totalItems); 
            circle.style.transform = `rotate(${currentRotation}deg)`;

            // Counter-rotate items so the images themselves stay upright
            items.forEach((item, index) => {
                const angle = (360 / totalItems) * index;
                item.style.transform = `rotate(${angle}deg) translate(calc(var(--circle-size) / 2)) rotate(calc(-1 * ${angle}deg - ${currentRotation}deg))`;
            });
        });
    </script>
</body>
</html>
