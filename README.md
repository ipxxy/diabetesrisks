<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Тест: Насколько ты в риске диабета?</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
      margin: 0;
      padding: 20px;
    }
    .container {
      max-width: 700px;
      margin: 0 auto;
      background: white;
      border-radius: 15px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.2);
      overflow: hidden;
    }
    header {
      background: #d32f2f;
      color: white;
      padding: 20px;
      text-align: center;
    }
    .quiz {
      padding: 30px;
    }
    .question {
      margin-bottom: 25px;
    }
    h2 {
      color: #333;
    }
    label {
      display: block;
      padding: 12px;
      margin: 8px 0;
      background: #f8f9fa;
      border-radius: 8px;
      cursor: pointer;
      transition: all 0.2s;
    }
    label:hover {
      background: #e3f2fd;
    }
    button {
      padding: 12px 25px;
      font-size: 16px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
    .next-btn {
      background: #1976d2;
      color: white;
    }
    .prev-btn {
      background: #666;
      color: white;
    }
    .result {
      text-align: center;
      padding: 40px 20px;
      display: none;
    }
    .high-risk {
      background: #ffebee;
      color: #c62828;
    }
    .low-risk {
      background: #e8f5e9;
      color: #2e7d32;
    }
    .progress {
      height: 6px;
      background: #1976d2;
      transition: width 0.3s;
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>Тест: Факторы риска диабета 2 типа</h1>
      <p>Отвечай честно • 8 вопросов</p>
    </header>
    
    <div class="quiz" id="quiz">
      <!-- Вопросы будут здесь -->
    </div>

    <div class="result" id="result"></div>
  </div>

  <script>
    const questions = [
      { q: "Тебе больше 40 лет?", name: "q1" },
      { q: "У тебя есть избыточный вес или ожирение? (ИМТ больше 25)", name: "q2" },
      { q: "У твоих родителей, братьев или сестёр есть сахарный диабет?", name: "q3" },
      { q: "Ты мало двигаешься? (меньше 30 минут активности в день)", name: "q4" },
      { q: "Ты часто ешь сладкое, фастфуд и сладкие напитки?", name: "q5" },
      { q: "У тебя повышенное артериальное давление (выше 130/85)?", name: "q6" },
      { q: "У тебя когда-нибудь был повышенный сахар в крови при анализе?", name: "q7" },
      { q: "Ты куришь или раньше курил?", name: "q8" }
    ];

    let currentQuestion = 0;
    let score = 0;
    let answers = {};

    const quizDiv = document.getElementById('quiz');
    const resultDiv = document.getElementById('result');

    function renderQuestion() {
      const progress = Math.round(((currentQuestion) / questions.length) * 100);

      quizDiv.innerHTML = `
        <div style="background:#eee; height:6px; border-radius:3px; margin-bottom:20px;">
          <div class="progress" style="width:${progress}%"></div>
        </div>
        
        <h2>Вопрос ${currentQuestion + 1} из ${questions.length}</h2>
        <p style="font-size:18px; margin:25px 0; line-height:1.4;">${questions[currentQuestion].q}</p>
        
        <label><input type="radio" name="answer" value="1"> Да</label>
        <label><input type="radio" name="answer" value="0"> Нет</label>
        
        <div style="margin-top:35px; display:flex; gap:10px;">
          ${currentQuestion > 0 ? 
            `<button onclick="prevQuestion()" class="prev-btn">← Назад</button>` : ''}
          <button onclick="nextQuestion()" class="next-btn" style="flex:1;">
            ${currentQuestion === questions.length - 1 ? 'Завершить тест' : 'Далее →'}
          </button>
        </div>
      `;

      // Восстанавливаем ранее выбранный ответ
      setTimeout(() => {
        if (answers[currentQuestion] !== undefined) {
          const radios = document.querySelectorAll('input[name="answer"]');
          radios.forEach(radio => {
            if (parseInt(radio.value) === answers[currentQuestion]) {
              radio.checked = true;
            }
          });
        }
      }, 10);
    }

    function nextQuestion() {
      const selected = document.querySelector('input[name="answer"]:checked');
      if (!selected) {
        alert("Пожалуйста, выбери Да или Нет!");
        return;
      }

      const value = parseInt(selected.value);
      const oldValue = answers[currentQuestion];

      // Корректируем счёт при изменении ответа
      if (oldValue !== undefined) {
        score -= oldValue;
      }
      score += value;

      answers[currentQuestion] = value;

      currentQuestion++;

      if (currentQuestion < questions.length) {
        renderQuestion();
      } else {
        showResult();
      }
    }

    function prevQuestion() {
      currentQuestion--;
      // Пересчитываем score полностью
      score = Object.values(answers).reduce((a, b) => a + b, 0);
      renderQuestion();
    }

    function showResult() {
      quizDiv.style.display = "none";
      resultDiv.style.display = "block";

      let riskText = "";
      let colorClass = "";

      if (score >= 5) {
        colorClass = "high-risk";
        riskText = `
          <h2>⚠️ Ты в группе повышенного риска!</h2>
          <p style="font-size:22px; margin:20px 0;">У тебя ${score} из 8 факторов риска.</p>
          <p><strong>Рекомендуется срочно:</strong></p>
          <ul style="text-align:left; display:inline-block;">
            <li>Сдать анализ крови на сахар (глюкоза натощак или HbA1c)</li>
            <li>Обратиться к терапевту или эндокринологу</li>
            <li>Начать больше двигаться и изменить питание</li>
          </ul>
        `;
      } else {
        colorClass = "low-risk";
        riskText = `
          <h2>✅ Риск низкий или средний</h2>
          <p style="font-size:22px; margin:20px 0;">У тебя ${score} из 8 факторов риска.</p>
          <p>Хороший результат! Для профилактики диабета всё равно рекомендуется:</p>
          <ul style="text-align:left; display:inline-block;">
            <li>Больше двигаться (минимум 30 минут в день)</li>
            <li>Правильно питаться, уменьшить сладкое</li>
            <li>Регулярно проверять здоровье</li>
          </ul>
        `;
      }

      resultDiv.innerHTML = `
        <div class="${colorClass}" style="padding:40px 25px; border-radius:15px;">
          ${riskText}
          <button onclick="location.reload()" style="margin-top:25px; padding:14px 30px; background:#1976d2; color:white; font-size:17px; border-radius:8px;">
            Пройти тест заново
          </button>
        </div>
      `;
    }

    // Запуск теста
    renderQuestion();
  </script>
</body>
</html>
