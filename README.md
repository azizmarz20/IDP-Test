<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>MCQ Test</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
    body {
        font-family: Arial, sans-serif;
        background: #f4f6f7;
        margin: 0;
        padding: 10px;
    }

    .container {
        max-width: 520px;
        margin: auto;
        background: #ffffff;
        padding: 20px;
        border-radius: 10px;
        border-top: 6px solid #007a3d; /* Aramco Green */
        box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }

    .header {
        text-align: center;
        color: #007a3d;
        font-weight: bold;
        margin-bottom: 15px;
    }

    h2 {
        font-size: 18px;
        margin-bottom: 15px;
        color: #333;
    }

    .option {
        padding: 12px;
        margin: 10px 0;
        border: 1px solid #ccc;
        border-radius: 6px;
        cursor: pointer;
    }

    .option.correct {
        background: #d4edda;
        border-color: #28a745;
    }

    .option.wrong {
        background: #f8d7da;
        border-color: #dc3545;
    }

    button {
        width: 100%;
        padding: 14px;
        margin-top: 15px;
        font-size: 16px;
        background: #007a3d;
        color: #fff;
        border: none;
        border-radius: 6px;
        cursor: pointer;
    }

    button:disabled {
        background: #aaa;
    }

    .feedback {
        margin-top: 12px;
        font-weight: bold;
    }

    .pass {
        color: #28a745;
        font-size: 20px;
        font-weight: bold;
    }

    .fail {
        color: #dc3545;
        font-size: 20px;
        font-weight: bold;
    }
</style>
</head>
<body>

<div class="container">
    <div class="header">Saudi Aramco Knowledge Assessment</div>

    <h2 id="question"></h2>
    <div id="options"></div>
    <div id="feedback" class="feedback"></div>

    <button id="nextBtn" onclick="nextQuestion()">Next</button>
</div>

<script>
    // ===== EDIT QUESTIONS HERE =====
    const questions = [
        {
            question: "What is the capital of Saudi Arabia?",
            options: ["Jeddah", "Riyadh", "Dammam", "Mecca"],
            correct: 1
        },
        {
            question: "Which color represents safety warning?",
            options: ["Green", "Blue", "Yellow", "White"],
            correct: 2
            },question: "who is you uncle?"
            options: ["Andulaziz", "Andulaziz", "Andulaziz", "Andulaziz",],
            correct: 3
        },
    ];
    // ===============================

    let currentQuestion = 0;
    let score = 0;
    let answered = false;

    const questionEl = document.getElementById("question");
    const optionsEl = document.getElementById("options");
    const feedbackEl = document.getElementById("feedback");
    const nextBtn = document.getElementById("nextBtn");

    function loadQuestion() {
        answered = false;
        nextBtn.disabled = true;
        feedbackEl.innerHTML = "";

        const q = questions[currentQuestion];
        questionEl.innerText = `Q${currentQuestion + 1}. ${q.question}`;
        optionsEl.innerHTML = "";

        q.options.forEach((opt, i) => {
            const div = document.createElement("div");
            div.className = "option";
            div.innerText = opt;
            div.onclick = () => selectOption(div, i);
            optionsEl.appendChild(div);
        });
    }

    function selectOption(div, index) {
        if (answered) return;
        answered = true;

        const correctIndex = questions[currentQuestion].correct;
        const options = document.querySelectorAll(".option");

        options.forEach((opt, i) => {
            if (i === correctIndex) opt.classList.add("correct");
            if (i === index && i !== correctIndex) opt.classList.add("wrong");
        });

        if (index === correctIndex) {
            feedbackEl.innerHTML = "Correct ✅";
            score++;
        } else {
            feedbackEl.innerHTML =
                `Wrong ❌<br>Correct Answer: <strong>${questions[currentQuestion].options[correctIndex]}</strong>`;
        }

        nextBtn.disabled = false;
    }

    function nextQuestion() {
        currentQuestion++;
        if (currentQuestion < questions.length) {
            loadQuestion();
        } else {
            showResult();
        }
    }

    function showResult() {
        const percentage = Math.round((score / questions.length) * 100);
        const pass = percentage >= 80;

        document.querySelector(".container").innerHTML = `
            <div class="header">Saudi Aramco Assessment Result</div>
            <h2>Final Score</h2>
            <p><strong>${score} / ${questions.length}</strong></p>
            <p><strong>${percentage}%</strong></p>
            <p class="${pass ? 'pass' : 'fail'}">
                ${pass ? 'PASS ✅' : 'FAIL ❌'}
            </p>
            <p>Passing requirement: 80%</p>
        `;
    }

    loadQuestion();
</script>

</body>
</html>
