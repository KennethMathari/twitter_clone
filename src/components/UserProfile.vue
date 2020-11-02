<template>
  <div class="user-profile">
    <div class="user-profile__sidebar">
      <div class="user-profile__user-panel">
        <h1 class="user-profile__username">{{user.username}}</h1>
        <div class="user-profile__admin-badge" v-if ="user.isAdmin">
          Admin
        </div>
        <div class="user-profile__follower-count">
          <strong>Followers: </strong>{{followers}}
        </div>
      </div>
      <TweetPanel @add-tweet="addTweet"/>
    </div>
    <div class="user-profile__tweets-wrapper">
      <TweetItem v-for="tweet in user.tweets" :key="tweet.id" :username="user.username" :tweet="tweet"/>
    </div>
  </div>
</template>

<script>
import TweetItem from './TweetItem';
import TweetPanel from './TweetPanel';


export default {
  name: "UserProfile",
  components:{TweetPanel,TweetItem},
  data() {
    return {
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
    };
  },
  
  methods:{
    addTweet(tweet){
      this.user.tweets.unshift({id:this.user.tweets.length + 1,content:tweet});
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
