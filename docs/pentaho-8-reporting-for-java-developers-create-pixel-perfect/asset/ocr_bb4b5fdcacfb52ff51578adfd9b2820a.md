| File | Description |
|---|---|
| `datadefinition.xml` | Contains the definitions of data sources (pointing to `compound-ds.xml` in the `datasources` folder), functions and expressions, and parameters. |
| `dataschema.xml` | May contain additional metadata used to increase the richness of a data source. |
| `layout.xml` | Defines the primary structure of report bands and elements. |
| `meta.xml` | This file is part of the OpenDocument standard, and contains information about the `.prpt` bundle file. |
| `mimetype` | It is a simple text file that contains the MIME type of the `.prpt` bundle file with value `application/vnd.pentaho.reporting.classic.` |
| `settings.xml` | Contains global configuration properties for a report, as well as a placeholder for future runtime information. |
| `styles.xml` | Defines what page format the report should be rendered as, style rules that dictate how a report should be rendered, global layout processors, a report watermark, and finally the page header and footer sections of a report. |
| `translation.properties, ...` | Localization files. See Chapter 9, *Internationalization and Localization* for further details. The content of the files is exactly the same as those you saw in the chapter dedicated to internationalization and localization. |
| `wizard-specification.xml` | Contains the data specified during the report definition, in case of use of the report wizard. The file is not defined if the report is not created using the report wizard. |