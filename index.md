# Database Keys

##### By: 
##### Grace Walkuski
##### Kendall DiLorenzo
##### Michael Hewitt
##### Ian Hecker

##### MSU CSCI 440 Database Systems, Fall 2018


This tutorial will outline the difference between Primary Keys, Candidate Keys, Foreign Keys, Composite Keys, and Surrogate Keys. How do you assign them? When do you need them? What are they used for? This is a topic that will help you create stronger Relational Schemas and therefore design and build better databases. 

#### What are Keys?

Keys in a database are fields in a table that help to uniquely identify data. This is helpful when you want to access a collection of data (a tuple in a database). In the example below, you can use the key 9876 to access Ron's information. This helps give you confidence that you are getting the data you want. If you used the lookup "Weasley", you could get Ginny, Ron, or both. We use keys instead to get the correct data.


#### Student

|**WizardID**|**FirstName**|**Lastname**|**HouseID**|
|----|----|----|----|
|6543|Hermione|Granger|101|
|6667|Draco|Malfoy|104|
|5467|Neville|Longbottom|101|
|3434|Luna|Lovegood|103|
|1234|Harry|Potter|101|
|3456|Ginny|Weasley|101|
|9876|Ron|Weasley|101|


#### House

|**HouseID**|**Name**|**Founder**|**HousePoints**|
|-------|----------|-----------------|-----------|
|101|Gryffindor|Godric Gryffindor|482|
|102|Hufflepuff|Helga Hufflepuff|352|
|103|Ravenclaw|Rowena Ravenclaw|426|
|104|Slytherin|Salazar Slytherin|472|


We use these keys to lookup data from one table and associate it with data in another table. In the example above, if you want to know what house Luna is in, you take her HouseID and look up the data in the House table and find that she is in Ravenclaw.

  
## Primary Keys

Primary keys are a unique set of attributes that identify tables in the relational database. Some common primary keys used are social security numbers (SSN) and uniquely assigned identification numbers like student ID. It is important that primary keys are unique and do not contain null values as this will cause data integrity issues. Using primary keys correctly can save a lot of time when using queries to attain database information. In the example below, you can see that WizardID is unique and therefore the primary key. This primary key allows the user to know the wizard and their patronus.


#### Student

|**WizardID**|**FirstName**|**Lastname**|
|---------|---------|--------|
|1234|Harry|Potter|
|6543|Hermione|Granger|
|9876|Ron|Weasley|


#### StudentPatronus

|**WizardID**|**Patronus**|
|---------|--------|
|1234|Stag|
|6543|Otter|
|9876|Dog|

As seen above the primary key, WizardID is used to connect the two tables. For the WizardID 1234, his name is Harry Potter and his patronus is the stag. This allows tables to have pertinent information. When creating the Student table in SQL you can specify primary key as seen below.
```
create table Student(
    WizardID integer primary key,
    FirstName text,
    LastName text
);
```
## Foreign Keys
Foreign keys are used to link one table to another. It is a column in one table that stores the primary key of the referenced table in order to access one specific, unique entry of that table. It is crucial that anytime a foreign key is inserted into a table, there has to be a corresponding key for the table it is attempting to reference. Incorrect insertions, as well as deletions from the referenced table could result in the destruction of that relationship. A good example of when to use this is with the patronus table referenced earlier. If we added a column for the ID of each entry, the wizard ID functions as solely a foreign key to access the name of the wizard who owns the patronus. This will make it easier to find a specific (wizard, patronus) pair, specifically if two wizards have the same patronus, like Harry and his father, James.

#### Wizards

|WizardID |FirstName|Lastname|
|---------|---------|--------|
|1234|Harry|Potter|
|0153|James|Potter|
|6543|Hermione| Granger|
|9876|Ron|Weasley|


#### Patronuses

|PatronusID|WizardID|Patronus|
|---------|---------|--------|
|1|1234|Stag|
|2|0153|Stag|
|3|6543|Otter|
|4|9876|Dog|


```
create table Patronus(
    PatronusID integer primary key,
    WizardID integer REFERENCES Wizard(WizardID),
    Patronus text
);
```

## Composite Keys
A composite key is a combination of attributes that can help uniquely identify the rows of the table. The uniqueness is guaranteed when the attributes are used together, where individually they may not be unique. A common composite key would be the use of a house number and street. As those two attributes individually are not unique, but when combined they create a unique key, given that it is local. To create a composite key all you have to do is define the attributes then make them a primary key as shown below.
```
create table HouseAddress(
    Address text,
    StreetName text,
    Primary Key(Address, StreetName)
);
```

## Surrogate Keys
Surrogate keys are sometimes called system generated keys or database sequence numbers.  They are generated by the database and not derived from any data in the table. This is good for logging chronological data. A good example of this is if you were logging the house points won and lost throughout the school year.

#### House Points

|EntryNo|WizardID|HouseID|PointsEarned|Reason|
|-------|--------|-------|------------|------|
|01|6667|104|-20|out of bed at midnight|
|02|1234|101|-20|poked a dragon with a stick|
|03|9876|101|50|the best-played game of chess Hogwarts has seen in many years|
|04|6543|101|50|cool logic in the face of fire| 
|05|1234|101|50|pure nerve and outstanding courage|
|06|5466|101|10|standing up to his friends for what he thought was right|

Surrogate keys can be used as primary keys.
Surrogate keys can include a timestamp.

To make the database create a surrogate key for each entry, use the following syntax when creating your table:
```
create table HousePoints(
    EntryNo integer primary key autoincrement,
    WizardID integer,
    HouseID integer,
    PointsEarned integer,
    Reason text
);
```

## Candidate Keys


Candidate Keys are keys that can be used to uniquely identify a specific entry in a database. Candidate keys can be any column or combination of columns. Therefore, a table may potentially have more than one option of a candidate key, due to the combinations of table columns. If a chosen candidate key contains more than one column, it is also a  **composite key**. It's helpful to imagine candidate keys as their name implies; *candidates*. Think of the entire set of candidate keys of a table, as the step towards choosing a **primary key**. Once a column, or multiple columns, are selected to represent a table's entries, the candidate key becomes the **primary key** of that table. Additionally, candidate keys must remain the same set of columns for its entire lifetime, because it could potentially lose its property of uniquely identifying specific database entries.


#### Professors of Hogwarts

|**StaffID**|**FirstName**|**LastName**|**ProfessorOf**|
|-------|-----|-----|---|
|956|Minerva|McGonagall|Transfiguration|
|984|Severus|Snape|Potions|
|970|Quirinus|Quirrell|Defence Against the Dark Arts|
|962|Sybill|Trelawney|Divination|


In this case, all of the columns, and any combination of columns, are candidate keys because they all uniquely identify an entry in the table. The column StaffID is a surrogate key and therfore is unique to a staff member. It is similar to a SSN. Therefore, this makes it one of the best (*if not the best*) of the candidate keys, because it is guaranteed to always be unique. It is unique to each professor and can therefore uniquely identify a specific database entry. If we *did not have a StaffID*, like this:


#### Professors of Hogwarts 2

|**FirstName**|**LastName**|**ProfessorOf**|
|-----|-----|---|
|Minerva|McGonagall|Transfiguration|
|Severus|Snape|**Potions**|
|Quirinus|Quirrell|Defence Against the Dark Arts|
|Sybill|Trelawney|Divination|
|Charity|Burbage|Muggle Studies|
|Filius|Flitwick|Charms|
|Horace|Slughorn|**Potions**|
|Rubeus|Hagrid|Magical Creature Care|


Then a good candidate key may be the combination of FirstName & LastName. *Notice that the attribute ProfessorOf in the table above has two professors who teaches Potions.* This is why we cannot say the column ProfessorOf is a candidate key, because it does not uniquely identify a specific entry.


## Example Problems


Work the following problems to check your understanding of keys
1. What are keys used for?
2. What is another example of when a surrogate key would be useful (other than the example given)?
3. In the *Professors of Hogwarts 2* table in the Candidate key section, is only the column *FirstName* a candidate key? (Hint: Does it uniquely identify specific entries?)
4. Imagine you were suddenly the Head Wizard of Hogwarts, and your first problem was to solve the chaos in the mail room. Delivery owls can't distinguish which maailbox belongs to which student, and mail is flying everywhere. Create a few mailbox label ideas to uniquely identify student mailboxes. What would you choose your labels to be comprised of, Head Wizard? (Be creative!)
5. What are some other primary keys besides SSN?
6. What are some other composite keys besides address and stree name?

## References


##### [Candidate Keys](https://www.lifewire.com/candidate-key-definition-1019246)

##### [Surrogate Keys](https://www.sisense.com/blog/when-and-how-to-use-surrogate-keys/)

##### [Foreign Keys](https://www.techopedia.com/definition/7272/foreign-key)

##### [Primary Keys](https://www.tutorialspoint.com/sql/sql-primary-key.htm)

##### [Composite Keys](https://www.javatpoint.com/sql-composite-key)


## Additional Reading

##### [Database Keys: The Complete Guide (Surrogate, Natural, Composite & More)](https://www.databasestar.com/database-keys/)

##### [How to Use Keys to Access Information Quickly in a SQL Database](https://www.dummies.com/programming/sql/how-to-use-keys-to-access-information-quickly-in-a-sql-database/)

##### [Relational Database Management Keys](http://rdbms.opengrass.net/2_Database%20Design/2.1_TermsOfReference/2.1.2_Keys.html)


## Solutions


1. Keys are used to uniquely identify data
2. Potential examples when surrogate keys are useful:
	* logging quiddich game results
	* logging hours studied for diffe
	rent classes
3. Yes! All the FirstName entries are unique, and therefore the FirstName qualifies as a candidate key
	* However, as the table expands for more staff perhaps the same FirstName will occur again!
4. Potential candidate keys: 
	* a student's house (Gryffindor, Slytherin, Hufflepuff, Ravenclaw) plus their last name
	* a uniquely generated student ID number
	* a trace of a student's magic wand power (perhaps all wizard wands are unique)
	* a student's first name, last name, and city of birth
	* a student's full name and favorite spell incantation
5. Driver license number, email addresses, and telephone numbers. If you can't find any primary key you can always make your own by incrementing a nubmer for each row.
6. Course and course number, Student and student number, and department and department number

