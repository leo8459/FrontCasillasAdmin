<template>
  <div>
    <JcLoader :load="load"></JcLoader>
    <AdminTemplate :page="page" :modulo="modulo">
      <div slot="body">
        <div class="row justify-content-center">

          <div class="col-sm-8 col-12">
            <div class="card">
              <div class="card-header">
                <h3>Actualizar</h3>
              </div>
              <div class="card-body">
                <CrudUpdate :model="model" :apiUrl="apiUrl">
                  <div slot="body" class="row">

                    <div class="form-group col-12">
                      <label for="">Tiempo</label>
                      <input type="text" v-model="model.tiempo" class="form-control" id="">
                    </div>


                    <div class="form-group col-12">
                      <label for="">Precio</label>
                      <input type="text" v-model="model.precio" class="form-control" id="">
                    </div>



                    <div class="form-group col-12">
                      <label for="">Tamaño</label>
                      <select name="" id="" class="form-control" v-model="model.categoria_id">
                        <option v-for="m in categorias" :value="m.id">{{ m.nombre }}</option>
                      </select>
                    </div>

                    

                    <div class="form-group col-6">
                      <label for="">Estado de la cuenta</label>
                      <select v-model="model.estado" class="form-control">
                        <option value="1">Activo</option>
                        <option value="0">Mantenimiento</option>
                      </select>
                    </div>





                  </div>
                </CrudUpdate>
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
      title: "precios",
    };
    ;
  },
  data() {
    return {
      load: true,

      model: {
        tiempo: '',
        precio: '',
        categoria_id: '',
        estado: '',

      },
      apiUrl: "precios",
      page: "precios",
      modulo: "Agbc",
      categorias: [],
    };
  },
  methods: {
    async GET_DATA(path) {
      const res = await this.$api.$get(path);
      return res
    },

  },
  mounted() {
    this.$nextTick(async () => {

      try {
        await Promise.all([this.GET_DATA(this.apiUrl + '/' + this.$route.params.id),this.GET_DATA('categorias')]).then((v) => {
          this.model = v[0];
          this.categorias = v[1];
          // if (this.categorias.length) {
          //   this.model.categoria_id = this.categorias[0].id
          // }
          // if (this.secciones.length) {
          //   this.model.seccione_id = this.secciones[0].id
          // }
        })
      } catch (e) {
        console.log(e);
      } finally {
        this.load = false
      }



    });
  }
};
</script>