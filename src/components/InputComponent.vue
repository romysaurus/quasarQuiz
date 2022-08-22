<template>
  <input
    class="input-field"
    type="text"
    required
    :placeholder="text"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'InputComponent',
  props: {
    modelValue: {
      type: String,
      required: true,
    },
    text: {
      type: String,
    },
  },

  emits: ['update:modelValue'],
});
</script>

<style lang="scss" scoped>
.input-field {
  margin: 5px;
  display: block;
  border: 0.05rem solid #ff8c00;
  color: deeppink;
  width: 18rem;
  height: 2rem;

  &::placeholder {
    color: deeppink;
    text-align: center;
  }

  &:focus {
    outline: none !important;
    border: 0.1rem solid #ff8c00;
    box-shadow: 0 0 0.3rem #ff1493;
  }
}
</style>
