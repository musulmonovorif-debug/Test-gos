<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<title>Test Sayt</title>
<style>
body { font-family: Arial; padding: 20px; }
button { padding: 10px; margin-top: 10px; }
.question { margin: 15px 0; }
</style>
</head>
<body>

<h2>Test yuklash</h2>
<textarea id="input" rows="10" cols="50" placeholder="Testlarni shu yerga qo‘y"></textarea><br>
<button onclick="loadTest()">Yuklash</button>

<div id="quiz"></div>
<button onclick="finish()">Tugatish</button>

<h3 id="result"></h3>

<script>
let questions = [];
let answers = [];

function loadTest() {
    let text = document.getElementById("input").value;
    let blocks = text.split("\n\n");

    questions = blocks.map(b => {
        let lines = b.split("\n");
        return {
            q: lines[0],
            a: lines.slice(1,5),
            correct: lines[5].split(":")[1].trim()
        };
    });

    questions.sort(() => Math.random() - 0.5);

    let html = "";
    questions.forEach((item, i) => {
        html += `<div class="question">
        <p>${i+1}. ${item.q}</p>`;
        item.a.forEach(opt => {
            html += `<label>
            <input type="radio" name="q${i}" value="${opt[0]}"> ${opt}
            </label><br>`;
        });
        html += "</div>";
    });

    document.getElementById("quiz").innerHTML = html;
}

function finish() {
    let score = 0;

    questions.forEach((q, i) => {
        let selected = document.querySelector(`input[name=q${i}]:checked`);
        if (selected && selected.value === q.correct) {
            score++;
        }
    });

    document.getElementById("result").innerText =
        `Natija: ${score} / ${questions.length}`;
}
</script>

</body>
</html>
