<template>
  <v-app>
    <v-container>
      <CardReservation @click="save" :reservations="reservations" />

      <TableReservation :reservations="reservations" />
    </v-container>
  </v-app>
</template>

<script>
import TableReservation from "./components/TableReservation/TableReservation.vue";
import CardReservation from "./components/CardReservation/CardReservation.vue";

export default {
  name: "App",
  components: {
    TableReservation,
    CardReservation,
  },
  data() {
    return {
      reservations: [],
    };
  },
  methods: {
    save(info) {
      this.reservations.push(info)
      localStorage.setItem('reservations', JSON.stringify(this.reservations))
    },
    getReservations(){
        this.reservations = JSON.parse(localStorage.getItem("reservations")) ?? []
    }
  },
  mounted(){
   this.getReservations()
  }
}
</script>
