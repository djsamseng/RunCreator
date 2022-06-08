# RunCreator
Create run routes using open streets maps

## Query a region for street data
```wget -O test.osm "https://api.openstreetmap.org/api/0.6/map?bbox=-73.2912,41.1319,-73.2717,41.1459"```

## Debugging using the Overpass API
[http://overpass-turbo.eu/#](http://overpass-turbo.eu/#)

## Find adjacent roads
1. Start with a geo location
```
(
  way
  (around:100,41.1387123,-73.2758326)
  [highway~"^(primary|secondary|tertiary|residential)$"]
  [name];
  
>;);out;
```

2. Find the closest road `<way id="">` where the name matches. 
3. Now find connected roads. This entity contains `<nd ref="">`. Those reference ids will match up with intersections with other roads. There are also lat/lon coordinates for each ref. The refs in order are the points for the road and are a straight line. For example for "River Steet" `<wav id="10166782"` the refs are `83925660` (Harbor Road), `3987329574` (bend), `83925658` (bend), `2031064506` (bend), `6538386660` (River Lane), `2031064511` (bend), `83925656` (Cardinal Hill), `83925654` (Taintor), `2200024065` (Post Road)
```
way(10166782);
node(w);
way[highway](bn);
out geom;
```

Click data to see the raw osm data

[Routing Considerations](https://wiki.openstreetmap.org/wiki/Routing#Routing_considerations)
- Minimize traffic signal crossings
```
<node id="83989035" lat="41.1412430" lon="-73.2630555">
  <tag k="highway" v="traffic_signals"/>
</node>
```
- Maximize foot path or bycycle or sidewalk
```
<tag k="bicycle" v="yes"/>
<tag k="foot" v="yes"/>
<tag k="sidewalk" v="right"/>
```
