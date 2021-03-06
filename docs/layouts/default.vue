<template>
  <div class="pt-16">
    <Navbar />
    <main class="container mx-auto px-4 lg:px-8">
      <div class="flex flex-wrap relative">
        <aside
          class="h-screen w-full lg:w-1/5 fixed lg:sticky top-0 left-0 bottom-0 pt-16 lg:-mt-16 lg:block bg-white dark:bg-gray-900 lg:bg-transparent z-30 lg:border-r dark:border-gray-800"
          :class="{ 'block': menu, 'hidden': !menu }"
        >
          <div class="container mx-auto overflow-auto h-full">
            <div class="lg:hidden flex-1 flex justify-center px-4 mt-8 mb-4 w-full">
              <SearchInput />
            </div>
            <ul class="lg:pl-0 p-4 lg:py-8 lg:pr-8">
              <li v-for="(docs, category) in categories" :key="category" class="mb-6 last:mb-0">
                <h3 class="aside-title">{{ category }}</h3>
                <ul>
                  <li v-for="doc of docs" :key="doc.slug">
                    <NuxtLink
                      :to="toLink(doc.slug)"
                      class="px-2 rounded font-medium py-1 block text-gray-600 dark:text-gray-500 hover:text-gray-800 dark-hover:text-gray-100"
                      exact-active-class="text-green-600 bg-green-100 hover:text-green-600 dark:text-green-200 dark:bg-green-900 dark-hover:text-green-200"
                    >{{ doc.title }}</NuxtLink>
                  </li>
                </ul>
              </li>
            </ul>
            <h3 class="lg:hidden aside-title px-4">More</h3>
            <div class="lg:hidden flex items-center px-4">
              <a
                href="https://twitter.com/nuxt_js"
                target="_blank"
                rel="noopener noreferrer"
                title="Twitter"
                name="Twitter"
                class="hover:text-green-500 mr-4 ml-2"
              >
                <icon-twitter class="w-6 h-6" />
              </a>

              <a
                href="https://github.com/nuxt/content"
                target="_blank"
                rel="noopener noreferrer"
                title="Github"
                name="Github"
                class="hover:text-green-500 mr-4"
              >
                <icon-github class="w-6 h-6" />
              </a>
              <LangSwitcher class="mr-4" />
              <ColorSwitcher />
            </div>
          </div>
        </aside>
        <Nuxt class="w-full lg:w-4/5" />
      </div>
    </main>
    <TheFooter />
  </div>
</template>

<script>
import Navbar from '@/components/Navbar'
import TheFooter from '@/components/TheFooter'
import SearchInput from '@/components/SearchInput'

export default {
  components: {
    Navbar,
    TheFooter,
    SearchInput
  },
  computed: {
    bodyClass () {
      return this.$store.state.menu.open ? ['h-screen lg:h-auto overflow-y-hidden lg:overflow-y-auto'] : []
    },
    menu: {
      get () {
        return this.$store.state.menu.open
      },
      set (val) {
        this.$store.commit('menu/toggle', val)
      }
    },
    categories () {
      return this.$store.state.categories[this.$i18n.locale]
    }
  },
  methods: {
    toLink (slug) {
      if (slug === 'index') {
        return this.localePath('slug')
      }
      return this.localePath({ name: 'slug', params: { slug } })
    }
  },
  head () {
    const i18nSeo = this.$nuxtI18nSeo()

    return {
      bodyAttrs: {
        class: [...this.bodyClass, 'antialiased text-gray-800 leading-normal bg-white dark:bg-gray-900 dark:text-gray-100']
      },
      ...i18nSeo
    }
  }
}
</script>

<style lang="postcss">
.aside-title {
  @apply mb-3 text-gray-500 uppercase tracking-wide font-bold text-sm;
}
.dark-mode .aside-title {
  @apply text-gray-600;
}
@screen lg {
  .aside-title {
    @apply mb-2 text-xs;
  }
}
</style>
