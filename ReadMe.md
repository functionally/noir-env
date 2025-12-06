# Development environment for Noir

A Nix development environment with Noir and related ZK tools.


## Entering the environment

One can either clone this repository and enter the development environment from within it:

```bash
git clone https://github.com/functionally/noir-env
cd noir-env
nix develop
```

Or enter the environment without cloning the repository:

```bash
nix develop github:functionally/noir-env
```


## Available tools

The following command-line tools are available in this enviornment.

- [Noir](https://noir-lang.org/)
    - `nargo`
    - `noir-execute`
    - `noir-inspector`
    - `noir-profiler`
    - `noir-ssa`
- [Barretenberg](https://barretenberg.aztec.network/docs/)
    - `bb`
- [Co-Snarks](https://docs.taceo.io/docs/co-noir/)
    - `co-noir`
    - `noir_repr3`
    - `co-circom`
- [Circom](https://docs.circom.io/)
    - `circom`


## Example


### 1. Create a new project

```console
$ nargo new example

$ cd example

Project successfully created! It is located at noir-env/example

$ tree

.
├── Nargo.toml
└── src
    └── main.nr

$ cat src/main.nr

fn main(x: Field, y: pub Field) {
    assert(x != y);
}

#[test]
fn test_main() {
    main(1, 2);

    // Uncomment to make test fail
    // main(1, 1);
}

```


### 2. Create a `Prover.toml` file for the inputs

```console
$ nargo check

$ cat Prover.toml

x = ""
y = ""
```


### 3. Set the inputs `x = 1` and `y = 2`

```console
$ sed -e 's/x = ""/x = "1"/' -e 's/y = ""/y = "2"/' -i Prover.toml

$ cat Prover.toml

x = "1"
y = "2"
```


### 4. Compile and execute the circuit

```console
$ nargo execute

[example] Circuit witness successfully solved
[example] Witness saved to target/example.gz
```


### 5. Create the proof

```console
$ bb prove -b ./target/example.json -w ./target/example.gz --write_vk -o target                                                                                                                        

Scheme is: ultra_honk, num threads: 16 (mem: 12.69 MiB)
CircuitProve: Proving key computed in 11 ms (mem: 32.10 MiB)
WARNING: computing verification key while proving. Pass in a precomputed vk for better performance. (mem: 32.10 MiB)
Public inputs saved to "target/public_inputs" (mem: 35.10 MiB)
Proof saved to "target/proof" (mem: 35.10 MiB)
VK saved to "target/vk" (mem: 35.10 MiB)
VK Hash saved to "target/vk_hash" (mem: 35.10 MiB)

$ tree

.
├── Nargo.toml
├── Prover.toml
├── src
│   └── main.nr
└── target
    ├── example.gz
    ├── example.json
    ├── proof
    ├── public_inputs
    ├── vk
    └── vk_hash

3 directories, 9 files
```

### 6. View the public inputs

```bash
od -t x1 -w32 target/public_inputs
```


### 7. Verify the proof

```console
$ bb verify -p ./target/proof -k ./target/vk -i ./target/public_inputs

Scheme is: ultra_honk, num threads: 16 (mem: 12.69 MiB)
Proof verified successfully (mem: 15.73 MiB)
```
