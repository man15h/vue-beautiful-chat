<template>
  <div class="sc-message--text" :style="messageColors">
    {{data.text}}
    <p v-if="data.meta" class='sc-message--meta' :style="{color: messageColors.color}">
      {{res(data.meta)}}
    </p>
  </div>
</template>

<script>
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
    res(data){
      if (Number.isInteger(Number(data))) {
        let d = new Date(data * 1000),
        yyyy = d.getFullYear(),
        mm = ('0' + (d.getMonth() + 1)).slice(-2),
        dd = ('0' + d.getDate()).slice(-2),
        hh = d.getHours(),
        h = hh,
        min = ('0' + d.getMinutes()).slice(-2),
        ampm = 'AM',
        time;
        if (hh > 12) {
          h = hh - 12;
          ampm = 'PM';
        } else if (hh === 12) {
          h = 12;
          ampm = 'PM';
        } else if (hh == 0) {
          h = 12;
        }
        // ie: 2013-02-18, 8:35 AM
        time = yyyy + '-' + dd + '-' + mm + ', ' + h + ':' + min + ' ' + ampm;
        return time;
      }
      else {
        return data;
      }
    }
  }
}
</script>

<style scoped>

</style>
