<template>
    <div>
        <div class="calculator">
            <div class="result">
                <div class="previous-operand">{{ state.previousOperand }} {{ state.operation }}</div>
                <div class="current-operand">{{ state.currentOperand }}</div>
            </div>
            <CustomButton
                v-for="btn, key in buttons"
                :key="key"
                :active="btn.active"
                :label="btn.label"
                :style="btn.style"
                v-model:state="state"
                :action="key"
            />
        </div>
    </div>
</template>

<script setup>

import CustomButton from './CustomButton.vue';
import { reactive, ref } from 'vue';

const state = reactive({
    currentOperand: "",
    previousOperand: "",
    operation: "",
    calculated: false
})

const buttons = ref({
    Numpad7: { active: false, label: '7' },
    Numpad8: { active: false, label: '8' },
    Numpad9: { active: false, label: '9' },
    Backspace: { active: false, label: "⌫" },
    Escape: { active: false, label: 'CE' },
    Numpad4: { active: false, label: '4' },
    Numpad5: { active: false, label: '5' },
    Numpad6: { active: false, label: '6' },
    NumpadMultiply: { active: false, label: 'x' },
    NumpadSubtract: { active: false, label: '-' },
    Numpad1: { active: false, label: '1' },
    Numpad2: { active: false, label: '2' },
    Numpad3: { active: false, label: '3' },
    NumpadAdd: { active: false, label: '+', style: "grid-row:span 2" },
    NumpadDivide: { active: false, label: '÷' },
    KeyN: { active: false, label: '+/-' },
    Numpad0: { active: false, label: '0' },
    NumpadDecimal: { active: false, label: '.' },
    NumpadEnter: { active: false, label: '=' },
})

window.addEventListener('keydown', (event) => {
    if (event.key == 'Enter') {
        event.preventDefault();
    }
    var button = buttons.value[event.code];
    if (button) {
        button.active = true;
    }
})
window.addEventListener('keyup', (event) => {
    var button = buttons.value[event.code];
    if (button) {
        button.active = false;
    }
})

</script>

<style lang="scss">
*,
*::before,
*::after {
    box-sizing: border-box;
}
body {
    margin: 0;
    background: black;
}

.calculator {
    display: inline-grid;
    grid-template-columns: repeat(5, minmax(40px, 70px));
    grid-template-rows: minmax(150px, auto) repeat(4, minmax(40px, 70px));
    justify-content: center;
    width: auto;
    background-color: rgb(214, 214, 214);
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0px 20px #999999, 0 0 20px -15px #000;
}
.result {
    grid-column: -1 / 1;
    background: rgba(0, 0, 0, 0.75);
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    justify-content: space-around;
    padding: 8px;
    margin: 8px;
    margin-top: 20px;
    margin-bottom: 20px;
    box-shadow: 0px -8px #999999;
    border-radius: 3px;
    font-family: Courier, "Courier New", monospace;
}
.previous-operand {
    color: rgba(255, 255, 255, 0.6);
    font-size: 16px;
    word-break: break-all;
    text-align: end;
}

.current-operand {
    color: white;
    font-size: 25px;
    word-break: break-all;
    text-align: end;
}
</style>