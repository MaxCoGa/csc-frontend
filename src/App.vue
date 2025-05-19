<script setup>
import { ref } from 'vue';
import LoginSignUp from './components/LoginSignUp.vue'
import MainApp from './components/MainApp.vue'
import Conversation from './components/Conversation.vue';
import UserSettings from './components/UserSettings.vue';
import Contact from './components/Contact.vue';

const isLoggedIn = ref(false);
const showSettings = ref(false);
const activeTab = ref('conversations'); // Add activeTab state use in MainApp.vue

const handleLoginSuccess = () => {
  isLoggedIn.value = true;
};

const handleShowSettings = () => {
  showSettings.value = true;
};

const handleHideSettings = () => {
  showSettings.value = false;
};

const selectedConversation = ref(null);

const handleConversationSelected = (conversation) => {
  selectedConversation.value = conversation;
};

const handleCloseConversation = () => {
  selectedConversation.value = null;
};


const selectedContact = ref(null);

const handleContactSelected = (contact) => {
  selectedContact.value = contact;
  // console.log('test:'+contact.contact.name);
};

const handleCloseContact = () => {
  selectedContact.value = null;
};

const handleTabChange = (tab) => {
  activeTab.value = tab;
};


</script>

<template>
  <div v-if="!isLoggedIn" class="center-container">
    <LoginSignUp @login-success="handleLoginSuccess" />
  </div>
  <UserSettings v-else-if="showSettings" @hide-settings="handleHideSettings" />
  <Contact v-else-if="selectedContact" :contact="selectedContact" @close-contact="handleCloseContact" @conversation-selected="handleConversationSelected"/>
  <Conversation v-else-if="selectedConversation" :conversation="selectedConversation" @close-conversation="handleCloseConversation" />
  <MainApp v-else :activeTab="activeTab" @show-settings="handleShowSettings" @conversation-selected="handleConversationSelected" @contact-selected="handleContactSelected" @tab-changed="handleTabChange" />
</template>

<style scoped>
.center-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Use 100vh to center in the full viewport height */
}
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

</style>
