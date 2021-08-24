<template>
  <ValidationProvider
    v-slot="{ errors }"
    :vid="id"
    :rules="rules"
    :name="name"
  >
    <b-form-group
      class="position-relative"
      :class="fieldSetClass"
    >
      <component
        :is="'readonly' in $attrs?'small':'label'"
        v-if="label"
        :class="'readonly' in $attrs && 'text-black-50'"
        :for="id"
      >
        {{ label }}
      </component>
      <slot :errors="errors" />

      <div
        class="text-danger"
        :class="fieldSetErrorClass"
      >
        {{ errors[0] }}
      </div>
    </b-form-group>
  </ValidationProvider>
</template>

<script>
  import { ValidationProvider, extend, localize } from "vee-validate";
  import pl from "vee-validate/dist/locale/pl.json";
  import * as rules from "vee-validate/dist/rules";

  // install rules and localization
  Object.keys(rules).forEach(rule => {
    extend(rule, rules[rule]);
  });

  localize("pl", pl);

  export default {
    name: "NkBaseValidateProvider",
    components: {
      ValidationProvider
    },
    props: {
      rules: {
        type: String,
        default: null
      },
      label: {
        type: String,
        default: ""
      },
      fieldSetClass: {
        type: String,
        default: ""
      },
      fieldSetErrorClass: {
        type: String,
        default: ""
      },
      id: {
        type: String,
        default: ""
      },
      name: {
        type: String,
        default: ""
      }
    }
  };
</script>

<style>
</style>