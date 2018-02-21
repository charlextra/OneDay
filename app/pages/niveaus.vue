<template>
  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('niveaux')}}</f7-nav-center>
    </f7-navbar>
    <f7-block>
      <div class="content-block-title">{{$lang('enregistrerniveau')}}</div>
      <form class="list-block inputs-list">
      <ul>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('niveaux')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('libniv') }" v-model="niveaus.libniv" name="libniv" data-vv-as="Le libellé du niveau" type="text" :placeholder="$lang('niveaux')"/>
                </div>
                <span v-show="errors.has('libniv')" class="danger">{{ errors.first('libniv') }}</span>
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
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('description') }" v-model="niveaus.description" name="description" data-vv-as="La description" type="text" :placeholder="$lang('description')"/>
                </div>
                <span v-show="errors.has('description')" class="danger">{{ errors.first('description') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('note')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('note') }" v-model="niveaus.note" name="note" data-vv-as="La note" type="number" :placeholder="$lang('note')"/>
                </div>
                <span v-show="errors.has('note')" class="danger">{{ errors.first('note') }}</span>
              </p>
            </div>
          </div>
        </li>
      </ul>
      <div v-if="this.niveaus.edit==''">
        <f7-button small fill raised bg="orange"  @click.prevent="validateBeforeSubmit" internal>{{$lang('sauvegarder')}}</f7-button>
        <!--<f7-button route-tab-link="#tab-1" href="/tabs/">Button 1</f7-button>-->
      </div>
      <div v-else>
        <f7-button small fill raised bg="green"  @click.prevent="edit()" internal>{{$lang('sauvegarder')}}</f7-button>
        <br />
        <f7-button small fill raised bg="red"  @click.prevent="annuler()" internal>{{$lang('terminer')}}</f7-button>
      <!--<f7-button @click="$db('niveaus.description', null)">Remove data</f7-button>-->
      <!--<f7-button small fill raised bg="red" @click="errors.clear" internal>Effacer</f7-button>-->
    </div>
    </form>
  </f7-block>
    <f7-block>
      <f7-list media-list>
      <f7-list-item swipeout v-for="(value, key) in this.$db('niveaus')"  :title="value.libniv" :subtitle="value.note" :text="value.description" @swipeout:deleted="supprimer(key)">
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
        niveaus: {
            libniv: '',
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
          //this.$db('niveaus', this.niveaus);
          this.$fireDB('niveaus').push(this.niveaus);
          this.$f7.alert('enrégistrement effectué','Succès');
          return;
        }
        this.$f7.alert('Les informations du formulaire sont incorrects','Erreur');

      });
    },
    supprimer(key){
      this.$fireDB('niveaus').child(key).remove();
    },
    SetEdit(key){
      this.$fireDB('niveaus').child(key).update({edit: key});
      this.niveaus = JSON.parse(JSON.stringify(this.$db('niveaus.'+key)));
    },
    annuler(key){
      this.$fireDB('niveaus').child(this.niveaus.edit).update({edit: ''});
      this.niveaus.edit = '';
      this.$f7.getCurrentView().router.back({pageName:'accueil', force: true})
    },
    edit(){
      this.$fireDB('niveaus').child(this.niveaus.edit).update(this.niveaus);
      this.$fireDB('niveaus').child(this.niveaus.edit).update({edit: ''});
      this.niveaus.edit = '';
      this.$f7.alert('Modification réussie','Succès');
    }

  },
  created() {
    this.$fireDB('niveaus').on('value', (snap) => {
    this.$db('niveaus', snap.val())
  })
  },


};
</script>
