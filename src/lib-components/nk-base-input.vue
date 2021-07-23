<template>
  <BaseValidateProvider
    :id="id"
    :name="fieldName"
    v-slot="{ errors }"
    :field-set-class="fieldSetClass"
    :rules="rules"
    :label="label"
  >
    <b-form-input
      class="base-input"
      ref="input"
      :key="key"
      :id="id"
      :name="name"
      v-mask="mask"
      v-model="value"
      :state="state"
      :type="$attrs['type']"
      :class="[{'is-invalid': errors.length},inputClass]"
      v-bind="$attrs"
      :value="model"
      :debounce="300"
      :formatter="formatter"
      :autocomplete="$attrs['type']==='password'?'on':'off'"
      @input="onEventChange($event)"
      @change="onEventChange($event)"
    />
  </BaseValidateProvider>
</template>

<script>
import BaseValidateProvider from "./nk-base-validate-provider.vue";
export default {
  name: "nk-base-input",
  components: { BaseValidateProvider },
  inheritAttrs: false,
  model: {
    prop: "model",
  },
  props: {
    rules: {
      type: String,
      default: null,
    },
    state: {
      validator: (v) => typeof v === "boolean" || v === null,
      default: null,
    },
    fieldSetClass: {
      type: String,
      default: "",
    },
    inputClass: {
      type: String,
      default: "",
    },
    label: {
      type: String,
      default: "",
    },
    mask: {
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
    fieldName: {
      type: String,
      default: "",
    },
    model: {
      type: [String, Number],
      default: "",
    },
    formatter: {
      type: Function,
      default: (val) => val,
    },
  },
  data() {
    return {
      value: "",
      key: 1,
    };
  },
  computed: {
    isNumber() {
      const { $attrs } = this;
      return (
        $attrs &&
        Object.keys($attrs).includes("type") &&
        $attrs.type === "number"
      );
    },
    hasMinValue() {
      const { $attrs, isNumber } = this;

      return isNumber && $attrs && Object.keys($attrs).includes("min");
    },
    minValue() {
      const { hasMinValue, $attrs } = this;

      return hasMinValue ? parseFloat($attrs["min"]) : null;
    },
    hasMaxValue() {
      const { $attrs, isNumber } = this;

      return isNumber && $attrs && Object.keys($attrs).includes("max");
    },
    maxValue() {
      const { hasMaxValue, $attrs } = this;

      return hasMaxValue ? parseFloat($attrs["max"]) : null;
    },
    hasMaxLength() {
      const { $attrs, isNumber } = this;

      return isNumber && $attrs && Object.keys($attrs).includes("maxlength");
    },
    maxLength() {
      const { hasMaxLength, $attrs } = this;

      return hasMaxLength ? Number($attrs["maxlength"]) : null;
    },
  },

  methods: {
    onEventChange(e) {
      if (this.isNumber) {
        this.validateNumber(e)
          .then((number) => {
            this.$emit("input", Number(number));
          })
          .catch(this.forceUpdate);
      } else {
        this.$emit("input", e);
      }
    },
    async forceUpdate(e) {
      this.$emit("input", Number(e));

      this.key = this.key + 1;
      await this.$nextTick();

      this.$refs.input.$el.select();
    },
    preventEmpty(e) {
      return new Promise((res, rej) => {
        e === "" || e == undefined ? rej(0) : res(Number(e));
      });
    },
    preventMin(e) {
      return new Promise((res, rej) => {
        this.hasMinValue && e < this.minValue ? rej(this.minValue) : res(e);
      });
    },
    preventMax(e) {
      return new Promise((res, rej) => {
        this.hasMaxValue && e > this.maxValue ? rej(this.maxValue) : res(e);
      });
    },
    preventMaxLength(e) {
      return new Promise((res, rej) => {
        this.hasMaxLength && String(e).length > this.maxLength
          ? rej(String(e).slice(0, this.maxLength))
          : res(e);
      });
    },

    validateNumber(number) {
      return this.preventMin(number)
        .then(this.preventMax)
        .then(this.preventMaxLength);
    },
  },
};
</script>