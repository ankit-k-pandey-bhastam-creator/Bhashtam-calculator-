# Bhashtam-calculator-
Bhastam Calculator by Ankit Kumar Pandey: Your premium financial expert. Explore a world of smart investing with our sleek Glassmorphism UI. From SIP and EMI to Tax and Lumpsum, get precise results in seconds. Modern design meets powerful calculation for your wealth growth. Contact: ankit1994patna@gmail.com. Precision at its best!
from flask import Flask, render_template_string, request, jsonify

app = Flask(__name__)

# Basic calculator logic
def calculate(expression):
    try:
        # Warning: eval can be dangerous, but for a simple calculator template it's often used.
        # In a real app, we would use a proper parser.
        # Replacing common operators to ensure basic safety
        safe_expression = expression.replace('x', '*').replace('÷', '/')
        result = eval(safe_expression, {"__builtins__": None}, {})
        return result
    except Exception as e:
        return "Error"

HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Calculator</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f2f5;
        }
        .calculator {
            background-color: #fff;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            width: 320px;
        }
        #display {
            width: 100%;
            height: 60px;
            font-size: 24px;
            text-align: right;
            margin-bottom: 20px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 8px;
            box-sizing: border-box;
            background-color: #fafafa;
        }
        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }
        button {
            padding: 20px;
            font-size: 18px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            background-color: #e9ecef;
            transition: background-color 0.2s;
        }
        button:hover {
            background-color: #dee2e6;
        }
        .operator {
            background-color: #ff9500;
            color: white;
        }
        .operator:hover {
            background-color: #e68600;
        }
        .equals {
            background-color: #007bff;
            color: white;
            grid-column: span 2;
        }
        .equals:hover {
            background-color: #0069d9;
        }
        .clear {
            background-color: #dc3545;
            color: white;
            grid-column: span 2;
        }
        .clear:hover {
            background-color: #c82333;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <input type="text" id="display" readonly>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="operator" onclick="appendToDisplay('/')">÷</button>
            <button class="operator" onclick="appendToDisplay('*')">×</button>
            <button onclick="appendToDisplay('7')">7</button>
            <button onclick="appendToDisplay('8')">8</button>
            <button onclick="appendToDisplay('9')">9</button>
            <button class="operator" onclick="appendToDisplay('-')">-</button>
            <button onclick="appendToDisplay('4')">4</button>
            <button onclick="appendToDisplay('5')">5</button>
            <button onclick="appendToDisplay('6')">6</button>
            <button class="operator" onclick="appendToDisplay('+')">+</button>
            <button onclick="appendToDisplay('1')">1</button>
            <button onclick="appendToDisplay('2')">2</button>
            <button onclick="appendToDisplay('3')">3</button>
            <button class="equals" onclick="calculateResult()">=</button>
            <button onclick="appendToDisplay('0')">0</button>
            <button onclick="appendToDisplay('.')">.</button>
        </div>
    </div>

    <script>
        const display = document.getElementById('display');

        function appendToDisplay(value) {
            display.value += value;
        }

        function clearDisplay() {
            display.value = '';
        }

        async function calculateResult() {
            const expression = display.value;
            if (!expression) return;
            
            try {
                // We'll use a simple client-side eval for speed, 
                // but the backend logic is there if you want to extend it.
                display.value = eval(expression);
            } catch (e) {
                display.value = 'Error';
            }
        }
    </script>
</body>
</html>
"""

@app.route('/')
def index():
    return render_template_string(HTML_TEMPLATE)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
import datetime
import os
from flask import Flask, jsonify, request

app = Flask(__name__)

LOG_FILE = "activity_log.txt"

def log_activity(event):
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    with open(LOG_FILE, "a") as f:
        f.write(f"[{timestamp}] {event}\n")

@app.route('/')
def index():
    log_activity(f"Viewed index page from {request.remote_addr}")
    return "<h1>Activity Logger</h1><p>The system is tracking access.</p>"
    
