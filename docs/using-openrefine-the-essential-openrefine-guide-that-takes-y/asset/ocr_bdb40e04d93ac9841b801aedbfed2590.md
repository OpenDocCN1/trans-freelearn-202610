+   - The expression `[^a-zA-Z0-9]\d[^a-zA-Z0-9]` means any digit surrounded by non-letters or non-numbers. For instance, we see that single digits in parentheses are matched. Note that items such as `(123)` will not be matched, but `(3)` will, as we explicitly say that it should be a single digit.

If you really want to match any character, the dot `.` is what you need. This symbol matches anything (except newlines, which are `\n`). While that might not seem useful by itself, it is very useful when combined with other character classes:

+   - The pattern `a.a.a` matches any text where an a is followed by any character, another a, any character, and another a. For instance, texts with the words `dulcamara`, `alabaster`, and `salamander` will match.
+   - The pattern `19..` (there is a space before and after) will match years in the 20th century.
+   - However, you should be careful with the dot: the last pattern also matches `19th` and `19$`, because a dot really means anything. If a year is what you want, `19\d\d` is more accurate.

Now, you might wonder what to do if you want to match an actual dot, actual square brackets, or an actual backslash followed by a "d". The trick is to tell in the regular expression when you don't want a symbol to be recognized as something special. For letters, numbers, spaces, and a few other characters, this is the default. Characters that have a special meaning need to be escaped by preceding them with a backslash `\`. A literal dot is thus represented as `\.`; the backslash itself as `\\`.

+   - To find texts that have three consecutive dots in them, use `\. \. \.`. (This can actually be done in a more handy way, as we'll see in the next section.)
+   - To find texts with a backslash, use `\\`. (There are none in the dataset.)
+   - Texts that contain an opening or a closing square bracket are found with `[\[\]]`. This looks complicated, but it is quite logical: the first and last bracket say "choose any of the things within". And the things within are actual square brackets, but they have to be escaped, so they become `\[` and `\]`.
+   - With `[2]`, you find texts that contain the number 2 ("choose any of the things within", and the only thing within is a 2). With `\[2\]`, you find texts with the number 2 surrounded in square brackets, as the brackets are escaped this time and thus have no special meaning anymore.