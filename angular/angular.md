# Angular Example

This example is part of what is done in this [youtube video](https://www.youtube.com/watch?v=xU5TBS_MsAs). Here is [another video](https://www.youtube.com/watch?v=QdjZyOkcG_w) of making an report change based on inputs

## Angular Update

Make an updated form what runs scala code when button is clicked. 

### Make inputs

First we need to call angularBind first otherwise angular and angularWatch do not work. Once we have that we can make the "form".

```scala

z.angularBind("starttime", "")
z.angularBind("endtime", "")
z.angularBind("userId", "1234")
print(s"""
%angular 
starttime-local: <input type="datetime-local" ng-model="starttime">
endtime-local: <input type="datetime-local" ng-model="endtime">
userId: (1234) <input type="text" ng-model="userId">
<button class="btn btn-success" ng-click="Run=Run+1">Run</button> 

""")

```

Next, lets make a watch function to watch when run is clicked. And when that function is called were going to update some varibles and then run a zeppelin note. Notice  above the we define a function for ng-click, we have to do this other wise we cant watch it. Or so it seems, the might be other ways to watch it, not sure.

```scala
z.angularUnwatch("Run") // unwatch so we don't get more the one call back registered
var starttime = ""
var endtime = ""
var userId = ""
val iCon= z.getInterpreterContext()
z.angularBind("Run", 0)
z.angularWatch("Run", (before, after) => {
    starttime = z.angular("starttime").toString
    endtime =z.angular("endtime").toString
    userId =z.angular("userId").toString
    z.run(7, iCon) // 7 will run 7th note
})

```

This is the 7th note in my noodbook. The idea here is you would have a note that takes the inputs and graphs some things.

```scala
userId
starttime
endtime
```