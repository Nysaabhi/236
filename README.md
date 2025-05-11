<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Nysa Chatbot</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
    <style>
:root { 
    --primary-color: #FFD700; 
    --secondary-color: #FDB931; 
    --background-dark: #1a1a1f; 
    --text-light: #ffffff; 
    --text-dark: #000000; 
    --accent-gradient: linear-gradient(135deg, var(--primary-color), var(--secondary-color)); 
    --error-color: #ff4444; 
    --success-color: #00C851; 
}

* { 
    box-sizing: border-box; 
    margin: 0; 
    padding: 0; 
}

body { 
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif; 
    background-color: var(--background-dark); 
    color: var(--text-light); 
    line-height: 1.6; 
    overflow-x: hidden; 
}

.header { 
    padding: 8px 16px; 
    background: rgba(26, 26, 31, 0.95); 
    position: fixed; 
    width: 100%; 
    top: 0; 
    left: 0; 
    z-index: 30; 
    border-bottom: 2px solid var(--primary-color); 
    backdrop-filter: blur(10px); 
    height: 60px; 
}

.header-content { 
    max-width: 1400px; 
    margin: 0 auto; 
    display: flex; 
    justify-content: space-between; 
    align-items: center; 
    height: 100%; 
}

.logo { 
    font-size: 1.8em; 
    font-weight: 900; 
    background: var(--accent-gradient); 
    -webkit-background-clip: text; 
    color: transparent; 
    letter-spacing: 1.2px; 
}

.menu-button { 
    padding: 10px 20px; 
    background: var(--accent-gradient); 
    border: none; 
    border-radius: 50px; 
    color: var(--text-dark); 
    font-weight: 600; 
    cursor: pointer; 
    transition: all 0.3s ease; 
    font-size: 0.95em; 
}

.chatbot-container { 
    position: fixed; 
    top: 60px; 
    left: 0; 
    right: 0; 
    bottom: 60px; 
    background: rgba(26, 26, 31, 0.95); 
    display: flex; 
    flex-direction: column; 
}

.chat-header { 
    padding: 10px; 
    background: linear-gradient(135deg, rgba(255, 215, 0, 0.1), rgba(253, 185, 49, 0.1)); 
    border-bottom: 1px solid rgba(255, 215, 0, 0.2); 
    display: flex; 
    justify-content: space-between; 
    align-items: center; 
}

.chat-status { 
    display: flex; 
    align-items: center; 
    color: var(--primary-color); 
    font-weight: 600; 
}

.status-dot { 
    width: 8px; 
    height: 8px; 
    background: var(--primary-color); 
    border-radius: 50%; 
    margin-right: 8px; 
    animation: pulse 2s infinite; 
}

.chat-messages { 
    flex: 1; 
    overflow-y: auto; 
    padding: 20px; 
    scroll-behavior: smooth; 
}

.message {
    font-family: 'Poppins', sans-serif;
    display: flex;
    gap: 10px;
    margin-bottom: 16px;
    animation: messageSlide 0.3s ease-out;
    width: 100%;
    max-width: 100%;
}

/* Bot message styles */
.bot-message {
    align-self: flex-start;
    width: 100%;
    max-width: 100%;
}

.bot-message .message-content {
    background: transparent;
    padding: 16px;
    border-radius: 0;
    color: var(--text-light);
    font-size: 16px;
    line-height: 1.6;
    position: relative;
    width: 100%;
    max-width: 100%;
}

/* User message styles */
.user-message {
    align-self: flex-end;
    flex-direction: row-reverse;
    width: 100%;
    max-width: 100%;
}

.user-message .message-content {
    background: rgba(255, 215, 0, 0.1);
    border-radius: 16px;
    width: auto;
    max-width: 80%;
}


/* Welcome message styles */
.welcome-message {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    margin-bottom: 16px;
    width: 20%;
    max-width: 100%;
}

.welcome-avatar {
  width: 40px;
  height: 40px;
  min-width: 40px;
  min-height: 40px;
  background: rgba(255, 215, 0, 0.1);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--primary-color);
  flex-shrink: 0;
}

.welcome-content {
    background: rgba(255, 255, 255, 0.05);
    padding: 12px 16px;
    border-radius: 16px;
    color: var(--text-light);
    max-width: 100%;
    width: 100%;
}

.welcome-content p {
  margin: 0;
  font-size: 16px;
  line-height: 1.5;
}

/* Animation for welcome message */
.welcome-message {
    animation: messageSlide 0.3s ease-out;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .welcome-message {
        width: 80%;
    }
}

/* Fade out animation */
.welcome-message.fade-out {
    opacity: 0;
    transition: opacity 0.3s ease-out;
}

/* Add markdown styling for bot messages */
.bot-message .message-content pre {
    background: rgba(255, 255, 255, 0.05);
    border-radius: 6px;
    padding: 12px;
    margin: 8px 0;
    overflow-x: auto;
}

.bot-message .message-content code {
    background: rgba(255, 255, 255, 0.1);
    padding: 2px 6px;
    border-radius: 4px;
    font-family: 'Menlo', 'Monaco', 'Courier New', monospace;
}

.bot-message .message-content p {
    margin-bottom: 16px;
}

.bot-message .message-content ul,
.bot-message .message-content ol {
    margin: 16px 0;
    padding-left: 24px;
}

.bot-message .message-content li {
    margin-bottom: 8px;
}

/* Add a subtle separator between messages */
.bot-message:not(:last-child) {
    border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    padding-bottom: 16px;
}

/* Remove any padding that might be causing the issue */
.chat-messages {
    padding: 20px 10px !important;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .bot-message .message-content,
    .user-message .message-content,
    .welcome-content {
        padding: 12px;
    }
    
    .user-message .message-content {
        max-width: 90%;
    }
}

.message-content { 
    background: rgba(255, 255, 255, 0.05); 
    padding: 12px 16px; 
    border-radius: 16px; 
    color: var(--text-light); 
}

.user-message .message-content { 
    background: rgba(255, 215, 0, 0.1); 
}

.chat-input-container {
  padding: 20px;
  background: rgba(26, 26, 31, 0.98);
  border-top: 1px solid rgba(255, 215, 0, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
}

.input-with-voice {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#chatInput {
  width: 100%;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 20px 50px 20px 20px;
  color: var(--text-light);
  font-size: 0.95em;
}

.voice-button {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-button:hover {
  color: var(--secondary-color);
}

.send-message {
  padding: 20px;
  background: var(--accent-gradient);
  border: none;
  border-radius: 8px;
  color: #ffffff;
  cursor: pointer;
  transition: all 0.3s ease;
}

/* Desktop styles */
@media (min-width: 769px) {
  .chat-input-container {
    width: 60%;
    margin-left: auto;
    margin-right: auto;
  }
  
  #chatInput {
    width: 100%;
    height: 70px;
    font-size: 1.05rem;
    padding: 0 70px 0 24px;  /* Increased right padding for larger voice button */
    border-radius: 16px;
  }
  
  .voice-button {
    width: 60px;  /* Increased width */
    height: 60px; /* Increased height */
    right: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .voice-button i {
    font-size: 1.7em;  /* Increased font size */
  }
  
  .send-message {
    padding: 20px;
    height: 70px;
    width: 70px;  /* Made slightly wider */
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .send-message i {
    font-size: 1.7em;  /* Increased font size */
  }
}

/* Mobile styles */
@media (max-width: 768px) {
  .chat-input-container {
    width: 100%;
    padding: 10px;
  }
  
  #chatInput {
    width: 100%;
  }
  
  .voice-button i {
    font-size: 1.5em;
  }
  
  .send-message i {
    font-size: 1.5em;
  }
}

.alert { 
    position: fixed; 
    top: 20px; 
    right: 20px; 
    padding: 15px 20px; 
    border-radius: 8px; 
    color: var(--text-light); 
    z-index: 1000; 
    animation: slideIn 0.3s ease-out; 
}

.alert-success { 
    background: var(--success-color); 
}

.alert-error { 
    background: var(--error-color); 
}

.bottom-nav { 
    position: fixed; 
    bottom: 0; 
    left: 0; 
    right: 0; 
    height: 60px; 
    background: rgba(26, 26, 31, 0.98); 
    padding: 8px 0; 
    border-top: 1px solid rgba(255, 215, 0, 0.2); 
}

.nav-container { 
    display: flex; 
    justify-content: space-around; 
    align-items: center; 
    height: 100%; 
    max-width: 600px; 
    margin: 0 auto; 
}

.nav-item { 
    display: flex; 
    flex-direction: column; 
    align-items: center; 
    text-decoration: none; 
    color: #fff; 
    transition: all 0.3s ease; 
    padding: 5px; 
    cursor: pointer; 
    font-family: 'Poppins', sans-serif; 
}

.nav-item i { 
    color: #ffffff; 
    font-size: 20px; 
    margin-bottom: 4px; 
}

.nav-item span { 
    font-size: 12px; 
    font-family: 'Poppins', sans-serif; 
    font-weight: 600; 
    letter-spacing: 0.6px; 
    text-align: center; 
}

@keyframes pulse { 
    0% { opacity: 1; } 
    50% { opacity: 0.5; } 
    100% { opacity: 1; } 
}

@keyframes messageSlide { 
    from { opacity: 0; transform: translateY(10px); } 
    to { opacity: 1; transform: translateY(0); } 
}

@keyframes slideIn { 
    from { opacity: 0; transform: translateX(100px); } 
    to { opacity: 1; transform: translateX(0); } 
}

@media (max-width: 768px) { 
    .message { 
        max-width: 90%; 
    }
}

body > h1:first-of-type:not(.heading) { 
    display: none !important; 
}

.markdown-body h1:first-child { 
    display: none !important; 
}

.position-relative h1:first-child { 
    display: none !important; 
}
      
      .modal-overlay { 
          position: absolute; 
          top: 0; 
          left: 0; 
          width: 100%; 
          height: 100%; 
          background-color: rgba(0, 0, 0, 0.5); 
          display: flex; 
          justify-content: center; 
          align-items: center; 
          padding: 20px; 
      }
      
      .modal-content { 
          background-color: #1a1a1f; 
          border-radius: 12px; 
          width: 100%; 
          max-width: 600px; 
          max-height: 90vh; 
          overflow-y: auto; 
          box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15); 
      }
      
      .modal-header { 
          padding: 20px; 
          border-bottom: 1px solid #eee; 
          display: flex; 
          justify-content: space-between; 
          align-items: center; 
      }
      
      .modal-header h2 { 
          margin: 0; 
          font-size: 1.5rem; 
          color: #fffdfd; 
      }
            
      .modal-body { 
          padding: 20px; 
      }
      
      .menu-overlay { 
          display: none; 
          position: fixed; 
          top: 60px; 
          left: 0; 
          right: 0; 
          bottom: 60px; 
          background: rgba(26, 26, 31, 0.95); 
          z-index: 25; 
          padding: 0; 
          animation: fadeIn 0.3s ease-out; 
          overflow-y: auto; 
      }
      
      .menu-sections { 
          background: rgba(26, 26, 31, 0.95); 
          padding: 20px; 
      }
      
      .menu-section { 
        background: rgba(26, 26, 31, 0.95); 
          margin-bottom: 24px; 
      }
      
      .menu-section-title { 
          color: var(--primary-color); 
          font-size: 1.2em; 
          font-weight: 600; 
          margin-bottom: 12px; 
          padding-bottom: 8px; 
          border-bottom: 1px solid rgba(255, 215, 0, 0.2); 
      }
      
      .menu-items { 
          display: flex; 
          flex-direction: column; 
          gap: 8px; 
      }
      
      .menu-item { 
          padding: 12px 16px; 
          background: rgba(255, 255, 255, 0.05); 
          border-radius: 8px; 
          color: var(--text-light); 
          cursor: pointer; 
          transition: all 0.3s ease; 
          display: flex; 
          justify-content: space-between; 
          align-items: center; 
      }
      
      .menu-item:hover { 
          background: rgba(255, 215, 0, 0.1); 
      }
      
      .menu-item-price { 
          color: var(--primary-color); 
          font-weight: 600; 
      }
      
      .social-links { 
          display: flex; 
          gap: 12px; 
      }
      
      .social-link { 
          width: 40px; 
          height: 40px; 
          border-radius: 50%; 
          background: rgba(255, 255, 255, 0.05); 
          display: flex; 
          align-items: center; 
          justify-content: center; 
          color: var(--text-light); 
          text-decoration: none; 
          transition: all 0.3s ease; 
      }
      
      .social-link:hover { 
          background: var(--primary-color); 
          color: var(--text-dark); 
      }
      
      .about-content { 
          line-height: 1.6; 
          color: #cccccc; 
      }
      
      .menu-close-button { 
          background: none; 
          border: none; 
          font-size: 1.5rem; 
          cursor: pointer; 
          color: #FFD700; 
          padding: 5px; 
      }

      .menu-item { 
          animation: slideIn 0.3s ease-out; 
          animation-fill-mode: both; 
      }
      
      .menu-item:nth-child(1) { 
          animation-delay: 0.1s; 
      }
      
      .menu-item:nth-child(2) { 
          animation-delay: 0.2s; 
      }
      
      .menu-item:nth-child(3) { 
          animation-delay: 0.3s; 
      }
      
      .menu-item:nth-child(4) { 
          animation-delay: 0.4s; 
      }
      
      .menu-item:nth-child(5) { 
          animation-delay: 0.5s; 
      }
      
      @keyframes slideIn { 
          from { 
              opacity: 0; 
              transform: translateX(-20px); 
          } 
          to { 
              opacity: 1; 
              transform: translateX(0); 
          } 
      }
      
      .deepchat-item { 
          background-color: #ff0000; 
          color: rgb(255, 255, 255); 
          border-radius: 8px; 
          padding: 10px; 
          text-align: center; 
      }
      
      .deepchat-item i { 
          color: rgb(255, 255, 5); 
      }
      
      .deepchat-item:hover { 
          background-color: #ff0606; 
          color: rgb(255, 255, 255); 
      }
      
      .deepchat-item span { 
          font-weight: 600; 
          font-family: 'Poppins', -apple-system, BlinkMacSystemFont, Roboto, sans-serif; 
          letter-spacing: 0.3px; 
      }
      
      .confirmation-icon { 
          font-size: 80px; 
          color: #4CAF50; 
          margin-bottom: 20px; 
      }
      
      .confirmation-icon i { 
          display: block; 
      }
      
      .alert { 
          position: fixed; 
          top: 20px; 
          left: 50%; 
          transform: translateX(-50%); 
          padding: 10px 20px; 
          border-radius: 5px; 
          z-index: 1100; 
      }
      
      .alert-success { 
          background-color: #4CAF50; 
          color: white; 
      }
      
      .alert-error { 
          background-color: #f44336; 
          color: white; 
      }
      
      .location-links, .partner-links { 
          display: flex; 
          flex-direction: column; 
      } 
      
      .location-link, .partner-link { 
          text-decoration: none; 
          color: #ffffff; 
          padding: 8px 10px; 
          transition: all 0.3s ease; 
          border-radius: 4px; 
      } 
      
      .location-link:hover, .partner-link:hover { 
          background-color: #f0f0f0; 
          color: #333; 
      } 
      
      .location-link i, .partner-link i { 
          margin-right: 10px; 
          color: #fff; 
      } 
      
      .location-link:hover i, .partner-link:hover i { 
          color: #2c7cd1; 
      }
      
      .header-button-group { 
          display: flex; 
          align-items: center; 
          gap: 5px; 
      }
      
      .coupon-button, 
      .nysa-feed-button, 
      .jobs-button { 
          display: flex; 
          align-items: center; 
          justify-content: center; 
          padding: 15px; 
          background: none; 
          border: none; 
          color: var(--primary-color); 
          cursor: pointer; 
          transition: background-color 0.3s ease; 
          border-radius: 4px; 
          width: 40px; 
          height: 40px; 
      }
      
      .coupon-button:hover, 
      .nysa-feed-button:hover, 
      .jobs-button:hover { 
          background: rgba(255, 215, 0, 0.1); 
      }
      
      .coupon-button i, 
      .nysa-feed-button i, 
      .jobs-button i { 
          font-size: 18px; 
      }
      
      /* ===== Tablet (768px - 1024px) ===== */
@media (min-width: 768px) and (max-width: 1024px) {
  .welcome-message {
        width: 30%;
    }
}

      </style>        
        </head>
        <body>
            <header class="header">
                <div class="header-content">
                    <a href="https://nysaabhi.github.io/A2" class="logo">Nysa</a>
                    <button class="menu-button" id="menuButton">
                        <i class="fas fa-bars"></i>
                     
                    </button>
            </div>
            </header>
            
            <div class="chatbot-container">
                <div class="chat-header">
                    <div class="chat-status">
                        <span class="status-dot"></span>
                        Nysa AI Assistant
                    </div>
                    <div class="header-button-group">
                        <button class="coupon-button" id="couponButton">
                            <i class="fas fa-gift"></i>
                        </button>
                        <button class="nysa-feed-button" id="nysaFeedButton">
                          <i class="fas fa-image"></i>
                        </button>
                                  <button class="jobs-button" id="jobsButton">
                                    <i class="fas fa-briefcase"></i>
                        </button>
                                          </div>
                </div>
        
                <div class="chat-messages" id="chatMessages">
                    <div class="category-grid" id="categoryGrid">
                        <!-- Categories will be dynamically populated -->
                    </div>
                             
                    <div class="woohoo" id="woohoo">
                        <div class="hahaha">
                    </div>
                </div>
            </div>

            
            <!-- Bottom Navigation -->
            <nav class="bottom-nav">
                <div class="nav-container">
                    <div class="nav-item" data-page="services">
                           <i class="fas fa-user-tie"></i>
                        <span>Service</span>
                    </div>
                    <div class="nav-item" data-page="brand">
                        <i class="fas fa-store"></i>
                        <span>Stores</span>
                    </div>
                    <div class="nav-item deepchat-item" data-page="chat">
                            <i class="fas fa-message"></i>
                            <span>Nysa Chat</span>
                        </div>
                        <div class="nav-item" data-page="places">
                            <i class="fas fa-landmark"></i>
                            <span>Places</span>
                       </div>
                       <div class="nav-item" data-page="real-estate" onclick="showRealEstateOverlay()">
                        <i class="fas fa-home"></i>
                        <span>Real Estate</span>
                    </div>
                    </div>
                </nav>
    
            <div class="menu-overlay" id="menuOverlay">
                <div class="menu-sections">
                    <div class="menu-section">
                        <div class="menu-section-title">About Us</div>
                        <div class="about-content">
                  Nysa is a revolutionary digital platform that connects users with a diverse range of stores, services, opportunities, and innovations—all in one place. From groceries, fashion, and restaurants to real estate, jobs, healthcare, and education, Nysa makes everyday life simpler, smarter, and more interactive. Users can explore live feeds, place orders, interact with posts, access exclusive offers, and engage with businesses like never before. With features like VR views, gamified actions, and seamless chat, Nysa is reshaping how people discover and interact with the world around them. Nysa isn’t just an platform—it’s your gateway to a smarter lifestyle.
                        </div>   
                    </div>
                         
                    <div class="menu-section">
                        <div class="menu-section-title">Connect With Us</div>
                        <div class="social-links">
                            <a href="https://nysaabhi.github.io/chat" class="social-link">
                                <i class="fab fa-facebook-f"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-twitter"></i>
                            </a>
                            <a href="https://www.instagram.com/nysa.world?igsh=MWNkMWN5cDBvNHhzdQ==" class="social-link">
                                <i class="fab fa-instagram"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-linkedin-in"></i>
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Our Locations</div>
                        <div class="location-links">
                            <a href="location.html" class="location-link">
                                <i class="fas fa-map-marker-alt"></i> India
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">List On Nysa</div>
                        <div class="partner-links">
                            <a href="https://www.example1.com" class="partner-link">
                                <i class="fas fa-tools"></i> Service Provider
                            </a>
                            <a href="https://www.example2.com" class="partner-link">
                                <i class="fas fa-store"></i> Stores
                            </a>
                            <a href="https://www.example3.com" class="partner-link">
                                <i class="fas fa-map-marker-alt"></i> Public Places
                            </a>
                            <a href="https://www.example4.com" class="partner-link">
                                <i class="fas fa-building"></i> Real Estate
                            </a>
                        </div>
                    </div>
                </div>
            </div> 

            <div class="chat-input-container">
                <div class="input-with-voice">
                    <input type="text" id="chatInput" placeholder="Search for services or ask a question..." />
                    <button class="voice-button" id="voiceButton">
                        <i class="fas fa-microphone"></i>
                    </button>
                </div>
                <button class="send-message" id="sendButton">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
            </div>
      
    
        
            <script src="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.js"></script>
            <script>                        
// Complete Chat System Implementation
const searchConfig = {
  promptDatabase: {
  'where can i recharge my phone': {
    answer: 'You can recharge your phone using Paytm, PhonePe, or Google Pay.',
    clickButtons: [
      {
        label: 'Recharge',
        url: 'https://paytm.com/recharge',
        icon: 'fa-mobile'
      }
    ],
    elaborateInfo: {
      'Payment Methods': [
        'UPI payments through digital wallets',
        'Credit/Debit cards',
        'Net banking',
        'Wallet balance'
      ],
      'Process': [
        'Select operator and circle',
        'Enter mobile number',
        'Choose recharge plan',
        'Select payment method',
        'Complete payment'
      ],
      'Security': [
        'Encrypted transactions',
        'OTP verification',
        'Payment protection guarantee'
      ]
    }
  },
  
  'book cab': {
    answer: 'You can book a cab using Uber or Ola apps.',
    clickButtons: [
      {
        label: 'Uber',
        url: '/redirect-uber',
        icon: 'fa-car'
      },
      {
        label: 'Ola',
        url: '/redirect-ola',
        icon: 'fa-taxi'
      }
    ],
    elaborateInfo: {
      'Available Services': [
        'Mini/Sedan/SUV options',
        'Shared rides',
        'Rentals',
        'Outstation'
      ],
      'Booking Steps': [
        'Enter pickup location',
        'Choose destination',
        'Select car type',
        'Confirm booking'
      ],
      'Payment Options': [
        'Cash payment',
        'Digital wallets',
        'Cards',
        'UPI'
      ]
    }
  },

  'book train': {
      answer: 'You can book a train ticket. Please provide the source station, destination station, and travel date.',
      clickButtons: [
        {
          label: 'Check Trains',
          url: 'https://www.confirmtkt.com/rbooking/trains/from/{from}/to/{to}/{date}',
          icon: 'fa-train'
        }
      ],
      elaborateInfo: {
        'Instructions': [
          'Enter your source station code (e.g., BPL for Bhopal)',
          'Enter your destination station code (e.g., MMCT for Mumbai)',
          'Enter your travel date in DD-MM-YYYY format'
        ]
      }
    },

    'book flight': {
      answer: 'You can book a flight ticket. Please provide the source airport, destination airport, and travel dates.',
      clickButtons: [
        {
          label: 'Check Flights',
          url: 'https://www.skyscanner.co.in/transport/flights/{from}/{to}/{departureDate}/{returnDate}/?adultsv2=1&cabinclass=economy&childrenv2=&ref=home&rtn=1&preferdirects=false&outboundaltsenabled=false&inboundaltsenabled=false',
          icon: 'fa-plane'
        }
      ],
      elaborateInfo: {
        'Instructions': [
          'Enter your source airport code (e.g., BHO for Bhopal)',
          'Enter your destination airport code (e.g., BOM for Mumbai)',
          'Enter your departure date in DDMMYY format',
          'Enter your return date in DDMMYY format (if round trip)'
        ]
      }
    },

    'register domain': {
      answer: 'You can register a domain. Please provide the domain name you want to check.',
      clickButtons: [
        {
          label: 'Check Domain',
          url: 'https://www.godaddy.com/en-in/domainsearch/find?domainToCheck={domain}',
          icon: 'fa-globe'
        }
      ],
      elaborateInfo: {
        'Instructions': [
          'Enter the domain name you want to register (e.g., RadhaKrishna.com)'
        ]
      }
    },

    'check stocks': {
      answer: 'You can check stock prices. Please provide the stock name or ticker symbol.',
      clickButtons: [
        {
          label: 'Check Stock',
          url: 'https://upstox.com/stocks/{stock}',
          icon: 'fa-chart-line'
        }
      ],
      elaborateInfo: {
        'Instructions': [
          'Enter the stock name or ticker symbol (e.g., TITAN for Titan Company Limited)'
        ]
      }
    },


  'apply for aadhar card': {
    answer: 'You can apply for Aadhar card online or visit nearest enrollment center.',
    clickButtons: [
      {
        label: 'Apply',
        url: 'https://uidai.gov.in/my-aadhaar/appointment-booking.html',
        icon: 'fa-id-card'
      },
      {
        label: 'Find',
        url: 'https://appointments.uidai.gov.in/centerSearch',
        icon: 'fa-location-dot'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Proof of Identity (Passport/DL/Voter ID)',
        'Proof of Address (Utility bill/Rent agreement)',
        'Proof of Birth (Birth certificate/School certificate)',
        'Recent passport size photograph'
      ],
      'Application Steps': [
        'Book appointment online or visit center',
        'Submit required documents',
        'Biometric capture (fingerprints, iris, photograph)',
        'Receive acknowledgment slip',
        'Track application status online'
      ],
      'Important Notes': [
        'Free of cost for first time applicants',
        'Mandatory biometric update for children at 5 and 15 years',
        'SMS notifications for status updates',
        'E-Aadhaar available for download after generation'
      ]
    }
  },

  'apply for pan card': {
    answer: 'You can apply for PAN card online through NSDL or UTITSL portal.',
    clickButtons: [
      {
        label: 'Apply Online',
        url: 'https://www.onlineservices.nsdl.com/paam/endUserRegisterContact.html',
        icon: 'fa-credit-card'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Proof of Identity',
        'Proof of Address',
        'Proof of Date of Birth',
        'Recent passport size photograph',
        'Digital signature (for online application)'
      ],
      'Application Process': [
        'Fill online application form',
        'Upload required documents',
        'Pay application fee',
        'Schedule in-person verification if required',
        'Track application status'
      ],
      'Fee Structure': [
        'Regular application: ₹107',
        'Express delivery: Additional charges apply',
        'Digital signature charges if applicable'
      ]
    }
  },

  'open bank account': {
    answer: 'You can open a bank account online or visit nearest branch of preferred bank.',
    clickButtons: [
      {
        label: 'Compare Banks',
        url: '/compare-banks',
        icon: 'fas fa-building'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Valid ID proof (Aadhar/PAN/Passport)',
        'Address proof',
        'Passport size photographs',
        'Income proof (for certain accounts)'
      ],
      'Account Types': [
        'Savings Account',
        'Current Account',
        'Salary Account',
        'Fixed Deposit Account'
      ],
      'Process Steps': [
        'Choose account type and bank',
        'Fill application form',
        'Submit KYC documents',
        'Initial deposit (if required)',
        'Receive account kit'
      ],
      'Features to Compare': [
        'Minimum balance requirement',
        'Interest rates',
        'Online banking facilities',
        'Debit card features',
        'Banking charges'
      ]
    }
  },

  'file income tax returns': {
    answer: 'You can file your Income Tax Returns (ITR) online through the Income Tax portal.',
    clickButtons: [
      {
        label: 'File ITR',
        url: 'https://www.incometax.gov.in/iec/foportal',
        icon: 'fa-file-invoice-dollar'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Form 16 from employer',
        'Bank statements',
        'Investment proofs',
        'Property tax receipts',
        'Loan statements'
      ],
      'Filing Process': [
        'Register on Income Tax portal',
        'Choose correct ITR form',
        'Fill in tax details',
        'Upload required documents',
        'Verify using Aadhaar OTP/Digital Signature'
      ],
      'Important Dates': [
        'Financial Year: April 1 to March 31',
        'Due date for individuals: July 31',
        'Late filing fees applicable after due date'
      ],
      'Additional Information': [
        'Tax saving investments under 80C',
        'Deductions available',
        'Penalty for non-filing',
        'Tax refund process'
      ]
    }
  },

  'apply for passport': {
    answer: 'You can apply for passport online through Passport Seva portal.',
    clickButtons: [
      {
        label: 'Apply Now',
        url: 'https://www.passportindia.gov.in',
        icon: 'fa-passport'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Proof of Identity',
        'Proof of Address',
        'Birth Certificate',
        'Educational certificates',
        'Aadhaar card'
      ],
      'Application Steps': [
        'Register on Passport Seva portal',
        'Fill online application form',
        'Schedule appointment',
        'Pay fees online',
        'Visit PSK for verification'
      ],
      'Fee Structure': [
        'Normal Application: ₹1500',
        'Tatkal Application: ₹3500',
        'Additional charges for SMS updates'
      ],
      'Processing Time': [
        'Normal: 30-45 days',
        'Tatkal: 1-3 days',
        'Subject to police verification'
      ]
    }
  }
},

  diseaseDatabase: {
    diseases: [
      {
        id: 1,
        name: "Asthma",
        description: "A condition in which a person's airways become inflamed, narrow, and produce extra mucus, making breathing difficult.",
        symptoms: ["Shortness of breath", "Chest tightness", "Wheezing", "Coughing"],
        treatments: ["Inhalers", "Bronchodilators", "Anti-inflammatory medications"],
        prevention: ["Avoid allergens", "Quit smoking", "Regular exercise"],
        metadata: {
          'Overview': [
            'Chronic respiratory condition',
            'Affects airways and breathing',
            'Can be managed with proper treatment'
          ],
          'Symptoms': [
            'Shortness of breath',
            'Chest tightness',
            'Wheezing',
            'Coughing'
          ],
          'Treatments': [
            'Inhalers',
            'Bronchodilators',
            'Anti-inflammatory medications'
          ],
          'Prevention': [
            'Avoid allergens',
            'Quit smoking',
            'Regular exercise'
          ],
          'Emergency Signs': [
            'Severe shortness of breath',
            'Bluish lips or face',
            'Rapid worsening of symptoms'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://www.mayoclinic.org/diseases-conditions/asthma/symptoms-causes/syc-20369653',
            icon: 'fa-info-circle'
          },
          {
            label: 'Find a Doctor',
            url: 'https://www.mayoclinic.org/appointments',
            icon: 'fa-user-md'
          }
        ]
      },
      {
        id: 2,
        name: "Diabetes",
        description: "A chronic condition that affects how your body turns food into energy, leading to high blood sugar levels.",
        symptoms: ["Increased thirst", "Frequent urination", "Extreme fatigue", "Blurred vision"],
        treatments: ["Insulin therapy", "Oral medications", "Diet and exercise"],
        prevention: ["Healthy diet", "Regular exercise", "Weight management"],
        metadata: {
          'Overview': [
            'Chronic metabolic disorder',
            'Affects blood sugar regulation',
            'Type 1 and Type 2 diabetes'
          ],
          'Symptoms': [
            'Increased thirst',
            'Frequent urination',
            'Extreme fatigue',
            'Blurred vision'
          ],
          'Treatments': [
            'Insulin therapy',
            'Oral medications',
            'Diet and exercise'
          ],
          'Prevention': [
            'Healthy diet',
            'Regular exercise',
            'Weight management'
          ],
          'Complications': [
            'Heart disease',
            'Kidney damage',
            'Nerve damage'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://www.mayoclinic.org/diseases-conditions/diabetes/symptoms-causes/syc-20371444',
            icon: 'fa-info-circle'
          },
          {
            label: 'Find a Doctor',
            url: 'https://www.mayoclinic.org/appointments',
            icon: 'fa-user-md'
          }
        ]
      },
      // Add more diseases alphabetically...
    ]
  },
  
  coursesDatabase: {
    courses: [
      {
        id: 1,
        name: "Full Stack Web Development",
        description: "Learn full-stack web development with HTML, CSS, JavaScript, Node.js, and React.",
        category: "Web Development",
        duration: "6 months",
        fees: "$500",
        metadata: {
          'Overview': [
            'Comprehensive course covering front-end and back-end development',
            'Hands-on projects and real-world applications',
            'Suitable for beginners and intermediate learners'
          ],
          'Curriculum': [
            'HTML, CSS, JavaScript Basics',
            'Advanced JavaScript and ES6',
            'React for Front-End Development',
            'Node.js and Express for Back-End',
            'Database Management with MongoDB'
          ],
          'Instructors': [
            'John Doe - Senior Web Developer',
            'Jane Smith - Full Stack Engineer'
          ],
          'Certification': [
            'Certificate of Completion',
            'Project-based assessments'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://example.com/full-stack-course',
            icon: 'fa-info-circle'
          },
          {
            label: 'Enroll Now',
            url: 'https://example.com/enroll/full-stack-course',
            icon: 'fa-user-graduate'
          }
        ]
      },
      {
        id: 2,
        name: "Data Science with Python",
        description: "Master data science concepts with Python, including data analysis, machine learning, and visualization.",
        category: "Data Science",
        duration: "4 months",
        fees: "$400",
        metadata: {
          'Overview': [
            'Learn data analysis, machine learning, and data visualization',
            'Hands-on projects with real datasets',
            'Suitable for beginners and professionals'
          ],
          'Curriculum': [
            'Python Basics for Data Science',
            'Data Analysis with Pandas and NumPy',
            'Machine Learning with Scikit-Learn',
            'Data Visualization with Matplotlib and Seaborn',
            'Capstone Project'
          ],
          'Instructors': [
            'Dr. Emily Johnson - Data Scientist',
            'Michael Brown - Machine Learning Engineer'
          ],
          'Certification': [
            'Certificate of Completion',
            'Capstone Project Certification'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://example.com/data-science-course',
            icon: 'fa-info-circle'
          },
          {
            label: 'Enroll Now',
            url: 'https://example.com/enroll/data-science-course',
            icon: 'fa-user-graduate'
          }
        ]
      },
      // Add more courses...
    ]
  },

  franchiseDatabase: {
    franchises: [
      {
        id: 1,
        name: "McDonald's",
        description: "McDonald's is the world's largest chain of hamburger fast food restaurants, serving around 68 million customers daily in 119 countries.",
        category: "Fast Food",
        investmentRange: "$1,000,000 - $2,300,000",
        initialFranchiseFee: "$45,000",
        royaltyFee: "4%",
        metadata: {
          'Overview': [
            'Global fast food chain',
            'Specializes in hamburgers, fries, and beverages',
            'Over 38,000 locations worldwide'
          ],
          'Requirements': [
            'Minimum net worth: $500,000',
            'Liquid assets: $250,000',
            'Experience in restaurant management preferred'
          ],
          'Support': [
            'Comprehensive training programs',
            'Ongoing operational support',
            'Marketing and advertising assistance'
          ],
          'Benefits': [
            'Strong brand recognition',
            'Proven business model',
            'Access to global supply chain'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://www.mcdonalds.com/us/en-us/franchising.html',
            icon: 'fa-info-circle'
          },
          {
            label: 'Apply Now',
            url: 'https://www.mcdonalds.com/us/en-us/franchising/apply.html',
            icon: 'fa-file-alt'
          }
        ]
      },
      {
        id: 2,
        name: "Domino's Pizza",
        description: "Domino's Pizza is an American multinational pizza restaurant chain founded in 1960. It is the second-largest pizza chain in the United States and the largest worldwide.",
        category: "Pizza",
        investmentRange: "$145,000 - $500,000",
        initialFranchiseFee: "$10,000",
        royaltyFee: "5.5%",
        metadata: {
          'Overview': [
            'Global pizza delivery chain',
            'Specializes in pizza, pasta, and sides',
            'Over 17,000 locations worldwide'
          ],
          'Requirements': [
            'Minimum net worth: $250,000',
            'Liquid assets: $75,000',
            'Experience in food service preferred'
          ],
          'Support': [
            'Comprehensive training programs',
            'Ongoing operational support',
            'Marketing and advertising assistance'
          ],
          'Benefits': [
            'Strong brand recognition',
            'Proven business model',
            'Access to global supply chain'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://www.dominos.com/franchising',
            icon: 'fa-info-circle'
          },
          {
            label: 'Apply Now',
            url: 'https://www.dominos.com/franchising/apply',
            icon: 'fa-file-alt'
          }
        ]
      },
      // Add more franchises...
    ]
  },

  loanProvidersDatabase: {
        providers: [
            {
                id: 1,
                name: "HDFC Bank",
                loanTypes: ["Personal", "Home", "Business"],
                loanAmount: { min: 10000, max: 5000000 },
                interestRate: { min: 8.5, max: 15 },
                processingFees: "1% of loan amount or ₹2,000 (whichever is higher)",
                tenure: { min: 1, max: 20 },
                emiOptions: "Flexible EMI options available",
                collateralRequired: false,
                eligibilityCriteria: [
                    "Minimum salary: ₹25,000 per month",
                    "Credit score: 750 or above",
                    "Stable employment history"
                ],
                documentsRequired: [
                    "PAN Card",
                    "Aadhaar Card",
                    "Salary slips (last 3 months)",
                    "Bank statements (last 6 months)"
                ],
                topLenders: ["HDFC Bank", "ICICI Bank", "Axis Bank"],
                governmentSchemes: false,
                processingTime: "3-5 working days",
                prepaymentCharges: "2% of outstanding amount",
                applyNowLink: "https://www.hdfcbank.com/personal/loans",
                metadata: {
                    'Overview': [
                        'HDFC Bank offers a wide range of loan products with competitive interest rates.',
                        'Flexible repayment options and quick processing.'
                    ],
                    'Benefits': [
                        'No collateral required for personal loans',
                        'Low processing fees',
                        'Quick approval and disbursal'
                    ],
                    'Contact Details': [
                        'Customer Care: 1800 202 6161',
                        'Email: loans@hdfcbank.com'
                    ]
                }
            },
            {
                id: 2,
                name: "ICICI Bank",
                loanTypes: ["Personal", "Home", "Business"],
                loanAmount: { min: 50000, max: 10000000 },
                interestRate: { min: 9, max: 16 },
                processingFees: "1.5% of loan amount or ₹3,000 (whichever is higher)",
                tenure: { min: 1, max: 25 },
                emiOptions: "Customizable EMI plans",
                collateralRequired: false,
                eligibilityCriteria: [
                    "Minimum salary: ₹30,000 per month",
                    "Credit score: 700 or above",
                    "Stable employment history"
                ],
                documentsRequired: [
                    "PAN Card",
                    "Aadhaar Card",
                    "Salary slips (last 3 months)",
                    "Bank statements (last 6 months)"
                ],
                topLenders: ["ICICI Bank", "HDFC Bank", "Axis Bank"],
                governmentSchemes: false,
                processingTime: "5-7 working days",
                prepaymentCharges: "3% of outstanding amount",
                applyNowLink: "https://www.icicibank.com/loans",
                metadata: {
                    'Overview': [
                        'ICICI Bank provides a variety of loan options with flexible repayment terms.',
                        'Competitive interest rates and quick processing.'
                    ],
                    'Benefits': [
                        'No collateral required for personal loans',
                        'Low processing fees',
                        'Quick approval and disbursal'
                    ],
                    'Contact Details': [
                        'Customer Care: 1800 1080',
                        'Email: loans@icicibank.com'
                    ]
                }
            },
            // Add more loan providers as needed
        ]
    },


  qaDatabase: {
    'how do i book a service': {
      answer: 'To book a service, click on the category, select a service, choose a package, and fill out the booking form. You can also call us directly at +91 9669181789 for immediate assistance.',
      category: 'Booking',
      keywords: ['book', 'service', 'booking', 'schedule', 'appointment']
    },
    "come posso prenotare un servizio": {
  "answer": "Per prenotare un servizio, fai clic sulla categoria, seleziona un servizio, scegli un pacchetto e compila il modulo di prenotazione. Puoi anche chiamarci direttamente al +91 9669181789 per assistenza immediata.",
  "category": "Prenotazione",
  "keywords": ["prenotare", "servizio", "prenotazione", "programmare", "appuntamento"]
},
"मैं सेवा कैसे बुक करूं": {
  "answer": "सेवा बुक करने के लिए, श्रेणी पर क्लिक करें, एक सेवा चुनें, एक पैकेज चुनें और बुकिंग फॉर्म भरें। आप तत्काल सहायता के लिए हमें सीधे +91 9669181789 पर कॉल भी कर सकते हैं।",
  "category": "बुकिंग",
  "keywords": ["बुक", "सेवा", "बुकिंग", "शेड्यूल", "अपॉइंटमेंट"]
},
    'what services do you offer': {
      answer: 'We offer a wide range of home and occasion services including Plumbing, Electrical, Carpentry, Painting, Appliance Repair, AC/Heating Repair, Cleaning Services, and Skills Training.',
      category: 'Services',
      keywords: ['services', 'offer', 'available', 'provide', 'types']
    },
    'hey': {
      answer: 'Hello! How can I assist you today?',
      category: 'Greeting',
      keywords: ['hey', 'hi', 'hello', 'greetings']
    },
    'hello': {
      answer: 'Hi there! What can I help you with?',
      category: 'Greeting',
      keywords: ['hello', 'hi', 'hey']
    },
    'how are you': {
      answer: 'I am just a program, but I am here to help! How about you?',
      category: 'General',
      keywords: ['how', 'are', 'you', 'doing']
    },
    'good morning': {
      answer: 'Good morning! How can I assist you today?',
      category: 'Greeting',
      keywords: ['good', 'morning']
    },
    'good afternoon': {
      answer: 'Good afternoon! How may I help you?',
      category: 'Greeting',
      keywords: ['good', 'afternoon']
    },
    'good evening': {
      answer: 'Good evening! What can I do for you today?',
      category: 'Greeting',
      keywords: ['good', 'evening']
    },
    'what is your name': {
      answer: 'I am your assistant. How can I help you?',
      category: 'General',
      keywords: ['your', 'name']
    },
    'who are you': {
      answer: 'I am your assistant, here to assist you with your queries.',
      category: 'General',
      keywords: ['who', 'are', 'you']
    },
    'what can you do': {
      answer: 'I can help you book services, answer your queries, and assist with information about our offerings.',
      category: 'General',
      keywords: ['what', 'can', 'do']
    },
    'thank you': {
      answer: 'You’re welcome! Let me know if you need any further assistance.',
      category: "Polite",
      keywords: ['thank', 'you', 'thanks']
    },
    'bye': {
      answer: 'Goodbye! Have a great day!',
      category: 'Polite',
      keywords: ['bye', 'goodbye', 'see', 'later']
    },
    'do you offer emergency services': {
      answer: 'Yes, we do offer emergency services. Please call us at +91 9669181789 for immediate assistance.',
      category: 'Services',
      keywords: ['emergency', 'services', 'urgent', 'immediate']
    },
    'how do i cancel a booking': {
      answer: 'To cancel a booking, please log into your account, go to your bookings, and select the cancel option. You can also contact our support team.',
      category: 'Booking',
      keywords: ['cancel', 'booking', 'remove', 'appointment']
    },
    'do you have a mobile app': {
      answer: 'Yes, we have a mobile app available for download on Android and iOS platforms. Search for our app in your store.',
      category: 'General',
      keywords: ['mobile', 'app', 'application', 'download']
    },
    'how can i give feedback': {
      answer: 'You can provide feedback through our website or mobile app under the feedback section. We value your input!',
      category: 'Feedback',
      keywords: ['feedback', 'review', 'suggestion', 'input']
    },
    'do you offer discounts': {
      answer: 'Yes, we have seasonal and promotional discounts. Please check our website or subscribe to our newsletter for updates.',
      category: 'Services',
      keywords: ['discounts', 'offers', 'promotions', 'deals']
    },
    'where are you located': {
      answer: 'We are an online platform but operate across multiple cities. Contact us for location-specific services.',
      category: 'General',
      keywords: ['location', 'where', 'address', 'based']
    },
    'can i reschedule a booking': {
      answer: 'Yes, you can reschedule your booking through your account or by contacting our support team.',
      category: 'Booking',
      keywords: ['reschedule', 'change', 'booking', 'appointment']
    },
    'do you provide 24/7 support': {
      answer: 'Yes, our support team is available 24/7 to assist you.',
      category: 'Greeting',
      keywords: ['24/7', 'support', 'available', 'hours']
    },
    "who is narendra modi": {
      "answer": "Narendra Modi is the current Prime Minister of India, serving since May 2014. He is a leader of the Bharatiya Janata Party (BJP) and was previously the Chief Minister of Gujarat from 2001 to 2014.",
      "category": "Politics",
      "keywords": ["narendra", "modi", "prime minister", "india", "bjp"]
    },
    "who was the first prime minister of india": {
      "answer": "Jawaharlal Nehru was the first Prime Minister of India, serving from 1947 to 1964.",
      "category": "Politics",
      "keywords": ["first", "prime minister", "india", "jawaharlal", "nehru"]
    },
    "who is the president of the united states": {
      "answer": "As of 2025, the current President of the United States is [Check latest information], serving since [year].",
      "category": "Politics",
      "keywords": ["president", "usa", "united states", "america"]
    },
    "who was the first american prime minister": {
      "answer": "The United States does not have a Prime Minister. Instead, it has a President. The first President of the United States was George Washington, who served from 1789 to 1797.",
      "category": "Politics",
      "keywords": ["american", "prime minister", "first", "usa", "george washington"]
    },
    "who is the prime minister of the uk": {
      "answer": "As of 2025, the current Prime Minister of the United Kingdom is [Check latest information], serving since [year].",
      "category": "Politics",
      "keywords": ["prime minister", "uk", "united kingdom", "britain"]
    },
    // ... Add additional entries in a similar structure
  },

  mathKeywords: ['calculate', 'solve', 'what is', 'find', 'area', 'perimeter', 'volume', 'divide', 'multiply', '+', '-', '*', '/', '=', 'equation'],
  
  fallbackResponses: [
    "I apologize, but I couldn't find an exact match for your query. Could you please rephrase your question?",
    "I'm not sure about that specific question. Would you like to know about our available services or booking process instead?",
    "I couldn't find a direct answer to your question. Would you like to speak with a customer service representative?"
    ]
};
  
class EnhancedMathSolver {
  constructor() {
    this.basicOperators = {
      'plus': '+',
      'add': '+',
      'sum': '+',
      'minus': '-',
      'subtract': '-',
      'difference': '-',
      'times': '*',
      'multiply': '*',
      'product': '*',
      'divided by': '/',
      'divide': '/',
      'over': '/',
      'power': '^',
      'squared': '^2',
      'cubed': '^3'
    };

    // Word problem patterns
    this.wordProblemPatterns = [
      {
        type: 'total_cost_with_change',
        patterns: [
          /bought (\d+) packs of biscuits for ₹(\d+) each and (\d+) chocolate bars for ₹(\d+) each. If I pay with a ₹(\d+) note, how much change will I get?/i,
          /purchased (\d+) items for ₹(\d+) each and (\d+) items for ₹(\d+) each. If I pay with a ₹(\d+) note, how much change will I get?/i
        ],
        solve: (matches) => {
          const qty1 = parseInt(matches[1]);
          const price1 = parseFloat(matches[2]);
          const qty2 = parseInt(matches[3]);
          const price2 = parseFloat(matches[4]);
          const paid = parseFloat(matches[5]);

          const totalCost = (qty1 * price1) + (qty2 * price2);
          const change = paid - totalCost;

          return {
            result: change,
            explanation: `Items Purchased:\n- ${qty1} packs of biscuits at ₹${price1} each\n- ${qty2} chocolate bars at ₹${price2} each\nTotal Cost: ₹${totalCost.toFixed(2)}\nAmount Paid: ₹${paid.toFixed(2)}\nChange Due: ₹${change.toFixed(2)}`
          };
        }
      },
      {
        type: 'discount_with_tax',
        patterns: [
          /A jacket originally costs ₹(\d+) but is on a (\d+)% discount. After applying a (\d+)% tax on the discounted price, what is the final cost?/i,
          /An item costs ₹(\d+) with a (\d+)% discount and a (\d+)% tax applied. What is the final price?/i
        ],
        solve: (matches) => {
          const originalPrice = parseFloat(matches[1]);
          const discount = parseFloat(matches[2]);
          const tax = parseFloat(matches[3]);

          const discountedPrice = originalPrice * (1 - discount / 100);
          const finalPrice = discountedPrice * (1 + tax / 100);

          return {
            result: finalPrice,
            explanation: `Original Price: ₹${originalPrice.toFixed(2)}\nDiscount: ${discount}% (-₹${(originalPrice * discount / 100).toFixed(2)})\nDiscounted Price: ₹${discountedPrice.toFixed(2)}\nTax: ${tax}% (+₹${(discountedPrice * tax / 100).toFixed(2)})\nFinal Price: ₹${finalPrice.toFixed(2)}`
          };
        }
      }
    ];
  }

  detectMathQuery(query) {
    const normalizedQuery = query.toLowerCase();
    
    // Check for word problems first
    for (const problemType of this.wordProblemPatterns) {
      for (const pattern of problemType.patterns) {
        if (pattern.test(normalizedQuery)) {
          return true;
        }
      }
    }
    
    // Then check for regular math expressions
    const hasNumbers = /\d+(\.\d+)?/.test(normalizedQuery);
    const hasAdvancedPattern = /(?:solve|calculate|evaluate|find|what is|compute)/i.test(normalizedQuery);
    
    return hasNumbers && hasAdvancedPattern;
  }

  solveMathProblem(query) {
    const normalizedQuery = query.toLowerCase();
    
    // Try to solve as a word problem first
    for (const problemType of this.wordProblemPatterns) {
      for (const pattern of problemType.patterns) {
        const matches = normalizedQuery.match(pattern);
        if (matches) {
          const solution = problemType.solve(matches);
          return this.formatWordProblemResponse(problemType.type, solution);
        }
      }
    }
    
    // If not a word problem, use existing math solving logic
    try {
      const parsedExpression = this.parseExpression(normalizedQuery);
      const result = eval(parsedExpression.replace(/\^/g, '**'));
      return this.formatArithmeticResponse(parsedExpression, result);
    } catch (e) {
      return this.handleError(query);
    }
  }

  formatWordProblemResponse(type, solution) {
    return `📝 Problem Solution:\n\n${solution.explanation}`;
  }

  formatArithmeticResponse(expression, result) {
    return `🔢 Result: ${result}`;
  }

  handleError(query) {
    return `❌ Unable to process the mathematical expression: "${query}"\nPlease check the format and try again.`;
  }

  parseExpression(query) {
    let normalizedQuery = query.toLowerCase()
      .replace(/what is|calculate|solve|find|evaluate|compute/g, '')
      .trim();

    Object.entries(this.basicOperators).forEach(([word, symbol]) => {
      normalizedQuery = normalizedQuery.replace(new RegExp(word, 'g'), symbol);
    });

    return normalizedQuery;
  }
}

class ChatSystem {
  constructor() {
    this.messageHistory = [];
    this.typingTimeout = null;
    this.similarityThreshold = 0.5;
    this.mathSolver = new EnhancedMathSolver();
    this.hasUserChatted = false; // Track if the user has started chatting
  }

  initialize() {
    this.chatInput = document.getElementById('chatInput');
    this.chatMessages = document.getElementById('chatMessages');
    this.sendButton = document.getElementById('sendButton');
    this.setupEventListeners();
    this.displayWelcomeMessage();
  }

  setupEventListeners() {
    this.sendButton.addEventListener('click', () => this.handleUserInput());
    this.chatInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        e.preventDefault();
        this.handleUserInput();
      }
    });
    this.chatInput.addEventListener('input', () => this.handleTypingIndicator());
  }

  handleTypingIndicator() {
    clearTimeout(this.typingTimeout);
    const typingIndicator = document.getElementById('typingIndicator');
    
    if (!typingIndicator) {
      const indicator = document.createElement('div');
      indicator.id = 'typingIndicator';
      indicator.className = 'message bot-message typing-indicator';
      indicator.innerHTML = '<div class="dots"><span>.</span><span>.</span><span>.</span></div>';
      this.chatMessages.appendChild(indicator);
    }

    this.typingTimeout = setTimeout(() => {
      const indicator = document.getElementById('typingIndicator');
      if (indicator) indicator.remove();
    }, 1000);
  }

  displayWelcomeMessage() {
    const welcomeMessage = `
      <div id="welcomeMessage" class="message bot-message welcome-message">
        <div class="welcome-avatar"><i class="fas fa-message"></i></div>
        <div class="welcome-content">
          <p>👋 Hi! I'm your Assistant</p>
        </div>
      </div>`;
    this.chatMessages.innerHTML = welcomeMessage;
}

  async handleUserInput() {
    const query = this.chatInput.value.trim();
    if (!query) return;

    // Remove the welcome message if it's the first user message
    if (!this.hasUserChatted) {
      this.hasUserChatted = true;
      const welcomeMessage = document.getElementById('welcomeMessage');
      if (welcomeMessage) {
        welcomeMessage.classList.add('fade-out');
        setTimeout(() => welcomeMessage.remove(), 300); // Wait for fade-out animation
      }
    }

    this.displayMessage(query, 'user');
    this.chatInput.value = '';

    await this.simulateProcessing();
    const response = this.processQuery(query);
    this.displayMessage(response, 'bot');
    
    this.messageHistory.push({ query, response, timestamp: new Date() });
    this.scrollToBottom();
    
    this.setupClickButtons();
    this.setupTouchButtons();
  }

  setupClickButtons() {
      document.querySelectorAll('.click-button').forEach(button => {
          button.addEventListener('click', () => {
              const url = button.getAttribute('data-url');
              if (url) window.location.href = url;
          });
      });
  }

  setupTouchButtons() {
      document.querySelectorAll('.touch-button').forEach(button => {
          button.addEventListener('click', () => {
              const url = button.getAttribute('data-url');
              if (url) window.location.href = url;
          });
      });
  }

  processQuery(query) {
  const normalizedQuery = query.toLowerCase();

          // Check for loan-related keywords
          const loanKeywords = ["loan", "personal loan", "home loan", "business loan", "emi", "interest rate"];
        const isLoanQuery = loanKeywords.some(keyword => normalizedQuery.includes(keyword));

        if (isLoanQuery) {
            return this.processLoanQuery(normalizedQuery);
        }

      // Try course query processing first
      const courseResponse = this.processCourseQuery(normalizedQuery);
    if (courseResponse) return courseResponse;

      // Try franchise query processing first
      const franchiseResponse = this.processFranchiseQuery(normalizedQuery);
    if (franchiseResponse) return franchiseResponse;

    // Try flight query processing
    const flightResponse = this.processFlightQuery(normalizedQuery);
  if (flightResponse) return flightResponse;

    const busResponse = this.processBusQuery(normalizedQuery);
    if (busResponse) return busResponse;

    // Try train query processing
    const trainResponse = this.processTrainQuery(normalizedQuery);
  if (trainResponse) return trainResponse;

    // Try live train status query processing
    const liveTrainResponse = this.processLiveTrainStatus(normalizedQuery);
  if (liveTrainResponse) return liveTrainResponse;

      // Try disease query processing first
  const diseaseResponse = this.processDiseaseQuery(normalizedQuery);
  if (diseaseResponse) return diseaseResponse;

  // Other query processing (unchanged)
  const domainResponse = this.processDomainRegistrationQuery(normalizedQuery);
  if (domainResponse) return domainResponse;

  const promptResponse = this.processPromptQuery(normalizedQuery);
  if (promptResponse) return promptResponse;

  if (this.mathSolver.detectMathQuery(normalizedQuery)) {
    return this.mathSolver.solveMathProblem(normalizedQuery);
  }

  const qaResponse = this.searchDatabase(normalizedQuery);
  if (qaResponse) return qaResponse;

  return this.getRandomFallbackResponse();
}

processLoanQuery(query) {
        const normalizedQuery = query.toLowerCase();

        // Extract loan details from the query
        const loanDetails = this.extractLoanDetails(normalizedQuery);

        if (!loanDetails) {
            return "Sorry, I couldn't understand your loan request. Please provide more details.";
        }

        // Get relevant loan providers
        const relevantProviders = this.getRelevantLoanProviders(loanDetails);

        if (relevantProviders.length > 0) {
            return this.generateLoanResponse(relevantProviders, loanDetails);
        } else {
            return "Sorry, I couldn't find any loan providers matching your request. Please try a different query.";
        }
    }

    extractLoanDetails(query) {
        const loanTypes = ["personal", "home", "business"];
        const loanTypeMatch = loanTypes.find(type => query.includes(type));

        const amountMatch = query.match(/₹?\s*(\d+,\d+|\d+)\s*(?:lakh|lac|thousand|k|rs|rupees)?/i);
        const amount = amountMatch ? parseFloat(amountMatch[1].replace(/,/g, '')) : null;

        const interestRateMatch = query.match(/(\d+(\.\d+)?)\s*%?\s*interest rate/i);
        const interestRate = interestRateMatch ? parseFloat(interestRateMatch[1]) : null;

        const tenureMatch = query.match(/(\d+)\s*(?:years|yrs|year|yr)/i);
        const tenure = tenureMatch ? parseInt(tenureMatch[1]) : null;

        return {
            type: loanTypeMatch,
            amount: amount,
            interestRate: interestRate,
            tenure: tenure
        };
    }

    getRelevantLoanProviders(loanDetails) {
        let providers = searchConfig.loanProvidersDatabase.providers;

        // Filter by loan type
        if (loanDetails.type) {
            providers = providers.filter(provider => 
                provider.loanTypes.map(t => t.toLowerCase()).includes(loanDetails.type)
            );
        }

        // Filter by loan amount
        if (loanDetails.amount) {
            providers = providers.filter(provider => 
                loanDetails.amount >= provider.loanAmount.min && 
                loanDetails.amount <= provider.loanAmount.max
            );
        }

        // Filter by interest rate
        if (loanDetails.interestRate) {
            providers = providers.filter(provider => 
                loanDetails.interestRate >= provider.interestRate.min && 
                loanDetails.interestRate <= provider.interestRate.max
            );
        }

        // Filter by tenure
        if (loanDetails.tenure) {
            providers = providers.filter(provider => 
                loanDetails.tenure >= provider.tenure.min && 
                loanDetails.tenure <= provider.tenure.max
            );
        }

        return providers.slice(0, 5); // Return top 5 results
    }

    generateLoanResponse(providers, loanDetails) {
        const providerList = providers.map(provider => this.formatLoanProvider(provider)).join('');

        return `
            <div class="loan-container">
                <h3>🏦 Loan Options</h3>
                <div class="loan-providers-list">
                    ${providerList}
                </div>
            </div>
        `;
    }

    formatLoanProvider(provider) {
        const metadata = provider.metadata ? `
            <div class="loan-info hidden">
                ${Object.entries(provider.metadata).map(([section, items]) => `
                    <div class="info-section">
                        <h4>${section}</h4>
                        <ul>
                            ${items.map(item => `<li>${item}</li>`).join('')}
                        </ul>
                    </div>
                `).join('')}
            </div>
        ` : '';

        return `
            <div class="loan-item" onclick="toggleLoanInfo(this)">
                <div class="loan-header">
                    <div class="loan-name">${provider.name}</div>
                    <div class="loan-type">🏷️ ${provider.loanTypes.join(', ')}</div>
                </div>
                <div class="loan-details">
                    <div class="loan-amount">💰 ₹${provider.loanAmount.min.toLocaleString()} - ₹${provider.loanAmount.max.toLocaleString()}</div>
                    <div class="interest-rate">📈 ${provider.interestRate.min}% - ${provider.interestRate.max}%</div>
                    <div class="tenure">📅 ${provider.tenure.min} - ${provider.tenure.max} years</div>
                </div>
                <div class="loan-description">${provider.emiOptions}</div>
                <div class="action-buttons">
                    <a href="${provider.applyNowLink}" class="apply-now-button" target="_blank">
                        <i class="fas fa-file-signature"></i> Apply Now
                    </a>
                </div>
                ${metadata}
            </div>
        `;
    }

processCourseQuery(query) {
    const normalizedQuery = query.toLowerCase();

    // Extract course name or category from the query
    const courseMatch = normalizedQuery.match(/(web development|data science|python|full stack|course)/i);
    if (!courseMatch) return null;

    const courseNameOrCategory = courseMatch[0].toLowerCase();

    // Find courses in the database
    const courses = searchConfig.coursesDatabase.courses.filter(course => 
      course.name.toLowerCase().includes(courseNameOrCategory) || 
      course.category.toLowerCase().includes(courseNameOrCategory)
    );

    if (courses.length > 0) {
      return this.generateCourseResponse(courses);
    }

    return null;
  }

  generateCourseResponse(courses) {
    const courseList = courses.map(course => this.formatCourse(course)).join('');

    return `
      <div class="courses-container">
        <h3>📚 Courses</h3>
        <div class="courses-list">
          ${courseList}
        </div>
      </div>
    `;
  }

  formatCourse(course) {
    const metadata = course.metadata ? `
      <div class="course-info hidden">
        ${Object.entries(course.metadata).map(([section, items]) => `
          <div class="info-section">
            <h4>${section}</h4>
            <ul>
              ${items.map(item => `<li>${item}</li>`).join('')}
            </ul>
          </div>
        `).join('')}
      </div>
    ` : '';

    const actionButtons = course.actionButtons.map(btn => `
      <a href="${btn.url}" class="action-button">
        <i class="fas ${btn.icon}"></i> ${btn.label}
      </a>
    `).join('');

    return `
      <div class="course-item" onclick="toggleCourseInfo(this)">
        <div class="course-header">
          <div class="course-name">${course.name}</div>
          <div class="course-category">🏷️ ${course.category}</div>
        </div>
        <div class="course-details">
          <div class="course-duration">⏳ ${course.duration}</div>
          <div class="course-fees">💰 ${course.fees}</div>
        </div>
        <div class="course-description">${course.description}</div>
        <div class="action-buttons">
          ${actionButtons}
        </div>
        ${metadata}
      </div>
    `;
  }

processFranchiseQuery(query) {
    const normalizedQuery = query.toLowerCase();

    // Extract franchise name or category from the query
    const franchiseMatch = normalizedQuery.match(/(mcdonald's|domino's|pizza|fast food|franchise)/i);
    if (!franchiseMatch) return null;

    const franchiseNameOrCategory = franchiseMatch[0].toLowerCase();

    // Find franchises in the database
    const franchises = searchConfig.franchiseDatabase.franchises.filter(franchise => 
      franchise.name.toLowerCase().includes(franchiseNameOrCategory) || 
      franchise.category.toLowerCase().includes(franchiseNameOrCategory)
    );

    if (franchises.length > 0) {
      return this.generateFranchiseResponse(franchises);
    }

    return null;
  }

  generateFranchiseResponse(franchises) {
    const franchiseList = franchises.map(franchise => this.formatFranchise(franchise)).join('');

    return `
      <div class="franchise-container">
        <h3>🏢 Franchise Opportunities</h3>
        <div class="franchises-list">
          ${franchiseList}
        </div>
      </div>
    `;
  }

  formatFranchise(franchise) {
    const metadata = franchise.metadata ? `
      <div class="franchise-info hidden">
        ${Object.entries(franchise.metadata).map(([section, items]) => `
          <div class="info-section">
            <h4>${section}</h4>
            <ul>
              ${items.map(item => `<li>${item}</li>`).join('')}
            </ul>
          </div>
        `).join('')}
      </div>
    ` : '';

    const actionButtons = franchise.actionButtons.map(btn => `
      <a href="${btn.url}" class="action-button">
        <i class="fas ${btn.icon}"></i> ${btn.label}
      </a>
    `).join('');

    return `
      <div class="franchise-item" onclick="toggleFranchiseInfo(this)">
        <div class="franchise-header">
          <div class="franchise-name">${franchise.name}</div>
          <div class="franchise-category">🏷️ ${franchise.category}</div>
        </div>
        <div class="franchise-details">
          <div class="investment-range">💰 ${franchise.investmentRange}</div>
          <div class="franchise-fee">💵 Initial Fee: ${franchise.initialFranchiseFee}</div>
          <div class="royalty-fee">📊 Royalty Fee: ${franchise.royaltyFee}</div>
        </div>
        <div class="franchise-description">${franchise.description}</div>
        <div class="action-buttons">
          ${actionButtons}
        </div>
        ${metadata}
      </div>
    `;
  }

  processFlightQuery(query) {
  const normalizedQuery = query.toLowerCase();

  // Airport data mapping city names to airport codes
  const airportData = {
    'mumbai': 'BOM',
    'delhi': 'DEL',
    'bangalore': 'BLR',
    'bengaluru': 'BLR',
    'hyderabad': 'HYD',
    'chennai': 'MAA',
    'kolkata': 'CCU',
    'pune': 'PNQ',
    'ahmedabad': 'AMD',
    'indore': 'IDR',
    'bhopal': 'BHO'
  };

  // Enhanced flight pattern to capture more details
  const flightPattern = /(?:book|search|find|check)?\s?(?:a )?flight\s?(?:from )?([\w\s]+)?\s?(?:to )?([\w\s]+)?\s?(?:on )?(\d{1,2}(?:th|st|nd|rd)?\s?\w+)?\s?(?:and return on )?(\d{1,2}(?:th|st|nd|rd)?\s?\w+)?/i;
  const match = normalizedQuery.match(flightPattern);

  if (match) {
    let fromCity = null;
    let toCity = null;

    // Extract cities from the query
    const cities = normalizedQuery.match(/\b(mumbai|delhi|bangalore|bengaluru|hyderabad|chennai|kolkata|pune|ahmedabad|indore|bhopal)\b/g);

    if (cities && cities.length >= 2) {
      // If the query explicitly mentions "from" and "to", use that order
      if (normalizedQuery.includes('from') && normalizedQuery.includes('to')) {
        const fromIndex = normalizedQuery.indexOf('from');
        const toIndex = normalizedQuery.indexOf('to');
        fromCity = normalizedQuery.slice(fromIndex + 5, toIndex).trim();
        toCity = normalizedQuery.slice(toIndex + 3).trim();
      } else {
        // Otherwise, assume the first city is "from" and the second is "to"
        fromCity = cities[0];
        toCity = cities[1];
      }
    } else if (cities && cities.length === 1) {
      // If only one city is mentioned, assume it's the "to" city
      toCity = cities[0];
    }

    const departureDate = match[3] ? this.parseDate(match[3].trim()) : null;
    const returnDate = match[4] ? this.parseDate(match[4].trim()) : null;

    // Extract class type from the query
    const classMatch = normalizedQuery.match(/\b(economy|premium economy|business|first class)\b/i);
    const classType = classMatch ? classMatch[0].charAt(0).toLowerCase() : 'e';

    // Extract number of travelers from the query
    const travellersMatch = normalizedQuery.match(/\b(\d+)\s?(?:passengers?|travelers?|people|adults?)\b/i);
    const travellers = travellersMatch ? parseInt(travellersMatch[1], 10) : 1;

    // Determine trip type
    const tripType = returnDate ? 'round-trip' : 'one-way';

    return this.createFlightSearchResponse(fromCity, toCity, departureDate, returnDate, classType, travellers, tripType);
  }

  // If the query is just a generic flight search
  if (/\b(?:search|find|book)\s?flight\b/i.test(normalizedQuery)) {
    return this.createFlightSearchResponse();
  }

  return null;
}

createFlightSearchResponse(fromCity = null, toCity = null, departureDate = null, returnDate = null, classType = 'e', travellers = 1, tripType = 'one-way') {
  const isRoundTrip = tripType === 'round-trip';

  const airportData = {
    'mumbai': 'BOM',
    'delhi': 'DEL',
    'bangalore': 'BLR',
    'bengaluru': 'BLR',
    'hyderabad': 'HYD',
    'chennai': 'MAA',
    'kolkata': 'CCU',
    'pune': 'PNQ',
    'ahmedabad': 'AMD',
    'indore': 'IDR',
    'bhopal': 'BHO'
  };

  const createCustomSelect = (id, label, selectedCity) => {
  const cities = Object.keys(airportData);
  return `
    <div class="custom-select-container">
      <input type="text" 
             id="${id}-input" 
             class="city-search-input" 
             placeholder="${label}"
             autocomplete="off"
             ${selectedCity ? `value="${selectedCity}"` : ''}>
      <input type="hidden" 
             id="${id}" 
             name="${id}" 
             ${selectedCity ? `value="${airportData[selectedCity]}"` : ''}>
      <div id="${id}-dropdown" class="city-dropdown">
        <div class="dropdown-header">
          <span>Select City</span>
          <button class="close-dropdown-btn">×</button>
        </div>
        <div class="city-options">
          ${cities.map(city => `
            <div class="city-option" 
                 data-city="${city}"
                 data-code="${airportData[city]}"
                 ${selectedCity === city ? 'data-selected="true"' : ''}>
              <div class="city-code">${airportData[city]}</div>
              <div class="city-name">${city.charAt(0).toUpperCase() + city.slice(1)}</div>
            </div>
          `).join('')}
        </div>
      </div>
    </div>
  `;
};

const flightSearchHtml = `
    <div class="flight-search-response">
      <div class="trip-type-toggle">
        <button class="trip-type-btn ${tripType === 'one-way' ? 'active' : ''}" data-type="one-way">One-Way</button>
        <button class="trip-type-btn ${tripType === 'round-trip' ? 'active' : ''}" data-type="round-trip">Round-Trip</button>
      </div>

      <div class="city-selection">
        ${createCustomSelect('from', 'From', fromCity)}
        <button class="swap-cities-btn">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M8 3L4 7L8 11" />
            <path d="M4 7H20" />
            <path d="M16 21L20 17L16 13" />
            <path d="M20 17H4" />
          </svg>
        </button>
        ${createCustomSelect('to', 'To', toCity)}
      </div>

      <div class="date-selection">
        <div class="date-field">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="4" width="18" height="18" rx="2" ry="2" />
            <line x1="16" y1="2" x2="16" y2="6" />
            <line x1="8" y1="2" x2="8" y2="6" />
            <line x1="3" y1="10" x2="21" y2="10" />
          </svg>
          <input type="date" id="departureDate" name="departureDate" value="${departureDate || ''}" required>
        </div>
        <div class="date-field return-date ${isRoundTrip ? 'visible' : ''}" id="returnDateGroup">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="4" width="18" height="18" rx="2" ry="2" />
            <line x1="16" y1="2" x2="16" y2="6" />
            <line x1="8" y1="2" x2="8" y2="6" />
            <line x1="3" y1="10" x2="21" y2="10" />
          </svg>
          <input type="date" id="returnDate" name="returnDate" value="${returnDate || ''}">
        </div>
      </div>

      <div class="traveller-class">
        <div class="traveller-btn">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2" />
            <circle cx="9" cy="7" r="4" />
            <path d="M23 21v-2a4 4 0 0 0-3-3.87" />
            <path d="M16 3.13a4 4 0 0 1 0 7.75" />
          </svg>
          <span>${travellers} Traveller${travellers > 1 ? 's' : ''} • ${classType === 'e' ? 'Economy' : classType === 'p' ? 'Premium Economy' : 'Business'}</span>
        </div>
        <div class="traveller-dropdown">
          <div class="traveller-count">
            <span>Travellers</span>
            <div class="counter">
              <button class="counter-btn minus">-</button>
              <input type="number" id="travellers" name="travellers" value="${travellers}" min="1" max="9">
              <button class="counter-btn plus">+</button>
            </div>
          </div>
          <div class="class-selection">
            <span>Class</span>
            <select id="class" name="class">
              <option value="e" ${classType === 'e' ? 'selected' : ''}>Economy</option>
              <option value="p" ${classType === 'p' ? 'selected' : ''}>Premium Economy</option>
              <option value="b" ${classType === 'b' ? 'selected' : ''}>Business</option>
            </select>
          </div>
        </div>
      </div>

      <button type="submit" class="search-btn">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="11" cy="11" r="8" />
          <line x1="21" y1="21" x2="16.65" y2="16.65" />
        </svg>
        Search Flight
      </button>
    </div>

    <style>
      .flight-search-response {
        background: #FF69B4;
        border-radius: 24px;
        padding: 24px;
        max-width: 600px;
        margin: 0 auto;
      }

      .trip-type-toggle {
        background: #000;
        border-radius: 100px;
        padding: 4px;
        display: flex;
        margin-bottom: 24px;
      }

      .trip-type-btn {
        flex: 1;
        padding: 12px;
        border: none;
        border-radius: 100px;
        background: transparent;
        color: white;
        cursor: pointer;
        font-weight: 600;
        transition: all 0.3s ease;
      }

      .trip-type-btn.active {
        background: white;
        color: black;
      }

      .city-selection {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 24px;
      }

      .swap-cities-btn {
        background: rgba(0, 0, 0, 0.1);
        border: none;
        border-radius: 50%;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .swap-cities-btn:hover {
        background: rgba(0, 0, 0, 0.2);
      }

      .custom-select-container {
        flex: 1;
        position: relative;
      }

      .city-search-input {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: rgba(0, 0, 0, 0.1);
        color: white;
        font-size: 16px;
      }

      .city-search-input::placeholder {
        color: rgba(255, 255, 255, 0.7);
      }

/* Dropdown Container */
.city-dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: white;
  border-radius: 12px;
  margin-top: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  display: none;
  z-index: 1000;
  max-height: 300px;
  overflow-y: auto;
}

/* Dropdown Header */
.dropdown-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 16px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  color: #000;
  background: #fff;
  border-radius: 12px 12px 0 0;
  position: sticky;
  top: 0;
  z-index: 1;
}

/* Close Button */
.close-dropdown-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #666;
  padding: 0;
  line-height: 1;
}

.close-dropdown-btn:hover {
  color: #000;
}

/* City Options */
.city-options {
  padding: 8px 0;
}

.city-option {
  padding: 12px 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 12px;
}

.city-option:hover {
  background: rgba(0, 0, 0, 0.05);
}

.city-code {
  font-weight: 600;
  color: #000;
}

.city-name {
  color: #666;
}

      .date-selection {
        display: flex;
        gap: 12px;
        margin-bottom: 24px;
      }

      .date-field {
        flex: 1;
        background: rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        padding: 16px;
        display: flex;
        align-items: center;
        gap: 12px;
      }

      .date-field input {
        background: transparent;
        border: none;
        color: white;
        font-size: 16px;
        width: 100%;
      }

      .return-date {
        display: none;
      }

      .return-date.visible {
        display: flex;
      }

      .traveller-class {
        margin-bottom: 24px;
        position: relative;
      }

      .traveller-btn {
        background: rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        padding: 16px;
        display: flex;
        align-items: center;
        gap: 12px;
        cursor: pointer;
        color: white;
      }

      .traveller-dropdown {
        position: absolute;
        top: 100%;
        left: 0;
        right: 0;
        background: white;
        border-radius: 12px;
        margin-top: 8px;
        padding: 16px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        display: none;
      }

      .traveller-count,
      .class-selection {
        margin-bottom: 16px;
      }

      .counter {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-top: 8px;
      }

      .counter-btn {
        width: 32px;
        height: 32px;
        border-radius: 50%;
        border: 1px solid #ddd;
        background: white;
        cursor: pointer;
      }

      .counter input {
        width: 40px;
        text-align: center;
        border: none;
        font-size: 16px;
      }

      .class-selection select {
        width: 100%;
        padding: 8px;
        border: 1px solid #ddd;
        border-radius: 8px;
        margin-top: 8px;
        font-size: 16px;
      }

      .search-btn {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: white;
        color: #FF69B4;
        font-size: 16px;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .search-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      }

@media (max-width: 480px) {
  .flight-search-response {
    width: 83vw;
    padding: 16px;
    border-radius: 16px;
  }

  /* Improved city selection layout */
  .city-selection {
    flex-direction: row;
    gap: 12px;
    color: #FFF;
    align-items: center;
    flex-wrap: nowrap;
    margin-bottom: 16px;
  }

  .custom-select-container {
    flex: 1;
    min-width: 0;
    position: relative;
  }

  /* Enhanced city search input */
  .city-search-input {
    width: 100%;
    padding: 14px;
    font-size: 15px;
    height: 52px;
    border: 1px solid rgba(0, 0, 0, 0.12);
    border-radius: 12px;
    background: #000;
    transition: all 0.2s ease;
  }

  .city-search-input:focus {
    border-color: #FFF;
    outline: none;
  }

.city-dropdown {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  top: auto;
  max-height: 75vh;
  overflow-y: auto;
  margin: 0;
  border-radius: 20px 20px 0 0;
  box-shadow: 0 -4px 16px rgba(0, 0, 0, 0.12);
  padding: 24px 0 16px;
  z-index: 1001;
  background: #FFFFFF;
  transform: translateY(0);
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.city-dropdown.hidden {
  transform: translateY(100%);
}

.dropdown-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 16px 16px;
  color: #000;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

.close-dropdown-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #666;
}

.city-options {
  max-height: 60vh;
  overflow-y: auto;
}

  /* Improved typography and layout */
  .city-code {
    font-size: 18px;
    font-weight: 600;
    min-width: 48px;
    color: #1A1A1A;
  }

  .city-name {
    font-size: 15px;
    font-weight: 400;
    flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    color: #333333;
  }

  /* Professional overlay background */
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(4px);
    z-index: 1000;
    opacity: 1;
    transition: opacity 0.3s ease;
  }

  .modal-overlay.hidden {
    opacity: 0;
    pointer-events: none;
  }

  /* Improved swap button */
  .swap-cities-btn {
    width: 36px;
    height: 36px;
    flex-shrink: 0;
    border-radius: 50%;
    background: #FFFFFF;
    border: 1px solid rgba(0, 0, 0, 0.12);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
  }

  .swap-cities-btn:active {
    background: rgba(0, 0, 0, 0.05);
    transform: scale(0.95);
  }

  /* Scrollbar styling */
  .city-dropdown::-webkit-scrollbar {
    width: 8px;
  }

  .city-dropdown::-webkit-scrollbar-track {
    background: transparent;
  }

  .city-dropdown::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 4px;
  }
}
            </style>
  `;

  // Add event listeners
  setTimeout(() => {
    const setupFlightCitySearch = (inputId) => {
  const input = document.getElementById(`${inputId}-input`);
  const hiddenInput = document.getElementById(inputId);
  const dropdown = document.getElementById(`${inputId}-dropdown`);
  const closeBtn = dropdown.querySelector('.close-dropdown-btn');
  const options = dropdown.querySelectorAll('.city-option');

  let isDropdownOpen = false;

  const openDropdown = () => {
    dropdown.style.display = 'block';
    isDropdownOpen = true;
  };

  const closeDropdown = () => {
    dropdown.style.display = 'none';
    isDropdownOpen = false;
  };

  // Open dropdown on input click (more reliable on mobile)
  input.addEventListener('click', (e) => {
    e.stopPropagation(); // Prevent event from bubbling up
    if (!isDropdownOpen) {
      openDropdown();
    }
  });

  // Close dropdown when clicking outside
  document.addEventListener('click', (e) => {
    if (!input.contains(e.target) && !dropdown.contains(e.target)) {
      closeDropdown();
    }
  });

  // Close dropdown when clicking the close button
  closeBtn.addEventListener('click', (e) => {
    e.stopPropagation(); // Prevent event from bubbling up
    closeDropdown();
  });

  // Handle city selection
  options.forEach(option => {
    option.addEventListener('click', (e) => {
      e.stopPropagation(); // Prevent event from bubbling up
      const cityName = option.dataset.city;
      const cityCode = option.dataset.code;
      input.value = `${cityCode} - ${cityName.charAt(0).toUpperCase() + cityName.slice(1)}`;
      hiddenInput.value = cityCode;
      closeDropdown();
    });
  });

  // Filter options based on input
  input.addEventListener('input', (e) => {
    const searchText = e.target.value.toLowerCase();
    options.forEach(option => {
      const cityName = option.dataset.city;
      const cityCode = option.dataset.code;
      const matches = cityName.toLowerCase().includes(searchText) || 
                     cityCode.toLowerCase().includes(searchText);
      option.style.display = matches ? 'flex' : 'none';
    });
  });
};

  // Flight dropdown setup
  setupFlightCitySearch('from');
  setupFlightCitySearch('to');

// Trip type toggle
const tripTypeBtns = document.querySelectorAll('.trip-type-btn');
const returnDateGroup = document.getElementById('returnDateGroup');

tripTypeBtns.forEach(btn => {
  btn.addEventListener('click', () => {
    tripTypeBtns.forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    const type = btn.dataset.type;
    returnDateGroup.style.display = type === 'round-trip' ? 'flex' : 'none';
    document.getElementById('tripType').value = type;
  });
});

// City swap functionality
const swapBtn = document.querySelector('.swap-cities-btn');
swapBtn.addEventListener('click', () => {
  const fromInput = document.getElementById('from-input');
  const toInput = document.getElementById('to-input');
  const fromHidden = document.getElementById('from');
  const toHidden = document.getElementById('to');

  const tempValue = fromInput.value;
  const tempHidden = fromHidden.value;
  
  fromInput.value = toInput.value;
  fromHidden.value = toHidden.value;
  toInput.value = tempValue;
  toHidden.value = tempHidden;
});

// Traveller and class selection
const travellerBtn = document.querySelector('.traveller-btn');
const travellerDropdown = document.querySelector('.traveller-dropdown');
const counterBtns = document.querySelectorAll('.counter-btn');
const travellersInput = document.getElementById('travellers');
const classSelect = document.getElementById('class');

travellerBtn.addEventListener('click', () => {
  travellerDropdown.style.display = travellerDropdown.style.display === 'block' ? 'none' : 'block';
});

document.addEventListener('click', (e) => {
  if (!travellerBtn.contains(e.target) && !travellerDropdown.contains(e.target)) {
    travellerDropdown.style.display = 'none';
  }
});

counterBtns.forEach(btn => {
  btn.addEventListener('click', () => {
    const isPlus = btn.classList.contains('plus');
    const currentValue = parseInt(travellersInput.value);
    
    if (isPlus && currentValue < 9) {
      travellersInput.value = currentValue + 1;
    } else if (!isPlus && currentValue > 1) {
      travellersInput.value = currentValue - 1;
    }
    
    updateTravellerDisplay();
  });
});

classSelect.addEventListener('change', updateTravellerDisplay);

function updateTravellerDisplay() {
  const travellers = travellersInput.value;
  const classType = classSelect.options[classSelect.selectedIndex].text;
  const displayText = `${travellers} Traveller${travellers > 1 ? 's' : ''} • ${classType}`;
  document.querySelector('.traveller-btn span').textContent = displayText;
}

  // Form submission
    // Form submission
    document.querySelector('.search-btn').addEventListener('click', (e) => {
      e.preventDefault();
      
      const formData = {
        from: document.getElementById('from').value,
        to: document.getElementById('to').value,
        departureDate: document.getElementById('departureDate').value,
        returnDate: document.getElementById('returnDate').value,
        class: document.getElementById('class').value,
        travellers: document.getElementById('travellers').value,
        tripType: document.querySelector('.trip-type-btn.active').dataset.type
      };

      // Validate required fields
      if (!formData.from || !formData.to || !formData.departureDate) {
        alert('Please fill out all required fields: From, To, and Departure Date.');
        return;
      }

      if (formData.tripType === 'round-trip' && !formData.returnDate) {
        alert('Please select a return date for round-trip flights.');
        return;
      }

      // Generate and open URL
      const baseUrl = 'https://www.ixigo.com/search/result/flight';
      
      const formatDate = (dateString) => {
        if (!dateString) return '';
        const [year, month, day] = dateString.split('-');
        return `${day}${month}${year}`;
      };

      const params = new URLSearchParams({
        from: formData.from,
        to: formData.to,
        date: formatDate(formData.departureDate),
        returnDate: formData.tripType === 'round-trip' ? formatDate(formData.returnDate) : '',
        adults: formData.travellers,
        children: '0',
        infants: '0',
        class: formData.class,
        source: 'Search Form'
      });

      const url = `${baseUrl}?${params.toString()}`;
      window.location.href = url; // Changed from window.open to window.location.href
    });

  // Date input formatting and validation
  const departureDateInput = document.getElementById('departureDate');
  const returnDateInput = document.getElementById('returnDate');

  departureDateInput.addEventListener('change', () => {
    if (returnDateInput.value) {
      validateDates();
    }
    returnDateInput.min = departureDateInput.value;
  });

  returnDateInput.addEventListener('change', validateDates);

  function validateDates() {
    const departureDate = new Date(departureDateInput.value);
    const returnDate = new Date(returnDateInput.value);
    
    if (returnDate < departureDate) {
      returnDateInput.value = departureDateInput.value;
    }
  }

  // Set minimum date to today
  const today = new Date().toISOString().split('T')[0];
  departureDateInput.min = today;
  returnDateInput.min = today;
}, 0);

return flightSearchHtml;
}

processTrainQuery(query) {
  const normalizedQuery = query.toLowerCase();

  // Station data mapping city names to station codes
  const stationData = {
    'mumbai': 'BCT',
    'delhi': 'NDLS',
    'bangalore': 'SBC',
    'bengaluru': 'SBC',
    'hyderabad': 'HYB',
    'chennai': 'MAS',
    'kolkata': 'HWH',
    'pune': 'PUNE',
    'ahmedabad': 'ADI',
    'indore': 'INDB',
    'bhopal': 'BPL'
  };

  // Enhanced train pattern to capture more details
  const trainPattern = /(?:book|search|find|check)?\s?(?:a )?train\s?(?:from )?([\w\s]+)?\s?(?:to )?([\w\s]+)?\s?(?:on )?(\d{1,2}(?:th|st|nd|rd)?\s?\w+)?/i;
  const match = normalizedQuery.match(trainPattern);

  if (match) {
    let fromStation = null;
    let toStation = null;

    // Extract stations from the query
    const stations = normalizedQuery.match(/\b(mumbai|delhi|bangalore|bengaluru|hyderabad|chennai|kolkata|pune|ahmedabad|indore|bhopal)\b/g);

    if (stations && stations.length >= 2) {
      // If the query explicitly mentions "from" and "to", use that order
      if (normalizedQuery.includes('from') && normalizedQuery.includes('to')) {
        const fromIndex = normalizedQuery.indexOf('from');
        const toIndex = normalizedQuery.indexOf('to');
        fromStation = normalizedQuery.slice(fromIndex + 5, toIndex).trim();
        toStation = normalizedQuery.slice(toIndex + 3).trim();
      } else {
        // Otherwise, assume the first station is "from" and the second is "to"
        fromStation = stations[0];
        toStation = stations[1];
      }
    } else if (stations && stations.length === 1) {
      // If only one station is mentioned, assume it's the "to" station
      toStation = stations[0];
    }

    const departureDate = match[3] ? this.parseDate(match[3].trim()) : null;

    return this.createTrainSearchResponse(fromStation, toStation, departureDate);
  }

  // If the query is just a generic train search
  if (/\b(?:search|find|book)\s?train\b/i.test(normalizedQuery)) {
    return this.createTrainSearchResponse();
  }

  return null;
}

createTrainSearchResponse(fromStation = null, toStation = null, departureDate = null) {
  const stationData = {
    'mumbai': 'BCT',
    'delhi': 'NDLS',
    'bangalore': 'SBC',
    'bengaluru': 'SBC',
    'hyderabad': 'HYB',
    'chennai': 'MAS',
    'kolkata': 'HWH',
    'pune': 'PUNE',
    'ahmedabad': 'ADI',
    'indore': 'INDB',
    'bhopal': 'BPL'
  };

  const createCustomSelect = (id, label, selectedStation) => {
  const stations = Object.keys(stationData);
  return `
    <div class="custom-select-container">
      <input type="text" 
             id="${id}-input" 
             class="station-search-input" 
             placeholder="${label}"
             autocomplete="off"
             ${selectedStation ? `value="${selectedStation}"` : ''}>
      <input type="hidden" 
             id="${id}" 
             name="${id}" 
             ${selectedStation ? `value="${stationData[selectedStation]}"` : ''}>
      <div id="${id}-dropdown" class="station-dropdown">
        <div class="dropdown-header">
          <span>Select Station</span>
          <button class="close-dropdown-btn">×</button>
        </div>
        <div class="station-options">
          ${stations.map(station => `
            <div class="station-option" 
                 data-station="${station}"
                 data-code="${stationData[station]}"
                 ${selectedStation === station ? 'data-selected="true"' : ''}>
              <div class="station-code">${stationData[station]}</div>
              <div class="station-name">${station.charAt(0).toUpperCase() + station.slice(1)}</div>
            </div>
          `).join('')}
        </div>
      </div>
    </div>
  `;
};

const trainSearchHtml = `
  <div class="train-search-response">
    <div class="station-selection">
      ${createCustomSelect('departure', 'From', fromStation)}
      <button class="swap-stations-btn">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M8 3L4 7L8 11" />
          <path d="M4 7H20" />
          <path d="M16 21L20 17L16 13" />
          <path d="M20 17H4" />
        </svg>
      </button>
      ${createCustomSelect('destination', 'To', toStation)}
    </div>

    <div class="date-selection">
      <div class="date-field">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <rect x="3" y="4" width="18" height="18" rx="2" ry="2" />
          <line x1="16" y1="2" x2="16" y2="6" />
          <line x1="8" y1="2" x2="8" y2="6" />
          <line x1="3" y1="10" x2="21" y2="10" />
        </svg>
        <input type="date" id="departureDate" name="departureDate" value="${departureDate || ''}" required>
      </div>
    </div>

    <button type="submit" class="search-btn">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="11" cy="11" r="8" />
        <line x1="21" y1="21" x2="16.65" y2="16.65" />
      </svg>
      Search Train
    </button>
  </div>

    <style>
      .train-search-response {
        background: #FF69B4;
        border-radius: 24px;
        padding: 24px;
        max-width: 600px;
        margin: 0 auto;
      }

      .station-selection {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 24px;
      }

      .swap-stations-btn {
        background: rgba(0, 0, 0, 0.1);
        border: none;
        border-radius: 50%;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .swap-stations-btn:hover {
        background: rgba(0, 0, 0, 0.2);
      }

      .custom-select-container {
        flex: 1;
        position: relative;
      }

      .station-search-input {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: rgba(0, 0, 0, 0.1);
        color: white;
        font-size: 16px;
      }

      .station-search-input::placeholder {
        color: rgba(255, 255, 255, 0.7);
      }

      .station-dropdown {
        position: absolute;
        top: 100%;
        left: 0;
        right: 0;
        background: white;
        border-radius: 12px;
        margin-top: 8px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        display: none;
        z-index: 1000;
        max-height: 300px;
        overflow-y: auto;
      }

      .dropdown-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 12px 16px;
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        color: #000;
        background: #fff;
        border-radius: 12px 12px 0 0;
        position: sticky;
        top: 0;
        z-index: 1;
      }

      .close-dropdown-btn {
        background: none;
        border: none;
        font-size: 24px;
        cursor: pointer;
        color: #666;
        padding: 0;
        line-height: 1;
      }

      .close-dropdown-btn:hover {
        color: #000;
      }

      .station-options {
        padding: 8px 0;
      }

      .station-option {
        padding: 12px 16px;
        cursor: pointer;
        display: flex;
        align-items: center;
        gap: 12px;
      }

      .station-option:hover {
        background: rgba(0, 0, 0, 0.05);
      }

      .station-code {
        font-weight: 600;
        color: #000;
      }

      .station-name {
        color: #666;
      }

      .date-selection {
        display: flex;
        gap: 12px;
        margin-bottom: 24px;
      }

      .date-field {
        flex: 1;
        background: rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        padding: 16px;
        display: flex;
        align-items: center;
        gap: 12px;
      }

      .date-field input {
        background: transparent;
        border: none;
        color: white;
        font-size: 16px;
        width: 100%;
      }

      .search-btn {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: white;
        color: #FF69B4;
        font-size: 16px;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .search-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      }
    

    @media (max-width: 480px) {
  .train-search-response {
    width: 83vw;
    padding: 16px;
    border-radius: 16px;
  }

  /* Station selection layout */
  .station-selection {
    flex-direction: row;
    gap: 12px;
    color: #FFF;
    align-items: center;
    flex-wrap: nowrap;
    margin-bottom: 16px;
  }

  .custom-select-container {
    flex: 1;
    min-width: 0;
    position: relative;
  }

  /* Station search input */
  .station-search-input {
    width: 100%;
    padding: 14px;
    font-size: 15px;
    height: 52px;
    border: 1px solid rgba(0, 0, 0, 0.12);
    border-radius: 12px;
    background: #000;
    transition: all 0.2s ease;
  }

  .station-search-input:focus {
    border-color: #FFF;
    outline: none;
  }

  /* Bottom sheet station dropdown */
  .station-dropdown {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    top: auto;
    max-height: 75vh;
    overflow-y: auto;
    margin: 0;
    border-radius: 20px 20px 0 0;
    box-shadow: 0 -4px 16px rgba(0, 0, 0, 0.12);
    padding: 24px 0 16px;
    z-index: 1001;
    background: #FFFFFF;
    transform: translateY(0);
    transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .station-dropdown.hidden {
    transform: translateY(100%);
  }

  .dropdown-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 16px 16px;
    color: #000;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  }

  .close-dropdown-btn {
    background: none;
    border: none;
    font-size: 24px;
    cursor: pointer;
    color: #666;
  }

  .station-options {
    max-height: 60vh;
    overflow-y: auto;
  }

  /* Station option typography and layout */
  .station-code {
    font-size: 18px;
    font-weight: 600;
    min-width: 48px;
    color: #1A1A1A;
  }

  .station-name {
    font-size: 15px;
    font-weight: 400;
    flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    color: #333333;
  }

  /* Modal overlay */
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(4px);
    z-index: 1000;
    opacity: 1;
    transition: opacity 0.3s ease;
  }

  .modal-overlay.hidden {
    opacity: 0;
    pointer-events: none;
  }

  /* Swap stations button */
  .swap-stations-btn {
    width: 36px;
    height: 36px;
    flex-shrink: 0;
    border-radius: 50%;
    background: #FFFFFF;
    border: 1px solid rgba(0, 0, 0, 0.12);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
  }

  .swap-stations-btn:active {
    background: rgba(0, 0, 0, 0.05);
    transform: scale(0.95);
  }

  /* Scrollbar styling */
  .station-dropdown::-webkit-scrollbar {
    width: 8px;
  }

  .station-dropdown::-webkit-scrollbar-track {
    background: transparent;
  }

  .station-dropdown::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 4px;
  }
}
    </style>
  `;

  // Add event listeners
  setTimeout(() => {
    const setupTrainStationSearch = (inputId) => {
  const input = document.getElementById(`${inputId}-input`);
  const hiddenInput = document.getElementById(inputId);
  const dropdown = document.getElementById(`${inputId}-dropdown`);
  const closeBtn = dropdown.querySelector('.close-dropdown-btn');
  const options = dropdown.querySelectorAll('.station-option');

  let isDropdownOpen = false;

  const openDropdown = () => {
    dropdown.style.display = 'block';
    isDropdownOpen = true;
  };

  const closeDropdown = () => {
    dropdown.style.display = 'none';
    isDropdownOpen = false;
  };

  // Open dropdown on input click (more reliable on mobile)
  input.addEventListener('click', (e) => {
    e.stopPropagation(); // Prevent event from bubbling up
    if (!isDropdownOpen) {
      openDropdown();
    }
  });

  // Close dropdown when clicking outside
  document.addEventListener('click', (e) => {
    if (!input.contains(e.target) && !dropdown.contains(e.target)) {
      closeDropdown();
    }
  });

  // Close dropdown when clicking the close button
  closeBtn.addEventListener('click', (e) => {
    e.stopPropagation(); // Prevent event from bubbling up
    closeDropdown();
  });

  // Handle station selection
  options.forEach(option => {
    option.addEventListener('click', (e) => {
      e.stopPropagation(); // Prevent event from bubbling up
      const stationName = option.dataset.station;
      const stationCode = option.dataset.code;
      input.value = `${stationCode} - ${stationName.charAt(0).toUpperCase() + stationName.slice(1)}`;
      hiddenInput.value = stationCode;
      closeDropdown();
    });
  });

  // Filter options based on input
  input.addEventListener('input', (e) => {
    const searchText = e.target.value.toLowerCase();
    options.forEach(option => {
      const stationName = option.dataset.station;
      const stationCode = option.dataset.code;
      const matches = stationName.toLowerCase().includes(searchText) || 
                     stationCode.toLowerCase().includes(searchText);
      option.style.display = matches ? 'flex' : 'none';
    });
  });
};

// Train dropdown setup
  setupTrainStationSearch('departure');
  setupTrainStationSearch('destination');


    // Station swap functionality
    const swapBtn = document.querySelector('.swap-stations-btn');
    swapBtn.addEventListener('click', () => {
      const fromInput = document.getElementById('from-input');
      const toInput = document.getElementById('to-input');
      const fromHidden = document.getElementById('from');
      const toHidden = document.getElementById('to');

      const tempValue = fromInput.value;
      const tempHidden = fromHidden.value;
      
      fromInput.value = toInput.value;
      fromHidden.value = toHidden.value;
      toInput.value = tempValue;
      toHidden.value = tempHidden;
    });

    // Form submission
    document.querySelector('.search-btn').addEventListener('click', (e) => {
  e.preventDefault();
  
  const formData = {
    from: document.getElementById('departure').value,
    to: document.getElementById('destination').value,
    departureDate: document.getElementById('departureDate').value,
  };

  // Validate required fields
  if (!formData.from || !formData.to || !formData.departureDate) {
    alert('Please fill out all required fields: From, To, and Departure Date.');
    return;
  }

  // Generate and open URL
  const baseUrl = 'https://www.ixigo.com/search/result/train';
  
  const formatDate = (dateString) => {
    if (!dateString) return '';
    const [year, month, day] = dateString.split('-');
    return `${day}${month}${year}`;
  };

  const params = new URLSearchParams({
    from: formData.from,
    to: formData.to,
    date: formatDate(formData.departureDate),
  });

  const url = `${baseUrl}/${formData.from}/${formData.to}/${formatDate(formData.departureDate)}//1/0/0/0/all`;
  window.location.href = url;
});

    // Date input formatting and validation
    const departureDateInput = document.getElementById('departureDate');

    // Set minimum date to today
    const today = new Date().toISOString().split('T')[0];
    departureDateInput.min = today;
  }, 0);

  return trainSearchHtml;
}

processLiveTrainStatus(query) {
  const normalizedQuery = query.toLowerCase();

  // Train data mapping train names to train numbers
  const trainData = {
    '14722': 'Abohar Jodhpur Express',
    '54703': 'Abohar Jodhpur Passenger',
    '79438': 'Abu Road Mahesana DEMU',
    '55338': 'Achhnera Kasganj Fast Passenger',
    '55332': 'Achhnera Kasganj Passenger',
    '22188': 'Adhartal Rani Kamlapati Intercity Express',
    '17409': 'Adilabad Hazur Sahib Nanded Express'
  };

  // Enhanced train pattern to capture train name or number
  const trainPattern = /(?:live|status|check)?\s?(?:train)?\s?(?:status)?\s?(\d{5})?\s?([\w\s]+)?/i;
  const match = normalizedQuery.match(trainPattern);

  if (match) {
    const trainNumber = match[1];
    const trainName = match[2] ? match[2].trim() : null;

    // Find the train number if only the name is provided
    const foundTrainNumber = trainNumber || Object.keys(trainData).find(key => trainData[key].toLowerCase().includes(trainName.toLowerCase()));

    if (foundTrainNumber) {
      const trainName = trainData[foundTrainNumber];
      return this.createLiveTrainStatusResponse(foundTrainNumber, trainName);
    }
  }

  // If the query is just a generic live train status search
  if (/\b(?:live|status|check)\s?train\b/i.test(normalizedQuery)) {
    return this.createLiveTrainStatusResponse();
  }

  return null;
}

createLiveTrainStatusResponse(trainNumber = null, trainName = null) {
  const liveTrainStatusHtml = `
    <div class="live-train-status-response">
      <div class="train-input">
        <input type="text" 
               id="trainNumberInput" 
               class="train-search-input" 
               placeholder="Enter Train Number or Name"
               autocomplete="off"
               ${trainNumber ? `value="${trainNumber}"` : ''}>
        <input type="hidden" 
               id="trainNumber" 
               name="trainNumber" 
               ${trainNumber ? `value="${trainNumber}"` : ''}>
      </div>

      <button type="submit" class="check-status-btn">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="11" cy="11" r="8" />
          <line x1="21" y1="21" x2="16.65" y2="16.65" />
        </svg>
        Check Live Status
      </button>
    </div>

    <style>
      .live-train-status-response {
        background: #FF69B4;
        border-radius: 24px;
        padding: 24px;
        max-width: 600px;
        margin: 0 auto;
      }

      .train-input {
        margin-bottom: 24px;
      }

      .train-search-input {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: rgba(0, 0, 0, 0.1);
        color: white;
        font-size: 16px;
      }

      .train-search-input::placeholder {
        color: rgba(255, 255, 255, 0.7);
      }

      .check-status-btn {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: white;
        color: #FF69B4;
        font-size: 16px;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .check-status-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      }
    </style>
  `;

  // Add event listeners
  setTimeout(() => {
    const trainNumberInput = document.getElementById('trainNumberInput');
    const trainNumberHidden = document.getElementById('trainNumber');

    // Handle form submission
    document.querySelector('.check-status-btn').addEventListener('click', (e) => {
      e.preventDefault();

      const trainNumber = trainNumberHidden.value;
      const trainName = trainNumberInput.value;

      if (!trainNumber && !trainName) {
        alert('Please enter a valid train number or name.');
        return;
      }

      // Generate and open URL
      const baseUrl = 'https://www.redbus.in/railways/liveTrainDetails';
      const params = new URLSearchParams({
        trainNo: trainNumber || '',
        trainName: trainName || '',
        stn: 'null',
        from: 'LTS Home'
      });

      const url = `${baseUrl}?${params.toString()}`;
      window.location.href = url;
    });

    // Auto-fill train number if train name is entered
    trainNumberInput.addEventListener('input', (e) => {
      const searchText = e.target.value.toLowerCase();
      const foundTrainNumber = Object.keys(trainData).find(key => trainData[key].toLowerCase().includes(searchText));

      if (foundTrainNumber) {
        trainNumberHidden.value = foundTrainNumber;
      } else {
        trainNumberHidden.value = '';
      }
    });
  }, 0);

  return liveTrainStatusHtml;
}

processBusQuery(query) {
  const normalizedQuery = query.toLowerCase();

  // Bus city data mapping city names to city IDs
  const busCityData = {
    'mumbai': { name: 'Mumbai', id: '123' },
    'delhi': { name: 'Delhi', id: '456' },
    'bangalore': { name: 'Bangalore', id: '789' },
    'bengaluru': { name: 'Bangalore', id: '789' },
    'hyderabad': { name: 'Hyderabad', id: '101' },
    'chennai': { name: 'Chennai', id: '202' },
    'kolkata': { name: 'Kolkata', id: '303' },
    'pune': { name: 'Pune', id: '404' },
    'ahmedabad': { name: 'Ahmedabad', id: '505' },
    'indore': { name: 'Indore', id: '313' },
    'bhopal': { name: 'Bhopal', id: '979' }
  };

  // Enhanced bus pattern to capture more details
  const busPattern = /(?:book|search|find|check)?\s?(?:a )?bus\s?(?:from )?([\w\s]+)?\s?(?:to )?([\w\s]+)?\s?(?:on )?(\d{1,2}(?:th|st|nd|rd)?\s?\w+)?/i;
  const match = normalizedQuery.match(busPattern);

  if (match) {
    let fromCity = null;
    let toCity = null;

    // Extract cities from the query
    const cities = normalizedQuery.match(/\b(mumbai|delhi|bangalore|bengaluru|hyderabad|chennai|kolkata|pune|ahmedabad|indore|bhopal)\b/g);

    if (cities && cities.length >= 2) {
      // If the query explicitly mentions "from" and "to", use that order
      if (normalizedQuery.includes('from') && normalizedQuery.includes('to')) {
        const fromIndex = normalizedQuery.indexOf('from');
        const toIndex = normalizedQuery.indexOf('to');
        fromCity = normalizedQuery.slice(fromIndex + 5, toIndex).trim();
        toCity = normalizedQuery.slice(toIndex + 3).trim();
      } else {
        // Otherwise, assume the first city is "from" and the second is "to"
        fromCity = cities[0];
        toCity = cities[1];
      }
    } else if (cities && cities.length === 1) {
      // If only one city is mentioned, assume it's the "to" city
      toCity = cities[0];
    }

    const departureDate = match[3] ? this.parseDate(match[3].trim()) : null;

    return this.createBusSearchResponse(fromCity, toCity, departureDate);
  }

  // If the query is just a generic bus search
  if (/\b(?:search|find|book)\s?bus\b/i.test(normalizedQuery)) {
    return this.createBusSearchResponse();
  }

  return null;
}

createBusSearchResponse(fromCity = null, toCity = null, departureDate = null) {
  const busCityData = {
    'mumbai': { name: 'Mumbai', id: '123' },
    'delhi': { name: 'Delhi', id: '456' },
    'bangalore': { name: 'Bangalore', id: '789' },
    'bengaluru': { name: 'Bangalore', id: '789' },
    'hyderabad': { name: 'Hyderabad', id: '101' },
    'chennai': { name: 'Chennai', id: '202' },
    'kolkata': { name: 'Kolkata', id: '303' },
    'pune': { name: 'Pune', id: '404' },
    'ahmedabad': { name: 'Ahmedabad', id: '505' },
    'indore': { name: 'Indore', id: '313' },
    'bhopal': { name: 'Bhopal', id: '979' }
  };

  const createCustomSelect = (id, label, selectedCity) => {
  const cities = Object.keys(busCityData);
  return `
    <div class="custom-select-container">
      <input type="text" 
             id="${id}-input" 
             class="city-search-input" 
             placeholder="${label}"
             autocomplete="off"
             ${selectedCity ? `value="${selectedCity}"` : ''}>
      <input type="hidden" 
             id="${id}" 
             name="${id}" 
             ${selectedCity ? `value="${busCityData[selectedCity].id}"` : ''}>
      <div id="${id}-dropdown" class="city-dropdown">
        <div class="dropdown-header">
          <span>Select City</span>
          <button class="close-dropdown-btn">×</button>
        </div>
        <div class="city-options">
          ${cities.map(city => `
            <div class="city-option" 
                 data-city="${city}"
                 data-id="${busCityData[city].id}"
                 ${selectedCity === city ? 'data-selected="true"' : ''}>
              <div class="city-id">${busCityData[city].id}</div>
              <div class="city-name">${busCityData[city].name}</div>
            </div>
          `).join('')}
        </div>
      </div>
    </div>
  `;
};

const busSearchHtml = `
  <div class="bus-search-response">
    <div class="city-selection">
      ${createCustomSelect('origin', 'From', fromCity)}
      <button class="swap-cities-btn">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M8 3L4 7L8 11" />
          <path d="M4 7H20" />
          <path d="M16 21L20 17L16 13" />
          <path d="M20 17H4" />
        </svg>
      </button>
      ${createCustomSelect('arrival', 'To', toCity)}
    </div>

    <div class="date-selection">
      <div class="date-field">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <rect x="3" y="4" width="18" height="18" rx="2" ry="2" />
          <line x1="16" y1="2" x2="16" y2="6" />
          <line x1="8" y1="2" x2="8" y2="6" />
          <line x1="3" y1="10" x2="21" y2="10" />
        </svg>
        <input type="date" id="departureDate" name="departureDate" value="${departureDate || ''}" required>
      </div>
    </div>

    <button type="submit" class="search-btn">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="11" cy="11" r="8" />
        <line x1="21" y1="21" x2="16.65" y2="16.65" />
      </svg>
      Check Bus
    </button>
  </div>

    <style>
/* Base Styles */
.bus-search-response {
  background: #1a1a1f;
  border-radius: 24px;
  padding: 24px;
  max-width: 600px;
  margin: 0 auto;
  color: white;
  font-family: 'Segoe UI', Roboto, -apple-system, sans-serif;
}

/* City Selection Section */
.city-selection {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 24px;
  position: relative;
}

.custom-select-container {
  flex: 1;
  position: relative;
  min-width: 0;
}

.city-search-input {
  width: 100%;
  padding: 16px 52px 16px 16px;
  border: 2px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(4px);
  color: white;
  font-size: 16px;
  font-weight: 500;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.city-search-input:hover {
  border-color: rgba(255, 255, 255, 0.4);
}

.city-search-input:focus {
  border-color: white;
  outline: none;
  box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.2);
}

.city-search-input::placeholder {
  color: rgba(255, 255, 255, 0.7);
}

/* City Dropdown */
.city-dropdown {
  position: absolute;
  top: calc(100% + 8px);
  left: 0;
  right: 0;
  background: white;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
  display: none;
  z-index: 1000;
  max-height: 320px;
  overflow-y: auto;
  transform-origin: top center;
  animation: dropdownFadeIn 0.2s ease-out forwards;
}

@keyframes dropdownFadeIn {
  from {
    opacity: 0;
    transform: translateY(-8px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.dropdown-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 16px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.08);
  color: #333;
  background: #f9f9f9;
  border-radius: 12px 12px 0 0;
  position: sticky;
  top: 0;
  z-index: 1;
}

.close-dropdown-btn {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: #666;
  padding: 4px;
  line-height: 1;
  border-radius: 50%;
  transition: all 0.2s ease;
}

.close-dropdown-btn:hover {
  background: rgba(0, 0, 0, 0.05);
  color: #333;
}

.city-options {
  padding: 8px 0;
}

.city-option {
  padding: 12px 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 12px;
  transition: all 0.2s ease;
}

.city-option:hover {
  background: rgba(0, 0, 0, 0.03);
}

.city-option[data-selected="true"] {
  background: rgba(255, 105, 180, 0.1);
}

.city-id {
  font-weight: 600;
  color: #000;
  font-size: 14px;
  min-width: 36px;
}

.city-name {
  color: #444;
  font-size: 15px;
}

/* Swap Button */
.swap-cities-btn {
  background: rgba(255, 255, 255, 0.9);
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  flex-shrink: 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.swap-cities-btn:hover {
  background: white;
  transform: rotate(180deg) scale(1.05);
}

.swap-cities-btn svg {
  stroke: #FF69B4;
}

/* Date Selection */
.date-selection {
  display: flex;
  gap: 12px;
  margin-bottom: 24px;
}

.date-field {
  flex: 1;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  padding: 12px 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  border: 2px solid rgba(255, 255, 255, 0.2);
  transition: all 0.3s ease;
}

.date-field:hover {
  border-color: rgba(255, 255, 255, 0.4);
}

.date-field svg {
  flex-shrink: 0;
}

.date-field input {
  background: transparent;
  border: none;
  color: white;
  font-size: 16px;
  width: 100%;
  font-weight: 500;
}

.date-field input:focus {
  outline: none;
}

.date-field input::-webkit-calendar-picker-indicator {
  filter: invert(1);
  opacity: 0.8;
  cursor: pointer;
}

/* Search Button */
.search-btn {
  width: 100%;
  padding: 16px;
  border: none;
  border-radius: 12px;
  background: white;
  color: #FF69B4;
  font-size: 16px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.search-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

.search-btn:active {
  transform: translateY(0);
}

.search-btn svg {
  stroke: #FF69B4;
}

/* Mobile Styles */
@media (max-width: 768px) {
  .bus-search-response {
    border-radius: 20px;
    padding: 20px;
    width: calc(100% - 32px);
    margin: 0 auto;
  }
  
  .city-selection {
    flex-direction: row;
    gap: 8px;
    margin-bottom: 20px;
  }
  
  .city-search-input {
    padding: 14px 16px;
    font-size: 15px;
  }
  
  .city-dropdown {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    top: auto;
    max-height: 70vh;
    border-radius: 20px 20px 0 0;
    box-shadow: 0 -8px 32px rgba(0, 0, 0, 0.2);
    animation: mobileSlideUp 0.3s ease-out forwards;
  }
  
  @keyframes mobileSlideUp {
    from {
      transform: translateY(100%);
    }
    to {
      transform: translateY(0);
    }
  }
  
  .dropdown-header {
    padding: 16px;
  }
  
  .swap-cities-btn {
    width: 36px;
    height: 36px;
  }
  
  .date-field {
    padding: 12px 14px;
  }
  
  .search-btn {
    padding: 14px;
  }
}

/* Tablet Styles */
@media (min-width: 769px) and (max-width: 1024px) {
  .bus-search-response {
    max-width: 500px;
  }
}

/* Scrollbar Styling */
.city-dropdown::-webkit-scrollbar {
  width: 6px;
}

.city-dropdown::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.05);
  border-radius: 0 12px 12px 0;
}

.city-dropdown::-webkit-scrollbar-thumb {
  background: rgba(0, 0, 0, 0.2);
  border-radius: 3px;
}

.city-dropdown::-webkit-scrollbar-thumb:hover {
  background: rgba(0, 0, 0, 0.3);
}
    }
</style>
  `;

  // Add event listeners
  setTimeout(() => {
    const setupBusCitySearch = (inputId) => {
      const input = document.getElementById(`${inputId}-input`);
      const hiddenInput = document.getElementById(inputId);
      const dropdown = document.getElementById(`${inputId}-dropdown`);
      const closeBtn = dropdown.querySelector('.close-dropdown-btn');
      const options = dropdown.querySelectorAll('.city-option');

      let isDropdownOpen = false;

      const openDropdown = () => {
        dropdown.style.display = 'block';
        isDropdownOpen = true;
      };

      const closeDropdown = () => {
        dropdown.style.display = 'none';
        isDropdownOpen = false;
      };

      input.addEventListener('click', (e) => {
        e.stopPropagation();
        if (!isDropdownOpen) {
          openDropdown();
        }
      });

      document.addEventListener('click', (e) => {
        if (!input.contains(e.target) && !dropdown.contains(e.target)) {
          closeDropdown();
        }
      });

      closeBtn.addEventListener('click', (e) => {
        e.stopPropagation();
        closeDropdown();
      });

      options.forEach(option => {
        option.addEventListener('click', (e) => {
          e.stopPropagation();
          const cityName = option.dataset.city;
          const cityId = option.dataset.id;
          input.value = `${cityId} - ${busCityData[cityName].name}`;
          hiddenInput.value = cityId;
          closeDropdown();
        });
      });

      input.addEventListener('input', (e) => {
        const searchText = e.target.value.toLowerCase();
        options.forEach(option => {
          const cityName = option.dataset.city;
          const cityId = option.dataset.id;
          const matches = cityName.toLowerCase().includes(searchText) || 
                         cityId.toLowerCase().includes(searchText);
          option.style.display = matches ? 'flex' : 'none';
        });
      });
    };

    setupBusCitySearch('origin');
    setupBusCitySearch('arrival');

    const swapBtn = document.querySelector('.swap-cities-btn');
    swapBtn.addEventListener('click', () => {
      const fromInput = document.getElementById('origin-input');
      const toInput = document.getElementById('arrival-input');
      const fromHidden = document.getElementById('origin');
      const toHidden = document.getElementById('arrival');

      const tempValue = fromInput.value;
      const tempHidden = fromHidden.value;
      
      fromInput.value = toInput.value;
      fromHidden.value = toHidden.value;
      toInput.value = tempValue;
      toHidden.value = tempHidden;
    });

    document.querySelector('.search-btn').addEventListener('click', (e) => {
      e.preventDefault();
      
      const fromCityId = document.getElementById('origin').value;
      const toCityId = document.getElementById('arrival').value;
      const departureDate = document.getElementById('departureDate').value;

      if (!fromCityId || !toCityId || !departureDate) {
        alert('Please fill out all required fields: From, To, and Departure Date.');
        return;
      }

      const fromCityName = document.getElementById('origin-input').value.split(' - ')[1];
      const toCityName = document.getElementById('arrival-input').value.split(' - ')[1];

      const formatDate = (dateString) => {
        const [year, month, day] = dateString.split('-');
        const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
        return `${day}-${months[parseInt(month) - 1]}-${year}`;
      };

      const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);

      let url;
      if (isMobile) {
        url = `https://www.redbus.in/bus-tickets/${fromCityName.toLowerCase()}-to-${toCityName.toLowerCase()}?fromCityId=${fromCityId}&fromCityName=${encodeURIComponent(fromCityName)}&toCityId=${toCityId}&toCityName=${encodeURIComponent(toCityName)}&doj=${formatDate(departureDate)}&srcType=null&destType=null&srcCountry=IND&destCountry=IND&ref=mweb&step=srp`;
      } else {
        url = `https://www.redbus.in/search?fromCityName=${encodeURIComponent(fromCityName)}&fromCityId=${fromCityId}&toCityName=${encodeURIComponent(toCityName)}&toCityId=${toCityId}&onward=${formatDate(departureDate)}&srcCountry=IND&destCountry=IND`;
      }

      window.location.href = url;
    });

    const departureDateInput = document.getElementById('departureDate');
    const today = new Date().toISOString().split('T')[0];
    departureDateInput.min = today;
  }, 0);

  return busSearchHtml;
}

processDiseaseQuery(query) {
    const normalizedQuery = query.toLowerCase();

    // Extract disease name from the query
    const diseaseMatch = normalizedQuery.match(/(asthma|diabetes|flu|cancer|hypertension)/i);
    if (!diseaseMatch) return null;

    const diseaseName = diseaseMatch[0].toLowerCase();

    // Find the disease in the database
    const disease = searchConfig.diseaseDatabase.diseases.find(d => 
      d.name.toLowerCase() === diseaseName
    );

    if (disease) {
      return this.generateDiseaseResponse(disease);
    }

    return null;
  }

  generateDiseaseResponse(disease) {
  const metadata = disease.metadata ? `
    <div class="disease-info hidden">
      ${Object.entries(disease.metadata).map(([section, items]) => `
        <div class="info-section">
          <h3>${section}</h3>
          <ul>
            ${items.map(item => `<li>${item}</li>`).join('')}
          </ul>
        </div>
      `).join('')}
    </div>
  ` : '';

  const actionButtons = disease.actionButtons.map(btn => {
    if (btn.label === 'Find a Doctor') {
      return `
        <button class="action-button" onclick="openFindDoctorsModal()">
          <i class="fas ${btn.icon}"></i> ${btn.label}
        </button>
      `;
    }
    return `
      <a href="${btn.url}" class="action-button">
        <i class="fas ${btn.icon}"></i> ${btn.label}
      </a>
    `;
  }).join('');

  return `
    <div class="disease-item" onclick="toggleDiseaseInfo(this)">
      <div class="disease-header">
        <div class="disease-name">${disease.name}</div>
      </div>
      <div class="disease-description">${disease.description}</div>
      <div class="action-buttons">
        ${actionButtons}
      </div>
      ${metadata}
    </div>
  `;
}

  processDomainRegistrationQuery(query) {
    const domainPattern = /(?:register|check) (?:a )?domain (?:named )?([\w-]+\.\w+)/i;
    const match = query.match(domainPattern);
    if (match) {
      const domain = match[1];
      const url = `https://www.godaddy.com/en-in/domainsearch/find?domainToCheck=${domain}`;
      return this.createDomainRegistrationResponse(domain, url);
    }
    return null;
  }


  createDomainRegistrationResponse(domain, url) {
    return `
      <div class="domain-registration-response">
        <p>🌐 Domain Registration Details:</p>
        <ul>
          <li>Domain: ${domain}</li>
        </ul>
        <button class="click-button" data-url="${url}">
          <i class="fas fa-globe"></i> Check Domain
        </button>
      </div>
    `;
  }



  processPromptQuery(query) {
    // Normalize the query for better matching
    const normalizedQuery = query.toLowerCase().trim();

    // Check for exact match in prompt database
    if (searchConfig.promptDatabase[normalizedQuery]) {
      return this.createPromptResponse(searchConfig.promptDatabase[normalizedQuery]);
    }

    // Check for partial matches or similar queries
    let bestMatch = null;
    let highestSimilarity = 0;

    for (const [prompt, data] of Object.entries(searchConfig.promptDatabase)) {
      const normalizedPrompt = prompt.toLowerCase().trim();

      // Check if the query contains the prompt or vice versa
      if (
        normalizedQuery.includes(normalizedPrompt) ||
        normalizedPrompt.includes(normalizedQuery)
      ) {
        return this.createPromptResponse(data);
      }

      // Check for similarity using a similarity threshold
      const similarity = this.calculateSimilarity(normalizedQuery, normalizedPrompt);
      if (similarity > highestSimilarity && similarity > this.similarityThreshold) {
        highestSimilarity = similarity;
        bestMatch = data;
      }
    }

    // Return the best match if found
    if (bestMatch) {
      return this.createPromptResponse(bestMatch);
    }

    return null;
  }

  createPromptResponse(promptData) {
  const elaborateInfo = promptData.elaborateInfo ? `
    <div class="elaborate-info hidden">
      ${Object.entries(promptData.elaborateInfo).map(([section, items]) => `
        <div class="elaborate-section">
          <h3>${section}</h3>
          <ul>
            ${items.map(item => `<li>${item}</li>`).join('')}
          </ul>
        </div>
      `).join('')}
    </div>
  ` : '';

  return `
    <div class="prompt-response" onclick="toggleElaborateInfo(this)">
      <div class="answer">${promptData.answer}</div>
      <div class="click-buttons">
        ${promptData.clickButtons.map(btn => `
          <button class="click-button" data-url="${btn.url}">
            <i class="fas ${btn.icon}"></i> ${btn.label}
          </button>
        `).join('')}
      </div>
      ${elaborateInfo}
    </div>
  `;
}

  searchDatabase(query) {
    const normalizedQuery = query.toLowerCase();
    const words = normalizedQuery.split(/\s+/);
    const results = [];

    for (const [question, data] of Object.entries(searchConfig.qaDatabase)) {
      if (normalizedQuery === question.toLowerCase()) {
        results.push({ similarity: 1, data });
        continue;
      }

      const keywordMatch = data.keywords?.some(keyword => 
        words.includes(keyword.toLowerCase())
      );
      if (keywordMatch) {
        results.push({ similarity: 0.8, data });
        continue;
      }

      const similarity = this.calculateSimilarity(normalizedQuery, question.toLowerCase());
      if (similarity > this.similarityThreshold) {
        results.push({ similarity, data });
      }
    }

    if (results.length > 0) {
      results.sort((a, b) => b.similarity - a.similarity);
      return results[0].data.answer;
    }

    return this.getRandomFallbackResponse();
  }

  calculateSimilarity(str1, str2) {
  const words1 = str1.split(/\s+/);
  const words2 = str2.split(/\s+/);
  const set1 = new Set(words1);
  const set2 = new Set(words2);
  const intersection = new Set([...set1].filter(x => set2.has(x)));
  const union = new Set([...set1, ...set2]);
  const jaccardSimilarity = intersection.size / union.size;

  let orderSimilarity = 0;
  words1.forEach((word, index) => {
    const pos2 = words2.indexOf(word);
    if (pos2 !== -1) {
      orderSimilarity += 1 - Math.abs(index - pos2) / Math.max(words1.length, words2.length);
    }
  });

  return (jaccardSimilarity * 0.7) + (orderSimilarity / words1.length * 0.3);
}

  getRandomFallbackResponse() {
    const responses = searchConfig.fallbackResponses;
    return responses[Math.floor(Math.random() * responses.length)];
  }

  displayMessage(content, type) {
    const messageDiv = document.createElement('div');
    messageDiv.className = `message ${type}-message`;

    // Add long-press event listeners
    let pressTimer;
    messageDiv.addEventListener('mousedown', () => {
      pressTimer = setTimeout(() => {
        this.showActionIcons(messageDiv, content);
      }, 1000); // 1 second long press
    });

    messageDiv.addEventListener('mouseup', () => {
      clearTimeout(pressTimer);
    });

    messageDiv.addEventListener('mouseleave', () => {
      clearTimeout(pressTimer);
    });

    // Touch events for mobile devices
    messageDiv.addEventListener('touchstart', () => {
      pressTimer = setTimeout(() => {
        this.showActionIcons(messageDiv, content);
      }, 1000); // 1 second long press
    });

    messageDiv.addEventListener('touchend', () => {
      clearTimeout(pressTimer);
    });

    messageDiv.addEventListener('touchmove', () => {
      clearTimeout(pressTimer);
    });

    if (type === 'bot') {
      messageDiv.innerHTML = `
        <div class="message-content">${content}</div>`;
    } else {
      messageDiv.innerHTML = `
        <div class="message-content">${content}</div>`;
    }

    this.chatMessages.appendChild(messageDiv);
  }

  showActionIcons(container, content) {
    // Remove any existing action icons
    const existingIcons = container.querySelector('.action-icons');
    if (existingIcons) existingIcons.remove();

    // Create action icons container
    const actionIcons = document.createElement('div');
    actionIcons.className = 'action-icons';

    // Copy icon
    const copyIcon = document.createElement('div');
    copyIcon.className = 'action-icon copy-icon';
    copyIcon.innerHTML = `<i class="fas fa-copy"></i><span>Copy</span>`;
    copyIcon.addEventListener('click', (e) => {
      e.stopPropagation();
      this.copyMessage(container);
      this.hideActionIcons(actionIcons); // Hide icons after action
    });

    // Share icon
    const shareIcon = document.createElement('div');
    shareIcon.className = 'action-icon share-icon';
    shareIcon.innerHTML = `<i class="fas fa-share-alt"></i><span>Share</span>`;
    shareIcon.addEventListener('click', (e) => {
      e.stopPropagation();
      this.shareMessage(container);
      this.hideActionIcons(actionIcons); // Hide icons after action
    });

    // Append icons to container
    actionIcons.appendChild(copyIcon);
    actionIcons.appendChild(shareIcon);

    // Append container to the target element
    container.appendChild(actionIcons);

    // Hide icons if no action is taken after a short delay
    setTimeout(() => {
      if (actionIcons.parentElement) {
        this.hideActionIcons(actionIcons);
      }
    }, 3000); // Hide after 3 seconds of inactivity
  }

  hideActionIcons(actionIcons) {
    actionIcons.classList.add('fade-out');
    setTimeout(() => {
      if (actionIcons.parentElement) {
        actionIcons.parentElement.removeChild(actionIcons);
      }
    }, 200); // Match the fade-out animation duration
  }


  shareMessage(container) {
    let textToShare = '';

    // Handle different types of content
    if (container.classList.contains('job-item')) {
      textToShare = this.getJobText(container);
    } else if (container.classList.contains('coupon-item')) {
      textToShare = this.getCouponText(container);
    } else if (container.classList.contains('property-item')) {
      textToShare = this.getPropertyText(container);
    } else {
      textToShare = container.innerText; // Default to sharing all text
    }

    if (navigator.share) {
      navigator.share({
        title: 'Check this out!',
        text: textToShare,
        url: window.location.href,
      })
        .then(() => {
          this.showToast('Shared successfully!');
        })
        .catch((err) => {
          this.showToast('Failed to share');
        });
    } else {
      // Fallback for devices that don't support the Web Share API
      const shareUrl = `mailto:?subject=Check this out!&body=${encodeURIComponent(textToShare)}`;
      window.location.href = shareUrl;
    }
  }

  getJobText(jobItem) {
    const title = jobItem.querySelector('.job-title')?.innerText || '';
    const company = jobItem.querySelector('.job-company')?.innerText || '';
    const location = jobItem.querySelector('.job-location')?.innerText || '';
    const salary = jobItem.querySelector('.job-salary')?.innerText || '';
    const description = jobItem.querySelector('.job-description')?.innerText || '';
    return `${title}\n${company}\n${location}\n${salary}\n\n${description}`;
  }

  getCouponText(couponItem) {
    const brand = couponItem.querySelector('.brand-info span')?.innerText || '';
    const discount = couponItem.querySelector('.discount-tag')?.innerText || '';
    const code = couponItem.querySelector('.coupon-code')?.innerText || '';
    const validity = couponItem.querySelector('.validity')?.innerText || '';
    return `${brand}\n${discount}\nCode: ${code}\n${validity}`;
  }

  getPropertyText(propertyItem) {
    const title = propertyItem.querySelector('.property-title')?.innerText || '';
    const price = propertyItem.querySelector('.property-price')?.innerText || '';
    const location = propertyItem.querySelector('.property-location')?.innerText || '';
    const description = propertyItem.querySelector('.property-description')?.innerText || '';
    return `${title}\n${price}\n${location}\n\n${description}`;
  }

  async simulateProcessing() {
    const typingIndicator = document.createElement('div');
    typingIndicator.className = 'message bot-message typing-indicator';
    typingIndicator.innerHTML = '<div class="dots"><span>.</span><span>.</span><span>.</span></div>';
    this.chatMessages.appendChild(typingIndicator);
    this.scrollToBottom();
    await new Promise(resolve => setTimeout(resolve, 1000));
    typingIndicator.remove();
  }

  scrollToBottom() {
    this.chatMessages.scrollTop = this.chatMessages.scrollHeight;
  }
}

function createDoctorSearchModal() {
  // Create modal container
  const modal = document.createElement('div');
  modal.id = 'findDoctorsModal';
  modal.className = 'modal';

  // Modal inner content
  modal.innerHTML = `
    <div class="modal-content">
      <span class="close">&times;</span>
      <h2>Find Doctors</h2>
      <form id="findDoctorsForm">
        <div class="form-group">
          <label for="city">City</label>
          <input type="text" id="city" placeholder="Enter your city" required>
        </div>
        <div class="form-group">
          <label for="specialty">Specialty</label>
          <select id="specialty" required>
            <option value="Dentist">Dentist</option>
            <option value="Cardiologist">Cardiologist</option>
            <option value="Dermatologist">Dermatologist</option>
            <option value="Gynecologist">Gynecologist</option>
            <option value="Pediatrician">Pediatrician</option>
            <option value="Orthopedist">Orthopedist</option>
            <option value="General Physician">General Physician</option>
          </select>
        </div>
        <button type="submit" class="search-button">Search</button>
      </form>
    </div>
  `;

  // Append to body
  document.body.appendChild(modal);

  // Event listener to close modal
  modal.querySelector('.close').onclick = function () {
    modal.style.display = 'none';
  };

  // Optional: Close modal when clicking outside content
  window.onclick = function (event) {
    if (event.target === modal) {
      modal.style.display = 'none';
    }
  };

  // Optional: Form submission logic
  document.getElementById('findDoctorsForm').onsubmit = function (e) {
    e.preventDefault();
    const city = document.getElementById('city').value;
    const specialty = document.getElementById('specialty').value;
    console.log(`Searching for ${specialty} in ${city}`);
    // Add your search logic here
  };
}

// Call this function to create and display the modal
createDoctorSearchModal();

// Open Modal
function openFindDoctorsModal() {
  const modal = document.getElementById('findDoctorsModal');
  modal.style.display = 'flex';
}

// Close Modal
function closeFindDoctorsModal() {
  const modal = document.getElementById('findDoctorsModal');
  modal.style.display = 'none';
}

// Handle Form Submission
document.getElementById('findDoctorsForm').addEventListener('submit', function (e) {
  e.preventDefault();

  const city = document.getElementById('city').value.trim();
  const specialty = document.getElementById('specialty').value.trim();

  if (!city || !specialty) {
    alert('Please fill in all fields.');
    return;
  }

  // Generate Practo Search Link
  const practoLink = generatePractoLink(city, specialty);
  window.location.href = practoLink; // Redirect to Practo
});

// Generate Practo Link
function generatePractoLink(city, specialty) {
  const baseUrl = 'https://www.practo.com/search/doctors?results_type=doctor';
  const query = encodeURIComponent(
    JSON.stringify([{ word: specialty, autocompleted: true, category: 'subspeciality' }])
  );
  return `${baseUrl}&q=${query}&city=${encodeURIComponent(city)}`;
}

// Close Modal on Click Outside
window.addEventListener('click', function (e) {
  const modal = document.getElementById('findDoctorsModal');
  if (e.target === modal) {
    closeFindDoctorsModal();
  }
});

// Close Modal on Close Button Click
document.querySelector('.close').addEventListener('click', closeFindDoctorsModal);

// Menu System Implementation
function setupMenu() {
  const menuButton = document.getElementById('menuButton');
  const menuOverlay = document.getElementById('menuOverlay');
  const menuItems = document.querySelectorAll('.menu-item');
  const chatMessages = document.getElementById('chatMessages');
  
  const closeButton = document.createElement('button');
  closeButton.innerHTML = '<i class="fas fa-times"></i>';
  closeButton.className = 'menu-close-button';
  menuOverlay.appendChild(closeButton);

  function toggleMenu(show) {
      menuOverlay.style.display = show ? 'block' : 'none';
      menuOverlay.classList.toggle('fade-in', show);
      document.body.style.overflow = show ? 'hidden' : '';
  }

  menuButton.addEventListener('click', () => toggleMenu(true));
  closeButton.addEventListener('click', () => toggleMenu(false));
  menuOverlay.addEventListener('click', (e) => {
      if (e.target === menuOverlay) toggleMenu(false);
  });

  menuItems.forEach(item => {
      item.addEventListener('click', () => {
          const selectedItem = item.textContent.trim();
          const message = document.createElement('div');
          message.className = 'message bot-message';
          message.innerHTML = `
              <div class="message-content">You selected: ${selectedItem}. How can I assist you further?</div>
          `;
          chatMessages.appendChild(message);
          chatMessages.scrollTop = chatMessages.scrollHeight;
          toggleMenu(false);
      });
  });

  document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') toggleMenu(false);
  });
}

        // Voice Recognition Class
        class VoiceRecognition {
            constructor() {
                this.recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
                this.recognition.continuous = false;
                this.recognition.interimResults = false;
                this.recognition.lang = 'en-US';

                this.recognition.onresult = (event) => {
                    const transcript = event.results[0][0].transcript;
                    this.handleVoiceInput(transcript);
                };

                this.recognition.onerror = (event) => {
                    console.error('Speech recognition error:', event.error);
                    this.showToast('Error: ' + event.error);
                };

                this.recognition.onend = () => {
                    this.recognition.stop();
                };
            }

            start() {
                this.recognition.start();
                this.showToast('Listening...');
            }

            handleVoiceInput(transcript) {
                const chatInput = document.getElementById('chatInput');
                chatInput.value = transcript;
                chatSystem.handleUserInput();
            }

            showToast(message) {
                const toast = document.createElement('div');
                toast.className = 'toast';
                toast.textContent = message;
                document.body.appendChild(toast);

                setTimeout(() => {
                    toast.classList.add('show');
                    setTimeout(() => {
                        toast.classList.remove('show');
                        setTimeout(() => toast.remove(), 300);
                    }, 2000);
                }, 100);
            }
        }

        // Initialize Voice Recognition
        const voiceRecognition = new VoiceRecognition();

        // Add Event Listener to Voice Button
        document.getElementById('voiceButton').addEventListener('click', () => {
            voiceRecognition.start();
        });
     
// Utility function to toggle detailed information
function toggleDetailedInfo(element) {
  const infoPanel = element.closest('.list-item').querySelector('.detailed-info');
  infoPanel.classList.toggle('hidden');
}

function toggleVehicleInfo(element) {
  const infoPanel = element.querySelector('.vehicle-info');
  infoPanel.classList.toggle('hidden');
}

function toggleProviderInfo(element) {
  const infoPanel = element.querySelector('.provider-info');
  infoPanel.classList.toggle('hidden');
}

function toggleFranchiseInfo(element) {
  const infoPanel = element.querySelector('.franchise-info');
  infoPanel.classList.toggle('hidden');
}

function toggleMovieInfo(element) {
  const infoPanel = element.querySelector('.movie-info');
  infoPanel.classList.toggle('hidden');
}

function toggleElaborateInfo(promptResponse) {
    const infoPanel = promptResponse.querySelector('.elaborate-info');
    infoPanel.classList.toggle('hidden');
}

function changeSlide(carousel, direction) {
  const items = carousel.querySelectorAll('.carousel-item');
  let activeIndex = Array.from(items).findIndex(item => item.classList.contains('active'));
  items[activeIndex].classList.remove('active');
  activeIndex = (activeIndex + direction + items.length) % items.length;
  items[activeIndex].classList.add('active');
}

function changeAutomotiveSlide(carousel, direction) {
  const items = carousel.querySelectorAll('.automotive-carousel-item');
  let activeIndex = Array.from(items).findIndex(item => item.classList.contains('active'));
  items[activeIndex].classList.remove('active');
  activeIndex = (activeIndex + direction + items.length) % items.length;
  items[activeIndex].classList.add('active');
}

function togglePropertyInfo(element) {
  const infoPanel = element.querySelector('.property-info');
  infoPanel.classList.toggle('hidden');
}

function toggleLoanInfo(element) {
    const infoPanel = element.querySelector('.loan-info');
    infoPanel.classList.toggle('hidden');
}

// Utility functions
function toggleCouponInfo(element) {
  const infoPanel = element.querySelector('.coupon-info');
  infoPanel.classList.toggle('hidden');
}

function toggleJobInfo(element) {
  const infoPanel = element.querySelector('.job-info');
  infoPanel.classList.toggle('hidden');
}

function toggleCourseInfo(element) {
  const infoPanel = element.querySelector('.course-info');
  infoPanel.classList.toggle('hidden');
}

function toggleDiseaseInfo(element) {
  const infoPanel = element.querySelector('.disease-info');
  infoPanel.classList.toggle('hidden');
}

function copyCouponCode(code) {
  navigator.clipboard.writeText(code).then(() => {
    showToast('Coupon code copied!');
  }).catch(err => {
    showToast('Failed to copy code');
  });
}

function showToast(message) {
  const toast = document.createElement('div');
  toast.className = 'toast';
  toast.textContent = message;
  document.body.appendChild(toast);
  
  setTimeout(() => {
    toast.classList.add('show');
    setTimeout(() => {
      toast.classList.remove('show');
      setTimeout(() => toast.remove(), 300);
    }, 2000);
  }, 100);
}

document.addEventListener('DOMContentLoaded', function() {
  const chatSystem = new ChatSystem();
  chatSystem.initialize();
  setupMenu();

  if (typeof flatpickr !== 'undefined') {
      flatpickr("#dateInput", {
          minDate: "today",
          maxDate: new Date().fp_incr(30),
          dateFormat: "Y-m-d"
      });

      flatpickr("#timeInput", {
          enableTime: true,
          noCalendar: true,
          dateFormat: "H:i",
          minTime: "09:00",
          maxTime: "18:00",
          minuteIncrement: 30
      });
  }
});

const style = document.createElement('style');
style.textContent = `
  .typing-indicator {
      padding: 20px;
  }

  .typing-indicator .dots {
      display: inline-flex;
      gap: 4px;
  }

  .typing-indicator .dots span {
      width: 8px;
      height: 8px;
      background: #666;
      border-radius: 50%;
      animation: bounce 1.4s infinite ease-in-out;
  }

  .typing-indicator .dots span:nth-child(1) { animation-delay: -0.32s; }
  .typing-indicator .dots span:nth-child(2) { animation-delay: -0.16s; }

  @keyframes bounce {
      0%, 80%, 100% { transform: scale(0); }
      40% { transform: scale(1); }
  }

    .message {
    display: flex; 
    gap: 10px; 
    max-width: 100%; 
    margin-bottom: 16px; 
    animation: messageSlide 0.3s ease-out; 
    }

    .bot-message { 
  align-self: flex-start; 
}
   
   .user-message {
        background: #lalala;
        color: white;
        margin-left: auto;
    }

    .message-avatar {
        width: 30px;
        height: 30px;
        background: #lalala;
        color: #FFD700;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .welcome-message ul {
        margin: 10px 0;
        padding-left:20px;
    }

    .fade-out {
  animation: fadeOut 0.3s ease-out forwards;
    }

   @keyframes fadeOut {
   from { opacity: 1; }
   to { opacity: 0; transform: translateY(-20px); }
   }

    .menu-close-button {
        position: absolute;
        top: 15px;
        right: 15px;
        background: #1a1a1f;
        border: none;
        color: #FFD700;
        font-size: 24px;
        cursor: pointer;
        padding: 5px;
        z-index: 1001;
    }

    .menu-close-button:hover {
        color: #FFD700;
    }

/* Action Icons Container */
.action-icons {
  display: flex;
  gap: 12px;
  position: absolute;
  right: 16px;
  top: 16px;
  background: rgba(255, 255, 255, 0.9);
  padding: 8px 12px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  z-index: 10;
  animation: fadeIn 0.2s ease;
}

.action-icons.fade-out {
  animation: fadeOut 0.2s ease forwards;
}

/* Action Icon */
.action-icon {
  display: flex;
  align-items: center;
  gap: 6px;
  cursor: pointer;
  padding: 6px 10px;
  border-radius: 6px;
  background: rgba(0, 0, 0, 0.05);
  transition: all 0.2s ease;
}

.action-icon:hover {
  background: rgba(0, 0, 0, 0.1);
}

.action-icon i {
  font-size: 14px;
  color: #333;
}

.action-icon span {
  font-size: 12px;
  font-weight: 500;
  color: #333;
}

/* Share Icon Specific Styles */
.share-icon i {
  color: #2563eb; /* Blue color for share icon */
}

/* Animations */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeOut {
  from { opacity: 1; transform: translateY(0); }
  to { opacity: 0; transform: translateY(-10px); }
}

/* Toast Messages */
.toast {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%) translateY(100px);
  background: rgba(0, 0, 0, 0.8);
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  transition: transform 0.3s ease;
  z-index: 1000;
}

.toast.show {
  transform: translateX(-50%) translateY(0);
}

.prompt-response {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
  width: 100%; /* Full width */
  margin-left: 0; /* Remove auto margin to align to the edges */
  margin-right: 0; /* Remove auto margin to align to the edges */
}

    .click-buttons {
        display: flex;
        align-items: center;
        gap: 8px;
        margin-top: 10px;
        height: 40px;
        pointer-events: none; /* Prevent action buttons from triggering detailed info */
    }

    .click-button {
        pointer-events: auto; /* Re-enable clicks for action buttons */
        padding: 8px 16px;
        background: linear-gradient(to bottom, #2563eb, #1d4ed8);
        color: white;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        display: flex;
        align-items: center;
        gap: 8px;
        font-family: Poppins;
        font-weight: 500;
        transition: all 0.2s ease;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        height: 40px;
        line-height: 40px;
    }

    .click-button:hover {
        background: linear-gradient(to bottom, #1d4ed8, #1e40af);
        transform: translateY(-1px);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.15);
    }

    .elaborate-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}
    .hidden {
        display: none;
    }

    .elaborate-section {
        margin-bottom: 15px;
    }

    .elaborate-section h3 {
  font-size: 18px; /* Larger font size for section headings on desktop */
  margin-bottom: 8px;
  color: #FFD700;
}

    .elaborate-section ul {
        margin: 0;
        padding-left: 20px;
    }

/* Media Queries for Mobile Devices */
@media (max-width: 480px) {
  .prompt-response {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
}

  .coupons-container {
    display: flex;
    flex-direction: column;
    gap: 16px;
    width: 100%;
    max-width: 100%;
      padding: 0;
  margin: 0;
  }

.coupon-item {
  width: 100%;
  max-width: 100%;
  box-sizing: border-box;
  margin-left: 0;
  margin-right: 0;
    background: #1a1a1f;
    border-radius: 12px;
    padding: 16px;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    border: 1px solid rgba(255, 255, 255, 0.1);
    margin-bottom: 16px; /* Add margin-bottom for extra spacing */
}

  .coupon-item:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  }

/* Update the coupon header styles */
.coupon-header-text h2 {
    font-size: 24px;
    font-weight: 600;
    color: #FFF;
    margin-bottom: 16px;
    border-bottom: none;
    text-decoration: none;
    border: none;
    position: relative;
}

.coupon-header-text h2::after {
    content: none;
}

  .coupon-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
  }

  .brand-info {
    display: flex;
    align-items: center;
    gap: 8px;
    font-weight: 500;
  }

  .brand-info i {
    font-size: 20px;
    color: #FFD700;
  }

  .discount-tag {
    background: linear-gradient(135deg, #2563eb, #1d4ed8);
    color: white;
    padding: 4px 8px;
    margin-left: 18px;
    border-radius: 4px;
    font-weight: 600;
  }

  .coupon-content {
    margin-bottom: 12px;
  }

  .description {
    margin-bottom: 8px;
    color: #fff;
  }

  .code-container {
    display: flex;
    align-items: center;
    gap: 8px;
    margin: 12px 0;
    background: rgba(255, 255, 255, 0.05);
    padding: 8px;
    border-radius: 6px;
  }

  .coupon-code {
    font-family: monospace;
    font-size: 16px;
    letter-spacing: 1px;
    color: #FFD700;
    background: transparent;
  }

  .copy-button {
    background: transparent;
    border: none;
    color: #FFD700;
    cursor: pointer;
    padding: 4px 8px;
    transition: transform 0.2s ease;
  }

  .copy-button:hover {
    transform: scale(1.1);
  }

  .validity {
    font-size: 0.9em;
    color: #888;
  }

  .toast {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%) translateY(100px);
    background: rgba(0, 0, 0, 0.8);
    color: white;
    padding: 12px 24px;
    border-radius: 8px;
    transition: transform 0.3s ease;
    z-index: 1000;
  }

  .toast.show {
    transform: translateX(-50%) translateY(0);
  }

  .coupon-info {
    margin-top: 16px;
    padding-top: 16px;
    border-top: none !important;
  }

  .coupon-info .info-section {
    margin-bottom: 12px;
  }

  .coupon-info h4 {
    font-size: 14.9px;
    margin-left: 0px;
    color: #FFD700;
    margin-bottom: 8px;
  }

  .coupon-info ul {
    font-size: 14px;
    margin-left: 0px;
    list-style: none;
    padding-left: 0;
  }

  .coupon-info li {
    color: #fff;
    margin-bottom: 4px;
    position: relative;
    padding-left: 20px;
  }

  .coupon-info li:before {
    content: '•';
    position: absolute;
    left: 0;
    color: #FFD700;
  }

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

  @keyframes messageSlide {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
  }

  @media (max-width: 480px) {
    .coupons-container {
    width: 82vw;
  }

    .coupon-header-text h2 {
        font-size: 18px; /* Smaller size for mobile */
    }
}

.real-estate-container {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 100%;
  max-width: 540px; /* Drastically reduced width */
  margin: 0 auto;
  padding: 8px; /* Minimal padding */
}

.property-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
  width: 100%; /* Ensure property items take full width */
  box-sizing: border-box; /* Include padding and border in the width */
}

.property-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.property-header {
  display: flex;
  flex-direction: column; /* Stack title and price vertically on mobile */
  gap: 8px; /* Add spacing between title and price */
  margin-bottom: 12px;
}

.real-estate-header h2 {
    font-size: 24px;
    font-weight: 600;
    color: #FFF;
    margin-bottom: 16px;
    border-bottom: none;
    text-decoration: none;
    border: none;
    position: relative;
}

.real-estate-header h2::after {
    content: none;
}

.property-title {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.property-price {
  font-size: 16px;
  color: #fff;
}

.property-details {
  display: flex;
  gap: 12px; /* Space between details */
  margin-bottom: 12px;
  font-size: 18px;
  color: #fff;
  overflow-x: auto; /* Allow horizontal scrolling if needed */
  white-space: nowrap; /* Prevent wrapping */
}

.property-details > div {
  display: flex;
  align-items: center;
  gap: 4px; /* Space between icon and text */
  flex-shrink: 0; /* Prevent shrinking */
}

.property-description {
  font-size: 16px;
  color: #ccc;
  margin-bottom: 12px;
}

.property-contact {
  font-size: 14px;
  color: #fff;
  margin-bottom: 12px;
}

.carousel {
  position: relative;
  width: 100%;
  height: 200px; /* Reduce height for mobile */
  overflow: hidden;
  border-radius: 8px;
  margin-bottom: 12px;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.carousel-item.active {
  opacity: 1;
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  padding: 8px; /* Reduce padding for mobile */
  cursor: pointer;
  font-size: 16px; /* Reduce font size for mobile */
  z-index: 100;
}

.carousel-control.prev {
  left: 8px; /* Adjust position for mobile */
}

.carousel-control.next {
  right: 8px; /* Adjust position for mobile */
}

.property-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h3 {
  font-size: 20px; /* Larger font size for section headings on desktop */
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 18px; /* Larger font size for list items on desktop */
  color: #fff;
  margin-bottom: 4px;
}

.action-buttons {
  display: flex;
  gap: 12px; /* Space between buttons */
  margin-top: 16px;
}

.book-now-button,
.whatsapp-button {
  display: inline-flex; /* Use flex for better alignment */
  align-items: center;
  justify-content: center;
  padding: 10px 16px; /* Adjust padding for better touch targets */
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
  cursor: pointer;
  flex: 1; /* Equal width for both buttons */
}

.book-now-button {
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
}

.whatsapp-button {
  background: linear-gradient(135deg, #25d366, #128c7e);
}

.book-now-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.whatsapp-button:hover {
  background: linear-gradient(135deg, #128c7e, #075e54);
  transform: translateY(-1px);
}

.book-now-button i,
.whatsapp-button i {
  margin-right: 8px;
}

/* Media Queries for Mobile Devices */
@media (max-width: 480px) {
  .real-estate-container {
    width: 85vw;
  }

  .property-item {
    border-radius: 12px;
    margin-left: 0; /* Remove left margin */
    margin-right: 0; /* Remove right margin */
  }

  .property-header {
    flex-direction: column; /* Stack title and price vertically */
  }

    .real-estate-header h2 {
      font-size:  18px; /* Smaller size for mobile */
    }

  .property-details {
    gap: 8px; /* Reduce gap for smaller screens */
    font-size: 12px; /* Reduce font size for smaller screens */
  }

  .property-details > div {
    gap: 2px; /* Reduce gap between icon and text */
  }

    .property-description {
    font-size: 12px; /* Reduce font size for smaller screens */
    margin-bottom: 12px; /* Reduce margin for smaller screens */
    line-height: 1.5; /* Adjust line height for mobile */
  }

  .carousel {
    height: 150px; /* Further reduce height for smaller mobile screens */
  }

  .carousel-control {
    padding: 6px; /* Further reduce padding for smaller screens */
    font-size: 14px; /* Further reduce font size for smaller screens */
  }

    .info-section h3 {
    font-size: 16px; /* Slightly smaller font size for section headings on mobile */
  }

  .info-section li {
    font-size: 14px; /* Smaller font size for list items on mobile */
  }

  .action-buttons {
    flex-direction: row; /* Ensure buttons stay horizontal */
    gap: 8px; /* Reduce gap for smaller screens */
  }

  .book-now-button,
  .whatsapp-button {
    padding: 8px 12px; /* Adjust padding for smaller screens */
    font-size: 14px; /* Reduce font size for smaller screens */
  }
}

/* Default styles for jobs container */
.jobs-container {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 100%;
  max-width: 540px; /* Drastically reduced width */
  margin: 0 auto;
  padding: 8px; /* Minimal padding */
}

/* Default styles for job items */
.job-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
  width: 100%; /* Full width */
  margin-left: 0; /* Remove auto margin to align to the edges */
  margin-right: 0; /* Remove auto margin to align to the edges */
}

.job-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.job-header {
  display: flex;
  flex-direction: column; /* Stack title and company vertically on mobile */
  gap: 8px; /* Add spacing between title and company */
  margin-bottom: 12px;
}

.job-header-text h2 {
    font-size: 24px;
    font-weight: 600;
    color: #FFF;
    margin-bottom: 16px;
    border-bottom: none;
    text-decoration: none;
    border: none;
    position: relative;
}

.job-header-text h2::after {
    content: none;
}

.job-title {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.job-company {
  font-size: 14px;
  color: #888;
}

.job-details {
  display: flex;
  flex-wrap: wrap; /* Allow details to wrap on smaller screens */
  gap: 8px; /* Reduce gap for mobile */
  margin-bottom: 12px;
  font-size: 14px;
  color: #fff;
}

.job-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.apply-button {
  display: inline-block;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.apply-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.job-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h3 {
  font-size: 18px; /* Larger font size for section headings on desktop */
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 16px; /* Larger font size for list items on desktop */
  color: #fff;
  margin-bottom: 8px; /* Increase spacing between list items */
  line-height: 1.6; /* Improve readability with more line spacing */
}

/* Media Queries for Mobile Devices */
@media (max-width: 480px) {
  .job-item {
    width: 85vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }

  .job-header {
    flex-direction: column; /* Stack title and company vertically */
    gap: 4px; /* Reduce gap for smaller screens */
  }

 .job-header-text h2 {
    font-size: 18px; /* Smaller size for mobile */
  }

  .job-title {
    font-size: 16px; /* Reduce font size for smaller screens */
  }

  .job-company {
    font-size: 12px; /* Reduce font size for smaller screens */
  }

  .job-details {
    flex-direction: column; /* Stack details vertically */
    gap: 4px; /* Reduce gap for smaller screens */
    font-size: 12px; /* Reduce font size for smaller screens */
  }

  .job-description {
    font-size: 12px; /* Reduce font size for smaller screens */
    margin-bottom: 8px; /* Reduce margin for smaller screens */
  }

  .apply-button {
    padding: 6px 12px; /* Adjust padding for smaller screens */
    font-size: 14px; /* Reduce font size for smaller screens */
  }

  .info-section h3 {
    font-size: 16px; /* Slightly smaller font size for section headings on mobile */
  }

  .info-section li {
    font-size: 14px; /* Smaller font size for list items on mobile */
    margin-bottom: 6px; /* Adjust spacing for smaller screens */
    line-height: 1.5; /* Adjust line height for mobile */
  }
}

.automotive-response {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 100%;
  max-width: 540px; /* Drastically reduced width */
  margin: 0 auto;
  padding: 8px; /* Minimal padding */
}

.automotive-response p {
  margin-top: -20px;
  font-size: 18px; /* Minimal font size */
  font-weight: 600;
  color: #FFF;
}

.vehicles-list {
  display: flex;
  flex-direction: column;
  gap: 8px; /* Minimal gap */
  width: 100%;
}

.vehicle-item {
  background: #1a1a1f;
  border-radius: 12px; /* Reduced radius */
  padding: 10px; /* Minimal padding */
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  max-width: 520px; /* Much smaller width */
  margin: 0 auto;
}

.vehicle-item:hover {
  transform: translateY(-1px);
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.2);
}

.vehicle-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4px; /* Minimal margin */
}

.vehicle-title {
  font-size: 15px; /* Minimal title size */
  font-weight: 600;
  color: #FFD700;
}

.vehicle-price {
  font-size: 14px; /* Minimal font size */
  color: #fff;
  font-weight: 500;
}

.vehicle-details {
  display: flex;
  gap: 10px; /* Reduced gap */
  margin-bottom: 6px; /* Minimal margin */
  font-size: 12px; /* Very small font */
  color: #fff;
}

.vehicle-description {
  font-size: 12px; /* Very small font */
  color: #ccc;
  margin-bottom: 6px; /* Minimal margin */
  line-height: 1.3; /* Compact line height */
}

.vehicle-contact {
  font-size: 12px; /* Very small font */
  color: #fff;
  margin-bottom: 6px; /* Minimal margin */
}

.vehicle-info {
  margin-top: 8px; /* Minimal margin */
  padding: 10px; /* Minimal padding */
  background: #1a1a1f;
  border-radius: 5px; /* Reduced radius */
  clear: both;
  animation: fadeIn 0.3s ease-out;
  max-width: 520px; /* Much smaller width */
  margin-left: auto;
  margin-right: auto;
}

.vehicle-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 8px; /* Minimal margin */
}

.info-section h3 {
  font-size: 14px; /* Minimal font size */
  margin-bottom: 4px; /* Minimal margin */
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 12px; /* Minimal padding */
}

.info-section li {
  font-size: 12px; /* Very small font */
  color: #fff;
  margin-bottom: 3px; /* Minimal margin */
  line-height: 1.2; /* Very compact */
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

/* Drastically Reduced Carousel */
.automotive-carousel {
  position: relative;
  width: 100%;
  height: 220px; /* Much smaller height */
  overflow: hidden;
  border-radius: 6px; /* Reduced radius */
  margin-bottom: 8px; /* Minimal margin */
  max-width: 420px; /* Much smaller width */
  margin-left: auto;
  margin-right: auto;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.2);
}

.automotive-carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.automotive-carousel-item.active {
  opacity: 1;
}

.automotive-carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  background-color: #121216;
}

.automotive-carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.7);
  color: white;
  border: none;
  padding: 4px; /* Minimal padding */
  cursor: pointer;
  font-size: 12px; /* Very small font */
  z-index: 100;
  border-radius: 50%;
  width: 24px; /* Very small control */
  height: 24px; /* Very small control */
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.automotive-carousel-control:hover {
  background: rgba(0, 0, 0, 0.9);
}

.automotive-carousel-control.prev {
  left: 8px; /* Minimal spacing */
}

.automotive-carousel-control.next {
  right: 8px; /* Minimal spacing */
}

.action-buttons {
  display: flex;
  gap: 8px; /* Minimal gap */
  margin-top: 8px; /* Minimal margin */
  justify-content: center;
}

.book-now-button,
.whatsapp-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 4px; /* Minimal gap */
  padding: 5px 10px; /* Minimal padding */
  color: white;
  border: none;
  border-radius: 4px; /* Reduced radius */
  text-align: center;
  text-decoration: none;
  font-weight: 600;
  font-size: 12px; /* Very small font */
  transition: all 0.2s ease;
  cursor: pointer;
  min-width: 100px; /* Much smaller width */
}

.book-now-button {
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
}

.whatsapp-button {
  background: linear-gradient(135deg, #25d366, #128c7e);
}

.book-now-button:hover,
.whatsapp-button:hover {
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

@media (min-width: 481px) {
  .automotive-response {
    width: 85vw;
  }
  
  .vehicle-item {
    border-radius: 10px;
    padding: 16px;
  }
  
  .automotive-carousel {
    height: 300px;
  }
}

@media (min-width: 1080px) {
  .automotive-response {
    margin-left: 450px;
  }
}

/* Media Queries for Mobile Devices */
@media (max-width: 480px) {
  .vehicle-item {
    width: 85vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }

  /* Automotive Carousel Styles */
.automotive-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 8px;
  margin-bottom: 12px;
}

  .action-buttons {
    flex-direction: row;
    gap: 8px;
  }

  .book-now-button,
  .whatsapp-button {
    padding: 8px 8px;
    font-size: 14px;
  }
}  

.disease-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  padding: 0;
  margin: 0;
}

.disease-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
}

.disease-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.disease-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.disease-name {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.disease-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.action-buttons {
  display: flex;
  gap: 12px;
  margin-top: 16px;
}

.action-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.action-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.disease-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.disease-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h3 {
  font-size: 16px;
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 14px;
  color: #fff;
  margin-bottom: 4px;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@media (max-width: 480px) {
  .disease-item {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
  .action-button {
    padding: 6px 12px; /* Smaller padding for mobile */
    font-size: 12px; /* Smaller font size for mobile */
    gap: 4px; /* Reduce gap between icon and text */
  }

  .action-button i {
    font-size: 12px; /* Smaller icon size for mobile */
  }
}

.domain-registration-response {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  margin-bottom: 16px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.domain-registration-response p {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
  margin-bottom: 12px;
}

.domain-registration-response ul {
  margin: 0;
  padding-left: 20px;
  color: #fff;
  margin-bottom: 12px;
}

.train-booking-response button,
.flight-booking-response button,
.domain-registration-response button,
.bus-booking-response button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.2s ease;
}

.train-booking-response button:hover,
.flight-booking-response button:hover,
.domain-registration-response button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

/* Modal Styles */
.modal {
  display: none;
  position: fixed;
  z-index: 1000;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: #1a1a1f;
  padding: 20px;
  border-radius: 12px;
  width: 90%;
  max-width: 400px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.modal-content h2 {
  font-size: 24px;
  font-weight: 600;
  color: #FFD700;
  margin-bottom: 16px;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
}

.close:hover {
  color: #fff;
}

.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  font-size: 14px;
  color: #fff;
  margin-bottom: 8px;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 8px;
  border-radius: 6px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  background: #2a2a2f;
  color: #fff;
  font-size: 14px;
}

.search-button {
  width: 100%;
  padding: 10px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.search-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.movie-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  padding: 0;
  margin: 0;
}

.movie-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
}

.movie-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.movie-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.movie-title {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.movie-release-date {
  font-size: 14px;
  color: #fff;
}

.movie-details {
  display: flex;
  gap: 12px;
  margin-bottom: 12px;
  font-size: 14px;
  color: #fff;
}

.movie-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.action-buttons {
  display: flex;
  gap: 12px;
  margin-top: 16px;
}

.action-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.action-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.movie-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.movie-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h4 {
  font-size: 16px;
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 14px;
  color: #fff;
  margin-bottom: 4px;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@media (max-width: 480px) {
  .movie-item {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
}

.franchise-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  padding: 0;
  margin: 0;
}

.franchise-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
}

.franchise-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.franchise-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.franchise-name {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.franchise-category {
  font-size: 14px;
  color: #fff;
}

.franchise-details {
  display: flex;
  gap: 12px;
  margin-bottom: 12px;
  font-size: 14px;
  color: #fff;
}

.franchise-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.action-buttons {
  display: flex;
  gap: 12px;
  margin-top: 16px;
}

.action-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.action-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.franchise-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.franchise-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h4 {
  font-size: 16px;
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 14px;
  color: #fff;
  margin-bottom: 4px;
}

@media (max-width: 480px) {
  .franchise-item {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
}

.courses-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  padding: 0;
  margin: 0;
}

.course-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
}

.course-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.course-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.course-name {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.course-category {
  font-size: 14px;
  color: #fff;
}

.course-details {
  display: flex;
  gap: 12px;
  margin-bottom: 12px;
  font-size: 14px;
  color: #fff;
}

.course-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.action-buttons {
  display: flex;
  gap: 12px;
  margin-top: 16px;
}

.action-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.action-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.course-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.course-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h4 {
  font-size: 16px;
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 14px;
  color: #fff;
  margin-bottom: 4px;
}

@media (max-width: 480px) {
  .course-item {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
}

.loan-container {
    display: flex;
    flex-direction: column;
    gap: 16px;
    width: 100%;
    padding: 0;
    margin: 0;
}

.loan-item {
    background: #1a1a1f;
    border-radius: 12px;
    padding: 16px;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    border: 1px solid rgba(255, 255, 255, 0.1);
    margin-bottom: 16px;
}

.loan-item:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.loan-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
}

.loan-name {
    font-size: 18px;
    font-weight: 600;
    color: #FFD700;
}

.loan-type {
    font-size: 14px;
    color: #fff;
}

.loan-details {
    display: flex;
    gap: 12px;
    margin-bottom: 12px;
    font-size: 14px;
    color: #fff;
}

.loan-description {
    font-size: 14px;
    color: #ccc;
    margin-bottom: 12px;
}

.apply-now-button {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 8px 16px;
    background: linear-gradient(135deg, #2563eb, #1d4ed8);
    color: white;
    border: none;
    border-radius: 6px;
    text-align: center;
    text-decoration: none;
    font-weight: 500;
    transition: all 0.2s ease;
}

.apply-now-button:hover {
    background: linear-gradient(135deg, #1d4ed8, #1e40af);
    transform: translateY(-1px);
}

.loan-info {
    margin-top: 16px;
    padding: 16px;
    background: #1a1a1f;
    border-radius: 4px;
    clear: both;
    animation: fadeIn 0.3s ease-out;
}

.loan-info.hidden {
    display: none;
}

.info-section {
    margin-bottom: 16px;
}

.info-section h4 {
    font-size: 16px;
    margin-bottom: 8px;
    color: #FFD700;
}

.info-section ul {
    margin: 0;
    padding-left: 20px;
}

.info-section li {
    font-size: 14px;
    color: #fff;
    margin-bottom: 4px;
}

@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

@media (max-width: 480px) {
    .loan-item {
        width: 82vw;
        padding: 12px;
        border-radius: 12px;
        margin-bottom: 12px;
    }
}

      `;

document.head.appendChild(style);


const nysaFeedData = [
  // Service Providers
  {
    id: "feed-001",
    category: "Service Providers",
    details: {
      title: "Premium Home Cleaning Services",
      headerImage: "https://logodownload.org/wp-content/uploads/2021/07/dominos-pizza-logo-0.png",
      detailImage: "https://logodownload.org/wp-content/uploads/2021/07/dominos-pizza-logo-0.png",
      images: [
        "https://images.unsplash.com/photo-1581578731548-c64695cc6952?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1600585152220-90363fe7e115?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1513694203232-719a280e022f?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Professional home cleaning services with eco-friendly products. Serving all areas of the city with trained staff.",
      provider: "CleanMaster Pro",
      location: "New Delhi",
      datePosted: "2 hours ago",
      price: "₹499 per hour",
      contact: "+91 9876543210",
      content: `
        <h2>About Our Service</h2>
        <p>We provide top-quality home cleaning services with 100% satisfaction guarantee. Our team is trained in modern cleaning techniques and uses only eco-friendly products.</p>
        
        <h3>Service Details</h3>
        <ul>
          <li>Deep cleaning for homes and offices</li>
          <li>Regular maintenance cleaning packages</li>
          <li>Specialized kitchen and bathroom cleaning</li>
          <li>Carpet and sofa cleaning</li>
          <li>Post-renovation cleaning</li>
        </ul>
        
        <h3>Why Choose Us?</h3>
        <ul>
          <li>5 years of trusted service</li>
          <li>300+ satisfied customers</li>
          <li>Verified and background-checked staff</li>
          <li>Flexible scheduling</li>
          <li>Affordable pricing</li>
        </ul>
        
        <h3>Service Areas</h3>
        <p>All major areas of New Delhi including South Delhi, West Delhi, and Noida.</p>
      `,
      tags: ["cleaning", "home services", "professional"],
      actionButton: {
        text: "Book Now",
        url: "#"
      },
      social: {
        website: "https://cleanmasterpro.com",
        whatsapp: "https://wa.me/919876543210"
      }
    }
  },
  {
    id: "feed-002",
    category: "Healthcare",
    details: {
      title: "Dr. Sharma's Dental Clinic",
      headerImage: "https://example.com/providers/dr-sharma-header.jpg",
      detailImage: "https://example.com/providers/dr-sharma-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1588776814546-1ffcf47267a5?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1588776814546-1ffcf47267a5?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Experienced dental care with modern equipment. Specializing in cosmetic dentistry and dental implants.",
      provider: "Dr. Rajesh Sharma",
      location: "Mumbai",
      datePosted: "1 day ago",
      price: "Consultation: ₹500",
      contact: "+91 8765432109",
      content: `
        <h2>About Our Clinic</h2>
        <p>Dr. Sharma's Dental Clinic has been serving patients for over 15 years with state-of-the-art dental care facilities.</p>
        
        <h3>Services Offered</h3>
        <ul>
          <li>Teeth Whitening</li>
          <li>Dental Implants</li>
          <li>Root Canal Treatment</li>
          <li>Braces and Aligners</li>
          <li>General Dentistry</li>
        </ul>
        
        <h3>Clinic Hours</h3>
        <p>Monday to Saturday: 9:00 AM - 8:00 PM<br>
        Sunday: Emergency only</p>
        
        <h3>Our Team</h3>
        <p>Led by Dr. Rajesh Sharma (MDS) with 3 supporting dentists and 5 dental hygienists.</p>
      `,
      tags: ["dentist", "healthcare", "mumbai"],
      actionButton: {
        text: "Book Appointment",
        url: "#"
      },
      social: {
        website: "https://drsharmadental.com",
        instagram: "https://instagram.com/drsharmadental"
      }
    }
  },
  {
    id: "feed-003",
    category: "Jobs",
    details: {
      title: "Marketing Manager - Digital Marketing",
      headerImage: "https://example.com/providers/techsolutions-header.jpg",
      detailImage: "https://example.com/providers/techsolutions-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1551288049-bebda4e38f71?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Looking for an experienced Digital Marketing Manager to lead our marketing team. Minimum 5 years experience required.",
      provider: "TechSolutions Pvt. Ltd.",
      location: "Bangalore",
      datePosted: "3 days ago",
      salary: "₹8-12 LPA",
      requirements: `
        <h3>Requirements</h3>
        <ul>
          <li>Bachelor's degree in Marketing or related field</li>
          <li>5+ years digital marketing experience</li>
          <li>Expertise in SEO, SEM, and social media</li>
          <li>Analytical skills and data-driven thinking</li>
          <li>Excellent communication skills</li>
        </ul>
        
        <h3>Responsibilities</h3>
        <ul>
          <li>Develop digital marketing strategy</li>
          <li>Manage all digital marketing channels</li>
          <li>Measure and report performance</li>
          <li>Identify trends and insights</li>
          <li>Optimize spend and performance</li>
        </ul>
      `,
      tags: ["job", "marketing", "remote"],
      actionButton: {
        text: "Apply Now",
        url: "#"
      },
      social: {
        website: "https://techsolutions.com/careers",
        linkedin: "https://linkedin.com/company/techsolutions"
      }
    }
  },
  {
    id: "feed-004",
    category: "Real Estate",
    details: {
      title: "3BHK Luxury Apartment in Gurgaon",
      headerImage: "https://example.com/providers/elite-properties-header.jpg",
      detailImage: "https://example.com/providers/elite-properties-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1580587771525-78b9dba3b914?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1493809842364-78817add7ffb?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Spacious 3BHK apartment in premium society with all modern amenities. Ready to move in.",
      provider: "Elite Properties",
      location: "Gurgaon, Sector 56",
      datePosted: "1 week ago",
      price: "₹2.5 Cr",
      specs: "2100 sq.ft | 3 Bed | 3 Bath | 2 Balcony",
      content: `
        <h2>Property Details</h2>
        <p>This luxurious apartment offers premium living in one of Gurgaon's most sought-after locations.</p>
        
        <h3>Key Features</h3>
        <ul>
          <li>Spacious living/dining area</li>
          <li>Modern modular kitchen</li>
          <li>Master bedroom with walk-in closet</li>
          <li>3 bathrooms with premium fittings</li>
          <li>Floor-to-ceiling windows</li>
        </ul>
        
        <h3>Society Amenities</h3>
        <ul>
          <li>24/7 security and CCTV</li>
          <li>Swimming pool</li>
          <li>Clubhouse</li>
          <li>Gymnasium</li>
          <li>Children's play area</li>
          <li>Power backup</li>
        </ul>
        
        <h3>Location Advantages</h3>
        <ul>
          <li>5 mins from Delhi-Jaipur Highway</li>
          <li>10 mins from Metro Station</li>
          <li>Close to top schools and hospitals</li>
          <li>Near shopping malls and restaurants</li>
        </ul>
      `,
      tags: ["real estate", "apartment", "gurgaon"],
      actionButton: {
        text: "Schedule Visit",
        url: "#"
      },
      social: {
        website: "https://eliteproperties.in",
        whatsapp: "https://wa.me/919876543210"
      }
    }
  },
  {
    id: "feed-005",
    category: "Government Schemes",
    details: {
      title: "PM Awas Yojana - Housing for All",
      headerImage: "https://example.com/providers/govt-scheme-header.jpg",
      detailImage: "https://example.com/providers/govt-scheme-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1560518883-ce09059eeffa?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Affordable housing scheme for urban poor. Apply now to get benefits under this government scheme.",
      provider: "Ministry of Housing",
      datePosted: "2 weeks ago",
      deadline: "31 December 2024",
      content: `
        <h2>Scheme Details</h2>
        <p>Pradhan Mantri Awas Yojana (Urban) aims to provide housing for all in urban areas by 2024.</p>
        
        <h3>Benefits</h3>
        <ul>
          <li>Interest subsidy of up to ₹2.67 lakh</li>
          <li>Credit Linked Subsidy Scheme (CLSS)</li>
          <li>Affordable housing in partnership</li>
          <li>Benefit for slum dwellers</li>
        </ul>
        
        <h3>Eligibility</h3>
        <ul>
          <li>Family income up to ₹18 lakh per annum</li>
          <li>No pucca house in name of any family member</li>
          <li>Beneficiary family should not have availed central assistance</li>
        </ul>
        
        <h3>How to Apply</h3>
        <ol>
          <li>Visit official website pmaymis.gov.in</li>
          <li>Click on "Citizen Assessment"</li>
          <li>Fill the application form</li>
          <li>Upload required documents</li>
          <li>Submit application</li>
        </ol>
      `,
      tags: ["government", "housing", "scheme"],
      actionButton: {
        text: "Apply Now",
        url: "https://pmaymis.gov.in"
      }
    }
  },
  {
    id: "feed-006",
    category: "Education",
    details: {
      title: "CBSE Board Exam Preparation Classes",
      headerImage: "https://example.com/providers/excel-academy-header.jpg",
      detailImage: "https://example.com/providers/excel-academy-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Specialized coaching for CBSE Class 10 and 12 students. Experienced faculty and small batch sizes.",
      provider: "Excel Academy",
      location: "Hyderabad",
      datePosted: "5 days ago",
      price: "₹3000/month",
      content: `
        <h2>About Our Program</h2>
        <p>Excel Academy has been helping students achieve academic excellence for over 10 years with our specialized CBSE coaching program.</p>
        
        <h3>Subjects Offered</h3>
        <ul>
          <li>Mathematics</li>
          <li>Science (Physics, Chemistry, Biology)</li>
          <li>Social Studies</li>
          <li>English</li>
          <li>Hindi</li>
        </ul>
        
        <h3>Features</h3>
        <ul>
          <li>Small batch sizes (max 15 students)</li>
          <li>Weekly tests and progress reports</li>
          <li>Doubt clearing sessions</li>
          <li>Study material provided</li>
          <li>Mock board exams</li>
        </ul>
        
        <h3>Faculty</h3>
        <p>Our faculty includes retired CBSE examiners and subject matter experts with 10+ years teaching experience.</p>
      `,
      tags: ["education", "cbse", "coaching"],
      actionButton: {
        text: "Enquire Now",
        url: "#"
      },
      social: {
        website: "https://excelacademy.in",
        facebook: "https://facebook.com/excelacademyhyd"
      }
    }
  }
];

function showNysaFeedOverlay() {
  let overlay = document.getElementById('nysaFeedOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'nysaFeedOverlay';
    overlay.className = 'nysa-feed-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(nysaFeedData.map(feed => feed.category))];

  overlay.innerHTML = `
    <div class="nysa-feed-content">
      <div class="nysa-feed-header">
        <h2>Nysa Feed</h2>
        <button class="close-nysa-feed" onclick="hideNysaFeedOverlay()">
          <i class="fas fa-times"></i>
        </button>
      </div>
      
      <div class="nysa-feed-filter-container">
        <div class="nysa-feed-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterNysaFeed('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="nysa-feed-grid" id="nysaFeedGrid">
        ${renderNysaFeedCards(nysaFeedData)}
      </div>
    </div>

    <div class="nysa-feed-search-container">
      <div class="input-container">
        <input type="text" id="nysaFeedSearchInput" placeholder="Search services, jobs, properties..." />
        <button class="voice-search" id="nysaFeedVoiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const searchInput = document.getElementById('nysaFeedSearchInput');
  const nysaFeedGrid = document.getElementById('nysaFeedGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performNysaFeedSearch(nysaFeedData, query, nysaFeedGrid, filterButtons, renderNysaFeedCards);
  }, 300);

  searchInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  searchInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performNysaFeedSearch(nysaFeedData, searchInput.value, nysaFeedGrid, filterButtons, renderNysaFeedCards);
    }
  });

  initializeNysaFeedVoiceSearch(searchInput, (query) => {
    performNysaFeedSearch(nysaFeedData, query, nysaFeedGrid, filterButtons, renderNysaFeedCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
      :root {
      --service-providers: #4CAF50;
      --healthcare: #F44336;
      --jobs: #2196F3;
      --real-estate: #FF9800;
      --government-schemes: #9C27B0;
      --education: #00BCD4;
      --primary-color: #FFD700;
      --background-dark: #1a1a1f;
      --card-background: rgba(255, 255, 255, 0.03);
      --border-color: rgba(255, 255, 255, 0.1);
      --text-primary: #FFFFFF;
      --text-secondary: rgba(255, 255, 255, 0.8);
      --text-tertiary: rgba(255, 255, 255, 0.6);
      --transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
      --border-radius: 12px;
    }

    .nysa-feed-overlay {
      position: fixed;
      top: 60px;
      left: 0;
      right: 0;
      bottom: 60px;
      background: #1a1a1f;
      z-index: 1000;
      opacity: 0;
      visibility: hidden;
      transition: all 0.3s ease;
      overflow-y: auto;
      width: 100%;
      display: flex;
      flex-direction: column;
      scrollbar-width: none;
      -ms-overflow-style: none;
    }

    .nysa-feed-overlay::-webkit-scrollbar {
      display: none;
    }

    .nysa-feed-overlay.active {
      opacity: 1;
      visibility: visible;
    }

    .nysa-feed-content {
      flex: 1;
      width: 100%;
      max-width: 100%;
      padding: 20px;
      margin: 0 auto;
      position: relative;
    }

    /* Original Header Style */
    .nysa-feed-header {
      padding: 16px 24px;
      background: rgba(26, 26, 31, 0.95);
      position: fixed;
      width: 100%;
      top: 0;
      left: 0;
      z-index: 30;
      border-bottom: 1px solid rgba(255, 215, 0, 0.15);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      height: 72px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }

    .nysa-feed-header h2 {
      color: var(--primary-color);
      margin: 0;
      font-size: 1.75em;
      font-weight: 600;
      letter-spacing: -0.02em;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .nysa-feed-header h2::before {
      content: '';
      display: inline-block;
      width: 24px;
      height: 24px;
      background: var(--primary-color);
      mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M22 12h-4l-3 9L9 3l-3 9H2'%3E%3C/path%3E%3C/svg%3E");
      mask-repeat: no-repeat;
      mask-position: center;
      mask-size: contain;
    }

    .close-nysa-feed {
      background: rgba(255, 255, 255, 0.1);
      border: none;
      color: white;
      font-size: 1em;
      cursor: pointer;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s ease;
    }

    .close-nysa-feed:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: scale(1.05);
    }

    .close-nysa-feed:active {
      transform: scale(0.95);
    }

.nysa-feed-filter-container {
  position: fixed;
  top: 72px;
  left: 0;
  right: 0;
  width: 100%;
  z-index: 10;
  background: var(--background-dark);
  padding: 12px 0;
  box-sizing: border-box;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
  border-bottom: 1px solid rgba(255, 215, 0, 0.1);
}

.nysa-feed-filter-container::-webkit-scrollbar {
  display: none;
}

.nysa-feed-filter-buttons {
  display: inline-flex;
  padding: 0 16px;
  gap: 8px;
  white-space: nowrap;
}

.filter-button {
  font-family: poppins;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-primary);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 20px;
  border-radius: 25px;
  transition: var(--transition);
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  margin: 0;
}

.filter-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: scale(1.03);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: #000;
  font-weight: 600;
  border-color: var(--primary-color);
  box-shadow: 0 4px 10px rgba(255, 215, 0, 0.3);
}

/* Category-specific colors */
.filter-button[data-category="service-providers"] {
  background: rgba(76, 175, 80, 0.1);
  border-color: rgba(76, 175, 80, 0.2);
}
.filter-button[data-category="healthcare"] {
  background: rgba(244, 67, 54, 0.1);
  border-color: rgba(244, 67, 54, 0.2);
}
.filter-button[data-category="jobs"] {
  background: rgba(33, 150, 243, 0.1);
  border-color: rgba(33, 150, 243, 0.2);
}
.filter-button[data-category="real-estate"] {
  background: rgba(255, 152, 0, 0.1);
  border-color: rgba(255, 152, 0, 0.2);
}
.filter-button[data-category="government-schemes"] {
  background: rgba(156, 39, 176, 0.1);
  border-color: rgba(156, 39, 176, 0.2);
}
.filter-button[data-category="education"] {
  background: rgba(0, 188, 212, 0.1);
  border-color: rgba(0, 188, 212, 0.2);
}

/* Add some space at the end for better scrolling */
.nysa-feed-filter-buttons::after {
  content: '';
  display: block;
  min-width: 16px;
  height: 1px;
}

    /* Original Search Container Style */
    .nysa-feed-search-container {
      position: fixed;
      bottom: 0px;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      display: flex;
      gap: 12px;
      align-items: center;
      box-sizing: border-box;
    }

    .input-container {
      flex: 1;
      position: relative;
      display: flex;
      align-items: center;
    }

    #nysaFeedSearchInput {
      flex: 1;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 12px 50px 12px 20px;
      color: #fff;
      font-size: 0.95em;
      width: 100%;
      transition: border-color 0.3s ease;
    }

    #nysaFeedSearchInput:focus {
      border-color: rgba(255, 255, 255, 0.5);
    }

    .voice-search {
      position: absolute;
      right: 10px;
      background: none;
      border: none;
      color: var(--primary-color);
      cursor: pointer;
      padding: 10px;
      transition: all 0.3s ease;
    }

    .voice-search:hover {
      opacity: 0.8;
    }

    .voice-search.active {
      color: #ffd700;
      animation: pulse 1.5s infinite;
    }

    .voice-search i { 
      font-size: 1.5em; 
    }

    /* Rest of the styles remain the same as your improved version */
    .nysa-feed-grid {
      margin-top: 70px;
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(450px, 1fr));
      gap: 20px;
      animation: fadeInUp 0.5s ease-out;
      padding-bottom: 140px;
      width: 100%;
    }

    .nysa-feed-card {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 12px;
      overflow: hidden;
      border: 1px solid rgba(255, 255, 255, 0.1);
      position: relative;
    }

    .nysa-feed-card:hover {
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
      border-color: rgba(255, 215, 0, 0.3);
    }

    .feed-card-header {
      display: flex;
      align-items: center;
      padding: 12px 16px;
      background: rgba(255, 255, 255, 0.03);
      border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    }

    .feed-provider-avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      object-fit: cover;
      margin-right: 12px;
      border: 1px solid rgba(255, 215, 0, 0.3);
    }

    .feed-provider-info {
      flex: 1;
    }

    .feed-provider-name {
      font-weight: 600;
      font-size: 0.95em;
      margin-bottom: 2px;
    }

.feed-meta-badges {
  display: flex;
  gap: 12px;
  font-size: 0.78em;
  margin-top: 4px;
}

.time-badge,
.location-badge {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  border-radius: 16px;
  background: linear-gradient(135deg, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0.05) 100%);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border: 1px solid rgba(255, 215, 0, 0.15);
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  position: relative;
  overflow: hidden;
}

/* Glow effect on hover */
.time-badge:hover,
.location-badge:hover {
  background: linear-gradient(135deg, rgba(255,255,255,0.15) 0%, rgba(255,255,255,0.08) 100%);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.1), 0 0 0 1px rgba(255,215,0,0.3);
}

/* Pseudo-element for subtle shine effect */
.time-badge::before,
.location-badge::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 50%;
  height: 100%;
  background: linear-gradient(
    to right,
    transparent 0%,
    rgba(255, 255, 255, 0.1) 50%,
    transparent 100%
  );
  transition: all 0.6s ease;
}

.time-badge:hover::before,
.location-badge:hover::before {
  left: 100%;
}

.time-badge i,
.location-badge i {
  font-size: 0.85em;
  color: var(--primary-color);
  transition: transform 0.3s ease;
}

.time-badge:hover i,
.location-badge:hover i {
  transform: scale(1.1);
  filter: drop-shadow(0 0 4px rgba(255,215,0,0.4));
}

.time-badge span,
.location-badge span {
  font-weight: 500;
  letter-spacing: 0.02em;
  color: rgba(255,255,255,0.9);
  text-shadow: 0 1px 2px rgba(0,0,0,0.1);
}

/* Unique colors for each badge type */
.time-badge {
  --badge-accent: rgba(100, 210, 255, 0.2);
}

.location-badge {
  --badge-accent: rgba(255, 140, 100, 0.2);
}

.time-badge {
  background: linear-gradient(135deg, 
    rgba(100, 210, 255, 0.1) 0%, 
    rgba(100, 210, 255, 0.05) 100%);
  border-color: rgba(100, 210, 255, 0.2);
}

.location-badge {
  background: linear-gradient(135deg, 
    rgba(255, 140, 100, 0.1) 0%, 
    rgba(255, 140, 100, 0.05) 100%);
  border-color: rgba(255, 140, 100, 0.2);
}

/* Animation for initial appearance */
@keyframes badgeEntrance {
  0% {
    opacity: 0;
    transform: translateY(5px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

.time-badge,
.location-badge {
  animation: badgeEntrance 0.4s ease-out both;
}

.location-badge {
  animation-delay: 0.1s;
}

/* Image Slider Container */
.feed-card-image-container {
  position: relative;
  width: 100%;
  height: 450px;
  overflow: hidden;
  cursor: grab;
}

.feed-card-image-container:active {
  cursor: grabbing;
}

.feed-card-image-slider {
  display: flex;
  width: 100%;
  height: 100%;
  transition: transform 0.4s cubic-bezier(0.22, 1, 0.36, 1);
}

.feed-card-slide {
  min-width: 100%;
  height: 100%;
  position: relative;
}

.feed-card-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.nysa-feed-card:hover .feed-card-image {
  transform: scale(1.03);
}

.feed-card-carousel {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 8px;
  z-index: 2;
  padding: 6px 12px;
  background: rgba(0, 0, 0, 0.4);
  border-radius: 20px;
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
}

.carousel-indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  transition: all 0.3s ease;
  cursor: pointer;
}

.carousel-indicator.active {
  background: var(--primary-color);
  transform: scale(1.2);
}
  
    .feed-card-content {
      padding: 16px;
    }

    .feed-card-title {
      font-size: 1.2em;
      font-weight: 700;
      margin: 0 0 8px 0;
      color: white;
      line-height: 1.3;
    }

    .feed-card-description {
      font-size: 0.95em;
      color: rgba(255, 255, 255, 0.8);
      margin-bottom: 12px;
      line-height: 1.5;
    }

    .feed-card-meta {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
    }

    .feed-card-price {
      font-size: 1.1em;
      font-weight: 600;
      color: var(--primary-color);
    }

    .feed-card-location {
      font-size: 0.9em;
      color: rgba(255, 255, 255, 0.6);
      display: flex;
      align-items: center;
    }

    .feed-card-location i {
      margin-right: 5px;
    }

    .feed-card-actions {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-top: 12px;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
    }

    .feed-action-button {
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
      border: none;
      padding: 8px 16px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .feed-action-button:hover {
      background: rgba(255, 215, 0, 0.2);
    }

    .feed-action-button i {
      font-size: 0.9em;
    }

    .feed-share-button {
      background: transparent;
      border: none;
      color: rgba(255, 255, 255, 0.7);
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 0.9em;
      transition: all 0.3s ease;
    }

    .feed-share-button:hover {
      color: var(--primary-color);
    }

    .feed-card-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-top: 12px;
    }

    .feed-tag {
      background: rgba(255, 255, 255, 0.1);
      padding: 4px 10px;
      border-radius: 20px;
      font-size: 0.8em;
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .nysa-feed-grid {
        grid-template-columns: 1fr;
        padding: 0 10px;
        margin-top: 80px;
        gap: 15px;
      }

      .nysa-feed-card {
        width: 100%;
        margin: 0;
      }

      .feed-card-image-container {
        height: 350px;
      }

      .nysa-feed-content {
        padding: 10px 0;
        padding-bottom: 200px;
      }

      #nysaFeedSearchInput {
        padding: 20px 20px;
      }
    }

    @media (min-width: 769px) {
      .nysa-feed-search-container {
        padding: 24px;
        bottom: 0px;
      }

      .input-container {
        max-width: 800px;
        margin: 0 auto;
      }

      #nysaFeedSearchInput {
        height: 70px;
        font-size: 1.05rem;
        padding: 0 50px 0 24px;
        border-radius: 16px;
      }

      .voice-search {
        width: 50px;
        height: 50px;
        right: 8px;
      }

      .voice-search i {
        font-size: 1.7em;
      }

      .nysa-feed-grid {
        padding-bottom: 180px;
      }
    }

    /* ===== Tablet (768px - 1024px) ===== */
@media (min-width: 768px) and (max-width: 1024px) {
  .nysa-feed-grid {
    grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
    gap: 16px;
    padding-bottom: 120px;
  }

  .nysa-feed-card {
    border-radius: 12px;
  }

  .feed-card-image-container {
    height: 300px;
  }

  .feed-card-header {
    padding: 10px 14px;
  }

  .feed-provider-avatar {
    width: 36px;
    height: 36px;
  }

  .feed-card-title {
    font-size: 1.1em;
  }

  .feed-card-description {
    font-size: 0.9em;
  }

  .feed-card-price {
    font-size: 1em;
  }

  .feed-action-button {
    padding: 6px 12px;
    font-size: 0.9em;
  }
}

/* ===== Laptop & Desktop (1025px+) - Compact Version ===== */
@media (min-width: 1025px) {
  .nysa-feed-grid {
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 16px;
    padding-bottom: 120px;
  }

  .nysa-feed-card {
    border-radius: 12px;
  }

  .feed-card-image-container {
    height: 240px;
  }

  .feed-card-header {
    padding: 10px 12px;
  }

  .feed-provider-avatar {
    width: 32px;
    height: 32px;
    margin-right: 10px;
  }

  .feed-provider-name {
    font-size: 0.9em;
  }

  .feed-meta-badges {
    gap: 8px;
  }

  .time-badge,
  .location-badge {
    padding: 4px 10px;
    font-size: 0.7em;
  }

  .feed-card-title {
    font-size: 1.05em;
    margin-bottom: 6px;
  }

  .feed-card-description {
    font-size: 0.85em;
    margin-bottom: 10px;
    -webkit-line-clamp: 3;
  }

  .feed-card-price {
    font-size: 0.95em;
  }

  .feed-card-location {
    font-size: 0.8em;
  }

  .feed-action-button {
    padding: 6px 12px;
    font-size: 0.85em;
  }

  .feed-card-tags {
    gap: 6px;
    margin-top: 10px;
  }

  .feed-tag {
    padding: 3px 8px;
    font-size: 0.75em;
  }
}
    `;
  document.head.appendChild(style);
}

function getCategoryButtonStyle(category) {
  const colors = {
    'All': 'background: rgba(255, 255, 255, 0.1); color: white;',
    'Service Providers': 'background: var(--service-providers); color: white;',
    'Healthcare': 'background: var(--healthcare); color: white;',
    'Jobs': 'background: var(--jobs); color: white;',
    'Real Estate': 'background: var(--real-estate); color: white;',
    'Government Schemes': 'background: var(--government-schemes); color: white;',
    'Education': 'background: var(--education); color: white;'
  };
  
  const activeColors = {
    'All': 'background: white; color: black;',
    'Service Providers': 'background: #3d8b40; color: white;',
    'Healthcare': 'background: #d32f2f; color: white;',
    'Jobs': 'background: #1976d2; color: white;',
    'Real Estate': 'background: #f57c00; color: white;',
    'Government Schemes': 'background: #7b1fa2; color: white;',
    'Education': 'background: #0097a7; color: white;'
  };
  
  const hoverColors = {
    'All': 'background: rgba(255, 255, 255, 0.2);',
    'Service Providers': 'background: #66bb6a;',
    'Healthcare': 'background: #ef5350;',
    'Jobs': 'background: #42a5f5;',
    'Real Estate': 'background: #ffa726;',
    'Government Schemes': 'background: #ab47bc;',
    'Education': 'background: #26c6da;'
  };
  
  return `
    ${colors[category] || colors['All']}
    border: none;
  `;
}

function getActionButtonStyle(category) {
  const colors = {
    'Service Providers': 'var(--service-providers)',
    'Healthcare': 'var(--healthcare)',
    'Jobs': 'var(--jobs)',
    'Real Estate': 'var(--real-estate)',
    'Government Schemes': 'var(--government-schemes)',
    'Education': 'var(--education)'
  };
  
  return `
    background: ${colors[category] || 'var(--primary-color)'};
    color: white;
  `;
}

function getActionButtonText(category) {
  const texts = {
    'Service Providers': 'Book Now',
    'Healthcare': 'Book Appointment',
    'Jobs': 'Apply Now',
    'Real Estate': 'Schedule Visit',
    'Government Schemes': 'Apply Now',
    'Education': 'Enquire Now'
  };
  
  return texts[category] || 'View Details';
}

function renderNysaFeedCards(filteredData) {
  return filteredData.map(feed => `
    <div class="nysa-feed-card" onclick="openNysaFeedDetail('${feed.id}')">
      <div class="feed-card-header">
        <img src="${feed.details.headerImage || `https://ui-avatars.com/api/?name=${encodeURIComponent(feed.details.provider)}&background=random`}" 
             alt="${feed.details.provider}" class="feed-provider-avatar">
        <div class="feed-provider-info">
          <div class="feed-provider-name">${feed.details.provider}</div>
          <div class="feed-meta-badges">
            <div class="time-badge">
              <i class="far fa-clock"></i>
              <span>${feed.details.datePosted}</span>
            </div>
            ${feed.details.location ? `
              <div class="location-badge">
                <i class="fas fa-map-marker-alt"></i>
                <span>${feed.details.location}</span>
              </div>
            ` : ''}
          </div>
        </div>
      </div>
      
      <div class="feed-card-image-container" id="image-container-${feed.id}">
        <div class="feed-card-image-slider">
          ${feed.details.images.map((image, index) => `
            <div class="feed-card-slide ${index === 0 ? 'active' : ''}" data-index="${index}">
              <img src="${image}" alt="${feed.details.title}" class="feed-card-image">
            </div>
          `).join('')}
        </div>
        
        ${feed.details.images.length > 1 ? `
          <div class="feed-card-carousel">
            ${feed.details.images.map((_, index) => `
              <div class="carousel-indicator ${index === 0 ? 'active' : ''}" data-index="${index}"></div>
            `).join('')}
          </div>
        ` : ''}
      </div>

      <div class="feed-card-content">
        <h3 class="feed-card-title">${feed.details.title}</h3>
        <p class="feed-card-description">${feed.details.description}</p>
        
        <div class="feed-card-meta">
          ${feed.details.price || feed.details.salary ? `
            <div class="feed-card-price">${feed.details.price || feed.details.salary}</div>
          ` : ''}
          ${feed.details.location ? `
            <div class="feed-card-location">
              <i class="fas fa-map-marker-alt"></i> ${feed.details.location}
            </div>
          ` : ''}
        </div>
        
        <div class="feed-card-actions">
          <button class="feed-action-button" onclick="event.stopPropagation(); handleFeedAction('${feed.id}')" 
                  style="${getActionButtonStyle(feed.category)}">
            <i class="fas ${getActionIcon(feed.category)}"></i> ${getActionButtonText(feed.category)}
          </button>
          <button class="feed-share-button" onclick="event.stopPropagation(); shareFeed('${feed.id}')">
            <i class="fas fa-share"></i> Share
          </button>
        </div>
        
        ${feed.details.tags ? `
          <div class="feed-card-tags">
            ${feed.details.tags.map(tag => `
              <span class="feed-tag">#${tag}</span>
            `).join('')}
          </div>
        ` : ''}
      </div>
    </div>
  `).join('');
}

function getActionIcon(category) {
  const icons = {
    'Service Providers': 'fa-calendar-alt',
    'Healthcare': 'fa-calendar-check',
    'Jobs': 'fa-briefcase',
    'Real Estate': 'fa-home',
    'Government Schemes': 'fa-file-alt',
    'Education': 'fa-graduation-cap'
  };
  return icons[category] || 'fa-info-circle';
}

function openNysaFeedDetail(feedId) {
  const feed = nysaFeedData.find(item => item.id === feedId);
  if (!feed) return;

  const feedPage = document.createElement('div');
  feedPage.className = 'nysa-feed-detail-page';
  feedPage.innerHTML = `
    <div class="nysa-feed-detail-header">
      <button class="back-button" onclick="closeNysaFeedDetail()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <div class="header-title">${feed.details.title}</div>
      <div class="header-actions">
        <button class="nysa-share-button" onclick="shareFeed('${feed.id}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="nysa-feed-detail-content">
      <div class="feed-detail-image-container">
        <div class="feed-image-slider">
          ${feed.details.images.map((image, index) => `
            <div class="feed-slide ${index === 0 ? 'active' : ''}" data-index="${index}">
              <img src="${image}" alt="${feed.details.title} - Image ${index + 1}" class="feed-detail-image">
            </div>
          `).join('')}
        </div>
        <button class="slider-nav prev" onclick="navigateSlide(-1)"><i class="fas fa-chevron-left"></i></button>
        <button class="slider-nav next" onclick="navigateSlide(1)"><i class="fas fa-chevron-right"></i></button>
        ${feed.details.images.length > 1 ? `
          <div class="feed-detail-carousel">
            ${feed.details.images.map((_, index) => `
              <div class="carousel-dot ${index === 0 ? 'active' : ''}" data-index="${index}"></div>
            `).join('')}
          </div>
        ` : ''}
      </div>
      
      <div class="feed-detail-info-container">
        <div class="feed-detail-basic-info">
          <div class="feed-provider-section">
            <img src="${feed.details.detailImage || `https://ui-avatars.com/api/?name=${encodeURIComponent(feed.details.provider)}&background=random`}" 
                 alt="${feed.details.provider}" class="feed-provider-avatar">
            <div class="feed-provider-meta">
              <div class="feed-provider-name">${feed.details.provider}</div>
              <div class="feed-posted-info">
                <span><i class="far fa-clock"></i> ${feed.details.datePosted}</span>
                ${feed.details.location ? `<span><i class="fas fa-map-marker-alt"></i> ${feed.details.location}</span>` : ''}
              </div>
            </div>
          </div>
          
          <div class="feed-content-section">
            <h1 class="feed-title">${feed.details.title}</h1>
            <p class="feed-description">${feed.details.description}</p>
            
            ${feed.details.price || feed.details.salary ? `
              <div class="feed-price-section">
                <span class="price-label">${feed.category === 'Jobs' ? 'Salary' : 'Price'}</span>
                <span class="price-value">${feed.details.price || feed.details.salary}</span>
              </div>
            ` : ''}
            
            ${feed.details.specs ? `
              <div class="feed-specs-section">
                <span class="specs-label">Details</span>
                <span class="specs-value">${feed.details.specs}</span>
              </div>
            ` : ''}
            
            ${feed.details.deadline ? `
              <div class="feed-deadline-section">
                <span class="deadline-label">Deadline</span>
                <span class="deadline-value">${feed.details.deadline}</span>
              </div>
            ` : ''}
          </div>
          
          <div class="feed-detail-full-content">
            ${feed.details.content}
          </div>
          
          ${feed.details.contact ? `
            <div class="feed-contact-section">
              <h3 class="section-title">Contact Information</h3>
              <div class="contact-methods">
                <div class="contact-method">
                  <i class="fas fa-phone"></i>
                  <span>${feed.details.contact}</span>
                </div>
                ${feed.details.social?.whatsapp ? `
                  <a href="${feed.details.social.whatsapp}" target="_blank" class="contact-method">
                    <i class="fab fa-whatsapp"></i>
                    <span>Chat on WhatsApp</span>
                  </a>
                ` : ''}
                ${feed.details.social?.website ? `
                  <a href="${feed.details.social.website}" target="_blank" class="contact-method">
                    <i class="fas fa-globe"></i>
                    <span>Visit Website</span>
                  </a>
                ` : ''}
              </div>
            </div>
          ` : ''}
          
          <div class="feed-action-section">
            <button class="feed-action-detail-button" onclick="handleFeedAction('${feed.id}')" 
                    style="${getActionButtonStyle(feed.category)}">
              <i class="fas ${getActionIcon(feed.category)}"></i>
              <span>${getActionButtonText(feed.category)}</span>
            </button>
          </div>
        </div>
        
        ${feed.details.tags ? `
          <div class="feed-tags-section">
            <h3 class="section-title">Tags</h3>
            <div class="tags-container">
              ${feed.details.tags.map(tag => `
                <span class="tag">#${tag}</span>
              `).join('')}
            </div>
          </div>
        ` : ''}
      </div>
    </div>
  `;

  document.body.appendChild(feedPage);
  addNysaFeedDetailStyles();
  initializeFeedCardSliders();
  initializeImageSlider();
  scrollToTop();
}

function initializeFeedCardSliders() {
  nysaFeedData.forEach(feed => {
    const container = document.getElementById(`image-container-${feed.id}`);
    if (!container || feed.details.images.length <= 1) return;
    
    const slider = container.querySelector('.feed-card-image-slider');
    const slides = container.querySelectorAll('.feed-card-slide');
    const indicators = container.querySelectorAll('.carousel-indicator');
    let currentIndex = 0;
    let touchStartX = 0;
    let touchEndX = 0;
    
    // Touch event handlers
    container.addEventListener('touchstart', (e) => {
      touchStartX = e.changedTouches[0].clientX;
    }, { passive: true });
    
    container.addEventListener('touchend', (e) => {
      touchEndX = e.changedTouches[0].clientX;
      handleSwipe();
    }, { passive: true });
    
    function handleSwipe() {
      const threshold = 50;
      const difference = touchStartX - touchEndX;
      
      if (difference > threshold) {
        // Swipe left - next slide
        goToSlide(currentIndex + 1);
      } else if (difference < -threshold) {
        // Swipe right - previous slide
        goToSlide(currentIndex - 1);
      }
    }
    
    function goToSlide(index) {
      if (index < 0) index = slides.length - 1;
      if (index >= slides.length) index = 0;
      
      currentIndex = index;
      slider.style.transform = `translateX(-${currentIndex * 100}%)`;
      
      // Update indicators
      indicators.forEach((indicator, i) => {
        indicator.classList.toggle('active', i === currentIndex);
      });
    }
    
    // Click events for indicators
    indicators.forEach(indicator => {
      indicator.addEventListener('click', (e) => {
        e.stopPropagation();
        const index = parseInt(indicator.getAttribute('data-index'));
        goToSlide(index);
      });
    });
  });
}

function initializeImageSlider() {
  const slider = document.querySelector('.feed-image-slider');
  const slides = document.querySelectorAll('.feed-slide');
  const dots = document.querySelectorAll('.carousel-dot');
  const prevBtn = document.querySelector('.slider-nav.prev');
  const nextBtn = document.querySelector('.slider-nav.next');
  
  if (!slider || slides.length === 0) return;
  
  let currentIndex = 0;
  let touchStartX = 0;
  let touchEndX = 0;
  
  // Function to update the slider
  function updateSlider() {
    slides.forEach((slide, index) => {
      slide.classList.toggle('active', index === currentIndex);
    });
    
    if (dots) {
      dots.forEach((dot, index) => {
        dot.classList.toggle('active', index === currentIndex);
      });
    }
  }
  
  // Navigation functions
  function goToSlide(index) {
    if (index < 0) index = slides.length - 1;
    if (index >= slides.length) index = 0;
    currentIndex = index;
    updateSlider();
  }
  
  function navigateSlide(direction) {
    goToSlide(currentIndex + direction);
  }
  
  // Event listeners for buttons
  if (prevBtn) prevBtn.addEventListener('click', () => navigateSlide(-1));
  if (nextBtn) nextBtn.addEventListener('click', () => navigateSlide(1));
  
  // Touch events for swipe
  slider.addEventListener('touchstart', (e) => {
    touchStartX = e.changedTouches[0].screenX;
  }, { passive: true });
  
  slider.addEventListener('touchend', (e) => {
    touchEndX = e.changedTouches[0].screenX;
    handleSwipe();
  }, { passive: true });
  
  function handleSwipe() {
    const difference = touchStartX - touchEndX;
    if (difference > 50) {
      // Swipe left - next slide
      navigateSlide(1);
    } else if (difference < -50) {
      // Swipe right - previous slide
      navigateSlide(-1);
    }
  }
  
  // Click events for dots
  if (dots) {
    dots.forEach(dot => {
      dot.addEventListener('click', () => {
        const index = parseInt(dot.getAttribute('data-index'));
        goToSlide(index);
      });
    });
  }
  
  // Auto-advance slides (optional)
  let slideInterval = setInterval(() => navigateSlide(1), 5000);
  
  // Pause on hover
  slider.addEventListener('mouseenter', () => clearInterval(slideInterval));
  slider.addEventListener('mouseleave', () => {
    slideInterval = setInterval(() => navigateSlide(1), 5000);
  });
  
  // Keyboard navigation
  document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowLeft') navigateSlide(-1);
    if (e.key === 'ArrowRight') navigateSlide(1);
  });
}

function addNysaFeedDetailStyles() {
  const style = document.createElement('style');
  style.textContent = `
    .nysa-feed-detail-page {
      font-family: poppins;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: var(--background-dark);
      z-index: 2000;
      overflow-y: auto;
      color: var(--text-primary);
      animation: fadeIn 0.3s ease-out;
    }

    /* Enhanced Header */
    .nysa-feed-detail-header {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      padding: 16px 24px;
      background: rgba(26, 26, 31, 0.98);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      z-index: 10;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-shadow: 0 2px 15px rgba(0, 0, 0, 0.3);
      border-bottom: 1px solid var(--border-color);
    }

    .header-title {
      font-size: 1.1em;
      font-weight: 600;
      color: var(--text-primary);
      flex: 1;
      text-align: center;
      padding: 0 20px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .back-button {
      background: rgba(255, 255, 255, 0.1);
      border: none;
      color: var(--text-primary);
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: var(--transition);
      flex-shrink: 0;
    }

    .back-button:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: translateX(-2px);
    }

    .header-actions {
      display: flex;
      gap: 10px;
      flex-shrink: 0;
    }

    .nysa-share-button {
      background: rgba(255, 255, 255, 0.05);
      border: none;
      color: var(--text-primary);
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: var(--transition);
    }

    .nysa-share-button:hover {
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
    }

    /* Content Layout */
    .nysa-feed-detail-content {
      display: flex;
      flex-direction: column;
      padding-top: 72px;
      min-height: 100vh;
    }

    /* Enhanced Image Slider */
    .feed-detail-image-container {
      position: relative;
      width: 100%;
      height: 400px;
      overflow: hidden;
      background: rgba(0, 0, 0, 0.3);
    }

    .feed-image-slider {
      display: flex;
      width: 100%;
      height: 100%;
      transition: transform 0.5s ease;
    }

    .feed-slide {
      min-width: 100%;
      height: 100%;
      position: relative;
      opacity: 0;
      transition: opacity 0.5s ease;
    }

    .feed-slide.active {
      opacity: 1;
    }

    .feed-detail-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .slider-nav {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(0, 0, 0, 0.5);
      border: none;
      color: white;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      z-index: 2;
      transition: var(--transition);
    }

    .slider-nav:hover {
      background: rgba(0, 0, 0, 0.8);
    }

    .slider-nav.prev {
      left: 20px;
    }

    .slider-nav.next {
      right: 20px;
    }

    .slider-nav i {
      font-size: 1.2em;
    }

    .feed-detail-carousel {
      position: absolute;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      gap: 8px;
      z-index: 2;
    }

    .carousel-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: rgba(255, 255, 255, 0.5);
      cursor: pointer;
      transition: var(--transition);
    }

    .carousel-dot.active {
      background: var(--primary-color);
      transform: scale(1.2);
    }

    /* Info Container */
    .feed-detail-info-container {
      padding: 30px;
      background: var(--background-dark);
    }

    /* Provider Section */
    .feed-provider-section {
      display: flex;
      align-items: center;
      gap: 15px;
      margin-bottom: 25px;
      padding-bottom: 20px;
      border-bottom: 1px solid var(--border-color);
    }

    .feed-provider-avatar {
      width: 60px;
      height: 60px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid var(--primary-color);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    }

    .feed-provider-meta {
      flex: 1;
    }

    .feed-provider-name {
      font-weight: 600;
      font-size: 1.1em;
      margin-bottom: 5px;
    }

    .feed-posted-info {
      display: flex;
      gap: 15px;
      font-size: 0.85em;
      color: var(--text-tertiary);
    }

    .feed-posted-info span {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .feed-posted-info i {
      font-size: 0.9em;
    }

    /* Content Section */
    .feed-content-section {
      margin-bottom: 30px;
    }

    .feed-title {
      font-size: 1.8em;
      font-weight: 700;
      margin: 0 0 15px 0;
      line-height: 1.3;
      color: var(--text-primary);
    }

    .feed-description {
      font-size: 1em;
      line-height: 1.6;
      margin-bottom: 25px;
      color: var(--text-secondary);
    }

    /* Info Sections */
    .feed-price-section,
    .feed-specs-section,
    .feed-deadline-section {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px;
      margin-bottom: 15px;
      background: var(--card-background);
      border-radius: var(--border-radius);
      border-left: 3px solid var(--primary-color);
    }

    .price-label,
    .specs-label,
    .deadline-label {
      font-size: 0.9em;
      color: var(--text-secondary);
    }

    .price-value,
    .specs-value,
    .deadline-value {
      font-size: 1.1em;
      font-weight: 600;
    }

    .price-value {
      color: var(--primary-color);
    }

    /* Full Content */
    .feed-detail-full-content {
      line-height: 1.7;
      margin-bottom: 30px;
      font-size: 0.95em;
      color: var(--text-secondary);
    }

    .feed-detail-full-content h2 {
      font-size: 1.4em;
      margin: 30px 0 15px;
      color: var(--primary-color);
      font-weight: 600;
    }

    .feed-detail-full-content h3 {
      font-size: 1.2em;
      margin: 25px 0 15px;
      font-weight: 500;
      color: var(--text-primary);
    }

    .feed-detail-full-content p {
      margin-bottom: 15px;
    }

    .feed-detail-full-content ul,
    .feed-detail-full-content ol {
      margin-bottom: 15px;
      padding-left: 20px;
    }

    .feed-detail-full-content li {
      margin-bottom: 8px;
    }

    .feed-detail-full-content blockquote {
      border-left: 3px solid var(--primary-color);
      padding-left: 15px;
      margin: 20px 0;
      font-style: italic;
      color: var(--text-tertiary);
    }

    .feed-detail-full-content table {
      width: 100%;
      border-collapse: collapse;
      margin: 20px 0;
      background: var(--card-background);
    }

    .feed-detail-full-content th,
    .feed-detail-full-content td {
      padding: 12px 15px;
      text-align: left;
      border-bottom: 1px solid var(--border-color);
    }

    .feed-detail-full-content th {
      background: rgba(58, 134, 255, 0.1);
      color: var(--primary-color);
      font-weight: 500;
    }

    /* Contact Section */
    .feed-contact-section {
      margin-bottom: 30px;
    }

    .section-title {
      font-size: 1.1em;
      font-weight: 600;
      margin: 0 0 15px 0;
      color: var(--primary-color);
      position: relative;
      padding-bottom: 8px;
    }

    .section-title::after {
      content: '';
      position: absolute;
      bottom: 0;
      left: 0;
      width: 40px;
      height: 2px;
      background: var(--primary-color);
    }

    .contact-methods {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .contact-method {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px 15px;
      background: var(--card-background);
      border-radius: var(--border-radius);
      transition: var(--transition);
      text-decoration: none;
      color: var(--text-primary);
    }

    .contact-method:hover {
      background: rgba(255, 255, 255, 0.07);
      transform: translateX(5px);
    }

    .contact-method i {
      width: 20px;
      text-align: center;
      color: var(--primary-color);
    }

    /* Action Button */
    .feed-action-section {
      margin: 40px 0;
    }

    .feed-action-detail-button {
      width: 100%;
      border: none;
      padding: 16px;
      border-radius: var(--border-radius);
      font-weight: 600;
      font-size: 1em;
      cursor: pointer;
      transition: var(--transition);
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    }

    .feed-action-detail-button:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
    }

    .feed-action-detail-button:active {
      transform: translateY(0);
    }

    /* Tags Section */
    .feed-tags-section {
      margin-bottom: 30px;
    }

    .tags-container {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .tag {
      background: rgba(58, 134, 255, 0.1);
      padding: 8px 15px;
      border-radius: 20px;
      font-size: 0.85em;
      color: var(--primary-color);
      transition: var(--transition);
    }

    .tag:hover {
      background: rgba(58, 134, 255, 0.2);
    }

    /* Responsive Design */
    @media (max-width: 1200px) {
      .feed-detail-image-container {
        height: 350px;
      }
    }

    @media (max-width: 768px) {
      .feed-title {
        font-size: 1.5em;
      }
      
      .feed-detail-info-container {
        padding: 20px;
      }
      
      .feed-provider-avatar {
        width: 50px;
        height: 50px;
      }
      
      .feed-price-section,
      .feed-specs-section,
      .feed-deadline-section {
        flex-direction: column;
        align-items: flex-start;
        gap: 5px;
      }

      .feed-detail-image-container {
        height: 300px;
      }
    }

    @media (max-width: 480px) {
      .nysa-feed-detail-header {
        padding: 12px 15px;
      }
      
      .header-title {
        font-size: 1em;
        padding: 0 10px;
      }
      
      .back-button,
      .nysa-share-button {
        width: 36px;
        height: 36px;
      }
      
      .feed-detail-image-container {
        height: 250px;
      }
      
      .feed-title {
        font-size: 1.3em;
      }

      .slider-nav {
        width: 30px;
        height: 30px;
      }
    }

    /* Animations */
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
  `;
  document.head.appendChild(style);
}

function closeNysaFeedDetail() {
  const feedPage = document.querySelector('.nysa-feed-detail-page');
  if (feedPage) {
    feedPage.remove();
  }
}

function hideNysaFeedOverlay() {
  const overlay = document.getElementById('nysaFeedOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => overlay.remove(), 300);
  }
}

function filterNysaFeed(category) {
  const nysaFeedGrid = document.getElementById('nysaFeedGrid');
  let filteredData = nysaFeedData;

  if (category !== 'All') {
    filteredData = filteredData.filter(feed => feed.category === category);
  }

  nysaFeedGrid.innerHTML = renderNysaFeedCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function performNysaFeedSearch(data, query, gridElement, filterButtons, renderFunction) {
  if (!query) {
    // If no query, show all feeds
    gridElement.innerHTML = renderFunction(data);
    return;
  }

  const lowerCaseQuery = query.toLowerCase();
  const filteredData = data.filter(feed => {
    return (
      feed.details.title.toLowerCase().includes(lowerCaseQuery) ||
      feed.details.description.toLowerCase().includes(lowerCaseQuery) ||
      feed.details.provider.toLowerCase().includes(lowerCaseQuery) ||
      feed.category.toLowerCase().includes(lowerCaseQuery) ||
      (feed.details.tags && feed.details.tags.some(tag => tag.toLowerCase().includes(lowerCaseQuery)))
    );
  });

  gridElement.innerHTML = filteredData.length > 0 
    ? renderFunction(filteredData)
    : '<div class="no-results">No results found matching your search.</div>';
}

function initializeNysaFeedVoiceSearch(inputElement, callback) {
  const voiceButton = document.getElementById('nysaFeedVoiceSearchButton');
  if (!voiceButton) return;

  let recognition;
  try {
    recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.continuous = false;
    recognition.interimResults = false;

    voiceButton.addEventListener('click', () => {
      if (voiceButton.classList.contains('active')) {
        recognition.stop();
        voiceButton.classList.remove('active');
        return;
      }

      voiceButton.classList.add('active');
      recognition.start();

      recognition.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        inputElement.value = transcript;
        if (callback) callback(transcript);
        voiceButton.classList.remove('active');
      };

      recognition.onerror = (event) => {
        console.error('Voice recognition error', event.error);
        voiceButton.classList.remove('active');
      };

      recognition.onend = () => {
        voiceButton.classList.remove('active');
      };
    });
  } catch (e) {
    console.error('Voice recognition not supported', e);
    voiceButton.style.display = 'none';
  }
}

// Nysa Feed Interaction Functions
function handleFeedAction(feedId) {
  const feed = nysaFeedData.find(item => item.id === feedId);
  if (feed) {
    // In a real app, this would handle the appropriate action
    console.log(`Action taken on feed: ${feed.details.title} - ${feed.details.actionButton.text}`);
    showToast(`Action: ${feed.details.actionButton.text} for "${feed.details.title}"`);
    
    // Open the action URL if it exists
    if (feed.details.actionButton.url && feed.details.actionButton.url !== '#') {
      window.open(feed.details.actionButton.url, '_blank');
    }
  }
}

function shareFeed(feedId) {
  const feed = nysaFeedData.find(item => item.id === feedId);
  if (!feed) return;

  if (navigator.share) {
    navigator.share({
      title: feed.details.title,
      text: feed.details.description,
      url: feed.details.social?.website || '#',
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    // Fallback for browsers that don't support Web Share API
    const shareUrl = `https://twitter.com/intent/tweet?text=${encodeURIComponent(`Check out "${feed.details.title}" on Nysa Feed`)}&url=${encodeURIComponent(feed.details.social?.website || '#')}`;
    window.open(shareUrl, '_blank');
  }
}

function showToast(message) {
  const toast = document.createElement('div');
  toast.className = 'nysa-feed-toast';
  toast.textContent = message;
  document.body.appendChild(toast);
  
  setTimeout(() => {
    toast.classList.add('show');
  }, 10);
  
  setTimeout(() => {
    toast.classList.remove('show');
    setTimeout(() => toast.remove(), 300);
  }, 3000);
  
  // Add toast styles if they don't exist
  if (!document.getElementById('nysa-feed-toast-styles')) {
    const style = document.createElement('style');
    style.id = 'nysa-feed-toast-styles';
    style.textContent = `
      .nysa-feed-toast {
        position: fixed;
        bottom: 30px;
        left: 50%;
        transform: translateX(-50%) translateY(100px);
        background: rgba(0, 0, 0, 0.8);
        color: white;
        padding: 12px 24px;
        border-radius: var(--border-radius);
        z-index: 3000;
        transition: transform 0.3s ease;
        font-size: 0.9em;
        max-width: 90%;
        text-align: center;
      }
      
      .nysa-feed-toast.show {
        transform: translateX(-50%) translateY(0);
      }
    `;
    document.head.appendChild(style);
  }
}

function scrollToTop() {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  });
}

// Utility function for debouncing
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

// Initialize Nysa Feed button event listener
document.addEventListener('DOMContentLoaded', function() {
  const nysaFeedButton = document.getElementById('nysaFeedButton');
  
  if (nysaFeedButton) {
    nysaFeedButton.addEventListener('click', function(e) {
      e.preventDefault();
      showNysaFeedOverlay();
    });
  }
});
  
  
const jobsData = [
  // Tech Jobs
  {
    category: "Tech",
    details: {
      title: "Senior AI Engineer at OpenAI",
      image: "https://a57.foxnews.com/static.foxnews.com/foxnews.com/content/uploads/2024/05/1200/675/OPENAI-CHATGPT.jpg?ve=1&tl=1",
      description: "Work on cutting-edge AI models like GPT-5. Lead research team in developing next-generation language models.",
      company: "OpenAI",
      location: "San Francisco, CA (Hybrid)",
      salary: "$220,000 - $350,000",
      type: "Full-time",
      posted: "May 15, 2024",
      experience: "5+ years",
      content: `
        <h2>About the Role</h2>
        <p>We're looking for an experienced AI Engineer to join our core research team. You'll be working directly on our next-generation models with responsibilities including:</p>
        <ul>
          <li>Designing and implementing novel neural architectures</li>
          <li>Optimizing large-scale training pipelines</li>
          <li>Leading a team of 3-5 researchers</li>
          <li>Publishing papers at top conferences</li>
        </ul>
        
        <h2>Requirements</h2>
        <table>
          <tr>
            <th>Must Have</th>
            <th>Nice to Have</th>
          </tr>
          <tr>
            <td>PhD in Computer Science or related</td>
            <td>Publications at NeurIPS/ICML</td>
          </tr>
          <tr>
            <td>Expertise in PyTorch/TensorFlow</td>
            <td>Experience with RLHF</td>
          </tr>
          <tr>
            <td>5+ years industry experience</td>
            <td>Open source contributions</td>
          </tr>
        </table>
        
        <h2>Benefits</h2>
        <ul>
          <li>Equity in a leading AI company</li>
          <li>Unlimited PTO</li>
          <li>Top-tier health insurance</li>
          <li>$10k/year learning budget</li>
        </ul>
      `,
      tags: ["Artificial Intelligence", "Machine Learning", "Python"],
      applyUrl: "https://openai.com/careers",
      social: {
        linkedin: "https://linkedin.com/company/openai",
        twitter: "https://twitter.com/openai"
      }
    }
  },
  
  // Healthcare Jobs
  {
    category: "Healthcare",
    details: {
      title: "Telemedicine Physician",
      image: "https://www.healthcareitnews.com/sites/hitn/files/Telemedicine%20video%20visit.jpg",
      description: "Provide remote care to patients via our telehealth platform. Flexible hours, full malpractice coverage.",
      company: "Teladoc Health",
      location: "Remote (USA)",
      salary: "$180,000 - $250,000",
      type: "Contract",
      posted: "May 14, 2024",
      experience: "Board Certified",
      content: `
        <h2>Position Details</h2>
        <p>Join our network of physicians delivering high-quality care through our telemedicine platform. This position offers:</p>
        <ul>
          <li>100% remote work</li>
          <li>Choose your own hours (minimum 20hrs/week)</li>
          <li>No overhead or administrative work</li>
          <li>Malpractice insurance included</li>
        </ul>
        
        <h2>Specialties Needed</h2>
        <div class="specialties">
          <span class="specialty">Primary Care</span>
          <span class="specialty">Psychiatry</span>
          <span class="specialty">Dermatology</span>
          <span class="specialty">Pediatrics</span>
        </div>
        
        <h2>Technology Requirements</h2>
        <ul>
          <li>Reliable high-speed internet</li>
          <li>Dual monitors recommended</li>
          <li>Quiet, private workspace</li>
        </ul>
      `,
      tags: ["Remote", "Telehealth", "Physician"],
      applyUrl: "https://teladoc.com/careers",
      social: {
        linkedin: "https://linkedin.com/company/teladoc-health"
      }
    }
  },
  // More jobs...
];

function showJobsOverlay() {
  let overlay = document.getElementById('jobsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'jobsOverlay';
    overlay.className = 'jobs-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(jobsData.map(job => job.category))];

  overlay.innerHTML = `
    <div class="jobs-content">
      <div class="jobs-header">
        <h2>Nysa Careers</h2>
        <div class="header-actions">
          <button class="toggle-filters" onclick="toggleAdvancedFilters()">
            <i class="fas fa-sliders-h"></i>
          </button>
          <button class="close-jobs" onclick="hideJobsOverlay()">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      
      <div class="jobs-filter-container">
        <div class="jobs-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterJobs('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="jobs-grid" id="jobsGrid">
        ${renderJobsCards(jobsData)}
      </div>
    </div>

    <div class="jobs-search-container">
      <div class="input-container">
        <input type="text" id="jobsSearchInput" placeholder="Search for jobs, companies, or locations..." />
        <button class="voice-search" id="jobsVoiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
    
    <div class="advanced-filters" id="advancedFilters">
      <div class="filter-section">
        <h4>Job Type</h4>
        <div class="filter-options">
          <label><input type="checkbox" name="job-type" value="Full-time" checked> Full-time</label>
          <label><input type="checkbox" name="job-type" value="Part-time" checked> Part-time</label>
          <label><input type="checkbox" name="job-type" value="Contract" checked> Contract</label>
          <label><input type="checkbox" name="job-type" value="Internship" checked> Internship</label>
        </div>
      </div>
      
      <div class="filter-section">
        <h4>Experience Level</h4>
        <div class="filter-options">
          <label><input type="checkbox" name="experience" value="Entry Level" checked> Entry Level</label>
          <label><input type="checkbox" name="experience" value="Mid Level" checked> Mid Level</label>
          <label><input type="checkbox" name="experience" value="Senior Level" checked> Senior Level</label>
          <label><input type="checkbox" name="experience" value="Executive" checked> Executive</label>
        </div>
      </div>
      
      <div class="filter-section">
        <h4>Salary Range</h4>
        <div class="salary-range">
          <input type="range" min="0" max="500000" value="0" step="10000" id="salaryMin">
          <input type="range" min="0" max="500000" value="500000" step="10000" id="salaryMax">
          <div class="salary-values">
            <span id="minSalaryValue">$0</span> - <span id="maxSalaryValue">$500k+</span>
          </div>
        </div>
      </div>
      
      <div class="filter-actions">
        <button class="apply-filters" onclick="applyAdvancedFilters()">Apply Filters</button>
        <button class="reset-filters" onclick="resetFilters()">Reset</button>
      </div>
    </div>
  `;

  const searchInput = document.getElementById('jobsSearchInput');
  const jobsGrid = document.getElementById('jobsGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performJobsSearch(jobsData, query, jobsGrid, filterButtons, renderJobsCards);
  }, 300);

  searchInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  searchInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performJobsSearch(jobsData, searchInput.value, jobsGrid, filterButtons, renderJobsCards);
    }
  });

  initializeJobsVoiceSearch(searchInput, (query) => {
    performJobsSearch(jobsData, query, jobsGrid, filterButtons, renderJobsCards);
  });

  // Initialize salary range display
  const salaryMin = document.getElementById('salaryMin');
  const salaryMax = document.getElementById('salaryMax');
  const minSalaryValue = document.getElementById('minSalaryValue');
  const maxSalaryValue = document.getElementById('maxSalaryValue');

  salaryMin.addEventListener('input', () => {
    minSalaryValue.textContent = formatSalary(salaryMin.value);
  });

  salaryMax.addEventListener('input', () => {
    maxSalaryValue.textContent = formatSalary(salaryMax.value);
  });

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
    .jobs-overlay {
      position: fixed;
      top: 60px;
      left: 0;
      right: 0;
      bottom: 60px;
      background: #1a1a1f;
      z-index: 1000;
      opacity: 0;
      visibility: hidden;
      transition: all 0.3s ease;
      overflow-y: auto;
      width: 100%;
      display: flex;
      flex-direction: column;
      scrollbar-width: none;
      -ms-overflow-style: none;
    }

    .jobs-overlay::-webkit-scrollbar {
      display: none;
    }

    .jobs-overlay.active {
      opacity: 1;
      visibility: visible;
    }

    .jobs-content {
      flex: 1;
      width: 100%;
      max-width: 100%;
      padding: 20px;
      margin: 0 auto;
      position: relative;
    }

    .jobs-header {
      padding: 16px 24px;
      background: rgba(26, 26, 31, 0.95);
      position: fixed;
      width: 100%;
      top: 0;
      left: 0;
      z-index: 30;
      border-bottom: 1px solid rgba(255, 215, 0, 0.15);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      height: 72px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }

    .jobs-header h2 {
      color: var(--primary-color);
      margin: 0;
      font-size: 1.75em;
      font-weight: 600;
      letter-spacing: -0.02em;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .header-actions {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .jobs-header h2::before {
      content: '';
      display: inline-block;
      width: 24px;
      height: 24px;
      background: var(--primary-color);
      mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M20 14.66V20a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h5.34'%3E%3C/path%3E%3Cpolygon points='18 2 22 6 12 16 8 16 8 12 18 2'%3E%3C/polygon%3E%3C/svg%3E");
      mask-repeat: no-repeat;
      mask-position: center;
      mask-size: contain;
    }

    .close-jobs {
      background: rgba(255, 255, 255, 0.1);
      border: none;
      color: white;
      font-size: 1em;
      cursor: pointer;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s ease;
    }

    .close-jobs:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: scale(1.05);
    }

    .close-jobs:active {
      transform: scale(0.95);
    }

    .jobs-filter-container {
      position: fixed;
      top: 72px;
      left: 0;
      right: 0;
      width: 100%;
      z-index: 10;
      background: #1a1a1f;
      padding: 12px 0;
      box-sizing: border-box;
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      scrollbar-width: none;
      border-bottom: 1px solid rgba(255, 215, 0, 0.1);
    }

    .jobs-filter-container::-webkit-scrollbar {
      display: none;
    }

    .jobs-filter-buttons {
      display: inline-flex;
      padding: 0 16px;
      gap: 8px;
      white-space: nowrap;
    }

    .filter-button {
      font-family: poppins;
      flex-shrink: 0;
      background: rgba(255, 215, 0, 0.1);
      color: rgba(255, 255, 255, 0.9);
      border: 1px solid rgba(255, 215, 0, 0.2);
      padding: 10px 20px;
      border-radius: 25px;
      transition: all 0.3s ease;
      cursor: pointer;
      font-size: 14px;
      font-weight: 500;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
      margin: 0;
    }

    .filter-button:hover {
      background: rgba(255, 215, 0, 0.15);
      transform: scale(1.03);
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    .filter-button.active {
      background: var(--primary-color);
      color: #000;
      font-weight: 600;
      border-color: var(--primary-color);
      box-shadow: 0 4px 10px rgba(255, 215, 0, 0.3);
    }

    /* Add some space at the end for better scrolling */
    .jobs-filter-buttons::after {
      content: '';
      display: block;
      min-width: 16px;
      height: 1px;
    }

  .toggle-filters {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  font-size: 0.9em;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.toggle-filters:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
}

.toggle-filters:active {
  transform: translateY(0);
}

    .jobs-grid {
      margin-top: 70px;
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
      gap: 20px;
      animation: fadeInUp 0.5s ease-out;
      padding-bottom: 140px;
      width: 100%;
    }

    .jobs-card {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      overflow: hidden;
      transition: all 0.3s ease;
      border: 1px solid rgba(255, 255, 255, 0.1);
      cursor: pointer;
    }

    .jobs-card:hover {
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
      border-color: rgba(255, 215, 0, 0.3);
      transform: translateY(-5px);
    }

    .jobs-card-image {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .jobs-card-content {
      padding: 20px;
    }

    .jobs-card-category {
      display: inline-block;
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
      padding: 4px 12px;
      border-radius: 20px;
      font-size: 0.8em;
      font-weight: 600;
      margin-bottom: 12px;
    }

    .jobs-card-title {
      font-size: 1.3em;
      font-weight: 700;
      margin: 0 0 10px 0;
      color: white;
      line-height: 1.3;
    }

    .jobs-card-company {
      font-size: 1.1em;
      color: rgba(255, 255, 255, 0.9);
      margin: 0 0 10px 0;
    }

    .jobs-card-location {
      display: flex;
      align-items: center;
      gap: 5px;
      font-size: 0.9em;
      color: rgba(255, 255, 255, 0.7);
      margin-bottom: 10px;
    }

    .jobs-card-salary {
      font-size: 1em;
      color: var(--primary-color);
      font-weight: 600;
      margin-bottom: 15px;
    }

    .jobs-card-meta {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.85em;
      color: rgba(255, 255, 255, 0.6);
    }

    .jobs-card-type {
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
      padding: 4px 10px;
      border-radius: 4px;
      font-size: 0.8em;
    }

    .jobs-card-date {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .jobs-search-container {
      position: fixed;
      bottom: 0px;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      display: flex;
      flex-direction: column;
      gap: 12px;
      align-items: center;
      box-sizing: border-box;
    }

    .input-container {
      flex: 1;
      position: relative;
      display: flex;
      align-items: center;
      width: 100%;
      max-width: 800px;
    }

    #jobsSearchInput {
      flex: 1;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 12px 50px 12px 20px;
      color: #fff;
      font-size: 0.95em;
      width: 100%;
      transition: border-color 0.3s ease;
    }

    #jobsSearchInput:focus {
      border-color: rgba(255, 255, 255, 0.5);
    }

    .voice-search {
      position: absolute;
      right: 10px;
      background: none;
      border: none;
      color: var(--primary-color);
      cursor: pointer;
      padding: 10px;
      transition: all 0.3s ease;
    }

    .voice-search:hover {
      opacity: 0.8;
    }

    .voice-search.active {
      color: #ffd700;
      animation: pulse 1.5s infinite;
    }

    .voice-search i { 
      font-size: 1.5em; 
    }

    .advanced-filters {
      position: fixed;
      bottom: 100px;
      left: 0;
      right: 0;
      background: #1a1a1f;
      padding: 20px;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      transform: translateY(100%);
      transition: transform 0.3s ease;
      z-index: 999;
      max-height: 60vh;
      overflow-y: auto;
    }

    .advanced-filters.active {
      transform: translateY(0);
    }

    .filter-section {
      margin-bottom: 20px;
    }

    .filter-section h4 {
      color: white;
      margin-bottom: 12px;
      font-size: 1em;
    }

    .filter-options {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
    }

    .filter-options label {
      display: flex;
      align-items: center;
      gap: 6px;
      color: rgba(255, 255, 255, 0.9);
      font-size: 0.9em;
      cursor: pointer;
    }

    .filter-options input[type="checkbox"] {
      accent-color: var(--primary-color);
    }

    .salary-range {
      padding: 0 10px;
    }

    .salary-range input[type="range"] {
      width: 100%;
      margin: 10px 0;
      -webkit-appearance: none;
      height: 4px;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 2px;
    }

    .salary-range input[type="range"]::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 16px;
      height: 16px;
      background: var(--primary-color);
      border-radius: 50%;
      cursor: pointer;
    }

    .salary-values {
      display: flex;
      justify-content: space-between;
      color: white;
      font-size: 0.9em;
    }

    .filter-actions {
      display: flex;
      justify-content: flex-end;
      gap: 10px;
      margin-top: 20px;
    }

    .apply-filters, .reset-filters {
      padding: 10px 20px;
      border-radius: 6px;
      font-weight: 500;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .apply-filters {
      background: var(--primary-color);
      color: #000;
      border: none;
    }

    .apply-filters:hover {
      opacity: 0.9;
    }

    .reset-filters {
      background: transparent;
      color: var(--primary-color);
      border: 1px solid var(--primary-color);
    }

    .reset-filters:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .jobs-grid {
        grid-template-columns: 1fr;
        padding: 0 10px;
        margin-top: 80px;
        gap: 15px;
      }

      .jobs-card {
        width: 100%;
        margin: 0;
      }

      .jobs-card-image {
        height: 180px;
      }

      .jobs-content {
        padding: 10px 0;
        padding-bottom: 200px;
      }

      #jobsSearchInput {
        padding: 20px 20px;
      }
      
      .advanced-filters {
        padding: 15px;
      }
      
      .filter-options {
        gap: 8px;
      }
    }

    @media (min-width: 769px) {
      .jobs-search-container {
        padding: 24px;
        bottom: 0px;
      }

      .input-container {
        max-width: 800px;
        margin: 0 auto;
      }

      #jobsSearchInput {
        height: 70px;
        font-size: 1.05rem;
        padding: 0 50px 0 24px;
        border-radius: 16px;
      }

      .voice-search {
        width: 50px;
        height: 50px;
        right: 8px;
      }

      .voice-search i {
        font-size: 1.7em;
      }

      .jobs-grid {
        padding-bottom: 180px;
      }
      
      .advanced-filters {
        padding: 30px;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 30px;
      }
      
      .filter-actions {
        grid-column: 1 / -1;
      }
    }
  `;
  document.head.appendChild(style);
}

function renderJobsCards(filteredData) {
  return filteredData.map(job => `
    <div class="jobs-card" onclick="openJobDetail('${job.category}', '${job.details.title.replace(/'/g, "\\'")}')">
      <img src="${job.details.image}" alt="${job.details.title}" class="jobs-card-image">
      <div class="jobs-card-content">
        <span class="jobs-card-category">${job.category}</span>
        <h3 class="jobs-card-title">${job.details.title}</h3>
        <p class="jobs-card-company">${job.details.company}</p>
        <div class="jobs-card-location">
          <i class="fas fa-map-marker-alt"></i> ${job.details.location}
        </div>
        <p class="jobs-card-salary">${job.details.salary}</p>
        <div class="jobs-card-meta">
          <span class="jobs-card-type">${job.details.type}</span>
          <span class="jobs-card-date">
            <i class="far fa-calendar-alt"></i> ${job.details.posted}
          </span>
        </div>
      </div>
    </div>
  `).join('');
}

function openJobDetail(category, title) {
  const job = jobsData.find(item => 
    item.category === category && item.details.title === title.replace(/\\'/g, "'")
  );
  
  if (!job) return;

  const jobPage = document.createElement('div');
  jobPage.className = 'job-detail-page';
  jobPage.innerHTML = `
    <div class="job-detail-header">
      <button class="back-button" onclick="closeJobDetail()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <button class="share-button" onclick="shareJob('${job.details.title.replace(/'/g, "\\'")}')">
        <i class="fas fa-share"></i>
      </button>
    </div>
    
    <div class="job-hero">
      <div class="hero-gradient"></div>
      <img src="${job.details.image}" alt="${job.details.title}" class="hero-image">
      <div class="hero-content">
        <div class="job-category">${job.category}</div>
        <h1>${job.details.title}</h1>
        <div class="job-company">${job.details.company}</div>
        <div class="job-meta-container">
          <div class="job-meta-scroll">
            <div class="job-meta">
              <div class="meta-item">
                <i class="fas fa-map-marker-alt"></i>
                <span>${job.details.location}</span>
              </div>
              <div class="meta-item">
                <i class="fas fa-dollar-sign"></i>
                <span>${job.details.salary}</span>
              </div>
              <div class="meta-item">
                <i class="fas fa-briefcase"></i>
                <span>${job.details.type}</span>
              </div>
              <div class="meta-item">
                <i class="fas fa-clock"></i>
                <span>Posted ${job.details.posted}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <div class="job-content-container">
      <div class="job-content">
        ${job.details.content}
        
        <div class="job-actions">
          <a href="${job.details.applyUrl}" target="_blank" class="apply-button">Apply Now</a>
        </div>
      </div>
      
      <div class="job-sidebar">
        <div class="sidebar-section">
          <h3>Job Details</h3>
          <div class="detail-item">
            <i class="fas fa-calendar-alt"></i>
            <div>
              <span class="detail-label">Posted Date</span>
              <span class="detail-value">${job.details.posted}</span>
            </div>
          </div>
          <div class="detail-item">
            <i class="fas fa-briefcase"></i>
            <div>
              <span class="detail-label">Job Type</span>
              <span class="detail-value">${job.details.type}</span>
            </div>
          </div>
          <div class="detail-item">
            <i class="fas fa-dollar-sign"></i>
            <div>
              <span class="detail-label">Salary</span>
              <span class="detail-value">${job.details.salary}</span>
            </div>
          </div>
          <div class="detail-item">
            <i class="fas fa-map-marker-alt"></i>
            <div>
              <span class="detail-label">Location</span>
              <span class="detail-value">${job.details.location}</span>
            </div>
          </div>
        </div>
        
        <div class="sidebar-section">
          <h3>Tags</h3>
          <div class="tags-container">
            ${job.details.tags.map(tag => `
              <span class="tag">${tag}</span>
            `).join('')}
          </div>
        </div>
      </div>
    </div>
  `;

  document.body.appendChild(jobPage);
  addJobDetailStyles();
  scrollToTop();
  
  // Add scroll effect for header
  const header = document.querySelector('.job-detail-header');
  const hero = document.querySelector('.job-hero');
  
  if (header && hero) {
    const heroHeight = hero.offsetHeight;
    
    window.addEventListener('scroll', () => {
      if (window.scrollY > heroHeight - 100) {
        header.classList.add('scrolled');
      } else {
        header.classList.remove('scrolled');
      }
    });
  }
}

function addJobDetailStyles() {
  const style = document.createElement('style');
  style.textContent = `
    .job-detail-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      color: #333;
      font-family: poppins;
      line-height: 1.6;
    }

    .job-detail-header {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      padding: 15px 20px;
      background: transparent;
      z-index: 100;
      display: flex;
      justify-content: space-between;
      align-items: center;
      transition: all 0.3s ease;
    }

    .job-detail-header.scrolled {
      background: rgba(255, 255, 255, 0.98);
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
      border-bottom: 1px solid rgba(0, 0, 0, 0.05);
    }

    .back-button {
      background: rgba(0, 0, 0, 0.7);
      border: none;
      color: white;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.3s ease;
      backdrop-filter: blur(5px);
      -webkit-backdrop-filter: blur(5px);
    }

    .job-detail-header.scrolled .back-button {
      background: rgba(0, 0, 0, 0.05);
      color: #333;
    }

    .back-button:hover {
      transform: translateX(-3px);
    }

    .share-button {
      background: rgba(0, 0, 0, 0.7);
      border: none;
      color: white;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.3s ease;
      backdrop-filter: blur(5px);
      -webkit-backdrop-filter: blur(5px);
    }

    .job-detail-header.scrolled .share-button {
      background: rgba(0, 0, 0, 0.05);
      color: #333;
    }

    .share-button:hover {
      transform: scale(1.1);
    }

    .job-hero {
      position: relative;
      width: 100%;
      height: 55vh;
      min-height: 350px;
      max-height: 600px;
      overflow: hidden;
    }
    
    .hero-gradient {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 70%;
      background: linear-gradient(to top, rgba(0,0,0,0.9) 0%, transparent 100%);
      z-index: 2;
    }
    
    .hero-image {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      z-index: 1;
    }
    
    .hero-content {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      padding: 25px 20px;
      z-index: 3;
      color: white;
      max-width: 1200px;
      margin: 0 auto;
      box-sizing: border-box;
    }
    
    .job-category {
      display: inline-block;
      background: rgba(255, 215, 0, 0.9);
      color: #111;
      padding: 6px 15px;
      border-radius: 20px;
      font-size: 0.85em;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      margin-bottom: 15px;
    }
    
    .hero-content h1 {
      font-size: 2em;
      font-weight: 800;
      margin: 0 0 10px 0;
      line-height: 1.2;
      text-shadow: 0 2px 10px rgba(0,0,0,0.3);
    }
    
    .job-company {
      font-size: 1.2em;
      margin: 0 0 15px 0;
      color: rgba(255,255,255,0.9);
    }
    
    .job-meta-container {
      width: 100%;
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      padding-bottom: 10px;
    }
    
    .job-meta-scroll {
      display: inline-block;
      min-width: 100%;
    }
    
    .job-meta {
      display: inline-flex;
      gap: 15px;
      padding: 8px 0;
    }
    
    .meta-item {
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 0.9em;
      background: rgba(0, 0, 0, 0.4);
      padding: 8px 16px;
      border-radius: 20px;
      backdrop-filter: blur(5px);
      -webkit-backdrop-filter: blur(5px);
      white-space: nowrap;
      flex-shrink: 0;
      border: 1px solid rgba(255, 215, 0, 0.2);
    }
    
    .meta-item i {
      color: var(--primary-color);
      font-size: 0.9em;
    }
    
    /* Hide scrollbar but keep functionality */
    .job-meta-container::-webkit-scrollbar {
      display: none;
    }
    
    /* Mobile Responsiveness */
    @media (max-width: 768px) {
      .job-hero {
        height: 50vh;
        min-height: 300px;
      }
      
      .hero-content {
        padding: 20px 15px;
      }
      
      .hero-content h1 {
        font-size: 1.7em;
      }
      
      .job-company {
        font-size: 1.1em;
        margin-bottom: 12px;
      }
      
      .job-meta {
        gap: 10px;
      }
      
      .meta-item {
        font-size: 0.85em;
        padding: 6px 12px;
      }
    }
    
    @media (max-width: 480px) {
      .job-hero {
        height: 45vh;
        min-height: 250px;
      }
      
      .hero-content h1 {
        font-size: 1.5em;
      }
      
      .job-category {
        font-size: 0.75em;
        padding: 4px 12px;
        margin-bottom: 10px;
      }
      
      .job-company {
        font-size: 1em;
      }
      
      .meta-item {
        font-size: 0.8em;
        padding: 5px 10px;
      }
      
      .hero-gradient {
        height: 80%;
      }
    }
    
    @media (max-width: 360px) {
      .job-hero {
        height: 40vh;
        min-height: 220px;
      }
      
      .hero-content h1 {
        font-size: 1.4em;
      }
    }

    .job-content-container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 40px 20px;
      position: relative;
      background: #1a1a1f;
      border-radius: 30px 30px 0 0;
      margin-top: -30px;
      z-index: 5;
      display: grid;
      grid-template-columns: 2fr 1fr;
      gap: 30px;
    }

    .job-content {
      margin: 0px;
      font-size: 1.1em;
      line-height: 1.8;
      color: #fff;
    }

    .job-content h2 {
      font-size: 1.6em;
      margin: 1.5em 0 0.8em;
      margin-top: 0px;
      font-weight: 700;
      color: #fff;
    }

    .job-content p {
      margin-bottom: 1.5em;
    }

    .job-content ul, .job-content ol {
      margin-bottom: 1.5em;
      padding-left: 1.5em;
    }

    .job-content li {
      margin-bottom: 0.5em;
    }

    .job-content blockquote {
      border-left: 4px solid #ffd700;
      padding: 15px 20px;
      margin: 2em 0;
      background: rgba(255, 255, 255, 0.05);
      font-style: italic;
      color: #fff;
    }

    .job-content table {
      width: 100%;
      border-collapse: collapse;
      margin: 2em 0;
      box-shadow: 0 2px 15px rgba(0,0,0,0.1);
    }

    .job-content th, .job-content td {
      padding: 12px 15px;
      text-align: left;
      border-bottom: 1px solid rgba(255,255,255,0.1);
    }

    .job-content th {
      background: rgba(255, 215, 0, 0.1);
      font-weight: 600;
      color: #fff;
    }

    .job-content tr:hover {
      background: rgba(255,255,255,0.05);
    }

    .job-actions {
      margin-top: 60px;
    }

    .apply-button {
      background: var(--primary-color);
      color: #000;
      padding: 15px 30px;
      border-radius: 8px;
      font-weight: 600;
      text-decoration: none;
      transition: all 0.3s ease;
      display: inline-block;
      text-align: center;
      width: 100%;
      box-sizing: border-box;
    }

    .apply-button:hover {
      opacity: 0.9;
      transform: translateY(-2px);
    }

    .job-sidebar {
      position: sticky;
      top: 20px;
      align-self: start;
    }

    .sidebar-section {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 12px;
      padding: 20px;
      margin-bottom: 20px;
    }

    .sidebar-section h3 {
      margin-top: 0;
      margin-bottom: 20px;
      color: #fff;
      font-size: 1.3em;
    }

    .detail-item {
      display: flex;
      gap: 15px;
      margin-bottom: 15px;
      align-items: center;
    }

    .detail-item i {
      color: var(--primary-color);
      font-size: 1.2em;
      min-width: 20px;
    }

    .detail-label {
      display: block;
      font-size: 0.85em;
      color: rgba(255,255,255,0.7);
    }

    .detail-value {
      display: block;
      font-weight: 500;
      color: #fff;
    }

    .tags-container {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }

    .tag {
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
      padding: 8px 15px;
      border-radius: 20px;
      font-size: 0.85em;
      transition: all 0.3s ease;
    }

    .tag:hover {
      background: var(--primary-color);
      color: #000;
    }

    .specialties, .topics, .skills {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin: 15px 0;
    }

    .specialty, .topic, .skill {
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
      padding: 8px 15px;
      border-radius: 20px;
      font-size: 0.9em;
    }

    .metrics {
      display: flex;
      gap: 20px;
      margin: 20px 0;
      flex-wrap: wrap;
    }

    .metric {
      text-align: center;
    }

    .metric-value {
      display: block;
      font-size: 1.8em;
      font-weight: 700;
      color: var(--primary-color);
    }

    .metric-label {
      display: block;
      font-size: 0.9em;
      color: rgba(255,255,255,0.7);
    }

    /* Mobile Responsiveness */
    @media (max-width: 768px) {      
      .job-content-container {
        padding: 30px 15px;
        margin-top: -20px;
        border-radius: 20px 20px 0 0;
        grid-template-columns: 1fr;
      }
      
      .job-content {
        font-size: 1em;
      }
      
      .job-content h2 {
        font-size: 1.4em;
      }
      
      .job-sidebar {
        position: static;
        margin-top: 30px;
      }
      
      .sidebar-section {
        padding: 15px;
      }
      
      .detail-item {
        gap: 10px;
      }
      
      .apply-button {
        padding: 15px 20px;
      }
    }

    @media (max-width: 480px) {            
      .job-content-container {
        padding: 25px 10px;
      }
    }
  `;
  document.head.appendChild(style);
}

function closeJobDetail() {
  const jobPage = document.querySelector('.job-detail-page');
  if (jobPage) {
    jobPage.remove();
  }
}

function saveJob(title) {
  // In a real app, this would save to user's account
  alert(`Job "${title}" saved to your favorites!`);
  
  // Toggle save button state
  const saveButtons = document.querySelectorAll('.save-button, .save-job-button');
  saveButtons.forEach(button => {
    button.innerHTML = button.innerHTML.includes('fa-bookmark') 
      ? '<i class="fas fa-bookmark"></i> Saved' 
      : '<i class="far fa-bookmark"></i> Save Job';
  });
}

function shareJob(title) {
  if (navigator.share) {
    navigator.share({
      title: title,
      text: 'Check out this job opportunity!',
      url: window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    // Fallback for browsers that don't support Web Share API
    const shareUrl = `https://twitter.com/intent/tweet?text=${encodeURIComponent(title)}&url=${encodeURIComponent(window.location.href)}`;
    window.open(shareUrl, '_blank');
  }
}

function openRelatedJob(title) {
  // In a real app, this would open the related job
  alert(`This would open the job: "${title}" in a real application.`);
}

function hideJobsOverlay() {
  const overlay = document.getElementById('jobsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => overlay.remove(), 300);
  }
}

function filterJobs(category) {
  const jobsGrid = document.getElementById('jobsGrid');
  let filteredData = jobsData;

  if (category !== 'All') {
    filteredData = filteredData.filter(job => job.category === category);
  }

  jobsGrid.innerHTML = renderJobsCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function performJobsSearch(data, query, gridElement, filterButtons, renderFunction) {
  if (!query) {
    // If no query, show all jobs
    gridElement.innerHTML = renderFunction(data);
    return;
  }

  const lowerCaseQuery = query.toLowerCase();
  const filteredData = data.filter(job => {
    return (
      job.details.title.toLowerCase().includes(lowerCaseQuery) ||
      job.details.company.toLowerCase().includes(lowerCaseQuery) ||
      job.details.location.toLowerCase().includes(lowerCaseQuery) ||
      job.category.toLowerCase().includes(lowerCaseQuery) ||
      (job.details.tags && job.details.tags.some(tag => tag.toLowerCase().includes(lowerCaseQuery)))
    );
  });

  gridElement.innerHTML = filteredData.length > 0 
    ? renderFunction(filteredData)
    : '<div class="no-results">No jobs found matching your search.</div>';
}

function initializeJobsVoiceSearch(inputElement, callback) {
  const voiceButton = document.getElementById('jobsVoiceSearchButton');
  if (!voiceButton) return;

  let recognition;
  try {
    recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.continuous = false;
    recognition.interimResults = false;

    voiceButton.addEventListener('click', () => {
      if (voiceButton.classList.contains('active')) {
        recognition.stop();
        voiceButton.classList.remove('active');
        return;
      }

      voiceButton.classList.add('active');
      recognition.start();

      recognition.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        inputElement.value = transcript;
        if (callback) callback(transcript);
        voiceButton.classList.remove('active');
      };

      recognition.onerror = (event) => {
        console.error('Voice recognition error', event.error);
        voiceButton.classList.remove('active');
      };

      recognition.onend = () => {
        voiceButton.classList.remove('active');
      };
    });
  } catch (e) {
    console.error('Voice recognition not supported', e);
    voiceButton.style.display = 'none';
  }
}

function toggleAdvancedFilters() {
  const filters = document.getElementById('advancedFilters');
  filters.classList.toggle('active');
}

function applyAdvancedFilters() {
  // Get all filter values
  const jobTypeCheckboxes = document.querySelectorAll('input[name="job-type"]:checked');
  const experienceCheckboxes = document.querySelectorAll('input[name="experience"]:checked');
  const salaryMin = document.getElementById('salaryMin').value;
  const salaryMax = document.getElementById('salaryMax').value;
  
  const selectedJobTypes = Array.from(jobTypeCheckboxes).map(cb => cb.value);
  const selectedExperience = Array.from(experienceCheckboxes).map(cb => cb.value);
  
  // Filter jobs
  let filteredJobs = jobsData.filter(job => {
    // Job type filter
    if (selectedJobTypes.length > 0 && !selectedJobTypes.includes(job.details.type)) {
      return false;
    }
    
    // Experience filter (simplified for demo)
    if (selectedExperience.length > 0) {
      const jobExp = job.details.experience.toLowerCase();
      if (selectedExperience.some(exp => jobExp.includes(exp.toLowerCase()))) {
        return true;
      }
      return false;
    }
    
    // Salary filter (simplified for demo)
    const salary = job.details.salary;
    const numericSalary = extractNumberFromSalary(salary);
    if (numericSalary) {
      if (numericSalary < salaryMin || numericSalary > salaryMax) {
        return false;
      }
    }
    
    return true;
  });
  
  // Update grid
  const jobsGrid = document.getElementById('jobsGrid');
  jobsGrid.innerHTML = filteredJobs.length > 0 
    ? renderJobsCards(filteredJobs)
    : '<div class="no-results">No jobs match all your filters.</div>';
  
  // Close filters
  toggleAdvancedFilters();
}

function resetFilters() {
  // Reset checkboxes
  document.querySelectorAll('input[type="checkbox"]').forEach(cb => {
    cb.checked = true;
  });
  
  // Reset salary range
  document.getElementById('salaryMin').value = 0;
  document.getElementById('salaryMax').value = 500000;
  document.getElementById('minSalaryValue').textContent = '$0';
  document.getElementById('maxSalaryValue').textContent = '$500k+';
  
  // Reset grid
  const jobsGrid = document.getElementById('jobsGrid');
  jobsGrid.innerHTML = renderJobsCards(jobsData);
}

function extractNumberFromSalary(salaryText) {
  // Simple extraction for demo - would need more robust parsing in real app
  const match = salaryText.match(/\$([\d,]+)/);
  if (match) {
    return parseInt(match[1].replace(/,/g, ''));
  }
  return null;
}

function formatSalary(value) {
  const num = parseInt(value);
  if (num >= 100000) {
    return `$${Math.floor(num/1000)}k+`;
  }
  return `$${num.toLocaleString()}`;
}

// Utility function for debouncing
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

document.addEventListener('DOMContentLoaded', function() {
  // Get the jobs button element
  const jobsButton = document.getElementById('jobsButton');
  
  // Add click event listener
  if (jobsButton) {
    jobsButton.addEventListener('click', function(e) {
      e.preventDefault(); // Prevent default behavior if needed
      showJobsOverlay(); // This opens the jobs feed
    });
  }
});

const couponData = [
  // Food & Beverage
  {
    category: "Food & Beverage",
    details: {
      title: "Starbucks: Buy 1 Get 1 Free on All Frappuccinos",
      image: "https://b.zmtcdn.com/data/pictures/chains/7/96507/9b60e87fc9c1d60fbb4c2205ccdbd5ef.jpg",
      description: "Get any Frappuccino and receive a second one free. Valid at participating locations.",
      code: "BOGOFREE24",
      expiry: "June 30, 2024",
      terms: "Valid only on grande or venti sizes. Not valid with other offers.",
      content: `
        <h2>Starbucks Summer Special</h2>
        <p>Enjoy Starbucks' famous Frappuccinos with this amazing buy one get one free offer:</p>
        <ul>
          <li>Valid on all Frappuccino flavors</li>
          <li>Participating locations only</li>
          <li>Present code at checkout</li>
          <li>Limit one offer per customer per visit</li>
        </ul>
        <p>This offer is perfect for trying new summer flavors like:</p>
        <ul>
          <li>Caramel Ribbon Crunch</li>
          <li>Mocha Cookie Crumble</li>
          <li>Strawberry Funnel Cake</li>
        </ul>
        <p>Offer expires June 30, 2024 or while supplies last.</p>
      `,
      tags: ["Coffee", "Beverage", "Summer Special"],
      store: "Starbucks",
      social: {
        twitter: "https://twitter.com/Starbucks",
        instagram: "https://instagram.com/starbucks"
      },
      related: [
        { title: "20% Off All Pastries", url: "#" },
        { title: "Free Refills for Rewards Members", url: "#" }
      ]
    }
  },
  {
    category: "Food & Beverage",
    details: {
      title: "McDonald's: Free Medium Fries with Any Purchase",
      image: "https://www.mcdonalds.com/content/dam/sites/usa/nfl/publication/1pub_Fries_Hero_1260x560.jpg",
      description: "Get free medium fries with any purchase in the McDonald's app. Mobile only.",
      code: "FREEFRIES",
      expiry: "July 15, 2024",
      terms: "Mobile order only. One offer per customer per day.",
      content: `
        <h2>Free Fries at McDonald's</h2>
        <p>Download the McDonald's app and get free medium fries with any purchase:</p>
        <ul>
          <li>Must use mobile ordering</li>
          <li>Valid on any purchase amount</li>
          <li>World Famous Fries included</li>
          <li>Participating US locations</li>
        </ul>
        <p>How to redeem:</p>
        <ol>
          <li>Download the McDonald's app</li>
          <li>Create an account or sign in</li>
          <li>Add any item to your cart</li>
          <li>Apply promo code FREEFRIES</li>
          <li>Complete your mobile order</li>
        </ol>
        <p>Offer valid through July 15, 2024.</p>
      `,
      tags: ["Fast Food", "Mobile App", "Free Item"],
      store: "McDonald's",
      social: {
        twitter: "https://twitter.com/McDonalds",
        facebook: "https://facebook.com/McDonalds"
      },
      related: [
        { title: "$1 Any Size Soft Drink", url: "#" },
        { title: "Breakfast Sandwich Combo Deal", url: "#" }
      ]
    }
  },
  
  // Retail
  {
    category: "Retail",
    details: {
      title: "Amazon: 30% Off Select Electronics",
      image: "https://m.media-amazon.com/images/I/71S5OpiaJZL._SL1500_.jpg",
      description: "Save 30% on select electronics including headphones, smartwatches, and tablets.",
      code: "ELECTRO30",
      expiry: "May 31, 2024",
      terms: "Valid on select items only. Limited quantities available.",
      content: `
        <h2>Amazon Electronics Sale</h2>
        <p>Massive 30% discount on top electronics brands:</p>
        <ul>
          <li>Headphones from Sony, Bose, and JBL</li>
          <li>Smartwatches including Apple Watch and Samsung Galaxy Watch</li>
          <li>Tablets from Amazon Fire, Samsung, and Lenovo</li>
        </ul>
        <p>How to redeem:</p>
        <ol>
          <li>Add eligible items to your cart</li>
          <li>Enter promo code ELECTRO30 at checkout</li>
          <li>Discount will be applied automatically</li>
        </ol>
        <p>Featured deals:</p>
        <table>
          <tr>
            <th>Item</th>
            <th>Original Price</th>
            <th>Discounted Price</th>
          </tr>
          <tr>
            <td>Sony WH-1000XM5 Headphones</td>
            <td>$399.99</td>
            <td>$279.99</td>
          </tr>
          <tr>
            <td>Apple Watch Series 9</td>
            <td>$429.00</td>
            <td>$300.30</td>
          </tr>
        </table>
        <p>Offer ends May 31, 2024 or while supplies last.</p>
      `,
      tags: ["Electronics", "Amazon", "Discount"],
      store: "Amazon",
      social: {
        twitter: "https://twitter.com/amazon",
        facebook: "https://facebook.com/amazon"
      },
      related: [
        { title: "Prime Member Exclusive Deals", url: "#" },
        { title: "Back to School Tech Bundle", url: "#" }
      ]
    }
  },
  
  // Travel
  {
    category: "Travel",
    details: {
      title: "Uber: 50% Off First 3 Rides (New Users)",
      image: "https://www.uber-assets.com/image/upload/f_auto,q_auto:eco,c_fill,w_1899,h_804/v1613521692/assets/d9/ce6c00-32b0-4b93-9f0d-6f927d93da08/original/Rider_Home_bg_desktop2x.png",
      description: "New users get 50% off their first 3 Uber rides up to $15 discount per ride.",
      code: "UBER50NYSA",
      expiry: "December 31, 2024",
      terms: "New users only. Max $15 discount per ride. Valid in US only.",
      content: `
        <h2>Uber New User Discount</h2>
        <p>Welcome to Uber! Enjoy 50% off your first 3 rides:</p>
        <ul>
          <li>New users only</li>
          <li>Maximum $15 discount per ride</li>
          <li>Valid on UberX, Uber Comfort, and UberXL</li>
          <li>Available in all US cities where Uber operates</li>
        </ul>
        <p>How to redeem:</p>
        <ol>
          <li>Download the Uber app</li>
          <li>Create a new account</li>
          <li>Enter promo code UBER50NYSA in the Payments section</li>
          <li>Request your ride as normal</li>
          <li>Discount will be automatically applied</li>
        </ol>
        <p>This offer is perfect for:</p>
        <ul>
          <li>Airport transfers</li>
          <li>Night out with friends</li>
          <li>Commuting to work</li>
          <li>Exploring new cities</li>
        </ul>
        <p>Offer valid through December 31, 2024.</p>
      `,
      tags: ["Transportation", "New User", "Discount"],
      store: "Uber",
      social: {
        twitter: "https://twitter.com/uber",
        instagram: "https://instagram.com/uber"
      },
      related: [
        { title: "Uber Eats: $20 Off First Order", url: "#" },
        { title: "Lyft: 25% Off 5 Rides", url: "#" }
      ]
    }
  },
  
  // Entertainment
  {
    category: "Entertainment",
    details: {
      title: "AMC Theatres: $5 Ticket Tuesdays",
      image: "https://www.amctheatres.com/media/v5/styles/utilities_generated/public/2023-06/AMC%20Theatres%20Hero%20Image%20Desktop%201600x500.jpg",
      description: "Every Tuesday, enjoy any standard movie for just $5 at AMC locations.",
      code: "TUESDAY5",
      expiry: "Ongoing",
      terms: "Standard shows only. Not valid on premium formats. One ticket per transaction.",
      content: `
        <h2>AMC $5 Tuesdays</h2>
        <p>Enjoy movies at AMC Theatres every Tuesday for just $5:</p>
        <ul>
          <li>Any standard movie showing</li>
          <li>All day every Tuesday</li>
          <li>No minimum purchase required</li>
          <li>Valid at all AMC locations nationwide</li>
        </ul>
        <p>How to redeem:</p>
        <ol>
          <li>Visit AMC website or app</li>
          <li>Select any Tuesday showtime</li>
          <li>Enter promo code TUESDAY5</li>
          <li>Complete your ticket purchase</li>
        </ol>
        <p>Note: This offer is not valid for:</p>
        <ul>
          <li>IMAX, Dolby Cinema, or other premium formats</li>
          <li>Special events or private screenings</li>
          <li>Concessions or other purchases</li>
        </ul>
        <p>This is an ongoing promotion with no expiration date.</p>
      `,
      tags: ["Movies", "Theater", "Discount"],
      store: "AMC Theatres",
      social: {
        twitter: "https://twitter.com/AMCTheatres",
        facebook: "https://facebook.com/amctheatres"
      },
      related: [
        { title: "AMC Stubs Membership Benefits", url: "#" },
        { title: "Free Popcorn Wednesdays", url: "#" }
      ]
    }
  },
  
  // Add more coupon articles as needed
];

// Function to show the coupon overlay
function showCouponOverlay() {
  let overlay = document.getElementById('couponOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'couponOverlay';
    overlay.className = 'coupon-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(couponData.map(coupon => coupon.category))];

  overlay.innerHTML = `
    <div class="coupon-content">
      <div class="coupon-header">
        <h2>Nysa Coupons</h2>
        <button class="close-coupon" onclick="hideCouponOverlay()">
          <i class="fas fa-times"></i>
        </button>
      </div>
      
      <div class="coupon-filter-container">
        <div class="coupon-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterCoupons('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="coupon-grid" id="couponGrid">
        ${renderCouponCards(couponData)}
      </div>
    </div>

    <div class="coupon-search-container">
      <div class="input-container">
        <input type="text" id="couponSearchInput" placeholder="Search for coupons, stores, or categories..." />
        <button class="voice-search" id="couponVoiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const searchInput = document.getElementById('couponSearchInput');
  const couponGrid = document.getElementById('couponGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performCouponSearch(couponData, query, couponGrid, filterButtons, renderCouponCards);
  }, 300);

  searchInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  searchInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performCouponSearch(couponData, searchInput.value, couponGrid, filterButtons, renderCouponCards);
    }
  });

  initializeCouponVoiceSearch(searchInput, (query) => {
    performCouponSearch(couponData, query, couponGrid, filterButtons, renderCouponCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
    .coupon-overlay {
      position: fixed;
      top: 60px;
      left: 0;
      right: 0;
      bottom: 60px;
      background: #1a1a1f;
      z-index: 1000;
      opacity: 0;
      visibility: hidden;
      transition: all 0.3s ease;
      overflow-y: auto;
      width: 100%;
      display: flex;
      flex-direction: column;
      scrollbar-width: none;
      -ms-overflow-style: none;
    }

    .coupon-overlay::-webkit-scrollbar {
      display: none;
    }

    .coupon-overlay.active {
      opacity: 1;
      visibility: visible;
    }

    .coupon-content {
      flex: 1;
      width: 100%;
      max-width: 100%;
      padding: 20px;
      margin: 0 auto;
      position: relative;
    }

    .coupon-header {
      padding: 16px 24px;
      background: rgba(26, 26, 31, 0.95);
      position: fixed;
      width: 100%;
      top: 0;
      left: 0;
      z-index: 30;
      border-bottom: 1px solid rgba(255, 215, 0, 0.15);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      height: 72px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }

    .coupon-header h2 {
      color: var(--primary-color);
      margin: 0;
      font-size: 1.75em;
      font-weight: 600;
      letter-spacing: -0.02em;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .coupon-header h2::before {
      content: '';
      display: inline-block;
      width: 24px;
      height: 24px;
      background: var(--primary-color);
      mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M19 21l-7-5-7 5V5a2 2 0 0 1 2-2h10a2 2 0 0 1 2 2z'%3E%3C/path%3E%3C/svg%3E");
      mask-repeat: no-repeat;
      mask-position: center;
      mask-size: contain;
    }

    .close-coupon {
      background: rgba(255, 255, 255, 0.1);
      border: none;
      color: white;
      font-size: 1em;
      cursor: pointer;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s ease;
    }

    .close-coupon:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: scale(1.05);
    }

    .close-coupon:active {
      transform: scale(0.95);
    }

    .coupon-filter-container {
      position: fixed;
      top: 72px;
      left: 0;
      right: 0;
      width: 100%;
      z-index: 10;
      background: #1a1a1f;
      padding: 12px 0;
      box-sizing: border-box;
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      scrollbar-width: none;
      border-bottom: 1px solid rgba(255, 215, 0, 0.1);
    }

    .coupon-filter-container::-webkit-scrollbar {
      display: none;
    }

    .coupon-filter-buttons {
      display: inline-flex;
      padding: 0 16px;
      gap: 8px;
      white-space: nowrap;
    }

    .filter-button {
      font-family: poppins;
      flex-shrink: 0;
      background: rgba(255, 215, 0, 0.1);
      color: rgba(255, 255, 255, 0.9);
      border: 1px solid rgba(255, 215, 0, 0.2);
      padding: 10px 20px;
      border-radius: 25px;
      transition: all 0.3s ease;
      cursor: pointer;
      font-size: 14px;
      font-weight: 500;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
      margin: 0;
    }

    .filter-button:hover {
      background: rgba(255, 215, 0, 0.15);
      transform: scale(1.03);
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    .filter-button.active {
      background: var(--primary-color);
      color: #000;
      font-weight: 600;
      border-color: var(--primary-color);
      box-shadow: 0 4px 10px rgba(255, 215, 0, 0.3);
    }

    /* Add some space at the end for better scrolling */
    .coupon-filter-buttons::after {
      content: '';
      display: block;
      min-width: 16px;
      height: 1px;
    }

    .coupon-grid {
      margin-top: 70px;
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
      gap: 20px;
      animation: fadeInUp 0.5s ease-out;
      padding-bottom: 140px;
      width: 100%;
    }

    .coupon-card {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      overflow: hidden;
      transition: all 0.3s ease;
      border: 1px solid rgba(255, 255, 255, 0.1);
      cursor: pointer;
      position: relative;
    }

    .coupon-card:hover {
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
      border-color: rgba(255, 215, 0, 0.3);
    }

    .coupon-card-image {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .coupon-card-content {
      padding: 20px;
    }

    .coupon-card-category {
      display: inline-block;
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
      padding: 4px 12px;
      border-radius: 20px;
      font-size: 0.8em;
      font-weight: 600;
      margin-bottom: 12px;
    }

    .coupon-card-title {
      font-size: 1.3em;
      font-weight: 700;
      margin: 0 0 10px 0;
      color: white;
      line-height: 1.3;
    }

    .coupon-card-description {
      font-size: 0.95em;
      color: rgba(255, 255, 255, 0.8);
      margin: 0 0 15px 0;
      line-height: 1.5;
    }

    .coupon-card-meta {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.85em;
      color: rgba(255, 255, 255, 0.6);
    }

    .coupon-card-store {
      font-weight: 500;
    }

    .coupon-card-expiry {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .coupon-code-container {
      display: flex;
      align-items: center;
      justify-content: space-between;
      background: rgba(255, 215, 0, 0.1);
      border: 1px dashed rgba(255, 215, 0, 0.5);
      padding: 10px 15px;
      border-radius: 8px;
      margin-top: 15px;
    }

    .coupon-code {
      font-family: monospace;
      font-size: 1.1em;
      font-weight: bold;
      color: var(--primary-color);
    }

    .copy-button {
      background: var(--primary-color);
      color: #000;
      border: none;
      padding: 5px 10px;
      border-radius: 4px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s ease;
    }

    .copy-button:hover {
      opacity: 0.9;
    }

    .copy-button:active {
      transform: scale(0.95);
    }

    .coupon-search-container {
      position: fixed;
      bottom: 0px;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      display: flex;
      gap: 12px;
      align-items: center;
      box-sizing: border-box;
    }

    .input-container {
      flex: 1;
      position: relative;
      display: flex;
      align-items: center;
    }

    #couponSearchInput {
      flex: 1;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 12px 50px 12px 20px;
      color: #fff;
      font-size: 0.95em;
      width: 100%;
      transition: border-color 0.3s ease;
    }

    #couponSearchInput:focus {
      border-color: rgba(255, 255, 255, 0.5);
    }

    .voice-search {
      position: absolute;
      right: 10px;
      background: none;
      border: none;
      color: var(--primary-color);
      cursor: pointer;
      padding: 10px;
      transition: all 0.3s ease;
    }

    .voice-search:hover {
      opacity: 0.8;
    }

    .voice-search.active {
      color: #ffd700;
      animation: pulse 1.5s infinite;
    }

    .voice-search i { 
      font-size: 1.5em; 
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .coupon-grid {
        grid-template-columns: 1fr;
        padding: 0 10px;
        margin-top: 80px;
        gap: 15px;
      }

      .coupon-card {
        width: 100%;
        margin: 0;
      }

      .coupon-card-image {
        height: 180px;
      }

      .coupon-content {
        padding: 10px 0;
        padding-bottom: 200px;
      }

      #couponSearchInput {
        padding: 20px 20px;
      }
    }

    @media (min-width: 769px) {
      .coupon-search-container {
        padding: 24px;
        bottom: 0px;
      }

      .input-container {
        max-width: 800px;
        margin: 0 auto;
      }

      #couponSearchInput {
        height: 70px;
        font-size: 1.05rem;
        padding: 0 50px 0 24px;
        border-radius: 16px;
      }

      .voice-search {
        width: 50px;
        height: 50px;
        right: 8px;
      }

      .voice-search i {
        font-size: 1.7em;
      }

      .coupon-grid {
        padding-bottom: 180px;
      }
    }

    /* ===== Laptop & Desktop (1025px+) - Compact Version ===== */
@media (min-width: 1025px) {
  .coupon-grid {
    grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
    gap: 16px;
    padding-bottom: 120px;
  }

  .coupon-card {
    border-radius: 14px;
  }

  .coupon-card-image {
    height: 180px;
  }

  .coupon-card-content {
    padding: 16px;
  }

  .coupon-card-title {
    font-size: 1.15em;
    margin-bottom: 8px;
  }

  .coupon-card-description {
    font-size: 0.85em;
    margin-bottom: 12px;
    -webkit-line-clamp: 3;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  .coupon-card-meta {
    font-size: 0.8em;
  }

  .coupon-code-container {
    padding: 8px 12px;
    margin-top: 12px;
  }

  .coupon-code {
    font-size: 1em;
  }

  .copy-button {
    padding: 4px 8px;
    font-size: 0.85em;
  }
}


  `;
  document.head.appendChild(style);
}

function renderCouponCards(filteredData) {
  return filteredData.map(coupon => `
    <div class="coupon-card" onclick="openCouponDetail('${coupon.category}', '${coupon.details.title.replace(/'/g, "\\'")}')">
      <img src="${coupon.details.image}" alt="${coupon.details.title}" class="coupon-card-image">
      <div class="coupon-card-content">
        <span class="coupon-card-category">${coupon.category}</span>
        <h3 class="coupon-card-title">${coupon.details.title}</h3>
        <p class="coupon-card-description">${coupon.details.description}</p>
        <div class="coupon-card-meta">
          <span class="coupon-card-store">${coupon.details.store}</span>
          <span class="coupon-card-expiry">
            <i class="far fa-clock"></i> Expires: ${coupon.details.expiry}
          </span>
        </div>
        <div class="coupon-code-container">
          <span class="coupon-code">${coupon.details.code}</span>
          <button class="copy-button" onclick="event.stopPropagation(); copyCouponCode('${coupon.details.code}')">
            Copy Code
          </button>
        </div>
      </div>
    </div>
  `).join('');
}

function openCouponDetail(category, title) {
  const coupon = couponData.find(item => 
    item.category === category && item.details.title === title.replace(/\\'/g, "'")
  );
  
  if (!coupon) return;

  const couponPage = document.createElement('div');
  couponPage.className = 'coupon-detail-page';
  couponPage.innerHTML = `
    <div class="coupon-detail-header">
      <button class="back-button" onclick="closeCouponDetail()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <div class="header-actions">
        <button class="share-button" onclick="shareCoupon('${coupon.details.title.replace(/'/g, "\\'")}', '${coupon.details.code}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="coupon-hero">
      <div class="hero-gradient"></div>
      <img src="${coupon.details.image}" alt="${coupon.details.title}" class="hero-image">
      <div class="hero-content">
        <div class="coupon-category">${coupon.category}</div>
        <h1>${coupon.details.title}</h1>
        <div class="coupon-meta">
          <div class="store-info">
            <span>${coupon.details.store}</span>
            <span class="meta-divider">•</span>
            <span>Expires: ${coupon.details.expiry}</span>
          </div>
        </div>
      </div>
    </div>
    
    <div class="coupon-content-container">
      <div class="coupon-code-detail">
        <div class="code-display">
          <span>CODE: </span>
          <strong>${coupon.details.code}</strong>
        </div>
        <button class="copy-button-large" onclick="copyCouponCode('${coupon.details.code}')">
          Copy Code
        </button>
      </div>
      
      <div class="coupon-content">
        ${coupon.details.content}
      </div>
      
      <div class="terms-container">
        <h3>Terms & Conditions</h3>
        <p>${coupon.details.terms}</p>
      </div>
      
      <div class="coupon-footer">
        <div class="tags-container">
          ${coupon.details.tags.map(tag => `
            <span class="tag">${tag}</span>
          `).join('')}
        </div>
        
        <div class="source-coupon">
          <div class="social-coupon">
            ${coupon.details.social?.twitter ? `<a href="${coupon.details.social.twitter}" target="_blank"><i class="fab fa-twitter"></i></a>` : ''}
            ${coupon.details.social?.facebook ? `<a href="${coupon.details.social.facebook}" target="_blank"><i class="fab fa-facebook-f"></i></a>` : ''}
            ${coupon.details.social?.instagram ? `<a href="${coupon.details.social.instagram}" target="_blank"><i class="fab fa-instagram"></i></a>` : ''}
          </div>
        </div>
      </div>
    </div>
  `;

  document.body.appendChild(couponPage);
  addCouponDetailStyles();
  scrollToTop();
  
  // Add scroll effect for header
  const header = document.querySelector('.coupon-detail-header');
  const hero = document.querySelector('.coupon-hero');
  
  if (header && hero) {
    const heroHeight = hero.offsetHeight;
    
    window.addEventListener('scroll', () => {
      if (window.scrollY > heroHeight - 100) {
        header.classList.add('scrolled');
      } else {
        header.classList.remove('scrolled');
      }
    });
  }
}

function addCouponDetailStyles() {
  const style = document.createElement('style');
  style.textContent = `
    .coupon-detail-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      color: #333;
      font-family: poppins;
      line-height: 1.6;
    }

    .coupon-detail-header {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      padding: 15px 20px;
      background: transparent;
      z-index: 100;
      display: flex;
      justify-content: space-between;
      align-items: center;
      transition: all 0.3s ease;
    }

    .coupon-detail-header.scrolled {
      background: rgba(255, 255, 255, 0.98);
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
      border-bottom: 1px solid rgba(0, 0, 0, 0.05);
    }

    .back-button {
      background: rgba(0, 0, 0, 0.7);
      border: none;
      color: white;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.3s ease;
      backdrop-filter: blur(5px);
      -webkit-backdrop-filter: blur(5px);
    }

    .coupon-detail-header.scrolled .back-button {
      background: rgba(0, 0, 0, 0.05);
      color: #333;
    }

    .back-button:hover {
      transform: translateX(-3px);
    }

    .header-actions {
      display: flex;
      gap: 10px;
    }

    .share-button {
      background: rgba(0, 0, 0, 0.7);
      border: none;
      color: white;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.3s ease;
      backdrop-filter: blur(5px);
      -webkit-backdrop-filter: blur(5px);
    }

    .coupon-detail-header.scrolled .share-button {
      background: rgba(0, 0, 0, 0.05);
      color: #333;
    }

    .share-button:hover {
      transform: scale(1.1);
    }

    .coupon-hero {
      position: relative;
      width: 100%;
      height: 70vh;
      min-height: 500px;
      max-height: 800px;
      overflow: hidden;
    }

    .hero-gradient {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 60%;
      background: linear-gradient(to top, rgba(0,0,0,0.8) 0%, transparent 100%);
      z-index: 2;
    }

    .hero-image {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      z-index: 1;
    }

    .hero-content {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      padding: 40px;
      z-index: 3;
      color: white;
      max-width: 1200px;
      margin: 0 auto;
      box-sizing: border-box;
    }

    .coupon-category {
      display: inline-block;
      background: rgba(255, 215, 0, 0.9);
      color: #111;
      padding: 6px 15px;
      border-radius: 20px;
      font-size: 0.85em;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      margin-bottom: 20px;
    }

    .hero-content h1 {
      font-size: 3em;
      font-weight: 800;
      margin: 0 0 20px 0;
      line-height: 1.2;
      text-shadow: 0 2px 10px rgba(0,0,0,0.3);
    }

    .coupon-meta {
      display: flex;
      align-items: center;
      font-size: 0.95em;
      color: rgba(255,255,255,0.9);
    }

    .store-info {
      display: flex;
      align-items: center;
      flex-wrap: wrap;
    }

    .meta-divider {
      margin: 0 10px;
      opacity: 0.7;
    }

    .coupon-content-container {
      max-width: 800px;
      margin: 0 auto;
      padding: 60px 20px;
      position: relative;
      background:  #1a1a1f;
      border-radius: 30px 30px 0 0;
      margin-top: 0px;
      z-index: 5;
    }

    .coupon-code-detail {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(255, 215, 0, 0.1);
      border: 2px dashed rgba(255, 215, 0, 0.5);
      padding: 20px;
      border-radius: 12px;
      margin-bottom: 30px;
    }

    .code-display {
      font-size: 1.2em;
      color: #fff;
    }

    .code-display strong {
      color: var(--primary-color);
      font-size: 1.4em;
    }

    .copy-button-large {
      background: var(--primary-color);
      color: #000;
      border: none;
      padding: 12px 24px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s ease;
      font-size: 1em;
    }

    .copy-button-large:hover {
      opacity: 0.9;
    }

    .copy-button-large:active {
      transform: scale(0.95);
    }

    .coupon-content {
      margin: 0px;
      font-size: 1.1em;
      line-height: 1.8;
      color: #fff;
    }

    .coupon-content h2 {
      font-size: 1.8em;
      margin: 1.5em 0 0.8em;
      margin-top: 0px;
      font-weight: 700;
      color: #fff;
    }

    .coupon-content p {
      margin-bottom: 1.5em;
    }

    .coupon-content ul, .coupon-content ol {
      margin-bottom: 1.5em;
      padding-left: 1.5em;
    }

    .coupon-content li {
      margin-bottom: 0.5em;
    }

    .coupon-content blockquote {
      border-left: 4px solid #ffd700;
      padding: 15px 20px;
      margin: 2em 0;
      background: #f9f9f9;
      font-style: italic;
      color: #555;
    }

    .coupon-content table {
      width: 100%;
      border-collapse: collapse;
      margin: 2em 0;
      box-shadow: 0 2px 15px rgba(0,0,0,0.05);
    }

    .coupon-content th, .coupon-content td {
      padding: 12px 15px;
      text-align: left;
      border-bottom: 1px solid #eee;
    }

    .coupon-content th {
      background: #f5f5f5;
      font-weight: 600;
      color: #333;
    }

    .coupon-content tr:hover {
      background: #000;
    }

    .terms-container {
      background: rgba(255, 255, 255, 0.05);
      padding: 20px;
      border-radius: 8px;
      margin-top: 40px;
      margin-bottom: 20px;
    }

    .terms-container h3 {
      color: var(--primary-color);
      margin-top: 0;
      margin-bottom: 15px;
    }

    .terms-container p {
      color: rgba(255, 255, 255, 0.7);
      font-size: 0.9em;
      line-height: 1.6;
      margin-bottom: 0;
    }

    .coupon-footer {
      margin-top: 60px;
    }

    .tags-container {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-bottom: 40px;
    }

    .tag {
      background: rgba(255, 215, 0, 0.1);
      padding: 8px 15px;
      border-radius: 20px;
      font-size: 0.85em;
      color: var(--primary-color);
      transition: all 0.3s ease;
      border: 1px solid rgba(255, 215, 0, 0.3);
    }

    .tag:hover {
      background: var(--primary-color);
      color: #111;
    }

    .source-coupon {
      display: flex;
      justify-content: flex-end;
      padding: 20px 0;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      margin-bottom: 0px;
      font-size: 0.9em;
      color: #fff;
    }

    .social-coupon {
      display: flex;
      gap: 15px;
    }

    .social-coupon a {
      color: var(--primary-color);
      transition: all 0.3s ease;
      font-size: 1.2em;
    }

    .social-coupon a:hover {
      color: #fff;
      transform: scale(1.1);
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .coupon-hero {
        height: 60vh;
        min-height: 400px;
      }
      
      .hero-content {
        padding: 30px 20px;
      }
      
      .hero-content h1 {
        font-size: 2.2em;
      }
      
      .coupon-content-container {
        padding: 40px 15px;
        margin-top: -20px;
        border-radius: 20px 20px 0 0;
      }
      
      .coupon-content {
        font-size: 1em;
      }
      
      .coupon-content h2 {
        font-size: 1.5em;
      }

      .coupon-code-detail {
        flex-direction: column;
        gap: 15px;
        text-align: center;
      }

      .terms-container {
        margin-top: 30px;
        padding: 15px;
      }
    }

    @media (max-width: 480px) {
      .coupon-hero {
        height: 50vh;
        min-height: 300px;
      }
      
      .hero-content h1 {
        font-size: 1.8em;
      }
      
      .coupon-meta {
        font-size: 0.85em;
      }
      
      .coupon-content-container {
        padding: 30px 15px;
      }

      .source-coupon {
      margin-bottom: -90px;
    }

      .terms-container {
        margin-top: -200px;
      }
    }

  `;
  document.head.appendChild(style);
}

function closeCouponDetail() {
  const couponPage = document.querySelector('.coupon-detail-page');
  if (couponPage) {
    // Remove all style elements that were added for the detail page
    const styleElements = document.querySelectorAll('style');
    styleElements.forEach(style => {
      if (style.textContent.includes('.coupon-detail-page') || 
          style.textContent.includes('.coupon-detail-header')) {
        style.remove();
      }
    });
    
    couponPage.remove();
  }
}

function shareCoupon(title, code) {
  if (navigator.share) {
    navigator.share({
      title: title,
      text: `Check out this coupon: ${code}`,
      url: window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    // Fallback for browsers that don't support Web Share API
    const shareUrl = `https://twitter.com/intent/tweet?text=${encodeURIComponent(title + " - Use code: " + code)}&url=${encodeURIComponent(window.location.href)}`;
    window.open(shareUrl, '_blank');
  }
}

function copyCouponCode(code) {
  navigator.clipboard.writeText(code).then(() => {
    // Show copied notification
    const notification = document.createElement('div');
    notification.className = 'copy-notification';
    notification.textContent = 'Copied to clipboard!';
    document.body.appendChild(notification);
    
    setTimeout(() => {
      notification.classList.add('show');
    }, 10);
    
    setTimeout(() => {
      notification.classList.remove('show');
      setTimeout(() => notification.remove(), 300);
    }, 2000);
  }).catch(err => {
    console.error('Failed to copy: ', err);
    // Fallback for browsers that don't support clipboard API
    const textarea = document.createElement('textarea');
    textarea.value = code;
    document.body.appendChild(textarea);
    textarea.select();
    document.execCommand('copy');
    document.body.removeChild(textarea);
    
    // Show fallback notification
    alert('Coupon code copied to clipboard!');
  });
}

function hideCouponOverlay() {
  const overlay = document.getElementById('couponOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => overlay.remove(), 300);
  }
}

function filterCoupons(category) {
  const couponGrid = document.getElementById('couponGrid');
  let filteredData = couponData;

  if (category !== 'All') {
    filteredData = filteredData.filter(coupon => coupon.category === category);
  }

  couponGrid.innerHTML = renderCouponCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function performCouponSearch(data, query, gridElement, filterButtons, renderFunction) {
  if (!query) {
    // If no query, show all coupons
    gridElement.innerHTML = renderFunction(data);
    return;
  }

  const lowerCaseQuery = query.toLowerCase();
  const filteredData = data.filter(coupon => {
    return (
      coupon.details.title.toLowerCase().includes(lowerCaseQuery) ||
      coupon.details.description.toLowerCase().includes(lowerCaseQuery) ||
      coupon.details.store.toLowerCase().includes(lowerCaseQuery) ||
      coupon.category.toLowerCase().includes(lowerCaseQuery) ||
      coupon.details.code.toLowerCase().includes(lowerCaseQuery) ||
      (coupon.details.tags && coupon.details.tags.some(tag => tag.toLowerCase().includes(lowerCaseQuery)))
    );
  });

  gridElement.innerHTML = filteredData.length > 0 
    ? renderFunction(filteredData)
    : '<div class="no-results">No coupons found matching your search.</div>';
}

function initializeCouponVoiceSearch(inputElement, callback) {
  const voiceButton = document.getElementById('couponVoiceSearchButton');
  if (!voiceButton) return;

  let recognition;
  try {
    recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.continuous = false;
    recognition.interimResults = false;

    voiceButton.addEventListener('click', () => {
      if (voiceButton.classList.contains('active')) {
        recognition.stop();
        voiceButton.classList.remove('active');
        return;
      }

      voiceButton.classList.add('active');
      recognition.start();

      recognition.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        inputElement.value = transcript;
        if (callback) callback(transcript);
        voiceButton.classList.remove('active');
      };

      recognition.onerror = (event) => {
        console.error('Voice recognition error', event.error);
        voiceButton.classList.remove('active');
      };

      recognition.onend = () => {
        voiceButton.classList.remove('active');
      };
    });
  } catch (e) {
    console.error('Voice recognition not supported', e);
    voiceButton.style.display = 'none';
  }
}

// Utility function for debouncing
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

function scrollToTop() {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  });
}

document.addEventListener('DOMContentLoaded', function() {
  // Get the coupon button element
  const couponButton = document.getElementById('couponButton');
  
  // Add click event listener
  if (couponButton) {
    couponButton.addEventListener('click', function(e) {
      e.preventDefault();
      showCouponOverlay();
    });
  }
  
  // Add copy notification styles
  const copyNotificationStyle = document.createElement('style');
  copyNotificationStyle.textContent = `
    .copy-notification {
      position: fixed;
      bottom: 100px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0, 0, 0, 0.8);
      color: white;
      padding: 12px 24px;
      border-radius: 25px;
      font-size: 0.9em;
      opacity: 0;
      transition: opacity 0.3s ease;
      z-index: 3000;
      pointer-events: none;
    }
    
    .copy-notification.show {
      opacity: 1;
    }
  `;
  document.head.appendChild(copyNotificationStyle);
});

const locatorsData = [
  // Delhi
  {
    category: "Grocery",
    metropolis: "Delhi",
    details: {
      name: "Dmart",
      cardImages: [
        "https://www.projectsmonitor.com/wp-content/uploads/2021/09/@DMartOfficiall.jpg",
        "https://www.goodreturns.in/img/1200x60x675/2020/04/dmart-1525851463-1586576977.jpg"
      ],
      storeImages: [
        "https://www.projectsmonitor.com/wp-content/uploads/2021/09/@DMartOfficiall.jpg",
        "https://www.goodreturns.in/img/1200x60x675/2020/04/dmart-1525851463-1586576977.jpg",
        "https://www.dmartindia.com/static/media/fruits-vegetables.2c1e8a1e.jpg"
      ],
      description: "India's leading supermarket chain offering groceries, household essentials, and daily necessities at highly competitive prices. Known for efficient operations and value pricing.",
      address: "12, Karol Bagh, Delhi - 110005",
      timings: "Mon-Sun: 9 AM - 10 PM",
      entryFee: "Free Entry",
      mapLink: "https://maps.example.com/dmart",
      website: "https://www.dmart.in",
      social: {
        facebook: "https://facebook.com/dmart",
        twitter: "https://twitter.com/dmart",
        instagram: "https://instagram.com/dmart"
      },
      facilities: [
        { name: "Wide Selection", description: "Thousands of products across categories" },
        { name: "Great Prices", description: "Everyday low prices on all items" },
        { name: "Clean Stores", description: "Well-organized and hygienic shopping environment" }
      ],
      services: [
        {
          name: "Fortune Sunflower Oil",
          type: "product",
          image: "https://kiasumart.com/wp-content/uploads/2023/11/Fortune-1l-2-1.jpg",
          mrp: "₹220",
          price: "₹199",
          quantity: "1L",
          stock: "In Stock",
          description: "100% pure sunflower oil for healthy cooking"
        },
        {
          name: "Tata Salt",
          type: "product",
          image: "https://5.imimg.com/data5/SELLER/Default/2023/7/329417195/WP/SH/PZ/27715606/tata-namak-1000x1000.jpg",
          mrp: "₹25",
          price: "₹18",
          quantity: "1kg",
          stock: "In Stock",
          description: "Iodized salt for daily cooking"
        }
      ],
      promos: [
        { name: "Weekly Offers", description: "Special discounts every week", code: "DMART10" }
      ],
      support: {
        email: "orders@dmart.com",
        phone: "+91-1234567890",
        whatsapp: "+91-7869809022",
        primaryContact: "whatsapp" // or "email"
      },
      rating: "4.3",
      reviews: "5000+",
      established: "2002",
      tags: ["Discounts", "Essentials", "Bulk Purchase"]
    }
  },
  {
    category: "Food",
    metropolis: "Delhi",
    details: {
      name: "Burger King",
      cardImages: [
        "https://staticg.sportskeeda.com/editor/2022/12/c6e19-16716485336805-1920.jpg",
        "http://www.ourcities.in/wp-content/uploads/2020/05/Reliance-Digital-Brookefields-Plaza-Coimbatore.jpg"
      ],
      storeImages: [
        "https://www.burgerking.in/static/media/store-interior.1a9f8a4e.jpg",
        "https://www.burgerking.in/static/media/burger-closeup.2c1e8a1e.jpg",
        "https://www.burgerking.in/static/media/dining-area.3d1e8a1e.jpg"
      ],
      description: "Global fast food chain famous for flame-grilled burgers, fries and shakes. Home of the iconic Whopper sandwich with vegetarian options available.",
      address: "25, Connaught Place, Delhi - 110001",
      timings: "10 AM - 11 PM (All Days)",
      entryFee: "No Cover Charge",
      mapLink: "https://maps.example.com/burgerking",
      website: "https://www.burgerking.in",
      social: {
        facebook: "https://facebook.com/burgerking",
        twitter: "https://twitter.com/burgerking",
        instagram: "https://instagram.com/burgerking"
      },
      facilities: [
        { name: "Vegetarian Options", description: "Wide range of vegetarian meals" },
        { name: "Family Friendly", description: "Kid's meals and play areas" },
        { name: "Late Night", description: "Open till midnight" }
      ],
      services: [
        {
          name: "Whopper Meal",
          type: "menu",
          image: "https://3.bp.blogspot.com/-yXFrzno9rWw/V1Bh6DB-u5I/AAAAAAAAvgA/VEBQ0Irou8gfshSPW3KgO2v6djrE4zREgCLcB/s1600/burger-king-2-for-10-dollars-whopper-meal.jpg",
          price: "₹199",
          description: "Signature Whopper burger with fries and drink",
          variants: [
            { name: "Regular", price: "₹199" },
            { name: "Large", price: "₹249" }
          ]
        },
        {
          name: "Chicken Fries",
          type: "menu",
          image: "http://static3.businessinsider.com/image/53e8db59ecad04a96e35c05e/burger-king-is-bringing-back-chicken-fries.jpg",
          price: "₹129",
          description: "Crispy chicken strips with dipping sauce"
        }
      ],
      promos: [
        { name: "Combo Deal", description: "15% off combo meals", code: "BURGER15" }
      ],
      support: {
        email: "orders@burgerking.com",
        phone: "+91-9876543210",
        whatsapp: "+91-9876543210",
        primaryContact: "whatsapp"
      },
      rating: "4.1",
      reviews: "2500+",
      established: "2014",
      tags: ["Fast Food", "American", "Takeaway"]
    }
  },
  // Bangalore
  {
    category: "Electronics",
    metropolis: "Bangalore",
    details: {
      name: "Reliance Digital Koramangala",
      cardImages: [
        "http://www.ourcities.in/wp-content/uploads/2020/05/Reliance-Digital-Brookefields-Plaza-Coimbatore.jpg",
        "https://www.reliancedigital.in/static/media/electronics-display.2c1e8a1e.jpg"
      ],
      storeImages: [
        "https://www.reliancedigital.in/static/media/store-interior.1a9f8a4e.jpg",
        "https://www.reliancedigital.in/static/media/tv-section.3d1e8a1e.jpg",
        "https://www.reliancedigital.in/static/media/appliance-section.4d1e8a1e.jpg"
      ],
      description: "Premium electronics store in Koramangala offering latest smartphones, laptops, TVs and home appliances with expert guidance.",
      address: "5th Block, Koramangala, Bangalore - 560034",
      timings: "10 AM - 9 PM (All Days)",
      entryFee: "Free Entry",
      mapLink: "https://maps.example.com/reliancedigital-koramangala",
      website: "https://www.reliancedigital.in",
      social: {
        facebook: "https://facebook.com/reliancedigital",
        twitter: "https://twitter.com/reliancedigital",
        instagram: "https://instagram.com/reliancedigital"
      },
      facilities: [
        { name: "Demo Units", description: "Try before you buy" },
        { name: "EMI Options", description: "Flexible payment plans" },
        { name: "Gift Cards", description: "Perfect present solution" }
      ],
      services: [
        {
          name: "Samsung Galaxy S21",
          type: "product",
          image: "https://storage.comprasmartphone.com/smartphones/samsung-galaxy-s25-ultra.png",
          mrp: "₹54,999",
          price: "₹49,999",
          description: "Flagship smartphone with 120Hz AMOLED display",
          variants: [
            { name: "128GB", price: "₹49,999" },
            { name: "256GB", price: "₹54,999" }
          ]
        }
      ],
      promos: [
        { name: "Festive Season", description: "No-cost EMI available", code: "NOCOSTEMI" }
      ],
      support: {
        email: "orders@reliancedigital.com",
        phone: "+91-9876543213",
        whatsapp: "+91-9876543213",
        primaryContact: "email"
      },
      rating: "4.5",
      reviews: "2000+",
      established: "2018",
      tags: ["Electronics", "Gadgets", "Appliances"]
    }
  },
  // Additional stores can be added in the same format
];

// State management for overlays
const overlayState = {
  currentOverlay: null,
  navListeners: new Map()
};

// Global cart state
let cartState = {
  items: [],
  store: null,
  total: 0,
  discountedTotal: 0,  // Add this to track discounted total
  appliedCoupon: null, // Add this to track applied coupon
  directOrderItems: null
};

// Function to initialize all overlay event listeners
function initializeOverlaySystem() {
  // Clear existing listeners
  overlayState.navListeners.forEach((listener, element) => {
    element.removeEventListener('click', listener);
  });
  overlayState.navListeners.clear();

  // Add listeners for each nav item
  const navItems = {
    'brand': {
      handler: showLocatorsOverlay,
      type: 'locators'
    },
    'services': {
      handler: showProvidersOverlay,
      type: 'providers'
    },
    'places': {
      handler: showPlacesOverlay,
      type: 'places'
    }
  };

  Object.entries(navItems).forEach(([page, { handler, type }]) => {
    const navItem = document.querySelector(`.nav-item[data-page="${page}"]`);
    if (navItem) {
      const listener = async (e) => {
        e.preventDefault();
        
        // If there's a current overlay and it's different from the one being opened
        if (overlayState.currentOverlay && overlayState.currentOverlay !== type) {
          // Hide current overlay first
          hideCurrentOverlay();
          
          // Wait for the hide animation to complete
          await new Promise(resolve => setTimeout(resolve, 300));
        }
        
        // Update state and show new overlay
        overlayState.currentOverlay = type;
        handler();
      };
      
      navItem.addEventListener('click', listener);
      overlayState.navListeners.set(navItem, listener);
    }
  });
}

function showLocatorsOverlay() {
  let overlay = document.getElementById('locatorsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'locatorsOverlay';
    overlay.className = 'locators-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(locatorsData.map(locator => locator.category))];

  overlay.innerHTML = `
    <div class="locators-content">
      <div class="locators-header">
        <div class="header-title-container">
          <h2>Stores</h2>
          <div class="header-subtitle">Find local businesses and shopping</div>
        </div>
        <div class="header-buttons-container">
          <button class="sort-button" onclick="toggleLocatorSortOptions()">
            <i class="fas fa-sliders-h"></i>
          </button>
          <button class="close-locators" onclick="hideLocatorsOverlay()">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      
      <div class="sort-options-container" id="locatorSortOptionsContainer">
        <div class="sort-option" onclick="sortLocators('rating')">
          <i class="fas fa-star"></i> Highest Rated
        </div>
        <div class="sort-option" onclick="sortLocators('name')">
          <i class="fas fa-sort-alpha-down"></i> Alphabetical
        </div>
        <div class="sort-option" onclick="sortLocators('distance')">
          <i class="fas fa-map-marker-alt"></i> Nearest
        </div>
      </div>

      <div class="locators-filter-container">
        <div class="locators-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterLocators('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="locators-grid" id="locatorsGrid">
        ${renderLocatorCards(locatorsData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="locatorsExploreInput" placeholder="Search for stores, restaurants..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('locatorsExploreInput');
  const locatorsGrid = document.getElementById('locatorsGrid');
  const filterButtons = document.querySelectorAll('.filter-button');
     
  const debouncedSearch = debounce((query) => {
    performSearch(locatorsData, query, locatorsGrid, filterButtons, renderLocatorCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(locatorsData, exploreInput.value, locatorsGrid, filterButtons, renderLocatorCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(locatorsData, query, locatorsGrid, filterButtons, renderLocatorCards);
  });

  function performSearch(locators, query, gridElement, filterButtons, renderFunction) {
  if (!query.trim()) {
    gridElement.innerHTML = renderFunction(locators);
    // Reset to 'All' filter when search is cleared
    filterButtons.forEach(btn => {
      btn.classList.remove('active');
      if (btn.dataset.category === 'All') btn.classList.add('active');
    });
    return;
  }

  // Normalize and preprocess the query
  const normalizedQuery = query.toLowerCase().trim()
    .replace(/[^\w\s₹]/g, '') // Remove special chars except ₹
    .replace(/\s+/g, ' ');    // Collapse multiple spaces

  // Enhanced extraction with synonyms and broader matching
  const categorySynonyms = {
    'grocery': 'Grocery',
    'supermarket': 'Grocery',
    'mart': 'Grocery',
    'food': 'Food',
    'restaurant': 'Food',
    'cafe': 'Food',
    'dine': 'Food',
    'eat': 'Food',
    'electronics': 'Electronics',
    'gadgets': 'Electronics',
    'appliances': 'Electronics',
    'devices': 'Electronics',
    'fashion': 'Fashion',
    'clothing': 'Fashion',
    'apparel': 'Fashion',
    'footwear': 'Fashion',
    'pharmacy': 'Pharmacy',
    'medical': 'Pharmacy',
    'medicine': 'Pharmacy',
    'drug': 'Pharmacy'
  };

  const citySynonyms = {
    'ncr': 'Delhi',
    'bpl': 'Bhopal',
    'idr': 'Indore',
    'new delhi': 'Delhi',
    'bombay': 'Mumbai',
    'bengaluru': 'Bangalore',
    'blr': 'Bangalore'
  };

  // Advanced query parsing with priority scoring
  let categoryMatch, cityMatch, priceMatch, productMatch, qualityMatch, ratingMatch;
  let hasNegation = false;

  // Check for negation terms
  hasNegation = /\b(no|not|without|except)\b/.test(normalizedQuery);

  // 1. Category detection with synonyms and fuzzy matching
  const categoryPattern = new RegExp(
    `\\b(${Object.keys(categorySynonyms).concat([
      'grocery', 'food', 'electronics', 
      'fashion', 'pharmacy', 'restaurant',
      'cafe', 'supermarket', 'store', 'shop'
    ]).join('|')})\\b`, 'i');
  
  categoryMatch = normalizedQuery.match(categoryPattern);
  if (categoryMatch) {
    const matchedTerm = categoryMatch[0].toLowerCase();
    categoryMatch[0] = categorySynonyms[matchedTerm] || matchedTerm;
    // Capitalize first letter
    categoryMatch[0] = categoryMatch[0].charAt(0).toUpperCase() + categoryMatch[0].slice(1).toLowerCase();
  }

  // 2. City detection with synonyms
  const cityPattern = new RegExp(
    `\\b(${Object.keys(citySynonyms).concat([
      'delhi', 'mumbai', 'bhopal', 'bangalore',
      'kolkata', 'chennai', 'hyderabad', 'pune', 'ahmedabad',
      'jaipur', 'lucknow', 'kanpur', 'nagpur', 'indore',
      'thane', 'bhubaneswar', 'visakhapatnam', 'vadodara', 'surat',
      'coimbatore', 'patna', 'ranchi', 'guwahati', 'chandigarh'
    ]).join('|')})\\b`, 'i'
  );
  
  cityMatch = normalizedQuery.match(cityPattern);
  if (cityMatch) {
    const matchedTerm = cityMatch[0].toLowerCase();
    cityMatch[0] = citySynonyms[matchedTerm] || matchedTerm;
    // Capitalize first letter
    cityMatch[0] = cityMatch[0].charAt(0).toUpperCase() + cityMatch[0].slice(1).toLowerCase();
  }

  // 3. Price detection with multiple formats
  priceMatch = normalizedQuery.match(/(?:rs\.?|₹|inr)\s*(\d+)/i) || 
               normalizedQuery.match(/(\d+)\s*(?:rs|rupees|inr)/i) ||
               normalizedQuery.match(/\b(\d+)\s*(?:price|cost|charge)/i) ||
               normalizedQuery.match(/\b(under|below|less than|up to|above|over|more than)\s*₹?\s*(\d+)/i);

  // 4. Product detection with expanded terms
  const productTerms = [
    'oil', 'salt', 'rice', 'atta', 'sugar', 
    'burger', 'pizza', 'pasta', 'sandwich', 'meal',
    'phone', 'tv', 'laptop', 'refrigerator', 'washing',
    'shirt', 'pant', 'jeans', 'dress', 'shoes',
    'medicine', 'tablet', 'syrup', 'ointment', 'cream'
  ];
  productMatch = normalizedQuery.match(new RegExp(`\\b(${productTerms.join('|')})\\b`, 'i'));

  // 5. Quality detection with expanded terms
  qualityMatch = normalizedQuery.match(/(cheap|affordable|budget|low cost|low price|economical|expensive|premium|luxury|high end|high quality)/i);

  // 6. Rating detection
  ratingMatch = normalizedQuery.match(/(\d+(?:\.\d+)?)\s*(?:star|rating)/i) ||
                normalizedQuery.match(/(high|good|excellent|poor|bad)\s*(?:rated|rating|reviews?)/i);

  // 7. Brand detection
  const brandMatch = normalizedQuery.match(/(dmart|burger king|reliance digital|big bazaar|dominos|mcdonalds|pantaloons|apollo)/i);

  // Advanced filtering with scoring system
  const filteredLocators = locators.map(locator => {
    let score = 0;
    let matches = [];
    let mismatches = [];

    // Category matching (high importance)
    if (categoryMatch) {
      const categoryLower = locator.category.toLowerCase();
      if (categoryLower.includes(categoryMatch[0].toLowerCase())) {
        score += 30;
        matches.push(`Category: ${locator.category}`);
      } else {
        mismatches.push(`Category: ${categoryMatch[0]}`);
        return null; // Exclude if category doesn't match
      }
    }

    // City matching (high importance)
    if (cityMatch) {
      const cityLower = locator.metropolis.toLowerCase();
      if (cityLower.includes(cityMatch[0].toLowerCase())) {
        score += 30;
        matches.push(`City: ${locator.metropolis}`);
      } else {
        mismatches.push(`City: ${cityMatch[0]}`);
        return null; // Exclude if city doesn't match
      }
    }

    // Price matching (medium importance)
    if (priceMatch) {
      let price;
      if (priceMatch[2]) { // For "under ₹1000" type patterns
        price = parseInt(priceMatch[2]);
        const comparison = priceMatch[1].toLowerCase();
        
        const priceProducts = locator.details.services.filter(service => {
          const servicePrice = parseInt(service.price.replace(/[^\d]/g, ''));
          if (comparison.includes('under') || comparison.includes('below') || 
              comparison.includes('less than') || comparison.includes('up to')) {
            return servicePrice <= price;
          } else if (comparison.includes('above') || comparison.includes('over') || 
                    comparison.includes('more than')) {
            return servicePrice >= price;
          }
          return servicePrice <= price;
        });
        
        if (priceProducts.length > 0) {
          score += 20;
          matches.push(`Price: ${comparison} ₹${price}`);
        } else {
          mismatches.push(`Price: ${comparison} ₹${price}`);
        }
      } else {
        price = parseInt(priceMatch[1]);
        const priceProducts = locator.details.services.filter(service => {
          const servicePrice = parseInt(service.price.replace(/[^\d]/g, ''));
          return servicePrice <= price;
        });
        
        if (priceProducts.length > 0) {
          score += 20;
          matches.push(`Price: ≤₹${price}`);
        } else {
          mismatches.push(`Price: ≤₹${price}`);
        }
      }
    }

    // Product matching (medium importance)
    if (productMatch) {
      const matchedProducts = locator.details.services.filter(service => {
        return service.name.toLowerCase().includes(productMatch[0]) || 
               service.description.toLowerCase().includes(productMatch[0]);
      });
      
      if (matchedProducts.length > 0) {
        score += 20;
        matches.push(`Product: ${productMatch[0]}`);
      } else {
        mismatches.push(`Product: ${productMatch[0]}`);
      }
    }

    // Quality matching (low importance)
    if (qualityMatch) {
      const quality = qualityMatch[0].toLowerCase();
      const isBudget = ['cheap', 'affordable', 'budget', 'low cost', 'low price', 'economical'].includes(quality);
      const isPremium = ['expensive', 'premium', 'luxury', 'high end', 'high quality'].includes(quality);
      
      if (isBudget) {
        if (locator.details.tags.some(tag => tag.toLowerCase().includes('budget'))) {
          score += 10;
          matches.push(`Budget-friendly`);
        }
      }
      
      if (isPremium) {
        if (locator.details.tags.some(tag => tag.toLowerCase().includes('premium'))) {
          score += 10;
          matches.push(`Premium store`);
        }
      }
    }

    // Rating matching (medium importance)
    if (ratingMatch) {
      if (ratingMatch[1] && !isNaN(ratingMatch[1])) {
        const minRating = parseFloat(ratingMatch[1]);
        const locatorRating = parseFloat(locator.details.rating);
        if (locatorRating >= minRating) {
          score += 15;
          matches.push(`Rating ≥ ${minRating}`);
        }
      } else if (/high|good|excellent/i.test(ratingMatch[0])) {
        const locatorRating = parseFloat(locator.details.rating);
        if (locatorRating >= 4.5) {
          score += 15;
          matches.push(`Highly rated`);
        }
      }
    }

    // Brand matching (high importance)
    if (brandMatch) {
      const brand = brandMatch[0].toLowerCase();
      if (locator.details.name.toLowerCase().includes(brand)) {
        score += 40;
        matches.push(`Brand: ${locator.details.name}`);
      } else {
        mismatches.push(`Brand: ${brand}`);
        return null; // Exclude if brand doesn't match
      }
    }

    // General text search (fallback)
    if (!categoryMatch && !cityMatch && !priceMatch && !productMatch && 
        !qualityMatch && !ratingMatch && !brandMatch) {
      const searchFields = [
        locator.category,
        locator.metropolis,
        locator.details.name,
        locator.details.description,
        ...locator.details.tags,
        ...locator.details.services.map(s => s.name),
        ...locator.details.services.map(s => s.description),
        ...locator.details.facilities.map(f => f.name),
        ...locator.details.facilities.map(f => f.description)
      ].join(' ').toLowerCase();
      
      if (searchFields.includes(normalizedQuery)) {
        score += 50; // High score for direct match
        matches.push(`General match`);
      } else {
        // Try partial matching
        const queryWords = normalizedQuery.split(/\s+/);
        const matchedWords = queryWords.filter(word => 
          word.length > 3 && searchFields.includes(word)
        );
        if (matchedWords.length > 0) {
          score += matchedWords.length * 5;
          matches.push(`Partial match: ${matchedWords.join(', ')}`);
        } else {
          return null; // No match at all
        }
      }
    }

    // Handle negation queries
    if (hasNegation) {
      const negatedTerms = normalizedQuery.match(/\b(no|not|without|except)\s+(\w+)/i);
      if (negatedTerms) {
        const negatedTerm = negatedTerms[2];
        const searchFields = [
          locator.category,
          locator.metropolis,
          locator.details.name,
          ...locator.details.tags
        ].join(' ').toLowerCase();
        
        if (searchFields.includes(negatedTerm)) {
          return null; // Exclude locators matching negated term
        }
      }
    }

    return {
      locator,
      score,
      matches,
      mismatches
    };
  })
  .filter(Boolean) // Remove null entries
  .sort((a, b) => b.score - a.score) // Sort by score descending
  .map(item => item.locator); // Extract just the locators

  // Enhanced empty state handling
  if (filteredLocators.length === 0) {
    const suggestions = generateLocatorSearchSuggestions(query, locators);
    gridElement.innerHTML = `
      <div class="no-results">
        <div class="no-results-icon">🔍</div>
        <h3>No exact matches found</h3>
        <p>We couldn't find stores matching "${query}"</p>
        ${suggestions.length > 0 ? `
          <div class="suggestions">
            <p>Try one of these instead:</p>
            <ul>
              ${suggestions.map(suggestion => `
                <li onclick="document.getElementById('locatorsExploreInput').value = '${suggestion}'; performSearch(locatorsData, '${suggestion}', document.getElementById('locatorsGrid'), document.querySelectorAll('.filter-button'), renderLocatorCards)">
                  ${suggestion}
                </li>
              `).join('')}
            </ul>
          </div>
        ` : ''}
      </div>
    `;
  } else {
    gridElement.innerHTML = renderFunction(filteredLocators);
  }

  // Update active filter button based on search
  updateLocatorActiveFilterButton(filterButtons, categoryMatch, cityMatch, productMatch);
}

function updateLocatorActiveFilterButton(filterButtons, categoryMatch, cityMatch, productMatch) {
  // Reset all buttons first
  filterButtons.forEach(btn => btn.classList.remove('active'));
  
  // Determine which filter to activate
  if (categoryMatch) {
    // Find matching filter button
    const matchingButton = Array.from(filterButtons).find(btn => 
      btn.dataset.category.toLowerCase() === categoryMatch[0].toLowerCase()
    );
    
    if (matchingButton) {
      matchingButton.classList.add('active');
    } else {
      // If no exact match, try to find a partial match
      const partialMatch = Array.from(filterButtons).find(btn => 
        btn.dataset.category.toLowerCase().includes(categoryMatch[0].toLowerCase()) ||
        categoryMatch[0].toLowerCase().includes(btn.dataset.category.toLowerCase())
      );
      
      if (partialMatch) {
        partialMatch.classList.add('active');
      } else {
        // Default to 'All' if no match found
        filterButtons[0].classList.add('active');
      }
    }
  } else if (productMatch) {
    // Try to infer category from product
    const productToCategory = {
      'oil': 'Grocery',
      'salt': 'Grocery',
      'rice': 'Grocery',
      'atta': 'Grocery',
      'sugar': 'Grocery',
      'burger': 'Food',
      'pizza': 'Food',
      'pasta': 'Food',
      'sandwich': 'Food',
      'meal': 'Food',
      'phone': 'Electronics',
      'tv': 'Electronics',
      'laptop': 'Electronics',
      'refrigerator': 'Electronics',
      'washing': 'Electronics',
      'shirt': 'Fashion',
      'pant': 'Fashion',
      'jeans': 'Fashion',
      'dress': 'Fashion',
      'shoes': 'Fashion',
      'medicine': 'Pharmacy',
      'tablet': 'Pharmacy',
      'syrup': 'Pharmacy',
      'ointment': 'Pharmacy',
      'cream': 'Pharmacy'
    };
    
    const inferredCategory = productToCategory[productMatch[0].toLowerCase()];
    if (inferredCategory) {
      const matchingButton = Array.from(filterButtons).find(btn => 
        btn.dataset.category === inferredCategory
      );
      if (matchingButton) matchingButton.classList.add('active');
    } else {
      filterButtons[0].classList.add('active');
    }
  } else {
    // Default to 'All' if no specific category or product was mentioned
    filterButtons[0].classList.add('active');
  }
}

// Helper function to generate search suggestions for locators
function generateLocatorSearchSuggestions(query, locators) {
  const suggestions = new Set();
  
  // 1. Suggest similar categories
  const categories = [...new Set(locators.map(p => p.category))];
  categories.forEach(cat => {
    if (cat.toLowerCase().includes(query.toLowerCase().substring(0, 3))) {
      suggestions.add(cat);
    }
  });

  // 2. Suggest popular products
  if (query.length > 3) {
    locators.forEach(locator => {
      locator.details.services.forEach(service => {
        if (service.name.toLowerCase().includes(query.toLowerCase())) {
          suggestions.add(service.name);
        }
      });
    });
  }

  // 3. Add price-based suggestions if no price was mentioned
  if (!/(rs|₹|inr|price|cost)/i.test(query)) {
    suggestions.add(`${query} under ₹100`);
    suggestions.add(`${query} under ₹500`);
    suggestions.add(`affordable ${query}`);
  }

  // 4. Add brand suggestions
  if (/mart|store|shop|restaurant|cafe/i.test(query)) {
    suggestions.add(`Dmart ${query}`);
    suggestions.add(`Reliance Digital ${query}`);
    suggestions.add(`Burger King ${query}`);
  }

  // 5. Add quality suggestions
  if (/cheap|affordable|budget/i.test(query)) {
    suggestions.add(`affordable ${query.replace(/cheap|affordable|budget/gi, '').trim()}`);
  } else if (/premium|quality|best/i.test(query)) {
    suggestions.add(`premium ${query.replace(/premium|quality|best/gi, '').trim()}`);
  }

  return Array.from(suggestions).slice(0, 5); // Return top 5 suggestions
}

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
.locators-overlay {
  font-family: poppins;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.locators-overlay::-webkit-scrollbar {
  display: none;
}

.locators-overlay.active {
  opacity: 1;
  visibility: visible;
}

.locators-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.locators-header {
  padding: 12px 20px;
  background: rgba(26, 26, 31, 0.98);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.header-title-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}

.locators-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.header-subtitle {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.8em;
  margin-top: 2px;
  font-weight: 400;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.close-locators {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 1em;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-locators:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.close-locators:active {
  transform: scale(0.95);
}

.sort-button {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  font-size: 0.9em;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.sort-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
}

.sort-button:active {
  transform: translateY(0);
}

.button-text {
  font-weight: 500;
}

.sort-options-container {
  position: fixed;
  top: 72px;
  right: 20px;
  background: #2a2a35;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  z-index: 40;
  padding: 8px 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.sort-options-container.active {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.sort-option {
  padding: 12px 20px;
  color: white;
  font-size: 0.9em;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.sort-option:hover {
  background: rgba(255, 215, 0, 0.1);
  color: var(--primary-color);
}

.sort-option i {
  width: 20px;
  text-align: center;
}

.locators-filter-container {
  position: fixed;
  top: 64px;
  left: 0;
  right: 0;
  width: 100%;
  z-index: 10;
  background: #1a1a1f;
  padding: 12px 0;
  box-sizing: border-box;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
  border-bottom: 1px solid rgba(255, 215, 0, 0.1);
}

.locators-filter-container::-webkit-scrollbar {
  display: none;
}

.locators-filter-buttons {
  display: inline-flex;
  padding: 0 16px;
  gap: 8px;
  white-space: nowrap;
}

.filter-button {
  font-family: poppins;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: rgba(255, 255, 255, 0.9);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 20px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  margin: 0;
}

.filter-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: scale(1.03);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: #000;
  font-weight: 600;
  border-color: var(--primary-color);
  box-shadow: 0 4px 10px rgba(255, 215, 0, 0.3);
}

/* Add some space at the end for better scrolling */
.locators-filter-buttons::after {
  content: '';
  display: block;
  min-width: 16px;
  height: 1px;
}

@media (min-width: 1024px) {
   .locators-filter-container {
   top: 72px;
    }
}

.locators-grid {
  margin-top: 90px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  padding-bottom: 140px;
  width: 100%;
}

.locator-card {
  background: #ffffff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  position: relative;
  border: none;
  display: flex;
  flex-direction: column;
  height: 100%;
}

.locator-card:hover {
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.locator-card:active {
  transform: translateY(-1px);
  transition: transform 0.1s ease;
}

.image-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 12px 12px 0 0;
  margin-bottom: 0;
  box-shadow: none;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease, transform 0.5s ease;
  transform: scale(1.03);
  will-change: opacity, transform;
}

.carousel-item.active {
  opacity: 1;
  transform: scale(1);
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.locator-card:hover .carousel-item.active img {
  transform: scale(1.05);
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  -webkit-tap-highlight-color: transparent;
}

.image-carousel:hover .carousel-control {
  opacity: 0.9;
}

.carousel-control:hover {
  background: rgba(0, 0, 0, 0.85);
  transform: translateY(-50%) scale(1.1);
}

.carousel-control:active {
  transform: translateY(-50%) scale(0.95);
}

.carousel-control.prev {
  left: 12px;
}

.carousel-control.next {
  right: 12px;
}

.carousel-pagination {
  position: absolute;
  bottom: 12px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
}

.locator-card-content {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.locator-card-header {
  display: flex;
  flex-direction: column;
  padding: 0;
  background: transparent;
  border-bottom: none;
}

.locator-name {
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin: 0 0 4px 0;
  line-height: 1.3;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.locator-category {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 14px;
  margin-bottom: 8px;
}

.locator-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
}

.locator-rating .star {
  color: #ffc107;
  font-weight: 600;
}

.locator-rating .count {
  color: #666;
  font-size: 14px;
}

.locator-info-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
  margin: 4px 0;
}

.info-item {
  flex: 1 1 100%;
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px;
  border-radius: 10px;
  background: rgba(255, 215, 0, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.1);
  transition: all 0.3s ease;
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

.info-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: var(--primary-color);
}

.info-icon i {
  font-size: 14px;
}

.info-content {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.info-label {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #ffc107;
  margin-bottom: 6px;
}

.info-value {
  font-size: 13px;
  font-weight: 500;
  color: #000;
  line-height: 1.4;
  display: block;
  margin-top: 0;
}

.info-value + .info-value {
  margin-top: 6px;
  position: relative;
  padding-top: 6px;
}

.info-value + .info-value::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 215, 0, 0.1);
}

.info-item:hover {
  background: rgba(255, 215, 0, 0.08);
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.locator-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
  margin: 12px 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.tag-container {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 10px 0;
}

.tag {
  background: #f5f5f5;
  color: #666;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
  transition: background-color 0.2s ease;
}

.tag:hover {
  background: #eeeeee;
}

.locator-card-actions {
  display: flex;
  border-top: 1px solid #eee;
  padding: 0;
  background: transparent;
  margin-top: auto;
}

.action-button {
  flex: 1;
  background: transparent;
  color: #333;
  font-weight: 500;
  padding: 14px 0;
  border: none;
  border-radius: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  position: relative;
  transition: background-color 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.action-button:hover {
  background: #f5f5f5;
}

.action-button:active {
  background: #eeeeee;
}

.action-button i {
  color: #333;
  font-size: 16px;
}

.action-button:first-child {
  border-right: 1px solid #eee;
}

@media (max-width: 991px) {
  .locators-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  }
}

@media (max-width: 768px) {
  .locators-grid {
    grid-template-columns: 2fr;
    padding: 0 12px;
    margin-top: 90px;
    gap: 16px;
  }
  
  .locator-card {
    margin-bottom: 0;
    border-radius: 12px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .image-carousel {
    height: 180px;
    border-radius: 12px 12px 0 0;
  }
  
  .locator-card-content {
    padding: 14px;
  }
  
  .locator-name {
    font-size: 16px;
  }
  
  .locator-description {
    font-size: 13px;
    margin: 10px 0;
  }
  
  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }
  
  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }
  
  .carousel-control {
    width: 32px;
    height: 32px;
    opacity: 0.7;
  }
  
  .image-carousel .carousel-control {
    opacity: 0.7;
  }
  
  .pagination-dot {
    width: 6px;
    height: 6px;
  }
  
  .pagination-dot.active {
    width: 16px;
  }
}

@media (max-width: 480px) {
  .locator-card {
    margin-bottom: 0px;
  }
  
  .image-carousel {
    height: 160px;
  }
  
  .locator-card-content {
    padding: 12px;
  }
  
  .locator-description {
    -webkit-line-clamp: 2;
    margin: 8px 0;
  }
    
  .tag-container {
    margin: 8px 0;
  }
  
  .tag {
    padding: 2px 8px;
    font-size: 10px;
  }
  
  .action-button {
    padding: 10px 0;
    font-size: 12px;
  }
  
  .action-button i {
    font-size: 14px;
  }
  
  .carousel-control {
    width: 28px;
    height: 28px;
  }
  
  .locators-filter-buttons {
    margin-top: 10px;
  }
  
  .locators-filter-buttons {
    padding-bottom: 2px;
  }
  
  .filter-button {
    padding: 9px 12px;
    font-size: 14px;
  }
}

@media (max-width: 768px) and (orientation: portrait) {
  .locators-grid {
    margin-top: 130px;
  }
  
  .locator-card {
    border-radius: 10px;
  }
}

.locator-card {
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .locator-card,
  .locator-card:hover,
  .carousel-item,
  .carousel-item.active,
  .carousel-item img,
  .carousel-control,
  .carousel-control:hover,
  .pagination-dot {
    transition: none;
    transform: none;
    animation: none;
  }
}

.locator-card:focus {
  outline: 2px solid var(--primary-color);
  outline-offset: 3px;
}

@media (prefers-color-scheme: dark) {
  .locator-card {
    background: #fff;
  }
  
  .locator-name {
    color: #000;
  }
  
  .locator-category,
  .locator-description,
  .locator-rating .count {
    color: #000;
  }
  
  .tag {
    background: #000;
    color: #ddd;
  }
  
  .tag:hover {
    background: #444;
  }
  
  .locator-card-actions {
    border-top: 1px solid #333;
  }
  
  .action-button {
    color: #000;
  }
  
  .action-button i {
    color: #000;
  }
  
  .action-button:first-child {
    border-right: 1px solid #333;
  }
  
  .action-button:hover {
    background: #fff;
  }
}

.chat-container {
  position: fixed;
  bottom: 0px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

.input-container {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#locatorsExploreInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 12px 50px 12px 20px;
  color: #fff;
  font-size: 0.95em;
  width: 100%;
  transition: border-color 0.3s ease;
}

#locatorsExploreInput:focus {
  border-color: rgba(255, 255, 255, 0.5);
}

.voice-search {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-search:hover {
  opacity: 0.8;
}

.voice-search.active {
  color: #ffd700;
  animation: pulse 1.5s infinite;
}

.voice-search i { 
  font-size: 1.5em; 
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 0px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #locatorsExploreInput {
    height: 70px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 1.7em;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #locatorsExploreInput {
    bottom: 0px;
    width: 60%;
    max-width: 1200px;
  }
  
  .locators-grid {
    padding-bottom: 180px;
  }
}

@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .locators-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .locator-card {
    width: 100%;
    margin: 0;
  }

  .locators-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .locators-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #locatorsExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}

.full-screen-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.95);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.full-screen-image.active {
  opacity: 1;
  visibility: visible;
}

.full-screen-image img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 10px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  transform: scale(0.95);
  opacity: 0;
  transition: all 0.4s ease;
}

.full-screen-image.active img {
  transform: scale(1);
  opacity: 1;
}

.close-full-screen {
  position: absolute;
  top: 24px; 
  right: 24px;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close-full-screen:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}

@media (min-width: 769px) and (max-width: 1024px) {
  /* Tablet styles */
  .locators-grid {
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 16px;
  }
  
  .locator-card {
    border-radius: 14px;
  }
  
  .image-carousel {
    height: 180px;
  }
  
  .locator-card-content {
    padding: 14px;
  }
  
  .locator-name {
    font-size: 17px;
  }
  
  .locator-category {
    font-size: 13px;
  }
  
  .info-item {
    padding: 10px;
  }
  
  .info-icon {
    width: 28px;
    height: 28px;
  }
  
  .info-label {
    font-size: 10px;
  }
  
  .info-value {
    font-size: 12px;
  }
  
  .locator-description {
    font-size: 13px;
    margin: 10px 0;
  }
  
  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }
  
  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }
  
  .locators-filter-container {
   top: 72px;
    }
}

/* ===== Laptop & Desktop (1025px+) - Compact Version ===== */
@media (min-width: 1025px) {
  .locators-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); /* Reduced from 340px */
    gap: 16px; /* Reduced from 18px */
    padding-bottom: 120px; /* Reduced from 140px */
  }

  .locator-card {
    border-radius: 14px; /* Slightly smaller radius */
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); /* Softer shadow */
  }

  .image-carousel {
    height: 180px; /* Reduced from 200px */
  }

  .locator-card-content {
    padding: 14px; /* Reduced from 16px */
  }

  .locator-name {
    font-size: 16px; /* Reduced from 17px */
    margin-bottom: 3px; /* Tighter spacing */
  }

  .locator-category {
    font-size: 13px; /* Reduced from 14px */
    margin-bottom: 6px; /* Tighter spacing */
  }

  .info-item {
    padding: 10px; /* Reduced from 12px */
    gap: 8px; /* Reduced from 10px */
  }

  .info-icon {
    width: 28px; /* Reduced from 30px */
    height: 28px;
    border-radius: 6px; /* Smaller radius */
  }

  .info-icon i {
    font-size: 13px; /* Reduced from 14px */
  }

  .info-label {
    font-size: 10px; /* Reduced from 11px */
    margin-bottom: 4px; /* Tighter spacing */
  }

  .info-value {
    font-size: 12px; /* Reduced from 13px */
  }

  .locator-description {
    font-size: 13px; /* Reduced from 14px */
    margin: 10px 0; /* Tighter spacing */
    -webkit-line-clamp: 2;
  }

  .tag {
    padding: 3px 8px; /* More compact */
    font-size: 11px; /* Reduced from 12px */
  }

  .action-button {
    padding: 12px 0; /* Reduced from 14px */
    font-size: 13px; /* Reduced from 14px */
  }
}

/* No Results State */
.no-results {
  text-align: center;
  padding: 40px 20px;
  max-width: 600px;
  margin: 0 auto;
}

.no-results-icon {
  font-size: 3rem;
  color: #fff;
  margin-bottom: 20px;
}

.no-results h3 {
  font-size: 1.5rem;
  color: #fff;
  margin-bottom: 8px;
}

.no-results p {
  color: #fff;
  margin-bottom: 24px;
}

/* Suggestions */
.suggestions {
  background: #f9f9f9;
  border-radius: 12px;
  padding: 20px;
  margin-top: 20px;
}

.suggestions p {
  font-weight: 500;
  margin-bottom: 12px;
  color: #333;
}

.suggestions ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.suggestions li {
  padding: 12px 16px;
  margin-bottom: 8px;
  background: #000;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.2s ease;
  border: 1px solid #eee;
}

.suggestions li:hover {
  background: #000;
}

  `;
  document.head.appendChild(style);
}

function renderLocatorCards(filteredData) {
  return filteredData.map(locator => {
    const tagsHTML = locator.details.tags && locator.details.tags.length > 0 
      ? `<div class="tag-container">
          ${locator.details.tags.map(tag => `<span class="tag">${tag}</span>`).join('')}
         </div>`
      : '';
    
    const actionButtonsHTML = `
      <a href="${locator.details.mapLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
        <i class="fas fa-map-marker-alt"></i> Map
      </a>
      <button class="action-button" onclick="openStorePage('${locator.details.name}'); event.stopPropagation()">
        <i class="fas fa-info-circle"></i> Details
      </button>
      <button class="action-button" onclick="showChatSupport('${locator.details.name}'); event.stopPropagation()">
        <i class="fas fa-comments"></i> Chat
      </button>
    `;
    
    return `
      <div class="locator-card" onclick="openStorePage('${locator.details.name}')">
        <div class="image-carousel">
          ${locator.details.cardImages.map((image, index) => `
            <div class="carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${locator.details.name}" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="carousel-pagination">
            ${locator.details.cardImages.map((_, index) => `
              <div class="pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="carousel-control prev" onclick="changeCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="carousel-control next" onclick="changeCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        
        <div class="locator-card-content">
          <div class="locator-card-header">
            <div style="display: flex; justify-content: space-between; align-items: flex-start;">
              <h3 class="locator-name">${locator.details.name}</h3>
              <div class="locator-rating">
                <span class="star">${locator.details.rating || '4.0'} ★</span>
              </div>
            </div>
            
            <div class="locator-category">
              <span>${locator.category} • ${locator.metropolis}</span>
            </div>
            
            <div class="locator-info-grid">
              <div class="info-item">
                <div class="info-icon">
                  <i class="fas fa-map-marker-alt"></i>
                </div>
                <div class="info-content">
                  <span class="info-label">Location & Hours</span>
                  <span class="info-value">${locator.details.address}</span>
                  <span class="info-value" style="margin-top: 4px;">${locator.details.timings}</span>
                </div>
              </div>
            </div>
            
            ${tagsHTML}
          </div>
        </div>

        <div class="locator-card-actions">
          ${actionButtonsHTML}
        </div>
      </div>
    `;
  }).join('');
}

function openStorePage(storeName) {
  const store = locatorsData.find(loc => loc.details.name === storeName);
  if (!store) return;

  // Set the current store in cart state
  cartState.store = store;

  const storePage = document.createElement('div');
  storePage.className = 'store-page';
  storePage.innerHTML = `
    <div class="store-header">
      <button class="back-button" onclick="closeStorePage()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>${store.details.name}</h2>
      <div class="header-actions">
        <button class="cart-button" onclick="showCart()">
          <i class="fas fa-shopping-cart"></i>
          ${cartState.items.length > 0 ? `<span class="cart-count">${cartState.items.length}</span>` : ''}
        </button>
        <button class="share-button" onclick="shareStore('${store.details.name}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="store-content">
      <!-- Hero Section with Store Image Carousel -->
      <div class="store-hero">
        <div class="store-image-carousel">
          ${store.details.storeImages.map((image, index) => `
            <div class="store-carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${store.details.name}" class="store-image" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="store-carousel-pagination">
            ${store.details.storeImages.map((_, index) => `
              <div class="store-pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentStoreSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="store-carousel-control prev" onclick="changeStoreCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="store-carousel-control next" onclick="changeStoreCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        <div class="store-basic-info">
          <div class="store-rating">
            <i class="fas fa-star"></i> ${store.details.rating} (${store.details.reviews})
          </div>
          <div class="store-category">
            ${store.category}
          </div>
          <div class="store-timings">
            <i class="fas fa-clock"></i> ${store.details.timings}
          </div>
        </div>
      </div>
      
      <!-- Store Description Section -->
      <div class="store-description-section">
        <h3>About</h3>
        <p>${store.details.description}</p>
        <div class="established-since">
          <i class="fas fa-calendar-alt"></i> Established: ${store.details.established}
        </div>
      </div>
      
      <!-- Enhanced Services Section -->
      <div class="store-services-section">
        <h3>${getServicesSectionTitle(store.category)}</h3>
        <div class="services-container">
          ${renderServices(store.details.services, store.category)}
        </div>
      </div>
      
      <!-- Facilities Section -->
      <div class="store-facilities-section">
        <h3>Facilities</h3>
        <div class="facilities-grid">
          ${store.details.facilities?.map(facility => `
            <div class="facility-item">
              <div class="facility-icon">
                <i class="fas fa-${getFacilityIcon(facility.name)}"></i>
              </div>
              <div class="facility-info">
                <h4>${facility.name}</h4>
                <p>${facility.description}</p>
              </div>
            </div>
          `).join('') || '<p>No facilities information available</p>'}
        </div>
      </div>
      
      <!-- Promotions Section -->
      <div class="store-promos-section">
        <h3>Special Offers</h3>
        <div class="promos-grid">
          ${store.details.promos?.map(promo => `
            <div class="promo-item">
              <div class="promo-info">
                <h4>${promo.name}</h4>
                <p>${promo.description}</p>
              </div>
              <div class="promo-code">
                <span>${promo.code}</span>
                <button onclick="copyToClipboard('${promo.code}')">Copy</button>
              </div>
            </div>
          `).join('') || '<p>No current promotions</p>'}
        </div>
      </div>
      
      <!-- Contact Section -->
      <div class="store-contact-section">
        <h3>Contact</h3>
        <div class="contact-methods">
          ${store.details.support?.phone ? `
            <a href="tel:${store.details.support.phone}" class="contact-item">
              <i class="fas fa-phone"></i> ${store.details.support.phone}
            </a>
          ` : ''}
          ${store.details.support?.whatsapp ? `
            <a href="https://wa.me/${store.details.support.whatsapp}" target="_blank" class="contact-item">
              <i class="fab fa-whatsapp"></i> WhatsApp
            </a>
          ` : ''}
          ${store.details.support?.email ? `
            <a href="mailto:${store.details.support.email}" class="contact-item">
              <i class="fas fa-envelope"></i> ${store.details.support.email}
            </a>
          ` : ''}
          ${store.details.website ? `
            <a href="${store.details.website}" target="_blank" class="contact-item">
              <i class="fas fa-globe"></i> Visit Website
            </a>
          ` : ''}
        </div>
        
        <div class="social-links">
          ${store.details.social?.facebook ? `
            <a href="${store.details.social.facebook}" target="_blank" class="social-link">
              <i class="fab fa-facebook-f"></i>
            </a>
          ` : ''}
          ${store.details.social?.twitter ? `
            <a href="${store.details.social.twitter}" target="_blank" class="social-link">
              <i class="fab fa-twitter"></i>
            </a>
          ` : ''}
          ${store.details.social?.instagram ? `
            <a href="${store.details.social.instagram}" target="_blank" class="social-link">
              <i class="fab fa-instagram"></i>
            </a>
          ` : ''}
        </div>
      </div>
      
      <!-- Store Action Buttons -->
      <div class="store-actions">
        <a href="${store.details.mapLink}" target="_blank" class="primary-action">
          <i class="fas fa-directions"></i> Get Directions
        </a>
        <button class="secondary-action" onclick="showChatSupport('${store.details.name}')">
          <i class="fas fa-comment-alt"></i> Contact Store
        </button>
      </div>
    </div>
    
    <!-- Cart Overlay -->
    <div class="cart-overlay" id="cartOverlay">
      <div class="cart-container">
        <div class="cart-header">
          <h3>Your Cart</h3>
          <button class="close-cart" onclick="hideCart()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        
        <div class="cart-items" id="cartItems">
          ${renderCartItems()}
        </div>
        
        <div class="cart-summary">
          <div class="cart-total">
            <span>Total:</span>
            <span id="cartTotalAmount">₹${cartState.total.toFixed(2)}</span>
          </div>
          <div class="cart-actions">
            <button class="clear-cart" onclick="clearCart()">
              <i class="fas fa-trash"></i> Clear Cart
            </button>
            <button class="checkout-btn" onclick="showCheckoutForm()">
              <i class="fas fa-credit-card"></i> Checkout
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Order Form Overlay -->
    <div class="order-form-overlay" id="orderFormOverlay">
      <div class="order-form-container">
        <button class="close-order-form" onclick="closeOrderForm()">
          <i class="fas fa-times"></i>
        </button>
        <h3>Checkout</h3>
        <form id="orderForm">
          <div class="form-group">
            <label for="customerName">Full Name</label>
            <input type="text" id="customerName" required>
          </div>
          <div class="form-group">
            <label for="customerPhone">Phone Number</label>
            <input type="tel" id="customerPhone" required>
          </div>
          <div class="form-group">
            <label for="customerAddress">Delivery Address</label>
            <textarea id="customerAddress" rows="3" required></textarea>
          </div>
          <div class="form-group">
            <label for="deliveryTime">Preferred Delivery/Pickup Time</label>
            <input type="datetime-local" id="deliveryTime" required>
          </div>
          <div class="form-group">
            <label for="specialInstructions">Special Instructions</label>
            <textarea id="specialInstructions" rows="2"></textarea>
          </div>
          <div class="form-group">
            <label for="paymentMethod">Payment Method</label>
            <select id="paymentMethod" required>
              <option value="">Select payment method</option>
              <option value="Cash on Delivery">Cash on Delivery</option>
              <option value="Online Payment">Online Payment</option>
              <option value="Card on Delivery">Card on Delivery</option>
            </select>
          </div>
          
          <div class="order-summary">
            <h4>Order Summary</h4>
            <div id="orderSummaryItems">
              ${renderOrderSummaryItems()}
            </div>
            <div class="order-total">
              <span>Total:</span>
              <span id="orderTotal">₹${cartState.total.toFixed(2)}</span>
            </div>
          </div>
          
          <div class="coupon-section">
            <div class="form-group">
              <label for="couponCode">Coupon Code</label>
              <div class="coupon-input">
                <input type="text" id="couponCode" placeholder="Enter coupon code">
                <button type="button" class="apply-coupon" onclick="applyCoupon()">Apply</button>
              </div>
              <div id="couponMessage" class="coupon-message"></div>
            </div>
          </div>
          
          <button type="submit" class="submit-order">
            <i class="fas fa-paper-plane"></i> Place Order
          </button>
        </form>
      </div>
    </div>
    
    <!-- Order Confirmation Modal -->
    <div class="confirmation-modal" id="confirmationModal">
      <div class="confirmation-content">
        <div class="confirmation-icon">
          <i class="fas fa-check-circle"></i>
        </div>
        <h3>Order Confirmed!</h3>
        <p id="confirmationMessage">Your order has been placed successfully.</p>
        <div class="confirmation-actions">
          <button class="view-receipt" onclick="viewOrderReceipt()">
            <i class="fas fa-receipt"></i> View Receipt
          </button>
          <button class="close-confirmation" onclick="closeConfirmationModal()">
            <i class="fas fa-times"></i> Close
          </button>
        </div>
      </div>
    </div>
    
    <!-- Toast Notification -->
    <div class="toast"></div>
  `;

  document.body.appendChild(storePage);
  addStorePageStyles();
  initializeTabs();
  initializeOrderForm(store);
  initializeStoreCarousel();
  initializeSearch();
  scrollToTop();
}

// Helper functions for the store page
function getServicesSectionTitle(category) {
  const titles = {
    "Grocery": "Products",
    "Food": "Menu",
    "Electronics": "Products",
    "Medicines": "Medicines & Health Products",
    "Laundry": "Services",
    "Fashion": "Collections",
    "Furniture": "Products"
  };
  return titles[category] || "Products & Services";
}

function renderServices(services, category) {
  if (!services || services.length === 0) {
    return '<p class="no-services">No services information available</p>';
  }

  return services.map(service => {
    if (category === "Grocery" || category === "Electronics") {
      return `
        <div class="product-item" data-name="${service.name}" data-price="${service.price.replace('₹','')}">
          <div class="product-image">
            <img src="${service.image}" alt="${service.name}">
          </div>
          <div class="product-details">
            <h4>${service.name}</h4>
            <p class="product-description">${service.description}</p>
            <div class="product-pricing">
              <span class="product-price">${service.price}</span>
              ${service.mrp ? `<span class="product-mrp"><del>${service.mrp}</del></span>` : ''}
            </div>
            <div class="product-meta">
              ${service.quantity ? `<span class="product-quantity"><i class="fas fa-weight"></i> ${service.quantity}</span>` : ''}
              <span class="product-stock ${service.stock === 'In Stock' ? 'in-stock' : 'out-of-stock'}">
                <i class="fas ${service.stock === 'In Stock' ? 'fa-check-circle' : 'fa-times-circle'}"></i> ${service.stock}
              </span>
            </div>
            ${renderVariants(service.variants)}
            <div class="product-actions">
              <button class="add-to-cart" onclick="addToCart(event, '${service.name}', ${service.price.replace('₹','')})">
                <i class="fas fa-cart-plus"></i> Add to Cart
              </button>
              <button class="buy-now" onclick="showOrderForm(event, '${service.name}', ${service.price.replace('₹','')})">
                <i class="fas fa-bolt"></i> Buy Now
              </button>
            </div>
          </div>
        </div>
      `;
    } else if (category === "Food") {
      return `
        <div class="fare-item" data-name="${service.name}" data-price="${service.price.replace('₹','')}">
          <div class="fare-image">
            <img src="${service.image}" alt="${service.name}">
          </div>
          <div class="fare-details">
            <h4>${service.name}</h4>
            <p class="fare-description">${service.description}</p>
            ${renderVariants(service.variants)}
            <div class="fare-actions">
              <button class="add-to-cart" onclick="addToCart(event, '${service.name}', ${service.price.replace('₹','')})">
                <i class="fas fa-cart-plus"></i> Add to Cart
              </button>
              <button class="order-now" onclick="showOrderForm(event, '${service.name}', ${service.price.replace('₹','')})">
                <i class="fas fa-utensils"></i> Order Now
              </button>
            </div>
          </div>
          <div class="fare-price">
            ${service.price}
          </div>
        </div>
      `;
    } else {
      // Default service item for other categories
      return `
        <div class="service-item">
          <h4>${service.name}</h4>
          <div class="service-price">${service.price}</div>
          <p>${service.description}</p>
          <button class="book-service" onclick="showOrderForm(event, '${service.name}', ${service.price.replace('₹','')})">
            <i class="fas fa-calendar-check"></i> Book Now
          </button>
        </div>
      `;
    }
  }).join('');
}

function renderVariants(variants) {
  if (!variants || variants.length === 0) return '';
  
  return `
    <div class="variants-container">
      <select class="variant-select" onchange="updatePrice(this)">
        ${variants.map(variant => `
          <option value="${variant.price.replace('₹','')}" data-name="${variant.name}">
            ${variant.name} - ${variant.price}
          </option>
        `).join('')}
      </select>
    </div>
  `;
}

function initializeOrderForm() {
  const orderForm = document.getElementById('orderForm');
  if (!orderForm) return;

  orderForm.addEventListener('submit', function(e) {
    e.preventDefault();
    
    // Get form values
    const customerName = document.getElementById('customerName').value;
    const customerPhone = document.getElementById('customerPhone').value;
    const customerAddress = document.getElementById('customerAddress').value;
    const deliveryTime = document.getElementById('deliveryTime').value;
    const specialInstructions = document.getElementById('specialInstructions').value;
    const paymentMethod = document.getElementById('paymentMethod').value;
    
    // Format delivery time
    let formattedDeliveryTime = '';
    if (deliveryTime) {
      const deliveryDate = new Date(deliveryTime);
      formattedDeliveryTime = deliveryDate.toLocaleString('en-IN', {
        weekday: 'short',
        day: 'numeric',
        month: 'short',
        year: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      });
    }
    
    // Determine which items to include in the order
    const orderItems = cartState.items.length > 0 ? 
      cartState.items : 
      (cartState.directOrderItems || []);
    
    // Calculate subtotal (before discount)
    const subtotal = orderItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    
    // Use discounted total if coupon was applied, otherwise use regular total
    const orderTotal = cartState.appliedCoupon ? 
      cartState.discountedTotal : 
      subtotal;
    
    // Create order details message
    let orderDetails = `*NEW ORDER NOTIFICATION*\n\n`;
    orderDetails += `*Store:* ${cartState.store.details.name}\n`;
    orderDetails += `*Order Time:* ${new Date().toLocaleString('en-IN')}\n\n`;
    
    orderDetails += `*Customer Details:*\n`;
    orderDetails += `👤 Name: ${customerName}\n`;
    orderDetails += `📞 Phone: ${customerPhone}\n`;
    orderDetails += `🏠 Address: ${customerAddress}\n`;
    if (formattedDeliveryTime) {
      orderDetails += `⏰ Delivery Time: ${formattedDeliveryTime}\n`;
    }
    if (specialInstructions) {
      orderDetails += `📝 Special Instructions: ${specialInstructions}\n`;
    }
    orderDetails += `💳 Payment Method: ${paymentMethod}\n\n`;
    
    orderDetails += `*Order Items:*\n`;
    orderDetails += orderItems.map(item => 
      `- ${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}: ₹${(item.price * item.quantity).toFixed(2)}`
    ).join('\n');
    
    // Add discount information if coupon was applied
    if (cartState.appliedCoupon) {
      orderDetails += `\n\n*Discount Applied:*\n`;
      orderDetails += `Coupon Code: ${cartState.appliedCoupon.code}\n`;
      orderDetails += `Discount: ${(cartState.appliedCoupon.discount * 100)}%\n`;
      orderDetails += `Discount Amount: -₹${cartState.appliedCoupon.discountAmount.toFixed(2)}\n`;
      orderDetails += `\n*Subtotal:* ₹${subtotal.toFixed(2)}`;
      orderDetails += `\n*Discount:* -₹${cartState.appliedCoupon.discountAmount.toFixed(2)}`;
    }
    
    orderDetails += `\n*Order Total:* ₹${orderTotal.toFixed(2)}`;
    
    // Send order based on store's preferred contact method
    if (cartState.store.details.support.primaryContact === "whatsapp" && cartState.store.details.support.whatsapp) {
      const whatsappUrl = `https://wa.me/${cartState.store.details.support.whatsapp}?text=${encodeURIComponent(orderDetails)}`;
      window.open(whatsappUrl, '_blank');
    } else if (cartState.store.details.support.primaryContact === "email" && cartState.store.details.support.email) {
      const subject = `New Order from ${customerName} - ${cartState.store.details.name}`;
      const body = orderDetails.replace(/\*/g, '');
      const mailtoUrl = `mailto:${cartState.store.details.support.email}?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
      window.location.href = mailtoUrl;
    }
    
    // Show confirmation with the correct total
    showOrderConfirmation(customerName, orderTotal.toFixed(2));
    
    // Clear cart/direct order and close form
    clearCart();
    delete cartState.directOrderItems;
    closeOrderForm();
  });
}

function applyCoupon() {
  const couponCode = document.getElementById('couponCode').value;
  const couponMessage = document.getElementById('couponMessage');
  
  if (!couponCode) {
    couponMessage.textContent = 'Please enter a coupon code';
    couponMessage.className = 'coupon-message error';
    return;
  }
  
  // Get the current total (either from cart or direct order)
  let currentTotal;
  if (cartState.items.length > 0) {
    currentTotal = cartState.total;
  } else if (cartState.directOrderItems) {
    currentTotal = cartState.directOrderItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  } else {
    couponMessage.textContent = 'No items to apply coupon to';
    couponMessage.className = 'coupon-message error';
    return;
  }

  // Valid coupons with their discount percentages
  const validCoupons = {
    'SAVE10': { discount: 0.1, message: '10% discount applied!' },
    'FREESHIP': { discount: 0, message: 'Free shipping applied!' },
    'WELCOME20': { discount: 0.2, message: '20% discount applied!' }
  };
  
  if (validCoupons[couponCode]) {
    const coupon = validCoupons[couponCode];
    const discountAmount = currentTotal * coupon.discount;
    const newTotal = currentTotal - discountAmount;
    
    // Update cart state with discount information
    cartState.discountedTotal = newTotal;
    cartState.appliedCoupon = {
      code: couponCode,
      discount: coupon.discount,
      discountAmount: discountAmount
    };
    
    // Update UI
    couponMessage.textContent = coupon.message;
    couponMessage.className = 'coupon-message success';
    
    // Update order total display
    document.getElementById('orderTotal').textContent = `₹${newTotal.toFixed(2)}`;
  } else {
    couponMessage.textContent = 'Invalid coupon code';
    couponMessage.className = 'coupon-message error';
    // Reset discount if invalid coupon
    cartState.discountedTotal = currentTotal;
    cartState.appliedCoupon = null;
    document.getElementById('orderTotal').textContent = `₹${currentTotal.toFixed(2)}`;
  }
}

// Order Confirmation Functions
function showOrderConfirmation(customerName, orderTotal) {
  const confirmationModal = document.getElementById('confirmationModal');
  const confirmationMessage = document.getElementById('confirmationMessage');
  
  if (confirmationModal && confirmationMessage) {
    confirmationMessage.textContent = `Thank you, ${customerName}! Your order of ₹${orderTotal} has been placed successfully.`;
    confirmationModal.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function closeConfirmationModal() {
  const confirmationModal = document.getElementById('confirmationModal');
  if (confirmationModal) {
    confirmationModal.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function viewOrderReceipt() {
  // In a real app, this would generate or show a detailed receipt
  alert('Receipt would be displayed here in a real implementation.');
  closeConfirmationModal();
}

// Helper Functions
function showToast(message) {
  let toast = document.querySelector('.toast');
  if (!toast) {
    toast = document.createElement('div');
    toast.className = 'toast';
    document.body.appendChild(toast);
  }
  
  toast.textContent = message;
  toast.classList.add('show');
  
  setTimeout(() => {
    toast.classList.remove('show');
  }, 3000);
}

let cartItems = [];

// Cart Management Functions
function renderCartItems() {
  if (cartState.items.length === 0) {
    return `
      <div class="empty-cart">
        <i class="fas fa-shopping-cart"></i>
        <p>Your cart is empty</p>
        <button onclick="hideCart()">Continue Shopping</button>
      </div>
    `;
  }

  return cartState.items.map((item, index) => `
    <div class="cart-item" data-index="${index}">
      <div class="cart-item-image">
        <img src="${item.image}" alt="${item.name}">
      </div>
      <div class="cart-item-details">
        <h4>${item.name}</h4>
        ${item.variant ? `<p class="variant">${item.variant}</p>` : ''}
        <div class="cart-item-price">₹${item.price.toFixed(2)}</div>
        <div class="cart-item-quantity">
          <button class="quantity-btn minus" onclick="updateQuantity(${index}, -1)">−</button>
          <span class="quantity">${item.quantity}</span>
          <button class="quantity-btn plus" onclick="updateQuantity(${index}, 1)">+</button>
        </div>
      </div>
      <button class="remove-item" onclick="removeFromCart(${index})">
        <i class="fas fa-times"></i>
      </button>
    </div>
  `).join('');
}

function renderOrderSummaryItems() {
  return cartState.items.map(item => `
    <div class="order-item">
      <span>${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}</span>
      <span>₹${(item.price * item.quantity).toFixed(2)}</span>
    </div>
  `).join('');
}

function addToCart(event, itemName, basePrice) {
  event.stopPropagation();
  
  const itemElement = event.target.closest('[data-name]');
  const variantSelect = itemElement?.querySelector('.variant-select');
  
  let variantName = null;
  let price = parseFloat(basePrice);
  let image = itemElement.querySelector('img')?.src || '';
  
  // Handle variants if they exist
  if (variantSelect) {
    const selectedOption = variantSelect.options[variantSelect.selectedIndex];
    variantName = selectedOption.dataset.name;
    price = parseFloat(selectedOption.value);
  }
  
  // Check if item already exists in cart
  const existingItemIndex = cartState.items.findIndex(item => 
    item.name === itemName && item.variant === variantName
  );
  
  if (existingItemIndex >= 0) {
    // Update quantity if item exists
    cartState.items[existingItemIndex].quantity += 1;
  } else {
    // Add new item to cart
    cartState.items.push({
      name: itemName,
      variant: variantName,
      price: price,
      quantity: 1,
      image: image
    });
  }
  
  // Update cart total
  updateCartTotal();
  
  // Update cart UI
  updateCartUI();
  
  // Show success message
  showToast(`${itemName} ${variantName ? `(${variantName})` : ''} added to cart`);
}

function removeFromCart(index) {
  cartState.items.splice(index, 1);
  updateCartTotal();
  updateCartUI();
  
  if (cartState.items.length === 0) {
    hideCart();
  }
}

function updateQuantity(index, change) {
  const newQuantity = cartState.items[index].quantity + change;
  
  if (newQuantity < 1) {
    removeFromCart(index);
    return;
  }
  
  cartState.items[index].quantity = newQuantity;
  updateCartTotal();
  updateCartUI();
}

function clearCart() {
  cartState.items = [];
  cartState.total = 0;
  cartState.discountedTotal = 0;
  cartState.appliedCoupon = null;
  updateCartUI();
  hideCart();
}

function updateCartTotal() {
  cartState.total = cartState.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  
  // Reapply coupon discount if one was applied
  if (cartState.appliedCoupon) {
    const discountAmount = cartState.total * cartState.appliedCoupon.discount;
    cartState.discountedTotal = cartState.total - discountAmount;
    cartState.appliedCoupon.discountAmount = discountAmount;
  } else {
    cartState.discountedTotal = cartState.total;
  }
}

function updateCartUI() {
  const cartOverlay = document.getElementById('cartOverlay');
  if (!cartOverlay) return;
  
  // Update cart items list
  const cartItemsContainer = document.getElementById('cartItems');
  if (cartItemsContainer) {
    cartItemsContainer.innerHTML = renderCartItems();
  }
  
  // Update cart total (show discounted total if available)
  const cartTotalElement = document.getElementById('cartTotalAmount');
  if (cartTotalElement) {
    const displayTotal = cartState.appliedCoupon ? cartState.discountedTotal : cartState.total;
    cartTotalElement.textContent = `₹${displayTotal.toFixed(2)}`;
  }
  
  // Update cart count in header
  const cartButton = document.querySelector('.cart-button');
  if (cartButton) {
    const cartCount = cartButton.querySelector('.cart-count');
    
    if (cartState.items.length > 0) {
      if (!cartCount) {
        const countElement = document.createElement('span');
        countElement.className = 'cart-count';
        countElement.textContent = cartState.items.length;
        cartButton.appendChild(countElement);
      } else {
        cartCount.textContent = cartState.items.length;
      }
    } else if (cartCount) {
      cartCount.remove();
    }
  }
  
  // Update order summary in checkout form
  const orderSummaryItems = document.getElementById('orderSummaryItems');
  const orderTotal = document.getElementById('orderTotal');
  
  if (orderSummaryItems && orderTotal) {
    orderSummaryItems.innerHTML = renderOrderSummaryItems();
    const displayTotal = cartState.appliedCoupon ? cartState.discountedTotal : cartState.total;
    orderTotal.textContent = `₹${displayTotal.toFixed(2)}`;
  }
}

// Cart Visibility Functions
function showCart() {
  const cartOverlay = document.getElementById('cartOverlay');
  if (cartOverlay) {
    cartOverlay.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function hideCart() {
  const cartOverlay = document.getElementById('cartOverlay');
  if (cartOverlay) {
    cartOverlay.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function showCheckoutForm() {
  if (cartState.items.length === 0) {
    showToast('Your cart is empty');
    return;
  }
  
  hideCart();
  
  const orderFormOverlay = document.getElementById('orderFormOverlay');
  if (orderFormOverlay) {
    orderFormOverlay.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function showOrderForm(event, itemName, itemPrice) {
  event.stopPropagation();
  const itemElement = event.target.closest('[data-name]');
  const variantSelect = itemElement?.querySelector('.variant-select');
  
  let items = [];
  if (variantSelect) {
    const selectedOption = variantSelect.options[variantSelect.selectedIndex];
    items.push({
      name: `${itemName} (${selectedOption.dataset.name})`,
      price: parseFloat(selectedOption.value),
      quantity: 1,
      image: itemElement.querySelector('img')?.src || ''
    });
  } else {
    items.push({
      name: itemName,
      price: itemPrice,
      quantity: 1,
      image: itemElement.querySelector('img')?.src || ''
    });
  }

  // Store the direct order items temporarily
  cartState.directOrderItems = items;
  // Reset any previous coupon
  cartState.appliedCoupon = null;
  cartState.discountedTotal = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  
  openOrderForm(items);
}

function openOrderForm(items) {
  const orderFormOverlay = document.getElementById('orderFormOverlay') || 
    document.querySelector('.order-form-overlay');
  
  if (!orderFormOverlay) return;

  orderFormOverlay.classList.add('active');
  
  const orderSummary = orderFormOverlay.querySelector('#orderSummaryItems');
  let total = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  
  // Reset coupon message
  const couponMessage = orderFormOverlay.querySelector('#couponMessage');
  if (couponMessage) {
    couponMessage.textContent = '';
    couponMessage.className = 'coupon-message';
  }
  
  // Reset coupon code input
  const couponCodeInput = orderFormOverlay.querySelector('#couponCode');
  if (couponCodeInput) {
    couponCodeInput.value = '';
  }
  
  orderSummary.innerHTML = items.map(item => {
    return `
      <div class="order-item">
        <span>${item.name} × ${item.quantity}</span>
        <span>₹${(item.price * item.quantity).toFixed(2)}</span>
      </div>
    `;
  }).join('');
  
  orderFormOverlay.querySelector('#orderTotal').textContent = `₹${total.toFixed(2)}`;
}

function closeOrderForm() {
  const orderFormOverlay = document.getElementById('orderFormOverlay');
  if (orderFormOverlay) {
    orderFormOverlay.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function closeStorePage() {
  const storePage = document.querySelector('.store-page');
  if (storePage) {
    storePage.remove();
  }
}

function addStorePageStyles() {
  const style = document.createElement('style');
  style.textContent = `
.store-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #1a1a1f;
  z-index: 2000;
  overflow-y: auto;
  color: #fff;
  font-family: poppins;
}

.store-header {
  position: sticky;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  z-index: 10;
  backdrop-filter: blur(12px);
}

.back-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.5em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.back-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.store-header h2 {
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  text-align: center;
  flex: 1;
  padding: 0 16px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.header-actions {
  display: flex;
  gap: 8px;
}

.share-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.2em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.share-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.store-content {
  padding: 0 0 100px;
  width: 100%;
  margin: 0 auto;
}

.store-hero {
  position: relative;
  padding: 20px;
}

.store-image-container {
  width: 100%;
  height: 200px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
}

.store-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.store-image:hover {
  transform: scale(1.02);
}

.store-basic-info {
  display: flex;
  gap: 12px;
  margin-top: 16px;
  flex-wrap: wrap;
}

.store-rating,
.store-category,
.store-timings {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 12px;
  border-radius: 20px;
  font-size: 0.9em;
  font-weight: 500;
}

.store-rating {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
}

.store-category {
  background: rgba(0, 191, 255, 0.1);
  color: #00bfff;
}

.store-timings {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

.established-since {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-top: 12px;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.store-description-section {
  padding: 0 20px 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-description-section h3 {
  margin: 0 0 12px;
  font-size: 1.2em;
  font-weight: 600;
}

.store-description-section p {
  margin: 0;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.8);
}

.store-services-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-services-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.services-grid {
  display: grid;
  gap: 16px;
}

.service-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.service-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.service-item h4 {
  margin: 0 0 8px;
  font-size: 1.1em;
  font-weight: 600;
}

.service-price {
  color: #ffd700;
  font-weight: 600;
  margin-bottom: 8px;
}

.service-item p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.store-facilities-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-facilities-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.facilities-grid {
  display: grid;
  gap: 16px;
}

.facility-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.facility-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.facility-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: #ffd700;
  font-size: 1.2em;
}

.facility-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.facility-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.store-promos-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-promos-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.promos-grid {
  display: grid;
  gap: 16px;
}

.promo-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.promo-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.promo-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.promo-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.promo-code {
  display: flex;
  align-items: center;
  gap: 8px;
}

.promo-code span {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
  padding: 6px 12px;
  border-radius: 8px;
  font-weight: 600;
}

.promo-code button {
  background: rgba(255, 215, 0, 0.1);
  border: none;
  color: #ffd700;
  padding: 6px 12px;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.promo-code button:hover {
  background: rgba(255, 215, 0, 0.2);
}

.store-contact-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-contact-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.contact-methods {
  display: grid;
  gap: 12px;
  margin-bottom: 20px;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 8px;
  color: #fff;
  text-decoration: none;
  transition: background 0.3s ease;
}

.contact-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.social-links {
  display: flex;
  gap: 16px;
  margin-top: 20px;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.05);
  color: #fff;
  font-size: 1.2em;
  transition: transform 0.3s ease, background 0.3s ease;
}

.social-link:hover {
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.1);
}

.store-actions {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  z-index: 10;
  backdrop-filter: blur(12px);
}

.primary-action,
.secondary-action {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 14px;
  border-radius: 8px;
  font-weight: 500;
  font-size: 0.95em;
  cursor: pointer;
  transition: transform 0.3s ease, background 0.3s ease;
}

.primary-action {
  background: var(--primary-color, #ffd700);
  color: #000;
  border: none;
}

.primary-action:hover {
  background: #ffd700;
  transform: translateY(-2px);
}

.secondary-action {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  border: none;
}

.secondary-action:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

@media (min-width: 768px) {
  .store-header {
    padding: 20px 30px;
  }
  
  .store-header h2 {
    font-size: 1.4em;
  }
  
  .store-content {
    max-width: 90%;
    margin: 0 auto;
  }
  
  .store-hero {
    padding: 30px;
  }
  
  .store-image-container {
    height: 280px;
    border-radius: 20px;
  }
  
  .store-basic-info {
    gap: 16px;
    margin-top: 24px;
  }
  
  .store-rating,
  .store-category,
  .store-timings {
    padding: 10px 16px;
    font-size: 1em;
  }
  
  .store-description-section,
  .store-services-section,
  .store-facilities-section,
  .store-promos-section,
  .store-contact-section {
    padding: 0 30px 30px;
  }
  
  .store-description-section h3,
  .store-services-section h3,
  .store-facilities-section h3,
  .store-promos-section h3,
  .store-contact-section h3 {
    font-size: 1.3em;
    margin-bottom: 16px;
  }
  
  .store-description-section p {
    font-size: 1em;
    line-height: 1.6;
  }
  
  .services-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .facilities-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .store-actions {
    padding: 20px 30px;
    max-width: 90%;
    margin: 0 auto;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 12px 12px 0 0;
  }
  
  .primary-action,
  .secondary-action {
    padding: 14px;
    font-size: 1em;
  }
}

@media (min-width: 1024px) {
  .store-content {
    max-width: 900px;
    margin: 0 auto;
    padding: 0 0 120px;
  }
  
  .store-header {
    padding: 22px 32px;
  }
  
  .store-header h2 {
    font-size: 1.5em;
  }
  
  .store-hero {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
    padding: 30px;
    align-items: center;
  }
  
  .store-image-container {
    height: 350px;
    border-radius: 20px;
  }
  
  .store-hero-content {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  
  .store-name {
    font-size: 1.8em;
    font-weight: 600;
    margin: 0;
  }
  
  .store-basic-info {
    margin-top: 0;
  }
  
  .store-description-section {
    padding: 0 30px 30px;
    max-width: 85%;
    margin: 0 auto;
  }
  
  .store-description-section h3 {
    font-size: 1.5em;
  }
  
  .store-description-section p {
    font-size: 1.05em;
    line-height: 1.7;
  }
  
  .store-services-section,
  .store-facilities-section,
  .store-promos-section,
  .store-contact-section {
    padding: 30px;
  }
  
  .store-services-section h3,
  .store-facilities-section h3,
  .store-promos-section h3,
  .store-contact-section h3 {
    font-size: 1.5em;
  }
  
  .services-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .facilities-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .social-links {
    gap: 18px;
  }
  
  .social-link {
    width: 44px;
    height: 44px;
    font-size: 1.3em;
  }
  
  .store-actions {
    max-width: 700px;
    left: 50%;
    transform: translateX(-50%);
  }
  
  .primary-action,
  .secondary-action {
    padding: 16px;
    font-size: 1.05em;
  }
}

@media (min-width: 1440px) {
  .store-content {
    max-width: 1000px;
  }
  
  .store-hero {
    grid-template-columns: 3fr 2fr;
  }
  
  .store-image-container {
    height: 400px;
  }
  
  .store-name {
    font-size: 2em;
  }
  
  .store-description-section {
    max-width: 80%;
  }
  
  .services-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .facilities-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .store-actions {
    max-width: 800px;
    left: 50%;
    transform: translateX(-50%);
  }
}

.store-image-carousel {
  position: relative;
  width: 100%;
  height: 280px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  margin-bottom: 16px;
}

.store-carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.store-carousel-item.active {
  opacity: 1;
}

.store-carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.store-carousel-item.active img:hover {
  transform: scale(1.02);
}

.store-carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.7;
  transition: all 0.3s ease;
  z-index: 5;
}

.store-carousel-control:hover {
  background: rgba(0, 0, 0, 0.9);
  opacity: 1;
  transform: translateY(-50%) scale(1.1);
}

.store-carousel-control.prev {
  left: 16px;
}

.store-carousel-control.next {
  right: 16px;
}

.store-carousel-pagination {
  position: absolute;
  bottom: 16px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.store-pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
}

.store-pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
}

@media (min-width: 768px) {
  .store-image-carousel {
    height: 350px;
    border-radius: 20px;
  }
  
  .store-carousel-control {
    width: 48px;
    height: 48px;
    font-size: 20px;
  }
}

@media (min-width: 1024px) {
  .store-image-carousel {
    height: 400px;
  }
}

    .services-container {
      display: grid;
      gap: 20px;
    }
    
    .product-item {
      font-family: poppins;
      display: flex;
      background: #fff;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      transition: transform 0.3s ease;
    }
    
    .product-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 16px rgba(0,0,0,0.15);
    }

     .fare-item {
      font-family: poppins;
      display: flex;
      background: #FFD700;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      transition: transform 0.3s ease;
    }

     .fare-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 16px rgba(0,0,0,0.15);
      background: #FFD700;
    }
    
    .product-image, .fare-image {
      width: 150px;
      height: 150px;
      flex-shrink: 0;
    }
    
    .product-image img, .fare-image img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
    
    .product-details, .fare-details {
      flex: 1;
      padding: 16px;
    }
    
    .product-details h4, .fare-details h4 {
      margin: 0 0 8px;
      color: #000;
      font-size: 1.1em;
    }
    
    .product-description, .fare-description {
      color: #000;
      font-size: 0.9em;
      margin: 0 0 12px;
    }
    
    .product-pricing {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 8px;
    }
    
    .product-price {
      font-weight: bold;
      color: #2e7d32;
      font-size: 1.1em;
    }
    
    .product-mrp {
      color: #999;
      font-size: 0.9em;
    }
    
    .product-meta {
      display: flex;
      gap: 15px;
      margin-bottom: 12px;
      font-size: 0.85em;
    }
    
    .product-quantity {
      color: #555;
    }
    
    .product-stock.in-stock {
      color: #2e7d32;
    }
    
    .product-stock.out-of-stock {
      color: #c62828;
    }
    
    .variants-container {
      margin-bottom: 12px;
    }
    
    .variant-select {
      width: 100%;
      padding: 8px;
      border-radius: 8px;
      border: 1px solid #ddd;
      background: #f9f9f9;
    }
    
    .product-actions, .fare-actions {
      display: flex;
      gap: 10px;
    }
    
    .add-to-cart, .buy-now, .order-now, .book-service {
      font-family: poppins;
      flex: 1;
      padding: 8px 12px;
      border: none;
      border-radius: 8px;
      font-weight: 500;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
      transition: all 0.2s ease;
    }
    
    .add-to-cart {
      background: #f5f5f5;
      color: #333;
    }
    
    .add-to-cart:hover {
      background: #e0e0e0;
    }
    
    .buy-now, .book-service {
      background: #ffd700;
      color: #000;
    }
    
    .buy-now:hover, .book-service:hover {
      background: #ffc400;
    }
    
     .order-now {
      font-family: poppins;
      background: #ed2939;
      color: #fff;
    }
    
     .order-now:hover {
      background: #4169E1	;
    }

    .fare-price {
      width: 80px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
      font-size: 1.2em;
      background: #f5f5f5;
      color: #333;
    }
    
/* Cart Overlay Styles */
.cart-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 3000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.cart-overlay.active {
  opacity: 1;
  visibility: visible;
}

.cart-container {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 500px;
  max-height: 80vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.cart-header {
  padding: 20px;
  border-bottom: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cart-header h3 {
  margin: 0;
  font-size: 1.3em;
  color: #333;
}

.close-cart {
  background: none;
  border: none;
  font-size: 1.2em;
  color: #666;
  cursor: pointer;
  padding: 5px;
}

.cart-items {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
}

.empty-cart {
  text-align: center;
  padding: 30px 0;
}

.empty-cart i {
  font-size: 3em;
  color: #ccc;
  margin-bottom: 15px;
}

.empty-cart p {
  margin: 10px 0 20px;
  color: #666;
}

.empty-cart button {
  background: var(--primary-color);
  color: #000;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
}

.cart-item {
  display: flex;
  padding: 15px 0;
  border-bottom: 1px solid #f5f5f5;
  position: relative;
}

.cart-item-image {
  width: 80px;
  height: 80px;
  flex-shrink: 0;
  margin-right: 15px;
}

.cart-item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
}

.cart-item-details {
  flex: 1;
}

.cart-item-details h4 {
  margin: 0 0 5px;
  font-size: 1em;
  color: #333;
}

.cart-item-details .variant {
  margin: 0 0 8px;
  font-size: 0.85em;
  color: #666;
}

.cart-item-price {
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.cart-item-quantity {
    color: #333;
  display: flex;
  align-items: center;
  gap: 10px;
}

.quantity-btn {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  border: 1px solid #ddd;
  background: #fff;
  font-size: 1em;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.quantity-btn:hover {
  background: #f5f5f5;
}

.quantity {
  min-width: 20px;
  text-align: center;
}

.remove-item {
  position: absolute;
  top: 15px;
  right: 0;
  background: none;
  border: none;
  color: #999;
  cursor: pointer;
  font-size: 1em;
}

.remove-item:hover {
  color: #ff5252;
}

.cart-summary {
  padding: 20px;
  border-top: 1px solid #eee;
}

.cart-total {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  font-size: 1.1em;
  font-weight: bold;
}

.cart-actions {
  display: flex;
  gap: 10px;
}

.clear-cart, .checkout-btn {
  flex: 1;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.clear-cart {
  background: #f5f5f5;
  color: #333;
  border: none;
}

.clear-cart:hover {
  background: #e0e0e0;
}

.checkout-btn {
  background: var(--primary-color);
  color: #000;
  border: none;
}

.checkout-btn:hover {
  background: #ffc400;
}

      .cart-button { 
          display: flex; 
          align-items: center; 
          justify-content: center; 
          padding: 15px; 
          background: none; 
          border: none; 
          color: var(--primary-color); 
          cursor: pointer; 
          transition: background-color 0.3s ease; 
          border-radius: 4px; 
          width: 40px; 
          height: 40px; 
      }
      
      .cart-button:hover { 
          background: rgba(255, 215, 0, 0.1); 
      }
      
      .cart-button i { 
          font-size: 18px; 
      }

/* Order Form Styles */
.order-form-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 3000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.order-form-overlay.active {
  opacity: 1;
  visibility: visible;
}

.order-form-container {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 500px;
  max-height: 90vh;
  overflow-y: auto;
  padding: 24px;
  position: relative;
}

.order-form-container h3 {
  margin: 0 0 20px;
  text-align: center;
  color: #333;
}

.close-order-form {
  background: none;
  border: none;
  font-size: 1.2em;
  color: #666;
  cursor: pointer;
  padding: 5px;
}

.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #333;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1em;
}

.form-group textarea {
  resize: vertical;
  min-height: 80px;
}

.coupon-input {
  display: flex;
  gap: 10px;
}

.coupon-input input {
  flex: 1;
}

.apply-coupon {
  background: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 0 15px;
  cursor: pointer;
}

.apply-coupon:hover {
  background: #e0e0e0;
}

.coupon-message {
  margin-top: 8px;
  font-size: 0.9em;
}

.coupon-message.success {
  color: #2e7d32;
}

.coupon-message.error {
  color: #c62828;
}

.order-summary {
  margin: 20px 0;
  padding: 16px;
  background: #f9f9f9;
  border-radius: 8px;
}

.order-summary h4 {
  margin: 0 0 12px;
  color: #333;
}

.order-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  padding-bottom: 8px;
  border-bottom: 1px solid #eee;
  color: #333;
}

.order-total {
  display: flex;
  justify-content: space-between;
  margin-top: 12px;
  font-weight: bold;
  font-size: 1.1em;
  color: #333;
}

.submit-order {
  width: 100%;
  padding: 14px;
  background: var(--primary-color);
  color: #000;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  font-size: 1em;
  cursor: pointer;
  transition: background 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.submit-order:hover {
  background: #ffc400;
}

/* Confirmation Modal Styles */
.confirmation-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 4000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.confirmation-modal.active {
  opacity: 1;
  visibility: visible;
}

.confirmation-content {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 400px;
  padding: 30px;
  text-align: center;
}

.confirmation-icon {
  font-size: 4em;
  color: #4caf50;
  margin-bottom: 20px;
}

.confirmation-content h3 {
  margin: 0 0 15px;
  color: #333;
}

.confirmation-content p {
  margin: 0 0 25px;
  color: #666;
  line-height: 1.5;
}

.confirmation-actions {
  display: flex;
  gap: 10px;
}

.view-receipt, .close-confirmation {
  flex: 1;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.view-receipt {
  background: #4caf50;
  color: #fff;
  border: none;
}

.view-receipt:hover {
  background: #3d8b40;
}

.close-confirmation {
  background: #f5f5f5;
  color: #333;
  border: none;
}

.close-confirmation:hover {
  background: #e0e0e0;
}

/* Responsive Styles */
@media (max-width: 480px) {
  .cart-container {
    width: 95%;
  }
  
  .cart-item {
    flex-direction: column;
  }
  
  .cart-item-image {
    width: 100%;
    height: 150px;
    margin-right: 0;
    margin-bottom: 10px;
  }
  
  .cart-actions {
    flex-direction: column;
  }
  
  .order-form-container {
    padding: 16px;
  }
  
  .confirmation-actions {
    flex-direction: column;
  }
}
    
    /* Toast notification */
    .toast {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      background: #333;
      color: #fff;
      padding: 12px 24px;
      border-radius: 8px;
      z-index: 4000;
      opacity: 0;
      visibility: hidden;
      transition: all 0.3s ease;
    }
    
    .toast.show {
      opacity: 1;
      visibility: visible;
    }
    
    @media (max-width: 768px) {
      .product-item, .fare-item {
        flex-direction: column;
      }
      
      .product-image, .fare-image {
        width: 100%;
        height: 180px;
      }
      
      .fare-price {
        width: 100%;
        padding: 12px;
      }
    }

  `;
  document.head.appendChild(style);
}

function showToast(message) {
  let toast = document.querySelector('.toast');
  if (!toast) {
    toast = document.createElement('div');
    toast.className = 'toast';
    document.body.appendChild(toast);
  }
  
  toast.textContent = message;
  toast.classList.add('show');
  
  setTimeout(() => {
    toast.classList.remove('show');
  }, 3000);
}

function updatePrice(selectElement) {
  const price = selectElement.value;
  const itemElement = selectElement.closest('[data-price]');
  if (itemElement) {
    itemElement.dataset.price = price;
    const priceDisplay = itemElement.querySelector('.product-price, .fare-price');
    if (priceDisplay) {
      priceDisplay.textContent = `₹${price}`;
    }
  }
}

function initializeTabs() {
  // You can add tab functionality if needed for different service sections
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function getFacilityIcon(facilityName) {
  const icons = {
    'Wide Selection': 'shopping-basket',
    'Great Prices': 'tags',
    'Clean Stores': 'broom',
    'Vegetarian Options': 'leaf',
    'Family Friendly': 'users',
    'Late Night': 'moon',
    'Expert Advice': 'user-tie',
    'Installation': 'tools',
    'EMI Options': 'credit-card',
    'Parking': 'parking',
    'AC Shopping': 'snowflake',
    'Wheelchair Access': 'wheelchair',
    'Drive-Thru': 'car',
    'Free WiFi': 'wifi'
  };
  
  return icons[facilityName] || 'check-circle';
}

function filterLocators(category) {
  const locatorsGrid = document.getElementById('locatorsGrid');
  let filteredData = locatorsData;

  if (category !== 'All') {
    filteredData = filteredData.filter(locator => locator.category === category);
  }

  locatorsGrid.innerHTML = renderLocatorCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function sortLocators(criteria) {
  const locatorsGrid = document.getElementById('locatorsGrid');
  let sortedData = [...locatorsData];

  switch(criteria) {
    case 'rating':
      sortedData.sort((a, b) => parseFloat(b.details.rating) - parseFloat(a.details.rating));
      break;
    case 'name':
      sortedData.sort((a, b) => a.details.name.localeCompare(b.details.name));
      break;
    case 'distance':
      // This would require location data to be implemented
      break;
  }

  locatorsGrid.innerHTML = renderLocatorCards(sortedData);
  toggleLocatorSortOptions();
}

function toggleLocatorSortOptions() {
  const sortOptions = document.getElementById('locatorSortOptionsContainer');
  sortOptions.classList.toggle('active');
}

function hideLocatorsOverlay() {
  const overlay = document.getElementById('locatorsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
    }, 300);
  }
}

function shareStore(storeName) {
  const store = locatorsData.find(loc => loc.details.name === storeName);
  if (!store) return;

  if (navigator.share) {
    navigator.share({
      title: store.details.name,
      text: `Check out ${store.details.name} - ${store.details.description}`,
      url: store.details.website || window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    const shareUrl = store.details.website || window.location.href;
    const shareText = `Check out ${store.details.name}: ${shareUrl}`;
    prompt('Copy this link to share:', shareText);
  }
}

function openFullScreen(imageUrl) {
  event.stopPropagation();
  const fullScreenDiv = document.createElement('div');
  fullScreenDiv.className = 'full-screen-image';
  fullScreenDiv.innerHTML = `
    <img src="${imageUrl}" alt="Full screen">
    <button class="close-full-screen" onclick="document.querySelector('.full-screen-image').remove()">
      <i class="fas fa-times"></i>
    </button>
  `;
  document.body.appendChild(fullScreenDiv);
  
  setTimeout(() => {
    fullScreenDiv.classList.add('active');
    const img = fullScreenDiv.querySelector('img');
    if (img) {
      img.style.opacity = '1';
      img.style.transform = 'scale(1)';
    }
  }, 10);
}

function changeCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentSlide(index) {
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

// Add store carousel functionality
function initializeStoreCarousel() {
  const carousel = document.querySelector('.store-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.store-carousel-item');
  const dots = carousel.querySelectorAll('.store-pagination-dot');
  let currentIndex = 0;

  function showSlide(index) {
    items.forEach((item, i) => {
      item.classList.remove('active');
      dots[i].classList.remove('active');
      if (i === index) {
        item.classList.add('active');
        dots[i].classList.add('active');
      }
    });
    currentIndex = index;
  }

  function nextSlide() {
    const newIndex = (currentIndex + 1) % items.length;
    showSlide(newIndex);
  }

  function prevSlide() {
    const newIndex = (currentIndex - 1 + items.length) % items.length;
    showSlide(newIndex);
  }

  // Auto-rotate every 5 seconds
  let interval = setInterval(nextSlide, 5000);

  // Pause on hover
  carousel.addEventListener('mouseenter', () => {
    clearInterval(interval);
  });

  carousel.addEventListener('mouseleave', () => {
    interval = setInterval(nextSlide, 5000);
  });
}

function changeStoreCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.store-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.store-carousel-item');
  const dots = carousel.querySelectorAll('.store-pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentStoreSlide(index) {
  const carousel = event.target.closest('.store-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.store-carousel-item');
  const dots = carousel.querySelectorAll('.store-pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

function copyToClipboard(text) {
  event.stopPropagation();
  navigator.clipboard.writeText(text).then(() => {
    const button = event.target.closest('button');
    if (button) {
      const originalText = button.textContent;
      button.textContent = 'Copied!';
      setTimeout(() => {
        button.textContent = originalText;
      }, 2000);
    }
  }).catch(err => {
    console.error('Failed to copy text: ', err);
  });
}

function showChatSupport(storeName) {
  event.stopPropagation();
  alert(`Chat support for ${storeName} would open here in a real implementation.`);
}

const publicPlacesData = [
  // Delhi - Park
  {
    category: "Park",
    metropolis: "Delhi",
    details: {
      name: "Lodhi Garden",
      cardImages: [
        "https://www.tripsavvy.com/thmb/IfH8eeAQCSljDTMWfMK8JWv45OE=/960x0/filters:no_upscale():max_bytes(150000):strip_icc()/GettyImages-180344996-a67b6e54b64f450ab849d0a1fb15d99c.jpg",
        "https://www.gosahin.com/go/p/h/1564857104_lodhi-garden-delhi3.jpg"
      ],
      placeImages: [
        "https://www.tripsavvy.com/thmb/IfH8eeAQCSljDTMWfMK8JWv45OE=/960x0/filters:no_upscale():max_bytes(150000):strip_icc()/GettyImages-180344996-a67b6e54b64f450ab849d0a1fb15d99c.jpg",
        "https://www.gosahin.com/go/p/h/1564857104_lodhi-garden-delhi3.jpg",
        "https://www.delhitourism.gov.in/dttdc/explore/lodhi_garden_1.jpg",
        "https://www.delhitourism.gov.in/dttdc/explore/lodhi_garden_2.jpg"
      ],
      description: "A serene park with historical monuments and lush greenery.",
      address: "Lodhi Road, Delhi",
      timings: "Mon-Sun: 9 AM - 10 PM",
      entryFee: "Free",
      mapLink: "https://maps.example.com/lodhi-garden",
      website: "https://www.delhitourism.gov.in/lodhi-garden",
      social: {
        facebook: "https://facebook.com/lodhigarden",
        twitter: "https://twitter.com/lodhigarden",
        instagram: "https://instagram.com/lodhigarden"
      },
      facilities: [
        { name: "Walking Trails", description: "Well-maintained walking paths" },
        { name: "Picnic Spots", description: "Designated areas for picnics" }
      ],
      events: [
        { name: "Yoga Sessions", date: "Every Sunday, 7 AM" }
      ],
      support: {
        email: "info@lodhigarden.com",
        phone: "+91-1234567890"
      },
      rating: "4.5",
      reviews: "1200+",
      established: "1936",
      tags: ["Historical", "Nature", "Free Entry"],
      categoryInsights: {
        activities: ["Morning Walks", "Bird Watching", "Photography"],
        historicalMonuments: ["Mohammed Shah's Tomb", "Sikander Lodi's Tomb"],
        bestTimeToVisit: "6-8 AM for walks, 4-6 PM for photography",
        parkingAvailability: "Available (₹50 for 2 hours)",
        guidedTours: "Heritage walks every Saturday at 8 AM",
        accessibility: "Wheelchair accessible pathways",
        petPolicy: "Allowed on leash",
        notableFeatures: ["Rose Garden", "Bonsai Section", "Herb Garden"]
      }
    }
  },
  
  // Delhi - School
  {
    category: "School",
    metropolis: "Delhi",
    details: {
      name: "Delhi Public School, RK Puram",
      cardImages: [
        "https://example.com/dps-rkp1.jpg",
        "https://example.com/dps-rkp2.jpg"
      ],
      placeImages: [
        "https://example.com/dps-rkp1.jpg",
        "https://example.com/dps-rkp2.jpg",
        "https://example.com/dps-rkp3.jpg",
        "https://example.com/dps-rkp4.jpg"
      ],
      description: "Premier educational institution with excellent academic record.",
      address: "Dr. S. Radhakrishnan Marg, RK Puram, Delhi",
      timings: "Mon-Fri: 8 AM - 2 PM",
      entryFee: "N/A",
      mapLink: "https://maps.example.com/dps-rkp",
      website: "https://www.dpsrkp.net",
      social: {
        facebook: "https://facebook.com/dpsrkp",
        twitter: "https://twitter.com/dpsrkp",
        instagram: "https://instagram.com/dpsrkp"
      },
      facilities: [
        { name: "Library", description: "Extensive collection of books" },
        { name: "Sports Complex", description: "Cricket, football, basketball facilities" }
      ],
      events: [
        { name: "Annual Day", date: "15th December, 2023" }
      ],
      support: {
        email: "info@dpsrkp.net",
        phone: "+91-1126185888"
      },
      rating: "4.7",
      reviews: "500+",
      established: "1972",
      tags: ["Education", "CBSE", "Premium School"],
      categoryInsights: {
        admissionStatus: "Open (Till 31st March 2024)",
        feeStructure: {
          nursery: "₹85,000 annually",
          primary: "₹95,000 annually",
          secondary: "₹1,10,000 annually"
        },
        admissionForm: "https://www.dpsrkp.net/admissions/form.pdf",
        curriculum: "CBSE",
        classesOffered: "Nursery to Class 12",
        entranceExam: "Entrance test for Grade 1 onwards",
        facilities: ["Smart Classrooms", "Science Labs", "Library", "Sports Complex"],
        faculty: "150+ teachers with 70% postgraduates",
        scholarships: "Merit-based scholarships available",
        extracurricular: ["Annual Sports Day", "Science Fair", "Music Competition"],
        results: "100% pass rate in 2023 board exams",
        transport: "School buses available across Delhi",
        uniform: "Available at school store (₹2,500 complete set)"
      }
    }
  },

  // Delhi - Hospital
  {
    category: "Hospital",
    metropolis: "Delhi",
    details: {
      name: "AIIMS",
      cardImages: [
        "https://example.com/aiims1.jpg",
        "https://example.com/aiims2.jpg"
      ],
      placeImages: [
        "https://example.com/aiims1.jpg",
        "https://example.com/aiims2.jpg",
        "https://example.com/aiims3.jpg",
        "https://example.com/aiims4.jpg"
      ],
      description: "Premier medical institution and hospital in India.",
      address: "Ansari Nagar, Delhi",
      timings: "24/7",
      entryFee: "Free for OPD (charges may apply for treatments)",
      mapLink: "https://maps.example.com/aiims-delhi",
      website: "https://www.aiims.edu",
      social: {
        facebook: "https://facebook.com/aiims",
        twitter: "https://twitter.com/aiims",
        instagram: "https://instagram.com/aiims"
      },
      facilities: [
        { name: "Emergency Services", description: "24/7 emergency care" },
        { name: "Specialty Clinics", description: "All major specialties available" }
      ],
      events: [
        { name: "Health Camp", date: "Quarterly" }
      ],
      support: {
        email: "info@aiims.edu",
        phone: "+91-1126589111"
      },
      rating: "4.6",
      reviews: "5000+",
      established: "1956",
      tags: ["Healthcare", "Government", "Multi-specialty"],
      categoryInsights: {
        departments: ["Cardiology", "Neurology", "Oncology", "Pediatrics", "Orthopedics"],
        consultationCharges: {
          opd: "₹100 (General), ₹500 (Specialist)",
          ipd: "₹1,000 per day (General Ward)"
        },
        appointment: "Online booking available at portal.aiims.edu",
        emergency: "24/7 emergency services with ambulance",
        healthPackages: [
          "Full Body Checkup: ₹5,000",
          "Cardiac Screening: ₹7,500",
          "Diabetes Profile: ₹3,200"
        ],
        insurance: ["All major insurers accepted"],
        doctors: "500+ specialists on staff",
        bedAvailability: {
          general: "250 beds",
          icu: "50 beds",
          ventilator: "30 units"
        },
        pharmacy: "24/7 pharmacy on premises",
        diagnosticServices: ["MRI", "CT Scan", "Ultrasound", "X-Ray"],
        visitingHours: "4 PM - 6 PM daily"
      }
    }
  },

  // Delhi - Gym
  {
    category: "Gym",
    metropolis: "Delhi",
    details: {
      name: "Talwalkars Fitness Center",
      cardImages: [
        "https://example.com/talwalkars1.jpg",
        "https://example.com/talwalkars2.jpg"
      ],
      placeImages: [
        "https://example.com/talwalkars1.jpg",
        "https://example.com/talwalkars2.jpg",
        "https://example.com/talwalkars3.jpg",
        "https://example.com/talwalkars4.jpg"
      ],
      description: "Premium fitness center with modern equipment and trained trainers.",
      address: "Hauz Khas, Delhi",
      timings: "Mon-Sun: 6 AM - 10 PM",
      entryFee: "Membership required",
      mapLink: "https://maps.example.com/talwalkars-hk",
      website: "https://www.talwalkars.net",
      social: {
        facebook: "https://facebook.com/talwalkars",
        twitter: "https://twitter.com/talwalkars",
        instagram: "https://instagram.com/talwalkars"
      },
      facilities: [
        { name: "Cardio Section", description: "Treadmills, ellipticals, etc." },
        { name: "Weight Training", description: "Free weights and machines" }
      ],
      events: [
        { name: "Fitness Challenge", date: "Monthly" }
      ],
      support: {
        email: "hauzkhas@talwalkars.net",
        phone: "+91-1126567890"
      },
      rating: "4.5",
      reviews: "1200+",
      established: "2010",
      tags: ["Fitness", "Health", "Premium Gym"],
      categoryInsights: {
        membershipPlans: {
          basic: "₹2,500/month",
          premium: "₹4,000/month",
          annual: "₹35,000/year",
          couple: "₹6,000/month"
        },
        trainers: [
          "Rahul Sharma (Certified Personal Trainer - 10 years experience)",
          "Priya Patel (Yoga Specialist - RYT 500 certified)"
        ],
        classSchedule: {
          zumba: "Mon, Wed, Fri - 7 AM & 6 PM",
          yoga: "Tue, Thu, Sat - 8 AM & 7 PM",
          kickboxing: "Mon, Thu - 5 PM",
          pilates: "Wed, Fri - 9 AM"
        },
        trialSession: "1 free trial session available",
        equipment: "Latest cardio and strength machines from Technogym",
        personalTraining: "₹1,500/session (package discounts available)",
        nutritionConsultation: "Available with certified dietician (₹800/session)",
        lockerFacility: "Available (bring your own lock)",
        towelService: "Provided for premium members",
        cancellationPolicy: "30-day notice for membership cancellation"
      }
    }
  },

  // Delhi - Wedding Venue
  {
    category: "Wedding Venue",
    metropolis: "Delhi",
    details: {
      name: "The Ashok Hotel",
      cardImages: [
        "https://example.com/ashok1.jpg",
        "https://example.com/ashok2.jpg"
      ],
      placeImages: [
        "https://example.com/ashok1.jpg",
        "https://example.com/ashok2.jpg",
        "https://example.com/ashok3.jpg",
        "https://example.com/ashok4.jpg"
      ],
      description: "Luxurious heritage hotel with grand wedding venues.",
      address: "50-B, Chanakyapuri, Delhi",
      timings: "24/7",
      entryFee: "Booking required",
      mapLink: "https://maps.example.com/ashok-hotel",
      website: "https://www.theashok.com",
      social: {
        facebook: "https://facebook.com/theashok",
        twitter: "https://twitter.com/theashok",
        instagram: "https://instagram.com/theashok"
      },
      facilities: [
        { name: "Banquet Halls", description: "Multiple sizes available" },
        { name: "Catering", description: "In-house catering services" }
      ],
      events: [
        { name: "Wedding Expo", date: "Annually in November" }
      ],
      support: {
        email: "events@theashok.com",
        phone: "+91-1126110101"
      },
      rating: "4.4",
      reviews: "800+",
      established: "1956",
      tags: ["Luxury", "Heritage", "Grand Weddings"],
      categoryInsights: {
        venueCapacity: {
          "Grand Ballroom": "500 guests",
          "Garden Lawn": "1000 guests",
          "Royal Suite": "150 guests"
        },
        packageRates: {
          basic: "₹2,500 per plate (min. 200 guests)",
          premium: "₹3,500 per plate (min. 300 guests)",
          royal: "₹5,000 per plate (min. 150 guests)"
        },
        bookingPolicy: "50% advance to block dates",
        decorationOptions: ["Floral", "Theme-based", "Traditional"],
        cateringOptions: ["Vegetarian", "Non-vegetarian", "Jain", "Continental"],
        accommodation: "200 rooms available for wedding guests",
        parking: "Valet parking for 200 cars",
        cancellationPolicy: "90 days for full refund",
        preferredVendors: {
          photographers: ["Wedding Moments", "Frame by Frame"],
          makeupArtists: ["Glam Squad", "Bridal Beauty"]
        },
        soundPolicy: "Music must end by 11 PM",
        weddingPlanner: "Dedicated planner assigned for each booking"
      }
    }
  },

  // Delhi - Movie Theater
  {
    category: "Movie Theater",
    metropolis: "Delhi",
    details: {
      name: "PVR Select Citywalk",
      cardImages: [
        "https://example.com/pvr1.jpg",
        "https://example.com/pvr2.jpg"
      ],
      placeImages: [
        "https://example.com/pvr1.jpg",
        "https://example.com/pvr2.jpg",
        "https://example.com/pvr3.jpg",
        "https://example.com/pvr4.jpg"
      ],
      description: "Premium multiplex with latest movies and comfortable seating.",
      address: "Select Citywalk Mall, Saket, Delhi",
      timings: "10 AM - 1 AM",
      entryFee: "Ticket prices vary",
      mapLink: "https://maps.example.com/pvr-saket",
      website: "https://www.pvrcinemas.com",
      social: {
        facebook: "https://facebook.com/pvrcinemas",
        twitter: "https://twitter.com/pvrcinemas",
        instagram: "https://instagram.com/pvrcinemas"
      },
      facilities: [
        { name: "Dolby Atmos", description: "Premium sound system" },
        { name: "Food Court", description: "Snacks and beverages available" }
      ],
      events: [
        { name: "Film Festival", date: "Annually in September" }
      ],
      support: {
        email: "customercare@pvrcinemas.com",
        phone: "+91-1124567890"
      },
      rating: "4.4",
      reviews: "5000+",
      established: "2007",
      tags: ["Movies", "Entertainment", "Premium"],
      categoryInsights: {
        currentMovies: [
          {
            title: "Avengers: Endgame",
            timings: ["10:30 AM", "2:00 PM", "6:30 PM", "10:00 PM"],
            screenType: "IMAX 3D"
          },
          {
            title: "The Lion King",
            timings: ["11:00 AM", "3:00 PM", "7:30 PM"],
            screenType: "4DX"
          }
        ],
        ticketPricing: {
          standard: "₹250-₹400",
          premium: "₹500-₹800",
          recliner: "₹1,200"
        },
        foodMenu: {
          combos: [
            "Popcorn + Pepsi: ₹300",
            "Burger + Fries + Coke: ₹450"
          ],
          alacarte: {
            "Popcorn (Large)": "₹200",
            "Nachos with Cheese": "₹250"
          }
        },
        screenTypes: ["IMAX", "4DX", "PXL", "Director's Cut"],
        seatMap: "Available on booking page",
        offers: [
          "Monday Magic: 20% off on all tickets",
          "Student Discount: 15% off with valid ID"
        ],
        parking: "Validated parking for 3 hours",
        accessibility: "Wheelchair accessible screens available",
        membership: "PVR Privilege Card offers 10% discount"
      }
    }
  },

  // Delhi - Hotel & Resort
  {
    category: "Hotel & Resort",
    metropolis: "Delhi",
    details: {
      name: "The Taj Mahal Hotel",
      cardImages: [
        "https://example.com/taj1.jpg",
        "https://example.com/taj2.jpg"
      ],
      placeImages: [
        "https://example.com/taj1.jpg",
        "https://example.com/taj2.jpg",
        "https://example.com/taj3.jpg",
        "https://example.com/taj4.jpg"
      ],
      description: "Iconic luxury hotel with world-class amenities.",
      address: "1, Mansingh Road, Delhi",
      timings: "24/7",
      entryFee: "Room rates apply",
      mapLink: "https://maps.example.com/taj-mahal-hotel",
      website: "https://www.tajhotels.com",
      social: {
        facebook: "https://facebook.com/tajhotels",
        twitter: "https://twitter.com/tajhotels",
        instagram: "https://instagram.com/tajhotels"
      },
      facilities: [
        { name: "Spa", description: "Luxury spa services" },
        { name: "Fine Dining", description: "Multiple restaurants" }
      ],
      events: [],
      support: {
        email: "reservations@tajhotels.com",
        phone: "+91-1123026162"
      },
      rating: "4.8",
      reviews: "2500+",
      established: "1978",
      tags: ["Luxury", "5-Star", "Iconic"],
      categoryInsights: {
        roomTypes: [
          {
            name: "Deluxe Room",
            price: "₹15,000/night",
            capacity: "2 adults"
          },
          {
            name: "Executive Suite",
            price: "₹25,000/night",
            capacity: "2 adults + 1 child"
          },
          {
            name: "Presidential Suite",
            price: "₹1,00,000/night",
            capacity: "4 adults"
          }
        ],
        diningOptions: [
          "Indian (Bukhara - famous for dal makhani)",
          "Continental (The Grill Room)",
          "Chinese (House of Ming)"
        ],
        spaServices: [
          "Ayurvedic Massage (60 mins - ₹5,000)",
          "Swedish Massage (90 mins - ₹7,500)"
        ],
        bookingPolicy: "1 night deposit required",
        cancellationPolicy: "48 hours for full refund",
        checkInOut: "2 PM / 12 PM",
        amenities: [
          "24-hour room service",
          "Free WiFi",
          "Swimming pool",
          "Business center"
        ],
        nearbyAttractions: [
          "India Gate (1 km)",
          "Connaught Place (3 km)"
        ],
        transport: "Airport pickup available (₹2,500)",
        petPolicy: "Small pets allowed (₹2,000 cleaning fee)"
      }
    }
  },

  // Delhi - Shopping Mall
  {
    category: "Shopping Mall",
    metropolis: "Delhi",
    details: {
      name: "DLF Promenade",
      cardImages: [
        "https://example.com/promenade1.jpg",
        "https://example.com/promenade2.jpg"
      ],
      placeImages: [
        "https://example.com/promenade1.jpg",
        "https://example.com/promenade2.jpg",
        "https://example.com/promenade3.jpg",
        "https://example.com/promenade4.jpg"
      ],
      description: "Upscale shopping mall with international brands and dining options.",
      address: "Vasant Kunj, Delhi",
      timings: "11 AM - 10 PM",
      entryFee: "Free",
      mapLink: "https://maps.example.com/dlf-promenade",
      website: "https://www.dlfpromenade.com",
      social: {
        facebook: "https://facebook.com/dlfpromenade",
        twitter: "https://twitter.com/dlfpromenade",
        instagram: "https://instagram.com/dlfpromenade"
      },
      facilities: [
        { name: "Valet Parking", description: "Available at all entrances" },
        { name: "Kids Play Area", description: "Supervised play zone" }
      ],
      events: [
        { name: "Winter Sale", date: "15th December - 15th January" }
      ],
      support: {
        email: "info@dlfpromenade.com",
        phone: "+91-1123456789"
      },
      rating: "4.3",
      reviews: "3200+",
      established: "2008",
      tags: ["Shopping", "Luxury", "Dining"],
      categoryInsights: {
        storeDirectory: [
          {
            category: "Fashion",
            brands: ["Zara", "H&M", "Marks & Spencer"]
          },
          {
            category: "Electronics",
            brands: ["Apple Store", "Samsung", "Bose"]
          }
        ],
        foodCourt: [
          "McDonald's",
          "KFC",
          "Haldiram's",
          "Subway"
        ],
        fineDining: [
          "Olive Bar & Kitchen",
          "The Big Chill Cafe"
        ],
        floorPlan: {
          "LG Floor": "Supermarket & Parking",
          "Ground Floor": "Luxury Brands",
          "First Floor": "Fashion & Accessories",
          "Second Floor": "Electronics & Home",
          "Third Floor": "Food Court & Cinema"
        },
        ongoingOffers: [
          "10% off on first purchase with mall app",
          "Free parking for 4 hours with ₹5,000 purchase"
        ],
        eventsCalendar: [
          "Live Music: Every Friday 7 PM",
          "Kids Workshop: Every Sunday 11 AM"
        ],
        services: [
          "Wheelchair available at concierge",
          "Stroller rentals",
          "Gift wrapping"
        ],
        parkingRates: [
          "First 2 hours: Free",
          "Additional hours: ₹50 per hour"
        ],
        cinema: "PVR Cinemas - 6 screens",
        loyaltyProgram: "DLF Privilege Card - Earn points on purchases"
      }
    }
  },

  // Delhi - Museum
  {
    category: "Museum",
    metropolis: "Delhi",
    details: {
      name: "National Museum",
      cardImages: [
        "https://example.com/national-museum1.jpg",
        "https://example.com/national-museum2.jpg"
      ],
      placeImages: [
        "https://example.com/national-museum1.jpg",
        "https://example.com/national-museum2.jpg",
        "https://example.com/national-museum3.jpg",
        "https://example.com/national-museum4.jpg"
      ],
      description: "Explore India's rich history and cultural heritage.",
      address: "Janpath, Delhi",
      timings: "Mon-Sun: 7 AM - 11 PM",
      entryFee: "$5",
      mapLink: "https://maps.example.com/national-museum",
      website: "https://www.nationalmuseumindia.gov.in",
      social: {
        facebook: "https://facebook.com/nationalmuseum",
        twitter: "https://twitter.com/nationalmuseum",
        instagram: "https://instagram.com/nationalmuseum"
      },
      facilities: [
        { name: "Guided Tours", description: "Available in multiple languages" },
        { name: "Cafeteria", description: "Serves snacks and beverages" }
      ],
      events: [
        { name: "Art Exhibition", date: "15th October, 2023" }
      ],
      support: {
        email: "info@nationalmuseum.com",
        phone: "+91-9876543210"
      },
      rating: "4.3",
      reviews: "850+",
      established: "1949",
      tags: ["Historical", "Art", "Cultural"],
      categoryInsights: {
        collections: [
          "Harappan Civilization Gallery",
          "Buddhist Art Section",
          "Coin Gallery"
        ],
        guidedTourTimings: [
          "English: 10:30 AM, 2:30 PM",
          "Hindi: 11:30 AM, 3:30 PM"
        ],
        ticketPricing: {
          indianAdults: "₹20",
          foreignTourists: "₹650",
          children: "Free (below 12 years)",
          cameraFee: "₹300 (no flash allowed)"
        },
        currentExhibitions: [
          "Mughal Miniature Paintings (Till 30th Nov)",
          "Ancient Indian Textiles (Till 15th Dec)"
        ],
        accessibility: [
          "Wheelchair accessible",
          "Braille guides available"
        ],
        photographyPolicy: "Allowed without flash (₹300 fee)",
        nearbyAttractions: [
          "India Gate (1 km)",
          "Connaught Place (2 km)"
        ],
        library: "Reference library open to researchers",
        museumShop: "Sells replicas and books",
        averageVisitDuration: "2-3 hours"
      }
    }
  },

    // Mumbai - Shopping Mall
    {
    category: "Shopping Mall",
    metropolis: "Mumbai",
    details: {
      name: "Phoenix Marketcity",
      cardImages: [
        "https://example.com/phoenix-mumbai1.jpg",
        "https://example.com/phoenix-mumbai2.jpg"
      ],
      placeImages: [
        "https://example.com/phoenix-mumbai1.jpg",
        "https://example.com/phoenix-mumbai2.jpg",
        "https://example.com/phoenix-mumbai3.jpg",
        "https://example.com/phoenix-mumbai4.jpg"
      ],
      description: "One of India's largest malls spanning 1.3 million sq ft with over 600 brands, entertainment zones, and gourmet dining options.",
      address: "LBS Marg, Kurla West, Mumbai - 400070",
      timings: "Mon-Sun: 11 AM - 10 PM",
      entryFee: "Free",
      mapLink: "https://maps.example.com/phoenix-mumbai",
      website: "https://www.phoenixmarketcity.com/mumbai",
      social: {
        facebook: "https://facebook.com/PhoenixMarketcityMumbai",
        twitter: "https://twitter.com/PhoenixMCMumbai",
        instagram: "https://instagram.com/phoenixmarketcitymum"
      },
      facilities: [
        { name: "Valet Parking", description: "Available at all entrances (₹200 for 4 hours)" },
        { name: "Kids Play Zone", description: "Hamleys Play Area with supervised activities" },
        { name: "VIP Lounges", description: "Exclusive lounges for premium shoppers" }
      ],
      events: [
        { name: "Winter Carnival", date: "15th Dec - 15th Jan" },
        { name: "Food Festival", date: "First week of every month" }
      ],
      support: {
        email: "customercare.mum@phoenixmarketcity.com",
        phone: "+91-2245678901"
      },
      rating: "4.5",
      reviews: "4500+",
      established: "2011",
      tags: ["Luxury Shopping", "Entertainment", "Fine Dining"],
      categoryInsights: {
        storeDirectory: {
          luxuryBrands: ["Louis Vuitton", "Gucci", "Tiffany & Co."],
          highStreetFashion: ["Zara", "H&M", "Forever 21"],
          electronics: ["Apple Store", "Samsung", "Bose"],
          homeDecor: ["Pottery Barn", "Westside Home"]
        },
        entertainment: [
          "PVR ICON: 14-screen multiplex with IMAX",
          "Smaaash: Virtual reality gaming arena",
          "Bowling Co.: Premium bowling alley"
        ],
        diningOptions: {
          foodCourt: ["McDonald's", "KFC", "Burger King"],
          fineDining: ["The Black Pearl", "Indigo Deli", "Shiro"],
          cafes: ["Starbucks", "Theobroma", "Blue Tokai"]
        },
        parking: {
          rates: "₹50 per hour (first 2 hours free with purchase above ₹3000)",
          capacity: "3000 cars"
        },
        services: [
          "Personal Shopper Assistance",
          "Wheelchair Access (available at all gates)",
          "Nursing Rooms"
        ],
        mallLayout: {
          floors: 5,
          area: "1.3 million sq ft",
          floorPlan: "Available on mobile app"
        },
        loyaltyProgram: {
          name: "Phoenix Prime",
          benefits: "10% discount at participating stores, free parking"
        }
      }
    }
  },

  // Bhopal - Shopping Mall
  {
    category: "Shopping Mall",
    metropolis: "Bhopal",
    details: {
      name: "DB City Mall",
      cardImages: [
        "https://example.com/db-mall1.jpg",
        "https://example.com/db-mall2.jpg"
      ],
      placeImages: [
        "https://example.com/db-mall1.jpg",
        "https://example.com/db-mall2.jpg",
        "https://example.com/db-mall3.jpg",
        "https://example.com/db-mall4.jpg"
      ],
      description: "Central India's largest shopping destination spread across 1 million sq ft with 200+ national and international brands.",
      address: "DB City, Arera Hills, Bhopal - 462011",
      timings: "Mon-Sun: 10:30 AM - 9:30 PM",
      entryFee: "Free",
      mapLink: "https://maps.example.com/db-city-mall",
      website: "https://www.dbcitymall.com",
      social: {
        facebook: "https://facebook.com/DBCityMall",
        instagram: "https://instagram.com/dbcitymall"
      },
      facilities: [
        { name: "Family Lounge", description: "Dedicated space for families with kids" },
        { name: "Prayer Room", description: "Separate spaces for all religions" },
        { name: "ATM Zone", description: "All major bank ATMs available" }
      ],
      events: [
        { name: "Cultural Fest", date: "During Navratri" },
        { name: "Kids Summer Camp", date: "April-May annually" }
      ],
      support: {
        email: "info@dbcitymall.com",
        phone: "+91-7554001234"
      },
      rating: "4.3",
      reviews: "1800+",
      established: "2013",
      tags: ["Family Shopping", "Entertainment", "Regional Hub"],
      categoryInsights: {
        anchorStores: ["Shoppers Stop", "Pantaloons", "Westside"],
        entertainment: [
          "Inox Multiplex (6 screens)",
          "Fun City (Amusement arcade)",
          "Bowling Alley"
        ],
        dining: {
          foodCourt: ["Haldiram's", "Domino's", "Subway"],
          restaurants: ["Mainland China", "Barbeque Nation", "The Chocolate Room"]
        },
        parkingFacility: {
          capacity: "1500 vehicles",
          rates: "₹20 first hour, ₹10 subsequent hours"
        },
        accessibility: [
          "Wheelchair accessible all floors",
          "Special parking for disabled"
        ],
        mallFeatures: [
          "Central atrium with musical fountain",
          "Open-air amphitheater for events",
          "Free WiFi throughout"
        ],
        localSpecialties: [
          "MP Handicrafts section",
          "Chanderi & Maheshwari saree stores"
        ]
      }
    }
  },

  // Mumbai - School
  {
    category: "School",
    metropolis: "Mumbai",
    details: {
      name: "Dhirubhai Ambani International School",
      cardImages: [
        "https://example.com/dhirubhai-school1.jpg",
        "https://example.com/dhirubhai-school2.jpg"
      ],
      placeImages: [
        "https://example.com/dhirubhai-school1.jpg",
        "https://example.com/dhirubhai-school2.jpg",
        "https://example.com/dhirubhai-school3.jpg",
        "https://example.com/dhirubhai-school4.jpg"
      ],
      description: "IB World School offering PYP, MYP and DP programmes with state-of-the-art infrastructure and global faculty.",
      address: "Bandra Kurla Complex, G Block, Bandra East, Mumbai - 400051",
      timings: "Mon-Fri: 8 AM - 3 PM, Sat: 8 AM - 1 PM",
      entryFee: "N/A",
      mapLink: "https://maps.example.com/dhirubhai-school",
      website: "https://www.dais.edu.in",
      social: {
        facebook: "https://facebook.com/DAISchool",
        linkedin: "https://linkedin.com/school/dhirubhai-ambani-international-school"
      },
      facilities: [
        { name: "STEM Labs", description: "Advanced robotics and AI labs" },
        { name: "Performing Arts Center", description: "500-seat auditorium with Broadway-grade facilities" },
        { name: "Olympic-size Pool", description: "FINA standard swimming pool" }
      ],
      events: [
        { name: "Global Education Summit", date: "Annually in November" },
        { name: "Tech Fest", date: "February 2024" }
      ],
      support: {
        email: "admissions@dais.edu.in",
        phone: "+91-2226581234"
      },
      rating: "4.8",
      reviews: "350+",
      established: "2003",
      tags: ["IB School", "Premium", "Global Education"],
      categoryInsights: {
        academics: {
          curriculum: "International Baccalaureate (PYP, MYP, DP)",
          studentTeacherRatio: "8:1",
          foreignLanguages: ["French", "Spanish", "Mandarin"]
        },
        admissionProcess: {
          stages: ["Application", "Student Interaction", "Parent Interview"],
          deadlines: "November for following academic year"
        },
        feeStructure: {
          annualTuition: "₹8,50,000 - ₹10,00,000",
          inclusion: ["Textbooks", "Technology Fee", "Sports Facilities"]
        },
        infrastructure: [
          "Digital Classrooms with Smart Boards",
          "600-seat Auditorium",
          "Fully equipped Science Labs"
        ],
        extracurricular: {
          sports: ["Squash", "Golf", "Equestrian"],
          arts: ["Western Music", "Kathak", "Drama"],
          clubs: ["Model UN", "Debate", "Robotics"]
        },
        achievements: [
          "100% IBDP pass rate since inception",
          "Average IB score: 38/45 (2023)"
        ],
        alumniNetwork: "Active global alumni association",
        transport: {
          coverage: "Across Mumbai",
          fleet: "GPS-enabled AC buses"
        },
        safety: [
          "CCTV surveillance",
          "Professional security staff",
          "Regular safety drills"
        ]
      }
    }
  },

  // Bhopal - School
  {
    category: "School",
    metropolis: "Bhopal",
    details: {
      name: "The Sanskaar Valley School",
      cardImages: [
        "https://example.com/sanskaar-school1.jpg",
        "https://example.com/sanskaar-school2.jpg"
      ],
      placeImages: [
        "https://example.com/sanskaar-school1.jpg",
        "https://example.com/sanskaar-school2.jpg",
        "https://example.com/sanskaar-school3.jpg",
        "https://example.com/sanskaar-school4.jpg"
      ],
      description: "CBSE-affiliated residential school spread over 40 acres focusing on value-based education with modern amenities.",
      address: "Post: Ghonsla, Bhadbhada Road, Bhopal - 462030",
      timings: "Mon-Sat: 8 AM - 4 PM (Residential)",
      entryFee: "N/A",
      mapLink: "https://maps.example.com/sanskaar-school",
      website: "https://www.sanskaarvalley.com",
      social: {
        facebook: "https://facebook.com/SanskaarValleySchool",
        youtube: "https://youtube.com/SanskaarValley"
      },
      facilities: [
        { name: "Residential Hostels", description: "Separate boys and girls hostels with house parents" },
        { name: "Equestrian Center", description: "Horse riding as part of curriculum" },
        { name: "Meditation Center", description: "Daily yoga and meditation sessions" }
      ],
      events: [
        { name: "Annual Day 'Pratibimb'", date: "December 15th" },
        { name: "Sports Meet", date: "January 20-22, 2024" }
      ],
      support: {
        email: "admissions@sanskaarvalley.com",
        phone: "+91-7554095000"
      },
      rating: "4.6",
      reviews: "280+",
      established: "2009",
      tags: ["Residential", "CBSE", "Holistic Development"],
      categoryInsights: {
        academics: {
          curriculum: "CBSE with optional Cambridge IGCSE",
          specialPrograms: ["STEM Innovation Lab", "Financial Literacy Course"]
        },
        residentialLife: {
          housing: "Air-conditioned dorms for senior students",
          meals: "Nutritious vegetarian meals prepared by dietitians"
        },
        admissionDetails: {
          process: ["Entrance Test", "Interview", "Interaction"],
          ageCriteria: "Class I - 5+ years as on 31st March"
        },
        feeStructure: {
          dayScholar: "₹1,20,000 annually",
          residential: "₹3,50,000 annually (includes boarding)"
        },
        sportsFacilities: [
          "Olympic-size swimming pool",
          "Synthetic basketball court",
          "Squash courts"
        ],
        coCurriculum: [
          "Robotics Club",
          "Debating Society",
          "Eco Club"
        ],
        healthCare: [
          "24/7 medical center",
          "Tie-up with nearby hospitals",
          "Psychological counseling"
        ],
        parentEngagement: [
          "Monthly webinars",
          "Parent-teacher meetings quarterly",
          "Online progress tracking"
        ],
        sustainability: [
          "Solar-powered campus",
          "Rainwater harvesting",
          "Organic farming"
        ]
      }
    }
  },

  // Indore - School
  {
    category: "School",
    metropolis: "Indore",
    details: {
      name: "The Emerald Heights International School",
      cardImages: [
        "https://example.com/emerald-school1.jpg",
        "https://example.com/emerald-school2.jpg"
      ],
      placeImages: [
        "https://example.com/emerald-school1.jpg",
        "https://example.com/emerald-school2.jpg",
        "https://example.com/emerald-school3.jpg",
        "https://example.com/emerald-school4.jpg"
      ],
      description: "Premier co-educational school offering ICSE, IGCSE and IBDP programmes with world-class infrastructure.",
      address: "Opp. CASA Grand, Airport Road, Indore - 452005",
      timings: "Mon-Sat: 8 AM - 3 PM",
      entryFee: "N/A",
      mapLink: "https://maps.example.com/emerald-school",
      website: "https://www.emeraldheights.edu.in",
      social: {
        facebook: "https://facebook.com/EmeraldHeightsIndore",
        instagram: "https://instagram.com/emeraldheightsindore"
      },
      facilities: [
        { name: "Digital Classrooms", description: "Smart boards and audio-visual aids" },
        { name: "Astronomy Dome", description: "Fully equipped observatory" },
        { name: "Sports Complex", description: "Includes skating rink and shooting range" }
      ],
      events: [
        { name: "Technovation", date: "Annual tech fest in August" },
        { name: "Kaleidoscope", date: "Cultural festival in December" }
      ],
      support: {
        email: "info@emeraldheights.edu.in",
        phone: "+91-7314051000"
      },
      rating: "4.7",
      reviews: "320+",
      established: "1993",
      tags: ["ICSE", "IGCSE", "IB", "Sports Excellence"],
      categoryInsights: {
        curriculumOptions: {
          ICSE: "Class I-X",
          IGCSE: "Class IX-X",
          IBDP: "Class XI-XII"
        },
        admissionProcess: {
          steps: ["Registration", "Assessment", "Interaction"],
          documentsRequired: ["Birth Certificate", "Previous School Reports"]
        },
        feeDetails: {
          registration: "₹5,000 (non-refundable)",
          annualTuition: "₹1,75,000 - ₹2,25,000 depending on grade"
        },
        sportsAcademy: [
          "Cricket coaching by former Ranji players",
          "State-level swimming coaches",
          "Professional badminton courts"
        ],
        performingArts: [
          "Western and Indian music departments",
          "Professional dance studios",
          "Theatre workshops"
        ],
        internationalPrograms: [
          "Student exchange with Singapore schools",
          "Model UN participation",
          "NASA Space Camp collaborations"
        ],
        transportation: {
          fleet: "50+ GPS-enabled buses",
          coverage: "Entire Indore city and nearby areas"
        },
        achievements: [
          "Consistent 100% ICSE/ISC pass rate",
          "National level sports champions",
          "International robotics competition winners"
        ],
        parentCommunity: [
          "Active PTA",
          "Monthly newsletters",
          "Mobile app for updates"
        ]
      }
    }
  },

  // Bangalore - School
  {
    category: "School",
    metropolis: "Bangalore",
    details: {
      name: "Inventure Academy",
      cardImages: [
        "https://example.com/inventure-school1.jpg",
        "https://example.com/inventure-school2.jpg"
      ],
      placeImages: [
        "https://example.com/inventure-school1.jpg",
        "https://example.com/inventure-school2.jpg",
        "https://example.com/inventure-school3.jpg",
        "https://example.com/inventure-school4.jpg"
      ],
      description: "Progressive day school offering ICSE curriculum with emphasis on innovation, critical thinking and experiential learning.",
      address: "Whitefield-Sarjapur Road, Chikkavaderapura, Bangalore - 560087",
      timings: "Mon-Fri: 8:30 AM - 3:30 PM, Sat: 8:30 AM - 12:30 PM",
      entryFee: "N/A",
      mapLink: "https://maps.example.com/inventure-academy",
      website: "https://www.inventureacademy.com",
      social: {
        facebook: "https://facebook.com/InventureAcademy",
        twitter: "https://twitter.com/InventureAcademy"
      },
      facilities: [
        { name: "Innovation Lab", description: "Makerspace with 3D printers and tools" },
        { name: "Outdoor Classrooms", description: "Nature-based learning spaces" },
        { name: "Performing Arts Block", description: "Dedicated spaces for music, dance and drama" }
      ],
      events: [
        { name: "Invention Convention", date: "Annual science fair in October" },
        { name: "Literary Fest", date: "July 15-17, 2024" }
      ],
      support: {
        email: "admissions@inventureacademy.com",
        phone: "+91-8049451234"
      },
      rating: "4.6",
      reviews: "400+",
      established: "2005",
      tags: ["ICSE", "Innovative Pedagogy", "Tech-Integrated"],
      categoryInsights: {
        educationalApproach: {
          methodology: "Inquiry-based learning",
          specialPrograms: ["Design Thinking", "Financial Literacy"]
        },
        admissionDetails: {
          process: ["Online Application", "Child Assessment", "Parent Interaction"],
          ageCutoff: "3 years for Pre-KG as on June 1"
        },
        feeStructure: {
          oneTime: "Admission Fee: ₹50,000",
          annual: "Tuition Fee: ₹2,00,000 - ₹2,50,000"
        },
        technologyIntegration: [
          "1:1 iPad program for middle school",
          "Coding curriculum from Grade 3",
          "AI and robotics labs"
        ],
        sustainabilityInitiatives: [
          "Solar-powered campus",
          "Waste management program",
          "Organic kitchen garden"
        ],
        coCurriculum: [
          "50+ clubs including Debate, Robotics, Eco Club",
          "Mandatory community service hours",
          "Outbound learning programs"
        ],
        sportsFacilities: [
          "FIFA-standard football field",
          "Indoor sports complex",
          "Swimming pool"
        ],
        parentPartnership: [
          "Regular workshops",
          "Digital portfolio access",
          "Parent volunteer opportunities"
        ],
        achievements: [
          "National award for innovative teaching practices",
          "Consistent top ICSE results in Bangalore",
          "International robotics competition winners"
        ]
      }
    }
  },

  // Chennai - School
  {
    category: "School",
    metropolis: "Chennai",
    details: {
      name: "The Hindu Senior Secondary School",
      cardImages: [
        "https://example.com/hindu-school1.jpg",
        "https://example.com/hindu-school2.jpg"
      ],
      placeImages: [
        "https://example.com/hindu-school1.jpg",
        "https://example.com/hindu-school2.jpg",
        "https://example.com/hindu-school3.jpg",
        "https://example.com/hindu-school4.jpg"
      ],
      description: "Renowned CBSE-affiliated institution with 50+ years of academic excellence and strong emphasis on traditional values.",
      address: "11, Third Main Road, Indira Nagar, Chennai - 600020",
      timings: "Mon-Sat: 8:15 AM - 3:15 PM",
      entryFee: "N/A",
      mapLink: "https://maps.example.com/hindu-school",
      website: "https://www.thehinduschool.org",
      social: {
        facebook: "https://facebook.com/TheHinduSchool",
        youtube: "https://youtube.com/TheHinduSchoolOfficial"
      },
      facilities: [
        { name: "Heritage Library", description: "10,000+ volumes including rare collections" },
        { name: "Science Park", description: "Outdoor science exhibits" },
        { name: "Audio-Visual Theater", description: "300-seat digital learning space" }
      ],
      events: [
        { name: "Sanskriti", date: "Annual cultural fest in January" },
        { name: "Vigyan Mela", date: "Science exhibition in August" }
      ],
      support: {
        email: "principal@thehinduschool.org",
        phone: "+91-4424912345"
      },
      rating: "4.5",
      reviews: "380+",
      established: "1973",
      tags: ["CBSE", "Academic Excellence", "Value-Based"],
      categoryInsights: {
        academicStructure: {
          curriculum: "CBSE with Tamil as second language",
          grading: "Continuous and Comprehensive Evaluation (CCE)"
        },
        admissionProcedure: {
          process: ["Application Submission", "Interaction", "Document Verification"],
          importantDates: "Registration begins November for next academic year"
        },
        feeDetails: {
          admissionFee: "₹25,000 (one-time)",
          tuitionFee: "₹75,000 annually"
        },
        specialPrograms: [
          "Heritage Club preserving Tamil culture",
          "Young Scholars Program for gifted students",
          "Remedial classes for slow learners"
        ],
        sportsExcellence: [
          "State-level cricket and basketball teams",
          "Aerobics and yoga as part of curriculum",
          "Annual sports day with 100+ events"
        ],
        technologyIntegration: [
          "Smart classrooms for all grades",
          "Computer labs with latest software",
          "Digital library access"
        ],
        alumniNetwork: [
          "Strong global alumni association",
          "Annual alumni meet",
          "Mentorship program"
        ],
        communityOutreach: [
          "Adopted nearby government school",
          "Regular charity drives",
          "Elderly support initiatives"
        ],
        achievements: [
          "Consistent 100% CBSE pass rate",
          "National toppers in various subjects",
          "Awards for best managed school"
        ]
      }
    }
  },

  // Delhi - Airport
  {
    category: "Airport",
    metropolis: "Delhi",
    details: {
      name: "Indira Gandhi International Airport",
      cardImages: [
        "https://example.com/igia1.jpg",
        "https://example.com/igia2.jpg"
      ],
      placeImages: [
        "https://example.com/igia1.jpg",
        "https://example.com/igia2.jpg",
        "https://example.com/igia3.jpg",
        "https://example.com/igia4.jpg"
      ],
      description: "India's largest and busiest international airport.",
      address: "Airport Road, Delhi",
      timings: "24/7",
      entryFee: "Free (ticket required for flights)",
      mapLink: "https://maps.example.com/delhi-airport",
      website: "https://www.newdelhiairport.in",
      social: {
        facebook: "https://facebook.com/delhiairport",
        twitter: "https://twitter.com/delhiairport",
        instagram: "https://instagram.com/delhiairport"
      },
      facilities: [
        { name: "Lounges", description: "Premium lounges available" },
        { name: "Shopping", description: "Duty-free and retail stores" }
      ],
      events: [],
      support: {
        email: "feedback@delhiairport.com",
        phone: "+91-1125602000"
      },
      rating: "4.5",
      reviews: "15000+",
      established: "1962",
      tags: ["International", "Transport", "Aviation"],
      categoryInsights: {
        terminals: [
          {
            name: "Terminal 3",
            airlines: "All international & some domestic flights",
            checkIn: "3 hours before departure (international)"
          },
          {
            name: "Terminal 2",
            airlines: "Domestic flights (IndiGo, SpiceJet)",
            checkIn: "2 hours before departure"
          }
        ],
        transportation: [
          "Delhi Metro Airport Express Line (every 10 mins)",
          "Prepaid taxi counter (24/7)",
          "Bus services to city center"
        ],
        lounges: [
          "Plaza Premium Lounge (T3 - ₹2,500 for 3 hours)",
          "Air India Maharaja Lounge (T3 - Business class access)"
        ],
        shopping: [
          "Duty-free shops (international departures)",
          "Indian handicrafts & souvenirs"
        ],
        dining: [
          "Food Court (T3 - Indian & international cuisine)",
          "Cafés (Starbucks, Costa Coffee)"
        ],
        services: [
          "Currency exchange (24/7)",
          "Baggage wrapping",
          "Left luggage facility"
        ],
        parking: [
          "Short-term: ₹150 first hour, ₹100 subsequent hours",
          "Long-term: ₹500 per day"
        ],
        wifi: "Free for 45 minutes (requires mobile verification)",
        hotels: [
          "Aerocity Hotels (5 min drive)",
          "Transit hotel inside T3"
        ],
        flightInfo: "Real-time displays throughout terminals"
      }
    }
  }
  // Additional places can be added following the same structure
];

function showPlacesOverlay() {
  let overlay = document.getElementById('placesOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'placesOverlay';
    overlay.className = 'places-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(publicPlacesData.map(place => place.category))];

  overlay.innerHTML = `
    <div class="places-content">
      <div class="places-header">
        <div class="header-title-container">
          <h2>Public Places</h2>
          <div class="header-subtitle">Discover attractions</div>
        </div>
        <div class="header-buttons-container">
          <button class="sort-button" onclick="toggleSortOptions()">
            <i class="fas fa-sliders-h"></i>
          </button>
          <button class="close-places" onclick="hidePlacesOverlay()">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      
      <div class="sort-options-container" id="sortOptionsContainer">
        <div class="sort-option" onclick="sortPlaces('rating')">
          <i class="fas fa-star"></i> Highest Rated
        </div>
        <div class="sort-option" onclick="sortPlaces('name')">
          <i class="fas fa-sort-alpha-down"></i> Alphabetical
        </div>
        <div class="sort-option" onclick="sortPlaces('distance')">
          <i class="fas fa-map-marker-alt"></i> Nearest
        </div>
      </div>

      <div class="places-filter-container">
        <div class="places-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterPlaces('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="places-grid" id="placesGrid">
        ${renderPlaceCards(publicPlacesData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="placesExploreInput" placeholder="Search for parks, museums..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('placesExploreInput');
  const placesGrid = document.getElementById('placesGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
  performSearch(publicPlacesData, query, placesGrid, filterButtons, renderPlaceCards);
}, 300);

exploreInput.addEventListener('input', (e) => {
  debouncedSearch(e.target.value);
});

exploreInput.addEventListener('keypress', (e) => {
  if (e.key === 'Enter') {
    performSearch(publicPlacesData, exploreInput.value, placesGrid, filterButtons, renderPlaceCards);
  }
});

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(publicPlacesData, query, placesGrid, filterButtons, renderPlaceCards);
  });

  function performSearch(places, query, gridElement, filterButtons, renderFunction) {
  if (!query.trim()) {
    gridElement.innerHTML = renderFunction(places);
    // Reset to 'All' filter when search is cleared
    filterButtons.forEach(btn => {
      btn.classList.remove('active');
      if (btn.dataset.category === 'All') btn.classList.add('active');
    });
    return;
  }

  // Normalize and preprocess the query
  const normalizedQuery = query.toLowerCase().trim()
    .replace(/[^\w\s₹]/g, '') // Remove special chars except ₹
    .replace(/\s+/g, ' ');    // Collapse multiple spaces

  // Enhanced extraction with synonyms and broader matching
  const categorySynonyms = {
    'park': 'Park',
    'garden': 'Park',
    'school': 'School',
    'college': 'School',
    'education': 'School',
    'hospital': 'Hospital',
    'clinic': 'Hospital',
    'medical': 'Hospital',
    'gym': 'Gym',
    'fitness': 'Gym',
    'workout': 'Gym',
    'wedding': 'Wedding Venue',
    'marriage': 'Wedding Venue',
    'reception': 'Wedding Venue',
    'movie': 'Movie Theater',
    'cinema': 'Movie Theater',
    'theatre': 'Movie Theater',
    'hotel': 'Hotel & Resort',
    'resort': 'Hotel & Resort',
    'mall': 'Shopping Mall',
    'shopping': 'Shopping Mall',
    'museum': 'Museum',
    'gallery': 'Museum',
    'airport': 'Airport',
    'flight': 'Airport'
  };

  const citySynonyms = {
    'ncr': 'Delhi',
    'bpl': 'Bhopal',
    'idr': 'Indore',
    'new delhi': 'Delhi',
    'bombay': 'Mumbai',
    'bengaluru': 'Bangalore',
    'blr': 'Bangalore'
  };

  // Advanced query parsing with priority scoring
  let categoryMatch, cityMatch, facilityMatch, eventMatch, ratingMatch;
  let hasNegation = false;

  // Check for negation terms
  hasNegation = /\b(no|not|without|except)\b/.test(normalizedQuery);

  // 1. Category detection with synonyms and fuzzy matching
  const categoryPattern = new RegExp(
    `\\b(${Object.keys(categorySynonyms).concat([
      'park', 'school', 'hospital', 'gym', 
      'wedding venue', 'movie theater', 
      'hotel', 'shopping mall', 'museum', 'airport'
    ]).join('|')})\\b`, 'i');
  
  categoryMatch = normalizedQuery.match(categoryPattern);
  if (categoryMatch) {
    const matchedTerm = categoryMatch[0].toLowerCase();
    categoryMatch[0] = categorySynonyms[matchedTerm] || matchedTerm;
    // Capitalize first letter
    categoryMatch[0] = categoryMatch[0].charAt(0).toUpperCase() + categoryMatch[0].slice(1).toLowerCase();
  }

  // 2. City detection with synonyms
  const cityPattern = new RegExp(
    `\\b(${Object.keys(citySynonyms).concat([
      'delhi', 'mumbai', 'bhopal', 'bangalore',
      'kolkata', 'chennai', 'hyderabad', 'pune', 'ahmedabad',
      'jaipur', 'lucknow', 'kanpur', 'nagpur', 'indore'
    ]).join('|')})\\b`, 'i'
  );
  
  cityMatch = normalizedQuery.match(cityPattern);
  if (cityMatch) {
    const matchedTerm = cityMatch[0].toLowerCase();
    cityMatch[0] = citySynonyms[matchedTerm] || matchedTerm;
    // Capitalize first letter
    cityMatch[0] = cityMatch[0].charAt(0).toUpperCase() + cityMatch[0].slice(1).toLowerCase();
  }

  // 3. Facility detection
  const facilityTerms = [
    'parking', 'wifi', 'restroom', 'food', 'cafeteria',
    'guided tour', 'accessibility', 'wheelchair', 'shop',
    'first aid', 'water', 'picnic', 'library', 'sports',
    'emergency', 'clinic', 'cardio', 'weights', 'banquet',
    'catering', 'dolby', 'food court', 'spa', 'dining',
    'valet', 'kids', 'play area', 'pool', 'gym'
  ];
  facilityMatch = normalizedQuery.match(new RegExp(`\\b(${facilityTerms.join('|')})\\b`, 'i'));

  // 4. Event detection
  const eventTerms = [
    'yoga', 'festival', 'exhibition', 'concert', 'show',
    'camp', 'workshop', 'fair', 'sale', 'performance',
    'movie', 'screening', 'wedding', 'expo', 'seminar'
  ];
  eventMatch = normalizedQuery.match(new RegExp(`\\b(${eventTerms.join('|')})\\b`, 'i'));

  // 5. Rating detection
  ratingMatch = normalizedQuery.match(/(\d+(?:\.\d+)?)\s*(?:star|rating)/i) ||
                normalizedQuery.match(/(high|good|excellent|poor|bad)\s*(?:rated|rating|reviews?)/i);

  // 6. Price detection
  const priceMatch = normalizedQuery.match(/(free|paid|entry fee|ticket|cost)/i);

  // Advanced filtering with scoring system
  const filteredPlaces = places.map(place => {
    let score = 0;
    let matches = [];
    let mismatches = [];

    // Category matching (high importance)
    if (categoryMatch) {
      const categoryLower = place.category.toLowerCase();
      if (categoryLower.includes(categoryMatch[0].toLowerCase())) {
        score += 30;
        matches.push(`Category: ${place.category}`);
      } else {
        mismatches.push(`Category: ${categoryMatch[0]}`);
        return null; // Exclude if category doesn't match
      }
    }

    // City matching (high importance)
    if (cityMatch) {
      const cityLower = place.metropolis.toLowerCase();
      if (cityLower.includes(cityMatch[0].toLowerCase())) {
        score += 30;
        matches.push(`City: ${place.metropolis}`);
      } else {
        mismatches.push(`City: ${cityMatch[0]}`);
        return null; // Exclude if city doesn't match
      }
    }

    // Facility matching (medium importance)
    if (facilityMatch) {
      const facilityLower = facilityMatch[0].toLowerCase();
      const hasFacility = place.details.facilities?.some(f => 
        f.name.toLowerCase().includes(facilityLower) ||
        f.description.toLowerCase().includes(facilityLower)
      );
      
      if (hasFacility) {
        score += 20;
        matches.push(`Facility: ${facilityMatch[0]}`);
      } else {
        mismatches.push(`Facility: ${facilityMatch[0]}`);
      }
    }

    // Event matching (medium importance)
    if (eventMatch) {
      const eventLower = eventMatch[0].toLowerCase();
      const hasEvent = place.details.events?.some(e => 
        e.name.toLowerCase().includes(eventLower) ||
        (e.date && e.date.toLowerCase().includes(eventLower))
      );
      
      if (hasEvent) {
        score += 20;
        matches.push(`Event: ${eventMatch[0]}`);
      } else {
        mismatches.push(`Event: ${eventMatch[0]}`);
      }
    }

    // Rating matching (medium importance)
    if (ratingMatch) {
      if (ratingMatch[1] && !isNaN(ratingMatch[1])) {
        const minRating = parseFloat(ratingMatch[1]);
        const placeRating = parseFloat(place.details.rating);
        if (placeRating >= minRating) {
          score += 15;
          matches.push(`Rating ≥ ${minRating}`);
        }
      } else if (/high|good|excellent/i.test(ratingMatch[0])) {
        const placeRating = parseFloat(place.details.rating);
        if (placeRating >= 4.5) {
          score += 15;
          matches.push(`Highly rated`);
        }
      }
    }

    // Price matching (low importance)
    if (priceMatch) {
      const priceTerm = priceMatch[0].toLowerCase();
      if (priceTerm === 'free') {
        if (place.details.entryFee?.toLowerCase().includes('free')) {
          score += 10;
          matches.push(`Free entry`);
        }
      } else if (priceTerm === 'paid') {
        if (!place.details.entryFee?.toLowerCase().includes('free')) {
          score += 10;
          matches.push(`Paid entry`);
        }
      }
    }

    // General text search (fallback)
    if (!categoryMatch && !cityMatch && !facilityMatch && 
        !eventMatch && !ratingMatch && !priceMatch) {
      const searchFields = [
        place.category,
        place.metropolis,
        place.details.name,
        place.details.description,
        ...place.details.tags,
        ...(place.details.facilities?.map(f => f.name) || []),
        ...(place.details.facilities?.map(f => f.description) || []),
        ...(place.details.events?.map(e => e.name) || []),
        ...(place.details.categoryInsights ? Object.values(place.details.categoryInsights).flat() : [])
      ].join(' ').toLowerCase();
      
      if (searchFields.includes(normalizedQuery)) {
        score += 50; // High score for direct match
        matches.push(`General match`);
      } else {
        // Try partial matching
        const queryWords = normalizedQuery.split(/\s+/);
        const matchedWords = queryWords.filter(word => 
          word.length > 3 && searchFields.includes(word)
        );
        if (matchedWords.length > 0) {
          score += matchedWords.length * 5;
          matches.push(`Partial match: ${matchedWords.join(', ')}`);
        } else {
          return null; // No match at all
        }
      }
    }

    // Handle negation queries
    if (hasNegation) {
      const negatedTerms = normalizedQuery.match(/\b(no|not|without|except)\s+(\w+)/i);
      if (negatedTerms) {
        const negatedTerm = negatedTerms[2];
        const searchFields = [
          place.category,
          place.metropolis,
          place.details.name,
          ...place.details.tags
        ].join(' ').toLowerCase();
        
        if (searchFields.includes(negatedTerm)) {
          return null; // Exclude places matching negated term
        }
      }
    }

    return {
      place,
      score,
      matches,
      mismatches
    };
  })
  .filter(Boolean) // Remove null entries
  .sort((a, b) => b.score - a.score) // Sort by score descending
  .map(item => item.place); // Extract just the places

  // Enhanced empty state handling
  if (filteredPlaces.length === 0) {
    const suggestions = generatePlaceSearchSuggestions(query, places);
    gridElement.innerHTML = `
      <div class="no-results">
        <div class="no-results-icon">🔍</div>
        <h3>No exact matches found</h3>
        <p>We couldn't find places matching "${query}"</p>
        ${suggestions.length > 0 ? `
          <div class="suggestions">
            <p>Try one of these instead:</p>
            <ul>
              ${suggestions.map(suggestion => `
                <li onclick="document.getElementById('placesExploreInput').value = '${suggestion}'; performSearch(publicPlacesData, '${suggestion}', document.getElementById('placesGrid'), document.querySelectorAll('.filter-button'), renderPlaceCards)">
                  ${suggestion}
                </li>
              `).join('')}
            </ul>
          </div>
        ` : ''}
      </div>
    `;
  } else {
    gridElement.innerHTML = renderFunction(filteredPlaces);
  }

  // Update active filter button based on search
  updatePlaceActiveFilterButton(filterButtons, categoryMatch);
}

function updatePlaceActiveFilterButton(filterButtons, categoryMatch) {
  // Reset all buttons first
  filterButtons.forEach(btn => btn.classList.remove('active'));
  
  // Determine which filter to activate
  if (categoryMatch) {
    // Find matching filter button
    const matchingButton = Array.from(filterButtons).find(btn => 
      btn.dataset.category.toLowerCase() === categoryMatch[0].toLowerCase()
    );
    
    if (matchingButton) {
      matchingButton.classList.add('active');
    } else {
      // If no exact match, try to find a partial match
      const partialMatch = Array.from(filterButtons).find(btn => 
        btn.dataset.category.toLowerCase().includes(categoryMatch[0].toLowerCase()) ||
        categoryMatch[0].toLowerCase().includes(btn.dataset.category.toLowerCase())
      );
      
      if (partialMatch) {
        partialMatch.classList.add('active');
      } else {
        // Default to 'All' if no match found
        filterButtons[0].classList.add('active');
      }
    }
  } else {
    // Default to 'All' if no specific category was mentioned
    filterButtons[0].classList.add('active');
  }
}

// Helper function to generate search suggestions for places
function generatePlaceSearchSuggestions(query, places) {
  const suggestions = new Set();
  
  // 1. Suggest similar categories
  const categories = [...new Set(places.map(p => p.category))];
  categories.forEach(cat => {
    if (cat.toLowerCase().includes(query.toLowerCase().substring(0, 3))) {
      suggestions.add(cat);
    }
  });

  // 2. Suggest popular places
  if (query.length > 3) {
    places.forEach(place => {
      if (place.details.name.toLowerCase().includes(query.toLowerCase())) {
        suggestions.add(place.details.name);
      }
    });
  }

  // 3. Add facility-based suggestions
  const allFacilities = new Set();
  places.forEach(place => {
    place.details.facilities?.forEach(f => {
      allFacilities.add(f.name);
    });
  });
  
  allFacilities.forEach(facility => {
    if (facility.toLowerCase().includes(query.toLowerCase())) {
      suggestions.add(facility);
    }
  });

  // 4. Add event-based suggestions
  const allEvents = new Set();
  places.forEach(place => {
    place.details.events?.forEach(e => {
      allEvents.add(e.name);
    });
  });
  
  allEvents.forEach(event => {
    if (event.toLowerCase().includes(query.toLowerCase())) {
      suggestions.add(event);
    }
  });

  // 5. Add city-based suggestions if no city was mentioned
  if (!/(delhi|mumbai|bangalore|kolkata|chennai|hyderabad|pune)/i.test(query)) {
    suggestions.add(`${query} in Delhi`);
    suggestions.add(`${query} in Mumbai`);
  }

  return Array.from(suggestions).slice(0, 5); // Return top 5 suggestions
}

  setTimeout(() => overlay.classList.add('active'), 10);


  const style = document.createElement('style');
  style.textContent = `
.places-overlay {
  font-family: poppins;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.places-overlay::-webkit-scrollbar {
  display: none;
}

.places-overlay.active {
  opacity: 1;
  visibility: visible;
}

.places-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.places-header {
  padding: 12px 20px;
  background: rgba(26, 26, 31, 0.98);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.header-title-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}

.places-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.header-subtitle {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.8em;
  margin-top: 2px;
  font-weight: 400;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.close-places {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 1em;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-places:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.close-places:active {
  transform: scale(0.95);
}

.sort-button {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  font-size: 0.9em;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.sort-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
}

.sort-button:active {
  transform: translateY(0);
}

.button-text {
  font-weight: 500;
}

.sort-options-container {
  position: fixed;
  top: 72px;
  right: 20px;
  background: #2a2a35;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  z-index: 40;
  padding: 8px 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.sort-options-container.active {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.sort-option {
  padding: 12px 20px;
  color: white;
  font-size: 0.9em;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.sort-option:hover {
  background: rgba(255, 215, 0, 0.1);
  color: var(--primary-color);
}

.sort-option i {
  width: 20px;
  text-align: center;
}

.places-filter-container {
  position: fixed;
  top: 64px;
  left: 0;
  right: 0;
  width: 100%;
  z-index: 10;
  background: #1a1a1f;
  padding: 12px 0;
  box-sizing: border-box;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
  border-bottom: 1px solid rgba(255, 215, 0, 0.1);
}

.places-filter-container::-webkit-scrollbar {
  display: none;
}

.places-filter-buttons {
  display: inline-flex;
  padding: 0 16px;
  gap: 8px;
  white-space: nowrap;
}

.filter-button {
    font-family: poppins;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: rgba(255, 255, 255, 0.9);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 20px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  margin: 0;
}

.filter-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: scale(1.03);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: #000;
  font-weight: 600;
  border-color: var(--primary-color);
  box-shadow: 0 4px 10px rgba(255, 215, 0, 0.3);
}

/* Add some space at the end for better scrolling */
.places-filter-buttons::after {
  content: '';
  display: block;
  min-width: 16px;
  height: 1px;
}

@media (min-width: 1024px) {
   .places-filter-container {
   top: 72px;
    }
}

.places-grid {
  margin-top: 90px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  padding-bottom: 140px;
  width: 100%;
}

.place-card {
  background: #ffffff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  position: relative;
  border: none;
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
  height: 100%;
}

.place-card:hover {
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.place-card:active {
  transform: translateY(-1px);
  transition: transform 0.1s ease;
}

.image-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 12px 12px 0 0;
  margin-bottom: 0;
  box-shadow: none;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease, transform 0.5s ease;
  transform: scale(1.03);
  will-change: opacity, transform;
}

.carousel-item.active {
  opacity: 1;
  transform: scale(1);
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.place-card:hover .carousel-item.active img {
  transform: scale(1.05);
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  -webkit-tap-highlight-color: transparent;
}

.image-carousel:hover .carousel-control {
  opacity: 0.9;
}

.carousel-control:hover {
  background: rgba(0, 0, 0, 0.85);
  transform: translateY(-50%) scale(1.1);
}

.carousel-control:active {
  transform: translateY(-50%) scale(0.95);
}

.carousel-control.prev {
  left: 12px;
}

.carousel-control.next {
  right: 12px;
}

.carousel-pagination {
  position: absolute;
  bottom: 12px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
}

.place-card-content {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.place-card-header {
  display: flex;
  flex-direction: column;
  padding: 0;
  background: transparent;
  border-bottom: none;
}

.metro-name {
  font-family: poppins;
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin: 0 0 4px 0;
  line-height: 1.3;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.place-category {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 14px;
  margin-bottom: 8px;
}

.place-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
}

.place-rating .star {
  color: #ffc107;
  font-weight: 600;
}

.place-rating .count {
  color: #666;
  font-size: 14px;
}

.place-info-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
  margin: 4px 0;
}

.info-item {
  flex: 1 1 100%;
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px;
  border-radius: 10px;
  background: rgba(255, 215, 0, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.1);
  transition: all 0.3s ease;
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

.info-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: var(--primary-color);
}

.info-icon i {
  font-size: 14px;
}

.info-content {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.info-label {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #ffc107;
  margin-bottom: 6px;
}

.info-value {
  font-size: 13px;
  font-weight: 500;
  color: #000;
  line-height: 1.4;
  display: block;
  margin-top: 0;
}

.info-value + .info-value {
  margin-top: 6px;
  position: relative;
  padding-top: 6px;
}

.info-value + .info-value::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 215, 0, 0.1);
}

.info-item:hover {
  background: rgba(255, 215, 0, 0.08);
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.place-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
  margin: 12px 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.tag-container {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 10px 0;
}

.tag {
  background: #f5f5f5;
  color: #666;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
  transition: background-color 0.2s ease;
}

.tag:hover {
  background: #eeeeee;
}

.place-card-actions {
  display: flex;
  border-top: 1px solid #eee;
  padding: 0;
  background: transparent;
  margin-top: auto;
}

.action-button {
  flex: 1;
  background: transparent;
  color: #333;
  font-weight: 500;
  padding: 14px 0;
  border: none;
  border-radius: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  position: relative;
  transition: background-color 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.action-button:hover {
  background: #f5f5f5;
}

.action-button:active {
  background: #eeeeee;
}

.action-button i {
  color: #333;
  font-size: 16px;
}

.action-button:first-child {
  border-right: 1px solid #eee;
}

@media (max-width: 991px) {
  .places-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  }
}

@media (max-width: 768px) {
  .places-grid {
    grid-template-columns: 2fr;
    padding: 0 12px;
    margin-top: 90px;
    gap: 16px;
  }
  
  .place-card {
    margin-bottom: 0;
    border-radius: 12px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .image-carousel {
    height: 180px;
    border-radius: 12px 12px 0 0;
  }
  
  .place-card-content {
    padding: 14px;
  }
  
  .metro-name {
    font-size: 16px;
  }
  
  .place-description {
    font-size: 13px;
    margin: 10px 0;
  }
  
  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }
  
  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }
  
  .carousel-control {
    width: 32px;
    height: 32px;
    opacity: 0.7;
  }
  
  .image-carousel .carousel-control {
    opacity: 0.7;
  }
  
  .pagination-dot {
    width: 6px;
    height: 6px;
  }
  
  .pagination-dot.active {
    width: 16px;
  }
}

@media (max-width: 480px) {
  .place-card {
    margin-bottom: 0px;
  }
  
  .image-carousel {
    height: 160px;
  }
  
  .place-card-content {
    padding: 12px;
  }
  
  .place-description {
    -webkit-line-clamp: 2;
    margin: 8px 0;
  }
    
  .tag-container {
    margin: 8px 0;
  }
  
  .tag {
    padding: 2px 8px;
    font-size: 10px;
  }
  
  .action-button {
    padding: 10px 0;
    font-size: 12px;
  }
  
  .action-button i {
    font-size: 14px;
  }
  
  .carousel-control {
    width: 28px;
    height: 28px;
  }
  
  .places-filter-buttons {
    margin-top: 10px;
  }
  
  .places-filter-buttons {
    padding-bottom: 2px;
  }
  
  .filter-button {
    padding: 9px 12px;
    font-size: 14px;
  }
}

@media (max-width: 768px) and (orientation: portrait) {
  .places-grid {
    margin-top: 130px;
  }
  
  .place-card {
    border-radius: 10px;
  }
}

.place-card {
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .place-card,
  .place-card:hover,
  .carousel-item,
  .carousel-item.active,
  .carousel-item img,
  .carousel-control,
  .carousel-control:hover,
  .pagination-dot {
    transition: none;
    transform: none;
    animation: none;
  }
}

.place-card:focus {
  outline: 2px solid var(--primary-color);
  outline-offset: 3px;
}

@media (prefers-color-scheme: dark) {
  .place-card {
    background: #fff;
  }
  
  .metro-name {
    color: #000;
  }
  
  .place-category,
  .place-description,
  .place-rating .count {
    color: #000;
  }
  
  .tag {
    background: #000;
    color: #ddd;
  }
  
  .tag:hover {
    background: #444;
  }
  
  .place-card-actions {
    border-top: 1px solid #333;
  }
  
  .action-button {
    color: #000;
  }
  
  .action-button i {
    color: #000;
  }
  
  .action-button:first-child {
    border-right: 1px solid #333;
  }
  
  .action-button:hover {
    background: #fff;
  }
}

.chat-container {
  position: fixed;
  bottom: 0px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

.input-container {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#placesExploreInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 12px 50px 12px 20px;
  color: #fff;
  font-size: 0.95em;
  width: 100%;
  transition: border-color 0.3s ease;
}

#placesExploreInput:focus {
  border-color: rgba(255, 255, 255, 0.5);
}

.voice-search {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-search:hover {
  opacity: 0.8;
}

.voice-search.active {
  color: #ffd700;
  animation: pulse 1.5s infinite;
}

.voice-search i { 
  font-size: 1.5em; 
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 0px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #placesExploreInput {
    height: 70px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 1.7em;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #placesExploreInput {
    bottom: 0px;
    width: 60%;
    max-width: 1200px;
  }
  
  .places-grid {
    padding-bottom: 180px;
  }
}

@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .places-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .place-card {
    width: 100%;
    margin: 0;
  }

  .places-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .places-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #placesExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}

.full-screen-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.95);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.full-screen-image.active {
  opacity: 1;
  visibility: visible;
}

.full-screen-image img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 10px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  transform: scale(0.95);
  opacity: 0;
  transition: all 0.4s ease;
}

.full-screen-image.active img {
  transform: scale(1);
  opacity: 1;
}

.close-full-screen {
  position: absolute;
  top: 24px; 
  right: 24px;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close-full-screen:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}

@media (min-width: 1024px) {
.metro-name {
  font-size: 18px;
  }
}

@media (min-width: 1440px) {
.metro-name {
  font-size: 18px;
  }
}

/* No Results State */
.no-results {
  text-align: center;
  padding: 40px 20px;
  max-width: 600px;
  margin: 0 auto;
}

.no-results-icon {
  font-size: 3rem;
  color: #fff;
  margin-bottom: 20px;
}

.no-results h3 {
  font-size: 1.5rem;
  color: #fff;
  margin-bottom: 8px;
}

.no-results p {
  color: #fff;
  margin-bottom: 24px;
}

/* Suggestions */
.suggestions {
  background: #f9f9f9;
  border-radius: 12px;
  padding: 20px;
  margin-top: 20px;
}

.suggestions p {
  font-weight: 500;
  margin-bottom: 12px;
  color: #333;
}

.suggestions ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.suggestions li {
  padding: 12px 16px;
  margin-bottom: 8px;
  background: #000;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.2s ease;
  border: 1px solid #eee;
}

.suggestions li:hover {
  background: #000;
}

/* ===== Tablet (768px - 1024px) ===== */
@media (min-width: 768px) and (max-width: 1024px) {
  .places-grid {
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 16px;
    padding-bottom: 120px;
  }

  .place-card {
    border-radius: 14px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }

  .image-carousel {
    height: 180px;
  }

  .place-card-content {
    padding: 14px;
  }

  .metro-name {
    font-size: 16px;
    margin-bottom: 4px;
  }

  .place-category {
    font-size: 13px;
    margin-bottom: 8px;
  }

  .info-item {
    padding: 10px;
    gap: 8px;
  }

  .info-icon {
    width: 28px;
    height: 28px;
    border-radius: 6px;
  }

  .info-icon i {
    font-size: 13px;
  }

  .info-label {
    font-size: 10px;
    margin-bottom: 4px;
  }

  .info-value {
    font-size: 12px;
  }

  .place-description {
    font-size: 13px;
    margin: 10px 0;
    -webkit-line-clamp: 2;
  }

  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }

  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }

   .places-filter-container {
   top: 72px;
    }
}

/* ===== Laptop & Desktop (1025px+) ===== */
@media (min-width: 1025px) {
  .places-grid {
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 18px;
    padding-bottom: 140px;
  }

  .place-card {
    border-radius: 16px;
    box-shadow: 0 5px 12px rgba(0, 0, 0, 0.12);
    margin-bottom: 16px;
  }

  .image-carousel {
    height: 200px;
  }

  .place-card-content {
    padding: 16px;
  }

  .metro-name {
    font-size: 17px;
    margin-bottom: 4px;
  }

  .place-category {
    font-size: 14px;
    margin-bottom: 8px;
  }

  .info-item {
    padding: 12px;
    gap: 10px;
  }

  .info-icon {
    width: 30px;
    height: 30px;
    border-radius: 8px;
  }

  .info-icon i {
    font-size: 14px;
  }

  .info-label {
    font-size: 11px;
    margin-bottom: 6px;
  }

  .info-value {
    font-size: 13px;
  }

  .place-description {
    font-size: 14px;
    margin: 12px 0;
    -webkit-line-clamp: 2;
  }

  .tag {
    padding: 4px 10px;
    font-size: 12px;
  }

  .action-button {
    padding: 14px 0;
    font-size: 14px;
  }
}
  `;
  document.head.appendChild(style);
}

function renderPlaceCards(filteredData) {
  return filteredData.map(place => {
    const tagsHTML = place.details.tags && place.details.tags.length > 0 
      ? `<div class="tag-container">
          ${place.details.tags.map(tag => `<span class="tag">${tag}</span>`).join('')}
         </div>`
      : '';
    
    const actionButtonsHTML = `
      <a href="${place.details.mapLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
        <i class="fas fa-map-marker-alt"></i> Map
      </a>
      <button class="action-button" onclick="openPlacePage('${place.details.name}'); event.stopPropagation()">
        <i class="fas fa-info-circle"></i> Details
      </button>
    `;
    
    return `
      <div class="place-card" onclick="openPlacePage('${place.details.name}')">
        <div class="image-carousel">
          ${place.details.cardImages.map((image, index) => `
            <div class="carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${place.details.name}" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="carousel-pagination">
            ${place.details.cardImages.map((_, index) => `
              <div class="pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="carousel-control prev" onclick="changeCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="carousel-control next" onclick="changeCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        
        <div class="place-card-content">
          <div class="place-card-header">
            <div style="display: flex; justify-content: space-between; align-items: flex-start;">
              <h3 class="metro-name">${place.details.name}</h3>
              <div class="place-rating">
                <span class="star">${place.details.rating || '4.0'} ★</span>
              </div>
            </div>
            
            <div class="place-category">
              <span>${place.category} • ${place.metropolis}</span>
            </div>
            
            <div class="place-info-grid">
              <div class="info-item">
                <div class="info-icon">
                  <i class="fas fa-map-marker-alt"></i>
                </div>
                <div class="info-content">
                  <span class="info-label">Location & Hours</span>
                  <span class="info-value">${place.details.address}</span>
                  <span class="info-value" style="margin-top: 4px;">${place.details.timings}</span>
                </div>
              </div>
            </div>
            
            ${tagsHTML}
          </div>
        </div>

        <div class="place-card-actions">
          ${actionButtonsHTML}
        </div>
      </div>
    `;
  }).join('');
}

function openPlacePage(placeName) {
  const place = publicPlacesData.find(pl => pl.details.name === placeName);
  if (!place) return;

  const placePage = document.createElement('div');
  placePage.className = 'place-page';
  
  // Main page structure
  placePage.innerHTML = `
    <div class="place-header">
      <button class="back-button" onclick="closePlacePage()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>${escapeHTML(place.details.name)}</h2>
      <div class="header-actions">
        <button class="share-button" onclick="shareLocation('${escapeHTML(place.details.name)}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="place-content">
      <!-- Hero Section -->
      <div class="place-hero">
        <div class="place-image-carousel">
          ${place.details.placeImages.map((image, index) => `
            <div class="place-carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${escapeHTML(place.details.name)}" class="place-image" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="place-carousel-pagination">
            ${place.details.placeImages.map((_, index) => `
              <div class="place-pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentPlaceSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="place-carousel-control prev" onclick="changePlaceCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="place-carousel-control next" onclick="changePlaceCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        <div class="place-basic-info">
          <div class="place-star">
            <i class="fas fa-star"></i> ${place.details.rating} (${place.details.reviews})
          </div>
          <div class="place-type">
            ${place.category}
          </div>
          <div class="place-timings">
            <i class="fas fa-clock"></i> ${place.details.timings}
          </div>
        </div>
      </div>
      
      <!-- Description Section -->
      <div class="place-description-section">
        <h3>About</h3>
        <p>${escapeHTML(place.details.description)}</p>
        ${place.details.established ? `
          <div class="established-since">
            <i class="fas fa-calendar-alt"></i> Established: ${place.details.established}
          </div>
        ` : ''}
      </div>
      
      <!-- Location Section -->
      <div class="place-location-section">
        <h3>Location</h3>
        <div class="locations-grid">
          <div class="location-card">
            <div class="location-info">
              <h4>${escapeHTML(place.details.name)}</h4>
              <p class="location-address">
                <i class="fas fa-map-marker-alt"></i> ${escapeHTML(place.details.address)}
              </p>
              <p class="location-timings">
                <i class="fas fa-clock"></i> ${place.details.timings}
              </p>
            </div>
            <a href="${place.details.mapLink}" target="_blank" class="map-button">
              <i class="fas fa-map"></i> View Map
            </a>
          </div>
        </div>
      </div>
      
      <!-- Facilities Section -->
      <div class="place-facilities-section">
        <h3>Facilities</h3>
        <div class="facilities-grid">
          ${place.details.facilities?.map(facility => `
            <div class="facility-item">
              <div class="facility-icon">
                <i class="fas fa-${getFacilityIcon(facility.name)}"></i>
              </div>
              <div class="facility-info">
                <h4>${escapeHTML(facility.name)}</h4>
                <p>${escapeHTML(facility.description)}</p>
              </div>
            </div>
          `).join('') || '<p>No facilities information available</p>'}
        </div>
      </div>
      
      <!-- Category Specific Insights Section -->
      <div class="category-insights-section">
        <h3>${getCategoryInsightsTitle(place.category)}</h3>
        ${renderCategoryInsights(place)}
      </div>
      
      <!-- Events Section -->
      ${place.details.events?.length > 0 ? `
      <div class="place-events-section">
        <h3>Upcoming Events</h3>
        <div class="events-grid">
          ${place.details.events.map(event => `
            <div class="event-item">
              <div class="event-date">
                <span class="event-day">${getDayFromDate(event.date)}</span>
                <span class="event-month">${getMonthFromDate(event.date)}</span>
              </div>
              <div class="event-info">
                <h4>${escapeHTML(event.name)}</h4>
                <p>${escapeHTML(event.date)}</p>
              </div>
            </div>
          `).join('')}
        </div>
      </div>
      ` : ''}
      
      <!-- Contact Section -->
      <div class="place-contact-section">
        <h3>Contact</h3>
        <div class="contact-methods">
          ${place.details.support?.phone ? `
            <a href="tel:${place.details.support.phone}" class="contact-item">
              <i class="fas fa-phone"></i> ${place.details.support.phone}
            </a>
          ` : ''}
          ${place.details.support?.email ? `
            <a href="mailto:${place.details.support.email}" class="contact-item">
              <i class="fas fa-envelope"></i> ${place.details.support.email}
            </a>
          ` : ''}
          ${place.details.website ? `
            <a href="${place.details.website}" target="_blank" class="contact-item">
              <i class="fas fa-globe"></i> Visit Website
            </a>
          ` : ''}
        </div>
        
        <div class="social-links">
          ${place.details.social?.facebook ? `
            <a href="${place.details.social.facebook}" target="_blank" class="social-link">
              <i class="fab fa-facebook-f"></i>
            </a>
          ` : ''}
          ${place.details.social?.twitter ? `
            <a href="${place.details.social.twitter}" target="_blank" class="social-link">
              <i class="fab fa-twitter"></i>
            </a>
          ` : ''}
          ${place.details.social?.instagram ? `
            <a href="${place.details.social.instagram}" target="_blank" class="social-link">
              <i class="fab fa-instagram"></i>
            </a>
          ` : ''}
        </div>
      </div>
      
      <!-- Action Buttons -->
      <div class="place-actions">
        <a href="${place.details.mapLink}" target="_blank" class="primary-action">
          <i class="fas fa-directions"></i> Get Directions
        </a>
        <button class="secondary-action" onclick="showChatSupport('${escapeHTML(place.details.name)}')">
          <i class="fas fa-comment-alt"></i> Contact Place
        </button>
      </div>
    </div>
  `;

  document.body.appendChild(placePage);
  addPlacePageStyles();
  initializePlaceCarousel();
  initializeTabs();
  initializeSearch();
  scrollToTop();
}

// Helper functions for place page
function getFacilityIcon(facilityName) {
  const icons = {
    'Parking': 'parking',
    'Wi-Fi': 'wifi',
    'Restrooms': 'restroom',
    'Food Stalls': 'utensils',
    'Guided Tours': 'map-marked-alt',
    'Accessibility': 'wheelchair',
    'Souvenir Shop': 'shopping-bag',
    'First Aid': 'first-aid',
    'Drinking Water': 'tint',
    'Picnic Area': 'umbrella-beach',
    'Library': 'book',
    'Sports Complex': 'running',
    'Emergency Services': 'ambulance',
    'Specialty Clinics': 'stethoscope',
    'Cardio Section': 'heartbeat',
    'Weight Training': 'dumbbell',
    'Banquet Halls': 'glass-cheers',
    'Catering': 'utensils',
    'Dolby Atmos': 'volume-up',
    'Food Court': 'hamburger',
    'Spa': 'spa',
    'Fine Dining': 'wine-glass-alt',
    'Valet Parking': 'parking',
    'Kids Play Area': 'child'
  };
  return icons[facilityName] || 'check-circle';
}

function getCategoryInsightsTitle(category) {
  const titles = {
    'Park': 'Activities & Features',
    'School': 'Admissions & Academic Info',
    'Hospital': 'Medical Services & Facilities',
    'Gym': 'Membership & Training Plans',
    'Wedding Venue': 'Event Hosting Info',
    'Movie Theater': 'Now Showing & Booking',
    'Hotel & Resort': 'Bookings & Packages',
    'Shopping Mall': 'Mall Directory & Offers',
    'Museum': 'Exhibitions & Collections',
    'Airport': 'Traveler Services'
  };
  return titles[category] || 'Additional Information';
}

function renderCategoryInsights(place) {
  if (!place.details.categoryInsights) return '<p>No additional information available</p>';
  
  const insights = place.details.categoryInsights;
  let html = '<div class="insights-grid">';
  
  // School specific template
  if (place.category === 'School') {
    html += `
      <div class="insight-item">
        <h4>Admission Status</h4>
        <p>${insights.admissionStatus || 'Not specified'}</p>
      </div>
      <div class="insight-item">
        <h4>Fee Structure</h4>
        ${Object.entries(insights.feeStructure || {}).map(([grade, fee]) => `
          <p><strong>${grade}:</strong> ${fee}</p>
        `).join('')}
      </div>
      <div class="insight-item">
        <h4>Curriculum</h4>
        <p>${insights.curriculum || 'Not specified'}</p>
      </div>
      <div class="insight-item">
        <h4>Classes Offered</h4>
        <p>${insights.classesOffered || 'Not specified'}</p>
      </div>
      ${insights.entranceExam ? `
      <div class="insight-item">
        <h4>Entrance Exam</h4>
        <p>${insights.entranceExam}</p>
      </div>
      ` : ''}
      ${insights.facilities?.length > 0 ? `
      <div class="insight-item">
        <h4>Facilities</h4>
        <ul>${insights.facilities.map(f => `<li>${f}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Hospital specific template
  else if (place.category === 'Hospital') {
    html += `
      <div class="insight-item">
        <h4>Departments</h4>
        <ul>${(insights.departments || []).map(dept => `<li>${dept}</li>`).join('')}</ul>
      </div>
      <div class="insight-item">
        <h4>Consultation Charges</h4>
        ${Object.entries(insights.consultationCharges || {}).map(([type, charge]) => `
          <p><strong>${type}:</strong> ${charge}</p>
        `).join('')}
      </div>
      <div class="insight-item">
        <h4>Emergency Services</h4>
        <p>${insights.emergency || '24/7 emergency services'}</p>
      </div>
      ${insights.healthPackages?.length > 0 ? `
      <div class="insight-item">
        <h4>Health Packages</h4>
        <ul>${insights.healthPackages.map(pkg => `<li>${pkg}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Gym specific template
  else if (place.category === 'Gym') {
    html += `
      <div class="insight-item">
        <h4>Membership Plans</h4>
        ${Object.entries(insights.membershipPlans || {}).map(([plan, price]) => `
          <p><strong>${plan}:</strong> ${price}</p>
        `).join('')}
      </div>
      ${insights.trainers?.length > 0 ? `
      <div class="insight-item">
        <h4>Trainers</h4>
        <ul>${insights.trainers.map(trainer => `<li>${trainer}</li>`).join('')}</ul>
      </div>
      ` : ''}
      <div class="insight-item">
        <h4>Class Schedule</h4>
        ${Object.entries(insights.classSchedule || {}).map(([cls, time]) => `
          <p><strong>${cls}:</strong> ${time}</p>
        `).join('')}
      </div>
      ${insights.personalTraining ? `
      <div class="insight-item">
        <h4>Personal Training</h4>
        <p>${insights.personalTraining}</p>
      </div>
      ` : ''}
    `;
  }
  // Wedding Venue specific template
  else if (place.category === 'Wedding Venue') {
    html += `
      <div class="insight-item">
        <h4>Venue Capacity</h4>
        ${Object.entries(insights.venueCapacity || {}).map(([venue, capacity]) => `
          <p><strong>${venue}:</strong> ${capacity}</p>
        `).join('')}
      </div>
      <div class="insight-item">
        <h4>Package Rates</h4>
        ${Object.entries(insights.packageRates || {}).map(([pkg, rate]) => `
          <p><strong>${pkg}:</strong> ${rate}</p>
        `).join('')}
      </div>
      ${insights.cateringOptions?.length > 0 ? `
      <div class="insight-item">
        <h4>Catering Options</h4>
        <ul>${insights.cateringOptions.map(option => `<li>${option}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Movie Theater specific template
  else if (place.category === 'Movie Theater') {
    html += `
      ${insights.currentMovies?.length > 0 ? `
      <div class="insight-item">
        <h4>Now Showing</h4>
        ${insights.currentMovies.map(movie => `
          <p><strong>${movie.title}</strong> (${movie.screenType})</p>
          <p>${movie.timings.join(', ')}</p>
        `).join('')}
      </div>
      ` : ''}
      <div class="insight-item">
        <h4>Ticket Pricing</h4>
        ${Object.entries(insights.ticketPricing || {}).map(([type, price]) => `
          <p><strong>${type}:</strong> ${price}</p>
        `).join('')}
      </div>
      ${insights.foodMenu?.combos?.length > 0 ? `
      <div class="insight-item">
        <h4>Food Combos</h4>
        <ul>${insights.foodMenu.combos.map(combo => `<li>${combo}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Hotel specific template
  else if (place.category === 'Hotel & Resort') {
    html += `
      ${insights.roomTypes?.length > 0 ? `
      <div class="insight-item">
        <h4>Room Types</h4>
        ${insights.roomTypes.map(room => `
          <p><strong>${room.name}:</strong> ${room.price} (${room.capacity})</p>
        `).join('')}
      </div>
      ` : ''}
      ${insights.diningOptions?.length > 0 ? `
      <div class="insight-item">
        <h4>Dining Options</h4>
        <ul>${insights.diningOptions.map(option => `<li>${option}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.spaServices?.length > 0 ? `
      <div class="insight-item">
        <h4>Spa Services</h4>
        <ul>${insights.spaServices.map(service => `<li>${service}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Shopping Mall specific template
  else if (place.category === 'Shopping Mall') {
    html += `
      ${insights.storeDirectory?.length > 0 ? `
      <div class="insight-item">
        <h4>Store Directory</h4>
        ${insights.storeDirectory.map(category => `
          <p><strong>${category.category}:</strong> ${category.brands.join(', ')}</p>
        `).join('')}
      </div>
      ` : ''}
      ${insights.foodCourt?.length > 0 ? `
      <div class="insight-item">
        <h4>Food Court</h4>
        <ul>${insights.foodCourt.map(option => `<li>${option}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.ongoingOffers?.length > 0 ? `
      <div class="insight-item">
        <h4>Ongoing Offers</h4>
        <ul>${insights.ongoingOffers.map(offer => `<li>${offer}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Museum specific template
  else if (place.category === 'Museum') {
    html += `
      ${insights.collections?.length > 0 ? `
      <div class="insight-item">
        <h4>Collections</h4>
        <ul>${insights.collections.map(collection => `<li>${collection}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.guidedTourTimings?.length > 0 ? `
      <div class="insight-item">
        <h4>Guided Tour Timings</h4>
        <ul>${insights.guidedTourTimings.map(tour => `<li>${tour}</li>`).join('')}</ul>
      </div>
      ` : ''}
      <div class="insight-item">
        <h4>Ticket Pricing</h4>
        ${Object.entries(insights.ticketPricing || {}).map(([type, price]) => `
          <p><strong>${type}:</strong> ${price}</p>
        `).join('')}
      </div>
    `;
  }
  // Airport specific template
  else if (place.category === 'Airport') {
    html += `
      ${insights.terminals?.length > 0 ? `
      <div class="insight-item">
        <h4>Terminals</h4>
        ${insights.terminals.map(terminal => `
          <p><strong>${terminal.name}:</strong> ${terminal.airlines}</p>
          <p>Check-in: ${terminal.checkIn}</p>
        `).join('')}
      </div>
      ` : ''}
      ${insights.transportation?.length > 0 ? `
      <div class="insight-item">
        <h4>Transportation</h4>
        <ul>${insights.transportation.map(option => `<li>${option}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.services?.length > 0 ? `
      <div class="insight-item">
        <h4>Services</h4>
        <ul>${insights.services.map(service => `<li>${service}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Park specific template
  else if (place.category === 'Park') {
    html += `
      ${insights.activities?.length > 0 ? `
      <div class="insight-item">
        <h4>Activities</h4>
        <ul>${insights.activities.map(activity => `<li>${activity}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.historicalMonuments?.length > 0 ? `
      <div class="insight-item">
        <h4>Historical Monuments</h4>
        <ul>${insights.historicalMonuments.map(monument => `<li>${monument}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.bestTimeToVisit ? `
      <div class="insight-item">
        <h4>Best Time to Visit</h4>
        <p>${insights.bestTimeToVisit}</p>
      </div>
      ` : ''}
    `;
  }
  // Generic template for other categories
  else {
    html += `
      ${Object.entries(insights).map(([key, value]) => {
        if (Array.isArray(value)) {
          return `
            <div class="insight-item">
              <h4>${key.replace(/([A-Z])/g, ' $1').replace(/^./, str => str.toUpperCase())}</h4>
              <ul>${value.map(item => `<li>${item}</li>`).join('')}</ul>
            </div>
          `;
        } else if (typeof value === 'object' && value !== null) {
          return `
            <div class="insight-item">
              <h4>${key.replace(/([A-Z])/g, ' $1').replace(/^./, str => str.toUpperCase())}</h4>
              ${Object.entries(value).map(([subKey, subValue]) => `
                <p><strong>${subKey}:</strong> ${subValue}</p>
              `).join('')}
            </div>
          `;
        } else {
          return `
            <div class="insight-item">
              <h4>${key.replace(/([A-Z])/g, ' $1').replace(/^./, str => str.toUpperCase())}</h4>
              <p>${value}</p>
            </div>
          `;
        }
      }).join('')}
    `;
  }
  
  html += '</div>';
  return html;
}

function getDayFromDate(dateString) {
  // Simple implementation - extract day number
  const dayMatch = dateString.match(/(\d+)(?:th|st|nd|rd)/);
  return dayMatch ? dayMatch[1] : dateString.split(' ')[0] || '';
}

function getMonthFromDate(dateString) {
  // Simple implementation - extract month name
  const monthMatch = dateString.match(/(January|February|March|April|May|June|July|August|September|October|November|December)/);
  return monthMatch ? monthMatch[1].substring(0, 3) : dateString.split(' ')[1] || '';
}

function escapeHTML(str) {
  if (!str) return '';
  return str.toString()
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#039;');
}

// Initialize place carousel similar to store carousel
function initializePlaceCarousel() {
  const carousel = document.querySelector('.place-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.place-carousel-item');
  const dots = carousel.querySelectorAll('.place-pagination-dot');
  let currentIndex = 0;

  function showSlide(index) {
    items.forEach((item, i) => {
      item.classList.remove('active');
      dots[i].classList.remove('active');
      if (i === index) {
        item.classList.add('active');
        dots[i].classList.add('active');
      }
    });
    currentIndex = index;
  }

  function nextSlide() {
    const newIndex = (currentIndex + 1) % items.length;
    showSlide(newIndex);
  }

  function prevSlide() {
    const newIndex = (currentIndex - 1 + items.length) % items.length;
    showSlide(newIndex);
  }

  // Auto-rotate every 5 seconds
  let interval = setInterval(nextSlide, 5000);

  // Pause on hover
  carousel.addEventListener('mouseenter', () => {
    clearInterval(interval);
  });

  carousel.addEventListener('mouseleave', () => {
    interval = setInterval(nextSlide, 5000);
  });
}

function changePlaceCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.place-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.place-carousel-item');
  const dots = carousel.querySelectorAll('.place-pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentPlaceSlide(index) {
  const carousel = event.target.closest('.place-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.place-carousel-item');
  const dots = carousel.querySelectorAll('.place-pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

function closePlacePage() {
  const placePage = document.querySelector('.place-page');
  if (placePage) {
    placePage.remove();
  }
}

function addPlacePageStyles() {
  const style = document.createElement('style');
  style.textContent = `
/* Place Page Container */
.place-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #1a1a1f;
  z-index: 2000;
  overflow-y: auto;
  color: #fff;
  font-family: 'poppins';
}

/* Place Header */
.place-header {
  position: sticky;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  z-index: 10;
  backdrop-filter: blur(12px);
}

.back-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.5em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.back-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.place-header h2 {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  text-align: center;
  flex: 1;
  padding: 0 16px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.header-actions {
  display: flex;
  gap: 8px;
}

.share-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.2em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.share-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

/* Place Content */
.place-content {
  padding: 0 0 100px;
  width: 100%;
  margin: 0 auto;
}

/* Hero Section */
.place-hero {
  position: relative;
  padding: 20px;
}

.place-image-carousel {
  position: relative;
  width: 100%;
  height: 280px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  margin-bottom: 16px;
}

.place-carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.place-carousel-item.active {
  opacity: 1;
}

.place-carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.place-carousel-item.active img:hover {
  transform: scale(1.02);
}

.place-carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.7;
  transition: all 0.3s ease;
  z-index: 5;
}

.place-carousel-control:hover {
  background: rgba(0, 0, 0, 0.9);
  opacity: 1;
  transform: translateY(-50%) scale(1.1);
}

.place-carousel-control.prev {
  left: 16px;
}

.place-carousel-control.next {
  right: 16px;
}

.place-carousel-pagination {
  position: absolute;
  bottom: 16px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.place-pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
}

.place-pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
}

.place-basic-info {
  display: flex;
  gap: 12px;
  margin-top: 16px;
  flex-wrap: wrap;
}

.place-star,
.place-type,
.place-timings {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 12px;
  border-radius: 20px;
  font-size: 0.9em;
  font-weight: 500;
}

.place-star {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
}

.place-type {
  background: rgba(0, 191, 255, 0.1);
  color: #00bfff;
}

.place-timings {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

.established-since {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-top: 12px;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

/* Description Section */
.place-description-section {
  padding: 0 20px 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-description-section h3 {
  margin: 0 0 12px;
  font-size: 1.2em;
  font-weight: 600;
}

.place-description-section p {
  margin: 0;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.8);
}

/* Location Section */
.place-location-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-location-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.locations-grid {
  display: grid;
  gap: 16px;
}

.location-card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.location-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.location-info h4 {
  margin: 0 0 8px;
  font-size: 1.1em;
  font-weight: 600;
}

.location-address,
.location-timings {
  margin: 4px 0;
  display: flex;
  align-items: center;
  gap: 8px;
  color: rgba(255, 255, 255, 0.8);
  font-size: 0.9em;
}

.map-button {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 16px;
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
  border-radius: 8px;
  text-decoration: none;
  font-size: 0.9em;
  font-weight: 500;
  transition: background 0.3s ease;
  margin-top: 8px;
  justify-content: center;
}

.map-button:hover {
  background: rgba(255, 215, 0, 0.2);
}

/* Facilities Section */
.place-facilities-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-facilities-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.facilities-grid {
  display: grid;
  gap: 16px;
}

.facility-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.facility-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.facility-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: #ffd700;
  font-size: 1.2em;
}

.facility-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.facility-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

/* Events Section */
.place-events-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-events-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.events-grid {
  display: grid;
  gap: 16px;
}

.event-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.event-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.event-date {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: #ffd700;
  font-weight: 600;
}

.event-day {
  font-size: 1.2em;
  line-height: 1;
}

.event-month {
  font-size: 0.8em;
  line-height: 1;
  text-transform: uppercase;
}

.event-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.event-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

/* Contact Section */
.place-contact-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-contact-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.contact-methods {
  display: grid;
  gap: 12px;
  margin-bottom: 20px;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 8px;
  color: #fff;
  text-decoration: none;
  transition: background 0.3s ease;
}

.contact-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.social-links {
  display: flex;
  gap: 16px;
  margin-top: 20px;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.05);
  color: #fff;
  font-size: 1.2em;
  transition: transform 0.3s ease, background 0.3s ease;
}

.social-link:hover {
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.1);
}

/* Action Buttons */
.place-actions {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  z-index: 10;
  backdrop-filter: blur(12px);
}

.primary-action,
.secondary-action {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 14px;
  border-radius: 8px;
  font-weight: 500;
  font-size: 0.95em;
  cursor: pointer;
  transition: transform 0.3s ease, background 0.3s ease;
}

.primary-action {
  background: var(--primary-color, #ffd700);
  color: #000;
  border: none;
}

.primary-action:hover {
  background: #ffd700;
  transform: translateY(-2px);
}

.secondary-action {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  border: none;
}

.secondary-action:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

/* Responsive Styles */
@media (min-width: 768px) {
  .place-header {
    padding: 20px 30px;
  }
  
  .place-header h2 {
    font-size: 1.4em;
  }
  
  .place-content {
    max-width: 90%;
    margin: 0 auto;
  }
  
  .place-hero {
    padding: 30px;
  }
  
  .place-image-carousel {
    height: 350px;
    border-radius: 20px;
  }
  
  .place-basic-info {
    gap: 16px;
    margin-top: 24px;
  }
  
  .place-star,
  .place-type,
  .place-timings {
    padding: 10px 16px;
    font-size: 1em;
  }
  
  .place-description-section,
  .place-location-section,
  .place-facilities-section,
  .place-events-section,
  .place-contact-section {
    padding: 0 30px 30px;
  }
  
  .place-description-section h3,
  .place-location-section h3,
  .place-facilities-section h3,
  .place-events-section h3,
  .place-contact-section h3 {
    font-size: 1.3em;
    margin-bottom: 16px;
  }
  
  .place-description-section p {
    font-size: 1em;
    line-height: 1.6;
  }
  
  .locations-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .facilities-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .events-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .place-actions {
    padding: 20px 30px;
    max-width: 90%;
    margin: 0 auto;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 12px 12px 0 0;
  }
  
  .primary-action,
  .secondary-action {
    padding: 14px;
    font-size: 1em;
  }
}

@media (min-width: 1024px) {
  .place-content {
    max-width: 900px;
    margin: 0 auto;
    padding: 0 0 120px;
  }
  
  .place-header {
    padding: 22px 32px;
  }
  
  .place-header h2 {
    font-size: 1.5em;
  }
  
  .place-hero {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
    padding: 30px;
    align-items: center;
  }
  
  .place-image-carousel {
    height: 400px;
    border-radius: 20px;
  }
  
  .place-hero-content {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  
  .place-name {
    font-size: 1.8em;
    font-weight: 600;
    margin: 0;
  }
  
  .place-basic-info {
    margin-top: 0;
  }
  
  .place-description-section {
    padding: 0 30px 30px;
    max-width: 85%;
    margin: 0 auto;
  }
  
  .place-description-section h3 {
    font-size: 1.5em;
  }
  
  .place-description-section p {
    font-size: 1.05em;
    line-height: 1.7;
  }
  
  .place-location-section,
  .place-facilities-section,
  .place-events-section,
  .place-contact-section {
    padding: 30px;
  }
  
  .place-location-section h3,
  .place-facilities-section h3,
  .place-events-section h3,
  .place-contact-section h3 {
    font-size: 1.5em;
  }
  
  .locations-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .facilities-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .events-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .social-links {
    gap: 18px;
  }
  
  .social-link {
    width: 44px;
    height: 44px;
    font-size: 1.3em;
  }
  
  .place-actions {
    max-width: 700px;
    left: 50%;
    transform: translateX(-50%);
  }
  
  .primary-action,
  .secondary-action {
    padding: 16px;
    font-size: 1.05em;
  }
}

@media (min-width: 1440px) {
  .place-content {
    max-width: 1000px;
  }
  
  .place-hero {
    grid-template-columns: 3fr 2fr;
  }
  
  .place-image-carousel {
    height: 450px;
  }
  
  .place-name {
    font-size: 2em;
  }
  
  .place-description-section {
    max-width: 80%;
  }
  
  .locations-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .facilities-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .events-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .place-actions {
    max-width: 800px;
    left: 50%;
    transform: translateX(-50%);
  }
}

    /* Category Insights Section */
    .category-insights-section {
      padding: 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }
    
    .category-insights-section h3 {
      margin: 0 0 16px;
      font-size: 1.2em;
      font-weight: 600;
      color: var(--primary-color, #ffd700);
    }
    
    .insights-grid {
      display: grid;
      gap: 16px;
    }
    
    .insight-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 12px;
      padding: 16px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    
    .insight-item:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
    }
    
    .insight-item h4 {
      margin: 0 0 8px;
      font-size: 1em;
      font-weight: 600;
      color: #fff;
    }
    
    .insight-item p, .insight-item li {
      margin: 4px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 0.9em;
      line-height: 1.5;
    }
    
    .insight-item ul {
      padding-left: 20px;
      margin: 8px 0;
    }
    
    @media (min-width: 768px) {
      .insights-grid {
        grid-template-columns: repeat(2, 1fr);
        gap: 20px;
      }
      
      .category-insights-section {
        padding: 30px;
      }
    }
    
    @media (min-width: 1024px) {
      .insights-grid {
        grid-template-columns: repeat(3, 1fr);
      }
    }

  `;
  document.head.appendChild(style);
}

// Helper functions for place page
function getFacilityIcon(facilityName) {
  const icons = {
    'Parking': 'parking',
    'Wi-Fi': 'wifi',
    'Restrooms': 'restroom',
    'Food Stalls': 'utensils',
    'Guided Tours': 'map-marked-alt',
    'Accessibility': 'wheelchair',
    'Souvenir Shop': 'shopping-bag',
    'First Aid': 'first-aid',
    'Drinking Water': 'tint',
    'Picnic Area': 'umbrella-beach'
  };
  
  return icons[facilityName] || 'check-circle';
}

function getDayFromDate(dateString) {
  // Extract day from date string (simple implementation)
  const parts = dateString.split(/[\s,]+/);
  return parts[0] || '';
}

function getMonthFromDate(dateString) {
  // Extract month from date string (simple implementation)
  const parts = dateString.split(/[\s,]+/);
  return parts[1] || '';
}

// Helper function to escape HTML special characters
function escapeHTML(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

// Function to filter places based on category
function filterPlaces(category) {
  const placesGrid = document.getElementById('placesGrid');
  let filteredData = publicPlacesData;

  if (category !== 'All') {
    filteredData = filteredData.filter(place => place.category === category);
  }

  placesGrid.innerHTML = renderPlaceCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

// Additional placeholder functions (you might want to implement these)
function selectPlace(category) {
  console.log(`Selected place in category: ${category}`);
}

function selectPlace(category) {
  // Optional: Add any specific action when a place card is clicked
  console.log(`Selected place in category: ${category}`);
}

const serviceProvidersData = [
  // Delhi - Plumber
  {
    category: "Plumber",
    metropolis: "Delhi",
    details: {
      name: "Delhi Plumbing Services",
      cardImages: [
        "https://www.serviceonwheel.com/uploads/service/834431670584630.jpg",
        "https://example.com/plumber-card2.jpg"
      ],
      providerImages: [
        "https://5.imimg.com/data5/PX/TS/AC/SELLER-8453614/plumber-500x500.jpg",
        "https://example.com/plumber2.jpg",
        "https://example.com/plumber3.jpg"
      ],
      description: "Professional plumbing services for homes and businesses with 24/7 emergency support.",
      address: "12, Karol Bagh, Delhi - 110005",
      timings: "24/7 Emergency Service",
      entryFee: "Visiting Fees: ₹500",
      mapLink: "https://maps.example.com/plumber",
      website: "https://www.delhiplumbing.com",
      social: {
        facebook: "https://facebook.com/delhiplumbing",
        twitter: "https://twitter.com/delhiplumbing",
        instagram: "https://instagram.com/delhiplumbing"
      },
      facilities: [
        { name: "Emergency Service", description: "Available 24 hours, 7 days a week" },
        { name: "Guarantee", description: "1-year guarantee on all repairs" },
        { name: "Equipment", description: "Modern tools and diagnostic equipment" }
      ],
      services: [
        { 
          name: "Pipe Repair", 
          price: "₹800-₹2000", 
          image: "https://example.com/pipe-repair.jpg",
          description: "Fix for leaking or burst pipes",
          variants: [
            { name: "Basic Repair", price: "₹800" },
            { name: "Standard Repair", price: "₹1200" },
            { name: "Complex Repair", price: "₹2000" }
          ]
        },
        { 
          name: "Drain Cleaning", 
          price: "₹500-₹1500", 
          image: "https://example.com/drain-cleaning.jpg",
          description: "Unclog sinks, showers, and toilets",
          variants: [
            { name: "Basic Cleaning", price: "₹500" },
            { name: "Standard Cleaning", price: "₹800" },
            { name: "Deep Cleaning", price: "₹1500" }
          ]
        },
        { 
          name: "Water Heater", 
          price: "₹2000-₹5000", 
          image: "https://example.com/water-heater.jpg",
          description: "Installation and repair",
          variants: [
            { name: "Repair", price: "₹2000" },
            { name: "Standard Installation", price: "₹3500" },
            { name: "Premium Installation", price: "₹5000" }
          ]
        }
      ],
      promos: [
        { name: "First Time Customer", description: "10% off your first service", code: "PLUMB10" }
      ],
      support: {
        email: "support@delhiplumbing.com",
        phone: "+91-9876543210",
        whatsapp: "+91-7869809022",
        primaryContact: "whatsapp"
      },
      rating: "4.8",
      reviews: "1200+",
      established: "2010",
      tags: ["24/7 Service", "Licensed", "Free Estimates"]
    }
  },

  // Mumbai - Electrician
  {
    category: "Electrician",
    metropolis: "Mumbai",
    details: {
      name: "Mumbai Electrical Solutions",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/QH/BN/JA/3033603/electrician-service-500x500.jpg",
        "https://example.com/electrician-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1212746473/photo/electrician-working-on-switchboard-with-wires.jpg",
        "https://example.com/electrician2.jpg"
      ],
      description: "Certified electricians providing reliable electrical services for residential and commercial properties.",
      address: "45, Andheri East, Mumbai - 400069",
      timings: "8:00 AM - 10:00 PM",
      entryFee: "Visiting Fees: ₹300",
      mapLink: "https://maps.example.com/electrician",
      website: "https://www.mumbaielectrical.com",
      social: {
        facebook: "https://facebook.com/mumbaielectrical",
        instagram: "https://instagram.com/mumbaielectrical"
      },
      facilities: [
        { name: "Safety Certified", description: "All technicians are safety certified" },
        { name: "Warranty", description: "6 months warranty on all repairs" },
        { name: "Free Inspection", description: "First inspection free for new customers" }
      ],
      services: [
        { 
          name: "Wiring Installation", 
          price: "₹1500-₹5000", 
          image: "https://example.com/wiring.jpg",
          description: "Complete home/office wiring solutions",
          variants: [
            { name: "Basic Wiring", price: "₹1500" },
            { name: "Standard Wiring", price: "₹3000" },
            { name: "Premium Wiring", price: "₹5000" }
          ]
        },
        { 
          name: "Switchboard Repair", 
          price: "₹400-₹1200", 
          image: "https://example.com/switchboard.jpg",
          description: "Faulty switchboard diagnosis and repair",
          variants: [
            { name: "Basic Repair", price: "₹400" },
            { name: "Standard Repair", price: "₹800" },
            { name: "Complex Repair", price: "₹1200" }
          ]
        },
        { 
          name: "Light Fixture Installation", 
          price: "₹200-₹800", 
          image: "https://example.com/light-fixture.jpg",
          description: "Installation of various light fixtures",
          variants: [
            { name: "Basic Fixture", price: "₹200" },
            { name: "Standard Fixture", price: "₹500" },
            { name: "Premium Fixture", price: "₹800" }
          ]
        }
      ],
      promos: [
        { name: "Senior Citizen Discount", description: "15% off for senior citizens", code: "SENIOR15" }
      ],
      support: {
        email: "help@mumbaielectrical.com",
        phone: "+91-8765432109",
        whatsapp: "+91-9876543210",
        primaryContact: "phone"
      },
      rating: "4.7",
      reviews: "950+",
      established: "2015",
      tags: ["Certified", "Same Day Service", "Free Estimates"]
    }
  },

  // Bangalore - Carpenter
  {
    category: "Carpenter",
    metropolis: "Bangalore",
    details: {
      name: "Bangalore Woodworks",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/carpenter-service-500x500.jpg",
        "https://example.com/carpenter-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1202203803/photo/carpenter-working-with-wood.jpg",
        "https://example.com/carpenter2.jpg"
      ],
      description: "Skilled carpenters providing custom furniture and woodwork solutions with premium materials.",
      address: "78, Koramangala, Bangalore - 560034",
      timings: "9:00 AM - 7:00 PM",
      entryFee: "Visiting Fees: ₹400",
      mapLink: "https://maps.example.com/carpenter",
      website: "https://www.bangalorewoodworks.com",
      social: {
        facebook: "https://facebook.com/bangalorewoodworks",
        instagram: "https://instagram.com/bangalorewoodworks"
      },
      facilities: [
        { name: "Custom Designs", description: "Tailored to your specific requirements" },
        { name: "Quality Materials", description: "Only premium wood and hardware used" },
        { name: "On-site Work", description: "Can work at your location if needed" }
      ],
      services: [
        { 
          name: "Furniture Making", 
          price: "₹5000-₹25000", 
          image: "https://example.com/furniture.jpg",
          description: "Custom furniture as per your design",
          variants: [
            { name: "Basic Furniture", price: "₹5000" },
            { name: "Standard Furniture", price: "₹12000" },
            { name: "Premium Furniture", price: "₹25000" }
          ]
        },
        { 
          name: "Wardrobe Installation", 
          price: "₹3000-₹15000", 
          image: "https://example.com/wardrobe.jpg",
          description: "Built-in or standalone wardrobes",
          variants: [
            { name: "Basic Wardrobe", price: "₹3000" },
            { name: "Standard Wardrobe", price: "₹8000" },
            { name: "Premium Wardrobe", price: "₹15000" }
          ]
        },
        { 
          name: "Door Repair/Installation", 
          price: "₹1500-₹6000", 
          image: "https://example.com/door.jpg",
          description: "All types of wooden doors",
          variants: [
            { name: "Basic Repair", price: "₹1500" },
            { name: "Standard Installation", price: "₹3500" },
            { name: "Premium Installation", price: "₹6000" }
          ]
        }
      ],
      promos: [
        { name: "First Project Discount", description: "5% off on first project above ₹10000", code: "WOOD5" }
      ],
      support: {
        email: "contact@bangalorewoodworks.com",
        phone: "+91-7654321098",
        whatsapp: "+91-8765432109",
        primaryContact: "whatsapp"
      },
      rating: "4.9",
      reviews: "850+",
      established: "2018",
      tags: ["Custom Work", "Premium Materials", "Free Consultation"]
    }
  },

  // Delhi - AC Repair
  {
    category: "AC Repair",
    metropolis: "Delhi",
    details: {
      name: "Delhi Cool Solutions",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/ac-repair-service-500x500.jpg",
        "https://example.com/ac-repair-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1321462048/photo/service-engineer-repairing-air-conditioner.jpg",
        "https://example.com/ac-tech2.jpg"
      ],
      description: "Expert AC repair and maintenance services for all brands with genuine spare parts.",
      address: "23, Connaught Place, Delhi - 110001",
      timings: "7:00 AM - 10:00 PM",
      entryFee: "Visiting Fees: ₹600",
      mapLink: "https://maps.example.com/ac-repair",
      website: "https://www.delhicoolsolutions.com",
      social: {
        facebook: "https://facebook.com/delhicoolsolutions",
        twitter: "https://twitter.com/delhicool"
      },
      facilities: [
        { name: "Brand Specialists", description: "Technicians trained for specific brands" },
        { name: "Genuine Parts", description: "Only manufacturer-approved spare parts" },
        { name: "Annual Maintenance", description: "Discounts on AMC contracts" }
      ],
      services: [
        { 
          name: "AC Repair", 
          price: "₹800-₹3000", 
          image: "https://example.com/ac-repair.jpg",
          description: "Diagnosis and repair of all AC issues",
          variants: [
            { name: "Basic Repair", price: "₹800" },
            { name: "Standard Repair", price: "₹1500" },
            { name: "Complex Repair", price: "₹3000" }
          ]
        },
        { 
          name: "AC Gas Refill", 
          price: "₹1200-₹2500", 
          image: "https://example.com/gas-refill.jpg",
          description: "Complete gas refill with leak testing",
          variants: [
            { name: "1 Ton AC", price: "₹1200" },
            { name: "1.5 Ton AC", price: "₹1800" },
            { name: "2 Ton AC", price: "₹2500" }
          ]
        },
        { 
          name: "AC Installation", 
          price: "₹1500-₹4000", 
          image: "https://example.com/ac-install.jpg",
          description: "Professional installation service",
          variants: [
            { name: "Window AC", price: "₹1500" },
            { name: "Split AC (1-1.5 Ton)", price: "₹2500" },
            { name: "Split AC (2 Ton)", price: "₹4000" }
          ]
        }
      ],
      promos: [
        { name: "Summer Special", description: "10% off on all services in May-June", code: "COOL10" }
      ],
      support: {
        email: "support@delhicoolsolutions.com",
        phone: "+91-9876543211",
        whatsapp: "+91-9876543212",
        primaryContact: "phone"
      },
      rating: "4.6",
      reviews: "1100+",
      established: "2012",
      tags: ["Brand Specialists", "Emergency Service", "AMC Available"]
    }
  },

  // Mumbai - Pest Control
  {
    category: "Pest Control",
    metropolis: "Mumbai",
    details: {
      name: "Mumbai Pest Free",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/pest-control-service-500x500.jpg",
        "https://example.com/pest-control-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1216221202/photo/pest-control-technician-spraying-insecticide-to-kill-cockroach.jpg",
        "https://example.com/pest-control2.jpg"
      ],
      description: "Effective pest control solutions using safe, eco-friendly methods for homes and businesses.",
      address: "34, Bandra West, Mumbai - 400050",
      timings: "8:00 AM - 8:00 PM",
      entryFee: "Visiting Fees: ₹400",
      mapLink: "https://maps.example.com/pest-control",
      website: "https://www.mumbaipestfree.com",
      social: {
        facebook: "https://facebook.com/mumbaipestfree",
        instagram: "https://instagram.com/mumbaipestfree"
      },
      facilities: [
        { name: "Eco-Friendly", description: "Safe for children and pets" },
        { name: "Guaranteed Results", description: "Free follow-up if pests return" },
        { name: "Commercial Solutions", description: "Special packages for offices and restaurants" }
      ],
      services: [
        { 
          name: "General Pest Control", 
          price: "₹800-₹2000", 
          image: "https://example.com/general-pest.jpg",
          description: "For cockroaches, ants, spiders, etc.",
          variants: [
            { name: "1 BHK", price: "₹800" },
            { name: "2 BHK", price: "₹1200" },
            { name: "3 BHK", price: "₹2000" }
          ]
        },
        { 
          name: "Termite Control", 
          price: "₹1500-₹3500", 
          image: "https://example.com/termite.jpg",
          description: "Complete termite treatment",
          variants: [
            { name: "Basic Treatment", price: "₹1500" },
            { name: "Standard Treatment", price: "₹2500" },
            { name: "Premium Treatment", price: "₹3500" }
          ]
        },
        { 
          name: "Rodent Control", 
          price: "₹1000-₹2500", 
          image: "https://example.com/rodent.jpg",
          description: "Rat and mouse elimination",
          variants: [
            { name: "Basic Service", price: "₹1000" },
            { name: "Standard Service", price: "₹1800" },
            { name: "Premium Service", price: "₹2500" }
          ]
        }
      ],
      promos: [
        { name: "Package Deal", description: "15% off when booking 3 services", code: "PESTFREE15" }
      ],
      support: {
        email: "info@mumbaipestfree.com",
        phone: "+91-8765432111",
        whatsapp: "+91-8765432112",
        primaryContact: "whatsapp"
      },
      rating: "4.5",
      reviews: "800+",
      established: "2016",
      tags: ["Eco-Friendly", "Guaranteed", "Commercial Services"]
    }
  },

  // Bangalore - Home Cleaning
  {
    category: "Home Cleaning",
    metropolis: "Bangalore",
    details: {
      name: "Bangalore Clean Homes",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/home-cleaning-service-500x500.jpg",
        "https://example.com/cleaning-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1018141890/photo/housekeeper-holding-cleaning-products-and-bucket.jpg",
        "https://example.com/cleaner2.jpg"
      ],
      description: "Professional home cleaning services with trained staff and eco-friendly products.",
      address: "56, Indiranagar, Bangalore - 560038",
      timings: "7:00 AM - 9:00 PM",
      entryFee: "No visiting fees",
      mapLink: "https://maps.example.com/home-cleaning",
      website: "https://www.bangalorecleanhomes.com",
      social: {
        facebook: "https://facebook.com/bangalorecleanhomes",
        instagram: "https://instagram.com/bangalorecleanhomes"
      },
      facilities: [
        { name: "Eco-Friendly Products", description: "Safe for children and pets" },
        { name: "Trained Staff", description: "Background verified professionals" },
        { name: "Flexible Scheduling", description: "One-time or regular cleaning" }
      ],
      services: [
        { 
          name: "Basic Home Cleaning", 
          price: "₹1000-₹2500", 
          image: "https://example.com/basic-cleaning.jpg",
          description: "General cleaning of all rooms",
          variants: [
            { name: "1 BHK", price: "₹1000" },
            { name: "2 BHK", price: "₹1500" },
            { name: "3 BHK", price: "₹2500" }
          ]
        },
        { 
          name: "Deep Cleaning", 
          price: "₹2000-₹5000", 
          image: "https://example.com/deep-cleaning.jpg",
          description: "Thorough cleaning including kitchens and bathrooms",
          variants: [
            { name: "1 BHK", price: "₹2000" },
            { name: "2 BHK", price: "₹3500" },
            { name: "3 BHK", price: "₹5000" }
          ]
        },
        { 
          name: "Post-Renovation Cleaning", 
          price: "₹3000-₹8000", 
          image: "https://example.com/renovation-cleaning.jpg",
          description: "Specialized cleaning after construction work",
          variants: [
            { name: "1 BHK", price: "₹3000" },
            { name: "2 BHK", price: "₹5000" },
            { name: "3 BHK", price: "₹8000" }
          ]
        }
      ],
      promos: [
        { name: "Monthly Package", description: "20% off on 4 cleanings per month", code: "CLEAN20" }
      ],
      support: {
        email: "bookings@bangalorecleanhomes.com",
        phone: "+91-7654321122",
        whatsapp: "+91-7654321123",
        primaryContact: "phone"
      },
      rating: "4.8",
      reviews: "1300+",
      established: "2017",
      tags: ["Eco-Friendly", "Verified Staff", "Flexible Packages"]
    }
  },

  // Delhi - Appliance Repair
  {
    category: "Appliance Repair",
    metropolis: "Delhi",
    details: {
      name: "Delhi Appliance Care",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/appliance-repair-service-500x500.jpg",
        "https://example.com/appliance-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1212746473/photo/electrician-working-on-switchboard-with-wires.jpg",
        "https://example.com/technician2.jpg"
      ],
      description: "Expert repair services for all home appliances with genuine spare parts and warranty.",
      address: "67, Rajouri Garden, Delhi - 110027",
      timings: "8:00 AM - 8:00 PM",
      entryFee: "Visiting Fees: ₹400",
      mapLink: "https://maps.example.com/appliance-repair",
      website: "https://www.delhiappliancecare.com",
      social: {
        facebook: "https://facebook.com/delhiappliancecare",
        twitter: "https://twitter.com/delhiappliance"
      },
      facilities: [
        { name: "Multi-Brand Expertise", description: "Trained on all major brands" },
        { name: "Warranty on Repairs", description: "90 days warranty on all repairs" },
        { name: "Doorstep Service", description: "Repairs at your location" }
      ],
      services: [
        { 
          name: "Refrigerator Repair", 
          price: "₹1000-₹3500", 
          image: "https://example.com/fridge-repair.jpg",
          description: "All refrigerator issues including gas refill",
          variants: [
            { name: "Basic Repair", price: "₹1000" },
            { name: "Standard Repair", price: "₹2000" },
            { name: "Complex Repair", price: "₹3500" }
          ]
        },
        { 
          name: "Washing Machine Repair", 
          price: "₹800-₹3000", 
          image: "https://example.com/washing-machine.jpg",
          description: "All types of washing machine repairs",
          variants: [
            { name: "Top Load", price: "₹800" },
            { name: "Front Load", price: "₹1500" },
            { name: "Commercial Machine", price: "₹3000" }
          ]
        },
        { 
          name: "Microwave Repair", 
          price: "₹500-₹2000", 
          image: "https://example.com/microwave.jpg",
          description: "Diagnosis and repair of microwave ovens",
          variants: [
            { name: "Basic Repair", price: "₹500" },
            { name: "Standard Repair", price: "₹1200" },
            { name: "Complex Repair", price: "₹2000" }
          ]
        }
      ],
      promos: [
        { name: "Multiple Appliance Discount", description: "10% off when repairing 2+ appliances", code: "APP10" }
      ],
      support: {
        email: "support@delhiappliancecare.com",
        phone: "+91-9876543222",
        whatsapp: "+91-9876543223",
        primaryContact: "whatsapp"
      },
      rating: "4.7",
      reviews: "1050+",
      established: "2014",
      tags: ["Multi-Brand", "Warranty", "Same Day Service"]
    }
  },

  // Bhopal - Painter
  {
    category: "Painter",
    metropolis: "Bhopal",
    details: {
      name: "Bhopal Perfect Painters",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/painting-service-500x500.jpg",
        "https://example.com/painter-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1202203803/photo/painter-working-on-wall.jpg",
        "https://example.com/painter2.jpg"
      ],
      description: "Professional painting services for homes and offices with premium quality paints.",
      address: "12, Arera Colony, Bhopal - 462016",
      timings: "8:00 AM - 7:00 PM",
      entryFee: "Consultation Fee: ₹300",
      mapLink: "https://maps.example.com/bhopal-painters",
      website: "https://www.bhopalperfectpainters.com",
      social: {
        facebook: "https://facebook.com/bhopalperfectpainters",
        instagram: "https://instagram.com/bhopalperfectpainters"
      },
      facilities: [
        { name: "Premium Paints", description: "Asian Paints, Berger, Nerolac available" },
        { name: "Wall Preparation", description: "Proper surface preparation included" },
        { name: "Clean Work", description: "Minimal mess during and after work" }
      ],
      services: [
        {
          name: "Interior Painting",
          price: "₹8-₹15 per sq.ft.",
          image: "https://example.com/interior-painting.jpg",
          description: "Complete interior wall painting",
          variants: [
            { name: "Economy Paint", price: "₹8/sq.ft." },
            { name: "Standard Paint", price: "₹12/sq.ft." },
            { name: "Premium Paint", price: "₹15/sq.ft." }
          ]
        },
        {
          name: "Exterior Painting",
          price: "₹12-₹20 per sq.ft.",
          image: "https://example.com/exterior-painting.jpg",
          description: "Weather-resistant exterior painting",
          variants: [
            { name: "Economy Paint", price: "₹12/sq.ft." },
            { name: "Standard Paint", price: "₹16/sq.ft." },
            { name: "Premium Paint", price: "₹20/sq.ft." }
          ]
        }
      ],
      rating: "4.6",
      reviews: "650+",
      established: "2015"
    }
  },

  // Indore - Car Repair
  {
    category: "Car Repair",
    metropolis: "Indore",
    details: {
      name: "Indore Auto Care",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/car-repair-service-500x500.jpg",
        "https://example.com/car-repair-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1212746473/photo/mechanic-working-on-car.jpg",
        "https://example.com/mechanic2.jpg"
      ],
      description: "Complete car repair and maintenance services with genuine spare parts.",
      address: "45, Vijay Nagar, Indore - 452010",
      timings: "8:00 AM - 8:00 PM",
      entryFee: "Diagnostic Fee: ₹500",
      mapLink: "https://maps.example.com/indore-autocare",
      website: "https://www.indoreautocare.com",
      social: {
        facebook: "https://facebook.com/indoreautocare",
        instagram: "https://instagram.com/indoreautocare"
      },
      facilities: [
        { name: "Computer Diagnostics", description: "Advanced diagnostic equipment" },
        { name: "Genuine Parts", description: "OEM and OEM-equivalent parts available" },
        { name: "Pickup/Drop", description: "Available for major repairs" }
      ],
      services: [
        {
          name: "General Service",
          price: "₹1500-₹4000",
          image: "https://example.com/car-service.jpg",
          description: "Periodic maintenance service",
          variants: [
            { name: "Hatchback", price: "₹1500" },
            { name: "Sedan", price: "₹2500" },
            { name: "SUV", price: "₹4000" }
          ]
        },
        {
          name: "AC Repair",
          price: "₹1000-₹5000",
          image: "https://example.com/car-ac.jpg",
          description: "Car air conditioning service",
          variants: [
            { name: "Gas Refill", price: "₹1000" },
            { name: "AC Repair", price: "₹2500" },
            { name: "Complete Overhaul", price: "₹5000" }
          ]
        }
      ],
      rating: "4.7",
      reviews: "720+",
      established: "2016"
    }
  },

  // Chennai - Home Appliances
  {
    category: "Home Appliances",
    metropolis: "Chennai",
    details: {
      name: "Chennai Appliance Masters",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/appliance-repair-service-500x500.jpg",
        "https://example.com/appliance-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1212746473/photo/technician-repairing-washing-machine.jpg",
        "https://example.com/technician2.jpg"
      ],
      description: "Expert repair services for all home appliances with warranty.",
      address: "78, T. Nagar, Chennai - 600017",
      timings: "9:00 AM - 8:00 PM",
      entryFee: "Service Charge: ₹300",
      mapLink: "https://maps.example.com/chennai-appliance",
      website: "https://www.chennaiappliancemasters.com",
      social: {
        facebook: "https://facebook.com/chennaiappliancemasters",
        instagram: "https://instagram.com/chennaiappliancemasters"
      },
      facilities: [
        { name: "Same Day Service", description: "For most repairs" },
        { name: "90-Day Warranty", description: "On all repairs" },
        { name: "Brand Specialists", description: "For all major brands" }
      ],
      services: [
        {
          name: "Refrigerator Repair",
          price: "₹800-₹3500",
          image: "https://example.com/fridge-repair.jpg",
          description: "All refrigerator issues",
          variants: [
            { name: "Basic Repair", price: "₹800" },
            { name: "Standard Repair", price: "₹1800" },
            { name: "Complex Repair", price: "₹3500" }
          ]
        },
        {
          name: "Washing Machine Repair",
          price: "₹700-₹3000",
          image: "https://example.com/washing-machine.jpg",
          description: "All types of washing machines",
          variants: [
            { name: "Top Load", price: "₹700" },
            { name: "Front Load", price: "₹1500" },
            { name: "Commercial", price: "₹3000" }
          ]
        }
      ],
      rating: "4.5",
      reviews: "850+",
      established: "2014"
    }
  },

  // Pune - Pest Control
  {
    category: "Pest Control",
    metropolis: "Pune",
    details: {
      name: "Pune Pest Free",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/pest-control-service-500x500.jpg",
        "https://example.com/pest-control-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1216221202/photo/pest-control-technician-spraying.jpg",
        "https://example.com/pest-control2.jpg"
      ],
      description: "Effective pest control solutions using safe methods.",
      address: "34, Kothrud, Pune - 411038",
      timings: "8:00 AM - 8:00 PM",
      entryFee: "Inspection Fee: ₹300",
      mapLink: "https://maps.example.com/pune-pestfree",
      website: "https://www.punepestfree.com",
      social: {
        facebook: "https://facebook.com/punepestfree",
        instagram: "https://instagram.com/punepestfree"
      },
      facilities: [
        { name: "Child/Pet Safe", description: "Eco-friendly chemicals" },
        { name: "Guaranteed Results", description: "Free follow-up if needed" },
        { name: "Commercial Solutions", description: "For offices and restaurants" }
      ],
      services: [
        {
          name: "General Pest Control",
          price: "₹800-₹2000",
          image: "https://example.com/general-pest.jpg",
          description: "For cockroaches, ants, etc.",
          variants: [
            { name: "1 BHK", price: "₹800" },
            { name: "2 BHK", price: "₹1200" },
            { name: "3 BHK", price: "₹2000" }
          ]
        }
      ],
      rating: "4.6",
      reviews: "780+",
      established: "2017"
    }
  },

  // Ujjain - Electrician
  {
    category: "Electrician",
    metropolis: "Ujjain",
    details: {
      name: "Ujjain Electrical Works",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/QH/BN/JA/3033603/electrician-service-500x500.jpg",
        "https://example.com/electrician-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1212746473/photo/electrician-working.jpg",
        "https://example.com/electrician2.jpg"
      ],
      description: "Certified electricians for all residential and commercial needs.",
      address: "56, Freeganj, Ujjain - 456010",
      timings: "9:00 AM - 8:00 PM",
      entryFee: "Visiting Charge: ₹200",
      mapLink: "https://maps.example.com/ujjain-electrical",
      website: "https://www.ujjainelectricalworks.com",
      social: {
        facebook: "https://facebook.com/ujjainelectricalworks"
      },
      facilities: [
        { name: "Safety First", description: "All safety protocols followed" },
        { name: "Quality Materials", description: "Finolex, Havells, etc." },
        { name: "Emergency Service", description: "24/7 for critical issues" }
      ],
      services: [
        {
          name: "Wiring Installation",
          price: "₹1200-₹4500",
          image: "https://example.com/wiring.jpg",
          description: "Complete home/office wiring",
          variants: [
            { name: "Basic Wiring", price: "₹1200" },
            { name: "Standard Wiring", price: "₹2500" },
            { name: "Premium Wiring", price: "₹4500" }
          ]
        }
      ],
      rating: "4.4",
      reviews: "580+",
      established: "2018"
    }
  },

  // Hyderabad - AC Repair
  {
    category: "AC Repair",
    metropolis: "Hyderabad",
    details: {
      name: "Hyderabad Cool Comfort",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/ac-repair-service-500x500.jpg",
        "https://example.com/ac-repair-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1321462048/photo/ac-technician-working.jpg",
        "https://example.com/ac-tech2.jpg"
      ],
      description: "Expert AC repair and maintenance for all brands.",
      address: "23, Banjara Hills, Hyderabad - 500034",
      timings: "7:00 AM - 9:00 PM",
      entryFee: "Service Charge: ₹500",
      mapLink: "https://maps.example.com/hyderabad-ac",
      website: "https://www.hyderabadcoolcomfort.com",
      social: {
        facebook: "https://facebook.com/hyderabadcoolcomfort",
        instagram: "https://instagram.com/hyderabadcoolcomfort"
      },
      facilities: [
        { name: "Brand Specialists", description: "For all major AC brands" },
        { name: "Genuine Parts", description: "Manufacturer-approved spares" },
        { name: "Annual Contracts", description: "Special AMC rates" }
      ],
      services: [
        {
          name: "AC Service",
          price: "₹600-₹1200",
          image: "https://example.com/ac-service.jpg",
          description: "Regular maintenance service",
          variants: [
            { name: "1 Ton", price: "₹600" },
            { name: "1.5 Ton", price: "₹800" },
            { name: "2 Ton", price: "₹1200" }
          ]
        }
      ],
      rating: "4.7",
      reviews: "920+",
      established: "2015"
    }
  },

  // Jaipur - Plumber
  {
    category: "Plumber",
    metropolis: "Jaipur",
    details: {
      name: "Jaipur Plumbing Experts",
      cardImages: [
        "https://www.serviceonwheel.com/uploads/service/834431670584630.jpg",
        "https://example.com/plumber-card2.jpg"
      ],
      providerImages: [
        "https://5.imimg.com/data5/PX/TS/AC/SELLER-8453614/plumber-500x500.jpg",
        "https://example.com/plumber2.jpg"
      ],
      description: "Professional plumbing services with 24/7 emergency support.",
      address: "34, Vaishali Nagar, Jaipur - 302021",
      timings: "24/7 Service",
      entryFee: "Visiting Charge: ₹400",
      mapLink: "https://maps.example.com/jaipur-plumbing",
      website: "https://www.jaipurplumbingexperts.com",
      social: {
        facebook: "https://facebook.com/jaipurplumbingexperts"
      },
      facilities: [
        { name: "Emergency Service", description: "Available 24/7" },
        { name: "Modern Equipment", description: "Latest tools and techniques" },
        { name: "Guaranteed Work", description: "1-year guarantee on repairs" }
      ],
      services: [
        {
          name: "Pipe Repair",
          price: "₹700-₹1800",
          image: "https://example.com/pipe-repair.jpg",
          description: "For leaking or burst pipes",
          variants: [
            { name: "Basic Repair", price: "₹700" },
            { name: "Standard Repair", price: "₹1000" },
            { name: "Complex Repair", price: "₹1800" }
          ]
        }
      ],
      rating: "4.5",
      reviews: "680+",
      established: "2017"
    }
  },

  // Kolkata - Carpenter
  {
    category: "Carpenter",
    metropolis: "Kolkata",
    details: {
      name: "Kolkata Wood Crafts",
      cardImages: [
        "https://5.imimg.com/data5/SELLER/Default/2021/12/CL/IB/HS/3033603/carpenter-service-500x500.jpg",
        "https://example.com/carpenter-card2.jpg"
      ],
      providerImages: [
        "https://media.istockphoto.com/id/1202203803/photo/carpenter-working.jpg",
        "https://example.com/carpenter2.jpg"
      ],
      description: "Skilled carpenters for custom furniture and woodwork.",
      address: "56, Salt Lake, Kolkata - 700091",
      timings: "9:00 AM - 7:00 PM",
      entryFee: "Consultation Fee: ₹300",
      mapLink: "https://maps.example.com/kolkata-woodcrafts",
      website: "https://www.kolkatawoodcrafts.com",
      social: {
        facebook: "https://facebook.com/kolkatawoodcrafts",
        instagram: "https://instagram.com/kolkatawoodcrafts"
      },
      facilities: [
        { name: "Custom Designs", description: "As per your requirements" },
        { name: "Quality Materials", description: "Teak, Sheesham, MDF available" },
        { name: "On-site Work", description: "Work at your location" }
      ],
      services: [
        {
          name: "Furniture Making",
          price: "₹4000-₹20000",
          image: "https://example.com/furniture.jpg",
          description: "Custom furniture pieces",
          variants: [
            { name: "Basic", price: "₹4000" },
            { name: "Standard", price: "₹10000" },
            { name: "Premium", price: "₹20000" }
          ]
        }
      ],
      rating: "4.6",
      reviews: "720+",
      established: "2016"
    }
  }
];

// Global booking state
// Global booking state
let bookingState = {
  items: [],
  provider: null,
  total: 0,
  discountedTotal: 0,  // Add this line
  directBookingItems: null,
  appliedCoupon: null  // Add this to track applied coupon
};

function showProvidersOverlay() {
  let overlay = document.getElementById('providersOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'providersOverlay';
    overlay.className = 'providers-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(serviceProvidersData.map(provider => provider.category))];

  overlay.innerHTML = `
    <div class="providers-content">
      <div class="providers-header">
        <div class="header-title-container">
          <h2>Service Providers</h2>
          <div class="header-subtitle">Find trusted professionals</div>
        </div>
        <div class="header-buttons-container">
          <button class="sort-button" onclick="toggleSortOptions()">
            <i class="fas fa-sliders-h"></i>
          </button>
          <button class="close-providers" onclick="hideProvidersOverlay()">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      
      <div class="sort-options-container" id="sortOptionsContainer">
        <div class="sort-option" onclick="sortProviders('rating')">
          <i class="fas fa-star"></i> Highest Rated
        </div>
        <div class="sort-option" onclick="sortProviders('name')">
          <i class="fas fa-sort-alpha-down"></i> Alphabetical
        </div>
        <div class="sort-option" onclick="sortProviders('distance')">
          <i class="fas fa-map-marker-alt"></i> Nearest
        </div>
      </div>

      <div class="providers-filter-container">
        <div class="providers-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterProviders('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="providers-grid" id="providersGrid">
        ${renderProviderCards(serviceProvidersData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="providersExploreInput" placeholder="Search for plumbers, electricians..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('providersExploreInput');
  const providersGrid = document.getElementById('providersGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performSearch(serviceProvidersData, query, providersGrid, filterButtons, renderProviderCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(serviceProvidersData, exploreInput.value, providersGrid, filterButtons, renderProviderCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(serviceProvidersData, query, providersGrid, filterButtons, renderProviderCards);
  });

  function performSearch(providers, query, gridElement, filterButtons, renderFunction) {
  if (!query.trim()) {
    gridElement.innerHTML = renderFunction(providers);
    // Reset to 'All' filter when search is cleared
    filterButtons.forEach(btn => {
      btn.classList.remove('active');
      if (btn.dataset.category === 'All') btn.classList.add('active');
    });
    return;
  }

  // Normalize and preprocess the query
  const normalizedQuery = query.toLowerCase().trim()
    .replace(/[^\w\s₹]/g, '') // Remove special chars except ₹
    .replace(/\s+/g, ' ');    // Collapse multiple spaces

  // Enhanced extraction with synonyms and broader matching
  const categorySynonyms = {
    'plumb': 'Plumber',
    'pipe': 'Plumber',
    'leak': 'Plumber',
    'drain': 'Plumber',
    'electri': 'Electrician',
    'wiring': 'Electrician',
    'switch': 'Electrician',
    'light': 'Electrician',
    'carpent': 'Carpenter',
    'wood': 'Carpenter',
    'furniture': 'Carpenter',
    'wardrobe': 'Carpenter',
    'ac': 'AC Repair',
    'air cond': 'AC Repair',
    'cooling': 'AC Repair',
    'clean': 'Home Cleaning',
    'housekeep': 'Home Cleaning',
    'maid': 'Home Cleaning',
    'appliance': 'Appliance Repair',
    'fridge': 'Appliance Repair',
    'washing': 'Appliance Repair',
    'microwave': 'Appliance Repair',
    'pest': 'Pest Control',
    'insect': 'Pest Control',
    'termite': 'Pest Control',
    'rodent': 'Pest Control'
  };

  const citySynonyms = {
    'ncr': 'Delhi',
    'bpl': 'Bhopal',
    'idr': 'Indore',
    'new delhi': 'Delhi',
    'bombay': 'Mumbai',
    'bengaluru': 'Bangalore',
    'blr': 'Bangalore'
  };

  // Advanced query parsing with priority scoring
  let categoryMatch, cityMatch, priceMatch, serviceMatch, qualityMatch, ratingMatch;
  let hasNegation = false;

  // Check for negation terms
  hasNegation = /\b(no|not|without|except)\b/.test(normalizedQuery);

  // 1. Category detection with synonyms and fuzzy matching
  const categoryPattern = new RegExp(
    `\\b(${Object.keys(categorySynonyms).concat([
      'plumber', 'electrician', 'carpenter', 
      'ac repair', 'pest control', 
      'home cleaning', 'appliance repair'
    ]).join('|')})\\b`, 'i');
  
  categoryMatch = normalizedQuery.match(categoryPattern);
  if (categoryMatch) {
    const matchedTerm = categoryMatch[0].toLowerCase();
    categoryMatch[0] = categorySynonyms[matchedTerm] || matchedTerm;
    // Capitalize first letter
    categoryMatch[0] = categoryMatch[0].charAt(0).toUpperCase() + categoryMatch[0].slice(1).toLowerCase();
  }

// 2. City detection with synonyms
const cityPattern = new RegExp(
  `\\b(${Object.keys(citySynonyms).concat([
    'delhi', 'mumbai', 'bhopal', 'bangalore',
    'kolkata', 'chennai', 'hyderabad', 'pune', 'ahmedabad',
    'jaipur', 'lucknow', 'kanpur', 'nagpur', 'indore',
    'thane', 'bhubaneswar', 'visakhapatnam', 'vadodara', 'surat',
    'coimbatore', 'patna', 'ranchi', 'guwahati', 'chandigarh',
    'amritsar', 'noida', 'ghaziabad', 'faridabad', 'meerut',
    'rajkot', 'jodhpur', 'madurai', 'nashik', 'aurangabad',
    'vijayawada', 'howrah', 'gwalior', 'ludhiana', 'allahabad',
    'mysore', 'trivandrum', 'dehradun', 'varanasi', 'agartala',
    'jalandhar', 'udaipur', 'bilaspur', 'panaji', 'shimla',
    'tiruchirappalli', 'tirupati', 'jamshedpur', 'kota', 'bareilly'
  ]).join('|')})\\b`, 'i'
);
  
  cityMatch = normalizedQuery.match(cityPattern);
  if (cityMatch) {
    const matchedTerm = cityMatch[0].toLowerCase();
    cityMatch[0] = citySynonyms[matchedTerm] || matchedTerm;
    // Capitalize first letter
    cityMatch[0] = cityMatch[0].charAt(0).toUpperCase() + cityMatch[0].slice(1).toLowerCase();
  }

  // 3. Price detection with multiple formats
  priceMatch = normalizedQuery.match(/(?:rs\.?|₹|inr)\s*(\d+)/i) || 
               normalizedQuery.match(/(\d+)\s*(?:rs|rupees|inr)/i) ||
               normalizedQuery.match(/\b(\d+)\s*(?:price|cost|charge)/i) ||
               normalizedQuery.match(/\b(under|below|less than|up to|above|over|more than)\s*₹?\s*(\d+)/i);

  // 4. Service detection with expanded terms
  const serviceTerms = [
    'wiring', 'pipe', 'drain', 'installation', 'repair', 
    'cleaning', 'control', 'fix', 'unclog', 'heater',
    'gas', 'refill', 'furniture', 'wardrobe', 'door',
    'switch', 'board', 'fixture', 'light', 'rodent',
    'termite', 'deep clean', 'renovation', 'maintenance',
    'leak', 'tap', 'toilet', 'shower', 'sink',
    'circuit', 'fuse', 'outlet', 'socket', 'panel',
    'cabinet', 'shelf', 'table', 'chair', 'bed',
    'cooling', 'heating', 'ventilation', 'filter',
    'cockroach', 'ant', 'spider', 'rat', 'mouse',
    'vacuum', 'mop', 'dust', 'organize', 'tidy',
    'fridge', 'freezer', 'oven', 'stove', 'dishwasher'
  ];
  serviceMatch = normalizedQuery.match(new RegExp(`\\b(${serviceTerms.join('|')})\\b`, 'i'));

  // 5. Quality detection with expanded terms
  qualityMatch = normalizedQuery.match(/(cheap|affordable|budget|low cost|low price|economical|expensive|premium|luxury|high end|high quality)/i);

  // 6. Rating detection
  ratingMatch = normalizedQuery.match(/(\d+(?:\.\d+)?)\s*(?:star|rating)/i) ||
                normalizedQuery.match(/(high|good|excellent|poor|bad)\s*(?:rated|rating|reviews?)/i);

  // 7. Availability detection
  const availabilityMatch = normalizedQuery.match(/(24\/7|emergency|immediate|now|today|urgent)/i);

  // 8. Provider type detection
  const providerTypeMatch = normalizedQuery.match(/(local|professional|expert|certified|licensed)/i);

  // Advanced filtering with scoring system
  const filteredProviders = providers.map(provider => {
    let score = 0;
    let matches = [];
    let mismatches = [];

    // Category matching (high importance)
    if (categoryMatch) {
      const categoryLower = provider.category.toLowerCase();
      if (categoryLower.includes(categoryMatch[0].toLowerCase())) {
        score += 30;
        matches.push(`Category: ${provider.category}`);
      } else {
        mismatches.push(`Category: ${categoryMatch[0]}`);
        return null; // Exclude if category doesn't match
      }
    }

    // City matching (high importance)
    if (cityMatch) {
      const cityLower = provider.metropolis.toLowerCase();
      if (cityLower.includes(cityMatch[0].toLowerCase())) {
        score += 30;
        matches.push(`City: ${provider.metropolis}`);
      } else {
        mismatches.push(`City: ${cityMatch[0]}`);
        return null; // Exclude if city doesn't match
      }
    }

    // Price matching (medium importance)
    if (priceMatch) {
      let price;
      if (priceMatch[2]) { // For "under ₹1000" type patterns
        price = parseInt(priceMatch[2]);
        const comparison = priceMatch[1].toLowerCase();
        
        const priceServices = provider.details.services.filter(service => {
          return service.variants.some(variant => {
            const variantPrice = parseInt(variant.price.replace(/[^\d]/g, ''));
            if (comparison.includes('under') || comparison.includes('below') || 
                comparison.includes('less than') || comparison.includes('up to')) {
              return variantPrice <= price;
            } else if (comparison.includes('above') || comparison.includes('over') || 
                      comparison.includes('more than')) {
              return variantPrice >= price;
            }
            return variantPrice <= price;
          });
        });
        
        if (priceServices.length > 0) {
          score += 20;
          matches.push(`Price: ${comparison} ₹${price}`);
        } else {
          mismatches.push(`Price: ${comparison} ₹${price}`);
        }
      } else {
        price = parseInt(priceMatch[1]);
        const priceServices = provider.details.services.filter(service => {
          return service.variants.some(variant => {
            const variantPrice = parseInt(variant.price.replace(/[^\d]/g, ''));
            return variantPrice <= price;
          });
        });
        
        if (priceServices.length > 0) {
          score += 20;
          matches.push(`Price: ≤₹${price}`);
        } else {
          mismatches.push(`Price: ≤₹${price}`);
        }
      }
    }

    // Service matching (medium importance)
    if (serviceMatch) {
      const matchedServices = provider.details.services.filter(service => {
        return service.name.toLowerCase().includes(serviceMatch[0]) || 
               service.description.toLowerCase().includes(serviceMatch[0]);
      });
      
      if (matchedServices.length > 0) {
        score += 20;
        matches.push(`Service: ${serviceMatch[0]}`);
      } else {
        mismatches.push(`Service: ${serviceMatch[0]}`);
      }
    }

    // Quality matching (low importance)
    if (qualityMatch) {
      const quality = qualityMatch[0].toLowerCase();
      const isBudget = ['cheap', 'affordable', 'budget', 'low cost', 'low price', 'economical'].includes(quality);
      const isPremium = ['expensive', 'premium', 'luxury', 'high end', 'high quality'].includes(quality);
      
      if (isBudget) {
        const hasBudgetServices = provider.details.services.some(service => 
          service.variants.some(variant => 
            variant.name.toLowerCase().includes('basic') || 
            variant.name.toLowerCase().includes('standard')
          )
        );
        if (hasBudgetServices) {
          score += 10;
          matches.push(`Budget-friendly`);
        }
      }
      
      if (isPremium) {
        const hasPremiumServices = provider.details.services.some(service => 
          service.variants.some(variant => 
            variant.name.toLowerCase().includes('premium') || 
            variant.name.toLowerCase().includes('complex') ||
            variant.name.toLowerCase().includes('high end')
          )
        );
        if (hasPremiumServices) {
          score += 10;
          matches.push(`Premium services`);
        }
      }
    }

    // Rating matching (medium importance)
    if (ratingMatch) {
      if (ratingMatch[1] && !isNaN(ratingMatch[1])) {
        const minRating = parseFloat(ratingMatch[1]);
        const providerRating = parseFloat(provider.details.rating);
        if (providerRating >= minRating) {
          score += 15;
          matches.push(`Rating ≥ ${minRating}`);
        }
      } else if (/high|good|excellent/i.test(ratingMatch[0])) {
        const providerRating = parseFloat(provider.details.rating);
        if (providerRating >= 4.5) {
          score += 15;
          matches.push(`Highly rated`);
        }
      }
    }

    // Availability matching (low importance)
    if (availabilityMatch) {
      if (provider.details.timings.toLowerCase().includes('24/7') || 
          provider.details.timings.toLowerCase().includes('emergency') ||
          provider.details.facilities.some(f => f.name.toLowerCase().includes('emergency'))) {
        score += 10;
        matches.push(`Available ${availabilityMatch[0]}`);
      }
    }

    // Provider type matching (low importance)
    if (providerTypeMatch) {
      const type = providerTypeMatch[0].toLowerCase();
      if (provider.details.tags.some(tag => tag.toLowerCase().includes(type)) ||
          provider.details.description.toLowerCase().includes(type)) {
        score += 5;
        matches.push(`${type} provider`);
      }
    }

    // General text search (fallback)
    if (!categoryMatch && !cityMatch && !priceMatch && !serviceMatch && 
        !qualityMatch && !ratingMatch && !availabilityMatch && !providerTypeMatch) {
      const searchFields = [
        provider.category,
        provider.metropolis,
        provider.details.name,
        provider.details.description,
        ...provider.details.tags,
        ...provider.details.services.map(s => s.name),
        ...provider.details.services.map(s => s.description),
        ...provider.details.facilities.map(f => f.name),
        ...provider.details.facilities.map(f => f.description)
      ].join(' ').toLowerCase();
      
      if (searchFields.includes(normalizedQuery)) {
        score += 50; // High score for direct match
        matches.push(`General match`);
      } else {
        // Try partial matching
        const queryWords = normalizedQuery.split(/\s+/);
        const matchedWords = queryWords.filter(word => 
          word.length > 3 && searchFields.includes(word)
        );
        if (matchedWords.length > 0) {
          score += matchedWords.length * 5;
          matches.push(`Partial match: ${matchedWords.join(', ')}`);
        } else {
          return null; // No match at all
        }
      }
    }

    // Handle negation queries
    if (hasNegation) {
      const negatedTerms = normalizedQuery.match(/\b(no|not|without|except)\s+(\w+)/i);
      if (negatedTerms) {
        const negatedTerm = negatedTerms[2];
        const searchFields = [
          provider.category,
          provider.metropolis,
          provider.details.name,
          ...provider.details.tags
        ].join(' ').toLowerCase();
        
        if (searchFields.includes(negatedTerm)) {
          return null; // Exclude providers matching negated term
        }
      }
    }

    return {
      provider,
      score,
      matches,
      mismatches
    };
  })
  .filter(Boolean) // Remove null entries
  .sort((a, b) => b.score - a.score) // Sort by score descending
  .map(item => item.provider); // Extract just the providers

  // Enhanced empty state handling
  if (filteredProviders.length === 0) {
    const suggestions = generateSearchSuggestions(query, providers);
    gridElement.innerHTML = `
      <div class="no-results">
        <div class="no-results-icon">🔍</div>
        <h3>No exact matches found</h3>
        <p>We couldn't find providers matching "${query}"</p>
        ${suggestions.length > 0 ? `
          <div class="suggestions">
            <p>Try one of these instead:</p>
            <ul>
              ${suggestions.map(suggestion => `
                <li onclick="document.getElementById('providersExploreInput').value = '${suggestion}'; performSearch(serviceProvidersData, '${suggestion}', document.getElementById('providersGrid'), document.querySelectorAll('.filter-button'), renderProviderCards)">
                  ${suggestion}
                </li>
              `).join('')}
            </ul>
          </div>
        ` : ''}
      </div>
    `;
  } else {
    gridElement.innerHTML = renderFunction(filteredProviders);
  }

  // Update active filter button based on search
  updateActiveFilterButton(filterButtons, categoryMatch, cityMatch, serviceMatch);
}

function updateActiveFilterButton(filterButtons, categoryMatch, cityMatch, serviceMatch) {
  // Reset all buttons first
  filterButtons.forEach(btn => btn.classList.remove('active'));
  
  // Determine which filter to activate
  if (categoryMatch) {
    // Find matching filter button
    const matchingButton = Array.from(filterButtons).find(btn => 
      btn.dataset.category.toLowerCase() === categoryMatch[0].toLowerCase()
    );
    
    if (matchingButton) {
      matchingButton.classList.add('active');
    } else {
      // If no exact match, try to find a partial match
      const partialMatch = Array.from(filterButtons).find(btn => 
        btn.dataset.category.toLowerCase().includes(categoryMatch[0].toLowerCase()) ||
        categoryMatch[0].toLowerCase().includes(btn.dataset.category.toLowerCase())
      );
      
      if (partialMatch) {
        partialMatch.classList.add('active');
      } else {
        // Default to 'All' if no match found
        filterButtons[0].classList.add('active');
      }
    }
  } else if (serviceMatch) {
    // Try to infer category from service
    const serviceToCategory = {
      'pipe': 'Plumber',
      'drain': 'Plumber',
      'leak': 'Plumber',
      'wiring': 'Electrician',
      'switch': 'Electrician',
      'light': 'Electrician',
      'furniture': 'Carpenter',
      'wardrobe': 'Carpenter',
      'cooling': 'AC Repair',
      'heating': 'AC Repair',
      'clean': 'Home Cleaning',
      'pest': 'Pest Control',
      'rodent': 'Pest Control',
      'fridge': 'Appliance Repair',
      'washing': 'Appliance Repair'
    };
    
    const inferredCategory = serviceToCategory[serviceMatch[0].toLowerCase()];
    if (inferredCategory) {
      const matchingButton = Array.from(filterButtons).find(btn => 
        btn.dataset.category === inferredCategory
      );
      if (matchingButton) matchingButton.classList.add('active');
    } else {
      filterButtons[0].classList.add('active');
    }
  } else {
    // Default to 'All' if no specific category or service was mentioned
    filterButtons[0].classList.add('active');
  }
}

// Helper function to generate search suggestions
function generateSearchSuggestions(query, providers) {
  const suggestions = new Set();
  
  // 1. Suggest similar categories
  const categories = [...new Set(providers.map(p => p.category))];
  categories.forEach(cat => {
    if (cat.toLowerCase().includes(query.toLowerCase().substring(0, 3))) {
      suggestions.add(cat);
    }
  });

  // 2. Suggest popular services
  if (query.length > 3) {
    providers.forEach(provider => {
      provider.details.services.forEach(service => {
        if (service.name.toLowerCase().includes(query.toLowerCase())) {
          suggestions.add(service.name);
        }
      });
    });
  }

  // 3. Add price-based suggestions if no price was mentioned
  if (!/(rs|₹|inr|price|cost)/i.test(query)) {
    suggestions.add(`${query} under ₹1000`);
    suggestions.add(`${query} under ₹2000`);
    suggestions.add(`affordable ${query}`);
  }

  // 4. Add availability suggestions
  if (/emergency|urgent|now|today/i.test(query)) {
    suggestions.add(`24/7 ${query.replace(/emergency|urgent|now|today/gi, '').trim()}`);
  }

  // 5. Add quality suggestions
  if (/cheap|affordable|budget/i.test(query)) {
    suggestions.add(`affordable ${query.replace(/cheap|affordable|budget/gi, '').trim()}`);
  } else if (/premium|quality|best/i.test(query)) {
    suggestions.add(`premium ${query.replace(/premium|quality|best/gi, '').trim()}`);
  }

  return Array.from(suggestions).slice(0, 5); // Return top 5 suggestions
}

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
.providers-overlay {
  font-family: poppins;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.providers-overlay::-webkit-scrollbar {
  display: none;
}

.providers-overlay.active {
  opacity: 1;
  visibility: visible;
}

.providers-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.providers-header {
  padding: 12px 20px;
  background: rgba(26, 26, 31, 0.98);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.header-title-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}

.providers-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.header-subtitle {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.8em;
  margin-top: 2px;
  font-weight: 400;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.close-providers {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 1em;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-providers:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.close-providers:active {
  transform: scale(0.95);
}

.sort-button {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  font-size: 0.9em;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.sort-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
}

.sort-button:active {
  transform: translateY(0);
}

.button-text {
  font-weight: 500;
}

.sort-options-container {
  position: fixed;
  top: 72px;
  right: 20px;
  background: #2a2a35;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  z-index: 40;
  padding: 8px 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.sort-options-container.active {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.sort-option {
  padding: 12px 20px;
  color: white;
  font-size: 0.9em;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.sort-option:hover {
  background: rgba(255, 215, 0, 0.1);
  color: var(--primary-color);
}

.sort-option i {
  width: 20px;
  text-align: center;
}

.providers-filter-container {
  position: fixed;
  top: 64px;
  left: 0;
  right: 0;
  width: 100%;
  z-index: 10;
  background: #1a1a1f;
  padding: 12px 0;
  box-sizing: border-box;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
  border-bottom: 1px solid rgba(255, 215, 0, 0.1);
}

.providers-filter-container::-webkit-scrollbar {
  display: none;
}

.providers-filter-buttons {
  display: inline-flex;
  padding: 0 16px;
  gap: 8px;
  white-space: nowrap;
}

.filter-button {
  font-family: poppins;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: rgba(255, 255, 255, 0.9);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 20px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  margin: 0;
}

.filter-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: scale(1.03);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: #000;
  font-weight: 600;
  border-color: var(--primary-color);
  box-shadow: 0 4px 10px rgba(255, 215, 0, 0.3);
}

/* Add some space at the end for better scrolling */
.providers-filter-buttons::after {
  content: '';
  display: block;
  min-width: 16px;
  height: 1px;
}

@media (min-width: 1024px) {
   .providers-filter-container {
   top: 72px;
    }
}

.providers-grid {
  margin-top: 90px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  padding-bottom: 140px;
  width: 100%;
}

.provider-card {
  background: #ffffff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  position: relative;
  border: none;
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
  height: 100%;
}

.provider-card:hover {
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.provider-card:active {
  transform: translateY(-1px);
  transition: transform 0.1s ease;
}

.image-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 12px 12px 0 0;
  margin-bottom: 0;
  box-shadow: none;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease, transform 0.5s ease;
  transform: scale(1.03);
  will-change: opacity, transform;
}

.carousel-item.active {
  opacity: 1;
  transform: scale(1);
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.provider-card:hover .carousel-item.active img {
  transform: scale(1.05);
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  -webkit-tap-highlight-color: transparent;
}

.image-carousel:hover .carousel-control {
  opacity: 0.9;
}

.carousel-control:hover {
  background: rgba(0, 0, 0, 0.85);
  transform: translateY(-50%) scale(1.1);
}

.carousel-control:active {
  transform: translateY(-50%) scale(0.95);
}

.carousel-control.prev {
  left: 12px;
}

.carousel-control.next {
  right: 12px;
}

.carousel-pagination {
  position: absolute;
  bottom: 12px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
}

.provider-card-content {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.provider-card-header {
  display: flex;
  flex-direction: column;
  padding: 0;
  background: transparent;
  border-bottom: none;
}

.provider-name {
  font-family: poppins;
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin: 0 0 4px 0;
  line-height: 1.3;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.provider-category {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 14px;
  margin-bottom: 8px;
}

.provider-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
}

.provider-rating .star {
  color: #ffc107;
  font-weight: 600;
}

.provider-rating .count {
  color: #666;
  font-size: 14px;
}

.provider-info-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
  margin: 4px 0;
}

.info-item {
  flex: 1 1 100%;
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px;
  border-radius: 10px;
  background: rgba(255, 215, 0, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.1);
  transition: all 0.3s ease;
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

.info-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: var(--primary-color);
}

.info-icon i {
  font-size: 14px;
}

.info-content {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.info-label {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #ffc107;
  margin-bottom: 6px;
}

.info-value {
  font-size: 13px;
  font-weight: 500;
  color: #000;
  line-height: 1.4;
  display: block;
  margin-top: 0;
}

.info-value + .info-value {
  margin-top: 6px;
  position: relative;
  padding-top: 6px;
}

.info-value + .info-value::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 215, 0, 0.1);
}

.info-item:hover {
  background: rgba(255, 215, 0, 0.08);
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.provider-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
  margin: 12px 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.tag-container {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 10px 0;
}

.tag {
  background: #f5f5f5;
  color: #666;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
  transition: background-color 0.2s ease;
}

.tag:hover {
  background: #eeeeee;
}

.provider-card-actions {
  display: flex;
  border-top: 1px solid #eee;
  padding: 0;
  background: transparent;
  margin-top: auto;
}

.action-button {
  flex: 1;
  background: transparent;
  color: #333;
  font-weight: 500;
  padding: 14px 0;
  border: none;
  border-radius: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  position: relative;
  transition: background-color 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.action-button:hover {
  background: #f5f5f5;
}

.action-button:active {
  background: #eeeeee;
}

.action-button i {
  color: #333;
  font-size: 16px;
}

.action-button:first-child {
  border-right: 1px solid #eee;
}

@media (max-width: 991px) {
  .providers-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  }
}

@media (max-width: 768px) {
  .providers-grid {
    grid-template-columns: 2fr;
    padding: 0 12px;
    margin-top: 90px;
    gap: 16px;
  }
  
  .provider-card {
    margin-bottom: 0;
    border-radius: 12px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .image-carousel {
    height: 180px;
    border-radius: 12px 12px 0 0;
  }
  
  .provider-card-content {
    padding: 14px;
  }
  
  .provider-name {
    font-size: 16px;
  }
  
  .provider-description {
    font-size: 13px;
    margin: 10px 0;
  }
  
  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }
  
  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }
  
  .carousel-control {
    width: 32px;
    height: 32px;
    opacity: 0.7;
  }
  
  .image-carousel .carousel-control {
    opacity: 0.7;
  }
  
  .pagination-dot {
    width: 6px;
    height: 6px;
  }
  
  .pagination-dot.active {
    width: 16px;
  }
}

@media (max-width: 480px) {
  .provider-card {
    margin-bottom: 0px;
  }
  
  .image-carousel {
    height: 160px;
  }
  
  .provider-card-content {
    padding: 12px;
  }
  
  .provider-description {
    -webkit-line-clamp: 2;
    margin: 8px 0;
  }
    
  .tag-container {
    margin: 8px 0;
  }
  
  .tag {
    padding: 2px 8px;
    font-size: 10px;
  }
  
  .action-button {
    padding: 10px 0;
    font-size: 12px;
  }
  
  .action-button i {
    font-size: 14px;
  }
  
  .carousel-control {
    width: 28px;
    height: 28px;
  }
  
  .providers-filter-buttons {
    margin-top: 10px;
  }
  
  .providers-filter-buttons {
    padding-bottom: 2px;
  }
  
  .filter-button {
    padding: 9px 12px;
    font-size: 14px;
  }
}

@media (max-width: 768px) and (orientation: portrait) {
  .providers-grid {
    margin-top: 130px;
  }
  
  .provider-card {
    border-radius: 10px;
  }
}

.provider-card {
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .provider-card,
  .provider-card:hover,
  .carousel-item,
  .carousel-item.active,
  .carousel-item img,
  .carousel-control,
  .carousel-control:hover,
  .pagination-dot {
    transition: none;
    transform: none;
    animation: none;
  }
}

.provider-card:focus {
  outline: 2px solid var(--primary-color);
  outline-offset: 3px;
}

@media (prefers-color-scheme: dark) {
  .provider-card {
    background: #fff;
  }
  
  .provider-name {
    color: #000;
  }
  
  .provider-category,
  .provider-description,
  .provider-rating .count {
    color: #000;
  }
  
  .tag {
    background: #000;
    color: #ddd;
  }
  
  .tag:hover {
    background: #444;
  }
  
  .provider-card-actions {
    border-top: 1px solid #333;
  }
  
  .action-button {
    color: #000;
  }
  
  .action-button i {
    color: #000;
  }
  
  .action-button:first-child {
    border-right: 1px solid #333;
  }
  
  .action-button:hover {
    background: #fff;
  }
}

.chat-container {
  position: fixed;
  bottom: 0px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

.input-container {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#providersExploreInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 12px 50px 12px 20px;
  color: #fff;
  font-size: 0.95em;
  width: 100%;
  transition: border-color 0.3s ease;
}

#providersExploreInput:focus {
  border-color: rgba(255, 255, 255, 0.5);
}

.voice-search {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-search:hover {
  opacity: 0.8;
}

.voice-search.active {
  color: #ffd700;
  animation: pulse 1.5s infinite;
}

.voice-search i { 
  font-size: 1.5em; 
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 0px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #providersExploreInput {
    height: 70px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 1.7em;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #providersExploreInput {
    bottom: 0px;
    width: 60%;
    max-width: 1200px;
  }
  
  .providers-grid {
    padding-bottom: 180px;
  }
}

@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .providers-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .provider-card {
    width: 100%;
    margin: 0;
  }

  .providers-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .providers-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #providersExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}

.full-screen-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.95);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.full-screen-image.active {
  opacity: 1;
  visibility: visible;
}

.full-screen-image img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 10px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  transform: scale(0.95);
  opacity: 0;
  transition: all 0.4s ease;
}

.full-screen-image.active img {
  transform: scale(1);
  opacity: 1;
}

.close-full-screen {
  position: absolute;
  top: 24px; 
  right: 24px;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close-full-screen:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}

/* No Results State */
.no-results {
  text-align: center;
  padding: 40px 20px;
  max-width: 600px;
  margin: 0 auto;
}

.no-results-icon {
  font-size: 3rem;
  color: #fff;
  margin-bottom: 20px;
}

.no-results h3 {
  font-size: 1.5rem;
  color: #fff;
  margin-bottom: 8px;
}

.no-results p {
  color: #fff;
  margin-bottom: 24px;
}

/* Suggestions */
.suggestions {
  background: #f9f9f9;
  border-radius: 12px;
  padding: 20px;
  margin-top: 20px;
}

.suggestions p {
  font-weight: 500;
  margin-bottom: 12px;
  color: #333;
}

.suggestions ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.suggestions li {
  padding: 12px 16px;
  margin-bottom: 8px;
  background: #000;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.2s ease;
  border: 1px solid #eee;
}

.suggestions li:hover {
  background: #000;
}

/* ===== Tablet (768px - 1024px) ===== */
@media (min-width: 768px) and (max-width: 1024px) {
  .providers-grid {
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 16px;
    padding-bottom: 120px;
  }

  .provider-card {
    border-radius: 14px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }

  .image-carousel {
    height: 180px;
  }

  .provider-card-content {
    padding: 14px;
  }

  .provider-name {
    font-size: 16px;
    margin-bottom: 4px;
  }

  .provider-category {
    font-size: 13px;
    margin-bottom: 8px;
  }

  .info-item {
    padding: 10px;
    gap: 8px;
  }

  .info-icon {
    width: 28px;
    height: 28px;
    border-radius: 6px;
  }

  .info-icon i {
    font-size: 13px;
  }

  .info-label {
    font-size: 10px;
    margin-bottom: 4px;
  }

  .info-value {
    font-size: 12px;
  }

  .provider-description {
    font-size: 13px;
    margin: 10px 0;
    -webkit-line-clamp: 2;
  }

  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }

  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }

  .providers-filter-container {
  top: 72px;
  }
}


/* ===== Laptop & Desktop (1025px+) ===== */
@media (min-width: 1025px) {
  .providers-grid {
    grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
    gap: 18px;
    padding-bottom: 140px;
  }

  .provider-card {
    border-radius: 16px;
    box-shadow: 0 5px 12px rgba(0, 0, 0, 0.12);
  }

  .image-carousel {
    height: 200px;
  }

  .provider-card-content {
    padding: 16px;
  }

  .provider-name {
    font-size: 17px;
    margin-bottom: 4px;
  }

  .provider-category {
    font-size: 14px;
    margin-bottom: 8px;
  }

  .info-item {
    padding: 12px;
    gap: 10px;
  }

  .info-icon {
    width: 30px;
    height: 30px;
    border-radius: 8px;
  }

  .info-icon i {
    font-size: 14px;
  }

  .info-label {
    font-size: 11px;
    margin-bottom: 6px;
  }

  .info-value {
    font-size: 13px;
  }

  .provider-description {
    font-size: 14px;
    margin: 12px 0;
    -webkit-line-clamp: 2;
  }

  .tag {
    padding: 4px 10px;
    font-size: 12px;
  }

  .action-button {
    padding: 14px 0;
    font-size: 14px;
  }
}
  `;
  document.head.appendChild(style);
}

function renderProviderCards(filteredData) {
  return filteredData.map(provider => {
    const tagsHTML = provider.details.tags && provider.details.tags.length > 0 
      ? `<div class="tag-container">
          ${provider.details.tags.map(tag => `<span class="tag">${tag}</span>`).join('')}
         </div>`
      : '';
    
    const actionButtonsHTML = `
      <a href="${provider.details.mapLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
        <i class="fas fa-map-marker-alt"></i> Map
      </a>
      <button class="action-button" onclick="openProviderPage('${provider.details.name}'); event.stopPropagation()">
        <i class="fas fa-info-circle"></i> Details
      </button>
      <button class="action-button" onclick="showChatSupport('${provider.details.name}'); event.stopPropagation()">
        <i class="fas fa-comments"></i> Chat
      </button>
    `;
    
    return `
      <div class="provider-card" onclick="openProviderPage('${provider.details.name}')">
        <div class="image-carousel">
          ${provider.details.cardImages.map((image, index) => `
            <div class="carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${provider.details.name}" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="carousel-pagination">
            ${provider.details.cardImages.map((_, index) => `
              <div class="pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="carousel-control prev" onclick="changeCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="carousel-control next" onclick="changeCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        
        <div class="provider-card-content">
          <div class="provider-card-header">
            <div style="display: flex; justify-content: space-between; align-items: flex-start;">
              <h3 class="provider-name">${provider.details.name}</h3>
              <div class="provider-rating">
                <span class="star">${provider.details.rating || '4.0'} ★</span>
              </div>
            </div>
            
            <div class="provider-category">
              <span>${provider.category} • ${provider.metropolis}</span>
            </div>
            
            <div class="provider-info-grid">
              <div class="info-item">
                <div class="info-icon">
                  <i class="fas fa-map-marker-alt"></i>
                </div>
                <div class="info-content">
                  <span class="info-label">Location & Hours</span>
                  <span class="info-value">${provider.details.address}</span>
                  <span class="info-value" style="margin-top: 4px;">${provider.details.timings}</span>
                </div>
              </div>
            </div>
            
            ${tagsHTML}
          </div>
        </div>

        <div class="provider-card-actions">
          ${actionButtonsHTML}
        </div>
      </div>
    `;
  }).join('');
}

function openProviderPage(providerName) {
  const provider = serviceProvidersData.find(prov => prov.details.name === providerName);
  if (!provider) return;

  // Set the current provider in booking state
  bookingState.provider = provider;

  const providerPage = document.createElement('div');
  providerPage.className = 'provider-page';
  providerPage.innerHTML = `
    <div class="provider-header">
      <button class="back-button" onclick="closeProviderPage()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>${provider.details.name}</h2>
      <div class="header-actions">
        <button class="cart-button" onclick="showBookingCart()">
          <i class="fas fa-shopping-cart"></i>
          ${bookingState.items.length > 0 ? `<span class="cart-count">${bookingState.items.length}</span>` : ''}
        </button>
        <button class="share-button" onclick="shareProvider('${provider.details.name}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="provider-content">
      <!-- Hero Section with Provider Image Carousel -->
      <div class="provider-hero">
        <div class="provider-image-carousel">
          ${provider.details.providerImages.map((image, index) => `
            <div class="provider-carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${provider.details.name}" class="provider-image" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="provider-carousel-pagination">
            ${provider.details.providerImages.map((_, index) => `
              <div class="provider-pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentProviderSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="provider-carousel-control prev" onclick="changeProviderCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="provider-carousel-control next" onclick="changeProviderCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        <div class="provider-basic-info">
          <div class="provider-star">
            <i class="fas fa-star"></i> ${provider.details.rating} (${provider.details.reviews})
          </div>
          <div class="provider-type">
            ${provider.category}
          </div>
          <div class="provider-timings">
            <i class="fas fa-clock"></i> ${provider.details.timings}
          </div>
          <div class="visiting-fees">
            <i class="fas fa-money-bill-wave"></i>${provider.details.entryFee}
          </div>
        </div>
      </div>
      
      <!-- Provider Description -->
      <div class="provider-description-section">
        <h3>About</h3>
        <p>${provider.details.description}</p>
        <div class="established-since">
          <i class="fas fa-calendar-alt"></i> Established: ${provider.details.established}
        </div>
      </div>
      
      <!-- Services Section -->
    <div class="provider-services-section">
      <h3>Services</h3>
      <div class="services-container">
        ${renderProviderServices(provider.details.services)}
      </div>
      </div>
      
      <!-- Facilities Section -->
      <div class="provider-facilities-section">
        <h3>Why Choose Us</h3>
        <div class="facilities-grid">
          ${provider.details.facilities?.map(facility => `
            <div class="facility-item">
              <div class="facility-icon">
                <i class="fas fa-${getFacilityIcon(facility.name)}"></i>
              </div>
              <div class="facility-info">
                <h4>${facility.name}</h4>
                <p>${facility.description}</p>
              </div>
            </div>
          `).join('') || '<p>No facilities information available</p>'}
        </div>
      </div>
      
      <!-- Promotions Section -->
      <div class="provider-promos-section">
        <h3>Special Offers</h3>
        <div class="promos-grid">
          ${provider.details.promos?.map(promo => `
            <div class="promo-item">
              <div class="promo-info">
                <h4>${promo.name}</h4>
                <p>${promo.description}</p>
              </div>
              <div class="promo-code">
                <span>${promo.code}</span>
                <button onclick="copyToClipboard('${promo.code}')">Copy</button>
              </div>
            </div>
          `).join('') || '<p>No current promotions</p>'}
        </div>
      </div>
      
      <!-- Contact Section -->
      <div class="provider-contact-section">
        <h3>Contact</h3>
        <div class="contact-methods">
          ${provider.details.support?.phone ? `
            <a href="tel:${provider.details.support.phone}" class="contact-item">
              <i class="fas fa-phone"></i> ${provider.details.support.phone}
            </a>
          ` : ''}
          ${provider.details.support?.whatsapp ? `
            <a href="https://wa.me/${provider.details.support.whatsapp}" target="_blank" class="contact-item">
              <i class="fab fa-whatsapp"></i> WhatsApp
            </a>
          ` : ''}
          ${provider.details.support?.email ? `
            <a href="mailto:${provider.details.support.email}" class="contact-item">
              <i class="fas fa-envelope"></i> ${provider.details.support.email}
            </a>
          ` : ''}
          ${provider.details.website ? `
            <a href="${provider.details.website}" target="_blank" class="contact-item">
              <i class="fas fa-globe"></i> Visit Website
            </a>
          ` : ''}
        </div>
        
        <div class="social-links">
          ${provider.details.social?.facebook ? `
            <a href="${provider.details.social.facebook}" target="_blank" class="social-link">
              <i class="fab fa-facebook-f"></i>
            </a>
          ` : ''}
          ${provider.details.social?.twitter ? `
            <a href="${provider.details.social.twitter}" target="_blank" class="social-link">
              <i class="fab fa-twitter"></i>
            </a>
          ` : ''}
          ${provider.details.social?.instagram ? `
            <a href="${provider.details.social.instagram}" target="_blank" class="social-link">
              <i class="fab fa-instagram"></i>
            </a>
          ` : ''}
        </div>
      </div>
      
      <!-- Provider Action Buttons -->
      <div class="provider-actions">
        <a href="${provider.details.mapLink}" target="_blank" class="primary-action">
          <i class="fas fa-directions"></i> Get Directions
        </a>
        <button class="secondary-action" onclick="showChatSupport('${provider.details.name}')">
          <i class="fas fa-comment-alt"></i> Contact
        </button>
      </div>
    </div>
    
    <!-- Booking Cart Overlay -->
    <div class="booking-cart-overlay" id="bookingCartOverlay">
      <div class="booking-cart-container">
        <div class="booking-cart-header">
          <h3>Your Booking Cart</h3>
          <button class="close-booking-cart" onclick="hideBookingCart()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        
        <div class="booking-cart-items" id="bookingCartItems">
          ${renderBookingCartItems()}
        </div>
        
        <div class="booking-cart-summary">
          <div class="booking-cart-total">
            <span>Total:</span>
            <span id="bookingCartTotalAmount">₹${bookingState.total.toFixed(2)}</span>
          </div>
          <div class="booking-cart-actions">
            <button class="clear-booking-cart" onclick="clearBookingCart()">
              <i class="fas fa-trash"></i> Clear Cart
            </button>
            <button class="checkout-booking-btn" onclick="showBookingForm()">
              <i class="fas fa-calendar-check"></i> Book Now
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Booking Form Overlay -->
    <div class="booking-form-overlay" id="bookingFormOverlay">
      <div class="booking-form-container">
        <button class="close-booking-form" onclick="closeBookingForm()">
          <i class="fas fa-times"></i>
        </button>
        <h3>Book Services</h3>
        <form id="bookingForm">
          <div class="form-group">
            <label for="customerName">Full Name</label>
            <input type="text" id="customerName" required>
          </div>
          <div class="form-group">
            <label for="customerPhone">Phone Number</label>
            <input type="tel" id="customerPhone" required>
          </div>
          <div class="form-group">
            <label for="customerAddress">Service Address</label>
            <textarea id="customerAddress" rows="3" required></textarea>
          </div>
          <div class="form-group">
            <label for="serviceTime">Preferred Service Time</label>
            <input type="datetime-local" id="serviceTime" required>
          </div>
          <div class="form-group">
            <label for="specialInstructions">Special Instructions</label>
            <textarea id="specialInstructions" rows="2"></textarea>
          </div>
          
          <div class="booking-summary">
            <h4>Service Summary</h4>
            <div id="bookingSummaryItems">
              ${renderBookingSummaryItems()}
            </div>
            <div class="booking-total">
              <span>Total:</span>
              <span id="bookingTotal">₹${bookingState.total.toFixed(2)}</span>
            </div>
          </div>
          
          <div class="coupon-section">
            <div class="form-group">
              <label for="bookingCouponCode">Coupon Code</label>
              <div class="coupon-input">
                <input type="text" id="bookingCouponCode" placeholder="Enter coupon code">
                <button type="button" class="apply-coupon" onclick="applyBookingCoupon()">Apply</button>
              </div>
              <div id="bookingCouponMessage" class="coupon-message"></div>
            </div>
          </div>
          
          <button type="submit" class="submit-booking">
            <i class="fas fa-paper-plane"></i> Confirm Booking
          </button>
        </form>
      </div>
    </div>
    
    <!-- Booking Confirmation Modal -->
    <div class="booking-confirmation-modal" id="bookingConfirmationModal">
      <div class="confirmation-content">
        <div class="confirmation-icon">
          <i class="fas fa-check-circle"></i>
        </div>
        <h3>Booking Confirmed!</h3>
        <p id="bookingConfirmationMessage">Your service booking has been confirmed successfully.</p>
        <div class="confirmation-actions">
          <button class="view-receipt" onclick="viewBookingReceipt()">
            <i class="fas fa-receipt"></i> View Details
          </button>
          <button class="close-confirmation" onclick="closeBookingConfirmationModal()">
            <i class="fas fa-times"></i> Close
          </button>
        </div>
      </div>
    </div>
    
    <!-- Toast Notification -->
    <div class="toast"></div>
  `;

  document.body.appendChild(providerPage);
  addProviderPageStyles();
  initializeTabs();
  initializeProviderCarousel();
  initializeBookingForm();
  initializeSearch();
  scrollToTop();
}

function renderProviderServices(services) {
  if (!services || services.length === 0) {
    return '<p class="no-services">No services available</p>';
  }

  return services.map(service => `
    <div class="service-item" data-name="${service.name}" data-price="${service.price.replace('₹','').split('-')[0]}">
      <div class="service-image">
        <img src="${service.image}" alt="${service.name}">
      </div>
      <div class="service-details">
        <h4>${service.name}</h4>
        <p class="service-description">${service.description}</p>
        <div class="service-pricing">
          <span class="service-price">${service.price}</span>
        </div>
        ${renderServiceVariants(service.variants)}
        <div class="service-actions">
          <button class="add-to-booking-cart" onclick="addToBookingCart(event, '${service.name}', ${service.price.replace('₹','').split('-')[0]}, '${service.image}')">
            <i class="fas fa-cart-plus"></i> Add to Cart
          </button>
          <button class="book-now" onclick="event.stopPropagation(); showBookingForm(event, '${service.name}', ${service.price.replace('₹','').split('-')[0]}, '${service.image}')">
            <i class="fas fa-calendar-check"></i> Book Now
          </button>
        </div>
      </div>
    </div>
  `).join('');
}

function renderServiceVariants(variants) {
  if (!variants || variants.length === 0) return '';
  
  return `
    <div class="variants-container">
      <select class="variant-select" onchange="updateServicePrice(this)">
        ${variants.map(variant => `
          <option value="${variant.price.replace('₹','')}" data-name="${variant.name}">
            ${variant.name} - ${variant.price}
          </option>
        `).join('')}
      </select>
    </div>
  `;
}

// Update the price update function to use the correct naming
function updateServicePrice(selectElement) {
  const price = selectElement.value;
  const itemElement = selectElement.closest('[data-price]');
  if (itemElement) {
    itemElement.dataset.price = price;
    const priceDisplay = itemElement.querySelector('.service-price');
    if (priceDisplay) {
      priceDisplay.textContent = `₹${price}`;
    }
  }
}

// Booking Cart Management
function renderBookingCartItems() {
  if (bookingState.items.length === 0) {
    return `
      <div class="empty-booking-cart">
        <i class="fas fa-shopping-cart"></i>
        <p>Your booking cart is empty</p>
        <button onclick="hideBookingCart()">Continue Browsing</button>
      </div>
    `;
  }

  return bookingState.items.map((item, index) => `
    <div class="booking-cart-item" data-index="${index}">
      <div class="booking-cart-item-image">
        <img src="${item.image}" alt="${item.name}">
      </div>
      <div class="booking-cart-item-details">
        <h4>${item.name}</h4>
        ${item.variant ? `<p class="variant">${item.variant}</p>` : ''}
        <div class="booking-cart-item-price">₹${item.price.toFixed(2)}</div>
        <div class="booking-cart-item-quantity">
          <button class="quantity-btn minus" onclick="updateBookingQuantity(${index}, -1)">−</button>
          <span class="quantity">${item.quantity}</span>
          <button class="quantity-btn plus" onclick="updateBookingQuantity(${index}, 1)">+</button>
        </div>
      </div>
      <button class="remove-item" onclick="removeFromBookingCart(${index})">
        <i class="fas fa-times"></i>
      </button>
    </div>
  `).join('');
}

function renderBookingSummaryItems() {
  return bookingState.items.map(item => `
    <div class="booking-item">
      <span>${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}</span>
      <span>₹${(item.price * item.quantity).toFixed(2)}</span>
    </div>
  `).join('');
}

function addToBookingCart(event, serviceName, basePrice, image) {
  event.stopPropagation();
  
  const itemElement = event.target.closest('[data-name]');
  const variantSelect = itemElement?.querySelector('.variant-select');
  
  let variantName = null;
  let price = parseFloat(basePrice);
  
  // Handle variants if they exist
  if (variantSelect) {
    const selectedOption = variantSelect.options[variantSelect.selectedIndex];
    variantName = selectedOption.dataset.name;
    price = parseFloat(selectedOption.value);
  }
  
  // Check if item already exists in cart
  const existingItemIndex = bookingState.items.findIndex(item => 
    item.name === serviceName && item.variant === variantName
  );
  
  if (existingItemIndex >= 0) {
    // Update quantity if item exists
    bookingState.items[existingItemIndex].quantity += 1;
  } else {
    // Add new item to cart
    bookingState.items.push({
      name: serviceName,
      variant: variantName,
      price: price,
      quantity: 1,
      image: image
    });
  }
  
  // Update booking total
  updateBookingTotal();
  
  // Update booking cart UI
  updateBookingCartUI();
  
  // Show success message
  showToast(`${serviceName} ${variantName ? `(${variantName})` : ''} added to booking cart`);
}

function removeFromBookingCart(index) {
  bookingState.items.splice(index, 1);
  updateBookingTotal();
  updateBookingCartUI();
  
  if (bookingState.items.length === 0) {
    hideBookingCart();
  }
}

function updateBookingQuantity(index, change) {
  const newQuantity = bookingState.items[index].quantity + change;
  
  if (newQuantity < 1) {
    removeFromBookingCart(index);
    return;
  }
  
  bookingState.items[index].quantity = newQuantity;
  updateBookingTotal();
  updateBookingCartUI();
}

function clearBookingCart() {
  bookingState.items = [];
  bookingState.total = 0;
  bookingState.discountedTotal = 0;
  bookingState.appliedCoupon = null;
  updateBookingCartUI();
  hideBookingCart();
}

function updateBookingTotal() {
  bookingState.total = bookingState.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
}

function updateBookingCartUI() {
  const bookingCartOverlay = document.getElementById('bookingCartOverlay');
  if (!bookingCartOverlay) return;
  
  // Update booking cart items list
  const bookingCartItemsContainer = document.getElementById('bookingCartItems');
  if (bookingCartItemsContainer) {
    bookingCartItemsContainer.innerHTML = renderBookingCartItems();
  }
  
  // Update booking cart total
  const bookingCartTotalElement = document.getElementById('bookingCartTotalAmount');
  if (bookingCartTotalElement) {
    bookingCartTotalElement.textContent = `₹${bookingState.total.toFixed(2)}`;
  }
  
  // Update booking cart count in header
  const bookingCartButton = document.querySelector('.cart-button');
  if (bookingCartButton) {
    const bookingCartCount = bookingCartButton.querySelector('.cart-count');
    
    if (bookingState.items.length > 0) {
      if (!bookingCartCount) {
        const countElement = document.createElement('span');
        countElement.className = 'cart-count';
        countElement.textContent = bookingState.items.length;
        bookingCartButton.appendChild(countElement);
      } else {
        bookingCartCount.textContent = bookingState.items.length;
      }
    } else if (bookingCartCount) {
      bookingCartCount.remove();
    }
  }
  
  // Update booking summary in booking form
  const bookingSummaryItems = document.getElementById('bookingSummaryItems');
  const bookingTotal = document.getElementById('bookingTotal');
  
  if (bookingSummaryItems && bookingTotal) {
    bookingSummaryItems.innerHTML = renderBookingSummaryItems();
    bookingTotal.textContent = `₹${bookingState.total.toFixed(2)}`;
  }
}

function initializeBookingForm() {
  const bookingForm = document.getElementById('bookingForm');
  if (!bookingForm) return;

  bookingForm.addEventListener('submit', function(e) {
    e.preventDefault();
    
    // Get form values
    const customerName = document.getElementById('customerName').value;
    const customerPhone = document.getElementById('customerPhone').value;
    const customerAddress = document.getElementById('customerAddress').value;
    const serviceTime = document.getElementById('serviceTime').value;
    const specialInstructions = document.getElementById('specialInstructions').value;
    
    // Format service time
    let formattedServiceTime = '';
    if (serviceTime) {
      const serviceDate = new Date(serviceTime);
      formattedServiceTime = serviceDate.toLocaleString('en-IN', {
        weekday: 'short',
        day: 'numeric',
        month: 'short',
        year: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      });
    }
    
    // Determine which items to include in the booking
    const bookingItems = bookingState.items.length > 0 ? 
      bookingState.items.map(item => ({
        name: item.name,
        variant: item.variant,
        price: item.price,
        quantity: item.quantity,
        total: (item.price * item.quantity).toFixed(2)
      })) : 
      (bookingState.directBookingItems || []).map(item => ({
        name: item.name,
        variant: null,
        price: item.price,
        quantity: item.quantity,
        total: (item.price * item.quantity).toFixed(2)
      }));
    
    // Use discounted total if coupon was applied, otherwise use regular total
    const bookingTotal = bookingState.appliedCoupon ? 
      bookingState.discountedTotal : 
      bookingItems.reduce((sum, item) => sum + parseFloat(item.total), 0);
    
    // Create booking details message
    let bookingDetails = `*NEW SERVICE BOOKING*\n\n`;
    bookingDetails += `*Service Provider:* ${bookingState.provider.details.name}\n`;
    bookingDetails += `*Booking Time:* ${new Date().toLocaleString('en-IN')}\n\n`;
    
    bookingDetails += `*Customer Details:*\n`;
    bookingDetails += `👤 Name: ${customerName}\n`;
    bookingDetails += `📞 Phone: ${customerPhone}\n`;
    bookingDetails += `🏠 Address: ${customerAddress}\n`;
    if (formattedServiceTime) {
      bookingDetails += `⏰ Service Time: ${formattedServiceTime}\n`;
    }
    if (specialInstructions) {
      bookingDetails += `📝 Special Instructions: ${specialInstructions}\n`;
    }
    bookingDetails += `\n`;
    
    bookingDetails += `*Services Booked:*\n`;
    bookingDetails += bookingItems.map(item => 
      `- ${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}: ₹${item.total}`
    ).join('\n');
    
    // Add coupon information if applied
    if (bookingState.appliedCoupon) {
      bookingDetails += `\n\n*Discount Applied:*\n`;
      bookingDetails += `Coupon Code: ${bookingState.appliedCoupon.code}\n`;
      bookingDetails += `Discount: ${(bookingState.appliedCoupon.discount * 100)}%\n`;
      bookingDetails += `Discount Amount: ₹${bookingState.appliedCoupon.discountAmount.toFixed(2)}\n`;
    }
    
    bookingDetails += `\n\n*Subtotal:* ₹${bookingItems.reduce((sum, item) => sum + parseFloat(item.total), 0).toFixed(2)}`;
    
    if (bookingState.appliedCoupon) {
      bookingDetails += `\n*Discount:* -₹${bookingState.appliedCoupon.discountAmount.toFixed(2)}`;
    }
    
    bookingDetails += `\n*Booking Total:* ₹${bookingTotal.toFixed(2)}`;
    
    // Send booking based on provider's preferred contact method
    if (bookingState.provider.details.support.primaryContact === "whatsapp" && bookingState.provider.details.support.whatsapp) {
      const whatsappUrl = `https://wa.me/${bookingState.provider.details.support.whatsapp}?text=${encodeURIComponent(bookingDetails)}`;
      window.open(whatsappUrl, '_blank');
    } else if (bookingState.provider.details.support.primaryContact === "email" && bookingState.provider.details.support.email) {
      const subject = `New Booking from ${customerName} - ${bookingState.provider.details.name}`;
      const body = bookingDetails.replace(/\*/g, '');
      const mailtoUrl = `mailto:${bookingState.provider.details.support.email}?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
      window.location.href = mailtoUrl;
    }
    
    // Show confirmation
    showBookingConfirmation(customerName, bookingTotal.toFixed(2));
    
    // Clear booking cart/direct booking and close form
    clearBookingCart();
    delete bookingState.directBookingItems;
    closeBookingForm();
  });
}

function applyBookingCoupon() {
  const couponCode = document.getElementById('bookingCouponCode').value;
  const couponMessage = document.getElementById('bookingCouponMessage');
  
  if (!couponCode) {
    couponMessage.textContent = 'Please enter a coupon code';
    couponMessage.className = 'coupon-message error';
    return;
  }
  
  // Simulate coupon validation
  const validCoupons = {
    'SAVE10': { discount: 0.1, message: '10% discount applied!' },
    'FREESERVICE': { discount: 0, message: 'Free service call applied!' },
    'WELCOME20': { discount: 0.2, message: '20% discount applied!' }
  };
  
  if (validCoupons[couponCode]) {
    const coupon = validCoupons[couponCode];
    const discountAmount = bookingState.total * coupon.discount;
    const newTotal = bookingState.total - discountAmount;
    
    // Update booking state
    bookingState.discountedTotal = newTotal;
    bookingState.appliedCoupon = {
      code: couponCode,
      discount: coupon.discount,
      discountAmount: discountAmount
    };
    
    // Update UI
    couponMessage.textContent = coupon.message;
    couponMessage.className = 'coupon-message success';
    
    document.getElementById('bookingTotal').textContent = `₹${newTotal.toFixed(2)}`;
  } else {
    couponMessage.textContent = 'Invalid coupon code';
    couponMessage.className = 'coupon-message error';
    bookingState.discountedTotal = bookingState.total;
    bookingState.appliedCoupon = null;
  }
}

// Booking Confirmation Functions
function showBookingConfirmation(customerName, bookingTotal) {
  const confirmationModal = document.getElementById('bookingConfirmationModal');
  const confirmationMessage = document.getElementById('bookingConfirmationMessage');
  
  if (confirmationModal && confirmationMessage) {
    confirmationMessage.textContent = `Thank you, ${customerName}! Your booking of ₹${bookingTotal} has been confirmed.`;
    confirmationModal.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function closeBookingConfirmationModal() {
  const confirmationModal = document.getElementById('bookingConfirmationModal');
  if (confirmationModal) {
    confirmationModal.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function viewBookingReceipt() {
  // In a real app, this would generate or show a detailed receipt
  alert('Booking details would be displayed here in a real implementation.');
  closeBookingConfirmationModal();
}

// Booking Visibility Functions
function showBookingCart() {
  const bookingCartOverlay = document.getElementById('bookingCartOverlay');
  if (bookingCartOverlay) {
    bookingCartOverlay.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function hideBookingCart() {
  const bookingCartOverlay = document.getElementById('bookingCartOverlay');
  if (bookingCartOverlay) {
    bookingCartOverlay.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function showBookingForm() {
  if (bookingState.items.length === 0 && !bookingState.directBookingItems) {
    showToast('Please add services to book');
    return;
  }
  
  hideBookingCart();
  
  const bookingFormOverlay = document.getElementById('bookingFormOverlay');
  if (bookingFormOverlay) {
    bookingFormOverlay.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function showBookingForm(event, serviceName, servicePrice, serviceImage) {
  if (event) event.stopPropagation();
  
  let items = [];
  
  if (serviceName && servicePrice) {
    // Handle direct booking from "Book Now" button
    const itemElement = event?.target.closest('[data-name]');
    const variantSelect = itemElement?.querySelector('.variant-select');
    
    if (variantSelect) {
      const selectedOption = variantSelect.options[variantSelect.selectedIndex];
      items.push({
        name: `${serviceName} (${selectedOption.dataset.name})`,
        price: parseFloat(selectedOption.value),
        quantity: 1,
        image: serviceImage
      });
    } else {
      items.push({
        name: serviceName,
        price: servicePrice,
        quantity: 1,
        image: serviceImage
      });
    }
    
    // Store the direct booking items temporarily
    bookingState.directBookingItems = items;
    // Update the total for direct booking
    bookingState.total = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  } else if (bookingState.items.length === 0) {
    showToast('Please add services to book');
    return;
  }
  
  // Create or show the booking form overlay
  let bookingFormOverlay = document.getElementById('bookingFormOverlay');
  
  if (!bookingFormOverlay) {
    // If overlay doesn't exist (shouldn't happen in normal flow), create it
    bookingFormOverlay = document.createElement('div');
    bookingFormOverlay.id = 'bookingFormOverlay';
    bookingFormOverlay.className = 'booking-form-overlay';
    bookingFormOverlay.innerHTML = `
      <div class="booking-form-container">
        <button class="close-booking-form" onclick="closeBookingForm()">
          <i class="fas fa-times"></i>
        </button>
        <h3>Book Services</h3>
        <form id="bookingForm">
          <!-- Form fields here -->
        </form>
      </div>
    `;
    document.querySelector('.provider-page').appendChild(bookingFormOverlay);
    initializeBookingForm();
  }
  
  // Update the booking summary in the form
  updateBookingSummary();
  
  bookingFormOverlay.classList.add('active');
  document.body.style.overflow = 'hidden';
}

function updateBookingSummary() {
  const bookingSummaryItems = document.getElementById('bookingSummaryItems');
  const bookingTotal = document.getElementById('bookingTotal');
  
  if (!bookingSummaryItems || !bookingTotal) return;
  
  // Determine which items to show (direct booking or cart items)
  const items = bookingState.items.length > 0 ? 
    bookingState.items : 
    (bookingState.directBookingItems || []);
  
  bookingSummaryItems.innerHTML = items.map(item => `
    <div class="booking-item">
      <span>${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}</span>
      <span>₹${(item.price * item.quantity).toFixed(2)}</span>
    </div>
  `).join('');
  
  const total = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  bookingTotal.textContent = `₹${total.toFixed(2)}`;
}

function closeBookingForm() {
  const bookingFormOverlay = document.getElementById('bookingFormOverlay');
  if (bookingFormOverlay) {
    bookingFormOverlay.classList.remove('active');
    document.body.style.overflow = '';
    
    // Reset coupon-related fields and state
    const couponCodeInput = document.getElementById('bookingCouponCode');
    const couponMessage = document.getElementById('bookingCouponMessage');
    
    if (couponCodeInput) couponCodeInput.value = '';
    if (couponMessage) {
      couponMessage.textContent = '';
      couponMessage.className = 'coupon-message';
    }
    
    // Reset the discounted total (but keep the regular total)
    bookingState.discountedTotal = bookingState.total;
    bookingState.appliedCoupon = null;
    
    // Update the displayed total to remove any discount
    const bookingTotalElement = document.getElementById('bookingTotal');
    if (bookingTotalElement) {
      bookingTotalElement.textContent = `₹${bookingState.total.toFixed(2)}`;
    }
  }
}


function closeProviderPage() {
  const providerPage = document.querySelector('.provider-page');
  if (providerPage) {
    providerPage.remove();
  }
}

function initializeProviderCarousel() {
  const carousel = document.querySelector('.provider-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.provider-carousel-item');
  const dots = carousel.querySelectorAll('.provider-pagination-dot');
  let currentIndex = 0;

  function showSlide(index) {
    items.forEach((item, i) => {
      item.classList.remove('active');
      dots[i].classList.remove('active');
      if (i === index) {
        item.classList.add('active');
        dots[i].classList.add('active');
      }
    });
    currentIndex = index;
  }

  function nextSlide() {
    const newIndex = (currentIndex + 1) % items.length;
    showSlide(newIndex);
  }

  function prevSlide() {
    const newIndex = (currentIndex - 1 + items.length) % items.length;
    showSlide(newIndex);
  }

  // Auto-rotate every 5 seconds
  let interval = setInterval(nextSlide, 5000);

  // Pause on hover
  carousel.addEventListener('mouseenter', () => {
    clearInterval(interval);
  });

  carousel.addEventListener('mouseleave', () => {
    interval = setInterval(nextSlide, 5000);
  });
}

function changeProviderCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.provider-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.provider-carousel-item');
  const dots = carousel.querySelectorAll('.provider-pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentProviderSlide(index) {
  const carousel = event.target.closest('.provider-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.provider-carousel-item');
  const dots = carousel.querySelectorAll('.provider-pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

// Add styles for the provider page
function addProviderPageStyles() {
  const style = document.createElement('style');
  style.textContent = `
.provider-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #1a1a1f;
  z-index: 2000;
  overflow-y: auto;
  color: #fff;
  font-family: 'Poppins', sans-serif;
}

.provider-header {
  position: sticky;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  z-index: 10;
  backdrop-filter: blur(12px);
}

.back-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.5em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.back-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.provider-header h2 {
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  text-align: center;
  flex: 1;
  padding: 0 16px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.header-actions {
  display: flex;
  gap: 8px;
}

.cart-button {
  position: relative;
  background: none;
  border: none;
  color: #ffd700;
  font-size: 1.2em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.cart-button:hover {
  background: rgba(255, 215, 0, 0.1);
}

.cart-count {
  position: absolute;
  top: -5px;
  right: -5px;
  background: #ff0000;
  color: white;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  font-size: 0.7em;
  display: flex;
  align-items: center;
  justify-content: center;
}

.share-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.2em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.share-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.provider-content {
  padding: 0 0 100px;
  width: 100%;
  margin: 0 auto;
}

.provider-hero {
  position: relative;
  padding: 20px;
}

.provider-image-carousel {
  position: relative;
  width: 100%;
  height: 280px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  margin-bottom: 16px;
}

.provider-carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.provider-carousel-item.active {
  opacity: 1;
}

.provider-carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.provider-carousel-item.active img:hover {
  transform: scale(1.02);
}

.provider-carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.7;
  transition: all 0.3s ease;
  z-index: 5;
}

.provider-carousel-control:hover {
  background: rgba(0, 0, 0, 0.9);
  opacity: 1;
  transform: translateY(-50%) scale(1.1);
}

.provider-carousel-control.prev {
  left: 16px;
}

.provider-carousel-control.next {
  right: 16px;
}

.provider-carousel-pagination {
  position: absolute;
  bottom: 16px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.provider-pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
}

.provider-pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
}

.provider-basic-info {
  display: flex;
  gap: 12px;
  margin-top: 16px;
  flex-wrap: wrap;
}

.provider-star,
.provider-type,
.visiting-fees,
.provider-timings {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 12px;
  border-radius: 20px;
  font-size: 0.9em;
  font-weight: 500;
}

.provider-star {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
}

.provider-type {
  background: rgba(0, 191, 255, 0.1);
  color: #00bfff;
}

.visiting-fees {
  background: rgba(0, 191, 255, 0.1);
  color: #00bfff;
}

.provider-timings {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

.established-since {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-top: 12px;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.provider-description-section {
  padding: 0 20px 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-description-section h3 {
  margin: 0 0 12px;
  font-size: 1.2em;
  font-weight: 600;
}

.provider-description-section p {
  margin: 0;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.8);
}

.provider-services-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-services-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.services-container {
  display: grid;
  gap: 20px;
}

.service-item {
  display: flex;
  background: #fff;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
}

.service-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.15);
}

.service-image {
  width: 150px;
  height: 150px;
  flex-shrink: 0;
}

.service-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.service-details {
  flex: 1;
  padding: 16px;
}

.service-details h4 {
  margin: 0 0 8px;
  color: #000;
  font-size: 1.1em;
}

.service-description {
  color: #000;
  font-size: 0.9em;
  margin: 0 0 12px;
}

.service-pricing {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 8px;
}

.service-price {
  font-weight: bold;
  color: #2e7d32;
  font-size: 1.1em;
}

.variants-container {
  margin-bottom: 12px;
}

.variant-select {
  width: 100%;
  padding: 8px;
  border-radius: 8px;
  border: 1px solid #ddd;
  background: #f9f9f9;
}

.service-actions {
  display: flex;
  gap: 10px;
}

.add-to-booking-cart,
.book-now {
  flex: 1;
  padding: 8px 12px;
  border: none;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.add-to-booking-cart {
  background: #f5f5f5;
  color: #333;
}

.add-to-booking-cart:hover {
  background: #e0e0e0;
}

.book-now {
  background: #ffd700;
  color: #000;
}

.book-now:hover {
  background: #ffc400;
}

.provider-facilities-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-facilities-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.facilities-grid {
  display: grid;
  gap: 16px;
}

.facility-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.facility-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.facility-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: #ffd700;
  font-size: 1.2em;
}

.facility-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.facility-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.provider-promos-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-promos-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.promos-grid {
  display: grid;
  gap: 16px;
}

.promo-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.promo-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.promo-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.promo-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.promo-code {
  display: flex;
  align-items: center;
  gap: 8px;
}

.promo-code span {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
  padding: 6px 12px;
  border-radius: 8px;
  font-weight: 600;
}

.promo-code button {
  background: rgba(255, 215, 0, 0.1);
  border: none;
  color: #ffd700;
  padding: 6px 12px;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.promo-code button:hover {
  background: rgba(255, 215, 0, 0.2);
}

.provider-contact-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-contact-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.contact-methods {
  display: grid;
  gap: 12px;
  margin-bottom: 20px;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 8px;
  color: #fff;
  text-decoration: none;
  transition: background 0.3s ease;
}

.contact-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.social-links {
  display: flex;
  gap: 16px;
  margin-top: 20px;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.05);
  color: #fff;
  font-size: 1.2em;
  transition: transform 0.3s ease, background 0.3s ease;
}

.social-link:hover {
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.1);
}

.provider-actions {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  z-index: 10;
  backdrop-filter: blur(12px);
}

.primary-action,
.secondary-action {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 14px;
  border-radius: 8px;
  font-weight: 500;
  font-size: 0.95em;
  cursor: pointer;
  transition: transform 0.3s ease, background 0.3s ease;
}

.primary-action {
  background: var(--primary-color, #ffd700);
  color: #000;
  border: none;
}

.primary-action:hover {
  background: #ffd700;
  transform: translateY(-2px);
}

.secondary-action {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  border: none;
}

.secondary-action:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

/* Booking Cart Overlay Styles */
.booking-cart-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 3000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.booking-cart-overlay.active {
  opacity: 1;
  visibility: visible;
}

.booking-cart-container {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 500px;
  max-height: 80vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.booking-cart-header {
  padding: 20px;
  border-bottom: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.booking-cart-header h3 {
  margin: 0;
  font-size: 1.3em;
  color: #333;
}

.close-booking-cart {
  background: none;
  border: none;
  font-size: 1.2em;
  color: #666;
  cursor: pointer;
  padding: 5px;
}

.booking-cart-items {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
}

.empty-booking-cart {
  text-align: center;
  padding: 30px 0;
}

.empty-booking-cart i {
  font-size: 3em;
  color: #ccc;
  margin-bottom: 15px;
}

.empty-booking-cart p {
  margin: 10px 0 20px;
  color: #666;
}

.empty-booking-cart button {
  background: var(--primary-color);
  color: #000;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
}

.booking-cart-item {
  display: flex;
  padding: 15px 0;
  border-bottom: 1px solid #f5f5f5;
  position: relative;
}

.booking-cart-item-image {
  width: 80px;
  height: 80px;
  flex-shrink: 0;
  margin-right: 15px;
}

.booking-cart-item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
}

.booking-cart-item-details {
  flex: 1;
}

.booking-cart-item-details h4 {
  margin: 0 0 5px;
  font-size: 1em;
  color: #333;
}

.booking-cart-item-details .variant {
  margin: 0 0 8px;
  font-size: 0.85em;
  color: #666;
}

.booking-cart-item-price {
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.booking-cart-item-quantity {
  color: #000;
  display: flex;
  align-items: center;
  gap: 10px;
}

.quantity-btn {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  border: 1px solid #ddd;
  background: #fff;
  font-size: 1em;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.quantity-btn:hover {
  background: #f5f5f5;
}

.quantity {
  min-width: 20px;
  text-align: center;
}

.remove-item {
  position: absolute;
  top: 15px;
  right: 0;
  background: none;
  border: none;
  color: #999;
  cursor: pointer;
  font-size: 1em;
}

.remove-item:hover {
  color: #ff5252;
}

.booking-cart-summary {
  padding: 20px;
  border-top: 1px solid #eee;
}

.booking-cart-total {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  font-size: 1.1em;
  font-weight: bold;
}

.booking-cart-actions {
  display: flex;
  gap: 10px;
}

.clear-booking-cart, .checkout-booking-btn {
  flex: 1;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.clear-booking-cart {
  background: #f5f5f5;
  color: #333;
  border: none;
}

.clear-booking-cart:hover {
  background: #e0e0e0;
}

.checkout-booking-btn {
  background: var(--primary-color);
  color: #000;
  border: none;
}

.checkout-booking-btn:hover {
  background: #ffc400;
}

/* Booking Form Styles */
.booking-form-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 3000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.booking-form-overlay.active {
  opacity: 1;
  visibility: visible;
}

.booking-form-container {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 500px;
  max-height: 90vh;
  overflow-y: auto;
  padding: 24px;
  position: relative;
}

.booking-form-container h3 {
  margin: 0 0 20px;
  text-align: center;
  color: #333;
}

.close-booking-form {
  background: none;
  border: none;
  font-size: 1.2em;
  color: #666;
  cursor: pointer;
  padding: 5px;
}

.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #333;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1em;
}

.form-group textarea {
  resize: vertical;
  min-height: 80px;
}

.coupon-input {
  display: flex;
  gap: 10px;
}

.coupon-input input {
  flex: 1;
}

.apply-coupon {
  background: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 0 15px;
  cursor: pointer;
}

.apply-coupon:hover {
  background: #e0e0e0;
}

.coupon-message {
  margin-top: 8px;
  font-size: 0.9em;
}

.coupon-message.success {
  color: #2e7d32;
}

.coupon-message.error {
  color: #c62828;
}

.booking-summary {
  margin: 20px 0;
  padding: 16px;
  background: #f9f9f9;
  border-radius: 8px;
}

.booking-summary h4 {
  margin: 0 0 12px;
  color: #333;
}

.booking-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  padding-bottom: 8px;
  border-bottom: 1px solid #eee;
  color: #333;
}

.booking-total {
  display: flex;
  justify-content: space-between;
  margin-top: 12px;
  font-weight: bold;
  font-size: 1.1em;
  color: #333;
}

.submit-booking {
  width: 100%;
  padding: 14px;
  background: var(--primary-color);
  color: #000;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  font-size: 1em;
  cursor: pointer;
  transition: background 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.submit-booking:hover {
  background: #ffc400;
}

/* Booking Confirmation Modal Styles */
.booking-confirmation-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 4000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.booking-confirmation-modal.active {
  opacity: 1;
  visibility: visible;
}

.confirmation-content {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 400px;
  padding: 30px;
  text-align: center;
}

.confirmation-icon {
  font-size: 4em;
  color: #4caf50;
  margin-bottom: 20px;
}

.confirmation-content h3 {
  margin: 0 0 15px;
  color: #333;
}

.confirmation-content p {
  margin: 0 0 25px;
  color: #666;
  line-height: 1.5;
}

.confirmation-actions {
  display: flex;
  gap: 10px;
}

.view-receipt, .close-confirmation {
  flex: 1;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.view-receipt {
  background: #4caf50;
  color: #fff;
  border: none;
}

.view-receipt:hover {
  background: #3d8b40;
}

.close-confirmation {
  background: #f5f5f5;
  color: #fff;
  border: none;
}

.close-confirmation:hover {
  background: #fff;
}

/* Toast notification */
.toast {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: #333;
  color: #fff;
  padding: 12px 24px;
  border-radius: 8px;
  z-index: 4000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.toast.show {
  opacity: 1;
  visibility: visible;
}

/* Responsive Styles */
@media (max-width: 768px) {
  .service-item {
    flex-direction: column;
  }
  
  .service-image {
    width: 100%;
    height: 180px;
  }
  
  .service-actions {
    flex-direction: column;
  }
  
  .add-to-booking-cart,
  .book-now {
    width: 100%;
  }
}

@media (max-width: 480px) {
  .booking-cart-container,
  .booking-form-container {
    width: 95%;
  }
  
  .booking-cart-item {
    flex-direction: column;
  }
  
  .booking-cart-item-image {
    width: 100%;
    height: 150px;
    margin-right: 0;
    margin-bottom: 10px;
  }
  
  .booking-cart-actions,
  .confirmation-actions {
    flex-direction: column;
  }
  
  .booking-form-container {
    padding: 16px;
  }
}

@media (min-width: 768px) and (max-width: 1024px) {
.provider-content {
    max-width: 900px;
    margin: 0 auto;
    padding: 0 0 120px;
  }
  
.provider-image-carousel {
    height: 350px;
    border-radius: 20px;
  }
    
  .social-links {
    gap: 18px;
  }
  
  .social-link {
    width: 44px;
    height: 44px;
    font-size: 1.3em;
  }
  
  .provider-actions {
    max-width: 700px;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 20px 20px 0 0; /* This curves only the top corners */
  }
  
  .primary-action,
  .secondary-action {
    padding: 16px;
    font-size: 1.05em;
  }
}

@media (min-width: 1024px) {
.provider-content {
    max-width: 900px;
    margin: 0 auto;
    padding: 0 0 120px;
  }
  
.provider-image-carousel {
    height: 350px;
    border-radius: 20px;
  }
    
  .social-links {
    gap: 18px;
  }
  
  .social-link {
    width: 44px;
    height: 44px;
    font-size: 1.3em;
  }
  
  .provider-actions {
    max-width: 700px;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 20px 20px 0 0; /* This curves only the top corners */
  }
  
  .primary-action,
  .secondary-action {
    padding: 16px;
    font-size: 1.05em;
  }
}

@media (min-width: 1440px) {
.provider-content {
    max-width: 1000px;
  }
    
.provider-image-carousel {
    height: 400px;
  }
    
  .provider-actions {
    border-radius: 12px 12px 0 0; /* This curves only the top corners */
    max-width: 800px;
    left: 50%;
    transform: translateX(-50%);
  }
}

  `;
  document.head.appendChild(style);
}

// Helper functions for provider page
function getFacilityIcon(facilityName) {
  const icons = {
    'Emergency Service': 'ambulance',
    'Guarantee': 'shield-alt',
    'Equipment': 'tools',
    'Certified Technicians': 'certificate',
    'Safety First': 'shield-alt',
    'Warranty': 'file-contract',
    'Custom Work': 'ruler-combined',
    'Quality Materials': 'gem',
    'On-site Service': 'home',
    'Rapid Response': 'bolt',
    'Waterproofing': 'umbrella',
    'Free Estimates': 'file-invoice-dollar',
    'Smart Home': 'home',
    'Energy Audit': 'lightbulb',
    'Safety Checks': 'clipboard-check'
  };
  
  return icons[facilityName] || 'check-circle';
}

// Function to filter providers based on category
function filterProviders(category) {
  const providersGrid = document.getElementById('providersGrid');
  let filteredData = serviceProvidersData;

  if (category !== 'All') {
    filteredData = filteredData.filter(provider => provider.category === category);
  }

  providersGrid.innerHTML = renderProviderCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function sortProviders(criteria) {
  const providersGrid = document.getElementById('providersGrid');
  let sortedData = [...serviceProvidersData];

  switch(criteria) {
    case 'rating':
      sortedData.sort((a, b) => parseFloat(b.details.rating) - parseFloat(a.details.rating));
      break;
    case 'name':
      sortedData.sort((a, b) => a.details.name.localeCompare(b.details.name));
      break;
    case 'distance':
      // This would require location data to be implemented
      break;
  }

  providersGrid.innerHTML = renderProviderCards(sortedData);
  toggleSortOptions();
}

function toggleSortOptions() {
  const sortOptions = document.getElementById('sortOptionsContainer');
  sortOptions.classList.toggle('active');
}

function hideProvidersOverlay() {
  const overlay = document.getElementById('providersOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
    }, 300);
  }
}

// Shared helper functions
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

function performSearch(data, query, gridElement, filterButtons, renderFunction) {
  let filteredData = data;
  let activeCategory = 'All'; // Default to 'All'
  
  if (query) {
    const searchTerm = query.toLowerCase();
    
    // First try to match exact category names
    const categoryMatch = Array.from(filterButtons).find(button => {
      const category = button.getAttribute('data-category').toLowerCase();
      return category === searchTerm;
    });
    
    if (categoryMatch) {
      activeCategory = categoryMatch.getAttribute('data-category');
    } else {
      // If no exact category match, filter the data
      filteredData = filteredData.filter(item => 
        item.details.name.toLowerCase().includes(searchTerm) ||
        item.category.toLowerCase().includes(searchTerm) ||
        item.metropolis.toLowerCase().includes(searchTerm) ||
        (item.details.description && item.details.description.toLowerCase().includes(searchTerm)) ||
        (item.details.tags && item.details.tags.some(tag => tag.toLowerCase().includes(searchTerm)))
      );
      
      // Find if any provider's name matches exactly (for more precise filtering)
      const exactNameMatch = data.find(item => 
        item.details.name.toLowerCase() === searchTerm
      );
      
      if (exactNameMatch) {
        activeCategory = exactNameMatch.category;
      } else if (filteredData.length > 0) {
        // If we have filtered results, use the first result's category
        activeCategory = filteredData[0].category;
      }
    }
  }
  
  // Apply the category filter if not 'All'
  if (activeCategory !== 'All') {
    filteredData = filteredData.filter(item => item.category === activeCategory);
  }
  
  // Update the UI
  gridElement.innerHTML = renderFunction(filteredData);
  
  // Update active filter button
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === activeCategory) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function initializeOverlayVoiceSearch(inputElement, callback) {
  const voiceSearchButton = document.getElementById('voiceSearchButton');
  if (!voiceSearchButton) return;

  voiceSearchButton.addEventListener('click', () => {
    if (!('webkitSpeechRecognition' in window)) {
      alert('Voice search is not supported in your browser');
      return;
    }

    const recognition = new webkitSpeechRecognition();
    recognition.continuous = false;
    recognition.interimResults = false;

    voiceSearchButton.classList.add('active');
    
    recognition.onstart = () => {
      voiceSearchButton.innerHTML = '<i class="fas fa-microphone-slash"></i>';
    };

    recognition.onresult = (event) => {
      const transcript = event.results[0][0].transcript;
      inputElement.value = transcript;
      callback(transcript);
      voiceSearchButton.innerHTML = '<i class="fas fa-microphone"></i>';
      voiceSearchButton.classList.remove('active');
    };

    recognition.onerror = (event) => {
      voiceSearchButton.innerHTML = '<i class="fas fa-microphone"></i>';
      voiceSearchButton.classList.remove('active');
      console.error('Voice recognition error', event.error);
    };

    recognition.onend = () => {
      voiceSearchButton.innerHTML = '<i class="fas fa-microphone"></i>';
      voiceSearchButton.classList.remove('active');
    };

    recognition.start();
  });
}

function openFullScreen(imageUrl) {
  event.stopPropagation();
  const fullScreenDiv = document.createElement('div');
  fullScreenDiv.className = 'full-screen-image';
  fullScreenDiv.innerHTML = `
    <img src="${imageUrl}" alt="Full screen">
    <button class="close-full-screen" onclick="document.querySelector('.full-screen-image').remove()">
      <i class="fas fa-times"></i>
    </button>
  `;
  document.body.appendChild(fullScreenDiv);
  
  setTimeout(() => {
    fullScreenDiv.classList.add('active');
    const img = fullScreenDiv.querySelector('img');
    if (img) {
      img.style.opacity = '1';
      img.style.transform = 'scale(1)';
    }
  }, 10);
}

function changeCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentSlide(index) {
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

function copyToClipboard(text) {
  event.stopPropagation();
  navigator.clipboard.writeText(text).then(() => {
    const button = event.target.closest('button');
    if (button) {
      const originalText = button.textContent;
      button.textContent = 'Copied!';
      setTimeout(() => {
        button.textContent = originalText;
      }, 2000);
    }
  }).catch(err => {
    console.error('Failed to copy text: ', err);
  });
}

function shareProvider(providerName) {
  event.stopPropagation();
  const provider = serviceProvidersData.find(prov => prov.details.name === providerName);
  if (!provider) return;

  if (navigator.share) {
    navigator.share({
      title: provider.details.name,
      text: `Check out ${provider.details.name} - ${provider.details.description}`,
      url: provider.details.website || window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    // Fallback for browsers that don't support Web Share API
    const shareUrl = provider.details.website || window.location.href;
    const shareText = `Check out ${provider.details.name}: ${shareUrl}`;
    prompt('Copy this link to share:', shareText);
  }
}

function showChatSupport(providerName) {
  event.stopPropagation();
  alert(`Chat support for ${providerName} would open here in a real implementation.`);
}

const realEstateData = [
  // Offline Properties (Residential)
  {
    type: "Property",
    category: "Residential",
    subcategory: "Apartment",
    metropolis: "Mumbai",
    details: {
      name: "Luxury Bayview Apartments",
      cardImages: [
        "https://i.pinimg.com/originals/23/a3/f6/23a3f6d6d618427e899a29307026a3cf.jpg",
        "https://example.com/apartment-card2.jpg"
      ],
      storeImages: [
        "https://example.com/apartment1.jpg",
        "https://example.com/apartment2.jpg",
        "https://example.com/apartment3.jpg"
      ],
      description: "Premium sea-facing apartments with world-class amenities in the heart of Mumbai. 2-4 BHK configurations available.",
      address: "Marine Drive, Mumbai - 400020",
      price: "₹4.2 Cr - ₹8.5 Cr",
      area: "850 sq.ft - 1800 sq.ft",
      possession: "Ready to Move",
      mapLink: "https://maps.example.com/bayview",
      developer: "Omkar Realtors",
      amenities: [
        { name: "Swimming Pool", icon: "swimming-pool" },
        { name: "Gymnasium", icon: "dumbbell" },
        { name: "24/7 Security", icon: "shield-alt" }
      ],
      specifications: [
        { name: "Bedrooms", value: "2-4 BHK" },
        { name: "Bathrooms", value: "2-4" },
        { name: "Parking", value: "2 Covered" }
      ],
      contact: {
        email: "sales@bayviewmumbai.com",
        phone: "+91-9876543210",
        agent: "Rahul Sharma"
      },
      rating: "4.8",
      tags: ["Sea View", "Premium", "Ready Possession"]
    }
  },
  // Other properties follow the same pattern...
];

// Function to show the real estate overlay
function showRealEstateOverlay() {
  let overlay = document.getElementById('realEstateOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'realEstateOverlay';
    overlay.className = 'real-estate-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(realEstateData.map(item => item.category))];

  overlay.innerHTML = `
    <div class="real-estate-content">
      <div class="real-estate-header">
        <div class="header-title-container">
          <h2>Real Estate</h2>
          <div class="header-subtitle">Find properties across categories</div>
        </div>
        <div class="header-buttons-container">
          <button class="sort-button" onclick="toggleSortOptions()">
            <i class="fas fa-sliders-h"></i>
          </button>
          <button class="close-real-estate" onclick="hideRealEstateOverlay()">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      
      <div class="sort-options-container" id="sortOptionsContainer">
        <div class="sort-option" onclick="sortRealEstate('rating')">
          <i class="fas fa-star"></i> Highest Rated
        </div>
        <div class="sort-option" onclick="sortRealEstate('price')">
          <i class="fas fa-rupee-sign"></i> Price
        </div>
        <div class="sort-option" onclick="sortRealEstate('name')">
          <i class="fas fa-sort-alpha-down"></i> Alphabetical
        </div>
        <div class="sort-option" onclick="sortRealEstate('distance')">
          <i class="fas fa-map-marker-alt"></i> Nearest
        </div>
      </div>

      <div class="real-estate-filter-container">
        <div class="category-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterRealEstate('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="real-estate-grid" id="realEstateGrid">
        ${renderRealEstateCards(realEstateData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="realEstateExploreInput" placeholder="Search for properties..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  // Initialize functionality
  const exploreInput = document.getElementById('realEstateExploreInput');
  const realEstateGrid = document.getElementById('realEstateGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performSearch(realEstateData, query, realEstateGrid, filterButtons, renderRealEstateCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(realEstateData, exploreInput.value, realEstateGrid, filterButtons, renderRealEstateCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(realEstateData, query, realEstateGrid, filterButtons, renderRealEstateCards);
  });

  function performSearch(data, query, gridElement, filterButtons, renderFunction) {
  if (!query.trim()) {
    gridElement.innerHTML = renderFunction(data);
    // Reset to 'All' filter when search is cleared
    filterButtons.forEach(btn => {
      btn.classList.remove('active');
      if (btn.dataset.category === 'All') btn.classList.add('active');
    });
    return;
  }

  // Normalize and preprocess the query
  const normalizedQuery = query.toLowerCase().trim()
    .replace(/[^\w\s₹]/g, '') // Remove special chars except ₹
    .replace(/\s+/g, ' ');    // Collapse multiple spaces

  // Enhanced extraction with synonyms and broader matching
  const categorySynonyms = {
    'resid': 'Residential',
    'home': 'Residential',
    'house': 'Residential',
    'apart': 'Residential',
    'flat': 'Residential',
    'villa': 'Residential',
    'comm': 'Commercial',
    'office': 'Commercial',
    'shop': 'Commercial',
    'retail': 'Commercial',
    'indust': 'Industrial',
    'factory': 'Industrial',
    'warehouse': 'Industrial',
    'land': 'Land',
    'plot': 'Land',
    'agricult': 'Agricultural',
    'farm': 'Agricultural'
  };

  const citySynonyms = {
    'ncr': 'Delhi',
    'bombay': 'Mumbai',
    'bengaluru': 'Bangalore',
    'blr': 'Bangalore',
    'calcutta': 'Kolkata',
    'madras': 'Chennai'
  };

  // Advanced query parsing with priority scoring
  let categoryMatch, cityMatch, priceMatch, featureMatch, typeMatch;

  // 1. Category detection with synonyms and fuzzy matching
  const categoryPattern = new RegExp(
    `\\b(${Object.keys(categorySynonyms).concat([
      'residential', 'commercial', 'industrial', 
      'land', 'agricultural'
    ]).join('|')})\\b`, 'i');
  
  categoryMatch = normalizedQuery.match(categoryPattern);
  if (categoryMatch) {
    const matchedTerm = categoryMatch[0].toLowerCase();
    categoryMatch[0] = categorySynonyms[matchedTerm] || matchedTerm;
    // Capitalize first letter
    categoryMatch[0] = categoryMatch[0].charAt(0).toUpperCase() + categoryMatch[0].slice(1).toLowerCase();
  }

  // 2. City detection with synonyms
  const cityPattern = new RegExp(
    `\\b(${Object.keys(citySynonyms).concat([
      'mumbai', 'delhi', 'bangalore', 'hyderabad', 
      'ahmedabad', 'chennai', 'kolkata', 'pune',
      'jaipur', 'surat', 'lucknow', 'kanpur', 'nagpur',
      'indore', 'thane', 'bhopal', 'visakhapatnam',
      'patna', 'vadodara', 'ghaziabad', 'ludhiana',
      'agra', 'nashik', 'faridabad', 'meerut', 'rajkot',
      'varanasi', 'srinagar', 'aurangabad', 'dhanbad',
      'amritsar', 'noida', 'allahabad', 'ranchi',
      'howrah', 'coimbatore', 'jabalpur', 'gwalior',
      'vijayawada', 'jodhpur', 'madurai', 'raipur',
      'kota', 'guwahati', 'chandigarh', 'solapur'
    ]).join('|')})\\b`, 'i'
  );
  
  cityMatch = normalizedQuery.match(cityPattern);
  if (cityMatch) {
    const matchedTerm = cityMatch[0].toLowerCase();
    cityMatch[0] = citySynonyms[matchedTerm] || matchedTerm;
    // Capitalize first letter
    cityMatch[0] = cityMatch[0].charAt(0).toUpperCase() + cityMatch[0].slice(1).toLowerCase();
  }

  // 3. Price detection with multiple formats
  priceMatch = normalizedQuery.match(/(?:rs\.?|₹|inr)\s*(\d+)/i) || 
               normalizedQuery.match(/(\d+)\s*(?:rs|rupees|inr)/i) ||
               normalizedQuery.match(/\b(\d+)\s*(?:price|cost)/i) ||
               normalizedQuery.match(/\b(under|below|less than|up to|above|over|more than)\s*₹?\s*(\d+)/i);

  // 4. Feature detection
  const featureTerms = [
    'pool', 'gym', 'security', 'park', 'garden', 'lift',
    'parking', 'club', 'play', 'power', 'water', 'modular',
    'furnish', 'view', 'sea', 'lake', 'river', 'mountain',
    'central', 'ac', 'modular', 'wardrobe', 'modular'
  ];
  featureMatch = normalizedQuery.match(new RegExp(`\\b(${featureTerms.join('|')})\\b`, 'i'));

  // 5. Property type detection
  const typeTerms = [
    'apartment', 'villa', 'bunglow', 'penthouse', 'studio',
    'duplex', 'triplex', 'rowhouse', 'plot', 'shop',
    'office', 'showroom', 'godown', 'warehouse', 'factory'
  ];
  typeMatch = normalizedQuery.match(new RegExp(`\\b(${typeTerms.join('|')})\\b`, 'i'));

  // Advanced filtering with scoring system
  const filteredProperties = data.map(property => {
    let score = 0;
    let matches = [];
    let mismatches = [];

    // Category matching (high importance)
    if (categoryMatch) {
      const categoryLower = property.category.toLowerCase();
      if (categoryLower.includes(categoryMatch[0].toLowerCase())) {
        score += 30;
        matches.push(`Category: ${property.category}`);
      } else {
        mismatches.push(`Category: ${categoryMatch[0]}`);
        return null; // Exclude if category doesn't match
      }
    }

    // City matching (high importance)
    if (cityMatch) {
      const cityLower = property.metropolis.toLowerCase();
      if (cityLower.includes(cityMatch[0].toLowerCase())) {
        score += 30;
        matches.push(`City: ${property.metropolis}`);
      } else {
        mismatches.push(`City: ${cityMatch[0]}`);
        return null; // Exclude if city doesn't match
      }
    }

    // Price matching (medium importance)
    if (priceMatch) {
      let price;
      if (priceMatch[2]) { // For "under ₹1000" type patterns
        price = parseInt(priceMatch[2]);
        const comparison = priceMatch[1].toLowerCase();
        
        const priceRange = property.details.price.match(/\d+/g);
        if (priceRange && priceRange.length > 0) {
          const minPrice = parseInt(priceRange[0].replace(/,/g, ''));
          const maxPrice = priceRange.length > 1 ? parseInt(priceRange[1].replace(/,/g, '')) : minPrice;
          
          if (comparison.includes('under') || comparison.includes('below') || 
              comparison.includes('less than') || comparison.includes('up to')) {
            if (minPrice <= price) {
              score += 20;
              matches.push(`Price: ${comparison} ₹${price}`);
            } else {
              mismatches.push(`Price: ${comparison} ₹${price}`);
            }
          } else if (comparison.includes('above') || comparison.includes('over') || 
                    comparison.includes('more than')) {
            if (maxPrice >= price) {
              score += 20;
              matches.push(`Price: ${comparison} ₹${price}`);
            } else {
              mismatches.push(`Price: ${comparison} ₹${price}`);
            }
          }
        }
      } else {
        price = parseInt(priceMatch[1]);
        const priceRange = property.details.price.match(/\d+/g);
        if (priceRange && priceRange.length > 0) {
          const minPrice = parseInt(priceRange[0].replace(/,/g, ''));
          if (minPrice <= price) {
            score += 20;
            matches.push(`Price: ≤₹${price}`);
          } else {
            mismatches.push(`Price: ≤₹${price}`);
          }
        }
      }
    }

    // Feature matching (medium importance)
    if (featureMatch) {
      const hasFeature = property.details.amenities.some(amenity => 
        amenity.name.toLowerCase().includes(featureMatch[0]) ||
        property.details.tags.some(tag => tag.toLowerCase().includes(featureMatch[0]))
      );
      
      if (hasFeature) {
        score += 15;
        matches.push(`Feature: ${featureMatch[0]}`);
      } else {
        mismatches.push(`Feature: ${featureMatch[0]}`);
      }
    }

    // Property type matching (medium importance)
    if (typeMatch) {
      const typeLower = property.subcategory.toLowerCase();
      if (typeLower.includes(typeMatch[0].toLowerCase())) {
        score += 15;
        matches.push(`Type: ${property.subcategory}`);
      } else {
        mismatches.push(`Type: ${typeMatch[0]}`);
      }
    }

    // General text search (fallback)
    if (!categoryMatch && !cityMatch && !priceMatch && !featureMatch && !typeMatch) {
      const searchFields = [
        property.category,
        property.subcategory,
        property.metropolis,
        property.details.name,
        property.details.description,
        property.details.address,
        ...property.details.tags,
        ...property.details.amenities.map(a => a.name),
        ...property.details.specifications.map(s => `${s.name} ${s.value}`)
      ].join(' ').toLowerCase();
      
      if (searchFields.includes(normalizedQuery)) {
        score += 50; // High score for direct match
        matches.push(`General match`);
      } else {
        // Try partial matching
        const queryWords = normalizedQuery.split(/\s+/);
        const matchedWords = queryWords.filter(word => 
          word.length > 3 && searchFields.includes(word)
        );
        if (matchedWords.length > 0) {
          score += matchedWords.length * 5;
          matches.push(`Partial match: ${matchedWords.join(', ')}`);
        } else {
          return null; // No match at all
        }
      }
    }

    return {
      property,
      score,
      matches,
      mismatches
    };
  })
  .filter(Boolean) // Remove null entries
  .sort((a, b) => b.score - a.score) // Sort by score descending
  .map(item => item.property); // Extract just the properties

  // Enhanced empty state handling
  if (filteredProperties.length === 0) {
    const suggestions = generateRealEstateSearchSuggestions(query, data);
    gridElement.innerHTML = `
      <div class="no-results">
        <div class="no-results-icon">🏠</div>
        <h3>No properties found</h3>
        <p>We couldn't find properties matching "${query}"</p>
        ${suggestions.length > 0 ? `
          <div class="suggestions">
            <p>Try one of these instead:</p>
            <ul>
              ${suggestions.map(suggestion => `
                <li onclick="document.getElementById('realEstateExploreInput').value = '${suggestion}'; performSearch(realEstateData, '${suggestion}', document.getElementById('realEstateGrid'), document.querySelectorAll('.filter-button'), renderRealEstateCards)">
                  ${suggestion}
                </li>
              `).join('')}
            </ul>
          </div>
        ` : ''}
        <button class="clear-search" onclick="document.getElementById('realEstateExploreInput').value = ''; performSearch(realEstateData, '', document.getElementById('realEstateGrid'), document.querySelectorAll('.filter-button'), renderRealEstateCards)">
          Clear search
        </button>
      </div>
    `;
  } else {
    gridElement.innerHTML = renderFunction(filteredProperties);
  }

  // Update active filter button based on search
  updateRealEstateActiveFilterButton(filterButtons, categoryMatch, typeMatch);
}

function updateRealEstateActiveFilterButton(filterButtons, categoryMatch, typeMatch) {
  // Reset all buttons first
  filterButtons.forEach(btn => btn.classList.remove('active'));
  
  // Determine which filter to activate
  if (categoryMatch) {
    // Find matching filter button
    const matchingButton = Array.from(filterButtons).find(btn => 
      btn.dataset.category.toLowerCase() === categoryMatch[0].toLowerCase()
    );
    
    if (matchingButton) {
      matchingButton.classList.add('active');
    } else {
      // If no exact match, try to find a partial match
      const partialMatch = Array.from(filterButtons).find(btn => 
        btn.dataset.category.toLowerCase().includes(categoryMatch[0].toLowerCase()) ||
        categoryMatch[0].toLowerCase().includes(btn.dataset.category.toLowerCase())
      );
      
      if (partialMatch) {
        partialMatch.classList.add('active');
      } else {
        // Default to 'All' if no match found
        filterButtons[0].classList.add('active');
      }
    }
  } else if (typeMatch) {
    // Try to infer category from property type
    const typeToCategory = {
      'apartment': 'Residential',
      'villa': 'Residential',
      'bunglow': 'Residential',
      'penthouse': 'Residential',
      'studio': 'Residential',
      'duplex': 'Residential',
      'triplex': 'Residential',
      'rowhouse': 'Residential',
      'plot': 'Land',
      'shop': 'Commercial',
      'office': 'Commercial',
      'showroom': 'Commercial',
      'godown': 'Industrial',
      'warehouse': 'Industrial',
      'factory': 'Industrial'
    };
    
    const inferredCategory = typeToCategory[typeMatch[0].toLowerCase()];
    if (inferredCategory) {
      const matchingButton = Array.from(filterButtons).find(btn => 
        btn.dataset.category === inferredCategory
      );
      if (matchingButton) matchingButton.classList.add('active');
    } else {
      filterButtons[0].classList.add('active');
    }
  } else {
    // Default to 'All' if no specific category or type was mentioned
    filterButtons[0].classList.add('active');
  }
}

// Helper function to generate real estate search suggestions
function generateRealEstateSearchSuggestions(query, properties) {
  const suggestions = new Set();
  
  // 1. Suggest similar categories
  const categories = [...new Set(properties.map(p => p.category))];
  categories.forEach(cat => {
    if (cat.toLowerCase().includes(query.toLowerCase().substring(0, 3))) {
      suggestions.add(cat);
    }
  });

  // 2. Suggest property types
  const types = [...new Set(properties.map(p => p.subcategory))];
  types.forEach(type => {
    if (type && type.toLowerCase().includes(query.toLowerCase())) {
      suggestions.add(type);
    }
  });

  // 3. Suggest cities
  const cities = [...new Set(properties.map(p => p.metropolis))];
  cities.forEach(city => {
    if (city && city.toLowerCase().includes(query.toLowerCase())) {
      suggestions.add(`Properties in ${city}`);
    }
  });

  // 4. Add price-based suggestions if no price was mentioned
  if (!/(rs|₹|inr|price|cost)/i.test(query)) {
    suggestions.add(`${query} under ₹50 lakh`);
    suggestions.add(`${query} under ₹1 crore`);
    suggestions.add(`affordable ${query}`);
  }

  // 5. Add feature suggestions
  const allFeatures = [];
  properties.forEach(property => {
    property.details.amenities.forEach(amenity => {
      allFeatures.push(amenity.name);
    });
    property.details.tags.forEach(tag => {
      allFeatures.push(tag);
    });
  });
  
  const uniqueFeatures = [...new Set(allFeatures)];
  uniqueFeatures.forEach(feature => {
    if (feature.toLowerCase().includes(query.toLowerCase())) {
      suggestions.add(`${feature} properties`);
    }
  });

  return Array.from(suggestions).slice(0, 5); // Return top 5 suggestions
}

  setTimeout(() => overlay.classList.add('active'), 10);

  // Add styles
  const style = document.createElement('style');
  style.textContent = `
.real-estate-overlay {
  font-family: 'Poppins', sans-serif;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.real-estate-overlay::-webkit-scrollbar {
  display: none;
}

.real-estate-overlay.active {
  opacity: 1;
  visibility: visible;
}

.real-estate-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.real-estate-header {
  padding: 12px 20px;
  background: rgba(26, 26, 31, 0.98);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.header-title-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}

.real-estate-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.header-subtitle {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.8em;
  margin-top: 2px;
  font-weight: 400;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.close-real-estate {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 1em;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-real-estate:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.close-real-estate:active {
  transform: scale(0.95);
}

.sort-button {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  font-size: 0.9em;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.sort-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
}

.sort-button:active {
  transform: translateY(0);
}

.sort-options-container {
  position: fixed;
  top: 72px;
  right: 20px;
  background: #2a2a35;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  z-index: 40;
  padding: 8px 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.sort-options-container.active {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.sort-option {
  padding: 12px 20px;
  color: white;
  font-size: 0.9em;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.sort-option:hover {
  background: rgba(255, 215, 0, 0.1);
  color: var(--primary-color);
}

.sort-option i {
  width: 20px;
  text-align: center;
}

.real-estate-filter-container {
  position: fixed;
  top: 72px;
  left: 0;
  right: 0;
  width: 100%;
  z-index: 10;
  background: #1a1a1f;
  padding: 12px 0;
  box-sizing: border-box;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
  border-bottom: 1px solid rgba(255, 215, 0, 0.1);
}

.real-estate-filter-container::-webkit-scrollbar {
  display: none;
}

.category-filter-buttons {
  display: inline-flex;
  padding: 0 16px;
  gap: 8px;
  white-space: nowrap;
}

.filter-button {
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: rgba(255, 255, 255, 0.9);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 20px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  margin: 0;
}

.filter-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: scale(1.03);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: #000;
  font-weight: 600;
  border-color: var(--primary-color);
  box-shadow: 0 4px 10px rgba(255, 215, 0, 0.3);
}

/* Add some space at the end for better scrolling */
.category-filter-buttons::after {
  content: '';
  display: block;
  min-width: 16px;
  height: 1px;
}

@media (min-width: 1024px) {
.real-estate-filter-container {
   top: 72px;
    }
}

.real-estate-grid {
  margin-top: 90px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  padding-bottom: 140px;
}

.real-estate-card {
  background: #ffffff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  position: relative;
  border: none;
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
  height: 100%;
}

.real-estate-card:hover {
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.real-estate-card:active {
  transform: translateY(-1px);
  transition: transform 0.1s ease;
}

.image-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 12px 12px 0 0;
  margin-bottom: 0;
  box-shadow: none;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease, transform 0.5s ease;
  transform: scale(1.03);
  will-change: opacity, transform;
}

.carousel-item.active {
  opacity: 1;
  transform: scale(1);
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.real-estate-card:hover .carousel-item.active img {
  transform: scale(1.05);
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  -webkit-tap-highlight-color: transparent;
}

.image-carousel:hover .carousel-control {
  opacity: 0.9;
}

.carousel-control:hover {
  background: rgba(0, 0, 0, 0.85);
  transform: translateY(-50%) scale(1.1);
}

.carousel-control:active {
  transform: translateY(-50%) scale(0.95);
}

.carousel-control.prev {
  left: 12px;
}

.carousel-control.next {
  right: 12px;
}

.carousel-pagination {
  position: absolute;
  bottom: 12px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
}

.real-estate-card-content {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.real-estate-card-header {
  display: flex;
  flex-direction: column;
  padding: 0;
  background: transparent;
  border-bottom: none;
}

.real-estate-name {
  font-family: poppins;
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin: 0 0 4px 0;
  line-height: 1.3;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.real-estate-meta {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 14px;
  margin-bottom: 8px;
}

.real-estate-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
}

.real-estate-rating .star {
  color: #ffc107;
  font-weight: 600;
}

.real-estate-rating .count {
  color: #666;
  font-size: 14px;
}

.real-estate-info-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
  margin: 4px 0;
}

.info-item {
  flex: 1 1 calc(50% - 6px);
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px;
  border-radius: 10px;
  background: rgba(255, 215, 0, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.1);
  transition: all 0.3s ease;
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

.info-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: var(--primary-color);
}

.info-icon i {
  font-size: 14px;
}

.info-content {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.info-label {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #ffc107;
  margin-bottom: 6px;
}

.info-value {
  font-size: 13px;
  font-weight: 500;
  color: #000;
  line-height: 1.4;
  display: block;
  margin-top: 0;
}

.info-value + .info-value {
  margin-top: 6px;
  position: relative;
  padding-top: 6px;
}

.info-value + .info-value::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 215, 0, 0.1);
}

.info-item:hover {
  background: rgba(255, 215, 0, 0.08);
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.real-estate-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
  margin: 12px 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.tag-container {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 10px 0;
}

.tag {
  background: #f5f5f5;
  color: #666;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
  transition: background-color 0.2s ease;
}

.tag:hover {
  background: #eeeeee;
}

.real-estate-card-actions {
  display: flex;
  border-top: 1px solid #eee;
  padding: 0;
  background: transparent;
  margin-top: auto;
}

.action-button {
  flex: 1;
  background: transparent;
  color: #333;
  font-weight: 500;
  padding: 14px 0;
  border: none;
  border-radius: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  position: relative;
  transition: background-color 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.action-button:hover {
  background: #f5f5f5;
}

.action-button:active {
  background: #eeeeee;
}

.action-button i {
  color: #333;
  font-size: 16px;
}

.action-button:first-child {
  border-right: 1px solid #eee;
}

@media (max-width: 991px) {
  .real-estate-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  }
}

@media (max-width: 768px) {
  .real-estate-grid {
    grid-template-columns: 1fr;
    padding: 0 12px;
    margin-top: 120px;
    gap: 16px;
  }
  
  .real-estate-card {
    margin-bottom: 0;
    border-radius: 12px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .image-carousel {
    height: 180px;
    border-radius: 12px 12px 0 0;
  }
  
  .real-estate-card-content {
    padding: 14px;
  }
  
  .real-estate-name {
    font-size: 16px;
  }
  
  .real-estate-description {
    font-size: 13px;
    margin: 10px 0;
  }
  
  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }
  
  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }
  
  .carousel-control {
    width: 32px;
    height: 32px;
    opacity: 0.7;
  }
  
  .image-carousel .carousel-control {
    opacity: 0.7;
  }
  
  .pagination-dot {
    width: 6px;
    height: 6px;
  }
  
  .pagination-dot.active {
    width: 16px;
  }
}

@media (max-width: 480px) {
  .real-estate-card {
    margin-bottom: 0px;
  }
  
  .image-carousel {
    height: 160px;
  }
  
  .real-estate-card-content {
    padding: 12px;
  }
  
  .real-estate-description {
    -webkit-line-clamp: 2;
    margin: 8px 0;
  }
    
  .tag-container {
    margin: 8px 0;
  }
  
  .tag {
    padding: 2px 8px;
    font-size: 10px;
  }
  
  .action-button {
    padding: 10px 0;
    font-size: 12px;
  }
  
  .action-button i {
    font-size: 14px;
  }
  
  .carousel-control {
    width: 28px;
    height: 28px;
  }
  
  .real-estate-filter-container {
    padding: 12px;
  }
  
  .filter-button {
    padding: 9px 12px;
    font-size: 14px;
  }
}

@media (max-width: 768px) and (orientation: portrait) {
  .real-estate-grid {
    margin-top: 130px;
  }
  
  .real-estate-card {
    border-radius: 10px;
  }
}

.real-estate-card {
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .real-estate-card,
  .real-estate-card:hover,
  .carousel-item,
  .carousel-item.active,
  .carousel-item img,
  .carousel-control,
  .carousel-control:hover,
  .pagination-dot {
    transition: none;
    transform: none;
    animation: none;
  }
}

.real-estate-card:focus {
  outline: 2px solid var(--primary-color);
  outline-offset: 3px;
}

@media (prefers-color-scheme: dark) {
  .real-estate-card {
    background: #2a2a35;
  }
  
  .real-estate-name {
    color: #fff;
  }
  
  .real-estate-meta,
  .real-estate-description,
  .real-estate-rating .count {
    color: rgba(255, 255, 255, 0.7);
  }
  
  .tag {
    background: #3a3a45;
    color: rgba(255, 255, 255, 0.8);
  }
  
  .tag:hover {
    background: #4a4a55;
  }
  
  .real-estate-card-actions {
    border-top: 1px solid #3a3a45;
  }
  
  .action-button {
    color: #fff;
  }
  
  .action-button i {
    color: #fff;
  }
  
  .action-button:first-child {
    border-right: 1px solid #3a3a45;
  }
  
  .action-button:hover {
    background: #3a3a45;
  }
  
  .info-item {
    background: rgba(255, 215, 0, 0.05);
    border-color: rgba(255, 215, 0, 0.2);
  }
  
  .info-label {
    color: var(--primary-color);
  }
  
  .info-value {
    color: #fff;
  }
}

.chat-container {
  position: fixed;
  bottom: 0px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

.input-container {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#realEstateExploreInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 12px 50px 12px 20px;
  color: #fff;
  font-size: 0.95em;
  width: 100%;
  transition: border-color 0.3s ease;
}

#realEstateExploreInput:focus {
  border-color: rgba(255, 255, 255, 0.5);
}

.voice-search {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-search:hover {
  opacity: 0.8;
}

.voice-search.active {
  color: #ffd700;
  animation: pulse 1.5s infinite;
}

.voice-search i { 
  font-size: 1.5em; 
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 0px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #realEstateExploreInput {
    height: 70px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 1.7em;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #realEstateExploreInput {
    bottom: 0px;
    width: 60%;
    max-width: 1200px;
  }
  
  .real-estate-grid {
    padding-bottom: 180px;
  }
}

@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .real-estate-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .real-estate-card {
    width: 100%;
    margin: 0;
  }

  .real-estate-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #realEstateExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}

.full-screen-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.95);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.full-screen-image.active {
  opacity: 1;
  visibility: visible;
}

.full-screen-image img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 10px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  transform: scale(0.95);
  opacity: 0;
  transition: all 0.4s ease;
}

.full-screen-image.active img {
  transform: scale(1);
  opacity: 1;
}

.close-full-screen {
  position: absolute;
  top: 24px; 
  right: 24px;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close-full-screen:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}

.no-results {
  text-align: center;
  padding: 40px 20px;
  grid-column: 1 / -1;
}

.no-results-icon {
  font-size: 48px;
  margin-bottom: 20px;
}

.no-results h3 {
  color: var(--primary-color);
  margin-bottom: 10px;
}

.no-results p {
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 20px;
}

.suggestions {
  margin-top: 20px;
}

.suggestions p {
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 10px;
}

.suggestions ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.suggestions li {
  padding: 10px 15px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.2s ease;
}

.suggestions li:hover {
  background: rgba(255, 215, 0, 0.1);
}

.clear-search {
  margin-top: 20px;
  padding: 10px 20px;
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.clear-search:hover {
  background: rgba(255, 215, 0, 0.15);
}

/* ===== Tablet (768px - 1024px) ===== */
@media (min-width: 768px) and (max-width: 1024px) {
  .real-estate-grid {
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 16px;
    padding-bottom: 120px;
  }

  .real-estate-card {
    border-radius: 14px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }

  .image-carousel {
    height: 180px;
  }

  .real-estate-card-content {
    padding: 14px;
  }

  .real-estate-name {
    font-size: 16px;
    margin-bottom: 4px;
  }

  .real-estate-meta {
    font-size: 13px;
    margin-bottom: 8px;
  }

  .info-item {
    padding: 10px;
    gap: 8px;
  }

  .info-icon {
    width: 28px;
    height: 28px;
    border-radius: 6px;
  }

  .info-icon i {
    font-size: 13px;
  }

  .info-label {
    font-size: 10px;
    margin-bottom: 4px;
  }

  .info-value {
    font-size: 12px;
  }

  .real-estate-description {
    font-size: 13px;
    margin: 10px 0;
    -webkit-line-clamp: 2;
  }

  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }

  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }

  .real-estate-filter-container {
   top: 72px;
    }
}

/* ===== Laptop & Desktop (1025px+) ===== */
@media (min-width: 1025px) {
  .real-estate-grid {
    grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
    gap: 18px;
    padding-bottom: 140px;
  }

  .real-estate-card {
    border-radius: 16px;
    box-shadow: 0 5px 12px rgba(0, 0, 0, 0.12);
  }

  .image-carousel {
    height: 200px;
  }

  .real-estate-card-content {
    padding: 16px;
  }

  .real-estate-name {
    font-size: 17px;
    margin-bottom: 4px;
  }

  .real-estate-meta {
    font-size: 14px;
    margin-bottom: 8px;
  }

  .info-item {
    padding: 12px;
    gap: 10px;
  }

  .info-icon {
    width: 30px;
    height: 30px;
    border-radius: 8px;
  }

  .info-icon i {
    font-size: 14px;
  }

  .info-label {
    font-size: 11px;
    margin-bottom: 6px;
  }

  .info-value {
    font-size: 13px;
  }

  .real-estate-description {
    font-size: 14px;
    margin: 12px 0;
    -webkit-line-clamp: 2;
  }

  .tag {
    padding: 4px 10px;
    font-size: 12px;
  }

  .action-button {
    padding: 14px 0;
    font-size: 14px;
  }
}
  `;
  document.head.appendChild(style);
}

function renderRealEstateCards(filteredData) {
  return filteredData.map(item => {
    const carouselHTML = item.details.cardImages && item.details.cardImages.length > 0 ? `
      <div class="image-carousel">
        ${item.details.cardImages.map((image, index) => `
          <div class="carousel-item ${index === 0 ? 'active' : ''}">
            <img src="${image}" alt="${item.details.name}" onclick="openFullScreen('${image}')">
          </div>
        `).join('')}
        <div class="carousel-pagination">
          ${item.details.cardImages.map((_, index) => `
            <div class="pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentSlide(${index + 1}); event.stopPropagation()"></div>
          `).join('')}
        </div>
        <button class="carousel-control prev" onclick="changeCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
        <button class="carousel-control next" onclick="changeCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
      </div>
    ` : `
      <div class="image-carousel">
        <img src="${item.details.image}" alt="${item.details.name}" class="carousel-item active">
      </div>
    `;
    
    // Generate tags HTML if they exist
    const tagsHTML = item.details.tags && item.details.tags.length > 0 
      ? `<div class="tag-container">
          ${item.details.tags.map(tag => `<span class="tag">${tag}</span>`).join('')}
         </div>`
      : '';
    
    // Generate combined info grid similar to locator card
    const infoGridHTML = `
      <div class="real-estate-info-grid">
        <div class="info-item">
          <div class="info-icon">
            <i class="fas fa-map-marker-alt"></i>
          </div>
          <div class="info-content">
            <span class="info-label">Location & Details</span>
            <span class="info-value">${item.details.address || item.metropolis || 'N/A'}</span>
            <span class="info-value">${item.subcategory || item.category} • ${item.details.area || 'N/A'}</span>
            <span class="info-value">${item.details.price || 'Contact for price'}</span>
          </div>
        </div>
      </div>
    `;
    
    // Generate action buttons
    const actionButtonsHTML = `
      <button class="action-button" onclick="openRealEstatePage('${item.details.name}'); event.stopPropagation()">
        <i class="fas fa-info-circle"></i> Details
      </button>
      <button class="action-button" onclick="event.stopPropagation(); contactAgent('${item.details.contact?.phone || ''}')">
        <i class="fas fa-phone"></i> Contact
      </button>
    `;
    
    return `
      <div class="real-estate-card" onclick="openRealEstatePage('${item.details.name}')">
        ${carouselHTML}
        
        <div class="real-estate-card-content">
          <div class="real-estate-card-header">
            <div style="display: flex; justify-content: space-between; align-items: flex-start;">
              <h3 class="real-estate-name">${item.details.name}</h3>
              ${item.details.rating ? `
                <div class="real-estate-rating">
                  <span class="star">${item.details.rating} ★</span>
                </div>
              ` : ''}
            </div>
            
            <div class="real-estate-meta">
              <span>${item.subcategory || item.category}</span>
              ${item.metropolis ? `<span>• ${item.metropolis}</span>` : ''}
            </div>
            
            ${infoGridHTML}
            
            ${tagsHTML}
          </div>
          
          <p class="real-estate-description">${item.details.description}</p>
        </div>

        <div class="real-estate-card-actions">
          ${actionButtonsHTML}
        </div>
      </div>
    `;
  }).join('');
}

function openRealEstatePage(name) {
  const item = realEstateData.find(i => i.details.name === name);
  if (!item) return;

  const realEstatePage = document.createElement('div');
  realEstatePage.className = 'real-land-page';
  realEstatePage.innerHTML = renderRealLandPage(item);
  document.body.appendChild(realEstatePage);
  addRealLandPageStyles();
  scrollToTop();
}

function renderRealLandPage(property) {
  const headerHTML = `
    <div class="real-land-header">
      <button class="back-button" onclick="closeRealLandPage()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>${property.details.name}</h2>
      <div class="header-actions">
        <button class="share-button" onclick="shareProperty('${property.details.name}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
  `;

  const featuresHTML = property.details.features?.map(feature => `
    <div class="feature-item">
      <div class="feature-icon">
        <i class="fas fa-${feature.icon || 'check'}"></i>
      </div>
      <div class="feature-content">
        <div class="feature-name">${feature.name}</div>
        <div class="feature-value">${feature.value}</div>
      </div>
    </div>
  `).join('') || '<p>No features information available</p>';
  
  const specsHTML = property.details.specifications?.map(spec => `
    <div class="spec-item">
      <div class="spec-name">${spec.name}</div>
      <div class="spec-value">${spec.value}</div>
    </div>
  `).join('') || '<p>No specifications available</p>';
  
  const contactHTML = property.details.contact ? `
    <div class="contact-agent">
      <div class="agent-info">
        <div class="agent-name">${property.details.contact.agent || 'Land Specialist'}</div>
        ${property.details.contact.phone ? `
          <a href="tel:${property.details.contact.phone}" class="agent-phone">
            <i class="fas fa-phone"></i> ${property.details.contact.phone}
          </a>
        ` : ''}
        ${property.details.contact.email ? `
          <a href="mailto:${property.details.contact.email}" class="agent-email">
            <i class="fas fa-envelope"></i> ${property.details.contact.email}
          </a>
        ` : ''}
      </div>
      <button class="contact-button" onclick="contactAgent('${property.details.contact.phone || ''}')">
        <i class="fas fa-phone"></i> Contact Agent
      </button>
    </div>
  ` : '';
  
  const imageCarouselHTML = property.details.storeImages && property.details.storeImages.length > 0 ? `
    <div class="real-land-image-carousel">
      ${property.details.storeImages.map((image, index) => `
        <div class="real-land-carousel-item ${index === 0 ? 'active' : ''}">
          <img src="${image}" alt="${property.details.name}" onclick="openFullScreen('${image}')">
        </div>
      `).join('')}
      <div class="real-land-carousel-pagination">
        ${property.details.storeImages.map((_, index) => `
          <div class="real-land-pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentRealLandSlide(${index + 1}); event.stopPropagation()"></div>
        `).join('')}
      </div>
      <button class="real-land-carousel-control prev" onclick="changeRealLandSlide(event, -1); event.stopPropagation()">&#10094;</button>
      <button class="real-land-carousel-control next" onclick="changeRealLandSlide(event, 1); event.stopPropagation()">&#10095;</button>
    </div>
  ` : `
    <div class="real-land-image-container">
      <img src="${property.details.image}" alt="${property.details.name}" class="real-land-image">
    </div>
  `;

  return `
    ${headerHTML}
    
    <div class="real-land-content">
      <!-- Hero Section -->
      <div class="real-land-hero">
        ${imageCarouselHTML}
        <div class="real-land-basic-info">
          <div class="real-land-rating">
            <i class="fas fa-star"></i> ${property.details.rating || '4.0'} (${property.details.reviews || '100+'})
          </div>
          <div class="real-land-type">
            <i class="fas fa-map-marked-alt"></i> ${property.subcategory || property.category}
          </div>
          <div class="real-land-location">
            <i class="fas fa-location-dot"></i> ${property.metropolis || 'N/A'}
          </div>
          <div class="real-land-price">
            <i class="fas fa-tag"></i> ${property.details.price || 'Contact for price'}
          </div>
        </div>
      </div>
      
      <!-- Description Section -->
      <div class="real-land-description-section">
        <h3>Land Details</h3>
        <p>${property.details.description}</p>
      </div>
      
      <!-- Features Section -->
      <div class="real-land-features-section">
        <h3>Key Features</h3>
        <div class="features-grid">
          ${featuresHTML}
        </div>
      </div>
      
      <!-- Specifications Section -->
      <div class="real-land-specs-section">
        <h3>Specifications</h3>
        <div class="specs-grid">
          ${specsHTML}
        </div>
      </div>
      
      <!-- Location Section -->
      <div class="real-land-location-section">
        <h3>Location Details</h3>
        <div class="location-info">
          <div class="address">
            <i class="fas fa-map-marker-alt"></i>
            <span>${property.details.address}</span>
          </div>
          <a href="${property.details.mapLink}" target="_blank" class="map-button">
            <i class="fas fa-map-marked-alt"></i> View on Map
          </a>
        </div>
      </div>
      
      <!-- Contact Section -->
      ${contactHTML}
      
      <!-- Action Buttons -->
      <div class="real-land-actions">
        <button class="primary-action" onclick="contactAgent('${property.details.contact?.phone || ''}')">
          <i class="fas fa-phone"></i> Contact Agent
        </button>
        <a href="${property.details.mapLink}" target="_blank" class="secondary-action">
          <i class="fas fa-directions"></i> Get Directions
        </a>
      </div>
    </div>
  `;
}

function addRealLandPageStyles() {
  const style = document.createElement('style');
  style.textContent = `
    /* Real Land Page Container */
    .real-land-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      color: #fff;
      font-family: 'Poppins', sans-serif;
    }

    /* Real Land Header */
    .real-land-header {
      position: sticky;
      top: 0;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 16px 20px;
      background: rgba(26, 26, 31, 0.95);
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
      z-index: 10;
      backdrop-filter: blur(12px);
    }

    .back-button {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.5em;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background 0.2s ease;
    }

    .back-button:hover {
      background: rgba(255, 255, 255, 0.1);
    }

    .real-land-header h2 {
      margin: 0;
      font-size: 16px;
      font-weight: 600;
      text-align: center;
      flex: 1;
      padding: 0 16px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .header-actions {
      display: flex;
      gap: 8px;
    }

    .share-button {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.2em;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background 0.2s ease;
    }

    .share-button:hover {
      background: rgba(255, 255, 255, 0.1);
    }

    /* Real Land Content */
    .real-land-content {
      padding: 0 0 100px;
      width: 100%;
      margin: 0 auto;
    }

    /* Hero Section */
    .real-land-hero {
      position: relative;
      padding: 20px;
    }

    .real-land-image-carousel {
      position: relative;
      width: 100%;
      height: 250px;
      border-radius: 16px;
      overflow: hidden;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
    }

    .real-land-carousel-item {
      position: absolute;
      width: 100%;
      height: 100%;
      opacity: 0;
      transition: opacity 0.5s ease, transform 0.5s ease;
      transform: scale(1.03);
      will-change: opacity, transform;
    }

    .real-land-carousel-item.active {
      opacity: 1;
      transform: scale(1);
    }

    .real-land-carousel-item img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform 0.4s ease;
    }

    .real-land-image-carousel:hover .real-land-carousel-item.active img {
      transform: scale(1.05);
    }

    .real-land-carousel-control {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(0, 0, 0, 0.6);
      color: white;
      border: none;
      width: 36px;
      height: 36px;
      border-radius: 50%;
      cursor: pointer;
      font-size: 16px;
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      transition: all 0.3s ease;
      z-index: 5;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
      -webkit-tap-highlight-color: transparent;
    }

    .real-land-image-carousel:hover .real-land-carousel-control {
      opacity: 0.9;
    }

    .real-land-carousel-control:hover {
      background: rgba(0, 0, 0, 0.85);
      transform: translateY(-50%) scale(1.1);
    }

    .real-land-carousel-control:active {
      transform: translateY(-50%) scale(0.95);
    }

    .real-land-carousel-control.prev {
      left: 12px;
    }

    .real-land-carousel-control.next {
      right: 12px;
    }

    .real-land-carousel-pagination {
      position: absolute;
      bottom: 12px;
      left: 0;
      right: 0;
      display: flex;
      justify-content: center;
      gap: 8px;
      z-index: 4;
    }

    .real-land-pagination-dot {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: rgba(255, 255, 255, 0.5);
      cursor: pointer;
      transition: all 0.25s ease;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
      border: 1px solid rgba(255, 255, 255, 0.2);
    }

    .real-land-pagination-dot.active {
      background: #fff;
      width: 20px;
      border-radius: 4px;
      box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
    }

    .real-land-image-container {
      width: 100%;
      height: 250px;
      border-radius: 16px;
      overflow: hidden;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
    }

    .real-land-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform 0.3s ease;
    }

    .real-land-image:hover {
      transform: scale(1.02);
    }

    .real-land-basic-info {
      display: flex;
      gap: 12px;
      margin-top: 16px;
      flex-wrap: wrap;
    }

    .real-land-rating,
    .real-land-type,
    .real-land-location,
    .real-land-price {
      display: flex;
      align-items: center;
      gap: 6px;
      padding: 8px 12px;
      border-radius: 20px;
      font-size: 0.9em;
      font-weight: 500;
    }

    .real-land-rating {
      background: rgba(255, 215, 0, 0.1);
      color: #ffd700;
    }

    .real-land-type {
      background: rgba(0, 191, 255, 0.1);
      color: #00bfff;
    }

    .real-land-location {
      background: rgba(255, 255, 255, 0.1);
      color: #fff;
    }

    .real-land-price {
      background: rgba(76, 175, 80, 0.1);
      color: #4CAF50;
    }

    /* Description Section */
    .real-land-description-section {
      padding: 0 20px 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .real-land-description-section h3 {
      margin: 0 0 12px;
      font-size: 1.2em;
      font-weight: 600;
    }

    .real-land-description-section p {
      margin: 0;
      line-height: 1.6;
      color: rgba(255, 255, 255, 0.8);
    }

    /* Features Section */
    .real-land-features-section {
      padding: 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .real-land-features-section h3 {
      margin: 0 0 16px;
      font-size: 1.2em;
      font-weight: 600;
    }

    .features-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
    }

    .feature-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 8px;
      padding: 12px;
      display: flex;
      gap: 10px;
      align-items: center;
    }

    .feature-icon {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      background: rgba(255, 215, 0, 0.1);
      display: flex;
      align-items: center;
      justify-content: center;
      color: #ffd700;
      flex-shrink: 0;
    }

    .feature-content {
      display: flex;
      flex-direction: column;
    }

    .feature-name {
      font-size: 0.9em;
      color: rgba(255, 255, 255, 0.7);
    }

    .feature-value {
      font-weight: 500;
      font-size: 0.95em;
    }

    /* Specifications Section */
    .real-land-specs-section {
      padding: 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .real-land-specs-section h3 {
      margin: 0 0 16px;
      font-size: 1.2em;
      font-weight: 600;
    }

    .specs-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
    }

    .spec-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 8px;
      padding: 12px;
    }

    .spec-name {
      color: rgba(255, 255, 255, 0.7);
      font-size: 0.9em;
      margin-bottom: 4px;
    }

    .spec-value {
      font-weight: 500;
    }

    /* Location Section */
    .real-land-location-section {
      padding: 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .real-land-location-section h3 {
      margin: 0 0 16px;
      font-size: 1.2em;
      font-weight: 600;
    }

    .location-info {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .address {
      display: flex;
      align-items: flex-start;
      gap: 8px;
    }

    .address i {
      margin-top: 2px;
    }

    .map-button {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      padding: 12px;
      background: rgba(255, 215, 0, 0.1);
      color: #ffd700;
      border-radius: 8px;
      text-decoration: none;
      font-weight: 500;
    }

    /* Contact Agent Section */
    .contact-agent {
      margin: 20px;
      padding: 16px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 12px;
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    .agent-info {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .agent-name {
      font-weight: 600;
    }

    .agent-phone,
    .agent-email {
      display: flex;
      align-items: center;
      gap: 8px;
      color: #fff;
      text-decoration: none;
    }

    .contact-button {
      padding: 12px;
      background: var(--primary-color);
      color: #000;
      border: none;
      border-radius: 8px;
      font-weight: 600;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      cursor: pointer;
      transition: transform 0.3s ease;
    }

    .contact-button:hover {
      transform: translateY(-2px);
    }

.real-land-actions {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  z-index: 10;
  backdrop-filter: blur(12px);
}

.primary-action,
.secondary-action {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 14px;
  border-radius: 8px;
  font-weight: 500;
  font-size: 0.95em;
  cursor: pointer;
  transition: transform 0.3s ease, background 0.3s ease;
}

.primary-action {
  background: var(--primary-color, #ffd700);
  color: #000;
  border: none;
}

.primary-action:hover {
  background: #ffd700;
  transform: translateY(-2px);
}

.secondary-action {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  border: none;
}

.secondary-action:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

    /* Tablet Styles */
    @media (min-width: 768px) {
      .real-land-header {
        padding: 20px 30px;
      }
      
      .real-land-header h2 {
        font-size: 1.4em;
      }
      
      .real-land-content {
        max-width: 90%;
        margin: 0 auto;
      }
      
      .real-land-hero {
        padding: 30px;
      }
      
      .real-land-image-carousel,
      .real-land-image-container {
        height: 350px;
        border-radius: 20px;
      }
      
      .real-land-basic-info {
        gap: 16px;
        margin-top: 24px;
      }
      
      .real-land-rating,
      .real-land-type,
      .real-land-location,
      .real-land-price {
        padding: 10px 16px;
        font-size: 1em;
      }
      
      .real-land-description-section,
      .real-land-features-section,
      .real-land-specs-section,
      .real-land-location-section {
        padding: 0 30px 30px;
      }
      
      .real-land-description-section h3,
      .real-land-features-section h3,
      .real-land-specs-section h3,
      .real-land-location-section h3 {
        font-size: 1.3em;
        margin-bottom: 16px;
      }
      
      .real-land-description-section p {
        font-size: 1em;
        line-height: 1.6;
      }
      
      .features-grid,
      .specs-grid {
        grid-template-columns: repeat(3, 1fr);
        gap: 16px;
      }
      
      .real-land-actions {
        padding: 20px 30px;
        max-width: 90%;
        margin: 0 auto;
        left: 50%;
        transform: translateX(-50%);
        border-radius: 12px 12px 0 0;
      }
      
      .primary-action,
      .secondary-action {
        padding: 14px;
        font-size: 1em;
      }
    }

    /* Desktop Styles */
    @media (min-width: 1024px) {
      .real-land-content {
        max-width: 900px;
        margin: 0 auto;
        padding: 0 0 120px;
      }
      
      .real-land-header {
        padding: 22px 32px;
      }
      
      .real-land-header h2 {
        font-size: 16px;
      }
      
      .real-land-hero {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 30px;
        padding: 30px;
        align-items: center;
      }
      
      .real-land-image-carousel,
      .real-land-image-container {
        height: 400px;
        border-radius: 20px;
      }
      
      .real-land-basic-info {
        margin-top: 0;
      }
      
      .real-land-description-section {
        padding: 0 30px 30px;
        max-width: 85%;
        margin: 0 auto;
      }
      
      .real-land-description-section h3 {
        font-size: 1.5em;
      }
      
      .real-land-description-section p {
        font-size: 1.05em;
        line-height: 1.7;
      }
      
      .real-land-features-section,
      .real-land-specs-section,
      .real-land-location-section {
        padding: 30px;
      }
      
      .real-land-features-section h3,
      .real-land-specs-section h3,
      .real-land-location-section h3 {
        font-size: 1.5em;
      }
      
      .features-grid {
        grid-template-columns: repeat(4, 1fr);
      }
      
      .specs-grid {
        grid-template-columns: repeat(4, 1fr);
      }
      
  .real-land-actions {
    max-width: 700px;
    left: 50%;
    transform: translateX(-50%);
  }
  
  .primary-action,
  .secondary-action {
    padding: 16px;
    font-size: 1.05em;
  }
 }

    /* Large Desktop Styles */
    @media (min-width: 1440px) {
      .real-land-content {
        max-width: 1000px;
      }
      
      .real-land-hero {
        grid-template-columns: 3fr 2fr;
      }
      
      .real-land-image-carousel,
      .real-land-image-container {
        height: 450px;
      }
      
      .real-land-description-section {
        max-width: 80%;
      }
      
      .features-grid {
        grid-template-columns: repeat(5, 1fr);
      }
      
      .specs-grid {
        grid-template-columns: repeat(5, 1fr);
      }
      
      .real-land-actions {
        max-width: 800px;
        left: 50%;
      }
    }
  `;
  document.head.appendChild(style);
}

function closeRealLandPage() {
  const page = document.querySelector('.real-land-page');
  if (page) {
    page.style.opacity = '0';
    page.style.transform = 'translateY(20px)';
    setTimeout(() => {
      document.body.removeChild(page);
    }, 300);
  }
}

function shareProperty(propertyName) {
  if (navigator.share) {
    navigator.share({
      title: propertyName,
      text: 'Check out this property I found',
      url: window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    const shareUrl = window.location.href;
    alert(`Share this property: ${propertyName}\n${shareUrl}`);
  }
}

function filterRealEstate(category) {
  const grid = document.getElementById('realEstateGrid');
  let filteredData = realEstateData;

  if (category !== 'All') {
    filteredData = filteredData.filter(item => item.category === category);
  }

  grid.innerHTML = renderRealEstateCards(filteredData);

  const filterButtons = document.querySelectorAll('.category-filter-buttons .filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function sortRealEstate(criteria) {
  const grid = document.getElementById('realEstateGrid');
  let sortedData = [...realEstateData];

  switch(criteria) {
    case 'rating':
      sortedData.sort((a, b) => {
        const ratingA = parseFloat(a.details.rating) || 0;
        const ratingB = parseFloat(b.details.rating) || 0;
        return ratingB - ratingA;
      });
      break;
    case 'price':
      sortedData.sort((a, b) => {
        const priceA = extractPriceValue(a.details.price) || 0;
        const priceB = extractPriceValue(b.details.price) || 0;
        return priceB - priceA;
      });
      break;
    case 'name':
      sortedData.sort((a, b) => a.details.name.localeCompare(b.details.name));
      break;
    case 'distance':
      sortedData.sort(() => Math.random() - 0.5);
      break;
  }

  grid.innerHTML = renderRealEstateCards(sortedData);
  toggleSortOptions();
}

function extractPriceValue(priceStr) {
  if (!priceStr) return 0;
  const match = priceStr.match(/\d[\d,.]*/);
  if (match) {
    return parseFloat(match[0].replace(/,/g, ''));
  }
  return 0;
}

function toggleSortOptions() {
  const container = document.getElementById('sortOptionsContainer');
  container.classList.toggle('active');
}

function hideRealEstateOverlay() {
  const overlay = document.getElementById('realEstateOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
    }, 300);
  }
}

function shareRealEstate(name) {
  const item = realEstateData.find(i => i.details.name === name);
  if (!item) return;

  if (navigator.share) {
    navigator.share({
      title: item.details.name,
      text: `Check out ${item.details.name} - ${item.details.description}`,
      url: item.details.mapLink || window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    const shareUrl = item.details.mapLink || window.location.href;
    const shareText = `Check out ${item.details.name}: ${shareUrl}`;
    prompt('Copy this link to share:', shareText);
  }
}

function contactAgent(phoneNumber) {
  event.stopPropagation();
  if (phoneNumber) {
    window.open(`tel:${phoneNumber}`);
  } else {
    alert('Contact information would appear here in a real implementation.');
  }
}

// Carousel functions for real land page
function changeRealLandSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.real-land-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.real-land-carousel-item');
  const dots = carousel.querySelectorAll('.real-land-pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentRealLandSlide(index) {
  const carousel = event.target.closest('.real-land-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.real-land-carousel-item');
  const dots = carousel.querySelectorAll('.real-land-pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

// Helper functions
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    const context = this;
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(context, args), wait);
  };
}

function performSearch(data, query, container, filterButtons, renderFunction) {
  if (!query.trim()) {
    container.innerHTML = renderFunction(data);
    return;
  }

  const searchTerm = query.toLowerCase();
  const filteredData = data.filter(item => 
    item.details.name.toLowerCase().includes(searchTerm) ||
    (item.category && item.category.toLowerCase().includes(searchTerm)) ||
    (item.metropolis && item.metropolis.toLowerCase().includes(searchTerm)) ||
    (item.details.description && item.details.description.toLowerCase().includes(searchTerm))
  );

  container.innerHTML = renderFunction(filteredData);
}

function initializeOverlayVoiceSearch(inputElement, callback) {
  const voiceSearchButton = document.getElementById('voiceSearchButton');
  if (!voiceSearchButton) return;

  voiceSearchButton.addEventListener('click', () => {
    if (!('webkitSpeechRecognition' in window)) {
      alert('Voice search is not supported in your browser');
      return;
    }

    const recognition = new webkitSpeechRecognition();
    recognition.continuous = false;
    recognition.interimResults = false;

    recognition.onstart = () => {
      voiceSearchButton.classList.add('active');
    };

    recognition.onresult = (event) => {
      const transcript = event.results[0][0].transcript;
      inputElement.value = transcript;
      callback(transcript);
    };

    recognition.onerror = (event) => {
      console.error('Voice recognition error', event.error);
      voiceSearchButton.classList.remove('active');
    };

    recognition.onend = () => {
      voiceSearchButton.classList.remove('active');
    };

    recognition.start();
  });
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

// Carousel functions
function changeCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentSlide(index) {
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

function openFullScreen(imageUrl) {
  event.stopPropagation();
  const fullScreenDiv = document.createElement('div');
  fullScreenDiv.className = 'full-screen-image';
  fullScreenDiv.innerHTML = `
    <img src="${imageUrl}" alt="Full screen">
    <button class="close-full-screen" onclick="document.querySelector('.full-screen-image').remove()">
      <i class="fas fa-times"></i>
    </button>
  `;
  document.body.appendChild(fullScreenDiv);
  
  setTimeout(() => {
    fullScreenDiv.classList.add('active');
    const img = fullScreenDiv.querySelector('img');
    if (img) {
      img.style.opacity = '1';
      img.style.transform = 'scale(1)';
    }
  }, 10);
}

// Modified hide functions for each overlay
function hideLocatorsOverlay() {
  const overlay = document.getElementById('locatorsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}

function hideProvidersOverlay() {
  const overlay = document.getElementById('providersOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}

function hidePlacesOverlay() {
  const overlay = document.getElementById('placesOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}


// First, add the voice search functionality
function initializeVoiceSearch(detailName) {
  if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    const recognition = new SpeechRecognition();
    
    recognition.continuous = false;
    recognition.interimResults = false;
    recognition.lang = 'en-US';

    recognition.onresult = (event) => {
      const voiceQuery = event.results[0][0].transcript;
      const searchInput = document.getElementById('chatSearchInput');
      searchInput.value = voiceQuery;
      searchFAQs(voiceQuery, detailName);
    };

    recognition.onerror = (event) => {
      console.error('Speech recognition error:', event.error);
      showVoiceSearchError(event.error);
    };

    return recognition;
  }
  return null;
}

function showVoiceSearchError(error) {
  const searchResults = document.getElementById('searchResults');
  searchResults.innerHTML = `
    <div class="voice-search-error">
      <i class="fas fa-microphone-slash"></i>
      <h3>Voice Search Error</h3>
      <p>${getVoiceErrorMessage(error)}</p>
    </div>
  `;
}

function getVoiceErrorMessage(error) {
  switch (error) {
    case 'not-allowed':
      return 'Microphone access was denied. Please enable microphone access to use voice search.';
    case 'no-speech':
      return 'No speech was detected. Please try again.';
    case 'network':
      return 'Network error occurred. Please check your connection and try again.';
    default:
      return 'An error occurred with voice search. Please try again.';
  }
}

function showChatSupport(detailName) {
  const chatPage = document.createElement('div');
  chatPage.className = 'chat-support-page';
  chatPage.innerHTML = `
    <div class="chat-support-header">
      <button class="back-button" onclick="hideChatSupport()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>FAQ Chat Support - ${detailName}</h2>
    </div>
    <div class="chat-support-content">
      <div class="faq-section">
        <h3>Frequently Asked Questions</h3>
        <div class="faq-list" id="faqList">
          ${renderFAQsByDetail(detailName)}
        </div>
      </div>
      <div class="contact-support" id="contactSupport">
        <div class="contact-support-header">
          <h3>Need More Help?</h3>
          <button class="refresh-button" onclick="refreshChatSupport('${detailName}')">
            <i class="fas fa-sync-alt"></i>
          </button>
        </div>
        <a href="mailto:support@example.com" class="contact-button">
          <i class="fas fa-envelope"></i> Email Support
        </a>
        <a href="tel:+1234567890" class="contact-button">
          <i class="fas fa-phone"></i> Call Support
        </a>
      </div>
      <div class="search-results" id="searchResults"></div>
    </div>
    <div class="chat-search-container">
      <div class="input-container">
        <input type="text" id="chatSearchInput" placeholder="Search for answers..." />
        <button class="voice-search" id="voiceSearchBtn">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  document.body.appendChild(chatPage);
  addChatSupportStyles();
  initializeFAQToggle();
  initializeChatSearch(detailName);
  initializeVoiceSearchButton(detailName);
}

function initializeVoiceSearchButton(detailName) {
  const voiceSearchBtn = document.getElementById('voiceSearchBtn');
  const recognition = initializeVoiceSearch(detailName);
  
  if (recognition) {
    let isListening = false;

    voiceSearchBtn.addEventListener('click', () => {
      if (!isListening) {
        recognition.start();
        voiceSearchBtn.classList.add('listening');
        voiceSearchBtn.querySelector('i').classList.remove('fa-microphone');
        voiceSearchBtn.querySelector('i').classList.add('fa-microphone-slash');
      } else {
        recognition.stop();
        voiceSearchBtn.classList.remove('listening');
        voiceSearchBtn.querySelector('i').classList.remove('fa-microphone-slash');
        voiceSearchBtn.querySelector('i').classList.add('fa-microphone');
      }
      isListening = !isListening;
    });

    recognition.onend = () => {
      voiceSearchBtn.classList.remove('listening');
      voiceSearchBtn.querySelector('i').classList.remove('fa-microphone-slash');
      voiceSearchBtn.querySelector('i').classList.add('fa-microphone');
      isListening = false;
    };
  } else {
    voiceSearchBtn.style.display = 'none';
  }
}

function renderFAQsByDetail(detailName) {
  const faqs = getFAQsByDetail(detailName);
  return faqs.map(faq => `
    <div class="faq-item" onclick="toggleFAQAnswer('${faq.id}')">
      <div class="faq-question">
        <span>${faq.question}</span>
        <i class="fas fa-chevron-down"></i>
      </div>
      <div class="faq-answer" id="faqAnswer${faq.id}">
        <p>${faq.answer}</p>
      </div>
    </div>
  `).join('');
}

function getFAQsByDetail(detailName) {
  // Example FAQs, you can replace this with your own data
  const faqs = {
    "Dmart": [
      { id: 1, question: "What are the store timings for Dmart?", answer: "Dmart stores are open from 9 AM to 10 PM, Monday to Sunday." },
      { id: 2, question: "Does Dmart offer home delivery?", answer: "Yes, Dmart offers home delivery through their app and website." }
    ],
    "Amazon": [
      { id: 3, question: "How do I track my Amazon order?", answer: "You can track your order by logging into your Amazon account and visiting the 'Your Orders' section." },
      { id: 4, question: "What is Amazon Prime?", answer: "Amazon Prime is a subscription service that offers benefits like free delivery, access to streaming services, and more." }
    ],
    "Delhi Plumbing Services": [
      { id: 5, question: "What services do you offer?", answer: "We offer a wide range of plumbing services including pipe repair, leak fixing, and more." },
      { id: 6, question: "Are your plumbers certified?", answer: "Yes, all our plumbers are certified and experienced professionals." }
    ],
    "Lodhi Garden": [
      { id: 7, question: "What are the entry fees for Lodhi Garden?", answer: "Entry to Lodhi Garden is free for all visitors." },
      { id: 8, question: "Are pets allowed in Lodhi Garden?", answer: "Yes, pets are allowed in Lodhi Garden but must be kept on a leash." }
    ]
  };

  return faqs[detailName] || [];
}

function toggleFAQ(index) {
  const answer = document.getElementById(`faq-answer-${index}`);
  const question = answer.previousElementSibling;
  const chevron = question.querySelector('i');

  if (answer.classList.contains('active')) {
    // Collapse the answer
    answer.style.maxHeight = `${answer.scrollHeight}px`;
    setTimeout(() => {
      answer.style.maxHeight = '0';
    }, 10);
  } else {
    // Expand the answer
    answer.style.maxHeight = `${answer.scrollHeight}px`;
  }

  // Toggle the active class after a slight delay
  setTimeout(() => {
    answer.classList.toggle('active');
    question.classList.toggle('active');
    chevron.classList.toggle('fa-chevron-down');
    chevron.classList.toggle('fa-chevron-up');
  }, 10);
}

// Function to initialize FAQ toggle functionality
function initializeFAQToggle() {
  const faqQuestions = document.querySelectorAll('.faq-question');
  faqQuestions.forEach(question => {
    question.addEventListener('click', () => {
      const answer = question.nextElementSibling;
      answer.classList.toggle('active');
      question.classList.toggle('active');
    });
  });
}

// Function to initialize chat search functionality
function initializeChatSearch() {
  const chatSearchInput = document.getElementById('chatSearchInput');
  if (chatSearchInput) {
    chatSearchInput.addEventListener('input', (e) => {
      const query = e.target.value.trim();
      if (query) {
        searchFAQs(query);
      } else {
        clearSearchResults();
      }
    });
  }
}

// Function to search FAQs
function searchFAQs(query) {
  const faqList = document.getElementById('faqList');
  const searchResults = document.getElementById('searchResults');
  if (faqList && searchResults) {
    const faqItems = faqList.querySelectorAll('.faq-item');
    let resultsFound = false;

    faqItems.forEach(item => {
      const question = item.querySelector('.faq-question span').textContent.toLowerCase();
      const answer = item.querySelector('.faq-answer p').textContent.toLowerCase();
      if (question.includes(query.toLowerCase()) || answer.includes(query.toLowerCase())) {
        const resultItem = document.createElement('div');
        resultItem.className = 'search-result-item';
        resultItem.innerHTML = `
          <h4>${item.querySelector('.faq-question span').textContent}</h4>
          <p>${item.querySelector('.faq-answer p').textContent}</p>
        `;
        searchResults.appendChild(resultItem);
        resultsFound = true;
      }
    });

    if (!resultsFound) {
      searchResults.innerHTML = `<p>No results found for "${query}".</p>`;
    }
  }
}

// Function to clear search results
function clearSearchResults() {
  const searchResults = document.getElementById('searchResults');
  if (searchResults) {
    searchResults.innerHTML = '';
  }
}

// Function to hide chat support
function hideChatSupport() {
  const chatPage = document.querySelector('.chat-support-page');
  if (chatPage) {
    chatPage.remove();
  }
}

function refreshChatSupport(detailName) {
  const faqList = document.getElementById('faqList');
  if (faqList) {
    faqList.innerHTML = renderFAQsByDetail(detailName);
  }
}

// Function to add chat support styles
function addChatSupportStyles() {
  const style = document.createElement('style');
  style.textContent = `
    .chat-support-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      animation: slideIn 0.3s ease-out;
    }

    @keyframes slideIn {
      from { transform: translateX(100%); }
      to { transform: translateX(0); }
    }

    .chat-support-header {
      position: sticky;
      top: 0;
      background: rgba(26, 26, 31, 0.95);
      font-size: 11px;
      padding: 20px;
      display: flex;
      align-items: center;
      gap: 20px;
      border-bottom: 2px solid var(--primary-color);
      backdrop-filter: blur(10px);
      z-index: 10;
    }

    .back-button {
      background: none;
      border: none;
      color: var(--primary-color);
      font-size: 1.2em;
      cursor: pointer;
      padding: 10px;
      border-radius: 50%;
      transition: all 0.3s ease;
    }

    .back-button:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    .chat-support-content {
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }

    .faq-section {
      margin-top: 0px;
      transition: all 0.3s ease;
    }

    .faq-section h3 {
  color: #fff;
  margin-bottom: 14px;
  font-size: 20px;
  padding-left: 0px;
}

    .faq-item {
      background: rgba(255, 215, 0, 0.05);
      border-radius: 10px;
      margin-top: 5px;
      overflow: hidden;
      transition: all 0.3s ease;
    }

    .faq-question {
      padding: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      color: var(--text-light);
      font-weight: 500;
      transition: all 0.3s ease;
    }

    .faq-question:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    .faq-question i {
      transition: transform 0.3s ease;
    }

    .faq-question.active i {
      transform: rotate(180deg);
    }

    .faq-answer {
      padding: 0 20px;
      max-height: 0;
      overflow: hidden;
      transition: all 0.3s ease;
      color: #aaa;
    }

    .faq-answer.active {
      padding: 20px;
      max-height: 500px;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
    }

  
    .contact-support {
      display: none;
      animation: fadeIn 0.3s ease-out;
      padding: 40px 20px;
      background: rgba(255, 215, 0, 0.05);
      border-radius: 15px;
      text-align: center;
      margin: 20px auto;
      max-width: 600px;
    }

    .contact-support-header {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 15px;
      margin-bottom: 20px;
    }

    .refresh-button {
      background: none;
      border: none;
      color: var(--primary-color);
      font-size: 1.2em;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      transition: all 0.3s ease;
    }

    .refresh-button:hover {
      background: rgba(255, 215, 0, 0.1);
      transform: rotate(180deg);
    }

    .contact-button {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 15px 25px;
      background: rgba(255, 215, 0, 0.1);
      border-radius: 10px;
      color: var(--primary-color);
      text-decoration: none;
      transition: all 0.3s ease;
      margin: 10px 0;
    }

    .contact-button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: translateX(5px);
    }

    .chat-search-container {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
      display: flex;
      justify-content: center;
      align-items: center;
      box-sizing: border-box;
    }

    .input-container {
      position: relative;
      width: 100%;
      max-width: 800px;
      display: flex;
      align-items: center;
    }

    #chatSearchInput {
      width: 100%;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 12px 50px 12px 20px;
      color: #fff;
      font-size: 0.95em;
      height: 60px;
      transition: border-color 0.3s ease;
    }

    #chatSearchInput:focus {
      border-color: rgba(255, 255, 255, 0.5);
      outline: none;
    }

    .voice-search {
      position: absolute;
      right: 10px;
      background: none;
      border: none;
      color: var(--primary-color);
      cursor: pointer;
      padding: 10px;
      transition: all 0.3s ease;
    }

    .voice-search:hover {
      opacity: 0.8;
    }

    .voice-search.active {
      color: #ffd700;
      animation: pulse 1.5s infinite;
    }

    .voice-search i {
      font-size: 1.5em;
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }

    .voice-search-error {
    text-align: center;
    padding: 40px 20px;
    color: var(--text-light);
  }

  .voice-search-error i {
    font-size: 3em;
    color: #ff4444;
    margin-bottom: 20px;
  }

  /* Responsive styles for the new button */
  @media (max-width: 768px) {
    .voice-search-btn {
      padding: 20px 20px;
    }
  }


    /* Desktop Specific Styles */
    @media (min-width: 769px) {
      .chat-search-container {
        padding: 24px;
        bottom: 0;
      }

      .input-container {
        max-width: 800px;
        margin: 0 auto;
      }

      #chatSearchInput {
        height: 80px;
        font-size: 1.05rem;
        padding: 0 50px 0 24px;
        border-radius: 16px;
      }

      .voice-search {
        width: 50px;
        height: 50px;
        right: 15px;
      }

      .voice-search i {
        font-size: 2.1em;
      }
    }

    /* Mobile styles */
    @media (max-width: 768px) {
    .chat-search-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }
      #chatSearchInput {
    width: 100%;
    padding: 20px 20px;
  }
}

      .voice-search {
        width: 40px;
        height: 40px;
        right: 5px;
      }

      .voice-search i {
        font-size: 1.3em;
      }
    }

  `;
  document.head.appendChild(style);
}

function searchFAQs(query, detailName) {
  const faqList = document.getElementById('faqList');
  const searchResults = document.getElementById('searchResults');
  const contactSupport = document.getElementById('contactSupport');
  
  if (!query.trim()) {
    clearSearchResults();
    faqList.style.display = 'block';
    contactSupport.style.display = 'none';
    return;
  }

  if (faqList && searchResults) {
    const faqItems = faqList.querySelectorAll('.faq-item');
    const results = [];
    
    faqItems.forEach(item => {
      const question = item.querySelector('.faq-question span').textContent;
      const answer = item.querySelector('.faq-answer p').textContent;
      
      // Calculate relevance score using fuzzy matching
      const questionScore = calculateRelevance(query, question);
      const answerScore = calculateRelevance(query, answer);
      const totalScore = Math.max(questionScore, answerScore);

      if (totalScore > 0.3) { // Threshold for relevance
        results.push({
          question,
          answer,
          score: totalScore,
          element: item.cloneNode(true)
        });
      }
    });

    // Sort results by relevance score
    results.sort((a, b) => b.score - a.score);
    
    // Clear previous results
    searchResults.innerHTML = '';
    faqList.style.display = 'none';
    
    if (results.length > 0) {
      // Add search summary
      const summary = document.createElement('div');
      summary.className = 'search-summary';
      summary.innerHTML = `Found ${results.length} result${results.length === 1 ? '' : 's'} for "${query}"`;
      searchResults.appendChild(summary);

      // Display results with highlighted matches
      results.forEach(result => {
        const resultItem = document.createElement('div');
        resultItem.className = 'search-result-item';
        resultItem.innerHTML = `
          <h4>${highlightMatches(result.question, query)}</h4>
          <p>${highlightMatches(result.answer, query)}</p>
          <div class="relevance-indicator" style="width: ${result.score * 100}%"></div>
        `;
        searchResults.appendChild(resultItem);
      });
    } else {
      // Show "no results" message with suggestions
      searchResults.innerHTML = `
        <div class="no-results">
          <i class="fas fa-search"></i>
          <h3>No results found for "${query}"</h3>
          <p>Try:</p>
          <ul>
            <li>Checking your spelling</li>
            <li>Using more general terms</li>
            <li>Using fewer keywords</li>
          </ul>
          <button onclick="showContactSupport()" class="contact-support-btn">
            Contact Support
          </button>
        </div>
      `;
    }

    // Show/hide contact support based on results
    contactSupport.style.display = results.length === 0 ? 'block' : 'none';
  }
}

// Calculate relevance score using Levenshtein distance and token matching
function calculateRelevance(query, text) {
  const queryTokens = query.toLowerCase().split(/\s+/);
  const textTokens = text.toLowerCase().split(/\s+/);
  
  let totalScore = 0;
  queryTokens.forEach(queryToken => {
    let bestTokenScore = 0;
    textTokens.forEach(textToken => {
      const distance = levenshteinDistance(queryToken, textToken);
      const maxLength = Math.max(queryToken.length, textToken.length);
      const similarity = 1 - (distance / maxLength);
      bestTokenScore = Math.max(bestTokenScore, similarity);
    });
    totalScore += bestTokenScore;
  });
  
  return totalScore / queryTokens.length;
}

// Levenshtein distance calculation for fuzzy matching
function levenshteinDistance(str1, str2) {
  const matrix = Array(str2.length + 1).fill().map(() => Array(str1.length + 1).fill(0));
  
  for (let i = 0; i <= str1.length; i++) matrix[0][i] = i;
  for (let j = 0; j <= str2.length; j++) matrix[j][0] = j;
  
  for (let j = 1; j <= str2.length; j++) {
    for (let i = 1; i <= str1.length; i++) {
      const cost = str1[i - 1] === str2[j - 1] ? 0 : 1;
      matrix[j][i] = Math.min(
        matrix[j - 1][i] + 1,
        matrix[j][i - 1] + 1,
        matrix[j - 1][i - 1] + cost
      );
    }
  }
  
  return matrix[str2.length][str1.length];
}

// Highlight matching text in results
function highlightMatches(text, query) {
  const queryTokens = query.toLowerCase().split(/\s+/);
  let highlightedText = text;
  
  queryTokens.forEach(token => {
    const regex = new RegExp(`(${token})`, 'gi');
    highlightedText = highlightedText.replace(regex, '<mark>$1</mark>');
  });
  
  return highlightedText;
}

// Debounce search input to improve performance
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

function initializeChatSearch(detailName) {
  const chatSearchInput = document.getElementById('chatSearchInput');
  if (chatSearchInput) {
    const debouncedSearch = debounce((e) => {
      const query = e.target.value.trim();
      searchFAQs(query, detailName);
    }, 300);
    
    chatSearchInput.addEventListener('input', debouncedSearch);
    
    // Add keyboard navigation
    chatSearchInput.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') {
        clearSearchResults();
        chatSearchInput.value = '';
      }
    });
  }
}

// Additional styles for enhanced features
const additionalStyles = `
  .search-summary {
    color: var(--text-light);
    padding: 10px 0;
    margin-bottom: 15px;
    font-size: 0.9em;
  }

  .search-result-item {
    position: relative;
    overflow: hidden;
  }

  .search-result-item mark {
    background: rgba(255, 215, 0, 0.2);
    color: var(--primary-color);
    padding: 0 2px;
    border-radius: 2px;
  }

  .relevance-indicator {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 2px;
    background: var(--primary-color);
    opacity: 0.5;
    transition: width 0.3s ease;
  }

  .no-results {
    text-align: center;
    padding: 40px 20px;
    color: var(--text-light);
  }

  .no-results i {
    font-size: 3em;
    color: var(--primary-color);
    margin-bottom: 20px;
  }

  .no-results ul {
    list-style: none;
    padding: 0;
    margin: 20px 0;
  }

  .no-results li {
    margin: 10px 0;
    color: #aaa;
  }

  .contact-support-btn {
    background: var(--primary-color);
    color: #1a1a1f;
    border: none;
    padding: 12px 24px;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.3s ease;
  }

  .contact-support-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(255, 215, 0, 0.2);
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .search-result-item {
    animation: fadeIn 0.3s ease-out forwards;
    animation-delay: calc(var(--index) * 0.05s);
  }
`;

// Helper function to hide current overlay
function hideCurrentOverlay() {
  switch (overlayState.currentOverlay) {
    case 'locators':
      hideLocatorsOverlay();
      break;
    case 'providers':
      hideProvidersOverlay();
      break;
    case 'places':
      hidePlacesOverlay();
      break;
    case 'platforms':
      hidePlatformsOverlay();
      break;
  }
}

// Initialize the system when the DOM loads
document.addEventListener('DOMContentLoaded', initializeOverlaySystem);

const deepChatServices = {
  contacts: locatorsData.map(brand => ({
    id: `${brand.details.name}-${brand.metropolis}`,
    name: brand.details.name,
    avatar: brand.details.image,
    subtitle: `${brand.category} • ${brand.metropolis}`,
    lastMessage: brand.details.description,
    timestamp: this.randomTime(),
    unread: Math.floor(Math.random() * 5),
    online: true,
    verification: 'verified',
    metadata: brand.details
  })),
  recent: [],
  getChatHistory: (contactId) => {
    const brand = locatorsData.find(b => `${b.details.name}-${b.metropolis}` === contactId);
    return [
      {
        type: 'system',
        message: `You're now chatting with ${brand.details.name}`,
        timestamp: new Date().toISOString()
      },
      {
        type: 'brand',
        message: brand.details.description,
        timestamp: brand.details.support.phone
      }
    ];
  }
};

// WhatsApp-like UI Components
function createChatInterface() {
  const chatContainer = document.createElement('div');
  chatContainer.className = 'deep-chat-container';
  
  chatContainer.innerHTML = `
    <div class="chat-list">
      ${deepChatServices.contacts.map(contact => `
        <div class="chat-item" onclick="openChatModal('${contact.id}')">
          <div class="avatar-container">
            <img src="${contact.avatar}" class="avatar" />
            ${contact.online ? '<div class="online-indicator"></div>' : ''}
          </div>
          <div class="chat-info">
            <div class="chat-header">
              <h3>${contact.name}</h3>
              <span class="timestamp">${contact.timestamp}</span>
            </div>
            <div class="chat-preview">
              <p>${contact.lastMessage}</p>
              ${contact.unread > 0 ? 
                `<span class="unread-badge">${contact.unread}</span>` : ''}
            </div>
            <span class="category-tag">${contact.subtitle}</span>
          </div>
        </div>
      `).join('')}
    </div>
  `;

  const style = document.createElement('style');
  style.textContent = `
    .deep-chat-container {
      width: 100%;
      height: 100vh;
      background: #111;
      color: white;
      font-family: 'Helvetica Neue', sans-serif;
    }

    .chat-list {
      padding: 20px;
    }

    .chat-item {
      display: flex;
      align-items: center;
      padding: 15px;
      border-bottom: 1px solid #2d2d2d;
      cursor: pointer;
      transition: background 0.3s;
    }

    .chat-item:hover {
      background: rgba(255,255,255,0.05);
    }

    .avatar-container {
      position: relative;
      margin-right: 15px;
    }

    .avatar {
      width: 55px;
      height: 55px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid #ffd700;
    }

    .online-indicator {
      position: absolute;
      bottom: 2px;
      right: 2px;
      width: 12px;
      height: 12px;
      background: #00af4c;
      border-radius: 50%;
      border: 2px solid #111;
    }

    .chat-info {
      flex: 1;
    }

    .chat-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 5px;
    }

    .chat-header h3 {
      margin: 0;
      font-size: 1.1em;
      font-weight: 500;
    }

    .timestamp {
      font-size: 0.8em;
      color: #888;
    }

    .chat-preview {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .chat-preview p {
      margin: 0;
      font-size: 0.9em;
      color: #888;
      max-width: 70%;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .unread-badge {
      background: #00af4c;
      color: white;
      padding: 3px 8px;
      border-radius: 20px;
      font-size: 0.8em;
    }

    .category-tag {
      display: block;
      font-size: 0.75em;
      color: #ffd700;
      margin-top: 5px;
    }

    /* Chat Modal */
    .chat-modal {
      position: fixed;
      top: 0;
      right: 0;
      bottom: 0;
      width: 400px;
      background: #0c0c0c;
      border-left: 1px solid #2d2d2d;
      transform: translateX(100%);
      transition: transform 0.3s;
    }

    .chat-modal.active {
      transform: translateX(0);
    }

    .chat-header {
      padding: 20px;
      background: #1a1a1a;
      display: flex;
      align-items: center;
    }

    .chat-body {
      padding: 20px;
      height: calc(100vh - 160px);
      overflow-y: auto;
    }

    .message-input {
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 20px;
      background: #1a1a1a;
      display: flex;
      gap: 10px;
    }

    .message-input input {
      flex: 1;
      background: #2d2d2d;
      border: none;
      padding: 12px;
      color: white;
      border-radius: 25px;
    }
  `;

  document.body.appendChild(chatContainer);
  document.head.appendChild(style);
}

function openChatModal(contactId) {
  const brand = deepChatServices.contacts.find(c => c.id === contactId);
  const modal = document.createElement('div');
  modal.className = 'chat-modal active';
  
  modal.innerHTML = `
    <div class="chat-header">
      <img src="${brand.avatar}" class="avatar" />
      <div>
        <h2>${brand.name}</h2>
        <p>${brand.subtitle}</p>
      </div>
    </div>
    
    <div class="chat-body">
      <div class="chat-section">
        <h3>About</h3>
        <p>${brand.metadata.description}</p>
      </div>

      <div class="chat-section">
        <h3>Menu/Services</h3>
        ${brand.metadata.menu?.map(item => `
          <div class="menu-item">
            <img src="${item.image}" />
            <div>
              <h4>${item.name}</h4>
              <p>${item.price}</p>
            </div>
          </div>
        `).join('')}
      </div>

      <div class="chat-section">
        <h3>Promotions</h3>
        ${brand.metadata.promos?.map(promo => `
          <div class="promo-item">
            <p><strong>${promo.code}</strong> - ${promo.description}</p>
          </div>
        `).join('')}
      </div>

      <div class="chat-section">
        <h3>Outlets</h3>
        ${brand.metadata.outlets?.map(outlet => `
          <div class="outlet-item">
            <p>${outlet.name}</p>
            <p>${outlet.address}</p>
            <a href="${outlet.mapLink}" target="_blank">View on Map</a>
          </div>
        `).join('')}
      </div>

      <div class="chat-section">
        <h3>Support</h3>
        <p>Email: ${brand.metadata.support.email}</p>
        <p>Phone: ${brand.metadata.support.phone}</p>
        <button class="support-button">
          <i class="fas fa-comments"></i> Start Chat
        </button>
      </div>
    </div>

    <div class="message-input">
      <input type="text" placeholder="Type your message..." />
      <button class="send-button">
        <i class="fas fa-paper-plane"></i>
      </button>
    </div>
  `;

  document.body.appendChild(modal);
}

// Initialize the chat interface
createChatInterface();

function submitBookingViaWhatsApp() { const name = document.getElementById('nameInput').value.trim(); const contact = document.getElementById('contactInput').value.trim(); const address = document.getElementById('addressInput').value.trim(); const pincode = document.getElementById('pincodeInput').value.trim(); const date = document.getElementById('dateInput').value.trim(); const time = document.getElementById('timeInput').value.trim(); const bookingForm = document.getElementById('bookingForm'); const categoryId = bookingForm.dataset.categoryId || 'Not Specified'; const serviceName = bookingForm.dataset.serviceName || 'Not Specified'; const packageType = bookingForm.dataset.packageType || 'Not Specified'; if (!name || !contact) { showAlert('Please fill in Name and Contact Number', 'error'); return; } const message = `New Booking Request:\n📋 Category: ${categoryId}\n🛠 Service: ${serviceName}\n💎 Package: ${packageType}\n👤 Name: ${name}\n📞 Contact: ${contact}\n🏠 Address: ${address || 'Not Provided'}\n📍 Pincode: ${pincode || 'Not Provided'}\n📅 Date: ${date}\n⏰ Time: ${time}\n\nPlease confirm and process this booking.`; const encodedMessage = encodeURIComponent(message); const whatsappNumber = '78698 09022'; const whatsappUrl = `https://wa.me/${whatsappNumber.replace(/\s/g, '')}?text=${encodedMessage}`; window.open(whatsappUrl, '_blank'); showBookingConfirmation(); } function showBookingConfirmation() { const confirmationModal = document.createElement('div'); confirmationModal.id = 'bookingConfirmation'; confirmationModal.className = 'modal-overlay'; confirmationModal.innerHTML = `<div class="modal-content"><div class="confirmation-icon"><i class="fas fa-check-circle"></i></div><h2>Booking Submitted!</h2><p>Your booking request has been sent to WhatsApp. Our team will contact you shortly.</p><button onclick="closeConfirmation()" class="action-button">Close</button></div>`; document.body.appendChild(confirmationModal); document.getElementById('bookingForm').reset(); } function closeConfirmation() { const confirmationModal = document.getElementById('bookingConfirmation'); if (confirmationModal) { confirmationModal.remove(); } } document.addEventListener('DOMContentLoaded', function() { const submitBookingButton = document.getElementById('submitBooking'); if (submitBookingButton) { submitBookingButton.addEventListener('click', submitBookingViaWhatsApp); } }); document.addEventListener('DOMContentLoaded', function() { const locationNav = document.querySelector('.nav-item[data-page="location"]'); if (locationNav) { locationNav.addEventListener('click', function(e) { e.preventDefault(); showCitiesOverlay(); }); } }); function showAlert(message, type = 'success') { const alert = document.createElement('div'); alert.className = `alert alert-${type}`; alert.textContent = message; document.body.appendChild(alert); setTimeout(() => { alert.remove(); }, 3000); } function setupEventListeners() { window.addEventListener('click', function(event) { if (event.target.classList.contains('modal-overlay')) { closePackageInfo(); closeBookingForm(); closeConfirmation(); } }); }
                            </script>
                            
