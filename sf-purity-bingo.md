---
layout: none
title: SF Purity Bingo
permalink: /sf-purity-bingo/
description: icebreaker
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SF Purity Bingo</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #ff6b6b, #feca57);
            padding: 30px;
            text-align: center;
            color: white;
        }

        .header h1 {
            font-size: 2.5rem;
            font-weight: 800;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.1rem;
            opacity: 0.9;
            max-width: 600px;
            margin: 0 auto;
            line-height: 1.6;
        }

        .content {
            padding: 40px;
        }

        .player-info {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 30px;
            border-left: 5px solid #667eea;
        }

        .player-info input {
            width: 100%;
            padding: 12px;
            border: 2px solid #e9ecef;
            border-radius: 8px;
            font-size: 1rem;
            transition: border-color 0.3s ease;
        }

        .player-info input:focus {
            outline: none;
            border-color: #667eea;
        }

        .progress-bar {
            background: #e9ecef;
            height: 8px;
            border-radius: 4px;
            margin: 20px 0;
            overflow: hidden;
        }

        .progress-fill {
            background: linear-gradient(135deg, #667eea, #764ba2);
            height: 100%;
            width: 0%;
            transition: width 0.3s ease;
        }

        .progress-text {
            text-align: center;
            font-weight: 600;
            color: #667eea;
            margin-bottom: 20px;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
            gap: 20px;
        }

        .bingo-item {
            background: white;
            border: 2px solid #e9ecef;
            border-radius: 12px;
            padding: 20px;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .bingo-item:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            border-color: #667eea;
        }

        .bingo-item.completed {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-color: #667eea;
        }

        .bingo-item.completed::after {
            content: '✓';
            position: absolute;
            top: 15px;
            right: 15px;
            font-size: 1.5rem;
            font-weight: bold;
        }

        .item-number {
            font-weight: bold;
            color: #667eea;
            font-size: 0.9rem;
            margin-bottom: 8px;
        }

        .bingo-item.completed .item-number {
            color: rgba(255,255,255,0.8);
        }

        .item-text {
            font-size: 1rem;
            line-height: 1.4;
            margin-bottom: 12px;
        }

        .name-input {
            width: 100%;
            padding: 8px 12px;
            border: 2px solid #e9ecef;
            border-radius: 6px;
            font-size: 0.9rem;
            background: #f8f9fa;
            transition: all 0.3s ease;
        }

        .bingo-item.completed .name-input {
            background: rgba(255,255,255,0.2);
            border-color: rgba(255,255,255,0.3);
            color: white;
        }

        .bingo-item.completed .name-input::placeholder {
            color: rgba(255,255,255,0.7);
        }

        .name-input:focus {
            outline: none;
            border-color: #667eea;
            background: white;
        }

        .actions {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 30px;
            flex-wrap: wrap;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-decoration: none;
            display: inline-block;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
        }

        .btn-secondary {
            background: #6c757d;
            color: white;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        .share-section {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 15px;
            margin-top: 20px;
            text-align: center;
        }

        .score-display {
            font-size: 2rem;
            font-weight: bold;
            color: #667eea;
            margin-bottom: 10px;
        }

        @media (max-width: 768px) {
            .grid {
                grid-template-columns: 1fr;
            }

            .header h1 {
                font-size: 2rem;
            }

            .content {
                padding: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎯 SF PURITY BINGO</h1>
            <p>Fill in the blanks with either your name or someone else's name if it applies to them! One person can only fill out 5 spots on your bingo! Go talk to people and see if you can fill it all the way out.

            <br><br>
            Note: Get people's full names because we will be contacting random people to check the winner!</p>
        </div>

        <div class="content">
            <div class="player-info">
                <label for="playerName" style="display: block; margin-bottom: 8px; font-weight: 600; color: #495057;">Your Name:</label>
                <input type="text" id="playerName" placeholder="Enter your name here..." />
            </div>

            <div class="progress-text">Progress: <span id="progressCount">0</span>/50 completed</div>
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill"></div>
            </div>

            <div class="grid" id="bingoGrid"></div>

            <div class="actions">
                <button class="btn btn-primary" onclick="shareResults()">📤 Share Results</button>
                <button class="btn btn-secondary" onclick="resetBingo()">🔄 Reset All</button>
            </div>

            <div class="share-section" id="shareSection" style="display: none;">
                <div class="score-display" id="finalScore"></div>
                <p>Copy this text to share your results:</p>
                <textarea id="shareText" readonly style="width: 100%; height: 100px; margin-top: 10px; padding: 10px; border-radius: 8px; border: 2px solid #e9ecef;"></textarea>
            </div>
        </div>
    </div>

<!-- [Everything above <script>] remains the same -->

<script>
    const bingoItems = [
        "Applied to a job at OpenAI",
        "Been asked \"what's your AGI timeline\"",
        "Told someone your AGI timeline",
        "Switched from ChatGPT to Claude",
        "Switched back from Claude to ChatGPT",
        "Gave up on learning and downloaded Cursor",
        "Considered alternate careers when Devin was released",
        "Built a RAG pipeline",
        "Used LlamaIndex, Langchain, or CrewAI",
        "Attempted to jailbreak ChatGPT, Claude, or any popular LLM",
        "Taken a meeting at the blue bottle in 2 South Park",
        "Taken a job interview/pitch meeting at Cafe Reveille",
        "Worked late at Delahs coffee",
        "Attended a party at Mission Control",
        "Attended a party at AGI house",
        "Co-worked from Newton or Solaris",
        "Co-worked from SF Commons",
        "Ate Souvla more than 3 times in one week",
        "Drank too much at Monroes",
        "Ordered an espresso martini at Balboas",
        "Got rejected by YC but pretended you didn't apply",
        "Got accepted into YC",
        "Slept on a mattress without a bed frame",
        "Started or attempted to start an AI startup",
        "Pivoted your non-AI startup into an AI-first startup",
        "Listened to a Latent Space podcast episode",
        "Read \"The Chip War\" by Chris Miller",
        "Read \"Zero to One\" by Peter Thiel",
        "Read \"Atomic Habits\" by James Clear",
        "Watched or taken a Deep Learning course",
        "Bought an 8 Sleep",
        "Attempted the 75 Hard",
        "Stripped down for Archimedes Banya",
        "Worn more than 1 fitness wearable simultaneously",
        "Subscribed to Blueprint meals and didn't lose weight",
        "Watched Polymarket during the election",
        "Followed Sam Altman on Twitter",
        "Followed Andrej Karpathy on Twitter",
        "Tweeted \"OpenAI is nothing without its people\"",
        "Tried airchat and churned",
        "Asked someone out at a tech event",
        "Gone on a date with someone you met at a tech event",
        "Written a date me doc",
        "Told someone you're not going to date because AGI is coming",
        "Gone on a Hinge date with a non-tech person and realized how out of touch you are",
        "Unironically called a startup \"ngmi\"",
        "Had a friend say \"I'm gonna move to NYC\" because they can't pull in SF",
        "Taken a Waymo",
        "Actively calculated the gender ratio at an event",
        "Lived in a hacker house (>50% founders)"
    ];

    let completedItems = new Set();
    let nameTracker = {};

    function initializeBingo() {
        const grid = document.getElementById('bingoGrid');
        grid.innerHTML = '';

        loadFromLocalStorage();

        bingoItems.forEach((item, index) => {
            const itemDiv = document.createElement('div');
            itemDiv.className = 'bingo-item';
            itemDiv.dataset.index = index;

            itemDiv.innerHTML = `
                <div class="item-number">#${index + 1}</div>
                <div class="item-text">${item}</div>
                <input type="text" class="name-input" placeholder="Enter name..." />
            `;

            const input = itemDiv.querySelector('.name-input');
            input.addEventListener('input', (e) => {
                handleInputChange(index, e.target.value, itemDiv);
            });

            if (completedItems.has(index)) {
                input.value = nameTracker[index];
                itemDiv.classList.add('completed');
            }

            grid.appendChild(itemDiv);
        });

        document.getElementById('playerName').addEventListener('input', saveToLocalStorage);
        updateProgress();
    }

    function handleInputChange(index, value, itemDiv) {
        const trimmedValue = value.trim();

        if (trimmedValue) {
            if (!completedItems.has(index)) {
                const nameCount = Object.values(nameTracker).filter(name => name.toLowerCase() === trimmedValue.toLowerCase()).length;
                if (nameCount >= 5) {
                    alert(`${trimmedValue} has already been used 5 times! Try someone else.`);
                    itemDiv.querySelector('.name-input').value = '';
                    return;
                }
            }

            if (completedItems.has(index)) {
                delete nameTracker[index];
            }

            completedItems.add(index);
            nameTracker[index] = trimmedValue;
            itemDiv.classList.add('completed');
        } else {
            if (completedItems.has(index)) {
                completedItems.delete(index);
                delete nameTracker[index];
                itemDiv.classList.remove('completed');
            }
        }

        saveToLocalStorage();
        updateProgress();
    }

    function updateProgress() {
        const count = completedItems.size;
        const percentage = (count / 50) * 100;
        document.getElementById('progressCount').textContent = count;
        document.getElementById('progressFill').style.width = percentage + '%';
    }

    function shareResults() {
        const playerName = document.getElementById('playerName').value || 'Anonymous';
        const score = completedItems.size;

        let shareText = `🎯 SF PURITY BINGO RESULTS 🎯\n`;
        shareText += `Player: ${playerName}\n`;
        shareText += `Score: ${score}/50 (${Math.round((score / 50) * 100)}%)\n\n`;
        shareText += `Completed items:\n`;

        Array.from(completedItems).sort((a, b) => a - b).forEach(index => {
            const name = nameTracker[index];
            shareText += `✓ #${index + 1}: ${bingoItems[index]} (${name})\n`;
        });

        shareText += `\n🔗 Play at: [Your event URL here]`;

        document.getElementById('finalScore').textContent = `${score}/50 (${Math.round((score / 50) * 100)}%)`;
        document.getElementById('shareText').value = shareText;
        document.getElementById('shareSection').style.display = 'block';
        document.getElementById('shareSection').scrollIntoView({ behavior: 'smooth' });
    }

    function resetBingo() {
        if (confirm('Are you sure you want to reset all progress?')) {
            completedItems.clear();
            nameTracker = {};
            document.getElementById('playerName').value = '';
            document.getElementById('shareSection').style.display = 'none';

            document.querySelectorAll('.bingo-item').forEach(item => {
                item.classList.remove('completed');
                item.querySelector('.name-input').value = '';
            });

            localStorage.removeItem('bingoData');
            updateProgress();
        }
    }

    function saveToLocalStorage() {
        const data = {
            playerName: document.getElementById('playerName').value,
            completedItems: Array.from(completedItems),
            nameTracker: nameTracker
        };
        localStorage.setItem('bingoData', JSON.stringify(data));
    }

    function loadFromLocalStorage() {
        const saved = localStorage.getItem('bingoData');
        if (saved) {
            const data = JSON.parse(saved);
            document.getElementById('playerName').value = data.playerName || '';
            completedItems = new Set(data.completedItems || []);
            nameTracker = data.nameTracker || {};
        }
    }

    // Initialize on load
    initializeBingo();
</script>

</body>
</html>
