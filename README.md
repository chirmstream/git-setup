# GPG Commit Signing
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

    git config --global user.name 'username'
    git config --global user.email 'example.email@gmail.com'

### Confirm Git configuration:

    git config --list

Should contain:

    user.name='username'
    user.email='example.email@gmail.com'
    user.signingkey='examplekey'
    commit.gpgsign=true

## Exporting/Importing Keys
Exports all keys into home directory:

    gpg -o private.gpg --export-options backup --export-secret-keys

To export a specific key use:

    gpg -o private.gpg --export-options backup --export-secret-keys <key-email>

Import (will need to set owner trust afterwards):

    gpg --import-options restore --import private.gpg

To import with the maximum ownertrust at the same time use:

    gpg --import -ownertrust private.gpg


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


## WSL Notes
Since VSCode and WSL are running different systems, you may experience issues when making your commit with Git not finding your gpg keys.  If you make your commit in the WSL terminal however, it should work.

    git commit -S -m "your commit message here"

----------------------------

# SSH Access
## Generate new SSH key pair

    ssh-keygen -t ed25519 -C “comment”
Copy ssh key either manually or use:

### macOS

    tr -d '\n' < ~/.ssh/id_ed25519.pub | pbcopy

### Linux (requires the xclip package)

    xclip -sel clip < ~/.ssh/id_ed25519.pub

### Git Bash on Windows

    cat ~/.ssh/id_ed25519.pub | clip

Add key to account.

Verify by opening a terminal and run this command, replacing "gitlab.example.com" with your GitLab instance URL (or github's):

    ssh -T git@gitlab.example.com

If this is the first time you connect, you should verify the authenticity of the GitLab host. If you see a message like:

    The authenticity of host 'gitlab.example.com (35.231.145.151)' can't be established.
    ECDSA key fingerprint is SHA256:HbW3g8zUjNSksFbqTiUWPWg2Bq1x8xdGUrliXFzSnUw.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'gitlab.example.com' (ECDSA) to the list of known hosts.

If you need to troubleshoot you can run:

    ssh -Tvvv git@gitlab.example.com

## Using nonstandard ssh port (port 22)
SSH by default uses port 22.  If your selfhosted instance is using a different port (such as 9022 for example) you will need to configure git to use a different port.  The easiest way is to edit your ``~/.ssh/config`` file.  You can individually set ports for each domain you use.

    Host github.com
    Port 22
    Host git.selfhosted.example
    Port 9022

Pulled from gitlab [docs](https://docs.gitlab.com/ee/user/ssh.html).

