<script setup>
const emit = defineEmits("selectedRow");
const props = defineProps({
  list: {
    type: Array,
    default: () => [],
  },
});

let selectedRow = ref(null);
</script>
<template>
  <div class="bg-[#2F2F2F] rounded-lg">
    <table class="w-full whitespace-nowrap text-left">
      <colgroup>
        <col class="w-full sm:w-3/12" />
        <col class="lg:w-2/12" />
        <col class="lg:w-2/12" />
        <col class="lg:w-1/12" />
        <col class="lg:w-1/12" />
      </colgroup>
      <thead
        class="border-b border-white/10 text-sm leading-6 text-white bg-[#1D1D1D] rounded-t-lg"
      >
        <tr>
          <th scope="col" class="py-2 pl-4 pr-8 font-semibold sm:pl-6 lg:pl-8">
            Name
          </th>
          <th
            scope="col"
            class="hidden py-2 pl-0 pr-8 font-semibold sm:table-cell"
          >
            Team
          </th>
          <th
            scope="col"
            class="py-2 pl-0 pr-4 text-right font-semibold sm:pr-8 sm:text-left lg:pr-20"
          >
            Position
          </th>
          <th
            scope="col"
            class="hidden py-2 pl-0 pr-8 font-semibold md:table-cell lg:pr-20"
          >
            Salary
          </th>
          <th
            scope="col"
            class="hidden py-2 pl-0 pr-4 text-right font-semibold sm:table-cell sm:pr-6 lg:pr-8"
          >
            Points
          </th>
        </tr>
      </thead>
      <tbody class="divide-y divide-white/5">
        <tr
          v-for="(item, index) in list"
          :key="index"
          :class="[
            'hover:bg-[#807B0F] cursor-pointer transition-colors',
            selectedRow === index && 'bg-[#807B0F]',
          ]"
          @click="
            () => {
              emit('selectedRow', item);
              selectedRow = index;
            }
          "
        >
          <td class="py-4 pl-4 pr-8 sm:pl-6 lg:pl-8">
            <div class="flex items-center gap-x-4">
              <div class="truncate text-sm font-medium leading-6 text-white">
                {{ item.operatorPlayerName }}
              </div>
            </div>
          </td>
          <td class="hidden py-4 pl-0 pr-4 sm:table-cell sm:pr-8">
            <div class="flex gap-x-3">
              <div class="font-mono text-sm leading-6 text-white">
                {{ item.team }}
              </div>
            </div>
          </td>
          <td class="py-4 pl-0 pr-4 text-sm leading-6 sm:pr-8 lg:pr-20">
            <div class="flex items-center justify-end gap-x-2 sm:justify-start">
              <div class="hidden text-white sm:block">
                {{ item.operatorPosition }}
              </div>
            </div>
          </td>
          <td
            class="hidden py-4 pl-0 pr-8 text-sm leading-6 text-white md:table-cell lg:pr-20"
          >
            ${{ item.operatorSalary }}
          </td>
          <td
            class="hidden py-4 pl-0 pr-4 text-right text-sm leading-6 text-white sm:table-cell sm:pr-6 lg:pr-8"
          >
            {{ item.fantasyPoints }}
          </td>
        </tr>
      </tbody>
    </table>
    <slot name="pagination" />
  </div>
</template>
