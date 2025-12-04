<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <title>เกมวิเคราะห์นิสัยจากพฤติกรรมการกิน</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    * {
      box-sizing: border-box;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: #fff7ec;
    }

    .card {
      background: #ffffff;
      max-width: 480px;
      width: 100%;
      padding: 24px 20px;
      border-radius: 24px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.08);
    }

    h1 {
      font-size: 1.4rem;
      margin: 0 0 4px;
      text-align: center;
    }

    .subtitle {
      text-align: center;
      color: #666;
      font-size: 0.9rem;
      margin-bottom: 18px;
    }

    .progress {
      font-size: 0.9rem;
      color: #666;
      margin-bottom: 12px;
      text-align: right;
    }

    .question {
      font-size: 1.05rem;
      font-weight: 600;
      margin-bottom: 16px;
    }

    .choices {
      display: grid;
      gap: 10px;
      margin-bottom: 18px;
    }

    .choice-btn {
      padding: 10px 14px;
      border-radius: 999px;
      border: 1px solid #ffd4a8;
      background: #fffaf4;
      cursor: pointer;
      text-align: left;
      font-size: 0.95rem;
      transition: transform 0.1s ease, box-shadow 0.1s ease, background 0.1s ease, border-color 0.1s ease;
    }

    .choice-btn:hover {
      transform: translateY(-1px);
      box-shadow: 0 4px 10px rgba(0,0,0,0.06);
      background: #ffe9c9;
      border-color: #ffb766;
    }

    .choice-btn:active {
      transform: translateY(0);
      box-shadow: none;
    }

    .next-row {
      display: flex;
      justify-content: flex-end;
      align-items: center;
      gap: 12px;
    }

    .next-btn {
      padding: 8px 14px;
      border-radius: 999px;
      border: none;
      background: #ffb766;
      color: #fff;
      font-weight: 600;
      cursor: pointer;
      font-size: 0.9rem;
      transition: background 0.1s ease, transform 0.1s ease, box-shadow 0.1s ease;
    }

    .next-btn:disabled {
      opacity: 0.5;
      cursor: default;
      box-shadow: none;
      transform: none;
    }

    .next-btn:not(:disabled):hover {
      background: #ff9e38;
      transform: translateY(-1px);
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }

    .result-title {
      font-size: 1.2rem;
      font-weight: 700;
      margin-bottom: 6px;
      text-align: center;
    }

    .result-type {
      font-size: 1.05rem;
      font-weight: 600;
      color: #ff8a3a;
      text-align: center;
      margin-bottom: 8px;
    }

    .result-text {
      font-size: 0.95rem;
      color: #444;
      line-height: 1.5;
      margin-bottom: 16px;
    }

    .restart-btn {
      display: block;
      margin: 0 auto;
      padding: 8px 16px;
      border-radius: 999px;
      border: 1px solid #ffb766;
      background: #fffaf4;
      cursor: pointer;
      font-size: 0.9rem;
    }

    @media (max-width: 600px) {
      .card {
        margin: 16px;
      }
    }
  </style>
</head>
<body>
  <div class="card">
    <h1>วิเคราะห์นิสัยจากพฤติกรรมการกิน</h1>
    <div class="subtitle">ค้นพบตัวตนและรับคำแนะนำโภชนาการส่วนตัว</div>

    <div id="quiz-container">
      <div class="progress" id="progress">คำถามที่ 1 / 3</div>
      <div class="question" id="question">ถ้าคุณหิวมาก คุณมักจะเลือกกินอะไรเป็นอย่างแรก?</div>
      <div class="choices" id="choices"></div>

      <div class="next-row">
        <span id="hint" style="font-size: 0.8rem; color:#999;">เลือกคำตอบก่อนจึงจะไปต่อได้</span>
        <button class="next-btn" id="next-btn" disabled>คำถามถัดไป →</button>
      </div>
    </div>
  </div>

  <script>
    // -----------------------------
    // 1) กำหนดคำถามตัวอย่าง
    // -----------------------------
    const questions = [
      {
        text: "ถ้าคุณหิวมาก คุณมักจะเลือกกินอะไรเป็นอย่างแรก?",
        choices: [
          { text: "อะไรก็ได้ ขอเร็วไว้ก่อน", type: "สายสบาย ๆ", score: 1 },
          { text: "อาหารจานหลักที่อิ่มและครบ 3 หมู่", type: "สายวางแผน", score: 2 },
          { text: "ของกินเล่น ขนม เครื่องดื่มหวาน ๆ ก่อน", type: "สายใจร้อน", score: 0 },
        ],
      },
      {
        text: "เวลาคุณกินข้าว คุณมักจะ…",
        choices: [
          { text: "ดูมือถือ/ซีรีส์ไปด้วยทุกมื้อ", type: "สายดิจิทัล", score: 0 },
          { text: "บางมื้อเท่านั้น ส่วนใหญ่โฟกัสกับอาหาร", type: "สายบาลานซ์", score: 2 },
          { text: "ไม่ค่อยเล่นมือถือขณะกิน", type: "สายโฟกัสสุขภาพ", score: 3 },
        ],
      },
      {
        text: "คุณอ่านฉลากโภชนาการ ( Nutrition Facts ) บ่อยแค่ไหน?",
        choices: [
          { text: "แทบไม่เคยอ่านเลย", type: "เริ่มต้นเรื่องโภชนาการ", score: 0 },
          { text: "อ่านเฉพาะบางครั้ง เช่น ซื้อของใหม่ ๆ", type: "กำลังใส่ใจสุขภาพ", score: 2 },
          { text: "อ่านแทบทุกครั้งก่อนซื้อ", type: "สายเฮลท์ตัวยง", score: 3 },
        ],
      },
    ];

    // -----------------------------
    // 2) ตั้งค่า state เริ่มต้น
    // -----------------------------
    let currentIndex = 0;
    let totalScore = 0;
    let hasSelected = false;

    const progressEl = document.getElementById("progress");
    const questionEl = document.getElementById("question");
    const choicesEl = document.getElementById("choices");
    const nextBtn = document.getElementById("next-btn");
    const hintEl = document.getElementById("hint");
    const quizContainer = document.getElementById("quiz-container");

    // -----------------------------
    // 3) ฟังก์ชันแสดงคำถาม
    // -----------------------------
    function renderQuestion() {
      const q = questions[currentIndex];
      progressEl.textContent = `คำถามที่ ${currentIndex + 1} / ${questions.length}`;
      questionEl.textContent = q.text;

      // ล้างตัวเลือกเดิม
      choicesEl.innerHTML = "";
      hasSelected = false;
      nextBtn.disabled = true;
      hintEl.textContent = "เลือกคำตอบก่อนจึงจะไปต่อได้";

      // สร้างปุ่มตัวเลือก
      q.choices.forEach((choice, index) => {
        const btn = document.createElement("button");
        btn.className = "choice-btn";
        btn.textContent = choice.text;

        btn.addEventListener("click", () => {
          if (hasSelected) return; // กันการคลิกหลายครั้ง

          hasSelected = true;
          totalScore += choice.score;

          // ไฮไลต์ปุ่มที่เลือก
          document.querySelectorAll(".choice-btn").forEach((b) => {
            b.style.opacity = "0.6";
          });
          btn.style.opacity = "1";
          btn.style.borderColor = "#ff8a3a";
          btn.style.background = "#ffe1c4";

          nextBtn.disabled = false;
          hintEl.textContent = "พร้อมแล้วกดไปคำถามถัดไปได้เลย";
        });

        choicesEl.appendChild(btn);
      });
    }

    // -----------------------------
    // 4) แสดงผลลัพธ์ท้ายเกม
    // -----------------------------
    function showResult() {
      let type, advice;

      if (totalScore <= 2) {
        type = "สายเริ่มต้นดูแลตัวเอง";
        advice =
          "คุณอาจจะยังไม่ค่อยได้สนใจเรื่องโภชนาการมากนัก ลองเริ่มจากเป้าหมายเล็ก ๆ เช่น ลดน้ำหวานวันละแก้ว หรือเติมผักในจานข้าวให้มากขึ้นทีละนิด 😊";
      } else if (totalScore <= 6) {
        type = "สายบาลานซ์กำลังพัฒนา";
        advice =
          "คุณใส่ใจสุขภาพพอสมควร แต่ยังมีพื้นที่ให้พัฒนาอีก ลองอ่านฉลากโภชนาการให้บ่อยขึ้น และวางแผนมื้ออาหารล่วงหน้า จะช่วยให้ดูแลตัวเองได้ดีกว่าเดิมมาก!";
      } else {
        type = "สายโภชนาการโปร";
        advice =
          "คุณมีพื้นฐานความรู้และพฤติกรรมด้านอาหารที่ดีอยู่แล้ว ลองแชร์ความรู้หรือชวนคนรอบตัวมากินให้ดีกับคุณ จะได้สุขภาพดีไปด้วยกัน 💪🥦";
      }

      quizContainer.innerHTML = `
        <div class="result-title">สรุปผลวิเคราะห์นิสัยการกินของคุณ</div>
        <div class="result-type">${type}</div>
        <div class="result-text">${advice}</div>
        <button class="restart-btn" id="restart-btn">เล่นอีกครั้ง</button>
      `;

      document.getElementById("restart-btn").addEventListener("click", () => {
        currentIndex = 0;
        totalScore = 0;
        quizContainer.innerHTML = "";
        quizContainer.appendChild(progressEl);
        quizContainer.appendChild(questionEl);
        quizContainer.appendChild(choicesEl);

        const nextRow = document.createElement("div");
        nextRow.className = "next-row";
        nextRow.appendChild(hintEl);
        nextRow.appendChild(nextBtn);
        quizContainer.appendChild(nextRow);

        renderQuestion();
      });
    }

    // -----------------------------
    // 5) ปุ่ม "คำถามถัดไป"
    // -----------------------------
    nextBtn.addEventListener("click", () => {
      if (!hasSelected) return;
      if (currentIndex < questions.length - 1) {
        currentIndex++;
        renderQuestion();
      } else {
        showResult();
      }
    });

    // -----------------------------
    // 6) เรียกครั้งแรก
    // -----------------------------
    renderQuestion();
  </script>
</body>
</html>
