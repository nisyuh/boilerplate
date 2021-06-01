A boilerplate to develop [Redmine][redmine] plugins.


# Requirements

bash, make, git, docker, docker-compose, docker-sync


# Initialization

Modify environment templates `.env.e.g.` or `envr/e.g./*` if necessary.
Then initialize with your Redmine version like

```sh
make init git_tag=4.0.9
```

You put templates for a new plugin by

```sh
make plug-new name=my_plugin
```

in `redmine/plugins/my_plugin`.


# Composition

Currently it goes on composing:

- [Ruby][ruby-dockhub] 2.6;
- [PostgreSQL][potgres-dockhub] 9.6;
- [Mailhog][mailhog-dockhub] 1.0.1.

See `docker-compose.yml` or `dock/**` for the details.

## Networks

We publish ports for test use.

| Host                | Containers                          |
| ------------------- | ----------------------------------- |
| `localhost:3300`    | Redmine's HTTP serving (port: 3000) |
| `localhost:8025`    | Mailhog's HTTP serving (port: 8025) |

## Volumes (minimally noting)

Remember the directory `redmine/` synchronizes with the source in the Redmine container including plugins.

## Command `make` Examples

`Makefile` tells you some shorthand to use.

Those might not be sufficient or necessary for you, so be cutomized in your own environment.

- `init {git_branch|git_tag|git_commit}=`:

    Initialization, git-clone Redmine source, build docker images up, and install Redmine.
    Require the valid Redmine version as git-commitish name.

- `reinit`:

    Re-initialization, which reflects a corrected Redmine version by hand in `redmine/`,
    or changes on docker composition, etc., after the first `init`.

- `env`:

    Overwriting copy enviroment templates so that docker could reflect them to images or containers for the next run.

- `up`:

    Start docker and docker-sync containers.

- `down`:

    Stop and remove docker and docker-sync containers.

- `bundle-exec cmd=`:

    Execute `bundle exec` command on a Redmine container.
    Require a command to be appended to `bundle exec` entry,
    e.g., `make bundle-exec cmd="runner hello.rb"` executes `bundle exec runner hello.rb`.

- `rake task=`:

    Execute `bundle exec rake` command on a Redmine container.
    Require a task name to be appended to `bundle exec rake` entry,
    e.g., `make rake task=log:clear` executes `bundle exec rake log:clear`.

- `rails cmd=`:

    Execute `bundle exec rails` command on a Redmine container.
    Require a command to be appended to `bundle exec rails` entry.

- `rails-c`:

    Attach to rails console.

- `rails-s`:

    Start rails server.

- `plug-new name=`:

    Create a set of new plugin's sources, see [the document][plug-new].
    Require a plugin name.

- `plug-new-model plug= model=`:

    Create a set of new plugin's model sources, see [the document][plug-new-model].
    Require a plugin name and model profile.

- `plug-new-ctrl plug= ctrl=`:

    Create a set of new plugin's controller sources, see [the document][plug-new-ctrl].
    Require a plugin name and controller profile.

- `plug-migrate [name=]`:

    Execute migration for plugins, see [the document][plug-migrate].
    Accept a plugin name ommitable to migrate for all plugins.



[ruby-dockhub]: https://hub.docker.com/_/ruby
[potgres-dockhub]: https://hub.docker.com/_/postgres
[mailhog-dockhub]: https://hub.docker.com/r/mailhog/mailhog
[redmine]: https://www.redmine.org/
[plug-new]: https://www.redmine.org/projects/redmine/wiki/plugin_tutorial#Creating-a-new-Plugin
[plug-new-model]: https://www.redmine.org/projects/redmine/wiki/plugin_tutorial#Generating-a-model
[plug-new-ctrl]: https://www.redmine.org/projects/redmine/wiki/plugin_tutorial#Generating-a-controller
[plug-migrate]: https://www.redmine.org/projects/redmine/wiki/plugins
