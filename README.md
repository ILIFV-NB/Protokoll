<!--
author:   Nancy Brinkmann, Ronny Stolze

email:    nancy.brinkmann@hs-magdeburg.de, ronny.stolze@hs-magdeburg.de

version:  1.0.0

language: de_DE

narrator: DE FEMALE

comment:  Try to write a short comment about
          your course, multiline is also okay.

script:  https://cdnjs.cloudflare.com/ajax/libs/echarts/4.1.0/echarts-en.min.js
-->

# **Versuchsprotokoll** ...

... für das Praktikum *Drehen mit Fernzugriff*


Hier sind alle Beobachtungen und Ergebnisse aller Versuche des Praktikums zu protokollieren. Mit der Abgabe eines vollständigen Protokolles ist das Praktikum abgeschlossen.


# Test Course Chart

``` cvs (Zeit in s - Kraft Fx in N - Kraft Fy in N - Kraft Fz in N)
0;1,5;2,4;0,2
1;1,8;2,1;0,5
2;1,1;2,9;0,8
3;1,1;2,9;-0,8
4;1,1;2,9;-0,4
5;1,1;2,9;0,8
6;1,1;2,9;-0,6
```
<script>
let data = `@input`.replace(/,/g, ".");

let split = data.match(/\d+(?:\.\d+)?|\-\d+(?:\.\d+)?/g);
//document.write(split);
let T = []
let Fx = []
let Fy = []
let Fz = []

for(let i=0; i<split.length; i=i+4) {
  T.push(parseFloat(split[i]));
  Fx.push(parseFloat(split[i+1]));
  Fy.push(parseFloat(split[i+2]));
  Fz.push(parseFloat(split[i+3]));
}

plotData(T, Fx, Fy, Fz);

function plotData(t, x, y, z) {

  let main = document.getElementById('main');
  main.hidden = false;

  let fx = []
  let fy = []
  let fz = []

  for(let i=0; i<t.length; i++) {
    fx.push([t[i], x[i]])
    fy.push([t[i], y[i]])
    fz.push([t[i], z[i]])
  }

  let chart = echarts.init(main);

  let option = {

    title : {
      display: false,
      text: "Zerspankraft",
      subtext: 'Drehen',
      itemGap: 10,
      textAlign: 'auto',
      textVerticalAlign: 'middle',
      textStyle: {
        fontSize: 30,
      },
      subtextStyle: {
        fontSize: 20,
      },
    },

    grid: {
      top: 120,
    },

    legend: {
        data:['Fx', 'Fy', 'Fz'],
        top: 80,
        itemGap: 30,
        itemWidth: 50,
        itemHeight: 20,
        textStyle: {
          fontSize: 24,
        },
    },

    toolbox: {
      show : true,
      feature : {
        mark : {show: true},
        dataZoom : {show: true},
        dataView : {show: true, readOnly: false},
        restore : {show: true},
        saveAsImage : {
          show: true,
          pixelRatio: 4,
        },
      },
    },

    xAxis: [{
      type: 'value',
      name: 'Zeit in s',
      nameLocation: 'middle',
      nameGap: 40,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    yAxis: [{
      type : 'value',
      name: 'Kraft in N',
      nameLocation: 'middle',
      nameGap: 60,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],


    series : [{
      name:'Fx',
      type:'line',
      data: fx,
      symbol: 'none',
      lineStyle: {
        width: 3,
      },
    },
    {
      name:'Fy',
      type:'line',
      data: fy,
      symbol: 'none',
      lineStyle: {
        width: 3,
      },
    },
    {
      name:'Fz',
      type:'line',
      data: fz,
      symbol: 'none',
      lineStyle: {
        width: 3,
      },
    }]
  };

  // use configuration item and data specified to show chart
  chart.setOption(option);

  window.addEventListener('resize', chart.resize);
}
</script>


<div id="main" style="position:relative; width:100%; height:600%;" hidden="true"></div>


# Testvideos

!?[movie](movies/math.mp4)<!--
style = "height: 446px; width: 793px; "
-->

!?[movie](movies/test1_NX8.mp4)<!--
style = "height: 446px; width: 793px; margin: 0em 0em; "
-->

<iframe width="793" height="446" src="https://www.youtube.com/embed/bICfKRyKTwE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Test Dateien laden

<input type="file" onchange="getFileContent(this.files)">
<pre id="content">
</pre>


<script>
window.getFileContent = (files) => {
	if (files.length !== 1) {
  	// Sicherstellen, dass nur eine Datei hochgeladen wurde.
    return;
  }
  const reader = new FileReader();
  reader.addEventListener("loadend", () => {
  	// in reader.result stehen die bytes
    // also müssen wir es noch in text umwandeln.

    const decoder = new TextDecoder();
    const textValue = decoder.decode(reader.result);

    // Jetzt kannst du dinge mit dem text machen
  	document.getElementById("content").innerText = textValue;
  });
  reader.readAsArrayBuffer(files[0]);
}
</script>
