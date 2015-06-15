# Guide to Solving and Reviewing SQL Bear organizer

## spec/create_spec.rb
```ruby
creating bears table
  creates a bears table with a name field (FAILED - 1)

Failures:

  1) creating bears table creates a bears table with a name field
     Failure/Error: expect{@db.execute("SELECT name FROM bears;")}.to_not raise_exception
       expected no Exception, got #<SQLite3::SQLException: no such table: bears> with backtrace
```
For the first test to pass we need to create a new bears table with the `name` attribute. But for the sake of time lets add all the attributes from the `README.md` (`id`, `name`, `age`, `gender`, `color`, `temperament` and `alive`).

```ruby
CREATE TABLE bears (
  id INTEGER PRIMARY KEY,
  name TEXT,
  age INTEGER,
  gender CHAR(1),
  color TEXT,
  temperament TEXT,
  alive BOOLEAN
);
```
This SQL-Statement should make all the tests in the `create_spec.rb` pass.

## spec/insert_spec.rb

```ruby
populating the bears table
  has 8 bears (FAILED - 1)

Failures:

  1) populating the bears table has 8 bears
     Failure/Error: expect(@db.execute("SELECT COUNT(*) FROM bears;").flatten[0]).to eq(8)
       
       expected: 8
            got: 0
       
       (compared using ==)

```
This spec is just testing if you have 8 bears in your table. It is not validating the actual data you insert to the database except for the bear with no name.

```ruby
INSERT INTO bears VALUES (1,"Mr. Chocolate", 20, "M", "dark brown", "calm", 1);
INSERT INTO bears VALUES (2,"Rowdy", 10, "M", "black", "intense", 1);
INSERT INTO bears VALUES (3,"Tabitha", 6, "F", "dark brown", "Nice", 0);
INSERT INTO bears VALUES (4,"Seargeant Brown", 19, "M", "Green", "Slimy", 1);
INSERT INTO bears VALUES (5,"Melissa", 13, "F", "dark brown", "goofy", 1);
INSERT INTO bears VALUES (6,"Grinch", 2, "M", "Black", "Grinchy", 1);
INSERT INTO bears VALUES (7,"Wendy", 12, "F", "Blue", "naive", 0);
INSERT INTO bears VALUES (8,null, 20, "M", "black", "aggressive", 0);
```
## spec/insert_spec.rb
First error message for the `insert_spec.rb` is:

```ruby
querying the bears table
  selects all of the female bears and returns their name and age (FAILED - 1)

Failures:

  1) querying the bears table selects all of the female bears and returns their name and age
     Failure/Error: expect(@db.execute('')).to eq([["Tabitha", 6],["Melissa", 13], ["Wendy", 6]])
```

Enter following SQL-Query in your `selects_all_female_bears_return_name_and_age` method to select all of the female bears and returns their name and age

```ruby
"SELECT bears.name, bears.age FROM bears WHERE gender='F';"
```


The next error message is:

```ruby
querying the bears table
  selects all of the female bears and returns their name and age
  selects all of the bears names and orders them in alphabetical order (FAILED - 1)

Failures:

  1) querying the bears table selects all of the bears names and orders them in alphabetical order
     Failure/Error: expect(@db.execute(selects_all_bears_names_and_orders_in_alphabetical_order )).to eq([[nil],["Grinch"],["Melissa"],["Mr. Chocolate"],["Rowdy"],["Sargeant Brown"],["Tabitha"],["Wendy"]])
```

Enter following SQL-Query in your `selects_all_bears_names_and_orders_in_alphabetical_order` method to select all the bears names and to order them in alphabetical order

```ruby
"SELECT bears.name FROM bears ORDER BY bears.name ASC;"
```

The third error message is:

```ruby
querying the bears table
  selects all of the female bears and returns their name and age
  selects all of the bears names and orders them in alphabetical order
  selects all of the bears names and ages that are alive and order them from youngest to oldest (FAILED - 1)

Failures:

  1) querying the bears table selects all of the bears names and ages that are alive and order them from youngest to oldest
     Failure/Error: expect(@db.execute(selects_all_bears_names_and_ages_that_are_alive_and_order_youngest_to_oldest)).to eq([["Grinch", 2], ["Tabitha", 6], ["Wendy", 6], ["Rowdy", 10], ["Melissa", 13]])
```
Enter following SQL-Query in your `selects_all_bears_names_and_ages_that_are_alive_and_order_youngest_to_oldest` method to select all the names and the ages of bears that are still alive and order them from youngest to oldest

```ruby
"SELECT bears.name, bears.age FROM bears WHERE alive='TRUE' ORDER BY bears.age ASC;"
```

Your next test should be:

```ruby
querying the bears table
  selects all of the female bears and returns their name and age
  selects all of the bears names and orders them in alphabetical order
  selects all of the bears names and ages that are alive and order them from youngest to oldest
  selects the oldest bear and returns their name and age (FAILED - 1)

Failures:

  1) querying the bears table selects the oldest bear and returns their name and age
     Failure/Error: expect(@db.execute(selects_oldest_bear_and_returns_name_and_age )).to eq([["Mr. Chocolate", 20]])
```

Enter following SQL-Query in your `selects_oldest_bear_and_returns_name_and_age` method to select the oldest bear and return it's name and age

```ruby
"SELECT bears.name, bears.age FROM bears ORDER BY bears.age DESC LIMIT 1;"
```

The next error message is:

```ruby
querying the bears table
  selects all of the female bears and returns their name and age
  selects all of the bears names and orders them in alphabetical order
  selects all of the bears names and ages that are alive and order them from youngest to oldest
  selects the oldest bear and returns their name and age
  selects the youngest bear and returns their name and age (FAILED - 1)

Failures:

  1) querying the bears table selects the youngest bear and returns their name and age
     Failure/Error: expect(@db.execute(select_youngest_bear_and_returns_name_and_age )).to eq([["Grinch", 2]])
```

Enter following SQL-Query in your `select_youngest_bear_and_returns_name_and_age` method to select the youngest bear and returns it's name and age

```ruby
"SELECT bears.name, bears.age FROM bears ORDER BY bears.age ASC LIMIT 1;"
```

Next test:

```ruby
querying the bears table
  selects all of the female bears and returns their name and age
  selects all of the bears names and orders them in alphabetical order
  selects all of the bears names and ages that are alive and order them from youngest to oldest
  selects the oldest bear and returns their name and age
  selects the youngest bear and returns their name and age
  selects the most prominent color and returns it with its count (FAILED - 1)
  Failures:

  1) querying the bears table selects the most prominent color and returns it with its count
     Failure/Error: expect(@db.execute(selects_most_prominent_color_and_returns_with_count)).to eq([["dark brown", 3]])
```

Enter following SQL-Query in your `selects_most_prominent_color_and_returns_with_count` method to select the most prominent color and to return it with its count

```ruby
"SELECT bears.color, COUNT(bears.color) FROM bears GROUP BY bears.color ORDER BY COUNT(*) DESC LIMIT 1;"
```

```ruby
querying the bears table
  selects all of the female bears and returns their name and age
  selects all of the bears names and orders them in alphabetical order
  selects all of the bears names and ages that are alive and order them from youngest to oldest
  selects the oldest bear and returns their name and age
  selects the youngest bear and returns their name and age
  selects the most prominent color and returns it with its count
  counts the number of bears with goofy temperments (FAILED - 1)

Failures:

  1) querying the bears table counts the number of bears with goofy temperments
     Failure/Error: expect(@db.execute(counts_number_of_bears_with_goofy_temperments )).to eq([[1]])
```

Enter following SQL-Query in your `counts_number_of_bears_with_goofy_temperments` method to count the number of bears with goofy temperments

```ruby
"SELECT COUNT(bears.temperament) FROM bears WHERE bears.temperament='goofy';"
```
Your last error in this lab is:

```ruby
querying the bears table
  selects all of the female bears and returns their name and age
  selects all of the bears names and orders them in alphabetical order
  selects all of the bears names and ages that are alive and order them from youngest to oldest
  selects the oldest bear and returns their name and age
  selects the youngest bear and returns their name and age
  selects the most prominent color and returns it with its count
  counts the number of bears with goofy temperments
  selects the bear that killed Tim (FAILED - 1)

Failures:

  1) querying the bears table selects the bear that killed Tim
     Failure/Error: expect(@db.execute(selects_bear_that_killed_Tim )).to eq([[8, nil, 20, "M", "black", "aggressive", "FALSE"]])
```

Enter following SQL-Query in your `selects_bear_that_killed_Tim` method to select the bear that killed Tim
```ruby
"SELECT * FROM bears WHERE bears.name IS NULL;"
```
