<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Печатающийся текст</title>
    <style>
        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 20px;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 40px;
            max-width: 600px;
            width: 100%;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .text-display {
            font-size: 24px;
            line-height: 1.6;
            color: #ffffff;
            min-height: 400px;
            white-space: pre-line;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        .cursor {
            display: inline-block;
            background-color: #ffffff;
            width: 2px;
            animation: blink 1s infinite;
        }
        
        @keyframes blink {
            0%, 50% { opacity: 1; }
            51%, 100% { opacity: 0; }
        }
        
        .controls {
            text-align: center;
            margin-top: 30px;
        }
        
        .btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 25px;
            font-size: 16px;
            cursor: pointer;
            margin: 0 10px;
            transition: transform 0.2s, box-shadow 0.2s;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }
        
        .btn:active {
            transform: translateY(0);
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 20px;
                margin: 10px;
            }
            
            .text-display {
                font-size: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="text-display" id="textDisplay"></div>
        <div class="controls">
            <button class="btn" onclick="startTypewriter()">Начать</button>
            <button class="btn" onclick="resetText()">Сброс</button>
        </div>
    </div>

    <script>
        const lines = [
            "Я выполню любую твою просьбу, будто монолит",
            "Зачем ты сбросил ее с неба, господь? Ее дом это олимп",
            "И мой ебальник светиться, как малая медведица", 
            "Какая ты пиздатая досталась мне не верится",
            "",
            "Охуительна, ослепительна",
            "Невозможно совсем не любить тебя",
            "Руки целовал твоим родителям",
            "Все другие телки на любителя"
        ];
        
        const delays = [66, 70, 63, 60, 100, 90, 90, 85, 88]; // в миллисекундах
        
        let isTyping = false;
        let currentTimeout;
        
        function startTypewriter() {
            if (isTyping) return;
            
            const textDisplay = document.getElementById('textDisplay');
            textDisplay.innerHTML = '<span class="cursor">|</span>';
            
            isTyping = true;
            typeLines(0);
        }
        
        function typeLines(lineIndex) {
            if (lineIndex >= lines.length) {
                isTyping = false;
                removeCursor();
return;
            }
            
            const line = lines[lineIndex];
            const delay = delays[lineIndex] || delays[delays.length - 1];
            
            typeLine(line, 0, delay, () => {
                // После завершения строки добавляем перенос и паузу
                const textDisplay = document.getElementById('textDisplay');
                const currentText = textDisplay.innerHTML.replace('<span class="cursor">|</span>', '');
                textDisplay.innerHTML = currentText + '\n<span class="cursor">|</span>';
                
                // Пауза между строками
                currentTimeout = setTimeout(() => {
                    typeLines(lineIndex + 1);
                }, 800);
            });
        }
        
        function typeLine(text, charIndex, delay, callback) {
            if (charIndex >= text.length) {
                callback();
                return;
            }
            
            const textDisplay = document.getElementById('textDisplay');
            const currentText = textDisplay.innerHTML.replace('<span class="cursor">|</span>', '');
            const char = text[charIndex];
            
            textDisplay.innerHTML = currentText + char + '<span class="cursor">|</span>';
            
            currentTimeout = setTimeout(() => {
                typeLine(text, charIndex + 1, delay, callback);
            }, delay);
        }
        
        function removeCursor() {
            const textDisplay = document.getElementById('textDisplay');
            textDisplay.innerHTML = textDisplay.innerHTML.replace('<span class="cursor">|</span>', '');
        }
        
        function resetText() {
            if (currentTimeout) {
                clearTimeout(currentTimeout);
            }
            isTyping = false;
            const textDisplay = document.getElementById('textDisplay');
            textDisplay.innerHTML = '';
        }
        
        // Автоматический запуск через 2 секунды после загрузки
        window.addEventListener('load', () => {
            setTimeout(startTypewriter, 2000);
        });
    </script>
</body>
</html>
