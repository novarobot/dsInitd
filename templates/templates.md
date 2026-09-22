# templates

A `templates/` mappa a dsInitd sablonjait és segédprogramjait tartalmazza.

* `standalone-template` – teljes, önálló SysV/Screen daemon sablon.
* `dsInit-template` – rövid dsInit konfigurációs sablon.
* `run-template` – dsInit szolgáltatások generált DSI/SYS indítójának sablonja.
* `dsInitd-auto` – a DSI módban engedélyezett szolgáltatások központi indítója.
* `dsInitd-auto.enabled` – symlink, amely jelzi, hogy a központi dsInitd indítás engedélyezett.
* `completion` – Bash TAB-kiegészítés a `dsinitctl` parancshoz.
* `install` – a dsInitd telepítő és alapbeállító scriptje.
* `remove-test` – az install által létrehozott tesztdaemonok eltávolítása.
* `test-standalone` – standalone tesztdaemon sablon.
* `test-dsInit` – dsInit tesztdaemon konfigurációs sablon.
