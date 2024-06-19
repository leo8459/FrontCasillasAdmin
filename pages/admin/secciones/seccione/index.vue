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
                    <th class="py-0 px-1">estado</th>
                    <th class="py-0 px-1"></th>
                  </thead>
                  <tbody>
                    <tr v-for="(m, i) in paginatedList" :key="m.id">
                      <td class="py-0 px-1">{{ (currentPage - 1) * pageSize + i + 1 }}</td>
                      <td class="py-0 px-1">{{ m.nombre }}</td>
                      <td class="py-0 px-1" :class="m.estado === 1 ? 'activo' : 'Mantenimiento'">
                        {{ m.estado === 1 ? 'Activo' : 'Mantenimiento' }}
                      </td>
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
                <div class="pagination">
                  <button @click="prevPage" :disabled="currentPage === 1" class="btn btn-primary">
                    &laquo;
                  </button>
                  <span v-for="page in pages" :key="page" @click="goToPage(page)" :class="{ active: page === currentPage }" class="page-number">
                    {{ page }}
                  </span>
                  <button @click="nextPage" :disabled="currentPage === totalPages" class="btn btn-primary">
                    &raquo;
                  </button>
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
      apiUrl: 'secciones',
      page: 'secciones',
      modulo: 'agbc',
      url_nuevo: '/admin/secciones/seccione/nuevo',
      url_editar: '/admin/secciones/seccione/editar/',
      currentPage: 1,
      pageSize: 10,
      searchTerm: "",
      filteredList: [],
    };
  },
  computed: {
    paginatedList() {
      const start = (this.currentPage - 1) * this.pageSize;
      const end = start + this.pageSize;
      return this.filteredList.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.filteredList.length / this.pageSize);
    },
    pages() {
      const totalPages = this.totalPages;
      const currentPage = this.currentPage;
      let pages = [];

      if (totalPages <= 7) {
        for (let i = 1; i <= totalPages; i++) {
          pages.push(i);
        }
      } else {
        if (currentPage <= 4) {
          for (let i = 1; i <= 5; i++) {
            pages.push(i);
          }
          pages.push('...', totalPages);
        } else if (currentPage > totalPages - 4) {
          pages.push(1, '...');
          for (let i = totalPages - 4; i <= totalPages; i++) {
            pages.push(i);
          }
        } else {
          pages.push(1, '...');
          for (let i = currentPage - 1; i <= currentPage + 1; i++) {
            pages.push(i);
          }
          pages.push('...', totalPages);
        }
      }

      return pages;
    }
  },
  methods: {
    buscar() {
      if (this.searchTerm.trim() === "") {
        this.filteredList = this.list;
      } else {
        this.filteredList = this.list.filter((item) => {
          const searchTermLower = this.searchTerm.toLowerCase();
          return (
            (item.nombre && item.nombre.toLowerCase().includes(searchTermLower)) ||
            (typeof item.estado === 'string' && item.estado.toLowerCase().includes(searchTermLower))
          );
        });
        this.currentPage = 1; // Reset the page to 1 after search
      }
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
          this.filteredList = this.list; // Reset filtered list after deletion
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
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    goToPage(page) {
      if (page !== '...') {
        this.currentPage = page;
      }
    },
  },
  mounted() {
    this.$nextTick(async () => {
      try {
        await Promise.all([this.GET_DATA(this.apiUrl)]).then((v) => {
          this.list = v[0];
          this.filteredList = this.list;
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
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}
.pagination button {
  margin: 0 10px;
}
.pagination .page-number {
  margin: 0 5px;
  cursor: pointer;
}
.pagination .page-number.active {
  font-weight: bold;
  text-decoration: underline;
}
</style>
