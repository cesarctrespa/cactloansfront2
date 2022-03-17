<script>
  import SidebarLink from './SidebarLink'
  import { collapsed, toggleSidebar, sidebarWidth } from './state'

  export default {
    props: {},
    components: { SidebarLink },
    setup() {
      return { collapsed, toggleSidebar, sidebarWidth }
    }
  }
</script>

<template>
  <div class="sidebar" :style="{ width: sidebarWidth }">
    <h1 class="mt-3">
      <img v-if="collapsed" src="../../assets/logo2.png" class="logo-collapsed">
      <img v-else src="../../assets/logo1.png" class="logo-expanded">
    </h1>

    <SidebarLink class="mb-2" to="/" icon="fas fa-calculator">Simulación</SidebarLink>
    <SidebarLink class="mb-2" to="/payment-plan" icon="fas fa-chart-bar">Plan</SidebarLink>
    <SidebarLink class="mb-2" to="/rates" icon="fas fa-percentage">Tasas</SidebarLink>

    <SidebarLink class="mb-2" to="/loans" icon="fas fa-landmark">Mis Préstamos</SidebarLink>
    <SidebarLink class="mb-2" to="/receipt" icon="fas fa-receipt">Mis Facturas</SidebarLink>
    <SidebarLink class="mb-2" to="/invoice" icon="fas fa-file-invoice-dollar">Mis Recibos</SidebarLink>
    <span v-if="collapsed" class="mt-auto mb-2">
      <h5 data-bs-toggle="tooltip" data-bs-placement="right" title="Cesar Cadavid">CC</h5>
      <div class="user">
        <SidebarLink class="icon-user" to="/user" icon="fas fa-user">Usuario</SidebarLink>    
        <SidebarLink class="icon-log-in" to="/user" icon="fas fa-sign-in-alt" data-bs-toggle="tooltip" data-bs-placement="right" title="Iniciar sesión">Usuario</SidebarLink>    
    </div>
    </span>
    <span v-else class="user mt-auto mb-2">
      <SidebarLink class="icon-user" to="/user" icon="fas fa-user-check">Cesar Cadavid</SidebarLink>    
      <SidebarLink class="icon-log-in" to="/user" icon="fas fa-sign-in-alt">Iniciar sesión</SidebarLink>    
    </span>
    <SidebarLink class="mb-5" to="/settings" icon="fas fa-wrench">Ajustes</SidebarLink>

    <span
      class="collapse-icon mb-2"
      :class="{ 'rotate-180': collapsed }"
      @click="toggleSidebar"
    >
      <i class="fas fa-angle-double-left" />
    </span>
  </div>
</template>

<style>
  :root {
    --sidebar-bg-color: #4381A7;
    --sidebar-item-hover: #80B3BA;
    --sidebar-item-active: #29536d;
  }
</style>

<style scoped lang="scss">
  .user:hover .icon-user,
  .user .icon-log-in {
    transition: 1s ease;
    display: none;
  }
  .user:hover .icon-log-in {
    transition: 1s ease;
    display: flex;
  }

  .logo-expanded{
    height: 40px;
    float: left;
  }

  .logo-collapsed{
    height: 40px;
    float: left;
  }

  .sidebar {
    color: white;
    background-color: var(--sidebar-bg-color);
    float: left;
    position: fixed;
    z-index: 2;
    top: 0;
    left: 0;
    bottom: 0;
    padding: 0.5em;
    transition: 0.3s ease;
    display: flex;
    flex-direction: column;
    white-space: nowrap;
    overflow: hidden;
  }

  .sidebar h1 {
    height: 2.5em;
  }

  .collapse-icon {
    position: absolute;
    bottom: 0;
    padding: 0.75em;
    color: rgba(255, 255, 255, 0.7);
    transition: 0.2s linear;
  }

  .rotate-180 {
    transform: rotate(180deg);
    transition: 0.2s linear;
  }
</style>
