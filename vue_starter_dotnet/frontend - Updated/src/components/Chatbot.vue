<template>

<section class="chat-box">
 <br/>

<div class="chat-box-list-container" ref="chatbox">
<ul class="chat-box-list">

    <li
      class="message"
      v-for="(message, idx) in messages"
      :key="idx"
      :class="message.author"
    >
      <p>
    
        <span v-html="message.text"></span>
      </p>
    </li>
  
  </ul>
</div>
<div class="chat-inputs">
<input type="text" maxlength='100' placeholder='Type a message...' v-model="message" @keyup.enter="sendMessage"/>
<button @click="sendMessage">Send</button>
</div>
</section>

</template>

<script>
//import { createWriteStream } from 'fs';
export default {
name: 'Chatbot',
blank: ' fasfsf',

data: () => ({

message: '',
messages: [{text:"Hey there, what's your name?", author: "server"}],
}),
methods: {
sendMessage() {
let query = this.message

  if(query.includes("#")){
    query= this.message.replace("#","$");
  }
  if(query.includes("/")){
    query= this.message.replace("/","~");
  }
  const message = query
  this.messages.push({
    text: this.message,
    author: 'client'
  })
  this.message = ''

  let arrayLength = this.messages.length;
  if (arrayLength < 3)
  {  
      this.messages.push({
      text: "Hi " + message + ", what can I do for you today? ",
      author: 'server'
  })
  }
  else {

  fetch(`https://localhost:44392/api/chat/${message}`, {
  method: "GET"
  }) .then(response => {
  return response.text();
  })
  .then((data) => {
    let responseArray = data.split("&&&");
    responseArray.map((response) => {
      this.messages.push({
      text: response,
      author: 'server'
    })
    })
    
    this.$nextTick(() => {
      this.$refs.chatbox.scrollTop = this.$refs.chatbox.scrollHeight
    })
  })
  }
}
}
}
</script>

<style scoped lang="scss">

// this styling displays the presentation of the messages
.chat-box,
.chat-box-list {
background-color: white !important;
display: flex;
flex-direction: column;
list-style-type: none;
word-wrap: break-word;
height: 65vh;
}
//////////////// Scroll functionality of the
.chat-box-list-container {
background-color: white !important;
overflow: scroll;
margin-bottom: 1px;

}

// This styling displays the look of the messages;

.chat-box-list {
padding-left: 10px;
padding-right: 10px;
span {
padding: 1.5px;
color: white;
border-radius: 6px;
}
.server {
    margin-left: 15%;
    span {
        background: #8fc74a;
        box-shadow: 4px 4px 4px gray;
        float: right;
        padding: 10px;
        
         }
     p  {
          float: right;
        }
}
.client {
     span {
         background: #00afef;
         box-shadow: 4px 4px 4px gray;
         float: left;
         text-align: right;
         padding: 10px;
        }
      p {
        float: left;
        text-align: right;
        }
      }
}
//////////////////////////

// This style formats the overall Chatbox

.chat-box {

margin: 10px;
border: 5px solid #60686b;
width: 70vw;
height: 65vh;
border-radius: 10px;
 border-width: thick;
margin-left: auto;
margin-right: auto;
align-items: space-between;
justify-content: space-between;
}

// This styling formats the layout of the input-box and send button

.chat-inputs {
display: flex;


      input {
      line-height: 3;
      width: 100%;
      border: 1px solid #999;
      border-left: none;
      border-bottom: none;
      border-right: none;
      border-bottom-left-radius: 4px;
      padding-left: 15px;
      }
       button {
       width: 145px;
       color: white;
       background:linear-gradient(to right, #0fb4b4 0%, #0e7ba3 100%);
       border-color: #999;
       border-bottom: none;
       border-right:none;
       border-bottom-right-radius: 4px;
       border-left:none;
      }
}


</style>
