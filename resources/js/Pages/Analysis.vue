<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import { reactive, onMounted } from 'vue';
import { getToday } from '@/common';
import axios from 'axios';

onMounted(() => {
    form.startDate = getToday()
    form.endDate = getToday();

    // axios.get('/api/user')
    // .then( res => {
    //     console.log(res)
    // })
})

const form = reactive({
    startDate: null,
    endDate: null,
    type: 'perDay'
})

const getDate = async () => {
    try {
        const response = await axios.get('/api/analysis', {
            withCredentials: true,
            params: {
                startDate: form.startDate,
                endDate: form.endDate,
                type: form.type
            }
        });
        console.log(response.data);
    } catch (error) {
        console.error(error);
    }
};

</script>

<template>
    <Head title="データ分析" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">データ分析</h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <form @submit.prevent="getDate">
                            From: <input type="date" name="startDate" v-model="form.startDate">
                            To: <input type="date" name="endDate" v-model="form.endDate"><br>
                            <button class="mt-4 flex mx-auto text-white bg-indigo-500 border-0 py-2 px-8 focus:outline-none hover:bg-indigo-600 rounded text-lg">分析する</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
