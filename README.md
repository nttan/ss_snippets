ss_snippets
===========

Snippets for Silverstripe

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
