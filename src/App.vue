<template>
  <v-app>
    <v-container>
      <CardReservation @click="save" :reservations="reservations" />
      <Timeline :items="reservations"/>
      <TableReservation :reservations="reservations" />
    </v-container>
  </v-app>
</template>

<script>
import TableReservation from "./components/TableReservation/TableReservation.vue";
import CardReservation from "./components/CardReservation/CardReservation.vue";
import Timeline from "./components/Timeline/Timeline.vue";

export default {
  name: "App",
  components: {
    TableReservation,
    CardReservation,
    Timeline
  },
  data() {
    return {
      reservations: [],
    };
  },
  methods: {
    save(info) {
      this.reservations.push(info)
      this.reservations.sort((a,b)=>{
        const dateA = this.parseObject(a)
        const dateB = this.parseObject(b)
        return dateA - dateB
      })
      localStorage.setItem('reservations', JSON.stringify(this.reservations))
    },
    parseObject(object){
      const dateParts = object.date.split("-");
      const timeParts = object.hours.split(" ")[1].split(":");
      const [year, month, day] = dateParts;
      const [hour, minute] = timeParts;
      return new Date(year, month - 1, day, hour, minute);
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
