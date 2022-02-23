

<template>
  <main class="pt-10">
    <h1 class="text-5xl py-5">Hello, Web3!</h1>
    <section class="py-5" v-if="accountAddress">
    <p class="text-5xl py-5">
      <span class="text-7xl">
        <i class="bx bxs-hand"></i>
      </span>
      <span>{{ count }}</span>
    </p>
      <p>Wallet Address:</p>
      <p>{{ accountAddress.slice(0, 4) }}...{{ accountAddress.slice(-4) }}</p>
    </section>

    <p class="py-5">
      <button
      @click="countOnce"
        v-if="accountAddress"
        class="bg-slate-800 py-4 px-7 rounded-full transition active:opacity-80 text-2xl hover:bg-stone-800"
      >招手</button>
      <button
        @click="connectWallet"
        v-else
        class="bg-slate-800 py-4 px-7 rounded-full transition active:opacity-80 text-2xl hover:bg-stone-800"
      >連結錢包</button>
    </p>
  </main>
</template>
<script>
import { onBeforeMount, onMounted, ref } from 'vue'
import { ethers } from 'ethers'
import abi from '../contract/Counter.json'
const contractAddress = `0xe7f1725e7734ce288f8367e1bb143e90bb3f0512`
const contractABI = abi.abi

export default {
  setup() {
    const count = ref(0)
    const accountAddress = ref(null)

    const countOnce = async () => {
      try {
        const { ethereum } = window
        if (ethereum) {
          console.log(`已安裝錢包`)
        } else {
          console.log(`尚未安裝錢包`)
        }
        const provider = new ethers.providers.Web3Provider(ethereum)
        const signer = provider.getSigner()
        const CounterContract = new ethers.Contract(
          contractAddress,
          contractABI,
          signer
        )
        let tx = await CounterContract.add()
        await tx.wait()
        await getCounts()
      } catch (error) {
        console.error(error)
      }
    }

    const getCounts = async () => {
      try {
        const { ethereum } = window
        if (ethereum) {
          console.log(`已安裝錢包`)
        } else {
          console.log(`尚未安裝錢包`)
        }
        const provider = new ethers.providers.Web3Provider(ethereum)
        const signer = provider.getSigner()
        const CounterContract = new ethers.Contract(
          contractAddress,
          contractABI,
          signer
        )
        const counts = await CounterContract.getCounts()
        count.value = parseInt(counts)
      } catch (error) {
        console.error(error)
      }
    }

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
          await getCounts()
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
    onMounted(getCounts)

    return {
      count,
      accountAddress,
      connectWallet,
      countOnce
    }
  }
}

</script>