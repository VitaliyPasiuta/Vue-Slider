<template>
  <div id="app">
    <input type="button" value="Start" @click="load" />
    <div v-if="photoStore.length > 0" class="container">
      <img
        src="./img/Arrow-rigth.png"
        alt=""
        class="arrow-left"
        @click="prevPhoto"
      />
      <img
        src="./img/Arrow-rigth.png"
        alt=""
        class="arrow-rigth"
        @click="nextPhoto"
      />
      <div class="main_photo">
        <img :src="activePhoto" :alt="activePhoto" />
      </div>
    </div>
    <div v-if="photoStore.length > 0" class="container-bar">
      <div class="img_bar">
        <div v-for="(photo, inx) in photoStore" :key="inx">
          <img :src="photo" :alt="photo + inx" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//import axios from "axios";
export default {
  name: "App",
  components: {},
  data() {
    return {
      activePhoto: "",
      activePhotoIndex: 0,
      photoStore: [],
    };
  },
  methods: {
    load() {
      if (this.photoStore.length) {
        //Останавливает загрузку лишних изначальных фоток
        return;
      }
      fetch(
        "https://www.flickr.com/services/rest/?method=flickr.galleries.getPhotos&api_key=97602d1e270e82a5f42dee2ad4418907&gallery_id=66911286-72157647277042064&format=json&nojsoncallback=1"
      )
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          data.photos.photo.map((e) => {
            let photoData = "";
            photoData = `https://farm${e.farm}.staticflickr.com/${e.server}/${e.id}_${e.secret}.jpg`;
            this.photoStore.push(photoData);
          });
          console.log(this.photoStore);
          this.changeActivePhoto(this.activePhotoIndex);
        });
    },
    nextPhoto() {
      if (this.activePhotoIndex >= this.photoStore.length - 1) {
        return null;
      }
      this.activePhotoIndex += 1;
      this.changeActivePhoto(this.activePhotoIndex);
    },
    prevPhoto() {
      if (this.activePhotoIndex <= 0) {
        return null;
      }
      this.activePhotoIndex -= 1;
      this.changeActivePhoto(this.activePhotoIndex);
    },
    changeActivePhoto(inx) {
      this.activePhoto = this.photoStore[inx];
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.img_bar {
  display: flex;
  justify-content: space-around;
  align-items: center;
  height: 260px;
}

.img_bar img {
  width: 90%;
}

.main_photo img {
  height: 60vh;
}

.container {
  padding: 10% 10% 0;
  position: relative;
}

.container-bar {
  padding: 0 10%;
}

.arrow-rigth {
  position: absolute;
  top: 50%;
  right: 20%;
}

.arrow-left {
  position: absolute;
  top: 50%;
  left: 20%;
  transform: rotate(180deg);
}
</style>
