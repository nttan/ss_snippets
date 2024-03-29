#Table of Contents

1. Config
  * [Comments](#comments)
  * [Dev mode](#dev-mode)
  * [Additional memory](#additional-memory)
  * [Default admin](#default-admin)
  * [Remove image max width in HtmlEditorField](#remove-image-max-width-in-htmleditorfield)
2. Templates
  * [Loop](#template-loop)
  * [Modal](#modal)
  * [Index Template Switching](#index-template-switching)
3. Model Biz
  * [Array list loop](#array-list-loop)
  * [Dataobject](#data-object)
  * [Gridfield](#gridfield)
  * [Site Config Extension](#site-config-extension)
4. CMS Fields
  * [Tab](#tab)
  * [Model admin](#model-admin)
  * [Settings](#settings)
  * [Pagination](#pagination)
  * [SiteConfig tab](#site-config-tab)
5. Pages
  * [HomePage](#blank-homepage)

#Config

###Comments

####Section

```php
/** =========================================
 * Section
 ===========================================*/
```

####Sub Section

```php
/** -----------------------------------------
 * SubSection
 * ----------------------------------------*/
```

###Dev Mode

```yaml
Director:
  environment_type: 'dev'
```

###Additional Memory

```php
ini_set('memory_limit','1000M');
```

###Default Admin

```php
Security::setDefaultAdmin('admin', 'password');
```

###Remove image max-width in HtmlEditorField

```yaml
HtmlEditorField:
  insert_width: 1600
```

##Templates

###Loop

```
<% if $Children %>
    <section class="block">
        <div class="container">
            <div class="row">
                <% loop $Children %>
                    <article class="block__item is-{$EvenOdd}<% if $FirstLast %> is-{$FirstLast}<% end_if %>">
                        <% if $Image %>
                            <figure class="block__item__image">
                                <a href="{$Link}" title="{$Title}">
                                    {$Image}
                                </a>
                            </figure><!-- /.block__item__image -->
                        <% end_if %>
                        <h4 class="block__item__heading">
                            <a href="{$Link}" title="{$Title}">{$MenuTitle}</a>
                        </h4><!-- /.block__item__heading -->
                        <% if $Content %>
                            <div class="block__item__summary typography">
                                {$Content.LimitWordCountXML(40)}
                            </div><!-- /.block__item__summary typography -->
                        <% end_if %>
                        <div class="block__item__actions">
                            <a href="$Link" class="btn--primary" title="{$Title}">Read more</a>
                        </div><!-- /.block__item__actions -->
                    </article><!-- /.block__item -->
                <% end_loop %>
            </div><!-- /.row -->
        </div><!-- /.container -->
    </section><!-- /.block -->
<% end_if %>
```

###Modal

```
<button type="button" class="btn--primary" data-toggle="modal" data-target="#modal">
    Launch demo modal
</button><!-- /.btn--primary -->
<div class="modal" id="modal" tabindex="-1" role="dialog" aria-labelledby="modalLabel">
    <div class="modal__dialog" role="document">
        <div class="modal__dialog__content">
            <div class="modal__dialog__content__header">
                <button type="button" class="close" data-dismiss="modal" aria-label="Close"><i class="fa fa-close"></i></button>
                <h4 class="modal__dialog__content__header__title" id="modalLabel">Modal title</h4>
            </div><!-- /.modal__dialog__content__header -->
            <div class="modal__dialog__content__body">
            </div><!-- /.modal__dialog__content__body -->
            <div class="modal__dialog__content__footer">
                <button type="button" class="btn--inverse" data-dismiss="modal">Close</button>
            </div><!-- /.modal__dialog__content__footer -->
        </div><!-- /.modal__dialog__content -->
    </div><!-- /.modal__dialog -->
</div><!-- /.modal -->
```

###Index Template Switching
```php
/**
 * @return array
 */
public function index()
{
    if ($this->Foo()) {
        $class = $this->ClassName . "_Controller";
        $controller = new $class($this);
        return $controller->renderWith(array('Some_Template', 'Page'));
    }
    return array();
}
```

##Model Biz

###Array List Loop
```php
$arrayList = new ArrayList();
foreach ($foo as $item) {
    $arrayList->push(new ArrayData(array(
        'Title' => $item->Title
    )));
}
```

###Data Object

```php
<?php

/**
 * Class Name
 *
 * @property Int SortOrder
 *
 * @method Page Page
 */
class Name extends DataObject
{
    /**
     * @var array
     */
    private static $db = array(
        'SortOrder' => 'Int'
    );

    /**
     * @var string
     */
    private static $singular_name = 'Name';

    /**
     * @var string
     */
    private static $plural_name = 'Names';

    /**
     * @var array
     */
    private static $has_one = array(
        'Page' => 'Page'
    );

    /**
     * @var array
     */
//    private static $summary_fields = array();

    /**
     * @var string
     */
    private static $default_sort = 'SortOrder';

//    /**
//     * @return RequiredFields
//     */
//    public function getCMSValidator() {
//        return new RequiredFields(array());
//    }

    /**
     * @return FieldList
     */
    public function getCMSFields()
    {
        /** =========================================
         * @var FieldList $fields
        ===========================================*/

        $fields = FieldList::create(TabSet::create('Root'));

        /** -----------------------------------------
         * Fields
         * ----------------------------------------*/

//        $fields->addFieldToTab('Root.Main', $bar = TextField::create('Foo'));

        $this->extend('updateCMSFields', $fields);

        return $fields;
    }

    /**
     * On Before Write
     */
    protected function onBeforeWrite()
    {
        /** Set SortOrder */
        if (!$this->SortOrder) {
            $this->SortOrder = DataObject::get($this->ClassName)->max('SortOrder') + 1;
        }
        parent::onBeforeWrite();
    }

}
```

###Gridfield
[Uses the Grid Field Extensions Module](https://github.com/silverstripe-australia/silverstripe-gridfieldextensions)

```php
$config = GridFieldConfig_RelationEditor::create(10);
$config->addComponent(GridFieldOrderableRows::create('SortOrder'))
    ->addComponent(new GridFieldDeleteAction());
$gridField = GridField::create(
    'Name',
    'Title',
    $this->owner->Items(),
    $config
);
$fields->addFieldToTab('Root.Main', $gridField);
```

####Site Config Extension

```php
<?php

/**
 * Class SiteConfigExtension
 */
class SiteConfigExtension extends DataExtension
{

    private static $db = array();

    /**
     * @param FieldList $fields
     */
    public function updateCMSFields(FieldList $fields)
    {
        /** =========================================
         * @var FieldList $fields
        ===========================================*/

        /** -----------------------------------------
         * Settings
         * ----------------------------------------*/

        if (!$fields->fieldByName('Root.Settings')) {
            $fields->addFieldToTab('Root', new TabSet('Settings'));
        }

    }

}
```

#####_Config

```yaml
SiteConfig:
  extensions:
    - SiteConfigExtension
```

###CMS Fields

####Tab

```php
$fields->findOrMakeTab('Root.Settings.TabName', 'Tab Name');
$fields->addFieldsToTab('Root.Settings.TabName',
    array()
);
```

###Model Admin
[Uses the Grid Field Extensions Module](https://github.com/silverstripe-australia/silverstripe-gridfieldextensions)

```php
<?php

/**
 * Class CustomModelAdmin
 */
class CustomModelAdmin extends ModelAdmin
{

    //private static $menu_icon = 'path/to/image.png';

    private static $managed_models = array(
        'Model'
    );

    //private static $menu_priority = 1;

    private static $url_segment = 'models';

    private static $menu_title = 'models';

    private static $url_handlers = array(
        '$ModelClass/$Action' => 'handleAction',
        '$ModelClass/$Action/$ID' => 'handleAction',
    );

    public function getEditForm($id = null, $fields = null)
    {
        /** =========================================
         * @var Form $form
         * @var GridField $gridField
        ===========================================*/

        $form = parent::getEditForm($id, $fields);

        if ($this->modelClass == 'Model' && $gridField = $form->Fields()->dataFieldByName($this->sanitiseClassName($this->modelClass))) {
            /**
             * This is just a precaution to ensure we got a GridField from dataFieldByName() which you should have
             */
            if ($gridField instanceof GridField) {
                $gridField->getConfig()->addComponent(new GridFieldOrderableRows('SortOrder'));
            }
        }

        return $form;
    }

}
```

###Settings

```php
/**
 * @return mixed
 */
public function getSettingsFields()
{
    $fields = parent::getSettingsFields();
    return $fields;
}
```

###Pagination

```php
/**
 * @return PaginatedList
 */
public function PaginatedPages()
{
    /** =========================================
     * @var PaginatedList $pagination
    ===========================================*/

    $pagination = new PaginatedList($this->AllChildren(), Controller::curr()->request);
    $pagination->setPageLength(15);
    return $pagination;
}
```

###Site Config Tab

```php
$fields->findOrMakeTab('Root.Settings.Tab', 'Tab');
$fields->addFieldsToTab('Root.Settings.Tab',
    array(
        // Fields
    )
);
```

##Pages

###Blank HomePage

```php
<?php

/**
 * Class HomePage
 */
class HomePage extends Page
{
    /**
     * @var string
     */
//    private static $icon = '';

    /**
     * @var array
     */
    private static $db = array();

    /**
     * @var bool
     */
//    private static $can_be_root = false;

    /**
     * @var array
     */
//    private static $allowed_children = array();

//    /**
//     * @return RequiredFields
//     */
//    public function getCMSValidator() {
//        return new RequiredFields(array());
//    }

    /**
     * @return FieldList
     */
    public function getCMSFields()
    {
        /** =========================================
         * @var FieldList $fields
        ===========================================*/

        $fields = parent::getCMSFields();

        /** -----------------------------------------
         * Fields
         * ----------------------------------------*/

        return $fields;
    }

}

/**
 * Class HomePage_Controller
 */
class HomePage_Controller extends Page_Controller
{
}
```
