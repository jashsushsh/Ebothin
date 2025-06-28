# Ebothin <!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>ebothink - اختبار الشخصية</title>
<style>
  body {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    background: #f0f8ff;
    color: #003366;
    margin: 0; padding: 0;
  }
  header {
    background: #cce7ff;
    padding: 20px;
    text-align: center;
    font-size: 1.8rem;
    font-weight: bold;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  }
  main {
    max-width: 600px;
    margin: 30px auto;
    background: white;
    padding: 20px 30px 40px 30px;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  }
  .question-number {
    font-weight: bold;
    margin-bottom: 10px;
    font-size: 1.2rem;
  }
  .question-text {
    margin-bottom: 15px;
    font-size: 1.1rem;
  }
  .answers label {
    display: block;
    background: #e6f0ff;
    margin-bottom: 10px;
    padding: 10px 15px;
    border-radius: 6px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    user-select: none;
  }
  .answers input[type="radio"] {
    display: none;
  }
  .answers input[type="radio"]:checked + label {
    background: #99c2ff;
    color: #00224d;
    font-weight: bold;
  }
  button {
    background: #4d90fe;
    border: none;
    color: white;
    padding: 12px 25px;
    font-size: 1rem;
    border-radius: 6px;
    cursor: pointer;
    margin-top: 15px;
    transition: background-color 0.3s ease;
  }
  button:disabled {
    background: #a0c1ff;
    cursor: not-allowed;
  }
  button:hover:not(:disabled) {
    background: #357ae8;
  }
  .result {
    text-align: center;
  }
  .result h2 {
    color: #003366;
    margin-bottom: 10px;
  }
  .result p {
    font-size: 1rem;
    margin: 10px 0;
  }
  footer {
    margin: 40px auto 20px;
    max-width: 600px;
    text-align: center;
    color: #666;
    font-size: 0.9rem;
  }
  @media (max-width: 640px) {
    main {
      margin: 15px;
      padding: 15px 20px 30px 20px;
    }
  }
</style>
</head>
<body>

<header>مرحبًا بك في ebothink<br>اكتشف شخصيتك الحقيقية</header>

<main>
  <div id="quiz-container">
    <div id="question-area">
      <!-- السؤال الحالي -->
    </div>
    <button id="next-btn" disabled>التالي</button>
  </div>

  <div id="result-container" style="display:none;">
    <div class="result">
      <h2>نتيجة اختبار شخصيتك</h2>
      <h3 id="result-rank"></h3>
      <p id="result-opinion"></p>
      <p id="result-advice"></p>
      <p id="result-info"></p>
      <button id="restart-btn">إعادة الاختبار</button>
    </div>
  </div>
</main>

<footer>© 2025 ebothink. كل الحقوق محفوظة.</footer>

<script>
const questions = [
  {
    q: "عندما تواجه مشكلة مع شخص مقرب، كيف تتصرف؟",
    answers: [
      { text: "أتحدث معه بصراحة لحل المشكلة", score: 3 },
      { text: "أحاول تجنب النزاع والابتعاد عنه", score: 1 },
      { text: "أحتفظ بالغضب بداخلي ولا أظهره", score: 2 },
      { text: "أواجهه بشكل مباشر وأصر على حقي", score: 4 }
    ]
  },
  {
    q: "كيف تصف نفسك في المواقف الاجتماعية؟",
    answers: [
      { text: "اجتماعي وأحب التعرف على الناس", score: 4 },
      { text: "هادئ وأفضل المراقبة", score: 2 },
      { text: "أحب النقاشات وأعبر عن رأيي", score: 3 },
      { text: "أفضل البقاء وحيدًا", score: 1 }
    ]
  },
  {
    q: "إذا طلب منك أحد المساعدة، كيف تتصرف؟",
    answers: [
      { text: "أساعد فورًا دون تردد", score: 4 },
      { text: "أفكر قبل أن أقرر", score: 3 },
      { text: "أحيانًا أساعد وأحيانًا لا", score: 2 },
      { text: "عادةً أرفض المساعدة", score: 1 }
    ]
  },
  {
    q: "عندما تشعر بالتوتر، كيف تتعامل معه؟",
    answers: [
      { text: "أمارس الرياضة أو النشاط البدني", score: 4 },
      { text: "أجلس وأفكر بحلول", score: 3 },
      { text: "أتحدث مع أحد أصدقائي", score: 2 },
      { text: "أفضل الانعزال والتجاهل", score: 1 }
    ]
  },
  {
    q: "كيف تتصرف عندما تواجه نقدًا سلبيًا؟",
    answers: [
      { text: "أستمع وأحاول تحسين نفسي", score: 4 },
      { text: "أغضب وأرد بصعوبة", score: 1 },
      { text: "أتجاهل النقد وأمضي", score: 2 },
      { text: "أشعر بالحزن لفترة", score: 3 }
    ]
  },
  {
    q: "كيف تقضي وقت فراغك عادةً؟",
    answers: [
      { text: "قراءة أو تعلم شيء جديد", score: 4 },
      { text: "مشاهدة التلفاز أو الألعاب", score: 2 },
      { text: "الخروج مع الأصدقاء", score: 3 },
      { text: "الاسترخاء والهدوء", score: 1 }
    ]
  },
  {
    q: "عندما يكون هناك خلاف في مجموعة، ما هو دورك؟",
    answers: [
      { text: "أحاول أن أكون وسيطًا لحل الخلاف", score: 4 },
      { text: "أتحاشى المشاركة", score: 1 },
      { text: "أدافع عن رأيي بقوة", score: 3 },
      { text: "أترك الأمر للآخرين", score: 2 }
    ]
  },
  {
    q: "كيف تتعامل مع الضغوط اليومية؟",
    answers: [
      { text: "أنظم وقتي جيدًا وأضع أولوياتي", score: 4 },
      { text: "أشعر بالقلق والتوتر", score: 2 },
      { text: "أتجاهلها وأتمنى أن تمر", score: 1 },
      { text: "أطلب المساعدة من الآخرين", score: 3 }
    ]
  },
  {
    q: "هل تحب تجربة أشياء جديدة؟",
    answers: [
      { text: "نعم أحب المغامرة والتجديد", score: 4 },
      { text: "أحيانًا فقط", score: 3 },
      { text: "أفضل الروتين والهدوء", score: 1 },
      { text: "لا أحب التغيير كثيرًا", score: 2 }
    ]
  },
  {
    q: "كيف تتصرف إذا خسرت في شيء مهم؟",
    answers: [
      { text: "أتعلم من الفشل وأحاول مجددًا", score: 4 },
      { text: "أشعر بالإحباط لفترة", score: 2 },
      { text: "أتجاهل الخسارة وأمضي", score: 3 },
      { text: "ألوم الآخرين أو الظروف", score: 1 }
    ]
  },
  {
    q: "ما هو شعورك تجاه العمل الجماعي؟",
    answers: [
      { text: "أفضل العمل الجماعي والتعاون", score: 4 },
      { text: "أفضل العمل الفردي", score: 2 },
      { text: "لا أحب الاعتماد على الآخرين", score: 1 },
      { text: "يعتمد على نوع المهمة", score: 3 }
    ]
  },
  {
    q: "كيف تصف طريقة اتخاذك للقرارات؟",
    answers: [
      { text: "متأنٍ وأفكر جيدًا", score: 3 },
      { text: "سريع ولا أتردد", score: 4 },
      { text: "أتردد وأحتاج مساعدة", score: 1 },
      { text: "أتبع حدسي فقط", score: 2 }
    ]
  },
  {
    q: "هل تشعر بالراحة عند التعبير عن مشاعرك؟",
    answers: [
      { text: "نعم، أعبّر بحرية", score: 4 },
      { text: "أحيانًا فقط", score: 3 },
      { text: "نادراً ما أعبّر", score: 2 },
      { text: "أصعب عليّ التعبير", score: 1 }
    ]
  },
  {
    q: "كيف تتصرف عند مواجهة موقف غير متوقع؟",
    answers: [
      { text: "أتعامل معه بهدوء وأخطط", score: 4 },
      { text: "أتوتر وأشعر بالضغط", score: 1 },
      { text: "أطلب المساعدة", score: 3 },
      { text: "أتجاهل الأمر وأنتظر", score: 2 }
    ]
  },
  {
    q: "ما هي الطريقة التي تفضلها للتعلم؟",
    answers: [
      { text: "التعلم العملي والتجريبي", score: 4 },
      { text: "القراءة والدراسة", score: 3 },
      { text: "الاستماع والملاحظة", score: 2 },
      { text: "التعلم بالضغط أو الاندفاع", score: 1 }
    ]
  },
  {
    q: "كيف تتعامل مع الأشخاص الذين تختلف معهم في الرأي؟",
    answers: [
      { text: "أحترم رأيهم وأناقش بهدوء", score: 4 },
      { text: "أتجنب النقاشات معهم", score: 1 },
      { text: "أحاول إقناعهم بقوة", score: 3 },
      { text: "أشعر بالانزعاج وأبتعد", score: 2 }
    ]
  },
  {
    q: "هل تفضل التخطيط للمستقبل أم العيش في الحاضر؟",
    answers: [
      { text: "التخطيط للمستقبل دائمًا", score: 4 },
      { text: "العيش في الحاضر وأخذ الأمور ببساطة", score: 3 },
      { text: "أحيانًا أفكر بالمستقبل فقط", score: 2 },
      { text: "أعيش اللحظة ولا أهتم بالمستقبل", score: 1 }
    ]
  },
  {
    q: "كيف تتصرف إذا شعرت بالضغط من الآخرين؟",
    answers: [
      { text: "أتحدث معهم بصراحة عن مشاعري", score: 4 },
      { text: "أبتعد لأهدأ", score: 2 },
      { text: "أحاول إرضاء الجميع", score: 3 },
      { text: "أغضب وأتفجر أحيانًا", score: 1 }
    ]
  }
];

const results = [
  {
    minScore: 54,
    maxScore: 72,
    rank: "القائد الحازم",
    opinion: "أنت شخص يملك القدرة على اتخاذ القرارات بسرعة ويحب السيطرة، لكن أحيانًا تحتاج للتهدئة وعدم الاستعجال.",
    advice: "حاول تستمع أكثر للآخرين وتأخذ رأيهم قبل أن تحسم الأمور.",
    info: "القائد الحازم عادةً يتمتع بحس مسؤولية عالي، ولديه قدرة على تحفيز الناس حوله، لكنه يحتاج إلى تطوير الجانب العاطفي ليرتبط بالآخرين بشكل أفضل."
  },
  {
    minScore: 36,
    maxScore: 53,
    rank: "الشخص العاطفي",
    opinion: "أنت شخص حساس ومتفاعل مع مشاعره ومشاعر الآخرين، وهذا يجعلك محبوبًا لكن قد تتأثر بسهولة.",
    advice: "حاول أن توازن بين عاطفتك وعقلك ولا تدع المشاعر تسيطر عليك بشكل كامل.",
    info: "الشخص العاطفي يمتلك قدرة عالية على التعاطف والتواصل، لكنه يحتاج إلى تعزيز الثقة بالنفس ليكون أكثر توازنًا."
  },
  {
    minScore: 18,
    maxScore: 35,
    rank: "الشخص الهادئ",
    opinion: "تميل للهدوء والتفكير العميق، تفضل المراقبة بدلًا من المشاركة المباشرة.",
    advice: "حاول أن تخرج من قوقعتك وتشارك أكثر في الحياة الاجتماعية لتكتسب ثقة أكبر.",
    info: "الشخص الهادئ يتمتع بقدرة على التركيز وتحليل الأمور بعمق، لكنه قد يفقد فرصًا بسبب تردده."
  }
];

// المتغيرات
let currentQuestionIndex = 0;
let score = 0;

const questionArea = document.getElementById("question-area");
const nextBtn = document.getElementById("next-btn");
const quizContainer = document.getElementById("quiz-container");
const resultContainer = document.getElementById("result-container");
const resultRank = document.getElementById("result-rank");
const resultOpinion = document.getElementById("result-opinion");
const resultAdvice = document.getElementById("result-advice");
const resultInfo = document.getElementById("result-info");
const restartBtn = document.getElementById("restart-btn");

function showQuestion() {
  const q = questions[currentQuestionIndex];
  let answersHtml = "";
  q.answers.forEach((a, i) => {
    answersHtml += `
      <input type="radio" id="answer${i}" name="answer" value="${a.score}">
      <label for="answer${i}">${a.text}</label>
    `;
  });

  questionArea.innerHTML = `
    <div class="question-number">السؤال ${currentQuestionIndex + 1} من ${questions.length}</div>
    <div class="question-text">${q.q}</div>
    <div class="answers">${answersHtml}</div>
  `;
  nextBtn.disabled = true;

  const radios = questionArea.querySelectorAll("input[type=radio]");
  radios.forEach(radio => {
    radio.addEventListener("change", () => {
      nextBtn.disabled = false;
    });
  });
}

function showResult() {
  quizContainer.style.display = "none";
  resultContainer.style.display = "block";

  // تحديد النتيجة بناء على النقاط
  let userResult = results.find(r => score >= r.minScore && score <= r.maxScore);
  if (!userResult) userResult = results[results.length - 1]; // في حالة النقاط خارج النطاق

  resultRank.textContent = userResult.rank;
  resultOpinion.textContent = "رأيي فيها: " + userResult.opinion;
  resultAdvice.textContent = "نصيحتي: " + userResult.advice;
  resultInfo.textContent = "معلومات عن هذه الشخصية: " + userResult.info;
}

nextBtn.addEventListener("click", () => {
  // جمع النقاط من الإجابة المختارة
  const selected = document.querySelector('input[name="answer"]:checked');
  if (!selected) return;
  score += parseInt(selected.value);

  currentQuestionIndex++;
  if (currentQuestionIndex < questions.length) {
    showQuestion();
  } else {
    showResult();
  }
});

restartBtn.addEventListener("click", () => {
  // إعادة تعيين كل شيء
  currentQuestionIndex = 0;
  score = 0;
  resultContainer.style.display = "none";
  quizContainer.style.display = "block";
  showQuestion();
});

// بداية الاختبار
showQuestion();
</script>

</body>
</html>
