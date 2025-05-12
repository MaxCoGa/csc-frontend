<template>
  <div class="main-app">
    <header class="top-bar">
      <div class="app-name">CSC</div>
      <div class="settings-icon" @click="onShowSettings">⚙️
      </div>
      
    </header>
    <main class="middle-section">
      <div v-if="activeTab === 'conversations'">
        <h2>Conversations</h2>        <ul>
          <li v-for="(messages, conversationName) in conversations" :key="conversationName" @click="selectConversation(conversationName, messages)">
            <strong>{{ conversationName }}:</strong> {{ messages[messages.length - 1].sender }}: {{ messages[messages.length - 1].msg }}
          </li>
        </ul>
      </div>
      <div v-if="activeTab === 'contacts'">
        <h2>Contacts</h2>
        <ul>
          <li v-for="contact in contacts" :key="contact.name" @click="selectContact(contact)">
            <strong>{{ contact.name }}</strong> ({{ contact.type }})
          </li>
        </ul>
      </div>
    </main>

    <footer class="bottom-bar">
      <button @click="changeTab('conversations')" :class="{ active: activeTab === 'conversations' }">Conversations</button>
      <button @click="changeTab('contacts')" :class="{ active: activeTab === 'contacts' }">Contacts</button>
      <button @click="greet">test</button>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
 conversations: [],
      contacts: [],
    };
  },
  async mounted() {
    try {
      const conversationResponse = await fetch('/dummyConversation.json');
      this.conversations = await conversationResponse.json();

      const contactResponse = await fetch('/dummyContacts.json');
      this.contacts = await contactResponse.json();

    } catch (error) {
      console.error('Error fetching dummy data:', error);
    }

  },
  props: {
    onShowSettings: {
      type: Function,
      required: true
    },
    activeTab: {
 type: String,
      required: true
    }
  },
 methods: {
    selectContact(contact) {
 this.$emit('contact-selected', {
 contact: contact, activeTab: this.activeTab
      });
      console.log('activeTab:', this.activeTab);
 console.log('activeTab in selectContact:', this.activeTab);
    },
    selectConversation(conversationName, messages) {
      this.$emit('conversation-selected', {

        name: conversationName,
 messages: messages,
 activeTab: this.activeTab
      });
      console.log('activeTab:', this.activeTab);
 console.log('activeTab in selectConversation:', this.activeTab);
    },

    changeTab(tab) {
 this.$emit('tab-changed', tab);
    },
    greet(event) {
      console.log("activeTab value:", this.activeTab);
      // `event` is the native DOM event
    }
  },

  
  
};
</script>

<style scoped>
.main-app {
  display: flex;
  flex-direction: column;
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: sans-serif;
}

.top-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #282828; /* Dark gray background */
  border-bottom: 1px solid #555; /* Slightly lighter gray border */
  color: #cccccc; /* Light gray text */
}

.app-name {
  font-size: 1.2em;
  font-weight: bold;
  color: #cccccc; /* Light gray text */
}

.settings-icon {
  background: none;
  border: none;
  font-size: 1.5em;
  cursor: pointer;
  padding: 0; /* Remove padding from button inside settings-icon */
}

.middle-section {
  flex-grow: 1;
  padding: 10px;
  overflow-y: auto; /* Allow scrolling if content overflows */
}

.middle-section h2 {
  margin-top: 0;
  margin-bottom: 0;
  color: #cccccc; /* Light gray text for headings */
}

.middle-section ul {
  margin-top: 0;
  padding: 0;
}

.middle-section li {
  padding: 10px;
  border-bottom: 1px solid #555; /* Slightly lighter gray border */
  color: #cccccc; /* Light gray text */
  cursor: pointer;
}

.bottom-bar {
  display: flex;
  justify-content: space-around;
  padding: 10px;
  background-color: #282828; /* Dark gray background */
  border-top: 1px solid #555; /* Slightly lighter gray border */
}

.bottom-bar button {
  padding: 10px 20px;
  border: none;
  cursor: pointer;
  background-color: #3a3a3a; /* Medium gray background for buttons */
  color: #cccccc; /* Light gray text */
}

.bottom-bar button.active {
  background-color: #555; /* Lighter gray background for active button */
  font-weight: bold;
}
</style>