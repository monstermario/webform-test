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
      <div class="select-dropdown" :class="openDropdown ? 'open' : ''">
        <div class="select-text" @click="toggleDropdown">
          {{ displayValue }}
        </div>
        <div
          class="select-backdrop"
          v-if="openDropdown"
          @click="toggleDropdown"
        />
        <ul class="select-list" v-if="openDropdown">
          <li
            v-for="(option, index) in (options as string[])"
            :key="`select-${name}-${index}`"
            :class="
              multiSelect && value.includes(option)
                ? 'select-item-selected'
                : ''
            "
            @click="selectOption(option)"
          >
            {{ option }}
          </li>
        </ul>
      </div>
      <!-- <select
        v-else
        v-bind="$attrs"
        :value="value"
        :name="name"
        :multiple="multiSelect"
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
      <slot name="input-extra" /> -->
    </div>
    <div class="input-error">{{ required ? error : "" }}</div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "SelectBox",
  data() {
    return {
      openDropdown: false,
    };
  },
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
      type: [String, Array],
      default: "",
    },
    options: {
      type: Array,
      default: () => {
        return [];
      },
    },
    multiSelect: {
      type: Boolean,
      default: false,
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
    displayValue() {
      return Array.isArray(this.value) ? this.value.join(", ") : this.value;
    },
  },
  methods: {
    updateInput(event: Event) {
      this.$emit("update:modelValue", (event.target as HTMLInputElement).value);
    },
    selectOption(option: string) {
      this.$emit(
        "update:modelValue",
        this.multiSelect
          ? this.value.includes(option)
            ? (this.value as string[]).filter((item) => item !== option)
            : [...this.value, option]
          : option
      );
      if (!this.multiSelect) this.toggleDropdown();
    },
    toggleDropdown() {
      this.openDropdown = !this.openDropdown;
    },
  },
});
</script>
<style src="@vueform/multiselect/themes/default.css"></style>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css" scoped>
.select-dropdown {
  position: relative;
  width: 100%;
  display: flex;
  cursor: pointer;
  background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
  background-repeat: no-repeat;
  background-position: right 15px center;
  background-size: 15px;
}
.select-dropdown.open {
  z-index: 9999;
}
.select-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
}
.select-text {
  display: flex;
  align-items: center;
}
.select-dropdown ul {
  position: absolute;
  top: 100%;
  margin: 0;
  padding: 0;
  border: 1px solid #aaa;
  background-color: white;
  left: 0%;
  right: 0;
  border-radius: 6px;
}
.select-dropdown ul li {
  text-align: left;
  padding: 5px 15px;
}
.select-dropdown ul li:hover {
  background: #a8dfff;
}
.select-dropdown ul li.select-item-selected {
  background: #d6f0ff;
}
.select-dropdown ul li.select-item-selected:hover {
  background: #ec6bcd;
}
</style>
