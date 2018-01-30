<template>
<b-container fluid>
  <header>
    <img class="openerPlanets" src="./assets/openerPlanets.png">
    <img class="openerSchrift" src="./assets/openerSchrift.png">
    <span class="author">von Hannes Liehr und Anna Krauß</span>
  </header>
    <b-row>
        <b-col align-self="center" >
          <b-btn class="weiter zurueck" v-on:click="id-=1">&lsaquo;</b-btn>
        </b-col>
        <b-col>
        <div class=planetgroup>
          <div class="planet atmos hgAtmos">
          </div>
          <div class="planet gt" style="background-size:500%;" v-bind:style="grundTexturPosition(id)" >
          </div>
          <div class="planet gtFarbe" style="mix-blend-mode:overlay;" v-bind:style="gtFarbe(id)">
          </div>
          <div class="planet wasser" v-bind:style="wasserZoom(id)+''+wasserPosition(id)">
          </div>
          <div class="planet verlauf" v-bind:style="atmosphaereColor(id/*,true*/)">
          </div>
          <div class="planet atmos" v-bind:style="atmosphaereColor(id/*,false*/)">
          </div>
        </div>
        </b-col>
        <b-col align-self="center" >
          <b-btn class=weiter v-on:click="id+=1">&rsaquo;</b-btn>
        </b-col>
    </b-row>
</b-container>
</template>
<script>
import json from "./process.json";
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
    wasserZoom: function(childnr) {
      var anz;
      var proz;
      anz = json.process.childs[childnr].participants.length;
      switch (anz) {
        case 1:
          proz = "background-size:200% 200%;";
          break;
        case 2:
          proz = "background-size:300% 300%;";
          break;
        case 3:
          proz = "background-size:400% 400%;";
          break;
        case 4:
          proz = "background-size:500% 500%;";
          break;
      }
      return proz;
    },
    atmosphaereColor: function(childnr/*, hg*/) {
      var sh;
      var chnr = childnr;
      var rgba;
      var trans = 0.1;
      var light = 150;
      sh = json.process.childs[childnr].initiator.match(/\d+/g);
      chnr = chnr % 3;
      sh = sh % 3;
      switch (chnr) {
        case 0:
          rgba =
            "background-color:rgba(255," +
            light +
            "," +
            light +
            "," +
            trans +
            ");";
          break;
        case 1:
          rgba = "background-color:rgba(255,255," + light + "," + trans + ");";
          break;
        case 2:
          rgba = "background-color:rgba(255,255,255," + trans + ");";
          break;
      }
      if (chnr == 1) {
        switch (sh) {
          case 0:
            rgba =
              "background-color:rgba(" +
              light +
              "," +
              light +
              ",255," +
              trans +
              ");";
            break;
          case 1:
            rgba =
              "background-color:rgba(255," +
              light +
              "," +
              light +
              "," +
              trans +
              ");";
            break;
          case 2:
            rgba =
              "background-color:rgba(" +
              light +
              ",255," +
              light +
              "," +
              trans +
              ");";
            break;
        }
      }
      if (chnr == 2) {
        switch (sh) {
          case 0:
            rgba =
              "background-color:rgba(255," + light + ",255," + trans + ");";
            break;
          case 1:
            rgba =
              "background-color:rgba(255,255," + light + "," + trans + ");";
            break;
          case 2:
            rgba =
              "background-color:rgba(" + light + ",255,255," + trans + ");";
            break;
        }
      }
      // if (hg==true){
      //   rgba=rgba.match(/([\d{}+]*,){3}/g)
      //   rgba=rgba.split(",", 3);
      //   console.log(rgba+"Hallo")
      // }
       return rgba;
    },
    wasserPosition: function(childnr) {
      var count = 0;
      var proz1 = 0;
      var proz2 = 0;
      var wasserpos;
      var str = json.process.childs[childnr].description;
      for (var i = 0; i < str.length; i++) {
        count += str.charCodeAt(i);
        //console.log(count);
      }
      proz1 = count % 100;
      //console.log(proz1);
      proz2 = (proz1 % 51) * 2;
      //console.log(proz2);
      wasserpos = "background-position: " + proz1 + "% " + proz2 + "%;";

      return wasserpos;
    },
    grundTexturPosition: function(childnr) {
      var count = 0;
      var proz1 = 0;
      var proz2 = 0;
      var texturpos;
      var str = json.process.childs[childnr].start;
      for (var i = 0; i < str.length; i++) {
        count += str.charCodeAt(i);
        //console.log(count);
      }
      proz1 = count % 100;
      //console.log(proz1);
      proz2 = (proz1 % 51) * 2;
      //console.log(proz2);
      texturpos = "background-position: " + proz1 + "% " + proz2 + "%;";

      return texturpos;
    },
    gtFarbe: function(childnr) {
      var count1 = 1;
      var count2 = 1;
      var count3 = 1;
      var rgb;
      var proz1 = 0;
      var proz2 = 0;
      var gtFarbe;
      var split = json.process.childs[childnr].name.length / 3;
      split = parseInt(split);
      var str1 = json.process.childs[childnr].name.substr(0, split - 1);
      var str2 = json.process.childs[childnr].name.substr(
        split - 1,
        split * 2 - 1
      );
      var str3 = json.process.childs[childnr].name.substr(
        split * 2 - 1,
        json.process.childs[childnr].name.length
      );
      for (var i = 0; i < str1.length; i++) {
        if (str1.charCodeAt(i) != 0) count1 += str1.charCodeAt(i);
      }
      for (var i = 0; i < str2.length; i++) {
        if (str2.charCodeAt(i) != 0) count2 += str2.charCodeAt(i);
      }
      for (var i = 0; i < str3.length; i++) {
        if (str3.charCodeAt(i) != 0) count3 += str3.charCodeAt(i);
      }
      count1 = count1 % 256;
      count2 = count2 % 256;
      count3 = count3 % 256;
      rgb =
        "background-color:rgb(" + count1 + "," + count2 + "," + count3 + ");";
      return rgb;
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
  margin-top: 10% !important;
  top: 50%;
  position: relative;
  margin: auto;
  width: 170px;
  height: 170px;
  transform: translateY(-50%);
}
.planet {
  border-radius: 50%;
  position: absolute;
  top: 0%;
  left: 0%;
  width: 100%;
  height: 100%;
  background-size: 150px;
}
.atmos {
  top: -5%;
  left: -5%;
  width: 110%;
  height: 110%;
  mix-blend-mode: multiply;
  z-index: 4;
}
.hgAtmos {
  background: radial-gradient(
    circle farthest-side at 0px 0px,
    rgba(255, 255, 255, 0.1) 0%,
    rgba(255, 255, 255, 0.01)100%
  );
  top: -15%;
  left: -15%;
  width: 130%;
  height: 130%;
  z-index: 1 !important;
}
.verlauf {
  mix-blend-mode: normal;
  background: radial-gradient(
    circle farthest-side at 0px 0px,
    rgba(0, 0, 0, 0) 0%,
    rgba(20, 0, 0, 1)100%
  );
  z-index: 3;
}
.gt {
  background-image: url(/src/assets/grundTextur.svg);
  z-index: 1;
}
.gtFarbe {
  z-index: 1;
}
.wasser {
  background-image: url(/src/assets/wasserTextur.svg);
  z-index: 2;
}
header {
  background: radial-gradient(
    circle farthest-side at 50% 0px,
    #330d48 0%,
    #061840 64%,
    #05143a 100%
  );
  height: 100vh;
  text-align: center;
}
.openerPlanets {
  position: absolute;
  display: block;
  margin-left: auto;
  margin-right: auto;
  left: 40px;
  right: 0;
  padding-top: 15vh;
  max-width: 760px;
  max-height: 100vh;
  z-index: 1;
}
.openerSchrift {
  position: absolute;
  display: block;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  max-height: 100vh;
  max-width: 760px;
  z-index: 2;
}
.container-fluid {
  padding-left: 0px !important;
  padding-right: 0px !important;
  margin-left: 0px !important;
  margin-right: 0px !important;
}
.author {
  top: 95vh;
  position: absolute;
  display: block;
  z-index: 3;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  color: #3462ac;
}
.weiter {
}
.zurueck {
  margin-left: 100%;
  transform: translateX(-100%);
}
body {
  overflow-x: hidden;
  background-color: rgb(12, 11, 16);
}
</style>
