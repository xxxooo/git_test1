## Objective

Create a view that contextually shows articles related by tag

## Exercise

Using the **article** content type.

1. Create a view with:
  -  **Has taxonomy term ID** contextual filter that loads the default value from the node page
  -  **Content ID**, under advanced options choose to exclude
2. Add a title filter and expose to the user
3. Configure to use AJAX

## Steps

1. Add a few terms to **Tags** vocabulary
2. Enable **Devel generate** module

  ```shell
  drush en devel_generate
  ```
3. Generate 50 **articles**

  ```
  Configuration > Development > Generate content
  ```

4. Create a view to display articles tagged with a given taxonomy term ID

  ```
  Path: /articles-tagged-with-id/%
  Contextual filters: Has taxonomy term ID
  Vocabularies: Tags
  ```

  See [views.view.articles_tagged_with_id.yml](views.view.articles_tagged_with_id.yml)

5. Create a view to display articles tagged with a given taxonomy term

  ```
  Path: /articles-tagged-with-term/%
  Relationships: Taxonomy terms on node
  Vocabularies: Tags
  Contextual filters: Name
  ```

6. Add a title filter and expose to the user

  ```
  Add a filter criteria
  Choose "Title" under Category "Content"
  Check "Expose this filter to visitors, to allow them to change it" checkbox
  ```

7. Configure to use AJAX

  ```
  Advanced > Other > Use AJAX
  ```

  See [views.view.articles_tagged_with_term.yml](views.view.articles_tagged_with_term.yml)

### Additional resources

- https://www.drupal.org/node/1912118
