<template>
  <div>
    <JcLoader :load="load"></JcLoader>

    <AdminTemplate>
      <div slot="body">
        <div class="row justify-content-center">
          <div class="col-sm-8 col-12">
            <div class="card">
              <div class="card-header">
                <h3>Agregar</h3>
              </div>
              <div class="card-body">
                <div class="row">
                  <div class="col-12">
                    <div class="form-group">
                      <label for="">Nombre</label>
                      <input type="text" class="form-control" v-model="model.name">
                    </div>
                  </div>
                  <div class="col-6">
                    <button class="btn btn-info w-100" @click="$router.back()">Regresar</button>
                  </div>
                  <div class="col-6">
                    <button class="btn btn-dark w-100" @click="save()">Guardar</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </AdminTemplate>
  </div>
</template>

<script>
import JcLoader from "../components/JcLoader.vue";
import AdminTemplate from "../components/AdminTemplate.vue";

export default {
  name: 'IndexPage',
  components: {AdminTemplate, JcLoader},
  head (){
    return{
      title: 'Demo'
    }
  },
  data () {
    return {
      load: false,
      model: {
        name: ''
      }
    }
  },
  methods: {
    async save (  ) {
      this.load = true
      try {
        const res = await this.$api.$post('brands', this.model)
        console.log(res)
        this.$swal.fire({
          title: "Guardado!",
          showDenyButton: false,
          showCancelButton: false,
          confirmButtonText: 'Ok'
        }).then((result) => {
          if (result.isConfirmed) {
            this.$router.back()
          }
        })
      } catch (e) {
        console.log(e)
      } finally {
        this.load = false
      }
    }
  },
  mounted () {
    this.$nextTick(async() => {
      try {

      } catch (error) {
        console.log(e)
      } finally {

      }
    })
  }
}
</script>
