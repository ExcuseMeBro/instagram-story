<template>
  <div class="demo-app">
    <div class="demo-stories">
      <a
          v-for="(userStories, userStoriesIndex) in storiesData"
          :key="userStoriesIndex"
          href="#"
          @click="() => openUserStories(userStoriesIndex)"
      >
        <span class="demo-stories-avatar">
          <img :src="userStories.user.avatar" alt="" />
        </span>
        <span class="demo-stories-name">{{ userStories.user.name }}</span>
      </a>
    </div>
    <hr>
    <Wave/>
  </div>

  <!-- Stories Slider -->
  <StoriesSlider
      :swiper="Swiper"
      :effect-cube="EffectCube"
      :enabled="false"
      :autoplay-duration="5000"
      @slidesIndexesChange="onSlidesIndexesChange"
      @storiesSlider="onStoriesSlider"
  >
    <Stories
        v-for="(userStories, userStoriesIndex) in storiesData"
        :key="userStoriesIndex"
    >
      <Story
          v-for="(story, storyIndex) in userStories.stories"
          :key="storyIndex"
          user-link="#"
          :name="userStories.user.name"
          :date="story.date"
          close-button
          @closeButtonClick="onCloseButtonClick"
      >
        <template #avatar>
          <img :src="userStories.user.avatar" />
        </template>
        <video
            v-if="story.video"
            :src="story.video"
            playsinline
            preload="metadata"
        />
        <img v-else :src="story.image" />
      </Story>
    </Stories>
  </StoriesSlider>
</template>
<script>
import { onMounted } from 'vue';
import Swiper from 'swiper';
import { EffectCube } from 'swiper/modules';

import { StoriesSlider, Stories, Story } from './Story/stories-slider-vue';
import Wave from './components/Wave.vue'

import './Story/stories-slider.css';

import Post from './components/Post.vue';
import './style.css';

export default {
  components: {
    StoriesSlider,
    Stories,
    Story,
    Post,
    Wave
  },
  setup() {
    let storiesSlider = null;

    const openUserStories = (userIndex) => {
      // add "in" class (used in demo for animated appearance)
      storiesSlider.el.classList.add('stories-slider-in');
      // enable slider (as we passed enabled: false initially)
      storiesSlider.enable();
      // slide to specific user's stories
      storiesSlider.slideTo(userIndex, 0);
    };

    const onCloseButtonClick = () => {
      // disable slider as we don't need it autoplay stories while it is hidden
      storiesSlider.disable();
      // add "out" class (used in demo for animated disappearance)
      storiesSlider.el.classList.add('stories-slider-out');
    };

    onMounted(() => {
      if (storiesSlider) {
        // when slider became hidden we need to remove "in" and "out" class to return it initial state
        storiesSlider.el.addEventListener('animationend', () => {
          if (storiesSlider.el.classList.contains('stories-slider-out')) {
            storiesSlider.el.classList.remove('stories-slider-in');
            storiesSlider.el.classList.remove('stories-slider-out');
          }
        });
      }
    });

    // stories data
    const storiesData = [
      {
        user: {
          avatar: 'avatars/mark.jpg',
          name: 'mark_johnson',
        },
        stories: [
          {
            date: '21h',
            image: 'images/food-1.jpg',
          },
          {
            date: '12h',
            image: 'images/food-2.jpg',
          },
          {
            date: '2h',
            video: 'video.mp4',
          },
          {
            date: '30m',
            image: 'images/food-3.jpg',
          },
          {
            date: '15m',
            image: 'images/food-4.jpg',
          },
        ],
      },
      {
        user: {
          avatar: 'avatars/malenia.jpg',
          name: 'malenia',
        },
        stories: [
          {
            date: '5h',
            image: 'images/elden-1.jpg',
          },
          {
            date: '45m',
            image: 'images/elden-2.jpg',
          },
          {
            date: '5m',
            image: 'images/elden-3.jpg',
          },
        ],
      },
      {
        user: {
          avatar: 'avatars/john.jpg',
          name: 'john1986',
        },
        stories: [
          {
            date: '15h',
            image: 'images/vacation-1.jpg',
          },
          {
            date: '10h',
            image: 'images/vacation-2.jpg',
          },
          {
            date: '10h',
            image: 'images/vacation-3.jpg',
          },
          {
            date: '8h',
            image: 'images/vacation-4.jpg',
          },
          {
            date: '47m',
            image: 'images/vacation-5.jpg',
          },
        ],
      },
    ];

    const onStoriesSlider = (instance) => {
      storiesSlider = instance;
    };

    const onSlidesIndexesChange = (mainIndex, subIndex) => {
      console.log({ mainIndex, subIndex });
    };
    return {
      Swiper,
      EffectCube,
      openUserStories,
      onCloseButtonClick,
      storiesData,
      onStoriesSlider,
      onSlidesIndexesChange,
    };
  },
};
</script>
