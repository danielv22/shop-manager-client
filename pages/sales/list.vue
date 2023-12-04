<template>
  <div>
    <JcLoader :load="load"></JcLoader>
    <AdminTemplate :page="page" :modulo="modulo">
      <div slot="body">
        <div class="row justify-content-end">
          <div class="col-12">
            <div class="card">
              <div class="card-body">
                <table class="table">
                  <thead>
                    <th class="py-0 px-1">#</th>
                    <th class="py-0 px-1">FECHA</th>
                    <th class="py-0 px-1">CLIENTE</th>
                    <th class="py-0 px-1">TOTAL</th>
                    <th class="py-0 px-1"></th>
                  </thead>
                  <tbody>
                    <tr v-for="(m, i) in list">
                      <td class="py-0 px-1">{{ i + 1 }}</td>
                      <td class="py-0 px-1">{{ m.date}}</td>
                      <td class="py-0 px-1">{{ m.client }}</td>
                      <td class="py-0 px-1">{{ m.total}}</td>
                      <td class="py-0 px-1">
                        <div class="btn-group">
                          <nuxtLink
                            :to="url_modify + m.id"
                            class="btn btn-info btn-sm py-1 px-2"
                          >
                            <i class="fas fa-eye"></i>
                          </nuxtLink>
                          <a
                            type="button"
                            :href="m.url_pdf"
                            @click="ImprimirVenta(m)"
                            target="_blank"
                            class="btn btn-success btn-sm py-1 px-2"
                          >
                            <i class="fas fa-print"></i>
                        </a>
                          <button
                            type="button"
                            @click="Eliminar(m.id)"
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
export default {
  head() {
    return {
      title: this.modulo,
    };
  },

  data() {
    return {
      load: true,
      list: [],
      apiUrl: 'sales',
      branches: [],
      page: "Ventas",
      modulo: "Lista de Ventas",
      branch:{
    },
    url_modify: "/sales/invoice/",
  };
  },
  methods: {
    async getData(path) {
      const res = await this.$api.$get(path);
      return res;
    },
    async EliminarItem(id) {
      this.load = true;
      try {
        const res = await this.$api.$delete(this.apiUrl + "/" + id);
        console.log(res);
        await Promise.all([this.getData(this.apiUrl)]).then((v) => {
          this.list = v[0];
        });
      } catch (e) {
        console.log(e);
      } finally {
        this.load = false;
      }
    },
    Eliminar(id) {
      let self = this;
      this.$swal
        .fire({
          title: "Deseas Eliminar?",
          showDenyButton: false,
          showCancelButton: true,
          confirmButtonText: "Eliminar",
          cancelarButtonText: `Cancelar`,
        })
        .then(async (result) => {
          /* Read more about isConfirmed, isDenied below */
          if (result.isConfirmed) {
            await self.EliminarItem(id);
          }
        });
    },
    async ImprimirVenta(sale){
      let branch = this.branch
      branch.sale = sale
      const res = await this.$printer.$post(branch.url_printer+"sale",branch);
      console.log(res)
    }
  },
  mounted() {
    this.$nextTick(async () => {
      try {
        await Promise.all([this.getData(this.apiUrl)]).then((v) => {
          this.list = v[0];
        });
      } catch (e) {
        console.log(e);
      } finally {
        this.load = false;
      }
    });
  },
};
</script>
