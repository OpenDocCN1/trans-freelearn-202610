https://www.sqlite.org/lang.html.

As a forensic examiner, the queries you run usually entail filtering and searching through the data, so you may want to look into commands such as CASE, WHEN, and WHERE. We'll see more complex queries over the course of the chapter.

# Running SQL queries

Most forensic tools come with a built-in database viewer that allows the investigator to view the entire database and run queries directly within the tool itself; there are, however, other options:

+   - **The SQLite command-line utility**, which can be downloaded from http://www.sqlite.org, allows direct interaction with the database from the command line.
+   - Other free tools, such as **DB Browser for SQLite**, allow the user to view the database through a **Graphical User Interface (GUI)** and run queries on it. DB Browser for SQLite can be downloaded from https://sqlitebrowser.org.

The investigator should be familiar with the tools used to access the database directly, as these will prove to be huge timesavers when the scope of the examination is limited to one or more SQLite databases. Analyzing these databases with forensic tools such as Cellebrite Physical Analyzer or Magnet AXIOM results in the need to load and parse the entire extraction into memory; this can be very time-consuming, depending on the size of the data extracted.

# Using the SQLite command-line tool

sqlite3 is a command-line utility that is part of the SQLite library. macOS and most Linux distributions have it already installed, while Windows users can download the executable from SQLite's website, which is listed in the preceding section.