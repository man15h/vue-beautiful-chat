<template>
  <div class="sc-chat-window" :class="{opened: isOpen, closed: !isOpen}">
    <Header
      :teamName="agentProfile.userid"
      :id="agentProfile.tradeID || agentProfile.chatID"
      :imageUrl="agentProfile.imageUrl|| ''"
      :onClose="onClose"
      :onEnd="onEnd"
      :colors="colors"
    />
    <MessageList
      :messages="messages"
      :user="user"
      :imageUrl="agentProfile.imageUrl || ''"
      :chatImageUrl="agentProfile.imageUrl || ''"
      :showTypingIndicator="showTypingIndicator"
      :colors="colors"
      :alwaysScrollToBottom="alwaysScrollToBottom"
    />
    <UserInput
      :showEmoji="showEmoji"
      :onSubmit="onUserInputSubmit"
      :showFile="showFile"
      :placeholder="placeholder"
      :colors="colors" />
  </div>
</template>

<script>
import Header from './Header.vue'
import MessageList from './MessageList.vue'
import UserInput from './UserInput.vue'

export default {
  components: {
    Header,
    MessageList,
    UserInput
  },
  props: {
    showEmoji: {
      type: Boolean,
      default: false
    },
    showFile: {
      type: Boolean,
      default: false
    },
    user: {
      type: String,
      required: false,
    },
    agentProfile: {
      type: Object,
      required: true
    },
    onUserInputSubmit: {
      type: Function,
      required: true
    },
    onEnd: {
      type: Function,
      required: false
    },
    onClose: {
      type: Function,
      required: true
    },
    messageList: {
      type: Array,
      default: () => []
    },
    isOpen: {
      type: Boolean,
      default: () => false
    },
    placeholder: {
      type: String,
      default: 'Write a reply'
    },
    showTypingIndicator: {
      type: Boolean,
      default: () => false
    },
    colors: {
      type: Object,
      required: true
    },
    alwaysScrollToBottom: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {}
  },
  computed: {
    messages() {
      let messages = this.messageList

      return messages
    }
  },
  methods: {}
}
</script>
<style scoped>
.sc-chat-window {
  width: 370px;
  z-index: 100;
  height: calc(100% - 120px);
  max-height: 590px;
  position: fixed;
  right: 25px;
  bottom: 100px;
  box-sizing: border-box;
  box-shadow: 0px 7px 40px 2px rgba(148, 149, 150, 0.1);
  background: white;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: 0.3s ease-in-out;
  border-radius: 10px;
}

.sc-chat-window.closed {
  opacity: 0;
  visibility: hidden;
  bottom: 90px;
}

.sc-message--me {
  text-align: right;
}
.sc-message--them {
  text-align: left;
}

@media (max-width: 450px) {
  .sc-chat-window {
    width: 100%;
    height: 100%;
    max-height: 100%;
    right: 0px;
    bottom: 0px;
    border-radius: 0px;
  }
  .sc-chat-window {
    transition: 0.1s ease-in-out;
  }
  .sc-chat-window.closed {
    bottom: 0px;
  }
}
</style>
