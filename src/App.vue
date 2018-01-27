<template>
<b-container>
    <b-row class=test>
        <b-col>
          <b-list-group>
            <b-list-group-item>Sonnensystem:</b-list-group-item>
            <b-list-group-item>Anzahl der Planeten</b-list-group-item>
            <b-list-group-item>
              Sonnensystem wechseln?
              <b-button>zurück</b-button>
              <b-button>vor</b-button>
            </b-list-group-item>
        </b-col>
        <b-col>
          <div class=planetgroup>
            <div class=planet style="background-image:url();z-index=1;">
              </div>
              <div class=planet style="background-image:url();z-index=2;">
              </div>
              <div class=planet style="background-image:url();z-index=3;background-size:500px;background-position:100px 300px">
              </div>
              <div class=planetAtmos style="">
              </div>
            </div>
        </b-col>
        <b-col>3 of 4</b-col>
        <b-col>4 of 4</b-col>
    </b-row>
</b-container>
</template>
235
<script>
export default {
  name: "app",
  data() {
    return {
      msg: "Hi :>"
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.planetgroup {
  position: relative;
  top: 75px;
  left: 75px;
  width: 150px;
  height: 150px;
}
.planet {
  border-radius: 50%;
  position: absolute;
  top: 5px;
  left: 5px;
  width: 150px;
  height: 150px;
  background-size: 150px;
}
.planetAtmos {
  border-radius: 50%;
  position: absolute;
  top: 0px;
  left: 0px;
  width: 160px;
  height: 160px;
  background-color: rgba(255, 0, 0, 0.2);
  filter: blur(3px);
  mix-blend-mode:multiply;
}
</style>
