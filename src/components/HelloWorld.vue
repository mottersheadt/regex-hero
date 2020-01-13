<template>
  <div class="hello">
    <h1>Regex Hero</h1>
    <div>
      Enter a regex that matches all the pass keys and doesn't match any of the fail keys.
    </div>
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
      <div id="game"></div>
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
import * as PIXI from "pixi.js";
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
        if (!this.successRegex(this.passKeyTest[i])) {
          success = false;
          break;
        }
      }
      if (success) {
        for (let i = 0; i++; i < this.failKeyTest.length) {
          if (this.successRegex(this.failKeyTest[i])) {
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
      let re = new RegExp(this.attemptedRegex);
      let match = str.match(re);
      return match && str === match[0];
    }
  },
  created() {
    debugger;

    this.$nextTick(() => {
      const app = new PIXI.Application({ transparent: true });
      document.body.appendChild(app.view);

      const { renderer, stage, loader } = app;
    
      // const container = new PIXI.Container();
      // container.x = app.screen.width;
      // container.y = app.screen.height

      // container.pivot.x = container.width / 2;
      // container.pivot.y = container.height / 2;
      // stage.addChild(container);

      loader.add('image', require('../assets/angry-trump.png'));
      loader.load();
      loader.onComplete.add(onAssetsLoaded);
    
      function onAssetsLoaded () { 
        createElements();
        update();
      }
    
      function createElements () {
        const texture = loader.resources['image'].texture;
        const sprite = new PIXI.Sprite.from(texture);

        // setting up sprite x and y coords, height, width etc

        sprite.width = 100;
        sprite.height= 100;
        // center the sprite's anchor point
        sprite.anchor.set(0.5);

        // move the sprite to the center of the screen
        sprite.x = app.screen.width / 2;
        sprite.y = app.screen.height / 2;

        app.stage.addChild(sprite);

        app.ticker.add(() => {
            // just for fun, let's rotate mr rabbit a little
            sprite.rotation += 0.01;
        });

        // container.addChild(sprite);
      }

      function update () {
        renderer.render(stage);
        requestAnimationFrame(update);
      }
    });
  },
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
