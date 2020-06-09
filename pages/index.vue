<template>
  <div>
    <v-card ref="form" max-width="500" class="mt-12 mx-auto">
      <v-card-text>
        <v-form>
          <v-text-field v-model="name" label="Name"></v-text-field>
          <v-spacer />
          <v-text-field v-model="no" label="Room ID"></v-text-field>
          <!-- <v-divider class="my-5"></v-divider> -->
          <v-layout align-center justify-center>
            <v-btn class="mr-4" @click="submit">submit</v-btn>
          </v-layout>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
// import axios from 'axios'
export default {
  data() {
    return {
      name: '',
      no: ''
    }
  },
  created() {},
  methods: {
    submit() {
      this.$router.push({
        name: 'room-no',
        params: { no: this.no, name: this.name }
      })
    }
  }
}
</script>

<style scoped></style>
