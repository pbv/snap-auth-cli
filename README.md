# snap-auth-cli: CLI tool to manage Snap auth database

[![Travis CI build status](https://travis-ci.org/dzhus/snap-auth-cli.svg)](https://travis-ci.org/dzhus/snap-auth-cli)
[![Hackage](https://img.shields.io/hackage/v/snap-auth-cli.svg)](https://hackage.haskell.org/package/snap-auth-cli)
[![Hackage deps](https://img.shields.io/hackage-deps/v/snap-auth-cli.svg)](http://packdeps.haskellers.com/feed?needle=snap-auth-cli)

This tool provides command-line interface to
Haskell [Snap][snap] [AuthManager][snap-auth] to create, view and
delete users in database. Currently the Json and Sqlite file backends are
supported.

Passwords for new users are provided in plain text.

By default the database resides in current directory in `users.json`
file.  A different db may be specified using `-f` flag.
To specify a Sqlite database use `-s -f filename`.

# How to use

Type `snap-auth-cli --help` to get usage help.

Create a user:

    $ snap-auth-cli --create -u TwasBrillig -p SlithyToves1855

User roles may be set when creating account using arbitary number
of `-o` flags:

    $ snap-auth-cli --create -u TwasBrillig2 -p SlithyToves1855 -o gyre -o gimble

A user may have an arbitary number of key-value pairs attached in the
meta field (currently all fields are stored in Strings):

    $ snap-auth-cli --create -u AlexP -p 1234 -k number -v 3214 -k foo -v bar -o admin

Read a user from DB (`--read` flag may be omitted since reading
is the default mode):

    $ snap-auth-cli --read -u AlexP

```json
{
    "meta": {
        "number": "3214",
        "foo": "bar"
    },
    "suspended_at": null,
    "roles": [
        "admin"
    ],
    "pw": "sha256|12|VpUGBg2O/NBkDTVTSqqYuA==|TIDuc3ToAPmALXCHBxTA8SjlUBztPS8nH6qiV63a+f4=",
    "activated_at": null,
    "current_ip": null,
    "locked_until": null,
    "updated_at": "2012-02-22T09:00:29.377Z",
    "login_count": 0,
    "current_login_at": null,
    "login": "AlexP",
    "remember_token": null,
    "failed_login_count": 0,
    "last_ip": null,
    "last_login_at": null,
    "uid": "1",
    "created_at": null
}
```

To specify a different database file (Json by default):

    $ snap-auth-cli -f back.json --create -u MimsyBorogove -p 0utgr@b3d

Note that if database file doesn't exist, it will be created from
scratch.

To specify a Sqlite database:

    $ snap-auth-cli -s -f db.sqlite --read -u AlexP

Unlike the Json backend, the database file with the correct schema
must aleady exist; this should be automaticallly initialized by
when you first run the Snap application.


Existing users can be modified using the `-m` option. User is
selected by login. Any of `-p`, `-o` or `-k/-v` flags may be
specified to set new value for user password, roles or meta. If no
new value is provided, old field is preserved.

Set a new password for user:

    $ snap-auth-cli -m -u Mome -p r@th$$

Set a new role:

    $ snap-auth-cli -m -u Mome -o foobarer

Replace user meta:

    $ snap-auth-cli -m -u BG -k tel -v 2-12-85-06

The tool provides interface to delete users;  the Sqlite backend supports
this operation but the JsonFile backend does not.

[snap]: http://snapframework.com/
[snap-auth]: http://hackage.haskell.org/package/snap/docs/Snap-Snaplet-Auth.html
