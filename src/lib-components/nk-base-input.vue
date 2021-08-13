<template>
  <BaseValidateProvider
    :id="id"
    v-slot="{ errors }"
    :name="fieldName"
    :field-set-class="fieldSetClass"
    :field-set-error-class="fieldSetErrorClass"
    :rules="rules"
    :readonly="readonly"
    :label="label"
  >
    <b-form-input
      :id="id"
      ref="input"
      :key="key"
      v-model="value"
      v-mask="mask"
      class="base-input"
      :name="name"
      :state="state"
      :readonly="readonly"
      :size="size"
      :type="$attrs['type']"
      :class="[{'is-invalid': errors.length,'pt-0 pl-0':readonly},inputClass]"
      v-bind="$attrs"
      :value="model"
      :debounce="300"
      :formatter="formatter"
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
      state: {
        validator: v => typeof v === "boolean" || v === null,
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
      size: {
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
      name: {
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
      },
      readonly: {
        type: Boolean,
        default: false
      },
      formatter: {
        type: Function,
        default: val => val
      }
    },
    data() {
      return {
        value: "",
        key: 1
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
      }
    },
    watch: {
      model: {
        immediate: true,
        handler(value) {
          this.value = value;
        }
      }
    },
    methods: {
      onEventChange(e) {
        if (this.isNumber) {
          this.validateNumber(e)
            .then(number => {
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
      }
    }
  };
</script>