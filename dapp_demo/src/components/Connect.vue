<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click = "get">connect wallet</button>
    <button @click="getETH">获取账户信息</button>
    <button @click="getTransfer">ETH转帐</button>
    <button @click="getTokenBalance">代币余额</button>
    <button @click="getTokenTransfer">转账代币</button>
    <button @click="getAllowance">查询授权金额</button>
    <button @click="getApprove">授权</button>

  </div>
</template>

<script>
  import Web3 from 'web3';
  import abi from '../abi/ERC20.json';
export default {
  name: 'myConnect',
  props: {
    msg: String
  },
  data(){
    return{};
  },
  mounted(){},
  methods:{
    getData(){
      console.log('first')
    },
    get() {  // 唤起钱包
      if (window.ethereum) {
        window.ethereum.enable().then((res) => {
          alert("当前钱包地址:" + res[0]);
        });
      } else {
        alert("请安装MetaMask钱包");
      }
    },
    async getETH() {
      let web3 = new Web3(window.web3.currentProvider)
      console.log(web3)
      // console.log(web3.eth.getAccounts())
      let fromAddress = await web3.eth.getAccounts()
      console.log(web3.eth.getBalance(fromAddress[0]))
      console.log(fromAddress)
      web3.eth.getBalance(fromAddress[0],(err, res) => {
        if (!err) {
            alert("ETH余额=="+res/Math.pow(10,18));
            //console.log(res)
        }
      })
    },
    //ETH转账
    async getTransfer(){
      let web3 = new Web3(window.web3.currentProvider)
      let fromAddress = await web3.eth.getAccounts()
      let amount = 0.01*Math.pow(10,18);
      let toAddress = "0x1c84B5cB3Ab48Bd5314C950716f1E7017b0a1023";
      web3.eth.sendTransaction({
        gas: 21000,
        gasPrice: 5000000000,
        from: fromAddress[0],
        to: toAddress,
        value: amount
      }, (err, result) => {
        console.log("转账Hash=",result)
      })
    },
    //查询代币余额
    async getTokenBalance(){
      if(window.web3) {
        var web3 = web3 = new Web3(window.web3.currentProvider);
        let fromAddress = "0xd7a2CaB0EA71281af0DF030b5e0765109cdD031F";//查询用户地址
        let ethContract = new web3.eth.Contract(abi,"0x0bba3F3179A384F51990412731a330D6963a2331") //所有代币的abi可以通用（abi,合约地址）
        let balance = await ethContract.methods.balanceOf(fromAddress).call()
        alert(balance)
 
      }
    },
    //直接转账充币地址
    async getTokenTransfer(){
      if(window.web3) {
        let web3 = new Web3(window.web3.currentProvider)
         let ethContract = new web3.eth.Contract(abi,"0x0bba3F3179A384F51990412731a330D6963a2331") //所有代币的abi可以通用（abi,合约地址）
        //转账数量
        let amount = 100*Math.pow(10,18);//转账100个
        //小狐狸账户
        let fromAddress = await web3.eth.getAccounts()
        //接收地址
        let toAddress = "0x1c84B5cB3Ab48Bd5314C950716f1E7017b0a1023";
        ethContract.methods.transfer(toAddress,amount+'').send({ from: fromAddress[0] })
    }
 
  },
  //查询授权金额
    async getAllowance(){
      if(window.web3) {
        let web3 = new Web3(window.web3.currentProvider)
        let fromAddress = "0xd7a2CaB0EA71281af0DF030b5e0765109cdD031F";//查询地址
        let ethContract = new web3.eth.Contract(abi,"0x0bba3F3179A384F51990412731a330D6963a2331") //所有代币的abi可以通用（abi,合约地址）
        let toAddress = "0xd7a2CaB0EA71281af0DF030b5e0765109cdD031F";//被授权地址
        let balance = await ethContract.methods.allowance(fromAddress,toAddress).call()
        alert("授权金额"+balance/Math.pow(10,18))
 
      }
    },
 
    //授权
    async getApprove(){
      if(window.web3) {
        let web3 = new Web3(window.web3.currentProvider)
        let ethContract = new web3.eth.Contract(abi,"0x0bba3F3179A384F51990412731a330D6963a2331") //所有代币的abi可以通用（abi,合约地址）
        //授权数量
        let amount = 100*Math.pow(10,18);//转账100个
        let toAddress = "0xd7a2CaB0EA71281af0DF030b5e0765109cdD031F";//被授权地址
        //小狐狸账户
        let fromAddress = await web3.eth.getAccounts()
        ethContract.methods.approve(toAddress,amount+'').send({ from: fromAddress[0] })
      }
    }
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
