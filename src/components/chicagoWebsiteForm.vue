<template lang="html">
  <div class="chicagoWebsiteForm">

        <p ref="ref1" class="clickable" v-on:click="copy('ref1')">{{lName}}, {{fName}}. "{{Title}}." {{Website}}. {{url}} (accessed {{Month}} {{Day}}, {{Year}})</p>

        <p v-on:click="copy('ref1_text')" class="clickable"><b>In text:</b> <span ref="ref1_text"> ({{lName}}, {{Year}})</span></p>

    <form class="pure-form pure-form-aligned">
        <fieldset>

            <div class="pure-control-group">
                <label for="lName">Last Name</label>
                <input v-model="lName" placeholder="Last Name">
            </div>

            <div class="pure-control-group">
                <label for="fName">First Name</label>
                <input v-model="fName" placeholder="First Name">
            </div>

            <div class="pure-control-group">
                <label for="Title">Page Title</label>
                <input v-model="Title" placeholder="Page Title">
            </div>

            <div class="pure-control-group">
                <label for="Website">Website Name</label>
                <input v-model="Website" placeholder="Owner, if unknown">
            </div>

            <div class="pure-control-group">
                <label for="url">URL of article</label>
                <input v-model="url" placeholder="URL of article">
            </div>

            <div class="pure-control-group">
                <label for="Month">Month Published</label>
                <input v-model="Month" placeholder="Month published">
            </div>

            <div class="pure-control-group">
                <label for="Day">Date Published</label>
                <input v-model="Day" placeholder="Date published">
            </div>

            <div class="pure-control-group">
                <label for="Year">Year Published</label>
                <input v-model="Year" placeholder="Year Published">
            </div>

        </fieldset>

        <custom-footer v-on:save="test" v-on:copy="copy" v-on:clear="clearForm"></custom-footer>

    </form>
  </div>
</template>

<script>
import * as utils from '../assets/utils.js'
import Footer from './footer.vue'

const { clipboard } = require('electron')

export default {
  name: 'book',
  data: function () {
      return utils.initialState();
  },
  methods: {
      clearForm: function () {
          for (let prop in utils.initialState()) {
              this[prop] = '';
          }
      },
      test: function () {
          alert("hello");
      },
      copy: function(ref) {
          var text = this.$refs[ref].innerHTML
          var richText = utils.convertHtmlToRtf(this.$refs[ref].innerHTML)
          var plainText = text.replace(/<(?:.|\n)*?>/gm, '');
          clipboard.writeRTF(richText)
          let myNotification = new Notification('Bibliography copied!', {
            body: plainText
          })
      },
  },
  components: {
      'custom-footer': Footer,
  }
}
</script>
