<script setup>
import Navbar from '@/components/DefaultNavbar.vue'
import Footer from '@/components/IndexFooter.vue'
const messages = ref([
    {
        role: 'AI',
        message: 'Hello! How can I help you?'
    },
    // {
    //     role: 'AI',
    //     message: 'Hello! How can I help you?1'
    // }
]);
const loading = ref(false);
const message = ref('');

const scrollToEnd = () => {
    setTimeout(() => {
        const chatMessages = document.querySelector('.chat-messages > div:last-child');
        chatMessages?.scrollIntoView({ behavior: 'smooth', block: 'end' });
    }, 100);
};

const sendPrompt = async () => {
    if (message.value === '') return;
    loading.value = true;

    messages.value.push({
        role: 'User',
        message: message.value
    });

    message.value = '';
    scrollToEnd();
    
    const lastMessage = messages.value[messages.value.length - 1].message;
    console.log(lastMessage)
    const res = await fetch(`http://localhost:8000/api/v1/chat-history`, {
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ 
            'user': '4bbec010100843a4b10c5098710e25f3',
            'chat_message': lastMessage
        }),
        method: 'post'
    });

    if (res.status === 200 || res.status === 201) {
        const response = await res.json();
        messages.value.push({
            role: 'AI',
            message: response?.system_answer
        });
    } else {
        messages.value.push({
            role: 'AI',
            message: 'Sorry, an error occurred.'
        });
    }

    loading.value = false;
    scrollToEnd();
};
</script>

<template>
    <Navbar />
    <div class="max-w-xl mx-auto text-black">
        <!-- <a href="https://vercel.com/templates/next.js/blob-sveltekit"
            class="flex justify-center px-10 py-2 mx-auto space-x-1 text-sm font-medium text-center text-gray-600 transition-all rounded-full shadow-sm group bg-white/30 ring-1 ring-gray-900/5 hover:shadow-lg active:shadow-sm">
            Deploy your own to Vercel
        </a> -->
        <h3 class="my-4 text-xl text-center text-black">BVH chatbot</h3>
        <h1 class="my-2 text-4xl font-bold text-center text-black">Your Personal AI Travel Advisor</h1>
        <div class="max-w-xl mx-auto">
            <div class="bg-white rounded-md shadow h-[60vh] flex flex-col justify-between">
                <div class="h-full overflow-auto chat-messages">
                    <div v-for="(message, i) in messages" :key="i" class="flex flex-col p-4">
                        <div v-if="message.role === 'AI'" class="pr-8 mr-auto">
                            <div class="p-2 mt-1 text-sm text-gray-700 bg-gray-200 rounded-lg text-smp-2">
                                {{ message.message }}
                            </div>
                        </div>
                        <div v-else class="pl-8 ml-auto">
                            <div class="p-2 mt-1 text-sm text-white bg-blue-400 rounded-lg">
                                {{ message.message }}
                            </div>
                        </div>
                    </div>
                    <div class="p-4 ml-10 mr-auto" v-if="loading">
                        <span class="loader"></span>
                    </div>
                </div>
                <form @submit.prevent="sendPrompt">
                    <div class="flex items-center w-full p-4">
                        <input v-model="message" type="text" placeholder="Type here..."
                            class="w-full p-1 text-sm text-black bg-transparent bg-gray-100 border rounded-md shadow border-white/40 grow" />
                        <button :disabled="loading" type="submit"
                            class="flex items-center justify-center flex-none w-10 h-10 ml-2 bg-green-500 rounded-full">
                            <svg width="24" height="24" viewBox="0 0 24 24" fill="none"
                                xmlns="http://www.w3.org/2000/svg">
                                <path d="M22 2L11 13" stroke="white" stroke-width="1.5" stroke-linecap="round"
                                    stroke-linejoin="round" />
                                <path d="M22 2L15 22L11 13L2 9L22 2Z" stroke="white" stroke-width="1.5"
                                    stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                        </button>
                    </div>
                </form>
            </div>
        </div>
        <div class="flex flex-col justify-center w-full my-4">
            <div class="flex items-center justify-center my-2">
                <span>Built with</span>
                <a href="https://openai.com/blog/gpt-3-apps"
                    class="flex items-center mx-1 font-medium underline transition-colors underline-offset-4 hover:text-black/70">
                    <p>gpt-3</p>
                </a>
                <span>and</span>
                <a href="https://nuxt.com/docs"
                    class="flex items-center font-medium underline transition-colors underline-offset-4 hover:text-black/70">
                    <!-- <img src="/nuxt.svg" class="h-6 mx-2" /> -->
                    <p class="h-6 mx-2">Nuxt</p>
                </a>
                .
            </div>


        </div>

        <!--  -->
        <div class="container mx-auto px-4">
            <div class="py-16">
                <div class="text-center">
                    <div class="max-w-lg mx-auto">
                        <h2 class="text-2xl font-semibold">Upgrade to Premium</h2>
                        <div class="mt-2"></div>
                        <div>Get unlimited access for $9.99 per month.</div>
                        <div class="mt-6"></div>
                        <div class="flex flex-col sm:flex-row justify-center space-y-4 sm:space-y-0 sm:space-x-4">
                            <div class="w-full sm:max-w-xs">
                                <a href="/sign-up"
                                    class="flex items-center justify-center gap-2 border border-gray-300 bg-white text-gray-700 text-center whitespace-nowrap rounded-md px-4 py-2.5 font-sans text-base font-semibold leading-6 no-underline transition-all duration-300 shadow-sm">Sign
                                    up for
                                    Free</a>
                            </div>
                            <div class="w-full sm:max-w-xs">
                                <a href="/pricing"
                                    class="flex items-center justify-center gap-2 border border-gray-300  whitespace-nowrap rounded-md px-4 py-2.5 font-sans text-base font-semibold leading-6 no-underline transition-all duration-300 bg-blue-600 text-white text-center py-2 rounded">Try
                                    Premium</a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>
    <Footer />
</template>

<style>
.loader {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    display: block;
    position: relative;
    color: #d3d3d3;
    box-sizing: border-box;
    animation: animloader 2s linear infinite;
}

@keyframes animloader {
    0% {
        box-shadow: 14px 0 0 -2px, 38px 0 0 -2px, -14px 0 0 -2px, -38px 0 0 -2px;
    }

    25% {
        box-shadow: 14px 0 0 -2px, 38px 0 0 -2px, -14px 0 0 -2px, -38px 0 0 2px;
    }

    50% {
        box-shadow: 14px 0 0 -2px, 38px 0 0 -2px, -14px 0 0 2px, -38px 0 0 -2px;
    }

    75% {
        box-shadow: 14px 0 0 2px, 38px 0 0 -2px;
    }
}
</style>