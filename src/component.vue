<template lang="html">
  <v-text-field
         ref="textfield"
         :class="`${classes}`"
         :value="formattedValue"
         v-bind="$props"
         type="tel"
         @blur="newblur"
         @input="change"
         v-currency="{precision, decimal, thousands, allowBlank}"/>
</template>

<style>
.padding-prefix input {
  padding-left: 8px !important;
}
</style>

<script>
import currency from './directive'
import defaults from './options'
import {format, unformat} from './utils'

export default {
  name: 'Money',
  props: {
    value: {
      required: true,
      type: [Number, String],
      default: 0
    },
    masked: {
      type: Boolean,
      default: false
    },
    precision: {
      type: Number,
      default: () => defaults.precision
    },
    decimal: {
      type: String,
      default: () => defaults.decimal
    },
    thousands: {
      type: String,
      default: () => defaults.thousands
    },
    prefix: {
      type: String,
      default: () => defaults.prefix
    },
    suffix: {
      type: String,
      default: () => defaults.suffix
    },    
    allowBlank: {
      type: Boolean,
      default: () => defaults.allowBlank
    },
    appendOuterIcon: String,
    appendOuterIconCb: Function,
    autofocus: Boolean,
    browserAutocomplete: String,
    clearable: Boolean,
    clearIcon: {
      type: String,
      default: '$vuetify.icons.clear'
    },
    clearIconCb: Function,
    color: {
      type: String,
      default: 'primary'
    },
    counter: [Boolean, Number, String],
    flat: Boolean,
    fullWidth: Boolean,
    label: String,
    placeholder: String,
    prependInnerIcon: String,
    prependInnerIconCb: Function,
    reverse: Boolean,
    singleLine: Boolean,
    solo: Boolean,
    soloInverted: Boolean,
    dark: Boolean,
    disabled: Boolean,
    readonly: Boolean,
    light: Boolean,
    height: String,
    backgroundColor: String,
    hint: String,
    persistentHint: String,
    error: Boolean,
    errorCount: {
      type: [Number, String],
      default: 1
    },
    errorMessages: {
      type: [String, Array],
      default: () => []
    },
    messages: {
      type: [String, Array],
      default: () => []
    },
    rules: {
      type: Array,
      default: () => []
    },
    success: Boolean,
    successMessages: {
      type: [String, Array],
      default: () => []
    },
    validateOnBlur: Boolean,
    tabindex: String,
    hideDetails: Boolean,
    dense: Boolean,
    filled: Boolean,
    id: String,
    loaderHeight: {
      type: [Number, String]
    },
    loading: {
      type: [Boolean, String]
    },
    outlined: Boolean,
    rounded: Boolean,
    shaped: Boolean
  },

  directives: {currency},

  data () {
    return {
      formattedValue: ''
    }
  },
  computed: {
    classes () {
      const classes = {
        'padding-prefix': this.prefix,
      }

      let classString = "";
      for (let key in classes) {
        if (classes[key]) {
          classString += `${key} `
        }
      }
      return classString;
    }
  },
  watch: {
    value: {
      immediate: true,
      handler (newValue, oldValue) {
        var formatted = format(newValue, this.$props)
        if (formatted !== this.formattedValue) {
          this.formattedValue = formatted
          if (this.$refs.textfield) {
            this.$refs.textfield.$refs.input.value = this.formattedValue;
          }
        }
      }
    }
  },

  methods: {
    change (newVal) {
      this.$emit('input', this.masked ? newVal : unformat(newVal, this.precision))
    },
    newblur (newVal) {
      this.$emit('blur', newVal)
    }
  }
}
</script>
