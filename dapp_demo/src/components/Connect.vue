<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click = "get()">connect wallet</button>
  </div>
</template>

<script>
import Vue from 'vue';

  let {ref} =Vue
export default {
  name: 'myConnect',
  setup(){
    let get = ref('test')
    get(() =>{
        if(window.ethereum) {
        window.ethereum.enable().then((res)=>{
          alert("wallet address" + res[0]);
        });
      } else {
        alert("plz install metamask");
      }
    })
    return {get}
  },
  props: {
    msg: String
  },
  data(){
    return{};
  },
  mounted(){},
  methods(){
 
 
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
