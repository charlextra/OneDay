<template>
  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('secteuractivites')}}</f7-nav-center>
    </f7-navbar>
    <f7-block>
      <div class="content-block-title">{{$lang('secteuractivites')}}</div>
      <form class="list-block inputs-list">
      <ul>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('secteuractivite')}}</div>
                <div class="item-input">
                  <input v-validate="'required|alphanumeric'" :class="{'input': true, 'is-danger': errors.has('secteuractivite') }" name="libsecteur" type="text" placeholder="$lang('secteuractivite')"/>
                </div>
                <span v-show="errors.has('secteuractivite')" class="text-danger">{{ errors.first('secteuractivite') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('description')}}</div>
                <div class="item-input">
                  <input v-validate="'required|alphanumeric'" :class="{'input': true, 'is-danger': errors.has('description') }" name="description" type="text" placeholder="$lang('description')"/>
                </div>
                <span v-show="errors.has('description')" class="text-danger">{{ errors.first('description') }}</span>
              </p>
            </div>
          </div>
        </li>
      </ul>
      <f7-button small fill raised bg="orange" internal>Enregistrer</f7-button>  </f7-block>
      </form>
  </f7-page>
</template>
