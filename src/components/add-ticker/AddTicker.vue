<template>
  <section>
    <div class="flex">
      <div class="max-w-xs">
        <label for="wallet" class="block text-sm font-medium text-gray-700">
          Тикер
        </label>
        <div class="mt-1 relative rounded-md shadow-md">
          <input
            v-model="ticker"
            @keydown.enter="add(ticker)"
            type="text"
            name="wallet"
            id="wallet"
            class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
            placeholder="Например DOGE"
          />
        </div>
        <add-helper
          :tickersNames="tickersNames"
          :matchingTickersArr="matchingTickersArr"
          :ticker="ticker"
          :existingTicker="existingTicker"
          @add-ticker="add"
        />
      </div>
    </div>
    <add-button
      @click="add(ticker)"
      type="button"
      :disabled="disabled"
      class="my-4"
    />
    <!-- <br />
        tickersNames: {{ tickersNames }}
        <br />
        tickers: {{ tickers }} -->
  </section>
</template>

<script>
import AddButton from "../AddButton.vue";
import AddHelper from "./AddHelper.vue";

export default {
  components: {
    AddButton,
    AddHelper,
  },

  props: {
    disabled: {
      type: Boolean,
      required: false,
      default: false,
    },
    tickersNames: {
      type: Array,
    },
    existingTicker: {
      type: Boolean,
    },
  },

  emits: {
    "add-ticker": (value) => typeof value === "string" && value.length > 0,
    "download-сoins": (value) => typeof value === "boolean",
    "change-existing-ticker": (value) => typeof value === "boolean",
  },

  data() {
    return {
      ticker: "",

      matchingTickersArr: [],

      coinList: [],
      urlCoinlist:
        "https://min-api.cryptocompare.com/data/all/coinlist?summary=true",
    };
  },

  async mounted() {
    const response = await fetch(this.urlCoinlist);
    const result = await response.json();
    this.coinList = Object.keys(result.Data);
    this.downloadCoins();
  },

  methods: {
    add(item) {
      if (this.ticker.length === 0) {
        return;
      }

      if (this.tickersNames && this.tickersNames.includes(this.ticker)) {
        this.changeExistingTicker(true)
        return;
      } else {
        this.changeExistingTicker(false);
      }

      this.ticker = "";
      this.$emit("add-ticker", item);
    },

    downloadCoins() {
      this.$emit("download-сoins", true);
    },

    changeExistingTicker(bool) {
      this.$emit("change-existing-ticker", bool);
    },
  },

  watch: {
    ticker(value) {
     

      this.matchingTickersArr = [];
      // console.log(this.ticker);
      this.coinList.forEach((item) => {
        const string = item.slice(0, value.length);
        if (string === value && this.matchingTickersArr.length <= 3) {
          // console.log(item, typeof(item));
          this.matchingTickersArr.push(item);
        }
      });
       this.changeExistingTicker(false);
    },
  },
};
</script>
