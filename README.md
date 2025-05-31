<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Travel Feedback Bottom Sheet</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background: #000;
            min-height: 100vh;
            position: relative;
            overflow-x: hidden;
        }

        /* Home Screen Mockup */
        .home-screen {
            width: 100%;
            max-width: 390px;
            margin: 0 auto;
            background: #f5f5f5;
            min-height: 100vh;
            position: relative;
        }

        .status-bar {
            background: #f8f8f8;
            padding: 12px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 14px;
            font-weight: 500;
        }

        .trip-header {
            background: white;
            padding: 16px;
            display: flex;
            align-items: center;
            gap: 12px;
            border-bottom: 1px solid #eee;
        }

        .trip-info {
            flex: 1;
        }

        .trip-title {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 4px;
        }

        .trip-dates {
            font-size: 14px;
            color: #666;
        }

        .event-banner {
            position: relative;
            margin: 16px;
            border-radius: 16px;
            overflow: hidden;
            height: 200px;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 200"><rect fill="%2387CEEB" width="400" height="200"/><path fill="%23FFD700" d="M350 100 L370 120 L350 140 L330 120 Z"/><rect fill="%23F0F0F0" x="50" y="140" width="100" height="60"/><rect fill="%23F0F0F0" x="180" y="120" width="80" height="80"/></svg>') center/cover;
        }

        .current-event-badge {
            position: absolute;
            top: 16px;
            left: 16px;
            background: #FFE4B5;
            color: #8B4513;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }

        .event-info {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: linear-gradient(to top, rgba(0,0,0,0.8), transparent);
            padding: 20px;
            color: white;
        }

        .event-title {
            font-size: 20px;
            font-weight: 600;
            margin-bottom: 4px;
        }

        .event-time {
            font-size: 14px;
            opacity: 0.9;
        }

        /* Overlay */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.5);
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
            z-index: 998;
        }

        .overlay.active {
            opacity: 1;
            visibility: visible;
        }

        /* Bottom Sheet */
        .bottom-sheet {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            border-radius: 24px 24px 0 0;
            padding: 8px 0 32px;
            transform: translateY(100%);
            transition: transform 0.3s ease;
            z-index: 999;
            max-width: 390px;
            margin: 0 auto;
            box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.1);
        }

        .bottom-sheet.active {
            transform: translateY(0);
        }

        .sheet-handle {
            width: 36px;
            height: 4px;
            background: #ddd;
            border-radius: 2px;
            margin: 8px auto 20px;
        }

        .sheet-content {
            padding: 0 24px;
        }

        .sheet-header {
            text-align: center;
            margin-bottom: 24px;
        }

        .sheet-title {
            font-size: 22px;
            font-weight: 700;
            margin-bottom: 8px;
            color: #333;
        }

        .sheet-subtitle {
            font-size: 16px;
            color: #666;
        }

        /* Emoji Rating */
        .emoji-rating {
            display: flex;
            justify-content: center;
            gap: 16px;
            margin-bottom: 24px;
        }

        .emoji-option {
            width: 56px;
            height: 56px;
            font-size: 36px;
            background: #f8f8f8;
            border: 2px solid transparent;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .emoji-option:hover {
            transform: scale(1.1);
        }

        .emoji-option.selected {
            background: #E8F5E9;
            border-color: #4CAF50;
        }

        /* Agent Info */
        .agent-info {
            background: #f8f8f8;
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .agent-avatar {
            width: 48px;
            height: 48px;
            background: #4CAF50;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: 600;
            font-size: 18px;
        }

        .agent-details {
            flex: 1;
        }

        .agent-name {
            font-size: 16px;
            font-weight: 600;
            color: #333;
            margin-bottom: 2px;
        }

        .agent-company {
            font-size: 14px;
            color: #666;
        }

        /* Google Review Button */
        .google-review-btn {
            width: 100%;
            background: #4285F4;
            color: white;
            border: none;
            border-radius: 12px;
            padding: 16px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            transition: all 0.2s ease;
            margin-bottom: 12px;
        }

        .google-review-btn:hover {
            background: #3367D6;
            transform: translateY(-1px);
        }

        .google-icon {
            width: 20px;
            height: 20px;
        }

        .skip-btn {
            width: 100%;
            background: transparent;
            color: #666;
            border: none;
            padding: 12px;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .skip-btn:hover {
            color: #333;
        }

        /* Success State */
        .success-content {
            display: none;
            text-align: center;
            padding: 20px 0;
        }

        .success-icon {
            width: 64px;
            height: 64px;
            background: #4CAF50;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 16px;
            font-size: 32px;
        }

        .success-message {
            font-size: 18px;
            font-weight: 600;
            color: #333;
            margin-bottom: 8px;
        }

        .success-submessage {
            font-size: 14px;
            color: #666;
        }

        /* Demo Button */
        .demo-trigger {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 50%;
            width: 56px;
            height: 56px;
            font-size: 24px;
            cursor: pointer;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            z-index: 997;
        }
    </style>
</head>
<body>
    <!-- Home Screen Mockup -->
    <div class="home-screen">
        <div class="status-bar">
            <span>9:41</span>
            <span>📶 📶 🔋</span>
        </div>
        
        <div class="trip-header">
            <div class="trip-info">
                <div class="trip-title">Nitin's Dubai Trip</div>
                <div class="trip-dates">Sep 23 - Oct 6 • 2 Adults</div>
            </div>
        </div>
        
        <div class="event-banner">
            <div class="current-event-badge">Current event</div>
            <div class="event-info">
                <div class="event-title">Visit to Burj Al Arab</div>
                <div class="event-time">10:00 AM - 12:00 PM (4 Hours)</div>
            </div>
        </div>
    </div>

    <!-- Overlay -->
    <div class="overlay" id="overlay"></div>

    <!-- Bottom Sheet -->
    <div class="bottom-sheet" id="bottomSheet">
        <div class="sheet-handle"></div>
        
        <div class="sheet-content" id="ratingContent">
            <div class="sheet-header">
                <h2 class="sheet-title">Enjoying your trip?</h2>
                <p class="sheet-subtitle">How's everything going so far?</p>
            </div>
            
            <div class="emoji-rating">
                <button class="emoji-option" data-rating="1">😞</button>
                <button class="emoji-option" data-rating="2">😐</button>
                <button class="emoji-option" data-rating="3">🙂</button>
                <button class="emoji-option" data-rating="4">😊</button>
                <button class="emoji-option" data-rating="5">🤩</button>
            </div>
            
            <div class="agent-info">
                <div class="agent-avatar">RA</div>
                <div class="agent-details">
                    <div class="agent-name">Raj Agarwal</div>
                    <div class="agent-company">Dream Travels Agency</div>
                </div>
            </div>
            
            <button class="google-review-btn" id="googleReviewBtn" style="display: none;">
                <svg class="google-icon" viewBox="0 0 24 24">
                    <path fill="#4285F4" d="M12.48 10.92v3.28h7.84c-.24 1.84-.853 3.187-1.787 4.133-1.147 1.147-2.933 2.4-6.053 2.4-4.827 0-8.6-3.893-8.6-8.72s3.773-8.72 8.6-8.72c2.6 0 4.507 1.027 5.907 2.347l2.307-2.307C18.747 1.44 16.133 0 12.48 0 5.867 0 .307 5.387.307 12s5.56 12 12.173 12c3.573 0 6.267-1.173 8.373-3.36 2.16-2.16 2.84-5.213 2.84-7.667 0-.76-.053-1.467-.173-2.053H12.48z"/>
                    <path fill="#34A853" d="M5.58 14.82l-1.56 1.2C2.7 14.34 1.9 12.26 1.9 10s.8-4.34 2.12-6.02l1.56 1.2C4.6 6.62 4 8.23 4 10s.6 3.38 1.58 4.82z"/>
                    <path fill="#FBBC05" d="M12 6c1.66 0 3.13.57 4.3 1.7l2.3-2.3C16.96 3.98 14.73 3 12 3 8.47 3 5.43 5.02 4.02 7.82l1.56 1.2C6.48 7.18 9.04 6 12 6z"/>
                    <path fill="#EA4335" d="M12 18c-2.96 0-5.52-1.18-6.42-3.02l-1.56 1.2C5.43 18.98 8.47 21 12 21c2.7 0 4.96-.9 6.62-2.42l-1.44-1.18C15.99 18.37 14.14 18 12 18z"/>
                </svg>
                Share your experience on Google
            </button>
            
            <button class="skip-btn" id="skipBtn">Maybe later</button>
        </div>
        
        <div class="success-content" id="successContent">
            <div class="success-icon">✓</div>
            <div class="success-message">Thank you!</div>
            <div class="success-submessage">Your feedback helps us improve</div>
        </div>
    </div>

    <!-- Demo Trigger Button -->
    <button class="demo-trigger" onclick="showBottomSheet()">👆</button>

    <script>
        let selectedRating = 0;
        
        function showBottomSheet() {
            document.getElementById('overlay').classList.add('active');
            document.getElementById('bottomSheet').classList.add('active');
        }
        
        function hideBottomSheet() {
            document.getElementById('overlay').classList.remove('active');
            document.getElementById('bottomSheet').classList.remove('active');
            
            // Reset after animation
            setTimeout(() => {
                document.getElementById('ratingContent').style.display = 'block';
                document.getElementById('successContent').style.display = 'none';
                document.querySelectorAll('.emoji-option').forEach(emoji => {
                    emoji.classList.remove('selected');
                });
                document.getElementById('googleReviewBtn').style.display = 'none';
                selectedRating = 0;
            }, 300);
        }
        
        // Handle emoji selection
        document.querySelectorAll('.emoji-option').forEach(emoji => {
            emoji.addEventListener('click', function() {
                // Remove previous selection
                document.querySelectorAll('.emoji-option').forEach(e => {
                    e.classList.remove('selected');
                });
                
                // Add selection to clicked emoji
                this.classList.add('selected');
                selectedRating = parseInt(this.dataset.rating);
                
                // Show Google review button for ratings 4 and 5
                if (selectedRating >= 4) {
                    document.getElementById('googleReviewBtn').style.display = 'flex';
                } else {
                    document.getElementById('googleReviewBtn').style.display = 'none';
                }
            });
        });
        
        // Handle Google review button
        document.getElementById('googleReviewBtn').addEventListener('click', function() {
            // Show success state
            document.getElementById('ratingContent').style.display = 'none';
            document.getElementById('successContent').style.display = 'block';
            
            // In real implementation, this would open Google Reviews
            // window.open('https://g.page/r/YOUR_GOOGLE_BUSINESS_ID/review', '_blank');
            
            // Auto close after 2 seconds
            setTimeout(hideBottomSheet, 2000);
        });
        
        // Handle skip button
        document.getElementById('skipBtn').addEventListener('click', hideBottomSheet);
        
        // Handle overlay click
        document.getElementById('overlay').addEventListener('click', hideBottomSheet);
        
        // Auto-show after 3 seconds (simulating during trip trigger)
        setTimeout(showBottomSheet, 3000);
    </script>
</body>
</html>
