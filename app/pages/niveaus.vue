<template>
  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('niveaux')}}</f7-nav-center>
    </f7-navbar>
    <f7-block inner inset>{{$lang('niveaux')}}</f7-block>
    <f7-block>
  </f7-block>
  </f7-page>
</template>
