<template>
  <div class="list-item" :class="{ '--active': active }" @click="$emit('click')">
    <!-- Avatar -->
    <div class="list-item-avatar">
      <CompanyLogo :url="data.url" />
    </div>
    <!-- Summary -->
    <div class="list-item-summary">
      <span v-text="data.title" class="url" />
    </div>
    <!-- Detail -->
    <button class="list-item-detail">
      <VIcon name="chevron-right" size="14px" />
    </button>
  </div>
</template>

<script>
export default {
  name: 'ListItem',

  props: {
    data: {
      type: Object,
      default: () => ({})
    },
    active: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang="scss">
.list-item {
  height: 64px;
  display: flex;
  cursor: pointer;
  align-items: center;
  justify-content: space-between;
  background-color: black;
  border-bottom: 1px solid black;

  &.--active {
    background: $color-gray-600;
  }

  &-avatar {
    width: 32px;
    height: 32px;
    border-radius: 8px;
    background-color: $color-gray-400;
    margin-left: $spacer-3;
  }

  &-summary {
    color: #fff;
    margin: 0 auto 0 $spacer-3;
    display: flex;
    flex-direction: column;

    .url {
      font-weight: 600;
      font-size: $font-size-normal;
      line-height: 16px;
    }

    .username {
      font-weight: normal;
      font-size: $font-size-mini;
      line-height: 16px;
    }
  }

  &-detail {
    margin: 0 $spacer-3 0 auto;
    color: $color-gray-300;
  }
}
</style>
