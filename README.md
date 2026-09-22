# dsInitd

---
## English

**dsInitd** is a lightweight service management system for Debian/Linux systems.

It provides a simple layer on top of traditional SysV init scripts and GNU Screen, allowing long-running applications and custom daemons to be created, started, stopped, monitored and registered for automatic startup.

The project supports both fully standalone init scripts and compact dsInit configuration files.

### Features

* SysV-compatible service management
* GNU Screen based daemon execution
* Central `dsinitctl` management tool
* Standalone daemon scripts
* Compact dsInit daemon configuration files
* Automatic generation of init scripts
* Debian `/etc/init.d/` integration
* Central dsInitd startup mode
* DSI and SYS registration modes
* Automatic runlevel registration using `update-rc.d`
* Service status overview
* Runtime status detection
* Interactive Screen access
* Bash completion support
* Built-in Perl POD documentation
* Safe service removal
* No systemd unit files required

### Directory structure

```text
/etc/dsInitd/
├── dsinitctl
├── SPECIFIKACIO.md
│
├── daemons/
│   └── ...
│
├── inits/
│   └── ...
│
├── init.d/
│   └── ...
│
└── templates/
    ├── standalone-template
    ├── dsInit-template
    ├── run-template
    ├── dsInitd-auto
    ├── completion
    ├── install
    ├── remove-test
    ├── test-standalone
    └── test-dsInit
```

### Requirements

The current implementation is primarily designed for Debian-based Linux systems.

Required packages:

```bash
apt install perl screen
```

Optional but recommended:

```bash
apt install bash-completion nano
```

The system uses traditional SysV init compatibility provided by Debian.

### Installation

Copy the `etc` directory from the release package directly to the root filesystem:

```bash
cp -a etc/. /etc/
```

Then run:

```bash
/etc/dsInitd/templates/install
```

The installer can configure:

* `/sbin/dsinitctl`
* Bash completion
* central dsInitd automatic startup

### Basic usage

Show help:

```bash
dsinitctl
```

or:

```bash
dsinitctl dsinitd-help
```

Create a compact dsInit daemon:

```bash
dsinitctl create-dsInit mydaemon
```

Short form:

```bash
dsinitctl cd mydaemon
```

Create a standalone daemon:

```bash
dsinitctl create-standalone mydaemon
```

Short form:

```bash
dsinitctl cs mydaemon
```

Start a daemon:

```bash
dsinitctl start mydaemon
```

Stop it:

```bash
dsinitctl stop mydaemon
```

Restart it:

```bash
dsinitctl restart mydaemon
```

Check status:

```bash
dsinitctl status mydaemon
```

Attach to its GNU Screen session:

```bash
dsinitctl show mydaemon
```

Detach from Screen with:

```text
Ctrl+A, then D
```

Restart and immediately attach:

```bash
dsinitctl reshow mydaemon
```

### Registration modes

dsInitd supports two mutually exclusive registration modes.

#### DSI mode

```bash
dsinitctl enable-dsi mydaemon
```

The service is managed by the central dsInitd startup system.

Disable it with:

```bash
dsinitctl disable-dsi mydaemon
```

#### SYS mode

```bash
dsinitctl enable-sys mydaemon
```

The service is registered as a traditional Debian SysV service under `/etc/init.d/`.

Disable it with:

```bash
dsinitctl disable-sys mydaemon
```

A daemon cannot be enabled in DSI and SYS mode at the same time.

### Automatic mode selection

You can also use:

```bash
dsinitctl enable mydaemon
```

and:

```bash
dsinitctl disable mydaemon
```

If the central dsInitd startup system is enabled, `enable` uses DSI mode.

Otherwise it uses SYS mode.

### Central dsInitd startup

Enable:

```bash
dsinitctl dsinitd-enable
```

Disable:

```bash
dsinitctl dsinitd-disable
```

Show the complete daemon overview:

```bash
dsinitctl dsinitd-status
```

Example:

```text
Central dsInitd: ENABLED

DAEMON                               TYPE         DSI      SYS      RUNLEVELS  SYSTEM   RUNNING
mydaemon                             dsInit       YES      NO       -          -        YES
```

### Removing a daemon

```bash
dsinitctl remove mydaemon
```

The command attempts to:

1. stop the daemon,
2. verify that it stopped,
3. remove its DSI/SYS registration,
4. remove its daemon definition.

Foreign files and unrelated init scripts are not intentionally overwritten or removed.

### Bash completion

The `dsinitctl` file is a Bash/Perl polyglot script.

To load completion directly into the current Bash session:

```bash
source dsinitctl
```

After that, TAB completion is available for commands and daemon names.

The completion definition itself is stored in:

```text
/etc/dsInitd/templates/completion
```

### dsInit daemon configuration

A minimal dsInit configuration looks like this:

```bash
PROCCESSCMD='ADD_COMMAND'
PROCCESSNAME="mydaemon"
RUNAS="root"

SHORTDESCRIPTION=""
DESCRIPTION=""

REQUIRED_START='$local_fs $remote_fs'
REQUIRED_STOP='$local_fs $remote_fs'
SHOULD_START='$network'
SHOULD_STOP='$network'
DEFAULT_START='2 3 4 5'
DEFAULT_STOP='0 1 6'
```

`PROCCESSCMD` is required.

The remaining values have defaults where applicable.

The complete runtime shell script is generated in memory and is not permanently written to disk.

### Standalone daemon

Standalone daemons contain their complete init logic and can also be used without `dsinitctl`.

They are normal executable SysV-compatible shell scripts.

For example, a standalone script can be copied directly to:

```text
/etc/init.d/mydaemon
```

and registered manually:

```bash
chmod 755 /etc/init.d/mydaemon
update-rc.d mydaemon defaults
systemctl daemon-reload
```

Later it can be disabled and removed with:

```bash
/etc/init.d/mydaemon stop
update-rc.d -f mydaemon remove
rm /etc/init.d/mydaemon
systemctl daemon-reload
```

### Documentation

Detailed Perl documentation:

```bash
perldoc /etc/dsInitd/dsinitctl
```

The full project specification is available in:

```text
/etc/dsInitd/SPECIFIKACIO.md
```

---

# Magyar

## Mi a dsInitd?

A **dsInitd** egy könnyű szolgáltatáskezelő rendszer Debian/Linux környezethez.

A hagyományos SysV init rendszerre és a GNU Screenre épül, és egyszerű módot biztosít hosszú ideig futó programok és saját daemonok létrehozására, indítására, leállítására, ellenőrzésére és automatikus rendszerindításra történő regisztrálására.

Kétféle szolgáltatást támogat:

* teljesen önálló standalone init scriptet;
* rövid dsInit konfigurációs fájlt.

## Főbb funkciók

* SysV-kompatibilis szolgáltatáskezelés
* GNU Screen alapú daemon futtatás
* központi `dsinitctl` vezérlő
* standalone daemonok
* kompakt dsInit konfigurációk
* automatikusan generált init scriptek
* Debian `/etc/init.d/` integráció
* központi dsInitd indítás
* DSI és SYS mód
* `update-rc.d` alapú futásiszint-kezelés
* futási állapot lekérdezése
* interaktív Screen hozzáférés
* Bash TAB-kiegészítés
* beépített Perl POD dokumentáció
* biztonságos daemon törlés
* külön systemd unit fájlok nélkül is használható

## Telepítés

A release csomag `etc` könyvtárát másold közvetlenül a rendszer gyökerébe:

```bash
cp -a etc/. /etc/
```

Ezután:

```bash
/etc/dsInitd/templates/install
```

A telepítő többek között felajánlja:

* a `/sbin/dsinitctl` létrehozását;
* a Bash completion telepítését;
* a központi dsInitd rendszerindítás engedélyezését.

## Alapvető használat

Segítség:

```bash
dsinitctl
```

Új dsInit daemon:

```bash
dsinitctl create-dsInit mydaemon
```

Röviden:

```bash
dsinitctl cd mydaemon
```

Standalone daemon:

```bash
dsinitctl create-standalone mydaemon
```

Röviden:

```bash
dsinitctl cs mydaemon
```

Indítás:

```bash
dsinitctl start mydaemon
```

Leállítás:

```bash
dsinitctl stop mydaemon
```

Újraindítás:

```bash
dsinitctl restart mydaemon
```

Állapot:

```bash
dsinitctl status mydaemon
```

Screen megnyitása:

```bash
dsinitctl show mydaemon
```

Leválás:

```text
Ctrl+A, majd D
```

## Engedélyezési módok

### DSI

```bash
dsinitctl enable-dsi mydaemon
```

A daemon a központi dsInitd rendszerindításon keresztül indul.

Tiltás:

```bash
dsinitctl disable-dsi mydaemon
```

### SYS

```bash
dsinitctl enable-sys mydaemon
```

A daemon hagyományos Debian SysV szolgáltatásként kerül regisztrálásra.

Tiltás:

```bash
dsinitctl disable-sys mydaemon
```

Egy daemon egyszerre csak az egyik módban lehet engedélyezve.

## Automatikus módválasztás

```bash
dsinitctl enable mydaemon
```

Ha a központi dsInitd engedélyezett, akkor DSI módot használ.

Ellenkező esetben SYS módot.

## Központi dsInitd

Engedélyezés:

```bash
dsinitctl dsinitd-enable
```

Tiltás:

```bash
dsinitctl dsinitd-disable
```

Teljes állapotlista:

```bash
dsinitctl dsinitd-status
```

## Daemon törlése

```bash
dsinitctl remove mydaemon
```

A program:

1. leállítja a szolgáltatást;
2. ellenőrzi a leállást;
3. eltávolítja a DSI/SYS regisztrációt;
4. törli a daemon definícióját.

## TAB-kiegészítés

A `dsinitctl` egy Bash/Perl polyglot fájl.

Az aktuális Bash terminálban a kiegészítés betöltése:

```bash
source dsinitctl
```

Ezután TAB-bal kiegészíthetők a parancsok és a daemonnevek.

## Dokumentáció

Részletes beépített dokumentáció:

```bash
perldoc /etc/dsInitd/dsinitctl
```

A teljes rendszer specifikációja:

```text
/etc/dsInitd/SPECIFIKACIO.md
```

---

## License

Choose the license that best fits the project before publishing the repository.

For an open-source system utility, common options include MIT, BSD-2-Clause, BSD-3-Clause and GPL-3.0.

