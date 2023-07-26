<template>
  <v-container>
    <v-card elevation="1" outlined>
      <h1 class="ma-4">Reservar</h1>
      <v-row class="ma-2">
        <v-col>
          <label>Nombre</label>
          <v-text-field 
          v-model.trim="$v.name.$model"
          @blur="$v.name.$touch()"
          :error-messages="($v.name.$dirty && $v.name.$invalid) ? ((!$v.name.$required)? 'El campo es obligatorio':(!$v.name.$minLength) ? 'El nombre debe contener mas de tres caracteres':'') :''"
           solo></v-text-field>
          <!-- <div v-if="!$v.name.required">El nombre es requerido</div> -->
        </v-col>
        <v-col>
          <label>Fecha</label>
          <v-text-field v-model="$v.date.$model" type="date" solo append-icon="mdi-calendar" required ></v-text-field>
        </v-col>
        <v-col>
          <label>Desde</label>
          <v-text-field v-model="$v.from.$model" type="time" solo append-icon="mdi-clock-time-four-outline" 
          :error-messages="($v.from.$dirty && $v.from.$invalid) ? ((!$v.from.$required)? 'El campo es obligatorio':''):''" >
          </v-text-field>
        </v-col>
        <v-col>
          <label>Hasta</label>
          <v-text-field v-model="$v.to.$model" type="time" solo append-icon="mdi-clock-time-four-outline" required ></v-text-field>
          
        </v-col>
        <v-col class="mt-6">
          <v-btn color="teal lighten-1" class="pa-6" @click="save"
            >Guardar</v-btn>
        </v-col>
      </v-row>
    </v-card>
  </v-container>
</template>

<script>
import {required, minLength} from 'vuelidate/lib/validators'

export default {
    props: {
        reservations: {
            type: Array,
        },
    },
  data() {
    return {
      name: "",
      date: "",
      from: "",
      to: "",
    };
  },
  validations: {
    name: {
      required,
      minLength: minLength(3)
    },
    date: {
      required
    },
    from: {
      required
    },
    to: {
      required
    }
  },
  methods: {
    save() {
      if (this.$v.name.required && this.$v.date.required && this.$v.from.required && this.$v.to.required) {
        const reservation = {
          name: this.name,
          date: this.date,
          hours: `de ${this.from} hasta ${this.to}`,
        }
        const exist = this.reservations.find(({date,hours})=> date === reservation.date && hours === reservation.hours)
        if(!exist){
            this.$emit("click", reservation);
            this.clearInput()
        } else {
            console.log("Ya existe esta reserva")
        }
      }else{
        console.log("Los campos deben estar completos")
      }
    },
    clearInput() {
        this.name= "";
        this.from= "";
        this.to= "";
        this.getDate();
    },
    getDate() {
      const date = new Date();
      let day = date.getDate();
      let month = date.getMonth() + 1;
      const year = date.getFullYear();

      if (day < 10) {
        day = "0" + day;
      }

      if (month < 10) {
        month = "0" + month;
      }

      this.date = `${year}-${month}-${day}`;
    },
  },
  mounted() {
    this.getDate();
  },
};
</script>

<style lang="scss" scope>
    .v-btn.v-size--default {
        font-size: 1rem;
        color: white;
    }
    input[type="date"]::-webkit-calendar-picker-indicator, input[type="time"]::-webkit-calendar-picker-indicator{
        cursor: pointer;
        border-radius: 4px;
        margin: 1px;
        opacity: 0.9;
        filter: invert(1);
    }
</style>