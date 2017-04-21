# Introduction 
### Graph Theory

Undergraduate 2017 Project for Graph Theory at Galway-Mayo Institute of Technology. Requirement is to design and prototype a Neo4j Database for use in a timetabling system for a third level institute like [GMIT](https://learnonline.gmit.ie/). 

#### Project Requirements
1. The database should store information (Student groups, classrooms, lecturers, and work hours – just like the currently used timetabling system at GMIT.
2. A document detailing the design of the database. (It should clearly state what data needs to be stored by a timetabling system, and how that data is to be represented in the database. This means that design choices about what data are represented as nodes, relationships, labels, relationship types or properties must be clearly specified. It should also specify, and give examples of, Cypher queries for creating, retrieving, updating and deleting data.
3. A prototype Neo4j database following that design and using data from GMIT. (The data can be copied or scraped from GMIT’s timetabling website, or otherwise obtained.)
4. Seperate section in the design document, outlining how the data have been obtained for the prototipe database.

#### Installation Guide
1. Install [Neo4j](https://neo4j.com/download/).
2. Download & unzip this project.
3. Launch Neo4j & select the database path location of this project.
4. Username: neo4j Password: neo4j (This may vary, as Neo4j might only ask for your personal informations).
5. Further [guide](https://neo4j.com/developer/get-started/) and documentation for Neo4j can be found [here](https://neo4j.com/).

# 1. Storing information
The folowwing data will be stored by Neo4j GMIT timetable database:
- An Institute.
- List of Departments in the Institute.
- List of Courses in individual Departments.
- List of Years that each Course is broken into. (e.g. 4 Year course will be broken into 4 individual sections)
- List of Semesters that each Year us broken into. (e.g. 1 Year broken into 2 Semesters, if course is 4 years then there will be 8 semesters all together
- List of Modules in each individual Semester
- List of Lecturers teaching individual Modules
- List of Rooms available for venue bookings
- List of Groups that can attend the Room with already assigned venue to the module, which is appropriate to course of the groups
- List of Days that Rooms can be given venues for
- List of Timeframes that individual Days have that also is part of venue (starting with 8am to 10pm with 1hour timeframes)

Some of the adjustments to individual data are pure speculation sice the timetable could be used by every staff of the Institute, where students dont have an access to that data. e.g. I decided to use timeframe of 8am to 10pm for each day since those are the hours that the college is opened for, therefore it is only natural to speculate that staff members must be in the insitute carrying out some sort of work.

# 2. Initial design of the database
I have created initial model before the database was created. The model includes basic relationships between the nodes.

![alt tag](https://cloud.githubusercontent.com/assets/15648358/25286311/34e4915e-26b5-11e7-9f99-cf8eeeafa17c.png)

The screenshot shows my first thoughts about the current timetabling system of GMIT. However my goal was to create a model with entities and appropriate relationships so that it would be easy to understand. It would be very complex to use this model to create a timetable This model can definitely be improved and made more simple. Below is an initial list of Nodes & Relationships: 
#### Nodes - Institute, Department, Course, Year, Semester, Teacher, Module, Group, Room, Capacity, Week, Day, Timeframe
#### Relationships - HAS, TEACH, IN, OF, ON, AT

Even when I knew that the model is definitely not in the final form, I started creating the Neo4j database according to the above model.
### Bellow are the Cypher queries used to create certain nodes and relationships between them:
1. #### Create Institute node 
   ###### CREATE (i:Institute {name:"GMIT"})
2. #### Create Department node
   ###### CREATE (dep:Department {name:"Computer Science & Applied Physics"})
3. #### Create Course node
   ###### CREATE (c:course {name:"Computing in Software Development"})
4. #### Create Year nodes
   ###### CREATE (y1:year {name:"Year-1"}),
   ###### (y2:year{name:"Year-2"}), 
   ###### (y3:year{name:"Year-3"}), 
   ###### (y4:year{name:"Year-4"})
5. #### Create Semester nodes
   ###### CREATE (s1:semester {name:"S-1"}),
   ###### (s2:semester {name:"S-2"}),
   ###### (s3:semester {name:"S-3"}),
   ###### (s4:semester {name:"S-4"}),
   ###### (s5:semester {name:"S-5"}),
   ###### (s6:semester {name:"S-6"}),
   ###### (s7:semester {name:"S-7"}),
   ###### (s8:semester {name:"S-8"})
6. #### Create Module nodes for semester 5 & 6 of year 3
   ###### CREATE (m1:module {name:"Mobile Applications Development 2"}),
   ###### (m2:module {name:"Database Management"}),
   ###### (m3:module {name:"Server Side Rad"}),
   ###### (m4:module {name:"Graph Theory"}),
   ###### (m5:module {name:"Software Testing"}),
   ###### (m6:module {name:"Profesional Practice in IT"}),
   ###### (m7:module {name:"Graphics Programming"}),
   ###### (m8:module {name:"Data Centric Rad"}),
   ###### (m9:module {name:"Data Representation and Querying"}),
   ###### (m10:module {name:"Object Oriented Programming"}),
   ###### (m11:module {name:"Software Quality Management"}),
   ###### (m12:module {name:"Operating Systems 1"})
   
   Now that the nodes are created the relationship can be applied. The create node queries can also create the relationship, but I decided to keep the queries simple in case something goes wrong and changes would have to be reverted or changed again.
7. #### Create Relationship between Institute and Department
   ###### MATCH (i:Institute), (dep:Department) CREATE (i)-[r:HAS]->(dep)
8. #### Create Relationship between Department and Course
   ###### MATCH (dep:Department), (c:course) CREATE (dep)-[r:HAS]->(c)
9. #### Create Relationship between Course and Course Year
   ###### MATCH (c:course), (y:year) CREATE (dep)-[r:HAS]->(c)
10. #### Create Relationspip between Year and appropriate Semesters
    ###### MATCH (y:year),(s:semester) WHERE y.name = "Year-1" AND s.name = "S-1" CREATE (y)-[r:HAS]->(s)
    ###### MATCH (y:year),(s:semester) WHERE y.name = "Year-1" AND s.name = "S-2" CREATE (y)-[r:HAS]->(s)
    ###### MATCH (y:year),(s:semester) WHERE y.name = "Year-2" AND s.name = "S-3" CREATE (y)-[r:HAS]->(s)
    ###### MATCH (y:year),(s:semester) WHERE y.name = "Year-2" AND s.name = "S-4" CREATE (y)-[r:HAS]->(s)
    ###### MATCH (y:year),(s:semester) WHERE y.name = "Year-3" AND s.name = "S-5" CREATE (y)-[r:HAS]->(s)
    ###### MATCH (y:year),(s:semester) WHERE y.name = "Year-3" AND s.name = "S-6" CREATE (y)-[r:HAS]->(s)
    ###### MATCH (y:year),(s:semester) WHERE y.name = "Year-4" AND s.name = "S-7" CREATE (y)-[r:HAS]->(s)
    ###### MATCH (y:year),(s:semester) WHERE y.name = "Year-4" AND s.name = "S-8" CREATE (y)-[r:HAS]->(s)
11. #### Create Relationship between Semesters and appropriate Modules
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-5" AND m.name = "Graphics Programming" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-5" AND m.name = "Data Centric Rad" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-5" AND m.name = "Data Representation and Querying" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-5" AND m.name = "Object Oriented Programming" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-5" AND m.name = "Software Quality Management" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-5" AND m.name = "Operating Systems 1" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-6" AND m.name = "Mobile Applications Development 2" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-6" AND m.name = "Database Management" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-6" AND m.name = "Server Side Rad" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-6" AND m.name = "Graph Theory" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-6" AND m.name = "Software Testing" CREATE (s)-[r:HAS]->(m)
    ###### MATCH (s:semester), (m:module) WHERE s.name = "S-6" AND m.name = "Profesional Practice in IT" CREATE (s)-[r:HAS]->(m)

After creating the above nodes and relationships between them, the Neo4j model should look just like the below screenshot.
##### If needed one of the following simple Cypher queries can bring it up.
- ###### MATCH (n) RETURN n
- ###### MATCH p=()-[r:HAS]->() RETURN p
![alt tag](https://cloud.githubusercontent.com/assets/15648358/25246152/aa8bb962-25fe-11e7-9460-f87a082a8a2c.png)
So far the model shows the detailed nodes and relationships created between them, which are correct acording to the initial model.

The person using this model would also need to expand on it since data I have used in this project was only to match one part of Computing in Software Development Course of Computer Science & Applied Physics Department in Galway-Mayo Institute of technology. The decisions I made about the nodes and the relationships were to ensure the understanding of the model by person making timetable system.

Not all of the data available on [GMIT Timetable System](http://timetable.gmit.ie/) website was used as the model I have created wouldn't be clear to understand and would include too much informations. A lot of infromations all related together, in one way or another, would result in advanced and difficult to understand overall graph therefore I have kept it as simple as possible.

When creating the Neo4j initial model I realised that the database would store too much unncecessary/ duplicate informations. The model turns out to be good enough until the Capacity and Week is encountered in the model. I realised that when i would want to use capacity as an actual node then there would be too many nodes of exact same size number e.g. 22 as almost every room in the Institute should fit a full class in. 

Therfore I decided to not include it at all. The Capacity could be included as a property on each Room. Modules individual groups could then have seperate relationship or relationship property when requesting a room which would be completely up to the person making the timetable. 
#### Example query could look as follows:
###### CREATE (r:room {name:"145", capacity:"22"}, (r:room {name:"100", capacity:"19"}
###### CREATE (g:group {name:"Group-Z", size:"22"}
###### MATCH (r:room),(g:group) WHERE r.capacity <= g.size RETURN r
The above query could create 3 nodes (2 rooms with different capacity properties and 1 group with a size property). Match query would searh for all existing rooms and groups where capacity of each room is less than or equal to size of the group. Which should return just the node room with the property name of 145 and capacity of 22.

Then when I thought about the week node and the amount of data that would have to be taken into account, I figured that it is not necessary to have weeks at all. Having weeks would only mean that data will have to be duplicates of one week for the whole semester which increases the size of the database, when it can be shortened and it slows down the queries done on the database. Because of this I changed the initial model and decided to stick with the one below.

![alt tag](https://cloud.githubusercontent.com/assets/15648358/25252159/ddc6c0aa-2613-11e7-9592-d6886ffb1cd9.png)

The above model shows the following data that will be represented in the Neo4J database and go down in the tree hierarchy as follows:
1. Institute -- HAS --> Departments
2. Department -- HAS --> Courses
3. Course -- HAS --> Years
4. Year -- HAS --> Semesters
5. Semester -- HAS --> Modules
6. Teachers -- TEACH --> Modules
7. Modules -- IN --> Rooms
8. Groups -- IN --> Rooms
9. Rooms -- ON --> Days
10. Days -- AT --> Timeframes

I decided to represent most of the data by nodes in the Neo4j database as some of the relationships made me think it was necessary. 
Below is a list of Nodes & Relationships: 
#### Nodes - Institute, Department, Course, Year, Semester, Teacher, Module, Group, Room, Day, Timeframe
#### Relationships - HAS, TEACH, IN, ON, AT

# 3. Further Development
Now that the model is finalised and aim is more clear, I created more nodes and relationships between them according to the hierarchy of the model.

### Bellow are the Cypher queries used to create certain nodes and relationships between them:
1. ####  Create Lecturer nodes
   ###### CREATE (l1:lecturer {name:"Deirdre O'Donovan"}),
   ###### (l2:lecturer {name:"Gerard Harrison"}),
   ###### (l3:lecturer {name:"Martin Hynes"}),
   ###### (l4:lecturer {name:"Ian McLoughlin"}),
   ###### (l5:lecturer {name:"Damien Costello"}),
   ###### (l6:lecturer {name:"Brian McGinley"}),
   ###### (l7:lecturer {name:"John Healy"}),
   ###### (l8:lecturer {name:"Joseph McGinley"})
   ###### (l9:lecturer {name:"Daniel Cregg"}),
   ###### (l10:lecturer {name:"Martin Kenirons"}),
   ###### (l11:lecturer {name:"Patrick Mannion"}),
   ###### (l12:lecturer {name:"Kevin O'Brien"})
2. #### Create Relationship between Lecturer and the Module
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Damien Costello" AND m.name = "Mobile Applications Development 2" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Deirdre O'Donovan" AND m.name = "Database Management" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Patrick Mannion" AND m.name = "Database Management" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Gerard Harrison" AND m.name = "Server Side Rad" CREATE (l)-[r:TEACH]->(m)

   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Ian McLoughlin" AND m.name = "Graph Theory" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Martin Hynes" AND m.name = "Software Testing" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Damien Costello" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Daniel Cregg" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Gerard Harrison" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "John Healy" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Martin Hynes" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Martin Kenirons" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Patrick Mannion" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Brian McGinley" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Ian McLoughlin" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Kevin O'Brien" AND m.name = "Profesional Practice in IT" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Brian McGinley" AND m.name = "Graphics Programming" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Gerard Harrison" AND m.name = "Data Centric Rad" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Ian McLoughlin" AND m.name = "Data Representation and Querying" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "John Healy" AND m.name = "Object Oriented Programming" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Joseph McGinley" AND m.name = "Software Quality Management" CREATE (l)-[r:TEACH]->(m)
   ###### MATCH (l:lecturer), (m:module) WHERE l.name = "Martin Hynes" AND m.name = "Operating Systems 1" CREATE (l)-[r:TEACH]->(m)
   Below is the Neo4j representation of above relationship that was created:
   ![alt tag](https://cloud.githubusercontent.com/assets/15648358/25294268/4ea12780-26d6-11e7-8ad1-84e7935ff42f.png)  
3. #### Create Room nodes
   ###### CREATE (r1:room {name:"145"}),
   ###### (r2:room {name:"162"}),
   ###### (r3:room {name:"165"}),
   ###### (r4:room {name:"208"}),
   ###### (r5:room {name:"223"}),
   ###### (r6:room {name:"379"}),
   ###### (r7:room {name:"436 CR5"}),
   ###### (r8:room {name:"470 Computing Practical Lab"}),
   ###### (r9:room {name:"480 CR7"}),
   ###### (r10:room {name:"481 CR4"}),
   ###### (r11:room {name:"482 CR3"}),
   ###### (r12:room {name:"483 CR2"}),
   ###### (r13:room {name:"484 CR1"}),
   ###### (r14:room {name:"837"}),
   ###### (r15:room {name:"903"}),
   ###### (r16:room {name:"938"}),
   ###### (r17:room {name:"939"}),
   ###### (r18:room {name:"940"}),
   ###### (r19:room {name:"941"}),
   ###### (r20:room {name:"994"}),
   ###### (r21:room {name:"995"}),
   ###### (r22:room {name:"996"}),
   ###### (r23:room {name:"997"}),
   ###### (r24:room {name:"1000"}),
   ###### (r25:room {name:"PF05"}),
   ###### (r26:room {name:"PF18"})
4. #### Create Relationship between Module and Room
   ###### MATCH (m:module), (r:room) WHERE m.name = "Database Management" AND r.name = "994" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Mobile Applications Development 2" AND r.name = "223" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Graph Theory" AND r.name = "PF05" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Software Testing" AND r.name = "481 CR4" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Software Testing" AND r.name = "436 CR5" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Database Management" AND r.name = "481 CR4" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Database Management" AND r.name = "482 CR3" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Mobile Applications Development 2" AND r.name = "470" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Graph Theory" AND r.name = "379" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Graph Theory" AND r.name = "162" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Graph Theory" AND r.name = "938" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Profesional Practice in IT" AND r.name = "208" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Server Side Rad" AND r.name = "997" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Software Testing" AND r.name = "939" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Database Management" AND r.name = "995" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Mobile Applications Development 2" AND r.name = "PF18" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Server Side Rad" AND r.name = "436 CR5" CREATE (m)-[rel:IN]->(r)
   ###### MATCH (m:module), (r:room) WHERE m.name = "Graph Theory" AND r.name = "208" CREATE (m)-[rel:IN]->(r)
5. #### Create Group nodes
   ###### CREATE (g1:group {name:"Group-A"}),
   ###### (g2:group {name:"Group-B"}),
   ###### (g3:group {name:"Group-C"})
6. #### Create Relationship between Group and Room
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "994" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "994" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "994" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "223" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "223" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "223" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "PF05" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "481 CR4" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "436 CR5" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "482 CR3" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "470" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "379" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "436 CR5" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "162" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "938" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "938" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "938" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "208" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "208" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "208" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "997" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "997" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "997" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "939" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "939" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "939" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "995" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "995" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "995" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "PF18" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-C" AND r.name = "PF18" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-A" AND r.name = "436 CR5" CREATE (g)-[rel:IN]->(r)
   ###### MATCH (g:group), (r:room) WHERE g.name = "Group-B" AND r.name = "482 CR3" CREATE (g)-[rel:IN]->(r)
   Steps 3, 4, 5 & 6 will look like this in Neo4j graph when created:
   ![alt tag](https://cloud.githubusercontent.com/assets/15648358/25246181/c48673b6-25fe-11e7-89f7-1e771fac092d.png)  
7. #### Create Weekday nodes
   ###### CREATE (d1:day {name:"Monday"}),
   ###### (d2:day {name:"Tuesday"}),
   ###### (d3:day {name:"Wednesday"}),
   ###### (d4:day {name:"Thursday"}),
   ###### (d5:day {name:"Friday"})
8. #### Create Timeframe nodes
   ###### CREATE (t1:timeframe {name:"8.00-9.00"}),
   ###### (t2:timeframe {name:"9.00-10.00"}),
   ###### (t3:timeframe {name:"10.00-11.00"}),
   ###### (t4:timeframe {name:"11.00-12.00"}),
   ###### (t5:timeframe {name:"12.00-13.00"}),
   ###### (t6:timeframe {name:"13.00-14.00"}),
   ###### (t7:timeframe {name:"14.00-15.00"}),
   ###### (t8:timeframe {name:"15.00-16.00"}),
   ###### (t9:timeframe {name:"16.00-17.00"}),
   ###### (t10:timeframe {name:"17.00-18.00"}),
   ###### (t11:timeframe {name:"18.00-19.00"}),
   ###### (t12:timeframe {name:"19.00-20.00"}),
   ###### (t13:timeframe {name:"20.00-21.00"}),
   ###### (t14:timeframe {name:"21.00-22.00"})
9. #### Create Relationship between Day and all Timeframes
   ###### MATCH (d:day),(t:timeframe) CREATE (d)-[r:AT]->(t)
   Steps 7, 8 & 9 will look like this in Neo4j graph when created:
   ![alt tag](https://cloud.githubusercontent.com/assets/15648358/25246258/f93a1d56-25fe-11e7-817d-427ca2412bfe.png)
10. #### Create Relationship between Room and all Days
    ###### MATCH (r:room), (d:day) CREATE (r)-[rel:ON]->(d)
    Step 10 visual representation of the relationship:
    ![alt tag](https://cloud.githubusercontent.com/assets/15648358/25246214/df08a786-25fe-11e7-996a-8f79b81c544b.png)

Finally after step 10 the model is complete acording to finalised model.
![alt tag](https://cloud.githubusercontent.com/assets/15648358/25246009/3e553bc4-25fe-11e7-847d-8a77d546e211.png)
---


