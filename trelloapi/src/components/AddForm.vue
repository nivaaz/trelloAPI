<template>
    <form @submit.prevent="handleSubmit">
        <label> Name </label>
     <input 
        v-model="board.name" 
        type="text"
        @focus="clearStatus"
        @keypress="clearStatus"
        ref="first"
         />
     
        <label> Number of Cards </label>
     <input 
        v-model="board.cardsNumber" 
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        @focus="clearStatus"
     />

     <p v-if="error && submitting" class="error-message">
        ❗Please fill out all required fields
     </p>
     <p v-if="success" class="success-message">
        ✅ Board successfully added
     </p>
     
     <button > Add </button>
    </form>
</template>

<script>
export default {
    name: 'AddForm',
   data(){
       return {
            submitting : false, 
     error : false, 
     success : false, 

           board :{
               name:'',
               cardsNumber:'',
           }
       }
   },
   computed:{
       invalidName(){
           return this.board.name===''
       },
       invalidEmail(){
           return this.board.cardsNumber===''
       }
   },
   methods:{
       handleSubmit(){
           console.log("handling Submit")

           this.submitting = true;
           this.clearStatus()

           if (this.invalidName || this.invalidEmail){
               this.error = true;
               return 
           }

           this.$emit('add:board', this.board)

            this.$refs.first.focus()
            //adding was succesful 
           this.error = false;
           this.success= true;
           this.submitting = false;
           
            this.resetBoard();

       },
       clearStatus(){
           this.success = false;
           this.error = false;
       },
       resetBoard(){
           this.board.name = '';
           this.board.cardsNumber = '';
       }
   }
}
</script>

<style scoped>
h1{
    color: pink;
}
button{
    color: crimson;
    text-transform: uppercase;
    background-color: white;
    border: 2px solid crimson;
    border-radius: 1em;
    padding: 1em;
    font-weight: bold;
}
input{
    color: crimson;
    border: 1px solid lightgray;
    border-radius: 1em;
    padding: 1em;
}
form {
    margin: 2em;
    text-align: left;
    display: grid;
    grid-gap: 1em;
    color: crimson;
    font-weight: bold;
  }


[class*='-message'] {
    font-weight: 500;
}

.error-message {
color: #d33c40;
}

.success-message {
color: #32a95d;
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