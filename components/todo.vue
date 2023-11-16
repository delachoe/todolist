<template>
    <div class="container  mx-auto">

        <div>
         <h1 class="flex text-[55px] font-bold justify-center text-gray-300 pt-5 font-serifs">ToTo's Todolist</h1>
         </div>

         
      <div class=" grid grid-cols-2  gap-12 pt-12">
        
         <div  :class="{'bg-white border border-gray-900 w-full h-full rounded-lg':showEditForm, 'flex justify-center mx-[250px] my-6 bg-white border w-full h-full rounded-lg':!showEditForm}">
                
                <div class="mt-24 mx-4 flex justify-evenly" >
                    <div>
                        <h1 class="text-[35px] font-bold text-gray-900 mb-4">{{ todo.title }}</h1>
                        <p class="text-[20px] text-black mb-4">{{ todo.description }}</p>
                        <p class="text-sm text-slate-600 text-right ">{{ todo.dateTime }}</p>

                        <div class="mt-16 flex  justify-center space-x-12">
                            <button @click="editTodo(todo.id)" class="text-base text-white px-10 py-1 border bg-gray-900 rounded-2xl ">Edit</button>
                            <button @click="deleteTodo(todo.id)"   class="text-base text-white px-10 py-1 border bg-gray-900 rounded-2xl ">Delete</button> 
                    </div>
                    </div>
                </div>
        </div>

            <div class="bg-gray-900 border w-full h-full rounded-lg" v-if="showEditForm">

                <h1 class="flex text-xl font-bold justify-center my-8 text-white">Edit Task</h1>

                <form id="new" @submit.prevent="updateTodo()" >
                    <input type="text" id="title" v-model="todo.title" required="">

                    <textarea maxlength="400" class=" text-gray-700 flex border border-slate-400 rounded-lg w-80 h-24 mb-8 ml-6 p-2 resize-none placeholder:text-slate-400 " name="description" form="new"  v-model="todo.description" required=""></textarea>
                    <p v-if="error.description" class="text-sm text-red-600 mb-8 ml-6">*{{  this.error.description }}</p>

                    <input type="datetime-local" id="dateTime" placeholder="date" min="2023-10-01T00:00:00" v-model="todo.dateTime" required="">
                    <button class="inline-block mb-4 mx-44 text-base text-white px-10 py-1 border bg-gray-900 rounded-2xl">Done</button>
                </form>

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
                todoid:'',
                showEditForm: false,
                todo:{},
                error:{}
            }
        },

        methods: {
            async getTodo(){
                await axios.get(`http://localhost:8080/api/v1/todo/${this.todoid}`,)
                .then((res)=>
                {
                    this.todo = res.data;
                    console.log(res);
                });
        },

        async editTodo(index){
                this.showEditForm = true;
        },

        async updateTodo(){

                this.error = {};
                if (!this.description) {
                    this.error.description = "maximum of 400 characters";
                }

                await axios.put(`http://localhost:8080/api/v1/todo/${this.todoid}`,{
                    id:this.todoid,
                    title: this.todo.title,
                    dateTime: this.todo.dateTime,
                    description: this.todo.description,
                }).then((res)=>
                {
                    const data = res.data;
                    console.log(data);
                    alert("Data Updated");
                    this.showEditForm= false;                 
                });
        },

        async deleteTodo(){
                await axios.delete(`http://localhost:8080/api/v1/todo/${this.todoid}`,)
                .then((res)=>
                {
                    this.todo = res.data;
                    console.log(res);
                    this.$router.push('/');
                });
        },
    },
        mounted(){
            this.todoid = this.$route.params.id;
            this.getTodo(this.$route.params.id);
        }
    }
</script>