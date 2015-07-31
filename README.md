# node-metal

Metal is a JavaScript base class designed for your relational database.

The Metal suite consists of a base class, a cli for database interactions, and some testing utilities.

## Static methods (class methods)

Static methods return a new class instance, or an array of instances.

* `all`
* `find`
* `where`

Examples:

```
User.all()
		.then((rows) => map(rows, (user) => user.name)

User.find({ email: 'name@example.com' })
		.then((row) => row.name)

User.where({ location: 'Chicago', name: 'Gob' })
		.then((rows) => map(rows, (user) => user.name)
```

## Prototype methods (instance methods)

* `save`
* `destroy`
* `get`
* `set`
* `unset`
* `isNew`
* `toJSON`
* `isValid`

## Lifecycle callbacks

Callbacks should return a promise. If you throw or catch an error the subsequent method will not be called.

* `beforeValidation`
* `beforeSave`
* `beforeCreate`
* `afterValidation`
* `afterSave`
* `afterCreate`

Examples:

```
class User extends Metal {
	beforeValidate() {
		var phone = this.attributes.phone.replace('-', ' ');
		this.set({ phone: phone });
	}

	beforeSave() {
		return new Promise((resolve, reject) => {
			MailService.subscribe(this.attributes, (err, resp) => {
				if (err) reject(err);
				resolve(resp);
			});
		});
	}
}
```

## Associations

Associations are defined as a class property.

### hasMany

```
{
	name: 'Vehicle',
	model: Vehicle,
	foreignKey: 'user_id'
}
```

### belongsTo

```
{
	name: 'User',
	model: User,
	foreignKey: 'user_id'
}
```

### hasOne

```
{
	name: 'User',
	model: User,
	foreignKey: 'user_id'
}
```

# metal-cli

The metal-cli is useful for creating and dropping databases, creating and running migrations, and generating models based on migrations.

* `createdb`
* `dropdb`
* `migrate`
* `rollback`
* `model FIELDS` (migration and model generated)
* `migration FIELDS` (only migration generated, no model)

# db-cleaner

db-cleaner provides out of the box support for database cleaning strategies in your tests.

```
var dbCleaner = require('metal/utils/db-cleaner');
dbCleaner.setStrategy('truncation'); // or transaction, deletion
dbCleaner.clean();
```
