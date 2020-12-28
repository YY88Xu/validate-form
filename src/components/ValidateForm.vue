<template>
  <form class="validate-form-container">
    <slot name="default"></slot>
    <div class="submit-area" @click.prevent="submitForm">
      <slot name="submit">
        <button type="submit" class="btn btn-primary">提交</button>
      </slot>
    </div>
  </form>
</template>
<script lang="ts">
import { defineComponent, onUnmounted } from 'vue'
import mitt from 'mitt'
export const emitter = mitt()
type ValidateFunc = () => boolean;

export default defineComponent({
    emits: ['submit'],
    setup (props, context) {
        let funcArr: ValidateFunc[] = []
        const submitForm = function (): void{
            const result = funcArr.map(func => func()).every(result => result)
            context.emit('submit', result)
        }

        const callback = function (fn?: ValidateFunc): void{
            if (fn) {
                funcArr.push(fn)
            }
        }

        emitter.on('form-validate', callback)

        onUnmounted(() => {
            emitter.off('form-validate', callback)
            funcArr = []
        })

        return {
            submitForm
        }
    }
})
</script>
