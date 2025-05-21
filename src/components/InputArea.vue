<template>
  <div class="input-area">
    <form @submit.prevent="handleSubmit">
      <textarea
        ref="textarea"
        v-model="localInput"
        placeholder="Ask me something ..."
        @keydown.enter.exact.prevent="handleSubmit"
        @keydown.shift.enter="handleNewLine"
        @input="adjustHeight"
        rows="1"
      ></textarea>
    </form>
  </div>
</template>

<script>
export default {
  name: 'InputArea',
  props: {
    value: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      localInput: this.value
    }
  },
  watch: {
    value(newVal) {
      this.localInput = newVal
    }
  },
  mounted() {
    this.adjustHeight()
  },
  methods: {
    handleSubmit() {
      if (this.localInput.trim()) {
        this.$emit('send-message', this.localInput)
        this.localInput = ''
        this.$emit('input', '')
        this.$nextTick(() => {
          this.adjustHeight()
        })
      }
    },
    handleNewLine(e) {
      // Insert new line at cursor position
      const cursorPos = e.target.selectionStart
      this.localInput = this.localInput.substring(0, cursorPos) + '\n' + this.localInput.substring(cursorPos)
      this.$emit('input', this.localInput)
      this.$nextTick(() => {
        e.target.selectionStart = cursorPos + 1
        e.target.selectionEnd = cursorPos + 1
        this.adjustHeight()
      })
    },
    adjustHeight() {
      const textarea = this.$refs.textarea
      textarea.style.height = 'auto'
      textarea.style.height = `${Math.min(textarea.scrollHeight, 200)}px`
    }
  }
}
</script>

<style scoped>
.input-area {
  padding: 5px;
  border-top: 1px solid #e3e5e6;
  background-color: white;
}

.input-area form {
  display: flex;
  align-items: flex-end;
}

.input-area textarea {
  flex: 1;
  padding: 10px 10px;
  /* border: 1px solid #bdc3c7; */
  border-style: none;
  border-radius: 10px;
  outline: none;
  resize: none;
  min-height: 33px;
  max-height: 65px;
  line-height: 1;
  font-family: inherit;
  overflow-y: auto;
}

.hint-text {
  text-align: center;
  margin-top: 5px;
  color: #7f8c8d;
  font-size: 0.8rem;
}
</style>