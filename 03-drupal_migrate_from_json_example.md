# Migrate task

### 1. 依據欲輸入的 json 結構建立新的 content type
新增 content type，加入`post ID`與`email`兩個field

### 2. 新增模組資料夾與檔案
在 `module/custom` 下建立新模組資料夾，將 [https://github.com/steveworley/migrate-d8](https://github.com/steveworley/migrate-d8) 內容檔案放入資料夾中

### 3. 修改模組設定檔
#### `config/install/migrate_plus.migration.demo.yml`

輸入來源的json網址

```yaml
source:
  path: http://jsonplaceholder.typicode.com/comments
```

json資料的key

```yaml
source:
  fields:
    - id
    - postId
    - name
    - email
    - body
```

輸入目標 content type 與其資料 field

```yaml
process:
  type:
    plugin: default_value
    default_value: {target_content_type}
  title: name
  'body/value': body
  'body/summary': body
  'body/format':
    plugin: default_value
    default_value: full_html
  field_email: email
  field_post_id: postId
```

### 4. 手動下載模組放入`modules/contrib`
由於 migrate_source_json 這個模組還沒有支援 Drupal 8 的正式版本，於是手動下載開發版本  
[https://www.drupal.org/project/migrate_source_json](https://www.drupal.org/project/migrate_source_json)  

### 5. 啟動模組

`drush en {{module_name}}`  

`drush migrate-status`

```
Group: UDN  Status  Total  Imported  Unprocessed  Last imported
 demo        Idle    500    0         500
```

`drush migrate-import {{migrate_id}} --update`

### 6. 解除安裝模組（發生錯誤須重新安裝時）

#### `drupal module:uninstall`

```
{{module name}}  
migrate_tools  
migrate_source_json  
migrate_plus
migrate
```

#### `drupal config:delete`

```shell
Configuration name. [automated_cron.settings]:  
 > migrate_plus.migration.demo
```

#### 刪除 modules 內與 migrate 相關資料夾

```
contrib/migrate_plus
contrib/migrate_source_json
contrib/migrate_tools
```

#### 重建快取 `drush cr`
