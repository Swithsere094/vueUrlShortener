<script>
import {ref} from 'vue';

export default {
  setup() {
    const input = ref("");
    const output = ref("123");
    const copyUrl = ref("");

    const copyValues = {
      'copy' : '/src/assets/copy.svg',
      'check' : '/src/assets/check.svg'
    }

    function shortUrl(){
      const endpoint = "https://api-ssl.bitly.com/v4/shorten";
      const apiKey = "7d6d4e0849de8510e46f87165a268b589088c978";
      const group_guid = "Bo3jklCQMpT";

      if(input.value != ""){
        const response = fetch(endpoint, {
          method: 'POST',
          headers: {
              'Authorization': `Bearer ${apiKey}`,
              'Content-Type': 'application/json'
          },
          body: JSON.stringify({ "long_url": input.value, "domain": "bit.ly", "group_guid": group_guid })
        }).then(response => response.json())
          .then(data => (output.value = data.link));

        const shortUrl = JSON.parse(response.getContentText()).link;

        output.value = shortUrl;
      }
    }

    function copyText(){
      if(output.value != ""){
        navigator.clipboard.writeText(output.value);

        copyUrl.value = copyValues.check;

        setTimeout(()=>{
          copyUrl.value = copyValues.copy;
        }, 2000)
      }
    }

    copyUrl.value = copyValues.copy;

    return {
      input,
      output,
      copyUrl,
      shortUrl,
      copyText,
    };
  }
}

</script>

<template>
  <div class="contenedor">
    <div class="input">
      <input placeholder="Ingrese su URL" class="inputEntrada" v-model="input" />
      <button v-on:click="shortUrl()">Acortar URL</button>
    </div>
    <!-- {{ output }} -->
    <div class="output">
      <input disabled type="text" v-model="output">
      <button v-on:click="copyText()"> <img :src="copyUrl"/> </button>
    </div>
  </div>
</template>

<style scoped>
  .contenedor{
    display:flex;
    flex-direction: column;
    height: 97vh;
    width:100%;
    align-items: center;
    justify-content: center;
  }
  .inputEntrada{
    width: 40vw;
    height: 40px;
    border: 1px solid #686868;
    border-radius: 5px;
    text-align: center;
    font-size: 20px;
  }
  .input{
    display: flex;
    flex-direction: column;
  }
  .input button {
    margin-top: 5px;
    height: 40px;
    background-color: #33b249;
    color:#fff;
  }
  .output input{
    background-color: white;
    margin-top: 15px;
    width: 34vw;
    height: 45px;
    margin-right: 1vw;
    text-align: center;
    font-size: 20px;
  }
  .output button{
    position: relative;
    top: 9px;
    width: 5vw;
    height: 50px;
    border-radius: 5px;
    background-color: #33b249;
    color: #fff ;
  }
  .output button img {
    width: 70%;
    height: 70%;
    filter: invert(100%) sepia(100%) saturate(0%) hue-rotate(150deg) brightness(101%) contrast(102%);
  }
</style>
