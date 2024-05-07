<script setup>
import { Map } from "@maptiler/sdk";
import { ref, onMounted, defineProps } from "vue";
import { saveAs } from "file-saver";
import TableLink from "./Table/TableLink.vue";

const coordinatesPointersForPoint = [];
const reactivePoint = ref([]);
const idPoint = [];
const coordinatesPointersForLines = [];
const reactiveLines = ref([]);
const idLines = [];
const coordinatesPointersForPolygons = [];
const reactivePolygons = ref([]);
const idPolygons = [];

const map = ref(null);
const mapContainer = ref(null);
const colorForPoint = ref("rgb(239 68 68)");
const colorForLine = ref("rgb(239 68 68)");
const colorForPolygons = ref("rgb(239 68 68)");

onMounted(() => {
    const key = "";
    map.value = new Map({
        container: mapContainer.value,
        style: `https://api.maptiler.com/maps/basic-v2/style.json?key=${key}`,
        center: [30.523, 50.45],
        zoom: 15,
    });
});

function changeColor(color) {
    if ("bg-green-500" === color) {
        blockColor.value = "rgb(34 197 94)";
        colorForPoint.value = "rgb(34 197 94)";
        colorForLine.value = "rgb(34 197 94)";
        colorForPolygons.value = "rgb(34 197 94)";
    } else if ("bg-red-500" === color) {
        blockColor.value = "rgb(239 68 68)";
        colorForPoint.value = "rgb(239 68 68)";
        colorForLine.value = "rgb(239 68 68)";
        colorForPolygons.value = "rgb(239 68 68)";
    } else if ("bg-sky-600" === color) {
        blockColor.value = "rgb(2 132 199)";
        colorForPoint.value = "rgb(2 132 199)";
        colorForLine.value = "rgb(2 132 199)";
        colorForPolygons.value = "rgb(2 132 199)";
    } else if ("bg-fuchsia-600" === color) {
        blockColor.value = "rgb(192 38 211)";
        colorForPoint.value = "rgb(192 38 211)";
        colorForLine.value = "rgb(192 38 211)";
        colorForPolygons.value = "rgb(192 38 211)";
    } else if ("bg-neutral-950" === color) {
        blockColor.value = "rgb(10 10 10)";
        colorForPoint.value = "rgb(10 10 10)";
        colorForLine.value = "rgb(10 10 10)";
        colorForPolygons.value = "rgb(10 10 10)";
    }
}

function addCoordinatesPoint() {
    idPoint.push("point" + Date.now());
    map.value.addSource(idPoint[idPoint.length - 1], {
        type: "geojson",
        data: {
            type: "FeatureCollection",
            features: [
                {
                    type: "Feature",
                    geometry: {
                        type: "MultiPoint",
                        coordinates: [],
                    },
                },
            ],
        },
    });
    map.value.addLayer({
        id: idPoint[idPoint.length - 1],
        type: "circle",
        source: idPoint[idPoint.length - 1],
        paint: {
            "circle-radius": 3,
            "circle-color": colorForPoint.value,
        },
    });
    coordinatesPointersForPoint.push({
        type: "Feature",
        geometry: {
            type: "MultiPoint",
            coordinates: [],
        },
        id: idPoint[idPoint.length - 1],
    });
    reactivePoint.value.push({
        type: "Feature",
        geometry: {
            type: "MultiPoint",
            coordinates: [],
        },
        id: idPoint[idPoint.length - 1],
    });
}

function addCoordinatesLine() {
    idLines.push("line" + Date.now());
    map.value.addSource(idLines[idLines.length - 1], {
        type: "geojson",
        data: {
            type: "Feature",
            properties: {},
            geometry: {
                type: "LineString",
                coordinates: [],
            },
        },
    });
    map.value.addLayer({
        id: idLines[idLines.length - 1],
        type: "line",
        source: idLines[idLines.length - 1],
        layout: {
            "line-join": "round",
            "line-cap": "round",
        },
        paint: {
            "line-color": colorForLine.value,
            "line-width": 3,
        },
    });
    coordinatesPointersForLines.push({
        type: "Feature",
        geometry: {
            type: "LineString",
            coordinates: [],
        },
        id: idLines[idLines.length - 1],
        properties: {},
    });
    reactiveLines.value.push({
        type: "Feature",
        geometry: {
            type: "LineString",
            coordinates: [],
        },
        id: idLines[idLines.length - 1],
        properties: {},
    });
}

function addCoordinatesPolygon() {
    idPolygons.push("polygon" + Date.now());
    const last = idPolygons.length - 1;
    map.value.addSource(idPolygons[last], {
        type: "geojson",
        data: {
            type: "Feature",
            geometry: {
                type: "Polygon",
                coordinates: [[]],
            },
        },
    });
    map.value.addLayer({
        id: idPolygons[last],
        type: "fill",
        source: idPolygons[last],
        layout: {},
        paint: {
            "fill-color": colorForPolygons.value,
            "fill-opacity": 0.4,
        },
    });
    coordinatesPointersForPolygons.push({
        type: "Feature",
        geometry: {
            type: "Polygon",
            coordinates: [[]],
        },
        id: idPolygons[last],
    });
    reactivePolygons.value.push({
        type: "Feature",
        geometry: {
            type: "Polygon",
            coordinates: [[]],
        },
        id: idPolygons[last],
    });
}

function updateCoordinatesPoint(e) {
    const last = coordinatesPointersForPoint.length - 1;
    coordinatesPointersForPoint[last].geometry.coordinates.push([
        e.lngLat.lng,
        e.lngLat.lat,
    ]);
    map.value
        .getSource(idPoint[idPoint.length - 1])
        .setData(coordinatesPointersForPoint[last]);
    reactivePoint.value[last].geometry.coordinates.push([
        e.lngLat.lng,
        e.lngLat.lat,
    ]);
}

function updateCoordinatesLine(e) {
    const last = coordinatesPointersForLines.length - 1;
    coordinatesPointersForLines[last].geometry.coordinates.push([
        e.lngLat.lng,
        e.lngLat.lat,
    ]);
    map.value
        .getSource(idLines[idLines.length - 1])
        .setData(coordinatesPointersForLines[last]);
    reactiveLines.value[last].geometry.coordinates.push([
        e.lngLat.lng,
        e.lngLat.lat,
    ]);
}

function updateCoordinatesPolygon(e) {
    const last = coordinatesPointersForPolygons.length - 1;
    coordinatesPointersForPolygons[last].geometry.coordinates[0].push([
        e.lngLat.lng,
        e.lngLat.lat,
    ]);
    map.value
        .getSource(idPolygons[idPolygons.length - 1])
        .setData(coordinatesPointersForPolygons[last]);
    reactivePolygons.value[last].geometry.coordinates[0].push([
        e.lngLat.lng,
        e.lngLat.lat,
    ]);
}

const addPoint = () => {
    addCoordinatesPoint();
    map.value.on("click", updateCoordinatesPoint);
    map.value.off("click", updateCoordinatesLine);
    map.value.off("click", updateCoordinatesPolygon);
};

const addLine = () => {
    addCoordinatesLine();
    map.value.on("click", updateCoordinatesLine);
    map.value.off("click", updateCoordinatesPoint);
    map.value.off("click", updateCoordinatesPolygon);
};

const addPolygon = () => {
    addCoordinatesPolygon();
    map.value.on("click", updateCoordinatesPolygon);
    map.value.off("click", updateCoordinatesPoint);
    map.value.off("click", updateCoordinatesLine);
};

const finishDrawing = () => {
    map.value.off("click", updateCoordinatesPoint);
    map.value.off("click", updateCoordinatesLine);
    map.value.off("click", updateCoordinatesPolygon);
};

const cleanMap = () => {
    if (confirm("Do you want to delete all data?")) {
        if (map.value.getLayer(idPoint[idPoint.length - 1])) {
            idPoint.forEach((id) => {
                map.value.removeLayer(id);
                map.value.removeSource(id);
            });
            coordinatesPointersForPoint.length = 0;
            idPoint.length = 0;
            reactivePoint.value.length = 0;
        }
        if (map.value.getLayer(idLines[idLines.length - 1])) {
            idLines.forEach((id) => {
                map.value.removeLayer(id);
                map.value.removeSource(id);
            });
            coordinatesPointersForLines.length = 0;
            idLines.length = 0;
            reactiveLines.value.length = 0;
        }
        if (map.value.getLayer(idPolygons[idPolygons.length - 1])) {
            idPolygons.forEach((id) => {
                map.value.removeLayer(id);
                map.value.removeSource(id);
            });
            coordinatesPointersForPolygons.length = 0;
            idPolygons.length = 0;
            reactivePolygons.value.length = 0;
        }
    }
};
const showSidebar = ref(false);
const blockColor = ref("bg-red-500");

function toggleSidebar() {
    showSidebar.value = !showSidebar.value;
}

const showRangeslider = ref(false);
const sliderValue = ref(4);

function rangeslider() {
    showRangeslider.value = !showRangeslider.value;
}

function updateOpacity() {
    map.value.setPaintProperty(
        idPolygons[idPolygons.length - 1],
        "fill-opacity",
        sliderValue.value / 10
    );
}

function exportGeoJSON() {
    const geojsonData = {
        type: "FeatureCollection",
        features: [].concat(
            coordinatesPointersForPoint,
            coordinatesPointersForPolygons,
            coordinatesPointersForLines
        ),
    };
    if (geojsonData.features.length === 0) {
        alert("There is nothing on the map");
    } else if (confirm("Do you want to export data?")) {
        const blob = new Blob([JSON.stringify(geojsonData)], {
            type: "application/json;charset=utf-8",
        });
        saveAs(blob, "dataCollection.geojson");
    }
}

defineProps({
    show: {
        type: Boolean,
    },
});
</script>

<template>
    <div class="flex flex-row">
        <div v-if="show" class="basis-2/6">
            <TableLink
                :table="[
                    ...reactivePoint,
                    ...reactiveLines,
                    ...reactivePolygons,
                ]"
            />
        </div>
        <div class="map-wrap">
            <div class="map" ref="mapContainer"></div>
            <div class="flex flex-col absolute top-4 right-4">
                <button
                    class="bg-slate-50 hover:bg-slate-400 text-white font-bold py-2 px-2 rounded-t"
                    @click="finishDrawing"
                >
                    <img
                        src="../assets/iconForButton/8666726_mouse_pointer_icon.svg"
                        width="20"
                    />
                </button>
                <button
                    class="bg-slate-50 hover:bg-slate-400 text-white font-bold py-2 px-2 border-t"
                    @click="addPoint"
                >
                    <img
                        src="../assets/iconForButton/9081317_point_icon.svg"
                        width="20"
                    />
                </button>
                <button
                    class="bg-slate-50 hover:bg-slate-400 text-white font-bold py-2 px-2 border-t"
                    @click="addLine"
                >
                    <img
                        src="../assets/iconForButton/9023751_line_segment_fill_icon.svg"
                        width="20"
                    />
                </button>
                <button
                    class="bg-slate-50 hover:bg-slate-400 text-white font-bold py-2 px-2 border-t"
                    @click="addPolygon"
                >
                    <img
                        src="../assets/iconForButton/9026959_polygon_thin_icon.svg"
                        width="20"
                    />
                </button>
                <div class="relative">
                    <button
                        class="bg-slate-50 hover:bg-slate-400 py-2 px-2.5 flex items-center justify-center border-t"
                        @click="toggleSidebar"
                    >
                        <div
                            :style="{ backgroundColor: blockColor }"
                            class="h-4 w-4 rounded-full bg-red-500 border"
                        ></div>
                    </button>
                    <div
                        v-if="showSidebar"
                        class="sidebar flex items-center justify-center"
                    >
                        <button
                            class="bg-slate-50 hover:bg-slate-400 py-2 px-2.5 flex items-center justify-center border-r"
                            @click="changeColor('bg-green-500')"
                        >
                            <div
                                class="h-4 w-4 rounded-full bg-green-500 border"
                            ></div>
                        </button>
                        <button
                            class="bg-slate-50 hover:bg-slate-400 py-2 px-2.5 flex items-center justify-center border-r"
                            @click="changeColor('bg-sky-600')"
                        >
                            <div
                                class="h-4 w-4 rounded-full bg-sky-600 border"
                            ></div>
                        </button>
                        <button
                            class="bg-slate-50 hover:bg-slate-400 py-2 px-2.5 flex items-center justify-center border-r"
                            @click="changeColor('bg-fuchsia-600')"
                        >
                            <div
                                class="h-4 w-4 rounded-full bg-fuchsia-600 border"
                            ></div>
                        </button>
                        <button
                            class="bg-slate-50 hover:bg-slate-400 py-2 px-2.5 flex items-center justify-center border-r"
                            @click="changeColor('bg-neutral-950')"
                        >
                            <div
                                class="h-4 w-4 rounded-full bg-neutral-950 border"
                            ></div>
                        </button>
                        <button
                            class="bg-slate-50 hover:bg-slate-400 py-2 px-2.5 flex items-center justify-center"
                            @click="changeColor('bg-red-500')"
                        >
                            <div
                                class="h-4 w-4 rounded-full bg-red-500 border"
                            ></div>
                        </button>
                    </div>
                </div>
                <div class="relative">
                    <button
                        class="bg-slate-50 hover:bg-slate-400 py-2 px-2 flex items-center justify-center border-t"
                        @click="rangeslider"
                    >
                        <img
                            src="../assets/iconForButton/9054959_bx_slider_alt_icon.svg"
                            width="20"
                        />
                    </button>
                    <div
                        v-if="showRangeslider"
                        class="rangeslider flex items-center justify-center"
                    >
                        <div class="flex items-center space-x-4">
                            <input
                                type="range"
                                v-model="sliderValue"
                                min="2"
                                max="9"
                                class="w-30 h-6 appearance-none bg-gray-300 rounded-full"
                                @input="updateOpacity"
                            />
                            <span class="text-sm font-semibold">{{
                                sliderValue
                            }}</span>
                        </div>
                    </div>
                </div>
                <button
                    class="bg-slate-50 hover:bg-slate-400 text-white font-bold py-2 px-2"
                    id="button3"
                    @click="exportGeoJSON"
                >
                    <img
                        src="../assets/iconForButton/2931171_download_import_save_down_storage_icon.svg"
                        width="20"
                    />
                </button>
                <button
                    class="bg-red-300 hover:bg-red-500 text-white font-bold py-2 px-2 rounded-b border-t"
                    @click="cleanMap"
                >
                    <img
                        src="../assets/iconForButton/2290850_clean_delete_garbage_recycle bin_trash_icon.svg"
                        width="20"
                    />
                </button>
            </div>
        </div>
    </div>
</template>

<style setup>
.map-wrap {
    position: relative;
    width: 100%;
    height: calc(100vh - 80px);
}

.map {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
}

.sidebar {
    position: absolute;
    top: 0;
    left: -187px;
    background-color: white;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: right 0.3s ease;
}

.sidebar.show {
    right: 0;
}

.rangeslider {
    position: absolute;
    top: 1px;
    left: -230px;
    background-color: white;
    border-radius: 4px;
    padding: 6px;
}
</style>
