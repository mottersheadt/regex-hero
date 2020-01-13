<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <h2>pass keys</h2>
      <ul>
        <li
          v-bind:key="str"
          v-for="str in passKeyTest"
          v-bind:class="successRegex(str) ? 'green-text' : null"
        >{{str}}</li>
      </ul>
    </div>
    <div>
      <h2>fail keys</h2>
      <ul>
        <li
          v-bind:key="str"
          v-for="str in failKeyTest"
          v-bind:class="successRegex(str) ? 'red-text' : null"
        >{{str}}</li>
      </ul>
    </div>
    <input type="text" v-on:change="doRegexCheck" v-model="attemptedRegex">
    <div v-show="passed">Success</div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      passKeyTest: ["123-123-abc", "234-631-zzz"],
      failKeyTest: ["123-abc-abc", "234-ab-zzz"],
      passed: false,
      attemptedRegex: null
    };
  },
  props: {
    msg: String
  },
  methods: {
    doRegexCheck() {
      var success = true;
      for (let i = 0; i < this.passKeyTest.length; i++) {
        let re = new RegExp(this.attemptedRegex);
        if (!re.test(this.passKeyTest[i])) {
          success = false;
          break;
        }
      }
      if (success) {
        for (let i = 0; i++; i < this.failKeyTest.length) {
          let re = new RegExp(this.attemptedRegex);
          if (re.test(this.failKeyTest[i])) {
            success = false;
            break;
          }
        }
      }
      this.passed = success;
      console.log(this.passed);
    },
    successRegex: function(str) {
      if (!str) {
        return null;
      }
      let re = new RegExp(this.attemptedRegex, "g");
      let match = str.match(re);
      return match && str === match[0];
    }
  },
  computed: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
a {
  color: #42b983;
}
.green-text {
  color: rgb(76, 150, 76);
}
.red-text {
  color: rgb(150, 76, 76);
}
</style>
