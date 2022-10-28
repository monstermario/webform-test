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
          type: "group",
          label: "Group",
          name: "Group",
          fields: [
            {
              type: "text",
              value: "",
              name: "Multi-Text",
              label: "Multi-Text",
              required: true,
              multiSelect: true,
              placeholder: "Input text ...",
            },
            {
              type: "select",
              value: "",
              name: "Multi-Select",
              label: "Multi-Select",
              required: true,
              multiSelect: true,
              options: ["ms1", "ms2", "ms3"],
            },
          ],
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
          options: ["s1", "s2", "s3"],
        },
      ],
    };
  },
  methods: {
    checkValidate(fields: any) {
      let passed = true;
      const newfields = fields.map((field: any) => {
        if (field.type === "group") {
          [passed, field.fields] = this.checkValidate(field.fields);
          console.log(field.fields);
        }
        if (field.required && field.value.length === 0) {
          passed = false;
          return { ...field, error: "This field is required." };
        } else return { ...field, error: "" };
      });
      this.fields = newfields;
      return [passed, newfields];
    },
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    handleSubmit(e: any) {
      if (e?.type === "submit") return;
      const [passed, newFileds] = this.checkValidate(this.fields);
      console.log(passed);
      if (passed) {
        this.fields = newFileds;
        this.fields.forEach((field) => {
          console.log(field.name, ": ", field.value);
        });
      }
    },
  },
});
</script>
