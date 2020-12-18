<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome new user"/>
    <p id="version">{{version}}</p>
    <button @click="do_action">Buscar actualizaciones</button>
    <div id="notification" :class="[noti_hidden == true ? 'hidden': '']">
      <p id="message">{{message}}</p>
      <button id="close-button" @click="closeNotificaction">Close</button>
      <button id="restart-button" @click="restartApp()" :class="[btn_restart == true ? 'hidden': '']">Restart</button>
    </div>
  </div>
</template>

<script>
import { ipcRenderer } from 'electron'
window.ipcRenderer = ipcRenderer

const version = document.getElementById('version');
const notification = document.getElementById('notification');
const message = document.getElementById('message');
const restartButton = document.getElementById('restart-button');

import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      version: '',
      message: '',
      noti_hidden: true,
      btn_restart: true
    }
  },
  methods: {
    test(){
      this.noti_hidden = !this.noti_hidden
    },
    do_action(){

      ipcRenderer.send('app_version');

      ipcRenderer.on('app_version', (event, arg) => {
        ipcRenderer.removeAllListeners('app_version');
        this.version = arg.version
      });

      ipcRenderer.on('update_available', ()=>{
        ipcRenderer.removeAllListeners('update_available');
        this.message = 'A new update is available. Downloading now...';
        this.noti_hidden = false
      })

      ipcRenderer.on('update_downloaded', () => {
          ipcRenderer.removeAllListeners('update_downloaded');
          this.message = 'Update Downloaded. It will be installed on restart. Restart now?';
          this.btn_restart = false
          this.noti_hidden = false
      });
    },
    closeNotificaction(){
      this.noti_hidden = true
    },
    restartApp(){
      ipcRenderer.send('restart_app');
    }
  },
  mounted() {
    //this.do_action()
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#notification {
        position: fixed;
        bottom: 20px;
        left: 20px;
        width: 200px;
        padding: 20px;
        border-radius: 5px;
        background-color: white;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    }
.hidden {
  display: none;
}

</style>
