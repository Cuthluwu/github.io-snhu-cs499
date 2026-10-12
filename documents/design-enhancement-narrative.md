CS 499 Milestone Two: Software Design and Engineering Enhancement

Madison Parker

Southern New Hampshire University

CS 499: Computer Science Capstone

July 13, 2026

## Artifact Description

The artifact I selected for this software design and engineering milestone is the service-layer project I originally created in CS 320: Software Testing, Automation, and Quality Assurance on May 25, 2026. The project contains three related parts: a Contact Service, a Task Service, and an Appointment Service. Each part includes a model class, an in-memory service class, and JUnit tests. The original code was written in Java and used HashMap collections to enforce unique identifiers while supporting basic add, retrieve, update, and delete behavior.

The original project was already functional and closely followed its requirements. Identifiers were immutable, editable fields were validated, appointment dates were defensively copied, and the tests covered valid data, invalid data, boundary values, duplicate identifiers, and missing records. Those strengths made the project a good starting point because the enhancement could focus on architecture and maintainability instead of replacing code that did not work.

My Module One plan first considered the CS 250 SNHU Travel planning materials for the software design category. During the code review, I refined that choice and used the CS 320 service suite as the primary technical artifact because it provides stronger code-level evidence of design decisions. The CS 250 work still supports the planning and requirements side of my portfolio, but the CS 320 project gives me a clearer before-and-after example for this milestone.

## Why I Included This Artifact

I selected this artifact because it represents several parts of software development that are easy to overlook in a larger application. The project has a clear separation between model objects, service operations, storage, validation, and testing. It also shows that software design is not only about adding features. A professional design should make expected behavior obvious, handle invalid data consistently, protect internal state, and leave the code understandable for the next developer.

The Contact, Task, and Appointment classes showcase object-oriented design through encapsulated fields, immutable identifiers, constructors, getters, controlled updates, and reusable validation. The service classes showcase collection management and business operations. The tests demonstrate that the code was developed from explicit requirements instead of only being checked manually after it was written. Because all three services follow a similar pattern, the artifact also gave me a realistic opportunity to decide what behavior should be shared and what behavior should remain specific to each record type.

## Completed Enhancements

I used the findings from my code review to make targeted changes rather than refactoring only for appearance. The main improvements are summarized below.

| Area | Original Condition | Enhanced Design |
| --- | --- | --- |

| Project structure | Classes had no package declarations. | Code is organized into model, service, repository, validation, and exception packages. |

| Validation | Length limits were repeated, and required strings could still be blank. | Named constants and one shared validation utility trim input, reject blanks, and apply consistent messages. |

| Service behavior | Lookup could return null while updating and deleting general exceptions. | All services use the same missing-record policy and focused on exception types. |

| Repeated storage logic | Each service repeated duplicates checks, lookups, deletion, and count behavior. | A small repository contract contains only the operations genuinely shared by the services. |

| State protection | Services stored and returned the same mutable model objects. | Services store defensive copies, return defensive copies, and provide unmodifiable snapshots. |

| Testing and documentation | Tests covered the original requirements but not blank input or exposed state. | Expanded tests cover normalization, blank values, exception consistency, defensive copies, and protected collections. |

The enhanced project now has a professional package structure. Model classes are separate from services, repository behavior, validation rules, and exceptions. I added named constants such as MAX_NAME_LENGTH and MAX_DESCRIPTION_LENGTH so the field rules are readable and are not scattered through the code as unexplained numbers.

I also added a shared ValidationRules utility. It trims required text, rejects null and whitespace-only input, enforces maximum lengths, validates ten-digit phone numbers, and checks appointment dates. This removed repeated validation patterns while keeping the business rules visible inside each model. The models still validate during construction and later updates, so an object cannot become invalid after it has been created.

The service of API is now consistent. Duplicate identifiers raise DuplicateRecordException, and missing identifiers raise RecordNotFoundException. A caller no longer has to remember that one operation returns null while a similar operation throws an exception. I introduced a small RecordRepository interface and an in-memory HashMap implementation for the behavior that was truly repeated. Record-specific operations, including contact field updates and appointment date changes, remain in their concrete services so the abstraction does not hide the purpose of the code.

The most important boundary improvement was protecting stored state. The enhanced services copy an object when it is added and return a copy when it is retrieved. Collection methods return unmodifiable snapshots made from copies. This means outside code cannot silently change the repository by holding a reference to a stored object. Appointment dates continue to use defensive copies because java.util.Date is mutable.

I expanded the test suite to cover the new contract. The new tests check trimming, blank values, exact boundary lengths, custom exceptions, consistent missing-record behavior, defensive copies, and unmodifiable snapshots. I also added a Maven configuration for JUnit 5 and a dependency-free smoke test. The production code compiled with javac -Xlint:all without errors or warnings, and the smoke test passed.

## Course Outcome Progress

The enhancement met the main outcomes I planned to support, with one update to how the artifact is used in the portfolio.

| Outcome | Progress in This Milestone | Evidence |
| --- | --- | --- |

| Outcome 2: Professional communication | Substantial progress | Narrative, JavaDoc, README, traceability notes, consistent naming, and a clear project layout. |

| Outcome 4: Computing techniques and tools | Primary outcome demonstrated | Java, JUnit 5, package design, repository abstraction, validation utilities, Maven configuration, Git-ready structure, and automated verification. |

| Outcome 5: Security mindset | Meaningful progress | Blank-input rejection, defensive copying, protected snapshots, controlled update paths, and clear failure handling reduce invalid or unintended state changes. |

| Outcome 1: Collaborative decision making | Partial support | The enhancement traces code-review findings and earlier requirements into specific design decisions that another developer or reviewer can evaluate. |

| Outcome 3: Algorithms and data structures | Reserved for a later enhancement | The HashMap design remains, but search, filtering, sorting, and complexity analysis are intentionally saved for the algorithm's milestone. |

The main update to my Module One plan is the choice of technical artifacts. The outcome goals did not change, but the code review showed that the CS 320 service suite provides better evidence for software design and engineering than a planning document by itself. Using this artifact lets me show the actual design problem, the reason for each change, the improved code, and the tests that support the new behavior.

I intentionally did not add the planned search and sorting features during this milestone. Those changes are better evidence for the algorithms and data structures category. Keeping them separate makes the purpose of each enhancement clearer and prevents one milestone from becoming a collection of unrelated changes.

## Reflection on the Enhancement Process

The biggest lesson from this enhancement was that clean design is often about making small rules consistent. The original services were short and readable, but their different lookup behavior created an unnecessary decision for every caller. Replacing that inconsistency with one clear service contract made the code easier to use and easier to test without adding much complexity.

I also learned to be careful with abstraction. Since the three services repeat several operations, it would have been easy to build a large generic base class. I decided against that approach because it would hide record-specific behavior and make a small project harder to follow. The repository interface only handles add, required lookup, delete, count, and snapshot operations. The services still own the actions that are meaningful for contacts, tasks, and appointments. This balance reduced duplication without turning the project into a design-pattern exercise.

Protecting mutable state was another important part of the process. In the original code, a service could store an object and later return the same object. Even though the setters validated data, a caller could still change a record without going through the service. Copying on input and output creates a clearer boundary. It also made me think more carefully about the difference between preventing invalid values and controlling where valid changes are allowed to happen.

One challenge was appointment date testing. The original validation compared a value to the current time, which can make tests near the boundary unreliable. I kept the public constructor simple, but added an internal Clock-based path so model tests can use a fixed moment. This gave the tests predictable results without changing how normal application code creates an appointment.

The other challenge was preserving the value of the original work. I did not want the enhanced version to look unrelated to the CS 320 artifact. I kept the same three record types, the same core requirements, and the same in-memory scope. The changes strengthen the structure around that work instead of replacing it with a different application. That process taught me that a useful enhancement should make the original decisions easier to see, not erase them.

Overall, this milestone gave me a stronger understanding of maintainable service design. I improved the architecture, tightened validation, standardized errors, protected internal data, and expanded automated verification. The finished artifact is still small enough to explain clearly in an ePortfolio, but it now looks and behaves more like one organized system instead of three separate assignment folders.

## Submitted Artifact Files

The technical ZIP includes the unmodified original CS 320 files and the complete enhanced project. The enhanced project contains all Java source files, JUnit tests, the Maven configuration, documentation, the code-review traceability table, and the successful verification record. No generated images or decorative assets are used in the artifact.