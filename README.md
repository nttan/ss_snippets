ss_snippets
===========

Snippets for Silverstripe

** Data Object

```
class SliderItem extends DataObject{
    static $db = array (
        'SortOrder' => 'Int'
    );
    static $has_one = array (
        'Page' => 'Page'
    );
    private static $default_sort = 'SortOrder';

    function getCMSFields() {
        $fields = FieldList::create(TabSet::create('Root'));
        return $fields;
    }
}
```

**Gridfield

```
$config = GridFieldConfig_RelationEditor::create(10);
$config->addComponent(new GridFieldSortableRows('SortOrder'))
    ->addComponent(new GridFieldDeleteAction());
$config->getComponentByType('GridFieldDataColumns')->setDisplayFields(array(
    'Thumbnail' => 'Thumbnail'
));
$gridField = new GridField(
    'Name',
    'Title',
    $this->owner->Items(),
    $config
);
$fields->addFieldToTab('Root.Main', $gridField);
```

** Site Config Tab

```
$fields->findOrMakeTab('Root.Settings.Tab', 'Tab');
$fields->addFieldsToTab('Root.Settings.Tab',
    array(
        // Fields
    )
);
```
