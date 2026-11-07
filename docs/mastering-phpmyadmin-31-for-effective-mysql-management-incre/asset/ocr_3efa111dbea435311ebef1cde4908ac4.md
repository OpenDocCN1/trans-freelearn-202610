| Rule | Description |
|------|-------------|
| allow **Julie** from **localhost** | Equivalent to 127.0.0.1 |
| deny % from **all** | **all** can be used as an equivalent to 0.0.0.0/0, meaning any host. Here, the % sign means any user. |

The source part can also be formed with the special names **localnetA**, **localnetB**, or **localnetC**. These represent the complete class A, B, or C network in which the web server is located. Note that phpMyAdmin relies on the $_SERVER["SERVER_ADDR"] PHP parameter for this feature. Usually we will have several rules. Let's say we wish to have the two rules that follow:

```
allow Marc from 45.34.23.12
allow Melanie from all
```

We have to put them in config.inc.php (in the related server-specific section) as follows:

```
$cfg['Servers'][$i]['AllowDeny']['rules'] =
  array('allow Marc from 45.34.23.12',
    'allow Melanie from all');
```

When defining a single rule or multiple rules, a PHP array is used. We must follow its syntax, enclosing each complete rule within single quotes and separating each rule from the next with a comma. Thus, if we have only one rule, we must still use an array to specify it like this:

```
$cfg['Servers'][$i]['AllowDeny']['rules'] =
  array('allow Marc from 45.34.23.12');
```

The next parameter explains the order in which rules are interpreted.

# Order of interpretation for rules

By default, this parameter is empty:

```
$cfg['Servers'][$i]['AllowDeny']['order'] = '';
```

This means that no IP-based verification is made.

Suppose we want to allow access by default, denying access only to some username/ IP pairs. We should use:

```
$cfg['Servers'][$i]['AllowDeny']['order'] = 'deny,allow';
```