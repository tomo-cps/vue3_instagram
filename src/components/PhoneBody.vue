<template>
<div class="phone-body">
    <div v-if="step === 1" class="feed" v-dragscroll.y>
        <vuegram-post v-for="post in posts"
            v-bind:post="post"
            v-bind:key="posts.indexOf(post)">
        </vuegram-post>
      </div>
      <div v-if="step === 2">
        <div class="selected-image"
        :style="{ backgroundImage: 'url(' + image + ')' }"></div>
      </div>
    <div v-if="step === 3">
      <div class="selected-image"
        :style="{ backgroundImage: 'url(' + image + ')' }"></div>
      <div class="caption-container">
        <textarea class="caption-input"
          placeholder="Write a caption..."
          type="text"
          :value="value"
          @input="$emit('input', $event.target.value)">
        </textarea>
      </div>
    </div>
</div>
</template>

<script>
import VuegramPost from "./VuegramPost";

export default {
  name: 'PhoneBody',
  props: {
    step: Number,
    posts: Array,
    image: String,
    value: String
  },
  components: {
    "vuegram-post": VuegramPost,
  },
  methods: {
    // メニューバーのボタンを押した時に，そのボタンを非活性にする処理
    // 一旦全てのボタンを"false:活性"にした後に 該当するボタンを"true:非活性"にしている．
  }
}
</script>

<style lang="scss">
.phone-body {
  height: 100%;
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
  height: 220px;
  padding: 30px 10px;
  white-space: nowrap;
  overflow-x: scroll;
}

</style>