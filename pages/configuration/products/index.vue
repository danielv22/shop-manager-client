<template>
  <div>
    <JcLoader :load="load"></JcLoader>
    <AdminTemplate :page="page" :modulo="modulo">
      <div slot="body">
        <div class="row justify-content-end">
          <div class="col-2">
            <nuxtLink :to="url_new" class="btn btn-dark btn-sm w-100">
              <i class="fas fa-plus"></i> Agregar
            </nuxtLink>
          </div>
          <div class="col-12">
            <div class="card" style="overflow: auto;">
              <div class="card-body">
                <table class="table">
                  <thead>
                    <th class="py-0 px-1">#</th>
                    <th class="py-0 px-1">NOMBRE</th>
                    <th class="py-0 px-1">CODIGO BARRAS</th>
                    <th class="py-0 px-1">MARCA</th>
                    <th class="py-0 px-1">CATEGORIA</th>
                    <th class="py-0 px-1">STOCK MIN</th>
                    <th class="py-0 px-1">P. COMPRA</th>
                    <th class="py-0 px-1">P. VENTA</th>
                    <th class="py-0 px-1"></th>
                  </thead>
                  <tbody>
                    <tr v-for="(m, i) in list">
                      <td class="py-0 px-1">{{ i + 1 }}</td>
                      <td class="py-0 px-1">{{ m.name}}</td>
                      <td class="py-0 px-1">{{ m.barcode }}</td>
                      <td class="py-0 px-1">{{ m.brand.name}}</td>
                      <td class="py-0 px-1">{{ m.category.name }}</td>
                      <td class="py-0 px-1">{{m.minimum_stock}} {{ m.measurement.code }} </td>
                      <td class="py-0 px-1">{{ m.purchase_price }}</td>
                      <td class="py-0 px-1">{{ m.sale_price }}</td>
                      <td class="py-0 px-1">
                        <div class="btn-group">
                          <nuxtLink
                            :to="url_modify + m.id"
                            class="btn btn-info btn-sm py-1 px-2"
                          >
                            <i class="fas fa-pen"></i>
                          </nuxtLink>
                          <nuxtLink
                            :to="url_image + m.id"
                            class="btn btn-success btn-sm py-1 px-2"
                          >
                            <i class="fas fa-image"></i>
                          </nuxtLink>
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
      apiUrl: "products",
      page: "Configuracion",
      modulo: "Productos",
      url_new: "/configuration/products/new",
      url_modify: "/configuration/products/modify/",
      url_image: "/configuration/products/image/",
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
