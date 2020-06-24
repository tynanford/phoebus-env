# Phoebus

All configurations are based on three source repositories such as pheobus [1], nsls2-phoebus [2], and phoebus-sns [3].


## Requirements

### Apache Tomcat Native Library and Maven

* Debian 10

```
apt install maven openjfx libopenjfx-java
```

* CentOS 7

```
yum install maven
```

* CentOS 8 & Fedora 31
One can use `yum` instead of `dnf`. `epel-release` is needed. 
```
dnf install maven
```


### JDK 8 or newer
* Debian 10
```
openjdk version "11.0.6" 2020-01-14
OpenJDK Runtime Environment (build 11.0.6+10-post-Debian-1deb10u1)
OpenJDK 64-Bit Server VM (build 11.0.6+10-post-Debian-1deb10u1, mixed mode, sharing)
```

* Fedora 31
```
openjdk version "1.8.0_242"
OpenJDK Runtime Environment (build 1.8.0_242-b08)
OpenJDK 654-Bit Server VM (build 25.242-b08, mixed mode)
```

## A typical example to build and run the Phoebus 

```
$ make init
$ make build.phoebus
$ make prop.phoebus
$ make install.phoebus
$ source scripts/activate-phoebus.bash
$ run-phoebus.bash
```

## Makefile Rules

### `make init`
* Download the lastest phoebus source
* Switch to a specific version defined in `$(SRC_TAG)` in `configure/RELEASE`
```
$ make init
```

### `make build.phoebus`
* Build the default community phoebus, located in `phoebus-src/phoebus-product/target`.
* This rule builds everything in `phoebus-src` include all alarm services. 

### `make prop.phoebus`

* Generate phoebus setting file `phoebus_settings.ini` locate in `site-template` path based on repository configuration. One can check the most important configuration variables via
```
$ make vars
$ make vars FILTER=PS_
```
Please check site-template/phoebus_settings.ini file to match your environment. This rule is independent one, so any others cannot modify this file, so the local modification is safe. One can put its own settings.ini in `site-template` path. In this case, please don't run this command, but run `install.phoebus` directly.

### `make install.phoebus`
* `sudo` permission may be required.
* Install all phoebus files into `INSTALL_LOCATION_PHOEBUS` defined in `configure/CONTIG_SITE` and `configure/CONFIG_TARGET`
* one can check after execute this rule, what they are in, `make exist` or `make exist.phoebus`
```
tree -aL 1 /opt/phoebus-products/phoebus
/opt/phoebus-products/phoebus
├── activate-phoebus.bash
├── bin
├── lib
├── phoebus.jar
├── phoebus_logback.xml
├── phoebus_logging.properties
├── phoebus_settings.ini
├── phoebus.sh
├── product-phoebus.jar -> /opt/phoebus-products/phoebus/phoebus.jar
├── .versions
└── .versions~
```
* Expand its `tree` level with `make exist.phoebus LEVEL=2`
* Prerequirement of this rule is `make sh.phoebus` which generates two files
* If the exist `phoebus` path is found, the exist one will be renamed to `phoebus_backup_YYMMDD-HHMMSS`.

### `make sh.phoebus`
* `PHOEBUS_SHELL` and `PHOEBUS_ACTIVATE` in `configure/CONFIG_OPTS_PHOEBUS`. 
* With `make install.phoebus`, this rule generate two files : `activate-phoebus` and `xphoebus in `scripts` path. 

```
$ souce scripts/activate-phoebus
$ xPhoebus
```
That `activate-phoebus` is also installed in `INSTALL_LOCATION_PHOEBUS`. The installation location of `xpheobus` is `INSTALL_LOCATION_PHOEBUS/bin`. In addtion. the genric `phoebus.sh` is installed in `INSTALL_LOCATION_PHOEBUS`, however we don't add the path to `PATH` in `activate-phoebus`.



## Upcoming feature based on nsls2-phoebus


* Build the ALS products 
```
$ make als
```

* Print out makefile variables
```
$ make vars
```


## References
[1] https://github.com/ControlSystemStudio/phoebus

[2] https://github.com/shroffk/nsls2-phoebus

[3] https://github.com/kasemir/phoebus-sns