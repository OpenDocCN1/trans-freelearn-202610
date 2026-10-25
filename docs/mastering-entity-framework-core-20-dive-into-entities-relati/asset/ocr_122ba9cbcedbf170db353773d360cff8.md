The same implementation is available in an asynchronous way as well, which instructs us to perform two implementations if we are planning to have an extension of our requirement. The following screenshot will have asynchronous implementation:

```
154 RawSqlString sql,
155 CancellationToken cancellationToken = default(CancellationToken)
156 => ExecuteSqlCommandAsync(databaseFacade, sql, Enumerable.Empty<object>(), cancellationToken);
157
158 // Note that this method doesn't start a transaction hence it doesn't use ExecutionStrategy
159 public static Task<int> ExecuteSqlCommandAsync(
160 [NotNull] this DatabaseFacade databaseFacade,
161 RawSqlString sql,
162 [NotNull] params object[] parameters)
163 => ExecuteSqlCommandAsync(databaseFacade, sql, (IEnumerable<object>)parameters);
164
165 // Note that this method doesn't start a transaction hence it doesn't use ExecutionStrategy
166 public static async Task<int> ExecuteSqlCommandAsync(
167 [NotNull] this DatabaseFacade databaseFacade,
168 RawSqlString sql,
169 [NotNull] IEnumerable<object> parameters,
170 CancellationToken cancellationToken = default(CancellationToken)
171 {
172 Check.NotNull(databaseFacade, nameof(databaseFacade));
173 Check.NotNull(sql, nameof(sql));
174 Check.NotNull(parameters, nameof(parameters));
175
176 var concurrencyDetector = databaseFacade.GetService<IConcurrencyDetector>();
177
178 using (await concurrencyDetector.EnterCriticalSectionAsync(cancellationToken))
179 {
180 var rawSqlCommand = databaseFacade
181 .GetRelationalService<IRawSqlCommandBuilder>()
182 .Build(sql.Format, parameters);
183
184 return await rawSqlCommand
185 .RelationalCommand
186 .ExecuteNonQueryAsync(
187 databaseFacade.GetRelationalService<IRelationalConnection>(),
188 rawSqlCommand.ParameterValues,
189 cancellationToken);
190 }
```

The EF Core community has an open issue created with respect to our requirement, which is available at https://github.com/aspnet/EntityFramework/issues/1862#issuecomment-220787464.

In the preceding issue thread, a user sirentek provided us with a workaround until Microsoft fixes the same for us in the framework. Let's implement the extension methods required for the anonymous SQL query execution:

+   1. Create a DatabaseExtensions static class that holds the extension methods.