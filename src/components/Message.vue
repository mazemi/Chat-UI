<template>
  <div class="message" :class="messageClass">
    <div class="message-content">
      <div class="message-avatar">
        {{ avatarText }}
      </div>
      <div class="message-text">
        {{ message.content }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Message',
  props: {
    message: {
      type: Object,
      required: true
    }
  },
  computed: {
    messageClass() {
      return {
        'user-message': this.message.role === 'user',
        'assistant-message': this.message.role === 'assistant'
      }
    },
    avatarText() {
      return this.message.role === 'user' ? 'U' : 'AI'
    }
  }
}
</script>

<style scoped>
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
  background-color: #eceef0;
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
</style>