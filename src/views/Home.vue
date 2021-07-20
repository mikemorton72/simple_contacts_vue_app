<template>
  <div class="home">
    <h1>Simple Contacts App</h1>
    <h3>Create a new Contact</h3>
    <p>First Name: <input type="text" v-model="newContact.first_name"></input></p>
    <p>Last Name: <input type="text" v-model="newContact.last_name"></input></p>
    <p>Email: <input type="text" v-model="newContact.email"></input></p>
    <p>Phone Number: <input type="text" v-model="newContact.phone_number"></input></p>
    <p>Image URL: <input type="text" v-model="newContact.image"></input></p>
    <div v-if="errorsPresent">
      <p v-for="error in errors">{{ error }}</p>
    </div>
    <button v-on:click="contactCreate()">Add Contact</button>
    <hr />
    <div v-for="contact in contacts" :key="contact.id">
      <img v-bind:src="contact.image" />
      <p> {{ contact.first_name }}  {{ contact.last_name }} </p>
      <p> email: {{ contact.email }}</p>
      <p>phone: {{ contact.phone_number }}</p>
      <hr />
    </div>
  </div>
</template>

<style>
img {
  height: 250px;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      contacts: [],
      newContact: {},
      errors: {},
      errorsPresent: false,
    };
  },
  created: function () {
    this.contactIndex();
  },
  methods: {
    contactIndex: function () {
      axios.get("/contacts").then((response) => {
        this.contacts = response.data;
        // console.log(response.data);
      });
    },
    contactCreate: function () {
      axios
        .post("/contacts", this.newContact)
        .then((response) => {
          console.log(response.data);
          this.contacts.push(this.newContact);
          this.newContact = {};
          this.errorsPresent = false;
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.errorsPresent = true;
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
