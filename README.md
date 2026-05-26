<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Great ELO Climbers - Official Dashboard</title>
    <style>
        body {
            background-color: #000000;
            color: #ffffff;
            font-family: Arial, sans-serif;
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
            padding: 15px;
            text-align: center;
            border-radius: 8px;
            margin-bottom: 15px;
        }
        .header h1 {
            color: #d4af37;
            margin: 0;
            font-size: 22px;
            letter-spacing: 1px;
        }
        .box {
            background-color: #1a1a1a;
            border: 2px solid #d4af37;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 15px;
        }
        .box h3 {
            color: #d4af37;
            margin-top: 0;
            font-size: 16px;
            border-bottom: 1px solid #333;
            padding-bottom: 8px;
        }
        .nav-btn {
            display: block;
            padding: 12px;
            text-decoration: none;
            font-weight: bold;
            border-radius: 6px;
            font-size: 13px;
            margin: 12px 0;
            text-align: center;
            letter-spacing: 0.5px;
        }
        .progress-container {
            background-color: #111111;
            border: 1px solid #333;
            border-radius: 20px;
            height: 20px;
            width: 100%;
            overflow: hidden;
            margin: 10px 0;
        }
        .progress-bar {
            background: linear-gradient(90deg, #aa8010 0%, #d4af37 50%, #f3e5ab 100%);
            height: 100%;
            width: 70%; /* Default 70% matching ~35 members */
            border-radius: 20px;
            transition: width 1s ease-in-out;
        }
        .player-row {
            margin: 10px 0;
            padding: 12px;
            background-color: #121212;
            border: 1px solid #333;
            border-radius: 6px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .peak-badge {
            color: #ffffff;
            font-weight: bold;
            font-size: 13px;
            background-color: #2b2b2b;
            padding: 2px 6px;
            border-radius: 4px;
        }
    </style>
</head>
<body>

<div class="container">

    <!-- TOP HEADER BANNER -->
    <div class="header">
        <h1>THE GREAT ELO CLIMBERS</h1>
        <p style="color: #aaaaaa; font-size: 12px; margin: 5px 0 0 0; font-style: italic;">"Climbing the ladders, mastering the board."</p>
    </div>

    <!-- QUICK NAVIGATOR -->
    <div class="box" style="border-color: #ffffff; text-align: center;">
        <h3 style="color: #ffffff;">👑 CLUB NAVIGATOR</h3>
        <a href="https://www.chess.com/clubs/events/the-great-elo-climbers" class="nav-btn" style="background-color: #4a3e0b; color: #ffe680; border: 2px solid #d4af37;">🏆 LIVE TOURNAMENT</a>
        <a href="https://www.chess.com/clubs/forum/the-great-elo-climbers" class="nav-btn" style="background-color: #0f2a4a; color: #99c2ff; border: 2px solid #3385ff;">💬 CLUB FORUMS</a>
        <a href="https://www.chess.com/club/the-great-elo-climbers/announcements" class="nav-btn" style="background-color: #4a2305; color: #ffb380; border: 2px solid #ff771c;">📢 ANNOUNCEMENTS</a>
        <a href="https://www.chess.com/leaderboard/daily?group=930017" class="nav-btn" style="background-color: #2d0f4a; color: #e1b3ff; border: 2px solid #a64dff;">🏆 LEADERBOARD</a>
    </div>

    <!-- LIVE AUTOMATED STATS BOX -->
    <div class="box" style="text-align: center;">
        <h3>📈 LIVE AUTOMATED STATS</h3>
        <p style="font-size: 16px; margin: 10px 0;">👥 Current Members: <span id="member-count" style="color: #ffe680; font-weight: bold;">35</span></p>
        
        <div class="progress-container">
            <div id="progress-bar" class="progress-bar"></div>
        </div>
        
        <p style="font-size: 13px; color: #cccccc; margin: 10px 0;">🎯 Objective: <span id="tier-status" style="color: #ffe680;">15 more members until Silver Tier!</span></p>
    </div>

    <!-- MISSION -->
    <div class="box">
        <h3>⚔️ Our Mission</h3>
        <p style="color: #cccccc; font-size: 13px; line-height: 1.5; margin: 0;">Welcome to the ultimate home for tactical improvement. We analyze openings, share gambits, and help every single member push their Elo higher. From beginners to masters, we climb together.</p>
    </div>

    <!-- HALL OF FAME -->
    <div class="box">
        <h3>👑 CLUB TITLED PLAYERS & TOP RATINGS</h3>
        <div class="player-row"><span style="color: #ffe680; font-weight: bold;">♟ @hridansh2022</span> <span class="peak-badge">Peak: 1578</span></div>
        <div class="player-row"><span style="color: #ffe680; font-weight: bold;">♟ @hmayer11</span> <span class="peak-badge">Peak: 612</span></div>
    </div>

    <!-- FOOTER -->
    <div style="text-align: center; font-size: 11px; color: #666666; margin-top: 20px;">
        <a href="https://www.chess.com/club/the-great-elo-climbers" id="back-link" style="color: #666666; text-decoration: none; font-style: italic;">Super Admin: Dhaswin Murali — Go back to Chess.com</a>
    </div>

</div>

<!-- BULLETPROOF API AUTOMATION -->
<script>
    async function fetchClubStats() {
        try {
            // Added required custom headers to satisfy Chess.com's PubAPI requirements
            const response = await fetch('https://api.chess.com/pub/club/the-great-elo-climbers', {
                headers: {
                    'User-Agent': 'TheGreatEloClimbersDashboard/1.0 (contact: clubadmin@example.com)'
                }
            });
            
            if (response.ok) {
                const data = await response.json();
                const members = data.members_count;
                
                // Targets and layout generation
                const targetGoal = 50;
                const progressPercent = Math.min((members / targetGoal) * 100, 100);
                
                document.getElementById('member-count').innerText = members;
                document.getElementById('progress-bar').style.width = progressPercent + '%';
                
                if (members >= targetGoal) {
                    document.getElementById('tier-status').innerText = "Silver Tier Achieved! Next milestone: 100.";
                } else {
                    document.getElementById('tier-status').innerText = (targetGoal - members) + " more members until Silver Tier!";
                }
            }
        } catch (e) {
            console.log("Using standard visual fallbacks due to connection block.", e);
        }
    }
    // Fire immediately when loading the layout
    fetchClubStats();
</script>

</body>
</html>
