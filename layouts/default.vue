<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" fixed app dark>
      <v-list nav>
        <v-list-item to="/">
          <v-list-item-title>
            <v-icon>mdi-plus</v-icon>
            Novo Veículo
          </v-list-item-title>
        </v-list-item>
      </v-list>

      <v-divider />

      <v-list nav>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <template v-slot:append>
        <v-divider />
        <v-menu offset-y :nudge-width="-25" transition="slide-y-transition">
          <template v-slot:activator="{ on, attrs }">
            <v-list-item link v-bind="attrs" v-on="on">
              <v-list-item-avatar>
                <v-icon dark size="36"> mdi-account-circle </v-icon>
              </v-list-item-avatar>

              <v-list-item-content>
                <v-list-item-title>{{ $auth.user.name }}</v-list-item-title>

                <v-list-item-subtitle>{{
                  $auth.user.email
                }}</v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-action>
                <v-icon color="grey lighten-1">mdi-dots-horizontal</v-icon>
              </v-list-item-action>
            </v-list-item>
          </template>

          <v-list nav>
            <v-list-item to="/">
              <v-list-item-title>
                <v-icon>mdi-cog</v-icon>
                Configurações
              </v-list-item-title>
            </v-list-item>
            <v-list-item @click="logout">
              <v-list-item-title>
                <v-icon>mdi-logout</v-icon>
                Sair
              </v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
      </template>
    </v-navigation-drawer>

    <!-- <v-app-bar fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-spacer />
      <v-btn icon @click="logout">
        <v-icon>mdi-share</v-icon>
      </v-btn>
    </v-app-bar> -->
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data() {
    return {
      drawer: true,
      items: [
        {
          icon: "mdi-home",
          title: "Início",
          to: "/",
        },
        {
          icon: "mdi-car",
          title: "Veículos",
          to: "/veiculos",
        },
        {
          icon: "mdi-cogs",
          title: "Manutenção",
          to: "/manutencao",
        },
        {
          icon: "mdi-tow-truck",
          title: "Oficinas",
          to: "/oficinas",
        },
        {
          icon: "mdi-cash-multiple",
          title: "Despesas",
          to: "/despesas",
        },
      ],
    };
  },

  methods: {
    async logout() {
      await this.$auth
        .logout()
        .then(() => {
          this.$router.push("/login");
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>
