
---
layout: default
title: Madison Parker - CS 499 ePortfolio
---

# Madison Parker

Computer Science ePortfolio — Southern New Hampshire University

[Home](./) · [Code Review](code-review.html) · [Software Design](software-design.html) · [Algorithms](algorithms.html) · [Databases](databases.html)

## Professional Self-Assessment

When I started studying computer science at Southern New Hampshire University, I knew I enjoyed programming, but I was still figuring out what kind of work I wanted to do with it. I was interested in technology, problem-solving, and the possibility of building applications that people could actually use. As I moved through the program, I found myself becoming more interested in what happens beyond getting a program to run. I started paying closer attention to how code is organized, how information moves between different parts of an application, how problems are handled, and whether someone else could understand my work.

That change in perspective is one of the biggest things I am taking away from my degree. I have learned that writing functional code is only part of software development. It also needs to be understandable, maintainable, testable, and designed with the people using it in mind.

My coursework introduced me to several areas of computer science, including software engineering, data structures, algorithms, testing, mobile development, and databases. I worked with Java, Python, SQL, Android Studio, JUnit, and other development tools. Each course gave me a different perspective on solving problems. Some focused on implementing specific functionality, while others challenged me to consider performance, security, testing, and the decisions that go into developing a larger system.

Outside of my coursework, I have also spent time working on creative technical projects involving streaming software, interactive tools, and digital content. Those projects gave me additional experience troubleshooting problems, working with different technologies, and thinking about how software behaves when people interact with it in real time. They also helped me realize how much I enjoy creating things that bring together technical functionality and user experience.

### Communication, Collaboration, and Professional Development

One of the skills I have developed throughout the program is explaining technical decisions instead of assuming the code speaks for itself.

Through my software development coursework, I learned how important it is to understand requirements, document progress, and consider different perspectives when making decisions. My CS 250 work with Agile and Scrum introduced me to user stories, sprint planning, changing requirements, and the importance of communication between developers and stakeholders.

Although my capstone enhancements were completed individually, I approached them with future collaboration in mind. I organized my projects so someone unfamiliar with the original assignments could understand the structure, identify my changes, and review the testing evidence. I also included documentation explaining why I made certain decisions and what limitations remain.

I believe being able to explain a technical choice is just as important as being able to implement it. In a professional environment, another developer may need to maintain my work, a manager may need to understand the reasoning behind a decision, or a stakeholder may want to know how a feature solves their problem. Learning to communicate those details clearly has been an important part of my education.

### Software Engineering and Problem-Solving

Throughout the program, I became more comfortable looking at a problem as a complete system rather than a collection of individual functions.

For me, software engineering means thinking about how the different pieces of an application work together. A program may meet its immediate requirements, but that does not necessarily mean its structure will support future changes.

I learned this while revisiting my earlier Java projects. Code that seemed perfectly reasonable when I first wrote it sometimes became harder to follow after several months. I could recognize repeated logic, inconsistent error handling, or places where one part of the application depended too heavily on another.

Working through those issues helped me understand the value of separating responsibilities, using consistent validation rules, writing meaningful tests, and avoiding unnecessary complexity.

I also developed a stronger appreciation for testing. My CS 320 coursework showed me that tests should do more than confirm that expected inputs produce expected results. They should also examine invalid data, boundary conditions, unexpected behavior, and situations where changes to one part of a program could affect another.

### Data Structures, Algorithms, and Databases

My understanding of algorithms has grown beyond simply finding a solution that produces the correct output. I now pay more attention to why a particular approach makes sense and what its limitations might be.

Working with Java collections helped me better understand the differences between retrieving a record by a known identifier and searching through a larger collection. I learned that choosing a data structure involves considering the operations the application actually needs, rather than automatically choosing something more complicated.

Database development gave me another perspective on software design. Through SQL and my Android coursework, I learned how important it is to think carefully about what information represents, how records relate to one another, and how data should be retrieved and updated.

A database can store information successfully while still having weaknesses in its structure. Understanding relationships, keys, constraints, and data ownership helped me see why database design deserves as much attention as the application interface.

### Security and Responsible Development

Security is another area where my thinking has changed significantly.

Earlier in my education, I primarily associated security with passwords and login screens. As I gained experience, I realized how many security problems begin with ordinary design decisions.

An application needs to validate information, protect sensitive records, limit access appropriately, and avoid exposing internal data unnecessarily. Even a relatively small project can introduce risks if those concerns are ignored.

I now try to think about how software might behave when someone provides unexpected input, attempts to access information that does not belong to them, or uses a feature in a way the original developer did not anticipate.

I am still developing my knowledge of secure programming, but I have learned to treat security as something that should influence development from the beginning rather than something added after the application is finished.

## My CS 499 Capstone Enhancements

For my capstone, I returned to projects from earlier courses and evaluated what I could improve using the skills I developed throughout the Computer Science program.

I chose to focus on three categories: Software Design and Engineering, Algorithms and Data Structures, and Databases.

Rather than replace my original work with completely unrelated projects, I wanted the enhancements to demonstrate how my understanding of software development has changed.

### Software Design and Engineering

My first enhancement focused on the Contact, Task, and Appointment service project from CS 320.

The original project met its requirements, but I identified opportunities to improve its organization and consistency. I separated responsibilities into clearer packages, introduced shared validation, improved exception handling, added repository boundaries, and strengthened the way stored objects are protected from unintended changes.

I also expanded testing to cover the new behavior.

This enhancement helped me understand that improving software does not always require adding new features. Sometimes the most valuable improvements make existing functionality easier to understand, maintain, and trust.

[Explore the Software Design Enhancement](software-design.html)

### Algorithms and Data Structures

For my second enhancement, I returned to the CS 320 service project and focused on its search and retrieval capabilities.

The original services could retrieve records by their identifiers, but they did not support more flexible searches. I added functionality for searching contacts, finding tasks by keyword, and retrieving appointments within a date range.

I also implemented consistent sorting and examined the performance differences between direct identifier lookup and scanning and sorting collections.

One of the more useful lessons from this enhancement came from finding an issue with copying appointment records after their scheduled dates had passed. Correcting that behavior reminded me that testing often reveals problems outside the specific feature being added.

[Explore the Algorithms and Data Structures Enhancement](algorithms.html)

### Databases

My final enhancement focused on the Android Weight Tracker application I developed in CS 360.

The original application used SQLite to store account information and weight records, but its database structure had limitations involving data relationships, password storage, and record ownership.

I redesigned the database to use separate user and weight-entry records, improved validation and account-specific operations, introduced stronger password-verifier storage, and added ordered history and progress reporting.

I also worked on preserving existing information during database upgrades rather than deleting it.

This project was especially meaningful to me because I am interested in applications that combine software development with useful personal data. It gave me an opportunity to think about both the technical requirements of storing information and the responsibility of protecting it.

The verification work included standalone Java and SQLite tests. A complete Android Studio build was not performed in the final preparation environment, and I have documented that limitation rather than presenting those tests as something they were not.

[Explore the Database Enhancement](databases.html)

## What I Am Taking Away From This Experience

Looking at the three enhancements together, I can see how much my approach to programming has changed.

At the beginning of my degree, I was often focused on completing an assignment and making sure the program worked. Now I find myself asking different questions. Is this the best way to organize the code? What happens when something goes wrong? Could someone else work with this project? Are the results predictable? Is the data being handled responsibly?

I have also learned that being a good developer involves recognizing what still needs improvement. Not every problem requires a complicated solution, and not every feature needs to be added immediately. Understanding scope, making reasonable trade-offs, and documenting limitations are skills I want to continue developing.

My professional interests are in software development, particularly application development, databases, and technology that connects different systems. I would also like to continue my education and pursue graduate study as I build more professional experience.

I am proud of the progress I have made, but I also know there is more to learn. I want to gain additional experience with larger software projects, collaborative development, deployment, and advanced security practices.

This portfolio represents more than three updated assignments to me. It shows the difference between the way I approached programming when I began the Computer Science program and the way I approach it now.

I am leaving the program with stronger technical skills, a better understanding of the responsibility involved in developing software, and a clearer idea of the kind of work I want to pursue.

---

## Explore My Portfolio

Each section includes an explanation of the enhancement, the original and improved source code, and the accompanying narrative.

- [Code Review — Video Presentation](code-review.html)
- [Software Design and Engineering — CS 320](software-design.html)
- [Algorithms and Data Structures — CS 320](algorithms.html)
- [Databases — CS 360 Android Weight Tracker](databases.html)

