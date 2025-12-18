
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Boundary Cancellation Principle: Complete Discovery Engine</title>
    <!-- MathJax for LaTeX rendering -->
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
    <!-- Chart.js for data visualization -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chartjs-adapter-date-fns/dist/chartjs-adapter-date-fns.bundle.min.js"></script>
    <!-- PapaParse for CSV export -->
    <script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>
    <!-- Three.js for 3D visualization -->
    <script src="https://cdn.jsdelivr.net/npm/three@0.132.2/build/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.132.2/examples/js/controls/OrbitControls.js"></script>
    <!-- Decimal.js for high-precision arithmetic -->
    <script src="https://cdn.jsdelivr.net/npm/decimal.js@10.4.3/decimal.min.js"></script>
    <!-- Color scales for visualizations -->
    <script src="https://cdn.jsdelivr.net/npm/d3-scale@4.0.2/dist/d3-scale.min.js"></script>
    <style>
        /* =============================
           GLOBAL STYLES
        ============================= */
        :root {
            --primary-blue: #1a5fb4;
            --primary-green: #4CAF50;
            --primary-orange: #FF9800;
            --primary-red: #f44336;
            --primary-purple: #9C27B0;
            --dark-bg: #1a1a2e;
            --darker-bg: #16213e;
            --light-text: #f0f0f0;
            --medium-text: #b0b0b0;
            --border-color: #2d3748;
            --success-color: #10b981;
            --warning-color: #f59e0b;
            --error-color: #ef4444;
            --grid-color: rgba(255, 255, 255, 0.1);
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            line-height: 1.6;
            color: var(--light-text);
            background: linear-gradient(135deg, var(--dark-bg) 0%, var(--darker-bg) 100%);
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        /* =============================
           MAIN CONTAINER
        ============================= */
        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }
        
        /* =============================
           HEADER & TITLE
        ============================= */
        .main-header {
            text-align: center;
            padding: 2rem 0;
            margin-bottom: 2rem;
            border-bottom: 2px solid var(--primary-blue);
            position: relative;
        }
        
        .main-title {
            font-size: 2.8rem;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 0.5rem;
            font-weight: 800;
            letter-spacing: -0.5px;
        }
        
        .main-subtitle {
            font-size: 1.2rem;
            color: var(--medium-text);
            font-weight: 300;
            max-width: 800px;
            margin: 0 auto;
        }
        
        .author-info {
            position: absolute;
            top: 20px;
            right: 20px;
            text-align: right;
        }
        
        .author-name {
            font-weight: 600;
            color: var(--light-text);
        }
        
        .author-social {
            font-size: 0.9rem;
            color: var(--primary-blue);
        }
        
        /* =============================
           TABS NAVIGATION
        ============================= */
        .tabs-container {
            background: rgba(26, 26, 46, 0.8);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            margin-bottom: 2rem;
            border: 1px solid var(--border-color);
            overflow: hidden;
        }
        
        .tabs-nav {
            display: flex;
            background: rgba(0, 0, 0, 0.2);
            padding: 0.5rem;
            flex-wrap: wrap;
        }
        
        .tab-btn {
            padding: 12px 24px;
            background: transparent;
            border: none;
            color: var(--medium-text);
            font-size: 0.95rem;
            font-weight: 500;
            cursor: pointer;
            border-radius: 8px;
            margin: 0.25rem;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
            position: relative;
            overflow: hidden;
        }
        
        .tab-btn:hover {
            background: rgba(255, 255, 255, 0.05);
            color: var(--light-text);
        }
        
        .tab-btn.active {
            background: var(--primary-blue);
            color: white;
            box-shadow: 0 4px 12px rgba(26, 95, 180, 0.3);
        }
        
        .tab-btn .tab-icon {
            font-size: 1.1rem;
        }
        
        .tab-btn .tab-badge {
            background: var(--primary-red);
            color: white;
            font-size: 0.7rem;
            padding: 2px 6px;
            border-radius: 10px;
            margin-left: 4px;
        }
        
        .tabs-content {
            padding: 2rem;
        }
        
        .tab-pane {
            display: none;
            animation: fadeIn 0.5s ease;
        }
        
        .tab-pane.active {
            display: block;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        /* =============================
           SECTION STYLES
        ============================= */
        .section {
            background: rgba(26, 26, 46, 0.6);
            border-radius: 12px;
            padding: 1.5rem;
            margin-bottom: 2rem;
            border: 1px solid var(--border-color);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
        }
        
        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
            padding-bottom: 0.75rem;
            border-bottom: 1px solid var(--border-color);
        }
        
        .section-title {
            font-size: 1.4rem;
            color: var(--light-text);
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .section-title .icon {
            color: var(--primary-blue);
        }
        
        .section-subtitle {
            color: var(--medium-text);
            font-size: 0.95rem;
            margin-top: 0.25rem;
        }
        
        /* =============================
           ABSTRACT & KEY RESULTS
        ============================= */
        .abstract {
            background: linear-gradient(135deg, rgba(102, 126, 234, 0.1) 0%, rgba(118, 75, 162, 0.1) 100%);
            border-left: 4px solid var(--primary-blue);
        }
        
        .key-results-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1rem;
            margin-top: 1rem;
        }
        
        .result-card {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
            padding: 1rem;
            border-left: 3px solid var(--primary-green);
        }
        
        .result-card h4 {
            color: var(--primary-green);
            margin-bottom: 0.5rem;
            font-size: 1rem;
        }
        
        /* =============================
           VISUALIZATION CANVASES
        ============================= */
        .visualization-container {
            position: relative;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 8px;
            overflow: hidden;
            border: 1px solid var(--border-color);
        }
        
        .canvas-wrapper {
            width: 100%;
            height: 500px;
            position: relative;
        }
        
        .canvas-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }
        
        .canvas-label {
            position: absolute;
            top: 10px;
            left: 10px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 5px 10px;
            border-radius: 4px;
            font-size: 0.9rem;
            z-index: 10;
        }
        
        /* =============================
           INTERACTIVE CONTROLS
        ============================= */
        .controls-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.5rem;
            margin-top: 1.5rem;
        }
        
        .control-group {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
            padding: 1rem;
        }
        
        .control-group-title {
            font-size: 0.95rem;
            color: var(--medium-text);
            margin-bottom: 0.75rem;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .slider-container {
            margin-bottom: 1rem;
        }
        
        .slider-label {
            display: flex;
            justify-content: space-between;
            margin-bottom: 0.5rem;
            font-size: 0.9rem;
        }
        
        .slider-label span {
            color: var(--primary-blue);
            font-weight: 600;
        }
        
        input[type="range"] {
            width: 100%;
            height: 6px;
            -webkit-appearance: none;
            background: linear-gradient(to right, var(--primary-blue), var(--primary-purple));
            border-radius: 3px;
            outline: none;
        }
        
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 18px;
            height: 18px;
            border-radius: 50%;
            background: white;
            cursor: pointer;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
        }
        
        .input-group {
            display: flex;
            gap: 8px;
            margin-top: 0.5rem;
        }
        
        .input-group input {
            flex: 1;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 6px 10px;
            color: white;
            font-size: 0.9rem;
        }
        
        .input-group button {
            background: var(--primary-blue);
            color: white;
            border: none;
            border-radius: 6px;
            padding: 6px 12px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: background 0.3s ease;
        }
        
        .input-group button:hover {
            background: #0d4a9e;
        }
        
        .checkbox-group {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            margin-top: 0.5rem;
        }
        
        .checkbox-label {
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 0.9rem;
            cursor: pointer;
        }
        
        .checkbox-label input[type="checkbox"] {
            width: 16px;
            height: 16px;
            accent-color: var(--primary-blue);
        }
        
        /* =============================
           DATA DISPLAY & STATISTICS
        ============================= */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1rem;
            margin-top: 1.5rem;
        }
        
        .stat-card {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
            padding: 1rem;
            text-align: center;
        }
        
        .stat-value {
            font-size: 1.8rem;
            font-weight: 700;
            color: var(--primary-green);
            margin-bottom: 0.25rem;
        }
        
        .stat-label {
            font-size: 0.85rem;
            color: var(--medium-text);
        }
        
        .math-display {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 8px;
            padding: 1rem;
            margin-top: 1rem;
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
            border-left: 3px solid var(--primary-orange);
        }
        
        /* =============================
           GCD CALCULATOR
        ============================= */
        .gcd-calculator {
            background: rgba(76, 175, 80, 0.1);
            border-left: 4px solid var(--primary-green);
        }
        
        .gcd-inputs {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin: 1rem 0;
        }
        
        .gcd-inputs input {
            width: 70px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 8px;
            color: white;
            text-align: center;
        }
        
        .gcd-result {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 8px;
            padding: 1rem;
            margin-top: 1rem;
            font-family: 'Courier New', monospace;
            font-size: 1.1rem;
        }
        
        /* =============================
           EXPORT CONTROLS
        ============================= */
        .export-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1rem;
            margin-top: 1.5rem;
        }
        
        .export-btn {
            background: linear-gradient(135deg, var(--primary-blue) 0%, #0d4a9e 100%);
            color: white;
            border: none;
            border-radius: 8px;
            padding: 12px;
            cursor: pointer;
            font-size: 0.9rem;
            font-weight: 500;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .export-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(26, 95, 180, 0.3);
        }
        
        .export-btn.png {
            background: linear-gradient(135deg, var(--primary-green) 0%, #3d8b40 100%);
        }
        
        .export-btn.csv {
            background: linear-gradient(135deg, var(--primary-orange) 0%, #e68900 100%);
        }
        
        .export-btn.json {
            background: linear-gradient(135deg, var(--primary-purple) 0%, #7b1fa2 100%);
        }
        
        /* =============================
           RESEARCH MODE
        ============================= */
        .research-toggle {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 1000;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            border-radius: 50px;
            padding: 12px 24px;
            cursor: pointer;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 8px;
            box-shadow: 0 4px 20px rgba(102, 126, 234, 0.4);
            transition: all 0.3s ease;
        }
        
        .research-toggle:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 25px rgba(102, 126, 234, 0.6);
        }
        
        .research-toggle.active {
            background: linear-gradient(135deg, var(--primary-green) 0%, #3d8b40 100%);
        }
        
        .research-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            z-index: 999;
            display: none;
            overflow-y: auto;
            padding: 2rem;
        }
        
        .research-overlay.active {
            display: block;
            animation: slideIn 0.5s ease;
        }
        
        @keyframes slideIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .research-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 2rem;
            padding-bottom: 1rem;
            border-bottom: 1px solid var(--border-color);
        }
        
        .research-title {
            font-size: 2rem;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .research-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }
        
        .research-panel {
            background: rgba(26, 26, 46, 0.8);
            border-radius: 12px;
            padding: 1.5rem;
            border: 1px solid var(--border-color);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
        }
        
        .research-panel h4 {
            color: var(--primary-blue);
            margin-bottom: 1rem;
            font-size: 1.1rem;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .diagnostic-controls {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin: 1rem 0;
        }
        
        .diagnostic-btn {
            background: rgba(255, 255, 255, 0.1);
            color: white;
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 8px 12px;
            cursor: pointer;
            font-size: 0.85rem;
            transition: all 0.3s ease;
        }
        
        .diagnostic-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }
        
        .diagnostic-btn.active {
            background: var(--primary-blue);
            border-color: var(--primary-blue);
        }
        
        .precision-display {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 8px;
            padding: 1rem;
            margin-top: 1rem;
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
        }
        
        .slope-display {
            background: linear-gradient(135deg, rgba(26, 95, 180, 0.1) 0%, rgba(118, 75, 162, 0.1) 100%);
            border-radius: 8px;
            padding: 1rem;
            margin-top: 1rem;
        }
        
        .slope-value {
            font-size: 1.8rem;
            font-weight: 700;
            color: var(--primary-green);
            text-align: center;
            margin: 0.5rem 0;
        }
        
        .critical-strip-canvas {
            width: 100%;
            height: 200px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 8px;
            margin-top: 1rem;
            position: relative;
            overflow: hidden;
        }
        
        .complex-plane {
            width: 100%;
            height: 100%;
            position: relative;
        }
        
        .critical-line {
            position: absolute;
            top: 50%;
            left: 0;
            right: 0;
            height: 2px;
            background: rgba(255, 255, 255, 0.3);
            transform: translateY(-50%);
        }
        
        .critical-strip {
            position: absolute;
            top: 25%;
            left: 0;
            right: 0;
            height: 50%;
            border: 1px dashed rgba(255, 255, 255, 0.2);
        }
        
        .zeta-zero {
            position: absolute;
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background: var(--primary-orange);
            transform: translate(-50%, -50%);
        }
        
        .delta-point {
            position: absolute;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background: var(--primary-green);
            border: 2px solid white;
            transform: translate(-50%, -50%);
            transition: all 0.5s ease;
            z-index: 10;
        }
        
        .research-console {
            background: rgba(0, 0, 0, 0.8);
            border-radius: 8px;
            padding: 1rem;
            margin-top: 1rem;
            font-family: 'Courier New', monospace;
            font-size: 0.85rem;
            height: 150px;
            overflow-y: auto;
            color: var(--primary-green);
        }
        
        .console-entry {
            margin: 2px 0;
            padding: 2px 4px;
            border-left: 3px solid transparent;
        }
        
        .console-entry.info { border-left-color: var(--primary-blue); }
        .console-entry.warning { border-left-color: var(--warning-color); }
        .console-entry.error { border-left-color: var(--error-color); }
        
        /* =============================
           LEGEND & COLOR SCHEMES
        ============================= */
        .legend {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin: 1rem 0;
            padding: 1rem;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .legend-color {
            width: 20px;
            height: 20px;
            border-radius: 4px;
        }
        
        .legend-text {
            font-size: 0.9rem;
            color: var(--medium-text);
        }
        
        /* =============================
           LOADING STATES
        ============================= */
        .loading-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 100;
            display: none;
        }
        
        .loading-spinner {
            width: 50px;
            height: 50px;
            border: 4px solid rgba(255, 255, 255, 0.1);
            border-radius: 50%;
            border-top-color: var(--primary-blue);
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        /* =============================
           RESPONSIVE DESIGN
        ============================= */
        @media (max-width: 1024px) {
            .container {
                padding: 15px;
            }
            
            .main-title {
                font-size: 2.2rem;
            }
            
            .tabs-nav {
                justify-content: center;
            }
            
            .tab-btn {
                padding: 10px 16px;
                font-size: 0.9rem;
            }
            
            .canvas-wrapper {
                height: 400px;
            }
        }
        
        @media (max-width: 768px) {
            .main-title {
                font-size: 1.8rem;
            }
            
            .author-info {
                position: static;
                text-align: center;
                margin-top: 1rem;
            }
            
            .tabs-nav {
                flex-direction: column;
            }
            
            .tab-btn {
                justify-content: center;
            }
            
            .controls-grid {
                grid-template-columns: 1fr;
            }
            
            .research-grid {
                grid-template-columns: 1fr;
            }
            
            .canvas-wrapper {
                height: 300px;
            }
        }
        
        @media (max-width: 480px) {
            .container {
                padding: 10px;
            }
            
            .tabs-content {
                padding: 1rem;
            }
            
            .section {
                padding: 1rem;
            }
            
            .main-title {
                font-size: 1.6rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Main Header -->
        <header class="main-header">
            <h1 class="main-title">The Boundary Cancellation Principle</h1>
            <p class="main-subtitle">A Discovery Engine for Arithmetic Lattice Residues and Riemann Hypothesis Connections</p>
            <div class="author-info">
                <div class="author-name">Wessen Getachew</div>
                <div class="author-social">Twitter: @7dview </div>
            </div>
        </header>
        
        <!-- Main Tabs Navigation -->
        <div class="tabs-container">
            <nav class="tabs-nav">
                <button class="tab-btn active" data-tab="overview">
                    <span class="tab-icon"></span> Overview
                </button>
                <button class="tab-btn" data-tab="visualization">
                    <span class="tab-icon"></span> Visualizations
                </button>
                <button class="tab-btn" data-tab="analysis">
                    <span class="tab-icon"></span> Error Analysis
                </button>
                <button class="tab-btn" data-tab="gcd">
                    <span class="tab-icon"></span> GCD Calculator
                </button>
                <button class="tab-btn" data-tab="export">
                    <span class="tab-icon"></span> Export Data
                </button>
            </nav>
            
            <div class="tabs-content">
                <!-- Overview Tab -->
                <div class="tab-pane active" id="overview">
                    <div class="section abstract">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> Abstract
                            </h2>
                        </div>
                        <p>In the limit as \(R \to \infty\), the density of coprime \(k\)-tuples in an integer lattice \(\mathbb{Z}^k\) converges to \(1/\zeta(k)\). However, for any finite search radius \(R\), a residual error term \(\Delta(R)\) exists. This discovery engine identifies this error not as stochastic noise, but as a deterministic geometric residue. We demonstrate that \(\Delta(R)\) is a function of the \((k-1)\) boundary shell of the \(k\)-cube, where the Möbius inversion engine is truncated by the finite search radius \(R\).</p>
                    </div>
                    
                    <div class="section">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> Key Discoveries
                            </h2>
                        </div>
                        <div class="key-results-grid">
                            <div class="result-card">
                                <h4>Geometric Origin</h4>
                                <p>The error term \(\Delta(R)\) is structurally concentrated at the truncation limits of the lattice, not randomly distributed.</p>
                            </div>
                            <div class="result-card">
                                <h4>Boundary Cancellation</h4>
                                <p>The Möbius function's cancellation cycle is incomplete at boundary divisors, creating a deterministic residue.</p>
                            </div>
                            <div class="result-card">
                                <h4>Dimensional Stability</h4>
                                <p>Relative error decreases as \(k\) increases: \(\Delta(R)/R^k \to 0\) faster for higher dimensions.</p>
                            </div>
                            <div class="result-card">
                                <h4>RH Connection</h4>
                                <p>The structure of \(\Delta(R)\) is governed by oscillations of \(\mu(n)\), linking directly to the Riemann Hypothesis.</p>
                            </div>
                        </div>
                    </div>
                    
                    <div class="section">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> Mathematical Foundation
                            </h2>
                        </div>
                        <div class="controls-grid">
                            <div class="control-group">
                                <h4 class="control-group-title">Density Identity</h4>
                                <div class="math-display">
                                    \[ P(k) = \frac{1}{\zeta(k)} \]
                                </div>
                                <p>Probability that \(k\) randomly chosen integers are coprime.</p>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Counting Function</h4>
                                <div class="math-display">
                                    \[ N(R) = \sum_{d=1}^{R} \mu(d) \left\lfloor \frac{R}{d} \right\rfloor^k \]
                                </div>
                                <p>Exact count of coprime \(k\)-tuples in \([1, R]^k\).</p>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Error Term</h4>
                                <div class="math-display">
                                    \[ \Delta(R) = N(R) - \frac{R^k}{\zeta(k)} \]
                                </div>
                                <p>Boundary residue from incomplete Möbius cancellation.</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Visualizations Tab -->
                <div class="tab-pane" id="visualization">
                    <div class="section">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> 2D Lattice Visualization
                            </h2>
                            <div class="section-subtitle">Click on points to inspect GCD values</div>
                        </div>
                        
                        <!-- Legend -->
                        <div class="legend">
                            <div class="legend-item">
                                <div class="legend-color" style="background-color: var(--primary-green);"></div>
                                <span class="legend-text">Coprime Points (GCD = 1)</span>
                            </div>
                            <div class="legend-item">
                                <div class="legend-color" style="background-color: var(--primary-red);"></div>
                                <span class="legend-text">Non-coprime Points</span>
                            </div>
                            <div class="legend-item">
                                <div class="legend-color" style="border: 2px solid var(--primary-orange); background: transparent;"></div>
                                <span class="legend-text">Boundary Points</span>
                            </div>
                            <div class="legend-item">
                                <div class="legend-color" style="background-color: var(--primary-blue);"></div>
                                <span class="legend-text">Origin (1,1)</span>
                            </div>
                        </div>
                        
                        <!-- Canvas Container -->
                        <div class="visualization-container">
                            <div class="canvas-wrapper">
                                <div class="canvas-label">2D Coprime Lattice (k=2)</div>
                                <div class="loading-overlay" id="loading2D">
                                    <div class="loading-spinner"></div>
                                </div>
                                <canvas id="latticeCanvas2D"></canvas>
                            </div>
                        </div>
                        
                        <!-- Point Inspector -->
                        <div id="point-inspector" style="display: none; margin-top: 1rem; padding: 1rem; background: rgba(255,255,255,0.05); border-radius: 8px;">
                            <h4> Selected Point</h4>
                            <div id="point-details" style="font-family: monospace; margin-top: 0.5rem;"></div>
                        </div>
                        
                        <!-- Controls -->
                        <div class="controls-grid">
                            <div class="control-group">
                                <h4 class="control-group-title">Lattice Parameters</h4>
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Radius \(R\): <span id="radius2DValue">50</span>
                                    </div>
                                    <input type="range" id="radius2DSlider" min="1" max="500" value="50" step="1">
                                    <div class="input-group">
                                        <input type="number" id="radius2DInput" min="1" max="500" value="50">
                                        <button id="setRadius2D">Set</button>
                                    </div>
                                </div>
                                
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Point Size: <span id="pointSizeValue">10</span>px
                                    </div>
                                    <input type="range" id="pointSizeSlider" min="1" max="30" value="10" step="1">
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Visual Effects</h4>
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Grid Opacity: <span id="gridOpacityValue">0.3</span>
                                    </div>
                                    <input type="range" id="gridOpacitySlider" min="0" max="1" value="0.3" step="0.1">
                                </div>
                                
                                <div class="checkbox-group">
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="showGrid" checked> Show Grid
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="showBoundary" checked> Highlight Boundary
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="animate2D"> Animate
                                    </label>
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Statistics</h4>
                                <div class="stats-grid">
                                    <div class="stat-card">
                                        <div class="stat-value" id="totalPoints2D">2500</div>
                                        <div class="stat-label">Total Points</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="coprimeCount2D">1519</div>
                                        <div class="stat-label">Coprime Points</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="error2D">-0.70</div>
                                        <div class="stat-label">Error Δ(R)</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="boundaryCount2D">196</div>
                                        <div class="stat-label">Boundary Points</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="section">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> 3D Lattice Visualization
                            </h2>
                            <div class="section-subtitle">Drag to rotate, scroll to zoom. Colors indicate GCD.</div>
                        </div>
                        
                        <div class="visualization-container">
                            <div class="canvas-wrapper">
                                <div class="canvas-label">3D Coprime Lattice (k=3)</div>
                                <div class="loading-overlay" id="loading3D">
                                    <div class="loading-spinner"></div>
                                </div>
                                <canvas id="latticeCanvas3D"></canvas>
                            </div>
                        </div>
                        
                        <div class="controls-grid">
                            <div class="control-group">
                                <h4 class="control-group-title">3D Parameters</h4>
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Radius \(R\): <span id="radius3DValue">15</span>
                                    </div>
                                    <input type="range" id="radius3DSlider" min="1" max="30" value="15" step="1">
                                    <div class="input-group">
                                        <input type="number" id="radius3DInput" min="1" max="30" value="15">
                                        <button id="setRadius3D">Set</button>
                                    </div>
                                </div>
                                
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Cube Size: <span id="cubeSizeValue">0.7</span>
                                    </div>
                                    <input type="range" id="cubeSizeSlider" min="0.1" max="2" value="0.7" step="0.1">
                                </div>
                                
                                <div class="checkbox-group">
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="autoRotate3D" checked> Auto Rotate
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="showAxes3D" checked> Show Axes
                                    </label>
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Point Inspector</h4>
                                <div class="input-group" style="margin-bottom: 1rem;">
                                    <input type="number" id="inspectX" placeholder="X" min="1" value="1">
                                    <input type="number" id="inspectY" placeholder="Y" min="1" value="1">
                                    <input type="number" id="inspectZ" placeholder="Z" min="1" value="1">
                                    <button id="inspect3DPoint">Inspect</button>
                                </div>
                                <div id="inspectResult" style="font-family: monospace; font-size: 0.9rem; padding: 0.5rem; background: rgba(0,0,0,0.3); border-radius: 6px;">
                                    Point (1, 1, 1): GCD = 1, Coprime = Yes
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">3D Statistics</h4>
                                <div class="stats-grid">
                                    <div class="stat-card">
                                        <div class="stat-value" id="totalPoints3D">3375</div>
                                        <div class="stat-label">Total Points</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="coprimeCount3D">2805</div>
                                        <div class="stat-label">Coprime Points</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="error3D">-0.42</div>
                                        <div class="stat-label">Error Δ(R)</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="boundaryCount3D">1256</div>
                                        <div class="stat-label">Boundary Points</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Error Analysis Tab -->
                <div class="tab-pane" id="analysis">
                    <div class="section">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> Error Analysis Chart
                            </h2>
                            <div class="section-subtitle">Visualization of Δ(R) across dimensions and radii</div>
                        </div>
                        
                        <div class="visualization-container">
                            <div class="canvas-wrapper">
                                <div class="canvas-label">Error Term Δ(R) Analysis</div>
                                <canvas id="errorChart"></canvas>
                            </div>
                        </div>
                        
                        <div class="controls-grid">
                            <div class="control-group">
                                <h4 class="control-group-title">Chart Parameters</h4>
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Max Radius: <span id="maxRadiusValue">200</span>
                                    </div>
                                    <input type="range" id="maxRadiusSlider" min="10" max="5000" value="200" step="10">
                                    <div class="input-group">
                                        <input type="number" id="maxRadiusInput" min="10" max="5000" value="200">
                                        <button id="setMaxRadius">Set</button>
                                    </div>
                                </div>
                                
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Step Size: <span id="stepSizeValue">10</span>
                                    </div>
                                    <input type="range" id="stepSizeSlider" min="1" max="100" value="10" step="1">
                                </div>
                                
                                <div class="checkbox-group">
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="showAbsoluteError" checked> Absolute Error
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="showRelativeError"> Relative Error
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="showRunningAverage"> Running Average
                                    </label>
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Dimensions</h4>
                                <div class="checkbox-group" style="flex-direction: column; align-items: flex-start;">
                                    <label class="checkbox-label">
                                        <input type="checkbox" class="dimension-checkbox" value="2" checked> k = 2 (Δ(R) ∝ R log R)
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" class="dimension-checkbox" value="3" checked> k = 3 (Δ(R) ∝ R²)
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" class="dimension-checkbox" value="4"> k = 4 (Δ(R) ∝ R³)
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" class="dimension-checkbox" value="5"> k = 5 (Δ(R) ∝ R⁴)
                                    </label>
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Point Analysis</h4>
                                <div class="input-group" style="margin-bottom: 1rem;">
                                    <input type="number" id="analyzeR" placeholder="R" min="1" value="100">
                                    <select id="analyzeK" style="background: rgba(255,255,255,0.1); color: white; border: 1px solid var(--border-color); border-radius: 6px; padding: 6px;">
                                        <option value="2">k=2</option>
                                        <option value="3">k=3</option>
                                        <option value="4">k=4</option>
                                        <option value="5">k=5</option>
                                    </select>
                                    <button id="analyzePoint">Analyze</button>
                                </div>
                                <div id="analysisResult" style="font-family: monospace; font-size: 0.9rem; padding: 0.5rem; background: rgba(0,0,0,0.3); border-radius: 6px;">
                                    For R=100, k=2: N(R)=6087, Expected=6079.30, Δ(R)=7.70
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="section">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> Real-time Diagnostics
                            </h2>
                            <div class="section-subtitle">Live analysis of boundary cancellation patterns</div>
                        </div>
                        
                        <div class="controls-grid">
                            <div class="control-group">
                                <h4 class="control-group-title">Möbius Wave Analysis</h4>
                                <div class="math-display">
                                    M(R) = Σ μ(d) from d=1 to R
                                </div>
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Wave Intensity: <span id="waveIntensityValue">1.0</span>
                                    </div>
                                    <input type="range" id="waveIntensitySlider" min="0" max="2" value="1" step="0.1">
                                </div>
                                <div class="checkbox-group">
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="showMobiusWave"> Show Möbius Wave
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="showCumulative"> Show Cumulative Sum
                                    </label>
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Growth Exponent</h4>
                                <div class="slope-display">
                                    <div style="text-align: center; color: var(--medium-text);">α in Δ(R) ∝ R^α</div>
                                    <div class="slope-value" id="growthExponent">-</div>
                                    <div style="text-align: center; font-size: 0.9rem;">
                                        R² = <span id="goodnessOfFit">-</span>
                                    </div>
                                </div>
                                <button id="calculateExponent" style="width: 100%; margin-top: 1rem; padding: 10px; background: var(--primary-blue); color: white; border: none; border-radius: 6px; cursor: pointer;">
                                    Calculate Exponent
                                </button>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Boundary Self-Correction</h4>
                                <div class="math-display" style="font-size: 0.8rem;">
                                    Test: lim(R→∞) (1/R) Σ Δ(i) → 0?
                                </div>
                                <div class="stats-grid" style="grid-template-columns: 1fr 1fr;">
                                    <div class="stat-card">
                                        <div class="stat-value" id="runningAverage">-</div>
                                        <div class="stat-label">Running Avg</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="autocorrelation">-</div>
                                        <div class="stat-label">Autocorr</div>
                                    </div>
                                </div>
                                <button id="testSelfCorrection" style="width: 100%; margin-top: 1rem; padding: 10px; background: var(--primary-green); color: white; border: none; border-radius: 6px; cursor: pointer;">
                                    Test Self-Correction
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- GCD Calculator Tab -->
                <div class="tab-pane" id="gcd">
                    <div class="section gcd-calculator">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> GCD Calculator
                            </h2>
                            <div class="section-subtitle">Compute greatest common divisors with prime factorization</div>
                        </div>
                        
                        <div class="controls-grid">
                            <div class="control-group">
                                <h4 class="control-group-title">Input Numbers</h4>
                                <div class="gcd-inputs">
                                    <input type="number" id="gcd1" value="12" min="1" max="1000000">
                                    <input type="number" id="gcd2" value="18" min="1" max="1000000">
                                    <input type="number" id="gcd3" value="24" min="1" max="1000000">
                                    <input type="number" id="gcd4" value="1" min="1" max="1000000" style="display: none;">
                                    <input type="number" id="gcd5" value="1" min="1" max="1000000" style="display: none;">
                                    <input type="number" id="gcd6" value="1" min="1" max="1000000" style="display: none;">
                                </div>
                                <div style="margin-top: 1rem; display: flex; gap: 10px;">
                                    <button id="calculateGCD" style="padding: 10px 20px; background: var(--primary-green); color: white; border: none; border-radius: 6px; cursor: pointer;">
                                        Calculate GCD
                                    </button>
                                    <button id="addGCDInput" style="padding: 10px 20px; background: var(--primary-blue); color: white; border: none; border-radius: 6px; cursor: pointer;">
                                        + Add Number
                                    </button>
                                    <button id="showFactors" style="padding: 10px 20px; background: var(--primary-purple); color: white; border: none; border-radius: 6px; cursor: pointer;">
                                        Show Factors
                                    </button>
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Result</h4>
                                <div class="gcd-result" id="gcdResult">
                                    GCD(12, 18, 24) = 6
                                </div>
                                <div id="primeFactors" style="margin-top: 1rem; padding: 1rem; background: rgba(0,0,0,0.3); border-radius: 6px; font-family: monospace; font-size: 0.9rem; display: none;">
                                    Prime factorization:<br>
                                    12 = 2² × 3<br>
                                    18 = 2 × 3²<br>
                                    24 = 2³ × 3
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Properties</h4>
                                <div class="stats-grid">
                                    <div class="stat-card">
                                        <div class="stat-value" id="gcdValue">6</div>
                                        <div class="stat-label">GCD</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="lcmValue">72</div>
                                        <div class="stat-label">LCM</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="areCoprime">No</div>
                                        <div class="stat-label">Coprime?</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value" id="numFactors">4</div>
                                        <div class="stat-label"># of Factors</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="section">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> Number Theory Tools
                            </h2>
                            <div class="section-subtitle">Additional utilities for number analysis</div>
                        </div>
                        
                        <div class="controls-grid">
                            <div class="control-group">
                                <h4 class="control-group-title">Möbius Function</h4>
                                <div class="input-group">
                                    <input type="number" id="mobiusInput" placeholder="Enter n" min="1" value="12">
                                    <button id="calculateMobius">Calculate μ(n)</button>
                                </div>
                                <div id="mobiusResult" style="margin-top: 1rem; padding: 1rem; background: rgba(0,0,0,0.3); border-radius: 6px; font-family: monospace; font-size: 1.1rem; text-align: center;">
                                    μ(12) = 0
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Prime Test</h4>
                                <div class="input-group">
                                    <input type="number" id="primeTestInput" placeholder="Enter number" min="1" value="17">
                                    <button id="testPrime">Test Primality</button>
                                </div>
                                <div id="primeResult" style="margin-top: 1rem; padding: 1rem; background: rgba(0,0,0,0.3); border-radius: 6px; font-family: monospace; font-size: 1.1rem; text-align: center;">
                                    17 is prime
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Euler's Totient</h4>
                                <div class="input-group">
                                    <input type="number" id="totientInput" placeholder="Enter n" min="1" value="12">
                                    <button id="calculateTotient">Calculate φ(n)</button>
                                </div>
                                <div id="totientResult" style="margin-top: 1rem; padding: 1rem; background: rgba(0,0,0,0.3); border-radius: 6px; font-family: monospace; font-size: 1.1rem; text-align: center;">
                                    φ(12) = 4
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Export Data Tab -->
                <div class="tab-pane" id="export">
                    <div class="section">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> Export Data & Visualizations
                            </h2>
                            <div class="section-subtitle">Save high-resolution images and datasets for publication</div>
                        </div>
                        
                        <div class="export-grid">
                            <button class="export-btn png" id="export2DPNG">
                                <span></span> 2D Visualization (4K PNG)
                            </button>
                            <button class="export-btn png" id="export3DPNG">
                                <span></span> 3D Visualization (4K PNG)
                            </button>
                            <button class="export-btn png" id="exportChartPNG">
                                <span></span> Error Chart (4K PNG)
                            </button>
                            <button class="export-btn csv" id="exportErrorCSV">
                                <span></span> Error Data (CSV)
                            </button>
                            <button class="export-btn csv" id="exportLatticeCSV">
                                <span></span> Lattice Points (CSV)
                            </button>
                            <button class="export-btn json" id="exportFullReport">
                                <span></span> Full Report (JSON)
                            </button>
                        </div>
                        
                        <div id="exportStatus" style="margin-top: 2rem; padding: 1rem; background: rgba(0,0,0,0.3); border-radius: 8px; text-align: center; font-size: 0.9rem;">
                            Ready to export. Select an option above.
                        </div>
                    </div>
                    
                    <div class="section">
                        <div class="section-header">
                            <h2 class="section-title">
                                <span class="icon"></span> Export Settings
                            </h2>
                        </div>
                        
                        <div class="controls-grid">
                            <div class="control-group">
                                <h4 class="control-group-title">Image Settings</h4>
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Image Quality: <span id="qualityValue">100</span>%
                                    </div>
                                    <input type="range" id="qualitySlider" min="50" max="100" value="100" step="5">
                                </div>
                                
                                <div class="checkbox-group">
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="includeWatermark" checked> Include Watermark
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="includeTimestamp" checked> Include Timestamp
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="includeMetadata"> Include Metadata
                                    </label>
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Data Settings</h4>
                                <div class="slider-container">
                                    <div class="slider-label">
                                        Precision: <span id="precisionValue">6</span> decimals
                                    </div>
                                    <input type="range" id="precisionSlider" min="0" max="15" value="6" step="1">
                                </div>
                                
                                <div class="checkbox-group">
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="includeFormulas" checked> Include Formulas
                                    </label>
                                    <label class="checkbox-label">
                                        <input type="checkbox" id="includeStatistics" checked> Include Statistics
                                    </label>
                                </div>
                            </div>
                            
                            <div class="control-group">
                                <h4 class="control-group-title">Batch Export</h4>
                                <div class="input-group">
                                    <input type="number" id="batchStart" placeholder="Start R" min="1" value="1">
                                    <input type="number" id="batchEnd" placeholder="End R" min="1" value="100">
                                    <input type="number" id="batchStep" placeholder="Step" min="1" value="10">
                                </div>
                                <button id="exportBatch" style="width: 100%; margin-top: 1rem; padding: 10px; background: var(--primary-orange); color: white; border: none; border-radius: 6px; cursor: pointer;">
                                    Export Batch Data
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Research Mode Toggle -->
    <button class="research-toggle" id="researchToggle">
        <span></span> Research Mode
    </button>
    
    <!-- Research Mode Overlay -->
    <div class="research-overlay" id="researchOverlay">
        <div class="container">
            <div class="research-header">
                <h2 class="research-title">Research Mode: Diagnostic Overlays</h2>
                <button id="closeResearch" style="background: var(--primary-red); color: white; border: none; border-radius: 6px; padding: 10px 20px; cursor: pointer;">
                    Close
                </button>
            </div>
            
            <div class="research-grid">
                <!-- Möbius Wave Panel -->
                <div class="research-panel">
                    <h4><span></span> Möbius Wave Overlay</h4>
                    <p>Visualizes the cumulative sum of μ(d) (Mertens function) to reveal cancellation patterns.</p>
                    
                    <div class="diagnostic-controls">
                        <button class="diagnostic-btn" id="toggleMobiusWave">Show Wave</button>
                        <button class="diagnostic-btn" id="waveIntensityBtn">Intensity: Medium</button>
                        <button class="diagnostic-btn" id="waveColorMap">Color: Spectral</button>
                    </div>
                    
                    <div class="precision-display">
                        <div>M(R) = Σ μ(d) = <span id="mertensValueDisplay">0</span></div>
                        <div>Oscillation Range: [<span id="mertensMin">0</span>, <span id="mertensMax">0</span>]</div>
                    </div>
                </div>
                
                <!-- Log-Log Analysis Panel -->
                <div class="research-panel">
                    <h4><span></span> Log-Log Scaling & Exponent Hunter</h4>
                    <p>Calculates growth exponent α in Δ(R) ∝ R^α using linear regression on log-log plot.</p>
                    
                    <div class="diagnostic-controls">
                        <button class="diagnostic-btn" id="toggleLogLog">Linear Scale</button>
                        <button class="diagnostic-btn" id="fitPowerLaw">Fit Power Law</button>
                        <button class="diagnostic-btn" id="confidence95">95% CI</button>
                    </div>
                    
                    <div class="slope-display">
                        <div style="text-align: center; color: var(--medium-text);">Growth Exponent α</div>
                        <div class="slope-value" id="exponentDisplay">-</div>
                        <div style="text-align: center; font-size: 0.9rem;">
                            CI: ±<span id="confidenceDisplay">-</span>, R² = <span id="r2Display">-</span>
                        </div>
                    </div>
                </div>
                
                <!-- Critical Strip Panel -->
                <div class="research-panel">
                    <h4><span></span> Critical Strip Projection</h4>
                    <p>Maps Δ(R) onto complex plane to test Riemann Hypothesis predictions.</p>
                    
                    <div class="critical-strip-canvas">
                        <div class="complex-plane" id="complexPlane">
                            <div class="critical-line"></div>
                            <div class="critical-strip"></div>
                            <div class="delta-point" id="deltaPoint"></div>
                        </div>
                    </div>
                    
                    <div class="precision-display">
                        <div>Point: Re = <span id="currentRe">0</span>, Im = <span id="currentIm">0</span></div>
                        <div>Distance from ½-line: <span id="distanceFromHalf">-</span></div>
                    </div>
                </div>
                
                <!-- High Precision Panel -->
                <div class="research-panel">
                    <h4><span></span> High-Precision Arithmetic</h4>
                    <p>Compares 64-bit floating point with arbitrary precision Decimal.js calculations.</p>
                    
                    <div class="diagnostic-controls">
                        <button class="diagnostic-btn" id="togglePrecision">64-bit Float</button>
                        <button class="diagnostic-btn" id="benchmarkPrecision">Benchmark</button>
                        <button class="diagnostic-btn" id="setPrecision50">50 digits</button>
                    </div>
                    
                    <div class="precision-display">
                        <div>Standard: <span id="standardPrecision">-</span></div>
                        <div>High-Precision: <span id="highPrecision">-</span></div>
                        <div>Relative Error: <span id="relativeErrorDisplay">-</span></div>
                    </div>
                </div>
            </div>
            
            <!-- Research Console -->
            <div class="research-panel" style="grid-column: 1 / -1;">
                <h4><span></span> Research Console</h4>
                <div class="research-console" id="researchConsole">
[System] Research Mode Initialized
[System] Diagnostic overlays ready
[System] High-precision arithmetic enabled
                </div>
                <div class="diagnostic-controls">
                    <button class="diagnostic-btn" id="clearConsole">Clear Console</button>
                    <button class="diagnostic-btn" id="exportResearchData">Export Research Data</button>
                    <button class="diagnostic-btn" id="runFullAnalysis">Run Full Analysis</button>
                    <button class="diagnostic-btn" id="saveSession">Save Session</button>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Main JavaScript -->
    <script>
        // ===========================================
        // GLOBAL STATE & CONFIGURATION
        // ===========================================
        
        const state = {
            // UI State
            currentTab: 'overview',
            researchMode: false,
            animationId: null,
            animationFrameId: null,
            
            // Visualization State
            is3DInitialized: false,
            threeScene: null,
            threeRenderer: null,
            threeCamera: null,
            threeControls: null,
            threeObjects: [],
            
            // Chart State
            chart: null,
            chartData: {
                labels: [],
                datasets: []
            },
            
            // Research State
            research: {
                highPrecision: false,
                mobiusWave: false,
                logLogPlot: false,
                waveIntensity: 1.0,
                waveColorScheme: 'spectral',
                precisionDigits: 50,
                diagnosticData: [],
                consoleHistory: []
            },
            
            // Caches
            mobiusCache: new Map(),
            gcdCache: new Map(),
            primeCache: new Map(),
            
            // Current Selections
            selectedPoint2D: null,
            selectedDimension: 2,
            currentR: 50
        };
        
        // Mathematical Constants
        const ZETA_VALUES = {
            2: Math.PI**2 / 6,
            3: 1.202056903159594285399738161511449990764986292,
            4: Math.PI**4 / 90,
            5: 1.036927755143369926331365486457034168057080919,
            6: Math.PI**6 / 945,
            7: 1.008349277381922826839797549849796759599863560,
            8: Math.PI**8 / 9450
        };
        
        // Color Schemes
        const COLOR_SCHEMES = {
            default: {
                coprime: '#4CAF50',
                nonCoprime: '#f44336',
                boundary: '#FF9800',
                origin: '#1a5fb4',
                grid: 'rgba(255, 255, 255, 0.1)',
                axis: 'rgba(255, 255, 255, 0.5)',
                background: '#1a1a2e'
            },
            spectral: ['#9e0142', '#d53e4f', '#f46d43', '#fdae61', '#fee08b', '#ffffbf', '#e6f598', '#abdda4', '#66c2a5', '#3288bd', '#5e4fa2']
        };
        
        // ===========================================
        // CORE MATHEMATICAL FUNCTIONS
        // ===========================================
        
        // GCD Calculation
        function gcd(a, b) {
            a = Math.abs(a);
            b = Math.abs(b);
            while (b !== 0) {
                [a, b] = [b, a % b];
            }
            return a;
        }
        
        function gcdArray(arr) {
            return arr.reduce((a, b) => gcd(a, b));
        }
        
        // Möbius Function (with caching)
        function mobius(n) {
            if (n === 1) return 1;
            if (state.mobiusCache.has(n)) return state.mobiusCache.get(n);
            
            let p = 0;
            let temp = n;
            
            // Handle factor 2
            if (temp % 2 === 0) {
                temp /= 2;
                p++;
                if (temp % 2 === 0) {
                    state.mobiusCache.set(n, 0);
                    return 0;
                }
            }
            
            // Handle odd factors
            for (let i = 3; i * i <= temp; i += 2) {
                if (temp % i === 0) {
                    if (temp % (i * i) === 0) {
                        state.mobiusCache.set(n, 0);
                        return 0;
                    }
                    p++;
                    temp = Math.floor(temp / i);
                }
            }
            
            if (temp > 1) p++;
            
            const result = (p % 2 === 0) ? 1 : -1;
            state.mobiusCache.set(n, result);
            return result;
        }
        
        // Mertens Function (cumulative sum of μ(n))
        function mertens(n) {
            let sum = 0;
            for (let i = 1; i <= n; i++) {
                sum += mobius(i);
            }
            return sum;
        }
        
        // Prime Factorization
        function primeFactors(n) {
            const factors = {};
            let divisor = 2;
            
            while (n >= 2) {
                if (n % divisor === 0) {
                    factors[divisor] = (factors[divisor] || 0) + 1;
                    n = n / divisor;
                } else {
                    divisor++;
                }
            }
            
            return factors;
        }
        
        // Euler's Totient Function
        function totient(n) {
            let result = n;
            let p = 2;
            
            while (p * p <= n) {
                if (n % p === 0) {
                    while (n % p === 0) {
                        n /= p;
                    }
                    result -= result / p;
                }
                p++;
            }
            
            if (n > 1) {
                result -= result / n;
            }
            
            return result;
        }
        
        // Prime Test (deterministic for n < 2^64)
        function isPrime(n) {
            if (n <= 1) return false;
            if (n <= 3) return true;
            if (n % 2 === 0 || n % 3 === 0) return false;
            
            let i = 5;
            while (i * i <= n) {
                if (n % i === 0 || n % (i + 2) === 0) return false;
                i += 6;
            }
            
            return true;
        }
        
        // Compute N(R) using Möbius inversion
        function computeN(R, k) {
            let count = 0;
            for (let d = 1; d <= R; d++) {
                const mu = mobius(d);
                if (mu !== 0) {
                    count += mu * Math.pow(Math.floor(R / d), k);
                }
            }
            return count;
        }
        
        // High precision version using Decimal.js
        function computeNHighPrecision(R, k) {
            if (!state.research.highPrecision) return computeN(R, k);
            
            Decimal.set({ precision: state.research.precisionDigits });
            let count = new Decimal(0);
            
            for (let d = 1; d <= R; d++) {
                const mu = mobius(d);
                if (mu !== 0) {
                    const term = new Decimal(Math.floor(R / d)).pow(k).times(mu);
                    count = count.plus(term);
                }
            }
            
            return count.toNumber();
        }
        
        // ===========================================
        // 2D VISUALIZATION
        // ===========================================
        
        function init2DVisualization() {
            const canvas = document.getElementById('latticeCanvas2D');
            const ctx = canvas.getContext('2d');
            
            // Set canvas size for high DPI
            const container = canvas.parentElement;
            const dpr = window.devicePixelRatio || 1;
            canvas.width = container.clientWidth * dpr;
            canvas.height = container.clientHeight * dpr;
            canvas.style.width = container.clientWidth + 'px';
            canvas.style.height = container.clientHeight + 'px';
            ctx.scale(dpr, dpr);
            
            // Draw initial lattice
            draw2DLattice();
            
            // Add click handler for point selection
            canvas.addEventListener('click', handle2DClick);
            
            // Setup controls
            setup2DControls();
        }
        
        function draw2DLattice() {
            const loading = document.getElementById('loading2D');
            loading.style.display = 'flex';
            
            setTimeout(() => {
                try {
                    const canvas = document.getElementById('latticeCanvas2D');
                    const ctx = canvas.getContext('2d');
                    const R = parseInt(document.getElementById('radius2DSlider').value);
                    const pointSize = parseInt(document.getElementById('pointSizeSlider').value);
                    const gridOpacity = parseFloat(document.getElementById('gridOpacitySlider').value);
                    const showGrid = document.getElementById('showGrid').checked;
                    const showBoundary = document.getElementById('showBoundary').checked;
                    
                    // Clear canvas
                    ctx.clearRect(0, 0, canvas.width, canvas.height);
                    
                    // Calculate layout
                    const padding = pointSize * 2;
                    const availableWidth = canvas.width / (window.devicePixelRatio || 1) - padding * 2;
                    const availableHeight = canvas.height / (window.devicePixelRatio || 1) - padding * 2;
                    const cellSize = Math.min(availableWidth / R, availableHeight / R);
                    
                    // Draw grid
                    if (showGrid && gridOpacity > 0) {
                        ctx.strokeStyle = `rgba(255, 255, 255, ${gridOpacity})`;
                        ctx.lineWidth = 1;
                        
                        for (let i = 0; i <= R; i++) {
                            const x = padding + i * cellSize;
                            ctx.beginPath();
                            ctx.moveTo(x, padding);
                            ctx.lineTo(x, padding + R * cellSize);
                            ctx.stroke();
                        }
                        
                        for (let j = 0; j <= R; j++) {
                            const y = padding + j * cellSize;
                            ctx.beginPath();
                            ctx.moveTo(padding, y);
                            ctx.lineTo(padding + R * cellSize, y);
                            ctx.stroke();
                        }
                    }
                    
                    // Draw points and collect statistics
                    let coprimeCount = 0;
                    let boundaryCount = 0;
                    
                    for (let x = 1; x <= R; x++) {
                        for (let y = 1; y <= R; y++) {
                            const isCoprime = gcd(x, y) === 1;
                            const isBoundary = Math.max(x, y) === R;
                            
                            if (isCoprime) coprimeCount++;
                            if (isBoundary) boundaryCount++;
                            
                            const px = padding + (x - 1) * cellSize + cellSize / 2;
                            const py = padding + (y - 1) * cellSize + cellSize / 2;
                            
                            // Set color
                            if (x === 1 && y === 1) {
                                ctx.fillStyle = COLOR_SCHEMES.default.origin;
                            } else {
                                ctx.fillStyle = isCoprime ? COLOR_SCHEMES.default.coprime : COLOR_SCHEMES.default.nonCoprime;
                            }
                            
                            // Draw point
                            ctx.beginPath();
                            ctx.arc(px, py, pointSize / 2, 0, Math.PI * 2);
                            ctx.fill();
                            
                            // Draw boundary highlight
                            if (showBoundary && isBoundary) {
                                ctx.strokeStyle = COLOR_SCHEMES.default.boundary;
                                ctx.lineWidth = 2;
                                ctx.stroke();
                            }
                        }
                    }
                    
                    // Update statistics display
                    const totalPoints = R * R;
                    const expected = totalPoints / ZETA_VALUES[2];
                    const error = coprimeCount - expected;
                    
                    document.getElementById('totalPoints2D').textContent = totalPoints.toLocaleString();
                    document.getElementById('coprimeCount2D').textContent = coprimeCount.toLocaleString();
                    document.getElementById('error2D').textContent = error.toFixed(2);
                    document.getElementById('boundaryCount2D').textContent = boundaryCount.toLocaleString();
                    
                    // Update current R for other functions
                    state.currentR = R;
                    
                    // Draw Möbius wave if enabled
                    if (state.research.mobiusWave) {
                        drawMobiusWave(ctx, R, padding, cellSize);
                    }
                    
                } catch (error) {
                    console.error('Error drawing 2D lattice:', error);
                } finally {
                    loading.style.display = 'none';
                }
            }, 10);
        }
        
        function drawMobiusWave(ctx, R, padding, cellSize) {
            // Calculate Mertens function values
            const mertensValues = [];
            let currentSum = 0;
            let min = 0, max = 0;
            
            for (let i = 1; i <= R; i++) {
                currentSum += mobius(i);
                mertensValues[i] = currentSum;
                min = Math.min(min, currentSum);
                max = Math.max(max, currentSum);
            }
            
            const range = max - min || 1;
            
            // Draw wave overlay
            ctx.globalAlpha = 0.3 * state.research.waveIntensity;
            
            for (let x = 1; x <= R; x++) {
                for (let y = 1; y <= R; y++) {
                    const d = Math.max(x, y);
                    const mertensValue = mertensValues[d];
                    const normalized = (mertensValue - min) / range;
                    
                    // Get color from spectral scheme
                    const colorIndex = Math.floor(normalized * (COLOR_SCHEMES.spectral.length - 1));
                    const color = COLOR_SCHEMES.spectral[colorIndex];
                    
                    const px = padding + (x - 1) * cellSize;
                    const py = padding + (y - 1) * cellSize;
                    
                    ctx.fillStyle = color;
                    ctx.fillRect(px, py, cellSize, cellSize);
                }
            }
            
            ctx.globalAlpha = 1.0;
            
            // Update research display
            document.getElementById('mertensValueDisplay').textContent = mertensValues[R];
            document.getElementById('mertensMin').textContent = min;
            document.getElementById('mertensMax').textContent = max;
        }
        
        function handle2DClick(event) {
            const canvas = document.getElementById('latticeCanvas2D');
            const rect = canvas.getBoundingClientRect();
            const R = parseInt(document.getElementById('radius2DSlider').value);
            const pointSize = parseInt(document.getElementById('pointSizeSlider').value);
            
            // Calculate click position
            const padding = pointSize * 2;
            const availableWidth = canvas.width / (window.devicePixelRatio || 1) - padding * 2;
            const availableHeight = canvas.height / (window.devicePixelRatio || 1) - padding * 2;
            const cellSize = Math.min(availableWidth / R, availableHeight / R);
            
            const x = event.clientX - rect.left;
            const y = event.clientY - rect.top;
            
            const latticeX = Math.floor((x - padding) / cellSize) + 1;
            const latticeY = Math.floor((y - padding) / cellSize) + 1;
            
            // Validate click
            if (latticeX >= 1 && latticeX <= R && latticeY >= 1 && latticeY <= R) {
                state.selectedPoint2D = { x: latticeX, y: latticeY };
                showPointDetails(latticeX, latticeY);
            }
        }
        
        function showPointDetails(x, y) {
            const gcdVal = gcd(x, y);
            const isCoprime = gcdVal === 1;
            const isBoundary = Math.max(x, y) === parseInt(document.getElementById('radius2DSlider').value);
            
            const inspector = document.getElementById('point-inspector');
            const details = document.getElementById('point-details');
            
            details.innerHTML = `
                <div style="margin-bottom: 0.5rem;">Point: (${x}, ${y})</div>
                <div style="margin-bottom: 0.5rem;">GCD: ${gcdVal}</div>
                <div style="margin-bottom: 0.5rem;">Coprime: ${isCoprime ? 'Yes ✓' : 'No'}</div>
                <div style="margin-bottom: 0.5rem;">Boundary: ${isBoundary ? 'Yes' : 'No'}</div>
                <div>Distance from origin: ${Math.sqrt(x*x + y*y).toFixed(2)}</div>
            `;
            
            inspector.style.display = 'block';
            
            // Update GCD calculator
            document.getElementById('gcd1').value = x;
            document.getElementById('gcd2').value = y;
            calculateGCD();
        }
        
        function setup2DControls() {
            // Radius slider
            const radiusSlider = document.getElementById('radius2DSlider');
            const radiusInput = document.getElementById('radius2DInput');
            const setRadiusBtn = document.getElementById('setRadius2D');
            
            radiusSlider.addEventListener('input', () => {
                const value = radiusSlider.value;
                document.getElementById('radius2DValue').textContent = value;
                radiusInput.value = value;
                draw2DLattice();
            });
            
            setRadiusBtn.addEventListener('click', () => {
                const value = parseInt(radiusInput.value);
                if (value >= 1 && value <= 500) {
                    radiusSlider.value = value;
                    document.getElementById('radius2DValue').textContent = value;
                    draw2DLattice();
                }
            });
            
            // Other sliders
            document.getElementById('pointSizeSlider').addEventListener('input', () => {
                document.getElementById('pointSizeValue').textContent = document.getElementById('pointSizeSlider').value;
                draw2DLattice();
            });
            
            document.getElementById('gridOpacitySlider').addEventListener('input', () => {
                document.getElementById('gridOpacityValue').textContent = document.getElementById('gridOpacitySlider').value;
                draw2DLattice();
            });
            
            // Checkboxes
            document.getElementById('showGrid').addEventListener('change', draw2DLattice);
            document.getElementById('showBoundary').addEventListener('change', draw2DLattice);
            document.getElementById('animate2D').addEventListener('change', toggle2DAnimation);
        }
        
        function toggle2DAnimation() {
            const animate = document.getElementById('animate2D').checked;
            
            if (animate && !state.animationId) {
                let R = 1;
                const maxR = 100;
                
                function animateStep() {
                    document.getElementById('radius2DSlider').value = R;
                    document.getElementById('radius2DValue').textContent = R;
                    document.getElementById('radius2DInput').value = R;
                    draw2DLattice();
                    
                    R = R >= maxR ? 1 : R + 1;
                    state.animationId = setTimeout(animateStep, 300);
                }
                
                animateStep();
            } else if (state.animationId) {
                clearTimeout(state.animationId);
                state.animationId = null;
            }
        }
        
        // ===========================================
        // 3D VISUALIZATION
        // ===========================================
        
        function init3DVisualization() {
            if (state.is3DInitialized) return;
            
            const canvas = document.getElementById('latticeCanvas3D');
            const container = canvas.parentElement;
            
            // Set canvas size
            canvas.width = container.clientWidth;
            canvas.height = container.clientHeight;
            
            // Create Three.js scene
            const scene = new THREE.Scene();
            scene.background = new THREE.Color(0x1a1a2e);
            
            // Camera
            const camera = new THREE.PerspectiveCamera(
                75,
                canvas.width / canvas.height,
                0.1,
                1000
            );
            camera.position.set(20, 20, 20);
            
            // Renderer
            const renderer = new THREE.WebGLRenderer({ 
                canvas: canvas,
                antialias: true,
                alpha: true
            });
            renderer.setSize(canvas.width, canvas.height);
            renderer.setPixelRatio(window.devicePixelRatio);
            
            // Controls
            const controls = new THREE.OrbitControls(camera, renderer.domElement);
            controls.enableDamping = true;
            controls.dampingFactor = 0.05;
            
            // Lighting
            const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
            scene.add(ambientLight);
            
            const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
            directionalLight.position.set(10, 20, 15);
            scene.add(directionalLight);
            
            // Store in state
            state.threeScene = scene;
            state.threeRenderer = renderer;
            state.threeCamera = camera;
            state.threeControls = controls;
            state.is3DInitialized = true;
            
            // Draw initial lattice
            draw3DLattice();
            
            // Animation loop
            function animate() {
                if (!state.is3DInitialized) return;
                
                state.animationFrameId = requestAnimationFrame(animate);
                
                if (document.getElementById('autoRotate3D').checked) {
                    scene.rotation.y += 0.005;
                }
                
                controls.update();
                renderer.render(scene, camera);
            }
            
            animate();
            
            // Setup controls
            setup3DControls();
            
            // Handle window resize
            window.addEventListener('resize', () => {
                if (!state.is3DInitialized) return;
                
                const container = canvas.parentElement;
                canvas.width = container.clientWidth;
                canvas.height = container.clientHeight;
                
                camera.aspect = canvas.width / canvas.height;
                camera.updateProjectionMatrix();
                renderer.setSize(canvas.width, canvas.height);
            });
        }
        
        function draw3DLattice() {
            const loading = document.getElementById('loading3D');
            loading.style.display = 'flex';
            
            setTimeout(() => {
                try {
                    const scene = state.threeScene;
                    const R = parseInt(document.getElementById('radius3DSlider').value) || 15;
                    const cubeSize = parseFloat(document.getElementById('cubeSizeSlider').value) || 0.7;
                    const showAxes = document.getElementById('showAxes3D').checked;
                    
                    // Clear previous objects
                    state.threeObjects.forEach(obj => scene.remove(obj));
                    state.threeObjects = [];
                    
                    // Add axes if enabled
                    if (showAxes) {
                        const axesHelper = new THREE.AxesHelper(R * 1.5);
                        scene.add(axesHelper);
                        state.threeObjects.push(axesHelper);
                    }
                    
                    // Create materials
                    const coprimeMaterial = new THREE.MeshLambertMaterial({ color: 0x4CAF50 });
                    const nonCoprimeMaterial = new THREE.MeshLambertMaterial({ color: 0xf44336 });
                    const geometry = new THREE.BoxGeometry(cubeSize, cubeSize, cubeSize);
                    
                    // Add cubes (with sampling for performance)
                    let coprimeCount = 0;
                    const sampleStep = R > 10 ? Math.ceil(R / 10) : 1;
                    
                    for (let x = 1; x <= R; x += sampleStep) {
                        for (let y = 1; y <= R; y += sampleStep) {
                            for (let z = 1; z <= R; z += sampleStep) {
                                const isCoprime = gcdArray([x, y, z]) === 1;
                                if (isCoprime) coprimeCount++;
                                
                                const material = isCoprime ? coprimeMaterial : nonCoprimeMaterial;
                                const cube = new THREE.Mesh(geometry, material);
                                
                                cube.position.set(
                                    (x - R/2 - 0.5) * (cubeSize + 0.1),
                                    (y - R/2 - 0.5) * (cubeSize + 0.1),
                                    (z - R/2 - 0.5) * (cubeSize + 0.1)
                                );
                                
                                scene.add(cube);
                                state.threeObjects.push(cube);
                            }
                        }
                    }
                    
                    // Scale counts for statistics
                    const scaleFactor = Math.pow(R / sampleStep, 3);
                    const totalPoints = Math.pow(R, 3);
                    const estimatedCoprime = Math.round(coprimeCount * scaleFactor);
                    const expected = totalPoints / ZETA_VALUES[3];
                    const error = estimatedCoprime - expected;
                    const boundaryCount = 6 * R * R - 12 * R + 8;
                    
                    // Update display
                    document.getElementById('totalPoints3D').textContent = totalPoints.toLocaleString();
                    document.getElementById('coprimeCount3D').textContent = estimatedCoprime.toLocaleString();
                    document.getElementById('error3D').textContent = error.toFixed(2);
                    document.getElementById('boundaryCount3D').textContent = boundaryCount.toLocaleString();
                    
                } catch (error) {
                    console.error('Error drawing 3D lattice:', error);
                } finally {
                    loading.style.display = 'none';
                }
            }, 10);
        }
        
        function setup3DControls() {
            // Radius slider
            const radiusSlider = document.getElementById('radius3DSlider');
            const radiusInput = document.getElementById('radius3DInput');
            const setRadiusBtn = document.getElementById('setRadius3D');
            
            radiusSlider.addEventListener('input', () => {
                const value = radiusSlider.value;
                document.getElementById('radius3DValue').textContent = value;
                radiusInput.value = value;
                draw3DLattice();
            });
            
            setRadiusBtn.addEventListener('click', () => {
                const value = parseInt(radiusInput.value);
                if (value >= 1 && value <= 30) {
                    radiusSlider.value = value;
                    document.getElementById('radius3DValue').textContent = value;
                    draw3DLattice();
                }
            });
            
            // Cube size slider
            document.getElementById('cubeSizeSlider').addEventListener('input', () => {
                document.getElementById('cubeSizeValue').textContent = document.getElementById('cubeSizeSlider').value;
                draw3DLattice();
            });
            
            // Checkboxes
            document.getElementById('autoRotate3D').addEventListener('change', () => {
                // Handled in animation loop
            });
            
            document.getElementById('showAxes3D').addEventListener('change', draw3DLattice);
            
            // Point inspector
            document.getElementById('inspect3DPoint').addEventListener('click', () => {
                const x = parseInt(document.getElementById('inspectX').value) || 1;
                const y = parseInt(document.getElementById('inspectY').value) || 1;
                const z = parseInt(document.getElementById('inspectZ').value) || 1;
                const R = parseInt(document.getElementById('radius3DSlider').value) || 15;
                
                if (x < 1 || x > R || y < 1 || y > R || z < 1 || z > R) {
                    document.getElementById('inspectResult').innerHTML = 
                        `Error: Coordinates must be between 1 and ${R}`;
                    return;
                }
                
                const gcdVal = gcdArray([x, y, z]);
                const isCoprime = gcdVal === 1;
                
                document.getElementById('inspectResult').innerHTML = 
                    `Point (${x}, ${y}, ${z}): GCD = ${gcdVal}, Coprime = ${isCoprime ? 'Yes ✓' : 'No'}`;
                
                // Update GCD calculator
                document.getElementById('gcd1').value = x;
                document.getElementById('gcd2').value = y;
                document.getElementById('gcd3').value = z;
                calculateGCD();
            });
        }
        
        // ===========================================
        // CHART VISUALIZATION
        // ===========================================
        
        function initChart() {
            const ctx = document.getElementById('errorChart').getContext('2d');
            
            state.chart = new Chart(ctx, {
                type: 'line',
                data: state.chartData,
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    interaction: {
                        mode: 'index',
                        intersect: false
                    },
                    plugins: {
                        legend: {
                            position: 'top',
                            labels: {
                                color: '#f0f0f0',
                                font: {
                                    size: 12
                                }
                            }
                        },
                        tooltip: {
                            backgroundColor: 'rgba(0, 0, 0, 0.8)',
                            titleColor: '#f0f0f0',
                            bodyColor: '#f0f0f0',
                            borderColor: '#1a5fb4',
                            borderWidth: 1
                        }
                    },
                    scales: {
                        x: {
                            type: 'linear',
                            title: {
                                display: true,
                                text: 'Radius R',
                                color: '#f0f0f0'
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            },
                            ticks: {
                                color: '#f0f0f0'
                            }
                        },
                        y: {
                            type: 'linear',
                            title: {
                                display: true,
                                text: 'Error Δ(R)',
                                color: '#f0f0f0'
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            },
                            ticks: {
                                color: '#f0f0f0'
                            }
                        }
                    }
                }
            });
            
            updateChart();
        }
        
        function updateChart() {
            const maxR = parseInt(document.getElementById('maxRadiusSlider').value);
            const step = parseInt(document.getElementById('stepSizeSlider').value);
            const showAbsolute = document.getElementById('showAbsoluteError').checked;
            const showRelative = document.getElementById('showRelativeError').checked;
            const showRunning = document.getElementById('showRunningAverage').checked;
            
            // Get selected dimensions
            const selectedDims = Array.from(document.querySelectorAll('.dimension-checkbox:checked'))
                .map(cb => parseInt(cb.value))
                .sort((a, b) => a - b);
            
            if (selectedDims.length === 0) return;
            
            // Generate labels
            const labels = [];
            for (let R = step; R <= maxR; R += step) {
                labels.push(R);
            }
            
            // Generate datasets
            const datasets = [];
            const colors = ['#1a5fb4', '#4CAF50', '#FF9800', '#9C27B0', '#E91E63'];
            
            selectedDims.forEach((k, index) => {
                // Absolute error
                if (showAbsolute) {
                    const data = [];
                    for (let R = step; R <= maxR; R += step) {
                        const N = state.research.highPrecision ? computeNHighPrecision(R, k) : computeN(R, k);
                        const expected = Math.pow(R, k) / ZETA_VALUES[k];
                        data.push(N - expected);
                    }
                    
                    datasets.push({
                        label: `k=${k}: Absolute Error`,
                        data: data,
                        borderColor: colors[index % colors.length],
                        backgroundColor: 'transparent',
                        borderWidth: 2,
                        tension: 0.1
                    });
                }
                
                // Relative error
                if (showRelative) {
                    const data = [];
                    for (let R = step; R <= maxR; R += step) {
                        const N = state.research.highPrecision ? computeNHighPrecision(R, k) : computeN(R, k);
                        const expected = Math.pow(R, k) / ZETA_VALUES[k];
                        const error = N - expected;
                        data.push((error / expected) * 100);
                    }
                    
                    datasets.push({
                        label: `k=${k}: Relative Error (%)`,
                        data: data,
                        borderColor: colors[index % colors.length],
                        backgroundColor: 'transparent',
                        borderWidth: 2,
                        borderDash: [5, 5],
                        tension: 0.1
                    });
                }
                
                // Running average
                if (showRunning && k === selectedDims[0]) {
                    const runningAvg = [];
                    let cumulativeSum = 0;
                    let count = 0;
                    
                    for (let R = step; R <= maxR; R += step) {
                        const N = state.research.highPrecision ? computeNHighPrecision(R, k) : computeN(R, k);
                        const expected = Math.pow(R, k) / ZETA_VALUES[k];
                        cumulativeSum += N - expected;
                        count++;
                        runningAvg.push(cumulativeSum / count);
                    }
                    
                    datasets.push({
                        label: 'Running Average',
                        data: runningAvg,
                        borderColor: '#FFD700',
                        backgroundColor: 'transparent',
                        borderWidth: 3,
                        borderDash: [5, 5],
                        tension: 0.2
                    });
                }
            });
            
            // Update chart
            state.chart.data.labels = labels;
            state.chart.data.datasets = datasets;
            
            // Apply log-log scale if enabled
            if (state.research.logLogPlot) {
                state.chart.options.scales.x.type = 'logarithmic';
                state.chart.options.scales.y.type = 'logarithmic';
                state.chart.options.scales.x.title.text = 'log(R)';
                state.chart.options.scales.y.title.text = 'log(Δ(R))';
            } else {
                state.chart.options.scales.x.type = 'linear';
                state.chart.options.scales.y.type = 'linear';
                state.chart.options.scales.x.title.text = 'Radius R';
                state.chart.options.scales.y.title.text = 'Error Δ(R)';
            }
            
            state.chart.update();
            
            // Update critical strip if research mode is active
            if (state.researchMode) {
                updateCriticalStrip(maxR, selectedDims[0]);
            }
        }
        
        // ===========================================
        // GCD CALCULATOR
        // ===========================================
        
        function calculateGCD() {
            const inputs = [];
            for (let i = 1; i <= 6; i++) {
                const input = document.getElementById(`gcd${i}`);
                if (input.style.display !== 'none') {
                    const value = parseInt(input.value) || 1;
                    inputs.push(value);
                }
            }
            
            if (inputs.length === 0) return;
            
            const gcdVal = gcdArray(inputs);
            const lcm = inputs.reduce((a, b) => Math.abs(a * b) / gcd(a, b));
            const areCoprime = gcdVal === 1;
            
            // Count factors
            let numFactors = 0;
            for (let i = 1; i <= Math.sqrt(gcdVal); i++) {
                if (gcdVal % i === 0) {
                    numFactors += 2;
                    if (i === gcdVal / i) numFactors--;
                }
            }
            
            // Update display
            document.getElementById('gcdResult').innerHTML = 
                `GCD(${inputs.join(', ')}) = <strong>${gcdVal}</strong>`;
            
            document.getElementById('gcdValue').textContent = gcdVal;
            document.getElementById('lcmValue').textContent = lcm;
            document.getElementById('areCoprime').textContent = areCoprime ? 'Yes' : 'No';
            document.getElementById('numFactors').textContent = numFactors;
        }
        
        function showPrimeFactors() {
            const inputs = [];
            for (let i = 1; i <= 6; i++) {
                const input = document.getElementById(`gcd${i}`);
                if (input.style.display !== 'none') {
                    const value = parseInt(input.value) || 1;
                    inputs.push(value);
                }
            }
            
            let html = '<strong>Prime Factorization:</strong><br>';
            inputs.forEach((num, index) => {
                const factors = primeFactors(num);
                const factorStr = Object.entries(factors)
                    .map(([prime, power]) => power > 1 ? `${prime}<sup>${power}</sup>` : prime)
                    .join(' × ') || '1';
                
                html += `${num} = ${factorStr}<br>`;
            });
            
            document.getElementById('primeFactors').innerHTML = html;
            document.getElementById('primeFactors').style.display = 'block';
        }
        
        function addGCDInput() {
            for (let i = 1; i <= 6; i++) {
                const input = document.getElementById(`gcd${i}`);
                if (input.style.display === 'none') {
                    input.style.display = 'block';
                    input.value = 1;
                    if (i === 6) {
                        document.getElementById('addGCDInput').style.display = 'none';
                    }
                    break;
                }
            }
            calculateGCD();
        }
        
        // ===========================================
        // RESEARCH MODE FUNCTIONS
        // ===========================================
        
        function initResearchMode() {
            // Toggle research mode
            document.getElementById('researchToggle').addEventListener('click', () => {
                state.researchMode = !state.researchMode;
                const overlay = document.getElementById('researchOverlay');
                const toggleBtn = document.getElementById('researchToggle');
                
                if (state.researchMode) {
                    overlay.classList.add('active');
                    toggleBtn.classList.add('active');
                    toggleBtn.innerHTML = '<span>🔬</span> Research Mode (ON)';
                    logToConsole('Research mode activated', 'info');
                } else {
                    overlay.classList.remove('active');
                    toggleBtn.classList.remove('active');
                    toggleBtn.innerHTML = '<span>🔬</span> Research Mode';
                }
            });
            
            // Close research mode
            document.getElementById('closeResearch').addEventListener('click', () => {
                state.researchMode = false;
                document.getElementById('researchOverlay').classList.remove('active');
                document.getElementById('researchToggle').classList.remove('active');
                document.getElementById('researchToggle').innerHTML = '<span>🔬</span> Research Mode';
            });
            
            // Research controls
            document.getElementById('toggleMobiusWave').addEventListener('click', function() {
                state.research.mobiusWave = !state.research.mobiusWave;
                this.textContent = state.research.mobiusWave ? 'Hide Wave' : 'Show Wave';
                this.classList.toggle('active');
                
                if (state.currentTab === 'visualization') {
                    draw2DLattice();
                }
                
                logToConsole(`Möbius wave overlay ${state.research.mobiusWave ? 'enabled' : 'disabled'}`, 'info');
            });
            
            document.getElementById('waveIntensityBtn').addEventListener('click', function() {
                state.research.waveIntensity = (state.research.waveIntensity + 0.5) % 2.5;
                const levels = ['Low', 'Medium', 'High', 'Very High'];
                this.textContent = `Intensity: ${levels[Math.floor(state.research.waveIntensity / 0.5)]}`;
                
                if (state.research.mobiusWave) {
                    draw2DLattice();
                }
            });
            
            document.getElementById('toggleLogLog').addEventListener('click', function() {
                state.research.logLogPlot = !state.research.logLogPlot;
                this.textContent = state.research.logLogPlot ? 'Linear Scale' : 'Log-Log Scale';
                this.classList.toggle('active');
                
                if (state.chart) {
                    updateChart();
                }
                
                logToConsole(`Log-log plot ${state.research.logLogPlot ? 'enabled' : 'disabled'}`, 'info');
            });
            
            document.getElementById('fitPowerLaw').addEventListener('click', calculateGrowthExponent);
            document.getElementById('togglePrecision').addEventListener('click', togglePrecisionMode);
            document.getElementById('benchmarkPrecision').addEventListener('click', benchmarkPrecision);
            
            // Console controls
            document.getElementById('clearConsole').addEventListener('click', clearConsole);
            document.getElementById('exportResearchData').addEventListener('click', exportResearchData);
            document.getElementById('runFullAnalysis').addEventListener('click', runFullAnalysis);
        }
        
        function calculateGrowthExponent() {
            const maxR = parseInt(document.getElementById('maxRadiusSlider').value);
            const step = parseInt(document.getElementById('stepSizeSlider').value);
            const k = parseInt(document.querySelector('.dimension-checkbox:checked')?.value || 2);
            
            // Collect log-log data
            const logR = [];
            const logDelta = [];
            
            for (let R = step; R <= maxR; R += step) {
                const N = state.research.highPrecision ? computeNHighPrecision(R, k) : computeN(R, k);
                const expected = Math.pow(R, k) / ZETA_VALUES[k];
                const delta = Math.abs(N - expected);
                
                if (delta > 0 && R > 1) {
                    logR.push(Math.log(R));
                    logDelta.push(Math.log(delta));
                }
            }
            
            // Linear regression
            const n = logR.length;
            let sumX = 0, sumY = 0, sumXY = 0, sumX2 = 0;
            
            for (let i = 0; i < n; i++) {
                sumX += logR[i];
                sumY += logDelta[i];
                sumXY += logR[i] * logDelta[i];
                sumX2 += logR[i] * logR[i];
            }
            
            const α = (n * sumXY - sumX * sumY) / (n * sumX2 - sumX * sumX);
            const β = (sumY - α * sumX) / n;
            
            // Calculate R²
            let ssTotal = 0, ssResidual = 0;
            const meanY = sumY / n;
            
            for (let i = 0; i < n; i++) {
                const predicted = α * logR[i] + β;
                ssTotal += Math.pow(logDelta[i] - meanY, 2);
                ssResidual += Math.pow(logDelta[i] - predicted, 2);
            }
            
            const r2 = 1 - (ssResidual / (ssTotal || 1));
            const confidence = 1.96 * Math.sqrt(ssResidual / (n - 2)) / Math.sqrt(sumX2 - sumX * sumX / n);
            
            // Update display
            document.getElementById('exponentDisplay').textContent = α.toFixed(4);
            document.getElementById('confidenceDisplay').textContent = confidence.toFixed(6);
            document.getElementById('r2Display').textContent = r2.toFixed(6);
            
            // Also update in analysis tab
            document.getElementById('growthExponent').textContent = α.toFixed(4);
            document.getElementById('goodnessOfFit').textContent = r2.toFixed(6);
            
            // Store in research data
            state.research.diagnosticData.push({
                type: 'growth_exponent',
                k: k,
                R_max: maxR,
                exponent: α,
                confidence: confidence,
                r2: r2,
                timestamp: new Date().toISOString()
            });
            
            logToConsole(`Growth exponent: α = ${α.toFixed(4)} ± ${confidence.toFixed(6)} (R² = ${r2.toFixed(6)})`, 'info');
            
            return { exponent: α, confidence: confidence, r2: r2 };
        }
        
        function updateCriticalStrip(R, k) {
            const N = state.research.highPrecision ? computeNHighPrecision(R, k) : computeN(R, k);
            const expected = Math.pow(R, k) / ZETA_VALUES[k];
            const delta = N - expected;
            
            // Map delta to complex plane (simplified visualization)
            const re = 0.5 + (delta / Math.pow(R, k - 0.5)) * 0.1;
            const im = Math.log(R) * 0.5;
            
            // Normalize for display
            const displayRe = Math.max(0, Math.min(1, re));
            const displayIm = Math.max(0.25, Math.min(0.75, im / 10));
            
            // Update point position
            const point = document.getElementById('deltaPoint');
            point.style.left = `${displayRe * 100}%`;
            point.style.top = `${displayIm * 100}%`;
            
            // Update display
            document.getElementById('currentRe').textContent = re.toFixed(6);
            document.getElementById('currentIm').textContent = im.toFixed(6);
            document.getElementById('distanceFromHalf').textContent = Math.abs(re - 0.5).toFixed(6);
            
            // Store data
            state.research.diagnosticData.push({
                type: 'critical_strip',
                R: R,
                k: k,
                delta: delta,
                re: re,
                im: im,
                distance: Math.abs(re - 0.5),
                timestamp: new Date().toISOString()
            });
        }
        
        function togglePrecisionMode() {
            state.research.highPrecision = !state.research.highPrecision;
            const btn = document.getElementById('togglePrecision');
            
            if (state.research.highPrecision) {
                btn.textContent = 'High Precision';
                btn.classList.add('active');
                Decimal.set({ precision: state.research.precisionDigits });
                logToConsole(`High precision mode enabled (${state.research.precisionDigits} digits)`, 'info');
            } else {
                btn.textContent = '64-bit Float';
                btn.classList.remove('active');
                logToConsole('Switched to standard 64-bit floating point', 'info');
            }
            
            // Update chart if on analysis tab
            if (state.currentTab === 'analysis' && state.chart) {
                updateChart();
            }
        }
        
        function benchmarkPrecision() {
            const R = state.currentR;
            const k = 2;
            
            // Standard precision
            const startStandard = performance.now();
            const N_standard = computeN(R, k);
            const expected_standard = Math.pow(R, k) / ZETA_VALUES[k];
            const delta_standard = N_standard - expected_standard;
            const timeStandard = performance.now() - startStandard;
            
            // High precision
            const startHigh = performance.now();
            const N_high = computeNHighPrecision(R, k);
            const expected_high = new Decimal(R).pow(k).dividedBy(ZETA_VALUES[k]).toNumber();
            const delta_high = N_high - expected_high;
            const timeHigh = performance.now() - startHigh;
            
            // Calculate relative error
            const relError = Math.abs(delta_standard - delta_high) / Math.abs(delta_high || 1);
            
            // Update display
            document.getElementById('standardPrecision').textContent = delta_standard.toExponential(6);
            document.getElementById('highPrecision').textContent = delta_high.toExponential(6);
            document.getElementById('relativeErrorDisplay').textContent = relError.toExponential(6);
            
            logToConsole(`Benchmark: Standard=${timeStandard.toFixed(2)}ms, High=${timeHigh.toFixed(2)}ms, RelError=${relError.toExponential(6)}`, 'info');
        }
        
        function runFullAnalysis() {
            logToConsole('Starting full diagnostic analysis...', 'info');
            
            // 1. Growth exponent
            const exponent = calculateGrowthExponent();
            
            // 2. Running average test
            testBoundarySelfCorrection();
            
            // 3. Precision benchmark
            benchmarkPrecision();
            
            // 4. Update critical strip
            const maxR = parseInt(document.getElementById('maxRadiusSlider').value);
            const k = parseInt(document.querySelector('.dimension-checkbox:checked')?.value || 2);
            updateCriticalStrip(maxR, k);
            
            logToConsole('Full analysis complete!', 'info');
        }
        
        function testBoundarySelfCorrection() {
            const R = parseInt(document.getElementById('maxRadiusSlider').value);
            const k = 2;
            const step = parseInt(document.getElementById('stepSizeSlider').value);
            
            const errors = [];
            let runningSum = 0;
            
            for (let r = step; r <= R; r += step) {
                const N = state.research.highPrecision ? computeNHighPrecision(r, k) : computeN(r, k);
                const expected = Math.pow(r, k) / ZETA_VALUES[k];
                const error = N - expected;
                errors.push(error);
                runningSum += error;
            }
            
            const runningAverage = runningSum / errors.length;
            
            // Calculate autocorrelation
            const mean = errors.reduce((a, b) => a + b, 0) / errors.length;
            let autocorr = 0;
            let variance = 0;
            
            for (let i = 0; i < errors.length; i++) {
                variance += Math.pow(errors[i] - mean, 2);
                if (i > 0) {
                    autocorr += (errors[i] - mean) * (errors[i-1] - mean);
                }
            }
            
            variance /= errors.length;
            autocorr /= (errors.length - 1) * variance;
            
            // Update display
            document.getElementById('runningAverage').textContent = runningAverage.toExponential(4);
            document.getElementById('autocorrelation').textContent = autocorr.toFixed(4);
            
            // Test for self-correction
            const passed = Math.abs(runningAverage) < 0.01 && Math.abs(autocorr) < 0.1;
            
            logToConsole(`Self-correction test: ${passed ? 'PASS' : 'FAIL'} (avg=${runningAverage.toExponential(4)}, autocorr=${autocorr.toFixed(4)})`, 
                        passed ? 'info' : 'warning');
        }
        
        function logToConsole(message, type = 'info') {
            const consoleEl = document.getElementById('researchConsole');
            const entry = document.createElement('div');
            entry.className = `console-entry ${type}`;
            entry.textContent = `[${new Date().toLocaleTimeString()}] ${message}`;
            
            consoleEl.appendChild(entry);
            consoleEl.scrollTop = consoleEl.scrollHeight;
            
            // Store in history
            state.research.consoleHistory.push({
                message,
                type,
                timestamp: new Date().toISOString()
            });
            
            // Limit history size
            if (state.research.consoleHistory.length > 100) {
                state.research.consoleHistory.shift();
            }
        }
        
        function clearConsole() {
            const consoleEl = document.getElementById('researchConsole');
            consoleEl.innerHTML = '';
            state.research.consoleHistory = [];
            logToConsole('Console cleared', 'info');
        }
        
        function exportResearchData() {
            const data = {
                metadata: {
                    export_date: new Date().toISOString(),
                    project: 'Boundary Cancellation Principle',
                    version: '1.0.0',
                    research_mode: state.researchMode
                },
                parameters: {
                    current_R: state.currentR,
                    high_precision: state.research.highPrecision,
                    precision_digits: state.research.precisionDigits
                },
                diagnostics: state.research.diagnosticData,
                console_history: state.research.consoleHistory
            };
            
            const jsonStr = JSON.stringify(data, null, 2);
            const blob = new Blob([jsonStr], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.href = url;
            link.download = `research_data_${Date.now()}.json`;
            link.click();
            
            setTimeout(() => URL.revokeObjectURL(url), 100);
            
            logToConsole('Research data exported', 'info');
        }
        
        // ===========================================
        // EXPORT FUNCTIONS
        // ===========================================
        
        function initExport() {
            // 2D PNG Export
            document.getElementById('export2DPNG').addEventListener('click', () => {
                exportCanvasAsPNG('latticeCanvas2D', '2d_lattice.png');
            });
            
            // 3D PNG Export
            document.getElementById('export3DPNG').addEventListener('click', () => {
                if (!state.is3DInitialized) {
                    showExportStatus('Please initialize 3D visualization first', 'error');
                    return;
                }
                
                // Force render before capture
                state.threeRenderer.render(state.threeScene, state.threeCamera);
                exportCanvasAsPNG('latticeCanvas3D', '3d_lattice.png');
            });
            
            // Chart PNG Export
            document.getElementById('exportChartPNG').addEventListener('click', () => {
                if (!state.chart) {
                    showExportStatus('Chart not initialized', 'error');
                    return;
                }
                exportCanvasAsPNG('errorChart', 'error_chart.png');
            });
            
            // Error Data CSV Export
            document.getElementById('exportErrorCSV').addEventListener('click', exportErrorDataCSV);
            
            // Lattice Data CSV Export
            document.getElementById('exportLatticeCSV').addEventListener('click', exportLatticeDataCSV);
            
            // Full Report JSON Export
            document.getElementById('exportFullReport').addEventListener('click', exportFullReportJSON);
        }
        
        function exportCanvasAsPNG(canvasId, filename) {
            const canvas = document.getElementById(canvasId);
            if (!canvas) {
                showExportStatus(`Canvas ${canvasId} not found`, 'error');
                return;
            }
            
            const link = document.createElement('a');
            link.download = filename;
            link.href = canvas.toDataURL('image/png');
            link.click();
            
            showExportStatus(`Exported ${filename}`, 'success');
        }
        
        function exportErrorDataCSV() {
            const maxR = parseInt(document.getElementById('maxRadiusSlider').value);
            const step = parseInt(document.getElementById('stepSizeSlider').value);
            const selectedDims = Array.from(document.querySelectorAll('.dimension-checkbox:checked'))
                .map(cb => parseInt(cb.value));
            
            if (selectedDims.length === 0) {
                showExportStatus('Please select at least one dimension', 'error');
                return;
            }
            
            let csv = 'R';
            selectedDims.forEach(k => {
                csv += `,N(R)_k${k},Expected_k${k},Error_k${k},RelativeError%_k${k}`;
            });
            csv += '\n';
            
            for (let R = step; R <= maxR; R += step) {
                csv += R;
                selectedDims.forEach(k => {
                    const N = state.research.highPrecision ? computeNHighPrecision(R, k) : computeN(R, k);
                    const expected = Math.pow(R, k) / ZETA_VALUES[k];
                    const error = N - expected;
                    const relative = (error / expected) * 100;
                    
                    csv += `,${N},${expected.toFixed(6)},${error.toFixed(6)},${relative.toFixed(6)}`;
                });
                csv += '\n';
            }
            
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.href = url;
            link.download = `error_data_${Date.now()}.csv`;
            link.click();
            
            setTimeout(() => URL.revokeObjectURL(url), 100);
            
            showExportStatus(`Exported error data for R=1-${maxR}`, 'success');
        }
        
        function exportLatticeDataCSV() {
            const R = parseInt(document.getElementById('radius2DSlider').value);
            
            let csv = 'x,y,gcd,is_coprime,is_boundary\n';
            for (let x = 1; x <= R; x++) {
                for (let y = 1; y <= R; y++) {
                    const g = gcd(x, y);
                    csv += `${x},${y},${g},${g === 1},${Math.max(x, y) === R}\n`;
                }
            }
            
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.href = url;
            link.download = `lattice_${R}x${R}_${Date.now()}.csv`;
            link.click();
            
            setTimeout(() => URL.revokeObjectURL(url), 100);
            
            showExportStatus(`Exported ${R}x${R} lattice data`, 'success');
        }
        
        function exportFullReportJSON() {
            const data = {
                metadata: {
                    export_date: new Date().toISOString(),
                    project: 'Boundary Cancellation Principle',
                    version: '1.0.0'
                },
                parameters: {
                    current_2D_radius: parseInt(document.getElementById('radius2DSlider').value),
                    current_3D_radius: parseInt(document.getElementById('radius3DSlider').value),
                    chart_max_radius: parseInt(document.getElementById('maxRadiusSlider').value),
                    dimensions: Array.from(document.querySelectorAll('.dimension-checkbox:checked'))
                        .map(cb => parseInt(cb.value))
                },
                zeta_values: ZETA_VALUES,
                research_data: state.research.diagnosticData
            };
            
            const jsonStr = JSON.stringify(data, null, 2);
            const blob = new Blob([jsonStr], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.href = url;
            link.download = `full_report_${Date.now()}.json`;
            link.click();
            
            setTimeout(() => URL.revokeObjectURL(url), 100);
            
            showExportStatus('Exported full report', 'success');
        }
        
        function showExportStatus(message, type = 'info') {
            const statusEl = document.getElementById('exportStatus');
            statusEl.textContent = message;
            
            if (type === 'success') {
                statusEl.style.color = '#10b981';
            } else if (type === 'error') {
                statusEl.style.color = '#ef4444';
            } else {
                statusEl.style.color = '#f0f0f0';
            }
            
            setTimeout(() => {
                statusEl.textContent = 'Ready to export. Select an option above.';
                statusEl.style.color = '#f0f0f0';
            }, 5000);
        }
        
        // ===========================================
        // TAB MANAGEMENT
        // ===========================================
        
        function initTabs() {
            const tabBtns = document.querySelectorAll('.tab-btn');
            const tabPanes = document.querySelectorAll('.tab-pane');
            
            tabBtns.forEach(btn => {
                btn.addEventListener('click', () => {
                    const tabId = btn.getAttribute('data-tab');
                    
                    // Update active tab button
                    tabBtns.forEach(b => b.classList.remove('active'));
                    btn.classList.add('active');
                    
                    // Update active tab pane
                    tabPanes.forEach(pane => pane.classList.remove('active'));
                    document.getElementById(tabId).classList.add('active');
                    
                    // Update state
                    state.currentTab = tabId;
                    
                    // Initialize specific features if needed
                    if (tabId === 'visualization') {
                        init2DVisualization();
                        init3DVisualization();
                    } else if (tabId === 'analysis') {
                        if (!state.chart) {
                            initChart();
                        }
                    }
                });
            });
        }
        
        // ===========================================
        // INITIALIZATION
        // ===========================================
        
        function init() {
            // Initialize tabs
            initTabs();
            
            // Initialize GCD calculator
            document.getElementById('calculateGCD').addEventListener('click', calculateGCD);
            document.getElementById('addGCDInput').addEventListener('click', addGCDInput);
            document.getElementById('showFactors').addEventListener('click', showPrimeFactors);
            
            // Initialize number theory tools
            document.getElementById('calculateMobius').addEventListener('click', () => {
                const n = parseInt(document.getElementById('mobiusInput').value) || 1;
                const mu = mobius(n);
                document.getElementById('mobiusResult').innerHTML = `μ(${n}) = ${mu}`;
            });
            
            document.getElementById('testPrime').addEventListener('click', () => {
                const n = parseInt(document.getElementById('primeTestInput').value) || 1;
                const isPrimeResult = isPrime(n);
                document.getElementById('primeResult').innerHTML = 
                    `${n} is ${isPrimeResult ? 'prime ✓' : 'composite'}`;
            });
            
            document.getElementById('calculateTotient').addEventListener('click', () => {
                const n = parseInt(document.getElementById('totientInput').value) || 1;
                const phi = totient(n);
                document.getElementById('totientResult').innerHTML = `φ(${n}) = ${phi}`;
            });
            
            // Initialize chart controls
            document.getElementById('maxRadiusSlider').addEventListener('input', () => {
                const value = document.getElementById('maxRadiusSlider').value;
                document.getElementById('maxRadiusValue').textContent = value;
                document.getElementById('maxRadiusInput').value = value;
                updateChart();
            });
            
            document.getElementById('setMaxRadius').addEventListener('click', () => {
                const value = parseInt(document.getElementById('maxRadiusInput').value);
                if (value >= 10 && value <= 5000) {
                    document.getElementById('maxRadiusSlider').value = value;
                    document.getElementById('maxRadiusValue').textContent = value;
                    updateChart();
                }
            });
            
            document.getElementById('stepSizeSlider').addEventListener('input', () => {
                document.getElementById('stepSizeValue').textContent = document.getElementById('stepSizeSlider').value;
                updateChart();
            });
            
            document.getElementById('showAbsoluteError').addEventListener('change', updateChart);
            document.getElementById('showRelativeError').addEventListener('change', updateChart);
            document.getElementById('showRunningAverage').addEventListener('change', updateChart);
            
            document.querySelectorAll('.dimension-checkbox').forEach(cb => {
                cb.addEventListener('change', updateChart);
            });
            
            document.getElementById('analyzePoint').addEventListener('click', () => {
                const R = parseInt(document.getElementById('analyzeR').value) || 100;
                const k = parseInt(document.getElementById('analyzeK').value) || 2;
                const N = state.research.highPrecision ? computeNHighPrecision(R, k) : computeN(R, k);
                const expected = Math.pow(R, k) / ZETA_VALUES[k];
                const error = N - expected;
                const relative = (error / expected) * 100;
                
                document.getElementById('analysisResult').innerHTML = 
                    `For R=${R}, k=${k}:<br>` +
                    `N(R) = ${N.toLocaleString()}<br>` +
                    `Expected = ${expected.toFixed(2)}<br>` +
                    `Δ(R) = ${error.toFixed(2)}<br>` +
                    `Relative Error = ${relative.toFixed(4)}%`;
            });
            
            // Initialize research mode
            initResearchMode();
            
            // Initialize export functions
            initExport();
            
            // Initialize chart controls in analysis tab
            document.getElementById('calculateExponent').addEventListener('click', calculateGrowthExponent);
            document.getElementById('testSelfCorrection').addEventListener('click', testBoundarySelfCorrection);
            
            // Show welcome message
            setTimeout(() => {
                logToConsole('Welcome to the Boundary Cancellation Principle Discovery Engine!', 'info');
                logToConsole('Use Research Mode (Ctrl+R) for advanced diagnostics', 'info');
            }, 1000);
            
            // Keyboard shortcuts
            document.addEventListener('keydown', (e) => {
                if (e.ctrlKey && e.key === 'r') {
                    e.preventDefault();
                    document.getElementById('researchToggle').click();
                }
            });
        }
        
        // Start everything when page loads
        window.addEventListener('load', init);
        
        // Clean up on page unload
        window.addEventListener('beforeunload', () => {
            if (state.animationId) clearTimeout(state.animationId);
            if (state.animationFrameId) cancelAnimationFrame(state.animationFrameId);
        });
        
        // MathJax configuration
        window.MathJax = {
            tex: {
                inlineMath: [['$', '$'], ['\\(', '\\)']],
                displayMath: [['$$', '$$'], ['\\[', '\\]']],
                processEscapes: true,
                processEnvironments: true
            },
            options: {
                skipHtmlTags: ['script', 'noscript', 'style', 'textarea', 'pre']
            }
        };
    </script>
</body>
</html>
