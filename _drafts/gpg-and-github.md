# GPG Keys with Git

Then generate a key, you can follow the advice
[here](https://help.github.com/articles/generating-a-new-gpg-key/), but
essentially you want to create a new 4096 bit GPG key.

```bash
$  gpg --list-keys
/Users/gib/.gnupg/pubring.gpg
-----------------------------
pub   4096R/821C587A 2016-10-07
uid       [ultimate] Gibson Fahnestock <gibfahn@gmail.com>
sub   4096R/2C482931 2016-10-07
```

To tell git to sign all commits by default:

```bash
git config --global commit.gpgsign true
```

If you have multiple private gpg keys, you need to tell git which one to use,
or set the default in `~/.gnupg/gpg.conf`.

To tell git about your signing key:

```bash
git config --global user.signingkey 821C587A
```

You may need to tell GPG to prompt for a password in the current terminal.

```bash
export GPG_TTY=`tty`
```

Start by installing the right tools, for macOS:

```bash
brew cask install gpgtools
```

To usse gpg preferences to store your gpg key follow:
https://gpgtools.tenderapp.com/kb/faq/password-management

Copy and paste into github.com/settings/keys:

```bash
gpg --armor --export B01FBB92821C587A | pbcopy
```

