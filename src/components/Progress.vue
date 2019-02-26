<template>
  <div id="proogress_bar">
    <ol class="progress-tracker">
      <li v-bind:class="{'active': stages.isFirst, 'finished': stages.isSecond || stages.isFinal}">
        <span class="step-name">Person Info</span>
      </li>
      <li v-bind:class="{'active': stages.isSecond, 'finished': stages.isFinal}">
        <span class="step-name">Email & Password</span>
      </li>
      <li v-bind:class="{'active': stages.isFinal}">
        <span class="step-name">Thank you</span>
      </li>
    </ol>
  </div>
</template>
<script>
export default {
  name: "Progress",
  props: ["stages"]
};
</script>
<style scoped>
* {
  box-sizing: border-box;
}
#progress_bar {
}
.progress-tracker {
  counter-reset: step;
  position: absolute;
  width: 70%;
  top: 30px;
  left: 15%;
}
.progress-tracker li {
  list-style-type: none;
  float: left;
  width: 33.33%;
  position: relative;
  text-align: center;
}
.progress-tracker li::before {
  position: relative;
  display: block;
  content: counter(step);
  counter-increment: step;
  width: 35px;
  height: 35px;
  line-height: 30px;
  border: #ddd 1px solid;
  margin: 10px auto 10px auto;
  text-align: center;
  border-radius: 50%;
  background-color: #fff;
}
.progress-tracker li::after {
  content: "";
  background: #ddd;
  width: 100%;
  height: 1px;
  position: absolute;
  z-index: -1;
  top: 30px;
  left: -50%;
}
.progress-tracker li:first-child::after {
  content: none;
}
.progress-tracker li.active span {
  color: rgb(6, 177, 63);
}
.progress-tracker li.active::before {
  background-color: rgb(6, 177, 63);
  color: #fff;
}
.progress-tracker li.finished::before {
  content: "";
  background-color: rgb(6, 177, 63);
  color: #fff;
}
.progress-tracker li.finished + li::after {
  background-color: rgb(6, 177, 63);
}
</style>
