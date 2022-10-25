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
    <div class="form-radio">
      <div
        v-for="[v, optionLabel] of optionsArray"
        :key="type + v + optionLabel"
        class="input-radio"
        :class="{ selected: isSelected(v) }"
        @click="() => selectOption(v)"
      >
        <slot
          name="option"
          :label="optionLabel"
          :value="value"
          :inputName="name"
        >
          <input
            :type="type"
            :value="v"
            :name="name"
            :checked="Array.isArray(value) ? value.includes(v) : value === v"
          />
          <div class="radio-mark" />
          <label :for="name" class="input-radio-label">{{ v }}</label>
        </slot>
      </div>
    </div>
    <div class="input-error">{{ error }}</div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "RadioCheckBox",
  props: {
    type: {
      type: String,
      default: "radio",
    },
    containerClass: String,
    required: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    label: String,
    name: String,
    options: {
      type: Object,
      default: () => {
        return {};
      },
    },
    value: {
      type: [String, Array],
      default: "",
    },
    error: {
      type: String,
      default: "",
    },
  },
  model: {
    prop: "value",
    event: "update",
  },
  computed: {
    optionsArray() {
      return Object.entries(this.options);
    },
  },
  methods: {
    isSelected(v: typeof this.options["val"]) {
      if (Array.isArray(this.value)) return this.value.includes(v);
      return v === this.value;
    },
    selectOption(v: typeof this.options["val"]) {
      if (this.$attrs.disabled) return;
      if (Array.isArray(this.value)) {
        if (this.value.includes(v))
          return this.$emit(
            "update:modelValue",
            this.value.filter((h) => v !== h)
          );
        else {
          return this.$emit("update:modelValue", [...this.value, v]);
        }
      } else if (v === this.value) {
        return this.$emit("update:modelValue", false);
      } else {
        return this.$emit("update:modelValue", v);
      }
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css" scoped>
.input-radio {
  display: flex;
  align-items: center;
}
.input-radio input {
  margin-top: 0;
}
.input-radio:not(:last-child) {
  margin-right: 10px;
}
</style>
