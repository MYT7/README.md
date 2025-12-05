<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHub Profile Preview - T7</title>
    <style>
        body {
            background-color: #0d1117;
            color: #c9d1d9;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .readme-container {
            max-width: 880px;
            width: 100%;
            background-color: #0d1117;
            padding: 32px;
            border: 1px solid #30363d;
            border-radius: 6px;
            text-align: center;
            position: relative;
        }

        h1 {
            border-bottom: 1px solid #21262d;
            padding-bottom: 0.3em;
            color: #c9d1d9;
            font-size: 2em;
            margin-bottom: 10px;
        }

        h3 {
            color: #8b949e;
            font-size: 1.1em;
            margin-bottom: 30px;
        }

        img {
            max-width: 100%;
            height: auto;
            margin: 5px;
        }

        .section-title {
            color: #58a6ff;
            font-weight: bold;
            margin-top: 30px;
            margin-bottom: 15px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        /* AI Command Center Styles */
        .ai-panel {
            margin-top: 50px;
            padding: 20px;
            background-color: #161b22;
            border: 1px solid #30363d;
            border-radius: 8px;
            text-align: left;
        }

        .ai-header {
            font-size: 1.2em;
            color: #8A2BE2; /* Deep Purple */
            margin-bottom: 15px;
            border-bottom: 1px solid #30363d;
            padding-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .ai-controls {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .ai-btn {
            background-color: #238636;
            color: white;
            border: 1px solid rgba(240, 246, 252, 0.1);
            border-radius: 6px;
            padding: 8px 16px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .ai-btn:hover {
            background-color: #2ea043;
        }

        .ai-btn:disabled {
            background-color: #30363d;
            cursor: not-allowed;
            color: #8b949e;
        }

        .ai-output {
            margin-top: 15px;
            padding: 15px;
            background-color: #0d1117;
            border: 1px solid #30363d;
            border-radius: 6px;
            font-family: monospace;
            white-space: pre-wrap;
            color: #3fb950;
            min-height: 50px;
            display: none; /* Hidden by default */
        }

        .loading-spinner {
            display: inline-block;
            width: 12px;
            height: 12px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top-color: #fff;
            animation: spin 1s ease-in-out infinite;
            display: none;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }
    </style>
</head>
<body>

    <div class="readme-container">
        
        <!-- HEADER -->
        <h1 id="profile-name">👨‍💻 T7 | ARCHITECT OF INTELLIGENCE</h1>
        <h3 id="profile-role">CEO @ SPONSORACB | AI SPECIALIST</h3>

        <br>

        <!-- TROPHIES -->
        <div class="section-title">Achievements</div>
        <img src="https://github-profile-trophy.vercel.app/?username=MYT7&theme=onedark&no-frame=true&column=7&margin-w=15&margin-h=15&t=1" alt="Trophies" />

        <br><br>

        <!-- STATS -->
        <div class="section-title">Activity</div>
        <div>
            <img height="180" src="https://github-readme-stats.vercel.app/api?username=MYT7&show_icons=true&theme=onedark&include_all_commits=true&count_private=true&hide_border=true&t=1" alt="Stats" />
            <img height="180" src="https://github-readme-stats.vercel.app/api/top-langs/?username=MYT7&layout=compact&theme=onedark&hide_border=true&t=1" alt="Langs" />
        </div>

        <br><br>

        <!-- TECH STACK -->
        <div class="section-title">Tech Stack</div>
        <img src="https://skillicons.dev/icons?i=python,pytorch,tensorflow,linux,docker,aws,git,cpp,rust&theme=dark" alt="Icons" />
        <!-- Hidden data for AI to read -->
        <div id="tech-list" style="display:none;">Python, PyTorch, TensorFlow, Linux, Docker, AWS, Git, C++, Rust</div>

        <br><br>

        <!-- SNAKE -->
        <img src="https://github-readme-streak-stats.herokuapp.com/?user=MYT7&theme=onedark&hide_border=true&t=1" alt="Streak" />

        <br><br>

        <!-- FOOTER -->
        <div>
            <img src="https://img.shields.io/badge/WEBSITE-SPONSORACB-black?style=for-the-badge&logo=googlechrome&logoColor=white" />
            <img src="https://img.shields.io/badge/LINKEDIN-CONNECT-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" />
        </div>

        <!-- AI COMMAND CENTER -->
        <div class="ai-panel">
            <div class="ai-header">
                <span>🤖 Gemini AI Command Center</span>
            </div>
            <div class="ai-controls">
                <button class="ai-btn" id="btn-bio">
                    <span class="loading-spinner" id="spin-bio"></span>
                    Generate Epic Bio ✨
                </button>
                <button class="ai-btn" id="btn-ideas">
                    <span class="loading-spinner" id="spin-ideas"></span>
                    Project Ideas ✨
                </button>
            </div>
            <div id="ai-result" class="ai-output"></div>
        </div>

    </div>

    <!-- Gemini API Integration -->
    <script type="module">
        import { GoogleGenerativeAI } from "https://esm.run/@google/generative-ai";

        const apiKey = ""; // Runtime environment provides this
        let genAI;
        
        // Initialize Gemini
        try {
            genAI = new GoogleGenerativeAI(apiKey);
        } catch (e) {
            console.error("Gemini Init Error", e);
        }

        const btnBio = document.getElementById('btn-bio');
        const btnIdeas = document.getElementById('btn-ideas');
        const outputDiv = document.getElementById('ai-result');
        const spinBio = document.getElementById('spin-bio');
        const spinIdeas = document.getElementById('spin-ideas');

        const name = document.getElementById('profile-name').innerText;
        const role = document.getElementById('profile-role').innerText;
        const stack = document.getElementById('tech-list').innerText;

        async function generateContent(prompt, spinner, button) {
            if (!apiKey) {
                outputDiv.style.display = 'block';
                outputDiv.innerText = "Error: API Key is missing in the environment.";
                return;
            }

            // UI Loading State
            outputDiv.style.display = 'none';
            outputDiv.innerText = '';
            spinner.style.display = 'inline-block';
            button.disabled = true;

            try {
                const model = genAI.getGenerativeModel({ model: "gemini-2.5-flash-preview-09-2025" });
                const result = await model.generateContent(prompt);
                const response = await result.response;
                const text = response.text();

                // Display Result
                outputDiv.style.display = 'block';
                outputDiv.innerText = text;
            } catch (error) {
                outputDiv.style.display = 'block';
                outputDiv.innerText = "Error generating content: " + error.message;
            } finally {
                spinner.style.display = 'none';
                button.disabled = false;
            }
        }

        btnBio.addEventListener('click', () => {
            const prompt = `You are an expert developer branding assistant. 
            Generate a short, punchy, and professional GitHub profile bio (max 3 lines) for:
            Name: ${name}
            Role: ${role}
            Tech Stack: ${stack}
            Tone: Visionary, Hacker, CEO. 
            Use emojis sparsely but effectively.`;
            generateContent(prompt, spinBio, btnBio);
        });

        btnIdeas.addEventListener('click', () => {
            const prompt = `You are a Senior AI Architect. 
            Suggest 3 cutting-edge, complex AI project ideas that would look impressive on a GitHub portfolio for a developer with this stack: ${stack}.
            Format: bullet points. Keep descriptions brief (1 sentence each).`;
            generateContent(prompt, spinIdeas, btnIdeas);
        });

    </script>

</body>
</html>
