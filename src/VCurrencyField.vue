<template>
  <v-text-field
   ref="textfield"
   v-model="formattedValue"
   v-bind="attrs"
   v-on="listeners()"
   type="text"
   v-currency-directive="{currency, locale, distractionFree, precision: decimalLength, autoDecimalMode: decimalMode, valueRange, allowNegative, valueAsInteger}">

    <template v-for="(index, name) in $slots" v-slot:[name]>
      <slot :name="name" />
    </template>
  </v-text-field>
</template>

<script>
import { setValue } from 'vue-currency-input/src/api'
import { CurrencyDirective } from 'vue-currency-input';
import defaults from './options';

export default {
  name: 'VCurrencyField',
  props: {
    value: {
      type: [Number, String],
      default: () => 0,
    },
    locale: {
      type: String,
      default: () => defaults.locale,
    },
    currency: {
      type: [String, Object],
      default: () => defaults.currency,
    },
    decimalLength: {
      type: [Number, Object],
      default: () => defaults.decimalLength,
    },
    autoDecimalMode: {
      type: Boolean,
      default: () => defaults.autoDecimalMode,
    },
    min: {
      type: Number,
      default: () => defaults.min,
    },
    max: {
      type: Number,
      default: () => defaults.max,
    },
    defaultValue: {
      type: Number,
      default: () => defaults.defaultValue,
    },
    valueAsInteger: {
      type: Boolean,
      default: () => defaults.valueAsInteger,
    },
    allowNegative: {
      type: Boolean,
      default: () => defaults.allowNegative,
    }
  },
  directives: { CurrencyDirective },
  data() {
    return {
      formattedValue: this.value,
    };
  },
  mounted() {
    this.addListeners(this.$el.querySelector('input'));
    this.setValue(this.value);
  },
  computed: {
    attrs() {
      // eslint-disable-next-line
      const { value, ...attrs } = this.$attrs; // all but input event
      return attrs;
    },
    distractionFree() {
      if (this.decimalLength > 0) {
        return !this.autoDecimalMode;
      } else {
        return false;
      }
    },
    decimalMode() {
      if (this.decimalLength > 0) {
        return this.autoDecimalMode;
      } else {
        return false;
      }
    },
    valueRange() {
      if (this.min || this.max) {
        return {min: this.min, max: this.max};
      } else {
        return undefined;
      }
    }
  },
  watch: {
    value: 'setValue'
  },
  methods: {
    addListeners(el) {
      el.addEventListener('change', e => {
        if (e.detail) {
          this.$emit('change', e.detail.numberValue)
        }

        if (this.value == null && this.value == undefined && this.defaultValue !== null && this.defaultValue !== undefined) {
          this.setValue(this.valueAsInteger && this.defaultValue ? this.defaultValue * Math.pow(10, this.decimalLength) : this.defaultValue)
        }
      }, { capture: true })

      el.addEventListener('input', e => {
        if (e.detail && this.value !== e.detail.numberValue) {
          this.$emit('input', e.detail.numberValue)
        }
      }, { capture: true })
    },
    setValue (value) {
      let input = this.$el.querySelector('input')
      setValue(input, value)
    },
    listeners() {
      // eslint-disable-next-line
      const { input, keyup, ...listeners } = this.$listeners; // all but input event
      return {
        ...listeners,
        input: (value) => {
          if (this.$refs.textfield.isResetting || value == null) {
            this.setValue(this.valueAsInteger && this.defaultValue ? this.defaultValue * Math.pow(10, this.decimalLength) : this.defaultValue)
          }
        },
        'keyup': (event) => {
          if (event.key === '-' || event.key === '+') {
            if (this.value != null && event.key === '-' && this.value >= 0) {
              this.setValue(this.value * -1)
            }
            
            if (this.value != null && event.key === '+' && this.value <= 0) {
              this.setValue(this.value * -1)
            }
          }
          
          if (keyup) {
            keyup();
          }
        }
      };
    }
  }
};
</script>
