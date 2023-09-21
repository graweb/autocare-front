<template>
  <v-list nav>
    <v-list-item to="/" router exact>
      <v-list-item-action>
        <v-icon>mdi-home</v-icon>
      </v-list-item-action>
      <v-list-item-content>
        <v-list-item-title>Início</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
    <v-list-item to="/veiculos" router exact v-if="this.$auth.user.permissions.includes('read vehicles')">
      <v-list-item-action>
        <v-icon>mdi-car</v-icon>
      </v-list-item-action>
      <v-list-item-content>
        <v-list-item-title>Veículos</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
    <v-list-item to="/manutencao" router exact v-if="this.$auth.user.permissions.includes('read maintenance')">
      <v-list-item-action>
        <v-icon>mdi-cogs</v-icon>
      </v-list-item-action>
      <v-list-item-content>
        <v-list-item-title>Manutenção</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
    <v-list-item to="/oficinas" router exact v-if="this.$auth.user.permissions.includes('read workshops')">
      <v-list-item-action>
        <v-icon>mdi-tow-truck</v-icon>
      </v-list-item-action>
      <v-list-item-content>
        <v-list-item-title>Oficinas</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
    <v-list-item to="/despesas" router exact v-if="this.$auth.user.permissions.includes('read expenses')">
      <v-list-item-action>
        <v-icon>mdi-cash-multiple</v-icon>
      </v-list-item-action>
      <v-list-item-content>
        <v-list-item-title>Despesas</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
    <v-list-item to="/usuarios" router exact v-if="this.$auth.user.permissions.includes('read users')">
      <v-list-item-action>
        <v-icon>mdi-account-multiple</v-icon>
      </v-list-item-action>
      <v-list-item-content>
        <v-list-item-title>Usuários</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
    <v-list-item to="/perfis" router exact v-if="this.$auth.user.permissions.includes('read roles')">
      <v-list-item-action>
        <v-icon>mdi-sitemap</v-icon>
      </v-list-item-action>
      <v-list-item-content>
        <v-list-item-title>Perfis</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
    <v-list-item to="/permissoes" router exact v-if="this.$auth.user.permissions.includes('read permissions')">
      <v-list-item-action>
        <v-icon>mdi-checkbox-marked-outline</v-icon>
      </v-list-item-action>
      <v-list-item-content>
        <v-list-item-title>Permissões</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
  </v-list>
</template>
