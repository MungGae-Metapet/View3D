<template>
  <div class="about">
    <h1>This is an about page</h1>

    <View3D
      ref="view3d"
      src="https://naver.github.io/egjs-view3d/model/RobotExpressive.glb"
      class="view3d-3by1"
      @ready="onReady"
      :translate="false"
      :rotate="{ disablePitch: true, disableYaw: true }"
    >
      <button id="enter-ar" @click="enterAR">ENTER AR</button>
    </View3D>

    <!-- <button @click="changeAction">Change Animation</button> -->
    <br />
    <!-- {{ typeof animations }} -->
    <button @click="resetCamera">reset</button>
    <div>
      <button @click="pause">pause</button>
      <button @click="resume">resume</button>
      <button @click="stop">stop</button>
      <button @click="play">play</button>
    </div>
    <div v-for="(item, idx) in animationsName" :key="idx">
      <button @click="changeAction(idx)">
        {{ item }}
      </button>
    </div>
  </div>
</template>

<script>
// import View3D from "@egjs/view3d";
import { View3D, ARButton } from "@egjs/vue-view3d";
import "@egjs/vue-view3d/css/view3d-bundle.css";

export default {
  data() {
    return {
      model: null,
      action: 0,
      animations: null,
      animationsName: null,
      animationCount: null,
    };
  },

  components: {
    View3D,
  },
  methods: {
    onReady() {
      this.$refs.view3d.animator.play(0);
      this.animations = this.$refs.view3d.animator.clips;
      this.animationsName = this.$refs.view3d.animator.clips.map(
        (clip) => clip.name
      );
      this.animationCount = this.$refs.view3d.animator.animationCount;
      this.$refs.view3d.animator.crossFade(1, 3000, { synchronize: true });

      //

      this.$refs.view3d.loadPlugins(new ARButton());
      // console.log(this.animations[0], this.animations[1]);
    },
    changeAction(idx) {
      this.action = idx;
      this.fadeout(idx).then(() => {
        this.$refs.view3d.animator.play(idx);
      });
    },

    pause() {
      this.$refs.view3d.animator.pause();
    },
    resume() {
      this.$refs.view3d.animator.resume();
    },
    stop() {
      this.$refs.view3d.animator.stop();
    },
    play() {
      this.$refs.view3d.animator.play(this.action);
    },
    resetCamera() {
      this.$refs.view3d.camera.reset(500);
    },

    async fadeout(idx) {
      await this.$refs.view3d.animator.crossFade(
        idx,
        this.animations[idx].duration * 1000
      );
    },

    enterAR() {
      this.$refs.view3d.ar.enter();
    },
  },
};
</script>

<style></style>
