<template>
<div>
  <h1 class="title"> Trello Pop </h1>
    <div class ="renderBox">
   
    <div >
        <h1 v-if="this.boards===[]"> No boards </h1>
        <h1 v-else> Render board </h1> 
        <div v-for="board in boards" :key="board">
        <button class="data" @click="updateBoard(board.id)"> {{board.name}}</button>
        </div>
    </div>

    <div>
        <h1> Render list </h1> 
        <div v-for="list in lists" :key="list">
        <button  class="data" @click="updateList(list.id)"> {{list.name}} </button>
        </div>
    </div>
    
    <div>
        <h1> Render card </h1> 
        <div v-for="card in cards" :key="card">
       <button class="data" @click="updateCard(card.id)"> {{card.name}} </button>
        </div>
    </div>
    </div>

    <div class="inputBoxes">
        <form @submit.prevent="handleSubmit">
            <h2> Add Token </h2> 
            <input 
                v-model="token" 
                type="text"
                ref="token"
                />
            <button > Add </button>
        </form>

        <h2> Add Board Id </h2> 
        <input 
            v-model="boardid" 
            type="text"
            ref="board_id"
            />
        <button @click = "getCards"> Get Cards </button> 
    
        <h2> Add List id </h2>  
        <input 
            v-model="list_id" 
            type="text"
            ref="list_id"
            />
        <button @click = "addListid"> Add </button> 

        <h2> Add name for card</h2> 
        <label> Card name/s </label>
        <input 
            v-model="card_id" 
            type="text"
            ref="card_id"
            />
        <button @click = "makeCard"> Add </button> 
        <label> Each card name should be seperated by a ', '</label>
        <button @click = "addCardsList"> Add List of Cards </button>
    
       
    </div>

</div>
</template>

<script>
export default {
    name: 'TrelloPop',
   data(){
       return {
            token:'',
            boardid:'',
            listid:'',
            cardid:' ',
            cards:[],
            boards: [], 
            lists: [],
       }
   },
   methods:{
       updateList(id){
           console.log("updating list")
           this.listid = id;
           this.getCards();
        },
       updateBoard(id){
           console.log(id)
           console.log("updating board")
           this.boardid = id;
           console.log(this.boardid)
            this.getLists();
       },
      addCardsList(){
        const cards = this.card_id;
        // const urls = this.url_id;
        const cardsList = cards.split("~~")
        // const urlList = urls.split("~~")
        cardsList.map((val)=>{
            this.postCard({
                   name: val,
                //    url: urlList[i]
                } ) 
        })            
       console.log("done!")
        },
        handleSubmit(){
            console.log("handling Submit")
            console.log(this.token)
            this.getBoards();
        },
        handlecardid(name){
            console.log(name)
            const card = {
                name: name.card_id
            }
            this.sendCard(card);
        },
        
        addBoards(data){
            const dt = data.map((val)=>{
                const b = {
                    name: val.name,
                    id: val.id,
                }
                return b
            })
            console.log("dt")
            console.log(dt)
            this.boards = dt
        },
        addCards(data){
            console.log(this.cardid)
            const dt = data.map((val)=>{
                const b = {
                    name: val.name,
                    id: val.id,
                }
                return b
            })
            console.log("dt")
            console.log(dt)
            this.cards = dt
        },
        addLists(data){
        const dt = data.map((val)=>{
            const b = {
                name: val.name,
                id: val.id,
            }
        return b
            })
            console.log("dt")
            console.log(dt)
            this.lists = dt
        },
        addListid(){
            console.log(this.list_id)
            this.listid = this.list_id;
        },
        makeCard(){
            console.log(this.card_id);
            const card = {
                name: this.card_id,
            //  url: this.url_id
             };
            this.postCard(card);
        },
      
    async getBoards(){
        try {
        const API_ME_BASE = "https://api.trello.com/1/members/me/";
        const KEY = "47836fa3a77b0663415e2e5f7152222b";
        const TOKEN = this.token;
        const response = await fetch(`${API_ME_BASE}boards?key=${KEY}&token=${TOKEN}&fields=name`);
        const data = await response.json()
        console.log(data)
        this.addBoards(data)

        } catch (error) {
        console.log("error")
        console.error(error)
    }
    },
    async getCards(){
        try {
        const API_ME_BASE = "https://api.trello.com/1/boards/";
        const API_KEY = "47836fa3a77b0663415e2e5f7152222b";
        const TOKEN = this.token;
        const BOARDID = this.boardid;
        const response = await fetch(`${API_ME_BASE}${BOARDID}/cards/?limit=2&fields=name&members=true&member_fields=fullName&key=${API_KEY}&token=${TOKEN}`);
        const data = await response.json()
        console.log(data)
        this.addCards(data)
        // get the lists 
        this.getLists();

        } catch (error) {
        console.log("error")
        console.error(error)
    }
    },
    async getLists(){
        try {
            const API_ME_BASE = "https://api.trello.com/1/boards/";
            const API_KEY = "47836fa3a77b0663415e2e5f7152222b";
            const TOKEN = this.token;
            const BOARD_ID = this.boardid;
            const response = await fetch(`${API_ME_BASE}${BOARD_ID}/lists?cards=none&card_fields=all&filter=open&fields=all&key=${API_KEY}&token=${TOKEN}`);
            const data = await response.json()
            console.log(data)
            this.addLists(data)

        } catch (error) {
            console.log("error")
            console.error(error)
        }
    },
    async postCard(card){
    console.log("POSTING CARD")
    const data = null;
    const API_ME_BASE = "https://api.trello.com/1/cards?name=";
    const API_KEY = "47836fa3a77b0663415e2e5f7152222b";
    const TOKEN = this.token;
    const LIST_ID = this.listid;
    const CARD_NAME = card.name;
    // const URL_SOURCE = card.url;

    var xhr = new XMLHttpRequest();

    xhr.addEventListener("readystatechange", function () {
    if (this.readyState === this.DONE) {
        console.log(this.responseText);
    }
    });

    // xhr.open("POST", `${API_ME_BASE}${CARD_NAME}&idList=${LIST_ID}&urlSource=${URL_SOURCE}&keepFromSource=all&key=${API_KEY}&token=${TOKEN}`);
    xhr.open("POST", `${API_ME_BASE}${CARD_NAME}&idList=${LIST_ID}&keepFromSource=all&key=${API_KEY}&token=${TOKEN}`);

    xhr.send(data);
   },
   }
}
</script>

<style scoped>
h1{
    color: crimson;
    font-size: medium;
    text-transform: uppercase;  
}
h1.title{
    font-size: x-large;
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
button.data{
    color:white;
    background-color: crimson;
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
.renderBox{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
}
.inputBoxes{
    display: grid;
    grid-gap: 1em;
    margin: 1em;
    color: crimson;
    text-align: left;
}
div{
    padding: 1em;
    grid-gap: 1em;
}
</style>