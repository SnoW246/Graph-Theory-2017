# Graph-Theory-2017
Graph Theory Timetable Project 

## Create Commands for nodes in the Software Development course of 2016/2017 school year
// Create Institute node
CREATE (i:Institute {name:"GMIT"})

// Create Department node
CREATE (dep:Department {name:"Computer Science & Applied Physics"})

// Create Course node
CREATE (c:course {name:"Computing in Software Development"})

// Create Year nodes
CREATE (y1:year {name:"Year-1"}),
 (y2:year{name:"Year-2"}), 
 (y3:year{name:"Year-3"}), 
 (y4:year{name:"Year-4"})

// Create Semester nodes
CREATE (s1:semester {name:"S-1"}),
 (s2:semester {name:"S-2"}),
 (s3:semester {name:"S-3"}),
 (s4:semester {name:"S-4"}),
 (s5:semester {name:"S-5"}),
 (s6:semester {name:"S-6"}),
 (s7:semester {name:"S-7"}),
 (s8:semester {name:"S-8"})

// Create Module nodes for semester 5 & 6 of year 3
CREATE (m1:module {name:"Mobile Applications Development 2"}),
 (m2:module {name:"Database Management"}),
 (m3:module {name:"Server Side Rad"}),
 (m4:module {name:"Graph Theory"}),
 (m5:module {name:"Software Testing"}),
 (m6:module {name:"Profesional Practice in IT"}),
 (m7:module {name:"Graphics Programming"}),
 (m8:module {name:"Data Centric Rad"}),
 (m9:module {name:"Data Representation and Querying"}),
 (m10:module {name:"Object Oriented Programming"}),
 (m11:module {name:"Software Quality Management"}),
 (m12:module {name:"Operating Systems 1"})

// Create Lecturer nodes
CREATE (l1:lecturer {name:"Deirdre O'Donovan"}),
 (l2:lecturer {name:"Gerard Harrison"}),
 (l3:lecturer {name:"Martin Hynes"}),
 (l4:lecturer {name:"Ian McLoughlin"}),
 (l5:lecturer {name:"Damien Costello"}),
 (l6:lecturer {name:"Brian McGinley"}),
 (l7:lecturer {name:"John Healy"}),
 (l8:lecturer {name:"Joseph McGinley"})

// Create Room nodes
CREATE (r1:room {name:"145"}),
 (r2:room {name:"162"}),
 (r3:room {name:"165"}),
 (r4:room {name:"208"}),
 (r5:room {name:"223"}),
 (r6:room {name:"379"}),
 (r7:room {name:"436 CR5"}),
 (r8:room {name:"470 Computing Practical Lab"}),
 (r9:room {name:"480 CR7"}),
 (r10:room {name:"481 CR4"}),
 (r11:room {name:"482 CR3"}),
 (r12:room {name:"483 CR2"}),
 (r13:room {name:"484 CR1"}),
 (r14:room {name:"837"}),
 (r15:room {name:"903"}),
 (r16:room {name:"938"}),
 (r17:room {name:"939"}),
 (r18:room {name:"940"}),
 (r19:room {name:"941"}),
 (r20:room {name:"994"}),
 (r21:room {name:"995"}),
 (r22:room {name:"996"}),
 (r23:room {name:"997"}),
 (r24:room {name:"1000"}),
 (r25:room {name:"PF05"}),
 (r26:room {name:"PF18"})

// Create Group nodes
CREATE (g1:group {name:"Group-A"}),
(g2:group {name:"Group-B"}),
(g3:group {name:"Group-C"}),
(g4:group {name:"Group-D"})

// Create Weekday nodes
CREATE (d1:day {name:"Monday"}),
 (d2:day {name:"Tuesday"}),
 (d3:day {name:"Wednesday"}),
 (d4:day {name:"Thursday"}),
 (d5:day {name:"Friday"})

// Create Timeframe nodes
CREATE (t1:timeframe {name:"8.00-9.00"}),
 (t2:timeframe {name:"9.00-10.00"}),
 (t3:timeframe {name:"10.00-11.00"}),
 (t4:timeframe {name:"11.00-12.00"}),
 (t5:timeframe {name:"12.00-13.00"}),
 (t6:timeframe {name:"13.00-14.00"}),
 (t7:timeframe {name:"14.00-15.00"}),
 (t8:timeframe {name:"15.00-16.00"}),
 (t9:timeframe {name:"16.00-17.00"}),
 (t10:timeframe {name:"17.00-18.00"}),
 (t11:timeframe {name:"18.00-19.00"}),
 (t12:timeframe {name:"19.00-20.00"}),
 (t13:timeframe {name:"20.00-21.00"}),
 (t14:timeframe {name:"21.00-22.00"})
_______________________________________________________________________

MATCH (c: course), (y:year)
CREATE (c)-[r:HAS]->(y)

MATCH (a:Person),(b:Person)
WHERE a.name = 'Node A' AND b.name = 'Node B'
CREATE (a)-[r:RELTYPE]->(b)
RETURN r

// Create Relationship between Institute and Department
MATCH (i:Institute), (dep:Department)
CREATE (i)-[r:HAS]->(dep)

// Create Relationship between Department and Course
MATCH (dep:Department), (c:course)
CREATE (dep)-[r:HAS]->(c)

// Create Relationship between Course and Course Year
MATCH (c:course), (y:year)
CREATE (dep)-[r:HAS]->(c)

// Create Relationspip between Year and appropriate Semesters
MATCH (y:year),(s:semester)
WHERE y.name = "Year-1" AND s.name = "S-1"
CREATE (y)-[r:HAS]->(s)

MATCH (y:year),(s:semester)
WHERE y.name = "Year-1" AND s.name = "S-2"
CREATE (y)-[r:HAS]->(s)

MATCH (y:year),(s:semester)
WHERE y.name = "Year-2" AND s.name = "S-3"
CREATE (y)-[r:HAS]->(s)

MATCH (y:year),(s:semester)
WHERE y.name = "Year-2" AND s.name = "S-4"
CREATE (y)-[r:HAS]->(s)

MATCH (y:year),(s:semester)
WHERE y.name = "Year-3" AND s.name = "S-5"
CREATE (y)-[r:HAS]->(s)

MATCH (y:year),(s:semester)
WHERE y.name = "Year-3" AND s.name = "S-6"
CREATE (y)-[r:HAS]->(s)

MATCH (y:year),(s:semester)
WHERE y.name = "Year-4" AND s.name = "S-7"
CREATE (y)-[r:HAS]->(s)

MATCH (y:year),(s:semester)
WHERE y.name = "Year-4" AND s.name = "S-8"
CREATE (y)-[r:HAS]->(s)

// Create Relationship between Semesters and appropriate Modules
MATCH (s:semester), (m:module)
WHERE s.name = "S-5" AND m.name = "Graphics Programming"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-5" AND m.name = "Data Centric Rad"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-5" AND m.name = "Data Representation and Querying"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-5" AND m.name = "Object Oriented Programming"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-5" AND m.name = "Software Quality Management"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-5" AND m.name = "Operating Systems 1"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-6" AND m.name = "Mobile Applications Development 2"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-6" AND m.name = "Database Management"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-6" AND m.name = "Server Side Rad"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-6" AND m.name = "Graph Theory"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-6" AND m.name = "Software Testing"
CREATE (s)-[r:HAS]->(m)

MATCH (s:semester), (m:module)
WHERE s.name = "S-6" AND m.name = "Profesional Practice in IT"
CREATE (s)-[r:HAS]->(m)

// Create Relationship between Lecturer and the Module
MATCH (l:lecturer), (m:module)
WHERE l.name = "Damien Costello" AND m.name = "Mobile Applications Development 2"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Deirdre O'Donovan" AND m.name = "Database Management"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Patrick Mannion" AND m.name = "Database Management"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Gerard Harrison" AND m.name = "Server Side Rad"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Ian McLoughlin" AND m.name = "Graph Theory"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Martin Hynes" AND m.name = "Software Testing"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Damien Costello" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Daniel Cregg" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Gerard Harrison" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "John Healy" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Martin Hynes" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Martin Kenirons" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Patrick Mannion" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Brian McGinley" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Ian McLoughlin" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Kevin O'Brien" AND m.name = "Profesional Practice in IT"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Brian McGinley" AND m.name = "Graphics Programming"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Gerard Harrison" AND m.name = "Data Centric Rad"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Ian McLoughlin" AND m.name = "Data Representation and Querying"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "John Healy" AND m.name = "Object Oriented Programming"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Joseph McGinley" AND m.name = "Software Quality Management"
CREATE (l)-[r:TEACH]->(m)

MATCH (l:lecturer), (m:module)
WHERE l.name = "Martin Hynes" AND m.name = "Operating Systems 1"
CREATE (l)-[r:TEACH]->(m)

// Create Relationship between Day and different Timeframes
MATCH (d:day),(t:timeframe)
CREATE (d)-[r:AT]->(t)

// Create Relationship between Group and Module
MATCH (g:group), (m:module)
CREATE (g)-[r:HAS]->(m)

// Create Relationship between Room and all Days
MATCH (r:room), (d:day)
CREATE (r)-[rel:ON]->(d)

// Create Relationship between Module and Room
MATCH (m:module), (r:room)
WHERE m.name = "Database Management" AND r.name = "994"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Mobile Applications Development 2" AND r.name = "223"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Graph Theory" AND r.name = "PF05"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Software Testing" AND r.name = "481 CR4"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Software Testing" AND r.name = "436 CR5"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Database Management" AND r.name = "481 CR4"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Database Management" AND r.name = "482 CR3"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Mobile Applications Development 2" AND r.name = "470"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Graph Theory" AND r.name = "379"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Graph Theory" AND r.name = "162"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Graph Theory" AND r.name = "938"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Profesional Practice in IT" AND r.name = "208"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Server Side Rad" AND r.name = "997"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Software Testing" AND r.name = "939"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Database Management" AND r.name = "995"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Mobile Applications Development 2" AND r.name = "PF18"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Server Side Rad" AND r.name = "436 CR5"
CREATE (m)-[rel:IN]->(r)

MATCH (m:module), (r:room)
WHERE m.name = "Graph Theory" AND r.name = "208"
CREATE (m)-[rel:IN]->(r)
