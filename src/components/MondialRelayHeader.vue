
<template>
  <div class="mondial-relay-header">
    <div class="mondial-relay-header__title">{{ headerTitle }}</div>
    <div class="mondial-relay-header__row">
      <div
        class="mondial-relay-header__column mondial-relay-header__column--left"
      >
        <img
          src="https://widget.mondialrelay.com/parcelshop-picker/v4_0/css/imgs/mr-64.png"
          alt="mondial relay"
          class="mondial-relay-header__logo"
        />
      </div>
      <div
        class="mondial-relay-header__column mondial-relay-header__column--right"
      >
        <slot name="formSearchCp"></slot>
      </div>
    </div>
  </div>
</template>


<script>
import { defineComponent } from "vue";
import './../assets/scss/header.scss'
export default defineComponent({
  name: "MondialRelayHeader",
  props: ["headerTitle"],
});
</script>