---
layout: wiki
title: QubesR3Building
permalink: /wiki/QubesR3Building/
---

Building Qubes OS 3.0 ISO
=========================

Get the necessary keys to verify the sources:

``` {.wiki}
$ wget https://keys.qubes-os.org/keys/qubes-master-signing-key.asc
$ gpg --import qubes-master-signing-key.asc 
$ gpg --edit-key 36879494
# Verify fingerprint!, set trust to *ultimate*
$ wget https://keys.qubes-os.org/keys/qubes-developers-keys.asc
$ gpg --import qubes-developers-keys.asc
```

Note we do *not* relay above on the security of our server (keys.qubes-os.org) nor the connection (ssl, cert) -- we only rely on you getting the Qubes Master Signing Key fingerprint *somehow* and ensure they match!

Now lets bootstrap the builder. Unfortunately the builder cannot verify itself (the classic Chicken and Egg problem), so we need to verify the signature manually:

``` {.wiki}
$ git clone git://git.qubes-os.org/qubes-r3/qubes-builder.git
$ cd qubes-builder
$ git describe --exact-match HEAD
<some tag>
$ git tag -v <some tag>
```

Assuming the verification went fine, we're good to go with all the rest without ever thinking more about verifying digital signatures on all the rest of the components, as the builder will do that for us, for each component, every time we, even for all aux files (e.g. Xen or Linux kernel sources).

Let's configure the builder first (we can use one of the example configs, either for R2 or "master", which currently means pre-released R3):

``` {.wiki}
cp example-configs/qubes-os-master.conf builder.conf
```

You can take a loot at the `builder.conf.default` for a description of all available options. Nevertheless, the default config should be enough for start:

``` {.wiki}
$ make get-sources qubes
$ make sign-all # this requires setting SIGN_KEY in the builder.conf, can be skipped for test builds.
$ make iso
```

Enjoy your new ISO!