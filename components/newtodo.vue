<template>
    <div class="container mx-auto">

        <div>
         <h1 class="flex text-[55px] font-bold justify-center text-gray-300 pt-5 font-serifs">ToTo's Todolist</h1>
         </div>

        <div class="grid grid-cols-2  gap-x-32 pt-8">
            <div class="bg-white border w-full h-full rounded-lg ">

                <h1 class="flex text-2xl font-bold justify-center my-8 text-gray-900">New Task</h1>

                <form ref_for="new" @submit.prevent="addPost()">
                    <input type="text" id="title" placeholder="Title" required="" v-model="title">
                   
                    <textarea maxlength="400" class="text-black flex border border-slate-400 rounded-lg w-80 h-24 ml-6 p-2 resize-none placeholder:text-slate-400 " name="description" form="new" placeholder="Description" required="" v-model="description"></textarea>
                    <p v-if="errors.description" class="text-sm text-red-600 mb-8 ml-6">*{{  this.errors.description }}</p>

                    <input type="datetime-local" id="dateTime" placeholder="date"  min="2023-10-01T00:00:00" v-model="dateTime" required="">

                    <button class="text-base text-white mx-44 mb-4 px-10 py-1 border bg-gray-900 rounded-2xl"  >Add</button>
                </form>
            </div>

            <div  class="bg-gray-900 border w-full h-full rounded-lg">
                
                <div class="flex justify-center my-8 mx-3 sticky">

                <h1 class="text-3xl font-bold text-white">Tasks</h1>

                <input type="text" placeholder="search" id="search" class="text-sm text-black my-1 p-1 w-24 font-serifs">

            </div>

                <div class="m-6 overflow-y-scroll h-[300px] " >
                    <ul role="list" class="divide-y divide-white">
                        <li v-for="(post,index) in posts" :key="index">

                            <div class="flex justify-evenly my-4 space-x-16">
                                <input type="checkbox" id="yes">
                                <NuxtLink :to="`/todos/${post.id}`" class="text-lg text-gray-300 hover:text-white hover:font-bold">{{ post.title }}</NuxtLink>
                                <p class="text-base text-gray-400">{{ post.dateTime }}</p>
                            </div>

                        </li>
                    </ul>
                </div>

            </div>
        </div>
    </div>
</template>


<script>
    import axios from 'axios'
    export default {
        data(){
            return {
                title: "",
                dateTime:"",
                description:"",
                posts:{},
                errors:{}
            }
        },

        methods: {
            async getPosts(){

                this.errors = {};
                if (!this.description) {
                    this.errors.description = "maximum of 400 characters";
                }
                
                await axios.get(`http://localhost:8080/api/v1/todo`,)
                .then((res)=>
                {
                    this.posts = res.data;
                    console.log(res);
                });
        },

        async addPost(){
                


                await axios.post('http://localhost:8080/api/v1/todo',{
                    title: this.title,
                    dateTime: this.dateTime,
                    description: this.description,
                }).then((res)=>
                {
                    const data= res.data;
                    console.log(res.data);

                    if( res.data==200)
                    {

                      alert("Task Added");  
                      window.location.reload();
                    }
                     else{
                        alert("Error in Adding Task");
                     }

                     this.$refs.new.reset();
                });
        },
    },

        mounted(){
            this.getPosts();
        }
    }
</script>