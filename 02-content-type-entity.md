## Objective

- Explore creating content entity type, with administration management pages.
- Use Drupal console to generate content types

## Exercise

1. Create a new custom entity with the following fields:
  - name
  - body
  - rush_status
  - tags (term reference)
2. Add validation around the form

## Additional resources

- http://drupal.stackexchange.com/questions/180373/how-to-get-the-list-of-field-types#answer-180378
- https://github.com/steveworley/robots-d8
- https://www.drupal.org/node/2192175

## Steps

1. Install Drupal console

    ```bash
    # Download Drupal Console
    php -r "readfile('https://drupalconsole.com/installer');" > drupal.phar
    # Enable accessing from anywhere on your system
    mv drupal.phar /usr/local/bin/drupal
    # Apply executable permissions on the downloaded file:
    chmod +x /usr/local/bin/drupal
    ```

2. Generate a new Module with Drupal console

    ```bash
    drupal generate:module
    ```

    Enter the new module name:

    \> **udn_newsroom**

    Enter the module machine name [udn_newsroom]:

    Enter the module Path [/modules/custom]:

    Enter module description [My Awesome Module]:

    \> **UDN Editorial Module**

    Enter package name [Custom]:

    \> **UDN**

    Enter Drupal Core version [8.x]:

    Do you want to generate a .module file (yes/no) [yes]:

    Define module as feature (yes/no) [no]:

    Do you want to add a composer.json file to your module (yes/no) [yes]:

    Would you like to add module dependencies (yes/no) [no]:

    Do you confirm generation? (yes/no) [yes]

3. Generate a new Entity Type via Drupal console

    ```bash
    drupal generate:entity:content
    ```

    Enter the module name [udn_newsroom]:

    Enter the class of your new content entity [DefaultEntity]:

    \> **News**

    Enter the name of your new content entity [news]:

    Enter the label of your new content entity [News]:

    Enter the base-path for the content entity routes [/admin/structure]:

    Do you want this (content) entity to have bundles (yes/no) [no]:

4. Define fields for `News` entity

    ```javascript
    // src/Entity/News.php`

    public static function baseFieldDefinitions(EntityTypeInterface $entity_type) {
      ...

      // Define 'body' field
      $fields['body'] = BaseFieldDefinition::create('text_long')
        ->setLabel(t('Body'))
        ->setDescription(t('The body of news article'))
        ->setDisplayOptions('view', [
          'label' => 'hidden',
          'weight' => 15,
        ])
        ->setDisplayOptions('form', [
          'label' => 'hidden',
          'weight' => 15,
        ]);

      // Define 'rush_status' field
      $fields['rush_status'] = BaseFieldDefinition::create('boolean')
        ->setLabel(t('Rush Status'))
        ->setDescription(t('True to rush publication'))
        ->setSetting('on_label', 'Rush?')
        ->setDisplayOptions('view', [
          'label' => 'inline',
          'weight' => 20,
        ])
        ->setDisplayOptions('form', [
          'label' => 'inline',
          'weight' => 20,
        ]);

      // Define 'tags' field
      $fields['tags'] = BaseFieldDefinition::create('entity_reference')
        ->setLabel(t('Category'))
        ->setDescription(t('Category of News Article'))
        ->setSettings([
          'target_type' => 'taxonomy_term',
          'vocabulary' => 'tags',
        ])
        ->setCardinality(FieldStorageDefinitionInterface::CARDINALITY_UNLIMITED)
        ->setDisplayOptions('view', [
          'label' => 'inline',
          'weight' => 25,
        ])
        ->setDisplayOptions('form', [
          'label' => 'inline',
          'weight' => 25,
        ]);
    ```

5. Enable module using drush

    ```bash
    drush en udn_newsroom
    ```

5. Add validation to the add form

    ```javascript
    // src/Form/NewsForm.php

    public function validateForm(array &$form, FormStateInterface $form_state) {
      $name = $form_state->getValue('name')[0]['value'];
      if (empty($name)) {
        $form_state->setErrorByName('name', 'Name should not be empty');
      }
      $body = $form_state->getValue('body')[0]['value'];
      if (empty($body)) {
        $form_state->setErrorByName('body', 'Body should not be empty');
      }

      return parent::validateForm($form, $form_state);
    }
    ```

6. Visit `/admin/structure/news/add` to test validation logic
