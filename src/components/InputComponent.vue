<template>
  <input
    class="inputField"
    type="text"
    :placeholder="text"
    :value="valueName"
    @change="onChange($event)"
  />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'InputComponent',
  props: {
    valueName: {
      type: String,
      required: true,
    },
    text: {
      type: String,
    },
  },
  emits: ['inputChange'],
  setup(props, context) {
    function onChange(inputValue: { target: { value: string } }) {
      context.emit('inputChange', inputValue.target.value);
    }

    return { onChange };
  },
});
</script>

<style lang="scss" scoped>
.inputField {
  margin: 5px;
  display: block;
}
</style>
