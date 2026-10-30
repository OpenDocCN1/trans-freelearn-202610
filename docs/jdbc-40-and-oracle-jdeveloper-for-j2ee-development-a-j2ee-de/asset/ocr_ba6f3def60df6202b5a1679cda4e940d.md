![](bbox=[0.18, 0.145, 0.82, 0.424])

Click on the OK button in the Insert Out window.

![](bbox=[0.357, 0.475, 0.642, 0.585])

Similarly add c:out tags for the other columns in the result set. The catalogQuery.jsp is listed below:

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;
charset=windows-1252"/>
<title>catalogQuery</title>
</head>
<body><sql:setDataSource driver="oracle.jdbc.OracleDriver"
url="jdbc:oracle:thin:@localhost:1521:ORCL" user="oe"
```