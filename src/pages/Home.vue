<template>
  <div class="text-center">
    <h1 class="text-4xl font-bold">Авторизация через TON Connect</h1>
    <button
      v-if="!connected"
      @click="connectWallet"
      class="mt-4 px-4 py-2 bg-blue-500 text-white rounded"
    >
      Подключить кошелек
    </button>
    <div v-else>
      <p class="mt-4 text-green-600">Вы успешно подключились!</p>
      <button @click="disconnectWallet" class="mt-4 px-4 py-2 bg-red-500 text-white rounded">
        Отключить кошелек
      </button>
    </div>
  </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
import { TonConnect } from '@tonconnect/sdk';

const connected = ref(false); // Состояние подключения
const tonConnect = new TonConnect();

const connectWallet = async () => {
  try {
    await tonConnect.connectWallet();  // Метод подключения кошелька
    connected.value = true;
  } catch (error) {
    console.error("Ошибка подключения:", error);
  }
};

const disconnectWallet = async () => {
  tonConnect.disconnect(); // Отключаем кошелек
  connected.value = false;
};

onMounted(() => {
  // Проверка, подключен ли кошелек при загрузке страницы
  connected.value = tonConnect.wallet?.isConnected || false;
});
  </script>
  