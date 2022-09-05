<template>

<div id="app">
  <header>
    <h1>My Music</h1>
  </header>
  <main>
    <section class="player">
      <h2 class="song-title">
        {{current.title}} -
        <span>{{ current.artist }}</span>
      </h2>
      <div class="controls">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" v-bind:onclick="play">Play</button>
        <button class="pause" v-else v-bind:onclick="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
    </section>

    <section class="playlist">
      <h3>The playlist</h3>
      <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
        {{song.title}} - {{song.artist}}
      </button>
    </section>

  </main>
</div>

</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      current: {
      },
      index: 0,
      isPlaying: false,
      songs: [
      {
          title: 'Don\'t kill the magic',
          artist: 'Magic',
          src: require('./assets/dont-kill-the-magic.mp3')
        },
        {
          title: 'Rude',
          artist: 'Magic',
          src: require('./assets/rude.mp3')
        },
        {
          title: 'Mostrame cómo sos',
          artist: 'No te va gustar',
          src: require('./assets/mostrame-como-sos.mp3')
        },
        {
          title: 'Sugar',
          artist: 'Maroon 5',
          src: require('./assets/sugar.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song){
      if(typeof song.src != 'undefined'){
        this.current = song;
        this.player.src = this.current.src;
      }   
      this.index = this.songs.indexOf(song);
      this.player.play();
      this.player.addEventListener('ended', function(){
        this.next();
      }.bind(this))
      this.isPlaying = true;
    },
    pause (){
      this.player.pause();
      this.isPlaying = false;
    },
    prev(){
      this.index--;
      if(this.index < 0){
        this.index = this.songs.length -1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next(){
      this.index++;
      if(this.index > this.songs.length -1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}

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
  background-color: #212121;
  color: #FFF;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 23px;
}

.song-title {
  color: #212121;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center
}

.song-title span {
  font-weight: 400;
  font-style: italic;
}

.controls {
  display: flex;
  justify-content: center;
  padding: 30px 15px;
  align-items: center;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursos: pointer;
}

.play, .pause{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CC2E5D;
}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #ff5858;
}

.playlist {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
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
  color: #ff5858
}

.playlist .song.playing{
  color: #fff;
  background-image: linear-gradient(to right, #cc2e5d, #ff5858);
}


</style>
