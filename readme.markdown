# Code Igniter Migrations & Active Record

To use this code download a copy of CodeIgniter and replace the application folder with the one in this repo. To run the migrations you must enter your MySQL database credentials into the database.php file.

### Creating Migrations

We'll talk about this at a later time.

## Running Migrations

Once you have connected CI to the database, migrations can be run by hitting the url /migrations. This is **not** for production projects. To create a migration goto the url /migrations/create/CreateProjectTable. CreateProjectTable can be replaced with any name you want for the migration. This does not generate the table code, you must do that yourself.

To edit migrations look at the applications/db in your CI project. There are examples in there.

# Active Record

This is an early port of ActiveRecord from Rails 2.x.

## Writing a model

	class Blogs_Model extends My_Model 
	{	
		function init()
		{
			$this->fields('rss_url', 'slug', 'name', 'description');
			$this->validates('name', array('presence' => TRUE, 'uniqueness' => TRUE));
			$this->validates('slug', array('presence' => TRUE, 'uniqueness' => TRUE));
			
			$this->before_save('force_slug');
		}
		
		function force_slug()
		{
			/* Force the slug into a format*/
		}
	}
	
The only requirements for a model is the fields must be defined. I am thinking about taking out the fields requirement. I do not see any valid reason why a model should no know this information based on how I built them.

## Validation

This class provides three validation methods with the option of creating more.

### Predefined Validation Methods

#### Presence
    $this->validates('name', array('presence' => TRUE));

The presence method validates whether a field is present or not.

#### Uniqueness
    $this->validates('name', array('uniqueness' => TRUE));

The uniqueness method validates whether the a row already exists with a value for a certain field.

    $this->validates('name', array('uniqueness' => array('scope' => array('section_id' => 1))));

If a scope is present, the uniqueness of a validation becomes more specific. In this case, the name will only validate if it does not exist in the section 1.

### Creating validation methods

This section will be filled in later.

#### Length
    $this->validates('name', array('length', array('maximum' => 10));

The length method validates whether the string value has a certain length. Arguments for the length are maximum and minimum. Both arguments are optional.

## Callbacks

A callback allows a model to extend the functionality of the methods provided by My_Model. Callbacks are stacked, this allows you to add many callbacks to keep the code clean.

#### Available Callbacks

- before_create($callback)
- after_create($callback)
- before_save($callback)
- after_save($callback)
- before_validation($callback)
- after_validation($callback)

		$this->before_create('change_slug');
		$this->before_create('check_something');
		$this->after_create('create_notificiation');
	
## Finder Methods

#### exists(conditions = NULL)

Checks against the conditions if a row exists.

#### counts(conditions = NULL)

Returns the number of rows that match the given conditions.

#### first(conditions = NULL)

Returns the first row that matches the given conditions.

#### last(conditions = NULL)

Returns the last row that matches the given conditions.

#### all($conditions = NULL)

Returns the all rows that match the given conditions.

#### find($conditions = array(), $page = 1, $limit = 25)

Returns the requested amount of rows based on the conditions.
