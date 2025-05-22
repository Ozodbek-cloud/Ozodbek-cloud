<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ozodbek Nasriddinov | Full Stack Developer</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.11.4/gsap.min.js"></script>
    <style>
        :root {
            --neon: #0af;
            --dark: #0a0a12;
            --darker: #050508;
            --light: #f0f0ff;
            --accent: #f06;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Rajdhani', sans-serif;
            background-color: var(--darker);
            color: var(--light);
            overflow-x: hidden;
            perspective: 1000px;
        }
        
        #video-bg {
            position: fixed;
            right: 0;
            bottom: 0;
            min-width: 100%;
            min-height: 100%;
            z-index: -1;
            opacity: 0.15;
            filter: blur(1px) brightness(0.3);
        }
        
        #threejs-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.7;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
            position: relative;
            z-index: 2;
        }
        
        /* Mobile Menu */
        .mobile-menu-btn {
            display: none;
            background: none;
            border: none;
            color: var(--light);
            font-size: 1.5rem;
            cursor: pointer;
            z-index: 1000;
        }
        
        .mobile-menu {
            position: fixed;
            top: 0;
            right: -100%;
            width: 80%;
            height: 100vh;
            background: var(--darker);
            z-index: 999;
            transition: right 0.3s ease;
            padding: 80px 30px;
            box-shadow: -5px 0 15px rgba(0,0,0,0.5);
        }
        
        .mobile-menu.active {
            right: 0;
        }
        
        .mobile-menu ul {
            list-style: none;
        }
        
        .mobile-menu ul li {
            margin-bottom: 30px;
        }
        
        .mobile-menu ul li a {
            color: var(--light);
            text-decoration: none;
            font-size: 1.5rem;
            transition: all 0.3s ease;
        }
        
        .mobile-menu ul li a:hover {
            color: var(--neon);
        }
        
        .mobile-menu-close {
            position: absolute;
            top: 20px;
            right: 20px;
            background: none;
            border: none;
            color: var(--light);
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        header {
            padding: 30px 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
            animation: fadeIn 1.5s ease-out;
        }
        
        .logo {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(90deg, var(--neon), var(--accent));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 10px rgba(0, 170, 255, 0.3);
        }
        
        nav ul {
            display: flex;
            list-style: none;
        }
        
        nav ul li {
            margin-left: 25px;
            position: relative;
        }
        
        nav ul li a {
            color: var(--light);
            text-decoration: none;
            font-weight: 500;
            font-size: 1rem;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        nav ul li a:hover {
            color: var(--neon);
            text-shadow: 0 0 10px var(--neon);
        }
        
        nav ul li a::after {
            content: '';
            position: absolute;
            width: 0;
            height: 2px;
            background: var(--neon);
            bottom: -5px;
            left: 0;
            transition: width 0.3s ease;
        }
        
        nav ul li a:hover::after {
            width: 100%;
        }
        
        .hero {
            height: 90vh;
            display: flex;
            align-items: center;
            position: relative;
        }
        
        .hero-content {
            max-width: 800px;
        }
        
        .hero h1 {
            font-size: clamp(2.5rem, 5vw, 5rem);
            margin-bottom: 20px;
            line-height: 1.1;
            background: linear-gradient(90deg, var(--light), var(--neon));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            animation: textGlow 3s ease-in-out infinite alternate;
        }
        
        @keyframes textGlow {
            0% { text-shadow: 0 0 10px rgba(0, 170, 255, 0.3); }
            100% { text-shadow: 0 0 20px var(--neon), 0 0 30px rgba(0, 170, 255, 0.5); }
        }
        
        .hero p {
            font-size: clamp(1rem, 2vw, 1.5rem);
            margin-bottom: 30px;
            opacity: 0.9;
            line-height: 1.6;
        }
        
        .cta-button {
            display: inline-block;
            padding: 12px 30px;
            background: transparent;
            color: var(--neon);
            border: 2px solid var(--neon);
            border-radius: 50px;
            font-weight: 600;
            text-decoration: none;
            transition: all 0.4s ease;
            position: relative;
            overflow: hidden;
            z-index: 1;
            font-size: 1rem;
            letter-spacing: 1px;
            text-transform: uppercase;
            box-shadow: 0 0 15px rgba(0, 170, 255, 0.2);
            margin-right: 15px;
            margin-bottom: 15px;
        }
        
        .cta-button:hover {
            color: var(--darker);
            transform: translateY(-3px);
            box-shadow: 0 5px 20px var(--neon);
        }
        
        .cta-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, var(--neon), transparent);
            transition: all 0.6s ease;
            z-index: -1;
        }
        
        .cta-button:hover::before {
            left: 100%;
        }
        
        .section {
            padding: 80px 0;
            position: relative;
        }
        
        .section-title {
            font-size: clamp(2rem, 5vw, 3rem);
            margin-bottom: 40px;
            text-align: center;
            position: relative;
            display: inline-block;
            left: 50%;
            transform: translateX(-50%);
        }
        
        .section-title::after {
            content: '';
            position: absolute;
            width: 50%;
            height: 3px;
            background: linear-gradient(90deg, transparent, var(--neon), transparent);
            bottom: -10px;
            left: 25%;
        }
        
        .projects-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
        }
        
        .project-card {
            background: rgba(10, 10, 18, 0.7);
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(0, 170, 255, 0.1);
            transform-style: preserve-3d;
        }
        
        .project-card:hover {
            transform: translateY(-10px) scale(1.02);
            box-shadow: 0 15px 40px rgba(0, 170, 255, 0.3);
            border: 1px solid rgba(0, 170, 255, 0.3);
        }
        
        .project-img {
            height: 180px;
            width: 100%;
            object-fit: cover;
            border-bottom: 1px solid rgba(0, 170, 255, 0.1);
        }
        
        .project-content {
            padding: 20px;
        }
        
        .project-title {
            font-size: 1.3rem;
            margin-bottom: 10px;
            color: var(--neon);
        }
        
        .project-description {
            margin-bottom: 15px;
            line-height: 1.6;
            opacity: 0.8;
            font-size: 0.9rem;
        }
        
        .project-tech {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-bottom: 15px;
        }
        
        .tech-tag {
            background: rgba(0, 170, 255, 0.1);
            color: var(--neon);
            padding: 4px 12px;
            border-radius: 50px;
            font-size: 0.7rem;
            border: 1px solid rgba(0, 170, 255, 0.2);
        }
        
        .project-links {
            display: flex;
            gap: 10px;
        }
        
        .project-link {
            color: var(--light);
            text-decoration: none;
            font-size: 0.8rem;
            display: flex;
            align-items: center;
            transition: all 0.3s ease;
        }
        
        .project-link:hover {
            color: var(--neon);
            transform: translateX(5px);
        }
        
        .skills-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }
        
        .skill-category {
            background: rgba(10, 10, 18, 0.7);
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(0, 170, 255, 0.1);
            transition: all 0.3s ease;
        }
        
        .skill-category:hover {
            border: 1px solid rgba(0, 170, 255, 0.3);
            transform: translateY(-5px);
        }
        
        .skill-title {
            font-size: 1.1rem;
            margin-bottom: 15px;
            color: var(--neon);
            position: relative;
            padding-bottom: 8px;
        }
        
        .skill-title::after {
            content: '';
            position: absolute;
            width: 40px;
            height: 2px;
            background: var(--neon);
            bottom: 0;
            left: 0;
        }
        
        .skill-list {
            list-style: none;
        }
        
        .skill-item {
            margin-bottom: 8px;
            position: relative;
            padding-left: 15px;
            font-size: 0.9rem;
        }
        
        .skill-item::before {
            content: '▹';
            position: absolute;
            left: 0;
            color: var(--neon);
        }
        
        .contact-form {
            max-width: 600px;
            margin: 0 auto;
            background: rgba(10, 10, 18, 0.7);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(0, 170, 255, 0.1);
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-label {
            display: block;
            margin-bottom: 8px;
            color: var(--neon);
            font-size: 0.9rem;
        }
        
        .form-input, .form-textarea {
            width: 100%;
            padding: 12px;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(0, 170, 255, 0.2);
            border-radius: 8px;
            color: var(--light);
            font-family: inherit;
            transition: all 0.3s ease;
            font-size: 0.9rem;
        }
        
        .form-input:focus, .form-textarea:focus {
            outline: none;
            border: 1px solid var(--neon);
            box-shadow: 0 0 10px rgba(0, 170, 255, 0.3);
        }
        
        .form-textarea {
            min-height: 120px;
            resize: vertical;
        }
        
        .submit-btn {
            background: transparent;
            color: var(--neon);
            border: 2px solid var(--neon);
            padding: 12px 30px;
            border-radius: 50px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.4s ease;
            display: block;
            margin: 0 auto;
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .submit-btn:hover {
            background: var(--neon);
            color: var(--darker);
            box-shadow: 0 0 20px var(--neon);
            transform: translateY(-3px);
        }
        
        footer {
            text-align: center;
            padding: 40px 0;
            border-top: 1px solid rgba(0, 170, 255, 0.1);
            margin-top: 60px;
        }
        
        .social-links {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin: 20px 0;
        }
        
        .social-link {
            color: var(--light);
            font-size: 1.2rem;
            transition: all 0.3s ease;
        }
        
        .social-link:hover {
            color: var(--neon);
            transform: translateY(-5px) scale(1.2);
        }
        
        .copyright {
            opacity: 0.7;
            font-size: 0.8rem;
        }
        
        /* Floating animations */
        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-15px); }
            100% { transform: translateY(0px); }
        }
        
        .floating {
            animation: float 6s ease-in-out infinite;
        }
        
        /* Scroll animations */
        .hidden {
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.8s ease;
        }
        
        .show {
            opacity: 1;
            transform: translateY(0);
        }
        
        /* Particle cursor - Desktop only */
        .cursor {
            position: fixed;
            width: 15px;
            height: 15px;
            border-radius: 50%;
            background: var(--neon);
            pointer-events: none;
            mix-blend-mode: screen;
            z-index: 999;
            transform: translate(-50%, -50%);
            opacity: 0.7;
            transition: transform 0.1s ease;
        }
        
        .cursor-follower {
            position: fixed;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            border: 1px solid var(--neon);
            pointer-events: none;
            z-index: 998;
            transform: translate(-50%, -50%);
            transition: transform 0.4s ease, width 0.3s ease, height 0.3s ease;
            opacity: 0.3;
        }
        
        /* Responsive Breakpoints */
        @media (max-width: 992px) {
            .projects-grid {
                grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            }
            
            .skills-container {
                grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
            }
        }
        
        @media (max-width: 768px) {
            nav ul {
                display: none;
            }
            
            .mobile-menu-btn {
                display: block;
            }
            
            .hero {
                height: auto;
                padding: 100px 0;
                text-align: center;
            }
            
            .cta-button {
                display: block;
                margin: 0 auto 15px;
                width: 80%;
                max-width: 250px;
            }
            
            .project-links {
                justify-content: center;
            }
            
            .cursor, .cursor-follower {
                display: none;
            }
        }
        
        @media (max-width: 576px) {
            .hero h1 {
                font-size: 2.2rem;
            }
            
            .hero p {
                font-size: 1rem;
            }
            
            .projects-grid {
                grid-template-columns: 1fr;
            }
            
            .skills-container {
                grid-template-columns: 1fr;
            }
            
            .section {
                padding: 60px 0;
            }
            
            .contact-form {
                padding: 20px;
            }
        }
    </style>
</head>
<body>
    <!-- Video Background -->
    <video autoplay muted loop id="video-bg">
        <source src="https://assets.mixkit.co/videos/preview/mixkit-network-of-lines-illuminated-in-a-dark-background-3633-large.mp4" type="video/mp4">
    </video>
    
    <!-- 3D Background -->
    <div id="threejs-container"></div>
    
    <!-- Custom Cursor (Desktop only) -->
    <div class="cursor"></div>
    <div class="cursor-follower"></div>
    
    <!-- Mobile Menu Button -->
    <button class="mobile-menu-btn" id="mobileMenuBtn">☰</button>
    
    <!-- Mobile Menu -->
    <div class="mobile-menu" id="mobileMenu">
        <button class="mobile-menu-close" id="mobileMenuClose">✕</button>
        <ul>
            <li><a href="#about" onclick="closeMobileMenu()">About</a></li>
            <li><a href="#projects" onclick="closeMobileMenu()">Projects</a></li>
            <li><a href="#skills" onclick="closeMobileMenu()">Skills</a></li>
            <li><a href="#contact" onclick="closeMobileMenu()">Contact</a></li>
        </ul>
    </div>
    
    <div class="container">
        <header>
            <div class="logo">OZODBEK</div>
            <nav>
                <ul>
                    <li><a href="#about">About</a></li>
                    <li><a href="#projects">Projects</a></li>
                    <li><a href="#skills">Skills</a></li>
                    <li><a href="#contact">Contact</a></li>
                </ul>
            </nav>
        </header>
        
        <section class="hero">
            <div class="hero-content">
                <h1 class="floating">Full Stack Developer & Digital Architect</h1>
                <p>I craft immersive digital experiences that push boundaries and elevate brands to new heights. With a perfect blend of creativity and technical expertise, I turn complex problems into elegant solutions.</p>
                <div style="display: flex; flex-wrap: wrap; justify-content: center;">
                    <a href="#projects" class="cta-button">Explore My Work</a>
                    <a href="#contact" class="cta-button">Hire Me</a>
                </div>
            </div>
        </section>
        
        <section id="about" class="section">
            <h2 class="section-title hidden">About Me</h2>
            <div class="about-content hidden" style="text-align: center; max-width: 800px; margin: 0 auto;">
                <p style="font-size: 1.1rem; line-height: 1.8; margin-bottom: 20px;">
                    I'm Ozodbek Nasriddinov, a passionate Full Stack Developer with over 5 years of experience building cutting-edge web applications. My journey in tech has taken me from simple websites to complex enterprise systems, always with a focus on performance, scalability, and user experience.
                </p>
                <p style="font-size: 1.1rem; line-height: 1.8;">
                    When I'm not coding, you can find me contributing to open-source projects, mentoring junior developers, or exploring the latest in AI and blockchain technologies. I believe in continuous learning and pushing the boundaries of what's possible on the web.
                </p>
            </div>
        </section>
        
        <section id="projects" class="section">
            <h2 class="section-title hidden">Featured Projects</h2>
            <div class="projects-grid">
                <div class="project-card hidden">
                    <img src="https://images.unsplash.com/photo-1551288049-bebda4e38f71?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=80" alt="E-Commerce Platform" class="project-img">
                    <div class="project-content">
                        <h3 class="project-title">Nexus Commerce</h3>
                        <p class="project-description">A high-performance e-commerce platform with AI-powered recommendations and blockchain payments.</p>
                        <div class="project-tech">
                            <span class="tech-tag">React</span>
                            <span class="tech-tag">Node.js</span>
                            <span class="tech-tag">GraphQL</span>
                        </div>
                        <div class="project-links">
                            <a href="#" class="project-link">Live Demo</a>
                            <a href="#" class="project-link">GitHub</a>
                        </div>
                    </div>
                </div>
                
                <div class="project-card hidden">
                    <img src="https://images.unsplash.com/photo-1516321318423-f06f85e504b3?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=80" alt="Dev Network" class="project-img">
                    <div class="project-content">
                        <h3 class="project-title">CodeSphere</h3>
                        <p class="project-description">A social platform for developers featuring real-time code collaboration and pair programming.</p>
                        <div class="project-tech">
                            <span class="tech-tag">Vue.js</span>
                            <span class="tech-tag">Firebase</span>
                            <span class="tech-tag">WebSockets</span>
                        </div>
                        <div class="project-links">
                            <a href="#" class="project-link">Live Demo</a>
                            <a href="#" class="project-link">GitHub</a>
                        </div>
                    </div>
                </div>
                
                <div class="project-card hidden">
                    <img src="https://images.unsplash.com/photo-1460925895917-afdab827c52f?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=80" alt="AI Analytics" class="project-img">
                    <div class="project-content">
                        <h3 class="project-title">Neural Insights</h3>
                        <p class="project-description">AI-powered business intelligence dashboard with predictive analytics and automated reporting.</p>
                        <div class="project-tech">
                            <span class="tech-tag">Python</span>
                            <span class="tech-tag">TensorFlow</span>
                            <span class="tech-tag">D3.js</span>
                        </div>
                        <div class="project-links">
                            <a href="#" class="project-link">Live Demo</a>
                            <a href="#" class="project-link">GitHub</a>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        
        <section id="skills" class="section">
            <h2 class="section-title hidden">Technical Skills</h2>
            <div class="skills-container">
                <div class="skill-category hidden">
                    <h3 class="skill-title">Frontend</h3>
                    <ul class="skill-list">
                        <li class="skill-item">React.js / Next.js</li>
                        <li class="skill-item">Vue.js / Nuxt.js</li>
                        <li class="skill-item">TypeScript</li>
                        <li class="skill-item">Three.js / WebGL</li>
                    </ul>
                </div>
                
                <div class="skill-category hidden">
                    <h3 class="skill-title">Backend</h3>
                    <ul class="skill-list">
                        <li class="skill-item">Node.js / Express</li>
                        <li class="skill-item">Python / Django</li>
                        <li class="skill-item">GraphQL / Apollo</li>
                        <li class="skill-item">RESTful APIs</li>
                    </ul>
                </div>
                
                <div class="skill-category hidden">
                    <h3 class="skill-title">Database</h3>
                    <ul class="skill-list">
                        <li class="skill-item">MongoDB / Mongoose</li>
                        <li class="skill-item">PostgreSQL</li>
                        <li class="skill-item">Firebase</li>
                        <li class="skill-item">Redis</li>
                    </ul>
                </div>
                
                <div class="skill-category hidden">
                    <h3 class="skill-title">DevOps & More</h3>
                    <ul class="skill-list">
                        <li class="skill-item">Docker / Kubernetes</li>
                        <li class="skill-item">AWS / GCP</li>
                        <li class="skill-item">CI/CD Pipelines</li>
                        <li class="skill-item">Blockchain</li>
                    </ul>
                </div>
            </div>
        </section>
        
        <section id="contact" class="section">
            <h2 class="section-title hidden">Get In Touch</h2>
            <div class="contact-form hidden">
                <form>
                    <div class="form-group">
                        <label for="name" class="form-label">Your Name</label>
                        <input type="text" id="name" class="form-input" required>
                    </div>
                    <div class="form-group">
                        <label for="email" class="form-label">Your Email</label>
                        <input type="email" id="email" class="form-input" required>
                    </div>
                    <div class="form-group">
                        <label for="message" class="form-label">Your Message</label>
                        <textarea id="message" class="form-textarea" required></textarea>
                    </div>
                    <button type="submit" class="submit-btn">Send Message</button>
                </form>
            </div>
        </section>
        
        <footer class="hidden">
            <div class="logo" style="font-size: 2rem; margin-bottom: 15px;">OZODBEK NASRIDDINOV</div>
            <div class="social-links">
                <a href="#" class="social-link">GitHub</a>
                <a href="#" class="social-link">LinkedIn</a>
                <a href="#" class="social-link">Twitter</a>
                <a href="#" class="social-link">Instagram</a>
            </div>
            <p class="copyright">© 2023 Ozodbek Nasriddinov. All rights reserved.</p>
        </footer>
    </div>
    
    <script>
        // Mobile Menu Functionality
        const mobileMenuBtn = document.getElementById('mobileMenuBtn');
        const mobileMenu = document.getElementById('mobileMenu');
        const mobileMenuClose = document.getElementById('mobileMenuClose');
        
        function toggleMobileMenu() {
            mobileMenu.classList.toggle('active');
        }
        
        function closeMobileMenu() {
            mobileMenu.classList.remove('active');
        }
        
        mobileMenuBtn.addEventListener('click', toggleMobileMenu);
        mobileMenuClose.addEventListener('click', closeMobileMenu);
        
        // 3D Scene Setup (Simplified for mobile)
        const threeContainer = document.getElementById('threejs-container');
        
        // Only initialize 3D on desktop
        if (window.innerWidth > 768) {
            const scene = new THREE.Scene();
            const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
            
            renderer.setSize(window.innerWidth, window.innerHeight);
            threeContainer.appendChild(renderer.domElement);
            
            // Add floating geometry
            const geometry = new THREE.IcosahedronGeometry(1, 0);
            const material = new THREE.MeshBasicMaterial({ 
                color: 0x00aaff,
                wireframe: true,
                transparent: true,
                opacity: 0.5
            });
            
            const particles = [];
            const particleCount = window.innerWidth < 992 ? 15 : 30;
            
            for (let i = 0; i < particleCount; i++) {
                const particle = new THREE.Mesh(geometry, material.clone());
                
                particle.position.x = Math.random() * 20 - 10;
                particle.position.y = Math.random() * 20 - 10;
                particle.position.z = Math.random() * 20 - 10;
                
                particle.scale.x = particle.scale.y = particle.scale.z = Math.random() * 0.5 + 0.1;
                
                particle.rotation.x = Math.random() * Math.PI;
                particle.rotation.y = Math.random() * Math.PI;
                
                particles.push({
                    mesh: particle,
                    speed: Math.random() * 0.01 + 0.005,
                    rotation: {
                        x: Math.random() * 0.01 - 0.005,
                        y: Math.random() * 0.01 - 0.005,
                        z: Math.random() * 0.01 - 0.005
                    }
                });
                
                scene.add(particle);
            }
            
            camera.position.z = 5;
            
            // Animation loop
            function animate() {
                requestAnimationFrame(animate);
                
                particles.forEach(particle => {
                    particle.mesh.position.y += particle.speed;
                    if (particle.mesh.position.y > 10) particle.mesh.position.y = -10;
                    
                    particle.mesh.rotation.x += particle.rotation.x;
                    particle.mesh.rotation.y += particle.rotation.y;
                    particle.mesh.rotation.z += particle.rotation.z;
                });
                
                renderer.render(scene, camera);
            }
            
            animate();
            
            // Handle window resize
            window.addEventListener('resize', () => {
                camera.aspect = window.innerWidth / window.innerHeight;
                camera.updateProjectionMatrix();
                renderer.setSize(window.innerWidth, window.innerHeight);
            });
            
            // Custom cursor (Desktop only)
            const cursor = document.querySelector('.cursor');
            const cursorFollower = document.querySelector('.cursor-follower');
            
            document.addEventListener('mousemove', (e) => {
                cursor.style.left = e.clientX + 'px';
                cursor.style.top = e.clientY + 'px';
                
                gsap.to(cursorFollower, {
                    duration: 0.5,
                    left: e.clientX + 'px',
                    top: e.clientY + 'px'
                });
            });
            
            document.querySelectorAll('a, button').forEach(el => {
                el.addEventListener('mouseenter', () => {
                    cursor.style.transform = 'translate(-50%, -50%) scale(1.5)';
                    cursor.style.opacity = '0.5';
                    cursorFollower.style.width = '25px';
                    cursorFollower.style.height = '25px';
                    cursorFollower.style.opacity = '0.5';
                });
                
                el.addEventListener('mouseleave', () => {
                    cursor.style.transform = 'translate(-50%, -50%) scale(1)';
                    cursor.style.opacity = '0.7';
                    cursorFollower.style.width = '30px';
                    cursorFollower.style.height = '30px';
                    cursorFollower.style.opacity = '0.3';
                });
            });
        } else {
            // Remove 3D container on mobile
            threeContainer.style.display = 'none';
        }
        
        // Scroll animations
        const hiddenElements = document.querySelectorAll('.hidden');
        
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('show');
                    observer.unobserve(entry.target);
                }
            });
        }, { threshold: 0.1 });
        
        hiddenElements.forEach(el => observer.observe(el));
        
        // Smooth scrolling for anchor links
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function(e) {
                e.preventDefault();
                
                const targetId = this.getAttribute('href');
                const targetElement = document.querySelector(targetId);
                
                if (targetElement) {
                    window.scrollTo({
                        top: targetElement.offsetTop - 80,
                        behavior: 'smooth'
                    });
                }
            });
        });
        
        // Video fallback for mobile
        const videoBg = document.getElementById('video-bg');
        if (window.innerWidth < 768) {
            videoBg.style.display = 'none';
            document.body.style.background = 'linear-gradient(135deg, #050508, #0a0a12)';
        }
    </script>
</body>
</html>
