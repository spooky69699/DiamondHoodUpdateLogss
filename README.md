<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Diamond Hood Update Logs</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #000;
            color: #fff;
            overflow: hidden;
            position: relative;
        }

        canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
        }

        header {
            background-color: transparent;
            color: white;
            padding: 20px 0;
            text-align: center;
            text-shadow: 0 0 10px #4a90e2;
            font-size: 2em;
        }

        #viewCounter {
            position: absolute;
            top: 20px;
            right: 20px;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 10px 20px;
            border-radius: 5px;
            font-size: 14px;
            color: #4a90e2;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        main {
            padding: 40px 20px;
            max-width: 800px;
            margin: auto;
        }

        section#logs {
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 30px;
            margin-bottom: 20px;
        }

        .log-entry {
            border-bottom: 1px solid #333;
            padding: 20px 0;
            transition: background-color 0.3s;
        }

        .log-entry:hover {
            background-color: #1a1a1a;
        }

        .log-entry:last-child {
            border-bottom: none;
        }

        .buttons {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
        }

        .btn {
            background-color: rgba(255, 255, 255, 0.1);
            color: #4a90e2;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s, transform 0.2s, color 0.3s;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        .btn:hover {
            background-color: #4a90e2;
            color: white;
            transform: translateY(-2px);
        }

        .btn:active {
            background-color: #357ab7;
            transform: translateY(0);
        }

        footer {
            text-align: center;
            padding: 20px 0;
            background-color: rgba(255, 255, 255, 0.1);
            color: #4a90e2;
            position: fixed;
            bottom: 0;
            width: 100%;
            box-shadow: 0 -2px 4px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body>
    <canvas id="backgroundCanvas"></canvas>
    <header>
        <h1>Diamond Hood Update Logs</h1>
    </header>
    <div id="viewCounter">Views: <span id="viewCount">0</span></div>
    <main>
        <section id="logs">
            <!-- Update logs will be dynamically added here -->
        </section>
        <div class="buttons">
            <button id="prevBtn" class="btn">Previous</button>
            <button id="nextBtn" class="btn">Next</button>
            <button id="addLogBtn" class="btn">Add New Log</button>
        </div>
    </main>
    <footer>
        <p>&copy; 2024 Update Diamond Hood Logs Inc. All rights reserved.</p>
    </footer>
    <script>
        const canvas = document.getElementById('backgroundCanvas');
        const ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        let dots = [];

        const initDots = (numDots) => {
            dots = [];
            for (let i = 0; i < numDots; i++) {
                dots.push({
                    x: Math.random() * canvas.width,
                    y: Math.random() * canvas.height,
                    radius: 2,
                    vx: Math.random() * 0.5 - 0.25,
                    vy: Math.random() * 0.5 - 0.25
                });
            }
        };

        const drawDots = () => {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            dots.forEach(dot => {
                ctx.beginPath();
                ctx.arc(dot.x, dot.y, dot.radius, 0, Math.PI * 2);
                ctx.fillStyle = '#4a90e2';
                ctx.fill();
            });
        };

        const updateDots = () => {
            dots.forEach(dot => {
                dot.x += dot.vx;
                dot.y += dot.vy;

                if (dot.x < 0 || dot.x > canvas.width) dot.vx *= -1;
                if (dot.y < 0 || dot.y > canvas.height) dot.vy *= -1;
            });
        };

        const animate = () => {
            drawDots();
            updateDots();
            requestAnimationFrame(animate);
        };

        initDots(100);
        animate();

        // View Counter Logic
        const updateViewCount = () => {
            const viewCountElement = document.getElementById('viewCount');
            let viewCount = parseInt(localStorage.getItem('viewCount') || '0', 10);
            viewCount++;
            localStorage.setItem('viewCount', viewCount);
            viewCountElement.textContent = viewCount;
        };

        updateViewCount();

        document.addEventListener('DOMContentLoaded', () => {
            const logs = [
                { date: '7/28/2024', description: '[ADDED] Anti Cheat' },
                { date: '7/28/2024', description: '[ADDED] Ping Reducer' },
                { date: '7/28/2024', description: '[ADDED] Fps Booster/Mem Reducer' },
                { date: '7/28/2024', description: '[UPDATE] ZOMBIE GAMEMODE' },
                { date: '7/28/2024', description: '[FIXED] Battle Royal' },
                { date: '7/28/2024', description: '[FIXED] Slide System' },
            ];

            const logsPerPage = 8;
            let currentPage = 0;
            const logsSection = document.getElementById('logs');

            const renderLogs = (page) => {
                logsSection.innerHTML = '';
                const start = page * logsPerPage;
                const end = start + logsPerPage;
                const logsToDisplay = logs.slice(start, end);
                logsToDisplay.forEach(log => {
                    const logEntry = document.createElement('div');
                    logEntry.className = 'log-entry';
                    logEntry.innerHTML = `<strong>${log.date}</strong>: ${log.description}`;
                    logsSection.appendChild(logEntry);
                });
            };

            const updateButtons = () => {
                document.getElementById('prevBtn').disabled = currentPage === 0;
                document.getElementById('nextBtn').disabled = currentPage >= Math.ceil(logs.length / logsPerPage) - 1;
            };

            document.getElementById('prevBtn').addEventListener('click', () => {
                if (currentPage > 0) {
                    currentPage--;
                    renderLogs(currentPage);
                    updateButtons();
                }
            });

            document.getElementById('nextBtn').addEventListener('click', () => {
                if (currentPage < Math.ceil(logs.length / logsPerPage) - 1) {
                    currentPage++;
                    renderLogs(currentPage);
                    updateButtons();
                }
            });

            document.getElementById('addLogBtn').addEventListener('click', () => {
                const key = prompt("Enter the key to add a new log:");
                const correctKey = "spookynchromacodedsnarppredsnipitccnrunnaserver234187532198356412";
                if (key === correctKey) {
                    const newDate = prompt("Enter the date for the new log (YYYY-MM-DD):");
                    const newDescription = prompt("Enter the description for the new log:");
                    if (newDate && newDescription) {
                        logs.push({ date: newDate, description: newDescription });
                        currentPage = Math.floor((logs.length - 1) / logsPerPage);
                        renderLogs(currentPage);
                        updateButtons();
                    }
                } else {
                    alert("Incorrect key. You cannot add a new log.");
                }
            });

            renderLogs(currentPage);
            updateButtons();
        });
    </script>
</body>
</html>
