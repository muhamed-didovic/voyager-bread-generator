
## Voyager BREAD generator

There is a common issue when we try to deploy local projects to a different environment. Currently, we need to export the database or so, in order to keep all the new BREADs structure across all the environments.

The only way to do that without having to create a database import each time is by creating the migrations, seeds, etc. for each bread.

This allows the developers to create new BREADs from the command line using Artisan.

## How to use:

This package tries to follow laravel conventions when dealing with classnames. It is recomended that you invoke the command utilising a singluar name for the resource. 

Utilising model names that end in 's' can cause issues as the generator relies on Laravel's singular/plural string methods.


### Create a new bread seeder
If you have an existing model / table called `list_items` with the corresponding model `App\ListItem`

```bash
### Create a seeder for an existing model / table
php artisan voyager:bread list_item

### Generates database/seeds/ListItemsBreadSeeder.php
```

You can also generate the model and migration files

```bash
## Creat a seeder along with a corresponding model and table migration
php artisan voyager:bread list_item --migration --model

### Generates seeder eg: database/seeds/ListItemsBreadSeeder.php
### Generates model class eg: app/ListItem.php
### Generates migration class eg {DATE/TIME}_create_list_items_table.php
```

If a seeder class already exists, you can supply the `--force` flag to overwrite the existing file. Any modifications to this file will be lost.

### Configure the bread seeder
This command will create a new BooksBreadSeeder file with the basic configuration for a new bread-seed, there you can add/edit all the bread fields. See DataRowsTableSeeder

The seeder starts with 3 example columns - edit, duplicate etc as you see fit. See the [voyager docs](https://voyager.readme.io/docs/bread) for reference.

Everything that can be created in the voyager admin can be created via these seeders, as most of the advanced voyager features translate into json in the 'details' of the input field.

### Running your seeders

In order to run a newly created seeder, you must regenerate your autoloader

```bash
composer dumpautoload
```

To run a seed for a particular class, run `db:seed` with the `--class` flag.

```bash
php artisan db:seed --class=ListItemsBreadSeeder
```

To run all seeds..

```bash
php artisan db:seed
```

If you have generated new menu items as part of your seeding process, you will need to regenerate permissions before you can view them in the admin.

```bash
php artisan db:seed --class=PermissionRoleTableSeeder
```

You can also do this manually from the admin panel

**Model / Migration considerations**


If you created new model and migration, don't forget to run the migration.

```bash
php artisan migrate
```

