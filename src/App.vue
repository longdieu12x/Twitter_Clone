<template>
    <div id="app" class="flex container h-screen w-full">
      <div class="lg:w-1/5 border-r border-lighter px-2 lg:px-6 py-2 flex flex-col justify-between">
        <div class="">
          <button class="h-12 w-12 hover:bg-lightblue text-3xl rounded-full text-blue">
            <i class="fab fa-twitter"></i>
          </button>
          <div>
            <button v-for="tab in state.tabs" @click="id = tab.id" :class="`focus:outline-none hover:text-blue flex items-center py-2 px-4 hover:bg-lightblue rounded-full mr-auto mb-3 ${ id === tab.id ? 'text-blue' : ''}`">
              <i :class="`${ tab.icon } text-2xl mr-4 text-left`"></i>
              <p class="text-lg font-semibold text-left hidden lg:block"> {{ tab.title }} </p>
            </button>
          </div>
          <button class="text-white bg-blue rounded-full font-semibold w-12 h-12 lg:w-full p-3 focus:outline-none hover:bg-darkblue">
            <p class="hidden lg:block">Tweet</p>
            <i class="fas fa-plus lg:hidden"></i>
          </button>
        </div>
        <div class="w-full relative">
          <button @click="state.dropdown = true" class="flex items-center w-full hover:bg-lightblue rounded-full p-2">
            <img src="img/profile.png" alt="profile" class="w-10 h-10 rounded-full border border-lighter">
            <div class="hidden lg:block ml-4">
              <p class="text-sm font-bold leading-light">Steph Dietz</p>
              <p class="text-sm leading-light"> @SaaSyEth </p>
            </div>
            <i class="hidden lg:block fas fa-angle-down ml-auto text-lg"></i>
          </button>
          <div v-if="state.dropdown == true" class="bg-white absolute bottom-0 left-0 w-64 rounded-lg shadow-md border-lightest mb-16">
            <button @click="state.dropdown = false" class="flex items-center w-full hover:bg-lightest rounded-full p-2">
              <img src="img/profile.png" alt="profile" class="w-10 h-10 rounded-full border border-lighter">
              <div class="ml-4">
                <p class="text-sm font-bold leading-light">Steph Dietz</p>
                <p class="text-sm leading-light"> @SaaSyEth </p>
              </div>
              <i class="fas fa-check ml-auto text-blue"></i>
            </button>
            <button class="focus:outline-none w-full p-3 text-sm text-left hover:bg-lightest border-t border-lighter">
              Add an existing account
            </button>
            <button class="focus:outline-none w-full p-3 text-sm text-left hover:bg-lightest border-t border-lighter">
              Log out @SaaSyEth
            </button>
          </div>
        </div>
      </div> 
      <!-- tweets -->
      <div class="w-full md:w-1/2 h-full overflow-y-scroll">
        <div class="px-5 py-3 border-b border-lighter flex items-center justify-between">
          <h1 class="text-xl font-bold">Home</h1>
          <i class="far fa-star text-xl text-blue"></i>
        </div>
        <div class="px-5 py-3 border-b-8 border-lighter flex">
          <div class="">
            <img src="img/profile.png" alt="profile" class="w-12 h-12 rounded-full border border-lighter">
          </div>
          <form v-on:submit.prevent="addNewTweet" action="" class="w-full px-4 relative">
            <textarea v-model="state.tweet.content" placeholder="What's up" class="mt-3 pb-3 w-full focus:outline-none"></textarea>
            <div class="flex items-center">
              <i class="text-lg text-blue mr-6 far fa-image "></i>
              <i class="text-lg text-blue mr-6 far fa-film "></i>
              <i class="text-lg text-blue mr-6 far fa-chart-bar "></i>
              <i class="text-lg text-blue mr-6 far fa-smile "></i>
            </div>
            <button type="submit" class="rounded-full absolute bottom-0 right-0 h-10 px-4 text-white font-semibold bg-blue hover:bg-darkblue focus:outline-none">
              Tweet
            </button>
          </form>
        </div>
        <div class="flex flex-col-reverse">
          <div v-for="tweet in state.tweets" class="w-full p-4 border-b hover:bg-lighter flex">
            <div class="flex-none mr-4">
              <img src="img/profile.png" class="h-12 w-12 rounded-full flex-none"/>
            </div>
            <div class="w-full">
              <div class="flex items-center w-full">
                <p class="font-semibold"> Steph Dietz </p>
                <p class="text-sm text-dark ml-2"> @SaaSyEth </p>
                <p class="text-sm text-dark ml-2"> 1 sec </p>
                <i class="fas fa-angle-down text-dark ml-auto"></i>
              </div>
              <p class="py-2">
                {{ tweet.content }}
              </p>
              <div class="flex items-center justify-between w-full">
                <div class="flex items-center text-sm text-dark">
                  <i class="far fa-comment mr-3"></i>
                  <p> 0 </p>
                </div>
                <div class="flex items-center text-sm text-dark">
                  <i class="fas fa-retweet mr-3"></i>
                  <p> 0 </p>
                </div>
                <div class="flex items-center text-sm text-dark">
                  <i class="fas fa-heart mr-3"></i>
                  <p> 1 </p>
                </div>
                <div class="flex items-center text-sm text-dark">
                  <i class="fas fa-share-square mr-3"></i>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-for="follow in state.following" class="w-full p-4 border-b hover:bg-lighter flex">
          <div class="flex-none mr-4">
            <img :src="`img/${follow.src}`" class="h-12 w-12 rounded-full flex-none">
          </div>
          <div class="w-full">
            <div class="flex items-center w-full">
              <p class="font-semibold">{{follow.name}}</p>
              <p class="text-sm text-dark ml-2">{{follow.handle}}</p>
              <p class="text-sm text-dark ml-2">{{follow.time}}</p>
              <i class="fas fa-angle-downtext-dark ml-auto"></i>
            </div>
            <p class="py-2">
              {{ follow.tweet }}
            </p>
            <div class="flex items-center justify-between w-full">
              <div class="flex items-center text-sm text-dark">
                <i class="far fa-comment mr-3"></i>
                <p>{{follow.comments}}</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="far fa-retweet mr-3"></i>
                <p>{{follow.retweets}}</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="far fa-heart mr-3"></i>
                <p>{{follow.likes}}</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="far fa-share-square mr-3"></i>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- trending -->
      <div class="hidden md:block w-1/3 h-full border-l border-lighter py-2 px-6  overflow-y-scroll relative">
        <input class="pl-12 text-sm rounded-full w-full p-2 bg-lighter" placeholder="Search Twitter"/>
        <i class="text-light fas fa-search absolute left-0 top-0 mt-4 w-12 h-12 ml-12 text-sm"></i>
        <div class="w-full rounded-lg bg-lightest">
          <div class="flex items-center justify-between p-3">
            <p class="text-lg font-bold">Trends For You</p>
            <i class="fas fa-cog text-lg text-blue"></i>
          </div>
          <button v-for="trend in state.trending" class="w-full flex justify-between hover:bg-lighter p-3 border-t border-lighter">
            <div class="">
              <p class="text-sm text-left leading-tight text-dark">{{trend.top}}</p>
              <p class="font-bold text-left leading-tight">{{trend.title}}</p>
              <p class="text-left leading-tight text-dark">{{trend.bottom}}</p>
            </div>
            <i class="fas fa-angle-down text-lg text-dark"></i>
          </button>
          <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter">
            Show More
          </button>
        </div>
        <div class="w-full rounded-lg bg-lightest my-4">
          <div class="p-3">
            <p class="text-lg font-bold">Who to Follow</p>
          </div>
          <button v-for="friend in state.friends" class="w-full flex hover:bg-lighter p-3 border-t border-lighter">
            <img :src="`img/${ friend.src }`" alt="profile" class="w-10 h-10 rounded-full border border-lighter">
            <div class="hidden lg:block ml-4">
              <p class="text-sm font-bold leading-light">{{friend.name}}</p>
              <p class="text-sm leading-light"> {{friend.handle}} </p>
            </div>
            <button class="ml-auto text-sm text-blue py-1 px-4 rounded-full border-2 border-blue">
              Follow
            </button>
          </button>
          <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter">
            Show More
          </button>
        </div>
      </div>
    </div>
</template>

<script>
import {reactive, ref} from 'vue'
export default{
  data(){
    return {
      id: 'home',
    }
  },
  setup(){
    // Declare state
    const state = reactive({
      tabs: [
        {icon: 'fas fa-home', title: 'Home', id:'home'},
        {icon: 'fas fa-hashtag', title: 'Explore', id: 'explore'},
        {icon: 'far fa-bell', title: 'Notifications', id: 'notifications'},
        {icon: 'far fa-envelope', title: 'Messages', id: 'messages'},
        {icon: 'far fa-bookmark', title: 'Bookmarks', id: 'bookmarks'},
        {icon: 'fas fa-clipboard-list', title: 'Lists', id: 'lists'},
        {icon: 'far fa-user', title: 'Profile', id: 'profile'},
        {icon: 'fas fa-ellipsis-h', title: 'More', id: 'more'}
      ],
      trending: [
        {top: 'Trending in TX', title: 'Gigi', bottom: 'Trending with: Rip Gigi'},
        {top: 'Music', title: 'We Won', bottom: '135K Tweets'},
        {top: 'Pop', title: 'Blue Ivy', bottom: '40k tweets'},
        {top: 'Trending in US', title: 'Denim Day', bottom: '40k tweets'},
        {top: 'Trending', title: 'When Beyonce', bottom: '25.4k tweets'},
      ],
      friends: [
        {src: 'elon.jpg', name: 'Elon Musk', handle: '@teslaBoy'},
        {src: 'monk.jpg', name: 'Adrian Monk', handle: '@detective:)'},
        {src: 'kevin.jpg', name: 'Kevin Hart', handle: '@miniRock'}
      ],
      following: [
        {src: 'elon.jpg', name: 'Elon Musk', handle: '@teslaBoy', time: '20 min', tweet: 'Should I just quarantine on mars??', comments: '1,000', retweets: '550', like: '1,000,003'},
        {src: 'kevin.jpg', name: 'Kevin Hart', handle: '@miniRock', time: '55 min', tweet: 'Should me and the rock do another sub-par movie together????', comments: '2,030', retweets: '50', like: '20,003'},
        {src: 'elon.jpg', name: 'Elon Musk', handle: '@teslaBoy', time: '1.4 hr', tweet: 'Haha just made a flame thrower. Shld I sell them?', comments: '100,000', retweets: '1,000,002', like: '5,000,003'},
        {src: 'elon.jpg', name: 'Elon Musk', handle: '@teslaBoy', time: '1.4 hr', tweet: 'Just did something crazyyyyyyy', comments: '100,500', retweets: '1,000,032', like: '5,000,103'}
      ],
      tweets: [
        {content: 'It is so nice outside!'}
      ],
      tweet: {content: ''},
      dropdown: false,
    })
    // Declare function
    function addNewTweet(){
      let newTweet = {
        content: state.tweet.content
      };
      state.tweets.push(newTweet);
      state.tweet.content = '';
    }

    return {
      state,
      addNewTweet,
    }
  }
}
</script>

<style>
</style>
