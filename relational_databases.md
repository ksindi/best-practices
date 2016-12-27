_Adapted from Silicon Age Relational Database Conventions v1.1_  

-   [Tables](#tables)
-   [Fields](#fields)
-   [Identities](#identities)
-   [Integrity](#integrity)
-   [Other Database Objects](#other-database-objects)
-   [Tracking](#tracking)
-   [Normalization](#normalization)
-   [Design Best Practices](#design-best-practices)
-   [Appendix A: Common Abbreviations/Acronyms](#appendix-a-common-abbreviationsacronyms)
-   [Appendix R: Reserved Words](#appendix-r-reserved-words)
-   [Appendix T: Sample Triggers](#appendix-t-sample-triggers)

## Tables

-   Table names will be singular nouns.
-   The initial character of each table name and the first character of
    each *significant* word in a table name should be uppercase. All
    other characters should be lowercase.
-   Separate words in table names will be separated by underscores.
-   Table names will be restricted to 30 characters.
-   Words reserved by common databases may not be used as table names.
-   Words may not be abbreviated unless the abbreviation is clear,
    documented, and is used consistently within the data model.
-   If a table Y provides the principal classification of the rows of
    another table X and, in addition, is both slowly-changing and
    single-level, it should be named "X\_Type".
-   Table names may not be generic collective words like "Group" or "Set".
-   Self-referencing, hierarchical tables should have the morpheme
    "\_Tree" appended to their names. The "\_Tree" morpheme should *not*
    be included in the name of the primary key because the values
    contained therein do not have a tree structure. Likewise,
    association tables joining a self-referencing table should not
    include the "\_Tree" in their name.
-   Tables implementing many-to-many relationships should be given names
    like "X\_to\_Y" where X and Y are the names of the other tables. The
    tables represented by X and Y should be included in the name in
    alphabetical order, i.e. Exercise_to_Member, not
    Member\_to\_Exercise. The "to" is not capitalized in the name of a
    table. Whenever possible, eschew table names of the form "X\_to\_Y",
    replacing them with precise names for the relationship.
-   Table names of the form X\_to\_Y\_to\_Z are not allowed; at least
    one of the subrelationships (X\_to\_Y or X\_to\_Z) should be
    renamed.

    **Table naming examples:**  

    | Example       | Rating | Reason                                             |
    |---------------|--------|----------------------------------------------------|
    | Level         | Bad    | "Level" is a reserved word in SQL Server           |
    | dexter        | Bad    | First letters should be capitalized                |
    | Zip\_Codes    | Bad    | Plural and "ZIP" should be capitalized             |
    | Receipt       | Good   |                                                    |
    | StarWars      | Bad    | Words should be separated by underscores           |
    | Criteria      | Bad    | "Criteria" is plural; should be "Criterion"        |
    | Thrashed      | Bad    | Not a noun                                         |
    | Turkey\_Pie   | Good   |                                                    |
    | Age\_of\_Emps | Bad    | "of" should be capitalized and "Emps" is not clear |


## Fields

-   Field names will be singular nouns or verb phrases.
-   Field names will be entirely lowercase.
-   Separate words in field names will be divided by underscores.
-   Field names will be restricted to 30 characters.
-   Surrogate keys will be named "id". Foreign keys will have associated
    table name prepended.
-   Words reserved by common databases may not be used as field names.
-   Words may not be abbreviated unless the abbreviation is clear and is
    used consistently within the data model (see
    [Appendix A](#appendix-a-common-abbreviationsacronyms)).
-   A field which contains numbers indicated the order in which its
    elements should be displayed or processed should be called "seq".
    If multiple sequences are necessary, a role descriptor should be
    prepended to "seq" as in "display\_seq".
-   Most tables should contain textual fields called "description" and
    "note". The desc field should contain information that could be
    displayed to a user or client detailing the row. The note field
    should contain internal information of interest to data entry
    operators or system administrators.
-   The field name "name" is suggested for names that will be meaningful
    for frequent external users of the system ("Serial Port 25M").
    "long\_name" is suggested for names that beginners will need
    ("Serial Port, RS232C, 25-pin, male"). "short\_name" is suggested
    for names that can be used for experts ("RS232C-25m") in order to
    save room. Not all of these three fields need always be present,
    though if long\_name or short\_name is present, name should also be.
-   Tables with data that will need to be statically bound to by
    developers should contain a "code" column. This column should
    contain only capital letters, underscores and no whitespace
    characters. This column should be uniquely indexed. Consider, for
    example, the following table:
    ```sql
    CREATE TABLE Department (
        id INT PRIMARY KEY,
        code VARCHAR(50) NOT NULL UNIQUE CHECK((UPPER(code) = code) AND (INSTR(code, ' ') = 0)),
        name VARCHAR(100) NOT NULL UNIQUE
    );
    ```

    Typical entries in the name field would include "Human
    Resources" and "Engineering". Related code entries would include
    "HUMAN\_RESOURCES" and "ENGINEERING".

## Identities

-   Surrogate keys will have an integral data type whose size is
    approximately 32 bits.
-   Identities will start at 1000 and increment by one (except for ids
    described herein).
-   Surrogate keys will always have the following eight special
    one-digit codes used to encode various common types of missing data:

    | ID | Name           | Meaning (Example: Person.hat\_id)     |
    |----|----------------|---------------------------------------|
    | 1  | None           | has no hat (and we know it)           |
    | 2  | Not Applicable | logically inconsistent to have hat (Person.is_alive = 0) |
    | 3  | Unspecified    | may be associated but unknown (new inserts) |
    | 4  | Unknown        | unknown after efforts to get state    |
    | 5  | Custom         | can't be properly associated with Hat |
    | 6  | Structural or Internal | for referential integrity (Hat.color\_id for Hat.hat\_id between 1 and 8) |
    | 7  | Error          | error occurred in generation of id    |
    | 8  | Inherit        | field obtained from other part of data model |

-   Some tables may require additional special codes, most frequently
    for different types of inheritance. They should be given 2- or
    3-digit codes as appropriate.

## Integrity

-   All tables will have primary keys.
-   All logical foreign key relationships will be created; if
    performance considerations prevent their use, they should be
    disabled, not deleted.
-   Nearly every table should have a surrogate key which forms its
    (entire) primary key.
-   NULLs should appear only when a field is inapplicable to the row
    because of polymorphism. In some databases NULL may also be used
    to indicate the empty string if it is syntactically identical to
    ''. In addition, some databases treat NULL textual fields far more
    (space-)efficiently than empty textual fields; in these cases the
    use of NULL may be preferable. In all other cases, NULL values
    should not appear and fields should be declared "NOT NULL".
-   The status of unavailable data should be indicated using the following
    convention:

    -   For foreign keys to surrogate keys (that is, id fields), the
        standard single-digit codes should be used along with any
        specific two- or three-digit codes defined only for that table
        (see [Identities](#identities)).
    -   For character data, the following characters, codes, or strings
        should be used to indicate the status of the field. These obviously
        encode special cases which should be checked by applications in
        order to perform the correct action.

        | Meaning     | 1-2 Chars | 3-14 Chars | 15+ Chars        |
        |-------------|-----------|------------|------------------|
        | None        | ‘-’       | ‘—’        | ‘None’           |
        | Not App.    | ‘/’       | ‘///’      | ‘Not Applicable’ |
        | Unspecified | ‘?’       | ‘???’      | ‘Unspecified’    |
        | Unknown     | ‘&’       | ‘&&&’      | ‘Unknown’        |
        | Custom      | ‘~’       | ‘~~~’      | ‘Custom’         |
        | Structural  | ‘\#’      | ‘\#\#\#’   | ‘Structural’     |
        | Error       | ‘!’       | ‘!!!’      | ‘Error’          |
        | Inherit     | ‘^’       | ‘^^^’      | ‘Inherit’        |

        *An attempt has been made to choose codes that will not interfere
        with real data. In the event that they do, choose appropriate codes
        and document the deviation from the standard.*

   -   Where appropriate, character fields should DEFAULT to the appropriate
       value which, in most cases, will be '?', '???', or 'Unspecified'.
   -   All foreign keys should be NOT NULL.

## Other Database Objects

-   Views should follow the naming conventions for tables except that
    the morpheme "\_v" should be appended to the name (without altering
    field names in any way).
-   Indexes
    -   Primary Key indexes should be named XPK\_[Table]. The X
        indicates an index and PK signifies a primary key.
    -   Foreign Key indexes should be named XFK[sequence the index was
        created]\_[Table].
    -   Unique Keys should be named XAK[sequence the index was
        created]\_[Table]. The AK stands for alternate key.
    -   Other indexes should be name X[sequence the index was
        created]\_[Table]
-   Triggers should be named [Table]\_tr\_[Action][Timing], where
    [Action] is some combination of "i", "u", "d", and "s" indicating
    insert, update, delete, and select respectively. If the database
    allows the specification of BEFORE and AFTER triggers (as Oracle
    does), the Timing variable should be replaced with "b" or "a".
-   Named defaults should have "\_df" appended to their names.
-   Denormalized tables that are automatically maintained by triggers,
    background processes, or other means should have "\_d" appended to
    their names.
-   User defined types should have "\_t" appended to their names.
-   Stored procedures should have "\_sp" appended to their names.

## Tracking

-   All tables should have fields named "inserted" and "updated" that
    contain the times at which each row was inserted and last updated.
-   Whenever performance considerations allow, the inserted and updated
    fields should be maintained by triggers. See
    [Appendix T](#appendix-t-sample-triggers) for examples.
-   Columns used to indicate the first and last dates (or other ranges)
    of validity for a row should be named "start\_date" and
    "end\_date" respectively. "9999-01-01" should be used as the
    general code for a row that is currently valid and has no specified
    date of invalidation.

## Normalization

-   All data models should be in
    [Boyce-Codd normal form](https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form);
    BCNF is slightly stronger than
    [Third normal form](https://en.wikipedia.org/wiki/Third_normal_form)
    and, in most cases, is equivalent to it.
-   Denormalization from this standard should be documented and
    justified.

## Design Best Practices

-   Measurements
    -   When storing measurement information such as mass, length,
        volume, time, etc. choose one unit of measurement that will be
        used throughout the database and separately store the display
        method preferred by the user and a list of conversions from the
        base unit.
    -   Recommended standards of measurement are kilograms, meters,
        cubic meters, and the number of milliseconds since January 1,
        1970 for time/date fields (epoch time).
-   Self-Referencing Tables
    -   The column that references the primary-key of the table will
        generally be called parent\_[name of the id column]. For
        example, if the table was named Turkey\_Pie, the primary key
        would be turkey\_pie\_id and the column referencing
        turkey\_pie\_id would be parent\_turkey\_pie\_id.
    -   The parent id of top-level entries should have their parent\_id
        set to the primary key.

## Appendix A: Common Abbreviations/Acronyms
Document the use of any abbreviation when used for naming
database objects no matter how common the usage.

| Abbreviation/Acronym | Expansion    |
|----------------------|--------------|
| cls                  | class        |
| grp                  | group        |
| param                | parameter    |
| qty                  | quantity     |
| seq                  | sequence     |
| std                  | standard     |
| str                  | string       |
| sys                  | system       |
| val                  | value        |
         
Any standard measurement reductions such as "mi" for mile and
"km" for kilometer.

## Appendix R: Reserved Words
Not Available

## Appendix T: Sample Triggers
Not Available
