<template>
  <router-view />
  <p v-if="updateAvailable">Une nouvelle MAJ est dispo</p>
  <button v-if="canUpdateApp" @click="restartApp">Mettre à jour l'app</button>
</template>

<script>
import 'windi.css';
const { ipcRenderer } = require('electron');
export default {
  name: 'App',
  data() {
    return {
      updateAvailable: false,
      canUpdateApp: false,
    };
  },
  methods: {
    restartApp() {
      ipcRenderer.send('restart_app');
    },
  },
  created() {
    ipcRenderer.on('update_available', () => {
      console.log('UPDATE AVAILABLE');
      this.updateAvailable = true;
    });
    ipcRenderer.on('update_downloaded', () => {
      ipcRenderer.removeAllListeners('update_downloaded');
      this.canUpdateApp = true;
    });
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
</style>
