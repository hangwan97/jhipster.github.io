---
layout: default
title: Creating an application
permalink: /creating-an-app/
redirect_from:
  - /creating_an_app.html
sitemap:
    priority: 0.7
    lastmod: 2018-03-18T18:20:00-00:00
---

# <i class="fa fa-rocket"></i> Creating an application

_**Please check our [video tutorial]({{ site.url }}/video-tutorial/) on creating a new JHipster application!**_

1. [Quick start](#1)
2. [Questions asked when generating an application](#2)
3. [Using a blueprint](#5)
4. [Command-line options](#3)
5. [Tips](#4)

<h2 id="1">Quick start</h2>

First of all, create an empty directory in which you will create your application:

`mkdir myapplication`

Go to that directory:

`cd myapplication/`

To generate your application, type:

`jhipster`

Answer the questions asked by the generator to create an application tailored to your needs. Those options are described in [the next section](#2).

Once the application is generated, you can launch it using Maven (`./mvnw` on Linux/MacOS/Windows PowerShell, `mvnw` on Windows Cmd) or Gradle (`./gradlew` on Linux/MacOS/Windows PowerShell, `gradlew` on Windows Cmd).

**Note** if you are using Maven and changed front end files after first run of the `./mvnw` command then you must run `./mvnw -Pwebapp` to see the latest front end version (Gradle detects front end changes automatically and recompiles front end if needed).

The application will be available on [http://localhost:8080](http://localhost:8080)

**Important** if you want to have "live reload" of your JavaScript/TypeScript code, you will need run `npm start` or `yarn start`. You can go to the [Using JHipster in development]({{ site.url }}/development/) page for more information.

If you are using "live reload" then you can speed up server start up by excluding client side tasks by `./mvnw -P-webapp` or `./gradlew -x webapp`. It speeds up especially Gradle.

<h2 id="2">Questions asked when generating an application</h2>

_Some questions change depending on the previous choices you have made. For example, you won't need to configure an Hibernate cache if you didn't select an SQL database._

### Which _type_ of application would you like to create?

Your type of application depends on whether you wish to use a microservices architecture or not. A full explanation on microservices is [available here]({{ site.url }}/microservices-architecture/), if unsure use the default "Monolithic application".

You can either use:

*   Monolithic application: this a classical, one-size-fits-all application. It's easier to use and develop, and is our recommended default.
*   Microservice application: in a microservices architecture, this is one of the services.
*   Microservice gateway: in a microservices architecture, this is an edge server that routes and secures requests.

### What is the base name of your application?

This is the name of your application.

### What is your default Java package name?

Your Java application will use this as its root package. This value is stored by Yeoman so that the next time you run the generator the last value will become default. 
You can override it by providing a new value.

### Do you want to use the JHipster Registry to configure, monitor and scale your application?

The [JHipster Registry]({{ site.url }}/jhipster-registry/) is an Open Source tool used to manage your application at runtime.

It is required when using a microservices architecture (this is why this question is only asked when generating a monolith).

### Which _type_ of authentication would you like to use?

Answers to this question depend on previous answers. For example, if you selected the [JHipster Registry]({{ site.url }}/jhipster-registry/) above, you can only use JWT authentication.

Here are all the possible options:

*   JWT authentication: use a [JSON Web Token (JWT)](https://jwt.io/), which is the default choice and what most people use.
*   OAuth 2.0 / OIDC Authentication: this uses an OpenID Connect server, like [Keycloak](https://www.keycloak.org/) or [Okta](https://developer.okta.com), which handles authentication outside of the application. This is more secured than JWT, but it requires to set up an OpenID Connect server, so it's a bit more complex. Please note that by default JHipster will synchronize the user data from the OpenID Connect server, and for this it will need a database.
*   HTTP Session Authentication: the classical session-based authentication mechanism, which is what people usually do with [Spring Security](http://docs.spring.io/spring-security/site/index.html).

You can find more information on our [securing your application]({{ site.url }}/security/) page.

### Which _type_ of database would you like to use?

You can choose between:

- An SQL database (H2, MySQL, MariaDB, PostgreSQL, MSSQL, Oracle), which you will access with Spring Data JPA
- [MongoDB]({{ site.url }}/using-mongodb/)
- [Cassandra]({{ site.url }}/using-cassandra/)
- [Couchbase]({{ site.url }}/using-couchbase/)
- [Neo4j]({{ site.url }}/using-neo4j/)
- No database (only available when using a [microservice application]({{ site.url }}/microservices-architecture/) with JWT authentication)

### Which _production_ database would you like to use?

This is the database you will use with your "production" profile. To configure it, please modify your `src/main/resources/config/application-prod.yml` file.

If you want to use Oracle, you might need to be aware of current limitations when [using Oracle database]({{ site.url }}/using-oracle/).

### Which _development_ database would you like to use?

This is the database you will use with your "development" profile. You can either use:

*   H2, running in-memory. This is the easiest way to use JHipster, but your data will be lost when you restart your server.
*   H2, with its data stored on disk. This is a better option than running in-memory, as you won't lose your data upon application restart.
*   The same database as the one you chose for production: it's a bit more complex to set up, but it should be better in the end to work on the same database as the one you will use in production. This is also the best way to use liquibase-hibernate as described in [the development guide]({{ site.url }}/development/).

To configure it, please modify your `src/main/resources/config/application-dev.yml` file.

### Do you want to use the Spring cache abstraction?

The Spring cache abstraction allows to use different cache implementations: you can use [ehcache](http://ehcache.org/) (local cache), [Caffeine](https://github.com/ben-manes/caffeine) (local cache), [Hazelcast](http://www.hazelcast.com/) (distributed cache), [Infinispan](http://infinispan.org/) (distributed cache), [Memcached](https://memcached.org/) (another distributed cache) or [Redis](https://redis.io/) (configured as a single server cache). This can have a very positive impact on your application's performance, and hence it is a recommended option.

### Do you want to use Hibernate 2nd level cache?

This option will only be available if you selected to use an SQL database (as JHipster will use Spring Data JPA to access it) and selected a cache provider in the previous question.

[Hibernate](http://hibernate.org/) is the JPA provider used by JHipster, and it can use a cache provider to greatly improve its performance. As a result, we highly recommend you to use this option, and to tune your cache implementation according to your application's needs.

### Would you like to use Maven or Gradle?

You can build your generated Java application either with [Maven](http://maven.apache.org/) or [Gradle](http://www.gradle.org/). Maven is more stable and more mature. Gradle is more flexible, easier to extend, and more hype.

### Which other technologies would you like to use?

This is a multi-select answer, to add one or several other technologies to the application. Available technologies are:

#### API first development using swagger-codegen

This option lets you do [API-first development]({{ site.url }}/doing-api-first-development) for your application by integrating the [Swagger-Codegen](https://github.com/swagger-api/swagger-codegen) into the build.

#### Search engine using ElasticSearch

[Elasticsearch](https://github.com/elastic/elasticsearch) will be configured using Spring Data Elasticsearch. You can find more information on our [Elasticsearch guide]({{ site.url }}/using-elasticsearch/).

#### Clustered HTTP sessions using Hazelcast

By default, JHipster uses a HTTP session only for storing [Spring Security](http://docs.spring.io/spring-security/site/index.html)'s authentication and authorisation information. You can choose to put more data in your HTTP sessions.
Using HTTP sessions will cause issues if you are running in a cluster, especially if you don't use a load balancer with "sticky sessions".
If you want to replicate your sessions inside your cluster, choose this option to have [Hazelcast](http://www.hazelcast.com/) configured.

#### WebSockets using Spring Websocket

Websockets can be enabled using Spring Websocket. We also provide a complete sample to show you how to use the framework efficiently.

#### Asynchronous messages using Apache Kafka

Use [Apache Kafka]({{ site.url }}/using-kafka/) as a publish/subscribe message broker.

#### Asynchronous messages using Apache Pulsar

Use [Apache Pulsar]({{ site.url }}/using-pulsar/) as a publish/subscribe message broker.

### Which _Framework_ would you like to use for the client?

The client-side framework to use.

You can either use:

*   Angular
*   React
*   Vue

### Would you like to use a Bootswatch theme?

The client theme to use.

You can use any themes from [Bootswatch](https://bootswatch.com/) or keep the default one.

### Would you like to use the Sass stylesheet preprocessor for your CSS?

[Sass](https://sass-lang.com/) a great solution to simplify designing CSS. To be used efficiently, you will need to run a [Webpack](https://webpack.js.org) dev server, which will be configured automatically.

### Would you like to enable internationalization support?

By default JHipster provides excellent internationalization support, both on the client side and on the server side. However, internationalization adds a little overhead, and is a little bit more complex to manage, so you can choose not to install this feature.

Please note that JHipster covers only UI internationalization. For data internationalization, you will need to code it yourself in JPA/Hibernate layer.

### Which testing frameworks would you like to use?

By default JHipster provide Java unit/integration testing (using Spring's JUnit support) and JavaScript unit testing (using Jest). As an option, you can also add support for:

*   Performance tests using Gatling
*   Behaviour tests using Cucumber
*   Angular integration tests with Protractor

You can find more information on our ["Running tests" guide]({{ site.url }}/running-tests/).

### Would you like to use incremental Liquibase changelogs?

JHipster optionally creates incremental changelogs for you, so you don't need to recreate the database or to manually generate Liquibase diff.

Run JHipster using `--incremental-changelog` option anytime to enable it.

When executing JHipster, entities contain two states:

*   The old state which is already saved to disk
*   The new state, which is in the memory (generated from jdl or prompts)

A diff is generated between them and changelogs are created.

Supported features:

*   Create/remove fields
*   Create/remove relationships
*   JDL and prompts

Doesn't support attribute changes like type and constraints.

Conflicts with:

*   `--fork` option, since it saves to disk overriding the old state.

### Would you like to install other generators from the JHipster Marketplace?

The [JHipster Marketplace]({{ site.url }}/modules/marketplace/) is where you can install additional modules, written by third-party developers, to add non-official features to your project.

<h2 id="5">Using a blueprint</h2>

JHipster 5 introduces the concept of a blueprint. Blueprints are JHipster modules that can provide custom client/server side templates that will override the ones from JHipster. For example, the [Kotlin blueprint](https://github.com/jhipster/jhipster-kotlin) replaces most of the Java server side code with Kotlin.

For example, to use the Kotlin blueprint pass the name of the blueprint like below while generating an app.

```bash
jhipster --blueprint kotlin
```

The name of the blueprint is saved in the `.yo-rc.json` and will be automatically used while executing sub-generators like `entity`, `spring-controller` and `spring-service`.

If a blueprint doesn't implement a specific sub-generator, it will be skipped and the JHipster templates for the same sub-generator will be used.

**Note:** An application can use only one blueprint, multiple blueprints are not supported yet.

<h2 id="3">Command-line options</h2>

You can also run JHipster with some optional command-line options. Reference for those options can be found by typing `jhipster app --help`.

Here are the options you can pass:

* `--help` - Print the generator's options and usage
* `--blueprint` - Specify a blueprint to use. For example `jhipster --blueprint kotlin`
* `--skip-cache` - Do not remember prompt answers (Default: false)
* `--skip-git` - Do not add the generated project to Git automatically (Default: false)
* `--skip-install` - Do not automatically install dependencies (Default: false)
* `--skip-client` - Skip the client-side application generation, so you only have the Spring Boot back-end code generated (Default: false).
* `--skip-server` - Skip the server-side application generation, so you only have the front-end code generated (Default: false).
* `--skip-user-management` - Skip the user management generation, both on the back-end and on the front-end (Default: false)
* `--i18n` - Disable or enable i18n when skipping client side generation, has no effect otherwise (Default: true)
* `--auth` - Specify the authentication type when skipping server side generation, has no effect otherwise but mandatory when using `skip-server`
* `--db` - Specify the database when skipping server side generation, has no effect otherwise but mandatory when using `skip-server`
* `--with-entities` - Regenerate the existing entities if they were already generated (using their configuration in the `.jhipster` folder) (Default: false)
* `--skip-checks` - Skip the check of the required tools (Default: false)
* `--jhi-prefix` - Add prefix before services, components and state/route names (Default: jhi)
* `--entity-suffix` - Add suffix after entities class names (Default: empty string)
* `--dto-suffix` - Add suffix after DTOs class names (Default: DTO)
* `--yarn` - Use Yarn instead of NPM (Default: false)
* `--prettier-java` - Use [prettier-java](https://github.com/jhipster/prettier-java) for formatting all Java classes (Default: false)
* `--experimental` - Enable experimental features. Please note that these features may be unstable and may undergo breaking changes at any time
* `--skip-fake-data` - Skip generation of fake data for development
* `--creation-timestamp` - Set a timestamp for reproducible build. Timestamp should be a parsable js date ex: 2019-01-01. Must be used with --with-entities or import-jdl (generator-jhipster > 6.5.1)

<h2 id="4">Tips</h2>

You can also use the Yeoman command-line options, like `--force` to automatically overwrite existing files. So if you want to regenerate your whole application, including its entities, you can run `jhipster --force --with-entities`.
