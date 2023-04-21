## Notes, issues, and workarounds

open issues:

### Support for Sailfish OS 4.5+

currently developed against SFOS 4.4.

### database tables are not automatically created.

   after running sync once, database exists. then do:

   ```
   devel-su -p sqlite3 $HOME/.local/share/system/privileged//Notifications/githubNotifications.db
   ```
   ```
   CREATE TABLE IF NOT EXISTS notifications (identifier INTEGER UNIQUE PRIMARY KEY AUTOINCREMENT,accountId INTEGER, typeStr TEXT, fromStr TEXT, repoStr TEXT, avatarUrl TEXT, url TEXT, createdTime INTEGER);
   ```

### Key Material (or rather, oauth material)

   1. create client id and client secret at github
   2. run   `sailfish-keyprovider-keygen xor xor_key <<appname>> <<clientid>> <<clientsecret>>`. You can use something else instead of `xor_key`, and need to set it as a `key` below:
   3. `devel-su -p vi $HOME/.local/share/system/privileged/Keys/storedkeys.ini`
   4. Enter this:

    ```
    [encoding]
    jolla/jolla-store/scheme=xor
    jolla/jolla-store/key=xor_key
    github/key=xor_key
    github/scheme=xor

    [encodedkeys]
    jolla/jolla-store/application_name=...
    jolla/jolla-store/client_id=...
    jolla/jolla-store/client_secret=...
    github/github-notifications/application_name=ENCRYPTED_APPNAME==
    github/github-notifications/client_id=ENCRYPTED_CLIENTID==
    github/github-notifications/client_secret=ENCRYPTED_CLIENTSECRET==
    github/application_name=ENCRYPTED_APPNAME==
    github/client_id=ENCRYPTED_CLIENTID==
    github/client_secret=ENCRYPTED_CLIENTSECRET==
    ```
Now you can add an account through the Settings application

### Lipstick:

apparently, the dconf key `/desktop/lipstick-jolla-home/events/auto_sync_feeds` enables social feeds. See `/usr/share/lipstick-jolla-home-qt5/eventsview/EventFeedAccountManager.qml`

