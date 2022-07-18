<template>
    <div class="tasks_container">
        <div class="tasks_content">
            <h1>Listado de Estudiantes</h1>
            <!--ul class="tasks_list"-->
                <span v-for="estudiante in tasks" :key="estudiante.id">
                    <p><b>telefono:</b> {{ estudiante.telefono }}</p>
                    <p><b>tipo:</b>{{ estudiante.tipo }}</p>
 
                    <hr>
                </span>
            <!--/ul-->
        </div>
    </div>
  <hr>
    <div class="add_task">
      <form v-on:submit.prevent="submitForm">
        <div class="form-group">
          <label for="title">telefono</label>
          <input type="text" class="form-control" id="telefono" v-model="telefono" />
        </div>
        <div class="form-group">
          <label for="description">tipo</label>
          <input type="text" class="form-control" id="tipo" v-model="tipo" />
        </div>

        <div class="form-group">
          <button type="submit">Add Telefono</button>
        </div>
      </form>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                // tasks
                tasks: [''],
                tipo: '',

            }
        },
        methods: {
            async getData() {
                try {
                    // fetch tasks
                    const response = await this.$http.get('http://localhost:8000/api/numeros/');
                    // set the data returned as tasks
                    this.tasks = response.data;
                } catch (error) {
                    // log the error
                    console.log(error);
                }
            },
            async submitForm() {
              try {
                // Send a POST request to the API
                const response = await this.$http.post(
                  "http://localhost:8000/api/numeros/",
                  {
                    telefono: this.telefono,
                    tipo: this.tipo,
                   
                  }
                );
                // Append the returned data to the tasks array
                this.tasks.push(response.data);
                // Reset the title and description field values.
                this.telefono = "";
                this.tipo = "";
              
              } catch (error) {
        // Log the error
              console.log(error);
            }
          },
        },
        created() {
            // Fetch tasks on page load
            this.getData();
        }
    }
</script>