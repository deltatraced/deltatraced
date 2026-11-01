---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[005 Model coin_store_hist and related in diesel]]'
context_type: task
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [005 Model coin_store_hist and related in diesel](005%20Model%20coin_store_hist%20and%20related%20in%20diesel.md)

Spawned in: [^spawn-task-d31c69](005%20Model%20coin_store_hist%20and%20related%20in%20diesel.md#spawn-task-d31c69)

# 1 Journal

2025-10-20 Wk 43 Mon - 22:27 +03:00

One option is to make a new type work with diesel by being auto-convertible to its types. We've already used an instance of this with [gh adwhit/diesel-derive-enum](https://github.com/adwhit/diesel-derive-enum)

See specifically [fn generate_new_diesel_mapping](https://github.com/adwhit/diesel-derive-enum/blob/816ebe062a99056a69a194b4ba15532980558c19/src/lib.rs#L407), [fn generate_common_impls](https://github.com/adwhit/diesel-derive-enum/blob/816ebe062a99056a69a194b4ba15532980558c19/src/lib.rs#L422), and [fn generate_sqlite_impl](https://github.com/adwhit/diesel-derive-enum/blob/816ebe062a99056a69a194b4ba15532980558c19/src/lib.rs#L570)

They make use of [FromSql](https://docs.diesel.rs/2.3.x/diesel/deserialize/trait.FromSql.html), [ToSql](https://docs.diesel.rs/2.3.x/diesel/serialize/trait.ToSql.html), [Queryable](https://docs.diesel.rs/2.3.x/diesel/deserialize/trait.Queryable.html), [AsExpression](https://docs.diesel.rs/2.3.x/diesel/expression/trait.AsExpression.html).

2025-10-20 Wk 43 Mon - 23:13 +03:00

The main error we get when we replace `String` and `&'a str` for `Person` and `&'a Person` in `create_diesel_hist_structs` macros is

````rust
the trait bound `Person: FromSqlRow<diesel::sql_types::Text, Sqlite>` is not satisfied  
double check your type mappings via the documentation of `diesel::sql_types::Text`  
`diesel::sql_query` requires the loading target to column names for loading values.  
You need to provide a type that explicitly derives `diesel::deserialize::QueryableByName`
````

2025-10-20 Wk 43 Mon - 23:35 +03:00

For the implementation of `FromSql` and `ToSql`,

````rust
use diesel::{backend::Backend, deserialize::FromSql, serialize::ToSql};
use thiserror::Error;

#[derive(Debug, Clone)]
pub struct Person {
    pub s: String,
}

#[derive(Error, Debug)]
pub enum PersonNewError {
    #[error("Person name cannot be admin")]
    AdminNotAllowed,
}

impl Person {
    pub fn new(s: &str) -> Result<Self, PersonNewError> {
        if s.to_lowercase() == "admin" {
            Err(PersonNewError::AdminNotAllowed)
        } else {
            Ok( Person {s: s.to_owned() })
        }
    }
}

impl<DB> FromSql<String, DB> for Person 
where 
    DB: Backend,
    String: FromSql<String, DB>,
{
    fn from_sql(bytes: DB::RawValue<'_>) -> diesel::deserialize::Result<Self> {
        let s = String::from_sql(bytes)?;
        Ok(Person::new(&s)?)
    }
}

impl<DB> ToSql<String, DB> for Person 
where 
    DB: Backend,
    String: ToSql<String, DB>,
{
    fn to_sql<'b>(&'b self, out: &mut diesel::serialize::Output<'b, '_, DB>) -> diesel::serialize::Result {
        self.s.to_sql(out)
    }
}
````

Note that extra constraints on `String` were needed for use of `String::from_sql` and `self.s.to_sql`.

2025-10-20 Wk 43 Mon - 23:37 +03:00

The diesel error persists about [FromSqlRow](https://docs.diesel.rs/2.3.x/diesel/deserialize/trait.FromSqlRow.html). There's also this error:

````
the trait bound `Person: diesel::Expression` is not satisfied
````

2025-10-21 Wk 43 Tue - 01:01 +03:00

Let's try [docs.rs diesel_derive_newtype](https://docs.rs/diesel-derive-newtype/latest/diesel_derive_newtype/)

2025-10-21 Wk 43 Tue - 01:08 +03:00

This works

````rust

#[derive(Debug, Clone, Hash, PartialEq, Eq, DieselNewType)]
pub struct Person(String);

#[derive(Error, Debug~~, AsExpression~~)]
~~#[diesel(sql_type = diesel::sql_types::Text)]~~
pub enum PersonNewError {
    #[error("Person name cannot be admin")]
    AdminNotAllowed,
}

impl Person {
    pub fn new(s: &str) -> Result<Self, PersonNewError> {
        if s.to_lowercase() == "admin" {
            Err(PersonNewError::AdminNotAllowed)
        } else {
            Ok( Person(s.to_owned()))
        }
    }
}
````

2025-10-21 Wk 43 Tue - 16:42 +03:00

There shouldn't be `AsExpression` or `sql_type` on `PersonNewError`, this was meant to go on `Person` as I was experimenting with creating a new type, but it is unnecessary now. The `FromSqlRow` unsatisfied trait error still persists when correcting it in the attempt.

Correction:

````rust

#[derive(Debug, Clone, Hash, PartialEq, Eq, DieselNewType)]
pub struct Person(String);

#[derive(Error, Debug)]
pub enum PersonNewError {
    #[error("Person name cannot be admin")]
    AdminNotAllowed,
}

impl Person {
    pub fn new(s: &str) -> Result<Self, PersonNewError> {
        if s.to_lowercase() == "admin" {
            Err(PersonNewError::AdminNotAllowed)
        } else {
            Ok( Person(s.to_owned()))
        }
    }
}
````
