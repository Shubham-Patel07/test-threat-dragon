<template>
  <b-navbar toggleable="lg" fixed="top" id="navbar" :class="themeClass">
    <b-navbar-brand :to="username ? '/dashboard' : '/'" class="td-brand">
      <b-img src="@/assets/threatdragon_logo_image.svg" class="td-brand-img" alt="Threat Dragon Logo" />
      Threat Dragon v{{ packageBuildVersion }}{{ packageBuildState }}
    </b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item>
          <td-locale-select />
        </b-nav-item>
        <b-nav-item>
          <theme-toggle-button />
        </b-nav-item>
      </b-navbar-nav>

      <b-navbar-nav class="ml-auto">
        <b-nav-text v-show="username" class="logged-in-as">{{ $t('nav.loggedInAs') }} {{ username }}</b-nav-text>
        <b-nav-item v-show="username" @click="onLogOut" id="nav-sign-out">
          <font-awesome-icon icon="sign-out-alt" class="td-fa-nav" v-b-tooltip.hover
            :title="$t('nav.logOut')"></font-awesome-icon>
        </b-nav-item>
        <b-nav-item href="https://owasp.org/www-project-threat-dragon/docs-2/" target="_blank" rel="noopener noreferrer"
          id="nav-docs">
          <font-awesome-icon icon="question-circle" class="td-fa-nav" v-b-tooltip.hover
            :title="$t('desktop.help.docs')"></font-awesome-icon>
        </b-nav-item>
        <b-nav-item href="https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html" target="_blank"
          rel="noopener noreferrer" id="nav-tm-cheat-sheet">
          <font-awesome-icon icon="gift" class="td-fa-nav" v-b-tooltip.hover
            :title="$t('desktop.help.sheets')"></font-awesome-icon>
        </b-nav-item>
        <b-nav-item href="https://owasp.org/www-project-threat-dragon/" target="_blank" rel="noopener noreferrer"
          id="nav-owasp-td">
          <b-img src="@/assets/owasp.svg" class="td-fa-nav td-owasp-logo" :title="$t('desktop.help.visit')" />
        </b-nav-item>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

<script>

import { LOGOUT } from '@/store/actions/auth.js';
import TdLocaleSelect from './LocaleSelect.vue';
import ThemeToggleButton from './ToggleTheme.vue';

export default {
    name: 'TdNavbar',
    components: {
        TdLocaleSelect,
        ThemeToggleButton,
    },
    methods: {
        onLogOut(evt) {
            evt.preventDefault();
            this.$store.dispatch(LOGOUT);
            this.$router.push('/').catch(error => {
                if (error.name != 'NavigationDuplicated') {
                    throw error;
                }
            });
        },
    },
};
</script>

<style lang="scss" scoped>
$icon-height: 1.2rem;
.navbar {
  background-color: $orange;
  border-color: $orange-alt;
  height: 60px; // Adjusted for example
  font-size: 15px;
}

.nav-link,
.logged-in-as {
  color: $white !important;
}

.logged-in-as {
  margin-right: 10px;
}

.td-fa-nav {
  font-size: $icon-height;
  max-height: $icon-height;
  margin: 0 5px 0 5px;
}
.navbar {
  background-color: $orange;
  border-color: $orange-alt;
  height: 60px;
  font-size: 15px;
}

.nav-link, .logged-in-as {
  color: $white !important;
}

.td-fa-nav {
  font-size: $icon-height;
  max-height: $icon-height;
  margin: 0 5px 0 5px;
}

.td-brand {
  color: $white !important;
  .td-brand-img {
    max-height: 50px;
  }
}

  .nav-link,
  .logged-in-as,
  .td-brand {
    &.dark {
      color: $dark-text !important;
    }
  }

  .td-fa-nav {
    &.dark {
      color: $light-text !important;
    }
  }


@media (max-width: 576px) {
  .td-owasp-logo {
    background-color: red; // Consider adjusting for dark mode
    border-radius: 50%;
    padding: 5px;
  }
}
</style>
