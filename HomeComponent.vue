<template>
    <div class="container">
        <h1>Home</h1>
        <p class="h5">Axios CRUD </p>

        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
          <div class="modal-dialog" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Edit</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                  <span aria-hidden="true">&times;</span>
                </button>
              </div>

        <!-- Modalbody -->
              <div class="modal-body">
                <table class="table">
                    <tr v-for="edit in editData">
                        <td><input type="text" class="form-control" v-model="editName" :placeholder="edit.name"></td>
                        <td><input type="text" class="form-control" v-model="editDesc" :placeholder="edit.description"></td>
                    </tr>
                </table>             
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary" @click.prevent="updateData">Save changes</button>
              </div>
            </div>
          </div>
        </div>

        <!-- table from database -->
        <div class="row">
            <div class="col-sm">
                <table class="table">
                    <tr v-for="datalist in data">
                        <td>{{datalist.name}}</td>
                        <td>{{datalist.description}}</td>
                        <td><button class="btn btn-primary" >delete</button></td>
                        <td><button class="btn btn-warning" data-toggle="modal" data-target="#exampleModal" @click.native="editForm(datalist.id)">edit</button></td>
                    </tr>
                </table>
            </div>
            <div class="col-sm">
              
        <!-- Formulier2 -->
            <div class="card-body">
                    <form @submit="formSubmit">
                    <strong>Name:</strong>
                    <input type="text" class="form-control" v-model="formName">
                    <strong>Description:</strong>
                    <textarea class="form-control" v-model="formDesc"></textarea>                    
                    <button class="btn btn-success">Submit</button>
                    </form>
                    <strong>Output:</strong>
                    <pre>
                    {{output}}
                    </pre>
                </div>


            </div>
            <div class="col-sm">
              One of three columns
            </div>
        </div>
    </div>
    
</template>

<script>
    export default{
        data() {
            return {
              data: null,
              formName: '',
              formDesc: null,
              output: '',
              editName: '',
              editDesc: '',
              editData: '',
            }
        },
        methods: {
            updateData() {
                alert('Hello, World!');
                console.log('update');
            },

            editForm(id){
                axios.get('http://localhost:8000/api/edit/' + id).then((response)=>{
                    if(response.status == 200){
                        this.editData = response.data
                    }
                })
            },

            getData(){
                axios.get('http://localhost:8000/api/show').then((response)=>{
                    if(response.status ==200){
                        this.data = response.data
                    }
                })
            },
            formSubmit(e){
                e.preventDefault();
                var currObj = this;

                axios.post('http://localhost:8000/api/post', { 
                    name: currObj.formName,
                    desc: currObj.formDesc,

                }).then(function(response){
                    currObj.output = response.data;
                    console.log(response.data);
                }).catch(function(error){
                    currObj.output = error;
                }).bind(this)
            }
        },

        created(){
            this.getData()
        }
    }
</script>
