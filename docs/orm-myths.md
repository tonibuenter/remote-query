

  
# ![Burden of ORM](g4932sm.png) A Critical View on ORM Tools

Assume the situation of junior developer starting to program against relational databases (RDB) with Java. 
A closer look at common articles and blogs will harden the impression that object relational (ORM) mapping tools 
are the way to pursue. He or she may finally choose a tool such as _Hibernate_, _EJB_, _TopLink_ or others the like.

Further, many large companies from e.g. the financial industry have an explicit policy using 
even a specific ORM tool for their in-house development.

In this article I like to point out some thought and experiences about the dark side of the ORM approach and advocate
our approach _RemoteQuery_.

Topics like:

* OO / RDB Properties (Granularity, Inheritance, Identity, ...)
* Maintainabilty
* Design Implications
* ORM Survival Tips
* Try _RemoteQuery_


In this article I refer to - my purpuse interchangable - an article from [Tutorialpoints.com](https://www.tutorialspoint.com/hibernate/orm_overview.htm) that points out the positive qualities of ORM tools [1].


I will critically cita and comment point by point and conclude with a final statement.

### Why Object Relational Mapping (ORM)?

**Citation**:


> When we work with an object-oriented system, there is a mismatch between the object model 
> and the relational database. RDBMSs represent 
> data in a tabular format whereas object-oriented languages, 
> such as Java or C# represent it as an interconnected graph of objects.



**Comment**:

The gab between relational and object models is overstated. 
As soon as you are using technical IDs for single table entries, the design
_is_ object-oriented. In many cases this is a good design even without ORM applied.

On the other side, if the database is _legacy_ and 'deep' relational designed, 
ORM tools do not provide elegant mappings rather often resulting in a lot of hand written glue code and 
configuration. 



### Granularity

**Citation**:

> Sometimes you will have an object model, which has more 
> classes than the number of corresponding tables in the database.

**Comment**:

In real time this situation is very rare. As the database structure has a longer life span than the classes it is reasonable to
design the classes according to the tables. Doing otherwise is calling for unforced difficulties.

With _RemoteQuery_ we show that in the most cases there is actually no need for ORM data access classes.


### Inheritance

**Citation**:

> RDBMSs do not define anything similar to Inheritance, which is a natural 
> paradigm in object-oriented programming languages.

**Comment**:

Even being enthusiastic about object-oriented concepts, I learned that implementing data access classes 
with inheritence result in a painful experience.
Avoiding inheritance on implementation in OO and RDB models is by far the best solution.


### Identity

**Citation**:

> An RDBMS defines exactly one notion of 'sameness': the primary key. Java, however, 
> defines both object identity (a==b) 
> and object equality (a.equals(b)).

**Comment**:

This is truly a academic view, but not really a problem. 
If needed, you always can overwrite the equals method.

### Associations

**Citation**:

> Object-oriented languages represent associations using object references whereas an RDBMS 
> represents an association as 
> a foreign key column.

**Comment**:

Most business cases can be implemented with flat tables. 
Objects with relations can easily be connected by using joins on the database side and hash tables over their keys on the client side.

With _RemoteQuery_ we usually solved this by connecting objects on the client side.

### Navigation

**Citation**:

> The ways you access objects in Java and in RDBMS are fundamentally different.

**Comment**:

It is only true to a certain extent. If you take the SQL join and group by statements into consideration close similarities can be seen between object-oriented navigation and relational joining of tables. 

With _RemoteQuery_ we enjoy the complementary character of RDM and object-oriented programming.


## Propagated Advantages

Having seen now that the problems and gaps between 
OO and RDB models are not that grave in the real world,
I like to comment on the claimed advantages.

**Citation**:

### Business Object over DB Tables

> Let’s business code access objects rather than DB tables.

**Comment**:

Actually there are no real arguments for that. Second, DB tables, views and even stored procedures do
live often significantly longer than application code. I would propagate: **keep business logic in
Java code to a absolute minimum**. For reasons of maintainability and performance rather use 
queries, views and even stored procedure for business logic.

With RemoteQuery we are able to put more than 95% of the business logic into the RemoteQuery scripts.

### Encapsulation

**Citation**:

> Hides details of SQL queries from OO logic.

**Comment**:

One of ORMs central achievment is  reading and writing parent child releations. In reality it turns out that 
in many cases the queries for read and write are slowing down when the number of records rise. The ORM tools solution ends up in
writing specific queries which optimize the process. So, it ends up where it could have began.

### Business Concepts


**Citation**:

> Entities based on business concepts rather than database structure.

**Comment**:

What is the background for this paradigm? Modelling business concepts have been among the 
top reasons for inventing and using RDB in the first place.

### Transaction Management and Key Generation

**Citation**:

> Transaction management and automatic key generation.

**Comment**:

This is not a main ORM topic. ORM tools actually help on that. 

### Productivity

**Citation**:

> Fast development of application.

**Comment**:

Fast and stable development with low maintenance is a honest intention of ORM. We have seen that _RemoteQuery_ compared to ORM (Hibernate)
achieves a much higher productivity. 


### Maintainabilty

ORM tools require the maintenance of overlapping concepts and sources. Relational design, Java POJO code mapping configuration. Maintenance is considered to be a costly and error prone task.


### Design Implications

ORM driven persistency produces strong dependencies between the object-oriented world and the relational world. A successful design is always a trade-off between these worlds. The victims of this compromise are simplicity, elegance, performance, readability.

It gets even worse when the data has to support the following requirements:

* historize records
* keep workflow states such as draft, verified, checked out, archived
* reporting 



## ORM Survival Tips

ORM is often the center of back-end server development problems and instabilites. That said, we should not blame ORM per se for them. It is still true that 'A Fool with a Tool is still a Fool'. In case of ORM there are some caveat that often give rise the negative impacts.

Here a not final list:

* Avoid server-side state especially session level caching of data. 
* Be very carefull with collection read and write.
* Be very carefull with sophistated transaction handling. IMHO explicit transaction tracking with tables is by far better then implicit transaction handling.
* ORM sees the database as a CRUD storage. This is driving on collision course with writing reports and data mining. A good RDB design is
still crucial and has long-ranging effects even beyond IT (see also: _The Art of SQL_ [2]).

_Remark_ : Some of these survival tips are actually recommendations of avoidance in otherwise praised tool features.

## Try _RemoteQuery_

_RemoteQuery_ tries to minimize the object-oriented part to its minimum. This gives the RDB design priority over the OO design. Experience so far - over 2000 services build and running - is encouraging. 


For complicated requirements and system-oriented tasks Java / Python program show their strength. Examples are:

* Calculating work time according to a set of rules about vacation, overtime, break time etc.
* Processing spreadsheet data (RDBMS are not good in spreasheet processing)
* File management

On the other side the following applications have been build just with SQL and the less then 10 _RemoteQuery_ commands:

* Company Accounting
* HR Data Management
* Training and Skill tool
* Chat Application
* Meeting Application
* Parking Space Rental Application


So far we enjoyed the lightness, maintenability and stability of SQL and the 10 _RemoteQuery_ commands
logic code ;-). Have a try.

### Positive Reactions

We had positive reaction from project manager such as:

> Usually the middle tier (business logic with ORM) is the slow, error prone and costly part in development, but with _RemoteQuery_ it's a totally different story.

> People with good SQL knowledge can build directly many parts of the middle tier. Other projects that still use an ORM tool always need additional personel for the middle tier. 

![Move from ORM to RemoteQuery](g5097.png) 



## Reference

| Nr | Reference Detail |
| ---- | ----|
| 1 | [ORM Overview Tutorialpoints.com](https://www.tutorialspoint.com/hibernate/orm_overview.htm) |
| 2 | **The Art of SQL** by Stéphane Faroult with Peter Robson; 2006 O’Reilly Media, Inc |

