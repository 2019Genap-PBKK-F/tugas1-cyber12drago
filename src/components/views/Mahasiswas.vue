<template>
  <div>
    <div id="app" ref="spreadsheet"></div>
    <div><input type="button" class="btn btn-primary" value="Insert Row" @click="() => spreadsheet.insertRow()" /></div>
  </div>
</template>

<script>
    import jexcel from 'jexcel'
    import 'jexcel/dist/jexcel.css'
    var data = [
      ['05111740000097', 'dsasad', '2017', '12/12/1999', '', true]
    ]
    var options = {
      data: data,
      allowToolbar: true,
      columns: [
        { type: 'numeric', title: 'NRP', width: '200px' },
        { type: 'text', title: 'Nama', width: '200px' },
        { type: 'numeric', title: 'Angkatan', width: '200px' },
        { type: 'calender', title: 'Tgl Lahir', width: '200px' },
        { type: 'image', title: 'foto', width: '200px' },
        { type: 'checkbox', title: 'Aktif', width: '80px' }
      ]
    }
    export default {
      name: 'App',
      mounted: function () {
        let spreadsheet = jexcel(this.$el, options)
        Object.assign(this, { spreadsheet })
      }
    }
</script>
