<template>
  <v-layout align-center justify-center column fill-height>
    <div
      v-for="(i,key) in mainList"
      :key="key"
      class="text-xl-center font-weight-black display-4"
      style="    margin-bottom: 1.5rem;
    margin-top: 1.5rem; "
    >
      <span class="display-2">{{i.name}}:</span>
      {{i.diffDays}}
      <span class="display-3">DNI</span>
    </div>
  </v-layout>
</template>

<script>
export default {
  components: {},
  data: () => ({
    ilosc: 6
  }),
  computed: {
    mainList() {
      debugger
      if (localStorage && localStorage.listOf) {
        var rList = JSON.parse(localStorage.listOf)
        for (var i of rList) {
          debugger
          var today = new Date()
          let rrrr = today.getFullYear() //i.date.split('-')[0]
          let mm = i.date.split('-')[1]
          let dd = i.date.split('-')[2]

          i.dataObj = new Date(rrrr, mm, dd)
          var oneDay = 24 * 60 * 60 * 1000 // hours*minutes*seconds*milliseconds
          i.diffDays = Math.round(
            Math.abs((i.dataObj.getTime() - today.getTime()) / oneDay)
          )
        }
        return rList.sort((a, b) => {
          if (a.diffDays > b.diffDays) return 1
          if (a.diffDays < b.diffDays) return -1
          if (a.diffDays == b.diffDays) return 0
        })
      }
      return null
    }
  }
}
</script>

<style>
</style>
