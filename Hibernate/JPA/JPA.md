# Hibernate / JPA Learning Roadmap

## 1. ORM Fundamentals

-   What is ORM (Object Relational Mapping)
-   Problems with JDBC
-   Benefits of ORM
-   ORM tools overview

## 2. JPA vs Hibernate

-   What is JPA
-   What is Hibernate
-   Specification vs Implementation
-   Why Hibernate is popular

## 3. Project Setup

-   Maven dependencies
-   Spring Boot + Spring Data JPA setup
-   Database configuration
-   application.properties / application.yml

## 4. Entity Mapping Basics

-   @Entity
-   @Table
-   @Id
-   @GeneratedValue
-   @Column
-   Data type mapping

## 5. Persistence Context

-   EntityManager
-   Hibernate Session
-   First Level Cache
-   Entity Lifecycle

## 6. CRUD Operations

-   save()
-   findById()
-   findAll()
-   delete()
-   update()

## 7. Spring Data JPA

-   JpaRepository
-   Repository methods
-   Custom query methods
-   Derived queries

## 8. Entity Relationships

-   @OneToOne
-   @OneToMany
-   @ManyToOne
-   @ManyToMany
-   Join tables

## 9. Fetching Strategies

-   FetchType.EAGER
-   FetchType.LAZY
-   Performance considerations

## 10. Cascading

-   Cascade types
-   When to use cascade
-   Orphan removal

## 11. JPQL & Queries

-   JPQL basics
-   @Query annotation
-   Named queries
-   Native queries

## 12. Pagination & Sorting

-   Pageable
-   PageRequest
-   Sorting results

## 13. Transactions

-   @Transactional
-   Transaction propagation
-   Rollback behavior

## 14. Performance Optimization

-   N+1 Problem
-   Batch fetching
-   Lazy loading optimization

## 15. Caching

-   First level cache
-   Second level cache
-   Query cache

## 16. Testing JPA

-   @DataJpaTest
-   H2 in-memory database
-   Repository testing