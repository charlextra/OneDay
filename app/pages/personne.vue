<template>
  <f7-page>
    <f7-navbar v-if="$theme.material">
      <f7-nav-left>
        <f7-link icon="icon-bars" open-panel="left"></f7-link>
      </f7-nav-left>
      <f7-nav-center sliding>{{$lang('personnes')}}</f7-nav-center>
    </f7-navbar>
    <f7-block>
      <div class="content-block-title">{{$lang('enregistrerpersonne')}}</div>
      <form class="list-block inputs-list">
      <ul>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('nom')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('nom') }" v-model="personnes.nom" name="nom" :data-vv-as="$lang('v_nom')" type="text" :placeholder="$lang('nom')"/>
                </div>
                <span v-show="errors.has('nom')" class="danger">{{ errors.first('nom') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('prenoms')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('prenoms') }" v-model="personnes.prenoms" name="prenoms" :data-vv-as="$lang('v_prenoms')" type="text" :placeholder="$lang('prenoms')"/>
                </div>
                <span v-show="errors.has('prenoms')" class="danger">{{ errors.first('prenoms') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <a href="#" class="item-link smart-select">
            <select name="fruits">
              <option value="H" data-option-icon="fa fa-male">{{this.$lang('masculin')}}</option>
              <option value="F" data-option-icon="fa fa-female">{{this.$lang('feminin')}}</option>
            </select>
            <div class="item-content">
              <div class="item-inner">
                <div class="item-title">{{this.$lang('sexe')}}</div>
              </div>
            </div>
          </a>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('datenais')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('datenais') }" v-model="personnes.datenais" name="datenais" data-vv-as="La datenais" type="date" :placeholder="$lang('datenais')"/>
                </div>
                <span v-show="errors.has('datenais')" class="danger">{{ errors.first('datenais') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('email')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('email') }" v-model="personnes.email" name="email" :data-vv-as="$lang('v_email')" type="email" :placeholder="$lang('email')"/>
                </div>
                <span v-show="errors.has('email')" class="danger">{{ errors.first('email') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('tel1')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('tel1') }" v-model="personnes.tel1" name="tel1" :data-vv-as="$lang('v_tel1')" type="number" :placeholder="$lang('tel1')"/>
                </div>
                <span v-show="errors.has('tel1')" class="danger">{{ errors.first('tel1') }}</span>
              </p>
            </div>
          </div>
        </li>
        <li>
          <div class="item-content">
            <div class="item-inner">
              <p :class="{ 'control': true }">
                <div class="item-title label">{{$lang('tel2')}}</div>
                <div class="item-input">
                  <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('tel2') }" v-model="personnes.tel2" name="tel2" :data-vv-as="$lang('v_tel2')" type="number" :placeholder="$lang('tel2')"/>
                </div>
                <span v-show="errors.has('tel2')" class="danger">{{ errors.first('tel2') }}</span>
              </p>
            </div>
          </div>
        </li>
      </ul>
      <div v-if="this.personnes.edit==''">
        <f7-button small fill raised bg="orange"  @click.prevent="validateBeforeSubmit" internal>{{$lang('sauvegarder')}}</f7-button>
        <!--<f7-button route-tab-link="#tab-1" href="/tabs/">Button 1</f7-button>-->
      </div>
      <div v-else>
        <f7-button small fill raised bg="green"  @click.prevent="edit()" internal>{{$lang('sauvegarder')}}</f7-button>
        <br />
        <f7-button small fill raised bg="red"  @click.prevent="annuler()" internal>{{$lang('terminer')}}</f7-button>
      <!--<f7-button @click="$db('personnes.email', null)">Remove data</f7-button>-->
      <!--<f7-button small fill raised bg="red" @click="errors.clear" internal>Effacer</f7-button>-->
    </div>
    </form>
  </f7-block>
    <f7-block>
      <f7-list media-list>
      <f7-list-item swipeout v-for="(value, key) in this.$db('personnes')"  :title="value.nom" :subtitle="value.tel1" :text="value.email" @swipeout:deleted="supprimer(key)">
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
        personnes: {
            nom: '',
            prenoms: '',
            datenais: '',
            email: '',
            tel1:'',
            tel2:'',
            edit:''
        },
        message: {
          error: this.$lang('error'),
          e_ajout: this.$lang('e_ajout'),
          e_modification: this.$lang('e_modification'),
          success: this.$lang('success'),
          s_ajout: this.$lang('s_ajout'),
          s_mofification: this.$lang('s_modification')
        }
      }
    },
  methods: {
    validateBeforeSubmit() {
      this.$validator.validateAll().then((result) => {
        if (result) {
          // eslint-disable-next-line
          //this.$db('personnes', this.personnes);
          this.$fireDB('personnes').push(this.personnes);
          this.$f7.alert(this.message.s_ajout, this.message.success);

          return;
        }
        this.$f7.alert(this.message.e_ajout, this.message.error);
      });
    },
    supprimer(key){
      this.$fireDB('personnes').child(key).remove();
    },
    SetEdit(key){
      this.$fireDB('personnes').child(key).update({edit: key});
      this.personnes = JSON.parse(JSON.stringify(this.$db('personnes.'+key)));
    },
    annuler(key){
      this.$fireDB('personnes').child(this.personnes.edit).update({edit: ''});
      this.personnes.edit = '';
      this.$f7.getCurrentView().router.back({pageName:'accueil', force: true})
    },
    edit(){
      this.$fireDB('personnes').child(this.personnes.edit).update(this.personnes);
      this.$fireDB('personnes').child(this.personnes.edit).update({edit: ''});
      this.personnes.edit = '';
      this.$f7.alert(this.message.s_modification, this.message.success);
    }

  },
  created() {
    this.$fireDB('personnes').on('value', (snap) => {
    this.$db('personnes', snap.val())
  })
  },


};
</script>
