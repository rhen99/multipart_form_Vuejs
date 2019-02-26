<template>
  <div id="form_div" class="form">
    <form @submit.prevent="submitForm">
      <p class="failed" v-if="submitStatus === 'ERROR'">Please fill in all fields correctly.</p>
      <div id="person" v-if="step === 1" class="sections">
        <p>Please tell us about yourself.</p>
        <div id="name_fields" class="fields">
          <div class="half">
            <label for="first">First Name</label>
            <input type="text" id="first" v-model.trim="$v.firstName.$model">
            <span
              class="error"
              v-bind:class="{'show-error':$v.firstName.$error}"
              v-if="!$v.firstName.required"
            >First Name is required!</span>
            <span
              class="error"
              v-bind:class="{'show-error':$v.firstName.$error}"
              v-if="!$v.firstName.alpha"
            >Invalid First Name!</span>
          </div>
          <div class="half">
            <label for="last">Last Name</label>
            <input type="text" id="last" v-model.trim="$v.lastName.$model">
            <span
              class="error"
              v-bind:class="{'show-error':$v.lastName.$error}"
              v-if="!$v.lastName.required"
            >Last Name is required!</span>
            <span
              class="error"
              v-bind:class="{'show-error':$v.lastName.$error}"
              v-if="!$v.lastName.alpha"
            >Invalid Last Name!</span>
          </div>
        </div>
        <div class="fields">
          <label for="age">Age</label>
          <input type="number" id="age" v-model.trim="$v.age.$model">
          <span
            class="error"
            v-bind:class="{'show-error':$v.age.$error}"
            v-if="!$v.age.required"
          >Age is required!</span>
        </div>
        <div class="fields">
          <label for="gender">Gender</label>
          <select id="gender" v-model.trim="$v.gender.$model">
            <option value="male">Male</option>
            <option value="female">Female</option>
            <option value="other">Other</option>
          </select>
          <span
            class="error"
            v-bind:class="{'show-error':$v.gender.$error}"
            v-if="!$v.gender.required"
          >Please select a gender!</span>
        </div>
      </div>
      <div id="register" v-if="step === 2" class="sections">
        <p>Enter your Email, and Password</p>
        <div class="fields">
          <label for="email">Email</label>
          <input type="email" id="email" v-model.trim="$v.email.$model">
          <span
            class="error"
            v-bind:class="{'show-error':$v.email.$error}"
            v-if="!$v.email.required"
          >Email is required!</span>
          <span
            class="error"
            v-bind:class="{'show-error':$v.email.$error}"
            v-if="!$v.email.email"
          >Needs to be a valid Email!</span>
        </div>
        <div class="fields">
          <label for="password">Password</label>
          <input type="password" v-model.trim="$v.password.$model" id="password">
          <span
            class="error"
            v-bind:class="{'show-error':$v.password.$error}"
            v-if="!$v.password.required"
          >Password is required!</span>
          
          <span
            class="error"
            v-bind:class="{'show-error':$v.password.$error}"
            v-if="!$v.password.minLength"
          >Password must be at least {{$v.password.$params.minLength.min}} letters!</span>
        </div>
        <div class="fields">
          <label for="confirm_password">Confirm Password</label>
          <input type="password" id="confirm_password" v-model.trim="$v.confirm_password.$model">
          
          <span
            class="error"
            v-bind:class="{'show-error':$v.confirm_password.$error}"
            v-if="!$v.confirm_password.sameAsPassword"
          >Passwords must be identical!</span>
        </div>
      </div>
      <div id="thank-you" v-if="step === 3">
        <h2>Registration Successful!</h2>
        <p>Please check your Email Inbox for verification.</p>
      </div>
      <div class="navigate" v-if="step === 2">
        <button id="prev" @click="prev" v-bind:class="{'disable':prevDisabled}">&larr;</button>
        <button id="submit" type="submit" v-bind:class="{'disable':nextDisabled}">&rarr;</button>
      </div>
      <div class="navigate" v-else>
        <button id="prev" @click="prev" v-bind:class="{'disable':prevDisabled}">&larr;</button>
        <button id="next" @click="next" v-bind:class="{'disable':nextDisabled}">&rarr;</button>
      </div>
    </form>
  </div>
</template>
<script>
import {
  required,
  minLength,
  alpha,
  email,
  sameAs
} from "vuelidate/lib/validators";
export default {
  name: "Form",
  data() {
    return {
      firstName: null,
      lastName: "",
      age: null,
      gender: "",
      email: "",
      password: "",
      confirm_password: "",
      prevDisabled: true,
      nextDisabled: false,
      stepVal: this.step,
      submitStatus: null
    };
  },
  props: ["step"],
  methods: {
    prev(e) {
      e.preventDefault();
      this.stepVal--;
      this.$emit("decrease-step", this.stepVal);
      if (this.stepVal < 2) {
        this.prevDisabled = true;
      }
    },
    next(e) {
      e.preventDefault();
      this.stepVal++;
      this.$emit("increase-step", this.stepVal);
      if (this.stepVal < 2) {
        this.prevDisabled = true;
      } else if (this.stepVal == 2) {
        this.prevDisabled = false;
      } else if (this.stepVal == 3) {
        this.prevDisabled = true;
        this.nextDisabled = true;
      }
    },
    submitForm() {
      console.log("submit");
      this.$v.$touch();
      if (this.$v.$invalid) {
        this.submitStatus = "ERROR";
      } else {
        this.submitStatus = "SUCCESS";
        this.stepVal++;
        this.prevDisabled = true;
        this.nextDisabled = true;
        this.$emit("submitted", this.stepVal);
      }
    }
  },
  validations: {
    firstName: {
      required,
      alpha
    },
    lastName: {
      required,
      alpha
    },
    age: {
      required
    },
    gender: {
      required
    },
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(8)
    },
    confirm_password: {
      required,
      sameAsPassword: sameAs("password")
    }
  }
};
</script>
<style scoped>
#form_div {
  background-color: #fff;
  margin: 10% auto;
  padding: 1rem;
  position: relative;
}
.form {
  min-height: 300px;
  width: 30%;
}
.fields {
  width: 100%;
}
.fields input,
.fields select {
  width: 100%;
  padding: 3px;
  font-size: 0.8rem;
  outline: 0;
}
.fields input:focus {
  border: 1px solid rgb(34, 185, 34);
}
.half {
  width: 50%;
  margin-left: 5px;
}
#name_fields {
  display: flex;
}
p {
  padding: 5px 0;
  font-weight: 600;
}
h2 {
  font-size: 1.2rem;
  text-align: center;
}
.navigate {
  display: block;
  padding: 1rem;
}
#thank-you p {
  text-align: center;
}
button {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  border: 0;
  font-size: 1rem;
  background-color: rgb(34, 185, 34);
  color: #fff;
  cursor: pointer;
}
#prev {
  float: left;
}
#next {
  float: right;
}
#submit {
  float: right;
}
.disable {
  display: none;
}
.error {
  color: rgb(255, 94, 0);
  display: none;
}
span {
  font-size: 0.8rem;
}
.show-error {
  display: inline;
}
.failed {
  font-size: 0.9rem;
  color: rgb(255, 60, 0);
}
@media (max-width: 1200px) {
  #form_div {
    width: 60%;
    margin: 20% auto;
  }
}
@media (max-width: 700px) {
  #form_div {
    width: 90%;
    margin: 30% auto;
  }
}
@media (max-width: 600px) {
  #form_div {
    width: 100%;
    margin: 40% auto;
  }
}
</style>
