<template>
  <div class="user-profile">
    <div class="user-profile__sidebar">
      <div class="user-profile__user-panel">
        <h1 class="user-profile__username">@{{state.user.username}}</h1>
        <div class="user-profile__admin-badge" v-if ="state.user.isAdmin">
          Admin
        </div>
        <div class="user-profile__follower-count">
          <strong>Followers: </strong>{{state.followers}}
        </div>
      </div>
      <TweetPanel @add-tweet="addTweet"/>
    </div>
    <div class="user-profile__tweets-wrapper">
      <TweetItem v-for="tweet in state.user.tweets" :key="tweet.id" :username="state.user.username" :tweet="tweet"/>
    </div>
  </div>
</template>

<script>
import {reactive} from 'vue';
import TweetItem from './TweetItem';
import TweetPanel from './TweetPanel';


export default {
  name: "UserProfile",
  components:{TweetPanel,TweetItem},

  setup(){
     const state=reactive({
      followers: 0,
      user: {
        id: 1,
        username: "Kenneth_Mathari",
        firstName: "Kenneth",
        lastName: "Mathari",
        email: "mathari580@gmail.com",
        isAdmin: true,
        tweets:[
            {id:1,content:"Kenneth Ndung'u is a pro software engineer"},
            {id:2,content:"He's the Lead Software Engineer at Code & Butter Studios"}
            ]
      },
     })

     function addTweet(tweet){
          state.user.tweets.unshift({id:state.user.tweets.length + 1,content:tweet});
     }

     function followUser(){
          return state.followers++;
     }

     return {
       state,
       addTweet,
       followUser
     }
  },
  
  //runs when the component is loaded for the first time
  mounted(){
    this.followUser();
  }
};
</script>

<style lang="scss" scoped>
.user-profile{
    display: grid;
    grid-template-columns: 1fr 3fr;
    width:100%;
    padding: 50px 5%;

    .user-profile__user-panel{
    display: flex;
    flex-direction:column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;

    h1{
    margin: 0;
    }

    .user-profile__admin-badge{
    background: rebeccapurple;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
      }

      .user-profile__new-tweet{
    display: flex;
    flex-direction: column;
    border-top: 1px solid #DFE3E8;
    padding-top: 20px;

    &.--exceeded{
      color: red;
      border-color: red;
      
      button{
        background-color: red;
        border: none;
        color: white;
      }
    }
    }
  }
}



</style>
