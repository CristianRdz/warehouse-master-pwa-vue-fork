<template>
  <div>
    <header class="w-full">
      <v-tabs fixed-tabs dark class="bg-deepRedAccent animate__animated animate__fadeInLeft">
        <v-tab v-for="item in items" :key="item.title" :to="item.to" class="tab-item">
          <v-icon>{{ item.icon }}</v-icon>
          {{ item.title }}
        </v-tab>
      </v-tabs>
    </header>
    <router-view></router-view>
  </div>
</template>

<script>
import { ref, watch, onMounted } from "vue";
import { useAuthStore } from "@/stores";

export default {
  setup() {
    const authStore = useAuthStore();
    const items = ref([]);

    const updateMenuItems = () => {
      items.value = [
        { title: "Inicio", icon: "mdi-home", to: "/home/inicio" },
        { title: "Productos", icon: "mdi-package-variant-closed", to: "/home/servicios" },
        { title: "Carrito", icon: "mdi-cart", to: "/home/carrito" },
      ];

      if (authStore.user) {
        items.value.push({ title: "Perfil", icon: "mdi-account", to: "/home/perfil" });
        items.value.push({ title: "Compras", icon: "mdi-currency-usd", to: "/home/compras"});
      } else {
        items.value.push(
          { title: "Iniciar sesión", icon: "mdi-login", to: "/home/login" },
          { title: "Registrarse", icon: "mdi-account-plus", to: "/home/registro" }
        );
      }
    };

    watch(
      () => authStore.user,
      () => updateMenuItems(),
      { immediate: true }
    );

    onMounted(() => updateMenuItems());

    return { items };
  },
};
</script>

<style scoped>


.v-toolbar {
  color: white;
}

.v-btn {
  color: white;
}

.v-icon {
  color: white;
}

/* Cambiar el color de las pestañas cuando se pasa el ratón o se selecciona */
.tab-item {
  transition: background-color 0.3s ease;
}

.tab-item:hover,
.v-tab--active {
  background-color: #BB5E7D; /* Rosa cuando se pasa el ratón o está activo */
}
</style>
