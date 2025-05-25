from fastapi import FastAPI, Form
from fastapi.responses import HTMLResponse

app = FastAPI()

# HTML с чиби Райден Сёгуном
HTML_PAGE = """
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Калькулятор с Райден Сёгуном</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .calculator-container {
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(15px);
            border-radius: 25px;
            padding: 40px;
            box-shadow: 0 8px 32px rgba(31, 38, 135, 0.37);
            border: 1px solid rgba(255, 255, 255, 0.18);
            max-width: 600px;
            width: 100%;
        }
        
        .title {
            text-align: center;
            color: white;
            font-size: 2.5em;
            margin-bottom: 40px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        .input-group {
            display: flex;
            align-items: center;
            margin-bottom: 30px;
            gap: 20px;
        }
        
        .chibi-raiden {
            width: 80px;
            height: 80px;
            background: linear-gradient(45deg, #9d4edd, #c77dff);
            border-radius: 50%;
            position: relative;
            flex-shrink: 0;
            box-shadow: 0 6px 20px rgba(157, 78, 221, 0.5);
            animation: float 3s ease-in-out infinite;
        }
        
        .chibi-raiden::before {
            content: '';
            position: absolute;
            top: 25%;
            left: 50%;
            transform: translateX(-50%);
            width: 50px;
            height: 35px;
            background: #fdbcb4;
            border-radius: 50% 50% 40% 40%;
            z-index: 2;
        }
        
        .chibi-raiden::after {
            content: '';
            position: absolute;
            top: 15%;
            left: 50%;
            transform: translateX(-50%);
            width: 60px;
            height: 40px;
            background: #4c1d95;
            border-radius: 50% 50% 20% 20%;
            z-index: 1;
        }
        
        .chibi-eyes {
            position: absolute;
            top: 40%;
            left: 50%;
            transform: translateX(-50%);
            width: 30px;
            height: 8px;
            z-index: 3;
        }
        
        .chibi-eyes::before,
        .chibi-eyes::after {
            content: '';
            position: absolute;
            width: 8px;
            height: 8px;
            background: #2d1b69;
            border-radius: 50%;
            top: 0;
        }
        
        .chibi-eyes::before { left: 2px; }
        .chibi-eyes::after { right: 2px; }
        
        .chibi-mouth {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translateX(-50%);
            width: 15px;
            height: 8px;
            border: 2px solid #2d1b69;
            border-top: none;
            border-radius: 0 0 15px 15px;
            z-index: 3;
        }
        
        .lightning {
            position: absolute;
            color: #ffd60a;
            font-size: 1.5em;
            animation: sparkle 1.5s infinite;
        }
        
        .lightning-1 { top: -10px; right: -5px; }
        .lightning-2 { bottom: -5px; left: -8px; font-size: 1.2em; }
        
        @keyframes float {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-12px) rotate(2deg); }
        }
        
        @keyframes sparkle {
            0%, 100% { opacity: 0.3; transform: scale(0.8); }
            50% { opacity: 1; transform: scale(1.3); }
        }
        
        .input-wrapper {
            flex-grow: 1;
        }
        
        label {
            display: block;
            color: white;
            font-size: 1.3em;
            margin-bottom: 8px;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
            font-weight: bold;
        }
        
        input, select {
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 12px;
            background: rgba(255, 255, 255, 0.25);
            color: white;
            font-size: 1.1em;
            backdrop-filter: blur(5px);
            border: 2px solid rgba(255, 255, 255, 0.3);
            transition: all 0.3s ease;
        }
        
        input:focus, select:focus {
            outline: none;
            background: rgba(255, 255, 255, 0.35);
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(157, 78, 221, 0.3);
        }
        
        input::placeholder {
            color: rgba(255, 255, 255, 0.8);
        }
        
        select option {
            background: #667eea;
            color: white;
        }
        
        .calculate-btn {
            width: 100%;
            padding: 20px;
            background: linear-gradient(45deg, #f72585, #c77dff, #9d4edd);
            color: white;
            border: none;
            border-radius: 20px;
            font-size: 1.4em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 6px 20px rgba(157, 78, 221, 0.4);
            margin-top: 30px;
            text-transform: uppercase;
        }
        
        .calculate-btn:hover {
            transform: translateY(-4px);
            box-shadow: 0 12px 35px rgba(157, 78, 221, 0.6);
            background: linear-gradient(45deg, #ff006e, #8338ec, #3a0ca3);
        }
        
        .result {
            margin-top: 30px;
            padding: 25px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 20px;
            border: 2px solid rgba(255, 255, 255, 0.3);
            color: white;
            font-size: 1.6em;
            text-align: center;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
            animation: resultAppear 0.5s ease-out;
        }
        
        @keyframes resultAppear {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .result-success {
            background: linear-gradient(45deg, rgba(46, 213, 115, 0.3), rgba(0, 206, 201, 0.3));
            border-color: rgba(46, 213, 115, 0.5);
        }
        
        .result-error {
            background: linear-gradient(45deg, rgba(231, 76, 60, 0.3), rgba(192, 57, 43, 0.3));
            border-color: rgba(231, 76, 60, 0.5);
        }
        
        @media (max-width: 768px) {
            .input-group {
                flex-direction: column;
                text-align: center;
            }
            
            .chibi-raiden {
                width: 70px;
                height: 70px;
            }
            
            .title {
                font-size: 2em;
            }
        }
    </style>
</head>
<body>
    <div class="calculator-container">
        <h1 class="title">⚡ Калькулятор Райден Сёгуна ⚡</h1>
        
        <form method="post" action="/calculate">
            <div class="input-group">
                <div class="chibi-raiden">
                    <div class="chibi-eyes"></div>
                    <div class="chibi-mouth"></div>
                    <div class="lightning lightning-1">⚡</div>
                    <div class="lightning lightning-2">✨</div>
                </div>
                <div class="input-wrapper">
                    <label>Введите первое число:</label>
                    <input type="number" name="number1" step="any" placeholder="Например: 42" required>
                </div>
            </div>

            <div class="input-group">
                <div class="chibi-raiden">
                    <div class="chibi-eyes"></div>
                    <div class="chibi-mouth"></div>
                    <div class="lightning lightning-1">⚡</div>
                    <div class="lightning lightning-2">⭐</div>
                </div>
                <div class="input-wrapper">
                    <label>Выберите операцию:</label>
                    <select name="operation" required>
                        <option value="">-- Выберите операцию --</option>
                        <option value="add">➕ Сложение</option>
                        <option value="subtract">➖ Вычитание</option>
                        <option value="multiply">✖️ Умножение</option>
                        <option value="divide">➗ Деление</option>
                        <option value="power">🔥 Возведение в степень</option>
                    </select>
                </div>
            </div>

            <div class="input-group">
                <div class="chibi-raiden">
                    <div class="chibi-eyes"></div>
                    <div class="chibi-mouth"></div>
                    <div class="lightning lightning-1">⚡</div>
                    <div class="lightning lightning-2">💫</div>
                </div>
                <div class="input-wrapper">
                    <label>Введите второе число:</label>
                    <input type="number" name="number2" step="any" placeholder="Например: 13" required>
                </div>
            </div>

            <button type="submit" class="calculate-btn">
                ⚡ Вычислить с силой Электро! ⚡
            </button>
        </form>

        {result_section}
    </div>
</body>
</html>
"""

@app.get("/", response_class=HTMLResponse)
async def home():
    return HTMLResponse(HTML_PAGE.replace("{result_section}", ""))

@app.post("/calculate", response_class=HTMLResponse)
async def calculate(
    number1: float = Form(...),
    number2: float = Form(...),
    operation: str = Form(...)
):
    try:
        if operation == "add":
            result = number1 + number2
            symbol = "+"
        elif operation == "subtract":
            result = number1 - number2
            symbol = "-"
        elif operation == "multiply":
            result = number1 * number2
            symbol = "×"
        elif operation == "divide":
            if number2 == 0:
                result_html = '<div class="result result-error">⚠️ Ошибка: Деление на ноль запрещено!</div>'
                return HTMLResponse(HTML_PAGE.replace("{result_section}", result_html))
            result = number1 / number2
            symbol = "÷"
        elif operation == "power":
            result = number1 ** number2
            symbol = "^"
        else:
            result_html = '<div class="result result-error">❌ Неизвестная операция</div>'
            return HTMLResponse(HTML_PAGE.replace("{result_section}", result_html))
        
        # Форматируем результат
        if isinstance(result, float) and result.is_integer():
            result = int(result)
        elif isinstance(result, float):
            result = round(result, 6)
        
        result_html = f'''
        <div class="result result-success">
            ✨ {number1} {symbol} {number2} = {result} ✨
        </div>
        '''
        
        return HTMLResponse(HTML_PAGE.replace("{result_section}", result_html))
        
    except Exception as e:
        result_html = f'<div class="result result-error">❌ Ошибка: {str(e)}</div>'
        return HTMLResponse(HTML_PAGE.replace("{result_section}", result_html))

# Для Replit
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
