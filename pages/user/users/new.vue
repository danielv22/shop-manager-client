<template>
  <div>

    <AdminTemplate  :page="page" :modulo="modulo">
      <div slot="body">
        <div class="row justify-content-center">
          <div class="col-sm-8 col-12">
            <div class="card">
              <div class="card-header">
                <h3>Agregar</h3>
              </div>
              <div class="card-body">
                <CrudCreate :model="model" :apiUrl="apiUrl">
                  <div slot="body" class="row">
                    <div class="form-group col-12">
              <label for="">Nombre</label>
              <input
                type="text"
                name=""
                v-model="model.name"
                class="form-control"
                id=""
              />
            </div>
                    
                    <div class="form-group col-12">
              <label for="">Correo Electronico</label>
              <input
                type="text"
                name=""
                v-model="model.email"
                class="form-control"
                id=""
              />
            </div>
                    <div class="form-group col-12">
              <label for="">Contraseña</label>
              <input
                type="password"
                name=""
                v-model="model.password"
                class="form-control"
                id=""
              />
            </div>

                  </div>
                </CrudCreate>
              </div>
            </div>
          </div>
        </div>
      </div>
    </AdminTemplate>
  </div>
</template>

<script>
export default {
  name: "index",
  head() {
    return {
      title:this.modulo,
    };
  },
  data() {
    return {
      model: {
        name: "",
        email:'',
        password:'',
      },
      apiUrl:'users',
      page:'Usuarios',
      modulo:'Usuarios'
    };
  },
  methods: {

  },
  mounted() {
    this.$nextTick(async () => {
      try {
      } catch (e) {
        console.log(e);
      } finally {
      }
    });
  },
};
</script>
