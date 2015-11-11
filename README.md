# MoodleVM

MoodleVM is a virtual machine for Moodle!

## Prerequisites

You need ansible and the roles in ansible/requirements.txt. Install them like
this:

```
$ ansible-galaxy install -r ansible/requirements.txt
```

## Usage

### Create a parameters.yml file

Like this:

```yml
---
# Github
github_access_token: YOUR_ACCESS_TOKEN

# Mandatory Moodle params
moodle_db_name:     moodle
moodle_db_username: moodle
moodle_db_password: moodle
moodle_location:    /srv/moodle
moodle_version:     2.8
moodle_dataroot:    /srv/moodledata
moodle_wwwroot:     http://moodle-vm.dev

# Optional parameters if using a fresh install
moodle_adminuser:   root
moodle_adminpass:   root
moodle_adminemail:  you@example.com
moodle_fullname:    "Moodle Virtual Machine"
moodle_shortname:   "MoodleVM"
moodle_summary:     "A virtual machine for developing Moodle."

# Optional parameters if doing a DB import
moodle_db_import:   "/vagrant/database/seed.sql"

# If you want to get Moodle from git, specify the tag
moodle_tag:         v2.8.5

# Local moodle plugins
moodle_plugins_dev:
  # Type of plugin (example "block", "auth", "enrol", etc)
  - type: enrol

    # Location of the plugin relative to ansible 
    location: ../plugins/enrol_ldap_group

  - type: block
    location: ../plugins/block_someblock  

# Moodle plugins from moodle.org or github
moodle_plugins:
  # This is an example of a plugin from moodle.org
  - name: block_rss_plus
    shortname: rss_plus
    url: "https://moodle.org/plugins/download.php/8660/block_rss_plus_moodle28_2015060101.zip"
    
    # Where to put the plugin
    destination: blocks
  
  # This is an example of a plugin hosted on github
  - name: theme_vision
    shortname: vision
    url: "https://github.com/HCPSS/moodle-theme_vision.git"
    destination: theme

    # Optional, defaults to HEAD
    tag: v1.0.2
    service: github

  # An Example of a plugin hosted on a github private repo
  - name: local_mycoplugin
    shortname: mycoplugin
    url: "https://github.com/VENDOR/REPO.git"
    destination: local
    service: github
    tag: v1.0.0
    
    # Your github personal access token. I'll use mine from above...
    auth: "{{ github_access_token }}"
    
# Patches to apply to the Moodle installation. The "patches" folder is supplied
# and ignored by .gitignore as a convenient place to put the patch files. The
# Path is relative to the ansible directory.
moodle_patches:
  - src: ../patches/mod_forum_lib.php.patch
    dest: "{{ moodle_location }}/mod/forum/lib.php"

# Apache
apache_user:    www-data
apache_group:   www-data

```

### Run it

```
$ vagrant up
```
