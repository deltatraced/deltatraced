---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[002 Add event accumulation events through diesel]]'
context_type: task
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [002 Add event accumulation events through diesel](002%20Add%20event%20accumulation%20events%20through%20diesel.md)

Spawned in: [^spawn-task-91bae6](002%20Add%20event%20accumulation%20events%20through%20diesel.md#spawn-task-91bae6)

# 1 Journal

2025-10-02 Wk 40 Thu - 11:34 +03:00

The feature branch is here: [feature/view_support branch](https://github.com/weiznich/diesel/tree/feature/view_support).

````sh
git clone git@github.com:weiznich/diesel.git ~/src/cloned/gh/weiznich/branches/diesel@view_support

# in /home/lan/src/cloned/gh/weiznich/branches/diesel@view_support
cargo run

# out (error, relevant)
error: `cargo run` could not determine which binary to run. Use the `--bin` option to specify a binary, or the `default-run` manifest key.
available binaries: advanced-blog-cli, custom-types, delete_post, delete_post, delete_post, diesel, get_post, get_post, get_post, publish_post, publish_post, publish_post, relations, relations, show_posts, show_posts, show_posts, show_posts_step_1, show_posts_step_1, show_posts_step_1, show_posts_step_2, show_posts_step_2, show_posts_step_2, write_post, write_post, write_post, write_post_step_2, write_post_step_2, write_post_step_2, xtask
````

````sh
# in /home/lan/src/cloned/gh/weiznich/branches/diesel@view_support
cargo run

# out (error, relevant)
error: failed to run custom build command for `mysqlclient-src v0.1.4+9.3.0`

Cannot find RPC development libraries.  You need to install the required
    packages:

      Debian/Ubuntu:              apt install libtirpc-dev
      RedHat/Fedora/Oracle Linux: yum install libtirpc-devel
      SuSE:                       zypper install glibc-devel
````

````sh
sudo apt install libtirpc-dev
````

````sh
# in /home/lan/src/cloned/gh/weiznich/branches/diesel@view_support
cargo run

# out
Usage: diesel [OPTIONS] <COMMAND>

Commands:
  migration     A group of commands for generating, running, and reverting migrations.
  setup         Creates the migrations directory, creates the database specified in your DATABASE_URL, and runs existing migrations.
  database      A group of commands for setting up and resetting your database.
  completions   Generate shell completion scripts for the diesel command.
  print-schema  Print table definitions for database schema.
  help          Print this message or the help of the given subcommand(s)

Options:
      --database-url <DATABASE_URL>  Specifies the database URL to connect to. Falls back to the DATABASE_URL environment variable if unspecified.
      --config-file <CONFIG_FILE>    The location of the configuration file to use. Falls back to the `DIESEL_CONFIG_FILE` environment variable if unspecified. Defaults to `diesel.toml` in your project root. See diesel.rs/guides/configuring-diesel-cli for documentation on this file.
      --locked-schema                Require that the schema file is up to date.
  -h, --help                         Print help (see more with '--help')
  -V, --version                      Print version

You can also run `diesel SUBCOMMAND -h` to get more information about that subcommand.
````

Awesome. Now let's do this in the view_support branch.

````sh
# in /home/lan/src/cloned/gh/weiznich/branches/diesel@view_support
git checkout feature/view_support
cargo run --bin diesel
````

2025-10-02 Wk 40 Thu - 11:57 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
source ./.env && rm $DATABASE_URL; ~/src/cloned/gh/weiznich/branches/diesel@view_support/target/debug/diesel migration run && python3 scripts/diesel-postprocess.py
````

The views are still not recognized in `schema.rs`.

2025-10-02 Wk 40 Thu - 12:34 +03:00

In [fn sqlite_diesel_types](https://github.com/weiznich/diesel/blob/da578f2af39bdd7e433cd0c7ca3286c6da6af1fd/diesel_cli/src/print_schema.rs#L155) they include some common types that I don't think are present in sqlite itself for some reason.

Anyway [fn output_schema](https://github.com/weiznich/diesel/blob/da578f2af39bdd7e433cd0c7ca3286c6da6af1fd/diesel_cli/src/print_schema.rs#L162) does not yet have any view-related support.
