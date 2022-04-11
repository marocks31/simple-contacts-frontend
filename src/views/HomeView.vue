<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Contacts",
      contacts: [],
      newContactParams: {},
      currentContact: {},
      editContactParams: {},
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get("/contacts").then((response) => {
        console.log("Contacts Index", response);
        this.contacts = response.data;
      });
    },
    createContact: function () {
      axios.post("/contacts", this.newContactParams).then((response) => {
        console.log("Contacts Create", response);
        this.contacts.push(response.data);
        this.newContactParams = {};
      });
    },
    showContact: function (contact) {
      this.currentContact = contact;
      document.querySelector("#contact-details").showModal();
    },
    updateContact: function (contact) {
      axios.patch("/contacts/" + contact.id, this.editContactParams).then((response) => {
        console.log("Contact Update", response);
        this.currentContact = {};
      });
    },
    destroyContact: function (contact) {
      axios.delete("/contacts/" + contact.id).then((response) => {
        console.log("Contacts Destroy", response);
        var index = this.contacts.indexOf(contact);
        this.contacts.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New Contact</h1>
    <p>First Name:</p>
    <input type="text" v-model="newContactParams.first_name" />
    <p>Last Name</p>
    <input type="text" v-model="newContactParams.last_name" />
    <p>Email</p>
    <input type="text" v-model="newContactParams.email" />
    <p>Phone Number</p>
    <input type="text" v-model="newContactParams.phone_number" />
    <p>Image</p>
    <input type="text" v-model="newContactParams.image" />
    <button v-on:click="createContact()">Create a New Contact</button>

    <h1>{{ message }}</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <h2>
        Name: {{ contact.last_name }} , {{ contact.first_name }}, Email: {{ contact.email }}, Phone number:
        {{ contact.phone_number }}
      </h2>
      <img v-bind:src="contact.image" v-bind:alt="contact.name" />
      <button v-on:click="showContact(contact)">More Info</button>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <h1>Contact Info</h1>
        <p>First Name {{ currentContact.first_name }}</p>
        <p>Last Name {{ currentContact.last_name }}</p>
        <p>Email {{ currentContact.email }}</p>
        <p>Phone Number {{ currentContact.phone_number }}</p>
        <p>Image {{ currentContact.image }}</p>
        <button>close</button>
        <p>
          First Name
          <input type="text" v-model="editContactParams.first_name" />
        </p>
        <p>
          Last Name
          <input type="text" v-model="editContactParams.last_name" />
        </p>
        <p>
          Email
          <input type="text" v-model="editContactParams.email" />
        </p>
        <p>
          Phone Number
          <input type="text" v-model="editContactParams.phone_number" />
        </p>
        <p>
          Image
          <input type="text" v-model="editContactParams.image" />
        </p>
        <button v-on:click="destroyContact(currentContact)">Delete Contact</button>
        <button v-on:click="updateContact(currentContact)">Update Contact</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
