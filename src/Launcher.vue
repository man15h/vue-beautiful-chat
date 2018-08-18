<template>
  <div class="sc-main-wrapper">
    <div
      class="sc-launcher"
      :class="{opened: isOpen || isMultipleOpen}"
      @click.prevent="onClick"
      :style="{backgroundColor: colors.launcher.bg}">
      <div v-if="newMessagesCount > 0 && !isOpen" class="sc-new-messsages-count">
        {{newMessagesCount}}
      </div>
      <img class="sc-open-icon" src="./assets/close-icon.png" />
      <img class="sc-closed-icon" src="./assets/logo-no-bg.svg" />
    </div>
    <ul class="fab-buttons" v-if="isMultipleOpen">
      <li class="fab-buttons__item"
        v-for="(head, index) in activeChats"
        :key="index"
        :style="{backgroundColor: colors.launcher.bg}"
        @click.prevent="chatSelected(head); open(); isMultipleOpen=false;">
        <div v-if="head.counts > 0" class="sc-head-count">
          {{head.counts}}
        </div>
        <a class="fab-buttons__link" v-bind:class="{'notify': head.counts > 0}" :data-tooltip="head.tradeID">
          <img :src="url(head.userid)" />
        </a>
      </li>
    </ul>
    <ChatWindow
      :messageList="messageList"
      :onUserInputSubmit="onMessageWasSent"
      :agentProfile="agentProfile"
      :isOpen="isOpen"
      :onClose="close"
      :onEnd="end"
      :user="user"
      :chatSelected="chatSelected"
      :showEmoji="showEmoji"
      :showFile="showFile"
      :placeholder="placeholder"
      :showTypingIndicator="showTypingIndicator"
      :colors="colors"
      :alwaysScrollToBottom="alwaysScrollToBottom"
    />
  </div>
</template>
<script>
import ChatWindow from './ChatWindow.vue'
import md5 from 'md5';

export default {
  props: {
    showEmoji: {
      type: Boolean,
      default: false
    },
    isOpen: {
      type: Boolean,
      required: true
    },
    open: {
      type: Function,
      required: true
    },
    end: {
      type: Function
    },
    chatSelected:{
      type: Function
    },
    user: {
      type: String,
      required: false,
    },
    activeChats: {
      type: Array,
    },
    close: {
      type: Function
    },
    gravatarType:{
      type:Object,
    },
    showFile: {
      type: Boolean,
      default: false
    },
    agentProfile: {
      type: Object,
      required: true
    },
    onMessageWasSent: {
      type: Function,
      required: true
    },
    messageList: {
      type: Array,
      default: () => []
    },
    newMessagesCount: {
      type: Number,
      default: () => 0
    },
    placeholder: {
      type: String,
      default: 'Write a reply'
    },
    showTypingIndicator: {
      type: Boolean,
      default: () => true
    },
    colors: {
      type: Object,
      required: false,
      validator: c =>
        'header' in c
        && 'bg' in c.header && 'text' in c.header
        && 'launcher' in c
        && 'bg' in c.launcher
        && 'messageList' in c
        && 'bg' in c.messageList
        && 'sentMessage' in c
        && 'bg' in c.sentMessage && 'text' in c.sentMessage
        && 'receivedMessage' in c
        && 'bg' in c.receivedMessage && 'text' in c.receivedMessage
        && 'userInput' in c
        && 'bg' in c.userInput && 'text' in c.userInput,
      default: function () {
        return {
          header: {
            bg: '#4e8cff',
            text: '#ffffff'
          },
          launcher: {
            bg: '#4e8cff'
          },
          messageList: {
            bg: '#ffffff'
          },
          sentMessage: {
            bg: '#4e8cff',
            text: '#ffffff'
          },
          receivedMessage: {
            bg: '#f4f7f9',
            text: '#ffffff'
          },
          userInput: {
            bg: '#f4f7f9',
            text: '#565867'
          }
        }
      }
    },
    alwaysScrollToBottom: {
      type: Boolean,
      default: () => false
    }
  },
  data () {
    return {
      isMultipleOpen: false,
    }
  },
  methods:{
    openMultiple(){
      this.isMultipleOpen = !this.isMultipleOpen;
    },
    url(userid) {
      const img = [
        '//www.gravatar.com/avatar/',
        md5(userid.trim().toLowerCase()),
        `?s=${this.gravatarType.size || 40}`,
         `&d=${this.gravatarType.defaultImg || 'retro'}`,
         `&r=${this.gravatarType.rating || 'g'}`
      ];
      return img.join('');
    },
    onClick(){
      if (this.isOpen || this.isMultipleOpen) {
        this.close();
        this.isMultipleOpen = false;
      }
      else {
        if (this.activeChats  && this.activeChats.length > 1) {
          this.isMultipleOpen =true;
        }
        else {
          this.open();
        }
      }
    }
  },
  components: {
    ChatWindow
  }
}
</script>
<style scoped>
.sc-main-wrapper{

  width: 60px;
  background-position: center;
  background-repeat: no-repeat;
  position: fixed;
  right: 25px;
  bottom: 25px;
}
.sc-launcher {
  width: 60px;
  height: 60px;
  background-position: center;
  background-repeat: no-repeat;
  border-radius: 50%;
  position: absolute;
  bottom: 0;
  box-shadow: none;
  transition: box-shadow 0.2s ease-in-out;
  cursor: pointer;
}

.sc-launcher:before {
  content: '';
  position: relative;
  display: block;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  transition: box-shadow 0.2s ease-in-out;
}

.sc-launcher .sc-open-icon,
.sc-launcher .sc-closed-icon {
  width: 60px;
  height: 60px;
  position: fixed;
  right: 25px;
  bottom: 25px;
  transition: opacity 100ms ease-in-out, transform 100ms ease-in-out;
}

.sc-launcher .sc-closed-icon {
  transition: opacity 100ms ease-in-out, transform 100ms ease-in-out;
  width: 60px;
  height: 60px;
}

.sc-launcher .sc-open-icon {
  padding: 20px;
  box-sizing: border-box;
  opacity: 0;
}

.sc-launcher.opened .sc-open-icon {
  transform: rotate(-90deg);
  opacity: 1;
}

.sc-launcher.opened .sc-closed-icon {
  transform: rotate(-90deg);
  opacity: 0;
}

.sc-launcher.opened:before {
  box-shadow: 0px 0px 400px 250px rgba(148, 149, 150, 0.2);
}

.sc-launcher:hover {
  box-shadow: 0 0px 27px 1.5px rgba(0,0,0,0.2);
}

.sc-new-messsages-count {
  position: absolute;
  top: -3px;
  left: 41px;
  display: flex;
  justify-content: center;
  flex-direction: column;
  border-radius: 50%;
	width: 22px;
  height: 22px;
  background: #ff4646;
  color: white;
  text-align: center;
  margin: auto;
  font-size: 12px;
  font-weight: 500;
}
.fab-buttons {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 70px;
    list-style: none;
    margin: 0;
    padding: 0;
  }

  .fab-action-button__icon {
    display: inline-block;
    width: 56px;
    height: 56px;
  }

  .fab-buttons__item {
    display: block;
    text-align: center;
    margin: 12px auto;
    width: 40px;
    overflow: hidden;
    height: 40px;
    border-radius: 50%;
  }

  .fab-buttons__link {
    display: inline-block;
    width: 40px;
    height: 40px;
    text-decoration: none;
    border-radius: 50%;
  }
  .fab-buttons__link img{
    position: relative;
    top: 0px;
  }
  .fab-buttons__link.notify img{
    top: -20px;
  }

  [data-tooltip]:before {
  top: 50%;
  margin-top: -11px;
  font-weight: 600;
  border-radius: 2px;
  background: #585858;
  color: #fff;
  content: attr(data-tooltip);
  font-size: 12px;
  text-decoration: none;
  visibility: hidden;
  opacity: 0;
  padding: 4px 7px;
  margin-right: 12px;
  position: absolute;
  transform: scale(0);
  right: 100%;
  white-space: nowrap;
  transform-origin: top right;
  transition: all .3s cubic-bezier(.25, .8, .25, 1);
  }

  [data-tooltip]:hover:before {
  visibility: visible;
  opacity: 1;
  transform: scale(1);
  transform-origin: right center 0;
  }

  .sc-head-count{
    position: relative;
    top: 10px;
    /* left: 15px; */
    z-index: 10;
    display: flex;
    justify-content: center;
    flex-direction: column;
    border-radius: 50%;
    width: 20px;
    height: 20px;
    background: #ff4646;
    color: white;
    text-align: center;
    margin: auto;
    font-size: 12px;
    font-weight: 500;
  }
</style>
