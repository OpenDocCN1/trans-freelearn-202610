```
for rw in select * from posts where pk=p_id loop
    ret.id := rw.pk;
    ret.title := rw.title;
    ret.record_data := hstore(ARRAY['title',rw.title,'Title and Content' ,format('%s %s',rw.title,rw.content)]);
    return next ret;
    end loop;
    return;
END;
$$ 
language 'plpgsql';
CREATE FUNCTION
```

As we can see, many things are concentrated in these few lines of PL/pgSQL code:

+   1. `rw posts%ROWTYPE`: With this statement, the rw variable is defined as a container of a single row of the posts table.
+   2. `for rw in select * from posts where pk=p_id loop`: With this statement, we cycle within the result of the selection, assigning the value returned by the select command each time to the rw variable. The next three steps assign the values to the ret variable.
+   3. `return next ret;`: This statement returns the value of the ret variable and goes to the next record of the for cycle.
+   4. `end loop;`: This statement tells PostgreSQL that the for cycle ends here.
+   5. `return;`: This is the return instruction of the function.

An important thing to remember is that the PL/pgSQL language is inside the PostgreSQL transaction system. This means that the functions are executed atomically and that the function returns the results not at the execution of the RETURN NEXT command but at the execution of the RETURN command placed at the end of the function. This may mean that for very large datasets, the PL/pgSQL functions can take a long time before returning results.

# The record type

In an example that we used previously, we introduced the %ROWTYPE data type. In the PL/pgSQL language, it is possible to generalize this concept. There is a data type called record that generalizes the concept of %ROWTYPE.