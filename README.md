
# scat

`scat` is a *password scatterer*. It allows the generation of unique passwords for each service,
website, email address or account you might have, all from a single password.

## Motivation

Nowadays, accounts for many services such as Facebook, Twitter, Reddit, Google, Amazon, your bank account, etc. are needed.
In a perfect world, all those accounts would have different passwords, so that, if someone gets to know, let's say, your Facebook password,
they don't gain access to your bank account and your money as well. But, on the other hand, who would like to remember dozens and dozens of different passwords?

`scat` is the solution to this problem. It allows you to safely generate for each website or service you suscribe to a unique password. All you have to do is remember a single, as strong as possible, password.

Given the same service and password, `scat` will always generate the same password, so you don't have to remember them or note them down!
Passwords generated by `scat` are very secure and independant of each others. If by misfortune one of the generated password is compromised, all other passwords are still safe, and so is the password you used to generate them.

## Example

To use `scat`, simply call it specifying which key, or service, it must generate a password for.
Then, simply enter your password (which is, in this example, `pony1234`):

```
> scat -c -k "github"
Password:
Confirm:
Generated password:
p3sz\x9vUsn{"Kjzxt
```

> Note that `pony1234` is **not** your github password. It's the original password you use to generate other passwords.
> **Never share it, or use it as a password for anything else than `scat`.**

Let's say that you now want to generate a Facebook password:

```
> scat -c -k "facebook"
Password:
Confirm:
Generated password:
6yorHvhrpj#8Yce:bl
```
The password generated for Facebook is completely different from the one it generated for GitHub.


Imagine now that your are on an other computer, with no access to your keychain, and you would like to login to Facebook (just for 5 minutes).
To your great despair, there is no way you can remember your obscure password!
However, as `scat` is fully deterministic, you can simply call it once more, to generate once again the exact same password, this time from another computer.

```
> scat -c -k "facebook"
Password:
Confirm:
Generated password:
6yorHvhrpj#8Yce:bl
```

> **CAUTION** `scat` is not an excuse to use a weak password such as `pony1234`. Always use a strong password (that you can remember) with `scat`.

## Security

By default, `scat` will generate password of length 18, using a mix of lower case letters, upper case letters, digits and various ascii symbols.
This leads to a password entropy of about 115 bits. Meaning that an attacker
knowing which schema you used and able to test a billion password per second would have to wait approximately 50 million times *the age of the universe* to 
guess your password correctly. So it's pretty safe.

Now, let's imagine for a second that an attacker gets to know one of your generated password.
This is pretty bad, but not as bad as having a single password. Imagine for a second the disaster if your attacker could directly access all your accounts!
Knowing a single generated password won't help your attacker much, it is still practically infeasable for him to get to your original password, so all of your other accounts are safe!

## Password schemas

As we have just seen, `scat` generates by default password of length 18. But, it also provides other schemas!

If you want, for some reason, an easily rememberable passphrase, for let's say Facebook, you can use the schema named `diceware`,
which will output 5 words out of the 7776 words of [the Diceware list][diceware].

```
> scat -c -k "facebook" -s diceware
Password:
Confirm:
Generated password:
ieee te real sepoy whiff
```

Or, if you prefer Pokémons:

```
> scat -c -k "facebook" -s pokemons
Password:
Confirm:
Generated password:
Zapdos 27, Vulpix 43, Venusaur 21, Staryu 12
```

### Summary of available schemas

Name | Comments | Entropy (in bits)
:--- | :------- | :----------------
safe | A mix of 18 ascii symbols. (default) | 115
alpha | A mix of 18 alphanumeric symbols. | 104
parano | 78 ascii characters | 512
pokemons | 4 classic Pokémons, each with a level up to 100 | 55
diceware | 5 words out of the [diceware's list][diceware]. | 65

## How it works

Under the hood, `scat` will use `SHA-512`, a cryptographic hash function, to generate a huge integer seed.
This integer seed will be consumed by `scat` to generate deterministically a new password from a schema.

## Installing

For now, `scat` is only distributed as a Haskell source code. 

1. If you do not have Haskell and Cabal installed, please visit [the Haskell website and download the Haskell platform][haskell-platform].

2. Once this is done, download the `scat` code and place yourself in the root directory of the project (in which you should find a file named `snap.cabal`).

3. Run the command `cabal configure && cabal install` to install `scat`.

4. Enjoy!

## Contributing

Feel free to contribute to this project! If you have a brilliant idea to make this project better, just say so!
If you lack ideas but would like to participate anyway, you can also find here a list of things to do!

### Things to do

1. Create other schemas.

2. Let the user specify a size for some schemas.

3. Compatible port in mainstream programming languages.

4. Some other improvements you might want!

### Contributors

Name | Contributions
:--- | :------------
Romain Edelmann | Initial work on the project.

[diceware]: http://world.std.com/~reinhold/diceware.html
[haskell-platform]: http://www.haskell.org/platform/