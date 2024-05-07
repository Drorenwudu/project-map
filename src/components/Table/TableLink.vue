<script setup>
import { defineProps } from "vue";
defineProps({
    table: {
        type: Array,
    },
});
</script>
<template>
    <div class="scrollable-container">
        <div class="scrollable-content">
            <table class="min-w-full divide-y divide-gray-200">
                <tr>
                    <th
                        class="px-3 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
                    >
                        Geometry
                    </th>
                    <th
                        class="px-3 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider"
                    >
                        Coordinates
                    </th>
                </tr>
                <tr v-for="item in table" :key="item">
                    <td class="px-3 py-2 text-sm whitespace-no-wrap">
                        {{ item.geometry.type }}
                    </td>
                    <td class="px-3 py-2 text-sm whitespace-no-wrap">
                        {{ item.geometry.coordinates }}
                    </td>
                </tr>
            </table>
        </div>
    </div>
</template>

<style setup>
.scrollable-container {
    height: calc(100vh - 80px);
    overflow-y: auto;
}
</style>
