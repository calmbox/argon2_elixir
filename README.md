# Argon2

[![Build Status](https://travis-ci.com/riverrun/argon2_elixir.svg?branch=master)](https://travis-ci.com/riverrun/argon2_elixir)
[![Hex Version](https://img.shields.io/hexpm/v/argon2_elixir.svg)](https://hex.pm/packages/argon2_elixir)
[![Hex Docs](https://img.shields.io/badge/hex-docs-lightgreen.svg)](https://hexdocs.pm/argon2_elixir/)
[![Total Download](https://img.shields.io/hexpm/dt/argon2_elixir.svg)](https://hex.pm/packages/argon2_elixir)
[![License](https://img.shields.io/hexpm/l/argon2_elixir.svg)](https://github.com/riverrun/argon2_elixir/blob/master/LICENSE)
[![Last Updated](https://img.shields.io/github/last-commit/riverrun/argon2_elixir.svg)](https://github.com/riverrun/argon2_elixir/commits/master)


Argon2 password hashing library for Elixir.

## calmbox fork

This is calmbox's fork of argon2_elixir 4.1.3. It is not published to Hex;
depend on it from GitHub by tag. Changes from upstream:

* Vendors the Argon2 reference implementation (P-H-C/phc-winner-argon2
  `f57e61e`) instead of using a git submodule.
* Builds the reference code with `ARGON2_NO_THREADS`. A `parallelism` above 1
  still produces standard hashes, but its lanes are computed on one thread
  instead of starting OS threads inside the NIF.
* The NIF no longer reads its unused output buffer uninitialized, and it rejects
  out-of-range arguments: `m_cost` outside 3..22, unknown Argon2 types, salts
  longer than 64 bytes, hashes longer than 512 bytes, encoded buffers larger than
  1024 bytes and stored hashes of 1024 characters or more.
* The base64 decoder rejects non-ASCII bytes in a stored hash. Platforms with a
  signed `char`, such as x86-64, previously decoded them as `/`.
* `Argon2.Base.hash_password/3` raises a descriptive `ArgumentError` for an
  out-of-range `m_cost`, `argon2_type`, `hashlen` or salt length.

[Argon2](https://github.com/P-H-C/phc-winner-argon2) is the official winner
of the [Password Hashing Competition](https://password-hashing.net/),
a several year project to identify a successor to Bcrypt / Pbkdf2 / Scrypt
password hashing methods.

## Installation


1.  Add `:argon2_elixir` to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [
        {:argon2_elixir, "~> 4.0"}
      ]
    end
    ```

2.  Optional: during tests (and tests only), you may want to speed up the tests. If you have a `config/test.exs`, you can add:

    ```elixir
    config :argon2_elixir,
      t_cost: 1,
      m_cost: 8
    ```

## Configuration

Configure `argon2_elixir` - see the documentation for
[`Argon2.Stats`](https://hexdocs.pm/argon2_elixir/Argon2.Stats.html) for more details

## Comeonin wiki

See the [Comeonin wiki](https://github.com/riverrun/comeonin/wiki) for more
information on the following topics:

* [algorithms](https://github.com/riverrun/comeonin/wiki/Choosing-the-password-hashing-algorithm)
* [requirements](https://github.com/riverrun/comeonin/wiki/Requirements)
* [deployment](https://github.com/riverrun/comeonin/wiki/Deployment)
  * including information about using Docker
* [references](https://github.com/riverrun/comeonin/wiki/References)

## Contributing

There are many ways you can contribute to the development of this library, including:

* reporting issues
* improving documentation
* sharing your experiences with others

### Development

```bash
$ git clone https://github.com/riverrun/argon2_elixir
$ git submodule update --init --recursive
$ mix do deps.get, compile
$ mix test
```

### Documentation

http://hexdocs.pm/argon2_elixir

## Copyright and License

Copyright (c) 2016 David Whitlock

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at [https://www.apache.org/licenses/LICENSE-2.0](https://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
