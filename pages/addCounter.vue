<template>
  <v-layout align-center justify-center row fill-height>
    <v-flex xs6>
      <form>
        <v-text-field
          v-model="m.name"
          :error-messages="nameErrors"
          :counter="10"
          label="Nazwa"
          prepend-icon="text_fields"
          required
          @input="$v.m.name.$touch()"
          @blur="$v.m.name.$touch()"
        ></v-text-field>

        <v-menu
          ref="menu"
          v-model="menu"
          :close-on-content-click="false"
          :nudge-right="40"
          lazy
          transition="scale-transition"
          offset-y
          full-width
          min-width="290px"
        >
          <template v-slot:activator="{ on }">
            <v-text-field
              v-model="m.date"
              label="Data urodzin"
              prepend-icon="event"
              readonly
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker
            ref="picker"
            v-model="m.date"
            :max="new Date().toISOString().substr(0, 10)"
            min="1930-01-01"
            @change="save"
          ></v-date-picker>
        </v-menu>

        <v-checkbox
          v-model="checkbox"
          :error-messages="checkboxErrors"
          label="Zapisac w przegladarce?"
          required
          @change="$v.checkbox.$touch()"
          @blur="$v.checkbox.$touch()"
        ></v-checkbox>

        <v-btn @click="submit">submit</v-btn>
        <v-btn @click="clear">clear</v-btn>
      </form>
    </v-flex>
    <!-- <v-flex xs12>
      <v-alert :value="alert" type="success" transition="scale-transition">This is a success alert.</v-alert>
    </v-flex>-->
  </v-layout>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, maxLength, email } from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],

  validations: {
    m: {
      name: { required, maxLength: maxLength(10) },
      date: { required }
    },
    checkbox: {
      checked(val) {
        return val
      }
    }
  },

  data: () => ({
    name: '',
    m: { name: '', date: '' },
    checkbox: false,
    date: null,
    menu: false,
    alert: false
  }),

  watch: {
    menu(val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = 'YEAR'))
    }
  },

  computed: {
    checkboxErrors() {
      const errors = []
      if (!this.$v.checkbox.$dirty) return errors
      !this.$v.checkbox.checked && errors.push('Obowiązkowe')
      return errors
    },
    nameErrors() {
      const errors = []
      if (!this.$v.m.name.$dirty) return errors
      !this.$v.m.name.maxLength &&
        errors.push('Tytuł musi mieć minimum 10 znaków')
      !this.$v.m.name.required && errors.push('Musisz wpisać jakis tytuł')
      return errors
    }
  },

  methods: {
    submit() {
      const self = this

      this.$v.$touch()
      let list = []
      this.m.uid = this.uuidv4()
      if (localStorage.listOf) list = JSON.parse(localStorage.listOf)
      list.push(this.m)
      localStorage.listOf = JSON.stringify(list)
      this.alert = true
      this.clear()

      setTimeout(() => {
        self.alert = false
      }, 3000)
    },
    clear() {
      this.$v.$reset()
      ;(this.m.name = ''), (this.checkbox = false)
    },
    save(date) {
      this.$refs.menu.save(date)
    },
    uuidv4() {
      return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(
        c
      ) {
        var r = (Math.random() * 16) | 0,
          v = c == 'x' ? r : (r & 0x3) | 0x8
        return v.toString(16)
      })
    }
  }
}
</script>

<style>
</style>
