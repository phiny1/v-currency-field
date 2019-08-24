# Currency Mask for Vuetify v2

This is a fork from https://github.com/phiny1/v-currency-field adapted for Vuetify v2.

## Features

- All features from v-money as v-text-field of vuetify v2.

## Usage

### Globally

```js
import Vue from 'vue'
import currency from 'v-currency-field'

import 'v-currency-field/dist/index.css'

Vue.use(currency)
```

### Example

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
          allowBlank: false,
          min: Number.MIN_SAFE_INTEGER,
          max: Number.MAX_SAFE_INTEGER
        }
      }
    }
  }
</script>
```

## Properties

All v-money properties

| property   | Required | Type    | Default                 | Description                                             |
|------------|----------|---------|-------------------------|---------------------------------------------------------|
| precision  | **true** | Number  | 2                       | How many decimal places                                 |
| decimal    | false    | String  | "."                     | Decimal separator                                       |
| thousands  | false    | String  | ","                     | Thousands separator                                     |
| prefix     | false    | String  | ""                      | Currency symbol followed by a Space, like "R$ "         |
| suffix     | false    | String  | ""                      | Percentage for example: " %"                            |
| masked     | false    | Boolean | false                   | If the component output should include the mask or not  |
| allowBlank | false    | Boolean | false                   | If the field can start blank and be cleared out by user |
| min        | false    | Number  | Number.MIN_SAFE_INTEGER | The min value allowed                                   |
| max        | false    | Number  | Number.MAX_SAFE_INTEGER | The max value allowed                                   |

And all vuetify properties

| property              | Required | Type      |  Observation             |
|-----------------------|----------|-----------| -------------------------|
| appendOuterIcon       | false    | String    |                          |
| appendOuterIconCb     | false    | Function  | Working but deprecated   |
| @click:append-outer   | false    | Function  | Not Working              |
| autofocus             | false    | Boolean   |                          |
| browserAutocomplete   | false    | String    | Not Tested               |
| clearable             | false    | Boolean   | Not Working Event        |
| clearIcon             | false    | String    |                          |
| clearIconCb           | false    | Number    | Working but deprecated   |
| @click:clear          | false    | Number    | Not Working              |
| color                 | false    | String    |                          |
| filled                | false    | Boolean   |                          |
| flat                  | false    | Boolean   |                          |
| fullWidth             | false    | Boolean   |                          |
| label                 | false    | String    |                          |
| outlined              | false    | Boolean   |                          |
| prependInnerIcon      | false    | String    |                          |
| prependInnerIconCb    | false    | Function  | Working but deprecated   |
| @click:prepend-inner  | false    | Function  | Not Working              |
| reverse               | false    | Boolean   |                          |
| singleLine            | false    | Boolean   |                          |
| solo                  | false    | Boolean   |                          |
| soloInverted          | false    | Boolean   |                          |
| error-messages        | false    | []        |                          |
| disabled              | false    | Boolean   |                          |
| readonly              | false    | Boolean   |                          |
| dark                  | false    | Boolean   |                          |
| height                | false    | String    |                          |
| hint                  | false    | String    |                          |
| light                 | false    | Boolean   |                          |
| background-color      | false    | String    |                          |


### References

- https://github.com/phiny1/v-currency-field
- https://github.com/64robots/v-money
- https://vuetifyjs.com/pt-BR/components/text-fields
