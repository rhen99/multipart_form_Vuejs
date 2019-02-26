<template>
  <div id="app">
    <Progress v-bind:stages="stages"/>
    <Form
      v-bind:step="step"
      v-on:decrease-step="decreaseStep"
      v-on:increase-step="increaseStep"
      v-on:submitted="submitForm"
    />
  </div>
</template>

<script>
import Form from "./components/Form";
import Progress from "./components/Progress";
export default {
  name: "app",
  components: {
    Form,
    Progress
  },
  data() {
    return {
      step: 1,
      stages: {
        isFirst: true,
        isSecond: false,
        isFinal: false
      }
    };
  },
  methods: {
    increaseStep(stepVal) {
      this.step = stepVal;
    },
    decreaseStep(stepVal) {
      this.step = stepVal;
    },
    submitForm(stepVal) {
      this.step = stepVal;
    }
  },
  watch: {
    step(n, o) {
      if (n === 1) {
        this.stages.isFinal = false;
        this.stages.isSecond = false;
        this.stages.isFirst = true;
      } else if (n === 2) {
        this.stages.isFirst = false;
        this.stages.isFinal = false;
        this.stages.isSecond = true;
      } else {
        this.stages.isFirst = false;
        this.stages.isSecond = false;
        this.stages.isFinal = true;
      }
    }
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
  background-color: #f0f0f0;
  overflow: hidden;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
#app {
  width: 100%;
  height: 100vh;
}
</style>
