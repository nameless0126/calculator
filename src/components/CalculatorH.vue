<template>
    <div class="calculator">
        <input type="text" v-model="display" readonly />
        <div class="buttons">
            <button v-for="(button, index) in buttons" :key="index" @click="appendValue(button.value)">{{ button.label }}</button>
            <button @click="clearDisplay">C</button>
        </div>
    </div>
</template>

<script>
// 用于计算四则运算表达式的函数
function calculateExpression(expression) {
    const operators = ['+', '-', '*', '/'];
    const numbers = [];
    const operatorStack = [];
    let currentNumber = '';
    for (let char of expression) {
        if (operators.includes(char)) {
            while (operatorStack.length > 0 && operators.indexOf(operatorStack[operatorStack.length - 1]) >= operators.indexOf(char)) {
                const operator = operatorStack.pop();
                const num2 = numbers.pop();
                const num1 = numbers.pop();
                let result;
                switch (operator) {
                    case '+':
                        result = num1 + num2;
                        break;
                    case '-':
                        result = num1 - num2;
                        break;
                    case '*':
                        result = num1 * num2;
                        break;
                    case '/':
                        result = num1 / num2;
                        break;
                }
                numbers.push(result);
            }
            operatorStack.push(char);
            numbers.push(Number(currentNumber));
            currentNumber = '';
        } else {
            currentNumber += char;
        }
    }
    numbers.push(Number(currentNumber));
    while (operatorStack.length > 0) {
        const operator = operatorStack.pop();
        const num2 = numbers.pop();
        const num1 = numbers.pop();
        let result;
        switch (operator) {
            case '+':
                result = num1 + num2;
                break;
            case '-':
                result = num1 - num2;
                break;
            case '*':
                result = num1 * num2;
                break;
            case '/':
                result = num1 / num2;
                break;
        }
        numbers.push(result);
    }
    return numbers.pop();
}

export default {
    name: "CalculatorH",
    data() {
        return {
            display: "",
            buttons: [
                { label: "7", value: "7" },
                { label: "8", value: "8" },
                { label: "9", value: "9" },
                { label: "/", value: "/" },
                { label: "4", value: "4" },
                { label: "5", value: "5" },
                { label: "6", value: "6" },
                { label: "*", value: "*" },
                { label: "1", value: "1" },
                { label: "2", value: "2" },
                { label: "3", value: "3" },
                { label: "-", value: "-" },
                { label: "0", value: "0" },
                { label: ".", value: "." },
                { label: "=", value: "=" },
                { label: "+", value: "+" }
            ],
            expression: ""
        };
    },
    methods: {
        appendValue(value) {
            if (value === "=") {
                this.expression += this.display;
                this.display = calculateExpression(this.expression);
                this.expression = "";
            } else if (value === "C") {
                this.clearDisplay();
            } else {
                this.display += value;
            }
        },
        calculate() {
            // 这里可以保留，如果后续有其他地方需要主动触发计算逻辑可以使用这个方法
            // 当前主要计算逻辑放在appendValue里点击“=”时执行了
        },
        clearDisplay() {
            this.display = "";
        }
    }
};
</script>

<style scoped>
.calculator {
    width: 300px;
    height: 400px;
    margin: 10vh auto;
    background-color: rgba(255, 255, 255, 0.8);
    padding: 20px;
    border-radius: 5px;
}

input {
    box-sizing: border-box;
    width: 100%;
    margin-bottom: 10px;
    padding: 10px;
    font-size: 20px;
}

.buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 10px;
}

button {
    padding: 10px;
    font-size: 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    background-color: #f0f0f0;
}

button:hover {
    background-color: #e0e0e0;
}
</style>