<template>
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <div class="panel panel-default">
                <div class="panel-heading">
                    <button class="btn btn-primary btn-xs pull-right" v-on:click="addTask()">
                        + Añadir Tarea
                    </button>
                   Mis Tareas</div>
                <div class="panel-body">
                    <table class="table table-bordered table-striped table-responsive" v-if="tasks.length > 0">
                        <tbody>
                            <tr>
                                <th>
                                    No.
                                </th>
                                <th>
                                    Name
                                </th>
                                <th>
                                    Description
                                </th>
                                <th>
                                    Action
                                </th>
                            </tr>
                            <tr v-for="(task, index) in tasks" v-bind:key="task.id">
                                <td>{{ index + 1 }}</td>
                                <td>{{ task.name }}</td>
                                <td>{{ task.description }}</td>
                                <td>
                                <button @click="iniUpdate(index)" class="btn btn-success btn-xs">Edit</button>
                                <button @click="deleteTask(index)" class="btn btn-danger btn-xs">Delete</button>
                                </td>
                            </tr>
                        </tbody>
                     </table>                      
                    </div>
                </div>
            </div>
        </div>
    
    <div class="modal fade" tabindex="-1" role="dialog" id="add_task_model">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title">Añadir Nueva Tarea</h4>
                </div>
                <div class="modal-body">

                     <div class="alert alert-danger" v-if="errors.length > 0">
                            <ul>
                                <li v-for="error in errors" :key="error">{{ error }}</li>
                            </ul>
                        </div>

                    <div class="form-group">
                        <label for="name">Nombre</label>
                        <input type="text" name="name" id="name" placeholder="Nombre Tarea" class="form-control" v-model="task.name">
                    </div>

                     <div class="form-group">
                        <label for="name">Descripcion</label>
                        <textarea name="descripcion" id="descripcion" placeholder="Descripcion Tarea" class="form-control" cols="30" row="3" v-model="task.description"></textarea>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary" @click="createTask()">Enviar</button>
                </div>
            </div>
        </div>
    </div>
     <div class="modal fade" tabindex="-1" role="dialog" id="update_task_model">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title">Actualizar Tarea</h4>
                </div>
                <div class="modal-body">

                     <div class="alert alert-danger" v-if="errors.length > 0">
                            <ul>
                                <li v-for="error in errors" :key="error">{{ error }}</li>
                            </ul>
                        </div>
<div class="modal fade" tabindex="-1" role="dialog" id="update_task_model">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title">Actualizar Tarea</h4>
                </div>
                <div class="modal-body">

                     <div class="alert alert-danger" v-if="errors.length > 0">
                            <ul>
                                <li v-for="error in errors" :key="error">{{ error }}</li>
                            </ul>
                        </div>

                    <div class="form-group">
                        <label for="name">Nombre</label>
                        <input type="text" name="name" id="name" placeholder="Nombre Tarea" class="form-control" v-model="update_task.name">
                    </div>

                     <div class="form-group">
                        <label for="name">Descripcion</label>
                        <textarea name="descripcion" id="descripcion" placeholder="Descripcion Tarea" class="form-control" cols="30" row="3" v-model="update_task.description"></textarea>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary" @click="updateTask()">Enviar</button>
                </div>
            </div>
        </div>
    </div>
                    <div class="form-group">
                        <label for="name">Nombre</label>
                        <input type="text" name="name" id="name" placeholder="Nombre Tarea" class="form-control" v-model="update_task.name">
                    </div>

                     <div class="form-group">
                        <label for="name">Descripcion</label>
                        <textarea name="descripcion" id="descripcion" placeholder="Descripcion Tarea" class="form-control" cols="30" row="3" v-model="update_task.description"></textarea>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary" @click="updateTask()">Enviar</button>
                </div>
            </div>
        </div>
    </div>
</div>  


</template>
<script>
export default {
    mounted(){
        console.log('Mounted Component.')
        this.readTasks();
    },
    data(){
        return {
            task: {
                name:'',
                description:'',
            },
            errors:[],
            tasks: [],
            update_task: {}
        }
    },
    methods:{
        addTask(){
            this.errors = [];
           $('#add_task_model').modal("show");
        },
        createTask(){
            axios.post('/task', {
                name: this.task.name,
                description: this.task.description,
            })
            .then(response => {
                this.reset();
                this.tasks.push(response.data.task);
                $('#add_task_model').modal("hide");
            })
            .catch(error => {
                this.errors = [];
                if(error.response.data.errors.name){
                    this.errors.push(error.response.data.errors.name[0]);
                }
                if(error.response.data.errors.description){
                     this.errors.push(error.response.data.errors.description[0]);
                }
            });
        },
        reset(){
            this.task.name ='';
            this.task.description ='';
        },
        readTasks(){
            axios.get('/task')
            .then(response => {
                this.tasks = response.data.tasks;
            })
        },
         iniUpdate(index){
            this.errors = [];
           $("#update_task_model").modal("show");
            this.update_task = this.tasks[index];
        },
         updateTask(){
            axios.patch('/task/' + this.update_task.id, {
                name: this.update_task.name,
                description: this.update_task.description,
            })
            .then(response => {

                $("#update_task_model").modal("hide");
                alert(response.data.message);
            })
            .catch(error => {
                this.errors = [];
                if(error.response.data.errors.name){
                    this.errors.push(error.response.data.errors.name[0]);
                }
                if(error.response.data.errors.description){
                     this.errors.push(error.response.data.errors.description[0]);
                }
            });
        },
        deleteTask(index){
            let conf = confirm("Quieres eliminar esta tarea");
            if (conf === true) {
                axios.delete('/task/' + this.tasks[index].id)
                .then(response => {
                    this.tasks.splice(index, 1);
                    alert(response.data.message);
                })
                .catch(error => {

                });
            }
        },
    },
}
</script>


