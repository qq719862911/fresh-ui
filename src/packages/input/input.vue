<template>
    <div :class="['d-input', , this.icon ? 'has-icon' : '']">
        <template v-if="type !== 'textarea'">
            <input :class="['d-input__inner', this.size ? 'd-input__inner--' + this.size : '']" :value="currentValue" :placeholder="placeholder" :autocomplete="autocomplete" :disabled="disabled" :readonly="readonly" @input="handleInputChange" @keyup.enter="handleInputKeyUp" :type="type">
            <d-icon class="d-input__icon" v-if="this.icon" :name="icon" @click="handleIconClick"></d-icon>
        </template>
        <template v-else>
            <textarea :class="['d-textarea__inner', autoSize ? 'auto-size' : '']" :style="{'resize':resize}" :row="row" :value="currentValue" :placeholder="placeholder" :autocomplete="autocomplete" :disabled="disabled" @input="handleInputChange" @keyup.enter="handleInputKeyUp"></textarea>
        </template>
    </div>
</template>
<script>
import DIcon from '../icon/icon';
export default {
    name: 'DInput',
    components: { DIcon },
    props: {
        disabled: {
            type: Boolean,
            default: false,
        },
        readonly: {
            type: Boolean,
            default: false,
        },
        type: {
            type: String,
            default: 'text'
        },
        value: [String, Number],
        size: {
            type: String,
            default: ''
        },
        placeholder: {
            type: String,
            default: ''
        },
        icon: {
            type: String,
            default: ''
        },
        row: {
            type: Number,
            default: 2
        },
        resize: {
            type: String,
            default: ''
        },
        autocomplete: {
            type: Boolean,
            default: false
        },
        autoSize: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            currentValue: this.value,
        }
    },
    watch: {
        value(val) {
            this.setCurrentValue(val);
        }
    },
    methods: {
        handleInputChange(e) {
            if (this.type === 'textarea' && this.autoSize) {
                this.resizeHeight(e);
            }
            const value = e.target.value;
            this.$emit('input', value, e);
            if (this.currentValue === value) {
                return;
            }
            this.setCurrentValue(value);
            this.$emit('change', value, e);
        },
        handleInputKeyUp(e) {
            this.$emit('enter', e.target.value, e);
        },
        setCurrentValue(val) {
            this.currentValue = val;
        },
        resizeHeight(e) {
            const border = 1;
            e.target.style.height = 'auto';
            e.target.style.height = e.target.scrollHeight + 2 * border + 'px';
        },
        handleIconClick() {
            this.$emit('icon-click', this);
        }
    }
}
</script>