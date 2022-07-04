
<template>
  <form method="post" @submit="search" class="mondial-relay-form">
    <div class="mondial-relay-form__city">
      <mondial-relay-city-search
        @setPostCode="changeCP"
        :country="country"
        :findCityText="t.findCityText"
        :cityNoResults="t.cityNoResults"
      />
    </div>
    <div class="mondial-relay-form__countries">
      <mondial-relay-country-selector
        @changeCountry="changeCountry"
        :countrySelected="country"
        :allowedCountries="allowedCountries"
      />
    </div>
    <div class="mondial-relay-form__cp">
      <input
        class="mondial-relay-input"
        id="cp"
        name="cp"
        v-model="cp"
        required
        :placeholder="t.findCpText"
      />
    </div>

    <button class="mondial-relay-form__search">
      <img src="@/assets/images/search.svg" alt="search" />
    </button>
  </form>
</template>


<script>
import { defineComponent } from "vue";
import MondialRelayCitySearch from "./MondialRelayCitySearch.vue";
import MondialRelayCountrySelector from "./MondialRelayCountrySelector.vue";
import './../assets/scss/form-search-cp.scss'

export default defineComponent({
  name: "MondialRelayFormSearchCp",
  props: ["defaultPostCode", "defaultCountry", "allowedCountries", "t"],
  components: {
    MondialRelayCountrySelector,
    MondialRelayCitySearch
  },
  data() {
    return {
      cp: this.defaultPostCode,
      country: this.defaultCountry,
    };
  },
  methods: {
    async search(e) {
      this.$emit("search", {
        cp: this.cp,
        country: this.country,
      });
      e.preventDefault();
    },
    changeCountry(country) {
      this.country = country;
    },
    changeCP(postCode) {
      this.cp = postCode;
      this.$emit("search", {
        cp: postCode,
        country: this.country,
      });
    },
  },
});
</script>