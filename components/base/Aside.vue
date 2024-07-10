<template>
  <aside
    class="sidenav navbar navbar-vertical navbar-expand-xs border-0 border-radius-xl my-3 fixed-start ms-3"
    id="sidenav-main"
  >
    <div
      class="collapse navbar-collapse w-auto h-auto"
      id="sidenav-collapse-main"
    >
      <ul class="navbar-nav">
        <li class="nav-item mt-3">
          <h6
            class="ps-4 ms-2 text-uppercase text-xs font-weight-bolder opacity-6"
          >
            AGBC
          </h6>
        </li>

        <li class="nav-item">
          <a
            @click="handleItemClick('admin')"
            data-bs-toggle="collapse"
            href="#perfil-admin"
            class="nav-link"
            aria-controls="configuracion-admin"
            role="button"
            aria-expanded="false"
          >
            <div
              class="icon icon-shape icon-sm shadow border-radius-md bg-white text-center d-flex align-items-center justify-content-center me-2"
            >
              <i v-if="role === 'admin'" class="fas fa-user blue-icon"></i>
            </div>
            <span class="nav-link-text ms-1">Admin</span>
          </a>
          <div class="collapse" id="perfil-admin">
            <ul class="nav ms-4 ps-3">
              <li class="nav-item">
                <nuxtLink class="nav-link" to="/admin">
                  <span class="sidenav-mini-icon"> Dashboard </span>
                  <span class="sidenav-normal"> Dashboard </span>
                </nuxtLink>
              </li>
              <li class="nav-item">
                <nuxtLink class="nav-link" to="/admin/usuarios/usuario">
                  <span class="sidenav-mini-icon"> U </span>
                  <span class="sidenav-normal"> Usuario </span>
                </nuxtLink>
              </li>
                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/categorias/categoria"
                   >
                     <span class="sidenav-mini-icon"> categorias </span>
                     <span class="sidenav-normal"> categorias </span>
                   </nuxtLink>
                 </li>

                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/secciones/seccione"
                   >
                     <span class="sidenav-mini-icon"> secciones </span>
                     <span class="sidenav-normal"> secciones </span>
                   </nuxtLink>
                 </li>

                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/clientes/cliente"
                   >
                     <span class="sidenav-mini-icon"> clientes </span>
                     <span class="sidenav-normal"> clientes </span>
                   </nuxtLink>
                 </li>
                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/cajeros/cajero"
                   >
                     <span class="sidenav-mini-icon"> Ventanilla </span>
                     <span class="sidenav-normal"> Ventanilla </span>
                   </nuxtLink>
                 </li>

                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/casillas/casilla"
                   >
                     <span class="sidenav-mini-icon"> Crear casillas </span>
                     <span class="sidenav-normal">Crear  casillas </span>
                   </nuxtLink>
                 </li>

                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/casillas2/casilla2"
                   >
                     <span class="sidenav-mini-icon"> casillas </span>
                     <span class="sidenav-normal"> casillas </span>
                   </nuxtLink>
                 </li>

                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/alquileres/alquilere"
                   >
                     <span class="sidenav-mini-icon"> alquileres </span>
                     <span class="sidenav-normal"> alquileres </span>
                   </nuxtLink>
                 </li>

                

                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/precios/precio"
                   >
                     <span class="sidenav-mini-icon"> precios </span>
                     <span class="sidenav-normal"> precios </span>
                   </nuxtLink>
                 </li>
                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/alquileres2d/alquilere2d"
                   >
                     <span class="sidenav-mini-icon"> Mapa 2D </span>
                     <span class="sidenav-normal"> Mapa 2D </span>
                   </nuxtLink>
                 </li>
                 <li class="nav-item">
                <nuxtLink class="nav-link" to="/admin/reservas/reserva">
                  <span class="sidenav-mini-icon"> Reservar </span>
                  <span class="sidenav-normal"> Reservar </span>
                </nuxtLink>
              </li>
              <li class="nav-item">
                <nuxtLink class="nav-link" to="/admin/reservascajero">
                  <span class="sidenav-mini-icon"> Reservas Vigentes </span>
                  <span class="sidenav-normal"> Reservas Vigentes </span>
                </nuxtLink>
              </li>
               </ul>
             </div>
             
           </li>
           

           <li class="nav-item">
          <a
            @click="handleItemClick('cajero')"
            data-bs-toggle="collapse"
            href="#perfil-cajero"
            class="nav-link"
            aria-controls="configuracion-cajero"
            role="button"
            aria-expanded="false"
          >
            <div
              class="icon icon-shape icon-sm shadow border-radius-md bg-white text-center d-flex align-items-center justify-content-center me-2"
            >
              <i v-if="role === 'cajero'" class="fas fa-money-bill blue-icon"></i>
            </div>
            <span class="nav-link-text ms-1">Alquiler</span>
          </a>
          <div class="collapse" id="perfil-cajero">
            <ul class="nav ms-4 ps-3">
              <li class="nav-item">
                <nuxtLink class="nav-link" to="/admin/clientes/cliente">
                  <span class="sidenav-mini-icon"> clientes </span>
                  <span class="sidenav-normal"> clientes </span>
                </nuxtLink>
              </li>

                 

                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/casillas2/casilla2"
                   >
                     <span class="sidenav-mini-icon"> Casillas </span>
                     <span class="sidenav-normal"> Casillas </span>
                   </nuxtLink>
                 </li>

                 <li class="nav-item">
                   <nuxtLink
                     class="nav-link"
                     to="/admin/alquileres/alquilere"
                   >
                     <span class="sidenav-mini-icon"> alquileres </span>
                     <span class="sidenav-normal"> alquileres </span>
                   </nuxtLink>
                 </li>

                

                 
               
              
               </ul>
             </div>
             
           </li>

         </ul>
       </div>
 
     </aside>
 </template>
 <style scoped>
 .blue-icon {
   color: blue; /* Set the desired color */
 }
 </style>
 
 <script>
 export default {
   data() {
     return {
       role: '', // Initialize the role data property
     };
   },
   methods: {
     handleItemClick(role) {
       this.role = role; // Update the role property when a menu item is clicked
       console.log(`Clicked on ${role}`);
     },
   },
 };
 </script>
 