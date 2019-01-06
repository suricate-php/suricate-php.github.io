# Directory structure

- [The Root directory](#the-root-directory)
- [The bootstrap directory](#the-bootstrap-directory)
- [The config directory](#the-config-directory)

## The Root Directory

#### The `bootstrap` directory

Two files are located in this directory.
The `paths.php` file, is called during Suricate initialisation. It's only used to define the `app`, `public` and `base` directories automatically

`start.php` file is the entrypoint of the application.
Suricate is initialised in it, and configuration files are referenced here.  
As this file is the entrypoint of the app, session management, global includes files or routing middleware should be inserted here.

#### The `config` directory

#### The `controllers` directory

#### The `events` directory

#### The `i18n` directory

#### The `models` directory

#### The `storage` directory

#### The `views` directory

#### The `worker` directory
