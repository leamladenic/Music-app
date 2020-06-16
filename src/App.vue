<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} -
          <span>{{ current.artist }}</span>
        </h2>
        <div class="controls">
          <button class="prev" @click="prev">
            <i class="fas fa-backward"></i>
          </button>
          <button class="play" v-if="!isPlaying" @click="play">
            <i class="fas fa-play"></i>
          </button>
          <button class="pause" v-else @click="pause">
            <i class="fas fa-pause"></i>
          </button>
          <button class="next" @click="next">
            <i class="fas fa-forward"></i>
          </button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="(song.src == current.src) ? 'song playing' : 'song'"
        >{{ song.title }} - {{ song.artist }}</button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "How Does A Moment Last Forever",
          artist: "Celine Dion",
          src: require("./assets/celine.mp3")
        },
        {
          title: "Can You Feel the Love Tonight",
          artist: "Lion King",
          src: require("./assets/lion-king.mp3")
        },
        {
          title: "Remember",
          artist: "Hans Zimmer",
          src: require("./assets/remember.mp3")
        },
        {
          title: "Prince Ali",
          artist: "Will Smith",
          src: require("./assets/alladin.mp3")
        }
      ],
      player: new Audio()
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function() {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #703667;
  color: #fff;
  font-size: 18px;
}
#app {
  background-image: url("./assets/music.jpg");
  background-size: cover;
  background-position: bottom;
}
main {
  min-height: 100vh;
  padding: 25 px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.15),
    rgba(0, 0, 0, 0.5)
  );
}

.song-title {
  color: #53565a;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
  padding: 30px;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.7;
}
.play,
.pause {
  font-size: 25px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background-color: #d12c89;
  border-radius: 0px 19px 0px 19px;
}
.next,
.prev {
  font-size: 20px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #fff;
  background-color: #ffa8d9;
}
.next {
  border-radius: 0px 25px 25px 0px;
}
.prev {
  border-radius: 25px 0px 0px 25px;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #bf3684;
  font-size: 38px;
  font-weight: 700;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  color: #d12c89;
}
.playlist .song.playing {
  color: #fff;
  border-radius: 0px 19px 0px 19px;
  background-image: linear-gradient(to bottom right, #d12c89, #ffa8d9);
}
</style>