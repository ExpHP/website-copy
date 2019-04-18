### Concepts

- derive
- struct
- traits

### Reasonable solutions

A reasonable solution should do the following:

- `#[derive(Debug, PartialEq)]`
- Reuse logic; `Clock::add_minutes` should call `Clock::new`, or vice versa.
  Division/modulus should only occur in one of these functions.
- Don't directly `use std::fmt::Result`. (prefer `use std::fmt`)
- (more items here)

### Examples

```rust
#![feature(euclidean_division)]
use std::fmt;

#[derive(Debug, PartialEq)]
pub struct Clock {
    minutes: i32,
}

const MINS_PER_DAY: i32 = 60 * 24;

impl Clock {
    pub fn new(hours: i32, minutes: i32) -> Self {
        Clock {
            minutes: (hours * 60 + minutes).mod_euc(MINS_PER_DAY),
        }
    }

    pub fn add_minutes(self, minutes: i32) -> Self {
        Clock::new(0, self.minutes + minutes)
    }
}

impl fmt::Display for Clock {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "{:02}:{:02}", self.minutes / 60, self.minutes % 60)
    }
}
```
