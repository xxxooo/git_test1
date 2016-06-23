## Objective

Configure a new Drupal site ready for local development

## Exercise

1. Change directory to your Drupal 8 installation root directory

  ```shell
  cd ~/sites/local-devel
  ```
2. Change directory to the `sites` directory

  ```shell
  cd sites
  ```

3. Copy `example.settings.local.php` into `default` folder and rename it to `settings.local.php`

  ```shell
  cp example.settings.local.php default/settings.local.php
  ```

4. Uncomment lines #717-719 from `default/settings.php` to include `default/settings.local.php`

  ```php
  if (file_exists(__DIR__ . '/settings.local.php')) {
    include __DIR__ . '/settings.local.php';
  }
  ```

5. Line #39 of `settings.local.php` enables local development services

  ```php
  $config['container_yamls'][] = DRUPAL_ROOT . 'sites/development.services.yml'
  ```

6. Uncomment lines 67 and 76 of `development.services.yml` to disable render cache and dynamic page cache

  ```php
  $settings['cache']['bins']['render'] = 'cache.backend.null'; # line 67
  $settings['cache']['bins']['dynamic_page_cache'] = 'cache.backend.null'; # line 76
  ```

7. Update the **development.services.yml** file to enable TWIG debug feature

	```yaml
	parameters:
	  twig.config:
	    debug: true
	```

8. Check in the browser's development tool that TWIG debug feature has been enabled and CSS and JS aggregation has been disabled.

### Additional settings for PhpStorm

1. Enable Drupal Integration
2. Install _**PHP Annotations**_ Plugin
3. Enable XDebug support

### Additional resources

- https://www.drupal.org/node/2598914
