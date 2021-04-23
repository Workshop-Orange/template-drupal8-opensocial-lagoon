# Composer template for OpenSocial (Druapl 8 based) projects hosted on amazee.io

This project template should provide a kickstart for managing your site
dependencies with [Composer](https://getcomposer.org/). It is based on the [amazee.io Drupal Example Simple template](github.com/amazeeio/drupal-example-simple/), and should include everything necessary to run on amazee.io (either the local development environment or on amazee.io servers.)

## Requirements

* [docker](https://docs.docker.com/install/)
* [pygmy](https://pygmy.readthedocs.io/) `gem install pygmy` (you might need `sudo` for this depending on your Ruby configuration)

**OR**

* [Lando](https://docs.lando.dev/basics/installation.html#system-requirements)

## Local environment setup - pygmy

1. Checkout this project repo and confirm the path is in Docker's file sharing config - https://docs.docker.com/docker-for-mac/#file-sharing

    ```bash
    git clone https://github.com/workshop-orange/template-drupal8-opensocial-lagoon.git opensocial-d8-lagoon && cd $_
    ```

2. Make sure you don't have anything running on port 80 on the host machine (like a web server) then run `pygmy up`

3. Build and start the build images:

    ```bash
    docker-compose up -d
    docker-compose exec cli composer install
    ```

4. Install your OpenSocial site with Drush:

```bash
docker-compose exec cli drush si social -y
```

5. Visit the new site @ `http://template-drupal8-opensocial-lagoon.docker.amazee.io`

* If any steps fail, you're safe to rerun from any point.
Starting again from the beginning will just reconfirm the changes.

## Local environment setup - Lando

This repository is set up with a `.lando.yml` file, which allows you to use Lando instead of pygmy for your local development environment.

1. [Install Lando](https://docs.lando.dev/basics/installation.html#system-requirements).

2. Checkout the project repo and confirm the path is in Docker's file sharing config - https://docs.docker.com/docker-for-mac/#file-sharing

    ```bash
    git clone https://github.com/amazeeio/template-drupal8-opensocial-lagoon.git drupal9-lagoon && cd $_
    ```

3. Make sure you have pygmy stopped. Run `pygmy stop` to be sure.

4. We already have a Lando file in this repository, so we just need to run the following command to get Lando up:

 ```bash
lando start
```

5. Install your OpenSocial site with Drush:

```bash
lando drush si social -y
```

6. And now we have a fully working local Open Social site on Lando! For more information on how to deploy your site, check out our documentation or our deployment demo.

## What does the template do?

When installing the given `composer.json` some tasks are taken care of:

* Drupal will be installed in the `web`-directory.
* Autoloader is implemented to use the generated composer autoloader in `vendor/autoload.php`,
  instead of the one provided by Drupal (`web/vendor/autoload.php`).
* Modules (packages of type `drupal-module`) will be placed in `web/modules/contrib/`
* Themes (packages of type `drupal-theme`) will be placed in `web/themes/contrib/`
* Profiles (packages of type `drupal-profile`) will be placed in `web/profiles/contrib/` (Open Social ends up in here!)
* Creates the `web/sites/default/files`-directory.
* Latest version of drush is installed locally for use at `vendor/bin/drush`.
* Latest version of [Drupal Console](http://www.drupalconsole.com) is installed locally for use at `vendor/bin/drupal`.
* The correct scaffolding for your Drupal core version is installed, along with Lagoon-specific scaffolding from our [amazeeio/drupal-integrations](https://github.com/amazeeio/drupal-integrations) project and the `assets/` directory in this repo.  For more information see [drupal/core-composer-scaffold](https://github.com/drupal/core-composer-scaffold)
* Open Social is patched so that the social_demo module drush commands are detcted

## Updating Open Social

This still needs to be figured out. Now accepting PRs.

## FAQ

### Should I commit the contrib modules I download?

Composer recommends **no**. They provide [argumentation against but also
workarounds if a project decides to do it anyway](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

### How can I apply patches to downloaded modules?

If you need to apply patches (depending on the project being modified, a pull
request is often a better solution), you can do so with the
[composer-patches](https://github.com/cweagans/composer-patches) plugin.

To add a patch to drupal module foobar insert the patches section in the extra
section of composer.json:

```json
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL to patch"
        }
    }
}
```
