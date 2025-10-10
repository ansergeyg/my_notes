I have recently encountered a weird behaviour when implementing a pager for specific list data.

The environment is Drupal + Mysql.

When you output a pager and expect it to show, say, 3 records per page, you expect to see something like this:

```

page 1       page 2
[1, 2, 3] => [4, 5, 6] => and so on. 

However, what happend is:

page 1       page 2
[1, 2, 3] => [3, 4, 5] => and so on.

```

If you're not attentive enough it is easy to miss this problem on a **larger** set of data. And this is important because you don't **show all the data** now.

The reason is that almost all databases do not guarantee any ordering of selected data if you don't sepcify it explicitly (using order by clause, for example).

Thus, when selected data is fetched it doesn't have any specific "ordering". Which means that you could expect it to appear completely **random**. 

So to fix this issue you should always specify some kind of order on selected data. This way the offset+limit clauses could predictably select correct portion of data with respect to its arguments.
