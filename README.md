Template engine with cache for Rust, PHP and Pythom
===================================================

Example of the Neutral TS cache in the terminal.

Download, navitate to neutral-cache-test directory and:

```plaintext
cargo run
```

Arbitrary key/value arguments can be set:

```plaintext
cargo run -- --argname argvalue
cargo run -- --argname "arg value"
```

There is an argument (`inject`) designed to try to inject code:

```plaintext
cargo run -- --inject "{:include; /path/to/secrets :}"
```

`inject` has the default value `{:exit; 403 :}`, what happens if the injection succeeds can be tested with:

```plaintext
cargo run -- --exit 403
```

The directory of the disk cache will be the temporary directory of the system, it can be changed in `main.rs` is indicated.

Cache
-----

The cache is modular, allowing only parts of the template to be included in the cache:

```plaintext
<!DOCTYPE html>
<html>
    <head>
        <title>Cache</title>
    </head>
    <body>

        {:cache; /120/ >>
            <div>{:code; ... :}</div>
        :}

        <div>{:date; %H:%M:%S :}</div>

        {:cache; /120/ >>
            <div>{:code; ... :}</div>
        :}

    </body>
</html>
```
Or exclude parts of the cache, the previous example would be much better like this:

```plaintext
{:cache; /120/ >>
    <!DOCTYPE html>
    <html>
        <head>
            <title>Cache</title>
        </head>
        <body>
            {:!cache;
                {:date; %H:%M:%S :}
            :}
        </body>
    </html>
:}
```

Overview of cache syntax
------------------------

```plaintext
{:cache; /expires/addtoid/only_custom_id/ >> ... :}
{:cache; /expires/addtoid/ >> ... :}
{:cache; /expires/ >> ... :}
{:!cache; ... :}
```

* expires: Seconds of life in the cache
* addtoid: Add a literal to the cache ID
* only_custom_id: Use only the ID passed as ID

The only mandatory parameter is `expires`, the cache automatically generates an ID with context data, such as language, cookies, ... and code.

Example
-------

The example template is in the `neutral-cache-test/tpl` directory with the name `index.ntpl`, you can modify it as much as you want:

```plaintext
{:cache; /300/ >>
    <!DOCTYPE html>
    <html>
        <head>
            <title>Cache</title>
        </head>
        <body>
            <main>
                <div>In cache:{:date; %H:%M:%S :}</div>
                <div>No cache:{:!cache; {:date; %H:%M:%S :} :}</div>
            </main>
        </body>
    </html>
:}
```

Links
-----

- [Repository](https://gitlab.com/neutralfw/neutralts)
- [Crate](https://crates.io/crates/neutralts)
- [Docs](https://docs.rs/neutralts/latest/neutralts/doc/index.html)
