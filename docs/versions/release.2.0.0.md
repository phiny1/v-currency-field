# Usage

## Default Install

```js
import Vue from 'vue'
import currency from 'v-currency-field'

import 'v-currency-field/dist/index.css'

Vue.use(currency)
```

## Install for Tree Shaking 

Webpack don't reconize v-text-field inside v-currency-field component. In this case we need to install v-text-field globally.

### Vue Cli

`plugins/vuetify.js`

```js
import Vue from 'vue';
import Vuetify from 'vuetify/lib';
import { VTextField } from 'vuetify/lib';

import 'vuetify/dist/vuetify.min.css';

Vue.use(
  Vuetify, 
  { components: { VTextField } }
);

const opts = {}
export default new Vuetify(opts);

```

`main.js`

```js
import Vue from 'vue';
import vuetify from './plugins/vuetify';

new Vue({
  vuetify,
  render: h => h(App),
}).$mount('#app');
```

### Nuxt

`plugins/Vuetify.js`

```js
import Vue from 'vue'
import Vuetify, { VTextField } from 'vuetify/lib'

Vue.component('v-text-field', VTextField)

const opts = {}
export default new Vuetify(opts)

```


`plugins/vCurrencyField.js`

```js
import Vue from 'vue'
import currency from 'v-currency-field'

import 'v-currency-field/dist/index.css'

Vue.use(currency)

```

`nuxt.config.js`

```js
/*
** Plugins to load before mounting the App
*/
plugins: [
  '~/plugins/Vuetify.js',
  '~/plugins/vCurrencyField.js'
],

```

## Example

```html
<template>
  <div>
    <v-currency-field label="Value" v-bind="currency_config" :error-messages="errors.price" v-model="price"></v-currency-field>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        errors: {},
        price: 123.45,
        currency_config: {
          decimal: ',',
          thousands: '.',
          prefix: 'R$ ',
          suffix: ' #',
          precision: 2,
          masked: false,
          allowBlank: false
        }
      }
    }
  }
</script>
```

# Properties

All `v-money` properties

| property   | Required | Type    | Default                 | Description                                             |
|------------|----------|---------|-------------------------|---------------------------------------------------------|
| precision  | **true** | Number  | 2                       | How many decimal places                                 |
| decimal    | false    | String  | "."                     | Decimal separator                                       |
| thousands  | false    | String  | ","                     | Thousands separator                                     |
| prefix     | false    | String  | ""                      | Currency symbol followed by a Space, like "R$ "         |
| suffix     | false    | String  | ""                      | Percentage for example: " %"                            |
| masked     | false    | Boolean | false                   | If the component output should include the mask or not  |
| allowBlank | false    | Boolean | false                   | If the field can start blank and be cleared out by user |

And all `v-text-field` properties

| property              | Required | Type             |  Observation             |
|-----------------------|----------|------------------| -------------------------|
| appendOuterIcon       | false    | String           |                          |
| appendOuterIconCb     | false    | Function         | Working but deprecated   |
| @click:append-outer   | false    | Function         | Not Working              |
| autofocus             | false    | Boolean          |                          |
| browserAutocomplete   | false    | String           | Not Tested               |
| clearable             | false    | Boolean          | Not Working Event        |
| clearIcon             | false    | String           |                          |
| clearIconCb           | false    | Number           | Working but deprecated   |
| @click:clear          | false    | Number           | Not Working              |
| color                 | false    | String           |                          |
| flat                  | false    | Boolean          |                          |
| fullWidth             | false    | Boolean          |                          |
| label                 | false    | String           |                          |
| prependInnerIcon      | false    | String           |                          |
| prependInnerIconCb    | false    | Function         | Working but deprecated   |
| @click:prepend-inner  | false    | Function         | Not Working              |
| reverse               | false    | Boolean          |                          |
| singleLine            | false    | Boolean          |                          |
| solo                  | false    | Boolean          |                          |
| soloInverted          | false    | Boolean          |                          |
| error-messages        | false    | []               |                          |
| disabled              | false    | Boolean          |                          |
| readonly              | false    | Boolean          |                          |
| dark                  | false    | Boolean          |                          |
| height                | false    | String           |                          |
| hint                  | false    | String           |                          |
| light                 | false    | Boolean          |                          |
| background-color      | false    | String           |                          |
| hide-details          | false    | Boolean          |                          |
| dense                 | false    | Boolean          |                          |
| filled                | false    | Boolean          |                          |
| id                    | false    | String           |                          |
| loader-height         | false    | Number,String    |                          |
| loading               | false    | Boolean,String   |                          |
| outlined              | false    | Boolean          |                          |
| rounded               | false    | Boolean          |                          |
| shaped                | false    | Boolean          |                          |


## References

- [v-money](https://github.com/64robots/v-money)
- [v-text-field](https://vuetifyjs.com/pt-BR/components/text-fields)