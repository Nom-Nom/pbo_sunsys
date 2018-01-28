<template>
<b-container-fluid>
  <header>
    <img class="openerPlanets" src="./assets/openerPlanets.png">
    <img class="openerSchrift" src="./assets/openerSchrift.png">
    <span class="author">von Hannes Liehr und Anna Krauß</span>
  </header>
    <b-row class=test>
        <b-col>
          <div>{{atmosphaereColor(1)}}</div>
        </b-col>
        <b-col>
        <div class=planetgroup>
          <b-button v-on:click="id+=1">Hi</b-button>
          <div class=planet style="background-image:url();z-index=1;">
            </div>
            <div class=planet style="background-image:url();z-index=2;">
            </div>
            <div class=planet style="background-image:url();z-index=3;background-size:500px;background-position:100px 300px" v-bind:style="atomosphaereColor(id)">
            </div>
            <div class=planetAtmos style="">
            </div>
            </div>
        </b-col>
        <b-col>2/3</b-col>
    </b-row>
</b-container-fluid>
</template>
<script>
import json from './process.json';
export default {
  name: "app",
  data() {
    return {
      msg: "Hi :>",
      json: json,
      id: 1
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
    
    // Nui hat quatsch gemacht 
    atomosphaereColor: function(value) {
      if (value == 1) {
        return "background-color:rgba(255," + 255 + ",0, 0.2)";
      }
      if (value == 2) {
        return "background-color:rgba(255," + 0 + ",0, 0.2)";
      }
      if (value == 3) {
        return "background-color:rgba(255," + 60 + ",0, 0.2)";
      }
      if (value == 4) {
        return "background-color:rgba(255," + 170 + ",0, 0.2)";
      }
    },
    // Bis hier hin :>

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
  filter: blur(3px);
  mix-blend-mode: multiply;
  background-color: rgba(255, 0, 0, 0.2);
}
header{
  background: radial-gradient(
  circle farthest-side at 50% 0px,
    #330d48 0%,
    #061840 64%,
    #05143a 100%
  );
  height: 100vh;
  text-align: center;
}
.openerPlanets{
  position: absolute;
  display:block;
  margin-left: auto;
  margin-right: auto;
  left: 40px;
  right: 0;
  padding-top:15vh;
  max-width: 760px;
  z-index: 1;
}
.openerSchrift{
  position: absolute;
  display:block;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  max-width: 760px;
  z-index: 2;
}
.container-fluid{
  padding-left: 0px !important;
  padding-right: 0px !important;
  margin-left: 0px !important;
  margin-right: 0px !important;
}
.author{
  top: 95vh;
  position: absolute;
  display:block;
  z-index: 3;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  color: #3462ac;
}
body{
  overflow-x: hidden;
}
</style>
