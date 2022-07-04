
<template>
  <div class="mondial-relay-alert mondial-relay-alert--info">
    {{message}}
  </div>
</template>


<script>
import { defineComponent } from "vue";
import './../assets/scss/error-message.scss'
export default defineComponent({
  name: "MondialRelayErrorMessage",
  props: ['message']
});
</script>