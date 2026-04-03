# inspiration42023-wq.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cosmic Planet Number Calculator</title>
    <style>
        /* Solar System Background */
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            display: flex; 
            justify-content: center; 
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background: radial-gradient(circle at center, #1b2735 0%, #090a0f 100%);
            overflow-x: hidden;
            color: white;
        }

        /* Animated Stars */
        body::before {
            content: "";
            position: absolute;
            width: 2px; height: 2px;
            background: white;
            box-shadow: 10vw 20vh white, 30vw 50vh white, 70vw 10vh white, 90vw 80vh white, 40vw 90vh white, 15vw 75vh white;
            border-radius: 50%;
            animation: twinkle 5s infinite;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        .card { 
            background: rgba(255, 255, 255, 0.1); 
            backdrop-filter: blur(10px);
            padding: 2rem; 
            border-radius: 15px; 
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 8px 32px rgba(0,0,0,0.5); 
            width: 350px; 
            z-index: 10;
        }

        h3 { margin-top: 0; color: #ffcc00; text-align: center; text-transform: uppercase; letter-spacing: 2px; }
        p { font-size: 0.9rem; text-align: center; opacity: 0.8; }

        input { 
            width: 100%; padding: 12px; margin: 15px 0; 
            border: none; border-radius: 8px; box-sizing: border-box; 
            background: rgba(255,255,255,0.9); font-size: 1rem; color: black;
        }

        button.calc-btn { 
            width: 100%; padding: 12px; 
            background: linear-gradient(45deg, #ffcc00, #ff6600); 
            color: black; font-weight: bold; border: none; 
            border-radius: 8px; cursor: pointer; transition: 0.3s;
        }

        button.calc-btn:hover { transform: scale(1.02); filter: brightness(1.1); }

        #result { 
            margin-top: 20px; 
            padding: 15px;
            border-radius: 8px;
            background: rgba(0, 0, 0, 0.3);
            display: none; 
        }

        .planet-name { color: #ffcc00; font-size: 1.4rem; display: block; margin-bottom: 5px; }
        .trait-line { font-style: italic; color: #a0e9ff; margin-bottom: 10px; display: block; }
        .info-grid { font-size: 0.85rem; text-align: left; line-height: 1.4; }
        .label { font-weight: bold; color: #ffcc00; }

        /* Support Link Styles */
        .support-container {
            margin-top: 25px;
            text-align: center;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            padding-top: 15px;
        }
        .paypal-link {
            color: #ffcc00;
            text-decoration: none;
            font-size: 0.9rem;
            font-weight: bold;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            transition: 0.3s;
            text-shadow: 0 0 8px rgba(255, 204, 0, 0.4);
        }
        .paypal-link:hover {
            text-shadow: 0 0 15px rgba(255, 204, 0, 0.8);
            transform: translateY(-1px);
        }
    </style>
</head>
<body>

<div class="card">
    <h3>Planet Number</h3>
    <p>Discover the celestial force behind your number</p>
    <input type="number" id="numInput" placeholder="Enter birth date or number">
    <button class="calc-btn" onclick="calculateRoot()">Reveal Destiny</button>
    
    <div id="result">
        <span id="resPlanet" class="planet-name"></span>
        <span id="resTraits" class="trait-line"></span>
        <div class="info-grid">
            <div><span class="label">Friendly:</span> <span id="resFriendly"></span></div>
            <div><span class="label">Enemy:</span> <span id="resEnemy"></span></div>
            <div><span class="label">Neutral:</span> <span id="resNeutral"></span></div>
        </div>
    </div>

    <div class="support-container">
        <p style="margin-bottom: 10px; font-size: 0.75rem; color: #aaa;">Enjoying the cosmic insight?</p>
        <a href="https://paypal.me/KSriPurushottam" class="paypal-link" target="_blank">
             Support the Creator
        </a>
    </div>
</div>

<script>
    const planetData = {
        1: { name: "Sun", traits: "Creative force, life-giver, leadership", friendly: "1, 2, 3, 5, 6, 9", enemy: "8", neutral: "4, 7" },
        2: { name: "Moon", traits: "Emotion, intuition, sensitivity", friendly: "1, 2, 3, 5", enemy: "8, 4, 9", neutral: "7, 6" },
        3: { name: "Jupiter", traits: "Wealth, wisdom, prosperity", friendly: "1, 2, 3, 5, 7", enemy: "6", neutral: "4, 8, 7, 9" },
        4: { name: "Uranus", traits: "Sudden change, unpredictability, innovation", friendly: "1, 5, 7, 6, 4, 8", enemy: "2, 9, 4, 8", neutral: "3" },
        5: { name: "Mercury", traits: "Speed, communication, knowledge", friendly: "1, 2, 3, 5, 6", enemy: "None", neutral: "4, 7, 8, 9" },
        6: { name: "Venus", traits: "Luxury, beauty, love, harmony", friendly: "1, 5, 6, 7", enemy: "3", neutral: "2, 4, 8, 9" },
        7: { name: "Neptune", traits: "Spirituality, mysticism, depth", friendly: "1, 3, 5, 4, 6", enemy: "None", neutral: "8, 2, 7, 9" },
        8: { name: "Saturn", traits: "Discipline, karma, struggle", friendly: "5, 3, 6, 7, 4, 8", enemy: "1, 2, 4, 8", neutral: "9" },
        9: { name: "Mars", traits: "Energy, courage, conflict", friendly: "1, 3, 5", enemy: "4, 2", neutral: "9, 7, 6, 8" }
    };

    function calculateRoot() {
        let val = document.getElementById('numInput').value;
        if (!val) return;

        let n = Math.abs(parseInt(val));
        let root = n === 0 ? 0 : 1 + ((n - 1) % 9);

        if (root > 0) {
            const data = planetData[root];
            document.getElementById('resPlanet').innerText = root + " - " + data.name;
            document.getElementById('resTraits').innerText = data.traits;
            document.getElementById('resFriendly').innerText = data.friendly;
            document.getElementById('resEnemy').innerText = data.enemy;
            document.getElementById('resNeutral').innerText = data.neutral;
            
            document.getElementById('result').style.display = "block";
        }
    }
</script>

</body>
</html>
