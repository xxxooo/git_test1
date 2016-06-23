## Objective

- Explore the different base themes available for use out of the box in D8 and how they are different.
- Use Drupal Console to generate a new theme

## Exercise

1. Generate a new theme with “stable” as the base theme using Drupal console.

    ```shell
    drupal generate:theme
    ```

    ```shell
    Enter the new theme name []:
    > udn_theme
    Enter the module machine name [udn_theme]:
    Enter the theme Path [/themes/custom]:
    Enter theme description [My Awesome theme]:
    > UDN Theme
    Enter package name: [Other]:
    > udnDigital
    Enter Drupal Core version [8.x]:
    Base theme (i.e. classy, stable) [false]:
    > stable
    Enter the global styling library name [global-styling]:
    Do you want to generate theme regions (yes/no) [yes]:
    Enter region name [Content]:
    Enter region machine name [content]:
    Do you want to generate theme regions (yes/no) [yes]:
    Enter region name [Content]:
    > Sidebar 1
    Enter region machine name [sidebar_1]:
    > sidebar1
    Do you want to generate theme regions (yes/no) [yes]:
    Enter region name [Content]:
    > Sidebar 2
    Enter region machine name [sidebar_2]:
    > sidebar2
    Do you want to generate theme regions (yes/no) [yes]:
    > no
    Do you want to generate the theme breakpoints (yes/no) [yes]:
    > no
    Do you confirm generation? (yes/no) [yes]:
    ```

2. Define 3 regions (sidebar1, content, sidebar2 while generating the theme)

    > Drupal console generated `udn_theme.info.yml` with the following content:

    ```yaml
    name: udn_theme
    type: theme
    description: UDN Theme
    package: udnDigital
    core: 8.x
    libraries:
      - udn_theme/global-styling

    base theme: stable

    regions:
      content: Content
      sidebar1: Sidebar 1
      sidebar2: Sidebar
    ```

3. Install the newly generated theme and set as default

    1. Navigate to Manage > Appearance page
    2. Locate `udn_theme` under the list of Uninstalled themes
    3. Click on **Install and set as default** link

4. Turn on Twig Debugging to see the template names in use, as well as template name suggestions in the view source.

    > see [Configuring Local Development Environment](01-configuring-local-dev.md) for steps on how to turn on twig debugging

5. Override page.html.twig in your theme such that the 3 columns in the page are rendered as below:

    1. Create `templates/layout` folder structure under `udn_theme`
    2. Create `page.html.twig` under this folder with the following:

        ```html
        <div class="container">
          <div class="row">
            <div class="col-xs-12 col-sm-4">
              {{ page.sidebar1 }}
            </div>
            <div class="col-xs-12 col-sm-4">
              {{ page.content }}
            </div>
            <div class="col-xs-12 col-sm-4">
              {{ page.sidebar2 }}
            </div>
          </div>
        </div>
        ```

    3. Create `udn_theme.libraries.yml` with the following:

        ```yaml
        global-styling:
          css:
            theme:
              //maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css: {}
        ```

        > Together with the libary dependencies in `udn_theme.info.yml`, bootstrap css is now imported into the page.

6. Check the result in Drupal site

### Additional resources

- https://www.lullabot.com/articles/a­tale­of­two­base­themes­in­drupal­8­core
- https://hechoendrupal.gitbooks.io/drupal­console/content/en/commands/generate­theme.html
