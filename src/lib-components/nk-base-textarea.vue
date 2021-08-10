<template>
  <BaseValidateProvider
    :id="id"
    v-slot="{ errors }"
    :name="fieldName"
    :field-set-class="fieldSetClass"
    :field-set-error-class="fieldSetErrorClass"
    :rules="rules"
    :label="label"
  >
    <b-form-textarea
      :id="id"
      v-model="value"
      :class="`form-control ${errors.length?'is-invalid':''}`"
      :state="state"
      v-bind="$attrs"
      :value="model"
      @input="$emit('input',$event)"
      @change="$emit('change',$event)"
    />
  </BaseValidateProvider>
</template>

<script>
  import BaseValidateProvider from "@/lib-components/nk-base-validate-provider";

  export default {
    name: "NkBaseTextarea",
    components: { BaseValidateProvider },
    inheritAttrs: false,
    model: {
      prop: "model"
    },
    props: {
      state: {
        validator: v => typeof v === "boolean" || v === null,
        default: null
      },
      fieldName: {
        type: String,
        default: ""
      },
      rules: {
        type: String,
        default: null
      },
      fieldSetClass: {
        type: String,
        default: ""
      },
      fieldSetErrorClass: {
        type: String,
        default: ""
      },
      label: {
        type: String,
        default: ""
      },
      id: {
        type: String,
        default: ""
      },
      model: {
        type: [String, Number],
        default: ""
      }
    },
    data() {
      return {
        value: ""
      };
    },
    watch: {
      model: {
        immediate: true,
        handler(value) {
          this.value = value;
        }
      }
    }
  };
</script>
