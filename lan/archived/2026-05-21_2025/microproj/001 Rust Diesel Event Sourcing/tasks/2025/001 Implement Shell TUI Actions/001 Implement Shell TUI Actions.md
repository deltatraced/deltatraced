---
status: todo
---

# 1 Journal

2025-09-21 Wk 38 Sun - 02:42 +03:00

We want to prompt the user for input similar to how we did in the diesel tutorial in [write_post.rs](https://github.com/deltachives/2025-001-tut-diesel-rs/blob/main/src/bin/write_post.rs).

2025-09-21 Wk 38 Sun - 03:02 +03:00

Created `read_str_or_quit` and `read_input_from_user_until_valid_or_quit`.

Put it under `drivers/drivers.rs` to keep `mod.rs` only for listing submodules, but then we'd have to do `drivers::drivers::my_fn` which is kinda redundant, so let's just keep in `mod.rs`.

And the two functions allow us to get input from the user, and allows them to signal to us to cancel the action or try again.

2025-09-21 Wk 38 Sun - 03:28 +03:00

We created the first driver to insert an event via a shi command!

2025-09-21 Wk 38 Sun - 03:48 +03:00

I made it so the user can type `[q]uit` which should mean full `quit` or just `q` to quit the input.  We ran into an issue where because we used `print!` instead of `println!`, the user wouldn't see anything and would be stuck in a loop of being asked correct input. I made `println!`, so this doesn't happen, but really we should flush the terminal.

Now the event accumulator says that work has arrived but nothing is happening once we're trying to insert an event. It seems that it got stuck, so probably blocking for the event accumulator to respond.

2025-09-21 Wk 38 Sun - 03:57 +03:00

````
| db credit_store insert
enter person or type [q]uit: 
aaa
enter credits (i32) or type [q]uit: 
12
enter event_stack_level (i32) or type [q]uit: 
0
[2025-09-21T00:56:52Z INFO  credit_store_demo::db::event_accumulator_actions] awaiting event accumulator
[2025-09-21T00:56:52Z INFO  credit_store_demo::db::event_accumulator] Work has arrived!
[2025-09-21T00:56:52Z INFO  credit_store_demo::db::event_accumulator_actions] Table credit_store_head has been serviced!
Created event CreditStoreEvent { id: 2, person: "aaa", credits: 12, opt_object_id: Some(291), opt_event_id: None, opt_event_arg: None, event_stack_level: 0, event_action: Insert, created_on: "2025-09-21T03:56:52.927064879+03:00" }
| 
````

Nothing has been serviced yet, just a run that await works. We removed some test code reading all events and then updating the head table row 1, even though there's nothing in the head table yet.

![Pasted image 20250921040047.png](../../../../../../../../attachments/Pasted%20image%2020250921040047.png)

Even with the freeze, our previous event has been registered.

2025-09-21 Wk 38 Sun - 04:06 +03:00

Okay sending an `Unimplemented` error instead of saying that we're done.

````
| db credit_store insert
enter person or type [q]uit: 
bbb
enter credits (i32) or type [q]uit: 
123
enter event_stack_level (i32) or type [q]uit: 
0
[2025-09-21T01:07:31Z INFO  credit_store_demo::db::event_accumulator_actions] awaiting event accumulator
[2025-09-21T01:07:31Z INFO  credit_store_demo::db::event_accumulator] Work has arrived!
Failed to create event: EaWorkError(Unimplemented)
````

Yup the command now outputs the error we get from the event accumulator to the shell stdout. This failure is not critical, it only informs the user that the command failed, and they can keep executing commands.

2025-09-21 Wk 38 Sun - 04:18 +03:00

We will need to implement in the event accumulator actions more event actions to add, like deleting objects, or updating them. Then we can have corresponding shell ui commands for them.

2025-10-21 Wk 43 Tue - 20:07 +03:00

* [ ] Add command for creating a User with an initial amount of coins. Fail if the user already exists.

* [ ] Add command for adding expenses to a user

* [ ] Add a command to view the expense transactions and one for the coin wallet

* [ ] Add a branch command, which creates a new frame in the next span

* [ ] Add a reset local, which creates a new frame in the current span

* [ ] Add a reset global, which creates a new frame in span 1

* [ ] Add a checkout command, where user can specify "latest" or a given span and frame

* [ ] Add an option to load initial data from CSV

* [ ] Add an option to reset the db on run (no persistance)

* [ ] Create a guide for customizing the ontology

2025-10-21 Wk 43 Tue - 20:28 +03:00

Spawn [000 Add TUI Commands to interact with the coin store](tasks/000%20Add%20TUI%20Commands%20to%20interact%20with%20the%20coin%20store.md) ^spawn-task-519857
