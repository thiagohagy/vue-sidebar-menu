<template>
  <div
    class="vsm-item"
    :class="[{'first-item' : firstItem}, {'open-item' : show}, {'active-item' : active}, {'parent-active-item' : childActive}]"
    @mouseenter="mouseEnter($event)"
  >
    <template v-if="isRouterLink">
      <router-link
        class="vsm-link"
        :class="item.class"
        :to="item.href"
        :disabled="item.disabled"
        :event="item.disabled ? '' : 'click'"
        v-bind="item.attributes"
        @click.native="clickEvent"
      >
        <ItemIcon
          v-if="item.icon"
          :icon="item.icon"
        />
        <template v-if="!isCollapsed">
          <ItemBadge
            v-if="item.badge"
            :badge="item.badge"
            :style="[rtl ? (item.child ? {'margin-left' : '30px'} : '') : (item.child ? {'margin-right' : '30px'} : '')]"
          />
          <span class="vsm-title">{{ item.title }}</span>
          <i
            v-if="item.child"
            class="vsm-arrow"
            :class="{'open-arrow' : show}"
          />
        </template>
      </router-link>
    </template>
    <template v-else>
      <a
        class="vsm-link"
        :class="item.class"
        :href="item.href ? item.href : '#'"
        :disabled="item.disabled"
        v-bind="item.attributes"
        @click="clickEvent"
      >
        <ItemIcon
          v-if="item.icon"
          :icon="item.icon"
        />
        <template v-if="!isCollapsed">
          <ItemBadge
            v-if="item.badge"
            :badge="item.badge"
            :style="[rtl ? (item.child ? {'margin-left' : '30px'} : '') : (item.child ? {'margin-right' : '30px'} : '')]"
          />
          <span class="vsm-title">{{ item.title }}</span>
          <i
            v-if="item.child"
            class="vsm-arrow"
            :class="{'open-arrow' : show}"
          />
        </template>
      </a>
    </template>
    <template v-if="item.child">
      <template v-if="!isCollapsed">
        <transition
          name="expand"
          @enter="expandEnter"
          @afterEnter="expandAfterEnter"
          @beforeLeave="expandBeforeLeave"
        >
          <div
            v-if="show"
            class="vsm-dropdown"
          >
            <div class="vsm-list">
              <sub-item
                v-for="(subItem, index) in item.child"
                :key="index"
                :item="subItem"
              />
            </div>
          </div>
        </transition>
      </template>
    </template>
  </div>
</template>

<script>
import SubItem from './SubItem.vue'
import ItemIcon from './ItemIcon.vue'
import ItemBadge from './ItemBadge.vue'
import { itemMixin, animationMixin } from '../mixin'

export default {
  components: {
    SubItem,
    ItemIcon,
    ItemBadge
  },
  mixins: [itemMixin, animationMixin],
  props: {
    item: {
      type: Object,
      required: true
    },
    firstItem: {
      type: Boolean,
      default: false
    },
    isCollapsed: {
      type: Boolean
    }
  },
  methods: {
    mouseEnter (event) {
      if (this.isCollapsed && this.firstItem) {
        this.$parent.$emit('mouseEnterItem', {
          item: this.item,
          pos:
              event.currentTarget.getBoundingClientRect().top -
              this.$parent.$el.getBoundingClientRect().top,
          height: this.$el.offsetHeight
        })
      }
    }
  }
}
</script>
