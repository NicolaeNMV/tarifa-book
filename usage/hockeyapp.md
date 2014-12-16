# hockeyapp

`tarifa hockeyapp` allows to interact with [http://hockeyapp.net](hockeyapp.net) for publishing apps to testers.

```
Usage: tarifa hockeyapp <command>

Commands:

    version list <platform> <config>
        Fetch the list of hockeyapp versions of <config> env for given platform.

    version upload <platform> <config> [options]
        Upload a version of <config> env package to hockeyapp for given platform.
        Options can be any of 'notes', 'notify', 'status', 'tags', 'teams', 'users',
        'commit_sha', 'build_server_url', 'repository_url'.
        See http://support.hockeyapp.net/kb/api/api-versions#upload-version for details.

    version update <platform> <config> [options]
        Modify last version of given platform for <config> env. You can only
        modify metadatas; you can't upload a new package.
        Options can be any of 'notes', 'notify', 'status', 'tags', 'teams', 'users',
        'commit_sha', 'build_server_url', 'repository_url'.
        See http://support.hockeyapp.net/kb/api/api-versions#update-version for details.

    version clean <keep>
        Clean all versions of all apps found in tarifa.json; specify the number
        of versions to keep (default: 3).

Options:

    --help, -h     Show this message
    --verbose, -V  Be more verbose on everything
```
