# foodmart-data-hsqldb
Foodmart data set in hsqldb format

This project contains the FoodMart data set as an embedded
HSQLDB database.

It originated as part of the test suite of the
<a href="http://mondrian.pentaho.org">Pentaho Mondrian OLAP engine</a>.

# Schema

Foodmart contains 37 tables:
* 7 fact tables: sales_fact_1997, sales_fact_1998, sales_fact_dec_1998,
  inventory_fact_1997, inventory_fact_1998, salary, expense_fact
* 19 dimension tables: product, customer, time_by_day, employee and more
* 11 aggregate tables

Its size is about 24MB uncompressed, 8MB compressed.

Here is a schema diagram: 

![Foodmart schema diagram](http://rz250.wwwdns.rz.uni-konstanz.de/teaching/ws0607/information-systems/materials/foodmart.jpg)

# Using the data set

The data set is packaged as a jar file that is published to
[Maven Central](https://search.maven.org/#search%7Cga%7C1%7Ca%3Afoodmart-data-hsqldb)
as a Maven artifact. To use the data in your Java application,
add the artifact to your project's dependencies:

```xml
<dependency>
  <groupId>net.hydromatic</groupId>
  <artifactId>foodmart-data-hsqldb</artifactId>
  <version>0.3</version>
</dependency>
```

Now you can connect using Java code:

```java
import java.sql.Connection;

Connection connection = DriverManager.getConnection("jdbc:hsqldb:res:foodmart");
Statement statement = connection.createStatement();
ResultSet resultSet =
  statement.executeQuery("select \"employee_id\", \"full_name\"\n"
      + "from \"foodmart\".\"employee\");
while (resultSet.next()) {
  System.out.println(resultSet.getInt(1) + ":" + resultSet.getString(2));
}
resultSet.close();
statement.close();
connection.close();
```

You can also connect using a JDBC interface such as [sqlline](https://github.com/julianhyde/sqlline).
Make sure that `foodmart-data-hsqldb.jar` is on the class path, and start `sqlline`:

```sql
$ ./sqlline
sqlline version 1.1.9
sqlline> !connect jdbc:hsqldb:res:foodmart sa ""
0: jdbc:hsqldb:res:foodmart> select count(*) from "foodmart"."sales_fact_1997";
+----------------------+
|          C1          |
+----------------------+
| 86837                |
+----------------------+
1 row selected (0.004 seconds)
0: jdbc:hsqldb:res:foodmart> !quit
```

# See also

Similar data sets:
* [foodmart-data-json](https://github.com/julianhyde/foodmart-data-json)
* [scott-data-hsqldb](https://github.com/julianhyde/scott-data-hsqldb)
* [foodmart-queries](https://github.com/julianhyde/foodmart-queries)

## More information

* License: Apache License, Version 2.0
* Author: Julian Hyde
* Blog: http://julianhyde.blogspot.com
* Project page: http://www.hydromatic.net/foodmart-data-hsqldb
* Source code: http://github.com/julianhyde/foodmart-data-hsqldb
* Developers list:
  <a href="mailto:dev@calcite.incubator.apache.org">dev at calcite.incubator.apache.org</a>
  (<a href="http://mail-archives.apache.org/mod_mbox/incubator-calcite-dev/">archive</a>,
  <a href="mailto:dev-subscribe@calcite.incubator.apache.org">subscribe</a>)
* Issues: https://github.com/julianhyde/foodmart-data-hsqldb/issues
* <a href="HISTORY.md">Release notes and history</a>
