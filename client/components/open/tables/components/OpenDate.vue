<template>
  <span v-if="valueIsObject">
    <template v-if="value[0]">{{ formattedDate(value[0]) }}</template>
    <template v-if="value[1]"><b class="mx-2">to</b>{{ formattedDate(value[1]) }}</template>
  </span>
  <span v-else>
    {{ formattedDate(value) }}
  </span>
</template>

<script>
import { format } from 'date-fns'
import { default as _has } from 'lodash/has'

export default {
  components: {},
  props: {
    // eslint-disable-next-line vue/require-prop-types
    property: {
      required: true
    },
    // eslint-disable-next-line vue/require-prop-types
    value: {
      required: true
    }
  },
  data () {
    return {}
  },
  computed: {
    valueIsObject () {
      if (typeof this.value === 'object' && this.value !== null) {
        return true
      }
      return false
    }
  },
  mounted () {
  },
  methods: {
    formattedDate(val) {
      if (!val) return ''
      const dateFormat = _has(this.property, 'date_format') ? this.property.date_format : 'dd/MM/yyyy'
      const timeFormat = _has(this.property, 'time_format') ? this.property.time_format : '24'
      if (this.property?.with_time) {
        try {
          return format(new Date(val), dateFormat + (timeFormat == 12 ? ' p':' HH:mm'))
        } catch (e) {
          return ''
        }
      }
      try {
        return format(new Date(val), dateFormat)
      } catch (e) {
        return ''
      }
    }
  }
}
</script>
