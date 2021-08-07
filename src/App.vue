<template>
  <div class="music-player-container" :class="{ 'is-playing': isPlaying }">
    <div class="music-player">
      <div class="player-content-container">
        <h1 class="artist-name">{{ currentMusic.name }}</h1>
        <!--  /.track-title -->
        <h3 class="song-title">{{ currentMusic.title }}</h3>
        <!--  /.song-title -->
        <div class="music-player-controls">
          <div class="control-back" @click="prev"></div>
          <!--  /.control-back -->
          <div class="control-play" @click="play"></div>
          <!--  /.control-play -->
          <div class="control-forwards" @click="next"></div>
          <!--  /.control-forwards -->
        </div>
        <!--  /.music-player-controls -->
      </div>
      <!--  /.player-content-container -->
    </div>
    <!--  /.music-player -->

    <div class="album">
      <div
        class="album-art"
        :style="`background: #fff url(${currentMusic.photo}) center/cover no-repeat;`"
      ></div>
      <div
        class="vinyl"
        :style="`background-image: url('https://s3-us-west-2.amazonaws.com/s.cdpn.io/83141/vinyl.png'), url(${currentMusic.photo});`"
      ></div>
      <!--  /.vinyl -->
    </div>
    <!--  /.album-art -->
  </div>
  <!--  /.music-player -->
</template>
<script>
import { ref } from "vue";

import musics from "./musics.js";

export default {
  name: "App",

  setup() {
    const currentMusic = ref(null);
    const isPlaying = ref(false);
    const audio = ref(null);
    const currentMusicIndex = ref(0)

    currentMusic.value = musics[0];
    audio.value = new Audio();
    audio.value.src = currentMusic.value.source;

    audio.value.onloadedmetadata = () => {
        
    }

    const play = () => {
      if (!isPlaying.value) {
        audio.value.play();
        isPlaying.value = true;
      } else {
        audio.value.pause();
        isPlaying.value = false;
      }
    };

    const prev = () => {
      if(isPlaying.value){
          play();
      }
    if(currentMusicIndex.value == 0){
          currentMusicIndex.value = musics.length - 1;

    } else{
        currentMusicIndex.value -= 1;
    }

          //audio.value.pause();
          currentMusic.value = musics[currentMusicIndex.value];
          audio.value.src = currentMusic.value.source;
          play();
      
    };

    const next = () => {
      if(isPlaying.value){
          play();
      }
    if(currentMusicIndex.value < musics.length-1){
          currentMusicIndex.value +=1;

    } else{
        currentMusicIndex.value = 0;
    }

          //audio.value.pause();
          currentMusic.value = musics[currentMusicIndex.value];
          audio.value.src = currentMusic.value.source;
          play();
      
    };

    console.log(currentMusic);

    return {
      currentMusic,
      isPlaying,
      play,
      audio,
      next,
      currentMusicIndex,
      prev,
    };
  },
};
</script>
