
## Predictive analytics software

Originally, predictive analytics was performed by hand, by statisticians on mainframe computers using a progression of various languages such as FORTRAN. Some of these languages are still very much in use today. FORTRAN, for example, is still one of the fastest-performing languages around, and operates with very little memory. So, although it may no longer be as widespread in predictive model development as other languages, it certainly can be used to implement models in a production environment.

Nowadays, there are many choices about which software to use, and many loyalists remain true to their chosen software. The reality is that for solving a specific type of predictive analytics problem, there exists a certain amount of overlap, and certainly the goal is the same. Once you get the hang of the methodologies used for predictive analytics in one software package, it should be fairly easy to translate your skills to another package.

## Open source software

Open source emphasizes agile development and community sharing. Of course, open source software is free, but free must also be balanced in the context of Total Cost Of Ownership (TCO). TCO costs include everything that is factored into a software's cost over a period of time: that not only includes the cost of the software itself, but includes training, infrastructure setup, maintenance, people costs, as well as other expenses associated with the quick upgrade and development cycles which exist in some products.

## Closed source software

Closed source (or proprietary) software such as SAS and SPSS was at the forefront of predictive analytics, and has continued to this day to extend its reach beyond the traditional realm of statistics and machine learning. Closed source software emphasizes stability, better support, and security, with better memory management, which are important factors for some companies.

## Peaceful coexistence

There is much debate nowadays regarding which one is better. My prediction is that they both will coexist peacefully, with one not replacing the other. Data sharing and common APIs will become more common. Each has its place within the data architecture and ecosystem that are deemed correct for a company. Each company will emphasize certain factors, and both open and closed software systems are constantly improving themselves. So, in terms of learning one or the other, it is not an either/or decision. Predictive analytics, per se does not care what software you use. Please be open to the advantages offered by both open and closed software. If you do, that will certainly open up possibilities for working for different kinds of companies and technologies

## Other helpful tools

*Man does not live by bread alone, so it would behoove you to learn additional tools in addition to R, so as to advance your analytic skills:*

- **SQL**: SQL is a valuable tool to know, regardless of which language/package/environment you choose to work in. Virtually every analytics tool will have a SQL interface, and knowledge of how to optimize SQL queries will definitely speed up your productivity, especially if you are doing a lot of data extraction directly from a SQL database. Today's common thought is to do as much pre-processing as possible within the database, so if you will be doing a lot of extracting from databases such as MySQL, PostgreSQL, Oracle, or Teradata, it will be a good thing to learn how queries are optimized within their native framework. In the R language, there are several SQL packages that are useful for interfacing with various external databases. We will be using `sqldf`, which is a popular R package for interfacing with R dataframes. There are other packages that are specifically tailored for the specific database you will be working with.
- **Web extraction tools**: Not every data source will originate from a data warehouse. Knowledge of APIs that extract data from the internet will be valuable to know. Some popular tools include Curl and Jsonlite.
- **Spreadsheets**: Despite their problems, spreadsheets are often the fastest way to do quick data analysis and, more importantly, enable you to share your results with others! R offers several interfaces to spreadsheets but, again, learning standalone spreadsheet skills such as pivot tables and Visual Basic for Applications will give you an advantage if you work for corporations in which these skills are heavily used.
- **Data visualization tools**: Data visualization tools are great for adding impact to an analysis, and for concisely encapsulating complex information. Native R visualization tools are great, but not every company will be using R. Learn some third-party visualization tools such as D3.js, Google Charts, Qlikview, or Tableau.
- **Big data, Spark, Hadoop, NoSQL database**: It is becoming increasingly important to know a little bit about these technologies, at least from the viewpoint of having to extract and analyze data that resides within these frameworks. Many software packages have APIs that talk directly to Hadoop and can run predictive analytics directly within the native environment, or extract data and perform the analytics locally.

## Past the basics

Given that the predictive analytics space is so huge, once you are past the basics, ask yourself what area of predictive analytics really interests you, and what you would like to specialize in. Learning all you can about everything concerning predictive analytics is good at the beginning, but ultimately you will be called upon because you are an expert in certain industries or techniques. This could be research, algorithmic development, or even managing analytics teams.

## Data analytics/research

But, as general guidance, if you are involved in, or are oriented toward, data, the analytics or research portion of data science, I would suggest that you concentrate on data mining methodologies and specific data modeling techniques that are heavily prevalent in the specific industries that interest you.

For example, logistic regression is heavily used in the insurance industry, but social network analysis is not. Economic research is geared toward time series analysis, but not so much cluster analysis. Recommender engines are prevalent in online retail.

## Data engineering

If you are involved more on the data engineering side, concentrate more on data cleaning, being able to integrate various data sources, and the tools needed to accomplish this.

## Management

If you are a manager, concentrate on model development, testing and control, metadata, and presenting results to upper management in order to demonstrate value or return on investment.

## Team data science

Of course, predictive analytics is becoming more of a team sport, rather than a solo endeavor, and the data science team is very much alive. There is a lot that has been written about the components of a data science team, much of which can be reduced to the three basic skills that I outlined earlier.

## Two different ways to look at predictive analytics

Various industries interpret the goals of predictive analytics differently. For example, social science and marketing like to understand the factors which go into a model, and can sacrifice a bit of accuracy if a model can be explained well enough. On the other hand, a black box stock trading model is more interested in minimizing the number of bad trades, and at the end of the day tallies up the gains and losses, not really caring which parts of the trading algorithm worked. Accuracy is more important in the end.

Depending upon how you intend to approach a particular problem, look at how two different analytical mindsets can affect the predictive analytics process:

- 1. Minimize prediction error goal: This is a very common use case for machine learning. The initial goal is to predict using the appropriate algorithms in order to minimize the prediction error. If done incorrectly, an algorithm will ultimately fail and it will need to be continually optimized to come up with the new best algorithm. If this is performed mechanically without regard to understanding the model, this will certainly result in failed outcomes. Certain models, especially over optimized ones with many variables, can have a very high prediction rate, but be unstable in a variety of ways. If one does not have an
