<template>
  <div class="container" :id="slugged">
    <div class="article-text-block">
      <div>
        <h2 v-html="title" :data-slug="hashSlug"></h2>
        <Button class="primary" v-if="buttonTarget">
          <router-link :to="`/${$i18n.locale}${buttonTarget}`" v-if="buttonType === 'int'">
            {{buttonLabel}}
          </router-link>
          <a :href="`${buttonTarget}`" target="_blank" v-if="buttonType === 'ext'">
            {{buttonLabel}}
          </a>
        </Button>
      </div>
      <div>
        <p v-html="content"></p>
        <template v-if="code && language">
          <p>
            <pre><code :class="`language-${language}`">{{ code }}</code></pre>
          </p>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import slugify from 'slugify';
import Prism from 'prismjs';
import 'prismjs/components/prism-clike';
import 'prismjs/components/prism-javascript';
import 'prismjs/components/prism-bash';
import 'prismjs/components/prism-c';
import 'prismjs/components/prism-csharp';
import 'prismjs/components/prism-cpp';
import 'prismjs/components/prism-cmake';
import 'prismjs/components/prism-docker';
import 'prismjs/components/prism-git';
import 'prismjs/components/prism-go';
import 'prismjs/components/prism-java';
import 'prismjs/components/prism-markdown';
import 'prismjs/components/prism-python';
import 'prismjs/components/prism-rust';
import 'prismjs/components/prism-typescript';
import 'prismjs/components/prism-json';
import 'prismjs/components/prism-yaml';
import 'prismjs/themes/prism.css';

export default {
  name: 'ArticleCodeblock',
  components: {},
  //---------------------------------------------------
  //
  //  Properties
  //
  //---------------------------------------------------
  props: {
    title: String,
    content: String,
    code: {
      type: String,
      default: null,
    },
    language: {
      type: String,
      default: null,
    },
    buttonLabel: {
      type: String,
      default: null,
    },
    buttonTarget: {
      type: String,
      default: null,
    },
    buttonType: {
      type: String,
      default: null,
    },
  },
  //---------------------------------------------------
  //
  //  Data model
  //
  //---------------------------------------------------
  data() {
    return {};
  },
  //---------------------------------------------------
  //
  //  Computed Properties
  //
  //---------------------------------------------------
  computed: {
    slugged() {
      if (this.title) {
        return this.title.toLowerCase()
          .replace(/\s/g, '-')
          .replace(/(<([^>]+)>)/ig, '')
          .replace(/[~`!@#$%^&*()+={}[\];:'"<>.,/\\?]/g, '');
      }
      return '';
    },
    hashSlug() {
      const title = (this.title || '').replace(/<\/?[^>]+(>|$)/g, '');
      return slugify(title, { lower: true, strict: true });
    },
  },
  //---------------------------------------------------
  //
  //  Watch Properties
  //
  //---------------------------------------------------
  watch: {},
  //---------------------------------------------------
  //
  //  Filter Properties
  //
  //---------------------------------------------------
  // filters: {},
  //---------------------------------------------------
  //
  //  Validation Properties
  //
  //---------------------------------------------------
  // validations: {},
  //---------------------------------------------------
  //
  //  Vue Lifecycle
  //
  //---------------------------------------------------
  // beforeCreate() {},
  // created() {},
  // beforeMount() {},
  // render(h) { return h(); },
  mounted() {
    if (this.code && this.language) {
      Prism.highlightAll();
    }
  },
  // beforeUpdate() {},
  // updated() {},
  // beforeDestroy() {},
  // destroyed() {},
  //---------------------------------------------------
  //
  //  Methods
  //
  //---------------------------------------------------
  methods: {
    //----------------------------------
    // Event Handlers
    //----------------------------------
  },
};
</script>

<style lang="scss" scoped>
@import '~scss/mixins';
@import '~scss/variables';

.article-text-block {
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  padding-top: 80px;
  width: 100%;
  gap: 32px;

  @include breakpoint('sm') {
    padding-top: 32px;
  }

  > div:first-child {
    flex: 1 0 40%;
    padding-right: 16px;
    position: sticky;
    max-width: 40%;
    top: 110px;

    @include breakpoint('sm') {
      flex: initial;
      padding-right: 0;
      position: initial;
      top: initial;
    }

    h2 {
      margin-top: 0;
    }

    @include breakpoint('sm') {
      h2 {
        margin-top: 0;
        margin-bottom: 32px;
      }
    }
  }

  > div:last-child {
    width: 100%;
  }

  p {
    @include style-body('large');
    font-weight: 400;
    flex: 1 0 52%;
    width: 100%;
    min-width: 52%;

    ::v-deep a {
      color: var(--color-blue);
      text-decoration: underline;
      @include style-body('large');
      word-break: break-all;
    }

    ::v-deep pre {
      font-size: 16px;
      line-height: 1;
      font-weight: normal;
      box-shadow: 0 0 8px -2px rgba(0, 0, 0, .1);
      letter-spacing: normal;
    }
  }

  @include breakpoint('sm') {
    flex-direction: column;
  }
}

div.container {
  margin: 0 auto;
  padding: 0 20px !important;
}
</style>
