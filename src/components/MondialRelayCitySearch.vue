<template>
  <div>
    <div class="mondial-relay-city">
      <input
        class="mondial-relay-city__input"
        id="search"
        name="search"
        @input="searchCity"
        :placeholder="findCityText"
        autocomplete="nope"
        v-model="search"
        @blur="resetField"
      />
      <transition name="fade">
        <div
          class="mondial-relay-city__dropdown"
          v-if="open"
          v-click-outside="close"
        >
          <ul v-if="this.cities.length > 0">
            <li v-for="(city, key) in cities" :key="key" @click="setCity(city)">
              {{ city.Name }} ({{ city.PostCode }})
            </li>
          </ul>
          <ul v-if="this.cities.length == 0">
            <li @click="close">
              {{ cityNoResults }}
            </li>
          </ul>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import { jsonp } from "vue-jsonp";
import ClickOutside from "vue-click-outside";
import './../assets/scss/city-search.scss'
import { defineComponent } from "vue";

export default defineComponent({
  name: "MondialRelayCitySearch",
  props: ["country", "findCityText", "cityNoResults"],
  data() {
    return {
      search: "",
      citySelected: {},
      cities: [],
      open: false,
      debounce: null
    };
  },
  methods: {
    resetField() {
      if (this.citySelected.Name != this.search) {
        this.search = "";
      }
    },
    searchCity() {
      if (this.search.length > 3) {
        clearTimeout(this.debounce);
        this.debounce = setTimeout(() => {
          this.getCitiesByInput(this.search);
        }, 600);
      }
    },
    getCitiesByInput(input) {
      jsonp(
        "https://widget.mondialrelay.com/parcelshop-picker/v4_0/services/parcelshop-picker.svc/AutoCPLCity",
        {
          callbackQuery: "method",
          callbackName: "receive",
          Country: this.country,
          City: input,
        }
      )
        .then((data) => {
          this.cities = data.Value;
          this.open = true;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    setCity(city) {
      this.search = city.Name;
      this.citySelected = city;
      this.$emit("setPostCode", city.PostCode);
      this.open = false;
    },
    close() {
      if (this.open) {
        this.open = false;
      }
    },
  },
  directives: {
    ClickOutside,
  },
});
</script>