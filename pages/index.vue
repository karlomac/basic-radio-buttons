<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card class="logo py-4 d-flex justify-center">
      </v-card>
      <v-card>
        <v-card-title class="headline d-flex justify-center">
          {{appBrand}} - {{appName}}
        </v-card-title>
        <v-card-text>
          <v-radio-group
            v-model="primaryGroup"
            column
          >
            <v-radio
              v-for="(rad,i) in menus[0]" :key="i"
              :label="rad.value"
              :value="rad.id"
              :disabled="rad.disabled"
              @change="generateGroup(rad.id)"
            ></v-radio>            
          </v-radio-group>
          <v-radio-group
            v-model="menuItems"
            column
          >
            <v-radio
              v-for="(rad,i) in menus[1]" :key="i"
              :label="rad.value"
              :value="rad.id"
              :disabled="rad.disabled"
              @change="generateGroup(rad.id)"
            ></v-radio>
          </v-radio-group>
          <v-radio-group
            v-model="sauces"
            column
          >
            <v-radio
              v-for="(rad,i) in menus[2]" :key="i"
              :label="rad.value"
              :value="rad.id"
              :disabled="rad.disabled"
              @change="generateGroup(rad.id)"
            ></v-radio>
          </v-radio-group>

        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
	export default {
		data() {
			return {
				menus: [
					// first group of radio-buttons
					[
						{ id: '101', value: 'Vegetarian' },
						{ id: '102', value: 'Nut allergy' },
						{ id: '103', value: 'Halal' }
					],
					// second group of radio-buttons
					[
						{ id: '201', value: 'Cashew chicken' },
						{ id: '202', value: 'Sweet and sour pork' },
						{ id: '203', value: 'Stir fried Tofu' },
						{ id: '204', value: 'Vegetable fried rice' },
						{ id: '205', value: 'Pad Thai' },
						{ id: '206', value: 'Massaman beef' },
					],
					// third group of radio-buttons
					[
						{ id: '301', value: 'Peanut sauce' },
						{ id: '302', value: 'Oyster sauce' },
						{ id: '303', value: 'Vegetable spring rolls' },
						{ id: '304', value: 'Steamed rice' },
					],
				],
				rules: {
					// 'Vegetarian' is NOT compatible with 'Cashew chicken', 'Sweet and sour pork', 'Massaman beef', 'Oyster sauce'
					101: [201, 202, 206, 302], 
					// 'Nut allergy' is NOT compatible with 'Cashew chicken', 'Peanut sauce',
					102: [201, 301], 
					// 'Halal' is NOT compatible with 'Sweet and sour pork', 399 allows clearing of all disabled in the menu
					103: [202, 399], 
					// 'Vegetable fried rice' is NOT compatible with 'Steamed rice' (you don't need more rice... carb overload),
					204: [304],
					// 'Pad thai' is NOT compatible with 'Steamed rice' (Pad thai comes with noodles),
					205: [304],
				},
        vegitarian: [],
        primaryGroup: [],
        sauces: [],
        menuItems: [],
		appBrand: '',
		appName: '',
			}
		},

	mounted() {
		this.appBrand = process.env.APP_BRAND
		this.appName = process.env.APP_NAME
	},

    methods: {
		generateGroup(id){
			// generate the next group list
			// find the corresponding rules
			const menus = this.menus

			let arrRules = []
			Object.entries(this.rules).forEach(([key, value]) => {
				if(key == id) {
				arrRules = value
				return true
				}
			});
			let x = 0
			let y = 0

			arrRules.forEach(e => {
				if(e > 200) x = 1
				if(e > 300) x = 2
				
				if(x != y){
					menus[x].map(e => e.disabled = false)
					y = x
				}

				menus[x].map((ele, i) => {
					if(ele.id == e){
					menus[x][i].disabled = true
					return true
					}
				})
			})
		}
    }
  }
</script>