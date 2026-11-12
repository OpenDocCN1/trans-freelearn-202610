# # Creating the recipe file

The second step consists of creating the recipe file; let's call this recipe webserver-packt_01.bb.

The idea is to have the following architecture inside the recipes-custom folder:

```
$ tree webserver-packt/
webserver-packt/
à"êa"êâ"€ webserver-packt
à",   à"à"â"êâ"€ server.tar.gz
à"""â"êâ"€ webserver-packt_01.bb
```

# ## Note

The server.tar.gz tarball contains the main application (nodejs + HTML5).

To create this recipe, we will use the recipetool command , as follows:

```
$ source oe-init-build-env rpi-build
$ recipetool create -d -o webserver-packt_0.1.bb ../meta-packt_rpi/recipes-custom/webserver-packt/webserver-packt/
$ cp webserver-packt_0.1.bb ../meta-packt_rpi/recipes-custom/webserver-packt/
```

# ## Note

The complete documentation of recipetool command is online at http://www.yoctoproject.org/docs/1.8/mega-manual/mega-manual.html#new-recipe-creating-the-base-recipe-using-recipetool .

Now we just need to copy the entire archive (server.tar.gz) containing the sources to ${datadir} (/usr/share/); this step is carried out in the do_install() block, as shown here:

```
DESCRIPTION = "Panel to monitor rpi temperature"
LICENSE = "GPLv2"
LIC_FILES_CHKSUM =
"file://${COREBASE}/meta/COPYING.GPLv2;md5=751419260aa954499f7abaabaa882bbe"

# Package Release
PR = "r0"

# Use local tarball
SRC_URI = " \\
file://server.tar.gz \\
"

RDEPENDS_${PN} += "bash"

# Make sure our source directory (for the build) matches the directory structure in
the tarball
S = "${WORKDIR}"

do_install() {
install -d ${D}${datadir}/server-packt
```