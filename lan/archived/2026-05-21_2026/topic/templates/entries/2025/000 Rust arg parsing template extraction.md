\#lan #rust #template

2025-06-20 Wk 25 Fri - 14:44

This is gonna be extracted from [bn_repo_editor](https://github.com/LanHikari22/bn_repo_editor) [^link1](000%20Rust%20arg%20parsing%20template%20extraction.md#link1).

# 1 Original extract

This is the original text, with some unnecessary details removed.

````rust
#![allow(unused_variables)]
#![allow(dead_code)]

use serde::{Deserialize};
use clap::{Arg, ArgAction, ArgMatches, Command, command};
use std::env;
use std::fs;

const SETTINGS_ENV: &str = "BN_REPO_EDITOR_SETTINGS";
const SETTINGS_FILENAME: &str = "bn_repo_editor_settings.json";

#[derive(Debug, Deserialize)]
pub struct AppSettings {
    bn_repo_dir: String,
    app_data_dir: String,
    blacklisted_proj_dirs: Vec<String>,
}

fn read_settings_path() -> Option<String> {
    // By default, if the user provides a path via an environmental variable, then use it
    if let Ok(val) = env::var(SETTINGS_ENV) {
        if !val.trim().is_empty() {
            return Some(val);
        }
    }

    // Otherwise the setting file should be in the same directory invoked
    let cur_dir = env::current_dir().ok()?;
    let cur_settings = cur_dir.join(SETTINGS_FILENAME);
    if cur_settings.exists() {
        return Some(cur_settings.to_string_lossy().into());
    }

    None
}

fn read_app_settings() -> Option<AppSettings> {
    let settings_path = read_settings_path()?;

    let content = fs::read_to_string(settings_path).ok()?;
    let settings: AppSettings = serde_json::from_str(&content).ok()?;

    Some(settings)
}

fn parse_args() -> ArgMatches {
    command!()
        .arg(
            Arg::new("verbose")
                .help("1: info, 2: debug, 3: trace")
                .short('v')
                .action(ArgAction::Count),
        )
        .subcommand(
            Command::new("lexer")
                .about("Scans the repository into stream of tokens")
        )
        .subcommand(
            Command::new("preproc")
                .about("Runs include preprocessing on the files")
                // .arg(
                //     arg!([output_dir] "Path to the output directory")
                //         .required(true)
                //         .value_parser(value_parser!(PathBuf)),
                // ),
        )
        .subcommand(
            Command::new("gen_debug_elf")
                .about("Another command")
                .arg(
                    Arg::new("regen")
                        .long("regen")
                        .help("Regenerates all intermediates")
                        .action(clap::ArgAction::SetTrue),
                )
        )
        .subcommand_required(true)
        // .get_matches()
        .get_matches_from(vec!["app", "--help"]) // for testing
}

fn main() {
    // let app_settings = {
    //     read_app_settings().expect(&format!(
    //         "\n\
    //             This app needs settings. It was not found or has errors.\n\
    //             The path provided be under the environment variable {SETTINGS_ENV} \n\
    //             or as {SETTINGS_FILENAME} where this command is invoked.\n\
    //             Refer to default/{SETTINGS_FILENAME} in the project repo.\n\
    //         "
    //     ))
    // };

    let matches = parse_args();

    let verbose = matches.get_count("verbose");

    match matches.subcommand() {
        Some(("lexer", _sub_matches)) => {
        },

        Some(("preproc", _sub_matches)) => {
        },

        Some(("gen_debug_elf", sub_matches)) => {
            let regen = sub_matches.get_flag("regen");
			// ...
        },

        _ => unreachable!(),
    }
}
````

# 2 Playground

Interact with the code in the Rust [Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=2e5c344fe0740c7c1c610b143985ace1) [^link2](000%20Rust%20arg%20parsing%20template%20extraction.md#link2)

# 3 References

1. https://github.com/LanHikari22/bn_repo_editor ^link1
1. [Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=2e5c344fe0740c7c1c610b143985ace1) ^link2
