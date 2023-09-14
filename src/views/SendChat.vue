<template>
  <div>
    <div class="container mt-3">
			<router-link to="/" class="btn btn-primary">Home</router-link>

      <h1 class="mb-5">Send Chat</h1>

      <form @submit.prevent="sendMessage">
        <div class="form-floating mb-2">
          <select class="form-select" id="floatingSelect" aria-label="Floating label select example" v-model="formData.id">
            <option selected disabled>Open this select menu</option>
            <option v-for="device in devices" :key="device.id" :value="device.id">{{ device.name }}</option>
          </select>
          <label for="floatingSelect">Select Device</label>
        </div>

        <div class="form-floating mb-2">
          <input type="number" class="form-control" id="floatingText" placeholder="Number" v-model="formData.number">
          <label for="floatingText">Number</label>
        </div>

        <div class="form-floating mb-2">
          <textarea class="form-control" placeholder="Message" id="floatingTextarea" rows="20" style="height: 200px" v-model="formData.message"></textarea>
          <label for="floatingTextarea">Message</label>
        </div>

        <button type="submit" class="btn btn-primary">Send</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      devices: {},
      formData: {
        number: '',
        message: '',
        id: ''
      }
    }
  },
  methods: {
    sendMessage() {
      axios.post(`whatsapp/chat/send`, this.formData)
        .then(res => {
          console.log(res.data);
          this.clearForm();
        })
    },
    getDevices() {
			axios.get('device')
				.then(res => {
					this.devices = res.data;
				})
		},
    clearForm() {
      this.formData.number = '';
      this.formData.message = '';
      this.formData.id = '';
    }
  },
  created() {
    this.getDevices();
  }
}
</script>