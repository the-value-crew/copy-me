<script>
import Vue from 'vue';
import CopyMe from '@/copy-me.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    CopyMe
  }
});
</script>

<template>
  <div id="app" style="padding: 100px">
    <copy-me text="hello from bibhuti">
      <button slot="handler">Copy me</button>
    </copy-me>
  </div>
</template>
