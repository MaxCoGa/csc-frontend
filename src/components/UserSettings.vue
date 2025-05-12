<template>
  <div class="user-settings">
    <h2>User Settings</h2>
    <button @click="onHideSettings">Back to Chat</button>
    <p>Placeholder for user settings options.</p>
    <ul>
      <li>Change Profile Picture</li>
      <li>Update Password</li>
      <li>Notification Settings</li>
      <li>Privacy Settings</li>
      <li>Logout</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'UserSettings',
  props: {
    onHideSettings: Function
  }
  // Component logic will go here later
}
</script>

<style scoped>
.user-settings {
 display: flex;
 flex-direction: column;
  padding: 20px; /* Add some padding */
  text-align: center;
  color: #cccccc; /* Light gray text for readability */
  height: 100%; /* Take full height */
}


h2 {
  color: #eeeeee; /* Slightly lighter gray for headings */
}

ul {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}

li {
  background-color: #333333; /* Dark gray background for list items */
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 5px;
}
</style>