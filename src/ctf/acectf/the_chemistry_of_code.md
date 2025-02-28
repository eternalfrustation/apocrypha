# The Chemistry Of Code

During a discussion with hackers, one idea stuck: every hacker must be able to understand code, no matter how cryptic.

Hidden within obfuscated Rust code is a function that holds the flag. But it only unlocks when you reverse its logic and uncover the correct username and password.

Do you have what it takes to decode this tangled masterpiece? Dive into the chemistry of code and prove your skills.

Hint: Some secrets are worth their salt.

```rust
use base64::{engine::general_purpose::STANDARD, Engine};
use hex::encode as hex_encode;
use num_bigint::BigUint;
use std::io::{self, Write};

const FERROUS_OXIDE_USERNAME: &str = "AdminFeroxide";
const ANIONIC_PASSWORD: &str = "NjQzMzcyNzUzNTM3MzE2Njc5MzE2ZTM2";
const ALKALINE_SECRET: &str = "4143454354467B34707072336E373163335F3634322C28010D3461302C392E";

fn ionic_bond(cation_input: &str, anion_input: &str) {
    let cation_hex = hex_encode(cation_input);
    let anion_hex = hex_encode(anion_input);

    let cation_value = BigUint::parse_bytes(cation_hex.as_bytes(), 16).unwrap();
    let anion_value = BigUint::parse_bytes(anion_hex.as_bytes(), 16).unwrap();

    let covalent_link = &cation_value ^ &anion_value;

    let alkaline_secret_value = BigUint::parse_bytes(ALKALINE_SECRET.as_bytes(), 16).unwrap();
    let metallic_alloy = &covalent_link ^ &alkaline_secret_value;

    let precipitate = format!("{:x}", metallic_alloy);

    let alloy_compound = (0..precipitate.len())
        .step_by(2)
        .map(|i| u8::from_str_radix(&precipitate[i..i + 2], 16).unwrap() as char)
        .collect::<String>();

    println!("Crystallized Flag (ASCII): {}", alloy_compound);
}

fn reaction_chamber() {
    let mut username = String::new();
    print!("Introduce the Catalyst: ");
    io::stdout().flush().unwrap();
    io::stdin().read_line(&mut username).unwrap();
    let username = username.trim();

    let mut password = String::new();
    print!("Introduce the Reagent: ");
    io::stdout().flush().unwrap();
    io::stdin().read_line(&mut password).unwrap();
    let password = password.trim();
    
    if username != FERROUS_OXIDE_USERNAME {
        println!("Reaction denied: Unstable molecule detected.");
        return;
    }
    
    let reagent_ion = STANDARD.encode(hex_encode(password).as_bytes());
    if reagent_ion != ANIONIC_PASSWORD {
        println!("Reaction denied: Unstable molecule detected.");
        return;
    }

    ionic_bond(username, password);
}

fn main() {
    reaction_chamber();
}
```

## Solution

Overall, one of the easier ones, `reaction_chamber` acts as the `main` function.

The program does the following:

- Read the username from `stdin` as a line
- Read the password from `stdin` as a line
- Early returns if username is not equal to AdminFeroxide
- Encodes the password as follows:
    - Consider the password as raw bytes and encode that as hex
    - Encode the result of previous step as base64
- Early returns if the encoded password is not equal to the constant ANIONIC_PASSWORD
- Proceeds to print the flag

So, to get the correct password, we need to reverse the encoding process.

We decode the ANIONIC_PASSWORD from base64 and get

```
643372753537316679316e36
```

Decoding this as hex, we get

```
d3ru571fy1n6
```

Inputting this as the password, we get:

```
Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.01s
Running `target/debug/chemistryofcode`

Introduce the Catalyst: AdminFeroxide
Introduce the Reagent: d3ru571fy1n6
Crystallized Flag (ASCII): ACECTF{4ppr3n71c3_w4l73r_wh1t3}
```

ACECTF{4ppr3n71c3_w4l73r_wh1t3}
