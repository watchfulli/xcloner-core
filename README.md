# xcloner-core

## Installation

Local composer.json should contain this custom repository for now

```
"repositories": [
        {
            "type": "vcs",
            "packagist": false,
            "url":  "https://github.com/watchfulli/xcloner-core.git"
        }
    ]
 ```

then 

```composer require watchfulli/xcloner-core```

## Usage

Trigger standalone backup with profile stored in standalone_backup_trigger_config.json

```
<?php
define('WP_DEBUG', true);
define('WP_DEBUG_DISPLAY', true);

require_once('../vendor/autoload.php');

$profile = [
    'id' => 0
];

if(isset($argv[1]) && $argv[1]) {
    $profile_name = $argv[1];
}

//loading the default xcloner settings in format [{'option_name':'value', {'option_value': 'value'}}]
$json_config = json_decode(file_get_contents(__DIR__ . '/standalone_backup_trigger_config.json'));

if (!$json_config) {
    die('Could not parse default JSON config, i will shutdown for now...');
}

//pass json config to Xcloner_Standalone lib
$xcloner_backup = new watchfulli\XClonerCore\Xcloner_Standalone($json_config);

if (isset($profile_name) && $profile_name) {
    $profile = ($xcloner_backup->xcloner_scheduler->get_schedule_by_id_or_name($profile_name));
}

$xcloner_backup->start($profile['id']);


```
