---
title: "Spring Boot JPA One to One relationships"
date: 2023-01-20T23:33:39Z
draft: false
---

# One to One relationships with Spring boot JPA

## TLDR

- Create two Entities you wish to join (they must have the @Entity annotation)
- On your main Entity create a reference to the other Entity you wish to connect with the @OneToOne and @JoinColumn annotations
- Within the @JoinColumn annotaions add the name and reference column to the other Entity
```
        @OneToOne
        @JoinColumn(name = "address_id", referencedColumnName = "id")
        private Address address;
```
- Now head over to your second Entity and add the reference to the first Entity with the annotation @OneToOne
- Within the @OneToOne Entity add the mappedBy to the first Entity
```
        @OneToOne(mappedBy = "address")
        private Account account;
```

## Introduction:
In this post, we will discuss how to manage one-to-one relationships using Spring Boot and JPA. JPA (Java Persistence API) provides a rich set of features for modeling and mapping relational data in a Java application. Spring Boot, on the other hand, is a popular framework for building production-ready applications quickly and easily. Together, these two technologies make it easy to build and manage one-to-one relationships in your application.

One-to-One Relationships:
A one-to-one relationship is a type of association between two entities, where one entity has a single instance of the other entity. For example, you may have an account and each account can have an address, generally one account can have one address which means you have a one to one relationship between accounts and addresses.

### Account Model
```
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;

@Entity
class Account {
    
        private @Id @GeneratedValue Long id;
        private String name;
        @OneToOne
        @JoinColumn(name = "address_id", referencedColumnName = "id")
        private Address address;

        //Getters and setters here

}
```
### Address Model
```
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;

@Entity
class Address {
    
        private @Id @GeneratedValue Long id;
        @OneToOne(mappedBy = "address")
        private Account account;
        private String addressLine1;
        private String addressLine2;
        private String city;
        private String state;
        private String country;
        private String zipCode;
        private java.sql.Timestamp createdAt;
        private java.sql.Timestamp updatedAt;

        //Getters and setters here

}
```

Managing One-to-One Relationships with JPA:
JPA provides several annotations to manage one-to-one relationships. The two most commonly used annotations are @OneToOne and @JoinColumn.

The @OneToOne annotation is used to specify the relationship between two entities. It is applied on the field that represents the relationship. For example, in the Account and the Address, we can annotate the Address field in the Account entity with @OneToOne.

The @JoinColumn annotation is used to specify the column that will be used to join the two entities. It is applied on the field that represents the relationship. For example, in the Account and Address example, we can annotate the address field in the account entity with @JoinColumn(name = "address_id").

Managing One-to-One Relationships with Spring Boot:
Spring Boot provides several features to make it easy to manage one-to-one relationships in your application. One of the most useful features is the ability to automatically create and update the database schema based on your entity classes.

To use this feature, you need to add the Spring Boot JPA starter to your project's dependencies. Once you've done that, you can use the @Entity annotation to mark your entity classes and the @OneToOne and @JoinColumn annotations to specify the relationships between them.