Overview
========
1. This utility script is designed to manage and organize Snowflake database objects, ensuring that their DDL (Data Definition Language) statements are properly versioned and prepared for check-in to Git. 

2. The script reads an input file listing database objects, retrieves their DDL statements, and saves them in a structured format with versioning. Errors encountered during the process are logged for review.

Flow Overview
=============
1. Input File: object_list.txt
-------------------------------
Purpose: Provides a list of database objects whose DDL statements need to be retrieved and processed.
Format: Each line contains the name of a database object (e.g., tables, views, tasks, procedures, functions, stages).
Example Content:
table1
view1
task1
procedure1
function1
stage1
                        
2. Script Execution
-------------------
Initialization: Reads from object_list.txt and initializes paths for various files:
base_output_dir: Base directory where output files will be stored.
error_file: File where errors will be logged if any objects can’t be processed.
Database Processing: For each database listed in databases:
Create Session: Establish a connection to Snowflake using create_snowflake_session().
Object Type Check: Determine the type of each object in object_list.txt (e.g., table, view) by querying the Snowflake metadata.

Generate DDLs: Retrieve and format DDL statements for:
--------------
    Tables
    Views
    Tasks
    Procedures
    Functions
    Stages

Create Directories: Generate directories for each object type within base_output_dir.

3. Output Files
---------------
Object-Specific DDL Files:
Path: Subdirectories created within base_output_dir for each object type (e.g., tables, views, tasks).
Filename Format: V{version}__{object_name}.sql
Versioning: Includes a version number in the filename (e.g., V4.1.1__table1.sql).
Content: Contains the DDL statement for the corresponding object.
4. Error File: object_missed.txt
Purpose: Logs errors encountered during the processing of objects.
Content: Includes entries for objects that could not be found or processed, with a brief explanation of the error.

Example Content:
----------------
Skipping .. tableXYZ - It may be an unknown entry / it may be a comment / object does not exist.
                        
5. Versioning and Indexing
---------------------------
Version Information: The script manages a version counter to track and format DDL files correctly.
Indexing: Each object type (e.g., table, view) is assigned a unique index in the format {major_version}.{minor_version}.{counter}.

6. Summary
----------
After processing all objects, the script writes output files in their respective directories and logs any errors in object_missed.txt. The generated SQL files and error logs aid in reviewing, documenting, or migrating database objects.

Detailed Flow
=============
Read Input: Open object_list.txt. Extract and de-duplicate object names.

Process Each Database:

Create a Snowflake session.

For each object name:
  Check its existence and type.
  Retrieve and format the DDL.
  Save the DDL to the appropriate file.
  Handle errors and log them if necessary.

  Write Output: Save formatted DDLs in directories organized by object type. Include versioning in filenames.

  Handle Errors: Log any issues in object_missed.txt.
