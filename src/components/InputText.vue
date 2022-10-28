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
        v-if="!multiSelect"
        v-bind="$attrs"
        :value="value"
        :id="name"
        :name="name"
        :placeholder="placeholder"
        type="text"
        :readonly="readonly"
        @input="updateInput"
      />
      <div v-else class="input-multi">
        <span v-for="tag in (value as string[])" :key="tag" class="input-tag">
          {{ tag }}
          <b class="input-tag-icon" @click="removeTag(tag)"> X </b>
        </span>

        <input
          class="input-el"
          placeholder=""
          :maxLength="20"
          :contentEditable="true"
          @keydown="inputTags"
        />
      </div>
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
      type: [String, Array],
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
    multiSelect: {
      type: Boolean,
      default: false,
    },
    error: {
      type: String,
      default: "",
    },
  },
  methods: {
    updateInput(event: Event) {
      this.$emit("update:modelValue", (event.target as HTMLInputElement).value);
    },
    inputTags(event: any) {
      const code = event.code;
      if (!["Comma", "Enter", "Backspace"].includes(code)) return;
      const target = event.target;
      const tagName = target.value;
      if (!tagName && tagName === ",") {
        target.value = "";
        return;
      }
      if (code !== "Backspace" && this.value.includes(tagName)) {
        event.preventDefault();
        return;
      }
      const newTags = this.value;
      if (!tagName) {
        target.value = "";
        if (code === "Backspace") {
          this.$emit(
            "update:modelValue",
            this.value.slice(0, this.value.length - 1)
          );
        }
        return;
      } else if (code === "Backspace") return;
      event.preventDefault();
      this.$emit("update:modelValue", [...newTags, tagName]);
      target.value = "";
    },
    removeTag(tag: string) {
      this.$emit(
        "update:modelValue",
        (this.value as string[]).filter((x: string) => x !== tag)
      );
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css" scoped>
.input-el {
  border-radius: 13px;
  padding: 0px;
  width: -webkit-fill-available;
  outline: none;
  background: transparent;
  border: none;
}
.input-multi {
  width: 100%;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.input-tag {
  display: flex;
  align-items: center;
  background: #ddd;
  padding: 1px 3px;
  border-radius: 4px;
  margin-right: 5px;
}
.input-tag-icon {
  font-size: 8px;
  margin-left: 5px;
  cursor: pointer;
}
</style>
