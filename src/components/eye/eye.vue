<template>
      <div id="eye"></div>
</template>

<script>
export default {
  name: "eye",
  props:{
      message:{
          type:Object,
          default(){
              return{}
          }
      }
  },
  mounted() {
      console.log(this.message)
//       //鹰眼图
//     let viewer2 = new Cesium.Viewer("eye", {
//       animation: false,
//       baseLayerPicker: true,
//       fullscreenButton: false,
//       geocoder: false,
//       homeButton: false,
//       sceneModePicker: false,
//       selectionIndicator: false,
//       timeline: false,
//       navigationHelpButton: false,
//       infoBox: false,
//       navigationInstructionsInitiallyVisible: false,
//     });
//         viewer2._cesiumWidget._creditContainer.style.display = "none"; //去掉logo
//     viewer2.baseLayerPicker.viewModel.selectedImagery =
//       viewer2.baseLayerPicker.viewModel.imageryProviderViewModels[3];
//     //设置鹰眼图中球属性
//     let control = viewer2.scene.screenSpaceCameraController;
//     control.enableRotate = false;
//     control.enableTranslate = false;
//     control.enableZoom = false;
//     control.enableTilt = false;
//     control.enableLook = false;
//     let syncViewer = function () {
//       viewer2.camera.flyTo({
//         destination: viewer.camera.position,
//         orientation: {
//           heading: viewer.camera.heading,
//           pitch: viewer.camera.pitch,
//           roll: viewer.camera.roll,
//         },
//         duration: 0.0,
//       });
//     };
//         //同步
//     viewer.entities.add({
//       position: Cesium.Cartesian3.fromDegrees(0, 0),
//       label: {
//         text: new Cesium.CallbackProperty(function () {
//           syncViewer();
//           return "";
//         }, true),
//       },
//     });
  },
}
</script>

<style>

</style>