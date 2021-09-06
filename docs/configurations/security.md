# GPG
Overview: https://www.aplawrence.com/Basics/gpg.html
Gnu Handbook:

## Example: Tom want to send an encrypted message to Marge
1. Marge should
    - Generate a key
```bash
gpg --gen-key
```
    - List keys
```bash
gpg --list-keys
```
    - Export her public key and send it to Tom
```bash
gpg --armor --export $EMAIL > marge_public_key
```
1. Tom should:
    - Import Marge's public key
```bash
gpg --import marge_public_key
```
    - Optionally, sign the key if he's sure that it comes from Marge and it wasn't intercepted/changed
```bash
gpg --edit-key $EMAIL
gpg > sign
```
    - Use it to encrypt a file
```bash
echo "A very very confidential message!" > secrets
gpg --out secrets_to_marge --encrypt secrets
```

Tom sends `secrets_to_marge` somehow (email, hand to hand ...), then marge do
```bash
gpg --output secrets_from_tom --decrypt secrets_to_marge
```

# Pass: password management
- Install pass
```bash
sudo apt install pass
```
- Create a gpg key
```
gpg --key-gen
```
- Init pass
```
pass init mohammedi.haroun@gmail.com
```
- Init git and push
```
pass git init
pass git remote add origin git@github.com:mohammedi-haroune/password-store.git
pass git push --set-upstream origin master
```
- Install [Password Store App](https://play.google.com/store/apps/details?id=dev.msfjarvis.aps)
    - Generate ssh key from the App
    - Add it to Github Settings page
    - Pull passwords
- Install [OpenKeyChain App](https://play.google.com/store/apps/details?id=org.sufficientlysecure.keychain)
    - Generate secret key for your gpg key
    - Copy it to your phone somehow and add it to OpenKeyChain (passphrase required to read it)
```bash
gpg --armor --export-secret-key mohammedi.haroun@gmail.com
```

Go to the Password Store App, it'll ask for permission to read from OpenKeyChain to be able to decrypt the password got from the git repository, give it the permission and Boom !!!

- Install [chrome extension](https://github.com/browserpass/browserpass-extension)
    - Install frontend from [Chrome webstore](https://chrome.google.com/webstore/detail/browserpass-ce/naepdomgkenhinolocfifgehidddafch)
    - Install the native host app from source
```bash
# Download the code from the releases page: https://github.com/browserpass/browserpass-native/releases
# Install go
sudo snap install go
# Build and test
sudo make
# Install
sudo make install
#Configure browserpass for Google Chrome browser, for the current user only
make hosts-chrome-user
# Or System-wide
# sudo make hosts-chrome	
```
!!! Note:
    Installing with `apt` didn't worked for me because there where no `hosts` and `policies` in `/usr/lib/browserpass/`

- Install `pass import`: https://github.com/roddhjav/pass-import
```bash
wget -qO - https://pkg.pujol.io/debian/gpgkey | sudo apt-key add -
echo 'deb [arch=amd64] https://pkg.pujol.io/debian/repo all main' | sudo tee /etc/apt/sources.list.d/pkg.pujol.io.list
sudo apt-get update
sudo apt-get install pass-extension-import
```
