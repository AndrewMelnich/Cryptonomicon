<template>
  <div
    v-show="ticker"
    class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap"
  >
    <span
      v-for="(item, index) in matchingTickersArr"
      :key="index"
      class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
      @click="add(item)"
    >
      {{ item }}
    </span>
  </div>
  <div v-if="existingTicker" class="text-sm text-red-600">
    Такой тикер уже добавлен
  </div>
</template>

<script>
export default {
  props:{
    tickersNames:{
      type: Array
    },
    matchingTickersArr:{
      type: Array
    },
    ticker:{
      type: String 
    },
    existingTicker:{
      type: Boolean
    }
  },

  emits: {
    "add-ticker": (value) => typeof value === "string",
  },

  methods:{
    add(item){
      this.$emit("add-ticker", item)
    }
  }

};

</script>

<style lang="scss" scoped></style>
