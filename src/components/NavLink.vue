<script setup>
import { ref } from "vue";
import MapLink from "./MapLink.vue";

const showTable = ref(false);

function showT() {
    showTable.value = !showTable.value;
}
</script>
<template>
    <header class="bg-slate-300 p-2">
        <div class="container mx-auto flex justify-between items-center">
            <div class="flex-none w-14 basis-1/3">
                <a href="Home">
                    <img
                        src="../assets/logoForSite/reshot-icon-map-marker-ZVTC7DK2PM.svg"
                        alt="SVG Image"
                    />
                </a>
            </div>
            <nav class="basis-1/3">
                <ul class="flex justify-center space-x-4 text-1xl">
                    <li>
                        <a
                            @click="showT"
                            class="font-serif text-black hover:underline"
                            >Table</a
                        >
                    </li>
                </ul>
            </nav>
            <div class="basis-1/3 flex flex-row-reverse"></div>
        </div>
    </header>
    <MapLink :show="showTable" />
</template>

<style setup></style>
