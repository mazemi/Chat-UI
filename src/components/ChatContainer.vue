<template>
  <div class="chat-container" ref="chatContainer">
    <Message 
      v-for="(message, index) in messages" 
      :key="index" 
      :message="message"
    />
  </div>
</template>

<script>
import Message from './Message.vue'


export default {
  name: 'ChatContainer',
  components: {
    Message,
  },
  props: {
    messages: {
      type: Array,
      required: true
    }
  },
  watch: {
    messages: {
      handler() {
        this.$nextTick(() => {
          this.scrollToBottom()
        })
      },
      deep: true
    }
  },
  methods: {
    scrollToBottom() {
      const container = this.$el
      container.scrollTop = container.scrollHeight
    }
  },
  mounted() {
    this.scrollToBottom()
  }
}
</script>

<style scoped>
.chat-container {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}
</style>