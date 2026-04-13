<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="format-detection" content="telephone=no">
  <title>Тест: Насколько ты в риске диабета?</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
      margin: 0;
      padding: 10px;
      min-height: 100vh;
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
      padding: 25px;
    }

    .progress {
      height: 6px;
      background: #1976d2;
      transition: width 0.3s;
    }

    h2 {
      color: #333;
      margin: 15px 0;
    }

    p {
      font-size: 17px;
      line-height: 1.45;
    }

    label {
      display: block;
      padding: 14px;
      margin: 10px 0;
      background: #f8f9fa;
      border-radius: 10px;
      cursor: pointer;
      transition: all 0.2s;
      font-size: 16px;
    }

    label:hover {
      background: #e3f2fd;
    }

    button {
      padding: 13px 25px;
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
      padding: 30px 20px;
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

    /* Мобильная адаптация */
    @media (max-width: 480px) {
      body {
        padding: 8px;
      }
      .container {
        border-radius: 12px;
      }
      .quiz {
        padding: 20px 18px;
      }
      header {
        padding: 18px 15px;
      }
      h1 {
        font-size: 1.35rem;
      }
      p {
        font-size: 16px;
      }
      label {
        padding: 13px;
        font-size: 15.5px;
        margin: 8px 0;
      }
      button {
        padding: 14px 20px;
        font-size: 15.5px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>Тест: Факторы риска диабета 2 типа</h1>
      <p>Отвечай честно • 8 вопросов</p>
    </header>
    
    <div class="quiz" id="quiz"></div>
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
        <p>${questions[currentQuestion].q}</p>
        
        <label><input type="radio" name="answer" value="1"> Да</label>
        <label><input type="radio" name="answer" value="0"> Нет</label>
        
        <div style="margin-top:30px; display:flex; gap:10px;">
          ${currentQuestion > 0 ? 
            `<button onclick="prevQuestion()" class="prev-btn">← Назад</button>` : ''}
          <button onclick="nextQuestion()" class="next-btn" style="flex:1;">
            ${currentQuestion === questions.length - 1 ? 'Завершить тест' : 'Далее →'}
          </button>
        </div>
      `;

      setTimeout(() => {
        if (answers[currentQuestion] !== undefined) {
          const radios = document.querySelectorAll('input[name="answer"]');
          radios.forEach(radio => {
            if (parseInt(radio.value) === answers[currentQuestion]) radio.checked = true;
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
      if (answers[currentQuestion] !== undefined) score -= answers[currentQuestion];
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
      score = Object.values(answers).reduce((a, b) => a + b, 0);
      renderQuestion();
    }

    function showResult() {
      quizDiv.style.display = "none";
      resultDiv.style.display = "block";

      const isHighRisk = score >= 5;
      
      resultDiv.innerHTML = `
        <div class="${isHighRisk ? 'high-risk' : 'low-risk'}" style="padding:35px 20px; border-radius:15px;">
          <h2>${isHighRisk ? '⚠️ Ты в группе повышенного риска!' : '✅ Риск низкий или средний'}</h2>
          <p style="font-size:21px; margin:20px 0;">У тебя ${score} из 8 факторов риска.</p>
          
          ${isHighRisk ? 
            `<p><strong>Рекомендуется:</strong></p>
             <ul style="text-align:left; display:inline-block; line-height:1.6;">
               <li>Сдать анализ на сахар крови</li>
               <li>Обратиться к врачу (терапевт/эндокринолог)</li>
               <li>Изменить питание и увеличить активность</li>
             </ul>` : 
            `<p>Хороший результат! Продолжай вести здоровый образ жизни.</p>`}
          
          <button onclick="location.reload()" style="margin-top:25px; padding:14px 30px; background:#1976d2; color:white; font-size:16px; border-radius:8px;">
            Пройти тест заново
          </button>
        </div>
      `;
    }

    renderQuestion();
  </script>
</body>
</html>
