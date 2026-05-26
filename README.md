#chess_widget
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Great ELO Climbers - Official Dashboard</title>
    <style>
        /* Your signature Black and Gold Theme */
        body {
            background-color: #000000;
            color: #ffffff;
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
        }
        .container {
            max-width: 600px;
            width: 100%;
        }
        .header {
            background-color: #121212;
            border: 2px solid #d4af37;
            padding: 20px;
            text-align: center;
            border-radius: 8px;
            margin-bottom: 15px;
        }
        .header h1 {
            color: #d4af37;
            margin: 0;
            font-size: 24px;
            letter-spacing: 1px;
        }
        .box {
            background-color: #1a1a1a;
            border: 2px solid #d4af37;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 15px;
            text-align: center;
        }
        .box h3 {
            color: #d4af37;
            margin-top: 0;
            font-size: 16px;
            border-bottom: 1px solid #333;
            padding-bottom: 8px;
        }
        /* Dynamic Progress Bar Styles */
        .progress-container {
            background-color: #111111;
            border: 1px solid #333;
            border-radius: 20px;
            height: 24px;
            width: 100%;
            overflow: hidden;
            margin: 15px 0;
        }
        .progress-bar {
            background: linear-gradient(90deg, #aa8010 0%, #d4af37 50%, #f3e5ab 100%);
            height: 100%;
            width: 0%; /* Starts at 0% and animates via JS */
            border-radius: 20px;
            transition: width 1s ease-in-out;
        }
        .stat-text {
            font-size: 16px;
            color: #cccccc;
        }
        .highlight {
            color: #ffe680;
            font-weight: bold;
        }
        .loading {
            color: #888888;
            font-style: italic;
        }
    </style>
</head>
<body>

<div class="container">

    <!-- HEADER BANNER -->
    <div class="header">
        <h1>THE GREAT ELO CLIMBERS</h1>
        <p style="color: #aaaaaa; font-size: 12px; margin: 5px 0 0 0; font-style: italic;">"Climbing the ladders, mastering the board."</p>
    </div>

    <!-- AUTOMATIC LIVE STATS BOX -->
    <div class="box">
        <h3>📈 LIVE AUTOMATED STATS</h3>
        <div id="stats-loading" class="loading">Fetching live club data from Chess.com...</div>
        
        <div id="stats-display" style="display: none;">
            <p class="stat-text">👥 Current Members: <span id="member-count" class="highlight">--</span></p>
            
            <div class="progress-container">
                <div id="progress-bar" class="progress-bar"></div>
            </div>
            
            <p class="stat-text">🎯 Next Objective: <span id="tier-status" class="highlight">--</span></p>
        </div>
    </div>

    <!-- BACK TO CHESS.COM BUTTON -->
    <div style="text-align: center; margin-top: 20px;">
        <a href="https://www.chess.com/club/the-great-elo-climbers" style="color: #ffe680; text-decoration: none; font-size: 14px; font-weight: bold; border-bottom: 1px solid #ffe680;">← Back to Main Chess.com Club Page</a>
    </div>

</div>

<!-- THE JAVASCRIPT AUTOMATION SCRIPT -->
<script>
    // This function automatically calls the public Chess.com API
    async function fetchClubStats() {
        try {
            // Chess.com public API URL for your club
            const response = await fetch('https://api.chess.com/pub/club/the-great-elo-climbers');
            
            if (!response.ok) {
                throw new Error('Network response was not ok');
            }
            
            const data = await response.json();
            
            // 1. Get the real-time member count from the API data
            const members = data.members_count; 
            
            // 2. Set up a target goal (Silver Tier = 50 members)
            const targetGoal = 50;
            const progressPercent = Math.min((members / targetGoal) * 100, 100);
            
            // 3. Update the HTML elements automatically
            document.getElementById('member-count').innerText = members;
            document.getElementById('progress-bar').style.width = progressPercent + '%';
            
            if (members >= targetGoal) {
                document.getElementById('tier-status').innerText = "Silver Tier Achieved! Next up: 100!";
            } else {
                const remaining = targetGoal - members;
                document.getElementById('tier-status').innerText = remaining + " more members until Silver Tier!";
            }
            
            // 4. Hide the loading text and show the live data
            document.getElementById('stats-loading').style.display = 'none';
            document.getElementById('stats-display').style.display = 'block';
            
        } catch (error) {
            console.error('Error fetching data:', error);
            document.getElementById('stats-loading').innerText = "Failed to load live stats. Please refresh the page.";
        }
    }

    // Run the automatic fetch code as soon as the webpage opens
    fetchClubStats();
</script>

</body>
</html>
