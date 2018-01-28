<template>
<b-container>
    <b-row class=test>
        <b-col>
          <div>{{atmosphaereColor(1)}}</div>
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
        <b-col>2/3</b-col>
    </b-row>
</b-container>
</template>
235
<script>
import json from './process.json';
export default {
  name: "app",
  data() {
    return {
      msg: "Hi :>",
      json: json
    };
  },
  methods: {
    wasserZoom: function (childnr) {
      var anz;
      var proz;
      anz=json.process.childs[childnr].participants.length;
      switch (anz) {
        case 1:
              proz= "80 80";
              break;
        case 2:
              proz="100 100";
              break;
        case 3:
              proz="110 110";
              break;
        case 4:
              proz="120 120";
              break;
      }
    return proz;
    },
    atmosphaereColor: function(childnr){
      var sh;
      var chnr=childnr;
      var rgba;
      sh=json.process.childs[childnr].initiator.match(/\d+/g);
      chnr= chnr % 3;
      sh=sh % 3;
      switch (chnr){
        case 0:
          rgba="rgba(255,0,0,0.3)";
          break;
        case 1:
          rgba="rgba(255,255,0,0.3)";
          break;
        case 2:
          rgba="rgba(255,255,255,0.3)";
          break;
      }
      if(chnr == 1){
        switch (sh){
          case 0:
            rgba="rgba(0,0,255,0.3)";
            break;           
          case 1:
            rgba="rgba(255,0,0,0.3)";
            break;
          case 2:
            rgba="rgba(0,255,0,0.3)";
            break;
        }
      }
      if(chnr == 2){
        switch (sh){
          case 0:
            rgba="rgba(255,0,255,0.3)";
            break;           
          case 1:
            rgba="rgba(255,255,0,0.3)";
            break;
          case 2:
            rgba="rgba(0,255,255,0.3)";
            break;
        }
      }
      console.log(rgba);
      return rgba;
    }
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
