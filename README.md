# ark-zkey

Library to read `zkey` faster by serializing to `arkworks` friendly format.

See https://github.com/zkmopro/mopro/issues/25 for context.

## How to use

Run the following to convert a `zkey` to an `arkzkey`. This should be done as a pre-processing step.

`cargo run --bin arkzkey-util --release -- ./test-vectors/keccak256_256_test_final.zkey`

This will generate and place an `arkzkey` file in the same directory as the original zkey.

You can also install it locally:

`cargo install --bin arkzkey-util --path .`

## Tests

```
cargo test multiplier2 --release -- --nocapture
cargo test keccak256 --release -- --nocapture
cargo test rsa --release -- --nocapture
```

## Benchmark (Keccak)

`cargo test keccak256 --release -- --nocapture`

```
running 1 test
Reading zkey from: ./test-vectors/keccak256_256_test_final.zkey
Time to read zkey: 101.761584ms
Serializing proving key and constraint matrices
Time to serialize proving key and constraint matrices: 42ns
[build] Writing arkzkey to: ./test-vectors/keccak256_256_test_final.arkzkey
[build] Time to write arkzkey: 9.048011125s
Reading arkzkey from: ./test-vectors/keccak256_256_test_final.arkzkey
Time to open arkzkey file: 40.875µs
Time to mmap arkzkey: 14.541µs
Time to deserialize proving key: 9.050112917s
Time to deserialize matrices: 26.763958ms
Time to read arkzkey: 9.077114958s
test tests::test_keccak256_serialization_deserialization ... ok
```

| circuits | zkey | ark-zkey |
| :--: | :--: | :--: |
| multiplier| 3 KB | 1KB |
| keccak256 | 81.1 MB | 49.5 MB |
| rsa| 134.9 MB| 97.8 MB| 