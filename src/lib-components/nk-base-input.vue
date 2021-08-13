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
    <b-form-input
      v-model="value"
      v-mask="mask"
      v-bind="$attrs"
      class="base-input"
      :class="[{'is-invalid': errors.length},inputClass]"
      debounce="500"
      :autocomplete="$attrs['type']==='password'?'on':'off'"
      v-on="$listeners"
    />
  </BaseValidateProvider>
</template>

<script>
  import BaseValidateProvider from "@/lib-components/nk-base-validate-provider";
  export default {
    name: "NkBaseInput",
    components: { BaseValidateProvider },
    inheritAttrs: false,
    model: {
      prop: "model"
    },
    props: {
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
      inputClass: {
        type: String,
        default: ""
      },
      label: {
        type: String,
        default: ""
      },
      mask: {
        type: String,
        default: ""
      },
      id: {
        type: String,
        default: ""
      },
      fieldName: {
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