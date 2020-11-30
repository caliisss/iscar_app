<template>
  <div class="hello">
    <h3>Iscar Calaminas</h3> <br>
  <div class="container" >
    <form class="form" action="">
      <fieldset class="form-fieldset ui-input __first">
        <input type="text" id="totalhorizontal" tabindex="0" v-model="totalhorizontal"/>
        <label for="totalhorizontal">
          <span data-text="totalhorizontal">Horizontal</span>
        </label>
      </fieldset>
      <fieldset class="form-fieldset ui-input __fourth">
        <input type="vertical" id="vertical" v-model="vertical"/>
        <label for="vertical">
          <span data-text="vertical">Vertical</span>
        </label>
      </fieldset>
      <fieldset class="form-fieldset ui-input __second">
        <input type="media" id="media" tabindex="0" v-model="media"/>
        <label for="media">
          <span data-text="media">Media Horizontal</span>
        </label>
      </fieldset>
    </form>
    <button class="btn" type="submit" v-on:click="chartsAmcore()">Calcular</button>
  </div>
    <br>
    <div id="container"></div><br>
    <div id="chartdiv"></div><br>
    <div id="chartdivInverse"></div><br>
  </div>
</template>

<script>

/////
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";

am4core.useTheme(am4themes_animated);

export default {
    props: {
      msg: String
    },
    data(){
      return{
          totalhorizontal:0,
          media:0,
          vertical:0,
          datos:[{
            vertical: 0,
            start: 0,
            end: 0,
          }]
      }
    },
    computed:{
      horizontal(){
        var totalhorizontal=this.totalhorizontal;
        var totalH=totalhorizontal/0.90;
        return totalH;
      },
      verticaltotal(){
        var vertical=this.vertical;
         var totalV=vertical;
         return totalV;
      },
      mediaM(){
        var media= this.media;
         return media;
      },
      mediadiv(){
       // var arrayEnd = [];
       var data = [];
       data.push({datos: []});
       var cont=1;
        var resto = this.totalhorizontal-this.mediaM;
         if (this.totalhorizontal>resto) {
           for (var i = this.mediaM; i <= this.totalhorizontal; i++) {
            data[0].datos.push({vertical: cont, start: "0", end:i});
             // console.log(data);
              //arrayEnd.push(i)
              cont++;
           }
         }
        //return  arrayEnd;
        return  data.[0];
      }
    },
    methods: {
      chartsAmcore(){

          console.log(this.horizontal);
          console.log(this.verticaltotal);
          console.log(this.mediaM);
          console.log('test');
          console.log(this.mediadiv);
         

         

        // Themes begin
          am4core.useTheme(am4themes_animated);
          // Themes end
          var chart = am4core.create("chartdiv", am4charts.XYChart);
          chart.data = this.mediadiv.datos;
        /*  [{
            vertical: 10,
            start: 0,
            end: 20,
            },{
            vertical: 9,
            start: 0,
            end: 19,
            },{
            "vertical": "8",
            "start": 0,
            "end": 18,
            },{
            "vertical": "7",
            "start": 0,
            "end": 17,
            },{
            "vertical": "6",
            "start": 0,
            "end": 16,
            },{
            "vertical": "5",
            "start": 0,
            "end": 15,
            },{
            "vertical": "4",
            "start": 0,
            "end": 14,
            },{
            "vertical": "3",
            "start": 0,
            "end": 13,
            },{
            vertical: "2",
            start: 0,
            end: 12,
            },{
            vertical: "1",
            start: 0,
            end: 11,
            }];*/
            console.log(chart.data);
            var categoryAxis = chart.yAxes.push(new am4charts.CategoryAxis());
            categoryAxis.dataFields.category = "vertical";
            categoryAxis.renderer.inversed = true;
            categoryAxis.renderer.grid.template.location = 0;

            var valueAxis = chart.xAxes.push(new am4charts.ValueAxis());
            valueAxis.renderer.minGridDistance = 50;

            var columnSeries = chart.series.push(new am4charts.ColumnSeries());
            columnSeries.dataFields.categoryY = "vertical";
            columnSeries.dataFields.valueX = "end";
            columnSeries.dataFields.openValueX = "start";
            columnSeries.columns.template.tooltipText = " Vertical [bold]{categoryY}[/]\n Horizontal \n Inicio de {openValueX}\n Hasta {valueX}";

            var columnTemplate = columnSeries.columns.template;
            columnTemplate.strokeOpacity = 0;
            columnTemplate.propertyFields.fill = "color";
            columnTemplate.height = am4core.percent(50);
      },
      chatsInverse(){
        // Themes begin
          am4core.useTheme(am4themes_animated);
          // Themes end
          var chart = am4core.create("chartdivInverse", am4charts.XYChart);
          chart.data = [{
            "name": "1",
            "startTime": 0,
            "endTime": 5,
            "color": chart.colors.next()
          }, {
            "name": "2",
            "startTime": 0,
            "endTime": 6,
            "color": chart.colors.next()
          }, {
            "name": "3",
            "startTime": 0,
            "endTime": 7,
            "color": chart.colors.next()
          }, {
            "name": "4",
            "startTime": 0,
            "endTime": 8,
            "color": chart.colors.next()
          }, {
            "name": "5",
            "startTime": 0,
            "endTime": 9,
            "color": chart.colors.next()
          }, {
            "name": "6",
            "startTime": 0,
            "endTime": 10,
            "color": chart.colors.next()
          }];

          var categoryAxis = chart.yAxes.push(new am4charts.CategoryAxis());
          categoryAxis.dataFields.category = "name";
          categoryAxis.renderer.inversed = true;
          categoryAxis.renderer.grid.template.location = 0;

          var valueAxis = chart.xAxes.push(new am4charts.ValueAxis());
          valueAxis.renderer.minGridDistance = 50;

          var columnSeries = chart.series.push(new am4charts.ColumnSeries());
          columnSeries.dataFields.categoryY = "name";
          columnSeries.dataFields.valueX = "endTime";
          columnSeries.dataFields.openValueX = "startTime";
          columnSeries.columns.template.tooltipText = "[bold]{categoryY}[/]\nstarts at {openValueX}\nends at {valueX}";

          var columnTemplate = columnSeries.columns.template;
          columnTemplate.strokeOpacity = 0;
          columnTemplate.propertyFields.fill = "color";
          columnTemplate.height = am4core.percent(100);
      }
    },
    mounted() {
      this.chatsInverse();
    }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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

#chartdiv {
  width: 100%;
  height: 500px;
}
#chartdivInverse{
  width: 100%;
  height: 500px; 
}



body {
  background-color: #363E4A;
  font-family: $base-font-family;
  font-size: 16px;
  color: #ffffff;
  line-height: 1.5;
}

h1 {
  font-family: "Texta", $base-font-family;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 4px;
  text-align: center;
}

h2 {
  font-weight: 400;
  font-size: 24px;
  color: rgba(255, 255, 255, .4);
}

.container {
  max-width: 320px;
  margin: 0 auto;
  padding: 20px 16px 40px 16px;
  transform: translateZ(0);
  text-align: center;
}

.form-footer {
  margin-top: 2em;
}

.ui-input {
  position: relative;
  padding: 0;
  border: 0;
}

.ui-input input {
  font-family: $base-font-family;
  border: 0;
  background: none;
  padding: 16px 0 16px 0;
  font-size: 24px;
  outline: 0;
  width: 100%;
  tap-highlight-color: rgba(0,0,0,0);
  touch-callout: none;
}

.ui-input input + label {
  position: relative;
  display: block;
  padding: 8px 0 8px 0;
  text-transform: uppercase;
  font-size: 14px;
  letter-spacing: .0875em;
  font-weight: 500;
  text-align: left;
  
  &::before, &::after {
    position: absolute;
    top: 0;
    left: 0;
    content: "";
    width: 100%;
    height: 1px;
  }
  
  &::before {
    background-color: rgba(255, 255, 255, .2);
  }
  
  &::after {
    transform: scaleX(0);
    transform-origin: left;
    transition: transform $time $bezier;
    background-color: #6EB1FF;
    height: 2px;
  }
  
  span {
    position: relative;
    color: rgba(255, 255, 255, .2);
    transition: color $time $bezier;
   
    &::after {
      content: attr(data-text);
      position: absolute;
      overflow: hidden;
      left: 0;
      transform: scaleX(1);
      white-space: nowrap;
      color: #fff;
      
      background-image: linear-gradient(to right,
          #4A90E2 50%,
          rgba(255,255,255,0) 0%);
      background-position: 100% 50%;
      background-size: 200%;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      
      backface-visibility: hidden;
      perspective: 1000;
      transform: translateZ(0);
      
      transition: background-position $time $bezier;
    }
  }
}

.ui-input input:focus,
.ui-input input.error,
.ui-input input:invalid,
.ui-input input.filled {
  
  & + label {
    
    &::after {
      transform: scaleX(1);
      transform-origin: left;
    }
    
    span::after {
      //color: #4A90E2;
      background-image: linear-gradient(to right,
      rgba(255,255,255,1) 50%,
      rgba(255,255,255,0) 0%);
      background-position: 0% 50%;
    }
  }
}

.ui-input input.filled {
  color: #ffffff;
  
  & + label {
    
    &::after {
      background-color: #ffffff;
    }
    
    span::after {
      background-image: linear-gradient(to right,
        #ffffff 50%,
        rgba(255,255,255,0) 0%);
      background-position: 0% 50%;
    }
  }
}

.ui-input input:focus {
  color: #6EB1FF;
  
  & + label {
    
    &::after {
      background-color: #6EB1FF;
    }
    
    span::after {
      background-image: linear-gradient(to right,
        #6EB1FF 50%,
        rgba(255,255,255,0) 0%);
      background-position: 0% 50%;
    }
  }
}

.ui-input input.error,
.ui-input input:invalid {
  color: #E66161;
  
  & + label {
    
    &::after {
      background-color: #E66161;
    }
    
    span::after {
      background-image: linear-gradient(to right,
        #E66161 50%,
        rgba(255,255,255,0) 0%);
      background-position: 0% 50%;
    }
  }
}

.btn {
  border: 0;
  background-color: #50617A;
  padding: 18px 30px;
  font-size: 14px;
  line-height: 1.5;
  text-transform: uppercase;
  letter-spacing: .0875em;
  font-weight: 500;
  color: #ffffff;
  font-family: $base-font-family;
  border-radius: 100px;
  outline: 0;
  transition: background-color $time $bezier,
              color $time $bezier;
}
</style>
