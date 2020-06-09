<template>
  <div>
    Room #{{ $route.params.no }}
    <v-divider class="ma-4"></v-divider>
    <v-row align="start" justify="start">
      <v-card class="ml-10" width="250" tile>
        <v-list>
          <v-subheader>Players</v-subheader>
          <v-list-item-group color="primary">
            <v-list-item v-for="user in users" :key="user.id">
              <v-list-item-content>
                <v-list-item-title align="center" justify="center"
                  >{{ user.name }}
                  {{
                    user.name == $route.params.name ? '(you)' : ''
                  }}</v-list-item-title
                >
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-card>
      <v-card class="ml-10" max-width="400" align="center" justify="center">
        <v-container class="pa-5">
          <h2 v-show="running" align="center" justify="center">
            Topic: {{ info.topic }}
          </h2>
          <h3 v-show="running" align="center" justify="center">
            Your word is<br />
            <h2>{{ info.word }}</h2>
          </h3>
        </v-container>
        <v-btn
          v-show="!running"
          class="pa-5 ma-5"
          align="center"
          justify="center"
          @click="start"
          >Start</v-btn
        >
        <v-btn
          v-show="running"
          class="pa-5 ma-5"
          align="center"
          justify="center"
          @click="stop"
          >Stop</v-btn
        >
      </v-card>
      <v-card
        v-show="running"
        class="ml-10"
        max-width="400"
        align="center"
        justify="center"
      >
        <v-container class="pa-5">
          <h2>
            First player <br />
            {{ info.first }}
          </h2>
        </v-container>
      </v-card>
    </v-row>

    <v-divider class="ma-4"></v-divider>
    <v-card-actions class="ml-auto">
      <v-btn @click="exit">Exit</v-btn>
    </v-card-actions>
  </div>
</template>

<script>
import io from 'socket.io-client'
let socket = 0
export default {
  data() {
    return {
      users: [],
      info: 'none',
      running: false
    }
  },
  created() {
    const ENDPOINT = 'https://tell-us-boardgame.herokuapp.com/'
    socket = io(ENDPOINT)
    socket.emit('join', {
      name: this.$route.params.name,
      room: this.$route.params.no
    })
    socket.on('update', (users) => {
      this.users = users
    })
    socket.on('startrun', (data) => {
      this.info = data
      this.running = true
    })
    socket.on('stoprun', () => {
      this.running = false
    })
  },
  methods: {
    exit() {
      socket.disconnect()
      this.$router.push({
        name: 'index'
      })
    },
    start() {
      socket.emit('start', this.$route.params.no)
    },
    stop() {
      socket.emit('stop', this.$route.params.no)
    }
  }
}
</script>

<style></style>
