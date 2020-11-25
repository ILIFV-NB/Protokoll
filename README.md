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


# Testvideos ...

Videos aus dem **Projektordner**

~~1. Möglichkeit **lokaler Pfad** - derzeit **nicht** möglich~~

Ein Video, das sich in Ihrem Projektordner (also lokal auf ihrem Rechner bzw. online bei GitHub) befindet, binden Sie wie ein Bild ein. Damit sich die Größe des Videos an die Größe ihres Browsers anpasst, ergänzen Sie den Pfad wie folgt:

`!?[movie](movies/math.mp4)<!--`
<br/>
`style = "width: 100%; "`
<br/>
`-->`

!?[movie](movies/math.mp4)<!--
style = "width: 100%; "
-->

<br/>
~~2. Möglichkeit **absolute URLs (Pfad auf GitHub)**~~

`?[movie](https://raw.githubusercontent.com/ILIFV-NB/TestKurs/master/movies/math.mp4)<!--`
<br/>
`style = "height: 446px; width: 793px; "`
<br/>
`-->`

!?[movie](https://raw.githubusercontent.com/ILIFV-NB/TestKurs/master/movies/math.mp4)<!--
style = "height: 446px; width: 793px; "
-->

!?[movie](https://raw.githubusercontent.com/ILIFV-NB/TestKurs/master/movies/test1_NX8.mp4)<!--
style = "height: 446px; width: 793px; "
-->

<br/>
Videos aus dem **Internet** (YouTube):

~~1. Möglichkeit **Video-URL** - im Moment im eLab **nicht möglich**~~

Öffnen des gewünschten Videos auf YouTube, Klick mit der rechten Maustaste darauf und  ~~Video-URL kopieren~~ wählen. Diese an entsprechender Stelle im Dokument platzieren  - dort, wo normalerweise der lokale Pfad aufgeführt sit:

`!?[embedded media](https://youtu.be/bICfKRyKTwE)`

!?[embedded media](https://youtu.be/bICfKRyKTwE)

~~2. Möglichkeit **Einbettungscode**~~

<iframe width="793" height="446" src="https://www.youtube.com/embed/bICfKRyKTwE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Testbilder ...

Bilder aus dem **Projektordner**

~~1. Möglichkeit **lokaler Pfad** - derzeit **nicht** möglich~~$^* $
Ein Foto, das sich im Projektordner (also lokal auf dem Rechner bzw. online bei GitHub) befindet, wie ein Video einbinden. Damit sich die Größe des Bildes an die Größe des Browsers anpasst, den Pfad wie folgt ergänzen:

`![image](images/Eingabefeld-Kraftverlauf.png)<!--`
<br/>
`style = "width: 100%; "`
<br/>
`-->`

![image](images/Eingabefeld-Kraftverlauf.png)<!--
style = "width: 100%; "
-->

~~Erläuterung:~~

Um ein Bild einzubinden, muss dieses in dem zugehörigen Projektordner abgelegt sein. Dieser befindet sich in der Regel lokal auf dem Rechner im Ordner ~~GitHub~~ (synchronisiert mit dem Projektordner auf GitHub) und trägt idealerweise den Namen des Kurses. In jedem Projektordner sollten Ordner angelegt sein, die eigens für Fotos bzw. Videos zur Verfügung stehen. Diese können bspw. *images* bzw. *movies* heißen. (Weitere Erläuterungen unter [GitHub -> 4.](#4) bzw. [Atom -> 3.](#5))

In Atom innerhalb der Textdatei ~~img~~ eingeben und die ~~Tabtaste~~ drücken. Es erscheint `![]()`. (Voraussetzung dafür ist die Installation des [Plugins](https://atom.io/packages/search?utf8=%E2%9C%93&q=liascript&commit=Search) *liascript-snippets*.) In die eckigen Klammern einen Hinweis eingeben, der verrät, um welche Art Datei es sich handelt. Für ein Foto kann dieser z.B. das Wort *image* sein. In die runden Klammern wird der Pfad innerhalb des Projektordners eingetragen. Heißt der Fotoordner *images* und das Foto *ErstesKursFoto.png*, sieht der Pfad wie folgt aus: `![image](images/ErstesKursFoto.png)`. In der Preview (**Alt$+$L**) bzw. im Browser (**Strg$+$N**) erscheint an dieser Stelle das Foto.

$^* $Hier gilt also das Gleiche wie für Videos: **absolute** URLs nutzen, um die Bilder direkt im Netz laden zu können.

<br/>
~~2. Möglichkeit **absolute URLs (Pfad auf GitHub)**~~

`![image](https://raw.githubusercontent.com/ILIFV-NB/TestKurs/master/images/Eingabefeld-Kraftverlauf.jpg)<!--`
<br/>
`style = "width: 100%; "`
<br/>
`-->`

![image](https://raw.githubusercontent.com/ILIFV-NB/TestKurs/master/images/Eingabefeld-Kraftverlauf.jpg)<!--
style = "width: 100%; "
-->


# Beispiele: Diagramme erzeugen

* [Zerspankräfte Drehen](#4)<br/>
* [Rauheitskenngrößen](#5)<br/>
* [Rauheitsprofile](#6)<br/>
* [Durchmesser](#10)

### Zerspankräfte Drehen

Lade eine Datei mit deiner Zerspankraftmessung hoch: <input type="file" onchange="getFileContent(this.files)">

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

    const data = textValue.replace(/,/g, ".");

    const split = data.match(/\d+(?:\.\d+)?|\-\d+(?:\.\d+)?/g);
    const T = []
    const Fx = []
    const Fy = []
    const Fz = []

    for(let i=0; i<split.length; i=i+4) {
      T.push(parseFloat(split[i]));
      Fx.push(parseFloat(split[i+1]));
      Fy.push(parseFloat(split[i+2]));
      Fz.push(parseFloat(split[i+3]));
    }

    plotData(T, Fx, Fy, Fz);

    // Jetzt kannst du dinge mit dem text machen
  	document.getElementById("content").innerText = textValue;
  });
  reader.readAsArrayBuffer(files[0]);
}

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


### Rauheitskenngrößen

Lade eine Datei mit deinen Rauheitskenngrößen hoch: <input type="file" onchange="getFileContent(this.files)">

<script>
window.getFileContent = (files) => {
	if (files.length !== 1) {
  	// Sicherstellen, dass nur eine Datei hochgeladen wurde.
    return;
  }
  const reader1 = new FileReader();
  reader1.addEventListener("loadend", () => {
  	// in reader.result stehen die bytes
    // also müssen wir es noch in text umwandeln.

    const decoder1 = new TextDecoder();
    const textValue1 = decoder1.decode(reader1.result);

    const data1 = textValue1.replace(/,/g, ".");

    const split1 = data1.match(/\d+(?:\.\d+)?|\-\d+(?:\.\d+)?/g);
    const M = []
    const Ra = []
    const Rz = []
    const Rmax = []

    for(let i=0; i<split1.length; i=i+4) {
      M.push(parseFloat(split1[i]));
      Ra.push(parseFloat(split1[i+1]));
      Rz.push(parseFloat(split1[i+2]));
      Rmax.push(parseFloat(split1[i+3]));
    }

    plotData(M, Ra, Rz, Rmax);

    // Jetzt kannst du dinge mit dem text machen
  	document.getElementById("content").innerText = textValue1;
  });
  reader1.readAsArrayBuffer(files[0]);
}

function plotData(t1, x1, y1, z1) {

  let main1 = document.getElementById('main1');
  main1.hidden = false;

  let ra = []
  let rz = []
  let rmax = []

  for(let i=0; i<t1.length; i++) {
    ra.push([t1[i], x1[i]])
    rz.push([t1[i], y1[i]])
    rmax.push([t1[i], z1[i]])
  }

  let chart1 = echarts.init(main1);

  let option1 = {

    title : {
      display: false,
      text: "Rauheit",
      subtext: 'Kenngrößen',
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
        data:['Ra', 'Rz', 'Rmax'],
        top: 80,
        itemGap: 40,
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
      type: 'category',
      nameLocation: 'middle',
      nameGap: 30,
      axisLabel: {
        fontSize: 20,
        formatter: 'Messung {value}'
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    yAxis: [{
      type : 'value',
      name: 'Rauheit in µm',
      nameLocation: 'middle',
      nameGap: 60,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    series : [
    {
      name:'Ra',
      type:'bar',
      data: ra,
      label: {
        show: true,
        rotate: 90,
        formatter: ra,
        fontSize: 18,
      },
    },
    {
      name:'Rz',
      type:'bar',
      data: rz,
      label: {
        show: true,
        rotate: 90,
        formatter: rz,
        fontSize: 18,
      },
    },
    {
      name:'Rmax',
      type:'bar',
      data: rmax,
      label: {
        show: true,
        rotate: 90,
        formatter: rmax,
        fontSize: 18,
      },
    }
    ]
  };

  // use configuration item and data specified to show chart
  chart1.setOption(option1);

  window.addEventListener('resize', chart1.resize);
}
</script>


<div id="main1" style="position: relative; width:100%; height:600%;" hidden="true"></div>


### Rauheitsprofile

* [P-Profil](#6)

* [R-Profil](#7)

* [W-Profil](#8)

#### **P-Profil**

Lade eine Datei mit deinem P-Profil hoch: <input type="file" onchange="getFileContent(this.files)">

<script>
window.getFileContent = (files) => {
	if (files.length !== 1) {
  	// Sicherstellen, dass nur eine Datei hochgeladen wurde.
    return;
  }
  const reader2 = new FileReader();
  reader2.addEventListener("loadend", () => {
  	// in reader.result stehen die bytes
    // also müssen wir es noch in text umwandeln.

    const decoder2 = new TextDecoder();
    const textValue2 = decoder2.decode(reader2.result);

    const data2 = textValue2.replace(/,/g, ".");

    const split2 = data2.match(/\d+(?:\.\d+)?|\-\d+(?:\.\d+)?/g);
    const Lt = []
    const P = []

    for(let i=0; i<split2.length; i=i+2) {
      Lt.push(parseFloat(split2[i]));
      P.push(parseFloat(split2[i+1]));
    }

    plotData(Lt, P);

    // Jetzt kannst du dinge mit dem text machen
  	document.getElementById("content").innerText = textValue2;
  });
  reader2.readAsArrayBuffer(files[0]);
}

function plotData(t2, x2) {

  let main2 = document.getElementById('main2');
  main2.hidden = false;

  let p = []

  for(let i=0; i<t2.length; i++) {
    p.push([t2[i], x2[i]])
  }

  let chart2 = echarts.init(main2);

  let option2 = {

    title : {
      display: false,
      text: "Primärprofil",
      subtext: 'P-Profil',
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
        data:['P-Profil'],
        top: 80,
        itemGap: 40,
        itemWidth: 50,
        itemHeight: 20,
        textStyle: {
          fontSize: 24,
          color: 'black',
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
      name: 'Taststrecke in µm',
      nameLocation: 'middle',
      nameGap: 40,
      min: 0,
      max: 4800,
      interval: 800,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    yAxis: [{
      type : 'value',
      name: 'Profil in µm',
      nameLocation: 'middle',
      nameGap: 60,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    series : [
    {
      name:'P-Profil',
      type:'line',
      data: p,
      symbol: 'none',
      color: 'black',
      lineStyle: {
        width: 1,
        color: 'black',
      },
    },
    ]
  };

  // use configuration item and data specified to show chart
  chart2.setOption(option2);

  window.addEventListener('resize', chart2.resize);
}
</script>


<div id="main2" style="position: relative; width:100%; height:600%;" hidden="true"></div>


<br/>

#### **R-Profil**

Lade eine Datei mit deinem R-Profil hoch:
<input type="file" onchange="getFileContent(this.files)">


<script>
window.getFileContent = (files) => {
	if (files.length !== 1) {
  	// Sicherstellen, dass nur eine Datei hochgeladen wurde.
    return;
  }
  const reader3 = new FileReader();
  reader3.addEventListener("loadend", () => {
  	// in reader.result stehen die bytes
    // also müssen wir es noch in text umwandeln.

    const decoder3 = new TextDecoder();
    const textValue3 = decoder3.decode(reader3.result);

    const data3 = textValue3.replace(/,/g, ".");

    const split3 = data3.match(/\d+(?:\.\d+)?|\-\d+(?:\.\d+)?/g);
    const Ln = []
    const R = []

    for(let i=0; i<split3.length; i=i+2) {
      Ln.push(parseFloat(split3[i]));
      R.push(parseFloat(split3[i+1]));
    }

    plotData(Ln, R);

    // Jetzt kannst du dinge mit dem text machen
  	document.getElementById("content").innerText = textValue3;
  });
  reader3.readAsArrayBuffer(files[0]);
}

function plotData(t3, x3) {

  let main3 = document.getElementById('main3');
  main3.hidden = false;

  let r = []

  for(let i=0; i<t3.length; i++) {
    r.push([t3[i], x3[i]])
  }

  let chart3 = echarts.init(main3);

  let option3 = {

    title : {
      display: false,
      text: "Rauheitsprofil",
      subtext: 'R-Profil',
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
        data:['R-Profil'],
        top: 80,
        itemGap: 40,
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
      name: 'Messstrecke in mm',
      nameLocation: 'middle',
      nameGap: 40,
      min: 0,
      max: 4.8,
      interval: 0.8,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    yAxis: [{
      type : 'value',
      name: 'Profil in µm',
      nameLocation: 'middle',
      nameGap: 60,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    series : [
    {
      name:'R-Profil',
      type:'line',
      data: r,
      symbol: 'none',
      lineStyle: {
        width: 1,
      },
    },
    ]
  };

  // use configuration item and data specified to show chart
  chart3.setOption(option3);

  window.addEventListener('resize', chart3.resize);
}
</script>


<div id="main3" style="position: relative; width:100%; height:600%;" hidden="true"></div>


<br/>


#### **W-Profil**

Lade eine Datei mit deinem W-Profil hoch: <input type="file" onchange="getFileContent(this.files)">

<script>
window.getFileContent = (files) => {
	if (files.length !== 1) {
  	// Sicherstellen, dass nur eine Datei hochgeladen wurde.
    return;
  }
  const reader4 = new FileReader();
  reader4.addEventListener("loadend", () => {
  	// in reader.result stehen die bytes
    // also müssen wir es noch in text umwandeln.

    const decoder4 = new TextDecoder();
    const textValue4 = decoder4.decode(reader4.result);

    const data4 = textValue4.replace(/,/g, ".");

    const split4 = data4.match(/\d+(?:\.\d+)?|\-\d+(?:\.\d+)?/g);
    const Ln1 = []
    const W = []

    for(let i=0; i<split4.length; i=i+2) {
      Ln1.push(parseFloat(split4[i]));
      W.push(parseFloat(split4[i+1]));
    }

    plotData(Ln1, W);

    // Jetzt kannst du dinge mit dem text machen
  	document.getElementById("content").innerText = textValue2;
  });
  reader4.readAsArrayBuffer(files[0]);
}

function plotData(t4, x4) {

  let main4 = document.getElementById('main4');
  main4.hidden = false;

  let w = []

  for(let i=0; i<t4.length; i++) {
    w.push([t4[i], x4[i]])
  }

  let chart4 = echarts.init(main4);

  let option4 = {

    title : {
      display: false,
      text: "Welligkeitsprofil",
      subtext: 'W-Profil',
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
        data:['W-Profil'],
        top: 80,
        itemGap: 40,
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
      name: 'Messstrecke in µm',
      nameLocation: 'middle',
      nameGap: 40,
      min: 0,
      max: 4800,
      interval: 800,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    yAxis: [{
      type : 'value',
      name: 'Profil in µm',
      nameLocation: 'middle',
      nameGap: 60,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    series : [
    {
      name:'W-Profil',
      type:'line',
      data: w,
      symbol: 'none',
      lineStyle: {
        width: 1,
      },
    },
    ]
  };

  // use configuration item and data specified to show chart
  chart4.setOption(option4);

  window.addEventListener('resize', chart4.resize);
}
</script>

<div id="main4" style="position: relative; width:100%; height:600%;" hidden="true"></div>


### Durchmesser

Lade eine Datei mit deinem Durchmesser hoch: <input type="file" onchange="getFileContent(this.files)">

<script>
window.getFileContent = (files) => {
	if (files.length !== 1) {
  	// Sicherstellen, dass nur eine Datei hochgeladen wurde.
    return;
  }
  const reader3 = new FileReader();
  reader3.addEventListener("loadend", () => {
  	// in reader.result stehen die bytes
    // also müssen wir es noch in text umwandeln.

    const decoder3 = new TextDecoder();
    const textValue3 = decoder3.decode(reader3.result);

    const data3 = textValue3.replace(/,/g, ".");

    const split3 = data3.match(/\d+(?:\.\d+)?|\-\d+(?:\.\d+)?/g);
    const M1 = []
    const D = []

    for(let i=0; i<split3.length; i=i+2) {
      M1.push(parseFloat(split3[i]));
      D.push(parseFloat(split3[i+1]));
    }

    plotData(M1, D);

    // Jetzt kannst du dinge mit dem text machen
  	document.getElementById("content").innerText = textValue1;
  });
  reader3.readAsArrayBuffer(files[0]);
}

function plotData(t3, x3) {

  let main3 = document.getElementById('main3');
  main3.hidden = false;

  let d = []

  for(let i=0; i<t3.length; i++) {
    d.push([t3[i], x3[i]])
  }

  let chart3 = echarts.init(main3);

  let option3 = {

    title : {
      display: false,
      text: "Durchmesser",
      subtext: 'Welle/Passsitz',
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
        data:['Durchmesser'],
        top: 80,
        itemGap: 40,
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
      type: 'category',
      nameLocation: 'middle',
      nameGap: 30,
      axisLabel: {
        fontSize: 20,
        formatter: 'Messung {value}'
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    yAxis: [{
      type : 'value',
      name: 'Messwert in mm',
      nameLocation: 'middle',
      nameGap: 60,
      axisLabel: {
        fontSize: 20,
      },
      nameTextStyle: {
        fontSize: 20,
      },
    }],

    series : [
    {
      name:'Durchmesser',
      type:'bar',
      data: d,
      label: {
        show: true,
        rotate: 90,
        formatter: d,
        fontSize: 18,
      },
    },
    ]
  };

  // use configuration item and data specified to show chart
  chart3.setOption(option3);

  window.addEventListener('resize', chart3.resize);
}
</script>


<div id="main3" style="position: relative; width:100%; height:600%;" hidden="true"></div>

# Input
<form>
      <input id="groesse" type="number" min="100" max="220" step="1">
      <input id="masse" type="number" min="30" max="225" step="0.5">
</form>

<br/>
<label for="fname">First name:</label>
<input type="text" id="fname" name="fname">


<br/>
Versuch 1: Tragen Sie hier bitte hintereinander alle Werte zu Versuch 1 ein! (ap; vc; f; n; Versuch)

[[___ ___]]


# Test - Bild in Tabellen integrieren (Werkzeugmaschine)

~~**Drehmaschine**~~

![image](https://raw.githubusercontent.com/ILIFV-NB/Maschinen-Geraetetechnik/master/images/EMCO.jpg)<!--
style = "width: 75%; "
-->

*EMCOMAT E-300, zyklengesteuerte Drehmaschine*

<br>

|**Technische Daten**|**Antriebsleistung und Drehmomentenverlauf**|
|![image](https://raw.githubusercontent.com/ILIFV-NB/Maschinen-Geraetetechnik/master/images/Technische_Daten_EMCO.png)<!--
style = "width: 75%; "
-->|![image](https://raw.githubusercontent.com/ILIFV-NB/Maschinen-Geraetetechnik/master/images/Antriebsleistung_und_Drehmomentenverlauf_E300.png)<!--
style = "width: 75%; "
-->|


**Technische Daten**

|Spitzenweite|$mm$ |$1500$ |
| Umlauf- $\text{\O}$ über Bett | $mm$|$570$ |
|Umlauf- $\text{\O}$ über Schlitten | $mm$ | $340$ |
| Vorschubkraft X max. | $kN$ | $10$ |
| Vorschubkraft Z max. | $kN$|$15$|
| Spindeldrehzahl| $U/min$| $0\space-\space2500$|
|Drehzahlregelung| | stufenlos|
| Antriebsleistung bei $40/100\space$ % ED|$kW $| $25/17$|
| max. Nennmoment an der Hauptspindel| $Nm$| $764/519$|
|Prozesskühlung| | Emulsion |

<br>

**Antriebsleistung und Drehmomentenverlauf**

![image](https://raw.githubusercontent.com/ILIFV-NB/Maschinen-Geraetetechnik/master/images/Antriebsleistung_und_Drehmomentenverlauf_E300.png)<!--
style = "width: 75%; "
-->
