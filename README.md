# VerifiedCommits
Tested on macOS, Linux, and WSL on Windows 11.

Note: XCode does not allow for signing your commits using GPG so they will not show up as verified on GitHub.  You can however, always use Github Desktop to sign your commits which I have tested myself.

## Generate the GPG Key

### Generate a GPG key using the command:

    gpg --full-generate-key

Enter to accept the default RSA and RSA kind of key.
Enter the keysize as 4096 bits.
When prompted to enter user ID information, enter the verified email address for your GitHub account.
Add a passphrase as required.

### Once the key is generated, verify by running:

    gpg --list-secret-keys --keyid-format LONG

##### Example output:
    gpg: checking the trustdb
    gpg: marginals needed: 3  completes needed: 1  trust model: pgp
    gpg: depth: 0  valid:   3  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 3u
    gpg: next trustdb check due at 2023-09-21
    /Users/firstnamelastname/.gnupg/pubring.kbx
    --------------------------------------
    sec   rsa3072/E3F678E2082FFD28 2023-09-14 [SC] [expires: 2023-09-21]
        EF5B15FEAB93784D5A978136E3F678E2082FFD28
    uid                 [ultimate] test-name (no comment) <test-email@example.com>
    ssb   rsa3072/0AE917C1039735BD 2023-09-14 [E] [expires: 2023-09-21]

```E3F678E2082FFD28``` is the key id in this example.

## Adding the GPG key to your GitHub account.
### Copy the GPC key id from the previous step, and export the key using:

    gpg --armor --export <key id>

Example output:

>-----BEGIN PGP PUBLIC KEY BLOCK-----
>
>...
>
>...
>
>-----END PGP PUBLIC KEY BLOCK-----

### Add exported gpg key to github
Settings > SSH and GPG keys > New GPG key

### Configure Git to Use GPG Key
    git config --global user.signingkey <key id>
    git config --global commit.gpgsign true

## Configuring Git
### Set username and email (if not already done):

    git config --global user.name='username'
    git config --global user.email-'example.email@gmail.com'

### Confirm Git configuration:

    git config --list

Should contain:

    user.name='username'
    user.email='example.email@gmail.com'
    user.signingkey='examplekey'
    commit.gpgsign=true

## WSL Notes
Since VSCode and WSL are running different systems, you may experience issues when making your commit with Git not finding your gpg keys.  If you make your commit in the WSL terminal however, it should work.

    git commit -S -m "your commit message here"


## Deleting keys
Show all your current keys:

    gpg --list-keys

Example output:

    --------------------------------
    pub   rsa3072 2023-11-07 [SC]
        AAF113832923E6BA2EF4FD5576A1BD04A19D987D
    uid           [ultimate] username (example-comment) <example@exampledomain.com>
    sub   rsa3072 2023-11-07 [E]

In this example your uid is ```username```.

To delete your key use:

    gpg --delete-secret-key [uid]
    gpg --delete-key [uid]