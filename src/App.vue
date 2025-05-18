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

      <div v-if="isLoading" class="spinner-wrapper">
        <Spinner width="2rem" height="2rem" label="Loading..." />
      </div>

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
import Spinner from './components/Spinner.vue'

export default {
  name: 'ChatApp',
  components: {
    Sidebar,
    ChatContainer,
    InputArea,
    Spinner
  },
  data() {
    return {
      userInput: '',
      isLoading: false,
      chatHistory: [
        {
          title: 'Chat 1',
          messages: []
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
    async sendMessage(message) {
    if (!message.trim()) return;

    // Add user message to chat
    this.currentChat.messages.push({
      role: 'user',
      content: message
    });

    this.userInput = '';
    this.isLoading = true;

    try {
      const response = await fetch('http://localhost:8000/api/chat', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          prompt: message
        })
      });

      if (!response.ok) {
        throw new Error('Failed to get response from backend.');
      }

      const data = await response.json();

      this.currentChat.messages.push({
        role: 'assistant',
        content: data.response
      });
    } catch (error) {
      this.currentChat.messages.push({
        role: 'assistant',
        content: 'Error: Could not connect to AI backend.'
      });
      console.error(error);
    } finally {
        this.isLoading = false;
      }
  },
  newChat() {
    this.chatHistory.push({
      title: `Chat ${this.chatHistory.length + 1}`,
      messages: []
    });
    this.activeChat = this.chatHistory.length - 1;
  },
  loadChat(index) {
    this.activeChat = index;
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
  /* background-color: #f8f8f8; */
}

.spinner-wrapper {
  display: flex;
  justify-content: center;
  padding: 1rem;
}
</style>