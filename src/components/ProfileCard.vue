<template>
    <div class="profile">
      <div class="card">
        <DoctorIcon class="avatar"/>
        <div class="data">
          <div class="profile-content">
            <div>
              <strong>{{profile.name}}</strong>
              <a :href="profile.email" class="email">{{profile.email}}</a>
            </div>
            <div class="description">{{profile.description}}</div>
          </div>
          <div class="likes">
            <span class="likes-icon">💚</span>
            <span class="likes-value">{{profile.likes}}</span>
              <button class="LikeDis" @click="likeProfile(profile)">
                <ThumsUp />
              </button>
              <button
                class="LikeDis"
                @click="dislikeProfile(profile)"
              >
                <ThumsDown />
              </button>
          </div>
        </div>
      </div>
      <div class="comment">
        <input class="comment-input" placeholder="Write your comment...">
      </div>
    </div>
  </template>
  
  <script>
  import DoctorIcon from "./DoctorIcon";
  import ThumsDown from './ThumsDown.vue';
  import ThumsUp from './ThumsUp.vue';
  
  export default {
    name: "ProfileCard",
    data() {
    return {
      likedProfiles: [],
      };
    },
    components: {
      DoctorIcon,
      ThumsDown,
      ThumsUp
    },
  
    props: {
      profile: {
        type: Object,
        required: true
      },
      ikedProfiles: null
    },
    methods: {
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
    }
  };
  </script>
  
  <style src="./ProfileCard.css"></style>
  