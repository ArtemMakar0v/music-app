<template>
<div id="app">
  <header>
    <h1>My music</h1>
  </header>
  <main>
    <section class="player">
      <h2 class="song-title">
        {{ current.title }} - <span>{{current.artist}}</span>
      </h2>
      <div class="controls">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
    </section>
    <section class="playList">
      <h3>The Playlist</h3>
      <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
        {{song.title}} - {{song.artist}}
      </button>
    </section>
  </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data () {
    return{
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Kalyna',
          artist: 'Boombox',
          src: require('./assets/Boombox_Kalyna.mp3')
        },
        {
          title: 'Ja ne znaju',
          artist: 'Tartak',
          src: require('./assets/tartak_ja_ne_znaju.mp3')
        },
        {
          title: 'Probass',
          artist: 'Hardi',
          src: require('./assets/Probass_Hardi.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods:{
    play (song){
      if (typeof song.src != "undefined"){
        this.current = song;

        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function (){
        this.index++;
        if (this.index > this.songs.length - 1){
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause (){
      this.player.pause();
      this.isPlaying = false;
    },
    next (){
      this.index++;
      if (this.index > this.songs.length - 1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev (){
      this.index--;
      if (this.index < 0 ){
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    // this.player.play();
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: sans-serif;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: antiquewhite;
}
main{
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title{
  color: #212121;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span{
  font-weight: 400;
}
.controls{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}
button{
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover{
  opacity: 0.8;
}
.play{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: aliceblue;
  background-color: crimson;
}
.pause{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: aliceblue;
  background-color: brown;
}

.next, .prev{
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 8px;
  color: aliceblue;
  background-color: burlywood;
}
.playList{
  padding: 0px 30px;
}
.playList h3{
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playList .song{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playList .song:hover{
  color: tomato;
}
.playList .song.playing{
  color: aliceblue;
  background-image: linear-gradient(to right, tomato, burlywood);
}
</style>
