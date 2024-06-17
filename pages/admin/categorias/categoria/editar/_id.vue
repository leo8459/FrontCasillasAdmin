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
                      <label for="">Nombre Categoria Casilla</label>
                      <input type="text" name="" v-model="model.nombre" class="form-control" id="" />
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
      title: "categorias",
    };
    ;
  },
  data() {
    return {
      load: true,

      model: {
        nombre:'',
        estado:'',

      },
      apiUrl: "categorias",
      page: "categorias",
      modulo: "Agbc",
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
        await Promise.all([this.GET_DATA(this.apiUrl + '/' + this.$route.params.id)]).then((v) => {
          this.model = v[0]
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