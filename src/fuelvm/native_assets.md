# Multiple Native Assets

Fuel supports multiple native assets as first-class citizens. Any single asset may be forwarded with a [`CALL`](https://github.com/FuelLabs/fuel-specs/blob/master/specs/vm/instruction_set.md#call-call-contract). Contracts have a balance of all possible assets instead of only the base asset.

Note that only the base asset is usable to pay for gas fees.
