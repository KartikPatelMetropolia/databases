# Metropolia Databases 1 (by Kartik Patel)

## Week 3

### Exercises 2 : Single Table Queries (with screenshots)

1. select * from goal;
2. select name from airport where iso_country="FI";
3. select name from airport where iso_country = "FI" order by name asc;
4. select name, type from airport where iso_country = "FI" order by type, name;
5. select DISTINCT name as name from country where iso_country like "F%";
6. select name from country where name like "%f%";
7. select location from game where screen_name="Vesa" LIMIT 1;
8. select co2_consumed from game where screen_name="ilkka";
9. select co2_budget from game LIMIT 1;
10. select screen_name,co2_budget,co2_consumed,@remaining_co2:=(co2_budget-co2_consumed) as remaining_co2 from game where screen_name="ilkka";

### Exercises 3 : Multiple Table Queries (with screenshots)

1. select country.name as "country name",airport.name as "airport name" from airport,country where country.name="Iceland" and airport.iso_country="IS";
2. select airport.name as "airport name" from airport,country where airport.type like "large%" and country.name="France" and country.iso_country=airport.iso_country;
3. select country.continent as "country_name",airport.name as "airport_name" from country, airport where country.continent="an" and airport.continent="an";
4. select airport.elevation_ft as "elevation_ft" from airport,game where game.screen_name="Heini" and game.location=airport.ident;
5. select airport.elevation_ft*0.3048 as "elevation_m" from airport,game where game.screen_name="Heini" and game.location=airport.ident;
6. select airport.name as "name" from airport,game where game.screen_name="ilkka" and game.location=airport.ident;
7. select country.name as "name" from airport,game,country where game.screen_name="ilkka" and game.location=airport.ident and airport.iso_country=country.iso_country;
8. select goal.name from goal,goal_reached,game where game.screen_name="Heini" and game.id=goal_reached.game_id and goal_reached.goal_id=goal.id;
9. select airport.name from airport inner join goal_reached inner join game on game.id = goal_reached.game_id inner join goal on goal.id = goal_reached.goal_id where game.screen_name = "Ilkka" and goal.name = "CLOUDS" and airport.ident = game.location;
10. select country.name from airport inner join goal_reached inner join country inner join game on game.id = goal_reached.game_id inner join goal on goal.id = goal_reached.goal_id where game.screen_name = "Ilkka" and goal.name = "CLOUDS" and airport.ident = game.location and country.iso_country = airport.iso_country;

## Week 4

### Exercise 4: Join (with screenshots)

1.  SELECT country.name AS "country name", airport.name AS "airport name" FROM airport INNER JOIN country ON airport.iso_country=country.iso_country WHERE country.name="Finland" AND airport.scheduled_service="yes";
2.  SELECT game.screen_name,airport.name AS "name" FROM airport INNER JOIN game ON game.location=airport.ident;
3.  SELECT game.screen_name,country.name AS "name" FROM airport INNER JOIN game ON game.location=airport.ident INNER JOIN country ON country.iso_country=airport.iso_country;
4.  SELECT airport.name AS "name",game.screen_name FROM airport left JOIN game ON game.location=airport.ident WHERE airport.name LIKE "%Hels%";
5.  SELECT goal.name AS "name", game.screen_name FROM goal LEFT JOIN goal_reached ON goal_reached.goal_id=goal.id LEFT JOIN game ON goal_reached.game_id=game.id;

### Exercise 5: Sub Queries (without screenshots: Passed in Moddle)

1.  SELECT country.name AS "name" FROM country WHERE iso_country IN (SELECT airport.iso_country FROM airport WHERE airport.name LIKE "Satsuma%");
2.  SELECT name FROM airport WHERE iso_country IN (SELECT iso_country FROM country WHERE country.name="Monaco");
3.  SELECT screen_name FROM game WHERE game.id IN ( SELECT goal_reached.game_id FROM goal_reached WHERE goal_reached.goal_id IN ( SELECT goal.id FROM goal WHERE goal.name="CLOUDS"));
4.  SELECT NAME FROM country WHERE country.iso_country NOT IN ( SELECT airport.iso_country FROM airport);
5.  SELECT goal.name FROM goal WHERE goal.id NOT IN (SELECT goal.id FROM goal_reached,goal,game WHERE game.id=goal_reached.game_id AND game.screen_name="Heini" AND goal.id=goal_reached.goal_id);

## Week 5

### Exercise 6: Aggregate queries(with screenshots)

1.  SELECT elevation_ft AS "max(elevation_ft)" FROM airport WHERE airport.elevation_ft IN (SELECT max(airport.elevation_ft) FROM airport);
2.  select continent, count(*) from country group by continent;
3.  select screen_name from game where co2_consumed in (select min(co2_consumed) from game);
4.  select screen_name from game where co2_consumed in (select min(co2_consumed) from game);
5.  select country.name,count(*) from airport inner join country on airport.iso_country = country.iso_country group by country.iso_country order by count(*) desc;
6.  SELECT DISTINCT country.name AS "name" FROM country,airport WHERE airport.iso_country=country.iso_country GROUP BY country.name HAVING COUNT(*) > 1000;
7.  SELECT NAME FROM airport WHERE airport.elevation_ft IN (SELECT MAX(airport.elevation_ft) FROM airport);
8.  SELECT country.name FROM airport,country WHERE airport.elevation_ft IN (SELECT MAX(airport.elevation_ft) FROM airport) AND country.iso_country=airport.iso_country;
9.  SELECT count(*) FROM goal_reached,game WHERE game.screen_name="Vesa" AND goal_reached.game_id=game.id;
10. SELECT name FROM airport ORDER BY latitude_deg ASC LIMIT 1;

### Exercise 7: Update Queries (without screenshots:Passed in Moddle)

1.  update game set co2_consumed = (select co2_consumed from game where screen_name="Vesa") + 500, location = (select airport.ident from airport where airport.name like "Nottingham%") where screen_name="Vesa";
2.  b.goal_reached
3.  delete from goal_reached;
4.  delete from game;