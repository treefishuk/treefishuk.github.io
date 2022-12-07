---
layout: post
title: Acronym Top Trumps For .NET Developers
--- 

After spending a few years coding in the .NET space you will invariably collect enough acronyms to fill a can of Alphabet soup! Some play nice together, however at times you have to choose one acronym based methodology over another. 

This article will break down the most common acronyms, and rank them in terms of: 
 - Usefullness (is it super valuable to know, or can you live without it)
 - Simplicity (how easy is it to explain it to somone who is new at coding)
 - Co-operation (how well does it play with the other acronyms)

## SOC (Seperation of concerns)

 Seperation of concerns is about keeping code files restricted to a single context. Database interaction code should stay seperate from business logic code, which should be seperate from UI code. Can result in things like N-Tier applications.

 |Category       |Score |Note                                                                      |
 |---------------|------|--------------------------------------------------------------------------|
 | Usefullness   | 5    | It doesn't provide much in the way of actual guidance or implementation  |
 | Simplicity    | 10   | Super simple                                                             |
 | Co-operation  | 9    | Works with most other acronyms                                           |

## YAGNI (ya aint gonna need it!)

 YAGNI, is a really simple concept. Its all about not adding unneccessary complexity. Its all about not over-arcitecting a project. Everytime additonal layers, or third party libraries are added you raise the learning curve for other developers who need to work on the code. It also prevents you from using the latest greatest shinny thing and instead re-consider the battle tested old faithful solutions available. 

 |Category       |Score |Note                                                                      |
 |---------------|------|--------------------------------------------------------------------------|
 | Usefullness   | 5    | Experiance will often tell you if you do need something or not.          |
 | Simplicity    | 10   | Super simple.                                                            |
 | Co-operation  | 2    | Might prevent you from wanting to use other acronyms.                    |

## SOLID (Single responsibility, something, something, Dependancy Inversion)

 If were honest the letters in the SOLID acronym are not created equal. The Open-Closed principle, the Liskov Substitution principle and Interface Segregation principle are all well and good. If you want to write testable code though, your need to implement the Dependancy Inversion principle. If you want to prevent changes causing knock-on effects, you need to implement the Single-Responsibility principle. 

 |Category       |Score |Note                                                                      |
 |---------------|------|--------------------------------------------------------------------------|
 | Usefullness   | 5    | If it was just the "D" of SOLID then it would be 10.                     |
 | Simplicity    | 2    | Each part is quite complex and needs working code examples               |
 | Co-operation  | 7    | The "S" is pretty much the same as SOC. Mostly plays well with others    |

## DRY (Don't repeat yourself)

  Copy and pasting code that you've used one place to another place is generally not a great idea. Instead in most cases shared code should be abstracted into a class referenced in both locations. As a rule this can often come into conflict with the "S" of SOLID. For example if you have an Product DTO thats used for a product index page listing and you then need to list products on the home page, you could use the existing DTO, so as to not repeat yourself. Doing so would then mean that updates to the class needed for the index page listing would also potentially affect the listing on the homepage. 

 |Category       |Score | Note                                                                     |
 |---------------|------|--------------------------------------------------------------------------|
 | Usefullness   | 2    | What seams like a repeat often isn't if the context is different         |
 | Simplicity    | 10   | Simple concept                                                           |
 | Co-operation  | 3    | Doesnt play well with the "S" of solid or the "R" of CQRS necessarilly   |

## CQRS (Comand Query Response Segregation)

CQRS in my view is very practical and I wish I had paid more attention to it when I first started coding. The most well known implementation of this pattern is the [Mediatr](https://github.com/jbogard/MediatR) project by Jimmy Bogard. You have a request class which implements an interface that promises a particular kind of result and a handler class that implements the request and returns the promised result. You can then und up doing "Vertical Slices" or use "Feature Folders" to organise your code. 


**Before CQRS:**

```
├── src
│   ├── Validators
│   │   ├── ProductValidator.cs
│   │   ├── OtherValidator.cs
│   │   ├── OtherValidator.cs
│   │   ├── OtherValidator.cs
│   ├── Services
│   │   ├── ProductService.cs
│   │   ├── OtherService.cs
│   │   ├── OtherService.cs
│   │   ├── OtherService.cs
│   │   ├── OtherService.cs
│   ├── DTOs
│   │   ├── ProductDTO.cs
│   │   ├── OtherDTO.cs
│   │   ├── OtherDTO.cs
│   │   ├── OtherDTO.cs
│   │   ├── OtherDTO.cs
│   ├── Mappings
│   │   ├── ProductMappings.cs
│   │   ├── OtherMappings.cs
│   │   ├── OtherMappings.cs
│   │   ├── OtherMappings.cs
```

**With CQRS:**

```
├── src
│   ├── Features
│   │   ├── Products
│   │   │   ├── Commands
│   │   │   |   ├── UpdateProduct
│   │   │   |   |   ├── UpdateProductCommand.cs
│   │   │   |   |   ├── UpdateProductCommandHandler.cs
│   │   │   |   |   ├── UpdateProductsValidator.cs
│   │   │   |   |   ├── UpdateProductResponse.cs
│   │   │   ├── Queries
│   │   │   |   ├── GetProducts
│   │   │   |   |   ├── GetProductsQuery.cs
│   │   │   |   |   ├── GetProductsQueryHandler.cs
│   │   │   |   |   ├── GetProductsValidator.cs
│   │   │   |   |   ├── GetProductsResponse.cs
```

The first version requires jumping around the solution to update the various moving parts. The CQRS version has all the related code next to each other. Much neater in my opinion, and the single responsibilityness (totally a word) means that your code is far less brittle. 


 |Category       |Score | Note                                                                          |
 |---------------|------|-------------------------------------------------------------------------------|
 | Usefullness   | 10   | What seams like a repeat often isn't if the context is different              |
 | Simplicity    | 2    | Bit of a learning curve. More files/classes needed                            |
 | Co-operation  | 9    | Ticks the S and D of solid, SOC is achieved but without creating distance   |


## DDD (Domain Driven Design)

There are sooooooo many parts to DDD... ubiqitus language, bounded contexts, entity classes having behavoir as well as state, aggregate roots, domain events and things like microservices and require a lot of this stuff. Working with an ORM like Entity Framework requires not only learning how to implement DDD but also some quirks to make things work in your ORM of choice. 

 |Category       |Score | Note                                                                          |
 |---------------|------|-------------------------------------------------------------------------------|
 | Usefullness   | 5    | Parts of it are useful in most scenarios but not necessarilly everything      |
 | Simplicity    | 1    | Lots of different parts to it, a lot to learn and understand                  |
 | Co-operation  | 9    | Ticks the S and D of solid, SOC is achieved but without creating distance     |