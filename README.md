# ng-jedi-layout
Layout components written in angularjs.

### Install

* Install the dependency:

   ```shell
   bower install ng-jedi-layout --save
   ```
* Add layout.js to your code:

   ```html
   <script src='assets/libs/ng-jedi-layout/layout.js'></script>
   <script src='assets/libs/ng-jedi-layout/datepicker.js'></script>
   <script src='assets/libs/ng-jedi-layout/input.js'></script>
   <script src='assets/libs/ng-jedi-layout/treeview.js'></script>
   <script src='assets/libs/ng-jedi-layout/modal.js'></script>
   <script src='assets/libs/ng-jedi-layout/panel.js'></script>
   ```
   - note that the base directory used was assets/libs, you should change bower_components to assets/libs or move from bower_components to assets/libs with grunt.
* Include module dependency:

   ```javascript
   angular.module('yourApp', ['jedi.layout']);
   ```
======

### How To Use

1. **Add jd-datepicker directive in your html**
   - this directive use bootstrap-datetimepicker and integrating its with angularjs
   ```html
   <input jd-datepicker jd-min-date="myCtrl.myModel.startDate" jd-max-date="myCtrl.myModel.endDate" type="text" ng-model="myCtrl.myModel.initialDate" />
   ```
   - if you need, you can customize the html. In your angular run from app.js, override the constant jedi.layout.datepicker.DatepickerConfig and set your html in template attribute.

2. **Add jd-treeview directive in your html**
   - this directive creates a treeview in your page. The value setted in directive shoud be the levels, separated by ';'. Each level should be passed the description field name. Your model should be similar to a tree. The last level uses the body content. E.g.:
   ```html
   <ol jd-treeview="level1[level1.fieldLevel1] in myCtrl.myModel.tree; level2[level2.fieldLevel2] in level1.levels2; level3 in level2.levels3">
      <input type="checkbox" ng-model="level3.selected">
      <span class="">{{level3.fieldLevel3}}</span>
   </ol>
   ```
   ```javascript
   vm.myModel = {tree: [
      {
         fieldLevel1: 'Description level 1',
         levels2: [
            {
               fieldLevel2: 'Description level 2',
               levels3: [
                  {
                     fieldLevel3: 'Description level 3'
                  }
               ]
            }
         ]
      }
   ]};
   ```
   * if you need, you can customize the html. In your angular run from app.js, override the constant jedi.layout.treeview.TreeviewConfig and set your html. There are three attributes:
   - emptyTpl: define html to write when tree is empty.
   - nodeTpl: define html to write the intermediate nodes.
   - lastNodeTpl: define html to write the last node.
   ```javascript
   app.run(['jedi.layout.treeview.TreeviewConfig', function(TreeviewConfig){
      TreeviewConfig.emptyTpl = '<div id="emptyTreeElement"><strong class="text-warning"><jd-i18n>Nenhum item encontrado.</jd-i18n></strong></div>';
   }]);
   ```

3. **Add jd-panel directive in your html**
   - this directive creates a bootstrap panel with few lines of code. The value setted in directive is the bootstrap col-lg size.
   ```html
   <form jd-panel="1" jd-title="My Panel">
      ...
   </form>
   ```
   * if you need, you can customize the html. In your angular run from app.js, override the constant jedi.layout.panel.PanelConfig and set your html. There are three attributes:
   - defaultElementClass: default class to apply in your element
   - defaultFormClass: default class to apply in the forms into your element
   - wrapSizeTpl: define html to write the div with col-lg if you define size to the panel.
   - useBoxedPage: define if the panel will be create a div.page wrap
   - containerFilter: jQuery filter to check if panel parent is a container for then create div.page
   - boxedPageTpl: define html to write on div.page element.
   - bodyTpl: define html to write arround of your element.
   - headerTpl: define html to write in the header panel.

4. **Add jd-modal directive in your html**
   - this directive creates a bootstrap modal panel with few lines of code. The value setted in directive is the bootstrap col-lg size.
   ```html
   <form jd-modal="1" jd-title="My Modal">
      ...
	  <div class="modal-footer">
	     ...
	  </div>
   </form>
   ```
   * if you need, you can customize the html. In your angular run from app.js, override the constant jedi.layout.modal.ModalConfig and set your html. There are three attributes:
   - defaultElementClass: default class to apply in your element
   - defaultFormClass: default class to apply in the forms into your element
   - defaultTableClass: default class to apply in the tables into your element
   - headerTpl: define html to write in the header panel.
   - titleTpl: define html to write the title content.
   - closeBtnTpl: define html to write the close button content.

5. **Add jd-input directive in your html**
   - This directive apply classes and htmls arround your input to improve bootstrap's power.

   - The value of the directive (if specified) will be bootstrap's col-lg size and the other sizes such as col-xs/col-sm/col-md will be calculated by proportion considering the value informed, *but only for the root element of the template*. In case you don't specify a value for the directive it will use the default values configured for all four sizes. The directive also add a validation tooltip when there is/are validation error(s) (see ng-jedi-utilities function applyValidationTooltip).

   - There are attributes that you can use to customize the input the way you want it:
   ```shell
      - Sizes (Correspond to the bootstrap classes col-xs/col-sm/col-md/col-lg. Values must be between 1 and 12):
         - jd-xs-size / jd-sm-size / jd-md-size / jd-lg-size (Size of the root div that wrap label + input).
         - jd-xs-label-size / jd-sm-label-size / jd-md-label-size / jd-lg-label-size (Label's size).
         - jd-xs-input-size / jd-sm-input-size / jd-md-input-size / jd-lg-input-size (Input's size).

      - Label 
         - jd-label (Correspond to the text that is displayed above the input).
         - jd-grouplabel (Correspond to the text that is displayed above the group of inputs - Multicheck/radio).

      - Help
         - jd-help (A small text displayed below the input for simple information about it).

      - Class
         - jd-input-class (Css classes that will be applied to the element(div) that wrap the input).
         - jd-label-class (Css classes that will be applied to the label).
         - jd-element-class (Css classes that will be applied to the input itself).
   ```
   * Keep in mind that if you use the Sizes attributes, it will override all configured values (default/proportion and specific) for the sizes.

   - **IMPORTANT: You will notice that some attributes of the inputs have a ```shell $parent``` tag. That's because of the transclude that is used in the directive. It creates a new scope for the cloned element, plus the directive itself has its own scope causing the model to be acessible only through the scope of the $parent.$parent element. In other cases through the $parent.$parent.$parent element because of ng-repeat.**

   *Examples:*

   - Type: Text
   ```html
   <input jd-input jd-label="MyInput" type="text" ng-model="myInput" />

   Becomes:

   <div class="col-xs-12 col-sm-3 col-md-3 col-lg-3>
      <div class="form-group">
         <label ng-if="showLabel" for="myInput" class="col-xs-12 col-sm-12 col-md-12 col-lg-12 control-label" jd-i18n="">MyInput</label>
            <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12 ">
               <input jd-input="" jd-label="MyInput" type="text" ng-model="myInput" id="myInput" name="myInput" jd-i18n="" class="form-control ng-pristine ng-valid ng-scope ng-touched" jd-validation-tooltip="">
            </div>
      </div>
   </div>
   ```

   - Type: Checkbox
   ```html
   <input jd-input jd-label="My Check" type="checkbox" ng-model="myCheck" ng-value="true">

   Becomes:

   <div class="col-xs-12 col-sm-3 col-md-3 col-lg-3 jd-checkbox">
      <div class="form-group">
         <div class="col-xs-offset-12 col-sm-offset-12 col-md-offset-12 col-lg-offset-12  col-xs-12 col-sm-12 col-md-12 col-lg-12 ">
            <div class="checkbox">
               <label class="ng-binding">
                  <input jd-input="" jd-label="My Check" type="checkbox" ng-model="myCheck" id="myCheck" name="myCheck" jd-i18n="" jd-validation-tooltip="" class="ng-pristine ng-untouched ng-valid ng-scope" ng-value="true">
                  My Check
                </label>
            </div>
         </div>
      </div>
   </div>
   ```

   - Type: Checkbox (Multi)
   ```html
   <input jd-input jd-grouplabel="My Multi Checks" jd-label="{{item.label}}" type="checkbox" ng-model="item.selected" ng-repeat="item in items" ng-value="item">

   Becomes:

   <div class="col-xs-12 col-sm-3 col-md-3 col-lg-3>
      <div class="form-group">
         <label ng-if="showLabel" class="col-xs-12 col-sm-12 col-md-12 col-lg-12 control-label" jd-i18n="">My Multi Checks</label>
         <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12 ">
            <label class="checkbox-inline">
               <input jd-input="" jd-grouplabel="My Multi Checks" jd-label="" type="checkbox" ng-model="$parent.$parent.$parent.data.checks[$parent.$index]" jd-repeat="item in list" id="data.checks[$index]" name="data.checks[$index]" jd-i18n="" jd-validation-tooltip="" class="ng-valid ng-scope ng-dirty ng-valid-parse ng-touched" style="" ng-value="[object Object]">
               value 1
            </label>
            <label class="checkbox-inline">
               <input jd-input="" jd-grouplabel="My Multi Checks" jd-label="" type="checkbox" ng-model="$parent.$parent.$parent.data.checks[$parent.$index]" jd-repeat="item in list" id="data.checks[$index]" name="data.checks[$index]" jd-i18n="" jd-validation-tooltip="" class="ng-valid ng-scope ng-dirty ng-valid-parse ng-touched" style="" ng-value="[object Object]">
               value 2
            </label>
         </div>
      </div>
   </div>
   ```
   
   - Type: Radio
   ```html
   <input jd-input jd-label="Radio" ng-model="myRadio" type="radio" ng-value="true"/>

   Becomes:

   <div class="col-xs-12 col-sm-3 col-md-3 col-lg-3 jd-radio">
      <div class="form-group">
         <div class="col-xs-offset-12 col-sm-offset-12 col-md-offset-12 col-lg-offset-12  col-xs-12 col-sm-12 col-md-12 col-lg-12 ">
            <div class="radio">
                   <label class="ng-binding">
                       <input jd-input="" jd-label="My Radio" ng-model="myRadio" type="radio" id="myRadio" name="myRadio" jd-i18n="" jd-validation-tooltip="" class="ng-pristine ng-untouched ng-valid ng-scope" ng-value="true">
                       My Radio
                   </label>
            </div>
         </div>
      </div>
   </div>   
   ```

   - Type: Radio (Multi)
   ```html
   <input jd-input jd-grouplabel="My Multi Radios" jd-label="{{item.value}}" ng-model="item.selected" type="radio" jd-repeat="item in list" ng-value="item" />

   Becomes:

   <div class="col-xs-12 col-sm-3 col-md-3 col-lg-3 jd-multi-radio">
      <div class="form-group">
         <label ng-if="showLabel" class="col-xs-12 col-sm-12 col-md-12 col-lg-12  control-label" jd-i18n="">Radios</label>
         <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12 ">
            <label class="radio-inline" ng-repeat="item in $parent.list">
               <input jd-input="" jd-grouplabel="Radios" jd-label="" ng-model="$parent.$parent.$parent.item.selected" type="radio" jd-repeat="item in list" ng-value="$parent.item" id="item.selected" name="item.selected" jd-i18n="" jd-validation-tooltip="" class="ng-pristine ng-untouched ng-valid ng-scope" value="[object Object]">
               value 1
            </label>
            <label class="radio-inline" ng-repeat="item in $parent.list">
               <input jd-input="" jd-grouplabel="Radios" jd-label="" ng-model="$parent.$parent.$parent.item.selected" type="radio" jd-repeat="item in list" ng-value="$parent.item" id="item.selected" name="item.selected" jd-i18n="" jd-validation-tooltip="" class="ng-pristine ng-untouched ng-valid ng-scope" value="[object Object]">
               value 2
            </label>
         </div>
      </div>
   </div>
   ```

   - Type: Select (1st Way)
   ```html
   <select jd-input jd-label="My Select 1" ng-model="mySelect1">
      <option ng-repeat="item in list track by item.id" value="{{item.id}}">{{item.value}}</option>
   </select>

   Becomes:

   <div class="col-xs-12 col-sm-3 col-md-3 col-lg-3 jd-select">
      <div class="form-group">
         <label ng-if="showLabel" for="mySelect1" class="col-xs-12 col-sm-12 col-md-12 col-lg-12  control-label" jd-i18n="">My Select 1</label>
         <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12 ">
            <select jd-input="" jd-label="My Select 1" ng-model="$parent.$parent.mySelect1" id="mySelect1" name="mySelect1" jd-i18n="" class="form-control ng-pristine ng-untouched ng-valid ng-scope" jd-validation-tooltip="">
               <option value="? undefined:undefined ?"></option>
               <option ng-repeat="item in list track by item.id" value="1" class="ng-binding ng-scope">value 1</option>
               <option ng-repeat="item in list track by item.id" value="2" class="ng-binding ng-scope">value 2</option>
               <option ng-repeat="item in list track by item.id" value="3" class="ng-binding ng-scope">value 3</option>
            </select>
         </div>
      </div>
   </div>
   ```
   
   - Type: Select (2nd Way)
   ```html
   <select jd-input jd-label="My Select 2" ng-model="mySelect2" jd-options="item.id as item.value for item in list"></select>

   Becomes:

   <div class="col-xs-12 col-sm-3 col-md-3 col-lg-3 jd-select">
      <div class="form-group">
         <label ng-if="showLabel" for="mySelect2" class="col-xs-12 col-sm-12 col-md-12 col-lg-12  control-label" jd-i18n="">My Select 2</label>
         <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12 ">
            <select jd-input="" jd-label="My Select 2" ng-model="$parent.$parent.mySelect2" jd-options="item.id as item.value for item in list" id="mySelect2" name="mySelect2" jd-i18n="" class="form-control ng-pristine ng-valid ng-scope ng-touched" ng-options="item.id as item.value for item in list" jd-validation-tooltip="">
               <option value="?" selected="selected"></option>
               <option value="number:1" label="value 1">value 1</option>
               <option value="number:2" label="value 2">value 2</option>
               <option value="number:3" label="value 3">value 3</option>
            </select>
         </div>
      </div>
   </div>
   ```

   - Type: Textarea
   ```html
   <textarea jd-input jd-label="My Textarea" ng-model="myTextarea"></textarea>

   Becomes:

   <div class="col-xs-12 col-sm-3 col-md-3 col-lg-3 jd-textarea">
      <div class="form-group">
         <label ng-if="showLabel" for="myTextarea" class="col-xs-12 col-sm-12 col-md-12 col-lg-12  control-label" jd-i18n="">My Textarea</label>
         <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12 ">
            <textarea jd-input="" jd-label="My Textarea" ng-model="myTextarea" id="myTextarea" name="myTextarea" jd-i18n="" class="form-control ng-pristine ng-valid ng-scope ng-touched" jd-validation-tooltip=""></textarea>
         </div>
      </div>
   </div>
   ```

   *Examples with attributes:*

   ```html
   <input jd-input="6" jd-help="I'm a tiny text below the input" jd-input-class="ImInTheDivThatWrapTheInput" jd-label-class="ImInTheLabel" jd-element-class="ImInTheInput" jd-label="MyInput's Label" type="text" ng-model="myInput" />

   Becomes:

   <div class="col-xs-12 col-sm-6 col-md-6 col-lg-6 jd-text">
      <div class="form-group">
         <label ng-if="showLabel" for="myInput" class="col-xs-12 col-sm-12 col-md-12 col-lg-12 ImInTheLabel control-label" jd-i18n="">MyInput's Label</label>
         <div class="col-xs-12 col-sm-12 col-md-12 col-lg-12 ImInTheDivThatWrapTheInput">
            <input jd-input="6" type="text" ng-model="myInput" id="myInput" name="myInput" jd-i18n="" class="form-control ng-pristine ng-valid ng-scope ImInTheInput ng-touched" jd-validation-tooltip="" style="">
            <small class="help-block ng-binding ng-scope" ng-if="showHelp" jd-i18n="" i18n-body="{{jdHelp}}">I'm a tiny text below the input</small>
         </div>
      </div>
   </div>
   ```
   As you can see the class 'ImInTheDivThatWrapTheInput' is in the input's parent element, the class 'ImInTheLabel' is in the label element and the class 'ImInTheInput' is in the input element.

   The ```html jd-help``` attribute adds a ```html small``` tag below the input with the provided text.

   Since we used ```html jd-input="6"``` notice that the sizes of the root element have changed in comparison with the examples previously discribed.

   Using the sizes attributes will result in the override of the classes (e.g.:
      • ```html jd-xs-size="4"``` will result the root element to have ```html col-xs-4``` instead of ```html col-xs-12```
      • ```html jd-sm-label-size="8"``` will result the label element to have ```html col-sm-8``` instead of ```html col-sm-12``` and will calculate the size for the input to be ```html col-sm-4``` instead of ```html col-sm-12```
      • ```html jd-md-input-size="10"``` will result the div wrapping the input to have ```html col-md-10``` instead of ```html col-md-12``` and will calculate the size for the label to be ```html col-md-2``` instead of ```html col-md-12```).


   * if you need, you can customize the html. In your angular run from app.js, override the constant jedi.layout.input.InputConfig and set your html. There are three attributes:

   - specificSizes: used by angular's $interpolate to determinate which sizes to use for a specific type of input. e.g.:
      - "{{(type === 'radio' || type === 'checkbox') && (jdRepeat == undefined || jdRepeat == '')}}": {
                xsSize: 12,
                smSize: 3,
                mdSize: 3,
                lgSize: 3,
                xsLabelSize: 0,
                smLabelSize: 0,
                mdLabelSize: 0,
                lgLabelSize: 0,
                xsInputSize: 12,
                smInputSize: 12,
                mdInputSize: 12,
                lgInputSize: 12,
            }

   - specificSizesProportion: used by angular's $interpolate to determinate which sizes to use in the root element for a specific type of input in case ```html jd-input``` has value, using the right proportion for each type of resolution/device. e.g.:
      - "{{(type === 'radio' || type === 'checkbox') && (jdRepeat == undefined || jdRepeat == '')}}": {
                "1": { mdSize: 3, smSize: 3, xsSize: 12 },
                ...
                "12": { mdSize: 4, smSize: 6, xsSize: 12 }
            }

   - lgSizesProportion: the size proportion that will be used in the root element for non-specific inputs in case ```html jd-input``` is specified. e.g.:
      -  "1": { mdSize: 1, smSize: 2, xsSize: 12 },
         ...
         "12": { mdSize: 12, smSize: 12, xsSize: 12 }

   - defaultSizes: the default sizes for the input, if none are defined. e.g:
      -  xsSize: 12,
         smSize: 3,
         mdSize: 3,
         lgSize: 3,
         xsLabelSize: 12,
         smLabelSize: 12,
         mdLabelSize: 12,
         lgLabelSize: 12,
         xsInputSize: 12,
         smInputSize: 12,
         mdInputSize: 12,
         lgInputSize: 12

   - templateSelector: used by angular's $interpolate to determinate which templates to use for specific inputs. e.g.:
      - "{{(type === 'radio' || type === 'checkbox')}}": 'your/template/path/template.html',

   - defaultTemplate: The url for the default template. Define if you want to use your own default (String).
   - useValidationTooltip: define if the input will use the validation tooltip to show erros (Boolean).