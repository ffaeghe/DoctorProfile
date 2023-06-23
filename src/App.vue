<template>
  <div id="app">
    <p>Profiles List</p>
    <div class="section">
      <div class="flex-row">
        <label class="label" for="filter">Find profile:</label>
        <input class="input" v-model="searchTerm" placeholder="Search Doctor name">
      </div>
      <div class="buttons">
        <button @click="sortAsc">▲</button>
        <button @click="sortDesc">▼</button>
      </div>
      <ProfileCard
        v-for="(profile, index) in filteredProfiles"
        :key="index"
        :profile="profile"
        class="profile"
        :likedProfiles="likedProfiles"
      />
    </div>

    <div class="section">
      <p class="header">Add new profile:</p>
      <div >
        <div class="flex-row">
          <label class="label">Name:</label>
          <input
            type="text"
            class="input"
            v-model="newProfile.name"
            v-pattern="namePattern"
            :class="{ 'is-invalid': !nameValid }"
          >
        </div>
        <p v-if="!nameValid" class="invalid-feedback">
          Name can only contain English alphabet and space.
        </p> 
      </div>
      <div >
        <div class="flex-row">
          <label class="label" for="filter">Email:</label>
          <input
            type="email"
            class="input"
            v-model="newProfile.email"
            v-pattern="emailPattern"
            :class="{ 'is-invalid': !emailValid }"
          >
        </div>
        <p
          v-if="!emailValid"
          class="invalid-feedback"
        >
          Please enter a valid email address.
        </p>
      </div>
      <div>
        <label class="label">Specialisation:</label>
        <div
          v-for="specialisation in specialisations"
          :key="specialisation"
          class="specItem"
        >
        <input
          type="checkbox"
          :value="specialisation"
          v-model="newProfile.specialisations"
          class="specInput"
        />
          {{ specialisation }}
        </div>
      </div>
      <button @click="addProfile">Add</button>
    </div>
  </div>
</template>

<script>
import ProfileCard from "./components/ProfileCard";

export default {
  name: "App",

  components: {
    ProfileCard
  },

  data() {
    return {
      profiles: [
        {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 28
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53
        }
      ],
      newProfile: {
        name: '',
        email: '',
        description: '',
        specialisations: []
      },
      searchTerm: null,
      specialisations: ['Surgeon', 'Radiologist', 'Cardiologist', 'Psychiatrist', 'Dermatologist'],
      likedProfiles: [],
      namePattern: /^[a-zA-Z\s]*$/,
      emailPattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    };
  },
  computed: {
    nameValid() {
      return this.namePattern.test(this.newProfile.name);
    },
    emailValid() {
      return this.emailPattern.test(this.newProfile.email);
    },
    filteredProfiles() {
      if (this.searchTerm) {
        return this.profiles.filter(profile => {
          return this.searchTerm
            .toLowerCase()
            .split(" ")
            .every(doctor => profile.name.toLowerCase().includes(doctor));
        });
      } else {
        return this.profiles;
      }
    }
  },
  methods: {
   sortAsc() {
     this.profiles.sort((a, b) => a.likes - b.likes);
     this.comments.sort((a, b) => {
       const aIndex = this.profiles.findIndex(p => p.id === a.profileId);
       const bIndex = this.profiles.findIndex(p => p.id === b.profileId);
       return aIndex - bIndex;
     });
   },
   sortDesc() {
     this.profiles.sort((a, b) => b.likes - a.likes);
     this.comments.sort((a, b) => {
       const aIndex = this.profiles.findIndex(p => p.id === a.profileId);
       const bIndex = this.profiles.findIndex(p => p.id === b.profileId);
       return aIndex - bIndex;
     });
   },
   addProfile() {
     const newId = Math.max(...this.profiles.map(p => p.id)) + 1;
     const newProfile = {
       id: newId,
       name: this.newProfile.name,
       email: this.newProfile.email,
       description: this.newProfile.description,
       specialisations: this.newProfile.specialisations,
       likes: 0
     };
     this.profiles.push(newProfile);
     this.newProfile = {
       name: '',
       email: '',
       description: '',
       specialisations: []
     };
   },
   likeProfile(profile) {
    if (this.likedProfiles.includes(profile.id)) {
      profile.likes--;
      this.likedProfiles = this.likedProfiles.filter(id => id !== profile.id);
    } else {
      profile.likes++;
      this.likedProfiles.push(profile.id);
      if (this.likedProfiles.includes(profile.id + 1)) {
        const otherProfile = this.profiles.find(p => p.id === profile.id + 1);
        if (otherProfile) {
          otherProfile.likes--;
          this.likedProfiles = this.likedProfiles.filter(id => id !== otherProfile.id);
        }
      }
    }
  },
  dislikeProfile(profile) {
    if (this.likedProfiles.includes(profile.id)) {
      profile.likes--;
      this.likedProfiles = this.likedProfiles.filter(id => id !== profile.id);
    } else {
      profile.likes--;
      this.likedProfiles.push(profile.id);
      if (this.likedProfiles.includes(profile.id + 1)) {
        const otherProfile = this.profiles.find(p => p.id === profile.id + 1);
        if (otherProfile) {
          otherProfile.likes++;
          this.likedProfiles.push(otherProfile.id);
        }
      }
    }
  }
},
};
</script>

<style src="./App.css"></style>
