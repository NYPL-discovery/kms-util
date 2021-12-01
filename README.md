# KMS Util

This is a tiny utility for encrypting & decrypting things using using [NYPL conventions](https://github.com/NYPL/engineering-general/blob/master/security/secrets.md).

It lets you do things like this:

1. Decrypt the contents of your clipboard and print it to screen:
```
pbpaste | kms-util decrypt
```

2. Encrypt the contents of your clipboard "in place":
```
pbpaste | kms-util encrypt | pbcopy
```

## Installation

### 1. Clone this repo somwhere sensible, e.g.:
```
mkdir -p ~/bin
cd ~/bin
git clone git://github.com/NYPL-discovery/kms-util
```

### 2. Make sure the binary is in your path

If using BASH:
```
echo "export PATH=\$PATH:~/bin/kms-util" >> ~/.bash_profile
source ~/.bash_profile
```

If using ZSH:
```
echo "export PATH=\$PATH:~/bin/kms-util" >> ~/.zshrc
source ~/.zshrc
```

## Usage

You can encrypt or decrypt like this:
```
kms-util [encrypt|decrypt] VALUE [--profile P]
```
Or:
```
echo 'some-value' | kms-util [encrypt|decrypt] [--profile P]
```

Parameters:
 * `--profile` - Set the AWS profile. Default is 'nypl-digital-dev'

### Fun with pipes

The output of this command can be piped to other commands. For example, this outputs "foo":

```
kms-util encrypt foo | kms-util decrypt
```

To encrypt the contents of your clipboard "in place":

```
pbpaste | kms-util encrypt | pbcopy
```

To effectively change the encryption of the contents of your clipboard from profile nypl-digital-dev to nypl-sandbox:

```
pbpaste | kms-util decrypt | kms-util encrypt --profile nypl-sandbox | pbcopy
```

### Mutli-line input

Multi-line input support is experimental.

Suppose you have a multi-line YAML file:

```
echo "multi:
  line:
  - thing
" > example.yml
```

You can decrypt the whole thing like this:

```
cat example.yml | kms-util encrypt
```

### Encrypt

This will encrypt `VALUE` using 'nypl-digital-dev' (or the given profile) and print it:
```
kms-util encrypt VALUE [--profile P]
```

This also reads stdin, so if the value to encrypt is in your clipboard, you can:
```
pbpaste | kms-util encrypt [--profile P]
```

### Decrypt

This will dencrypt `VALUE` using 'nypl-digital-dev' (or the given profile) and print it:
```
kms-util encrypt VALUE [--profile P]
```

This also reads stdin, so if the value to decrypt is in your clipboard, you can:
```
pbpaste | kms-util decrypt [--profile P]
```
