---
title: "Spring Boot JPA One to Many relationships"
date: 2023-01-21T02:10:11Z
draft: true
---

Introduction:

One-to-many relationships are a common occurrence in database design, and they can be tricky to implement. In this tutorial, we will learn how to use Spring Boot JPA to create a one-to-many relationship between two entities. We will also cover how to handle bidirectional relationships and how to use JPQL (Java Persistence Query Language) to write custom queries.

Creating the Entities:

Let's start by creating two entities, Department and Employee, that have a one-to-many relationship. The Department entity will have a List of Employee entities, and the Employee entity will have a reference to the Department entity that it belongs to.

```
@Entity
public class Department {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
    private List<Employee> employees;

    // Getters and setters
}

```

```
@Entity
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;

    // Getters and setters
}

```

In the Department entity, we have a List of Employee entities, and we use the @OneToMany annotation to indicate the one-to-many relationship. The mappedBy attribute specifies the property in the Employee entity that maps to this relationship. The Department entity also has a cascade attribute, which specifies that changes made to the Department entity should be cascaded to the Employee entities.

In the Employee entity, we have a reference to the Department entity, and we use the @ManyToOne annotation to indicate the many-to-one relationship. The @JoinColumn annotation specifies the column that will be used to join the two entities.

Handling Bidirectional Relationships:

When working with bidirectional relationships, it's important to keep the relationships in sync. For example, when adding an Employee to a Department, we need to also set the Department for the Employee. We can do this by adding a addEmployee method to the Department entity:

```
public void addEmployee(Employee employee) {
    employees.add(employee);
    employee.setDepartment(this);
}

```

It's also a good practice to use the same method to remove an Employee from a Department:

```
public void removeEmployee(Employee employee) {
    employees.remove(employee);
    employee.setDepartment(null);
}

```

Custom Queries with JPQL:

JPQL is a powerful query language that allows you to write custom queries for your entities. For example, we can write a query to retrieve all Employee