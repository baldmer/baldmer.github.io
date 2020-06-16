---
layout: post
title: Angular, a New Tool for my ToolBox
tags: [Linux, Node, Angular]
---


During the past week, I have been learning `Angular`, a framework for the so-called Front-End application development. I heard about it in the past, but I did not explore it properly until recently that I had to look at a real project. When I first opened the project I found several files with many configuration instructions. I was already familiar with the `Node.js` package manager. But I still could not find a way to understand the structure of the project. This was the point when I started reading the `Angular`'s documentation.

The documentation was very helpful to grasp some notions about the framework. For example, it is recommended to be familiar with `JavaScript`, `HTML` and `CSS` and have a recent version of `Node.js` and `npm`. There is a new language called `Typescript` on which `Angular` is written in, which I also had to learn about it. After I read some basic concepts such as Modules, Components, and Services, I stared a tutorial called "Tour Of Heroes". This tutorial is very well explained and helpful to understand how a real application works. Most of the code and ideas that I needed to understand for my project were in this tutorial. Specially
the existence of a Command Line Interface (CLI) that I will document on this post for my future reference.

#### Install Angular CLI


The CLI can be installed globally with the following command (requires sudo):

```bash
$ sudo npm install -g @angular/cli
```

This command will install the CLI in the path `/usr/local/lib/node_modules/@angular/` and create a the tool named `ng` in `/usr/local/bin/`.


#### Create new project

The following command will create the base structure of the project.

```bash
$ ng new my-project

? Would you like to add Angular routing? No
? Which stylesheet format would you like to use? CSS
CREATE my-project/README.md (1026 bytes)
CREATE my-project/.editorconfig (274 bytes)
CREATE my-project/.gitignore (631 bytes)
CREATE my-project/angular.json (3598 bytes)
CREATE my-project/package.json (1243 bytes)
CREATE my-project/tsconfig.json (489 bytes)
CREATE my-project/tslint.json (3125 bytes)
CREATE my-project/browserslist (429 bytes)
CREATE my-project/karma.conf.js (1022 bytes)
CREATE my-project/tsconfig.app.json (210 bytes)
CREATE my-project/tsconfig.spec.json (270 bytes)
CREATE my-project/src/favicon.ico (948 bytes)
CREATE my-project/src/index.html (295 bytes)
CREATE my-project/src/main.ts (372 bytes)
CREATE my-project/src/polyfills.ts (2835 bytes)
CREATE my-project/src/styles.css (80 bytes)
CREATE my-project/src/test.ts (753 bytes)
CREATE my-project/src/assets/.gitkeep (0 bytes)
CREATE my-project/src/environments/environment.prod.ts (51 bytes)
CREATE my-project/src/environments/environment.ts (662 bytes)
CREATE my-project/src/app/app.module.ts (314 bytes)
CREATE my-project/src/app/app.component.css (0 bytes)
CREATE my-project/src/app/app.component.html (25725 bytes)
CREATE my-project/src/app/app.component.spec.ts (954 bytes)
CREATE my-project/src/app/app.component.ts (214 bytes)
CREATE my-project/e2e/protractor.conf.js (808 bytes)
CREATE my-project/e2e/tsconfig.json (214 bytes)
CREATE my-project/e2e/src/app.e2e-spec.ts (643 bytes)
CREATE my-project/e2e/src/app.po.ts (301 bytes)
✔ Packages installed successfully.
    Successfully initialized git.

```

#### Run the app

To run a server and see our application running, we use:

```bash
$ cd my-project/
$ ng serve --open
```

#### Generate Interface

The following will create a file where we can declare an interface:

```bash
$ ng generate interface interface-name
```

or:

```bash
$ ng g i interface-name
```

#### Generate a service

In these files, we place the logic related to our data sources.

```bash
$ ng generate service service-name
```
or:

```bash
$ ng g s service-name
```

We can generate the file in a specific path as follows:

```bash
$ ng generate service parentdir/name --flat --spec=false
```
We use `--flat` to put the file in the parent directory instead of its own folder and `--spec=false` to prevent the generation of a unit test file (extension `.spec.ts`).

When we execute the following:

```bash
$ ng generate service InMemoryData
```

the generated file will be of the form `in-memory-data.service.ts` in `src/app/`.

#### Generate component

Use the following command:

```bash 
$ ng generate component comp-name
```

or 

```bash
$ ng g c comp-name
```

Will generate three files: a `.css`, in my case the selected stylesheet format, a `.html` with `Angular` directives and binding markup, a `.ts` `Typescript` file with application logic, and a `.spec.ts` for testing, e.g:

```bash
$ ng generate component comp-name
CREATE src/app/comp-name/comp-name.component.css (0 bytes)
CREATE src/app/comp-name/comp-name.component.html (24 bytes)
CREATE src/app/comp-name/comp-name.component.spec.ts (643 bytes)
CREATE src/app/comp-name/comp-name.component.ts (286 bytes)
UPDATE src/app/app.module.ts (406 bytes)

```

#### Generate module

```bash
$ ng generate module module-name
```
or
```bash
$ ng g m module-name
```

Will create a folder with the name of the module, e.g.:

```bash
$ ng g m modules
CREATE src/app/modules/modules.module.ts (193 bytes)
```

We can also specify additional options here, e.g: use `--module=app` to tell the CLI to register it in the import array of the AppModule. 

This command can be also used to generate the routing file, e.g.:

```bash
$ ng g m app-routing --flat --module=app

CREATE src/app/app-routing.module.ts (196 bytes)
UPDATE src/app/app.module.ts (393 bytes)
```


#### Versions

The versions of the tools I used were:

* Node: v12.18.0
* npm: 6.14.5
* Angular: 9.1.9
* Angular: CLI 9.1.7


#### References

[1] - https://angular.io/docs

 








