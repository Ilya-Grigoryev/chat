<!-- Тестирую git commit, не обращайте внимания)) -->
<template>
  <div id="app">
    <Container>
      <ChatWindow v-on:send-message="sendMessage">
        <ChatMessage v-for="message in messages" :key="message.id" :username="message.author" :datetime="message.datetime"><div class="message">{{message.text}}</div></ChatMessage>
      </ChatWindow>
    </Container>
  </div>
</template>

<script>
import ChatMessage from './components/ChatMessage.vue'
import ChatWindow from './components/ChatWindow.vue'
import Container from './components/Container.vue'
import { delay } from 'q'

export default {
  name: 'app',
  data(){
    return{
      messages: []
    }
  },
  methods: {
    getMessage(){
      this.axios.get('http://188.225.47.187/api/chat/getmessages.php')
      .then((response) => {
        this.messages = response.data
        this.messages.reverse()
      })
    },
    sendMessage(newMess){
      this.axios.post('http://188.225.47.187/api/chat/sendmessage.php',{
            author: newMess.username,
            text: newMess.text
      })
      delay(100)
      ChatWindow.nickname = ''
      ChatWindow.message = ''
      this.getMessage()
    }
  },
  mounted(){
    setInterval(this.getMessage(),1)
  },
  components: {
    ChatMessage, ChatWindow, Container
  }
}
</script>

<style>
body {
  margin: 0;
  background-color: #f9f9fa;
}
.message{
  font-size: 20px;
}
</style>
