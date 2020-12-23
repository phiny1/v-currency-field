<template>
  <div id="appRoot">
    <v-app id="inspire" class="app">
      <main>
        <v-main>
          <v-container fluid>
            <v-form
            ref="form"
            lazy-validation
            >
              <v-row dense>
                <v-currency-field 
                 outlined 
                 dense 
                 v-model="value" 
                 locale="pt-BR" 
                 :auto-decimal-mode="true" 
                 :decimal-length="2" 
                 :default-value="0" 
                 :value-as-integer="false"
                 :allow-negative="true"
                 label="Income" 
                 clearable 
                 :rules="[(v) => (v > 0 || !submited) || 'Wrong value']">
                    
                  <template v-slot:append>
                    R$
                  </template>
                </v-currency-field>
              </v-row>

              <v-row dense>
                {{ value }}
              </v-row>

              <v-row dense>
                <v-btn 
                class="mr-4"
                small 
                @click.prevent="submit()"
                color="primary">Submit</v-btn> 

                <v-btn 
                small
                class="mr-4 white--text"
                @click.prevent="resetValidation()"
                color="red darken-3">Reset Validation</v-btn> 

                <v-btn 
                small
                class="mr-4 white--text"
                @click.prevent="reset()"
                color="green darken-3">Reset</v-btn> 

                <v-btn 
                small
                class="mr-4 white--text"
                @click.prevent="changeValue()"
                color="purple darken-3">Change Value</v-btn> 
              </v-row>
            </v-form>

            <v-data-table
              :headers="headers"
              :items="desserts"
            >
              <template v-slot:item.name="props">
                <v-edit-dialog
                  :return-value.sync="props.item.name"
                  @save="save"
                  @cancel="cancel"
                  @open="open"
                  @close="close"
                > {{ props.item.name }}
                  <template v-slot:input>
                    <v-text-field
                      v-model="props.item.name"
                      :rules="[max25chars]"
                      label="Edit"
                      single-line
                      counter
                    ></v-text-field>
                  </template>
                </v-edit-dialog>
              </template>
              <template v-slot:item.carbs="props">
                <v-edit-dialog
                  :return-value.sync="props.item.carbs"
                  large
                  persistent
                  @save="save"
                  @cancel="cancel"
                  @open="open"
                  @close="close"
                >
                  <div>{{ props.item.carbs }}</div>
                  <template v-slot:input>
                    <div class="mt-4 title">Update Carbs</div>
                  </template>
                  <template v-slot:input>
                    <v-currency-field
                      v-model="props.item.carbs"
                      :rules="[max25chars]"
                      label="Edit"
                      single-line
                      counter
                      autofocus
                    ></v-currency-field>
                  </template>
                </v-edit-dialog>
              </template>
            </v-data-table>

            <v-snackbar v-model="snack" :timeout="3000" :color="snackColor">
              {{ snackText }}

              <template v-slot:action="{ attrs }">
                <v-btn v-bind="attrs" text @click="snack = false">Close</v-btn>
              </template>
            </v-snackbar>
          </v-container>
        </v-main>
      </main>
    </v-app>
  </div>
</template>

<script>
  export default {
    name: 'demo',
    data() {
      return {
        value: 9.90,
        submited: false,
        snack: false,
        snackColor: '',
        snackText: '',
        max25chars: v => v.length <= 25 || 'Input too long!',
        pagination: {},
        headers: [
          {
            text: 'Dessert (100g serving)',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          { text: 'Calories', value: 'calories' },
          { text: 'Fat (g)', value: 'fat' },
          { text: 'Carbs (g)', value: 'carbs' },
          { text: 'Protein (g)', value: 'protein' },
          { text: 'Iron (%)', value: 'iron' },
        ],
        desserts: [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%',
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%',
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%',
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: '8%',
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: '16%',
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: '0%',
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: '2%',
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: '45%',
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: '22%',
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: '6%',
          },
        ],
      }
    },
    methods: {
      changeValue () {
       this.value = 117;
     },
     submit () {
      this.submited = true 

      if (this.$refs.form.validate()) {
          //SENT DATA TO API
        }
      },
      reset () {
        this.submited = false
        
        this.$refs.form.reset()
      },
      resetValidation () {
        this.submited = false 
        
        this.$refs.form.resetValidation()
      },
      save () {
        this.snack = true
        this.snackColor = 'success'
        this.snackText = 'Data saved'
      },
      cancel () {
        this.snack = true
        this.snackColor = 'error'
        this.snackText = 'Canceled'
      },
      open () {
        this.snack = true
        this.snackColor = 'info'
        this.snackText = 'Dialog opened'
      },
      close () {
        console.log('Dialog closed')
      },
      test () {
        console.log("teste")
        alert("msg function");
      }
    },
  }
</script>
