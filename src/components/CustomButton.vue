<template>
    <div style="position:relative; margin:8px">
        <div class="buttonhole"></div>
        <button @click="doAction" :class="{ active: active }">{{ label }}</button>
    </div>
</template>

<script setup>
import { watch } from "vue";


const props = defineProps({
    label: {
        type: String
    },
    active: {
        type: Boolean,
        default: false
    },
    hover: {
        type: Boolean,
        default: false
    },
    action: {

    },
    state: {
        type: Object
    }
})

function calculate() {
    switch (props.state.operation) {
        case '+':
            return parseFloat(props.state.previousOperand) + parseFloat(props.state.currentOperand)
        case '-':
            return parseFloat(props.state.previousOperand) - parseFloat(props.state.currentOperand)
        case '÷':
            return parseFloat(props.state.previousOperand) / parseFloat(props.state.currentOperand)
        case 'x':
            return parseFloat(props.state.previousOperand) * parseFloat(props.state.currentOperand)
        default:
            return '';
    }
}

function doAction(event) {
    if (Array.from('0123456789').includes(props.label)) {
        if (props.state.currentOperand === "0") {
            props.state.currentOperand = props.label;
            props.state.calculated = false;
            return
        }
        if (props.state.calculated) {
            props.state.currentOperand = props.label;
            props.state.calculated = false;
        } else {
            props.state.currentOperand += props.label
        }
    } else if (['NumpadDivide', 'NumpadAdd', 'NumpadMultiply', 'NumpadSubtract'].includes(props.action)) {
        if (props.state.previousOperand !== '' && props.state.currentOperand === '') {
            props.state.operation = props.label;
        } else if (props.state.previousOperand === '' && props.state.currentOperand !== '') {
            props.state.operation = props.label;
            props.state.previousOperand = props.state.currentOperand;
            props.state.currentOperand = '';
        } else if (props.state.currentOperand === '' && props.state.previousOperand === '' && props.action == 'NumpadSubtract') {
            props.state.previousOperand = '0';
            props.state.operation = props.label;
            props.state.currentOperand = ''
        } else if (props.state.currentOperand !== '' && props.state.previousOperand !== '') {
            props.state.previousOperand = calculate();
            props.state.currentOperand = '';
            props.state.operation = props.label;
        }
    } else if (props.action === 'Escape') {
        props.state.currentOperand = '';
        props.state.previousOperand = '';
        props.state.operation = '';
    } else if (props.action === 'KeyN') {
        if (props.state.currentOperand !== '') {
            props.state.currentOperand = String(-parseFloat(props.state.currentOperand));
        }
    } else if (props.action === 'NumpadDecimal' && props.state.currentOperand !=='') {
        if (!props.state.currentOperand.includes('.')) {
            props.state.currentOperand += '.';
        }
    } else if (props.action === 'Backspace') {
        if (props.state.calculated) {
            props.state.currentOperand = '';
            props.state.previousOperand = '';
            props.state.operation = '';
            props.state.calculated = false;
        } else {
            props.state.currentOperand = props.state.currentOperand.substring(0, props.state.currentOperand.length - 1)
        }
    } else if (props.action === 'NumpadEnter') {
        if (props.state.previousOperand !== '' && props.state.currentOperand !== '') {
            props.state.currentOperand = String(calculate());
            props.state.previousOperand = '';
            props.state.operation = '';
            props.state.calculated = true;
        }
    }
}

watch(() => props.active, () => { if (!props.active) { doAction() } })

</script>

<style lang="scss">
.buttonhole {
    top: 6px;
    bottom: -10px;
    right: -2px;
    left: -2px;
    border-radius: 10px;
    position: absolute;
    background: black;
}

.buttonhole ~ button {
    -webkit-user-select: none; /* Safari */
    -moz-user-select: none; /* Firefox */
    -ms-user-select: none; /* IE10+/Edge */
    user-select: none; /* Standard */
    -webkit-tap-highlight-color: transparent;
    position: absolute;
    color: #333;
    inset: 0;
    width: 100%;
    height: 100%;
    cursor: pointer;
    outline: none;
    border-radius: 0.5rem;
    border-width: 0;
    background: linear-gradient(#efefef, #efefef);
    box-shadow: 0px 8px #999999, 0 0 4px #00000010, inset 0 0 20px 0px #00000010;
    font-size: 1.5rem;
    z-index: 2 !important;
    &:hover {
        background: linear-gradient(#e5e5e5, #efefef);
        margin-top: 2px;
        box-shadow: 0px 6px #999999, 0 0 4px #00000010,
            inset 0 0 20px 0px #00000010;
    }
    &:active,
    &.active {
        background: linear-gradient(#dfdfdf, #dfdfdf);
        margin-top: 8px;
        margin-bottom: 0;
        box-shadow: 0px 0px #999999, 0 0 0px #00000010,
            inset 0 0 20px 0px #00000010;
    }
    transition: all 0.1s ease-in-out;
}
</style>