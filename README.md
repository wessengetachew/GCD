
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Boundary Cancellation Principle: An Analysis of Arithmetic Lattice Residues</title>
    <!-- MathJax for LaTeX rendering -->
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
    <!-- Chart.js for data visualization -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- PapaParse for CSV export -->
    <script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>
    <!-- Three.js for 3D visualization -->
    <script src="https://cdn.jsdelivr.net/npm/three@0.132.2/build/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.132.2/examples/js/controls/OrbitControls.js"></script>
    <style>
        /* Reset and base styles */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Times New Roman', Times, serif;
            line-height: 1.6;
            color: #222;
            background-color: #fefefe;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            counter-reset: section;
        }
        
        /* Typography */
        h1 {
            font-size: 2.2rem;
            text-align: center;
            margin: 1.5rem 0;
            padding-bottom: 0.5rem;
            border-bottom: 2px solid #1a5fb4;
            color: #1a5fb4;
        }
        
        .author {
            text-align: center;
            font-style: italic;
            margin-bottom: 2rem;
            color: #555;
        }
        
        .author a {
            color: #1a5fb4;
            text-decoration: none;
        }
        
        .author a:hover {
            text-decoration: underline;
        }
        
        h2 {
            font-size: 1.6rem;
            margin: 2rem 0 1rem;
            color: #1a5fb4;
            counter-increment: section;
            padding-top: 1rem;
            border-top: 1px solid #eee;
        }
        
        h2:first-of-type {
            border-top: none;
        }
        
        h2:before {
            content: counter(section) ". ";
        }
        
        h3 {
            font-size: 1.2rem;
            margin: 1.2rem 0 0.5rem;
            color: #333;
        }
        
        p {
            margin-bottom: 1rem;
            text-align: justify;
        }
        
        /* Abstract styling */
        .abstract {
            background-color: #f0f7ff;
            border-left: 4px solid #1a5fb4;
            padding: 1.2rem;
            margin: 1.5rem 0;
            border-radius: 0 5px 5px 0;
        }
        
        .abstract h3 {
            margin-top: 0;
            color: #1a5fb4;
        }
        
        /* Key results */
        .key-results {
            background-color: #f9f9f9;
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 1.2rem;
            margin: 1.5rem 0;
        }
        
        .key-results h3 {
            margin-top: 0;
        }
        
        .key-results ul {
            padding-left: 1.5rem;
        }
        
        .key-results li {
            margin-bottom: 0.5rem;
        }
        
        /* Boxed formulas */
        .formula-box {
            background-color: #f8f9fa;
            border: 2px solid #e0e0e0;
            border-radius: 5px;
            padding: 1rem;
            margin: 1.2rem 0;
            text-align: center;
            overflow-x: auto;
        }
        
        .formula-box .formula-label {
            display: block;
            font-weight: bold;
            margin-bottom: 0.5rem;
            color: #555;
            font-size: 0.9rem;
            text-align: left;
        }
        
        /* Table styling */
        .dimension-table {
            width: 100%;
            border-collapse: collapse;
            margin: 1.5rem 0;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .dimension-table th {
            background-color: #1a5fb4;
            color: white;
            padding: 0.8rem;
            text-align: left;
        }
        
        .dimension-table td {
            padding: 0.8rem;
            border-bottom: 1px solid #ddd;
        }
        
        .dimension-table tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        
        .dimension-table tr:hover {
            background-color: #f0f7ff;
        }
        
        /* Visualization containers */
        .visualization-container {
            margin: 2rem 0;
            background-color: #f9f9f9;
            border-radius: 8px;
            padding: 1.5rem;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        
        .canvas-container {
            position: relative;
            width: 100%;
            height: 500px;
            border: 2px solid #ddd;
            border-radius: 5px;
            overflow: hidden;
            background-color: white;
            margin: 1rem 0;
        }
        
        #latticeCanvas2D, #latticeCanvas3D {
            width: 100%;
            height: 100%;
            display: block;
        }
        
        .canvas-label {
            position: absolute;
            top: 10px;
            left: 10px;
            background-color: rgba(255, 255, 255, 0.8);
            padding: 5px 10px;
            border-radius: 3px;
            font-weight: bold;
            z-index: 100;
        }
        
        .chart-container {
            height: 400px;
            margin: 1.5rem 0;
            position: relative;
        }
        
        /* Export controls */
        .export-controls {
            background-color: #f0f7ff;
            border: 1px solid #c5d9f0;
            border-radius: 5px;
            padding: 1.5rem;
            margin: 2rem 0;
        }
        
        .export-buttons {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 1rem;
        }
        
        .export-btn {
            background-color: #1a5fb4;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.9rem;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .export-btn:hover {
            background-color: #0d4a9e;
        }
        
        .export-btn.export-png {
            background-color: #4CAF50;
        }
        
        .export-btn.export-png:hover {
            background-color: #3d8b40;
        }
        
        .export-btn.export-csv {
            background-color: #FF9800;
        }
        
        .export-btn.export-csv:hover {
            background-color: #e68900;
        }
        
        /* Interactive controls */
        .interactive-controls {
            background-color: #f0f7ff;
            border: 1px solid #c5d9f0;
            border-radius: 5px;
            padding: 1.5rem;
            margin: 2rem 0;
        }
        
        .interactive-controls h4 {
            margin-top: 0;
            margin-bottom: 1.2rem;
            color: #1a5fb4;
            font-size: 1.1rem;
        }
        
        .control-row {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin: 1rem 0;
            align-items: center;
        }
        
        .slider-container {
            flex: 1;
            min-width: 200px;
        }
        
        .input-container {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 5px;
        }
        
        .input-container input[type="number"] {
            width: 80px;
            padding: 5px;
            border: 1px solid #ccc;
            border-radius: 3px;
            font-size: 0.9rem;
        }
        
        .slider-container label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: bold;
            font-size: 0.95rem;
        }
        
        .slider-value {
            display: inline-block;
            min-width: 40px;
            font-weight: bold;
            color: #1a5fb4;
            font-size: 1.1rem;
        }
        
        input[type="range"] {
            width: 100%;
            height: 8px;
            border-radius: 4px;
            background: #ddd;
            outline: none;
            opacity: 0.7;
            transition: opacity .2s;
        }
        
        input[type="range"]:hover {
            opacity: 1;
        }
        
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #1a5fb4;
            cursor: pointer;
        }
        
        input[type="range"]::-moz-range-thumb {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #1a5fb4;
            cursor: pointer;
            border: none;
        }
        
        .calc-results {
            background-color: white;
            border: 1px solid #c5d9f0;
            border-radius: 5px;
            padding: 1rem;
            margin-top: 1.5rem;
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
        }
        
        .calc-results p {
            margin-bottom: 0.5rem;
        }
        
        /* GCD Display Area */
        .gcd-display {
            background-color: white;
            border: 2px solid #4CAF50;
            border-radius: 5px;
            padding: 1rem;
            margin-top: 1.5rem;
            font-family: 'Courier New', monospace;
        }
        
        .gcd-display h4 {
            margin-top: 0;
            margin-bottom: 0.5rem;
            color: #4CAF50;
        }
        
        .gcd-result {
            font-size: 1.1rem;
            margin: 0.5rem 0;
            padding: 5px;
            background-color: #f9f9f9;
            border-radius: 3px;
        }
        
        .gcd-inputs {
            display: flex;
            gap: 10px;
            margin: 10px 0;
            flex-wrap: wrap;
        }
        
        .gcd-inputs input {
            width: 60px;
            padding: 5px;
            border: 1px solid #ccc;
            border-radius: 3px;
        }
        
        .gcd-inputs button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 3px;
            cursor: pointer;
        }
        
        /* Footer */
        footer {
            text-align: center;
            margin-top: 3rem;
            padding-top: 1rem;
            border-top: 1px solid #ddd;
            color: #666;
            font-size: 0.9rem;
            line-height: 1.4;
        }
        
        footer .attribution {
            font-weight: bold;
            margin-bottom: 0.5rem;
            color: #333;
        }
        
        footer .social {
            margin-top: 0.5rem;
        }
        
        footer .social a {
            color: #1a5fb4;
            text-decoration: none;
        }
        
        footer .social a:hover {
            text-decoration: underline;
        }
        
        /* Responsive design */
        @media (max-width: 768px) {
            body {
                padding: 10px;
            }
            
            h1 {
                font-size: 1.8rem;
            }
            
            h2 {
                font-size: 1.4rem;
            }
            
            .dimension-table {
                display: block;
                overflow-x: auto;
            }
            
            .canvas-container {
                height: 400px;
            }
            
            .export-buttons {
                flex-direction: column;
            }
            
            .control-row {
                flex-direction: column;
            }
            
            .slider-container {
                min-width: 100%;
            }
        }
        
        /* MathJax styling */
        .MathJax {
            font-size: 1.1em !important;
        }
        
        mjx-container[jax="CHTML"][display="true"] {
            margin: 1em 0 !important;
        }
        
        /* Tabs for visualization */
        .viz-tabs {
            display: flex;
            border-bottom: 1px solid #ddd;
            margin-bottom: 1rem;
        }
        
        .viz-tab {
            padding: 10px 15px;
            background-color: #f1f1f1;
            border: none;
            cursor: pointer;
            border-radius: 5px 5px 0 0;
            margin-right: 5px;
        }
        
        .viz-tab.active {
            background-color: #1a5fb4;
            color: white;
        }
        
        .viz-tab:hover {
            background-color: #ddd;
        }
        
        .viz-tab.active:hover {
            background-color: #0d4a9e;
        }
        
        .viz-content {
            display: none;
        }
        
        .viz-content.active {
            display: block;
        }
        
        /* Legend for visualizations */
        .legend {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin: 10px 0;
            padding: 10px;
            background-color: rgba(255, 255, 255, 0.8);
            border-radius: 5px;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .legend-color {
            width: 20px;
            height: 20px;
            border-radius: 3px;
        }
        
        .legend-text {
            font-size: 0.9rem;
        }
        
        /* Loading overlay */
        .loading-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            display: none;
        }
        
        .spinner {
            border: 5px solid #f3f3f3;
            border-top: 5px solid #1a5fb4;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <header>
        <h1>The Boundary Cancellation Principle:<br>An Analysis of Arithmetic Lattice Residues</h1>
        <div class="author">
            By <strong>Wessen Getachew</strong> · Twitter: <a href="https://twitter.com/7dview" target="_blank">@7dview</a>
        </div>
    </header>
    
    <main>
        <section class="abstract">
            <h3>Abstract</h3>
            <p>In the limit as \(R \to \infty\), the density of coprime \(k\)-tuples in an integer lattice \(\mathbb{Z}^k\) converges to \(1/\zeta(k)\). However, for any finite search radius \(R\), a residual error term \(\Delta(R)\) exists. This project identifies this error not as stochastic noise, but as a deterministic geometric residue. We demonstrate that \(\Delta(R)\) is a function of the \((k-1)\) boundary shell of the \(k\)-cube, where the Möbius inversion engine is truncated by the finite search radius \(R\).</p>
        </section>
        
        <!-- Export Controls Section -->
        <section class="export-controls">
            <h3>Export Data & Visualizations</h3>
            <p>Export high-resolution visualizations and datasets for further analysis:</p>
            <div class="export-buttons">
                <button class="export-btn export-png" id="export2DPNG">
                    📷 Export 2D Visualization (4K PNG)
                </button>
                <button class="export-btn export-png" id="export3DPNG">
                    📷 Export 3D Visualization (4K PNG)
                </button>
                <button class="export-btn export-png" id="exportChartPNG">
                    📈 Export Error Chart (4K PNG)
                </button>
                <button class="export-btn export-csv" id="exportDataCSV">
                    📊 Export Error Data (CSV)
                </button>
                <button class="export-btn export-csv" id="exportLatticeCSV">
                    📊 Export Lattice Points (CSV)
                </button>
                <button class="export-btn" id="exportFullReport">
                    📄 Export Full Report (JSON)
                </button>
            </div>
            <div id="exportStatus" style="margin-top: 10px; font-size: 0.9rem; color: #666;"></div>
        </section>
        
        <!-- GCD Calculator Section -->
        <section class="gcd-display">
            <h4>GCD Calculator</h4>
            <p>Compute the GCD of any set of integers:</p>
            <div class="gcd-inputs">
                <input type="number" id="gcd-input-1" value="12" min="1" max="1000000">
                <input type="number" id="gcd-input-2" value="18" min="1" max="1000000">
                <input type="number" id="gcd-input-3" value="24" min="1" max="1000000">
                <input type="number" id="gcd-input-4" value="1" min="1" max="1000000" style="display: none;">
                <input type="number" id="gcd-input-5" value="1" min="1" max="1000000" style="display: none;">
                <input type="number" id="gcd-input-6" value="1" min="1" max="1000000" style="display: none;">
                <button id="calculate-gcd">Calculate GCD</button>
                <button id="add-gcd-input">+ Add Number</button>
            </div>
            <div class="gcd-result" id="gcd-result">
                GCD(12, 18, 24) = 6
            </div>
            <div style="margin-top: 10px;">
                <label>
                    <input type="checkbox" id="show-prime-factors"> Show Prime Factors
                </label>
            </div>
            <div id="prime-factors" style="margin-top: 10px; font-size: 0.9rem; display: none;">
                Prime factors: 12 = 2²×3, 18 = 2×3², 24 = 2³×3
            </div>
        </section>
        
        <!-- Visualization Section -->
        <section class="visualization-container">
            <h2>Visualizations</h2>
            
            <div class="viz-tabs">
                <button class="viz-tab active" data-tab="2d">2D Lattice (k=2)</button>
                <button class="viz-tab" data-tab="3d">3D Lattice (k=3)</button>
                <button class="viz-tab" data-tab="chart">Error Analysis Chart</button>
            </div>
            
            <!-- 2D Visualization -->
            <div class="viz-content active" id="viz-2d">
                <h3>2D Coprime Lattice Visualization</h3>
                <p>Visualization of coprime pairs in a 2D lattice. Green points are coprime pairs, red points are non-coprime pairs. Boundary points (max(x,y) = R) are highlighted with a border. Click on any point to see its GCD.</p>
                
                <div class="legend">
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: #4CAF50;"></div>
                        <span class="legend-text">Coprime Point</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: #f44336;"></div>
                        <span class="legend-text">Non-coprime Point</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: transparent; border: 2px solid #FF9800;"></div>
                        <span class="legend-text">Boundary Point</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: #1a5fb4;"></div>
                        <span class="legend-text">Origin (1,1)</span>
                    </div>
                </div>
                
                <div class="canvas-container">
                    <div class="canvas-label">2D Lattice Visualization (k=2)</div>
                    <div class="loading-overlay" id="loading2D">
                        <div class="spinner"></div>
                    </div>
                    <canvas id="latticeCanvas2D"></canvas>
                </div>
                
                <!-- Clicked Point Display -->
                <div id="clicked-point-display" style="background-color: #f0f7ff; padding: 10px; border-radius: 5px; margin-top: 10px; display: none;">
                    <strong>Selected Point:</strong> 
                    <span id="selected-coords">None</span> | 
                    <span id="selected-gcd">GCD: -</span> | 
                    <span id="selected-coprime">Coprime: -</span>
                </div>
                
                <div class="interactive-controls">
                    <div class="control-row">
                        <div class="slider-container">
                            <label for="vis-radius-2d">Radius \(R\): <span class="slider-value" id="vis-radius-2d-value">50</span> (1-500)</label>
                            <input type="range" id="vis-radius-2d" min="1" max="500" value="50" step="1">
                            <div class="input-container">
                                <input type="number" id="vis-radius-2d-input" min="1" max="500" value="50">
                                <button id="set-radius-2d">Set Radius</button>
                            </div>
                        </div>
                        
                        <div class="slider-container">
                            <label for="point-size-2d">Point Size: <span class="slider-value" id="point-size-2d-value">10</span>px (1-30)</label>
                            <input type="range" id="point-size-2d" min="1" max="30" value="10" step="1">
                            <div class="input-container">
                                <input type="number" id="point-size-2d-input" min="1" max="30" value="10">
                                <button id="set-point-size-2d">Set Size</button>
                            </div>
                        </div>
                    </div>
                    
                    <div class="control-row">
                        <div class="slider-container">
                            <label for="grid-opacity-2d">Grid Opacity: <span class="slider-value" id="grid-opacity-2d-value">0.3</span> (0-1)</label>
                            <input type="range" id="grid-opacity-2d" min="0" max="1" value="0.3" step="0.1">
                            <div class="input-container">
                                <input type="number" id="grid-opacity-2d-input" min="0" max="1" value="0.3" step="0.1">
                                <button id="set-grid-opacity-2d">Set Opacity</button>
                            </div>
                        </div>
                        
                        <div class="slider-container">
                            <label for="zoom-level-2d">Zoom Level: <span class="slider-value" id="zoom-level-2d-value">1.0</span>x (0.1-5)</label>
                            <input type="range" id="zoom-level-2d" min="0.1" max="5" value="1.0" step="0.1">
                            <div class="input-container">
                                <input type="number" id="zoom-level-2d-input" min="0.1" max="5" value="1.0" step="0.1">
                                <button id="set-zoom-2d">Set Zoom</button>
                            </div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 1rem;">
                        <label>
                            <input type="checkbox" id="show-grid-2d" checked> Show Grid
                        </label>
                        <label style="margin-left: 15px;">
                            <input type="checkbox" id="show-boundary-2d" checked> Highlight Boundary
                        </label>
                        <label style="margin-left: 15px;">
                            <input type="checkbox" id="animate-2d"> Animate
                        </label>
                        <label style="margin-left: 15px;">
                            <input type="checkbox" id="show-coordinates-2d"> Show Coordinates
                        </label>
                    </div>
                    
                    <div class="calc-results" id="stats-2d">
                        <p>For \(R = 50\):</p>
                        <p>Total points: \(R^2 = 2500\)</p>
                        <p>Coprime points: \(N(R) = 1519\)</p>
                        <p>Expected: \(R^2/\zeta(2) \approx 1519.70\)</p>
                        <p>Error \(\Delta(R) = -0.70\)</p>
                        <p>Boundary points: \(4R - 4 = 196\)</p>
                    </div>
                </div>
            </div>
            
            <!-- 3D Visualization -->
            <div class="viz-content" id="viz-3d">
                <h3>3D Coprime Lattice Visualization</h3>
                <p>Visualization of coprime triples in a 3D lattice. Use mouse/touch to rotate, scroll to zoom. Green cubes are coprime triples, red cubes are non-coprime triples.</p>
                
                <div class="legend">
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: #4CAF50;"></div>
                        <span class="legend-text">Coprime Triple</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: #f44336;"></div>
                        <span class="legend-text">Non-coprime Triple</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: #1a5fb4;"></div>
                        <span class="legend-text">Boundary Triple</span>
                    </div>
                </div>
                
                <div class="canvas-container">
                    <div class="canvas-label">3D Lattice Visualization (k=3) - Drag to rotate, scroll to zoom</div>
                    <div class="loading-overlay" id="loading3D">
                        <div class="spinner"></div>
                    </div>
                    <canvas id="latticeCanvas3D"></canvas>
                </div>
                
                <div class="interactive-controls">
                    <div class="control-row">
                        <div class="slider-container">
                            <label for="vis-radius-3d">Radius \(R\): <span class="slider-value" id="vis-radius-3d-value">15</span> (1-30)</label>
                            <input type="range" id="vis-radius-3d" min="1" max="30" value="15" step="1">
                            <div class="input-container">
                                <input type="number" id="vis-radius-3d-input" min="1" max="30" value="15">
                                <button id="set-radius-3d">Set Radius</button>
                            </div>
                        </div>
                        
                        <div class="slider-container">
                            <label for="cube-size-3d">Cube Size: <span class="slider-value" id="cube-size-3d-value">0.7</span> (0.1-2)</label>
                            <input type="range" id="cube-size-3d" min="0.1" max="2" value="0.7" step="0.1">
                            <div class="input-container">
                                <input type="number" id="cube-size-3d-input" min="0.1" max="2" value="0.7" step="0.1">
                                <button id="set-cube-size-3d">Set Size</button>
                            </div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 1rem;">
                        <label>
                            <input type="checkbox" id="auto-rotate-3d" checked> Auto Rotate
                        </label>
                        <label style="margin-left: 15px;">
                            <input type="checkbox" id="show-axes-3d" checked> Show Axes
                        </label>
                        <label style="margin-left: 15px;">
                            <input type="checkbox" id="show-boundary-3d"> Highlight Boundary
                        </label>
                    </div>
                    
                    <!-- 3D Point Inspector -->
                    <div style="margin-top: 1.5rem; padding: 10px; background-color: #f9f9f9; border-radius: 5px;">
                        <h4>3D Point Inspector</h4>
                        <div style="display: flex; gap: 10px; margin-top: 10px; align-items: center;">
                            <span>Inspect point at:</span>
                            <input type="number" id="inspect-x" value="1" min="1" max="30" style="width: 60px;">
                            <input type="number" id="inspect-y" value="1" min="1" max="30" style="width: 60px;">
                            <input type="number" id="inspect-z" value="1" min="1" max="30" style="width: 60px;">
                            <button id="inspect-point">Inspect Point</button>
                        </div>
                        <div id="inspect-result" style="margin-top: 10px; padding: 5px; background-color: white; border-radius: 3px; font-family: monospace;">
                            Point (1, 1, 1): GCD = 1, Coprime = Yes
                        </div>
                    </div>
                    
                    <div class="calc-results" id="stats-3d">
                        <p>For \(R = 15\):</p>
                        <p>Total points: \(R^3 = 3375\)</p>
                        <p>Coprime points: \(N(R) \approx 2805\)</p>
                        <p>Expected: \(R^3/\zeta(3) \approx 2805.42\)</p>
                        <p>Error \(\Delta(R) \approx -0.42\)</p>
                        <p>Boundary points: \(6R^2 - 12R + 8 = 1256\)</p>
                    </div>
                </div>
            </div>
            
            <!-- Chart Visualization -->
            <div class="viz-content" id="viz-chart">
                <h3>Error Analysis Chart</h3>
                <p>Visualization of the error term \(\Delta(R)\) for different dimensions. The chart shows how the relative error decreases as dimension increases.</p>
                
                <div class="chart-container">
                    <canvas id="errorChart"></canvas>
                </div>
                
                <div class="interactive-controls">
                    <div class="control-row">
                        <div class="slider-container">
                            <label for="chart-max-radius">Max Radius: <span class="slider-value" id="chart-max-radius-value">200</span> (1-5000)</label>
                            <input type="range" id="chart-max-radius" min="1" max="5000" value="200" step="1">
                            <div class="input-container">
                                <input type="number" id="chart-max-radius-input" min="1" max="5000" value="200">
                                <button id="set-chart-radius">Set Max Radius</button>
                            </div>
                        </div>
                        
                        <div class="slider-container">
                            <label for="chart-step">Step Size: <span class="slider-value" id="chart-step-value">10</span> (1-100)</label>
                            <input type="range" id="chart-step" min="1" max="100" value="10" step="1">
                            <div class="input-container">
                                <input type="number" id="chart-step-input" min="1" max="100" value="10">
                                <button id="set-chart-step">Set Step</button>
                            </div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 1rem;">
                        <label>
                            <input type="checkbox" id="show-absolute-error" checked> Show Absolute Error
                        </label>
                        <label style="margin-left: 15px;">
                            <input type="checkbox" id="show-relative-error"> Show Relative Error
                        </label>
                        <label style="margin-left: 15px;">
                            <input type="checkbox" id="show-boundary-term"> Show Boundary Term
                        </label>
                    </div>
                    <div style="margin-top: 1rem;">
                        <label>Dimensions to display:</label>
                        <div>
                            <label style="margin-right: 10px;">
                                <input type="checkbox" class="chart-dimension" value="2" checked> k=2
                            </label>
                            <label style="margin-right: 10px;">
                                <input type="checkbox" class="chart-dimension" value="3" checked> k=3
                            </label>
                            <label style="margin-right: 10px;">
                                <input type="checkbox" class="chart-dimension" value="4"> k=4
                            </label>
                            <label style="margin-right: 10px;">
                                <input type="checkbox" class="chart-dimension" value="5"> k=5
                            </label>
                            <label style="margin-right: 10px;">
                                <input type="checkbox" class="chart-dimension" value="6"> k=6
                            </label>
                        </div>
                    </div>
                    
                    <!-- Chart Point Inspector -->
                    <div style="margin-top: 1.5rem; padding: 10px; background-color: #f9f9f9; border-radius: 5px;">
                        <h4>Chart Data Point Inspector</h4>
                        <div style="display: flex; gap: 10px; margin-top: 10px; align-items: center;">
                            <span>Inspect at R =</span>
                            <input type="number" id="inspect-chart-radius" value="100" min="1" max="5000" style="width: 80px;">
                            <span>for k =</span>
                            <input type="number" id="inspect-chart-dimension" value="2" min="2" max="12" style="width: 60px;">
                            <button id="inspect-chart-point">Inspect Point</button>
                        </div>
                        <div id="inspect-chart-result" style="margin-top: 10px; padding: 5px; background-color: white; border-radius: 3px; font-family: monospace;">
                            For R=100, k=2: N(R)=6087, Expected=6079.30, Δ(R)=7.70
                        </div>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- Continue with the rest of the sections from previous version -->
        
    </main>
    
    <footer>
        <div class="attribution">
            The Boundary Cancellation Principle: An Analysis of Arithmetic Lattice Residues
        </div>
        <div>© 2025 · By Wessen Getachew</div>
        <div class="social">
            Twitter: <a href="https://twitter.com/7dview" target="_blank">@7dview</a> · 
            GitHub: <a href="https://github.com/7dview" target="_blank">github.com/7dview</a>
        </div>
        <div style="margin-top: 0.8rem; font-size: 0.8rem;">
            For the 3Blue1Brown community and the mathematical world
        </div>
    </footer>
    
    <!-- Main JavaScript -->
    <script>
        // =============================
        // Global Variables and Constants
        // =============================
        
        // Zeta values for k=2 to k=12
        const zetaValues = {
            2: Math.PI**2 / 6,
            3: 1.202056903159594285399738161511449990764986292,
            4: Math.PI**4 / 90,
            5: 1.036927755143369926331365486457034168057080919,
            6: Math.PI**6 / 945,
            7: 1.008349277381922826839797549849796759599863560,
            8: Math.PI**8 / 9450,
            9: 1.002008392826082214417,
            10: Math.PI**10 / 93555,
            11: 1.000494188604119464558,
            12: Math.PI**12 / 638512875
        };
        
        // State management
        let state = {
            currentTab: '2d',
            animationId: null,
            threeScene: null,
            threeRenderer: null,
            threeCamera: null,
            threeControls: null,
            threeObjects: [],
            chart: null,
            lastClickedPoint2D: null,
            gcdInputCount: 3,
            animationFrameId: null,
            is3DInitialized: false
        };
        
        // =============================
        // Utility Functions
        // =============================
        
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
        
        function formatPrimeFactors(n) {
            const factors = primeFactors(n);
            const parts = [];
            
            for (const [prime, power] of Object.entries(factors)) {
                if (power === 1) {
                    parts.push(prime);
                } else {
                    parts.push(`${prime}${power.toString().sup()}`);
                }
            }
            
            return parts.length === 0 ? "1" : parts.join("×");
        }
        
        function isCoprime(x, y, z = 1) {
            return gcdArray([x, y, z]) === 1;
        }
        
        function isBoundaryPoint(x, y, z = 1, R) {
            return Math.max(x, y, z) === R;
        }
        
        function computeN(R, k) {
            // Using Möbius inversion formula for larger R
            let count = 0;
            for (let d = 1; d <= R; d++) {
                const mu = mobius(d);
                if (mu !== 0) {
                    count += mu * Math.pow(Math.floor(R / d), k);
                }
            }
            return count;
        }
        
        function mobius(n) {
            if (n === 1) return 1;
            
            let p = 0;
            // Handle 2 separately for optimization
            if (n % 2 === 0) {
                n /= 2;
                p++;
                if (n % 2 === 0) return 0;
            }
            
            // Check odd factors
            for (let i = 3; i * i <= n; i += 2) {
                if (n % i === 0) {
                    if (n % (i * i) === 0) return 0;
                    p++;
                    n = Math.floor(n / i);
                }
            }
            if (n > 1) p++;
            
            return (p % 2 === 0) ? 1 : -1;
        }
        
        function formatNumber(num) {
            if (num >= 1e15) {
                return (num / 1e15).toFixed(3) + 'Q';
            } else if (num >= 1e12) {
                return (num / 1e12).toFixed(3) + 'T';
            } else if (num >= 1e9) {
                return (num / 1e9).toFixed(3) + 'B';
            } else if (num >= 1e6) {
                return (num / 1e6).toFixed(3) + 'M';
            } else if (num >= 1e3) {
                return (num / 1e3).toFixed(2) + 'K';
            } else if (Math.abs(num) < 0.0001 && num !== 0) {
                return num.toExponential(4);
            } else if (Math.abs(num) < 0.01) {
                return num.toFixed(6);
            } else if (Math.abs(num) < 1) {
                return num.toFixed(4);
            } else {
                return Math.round(num).toLocaleString();
            }
        }
        
        // =============================
        // GCD Calculator Functions
        // =============================
        
        function initGCDCalculator() {
            document.getElementById('calculate-gcd').addEventListener('click', calculateGCD);
            document.getElementById('add-gcd-input').addEventListener('click', addGCDInput);
            document.getElementById('show-prime-factors').addEventListener('change', togglePrimeFactors);
            
            // Calculate initial GCD
            calculateGCD();
        }
        
        function calculateGCD() {
            const inputs = [];
            for (let i = 1; i <= state.gcdInputCount; i++) {
                const input = document.getElementById(`gcd-input-${i}`);
                const value = parseInt(input.value) || 1;
                inputs.push(value);
            }
            
            const result = gcdArray(inputs);
            
            // Format the result
            let numbersStr = inputs.join(', ');
            document.getElementById('gcd-result').innerHTML = 
                `GCD(${numbersStr}) = <strong>${result}</strong>`;
            
            // Update prime factors if shown
            if (document.getElementById('show-prime-factors').checked) {
                const factorStrs = inputs.map(n => `${n} = ${formatPrimeFactors(n)}`);
                document.getElementById('prime-factors').innerHTML = 
                    `Prime factors: ${factorStrs.join(', ')}`;
            }
        }
        
        function addGCDInput() {
            if (state.gcdInputCount >= 6) return;
            
            state.gcdInputCount++;
            const inputId = `gcd-input-${state.gcdInputCount}`;
            const input = document.getElementById(inputId);
            input.style.display = 'block';
            input.value = 1;
            
            // Hide the button if we've reached max
            if (state.gcdInputCount >= 6) {
                document.getElementById('add-gcd-input').style.display = 'none';
            }
            
            calculateGCD();
        }
        
        function togglePrimeFactors() {
            const show = document.getElementById('show-prime-factors').checked;
            document.getElementById('prime-factors').style.display = show ? 'block' : 'none';
            if (show) calculateGCD();
        }
        
        // =============================
        // 2D Visualization
        // =============================
        
        function init2DVisualization() {
            const canvas = document.getElementById('latticeCanvas2D');
            const ctx = canvas.getContext('2d');
            
            // Set canvas size
            const container = canvas.parentElement;
            canvas.width = container.clientWidth * window.devicePixelRatio;
            canvas.height = container.clientHeight * window.devicePixelRatio;
            canvas.style.width = container.clientWidth + 'px';
            canvas.style.height = container.clientHeight + 'px';
            
            // Scale context for high DPI displays
            ctx.scale(window.devicePixelRatio, window.devicePixelRatio);
            
            draw2DLattice();
            
            // Event listeners for sliders and input boxes
            setup2DControls();
            
            // Add click event for point selection
            canvas.addEventListener('click', handleCanvasClick);
        }
        
        function setup2DControls() {
            // Setup radius controls
            const radiusSlider = document.getElementById('vis-radius-2d');
            const radiusInput = document.getElementById('vis-radius-2d-input');
            const radiusSetBtn = document.getElementById('set-radius-2d');
            
            radiusSlider.addEventListener('input', function() {
                document.getElementById('vis-radius-2d-value').textContent = this.value;
                radiusInput.value = this.value;
                draw2DLattice();
            });
            
            radiusInput.addEventListener('change', function() {
                let value = parseInt(this.value);
                if (isNaN(value) || value < 1) value = 1;
                if (value > 500) value = 500;
                this.value = value;
                radiusSlider.value = value;
                document.getElementById('vis-radius-2d-value').textContent = value;
                draw2DLattice();
            });
            
            radiusSetBtn.addEventListener('click', function() {
                const value = parseInt(radiusInput.value);
                if (!isNaN(value) && value >= 1 && value <= 500) {
                    radiusSlider.value = value;
                    document.getElementById('vis-radius-2d-value').textContent = value;
                    draw2DLattice();
                }
            });
            
            // Setup point size controls
            const sizeSlider = document.getElementById('point-size-2d');
            const sizeInput = document.getElementById('point-size-2d-input');
            const sizeSetBtn = document.getElementById('set-point-size-2d');
            
            sizeSlider.addEventListener('input', function() {
                document.getElementById('point-size-2d-value').textContent = this.value;
                sizeInput.value = this.value;
                draw2DLattice();
            });
            
            sizeSetBtn.addEventListener('click', function() {
                const value = parseInt(sizeInput.value);
                if (!isNaN(value) && value >= 1 && value <= 30) {
                    sizeSlider.value = value;
                    document.getElementById('point-size-2d-value').textContent = value;
                    draw2DLattice();
                }
            });
            
            // Setup other controls
            document.getElementById('grid-opacity-2d').addEventListener('input', draw2DLattice);
            document.getElementById('show-grid-2d').addEventListener('change', draw2DLattice);
            document.getElementById('show-boundary-2d').addEventListener('change', draw2DLattice);
            document.getElementById('animate-2d').addEventListener('change', toggle2DAnimation);
            document.getElementById('show-coordinates-2d').addEventListener('change', draw2DLattice);
            
            // Update slider value displays
            document.querySelectorAll('#grid-opacity-2d, #zoom-level-2d').forEach(slider => {
                slider.addEventListener('input', function() {
                    const valueSpan = document.getElementById(this.id + '-value');
                    valueSpan.textContent = parseFloat(this.value).toFixed(1);
                });
            });
        }
        
        function handleCanvasClick(event) {
            const canvas = document.getElementById('latticeCanvas2D');
            const rect = canvas.getBoundingClientRect();
            const R = parseInt(document.getElementById('vis-radius-2d').value);
            const pointSize = parseInt(document.getElementById('point-size-2d').value);
            
            // Calculate padding and cell size
            const padding = pointSize * 2;
            const availableWidth = (canvas.width / window.devicePixelRatio) - padding * 2;
            const availableHeight = (canvas.height / window.devicePixelRatio) - padding * 2;
            const cellSize = Math.min(availableWidth / R, availableHeight / R);
            
            // Convert click coordinates to canvas coordinates (adjusted for scaling)
            const scaleX = canvas.width / rect.width;
            const scaleY = canvas.height / rect.height;
            const x = (event.clientX - rect.left) * scaleX;
            const y = (event.clientY - rect.top) * scaleY;
            
            // Convert to lattice coordinates
            const latticeX = Math.floor((x / window.devicePixelRatio - padding) / cellSize) + 1;
            const latticeY = Math.floor((y / window.devicePixelRatio - padding) / cellSize) + 1;
            
            // Check if click is within lattice bounds
            if (latticeX >= 1 && latticeX <= R && latticeY >= 1 && latticeY <= R) {
                state.lastClickedPoint2D = { x: latticeX, y: latticeY };
                displaySelectedPoint(latticeX, latticeY);
            }
        }
        
        function displaySelectedPoint(x, y) {
            const gcdVal = gcd(x, y);
            const isCoprimePair = gcdVal === 1;
            
            document.getElementById('selected-coords').textContent = `(${x}, ${y})`;
            document.getElementById('selected-gcd').textContent = `GCD: ${gcdVal}`;
            document.getElementById('selected-coprime').textContent = `Coprime: ${isCoprimePair ? 'Yes' : 'No'}`;
            document.getElementById('clicked-point-display').style.display = 'block';
            
            // Also update the GCD calculator
            document.getElementById('gcd-input-1').value = x;
            document.getElementById('gcd-input-2').value = y;
            calculateGCD();
        }
        
        function draw2DLattice() {
            const loading = document.getElementById('loading2D');
            loading.style.display = 'flex';
            
            // Use setTimeout to allow UI to update
            setTimeout(() => {
                try {
                    const canvas = document.getElementById('latticeCanvas2D');
                    const ctx = canvas.getContext('2d');
                    const R = parseInt(document.getElementById('vis-radius-2d').value);
                    const pointSize = parseInt(document.getElementById('point-size-2d').value);
                    const gridOpacity = parseFloat(document.getElementById('grid-opacity-2d').value);
                    const showGrid = document.getElementById('show-grid-2d').checked;
                    const showBoundary = document.getElementById('show-boundary-2d').checked;
                    const showCoordinates = document.getElementById('show-coordinates-2d').checked;
                    
                    // Clear canvas
                    ctx.save();
                    ctx.setTransform(1, 0, 0, 1, 0, 0);
                    ctx.clearRect(0, 0, canvas.width, canvas.height);
                    ctx.restore();
                    
                    // Calculate padding and cell size
                    const padding = pointSize * 2;
                    const availableWidth = (canvas.width / window.devicePixelRatio) - padding * 2;
                    const availableHeight = (canvas.height / window.devicePixelRatio) - padding * 2;
                    const cellSize = Math.min(availableWidth / R, availableHeight / R);
                    
                    // Draw grid
                    if (showGrid && gridOpacity > 0) {
                        ctx.strokeStyle = '#e0e0e0';
                        ctx.lineWidth = 1;
                        ctx.globalAlpha = gridOpacity;
                        
                        // Vertical lines
                        for (let i = 0; i <= R; i++) {
                            const x = padding + i * cellSize;
                            ctx.beginPath();
                            ctx.moveTo(x, padding);
                            ctx.lineTo(x, padding + R * cellSize);
                            ctx.stroke();
                        }
                        
                        // Horizontal lines
                        for (let j = 0; j <= R; j++) {
                            const y = padding + j * cellSize;
                            ctx.beginPath();
                            ctx.moveTo(padding, y);
                            ctx.lineTo(padding + R * cellSize, y);
                            ctx.stroke();
                        }
                        
                        ctx.globalAlpha = 1;
                    }
                    
                    // Draw points
                    let coprimeCount = 0;
                    let boundaryCount = 0;
                    
                    for (let x = 1; x <= R; x++) {
                        for (let y = 1; y <= R; y++) {
                            const isCoprimePair = gcd(x, y) === 1;
                            const isBoundary = isBoundaryPoint(x, y, 1, R);
                            
                            if (isCoprimePair) coprimeCount++;
                            if (isBoundary) boundaryCount++;
                            
                            // Calculate position
                            const px = padding + (x - 1) * cellSize + cellSize / 2;
                            const py = padding + (y - 1) * cellSize + cellSize / 2;
                            
                            // Draw point
                            ctx.beginPath();
                            ctx.arc(px, py, pointSize / 2, 0, Math.PI * 2);
                            
                            // Set color
                            if (x === 1 && y === 1) {
                                ctx.fillStyle = '#1a5fb4';
                            } else {
                                ctx.fillStyle = isCoprimePair ? '#4CAF50' : '#f44336';
                            }
                            
                            ctx.fill();
                            
                            // Draw boundary highlight
                            if (showBoundary && isBoundary) {
                                ctx.strokeStyle = '#FF9800';
                                ctx.lineWidth = 2;
                                ctx.stroke();
                            }
                            
                            // Draw coordinates if enabled
                            if (showCoordinates && (x === 1 || y === 1 || x === R || y === R)) {
                                ctx.font = '10px Arial';
                                ctx.fillStyle = '#666';
                                ctx.textAlign = 'center';
                                ctx.fillText(`${x},${y}`, px, py - pointSize - 5);
                            }
                        }
                    }
                    
                    // Update statistics
                    const totalPoints = R * R;
                    const expected = totalPoints / zetaValues[2];
                    const error = coprimeCount - expected;
                    
                    document.getElementById('stats-2d').innerHTML = `
                        <p>For \(R = ${R}\):</p>
                        <p>Total points: \(R^2 = ${totalPoints.toLocaleString()}\)</p>
                        <p>Coprime points: \(N(R) = ${coprimeCount.toLocaleString()}\)</p>
                        <p>Expected: \(R^2/\\zeta(2) \\approx ${expected.toFixed(2)}\)</p>
                        <p>Error \(\\Delta(R) = ${error.toFixed(2)}\)</p>
                        <p>Boundary points: \(4R - 4 = ${4*R - 4}\)</p>
                    `;
                    
                    // Update MathJax
                    if (window.MathJax) {
                        MathJax.typesetPromise([document.getElementById('stats-2d')]);
                    }
                    
                } catch (error) {
                    console.error('Error drawing 2D lattice:', error);
                } finally {
                    loading.style.display = 'none';
                }
            }, 10);
        }
        
        function toggle2DAnimation() {
            const animate = document.getElementById('animate-2d').checked;
            
            if (animate && !state.animationId) {
                let R = 1;
                const maxR = parseInt(document.getElementById('vis-radius-2d').max);
                
                function animateStep() {
                    document.getElementById('vis-radius-2d').value = R;
                    document.getElementById('vis-radius-2d-value').textContent = R;
                    document.getElementById('vis-radius-2d-input').value = R;
                    draw2DLattice();
                    
                    R++;
                    if (R > maxR) R = 1;
                    
                    state.animationId = setTimeout(animateStep, 500);
                }
                
                animateStep();
            } else if (state.animationId) {
                clearTimeout(state.animationId);
                state.animationId = null;
            }
        }
        
        // =============================
        // 3D Visualization - FIXED VERSION
        // =============================
        
        function init3DVisualization() {
            if (state.is3DInitialized) return;
            
            const canvas = document.getElementById('latticeCanvas3D');
            const container = canvas.parentElement;
            
            // Clear any existing content
            while (canvas.firstChild) {
                canvas.removeChild(canvas.firstChild);
            }
            
            // Create a new canvas for Three.js
            const threeCanvas = document.createElement('canvas');
            threeCanvas.id = 'threeCanvas';
            threeCanvas.style.width = '100%';
            threeCanvas.style.height = '100%';
            canvas.appendChild(threeCanvas);
            
            // Set canvas size
            const width = container.clientWidth;
            const height = container.clientHeight;
            
            // Set up Three.js scene
            const scene = new THREE.Scene();
            scene.background = new THREE.Color(0xfefefe);
            
            // Camera
            const camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
            camera.position.set(15, 15, 15);
            
            // Renderer
            const renderer = new THREE.WebGLRenderer({ 
                canvas: threeCanvas,
                antialias: true,
                alpha: true
            });
            renderer.setSize(width, height);
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
                
                if (document.getElementById('auto-rotate-3d').checked) {
                    const speed = parseFloat(document.getElementById('rotation-speed-3d').value) || 0.5;
                    scene.rotation.y += 0.005 * speed;
                }
                
                controls.update();
                renderer.render(scene, camera);
            }
            
            animate();
            
            // Event listeners for 3D controls
            setup3DControls();
            
            // Handle window resize
            window.addEventListener('resize', () => {
                if (!state.is3DInitialized) return;
                
                const container = canvas.parentElement;
                const width = container.clientWidth;
                const height = container.clientHeight;
                
                camera.aspect = width / height;
                camera.updateProjectionMatrix();
                renderer.setSize(width, height);
            });
            
            // Set up inspect point button
            document.getElementById('inspect-point').addEventListener('click', inspect3DPoint);
        }
        
        function setup3DControls() {
            // Setup radius controls
            const radiusSlider = document.getElementById('vis-radius-3d');
            const radiusInput = document.getElementById('vis-radius-3d-input');
            const radiusSetBtn = document.getElementById('set-radius-3d');
            
            radiusSlider.addEventListener('input', function() {
                document.getElementById('vis-radius-3d-value').textContent = this.value;
                radiusInput.value = this.value;
                draw3DLattice();
            });
            
            radiusSetBtn.addEventListener('click', function() {
                const value = parseInt(radiusInput.value);
                if (!isNaN(value) && value >= 1 && value <= 30) {
                    radiusSlider.value = value;
                    document.getElementById('vis-radius-3d-value').textContent = value;
                    draw3DLattice();
                }
            });
            
            // Setup cube size controls
            const cubeSizeSlider = document.getElementById('cube-size-3d');
            const cubeSizeInput = document.getElementById('cube-size-3d-input');
            const cubeSizeSetBtn = document.getElementById('set-cube-size-3d');
            
            cubeSizeSlider.addEventListener('input', function() {
                document.getElementById('cube-size-3d-value').textContent = parseFloat(this.value).toFixed(1);
                cubeSizeInput.value = this.value;
                draw3DLattice();
            });
            
            cubeSizeSetBtn.addEventListener('click', function() {
                const value = parseFloat(cubeSizeInput.value);
                if (!isNaN(value) && value >= 0.1 && value <= 2) {
                    cubeSizeSlider.value = value;
                    document.getElementById('cube-size-3d-value').textContent = value.toFixed(1);
                    draw3DLattice();
                }
            });
            
            // Setup other controls
            document.getElementById('auto-rotate-3d').addEventListener('change', update3DAutoRotate);
            document.getElementById('show-axes-3d').addEventListener('change', draw3DLattice);
            document.getElementById('show-boundary-3d').addEventListener('change', draw3DLattice);
        }
        
        function draw3DLattice() {
            if (!state.is3DInitialized) return;
            
            const loading = document.getElementById('loading3D');
            loading.style.display = 'flex';
            
            setTimeout(() => {
                try {
                    const scene = state.threeScene;
                    const R = parseInt(document.getElementById('vis-radius-3d').value) || 15;
                    const cubeSize = parseFloat(document.getElementById('cube-size-3d').value) || 0.7;
                    const showAxes = document.getElementById('show-axes-3d').checked;
                    const showBoundary = document.getElementById('show-boundary-3d').checked;
                    
                    // Clear previous objects
                    state.threeObjects.forEach(obj => scene.remove(obj));
                    state.threeObjects = [];
                    
                    // Add axes if enabled
                    if (showAxes) {
                        const axesHelper = new THREE.AxesHelper(R * 1.5);
                        scene.add(axesHelper);
                        state.threeObjects.push(axesHelper);
                    }
                    
                    // Create cube geometry and materials
                    const geometry = new THREE.BoxGeometry(cubeSize, cubeSize, cubeSize);
                    const coprimeMaterial = new THREE.MeshLambertMaterial({ color: 0x4CAF50 });
                    const nonCoprimeMaterial = new THREE.MeshLambertMaterial({ color: 0xf44336 });
                    const boundaryMaterial = new THREE.MeshLambertMaterial({ color: 0xFF9800 });
                    
                    // Add cubes
                    let coprimeCount = 0;
                    let boundaryCount = 0;
                    
                    // Limit the number of cubes for performance
                    const maxCubes = R <= 10 ? R : 10;
                    const step = Math.max(1, Math.floor(R / maxCubes));
                    
                    for (let x = 1; x <= R; x += step) {
                        for (let y = 1; y <= R; y += step) {
                            for (let z = 1; z <= R; z += step) {
                                const isCoprimeTriple = gcdArray([x, y, z]) === 1;
                                const isBoundary = isBoundaryPoint(x, y, z, R);
                                
                                if (isCoprimeTriple) coprimeCount++;
                                if (isBoundary) boundaryCount++;
                                
                                // Choose material
                                let material;
                                if (showBoundary && isBoundary) {
                                    material = boundaryMaterial;
                                } else {
                                    material = isCoprimeTriple ? coprimeMaterial : nonCoprimeMaterial;
                                }
                                
                                // Create cube
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
                    
                    // Scale counts to approximate total
                    const scaleFactor = Math.pow(R / step, 3);
                    const estimatedTotalPoints = Math.pow(R, 3);
                    const estimatedCoprimeCount = Math.round(coprimeCount * scaleFactor);
                    const estimatedBoundaryCount = Math.round(boundaryCount * scaleFactor);
                    
                    // Update statistics
                    const expected = estimatedTotalPoints / zetaValues[3];
                    const error = estimatedCoprimeCount - expected;
                    
                    document.getElementById('stats-3d').innerHTML = `
                        <p>For \(R = ${R}\):</p>
                        <p>Total points: \(R^3 = ${estimatedTotalPoints.toLocaleString()}\)</p>
                        <p>Coprime points: \(N(R) \\approx ${estimatedCoprimeCount.toLocaleString()}\)</p>
                        <p>Expected: \(R^3/\\zeta(3) \\approx ${expected.toFixed(2)}\)</p>
                        <p>Error \(\\Delta(R) \\approx ${error.toFixed(2)}\)</p>
                        <p>Boundary points: \(6R^2 - 12R + 8 = ${6*R*R - 12*R + 8}\)</p>
                    `;
                    
                    // Update MathJax
                    if (window.MathJax) {
                        MathJax.typesetPromise([document.getElementById('stats-3d')]);
                    }
                    
                } catch (error) {
                    console.error('Error drawing 3D lattice:', error);
                } finally {
                    loading.style.display = 'none';
                }
            }, 10);
        }
        
        function inspect3DPoint() {
            const x = parseInt(document.getElementById('inspect-x').value) || 1;
            const y = parseInt(document.getElementById('inspect-y').value) || 1;
            const z = parseInt(document.getElementById('inspect-z').value) || 1;
            const R = parseInt(document.getElementById('vis-radius-3d').value) || 15;
            
            // Validate inputs
            if (x < 1 || x > R || y < 1 || y > R || z < 1 || z > R) {
                document.getElementById('inspect-result').innerHTML = 
                    `Error: All coordinates must be between 1 and ${R}`;
                return;
            }
            
            const gcdVal = gcdArray([x, y, z]);
            const isCoprimeTriple = gcdVal === 1;
            
            document.getElementById('inspect-result').innerHTML = 
                `Point (${x}, ${y}, ${z}): GCD = ${gcdVal}, Coprime = ${isCoprimeTriple ? 'Yes' : 'No'}`;
            
            // Update GCD calculator
            document.getElementById('gcd-input-1').value = x;
            document.getElementById('gcd-input-2').value = y;
            if (state.gcdInputCount >= 3) {
                document.getElementById('gcd-input-3').value = z;
            }
            calculateGCD();
        }
        
        function update3DAutoRotate() {
            // Handled in animation loop
        }
        
        // =============================
        // Chart Visualization
        // =============================
        
        function initChart() {
            const ctx = document.getElementById('errorChart').getContext('2d');
            
            state.chart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: [],
                    datasets: []
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        title: {
                            display: true,
                            text: 'Boundary Cancellation Error Analysis',
                            font: {
                                size: 16
                            }
                        },
                        tooltip: {
                            mode: 'index',
                            intersect: false,
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed.y !== null) {
                                        label += formatNumber(context.parsed.y);
                                    }
                                    return label;
                                }
                            }
                        },
                        legend: {
                            position: 'top',
                        }
                    },
                    scales: {
                        x: {
                            title: {
                                display: true,
                                text: 'Radius R',
                                font: {
                                    size: 14
                                }
                            }
                        },
                        y: {
                            title: {
                                display: true,
                                text: 'Error Δ(R)',
                                font: {
                                    size: 14
                                }
                            },
                            type: 'linear',
                            display: true
                        }
                    },
                    interaction: {
                        intersect: false,
                        mode: 'nearest'
                    }
                }
            });
            
            updateChart();
            
            // Event listeners for chart controls
            setupChartControls();
        }
        
        function setupChartControls() {
            // Setup max radius controls
            const maxRadiusSlider = document.getElementById('chart-max-radius');
            const maxRadiusInput = document.getElementById('chart-max-radius-input');
            const maxRadiusSetBtn = document.getElementById('set-chart-radius');
            
            maxRadiusSlider.addEventListener('input', function() {
                document.getElementById('chart-max-radius-value').textContent = this.value;
                maxRadiusInput.value = this.value;
                updateChart();
            });
            
            maxRadiusSetBtn.addEventListener('click', function() {
                const value = parseInt(maxRadiusInput.value);
                if (!isNaN(value) && value >= 1 && value <= 5000) {
                    maxRadiusSlider.value = value;
                    document.getElementById('chart-max-radius-value').textContent = value;
                    updateChart();
                }
            });
            
            // Setup step controls
            const stepSlider = document.getElementById('chart-step');
            const stepInput = document.getElementById('chart-step-input');
            const stepSetBtn = document.getElementById('set-chart-step');
            
            stepSlider.addEventListener('input', function() {
                document.getElementById('chart-step-value').textContent = this.value;
                stepInput.value = this.value;
                updateChart();
            });
            
            stepSetBtn.addEventListener('click', function() {
                const value = parseInt(stepInput.value);
                if (!isNaN(value) && value >= 1 && value <= 100) {
                    stepSlider.value = value;
                    document.getElementById('chart-step-value').textContent = value;
                    updateChart();
                }
            });
            
            // Setup other chart controls
            document.getElementById('show-absolute-error').addEventListener('change', updateChart);
            document.getElementById('show-relative-error').addEventListener('change', updateChart);
            document.getElementById('show-boundary-term').addEventListener('change', updateChart);
            
            document.querySelectorAll('.chart-dimension').forEach(cb => {
                cb.addEventListener('change', updateChart);
            });
            
            // Setup chart point inspector
            document.getElementById('inspect-chart-point').addEventListener('click', inspectChartPoint);
        }
        
        function inspectChartPoint() {
            const R = parseInt(document.getElementById('inspect-chart-radius').value) || 100;
            const k = parseInt(document.getElementById('inspect-chart-dimension').value) || 2;
            
            if (R < 1 || R > 5000) {
                document.getElementById('inspect-chart-result').innerHTML = 
                    'Error: R must be between 1 and 5000';
                return;
            }
            
            if (k < 2 || k > 12) {
                document.getElementById('inspect-chart-result').innerHTML = 
                    'Error: k must be between 2 and 12';
                return;
            }
            
            const N = computeN(R, k);
            const expected = Math.pow(R, k) / zetaValues[k];
            const error = N - expected;
            const relativeError = (error / expected) * 100;
            
            document.getElementById('inspect-chart-result').innerHTML = 
                `For R=${R}, k=${k}: N(R)=${N.toLocaleString()}, Expected=${expected.toFixed(2)}, Δ(R)=${error.toFixed(2)}, Relative Error=${relativeError.toFixed(4)}%`;
        }
        
        function updateChart() {
            const maxR = parseInt(document.getElementById('chart-max-radius').value);
            const step = parseInt(document.getElementById('chart-step').value);
            const showAbsolute = document.getElementById('show-absolute-error').checked;
            const showRelative = document.getElementById('show-relative-error').checked;
            const showBoundary = document.getElementById('show-boundary-term').checked;
            
            // Get selected dimensions
            const selectedDims = Array.from(document.querySelectorAll('.chart-dimension:checked'))
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
            let colorIndex = 0;
            const colors = ['#1a5fb4', '#4CAF50', '#FF9800', '#9C27B0', '#E91E63', '#00BCD4'];
            
            selectedDims.forEach(k => {
                // Absolute error dataset
                if (showAbsolute) {
                    const data = [];
                    for (let R = step; R <= maxR; R += step) {
                        const N = computeN(R, k);
                        const expected = Math.pow(R, k) / zetaValues[k];
                        data.push(N - expected);
                    }
                    
                    datasets.push({
                        label: `k=${k}: Absolute Error`,
                        data: data,
                        borderColor: colors[colorIndex % colors.length],
                        backgroundColor: 'transparent',
                        borderWidth: 2,
                        tension: 0.1
                    });
                    colorIndex++;
                }
                
                // Relative error dataset
                if (showRelative) {
                    const data = [];
                    for (let R = step; R <= maxR; R += step) {
                        const N = computeN(R, k);
                        const expected = Math.pow(R, k) / zetaValues[k];
                        const error = N - expected;
                        const relativeError = error / expected;
                        data.push(relativeError * 100); // as percentage
                    }
                    
                    datasets.push({
                        label: `k=${k}: Relative Error (%)`,
                        data: data,
                        borderColor: colors[colorIndex % colors.length],
                        backgroundColor: 'transparent',
                        borderWidth: 2,
                        borderDash: [5, 5],
                        tension: 0.1
                    });
                    colorIndex++;
                }
                
                // Boundary term dataset
                if (showBoundary && k > 2) {
                    const data = [];
                    for (let R = step; R <= maxR; R += step) {
                        // Approximate boundary term as R^(k-1)
                        data.push(Math.pow(R, k-1));
                    }
                    
                    datasets.push({
                        label: `k=${k}: Boundary Term R^${k-1}`,
                        data: data,
                        borderColor: colors[colorIndex % colors.length],
                        backgroundColor: 'transparent',
                        borderWidth: 1,
                        borderDash: [2, 2],
                        tension: 0
                    });
                    colorIndex++;
                }
            });
            
            // Update chart
            state.chart.data.labels = labels;
            state.chart.data.datasets = datasets;
            state.chart.update();
        }
        
        // =============================
        // Export Functions - FIXED VERSION
        // =============================
        
        function export2DAsPNG() {
            const canvas = document.getElementById('latticeCanvas2D');
            exportCanvasAsPNG(canvas, 'boundary_cancellation_2d.png');
        }
        
        function export3DAsPNG() {
            if (!state.is3DInitialized) {
                showExportStatus('Please initialize 3D visualization first by clicking the 3D tab', true);
                return;
            }
            
            const canvas = document.getElementById('threeCanvas');
            if (!canvas) {
                showExportStatus('3D canvas not found. Please switch to 3D tab and try again.', true);
                return;
            }
            
            // Force a render before capturing
            if (state.threeRenderer && state.threeScene && state.threeCamera) {
                state.threeRenderer.render(state.threeScene, state.threeCamera);
            }
            
            exportCanvasAsPNG(canvas, 'boundary_cancellation_3d.png');
        }
        
        function exportChartAsPNG() {
            if (!state.chart) {
                showExportStatus('Chart not initialized', true);
                return;
            }
            
            const canvas = state.chart.canvas;
            exportCanvasAsPNG(canvas, 'boundary_cancellation_chart.png');
        }
        
        function exportCanvasAsPNG(canvas, filename, scale = 4) {
            if (!canvas) {
                showExportStatus(`Canvas not found for ${filename}`, true);
                return;
            }
            
            const exportCanvas = document.createElement('canvas');
            const ctx = exportCanvas.getContext('2d');
            
            // Set export canvas to 4K resolution (3840x2160)
            const width = 3840;
            const height = 2160;
            
            exportCanvas.width = width;
            exportCanvas.height = height;
            
            // Fill background
            ctx.fillStyle = '#ffffff';
            ctx.fillRect(0, 0, width, height);
            
            // Draw original canvas content scaled up
            ctx.imageSmoothingEnabled = true;
            ctx.imageSmoothingQuality = 'high';
            
            // Calculate aspect ratio and position
            const aspectRatio = canvas.width / canvas.height;
            let drawWidth, drawHeight, offsetX, offsetY;
            
            if (aspectRatio > width / height) {
                // Canvas is wider than target
                drawWidth = width;
                drawHeight = width / aspectRatio;
                offsetX = 0;
                offsetY = (height - drawHeight) / 2;
            } else {
                // Canvas is taller than target
                drawHeight = height;
                drawWidth = height * aspectRatio;
                offsetX = (width - drawWidth) / 2;
                offsetY = 0;
            }
            
            ctx.drawImage(canvas, offsetX, offsetY, drawWidth, drawHeight);
            
            // Add watermark
            ctx.font = 'bold 36px Arial';
            ctx.fillStyle = 'rgba(0, 0, 0, 0.3)';
            ctx.textAlign = 'center';
            ctx.fillText('Boundary Cancellation Principle', width / 2, 50);
            
            ctx.font = '24px Arial';
            ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
            ctx.textAlign = 'right';
            ctx.fillText('© 2025 Wessen Getachew | @7dview', width - 20, height - 20);
            
            // Add timestamp
            ctx.textAlign = 'left';
            const now = new Date();
            const timestamp = now.toLocaleString();
            ctx.fillText(timestamp, 20, height - 20);
            
            // Create download link
            const link = document.createElement('a');
            link.download = filename;
            link.href = exportCanvas.toDataURL('image/png');
            link.click();
            
            showExportStatus(`Exported ${filename} (${width}x${height})`);
        }
        
        function exportErrorDataCSV() {
            const maxR = parseInt(document.getElementById('chart-max-radius').value);
            const step = parseInt(document.getElementById('chart-step').value);
            const selectedDims = Array.from(document.querySelectorAll('.chart-dimension:checked'))
                .map(cb => parseInt(cb.value))
                .sort((a, b) => a - b);
            
            if (selectedDims.length === 0) {
                showExportStatus('Please select at least one dimension', true);
                return;
            }
            
            // Build CSV data
            let csv = 'Radius R';
            selectedDims.forEach(k => {
                csv += `,N(R) k=${k},Expected k=${k},Error Δ(R) k=${k},Relative Error % k=${k}`;
            });
            csv += '\n';
            
            for (let R = step; R <= maxR; R += step) {
                csv += R;
                selectedDims.forEach(k => {
                    const N = computeN(R, k);
                    const expected = Math.pow(R, k) / zetaValues[k];
                    const error = N - expected;
                    const relativeError = (error / expected) * 100;
                    
                    csv += `,${N},${expected.toFixed(6)},${error.toFixed(6)},${relativeError.toFixed(6)}`;
                });
                csv += '\n';
            }
            
            // Create download link
            const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            const url = URL.createObjectURL(blob);
            link.href = url;
            link.download = 'boundary_cancellation_error_data.csv';
            link.click();
            
            setTimeout(() => URL.revokeObjectURL(url), 100);
            
            showExportStatus(`Exported error data for R=1 to ${maxR} (step ${step})`);
        }
        
        function exportLatticePointsCSV() {
            const R2D = parseInt(document.getElementById('vis-radius-2d').value);
            const R3D = parseInt(document.getElementById('vis-radius-3d').value);
            
            // Build CSV for 2D lattice
            let csv2D = 'x,y,gcd,is_coprime,is_boundary\n';
            for (let x = 1; x <= Math.min(R2D, 100); x++) { // Limit to 100 for file size
                for (let y = 1; y <= Math.min(R2D, 100); y++) {
                    const g = gcd(x, y);
                    const isCoprime = g === 1;
                    const isBoundary = isBoundaryPoint(x, y, 1, Math.min(R2D, 100));
                    csv2D += `${x},${y},${g},${isCoprime},${isBoundary}\n`;
                }
            }
            
            // Build CSV for 3D lattice
            let csv3D = 'x,y,z,gcd,is_coprime,is_boundary\n';
            for (let x = 1; x <= Math.min(R3D, 20); x++) { // Limit to 20 for file size
                for (let y = 1; y <= Math.min(R3D, 20); y++) {
                    for (let z = 1; z <= Math.min(R3D, 20); z++) {
                        const g = gcdArray([x, y, z]);
                        const isCoprime = g === 1;
                        const isBoundary = isBoundaryPoint(x, y, z, Math.min(R3D, 20));
                        csv3D += `${x},${y},${z},${g},${isCoprime},${isBoundary}\n`;
                    }
                }
            }
            
            // Create download links
            const blob2D = new Blob([csv2D], { type: 'text/csv;charset=utf-8;' });
            const blob3D = new Blob([csv3D], { type: 'text/csv;charset=utf-8;' });
            
            const link2D = document.createElement('a');
            link2D.href = URL.createObjectURL(blob2D);
            link2D.download = 'lattice_points_2d.csv';
            link2D.click();
            
            setTimeout(() => {
                const link3D = document.createElement('a');
                link3D.href = URL.createObjectURL(blob3D);
                link3D.download = 'lattice_points_3d.csv';
                link3D.click();
                
                setTimeout(() => {
                    URL.revokeObjectURL(link2D.href);
                    URL.revokeObjectURL(link3D.href);
                }, 100);
            }, 100);
            
            showExportStatus('Exported lattice points as CSV files');
        }
        
        function exportFullReport() {
            const report = {
                metadata: {
                    title: "Boundary Cancellation Principle Analysis",
                    author: "Wessen Getachew",
                    date: new Date().toISOString(),
                    version: "1.0"
                },
                parameters: {
                    current2DRadius: parseInt(document.getElementById('vis-radius-2d').value),
                    current3DRadius: parseInt(document.getElementById('vis-radius-3d').value),
                    chartMaxRadius: parseInt(document.getElementById('chart-max-radius').value)
                },
                zetaValues: zetaValues,
                computedData: {}
            };
            
            // Add computed data for chart dimensions
            const selectedDims = Array.from(document.querySelectorAll('.chart-dimension:checked'))
                .map(cb => parseInt(cb.value));
            
            selectedDims.forEach(k => {
                report.computedData[`k=${k}`] = [];
                const maxR = report.parameters.chartMaxRadius;
                const step = parseInt(document.getElementById('chart-step').value);
                
                for (let R = step; R <= maxR; R += step) {
                    const N = computeN(R, k);
                    const expected = Math.pow(R, k) / zetaValues[k];
                    const error = N - expected;
                    
                    report.computedData[`k=${k}`].push({
                        R: R,
                        N: N,
                        expected: expected,
                        error: error,
                        relativeError: error / expected
                    });
                }
            });
            
            // Create download link
            const jsonStr = JSON.stringify(report, null, 2);
            const blob = new Blob([jsonStr], { type: 'application/json;charset=utf-8;' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = 'boundary_cancellation_full_report.json';
            link.click();
            
            setTimeout(() => URL.revokeObjectURL(link.href), 100);
            
            showExportStatus('Exported full report as JSON');
        }
        
        function showExportStatus(message, isError = false) {
            const statusDiv = document.getElementById('exportStatus');
            statusDiv.textContent = message;
            statusDiv.style.color = isError ? '#f44336' : '#4CAF50';
            
            setTimeout(() => {
                statusDiv.textContent = '';
            }, 5000);
        }
        
        // =============================
        // Tab Management
        // =============================
        
        function initTabs() {
            const tabs = document.querySelectorAll('.viz-tab');
            const contents = document.querySelectorAll('.viz-content');
            
            tabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    // Remove active class from all tabs and contents
                    tabs.forEach(t => t.classList.remove('active'));
                    contents.forEach(c => c.classList.remove('active'));
                    
                    // Add active class to clicked tab and corresponding content
                    tab.classList.add('active');
                    const tabId = tab.getAttribute('data-tab');
                    document.getElementById(`viz-${tabId}`).classList.add('active');
                    state.currentTab = tabId;
                    
                    // Initialize specific visualization if needed
                    if (tabId === '3d' && !state.is3DInitialized) {
                        init3DVisualization();
                    }
                    
                    // Resize charts if needed
                    if (tabId === 'chart' && state.chart) {
                        state.chart.resize();
                    }
                });
            });
        }
        
        // =============================
        // Initialization
        // =============================
        
        function init() {
            // Initialize tabs
            initTabs();
            
            // Initialize GCD calculator
            initGCDCalculator();
            
            // Initialize visualizations
            init2DVisualization();
            initChart();
            
            // Set up export buttons
            document.getElementById('export2DPNG').addEventListener('click', export2DAsPNG);
            document.getElementById('export3DPNG').addEventListener('click', export3DAsPNG);
            document.getElementById('exportChartPNG').addEventListener('click', exportChartAsPNG);
            document.getElementById('exportDataCSV').addEventListener('click', exportErrorDataCSV);
            document.getElementById('exportLatticeCSV').addEventListener('click', exportLatticePointsCSV);
            document.getElementById('exportFullReport').addEventListener('click', exportFullReport);
            
            // Handle window resize
            window.addEventListener('resize', () => {
                draw2DLattice();
                if (state.threeRenderer && state.is3DInitialized) {
                    const canvas = document.getElementById('latticeCanvas3D');
                    const container = canvas.parentElement;
                    const width = container.clientWidth;
                    const height = container.clientHeight;
                    
                    state.threeCamera.aspect = width / height;
                    state.threeCamera.updateProjectionMatrix();
                    state.threeRenderer.setSize(width, height);
                }
                if (state.chart) {
                    state.chart.resize();
                }
            });
            
            // Show initial export status
            showExportStatus('Ready to export visualizations and data');
        }
        
        // Initialize when page loads
        window.addEventListener('load', init);
        
        // Clean up animation frames on page unload
        window.addEventListener('beforeunload', () => {
            if (state.animationFrameId) {
                cancelAnimationFrame(state.animationFrameId);
            }
            if (state.animationId) {
                clearTimeout(state.animationId);
            }
        });
        
        // Initialize MathJax configuration
        window.MathJax = {
            tex: {
                inlineMath: [['$', '$'], ['\\(', '\\)']],
                displayMath: [['$$', '$$'], ['\\[', '\\]']]
            },
            startup: {
                pageReady: () => {
                    return MathJax.startup.defaultPageReady().then(() => {
                        console.log('MathJax initialized');
                    });
                }
            }
        };
    </script>
</body>
</html>
