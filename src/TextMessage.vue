<template>
  <div class="sc-message--text" :style="messageColors">
    {{data.text}}
    <p v-if="data.meta" class='sc-message--meta' :style="{color: messageColors.color}">
      {{formatDate(data.meta)}}
    </p>
  </div>
</template>

<script>
var moment = require('moment');
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
    formatDate(timestamp){
      if (Number.isInteger(Number(timestamp))) {
        var d = new Date(timestamp * 1000),	// Convert the passed timestamp to milliseconds
    		yyyy = d.getFullYear(),
    		mm = ('0' + (d.getMonth() + 1)).slice(-2),	// Months are zero based. Add leading 0.
    		dd = ('0' + d.getDate()).slice(-2),			// Add leading 0.
    		hh = d.getHours(),
    		h = hh,
    		min = ('0' + d.getMinutes()).slice(-2),		// Add leading 0.
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
    }
  }
}
</script>

<style scoped>

</style>
