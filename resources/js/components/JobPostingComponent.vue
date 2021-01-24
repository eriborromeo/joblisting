<template>
    <div>
        <br>
        <h1>Jobs</h1><br>

        <!--TABLE-->
        <table class="table table-hover">
            <thead>
                <th>Job Name</th>
                <th>Job Description</th>
                <th>Vacants</th>
                <th>Action</th>
            </thead>

            <tbody>
                <tr v-for="(item, index) in postingList" :key="item.id">
                    <td>{{ item.job_name }}</td>
                    <td>{{ item.job_description }}</td>
                    <td>{{ item.vacants }}</td>
                    <td>
                        <button class="btn btn-secondary btn-sm" @click="edit(item)"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil-square" viewBox="0 0 16 16">
                        <path d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456l-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"/>
                        <path fill-rule="evenodd" d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z"/>
                        </svg></button>
                        <button class="btn btn-danger btn-sm" @click="remove(item,index)" ><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash-fill" viewBox="0 0 16 16">
                        <path d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z"/>
                        </svg></button>
                    </td>
                </tr>
            </tbody>
               
            <tfoot>
                 <tr>
                    <td>
                        <div class="form-group">
                            <input type="text" class="form-control" placeholder="Input Job Name" v-model="form.job_name">
                        </div>
                    </td>
                    <td>
                        <div class="form-group">
                            <input type="text" class="form-control" placeholder="Input Job Description" v-model="form.job_description">
                        </div>
                    </td>
                    <td>
                        <div class="form-group">
                            <input type="number" class="form-control" placeholder="Input Vacants" v-model="form.vacants">
                        </div>
                    </td>
                    <td>
                        <div class="form-group">
                            <button class="btn btn-primary btn-sm" @click="submit"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-plus-square-fill" viewBox="0 0 16 16">
                            <path d="M2 0a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V2a2 2 0 0 0-2-2H2zm6.5 4.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3a.5.5 0 0 1 1 0z"/>
                            </svg></button>
                        </div> 
                    </td>
                </tr>
            </tfoot>
        </table>
        
        <!-- MODAL -->
        <div class="modal fade" id="editModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLongTitle">Edit Job Post</h5>
                    </div>
                    <div class="modal-body">
                        <div class="form-group">
                            <label for="">Job Name</label>
                            <input type="text" class="form-control" placeholder="Input Job Name" v-model="formEdit.job_name">
                        </div>
                        <div class="form-group">
                            <label for="">Job Description</label>
                            <input type="text" class="form-control" placeholder="Input Job Description" v-model="formEdit.job_description">
                        </div>
                        <div class="form-group">
                            <label for="">Vacants</label>
                            <input type="number" class="form-control" placeholder="Input Vacant" v-model="formEdit.vacants">
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="close()">Close</button>
                        <button type="button" class="btn btn-primary" @click="save()">Save changes</button>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>


<script>
export default {
    props: ['posting'],
    data(){
        return{        
            postingList: this.posting,
            form: {
                job_name:null,
                job_description:null,
                vacants:0
            },
            formEdit: {
                job_name:null,
                job_description:null,
                vacants:0
            },
            selectedId: null
        }
    },
    methods: {
        submit(){
            const vm = this;
            axios.post('/job_posting',this.form)
            .then(function (response) {
                vm.postingList.push(response.data.data);
                vm.form.job_name = null;
                vm.form.job_description = null;
                vm.form.vacants = 0;
            })
            .catch(error => {
                console.log(error.response.data);
            });

        },
        remove(item,index){
            const vm = this;
            axios.delete(`/job_posting/${item.id}`)
            .then(function (response) {
                vm.postingList.splice(index,1);               
            })
            .catch(error => {
                console.log(error.response.data);
            });

        },
        edit(item){
            
            this.formEdit.job_name =  item.job_name;            
            this.formEdit.job_description =  item.job_description;
            this.formEdit.vacants =  item.vacants;
            this.selectedId = item.id;
            $('#editModal').modal('show');

        },
        save(){
            const vm = this;
            axios.put(`/job_posting/${vm.selectedId}`,this.formEdit)
            .then(function (response) {                
                location.reload();
            })
            .catch(error => {
                console.log(error.response.data);
            });

        }
    }
}
</script>