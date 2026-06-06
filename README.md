<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IndiaGlobe Cine - Master Overlay Studio v5.1.0</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    
    <style>
        /* ==========================================================================
           1. MASTER VARIABLES & DYNAMIC INJECTION
           ========================================================================== */
        :root {
            /* STUDIO UI THEME - Midnight Glass */
            --studio-bg: #030712;
            --studio-panel: #111827;
            --studio-border: #1f2937;
            --studio-focus: #3b82f6;
            --studio-text: #f9fafb;
            --studio-text-muted: #9ca3af;
            --studio-accent: #2563eb;
            --studio-accent-hover: #1d4ed8;
            --studio-success: #10b981;
            
            /* EXPORT TARGET DEFAULT STATE */
            --post-bg-color: #000000;
            --post-text-color: #e7e9ea;
            --post-muted-color: #71767b;
            --post-verified-color: #1d9bf0;
            --avatar-bg-color: #16181c; 

            /* NEW HIGHLIGHT FEATURE COLORS */
            --quote-bg-color: #b91c1c;
            --quote-text-color: #ffffff;
            --paren-bg-color: #047857;
            --paren-text-color: #ffffff;

            /* Core Layout Dimensions */
            --dyn-width: 600px;
            --dyn-padding: 24px;
            
            /* Typography & Element Scaling Dimensions */
            --dyn-avatar-size: 48px;
            --dyn-name-size: 16px;
            --dyn-user-size: 15px;
            --dyn-text-size: 24px;
            --dyn-font-weight: 400; /* New Dynamic Font Weight */
            --dyn-line-height: 1.35;
            
            /* Font Stacks */
            --font-ui: "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            --font-post: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        /* ==========================================================================
           2. ZERO-SCROLL WORKSPACE ARCHITECTURE 
           ========================================================================== */
        * { box-sizing: border-box; margin: 0; padding: 0; }

        body {
            font-family: var(--font-ui);
            background-color: var(--studio-bg);
            color: var(--studio-text);
            width: 100vw;
            height: 100vh;
            overflow: hidden; 
            display: grid;
            grid-template-columns: 360px 1fr 340px;
        }

        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--studio-border); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--studio-text-muted); }

        /* ==========================================================================
           3. STUDIO CONTROL PANELS
           ========================================================================== */
        .panel {
            background: rgba(17, 24, 39, 0.85);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-right: 1px solid var(--studio-border);
            display: flex;
            flex-direction: column;
            height: 100vh;
            z-index: 100;
        }
        .panel.right { border-right: none; border-left: 1px solid var(--studio-border); }

        .panel-header {
            padding: 20px 24px;
            border-bottom: 1px solid var(--studio-border);
            background: rgba(17, 24, 39, 0.95);
            display: flex; align-items: center; gap: 10px;
            flex-shrink: 0;
        }
        
        .panel-header h2 { font-size: 12px; font-weight: 700; text-transform: uppercase; letter-spacing: 2px; color: var(--studio-accent); }

        .panel-scroll-area {
            padding: 24px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 24px;
            flex-grow: 1;
        }

        .section-label {
            font-size: 10px; font-weight: 700; color: var(--studio-text-muted);
            text-transform: uppercase; letter-spacing: 1.5px;
            display: flex; align-items: center; gap: 10px;
        }
        .section-label::after { content: ''; flex: 1; height: 1px; background: var(--studio-border); }

        /* ==========================================================================
           4. HIGH-PRECISION UI INPUTS
           ========================================================================== */
        .input-group { display: flex; flex-direction: column; gap: 8px; }
        .input-group label { font-size: 12px; font-weight: 500; color: var(--studio-text); }
        
        .val-badge { 
            font-family: monospace; font-size: 11px; background: rgba(37, 99, 235, 0.15); 
            color: #60a5fa; padding: 2px 6px; border-radius: 4px; border: 1px solid rgba(59, 130, 246, 0.2);
        }

        .flex-between { display: flex; justify-content: space-between; align-items: center; }

        input[type="text"], textarea, select {
            width: 100%; padding: 12px; background: rgba(0,0,0,0.3); border: 1px solid var(--studio-border);
            color: var(--studio-text); border-radius: 6px; font-size: 13px; font-family: inherit;
            transition: all 0.2s;
        }
        select { cursor: pointer; }
        select option { background: var(--studio-panel); color: var(--studio-text); }
        input[type="text"]:focus, textarea:focus, select:focus { outline: none; border-color: var(--studio-focus); box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.2); }
        textarea { resize: vertical; min-height: 100px; line-height: 1.5; font-family: monospace; }

        /* Advanced Color Picker Styling */
        .color-picker-wrapper {
            display: flex; align-items: center; justify-content: space-between; gap: 12px;
            background: rgba(0,0,0,0.3); padding: 8px 12px; border-radius: 6px; border: 1px solid var(--studio-border);
        }
        .color-picker-left { display: flex; align-items: center; gap: 10px; }
        .color-picker-wrapper span.label { font-size: 12px; color: var(--studio-text); font-weight: 500; }
        input[type="color"] {
            -webkit-appearance: none; border: none; width: 28px; height: 28px; border-radius: 4px; cursor: pointer;
            padding: 0; background: transparent;
        }
        input[type="color"]::-webkit-color-swatch-wrapper { padding: 0; }
        input[type="color"]::-webkit-color-swatch { border: 1px solid rgba(255,255,255,0.2); border-radius: 4px; }
        .color-hex { font-family: monospace; font-size: 12px; color: var(--studio-text-muted); text-transform: uppercase; }

        /* Micro-adjustment Sliders */
        input[type="range"] {
            -webkit-appearance: none; width: 100%; height: 4px; background: var(--studio-border); border-radius: 2px;
        }
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none; width: 16px; height: 16px; border-radius: 50%;
            background: #fff; box-shadow: 0 2px 4px rgba(0,0,0,0.5); cursor: pointer; transition: 0.1s;
        }
        input[type="range"]::-webkit-slider-thumb:active { transform: scale(1.2); background: var(--studio-focus); }

        /* Buttons & Toggles */
        .btn {
            width: 100%; padding: 14px; border: none; border-radius: 6px; font-weight: 600; font-size: 13px;
            cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px;
            transition: all 0.2s; text-transform: uppercase; letter-spacing: 0.5px;
        }
        .btn-action { background: var(--studio-accent); color: white; }
        .btn-action:hover { background: var(--studio-accent-hover); transform: translateY(-1px); box-shadow: 0 4px 12px rgba(37, 99, 235, 0.4); }
        .btn-outline { background: transparent; color: var(--studio-text); border: 1px solid var(--studio-border); }
        .btn-outline:hover { background: rgba(255,255,255,0.05); }

        .toggle {
            width: 40px; height: 22px; background: var(--studio-border); border-radius: 11px;
            position: relative; cursor: pointer; transition: 0.3s;
        }
        .toggle::after {
            content: ''; position: absolute; top: 2px; left: 2px; width: 18px; height: 18px;
            background: white; border-radius: 50%; transition: 0.3s;
        }
        .toggle.on { background: var(--studio-focus); }
        .toggle.on::after { transform: translateX(18px); }

        /* ==========================================================================
           5. CENTER STAGE (THE RENDER TARGET ZONE)
           ========================================================================== */
        .stage {
            display: flex; align-items: center; justify-content: center;
            position: relative; overflow: hidden;
            background: radial-gradient(circle at center, #1e293b 0%, var(--studio-bg) 100%);
        }

        .stage.alpha-mode::before {
            content: ''; position: absolute; inset: 0;
            background-image: linear-gradient(45deg, #111827 25%, transparent 25%), linear-gradient(-45deg, #111827 25%, transparent 25%), linear-gradient(45deg, transparent 75%, #111827 75%), linear-gradient(-45deg, transparent 75%, #111827 75%);
            background-size: 20px 20px; background-position: 0 0, 0 10px, 10px -10px, -10px 0px; opacity: 0.7; z-index: 1;
        }

        .render-wrapper {
            position: relative; z-index: 10;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.8);
            transition: transform 0.2s;
        }

        /* * THE EXACT 1:1 POST COMPONENT */
        .post-node {
            background-color: var(--post-bg-color);
            width: var(--dyn-width);
            padding: var(--dyn-padding);
            font-family: var(--font-post);
            border: 1px solid transparent; 
        }
        .post-node.alpha { background-color: transparent !important; }

        .post-head { display: flex; align-items: center; margin-bottom: 14px; }
        
        /* UPDATED AVATAR BOX: Fixes image aspect ratio and distortion in html2canvas completely */
        .avatar-box {
            width: var(--dyn-avatar-size);
            height: var(--dyn-avatar-size);
            border-radius: 50%; margin-right: 12px; overflow: hidden;
            background-color: var(--avatar-bg-color);
            flex-shrink: 0; display: flex; align-items: center; justify-content: center;
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
        }

        .id-box { display: flex; flex-direction: column; justify-content: center; }
        
        .name-row { display: flex; align-items: center; gap: 4px; }
        
        .post-name {
            font-size: var(--dyn-name-size);
            font-weight: 700; color: var(--post-text-color);
            line-height: 1.1; white-space: nowrap;
        }

        .badge {
            width: calc(var(--dyn-name-size) * 1.15);
            height: calc(var(--dyn-name-size) * 1.15);
            flex-shrink: 0;
        }

        .post-user {
            font-size: var(--dyn-user-size);
            color: var(--post-muted-color); line-height: 1.2; margin-top: 2px;
        }

        .post-body {
            font-size: var(--dyn-text-size);
            font-weight: var(--dyn-font-weight); /* NEW: Dynamic Boldness */
            line-height: var(--dyn-line-height);
            color: var(--post-text-color);
            white-space: pre-wrap; word-wrap: break-word;
        }
        .post-body strong { font-weight: 900; }

        /* NEW HIGHLIGHT CLASSES */
        .highlight-quote {
            background-color: var(--quote-bg-color);
            color: var(--quote-text-color);
            padding: 2px 8px;
            border-radius: 6px;
            display: inline-block;
            margin: 2px 0;
        }
        .highlight-paren {
            background-color: var(--paren-bg-color);
            color: var(--paren-text-color);
            padding: 2px 8px;
            border-radius: 6px;
            display: inline-block;
            margin: 2px 0;
        }

        .toast {
            position: fixed; bottom: 30px; left: 50%; transform: translate(-50%, 100px);
            background: var(--studio-success); color: white; padding: 12px 24px; border-radius: 30px;
            font-size: 13px; font-weight: 600; display: flex; align-items: center; gap: 8px;
            transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); z-index: 1000; opacity: 0;
        }
        .toast.show { transform: translate(-50%, 0); opacity: 1; }

        /* ==========================================================================
           6. RESPONSIVE ENGINE (Locks Template Down Perfectly)
           ========================================================================== */
        @media (max-width: 1200px) {
            body {
                display: flex;
                flex-direction: column;
                height: auto;
                overflow-y: auto;
                overflow-x: hidden;
            }
            .panel {
                width: 100%;
                height: auto;
                border-right: none;
                border-bottom: 1px solid var(--studio-border);
            }
            .panel.right { border-left: none; }
            .panel-scroll-area { overflow-y: visible; max-height: none; }
            
            .stage {
                min-height: 80vh;
                order: 10;
                padding: 40px 20px;
            }
        }

    </style>
</head>
<body>

    <aside class="panel">
        <div class="panel-header">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>
            <h2>Content Matrix</h2>
        </div>
        <div class="panel-scroll-area">
            
            <div class="section-label">Identity Core</div>
            <div class="input-group">
                <label>Logo / Profile Picture</label>
                <input type="file" id="inp-avatar" accept="image/*" hidden>
                <button class="btn btn-outline" onclick="document.getElementById('inp-avatar').click()">Select Local Image</button>
            </div>
            <div class="input-group">
                <label>Display Name</label>
                <input type="text" id="inp-name" value="India Globe Cine">
            </div>
            <div class="input-group">
                <label>User ID Handle</label>
                <input type="text" id="inp-user" value="@india_globecine">
            </div>

            <div class="section-label">Broadcast Text</div>
            <div class="input-group">
                <div class="flex-between">
                    <label>Post Body Content</label>
                    <span style="font-size: 10px; color: var(--studio-text-muted);">*bold*, "Quote" & (Paren)</span>
                </div>
                <textarea id="inp-body">"What If The System Completely Failed?"
— *India Globe Cine* Investigates The Latest (National Exam) Security Concerns</textarea>
            </div>

            <div class="section-label">Precise Element Sizing</div>
            <div class="input-group">
                <div class="flex-between"><label>Logo Avatar Size</label><span class="val-badge" id="val-avatar">48px</span></div>
                <input type="range" id="sl-avatar" min="24" max="500" value="48">
            </div>
            <div class="input-group">
                <div class="flex-between"><label>Display Name Size</label><span class="val-badge" id="val-name">16px</span></div>
                <input type="range" id="sl-name" min="10" max="200" value="16">
            </div>
            <div class="input-group">
                <div class="flex-between"><label>User ID Size</label><span class="val-badge" id="val-user">15px</span></div>
                <input type="range" id="sl-user" min="10" max="200" value="15">
            </div>
        </div>
    </aside>

    <main class="stage" id="stage">
        <div class="render-wrapper">
            <div class="post-node" id="render-target">
                <div class="post-head">
                    <!-- CHANGED: Replaced <img> with background-image style directly on div to fix aspect ratio/distortion issues -->
                    <div class="avatar-box" id="out-avatar" style="background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0OCIgaGVpZ2h0PSI0OCIgdmlld0JveD0iMCAwIDQ4IDQ4Ij48cmVjdCB3aWR0aD0iNDgiIGhlaWdodD0iNDgiIGZpbGw9IiMxNjE4MWMiLz48Y2lyY2xlIGN4PSIyNCIgY3k9IjE4IiByPSI4IiBmaWxsPSIjNTM2NDcxIi8+PHBhdGggZD0iTTEwIDQwQzEwIDMyIDE2IDI4IDI0IDI4QzMyIDI4IDM4IDMyIDM4IDQwIiBzdHJva2U9IiM1MzY0NzEiIHN0cm9rZS13aWR0aD0iNCIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIi8+PC9zdmc+');">
                    </div>
                    <div class="id-box">
                        <div class="name-row">
                            <div class="post-name" id="out-name">India Globe Cine</div>
                            <svg class="badge" viewBox="0 0 24 24">
                                <g>
                                    <path id="badge-main" d="M22.5 12.5c0-1.58-.875-2.95-2.148-3.6.154-.435.238-.905.238-1.4 0-2.21-1.71-3.998-3.918-3.998-.47 0-.92.084-1.336.25C14.818 2.415 13.51 1.5 12 1.5s-2.816.917-3.337 2.25c-.416-.165-.866-.25-1.336-.25-2.21 0-3.918 1.792-3.918 4 0 .495.084.965.238 1.4-1.273.65-2.148 2.02-2.148 3.6 0 1.46.74 2.746 1.867 3.45-.032.224-.048.452-.048.68 0 2.21 1.71 3.998 3.918 3.998.47 0 .92-.084 1.336-.25C9.184 21.585 10.49 22.5 12 22.5s2.816-.917 3.337-2.25c.416.165.866.25 1.336.25 2.21 0 3.918-1.792 3.918-4 0-.228-.016-.456-.048-.68 1.127-.704 1.867-1.99 1.867-3.45z" fill="#1d9bf0"></path>
                                    <path d="M15.94 9.176l-4.52 4.88-1.78-1.92c-.375-.404-1.01-.43-1.414-.055-.404.376-.43 1.012-.054 1.416l2.5 2.7c.187.202.447.317.72.317s.534-.115.72-.317l5.25-5.67c.374-.404.35-1.04-.055-1.415-.405-.375-1.04-.35-1.416.054z" fill="#000000" id="badge-bg"></path>
                                </g>
                            </svg>
                        </div>
                        <div class="post-user" id="out-user">@india_globecine</div>
                    </div>
                </div>
                <div class="post-body" id="out-body"></div>
            </div>
        </div>
    </main>

    <aside class="panel right">
        <div class="panel-header">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 20V10"/><path d="M18 20V4"/><path d="M6 20v-4"/></svg>
            <h2>Format & Engine</h2>
        </div>
        <div class="panel-scroll-area">

            <div class="section-label">Color Processing</div>

            <div class="input-group">
                <div class="flex-between">
                    <label>Enable Transparent Alpha</label>
                    <div class="toggle" id="tog-alpha"></div>
                </div>
                <span style="font-size: 11px; color: var(--studio-text-muted);">Overrides background for video editing.</span>
            </div>

            <div class="color-picker-wrapper" style="margin-top: 10px;">
                <div class="color-picker-left">
                    <input type="color" id="cp-bg" value="#000000">
                    <span class="label">Main Background Fill</span>
                </div>
                <span class="color-hex" id="hex-bg">#000000</span>
            </div>

            <div class="color-picker-wrapper">
                <div class="color-picker-left">
                    <input type="color" id="cp-text" value="#e7e9ea">
                    <span class="label">Main Text Color</span>
                </div>
                <span class="color-hex" id="hex-text">#E7E9EA</span>
            </div>

            <div class="color-picker-wrapper">
                <div class="color-picker-left">
                    <input type="color" id="cp-avatar-bg" value="#16181c">
                    <span class="label">Profile Background Color</span>
                </div>
                <span class="color-hex" id="hex-avatar-bg">#16181C</span>
            </div>

            <div class="color-picker-wrapper">
                <div class="color-picker-left">
                    <input type="color" id="cp-user-text" value="#71767b">
                    <span class="label">User ID Text Color</span>
                </div>
                <span class="color-hex" id="hex-user-text">#71767B</span>
            </div>

            <div class="input-group" style="margin-top: 4px;">
                <label>Verified Badge Color</label>
                <select id="cp-badge">
                    <option value="#1d9bf0">Blue Primary</option>
                    <option value="#F5C325">Golden</option>
                    <option value="#E7E9EA">Silver</option>
                    <option value="#FFFFFF">White</option>
                    <option value="#000000">Black</option>
                </select>
            </div>

            <div class="section-label" style="margin-top: 10px;">Text Highlights & Colors</div>
            
            <div class="color-picker-wrapper">
                <div class="color-picker-left">
                    <input type="color" id="cp-quote-bg" value="#B91C1C">
                    <span class="label">Quote "" Box Color</span>
                </div>
                <span class="color-hex" id="hex-quote-bg">#B91C1C</span>
            </div>
            
            <div class="color-picker-wrapper">
                <div class="color-picker-left">
                    <input type="color" id="cp-quote-text" value="#FFFFFF">
                    <span class="label">Quote "" Text Color</span>
                </div>
                <span class="color-hex" id="hex-quote-text">#FFFFFF</span>
            </div>
            
            <div class="color-picker-wrapper">
                <div class="color-picker-left">
                    <input type="color" id="cp-paren-bg" value="#047857">
                    <span class="label">Bracket () Box Color</span>
                </div>
                <span class="color-hex" id="hex-paren-bg">#047857</span>
            </div>
            
            <div class="color-picker-wrapper">
                <div class="color-picker-left">
                    <input type="color" id="cp-paren-text" value="#FFFFFF">
                    <span class="label">Bracket () Text Color</span>
                </div>
                <span class="color-hex" id="hex-paren-text">#FFFFFF</span>
            </div>

            <div class="section-label" style="margin-top: 10px;">Global Structure & Font</div>

            <div class="input-group">
                <div class="flex-between"><label>Total Post Width</label><span class="val-badge" id="val-width">600px</span></div>
                <input type="range" id="sl-width" min="300" max="2500" value="600" step="10">
            </div>

            <div class="input-group">
                <div class="flex-between"><label>Body Text Size</label><span class="val-badge" id="val-text">24px</span></div>
                <input type="range" id="sl-text" min="14" max="500" value="24">
            </div>
            
            <div class="input-group">
                <div class="flex-between"><label>Body Font Boldness (Weight)</label><span class="val-badge" id="val-weight">400</span></div>
                <input type="range" id="sl-weight" min="100" max="900" value="400" step="100">
            </div>

            <div style="margin-top: auto; padding-top: 20px; display: flex; flex-direction: column; gap: 10px;">
                <!-- ADDED: Auto button to perfectly reset standard state -->
                <button class="btn btn-outline" id="btn-auto">
                    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"/><path d="M3 3v5h5"/></svg>
                    Auto Reset (Default)
                </button>
                <button class="btn btn-action" id="btn-render">
                    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
                    Download 4K Quality Overlay
                </button>
            </div>

        </div>
    </aside>

    <div class="toast" id="toast">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="20 6 9 17 4 12"/></svg>
        <span>Render Saved Successfully!</span>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const DOM = {
                inpName: document.getElementById('inp-name'),
                inpUser: document.getElementById('inp-user'),
                inpBody: document.getElementById('inp-body'),
                inpAvatar: document.getElementById('inp-avatar'),
                
                outName: document.getElementById('out-name'),
                outUser: document.getElementById('out-user'),
                outBody: document.getElementById('out-body'),
                outAvatar: document.getElementById('out-avatar'),
                target: document.getElementById('render-target'),
                stage: document.getElementById('stage'),
                
                badgeMain: document.getElementById('badge-main'), 
                badgeBg: document.getElementById('badge-bg'),
                
                slAvatar: document.getElementById('sl-avatar'),
                slName: document.getElementById('sl-name'),
                slUser: document.getElementById('sl-user'),
                slWidth: document.getElementById('sl-width'),
                slText: document.getElementById('sl-text'),
                slWeight: document.getElementById('sl-weight'),
                
                cpBg: document.getElementById('cp-bg'),
                hexBg: document.getElementById('hex-bg'),
                cpText: document.getElementById('cp-text'),
                hexText: document.getElementById('hex-text'),
                
                cpAvatarBg: document.getElementById('cp-avatar-bg'),
                hexAvatarBg: document.getElementById('hex-avatar-bg'),
                cpUserText: document.getElementById('cp-user-text'),
                hexUserText: document.getElementById('hex-user-text'),
                
                cpQuoteBg: document.getElementById('cp-quote-bg'),
                hexQuoteBg: document.getElementById('hex-quote-bg'),
                cpQuoteText: document.getElementById('cp-quote-text'),
                hexQuoteText: document.getElementById('hex-quote-text'),
                
                cpParenBg: document.getElementById('cp-paren-bg'),
                hexParenBg: document.getElementById('hex-paren-bg'),
                cpParenText: document.getElementById('cp-paren-text'),
                hexParenText: document.getElementById('hex-paren-text'),
                
                cpBadge: document.getElementById('cp-badge'), 

                togAlpha: document.getElementById('tog-alpha'),
                btnRender: document.getElementById('btn-render'),
                btnAuto: document.getElementById('btn-auto'), // NEW
                toast: document.getElementById('toast')
            };

            let isAlphaMode = false;

            const parseMarkdown = (text) => {
                let safe = text.replace(/</g, "&lt;").replace(/>/g, "&gt;");
                // Existing Bold
                safe = safe.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>'); 
                safe = safe.replace(/\*(.*?)\*/g, '<strong>$1</strong>');     
                // New Quote Highlight ""
                safe = safe.replace(/"(.*?)"/g, '<span class="highlight-quote">"$1"</span>');
                // New Paren Highlight ()
                safe = safe.replace(/\((.*?)\)/g, '<span class="highlight-paren">($1)</span>');
                // Breaks
                return safe.replace(/\n/g, '<br>');
            };

            const syncText = () => {
                DOM.outName.textContent = DOM.inpName.value;
                DOM.outUser.textContent = DOM.inpUser.value;
                DOM.outBody.innerHTML = parseMarkdown(DOM.inpBody.value);
            };

            DOM.inpName.addEventListener('input', syncText);
            DOM.inpUser.addEventListener('input', syncText);
            DOM.inpBody.addEventListener('input', syncText);
            syncText();

            DOM.inpAvatar.addEventListener('change', (e) => {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    // CHANGED: Use backgroundImage to perfectly cover and crop without HTML2Canvas distortion
                    reader.onload = (ev) => DOM.outAvatar.style.backgroundImage = `url('${ev.target.result}')`;
                    reader.readAsDataURL(file);
                }
            });

            const bindSlider = (node, cssVar, displayId, isPx = true) => {
                node.addEventListener('input', (e) => {
                    const val = isPx ? `${e.target.value}px` : e.target.value;
                    document.documentElement.style.setProperty(cssVar, val);
                    document.getElementById(displayId).textContent = val;
                });
            };

            bindSlider(DOM.slAvatar, '--dyn-avatar-size', 'val-avatar');
            bindSlider(DOM.slName, '--dyn-name-size', 'val-name');
            bindSlider(DOM.slUser, '--dyn-user-size', 'val-user');
            bindSlider(DOM.slWidth, '--dyn-width', 'val-width');
            bindSlider(DOM.slText, '--dyn-text-size', 'val-text');
            bindSlider(DOM.slWeight, '--dyn-font-weight', 'val-weight', false);

            const bindColor = (picker, hexDisplay, cssVar) => {
                picker.addEventListener('input', (e) => {
                    const color = e.target.value;
                    hexDisplay.textContent = color.toUpperCase();
                    document.documentElement.style.setProperty(cssVar, color);
                });
            };

            bindColor(DOM.cpText, DOM.hexText, '--post-text-color');
            bindColor(DOM.cpAvatarBg, DOM.hexAvatarBg, '--avatar-bg-color');
            bindColor(DOM.cpUserText, DOM.hexUserText, '--post-muted-color');
            
            // New Highlight Bindings
            bindColor(DOM.cpQuoteBg, DOM.hexQuoteBg, '--quote-bg-color');
            bindColor(DOM.cpQuoteText, DOM.hexQuoteText, '--quote-text-color');
            bindColor(DOM.cpParenBg, DOM.hexParenBg, '--paren-bg-color');
            bindColor(DOM.cpParenText, DOM.hexParenText, '--paren-text-color');

            // Force-Update Badge SVG Color (Direct overwrite fixes html2canvas bug)
            DOM.cpBadge.addEventListener('change', (e) => {
                const color = e.target.value;
                document.documentElement.style.setProperty('--post-verified-color', color);
                DOM.badgeMain.setAttribute('fill', color); 
            });

            DOM.cpBg.addEventListener('input', (e) => {
                const color = e.target.value;
                DOM.hexBg.textContent = color.toUpperCase();
                document.documentElement.style.setProperty('--post-bg-color', color);
                if (!isAlphaMode) DOM.badgeBg.style.fill = color;
            });

            DOM.togAlpha.addEventListener('click', () => {
                isAlphaMode = !isAlphaMode;
                DOM.togAlpha.classList.toggle('on');
                DOM.target.classList.toggle('alpha');
                DOM.stage.classList.toggle('alpha-mode');
                DOM.badgeBg.style.fill = isAlphaMode ? 'transparent' : DOM.cpBg.value;
            });

            // ADDED: Auto reset feature
            DOM.btnAuto.addEventListener('click', () => {
                const triggerInput = (el, val) => { el.value = val; el.dispatchEvent(new Event('input')); };
                
                // Text Defaults
                DOM.inpName.value = "India Globe Cine";
                DOM.inpUser.value = "@india_globecine";
                DOM.inpBody.value = "\"What If The System Completely Failed?\"\n— *India Globe Cine* Investigates The Latest (National Exam) Security Concerns";
                
                // Sliders Defaults
                triggerInput(DOM.slAvatar, 48);
                triggerInput(DOM.slName, 16);
                triggerInput(DOM.slUser, 15);
                triggerInput(DOM.slWidth, 600);
                triggerInput(DOM.slText, 24);
                triggerInput(DOM.slWeight, 400);

                // Colors Defaults
                triggerInput(DOM.cpBg, "#000000");
                triggerInput(DOM.cpText, "#e7e9ea");
                triggerInput(DOM.cpAvatarBg, "#16181c");
                triggerInput(DOM.cpUserText, "#71767b");
                triggerInput(DOM.cpQuoteBg, "#b91c1c");
                triggerInput(DOM.cpQuoteText, "#ffffff");
                triggerInput(DOM.cpParenBg, "#047857");
                triggerInput(DOM.cpParenText, "#ffffff");

                // Badge Default
                DOM.cpBadge.value = "#1d9bf0";
                DOM.cpBadge.dispatchEvent(new Event('change'));

                // Alpha Default
                if (isAlphaMode) {
                    DOM.togAlpha.click();
                }

                // Restore Default Avatar Image Map perfectly
                DOM.outAvatar.style.backgroundImage = `url("data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0OCIgaGVpZ2h0PSI0OCIgdmlld0JveD0iMCAwIDQ4IDQ4Ij48cmVjdCB3aWR0aD0iNDgiIGhlaWdodD0iNDgiIGZpbGw9IiMxNjE4MWMiLz48Y2lyY2xlIGN4PSIyNCIgY3k9IjE4IiByPSI4IiBmaWxsPSIjNTM2NDcxIi8+PHBhdGggZD0iTTEwIDQwQzEwIDMyIDE2IDI4IDI0IDI4QzMyIDI4IDM4IDMyIDM4IDQwIiBzdHJva2U9IiM1MzY0NzEiIHN0cm9rZS13aWR0aD0iNCIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIi8+PC9zdmc+")`;
                
                syncText();
            });

            DOM.btnRender.addEventListener('click', async () => {
                const ogText = DOM.btnRender.innerHTML;
                DOM.btnRender.innerHTML = 'RENDERING MATRIX...';
                DOM.btnRender.style.opacity = '0.5';

                try {
                    const canvas = await html2canvas(DOM.target, {
                        backgroundColor: isAlphaMode ? null : DOM.cpBg.value,
                        scale: 4, 
                        logging: false,
                        useCORS: true
                    });

                    const link = document.createElement('a');
                    link.download = `Cine-Overlay-${Date.now()}.png`;
                    link.href = canvas.toDataURL('image/png');
                    link.click();

                    DOM.toast.classList.add('show');
                    setTimeout(() => DOM.toast.classList.remove('show'), 3000);
                } catch (err) {
                    console.error('Render failed:', err);
                    alert('Render calculation failed. Check console.');
                } finally {
                    DOM.btnRender.innerHTML = ogText;
                    DOM.btnRender.style.opacity = '1';
                }
            });
        });
    </script>
</body>
</html>
