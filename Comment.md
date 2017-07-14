<a name="Comment"></a>

# Comment[¶](#Comment)

TorXakis supports two kinds of [comment](https://en.wikipedia.org/wiki/Comment_(computer_programming)): single line and multiple lines comment.

<a name="Single-line-comment"></a>

## Single line comment[¶](#Single-line-comment)

Single line comments in TorXakis start with '--' (two hyphens) and continue up to the end of the line.

<a name="Multiple-lines-comment"></a>

## Multiple lines comment[¶](#Multiple-lines-comment)

Multiple lines comments in TorXakis start with '{-' and end with '-}'.  
Nesting of multiple lines comments is not supported.  
Take care not to comment with e.g. {-- some comment with double - instead of a single - --} , as this seems to be interpreted as nested comments and this may result in unpredictable errors elsewhere in the code!