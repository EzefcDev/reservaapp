<template>
  <v-timeline class="horizontal" align-top dense>
    <v-timeline-item 
      v-for="(item, i) in items"
      :key="i"
     fill-dot
     small
     >
     {{ item.date }} - {{ item.name }}
    </v-timeline-item>
  </v-timeline>
</template>

<script>
    export default{
        props:{
          items:{
            type: Array,
          },
        },
        data(){
            return{
            }
        },
        computed:{
            url(){
                return './timeline.html';
            }
        },
    }
</script>

<style scoped>
.horizontal{
  display: grid;          
  grid-template-columns: repeat(auto-fit, minmax(20px, 1fr));
  gap: 5px;  
}
.v-timeline::before {
  content: none;
}

</style>

