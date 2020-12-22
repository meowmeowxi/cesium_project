<template>
  <div id="cesiumContainer"></div>
</template>
<script>
// coder: miaomiaoxi
// update：2020/12/21
window.CESIUM_BASE_URL = "/static/Cesium/";
import * as Cesium from "cesium";
import "cesium/Build/Cesium/Widgets/widgets.css";
export default {
  name: "HelloWorld",
  mounted() {
    // Keep your Cesium.Ion.defaultAccessToken = 'your_token_here' line above. 
    Cesium.Ion.defaultAccessToken =
      "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJlNjAxOTNjMC05NWNmLTQ2N2EtYjJmMS0wZjllYTU2YTZkNGMiLCJpZCI6Mzk5MTEsImlhdCI6MTYwODA5MDQzMX0.2ZuBc4istFVN7wWweDEIR4_6WKziYvophGmFkv8Hl2Q";
    // Initialize the viewer with Cesium World Terrain.
    this.viewer = new Cesium.Viewer("cesiumContainer", {
      terrainProvider: Cesium.createWorldTerrain(),
    });
    // Add Cesium OSM Buildings, a global 3D buildings layer.
    this.buildingTileset = this.viewer.scene.primitives.add(
      Cesium.createOsmBuildings()
    );
    // Hide individual buildings in this area using 3D Tiles Styling language. 
    this.buildingTileset.style = new Cesium.Cesium3DTileStyle({
      // Create a style rule to control each building's "show" property.
      show: {
        conditions: [
          // Any building that has this elementId will have `show = false`.
          ["${elementId} === 532245203", false],
          ["${elementId} === 332469316", false],
          ["${elementId} === 332469317", false],
          ["${elementId} === 235368665", false],
          ["${elementId} === 530288180", false],
          ["${elementId} === 530288179", false],
          // If a building does not have one of these elementIds, set `show = true`.
          [true, true],
        ],
      },
      // Set the default color style for this particular 3D Tileset.
      // For any building that has a `cesium#color` property, use that color, otherwise make it white.
      color:
        "Boolean(${feature['cesium#color']}) ? color(${feature['cesium#color']}) : color('#ffffff')",
    });
    // Add the 3D Tileset I created from my Cesium ion account.
    this.newBuildingTileset = this.viewer.scene.primitives.add(
      new Cesium.Cesium3DTileset({
        url: Cesium.IonResource.fromAssetId(227908),
      })
    );
    this.newBuildingTileset = this.viewer.scene.primitives.add(
      new Cesium.Cesium3DTileset({
        url: Cesium.IonResource.fromAssetId(228284),
      })
    );
    // Move the camera to the new building.
    this.viewer.camera.flyTo({
      destination: Cesium.Cartesian3.fromDegrees(118.956, 32.112, 100),
      orientation: {
        heading: 0.13757405770819275,
        pitch: -0.1733178889216822,
        roll: 0.00042066523097261665,
      },
      flyOverLongitude: Cesium.Math.toRadians(60.0),
    });
    
  },
  data() {
    return {};
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
