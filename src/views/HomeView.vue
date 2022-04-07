<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Your Contacts:",
      contacts: [],
      newContact: {},
      currentContact: {},
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts() {
      axios.get("/contacts").then((response) => {
        console.log(response.data);
        this.contacts = response.data;
      });
    },
    createContacts() {
      axios.post("/contacts", this.newContact).then((response) => {
        console.log(response.data);
        this.contacts.push(response.data);
      });
    },
    showContacts(contact) {
      axios.get(`/contacts/${contact.id}`).then((response) => {
        console.log(response.data);
        this.currentContact = response.data;
        document.querySelector("#contact-details").showModal();
      });
    },
    updateContacts() {
      axios.patch(`/contacts/${this.currentContact.id}`, this.currentContact).then((response) => {
        console.log("Successfully updated!", response);
      });
    },
    deleteContacts(contact) {
      axios.delete(`/contacts/${contact.id}`).then((response) => {
        console.log(response.data);
        var index = this.contacts.indexOf(contact);
        this.contacts.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>
      First Name:
      <input type="text" v-model="newContact.first_name" />
    </p>
    <p>
      Last Name:
      <input type="text" v-model="newContact.last_name" />
    </p>
    <p>
      Phone Number:
      <input type="text" v-model="newContact.phone_number" />
    </p>
    <p>
      Email:
      <input type="text" v-model="newContact.email" />
    </p>
    <button v-on:click="createContacts()">Create New Contact</button>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <h3>{{ contact.first_name }} {{ contact.last_name }}</h3>
      <p>{{ contact.email }} | {{ contact.phone_number }}</p>
      <button v-on:click="showContacts(contact)">More Info</button>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <img v-bind:src="currentContact.image" v-bind:alt="contact" style="max-width: 100px" />
        <p>First Name: {{ currentContact.first_name }}</p>
        <p>Last Name: {{ currentContact.last_name }}</p>
        <p>Email: {{ currentContact.email }}</p>
        <p>Phone Number: {{ currentContact.phone_number }}</p>
        <h3>Update Contact</h3>
        <p>
          Edit Photo:
          <input v-model="currentContact.image" type="text" />
        </p>
        <p>
          Edit First Name:
          <input v-model="currentContact.first_name" type="text" />
        </p>
        <p>
          Edit Last Name:
          <input v-model="currentContact.last_name" type="text" />
        </p>
        <p>
          Edit Email:
          <input v-model="currentContact.email" type="text" />
        </p>
        <p>
          Edit Phone Number:
          <input v-model="currentContact.phone_number" type="text" />
        </p>
        <button v-on:click="deleteContacts(currentContact)">Delete</button>
        <button v-on:click="updateContacts()">Update</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
