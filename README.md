# chess-widget
from http.server import BaseHTTPRequestHandler
import urllib.request
import json

class handler(BaseHTTPRequestHandler):
    def do_GET(self):
        # 1. Fetch live club members from Chess.com API
        url = "https://api.chess.com/pub/club/the-great-elo-climbers/members"
        req = urllib.request.Request(url, headers={'User-Agent': 'ChessClubWidgetBot/1.0'})
        
        try:
            with urllib.request.urlopen(req) as response:
                data = json.loads(response.read().decode())
                
            # Grab the 3 newest weekly members
            new_members = data.get('weekly', [])[:3]
            
            # Fill in placeholders if there aren't 3 new members yet
            while len(new_members) < 3:
                new_members.append({'username': 'Searching...'})
        except Exception:
            new_members = [{'username': 'Error loading'} for _ in range(3)]

        # 2. Build the live SVG Graphic
        svg = f"""<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 240" width="100%">
            <rect width="100%" height="100%" rx="8" fill="#1a1a1a" stroke="#d4af37" stroke-width="2"/>
            
            <text x="20" y="40" font-family="Arial, sans-serif" font-size="16" font-weight="bold" fill="#d4af37">🌿 NEW ARRIVALS</text>
            <line x1="20" y1="55" x2="380" y2="55" stroke="#333333" stroke-width="1"/>
            
            <g transform="translate(20, 70)">
                <rect width="360" height="42" rx="6" fill="#121212" stroke="#333333"/>
                <text x="15" y="26" font-family="Arial, sans-serif" font-size="14" font-weight="bold" fill="#ffe680">♟ @{new_members[0]['username']}</text>
            </g>
            
            <g transform="translate(20, 122)">
                <rect width="360" height="42" rx="6" fill="#121212" stroke="#333333"/>
                <text x="15" y="26" font-family="Arial, sans-serif" font-size="14" font-weight="bold" fill="#ffe680">♟ @{new_members[1]['username']}</text>
            </g>
            
            <g transform="translate(20, 174)">
                <rect width="360" height="42" rx="6" fill="#121212" stroke="#333333"/>
                <text x="15" y="26" font-family="Arial, sans-serif" font-size="14" font-weight="bold" fill="#ffe680">♟ @{new_members[2]['username']}</text>
            </g>
        </svg>"""

        # 3. Send the image response
        self.send_response(200)
        self.send_header('Content-type', 'image/svg+xml')
        # Prevent Chess.com from caching it forever
        self.send_header('Cache-Control', 'no-cache, no-store, must-revalidate')
        self.end_headers()
        self.wfile.write(svg.encode('utf-8'))
        return
