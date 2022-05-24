<template>
  <q-page class="bg-grey-3">
      <div class="row">
          <q-input v-model="forward" placeholder="forward"/>
          <q-btn @click="drive">Drive</q-btn>
          <div>
            {{forwardFromTopic}}
          </div>
      </div>
  </q-page>
</template>
<script>
import { client } from "../boot/mqtt-boot"
export default {
  created () {
    client.on("connect", () => {
      console.log("Conntected!")
      client.subscribe("vichak/forward", function (err) {
      })
    })
    client.on("message", (topic, message) => {
      console.log(`${topic} - ${message.toString()}`)
      this.forwardFromTopic = message.toString()
    })
  },
  data () {
    return {
      forward: 0,
      forwardFromTopic: 0
    }
  },
  methods: {
    drive () {
      client.publish("vichak/forward", this.forward)
    },
  },
}
</script>
