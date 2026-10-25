They have an implementation for raw SQL queries, but it is a limited implementation. It has `ExecuteSqlCommand` that supports only `ExecuteNonQuery` functionality, which means we couldn't use this method for processing SQL queries that return result sets:

```
```csharp
public static int ExecuteSqlCommand(
    [NotNull] this DatabaseFacade databaseFacade,
    RawSqlString sql,
    [NotNull] params object[] parameters);

// Note that this method doesn't start a transaction hence it doesn't use ExecutionStrategy
public static int ExecuteSqlCommand(
    [NotNull] this DatabaseFacade databaseFacade,
    [NotNull] FormattableString sql)
    => ExecuteSqlCommand(databaseFacade, sql.Format, sql.GetArguments());

// Note that this method doesn't start a transaction hence it doesn't use ExecutionStrategy
public static int ExecuteSqlCommand(
    [NotNull] this DatabaseFacade databaseFacade,
    RawSqlString sql,
    [NotNull] IEnumerable<object> parameters)
{
    Check.NotNull(databaseFacade, nameof(databaseFacade));
    Check.NotNull(sql, nameof(sql));
    Check.NotNull(parameters, nameof(parameters));

    var concurrencyDetector = databaseFacade.GetService<IConcurrencyDetector>();

    using (concurrencyDetector.EnterCriticalSection())
    {
        var rawSqlCommand = databaseFacade
            .GetRelationalService<IRawSqlCommandBuilder>()
            .Build(sql, parameters);

        return rawSqlCommand
            .RelationalCommand
            .ExecuteNonQuery(
                databaseFacade.GetRelationalService<IRelationalConnection>(),
                rawSqlCommand.ParameterValues);
    }
}
```
```