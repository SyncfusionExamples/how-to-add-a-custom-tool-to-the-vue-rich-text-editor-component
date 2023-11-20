<template>
  <div>
    <ejs-richtexteditor
      ref="rteInstance"
      height="200px"
      width="700px"
      :toolbarSettings="toolbarSettings">
      <p>
        The Rich Text Editor component is a WYSIWYG ("what you see is what you
        get") editor that provides the best user experience to create and update
        the content. Users can format their content using standard toolbar
        commands.
      </p>
    </ejs-richtexteditor>
    <ejs-dialog
      id="rteDialog"
      :buttons="dlgButtons"
      :header="header"
      ref="dialogObj"
      :overlayClick="dialogOverlay"
      :visible="visible"
      minHeight="320px"
      width="30%"
      :showCloseIcon="showCloseIcon"
      :isModal="modal"
      :created="dialogCreate"
      :content="'contentTemplate'">
        <template v-slot:contentTemplate>
          <div id="customTbarDialog" >
          <div id="rteSpecial_char">
            <div class="char_block" title="&#94;">&#94;</div>
            <div class="char_block" title="&#95;">&#95;</div>
            <div class="char_block" title="&#96;">&#96;</div>
            <div class="char_block" title="&#123;">&#123;</div>
            <div class="char_block" title="&#124;">&#124;</div>
            <div class="char_block" title="&#125;">&#125;</div>
            <div class="char_block" title="&#126;">&#126;</div>
            <div class="char_block" title="&#160;">&#160;</div>
            <div class="char_block" title="&#161;">&#161;</div>
            <div class="char_block" title="&#162;">&#162;</div>
            <div class="char_block" title="&#163;">&#163;</div>
            <div class="char_block" title="&#164;">&#164;</div>
            <div class="char_block" title="&#165;">&#165;</div>
            <div class="char_block" title="&#x20B9;">&#x20B9;</div>
            <div class="char_block" title="&#166;">&#166;</div>
            <div class="char_block" title="&#167;">&#167;</div>
            <div class="char_block" title="&#168;">&#168;</div>
            <div class="char_block" title="&#169;">&#169;</div>
            <div class="char_block" title="&#170;">&#170;</div>
            <div class="char_block" title="&#171;">&#171;</div>
            <div class="char_block" title="&#172;">&#45;</div>
            <div class="char_block" title="&#173;">&#173;</div>
            <div class="char_block" title="&#174;">&#174;</div>
            <div class="char_block" title="&#175;">&#175;</div>
            <div class="char_block" title="&#176;">&#176;</div>
            <div class="char_block" title="&#177;">&#177;</div>
            <div class="char_block" title="&#178;">&#178;</div>
            <div class="char_block" title="&#179;">&#179;</div>
            <div class="char_block" title="&#180;">&#180;</div>
            <div class="char_block" title="&#181;">&#181;</div>
            <div class="char_block" title="&#182;">&#182;</div>
            <div class="char_block" title="&#183;">&#183;</div>
            <div class="char_block" title="&#184;">&#184;</div>
            <div class="char_block" title="&#185;">&#185;</div>
            <div class="char_block" title="&#186;">&#186;</div>
            <div class="char_block" title="&#187;">&#187;</div>
            <div class="char_block" title="&#188;">&#188;</div>
            <div class="char_block" title="&#189;">&#189;</div>
            <div class="char_block" title="&#190;">&#190;</div>
            <div class="char_block" title="&#191;">&#191;</div>
            <div class="char_block" title="&#192;">&#192;</div>
            <div class="char_block" title="&#193;">&#193;</div>
            <div class="char_block" title="&#194;">&#194;</div>
            <div class="char_block" title="&#195;">&#195;</div>
          </div>
    </div>
  </template></ejs-dialog>
    
  </div>
</template>

<script>
import {
  RichTextEditorComponent,
  Toolbar,
  Image,
  HtmlEditor,
  Link,
  Table,
  NodeSelection,
} from "@syncfusion/ej2-vue-richtexteditor";
import { DialogComponent } from "@syncfusion/ej2-vue-popups";

export default {
  name: "App",
  components: {
    "ejs-richtexteditor": RichTextEditorComponent,
    "ejs-dialog": DialogComponent,
  },
  methods: {
    onCreate: function (e) {
      this.customBtn = document.getElementById("custom_tbar");
    },
    customButtonClick: function (e) {
      var proxy = this;
      proxy.$refs.rteInstance.ej2Instances.contentModule.getEditPanel().focus();
      proxy.range = proxy.selection.getRange(document);
      proxy.saveSelection = proxy.selection.save(proxy.range, document);
      
      proxy.$refs.dialogObj.show();
    },
    dialogCreate: function () {
      var dialogCtn = document.getElementById("rteSpecial_char");
      var proxy = this;
      dialogCtn.onclick = function (e) {
        var target = e.target;
        var activeEle = proxy.$refs.dialogObj.$el.querySelector(
          ".char_block.e-active"
        );
        if (target.classList.contains("char_block")) {
          target.classList.add("e-active");
          if (activeEle) {
            activeEle.classList.remove("e-active");
          }
        }
      };
    },
    onInsert: function () {
      var activeEle = this.$refs.dialogObj.$el.querySelector(
        ".char_block.e-active"
      );
      if (activeEle) {
       
        this.$refs.rteInstance.ej2Instances.executeCommand(
          "insertText",
          activeEle.textContent
        );
        this.$refs.dialogObj.hide();
      }
      
    },
    
  },
  data: function () {
    return {
      selection: new NodeSelection(),
      range: null,
      customBtn: null,
      dialogCtn: null,
      saveSelection: new NodeSelection(),
      header: "Select the Special Characters",
      visible: false,
      modal: true,
      showCloseIcon: false,
      dlgButtons: [
        {
          click: this.onInsert.bind(this),
          buttonModel: { isPrimary: "true", content: "Insert" },
        },
       
      ],

      toolbarSettings: {
        items: [
          "Bold",
          "Italic",
          "Undo",
          "Redo",
          "CreateTable",
          "Image",
          {
            tooltipText: "Insert Symbol",
            template:
              '<button class="e-tbar-btn e-control e-btn e-lib e-icon-btn" tabindex="-1" id="custom_tbar" style="width:100%"><span class="e-tbar-btn-text" style=" font-size: 16px; font-weight: 600;">&#937;</span></button>',
            click: this.customButtonClick.bind(this),
          },
        ],
      },
    };
  },
  provide: {
    richtexteditor: [Toolbar, Image, HtmlEditor, Link, Table],
  },
};
</script>

<style>
@import "../node_modules/@syncfusion/ej2-base/styles/material.css";
@import "../node_modules/@syncfusion/ej2-inputs/styles/material.css";
@import "../node_modules/@syncfusion/ej2-lists/styles/material.css";
@import "../node_modules/@syncfusion/ej2-popups/styles/material.css";
@import "../node_modules/@syncfusion/ej2-buttons/styles/material.css";
@import "../node_modules/@syncfusion/ej2-navigations/styles/material.css";
@import "../node_modules/@syncfusion/ej2-splitbuttons/styles/material.css";
@import "../node_modules/@syncfusion/ej2-vue-richtexteditor/styles/material.css";
@import "../node_modules/@syncfusion/ej2-vue-popups/styles/material.css";

#rteSpecial_char .char_block {
  display: inline-block;
}

#rteSpecial_char {
  padding: 15px 0 15px 0;
}

#rteSpecial_char .char_block.e-active {
  outline: 1px solid #e3165b;
  border-color: #e3165b;
}

#rteSpecial_char .char_block {
  width: 30px;
  height: 30px;
  line-height: 30px;
  margin: 0 5px 5px 0;
  text-align: center;
  vertical-align: middle;
  border: 1px solid #dddddd;
  font-size: 20px;
  cursor: pointer;
  user-select: none;
}
</style>
