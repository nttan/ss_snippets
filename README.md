ss_snippets
===========

Snippets for Silverstripe

###Config Biz

```
// Set a default admin
Security::setDefaultAdmin('admin', 'password');
```
```
// Switch database based on server
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
class Name extends DataObject{
    static $db = array (
        'SortOrder' => 'Int'
    );
    static $has_one = array (
        'Page' => 'Page'
    );
    private static $default_sort = 'SortOrder';

    function getCMSFields() {
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

### CMS Fields

#### Tab

```
$fields->findOrMakeTab('Root.Settings.TabName', 'Tab Name');
$fields->addFieldsToTab('Root.Settings.TabName',
    array()
);
```

### Model Admin

```
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

### Settings

```
public function getSettingsFields() {
    $fields = parent::getSettingsFields();
    return $fields;
}
```

### Pagination

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
