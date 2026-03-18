# HaiLite — Full Details Single Reconstruction Prompt

> Paste this entire prompt into any capable AI to rebuild the HaiLite app exactly.

---

## PROMPT

Build a complete, single-file `index.html` web application called **HaiLite** — an intelligent multi-API AI chat assistant. All HTML, CSS, and JavaScript must be in one file. Below are the full specifications.

---

## 1. META / HEAD / PWA

- Title: `HaiLite — Intelligent Multi-API AI Assistant`
- Full SEO meta tags (description, keywords, author, robots, theme-color `#17181a`, canonical `https://hailite.vercel.app`)
- Full Open Graph tags (type, url, title, description, image `/favicon.png` 512×512)
- Full Twitter Card tags (card: summary)
- PWA: `<link rel="manifest" href="/manifest.json">`, apple-touch-icon, apple-mobile-web-app-capable, apple-mobile-web-app-status-bar-style: black-translucent, mobile-web-app-capable
- On load: try to register `/sw.js`; if it fails, generate a service worker from a Blob with cache name `hailite-v1`, caching `['/', 'index.html', '/favicon.png', '/logo.png']`, with install/activate/fetch handlers
- Inline manifest injection: fetch `/manifest.json`; if it fails, create a Blob manifest and replace the link href. Manifest fields: name/short_name = HaiLite, display: standalone, bg/theme `#17181a`, two icons from `/favicon.png` (192 and 512, 512 is maskable)
- Vercel Analytics: `window.va` stub + `defer src="/_vercel/insights/script.js"`
- Vercel Speed Insights: `window.si` stub + `defer src="/_vercel/speed-insights/script.js"`
- Fonts: Google Fonts — Inter (300,400,500,600,700) + JetBrains Mono (300,400,500)
- Material Symbols Outlined from Google CDN (variable font: opsz 20–48, wght 100–700)
- Script: `https://cdn.jsdelivr.net/npm/marked/marked.min.js`

---

## 2. CSS DESIGN SYSTEM

### CSS Variables (`:root`)
```
--bg: #17181a
--surface: #202225
--surface2: #2c2f33
--surface3: #35383c
--text: #f0f2f5
--text-muted: #a8b3cf
--text-active: #82aaff
--accent: #82aaff
--accent-ai: #c792ea
--border: #3a3d42
--danger: #ff8a80
--green: #69db7c
--sidebar-w: 260px
--radius-sm: 8px  --radius-md: 12px  --radius-lg: 16px  --radius-xl: 20px
--font: 'Inter', sans-serif
--font-mono: 'JetBrains Mono', monospace
--write-area-h: 120px
--msg-size: 15px
--transition: 0.2s cubic-bezier(0.4, 0, 0.2, 1)
```

### Material Symbols class `.ms`
Font-family Material Symbols Outlined, inline-block, user-select none, font-size 20px, font-variation-settings FILL 0 wght 300 GRAD 0 opsz 24.

### Global Reset
`*, *::before, *::after { box-sizing:border-box; margin:0; padding:0; -webkit-tap-highlight-color:transparent }`  
`html, body { height:100%; overflow:hidden; background:var(--bg); color:var(--text); font-family:var(--font); font-size:16px; }`  
`body { display:flex; height:100dvh; }`

### Scrollbar
Width 4px, transparent track, `--surface3` thumb with 4px radius, hover `--text-muted`.

### Overlays (`.overlay`)
Fixed, inset 0, rgba(0,0,0,0.55), z-index 99, opacity 0, pointer-events none, transition opacity, backdrop-filter blur(2px). `.overlay.show` → opacity 1, pointer-events all.

### Sidebar (`#sidebar`)
Fixed left 0, top 0, bottom 0, width `--sidebar-w`, background `--surface`, border-right `--border`, flex column, z-index 200, transform translateX(-100%), transition 0.3s cubic-bezier. `.open` → translateX(0).

- `.sidebar-header` — padding 16/14/10/14, flex column gap 10, border-bottom `--border`
- `.sidebar-title-row` — flex row, space-between
- `.sidebar-title` — 14px, weight 700, uppercase, letter-spacing 0.5px
- `.sidebar-new-btn` — full width, surface2 bg, border `--border`, radius-md, 9/14 padding, 13px font, hover surface3
- `.sidebar-search` — surface2, border, radius-sm, gap 8, 7/10 padding; focus-within border `--accent`
- `.sidebar-history` — flex 1, overflow-y auto, padding 10px
- `.history-section-label` — 10px, 700, uppercase, letter-spacing 1px
- `.history-item` — flex, gap 8, 9/8 padding, radius-sm, cursor pointer; hover/active: surface2
- `.history-item-text` — 13px, ellipsis, no-wrap
- `.history-empty` — 13px, text-muted, center, padding 32px 0

### Settings Panel (`#settings-sheet`)
Fixed right 0, top 0, bottom 0, width `--sidebar-w`, background `--surface`, border-left `--border`, flex column, z-index 200, transform translateX(100%). `.open` → translateX(0).

- `.settings-header` — flex, space-between, 16/14/10 padding, border-bottom `--border`
- `.settings-content` — flex 1, overflow-y auto, 14px padding, flex column, gap 8
- `.setting-group` — flex column, gap 6
- `.setting-label` — 11px, 600, text-muted, uppercase, letter-spacing 0.8px
- `.setting-input` — surface2, border, radius-sm, 9/11 padding, font-mono 12px, focus border-color `--accent`
- `.range-row` — flex, align-center, gap 10; `input[type=range]` flex 1, accent-color `--accent`
- `.range-val` — font-mono 12px, min-width 42px, text-right
- `.setting-btn` — full width, flex, gap 10, 10/4 padding, transparent bg, 13px, cursor pointer, radius-sm; hover surface2; `.danger` color `--danger`
- `.settings-divider` — border-top `--border`, margin 4 0
- `.settings-footer` — 10/14/14 padding, 11px, text-muted, center, line-height 1.5; `strong` weight 600

### Icon Buttons (`.icon-btn`)
36×36px, radius-sm, transparent bg/border, flex center, text-muted, transition all. Hover: surface2, color text. `.ms` 20px.

### Bottom Sheets (`.bottom-sheet`)
Fixed left/right 0, bottom 0, background `--surface`, z-index 300, border-radius 20 20 0 0, border-top `--border`, transform translateY(100%), transition 0.32s cubic-bezier. `.open` → translateY(0).

- `.sheet-handle` — 36×4px, radius 2, surface3, margin 12 auto 6, cursor grab
- `.sheet-header` — flex, space-between, padding 4/16/12
- `.sheet-title` — 16px, weight 700
- `.sheet-close` — 30×30, radius-sm, surface2, flex center, text-muted; hover surface3

#### Model Sheet
Max-height 70dvh, flex column. `.model-option` — flex, space-between, 13/18 padding, hover surface2. `.model-option-name` 14px 600, `.model-option-id` 11px mono text-muted. `.model-option-check` 18px accent, opacity 0; selected → opacity 1.

#### Filter Sheet
Max-height 60dvh, z-index 314. `.filter-mode-row` — flex, space-between, 12/18 padding, hover surface2. Left: flex, gap 12, 38×38 icon wrap (surface2 radius-md), name 14px 600, sub 11px text-muted. Check: 18px accent, opacity 0; `.selected` → opacity 1.

#### Upload Sheet
`.upload-option-row` — flex, gap 14, 14/18 padding, hover surface2. Icon wrap 40×40, radius-md, surface2. Label 14px 600, sub 11px text-muted.

#### User Actions Sheet
z-index 314. `.user-action-row` — flex, gap 14, 14/18 padding, hover surface2. Icon 36×36, radius-md, surface2. Label 14px.

#### API Detail Sheet
z-index 315, max-height 80dvh, flex column. `.api-detail-body` padding 0/16/24, flex column, gap 8, overflow-y auto. `.api-section-title` 10px 700 uppercase letter-spacing 0.8px padding 8/0/2. `.api-item-card` surface2 bg, radius-md, 12/14 padding, flex column, gap 4, border `--border`. Name 13px 700, desc 11px text-muted, URL 11px mono, bg `--bg`, 5/8 padding, radius 6, word-break break-all.

### Top Bar (`.top-bar`)
Fixed top/left/right 0, flex, space-between, 8/10 padding, rgba(23,24,26,0.85) bg, backdrop-filter blur(16px), border-bottom rgba(58,61,66,0.6), z-index 50. Left/right: flex, gap 2.

`.chat-logo` — flex, gap 8, margin-left 4, pointer-events none. Logo wrap 26×26, radius 8, surface2, overflow hidden. Logo name 15px 700, letter-spacing -0.3px.

### Main (`#main`)
Flex 1, flex column, height 100dvh, overflow hidden.

### Welcome Screen (`#welcome-screen`)
Position absolute, inset 0, flex column center, z-index 25, padding-bottom 100px. Transition opacity/transform 0.3s ease. `.hidden` → opacity 0, scale 0.97, pointer-events none.

- Logo wrap 68×68, radius-lg, gradient surface2→surface3, border `--border`, animation logoFloat 3s ease-in-out infinite (0%/100%: translateY(0), 50%: translateY(-6px))
- `.welcome-title` 28px 700, letter-spacing -0.5px, margin-bottom 6px
- `.welcome-subtitle` 14px text-muted, margin-bottom 24px
- `#suggestion-chips` flex wrap, justify center, gap 8, max-width 400px
- `.chip-btn` surface2 bg, border, radius 20, 7/14 padding, 13px, cursor pointer; hover surface3, color text, border-color `--accent`

### Chat (`#chat`)
Flex 1, overflow-y auto, overflow-x hidden, flex column, padding 58px 0 calc(write-area-h + safe-area + 10px), -webkit-overflow-scrolling touch.

### Message Groups (`.msg-group`)
Flex column, padding 0/16, margin-bottom 16, animation msgIn 0.25s cubic-bezier(0.34,1.56,0.64,1) (from opacity 0 translateY(12px) scale(0.98) to opacity 1 translateY(0) scale(1)).

- `.msg-group.user` align-items flex-end
- `.msg-group.ai` align-items flex-start
- `.ai-content` width 100%, flex column, gap 6

### User Bubble (`.user-bubble`)
surface2 bg, 10/15 padding, border-radius 18 18 4 18, font-size `--msg-size`, line-height 1.65, max-width 82%, word-break break-word, cursor pointer, border `--border`, transition bg. Hover surface3.

`.user-edit-area` — max-width 82%, surface2, border 1px solid `--accent`, radius 12, 10/14 padding, font var, font-size `--msg-size`, outline none, resize none, min-height 44px.  
`.user-edit-actions` flex gap 6, margin-top 6.  
`.edit-confirm-btn` 30px height, radius-sm, `--accent` bg, `--bg` color, no border, 0/14 padding, font 12px 600.  
`.edit-cancel-btn` 30px height, radius-sm, surface2, border `--border`, 0/14 padding, 12px.

### AI Text (`.ai-text`)
font-size `--msg-size`, line-height 1.8, font var, weight 400, color text, word-break break-word.
- `p` margin-bottom 8px, last-child 0
- `h1/h2/h3` weight 700, margin 14 0 6; h1 20px, h2 17px, h3 15px
- `ul/ol` padding-left 20, margin-bottom 8; `li` margin-bottom 4
- `code` (inline) surface2 bg, radius 5, 1/6 padding, font-mono 12px, color #e06c75, border `--border`
- `pre` bg `--bg`, radius-md, padding 0, overflow hidden, margin-bottom 8
- `pre code` bg none, 14px padding, color text, display block, overflow-x auto, 12.5px, no border
- `blockquote` 8/12 padding, border-left 3px solid `--accent`, text-muted, margin-bottom 8, rgba(130,170,255,0.06) bg, border-radius 0 radius-sm radius-sm 0
- `a` color `--accent`, underline, underline-offset 3px
- `table` border-collapse collapse, width 100%, margin-bottom 8, 13px
- `th/td` 7/10 padding, border `--border`; `th` surface2 bg, weight 600
- `img` radius-md, max-width 100%, block, margin 6 0
- `hr` border-top `--border`, margin 12 0
- `.ai-disclaimer` 11px text-muted, margin-top 6px, opacity 0.6

### Code Block Wrapper (`.code-block-wrapper`)
`--bg` bg, radius-md, margin 8 0, overflow hidden, border `--border`.
- Header: flex space-between, 8/12 padding, surface2 bg, border-bottom `--border`
- Lang label: font-mono 11px text-muted uppercase letter-spacing 0.5px
- Copy/Toggle buttons: transparent, font-mono-color text-muted, flex, gap 4, 3/7 padding, radius 6, 12px, font var; hover surface3, text; `.ms` 16px
- `.collapsed pre` display none

### Rich Link Cards (`.rich-link-card`)
Flex, gap 10, `--surface` bg, border `--border`, 10/12 padding, radius-md, margin 6 0, overflow hidden. Hover border-color text-muted. 
- Favicon 26×26, radius 6, object-fit contain, surface2. Placeholder same size, surface2, flex center; `.ms` 15px text-muted
- Info: flex 1, min-width 0. Title 12px 600 ellipsis no-wrap. URL 10px text-muted mono, ellipsis
- Actions: flex, gap 3. Buttons 28×28, radius-sm, surface2, flex center, text-muted; hover surface3
- `.image-card / .video-card / .audio-card` flex-direction column, align-items stretch
- `.rlc-top` flex, gap 10, width 100%
- `.rlc-image-wrap` relative, margin-top 8, radius-sm, overflow hidden; `img` width 100%, display block, object-fit cover, max-height 240px
- `.rlc-img-save` position absolute, bottom/right 8, 32×32, radius-sm, rgba(0,0,0,0.6) bg, flex center, text-decoration none, backdrop-filter blur(4px); hover rgba(0,0,0,0.8). `.ms` 18px #fff

### Status Messages
`.status-msg` flex column, padding 4 0, bg transparent, width fit-content, max-width 92%.  
`.status-header` flex, gap 8, margin-bottom 4.  
`.status-icon-wrap` 18×18, flex center.  
`.status-hex` 17px, color `--accent`, animation spin 1.8s linear infinite, Material Symbols Outlined FILL 1 wght 600.  
`@keyframes spin { 100% { transform: rotate(360deg) } }`  
`.status-logo-img` hidden 16×16, radius 4.  
`.status-complete .status-hex` animation none, display none.  
`.status-complete .status-logo-img` display block.  
`.status-title` 12px weight 500 text-muted. `.status-complete .status-title` 11px.  
`.status-steps` flex column, gap 3.  
`.step-item` flex, align flex-start, gap 5, 12px text-muted. `.ms` 13px margin-top 1.  
`.step-item.done .ms` color `--green`. `.step-item.error .ms` color `--danger`.  
`.status-complete` padding-bottom 8, margin-bottom 10. `.msg-group.status-done` margin-bottom 4.

Aurora shimmer animation on non-done/error step text:
```css
background: linear-gradient(100deg, text-muted 0%, text 25%, accent 45%, text 65%, text-muted 85%);
background-size: 300% 100%;
-webkit-background-clip: text; background-clip: text;
color: transparent;
animation: aurora-shine 2s ease-in-out infinite;
@keyframes aurora-shine { 0% { background-position: 200% center } 100% { background-position: -100% center } }
```
Also `.loading-shine` class for status title using same animation (2.2s).  
`.status-complete .status-title.loading-shine` removes animation, resets to text-muted.

`.error-block` rgba(255,138,128,0.1) bg, border rgba(255,138,128,0.3), radius-md, 10/14 padding, margin-top 8, max-width 82%, 13px, color `--danger`, flex, gap 8.

### Message Actions (`.msg-actions`)
Flex, align-center, margin-top 4, gap 2, opacity 0, transition 0.15s, flex-wrap wrap.  
`.msg-group:hover .msg-actions, .msg-group:last-child .msg-actions` opacity 1.  
`.act-btn` height 26, radius-sm, no bg/border, cursor, flex center, 0/6 padding, gap 3, text-muted; hover surface2 color text. `.ms` 14px.  
`.act-btn-lbl` 11px text-muted.  
`.used-api-badge` surface2 bg, border `--border`, radius 20, padding 2/8.

User attach row: `.user-attach-row` flex wrap gap 5, margin-bottom 6, justify flex-end. `.user-attach-tag` flex, gap 4, surface2, border, radius 8, 4/8 padding, 11px mono, text-muted, max-width 180px.

### Write Area (`#write-area`)
Fixed bottom/left/right 0, padding 10/12/calc(10+safe-area), rgba(23,24,26,0.95) bg, backdrop-filter blur(20px), border-top `--border`, z-index 40.

`.write-bar` surface bg, border `--border`, radius-xl, flex column, 10/12/8/16 padding, max-width 860px, margin 0 auto, transition border-color. Focus-within: border rgba(130,170,255,0.5), box-shadow 0 0 0 3px rgba(130,170,255,0.08).

`.write-bar-inner` position relative, width 100%, flex column.  
`.write-bar textarea` width 100%, transparent bg, no border/outline, resize none, font var, 14px, color text, line-height 1.55, max-height 140px, min-height 38px, padding 3/4.  
Placeholder: text-muted.  
`.write-bar-bottom` flex, space-between, margin-top 6, gap 2.  
Left/Right sections: flex, gap 2.

`#model-pill-btn` height 30, radius 20, surface2 bg, border, flex gap 5, 0/11 padding, 12px 500, text-muted, no-wrap; hover surface3 color text. `.ms` 14px.  
`.write-tool-btn` 34×34, radius-sm, transparent, flex center, text-muted; hover surface2 text. `.ms` 20px.  
`#filter-btn` 34px height, radius-sm, transparent, flex center, text-muted, 0/6 padding; hover surface2. `.ms` 20px. `.active .ms` color `--accent`, font-variation FILL 1 wght 500.  
`.write-btn` 34×34, radius-sm, transparent, flex center, text-muted; hover surface2.  
`.send-btn` 34×34, radius-sm, surface2 bg, border, flex center, text-muted, transition all. Hover: `--accent` bg, `--bg` color, `--accent` border. `.stop-mode` rgba(255,138,128,0.15) bg, `--danger` color/border. `.ms` 18px.

Attach chip styles: `.attached-files` flex wrap, gap 6, 0/0/6 padding. `.attach-chip` flex, gap 5, surface2, border, radius-sm, 5/8 padding, max-width 200px. `.attach-chip-name` 11px mono, ellipsis. `.attach-chip-remove` 16×16, radius 4, transparent, cursor. `.attach-loading` flex, gap 6, 5/8 padding, surface2, border, radius-sm. Spinner 12×12, border 2px surface3, border-top `--accent`, radius 50%, animation spin 0.7s. Text 11px mono text-muted.

STT styles: `.stt-lang-opt` flex, gap 10, 8/4 padding, cursor, radius-sm; hover surface2. Dot 12×12, radius 50%, border 2px text-muted; `.selected` bg + border `--accent`. Name 13px text. Code 10px mono text-muted, margin-left auto. `.stt-interim` 13px text-muted italic, 3/4 padding.

Scroll Down Button (`#scroll-down-btn`): fixed bottom calc(write-area-h + 10), left 50%, transform translateX(-50%) scale(0.8), 34×34, radius 50%, surface2, border, flex center, text-muted, opacity 0, pointer-events none, transition all, z-index 45, box-shadow 0 4px 12px rgba(0,0,0,0.4). `.show` → opacity 1, pointer-events all, scale 1. Hover surface3.

Toast (`.toast`): fixed bottom calc(write-area-h + 56), left 50%, transform translateX(-50%) translateY(8px), surface2 bg, border, text, 13px, 8/16 padding, radius 20, opacity 0, pointer-events none, transition 0.25s, z-index 500, box-shadow 0 4px 20px rgba(0,0,0,0.4), no-wrap, backdrop-filter blur(12px). `.show` → opacity 1, translateY(0).

Focus visible: outline 2px solid `--accent`, offset 2px, radius 4px.

Responsive:
- `@media (max-width:600px)`: write-bar 8/10/6/12 padding, textarea 15px, msg-group 0/10 padding
- `@media (min-width:900px)`: chat padding-left/right calc((100% - 860px)/2 + 16px)

---

## 3. HTML BODY STRUCTURE

### Overlays
Three overlays: `#sidebar-overlay`, `#sheet-overlay`, `#settings-overlay`.

### Left Sidebar (`#sidebar`)
- Header: title "Conversations" + close btn (`#sidebar-close-btn`)
- Search input `#history-search` placeholder "Search chats…"
- Button `#sidebar-new-chat-btn` (add icon + "New Chat")
- History list `#sidebar-history` (default: `<div class="history-empty">No conversations yet</div>`)

### Settings Panel (`#settings-sheet`)
- Header: "Settings" title + close btn `#settings-close-btn`
- Content (flex column, gap 8):
  - API Key group: `<input type="password" id="setting-api-key" placeholder="gsk_...">`
  - Temperature range 0–2 step 0.05 default 1, display `#temp-val`
  - Max Tokens range 256–8192 step 64 default 2048, display `#tokens-val`
  - Message Font Size range 12–22 step 1 default 15, display `#msg-size-val`
  - Voice Language section: `<div id="stt-lang-options">`
  - Buttons: `#save-settings-btn` (save icon), `#reset-settings-btn` (restart_alt icon)
  - Divider
  - Buttons: API Source (opens `https://apis.prexzyvilla.site`), Groq Cloud (opens `https://groq.com`), `#download-source-btn`, `#connect-dev-btn`
  - Divider
  - Danger buttons: `#delete-chats-btn`, `#delete-all-data-btn`
- Footer: "**HaiLite** — Created by **HaiLite**"

### Model Sheet (`#model-sheet`, bottom-sheet)
- Handle `#model-handle`, title "Choose Model", close `#model-close-btn`
- `<div id="model-options">` (dynamically populated)

### Filter Mode Sheet (`#filter-sheet`, bottom-sheet)
- Handle `#filter-handle`, title "Filter Mode", close `#filter-close-btn`
- Five static rows in `#filter-options` (each with `data-mode` attribute):
  1. `data-mode=""` (selected by default) — chat icon, "Default", "Normal conversation mode"
  2. `data-mode="download"` — download icon, "Download Links", "Find direct download links"
  3. `data-mode="image"` — image icon, "Create Image", "Generate images via available APIs"
  4. `data-mode="sound"` — music_note icon, "Search Sound / Music", "Find audio tracks and sound effects"
  5. `data-mode="apis"` — api icon, "Search APIs", "Explore and list available APIs"
- Each row has `.filter-mode-check` (check_circle icon)

### User Actions Sheet (`#user-actions-sheet`, bottom-sheet)
Handle `#user-actions-handle`, title "Message", close `#user-actions-close-btn`.  
Three rows in `#user-actions-body`: `#ua-edit` (edit icon), `#ua-copy` (content_copy), `#ua-retry` (refresh).

### Upload Sheet (`#upload-sheet`, bottom-sheet)
Handle `#upload-handle`, title "Attach File", close `#upload-close-btn`.  
Two rows: `#upload-image-ocr` (image_search icon, "Image OCR", "Extract text from a photo or screenshot"), `#upload-text-doc` (description icon, "Text Document", "Upload .txt, .md, .csv, .json…").

### API Detail Sheet (`#api-detail-sheet`, bottom-sheet)
Handle `#api-detail-handle`, title `#api-sheet-title`, close `#api-detail-close-btn`.  
Body: `<div class="api-detail-body" id="api-detail-body">`.

### Top Bar (`.top-bar`)
- Left: `#sidebar-open-btn` (menu), logo (logo.png → fallback /favicon.png, name "HaiLite")
- Right: `#new-chat-btn` (add_comment), `#settings-open-btn` (tune)

### Main (`#main`)
- Welcome screen `#welcome-screen`:
  - logo.png (fallback /favicon.png)
  - Title "HaiLite"
  - Subtitle "Your intelligent multi-API assistant"
  - `<div id="suggestion-chips">`
- Chat `<div id="chat">`
- Write Area `#write-area`:
  - Write bar with inner:
    - `<div class="attached-files" id="attached-files" style="display:none;">`
    - `<textarea id="write-input" rows="1" placeholder="Message HaiLite…">`
  - Bottom row:
    - Left: `#model-pill-btn` (psychology icon + `<span id="model-pill-label">Kimi K2</span>`)
    - Right:
      - `<input type="file" id="file-input-ocr" accept="image/*" style="display:none">`
      - `<input type="file" id="file-input-doc" accept=".txt,.md,.csv,.json,.xml,.html,.js,.py,.css,.log" style="display:none">`
      - `#filter-btn` (filter_alt icon)
      - `#add-btn` write-tool-btn (attach_file icon)
      - `#voice-btn` write-btn (mic icon)
      - `#send-btn` send-btn (arrow_upward icon)

### Other
- `#scroll-down-btn` (keyboard_arrow_down icon)
- `<div id="toast" class="toast">`

---

## 4. JAVASCRIPT — FULL LOGIC

Wrap all logic in `document.addEventListener('DOMContentLoaded', function() { ... })`.

### Constants & State
```js
const DEFAULT_API_KEY = "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";
const DEFAULT_MODEL_ID = 'moonshotai/kimi-k2-instruct';
const DEFAULT_TEMPERATURE = 1, DEFAULT_MAX_TOKENS = 2048, DEFAULT_MSG_SIZE = 15, DEFAULT_STT_LANG = 'auto';
const FALLBACK_KEYS = [DEFAULT_API_KEY];

const STT_LANGUAGES = [
  { label:'Auto Detect', code:'auto', bcp:'' },
  { label:'English', code:'en', bcp:'en-US' },
  { label:'Bangla', code:'bn', bcp:'bn-BD' },
  { label:'Hindi', code:'hi', bcp:'hi-IN' },
];

const MODELS = [
  { label:'Kimi K2', id:'moonshotai/kimi-k2-instruct' },
  { label:'Llama 3.3 70B', id:'llama-3.3-70b-versatile' },
  { label:'Llama 3.1 8B', id:'llama-3.1-8b-instant' },
  { label:'Llama 4 Scout', id:'meta-llama/llama-4-scout-17b-16e-instruct' },
  { label:'Gemma 2 9B', id:'gemma2-9b-it' },
  { label:'Mixtral 8x7B', id:'mixtral-8x7b-32768' },
];

const FILTER_MODES = [
  { mode:'', label:'Default', placeholder:'Message HaiLite…' },
  { mode:'download', label:'Download Links', placeholder:'What do you want to download…' },
  { mode:'image', label:'Create Image', placeholder:'Describe the image to create…' },
  { mode:'sound', label:'Sound / Music', placeholder:'Search for sound or music…' },
  { mode:'apis', label:'Search APIs', placeholder:'Find APIs for…' },
];

const SUGGESTIONS = ['Write code','Explain a concept','Translate text','Summarize article','Generate ideas','Debug my code'];

const BASE_API_URL = "https://apis.prexzyvilla.site";
const PROXY = "https://corsproxy.io/?url=";
const LIST_URL = PROXY + encodeURIComponent("http://apis.prexzyvilla.site/endpoints");
const CHAT_LIST_KEY = 'html_ai_chats';
```

Mutable state:
```js
let activeFilterMode = '';
let API_KEY = localStorage.getItem('html_ai_key') || DEFAULT_API_KEY;
let MODEL = localStorage.getItem('html_ai_model') || DEFAULT_MODEL_ID;
let TEMPERATURE = parseFloat(localStorage.getItem('html_ai_temp') || DEFAULT_TEMPERATURE);
let MAX_TOKENS = parseInt(localStorage.getItem('html_ai_max_tokens') || DEFAULT_MAX_TOKENS);
let MSG_SIZE = parseInt(localStorage.getItem('html_ai_msg_size') || DEFAULT_MSG_SIZE);
let STT_LANG = localStorage.getItem('html_ai_stt_lang') || DEFAULT_STT_LANG;
let lastSearchedApis = [], lastUsedApis = [], lastErrorApis = [];
let isGenerating = false, abortController = null, hasStartedChat = false, currentChatId = null;
let apiData = [];
let history = [];
let activeUserGroup = null;
let toastTimer = null;
```

### Initialization
- `marked.setOptions({ breaks:true, gfm:true })`
- `applyMsgSize(MSG_SIZE)` — sets `--msg-size` CSS variable
- `applySettingsToUI()` — fills all settings inputs
- `renderModelOptions()`
- `startNewChat()`
- Fetch api list: `fetch(LIST_URL).then(r=>r.json()).then(data => { apiData = Array.isArray(data)?data:(data.categories||data.endpoints||[]); }).catch(()=>{})`
- Populate suggestion chips: add `chip-btn` for each SUGGESTION; on click sets textarea value + fires input event + focuses

### System Prompt (`getSystemPrompt()`)
Returns `{ role:"system", content: "..." }` with:
- Identity: "You are HaiLite, a smart assistant created by HaiLite."
- Media link rules (strictly follow):
  - IMAGE URLs → `<image>url</image>` (inline) or `<image_link>url</image_link>` (card)
  - AUDIO URLs → always `<audio_link>url</audio_link>`
  - VIDEO URLs → always `<video_link>url</video_link>`
  - FILE/DOC URLs → always `<file_link>url</file_link>`
  - Generic web links → `<Title_link>url</Title_link>` or markdown `[text](url)`
  - NEVER use bare plain-text or plain markdown for media URLs
- Tool use (only when needed):
  - `[SEARCH: keyword]` — search internal API catalogue
  - `[FETCH: url]` — fetch a live API endpoint
  - `[LOADING: message]` — optional status update to user
  - `[THINKING: step description]` — optional thinking step
- Note: Include LOADING/THINKING tags only when performing tool actions, not in final answers. After receiving results, give FINAL answer directly. Do not loop unnecessarily.

### Sheet Management
- `openSheet(s)` — closes all other sheets, opens s, shows sheetOverlay
- `closeSheet(s)` — removes open from s, hides sheetOverlay if nothing else open
- `closeAllSheets()` — closes all, hides overlay
- `openSettings()` / `closeSettings()` — uses settingsOverlay

Event bindings for all close buttons, overlays.

`makeDraggable(handleEl, sheetEl)` — touch drag-to-dismiss: track touchstart/move/end, dismiss if dy > 80 or velocity > 0.4px/ms.

Apply to: model, filter, user-actions, api-detail, upload handles.

### Sidebar
- Open/close via `#sidebar-open-btn`, close btn, overlay
- `#sidebar-new-chat-btn` → startNewChat + close sidebar
- `#history-search` → `renderSidebarHistory(value)`

### Settings Logic
- Temperature input → update display label
- Max tokens input → update display label
- Message size input → update display + `applyMsgSize(v)`
- Save btn → read all inputs, save to localStorage (keys: html_ai_key, html_ai_temp, html_ai_max_tokens, html_ai_msg_size, html_ai_stt_lang), update history[0], showToast, closeSettings
- Reset btn → confirm, restore defaults, save to localStorage, update all UI
- Delete chats → confirm, remove CHAT_LIST_KEY + all 'html_ai_chat_*' keys, renderSidebarHistory, startNewChat, showToast, closeSettings
- Delete all data → confirm, localStorage.clear(), reinitialize all state/UI
- Download source → create Blob of `document.documentElement.outerHTML`, trigger download as `html_ai.html`
- Connect dev → `window.open('https://whatsapp.com/channel/0029VbB5SYZ7DAX1oHlYkz18', '_blank')`

`renderSttLangOptions()` — create radio-style rows for each STT_LANGUAGES entry, click updates STT_LANG + re-renders.

### Filter Sheet
- `renderFilterOptions()` — toggle `.selected` on rows by comparing `data-mode` to `activeFilterMode`
- Click on row → set `activeFilterMode`, toggle `filterBtn.active`, update textarea placeholder, re-render, closeSheet, showToast

### Model Sheet
- `renderModelOptions()` — create `.model-option` divs; click sets MODEL to localStorage, updates pill label, re-renders, closeSheet, showToast

### User Message Bubble Handling
- `attachUserBubbleHandlers(group)` — clone bubble, add click → `openUserActionsSheet(group)`
- `reattachUserBubbleHandlers()` — re-applies to all `.msg-group.user`
- `openUserActionsSheet(group)` — stores activeUserGroup, opens sheet

User action buttons:
- Edit (`#ua-edit`) → closeSheet, `enterEditMode(group)`
- Copy (`#ua-copy`) → closeSheet, `safeCopy(bubble.textContent)`
- Retry (`#ua-retry`) → closeSheet, `retryFromUserMessage(group, text)`

`enterEditMode(group)` — hide bubble, insert `<textarea class="user-edit-area">`, add row with Send/Cancel buttons. Cancel restores bubble. Send calls `retryFromUserMessage(group, txt, true)` with updated bubble text.

`retryFromUserMessage(group, text, updateHistory=false)` — remove all messages after group, update history array (trim to before last user or last pair), push new user message, call `processMessage(text)`.

### Message Rendering
`addUserMessage(msg, attachNames)` — create `.msg-group.user` with optional `.user-attach-row` (chips for attached files), `.user-bubble`, attach handlers, scroll, set hasStartedChat, hide welcome screen.

`addStatusMessage(title)` — create `.msg-group.ai` with status-msg structure: hex spinner icon (&#xe863; = settings/hexagon), logo img, loading-shine title, steps list. Returns the inner `.status-msg` element.

`addAIMessage()` — create `.msg-group.ai` with `.ai-content > .ai-text + .msg-actions`. Returns the group element.

`addErrorBlock(msg)` — create `.msg-group.ai > .error-block` with error_outline icon.

Status helpers:
- `updateStatusTitle(sd, txt)` — update title text + add loading-shine class
- `markStatusComplete(sd, steps)` — add status-complete class, set title to "Done in N steps." or "Done."
- `addStep(sd, txt, state)` — append `.step-item` with icon (check_circle/cancel/radio_button_unchecked) + text. Return the element.
- `markLastStepDone(sd)` — find last step-item, add `.done`, set icon to check_circle
- `markLastStepError(sd)` — find last step-item, add `.error`, set icon to cancel

### Rich Link Card System

`getFaviconUrl(url)` — returns `https://www.google.com/s2/favicons?sz=64&domain={hostname}`

`getLinkType(url)` — check lowercase extension:
- `.mp3/.wav/.ogg/.aac/.flac/.m4a` → 'audio'
- `.mp4/.webm/.ogv/.mov/.avi/.mkv` → 'video'
- `.jpg/.jpeg/.png/.gif/.webp/.svg/.bmp/.avif` → 'image'
- `.pdf/.doc/.docx/.xls/.xlsx/.ppt/.pptx/.txt/.zip/.rar/.tar/.gz` → 'file'
- else → 'link'

`getTypeIcon(type)` — maps type to Material Symbol name.

`buildRichLinkCard(title, url, forcedType)` — build `.rich-link-card` with favicon (fallback placeholder with icon), title, url, copy + open buttons. For image type: `.image-card` with `.rlc-image-wrap`, lazy img, and download link. Wire `.rl-copy` to `safeCopy(url)`, `.rl-open` to `window.open(url, '_blank')`.

### AI Content Renderer
`renderAiContent(text, container)`:
1. Strip `<chat_title>...</chat_title>` tags
2. Process typed link tags → collect into `linkEntries[]`:
   - `<image_link>url</image_link>` → type image, label "Image"
   - `<audio_link>url</audio_link>` → type audio, label "Audio"
   - `<video_link>url</video_link>` → type video, label "Video"
   - `<file_link>url</file_link>` → type file, label "File"
   - `<SomeTitle_link>url</SomeTitle_link>` → type null, label from tag name (replace _ with space)
   - Each replaced with `<span data-rlc="i">` placeholder
3. Replace `<image>url</image>` → `![](url)` (inline image in markdown)
4. `container.innerHTML = marked.parse(text)`
5. Replace `[data-rlc]` placeholders with `buildRichLinkCard(...)`
6. Wrap all `<pre>` elements (not already wrapped) in `.code-block-wrapper` with header (lang label from className + Hide/Copy buttons). Hide button toggles `.collapsed` class. Copy button uses `safeCopy(code.innerText)`.

### Type-Writer Effect
`typeText(container, fullText, statusDiv, stepText)` — returns a Promise. Add a step to statusDiv. Tick function: if not generating, render full immediately and resolve. Otherwise render substring up to `idx`, advance `idx` by 3 chars, setTimeout 6ms. On completion, mark step done, resolve.

### API Response Parsing
`parseAIResponse(text)` — extract:
- `[SEARCH: keyword]` → type 'search'
- `[FETCH: url]` → type 'fetch'
- `[LOADING: msg]` → loadingMsg
- `[THINKING: msg]` → thinkingMsg
- Else → type 'answer', cleaned text (strip all tool tags)

### API Fetching
`getAIResponse(messages, signal)` — iterate over keys array `[API_KEY, ...FALLBACK_KEYS without API_KEY]`. For each key:
- POST to `https://api.groq.com/openai/v1/chat/completions`
- Body: `{ model:MODEL, messages, stream:true, temperature:TEMPERATURE, max_tokens:MAX_TOKENS }`
- Stream SSE: collect `choices[0].delta.content` lines until DONE
- On 429/503/502/529: wait 800ms, continue to next key
- On 401: break
- On empty response: continue
- On AbortError: return `{ error:true, aborted:true }`
- Returns full string or `{ error:true, message:lastErr }`

`fetchWithProxy(url)` — prepend PROXY if not already prefixed, fetch, check ok, return `.json()`.

### Filter Application
`applyFilterToMessage(text)` — prepend mode prefix if activeFilterMode:
- download: "Search for and provide direct download links for: "
- image: "Use the image generation API to create an image of: "
- sound: "Search for and provide audio links for sound or music matching: "
- apis: "Search and list all available APIs related to: "

### Main Message Processor
`processMessage(text)` — async, up to 50 rounds:
1. Create statusDiv (`addStatusMessage('Thinking…')`) and aiGroup (`addAIMessage()`)
2. Set generating state, create AbortController
3. Reset api tracking arrays, refresh system prompt
4. Loop:
   - Call `getAIResponse(history, signal)`
   - If error/abort: add error step/block, break
   - `parseAIResponse(resp)` → update status title if loadingMsg present
   - **If type 'search'**: addStep, filter apiData by keywords (across all `cat.items`), format results as markdown table rows with `BASE_API_URL+path={prompt}`, push to lastSearchedApis, markLastStepDone, push resp + results as user message, continue
   - **If type 'fetch'**: addStep, look up in lastSearchedApis for known api, `fetchWithProxy(url)` → push to lastUsedApis or lastErrorApis on fail, markLastStepDone or Error, push resp + fetch result as user message, continue
   - **If type 'answer'**: scan text with `extractApisFromText`, merge into lastSearchedApis, `typeText(aiGroup.querySelector('.ai-text'), parsed.value, statusDiv, 'Rendering response')`, push assistant message to history, `setGeneratingState(false)`, `addActionButtons(...)`, `markStatusComplete(...)`, `saveCurrentChat()`, return
5. On catch: `addErrorBlock('Error: ' + err.message)`
6. Final cleanup: setGeneratingState(false), addActionButtons, markStatusComplete, saveCurrentChat

`extractApisFromText(text)` — extract API entries via two regex patterns:
1. Markdown table rows: `| Name | Desc | URL |`
2. Pattern: `Name: https://url`
Returns array of `{ name, desc, url }`.

### Send Handlers
`handleSend()` — check abort if generating, get trimmed text, `applyFilterToMessage`, `resetTextarea`, `addUserMessage`, push to history, `processMessage`.

`handleSendWithAttach()` — same but merges `attachedContexts` into fullText:
- OCR attachment: `[OCR from image "name"]:\n\`\`\`\ntext\`\`\``
- Doc attachment: `[Document "name"]:\n\`\`\`\ntext\`\`\``
- If no user text: "Please analyze the following: ..."
- Clears attachedContexts after snap, resets textarea, adds user message with attach chip names

`sendDispatch()` — calls `handleSendWithAttach` if defined, else `handleSend`.

### Action Buttons
`addActionButtons(group, userText, hasApis, searchedSnap, usedSnap, errorSnap)` — append to `.msg-actions`: copy btn, retry btn, optional APIs badge btn. Wire copy → `safeCopy(aiText.innerText)`. Retry → remove aiGroup + preceding status-done, pop last assistant from history, re-process last user message. APIs badge → `showApiDetail(...)`. Add disclaimer "HaiLite can make mistakes." to `.ai-content`.

`reattachAiActionButtons()` — re-wire all `.msg-group.ai` action buttons (clone + re-attach) after chat load.

### API Detail Sheet
`showApiDetail(searchedApis, usedApis, errorApis)` — render sections (Used APIs, Errors with danger border-left, Searched APIs) as `.api-item-card` divs. Open sheet.

### Chat Persistence
`genChatId()` → `'chat_' + Date.now() + '_' + randomStr`  
`getChatList()` / `saveChatList(list)` — JSON in localStorage `html_ai_chats`

`saveCurrentChat()` — serialize all `.msg-group` as `{ role, html, isStatus }` snapshots. Update or insert chat entry in list with title from first user history message (max 50 chars).

`loadChat(id)` — restore chatEl HTML from snapshot, rebuild history array by reading `.user-bubble` and `.ai-text` text content. Re-attach handlers.

`startNewChat()` — generate new chatId, clear chatEl and history (with fresh system prompt), show welcome screen.

`renderSidebarHistory(filter='')` — render filtered chat list as `.history-item` divs with active state.

### Utility Functions
`escapeHtml(str)` — replace &, <, >, ", ' with HTML entities.
`showToast(msg)` — display toast with 2200ms auto-hide.
`safeCopy(text)` / `fallbackCopy(text)` — clipboard write with execCommand fallback.
`applyMsgSize(sz)` — set `--msg-size` CSS variable.
`setGeneratingState(v)` — toggle stop-mode on send btn, swap icon.
`resetTextarea()` — clear value, reset height.
`scrollToBottom()` — set chatEl.scrollTop = scrollHeight.
`getModelLabel(id)` — find label in MODELS or use last path segment.

### Scroll Down Button
Listen to `chatEl.scroll` → toggle `.show` class on `#scroll-down-btn` when scroll distance > 150px. Click → smooth scrollTo bottom.

### Write Area Height Observer
`updateWriteH()` — measure `#write-area` height and set `--write-area-h`. Attach `ResizeObserver`.

### Keyboard & Input
- Send btn click → `sendDispatch()`
- Textarea keydown: Enter (no shift) → prevent default, sendDispatch if not generating
- Textarea input → auto-resize height

### STT (Speech-to-Text)
Check for `webkitSpeechRecognition` or `SpeechRecognition`. If unavailable, hide `#voice-btn`.

If available:
- `createRec()` — new SR, continuous=true, interimResults=true, maxAlternatives=1, set lang from STT_LANG bcp code.
  - onstart: set isRecording, change icon to 'mic' with danger color, append `.stt-interim` div to `.write-bar-inner`, showToast "Listening… tap mic to stop"
  - onresult: accumulate final + interim text; append final to textarea, update interim display
  - onerror: map error codes to user messages, stopRecording
  - onend: if still recording, restart
- `startRecording()` / `stopRecording()` — manage rec lifecycle
- `voiceBtn.click` — toggle recording

### File Upload / OCR
- `addBtn.click` → `openSheet(uploadSheet)`
- `#upload-image-ocr` click → close sheet, trigger `fileInputOcr.click()`
- `fileInputOcr.change` → `fileToBase64(file)` → `runOCR(base64)` → push to `attachedContexts`, `showAttachedFiles()`
- `#upload-text-doc` click → close sheet, trigger `fileInputDoc.click()`
- `fileInputDoc.change` → `readTextFile(file)` → push to `attachedContexts`, `showAttachedFiles()`

`fileToBase64(file)` — FileReader readAsDataURL, return base64 part.  
`readTextFile(file)` — FileReader readAsText.

`runOCR(base64)` — lazy-load Tesseract.js from CDN (`https://cdn.jsdelivr.net/npm/tesseract.js@5/dist/tesseract.min.js`). Call `Tesseract.recognize('data:image/png;base64,'+base64, 'eng+ben+hin', {logger:()=>{}})`. Return trimmed text or throw if empty.

`showAttachedFiles()` — render `.attach-chip` for each `attachedContexts` entry with remove button.  
`showLoadingChip(name)` / `removeLoadingChip()` — transient loading indicator.

---

## 5. KEY BEHAVIORS SUMMARY

| Behavior | Detail |
|---|---|
| Multi-round tool use | Up to 50 rounds; parses SEARCH/FETCH/ANSWER from AI response |
| API search | Searches fetched apiData catalogue by keywords, formats as result table |
| API fetch | Uses CORS proxy to fetch live endpoints, feeds result back to AI |
| Streaming | SSE stream from Groq; typewriter animation (3 chars per 6ms) |
| Rich media | Custom XML tags → beautiful cards (images, audio, video, file, link) |
| Code blocks | Syntax-labeled blocks with copy + show/hide toggle |
| Chat history | Persisted to localStorage; restore full HTML + history |
| Model switching | 6 Groq/OpenRouter models switchable mid-session |
| Filter modes | Pre-prompt injection (download/image/sound/apis/default) |
| File upload | Image OCR (Tesseract.js) + text document reader |
| STT | Web Speech API, multi-language, continuous, auto-restart |
| PWA | Manifest + service worker (w/ Blob fallback) |
| Retry/Edit | Branching edit from any user bubble; removes subsequent messages |
| API tracking | Tracks searched/used/errored APIs per response, viewable in sheet |
| Responsive | Mobile-first; max-width 860px on wide screens |

---

**End of prompt. Build this exactly as specified.**
