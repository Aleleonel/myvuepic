
<template>
<div>
  <h1 class="centralizado">{{titulo}}</h1>
  <p v-show="mensagem" class="centralizado">{{ mensagem }}</p>
  
  <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="buscar">
  
  <ul class="lista-fotos">
    <li class="lista-fotos-item" v-for="foto in fotosComFiltro" :key="foto.pk">

      <meu-painel :titulo="foto.titulo">

        <imagem-responsiva v-meu-transform:scale.animate="1.5" :url="foto.url" :titulo="foto.titulo"/>
        <router-link :to="{name: 'altera', params: { id: foto._id } }">
          <meu-botao tipo="button" rotulo="ALTERAR"/>
        </router-link> 
        <meu-botao 
          tipo="button" 
          rotulo="REMOVER" 
          @botaoAtivado="remove(foto)"
          :confirmacao="false"
          estilo="perigo"/>

      </meu-painel>

    </li>
  </ul>

</div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/painel/imagem-responsiva/ImagemResponsiva.vue';
import Botao from '../shared/botao/Botao.vue';
import Foto from '../../domain/foto/foto';
import FotoService from '../../domain/foto/FotoService';


export default {

  components: {
    'meu-painel': Painel,
    'imagem-responsiva': ImagemResponsiva,
    'meu-botao': Botao
  },

  data() {

    return {

      titulo: "Alurapic",
      fotos:[],
      filtro:'',
      mensagen: ''
    }
  },

  computed: {
    fotosComFiltro() {
      if(this.filtro){
        let exp = new RegExp(this.filtro.trim(), 'i')
        return this.fotos.filter(foto => exp.test(foto.titulo));
      }else{
        return this.fotos
      }
    }
  },


  methods: {

    remove(foto) { 
      
      this.service.apaga(foto._id)
      .then(()=> {
            let indice = this.fotos.indexOf(foto);
            this.fotos.splice(indice, 1);
            this.mensagen = 'foto removida com sucesso'
            }, err => {
              this.mensagem = err.message;
        });
    }

  },

  created() {

    this.service = new FotoService(this.$resource);
    
    this.service
    .lista()
    .then(fotos => this.fotos = fotos, err => this.mensagen = err.message);
  }
}
</script>

<style lang="scss">
  
  .cetralizado{

    text-align: center;
  }

  .lista-fotos{
    list-style: none;    
  }

  .lista-fotos .lista-fotos-item {
    display: inline-block;
  }

  .filtro {
    display: block;
    width: 100%;
  }

</style>
