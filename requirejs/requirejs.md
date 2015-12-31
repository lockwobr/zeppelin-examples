# RequireJS Example

In this example were going to load an external javascript lib to use with d3 to make a graph. I am going to show a generic way to load external javascript, and I am going us that method to load RequireJS. Once we have RequireJS we can then use that to load more libraries, and have then managed by Require.


## Inject a javascript libary

We want some snow on this graph because well I love snow! 

```scala
print(s"""%html
<script>
var firstScript = document.getElementsByTagName('script')[0],
      js = document.createElement('script');
  js.src = 'https://cdnjs.cloudflare.com/ajax/libs/Snowstorm/20131208/snowstorm-min.js';
  js.onload = function () {
    // do stuff with your dynamically loaded script
    snowStorm.snowColor = '#99ccff';
  };
  firstScript.parentNode.insertBefore(js, firstScript);
 
</script>

""")


```

Was not able to figure out how to make it start after the libary was loaded but I dont care in this case. We can just call start in a second block.

```scala
print(s"""%html
<script>
    snowStorm.start();
</script>

""")

```

Thats it you now have snow on your notebook! Also you can use the method for loading external javascript libaries into your notebook. I am not sure if this is a good idea, but it seems to work.


## Inject RequireJS

This is going to be build off what we just did to get some snow! Lets inject requirejs instead of snow and then use require to load what we really want to load more d3 libraries.

```scala

print(s"""%html
<script>

var firstScript = document.getElementsByTagName('script')[0],
      js = document.createElement('script');
  js.src = 'https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.22/require.js';
  js.onload = function () {
    // do stuff with your dynamically loaded script
    snowStorm.snowColor = '#99ccff';
  };
  firstScript.parentNode.insertBefore(js, firstScript);
 
</script>

""")

```
Boom! RequireJS is loaded. Now lets use it to load more d3 magic. Were are going to load [EventDrops](https://github.com/marmelab/EventDrops) and implement this [Demo](http://marmelab.com/EventDrops/) in this notebook.


```scala
print(s"""%html
<div id="chart_placeholder"></div>
<style>
.zoom{
    fill-opacity:0;
}
</style>
<script>

require.config({
    paths: {
        'eventDrops' : 'https://cdn.rawgit.com/marmelab/EventDrops/d55f8b001dc659eacef20edfec8b7159cffaa923/dist/eventDrops' // keep this at a version... not very nice but what can you do
    }
});

requirejs(["eventDrops"], function(eD) {
    var data = [];
    var names = ["Lorem", "Ipsum", "Dolor", "Sit", "Amet", "Consectetur", "Adipisicing", "elit", "Eiusmod tempor", "Incididunt"];
    var endTime = Date.now();
    var month = 30 * 24 * 60 * 60 * 1000;
    var startTime = endTime - 6 * month;
    
    function createEvent (name, maxNbEvents) {
        maxNbEvents = maxNbEvents | 200;
        var event = {
            name: name,
            dates: []
        };
        // add up to 200 events
        var max =  Math.floor(Math.random() * maxNbEvents);
        for (var j = 0; j < max; j++) {
            var time = (Math.random() * (endTime - startTime)) + startTime;
            event.dates.push(new Date(time));
        }
        return event;
    }
    
    for (var i = 0; i < 10; i++) {
        data.push(createEvent(names[i]));
    }

    var color = d3.scale.category20();
    
    var eventDropsChart = d3.chart.eventDrops()
        .eventLineColor(function (datum, index) {
            return color(index);
        })
        .start(new Date(startTime))
        .end(new Date(endTime))
        .hasTopAxis(false)
        .hasBottomAxis(false);
    
    d3.select('#chart_placeholder')
      .datum(data)
      .call(eventDropsChart);
    
});
</script>
""")

```

And it should look some thing like this.

![here is a screenshot](https://github.com/lockwobr/zeppelin-examples/blob/master/requirejs/requirejs-sreenshot.png)