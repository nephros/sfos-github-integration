## GitHub integration for Sailfish OS

WIP.

Current goal is to have Notifications integrated/shown in Events view

### Status:

 - Alpha: Account creation via libsocialcache and buteo-sync-plugins-social
 - Alpha: downsync of notifications via msyncd and buteo-sync-plugins-social
 - Done: display of notifications in Lipstick/Eventsfeed
 - Missing: interaction/opening of notifications from events view

### Issues:

See https://github.com/nephros/sfos-github-integration/issues

### Installation

You need:

	- jolla-settings-accounts-extensions-github: provides the account provider, sync profile and settings UI
	- buteo-sync-plugins-social-github: for the buteo syncer plugin
	- libsocialcache: provides the database and sync infrastructure
	- libsocialcache-qml-plugin: QML interface/model to the buteo database
	- eventsview-extensions-github: lipstick extension to display notifications in the Events view

`buteo-sync-plugins-social-github`, `libsocialcache` can e.g. be built on OBS.

`eventsview-extensions-github` and `jolla-settings-accounts-extensions-github`
are just QML and do not really need any build infrastrcture, and can be build
on-device by running the `build.sh` script in the repo. However, they should
build in SDK/OBS as well.

See the Issues about missing bits you have to do maually before it will work.

### Debugging hints

```
systemctl stop msyncd --user
devel-su -p
MSYNCD_LOGGING_LEVEL=6 msyncd # level 7 and 8 for trace
# now go to Settings->Accounts->Github and initiate a sync
# Ctrl-C to kill msyncd
# Ctrl-D to exit privileged shell
systemctl start msyncd --user
```

```
devel-su -p
sqlite3 -readonly $HOME/.local/share/system/privileged//Notifications/githubNotifications.db
```

```
devel-su -p
ag-tool list-accounts | grep github
ag-tool list-services <accountId>
ag-tool list-settings <accountId>
ag-tool update-account <accountId> string:displayName="My GitHub"
```

