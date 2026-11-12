4. I also the parameters to be visible on the dashboard, so I select **Show Parameter**:

![](bbox=[0.395, 0.09, 0.705, 0.395])

Figure 11.3: Show Parameter

5. If you were to test the parameter now, nothing would happen. We need a calculated field that defines that if a selection from the parameter has been made, Tableau will select the appropriate field. I create the following calculated field to achieve this (this field has been made part of the Starter and Solution workbooks to make your life easier):

```
If [Parameters].[X-Axis] = 'Human Development Index HDI-2014' then [Human Development Index HDI-2014]
Elseif [Parameters].[X-Axis] = 'Gini coefficient 2005-2013' then [Gini coefficient 2005-2013]
Elseif [Parameters].[X-Axis] = 'Adolescent birth rate 15-19 per 1000 20102015' then [Adolescent birth rate 1...]
Elseif [Parameters].[X-Axis] = 'Birth registration funder age 5 2005-2013' then [Birth registration funder ...]
Elseif [Parameters].[X-Axis] = 'Carbon dioxide emissionsAverage annual growth' then [Carbon dioxide emission...]
Elseif [Parameters].[X-Axis] = 'Carbon dioxide emissions per capita 2011 Tonnes' then [Carbon dioxide emissi...]
Elseif [Parameters].[X-Axis] = 'Change forest percentage 1900 to 2012' then [Change forest percentage 190...]
Elseif [Parameters].[X-Axis] = 'Change mobile usage 2009 2014' then [Change mobile usage 2009 2014]
Elseif [Parameters].[X-Axis] = 'Consumer price index 2013' then [Consumer price index 2013]
Elseif [Parameters].[X-Axis] = 'Domestic credit provided by financial sector 2013' then [Domestic credit pr...
Elseif [Parameters].[X-Axis] = 'Domestic food price level 2009 2014 index' then [Domestic food price lev...
Elseif [Parameters].[X-Axis] = 'Domestic food price level 2009-2014 volatility index' then [Domestic food pr...
Elseif [Parameters].[X-Axis] = 'Electrification rate or population' then [Electrification rate or populatio...
Elseif [Parameters].[X-Axis] = 'Expected years of schooling – Years' then [Expected years of schooling - Ye...
Elseif [Parameters].[X-Axis] = 'Exports and imports percentage GDP 2013' then [Exports and imports percentag...
Elseif [Parameters].[X-Axis] = 'Female Suicide Rate 100k people' then [Female Suicide Rate 100k peopl...
```

Figure 11.4: Calculated field