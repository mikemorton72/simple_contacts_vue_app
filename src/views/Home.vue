<template>
  <div class="home">
    <h1>Simple Contacts App</h1>
    <div v-for="contact in contacts" :key="contact.id">
      <p> {{ contact }} </p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      contacts: [],
    };
  },
  created: function () {
    this.contactIndex();
  },
  methods: {
    contactIndex: function () {
      axios.get("contacts").then((response) => {
        this.contacts = response.data;
        console.log(response.data);
      });
    },
  },
};
</script>
