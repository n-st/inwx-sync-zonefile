Unofficial INWX zone file exporter/importer
===========================================

The domain registrar [INWX](https://www.inwx.de/) offers DNS hosting for
domains registered with them, but currently does not support importing the DNS
records automatically (e.g. when making bulk changes or migrating from another
registrar).

This script uses INWX's ["DomRobot" API](https://www.inwx.de/en/help/apidoc) to
automate the process of exporting and importing zone files.

Initial setup
-------------

Optionally create a Python virtualenv to work in:

    python3 -mvenv venv
    source venv/bin/activate

Install dependencies (they are currently specified at fixed versions; you may
be able to use older/newer versions, but those are untested):

    pip install -r requirements.txt

Add your INWX username and password to your local user's [`.netrc`
file](https://everything.curl.dev/usingcurl/netrc):

    echo 'machine api.domrobot.com login jane.doe password hunter2' >> ~/.netrc

Usage
-----

The recommended process is to first dump the current zone:

    ./inwx-sync-zonefile example.com -o example.com.zone

Then copy the zone file, and make any changes you need:

    cp example.com.zone new-example.com.zone
    $EDITOR new-example.com.zone

Simulate importing the modified zone (this should reveal any syntax or access errors):

    ./inwx-sync-zonefile example.com -i new-example.com.zone --dry-run

Finally import the modified zone:

    ./inwx-sync-zonefile example.com -i new-example.com.zone --dry-run
