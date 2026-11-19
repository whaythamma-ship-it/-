```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>لعنة المقبرة | ألغاز الفراعنة والمومياوات</title>
    <!-- استيراد خط عربي مناسب للأجواء التاريخية والملحمية -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@600;700;900&family=Reem+Kufi:wght@600;700&display=swap" rel="stylesheet">

    <style>
        /* التصميم العام ولوحة الألوان الفرعونية (الذهب الداكن واللازورد) */
        :root {
            --bg-deep: #0b0907;
            --stone-dark: #1a140e;
            --stone-panel: #261d14;
            --gold-bright: #f5b942;
            --gold-dim: #b88228;
            --lapis-blue: #1e4b7a;
            --lapis-glow: #38bdf8;
            --text-sand: #f4ebd9;
            --danger-red: #dc2626;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }

        body {
            font-family: 'Cairo', sans-serif;
            background: radial-gradient(circle at center, #24190e 0%, #090705 100%);
            color: var(--text-sand);
            min-height: 100dvh;
            width: 100vw;
            overflow-x: hidden;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 8px;
        }

        /* الإطار الهيروغليفي الخارجي */
        #app-container {
            position: relative;
            width: 100%;
            max-width: 520px;
            background: linear-gradient(180deg, var(--stone-dark) 0%, #120d09 100%);
            border: 3px solid var(--gold-dim);
            border-radius: 20px;
            box-shadow: 0 0 40px rgba(245, 185, 66, 0.18), inset 0 0 25px rgba(0, 0, 0, 0.8);
            padding: 12px 14px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        /* شريط النقوش الفرعونية العلوي */
        .hieroglyph-bar {
            text-align: center;
            color: var(--gold-dim);
            font-size: 0.78rem;
            letter-spacing: 5px;
            opacity: 0.65;
            overflow: hidden;
            white-space: nowrap;
        }

        /* رأس اللعبة والأزرار العلوية */
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 8px;
        }

        .game-brand {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .game-brand h1 {
            font-family: 'Reem Kufi', 'Cairo', sans-serif;
            font-size: 1.55rem;
            color: var(--gold-bright);
            text-shadow: 0 2px 10px rgba(245, 185, 66, 0.4);
            line-height: 1.1;
        }

        .header-actions {
            display: flex;
            gap: 6px;
        }

        .icon-btn {
            background: var(--stone-panel);
            border: 1.5px solid var(--gold-dim);
            color: var(--gold-bright);
            border-radius: 10px;
            padding: 6px 10px;
            font-family: 'Cairo', sans-serif;
            font-weight: 700;
            font-size: 0.85rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 4px;
            transition: all 0.15s ease;
        }

        .icon-btn:hover {
            background: #36291c;
            border-color: var(--gold-bright);
        }

        .icon-btn:active {
            transform: scale(0.94);
        }

        /* شريط حالة المرحلة */
        #status-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(15, 11, 8, 0.75);
            border: 1px solid rgba(184, 130, 40, 0.4);
            border-radius: 12px;
            padding: 8px 12px;
            font-size: 0.9rem;
        }

        .status-item {
            display: flex;
            align-items: center;
            gap: 6px;
            font-weight: 700;
        }

        .badge-gold {
            color: var(--gold-bright);
            font-weight: 900;
        }

        .key-status {
            padding: 2px 10px;
            border-radius: 20px;
            font-size: 0.82rem;
            background: rgba(30, 75, 122, 0.35);
            border: 1px solid rgba(56, 189, 248, 0.4);
            color: #7dd3fc;
        }

        .key-status.collected {
            background: rgba(245, 185, 66, 0.25);
            border-color: var(--gold-bright);
            color: var(--gold-bright);
        }

        /* حاوية رقعة اللعب (Canvas) */
        #canvas-wrapper {
            position: relative;
            width: 100%;
            aspect-ratio: 1 / 1;
            max-height: 440px;
            margin: 0 auto;
            border-radius: 14px;
            border: 2.5px solid var(--gold-dim);
            overflow: hidden;
            background: #140f0a;
            box-shadow: inset 0 0 30px rgba(0, 0, 0, 0.9);
        }

        canvas {
            display: block;
            width: 100%;
            height: 100%;
        }

        /* شريط الأدوات السريع (تراجع / إعادة) */
        #action-bar {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 8px;
        }

        .tool-btn {
            background: linear-gradient(180deg, #2d2217 0%, #1c150e 100%);
            border: 1.5px solid var(--gold-dim);
            color: var(--text-sand);
            padding: 8px 6px;
            border-radius: 12px;
            font-family: 'Cairo', sans-serif;
            font-size: 0.88rem;
            font-weight: 700;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
            transition: transform 0.1s, border-color 0.15s;
        }

        .tool-btn:hover:not(:disabled) {
            border-color: var(--gold-bright);
            color: var(--gold-bright);
        }

        .tool-btn:active:not(:disabled) {
            transform: scale(0.95);
        }

        .tool-btn:disabled {
            opacity: 0.4;
            cursor: not-allowed;
        }

        /* لوحة التحكم الاتجاهية (D-Pad) + زر الانتظار */
        #controls-area {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 12px;
            padding: 4px 6px;
        }

        .dpad-grid {
            display: grid;
            grid-template-columns: repeat(3, 54px);
            grid-template-rows: repeat(2, 50px);
            gap: 6px;
            direction: ltr; /* لضمان اتجاه الأسهم يمين وشمال بدقة */
        }

        .dpad-btn {
            background: linear-gradient(180deg, #35281b 0%, #1f170f 100%);
            border: 2px solid var(--gold-dim);
            border-radius: 12px;
            color: var(--gold-bright);
            font-size: 1.25rem;
            font-weight: 900;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 0 #0d0906;
            transition: transform 0.08s, box-shadow 0.08s;
        }

        .dpad-btn:active {
            transform: translateY(3px);
            box-shadow: 0 1px 0 #0d0906;
            background: #473624;
        }

        .dpad-up { grid-column: 2; grid-row: 1; }
        .dpad-left { grid-column: 1; grid-row: 2; }
        .dpad-down { grid-column: 2; grid-row: 2; }
        .dpad-right { grid-column: 3; grid-row: 2; }

        /* زر انتظار الدور (تكتيك أساسي في اللعبة) */
        .wait-btn {
            flex: 1;
            height: 106px;
            background: linear-gradient(180deg, #1d4ed8 0%, #1e3a8a 100%);
            border: 2px solid #60a5fa;
            border-radius: 16px;
            color: #ffffff;
            font-family: 'Cairo', sans-serif;
            cursor: pointer;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 4px;
            box-shadow: 0 4px 0 #0f172a;
            transition: transform 0.08s;
        }

        .wait-btn:active {
            transform: translateY(3px);
            box-shadow: 0 1px 0 #0f172a;
        }

        .wait-btn .wait-icon {
            font-size: 1.6rem;
        }

        .wait-btn .wait-title {
            font-size: 1rem;
            font-weight: 900;
        }

        .wait-btn .wait-sub {
            font-size: 0.72rem;
            color: #bfdbfe;
        }

        /* النوافذ المنبثقة (Overlays & Modals) */
        .modal-overlay {
            position: absolute;
            inset: 0;
            background: rgba(10, 8, 6, 0.92);
            backdrop-filter: blur(6px);
            z-index: 30;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            text-align: center;
            border-radius: 12px;
            animation: fadeIn 0.25s ease;
        }

        .hidden {
            display: none !important;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.96); }
            to { opacity: 1; transform: scale(1); }
        }

        .modal-title {
            font-family: 'Reem Kufi', 'Cairo', sans-serif;
            font-size: 1.85rem;
            color: var(--gold-bright);
            margin-bottom: 8px;
        }

        .modal-desc {
            font-size: 0.95rem;
            color: #e7dcc8;
            line-height: 1.6;
            margin-bottom: 16px;
            max-width: 360px;
        }

        .rule-box {
            background: rgba(38, 29, 20, 0.9);
            border: 1.5px solid var(--gold-dim);
            border-radius: 14px;
            padding: 12px 14px;
            text-align: right;
            font-size: 0.86rem;
            line-height: 1.65;
            margin-bottom: 16px;
            width: 100%;
        }

        .rule-box li {
            margin-bottom: 6px;
            list-style: none;
        }

        .primary-btn {
            background: linear-gradient(135deg, #f5b942 0%, #d97706 100%);
            color: #140d05;
            border: none;
            padding: 12px 32px;
            border-radius: 50px;
            font-family: 'Cairo', sans-serif;
            font-size: 1.1rem;
            font-weight: 900;
            cursor: pointer;
            box-shadow: 0 6px 20px rgba(245, 185, 66, 0.35);
            transition: transform 0.15s;
        }

        .primary-btn:active {
            transform: scale(0.95);
        }

        /* شبكة اختيار المراحل */
        .levels-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            width: 100%;
            max-width: 340px;
            margin-bottom: 18px;
        }

        .lvl-card {
            background: var(--stone-panel);
            border: 2px solid var(--gold-dim);
            border-radius: 12px;
            padding: 12px 8px;
            color: var(--text-sand);
            font-family: 'Cairo', sans-serif;
            cursor: pointer;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
            transition: all 0.15s;
        }

        .lvl-card.active {
            border-color: var(--gold-bright);
            background: #3d2d1d;
            box-shadow: 0 0 12px rgba(245, 185, 66, 0.3);
        }

        .lvl-card.solved {
            border-color: #22c55e;
        }

        .lvl-num {
            font-size: 1.25rem;
            font-weight: 900;
            color: var(--gold-bright);
        }

        .lvl-name {
            font-size: 0.72rem;
            color: #cbd5e1;
        }
    </style>
</head>
<body>

<div id="app-container">
    <div class="hieroglyph-bar">𓂀 𓆣 𓋹 𓇳 𓅃 𓏏 𓊹 𓂀 𓆣 𓋹 𓇳 𓅃</div>

    <!-- رأس الصفحة -->
    <header>
        <div class="game-brand">
            <span style="font-size: 1.7rem;">🏺</span>
            <div>
                <h1>لعنة المقبرة</h1>
            </div>
        </div>
        <div class="header-actions">
            <button id="how-btn" class="icon-btn" title="طريقة اللعب">📜 كيف تلعب</button>
            <button id="levels-btn" class="icon-btn" title="المراحل">🏛️ المراحل</button>
            <button id="mute-btn" class="icon-btn" title="الصوت">🔊</button>
        </div>
    </header>

    <!-- شريط معلومات المرحلة -->
    <div id="status-bar">
        <div class="status-item">
            <span>المرحلة:</span>
            <span id="lvl-indicator" class="badge-gold">1 / 6</span>
        </div>
        <div id="key-badge" class="key-status">🚪 الباب مفتوح</div>
        <div class="status-item">
            <span>الخطوات:</span>
            <span id="moves-indicator" class="badge-gold">0</span>
        </div>
    </div>

    <!-- منطقة اللعب الرئيسية -->
    <div id="canvas-wrapper">
        <canvas id="tombCanvas"></canvas>

        <!-- نافذة شرح القواعد (كيف تلعب) -->
        <div id="how-modal" class="modal-overlay">
            <h2 class="modal-title">📜 أسرار المقبرة</h2>
            <ul class="rule-box">
                <li>🧭 <strong>الهدف:</strong> أوصل المستكشف إلى <strong>الباب الذهبي</strong> للهروب من المقبرة.</li>
                <li>☥ <strong>مفتاح عنخ:</strong> في بعض الغرف، اجمع <strong>مفتاح عنخ</strong> أو الجعران لفتح الباب المغلق.</li>
                <li>🧟 <strong>قانون المومياء (مهم جداً!):</strong> مقابل كل <strong>خطوة واحدة</strong> تتحركها (أو تنتظر دورك)، تتحرك المومياء <strong>خطوتين</strong> نحوك!</li>
                <li>🧠 <strong>ذكاء المومياء:</strong> المومياء تحاول دائماً الاقتراب منك <strong>أفقياً (يمين/شمال) أولاً</strong>، وإذا منعها جدار أو تساوى العمود تتحرك <strong>رأسياً (فوق/تحت)</strong>. استغل الجدران لحبسها!</li>
                <li>🌀 <strong>بوابات النقل:</strong> تنقلك فوراً للبوابة المقابلة داخل الغرفة.</li>
            </ul>
            <button id="close-how-btn" class="primary-btn">فهمت، ابدأ التحدي!</button>
        </div>

        <!-- نافذة اختيار المراحل -->
        <div id="levels-modal" class="modal-overlay hidden">
            <h2 class="modal-title">🏛️ غرف المعبد</h2>
            <p class="modal-desc" style="margin-bottom: 12px;">اختر الغرفة التي تريد استكشافها وحل لغزها:</p>
            <div id="levels-grid" class="levels-grid"></div>
            <button id="close-levels-btn" class="icon-btn" style="padding: 8px 24px;">إغلاق</button>
        </div>

        <!-- نافذة الفوز بالمرحلة -->
        <div id="win-modal" class="modal-overlay hidden">
            <div style="font-size: 3rem; margin-bottom: 4px;">🏆✨</div>
            <h2 class="modal-title">نجوت من اللعنة!</h2>
            <p id="win-desc" class="modal-desc">تمكنت من الهروب بذكاء في 12 خطوة!</p>
            <button id="next-lvl-btn" class="primary-btn">الغرفة التالية ⬅️</button>
        </div>

        <!-- نافذة الخسارة (الإمساك باللاعب) -->
        <div id="lose-modal" class="modal-overlay hidden">
            <div style="font-size: 3rem; margin-bottom: 4px;">🧟💀</div>
            <h2 class="modal-title" style="color: #f87171;">أمسكت بك المومياء!</h2>
            <p class="modal-desc">تذكر: المومياء تتحرك أفقياً أولاً خطوتين لكل خطوة تخطوها. جرب التراجع خطوة واستخدم الجدران كدرع!</p>
            <div style="display: flex; gap: 10px;">
                <button id="lose-undo-btn" class="icon-btn" style="padding: 12px 20px; font-size: 1rem;">↩️ تراجع خطوة</button>
                <button id="lose-retry-btn" class="primary-btn" style="padding: 12px 24px;">إعادة الغرفة</button>
            </div>
        </div>
    </div>

    <!-- أزرار المساعدة السريعة -->
    <div id="action-bar">
        <button id="undo-btn" class="tool-btn" disabled>↩️ تراجع</button>
        <button id="restart-btn" class="tool-btn">🔄 إعادة</button>
        <button id="hint-btn" class="tool-btn">💡 تلميح</button>
    </div>

    <!-- أزرار التحكم الاتجاهية للموبايل والكمبيوتر -->
    <div id="controls-area">
        <div class="dpad-grid">
            <button class="dpad-btn dpad-up" data-dir="UP" aria-label="فوق">▲</button>
            <button class="dpad-btn dpad-left" data-dir="LEFT" aria-label="شمال">◀</button>
            <button class="dpad-btn dpad-down" data-dir="DOWN" aria-label="تحت">▼</button>
            <button class="dpad-btn dpad-right" data-dir="RIGHT" aria-label="يمين">▶</button>
        </div>

        <button id="wait-btn" class="wait-btn">
            <span class="wait-icon">⏳</span>
            <span class="wait-title">انتظر دور</span>
            <span class="wait-sub">(مسافة / Space)</span>
        </button>
    </div>

    <div class="hieroglyph-bar">𓆣 𓂀 𓅃 𓇳 𓋹 𓊹 𓏏 𓆣 𓂀 𓅃 𓇳 𓋹</div>
</div>

<script>
/**
 * لعبة "لعنة المقبرة" (Curse of the Tomb)
 * لعبة ألغاز فرعونية تعتمد على الأدوار (Turn-Based Grid Puzzle)
 * القواعد الكلاسيكية:
 * - اللاعب يتحرك 1 خطوة (أو ينتظر دوراً).
 * - كل مومياء تتحرك 2 خطوة متتالية.
 * - أولوية حركة المومياء: المحور الأفقي (X) أولاً لتقليل المسافة مع اللاعب، ثم المحور الرأسي (Y).
 */

// --- 1. نظام الصوتيات الفرعونية (Web Audio API) ---
class TombAudio {
    constructor() {
        this.ctx = null;
        this.muted = false;
    }

    init() {
        if (!this.ctx) {
            const AudioCtx = window.AudioContext || window.webkitAudioContext;
            this.ctx = new AudioCtx();
        }
        if (this.ctx.state === 'suspended') {
            this.ctx.resume();
        }
    }

    toggleMute() {
        this.muted = !this.muted;
        return this.muted;
    }

    // صوت خطوة على الحجر الرملي
    playStep() {
        if (this.muted || !this.ctx) return;
        try {
            const now = this.ctx.currentTime;
            const osc = this.ctx.createOscillator();
            const gain = this.ctx.createGain();
            osc.type = 'triangle';
            osc.frequency.setValueAtTime(140, now);
            osc.frequency.exponentialRampToValueAtTime(65, now + 0.07);
            gain.gain.setValueAtTime(0.12, now);
            gain.gain.exponentialRampToValueAtTime(0.001, now + 0.07);
            osc.connect(gain);
            gain.connect(this.ctx.destination);
            osc.start(now);
            osc.stop(now + 0.07);
        } catch (e) {}
    }

    // صوت زحف المومياء
    playMummyStep() {
        if (this.muted || !this.ctx) return;
        try {
            const now = this.ctx.currentTime;
            const osc = this.ctx.createOscillator();
            const gain = this.ctx.createGain();
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(95, now);
            osc.frequency.linearRampToValueAtTime(72, now + 0.09);
            gain.gain.setValueAtTime(0.06, now);
            gain.gain.exponentialRampToValueAtTime(0.001, now + 0.09);
            osc.connect(gain);
            gain.connect(this.ctx.destination);
            osc.start(now);
            osc.stop(now + 0.09);
        } catch (e) {}
    }

    // صوت التقاط مفتاح عنخ أو الجعران
    playPickup() {
        if (this.muted || !this.ctx) return;
        try {
            const now = this.ctx.currentTime;
            [587.33, 880, 1174.66].forEach((freq, idx) => {
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(freq, now + idx * 0.06);
                gain.gain.setValueAtTime(0.14, now + idx * 0.06);
                gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.06 + 0.25);
     # -
لعبة
