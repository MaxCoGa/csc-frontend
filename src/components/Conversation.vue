<template> 
  <div class="conversation-container">
    <div class="header-buttons">
      <button @click="onCloseConversation">Back</button>
      <button @click="handleConverse">Converse</button>
    </div>
    <div class="conversation-title" style="display: flex; align-items: center;">
      <img src="https://i.pravatar.cc/50" alt="User avatar" style="max-width: 100%; margin-right: 10px;">
      <h2>{{ conversation.name }}</h2> 
    </div>
    <div class="messages-list" ref="messagesList" @scroll="handleScroll">
      <div v-for="(message, index) in conversation.messages" :key="index" class="message">
        <strong>{{ message.sender }}:</strong> <span v-html="message.msg"></span>
      </div>
    </div>
    <div v-if="showNewMessageNotification" class="new-message-notification" @click="scrollToBottom">
      New message(s)
    </div> 
    <div class="message-input">
      <input v-model="newMessage" @keyup.enter="sendMessage" @paste="handlePaste" placeholder="Type a message..." ></input>
      <button @click="sendImage">Img</button>
      <button @click="sendMessage">Send</button>
      <input type="file" ref="imageInput" accept="image/*" style="display: none;" @change="handleImageSelected">

    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    // Scroll to the bottom when the component is mounted
    this.$refs.messagesList.scrollTop = this.$refs.messagesList.scrollHeight;
  },

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
      showNewMessageNotification: false,
    }
  },
  methods: {
    // This is a mock send message function.
    // In a real app, this would send the message to the backend.
    sendMessage() {
      if (this.newMessage.trim() !== '') {
        // console.log("msg sent: " + this.newMessage);
        const newMsg = {
          sender: 'You', // Assuming the current user is "You" for mock data
          msg: this.newMessage.trim(),
        };
        this.addFakeMessage(this.newMessage.trim());
        this.newMessage = '';
      }
    },
    sendImage() {
     this.$refs.imageInput.click();
    },
    handleImageSelected(event) {
      const file = event.target.files[0];
      // console.log('Selected file:', file);
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          // console.log('Image Base64:', e.target.result);
          this.addFakeMessage(e.target.result);
          this.clearFileInput(event);
        };
        reader.readAsDataURL(file);
      }
    },
    addFakeMessage(msg) {
      // console.log("Fake msg received: " + msg);

      const handleMsg = this.handleMessage(msg);

      this.conversation.messages.push({
        sender: 'You',
        msg: handleMsg,
      });

      const messagesList = this.$refs.messagesList;
      // Check if the user is already at the bottom
      const isAtBottom = messagesList.scrollHeight - messagesList.scrollTop <= messagesList.clientHeight + 1; // Add a small buffer

      if (isAtBottom) {
        this.showNewMessageNotification = false;
        this.$nextTick(() => {
          messagesList.scrollTop = messagesList.scrollHeight;
        });
      } else {
        this.showNewMessageNotification = true;
      }
    },
    scrollToBottom() {
      this.showNewMessageNotification = false;
      this.$refs.messagesList.scrollTop = this.$refs.messagesList.scrollHeight;
    },
    handleScroll() {
      const messagesList = this.$refs.messagesList;
      // Check if the user has scrolled to the bottom
      const isAtBottom = messagesList.scrollHeight - messagesList.scrollTop <= messagesList.clientHeight + 1; // Add a small buffer
      if (isAtBottom) {
        this.showNewMessageNotification = false;
      }
    },
    clearFileInput(event) {
      event.target.value = '';
    },
    handleMessage(msg) {
      // console.log("handling: " + msg);
      // Check if the message is a Base64 image
      if (msg.startsWith('data:image/') && msg.includes(';base64,')) {
        return `<img src="${msg}" alt="User uploaded image" style="max-width: 100%;" onerror="console.error('corrupted base64 img')">`;
      } else {
        const urlRegex = /(https?:\/\/[^\s]+)/g;
        const messageWithLinks = msg.replace(urlRegex, (url) => {
          const imageRegex = /\.(jpg|png|jpeg|webp|gif)$/i;
          if (imageRegex.test(url)) {
            return `<img src="${url}" alt="User uploaded image" style="max-width: 100%;">`;
          } else {
            return `<a href="${url}" target="_blank">${url}</a>`;
          }
        });

        return messageWithLinks;
      }
    },
    handlePaste(event) {
      const items = event.clipboardData.items;
      for (let i = 0; i < items.length; i++) {
        const item = items[i];
        if (item.type.indexOf('image') !== -1) {
          const file = item.getAsFile();
          // console.log('Pasted image file:', file.name);
          if (file) {
            event.preventDefault(); // Prevent default paste only for images
            const reader = new FileReader();
            reader.onload = (e) => {
              // console.log('Pasted Image Base64:', e.target.result);

              this.addFakeMessage(e.target.result);
              this.clearFileInput(event);
            };
            reader.readAsDataURL(file);
          }
          break; // Stop after finding the first image
        }
      }
    },
    handleConverse() {
      console.log("fake Converse")
    }
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

.new-message-notification {
  position: absolute;
  bottom: 60px; /* Adjust based on input height */
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(0, 123, 255, 0.8);
  color: white;
  padding: 8px 15px;
  border-radius: 20px;
  cursor: pointer;
}
</style>