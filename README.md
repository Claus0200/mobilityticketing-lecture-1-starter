# MobilityTicketing: Lecture 1 starter

Submitted commit: []()
Setup and reset instructions: [link](setup.md)

## Where to find the work
Lecture 1: model, workload map and queries: [link](https://github.com/Claus0200/mobilityticketing-lecture-1-starter)
Lecture 2: constraints and tests: [link](https://github.com/Claus0200/mobilityticketing-lecture-2-starter)
Lecture 3: reporting experiment and comparison: [link](https://github.com/Claus0200/mobilityticketing-lecture-3-starter)
Lecture 4: migration stages and verification: [link](https://github.com/Claus0200/mobilityticketing-lecture-4-starter)

## Two decisions worth discussing
For each: What did we choose? What was the alternative?
Why does our choice fit MobilityTicketing? Which file or result supports it?

### Allow a stop to occur more than once on the same route

I chose to use (route_id, stop_sequence) as the primary key for route_stops, which means a stop may occur more than once on the same route as long as it has a different sequence number. The alternative was to use (route_id, stop_id) as the primary key, which would prevent the same stop from appearing more than once on a route.

The reason for this is that a route could potentially pass through the same stop more than once, so the stop's position in the route is what identifies its occurrence. This also allows the same stop to appear at different points in the ordered route.

The relevant evidence is the route_stops table [definition](database/postgres/init/001_relational_baseline.sql)

### Use a seperate route_stops table

Since there is a many-to-many relationship between routes and stops, having a separate table called route_stops is a good option. The alternative would be to store the route_id directly in the stops table.

The reason for this is that a route can contain many stops, and the same stop can be part of multiple routes. A separate route_stops table allows us to represent this relationship without duplicating stop information. It also allows us to store the stop_sequence, which tells us the order of the stops on a route.

The relevant evidence is the route_stops table [definition](database/postgres/init/001_relational_baseline.sql)

## One limitation or open question

The current model is based on the workload and scenarios given in the assignment, so we do not know if it would cover all possible route and timetable scenarios in a real MobilityTicketing system. This means that there could be changes further down the line.