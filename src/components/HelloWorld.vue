<template>
    <h1>
        <button @click="do_action">push</button>
        {{info}}
    </h1>
</template>

<script>
import { ipcRenderer } from 'electron'

// import { ipcRenderer } from 'electron'
// window.ipcRenderer = ipcRenderer

export default {
    data() {
        return {
            info: ''   
        }
    },
    methods: {
        do_action(){
            this.info = 'Bienvenido a la version 1.4.4 ok!'
        }
    },
}
</script>

<style>

</style>