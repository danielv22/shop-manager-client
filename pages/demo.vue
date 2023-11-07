<template>
  <div>
    <JcLoader :load="load"></JcLoader>

    <AdminTemplate>
      <div slot="body">
        <div class="row justify-content-end">
          <div class="col-2">
            <NuxtLink to="/new" class="btn btn-dark btn-sm w-100">
              <i class="fas fa-plus"></i> Agregar
            </NuxtLink>
          </div>
          <div class="col-12">
            <div class="card">
              <div class="card-body">
                <table class="table">
                  <thead>
                    <tr>
                      <th class="py-0 px-1">#</th>
                      <th class="py-0 px-1">Nombre</th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(m, i) in list">
                      <td class="py-0 px-1">{{i+1}}</td>
                      <td class="py-0 px-1">{{m.name}}</td>
                      <td class="py-0 px-1">
                        <div class="btn-group">
                          <NuxtLink
                            :to="'/modify/' + m.id"
                            class="btn btn-info btn-sm py-1 px-2"
                          >
                            <i class="fas fa-pen"></i>
                          </NuxtLink>
                          <button
                            type="button"
                            @click="removalRequest(m.id)"
                            class="btn btn-danger btn-sm py-1 px-2"
                          >
                            <i class="fas fa-trash"></i>
                          </button>
                        </div>
                      </td>
                    </tr>
                  </tbody>
                </table>
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
  components: { AdminTemplate, JcLoader },
  head () {
    return {
      title: 'Demo'
    }
  },
  data () {
    return {
      load: true,
      list: []
    }
  },
  methods: {
    async getData (path) {
      return await this.$api.$get(path)
    },
    async deleteRecord (id) {
      this.load = true
      try {
        const res = await this.$api.$delete(`brands/${id}`)
        console.log(res)
        await Promise.all([
          this.getData('brands')
        ]).then((v) => {
          this.list = v[0]
        })
      } catch (e) {
        console.log(e)
      } finally {
        this.load = false
      }
      console.log()
    },
    removalRequest (id) {
      let self = this
      this.$swal.fire({
        title: '¿Desea eliminar el registro?',
        showDenyButton: false,
        showCancelButton: true,
        confirmButtonText: 'Eliminar',
        cancelarButtonText: `Cancelar`
      }).then(async (result) => {
        if (result.isConfirmed) {
          await self.deleteRecord(id)
        }
      });
    }
  },
  mounted () {
    this.$nextTick (async() => {
      try {
        await Promise.all ([
          this.getData('brands')
        ]).then((v) => {
          this.list = v[0]
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
