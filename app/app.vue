<template>
  <!-- App -->
  <div id="app">
    <login-popup />

    <!-- Statusbar -->
    <f7-statusbar></f7-statusbar>

    <!-- Left Panel -->
    <f7-panel left reveal layout="dark">
      <f7-view id="left-panel-view" navbar-through :dynamic-navbar="true">
        <f7-navbar v-if="$theme.ios" title="Left Panel" sliding></f7-navbar>
        <f7-pages>
          <f7-page>
            <f7-navbar v-if="$theme.material" :title="$lang('menu')" sliding></f7-navbar>
            <!--<f7-block inner>
              <p>Left panel content goes here</p>
            </f7-block>-->
            <!--<f7-block-title>{{$lang('parametres')}}</f7-block-title>
            <f7-list>
              <f7-list-item link="/niveaus/" :title="$lang('niveaux')"></f7-list-item>
              <f7-list-item link="/secteuractivites/" :title="$lang('secteuractivites')"></f7-list-item>
            </f7-list>-->
            <f7-block-title>{{$lang('parametres')}}</f7-block-title>
            <f7-list>
              <f7-list-item link="/accueil/" :title="$lang('accueil')" link-view="#main-view" link-close-panel></f7-list-item>
              <f7-list-item link="/niveaus/" :title="$lang('niveaux')" link-view="#main-view" link-close-panel></f7-list-item>
              <f7-list-item link="/secteuractivites/" :title="$lang('secteuractivites')" link-view="#main-view" link-close-panel></f7-list-item>
              <f7-list-item link="/carte/" :title="$lang('carte')" link-view="#main-view" link-close-panel></f7-list-item>
              <f7-list-item link="/accueil/" @click="$root.$signOut()" :title="$lang('deconnexion')" link-view="#main-view" link-close-panel></f7-list-item>
            </f7-list>
          </f7-page>
        </f7-pages>
      </f7-view>
    </f7-panel>

    <!-- Main Views -->
    <f7-views>
      <f7-view id="main-view" navbar-through :dynamic-navbar="true" main>
        <!-- iOS Theme Navbar -->
        <f7-navbar v-if="$theme.ios">
          <f7-nav-left>
            <f7-link icon="icon-bars" open-panel="left"></f7-link>
          </f7-nav-left>
          <f7-nav-center sliding>{{$root.config.title}}</f7-nav-center>
        </f7-navbar>
        <!-- Pages -->
        <f7-pages>
          <f7-page>
            <!-- Material Theme Navbar -->
            <f7-navbar v-if="$theme.material">
              <f7-nav-left>
                <f7-link icon="icon-bars" open-panel="left"></f7-link>
              </f7-nav-left>
              <f7-nav-center sliding>{{$root.config.title}}</f7-nav-center>
            </f7-navbar>
            <!-- Page Content -->
            <f7-block-title>{{$lang('bonjour')}}</f7-block-title>
            <f7-block inner>
              <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
            </f7-block>
            <!--<f7-block-title>Navigation</f7-block-title>
            <f7-list>
              <f7-list-item link="/about/" title="About"></f7-list-item>
              <f7-list-item link="/form/" title="Form"></f7-list-item>
              <f7-list-item link="/dynamic-route/blog/45/post/125/?foo=bar#about" title="Dynamic Route"></f7-list-item>
            </f7-list>-->
          </f7-page>
        </f7-pages>
      </f7-view>
    </f7-views>
  </div>
</template>

<script>
export default {}
</script>
<script>
import Vue from 'vue';
import fr from 'vee-validate/dist/locale/fr';
import Datatable from 'vue2-datatable-component';
import VeeValidate, { Validator } from 'vee-validate';
import * as VueGoogleMaps from 'vue2-google-maps';
  Vue.use(VueGoogleMaps, {
    load: {
      key: 'AIzaSyAwXpg_XYwHGQx7uytTgXtvdR4GKm8IJYU',
      v: '3.exp',
      // libraries: 'places', //// If you need to use place input
    }
  });
//Localize takes the locale object as the second argument (optional) and merges it.
Validator.localize('en', fr);

//Install the plugin
Vue.use(VeeValidate);
Vue.use(Datatable);
const config = {
  errorBagName: 'errors', // change if property conflicts
  fieldsBagName: 'fields',
  delay: 0,
  locale: 'en',
  dictionary: null,
  strict: true,
  classes: false,
  classNames: {
    touched: 'touched', // the control has been blurred
    untouched: 'untouched', // the control hasn't been blurred
    valid: 'valid', // model is valid
    invalid: 'invalid', // model is invalid
    pristine: 'pristine', // control has not been interacted with
    dirty: 'dirty' // control has been interacted with
  },
  events: 'input|blur',
  inject: true,
  validity: false,
  aria: true,
  i18n: null, // the vue-i18n plugin instance,
  i18nRootKey: 'validations' // the nested key under which the validation messsages will be located
};

Vue.use(VeeValidate, config);
</script>
