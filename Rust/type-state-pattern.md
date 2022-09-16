# Type State Pattern

- [Gist](#gist)
- [Example](#example)

## Gist
We want to represent different states as different types so we can control the state transition and prevent invalid transition at compile time.

## Example
Imagine we have a `User` struct:
```rust
struct User {
    username: String,
    email: String,
    logged_in: bool,
}
```

We now have a few problems: `username` and `email` can be invalid, and nothing stopping a bug from changing `logged_in` unexpectedly. To solve the `username` and `email` being invalid, we can introduce new types.
```rust
struct Username(String);
struct Email(String);

impl Username {
    pub fn new(username: String) -> Result<Self, String> {
        if username.len() >= 3 {
            Ok(Self(username))
        } else {
            Err("Username too short".to_string())
        }
    }
}

impl Email {
    pub fn new(email: String) -> Result<Self, String> {
        if email.contains('@') {
            Ok(Self(email))
        } else {
            Err("Email invalid".to_string())
        }
    }
}
```

The new `Username` and `Email` types will ensure `username` and `email` fields are valid. And to solve the `logged_in` state issue we can introduce another type:
```rust
struct User {
    username: Username,
    email: Email,
}

struct LoggedInUser(User);

impl User {
    pub fn log_in(self) -> LoggedInUser {
        // login code
        LoggedInUser(self)
    }
}
```

We removed `logged_in` and introduced `LoggedInUser` type to indicate a logged in user. In `User` we also introduced `log_in` which transition the `User` type to `LoggedInUser` type. This way we can control the one directional transition from `User` to `LoggedInUser` and avoid future bugs due to invalid states.
