+   2. Open the view .cshtml file and add a reference to the DataTables, DataTables Bootstrap, and ColVis extension JavaScript files:

```
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.0/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/colvis/1.1.0/js/dataTables.colVis.min.js"></script>
```

+   3. Lastly, using jQuery, add an event handler to initialize the DataTables plugin and the ColVis extension after the page has loaded:

```
<script type="text/javascript">
    $(document).ready(function () {
        $('table').DataTable({
            "dom": 'C<"clear">lfrtip'
        });
    });
</script>
```

+   4. On navigating to the Customers page, you should see a button next to the search box with which you can show or hide columns in the table, as shown in the following screenshot:

![](bbox=[0.21, 0.475, 0.79, 0.77])