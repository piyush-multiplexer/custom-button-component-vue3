<script setup>
import AppBtn from './components/AppBtn.vue';
import { ref, reactive, onMounted } from 'vue';

const messages = reactive([]);
const messageBox = ref();
const height = 8;
const loading = ref(false);
let interval;
const onClick = (evt) => {
  addToast(`Clicked ${evt.target.innerText}`);
};
const onMouseOver = (evt) => {
  addToast(`MouseOver ${evt.target.innerText}`);
};
const onMouseOut = (evt) => {
  addToast(`MouseOut ${evt.target.innerText}`);
};
const onMouseDown = (evt) => {
  addToast(`MouseDown ${evt.target.innerText}`);
};
const onMouseUp = (flag, evt) => {
  addToast(`MouseUp ${evt.target.innerText}`);
};
const toggleLoader = () => {
  loading.value = !loading.value;
  setTimeout(() => {
    loading.value = !loading.value;
  }, 2000);
};
const addToast = (message) => {
  const total = messages.length;
  for (var i = 0; i < total; i++) {
    var msg = messages[i];
    var pos = -((i + 1) * height);
  }

  const newMessage = document.createElement('div');
  newMessage.className = 'message';
  newMessage.textContent = message;
  messageBox.value.append(newMessage);
  messages.unshift(newMessage);
  if (messages.length >= 20) {
    setTimeout(resetToasts, 200);
  }
};

const resetToasts = () => {
  messages.length = 0;
  messageBox.value.innerHTML = '';
  addToast('Cleared!!');
};

onMounted(() => {
  addToast('Cleared!!');
});
</script>

<template>
  <div class="container">
    <div class="row">
      <div class="column">
        <h3>Default</h3>
        <AppBtn
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >Default Button</AppBtn
        >
      </div>
      <div class="column">
        <h3>Loading</h3>
        <AppBtn
          @click="toggleLoader"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          :isLoading="loading"
          >Loader</AppBtn
        >
      </div>
      <div class="column">
        <h3>Disabled</h3>
        <AppBtn
          isDisabled
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >Disabled Button</AppBtn
        >
      </div>
    </div>
    <div class="row">
      <div class="column"><h2>Colors</h2></div>
      <div class="column">
        <h4>coral</h4>
        <AppBtn
          color="coral"
          size="small"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >Three</AppBtn
        >
      </div>
      <div class="column">
        <h4>#65A</h4>
        <AppBtn
          color="#65A"
          size="large"
          shape="circle"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >Five</AppBtn
        >
      </div>
      <div class="column">
        <h4>#929EDD</h4>
        <AppBtn
          color="#929EDD"
          :size="42"
          shape="rounded"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >Seven</AppBtn
        >
      </div>
    </div>
    <div class="row">
      <div class="column"><h2>Sizing</h2></div>
      <div class="column">
        <h4>small</h4>
        <AppBtn
          size="small"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >Small</AppBtn
        >
      </div>
      <div class="column">
        <h4>medium</h4>
        <AppBtn
          size="medium"
          color="#4854A0"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >Medium btn</AppBtn
        >
      </div>
      <div class="column">
        <h4>large</h4>
        <AppBtn
          size="large"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >Large Btn</AppBtn
        >
      </div>
      <div class="column">
        <h4>custom (42px)</h4>
        <AppBtn
          :size="42"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >Custom Size</AppBtn
        >
      </div>
    </div>

    <div class="row">
      <div class="column"><h2>Shapes</h2></div>
      <div class="column">
        <h4>rectangle</h4>
        <AppBtn
          shape="rectangle"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >rectangle</AppBtn
        >
      </div>
      <div class="column">
        <h4>rounded</h4>
        <AppBtn
          shape="rounded"
          color="#4C58A9"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >rounded</AppBtn
        >
      </div>
      <div class="column">
        <h4>circle</h4>
        <AppBtn
          shape="circle"
          @click="onClick"
          @mouseover="onMouseOver"
          @mouseout="onMouseOut"
          @mousedown="onMouseDown"
          @mouseup="onMouseUp"
          >circle</AppBtn
        >
      </div>
    </div>
  </div>
  <div id="message-box" ref="messageBox"></div>
</template>

<style scoped>
.container {
  max-width: 1200px;
}

.row {
  display: flex;
  flex-wrap: wrap;
  margin: -10px;
}

.column {
  flex: auto;
  padding: 10px;
}
#message-box {
  position: absolute;
  width: auto;
  height: auto;
  top: 0;
  right: 0;
  padding: 10px;
  background: transparent;
  overflow: hidden;
}
.message {
  width: 250px;
  height: 75px;
  padding: 6px;
  position: absolute;
  opacity: 0;
  transition: all 0.3s ease;
}
</style>
