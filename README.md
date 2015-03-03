#Table of Contents

1. Config
  * [Dev mode](#dev-mode)
  * [Additional memory](#additional-memory)
  * [Default admin](#default-admin)
  * [Switch database based on server](#switch-database-based-on-server)
  * [Remove image max width in HtmlEditorField](#remove-image-max-width-in-htmleditorfield)
2. Model Biz
  * [Array list loop](#array-list-loop)
  * [Dataobject](#data-object)
  * [Gridfield](#gridfield)
  * [Site Config Extension](#site-config-extension)
3. CMS Fields
  * [Tab](#tab)
  * [Model admin](#model-admin)
  * [Settings](#settings)
  * [Pagination](#pagination)
  * [SiteConfig tab](#site-config-tab)
4. Pages
  * [HomePage](#blank-homepage)

#Config

###Dev Mode

```
Director:
  environment_type: 'dev'
```

###Additional Memory

```
ini_set('memory_limit','1000M');
```

###Default Admin

```
Security::setDefaultAdmin('admin', 'password');
```

###Remove image max-width in HtmlEditorField

```
HtmlEditorField:
  insert_width: 1600
```

###Switch database based on server

```
switch ($_SERVER['SERVER_NAME']) {
    case 'pantry.local':
    case '10.10.10.127':
        $databaseConfig = array(
            "type" => 'MySQLDatabase',
            "server" => 'localhost',
            "username" => '',
            "password" => '',
            "database" => '',
            "path" => '',
        );
    break;
    default:
        $databaseConfig = array(
            "type" => 'MySQLDatabase',
            "server" => 'localhost',
            "username" => '',
            "password" => '',
            "database" => '',
            "path" => '',
        );
    break;
}
```

##Model Biz

###Array List Loop
```
$arrayList = new ArrayList();
foreach($foo as $item){
    $arrayList->push(new ArrayData(array(
        'Title' => $item->Title
    )));
}
```

###Data Object

```
<?php

/**
 * Class Name
 */
class Name extends DataObject{

    private static $db = array (
        'SortOrder' => 'Int'
    );
    
    private static $singular_name = 'Name';
    private static $plural_name = 'Names';

    private static $has_one = array (
        'Page' => 'Page'
    );

    //private static $summary_fields = array();

    private static $default_sort = 'SortOrder';

    /**
     * @return FieldList
     */
    public function getCMSFields() {
        $fields = FieldList::create(TabSet::create('Root'));
        // fields
        return $fields;
    }
    
}
```

###Gridfield

```
$config = GridFieldConfig_RelationEditor::create(10);
$config->addComponent(new GridFieldSortableRows('SortOrder'))
    ->addComponent(new GridFieldDeleteAction());
$config->getComponentByType('GridFieldDataColumns')->setDisplayFields(array(
    'Title' => 'Title'
));
$gridField = new GridField(
    'Name',
    'Title',
    $this->owner->Items(),
    $config
);
$fields->addFieldToTab('Root.Main', $gridField);
```

####Site Config Extension

```
<?php

/**
 * Class SiteConfigExtension
 */
class SiteConfigExtension extends DataExtension {

    private static $db = array();

    /**
     * @param FieldList $fields
     */
    public function updateCMSFields(FieldList $fields) {

        /* =========================================
         * Settings
         =========================================*/

        if (!$fields->fieldByName('Root.Settings')){
            $fields->addFieldToTab('Root', new TabSet('Settings'));
        }

    }

}
```

#####_Config

```
SiteConfig:
  extensions:
    - SiteConfigExtension
```

###CMS Fields

####Tab

```
$fields->findOrMakeTab('Root.Settings.TabName', 'Tab Name');
$fields->addFieldsToTab('Root.Settings.TabName',
    array()
);
```

###Model Admin

```
<?php

/**
 * Class CustomModelAdmin
 */
class CustomModelAdmin extends ModelAdmin {

    //private static $menu_icon = 'path/to/image.png';
    
    private static $managed_models = array(
        'Model'
    );
    private static $url_priority = 100;
    private static $url_segment = 'models';
    private static $menu_title = 'models';

    private static $url_handlers = array(
        '$ModelClass/$Action' => 'handleAction',
        '$ModelClass/$Action/$ID' => 'handleAction',
    );

    public function getEditForm($id = null, $fields = null) {
        $form = parent::getEditForm($id, $fields);
        // Form biz
        return $form;
    }

}
```

###Settings

```
public function getSettingsFields() {
    $fields = parent::getSettingsFields();
    return $fields;
}
```

###Pagination

```
public function PaginatedPages() {
    $pagination = new PaginatedList($this->AllChildren(), Controller::curr()->request);
    $pagination->setPageLength(15);
    return $pagination;
}
```

###Site Config Tab

```
$fields->findOrMakeTab('Root.Settings.Tab', 'Tab');
$fields->addFieldsToTab('Root.Settings.Tab',
    array(
        // Fields
    )
);
```

##Pages

###Blank HomePage

```
<?php

/**
 * Class HomePage
 */
class HomePage extends Page {

    //private static $icon = '';

    private static $db = array();
    
    //private static $can_be_root = false;
    //private static $allowed_children = array();

    public function getCMSFields() {
        $fields = parent::getCMSFields();

        /* =========================================
         * Fields
         =========================================*/

        return $fields;
    }

}

/**
 * Class HomePage_Controller
 */
class HomePage_Controller extends Page_Controller {}
```
