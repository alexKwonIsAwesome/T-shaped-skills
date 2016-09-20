# Data Normalization

When creating the database, data have to be unambiguously organized as the creater intended. To state in a brief way, data normalization is the process of migrating data in to well structured and organized table. Normalization involves identifying the data relationship, which object should be in the exact database table. 

Let's have a look on simple example

| Customer | Item purchased        | Purchase price |
|----------|--------------|----------------|
| Thomas   | Shirt        | $40            |
| Maria    | Tennis shoes | $35            |
| Evelyn   | Shirt        | $40            |
| Pajaro   | Trousers     | $25            |

Suppose you want to delete one of the customer's purchased item. If you want to delete Thomas's 'shirt' from the database, probably you would definately delete the price(the second column). Because you know those two columns are related. Also you know row 'Maria' and 'Thomas' are not related because they are seperate row in a table. Furthurmore, if you try to add one more row under the table, it doesn't affet other table rows.

Understanding and solving the problem when dealing of database table is 'Normalization' what you can think naturally.

reference:[http://searchsqlserver.techtarget.com/definition/normalization](http://searchsqlserver.techtarget.com/definition/normalization)
