<template>
  <RouterLink
    class="sidebar-menu-item"
    :class="{ '--lock': isLock }"
    :to="{ name: route || service }"
  >
    <VIcon :name="icon" size="14px" />
    {{ name }}
    <!-- Premium -->
    <div v-if="isLock" class="premium-icon" v-tooltip="'Premium'">
      <VIcon name="star" size="11" class="c-secondary" />
    </div>
  </RouterLink>
</template>

<script>
import { PremiumServices } from '@/utils/constant'

export default {
  name: 'MenuItem',

  props: {
    service: String,
    name: String,
    route: String,
    icon: String,
    plan: String
  },

  computed: {
    isLock() {
      return this.plan == 'Free' && PremiumServices.includes(this.service)
    }
  }
}
</script>