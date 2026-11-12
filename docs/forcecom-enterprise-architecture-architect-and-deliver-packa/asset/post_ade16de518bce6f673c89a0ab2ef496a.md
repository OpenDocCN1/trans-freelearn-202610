
When the user clicks on a race, the selected race is highlighted and a component application event c:RaceSelected is fired. This event will later be received by the c:RaceResults component. This event is defined as follows:

```
<aura:event type="APPLICATION"
description="Fired when the user selects a race" access="global">
 <aura:attribute name="raceId" type="string"/>
 <aura:attribute name="raceName" type="string"/>
</aura:event>
```

The following component markup uses a HTML unordered list <ul>. The list and items are styled using SLDS to show a list of tiles as specified by the SLDS design guidelines:

```
<aura:component controller="RaceCalendarComponentController" implements="flexipage:availableForAllPageTypes" access="global">
 <!-- Attributes -->
 <aura:attribute name="calendar" type="Object" access="private"/>
 <!-- Event handlers -->
 <aura:handler name="init" value="{!this}" action="{!c.onInit}"/>
 <!-- Component markup -->
 <lightning:card>
 <aura:set attribute="title">
 <lightning:icon iconName="standard:campaign" size="small"/>
 &nbsp;Race Calendar
 </aura:set>
 </lightning:card>
 <div class="slds-p-around--small">
 <ul class="slds-has-dividers--around-space">
 <aura:iteration items="{!v.calendar.Remaining}" var="race">
 <li class="{! 'slds-item ' + (race.Selected ? 'selectedRace' : '')}" onclick="{!c.onRaceClicked}" data-raceid="{!race.Id}" data-racename="{!race.Name}">
 <div class="slds-tile slds-tile--board">
 <h4 class="slds-text-heading--small">{!race.Name}</h4>
 <div class="slds-tile__details slds-text-body--small">
 <p>{!race.Location}</p>
 <p>{!race.RaceDate}</p>
 </div>
 </div>
 </li>
 </aura:iteration>
 <li class="slds-p-around--small">
 <div class="slds-text-heading--small">Completed races</div>
 </li>
 <aura:iteration items="{!v.calendar.Completed}" var="race">
 <li class="{!'slds-item ' + (race.Selected ? 'selectedRace' : '')}" onclick="{!c.onRaceClicked}" data-raceid="{!race.Id}" data-racename="{!race.Name}">
 <div class="slds-tile slds-tile--board">
 <h4 class="slds-text-heading--small">{!race.Name}</h4>
 <div class="slds-tile__details slds-text-body--small">
 <p>{!race.Location}</p>
 <p>{!race.RaceDate}</p>
 </div>
 </div>
 </li>
 </aura:iteration>
 </ul>
 </div>
 </aura:component>
```

Other new and notable aspects of this component are as following:

- The CSS class for the `<li>` elements is driven by an expression that evaluates the value of the `Selected` property from the race binding. This indicates which race is currently selected by the user.
- **HTML5 data attributes** `data-raceid` and `data-racename` are used to assign useful information to the `<li>` elements. This information can later be referenced by the following controller code.
- The `onclick` attribute on the `<li>` elements calls the `c.onRaceClicked` method. The helper `selectRace` function is invoked and uses the HTML5 data attributes to determine the Salesforce record Id for the race the user clicked on. A utility method updates the `Selected` property on items held in the `calendar` attribute. This update in turn causes the `class` attribute on the `<li>` elements to be refreshed and thus the selected race item shown. Finally the `c:RaceSelected` event is fired so that other components can respond accordingly:

```
selectRace : function(component, event ) {
  // Establish the selected race via HTML5 data attributes
  var selectedRaceId =
  event.currentTarget.dataset.raceid;
  var selectedRaceName =
  event.currentTarget.dataset.racename;

  // Mark the race as selected and deselect any selected
  var calendar = component.get('v.calendar');
  this.updateSelectedRace(calendar.Remaining, selectedRaceId);
  this.updateSelectedRace(calendar.Completed, selectedRaceId);
  component.set("v.calendar", calendar);

  // Fire the RaceSelected event
  var compEvent = $A.get("e.c:RaceSelected");
  compEvent.setParams({"raceId" :selectedRaceId } );
  compEvent.setParams({"raceName" :selectedRaceName } );
  compEvent.fire();
}
```

- The Apex Controller method (not shown) to load the race calendar splits and correctly orders the races into two lists (based on whether or not the race has been completed already) which are then sent to the component. This reduces the amount of JavaScript needed in the controller and helper files.

## RaceResults component

The following shows the topmost portion of the c:RaceResults component:

| DRIVER | TEAM | GRID | RACE TIME | POINTS |
|---|---|---|---|---|
| Nico Rosberg | Mercedes | 1 | 1:17:28.89 | 25 |
| Lewis Hamilton | Mercedes | 2 | 0:0:15.100 | 18 |
| Sebastian Vettel | Ferrari | 3 | 0:0:21.0 | 15 |
| Kimi Raikkonen | Ferrari | 4 | 0:0:27.600 | 12 |
| Daniel Ricciardo | Red Bull | 5 | 0:0:45.300 | 10 |
| Valtteri Bottas | Williams | 6 | 0:0:51.0 | 8 |
| Max Verstappen | Red Bull | 7 | 0:0:54.200 | 6 |
| Sergio Perez | Force India | 8 | 0:1:5.0 | 4 |
| Felipe Massa | Williams | 9 | 0:1:5.600 | 2 |
| Nico Hulkenberg | Force India | 10 | 0:1:18.700 | 1 |

The c:RaceResults component follows a similar implementation approach to the c:RaceStandings component, so the full source code is not shown here. You can review the full code through the sample code associated with this chapter:

```
<aura:component controller="RaceResultsComponentController" implements="force:hasRecordId,flexipage:availableForAllPageTypes" access="global">
<!-- Attributes -->
<aura:attribute name="recordName" type="String" access="private"/>
<aura:attribute name="results" type="Object[]" access="private"/>
<!-- Event handlers -->
<aura:handler name="init" value="{!this}" action="{!c.onInit}"/>
<aura:handler event="c:RaceSelected"
action="{!c.handleRaceSelectedEvent}"/>
```

The following code fragments from the component are notable:

- The flexipage:availableForRecordHome interface, states the component is available for dropping on **Record pages** via Lightning App Builder.
- The flexipage:availableForAllPageTypes interface, states the component is available for dropping on the **Home page** via Lightning App Builder.
- The force:hasRecordId interface, lets the container, Lightning Experience or Salesforce1 know that the component wants to know the record ID. This is placed by the container in the recordId attribute (added by the interface), when the component is placed on a Record page.
- The c.onInit method checks the value of the recordId attribute; if it is not null it will load the applicable race results and display them. Since the component can be dropped on the home page this can be null in that context:

```
onInit :function(component, event, helper) {
// If we have a recordId context load the race results
var recordId = component.get('v.recordId');
if(recordId!=null) {
helper.getRaceResults(component, event);
}
}
```

The c.handleRaceSelected is called when the c:RaceSelected event is fired. The method extracts the raceId and raceName from the event parameters and refreshes the race results. This approach allows the component to exist on both the Record and Home pages and still be contextual:

```
handleRaceSelectedEvent :function(component, event, helper) {
// Update race record Id and Name attributes from Event
component.set('v.recordId', event.getParam('raceId'));
component.set('v.recordName',event.getParam('raceName'));
// Retrieve race results
helper.getRaceResults(component, event);
}
```

## RaceSetup component

The following shows the c:RaceSetup component:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_340_0.png)

This component leverages a HTML table to display a table with a list of drivers. The lightning:input component is used to bind checkboxes to the list items. The full markup is not shown; the following code fragments are notable:

```
<aura:component controller="RaceSetupComponentController" implements="force:lightningQuickActionWithoutHeader, force:hasRecordId" access="global">
```

```
<lightning:input
value="{!driver.Selected}" type="checkbox"
```
