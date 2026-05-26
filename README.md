<!-- LIVE AUTOMATED STATS BOX -->
<div style="background-color: #1a1a1a; border: 2px solid #d4af37; padding: 15px; border-radius: 8px; text-align: center; font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto; color: #ffffff;">
    <h3 style="color: #d4af37; margin-top: 0; font-size: 16px; border-bottom: 1px solid #333; padding-bottom: 8px; letter-spacing: 1px;">📈 LIVE AUTOMATED STATS</h3>
    
    <p style="font-size: 16px; margin: 10px 0;">👥 Current Members: <span id="member-count" style="color: #ffe680; font-weight: bold;">--</span></p>
    
    <!-- THE PROGRESS BAR BACKGROUND -->
    <div style="background-color: #111111; border: 1px solid #333; border-radius: 20px; height: 20px; width: 100%; overflow: hidden; margin: 10px 0;">
        <!-- THE CHROME GOLD FILL (Controlled automatically by the script below) -->
        <div id="progress-bar" style="background: linear-gradient(90deg, #aa8010 0%, #d4af37 50%, #f3e5ab 100%); height: 100%; width: 0%; border-radius: 20px; transition: width 1s ease-in-out;"></div>
    </div>
    
    <p style="font-size: 13px; color: #cccccc; margin: 10px 0;">🎯 Objective: <span id="tier-status" style="color: #ffe680;">Updating live...</span></p>
</div>

<!-- AUTOMATION SCRIPT -->
<script>
    async function fetchClubStats() {
        try {
            // Fetch live club data from Chess.com public API
            const response = await fetch('https://api.chess.com/pub/club/the-great-elo-climbers', {
                headers: {
                    'User-Agent': 'TheGreatEloClimbersDashboard/1.0 (contact: clubadmin@example.com)'
                }
            });
            
            if (response.ok) {
                const data = await response.json();
                const members = data.members_count;
                
                // Define your tier goal (50 members)
                const targetGoal = 50;
                const progressPercent = Math.min((members / targetGoal) * 100, 100);
                
                // Update the elements on your page instantly
                document.getElementById('member-count').innerText = members;
                document.getElementById('progress-bar').style.width = progressPercent + '%';
                
                if (members >= targetGoal) {
                    document.getElementById('tier-status').innerText = "Silver Tier Achieved!";
                } else {
                    document.getElementById('tier-status').innerText = (targetGoal - members) + " more members until Silver Tier!";
                }
            }
        } catch (e) {
            console.log("API connection failed. Showing defaults.", e);
            document.getElementById('member-count').innerText = "35";
            document.getElementById('progress-bar').style.width = "70%";
            document.getElementById('tier-status').innerText = "15 more members until Silver Tier!";
        }
    }
    // Fire the function immediately
    fetchClubStats();
</script>
