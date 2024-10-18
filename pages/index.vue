<script setup>
import data from "~/assets/data.json";
const filters = [
  {
    label: "Select operator",
    listValue: "operator",
  },
  {
    label: "Select Game Type",
    listValue: "operatorGameType",
  },
  {
    label: "Select Slate Name",
    listValue: "operatorName",
  },
];

let selectedPlayer = ref(null);
let selected = ref({
  operator: "",
  operatorGameType: "",
  operatorName: "",
});
let currentPage = ref(0);
let itemsPerPage = ref(8);
let start = ref(0);
let end = ref(start.value + itemsPerPage.value);

const totalPages = computed(() =>
  Math.ceil(allPlayers.value.length / itemsPerPage.value)
);
// Get list of filters
const getDropdownList = (prop) => {
  return [...new Set(data?.map((item) => item[prop]))];
};

const allPlayers = computed(() => {
  console.log(selected.value.operator, "sel");
  let filteredData = selected.value.operator
    ? data.filter((item) => item.operator === selected.value.operator)
    : data;

  // Further filter by gameType if selected.value.gameType exists
  filteredData = selected.value.operatorGameType
    ? filteredData.filter(
        (item) => item.operatorGameType === selected.value.operatorGameType
      )
    : filteredData;

  // Further filter by operatorName if selected.value.operatorName exists
  filteredData = selected.value.operatorName
    ? filteredData.filter(
        (item) => item.operatorName === selected.value.operatorName
      )
    : filteredData;

  // Return the flattened array of dfsSlatePlayers
  console.log(filteredData, "hhhhh");
  return filteredData.map((item) => item.dfsSlatePlayers).flat();
});

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
};

const prevPage = () => {
  if (currentPage.value > 0) {
    currentPage.value--;
  }
};

const gotoPage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page;
  }
};

const paginatedData = computed(() => {
  start.value = currentPage.value * itemsPerPage.value;
  end.value = start.value + itemsPerPage.value;
  return allPlayers.value.slice(start.value, end.value);
});
</script>
<template>
  <div class="max-w-7xl mx-auto py-16">
    <div
      class="max-w-4xl mx-auto grid grid-cols-3 gap-4 bg-white/10 p-6 rounded-lg relative z-50"
    >
      <template v-if="data.length">
        <Filter
          v-for="(item, index) in filters"
          :key="index"
          :label="item.label"
          :list="getDropdownList(item.listValue)"
          :field="item.listValue"
          @selectedFilter="
            (filter) => {
              currentPage = 0;
              selected[filter.field] = filter.value;
            }
          "
        />
      </template>
    </div>
    <div class="grid md:grid-cols-12 mt-16 gap-6 relative h-full">
      <div :class="[selectedPlayer ? 'col-span-9' : 'col-span-12']">
        <template v-if="data.length">
          <DataTable
            :list="paginatedData"
            @selectedRow="(player) => (selectedPlayer = player)"
          >
            <template #pagination>
              <div
                class="flex items-center justify-stretch space-x-12 text-white/90 py-4 bg-black/30 px-6"
              >
                <div>
                  Page
                  <select
                    name="rows"
                    id=""
                    @change="(e) => gotoPage(Number(e.target.value))"
                    class="text-black/90"
                  >
                    <option :value="i" v-for="i in totalPages" :key="i">
                      {{ i }}
                    </option>
                  </select>
                </div>
                <div>
                  Rows per page
                  <select
                    name="rows"
                    id=""
                    @change="(e) => (itemsPerPage = +e.target.value)"
                    class="text-black/90"
                  >
                    <option :value="8 * i" v-for="i in 4" :key="i">
                      {{ i * 8 }}
                    </option>
                  </select>
                </div>
                <div class="flex items-center space-x-4">
                  <p>
                    {{ start + 1 }} -
                    {{ end > allPlayers.length ? allPlayers.length : end }} of
                    {{ allPlayers.length }}
                  </p>

                  <div class="space-x-4">
                    <button
                      :disabled="start < 0"
                      class="transform rotate-90"
                      @click="prevPage()"
                    >
                      <IconsChevron />
                    </button>
                    <button
                      :disabled="end > allPlayers.length"
                      class="transform -rotate-90"
                      @click="nextPage()"
                    >
                      <IconsChevron />
                    </button>
                  </div>
                </div>
              </div>
            </template>
          </DataTable>
        </template>
      </div>
      <div class="col-span-3" v-if="selectedPlayer">
        <div class="bg-black/10 rounded-t-lg">
          <img src="~/assets/img/player.png" />
        </div>
        <div class="text-center text-white/80 bg-[#2F2F2F] p-6 rounded-lg">
          <h4 class="text-[32px]">
            {{ selectedPlayer.operatorPlayerName }}
          </h4>
          <h2 class="text-[128px]">{{ selectedPlayer.fantasyPoints || 0 }}</h2>
          <p>Points</p>
        </div>
      </div>
    </div>
  </div>
</template>
