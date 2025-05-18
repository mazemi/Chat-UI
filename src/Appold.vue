<template>
  <div class="chat-app">
    <div class="sidebar">
      <div class="sidebar-header">
        <h2>Chat History</h2>
        <button @click="newChat" class="new-chat-btn">+ New Chat</button>
      </div>
      <div class="chat-history">
        <div 
          v-for="(chat, index) in chatHistory" 
          :key="index" 
          class="chat-item"
          :class="{ active: activeChat === index }"
          @click="loadChat(index)"
        >
          {{ chat.title || `Chat ${index + 1}` }}
        </div>
      </div>
      <div class="user-profile">
        <div class="avatar">U</div>
        <span class="username">User</span>
      </div>
    </div>
    
    <div class="main-container">
      <div class="chat-container" ref="chatContainer">
        <div 
          v-for="(message, index) in currentChat.messages" 
          :key="index" 
          class="message"
          :class="{ 'user-message': message.role === 'user', 'assistant-message': message.role === 'assistant' }"
        >
          <div class="message-content">
            <div class="message-avatar">
              {{ message.role === 'user' ? 'U' : 'AI' }}
            </div>
            <div class="message-text">
              {{ message.content }}
            </div>
          </div>
        </div>
      </div>
      
      <div class="input-area">
        <form @submit.prevent="sendMessage">
          <input
            v-model="userInput"
            type="text"
            placeholder="Type your message..."
            @keyup.enter="sendMessage"
          />
          <button type="submit">Send</button>
        </form>
        <div class="hint-text">
          <small>Press Enter to send, Shift+Enter for new line</small>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ChatApp',
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
      return this.chatHistory[this.activeChat] || { messages: [] };
    }
  },
  methods: {
    sendMessage() {
      if (!this.userInput.trim()) return;
      
      // Add user message
      this.currentChat.messages.push({
        role: 'user',
        content: this.userInput
      });
      
      // Clear input
      this.userInput = '';
      
      // Scroll to bottom
      this.$nextTick(() => {
        this.scrollToBottom();
      });
      
      // Simulate assistant response (replace with actual API call)
      setTimeout(() => {
        this.currentChat.messages.push({
          role: 'assistant',
          content: 'This is a simulated response. In a real app, you would connect to a chat API here.'
        });
        this.$nextTick(() => {
          this.scrollToBottom();
        });
      }, 1000);
    },
    newChat() {
      this.chatHistory.push({
        title: `Chat ${this.chatHistory.length + 1}`,
        messages: [
          { role: 'assistant', content: 'Hello! How can I assist you today?' }
        ]
      });
      this.activeChat = this.chatHistory.length - 1;
    },
    loadChat(index) {
      this.activeChat = index;
    },
    scrollToBottom() {
      const container = this.$refs.chatContainer;
      container.scrollTop = container.scrollHeight;
    }
  },
  mounted() {
    this.scrollToBottom();
  }
}
</script>

<style scoped>
.chat-app {
  display: flex;
  height: 100vh;
  font-family: 'Arial', sans-serif;
}

.sidebar {
  width: 250px;
  background-color: #2c3e50;
  color: white;
  display: flex;
  flex-direction: column;
}

.sidebar-header {
  padding: 15px;
  border-bottom: 1px solid #34495e;
}

.sidebar-header h2 {
  margin: 0 0 10px 0;
  font-size: 1.2rem;
}

.new-chat-btn {
  width: 100%;
  padding: 8px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.new-chat-btn:hover {
  background-color: #2980b9;
}

.chat-history {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
}

.chat-item {
  padding: 10px;
  margin: 5px 0;
  border-radius: 4px;
  cursor: pointer;
}

.chat-item:hover {
  background-color: #34495e;
}

.chat-item.active {
  background-color: #3498db;
}

.user-profile {
  padding: 15px;
  display: flex;
  align-items: center;
  border-top: 1px solid #34495e;
}

.avatar {
  width: 30px;
  height: 30px;
  background-color: #3498db;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 10px;
}

.main-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  background-color: #ecf0f1;
}

.chat-container {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}

.message {
  margin-bottom: 15px;
}

.message-content {
  display: flex;
  max-width: 80%;
}

.message-avatar {
  width: 40px;
  height: 40px;
  background-color: #bdc3c7;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 10px;
  flex-shrink: 0;
}

.user-message .message-content {
  margin-left: auto;
  flex-direction: row-reverse;
}

.user-message .message-avatar {
  background-color: #3498db;
  color: white;
  margin-right: 0;
  margin-left: 10px;
}

.user-message .message-text {
  background-color: #3498db;
  color: white;
}

.assistant-message .message-avatar {
  background-color: #2ecc71;
  color: white;
}

.assistant-message .message-text {
  background-color: white;
}

.message-text {
  padding: 10px 15px;
  border-radius: 18px;
  word-wrap: break-word;
}

.input-area {
  padding: 15px;
  border-top: 1px solid #bdc3c7;
  background-color: white;
}

.input-area form {
  display: flex;
}

.input-area input {
  flex: 1;
  padding: 10px 15px;
  border: 1px solid #bdc3c7;
  border-radius: 20px;
  outline: none;
}

.input-area button {
  margin-left: 10px;
  padding: 10px 20px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 20px;
  cursor: pointer;
}

.input-area button:hover {
  background-color: #2980b9;
}

.hint-text {
  text-align: center;
  margin-top: 5px;
  color: #7f8c8d;
  font-size: 0.8rem;
}
</style>