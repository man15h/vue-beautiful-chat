<template>
  <div class="sc-header" :style="{background: colors.header.bg, color: colors.header.text}">
    <div class="sc-header--team-name"> {{truncateName}} &nbsp;
      <span v-if="id" style="font-size:0.9rem">#{{truncateId}}</span>
    </div>
    <div class="sc-header--close-button" @click="onClose">
      <img src="./assets/minimize.svg" alt="" height="40px" width="40px"/>
    </div>
    <div class="sc-header--close-button" @click="onEnd">
      <img src="./assets/close-icon.png" alt="" />
    </div>
  </div>
</template>
<script>
export default {
  props: {
    teamName: {
      type: String,
      default: 'user',
    },
    onClose: {
      type: Function,
      required: true
    },
    id:{
      type: String,
    },
    onEnd: {
      type: Function,
      required: false
    },
    colors: {
      type: Object,
      required: true
    }
  },
  computed: {
    truncateName: function () {
      let length = 17;
      let ending = "...";
      if ( this.teamName.length > length) {
        return  this.teamName.substring(0, length - ending.length) + ending;
      } else {
        return  this.teamName;
      }
    },
    truncateId: function () {
      let length = 15;
      let ending = "...";
      if (this.id.length > length) {
        return  this.id.substring(0, length - ending.length) + ending;
      } else {
        return  this.id;
      }
    }
  }
}
</script>
<style scoped>
.sc-header {
  height: 60px;
  border-top-left-radius: 9px;
  border-top-right-radius: 9px;
  padding: 10px;
  box-shadow: 0 1px 4px rgba(0,0,0,.2);
  position: relative;
  box-sizing: border-box;
  display: flex;
}

.sc-header--img {
  border-radius: 50%;
  align-self: center;
  padding: 10px;
}

.sc-header--team-name {
  align-self: center;
  padding: 10px;
  flex-direction: row;
  flex: 1;
  user-select: none;
  border-radius: 5px;
}


.sc-header--close-button {
  width: 40px;
  align-self: center;
  height: 40px;
  margin-right: 10px;
  box-sizing: border-box;
  cursor: pointer;
  border-radius: 5px;
}

.sc-header--close-button:hover {
  box-shadow: 0px 2px 5px rgba(0.2, 0.2, 0.5, .1);
}

.sc-header--close-button img {
  width: 100%;
  height: 100%;
  padding: 13px;
  box-sizing: border-box;
}

@media (max-width: 450px) {
  .sc-header {
    border-radius: 0px;
  }
}
</style>
