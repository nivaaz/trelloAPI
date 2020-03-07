<template>
    <div>
        <h1> Boards </h1>
        <table>
        <tr >
          <td > <h2> Name</h2> </td>
          <td ><h2> # Cards </h2> </td>
         

        </tr>
      <!-- ...thead... -->
      <tbody>
        <tr v-for="board in boards" :key="board.id">
          <td>This board is named {{ board.name }}.</td>
          <td> This board has {{ board.cardsNumber }} cards. </td>
           <td >
              <button> ✏ </button>
              <button @click="$emit('delete:board', board.id)"> 🗑️ </button>
          </td>
        </tr>
      </tbody>
    </table>

    </div>
</template>

<script>
export default {
    name: 'InputArray',
    props:{
        boards: Array,
    }
}
</script>

<style scoped>
h1{
    color: crimson;
}
input{
    padding: 1em;
}

td{
    color: grey;
    padding: 0.5em;
}
table{
    margin: 0 auto;
}
h2{
    font-weight: bold;
    color: crimson;
}
</style>