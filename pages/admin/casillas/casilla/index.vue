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
                    <th class="py-0 px-1">NOMBRE</th>
                    <th class="py-0 px-1">Categoria</th>
                    <th class="py-0 px-1">Seccion</th>
                    <th class="py-0 px-1">estado</th>
                    <th class="py-0 px-1">Ubicacion</th>
                    <th class="py-0 px-1"></th>
                  </thead>
                  <tbody>
                    <tr v-for="(m, i) in paginatedList" :key="m.id">
                      <td class="py-0 px-1">{{ i + 1 }}</td>
                      <td class="py-0 px-1">{{ m.nombre }}</td>
                      <td class="py-0 px-1">{{ m.categoria.nombre }}</td>
                      <td class="py-0 px-1">{{ m.seccione.nombre }}</td>


                      <td class="py-0 px-1"
                        :class="m.estado === 1 ? 'Libre' : (m.estado === 2 ? 'Lleno' : (m.estado === 3 ? 'Mantenimiento' : 'Ocupado'))">
                        {{ m.estado === 1 ? 'Libre' : (m.estado === 2 ? 'Lleno' : (m.estado === 3 ? 'Mantenimiento' : 'Ocupado')) }}
                      </td>
                      <td class="py-0 px-1">{{ m.ubicacion }}</td>

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
          
          <!-- Botones de paginación -->
          <div class="pagination justify-content-between mt-3">
                  <button
                    @click="currentPage--"
                    :disabled="currentPage === 1"
                    class="btn btn-sm btn-transparent"
                    style="border-radius: 0;"
                  >
                    Anterior
                  </button>
                  <span class="align-self-center">
                    Página {{ currentPage }} de {{ Math.ceil(list.length / pageSize) }}
                    <span v-for="page in displayedPages" :key="page">
                      <button
                        v-if="page === currentPage || page === currentPage + 1 || page === currentPage + 2 || page === currentPage + 3 || page === currentPage + 4"
                        @click="changePage(page)"
                        :class="{ 'btn-primary': page === currentPage, 'btn-transparent': page !== currentPage }"
                        class="btn btn-sm"
                        style="border-radius: 0;"
                      >
                        {{ page }}
                      </button>
                    </span>
                  </span>
                  <button
                    @click="currentPage++"
                    :disabled="currentPage * pageSize >= list.length"
                    class="btn btn-sm btn-transparent"
                    style="border-radius: 0;"
                  >
                    Siguiente
                  </button>
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
  },

  data() {
    return {
      load: true,
      list: [],
      apiUrl: 'casillas',
      page: 'casillas',
      modulo: 'agbc',
      url_nuevo: '/admin/casillas/casilla/nuevo',
      url_editar: '/admin/casillas/casilla/editar/',
      currentPage: 1, // Página actual
      pageSize: 10,
    itemsPerPage: 10, // Elementos por página
    totalPages: 0, // Variable para almacenar el número total de páginas
    };
  },
  computed: {

    displayedPages() {
      const pageCount = Math.ceil(this.list.length / this.pageSize);
      return Array.from({ length: pageCount }, (_, i) => i + 1);
    },
    
  paginatedList() {
    const startIndex = (this.currentPage - 1) * this.itemsPerPage;
    const endIndex = startIndex + this.itemsPerPage;
    return this.list.slice(startIndex, endIndex);
  },
},

  methods: {
    changePage(pageNum) {
      this.currentPage = pageNum;
    },
    async GET_DATA(path) {
      const res = await this.$api.$get(path);
      return res;
    },
    async EliminarItem(id) {
      this.load = true;
      try {
        const res = await this.$api.$delete(this.apiUrl + '/' + id);
        console.log(res);
        await Promise.all([this.GET_DATA(this.apiUrl)]).then((v) => {
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
      this.$swal.fire({
        title: 'Deseas Eliminar?',
        showDenyButton: false,
        showCancelButton: true,
        confirmButtonText: 'Eliminar',
        cancelButtonText: 'Cancelar',
      }).then(async (result) => {
        if (result.isConfirmed) {
          await self.EliminarItem(id);
        }
      });
    }
  },
  mounted() {
    this.$nextTick(async () => {
      try {
        await Promise.all([this.GET_DATA(this.apiUrl)]).then((v) => {
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
<style scoped>
/* Estilos para los botones de paginación */
.btn-transparent {
  background-color: transparent;
  color: #000000; /* Cambia el color del texto según tus preferencias */
  border-color: transparent; /* Opcional: elimina el borde si lo deseas */
}

/* Estilos para el botón activo */
.btn-primary {
  background-color: #153e6a;
  color: #fff;
}
</style>
