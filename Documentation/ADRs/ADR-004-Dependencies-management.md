# Context

Almost all software we write depends on some other code, library or development tool which allows us to build what we want faster. Although this project attempts to avoid bringing in 3rd party dependencies, there are is functionality already written that is critical to this projects success.

# Decision

The main dependency is the programmable in-process mock server that can receive network requests and respond with the response we define. This dependency is written in rust and is available at [pact-foundation/pact-reference/rust](https://github.com/pact-foundation/pact-reference/tree/master/rust/pact_mock_server_ffi).

The binary framework(s) that are built using `cargo lipo --release` command are added into the Xcode project.

Unfortunately SPM doesn't handle the binary dependencies well at the time of this writing. Therefore a SPM package is required

There will be a separation of responsibilities between PactSwift framework and PactSwiftServices in a separate (yet embedded) project which will provide extra functionality by reaching out to and/or interact with different services (interacting with Pact Mock Server, etc.).

# Consequences

Due to SPM not handling binary dependencies well. When linking and embedding a binary framework while building and running in Xcode everything works fine, `xcodebuild` command in command line builds the project and dependencies just fine.

Yet, when running `swift build` in terminal, SPM doesn't know where to find it. That's why a separate SPM package to provide the binary framework as a dependency is required and unfortunately the binary framework is duplicated in the codebase - once in `PactSwiftServices` project and once in `PactMockServer` swift package.