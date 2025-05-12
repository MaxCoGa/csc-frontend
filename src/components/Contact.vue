<template>
  <div class="contact-details">
    <button @click="onCloseContact">Back</button>
    <h2>Contact Details: {{contact. contact.name }}</h2>
    <p><strong>Name:</strong> {{ contact.contact.name }}</p>
    <p><strong>Id:</strong> {{ contact.contact.id }}</p>
    <p><strong>Type:</strong> {{ contact.contact.type }}</p>
  </div>
</template>

<script>
export default {
  name: 'Contact',
  props: {
    contact: {
      type: Object,
      required: true,
    },
    onCloseContact: Function, // Add onCloseContact as a prop
  },
  created() {
    console.log('Contact component created. Contact prop:', this.contact);
  },
};
</script>

<style scoped>
.contact-details {
  padding: 20px;
  color: #cccccc;
  background-color: #1e1e1e;
  border-radius: 5px;
  height: 100%; /* Take full height of its container */
  box-sizing: border-box; /* Include padding in height */
  display: flex;
  flex-direction: column;
}

.contact-details h2 {
  color: #cccccc;
  margin-top: 0;
}

.contact-details p {
  margin-bottom: 10px;
}

.contact-details button {
  align-self: flex-start;
  padding: 8px 15px;
  background-color: #555;
  color: #eee;
  border: none;
  border-radius: 3px;
  cursor: pointer;
  margin-bottom: 20px;
}

.contact-details button:hover {
  background-color: #666;
}
</style>