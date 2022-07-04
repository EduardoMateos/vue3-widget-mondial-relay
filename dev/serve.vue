<script>
import { defineComponent } from "vue";
import Vue3WidgetMondialRelay from "@/vue3-widget-mondial-relay.vue";

export default defineComponent({
  name: "ServeDev",
  components: {
    Vue3WidgetMondialRelay,
  },
  methods: {
    setParcelSelected(event) {
      console.log(event);
    },
  },
});
</script>

<template>
  <div style="text-align: center">
    <div id="app" style="margin: auto; max-width: 585px">
      <vue-3-widget-mondial-relay
        brand="BDTEST  "
        defaultPostCode="28037"
        defaultCountry="ES"
        maxResults="7"
        deliveryMode="24R"
        allowedCountries="['ES']"
        :translations="{
          headerTitle: 'Select your mondial relay ®',
          findCpText: 'CP',
          findCityText: 'City',
          cityNoResults: 'no results',
          btnListMobile: 'List',
          btnMapMobile: 'Map',
        }"
        @select="setParcelSelected($event)"
      />
    </div>
  </div>
</template>
