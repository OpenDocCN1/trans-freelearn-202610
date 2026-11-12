```
Description='Season to update the standings for',
Required=True)
global Id seasonId,
@InvocableVariable(
Label='Issue News Letter'
Description='Send the newsletter out
to the teams and drivers'
Required=False)
global Boolean issueNewsLetter;
```

```
private SeasonService.UpdateStandings makeUpdateStandings() {
    SeasonService.UpdateStandings updateStandings =
    new SeasonService.UpdateStandings();
    updateStandings.seasonId = this.seasonId;
    updateStandings.issueNewsLetter = this.issueNewsLetter;
    return updateStandings;
}
}
```

```
@InvocableMethod(
Label='Update the leaderboard of the season'
Description='Updates the standings and optionally
sends the newsletter.')
global static void
updateStandings(
List<UpdateStandingsParameters> parameters) {
    // Marshall parameters into service parameters
    List<SeasonService.UpdateStandings> updateStandings =
    new List<SeasonService.UpdateStandings>();
    for (UpdateStandingsParameters parameter : parameters) {
    updateStandings.add(parameter.makeUpdateStandings());
    }
    // Call the service
    SeasonService.updateStandings(updateStandings);
}
}
```

> In order to isolate the parameters of this method from changes to the Service class methods, the UpdateStandings Apex class is used. This also allows the method to evolve in a different direction if needed.