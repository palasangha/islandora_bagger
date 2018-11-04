# Islandora Bagger

Tool to generate [Bags](https://en.wikipedia.org/wiki/BagIt) for objects using Islandora's REST interface. Currently only adds the Fedora Turtle representation of the Islandora object to the Bag's `data` directory. Also supports adding `bag-info.txt` tags via a configuration file.

This utility is for Islandora CLAW. For creating Bags for Islandora 7.x, use [Islandora Fetch Bags](https://github.com/mjordan/islandora_fetch_bags).

## Requirements

* PHP 7.1.3 or higher
* [composer](https://getcomposer.org/)

## Installation

1. Clone this git repository.
1. `cd islandora_bagger`
1. `php composer.phar install` (or equivalent on your system, e.g., `./composer install`)

## Usage

### The configuration file

Islandora Bagger requires a configuration file in YAML format:

```yaml
drupal_base_url: 'http://localhost:8000/node/'
fedora_base_url: 'http://localhost:8080/fcrepo/rest/'

# How to name the Bag directory (or file if serialized). One of 'nid' or 'uuid'.
bag_name: nid
temp_dir: /tmp/islandora_bagger_temp
output_dir: /home/mark/islandora_bagger

# Whether or not to zip up the Bag. One of false, 'zip', or 'tgz'.
# serialize: zip
serialize: false

# Include Internal-Sender-Identifier and Bagging-Date tags.
include_basic_baginfo_tags: true

# You can use any combination of additional tag name / value here.
bag-info:
    Contact-Name: Mark Jordan
    Contact-Email: bags@sfu.ca
    Source-Organization: Simon Fraser University
    Foo: Bar

# Whether or not to log Bag creation.
log_bag_creation: true
```

The command to generate a Bag takes two required parameters. Assuming the above configuration file is named `sample_config.yml`, and the Islandora node ID you want to generate a Bag from is 112, the command would look like this:

`./bin/console app:islandora_bagger:create_bag --settings=sample_config.yml --node=112`

The resulting Bag would look like this:

```
/home/mark/islandora_bagger/
└── 112
    ├── bag-info.txt
    ├── bagit.txt
    ├── data
    │   └── turtle.rdf
    ├── manifest-sha1.txt
    └── tagmanifest-sha1.txt
```

## To do

* Add plugins that allow the addition of various `data` files and dynamically generated bag-info.txt tags.
* Add more logging.

## Current maintainer

* [Mark Jordan](https://github.com/mjordan)

## License

[MIT](https://opensource.org/licenses/MIT)
