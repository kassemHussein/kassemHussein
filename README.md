<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">





    <!----- bootstrap-->


    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
<script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.12.9/dist/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>


<!-------------sitecolor-->


<meta name="icon"   href="https://taktlat.xyz/logo4_18_21528.png">
<!---color---->
    <meta name="color-scheme"  content="light dark">
    <title>Document</title>
</head>
<body>



    <!-- Styles -->
<style>


#body{
 background: #2F4F4F
;

}
#chartdiv {
  width: 100%;
  height: 500px;
  overflow: hidden;



</style>

<!-- Resources -->


<!-- Resources -->
<script src="https://cdn.amcharts.com/lib/5/index.js"></script>
<script src="https://cdn.amcharts.com/lib/5/map.js"></script>
<script src="https://cdn.amcharts.com/lib/5/geodata/worldLow.js"></script>
<script src="https://cdn.amcharts.com/lib/5/themes/Animated.js"></script>

<!-- Chart code -->
<script>
am5.ready(function() {

// Create root element
// https://www.amcharts.com/docs/v5/getting-started/#Root_element
var root = am5.Root.new("chartdiv");

// Set themes
// https://www.amcharts.com/docs/v5/concepts/themes/
root.setThemes([
  am5themes_Animated.new(root)
]);

// Create the map chart
// https://www.amcharts.com/docs/v5/charts/map-chart/
var chart = root.container.children.push(
  am5map.MapChart.new(root, {
    panX: "rotateX",
    panY: "translateY",
    projection: am5map.geoMercator()
  })
);

var cont = chart.children.push(
  am5.Container.new(root, {
    layout: root.horizontalLayout,
    x: 20,
    y: 40
  })
);

// Add labels and controls
cont.children.push(
  am5.Label.new(root, {
    centerY: am5.p50,
    text: "Map"
  })
);

var switchButton = cont.children.push(
  am5.Button.new(root, {
    themeTags: ["switch"],
    centerY: am5.p50,
    icon: am5.Circle.new(root, {
      themeTags: ["icon"]
    })
  })
);

switchButton.on("active", function() {
  if (!switchButton.get("active")) {
    chart.set("projection", am5map.geoMercator());
    chart.set("panY", "translateY");
    chart.set("rotationY", 0);
    backgroundSeries.mapPolygons.template.set("fillOpacity", 0);
  } else {
    chart.set("projection", am5map.geoOrthographic());
    chart.set("panY", "rotateY")

    backgroundSeries.mapPolygons.template.set("fillOpacity", 0.1);
  }
});

cont.children.push(
  am5.Label.new(root, {
    centerY: am5.p50,
    text: "Globe"
  })
);

// Create series for background fill
// https://www.amcharts.com/docs/v5/charts/map-chart/map-polygon-series/#Background_polygon
var backgroundSeries = chart.series.push(am5map.MapPolygonSeries.new(root, {}));
backgroundSeries.mapPolygons.template.setAll({
  fill: root.interfaceColors.get("alternativeBackground"),
  fillOpacity: 0,
  strokeOpacity: 0
});

// Add background polygon
// https://www.amcharts.com/docs/v5/charts/map-chart/map-polygon-series/#Background_polygon
backgroundSeries.data.push({
  geometry: am5map.getGeoRectangle(90, 180, -90, -180)
});

// Create main polygon series for countries
// https://www.amcharts.com/docs/v5/charts/map-chart/map-polygon-series/
var polygonSeries = chart.series.push(
  am5map.MapPolygonSeries.new(root, {
    geoJSON: am5geodata_worldLow
  })
);

// Create line series for trajectory lines
// https://www.amcharts.com/docs/v5/charts/map-chart/map-line-series/
var lineSeries = chart.series.push(am5map.MapLineSeries.new(root, {}));
lineSeries.mapLines.template.setAll({
  stroke: root.interfaceColors.get("alternativeBackground"),
  strokeOpacity: 0.3
});

// Create point series for markers
// https://www.amcharts.com/docs/v5/charts/map-chart/map-point-series/
var pointSeries = chart.series.push(am5map.MapPointSeries.new(root, {}));
var colorset = am5.ColorSet.new(root, {});

pointSeries.bullets.push(function () {
  var container = am5.Container.new(root, {});

  var circle = container.children.push(
    am5.Circle.new(root, {
      radius: 4,
      tooltipY: 0,
      fill: colorset.next(),
      strokeOpacity: 0,
      tooltipText: "{title}"
    })
  );

  var circle2 = container.children.push(
    am5.Circle.new(root, {
      radius: 4,
      tooltipY: 0,
      fill: colorset.next(),
      strokeOpacity: 0,
      tooltipText: "{title}"
    })
  );

  circle.animate({
    key: "scale",
    from: 1,
    to: 5,
    duration: 600,
    easing: am5.ease.out(am5.ease.cubic),
    loops: Infinity
  });
  circle.animate({
    key: "opacity",
    from: 1,
    to: 0,
    duration: 600,
    easing: am5.ease.out(am5.ease.cubic),
    loops: Infinity
  });

  return am5.Bullet.new(root, {
    sprite: container
  });
});


var cities = [
  {
    title: "Brussels",
    latitude: 50.8371,
    longitude: 4.3676
  },
  {
    title: "Copenhagen",
    latitude: 55.6763,
    longitude: 12.5681
  },
  {
    title: "Paris",
    latitude: 48.8567,
    longitude: 2.351
  },
  {
    title: "Reykjavik",
    latitude: 64.1353,
    longitude: -21.8952
  },
  {
    title: "Moscow",
    latitude: 55.7558,
    longitude: 37.6176
  },
  {
    title: "Madrid",
    latitude: 40.4167,
    longitude: -3.7033
  },
  {
    title: "London",
    latitude: 51.5002,
    longitude: -0.1262,
    url: "http://www.google.co.uk"
  },
  {
    title: "Peking",
    latitude: 39.9056,
    longitude: 116.3958
  },
  {
    title: "New Delhi",
    latitude: 28.6353,
    longitude: 77.225
  },
  {
    title: "Tokyo",
    latitude: 35.6785,
    longitude: 139.6823,
    url: "http://www.google.co.jp"
  },
  {
    title: "Ankara",
    latitude: 39.9439,
    longitude: 32.856
  },
  {
    title: "Buenos Aires",
    latitude: -34.6118,
    longitude: -58.4173
  },
  {
    title: "Brasilia",
    latitude: -15.7801,
    longitude: -47.9292
  },
  {
    title: "Ottawa",
    latitude: 45.4235,
    longitude: -75.6979
  },
  {
    title: "Washington",
    latitude: 38.8921,
    longitude: -77.0241
  },
  {
    title: "Kinshasa",
    latitude: -4.3369,
    longitude: 15.3271
  },
  {
    title: "EM35 ",
    latitude: 30.0571,
    longitude: 31.2272
  },
  {
    title: "Pretoria",
    latitude: -25.7463,
    longitude: 28.1876
  }
];

for (var i = 0; i < cities.length; i++) {
  var city = cities[i];
  addCity(city.longitude, city.latitude, city.title);
}

function addCity(longitude, latitude, title) {
  pointSeries.data.push({
    geometry: { type: "Point", coordinates: [longitude, latitude] },
    title: title
  });
}

// Make stuff animate on load
chart.appear(1000, 100);


}); // end am5.ready()
</script>

<!-- HTML -->
<div id="chartdiv">

  <header>
        <lebel>
        <table>
    <legend style="color:black;">

    <button onclick="window.print()" style="width:49px; height:25px;">Printer</button>
    
    <!---my location-->
    <?php = "window.Location"?>
       &nbsp;
        <select>
            <option value="#" style="color:rgb(166, 220, 195);">E-m35</option>
            <option value="#"style="color: black;">J1</option>
            <option value="#" style="color:  rgb(218, 202, 202);">J2</option>
            <option value="#"  style="color:red;">G</option>
             <option value="#" style="color: rgb(189, 95, 95);">T</option>
            <option value="#"   style="color: red;">L</option>
            <option value="#"   style="color:black;">A</option>
            <option value="#"    style="color: brown;">B</option
            <option value="#"   style="color:yellow;">E</option>
            <option value="#"    style="color:pink;">D</option>
            <option value="#"   style="color:gray;">M</option>
            <option value="#"   style="color:green;">C</option>
        
        
            
        </select>
                   </legend>

           </table>
           </lebel>
           </header>
</div>


<!----copy-->

<strong style="text-align:center; color:gray; font-weight:bold;">&copy; all right reserved BY Taktlat++ ..</strong>


<script  src="typescript">
/* Imports */


import am4geodata_https://cdn.amcharts.com/lib/5/worldLow from "@amcharts/amcharts4-geodata/https://cdn.amcharts.com/lib/5/worldLow";
import am4themes_https://cdn.amcharts.com/lib/5/Animated from "@amcharts/amcharts4/themes/https://cdn.amcharts.com/lib/5/Animated";

/* Chart code */
// Create root element
// https://www.amcharts.com/docs/v5/getting-started/#Root_element
let root = am5.Root.new("chartdiv");

// Set themes
// https://www.amcharts.com/docs/v5/concepts/themes/
root.setThemes([
  am5themes_Animated.new(root)
]);

// Create the map chart
// https://www.amcharts.com/docs/v5/charts/map-chart/
let chart = root.container.children.push(
  am5map.MapChart.new(root, {
    panX: "rotateX",
    panY: "translateY",
    projection: am5map.geoMercator()
  })
);

let cont = chart.children.push(
  am5.Container.new(root, {
    layout: root.horizontalLayout,
    x: 20,
    y: 40
  })
);

// Add labels and controls
cont.children.push(
  am5.Label.new(root, {
    centerY: am5.p50,
    text: "Map"
  })
);

let switchButton = cont.children.push(
  am5.Button.new(root, {
    themeTags: ["switch"],
    centerY: am5.p50,
    icon: am5.Circle.new(root, {
      themeTags: ["icon"]
    })
  })
);

switchButton.on("active", function() {
  if (!switchButton.get("active")) {
    chart.set("projection", am5map.geoMercator());
    chart.set("panY", "translateY");
    chart.set("rotationY", 0);
    backgroundSeries.mapPolygons.template.set("fillOpacity", 0);
  } else {
    chart.set("projection", am5map.geoOrthographic());
    chart.set("panY", "rotateY")

    backgroundSeries.mapPolygons.template.set("fillOpacity", 0.1);
  }
});

cont.children.push(
  am5.Label.new(root, {
    centerY: am5.p50,
    text: "Globe"
  })
);

// Create series for background fill
// https://www.amcharts.com/docs/v5/charts/map-chart/map-polygon-series/#Background_polygon
let backgroundSeries = chart.series.push(am5map.MapPolygonSeries.new(root, {}));
backgroundSeries.mapPolygons.template.setAll({
  fill: root.interfaceColors.get("alternativeBackground"),
  fillOpacity: 0,
  strokeOpacity: 0
});

// Add background polygon
// https://www.amcharts.com/docs/v5/charts/map-chart/map-polygon-series/#Background_polygon
backgroundSeries.data.push({
  geometry: am5map.getGeoRectangle(90, 180, -90, -180)
});

// Create main polygon series for countries
// https://www.amcharts.com/docs/v5/charts/map-chart/map-polygon-series/
let polygonSeries = chart.series.push(
  am5map.MapPolygonSeries.new(root, {
    geoJSON: am5geodata_worldLow
  })
);

// Create line series for trajectory lines
// https://www.amcharts.com/docs/v5/charts/map-chart/map-line-series/
let lineSeries = chart.series.push(am5map.MapLineSeries.new(root, {}));
lineSeries.mapLines.template.setAll({
  stroke: root.interfaceColors.get("alternativeBackground"),
  strokeOpacity: 0.3
});

// Create point series for markers
// https://www.amcharts.com/docs/v5/charts/map-chart/map-point-series/
let pointSeries = chart.series.push(am5map.MapPointSeries.new(root, {}));
let colorset = am5.ColorSet.new(root, {});

pointSeries.bullets.push(function () {
  let container = am5.Container.new(root, {});

  let circle = container.children.push(
    am5.Circle.new(root, {
      radius: 4,
      tooltipY: 0,
      fill: colorset.next(),
      strokeOpacity: 0,
      tooltipText: "{title}"
    })
  );

  let circle2 = container.children.push(
    am5.Circle.new(root, {
      radius: 4,
      tooltipY: 0,
      fill: colorset.next(),
      strokeOpacity: 0,
      tooltipText: "{title}"
    })
  );

  circle.animate({
    key: "scale",
    from: 1,
    to: 5,
    duration: 600,
    easing: am5.ease.out(am5.ease.cubic),
    loops: Infinity
  });
  circle.animate({
    key: "opacity",
    from: 1,
    to: 0,
    duration: 600,
    easing: am5.ease.out(am5.ease.cubic),
    loops: Infinity
  });

  return am5.Bullet.new(root, {
    sprite: container
  });
});

let cities = [
  {
    title: "Brussels",
    latitude: 50.8371,
    longitude: 4.3676
  },
  {
    title: "Copenhagen",
    latitude: 55.6763,
    longitude: 12.5681
  },
  {
    title: "Paris",
    latitude: 48.8567,
    longitude: 2.351
  },
  {
    title: "Reykjavik",
    latitude: 64.1353,
    longitude: -21.8952
  },
  {
    title: "Moscow",
    latitude: 55.7558,
    longitude: 37.6176
  },
  {
    title: "Madrid",
    latitude: 40.4167,
    longitude: -3.7033
  },
  {
    title: "London",
    latitude: 51.5002,
    longitude: -0.1262,
    url: "http://www.google.co.uk"
  },
  {
    title: "Peking",
    latitude: 39.9056,
    longitude: 116.3958
  },
  {
    title: "New Delhi",
    latitude: 28.6353,
    longitude: 77.225
  },
  {
    title: "Tokyo",
    latitude: 35.6785,
    longitude: 139.6823,
    url: "http://www.google.co.jp"
  },
  {
    title: "Ankara",
    latitude: 39.9439,
    longitude: 32.856
  },
  {
    title: "Buenos Aires",
    latitude: -34.6118,
    longitude: -58.4173
  },
  {
    title: "Brasilia",
    latitude: -15.7801,
    longitude: -47.9292
  },
  {
    title: "Ottawa",
    latitude: 45.4235,
    longitude: -75.6979
  },
  {
    title: "Washington",
    latitude: 38.8921,
    longitude: -77.0241
  },
  {
    title: "Kinshasa",
    latitude: -4.3369,
    longitude: 15.3271
  },
  {
    title: "Cairo",
    latitude: 30.0571,
    longitude: 31.2272
  },
  {
    title: "Pretoria",
    latitude: -25.7463,
    longitude: 28.1876
  }
];

for (var i = 0; i < cities.length; i++) {
  let city = cities[i];
  addCity(city.longitude, city.latitude, city.title);
}

function addCity(longitude, latitude, title) {
  pointSeries.data.push({
    geometry: { type: "Point", coordinates: [longitude, latitude] },
    title: title
  });
}

// Make stuff animate on load
chart.appear(1000, 100);





</script>
    
</body>
</html>
