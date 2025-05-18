<template>
  <div class="chat-app">
    <Sidebar
      :chatHistory="chatHistory"
      :activeChat="activeChat"
      @new-chat="newChat"
      @load-chat="loadChat"
    />
    <div class="main-container">
      <ChatContainer :messages="currentChat.messages" />
        <InputArea 
        v-model="userInput" 
        @send-message="sendMessage" 
      />
    </div>
  </div>
</template>

<script>
import Sidebar from './components/Sidebar.vue'
import ChatContainer from './components/ChatContainer.vue'
import InputArea from './components/InputArea.vue'

export default {
  name: 'ChatApp',
  components: {
    Sidebar,
    ChatContainer,
    InputArea
  },
  data() {
    return {
      userInput: '',
      chatHistory: [
        {
          title: 'First Chat',
          messages: [
            { role: 'assistant', content: 'Hello! How can I assist you today?' }
          ]
        }
      ],
      activeChat: 0
    }
  },
  computed: {
    currentChat() {
      return this.chatHistory[this.activeChat] || { messages: [] }
    }
  },
  methods: {
    sendMessage(message) {  // Updated to accept message parameter
      if (!message.trim()) return
      
      this.currentChat.messages.push({
        role: 'user',
        content: message
      })
      
      this.userInput = ''
      
      setTimeout(() => {
        this.currentChat.messages.push({
          role: 'assistant',
          content: 'This is a simulated response.'
        })
      }, 1000)
    },
    newChat() {
      this.chatHistory.push({
        title: `Chat ${this.chatHistory.length + 1}`,
        messages: [
          { role: 'assistant', content: 'Hello! How can I assist you today?' }
        ]
      })
      this.activeChat = this.chatHistory.length - 1
    },
    loadChat(index) {
      this.activeChat = index
    }
  }
}
</script>

<style scoped>
.chat-app {
  display: flex;
  height: 100vh;
  font-family: 'Arial', sans-serif;
}

.main-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  background-color: #f8f8f8;
}
</style>