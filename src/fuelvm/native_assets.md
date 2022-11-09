# Multiple Native Assets

Fuel supports multiple native assets as first-class citizens. Any single asset may be forwarded with a [`CALL`](https://fuellabs.github.io/fuel-specs/master/vm/instruction_set#call-call-contract). Contracts have a balance of all possible assets instead of only the base asset.

Note that only the base asset is usable to pay for gas fees.
