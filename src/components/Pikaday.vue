<template>

    <input
      type="text"
      autocomplete="new-password"
      @input="updateValue"
      ref="node">

</template>

<script>
import Pikaday from 'pikaday'

export default {
  props: {
    label: '',
    minDate: {},
    maxDate: {},
    defaultDate: {},
    setDefaultDate: {
      default: false,
      type: Boolean
    },
    firstDay: {
      default: 1,
      type: Number
    },
    numberOfMonths: {
      default: 1,
      type: Number
    },
    format: {
      default: 'MMMM Do YYYY',
      type: String
    },
    showMonthAfterYear: {
      default: false,
      type: Boolean
    },
    disableWeekends: {
      default: false,
      type: Boolean
    },
    options: {
      default: () => { return {} },
      type: Object
    }
  },
  data () {
    return {
      htmlNode: {},
      currentDate: {}
    }
  },

  methods: {
    updateValue (val) {
      this.$emit('input', {value: val, ref: this.$refs.node})
    }
  },
  watch: {
    minDate (newMinDate) {
      this.picker.setMinDate(newMinDate)
      if (this.currentDate < newMinDate) {
        this.picker.setDate('1970-01-01', true)
      }
    },
    maxDate (newMaxDate) {
      this.picker.setMaxDate(newMaxDate)
      if (this.currentDate > newMaxDate) {
        this.picker.setDate(newMaxDate)
      }
    }
  },
  mounted () {
    this.htmlNode = this.$refs.node
    let vm = this
    const options = Object.assign({}, {
      field: this.htmlNode,
      format: this.format,
      minDate: this.minDate,
      maxDate: this.maxDate,
      firstDay: this.firstDay,
      defaultDate: this.defaultDate,
      numberOfMonths: this.numberOfMonths,
      disableWeekends: this.disableWeekends,
      setDefaultDate: this.setDefaultDate,
      showMonthAfterYear: this.showMonthAfterYear,
      onSelect (date) {
        vm.currentDate = date
        vm.$emit('onSelect', {value: vm.currentDate, ref: vm.$refs.node})
      }
    }, this.options)
    this.picker = new Pikaday(options)
    this.currentDate = this.defaultDate
    this.picker.setDate(this.currentDate)
  }
}
</script>

<style src="pikaday/css/pikaday.css"></style>
