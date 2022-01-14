---
layout: post
title: Entity Framework 6 vs EF Core 6 Query Performance
--- 

Should you upgrade your project from a Net Framework application using Entity Framework 6 to a Net 6 application using EF Core 6? One of the biggest decisions as to whether you would go down this route which may require a serious investment of time is; will there be a performance benefit?

## Why Performance Matters

There are two main reasons:

- If an application is slow then the users of that application may get frustrated and seek an alternative provider
- A more performant application will use less CPU and Memory and so infrastructure or hosting costs can be reduced

## Creating the Benchmarking Tests

Off the back of a project by Chad Golden, using his DB schema and seed script, I have created a solution that compares the query performance of Entity Framework 6, EF Core 3 and EF Core 6. There is a project in the solution for each version of Entity Framework using BenchmarkDotNet which runs the following queries:

- Retrieve all books
- Retrieve all books, using "No Tracking"
- Retrieve all books, including the author info, using an Include
- Retrieve all books, including the author info, projected to a DTO
- Retrieve all books, including the author info, projected to a DTO, using "No Tracking"
- Retrieve all books, including the author info, using an Include, Ordered By Author
- Retrieve all books, including the author info, projected to a DTO, Ordered By Author
- Retrieve all books, including the author info, projected to a DTO, using "No Tracking", Ordered By Author

## Running the tests

To run the tests you will need to :

- Open the repo in Visual Studio
- Publish the database project (right click it and select "Publish"), to "(localdb)\MSSQLLocalDB" with the name "EFPerformanceDB"
- Run Each Of the projects in "Implementations" in turn in "Release" mode

## Test Results


### Entity Framework 6.4 on Net Framework 4.8

| Method                                         |      Mean |     Error |    StdDev |    Median |
|----------------------------------------------- |----------:|----------:|----------:|----------:|
| LoadAllBookBooks                               | 204.06 ms |  7.952 ms |  5.260 ms | 203.19 ms |
| LoadAllBooksUntracked                          |  32.07 ms |  1.575 ms |  1.042 ms |  32.20 ms |
| LoadAllBookBooksIncludingAuthor                | 302.80 ms | 15.428 ms | 10.204 ms | 302.20 ms |
| LoadAllBooksWithAuthorProjection               |  40.94 ms |  4.892 ms |  3.236 ms |  39.57 ms |
| LoadAllBooksWithAuthorProjectionUntracked      |  40.35 ms |  2.641 ms |  1.747 ms |  40.23 ms |
| LoadAllBooksWithAuthorOrderedByAuthor          | 353.90 ms | 10.720 ms |  7.091 ms | 353.45 ms |
| LoadAllBooksOrderedByAuthorProjection          |  72.25 ms |  1.625 ms |  0.967 ms |  72.33 ms |
| LoadAllBooksOrderedByAuthorProjectionUntracked |  72.90 ms |  2.428 ms |  1.445 ms |  73.40 ms |


### EF Core 3 on Net Core 3.1

|                                         Method |      Mean |     Error |   StdDev |    Median |
|----------------------------------------------- |----------:|----------:|---------:|----------:|
| LoadAllBookBooks                               |  64.89 ms |  3.923 ms | 2.334 ms |  64.68 ms |
| LoadAllBooksUntracked                          |  17.23 ms |  0.977 ms | 0.511 ms |  17.10 ms |
| LoadAllBookBooksIncludingAuthor                | 104.91 ms |  4.156 ms | 2.174 ms | 104.25 ms |
| LoadAllBooksWithAuthorProjection               |  38.71 ms |  3.687 ms | 2.438 ms |  37.74 ms |
| LoadAllBooksWithAuthorProjectionUntracked      |  39.07 ms |  4.986 ms | 2.608 ms |  37.82 ms |
| LoadAllBooksWithAuthorOrderedByAuthor          | 148.25 ms | 14.324 ms | 9.475 ms | 145.93 ms |
| LoadAllBooksOrderedByAuthorProjection          |  71.79 ms |  5.547 ms | 3.669 ms |  70.61 ms |
| LoadAllBooksOrderedByAuthorProjectionUntracked |  71.35 ms |  4.723 ms | 3.124 ms |  69.41 ms |

### EF Core 6 on Net 6

| Method                                         |      Mean |     Error |    StdDev |    Median |
|----------------------------------------------- |----------:|----------:|----------:|----------:|
| LoadAllBookBooks                               |  48.36 ms |  1.434 ms |  0.750 ms |  48.32 ms |
| LoadAllBooksUntracked                          |  18.89 ms |  2.209 ms |  1.461 ms |  18.84 ms |
| LoadAllBookBooksIncludingAuthor                |  92.66 ms |  5.287 ms |  3.146 ms |  91.37 ms |
| LoadAllBooksWithAuthorProjection               |  38.83 ms |  6.200 ms |  3.690 ms |  37.25 ms |
| LoadAllBooksWithAuthorProjectionUntracked      |  38.42 ms |  2.534 ms |  1.676 ms |  37.76 ms |
| LoadAllBooksWithAuthorOrderedByAuthor          | 134.84 ms | 21.555 ms | 14.258 ms | 127.06 ms |
| LoadAllBooksOrderedByAuthorProjection          |  71.02 ms |  5.152 ms |  3.066 ms |  70.51 ms |
| LoadAllBooksOrderedByAuthorProjectionUntracked |  70.88 ms |  4.888 ms |  3.233 ms |  70.79 ms |


## Analysing the results

### Retrieve all books

- EF Core 3 was 68% faster then Entity Framework 6
- EF Core 6 was 76% faster then Entity Framework 6

### Retrieve all books, using "No Tracking"

- EF Core 3 was 46% faster then Entity Framework 6
- EF Core 6 was 41% faster then Entity Framework 6

### Retrieve all books, including the author info, using an Include

- EF Core 3 was 65% faster then Entity Framework 6
- EF Core 6 was 69% faster then Entity Framework 6

### Retrieve all books, including the author info, projected to a DTO

- EF Core 3 was 5% faster then Entity Framework 6
- EF Core 6 was 5% faster then Entity Framework 6

### Retrieve all books, including the author info, projected to a DTO, using "No Tracking"

- EF Core 3 was 3% faster then Entity Framework 6
- EF Core 6 was 5% faster then Entity Framework 6

### Retrieve all books, including the author info, using an Include, Ordered By Author

- EF Core 3 was 58% faster then Entity Framework 6
- EF Core 6 was 61% faster then Entity Framework 6

### Retrieve all books, including the author info, projected to a DTO, Ordered By Author

- EF Core 3 was 0.5% faster then Entity Framework 6
- EF Core 6 was 1.7% faster then Entity Framework 6

### Retrieve all books, including the author info, projected to a DTO, using "No Tracking", Ordered By Author

- EF Core 3 was 0.5% faster then Entity Framework 6
- EF Core 6 was 2.7 faster then Entity Framework 6

## Conclusions

### Entity Framework 6 vs EF Core 3

EF Core 3 was faster in every test. Projected queries however were practically identical.

### EF Core 6 vs EF Core 3

EF Core 6 was marginally faster in every test bar one outlier.

### Projections

The results across all three versions show that projections are by far the best thing you can do to improve performance. However, I was surprised at how little difference there was in the performance of these tests across the three versions, and what little impact "No Tracking" has on projections. So if you have already optimized your Entity Framework 6 queries by selecting to DTOs or using something like AutoMapper projections, then there may not be as much of a saving for you by upgrading as you may imagine.