<template>    
<header class="bg-gray-800 py-4 px-3 text-white text-2xl">
    <nav class="w-full md:w-[80%] mx-auto flex justify-between items-center">
        <a href="" class="text-3xl">Github</a>
        <ul class="flex space-x-4">
            <li>Home</li>
            <li>About</li>
            <li>Contact</li>
        </ul>
    </nav>
</header>
<section class="min-h-screen bg-gray-900 flex justify-center items-center flex-col">

    <h1 class="text-center text-5xl text-white">Github Search</h1>
    <div class="w-full flex justify-center py-4">
        <input type="text" v-model="input" class="w-full md:w-[50%] mx-auto text-black py-4 px-6 rounded-md outline-none border-none text-3xl" placeholder="Search Profile" />
    </div>
    <div class="flex justify-end w-full md:w-[50%]">
        <button :disabled="input.length<3 || loading " @click="onClickHanlder" class="bg-indigo-500 hover:bg-indigo-600 duration-300 transition-all px-10 py-3 text-2xl text-white rounded-md mx-2 outline-none border-none">Search</button>
        <button @click="onClearHandler" v-if="value_Data" class="bg-red-500 hover:bg-red-600 duration-300 transition-all px-10 py-3 text-2xl text-white rounded-md mx-2 outline-none border-none">Clear</button>
    </div>
        <p v-if="loading" class="text-center text-2xl text-white">loading...</p>
    <div v-if="value_Data" class="flex justify-center text-white w-full md:w-[50%] mx-auto flex-col space-y-6 md:space-y-4">
                <img :src="value_Data?.avatar_url" alt="" class="w-[50%] md:w-[20%] rounded-full border-2 border-white mx-auto mb-3">
                <h1 class="text-4xl text-white text-center">
                    {{ value_Data?.login }} <span v-if="value_Data.name">
                    | {{ value_Data.name }}
                    </span>
                </h1>
                <a target="_blank" :href="value_Data.html_url" class="text-indigo-500 hover:text-white duration-300 transition-all text-center text-lg" >Visit Profile</a>
                <p class="text-2xl text-center">{{ value_Data?.bio }}</p>
    </div>

</section>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

        const input = ref('')
        const value_Data = ref(null);
        const loading = ref(false);

        const onClickHanlder = async()=>{
            if(input.value.length<3){
                return false
            }
            value_Data.value = null;
            // alert("this is run")
            try {
                loading.value= true
                const res = await axios.get(`https://api.github.com/users/${input.value}`)
                const data = await res.data;
                input.value = '';
                value_Data.value = data;
            loading.value= false
            } catch (error) {
            loading.value= false
                alert(`Error: ${error.response.data.message}`);
            }
        }

        const onClearHandler = ()=>{
            value_Data.value = null
        }
</script>

<style lang="scss" scoped>

</style>