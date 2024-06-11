<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Titan Bot Voting Page</title>
    <style>
        body {
            margin: 0;
            padding: 20px;
            font-family: Arial, sans-serif;
            color: white;
            background: black;
            text-align: center;
        }
        h1, h2 {
            color: #FFD700;
            text-shadow: 2px 2px #333, 4px 4px #222, 6px 6px #111;
        }
        button, input[type="text"] {
            background-color: grey;
            color: white;
            border: none;
            padding: 15px 30px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            margin: 10px;
            cursor: pointer;
            border-radius: 5px;
            transition: background-color 0.3s;
        }
        button:hover, input[type="text"]:hover {
            background-color: #575757;
        }
        #matrix {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
        }
        #nation-id-form, #candidates-list {
            display: none;
        }
        .candidate-button {
            margin: 5px;
        }
        .disabled {
            background-color: #333;
            cursor: not-allowed;
        }
    </style>
</head>
<body>

<canvas id="matrix"></canvas>

<h1>Titan Bot Voting Page</h1>

<div id="vote-section">
    <button id="start-vote-btn">Start Voting</button>

    <div id="nation-id-form">
        <h2>Enter Your Nation ID</h2>
        <input type="text" id="nation-id" placeholder="Nation ID">
        <button id="submit-nation-id-btn">Continue</button>
    </div>

    <div id="candidates-list">
        <h2>Select Your Candidate</h2>
        <!-- Candidate buttons will be added here dynamically -->
    </div>
</div>

<div id="completion-message" style="display: none;">
    <h2>Voting Completed!</h2>
</div>

<script>
    const candidates = ["Candidate 1", "Candidate 2", "Candidate 3"]; // Add your candidates here

    const startVoteBtn = document.getElementById('start-vote-btn');
    const nationIdForm = document.getElementById('nation-id-form');
    const submitNationIdBtn = document.getElementById('submit-nation-id-btn');
    const candidatesList = document.getElementById('candidates-list');
    const completionMessage = document.getElementById('completion-message');

    startVoteBtn.addEventListener('click', () => {
        startVoteBtn.style.display = 'none';
        nationIdForm.style.display = 'block';
    });

    submitNationIdBtn.addEventListener('click', () => {
        const nationId = document.getElementById('nation-id').value;
        if (nationId.trim() === "") {
            alert("Please enter your Nation ID");
            return;
        }
        nationIdForm.style.display = 'none';
        candidatesList.style.display = 'block';
        displayCandidates();
    });

    function displayCandidates() {
        candidates.forEach(candidate => {
            const button = document.createElement('button');
            button.innerText = candidate;
            button.className = 'candidate-button';
            button.addEventListener('click', () => voteForCandidate(button));
            candidatesList.appendChild(button);
        });
    }

    function voteForCandidate(button) {
        const buttons = document.querySelectorAll('.candidate-button');
        buttons.forEach(btn => {
            btn.disabled = true;
            btn.classList.add('disabled');
        });
        completionMessage.style.display = 'block';
    }

    const canvas = document.getElementById('matrix');
    const ctx = canvas.getContext('2d');
    
    function resizeCanvas() {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
    }

    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();
    
    const letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    const fontSize = 16;
    const columns = Math.floor(canvas.width / fontSize);
    
    const drops = Array(columns).fill(1);
    
    function draw() {
        ctx.fillStyle = "rgba(0, 0, 0, 0.05)";
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        
        ctx.fillStyle = "#0F0";
        ctx.font = `${fontSize}px monospace`;
        
        drops.forEach((y, index) => {
            const text = letters[Math.floor(Math.random() * letters.length)];
            const x = index * fontSize;
            ctx.fillText(text, x, y * fontSize);
            
            if (y * fontSize > canvas.height && Math.random() > 0.975) {
                drops[index] = 0;
            }
            drops[index]++;
        });
    }
    
    setInterval(draw, 33);
</script>

</body>
</html>
