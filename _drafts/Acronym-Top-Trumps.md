---
layout: post
title: Acronym Top Trumps For .NET Developers
--- 

After spending a few years coding in the .NET space you will invariably collect enough acronyms to fill a can of Alphabet soup. Some play nice together, however at tother times you have to choose one acronym based methodology over another. This article will break down the most common acronyms, and rank them in terms of: 
 - Usefullness (is it super valuable to know, or can you live without it)
 - Simplicity (how easy is it to explain it to somone who is new at coding)
 - Co-operation (how well does it play with the other acronyms)

 ## SOC (Seperation of concerns)

 Seperation of concerns is about keeping code files restricted to a single context. Database interaction code should stay seperate from business logic code, which should be seperate from UI code. 

 |Category       |Score |Note                           |
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

 ## SOLID (Single responsibility, something, something, Dependany Inversion)

 If were honest the letters in the SOLID acronym are not created equal. The Open-Closed principle, the Liskov Substitution principle and Interface Segregation principle are are well and good. If you want to write testable code though, your need to implement the Dependancy Inversion principle. If you want to prevent changes causing knock-on effects, you need to implement the single-Responsibility principle. 

 |Category       |Score |Note                                                                      |
 |---------------|------|--------------------------------------------------------------------------|
 | Usefullness   | 5    | If it was just the "D" of SOLID then it would be 10.                     |
 | Simplicity    | 2    | Each part is quite complex and needs working code examples               |
 | Co-operation  | 7    | The "S" is pretty much the same as SOC. Mostly plays well with others    |

 ## DRY (Don't repeat yourself)

  Copy and pasting code that you've used one place to another place is generally not a great idea. Instead in most cases shared code should be abstracted into a class referenced in both locations. As a rule this can often come into conflict with the "S" of SOLID. For example if you have an Product DTO thats used for a product index page listing and you then need to list products on the home page, you could use the existing DTO, so as to not repeat yourself. Doing so would then mean that updates to the class needed for the index page listing would also potentially affect the listing on the homepage. 

 |Category       |Score | Note                                                                     |
 |---------------|------|--------------------------------------------------------------------------|
 | Usefullness   | 2    | If it was just the "D" of SOLID then it would be 10.                     |
 | Simplicity    | 10   | Simple concept                                                           |
 | Co-operation  | 3    | Doesnt play well with the "S" of solid or the "R" of CQRS necessarilly   |

 ## CQRS (Comand Query Response Segregation)

 ## DDD (Domain Driven Design)