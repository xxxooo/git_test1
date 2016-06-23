## Objective

Take a look at basics of Twig Templating, that has replaced the PHPTemplate engine in Drupal.

_Ensure that Twig debug is on, outlined in configuring for local dev_

## Exercise

Using the `/node` page

1. Create a new template for a specific node
  - Add classes to the list of classes for this templte
  - Change the header style
2. Create a custom library to add a stylesheet
3. Create a new template for all nodes for this view
  - Include the new library that was created above
  - Change the colour of the headers

## Steps

Continue from the previous exercise [Create custom theme](03-theme.md)

1. Create at least 2 nodes (e.g. `/node/1` and `/node/2`)
2. Update `udn_theme.info.yml` to add a **Header** region

    ```yaml
    ...
    regions:
      header: Header
      ...
    ```
3. Update `templates/layout/page.html.twig` to display the **Header** region:

    ```html
    <div class="container">
      <header class="row">
        <div class="col-xs-12">
          {{ page.header }}
        </div>
      </header>
      ...
    </div>
    ```

3. Place block **Page Title** into **Header** region
4. Copy `templates/layout/page.html.twig` to `templates/layout/page--node--1`
5. Update `templates/layout/page--node--1.html.twig` to change the header style:

    ```html
    {%
      set classes = [
        'row',
        'myheader'
      ]
    %}
    <style>
      .myheader {
        color: white;
        background: blue;
      }
    </style>
    <div class="container">
      <header{{ attributes.addClass(classes) }}>
        <div class="col-xs-12">
          {{ page.header }}
        </div>
      </header>
    ```
6. Update `udn_theme.libraries.yml` to add a stylesheet:

    ```yaml
    custom-styling:
      css:
        theme:
          css/style.css: {}
    ```

7. Create `css/style.css` with the following:

    ```css
    .myheader {
      color: red;
    }
    ```

8. Copy `templates/layout/page.html.twig` to `templates/layout/page--node--%.html.twig`

9. Update `templates/layout/page--node--%.html.twig` to attach the custom library:

    ```html
    {% set classes = [ 'row', 'myheader' ] %}
    {{ attach_library('udn_theme/custom-styling') }}
    <div class="container">
      <header{{ attributes.addClass(classes) }}>
        <div class="col-xs-12">
          {{ page.header }}
        </div>
      </header>
      ...
    ```

10. Rebuild cache and visit `/node/1` and `node/2`

    ```shell
    drush cr
    ```

### Additional resources

- https://www.drupal.org/theme-guide/8/twig
- http://twig.sensiolabs.org/doc/templates.html
