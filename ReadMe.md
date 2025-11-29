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


## Tools

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

