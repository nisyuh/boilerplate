A boilerplate to develop Redmine plugins.


# Requirements

bash, make, git, docker, docker-compose, docker-sync


# Initialization

Modify environment templates `.env.e.g.` or `envr/e.g./*` if necessary.
Then initialize with your Redmine version like

```sh
make init git_tag=4.0.9
```

You put template sources for a new plugin by

```sh
make plug-new name=my_plugin
```

in `redmine/plugins/my_plugin`.

