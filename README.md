
# Prequelize

sequelize with less "features".

## Why

Sequelize has a clunky, promise API, and the `where` and `include`
pattern is extremely dificult to follow, for no apparent reason.

Prequelize simplifies this significantly by not providing features
that almost never get used.


## Settings

the prequelize settings object is basically the same as the sequelize one,
but with a simplified where and include syntax:

```javascript
{
    // sequelize style settings...
    skip: 0,
    limit: 10,
    order: [['name', 'DESC']],

    // simplified where:
    where:{
        someField: 'x',
        relatedTable:{
            relatedField: 'y'
        }
    }

    // seperate, simple include:
    include:{

        // Either a list of included fields
        $fields: ['id', 'name', 'someField'],

        // Or keys, with a truthy value
        age: true,

        // With related tables nested.
        relatedTable: {
            $fields: ['relatedField']
        }
    }
}
```


## API

All prequelize model methods take a callback as the last parameter,
and return a `righto`.

If no callback is passed, the operation will not be run until
the returned righto is run.

see [righto](https://github.com/KoryNunn/righto) for more info.

example:

Using normal callbacks:

```
    // Get a person with a callback,
    // Executes immediately
    prequelize.Person.get(123, {
            include: { $fields: ['firstName']}
        }function(error, person){

    });
```

Using the returned righto:

```
    // Get a person righto, does not execute until used.
    var person = prequelize.Person.get(123, {
            include: { $fields: ['firstName']}
        });

    // Execute the query and get the resut.
    person(function(error, person){

    });
```


## Get.

Get exactly one result by ID.

If no results are found, the call will be rejected with an Error with code 404.

get(id, settings, callback)

```
prequelize.Person.get(
    123,
    {
        where:{
            enabled: true
        },
        include: {
            $fields: ['firstName', 'surname']
        }
    },
    callback
)
```


## Find.

Find the first result of a query.

If no results are found, the call will be resolved no result.

find(settings, callback)

## Find All.

Find all results of a query.

findAll(settings, callback)

## Find And Count All.

Find and count all results of a query.

findAndCountAll(settings, callback)

## Find one.

Find exactly one result of a query.

If no results are found, the call will be rejected with an Error with code 404.

If more than one result is found, the call will throw.

findOne(settings, callback)

## Remove.

Remove all results of a query.

findAndRemove(settings, callback)

## Remove One.

Remove exactly one result of a query.

If no results are found, the call will be rejected with an Error with code 404.

If more than one result is found, the call will throw.

findOneAndRemove(settings, callback)

## Remove.

Remove exactly one result by ID.

If no results are found, the call will be rejected with an Error with code 404.


remove(id, settings, callback)

## Create.

Create a record.

create(data, settings, callback)

## Update.

Update all results of a query.

findAndUpdate(data, settings, callback)

## Update One.

Update exactly one result of a query.

If no results are found, the call will be rejected with an Error with code 404.

If more than one result is found, the call will throw.

findOneAndUpdate(data, settings, callback)

## Update.

Update exactly one result by ID.

If no results are found, the call will be rejected with an Error with code 404.

update(id, data, settings, callback)
