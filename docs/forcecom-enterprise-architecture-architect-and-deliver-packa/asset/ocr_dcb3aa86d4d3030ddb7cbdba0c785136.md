```
raceStandings.add(raceStanding);
}
return raceStandings;
}

public class RaceStanding {
  @AuraEnabled
  public Integer Position;
  @AuraEnabled
  public String Driver;
  @AuraEnabled
  public String Team;
  @AuraEnabled
  public Integer Wins;
  @AuraEnabled
  public Integer Points;
}
}
```

> The @AuraEnabled annotation is similar to the @RemoteAction annotation used with Visualforce. Both require the method to be static and thus stateless. They can also be used together on the same method thus helping with migration from Visualforce to Lightning.

# RaceCalendar component

The following shows the topmost portion of the c:RaceCalendar component:

![](bbox=[0.42, 0.605, 0.615, 0.905])