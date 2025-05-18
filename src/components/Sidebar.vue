<template>
  <div class="sidebar">
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
    <div class="user-profile" @click="toggleProfilePopup">
      <div class="avatar">U</div>
      <span class="username">User</span>
      
      <div v-if="showProfilePopup" class="profile-popup">
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
      showProfilePopup: false
    }
  },
  methods: {
    toggleProfilePopup() {
      this.showProfilePopup = !this.showProfilePopup
    }
  }
}
</script>

<style scoped>
.sidebar {
  width: 250px;
  background-color: #2c3e50;
  color: white;
  display: flex;
  flex-direction: column;
  position: relative;
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
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
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
  cursor: pointer;
  position: relative;
}

.user-profile:hover {
  background-color: #34495e;
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
}

.profile-popup {
  position: absolute;
  bottom: 70px;
  left: 15px;
  width: 220px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  z-index: 100;
  color: #2c3e50;
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
  word-break: break-all;
}

/* Triangle pointer */
.profile-popup::after {
  content: '';
  position: absolute;
  bottom: -10px;
  left: 20px;
  border-width: 10px 10px 0;
  border-style: solid;
  border-color: white transparent transparent;
}
</style>