<template>
    <div class="validate-input-container pb-3">
        <input type="text"
        class="form-control" :class="{'is-invalid': inputRef.error}"
        :value="inputRef.val" @input="updateValue" @blur="validateInput"
        v-bind="$attrs">
        <span v-if="inputRef.error" class="invalid-feedback">{{inputRef.message}}</span>
    </div>
</template>
<script lang="ts">
import { emitter } from './ValidateForm.vue'
import { defineComponent, reactive, PropType, onMounted, watch } from 'vue'
const emailReg = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/

interface RuleProp{
    type: 'required' | 'email' | 'custom';
    message: string;
    validator?: () => boolean;
}
export type RulesProp = RuleProp[];
export default defineComponent({
    props: {
        modelValue: String,
        rules: Array as PropType<RulesProp>
    },
    inheritAttrs: false,
    setup (props, context) {
        const inputRef = reactive({
            val: props.modelValue || '',
            error: false,
            message: ''
        })
        const updateValue = (e: KeyboardEvent) => {
            const targetValue = (e.target as HTMLInputElement).value
            inputRef.val = targetValue
            context.emit('update:modelValue', targetValue)
        }
        watch(() => props.modelValue, (newVal) => {
            inputRef.val = newVal as string
        })
        const validateInput = () => {
            if (props.rules) {
                const allPassed: boolean = props.rules.every((rule: RuleProp) => {
                    let passed = true
                    inputRef.message = rule.message
                    switch (rule.type) {
                        case 'required':
                            passed = (inputRef.val.trim() !== '')
                            break
                        case 'email':
                            passed = emailReg.test(inputRef.val)
                            break
                        case 'custom':
                            passed = rule.validator ? rule.validator() : true
                            break
                        default:
                            break
                    }
                    return passed
                })
                inputRef.error = !allPassed
                return allPassed
            }
            return true
        }
        onMounted(() => {
            emitter.emit('form-validate', validateInput)
        })
        return {
            inputRef,
            updateValue,
            validateInput
        }
    }
})
</script>
