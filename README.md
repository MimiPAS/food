
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>เกมวิเคราะห์นิสัยจากพฤติกรรมการกิน</title>
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Kanit', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            position: relative;
            overflow-x: hidden;
        }

        body::before {
            content: '';
            position: fixed;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 1px, transparent 1px);
            background-size: 50px 50px;
            animation: moveBackground 20s linear infinite;
            z-index: 0;
        }

        @keyframes moveBackground {
            0% { transform: translate(0, 0); }
            100% { transform: translate(50px, 50px); }
        }

        .container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            box-shadow: 0 30px 80px rgba(0,0,0,0.3);
            max-width: 800px;
            width: 100%;
            padding: 50px 40px;
            animation: fadeIn 0.6s ease;
            position: relative;
            z-index: 1;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .header {
            text-align: center;
            margin-bottom: 40px;
            position: relative;
        }

        .header-icon {
            font-size: 5em;
            margin-bottom: 15px;
            animation: bounce 2s infinite;
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        h1 {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            font-size: 2.5em;
            margin-bottom: 10px;
            font-weight: 700;
        }

        .subtitle {
            color: #666;
            font-size: 1.2em;
            font-weight: 300;
        }

        .progress-container {
            margin: 30px 0;
        }

        .progress-bar {
            background: linear-gradient(90deg, #e0e0e0 0%, #f5f5f5 100%);
            height: 12px;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: inset 0 2px 5px rgba(0,0,0,0.1);
            position: relative;
        }

        .progress-fill {
            background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
            height: 100%;
            transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            border-radius: 20px;
            box-shadow: 0 2px 10px rgba(102, 126, 234, 0.5);
            position: relative;
            overflow: hidden;
        }

        .progress-fill::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
            animation: shimmer 2s infinite;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .question-counter {
            text-align: center;
            color: #667eea;
            font-weight: 600;
            margin: 20px 0;
            font-size: 1.2em;
        }

        .question-section {
            margin-bottom: 30px;
        }

        .question {
            font-size: 1.5em;
            color: #333;
            margin-bottom: 15px;
            font-weight: 600;
            line-height: 1.5;
            text-align: center;
        }

        .question-icon {
            font-size: 2.5em;
            display: block;
            text-align: center;
            margin-bottom: 20px;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }

        .scenario-box {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
            padding: 25px;
            border-radius: 20px;
            margin-bottom: 30px;
            font-size: 1.1em;
            line-height: 1.7;
            box-shadow: 0 10px 30px rgba(240, 147, 251, 0.3);
            position: relative;
            overflow: hidden;
        }

        .scenario-box::before {
            content: '💭';
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 3em;
            opacity: 0.2;
        }

        .options-container {
            display: grid;
            gap: 15px;
        }

        .option-card {
            background: linear-gradient(135deg, #ffffff 0%, #f8f9fa 100%);
            border: 3px solid #e0e0e0;
            border-radius: 20px;
            padding: 25px;
            cursor: pointer;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .option-card::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(102, 126, 234, 0.1);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }

        .option-card:hover {
            transform: translateY(-8px) scale(1.02);
            box-shadow: 0 15px 40px rgba(102, 126, 234, 0.3);
            border-color: #667eea;
        }

        .option-card:hover::before {
            width: 300%;
            height: 300%;
        }

        .option-card.selected {
            border-color: #667eea;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            transform: translateY(-5px) scale(1.03);
            box-shadow: 0 20px 50px rgba(102, 126, 234, 0.4);
        }

        .option-card.selected .option-emoji {
            transform: scale(1.3) rotate(10deg);
        }

        .option-emoji {
            font-size: 3em;
            display: block;
            text-align: center;
            margin-bottom: 15px;
            transition: transform 0.3s ease;
        }

        .option-label {
            font-weight: 600;
            font-size: 1.2em;
            margin-bottom: 10px;
            text-align: center;
        }

        .option-desc {
            font-size: 1em;
            opacity: 0.9;
            line-height: 1.5;
            text-align: center;
        }

        .slider-container {
            margin: 40px 0;
            padding: 30px;
            background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
            border-radius: 20px;
        }

        .slider-label {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            font-weight: 600;
            color: #d35400;
        }

        .slider-wrapper {
            position: relative;
            padding: 20px 0;
        }

        .slider {
            width: 100%;
            height: 10px;
            border-radius: 10px;
            background: linear-gradient(90deg, #ff6b6b 0%, #ffd93d 50%, #6bcf7f 100%);
            outline: none;
            -webkit-appearance: none;
            cursor: pointer;
        }

        .slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: white;
            border: 5px solid #667eea;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            transition: transform 0.2s;
        }

        .slider::-webkit-slider-thumb:hover {
            transform: scale(1.2);
        }

        .slider::-moz-range-thumb {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: white;
            border: 5px solid #667eea;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        .slider-value {
            text-align: center;
            margin-top: 15px;
            font-size: 1.5em;
            font-weight: 700;
            color: #667eea;
        }

        .checkbox-group {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 15px;
            margin: 25px 0;
        }

        .checkbox-item {
            padding: 20px;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 3px solid transparent;
            text-align: center;
        }

        .checkbox-item:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.2);
        }

        .checkbox-item.checked {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border-color: #667eea;
            transform: scale(1.05);
        }

        .checkbox-icon {
            font-size: 2.5em;
            display: block;
            margin-bottom: 10px;
        }

        .checkbox-label {
            font-weight: 600;
            font-size: 1em;
        }

        .time-picker {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 15px;
            margin: 25px 0;
        }

        .time-slot {
            padding: 20px 15px;
            background: linear-gradient(135deg, #e0c3fc 0%, #8ec5fc 100%);
            border: 3px solid transparent;
            border-radius: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .time-slot:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(142, 197, 252, 0.4);
        }

        .time-slot.selected {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border-color: #667eea;
            transform: scale(1.05);
        }

        .time-icon {
            font-size: 2em;
            display: block;
            margin-bottom: 8px;
        }

        .time-label {
            font-weight: 600;
            font-size: 0.95em;
        }

        .rating-container {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin: 40px 0;
            padding: 30px;
            background: linear-gradient(135deg, #ffeaa7 0%, #fdcb6e 100%);
            border-radius: 20px;
        }

        .rating-star {
            font-size: 3.5em;
            cursor: pointer;
            transition: all 0.3s ease;
            filter: grayscale(100%);
            opacity: 0.4;
        }

        .rating-star.active {
            filter: grayscale(0%);
            opacity: 1;
            transform: scale(1.2);
            animation: starPop 0.3s ease;
        }

        @keyframes starPop {
            0% { transform: scale(1); }
            50% { transform: scale(1.3); }
            100% { transform: scale(1.2); }
        }

        .rating-star:hover {
            transform: scale(1.3) rotate(10deg);
            filter: grayscale(0%);
        }

        .btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 18px 50px;
            border-radius: 50px;
            font-size: 1.3em;
            cursor: pointer;
            width: 100%;
            font-weight: 600;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            margin-top: 30px;
            box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
            font-family: 'Kanit', sans-serif;
            position: relative;
            overflow: hidden;
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.3);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }

        .btn:hover:not(:disabled)::before {
            width: 300%;
            height: 300%;
        }

        .btn:hover:not(:disabled) {
            transform: translateY(-3px);
            box-shadow: 0 15px 40px rgba(102, 126, 234, 0.5);
        }

        .btn:active:not(:disabled) {
            transform: translateY(0);
        }

        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .result-section {
            animation: resultFadeIn 0.8s ease;
        }

        @keyframes resultFadeIn {
            from { opacity: 0; transform: scale(0.9); }
            to { opacity: 1; transform: scale(1); }
        }

        .personality-header {
            text-align: center;
            padding: 40px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 25px;
            color: white;
            margin-bottom: 30px;
            box-shadow: 0 20px 50px rgba(102, 126, 234, 0.4);
            position: relative;
            overflow: hidden;
        }

        .personality-header::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 1px, transparent 1px);
            background-size: 30px 30px;
            animation: movePattern 10s linear infinite;
        }

        @keyframes movePattern {
            0% { transform: translate(0, 0); }
            100% { transform: translate(30px, 30px); }
        }

        .personality-icon {
            font-size: 6em;
            margin-bottom: 20px;
            animation: iconFloat 3s ease-in-out infinite;
            position: relative;
            z-index: 1;
        }

        @keyframes iconFloat {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        .personality-title {
            font-size: 2.5em;
            margin-bottom: 10px;
            font-weight: 700;
            position: relative;
            z-index: 1;
        }

        .personality-subtitle {
            font-size: 1.3em;
            opacity: 0.95;
            position: relative;
            z-index: 1;
        }

        .traits-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }

        .trait-card {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            padding: 25px;
            border-radius: 20px;
            text-align: center;
            color: white;
            box-shadow: 0 10px 30px rgba(245, 87, 108, 0.3);
            transition: transform 0.3s ease;
        }

        .trait-card:hover {
            transform: translateY(-10px) rotate(2deg);
        }

        .trait-icon {
            font-size: 3em;
            margin-bottom: 15px;
            display: block;
        }

        .trait-label {
            font-weight: 600;
            margin-bottom: 8px;
            font-size: 1.1em;
        }

        .trait-value {
            font-weight: 700;
            font-size: 1.3em;
        }

        .description-card {
            background: white;
            border: 3px solid #e0e0e0;
            border-radius: 25px;
            padding: 35px;
            margin: 25px 0;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        .description-title {
            font-size: 1.6em;
            color: #667eea;
            margin-bottom: 20px;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .description-text {
            line-height: 1.9;
            color: #555;
            font-size: 1.1em;
        }

        .nutrition-section {
            background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
            border-radius: 25px;
            padding: 40px;
            margin: 30px 0;
            box-shadow: 0 15px 40px rgba(252, 182, 159, 0.4);
        }

        .nutrition-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .nutrition-title {
            font-size: 2em;
            color: #d35400;
            font-weight: 700;
            margin-bottom: 10px;
        }

        .nutrition-cards {
            display: grid;
            gap: 20px;
        }

        .nutrition-card {
            background: white;
            border-radius: 20px;
            padding: 25px;
            border-left: 6px solid #ff9800;
            box-shadow: 0 5px 20px rgba(0,0,0,0.1);
            transition: transform 0.3s ease;
        }

        .nutrition-card:hover {
            transform: translateX(10px);
        }

        .nutrition-card-title {
            font-size: 1.3em;
            color: #d35400;
            font-weight: 700;
            margin-bottom: 12px;
        }

        .nutrition-card-content {
            color: #555;
            line-height: 1.8;
            font-size: 1.05em;
        }

        .hidden {
            display: none;
        }

        @media (max-width: 768px) {
            .container {
                padding: 30px 20px;
            }
            
            h1 {
                font-size: 2em;
            }
            
            .question {
                font-size: 1.3em;
            }
            
            .checkbox-group,
            .time-picker {
                grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="header-icon">🍽️</div>
            <h1>วิเคราะห์นิสัยจากพฤติกรรมการกิน</h1>
            <p class="subtitle">ค้นพบตัวตนและรับคำแนะนำโภชนาการส่วนตัว</p>
        </div>

        <div class="progress-container">
            <div class="progress-bar">
                <div class="progress-fill" id="progressBar" style="width: 0%"></div>
            </div>
            <div class="question-counter" id="questionCounter">คำถามที่ 1 / 10</div>
        </div>

        <div id="quizSection">
            <div class="question-section" id="questionSection"></div>
            <button class="btn" id="nextBtn" onclick="nextQuestion()">คำถามถัดไป →</button>
        </div>

        <div id="resultSection" class="hidden result-section"></div>
    </div>

    <script>
        let currentQuestion = 0;
        let answers = {};
        let selectedValues = new Set();

        const questions = [
            {
                type: 'scenario',
                icon: '😰',
                question: 'เมื่อคุณรู้สึกเครียดหรือไม่มีความสุข คุณมักจะทำอย่างไร?',
                scenario: 'วันนี้งานของคุณล้นมือ เพื่อนร่วมงานทำผิดพลาด และหัวหน้าวิจารณ์โปรเจกต์ที่คุณทุ่มเทมา คุณรู้สึกหงุดหงิดและเหนื่อยล้า...',
                options: [
                    { emoji: '🍰', label: 'กินอาหารที่ชอบเพื่อปลอบใจตัวเอง', value: 'emotional', desc: 'หาความสุขจากอาหาร ช่วยให้รู้สึกดีขึ้นชั่วคราว' },
                    { emoji: '😔', label: 'ไม่มีอารมณ์อยากกินอะไร ข้ามมื้อไป', value: 'stress', desc: 'เครียดจนไม่อยากกิน ความอยากอาหารหายไป' },
                    { emoji: '⚖️', label: 'กินเหมือนเดิม ไม่ให้อารมณ์มาแทรกแซง', value: 'disciplined', desc: 'รักษาระเบียบการกินแม้จะเครียด มีวินัยสูง' },
                    { emoji: '🏃', label: 'ออกกำลังกายหรือทำกิจกรรมอื่นแทน', value: 'healthy', desc: 'จัดการความเครียดด้วยวิธีอื่นที่ดีต่อสุขภาพ' }
                ]
            },
            {
                type: 'slider',
                icon: '💪',
                question: 'คุณใส่ใจเรื่องโภชนาการและสุขภาพมากน้อยแค่ไหน?',
                min: 0,
                max: 100,
                labels: ['ไม่สนใจเลย', 'สนใจมาก'],
                dimension: 'health_consciousness'
            },
            {
                type: 'checkbox',
                icon: '🤔',
                question: 'ปัจจัยอะไรที่มีผลต่อการเลือกกินของคุณมากที่สุด?',
                subtitle: '(เลือกได้หลายข้อ)',
                options: [
                    { icon: '😋', label: 'รสชาติอร่อย', value: 'taste' },
                    { icon: '💰', label: 'ราคา', value: 'price' },
                    { icon: '⚡', label: 'ความสะดวกรวดเร็ว', value: 'convenience' },
                    { icon: '💪', label: 'คุณค่าโภชนาการ', value: 'nutrition' },
                    { icon: '🌍', label: 'ผลกระทบสิ่งแวดล้อม', value: 'environment' },
                    { icon: '❤️', label: 'ความทรงจำ/อารมณ์', value: 'emotion' },
                    { icon: '📱', label: 'เทรนด์โซเชียล', value: 'social' },
                    { icon: '🏋️', label: 'เป้าหมายสุขภาพ', value: 'fitness' }
                ],
                dimension: 'food_motivations'
            },
            {
                type: 'time',
                icon: '⏰',
                question: 'คุณมักจะหิวหรืออยากกินอาหารมากที่สุดช่วงไหน?',
                slots: [
                    { icon: '🌅', label: 'เช้า', value: 'morning' },
                    { icon: '☀️', label: 'สาย', value: 'late_morning' },
                    { icon: '🌤️', label: 'บ่าย', value: 'afternoon' },
                    { icon: '🌆', label: 'เย็น', value: 'evening' },
                    { icon: '🌃', label: 'ค่ำ', value: 'night' },
                    { icon: '🌙', label: 'ดึก', value: 'late_night' }
                ],
                dimension: 'eating_time'
            },
            {
                type: 'scenario',
                icon: '🍽️',
                question: 'ในงานเลี้ยงบุฟเฟต์ที่มีอาหารหลากหลาย คุณจะทำอย่างไร?',
                scenario: 'คุณเข้าร่วมงานเลี้ยงบุฟเฟต์ที่มีอาหารให้เลือกกว่า 50 เมนู ตั้งแต่สลัดผัก ซีฟู้ด เนื้อย่าง พาสต้า ซูชิ ไปจนถึงของหวานนานาชนิด',
                options: [
                    { emoji: '🎨', label: 'ตักทุกอย่างที่น่าสนใจ ลองให้ครบ', value: 'explorer', desc: 'อยากลองทุกอย่าง ไม่พลาดเมนูใหม่' },
                    { emoji: '❤️', label: 'เลือกแต่เมนูที่ชอบและคุ้นเคย', value: 'conservative', desc: 'เล่นในโซนความปลอดภัย เลือกที่ชอบแน่ๆ' },
                    { emoji: '🎯', label: 'สำรวจก่อน แล้วเลือกที่ดูดีที่สุด', value: 'strategic', desc: 'วางแผนก่อนตัก ประเมินคุณภาพ' },
                    { emoji: '🧘', label: 'กินแค่พอดี ไม่ให้มากเกินไป', value: 'mindful', desc: 'ควบคุมปริมาณ ไม่กินมากเพราะฟรี' }
                ]
            },
            {
                type: 'rating',
                icon: '🏷️',
                question: 'คุณให้ความสำคัญกับการอ่านฉลากโภชนาการมากน้อยแค่ไหน?',
                dimension: 'label_reading'
            },
            {
                type: 'checkbox',
                icon:
