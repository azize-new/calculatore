<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>آلة حاسبة متقدمة</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            transition: background-color 0.3s, color 0.3s;
        }

        .calculator {
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            width: 350px;
            text-align: center;
        }

        .display {
            width: 100%;
            height: 60px;
            background: #f0f0f0;
            border: none;
            border-radius: 5px;
            margin-bottom: 10px;
            text-align: right;
            font-size: 1.5rem;
            padding: 10px;
        }

        .history {
            background: #e9ecef;
            border: none;
            border-radius: 5px;
            height: 80px;
            overflow-y: auto;
            margin-bottom: 10px;
            padding: 10px;
            font-size: 1rem;
            color: #555;
            text-align: right;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            height: 60px;
            font-size: 1.2rem;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s, transform 0.1s;
        }

        button:hover {
            background: #0056b3;
        }

        button:active {
            transform: scale(0.95);
        }

        .clear {
            background: #dc3545;
        }

        .clear:hover {
            background: #a71d2a;
        }

        .equal {
            background: #28a745;
        }

        .equal:hover {
            background: #1c7c31;
        }

        .dark-mode {
            background-color: #222;
            color: #ddd;
        }

        .dark-mode .calculator {
            background: #333;
        }

        .dark-mode .display,
        .dark-mode .history {
            background: #444;
            color: #ddd;
        }

        .dark-mode button {
            background: #555;
        }

        .dark-mode button:hover {
            background: #777;
        }

        .dark-mode .clear {
            background: #b32d3a;
        }

        .dark-mode .clear:hover {
            background: #93222e;
        }

        .dark-mode .equal {
            background: #1f8b3d;
        }

        .dark-mode .equal:hover {
            background: #146429;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <input type="text" class="display" disabled />
        <div class="history"></div>
        <div class="buttons">
            <button class="clear">C</button>
            <button data-value="(">(</button>
            <button data-value=")">)</button>
            <button data-value="/">/</button>
            <button data-value="7">7</button>
            <button data-value="8">8</button>
            <button data-value="9">9</button>
            <button data-value="*">*</button>
            <button data-value="4">4</button>
            <button data-value="5">5</button>
            <button data-value="6">6</button>
            <button data-value="-">-</button>
            <button data-value="1">1</button>
            <button data-value="2">2</button>
            <button data-value="3">3</button>
            <button data-value="+">+</button>
            <button data-value="0">0</button>
            <button data-value=".">.</button>
            <button data-value="sqrt">√</button>
            <button data-value="^">^</button>
            <button data-value="%">%</button>
            <button class="equal" style="grid-column: span 2;">=</button>
            <button id="toggle-mode" style="grid-column: span 2;">الوضع الليلي</button>
        </div>
    </div>

    <script>
        const display = document.querySelector('.display');
        const buttons = document.querySelectorAll('.buttons button[data-value]');
        const history = document.querySelector('.history');
        const toggleMode = document.getElementById('toggle-mode');

        let currentInput = '';
        let darkMode = false;

        buttons.forEach(button => {
            button.addEventListener('click', () => {
                const buttonValue = button.getAttribute('data-value');

                if (buttonValue === 'sqrt') {
                    currentInput += 'Math.sqrt(';
                } else if (buttonValue === '^') {
                    currentInput += '**';
                } else {
                    currentInput += buttonValue;
                }

                display.value = currentInput;
            });
        });

        document.querySelector('.clear').addEventListener('click', () => {
            currentInput = '';
            display.value = '';
        });

        document.querySelector('.equal').addEventListener('click', () => {
            try {
                let result = eval(currentInput);
                history.innerHTML += `<div>${currentInput} = ${result}</div>`;
                display.value = result;
                currentInput = result.toString();
            } catch {
                display.value = 'خطأ';
            }
        });

        document.addEventListener('keydown', (e) => {
            if (!isNaN(e.key) || ['+', '-', '*', '/', '(', ')', '.', '%'].includes(e.key)) {
                currentInput += e.key;
                display.value = currentInput;
            } else if (e.key === 'Enter') {
                try {
                    let result = eval(currentInput);
                    history.innerHTML += `<div>${currentInput} = ${result}</div>`;
                    display.value = result;
                    currentInput = result.toString();
                } catch {
                    display.value = 'خطأ';
                }
            } else if (e.key === 'Backspace') {
                currentInput = currentInput.slice(0, -1);
                display.value = currentInput;
            }
        });

        toggleMode.addEventListener('click', () => {
            document.body.classList.toggle('dark-mode');
            darkMode = !darkMode;
            toggleMode.textContent = darkMode ? 'الوضع النهاري' : 'الوضع الليلي';
        });
    </script>
</body>
</html>
