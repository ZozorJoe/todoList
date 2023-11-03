<template>
    <div class="container">
        <h3 class="text-center mt-3">To do List</h3>

        <div class="row">
            <div class="col-6 ">
                <div class="card">
                    <div class="card-header">
                        Complete
                    </div>
                    <div class="card-body">
                        {{ nbComplet }}
                    </div>
                </div>

            </div>
            <div class="col-6">
                <div class="card">
                    <div class=" card-header">
                        In progeress
                    </div>
                    <div class="card-body">
                        {{ nbInprogess }}
                    </div>
                </div>

            </div>
            <div class="col-6"></div>
        </div>
        <div class="d-flex mt-5">
            <input type="text" v-model="task" placeholder="Entrer tache" class="form-control w-100">
            <button class="btn btn-primary" @click="addTask">Ajouter</button>
        </div>
        <table class="table table-bordered mt-2">
            <thead>
                <tr>
                    <th scope="col">Task</th>
                    <th scope="col">Status</th>
                    <th scope="col">Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <td>{{ task.name }}</td>
                    <td>
                        <span v-if="task.status == 1"><span class="badge bg-success">Complete</span></span>
                        <span v-if="task.status == 0"><span class="badge bg-info">In progress</span></span>
                    </td>
                    <td class="d-flex justify-content-center">
                        <div @click="editTask(index)"><span class=" btn btn-primary fa fa-pen"></span></div>

                        <div @click="deleteTask(index)"><span class="btn btn-danger fa fa-trash mx-2"></span></div>
                        <div v-if="task.status == 0" @click="status(index)"><span
                                class="btn btn-success fa fa-check"></span>
                        </div>
                    </td>
                </tr>

            </tbody>
        </table>
    </div>
    <div v-if="OpenClose" class="modal fade show" tabindex="-1" aria-labelledby="exampleModalLabel" aria-modal="true"
        role="dialog" style="display:block">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header bg-primary">
                    Edit Task:  <span>{{ name }}</span>
                    <button type="button" @click="OpenCloseFun()" class="btn-close"></button>
                </div>
                <div class="modal-body">
                    <input type="text" class="form-control mb-2" v-model="name">
                    <textarea name="" id="" cols="30" rows="10" class="form-control" v-model="content"></textarea>
                </div>
                <div class="modal-footer">
                    <button class="btn btn-primary" @click="addTask">Save</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
export default {
    name: 'TodoApp',
    data() {
        return {
            task: "",
            edit: null,
            delete: null,
            tasks: [],
            nbComplet: 0,
            nbInprogess: 0,
            OpenClose: false,
            name:"",
            content:""
        }
    },

    methods: {
        editTask(index) {
            this.OpenClose = true
            this.name = this.tasks[index].name;
            this.content=this.tasks[index].content
            this.edit = index;

        },
        deleteTask(index) {
            this.$swal.fire({
                title: 'Are you sure?',
                text: "You won't be able to revert this!",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Yes, delete it!'
            }).then((result) => {
                if (result.isConfirmed) {
                    this.tasks.splice(index, 1)
                    this.count()
                    toast.success(
                        'Your to do has been deleted.'
                    )
                }
            });

        },
        addTask() {

            if (this.edit != null) {
                this.tasks[this.edit].name = this.name;
                this.tasks[this.edit].content = this.content;
                this.edit = null
                this.OpenClose=false
                toast.success("Modification avec Success  !", {
                    position: toast.POSITION.TOP_RIGHT,
                    autoClose: 5000,
                });
            }
            else {
                if (this.task != '') {
                    this.tasks.push({
                        name: this.task,
                        status: 0,
                        content:""
                    })
                    this.task = ""
                    this.count()

                    toast.success("ajout avec Success  !", {
                        position: toast.POSITION.TOP_RIGHT,
                        autoClose: 5000,
                    });
                }
                else {
                    toast.error("champ requierd", {
                        position: toast.POSITION.TOP_RIGHT,
                        autoClose: 5000,
                    });
                }

            }
        },
        status(index) {
            this.$swal.fire({
                title: 'Are you sure?',
                text: "You won't be able to revert this!",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Yes, Im sur!'
            }).then((result) => {
                if (result.isConfirmed) {
                    this.tasks[index].status = 1;
                    this.count()
                    toast.success("modification status avec Success  !", {
                        position: toast.POSITION.TOP_RIGHT,
                        autoClose: 5000,
                    });
                }
            });

        },
        count() {
            this.nbComplet = 0
            this.nbInprogess = 0
            this.tasks.forEach(element => {
                if (element.status == 0)
                    this.nbInprogess += 1
                else {
                    this.nbComplet += 1
                }

            });
        },
        OpenCloseFun() {
            this.OpenClose = false;
        },
    }
}

</script>