<template>
  <div class="sc-message--text" :style="messageColors">
    {{data.text}}
    <p v-if="data.meta" class='sc-message--meta' :style="{color: messageColors.color}">
      {{res(data.meta)}}
    </p>
  </div>
</template>

<script>
import {formatDate} from './lib';
export default {
  props: {
    data: {
      type: Object,
      required: true
    },
    messageColors: {
      type: Object,
      required: true
    }
  },
  methods:{
    res(d){
      formatDate(d);
    }
  }
}
</script>

<style scoped>

</style>
