<template>
  <div>
    <JcLoader :load="load"></JcLoader>
    <AdminTemplate :page="page" :modulo="modulo">
      <div slot="body">
        <div class="col-2">
          <label for="seccionSelector">Selecciona una sección:</label>
          <select v-model="seccionSeleccionada" id="seccionSelector" @change="cargarDatos">
            <option value="1">Sección 1</option>
            <option value="2">Sección 2</option>
            <option value="3">Sección 3</option>
            <option value="4">Sección 4</option>
            <!-- Agrega más opciones según tus necesidades -->
          </select>
        </div>
        <div class="d-flex justify-content-center align-items-center">
          <div class="row mt-4">
            <div class="d-flex flex-wrap justify-content-center">
              <div v-for="(item, index) in casillasOrdenadas" :key="item.id" class="m-2" :style="{
                fontSize: '2rem',
                width: 'calc(175px - 10px)',
                transform: getIconSize(item.categoria.nombre),
              }">
                <div :class="['circle-icon', getIconColorClass(item.estado)]">
                  <i :class="getIconClass(item.categoria.nombre)" @click="abrirModal(item)"></i>
                </div>
                <div class="text-center">
                  <p class="casilla-nombre">{{ item.nombre }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </AdminTemplate>

    <!-- Modal personalizado -->
    <div v-if="modalVisible" class="modal fade show" style="display: block; background: rgba(0, 0, 0, 0.5);">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Detalles de la Casilla</h5>
            <button type="button" class="close" @click="cerrarModal">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <p>Numero de Casilla: {{ casillaSeleccionada.nombre }}</p>
            <p>Categoría: {{ casillaSeleccionada.categoria.nombre }}</p>
            <p>Estado: {{ getTextForEstado(casillaSeleccionada.estado) }}</p>
          </div>
          <div class="modal-footer">
            <nuxtLink :to="`${url_nuevo}?casillaId=${casillaSeleccionada.id}`" class="btn btn-dark btn-sm w-30">
              <i class="fas fa-plus"></i>Alquilar
            </nuxtLink>
            <nuxtLink :to="url_editar + casillaSeleccionada.id" class="btn btn-info btn-sm py-2 px-4">
              Editar
            </nuxtLink>
            <button type="button" class="btn btn-secondary" @click="cerrarModal">Cerrar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      load: true,
      casillas: [],
      apiUrl: 'ver1', // La ruta base
      page: 'Casillas',
      modulo: 'agbc',
      seccionSeleccionada: "1", // Valor por defecto
      url_nuevo: '/alquileres/alquilere/nuevo',
      url_editar: '/casillas/casilla/estado/',
      modalVisible: false,
      casillaSeleccionada: {},
    };
  },
  computed: {
    casillasOrdenadas() {
      // Ordenar las casillas por el nombre de la categoría en orden inverso
      return this.casillas.slice().sort((a, b) => b.categoria.nombre.localeCompare(a.categoria.nombre));
    },
  },
  methods: {
    async cargarDatos() {
      try {
        const res = await this.$api.$get(`${this.apiUrl}/${this.seccionSeleccionada}`);
        console.log('Datos recuperados de la API:', res);
        this.casillas = res.casillas;

        // Agrega la casilla "Lleno" a las casillas recuperadas

      } catch (error) {
        console.error('Error al recuperar los datos de la API:', error);
      } finally {
        this.load = false;
      }
    },
    abrirModal(item) {
      this.casillaSeleccionada = item;
      this.modalVisible = true;
    },
    cerrarModal() {
      this.modalVisible = false;
    },
    getIconColorClass(estado) {
      switch (estado) {
        case 1:
          return 'text-success'; // Estado "Disponible"
        case 2:
          return 'text-brown'; // Estado "Lleno" (nuevo color café)
          case 3:
          return 'text-danger'; // Estado "Lleno" (nuevo color café)
        default:
          return 'text-black'; // Otros estados
      }
    },
    getIconSize(categoria) {
      switch (categoria) {
        case 'Pequeño':
          return 'scale(0.6)';
        case 'Mediana':
          return 'scale(1)';
        case 'Gabeta':
          return 'scale(1.4)';
        case 'Cajon':
          return 'scale(1.6)';
        default:
          return 'scale(1)';
      }
    },
    getIconClass(categoria) {
      switch (categoria) {
        case 'Pequeño':
          return 'fas fa-box';
        case 'Mediana':
          return "fa fa-th-large";
        case 'Gabeta':
          return 'fa fa-archive';
        case 'Cajon':
          return 'fas fa-box-open';
        default:
          return 'fas fa-box';
      }
    },
    getTextForEstado(estado) {
      switch (estado) {
        case 0:
          return 'Ocupado';
        case 1:
          return 'Libre';
        case 2:
          return 'Lleno';
          case 3:
          return 'Mantenimiento';
        default:
          return 'Desconocido';
      }
    },
  },
  mounted() {
    this.cargarDatos();
  },
};
</script>

<style scoped>
.label-container {
  position: absolute;
  top: 10px;
  /* Ajusta la posición vertical según tus necesidades */
  right: 10px;
  /* Ajusta la posición horizontal según tus necesidades */
  background-color: #f0f0f0;
  /* Ajusta el color de fondo según tus preferencias */
  padding: 5px;
  /* Ajusta el espaciado interior según tus preferencias */
  border: 1px solid #ccc;
  /* Ajusta los bordes según tus preferencias */
  border-radius: 5px;
  /* Añade bordes redondeados si lo deseas */
}

.label-text {
  font-size: 14px;
  /* Ajusta el tamaño de fuente según tus preferencias */
}

.circle-icon {
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  font-size: 2rem;
  margin: 0 auto;
  transform-origin: center;
  transition: transform 0.2s;
}

.text-black {
  color: black;
}

.text-brown {
  color: rgb(255, 128, 0);
}

p {
  font-size: 1rem;
}

.casilla-nombre {
  font-size: 1rem;
}</style>
