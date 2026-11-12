After that, you will see a node named school1 in the Project Explorer view, as we can see in Figure 3.2:

![](bbox=[0.3, 0.13, 0.69, 0.25])

Figure 3.2 – The school1 project in the Project Explorer view

Did you think that something was wrong because the PHP version displayed on the right-hand side of the subnode, PHP Language Library, is lower than the installed version? It seems strange because you have configured the PHP interpreter before. However, if you access the Window | Preferences menu and select the PHP | Installed PHPs item, you will see the right version of PHP, as shown in Figure 3.3:

![](bbox=[0.13, 0.38, 0.87, 0.66])

Figure 3.3 – The configured PHP interpreter in Eclipse

Well, there is nothing wrong. The version that appears in the Project Explorer view is the version of the syntax and token validation for PHP. This means the Eclipse code editor for PHP will validate the elements defined until that version. If you are upset because you want to use the latest features immediately, I ask you to be calm. You can use the latest features without any problems because only what the interpreter understands matters. For example, PHP 8 has named arguments for functions but PHP 7 does not. If you use named arguments in a PHP file, the Eclipse code editor PHP (prepared for PHP 7.4) will highlight the named argument as an error. However, if you are using a PHP 8 interpreter, the code will run successfully. In other words, ignore error messages about features exclusive to PHP 8. They are false alerts that will disappear in the next release of Eclipse.