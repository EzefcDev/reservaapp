<template>
  <v-container>
    <v-data-table
      :headers="headers"
      :items="reservations" 
      hide-default-footer
      class="elevation-1"
    >
      <template v-slot:item.actions="{ item }">
        <v-icon small @click="deleteItem(item)" class="ml-6"> mdi-delete </v-icon>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
export default {
  props:{
    reservations:{
      type: Array,
    },
  },
  data(){
    return{
      pagination: true,
      headers: [
        {
          text: "Nombre",
          align: "start",
          sortable: false,
          value: "name",
        },
        { text: "Horario", value: "hours" },
        { text: "Acción", value: "actions", sortable: false },
      ],
    }
  },
  methods:{
    deleteItem(item){
      this.reservations.splice(this.reservations.indexOf(item), 1)
      localStorage.setItem('reservations', JSON.stringify(this.reservations))
    }
  }
};
</script>

<style lang="scss" scope>

 .v-data-table-header {
    background-color: #DCDCDC;
  }
  .v-data-table > .v-data-table__wrapper > table > tbody > tr > th, .v-data-table > .v-data-table__wrapper > table > thead > tr > th, .v-data-table > .v-data-table__wrapper > table > tfoot > tr > th {
      font-size: 1rem;
  }
  
</style>