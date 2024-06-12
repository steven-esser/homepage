****************
GnuPG Cheatsheet
****************

:title: GnuPG Cheatsheet
:date: 2020-11-16
:category: Posts
:slug: gpg-cheatsheet
:tags: gpg, unix

Here are some useful common operations when using GnuPG (gpg).

Generate a GnuPG key pair
#########################
Use the following to generate a ``gpg`` key pair:
::

    gpg --full-generate-key

Export and import a single key pair
###################################
::

    # export the keys
    gpg --export-secret-keys -a foo@bar.com > private.key.asc
    gpg --export -a foo@bar.com > public.key.asc

    # import the keys
    gpg --import private.key.asc
    gpg --import public.key.asc

Export and import all key pairs
###############################
The following technique is useful for backup and migration of ``gpg`` keys:
::

    # export public and private keys, along with owner trust data
    gpg --export-secret-keys -a > private-keys.asc
    gpg --export -a > public-keys.asc
    gpg --export-ownertrust > owner-trust.txt

    # import public and private keys, along with owner trust data
    gpg --import private-keys.asc
    gpg --import public-keys.asc
    gpg --import-ownertrust owner-trust.txt
