<template>
  <div class="modal__inner-1" role="dialog" aria-label="Publish to Blogger Page">
    <div class="modal__inner-2">
      <div class="modal__image">
        <icon-provider provider-id="bloggerPage"></icon-provider>
      </div>
      <p>This will publish <b>{{currentFileName}}</b> to your <b>Blogger Page</b>.</p>
      <form-entry label="Blog URL" error="blogUrl">
        <input slot="field" class="textfield" type="text" v-model.trim="blogUrl" @keyup.enter="resolve()">
        <div class="form-entry__info">
          <b>Example:</b> http://example.blogger.com/
        </div>
      </form-entry>
      <form-entry label="Existing page ID (optional)">
        <input slot="field" class="textfield" type="text" v-model.trim="pageId" @keyup.enter="resolve()">
      </form-entry>
      <form-entry label="Template">
        <select slot="field" class="textfield" v-model="selectedTemplate" @keyup.enter="resolve()">
          <option v-for="(template, id) in allTemplates" :key="id" :value="id">
            {{ template.name }}
          </option>
        </select>
        <div class="form-entry__actions">
          <a href="javascript:void(0)" @click="configureTemplates">Configure templates</a>
        </div>
      </form-entry>
      <div class="modal__info">
        <b>ProTip:</b> You can provide a value for <code>title</code> in the <a href="javascript:void(0)" @click="openFileProperties">file properties</a>.
      </div>
      <div class="modal__button-bar">
        <button class="button" @click="config.reject()">Cancel</button>
        <button class="button" @click="resolve()">Ok</button>
      </div>
    </div>
  </div>
</template>

<script>
import bloggerPageProvider from '../../services/providers/bloggerPageProvider';
import modalTemplate from './modalTemplate';

export default modalTemplate({
  data: () => ({
    pageId: '',
  }),
  computedLocalSettings: {
    blogUrl: 'bloggerBlogUrl',
    selectedTemplate: 'bloggerPublishTemplate',
  },
  methods: {
    resolve() {
      if (!this.blogUrl) {
        this.setError('blogUrl');
      } else {
        // Return new location
        const location = bloggerPageProvider.makeLocation(
          this.config.token, this.blogUrl, this.pageId);
        location.templateId = this.selectedTemplate;
        this.config.resolve(location);
      }
    },
  },
});
</script>
