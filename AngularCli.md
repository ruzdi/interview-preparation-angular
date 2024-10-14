
# Angular CLI Commands Cheatsheet

This is a cheatsheet for the most commonly used **Angular CLI** commands, along with some details on how to use them. The Angular CLI is a powerful tool that helps developers scaffold, build, and manage Angular applications. This cheatsheet will serve as a handy reference for all the important CLI commands.

## Table of Contents

- [Creating a New Angular Project](#creating-a-new-angular-project)
- [Serving the Application](#serving-the-application)
- [Generating Components, Services, Modules, etc.](#generating-components-services-modules-etc)
- [Building the Application](#building-the-application)
- [Running Tests](#running-tests)
- [Linting](#linting)
- [Adding Libraries or Features](#adding-libraries-or-features)
- [Updating Angular](#updating-angular)
- [Other Useful Commands](#other-useful-commands)

## Creating a New Angular Project

```bash
ng new <project-name>
```
- Creates a new Angular project.
- Example: `ng new my-app`

## Serving the Application

```bash
ng serve
```
- Runs the application on a development server.
- Default server URL: `http://localhost:4200`
- You can specify a different port: `ng serve --port 4300`

## Generating Components, Services, Modules, etc.

### Components

```bash
ng generate component <component-name>
```
- Shortcut: `ng g c <component-name>`
- Example: `ng g c home` (generates a `home` component)

### Services

```bash
ng generate service <service-name>
```
- Shortcut: `ng g s <service-name>`
- Example: `ng g s auth` (generates an `auth` service)

### Modules

```bash
ng generate module <module-name>
```
- Shortcut: `ng g m <module-name>`
- Example: `ng g m admin` (generates an `admin` module)

### Other Files

- **Directives**: `ng g d <directive-name>`
- **Pipes**: `ng g p <pipe-name>`
- **Guards**: `ng g g <guard-name>`
- **Classes**: `ng g class <class-name>`
- **Interfaces**: `ng g interface <interface-name>`
- **Enums**: `ng g enum <enum-name>`

## Building the Application

```bash
ng build
```
- Builds the app for production. Output is stored in the `dist/` folder.
- For a production build: `ng build --prod`

### Build with a Specific Base URL

```bash
ng build --prod --base-href <URL>
```
- Example: `ng build --prod --base-href /my-app/`

## Running Tests

### Unit Tests

```bash
ng test
```
- Runs unit tests using Karma.

### End-to-End Tests

```bash
ng e2e
```
- Runs end-to-end tests using Protractor.

## Linting

```bash
ng lint
```
- Lints the code to ensure adherence to coding standards.

## Adding Libraries or Features

```bash
ng add <package-name>
```
- Adds and configures packages to the project.
- Example: `ng add @angular/material`

## Updating Angular

```bash
ng update
```
- Updates Angular and its dependencies to the latest version.
- Example: `ng update @angular/core @angular/cli`

## Other Useful Commands

### Display Angular CLI Version

```bash
ng --version
```
- Displays the current version of Angular and CLI.

### Generate Routing Module

```bash
ng generate module <module-name> --routing
```
- Generates a new module with a routing file.
- Example: `ng g m users --routing`

### Lazy Load a Module

```bash
ng generate module <module-name> --route <route-name> --module <parent-module>
```
- Example: `ng g m admin --route admin --module app.module`

### Serve the App on a Different Port

```bash
ng serve --port <port-number>
```
- Example: `ng serve --port 4300`

## Conclusion

This Angular CLI cheatsheet covers the most common and useful commands that you will need while working with Angular projects. You can customize or expand these commands based on your needs.
