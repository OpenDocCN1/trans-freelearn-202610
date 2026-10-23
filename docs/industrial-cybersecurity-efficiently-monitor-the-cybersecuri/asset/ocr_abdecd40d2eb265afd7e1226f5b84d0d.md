8. We can see RITA displays beaconing stats with the show-beacons flag. Let's see what we can find in our environment:

```
pac@secOnion-002:~/tools/rita$ rita show-beacons zeek-1
Score,Source IP,Destination IP,Connections,Avg Bytes,Intvl Range,Size Range,Top Intvl,Top Size,Top Intvl Count,Top Size Count,Intvl Skew,Size Skew,Intvl Dispersion,Size Dispersion
0.996,172.25.100.220,222.222.222.222,594,862,265108,432,600,318,534,569,0,0,0,0
0.834,172.25.100.100,204.79.197.200,98,152,315934,0,7,152,30,98,0,0,0,0
0.762,172.25.100.211,52.241.128.114,1595,534,4551,0,224,534,42,1595,0,0,14,0
0.729,172.25.100.105,34.232.229.251,2517,76,267660,56,9,104,1254,1259,0.235294,0,13,0
...
```

9. As you can see from the output of RITA, a system on the ICS network – 172.25.100.220 – shows clear signs of beaconing to a system on the internet – 222.222.222.222. RITA discovered 594 connections, with an average of 862 bytes transferred per connection and an interval of 600 seconds (10 minutes). The confidence scoring RITA placed on this is 0.996 out of a perfect 1.

To conclude, RITA verified our earlier findings that there is beaconing traffic occurring between the internal machine with IP address 172.25.100.220 and an external IP address of 222.222.222.222. Time to go see what is up with this system.

To explore additional functionality or get more information on RITA, head on over to https://www.activecountermeasures.com/free-tools/rita/.