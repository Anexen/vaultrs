# vaultrs-test

> A test suite for testing against [Hashicorp Vault][1] servers.

## Installation

Add `vaultrs-test` as a developemnt depdendency to your cargo.toml:
```
[dev-dependencies]
vaultrs = "0.1.0"
```

## Usage

```rust
use vaultrs_test::docker::{Server, ServerConfig};
use vaultrs_test::{VaultServer, VaultServerConfig};

// Configures a container to run Vault server v1.8.2
let config = VaultServerConfig::default(Some("1.8.2"));

// Creates a test instance to run the container in
let instance = config.to_instance();

// Runs the test instance, passing in details about the container environment
// The code below only runs after the container is verified running
instance.run(|ops| async move {
    // Creates an abstraction for interacting with the Vault container
    let server = VaultServer::new(&ops, &config);

    // Verify server is ready for requests
    let status = server.client.status().await;
    assert!(matches! { status, vaultrs::sys::ServerStatus::OK });
})

// Container is cleaned up at this point
```

## Testing

Run tests with cargo:

```
cargo test
```

## Contributing

Check out the [issues][2] for items neeeding attention or submit your own and 
then:

1. Fork the repo (https://github.com/jmgilman/vaultrs-test/fork)
2. Create your feature branch (git checkout -b feature/fooBar)
3. Commit your changes (git commit -am 'Add some fooBar')
4. Push to the branch (git push origin feature/fooBar)
5. Create a new Pull Request

[1]: https://www.vaultproject.io/
[2]: https://github.com/jmgilman/vaultrs-test/issues