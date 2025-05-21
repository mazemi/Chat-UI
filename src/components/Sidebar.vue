<template>
  <div class="sidebar">

    <div class="Model">
      <label for="AIModel">Model:</label>

      <select id="AIModel" name="AIModel" v-model="selectedModel" @change="modelChanged">
        <option value="local">Local llama3.2</option>
        <option value="openai">OpenAI gpt-4o-mini</option>
      </select>
    </div>

    <div class="sidebar-header">
      <h2>Chat History</h2>
      <button @click="$emit('new-chat')" class="new-chat-btn">+ New Chat</button>
    </div>


    <div class="chat-history">
      <div 
        v-for="(chat, index) in chatHistory" 
        :key="index" 
        class="chat-item"
        :class="{ active: activeChat === index }"
        @click="$emit('load-chat', index)"
      >
        {{ chat.title || `Chat ${index + 1}` }}
      </div>
    </div>

    <div class="user-tools">
      <font-awesome-icon icon="gear" />
      <span class="tools-label">Setting</span>
    </div>

    <div class="user-tools">
      <font-awesome-icon icon="share" />
      <span class="tools-label">Share</span>
    </div>

    <div class="user-profile" @click="toggleProfilePopup">
      <div class="avatar">U</div>
      <span class="username">User</span>
      
      <div v-if="showProfilePopup" class="profile-popup">
        <!-- hard coded for now as REACH user -->
        <div class="popup-content">
          <div class="popup-header">
            <div class="popup-avatar">R</div>
            <h3>REACH</h3>
          </div>
          <div class="popup-details">
            <div class="detail-item">
              <span class="detail-value">reach@reach-initiative.org</span>
            </div>
            <div class="detail-item">
              <span class="detail-label">Member since:</span>
              <span class="detail-value">January 2025</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Sidebar',
  props: {
    chatHistory: {
      type: Array,
      required: true
    },
    activeChat: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      showProfilePopup: false,
      selectedModel: 'local',
      showProfilePopup: false
    }
  },
  methods: {
    toggleProfilePopup() {
      this.showProfilePopup = !this.showProfilePopup
    },
    modelChanged() {
      this.$emit('model-change', this.selectedModel)
    },
    toggleProfilePopup() {
      this.showProfilePopup = !this.showProfilePopup
    }
  }
}
</script>

<style scoped>
.sidebar {
  width: 250px;
  background-color: #f8f8f87e; /* light gray */
  color: #333; /* dark gray */
  display: flex;
  flex-direction: column;
  position: relative;
  border-right: 1px solid #d0d2d4;
}

.sidebar-header {
  padding: 15px;
  border-bottom: 1px solid #e0e0e0;
}

.sidebar-header h2 {
  margin: 0 0 10px 0;
  font-size: 1.2rem;
}

.new-chat-btn {
  width: 70%;
  padding: 8px;
  background-color: #4a90e2;
  color: #fff;
  border: none;
  border-radius: 20px;
  cursor: pointer;
}

.new-chat-btn:hover {
  background-color: #357ab8;
}

.chat-history {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
}

.chat-item {
  padding: 8px;
  margin: 3px 0;
  border-radius: 4px;
  cursor: pointer;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.chat-item:hover {
  bac
}
.user-tools {
  padding: 15px;
  display: flex;
  align-items: center;
  border-top: 1px solid #d0d2d4;
  cursor: pointer;
  position: relative;
}

.user-tools:hover {
  background-color: #eaeaea;
}

.tools-label {
  padding-left: 10px;
}

.user-profile {
  padding: 15px;
  display: flex;
  align-items: center;
  border-top: 1px solid #d0d2d4;
  cursor: pointer;
  position: relative;
}

.user-profile:hover {
  background-color: #e0e0e0;
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
  font-weight: bold;
  color: #fff;
}

.profile-popup {
  position: absolute;
  bottom: 70px;
  left: 15px;
  width: 220px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  z-index: 100;
  color: #333;
}

.popup-content {
  padding: 15px;
}

.popup-header {
  display: flex;
  align-items: center;
  margin-bottom: 15px;
}

.popup-avatar {
  width: 40px;
  height: 40px;
  background-color: #3498db;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 10px;
  color: white;
  font-weight: bold;
}

.popup-header h3 {
  margin: 0;
  font-size: 1rem;
}

.popup-details {
  font-size: 0.8rem;
}

.detail-item {
  margin-bottom: 8px;
  display: flex;
}

.detail-label {
  font-weight: bold;
  margin-right: 5px;
  min-width: 80px;
}

.detail-value {
  word-break: break-word;
}

/* Triangle pointer */
.profile-popup::after {
  content: '';
  position: absolute;
  bottom: -10px;
  left: 20px;
  border-width: 10px 10px 0;
  border-style: solid;
  border-color: #ffffff transparent transparent;
}

.Model {
  padding: 15px;
  padding-bottom: 20px;
  border-bottom: 1px solid #d0d2d4;
  background-color: #fff;
}

.Model label {
  font-weight: bold;
  margin-bottom: 8px;
  display: inline-block;
}

.Model select {
  padding: 4px 10px;
  border-radius: 3px;
  border: 1px solid #ccc;
  background-color: #fefefe;
  font-size: .9rem;
  cursor: pointer;
  transition: border-color 0.2s ease;
  min-width: 200px;
}

.Model select:hover {
  border-color: #4a90e2;
}

.Model select:focus {
  outline: none;
  border-color: #357ab8;
  box-shadow: 0 0 5px rgba(53, 122, 184, 0.6);
}
</style>
