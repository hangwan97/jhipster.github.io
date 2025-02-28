---
layout: default
title: Filtering
permalink: /entities-filtering/
sitemap:
    priority: 0.7
    lastmod: 2017-08-22T00:00:00-00:00
---

# <i class="fa fa-filter"></i> Filtering your entities

## Introduction

After the basic CRUD functionalities are implemented for an entity, there is a very common request to create various filters for the attributes of the entity,
so the server could be used more effectively. These filters should be sent as the request parameters, so any client - and any browser - could use it.
Additionally, these filters should follow a resonable, and concise pattern, and they must be allowed combining them freely.

## How to activate

_Note_: `filter` is not compatible with `reactive`.
When generating an entity with `jhipster entity` command, select services or service implementation to enable filtering on this entity. 

If you want to enable filtering for existing entities, you can modify the entity configuration in your projects `.jhipster` directory, by setting `service` to `serviceClass` or `serviceImpl` from `no`, and `jpaMetamodelFiltering` to `true` and then re-generate with `jhipster entity <entity name>`.

When using JDL, add a line `filter <entity name>` to your JDL file and re-import the definitions with `jhipster jdl` command.

## Public interface

For each entity, you can enable filtering in the entity generator, and after, you can call your `/api/my-entity` GET endpoint with the following parameters :

* For each *xyz* field
    * *xyz.equals=someValue*
        - To list all the entities, where xyz equals to 'someValue'
    * *xyz.in=someValue,otherValue*
        - To list all the entities, where xyz equals to 'someValue' or 'otherValue'
    * *xyz.specified=true*
        - To list all the entities, where xyz is not null, specified.
    * *xyz.specified=false*
        - To list all the entities, where xyz is null, unspecified.
* If *xyz*'s type is string:
    * *xyz.contains=something*
        - To list all the entities, where xyz contains 'something'.
* If *xyz*'s is either any of the number types, or the date types.
    * *xyz.greaterThan=someValue*
        - To list all the entities, where xyz is greater than 'someValue'.
    * *xyz.lessThan=someValue*
        - To list all the entities, where xyz is less than 'someValue'.
    * *xyz.greaterThanOrEqual=someValue*
        - To list all the entities, where xyz is greater than or equal to 'someValue'.
    * *xyz.lessThanOrEqual=someValue*
        - To list all the entities, where xyz is less than or equal to 'someValue'.

They can be combined freely.

A good way to experience the expressiveness of this filter API is to use it from swagger-ui in the API docs page of your JHipster application.

![]({{ site.url }}/images/entities_filtering_swagger.png)

## Implementation

When this feature is enabled, a new service named as `EntityQueryService` and an `EntityCriteria` is generated. Spring will convert the request parameters into the fields of the `EntityCriteria` class.

In the `EntityQueryService`, we convert the criteria object into a static, and type safe, JPA query object. For this, it is **required** that the **static metamodel generation is enabled** in the build. See the [JPA Static Metamodel Generator documentation](https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#tooling-modelgen) for details.

To prove that the generated criteria is working, and Spring is well configured, the `EntityResourceIntTest` is extended with lots of test cases, one for each individual filter.

### Angular

When using Angular, the proper way to take advantage of this useful feature would look like this:

* Equals (same applies for `contains` and `notEquals`)
```javascript
this.bookService.query({'title.equals':someValue}).subscribe(...);
```
* greaterThan (same applies for `lessThan`, `greaterThanOrEqual` and `lessThanOrEqual` when using `date` and `number` data types)
```javascript
this.bookService.query({'id.greaterThan':value}).subscribe(...);
this.bookService.query({'birthday.lessThanOrEqual':value}).subscribe(...);
```
* In (same applies for `notIn`)
```javascript
this.bookService.query({'id.in':[value1, value2]}).subscribe(...);
```
* Specified
```javascript
this.bookService.query({'author.specified':true}).subscribe(...);
```

## Limitations

Currently only SQL databases (with JPA) is supported, with the separate service or separate service implementation/interface combination.
