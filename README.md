# Working with Manual Data

## Data Used: 
A portion of text containing the early 20th century translation of 17th century Dutch Colonial records from Ulster County, NY.

## Rationale: 
The records in question are presented in a manner that makes automated data ingestion difficult, if not impossible. Further, they are fragmentary, contain potentially incorrect translation that needs remediation, and require careful parsing for useful representation as data.

## Benefits of Working with Manual Data: 
working with the types of data included in this workshop has some benefits over working with pre-structured, pre-sorted, prepared data. These include: 
	• having granular control over the data model and tailoring it to both the data and a your 	specific research questions 
	• a greater appreciation for what is included in, and excluded from, the data as well as the 	representation of it
	•  a slow, deliberative process of reading and thinking about every single line of the data and the ability to adjust the data model over time as needed.

## Drawbacks of Working with Manual Data: 
working with the types of data included in this workshop has some significant drawbacks compared to working with pre-structured, pre-sorted, prepared data. These include: 
	• slower time for ingesting data necessitated by a need for extensive pre-planning
	• extended time required for iterating over potential data models
	• need for subject matter expertise, or access to appropriate subject matter experts, to make informed decisions about data modeling, inclusions, exclusions, categorizations, 	and context
	• extended time to input data into the model when compared with prepared data sets as well as the need for close supervision to handle judgment calls, which are often numerous

### Basic Workflow: 
* Initial scan of the original data to understand the structure of each entry. 
* Explore candidate approaches for modeling the data (for the workshop, the main approach will be Entity Relationships)
	* Determine who your projected audience is for the data (scholars, 	curious lay people, government regulators, etc.) and what types of information will be interesting or useful for them.
	* Determine the types of information you wish to represent and understand why those aspects of the data are important for the project and audience.
	* Determine how, if at all, the audience will be able to access the data (e.g. the audience may be able to directly query the data or may only get aggregated data in pre-packaged tables or visualizations).
* Use the first two or three dozen entries as test cases for the chosen candidate model and iterate to refine as needed.
  * Determine whether or not data enrichment is required and if so, what type(s) and from which source(s).
	  * Adjust the model to accommodate any added data used for enrichment.
  * Once a model is finalized and ready to populate, attend to the project’s longevity and accessibility.
	  * Understand the storage and processing requirements for the model.
	  * Create, and follow, a version control and back-up strategy for the initial data, the model, and any digital artifacts created by the process.
	  * Determine how the model and information will be accessed, by whom, for how long, and what the project’s ‘lifespan’ will be.
* Update initial entries to match finalized model and proceed with further data entry.
* Ensure data accuracy as needed using subject matter expertise (yours or someone 	else’s). 
* Maintain a data dictionary for the model as well as use ‘literate’ naming conventions.
* Maintain and frequently update working lab notes that detail decisions regarding 	changes, alterations, etc. to the data, the rationales for doing so, and the tools, techniques, etc. used.
* Frequently test the structure of the model throughout the entire entry process by performing queries and testing the results against the ground truth of the data.
* If possible, and feasible, perform frequent usability tests throughout the process and adjust any interfaces accordingly, this includes any visualizations, sonifications, physicalization, etc.
* Release the finished project to the expected audience(s).

### Entity Relationship Basics
An entity is a means of modeling data. In an Entity Relationship (ER) approach, entities are sub-models that can be linked together to form more complex relationships. In general, ERs need to have specific characteristics, which are then ‘normalized’ to increase efficiency and effectiveness. When working with ER data, it is fairly common to encounter it in what is called “third normal form.” To get there, the data must:
	1. Eliminate repeating groups
	2. Eliminate redundant data
	3. Eliminate columns not dependent on key

At each step, it is important to reduce the model to only what is necessary to represent the data needed. 

In general, an entity will have:
	• A unique, or primary, key that identifies a discrete row of data
	• A limited number of features, or fields, representing the abstractions needed to model 	the data stored in the entity
	•  A foreign key, which references the primary key of another entity and makes it possible 	to associate data of two or more entities (hence the relationship part of ER).

The relationships in an ER environment come in a few types:
	• One-to-one—for any one X, there is only one Y
	• One-to-many—a specific X has many possible Ys or many Y’s can be associated with a 	particular X
	• Many-to-Many—there are many Xs that are associable with many Ys or many Y’s can 	be associated with many X’s. NOTE: this is 	theoretically possible when modeling data 	but in practice MtM relationships must be decomposed to two or more OtM relationships 	so that a database engine can handle them without conflict.

The type, and directionality, of relationships must be carefully planned and managed. The specifics of how to design and implement a full, working database are far outside the scope of this workshop. These sources should provide more detailed information:

Jan Harrington, Relational Database Design and Implementation, 4th ed., O’Reilly, 2016.

Allen G. Taylor and Richard Blum, SQL All-in-One For Dummies, Wiley, 2024.

Walter Shields, SQL QuickStart Guide: The Simplified Beginner’s Guide to Managing, Analyzing, and Manipulating Data With SQL, Clydebank Media, 2019. 


### Creating a Working Model 
The choices made here are reasonable for the data used for the workshop. As with any entity based approach, each data set will require its own model. Remember, creating a model does not have to be a solitary endeavor and can be the result of a cooperative, consultative process with colleagues and subject matter experts.

For this data, the following model will be used. Note: this is one possible model among many, is not in any way comprehensive, and is being used to illustrate some of the complexities of dealing with manual data.

* Person (personID, lastName, firstName, gender, statusID, ethnicityID)
* Status (statusID, description)
* Ethnicity(ethnicityID, description)
* Session_Record(sessionRecordID, bookNumber, pageNumber, dataEntryClerk, 			verificationClerk, dateEntered, fullMinutesEnteredBy)
* Session_Details (sessionDetailsID, sessionRecordID, sessionTypeID, minutesText, actionID)
* Session_Action (actionID, sessionDetailsID, description, amount)
* Person_to_Session_Details (sessionDetailsID, personID, participationID)
* Session_Type (sessionTypeID, description)
* Participation_Type (participationID, roleDescription)

### Entity Descriptions

**Person:** contains information regarding individuals appearing in the document including, name, social status, and gender. Status refers to significant social status indicators of the person. Gender is strictly binary because the records do not give indications of any non-binary status. Ethnicity is used in a broad sense to situate individuals among significant groups.

**Status:** Social status reflects if a designation (e.g. Sheriff, Pastor, Court Official, Slave) obtained at some point but may not reflect the current status of the person in question for a given proceeding (e.g. someone who served as Sheriff may appear after they no longer hold that title).

**Ethnicity:** Ethnicity is used in broad terms (e.g. European (i.e. colonist), Indigenous (may or may not include specific designations like Wappinger, Esopus, etc.), Black (to indicate people of Black African descent, often enslaved). The broadness of the terms is an artifact of the lack of specificity in the records. 

**Session_Record:** Provides details regarding how each session is recorded in the archival records. Note, each session often contains multiple cases of various types (e.g. litigation, criminal, land transfer, property transfer, etc.)

**Session_Details:** Includes the minutes of each single case included in a given session. 

**Session_Action:** Provides description for the actions taken during cases. Also includes a field for “amount,” which can be used to record fines, purchase prices, etc. This field can be set to 0 or NA for cases that do not involve monetary components.

**Person_to_Session_Details:** Associates individuals with session details. Often a single case will include multiple people, each with one or more role (e.g. plaintiff, respondent, attorney, etc.) 

**Session_Type:** Provides a code and description for the most common forms of case carried out in a session (e.g. criminal, litigation, land transfer, etc.)

**Participation_Type:** Provides a code and description for the roles taken on by individuals during a particular case (e.g. plaintiff, respondent, etc.)

### Implementation
Each of these entities will be represented on a spreadsheet. To increase portability and promote easier maintenance, it is suggested that 1) each sheet be maintained as a plain .csv file (as opposed to proprietary encodings) and 2) that each sheet be kept separate (i.e. not nested as tabs on a unified spreadsheet). Following these suggestions will make it possible to use a variety of methods for creating a database or database-like structure (including Pandas data frames, SQLlite using Python, MySQL Workbench, etc.) as well as facilitate a variety of data query and analysis approaches.

### Required Files
To follow this workshop, the following files are needed (available in the Data folder in the repository):
* Ulster_Colonial_Records_Small.txt
* Person.csv
* Status.csv
* Ethnicity.csv
* Session_Record.csv
* Session_Details.csv
* Session_Type.csv
* Session_Action.csv
* Person_to_Session.csv
* Participation_Type.csv

In addition, it will be necessary to access the following Python scripts (available in the Scripts folder in the repository):
* Initial_DB_Creation.ipynb
* Simple_DB_Query.ipynb
* Colonist_Simple_Viz.ipynb

### General Comments:
The structure used for this workshop is one among many possible ways of organizing the information. It is not meant to provide a fool-proof guide for creating relational databases and is highly idiosyncratic to the people, places, and events recorded in the data. If you wish to ensure that you are using an efficient and highly effective entity relationship model, it is best to consult with folks who specialize in database design. 

The scripts used for creating a simple database and running simple queries is meant as a gentle introduction to performing basic database tasks in a Python environment. We cover a tiny subset of actions possible; if you wish to dive deeper, you will find many resources on the Web. In a similar vein, the script for analysis and visualization is meant as a starting point and is not comprehensive. 

The point of the workshop is less about specific structures and more about thinking through workflows for handling data that needs manual organization and input. When deciding on a structure for manual data, it is best to think about the types of insights you or your projected audience hope to gain from it. This will help you determine which relationships are necessary for your model and will help you think through the variables you want to record for later analysis. Remember, the more entities used in a model involving manual data, the more time and effort will be needed for initial data entry as well as for any ongoing upkeep. In addition, it will be necessary to exercise a great deal of quality control at every stage of data entry to ensure data integrity.
