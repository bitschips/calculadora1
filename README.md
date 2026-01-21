# 🎯 Adivina el Número (HTML + CSS + JavaScript)

Este proyecto es un minijuego web donde el usuario debe adivinar un número del **1 al 100** en **3 intentos**.  
Incluye interfaz con estilos (CSS), lógica del juego (JavaScript) e historial de intentos.

---

## ✅ Código completo (`index.html`)

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Adivina el Número</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            max-width: 500px;
            width: 100%;
            text-align: center;
        }

        h1 {
            color: #667eea;
            margin-bottom: 10px;
            font-size: 2.5em;
        }

        .subtitle {
            color: #666;
            margin-bottom: 30px;
            font-size: 1.1em;
        }

        .game-info {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 25px;
        }

        .attempts {
            font-size: 1.2em;
            color: #333;
            margin-bottom: 10px;
        }

        .attempts-count {
            font-weight: bold;
            color: #667eea;
            font-size: 1.4em;
        }

        input[type="number"] {
            width: 100%;
            padding: 15px;
            font-size: 1.2em;
            border: 2px solid #ddd;
            border-radius: 10px;
            margin-bottom: 20px;
            text-align: center;
            transition: border-color 0.3s;
        }

        input[type="number"]:focus {
            outline: none;
            border-color: #667eea;
        }

        button {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.1em;
            border-radius: 10px;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            margin: 5px;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        button:active {
            transform: translateY(0);
        }

        .message {
            margin-top: 25px;
            padding: 15px;
            border-radius: 10px;
            font-size: 1.1em;
            font-weight: bold;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .message.hint {
            background: #fff3cd;
            color: #856404;
        }

        .message.success {
            background: #d4edda;
            color: #155724;
        }

        .message.error {
            background: #f8d7da;
            color: #721c24;
        }

        .history {
            margin-top: 20px;
            text-align: left;
        }

        .history-title {
            font-weight: bold;
            color: #667eea;
            margin-bottom: 10px;
        }

        .history-item {
            padding: 8px;
            margin: 5px 0;
            background: #f8f9fa;
            border-radius: 5px;
            border-left: 3px solid #667eea;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎯 Adivina el Número</h1>
        <p class="subtitle">Tengo un número del 1 al 100 en mente</p>
        
        <div class="game-info">
            <div class="attempts">
                Intentos restantes: <span class="attempts-count" id="attemptsLeft">3</span>
            </div>
        </div>

        <input type="number" id="guessInput" placeholder="Escribe tu número" min="1" max="100">
        
        <div>
            <button id="guessBtn" onclick="makeGuess()">Adivinar</button>
            <button onclick="resetGame()">Nuevo Juego</button>
        </div>

        <div id="message" class="message"></div>

        <div class="history">
            <div class="history-title">Historial de intentos:</div>
            <div id="history"></div>
        </div>
    </div>

    <script>
        let secretNumber;
        let attemptsLeft;
        let gameHistory;

        function initGame() {
            secretNumber = Math.floor(Math.random() * 100) + 1;
            attemptsLeft = 3;
            gameHistory = [];
            updateDisplay();
        }

        function updateDisplay() {
            document.getElementById('attemptsLeft').textContent = attemptsLeft;
            document.getElementById('guessInput').value = '';
            document.getElementById('message').textContent = '';
            document.getElementById('message').className = 'message';
            updateHistory();
        }

        function updateHistory() {
            const historyDiv = document.getElementById('history');
            historyDiv.innerHTML = gameHistory.map(item => 
                `<div class="history-item">${item}</div>`
            ).join('');
        }

        function makeGuess() {
            const guessInput = document.getElementById('guessInput');
            const guess = parseInt(guessInput.value);
            const messageDiv = document.getElementById('message');
            const guessBtn = document.getElementById('guessBtn');

            if (!guess || guess < 1 || guess > 100) {
                messageDiv.textContent = '⚠️ Por favor, ingresa un número entre 1 y 100';
                messageDiv.className = 'message hint';
                return;
            }

            attemptsLeft--;
            gameHistory.push(`Intento: ${guess}`);

            if (guess === secretNumber) {
                messageDiv.textContent = `🎉 ¡FELICITACIONES! ¡Adivinaste el número ${secretNumber}!`;
                messageDiv.className = 'message success';
                guessBtn.disabled = true;
                guessInput.disabled = true;
            } else if (attemptsLeft === 0) {
                messageDiv.textContent = `😔 ¡Perdiste! El número era ${secretNumber}`;
                messageDiv.className = 'message error';
                guessBtn.disabled = true;
                guessInput.disabled = true;
            } else {
                const hint = guess < secretNumber ? 'más alto' : 'más bajo';
                messageDiv.textContent = `❌ No es correcto. El número es ${hint}`;
                messageDiv.className = 'message hint';
            }

            document.getElementById('attemptsLeft').textContent = attemptsLeft;
            updateHistory();
        }

        function resetGame() {
            document.getElementById('guessBtn').disabled = false;
            document.getElementById('guessInput').disabled = false;
            initGame();
        }

        document.getElementById('guessInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                makeGuess();
            }
        });

        initGame();
    </script>
</body>
</html>
```

# Tareas pendientes:
