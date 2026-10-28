2. Next, let’s list the project migration status. Remember that earlier, we applied migrations to Django’s installed apps, and now we have created a new app, reviews. Run the following command in the shell or terminal, and it will show the status of model migrations throughout the project (run it from the folder where your manage.py file is stored):

```
python manage.py showmigrations
```

The output for the preceding command is as follows:

```
admin
  [X] 0001_initial
  [X] 0002_logentry_remove_auto_add
  [X] 0003_logentry_add_action_flag_choices
auth
  [X] 0001_initial
  [X] 0002_alter_permission_name_max_length
  [X] 0003_alter_user_email_max_length
  [X] 0004_alter_user_username_opts
  [X] 0005_alter_user_last_login_null
  [X] 0006_require_contenttypes_0002
  [X] 0007_alter_validators_add_error_messages
  [X] 0008_alter_user_username_max_length
  [X] 0009_alter_user_last_name_max_length
  [X] 0010_alter_group_name_max_length
  [X] 0011_update_proxy_permissions
contenttypes
  [X] 0001_initial
  [X] 0002_remove_content_type_name
reviews
  [ ] 0001_initial
sessions
  [X] 0001_initial
```

Here, the [X] mark indicates that the migrations have been applied. Notice the difference that all the other apps’ migrations have been applied except that of reviews. The showmigrations command can be executed to understand the migration status, but this is not a mandatory step while performing model migrations.