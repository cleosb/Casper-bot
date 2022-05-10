<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      persistent
      max-width="290"
    >
      <v-card>
        <v-card-title class="text-h5">
          PIN do Administrador
        </v-card-title>
        <v-card-actions>
            <v-form
            v-model="valid"
            >
                <v-container>
                    <v-row>
                        <v-col>
                            <v-text-field
                                v-model="PIN"
                                type= "password"
                                :rules="PinRules"
                                :counter="6"
                                label="PIN"
                                required
                            ></v-text-field>
                        </v-col>
                    </v-row>
                </v-container>
            </v-form>
          <v-row>
              <v-btn
                color="green darken-1"
                text
                @click="validate"
            >
                Confirmar
            </v-btn>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
export default {
    name: 'AdminDialogComponent',
    data () {
      return {
        dialog: true,
        valid: false,
        PIN: '',
        PINAssertion: 654321,
        PinRules: [
        v => !!v || 'PIN required',
        v => v.length <= 6 || 'PIN Contains 6 digits.',
      ],
      }
    },
    methods: {
      validate () {
          if(this.PINAssertion == this.PIN){
            this.dialog = false
          }
      }
    }
}
</script>

<style>

</style>