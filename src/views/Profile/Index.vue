<template>
  <div>
    <BaseLoading v-if="isLoading"/>
    <template v-if="profileData !== null">
      <MainBlock :profile-data="profileData"/>
      <ArtisansBlock :artisans-data="artisansData" />
    </template>
  </div>
</template>

<script>
import MainBlock from './MainBlock/Index'
import BaseLoading from '@/components/BaseLoading'
import setError from '@/mixins/setError'
import { getApiAccount } from '@/api/search'
import ArtisansBlock from './ArtisansBlock/Index'

export default {
  name: 'ProfileView',
  // Lo damos de alta
  mixins: [
    setError
  ],
  components: { BaseLoading, MainBlock, ArtisansBlock },
  data () {
    return {
      isLoading: false,
      profileData: null
    }
  },
  computed: {
    artisansData () {
      return {
        blacksmith: this.profileData.blacksmith,
        blacksmithHardcore: this.profileData.blacksmithHardcore,
        jeweler: this.profileData.jeweler,
        jewelerHardcore: this.profileData.jewelerHardcore,
        mystic: this.profileData.mystic,
        mysticHardcore: this.profileData.mysticHardcore
      }
    }
  },
  created () {
    // llamada a la API
    this.isLoading = true
    const { region, battleTag: account } = this.$route.params
    this.fetchData(region, account)
  },
  methods: {
    fetchData (region, account) {
      getApiAccount({ region, account })
        .then(({ data }) => {
          // Guardamos los datos en una variable local
          this.profileData = data
        })
        .catch((err) => {
          this.profileData = null
          // Creamos el objeto error
          const errObj = {
            routeParams: this.$route.params,
            message: err.message
          }
          if (err.response) {
            errObj.data = err.response.data
            errObj.status = err.response.status
          }
          // Hacemos uso del Mixin
          // Guardamos el objeto en el Store
          this.setApiErr(errObj)
          // Navegamos a la ruta de nombre 'Error'
          this.$router.push({ name: 'Error' })
        })
        .finally(() => {
          this.isLoading = false
        })
    }
  }
}

</script>
