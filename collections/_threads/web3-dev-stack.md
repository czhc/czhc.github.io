Settled on these tools for web3 dev, for anyone searching the open internet.

Key points:

- Concise and interactive
- Full visibility for debugging
- Tightly integrated - the less names I need to remember, the better.

[...]

1. 👷 @HardhatHQ + @ethers: for contract dev framework
Rich APIs and plugins, comprehesive docs and readable, simple syntax.

[...]

2. 🐞 Unit testing: hardhat + @ethworks waffle + smockit + chaijs
Moved away from remix-tests.

Generating fixtures, load contracts, unit-testing, and natural, rspec-ruby-ish matching syntax from chai. Smockit gives a nice stubbing/mocking API to properly test for separation of concerns.

[...]

2.1 🏁 Getting Started:
- https://docs.openzeppelin.com/learn
- https://hardhat.org/getting-started/
- @thingkingassets: https://ethereum-blockchain-developer.com/

[...]

3. 🏗 Scaffolding / Packaging: scaffold-eth

Good template to package a fullstack, monorepo app. Recommend this after you've sorted out contract dev and just need everything else, instead of starting directly from here.

[...]

4. work station: 🎧 @ethereumremix connected to local
Adds on linting, function test UI, security scanning, gas analysis, stack debugging etc.

Seem to be missing how to run hardhat tests on remix
