<script>
import Vue from "vue";
import { VueMaskDirective } from "v-mask";

// Uncomment import and local "components" registration if library is not registered globally.
import { NkBaseInput } from "@/entry.esm";
Vue.directive("mask", VueMaskDirective);

export default Vue.extend({
  name: "ServeDev",
  components: {
    NkBaseInput,
  },
  data() {
    return {
      model: "",
    };
  },
});
</script>

<template>
  <div id="app" class="mt-5">
    <b-container>
      <b-row align-v="center">
        <b-col cols="3"> <NkBaseInput label="test" v-model="model"/></b-col>
        <b-col cols="3">model: {{model}}</b-col>
      </b-row>
    </b-container>
  </div>
</template>
