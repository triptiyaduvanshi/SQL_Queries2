# SQL_Queries2
Hard leetcode queries with solution
<br>
1. show tables;
/1...* A group of travelers embark on world tours starting with their home cities. Each traveler 
has an undecided itinerary that evolves over the course of the tour. Some travelers decide to 
abruptly end their journey mid-travel and live in their last destination.
Given the dataset of dates on which they travelled between different pairs of cities, 
can you find out how many travellers ended back in their home city? For simplicity, 
you can assume that each traveler made at most one trip between two cities in a day.*/
<br>
describe employee_salary_data;
CREATE TABLE travel_history (date DATE, start_city VARCHAR(50), end_city VARCHAR(50), traveler VARCHAR(50));

INSERT INTO travel_history (date, start_city, end_city, traveler) VALUES ('2024-01-01', 'Delhi', 'Dubai', 'Amit'),
 ('2024-01-05', 'Dubai', 'London', 'Amit'), ('2024-01-10', 'London', 'Delhi', 'Amit'), 
 ('2024-02-01', 'Mumbai', 'Singapore', 'Priya'), ('2024-02-05', 'Singapore', 'Sydney', 'Priya'), 
 ('2024-02-10', 'Sydney', 'New York', 'Priya'), ('2024-03-01', 'Kolkata', 'Bangkok', 'Raj'), 
 ('2024-03-03', 'Bangkok', 'Tokyo', 'Raj'), ('2024-03-07', 'Tokyo', 'Kolkata', 'Raj'), 
 ('2024-04-01', 'Bangalore', 'Paris', 'Neha'), ('2024-04-05', 'Paris', 'Rome', 'Neha'),
 ('2024-04-10', 'Rome', 'Berlin', 'Neha'), ('2024-05-01', 'Chennai', 'Dubai', 'Arjun'), 
 ('2024-05-03', 'Dubai', 'Amsterdam', 'Arjun'), ('2024-05-06', 'Amsterdam', 'Chennai', 'Arjun'); 
<br>
<br>
  with base as (select *,row_number()over(partition by traveler order by date) ranks from travel_history),
 base2 as(select traveler, end_city as endcity from base where ranks=(select max(ranks) from base)
 group by traveler, end_city),
 base3 as (select traveler, start_city as startcity from base where ranks=(select min(ranks) from base)
 group by traveler, start_city)
 select b2.traveler, b3.startcity, b2.endcity from base2 b2 join base3 b3 on b2.traveler=b3.traveler
 where b3.startcity=b2.endcity;
