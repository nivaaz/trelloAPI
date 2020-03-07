<template>
    <div>

        <h1> Here is the component </h1>
        <table>
        <tr >
          <td>Name </td>
          <td> Email </td>
        </tr>
      <!-- ...thead... -->
      <tbody>
        <tr v-for="board in boards" :key="board.id">
          <td>This board is named {{ board.name }}.</td>
          <td> This board has {{ board.cardsNumber }} cards. </td>
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
    color: pink;
}
input{
    padding: 1em;
}
button{
    color: crimson;
    text-transform: uppercase;
    background-color: white;
    border: 2px solid crimson;
    border-radius: 1em;
    padding: 1em;
}
tr{
    border: 2px solid crimson;
}
</style>