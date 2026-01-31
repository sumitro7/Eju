<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Birthday Wish</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #da22af;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }
        #countdown {
            font-size: 48px;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 10;
        }
        .falling-emoji {
            position: absolute;
            font-size: 24px;
            animation: fall linear infinite;
            z-index: 1;
        }
        @keyframes fall {
            0% { top: -50px; opacity: 1; }
            100% { top: 100vh; opacity: 0; }
        }
        #heart {
            font-size: 100px;
            animation: pulse 1s infinite;
            display: none;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 5;
        }
        @keyframes pulse {
            0% { transform: translate(-50%, -50%) scale(1); }
            50% { transform: translate(-50%, -50%) scale(1.1); }
            100% { transform: translate(-50%, -50%) scale(1); }
        }
        #clickButton {
            display: none;
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 18px;
            background-color: #ff69b4;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            position: absolute;
            top: 60%;
            left: 50%;
            transform: translateX(-50%);
            z-index: 5;
        }
        #page2 {
            display: none;
            flex-direction: column;
            align-items: center;
            width: 100%;
            z-index: 10;
        }
        #popupPhoto {
            width: 300px;
            height: 200px;
            border-radius: 10px;
            animation: popup 1s ease-out;
            display: none;
        }
        @keyframes popup {
            0% { transform: scale(0); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        #quote {
            font-size: 24px;
            margin: 20px;
            animation: popup 1s ease-out 0.5s both;
            display: none;
        }
        .photo-container {
            display: none;
            margin-top: 20px;
            text-align: center;
        }
        .photo-container img {
            width: 300px;
            height: 300px;
            border-radius: 10px;
        }
        .subtitle {
            font-size: 18px;
            margin-top: 10px;
        }
        #nextButton {
            display: none;
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 18px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        #notebook {
            display: none;
            margin: 20px auto;
            width: 595px; /* A4 width approximation */
            height: 842px; /* A4 height approximation */
            background-color: #fff;
            border: 2px solid #ccc;
            padding: 20px;
            font-family: 'Courier New', monospace;
            line-height: 1.5;
            position: relative;
            overflow-y: auto;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        #notebook::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-image: repeating-linear-gradient(transparent, transparent 24px, #ccc 24px, #ccc 25px);
            pointer-events: none;
        }
        #notebook p {
            margin: 0;
            padding: 0;
            position: relative;
            z-index: 1;
        }
    </style>
</head>
<body>
    <div id="countdown">5</div>
    <div id="heart">❤️</div>
    <button id="clickButton">Click</button>
    
    <div id="page2">
        <img id="popupPhoto" src="add file path" alt="Birthday Photo">
        <div id="quote">Hey, Another candle, another chapter, Shine on 🔥</div>
        
        <div class="photo-container">
            <img id="currentPhoto" src="" alt="Memory">
            <div class="subtitle" id="currentSubtitle"></div>
        </div>
        <button id="nextButton">Next</button>
        
        <div id="notebook">
            <p>Dear [name],</p>
            <p>On this special day, I want to wish you all the happiness in the world. May your year ahead be filled with love, laughter, and unforgettable adventures. You are truly one of a kind, and I'm grateful to have you in my life. Happy Birthday!</p>
            <p>With love, Wishing you a calm and happy birthday, filled with smiles, good health, and moments that remind you how appreciated you are as a wonderful friend.
</p>
            <p>[It's me 😁]</p>
        </div>
    </div>

    <script>
        // Falling emojis during countdown
        function createFallingEmoji() {
            const emoji = document.createElement('div');
            emoji.className = 'falling-emoji';
            emoji.textContent = Math.random() > 0.5 ? '😊' : '❤️';
            emoji.style.left = Math.random() * 100 + 'vw';
            emoji.style.animationDuration = (Math.random() * 3 + 2) + 's'; // 2-5 seconds
            document.body.appendChild(emoji);
            setTimeout(() => {
                emoji.remove();
            }, 5000);
        }

        let countdown = 5;
        const countdownElement = document.getElementById('countdown');
        const heartElement = document.getElementById('heart');
        const clickButton = document.getElementById('clickButton');
        const page2 = document.getElementById('page2');
        const popupPhoto = document.getElementById('popupPhoto');
        const quote = document.getElementById('quote');
        const photoContainer = document.querySelector('.photo-container');
        const currentPhoto = document.getElementById('currentPhoto');
        const currentSubtitle = document.getElementById('currentSubtitle');
        const nextButton = document.getElementById('nextButton');
        const notebook = document.getElementById('notebook');

        const photos = [
            { src: "file path", subtitle: "Our first adventure together" },
            { src: "file path", subtitle: "The most memorable day" },
            { src: "file path", subtitle: "The puppy lover 🐕" },
            { src: "file path", subtitle: "Cherished moments forever" }
        ];
        let photoIndex = 0;

        // Start falling emojis
        const emojiInterval = setInterval(createFallingEmoji, 200);

        const interval = setInterval(() => {
            countdownElement.textContent = countdown;
            countdown--;
            if (countdown < 0) {
                clearInterval(interval);
                clearInterval(emojiInterval);
                countdownElement.style.display = 'none';
                heartElement.style.display = 'block';
                setTimeout(() => {
                    clickButton.style.display = 'block';
                }, 2000); // Show heart for 2 seconds before button
            }
        }, 1000);

        clickButton.addEventListener('click', () => {
            heartElement.style.display = 'none';
            clickButton.style.display = 'none';
            page2.style.display = 'flex';
            popupPhoto.style.display = 'block';
            quote.style.display = 'block';
            setTimeout(() => {
                photoContainer.style.display = 'block';
                currentPhoto.src = photos[photoIndex].src;
                currentSubtitle.textContent = photos[photoIndex].subtitle;
                nextButton.style.display = 'block';
            }, 3000); // Show popup photo and quote for 3 seconds before photos
        });

        nextButton.addEventListener('click', () => {
            photoIndex++;
            if (photoIndex < photos.length) {
                currentPhoto.src = photos[photoIndex].src;
                currentSubtitle.textContent = photos[photoIndex].subtitle;
            } else {
                photoContainer.style.display = 'none';
                nextButton.style.display = 'none';
                notebook.style.display = 'block';
            }
        });
    </script>
</body>
</html>
