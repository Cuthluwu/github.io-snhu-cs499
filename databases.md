---
layout: default
title: Databases
---

[Home](./) · [Code Review](code-review.html) · [Software Design](software-design.html) · [Algorithms](algorithms.html) · [Databases](databases.html)

# Databases

This CS 360 Android Weight Tracker enhancement focuses on relational SQLite structure, account ownership, safer credential storage, migration, indexed queries, and reliable CRUD behavior.

## Original and enhanced artifacts

- [Download the unmodified original source](artifacts/databases-original-source.zip)
- [Download the enhanced source](artifacts/databases-enhanced-source.zip)
- [Download the complete graded milestone package and supporting evidence](artifacts/databases-complete-milestone-submission.zip)
- [Download the enhancement narrative (Word)](documents/databases-enhancement-narrative.docx)

## Enhancement narrative

CS 499 Milestone Four

Enhancement Three: Databases

CS 360 Android Weight Tracker
Database Enhancement Narrative

Madison Parker
Southern New Hampshire University
CS 499: Computer Science Capstone
August 1, 2026

## Submission Organization and Capstone Continuity

This milestone continues with the database plan I wrote in Module One. I selected the CS 360 Weight Tracker at the beginning of the capstone and planned to separate users from weight entries, add numeric identifiers, store weight as a number, use one date format, improve password storage, connect records to the correct account, and add progress queries. I used the exact Week One artifact as my starting point and completed the enhancement in a separate copy, so the original and final versions can be compared directly.

I also changed how I prepared the submission after the earlier ZIP problem. The technical ZIP opens directly to a read-first file and four numbered folders. The first folder contains the untouched original project. The second contains the complete enhanced Android Studio project. The remaining folders contain the documentation and evidence needed to find the work without searching through every file. The Word narrative is submitted separately, which follows the instructions under What to Submit.

I used the same order throughout this narrative: original condition, decision, completed change, test evidence, and reflection. That structure is deliberate. My earlier feedback focused on professionalism and missing detail, so I wanted each statement to be checkable in the code instead of asking the reader to accept a general description.

## 1. Artifact Description

The artifact is the Android Weight Tracker application I created in CS 360: Mobile Architecture and Programming in June 2026. It was built in Android Studio with Java, XML layouts, and a local SQLite database. The application contains a login screen, an account-registration screen, a weight-management dashboard, and an SMS settings screen. The original dashboard allowed a user to enter a name, weight, and date, then insert, update, delete, or display records. The project also included Gradle files, Android resources, a manifest, screenshots, and a written CS 360 portfolio reflection.

The original database was small. The users table used the username as its primary key and stored a password in a plain text column. The Userdetails table used a name as its primary key and stored weight and date as text. The database helper provided methods for registration, login checks, insert, update, delete, and a SELECT * query that returned every row in Userdetails. This was enough to demonstrate basic persistence and CRUD behavior in the original course, but it did not create a reliable relationship between an account and that account's weight history.

I selected the Weight Tracker because the database is not an optional feature added around the edges of the application. The main purpose of the app depends on storing information correctly over time. A user needs to create an account, sign in, add several measurements, view them in order, update the correct record, remove the correct record, and see progress that belongs only to that account. That made this artifact appropriate for the database category because improvements to the schema and queries change whether the application can fulfill its basic purpose.

## 2. Why I Selected the Artifact and What the Original Version Needed

The original project was useful as an ePortfolio artifact because it had a recognizable user workflow and a working database layer, but it also had limitations that could be shown clearly in a before-and-after comparison. I did not want to choose an artifact where the database was only a list of sample rows or where the enhancement would be mostly cosmetic. The Weight Tracker gave me a reason to address schema design, persistence, authentication, authorization, migration, constraints, indexing, reporting, and testing in one contained application.

The first issue was password storage. The original insertData() method placed the supplied password directly into SQLite, and the login method compared the supplied password against that stored text. A person who obtained the database file could read every password. The enhanced version needed to store a verifier instead of a reversible or directly readable value. It also needed to store enough metadata to verify the password later and to support a future change in the work factor or algorithm.

The second issue was recording ownership. The original Userdetails table did not contain a user_id or any foreign key. A successful login opened the dashboard, but the application did not pass a stable account identifier to that screen. The dashboard query returned every weight row. Update and delete operations used only the name field, so the database itself had no rule connecting an action to the signed-in user. This was not only a privacy problem. It was also a structural problem because the application could not answer a basic question: which account owns this measurement?

The third issue was the meaning of the primary key. The original name field was the primary key in Userdetails. A primary key should identify one row. In a weight tracker, one row represents one measurement, not one person. If the same name entered another measurement, the original design would treat it as a duplicate instead of a second point in the history. The enhanced version therefore needed a numeric entry_id for each measurement and a separate user_id to connect many entries to one account.

The fourth issue was data typing and validation. Weight and date were stored as text. A text column can store values such as 138.5, unknown, or an empty string without distinguishing them. Different date formats also do not sort consistently. The application could validate inputs in Java, but the database should also reject impossible states when practical. The enhanced schema needed REAL weight values, an ISO YYYY-MM-DD date, a range check, required fields, and a controlled note length.

The fifth issue was database lifecycle behavior. The original onUpgrade() dropped both tables. That approach is simple during an early prototype, but it destroys the information that the application exists to preserve. Android's SQLiteOpenHelper is specifically responsible for creation and version management, and its upgrade callback runs within a transaction so a failed upgrade can roll back rather than leave a partly changed database (Android Developers, n.d.). For the capstone enhancement, I needed to replace the destructive upgrade with an explicit version-one to version-two migration.

The final issue was evidence. The original unit and instrumentation test files were template tests. They did not verify the database schema, password storage, user isolation, constraints, ordered history, progress calculations, foreign keys, cascade behavior, or migration. Since the capstone asks me to explain what I improved, I needed tests and verification that could support those statements.

## 2.1 Module One Plan Compared With the Completed Work

| Module One plan | Completed Milestone Four work | Where it is shown |
| --- | --- | --- |

| Separate users from weight history | Created users and weight_entries with a one-to-many foreign-key relationship. | DatabaseContract.java |

| Use numeric identifiers | Added user_id and entry_id as integer primary keys. | DatabaseContract.java |

| Use numeric weight and consistent dates | Stored weight with REAL affinity and validated ISO dates. | DatabaseContract.java and InputValidator.java |

| Improve account security | Replaced plaintext passwords with salted PBKDF2 verifiers and passed a stable user session to the dashboard. | PasswordHasher.java, UserRepository.java, MainActivity.java |

| Restrict records to the correct user | Added user_id to every insert, read, update, delete, and progress query. | WeightEntryRepository.java |

| Preserve data during upgrades | Replaced table deletion with a version-one to version-two migration and audit path. | DBHelper.java and LegacyDataNormalizer.java |

| Add progress reporting | Added ordered history, latest entry, and first-to-latest summary queries. | WeightEntryRepository.java |

| Verify the enhancement | Added unit tests, Android database tests, 46 SQLite checks, and 30 Java checks. | app/src/test, app/src/androidTest, and 04_INSTRUCTOR_EVIDENCE |

## 3. Completed Database Enhancement

## 3.1 Preservation of the Original and Creation of a Separate Enhanced Project

The technical ZIP includes the exact original project under 01_ORIGINAL_ARTIFACT_UNMODIFIED. I copied the full Android Studio project, mobile screenshots, and original reflection from the Week One package. I then calculated SHA-256 hashes and compared every copied file to the extracted source. The evidence report confirms that all 78 original files were present and unchanged. I made the Milestone Four changes in a separate copy under 02_ENHANCED_DATABASE_ARTIFACT/Project2.

This separation matters because it prevents the enhanced version from being presented as though it were the original assignment. It also lets the instructor compare the real starting point to the completed work. The evidence folder contains a recursive line-by-line diff and an enhancement map with exact file paths, methods, and line ranges.

## 3.2 Normalized Version-Two Schema

The enhanced schema is defined once in DatabaseContract.java. I replaced the two unrelated text-based tables with a one-to-many relationship between users and weight_entries. I also added a small migration_issues table for legacy rows that cannot be converted safely. The main relationship is one user to many measurements.

The users table now has an integer user_id, a case-insensitive unique username, a password hash, salt, algorithm name, iteration count, and creation timestamp. The weight_entries table has an integer entry_id, a required user_id, a REAL weight, an ISO entry date, an optional note with a controlled maximum length, and creation and update timestamps. The foreign key uses ON DELETE CASCADE, so removing an account does not leave orphaned measurements.

The schema also uses NOT NULL, UNIQUE, and CHECK constraints. Java validation is useful because it provides understandable messages before a database operation, but it is not the only line of defense. The database rejects zero or negative weights, values above 1500, impossible ISO dates, notes longer than 250 characters, missing users, and invalid password iteration values. This keeps the database from accepting an invalid state because of a future programming mistake or a different code path.

CREATE TABLE weight_entries (
    entry_id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    weight REAL NOT NULL CHECK(weight > 0 AND weight <= 1500),
    entry_date TEXT NOT NULL
        CHECK(length(entry_date) = 10 AND date(entry_date) = entry_date),
    note TEXT NOT NULL DEFAULT '' CHECK(length(note) <= 250),
    created_at TEXT NOT NULL,
    updated_at TEXT NOT NULL,
    FOREIGN KEY(user_id) REFERENCES users(user_id) ON DELETE CASCADE
);

I enabled foreign-key enforcement in DBHelper.onConfigure() instead of assuming that writing a FOREIGN KEY clause was enough. SQLite's documentation explains that foreign-key enforcement must be enabled for each database connection, and that careful applications should not depend on a default setting (SQLite, n.d.-a). This distinction was important because a declared relationship that is not enforced would not prevent orphaned rows.

## 3.3 Safer Credential Storage and Authentication

The enhanced PasswordHasher uses PBKDF2-HMAC-SHA-256, a unique 16-byte random salt, a 256-bit derived value, and 600,000 iterations. I selected PBKDF2 because it is available through the standard Java cryptography APIs used by the Android project and does not require an additional library. OWASP identifies Argon2id as its general first choice, but it also gives a work factor of 600,000 or more for PBKDF2-HMAC-SHA-256 when PBKDF2 is used (OWASP Foundation, n.d.). I used that value instead of an unexplained small number.

The database stores the Base64 hash, Base64 salt, algorithm name, and iteration count. It does not store the supplied password. The algorithm and work factor are stored with the verifier because NIST guidance recommends keeping the hashing scheme and cost information so those values can be changed over time (National Institute of Standards and Technology [NIST], n.d.). Each account receives a new random salt, so two accounts using the same password do not receive the same stored hash.

The UserRepository.authenticate() method retrieves one account by username, derives a verifier from the supplied password using the stored parameters, and uses MessageDigest.isEqual for the comparison. Successful authentication returns a UserSession containing the numeric ID and username. The login screen passes those values to the dashboard. Password character arrays are overwritten after use in the login and registration activities.

I am not claiming that the classroom application is a complete production identity platform. It does not include breach-password screening, account recovery, server-side rate limiting, or a hardware-protected pepper. I am claiming the narrower improvement that the app no longer stores or directly queries plaintext passwords and that the stored verifier includes a unique salt and explicit work-factor metadata.

## 3.4 Authentication and Ownership-Scoped CRUD

The enhancement separates authentication from authorization. Authentication confirms which account supplied the correct password. Authorization determines whether that account is allowed to read or change a specific row. In the original app, opening the dashboard was treated as enough, but the database methods still operated without account ownership.

The enhanced dashboard refuses to continue if the login activity did not provide a positive user_id and a username. Every weight operation then includes that authenticated ID. Insert places the ID into the new row. Read filters by user_id. Update and delete include both entry_id and user_id in the SQL condition. A user who knows another entry's numeric ID still receives a zero-row result because the ownership condition does not match.

UPDATE weight_entries
SET weight = ?, entry_date = ?, note = ?, updated_at = ?
WHERE entry_id = ? AND user_id = ?;

DELETE FROM weight_entries
WHERE entry_id = ? AND user_id = ?;

The registration screen also no longer contains a route directly to the dashboard. Its Sign In button returns to the login activity. After a new account is created, the user is sent to the login screen with the username prefilled instead of being given a dashboard session without authentication.

The dashboard was adjusted to match the new data model. The original name field was replaced by an entry ID field used only for update and delete. The signed-in username is displayed separately. Weight, date, and note describe one measurement. Saving a new row returns its entry ID to the screen. The View History action shows only the current user's records, and Logout returns to the login screen.

## 3.5 Parameterized Access and Repository Boundaries

The enhanced project separates database responsibilities into DBHelper, UserRepository, and WeightEntryRepository. DBHelper owns schema creation and migration. UserRepository owns registration, authentication, and account deletion. WeightEntryRepository owns weight CRUD and reporting. The activities call these repositories instead of assembling SQL themselves.

User-supplied values are passed with ContentValues, query selection arguments, or rawQuery argument arrays. I did not concatenate a username, password, note, date, or numeric identifier into a SQL statement. Table and column names are constants defined in DatabaseContract. This does not make every possible database attack irrelevant, but it removes the original pattern of treating database access as loosely structured strings spread through the interface code.

Cursors are handled with try-with resources. This replaces the original use of @SuppressLint("Recycle") and makes closure part of the method structure. Repository methods also return immutable model objects and unmodifiable history lists so a caller cannot change a returned collection and imply that the database was changed.

I also reviewed the enhanced manifest rather than copying every original permission forward automatically. The original project requested SEND_SMS, but the settings screen only records a switch state and does not send a message. I removed that unused sensitive permission from the enhanced manifest. The settings placeholder remains visible because it belongs to the original application, but the app no longer asks for access it does not use.

## 3.6 Ordered History, Latest Record, and Progress Reporting

The Module One plan called for more user value than a raw list of rows. The enhanced repository now provides ordered history, latest entry, and a progress summary. The history query filters by user_id, orders by entry_date DESC, and uses entry_id DESC as a final tie-breaker. The tie-breaker matters because two entries may use the same date. Without it, both rows would be valid but their order would not be fully defined.

The latest-entry query uses the same order with LIMIT 1. The progress query returns the number of entries, earliest weight and date, and latest weight and date for one account. The application calculates latest minus first after retrieving those endpoints. A user with no records receives a zero-count summary instead of an exception.

I added the compound index idx_weight_entries_user_date(user_id, entry_date DESC, entry_id DESC). The order of the columns follows the actual access pattern: first restrict the rows to one user, then return that user's records in date and entry-ID order. SQLite's query-planning documentation explains that the planner needs useful indexes supplied by the developer (SQLite, n.d.-b). I did not index every column. The app does not perform global searches by note or weight, so those indexes would add write and storage cost without supporting a current query. The verification harness uses EXPLAIN QUERY PLAN and confirms that SQLite selects the history index.

## 3.7 Version-One to Version-Two Migration

The migration was the most delicate database change because the original schema did not contain enough information to produce a perfect ownership mapping. The enhanced DBHelper raises the database version from 1 to 2. During upgrade, it renames the original tables, creates the new schema, migrates accounts and readable entries, and drops the renamed tables only after the conversion completes. Android documents that onUpgrade() executes inside a transaction and rolls back if an exception is thrown (Android Developers, n.d.). I rely on that transaction instead of catching structural failures and continuing with half of the schema.

Legacy account passwords are available in plaintext only because that is how the original project stored them. During migration, each old password is immediately converted to a salted PBKDF2 verifier before it enters the new user's table. Existing passwords are preserved even if an old password would not meet the new registration minimum. This distinction matters: rejecting an old account during migration would cause data loss, while new registrations can follow the stronger input rule.

The original Userdetails.name value sometimes provides a possible ownership clue, but it is not a foreign key and may not equal a username. I used a conservative rule. If the trimmed name matches an original username without case sensitivity, the row is linked to that account. If it does not match, the valid row is preserved under a separate legacy_import account. The migration does not guess which real user owns an ambiguous row. This prevents an incorrect migration from exposing one person's record to another account.

The migration supports the original long date format, ISO dates, and common slash dates. It converts them to ISO. It parses weight as a numeric value and checks the valid range. If either value cannot be normalized, the row is written to migration_issues with the raw values and a reason. I chose an audit record instead of silently discarding data or inserting invalid text into the new table. This is a concrete example of a design decision changing after I examined the original artifact. The Module One plan mentioned migration and stronger data types, but it did not yet account for the ownership information that the original schema never stored.

## 3.8 Verification and Functional Evidence

The enhanced Android project includes JUnit tests for password hashing and input validation and an Android instrumentation test for the database. The instrumentation test creates accounts, confirms that the stored password value is not plaintext, verifies successful and failed authentication, checks cross-user update and delete attempts, verifies history and progress behavior, confirms cascade deletion, creates the exact original version-one tables, and verifies that SQLiteOpenHelper migrates them to version two.

I performed the final file review and standalone checks from a Chromebook that did not have the Android SDK available. I do not claim that I assembled an APK in that environment. The complete Gradle project, Gradle wrapper, and Android instrumentation tests remain in the enhanced folder, so the project can be opened and run in Android Studio.

The verification evidence is separated by what was actually executed. The pure-Java password, validation, and legacy-conversion classes compiled with javac -Xlint:all and passed 30 checks. A standalone SQLite test created the version-two schema and passed 46 checks covering ownership, CRUD operations, constraints, index use, reporting, and migration. I also parsed the Android XML resources, checked every Java resource-ID reference, and checked the submitted Java syntax. Finally, I compared all 78 original files with the Week One source by SHA-256. These checks do not replace an Android Studio build, but they provide direct evidence for the database behavior and keep the limitation clear.

## 4. Course Outcome Progress and Updates

The main program outcome for this milestone is Outcome 4: using well-founded computing techniques, skills, and tools to implement a solution that delivers value. The enhanced artifact uses Android Studio project structure, Java, SQLite, relational schema design, constraints, foreign keys, repositories, migration, PBKDF2, parameter binding, indexes, reporting queries, unit tests, instrumentation tests, and repeatable verification. These techniques are connected to the application's goal rather than added as isolated examples. The user can store several measurements, retrieve only their own history, and view useful progress information.

Outcome 5, the security mindset, is also strongly represented. I began with specific flaws in the original artifact: plaintext passwords, no record ownership, a registration-to-dashboard bypass, destructive upgrades, unrestricted history, and missing structural constraints. The enhancement reduces those flaws through salted password hashing, stable account IDs, ownership conditions inside SQL, foreign-key enforcement, database constraints, controlled migration, disabled application backup, removal of the unused SMS permission, and explicit failure behavior. I also state what the artifact does not solve instead of calling it fully secure.

Outcome 2, professional communication, is supported through the Word narrative, read-first instructions, exact enhancement map, schema and data dictionary, migration explanation, security analysis, query and index analysis, test plan, full diff, rubric crosswalk, references, and recorded verification. The organization is intended for an instructor or developer who needs to locate the evidence without relying on my verbal explanation.

Outcome 3 is supported through design trade-offs. I evaluated whether to keep SQLiteOpenHelper, how much to normalize, which columns to index, how to order records deterministically, where to enforce validation, and how to preserve ambiguous legacy data without guessing ownership. The milestone primary category is databases, so I do not present those choices as a second algorithms milestone. I include them because database design still requires evaluating costs and consequences.

Outcome 1 receives partial support. I completed the artifact independently, so I do not claim that it demonstrates a full collaborative environment. It does provide artifacts another developer can review centralized contracts, repository boundaries, consistent names, source-to-evidence traceability, an unchanged baseline, and explicit limitations. Those choices make technical discussion and later collaboration easier.

The outcome plan from Module One remained appropriate, but the completed work is more specific. The planned normalized schema, numeric identifiers, numeric weight values, consistent dates, parameterized operations, password hashing, user-linked history, and progress queries are implemented. I added migration auditing, deterministic ordering, ownership-scoped update and delete, stored hashing metadata, cascade behavior, and exact verification because the original code review showed that those details were necessary to support the broader plan.

## 5. Reflection on the Enhancement Process

The first major lesson was that persistence and database design are not the same thing. In the original project, the values remained after the app closed, so I treated the database successfully. During this enhancement, I had to examine what each row represented, how rows related to one another, what the primary key actually identified, and what invalid states the database allowed. The original Userdetails table stored data, but it did not model a history owned by an account. Changing that model was more important than adding more buttons.

The second lesson was the difference between authentication and authorization. I initially thought of login as the security boundary. A successful login opened the dashboard, and that seemed like the important step. The database methods showed why that was incomplete. If the history query returns all rows, or an update uses only an entry name, the database still does not know whether the signed-in user owns the record. Passing a user ID to the screen is not enough by itself. The ID has to appear in the query condition. This is why the enhanced update and delete statements require both the entry ID and the authenticated user ID.

The third lesson was that the database should defend its own rules. Interface validation can prevent many mistakes, but interfaces change, and code paths multiply. A future caller could bypass one screen or forget one validation call. The CHECK, NOT NULL, UNIQUE, and foreign-key constraints give the data model a second boundary. At the same time, I learned not to rely only on constraints because the user still needs a clear explanation. The Java validator produces useful messages, and SQLite remains the final protection against invalid state.

Date handling required more precision than I expected. The original app displayed June 22, 2026, while database ordering works more reliably with 2026-06-22. I used LocalDate for new entries and explicitly supported a small set of legacy formats during migration. I did not accept any string that happened to contain numbers. The database checks both the length and SQLite's normalized date result, so an impossible date does not pass only because it has ten characters.

The migration exposed the most difficult limitation. The old weight table did not store an account identifier. I could not reconstruct ownership with certainty because that information did not exist. I considered assigning every old row to the first user or matching records by position, but both approaches would invent data. I used exact name-to-username matching only when it was available and sent unmatched records to a separate import account. That solution is not as convenient as a perfect migration, but it is more honest and safer than giving the record to the wrong person.

The password works also require careful wording. I did not encrypt the password, and I should not describe a hash as encryption. A password verifier is intentionally one-way. I also learned that storing only the hash is not enough. The salt, algorithm, and work factor are part of the record needed to verify the password and to support future upgrades. Using a random salt means equal passwords do not create equal stored values. Storing the iteration count prevents the current setting from becoming an invisible assumption in the code.

The index decision helped me connect the database to the actual workflow. I could have added indexes to several columns to make the schema appear advanced, but each index has a cost during inserts and updates. The application's repeated query is account history ordered by date. I therefore created one compound index that begins with user ID and continues with the requested order. The verification harness checks the query plan instead of assuming SQLite will use it.

I also learned to separate preservation from cleanup. The original artifact contains naming inconsistencies, older activity names, and an SMS settings placeholder outside the database milestone. I did not rewrite the entire application or pretend the placeholder was a finished notification feature. I kept the screen so the project still resembles the original, but I removed the unused SMS permission because the code does not send messages. That small change made me examine what the application actually needs instead of carrying every original setting into the enhanced version.

Testing affected the design rather than being added after the code. The user-isolation test forced the update and delete methods to use both IDs. The migration test forced me to preserve original credentials and normalize the actual date format used by the original project. The same-date ordering test required the final entry-ID tiebreaker. The invalid-record migration test led to the migration_issues table, so unreadable data would not disappear silently.

Packaging became part of my reflection because of the earlier submission failure. I now verify the ZIP after it is created, keep the Word document separate, avoid nested archives, include a root read-first file, and map each narrative claim to a file and line range. That work does not change the SQLite result, but it changes whether the artifact can be reviewed. Professional communication includes the state in which the files reach the reviewer.

The largest remaining technical limitation is that the app still performs database work synchronously from the activity. Android documentation warns that opening or upgrading a database can take time and should not be treated as free work on the main thread. Moving repository calls to a background executor or adopting Room with a lifecycle-aware architecture would be a sensible next improvement. I did not add that larger architectural change to this milestone because the planned focus was the database design, data protection, CRUD behavior, migration, and reporting. Other future work would include a better history interface, account recovery, failed-login rate limiting, encrypted sensitive-data storage, and a deliberate SMS reminder implementation rather than a settings placeholder.

## 6. Value of the Artifact in My ePortfolio

The completed artifact gives my ePortfolio a database example that can be inspected at several levels. A nontechnical reviewer can understand the user's problem and see that each account now has a private history and progress summary. A technical reviewer can compare the original and enhanced schemas, inspect the repositories and migration, run the tests, review the query plan, and follow the enhancement map to the exact code.

The artifact also shows growth without hiding the original work. The original CS 360 application demonstrated that I could connect an Android interface to SQLite and implement basic CRUD operations. The CS 499 enhancement demonstrates that I can revisit that code, identify structural and security flaws, preserve existing data, redesign the schema, enforce ownership, choose an index for a real query, test the result, document limitations, and package the work so another person can evaluate it. That progression is the reason I selected the artifact for the final ePortfolio.

## Appendix A: Artifact Evidence Guide

| Evidence | Location in Technical ZIP | Purpose |
| --- | --- | --- |

| Exact original project | 01_ORIGINAL_ARTIFACT_UNMODIFIED/Project2 | Untouched baseline required by What to Submit. |

| Complete enhanced project | 02_ENHANCED_DATABASE_ARTIFACT/Project2 | Android Studio project containing the version-two implementation. |

| Before-and-after summary | 04_INSTRUCTOR_EVIDENCE/WHAT_CHANGED_AT_A_GLANCE.md | Concise comparison of the original problem and completed change. |

| Schema and data dictionary | 03_DATABASE_DOCUMENTATION/DATABASE_SCHEMA_AND_DATA_DICTIONARY.md | Explains every table, key, constraint, and index. |

| Migration analysis | 03_DATABASE_DOCUMENTATION/MIGRATION_AND_DATA_PRESERVATION.md | Explains data preservation and ambiguous legacy ownership. |

| Capstone decision log | 03_DATABASE_DOCUMENTATION/CAPSTONE_PROGRESS_AND_DECISION_LOG.md | Connects the Week One plan, code review, decisions, testing, and packaging. |

| Security analysis | 03_DATABASE_DOCUMENTATION/SECURITY_ANALYSIS.md | Separates hashing, authentication, authorization, and structural controls. |

| Exact enhancement locations | 04_INSTRUCTOR_EVIDENCE/ENHANCEMENT_MAP.md | File, method, and line range for every major claim. |

| Original-to-enhanced diff | 04_INSTRUCTOR_EVIDENCE/ORIGINAL_TO_ENHANCED.diff | Complete code and resource comparison. |

| Database verification | 04_INSTRUCTOR_EVIDENCE/DATABASE_VERIFICATION_RESULTS.txt | 46 executed checks. |

| Java verification | 04_INSTRUCTOR_EVIDENCE/JAVA_SECURITY_VALIDATION_RESULTS.txt | Compilation and 30 standalone checks. |

| Original integrity | 04_INSTRUCTOR_EVIDENCE/ORIGINAL_INTEGRITY_SHA256.txt | SHA-256 hashes of every original file. |

## Appendix B: Verification Summary

The database verification script completed 46 checks covering schema creation, foreign-key enforcement, data types, keys, index creation, hashed password storage, unique salts, authentication, case-insensitive duplicate usernames, user-scoped insert/read/update/delete, deterministic ordering, database constraints, progress reporting, empty history, query-plan index use, cascade deletion, account migration, date normalization, unmatched legacy records, migration auditing, and removal of the renamed tables after successful conversion.

The standalone Java verification compiled the password, validation, and legacy-normalization classes with javac -Xlint:all and completed 30 checks. The Android XML files parsed successfully, all Java resource-ID references resolved, and the submitted Java syntax check completed without warnings. This is not presented as an Android Studio build. The technical package includes the actual Gradle wrapper and Android instrumentation tests for execution in Android Studio.

## References

Android Developers. (n.d.). SQLiteOpenHelper. Retrieved August 1, 2026, from https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper

National Institute of Standards and Technology. (n.d.). Digital identity guidelines: Authentication and authenticator management (NIST SP 800-63B). Retrieved August 1, 2026, from https://pages.nist.gov/800-63-4/sp800-63b.html

OWASP Foundation. (n.d.). Password storage cheat sheet. Retrieved August 1, 2026, from https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html

SQLite. (n.d.-a). SQLite foreign key support. Retrieved August 1, 2026, from https://sqlite.org/foreignkeys.html

SQLite. (n.d.-b). Query planning. Retrieved August 1, 2026, from https://sqlite.org/queryplanner.html

---

[Home](./) · [Code Review](code-review.html) · [Software Design](software-design.html) · [Algorithms](algorithms.html) · [Databases](databases.html)
