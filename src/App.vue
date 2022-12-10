<template>
    <div id="app">
    <div class="app-phone">
      <div class="phone-header">
        <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/1211695/vue_gram_logo_cp.png" />
        <v-btn class="cancel-cta"
           v-if="step === 2 || step === 3" 
           @click="goToHome">
            Cancel
      </v-btn>
        <v-btn class="next-cta"
           v-if="step === 2"
          @click="step++">
            Next
    </v-btn>
        <v-btn class="next-cta"
           v-if="step === 3"
          @click="sharePost">
            Share
  </v-btn>
      </div>
      <phone-body
        v-bind:step="step"
        v-bind:posts="posts"
        v-bind:image="image"
        v-model="caption"
      />
      <div class="phone-footer">
       <div class="home-cta" @click="goToHome">
        <v-icon>mdi-home</v-icon>
       </div>
       <div class="upload-cta">
          <input type="file"
            name="file"
            id="file"
            class="inputfile"
            @change="uploadImage"
            :disabled="step !== 1"
          />
          <label for="file">
            <v-icon>mdi-camera</v-icon>
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PhoneBody from "./components/PhoneBody";
import posts from "./data/posts";

export default {
  name: "App",
  data: () => ({
    step: 1,
    posts,
    image: "",
    caption: ""
  }),
  methods: {
    uploadImage(evt) {
      console.log('uploadImage!!!!!!!!!')
      const files = evt.target.files;
      if (!files.length) return;

      const reader = new FileReader();
      reader.readAsDataURL(files[0]);
      reader.onload = evt => {
        this.image = evt.target.result;
        this.step = 2;
        console.log(this.image)
      };

      // To enable reuploading of same files in Chrome
      document.querySelector("#file").value = "";
    },
    sharePost() {
      const post = {
        username: "fullstack_vue",
        userImage:
          "https://s3-us-west-2.amazonaws.com/s.cdpn.io/1211695/vue_lg_bg.png",
        postImage: this.image,
        likes: 0,
        caption: this.caption
      };
      this.posts.unshift(post);
      this.goToHome();
    },
    goToHome() {
      this.image = "";
      this.caption = "";
      this.step = 1;
    }
  },
  components: {
    "phone-body": PhoneBody
  }
};
</script>

<style lang="scss">
$small: 520px;
$medium: 768px;
$large: 1216px;

html,
body,
#app {
  height: 100%;
  margin: 0;
  overflow: hidden;
  background: #e6ecf1;
  font-family: "Roboto", sans-serif;
}

#app {
  display: flex;
  align-items: center;
  justify-content: center;
}

.app-phone {
  background-color: white;
  height: 620px;
  width: 375px;
  overflow: hidden;
}

.phone-header {
  height: 50px;
  width: 375px;
  position: sticky;
  position: -webkit-sticky;
  top: 0;
  background: #fafafa;
  border-bottom: 1px solid #eeeeee;
  z-index: 99;

  img {
    max-width: 120px;
    display: block;
    margin: 0 auto;
    padding-top: 1px;
  }

  .cancel-cta,
  .next-cta {
    position: absolute;
    top: 12px;
    color: #209cee;
    font-weight: bold;
    cursor: pointer;
  }

  .cancel-cta {
    left: 10px;
  }

  .next-cta {
    right: 10px;
  }
}

.feed {
  height: 100%;
  overflow-y: scroll;
  overflow-x: hidden;
  margin-right: -15px;
}

.caption-container {
  height: 210px;
  display: flex;
  align-items: center;
  justify-content: center;

  textarea {
    border: 0;
    font-size: 1rem;
    width: 100%;
    padding: 10px;
    border-bottom: 1px solid #eeeeee;
  }

  textarea:focus {
    outline: 0;
  }
}

.selected-image {
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center center;
  height: 330px;
}

.filter-container {
  height: 210px;
  padding: 30px 10px;
  white-space: nowrap;
  overflow-x: hidden;
}

.phone-footer {
  height: 35px;
  width: 375px;
  position: sticky;
  position: -webkit-sticky;
  bottom: 0;
  background: #fafafa;
  border-top: 1px solid #eeeeee;
  z-index: 99;

  .home-cta {
    position: absolute;
    left: 10px;
    top: 6px;
    cursor: pointer;
  }

  .upload-cta {
    position: absolute;
    right: 10px;
    top: 6px;
    p {
      font-size: 0.63rem;
      position: absolute;
      left: -25px;
      top: 5px;
    }
  }

  input[name="file"] {
    visibility: hidden;
  }

  label {
    cursor: pointer;
    z-index: 99;
  }
}

.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-leave-to {
  opacity: 0;
}

// Media Queries
@media (max-width: $small) {
  #app {
    height: 100% !important;
    padding-top: 0 !important;
  }

  .app-phone,
  .app-phone-scroll-cover {
    height: 100%;
    width: 100%;
  }

  .phone-header,
  .phone-footer {
    width: 100%;
  }
}

@media (max-height: $small) {
  .app-phone {
    transform: scale(0.6);
  }
}
</style>
