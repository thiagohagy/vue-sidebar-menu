<template>
  <i
    v-if="typeof icon === 'string' || (icon instanceof String)"
    class="vsm-icon"
    :class="icon"
  />
  <component
    :is="icon.element ? icon.element : 'i'"
    v-else
    class="vsm-icon"
    :class="icon.class"
    v-bind="icon.attributes"
  />
</template>

<script>
export default {
  props: {
    icon: {
      type: [Object, String],
      required: true
    }
  }
}
</script>
