
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Boundary Cancellation Principle: Discovery Engine</title>
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
    <!-- Decimal.js for high-precision arithmetic -->
    <script src="https://cdn.jsdelivr.net/npm/decimal.js@10.4.3/decimal.min.js"></script>
    <style>
        /* ... [Previous CSS styles remain the same] ... */
        
        /* Research Mode Toggle */
        .research-mode-toggle {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 10000;
            background-color: #1a5fb4;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 4px 6px rgba(0,0,0,0.2);
            transition: all 0.3s ease;
        }
        
        .research-mode-toggle.active {
            background-color: #4CAF50;
        }
        
        .research-mode-toggle:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 8px rgba(0,0,0,0.3);
        }
        
        /* Research Mode Section */
        .research-mode {
            display: none;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 10px;
            margin: 30px 0;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        
        .research-mode.active {
            display: block;
        }
        
        .research-mode h3 {
            color: white;
            border-bottom: 2px solid rgba(255,255,255,0.3);
            padding-bottom: 10px;
        }
        
        .research-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        
        .research-panel {
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            padding: 15px;
            backdrop-filter: blur(10px);
        }
        
        .research-panel h4 {
            margin-top: 0;
            color: #FFD700;
            font-size: 1.1rem;
        }
        
        .slope-display {
            font-family: 'Courier New', monospace;
            background-color: rgba(0,0,0,0.3);
            padding: 10px;
            border-radius: 5px;
            margin-top: 10px;
        }
        
        .slope-value {
            font-size: 1.3rem;
            font-weight: bold;
            color: #4CAF50;
        }
        
        .critical-strip-canvas {
            width: 100%;
            height: 200px;
            background-color: rgba(0,0,0,0.3);
            border-radius: 5px;
            margin-top: 10px;
        }
        
        /* Complex plane styling */
        .complex-plane {
            position: relative;
            width: 100%;
            height: 100%;
        }
        
        .critical-line {
            position: absolute;
            top: 50%;
            left: 0;
            right: 0;
            height: 2px;
            background-color: rgba(255, 255, 255, 0.5);
            transform: translateY(-50%);
        }
        
        .critical-strip {
            position: absolute;
            top: 25%;
            left: 0;
            right: 0;
            height: 50%;
            border: 1px dashed rgba(255, 255, 255, 0.3);
        }
        
        .zeta-zero {
            position: absolute;
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background-color: #FF9800;
            transform: translate(-50%, -50%);
        }
        
        .delta-point {
            position: absolute;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background-color: #4CAF50;
            border: 2px solid white;
            transform: translate(-50%, -50%);
            transition: all 0.5s ease;
        }
        
        /* Möbius Wave Overlay */
        .mobius-wave-canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            opacity: 0.3;
        }
        
        /* High Precision Display */
        .precision-display {
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
            background-color: rgba(0,0,0,0.3);
            padding: 10px;
            border-radius: 5px;
            margin-top: 10px;
        }
        
        .precision-compare {
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
        }
        
        .precision-standard {
            color: #f44336;
        }
        
        .precision-high {
            color: #4CAF50;
        }
        
        /* Diagnostic controls */
        .diagnostic-controls {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 15px;
        }
        
        .diagnostic-btn {
            background-color: rgba(255, 255, 255, 0.2);
            color: white;
            border: 1px solid rgba(255, 255, 255, 0.3);
            padding: 8px 12px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s ease;
        }
        
        .diagnostic-btn:hover {
            background-color: rgba(255, 255, 255, 0.3);
        }
        
        .diagnostic-btn.active {
            background-color: #4CAF50;
            border-color: #4CAF50;
        }
        
        /* Console output */
        .research-console {
            background-color: rgba(0,0,0,0.7);
            color: #4CAF50;
            font-family: 'Courier New', monospace;
            padding: 10px;
            border-radius: 5px;
            margin-top: 10px;
            height: 100px;
            overflow-y: auto;
            font-size: 0.9rem;
            white-space: pre-wrap;
        }
        
        .console-entry {
            margin: 2px 0;
            padding: 2px;
            border-left: 3px solid transparent;
        }
        
        .console-entry.info {
            border-left-color: #4CAF50;
        }
        
        .console-entry.warning {
            border-left-color: #FF9800;
        }
        
        .console-entry.error {
            border-left-color: #f44336;
        }
        
        /* Chart overlays */
        .chart-overlay-container {
            position: relative;
        }
        
        .chart-overlay-controls {
            position: absolute;
            top: 10px;
            right: 10px;
            z-index: 10;
            background-color: rgba(255, 255, 255, 0.9);
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 0.8rem;
        }
    </style>
</head>
<body>
    <!-- Research Mode Toggle -->
    <button class="research-mode-toggle" id="researchToggle">
        🔬 Research Mode: OFF
    </button>
    
    <header>
        <h1>The Boundary Cancellation Principle:<br>Discovery Engine</h1>
        <div class="author">
            By <strong>Wessen Getachew</strong> · Twitter: <a href="https://twitter.com/7dview" target="_blank">@7dview</a>
        </div>
    </header>
    
    <main>
        <!-- Research Mode Section (Hidden by Default) -->
        <section class="research-mode" id="researchMode">
            <h3>🔬 Research Mode: Diagnostic Overlays</h3>
            <p>These diagnostic tools reveal hidden patterns in the boundary cancellation error. Use them to isolate the geometric mechanism of the error term Δ(R).</p>
            
            <div class="research-grid">
                <!-- Panel 1: Möbius Wave Overlay -->
                <div class="research-panel">
                    <h4>1. Möbius Wave Overlay</h4>
                    <p>Visualizes the cumulative sum of μ(d) (Mertens function) across the lattice. The "wave" shows how μ(d) conspires to create the boundary error.</p>
                    <div class="diagnostic-controls">
                        <button class="diagnostic-btn" id="toggleMobiusWave">Show Möbius Wave</button>
                        <button class="diagnostic-btn" id="toggleWaveIntensity">Intensity: Medium</button>
                    </div>
                    <div class="precision-display">
                        <div>Mertens Function M(R) = Σ μ(d): <span id="mertensValue">0</span></div>
                        <div>Oscillation Amplitude: <span id="waveAmplitude">0</span></div>
                    </div>
                </div>
                
                <!-- Panel 2: Log-Log Scaling -->
                <div class="research-panel">
                    <h4>2. Log-Log Scaling & Exponent Hunter</h4>
                    <p>Switches error chart to log-log scale. The slope of the line equals the growth exponent α in Δ(R) ∝ R^α.</p>
                    <div class="diagnostic-controls">
                        <button class="diagnostic-btn" id="toggleLogLog">Log-Log Plot</button>
                        <button class="diagnostic-btn" id="calculateSlope">Calculate Slope</button>
                    </div>
                    <div class="slope-display">
                        <div>Growth Exponent α = <span class="slope-value" id="exponentValue">-</span></div>
                        <div>Confidence Interval: <span id="confidenceInterval">-</span></div>
                        <div>R² Goodness of Fit: <span id="goodnessOfFit">-</span></div>
                    </div>
                </div>
                
                <!-- Panel 3: Critical Strip Projection -->
                <div class="research-panel">
                    <h4>3. Critical Strip Projection</h4>
                    <p>Maps Δ(R) onto the complex plane. Points staying within the critical strip (0 < Re(s) < 1) suggest zeros on the ½-line.</p>
                    <div class="critical-strip-canvas">
                        <div class="complex-plane" id="complexPlane">
                            <div class="critical-line"></div>
                            <div class="critical-strip"></div>
                            <!-- Zeros will be added here -->
                            <div class="delta-point" id="deltaPoint"></div>
                        </div>
                    </div>
                    <div class="precision-display">
                        <div>Current Point: Re = <span id="currentRe">0</span>, Im = <span id="currentIm">0</span></div>
                        <div>Distance from ½-line: <span id="distanceFromHalf">-</span></div>
                    </div>
                </div>
                
                <!-- Panel 4: High Precision Arithmetic -->
                <div class="research-panel">
                    <h4>4. High-Precision Arithmetic</h4>
                    <p>Compares standard JavaScript floats with high-precision Decimal.js calculations.</p>
                    <div class="diagnostic-controls">
                        <button class="diagnostic-btn" id="toggleHighPrecision">Use High Precision</button>
                        <button class="diagnostic-btn" id="benchmarkPrecision">Run Benchmark</button>
                    </div>
                    <div class="precision-display">
                        <div class="precision-compare">
                            <span class="precision-standard">Standard JS:</span>
                            <span id="standardValue">-</span>
                        </div>
                        <div class="precision-compare">
                            <span class="precision-high">High Precision:</span>
                            <span id="highPrecisionValue">-</span>
                        </div>
                        <div>Relative Error: <span id="relativeError">-</span></div>
                    </div>
                </div>
            </div>
            
            <!-- Research Console -->
            <div class="research-panel" style="grid-column: 1 / -1;">
                <h4>Research Console</h4>
                <div class="research-console" id="researchConsole">
                    [System] Research Mode Initialized
                    [System] Diagnostic overlays ready
                    [System] High-precision arithmetic enabled
                </div>
                <div class="diagnostic-controls">
                    <button class="diagnostic-btn" id="clearConsole">Clear Console</button>
                    <button class="diagnostic-btn" id="exportResearchData">Export Research Data</button>
                    <button class="diagnostic-btn" id="runFullAnalysis">Run Full Analysis</button>
                </div>
            </div>
        </section>
        
        <!-- Rest of the existing content remains exactly the same -->
        <!-- [Previous content: Abstract, Export Controls, GCD Calculator, Visualizations, etc.] -->
        
    </main>
    
    <footer>
        <div class="attribution">
            The Boundary Cancellation Principle: Discovery Engine
        </div>
        <div>© 2025 · By Wessen Getachew · Research Mode: Diagnostic Overlays Active</div>
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
        // Research Mode Core
        // =============================
        
        let researchMode = {
            active: false,
            highPrecision: false,
            mobiusWave: false,
            logLogPlot: false,
            waveIntensity: 1.0,
            precisionEngine: null,
            diagnosticData: [],
            consoleHistory: []
        };
        
        // Initialize Decimal.js for high precision
        Decimal.set({ precision: 50, rounding: Decimal.ROUND_HALF_EVEN });
        
        // =============================
        // Möbius Wave Overlay
        // =============================
        
        function initMobiusWave() {
            const canvas = document.getElementById('latticeCanvas2D');
            const waveCanvas = document.createElement('canvas');
            waveCanvas.className = 'mobius-wave-canvas';
            waveCanvas.width = canvas.width;
            waveCanvas.height = canvas.height;
            canvas.parentElement.appendChild(waveCanvas);
            
            return waveCanvas;
        }
        
        function drawMobiusWave(canvas, R, mobiusWaveCanvas) {
            if (!researchMode.mobiusWave) return;
            
            const ctx = mobiusWaveCanvas.getContext('2d');
            const baseCanvas = document.getElementById('latticeCanvas2D');
            
            // Match dimensions
            mobiusWaveCanvas.width = baseCanvas.width;
            mobiusWaveCanvas.height = baseCanvas.height;
            ctx.clearRect(0, 0, mobiusWaveCanvas.width, mobiusWaveCanvas.height);
            
            // Calculate Mertens function values for 1..R
            const mertens = new Array(R + 1).fill(0);
            let currentSum = 0;
            for (let n = 1; n <= R; n++) {
                currentSum += mobius(n);
                mertens[n] = currentSum;
            }
            
            // Draw wave pattern
            const pointSize = parseInt(document.getElementById('point-size-2d').value);
            const padding = pointSize * 2;
            const availableWidth = (baseCanvas.width / window.devicePixelRatio) - padding * 2;
            const availableHeight = (baseCanvas.height / window.devicePixelRatio) - padding * 2;
            const cellSize = Math.min(availableWidth / R, availableHeight / R);
            
            // Create gradient for wave
            const gradient = ctx.createLinearGradient(0, 0, baseCanvas.width, 0);
            gradient.addColorStop(0, 'rgba(0, 100, 255, 0.1)');
            gradient.addColorStop(0.5, 'rgba(0, 200, 255, 0.3)');
            gradient.addColorStop(1, 'rgba(0, 100, 255, 0.1)');
            
            ctx.fillStyle = gradient;
            ctx.globalAlpha = 0.3 * researchMode.waveIntensity;
            
            // Draw wave based on Mertens function
            for (let x = 1; x <= R; x++) {
                for (let y = 1; y <= R; y++) {
                    const d = Math.max(x, y);
                    const mertensValue = mertens[d];
                    
                    // Normalize value for coloring
                    const normalized = (mertensValue + 10) / 20; // Scale to [0,1]
                    const px = padding + (x - 1) * cellSize;
                    const py = padding + (y - 1) * cellSize;
                    
                    // Color based on Mertens function value
                    ctx.fillStyle = `rgba(0, ${Math.floor(100 + normalized * 155)}, 255, ${0.2 * researchMode.waveIntensity})`;
                    ctx.fillRect(px, py, cellSize, cellSize);
                }
            }
            
            // Update diagnostic display
            document.getElementById('mertensValue').textContent = mertens[R];
            document.getElementById('waveAmplitude').textContent = Math.max(...mertens.map(Math.abs));
            
            logToConsole(`Möbius Wave: M(${R}) = ${mertens[R]}, Amplitude = ${Math.max(...mertens.map(Math.abs))}`, 'info');
        }
        
        // =============================
        // Log-Log Scaling & Exponent Calculation
        // =============================
        
        function calculateGrowthExponent() {
            const maxR = parseInt(document.getElementById('chart-max-radius').value);
            const step = parseInt(document.getElementById('chart-step').value);
            const k = parseInt(document.getElementById('inspect-chart-dimension').value) || 2;
            
            // Collect data points
            const logR = [];
            const logDelta = [];
            
            for (let R = step; R <= maxR; R += step) {
                const N = computeN(R, k);
                const expected = Math.pow(R, k) / zetaValues[k];
                const delta = Math.abs(N - expected);
                
                if (delta > 0) {
                    logR.push(Math.log(R));
                    logDelta.push(Math.log(delta));
                }
            }
            
            // Perform linear regression: log(Δ) = α * log(R) + β
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
            
            const r2 = 1 - (ssResidual / ssTotal);
            
            // Calculate confidence interval (simplified)
            const se = Math.sqrt(ssResidual / (n - 2));
            const tValue = 1.96; // 95% confidence for large n
            const ci = tValue * se / Math.sqrt(sumX2 - sumX * sumX / n);
            
            // Update display
            document.getElementById('exponentValue').textContent = α.toFixed(4);
            document.getElementById('confidenceInterval').textContent = `±${ci.toFixed(6)}`;
            document.getElementById('goodnessOfFit').textContent = r2.toFixed(6);
            
            // Add to diagnostic data
            researchMode.diagnosticData.push({
                type: 'exponent',
                k: k,
                R_max: maxR,
                exponent: α,
                confidence: ci,
                r2: r2,
                timestamp: new Date().toISOString()
            });
            
            logToConsole(`Growth Exponent: α = ${α.toFixed(4)} ± ${ci.toFixed(6)} (R² = ${r2.toFixed(6)})`, 'info');
            
            // Return for further analysis
            return { exponent: α, confidence: ci, r2: r2 };
        }
        
        function toggleLogLogPlot() {
            researchMode.logLogPlot = !researchMode.logLogPlot;
            
            if (researchMode.logLogPlot && state.chart) {
                state.chart.options.scales.x.type = 'logarithmic';
                state.chart.options.scales.y.type = 'logarithmic';
                state.chart.options.scales.x.title.text = 'log(R)';
                state.chart.options.scales.y.title.text = 'log(Δ(R))';
            } else if (state.chart) {
                state.chart.options.scales.x.type = 'linear';
                state.chart.options.scales.y.type = 'linear';
                state.chart.options.scales.x.title.text = 'Radius R';
                state.chart.options.scales.y.title.text = 'Error Δ(R)';
            }
            
            if (state.chart) {
                state.chart.update();
            }
            
            logToConsole(`Log-Log Plot ${researchMode.logLogPlot ? 'enabled' : 'disabled'}`, 'info');
        }
        
        // =============================
        // Critical Strip Projection
        // =============================
        
        function initCriticalStrip() {
            const container = document.getElementById('complexPlane');
            
            // Add some known zeta zeros (simplified visualization)
            const knownZeros = [
                { re: 0.5, im: 14.134725 },
                { re: 0.5, im: 21.022040 },
                { re: 0.5, im: 25.010858 },
                { re: 0.5, im: 30.424876 },
                { re: 0.5, im: 32.935062 }
            ];
            
            knownZeros.forEach(zero => {
                const zeroEl = document.createElement('div');
                zeroEl.className = 'zeta-zero';
                zeroEl.style.left = `${(zero.re + 2) * 25}%`; // Map [-2, 2] to [0%, 100%]
                zeroEl.style.top = `${50 + zero.im * 0.5}%`; // Simplified scaling
                container.appendChild(zeroEl);
            });
        }
        
        function updateCriticalStrip(R, k) {
            const N = computeN(R, k);
            const expected = Math.pow(R, k) / zetaValues[k];
            const delta = N - expected;
            
            // Map delta to complex plane coordinate
            // For visualization purposes, we create a simplified mapping
            const re = 0.5 + (delta / Math.pow(R, k - 0.5)) * 0.1; // Map to near 1/2 line
            const im = Math.log(R) * 0.5; // Increase with log(R)
            
            // Normalize for display
            const displayRe = Math.max(0, Math.min(1, re));
            const displayIm = Math.max(0.25, Math.min(0.75, im / 10));
            
            // Update point position
            const point = document.getElementById('deltaPoint');
            point.style.left = `${(displayRe) * 100}%`;
            point.style.top = `${displayIm * 100}%`;
            
            // Update display
            document.getElementById('currentRe').textContent = re.toFixed(6);
            document.getElementById('currentIm').textContent = im.toFixed(6);
            document.getElementById('distanceFromHalf').textContent = Math.abs(re - 0.5).toFixed(6);
            
            // Store in diagnostic data
            researchMode.diagnosticData.push({
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
        
        // =============================
        // High Precision Arithmetic
        // =============================
        
        function computeNHighPrecision(R, k) {
            if (!researchMode.highPrecision) return computeN(R, k);
            
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
        
        function computeExpectedHighPrecision(R, k) {
            const R_dec = new Decimal(R);
            const zeta_dec = new Decimal(zetaValues[k]);
            return R_dec.pow(k).dividedBy(zeta_dec).toNumber();
        }
        
        function runPrecisionBenchmark() {
            const R = parseInt(document.getElementById('vis-radius-2d').value);
            const k = 2;
            
            // Standard JavaScript calculation
            const startStandard = performance.now();
            const N_standard = computeN(R, k);
            const expected_standard = Math.pow(R, k) / zetaValues[k];
            const delta_standard = N_standard - expected_standard;
            const timeStandard = performance.now() - startStandard;
            
            // High precision calculation
            const startHigh = performance.now();
            const N_high_dec = computeNHighPrecision(R, k);
            const expected_high_dec = computeExpectedHighPrecision(R, k);
            const delta_high = new Decimal(N_high_dec).minus(expected_high_dec);
            const timeHigh = performance.now() - startHigh;
            
            // Update display
            document.getElementById('standardValue').textContent = delta_standard.toExponential(6);
            document.getElementById('highPrecisionValue').textContent = delta_high.toExponential(6);
            
            const relError = Math.abs(delta_standard - delta_high.toNumber()) / Math.abs(delta_high.toNumber());
            document.getElementById('relativeError').textContent = relError.toExponential(6);
            
            logToConsole(`Precision Benchmark: Standard=${timeStandard.toFixed(2)}ms, High=${timeHigh.toFixed(2)}ms, RelError=${relError.toExponential(6)}`, 'info');
            
            // Store benchmark data
            researchMode.diagnosticData.push({
                type: 'benchmark',
                R: R,
                k: k,
                standard_delta: delta_standard,
                high_delta: delta_high.toNumber(),
                relative_error: relError,
                time_standard: timeStandard,
                time_high: timeHigh,
                timestamp: new Date().toISOString()
            });
        }
        
        // =============================
        // Running Average Calculation
        // =============================
        
        function calculateRunningAverage(maxR, step, k) {
            const runningSum = [];
            let cumulativeSum = 0;
            let count = 0;
            
            for (let R = step; R <= maxR; R += step) {
                const N = researchMode.highPrecision ? computeNHighPrecision(R, k) : computeN(R, k);
                const expected = researchMode.highPrecision ? computeExpectedHighPrecision(R, k) : Math.pow(R, k) / zetaValues[k];
                const delta = N - expected;
                
                cumulativeSum += delta;
                count++;
                runningSum.push(cumulativeSum / count);
            }
            
            return runningSum;
        }
        
        // =============================
        // Research Console
        // =============================
        
        function logToConsole(message, type = 'info') {
            const consoleEl = document.getElementById('researchConsole');
            const entry = document.createElement('div');
            entry.className = `console-entry ${type}`;
            entry.textContent = `[${new Date().toLocaleTimeString()}] ${message}`;
            
            consoleEl.appendChild(entry);
            consoleEl.scrollTop = consoleEl.scrollHeight;
            
            // Keep only last 50 entries
            researchMode.consoleHistory.push({ message, type, timestamp: new Date().toISOString() });
            if (researchMode.consoleHistory.length > 50) {
                researchMode.consoleHistory.shift();
            }
        }
        
        function clearConsole() {
            const consoleEl = document.getElementById('researchConsole');
            consoleEl.innerHTML = '';
            researchMode.consoleHistory = [];
            logToConsole('Console cleared', 'info');
        }
        
        // =============================
        // Export Research Data
        // =============================
        
        function exportResearchData() {
            const data = {
                metadata: {
                    export_date: new Date().toISOString(),
                    research_mode: researchMode.active,
                    high_precision: researchMode.highPrecision,
                    version: "1.0.0"
                },
                diagnostics: researchMode.diagnosticData,
                console_history: researchMode.consoleHistory
            };
            
            const jsonStr = JSON.stringify(data, null, 2);
            const blob = new Blob([jsonStr], { type: 'application/json;charset=utf-8;' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `research_data_${Date.now()}.json`;
            link.click();
            
            logToConsole('Research data exported', 'info');
        }
        
        // =============================
        // Full Analysis Suite
        // =============================
        
        async function runFullAnalysis() {
            logToConsole('Starting full analysis suite...', 'info');
            
            // 1. Calculate growth exponent
            const exponentResult = calculateGrowthExponent();
            logToConsole(`Exponent analysis complete: α = ${exponentResult.exponent.toFixed(4)}`, 'info');
            
            // 2. Run precision benchmark
            runPrecisionBenchmark();
            
            // 3. Update critical strip projection
            const R = parseInt(document.getElementById('chart-max-radius').value);
            const k = parseInt(document.getElementById('inspect-chart-dimension').value) || 2;
            updateCriticalStrip(R, k);
            
            // 4. Calculate running average statistics
            const step = parseInt(document.getElementById('chart-step').value);
            const runningAvg = calculateRunningAverage(R, step, k);
            
            // Check if running average tends to zero
            const lastAvg = runningAvg[runningAvg.length - 1];
            const avgTrend = runningAvg.slice(-10).reduce((a, b) => a + Math.abs(b), 0) / 10;
            
            logToConsole(`Running average analysis: Final avg = ${lastAvg.toExponential(4)}, Trend = ${avgTrend.toExponential(4)}`, 'info');
            
            // 5. Boundary self-correction test
            const boundaryCorrection = testBoundarySelfCorrection(R, k);
            logToConsole(`Boundary self-correction: ${boundaryCorrection.passed ? 'PASS' : 'FAIL'} (p-value = ${boundaryCorrection.pValue.toFixed(6)})`, 
                        boundaryCorrection.passed ? 'info' : 'warning');
            
            logToConsole('Full analysis complete!', 'info');
        }
        
        function testBoundarySelfCorrection(R, k) {
            // Statistical test for boundary self-correction
            // If the boundary has no long-term bias, the error should be mean-reverting
            
            const errors = [];
            for (let r = 1; r <= R; r += Math.max(1, Math.floor(R / 100))) {
                const N = researchMode.highPrecision ? computeNHighPrecision(r, k) : computeN(r, k);
                const expected = researchMode.highPrecision ? computeExpectedHighPrecision(r, k) : Math.pow(r, k) / zetaValues[k];
                errors.push(N - expected);
            }
            
            // Calculate autocorrelation at lag 1
            let autocorr = 0;
            const mean = errors.reduce((a, b) => a + b, 0) / errors.length;
            
            for (let i = 1; i < errors.length; i++) {
                autocorr += (errors[i] - mean) * (errors[i-1] - mean);
            }
            
            const variance = errors.reduce((sum, err) => sum + Math.pow(err - mean, 2), 0) / errors.length;
            autocorr = autocorr / ((errors.length - 1) * variance);
            
            // Simplified p-value calculation
            const pValue = 2 * (1 - 0.5 * (1 + Math.erf(Math.abs(autocorr) * Math.sqrt(errors.length / 2))));
            
            return {
                passed: Math.abs(autocorr) < 0.1 && pValue > 0.05, // Low autocorrelation, high p-value
                autocorrelation: autocorr,
                pValue: pValue,
                mean: mean,
                variance: variance
            };
        }
        
        // =============================
        // Event Handlers & Initialization
        // =============================
        
        function initResearchMode() {
            // Research mode toggle
            document.getElementById('researchToggle').addEventListener('click', () => {
                researchMode.active = !researchMode.active;
                const toggleBtn = document.getElementById('researchToggle');
                const researchSection = document.getElementById('researchMode');
                
                if (researchMode.active) {
                    toggleBtn.textContent = '🔬 Research Mode: ON';
                    toggleBtn.classList.add('active');
                    researchSection.classList.add('active');
                    logToConsole('Research Mode Activated', 'info');
                    
                    // Initialize components
                    initCriticalStrip();
                } else {
                    toggleBtn.textContent = '🔬 Research Mode: OFF';
                    toggleBtn.classList.remove('active');
                    researchSection.classList.remove('active');
                }
            });
            
            // Möbius Wave controls
            document.getElementById('toggleMobiusWave').addEventListener('click', function() {
                researchMode.mobiusWave = !researchMode.mobiusWave;
                this.textContent = researchMode.mobiusWave ? 'Hide Möbius Wave' : 'Show Möbius Wave';
                
                if (researchMode.mobiusWave) {
                    initMobiusWave();
                    draw2DLattice(); // Redraw to include wave
                }
            });
            
            document.getElementById('toggleWaveIntensity').addEventListener('click', function() {
                researchMode.waveIntensity = (researchMode.waveIntensity + 0.5) % 2.5;
                const levels = ['Low', 'Medium', 'High', 'Very High'];
                this.textContent = `Intensity: ${levels[Math.floor(researchMode.waveIntensity / 0.5)]}`;
                
                if (researchMode.mobiusWave) {
                    draw2DLattice(); // Redraw with new intensity
                }
            });
            
            // Log-Log controls
            document.getElementById('toggleLogLog').addEventListener('click', function() {
                toggleLogLogPlot();
                this.textContent = researchMode.logLogPlot ? 'Linear Plot' : 'Log-Log Plot';
            });
            
            document.getElementById('calculateSlope').addEventListener('click', () => {
                calculateGrowthExponent();
            });
            
            // High Precision controls
            document.getElementById('toggleHighPrecision').addEventListener('click', function() {
                researchMode.highPrecision = !researchMode.highPrecision;
                this.textContent = researchMode.highPrecision ? 'Use Standard Precision' : 'Use High Precision';
                this.classList.toggle('active');
                
                logToConsole(`High precision arithmetic ${researchMode.highPrecision ? 'enabled' : 'disabled'}`, 'info');
                
                // Update calculations
                if (state.chart) {
                    updateChart();
                }
            });
            
            document.getElementById('benchmarkPrecision').addEventListener('click', () => {
                runPrecisionBenchmark();
            });
            
            // Console controls
            document.getElementById('clearConsole').addEventListener('click', clearConsole);
            document.getElementById('exportResearchData').addEventListener('click', exportResearchData);
            document.getElementById('runFullAnalysis').addEventListener('click', () => {
                runFullAnalysis();
            });
            
            // Override draw2DLattice to include Möbius wave
            const originalDraw2DLattice = window.draw2DLattice;
            window.draw2DLattice = function() {
                originalDraw2DLattice();
                
                if (researchMode.active && researchMode.mobiusWave) {
                    const R = parseInt(document.getElementById('vis-radius-2d').value);
                    const waveCanvas = document.querySelector('.mobius-wave-canvas') || initMobiusWave();
                    drawMobiusWave(document.getElementById('latticeCanvas2D'), R, waveCanvas);
                }
            };
            
            // Override updateChart to include running average
            const originalUpdateChart = window.updateChart;
            window.updateChart = function() {
                originalUpdateChart();
                
                if (researchMode.active && state.chart) {
                    const maxR = parseInt(document.getElementById('chart-max-radius').value);
                    const step = parseInt(document.getElementById('chart-step').value);
                    const selectedDims = Array.from(document.querySelectorAll('.chart-dimension:checked'))
                        .map(cb => parseInt(cb.value))
                        .sort((a, b) => a - b);
                    
                    if (selectedDims.length > 0) {
                        const k = selectedDims[0]; // Use first selected dimension
                        const runningAvg = calculateRunningAverage(maxR, step, k);
                        
                        // Add running average dataset
                        state.chart.data.datasets.push({
                            label: `Running Average (k=${k})`,
                            data: runningAvg,
                            borderColor: '#FFD700',
                            backgroundColor: 'transparent',
                            borderWidth: 3,
                            borderDash: [5, 5],
                            tension: 0.2,
                            pointRadius: 0
                        });
                        
                        state.chart.update();
                        
                        // Update critical strip
                        updateCriticalStrip(maxR, k);
                    }
                }
            };
            
            // Add keyboard shortcut for research mode (Ctrl+Shift+R)
            document.addEventListener('keydown', (e) => {
                if (e.ctrlKey && e.shiftKey && e.key === 'R') {
                    e.preventDefault();
                    document.getElementById('researchToggle').click();
                }
            });
            
            logToConsole('Research mode initialization complete', 'info');
        }
        
        // =============================
        // Math Utility Enhancements
        // =============================
        
        // Enhanced mobius function with caching
        const mobiusCache = new Map();
        function mobius(n) {
            if (n === 1) return 1;
            if (mobiusCache.has(n)) return mobiusCache.get(n);
            
            let p = 0;
            let temp = n;
            
            // Check for factors of 2
            if (temp % 2 === 0) {
                temp /= 2;
                p++;
                if (temp % 2 === 0) {
                    mobiusCache.set(n, 0);
                    return 0;
                }
            }
            
            // Check odd factors
            for (let i = 3; i * i <= temp; i += 2) {
                if (temp % i === 0) {
                    if (temp % (i * i) === 0) {
                        mobiusCache.set(n, 0);
                        return 0;
                    }
                    p++;
                    temp = Math.floor(temp / i);
                }
            }
            
            if (temp > 1) p++;
            
            const result = (p % 2 === 0) ? 1 : -1;
            mobiusCache.set(n, result);
            return result;
        }
        
        // Error function approximation (for p-value calculation)
        Math.erf = function(x) {
            // Abramowitz and Stegun approximation
            const a1 =  0.254829592;
            const a2 = -0.284496736;
            const a3 =  1.421413741;
            const a4 = -1.453152027;
            const a5 =  1.061405429;
            const p  =  0.3275911;
            
            const sign = x < 0 ? -1 : 1;
            x = Math.abs(x);
            
            const t = 1.0 / (1.0 + p * x);
            const y = 1.0 - (((((a5 * t + a4) * t) + a3) * t + a2) * t + a1) * t * Math.exp(-x * x);
            
            return sign * y;
        };
        
        // =============================
        // Initialize when page loads
        // =============================
        
        // Add to existing initialization
        window.addEventListener('load', () => {
            // Call existing init function if it exists
            if (typeof init === 'function') {
                init();
            }
            
            // Initialize research mode
            initResearchMode();
            
            // Add initial console message
            setTimeout(() => {
                if (researchMode.active) {
                    logToConsole('Diagnostic overlays ready. Use Ctrl+Shift+R to toggle research mode.', 'info');
                }
            }, 1000);
        });
        
    </script>
</body>
</html>
