#Table of Contents

1. Config
  * [Comments](#comments)
  * [Dev mode](#dev-mode)
  * [Additional memory](#additional-memory)
  * [Default admin](#default-admin)
  * [Switch database based on server](#switch-database-based-on-server)
  * [Remove image max width in HtmlEditorField](#remove-image-max-width-in-htmleditorfield)
2. Templates
  * [Loop](#template-loop)
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

```
/** =========================================
 * Section
 ===========================================*/
```

####Sub Section

```
/** -----------------------------------------
 * Section
 -------------------------------------------*/
```

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

##Templates

###Loop

```
<% if $Children %>
    <section class="loop section">
        <div class="container">
            <div class="row">
                <% loop $Children %>
                    <article class="loop__item loop__item--{$FirstLast} loop__item--{$EvenOdd} article">
                        <% if $Image %>
                            <figure class="article__image">
                                <a href="{$Link}" title="{$Title}">
                                    {$Image}
                                </a>
                            </figure><!-- /.article__image -->
                        <% end_if %>
                        <h4 class="article__heading">
                            <a href="{$Link}" title="{$Title}">{$MenuTitle}</a>
                        </h4><!-- /.article__heading -->
                        <% if $Content %>
                            <div class="article__summary typography">
                                {$Content.LimitWordCountXML(40)}
                            </div><!-- /.article__summary typography -->
                        <% end_if %>
                        <div class="article__actions">
                            <a href="$Link" class="btn--primary" title="{$Title}">Read more</a>
                        </div><!-- /.article__actions -->
                    </article><!-- /.loop__item loop__item--{$FirstLast} loop__item--{$EvenOdd} article -->
                <% end_loop %>
            </div><!-- /.row -->
        </div><!-- /.container -->
    </section><!-- /.loop section -->
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
class Name extends DataObject {

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
    
    //public function getCMSValidator() {
    //    return new RequiredFields(array());
    //}

    /**
     * @return FieldList
     */
    public function getCMSFields() {
        $fields = FieldList::create(TabSet::create('Root'));
        // fields
        return $fields;
    }
    
    /**
     * On Before Write
     */
    protected function onBeforeWrite() {
        /**
         * Set SortOrder
         */
        if (!$this->SortOrder) {
            $this->SortOrder = PortfolioImage::get()->max('SortOrder') + 1;
        }
        parent::onBeforeWrite();
    }
    
}
```

###Gridfield

```
$config = GridFieldConfig_RelationEditor::create(10);
$config->addComponent(new GridFieldOrderableRows('SortOrder'))
    ->addComponent(new GridFieldDeleteAction());
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

        /** =========================================
         * Settings
         ==========================================*/

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
    //private static $menu_priority = 1;
    private static $url_segment = 'models';
    private static $menu_title = 'models';

    private static $url_handlers = array(
        '$ModelClass/$Action' => 'handleAction',
        '$ModelClass/$Action/$ID' => 'handleAction',
    );

    public function getEditForm($id = null, $fields = null) {
        $form = parent::getEditForm($id, $fields);
        
        if($this->modelClass=='Model' && $gridField=$form->Fields()->dataFieldByName($this->sanitiseClassName($this->modelClass))) {
            /**
             * This is just a precaution to ensure we got a GridField from dataFieldByName() which you should have
             */
            if($gridField instanceof GridField) {
                $gridField->getConfig()->addComponent(new GridFieldSortableRows('SortOrder'));
            }
        }
        
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
    
    //public function getCMSValidator() {
    //    return new RequiredFields(array());
    //}

    public function getCMSFields() {
        $fields = parent::getCMSFields();

        /** =========================================
         * Fields
         ==========================================*/

        return $fields;
    }

}

/**
 * Class HomePage_Controller
 */
class HomePage_Controller extends Page_Controller {}
```
