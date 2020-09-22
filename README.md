# Solution to SQL Murdery Mystery
## Bootcamp 2020 - Bordeaux Ynov Campus

> There's been a Murder in SQL City! The SQL Murder Mystery is designed to be both a self-directed lesson to learn SQL concepts and commands and a fun game for experienced SQL users to solve an intriguing crime.

## Experienced SQL sleuths start here

> A crime has taken place and the detective needs your help. The detective gave you the crime scene report, but you somehow lost it. You vaguely remember that the crime was a ​murder​ that occurred sometime on ​Jan.15, 2018​ and that it took place in ​SQL City​. Start by retrieving the corresponding crime scene report from the police department’s database.

## Witnesses Repport

SQL COMMANDS :
```SQL
    SELECT description 
    FROM crime_scene_report
    WHERE LOWER(city)='sql city' 
    AND date = '20180115' AND type = 'murder';
```

> Response :
> Description: Security footage shows that there were 2 witnesses. 
> * The first witness lives at the last house on "Northwestern Dr". 
> * The second witness, named Annabel, lives somewhere on "Franklin Ave".

/**************************************************************************************/

## WITNESSES :

SQL COMMANDS -->Witness 1 :
```SQL
    SELECT * 
    FROM person
    WHERE address_street_name='Northwestern Dr'
    ORDER BY address_number DESC LIMIT 1;
```

Response :
|id	    |name	        |license_id  |address_number  |address_street_name     |ssn      ||------:|:--------------|:-----------|:------|:-------|:-----------------------|:--------||14887  |Morty Schapiro |118009      |4919            |Northwestern Dr         |111564949|

SQL COMMANDS --> Witness 2 :
```SQL
SELECT * 
FROM person
WHERE address_street_name='Franklin Ave'
AND name LIKE '%Annabel%'
```

> Response :
id	    name            license_id  address_number  address_street_name     ssn
16371	Annabel Miller	490173	    103             Franklin Ave            318771143

/**************************************************************************************/

## Transcripts Witnesses

SQL COMMANDS --> Transcript witnesses :
```SQL
SELECT interview.* 
FROM interview 
WHERE person_id IN (14887,16371)
```

> Response :
| name      |Transcript    |
|----------:|:-------------|

name	            transcript
Morty Schapiro	    I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".
Annabel Miller	    I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

/**************************************************************************************/

## SUSPECTS : 

SQL COMMANDS --> Suspect 1 :
```SQL
SELECT *
FROM get_fit_now_member
INNER JOIN get_fit_now_check_in ON membership_id=get_fit_now_member.id
INNER JOIN person ON person_id=person.id
INNER JOIN drivers_license ON drivers_license.id=person.license_id
WHERE check_in_date=20180109 
AND get_fit_now_member.id LIKE "48Z%"
AND drivers_license.plate_number LIKE "%H42W%"
```

> Response :
id	    person_id   name            membership_start_date	membership_status	membership_id	check_in_date	check_in_time	check_out_time	id	        name	        license_id	    address_number	address_street_name	    ssn	        id	    age	height	eye_color	hair_color	gender	plate_number	car_make	car_model
48Z55	67318	    Jeremy Bowers	20160101	            gold	            48Z55           20180109        1530            1700            67318	    Jeremy Bowers	423327	        530	            Washington Pl, Apt 3A	871539279	423327	30	70	    brown	    brown	    male	0H42W2	        Chevrolet	Spark LS

/**************************************************************************************/

## Who is the murder ?
SQL COMMANDS --> Check the solution 
```SQL
INSERT INTO solution VALUES (1, 'Jeremy Bowers');
    SELECT value FROM solution;
```

> Response :

value
Congrats, you found the murderer! But wait, there's more... 
If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villain behind this crime. 

If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries. 
Use this same INSERT statement with your new suspect to check your answer.

/**************************************************************************************/

## Interview Suspect 1
SQL COMMANDS --> interview Suspect 1
```SQL
SELECT interview.* 
FROM interview 
WHERE person_id = 67318
```

> Response :

person_id	    transcript
67318	        I was hired by a woman with a lot of money. 
                I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). 
                She has red hair and she drives a Tesla Model S. 
                I know that she attended the SQL Symphony Concert 3 times in December 2017.

/**************************************************************************************/

## Suspect 2 

SQL COMMANDS --> Suspect 2 :
```SQL
SELECT * 
FROM drivers_license
JOIN person ON drivers_license.id=person.license_id
JOIN facebook_event_checkin ON person.id=facebook_event_checkin.person_id
WHERE drivers_license.car_model="Model S" 
AND drivers_license.hair_color="red" 
AND drivers_license.height  
BETWEEN 65 AND 67 
AND facebook_event_checkin.event_name="SQL Symphony Concert"
```

> Response :
id	    age	height	eye_color	hair_color	gender	plate_number	car_make	car_model	id      name	            license_id	address_number	address_street_name	    ssn	        person_id	event_id	event_name	            date
202298	68	66	    green	    red	        female	500123	        Tesla	    Model S	    99716	Miranda Priestly	202298	    1883	        Golden Ave	            987756388	99716	    1143	    SQL Symphony Concert	20171206


/**************************************************************************************/

## Interview Suspect 2

SQL COMMANDS --> interview Suspect 1
```SQL
SELECT interview.* 
FROM interview 
WHERE person_id=99716
```

> Response :
No data returned

/**************************************************************************************/

## Who is the murder ?
SQL COMMANDS --> Check the solution 
```SQL
INSERT INTO solution VALUES (1, 'Miranda Priestly');
        SELECT value FROM solution;
```

> Response :
value
Congrats, you found the brains behind the murder! 
Everyone in SQL City hails you as the greatest SQL detective of all time. 
Time to break out the champagne!

/**************************************************************************************/
