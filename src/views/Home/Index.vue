<template>
  <div class="home">
    <HomeTitle />

    <MainForm />
  </div>
</template>

<script>
import MainForm from './HomeForm'
import HomeTitle from './HomeTitle'
// @ is an alias to /src
export default {
  name: 'Home',
  components: {
    HomeTitle,
    MainForm
  }
}
</script>
