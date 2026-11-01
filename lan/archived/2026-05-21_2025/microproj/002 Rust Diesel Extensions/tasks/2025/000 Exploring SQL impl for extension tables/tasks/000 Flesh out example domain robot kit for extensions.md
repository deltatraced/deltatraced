---
parent: '[[000 Exploring SQL impl for extension tables]]'
spawned_by: '[[000 Exploring SQL impl for extension tables]]'
context_type: task
status: todo
---

Parent: [000 Exploring SQL impl for extension tables](../000%20Exploring%20SQL%20impl%20for%20extension%20tables.md)

Spawned by: [000 Exploring SQL impl for extension tables](../000%20Exploring%20SQL%20impl%20for%20extension%20tables.md)

Spawned in: [^spawn-task-f9fabc](../000%20Exploring%20SQL%20impl%20for%20extension%20tables.md#spawn-task-f9fabc)

# 1 Journal

2025-10-10 Wk 41 Fri - 12:14 +03:00

A Robot can have an extensible Body. One extension is a `AndroidBody`, which has the following extensions: two Arms, two Legs, and a Head.

Further, many body parts share common parts:

They can have a `CPU` which has extensions for `CPU_A` and `CPU_B` which differ based on registers.

They may have a `TempSensor` which has extensions such as
\``
Some parts, such as the head, are complicated and thus must host 2 CPU Slots.

An `OpticalSensor` may be extended with `RGBChannels` but otherwise reports light intensities in grayscale.

2025-10-10 Wk 41 Fri - 12:31 +03:00

Some parts may need to delegate to software. For example we might model extensionality as one-to-one or one-to-many, constants of how "many" must be could be software invariants.

Extended tables are linked to extensions via `Rel{TABLE_NAME}SupportsExt` tables. By allowing this to be one-to-one with respect to the extension tables but one-to-many with respect to `{TABLE_NAME}` we ensure that each object can have many distinct unshared extensions.

Through this, a head could have two distinct CPU extensions.

We also decided prior that each `Rel{TABLE_NAME}SupportsExt` had a corresponding enum where each variant corresponds to one extension table. So for example, `AndroidBody`, `FactoryBody`, `RoverBody`. Those are all kinds of bodies, so the enum idea makes sense to them. They are all exclusive, so one could choose one of many choices for an extension, and only one. The exclusivity also means that a robot cannot have two of them, like in the case of the Head with the two CPUs.

2025-10-10 Wk 41 Fri - 12:57 +03:00

So we have two mini examples to consider so far. The Head and the two CPUs, and the Robot with one exclusive choice of Body type.

2025-10-10 Wk 41 Fri - 13:09 +03:00

I think the idea with the enum was also partial simulation of Rust Enums. There are variants each with their own data loads, and in this case, each table corresponds to a name.

We also had the idea though that there could be common parts shared by the extensions, and also that extension trees could exist...

2025-10-10 Wk 41 Fri - 13:12 +03:00

So maybe we have different things here.

* The extended table,
* The supports relationship, which links the extended table to extensions installed on it.
* The extension tables, each mapping to the load of a single variant for the unifying enum of them.

So far in this concept, there is no "common" fields, it's more an application of the extended table that it can be considered to be a shared concept to the variants unified by the enum but this is not necessarily the case.

This does not have any common/shared patterns:

Robot (Extended) $\to$ (BodyExt (Android/Factory/Rover))

This does:

CommonCPU (Extended) -> (CpuRegExt (Regs1/Regs2/Regs3))

2025-10-10 Wk 41 Fri - 13:24 +03:00

The extension tree bit is just a matter of recursive application, the extension variant table can itself support extensions.

The enum variant value is itself so far unused. The Supports relationship codifies it by linking load is used and keeping the others null. It could also have the variant name, but the information is already encoded in the choice of non-null extension variant id.

It is technically possible, if we include the enum variant, to have shared loads between different kinds, so this might be beneficial to support, although it would raise complexity, let's leave commons to be handled by joins by the user.

2025-10-10 Wk 41 Fri - 13:44 +03:00

There's another case, which is flags. Suppose that an `OpticalSensor` may or may not have `RGBChannels`. It may or may not have `ZoomLens`, and it may or may not be `Networked`.  These can be considered extensions, and yet there's no unifying concept as is typical with enums. And where enums would specify exclusivity, these allow up to $N$ choices.

In addition, there are requirement chains. an `OpticalSensor` may or may not have a `Logger`, but it must be `Networked` to have a `Logger`.

2025-10-10 Wk 41 Fri - 14:01 +03:00

Requirements may be between any variants that do not share exclusivity. And exclusivity would break if for example a head has a choice of 2 from 3 exclusive CPU Register set variants.

2025-10-10 Wk 41 Fri - 14:06 +03:00

For requirements to be codified, they may be best handled by software through a Requirements graph table. This case may bring use for the variants idea. Whether it's an exclusive-set enum or inclusive-set flags, a requirements graph can encode a directional requirement between two variants.

To generalize, the requirements graph would express two enums in name, and a left and right variant identifier from them. This could handle a requirements graph.

exclusive-sets (enums with loads) and inclusive sets (bitflag enums with loads) need to be variants of their own enums.

Then there's the matter of the fact that an extended table can support multiple enums. The `Rel{TABLE_NAME}SupportsExt` should be local, describing just one enum: `Rel{TABLE_NAME}Supports{ENUM_NAME}`

Also these should all be lower case since they're tables...

`rel_{TABLE_NAME}_supports_{ENUM_NAME}`.  One per enum. This will cover exclusivity, inclusivity, and finally the requirements graph, which we may be able to make less dynamic can be through `rel_{ENUM_NAME_1}_requires_{ENUM_NAME_2}` where `{ENUM_NAME1}` and `{ENUM_NAME2}` may be identical, in case of requirements within the same enum.

The other concept is extension look-through. To be able to figure out whether an extension is supported *anywhere* in the robot. So it may have an optical sensor extension which is networked, but we would like to ask of the robot whether any of its parts are networked. This is part of the software querying we need to be able to do.

Also, requirements so far are conceived as global, but what if they are local to an entity, what if only for a robot that logging must be networked, and for other concepts, logging can be done offline?

It may be that the requirements graph be in relation to a specific extended table. But then there remains the issue of extension look-through, should it be possible for an arm to log, if an eye's optical sensor has networking capability? Maybe not?

2025-10-10 Wk 41 Fri - 14:36 +03:00

Ok so condensing,

We have exclusive/inclusive extension enums. The supports relation row supports an $n^\text{th}$ slot. So the Head Extended with the 2 CPUs, just has 2 support rows per extended row.

If a table supports multiple enums, then it will have a support relationship per each.

After that we have to think about requirements, it seems easier to enforce them generally between each other without regard to the extended. We do intend to have extensions be general, in that there can be many extended with the same extensions, so they should share the same supporting requirements.

2025-10-10 Wk 41 Fri - 14:51 +03:00

How would lookthrough work? We need to be able to traverse extensions of extensions recursively. It could be something to experiment with later in software in case we have a way to apply the operation of "get the extensions of" recursively.

2025-10-10 Wk 41 Fri - 15:58 +03:00

Let's see how exclusive could look alone.

````sql
-- *.dbmts
.EXTENSION EXCLUSIVE BodyType (
	Android(android_body),
	Factory(factory_body),
	Rover(rover_body),
);

CREATE TABLE android_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE factory_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE rover_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

#[ext(BodyType)]
CREATE TABLE robot (
  id INTEGER NOT NULL PRIMARY KEY,
);
````

````sql
-- derived *.sql

CREATE TABLE android_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE factory_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE rover_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE robot (
  id INTEGER NOT NULL PRIMARY KEY,
);

-- extra:
CREATE TABLE rel_robot_has_ext_body_type (
  id INTEGER NOT NULL PRIMARY KEY,
  extended_id INTEGER NOT NULL REFERENCES robot(id),
  opt_android_body_id INTEGER NULL REFERENCES android_body(id),
  opt_factory_body_id INTEGER NULL REFERENCES factory_body(id),
  opt_rover_body_id INTEGER NULL REFERENCES rover_body(id),
);
````

2025-10-10 Wk 41 Fri - 16:17 +03:00

To also have inclusive features,

````sql
-- *.dbmts

.EXTENSION EXCLUSIVE BodyType (
	Android(android_body),
	Factory(factory_body),
	Rover(rover_body),
);

CREATE TABLE android_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE factory_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE rover_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

.EXTENSION INCLUSIVE RobotFeatures (
	NetworkingPackets(networking_packets),
	SensorStatistics(sensor_statistics),
	Diagnostics(diagnotics),
);

CREATE TABLE networking_packets (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE sensor_statistics (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE diagnostics (
  id INTEGER NOT NULL PRIMARY KEY,
);

#[ext(BodyType)]
#[ext(RobotFeatures)]
CREATE TABLE robot (
  id INTEGER NOT NULL PRIMARY KEY,
);
````

````sql
-- derived *.sql

CREATE TABLE android_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE factory_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE rover_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE networking_packets (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE sensor_statistics (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE diagnostics (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE robot (
  id INTEGER NOT NULL PRIMARY KEY,
);

-- extra:
CREATE TABLE rel_robot_has_ext_body_type (
  id INTEGER NOT NULL PRIMARY KEY,
  extended_id INTEGER NOT NULL REFERENCES robot(id),
  opt_android_body_id INTEGER NULL REFERENCES android_body(id),
  opt_factory_body_id INTEGER NULL REFERENCES factory_body(id),
  opt_rover_body_id INTEGER NULL REFERENCES rover_body(id),
);

CREATE TABLE rel_robot_has_ext_robot_features (
  id INTEGER NOT NULL PRIMARY KEY,
  extended_id INTEGER NOT NULL REFERENCES robot(id),
  opt_networking_packets_id INTEGER NULL REFERENCES networking_packets(id),
  opt_sensor_statistics_id INTEGER NULL REFERENCES sensor_statistics(id),
  opt_diagnostics_id INTEGER NULL REFERENCES diagnostics(id),
);
````

This would be for the tables. but so far nothing really enforces structure of slots. It's up to software to try to make a more ergonomic way to use this information, and to enforce its invariants on software inserts.

2025-10-10 Wk 41 Fri - 17:03 +03:00

In Rust, there could be functions that insert extensions for specified extended tables. Note that if there is event sourcing derived  (ie, a history table is specified as a variant), then event inserts may need to be provided instead.

This is brittle but temporarily workable. We can generate everything from `*.dbmts` because for the invariant-maintaining writes from model types, it might be best we make our own write function that uses the autogen one.  Similarly with reads, just map `Hist`  to the model object for example.

2025-10-20 Wk 43 Mon - 14:26 +03:00

Let's revise how extensions are created. Like Rust enums, they should have their own container being the extension.

Joining with extensions should just be normal joins. The details about slotting can be left to the user, ie they can choose 1-to-1 or 1-to-many through an indirect table.

````sql
-- *.dbmts
.EXTENSION EXCLUSIVE Body (
	Android(android_body),
	Factory(factory_body),
	Rover(rover_body),
);

CREATE TABLE android_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE factory_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE rover_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE robot (
  id INTEGER NOT NULL PRIMARY KEY,
  body_excl_ext_id INTEGER NOT NULL REFERENCES body_excl_ext(id),
);
````

````sql
-- derived *.sql

CREATE TABLE android_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE factory_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE rover_body (
  id INTEGER NOT NULL PRIMARY KEY,
);

CREATE TABLE robot (
  id INTEGER NOT NULL PRIMARY KEY,
  body_ext_id INTEGER NOT NULL REFERENCES body_ext(id),
);

-- extra:
CREATE TABLE body_excl_ext {
  id INTEGER NOT NULL PRIMARY KEY,
  opt_android_body_id INTEGER NULL REFERENCES android_body(id),
  opt_factory_body_id INTEGER NULL REFERENCES factory_body(id),
  opt_rover_body_id INTEGER NULL REFERENCES rover_body(id),
}
````

````sql
-- *.dbmts
.KIND INCLUSIVE Features (
	NetworkingPackets(networking_packets),
	SensorStatistics(sensor_statistics),
	Diagnostics(diagnotics),
);

CREATE TABLE networking_packets ( id INTEGER NOT NULL PRIMARY KEY, );
CREATE TABLE sensor_statistics ( id INTEGER NOT NULL PRIMARY KEY, );
CREATE TABLE diagnostics ( id INTEGER NOT NULL PRIMARY KEY, );

CREATE TABLE sensor (
  id INTEGER NOT NULL PRIMARY KEY,
  features_incl_kind_id INTEGER NOT NULL REFERENCES features_incl_kind(id),
);
````

````sql
-- derived
CREATE TABLE networking_packets ( id INTEGER NOT NULL PRIMARY KEY, );
CREATE TABLE sensor_statistics ( id INTEGER NOT NULL PRIMARY KEY, );
CREATE TABLE diagnostics ( id INTEGER NOT NULL PRIMARY KEY, );

CREATE TABLE robot (
  id INTEGER NOT NULL PRIMARY KEY,
  robot_features_incl_ext_id INTEGER NOT NULL REFERENCES robot_features_incl_ext(id),
);

-- extra:
CREATE TABLE robot_features_incl_ext (
  id INTEGER NOT NULL PRIMARY KEY,
  opt_networking_packets_id INTEGER NULL REFERENCES networking_packets(id),
  opt_sensor_statistics_id INTEGER NULL REFERENCES sensor_statistics(id),
  opt_diagnostics_id INTEGER NULL REFERENCES diagnostics(id),
);
````

2025-10-20 Wk 43 Mon - 14:48 +03:00

A powerful capability lies in recursive extensions, much like recursive enums in Rust whose variants reference it.

````sql
-- *.dbmts
.KIND EXCLUSIVE HWComponent (
	Motherboard(motherboard_excl_kind),
	CPU(cpu_excl_kind),
	RAM(ram_excl_kind),
);

.KIND EXCLUSIVE Motherboard (
	Cheap(cheap_motherboard),
	Fancy(fancy_motherboard),
);

CREATE TABLE cheap_motherboard (
  id INTEGER NOT NULL PRIMARY KEY,
  intel_cpu_id INTEGER NOT NULL REFERENCES intel_cpu(id),
  low_spec_ram_id INTEGER NOT NULL REFERENCES low_spec_ram_excl_kind(id),
  low_spec_ram_id INTEGER NOT NULL REFERENCES low_spec_ram_excl_kind(id),
);

CREATE TABLE fancy_motherboard (
  id INTEGER NOT NULL PRIMARY KEY,
  cpu_excl_kind_id INTEGER NOT NULL REFERENCES cpu_excl_kind(id),
  cpu_excl_kind_id INTEGER NOT NULL REFERENCES cpu_excl_kind(id),
  ram_excl_kind_id INTEGER NOT NULL REFERENCES ram_excl_kind(id),
  ram_excl_kind_id INTEGER NOT NULL REFERENCES ram_excl_kind(id),
  ram_excl_kind_id INTEGER NOT NULL REFERENCES ram_excl_kind(id),
  ram_excl_kind_id INTEGER NOT NULL REFERENCES ram_excl_kind(id),
);

.KIND EXCLUSIVE CPU (
	Intel(intel_cpu),
	AMD(amd_cpu),
);

CREATE TABLE intel_cpu ( id INTEGER NOT NULL PRIMARY KEY, );
CREATE TABLE amd_cpu ( id INTEGER NOT NULL PRIMARY KEY, );

.KIND EXCLUSIVE LOW_SPEC_RAM (
	Ram8GB(ram),
	Ram16GB(ram),
);

.KIND EXCLUSIVE RAM (
	Ram8GB(ram),
	Ram16GB(ram),
	Ram32GB(ram),
);

CREATE TABLE ram ( id INTEGER NOT NULL PRIMARY KEY, );

CREATE TABLE shopping_cart (  id INTEGER NOT NULL PRIMARY KEY, );

CREATE TABLE shopping_cat_has_many (
	id INTEGER NOT NULL PRIMARY KEY,
	shopping_cart_id INTEGER NOT NULL REFERENCES shopping_cart(id),
	hw_component_excl_kind_id INTEGER NOT NULL REFERENCES hw_component_excl_kind(id),
	cost INTEGER NOT NULL,
);
````

2025-10-20 Wk 43 Mon - 15:48 +03:00

````sql
-- *.dbmts
.KIND INCLUSIVE Capabilities (
	Transmission(transmission),
	BatteryBank(battery_bank)
	SolarCharging(solar_charging),
);

CREATE TABLE transmission ( id INTEGER NOT NULL PRIMARY KEY, );
CREATE TABLE battery_bank ( id INTEGER NOT NULL PRIMARY KEY, );

CREATE TABLE solar_charging (
	id INTEGER NOT NULL PRIMARY KEY, 
	battery_bank_req_id INTEGER NOT NULL REFERENCES battery_bank(id),
);

CREATE TABLE robot (
  id INTEGER NOT NULL PRIMARY KEY,
  capabilities_incl_kind_id INTEGER NOT NULL REFERENCES capabilities_incl_kind(id),
);
````

I renamed extensions to kinds because it's the closest to enum, and enum is already taken. An exclusive kind is like a rust enum. An inclusive kind is more bitflags with payload.
