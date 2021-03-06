# Cog Setup

## System Requirements

This setup is tested for OSX and Linux. nvm and avn are not compatible with Windows. On Windows you may be able to use something like [nvm-windows](https://github.com/coreybutler/nvm-windows)

## Bash script local environment setup

This sub theme uses node and npm to manage build tools such as sass. There are no additional front-end dependencies such as ruby. It is highly recommended to use [nvm](https://github.com/creationix/nvm) to install and manage node versions, and the theme provides a bash install script to install both nvm and the version of node that the theme uses, also via nvm.

Script usage (from generated sub theme directory):

```bash
./install-node.sh 8.9.1
```

The install script installs nvm and then uses nvm to install the version of node given to it as an argument. It also writes a `.node-version` file into the sub theme directory it was run from that can be picked up by tools such as avn. After running the script it is necessary to do the command it outputs to tell your session you want to use that version of node:

```bash
source ~/.bashrc && nvm use --delete-prefix 8.9.1
```

If you are not using avn or something similar then you will need to repeat `nvm use 8.9.1` if you close and reopen your session.

## AVN (Automatic Version Switching for Node)

### Why would you want to install AVN?

By default, nvm is session based so you will need to run `nvm use 8.9.1` again if you close your terminal and reopen it later. It is recommended to use a tool such as [avn](https://github.com/wbyoung/avn). This will pick up the node version from the `.node-version` file that the provided install script places in the theme directory. When you `cd` to the theme directory avn will use nvm to switch to the appropriate installed node version automatically.

### AVN install:

```bash
npm install -g avn avn-nvm
avn setup
source ~/.bash_profile
```

## 'Manual' local environment setup

The only requirement for running the theme build is node and running `npm install` in the theme to install dependencies. We have tested against Node LTS release v4. Installers are available here https://nodejs.org/en/download/ for multiple operating systems.

## Local development

The theme build is centered around the gulpfile, and the default gulp task does a production ready build of the theme. The directories that are generated in the build are `css` and `styleguide`. JS is linted via ESLint.

First install all local dependencies. From the subtheme folder:

```bash
npm run install-tools
```

To run gulp there is a built in npm run-script:

```bash
npm run build
```

## Gulp

Gulp is already included once `npm run install-tools` has been run, but unless it is installed globally you will need to use the npm scripts such as `npm run build` to call it. You can also install a global version of gulp to run gulp tasks directly:

```bash
npm install -g gulp-cli
```

After gulp is installed globally it can be called like `gulp`, `gulp build` or `gulp watch`. The gulpfile documents available tasks outside of the default `gulp` task, which runs a production ready theme build. You can use `gulp --tasks` to get a list of these tasks as well. You can also use `npm run` to see which npm scripts are available and what commands they call.

## Browsersync

[Browsersync](https://www.browsersync.io/) is configured to allow for multi-device testing and to automatically refresh the page when changes are made to your theme's styles and javascript. You can start Browsersync with `npm run serve`, or `gulp serve` if Gulp has been installed globally. The serve task will automatically start the watch task in the background, so there is no need to run them both.

In order to see changes to your styles and javascript immediately, you will need to disable Drupal's CSS and JS preprocessing. Follow the instructions at the bottom of `sites/default/settings.php` to create a `local.settings.php` file. In your local settings file, include the following:

```
/**
 * Disable CSS and JS aggregation.
 */
$config['system.performance']['css']['preprocess'] = FALSE;
$config['system.performance']['js']['preprocess'] = FALSE;
```
