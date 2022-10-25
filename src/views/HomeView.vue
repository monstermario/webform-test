<template>
  <div class="home">
    <WebForm @submit="handleSubmit" :fields="fields" icon="mdi-light:home">
    </WebForm>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import WebForm from "@/components/WebForm.vue";

export default defineComponent({
  name: "HomeView",
  components: {
    WebForm,
  },
  data() {
    return {
      fields: [
        {
          type: "text",
          value: "",
          name: "Text1",
          label: "Text1",
          required: true,
          placeholder: "Input text ...",
        },
        {
          type: "text",
          value: "Disabled text",
          name: "Text2",
          label: "Text2",
          disabled: true,
          placeholder: "Input text ...",
        },
        {
          type: "check",
          value: [],
          name: "Checkbox1",
          label: "Checkbox1",
          required: true,
          options: {
            Check1: "c1",
            Check2: "c2",
            Check3: "c3",
          },
        },
        {
          type: "radio",
          value: ["Radio1"],
          name: "Radiobox1",
          label: "Radiobox1",
          required: true,
          options: {
            Radio1: "r1",
            Radio2: "r2",
            Radio3: "r3",
          },
        },
        {
          type: "select",
          value: "",
          name: "Select1",
          label: "Select1",
          required: true,
          options: {
            Select1: "s1",
            Select2: "s2",
            Select3: "s3",
          },
        },
      ],
    };
  },
  methods: {
    checkValidate() {
      let passed = true;
      this.fields = this.fields.map((field) => {
        if (field.required && field.value.length === 0) {
          passed = false;
          return { ...field, error: "This field is required." };
        } else return { ...field, error: "" };
      });
      return passed;
    },
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    handleSubmit(e: any) {
      if (e?.type === "submit") return;
      if (this.checkValidate()) {
        this.fields.forEach((field) => {
          console.log(field.name, ": ", field.value);
        });
      }
    },
  },
});
</script>
