<template>
  <component
    :is="iconType"
    :type="iconName"
    :color="iconColor"
    :size="iconSize"
    :item="item"
  />
</template>

<script>
import Icons from '_c/icons'
export default {
  name: 'CommonIcon',
  components: { Icons },
  props: {
    // type: {
    //   type: String,
    //   required: true
    // },
    item: {
      type: Object
    },
    color: String,
    size: Number
  },
  computed: {
    type() {
      if (this.item && this.item.meta && this.item.meta.icon) {
        return this.item.meta.icon
      }
      return 'md-albums'
    },
    iconType() {
      return this.type.indexOf('_') === 0 ? 'Icons' : 'Icon'
    },
    iconName() {
      return this.iconType === 'Icons'
        ? this.getCustomIconName(this.type)
        : this.type
    },
    iconSize() {
      return this.size || (this.iconType === 'Icons' ? 12 : undefined)
    },
    iconColor() {
      return this.color || ''
    }
  },
  methods: {
    getCustomIconName(iconName) {
      return iconName.slice(1)
    }
  }
}
</script>

<style></style>
