# Backend gists

The tuzhub.io backend is built with the following technologies:

* Java/Kotlin 
* Javalin (tiny http routing layer)
* DInject (dependency injection through code generation - no crazy magic, actually see how java beans are wired with real source code)
* EbeanORM (database orm, query beans are generated via annotation processing)
* JUnit 

This stack is being compiled into a custom web framework collaboration between myself and Robin Bygrave.

## Testing Philosophy

[Third Age of Testing](https://ebean.io/docs/testing/#third-age)

In the second age of testing in memory databases like H2 became available. This allowed developers to use these in memory databases rather than mocking out all persistence API's.

This resulted in far less mocking/stubbing used in tests.

The limitations of testing using in memory DB's like H2 for testing is around the functional difference compared to the "real" target database like Postgres or Oracle etc. Differences in types (e.g. UUID, Array, Json, Hstore, Range types) and features (sql functions, advanced locking, table partitioning etc). 

The limitations with testing against H2 can now be addressed in this "third age" of persistence testing. Docker has made it easier to automate the install/setup of databases on developer machines. Developer machines are powerful enough to run tests fast enough against the "real" target database (where docker versions of databases are functionally the same as the real thing).

* Tests can cover database specific functionality and types (no excuses on test coverage)
* The ease of using docker test containers needs to match the ease of using H2
* We want tests to run against new/clean ephemeral databases
* We need tests to run fast - match in memory database experience
* Build/tests run on the CI server match build/tests run on the developer machine

ebean-test is provided to make testing against docker test containers as simple and good as using H2.

## Web tests

Due to the speed of the stack (around 150ms for entire backend start), we can easily throw up real services that connect to a docker pgsql instance, for each individual test case. We can satisfy the third age of testing this way and run all API level tests as part of a `$ mvn clean test`.


