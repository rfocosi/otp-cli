# otp-cli

## Description

One-Time Password generator tool using `oathtool`.

It works like `Authy` and `Google Authenticator`, but for command line.

Works on any shell (Tested on `sh`, `bash` and `zsh`).

Automatically copies the token into your computer's copy buffer. Just paste it anywhere.

This tool supports both encrypted and plain-text token files.

## Requirements

* oathtool (http://www.nongnu.org/oath-toolkit/)
* OpenSSL

### Required to send to copy buffer

* xclip (Linux)
* pbcopy (MacOS)

Ps.: You can still generate and print OTP without those tools

## Setup

### oathtool

- Ubuntu/Debian
```
sudo apt install oathtool
```

- MacOS
```
brew install core-utils
brew install oath-toolkit
```

### Clone the project

```
git clone git@github.com:rfocosi/otp-cli.git
```

### Add system wide (optional)

- Inside project's root:

```
sudo ln -s $( echo "$( pwd )/otp-cli" ) /usr/local/bin/otp-cli
```

## Usage

When you run any command for the first time, it will create a new directory on:

`$HOME/otp-cli/tokens/`

Where it will store the added tokens and config file.

Ex.:
```
$ ./otp-cli add my_token <secret_key>
An empty password will not lock the file
Password: <hidden>
Confirm password: <hidden>
Created [<$HOME>/otp-cli/tokens/my_token.enc]

$ ./otp-cli show my_token
OTP Password: <hidden>
[15] 923842

$ ./otp-cli clip my_token
OTP Password: <hidden>
Sent to clipboard!

```

### Config file

The config file is generated, after first run, on `<$HOME>/otp-cli/config`

Example file:
```
#!/bin/sh

## This is an example config file
## All configurations done here will be interpreted as a SH script

## If you have issues with the OS auto selection, try this
#OS=Linux

## Remaining seconds to wait for next OTP
#WAIT_FOR_NEXT=5
```

#### Add a token

```
Usage: otp-cli add [-h] [Token Name] [Token Key]

If [Token Name] or [Token Key] are empty, they will be prompted.

If the password is empty, the token will be a plain text file.
```

Ex.:
```
$ ./otp-cli add
Token name: my_token
Token key: <hidden>

An empty password will not lock the file
Password: <hidden>
Confirm password: <hidden>
Created [<$HOME>/otp-cli/tokens/my_token.enc]
```

#### Show OTP

```
Usage: otp-cli show [-h] [-1] [-c] [-s] <Token Name>

 -1 : Get one password and exit.
 -c : Copy to clipboard.
 -s : Silent. Do not output anything to console.
```

Ex.:
```
$ ./otp-cli show my_token
OTP Password: <hidden>
[SS] DDDDDD
```
Where:

- [SS] is the seconds counter. A new OTP will be generated every 30 seconds.
- [DDDDDD] is the 6-digit One-Time-Password.

#### Copy to clipboard

```
Usage: otp-cli clip [-h] [-k] <Token Name>

 -k : Keep generating OTP.
```

Ex.:
```
$ ./otp-cli clip my_token
OTP Password: <hidden>
```

#### Unlock token file

```
Usage: otp-cli unlock [-h] [Token Name]

If [Token Name] is empty, it will be prompted.
```

Ex.:
```
$ ./otp-cli unlock my_token
Password: <hidden>
Unlocked file [<$HOME>/otp-cli/tokens/my_token]
```

#### Remove token

```
Usage: otp-cli remove [-h] [Token Name]

If [Token Name] is empty, it will be prompted.
```

Ex.:
```
$ ./otp-cli remove my_token
Removed file [<$HOME>/otp-cli/tokens/my_token.enc]
```
