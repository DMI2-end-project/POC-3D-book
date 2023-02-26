<template>
  <div>
    <video ref="next" width="100" muted autoplay="autoplay">
        <source src="./assets/next.mp4" type="video/mp4">
    </video>
    <video ref="previous" width="100" muted autoplay="autoplay">
        <source src="./assets/previous.mp4" type="video/mp4">
    </video>
    <video ref="open" width="100" muted autoplay="autoplay">
        <source src="./assets/open.mp4" type="video/mp4">
    </video>
    <div class="content">
      <div class="book-content">
        <div ref="pageLeft" class="page page-left">page {{ page * 2 }}</div>
        <div ref="pageRight" class="page page-right">page {{ page * 2 + 1 }}</div>
      </div>
      <button class="open" ref="buttonOpen" @click="openTheBook">Ouvrir le livre</button>
      <button class="next disable" ref="buttonNext" @click="nextPage">Suivant</button>
      <button class="previous disable" ref="buttonPrevious" @click="previousPage">Précédent</button>
    </div>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      page: 0,
    }
  },
  mounted () {
    this.$refs.open.pause();
  },
  methods: {
    openTheBook () {
      this.$refs.open.play();
      this.$refs.buttonOpen.classList.add("disable");
      this.$refs.buttonNext.classList.remove("disable");
      this.$refs.buttonPrevious.classList.remove("disable");
      this.updateContentPage(2400, 0);
    },
    nextPage () {
      this.$refs.next.play();
      this.$refs.next.classList.add("first");
      this.$refs.previous.classList.remove("first");
      this.$refs.open.classList.remove("first");
      this.updateContentPage(1200, +1);

    },
    previousPage () {
      this.$refs.previous.play();
      this.$refs.previous.classList.add("first");
      this.$refs.next.classList.remove("first");
      this.$refs.open.classList.remove("first");
      this.updateContentPage(1200, -1);
    },
    updateContentPage (timeout, delta) {
      this.$refs.pageLeft.classList.remove("page-active");
      this.$refs.pageRight.classList.remove("page-active");
      console.log('1')
      setTimeout(() => {
        console.log('2')
        this.$refs.pageLeft.classList.add("page-active");
        this.$refs.pageRight.classList.add("page-active");
        this.page += delta
      }, timeout)
    }
  }
};
</script>

<style scoped>
video {
  position: fixed;
  width: 100%;
  height: auto;
  inset: 0;
  margin: auto;
  z-index: 1;
}

video.first {
  z-index: 2;
}

.content {
  position: fixed;
  z-index: 10;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  inset: 0;
  margin: auto;
}

button {
  z-index: 30;
}

button.disable {
  opacity: 0;
  pointer-events: none;
}

button.open {
  margin-left: 18%;
}

button.next {
  position: absolute;
  margin-left: calc(40% + 100px);
}

button.previous {
  position: absolute;
  margin-left: calc(-40% - 150px);
}

.book-content {
  position: absolute;
  z-index: 5;
  width: 100vw;
  height: 0;
  padding-bottom: 56.5%;
  inset: 0;
  margin: auto;
}

.book-content .page {
  width: 15%;
  height: 40%;
  position: absolute;
  inset:0;
  margin: auto;
  opacity: 0;
  transition: opacity 0.2s ease;
}

.book-content .page-active {
  opacity: 1;
}

.book-content .page-left {
  transform: translate(-65%, 4%);
}

.book-content .page-right {
  transform: translate(55%, 4%);
}

</style>
