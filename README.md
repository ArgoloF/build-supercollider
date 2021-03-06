# build-supercollider

A _dead simple_ script that installs (build) dependencies, clones [Supercollider](https://github.com/supercollider/supercollider/), builds it, and finally will install it.

## Current version:

> 3.8.0


## How to?

```bash
$ git clone https://github.com/lvm/build-supercollider/
$ cd build-supercollider
$ sh build-supercollider.sh
$ sh build-sc3-plugins.sh
```

That's all.  
Keep in mind this script works on Debian-derivates distributions (such as Ubuntu, Mint, etc). Sorry Windows, OS X, and other GNU/linux distros.

### Known issues

If for some reason SC doesn't compile and throws an error about `libyaml.a`, check [this issue](https://github.com/supercollider/supercollider/issues/2825) and try again.

## Debian way

Dan Stowell posted an article about [how to package SC for debian/ubuntu](http://mcld.co.uk/blog/2017/how-to-package-supercollider-for-debian-and-ubuntu-linux.html), which seems to be the most sane way to build SC if you're using one of these distros.  
To make things simpler (and avoid breaking your OS), I've created Docker recipes to create a clean Debian and Ubuntu building environments with the necessary dependencies to properly build deb packages.

### Using Docker

In order to build the Docker image, enter to your prefered distro and:
```
$ docker build -t scdeb .
```

To run it after:
```
$ docker run --rm -it scdeb
```

### Dan's post

Once you have a clean Debian installation working, just follow his advice.  
The only thing that changes are:

`gbp import-orig --pristine-tar --sign-tags ${path-to-fetched-tarball}`  
He describes `../tarballs` as the directory where the repacked tar is placed, in my case this directory was simply `..` (the directory above).

`gbp buildpackage -S --git-export-dir=../buildarea`  
Same thing here: He describes `../buildarea` as the directory where the repacked tar is placed, in my case this directory was simply `..` (the directory above).
  
Also, since I'm not Felipe Sateler nor Dan Stowell, I wasn't able to sign the `.changes` or the `source`, so I used the flags `-uc -us` which doesn't sign the `.changes` / `source` respectively.  
ie: `gbp buildpackage -S -uc -us --git-export-dir=../`

### That's it.

If everything went well, you'll see a bunch of `.deb` files placed in the directory above of the cloned repository.
