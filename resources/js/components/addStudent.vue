<template>
<nav class="navbar navbar-expand-lg bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">XYZ COLLEGE</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">HOME</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">GALLARY</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
            Option
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Faculty</a></li>
            <li><a class="dropdown-item" href="#">Courses</a></li>
            <li><hr class="dropdown-divider"></li>
            <li><a class="dropdown-item" href="#">Fees structure</a></li>
          </ul>
        
      </ul>
      <form class="d-flex" role="search">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </div>
</nav>
    <div>
    <div class="container mt-4">
    <h1 v-if="this.form.id !=''">Edit Student</h1>
    <h1 v-else>Add Student</h1>

    <form @submit.prevent="saveStudent">
       
        <div class="row">
            <div class="col-xl-3">
                <div class="form-group">
                    <label for="">First Name</label>
                    <input type="text" v-model="form.studentFName" class="form-control">
                </div>
            </div>

            <div class="col-xl-3">
                <div class="form-group">
                    <label for="">Last Name</label>
                    <input type="text" v-model="form.studentLName" class="form-control">
                </div>
            </div>

            <div class="col-xl-3">
                <div class="form-group">
                    <label for="">Email ID</label>
                    <input type="text" v-model="form.studentEmailId" class="form-control">
                </div>
            </div>

            <div class="col-xl-3">
                <div class="form-group">
                    <label for="">Contact No</label>
                    <input type="text" v-model="form.studentContactNo" class="form-control">
                </div>
            </div>

            <div class="col-xl-12">
               <button class="btn btn-warning" v-if="this.form.id !=''" type="submit">Update</button>
               <button class="btn btn-primary" v-else type="submit">Save</button>
               <button class="btn btn-danger"  type="button" @click="resetForm()">Reset</button>
            </div>
        </div>
    </form>

     </div>
    </div>
   
</template>

<script>
    
    export default {

        data(){
            return {
              form:
              {
                id               : '',
                studentFName     : '',
                studentLName     : '',
                studentEmailId   : '',
                studentContactNo : ''
              },
            }
        },
        
        methods:{
            //Save data to database
            saveStudent()
            {
                if(this.form.id)
                {
                    axios.post('/api/update-student/'+this.form.id, this.form)
                         .then((response) => {
                            if(response.data['success'])
                            {
                                this.$swal({
                                    icon: "success",
                                    title: " " + response.data["msg"][0],
                                    toast: true,
                                    position: "top-end",
                                    showConfirmButton: false,
                                    timer: 4000,
                                });
                            } 
                        });
                    }else{
                        axios.post('/api/add-student/',this.form)
                             .then(response => {
                                this.$router.push({ 
                                    path:'/view-student',
                                });
                                if(response.data['success'])
                                {
                                    this.$swal({
                                        icon: "success",
                                        title: " " + response.data["msg"][0],
                                        toast: true,
                                        position: "top-end",
                                        showConfirmButton: false,
                                        timer: 4000,
                                    });
                                } 
                            })
                        }
            },

            //reset input form
            resetForm()
            {
                this.form.studentFName     = '';
                this.form.studentLName     = '';
                this.form.studentEmailId   = '';
                this.form.studentContactNo = '';
            },
            
            //Data read from database for update
            readStudent()
            {
                const currentUrl = window.location.pathname;
                axios.get('/api'+currentUrl)
                     .then(response => this.form = {
                        id                    : response.data.id,
                        studentFName          : response.data.studentFName,
                        studentLName          : response.data.studentLName,
                        studentEmailId        : response.data.studentEmailId,
                        studentContactNo      : response.data.studentContactNo
                    });
            }

        },
        created() 
        {
            this.readStudent();
        },
    }
</script> 