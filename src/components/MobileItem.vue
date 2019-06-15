<template>
  <div
    v-if="item"
    class="vsm-item mobile-item"
    :class="[{'open-item' : item.child}, {'active-item' : active}, {'parent-active-item' : childActive}]"
  >
    <template v-if="isRouterLink">
      <router-link
        class="vsm-link"
        :to="item.href"
        :disabled="item.disabled"
        :event="item.disabled ? '' : 'click'"
        @click.native="clickEvent($event, true)"
      >
        <ItemIcon
          v-if="item.icon"
          :icon="item.icon"
        />
        <ItemBadge
          v-if="item.badge"
          :badge="item.badge"
          :style="[rtl ? (item.child ? {'margin-left' : '30px'} : '') : (item.child ? {'margin-right' : '30px'} : '')]"
        />
        {{ item.title }}
        <i
          v-if="item.child"
          class="vsm-arrow open-arrow"
        />
      </router-link>
    </template>
    <template v-else>
      <a
        class="vsm-link"
        :href="item.href ? item.href : '#'"
        :disabled="item.disabled"
        @click="clickEvent($event, true)"
      >
        <ItemIcon
          v-if="item.icon"
          :icon="item.icon"
        />
        <ItemBadge
          v-if="item.badge"
          :badge="item.badge"
          :style="[rtl ? (item.child ? {'margin-left' : '30px'} : '') : (item.child ? {'margin-right' : '30px'} : '')]"
        />
        {{ item.title }}
        <i
          v-if="item.child"
          class="vsm-arrow open-arrow"
        />
      </a>
    </template>
  </div>
</template>

<script>
import ItemIcon from './ItemIcon.vue'
import ItemBadge from './ItemBadge.vue'
import { itemMixin } from '../mixin'

export default {
  components: {
    ItemIcon,
    ItemBadge
  },
  mixins: [itemMixin],
  props: {
    item: {
      type: Object,
      default: null
    }
  },
  watch: {
    item () {
      this.active =
        this.item && this.item.href ? this.isLinkActive(this.item) : false
      this.childActive =
        this.item && this.item.child
          ? this.isChildActive(this.item.child)
          : false
    }
  }
}
</script>
