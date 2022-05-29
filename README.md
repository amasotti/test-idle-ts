# PoC for [idle-ts](https://github.com/amasotti/idle-ts)

This is just an hello world Vue app to test if my experiments in [idle-ts](https://github.com/amasotti/idle-ts) do work.

1. Scaffold new vue app (with Vue-cli)

> vue create hello-world

2. Add module declaration in `shims-vue.d.ts`

```js
declare module '@amasotti/idle-ts';
```

3. Test the plugin/library

*in this example I've just expanded the hello-World component created by vue-cli*

~~~vue
<template>
...
<div class="container-test">
    <h3>Status</h3>
    <p id="status">Active</p>
    </div>

    <div class="container-test">
    <h3>Visibility</h3>
    <p id="visibility">Visible</p>
    </div>
...
</template>

<script>
import { IdleTs } from '@amasotti/idle-ts';
...

export default defineComponent({
  ...
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
})
</script>

~~~
