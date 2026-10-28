2. In your terminal (or Command Prompt), change to the bookr project directory (using the cd command), and then run the manage.py runserver command as follows:

```
PYTHON3 MANAGE.PY RUNSERVER
```

Note
On Windows, you may need to run and replace python3 with just python to make the command work every time you run it.

This command starts the Django development server. You should get output similar to the following:

```
WATCHING FOR FILE CHANGES WITH StatReloader

PERFORMING SYSTEM CHECKS...

SYSTEM CHECK IDENTIFIED NO ISSUES (0 SILENCED).

YOU HAVE 18 UNAPPLIED MIGRATION(S). YOUR PROJECT MAY NOT WORK PROPERLY UNTIL YOU APPLY THE MIGRATIONS FOR APP(S): ADMIN, AUTH, CONTENTTYPES, SESSIONS.
RUN 'PYTHON MANAGE.PY MIGRATE' TO APPLY THEM.
JANUARY 01, 2022 - 02:54:03
DJANGO VERSION 4.0, USING SETTINGS 'BOOKR.SETTINGS'
STARTING DEVELOPMENT SERVER AT HTTP://127.0.0.1:8000/
QUIT THE SERVER WITH CONTROL-C.
```

You will probably receive some warnings about unapplied migrations, but that’s okay for now.

3. Open up a web browser and go to http://127.0.0.1:8000/, which will show you the Django welcome screen (Figure 1.2). If you see this, you will know that your Django project was created successfully and all is working fine for now: