#🎯Analyzing Road Safety in the UK

#Business problem:
The UK Department of Transport provides open datasets on road safety and casualties, and one can
use these datasets to analyze how safe the roads in the UK are. This project will help you answer a
few questions using their 2015 dataset.
The dataset has 3 tables i.e Accident_2015, vehicle_2015, Vehicle_types

Approach/Project Idea:

Use aggregate functions in SQL and Python to answer the following sample questions:

#1. Evaluate the median severity value of accidents caused by various Motorcycles.

#2. Evaluate Accident Severity and Total Accidents per Vehicle Type

#3. Calculate the Average Severity by vehicle type.

#4. Calculate the Average Severity and Total Accidents by Motorcycle

Github source code link:
https://lnkd.in/dbEAKY2G

#Creating Database Road_Safety:
create database if not exists Road_Safety;
use Road_Safety;
#Creating Table Accident_2015:
create table if not exists Accident_2015(
Accident_Index varchar(200),
Location_Easting_OSGR double,
Location_Northing_OSGR double,
Longitude double,
Latitude double,
Police_Force int,
Accident_Severity int,
Number_of_Vehicles int,
Number_of_Casualties int,
`Date` varchar(200),
Day_of_Week int,
`Time` time,
`Local_Authority_(District)` int,
`Local_Authority_(Highway)` varchar(200),
1st_Road_Class int,
1st_Road_Number int,
Road_Type int,
Speed_limit int,
Junction_Detail int,
Junction_Control int,
2nd_Road_Class int,
2nd_Road_Number int,
`Pedestrian_Crossing-Human_Control` int,
`Pedestrian_Crossing-Physical_Facilities` int,
Light_Conditions int,
Weather_Conditions int,
Road_Surface_Conditions int,
Special_Conditions_at_Site int,
Carriageway_Hazards int,
Urban_or_Rural_Area int,
Did_Police_Officer_Attend_Scene_of_Accident int,
LSOA_of_Accident_Location varchar(200)
);

select * from accident_2015;
#Bulk Upload in Table Accident_2015 from Accident_2015.csv file:

Load Data infile
'E:/DataAnalyst Ineuron/SQL_SUDHANSHU/SQL_ASSIGNMENTS/Anand Jha Assignement/Accident_2015.csv'
into table accident_2015
Fields terminated by ','
enclosed by '"'
Lines terminated by '\n'
ignore 1 rows;

select COUNT(*) from accident_2015;
#Creating Table Vehicle_2015:

create table vehicle_2015(
Accident_Index varchar(200),
Vehicle_Reference  int,
Vehicle_Type  int,
Towing_and_Articulation  int,
Vehicle_Manoeuvre  int,
`Vehicle_Location-Restricted_Lane`  int,
Junction_Location  int,
Skidding_and_Overturning  int,
Hit_Object_in_Carriageway  int,
Vehicle_Leaving_Carriageway  int,
Hit_Object_off_Carriageway  int,
1st_Point_of_Impact  int,
Was_Vehicle_Left_Hand_Drive  int,
Journey_Purpose_of_Driver  int,
Sex_of_Driver  int,
Age_of_Driver  int,
Age_Band_of_Driver  int,
`Engine_Capacity_(CC)`  int,
Propulsion_Code  int,
Age_of_Vehicle  int,
Driver_IMD_Decile  int,
Driver_Home_Area_Type  int,
Vehicle_IMD_Decile int
);

select * from vehicle_2015;
#Bulk Upload in Table Vehicle_2015 from Vehicle_2015.csv file:

Load Data infile 
'E:/DataAnalyst Ineuron/SQL_SUDHANSHU/SQL_ASSIGNMENTS/Anand Jha Assignement/Vehicle_2015.csv'
into table vehicle_2015
Fields terminated by ','
enclosed by '"'
Lines terminated by '\n'
ignore 1 rows;

select count(*) from vehicle_2015;
#Creating Table Vehicle_types:

create table vehicle_types(
`code` double,
label varchar(200));
#Load Data in Vehicle_types from Vehicle_types.csv:

Load Data infile 
'E:/DataAnalyst Ineuron/SQL_SUDHANSHU/SQL_ASSIGNMENTS/Anand Jha Assignement/Vehicle_types.csv'
into table vehicle_types
Fields terminated by ','
enclosed by '"'
Lines terminated by '\n'
ignore 1 rows;

select * from vehicle_types;

#2. Evaluate the median severity value of accidents caused by various Motorcycles:
with answer1 as(select avg(Accident_Severity) as Average_Severity ,Vehicle_type   from accident_2015 inner join vehicle_2015 on accident_2015.accident_index=vehicle_2015.accident_index   group by vehicle_type  order by vehicle_type)  select Average_Severity,Vehicle_type,label as Vehicle_Type_Name from answer1  inner join vehicle_types on answer1.Vehicle_type=vehicle_types.`code`  where label like '%otorcycle%';
 
#2. Evaluate Accident Severity and Total Accidents  per Vehicle Type:
#(i)Accident Severity per Vehicle Type:
with answer2 as(select Accident_Severity,Vehicle_type from accident_2015 inner join vehicle_2015 on accident_2015.accident_index=vehicle_2015.accident_index  group by vehicle_type  order by vehicle_type) select Accident_Severity,Vehicle_type,label as Vehicle_Type_Name from answer2 inner join vehicle_types on answer2.Vehicle_type=vehicle_types.`code`;
 

#(ii)Total Accidents per Vehicle Type:
with answer2b as(select sum(Number_of_casualties) as Total_Accidents,Vehicle_type from accident_2015  inner join vehicle_2015 on accident_2015.accident_index=vehicle_2015.accident_index group by vehicle_type order by Total_Accidents) select Total_Accidents,Vehicle_type,label as Vehicle_Type_Name from answer2b  inner join vehicle_types on answer2b.Vehicle_type=vehicle_types.`code`;
 
#3. Calculate the Average Severity by Vehicle Type:
with answer3 as(select avg(Accident_Severity) as Average_Severity ,Vehicle_type  from accident_2015 inner join vehicle_2015 on accident_2015.accident_index=vehicle_2015.accident_index   group by vehicle_type  order by vehicle_type) select Average_Severity,Vehicle_type,label as Vehicle_Type_Name from answer3  inner join vehicle_types on answer3.Vehicle_type=vehicle_types.`code` order by Average_Severity;
 
#4. Calculate the Average Severity and Total Accidents  by Motorcycle:
#(i). Average Severity by Motorcycle:
with answer4b as(select accident_severity ,Vehicle_type from accident_2015  inner join vehicle_2015 on accident_2015.accident_index=vehicle_2015.accident_index  group by vehicle_type )  select avg(accident_severity) as Average_Severity_By_Motorcycle from answer4b   inner join vehicle_types on answer4b.Vehicle_type=vehicle_types.`code` where vehicle_type in  (2,3,4,5,23,97) ;
 
#(ii). Total Accidents by Motorcycle:
with answer2b as(select sum(Number_of_casualties) as Total_Accidents,Vehicle_type from accident_2015  inner join vehicle_2015 on accident_2015.accident_index=vehicle_2015.accident_index  group by vehicle_type order by Total_Accidents)  select sum(Total_Accidents) as Total_Accidents_By_Motorcycle from answer2b   inner join vehicle_types on answer2b.Vehicle_type=vehicle_types.`code` where vehicle_type in  (2,3,4,5,23,97) ;
 
