<template>
  <div id="docsearch">
    <!-- DO NOT CHANGE: this code is just a placeholder -->
    <button type="button" class="DocSearch-Button" aria-label="Search">
      <svg width="20" height="20" class="DocSearch-Search-Icon" viewBox="0 0 20 20">
        <path
          d="M14.386 14.386l4.0877 4.0877-4.0877-4.0877c-2.9418 2.9419-7.7115 2.9419-10.6533 0-2.9419-2.9418-2.9419-7.7115 0-10.6533 2.9418-2.9419 7.7115-2.9419 10.6533 0 2.9419 2.9418 2.9419 7.7115 0 10.6533z"
          stroke="currentColor"
          fill="none"
          fill-rule="evenodd"
          stroke-linecap="round"
          stroke-linejoin="round"
        ></path>
      </svg>
    </button>
  </div>
</template>

<script>
function isSpecialClick(event) {
  return event.button === 1 || event.altKey || event.ctrlKey || event.metaKey || event.shiftKey
}

export default {
  props: {
    options: {
      type: Object,
      required: true
    },
    settings: {
      type: Object,
      required: true
    }
  },
  watch: {
    '$i18n.locale'(newValue) {
      this.update(this.options, newValue)
    },
    options(newValue) {
      this.update(newValue, this.$i18n.locale)
    }
  },
  mounted() {
    this.initialize(this.options, this.$i18n.locale)
  },
  methods: {
    stripTrailingSlash(url) {
      return url.replace(/\/$|\/(?=\?)|\/(?=#)/g, '')
    },
    getRelativePath(absoluteUrl) {
      const { pathname, hash } = new URL(absoluteUrl)
      const url = pathname.replace(this.settings.url, '/') + hash
      return this.stripTrailingSlash(url)
    },
    initialize(userOptions, code) {
      const lang = this.$i18n.locales.find(locale => locale.code === code)

      Promise.all([
        import(/* webpackChunkName: "docsearch" */ '@docsearch/js'),
        import(/* webpackChunkName: "docsearch" */ '@docsearch/css')
      ]).then(([docsearch]) => {
        docsearch = docsearch.default

        docsearch(
          Object.assign({}, userOptions, {
            container: '#docsearch',
            searchParameters: Object.assign(
              {},
              lang && {
                facetFilters: [`${userOptions.langAttribute || 'language'}:${lang.iso}`].concat(
                  userOptions.facetFilters || []
                )
              }
            ),
            navigator: {
              navigate: ({ suggestionUrl }) => {
                const { pathname: hitPathname } = new URL(window.location.origin + suggestionUrl)

                // Vue Router doesn't handle same-page navigation so we use
                // the native browser location API for anchor navigation.
                if (this.$router.history.current.path === hitPathname) {
                  window.location.assign(window.location.origin + suggestionUrl)
                } else {
                  this.$router.push(suggestionUrl)
                }
              }
            },
            transformItems: items => {
              return items.map(item => {
                return Object.assign({}, item, {
                  url: this.getRelativePath(item.url)
                })
              })
            },
            hitComponent: ({ hit, children }) => {
              return {
                type: 'a',
                ref: undefined,
                constructor: undefined,
                key: undefined,
                props: {
                  href: hit.url,
                  onClick: event => {
                    if (isSpecialClick(event)) {
                      return
                    }

                    // We rely on the native link scrolling when user is
                    // already on the right anchor because Vue Router doesn't
                    // support duplicated history entries.
                    if (this.$router.history.current.fullPath === hit.url) {
                      return
                    }

                    const { pathname: hitPathname } = new URL(window.location.origin + hit.url)

                    // If the hits goes to another page, we prevent the native link behavior
                    // to leverage the Vue Router loading feature.
                    if (this.$router.history.current.path !== hitPathname) {
                      event.preventDefault()
                    }

                    this.$router.push(hit.url)
                  },
                  children
                }
              }
            }
          })
        )
      })
    },
    update(options, lang) {
      this.$el.innerHTML = '<div id="docsearch"></div>'
      this.initialize(options, lang)
    }
  }
}
</script>

<style lang="postcss">
.DocSearch {
  --docsearch-primary-color: #00dc82;
  --docsearch-highlight-color: var(--docsearch-primary-color);
  --docsearch-text-color: var(--color-gray-700);
  --docsearch-modal-background: theme('colors.gray.100');
  --docsearch-searchbox-shadow: inset 0 0 0 2px var(--docsearch-primary-color);
  --docsearch-searchbox-background: var(--color-gray-200);
  --docsearch-searchbox-focus-background: var(--color-gray-200);
  --docsearch-hit-color: var(--color-gray-700);
  --docsearch-muted-color: var(--color-gray-500);
  /* bg-gray-400 with 0.8 opacity */
  --docsearch-container-background: rgb(244 244 245 / 55%);
}

.DocSearch-Container {
  @apply p-4 blur-8;
}

.DocSearch-Modal {
  @apply rounded-xl overflow-hidden h-auto !important;
  .DocSearch-SearchBar {
    @apply bg-gray-50 dark:bg-secondary-darkest p-2;
  }
}

.DocSearch-Footer {
  @apply relative !important;
}

.DocSearch-Button {
  @apply h-12 ml-auto relative rounded-lg flex items-center justify-center bg-transparent border-0 text-gray-500 dark:text-gray-600 hover:text-gray-600 transition-colors ring-0 px-3 !important;
}

.DocSearch-Button-Placeholder {
  @apply hidden px-3 font-medium !important;
}

.DocSearch-Search-Icon {
  @apply text-current w-5 h-5 d-icon !important;
  stroke-width: 2;
}

.DocSearch-Button-Key {
  @apply hidden bg-none font-medium top-0 relative rounded h-5 w-5 items-center justify-center border border-gray-300 dark:border-gray-600 text-gray-500 dark:text-gray-400 shadow-none p-1 text-xs mr-0.5 !important;
}

.DocSearch-Commands {
  @apply hidden !important;
}

.DocSearch-Screen-Icon > svg {
  display: inline !important;
}

.DocSearch-Input {
  color: #fff;
}
.dark {
  & .DocSearch {
    --docsearch-text-color: #fff;
    /* --docsearch-text-color: var(--color-gray-300); */
    --docsearch-container-background: rgb(0 30 38 / 64%);
    --docsearch-modal-background: #003543;
    --docsearch-modal-shadow: inset 1px 1px 0 0 #052f14, 0 3px 8px 0 #0b160d;
    /* --docsearch-searchbox-background: var(--color-gray-800); */
    --docsearch-searchbox-focus-background: rgba(1, 42, 53, 1);
    --docsearch-hit-color: var(--color-gray-300);
    --docsearch-highlight-color: var(--docsearch-primary-color);
    --docsearch-hit-shadow: none;
    --docsearch-hit-background: var(--color-gray-800);
    --docsearch-key-gradient: linear-gradient(-26.5deg, #565872, #31355b);
    --docsearch-key-shadow: inset 0 -2px 0 0 #282d55, inset 0 0 1px 1px #51577d, 0 2px 2px 0 rgba(3, 4, 9, 0.3);
    --docsearch-footer-background: rgba(1, 42, 53, 1);
    --docsearch-footer-shadow: inset 0 1px 0 0 rgba(73, 76, 106, 0.5), 0 -4px 8px 0 rgba(0, 0, 0, 0.2);
    --docsearch-logo-color: #fff;
    --docsearch-muted-color: var(--color-gray-500);
  }
}
</style>
