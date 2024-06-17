<template>
  <div>
    <JcLoader :load="load"></JcLoader>
    <AdminTemplate :page="page" :modulo="modulo">
      <div slot="body">
        <div class="row justify-content-end">
          <div class="col-2">
            <nuxtLink :to="url_nuevo" class="btn btn-dark btn-sm w-100">
              <i class="fas fa-plus"></i> Agregar
            </nuxtLink>
          </div>
          <div class="col-12">
            <div class="card">
              <div class="card-body">
                <table class="table">
                  <thead>
                    <th class="py-0 px-1">#</th>
                    <th class="py-0 px-1">Observacion</th>
                    <th class="py-0 px-1">Cliente</th>
                    <th class="py-0 px-1">Casilla</th>
                    <th class="py-0 px-1">Carnet</th>
                    <th class="py-0 px-1">SECCION</th>
                    <th class="py-0 px-1">Precio</th>
                    <th class="py-0 px-1">Tamaño</th>
                    <th class="py-0 px-1">Alquiler</th>
                    <th class="py-0 px-1">Tiempo Inicio</th>
                    <th class="py-0 px-1">Timepo Fin</th>
                    <!-- <th class="py-0 px-1">Estado</th> -->
                    <th class="py-0 px-1"></th>
                  </thead>
                  <tbody>
                    <tr v-for="(m, i) in list">
                      <td class="py-0 px-1">{{ i + 1 }}</td>
                      <td class="py-0 px-1">{{ m.nombre }}</td>
                      <td class="py-0 px-1">{{ m.cliente.nombre }}</td>
                      <td class="py-0 px-1">{{ m.casilla.nombre }}</td>
                      <td class="py-0 px-1">{{ m.cliente.carnet }}</td>
                      <td class="py-0 px-1">{{ m.casilla.seccione_id }}</td>
                      <td class="py-0 px-1">{{ m.precio.precio }}</td>
                      <td class="py-0 px-1">{{ m.categoria.nombre }}</td>
                      <td class="py-0 px-1"
                        :class="m.casilla.estado === 1 ? 'Libre' : (m.casilla.estado === 2 ? 'Lleno' : 'Ocupado')">
                        {{ m.casilla.estado === 1 ? 'Libre' : (m.casilla.estado === 2 ? 'Lleno' : 'Ocupado') }}
                      </td>
                      <td class="py-0 px-1">{{ m.ini_fecha }}</td>
                      <td class="py-0 px-1">{{ m.fin_fecha }}</td>

                      <!-- <td class="py-0 px-1" :class="m.estado === 1 ? 'activo' : 'Mantenimiento'">
                        {{ m.estado === 1 ? 'Activo' : 'Mantenimiento' }}
                      </td> -->
                      <td class="py-0 px-1">
                        <div class="btn-group">
                          <nuxtLink :to="url_editar + m.id" class="btn btn-info btn-sm py-1 px-2">
                            <i class="fas fa-pen"></i>
                          </nuxtLink>
                          <button type="button" @click="Eliminar(m.id)" class="btn btn-danger btn-sm py-1 px-2">
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
  name: "IndexPage",
  head() {
    return {
      title: this.modulo,
    };
    ;
  },

  data() {
    return {
      load: true,
      list: [],
      apiUrl: 'alquileres',
      page: 'alquileres',
      modulo: 'agbc',
      url_nuevo: '/alquileres/alquilere/nuevo',
      url_editar: '/alquileres/alquilere/editar/',
      casillasOcupadas: [], // Agregamos la propiedad casillasOcupadas
    };
  },
  methods: {
    async GET_DATA(path) {
      const res = await this.$api.$get(path);
      return res
    },
    async EliminarItem(id) {
      this.load = true
      try {
        const res = await this.$api.$delete(this.apiUrl + '/' + id);
        console.log(res)
        await Promise.all([this.GET_DATA(this.apiUrl)]).then((v) => {
          this.list = v[0]
          this.casillasOcupadas = this.list.map((item) => item.casilla.nombre); // Actualizamos casillasOcupadas
        })
      } catch (e) {
        console.log(e);
      } finally {
        this.load = false
      }
    },
    Eliminar(id) {
      let self = this
      this.$swal.fire({
        title: 'Deseas Eliminar?',
        showDenyButton: false,
        showCancelButton: true,
        confirmButtonText: 'Eliminar',
        cancelButtonText: 'Cancelar', // Agrega esta línea para definir el texto del botón Cancelar
      }).then(async (result) => {
        /* Read more about isConfirmed, isDenied below */
        if (result.isConfirmed) {
          await self.EliminarItem(id)
        }
      })
    }
  },
  mounted() {
    this.$nextTick(async () => {
      try {
        await Promise.all([this.GET_DATA(this.apiUrl)]).then((v) => {
          this.list = v[0]
          this.casillasOcupadas = this.list.map((item) => item.casilla.nombre); // Inicializamos casillasOcupadas
        })
      } catch (e) {
        console.log(e);
      } finally {
        this.load = false
      }

    });
  },
};
</script>
