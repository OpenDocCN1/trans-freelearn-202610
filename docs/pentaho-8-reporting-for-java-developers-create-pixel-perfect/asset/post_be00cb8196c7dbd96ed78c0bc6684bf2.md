
## Testing the localization

After the correct setup of the localization of your Pentaho report, you may want to test it in all the possible languages and countries (in one word, called locale). To achieve that goal, Pentaho Report Designer can be configured to be used with one locale instead of another.

Starting from the Report Designer user interface, you can access the Pentaho Report Designer settings from the upper menu, in File | Configuration... Once selected, the report configuration is shown as in the following screenshot.

In the `core-module` settings, you can recognize the `.environment.designtime.Locale` property, not defined with a default value. To set up the requested locale, check the property and fill the text field on the right, that will become editable. Then click the OK button and refresh the report to make the configuration active.

![](img/9337ac94537bf7e092e1ea6531dce8fd_491_0.png)

## An example of report localization

To see how the localization works in practice, let's develop an example of localized Pentaho Report that you can use in your future tasks. The idea is to reuse a previous example, replacing some static labels with `resource-label`, and see how the report looks in a couple of different languages (we will use English and Spanish as examples).

For this purpose, let's reuse `report_02.prpt` we saw in Chapter 4, *Creating a Report with Report Designer*. If you followed the suggestion, you should have the report saved in your `my_report` folder in your development environment. If not, you can download it from the https://github.com/fcorti/pentaho-8-reporting-for-java-developers repository, from the `Chapter 04 - Creating a report with Report Designer/my_reports` path.

Once the report is opened in the Report Designer, select the `label: Actuals per position title` element in the Structure tab of the report explorer (you can find it under Master Report | Report Header). To replace the label with a `resource-label` element, let's click the Format | Morph | resource-label item from the upper menu. This feature will transform the type of element, maintaining the same common properties. To complete the task, let's set the `value` property (in the common set) with `report.title`.

In exactly the same way (using the morph feature), change the three labels under Master Report | Group: DEPARTMENT | Details Body | Details Header. In case of the position title's header, set the `value` property to `header.positiontitle`. In case of the actual header, set the `value` property to `header.actual`. In case of the budget's header, set the `value` property to `header.budget`.

Now that the report is correctly updated, it's time to set up the resource bundles with the correct key/value pairs. To set up the resources, select File | Resources..., and then select `translations.properties` and click on the Edit button. Once done, fill the property file with the default values (in our case, in English), described as follows: report.title = Actuals per position title
header.positiontitle = Position title

```
header.actual = Actual
header.budget = Budget
```

Click on the OK button when done. To add a new translation file, press the Create button and fill the Entry Name field with `translations_es.properties`. After pressing the OK button, you will see a new property file will be created with the Spanish locale. Select the new property file and click Edit. Once done, fill the property file with the Spanish key/values pairs, described as follows:

```
report.title = Título real por posición
header.positiontitle = Título del puesto
header.actual = Real
header.budget = Presupuesto
```

## Disclaimer

![](img/9337ac94537bf7e092e1ea6531dce8fd_493_0.png)

*I hope Google provided me with the correct translation in Spanish. If not, I apologize in advance to my Spanish friends. It is not my native language, but I thought Spanish would be a good alternative language, considering it is widely used and the Pentaho community is huge in Spanish speaking countries.*

Now that the report is correctly localized (in English as default language and in Spanish), let's preview it by clicking on the eye (👁) icon on the toolbar. The following screenshot shows the preview with the default language (English):

![](img/9337ac94537bf7e092e1ea6531dce8fd_494_0.png)

## Actuals per position title

## Executive Management

| Position title | Actual | Budget |
|---|---|---|
| CEO | 500,000 | 478,750 |
| SVP Partnerships | 531,780 | 519,179 |
| SVP Strategic Development | 226,000 | 226,395 |
| SVP WW Operations | 249,800 | 249,184 |
| CEO | 500,000 | 488,750 |

As you can see, the report title and the three header labels for the fields are the same as in the static version. To change the locale, let's click on File | Configuration... from the upper menu and check the `.environment.designtime.Locale` property. Once done, fill the right text field with `es` (the *i18n* code for Spanish) and press the OK button.

Now that the Report Designer configuration is updated to use the Spanish locale, let's preview the report again by pressing the eye (👁️) icon on the toolbar. The following screenshot shows the preview with the Spanish language:

![](img/9337ac94537bf7e092e1ea6531dce8fd_495_0.png)

## Título real por posición

## Executive Management

| Título del puesto | Real | Presupuesto |
|---|---|---|
| CEO | 500.000 | 478.750 |
| SVP Partnerships | 531.780 | 519.179 |
| SVP Strategic Development | 226.000 | 226.395 |
| SVP WW Operations | 249.800 | 249.184 |
| CEO | 500.000 | 488.750 |

Now it's your turn to deal with it. For example, you could add the localization for French language (fr is the i18n code) or play with more detailed countries, like for example, the Spanish for Argentina (es_AR is the i18n code), the Spanish for Chile (es_CL is the i18n code), and so on.

![](img/9337ac94537bf7e092e1ea6531dce8fd_495_1.png)

> In the https://github.com/fcorti/pentaho-8-reporting-for-java-developers repository, specifically in the Chapter 09 - Internationalization and localization/my_reports path, you can find the report_multilanguage.prpt report developing exactly the example described here. Feel free to open it with Report Designer and play with settings and parameters to understand how to add and customize internationalization and localization.

## Summary

In this chapter, you learnt everything about internationalization and localization using Pentaho Report Designer. You started with the definition of internationalization (and i18n) and then you saw what localization means in case of a Pentaho report. To make everything practical, you saw how to include resource elements (`resource-label`, `resource-field`, and `resource-message`) and how to resolve the content using the resource bundles. At last, you also saw how to test the various localizations for your Pentaho report.

To complete the description, for future reference, you also saw how to localize an existing report in English and Spanish (but other translations could be developed in the same way).

Now that you have read this chapter, you should feel comfortable with all the ways Pentaho Report Designer offers to manage internationalization and localization. This is an advanced task in developing complex Pentaho reports and makes you an expert in dealing with complex reports that can be used in different languages and countries.

In the next chapter, you will learn how to add subreports and cross tabs to your Pentaho reports.

## Subreports and Cross Tabs

In this chapter, you will learn the ins and outs of two advanced reporting topics: subreports and cross tabs. Subreports allow you to embed reusable reports within a master report. As you will see, with the help of examples, subreports can take on many forms, from multi-page detail reports to summary subreports that include charts. As two specific types of subreports, you will learn about table of content element and index element. Along with subreports, you will learn how to incorporate cross tabs in your reports. Cross tabs allow you to compare multiple variables in a single table of data values.

This chapter is written as a reference manual for developers and information technologists. Reading this chapter, you should feel comfortable with all the advanced features of subreports and cross tabs available in Pentaho Report Designer. This is the last chapter dedicated to Report Designer and it covers the last advanced features of this front-end tool of Pentaho Reporting.
