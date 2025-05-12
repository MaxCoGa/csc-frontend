<template> 
  <div class="conversation-container">
    <button @click="onCloseConversation">Back</button>
    <h2>{{ conversation.name }}</h2>
    <div class="messages-list">
      <div v-for="(message, index) in conversation.messages" :key="index" class="message">
        <strong>{{ message.sender }}:</strong> {{ message.msg }}
      </div>
    </div>
    <div class="message-input">
      <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Type a message..." />
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Conversation',
  props: {
    onCloseConversation: Function, // Prop for closing the conversation
    conversation: {
      type: Object, // Correct type as we pass the object { name, messages }
      required: true
    },
  },
  data() {
    return {
      newMessage: '',
    };
  },
  methods: {
    // This is a mock send message function.
    // In a real app, this would send the message to the backend.
    sendMessage() {
      if (this.newMessage.trim() !== '') {
        const newMsg = {
          sender: 'You', // Assuming the current user is "You" for mock data
          msg: this.newMessage.trim(),
        };
        this.newMessage = '';
      }
    },

  },
};
</script>

<style scoped>
.conversation-container { 
  display: flex;
  flex-direction: column;
  height: 100%;
  padding: 10px;
  border: 1px solid #555;
  border-radius: 5px;
  background-color: #333;
  color: #eee;
  box-sizing: border-box; /* Include padding and border in the element's total width and height */
  overflow: hidden; /* Prevent content from overflowing if not handled by flex items */
}

.messages-list {
  flex-grow: 1;
  overflow-y: auto;
  background-color: #444;
  padding: 10px;
  border-radius: 5px;
  word-wrap: break-word; /* Ensure long words break and wrap */
  white-space: pre-wrap; /* Preserve whitespace and wrap text */
}

.message {
  color: #eee;
}

.message:not(:last-child) {
  margin-bottom: 5px;
}

.message-input {
  display: flex;
  padding-top: 10px; /* Add some space above the input */
}
.message-input input { 
  flex-grow: 1;
  padding: 8px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

.message-input button {
  padding: 8px 15px;
  background-color: #555;
 color: #eee; /* Light text on button */
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.message-input button:hover {
  background-color: #666;
}
</style>