<template>
  <div class="user-profile">
      <div class="user-profile__user-panel">
          <h1 class="user-profile__username">@{{ user.username }}</h1>
          <div class="user-profile__admin-badge" v-if="user.isAdmin">
              Admin
          </div>
        <div class="user-profile__followers-count">
            <strong>Followers</strong>{{followers}}
        </div>
    <button @click="followUser">Follow</button>
    <form class="user-profile__new-tweet" @submit.prevent="createNewTweet">
        <label for="newTweet"><strong>New Tweet </strong></label>
        <textarea id="newTweet" rows="4" v-model="newTweetContent"/>
        <div class="user-profile__create-tweet-type">
            <label for="newTweetType"><strong>Type:</strong></label>
            <select id="newTweetType" v-model="selectedTweetType">
                <option :value="option.value" v-for="(option,index) in tweetTypes" :key="index">
                    {{option.name}}
                </option>
            </select>
        </div>
        <button>Tweet</button>
    </form>
  </div>
  <div class="user-profle__tweets-wrapper">
      <TweetItem v-for="tweet in user.tweets" :key="tweet.id" :username="user.username" :tweet="tweet" @favourite="toggleFavourite"/>
  </div>
  </div>
</template>

<script>
import TweetItem from './TweetItem';

export default {
  name: "UserProfile",
  components:{TweetItem},
  data() {
    return {
      newTweetContent:'',
      selectedTweetType:'instant',
      tweetTypes:[
          {value:'draft',name:'Draft'},
          {value:'instant',name:'Instant Tweet'}
      ],
      followers: 0,
      user: {
        id: 1,
        username: "KennethMathari",
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
  watch:{
    followers(newFollowerCount, oldFollowerCount){
      if(oldFollowerCount<newFollowerCount){
        console.log(this.user.username+' '+'has a new follower!');
      }
    }
  },
  computed:{
    fullName(){
      return this.user.firstName+' '+this.user.lastName;
    }
  },
  methods:{
    followUser(){
      this.followers++
    },
    toggleFavourite(id){
        console.log('Favourite tweet'+ id );
    },
    createNewTweet(){
        if(this.newTweetContent && this.selectedTweetType !=='draft'){
            this.user.tweets.unshift({
                id:this.user.tweets.length+1,
                content:this.newTweetContent
            })
            this.newTweetContent="";
        }
    }
  },
  //runs when the component is loaded for the first time
  mounted(){
    this.followUser();
  }
};
</script>

<style>
.user-profile{
    display: grid;
    grid-template-columns: 1fr 3fr;
    width:100%;
    padding: 50px 5%;
}

.user-profile__user-panel{
    display: flex;
    flex-direction:column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
}
.user-profile__admin-badge{
    background: rebeccapurple;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
}
h1{
    margin: 0;
}
.user-profile__new-tweet{
    display: flex;
    flex-direction: column;
    border-top: 1px solid #DFE3E8;
    padding-top: 20px;
}
</style>
