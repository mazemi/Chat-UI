<template>
  <div class="chat-app">
    <Sidebar
      :chatHistory="chatHistory"
      :activeChat="activeChat"
      @new-chat="newChat"
      @load-chat="loadChat"
      @model-change="setModel"
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
      selectedModel: 'local',
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
    setModel(model) {
      this.selectedModel = model
      console.log(model)
    },


    async sendMessage(message) {
      if (!message.trim()) return;

      this.currentChat.messages.push({
        role: 'user',
        content: message
      });

      this.userInput = '';
      this.isLoading = true;

      const payload = {
        prompt: message,
        model: this.selectedModel
      };

      console.log("Request Payload:", payload);
      
      try {
        const response = await fetch('http://127.0.0.1:8000/api/query', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            prompt: message,
            model: this.selectedModel 
          })
        });
        
        

        if (!response.ok) {
          throw new Error('Failed to get response from backend.');
        }

        const data = await response.json();

        this.currentChat.messages.push({
          role: 'assistant',
          content: data.html || 'No data returned.'
        });
      } catch (error) {
        this.currentChat.messages.push({
          role: 'assistant',
          content: 'Sorry, I didn\'t fully understand your query.'
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

.message-text table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1em;
  font-size: 14px;
}

.message-text th,
.message-text td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
}

.message-text th {
  background-color: #f2f2f2;
  font-weight: bold;
}
</style>