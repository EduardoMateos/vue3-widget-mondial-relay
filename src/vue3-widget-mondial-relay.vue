<template>
  <div class="mondial-relay-widget">
    <mondial-relay-header :headerTitle="t.headerTitle">
      <template v-slot:formSearchCp>
        <mondial-relay-form-search-cp
          @search="search"
          :defaultPostCode="defaultPostCode"
          :defaultCountry="defaultCountry"
          :allowedCountries="allowedCountries"
          :t="t"
        ></mondial-relay-form-search-cp>
      </template>
    </mondial-relay-header>

    <mondial-relay-error-message :message="messageError" v-if="hasError" />

    <div v-if="!hasError">
      <div class="mondial-relay-tab mondial-relay-widget-hide-desktop">
        <button
          type="button"
          :class="mobileShowMap == true ? 'active' : ''"
          @click="mobileShowMap = true"
        >
          {{ t.btnListMobile }}
        </button>
        <button
          type="button"
          :class="mobileShowMap == false ? 'active' : ''"
          @click="mobileShowMap = false"
        >
          {{ t.btnMapMobile }}
        </button>
      </div>
      <div class="mondial-relay-row">
        <div
          class="mondial-relay-left-column"
          :class="!mobileShowMap ? 'mondial-relay-widget-hide-mobile' : ''"
        >
          <div class="mondial-relay-parcel-list">
            <div
              v-for="(item, key) in parcelShopList"
              :key="key"
              class="mondial-relay-parcel-list__shop"
              @click="selectParcel(item)"
              :class="[
                parcelSelected && parcelSelected.ID == item.ID
                  ? 'mondial-relay-parcel-list__shop--selected'
                  : '',

                !item.Available
                  ? 'mondial-relay-parcel-list__shop--disabled'
                  : '',
              ]"
            >
              <div class="mondial-relay-parcel-list__shop__name">
                {{ item.Nom }}
              </div>
              {{ item.Adresse1 }}<br />
              {{ item.CP }} {{ item.Ville }}
            </div>
          </div>
        </div>
        <div
          class="mondial-relay-right-column"
          :class="mobileShowMap ? 'mondial-relay-widget-hide-mobile' : ''"
        >
          <mondial-relay-map
            :parcelShopList="parcelShopList"
            :parcelSelected="parcelSelected"
            @selectParcel="selectParcel"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.vue3-widget-mondial-relay {
  display: block;
  width: 400px;
  margin: 25px auto;
  border: 1px solid #ccc;
  background: #eaeaea;
  text-align: center;
  padding: 25px;
}
.vue3-widget-mondial-relay p {
  margin: 0 0 1em;
}
</style>

<script>
import { defineComponent } from "vue";
import { jsonp } from "vue-jsonp";
import MondialRelayMap from "./components/MondialRelayMap.vue";
import MondialRelayHeader from "./components/MondialRelayHeader.vue";
import MondialRelayFormSearchCp from "./components/MondialRelayFormSearchCp.vue";
import MondialRelayErrorMessage from "./components/MondialRelayErrorMessage.vue";
import locales from "./assets/locales";
import "./assets/scss/base.scss";

export default defineComponent({
  components: { MondialRelayMap, MondialRelayHeader, MondialRelayFormSearchCp, MondialRelayErrorMessage },
  name: "Vue3WidgetMondialRelay", // vue component name
  props: {
    brand: {
      default: function () {
        return "BDTEST  ";
      },
    },
    defaultPostCode: {
      default: function () {
        return 59000;
      },
    },
    defaultCountry: {
      default: function () {
        return "FR";
      },
    },
    maxResults: {
      default: function () {
        return "7";
      },
    },
    deliveryMode: {
      default: function () {
        return "24R";
      },
    },
    allowedCountries: {
      default: function () {
        return ["FR", "ES", "BE", "NL", "LU", "DE", "AT"];
      },
    },
    translations: { type: Object, default: null },
  },
  computed: {
    t() {
      return {
        ...locales,
        ...this.translations,
      };
    },
  },
  data() {
    return {
      messageError: null,
      hasError: false,
      mobileShowMap: false,
      searchParcelShop: {
        Brand: this.brand,
        ClientContainerId: "Zone_Widget",
        ColLivMod: this.deliveryMode,
        Country: this.defaultCountry,
        Latitude: "",
        Longitude: "",
        NbResults: this.maxResults,
        PostCode: this.defaultPostCode,
        SearchDelay: "",
        SearchFar: "75",
        Service: "",
        VacationAfter: "",
        VacationBefore: "",
        Weight: "",
      },
      parcelShopList: [],
      parcelSelected: null,
    };
  },
  created() {
    this.getParcelShopList();
  },
  methods: {
    selectParcel(parcel) {
      if (parcel.Available) {
        this.$emit("select", parcel);
      }

      this.parcelSelected = parcel;
    },
    search(data) {
      if (data.cp != this.searchParcelShop.PostCode) {
        this.searchParcelShop.PostCode = data.cp;
        this.searchParcelShop.Country = data.country;
        this.getParcelShopList();
      }
    },
    getParcelShopList() {
      jsonp(
        "https://widget.mondialrelay.com/parcelshop-picker/v4_0/services/parcelshop-picker.svc/SearchPR",
        {
          callbackQuery: "method",
          callbackName: "receive",
          Brand: this.brand,
          ClientContainerId: "Zone_Widget",
          ColLivMod: this.searchParcelShop.ColLivMod,
          Country: this.searchParcelShop.Country,
          Latitude: "",
          Longitude: "",
          NbResults: this.searchParcelShop.NbResults,
          PostCode: this.searchParcelShop.PostCode,
          SearchDelay: "",
          SearchFar: "75",
          Service: "",
          VacationAfter: "",
          VacationBefore: "",
          Weight: "",
        }
      )
        .then((data) => {
          if (data.Error) {
            this.parcelShopList = [];
            this.messageError = data.Error;
            this.hasError = true;
          } else {
            this.parcelShopList = data.PRList;
            this.messageError = null;
            this.hasError = false;
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
});
</script>
