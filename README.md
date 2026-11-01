<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>game test</title>
    
    <style>
        body {
            background-color: #ff0000; /* Red page background */
            font-family: sans-serif;
            color: #ffffff;
            padding: 20px;
            margin: 0;
            text-align: center;
        }

        h1 {
            color: #000000;            /* Black title */
            font-size: 36px;
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 5px;
        }

        .subtitle {
            color: #000000;
            font-size: 14px;
            font-weight: bold;
            margin-bottom: 30px;
        }

        /* 20 GAMES GRID LAYOUT */
        .games-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            max-width: 900px;
            margin: 0 auto;
        }

        /* CARD BASIC LOOK */
        .game-card {
            background-color: #ffffff;
            border: 4px solid #000000;
            border-radius: 12px;
            padding: 12px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-between;
            min-height: 160px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
        }

        .game-title {
            color: #000000;
            font-size: 15px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .game-desc {
            color: #666666;
            font-size: 11px;
            margin-bottom: 8px;
            height: 28px;
            display: flex;
            align-items: center;
            text-align: center;
        }

        .score-box {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .click-btn {
            background-color: #000000;
            color: #ffffff;
            border: none;
            padding: 8px 15px;
            font-size: 13px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            width: 100%;
        }

        .click-btn:active {
            opacity: 0.8;
            transform: scale(0.96);
        }

        /* GENRE COLOR CODING (Applied to scores) */
        .genre-clicker { color: #2563eb; } /* Blue text */
        .genre-gambler { color: #b45309; } /* Orange text */
        .genre-clicker-minus { color: #dc2626; } /* Red text */
        .genre-multiplier { color: #16a34a; } /* Green text */
    </style>
</head>
<body>

    <h1>game test</h1>
    <p class="subtitle">20 Different Games • 4 Unique Genres</p>

    <div class="games-grid" id="gamesContainer"></div>

    <script>
        const container = document.getElementById('gamesContainer');

        // We run a single loop from 1 to 20, shifting mechanics every 5 cards
        for (let i = 1; i <= 20; i++) {
            const card = document.createElement('div');
            card.className = 'game-card';

            let title = "";
            let description = "";
            let scoreClass = "";
            let initialDisplay = "0";
            let buttonText = "TAP!";
            
            // Game Variables
            let count = 0; 
            let multiplier = 1;

            // 1. CHOOSE MECHANIC BASED ON GAME INDEX RANGE
            if (i <= 5) {
                // GAMES 1-5: The Standard Clickers (+1 per click)
                title = `Clicker ${i}`;
                description = "Standard clicker. Gains +1 point on every single hit.";
                scoreClass = "genre-clicker";
                
                card.innerHTML = `<div class="game-title">${title}</div><div class="game-desc">${description}</div><div class="score-box ${scoreClass}">Pts: <span class="val">${initialDisplay}</span></div><button class="click-btn">${buttonText}</button>`;
                
                card.querySelector('.click-btn').onclick = function() {
                    count += 1;
                    card.querySelector('.val').innerText = count;
                };

            } else if (i <= 10) {
                // GAMES 6-10: The Risk Gamblers (50/50 chance to win double or lose all)
                title = `Gamble ${i - 5}`;
                description = "Double or nothing! 50% chance to lose everything.";
                scoreClass = "genre-gambler";
                count = 10; // Start with some stakes
                initialDisplay = "10";
                buttonText = "RISK!";
                
                card.innerHTML = `<div class="game-title">${title}</div><div class="game-desc">${description}</div><div class="score-box ${scoreClass}">Risk: <span class="val">${initialDisplay}</span></div><button class="click-btn">${buttonText}</button>`;
                
                card.querySelector('.click-btn').onclick = function() {
                    if (count <= 0) {
                        count = 10; // Reset stakes if broke
                    } else {
                        count = Math.random() > 0.5 ? count * 2 : 0;
                    }
                    card.querySelector('.val').innerText = count;
                    card.querySelector('.click-btn').innerText = count === 0 ? "RESET" : "RISK!";
                };

            } else if (i <= 15) {
                // GAMES 11-15: The Countdown Bombs (Must count down to exactly zero)
                title = `Bomb ${i - 10}`;
                description = "Defuse the bomb! Click to reduce the timer down to 0.";
                scoreClass = "genre-clicker-minus";
                count = 100; // Start high
                initialDisplay = "100";
                buttonText = "TICK";
                
                card.innerHTML = `<div class="game-title">${title}</div><div class="game-desc">${description}</div><div class="score-box ${scoreClass}">Time: <span class="val">${initialDisplay}</span></div><button class="click-btn">${buttonText}</button>`;
                
                card.querySelector('.click-btn').onclick = function() {
                    if (count <= 0) {
                        count = 100; // Reset game
                    } else {
                        count -= 5;
                    }
                    card.querySelector('.val').innerText = count <= 0 ? "DEFUSED!" : count;
                    card.querySelector('.click-btn').innerText = count <= 0 ? "RESTART" : "TICK";
                };

            } else {
                // GAMES 16-20: The Capital Multipliers (Upgrades button power)
                title = `Market ${i - 15}`;
                description = "Earn points, then auto-buy multipliers to gain points faster.";
                scoreClass = "genre-multiplier";
                buttonText = "+1 Point";
                
                card.innerHTML = `<div class="game-title">${title}</div><div class="game-desc">${description}</div><div class="score-box ${scoreClass}">Val: <span class="val">${initialDisplay}</span></div><button class="click-btn">${buttonText}</button>`;
                
                card.querySelector('.click-btn').onclick = function() {
                    count += multiplier;
                    // Auto upgrade engine rules
                    if (count >= 15 * multiplier) {
                        count -= 15 * multiplier;
                        multiplier++;
                    }
                    card.querySelector('.val').innerText = count;
                    card.querySelector('.click-btn').innerText = `+${multiplier} Pts`;
                };
            }

            // 2. INJECT GAME INTO ROW
            container.appendChild(card);
        }
    </script>

</body>
</html>
