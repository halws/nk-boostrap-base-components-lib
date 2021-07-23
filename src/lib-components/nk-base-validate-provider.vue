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
      <label
        v-if="label"
        :for="id"
      >{{ label }}</label>
      <slot :errors="errors"></slot>

      <div class="text-danger">
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
Object.keys(rules).forEach((rule) => {
  extend(rule, rules[rule]);
});

localize("pl", pl);

export default {
  name: "nk-base-validate-provider",
  components: {
    ValidationProvider,
  },
  props: {
    rules: {
      type: String,
      default: null,
    },
    label: {
      type: String,
      default: "",
    },
    fieldSetClass: {
      type: String,
      default: "",
    },
    id: {
      type: String,
      default: "",
    },
    name: {
      type: String,
      default: "",
    },
  },
};
</script>

<style>
</style>