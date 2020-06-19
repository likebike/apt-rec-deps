# apt-rec-deps
Manually download apt dependencies, recursively.

I sometimes need to inject packages into Debian docker images that don't have an internet connection.  Here is how I gather all the packages and their dependencies:


## Usage

1. Look at your system's apt package source list:

    cat /etc/apt/sources.list

You'll see something like `deb http://deb.debian.org/debian/ stretch main`.
Put the URL piece (`http://deb.debian.org/debian/`) into your web browser and navigate to your architecture's URL... something like:

    http://deb.debian.org/debian/dists/stretch/main/binary-amd64/

2. Download the Package list:

    wget 'http://deb.debian.org/debian/dists/stretch/main/binary-amd64/Packages.gz'
    zcat Packages.gz >Packages

3. Get a recursive list of packages URLs:

    SRC_URL='http://deb.debian.org/debian/' ./rec-deps smbclient >pkg_urls

4. Download all the packages:

    mkdir debs
    ( cd debs && wget -i ../pkg_urls )

5. Create a bundle:

    tar czf smbclient_debs.tar.gz debs/

6. Copy the bundle to your target system.

7. Install

    ...command todo...

