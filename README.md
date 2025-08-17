<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Project Chimera: Interactive AI Ethics Adventure</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            color: #e0e0ff;
            min-height: 100vh;
            overflow-x: hidden;
            position: relative;
        }
        
        body::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                radial-gradient(circle at 10% 20%, rgba(106, 50, 145, 0.15) 0%, transparent 20%),
                radial-gradient(circle at 90% 80%, rgba(41, 128, 185, 0.15) 0%, transparent 20%);
            z-index: -1;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px 0;
            border-bottom: 1px solid rgba(106, 176, 255, 0.3);
            margin-bottom: 30px;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .logo-icon {
            width: 50px;
            height: 50px;
            background: linear-gradient(135deg, #6a3291, #2980b9);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            box-shadow: 0 0 15px rgba(106, 50, 145, 0.5);
            animation: pulse 2s infinite;
        }
        
        .logo-text {
            font-size: 28px;
            font-weight: 700;
            background: linear-gradient(to right, #8e44ad, #3498db);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 10px rgba(142, 68, 173, 0.3);
        }
        
        .user-info {
            display: flex;
            align-items: center;
            gap: 15px;
            background: rgba(25, 25, 60, 0.6);
            padding: 10px 20px;
            border-radius: 30px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(106, 176, 255, 0.3);
        }
        
        .xp-badge {
            display: flex;
            align-items: center;
            gap: 5px;
            background: rgba(52, 152, 219, 0.2);
            padding: 5px 15px;
            border-radius: 20px;
            font-weight: 600;
        }
        
        .avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: linear-gradient(135deg, #8e44ad, #3498db);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.3s ease;
        }
        
        .avatar:hover {
            transform: scale(1.1);
        }
        
        .hero {
            text-align: center;
            padding: 40px 20px;
            margin-bottom: 40px;
            background: rgba(25, 25, 60, 0.4);
            border-radius: 20px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(106, 176, 255, 0.2);
            position: relative;
            overflow: hidden;
        }
        
        .hero::before {
            content: "";
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(106, 50, 145, 0.1) 0%, transparent 70%);
            z-index: -1;
        }
        
        h1 {
            font-size: 3.5rem;
            margin-bottom: 20px;
            background: linear-gradient(to right, #ffffff, #8e44ad);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 20px rgba(142, 68, 173, 0.3);
            animation: glow 2s ease-in-out infinite alternate;
        }
        
        .subtitle {
            font-size: 1.4rem;
            max-width: 800px;
            margin: 0 auto 30px;
            color: #a0a0ff;
            line-height: 1.6;
        }
        
        .cta-button {
            background: linear-gradient(135deg, #8e44ad, #3498db);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.2rem;
            font-weight: 600;
            border-radius: 30px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(142, 68, 173, 0.4);
            position: relative;
            overflow: hidden;
        }
        
        .cta-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(142, 68, 173, 0.6);
        }
        
        .cta-button:active {
            transform: translateY(1px);
        }
        
        .cta-button::after {
            content: "";
            position: absolute;
            top: -50%;
            left: -60%;
            width: 20px;
            height: 200%;
            background: rgba(255, 255, 255, 0.3);
            transform: rotate(25deg);
            transition: all 0.6s;
        }
        
        .cta-button:hover::after {
            left: 120%;
        }
        
        .game-preview {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-bottom: 50px;
        }
        
        @media (max-width: 900px) {
            .game-preview {
                grid-template-columns: 1fr;
            }
        }
        
        .preview-panel {
            background: rgba(25, 25, 60, 0.6);
            border-radius: 20px;
            padding: 30px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(106, 176, 255, 0.2);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            transition: transform 0.3s ease;
        }
        
        .preview-panel:hover {
            transform: translateY(-5px);
        }
        
        .panel-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 25px;
        }
        
        .panel-icon {
            width: 50px;
            height: 50px;
            background: linear-gradient(135deg, #6a3291, #2980b9);
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
        }
        
        h2 {
            font-size: 2rem;
            background: linear-gradient(to right, #8e44ad, #3498db);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .panel-content {
            margin-top: 20px;
        }
        
        .data-spheres {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        
        .sphere-card {
            background: rgba(40, 40, 80, 0.7);
            border-radius: 15px;
            padding: 20px;
            transition: all 0.3s ease;
            border: 1px solid rgba(106, 176, 255, 0.1);
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }
        
        .sphere-card:hover {
            transform: translateY(-5px);
            border-color: rgba(106, 176, 255, 0.5);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
        }
        
        .sphere-card::before {
            content: "";
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
            transition: 0.5s;
        }
        
        .sphere-card:hover::before {
            left: 100%;
        }
        
        .sphere-icon {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: linear-gradient(135deg, #8e44ad, #3498db);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            margin-bottom: 15px;
            animation: float 3s ease-in-out infinite;
        }
        
        .sphere-title {
            font-size: 1.3rem;
            font-weight: 600;
            margin-bottom: 10px;
            color: #ffffff;
        }
        
        .sphere-concept {
            font-size: 0.9rem;
            color: #a0a0ff;
            margin-bottom: 15px;
        }
        
        .sphere-ethics {
            font-size: 0.85rem;
            color: #ff9a9e;
            font-style: italic;
        }
        
        .challenge-preview {
            background: rgba(30, 30, 70, 0.8);
            border-radius: 15px;
            padding: 25px;
            margin-top: 20px;
            min-height: 300px;
            position: relative;
            overflow: hidden;
        }
        
        .challenge-preview::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #8e44ad, #3498db);
        }
        
        .terminal {
            background: rgba(0, 0, 0, 0.7);
            border-radius: 10px;
            padding: 20px;
            font-family: 'Courier New', monospace;
            height: 250px;
            overflow-y: auto;
            position: relative;
        }
        
        .terminal-line {
            margin-bottom: 10px;
            line-height: 1.4;
            opacity: 0;
            transform: translateY(10px);
            animation: fadeInUp 0.5s forwards;
        }
        
        .prompt {
            color: #4caf50;
            font-weight: bold;
        }
        
        .output {
            color: #e0e0ff;
        }
        
        .command {
            color: #2196f3;
        }
        
        .error {
            color: #f44336;
        }
        
        .success {
            color: #4caf50;
        }
        
        .features {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
            margin: 50px 0;
        }
        
        .feature-card {
            background: rgba(25, 25, 60, 0.6);
            border-radius: 20px;
            padding: 30px;
            text-align: center;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(106, 176, 255, 0.2);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .feature-card:hover {
            transform: translateY(-10px);
            border-color: rgba(106, 176, 255, 0.5);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.4);
        }
        
        .feature-card::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #8e44ad, #3498db);
            transform: scaleX(0);
            transform-origin: left;
            transition: transform 0.3s ease;
        }
        
        .feature-card:hover::before {
            transform: scaleX(1);
        }
        
        .feature-icon {
            width: 80px;
            height: 80px;
            margin: 0 auto 20px;
            background: linear-gradient(135deg, #6a3291, #2980b9);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            animation: pulse 2s infinite;
        }
        
        .feature-title {
            font-size: 1.5rem;
            margin-bottom: 15px;
            color: #ffffff;
        }
        
        .feature-desc {
            color: #a0a0ff;
            line-height: 1.6;
        }
        
        footer {
            text-align: center;
            padding: 30px 0;
            margin-top: 50px;
            border-top: 1px solid rgba(106, 176, 255, 0.3);
            color: #a0a0ff;
        }
        
        .tagline {
            font-size: 1.2rem;
            margin-top: 10px;
            font-style: italic;
        }
        
        .pulse {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(142, 68, 173, 0.7); }
            70% { box-shadow: 0 0 0 15px rgba(142, 68, 173, 0); }
            100% { box-shadow: 0 0 0 0 rgba(142, 68, 173, 0); }
        }
        
        @keyframes glow {
            from { text-shadow: 0 0 10px rgba(142, 68, 173, 0.3), 0 0 20px rgba(142, 68, 173, 0.2); }
            to { text-shadow: 0 0 20px rgba(142, 68, 173, 0.5), 0 0 30px rgba(142, 68, 173, 0.3); }
        }
        
        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
            100% { transform: translateY(0px); }
        }
        
        @keyframes fadeInUp {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        .interactive-demo {
            background: rgba(30, 30, 70, 0.8);
            border-radius: 15px;
            padding: 25px;
            margin-top: 20px;
            position: relative;
            overflow: hidden;
        }
        
        .demo-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .demo-title {
            font-size: 1.5rem;
            color: #8e44ad;
        }
        
        .demo-controls {
            display: flex;
            gap: 10px;
        }
        
        .demo-btn {
            background: rgba(142, 68, 173, 0.3);
            border: 1px solid rgba(142, 68, 173, 0.5);
            color: #e0e0ff;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .demo-btn:hover {
            background: rgba(142, 68, 173, 0.5);
        }
        
        .demo-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }
        
        @media (max-width: 768px) {
            .demo-content {
                grid-template-columns: 1fr;
            }
        }
        
        .game-screen {
            background: rgba(0, 0, 0, 0.7);
            border-radius: 10px;
            padding: 20px;
            height: 300px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
            overflow: hidden;
        }
        
        .screen-title {
            position: absolute;
            top: 15px;
            left: 15px;
            color: #3498db;
            font-weight: bold;
        }
        
        .grid-world {
            width: 100%;
            height: 100%;
            position: relative;
        }
        
        .player {
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, #4CAF50, #8BC34A);
            border-radius: 50%;
            position: absolute;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            box-shadow: 0 0 15px rgba(76, 175, 80, 0.7);
            transition: all 0.5s ease;
        }
        
        .data-packet {
            width: 30px;
            height: 30px;
            background: linear-gradient(135deg, #3498db, #2980b9);
            border-radius: 5px;
            position: absolute;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            box-shadow: 0 0 10px rgba(52, 152, 219, 0.5);
            cursor: pointer;
            transition: transform 0.2s ease;
        }
        
        .data-packet:hover {
            transform: scale(1.1);
        }
        
        .command-panel {
            background: rgba(0, 0, 0, 0.7);
            border-radius: 10px;
            padding: 20px;
            height: 300px;
            display: flex;
            flex-direction: column;
        }
        
        .command-input {
            display: flex;
            margin-top: auto;
        }
        
        .cmd-input {
            flex: 1;
            background: rgba(50, 50, 100, 0.5);
            border: 1px solid rgba(106, 176, 255, 0.3);
            border-radius: 5px 0 0 5px;
            padding: 10px;
            color: #e0e0ff;
            font-family: 'Courier New', monospace;
        }
        
        .cmd-btn {
            background: linear-gradient(135deg, #8e44ad, #3498db);
            border: none;
            color: white;
            padding: 0 20px;
            border-radius: 0 5px 5px 0;
            cursor: pointer;
        }
        
        .cmd-history {
            flex: 1;
            overflow-y: auto;
            margin-bottom: 15px;
            font-family: 'Courier New', monospace;
            font-size: 14px;
            line-height: 1.5;
        }
        
        .cmd-line {
            margin-bottom: 8px;
        }
        
        .cmd-prompt {
            color: #4CAF50;
        }
        
        .cmd-output {
            color: #e0e0ff;
        }
        
        .cmd-success {
            color: #4CAF50;
        }
        
        .cmd-error {
            color: #f44336;
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(40, 40, 80, 0.9);
            border-left: 4px solid #8e44ad;
            padding: 15px 20px;
            border-radius: 5px;
            transform: translateX(120%);
            transition: transform 0.3s ease;
            z-index: 1000;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }
        
        .notification.show {
            transform: translateX(0);
        }
        
        .notification.success {
            border-left-color: #4CAF50;
        }
        
        .notification.error {
            border-left-color: #f44336;
        }
        
        .progress-container {
            margin-top: 20px;
            background: rgba(50, 50, 100, 0.3);
            border-radius: 10px;
            padding: 15px;
        }
        
        .progress-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        
        .progress-bar {
            height: 10px;
            background: rgba(100, 100, 150, 0.3);
            border-radius: 5px;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #8e44ad, #3498db);
            border-radius: 5px;
            width: 35%;
            transition: width 0.5s ease;
        }
        
        .sphere-selector {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            flex-wrap: wrap;
        }
        
        .sphere-option {
            padding: 8px 15px;
            background: rgba(106, 176, 255, 0.2);
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.9rem;
        }
        
        .sphere-option:hover {
            background: rgba(106, 176, 255, 0.4);
        }
        
        .sphere-option.active {
            background: linear-gradient(135deg, #8e44ad, #3498db);
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">
                <div class="logo-icon">
                    <i class="fas fa-brain"></i>
                </div>
                <div class="logo-text">Project Chimera</div>
            </div>
            <div class="user-info">
                <div class="xp-badge">
                    <i class="fas fa-star"></i>
                    <span>XP: <span id="xp-counter">1,250</span></span>
                </div>
                <div class="avatar" id="avatar">G</div>
            </div>
        </header>
        
        <section class="hero">
            <h1>Defang the Chimera</h1>
            <p class="subtitle">An immersive educational adventure where you become a "Glitch" in a corrupted digital world, learning AI principles and ethics through hands-on challenges.</p>
            <button class="cta-button pulse" id="start-mission">
                <i class="fas fa-play-circle"></i> Start Your Mission
            </button>
        </section>
        
        <div class="game-preview">
            <div class="preview-panel">
                <div class="panel-header">
                    <div class="panel-icon">
                        <i class="fas fa-globe"></i>
                    </div>
                    <h2>The Grid: Data-Spheres</h2>
                </div>
                <p>Explore seven interconnected domains, each focusing on a core AI concept paired with its ethical implications.</p>
                
                <div class="data-spheres">
                    <div class="sphere-card" data-sphere="1">
                        <div class="sphere-icon">
                            <i class="fas fa-cogs"></i>
                        </div>
                        <div class="sphere-title">The Thinking Machine</div>
                        <div class="sphere-concept">Algorithms & Logic</div>
                        <div class="sphere-ethics">Accountability</div>
                    </div>
                    
                    <div class="sphere-card" data-sphere="2">
                        <div class="sphere-icon">
                            <i class="fas fa-database"></i>
                        </div>
                        <div class="sphere-title">The Learning Algorithm</div>
                        <div class="sphere-concept">Supervised Learning</div>
                        <div class="sphere-ethics">Transparency</div>
                    </div>
                    
                    <div class="sphere-card" data-sphere="3">
                        <div class="sphere-icon">
                            <i class="fas fa-project-diagram"></i>
                        </div>
                        <div class="sphere-title">The Black Box</div>
                        <div class="sphere-concept">Neural Networks</div>
                        <div class="sphere-ethics">Explainability</div>
                    </div>
                    
                    <div class="sphere-card" data-sphere="4">
                        <div class="sphere-icon">
                            <i class="fas fa-balance-scale"></i>
                        </div>
                        <div class="sphere-title">The Biased Code</div>
                        <div class="sphere-concept">Algorithmic Bias</div>
                        <div class="sphere-ethics">Fairness</div>
                    </div>
                </div>
                
                <div class="progress-container">
                    <div class="progress-header">
                        <span>Mission Progress</span>
                        <span>35%</span>
                    </div>
                    <div class="progress-bar">
                        <div class="progress-fill"></div>
                    </div>
                </div>
            </div>
            
            <div class="preview-panel">
                <div class="panel-header">
                    <div class="panel-icon">
                        <i class="fas fa-gamepad"></i>
                    </div>
                    <h2>Interactive Challenge Demo</h2>
                </div>
                <p>Experience hands-on learning through immersive gameplay mechanics designed to teach real AI concepts.</p>
                
                <div class="interactive-demo">
                    <div class="demo-header">
                        <div class="demo-title">The Thinking Machine Challenge</div>
                        <div class="demo-controls">
                            <button class="demo-btn" id="reset-demo">Reset</button>
                            <button class="demo-btn" id="auto-demo">Auto Demo</button>
                        </div>
                    </div>
                    
                    <div class="demo-content">
                        <div class="game-screen">
                            <div class="screen-title">GRID MAP</div>
                            <div class="grid-world" id="grid-world">
                                <!-- Grid elements will be generated by JS -->
                            </div>
                        </div>
                        
                        <div class="command-panel">
                            <div class="cmd-history" id="cmd-history">
                                <div class="cmd-line"><span class="cmd-prompt">oracle@grid:~$</span> <span class="cmd-output">Welcome, Glitch. I am Oracle.</span></div>
                                <div class="cmd-line"><span class="cmd-prompt">oracle@grid:~$</span> <span class="cmd-output">You must learn the fundamental algorithms that govern our reality.</span></div>
                                <div class="cmd-line"><span class="cmd-prompt">oracle@grid:~$</span> <span class="cmd-output">Type 'help' to begin your first challenge.</span></div>
                            </div>
                            
                            <div class="command-input">
                                <input type="text" class="cmd-input" id="cmd-input" placeholder="Enter command..." autocomplete="off">
                                <button class="cmd-btn" id="cmd-submit">Execute</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="features">
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-graduation-cap"></i>
                </div>
                <h3 class="feature-title">Educational Foundation</h3>
                <p class="feature-desc">Curriculum designed to meet NEP 2020 requirements for AI and ethics education with integrated learning objectives.</p>
            </div>
            
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-heart"></i>
                </div>
                <h3 class="feature-title">Ethical Focus</h3>
                <p class="feature-desc">Every technical concept is paired with its ethical implications, teaching responsible AI development.</p>
            </div>
            
            <div class="feature-card">
                <div class="feature-icon">
                    <i class="fas fa-hands-helping"></i>
                </div>
                <h3 class="feature-title">Narrative-Driven</h3>
                <p class="feature-desc">Become a "Glitch" in a corrupted digital world, fighting against unethical AI through compelling storytelling.</p>
            </div>
        </div>
        
        <div class="preview-panel">
            <div class="panel-header">
                <div class="panel-icon">
                    <i class="fas fa-map-signs"></i>
                </div>
                <h2>Your Mission Path</h2>
            </div>
            <p>Select a Data-Sphere to explore its challenges and learn its concepts:</p>
            
            <div class="sphere-selector">
                <div class="sphere-option active">The Thinking Machine</div>
                <div class="sphere-option">The Learning Algorithm</div>
                <div class="sphere-option">The Black Box</div>
                <div class="sphere-option">The Biased Code</div>
                <div class="sphere-option">The Digital Ghost</div>
                <div class="sphere-option">The Prompt Engineer</div>
                <div class="sphere-option">The Creator's Responsibility</div>
            </div>
        </div>
        
        <footer>
            <p>Project Chimera: An AI Ethics Educational Adventure</p>
            <p class="tagline glow">"Other coding classes teach syntax. We teach consequence."</p>
        </footer>
    </div>
    
    <div class="notification" id="notification">
        <div class="notification-content">Mission started successfully!</div>
    </div>

    <script>
        // Game state
        const gameState = {
            xp: 1250,
            currentSphere: 1,
            playerPosition: { x: 150, y: 150 },
            dataPackets: [
                { id: 1, x: 100, y: 100, collected: false },
                { id: 2, x: 200, y: 80, collected: false },
                { id: 3, x: 250, y: 180, collected: false },
                { id: 4, x: 120, y: 220, collected: false }
            ],
            commandHistory: [
                { type: 'output', text: 'Welcome, Glitch. I am Oracle.' },
                { type: 'output', text: 'You must learn the fundamental algorithms that govern our reality.' },
                { type: 'output', text: "Type 'help' to begin your first challenge." }
            ]
        };
        
        // DOM Elements
        const xpCounter = document.getElementById('xp-counter');
        const avatar = document.getElementById('avatar');
        const startMissionBtn = document.getElementById('start-mission');
        const cmdInput = document.getElementById('cmd-input');
        const cmdSubmit = document.getElementById('cmd-submit');
        const cmdHistory = document.getElementById('cmd-history');
        const gridWorld = document.getElementById('grid-world');
        const notification = document.getElementById('notification');
        const resetDemoBtn = document.getElementById('reset-demo');
        const autoDemoBtn = document.getElementById('auto-demo');
        
        // Initialize game
        function initGame() {
            renderGridWorld();
            setupEventListeners();
            updateXPDisplay();
        }
        
        // Render grid world
        function renderGridWorld() {
            gridWorld.innerHTML = '';
            
            // Create player
            const player = document.createElement('div');
            player.className = 'player';
            player.id = 'player';
            player.textContent = 'G';
            player.style.left = `${gameState.playerPosition.x}px`;
            player.style.top = `${gameState.playerPosition.y}px`;
            gridWorld.appendChild(player);
            
            // Create data packets
            gameState.dataPackets.forEach(packet => {
                if (!packet.collected) {
                    const packetEl = document.createElement('div');
                    packetEl.className = 'data-packet';
                    packetEl.dataset.id = packet.id;
                    packetEl.textContent = '01';
                    packetEl.style.left = `${packet.x}px`;
                    packetEl.style.top = `${packet.y}px`;
                    gridWorld.appendChild(packetEl);
                    
                    // Add click event to collect packet
                    packetEl.addEventListener('click', () => {
                        collectDataPacket(packet.id);
                    });
                }
            });
        }
        
        // Collect data packet
        function collectDataPacket(packetId) {
            const packet = gameState.dataPackets.find(p => p.id === packetId);
            if (packet && !packet.collected) {
                packet.collected = true;
                const packetEl = document.querySelector(`.data-packet[data-id="${packetId}"]`);
                if (packetEl) {
                    packetEl.style.opacity = '0';
                    packetEl.style.transform = 'scale(0)';
                    setTimeout(() => packetEl.remove(), 300);
                }
                
                // Add to command history
                addToCommandHistory('output', `You collected a data packet! This represents an algorithm instruction.`);
                
                // Check if all packets collected
                const allCollected = gameState.dataPackets.every(p => p.collected);
                if (allCollected) {
                    setTimeout(() => {
                        addToCommandHistory('success', 'Challenge Complete! You\'ve mastered basic algorithmic thinking.');
                        showNotification('Challenge completed! +150 XP', 'success');
                        gameState.xp += 150;
                        updateXPDisplay();
                    }, 1000);
                }
            }
        }
        
        // Add to command history
        function addToCommandHistory(type, text) {
            const line = document.createElement('div');
            line.className = `cmd-line`;
            
            if (type === 'command') {
                line.innerHTML = `<span class="cmd-prompt">glitch@grid:~$</span> <span class="cmd-command">${text}</span>`;
            } else if (type === 'output') {
                line.innerHTML = `<span class="cmd-prompt">oracle@grid:~$</span> <span class="cmd-output">${text}</span>`;
            } else if (type === 'success') {
                line.innerHTML = `<span class="cmd-prompt">oracle@grid:~$</span> <span class="cmd-success">${text}</span>`;
            } else if (type === 'error') {
                line.innerHTML = `<span class="cmd-prompt">oracle@grid:~$</span> <span class="cmd-error">${text}</span>`;
            }
            
            cmdHistory.appendChild(line);
            cmdHistory.scrollTop = cmdHistory.scrollHeight;
        }
        
        // Process command
        function processCommand(command) {
            command = command.trim().toLowerCase();
            
            if (command === 'help') {
                addToCommandHistory('output', 'Available commands: go [direction], look, take [item], inventory, help');
            } else if (command.startsWith('go ')) {
                const direction = command.split(' ')[1];
                movePlayer(direction);
            } else if (command === 'look') {
                addToCommandHistory('output', 'You are in the Central Hub of the Grid. Data packets are scattered around.');
            } else if (command === 'inventory') {
                addToCommandHistory('output', 'You are carrying: None');
            } else if (command === '') {
                // Do nothing for empty commands
            } else {
                addToCommandHistory('error', `I don't understand '${command}'. Type 'help' for available commands.`);
            }
        }
        
        // Move player
        function movePlayer(direction) {
            const moveAmount = 20;
            let newX = gameState.playerPosition.x;
            let newY = gameState.playerPosition.y;
            
            switch(direction) {
                case 'north':
                case 'n':
                    newY = Math.max(20, newY - moveAmount);
                    break;
                case 'south':
                case 's':
                    newY = Math.min(280, newY + moveAmount);
                    break;
                case 'east':
                case 'e':
                    newX = Math.min(380, newX + moveAmount);
                    break;
                case 'west':
                case 'w':
                    newX = Math.max(20, newX - moveAmount);
                    break;
                default:
                    addToCommandHistory('error', `Invalid direction. Try: north, south, east, west`);
                    return;
            }
            
            gameState.playerPosition.x = newX;
            gameState.playerPosition.y = newY;
            
            const player = document.getElementById('player');
            player.style.left = `${newX}px`;
            player.style.top = `${newY}px`;
            
            addToCommandHistory('output', `You move ${direction}.`);
        }
        
        // Update XP display
        function updateXPDisplay() {
            xpCounter.textContent = gameState.xp.toLocaleString();
        }
        
        // Show notification
        function showNotification(message, type = 'info') {
            notification.className = 'notification show ' + type;
            notification.querySelector('.notification-content').textContent = message;
            
            setTimeout(() => {
                notification.className = 'notification';
            }, 3000);
        }
        
        // Reset demo
        function resetDemo() {
            gameState.playerPosition = { x: 150, y: 150 };
            gameState.dataPackets.forEach(packet => packet.collected = false);
            gameState.commandHistory = [
                { type: 'output', text: 'Welcome, Glitch. I am Oracle.' },
                { type: 'output', text: 'You must learn the fundamental algorithms that govern our reality.' },
                { type: 'output', text: "Type 'help' to begin your first challenge." }
            ];
            
            cmdHistory.innerHTML = '';
            gameState.commandHistory.forEach(entry => {
                addToCommandHistory(entry.type, entry.text);
            });
            
            renderGridWorld();
            showNotification('Demo reset successfully!', 'success');
        }
        
        // Auto demo
        function autoDemo() {
            const commands = ['help', 'go north', 'go east', 'go south', 'look'];
            let i = 0;
            
            function executeNext() {
                if (i < commands.length) {
                    cmdInput.value = commands[i];
                    setTimeout(() => {
                        cmdSubmit.click();
                        i++;
                        setTimeout(executeNext, 1000);
                    }, 500);
                }
            }
            
            executeNext();
        }
        
        // Setup event listeners
        function setupEventListeners() {
            // Start mission button
            startMissionBtn.addEventListener('click', function() {
                this.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Initializing Mission...';
                setTimeout(() => {
                    this.innerHTML = '<i class="fas fa-check"></i> Mission Started!';
                    this.style.background = 'linear-gradient(135deg, #4CAF50, #8BC34A)';
                    showNotification('Mission started successfully!', 'success');
                }, 1500);
            });
            
            // Command input
            cmdSubmit.addEventListener('click', function() {
                const command = cmdInput.value;
                if (command) {
                    addToCommandHistory('command', command);
                    processCommand(command);
                    cmdInput.value = '';
                }
            });
            
            cmdInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    cmdSubmit.click();
                }
            });
            
            // Avatar click
            avatar.addEventListener('click', function() {
                this.textContent = String.fromCharCode(65 + Math.floor(Math.random() * 26));
                showNotification('Avatar updated!', 'success');
            });
            
            // Reset demo
            resetDemoBtn.addEventListener('click', resetDemo);
            
            // Auto demo
            autoDemoBtn.addEventListener('click', autoDemo);
            
            // Sphere cards
            document.querySelectorAll('.sphere-card').forEach(card => {
                card.addEventListener('click', function() {
                    const sphereId = this.dataset.sphere;
                    showNotification(`Selected: ${this.querySelector('.sphere-title').textContent}`, 'info');
                });
            });
            
            // Sphere selector
            document.querySelectorAll('.sphere-option').forEach(option => {
                option.addEventListener('click', function() {
                    document.querySelectorAll('.sphere-option').forEach(opt => {
                        opt.classList.remove('active');
                    });
                    this.classList.add('active');
                    showNotification(`Mission path set to: ${this.textContent}`, 'info');
                });
            });
        }
        
        // Initialize when DOM is loaded
        document.addEventListener('DOMContentLoaded', initGame);
    </script>
</body>
</html>
