<template>
  <div>
    <div class="tab-container">
      <ul
        class="overflow-x-auto md:overflow-x-hidden flex flex-row flex-no-wrap items-stretch border-b-2 border-solid border-gray-300"
      >
        <li
          v-for="(tab, index) in tabs"
          :key="index"
          :class="['relative cursor-pointer px-4 py-4 sm:px-6 whitespace-no-wrap select-none hover:bg-gray-200', tab.id === activeTabId && 'border-b-4 border-solid border-brand-green']"
          @click="activeTabId = tab.id"
        >
          <a :class="['text-lg', tab.id === activeTabId && 'font-bold text-brand-green']">
            {{ tab.title }}
          </a>
        </li>
      </ul>
      <br>
      <component
        :is="tab.component"
        v-for="(tab, index) in tabs"
        v-show="tab.id === activeTabId"
        :key="index"
      />
    </div>
  </div>
</template>

<script>
import { faChevronDown } from '@fortawesome/free-solid-svg-icons'
export default {
  data () {
    return {
      icon: {
        faChevronDown
      },
      tabs: [
        {
          id: 'stat',
          title: 'Statistik',
          component: () => import('./DataSummary')
        },
        {
          id: 'map-kota',
          title: 'Peta Sebaran Kota/Kab',
          component: () => import('../../Tableau/PetaSebaranKotaKabupaten')
        },
        {
          id: 'map-kel',
          title: 'Peta Titik Sebaran Kasus',
          component: () => import('../../Tableau/PetaPersebaranAllCases')
        }
      ],
      activeTabId: 'stat',
      currentX: null
    }
  },
  computed: {
    tabComponent () {
      if (!this.activeTabId) {
        return null
      }
      return this.tabs.find(tab => tab.id === this.activeTabId).component || null
    }
  }
}
</script>

<style lang="scss" scoped>
.tab-container {
  //
}
</style>
