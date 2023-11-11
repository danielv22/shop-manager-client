<template>
  <div>
    <JcLoader :load="load"></JcLoader>
    <AdminTemplate :page="page" :modulo="modulo">
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
                      <label for="">Codigo Barras</label>
                      <input
                        type="text"
                        name=""
                        v-model="model.barcode"
                        class="form-control"
                        id=""
                      />
                    </div>
                    <div class="form-group col-6">
                      <label for="">Marca</label>
                     <select name="" id="" class="form-control" v-model="model.brand_id">
                      <option v-for="m in brands" :value="m.id">{{m.name}}</option>
                     </select>
                    </div>

                    <div class="form-group col-6">
                      <label for="">Categoria</label>
                     <select name="" id="" class="form-control" v-model="model.category_id">
                      <option v-for="m in categories" :value="m.id">{{m.name}}</option>
                     </select>
                    </div>
                    <div class="form-group col-6">
                      <label for="">Unidad Medida</label>
                     <select name="" id="" class="form-control" v-model="model.measurement_id">
                      <option v-for="m in measurements" :value="m.id">{{m.name}} - {{m.code}}</option>
                     </select>
                    </div>
                    <div class="form-group col-6">
                      <label for="">Stock minimo</label>
                      <input
                        type="text"
                        name=""
                        v-model.number="model.minimum_stock"
                        class="form-control"
                        id=""
                      />
                    </div>
                    <div class="form-group col-6">
                      <label for="">Precio Compra</label>
                      <input
                        type="text"
                        name=""
                        v-model.number="model.purchase_price"
                        class="form-control"
                        id=""
                      />
                    </div>
                    <div class="form-group col-6">
                      <label for="">Precio Venta</label>
                      <input
                        type="text"
                        name=""
                        v-model.number="model.sale_price"
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
  name: "IndexPage",
  head() {
    return {
      title: this.modulo
    };
  },
  data() {
    return {
      model: {
        name: "",
        barcode: "",
        minimun_stock:0,
        purchase_price:0,
        sale_price:0,
        brand_id:'',
        measurement_id:'',
        category_id:'',
      },
      apiUrl: "products",
      page: "Configuracion",
      modulo: "Productos",
      load:true,
      brands:[],
      measurements:[],
      categories:[],
    };
  },
  methods: {
    async getData(path) {
      const res = await this.$api.$get(path);
      return res;
    },

  },
  mounted() {
    this.$nextTick(async () => {
      try {
        await Promise.all([this.getData('brands'),this.getData('measurements'),this.getData('categories')]).then((v) => {
          this.brands = v[0];
          this.measurements = v[1];
          this.categories = v[2];
          if(this.brands.length){
            this.model.brand_id = this.brand[0].id
          }
          if(this.measurments.length){
            this.model.measurement_id = this.measurements[0].id
          }
          if(this.categories.length){
            this.model.category_id = this.categories[0].id
          }
        });
      } catch (e) {
        console.log(e);
      } finally {
        this.load=false
      }
    });
  },
};
</script>
