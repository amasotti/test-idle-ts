<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <div class="container-test">
    <h3>Status</h3>
    <p id="status">Active</p>
    </div>

    <div class="container-test">
    <h3>Visibility</h3>
    <p id="visibility">Visible</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { IdleTs } from '@amasotti/idle-ts';

export default defineComponent({
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  mounted() {
  const initialDelay = 2500;
  console.log("Initialize trigger in " + initialDelay + "ms");

  const idlenessTargetElement : HTMLElement | null = document.querySelector('#status');
  const visibilityTargetElement : HTMLElement | null = document.querySelector('#visibility');

  if (!idlenessTargetElement || !visibilityTargetElement) {
      console.error("[IDLE-TS]: No target Element found for either idleness or visibility");
  } else {

    let idle = new IdleTs({
      idle: initialDelay,

      onIdle: function () {
        idlenessTargetElement.classList.toggle('idle');
        idlenessTargetElement.textContent = 'Idle!';
      },
      onActive: function () {
        idlenessTargetElement.classList.toggle('idle');
        idlenessTargetElement.textContent = 'Active!';
      },
      onHide: function () {
        visibilityTargetElement.classList.toggle('idle');
        visibilityTargetElement.textContent = 'Hidden!';
      },
      onShow: function () {
        // Add a slight pause so you can see the change
        setTimeout(function () {
          visibilityTargetElement.classList.toggle('idle');
          visibilityTargetElement.textContent = 'Visible (again)!';
        }, 1000);
      },
    }).start();


    window.setTimeout(() => {
      let newDelay = 5000;
      idle.set({idle: newDelay});
      console.log(`now triggering in: ${newDelay}`)
    },5000)

}
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.container-test {
  display: flex;
  flex-direction: column;
  justify-items: center;
  align-items: center;
  border: 1px solid #42b983;
  border-radius: 8px;
  margin: 15px auto;
  max-width: 60vw;
}

.idle {
  color: crimson!important;
  font-size: 4rem!important;
}
</style>
