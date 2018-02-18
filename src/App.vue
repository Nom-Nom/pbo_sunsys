<template>
<b-container fluid>
  <header>
    <img class="openerPlanets" src="./assets/openerPlanets.png">
    <img class="openerSchrift" src="./assets/openerSchrift.png">
    <span class="author">von Hannes Liehr und Anna Krauß</span>
    <a  href=#sunSysView>
    <b-btn class="weiter runter">
      <img  class="pfeil" src="./assets/pfeil_r.svg">
    </b-btn>
    </a>
  </header>
    <b-row id=sunSysView class="site">
       <b-col cols="1" align-self="center" >
          <b-btn class="weiter zurueck" v-on:click="((initiator==0)? initiator=0 : initiator-=1), sonneFarbeRotation=0;">
            <img  class="pfeil" src="./assets/pfeil_r.svg">
          </b-btn>
        </b-col>
        <b-col>
          <div class="sonne">
            <img id="sonnenBild" v-on:click="sonneFarbeRotation+=45;" src="/src/assets/sonne.png" v-bind:style="sonneFarbe()">
          </div>
           <b-tooltip target="sonnenBild">Sonnensystem: {{sonnensysName(initiator)}}</b-tooltip>
          <div class="umlaufBahn" v-for="(item,index) in sunSys[initiator]" v-bind:style="ulb(index,100,200,60,30)">
          </div>
          <a href=#planetView class="planetAufUmlaufBahn" v-for="(item,index) in sunSys[initiator]" v-bind:style="lbTrafo(index, 100,200,60,30)" v-on:click="id=item">
            <div class=planetgroup>
              <div class="planet atmos hgAtmos">
              </div>
              <div class="planet gt" style="background-size:500%;" v-bind:style="grundTexturPosition(item)" >
              </div>
              <div class="planet gtFarbe" style="mix-blend-mode:overlay;" v-bind:style="gtFarbe(item)">
              </div>
              <div class="planet wasser" v-bind:style="wasserZoom(item)+''+wasserPosition(item)">
              </div>
              <div class="planet verlauf" v-bind:style="atmosphaereColor(item,true)">
              </div>
              <div class="planet atmos" v-bind:style="atmosphaereColor(item,false)">
              </div>
              <div class="planet ring" v-bind:style="ring(item)"> 
              </div>
            </div>        
          </a>
          <a  href=#planetView>
            <b-btn class="weiter runterSunSys">
              <img  class="pfeil" src="./assets/pfeil_r.svg">
            </b-btn>
          </a>
        </b-col>
        <b-col cols="1" align-self="center" >
          <b-btn class=weiter v-on:click="((initiator>=sunSys.length-1) ? initiator=sunSys.length-1 : initiator+=1), sonneFarbeRotation=0;">
            <img  class="pfeil" src="./assets/pfeil_r.svg">
          </b-btn>
        </b-col>
    </b-row>
    <b-row id="planetView" class="site">
        <b-col align-self="center" >
           <b-btn class="weiter zurueck" v-on:click="((id==0) ? id=0 : id-=1)">
             <img  class="pfeil" src="./assets/pfeil_r.svg">
           </b-btn>
        </b-col>
        <b-col>
        <a  href=#sunSysView>
          <b-btn class="weiter hoch">
            <img  class="pfeil" src="./assets/pfeil_r.svg">
          </b-btn>
        </a>
        <div class="planetgroup pView">
          <div class="planet atmos hgAtmos">
          </div>
          <div class="planet gt" style="background-size:500%;" v-bind:style="grundTexturPosition(id)" >
          </div>
          <div class="planet gtFarbe" style="mix-blend-mode:overlay;" v-bind:style="gtFarbe(id)">
          </div>
          <div class="planet wasser" v-bind:style="wasserZoom(id)+''+wasserPosition(id)">
          </div>
          <div class="planet verlauf" v-bind:style="atmosphaereColor(id,true)">
          </div>
          <div class="planet atmos" v-bind:style="atmosphaereColor(id,false)">
          </div>
          <div class="planet ring" v-bind:style="ring(id)"> </div>
        </div>
        <div class="plakette">
          Name: {{planetName(id)}} <br> Entdeckt am: {{foundPlanet(id)}}
        </div>
        </b-col>
        <b-col align-self="center" >
          <b-btn class=weiter v-on:click="((id>=43) ? id=43 : id+=1)">
            <img  class="pfeil" src="./assets/pfeil_r.svg">
          </b-btn>
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
      sonneFarbeRotation:0,
      msg: "Hi :>",
      json: json,
      id: 0,
      initiator: 0,
      planet:{
        gtPos:{x:0,y:0},
        gtFarbe:{r:0,g:0,b:0},
        wPos:{x:0,y:0},
        atmoFarbe:{r:0,g:0,b:0,a:0},
        csStyle:''
      }, 
      sunSys:[]
    };
  },
  beforeMount:function(){
        var maxAnzPlaneten=7;
        var AusgelagertePlaneten=0;
        var AusgelagerteSonnenSysNr=0;
        var AusgelagerteSonnenSys=[];
        AusgelagerteSonnenSys[0]=[];  
        var strrr;
        var nr, str, strl;
        for (var j=0; j<json.process.stakeholder.length;j++){
          this.sunSys[j]=[];
          var match=0;
          for(var i=0; i<json.process.childs.length;i++){
            str=json.process.childs[i].initiator;
            strl=str.length;
            nr=str.substr(strl-2,strl);
            nr=nr.match(/\d+/g);
            nr=parseInt(nr);
            if(j==nr){
            //console.log("MATCH(i,j,nr) "+i+" "+j+" "+nr);   
            if (match>maxAnzPlaneten){ 
              if(AusgelagertePlaneten>maxAnzPlaneten){
                AusgelagertePlaneten=0;
                AusgelagerteSonnenSysNr++;
                AusgelagerteSonnenSys[AusgelagerteSonnenSysNr]=[];
              }
              AusgelagerteSonnenSys[AusgelagerteSonnenSysNr][AusgelagertePlaneten]=i;
              AusgelagertePlaneten++;
            }
            else  {  
              //console.log("i:"+i+" IID(j): "+ j + " PlanetIID: " +nr);          
              this.sunSys[j][match]=i;
              match++;        
            }
            }
          }
        }

        //sortiere und lösche
        this.sunSys.sort();
        while (this.sunSys[0][0]==null){
          this.sunSys.shift();
        }
        //console.log(sunSys);
        //console.log(AusgelagerteSonnenSys);
        this.sunSys=this.sunSys.concat(AusgelagerteSonnenSys);
        //console.log(sunSys);
  },
  methods: {
    sonneFarbe:function(){
      var str = this.sonneFarbeRotation+(this.initiator*22.5);
      return "filter:hue-rotate("+ str +"deg);";
    },
    ulb:function(iterator, diffBH, breite, abstandb,abstandh){
      
      var b=(breite+iterator*abstandb)*2;
      var h=(b-diffBH*2)-abstandh*iterator*2;   

      //console.log("Laufbahn("+iterator+") h:"+ h+"  breite:"+b);
      return " height:" + h + "px; width:" + b + "px; border-radius:" + b + "px/" + h + "px; transform: translateX(-50%) translateY(-50%);";
    },
    lbTrafo: function(iterator, diffBH, breite, abstandb,abstandh){
      

      diffBH=diffBH+abstandh*iterator; 
      breite=breite+abstandb*iterator;
      
      //console.log("Planet("+iterator+") diffBH:"+ diffBH+"("+(breite*2-diffBH*2)+")  breite:"+breite*2);

      var grad=((360/this.sunSys[this.initiator].length)*(iterator));
      var r=(breite-diffBH*(Math.abs(Math.sin(grad* Math.PI / 180.0))));
      
      var scale=(Math.sin(grad* Math.PI / 180.0)+2)/3;
      if (grad>180){
        scale=scale-iterator*0.01
      } else {
        scale=scale+iterator*0.05
      }
      scale*=0.3
      //console.log("Planet("+iterator+") scale: "+scale);
      return "transform:translateX(-50%) translateY(-50%) rotate(" + grad + "deg) translate(" + r + "px) rotate(-" + grad + "deg) scale("+scale+");";
    },
    wasserZoom: function(childnr) {
      var anz;
      var proz;
      //console.log(childnr);
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
        default:
          proz= "background-size:250% 250%;";
          break;
      }
      //console.log(proz);
      return proz;
    },
    atmosphaereColor: function(childnr, hg) {
      var sh;
      var chnr = childnr;
      var r,g,b=0;
      var rgba;
      var trans = 0.1;
      var light = 150;
      sh = json.process.childs[childnr].initiator.match(/\d+/g);
      chnr = chnr % 3;
      sh = sh % 3;
      switch (chnr) {
        case 0:   r=255;  g=light;  b=light;  break;
        case 1:   r=255;  g=255;    b=light;  break;
        case 2:   r=255;  g=255;    b=255;    break;
      }
      if (chnr == 1) {
        switch (sh) {
          case 0:   r=light;  g=light;  b=255;    break;
          case 1:   r=255;    g=light;  b=light;  break;
          case 2:   r=light;  g=255;    b=light;  break;
        }
      }
      if (chnr == 2) {
        switch (sh) {
          case 0:   r=255;    g=light;  b=255;    break;
          case 1:   r=255;    g=255;    b=light;  break;
          case 2:   r=light;  g=255;    b=255;    break;
        }
      }
      if (hg==true){
        r-=light;
        g-=light;
        b-=light;
        rgba =   "background: radial-gradient( circle farthest-side at 0px 0px, rgba(0, 0, 0, 0) 0%, rgba("+r+", "+g+", "+b+", 1)90%);";
        return rgba;
      } else {
        rgba ="background-color:rgba("+r+","+g+","+b+","+trans+");";
        return rgba;
      }
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
      //console.log(wasserpos);
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
      proz1 = count*count*count % 100;
      proz2 = (count*count % 51) * 2;
      //console.log("x: "+proz1+"% y:"+proz2+"%");
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
    },
    sonnensysName: function(childNr) {
      var name;
      var sth = json.process.childs[childNr].name;
      name = sth[0] + "-";

      if (sth.length == 4) {
        name += sth[2] + sth[3];
      } else if (sth.length <= 7) {
        for (var i = 1; i < sth.length; i++) {
          if (i == 3 || i == 5 || i == sth.length - 1) {
            if (sth[i] == "(" || sth[i] == ")") name += "-";
            else name += sth[i];
          }
        }
      } else if (sth.length > 7) {
        for (var i = 1; i < sth.length; i++) {
          if (i == 3 || i == 5 || i == 7 || i == sth.length - 1) {
            if (sth[i] == "(" || sth[i] == ")") name += "-";
            else name += sth[i];
          }
        }
      }
      name += childNr;
      return name;
      //console.log(name);
    },
    ring: function(planetid){
      var type= json.process.childs[planetid].transformation.type;
      if(json.process.childs[planetid].transformation.decision == "true" && type == ">"){
        //console.log("ring");
        return "visibility: visible";
      } 
    },
    foundPlanet: function(planetid){
      var found=json.process.childs[planetid].start;
      var foundy=found.substr(0,4);
      var foundm=found.substr(5,2);
      var foundd=found.substr(8,2);
      found=foundd+"."+foundm+"."+foundy;
      return found;
    },
    planetName: function(childNr) {
      var name;
      var sth = json.process.childs[childNr].name;
      name = sth[0]+ sth[2]+sth[1]+"-";

      if (sth.length == 4) {
        name += sth[3] + sth[2];
      } else if (sth.length <= 7) {
        for (var i = sth.length; i >0; i--) {
          if (i == 3 || i == 5 || i == sth.length - 1) {
            if (sth[i] == "(" || sth[i] == ")") name += "-";
            else name += sth[i];
          }
        }
      } else if (sth.length > 7) {
        for (var i = sth.length; i >0; i--) {
          if (i == 3 || i == 5 || i == 7 || i == sth.length - 1) {
            if (sth[i] == "(" || sth[i] == ")") name += "-";
            else name += sth[i];
          }
        }
      }
      name += childNr;
      name=name.replace(/[ .]*/g,'');
      return name;
      //console.log(name);
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
.planetAufUmlaufBahn{
    position: absolute;
    border-radius: 50%;
    text-align:center;
    left:50%;
    top:50%;
}
.umlaufBahn{
  border-style: dashed;
  border-width: 1px;
  border-color: rgb(72, 72, 72);
  position: absolute;
  left:50%;
  top:50%;
  z-index: -2;
}
.sonne{
  top: 50%;
  transform: translateY(-50%);
  position: relative;
  margin: auto;
  display: box;
  text-align:center;
}
.planetgroup {
  margin-top: 10% !important;
  top: 50%;
  position: relative;
  margin: auto;
  width: 170px;
  height: 170px;
  z-index: -1;
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
.ring{
  width: 100%;
  height: 100%;
  top:10%;
  left:10%;
  z-index: 4;
  visibility: hidden;
  background-image: url(/src/assets/ring.svg);
  background-repeat: no-repeat;
  border-radius: 0;
  transform: scale(1.7);
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
  mix-blend-mode: difference;
  background: radial-gradient(
    circle farthest-side at 0px 0px,
    rgba(0, 0, 0, 0) 0%,
    rgba(0, 0, 0, 1)100%
  );
  z-index: 2;
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

.zurueck {
  margin-left: 100%;
  transform: translateX(-100%) rotate(180deg);
}
body {
  overflow-x: hidden;
  background-color: rgb(12, 11, 16);
}
.site{
  height: 100vh;
}
#sonnenBild{
  width: 200px;
  height: 200px;
  z-index: 11;
  cursor: pointer;
}
.pView{
  transform:scale(1.5) translateY(-50%);
}
.weiter{
  background-color: #E72C7C !important;
  border-color: rgba(0,0,0,0.6) !important;
  border-width: 3px !important;
  opacity: 0.5;
}
.weiter:hover{
  opacity: 1;
}
.weiter:visited{
  opacity: 0.5;
}
.pfeil{
  width: 25px;
  height: 35px;
}
.runter{
  transform: translateX(-50%) rotate(90deg);
  margin-top:86vh;
  position: absolute;
  z-index: 5;
}
.hoch{
  transform: translateX(-50%) rotate(-90deg);
  margin-top:10%;
  position: absolute;
  left:50%;
}
.runterSunSys{
  transform: translateX(-50%)  rotate(90deg) ;
  left:50%;
  position:absolute;
  top:90%;
}
.plakette{
  background-image: url(/src/assets/schildchen.svg);
  background-repeat: no-repeat;
  color:#301C47;
  font-weight:bold;
  font-size: 150%;
  text-align: center;
  position: absolute;
  width: 100%;
  min-width: 500px;
  padding-top:0.7em;
  top:80%;
  height: 120px;
}
</style>
