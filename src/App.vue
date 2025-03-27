

<template>
  <div id="map" class="map-container"></div>
  <el-button @click="addMarker">添加图标</el-button>
  <el-button @click="removeMarker">删除图标</el-button>
</template>
<script setup>
import { onMounted, ref } from "vue";
import markerIcon from "@/assets/target1.png";
const BM = window.BM;
var map;
onMounted(() => {
  BM.Config.HTTP_URL = "http://127.0.0.1:9000";
  BM.accessToken =
    "pk.eyJ1IjoiY3VzX2E3anZoZGgxIiwiYSI6ImR5NGJsdjFnMzEyYWFwOGlvZmNnc3EweTAifQ.on7bt4ZWNcFmfF8lPvTGsw";
  map = BM.map("map", "bigemap.b8gnlgem", {
    center: [30.427770614624023, 120.58232879638672],
    zoom: 15,
    zoomControl: true,
  });
  map.fitBounds([
    [30.264814376831055, 120.30171203613281],
    [30.590726852416992, 120.8629379272461],
  ]);
});
const posList = ref([
  [30.49, 120.66],
  [30.47, 120.71],
]);
const markerList = ref([]);
const addMarker = () => {
  markerList.value = [];
  posList.value.forEach((item) => {
    let icon = BM.icon({
      iconUrl: markerIcon,
    });
    markerList.value.push(
      BM.marker(item, {
        icon: icon,
      })
    );
    BM.marker(item, {
      icon: icon,
    }).addTo(map);
  });
};
const removeMarker = () => {
  markerList.value.forEach((item) => {
    item.remove(map);
  });
};
</script>
<style scoped>
.map-container {
  width: 100%;
  height: 90vh;
}
</style>
