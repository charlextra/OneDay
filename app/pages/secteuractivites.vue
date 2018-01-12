<template>
  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('secteuractivites')}}</f7-nav-center>
    </f7-navbar>
    <f7-block>
      <div class="content-block-title">{{$lang('enregistrersecteuractivite')}}</div>
      <form class="list-block inputs-list">
      <ul>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('secteuractivites')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('libsecteur') }" v-model="secteuractivites.libsecteur" name="libsecteur" data-vv-as="Le libellé du secteur" type="text" :placeholder="$lang('secteuractivites')"/>
                </div>
                <span v-show="errors.has('libsecteur')" class="danger">{{ errors.first('libsecteur') }}</span>
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
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('description') }" v-model="secteuractivites.description" name="description" data-vv-as="La description" type="text" :placeholder="$lang('description')"/>
                </div>
                <span v-show="errors.has('description')" class="danger">{{ errors.first('description') }}</span>
              </p>
            </div>
          </div>
        </li>
      </ul>
      <div v-if="this.secteuractivites.edit==''">
        <f7-button small fill raised bg="orange"  @click.prevent="validateBeforeSubmit" internal>{{$lang('enregistrer')}}</f7-button>
        <!--<f7-button route-tab-link="#tab-1" href="/tabs/">Button 1</f7-button>-->
      </div>
      <div v-else>
        <f7-button small fill raised bg="green"  @click.prevent="edit()" internal>{{$lang('enregistrer')}}</f7-button>
        <br />
        <f7-button small fill raised bg="red"  @click.prevent="annuler()" internal>{{$lang('terminer')}}</f7-button>
      <!--<f7-button @click="$db('secteuractivites.description', null)">Remove data</f7-button>-->
      <!--<f7-button small fill raised bg="red" @click="errors.clear" internal>Effacer</f7-button>-->
    </div>
    </form>
  </f7-block>
    <f7-block>
      <f7-list>
      <f7-list-item swipeout v-for="(value, key) in this.$db('secteuractivites')"  :title="value.libsecteur" @swipeout:deleted="supprimer(key)">
        <f7-swipeout-actions>
          <f7-swipeout-button close color="blue" @click="SetEdit(key)">{{$lang('editer')}}</f7-swipeout-button>
          <f7-swipeout-button v-show="value.edit==''" delete>{{$lang('supprimer')}}</f7-swipeout-button>
        </f7-swipeout-actions>
      </f7-list-item>
    </f7-list>
  </f7-block>
  </f7-page>
</template>
<script>
export default {
  data () {
      return {
        secteuractivites: {
            libsecteur: '',
            description: '',
            edit:''
        }
      }
    },
  methods: {
    validateBeforeSubmit() {
      this.$validator.validateAll().then((result) => {
        if (result) {
          // eslint-disable-next-line
          //this.$db('secteuractivites', this.secteuractivites);
          this.$fireDB('secteuractivites').push(this.secteuractivites);
          this.$f7.alert('enrégistrement effectué','Succès');
          return;
        }
        this.$f7.alert('Les informations du formulaire sont incorrects','Erreur');

      });
    },
    supprimer(key){
      this.$fireDB('secteuractivites').child(key).remove();
    },
    SetEdit(key){
      this.$fireDB('secteuractivites').child(key).update({edit: key});
      this.secteuractivites = JSON.parse(JSON.stringify(this.$db('secteuractivites.'+key)));
    },
    annuler(key){
      this.$fireDB('secteuractivites').child(this.secteuractivites.edit).update({edit: ''});
      this.secteuractivites.edit = '';
      this.$f7.getCurrentView().router.back({pageName:'accueil', force: true})
    },
    edit(){
      this.$fireDB('secteuractivites').child(this.secteuractivites.edit).update(this.secteuractivites);
      this.$fireDB('secteuractivites').child(this.secteuractivites.edit).update({edit: ''});
      this.secteuractivites.edit = '';
      this.$f7.alert('Modification réussie','Succès');
    }

  },
  created() {
    this.$fireDB('secteuractivites').on('value', (snap) => {
    this.$db('secteuractivites', snap.val())
  })
  },


};
</script>
