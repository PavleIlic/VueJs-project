<template>
  <div class="user-profile">
    <div class="user-profile_user-panel">
      <h1 class="user-profile_username">@{{ user.username }}</h1>
      <div class="user-profile_admin-badge" v-if="user.isAdmin">
        Admin
      </div>
      <div class="user-profile_follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>
      <form action="" class="user-profile_create-twoot" @submit.prevent="createNewTwoot" :class="{'--exceeded': newTwootCharacterCount > 180}">
        <label for="newTwoot"><strong>New twoot</strong>({{ newTwootCharacterCount }}/180)</label>
        <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>


        <div class="user-profile_create-twoot-type">
          <label for="newTwoot"><strong>Type: </strong></label>
          <select id="newTwootType" v-model="selectedTwootType">
            <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
              {{ option.name }}
            </option>
          </select>
        </div>
        <button>
          Twoot!
        </button>
      </form>
    </div>
    <div class="user-profile_twoots-wrapper">
      <TwootItem
          v-for="twoot in user.twoots"
          :key="twoot.id"
          :username="user.username"
          :twoot="twoot"
          @favourite="toggleFavourite"/>
    </div>
  </div>
</template>

<script>

import TwootItem from "@/components/TwootItem";

export default {
  name: 'UserProfile',
  components: {TwootItem},
  data (){
    return {
      newTwootContent: '',
      selectedTwootType: 'instant',
      twootTypes: [
        { value: 'draft' , name: 'Draft'},
        { value: 'instant' , name: 'Instant twoot!'}
      ],
      followers: 0,
      user: {
        id: 1,
        username: '_PavleIlic',
        firstName: 'Pavle',
        lastName: 'Ilic',
        email: 'pavle.ilic24d@gmail.com',
        isAdmin: true,
        twoots: [
          {id: 1, content: 'The twooter is amazing!'},
          {id: 2, content: "Don't forget to subscribe to the Earth is square!"}
        ]
      }
    }
  },
  watch: {
    followers(newFollowersCount, oldFollowersCount){
      if (oldFollowersCount < newFollowersCount){
        console.log(`${this.user.username} has gained a new follower!`)
      }
    }
  },
  computed: {
    newTwootCharacterCount() {
      return this.newTwootContent.length;
    }
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavourite(id) {
      console.log(`Favourite tweet #${id}`)
    },
    createNewTwoot() {
      if(this.newTwootContent && this.selectedTwootType !== 'draft'){
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        });
        this.newTwootContent = '';
      }
    }
  },
  mounted() {
    this.followUser();
  }
}
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;

  .user-profile_user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: #ffffFF;
    border-radius: 5px;
    border: 1px solid #DFE3E8;

    h1 {
      margin: 0px;
    }

    .user-profile_admin-badge{
      background-color: rebeccapurple;
      color: white;
      border-radius: 5px;
      margin-right: auto;
      padding: 0 10px;
      font-weight: bold;
    }
    .user-profile_create-twoot{
      padding-top: 20px;
      display: flex;
      flex-direction: column;

      &.--exceeded {
        color: red;
        border-color: red;
        button {
          background-color: red;
          color: white;
        }
      }
    }
  }
  .user-profile_twoots-wrapper{
    display: grid;
    grid-gap: 10px;

  }
}

</style>
