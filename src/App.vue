<template>
  <div v-if="isAuth" class="flex flex-col gap-2 p-4 text-sm h-screen">
    <div>
      <Select v-model="sessionType" :options="typeOptions" class="w-full border border-gray-300 rounded-sm h-10 px-4 bg-gray-100 outline-indigo-500"/>
    </div>
    <div>
      <input v-model="code" @input="handleAddItem" placeholder="Chạm vào đây để quét" class="w-full border border-gray-300 rounded-sm h-10 px-4 bg-gray-100 outline-indigo-500" />
    </div>
    <div class="flex-grow overflow-y-auto">
      <ListItem :items="listItems" :contentType="sessionType" />
    </div>
    <div class="flex flex-row gap-1">
      <Select v-model="transport" :options="transportOptions" class="flex-1 border border-gray-300 rounded-sm h-10 px-4 bg-gray-100 outline-indigo-500"/>
      <Select v-model="goodsStatus" :options="goodsStatusOptions" class="flex-1 border border-gray-300 rounded-sm h-10 px-4 bg-gray-100 outline-indigo-500"/>
    </div>
    <div>
      <textarea v-model="note" placeholder="Nhập ghi chú" class="w-full border border-gray-300 rounded-sm h-10 px-4 py-2 bg-gray-100 outline-indigo-500 resize-none" />
    </div>
    <div class="flex items-center justify-center gap-2">
      <button @click="handleReset" class="bg-yellow-500 focus:bg-yellow-400 text-white px-4 py-2 rounded-sm flex-1">Làm lại</button>
      <button @click="handleCloseSession" class="bg-red-500 focus:bg-red-400 text-white px-4 py-2 rounded-sm flex-1">Đóng phiên</button>
    </div>
  </div>
  <div v-else class="flex items-center justify-center h-screen bg-slate-200 text-sm">
    <div class="flex flex-col gap-2 w-full max-w-md bg-white p-4 rounded-md shadow-md">
      <h1 class="text-2xl font-bold text-center mb-8">Đăng nhập</h1>
      <div v-if="errorMessage" class="text-red-500 text-center mb-4">{{ errorMessage }}</div>
      <input v-model="email" type="email" placeholder="Email" class="border border-gray-300 rounded-sm h-10 px-4 bg-gray-100 outline-indigo-500" />
      <input v-model="password" type="password" placeholder="Mật khẩu" class="border border-gray-300 rounded-sm h-10 px-4 bg-gray-100 outline-indigo-500" />
      <button @click="handleAuth" class="bg-blue-500 focus:bg-blue-400 text-white px-4 py-2 rounded-sm flex-1 my-4">Đăng nhập</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import Select from "./components/kits/select/index.vue";
import ListItem from "./components/kits/list/index.vue";
import { typeOptions, items, transportOptions, goodsStatusOptions } from "./config";


const sessionType = ref(typeOptions[0].value);
const code = ref("");
const listItems = ref([]);
const transport = ref(transportOptions[0].value);
const goodsStatus = ref(goodsStatusOptions[0].value);
const note = ref("");
const timeout = ref(null);
const isAuth = ref(false);
const email = ref("");
const password = ref("");
const errorMessage = ref("");

const handleReset = () => {
  listItems.value = [];
  code.value = "";
  transport.value = transportOptions[0].value;
  goodsStatus.value = goodsStatusOptions[0].value;
  note.value = "";
};

const handleCloseSession = () => {
  let data = {
    listItems: listItems.value,
    code: code.value,
    transport: transport.value,
    goodsStatus: goodsStatus.value,
    note: note.value,
  };
  console.log(data);
};

const handleAddItem = () => {
  clearTimeout(timeout.value);
  timeout.value = setTimeout(() => {
    if (code.value.trim()) {
      listItems.value.unshift(code.value);
      code.value = "";
    }
  }, 500);
};

const handleAuth = () => {
  if (email.value && password.value) {
    axios.post("http://localhost:3000/login", {
      email: email.value,
      password: password.value,
    }).then((res) => {
      if (res.data.token) {
        isAuth.value = true;
        console.log(res.data.token);
      }
    }).catch((err) => {
      errorMessage.value = err.response.data.message;
      console.log(err.response.data.message);
    });
  }
};
</script>

