---
layout: default
title: Algorithms and Data Structures
---

[Home](./) · [Code Review](code-review.html) · [Software Design](software-design.html) · [Algorithms](algorithms.html) · [Databases](databases.html)

# Algorithms and Data Structures

This CS 320 enhancement builds on the software-design baseline to introduce contact and task searches, appointment date-range filtering, deterministic sorting, immutable results, and documented algorithmic trade-offs.

## Original and enhanced artifacts

- [Download the unmodified original source](artifacts/algorithms-original-source.zip)
- [Download the enhanced source](artifacts/algorithms-enhanced-source.zip)
- [Download the complete graded milestone package and supporting evidence](artifacts/algorithms-complete-milestone-submission.zip)
- [Download the enhancement narrative (Word)](documents/algorithms-enhancement-narrative.docx)

## Enhancement narrative

CS 499 Milestone Three

Enhancement Two: Algorithms and Data Structures

Madison Parker
Southern New Hampshire University
CS 499: Computer Science Capstone
August 1, 2026

Resubmission Note

The first artifact upload was packaged incorrectly and did not open for my instructor. I rebuilt the submission instead of only renaming the old file. The new ZIP opens directly to a READ ME file and numbered folders, contains no ZIP inside another ZIP, and includes an exact enhancement map. The map identifies the production file, method, line range, and test evidence for every change I claim in this narrative. The artifact ZIP and this Word narrative are submitted as two separate files as required.

### 1. Artifact Description

The artifact is the Contact, Task, and Appointment service project that I created in CS 320: Software Testing, Automation, and Quality Assurance in May 2026. It is a Java application with three model classes and three service classes. The original assignment stored records in memory and supported adding, retrieving, updating, and deleting a record by its unique ID. JUnit tests checked the field requirements, duplicate IDs, missing records, and update behavior.

I used the same artifact for my first CS 499 enhancement because the original code had enough working behavior to improve instead of replace. That enhancement reorganized the packages, centralized validation, added focused exceptions, introduced a repository boundary, protected stored objects with defensive copies, and expanded the tests. The version in the folder 02_BASELINE_BEFORE_MILESTONE3 is the exact starting point for this milestone. The completed algorithm version is in 03_MILESTONE3_ENHANCED.

### 2. Why I Selected It and What the Enhancement Shows

I chose this artifact for algorithms and data structures because it had a specific limitation. The original services were efficient when the caller knew an exact ID, but they were not useful when the caller remembered only part of a name, a word in a task, or an appointment date. That gave me a real reason to add algorithms instead of forcing an unrelated data structure into the project.

I kept the HashMap-backed repository. Add, get, and delete by ID remain O(1) on average. Replacing the map with a TreeMap would have made those operations O(log n) and would only order the IDs. It would not solve the three orders the application actually needs: contacts by name, tasks by task name, and appointments by date. I therefore kept the strongest part of the existing design and added an O(n) scan for broader criteria. If m records match, only those m records are copied and sorted, which costs O(m log m) time and O(m) additional space.

The final code uses explicit loops and ArrayList result collections. I originally had the new operations written as stream expressions, but I changed them because I wanted the steps to be visible during review: validate the criteria, scan the snapshot, decide whether a record matches, copy the match, sort the matches, and return an unmodifiable list. That made the algorithm easier for me to explain and easier for another person to trace.

Exact Location of the Completed Enhancements

The full path and line information is repeated in 04_EVIDENCE/ENHANCEMENT_MAP.txt. The table below lists the shortest path to each main change.

| Enhancement | Exact location and evidence |
| --- | --- |

| Contact algorithms | ContactService.java: searchContacts(), lines 61-73, and searchByLastName(), lines 79-91. Evidence: ContactServiceTest.java and AlgorithmEnhancementSmokeTest.java. |

| Task algorithm | TaskService.java: searchTasks(), lines 52-64. Evidence: TaskServiceTest.java and AlgorithmEnhancementSmokeTest.java. |

| Appointment algorithm | AppointmentService.java: findAppointmentsBetween(), lines 54-70. Evidence: AppointmentServiceTest.java and AlgorithmEnhancementSmokeTest.java. |

| Shared rules and safe results | RecordComparators.java, lines 14-25; ValidationRules.java methods requiredSearchTerm(), searchableText(), and inclusiveDateRange(). Each search copies matches and returns List.copyOf(matches). |

| Regression, analysis, and full diff | Appointment.java copy constructor, lines 41-46, and copy(), lines 70-72. Complexity: docs/ALGORITHM_ANALYSIS.md. Exact line-by-line changes: 04_EVIDENCE/MILESTONE3_CHANGESET.diff. |

These changes make the artifact more useful without removing the original ID operations. A caller can search for contacts across five fields, search for tasks across the name and description, or request appointments in an inclusive date range. Results are no longer dependent on HashMap iteration order. Contacts sort by last name, first name, and ID; tasks sort by name and ID; appointments sort by date and ID. The last ID comparison is important because equal names and equal dates are valid and still need a predictable order.

The testing is part of the enhancement rather than an attachment added afterward. The JUnit source suite contains 40 test methods across the model and service classes. The separate dependency-free smoke test performs 25 checks for matches, case differences, empty results, no matches, boundary dates, tiebreakers, invalid criteria, defensive copies, and unmodifiable lists. I included that second test path, so the main algorithm behavior can still be verified on a computer that has Java but does not have Maven available.

### 3. Course Outcome Progress and Updates

The primary outcome I planned to address was the third program outcome: designing and evaluating a computing solution with algorithmic principles and appropriate data structures while managing trade-offs. I met that plan. The artifact keeps a HashMap for average O(1) exact-ID operations, uses O(n) scans when the query is not based on the key, sorts m matching records in O(m log m), and uses O(m) space for safe result copies. I also documented why a TreeMap, several secondary indexes, or a database would change the trade-offs.

The artifact also supports the fourth outcome through Java 17, collections, comparators, Maven configuration, JUnit 5 source tests, and repeatable command-line verification. It supports the second outcome through the narrative, JavaDoc, README, change log, exact enhancement map, traceability file, and algorithm analysis. The security outcome is supported by rejecting invalid criteria, not returning repository objects, making result lists unmodifiable, and correcting the appointment-copy defect that testing exposed.

I do not claim that this individual project fully demonstrates the collaborative-environment outcome because I completed it independently. It does make review easier for another person through clear public methods, predictable output, a baseline-to-final diff, and exact evidence locations. My update from the Module One plan is mostly an increase in precision. Instead of writing that all sorted searches are O(n log n), I now separate the O(n) scan from the O(m log m) sort, where m is the number of matches. The appointment-copy correction was not in the original plan, but it became necessary when the date-range tests exposed the problem.

### 4. Reflection on the Enhancement Process

The first thing I learned was that an algorithms enhancement did not require replacing the existing data structure. It was more important to identify what the HashMap did well and what it could not do. The map is good for a known ID. It cannot directly answer a partial-name search or a date-range request. Keeping it and adding focused scan-and-sort operations produced a smaller and more defensible change than selecting a more complicated structure only to make the project appear advanced.

I also learned that correct records are not enough if their order changes unpredictably. HashMap iteration order is not a contract. A search could contain every correct match and still produce inconsistent output. The comparators fixed that, but the first field was not sufficient. Two contacts can share a last name, two tasks can share a name, and two appointments can occur at the same time. Adding the ID as the last comparison gave the methods a total and testable order.

Date filtering caused the most important bug I found. I expected the range boundaries to be the difficult part, so I tested dates exactly on the start and end. While doing that, I found that Appointment.copy() rebuilt an appointment through the public constructor. After the scheduled time passed, copying the stored record failed because the constructor correctly rejects a newly created appointment in the past. The record itself was valid; the copy path was wrong. I added a private copy constructor so stored state can be copied without pretending it is a new appointment. The regression test advances a controllable clock and confirms that the record remains readable.

Another decision was how much reuse to add. I kept the comparator definitions together because ordering rules are shared algorithm decisions. I did not create one generic search framework. Contact, task, and appointment matching rules are different, and hiding them behind a large generic abstraction would make this project harder to follow. The small amount of repeated loop structure is easier to trace than a framework that would only be used three times.

The failed first upload also taught me that a correct project is not useful if the reviewer cannot open it or locate the work. For this version, I treated packaging as part of the submission. I removed the nested archive, used short, numbered folder names, placed the READ ME at the ZIP root, added a direct enhancement map, generated a line-by-line diff, and verified the final ZIP before submitting it. That does not change the algorithm, but it changes whether the work can actually be evaluated.

Final Verification

The production source compiled for Java 17 with -Xlint:all and produced zero errors and zero warnings. The dependency-free algorithm test passed 25 checks, and the appointment-copy regression test passed. The final ZIP contains no nested archives, opens directly to the numbered submission structure, and passes an integrity test. The exact commands and results are recorded in 04_EVIDENCE/TEST_AND_VERIFICATION_RESULTS.txt.

---

[Home](./) · [Code Review](code-review.html) · [Software Design](software-design.html) · [Algorithms](algorithms.html) · [Databases](databases.html)
