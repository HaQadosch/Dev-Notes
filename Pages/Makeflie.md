# Rules to build one

ttps://www.gnu.org/software/make/manual/html_node/index.html

# React template

```sh
# The makefile is a elegant tool for a more civilised age.
# https://news.ycombinator.com/item?id=11195539
# make build | make2tap | tap-format-spec`
# http://i.imgur.com/chs0Jf3.png

.PHONY: git cra help core pre-commit story clean move buildspecs ci templates git-push

skeleton-repo = cx-project-skeleton-s3-repo
git-skeleton = git@github.com:ecs-cx/$(skeleton-repo).git

project = my-little-pony
git-project = git@github.com:ecs-cx/$(project)-repo.git
project-repo = $(project)-repo
project-folder = $(project-repo)/src/$(project)

all: git move buildspecs ci templates cra core pre-commit story preset-cra clean git-push

# Rule for displaying a list of command along with a one line info
# The info is whatever starts with ## in front of the rule.
# run with: >make how 
how:
	@echo "This is the help command"
	@echo "To start, run make without argument"
	@echo ""
	@grep -E '^[a-zA-Z0-9_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'


git: ## Clone the skeleton repo and the project one in the same folder as the current makefile.
	git clone $(git-skeleton)
	git clone $(git-project)
	cd $(project-repo)/ && git checkout -b first-import

move: ## Move the files needed for aws build into the new project as well as the gitignore for Node projects
	cp -R $(skeleton-repo)/buildspecs $(project-repo)/
	cp -R $(skeleton-repo)/ci $(project-repo)/
	cp -R $(skeleton-repo)/templates $(project-repo)/
	cp $(skeleton-repo)/.gitignore $(project-repo)/

buildspecs: ## update the skeleton-app to the actual project name
	mv $(project-repo)/buildspecs/buildspec-s3.yml $(project-repo)/buildspecs/buildspec-s3-pre.yml
	sed 's/s3-skeleton-app/$(project)/g' $(project-repo)/buildspecs/buildspec-s3-pre.yml > $(project-repo)/buildspecs/buildspec-s3.yml
	rm $(project-repo)/buildspecs/buildspec-s3-pre.yml

ci: ## Rename ci file using the project name
	mv $(project-repo)/ci/cx-project-skeleton-s3-dev.eu-central-1.cf-config.json $(project-repo)/ci/$(project)-dev.eu-central-1.cf-config.json 

templates: ## Rename the template file using the project name
	mv $(project-repo)/templates/cx-project-skeleton-s3.cf-template.yml $(project-repo)/templates/$(project).cf-template.yml

core:
	npm i --prefix $(project-folder)/ core-js

pre-commit:
	npm i --prefix $(project-folder)/ -D husky prettier pretty-quick
	mv $(project-folder)/package.json $(project-folder)/package.old.json
	sed 's/private": true,/private": true,\
	"homepage": ".\/",\
	"husky": \{\
		"hooks": \{\
			"pre-commit": "pretty-quick --staged \&\& CI=true npm run test"\
		\}\
	\},/' $(project-folder)/package.old.json > $(project-folder)/package.json

cra: ## Create a $(project) app using create-react-app with TS option on.
	npx create-react-app $(project-folder) --template typescript

story: ## Add storybook with type detection based on the package.json
	npm i --prefix $(project-folder)/ @storybook/cli
	cd $(project-folder) && npx sb init --type react
	npm i --prefix $(project-folder)/ -D @storybook/react @storybook/preset-create-react-app

preset-cra: ## Configure storybook to work with create-react-app and typescript
	touch $(project-folder)/.storybook/presets.js
	echo "module.exports = ['@storybook/preset-create-react-app'];" > $(project-folder)/.storybook/presets.js

# yarn install --modules-folder <path>
clean: 
	rm -rf $(skeleton-repo)

git-push:
	cd $(project-repo)/ && git add . && git commit -m "first import" && git push origin first-import
```

# Storybook template

# Aliases and Usual scripts
