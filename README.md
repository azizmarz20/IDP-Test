<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>Saudi Aramco Knowledge Assessment</title>

<style>
/* ================= THEME ================= */
:root {
    --bg-dark: #020617;
    --card-dark: #0f172a;
    --text-dark: #e5e7eb;
    --border-dark: #1e293b;

    --bg-light: #f4f6f7;
    --card-light: #ffffff;
    --text-light: #111827;
    --border-light: #d1d5db;

    --green: #007a3d;
    --correct: #14532d;
    --wrong: #7f1d1d;
}

html[data-theme="dark"] {
    background: var(--bg-dark);
    color: var(--text-dark);
}

html[data-theme="light"] {
    background: var(--bg-light);
    color: var(--text-light);
}

/* ================= BASE ================= */
body {
    margin: 0;
    padding: env(safe-area-inset-top) env(safe-area-inset-right)
             env(safe-area-inset-bottom) env(safe-area-inset-left);
    font-family: Arial, sans-serif;
}

/* ================= CARD ================= */
.container {
    width: 100%;
    max-width: 340px;   /* TRUE iPhone fit */
    margin: 12px auto;
    padding: 14px;
    border-radius: 14px;
    border-top: 5px solid var(--green);
    background:
        linear-gradient(
            to bottom,
            rgba(0,0,0,0.03),
            rgba(0,0,0,0.03)
        );
}

html[data-theme="dark"] .container {
    background: var(--card-dark);
    border-color: var(--border-dark);
}

html[data-theme="light"] .container {
    background: var(--card-light);
    border-color: var(--border-light);
}

/* ================= TEXT ================= */
.header {
    text-align: center;
    color: var(--green);
    font-weight: bold;
    font-size: 14px;
    margin-bottom: 10px;
}

h2 {
    font-size: 15px;
    line-height: 1.4;
}

/* ================= OPTIONS ================= */
.option {
    padding: 14px;
    margin: 10px 0;
    border-radius: 12px;
    border: 1px solid;
    cursor: pointer;
}

html[data-theme="dark"] .option {
    border-color: var(--border-dark);
}

html[data-theme="light"] .option {
    border-color: var(--border-light);
}

.option.correct {
    background: var(--correct);
    border-color: #22c55e;
}

.option.wrong {
    background: var(--wrong);
    border-color: #ef4444;
}

/* ================= BUTTON ================= */
button {
    width: 100%;
    padding: 14px;
    font-size: 15px;
    background: var(--green);
    color: white;
    border: none;
    border-radius: 12px;
    margin-top: 12px;
}

button:disabled {
    opacity: 0.5;
}

/* ================= EXTRA ================= */
.feedback {
    margin-top: 8px;
    font-weight: bold;
}

.toggle {
    text-align: right;
    font-size: 12px;
    opacity: 0.7;
    cursor: pointer;
}
.pass { color: #22c55e; font-weight: bold; }
.fail { color: #ef4444; font-weight: bold; }
</style>
</head>

<body>

<div class="container">
    <div class="toggle" onclick="toggleTheme()">🌙 Dark / ☀️ Light</div>
    <div class="header">Saudi Aramco Knowledge Assessment</div>

    <h2 id="question"></h2>
    <div id="options"></div>
    <div id="feedback" class="feedback"></div>

    <button id="nextBtn" onclick="nextQuestion()">Next</button>
</div>

<script>
/* ===== QUESTIONS ===== */
const questions = [
    {
        question: "What is the capital of Saudi Arabia?",
        options: ["Jeddah", "Riyadh", "Dammam", "Mecca"],
        correct: 1
    },
    {
        question: "Which color indicates a warning sign?",
        options: ["Green", "Blue", "Yellow", "White"],
        correct: 2
    }
];

let current = 0, score = 0, answered = false;

/* Shuffle */
function shuffle(a){for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]];}}

/* Theme toggle */
function toggleTheme(){
    const html = document.documentElement;
    html.setAttribute(
        "data-theme",
        html.getAttribute("data-theme") === "dark" ? "light" : "dark"
    );
}

/* Elements */
const qEl = document.getElementById("question");
const oEl = document.getElementById("options");
const fEl = document.getElementById("feedback");
const nBtn = document.getElementById("nextBtn");

/* Load */
function load(){
    answered = false;
    nBtn.disabled = true;
    fEl.innerHTML = "";
    oEl.innerHTML = "";

    const q = questions[current];
    qEl.innerText = `Q${current+1}. ${q.question}`;

    let opts = q.options.map((t,i)=>({t,i}));
    shuffle(opts);

    opts.forEach(o=>{
        const d=document.createElement("div");
        d.className="option";
        d.innerText=o.t;
        d.onclick=()=>select(d,o.i,q.correct);
        oEl.appendChild(d);
    });
}

function select(div,s,c){
    if(answered) return;
    answered=true;

    document.querySelectorAll(".option").forEach(o=>{
        if(o.innerText===questions[current].options[c]) o.classList.add("correct");
    });

    if(s===c){ score++; fEl.innerText="Correct ✅"; }
    else{ div.classList.add("wrong"); fEl.innerHTML=`Wrong ❌<br>Correct: <b>${questions[current].options[c]}</b>`; }

    nBtn.disabled=false;
}

function nextQuestion(){
    current++;
    current<questions.length?load():result();
}

function result(){
    const p=Math.round(score/questions.length*100);
    document.querySelector(".container").innerHTML=`
        <div class="header">Result</div>
        <p>${score}/${questions.length}</p>
        <p>${p}%</p>
        <p class="${p>=80?'pass':'fail'}">${p>=80?'PASS':'FAIL'}</p>
        <p>Passing score: 80%</p>`;
}

/* Start */
shuffle(questions);
load();
</script>

</body>
</html>