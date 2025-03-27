

<template>
  <div id="map" class="map-container"></div>
  <el-button @click="addMarker">添加图标</el-button>
  <el-button @click="removeMarker">删除图标</el-button>
  <el-button @click="addPloygon">添加多边形</el-button>
  <el-button @click="removePloygon">删除多边形</el-button>
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
const latlngs = [
  [30.43, 120.52],
  [30.48, 120.6],
  [30.39, 120.72],
];
const markerList = ref([]);
const addMarker = () => {
  let htmlData = `<div class='info-window'>
    <div class='title'>信息弹窗</div>
  </div>`;
  markerList.value = [];
  posList.value.forEach((item) => {
    let icon = BM.icon({
      iconUrl: markerIcon,
    });
    let marker = BM.marker(item, {
      icon: icon,
    }).addTo(map);
    marker.on("mouseover", function (e) {
      let popup = BM.popup({
        offset: BM.point(11, 0),
      })
        .setLatLng(item)
        .setContent(htmlData)
        .openOn(map);
    });
    markerList.value.push(marker);
  });
};
const removeMarker = () => {
  markerList.value.forEach((item) => {
    item.remove(map);
  });
};
const polygonList = ref([]);
const addPloygon = () => {
  polygonList.value.push(BM.polygon(latlngs, { color: "red" }).addTo(map));
};
const removePloygon = () => {
  polygonList.value.forEach((item) => {
    item.remove(map);
  });
};
</script>
<style scoped>
.map-container {
  width: 100%;
  height: 90vh;
  background: #c5c5c5;
}
:deep(.info-window) {
  width: 320px;
  height: 238px;
  background: url("@/assets/info-box2.png") no-repeat;
  background-size: 100% 100%;
  .title {
    color: #fff;
    font-size: 16px;
    padding: 20px 0 0 50px;
  }
}
:deep(.bigemap-popup-content) {
  padding: 0;
}
:deep(.bigemap-popup-close-button) {
  position: absolute;
  top: 20px;
  right: 4px;
  background: url(@/assets/close-circle.png) no-repeat;
  background-size: 100% 100%;
  width: 20px;
  height: 20px;
}
:deep(.bigemap-popup-content-wrapper) {
  padding-bottom: 0;
  background: transparent;
  border: none;
  box-shadow: none;
}
</style>
