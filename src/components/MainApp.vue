<template>
  <div class="main-app">
    <header class="top-bar">
      <div class="app-name">CSC</div>
      <div class="settings-icon" @click="onShowSettings">⚙️
      </div>
      
    </header>
    <main class="middle-section">
      <div v-if="activeTab === 'conversations'">
        <h2>Conversations</h2>        
        <ul>
          <li v-for="(messages, conversationName) in conversations" :key="conversationName">
            <span @click="selectConversation(conversationName, messages)">
              <strong>{{ conversationName }}:</strong> {{ messages[messages.length - 1].sender }}: {{ messages[messages.length - 1].msg }}
            </span>
            <button @click="getConversationsContact(conversationName)">Info</button>
            <button @click="handleConversationsContact(conversationName)">Info2</button>
          </li>
        </ul>
      </div>
      <div v-if="activeTab === 'contacts'">
        <h2>Contacts</h2>
        <ul>
          <li v-for="contact in contacts" :key="contact.name">
            <div class="contact-item">
              <span @click="selectContact(contact)">
                <strong>{{ contact.name }}</strong> ({{ contact.type }})
              </span>
              <button @click="handleStartConversation(contact)">Start Chat</button>
            </div>
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
      const conversationResponse = await fetch(`${import.meta.env.BASE_URL}dummyConversation.json`);
      this.conversations = await conversationResponse.json();

      const contactResponse = await fetch(`${import.meta.env.BASE_URL}dummyContacts.json`);
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
    selectContact(contact) { // emit to App.vue
      const conversationsList = this.getContactConversations(contact);
      this.$emit('contact-selected', {
        contactInfo: contact, 
        conversationsList: conversationsList,
        activeTab: this.activeTab
      });
      console.log('activeTab:', this.activeTab);
      console.log('activeTab in selectContact:', this.activeTab);
    },
    selectConversation(conversationName, messages) { // emit to App.vue
      const contactsList = this.getConversationsContact(conversationName);
      this.$emit('conversation-selected', {
        name: conversationName,
        messages: messages,
        contactsList: contactsList,
        activeTab: this.activeTab
      });
      console.log('activeTab:', this.activeTab);
      console.log('activeTab in selectConversation:', this.activeTab);
    },

    changeTab(tab) { // emit to App.vue
      document.title = tab;
      this.$emit('tab-changed', tab);
    },
    greet(event) {
      console.log("activeTab value:", this.activeTab);
      // `event` is the native DOM event
    },
    getConversationsContact(conversationName) {
      console.log('Getting contact for conversation:' + conversationName);

      // Todo: refactor
      const foundContact = this.contacts.find(
        (contact) => contact.name === conversationName
      );

      if(foundContact) {
        console.log('Found contact:', foundContact);
      } else {
        console.log('contact does not exist');
      };

      return foundContact;
    },
    handleConversationsContact(conversationName){
      console.log('Handling start or select contact for conversation:', conversationName);

      // Todo: refactor
      const foundContact = this.contacts.find(
        (contact) => contact.name === conversationName
      );

      if(foundContact) {
        console.log('Found contact:', foundContact);
        this.selectContact(foundContact);
      } else {
        console.log('contact does not exist');
      };

    },
    getContactConversations(contact) {
      console.log('Getting contact conversations for:'+ contact);

      // Todo: refactor
      const existingConversationName = Object.keys(this.conversations).find(
        (conversationName) => conversationName === contact.name // Assuming conversation name is contact name, Todo: use id from convo_db
      );

      if(existingConversationName) {
        console.log('convo exist:' + existingConversationName);
      } else {
        console.log('convo does not exist');
      };
      const existingConversationsList = {
          name: existingConversationName,
          messages: this.conversations[existingConversationName],
        };
      return existingConversationsList;
    },
    handleStartConversation(contact) {
      console.log('Handling start or select conversation for contact:', contact);
      // Find if a conversation with this contact already exists
      const existingConversationName = Object.keys(this.conversations).find(
        (conversationName) => conversationName === contact.name // Assuming conversation name is contact name, Todo: use id from convo_db
      );

      if (existingConversationName) {
        console.log('conversation exist');
        // If conversation exists, select it
        this.selectedConversation = {
          name: existingConversationName,
          messages: this.conversations[existingConversationName],
        };

        this.selectConversation(existingConversationName, this.conversations[existingConversationName]);
        
      } else { // TODO: handle error
        console.log('conversation does not exist');
        // If conversation doesn't exist, create a new one
        const newConversationName = contact.name; // Assuming new conversation name is contact name
        this.$set(this.conversations, newConversationName, []); // Use Vue.set to ensure reactivity
        this.selectedConversation = {
          name: newConversationName,
          messages: this.conversations[newConversationName],
        };
      }
      this.selectedContact = null; // Clear selected contact after handling conversation
    },
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