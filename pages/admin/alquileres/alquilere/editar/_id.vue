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
                      <label for="">Observacion</label>
                      <input type="text" v-model="model.nombre" class="form-control" id="">
                    </div>


                    <div class="form-group col-12">
                      <label for="">Cliente</label>
                      <select name="" id="" class="form-control" v-model="model.cliente_id">
                        <option v-for="m in clientes" :value="m.id">{{ m.nombre }}</option>
                      </select>
                    </div>


                    <div class="form-group col-12">
                      <label for="">Casilla</label>
                      <select name="" id="" class="form-control" v-model="model.casilla_id">
                        <option v-for="m in casillas" :value="m.id">{{ m.nombre }}</option>
                      </select>
                    </div>



                    <div class="form-group col-12">
                      <label for="">Tamaño</label>
                      <select name="" id="" class="form-control" v-model="model.categoria_id">
                        <option v-for="m in categorias" :value="m.id">{{ m.nombre }}</option>
                      </select>
                    </div>

                    <div class="form-group col-12">
                      <label for="">Tamaño</label>
                      <select name="" id="" class="form-control" v-model="model.precio_id">
                        <option v-for="m in precios" :value="m.id">{{ m.tiempo }}</option>
                      </select>
                    </div>

                    
                    <div class="form-group col-12">
                      <label for="">Fecha INICIAL</label>
                      <input type="date" v-model="model.ini_fecha" class="form-control">
                    </div>

                    <div class="form-group col-12">
                      <label for="">Fecha TERMINO</label>
                      <input type="date" v-model="model.fin_fecha" class="form-control">
                    </div>

                   

                    <!-- <div class="form-group col-12">
                      <label for="">Estado de Casilla</label>
                      <select v-model="model.casilla_estado" class="form-control">
                        <option value="Activo">Activo</option>
                        <option value="Mantenimiento">Mantenimiento</option>
                      </select>
                    </div>


                    <div class="form-group col-12">
                      <label for="">Estado</label>
                      <select v-model="model.estado" class="form-control">
                        <option value="1">Activo</option>
                        <option value="0">Mantenimiento</option>
                      </select>
                    </div> -->









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
      title: "alquileres",
    };
    ;
  },
  data() {
    return {
      load: true,

      model: {
        nombre: '',
        ini_fecha: '',
        fin_fecha: '',
        cliente_id: '',
        casilla_id: '',
        casilla_estado: '',
        categoria_id: '',
        precio_id: '',
        estado: ''

      },
      apiUrl: "alquileres",
      page: "alquileres",
      modulo: "Agbc",
      clientes: [],
      casillas: [],
      categorias: [],
      precios: [],


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
        await Promise.all([this.GET_DATA(this.apiUrl + "/" + this.$route.params.id), this.GET_DATA('clientes'), this.GET_DATA('casillas'), this.GET_DATA('categorias'), this.GET_DATA('precios'),]).then((v) => {
          this.model = v[0];
          this.clientes = v[1];
          this.casillas = v[2];
          this.categorias = v[3];
          this.precios = v[4];


          // if (this.clientes.length) {
          //   this.model.cliente_id = this.clientes[0].id
          // }
          // if (this.casillas.length) {
          //   this.model.casilla_id = this.casillas[0].id
          // }
          // if (this.categorias.length) {
          //   this.model.categoria_id = this.categorias[0].id
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