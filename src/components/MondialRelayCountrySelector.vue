<template>
  <div>
    <div class="mondial-relay-selector">
      <input
        class="mondial-relay-selector__input"
        id="cp"
        readonly="readonly"
        name="cp"
        required
        size="8"
        v-on:click="open = !open"
      />
      <div
        class="mondial-relay-selector__input__flag"
        v-on:click="open = !open"
      >
        <img v-if="countrySelected == 'ES'" src="@/assets/images/es-flag.svg" width="30" />
        <img v-if="countrySelected == 'AT'" src="@/assets/images/at-flag.svg" width="30" />
        <img v-if="countrySelected == 'FR'" src="@/assets/images/fr-flag.svg" width="30" />
        <img v-if="countrySelected == 'DE'" src="@/assets/images/de-flag.svg" width="30" />
        <img v-if="countrySelected == 'LU'" src="@/assets/images/lu-flag.svg" width="30" />
        <img v-if="countrySelected == 'NL'" src="@/assets/images/nl-flag.svg" width="30" />
        <img v-if="countrySelected == 'BE'" src="@/assets/images/be-flag.svg" width="30" />
      </div>
      <div
        class="mondial-relay-selector__input__toggle"
        v-on:click="open = !open"
      >
        <svg
          data-v-46e105de=""
          mlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          class="country-selector__toggle__arrow"
        >
          <path
            data-v-46e105de=""
            d="M7.41 8.59L12 13.17l4.59-4.58L18 10l-6 6-6-6 1.41-1.41z"
            class="arrow"
          ></path>
          <path data-v-46e105de="" fill="none" d="M0 0h24v24H0V0z"></path>
        </svg>
      </div>
      <transition name="fade">
        <div class="mondial-relay-selector__dropdown" v-if="open">
          <ul>
            <li
            v-if="allowedCountries.includes('FR')"
              :class="
                countrySelected == 'FR'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('FR')"
            >
              <img src="@/assets/images/fr-flag.svg" width="30" />
            </li>
            <li
            v-if="allowedCountries.includes('ES')"
              :class="
                countrySelected == 'ES'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('ES')"
            >
              <img src="@/assets/images/es-flag.svg" width="30" />
            </li>
            <li
            v-if="allowedCountries.includes('BE')"
              :class="
                countrySelected == 'BE'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('BE')"
            >
              <img src="@/assets/images/be-flag.svg" width="30" />
            </li>
            <li
            v-if="allowedCountries.includes('NL')"
              :class="
                countrySelected == 'NL'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('NL')"
            >
              <img src="@/assets/images/nl-flag.svg" width="30" />
            </li>
            <li
            v-if="allowedCountries.includes('LU')"
              :class="
                countrySelected == 'LU'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('LU')"
            >
              <img src="@/assets/images/lu-flag.svg" width="30" />
            </li>
            <li
            v-if="allowedCountries.includes('DE')"
              :class="
                countrySelected == 'DE'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('DE')"
            >
              <img src="@/assets/images/de-flag.svg" width="30" />
            </li>
            <li
              v-if="allowedCountries.includes('AT')"
              :class="
                countrySelected == 'AT'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('AT')"
            >
              <img src="@/assets/images/at-flag.svg" width="30" />
            </li>
          </ul>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import './../assets/scss/country-selector.scss'
export default defineComponent({
  name: "MondialRelayCountrySelector",
  props: ["countrySelected", "allowedCountries"],
  data() {
    return {
      open: false,
    };
  },
  methods: {
    selectCountry(country) {
      this.$emit("changeCountry", country);
      this.open = false;
    },
  },
});
</script>