<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <InputArray 
    :boards="boards" 
    @delete:board="deleteBoard"
    @edit:board="editBoard"

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
  mounted() {
    this.getEmployees()
  },
 methods:{
   async getEmployees(){
      try {
        // const API_BASE = "https://api.trello.com/1/";
        const API_ME_BASE = "https://api.trello.com/1/members/me/";
        const KEY = "47836fa3a77b0663415e2e5f7152222b";
        const TOKEN = "XXXX";
        const response = await fetch(`${API_ME_BASE}boards?key=${KEY}&token=${TOKEN}&fields=name`);
        const data = await response.json()
        console.log(data)
      } catch (error) {
        console.log("error")
       console.error(error)
  }
   },
   deleteBoard(id) {
    this.boards = this.boards.filter(
      board => board.id !== id
    )
  },
  editBoard(id, updatedBoard){
   this.boards = this.boards.map(board =>
    board.id === id ? updatedBoard : board
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
