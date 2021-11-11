<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <form>
      <input v-model='formData.title' placeholder="请输入title" />
      <input v-model='formData.url' placeholder="请输入url" />
    </form>
    <a class="add_btn" @click='uploadIPFSJson(formData)'>add content</a>
    <ul>
      <li v-for="(item, index) in dataList" :key='index'>
        <span>{{item && item.ipfs_pin_hash}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "index",
  data() {
    return {
      msg: "IPFS DEMO",
      formData: {
        title: '',
        url: ''
      },
      config: {
        APIKey: "af973c1eb3a117c5136b",
        APISecret:
          "078dc5b024da3bece223fdbe595096a4a8607abeba6e598ad62ba7ced5c620e0",
        JWT:
          "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySW5mb3JtYXRpb24iOnsiaWQiOiJjN2NlOWVkZi01MjY3LTQzN2ItYjY1Ni0zMzg2N2E4ZmE5Y2MiLCJlbWFpbCI6IjE0MDYyNDM0NjRAcXEuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsInBpbl9wb2xpY3kiOnsicmVnaW9ucyI6W3siaWQiOiJGUkExIiwiZGVzaXJlZFJlcGxpY2F0aW9uQ291bnQiOjF9XSwidmVyc2lvbiI6MX0sIm1mYV9lbmFibGVkIjpmYWxzZX0sImF1dGhlbnRpY2F0aW9uVHlwZSI6InNjb3BlZEtleSIsInNjb3BlZEtleUtleSI6ImFmOTczYzFlYjNhMTE3YzUxMzZiIiwic2NvcGVkS2V5U2VjcmV0IjoiMDc4ZGM1YjAyNGRhM2JlY2UyMjNmZGJlNTk1MDk2YTRhODYwN2FiZWJhNmU1OThhZDYyYmE3Y2VkNWM2MjBlMCIsImlhdCI6MTYzNjQyODA3Nn0.Yd21lUlWYQqOTBTLK2UkSy982h8EgNEVjrXsTYcfcbo"
      },
      dataList: [],
      IPFS_ADDRESS: 'https://gateway.pinata.cloud/ipfs/'
    };
  },
  methods: {
    getIPFSJson(hash) {
      if (!hash) return
      axios.get(this.IPFS_ADDRESS + hash)
        .then(res => {
          console.log(res, '9999999')
        })
        .catch(e => {
          console.log(e)
        })
    },
    getAllData() {
      axios
        .get("https://api.pinata.cloud/data/pinList?status=pinned", {
          headers: {
            // "Authorization": config.JWT
            pinata_api_key: this.config.APIKey,
            pinata_secret_api_key: this.config.APISecret
          }
        })
        .then(res => {
          if (res.data) {
            // this.getIPFSJson(item.ipfs_pin_hash)
            this.dataList = res.data.rows
          }
        })
        .catch(e => {
          console.log(e, "eeeeeee")
        })
    },
    uploadIPFSJson (data) {
      axios.post('https://api.pinata.cloud/pinning/pinJSONToIPFS', data, {
        headers: {
          // "Authorization": config.JWT
          "pinata_api_key": this.config.APIKey,
          "pinata_secret_api_key": this.config.APISecret
        }
      })
      .then(res => {
        if (res.data) {
          this.getAllData()
          window.alert('Add Success!')
        }
      })
    }
  },
  created () {
    this.getAllData()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .add_btn {
    display: inline-block;
    margin-top: 20px;
    padding: 0 20px;
    height: 32px;
    line-height: 32px;
    color: #fff;
    background: #0f8c20;
    cursor: pointer;
    border-radius: 8px;
  }
</style>
