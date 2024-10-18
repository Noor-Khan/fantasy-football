<script setup>
const emit = defineEmits(["selectedFilter"]);
const props = defineProps({
  label: String,
  field: String,
  list: Array,
});

let isDropdown = ref(false);
let selectedValue = ref("");
</script>
<template>
  <div class="relative">
    <button
      class="bg-[#1D1D1D] flex items-center justify-between w-full h-14 p-4 text-white/90 rounded-lg space-x-4"
      @click="isDropdown = !isDropdown"
    >
      <p>{{ !selectedValue ? label : selectedValue }}</p>
      <span
        :class="['transform transition-all', isDropdown ? 'rotate-180' : '0']"
      >
        <IconsChevron />
      </span>
    </button>
    <ul
      v-show="isDropdown"
      class="max-h-[200px] overflow-y-scroll bg-[#1D1D1D] absolute z-[9999] w-full rounded-lg mt-2 transition-all"
    >
      <li v-for="(item, index) in list" :key="index">
        <button
          v-if="item"
          class="py-2 px-4 border-b border-white/10 w-full text-left text-white/90 shadow-lg hover:bg-black last:border-none"
          @click="
            () => {
              selectedValue = item;
              isDropdown = false;
              emit('selectedFilter', { field: field, value: item });
            }
          "
        >
          {{ item }}
        </button>
      </li>
    </ul>
  </div>
</template>
