| Full Name | Title | Manager |
|---|---|---|
| Stephen Jiang | Vice President of Sales | Ken Sánchez |
| David Campbell | Sales Representative | Roger Hamilton |
| Garrett Vargas | Sales Representative | Roger Hamilton |
| Jae Pak | Sales Representative | Amy Alberts |
| Jillian Carson | Sales Representative | Roger Hamilton |
| José Saraiva | Sales Representative | Roger Hamilton |
| Linda Mitchell | Sales Representative | Roger Hamilton |
| Lynn Tsoflas | Sales Representative | Syed Abbas |
| Michael Blythe | Sales Representative | Roger Hamilton |
| Pamela Ansmann-Wolfe | Sales Representative | Roger Hamilton |
| Rachel Valdez | Sales Representative | Amy Alberts |
| Ranjit Varkey Chudukatil | Sales Representative | Amy Alberts |
| Shu Ito | Sales Representative | Roger Hamilton |
| Tete Mensa-Annan | Sales Representative | Roger Hamilton |
| Tsvi Reiter | Sales Representative | Roger Hamilton |
| Syed Abbas | Pacific Sales Manager | Stephen Jiang |
| Roger Hamilton | North American Sales Manager | Stephen Jiang |
| Stephen Jiang | North American Sales Manager | Stephen Jiang |
| Amy Alberts | European Sales Manager | Stephen Jiang |

Figure 12.2: Stephen’s team after he was promoted to Vice President of Sales

We will revisit the preceding scenario in the SCD type 2 (SCD 2) subsection in this chapter for more details.

From a data warehousing standpoint and depending on the business requirements, we have some options to deal with such situations, which leads us to different types of SDC. Keep in mind that the data changes in the source systems before being loaded into the data warehouse (in our examples, the HR system). The source systems are usually transactional. We transform and move the data from the transactional systems via extract, transform, and load (ETL) processes and land the transformed data into the data warehouse, which is where the concept of SCD kicks in. SCD is about how changes in the source systems reflect the data in the data warehouse. These kinds of changes in the source system are not something