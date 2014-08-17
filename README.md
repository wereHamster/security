Harddrive / USB stick encryption
--------------------------------

Whenever I need to encrypt a harddrive or a usb stick, I use LUKS with the following options.

    cryptsetup luksFormat -c aes-xts-plain -h sha512 -s 512 -y --use-random -i 5000 <dev>


USB stick backups
-----------------

To backup my USB stick, I tar up its contents and encrypt it with openssl. Then copy the tarball
to multiple online locations (google drive, my own servers etc).

Note that newer versions of `openssl enc` dropped support for XTS ciphers. I therefore switched
to CBC mode. 

    tar -cJf - -C /media/vault . | openssl enc -e -aes-256-cbc > ~/.vault.tar.xz

In case I lose the USB stick and need to restore its contents:

    cat ~/.vault.tar.xz | openssl enc -d -aes-256-cbc | tar -xJf - -C /media/vault
