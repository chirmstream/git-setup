Setup on linux is nearly identical to macOS.  Homebrew will not be needed, but you may need to install git depending on your distro.

### Generate the GPG Key
### Install GPG command line tools.
You may need to use the command gpg2 instead ofgpg.

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
### Copy the GPC key id, and export the key using:

    gpg --armor --export <key id>

### Add the GPG key to your GitHub account.

### Configure Git to Use GPG Key
    git config --global user.signingkey <key id>
    git config --global commit.gpgsign true

### Make sure your .gitconfig contains the following information
Get global git configuration:

    git config --list
Should contain:

    user.name='username'
    user.email='example.email@gmail.com'
    user.signingkey='examplekey'
    commit.gpgsign=true

If missing user.name and user.email simply add using:

    git config --global user.name='username'
    git config --global user.email-'example.email@gmail.com'

### Github desktop fork available for linux [here](https://github.com/shiftkey/desktop)