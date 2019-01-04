# Gitstrap.
Command line tool to bootstrap Github repository


[![DevOps By Rultor.com](http://www.rultor.com/b/g4s8/gitstrap)](http://www.rultor.com/p/g4s8/gitstrap)

[![Build Status](https://img.shields.io/travis/g4s8/gitstrap.svg?style=flat-square)](https://travis-ci.org/g4s8/gitstrap)

[![PDD status](http://www.0pdd.com/svg?name=g4s8/gitstrap)](http://www.0pdd.com/p?name=g4s8/gitstrap)
[![License](https://img.shields.io/github/license/g4s8/gitstrap.svg?style=flat-square)](https://github.com/g4s8/gitstrap/blob/master/LICENSE)

## About
This tool automates routine operations when creating new Github repository.
It can create and configure Github repository from `yaml` configuration file.
Gitstrap helps to:
 1. Create new repository on Github
 2. Sync with local directory
 3. Apply templates (such as README with badges, CI configs, LICENSE stuff, etc)
 4. Configure webhooks for Github repo
 5. Invite collaborators

## Usage
To bootstrap new repository:
 1. Create new directory for your project
 2. Write `.gitstrap.yaml` config in project root
 3. Create Github API token with repo permissions (if doesn't exist)
 4. Run `gitstrap -token=your-api-token create`

To remove repository (keep source files) run `gitstrap -token=your-api-token destroy`

## Configuration
The default configuration file is `.gitstrap.yaml`, you can specify another file with `-config=my-config.yaml` option.

Each config should include `gitstrap` key with `version` `v1`. 
`gitstrap` section must include `github` section and may include `templates` and `params` sections:
```yaml
gitstrap:
    version: v1
    github:
        repo:
            # github repository name
            name: gitstrap
            # github repository description
            description: "Command line tool to bootstrap Github repository"
            # (optional, default false) true if private
            private: false
            # github webhooks
            hooks:
                  # webhook url
                - url: "http://p.rehttp.net/http://www.0pdd.com/hook/github"
                  # webhook type: form or json
                  type: form
                  # events to send (see github docs)
                  events:
                      - push
                  # (optional, default true) false to create webhook in inactive state
                  active: true
            # github logins to add as collaborators
            collaborators:
                - "rultor"
                - "0pdd"
    # (optional) templates to apply
    templates:
          # file name in repository
        - name: "README.md"
          # template file name
          location: "/home/g4s8/.gitstrap/README.md"
        - name: "LICENSE"
          location: "/home/g4s8/.gitstrap/LICENSE.mit"
    # (optional) these params can be accessed from template, just a key-value pairs
    params:
        rultor: true
        travis: true
        readmeContrib: |
            Fork repository, clone it, make changes,
            push to new branch and submit a pull request.
        pdd: true
        license: MIT
```

## Templates
You can create any file templates to apply them using project configuration.
Templates can use [golang template](https://golang.org/pkg/text/template/) patterns.

*Example: README.md*
```markdown
# {{.Repo.Name}}.
{{.Repo.Description}}

{{if .Gitstrap.Params.eoPrinciples }}[![EO principles respected here](http://www.elegantobjects.org/badge.svg)](http://www.elegantobjects.org){{end}}
{{if .Gitstrap.Params.rultor}}[![DevOps By Rultor.com](http://www.rultor.com/b/{{.Repo.Owner.Login}}/{{.Repo.Name}})](http://www.rultor.com/p/{{.Repo.Owner.Login}}/{{.Repo.Name}}){{end}}

{{if .Gitstrap.Params.travis}}[![Build Status](https://img.shields.io/travis/{{.Repo.Owner.Login}}/{{.Repo.Name}}.svg?style=flat-square)](https://travis-ci.org/{{.Repo.Owner.Login}}/{{.Repo.Name}}){{end}}
{{if .Gitstrap.Params.appveyor}}[![Build status](https://ci.appveyor.com/api/projects/status/{{.Gitstrap.Params.appveyor}}?svg=true)](https://ci.appveyor.com/project/{{.Repo.Owner.Login}}/{{.Repo.Name}}){{end}}
{{if .Gitstrap.Params.pdd}}[![PDD status](http://www.0pdd.com/svg?name={{.Repo.Owner.Login}}/{{.Repo.Name}})](http://www.0pdd.com/p?name={{.Repo.Owner.Login}}/{{.Repo.Name}}){{end}}
{{if .Gitstrap.Params.license}}[![License](https://img.shields.io/github/license/{{.Repo.Owner.Login}}/{{.Repo.Name}}.svg?style=flat-square)](https://github.com/{{.Repo.Owner.Login}}/{{.Repo.Name}}/blob/master/LICENSE){{end}}
{{if .Gitstrap.Params.codecov}}[![Test Coverage](https://img.shields.io/codecov/c/github/{{.Repo.Owner.Login}}/{{.Repo.Name}}.svg?style=flat-square)](https://codecov.io/github/{{.Repo.Owner.Login}}/{{.Repo.Name}}?branch=master){{end}}
```
this template uses [`.gitstrap.yaml`](https://github.com/g4s8/gitstrap/blob/master/.gitstrap.yaml) config and produces `README.md`:
```markdown
# gitstrap.
Command line tool to bootstrap Github repository

[![DevOps By Rultor.com](http://www.rultor.com/b/g4s8/gitstrap)](http://www.rultor.com/p/g4s8/gitstrap)

[![Build Status](https://img.shields.io/travis/g4s8/gitstrap.svg?style=flat-square)](https://travis-ci.org/g4s8/gitstrap)

[![PDD status](http://www.0pdd.com/svg?name=g4s8/gitstrap)](http://www.0pdd.com/p?name=g4s8/gitstrap)
[![License](https://img.shields.io/github/license/g4s8/gitstrap.svg?style=flat-square)](https://github.com/g4s8/gitstrap/blob/master/LICENSE)
```

## Contribution
Fork repository, clone it, make changes,
push to new branch and submit a pull request.
