

<template>
  <main class="pt-10">
    <h1 class="text-5xl py-5">Hello, Web3!</h1>
    <p class="text-5xl py-5">
      <span class="text-7xl">
        <i class="bx bxs-hand"></i>
      </span>
      <span>{{ count }}</span>
    </p>
    <p class="py-5" v-if="accountAddress">{{accountAddress.slice(0,4)}}...{{accountAddress.slice(-4)}}</p>
    <p class="py-5">
      <button
        v-if="accountAddress"
        class="bg-slate-800 py-4 px-7 rounded-full transition active:opacity-80 text-2xl hover:bg-stone-800"
      >招手</button>
      <button
      @click="connectWallet"
        v-else
        class="bg-slate-800 py-4 px-7 rounded-full transition active:opacity-80 text-2xl hover:bg-stone-800"
      >連結</button>
    </p>
  </main>
</template>
<script>
import { onBeforeMount, ref } from 'vue'
import { ethers } from 'ethers'

export default {
  setup() {
    const count = ref(0)
    const accountAddress = ref(null)

    const connectWallet = async () => {
      try {
        const { ethereum } = window
        if (ethereum) {
          console.log(`已安裝錢包`)
        } else {
          console.log(`尚未安裝錢包`)
        }
        const accounts = await ethereum.request({ method: 'eth_requestAccounts' })
        if (accounts.length > 0) {
          const account = accounts[0]
          accountAddress.value = account
        } else {
          console.log(`尚未授權`)
        }

      } catch (error) {
        console.error(error)
      }
    }

    const checkIfWalletIsConnected = async () => {
      try {
        const { ethereum } = window
        if (ethereum) {
          console.log(`已安裝錢包`)
        } else {
          console.log(`尚未安裝錢包`)
        }
        const accounts = await ethereum.request({ method: 'eth_accounts' })
        if (accounts.length > 0) {
          const account = accounts[0]
          accountAddress.value = account
        } else {
          console.log(`尚未授權`)
        }
      } catch (error) {
        console.error(error)
      }
    }

    onBeforeMount(checkIfWalletIsConnected)

    return {
      count,
      accountAddress,
      connectWallet
    }
  }
}

</script>