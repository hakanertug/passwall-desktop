<template>
  <div class="content-container">
    <div class="items-container">
      <!-- Search -->
      <div class="search-wrapper">
        <VFormText
          :placeholder="$t('Search passwords, websites, notes')"
          theme="black"
          v-model="searchQuery"
          v-debounce:300="fetchAll"
          class="w-100"
        />
      </div>
      <!-- Items -->
      <PerfectScrollbar class="logins">
        <!-- List -->
        <template v-if="ItemList.length > 0">
          <ListItem
            v-for="item in ItemList"
            :key="item.id"
            :active="$route.params.id == item.id"
            :data="item"
            @click="onClickItem(item.id)"
          />
        </template>
        <!-- Empty State -->
        <div v-else class="flex-center flex-column c-gray-700 h-100">
          <VIcon name="logo-outline" size="48" class="mt-n7" />
          <span class="fs-big mt-3" v-text="$t('There is nothing here, yet...')" />
        </div>
      </PerfectScrollbar>

      <!-- Add Item Menu -->
      <AddItemMenu :active="itemMenuActive" @hide="itemMenuActive = false" />

      <!-- Add Btn -->
      <button
        class="add-item-menu-btn"
        :class="{ '--active': itemMenuActive }"
        @click="itemMenuActive = !itemMenuActive"
      >
        <VIcon name="plus" class="c-white" size="18px" />
      </button>
    </div>
    <!-- Detail -->
    <div
      class="w-100 h-100 flex-center c-gray-700"
      v-if="ItemList.length > 0 && $route.name == 'Notes'"
    >
      {{ $t('Select one item to see it’s details...') }}
    </div>
    <RouterView />
  </div>
</template>

<script>
import { mapActions, mapState } from 'vuex'

export default {
  data() {
    return {
      searchQuery: '',
      itemMenuActive: false,
      emptyCenterStateActive: false
    }
  },

  beforeRouteUpdate(to, from, next) {
    if (to.params.refresh) {
      this.fetchAll()
    }
    next()
  },

  created() {
    this.fetchAll()
  },

  methods: {
    ...mapActions('Notes', ['FetchAll']),

    async fetchAll() {
      try {
        await this.FetchAll({ Search: this.searchQuery })

        if (this.ItemList.length > 0) {
          this.onClickItem(this.ItemList[0].id)
        }
      } catch (error) {
        console.log(error)
      }
    },

    onClickItem(id) {
      this.$router.push({
        name: 'NoteDetail',
        params: { id }
      })
    }
  },

  computed: {
    ...mapState('Notes', ['ItemList'])
  }
}
</script>

<style lang="scss">
.content-container {
  width: 100%;
  display: flex;
}

.items-container {
  position: relative;
  display: flex;
  flex-direction: column;
  min-width: 295px;
  max-width: 295px;
  height: 100vh;
}

.logins {
  height: 100%;
}

.search-wrapper {
  display: flex;
  align-items: center;
  background-color: $color-gray-600;
  padding: 0 $spacer-3;
  min-height: 64px;
  height: 64px;
  border-bottom: 1px solid black;
}
</style>
