*osx 10.10.1 | Jan 15, 2015*
### Quick reference to Rails Generators.


## [Migrations]
*What has been generated for you is just a starting point. You can add or remove from it as you see fit by editing the migration file.*

### Creating Tables
If the migration name is of the form "CreateXXX" and is followed by a list of column names and types then a migration creating the table XXX with the columns listed will be generated. For example:
`bin/rails generate migration CreateProducts name:string part_number:string`
generates:

~~~ruby
class CreateProducts < ActiveRecord::Migration
  def change
    create_table :products do |t|
      t.string :name
      t.string :part_number
    end
  end
end
~~~

###Changing / Modifing Tables

####Adding Columns
Generate an empty but corectly named migration file:  
`bin/rails generate migration AddPartNumberToProducts`

Include a list of column names and types:  
`bin/rails generate migration AddPartNumberToProducts part_number:string`

will generate:
```ruby
class AddPartNumberToProducts < ActiveRecord::Migration
  def change
    add_column :products, :part_number, :string
  end
end
```

Optionaly add an index on the new column, you can do that as well:
`bin/rails generate migration AddPartNumberToProducts part_number:string:index`
will generate:
```ruby
class AddPartNumberToProducts < ActiveRecord::Migration
  def change
    add_column :products, :part_number, :string
    add_index :products, :part_number
  end
end
```

####Removing Columns
Generate a migration for removing a column:
`bin/rails generate migration RemovePartNumberFromProducts part_number:string`
generates:
```ruby
class RemovePartNumberFromProducts < ActiveRecord::Migration
  def change
    remove_column :products, :part_number, :string
  end
end
```



##Models
Capitolize and Singular model name.

```ruby
bin/rails generate model Article title:string text:text
```

Now with an assosiation. Notice the __`article:references`__.

```ruby
bin/rails generate model Comment commenter:string body:text article:references
```

Generates:
  - Migration file (To create the corresponding database table.)
  - Model file (The Comment model with the `belongs_to :article` line.)
  - Corisponding Test file


##Controllers






[Migrations]: http://edgeguides.rubyonrails.org/active_record_migrations.html
