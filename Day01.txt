-- COMMENT LINE
/*
a
b
c
*/

-- ** DECLARING VARIABLES **
/*

	$$: To be able to write some special characters without interfering with anything.
	
	do: Anonymous block. Making the return type void.

*/
do $$
declare

	-- variable_name  data_type  :=  assigned_value
	-- Default Value: null
	counter integer := 1;
	first_name varchar(50) := 'Jace';
	last_name varchar(50) := 'ABC';
	payment numeric(4,2) := 20.5; -- 20.50
	
	/*
			numeric(4,2):
			Number can only contain 4 digits in total and 2 for the decimal part.
			
			The place where we write 4 is called (First Parameter): Presicion
			The place where we write 2 is called (Second Parameter): Scale
	*/
	

begin

	-- System.out.println(...);
	raise notice 'First Name: %, Last Name: %', first_name, last_name;

end $$;

-- TASK: Raise an output: Name LastName received 20.5 USD. --> Dynamic Values: Name, LastName, 20.5
do $$
declare

	first_name varchar(50) = 'Jace';
	last_name varchar(50) := 'ABC';
	payment numeric(4,1) := 20.5;

begin

	raise notice '% % received % USD.', first_name, last_name, payment;

end $$;

-- TASK 1: Name and DifferentName got tickets for 50 Dollars --> Dynamic Values: Name, DifferentName, 50
do $$
declare
	first_name varchar(50) := 'John';
	different_name varchar(50) := 'Mark';
	price integer := 50;
begin

	raise notice '% and % got tickets for % Dollars', first_name, different_name, price;
end $$;

-- ** SLEEP COMMAND **

do $$
declare

	created_at time := now(); -- Getting the current time

begin
	
	raise notice '%', created_at;
	perform pg_sleep(5); -- Wait for 5 sec
	raise notice '%', created_at;

end $$;

-- ** COPYING THE DATA TYPE FROM THE TABLE **

do $$
declare

	movie_title movie.title%type; -- movie_title text

begin

	-- Get the title of the movie with ID 1, pass it into movie_title
	SELECT title FROM movie WHERE id=1 INTO movie_title;
	
	raise notice 'Movie with ID 1: %', movie_title;
	
end $$;

-- ** COPYING THE WHOLE ROW TYPE **
do $$
declare
	
	selected_actor actor%rowtype; -- Store all the row information in one variable
	
begin

	-- Get the Actor with ID 3
	SELECT *
	FROM actor
	WHERE id=3
	INTO selected_actor;
	
	raise notice 'Actor: %', selected_actor;
	raise notice 'Actor Name: % %', selected_actor.first_name, selected_actor.last_name;

end $$;

-- ** COPYING THE Record Type **
-- If you don't want to get all the information, but some of them.
do $$
declare
	
	rec record; -- record is the data type
	
	--selected_movie movie%rowtype;
	
begin 

	-- Selecting the movie
	SELECT id, title, type
	FROM movie
	WHERE id = 1
	INTO rec;
	
	raise notice '% % %', rec.id, rec.title, rec.type;

end $$;

-- ** NESTED BLOCKS **

do $$
<<outer_block>>
declare -- Outer Block
	counter integer := 0;
begin

	counter := counter + 1;
	raise notice 'Counter: %', counter;
	
	declare -- Sub Block
		counter integer := 0;
	begin
	
		counter := counter + 10;
		raise notice 'Sub Block Counter: %', counter;
		raise notice 'While inside the SubBlock, OuterBlocks counter: %', outer_block.counter;
	
	end; -- End of the Sub Block

end $$; -- End of the Outer Block


-- ** CONSTANT **
--	   final

/*
		--selling_price := 100 + 100 * 0.1;
		--selling_price := price + price * vat;
*/
do $$
declare
	
	vat constant numeric := 0.1;
	price numeric := 20.5;
	
begin

	raise notice 'Base Price: %', price;
	raise notice 'Selling Price: %', price*(1+vat);
	
	--vat := 0.2;

end $$;


-- Control Structures

-- ** IF Statements **
-- syntax:
/*
	if condition then
		...
	end if;
	------------------
	if x<y {
	
	};
	------------------
	if not found then

	end if;
*/

-- Task 1: Find the movie with ID: 10, and if not found, print a message says it's not found.
-- not found
do $$
declare

	selected_movie movie%rowtype;
	
	input_for_movie_id integer := 10;

begin

	SELECT * FROM movie
	WHERE id = input_for_movie_id -- 10
	INTO selected_movie;
	
	if not found then
		raise warning 'Movie not found with id: %', input_for_movie_id;
	end if;

end $$;












