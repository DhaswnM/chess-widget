<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Great ELO Climbers - Live Members</title>
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
        .box-6 {
            background-color: #1a1a1a;
            border: 2px solid #d4af37;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(212, 175, 55, 0.1);
        }
        .box-6 h3 {
            color: #d4af37;
            margin-top: 0;
            font-size: 18px;
            border-bottom: 1px solid #333;
            padding-bottom: 10px;
            letter-spacing: 1px;
            text-align: center;
            font-weight: bold;
        }
        .subtitle {
            color: #aaaaaa;
            font-size: 12px;
            text-align: center;
            margin: -5px 0 20px 0;
            font-style: italic;
        }
        .member-card {
            margin: 12px 0;
            padding: 14px;
            background-color: #121212;
            border: 1px solid #333;
            border-left: 4px solid #d4af37;
            border-radius: 6px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .username-side {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .chess-icon {
            color: #d4af37;
            font-size: 16px;
        }
        .profile-link {
            color: #ffe680;
            font-weight: bold;
            font-size: 15px;
            text-decoration: none;
        }
        .time-badge {
            color: #ffffff;
            font-weight: bold;
            font-size: 11px;
            background-color: #2b2b2b;
            padding: 4px 8px;
            border-radius: 4px;
            border: 1px solid #444;
        }
        .status-msg {
            text-align: center;
            padding: 30px;
            color: #888888;
            font-style: italic;
            font-size: 14px;
        }
    </style>
</head>
<body>

<div class="container">

    <div class="box-6">
        <h3>🌿 NEWEST CLUB ARRIVALS</h3>
        <p class="subtitle">Live Global Feed • Automatically updated via Chess.com APIs</p>
        
        <div id="live-feed-container">
            <div id="loading-state" class="status-msg">
                🔍 Connecting to Chess.com data streams...
            </div>
        </div>
    </div>

</div>

<script>
    async function fetchNewestMembers() {
        const feedContainer = document.getElementById('live-feed-container');
        
        try {
            // Target url
            const targetUrl = 'https://api.chess.com/pub/club/the-great-elo-climbers/members';
            
            // Bypassing browser CORS completely using a direct layout bypass proxy
            const response = await fetch('https://corsproxy.io/?url=' + encodeURIComponent(targetUrl));
            
            if (!response.ok) throw new Error("Proxy connection dropped");
            
            const payload = await response.json();
            
            // Gather all entries from all-time lists
            let allClubMembers = [];
            if (payload.weekly) allClubMembers = allClubMembers.concat(payload.weekly);
            if (payload.monthly) allClubMembers = allClubMembers.concat(payload.monthly);
            if (payload.all_time) allClubMembers = allClubMembers.concat(payload.all_time);
            
            // Remove duplicates
            const tracker = new Set();
            const uniqueMembers = allClubMembers.filter(m => {
                if (!m.username || tracker.has(m.username)) return false;
                tracker.add(m.username);
                return true;
            });
            
            // Sort by UNIX timestamp (Newest members first)
            uniqueMembers.sort((first, second) => second.joined - first.joined);
            
            // Slice top 5 newest
            const topFreshArrivals = uniqueMembers.slice(0, 5);
            
            feedContainer.innerHTML = ''; // Wipe out loading message
            
            topFreshArrivals.forEach(member => {
                const joinDate = new Date(member.joined * 1000);
                const readableDate = joinDate.toLocaleDateString('en-US', {
                    month: 'short',
                    day: 'numeric',
                    year: 'numeric'
                });
                
                const card = document.createElement('div');
                card.className = 'member-card';
                card.innerHTML = `
                    <div class="username-side">
                        <span class="chess-icon">♟</span>
                        <a href="https://www.chess.com/member/${member.username}" target="_blank" class="profile-link">@${member.username}</a>
                    </div>
                    <div class="time-badge">Joined: ${readableDate}</div>
                `;
                feedContainer.appendChild(card);
            });
            
        } catch (error) {
            console.error(error);
            feedContainer.innerHTML = '<div class="status-msg" style="color: #ffaa00;">⚠️ Temporary API lag. Please refresh the page in a few seconds!</div>';
        }
    }
    
    fetchNewestMembers();
</script>

</body>
</html>
