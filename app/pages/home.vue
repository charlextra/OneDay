<template>

  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('accueil')}}</f7-nav-center>
    </f7-navbar>
    <!--<f7-block inner inset>{{$lang('text')}}</f7-block>-->
    <f7-block>
      <div class="content-block-title">{{$lang('accueil')}}</div>
      <form class="list-block inputs-list">
        <ul>
          <li>
            <div class="item-content">
              <div class="item-inner">
                <p :class="{ 'control': true }">
                <div class="item-title label">E-mail</div>
                      <div class="item-input">
                        <input v-validate="'required|email'" :class="{'input': true, 'is-danger': errors.has('email') }" name="email" type="text" placeholder="Email"/>
                      </div>
                      <span v-show="errors.has('email')" class="text-danger">{{ errors.first('email') }}</span>
                  </p>
              </div>
            </div>
          </li>
        </ul>
        <f7-button small fill raised bg="orange" internal>Enregistrer</f7-button>
      </form>
    </f7-block>
<!--    <f7-toolbar tabbar bottom>
      <f7-link icon="bars" tab-link="#tab1"></f7-link>
      <f7-link icon="bars"  tab-link="#tab1"></f7-link>
      <f7-link icon="calendar"  tab-link="#tab1"></f7-link>
    </f7-toolbar> -->
  </f7-page>
</template>
