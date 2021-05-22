init:
	@make red-source
	@make env build up
	@make red-setup

rebuild:
	@make red-conf
	@make env build up
	@make red-setup

env:
	\cp -f .env.e.g. .env
	\cp -f envr/e.g./* envr/

up:
	@make sync-up
	docker-compose up -d

build:
	docker-compose build --no-cache --force-rm

stop:
	docker-compose stop

down:
	docker-compose down --remove-orphans
	@make sync-down

drop:
	docker-compose down --rmi all --volumes --remove-orphans

drop-volume:
	docker-compose down --volumes --remove-orphans

sync-up:
	docker-sync start

sync-down:
	docker-sync clean

sync:
	docker-sync sync

bundle-install:
	docker-compose exec redmine bundle install

bundle-exec:
	docker-compose exec redmine bundle exec $(cmd)

rake:
	docker-compose exec redmine bundle exec rake $(task)

rails:
	docker-compose exec redmine bundle exec rails $(cmd)

rails-c:
	docker-compose exec redmine bundle exec rails console

rails-s:
	docker-compose exec redmine bundle exec rails server --binding=0.0.0.0

red-source:
	@make red-clone red-switch red-conf

red-setup:
	@make bundle-install red-gen-secret db-init

red-clone:
	git clone https://github.com/redmine/redmine.git

red-switch:
ifneq ($(strip $(git_brabch)),)
	cd redmine && git switch $(strip $(git_brabch))
else ifneq ($(strip $(git_tag)),)
	cd redmine && git switch -c $(strip $(git_tag)) refs/tags/$(strip $(git_tag))
else ifneq ($(strip $(git_commit)),)
	cd redmine && git switch -c $(strip $(git_commit))
else
	$(error git commitish name is not set)
endif

red-conf:
	\cp -f dock/redmine/config/e.g./* redmine/config/

red-gen-secret:
	@make rake task=generate_secret_token

red-load-default:
	@make rake task=redmine:load_default_data

db-create:
	@make rake task=db:create

db-migrate:
	@make rake task=db:migrate

db-init:
	@make db-create db-migrate red-load-default

plug-new:
ifneq ($(strip $(name)),)
	@make rails cmd="generate redmine_plugin $(name)"
else
	$(error plugin name is not set)
endif

plug-new-model:
ifneq ($(strip $(plug)),)
  ifneq ($(strip $(model)),)
	@make rails cmd="generate redmine_plugin_model $(plug) $(model)"
  else
	$(error model profile is not set)
  endif
else
	$(error plugin name is not set)
endif

plug-new-ctrl:
ifneq ($(strip $(plug)),)
  ifneq ($(strip $(ctrl)),)
	@make rails cmd="generate redmine_plugin_controller $(plug) $(ctrl)"
  else
	$(error controller profile is not set)
endif
else
	$(error plugin name is not set)
endif

plug-migrate:
ifneq ($(strip $(name)),)
	docker-compose exec redmine bundle exec rake redmine:plugins:migrate NAME=$(strip $(name))
else
	docker-compose exec redmine bundle exec rake redmine:plugins:migrate
endif
