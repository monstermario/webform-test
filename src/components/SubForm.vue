<template>
  <div class="sub-form">
    <h2>
      <slot :name="groupName">{{ groupLabel }}</slot>
    </h2>
    <template v-for="field in fields" :key="`input-form-${field.name}`">
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
  </div>
</template>

<script lang="ts">
import { defineComponent, type PropType } from "vue";
import RadioCheckBox from "./RadioCheckBox.vue";
import SelectBox from "./SelectBox.vue";
import InputText from "./InputText.vue";
import { Field } from "./WebForm.vue";

export default defineComponent({
  name: "WebForm",
  props: {
    groupName: String,
    groupLabel: String,
    fields: {
      type: Array as PropType<Field[]>,
      required: true,
    },
  },
  components: {
    RadioCheckBox,
    SelectBox,
    InputText,
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css" scoped>
.sub-form {
  padding: 0 20px;
}
.sub-form h2 {
  text-align: left;
}
</style>
