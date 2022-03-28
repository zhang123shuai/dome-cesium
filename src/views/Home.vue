<template>
  <div class="home">
    <div id="cesiumContainer"></div>
    <!-- <div id="eye"></div> -->
    <div class="eye">
      <veye :message="viewer"></veye>
    </div>
  </div>
</template>

<script>
import veye from '@/components/eye/eye.vue';
export default {
  name: "Home",
  data() {
    return {};
  },
  components: {veye},
  mounted() {
    var viewer = new Cesium.Viewer("cesiumContainer", {
      geocoder: false, //一种地理位置搜索工具，用于显示相机访问的地理位置。默认使用微软的Bing地图。
      homeButton: false, // 首页位置，点击之后将视图跳转到默认视角。
      sceneModePicker: false, // 切换2D、3D 和 Columbus View (CV) 模式。
      baseLayerPicker: true, //选择三维数字地球的底图（imagery and terrain）。
      navigationHelpButton: false, // 帮助提示，如何操作数字地球。
      animation: false, //控制视窗动画的播放速度。
      timeline: false, //展示商标版权和数据源。
      fullscreenButton: false, //展示当前时间和允许用户在进度条上拖动到任何一个指定的时间。
      vrButton: false, //视察全屏按钮。
    });
    this.getClickPointAdd(viewer); //获取当前鼠标点击位置坐标，并添加到图上显示
    viewer._cesiumWidget._creditContainer.style.display = "none"; //版权控件的显示隐藏
    //设置初始位置
    viewer.camera.setView({
      destination: Cesium.Cartesian3.fromDegrees(110.2, 34.55, 20000000),
    });
    //设置默认地图源
    viewer.baseLayerPicker.viewModel.selectedImagery =
      viewer.baseLayerPicker.viewModel.imageryProviderViewModels[3];
    //鹰眼图
    // let viewer2 = new Cesium.Viewer("eye", {
    //   animation: false,
    //   baseLayerPicker: true,
    //   fullscreenButton: false,
    //   geocoder: false,
    //   homeButton: false,
    //   sceneModePicker: false,
    //   selectionIndicator: false,
    //   timeline: false,
    //   navigationHelpButton: false,
    //   infoBox: false,
    //   navigationInstructionsInitiallyVisible: false,
    // });
    // viewer2._cesiumWidget._creditContainer.style.display = "none"; //去掉logo
    // viewer2.baseLayerPicker.viewModel.selectedImagery =
    //   viewer2.baseLayerPicker.viewModel.imageryProviderViewModels[3];
    //设置鹰眼图中球属性
    // let control = viewer2.scene.screenSpaceCameraController;
    // control.enableRotate = false;
    // control.enableTranslate = false;
    // control.enableZoom = false;
    // control.enableTilt = false;
    // control.enableLook = false;
    // let syncViewer = function () {
    //   viewer2.camera.flyTo({
    //     destination: viewer.camera.position,
    //     orientation: {
    //       heading: viewer.camera.heading,
    //       pitch: viewer.camera.pitch,
    //       roll: viewer.camera.roll,
    //     },
    //     duration: 0.0,
    //   });
    // };
    //同步
    // viewer.entities.add({
    //   position: Cesium.Cartesian3.fromDegrees(0, 0),
    //   label: {
    //     text: new Cesium.CallbackProperty(function () {
    //       syncViewer();
    //       return "";
    //     }, true),
    //   },
    // });
  },
  methods: {
    // 调用
    /**
     * @description: 获取当前鼠标点击位置坐标，并添加到图上显示
     * @param {*} _viewer
     * @return {*}
     */
    getClickPointAdd(_viewer) {
      // 注册屏幕点击事件
      let handler = new Cesium.ScreenSpaceEventHandler(_viewer.scene.canvas);
      handler.setInputAction(function (event) {
        // 转换为不包含地形的笛卡尔坐标
        let clickPosition = _viewer.scene.camera.pickEllipsoid(event.position);
        // 转经纬度（弧度）坐标
        let radiansPos = Cesium.Cartographic.fromCartesian(clickPosition);
        // 转角度
        console.log(
          "经度：" +
            Cesium.Math.toDegrees(radiansPos.longitude) +
            ", 纬度：" +
            Cesium.Math.toDegrees(radiansPos.latitude)
        );
        // 添加点
        _viewer.entities.add({
          position: clickPosition,
          //自带圆点坐标
          point: {
            color: Cesium.Color.YELLOW,
            pixelSize: 20,
          },
          label: {
            text: "花里胡哨", //坐标名字
            font: "12px", //字体样式
            fillColor: Cesium.Color.RED, //字体颜色
            backgroundColor: Cesium.Color.AQUA, //背景颜色
            showBackground: false, //是否显示背景颜色
            style: Cesium.LabelStyle.FILL, //label样式
            outlineWidth: 2, //线宽
            pixelOffset: new Cesium.Cartesian2(1, 15), //像素偏移量
            verticalOrigin: Cesium.VerticalOrigin.TOP, //垂直位置
            horizontalOrigin: Cesium.HorizontalOrigin.CENTER, //水平位置
          },
        });
      }, Cesium.ScreenSpaceEventType.LEFT_CLICK);
    },
  },
};
</script>
<style lang="scss" scoped>
.home {
  width: 100%;
  height: 100%;
  #cesiumContainer {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
    overflow: hidden;
    /deep/ .cesium-viewer-toolbar {
      display: none !important;
    }
  }
  .eye {
    position: absolute;
    width: 20%;
    height: 20%;
    bottom: 0;
    right: 0;
    z-index: 999;
    background: red;
    border: solid blue 1px;
    /deep/ .cesium-viewer-toolbar {
      display: none !important;
    }
  }
}
</style>
