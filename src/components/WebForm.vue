<template>
  <form @submit.prevent.stop="$emit('submit')">
    <h1><slot name="formTitle">Form Title</slot></h1>
    <template v-for="field in fields" :key="`input-form-${field.name}`">
      <SubForm
        v-if="field.type === 'group' && field.fields"
        :group-name="field.name"
        :group-label="field.label"
        :fields="field.fields"
      />
      <InputText
        v-if="field.type === 'text'"
        v-model="field.value"
        :name="field.name"
        :label="field.label"
        :value="field.value"
        :required="field.required"
        :readonly="field.readonly"
        :disabled="field.disabled"
        :containerClass="field.containerClass"
        :placeholder="field.placeholder"
        :multiSelect="field.multiSelect"
        :error="field.error"
      />
      <RadioCheckBox
        v-else-if="field.type === 'check'"
        v-model="field.value"
        :value="field.value"
        type="checkbox"
        :name="field.name"
        :label="field.label"
        :required="field.required"
        :disabled="field.disabled"
        :options="field.options"
        :containerClass="field.containerClass"
        :error="field.error"
      />
      <RadioCheckBox
        v-else-if="field.type === 'radio'"
        v-model="field.value"
        :value="field.value"
        type="radio"
        :name="field.name"
        :label="field.label"
        :required="field.required"
        :disabled="field.disabled"
        :options="field.options"
        :containerClass="field.containerClass"
        :error="field.error"
      />
      <SelectBox
        v-else-if="field.type === 'select'"
        v-model="field.value"
        :value="field.value"
        :name="field.name"
        :label="field.label"
        :required="field.required"
        :disabled="field.disabled"
        :placeholder="field.placeholder"
        :options="Array.isArray(field.options) ? field.options : []"
        :containerClass="field.containerClass"
        :multiSelect="field.multiSelect"
        :error="field.error"
      />
    </template>
    <slot></slot>
    <div class="submit-btn" :class="submitClass">
      <button type="submit">
        <Icon :icon="icon" :inline="true" v-if="icon" /><span>Submit</span>
      </button>
    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent, type PropType } from "vue";
import { Icon } from "@iconify/vue";
import RadioCheckBox from "./RadioCheckBox.vue";
import SelectBox from "./SelectBox.vue";
import InputText from "./InputText.vue";
import SubForm from "./SubForm.vue";

export type Field = {
  type: string;
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  value?: any;
  name: string;
  label: string;
  required?: boolean;
  placeholder?: string;
  disabled?: boolean;
  readonly?: boolean;
  options?: object | string[];
  containerClass?: string;
  multiSelect?: boolean;
  error?: string;
  fields?: Field[];
};

export default defineComponent({
  name: "WebForm",
  props: {
    submitClass: String,
    icon: {
      type: String,
      default: "",
    },
    fields: {
      type: Array as PropType<Field[]>,
      required: true,
    },
  },
  components: {
    Icon,
    RadioCheckBox,
    SelectBox,
    InputText,
    SubForm,
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css" scoped>
.submit-btn {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}
.submit-btn button {
  cursor: pointer;
  padding: 10px 20px;
  border-radius: 6px;
  border: none;
  background: #0079bf;
  color: white;
  transition: all ease 0.3s;
  display: flex;
  align-items: center;
  line-height: 20px;
}

.submit-btn button svg {
  width: 20px;
  height: 20px;
  margin-right: 2px;
}
.submit-btn button:hover {
  background: #026aa7;
}
</style>
