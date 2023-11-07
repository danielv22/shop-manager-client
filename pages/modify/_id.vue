<template>
  <div>
    <JcLoader :load="load"></JcLoader>

    <AdminTemplate>
      <div slot="body">
        <div class="row justify-content-center">
          <div class="col-sm-8 col-12">
            <div class="card">
              <div class="card-header">
                <h3>Actualizar</h3>
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
                    <button class="btn btn-dark w-100" @click="save()">Actualizar</button>
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
import JcLoader from "../../components/JcLoader.vue";
import AdminTemplate from "../../components/AdminTemplate.vue";

export default {
  name: 'IndexPage',
  components: { AdminTemplate, JcLoader },
  head (){
    return{
      title: 'Demo'
    }
  },
  data () {
    return {
      load: true,
      model: {
        name: ''
      }
    }
  },
  methods: {
    async getData (path) {
      return await this.$api.$get(path)
    },
    async save () {
      this.load = true
      try {
        const res = await this.$api.$put(`brands/${this.$route.params.id}`, this.model)
        console.log(res)
        this.$swal.fire({
          title: "Actualizado!",
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
        await Promise.all ([
          this.getData(`brands/${this.$route.params.id}`)
        ]).then((v) => {
          this.model = v[0]
        })
      } catch (error) {
        console.log(e)
      } finally {
        this.load = false
      }
    })
  }
}
</script>
