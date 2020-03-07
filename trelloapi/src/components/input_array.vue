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
        <td v-if="editing === board.id">
            <input type="text" v-model="board.name" />
        </td>
        <td v-else>{{board.name}}</td>
        <td v-if="editing === board.id">
            <input type="text" v-model="board.cardsNumber" />
        </td>
        <td v-else>{{board.cardsNumber}}</td>
        <td v-if="editing === board.id">
            <button @click="editBoard(board)">Save</button>
            <button class="muted-button" @click="editing = null">Cancel</button>
        </td>
        <td v-else>
            <button @click="editMode(board.id)">Edit</button>
            <button @click="$emit('delete:board', board.id)">Delete</button>
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
        editing: -1,
    },
    methods:{
        editMode(id){
            this.editing = id;
        },
        editBoard(board){
             if (board.name === '' || board.email === '') return
            this.$emit('edit:board', board.id, board)
            this.editing = null
        }

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