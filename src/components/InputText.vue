<template>
  <div class="form-input" :class="containerClass">
    <label
      v-if="label"
      :for="name"
      class="input-label"
      :title="required ? 'Required' : 'Optional'"
    >
      <span class="input-label-text">{{ label }}</span>

      <span v-if="required" class="input-required-mark"> *</span>
    </label>
    <div class="input-area">
      <input
        v-bind="$attrs"
        :value="value"
        :id="name"
        :name="name"
        :placeholder="placeholder"
        type="text"
        :readonly="readonly"
        @input="updateInput"
      />
    </div>
    <div class="input-error">{{ required ? error : "" }}</div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "InputText",
  props: {
    label: String,
    name: String,
    value: {
      type: String,
      default: "",
    },
    required: {
      type: Boolean,
      default: false,
    },
    readonly: {
      type: Boolean,
      default: false,
    },
    containerClass: String,
    placeholder: String,
    error: {
      type: String,
      default: "",
    },
  },
  methods: {
    updateInput(event: Event) {
      this.$emit("update:modelValue", (event.target as HTMLInputElement).value);
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css" scoped></style>
