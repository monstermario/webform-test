<template>
  <div class="form-input form-select" :class="containerClass">
    <label
      :for="name"
      class="input-label"
      :title="required ? 'Required' : 'Optional'"
    >
      <span class="input-label-text">{{ label }}</span>

      <span v-if="required" class="input-required-mark"> *</span>
    </label>

    <div class="input-area">
      <slot v-if="$slots.input" name="input" />
      <select
        v-else
        v-bind="$attrs"
        :value="value"
        :name="name"
        :placeholder="placeholder"
        :autocomplete="autocomplete"
        @input="updateInput"
      >
        <option
          v-for="(key, option) of options"
          :key="`select-${key}`"
          :value="option"
        >
          {{ option }}
        </option>
      </select>
      <slot name="input-extra" />
    </div>
    <div class="input-error">{{ error }}</div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "SelectBox",
  props: {
    autocomplete: String,
    containerClass: String,
    required: {
      type: Boolean,
      default: false,
    },
    placeholder: String,
    label: String,
    name: String,
    value: {
      type: String,
      default: "",
    },
    options: {
      type: Object,
      default: () => {
        return {};
      },
    },
    error: {
      type: String,
      default: "",
    },
  },
  computed: {
    pairedOptions() {
      return Object.entries(this.options);
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
<style lang="css" scoped>
select {
  appearance: none;
  background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
  background-repeat: no-repeat;
  background-position: right 15px center;
  background-size: 15px;
}
</style>
