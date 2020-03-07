<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <InputArray 
    :boards="boards" @delete:board="deleteBoard"
     />
    <AddForm @add:board="addBoard"/>
  </div>
</template>

<script>
import InputArray from './components/input_array.vue'
import AddForm from './components/AddForm.vue'

export default {
  name: 'App',
  components: {
    InputArray, 
    AddForm
  }, 
 data(){
   return {
      boards:[
        {
          id: 0, 
          name: "boardOne",
          cardsNumber: "22",
        },
        {
          id: 1, 
          name: "boardTwo",
          cardsNumber: "2",
        },
           ]
   }
 },
 methods:{
   deleteBoard(id) {
    this.boards = this.boards.filter(
      board => board.id !== id
    )
  },
   addBoard(board){
     const lastId =
    this.boards.length > 0
      ? this.boards[this.boards.length - 1].id
      : 0;
  const id = lastId + 1;
    const newBoard = { ...board, id };
     this.boards =[...this.boards, newBoard]
   }
 }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
